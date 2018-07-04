---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
title: Validation avec les validateurs d’Annotation de données (VB) | Microsoft Docs
author: microsoft
description: Tirez parti du Binder de modèle de Annotation de données pour effectuer la validation au sein d’une application ASP.NET MVC. Découvrez comment utiliser les différents types de programme de validation...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/29/2009
ms.topic: article
ms.assetid: 0d23ff2b-f2ec-434a-be3b-1180beeccba3
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
msc.type: authoredcontent
ms.openlocfilehash: 94410d0e8ad132a4e7a31ac01163663b597c3225
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391416"
---
<a name="validation-with-the-data-annotation-validators-vb"></a>Validation avec les validateurs d’Annotation de données (VB)
====================
par [Microsoft](https://github.com/microsoft)

> Tirez parti du Binder de modèle de Annotation de données pour effectuer la validation au sein d’une application ASP.NET MVC. Découvrez comment utiliser les différents types d’attributs de validateur et de les manipuler dans Microsoft Entity Framework.


Dans ce didacticiel, vous allez apprendre à utiliser les validateurs d’Annotation de données pour effectuer la validation dans une application ASP.NET MVC. L’avantage d’utiliser les validateurs d’Annotation de données est qu’ils vous permettent d’effectuer la validation en ajoutant un ou plusieurs attributs, tels que requis ou attribut StringLength simplement : pour une propriété de classe.

Avant de pouvoir utiliser les validateurs d’Annotation de données, vous devez télécharger le classeur de modèles des Annotations de données. Vous pouvez télécharger l’exemple de classeur du modèle d’Annotations de données depuis le site Web CodePlex en cliquant sur [ici](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).


Il est important de comprendre que le Binder de modèle des Annotations de données n’est pas officiellement partie de l’infrastructure Microsoft ASP.NET MVC. Bien que le Binder de modèle des Annotations de données a été créé par l’équipe Microsoft ASP.NET MVC, Microsoft n’offre pas de prise en charge de produit officielle pour le classeur de modèles des Annotations de données décrit et utilisé dans ce didacticiel.


## <a name="using-the-data-annotation-model-binder"></a>À l’aide du Binder de modèle de données Annotation

Pour pouvoir utiliser le classeur de modèles des Annotations de données dans une application ASP.NET MVC, vous devez d’abord ajouter une référence à l’assembly Microsoft.Web.Mvc.DataAnnotations.dll et le fichier System.ComponentModel.DataAnnotations.dll. Sélectionnez l’option de menu **projet, ajouter une référence**. Cliquez ensuite sur le **Parcourir** onglet et accédez à l’emplacement où vous avez téléchargé (et décompressé) l’exemple de classeur de modèles Annotations de données (voir **Figure 1**).

[![](validation-with-the-data-annotation-validators-vb/_static/image2.png)](validation-with-the-data-annotation-validators-vb/_static/image1.png)

**Figure 1**: ajout d’une référence vers le classeur de modèles de données Annotations ([cliquez pour afficher l’image en taille réelle](validation-with-the-data-annotation-validators-vb/_static/image3.png))

Sélectionnez l’assembly Microsoft.Web.Mvc.DataAnnotations.dll et l’assembly de fichier System.ComponentModel.DataAnnotations.dll, cliquez sur le **OK** bouton.


Vous ne pouvez pas utiliser l’assembly de fichier System.ComponentModel.DataAnnotations.dll inclus avec .NET Framework Service Pack 1 avec le Binder de modèle des Annotations de données. Vous devez utiliser la version de l’assembly de fichier System.ComponentModel.DataAnnotations.dll inclus avec le téléchargement de modèle des Annotations de données, exemple de classeur.


Enfin, vous devez enregistrer le classeur de modèles DataAnnotations dans le fichier Global.asax. Ajoutez la ligne suivante de code à l’Application\_Start() Gestionnaire d’événements afin que l’Application\_méthode Start() ressemble à ceci :

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample1.vb)]

Cette ligne de code enregistre le DataAnnotationsModelBinder en tant que le binder de modèle par défaut pour l’ensemble de l’application ASP.NET MVC.

## <a name="using-the-data-annotation-validator-attributes"></a>En utilisant les attributs de validateur d’Annotation données

Lorsque vous utilisez le classeur de modèles des Annotations de données, vous utilisez les attributs de validateur pour effectuer la validation. L’espace de noms System.ComponentModel.DataAnnotations inclut les attributs du programme de validation suivants :

- Plage : vous permet de valider si la valeur d’une propriété se situe entre une plage de valeurs spécifiée.
- ReqularExpression – permet de valider si la valeur d’une propriété correspond à un modèle d’expression régulière spécifiée.
- Requis : vous permet de marquer une propriété en fonction des besoins.
- StringLength – permet de spécifier une longueur maximale pour une propriété de chaîne.
- Validation : classe de base pour tous les attributs du programme de validation.

> [!NOTE] 
> 
> Si vos besoins de validation ne sont pas satisfaites par un des validateurs standards vous avez toujours la possibilité de créer un attribut de validateur personnalisé en héritant d’un nouvel attribut de validateur de l’attribut de Validation de base.


La classe Product dans **liste 1** illustre l’utilisation de ces attributs de validateur. Les propriétés Name, Description et prix unitaire sont marquées comme requis. La propriété Name doit avoir une longueur de chaîne qui est inférieur à 10 caractères. Enfin, la propriété UnitPrice doit correspondre à un modèle d’expression régulière qui représente un montant en devise.

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample2.vb)]

**Liste 1**: Models\Product.vb

La classe Product illustre comment utiliser un attribut supplémentaire : l’attribut DisplayName. L’attribut DisplayName vous permet de modifier le nom de la propriété lorsque la propriété est affichée dans un message d’erreur. Au lieu d’afficher le message d’erreur « le champ prix unitaire est requis », vous pouvez afficher le message d’erreur « le champ prix est requis ».

> [!NOTE] 
> 
> Si vous souhaitez personnaliser entièrement le message d’erreur affiché par un programme de validation vous pouvez ensuite affecter un message d’erreur personnalisé pour la propriété de message d’erreur du validateur comme suit : `<Required(ErrorMessage:="This field needs a value!")>`


Vous pouvez utiliser la classe Product dans **liste 1** avec l’action de contrôleur Create() dans **Listing 2**. Cette action de contrôleur réaffiche la vue Create lors de l’état du modèle contient des erreurs.

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample3.vb)]

**Listing 2**: Controllers\ProductController.vb

Enfin, vous pouvez créer la vue dans **Listing 3** en double-cliquant sur l’action Create() et en sélectionnant l’option de menu **ajouter une vue**. Créer une vue fortement typée avec la classe Product comme classe de modèle. Sélectionnez **créer** dans la liste de contenu de liste déroulante des vues (consultez **Figure 2**).

[![](validation-with-the-data-annotation-validators-vb/_static/image5.png)](validation-with-the-data-annotation-validators-vb/_static/image4.png)

**Figure 2**: ajout de la vue Create

[!code-aspx[Main](validation-with-the-data-annotation-validators-vb/samples/sample4.aspx)]

**Liste 3**: Views\Product\Create.aspx

> [!NOTE] 
> 
> Supprimer le champ Id du formulaire de création généré par le **ajouter une vue** option de menu. Étant donné que le champ Id correspond à une colonne d’identité, vous ne souhaitez autoriser les utilisateurs à entrer une valeur pour ce champ.


Si vous envoyez le formulaire pour la création d’un produit et vous n’entrez pas de valeurs pour les champs requis, l’erreur de validation des messages dans **Figure 3** sont affichés.

[![](validation-with-the-data-annotation-validators-vb/_static/image7.png)](validation-with-the-data-annotation-validators-vb/_static/image6.png)

**Figure 3**: champs obligatoires sont manquants

Si vous entrez un montant en devise non valide, puis le message d’erreur dans **Figure 4** s’affiche.

[![](validation-with-the-data-annotation-validators-vb/_static/image9.png)](validation-with-the-data-annotation-validators-vb/_static/image8.png)

**Figure 4**: montant en devise non valide

## <a name="using-data-annotation-validators-with-the-entity-framework"></a>À l’aide des validateurs d’annotations de données avec Entity Framework

Si vous utilisez Microsoft Entity Framework pour générer vos classes de modèle de données vous ne pouvez pas appliquer les attributs du validateur directement à vos classes. Étant donné que l’Entity Framework Designer génère les classes de modèle, les modifications apportées aux classes de modèle seront remplacées la prochaine fois que vous apportez des modifications dans le concepteur.

Si vous souhaitez utiliser les validateurs avec les classes générées par Entity Framework vous devez créer des classes de données de métadonnées. Vous appliquez les validateurs à la classe méta de données au lieu d’appliquer les validateurs à la classe réelle.

Par exemple, imaginez que vous avez créé une classe de film à l’aide d’Entity Framework (consultez **Figure 5**). En outre, imaginez que vous souhaitez rendre le titre du film et le directeur propriétés requises de propriétés. Dans ce cas, vous pouvez créer la classe partielle et la classe méta de données dans **liste 4**.

[![](validation-with-the-data-annotation-validators-vb/_static/image11.png)](validation-with-the-data-annotation-validators-vb/_static/image10.png)

**Figure 5**: classe Movie générée par Entity Framework

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample5.vb)]

**Liste 4**: Models\Movie.vb

Le fichier dans **liste 4** contient deux classes nommées film et MovieMetaData. La classe Movie est une classe partielle. Il correspond à la classe partielle générée par Entity Framework qui est contenue dans le fichier DataModel.Designer.vb.

Actuellement, le .NET framework ne prend pas en charge les propriétés partielles. Par conséquent, il n’existe aucun moyen d’appliquer les attributs du programme de validation aux propriétés de la classe Movie définis dans le fichier DataModel.Designer.vb en appliquant les attributs du programme de validation aux propriétés de la classe Movie défini dans le fichier dans **liste 4**.

Notez que la classe partielle du film est décorée avec un attribut MetadataType qui pointe vers la classe MovieMetaData. La classe MovieMetaData contient les propriétés du proxy pour les propriétés de la classe Movie.

Les attributs du programme de validation sont appliqués aux propriétés de la classe MovieMetaData. Les propriétés du titre, le réalisateur et DateReleased sont marquées en tant que propriétés requises. Une chaîne contenant moins de 5 caractères doit être attribuée à la propriété directeur. Enfin, l’attribut DisplayName est appliqué à la propriété DateReleased pour afficher un message d’erreur comme « le champ de Date Released est requis. » au lieu de l’erreur « le champ DateReleased est requis. »

> [!NOTE] 
> 
> Notez que les propriétés de proxy dans la classe MovieMetaData n’avez pas besoin représenter les mêmes types que les propriétés correspondantes dans la classe Movie. Par exemple, la propriété directeur est une propriété de chaîne dans la classe Movie et une propriété d’objet dans la classe MovieMetaData.


La page dans **Figure 6** illustre les messages d’erreur retournés lorsque vous entrez des valeurs non valides pour les propriétés de film.

[![](validation-with-the-data-annotation-validators-vb/_static/image13.png)](validation-with-the-data-annotation-validators-vb/_static/image12.png)

**Figure 6**: à l’aide de validateurs avec Entity Framework ([cliquez pour afficher l’image en taille réelle](validation-with-the-data-annotation-validators-vb/_static/image14.png))

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous avez appris à tirer parti du Binder de modèle de Annotation de données pour effectuer la validation au sein d’une application ASP.NET MVC. Vous avez appris comment utiliser les différents types d’attributs de validateur comme requis et les attributs de StringLength. Vous avez également appris comment utiliser ces attributs lorsque vous travaillez avec Microsoft Entity Framework.

> [!div class="step-by-step"]
> [Précédent](validating-with-a-service-layer-vb.md)
