---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'EF Database First avec ASP.NET MVC : modification de la base de données | Microsoft Docs'
author: tfitzmac
description: À l’aide de la structure ASP.NET MVC et Entity Framework, vous pouvez créer une application web qui fournit une interface à une base de données existante. Ce didacticiel seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 7c9bca87c51bee35be2c5b533916255be80056b0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385432"
---
<a name="ef-database-first-with-aspnet-mvc-changing-the-database"></a>EF Database First avec ASP.NET MVC : modification de la base de données
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> À l’aide de la structure ASP.NET MVC et Entity Framework, vous pouvez créer une application web qui fournit une interface à une base de données existante. Cette série de didacticiels vous montre comment générer du code qui permet aux utilisateurs d’afficher, modifier, créer et supprimer automatiquement les données qui résident dans une table de base de données. Le code généré correspond aux colonnes dans la table de base de données.
> 
> Cette partie de la série se concentre sur effectuer une mise à jour à la structure de base de données et la propagation de cette modification tout au long de l’application web.


## <a name="add-a-column"></a>Ajouter une colonne

Si vous mettez à jour la structure d’une table dans votre base de données, vous devez vous assurer que votre modification est propagée vers le modèle de données, vues et contrôleur.

Pour ce didacticiel, vous allez ajouter une nouvelle colonne à la table Student pour enregistrer le deuxième prénom de l’étudiant. Pour ajouter cette colonne, ouvrez le projet de base de données et ouvrez le fichier Student.sql. Par le biais du concepteur ou dans le code T-SQL, ajoutez une colonne nommée **MiddleName** qui est un nvarchar (50) et autorise les valeurs NULL.

![ajouter le deuxième prénom](changing-the-database/_static/image1.png)

Déployer cette modification à votre base de données locale en démarrant votre projet de base de données (ou F5). Le nouveau champ est ajouté à la table. Si vous ne voyez pas dans l’Explorateur d’objets SQL Server, cliquez sur le bouton Actualiser dans le volet.

![afficher la nouvelle colonne](changing-the-database/_static/image2.png)

La nouvelle colonne existe dans la table de base de données, mais il n’existe pas dans la classe de modèle de données. Vous devez mettre à jour le modèle pour inclure votre nouvelle colonne. Dans le **modèles** dossier, ouvrez le **ContosoModel.edmx** fichier pour afficher le diagramme de modèle. Notez que le modèle Student ne contient pas la propriété MiddleName. Avec le bouton droit n’importe où sur l’aire de conception, puis sélectionnez **modèle de mise à jour à partir de la base de données**.

![modèle de mise à jour](changing-the-database/_static/image3.png)

Dans l’Assistant Mise à jour, sélectionnez le **Actualiser** onglet et le **étudiant** table.

![Assistant Mise à jour](changing-the-database/_static/image4.png)

Cliquez sur **Terminer**.

Une fois le processus de mise à jour est terminé, le schéma de base de données inclut la nouvelle **MiddleName** propriété. Enregistrer le **ContosoModel.edmx** fichier. Vous devez enregistrer ce fichier pour la nouvelle propriété à propager la **Student.cs** classe. Vous avez maintenant mis à jour la base de données et le modèle.

Générez la solution.

Malheureusement, les vues toujours ne contiennent pas la nouvelle propriété. Pour mettre à jour les vues vous avez deux options : vous pouvez soit générer de nouveau les vues en ajoutant une fois encore de génération de modèles automatique pour la classe Student, ou vous pouvez ajouter manuellement la nouvelle propriété à vos affichages existants. Dans ce didacticiel, vous allez ajouter la génération de modèles automatique à nouveau, car vous n’avez pas apporté des modifications personnalisées aux affichages généré automatiquement. Vous pouvez envisager d’ajouter manuellement la propriété lorsque vous avez apporté des modifications aux vues et que vous ne souhaitez pas perdre ces modifications.

Pour vous assurer les vues sont recréées, supprimez le **étudiants** dossier sous **vues**et supprimer le **StudentsController**. Ensuite, cliquez sur le **contrôleurs** dossier et ajouter la génération de modèles automatique pour le **étudiant** modèle. Là encore, nommez le contrôleur **StudentsController**. Sélectionnez **OK**.

Les vues contiennent désormais la propriété MiddleName.

![afficher le deuxième prénom](changing-the-database/_static/image5.png)

Dans la section suivante, vous ajouterez du code pour personnaliser l’affichage de l’affichage des détails d’un enregistrement d’étudiant.

> [!div class="step-by-step"]
> [Précédent](generating-views.md)
> [Suivant](customizing-a-view.md)
