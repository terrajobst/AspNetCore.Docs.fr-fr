---
uid: mvc/overview/getting-started/introduction/adding-validation
title: Ajout d’une Validation | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: 9f35ca15-e216-4db6-9ebf-24380b0f31b4
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-validation
msc.type: authoredcontent
ms.openlocfilehash: 97526d908dd3b835040453b53eae76f733a01c04
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38172191"
---
<a name="adding-validation"></a>Ajout de la validation
====================
par [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

Dans cette section, vous allez ajouter la logique de validation pour la `Movie` modèle et vous avez la certitude que les règles de validation sont appliquées chaque fois qu’un utilisateur tente de créer ou modifier un film à l’aide de l’application.

## <a name="keeping-things-dry"></a>Favoriser le sec

Un des principaux les principes de conception d’ASP.NET MVC est [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself) (&quot;ne vous répétez pas&quot;). ASP.NET MVC vous encourage à spécifier les fonctionnalités ou un comportement qu’une seule fois, puis d’être partout dans une application. Cela réduit la quantité de code, que vous devez écrire et rend le code que vous écrivez moins source d’erreurs et plus facile à gérer.

La prise en charge de la validation fournie par ASP.NET MVC et Entity Framework Code First est un bon exemple du principe DRY en action. Vous pouvez spécifier de façon déclarative des règles de validation au même endroit (dans la classe de modèle) et les règles sont appliquées partout dans l’application.

Examinons comment vous pouvez tirer parti de cette prise en charge de la validation dans l’application de films.

## <a name="adding-validation-rules-to-the-movie-model"></a>Ajout de règles de Validation au modèle Movie

Vous commencerez en ajoutant une logique de validation pour la `Movie` classe.

Ouvrez le fichier *Movie.cs*. Notez que le [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) espace de noms ne contient pas `System.Web`. DataAnnotations fournit un ensemble intégré d’attributs de validation que vous pouvez appliquer de façon déclarative à une classe ou d’une propriété. (Il contient également des attributs de mise en forme comme [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) qui vous aider à formater et ne fournissent aucune validation.)

Maintenant mettre à jour le `Movie` classe pour tirer parti des prédéfinis [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx), et [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) les attributs de validation. Remplacez le `Movie` classe par le code suivant :

[!code-csharp[Main](adding-validation/samples/sample1.cs?highlight=5,13-15,18-19,22-23)]

Le [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attribut définit la longueur maximale de la chaîne et il définit cette limitation sur la base de données, par conséquent, le schéma de base de données sera modifié. Cliquez avec le bouton droit sur le **films** table **Explorateur de serveurs** et cliquez sur **ouvrir la définition de Table**:

![](adding-validation/_static/image1.png)

Dans l’image ci-dessus, vous pouvez voir tous les champs de chaîne sont définies sur [NVARCHAR (MAX)](https://technet.microsoft.com/library/ms186939.aspx). Nous allons utiliser des migrations pour mettre à jour le schéma. Générez la solution, puis ouvrez le **Console du Gestionnaire de Package** fenêtre et entrez les commandes suivantes :

[!code-console[Main](adding-validation/samples/sample2.cmd)]

Une fois cette commande terminée, Visual Studio ouvre le fichier de classe qui définit la nouvelle `DbMIgration` classe dérivée portant le nom spécifié (`DataAnnotations`), puis, dans le `Up` méthode, vous pouvez voir le code qui met à jour les contraintes de schéma :

[!code-csharp[Main](adding-validation/samples/sample3.cs)]

Le `Genre` champ n’est plus nullable (autrement dit, vous devez entrer une valeur). Le `Rating` champ a une longueur maximale de 5 et `Title` a une longueur maximale de 60. La longueur minimale de 3 sur `Title` et la plage sur `Price` n’a pas créé de modifications de schéma.

Examinez le schéma de film :

![](adding-validation/_static/image2.png)

Les champs de chaîne affichent les nouvelles limites de longueur et `Genre` n’est plus vérifiée comme nullable.

Les attributs de validation spécifient le comportement que vous souhaitez appliquer sur les propriétés du modèle sur lesquels ils sont appliqués. Les attributs `Required` et `MinimumLength` indiquent qu’une propriété doit avoir une valeur, mais rien n’empêche un utilisateur d’entrer un espace blanc pour satisfaire cette validation. Le [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) attribut est utilisé pour limiter les caractères pouvant être entrée. Dans le code ci-dessus, `Genre` et `Rating` doivent utiliser uniquement des lettres (les espaces blancs, les chiffres et les caractères spéciaux ne sont pas autorisés). Le [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) attribut contraint une valeur à dans une plage spécifiée. L’attribut `StringLength` vous permet de définir la longueur maximale d’une propriété de chaîne, et éventuellement sa longueur minimale. Types valeur (tel que `decimal, int, float, DateTime`) sont obligatoires par nature et n’avez pas besoin du `Required` attribut.

Code vérifie tout d’abord que les règles de validation que vous spécifiez dans une classe de modèle sont appliquées avant que l’application enregistre les modifications dans la base de données. Par exemple, le code ci-dessous lèvera une [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception(v=vs.103).aspx) exception lors de la `SaveChanges` méthode est appelée, car plusieurs requis `Movie` les valeurs de propriété sont manquantes :

[!code-csharp[Main](adding-validation/samples/sample4.cs)]

Le code ci-dessus lève l’exception suivante :

*Échec de la validation pour une ou plusieurs entités. Consultez la propriété 'EntityValidationErrors' pour plus d’informations.*

Les règles de validation appliquées automatiquement par le .NET Framework vous aide à rendre votre application plus robuste. Cela garantit également que vous n’oublierez pas de valider un élément et que vous n’autoriserez pas par inadvertance l’insertion de données incorrectes dans la base de données.

## <a name="validation-error-ui-in-aspnet-mvc"></a>Erreur de validation de l’interface utilisateur dans ASP.NET MVC

Exécutez l’application et accédez à la */Movies* URL.

Cliquez sur le **créer un nouveau** lien permettant d’ajouter un nouveau film. Remplissez le formulaire avec des valeurs non valides. Dès que la validation côté client jQuery détecte l’erreur, elle affiche un message d’erreur.

![8_validationErrors](adding-validation/_static/image3.png)

> [!NOTE]
> pour prendre en charge la validation jQuery pour les paramètres régionaux non anglais qui utilisent une virgule («, ») pour une décimale, vous devez inclure le package NuGet globaliser comme décrit précédemment dans ce didacticiel.


Notez la façon dont le formulaire a utilisé automatiquement une couleur de bordure rouge pour mettre en évidence les zones de texte qui contiennent des données non valides et a émis un message d’erreur de validation approprié en regard de chacun d’eux. Les erreurs sont appliquées à la fois côté client (à l’aide de JavaScript et jQuery) et côté serveur (au cas où un utilisateur aurait désactivé JavaScript).

Un réel avantage est que vous n’avez pas à modifier une seule ligne de code dans le `MoviesController` classe ou dans le *Create.cshtml* vue afin d’activer l’interface utilisateur de cette validation. Le contrôleur et les vues créées précédemment dans ce didacticiel ont détecté les règles de validation que vous avez spécifiées à l’aide des attributs de validation sur les propriétés de la classe de modèle `Movie`. Testez la validation à l’aide de la méthode d’action `Edit`. La même validation est appliquée.

Les données de formulaire ne sont pas envoyées au serveur tant qu’il y a des erreurs de validation côté client. Vous pouvez vérifier cela en plaçant un point d’arrêt dans la méthode HTTP Post, à l’aide de la [outil fiddler](http://fiddler2.com/fiddler2/), ou IE [outils de développement F12](https://msdn.microsoft.com/ie/aa740478).

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>La Validation se produit dans la création permet d’afficher et créer la méthode d’Action

Vous vous demandez peut-être comment l’interface utilisateur de validation a été générée sans aucune mise à jour du code dans le contrôleur ou dans les vues. La liste suivante montre à quoi le `Create` méthodes dans la `MovieController` classe ressemble. Ils sont identiques à la façon dont vous les avez créées précédemment dans ce didacticiel.

[!code-csharp[Main](adding-validation/samples/sample5.cs)]

La première méthode d’action (HTTP GET) `Create` affiche le formulaire de création initial. La deuxième version (`[HttpPost]`) gère la publication de formulaire. La seconde méthode `Create` (la version `HttpPost`) appelle `ModelState.IsValid` pour vérifier si le film a des erreurs de validation. L’appel de cette méthode évalue tous les attributs de validation qui ont été appliqués à l’objet. Si l’objet comporte des erreurs de validation, la méthode `Create` réaffiche le formulaire. S’il n’y a pas d’erreur, la méthode enregistre le nouveau film dans la base de données. Dans notre exemple de film, **le formulaire n’est pas publié sur le serveur lorsqu’il existe des erreurs de validation détectées côté client ; la seconde** `Create` **méthode n’est jamais appelée**. Si vous désactivez JavaScript dans votre navigateur, la validation côté client est désactivée et la requête HTTP POST `Create` les appels de méthode `ModelState.IsValid` pour vérifier si le film a des erreurs de validation.

Vous pouvez définir un point d’arrêt dans la méthode `HttpPost Create` et vérifier que la méthode n’est jamais appelée. La validation côté client n’enverra pas les données du formulaire quand des erreurs de validation seront détectées. Si vous désactivez JavaScript dans votre navigateur et que vous envoyez ensuite le formulaire avec des erreurs, le point d’arrêt sera atteint. Vous bénéficiez toujours d’une validation complète sans JavaScript. L’image suivante montre comment désactiver JavaScript dans Internet Explorer.

![](adding-validation/_static/image5.png)

![](adding-validation/_static/image6.png)

L’illustration suivante montre comment désactiver JavaScript dans le navigateur FireFox.

![](adding-validation/_static/image7.png)

L’illustration suivante montre comment désactiver JavaScript dans le navigateur Chrome.

![](adding-validation/_static/image8.png)

Voici le *Create.cshtml* afficher le modèle qui vous structuré précédemment dans le didacticiel. Il est utilisé par les méthodes d’action ci-dessus à la fois pour afficher le formulaire initial et pour le réafficher en cas d’erreur.

[!code-cshtml[Main](adding-validation/samples/sample6.cshtml?highlight=16-17)]

Notez comment le code utilise un `Html.EditorFor` helper pour sortir le `<input>` élément pour chaque `Movie` propriété. En regard de ce programme d’assistance est un appel à la `Html.ValidationMessageFor` méthode d’assistance. Ces deux méthodes d’assistance fonctionnent avec l’objet de modèle qui est passé par le contrôleur à la vue (dans ce cas, un `Movie` objet). Ils recherchent automatiquement les attributs de validation spécifiés sur les messages d’erreur de modèle et l’affichage comme il convient.

Le grand avantage de cette approche est que ni le contrôleur ni le modèle de vue `Create` ne savent rien des règles de validation appliquées ou des messages d’erreur affichés. Les règles de validation et les chaînes d’erreur sont spécifiées uniquement dans la classe `Movie`. Ces mêmes règles de validation sont automatiquement appliquées à la vue `Edit` et à tous les autres modèles de vues que vous pouvez créer et qui modifient votre modèle.

Si vous souhaitez modifier la logique de validation ultérieurement, vous pouvez le faire dans un seul endroit en ajoutant des attributs de validation au modèle (dans cet exemple, le `movie` classe). Vous n’aurez pas à vous soucier des différentes parties de l’application qui pourraient être incohérentes avec la façon dont les règles sont appliquées. Toute la logique de validation sera définie à un seul emplacement et utilisée partout. Le code est ainsi très propre, facile à mettre à jour et évolutif. Et cela signifie que vous respecterez entièrement le *DRY* principe.

## <a name="using-datatype-attributes"></a>Utilisation d’attributs DataType

Ouvrez le fichier *Movie.cs* et examinez la classe `Movie`. Le [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) espace de noms fournit des attributs de mise en forme en plus de l’ensemble intégré d’attributs de validation. Nous avons déjà appliqué un [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) valeur d’énumération pour la date de publication et les champs de prix. Le code suivant illustre la `ReleaseDate` et `Price` propriétés avec le bon [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attribut.

[!code-csharp[Main](adding-validation/samples/sample7.cs)]

Le [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attributs fournissent uniquement des conseils pour le moteur d’affichage pour mettre en forme les données (et fournissent des attributs tels que `<a>` pour les URL et `<a href="mailto:EmailAddress.com">` pour le courrier électronique. Vous pouvez utiliser la [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) attribut à valider le format des données. Le [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attribut est utilisé pour spécifier un type de données qui est plus spécifique que le type intrinsèque de la base de données, ils sont ***pas*** les attributs de validation. Dans le cas présent, nous voulons uniquement effectuer le suivi de la date, pas de la date et de l’heure. Le [énumération DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) fournit de nombreux types de données, tel que *Date, heure, PhoneNumber, Currency, EmailAddress* et bien plus encore. L’attribut `DataType` peut également permettre à l’application de fournir automatiquement des fonctionnalités propres au type. Par exemple, un `mailto:` lien peut être créé pour [DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx), et un sélecteur de date peut être fourni pour [DataType.Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) dans les navigateurs qui prennent en charge [HTML5](http://html5.org/). Le [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) émet des attributs HTML 5 [data -](http://ejohn.org/blog/html-5-data-attributes/) (prononcé *data tiret*) attributs navigateurs HTML 5. Le [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attributs ne fournissent pas de toute opération de validation.

`DataType.Date` ne spécifie pas le format de la date qui s’affiche. Par défaut, le champ de données est affiché conformément aux formats par défaut basés sur le serveur [CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx).

L’attribut `DisplayFormat` est utilisé pour spécifier explicitement le format de date :


[!code-csharp[Main](adding-validation/samples/sample8.cs)]


Le `ApplyFormatInEditMode` paramètre spécifie que la mise en forme spécifiée doit également être appliquée quand la valeur est affichée dans une zone de texte pour la modification. (Ceci ne peut pas être souhaitable pour certains champs, par exemple, pour les valeurs monétaires, vous ne pouvez pas vouloir le symbole monétaire dans la zone de texte pour modification.)

Vous pouvez utiliser la [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attribut par lui-même, mais il est généralement une bonne idée d’utiliser le [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attribut également. Le `DataType` attribut transmet le *sémantique* des données en opposition aux comment restituer sur un écran et offre les avantages suivants que vous ne bénéficiez pas avec `DisplayFormat`:

- Le navigateur peut activer des fonctionnalités HTML5 (par exemple afficher un contrôle calendrier, le symbole monétaire correspondant aux paramètres régionaux, des liens de messagerie, etc.).
- Par défaut, le navigateur affiche les données à l’aide du format correspondant sur votre [paramètres régionaux](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- Le [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attribut peut permettre à MVC de choisir le modèle de champ de droite pour afficher les données (le [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) si utilisé par lui-même utilise le modèle de chaîne). Pour plus d’informations, consultez de Brad Wilson [modèles ASP.NET MVC 2](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (Bien qu’écrit pour MVC 2, cet article concerne toujours vers la version actuelle d’ASP.NET MVC.)

Si vous utilisez le `DataType` attribut avec un champ de date, vous devez spécifier le `DisplayFormat` attribut également afin de garantir que le champ s’affiche correctement dans les navigateurs de Chrome. Pour plus d’informations, consultez [ce thread Stack Overflow](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

> [!NOTE]
> validation jQuery ne fonctionne pas avec le [plage](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) attribut et [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). Par exemple, le code suivant affiche toujours une erreur de validation côté client, même quand la date se trouve dans la plage spécifiée :
> 
> [!code-csharp[Main](adding-validation/samples/sample9.cs)]
> 
> Vous devez désactiver la validation de date jQuery pour utiliser le [plage](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) attribut avec [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). Il n’est généralement pas une bonne pratique de compiler des dates en dur dans vos modèles, à l’aide de la [plage](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) attribut et [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx) est déconseillée.


Le code suivant illustre la combinaison d’attributs sur une seule ligne :

[!code-csharp[Main](adding-validation/samples/sample10.cs?highlight=6,10)]

Dans la partie suivante de la série, nous allons examiner l’application et apporter des améliorations aux méthodes `Details` et `Delete` générées automatiquement.

> [!div class="step-by-step"]
> [Précédent](adding-a-new-field.md)
> [Suivant](examining-the-details-and-delete-methods.md)
