---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: Tri, la pagination et filtrage des données avec la liaison de modèle et les web forms | Microsoft Docs
author: tfitzmac
description: Cette série de didacticiels montre les aspects de base de l’utilisation de la liaison de modèle avec un projet Web Forms ASP.NET. Liaison de modèle rend l’interaction des données plus simple-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: 7529c811e6196327094f8f735de1bd65be76ee3a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398305"
---
<a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a>Tri, la pagination et filtrage des données avec la liaison de modèle et les web forms
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> Cette série de didacticiels montre les aspects de base de l’utilisation de la liaison de modèle avec un projet Web Forms ASP.NET. Liaison de modèle rend interaction des données plus simple que vous traitez des données des objets de source (tels que ObjectDataSource ou SqlDataSource). Cette série commence par une partie introductive et progresse vers des concepts plus avancés dans les didacticiels suivants.
> 
> Ce didacticiel montre comment ajouter le tri, la pagination et le filtrage des données via la liaison de modèle.
> 
> Ce didacticiel s’appuie sur le projet créé dans la première [partie](retrieving-data.md) de la série.
> 
> Vous pouvez [télécharger](https://go.microsoft.com/fwlink/?LinkId=286116) le projet complet en c# ou VB. Le code téléchargeable fonctionne avec Visual Studio 2012 ou Visual Studio 2013. Elle utilise le modèle Visual Studio 2012, qui est légèrement différent de celle du modèle de Visual Studio 2013 présentée dans ce didacticiel.


## <a name="what-youll-build"></a>Vous allez générer

Dans ce didacticiel, vous allez :

1. Activer le tri et la pagination des données
2. Activer le filtrage des données basée sur une sélection par l’utilisateur

## <a name="add-sorting"></a>Ajouter le tri

Activation du tri dans le contrôle GridView est très facile. Dans le fichier Student.aspx, il suffit de définir **AllowSorting** à **true** dans le contrôle GridView. Vous n’avez pas besoin de définir un **SortExpression** valeur pour chaque colonne que le DataField est automatiquement utilisé. Le contrôle GridView modifie la requête pour inclure l’organisation des données par la valeur sélectionnée. Le code en surbrillance ci-dessous montre l’ajout que vous devez faire activer le tri.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

Exécuter l’application web et tester des enregistrements d’étudiants tri par les valeurs dans des colonnes différentes.

![étudiants de tri](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a>Ajouter la pagination

Il est également très facile de l’activation de la pagination. Dans le contrôle GridView, définissez le **AllowPaging** propriété **true** et définir le **PageSize** propriété pour le nombre d’enregistrements que vous souhaitez afficher sur chaque page. Dans ce didacticiel, vous pouvez la définir à 4.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

Exécuter l’application web et notez que maintenant les enregistrements sont réparties sur plusieurs pages avec pas plus de 4 enregistrements affichés sur une seule page.

![ajouter la pagination](sorting-paging-and-filtering-data/_static/image4.png)

Exécution de requête différée améliore l’efficacité de l’application. Au lieu de récupérer l’ensemble de données, le contrôle GridView modifie la requête pour récupérer uniquement les enregistrements de la page actuelle.

## <a name="filter-records-by-user-selection"></a>Filtrer les enregistrements par sélection de l’utilisateur

Liaison de modèle ajoute plusieurs attributs qui vous permettent de désigner la définition de la valeur pour un paramètre dans une méthode de liaison de modèle. Ces attributs se trouvent dans le **System.Web.ModelBinding** espace de noms. Elles comprennent :

- Contrôle
- Cookie
- Formulaire
- Profil
- QueryString
- RouteData
- Session
- UserProfile
- État d’affichage

Dans ce didacticiel, vous utiliserez une valeur d’un contrôle pour filtrer les enregistrements sont affichés dans le contrôle GridView. Vous allez ajouter le **contrôle** attribut à la méthode de requête que vous avez créé précédemment. Dans un [ultérieurement](using-query-string-values-to-retrieve-data.md) didacticiel, vous allez appliquer le **QueryString** d’attribut à un paramètre pour spécifier que la valeur du paramètre provient d’une valeur de chaîne de requête.

Tout d’abord, au-dessus du contrôle ValidationSummary, ajoutez une liste déroulante de filtrage les étudiants sont affichés.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

Dans le fichier code-behind, modifiez la méthode select pour recevoir une valeur à partir du contrôle et définissez le nom du paramètre sur le nom du contrôle qui fournit la valeur.

Vous devez ajouter un **à l’aide de** instruction pour la **System.Web.ModelBinding** espace de noms à résoudre l’attribut du contrôle.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

Le code suivant montre la méthode select nouveau travaillée pour filtrer les données renvoyées en fonction de la valeur de la liste déroulante. Ajout d’un attribut de contrôle avant un paramètre spécifie que la valeur pour ce paramètre est fourni à partir d’un contrôle portant le même nom.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

Exécuter l’application web et sélectionnez des valeurs différentes dans la liste déroulante pour filtrer la liste d’étudiants.

![étudiants de filtre](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a>Conclusion

Dans ce didacticiel, vous avez activé le tri et la pagination des données. Vous avez activé également le filtrage des données par la valeur d’un contrôle.

Dans la prochaine [didacticiel](integrating-jquery-ui.md) vous allez améliorer l’interface utilisateur en intégrant un widget de JQuery UI dans le modèle de données dynamiques.

> [!div class="step-by-step"]
> [Précédent](updating-deleting-and-creating-data.md)
> [Suivant](integrating-jquery-ui.md)
