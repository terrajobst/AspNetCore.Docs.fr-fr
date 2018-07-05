---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: La mise à jour, la suppression et la création des données avec la liaison de modèle et les web forms | Microsoft Docs
author: tfitzmac
description: Cette série de didacticiels montre les aspects de base de l’utilisation de la liaison de modèle avec un projet Web Forms ASP.NET. Liaison de modèle rend l’interaction des données plus simple-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: b6cafe29d1cb46061a8743cbee62a7ffec6be990
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393874"
---
<a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a>La mise à jour, la suppression et la création des données avec la liaison de modèle et les web forms
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> Cette série de didacticiels montre les aspects de base de l’utilisation de la liaison de modèle avec un projet Web Forms ASP.NET. Liaison de modèle rend interaction des données plus simple que vous traitez des données des objets de source (tels que ObjectDataSource ou SqlDataSource). Cette série commence par une partie introductive et progresse vers des concepts plus avancés dans les didacticiels suivants.
> 
> Ce didacticiel montre comment créer, mettre à jour et supprimer des données avec la liaison de modèle. Vous allez définir les propriétés suivantes :
> 
> - DeleteMethod
> - InsertMethod
> - UpdateMethod
> 
> Ces propriétés reçoivent le nom de la méthode qui gère l’opération correspondante. Dans cette méthode, vous fournissez la logique permettant d’interagir avec les données.
> 
> Ce didacticiel s’appuie sur le projet créé dans la première [partie](retrieving-data.md) de la série.
> 
> Vous pouvez [télécharger](https://go.microsoft.com/fwlink/?LinkId=286116) le projet complet en c# ou VB. Le code téléchargeable fonctionne avec Visual Studio 2012 ou Visual Studio 2013. Elle utilise le modèle Visual Studio 2012, qui est légèrement différent de celle du modèle de Visual Studio 2013 présentée dans ce didacticiel.


## <a name="what-youll-build"></a>Vous allez générer

Dans ce didacticiel, vous allez :

1. Ajouter des modèles de données dynamiques
2. Activer la mise à jour et suppression des données par le biais des méthodes de liaison de modèle
3. Appliquer des règles de validation de données - activer la création d’un nouvel enregistrement dans la base de données

## <a name="add-dynamic-data-templates"></a>Ajouter des modèles de données dynamiques

Pour offrir une expérience utilisateur optimale et réduire la répétition de code, vous allez utiliser des modèles de données dynamiques. Vous pouvez facilement intégrer des modèles de données dynamique préconfigurés dans votre site existant en installant un package NuGet.

À partir de la **gérer les Packages NuGet**, installer le **DynamicDataTemplatesCS**.

![modèles de données dynamiques](updating-deleting-and-creating-data/_static/image1.png)

Notez que votre projet inclut maintenant un dossier nommé **DynamicData**. Dans ce dossier, vous trouverez les modèles qui sont automatiquement appliquées à des contrôles dynamiques dans vos formulaires web.

![dossier de données dynamiques](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a>La mise à jour de l’activation et la suppression

Permettre aux utilisateurs de mettre à jour et supprimer des enregistrements dans la base de données est très similaire au processus de récupération des données. Dans le **UpdateMethod** et **DeleteMethod** propriétés, vous spécifiez les noms des méthodes qui effectuent ces opérations. Avec un contrôle GridView, vous pouvez également spécifier la génération automatique de modifier et supprimer des boutons. Le code en surbrillance suivant montre les ajouts au code GridView.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

Dans le fichier code-behind, ajoutez une instruction pour **System.Data.Entity.Infrastructure**.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

Ensuite, ajouter la mise à jour suivant et supprimer des méthodes.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

Le **TryUpdateModel** méthode s’applique les valeurs liées aux données correspondantes à partir du formulaire web à l’élément de données. L’élément de données est récupéré selon la valeur du paramètre id.

## <a name="enforce-validation-requirements"></a>Appliquer des exigences de validation

Les attributs de validation que vous avez appliqués aux propriétés FirstName, LastName et année dans la classe Student sont automatiquement appliquées lors de la mise à jour les données. Les contrôles DynamicField ajoutent les validateurs de client et serveur selon les attributs de validation. Les propriétés FirstName et LastName sont tous deux requis. Prénom ne peut pas dépasser 20 caractères et LastName ne peut pas dépasser 40 caractères. Année doit être une valeur valide pour l’énumération AcademicYear.

Si l’utilisateur ne respecte pas une des conditions de validation, la mise à jour s’arrête. Pour voir le message d’erreur, ajoutez un contrôle ValidationSummary ci-dessus le GridView. Pour afficher les erreurs de validation à partir de la liaison de modèle, définissez le **ShowModelStateErrors** propriété définie sur **true**. 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

Exécuter l’application web et mettre à jour et supprimer les enregistrements.

![mettre à jour des données](updating-deleting-and-creating-data/_static/image3.png)

Notez que lorsque vous en mode d’édition, la valeur de la propriété de l’année est affichée automatiquement comme une zone de liste déroulante. La propriété « Year » est une valeur d’énumération, et le modèle de données dynamiques pour une valeur d’énumération spécifie une liste déroulante de modification. Vous pouvez trouver ce modèle en ouvrant le **énumération\_Data Edit.ascx** de fichiers dans le **DynamicData**/**FieldTemplates** dossier.

Si vous fournissez des valeurs valides, la mise à jour se termine correctement. Si vous ne respectez pas une des conditions de validation, la mise à jour s’arrête et un message d’erreur s’affiche au-dessus de la grille.

![message d’erreur](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a>Ajouter de nouveaux enregistrements

Le contrôle GridView n’inclut pas le **InsertMethod** propriété et ne peut donc pas être utilisée pour ajouter un nouvel enregistrement avec la liaison de modèle. Vous pouvez trouver la propriété InsertMethod dans le **FormView**, **DetailsView**, ou **ListView** contrôles. Dans ce didacticiel, vous utiliserez un contrôle FormView pour ajouter un nouvel enregistrement.

Tout d’abord, ajoutez un lien vers la nouvelle page, que vous allez créer pour ajouter un nouvel enregistrement. Au-dessus du contrôle ValidationSummary, ajoutez :

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

Le nouveau lien s’affiche en haut du contenu pour la page des étudiants.

![nouveau lien](updating-deleting-and-creating-data/_static/image5.png)

Ensuite, ajoutez un nouveau formulaire web à l’aide d’une page maître et nommez-le **AddStudent**. Sélectionnez Site.Master comme la page maître.

Vous affichera les champs pour l’ajout d’un nouvel étudiant à l’aide un **DynamicEntity** contrôle. Le contrôle de DynamicEntity affiche ces propriétés modifiables dans la classe spécifiée dans la propriété ItemType. La propriété StudentID a été marquée avec la **[ScaffoldColumn(false)]** attribut afin qu’il n’est pas restitué. Dans l’espace réservé de MainContent de la page AddStudent, ajoutez le code suivant.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

Dans le fichier code-behind (AddStudent.aspx.cs), ajoutez un **à l’aide de** instruction pour la **ContosoUniversityModelBinding.Models** espace de noms.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

Ensuite, ajoutez les méthodes suivantes pour spécifier comment insérer un nouvel enregistrement et un gestionnaire d’événements pour le bouton Annuler.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

Enregistrer toutes les modifications.

Exécuter l’application web et créer un nouvel étudiant.

![Ajouter Nouvel étudiant](updating-deleting-and-creating-data/_static/image6.png)

Cliquez sur **insérer** et notez le nouvel étudiant a été créé.

![afficher le nouvel étudiant](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a>Conclusion

Dans ce didacticiel, vous activé la mise à jour, la suppression et la création de données. Vous assurez de règles de validation sont appliquées lors de l’interaction avec les données.

Dans la prochaine [didacticiel](sorting-paging-and-filtering-data.md) dans cette série, vous allez activer le tri, la pagination et filtrage des données.

> [!div class="step-by-step"]
> [Précédent](retrieving-data.md)
> [Suivant](sorting-paging-and-filtering-data.md)
