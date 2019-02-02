---
uid: mvc/overview/getting-started/database-first-development/setting-up-database
title: 'Tutoriel : Bien démarrer avec Entity Framework Database First avec MVC 5'
description: Ce didacticiel montre comment commencer avec un existant de base de données et de créer rapidement une application web qui permet aux utilisateurs d’interagir avec les données.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 095abad4-3bfe-4f06-b092-ae6a735b7e49
msc.legacyurl: /mvc/overview/getting-started/database-first-development/setting-up-database
msc.type: authoredcontent
ms.openlocfilehash: dfc6c7a7083524a1e7049fdc879fe679f951084d
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667737"
---
# <a name="tutorial-get-started-with-ef-database-first-using-mvc-5"></a>Tutoriel : Bien démarrer avec Entity Framework Database First avec MVC 5

À l’aide de la structure ASP.NET MVC et Entity Framework, vous pouvez créer une application web qui fournit une interface à une base de données existante. Cette série de didacticiels vous montre comment générer du code qui permet aux utilisateurs d’afficher, modifier, créer et supprimer automatiquement les données qui résident dans une table de base de données. Le code généré correspond aux colonnes dans la table de base de données. Dans la dernière partie de la série, vous allez découvrir comment ajouter des annotations de données au modèle de données pour spécifier les exigences de validation et afficher la mise en forme. Lorsque vous avez terminé, vous pouvez passer à un article pour savoir comment déployer une application .NET et la base de données SQL dans Azure App Service sur Azure.

Ce didacticiel montre comment commencer avec un existant de base de données et de créer rapidement une application web qui permet aux utilisateurs d’interagir avec les données. Il utilise l’Entity Framework 6 et MVC 5 pour générer l’application web. La fonctionnalité de génération de modèles automatique ASP.NET vous permet de générer automatiquement le code pour l’affichage, la mise à jour, la création et la suppression de données. Utilisez les outils de publication dans Visual Studio, vous pouvez facilement déployer le site et la base de données vers Azure.

Cette partie de la série se concentre sur la création d’une base de données et le remplir avec des données.

Cette série a été écrit avec les contributions de Tom Dykstra et Rick Anderson. Il a été amélioré en se basant sur les commentaires des utilisateurs dans la section commentaires.

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Configurer la base de données

## <a name="prerequisites"></a>Prérequis

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)

## <a name="introduction"></a>Introduction

Ce didacticiel traite de la situation où vous avez une base de données et que vous voulez générer du code pour une application web basée sur les champs de cette base de données. Cette approche est appelée développement Database First. Si vous ne disposez pas d’une base de données existante, vous pouvez utiliser à la place une approche de développement Code First qui implique la définition des classes de données et la génération de la base de données à partir des propriétés de classe.

## <a name="set-up-the-database"></a>Configurer la base de données

Pour reproduire l’environnement d’avoir une base de données existante, vous serez tout d’abord créer une base de données avec des données préremplies et puis créer votre application web qui se connecte à la base de données.

Ce didacticiel a été développé à l’aide de la base de données locale. Vous pouvez utiliser un serveur de base de données existant au lieu de la base de données locale, mais selon votre version de Visual Studio et votre type de base de données, tous les outils de données dans Visual Studio ne peuvent pas être pris en charge. Si les outils ne sont pas disponibles pour votre base de données, vous devrez peut-être effectuer certaines des étapes spécifiques à la base de données au sein de la suite de gestion pour votre base de données.

Si vous avez un problème avec les outils de base de données dans votre version de Visual Studio, assurez-vous que vous avez installé la dernière version des outils de base de données. Pour plus d’informations sur la mise à jour ou installation des outils de base de données, consultez [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).

Lancez Visual Studio et créez un **projet de base de données SQL Server**. Nommez le projet **ContosoUniversityData**.

![créer le projet de base de données](setting-up-database/_static/image1.png)

Vous disposez maintenant d’un projet de base de données vide. Pour vous assurer que vous pouvez déployer cette base de données vers Azure, vous allez définir la base de données SQL Azure en tant que la plateforme cible pour le projet. Définition de la plateforme cible ne déploie pas réellement la base de données ; Cela signifie simplement que le projet de base de données doit vérifier que la conception de base de données est compatible avec la plateforme cible. Pour définir la plateforme cible, ouvrez le **propriétés** pour le projet, puis sélectionnez **Microsoft Azure SQL Database** pour la plateforme cible.

Vous pouvez créer les tables nécessaires pour ce didacticiel en ajoutant les scripts SQL qui définissent les tables. Cliquez sur votre projet et ajouter un nouvel élément. Sélectionnez **Tables et vues** > **Table** et nommez-le *étudiant*.

Dans le fichier de la table, remplacez la commande T-SQL avec le code suivant pour créer la table.

[!code-sql[Main](setting-up-database/samples/sample1.sql)]

Notez que la fenêtre de conception se synchronise automatiquement avec le code. Vous pouvez travailler avec le code ou le concepteur.

![afficher le code et conception](setting-up-database/_static/image5.png)

Ajouter une autre table. Cette fois, nommez-le cours et utilisez la commande T-SQL suivante.

[!code-sql[Main](setting-up-database/samples/sample2.sql)]

Et bien, répétez le plus de temps pour créer une table nommée d’inscription.

[!code-sql[Main](setting-up-database/samples/sample3.sql)]

Vous pouvez remplir votre base de données via un script qui est exécuté une fois que la base de données est déployée. Ajouter un Script de post-déploiement pour le projet. Cliquez sur votre projet et ajouter un nouvel élément. Sélectionnez **Scripts utilisateur** > **Script de post-déploiement**. Vous pouvez utiliser le nom par défaut.

Ajoutez le code T-SQL suivant pour le script de post-déploiement. Ce script ajoute simplement les données à la base de données lorsqu’aucun enregistrement correspondant n’est trouvé. Il ne pas remplacer ou supprimer des données que vous avez entré dans la base de données.

[!code-sql[Main](setting-up-database/samples/sample4.sql)]

Il est important de noter que le script de post-déploiement est exécuté chaque fois que vous déployez votre projet de base de données. Par conséquent, vous devez soigneusement vos besoins lors de l’écriture de ce script. Dans certains cas, vous pouvez recommencer à partir d’un ensemble connu de données chaque fois que le projet est déployé. Dans d’autres cas, vous souhaiterez pas modifier les données existantes en aucune façon. Selon vos besoins, vous pouvez décider si vous avez besoin d’un script de post-déploiement ou ce que vous devez inclure dans le script. Pour plus d’informations sur le remplissage de votre base de données avec un script de post-déploiement, consultez [, y compris les données dans un projet de base de données SQL Server](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).

Vous disposez maintenant des fichiers de script SQL 4, mais aucune table réelle. Vous êtes prêt à déployer votre projet de base de données à la base de données locale. Dans Visual Studio, cliquez sur le bouton Démarrer (ou F5) pour générer et déployer votre projet de base de données. Vérifier le **sortie** onglet pour vérifier que la génération et le déploiement a réussi.

Pour voir que la nouvelle base de données a été créée, ouvrez **Explorateur d’objets SQL Server** et recherchez le nom du projet dans le serveur de base de données locale appropriée (dans ce cas **(localdb) \ProjectsV13**).

Pour voir que les tables sont remplies avec les données, cliquez sur une table, puis sélectionnez **afficher les données**.

![afficher les données de table](setting-up-database/_static/image9.png)

Une vue modifiable des données de la table s’affiche. Par exemple, si vous sélectionnez **Tables** > **dbo.course** > **afficher les données**, vous voyez une table avec trois colonnes (**cours**, **Titre**, et **crédits**) et quatre lignes.

## <a name="additional-resources"></a>Ressources supplémentaires

Pour obtenir un exemple d’introduction du développement Code First, consultez [mise en route avec ASP.NET MVC 5](../introduction/getting-started.md). Pour un exemple plus avancé, consultez [création d’un modèle de données Entity Framework pour une application ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Pour obtenir des conseils sur la sélection de l’approche Entity Framework à utiliser, consultez [approches de développement Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Configurer la base de données

Passez au didacticiel suivant pour apprendre à créer des modèles d’application et les données du web.
> [!div class="nextstepaction"]
> [Créer des modèles d’application et les données du web](creating-the-web-application.md)
