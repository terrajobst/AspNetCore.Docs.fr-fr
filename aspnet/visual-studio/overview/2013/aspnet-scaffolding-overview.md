---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: Génération de modèles automatique ASP.NET dans Visual Studio 2013 | Microsoft Docs
author: Rick-Anderson
description: Génération de modèles automatique ASP.NET est une nouvelle fonctionnalité qui est incluse dans Visual Studio 2013.
ms.author: riande
ms.date: 04/09/2014
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: 4246a52ad1d10da04a2a214f9dba6a935a9e9e72
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020973"
---
<a name="aspnet-scaffolding-in-visual-studio-2013"></a>Génération de modèles automatique ASP.NET dans Visual Studio 2013
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> Génération de modèles automatique ASP.NET est une nouvelle fonctionnalité qui est incluse dans Visual Studio 2013.


## <a name="overview"></a>Vue d'ensemble

Génération de modèles automatique ASP.NET est une infrastructure de génération de code pour les applications Web ASP.NET. Visual Studio 2013 inclut des générateurs de code préalablement installées pour les projets MVC et API Web. Vous ajoutez la structure à votre projet lorsque vous souhaitez ajouter rapidement du code qui interagit avec les modèles de données. À l’aide de la génération de modèles automatique peut réduire la quantité de temps à développer des opérations de données standard dans votre projet.

Par défaut, Visual Studio 2013 ne prend pas en charge la génération du code pour un projet Web Forms, mais vous pouvez utiliser la structure avec Web Forms en ajoutant des dépendances MVC au projet ou installation d’une extension. Les deux approches sont présentées ci-dessous.

Visual Studio 2013 Update 2 (actuellement RC) offre la possibilité d’étendre la génération de modèles automatique ASP.NET pour satisfaire les exigences de votre scénario. Avec cette fonctionnalité, vous pouvez créer un modèle personnalisé de la structure et ajoutez-le à la boîte de dialogue Ajouter la nouvelle structure. Dans le modèle personnalisé, vous spécifiez le code généré lors de l’ajout d’un élément généré automatiquement. Pour plus d’informations, consultez [création d’un générateur de modèles automatique personnalisé pour Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).

## <a name="prerequisites"></a>Prérequis

Pour utiliser la génération de modèles automatique ASP.NET, vous devez disposer de :

- Microsoft Visual Studio 2013
- Outils de développement Web (partie de l’installation de Visual Studio 2013 par défaut)
- ASP.NET Web Frameworks et outils 2013 (partie de l’installation de Visual Studio 2013 par défaut)

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a>Ajouter un élément généré automatiquement à MVC ou Web API

Pour ajouter une structure, cliquez sur le projet ou un dossier au sein du projet, puis sélectionnez **ajouter** – **nouvel élément structuré**, comme illustré dans l’image suivante.

![Ajouter un élément d’une structure](aspnet-scaffolding-overview/_static/image1.png)

À partir de la **ajouter une structure** fenêtre, sélectionnez le type de structure à ajouter.

![Sélectionnez le type de structure](aspnet-scaffolding-overview/_static/image2.png)

Le **ajouter un contrôleur** fenêtre vous donne la possibilité de sélectionner des options pour générer le contrôleur, notamment si vous souhaitez utiliser les nouvelles fonctionnalités asynchrones à partir d’Entity Framework 6.

![Ajouter un contrôleur](aspnet-scaffolding-overview/_static/image3.png)

Les classes appropriées et les pages sont créées pour votre scénario. Par exemple, l’image suivante montre le contrôleur MVC et les vues qui ont été créés via la structure d’une classe de modèle nommée films.

![Les fichiers créés](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a>Ajouter un élément généré automatiquement à Web Forms

Pour ajouter la génération de modèles automatique qui génère du code Web Forms, vous devez installer une extension pour Visual Studio ou ajouter des dépendances MVC. Les deux approches sont présentées ci-dessous, mais vous ne devez effectuer une de ces approches.

### <a name="web-forms-scaffolding-extension"></a>Extension de génération de modèles automatique de Web Forms

Vous pouvez installer une extension Visual Studio qui vous permettent d’utiliser la structure avec un projet Web Forms. Dans Visual Studio, sélectionnez **outils** , puis **Extensions et mises à jour**. À partir de cette boîte de dialogue Rechercher la galerie Visual Studio pour **génération de modèles automatique Web Forms**.

![installer les formulaires web génération de modèles automatique](aspnet-scaffolding-overview/_static/image5.png)

Pour plus d’informations, consultez [génération de modèles automatique Web Forms](https://go.microsoft.com/fwlink/p/?LinkId=396478).

### <a name="mvc-dependencies"></a>Dépendances MVC

Pour ajouter des dépendances MVC, sélectionnez **ajouter** - **nouvel élément structuré**. Dans la fenêtre Ajouter une structure, sélectionnez **des dépendances MVC**, comme illustré ci-dessous.

![ajouter des dépendances MVC](aspnet-scaffolding-overview/_static/image6.png)

Il existe deux options pour la génération de modèles automatique MVC ; Minimale et complète. Si vous sélectionnez Minimal, uniquement les packages NuGet et les références pour ASP.NET MVC sont ajoutés à votre projet. Si vous sélectionnez l’option complet, les dépendances minimales sont ajoutés, ainsi que les fichiers de contenu requis pour un projet MVC. Pour utiliser facilement la structure, sélectionnez dépendances complètes.

![Sélectionnez les dépendances complètes](aspnet-scaffolding-overview/_static/image7.png)

Après avoir ajouté les dépendances, vous verrez un **readme.txt** fichier. Suivez attentivement les instructions de ce fichier pour vous assurer que votre projet fonctionne correctement.

Lorsque vous avez terminé les étapes décrites dans le fichier readme.txt, vous pouvez ajouter un nouvel élément généré automatiquement, comme indiqué dans la section précédente sur les API Web et MVC. Les vues générées automatiquement et le contrôleur ne fonctionnera correctement au sein de votre projet.

## <a name="tutorials"></a>Didacticiels

Pour créer un générateur de modèles automatique personnalisé, consultez [création d’un générateur de modèles automatique personnalisé pour Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).

Pour personnaliser les fichiers générés, consultez [comment personnaliser les fichiers générés à partir de la boîte de dialogue Nouvel élément structuré](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).

Pour obtenir un exemple d’utilisation de la structure avec **développement Database First**, consultez [EF Database First avec ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).

Pour obtenir un exemple d’utilisation de la structure dans un **MVC** de projet, consultez [mise en route avec ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Pour obtenir un exemple d’utilisation de la structure dans un **API Web** de projet, consultez [créer une API REST avec le routage par attributs dans Web API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).
