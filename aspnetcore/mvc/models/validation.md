---
title: Validation de modèle dans ASP.NET Core MVC
author: tdykstra
description: Découvrez plus d’informations sur la validation de modèle dans ASP.NET Core MVC.
ms.author: riande
ms.custom: mvc
ms.date: 11/06/2018
uid: mvc/models/validation
ms.openlocfilehash: f1757f807e50019e5071abc42ec3129935ab77aa
ms.sourcegitcommit: fc7eb4243188950ae1f1b52669edc007e9d0798d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51225458"
---
# <a name="model-validation-in-aspnet-core-mvc"></a>Validation de modèle dans ASP.NET Core MVC

Par [Rachel Appel](https://github.com/rachelappel)

## <a name="introduction-to-model-validation"></a>Introduction à la validation de modèle

Avant qu’une application stocke des données dans une base de données, l’application doit valider les données. L’application doit vérifier que les données ne contiennent pas de menaces de sécurité potentielles, et qu’elles sont mises en forme de façon appropriée quand au type et à la taille. Elles doivent aussi être conformes à vos règles. La validation est nécessaire même si son implémentation peut être fastidieuse et redondante. Dans MVC, la validation se produit à la fois sur le client et sur le serveur.

Heureusement, .NET dispose d’une validation abstraite dans des attributs de validation. Ces attributs contiennent un code de validation, ce qui réduit la quantité de code à écrire.

Dans ASP.NET Core 2.2 et ultérieur, le runtime ASP.NET Core court-circuite (ignore) la validation s’il détermine qu’un graphe de modèle donné n’a pas besoin de validation. Cela peut déboucher sur une amélioration significative des performances lors de la validation de modèles qui ne peuvent pas avoir de validateurs associés ou qui n’en ont pas. La validation ignorée inclut des objets comme des collections de primitifs (`byte[]`, `string[]`, `Dictionary<string, string>`, etc.) ou des graphes d’objets complexes sans validateurs.

[Affichez ou téléchargez un exemple depuis GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample).

## <a name="validation-attributes"></a>Attributs de validation

Les attributs de validation sont un moyen de configurer la validation de modèle : elle est donc conceptuellement similaire à la validation des champs dans les tables de base de données. Ceci inclut des contraintes comme l’affectation de types de données ou des champs obligatoires. L’application de modèles aux données pour forcer le respect des règles d’entreprise, comme une carte de crédit, un numéro de téléphone ou une adresse e-mail, est un autre type de validation. Les attributs de validation simplifient et facilitent l’application de ces exigences.

Les attributs de validation sont spécifiés au niveau de la propriété : 

```csharp
[Required]
public string MyProperty { get; set; } 
```

Voici un modèle `Movie` annoté pour une application qui stocke des informations sur les films et les émissions de télévision. La plupart des propriétés sont obligatoires et plusieurs propriétés de type chaîne sont soumises à des exigences en matière de longueur. En outre, une restriction de plage numérique de 0 à 999,99 $ est en place pour la propriété `Price`, ainsi qu’un attribut de validation personnalisé.

[!code-csharp[](validation/sample/Movie.cs?range=6-29)]

Une simple lecture du modèle révèle les règles concernant les données de cette application, facilitant ainsi la maintenance du code. Voici plusieurs attributs de validation intégrés courants :

* `[CreditCard]` : vérifie que la propriété a un format de carte de crédit.

* `[Compare]` : vérifie que deux propriétés d’un modèle sont en correspondance.

* `[EmailAddress]` : vérifie que la propriété a un format d’e-mail.

* `[Phone]` : vérifie que la propriété a un format de numéro de téléphone.

* `[Range]` : vérifie que la valeur de la propriété est dans la plage donnée.

* `[RegularExpression]` : vérifie que les données correspondent à l’expression régulière spécifiée.

* `[Required]` : rend une propriété obligatoire.

* `[StringLength]` : vérifie que la longueur d’une propriété de type chaîne est au plus la longueur maximale spécifiée.

* `[Url]` : vérifie que la propriété a un format d’URL.

MVC prend en charge tout attribut dérivant de `ValidationAttribute` à des fins de validation. Vous pouvez trouver de nombreux attributs de validation utiles dans l’espace de noms [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations).

Pour certaines, vous pouvez avoir besoin de plus de fonctionnalités que celles fournies par les attributs prédéfinis. Dans ces cas, vous pouvez créer des attributs de validation personnalisés en dérivant depuis `ValidationAttribute` ou en modifiant votre modèle pour implémenter `IValidatableObject`.

## <a name="notes-on-the-use-of-the-required-attribute"></a>Remarques sur l’utilisation de l’attribut Required

Les [types valeur](/dotnet/csharp/language-reference/keywords/value-types) non Nullable (tels que `decimal`, `int`, `float` et `DateTime`) sont obligatoires par nature et n’ont pas besoin de l’attribut `Required`. L’application n’effectue pas de vérifications de validation côté serveur pour les types non Nullables qui sont marqués `Required`.

La liaison de modèle MVC, qui n’est pas concernée par la validation et les attributs de validation, rejette l’envoi d’un champ de formulaire contenant une valeur manquante ou un espace pour un type non Nullable. En l’absence d’un attribut `BindRequired` sur la propriété cible, la liaison de modèle ignore les données manquantes pour les types non Nullables, où le champ de formulaire est absent dans les données du formulaire entrant.

L’[attribut BindRequired](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.bindrequiredattribute) (consultez également <xref:mvc/models/model-binding#customize-model-binding-behavior-with-attributes>) est pratique pour vérifier que les données d’un formulaire sont complètes. Quand il est appliqué à une propriété, le système de la liaison de modèle exige une valeur pour cette propriété. Quand il est appliqué à un type, le système de la liaison de modèle exige des valeurs pour toutes les propriétés de ce type.

Quand vous utilisez un type [Nullable\<T> ](/dotnet/csharp/programming-guide/nullable-types/) (par exemple `decimal?` ou `System.Nullable<decimal>`) et que vous le marquez `Required`, une vérification de la validation côté serveur est effectuée comme si la propriété était de type nullable standard (par exemple `string`).

La validation côté client nécessite une valeur pour un champ de formulaire qui correspond à une propriété du modèle que vous avez marquée `Required` et pour une propriété de type non Nullable que vous n’avez pas marquée `Required`. `Required` peut être utilisé pour contrôler le message d’erreur de validation côté client.

## <a name="model-state"></a>État du modèle

L’état du modèle représente les erreurs de validation dans les valeurs du formulaire HTML envoyé.

MVC continue la validation des champs jusqu’à ce que le nombre maximal d’erreurs soit atteint (200 par défaut). Vous pouvez configurer ce nombre avec le code suivant dans `Startup.ConfigureServices` :

[!code-csharp[](validation/sample/Startup.cs?range=27)]

## <a name="handle-model-state-errors"></a>Gérer les erreurs d’état de modèle

La validation de modèle se produit avant l’exécution d’une action de contrôleur. Il incombe à l’action d’inspecter `ModelState.IsValid` et de réagir de façon appropriée. Dans de nombreux cas, la réaction appropriée est de retourner une réponse d’erreur, dans l’idéal en détaillant la raison de l’échec de validation du modèle.

::: moniker range=">= aspnetcore-2.1"

Quand `ModelState.IsValid` prend la valeur `false` dans les contrôleurs d’API web à l’aide de l’attribut `[ApiController]`, une réponse HTTP 400 automatique contenant les détails du problème est retournée. Pour plus d’informations, consultez [Réponses HTTP 400 automatiques](xref:web-api/index#automatic-http-400-responses).

::: moniker-end

Certaines applications choisissent de suivre une convention standard pour traiter les erreurs de validation de modèle ; dans ce cas, un filtre peut être un emplacement approprié pour implémenter une telle stratégie. Vous devez tester le comportement de vos actions avec des états de modèle valides et non valides.

## <a name="manual-validation"></a>Validation manuelle

Une fois la liaison de modèle et la validation terminées, vous pouvez en répéter des parties. Par exemple, un utilisateur peut avoir entré du texte dans un champ qui attendait un entier, ou il peut être nécessaire de calculer une valeur pour une propriété du modèle.

Il peut être nécessaire d’effectuer la validation manuellement. Pour cela, appelez la méthode `TryValidateModel`, comme indiqué ici :

[!code-csharp[](validation/sample/MoviesController.cs?range=52)]

## <a name="custom-validation"></a>Validation personnalisée

Les attributs de validation fonctionnent pour la plupart des besoins de validation. Certaines règles de validation sont cependant spécifiques à votre métier. Vos règles peuvent ne pas être des techniques de validation de données courantes comme vérifier qu’un champ est obligatoire ou qu’il est conforme à une plage de valeurs. Pour ces scénarios, les attributs de validation personnalisés sont une bonne solution. Vous pouvez créer facilement vos propres attributs de validation personnalisée dans MVC. Héritez simplement de `ValidationAttribute` et remplacez la méthode `IsValid`. La méthode `IsValid` accepte deux paramètres, le premier est un objet nommé *value* et le second est un objet `ValidationContext` nommé *validationContext*. *value* fait référence à la valeur réelle du champ que votre validateur personnalisé doit valider.

Dans l’exemple suivant, une règle métier stipule que les utilisateurs ne peuvent pas définir le genre sur *Classic* pour un film sorti après 1960. L’attribut `[ClassicMovie]` vérifie d’abord le genre et, s’il s’agit d’un classique, vérifie ensuite si date de sortie est postérieure à 1960. Si la sortie est postérieure à 1960, la validation échoue. L’attribut accepte un paramètre entier qui représente l’année que vous pouvez utiliser pour valider les données. Vous pouvez capturer la valeur du paramètre dans le constructeur de l’attribut, comme illustré ici :

[!code-csharp[](validation/sample/ClassicMovieAttribute.cs?range=9-28)]

La variable `movie` ci-dessus représente un objet `Movie` qui contient les données de l’envoi du formulaire à valider. Dans ce cas, le code de validation vérifie la date et le genre dans la méthode `IsValid` de la classe `ClassicMovieAttribute` selon les règles définies. Si la validation réussit, `IsValid` retourne un code `ValidationResult.Success`. Quand la validation échoue, un `ValidationResult` avec un message d’erreur est retourné :

[!code-csharp[](validation/sample/ClassicMovieAttribute.cs?range=55-58)]

Quand un utilisateur modifie le champ `Genre` et envoie le formulaire, la méthode `IsValid` de la classe `ClassicMovieAttribute` doit vérifier si le film est un classique. Comme pour tout attribut prédéfini, appliquez `ClassicMovieAttribute` à une propriété comme `ReleaseDate` pour garantir que la validation se produit, comme illustré dans l’exemple de code précédent. Étant donné que l’exemple fonctionne seulement avec les types `Movie`, une meilleure option consiste à utiliser `IValidatableObject` comme illustré dans le paragraphe suivant.

Vous pouvez aussi placer ce même code dans le modèle en implémentant la méthode `Validate` sur l’interface `IValidatableObject`. Si les attributs de validation personnalisés fonctionnent bien pour la validation de propriétés individuelles, vous pouvez aussi utiliser l’implémentation de `IValidatableObject` pour implémenter une validation au niveau de la classe, comme illustré ci-après.

[!code-csharp[](validation/sample/MovieIValidatable.cs?range=32-40)]

## <a name="client-side-validation"></a>Validation côté client

La validation côté client est très pratique pour les utilisateurs. Elle leur épargne le temps d’attente nécessaire à un aller-retour avec le serveur. En termes d’activité de l’entreprise, même de quelques fractions de secondes multipliées des centaines de fois par jour finissent par représenter un temps considérable, auquel s’ajoute un coût et de la frustration. Une validation directe et immédiate permet aux utilisateurs de travailler plus efficacement, et de produire une meilleure qualité des entrées et des sorties.

Vous devez disposer d’une vue avec les références de script JavaScript appropriées en place pour que la validation côté client fonctionne comme vous le voyez ici.

[!code-cshtml[](validation/sample/Views/Shared/_Layout.cshtml?range=37)]

[!code-cshtml[](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml)]

Le script [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) est une bibliothèque frontale personnalisée de Microsoft qui s’appuie sur le plug-in bien connu [jQuery Validate](https://jqueryvalidation.org/). Sans jQuery Unobtrusive Validation, vous devriez coder la même logique de validation à deux endroits : une fois dans les attributs de validation côté serveur sur les propriétés du modèle, puis à nouveau dans les scripts côté client (les exemples de méthode [`validate()`](https://jqueryvalidation.org/validate/) de jQuery Validate montrent combien ceci peut devenir complexe). Au lieu de cela, les [Tag Helpers](xref:mvc/views/tag-helpers/intro) et les [helpers HTML](xref:mvc/views/overview) peuvent utiliser les attributs de validation et les métadonnées de type des propriétés du modèle pour rendre les [attributs data-](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes) HTML 5 dans les éléments de formulaire nécessitant une validation. MVC génère les attributs `data-` pour les attributs intégrés et pour les attributs personnalisés. jQuery Unobtrusive Validation analyse ensuite les attributs `data-` et passe la logique à jQuery Validate, en « copiant » la logique de validation côté serveur vers le client. Vous pouvez afficher les erreurs de validation sur le client en utilisant les Tag Helpers appropriés, comme indiqué ici :

[!code-cshtml[](validation/sample/Views/Movies/Create.cshtml?highlight=4,5&range=19-25)]

Les Tag Helpers ci-dessus rendent le HTML ci-dessous. Notez que les attributs `data-` dans la sortie HTML correspondent aux attributs de validation pour la propriété `ReleaseDate`. L’attribut `data-val-required` ci-dessous contient un message d’erreur à afficher si l’utilisateur ne renseigne pas le champ correspondant à la date de sortie. jQuery Unobtrusive Validation passe cette valeur à la méthode [`required()`](https://jqueryvalidation.org/required-method/) de jQuery Validate, qui affiche alors ce message dans l’élément **\<span>** qui l’accompagne.

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

La validation côté client empêche l’envoi jusqu’à ce que le formulaire soit valide. Le bouton Submit exécute le code JavaScript qui envoie le formulaire ou qui affiche des messages d’erreur.

MVC détermine les valeurs d’attribut de type en fonction du type de données .NET d’une propriété, éventuellement remplacé avec des attributs `[DataType]`. L’attribut `[DataType]` de base ne fait pas de validation côté serveur réelle. Les navigateurs choisissent eux-mêmes leurs propres messages d’erreur et affichent ces erreurs ; le package jQuery Unobtrusive Validation peut cependant remplacer les messages et les afficher de façon cohérente avec d’autres messages. Ceci se produit de façon plus évidente quand des utilisateurs appliquent des sous-classes `[DataType]` comme `[EmailAddress]`.

### <a name="add-validation-to-dynamic-forms"></a>Ajouter une validation à des formulaires dynamiques

Comme jQuery Unobtrusive Validation passe la logique et les paramètres de validation à jQuery Validate lors du premier chargement de la page, les formulaires générés dynamiquement ne se présentent pas automatiquement à la validation. Au lieu de cela, vous devez indiquer à jQuery Validate d’analyser le formulaire dynamique immédiatement après sa création. Par exemple, le code ci-dessous montre comment vous pouvez configurer la validation côté client sur un formulaire ajouté via AJAX.

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

La méthode `$.validator.unobtrusive.parse()` accepte un sélecteur jQuery comme argument. Cette méthode indique à jQuery Unobtrusive Validation d’analyser les attributs `data-` des formulaires dans ce sélecteur. Les valeurs de ces attributs sont ensuite passées au plug-in jQuery Validate pour que le formulaire présente les règles de validation souhaitées côté client.

### <a name="add-validation-to-dynamic-controls"></a>Ajouter une validation à des contrôles dynamiques

Vous pouvez aussi mettre à jour les règles de validation sur un formulaire quand des contrôles individuels, comme `<input/>` et `<select/>`, sont générés dynamiquement. Vous ne pouvez pas passer de sélecteurs pour ces éléments directement à la méthode `parse()`, car le formulaire entourant a déjà été analysé et ne sera pas mis à jour. Au lieu de cela, vous supprimez d’abord les données de validation existantes, puis vous réanalysez tout le formulaire, comme indiqué ci-dessous :

```js
$.get({
    url: "https://url/that/returns/a/control",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add control. " + errorThrown);
    },
    success: function(newInputHTML) {
        var form = document.getElementById("my-form");
        form.insertAdjacentHTML("beforeend", newInputHTML);
        $(form).removeData("validator")    // Added by jQuery Validate
               .removeData("unobtrusiveValidation");   // Added by jQuery Unobtrusive Validation
        $.validator.unobtrusive.parse(form);
    }
})
```

## <a name="iclientmodelvalidator"></a>IClientModelValidator

Vous pouvez créer une logique côté client pour votre attribut personnalisé, et la [validation discrète](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) qui crée un adaptateur pour la [validation jquery](http://jqueryvalidation.org/documentation/) l’exécute sur le client automatiquement pour vous dans le cadre de la validation. La première étape consiste à contrôler quels attributs data- sont ajoutés en implémentant l’interface `IClientModelValidator` comme indiqué ici :

[!code-csharp[](validation/sample/ClassicMovieAttribute.cs?range=30-42)]

Les attributs qui implémentent cette interface peuvent ajouter des attributs HTML aux champs générés. L’examen de la sortie pour l’élément `ReleaseDate` révèle du HTML qui est similaire à l’exemple précédent, excepté qu’il existe maintenant un attribut `data-val-classicmovie` qui a été défini dans la méthode `AddValidation` de `IClientModelValidator`.

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="" />
```

La validation discrète utilise les données de l’attribut `data-` pour afficher les messages d’erreur. Cependant, jQuery ne dispose pas des règles ou des messages tant que vous ne les avez pas ajoutés à l’objet `validator` de jQuery. Cela est illustré dans l’exemple suivant, qui ajoute une méthode de validation client `classicmovie` personnalisée pour l’objet `validator` jQuery. Pour obtenir une explication de la méthode `unobtrusive.adapters.add`, consultez [Validation client non obstrusive dans ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html).

[!code-javascript[](validation/sample/Views/Movies/Create.cshtml?name=snippet_UnobtrusiveValidation)]

Avec le code précédent, la méthode `classicmovie` effectue la validation côté client à la date de mise en production du film. Le message d’erreur s’affiche si la méthode retourne `false`.

## <a name="remote-validation"></a>Validation à distance

La validation à distance est une fonctionnalité intéressante à utiliser quand vous devez valider des données sur le client relativement à des données présentes sur le serveur. Par exemple, votre application doit vérifier si une adresse e-mail ou un nom d’utilisateur est déjà utilisé, et elle doit pour cela interroger une grande quantité de données. Le téléchargement de grands jeux de données pour valider un seul champ ou même quelques-uns consomme trop de ressources. Il peut également exposer des informations sensibles. Une alternative consiste à faire une demande aller-retour pour valider un champ.

Vous pouvez implémenter une validation à distance selon un processus en deux étapes. Vous devez d’abord annoter votre modèle avec l’attribut `[Remote]`. L’attribut `[Remote]` accepte plusieurs surcharges que vous pouvez utiliser pour diriger le code JavaScript côté client vers le code approprié à appeler. L’exemple ci-dessous pointe vers la méthode d’action `VerifyEmail` du contrôleur `Users`.

[!code-csharp[](validation/sample/User.cs?range=7-8)]

La deuxième étape consiste à placer le code de validation dans la méthode d’action correspondante comme défini dans l’attribut `[Remote]`. Selon la documentation sur la méthode jQuery Validate [distante](https://jqueryvalidation.org/remote-method/), la réponse du serveur doit être une chaîne JSON qui est :

* `"true"` pour les éléments valides.
* `"false"`, `undefined` ou `null` pour les éléments non valides, avec le message d’erreur par défaut.

Si la réponse du serveur est une chaîne (par exemple, `"That name is already taken, try peter123 instead"`), la chaîne est affichée comme un message d’erreur personnalisé à la place de la chaîne par défaut.

La définition de la méthode `VerifyEmail` suit ces règles, comme indiqué ci-dessous. Elle retourne un message d’erreur de validation si l’adresse e-mail est déjà utilisée ou `true` si l’adresse e-mail est libre, et elle encapsule le résultat dans un objet `JsonResult`. Le côté client peut alors utiliser la valeur retournée pour continuer ou afficher l’erreur si nécessaire.

[!code-csharp[](validation/sample/UsersController.cs?range=19-28)]

Maintenant, quand les utilisateurs entrent une adresse e-mail, le code JavaScript de la vue effectue un appel à distance pour vérifier si l’e-mail a déjà été utilisé et, le cas échéant, affiche le message d’erreur. Dans le cas contraire, l’utilisateur peut envoyer le formulaire comme d’habitude.

La propriété `AdditionalFields` de l’attribut `[Remote]` est pratique pour valider des combinaisons de champs relativement à des données présentes sur le serveur. Par exemple, si le modèle `User` ci-dessus a deux propriétés supplémentaires appelées `FirstName` et `LastName`, vous pouvez vérifier qu’aucun utilisateur existant n’a déjà cette paire de noms. Vous définissez les nouvelles propriétés comme indiqué dans le code suivant :

[!code-csharp[](validation/sample/User.cs?range=10-13)]

`AdditionalFields` peut avoir été défini explicitement avec les chaînes `"FirstName"` et `"LastName"`, mais l’utilisation de l’opérateur [`nameof`](/dotnet/csharp/language-reference/keywords/nameof) de cette façon simplifie la refactorisation ultérieure. La méthode d’action pour effectuer la validation doit alors accepter deux arguments, un pour la valeur de `FirstName` et l’autre pour la valeur de `LastName`.

[!code-csharp[](validation/sample/UsersController.cs?range=30-39)]

Maintenant, quand des utilisateurs entrent un prénom et un nom, JavaScript :

* Effectue un appel à distance pour vérifier si cette paire de noms est déjà utilisée.
* Si la paire est déjà utilisée, un message d’erreur est affiché.
* Si elle n’est pas déjà utilisée, l’utilisateur peut envoyer le formulaire.

Si vous devez valider deux champs supplémentaires ou plus avec l’attribut `[Remote]`, vous les fournissez sous la forme d’une liste délimitée par des virgules. Par exemple, pour ajouter une propriété `MiddleName` au modèle, définissez l’attribut `[Remote]` comme indiqué dans le code suivant :

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

`AdditionalFields`, comme tous les arguments d’attribut, doit être une expression constante. Par conséquent, vous ne devez pas utiliser une [chaîne interpolée](/dotnet/csharp/language-reference/keywords/interpolated-strings) ou appeler [`string.Join()`](https://msdn.microsoft.com/library/system.string.join(v=vs.110).aspx) pour initialiser `AdditionalFields`. Pour chaque champ supplémentaire que vous ajoutez à l’attribut `[Remote]`, vous devez ajouter un autre argument à la méthode d’action de contrôleur correspondante.
