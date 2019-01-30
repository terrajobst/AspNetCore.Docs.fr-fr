---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'Tutoriel : Créer le l’Application Web et les modèles de données pour Entity Framework Database First avec ASP.NET MVC'
description: Ce didacticiel se concentre sur la création de l’application web et la génération de modèles de données basés sur vos tables de base de données.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: dced55386c3f810e406c5c2b3f0071b45e3b2dbd
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236365"
---
# <a name="tutorial-create-the-the-web-application-and-data-models-for-ef-database-first-with-aspnet-mvc"></a>Tutoriel : Créer le l’Application Web et les modèles de données pour Entity Framework Database First avec ASP.NET MVC

 À l’aide de la structure ASP.NET MVC et Entity Framework, vous pouvez créer une application web qui fournit une interface à une base de données existante. Cette série de didacticiels vous montre comment générer du code qui permet aux utilisateurs d’afficher, modifier, créer et supprimer automatiquement les données qui résident dans une table de base de données. Le code généré correspond aux colonnes dans la table de base de données.

Ce didacticiel se concentre sur la création de l’application web et la génération de modèles de données basés sur vos tables de base de données.

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Créer une application web ASP.NET
> * Générer les modèles

## <a name="prerequisites"></a>Prérequis

* [Bien démarrer avec Entity Framework 6 Database First avec MVC 5](setting-up-database.md)

## <a name="create-an-aspnet-web-app"></a>Créer une application web ASP.NET

Dans une nouvelle solution ou la même solution que le projet de base de données, créez un nouveau projet dans Visual Studio et sélectionnez le **Application Web ASP.NET** modèle. Nommez le projet **ContosoSite**.

![créer le projet](creating-the-web-application/_static/image1.png)

Cliquez sur **OK**.

Dans la fenêtre Nouveau projet ASP.NET, sélectionnez le **MVC** modèle. Vous pouvez effacer le **hôte dans le cloud** option pour l’instant, car vous allez déployer l’application vers le cloud plus tard. Cliquez sur **OK** pour créer l’application.

Le projet est créé avec les fichiers par défaut et les dossiers.

Dans ce didacticiel, vous allez utiliser Entity Framework 6. Vous pouvez vérifier la version d’Entity Framework dans votre projet via la fenêtre Gérer les Packages NuGet. Si nécessaire, mettez à jour votre version d’Entity Framework.

![Affiche la version](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a>Générer les modèles

Vous allez maintenant créer des modèles Entity Framework à partir des tables de base de données. Ces modèles sont des classes que vous allez utiliser pour travailler avec les données. Chaque modèle reflète une table dans la base de données et contient des propriétés qui correspondent aux colonnes dans la table.

Avec le bouton droit le **modèles** dossier, puis sélectionnez **ajouter** et **un nouvel élément**.

Dans la fenêtre Ajouter un nouvel élément, sélectionnez **données** dans le volet gauche et **ADO.NET Entity Data Model** parmi les options dans le volet central. Nommez le nouveau fichier de modèle **ContosoModel**.

Cliquez sur **Ajouter**.

Dans l’Assistant Entity Data Model, sélectionnez **Entity Framework Designer à partir de la base de données**.

Cliquez sur **Suivant**.

Si vous avez des connexions de base de données définies dans votre environnement de développement, vous pouvez voir une de ces connexions présélectionnées. Toutefois, vous souhaitez créer une nouvelle connexion à la base de données que vous avez créé dans la première partie de ce didacticiel. Cliquez sur le **nouvelle connexion** bouton.

Dans la fenêtre Propriétés de connexion, indiquez le nom du serveur local où votre base de données a été créé (dans ce cas **(localdb) \Projects13**). Après avoir entré le nom du serveur, sélectionnez le ContosoUniversityData dans les bases de données disponibles.

![définir les propriétés de connexion](creating-the-web-application/_static/image8.png)

Cliquez sur **OK**.

Les propriétés de connexion correctes sont maintenant affichées. Vous pouvez utiliser le nom par défaut pour la connexion dans le fichier Web.Config.

Cliquez sur **Suivant**.

Sélectionnez la dernière version d’Entity Framework.

Cliquez sur **Suivant**.

Sélectionnez **Tables** pour générer des modèles pour les trois tables.

Cliquez sur **Terminer**.

Si vous recevez un avertissement de sécurité, sélectionnez **OK** pour continuer l’exécution du modèle.

Les modèles sont générés à partir des tables de base de données, et un diagramme s’affiche et montre les propriétés et les relations entre les tables.

![diagramme du modèle](creating-the-web-application/_static/image11.png)

Le dossier de modèles inclut désormais les nombreux nouveaux fichiers associés pour les modèles qui ont été générés à partir de la base de données.

Le **ContosoModel.Context.cs** fichier contient une classe qui dérive de la **DbContext** classe et fournit une propriété pour chaque classe de modèle qui correspond à une table de base de données. Le **Course.cs**, **Enrollment.cs**, et **Student.cs** fichiers contiennent les classes de modèle qui représentent les tables de bases de données. Lorsque vous travaillez avec la structure, vous allez utiliser la classe de contexte et les classes de modèle.

Avant de poursuivre ce didacticiel, générez le projet. Dans la section suivante, vous allez générer du code basé sur les modèles de données, mais cette section ne fonctionnera pas si le projet n’a pas été créé.

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Créé une application web ASP.NET
> * Généré les modèles

Passez au didacticiel suivant pour apprendre à créer génère du code basé sur les modèles de données.
> [!div class="nextstepaction"]
> [Génération de vues](generating-views.md)