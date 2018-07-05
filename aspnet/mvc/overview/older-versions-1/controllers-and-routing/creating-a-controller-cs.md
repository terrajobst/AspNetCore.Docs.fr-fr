---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
title: Création d’un contrôleur (c#) | Microsoft Docs
author: StephenWalther
description: Dans ce didacticiel, Stephen Walther montre comment vous pouvez ajouter un contrôleur à une application ASP.NET MVC.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 719d50d4-2305-454c-98b4-bae64937c48f
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
msc.type: authoredcontent
ms.openlocfilehash: 4477418be589af7b9127c7b7373a128a6867bf11
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399436"
---
<a name="creating-a-controller-c"></a>Création d’un contrôleur (c#)
====================
par [Stephen Walther](https://github.com/StephenWalther)

> Dans ce didacticiel, Stephen Walther montre comment vous pouvez ajouter un contrôleur à une application ASP.NET MVC.


L’objectif de ce didacticiel est d’expliquer comment vous pouvez créer nouveau ASP.NET MVC contrôleurs. Vous allez apprendre à créer des contrôleurs à l’aide de l’option de menu de Visual Studio ajouter un contrôleur et en créant un fichier de classe à la main.

### <a name="using-the-add-controller-menu-option"></a>À l’aide de l’ajouter l’Option de Menu de contrôleur

Le moyen le plus simple pour créer un nouveau contrôleur consiste à cliquez sur le dossier contrôleurs dans la fenêtre Explorateur de solutions Visual Studio et sélectionnez le **ajouter, de contrôleur** option de menu (voir Figure 1). Cette option de menu ouvre le **ajouter un contrôleur** boîte de dialogue (voir Figure 2).


[![La boîte de dialogue Nouveau projet](creating-a-controller-cs/_static/image1.jpg)](creating-a-controller-cs/_static/image1.png)

**Figure 01**: ajoutez un nouveau contrôleur ([cliquez pour afficher l’image en taille réelle](creating-a-controller-cs/_static/image2.png))


[![La boîte de dialogue Nouveau projet](creating-a-controller-cs/_static/image2.jpg)](creating-a-controller-cs/_static/image3.png)

**Figure 02**: boîte de dialogue Ajouter un contrôleur The ([cliquez pour afficher l’image en taille réelle](creating-a-controller-cs/_static/image4.png))


Notez que la première partie du nom du contrôleur est mis en surbrillance dans le **ajouter un contrôleur** boîte de dialogue. Chaque nom de contrôleur doit se terminer par le suffixe *contrôleur*. Par exemple, vous pouvez créer un contrôleur nommé *ProductController* mais pas un contrôleur nommé *produit*.


Si vous créez un contrôleur qui manque le *contrôleur* suffixe puis vous ne pourrez pas appeler le contrôleur. Ne le faites pas, j’ai perdu un nombre incalculable d’heures de ma vie après avoir apporté cette erreur.


**Liste 1 - Controllers\ProductController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample1.cs)]

Vous devez toujours créer des contrôleurs dans le dossier contrôleurs. Sinon, vous allez être violer les conventions d’ASP.NET MVC et autres développeurs n’ont plus difficile de comprendre votre application.

### <a name="scaffolding-action-methods"></a>Méthodes d’Action de génération de modèles automatique

Lorsque vous créez un contrôleur, vous avez l’option pour générer automatiquement les méthodes d’action Create, Update et Details (voir Figure 3). Si vous sélectionnez cette option dans la liste 2, la classe de contrôleur est générée.


[![Création automatique de méthodes d’action](creating-a-controller-cs/_static/image3.jpg)](creating-a-controller-cs/_static/image5.png)

**Figure 03**: créent automatiquement les méthodes d’action ([cliquez pour afficher l’image en taille réelle](creating-a-controller-cs/_static/image6.png))


**Listing 2 - Controllers\CustomerController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample2.cs)]

Ces méthodes générées sont des méthodes stub. Vous devez ajouter la logique réelle de la création, la mise à jour et l’affichage des détails pour un client vous-même. Toutefois, les méthodes stub vous fournissent un bon point de départ.

### <a name="creating-a-controller-class"></a>Création d’une classe de contrôleur

Le contrôleur ASP.NET MVC est simplement une classe. Si vous préférez, vous pouvez ignorer l’échafaudage de contrôleur pratique Visual Studio et créez une classe de contrôleur à la main. Procédez comme suit :

1. Cliquez sur le dossier contrôleurs, puis sélectionnez l’option de menu **ajouter, nouvel élément** et sélectionnez le **classe** modèle (voir Figure 4).
2. Nommez la nouvelle classe PersonController.cs et cliquez sur le **ajouter** bouton.
3. Modifiez le fichier résultant de la classe afin que la classe hérite de la classe de base de System.Web.Mvc.Controller (voir Listing 3).


[![Création d’une nouvelle classe](creating-a-controller-cs/_static/image4.jpg)](creating-a-controller-cs/_static/image7.png)

**Figure 04**: création d’une nouvelle classe ([cliquez pour afficher l’image en taille réelle](creating-a-controller-cs/_static/image8.png))


**Liste 3 - Controllers\PersonController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample3.cs)]

Le contrôleur dans le Listing 3 expose une action nommée Index() qui retourne la chaîne « Hello World ! ». Vous pouvez appeler cette action de contrôleur en exécutant votre application et en demandant une URL comme suit :

`http://localhost:40071/Person`

> [!NOTE]
> 
> Le serveur de développement ASP.NET utilise un numéro de port aléatoire (par exemple, 40071). Lorsque vous entrez une URL pour appeler un contrôleur, vous devez fournir le numéro de port de droite. Vous pouvez déterminer le numéro de port en plaçant le curseur de votre souris sur l’icône pour le serveur de développement ASP.NET dans la zone de Notification Windows (en bas à droite de votre écran).
> 
> [!div class="step-by-step"]
> [Précédent](adding-dynamic-content-to-a-cached-page-cs.md)
> [Suivant](creating-an-action-cs.md)
