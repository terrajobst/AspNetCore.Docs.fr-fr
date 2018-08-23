---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-validation-to-the-model
title: Ajout de la Validation au modèle (VB) | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est en cours...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 878f6c31-972d-45f4-8849-5c633b511409
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: d2b4fb9d8f673f223c923ba0705b1ea27d430f43
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834647"
---
<a name="adding-validation-to-the-model-vb"></a>Ajout de la Validation au modèle (VB)
====================
par [Rick Anderson](https://github.com/Rick-Anderson)

> Ce didacticiel vous apprend les notions de base de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est une version gratuite de Microsoft Visual Studio. Avant de commencer, assurez-vous que vous avez installé les composants requis listés ci-dessous. Vous pouvez installer tous les en cliquant sur le lien suivant : [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Vous pouvez également installer individuellement les conditions préalables à l’aide des liens suivants :
> 
> - [Prérequis pour le Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Mettre à jour des outils ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + outils prennent en charge)
> 
> Si vous utilisez Visual Studio 2010 au lieu de Visual Web Developer 2010, installez les conditions préalables en cliquant sur le lien suivant : [configuration requise de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un projet de Visual Web Developer avec le code source VB.NET est disponible pour accompagner cette rubrique. [Téléchargez la version VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si vous préférez c#, basculez vers le [c# version](../cs/adding-validation-to-the-model.md) de ce didacticiel.


Dans cette section, vous allez ajouter la logique de validation pour la `Movie` modèle et vous avez la certitude que les règles de validation sont appliquées chaque fois qu’un utilisateur tente de créer ou modifier un film à l’aide de l’application.

## <a name="keeping-things-dry"></a>Favoriser le sec

Un des principaux les principes de conception d’ASP.NET MVC est DRY (« ne pas se répéter »). ASP.NET MVC vous encourage à spécifier les fonctionnalités ou un comportement qu’une seule fois, puis d’être partout dans une application. Cela réduit la quantité de code, que vous devez écrire et rend le code que vous écrivez beaucoup plus facile à maintenir.

La prise en charge de la validation fournie par ASP.NET MVC et Entity Framework Code First est un bon exemple du principe DRY en action. Vous pouvez spécifier de façon déclarative des règles de validation au même endroit (dans la classe de modèle) et ensuite ces règles sont appliquées partout dans l’application.

Examinons comment vous pouvez tirer parti de cette prise en charge de la validation dans l’application de films.

## <a name="adding-validation-rules-to-the-movie-model"></a>Ajout de règles de Validation au modèle Movie

Vous commencerez en ajoutant une logique de validation pour la `Movie` classe.

Ouvrez le *Movie.vb* fichier. Ajouter un `Imports` instruction en haut du fichier qui fait référence à la [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) espace de noms :

[!code-vb[Main](adding-validation-to-the-model/samples/sample1.vb)]

L’espace de noms fait partie du .NET Framework. Il fournit un ensemble intégré d’attributs de validation que vous pouvez appliquer de façon déclarative à une classe ou d’une propriété.

Maintenant mettre à jour le `Movie` classe pour tirer parti des prédéfinis [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), et [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) les attributs de validation . Utilisez le code suivant comme exemple de l’emplacement où appliquer les attributs.

[!code-vb[Main](adding-validation-to-the-model/samples/sample2.vb)]

Les attributs de validation spécifient le comportement que vous souhaitez appliquer sur les propriétés du modèle sur lesquels ils sont appliqués. Le `Required` attribut indique qu’une propriété doit avoir une valeur ; dans cet exemple, un film doit avoir des valeurs pour le `Title`, `ReleaseDate`, `Genre`, et `Price` propriétés afin de pouvoir être valide. L’attribut `Range` contraint une valeur à une plage spécifiée. L’attribut `StringLength` vous permet de définir la longueur maximale d’une propriété de chaîne, et éventuellement sa longueur minimale.

Code vérifie tout d’abord que les règles de validation que vous spécifiez dans une classe de modèle sont appliquées avant que l’application enregistre les modifications dans la base de données. Par exemple, le code suivant lève une exception lorsque la `SaveChanges` méthode est appelée, car plusieurs requis `Movie` valeurs de propriété sont manquants et le prix est égal à zéro (c'est-à-dire en dehors de la plage valide).

[!code-vb[Main](adding-validation-to-the-model/samples/sample3.vb)]

Les règles de validation appliquées automatiquement par le .NET Framework vous aide à rendre votre application plus robuste. Cela garantit également que vous n’oublierez pas de valider un élément et que vous n’autoriserez pas par inadvertance l’insertion de données incorrectes dans la base de données.

Voici un code complet pour la mise à jour *Movie.vb* fichier :

[!code-vb[Main](adding-validation-to-the-model/samples/sample4.vb)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Erreur de validation de l’interface utilisateur dans ASP.NET MVC

Exécutez de nouveau l’application et accédez à la */Movies* URL.

Cliquez sur le **créer un film** lien permettant d’ajouter un nouveau film. Remplissez le formulaire avec des valeurs non valides et puis cliquez sur le **créer** bouton.

[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)

Notez la façon dont le formulaire a utilisé automatiquement une couleur d’arrière-plan pour mettre en évidence les zones de texte qui contiennent des données non valides et a émis un message d’erreur de validation approprié en regard de chacun d’eux. Les messages d’erreur correspondent aux chaînes erreur que vous avez spécifié lorsque vous annoté la `Movie` classe. Les erreurs sont appliquées (à l’aide de JavaScript) côté client et côté serveur (au cas où un utilisateur aurait désactivé JavaScript).

Un réel avantage est que vous n’avez pas à modifier une seule ligne de code dans le `MoviesController` classe ou dans le *Create.vbhtml* vue afin d’activer l’interface utilisateur de cette validation. Le contrôleur et les vues créées précédemment dans ce didacticiel tombé les règles de validation que vous avez spécifié à l’aide des attributs sur le `Movie` classe de modèle.

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>La Validation se produit dans la création permet d’afficher et créer la méthode d’Action

Vous vous demandez peut-être comment l’interface utilisateur de validation a été générée sans aucune mise à jour du code dans le contrôleur ou dans les vues. La liste suivante montre à quoi le `Create` méthodes dans la `MovieController` classe ressemble. Ils sont identiques à la façon dont vous les avez créées précédemment dans ce didacticiel.

[!code-vb[Main](adding-validation-to-the-model/samples/sample5.vb)]

La première méthode d’action affiche le formulaire de création initial. La seconde gère la publication de formulaire. La seconde `Create` les appels de méthode `ModelState.IsValid` pour vérifier si le film a des erreurs de validation. L’appel de cette méthode évalue tous les attributs de validation qui ont été appliqués à l’objet. Si l’objet comporte des erreurs de validation, le `Create` méthode réaffiche le formulaire. S’il n’y a pas d’erreur, la méthode enregistre le nouveau film dans la base de données.

Voici le *Create.vbhtml* afficher le modèle qui vous structuré précédemment dans le didacticiel. Il est utilisé par les méthodes d’action ci-dessus à la fois pour afficher le formulaire initial et pour le réafficher en cas d’erreur.

[!code-vbhtml[Main](adding-validation-to-the-model/samples/sample6.vbhtml)]

Notez comment le code utilise un `Html.EditorFor` helper pour sortir le `<input>` élément pour chaque `Movie` propriété. En regard de ce programme d’assistance est un appel à la `Html.ValidationMessageFor` méthode d’assistance. Ces deux méthodes d’assistance fonctionnent avec l’objet de modèle qui est passé par le contrôleur à la vue (dans ce cas, un `Movie` objet). Ils recherchent automatiquement les attributs de validation spécifiés sur les messages d’erreur de modèle et l’affichage comme il convient.

Ce qui est vraiment agréable sur cette approche est que le contrôleur, ni le modèle de vue de créer savent rien sur les règles de validation appliquées ou sur les messages d’erreur affichés. Les règles de validation et les chaînes d’erreur sont spécifiées uniquement dans la classe `Movie`.

Si vous souhaitez modifier la logique de validation ultérieurement, vous pouvez le faire dans un seul endroit. Vous n’aurez pas à vous soucier des différentes parties de l’application qui pourraient être incohérentes avec la façon dont les règles sont appliquées. Toute la logique de validation sera définie à un seul emplacement et utilisée partout. Le code est ainsi très propre, facile à mettre à jour et évolutif. Et cela signifie que vous respecterez entièrement le principe DRY.

## <a name="adding-formatting-to-the-movie-model"></a>Ajout de mise en forme au modèle Movie

Ouvrez le *Movie.vb* fichier. Le [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) espace de noms fournit des attributs de mise en forme en plus de l’ensemble intégré d’attributs de validation. Vous allez appliquer le [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attribut et un [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) valeur d’énumération pour la date de publication et les champs de prix. Le code suivant illustre la `ReleaseDate` et `Price` propriétés avec le bon [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attribut.

[!code-vb[Main](adding-validation-to-the-model/samples/sample7.vb)]

Vous pouvez également explicitement définir un [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx) valeur. Le code suivant montre la propriété de date de mise en production avec une chaîne de format de date (à savoir, « d »). Vous utiliseriez ceci pour spécifier que vous ne souhaitez pas à temps dans le cadre de la date de publication.

[!code-vb[Main](adding-validation-to-the-model/samples/sample8.vb)]

Le code suivant formats le `Price` propriété sous forme de devise.

[!code-vb[Main](adding-validation-to-the-model/samples/sample9.vb)]

L’ensemble `Movie` classe est indiquée ci-dessous.

[!code-vb[Main](adding-validation-to-the-model/samples/sample10.vb)]

Exécutez l’application et accédez à la `Movies` contrôleur.

![8_format_SM](adding-validation-to-the-model/_static/image3.png)

Dans la partie suivante de la série, nous allons examiner l’application et apporter des améliorations aux généré automatiquement `Details` et `Delete` méthodes...

> [!div class="step-by-step"]
> [Précédent](adding-a-new-field.md)
> [Suivant](improving-the-details-and-delete-methods.md)
