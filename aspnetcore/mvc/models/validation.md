---
title: Validation de modèle dans ASP.NET Core MVC
author: rachelappel
description: Découvrez plus d’informations sur la validation de modèle dans ASP.NET Core MVC.
manager: wpickett
ms.author: riande
ms.date: 12/18/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/models/validation
ms.openlocfilehash: 1ab19fad90eab9f2da58b4d62615a85d71894218
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="introduction-to-model-validation-in-aspnet-core-mvc"></a>Introduction à la validation de modèle dans ASP.NET Core MVC

Par [Rachel Appel](https://github.com/rachelappel)

## <a name="introduction-to-model-validation"></a>Introduction à la validation du modèle

Avant qu'une application stocke des données dans une base de données, elle doit valider les données. Les données doivent être validées pour détecter la présence de menaces de sécurité potentielles, pour vérifier qu'elles sont correctement formatées en termes de type et de taille, et elles doivent être conformes à vos règles. La validation est nécessaire même si elle peut être fastidieuse à implémenter et redondante. Dans MVC, la validation se produit sur le client et le serveur. 

Heureusement, .NET a abstrait la validation dans des attributs de validation. Ces attributs contiennent du code de validation, ce qui réduit la quantité de code à écrire.

[Afficher ou télécharger l’exemple à partir de GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample).

## <a name="validation-attributes"></a>Attributs de validation

Les attributs de validation sont un moyen de configurer la validation de modèle afin qu’elle soit similaire sur le plan conceptuel à la validation des champs dans les tables de base de données. Cela inclut les contraintes telles que l’affectation de types de données ou les champs obligatoires. Les autres types de validation incluent l’application de modèles aux données pour appliquer des règles métiers, telles qu’une carte de crédit, une numéro de téléphone, ou une adresse de messagerie. Les attributs de validation rendent l’application de ces exigences beaucoup plus simples et plus faciles à utiliser.

Voici un modèle `Movie` annoté d’une application qui stocke des informations sur les films et les émissions de télévision. La plupart des propriétés sont nécessaires, et plusieurs propriétés de chaîne ont des exigences de longueur. En outre, il y a une restriction de plage numérique en place pour la propriété `Price` de 0 à $999,99, ainsi qu'un attribut de validation personnalisé. 

[!code-csharp[Main](validation/sample/Movie.cs?range=6-29)]

En lisant simplement le modèle, vous affichez les règles concernant les données de cette application, rendant ainsi plus facile à maintenir le code. Voici plusieurs attributs de validation intégrées courants :

* `[CreditCard]`: Valide que la propriété a un format de carte de crédit.

* `[Compare]`: Valide les deux propriétés dans une correspondance de modèle.

* `[EmailAddress]`: Valide que la propriété a un format de courrier électronique.

* `[Phone]`: Valide que la propriété a un format de téléphone.

* `[Range]`: Valide que la valeur de propriété se situe dans la plage donnée.

* `[RegularExpression]`: Vérifie que la donnée correspond à l’expression régulière spécifiée.

* `[Required]`: Rend une propriété obligatoire.

* `[StringLength]`: Valide qu'une propriété de chaîne ne dépasse la longueur maximale spécifiée.

* `[Url]`: Valide que la propriété a un format d’URL.

MVC prend en charge tout attribut qui dérive de `ValidationAttribute` à des fins de validation. Vous trouverez des attributs de validation utiles dans l'espace de noms [System.ComponentModel.DataAnnotations](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations).

Vous aurez peut-être parfois besoin de davantage de fonctionnalités que celles fournies par les attributs prédéfinis. Dans ce cas, vous pouvez créer des attributs de validation personnalisés en dérivant de `ValidationAttribute` ou en modifiant votre modèle pour implémenter `IValidatableObject`.

## <a name="notes-on-the-use-of-the-required-attribute"></a>Remarques sur l’utilisation de l’attribut Required

Les [types valeur](/dotnet/csharp/language-reference/keywords/value-types) non Nullable (tels que `decimal`, `int`, `float` et `DateTime`) sont obligatoires par nature et n’ont pas besoin de l’attribut `Required`. L’application n’effectue aucune vérification de la validation côté serveur pour les types non nullable marqués `Required`.

La liaison de modèle MVC, qui n’est pas concernée par la validation et les attributs de validation, rejette l’envoi d’un champ de formulaire contenant une valeur manquante ou un espace blanc pour un type non nullable. En l’absence d’un attribut `BindRequired` sur la propriété cible, la liaison de données ignore les données manquantes pour les types non nullable, où le champ de formulaire est absent des données de formulaire entrantes.

L'[attribut BindRequired](/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.bindrequiredattribute) (consultez également [Personnaliser le comportement de liaison de modèle avec des attributs](xref:mvc/models/model-binding#customize-model-binding-behavior-with-attributes)) est utile pour vérifier que les données de formulaire soient complètes. Quand il est appliqué à une propriété, le système de liaison de modèle requiert une valeur pour cette propriété. Quand il est appliqué à un type, le système de liaison de modèle requiert des valeurs pour toutes les propriétés de ce type.

Lorsque vous utilisez un [type Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) (par exemple, `decimal?` ou `System.Nullable<decimal>`) et que vous le marquez `Required`, une vérification de la validation côté serveur est effectuée comme si la propriété était de type nullable standard (par exemple, un `string`).

La validation côté client requiert une valeur pour un champ de formulaire qui correspond à une propriété de modèle que vous avez marquée `Required` et pour une propriété de type non nullable que vous n’avez pas marquée `Required`. `Required` peut être utilisé pour contrôler le message d’erreur de validation côté client.

## <a name="model-state"></a>État du modèle

L'état du modèle (Model State) représente les erreurs de validation dans les valeurs de formulaire HTML envoyées.

MVC continuera la validation des champs jusqu'à atteindre le nombre maximal d’erreurs (200 par défaut). Vous pouvez configurer ce nombre en insérant le code suivant dans la méthode `ConfigureServices` dans le fichier *Startup.cs* : 

[!code-csharp[Main](validation/sample/Startup.cs?range=27)]

## <a name="handling-model-state-errors"></a>Gérer l'état de modèle des erreurs

La validation du modèle se produit avant chaque action du contrôleur qui est appelée, et il incombe à la méthode d’action d'inspecter `ModelState.IsValid` et de réagir de façon appropriée. Dans de nombreux cas, la réaction appropriée doit renvoyer une réponse d’erreur, dans l’idéal, détaillant la raison de l’échec de validation du modèle.

Certaines applications choisissent de suivre une convention standard pour traiter les erreurs de validation de modèle, auquel cas un filtre peut être un emplacement approprié pour implémenter une telle stratégie. Vous devez tester le comportement de vos actions avec les États de modèles valides et non valides. 

## <a name="manual-validation"></a>Validation manuelle

Une fois la validation et la liaison de modèle terminées, vous souhaiterez répéter des parties de celui-ci. Par exemple, un utilisateur peut avoir entré du texte dans un champ attendant un entier, ou vous devrez peut-être calculer une valeur pour les propriétés d’un modèle. 

Vous devrez peut-être exécuter manuellement la validation. Pour ce faire, appelez la méthode `TryValidateModel`, comme illustré ici :

[!code-csharp[Main](validation/sample/MoviesController.cs?range=52)]

## <a name="custom-validation"></a>Validation personnalisée

Les attributs de validation fonctionnent pour la plupart des besoins de la validation. Toutefois, des règles de validation sont spécifiques à votre métier. Vos règles peuvent ne pas être des techniques de validation de données courantes telles que vous être assuré qu’un champ est obligatoire ou qu’il est conforme à une plage de valeurs. Pour ces scénarios, les attributs de validation personnalisés sont une excellente solution. Il est facile de créer vos propres attributs de validation personnalisés dans MVC. Héritez simplement de `ValidationAttribute`et remplacez le `IsValid` (méthode). Le `IsValid` méthode accepte deux paramètres, le premier est un objet nommé *value* et le second est un objet `ValidationContext` nommé *validationContext*. *Value* fait référence à la valeur réelle du champ de la validation de votre validateur personnalisé.

Dans l’exemple suivant, une règle métier stipule que les utilisateurs ne peuvent pas définir le genre *classique* pour une vidéo publiée après 1960. L'attribut `[ClassicMovie]` vérifie d’abord le genre et s’il s’agit d’un classique, il vérifie ensuite la date de publication pour que ça soit après 1960. Si il est sori après 1960, la validation échoue. L’attribut accepte un paramètre entier qui représente l’année que vous pouvez utiliser pour valider les données. Vous pouvez capturer la valeur du paramètre dans le constructeur de l’attribut, comme illustré ici :

[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=9-29)]

La variable `movie` ci-dessus représente un objet `Movie` qui contient les données à partir de l’envoi du formulaire à valider. Dans ce cas, le code de validation vérifie la date et le genre dans la méthode `IsValid` de la classe `ClassicMovieAttribute` conformément aux règles. Si la validation réussit `IsValid` retourne un code `ValidationResult.Success`, et lorsque la validation échoue, un `ValidationResult` avec un message d’erreur. Quand un utilisateur modifie le champ `Genre` et envoie le formulaire, la méthode `IsValid` de `ClassicMovieAttribute` doit vérifier si le film est un classique. Comme n’importe quel attribut intégré, vous devez appliquer le `ClassicMovieAttribute` à une propriété telle que `ReleaseDate` pour garantir la validation se produit, comme indiqué dans l’exemple de code précédent. Étant donné que l’exemple fonctionne uniquement avec le type `Movie`, une meilleure option consiste à utiliser `IValidatableObject` comme indiqué dans le paragraphe suivant.

De manière alternative, ce code aurait pu être placé dans le modèle en implémentant la méthode `Validate` de l'interface `IValidatableObject`. Tandis que les attributs de validation personnalisés fonctionnent bien pour la validation des propriétés individuelles, l'implémentation de `IValidatableObject` peut être utilisée pour implémenter la validation au niveau de la classe comme illustré ci-après.

[!code-csharp[Main](validation/sample/MovieIValidatable.cs?range=32-40)]

## <a name="client-side-validation"></a>Validation côté client

La validation côté client est très pratique pour les utilisateurs. Cela fait gagner du temps qui nécessiterait sinon un aller-retour vers le serveur. Sur le plan professionnel, quelques fractions de secondes multipliées des centaines de fois par jour accroissent le temps et les frais requis, ainsi que la frustration. La validation simple et immédiate permet aux utilisateurs de travailler plus efficacement et de produire une entrée et une sortie de meilleure qualité. 

Vous devez disposer d’une vue avec les références de script JavaScript appropriées en place pour que la validation côté client fonctionne comme vous le voyez ici.

[!code-cshtml[Main](validation/sample/Views/Shared/_Layout.cshtml?range=37)]

[!code-cshtml[Main](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml)]

Le script de [Validation jQuery non Obstrusive](https://github.com/aspnet/jquery-validation-unobtrusive) est une librairie front-end personnalisée de Microsoft qui s’appuie sur le plug-in [jQuery validation](https://jqueryvalidation.org/). Sans la Validation jQuery non obtruvsive, vous devrez avoir la même logique de validation à deux emplacements de code : une fois dans les attributs de validation côté serveur sur les propriétés du modèle, puis à nouveau dans les scripts côté client (les exemples de la méthode jQuery validation [ `validate()` ](https://jqueryvalidation.org/validate/) montrent comment complexe cela peut devenir). Au lieu de cela, des [Tag helpers](xref:mvc/views/tag-helpers/intro) et [HTML helpers](xref:mvc/views/overview) de MVC sont en mesure d’utiliser les attributs de validation et de métadonnées à partir des propriétés de modèle pour effectuer le rendu HTML 5 de type [data-attributes](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes) dans les éléments de formulaire nécessitant une validation. MVC génère les attributs `data-` pour les attributs intégrés et personnalisés. La validation jQuery non obstructive analyse ensuite ces attributs `data-` et transmet la logique de validation jQuery, en «copiant» de manière efficace la logique de validation côté serveur au client. Vous pouvez afficher les erreurs de validation sur le client en utilisant les tags helpers appropriés comme indiqué ici :

[!code-cshtml[Main](validation/sample/Views/Movies/Create.cshtml?highlight=4,5&range=19-25)]

Les tags helpers ci-dessus affichent le HTML ci-dessous. Notez que les attributs `data-` dans le code HTML de sortie correspondent aux attributs de validation pour la propriété `ReleaseDate`. L'attribut `data-val-required` ci-dessous contient un message d’erreur à afficher si l’utilisateur ne remplit pas dans le champ date de sortie. La Validation jQuery non obtrusive passe cette valeur à la méthode de validation jQuery [ `required()` ](https://jqueryvalidation.org/required-method/), qui affiche alors ce message dans l'élément **\<span >** l’accompagnant.

```html
<form action="/Movies/Create" method="post">
    <div class="form-horizontal">
        <h4>Movie</h4>
        <div class="text-danger"></div>
        <div class="form-group">
            <label class="col-md-2 control-label" for="ReleaseDate">ReleaseDate</label>
            <div class="col-md-10">
                <input class="form-control" type="datetime"
                data-val="true" data-val-required="The ReleaseDate field is required."
                id="ReleaseDate" name="ReleaseDate" value="" />
                <span class="text-danger field-validation-valid"
                data-valmsg-for="ReleaseDate" data-valmsg-replace="true"></span>
            </div>
        </div>
    </div>
</form>
```

La validation côté client empêche l'envoi jusqu'à ce que le formulaire soit valide. Le bouton d’envoi exécute JavaScript qui envoie le formulaire ou affiche des messages d’erreur. 

MVC détermine les valeurs d’attribut de type en fonction du type de données .NET d’une propriété, et éventuellement remplacés à l’aide d'attributs `[DataType]`. L'attribut`[DataType]` de base n'effectue aucune validation côté serveur. Les navigateurs choisissent leurs propres messages d’erreur et affichent ces erreurs, comme ils le souhaitent, toutefois le package Validation jQuery non obtrusive peut remplacer les messages et les afficher de manière cohérente avec d’autres. Cela se produit plus évidemment lorsque les utilisateurs appliquent les sous-classes`[DataType]` telles que `[EmailAddress]`.

### <a name="add-validation-to-dynamic-forms"></a>Ajouter une Validation aux formulaires dynamiques

Etant donné que la Validation jQuery non obtrusive transmet la logique de validation et les paramètres jQuery validation lors du premier chargement de la page, les formulaires générés de manière dynamique ne sont pas automatiquement validés. Au lieu de cela, vous devez indiquer à la Validation jQuery non obtrusive d'analyser le formulaire dynamique immédiatement après sa création. Par exemple, le code ci-dessous montre comment vous pouvez configurer la validation côté client sur un formulaire ajouté via AJAX.

```js
$.get({
    url: "https://url/that/returns/a/form",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add form. " + errorThrown);
    },
    success: function(newFormHTML) {
        var container = document.getElementById("form-container");
        container.insertAdjacentHTML("beforeend", newFormHTML);
        var forms = container.getElementsByTagName("form");
        var newForm = forms[forms.length - 1];
        $.validator.unobtrusive.parse(newForm);
    }
})
```

La méthode `$.validator.unobtrusive.parse()` accepte un sélecteur de jQuery pour argument. Cette méthod indique à la méthode d'analyser les attributs `data-`des formulaires dans ce sélecteur. Les valeurs de ces attributs sont ensuite passées pour le plug-in de la validation jQuery afin que le formulaire présente les règles de validation côté client souhaitées.

### <a name="add-validation-to-dynamic-controls"></a>Ajouter une Validation à des contrôles dynamiques

Vous pouvez également mettre à jour les règles de validation sur un formulaire lorsque les contrôles individuels, tels que les `<input/>` et les `<select/>`, sont générées de façon dynamique. Vous ne pouvez pas passer des sélecteurs pour ces éléments à la méthode `parse()` directement, car le formulaire qui l’entoure a déjà été analysé et ne sera pas mis à jour. Au lieu de cela, supprimez d’abord les données de validation existantes, puis analysez l’intégralité du formulaire, comme indiqué ci-dessous :

```js
$.get({
    url: "https://url/that/returns/a/control",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add form. " + errorThrown);
    },
    success: function(newInputHTML) {
        var form = document.getElementById("my-form");
        form.insertAdjacentHTML("beforeend", newInputHTML);
        form.removeData("validator")    // Added by the raw jQuery Validate
            .removeData("unobtrusiveValidation");   // Added by jQuery Unobtrusive Validation
        $.validator.unobtrusive.parse(form);
    }
})
```

## <a name="iclientmodelvalidator"></a>IClientModelValidator

Vous pouvez créer une logique côté client pour votre attribut personnalisé, et [la validation non obtrusive](http://jqueryvalidation.org/documentation/) s'exécutera sur le client pour vous automatiquement dans le cadre de la validation. La première étape consiste à contrôler que les attributs de données sont ajoutés en implémentant l'interface `IClientModelValidator` comme indiqué ici :

[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=30-42)]

Les attributs qui implémentent cette interface peuvent ajouter des attributs HTML pour les champs générés. L'examen de la sortie pour l'élément `ReleaseDate` révèle que le HTML est similaire à l’exemple précédent, mais il existe désormais un attribut `data-val-classicmovie` qui a été défini dans la méthode `AddValidation` de `IClientModelValidator`. 

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="" />
```

La validation discrète utilise les données dans les attributs `data-` pour afficher des messages d’erreur. Toutefois, jQuery ne connaît pas les règles ou les messages jusqu'à ce que vous les ajoutiez à l'objet `validator` de jQuery. Cela est illustré dans l’exemple ci-dessous qui ajoute une méthode nommée `classicmovie` contenant le code de validation client personnalisé pour l'objet `validator` de jQuery. 

[!code-javascript[Main](validation/sample/Views/Movies/Create.cshtml?range=71-93)]

JQuery possède maintenant les informations pour exécuter la validation personnalisée de JavaScript, ainsi que le message d’erreur à afficher si le code de validation retourne la valeur false.

## <a name="remote-validation"></a>Validation à distance

La validation à distance est une fonctionnalité intéressante à utiliser lorsque vous avez besoin valider les données sur le client par rapport aux données sur le serveur. Par exemple, votre application peut avoir besoin vérifier si un email ou un nom d’utilisateur est déjà en cours d’utilisation, et il doit interroger une grande quantité de données pour le faire. Le téléchargement de grands jeux de données pour la validation d’un ou plusieurs champs consomme trop de ressources. Cela peut également exposer des informations sensibles. Une alternative consiste à effectuer une requête de parcours circulaire (round-trip) pour valider un champ.

Vous pouvez implémenter la validation à distance dans un processus en deux étapes. Tout d’abord, vous devez annoter votre modèle avec l'attribut `[Remote]`. L'attribut `[Remote]` accepte plusieurs surcharges qui vous permettent de diriger le côté client JavaScript vers le code approprié à appeler. L’exemple ci-dessous pointe vers la méthode d’action `VerifyEmail` du contrôleur `Users`.

[!code-csharp[Main](validation/sample/User.cs?range=7-8)]

La deuxième étape consiste à placer le code de validation dans la méthode d’action correspondante tel que définie dans l'attribut `[Remote]`. Selon la documentation de la validation jQuery de la méthode [ `remote()` ](https://jqueryvalidation.org/remote-method/): 

> La réponse serverside doit être une chaîne JSON qui doit être `"true"` pour les éléments valides et peut être `"false"`, `undefined`, ou `null` pour les éléments non valides, à l’aide de message d’erreur par défaut. Si la réponse serverside est une chaîne, par exemple. `"That name is already taken, try peter123 instead"`, cette chaîne s’affichera sous la forme d’un message d’erreur personnalisé à la place de la valeur par défaut.

La définition de la méthode `VerifyEmail()` suit ces règles, comme indiqué ci-dessous. Elle renvoie un message d'erreur de validation si l'e-mail est pris ou `true` si l'e-mail est libre, et encapsule le résultat dans un objet `JsonResult`. Le côté client peut ensuite utiliser la valeur retournée pour continuer ou afficher l’erreur si nécessaire. 

[!code-csharp[Main](validation/sample/UsersController.cs?range=19-28)]

Maintenant lorsque les utilisateurs entrent un message électronique, le JavaScript dans la vue effectue un appel à distance pour voir si l’e-mail a été utilisé et, dans ce cas, affiche le message d’erreur. Dans le cas contraire, l’utilisateur peut soumettre le formulaire comme d’habitude.

La propriété `AdditionalFields` de l'attribut `[Remote]` est utile pour valider les combinaisons de champs sur des données sur le serveur. Par exemple, si le modèle `User` ci-dessus a deux propriétés supplémentaires : `FirstName` et `LastName`, vous pouvez souhaiter vérifier qu’aucun utilisateur existant a déjà cette paire de noms. Vous définissez les nouvelles propriétés comme indiqué dans le code suivant :

[!code-csharp[Main](validation/sample/User.cs?range=10-13)]

`AdditionalFields` peut avoir été défini explicitement pour les chaînes `"FirstName"` et `"LastName"`, mais en utilisant l'opérateur [ `nameof` ](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/nameof) comme cela simplifie la refactorisation ultérieure. La méthode d’action pour effectuer la validation doit accepter deux arguments, un pour la valeur de `FirstName` et l’autre pour la valeur de `LastName`.

[!code-csharp[Main](validation/sample/UsersController.cs?range=30-39)]

Maintenant lorsque les utilisateurs entrer un nom et prénom, JavaScript :

* Effectue un appel à distance pour voir si cette paire de noms a été prise.
* Si la paire existe, un message d’erreur s’affiche. 
* Si ne pas le cas, l’utilisateur peut envoyer le formulaire.

Si vous devez valider deux ou plusieurs champs supplémentaires avec l'attribut `[Remote]`, vous les fournissez sous forme de liste délimitée par des virgules. Par exemple, pour ajouter une propriété `MiddleName` au modèle, affectez l'attribut `[Remote]` comme indiqué dans le code suivant : 

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

`AdditionalFields`, comme tous les arguments d’attribut, doit être une expression constante. Par conséquent, vous ne devez pas utiliser une [chaîne interpolée](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings) ou appeler [ `string.Join()` ](https://msdn.microsoft.com/library/system.string.join(v=vs.110).aspx) pour initialiser `AdditionalFields`. Pour chaque champ supplémentaire que vous ajoutez à l'attribut `[Remote]`, vous devez ajouter un autre argument à la méthode d’action de contrôleur correspondant.
