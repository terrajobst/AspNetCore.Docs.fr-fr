---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: 'Déploiement d’une Application de Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : déploiement d’une mise à jour de base de données - 9 12 | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer une ASP.NET (publier) projet d’application web qui inclut une base de données SQL Server Compact à l’aide de Visual Stu...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: 5edb3cae785d578674f47f1aa3d5be7dfa6a8791
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822093"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a>Déploiement d’une Application de Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : déploiement d’une mise à jour de base de données - 9 de 12
====================
par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet de démarrage](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Cette série de didacticiels vous montre comment déployer une ASP.NET (publier) projet d’application web qui inclut une base de données SQL Server Compact à l’aide de Visual Studio 2012 RC ou Visual Studio Express 2012 RC pour le Web. Vous pouvez également utiliser Visual Studio 2010 si vous installez la mise à jour de publication Web. Pour une introduction à la série, consultez [le premier didacticiel de la série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Pour obtenir un didacticiel qui montre les fonctionnalités de déploiement introduites après la version RC de Visual Studio 2012, montre comment déployer des éditions de SQL Server autre que SQL Server Compact et montre comment déployer dans Azure App Service Web Apps, consultez [déploiement Web ASP.NET à l’aide de Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Vue d'ensemble

Dans ce didacticiel, vous apportez une modification de la base de données et les modifications de code connexe, testez les modifications dans Visual Studio, puis déployer la mise à jour dans les environnements de test et de production.

Rappel : Si vous obtenez un message d’erreur ou quelque chose ne fonctionne pas lorsque vous parcourez le didacticiel, veillez à consulter le [page Résolution des problèmes](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Ajout d’une nouvelle colonne à une Table

Dans cette section, vous ajoutez une colonne de date de naissance pour le `Person` classe de base pour le `Student` et `Instructor` entités. Puis vous mettez à jour la page qui affiche les données de l’instructeur afin qu’il affiche la nouvelle colonne.

Dans le *ContosoUniversity.DAL* projet, ouvrez *Person.cs* et ajoutez la propriété suivante à la fin de la `Person` classe (doit être deux accolades suivant) :

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

Ensuite, mettez à jour la méthode Seed pour qu’elle fournisse une valeur pour la nouvelle colonne. Ouvrez *migrations\configuration.cs* et remplacez le bloc de code qui commence `var instructors = new List<Instructor>` avec le bloc de code suivant, qui inclut des informations de date de naissance :

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

Dans le projet ContosoUniversity, ouvrez *Instructors.aspx* et ajouter un nouveau champ de modèle pour afficher la date de naissance. Ajoutez-le entre celles pour l’attribution d’embauche de date et d’office :

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

(Si la mise en retrait du code est désynchronisé, vous pouvez appuyer sur CTRL + K, puis sur CTRL + D pour reformater automatiquement le fichier.)

Générez la solution, puis ouvrez le **Console du Gestionnaire de Package** fenêtre. Assurez-vous que ContosoUniversity.DAL est toujours sélectionné comme le **projet par défaut**.

Dans le **Console du Gestionnaire de Package** fenêtre, sélectionnez **ContosoUniversity.DAL** en tant que le **projet par défaut**, puis entrez la commande suivante :

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

Une fois cette commande terminée, Visual Studio ouvre le fichier de classe qui définit la nouvelle `DbMIgration` (classe), puis, dans le `Up` (méthode), vous pouvez voir le code qui crée la nouvelle colonne.

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

Générez la solution, puis entrez la commande suivante dans le **Console du Gestionnaire de Package** fenêtre (Assurez-vous que le projet ContosoUniversity.DAL est toujours sélectionné) :

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

Une fois la commande terminée, exécutez l’application et sélectionnez la page des formateurs. Lorsque la page se charge, vous voyez qu’il a le nouveau champ de date de naissance.

[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Déploiement de la mise à jour de la base de données dans l’environnement de Test

Dans **l’Explorateur de solutions** sélectionnez le projet ContosoUniversity.

Dans le **publication Web en un clic** barre d’outils, sélectionnez le **Test** le profil de publication, puis cliquez sur **publier le site Web**. (Si la barre d’outils est désactivé, sélectionnez le projet ContosoUniversity dans **l’Explorateur de solutions**.)

Visual Studio déploie l’application mis à jour, et le navigateur ouvre la page d’accueil. Exécutez la page des formateurs pour vérifier que la mise à jour a été déployé avec succès. Lorsque l’application tente d’accéder à la base de données pour cette page, Code First met à jour le schéma de base de données et exécute le `Seed` (méthode). Lorsque la page s’affiche, vous voyez attendu **Date de naissance** colonne de dates qu’il contient.

[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Déploiement de la mise à jour de la base de données dans l’environnement de Production

Vous pouvez désormais déployer en production. La seule différence est que vous allez utiliser *application\_offline.htm* pour empêcher les utilisateurs d’accéder au site et par conséquent, la mise à jour la base de données pendant que vous déployez les modifications. Pour le déploiement de production, procédez comme suit :

- Télécharger le *application\_offline.htm* fichier sur le site de production.
- Dans Visual Studio, choisissez le profil de Production dans le **publication Web en un clic** barre d’outils et cliquez sur **publier le site Web**.
- Supprimer le *application\_offline.htm* fichier à partir du site de production.

> [!NOTE]
> Pendant que votre application est en cours d’utilisation dans l’environnement de production vous être devez implémenter un plan de sauvegarde. Autrement dit, vous devez régulièrement copier le *School-Prod.sdf* et *aspnet-Prod.sdf* fichiers à partir de la production de site vers un emplacement de stockage sécurisé et vous devez conserver plusieurs générations de telles sauvegardes. Lorsque vous mettez à jour la base de données, vous devez effectuer une copie de sauvegarde à partir d’immédiatement avant la modification. Ensuite, si vous commettez une erreur et ne détecter qu’une fois que vous l’avez déployée en production, vous serez toujours en mesure de récupérer la base de données à l’état qu'où il se trouvait avant qu’il a été endommagé.


Lorsque Visual Studio ouvre l’URL de la page d’accueil dans le navigateur, le *application\_offline.htm* page s’affiche. Après avoir supprimé le *application\_offline.htm* fichier, que vous pouvez accéder à votre page d’accueil à nouveau pour vérifier que la mise à jour a été déployé avec succès.

[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)

Vous avez maintenant déployé une mise à jour d’application incluant une modification de la base de données de test et de production. Le didacticiel suivant vous montre comment migrer votre base de données à partir de SQL Server Compact vers SQL Server Express et SQL Server.

> [!div class="step-by-step"]
> [Précédent](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
> [Suivant](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
