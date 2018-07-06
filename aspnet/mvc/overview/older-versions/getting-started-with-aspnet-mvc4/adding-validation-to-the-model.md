---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
title: Ajout de la Validation au modèle | Microsoft Docs
author: Rick-Anderson
description: 'Remarque : Une version mise à jour de ce didacticiel est disponible ici qui utilise ASP.NET MVC 5 et Visual Studio 2013. Il est plus sécurisé, beaucoup plus simple à suivre et de démonstration...'
ms.author: aspnetcontent
ms.date: 08/28/2012
ms.assetid: 5d9a2999-fcc4-4c45-a018-271fddf74a3b
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 50911920ed80f09033cfc53a137a6bbba3fc62f3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834489"
---
<a name="adding-validation-to-the-model"></a>Ajout de la Validation au modèle
====================
par [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Une version mise à jour de ce didacticiel est disponible [ici](../../getting-started/introduction/getting-started.md) qui utilise ASP.NET MVC 5 et Visual Studio 2013. Il est plus sécurisé, beaucoup plus simple à suivre et illustre plusieurs fonctionnalités.


Dans cette section, vous allez ajouter la logique de validation pour la `Movie` modèle et vous avez la certitude que les règles de validation sont appliquées chaque fois qu’un utilisateur tente de créer ou modifier un film à l’aide de l’application.

## <a name="keeping-things-dry"></a>Favoriser le sec

Un des principaux les principes de conception d’ASP.NET MVC est sec (&quot;ne vous répétez pas&quot;). ASP.NET MVC vous encourage à spécifier les fonctionnalités ou un comportement qu’une seule fois, puis d’être partout dans une application. Cela réduit la quantité de code, que vous devez écrire et rend le code que vous écrivez moins source d’erreurs et plus facile à gérer.

La prise en charge de la validation fournie par ASP.NET MVC et Entity Framework Code First est un bon exemple du principe DRY en action. Vous pouvez spécifier de façon déclarative des règles de validation au même endroit (dans la classe de modèle) et les règles sont appliquées partout dans l’application.

Examinons comment vous pouvez tirer parti de cette prise en charge de la validation dans l’application de films.

## <a name="adding-validation-rules-to-the-movie-model"></a>Ajout de règles de Validation au modèle Movie

Vous commencerez en ajoutant une logique de validation pour la `Movie` classe.

Ouvrez le fichier *Movie.cs*. Ajouter un `using` instruction en haut du fichier qui fait référence à la [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) espace de noms :

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

Notez l’espace de noms ne contient pas `System.Web`. DataAnnotations fournit un ensemble intégré d’attributs de validation que vous pouvez appliquer de façon déclarative à une classe ou d’une propriété.

Maintenant mettre à jour le `Movie` classe pour tirer parti des prédéfinis [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), et [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) les attributs de validation . Utilisez le code suivant comme exemple de l’emplacement où appliquer les attributs.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs?highlight=4,10,13,17)]

Exécutez l’application et vous obtenez à nouveau l’erreur d’exécution suivante :

***Le modèle soutient le contexte 'MovieDBContext' a changé depuis la création de la base de données. Envisagez d’utiliser les Migrations Code First pour mettre à jour de la base de données ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).***

Nous allons utiliser des migrations pour mettre à jour le schéma. Générez la solution, puis ouvrez le **Console du Gestionnaire de Package** fenêtre et entrez les commandes suivantes :

[!code-console[Main](adding-validation-to-the-model/samples/sample3.cmd)]

Une fois cette commande terminée, Visual Studio ouvre le fichier de classe qui définit la nouvelle `DbMIgration` classe dérivée portant le nom spécifié (*AddDataAnnotationsMig*), puis, dans le `Up` (méthode), vous pouvez voir le code qui met à jour les contraintes du schéma. Le `Title` et `Genre` champs ne sont plus nullables (autrement dit, vous devez entrer une valeur) et le `Rating` champ a une longueur maximale de 5.

Les attributs de validation spécifient le comportement que vous souhaitez appliquer sur les propriétés du modèle sur lesquels ils sont appliqués. Le `Required` attribut indique qu’une propriété doit avoir une valeur ; dans cet exemple, un film doit avoir des valeurs pour le `Title`, `ReleaseDate`, `Genre`, et `Price` propriétés afin de pouvoir être valide. L’attribut `Range` contraint une valeur à une plage spécifiée. L’attribut `StringLength` vous permet de définir la longueur maximale d’une propriété de chaîne, et éventuellement sa longueur minimale. Types intrinsèques (tel que `decimal, int, float, DateTime`) sont obligatoires par défaut et n’avez pas besoin du `Required` attribut.

Code vérifie tout d’abord que les règles de validation que vous spécifiez dans une classe de modèle sont appliquées avant que l’application enregistre les modifications dans la base de données. Par exemple, le code suivant lève une exception lorsque la `SaveChanges` méthode est appelée, car plusieurs requis `Movie` valeurs de propriété sont manquants et le prix est égal à zéro (c'est-à-dire en dehors de la plage valide).

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs?highlight=7-8)]

Les règles de validation appliquées automatiquement par le .NET Framework vous aide à rendre votre application plus robuste. Cela garantit également que vous n’oublierez pas de valider un élément et que vous n’autoriserez pas par inadvertance l’insertion de données incorrectes dans la base de données.

Voici un code complet pour la mise à jour *Movie.cs* fichier :

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Erreur de validation de l’interface utilisateur dans ASP.NET MVC

Exécutez de nouveau l’application et accédez à la */Movies* URL.

Cliquez sur le **créer un nouveau** lien permettant d’ajouter un nouveau film. Remplissez le formulaire avec des valeurs non valides et puis cliquez sur le **créer** bouton.

![8_validationErrors](adding-validation-to-the-model/_static/image1.png)

> [!NOTE]
> pour prendre en charge la validation jQuery pour les paramètres régionaux non anglais qui utilisent une virgule (&quot;,&quot;) pour une décimale, vous devez inclure *globalize.js* et votre propre *cultures/globalize.cultures.js* fichier (à partir de [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) et JavaScript pour utiliser `Globalize.parseFloat`. Le code suivant montre les modifications au fichier Views\Movies\Edit.cshtml pour travailler avec le &quot;fr-FR&quot; culture :


[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

Notez la façon dont le formulaire a utilisé automatiquement une couleur de bordure rouge pour mettre en évidence les zones de texte qui contiennent des données non valides et a émis un message d’erreur de validation approprié en regard de chacun d’eux. Les erreurs sont appliquées à la fois côté client (à l’aide de JavaScript et jQuery) et côté serveur (au cas où un utilisateur aurait désactivé JavaScript).

Un réel avantage est que vous n’avez pas à modifier une seule ligne de code dans le `MoviesController` classe ou dans le *Create.cshtml* vue afin d’activer l’interface utilisateur de cette validation. Le contrôleur et les vues créées précédemment dans ce didacticiel ont détecté les règles de validation que vous avez spécifiées à l’aide des attributs de validation sur les propriétés de la classe de modèle `Movie`.

Vous avez peut-être remarqué pour les propriétés `Title` et `Genre`, l’attribut required n’est pas appliquée tant que vous envoyez le formulaire (appuyez sur la **créer** bouton), ou entrez le texte dans le champ d’entrée et l’a supprimé. Pour un champ qui est initialement vide (par exemple, les champs de la vue Create) et qui a uniquement l’attribut obligatoire et aucun autre attribut de validation, vous pouvez effectuer la commande suivante pour déclencher la validation :

1. Onglet dans le champ.
2. Entrez du texte.
3. Appuyez sur Tab.
4. Onglet dans le champ.
5. Supprimez le texte.
6. Appuyez sur Tab.

La séquence ci-dessus déclenche la validation requise sans s’appuyer sur le bouton Envoyer. Simplement en appuyant sur le bouton Envoyer sans entrer les champs déclenchera la validation côté client. Les données de formulaire ne sont pas envoyées au serveur tant qu’il y a des erreurs de validation côté client. Vous pouvez tester cela en plaçant un point d’arrêt dans la méthode HTTP Post ou en utilisant le [outil fiddler](http://fiddler2.com/fiddler2/) ou Internet Explorer 9 [outils de développement F12](https://msdn.microsoft.com/ie/aa740478).

![](adding-validation-to-the-model/_static/image2.png)

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>La Validation se produit dans la création permet d’afficher et créer la méthode d’Action

Vous vous demandez peut-être comment l’interface utilisateur de validation a été générée sans aucune mise à jour du code dans le contrôleur ou dans les vues. La liste suivante montre à quoi le `Create` méthodes dans la `MovieController` classe ressemble. Ils sont identiques à la façon dont vous les avez créées précédemment dans ce didacticiel.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs?highlight=12,15)]

La première méthode d’action (HTTP GET) `Create` affiche le formulaire de création initial. La deuxième version (`[HttpPost]`) gère la publication de formulaire. La seconde méthode `Create` (la version `HttpPost`) appelle `ModelState.IsValid` pour vérifier si le film a des erreurs de validation. L’appel de cette méthode évalue tous les attributs de validation qui ont été appliqués à l’objet. Si l’objet comporte des erreurs de validation, la méthode `Create` réaffiche le formulaire. S’il n’y a pas d’erreur, la méthode enregistre le nouveau film dans la base de données. Dans notre exemple de film, nous utilisons, **le formulaire n’est pas publié sur le serveur lorsqu’il existe des erreurs de validation détectées côté client ; la seconde** `Create` **méthode n’est jamais appelée**. Si vous désactivez JavaScript dans votre navigateur, la validation côté client est désactivée et la requête HTTP POST `Create` les appels de méthode `ModelState.IsValid` pour vérifier si le film a des erreurs de validation.

Vous pouvez définir un point d’arrêt dans la méthode `HttpPost Create` et vérifier que la méthode n’est jamais appelée. La validation côté client n’enverra pas les données du formulaire quand des erreurs de validation seront détectées. Si vous désactivez JavaScript dans votre navigateur et que vous envoyez ensuite le formulaire avec des erreurs, le point d’arrêt sera atteint. Vous bénéficiez toujours d’une validation complète sans JavaScript. L’image suivante montre comment désactiver JavaScript dans Internet Explorer.

![](adding-validation-to-the-model/_static/image3.png)

![](adding-validation-to-the-model/_static/image4.png)

L’illustration suivante montre comment désactiver JavaScript dans le navigateur FireFox.

![](adding-validation-to-the-model/_static/image5.png)

L’image suivante montre comment désactiver JavaScript avec le navigateur Chrome.

![](adding-validation-to-the-model/_static/image6.png)

Voici le *Create.cshtml* afficher le modèle qui vous structuré précédemment dans le didacticiel. Il est utilisé par les méthodes d’action ci-dessus à la fois pour afficher le formulaire initial et pour le réafficher en cas d’erreur.

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample8.cshtml?highlight=22-23,30-31,38-39,46-47)]

Notez comment le code utilise un `Html.EditorFor` helper pour sortir le `<input>` élément pour chaque `Movie` propriété. En regard de ce programme d’assistance est un appel à la `Html.ValidationMessageFor` méthode d’assistance. Ces deux méthodes d’assistance fonctionnent avec l’objet de modèle qui est passé par le contrôleur à la vue (dans ce cas, un `Movie` objet). Ils recherchent automatiquement les attributs de validation spécifiés sur les messages d’erreur de modèle et l’affichage comme il convient.

Ce qui est vraiment agréable sur cette approche est que le contrôleur, ni le modèle de vue de créer savent rien sur les règles de validation appliquées ou sur les messages d’erreur affichés. Les règles de validation et les chaînes d’erreur sont spécifiées uniquement dans la classe `Movie`. Ces mêmes règles de validation sont automatiquement appliquées à la vue de modification et les autres modèles de vues que vous pouvez créer que modifier votre modèle.

Si vous souhaitez modifier la logique de validation ultérieurement, vous pouvez le faire dans un seul endroit en ajoutant des attributs de validation au modèle (dans cet exemple, le `movie` classe). Vous n’aurez pas à vous soucier des différentes parties de l’application qui pourraient être incohérentes avec la façon dont les règles sont appliquées. Toute la logique de validation sera définie à un seul emplacement et utilisée partout. Le code est ainsi très propre, facile à mettre à jour et évolutif. Et cela signifie que vous respecterez entièrement le principe DRY.

## <a name="adding-formatting-to-the-movie-model"></a>Ajout de mise en forme au modèle Movie

Ouvrez le fichier *Movie.cs* et examinez la classe `Movie`. Le [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) espace de noms fournit des attributs de mise en forme en plus de l’ensemble intégré d’attributs de validation. Nous avons déjà appliqué un [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) valeur d’énumération pour la date de publication et les champs de prix. Le code suivant illustre la `ReleaseDate` et `Price` propriétés avec le bon [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attribut.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

Le [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attributs ne sont pas des attributs de validation, ils sont utilisés pour indiquer le moteur d’affichage comment afficher le HTML. Dans l’exemple ci-dessus, le `DataType.Date` attribut affiche les dates de films en tant que dates uniquement, sans heure. Par exemple, ce qui suit [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attributs ne valident pas le format des données :

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

Les attributs répertoriés ci-dessus fournissent uniquement des conseils pour le moteur d’affichage pour mettre en forme les données (et fournissent des attributs tels que &lt;un&gt; pour les URL et &lt;un href =&quot;mailto:EmailAddress.com&quot; &gt; pour le courrier électronique. Vous pouvez utiliser la [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) attribut à valider le format des données.

Une approche alternative à l’aide du `DataType` attributs, vous pouvez définir explicitement un [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx) valeur. Le code suivant montre la propriété de date de mise en production avec une chaîne de format de date (à savoir, &quot;d&quot;). Vous utiliseriez ceci pour spécifier que vous ne souhaitez pas à temps dans le cadre de la date de publication.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample11.cs)]

L’ensemble `Movie` classe est indiquée ci-dessous.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample12.cs)]

Exécutez l’application et accédez à la `Movies` contrôleur. La date de publication et le prix sont correctement mise en forme. L’image ci-dessous montre la date de publication et à l’aide de prix &quot;fr-FR&quot; que la culture.

![8_format_SM](adding-validation-to-the-model/_static/image7.png)

L’image ci-dessous montre les mêmes données affichées avec la culture par défaut (anglais des États-Unis).

![](adding-validation-to-the-model/_static/image8.png)

Dans la partie suivante de la série, nous allons examiner l’application et apporter des améliorations aux méthodes `Details` et `Delete` générées automatiquement.

> [!div class="step-by-step"]
> [Précédent](adding-a-new-field-to-the-movie-model-and-table.md)
> [Suivant](examining-the-details-and-delete-methods.md)
