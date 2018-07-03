---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'Déploiement de Web ASP.NET à l’aide de Visual Studio : déploiement d’une mise à jour de la base de données | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer (publier) un ASP.NET web application dans Azure App Service Web Apps ou à un fournisseur d’hébergement tiers, en utilisant des éléments...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 37b996452dfa619ba1276a1aba562ed7efc579b5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389187"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a><span data-ttu-id="9e798-103">Déploiement de Web ASP.NET à l’aide de Visual Studio : déploiement d’une mise à jour de la base de données</span><span class="sxs-lookup"><span data-stu-id="9e798-103">ASP.NET Web Deployment using Visual Studio: Deploying a Database Update</span></span>
====================
<span data-ttu-id="9e798-104">par [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="9e798-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="9e798-105">Télécharger le projet de démarrage</span><span class="sxs-lookup"><span data-stu-id="9e798-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="9e798-106">Cette série de didacticiels vous montre comment déployer (publier) un ASP.NET web application dans Azure App Service Web Apps ou à un fournisseur d’hébergement tiers, à l’aide de Visual Studio 2012 ou Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="9e798-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="9e798-107">Pour plus d’informations sur la série, consultez [le premier didacticiel de la série](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="9e798-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="9e798-108">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="9e798-108">Overview</span></span>

<span data-ttu-id="9e798-109">Dans ce didacticiel, vous apportez une modification de la base de données et les modifications de code connexe, testez les modifications dans Visual Studio, puis déployer la mise à jour pour les environnements de test, intermédiaire et de production.</span><span class="sxs-lookup"><span data-stu-id="9e798-109">In this tutorial, you make a database change and related code changes, test the changes in Visual Studio, then deploy the update to the test, staging, and production environments.</span></span>

<span data-ttu-id="9e798-110">Tout d’abord, le didacticiel indique comment mettre à jour une base de données qui est géré par les Migrations Code First et plus tard elle explique ensuite comment mettre à jour une base de données en utilisant le fournisseur dbDacFx.</span><span class="sxs-lookup"><span data-stu-id="9e798-110">The tutorial first shows how to update a database that is managed by Code First Migrations, and then later it shows how to update a database by using the dbDacFx provider.</span></span>

<span data-ttu-id="9e798-111">Rappel : Si vous obtenez un message d’erreur ou quelque chose ne fonctionne pas lorsque vous parcourez le didacticiel, veillez à consulter le [page Résolution des problèmes](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="9e798-111">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a><span data-ttu-id="9e798-112">Déployer une mise à jour de la base de données à l’aide de Code First Migrations</span><span class="sxs-lookup"><span data-stu-id="9e798-112">Deploy a database update by using Code First Migrations</span></span>

<span data-ttu-id="9e798-113">Dans cette section, vous ajoutez une colonne de date de naissance pour le `Person` classe de base pour le `Student` et `Instructor` entités.</span><span class="sxs-lookup"><span data-stu-id="9e798-113">In this section, you add a birth date column to the `Person` base class for the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="9e798-114">Puis vous mettez à jour la page qui affiche les données de l’instructeur afin qu’il affiche la nouvelle colonne.</span><span class="sxs-lookup"><span data-stu-id="9e798-114">Then you update the page that displays instructor data so that it displays the new column.</span></span> <span data-ttu-id="9e798-115">Enfin, vous déployez les modifications de test, intermédiaire et de production.</span><span class="sxs-lookup"><span data-stu-id="9e798-115">Finally, you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-application-database"></a><span data-ttu-id="9e798-116">Ajouter une colonne à une table dans la base de données d’application</span><span class="sxs-lookup"><span data-stu-id="9e798-116">Add a column to a table in the application database</span></span>

1. <span data-ttu-id="9e798-117">Dans le *ContosoUniversity.DAL* projet, ouvrez *Person.cs* et ajoutez la propriété suivante à la fin de la `Person` classe (doit être deux accolades suivant) :</span><span class="sxs-lookup"><span data-stu-id="9e798-117">In the *ContosoUniversity.DAL* project, open *Person.cs* and add the following property at the end of the `Person` class (there should be two closing curly braces following it):</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    <span data-ttu-id="9e798-118">Ensuite, mettez à jour le `Seed` méthode afin qu’elle fournit une valeur pour la nouvelle colonne.</span><span class="sxs-lookup"><span data-stu-id="9e798-118">Next, update the `Seed` method so that it provides a value for the new column.</span></span> <span data-ttu-id="9e798-119">Ouvrez *migrations\configuration.cs* et remplacez le bloc de code qui commence `var instructors = new List<Instructor>` avec le bloc de code suivant, qui inclut des informations de date de naissance :</span><span class="sxs-lookup"><span data-stu-id="9e798-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes birth date information:</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. <span data-ttu-id="9e798-120">Générez la solution, puis ouvrez le **Console du Gestionnaire de Package** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="9e798-120">Build the solution, and then open the **Package Manager Console** window.</span></span> <span data-ttu-id="9e798-121">Assurez-vous que ContosoUniversity.DAL est toujours sélectionné comme le **projet par défaut**.</span><span class="sxs-lookup"><span data-stu-id="9e798-121">Make sure that ContosoUniversity.DAL is still selected as the **Default project**.</span></span>
3. <span data-ttu-id="9e798-122">Dans le **Console du Gestionnaire de Package** fenêtre, sélectionnez **ContosoUniversity.DAL** en tant que le **projet par défaut**, puis entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="9e798-122">In the **Package Manager Console** window, select **ContosoUniversity.DAL** as the **Default project**, and then enter the following command:</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    <span data-ttu-id="9e798-123">Une fois cette commande terminée, Visual Studio ouvre le fichier de classe qui définit la nouvelle `DbMIgration` (classe), puis, dans le `Up` (méthode), vous pouvez voir le code qui crée la nouvelle colonne.</span><span class="sxs-lookup"><span data-stu-id="9e798-123">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` class, and in the `Up` method you can see the code that creates the new column.</span></span> <span data-ttu-id="9e798-124">Le `Up` méthode crée la colonne lorsque vous implémentez la modification et le `Down` méthode supprime la colonne lorsque vous restaurez la modification.</span><span class="sxs-lookup"><span data-stu-id="9e798-124">The `Up` method creates the column when you are implementing the change, and the `Down` method deletes the column when you are rolling back the change.</span></span>

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. <span data-ttu-id="9e798-126">Générez la solution, puis entrez la commande suivante dans le **Console du Gestionnaire de Package** fenêtre (Assurez-vous que le projet ContosoUniversity.DAL est toujours sélectionné) :</span><span class="sxs-lookup"><span data-stu-id="9e798-126">Build the solution, and then enter the following command in the **Package Manager Console** window (make sure the ContosoUniversity.DAL project is still selected):</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    <span data-ttu-id="9e798-127">Entity Framework s’exécute le `Up` (méthode), puis exécute le `Seed` (méthode).</span><span class="sxs-lookup"><span data-stu-id="9e798-127">The Entity Framework runs the `Up` method and then runs the `Seed` method.</span></span>

### <a name="display-the-new-column-in-the-instructors-page"></a><span data-ttu-id="9e798-128">Afficher la nouvelle colonne dans la page des formateurs</span><span class="sxs-lookup"><span data-stu-id="9e798-128">Display the new column in the Instructors page</span></span>

1. <span data-ttu-id="9e798-129">Dans le projet ContosoUniversity, ouvrez *Instructors.aspx* et ajouter un nouveau champ de modèle pour afficher la date de naissance.</span><span class="sxs-lookup"><span data-stu-id="9e798-129">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field to display the birth date.</span></span> <span data-ttu-id="9e798-130">Ajoutez-le entre celles pour l’attribution d’embauche de date et d’office :</span><span class="sxs-lookup"><span data-stu-id="9e798-130">Add it between the ones for hire date and office assignment:</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    <span data-ttu-id="9e798-131">(Si la mise en retrait du code est désynchronisé, vous pouvez appuyer sur CTRL + K, puis sur CTRL + D pour reformater automatiquement le fichier.)</span><span class="sxs-lookup"><span data-stu-id="9e798-131">(If code indentation gets out of sync, you can press CTRL-K and then CTRL-D to automatically reformat the file.)</span></span>
2. <span data-ttu-id="9e798-132">Exécutez l’application et cliquez sur le **formateurs** lien.</span><span class="sxs-lookup"><span data-stu-id="9e798-132">Run the application and click the **Instructors** link.</span></span>

    <span data-ttu-id="9e798-133">Lorsque la page se charge, vous voyez qu’il a le nouveau champ de date de naissance.</span><span class="sxs-lookup"><span data-stu-id="9e798-133">When the page loads, you see that it has the new birth date field.</span></span>

    ![Page des formateurs avec la date de naissance](deploying-a-database-update/_static/image2.png)
3. <span data-ttu-id="9e798-135">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="9e798-135">Close the browser.</span></span>

### <a name="deploy-the-database-update"></a><span data-ttu-id="9e798-136">Déployer la mise à jour de la base de données</span><span class="sxs-lookup"><span data-stu-id="9e798-136">Deploy the database update</span></span>

1. <span data-ttu-id="9e798-137">Dans **l’Explorateur de solutions** sélectionnez le projet ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="9e798-137">In **Solution Explorer** select the ContosoUniversity project.</span></span>
2. <span data-ttu-id="9e798-138">Dans le **publication Web en un clic** barre d’outils, cliquez sur le **Test** le profil de publication, puis cliquez sur **publier le site Web**.</span><span class="sxs-lookup"><span data-stu-id="9e798-138">In the **Web One Click Publish** toolbar, click the **Test** publish profile, and then click **Publish Web**.</span></span> <span data-ttu-id="9e798-139">(Si la barre d’outils est désactivé, sélectionnez le projet ContosoUniversity dans **l’Explorateur de solutions**.)</span><span class="sxs-lookup"><span data-stu-id="9e798-139">(If the toolbar is disabled, select the ContosoUniversity project in **Solution Explorer**.)</span></span>

    <span data-ttu-id="9e798-140">Visual Studio déploie l’application mis à jour, et le navigateur ouvre la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="9e798-140">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
3. <span data-ttu-id="9e798-141">Exécutez le **formateurs** page pour vérifier que la mise à jour a été déployé avec succès.</span><span class="sxs-lookup"><span data-stu-id="9e798-141">Run the **Instructors** page to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="9e798-142">Lorsque l’application tente d’accéder à la base de données pour cette page, Code First met à jour le schéma de base de données et exécute le `Seed` (méthode).</span><span class="sxs-lookup"><span data-stu-id="9e798-142">When the application tries to access the database for this page, Code First updates the database schema and runs the `Seed` method.</span></span> <span data-ttu-id="9e798-143">Lorsque la page s’affiche, vous voyez attendu **Date de naissance** colonne de dates qu’il contient.</span><span class="sxs-lookup"><span data-stu-id="9e798-143">When the page displays, you see the expected **Birth Date** column with dates in it.</span></span>
4. <span data-ttu-id="9e798-144">Dans le **publication Web en un clic** barre d’outils, cliquez sur le **intermédiaire** le profil de publication, puis cliquez sur **publier le site Web**.</span><span class="sxs-lookup"><span data-stu-id="9e798-144">In the **Web One Click Publish** toolbar, click the **Staging** publish profile, and then click **Publish Web**.</span></span>
5. <span data-ttu-id="9e798-145">Exécutez le **formateurs** page dans l’environnement intermédiaire pour vérifier que la mise à jour a été déployé avec succès.</span><span class="sxs-lookup"><span data-stu-id="9e798-145">Run the **Instructors** page in staging to verify that the update was successfully deployed.</span></span>
6. <span data-ttu-id="9e798-146">Dans le **publication Web en un clic** barre d’outils, cliquez sur le **Production** le profil de publication, puis cliquez sur **publier le site Web**.</span><span class="sxs-lookup"><span data-stu-id="9e798-146">In the **Web One Click Publish** toolbar, click the **Production** publish profile, and then click **Publish Web**.</span></span>
7. <span data-ttu-id="9e798-147">Exécutez le **formateurs** page en production pour vérifier que la mise à jour a été déployé avec succès.</span><span class="sxs-lookup"><span data-stu-id="9e798-147">Run the **Instructors** page in production to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="9e798-148">Pour une mise à jour d’application réel de production qui inclut une modification de la base de données que vous prendriez également généralement l’application en mode hors connexion pendant le déploiement à l’aide de *application\_offline.htm*, comme vous l’avez vu dans le didacticiel précédent.</span><span class="sxs-lookup"><span data-stu-id="9e798-148">For a real production application update that includes a database change you would also typically take the application offline during deployment by using *app\_offline.htm*, as you saw in the previous tutorial.</span></span>

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a><span data-ttu-id="9e798-149">Déployer une mise à jour de la base de données en utilisant le fournisseur dbDacFx</span><span class="sxs-lookup"><span data-stu-id="9e798-149">Deploy a database update by using the dbDacFx provider</span></span>

<span data-ttu-id="9e798-150">Dans cette section, vous allez ajouter un *commentaires* colonne à la *utilisateur* de table dans la base de données d’appartenance et de créer une page qui vous permet d’afficher et modifier les commentaires pour chaque utilisateur.</span><span class="sxs-lookup"><span data-stu-id="9e798-150">In this section, you add a *Comments* column to the *User* table in the membership database and create a page that lets you display and edit comments for each user.</span></span> <span data-ttu-id="9e798-151">Vous déployez les modifications sur le test, intermédiaire et de production.</span><span class="sxs-lookup"><span data-stu-id="9e798-151">Then you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-membership-database"></a><span data-ttu-id="9e798-152">Ajouter une colonne à une table dans la base de données d’appartenance</span><span class="sxs-lookup"><span data-stu-id="9e798-152">Add a column to a table in the membership database</span></span>

1. <span data-ttu-id="9e798-153">Dans Visual Studio, ouvrez **Explorateur d’objets SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="9e798-153">In Visual Studio, open **SQL Server Object Explorer**.</span></span>
2. <span data-ttu-id="9e798-154">Développez **(localdb) \v11.0**, développez **bases de données**, développez **aspnet-ContosoUniversity** (pas **aspnet-ContosoUniversity-Prod**) puis développez **Tables**.</span><span class="sxs-lookup"><span data-stu-id="9e798-154">Expand **(localdb)\v11.0**, expand **Databases**, expand **aspnet-ContosoUniversity** (not **aspnet-ContosoUniversity-Prod**) and then expand **Tables**.</span></span>

    <span data-ttu-id="9e798-155">Si vous ne voyez pas **(localdb) \v11.0** sous le **SQL Server** nœud, avec le bouton droit le **SQL Server** nœud et cliquez sur **ajouter SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="9e798-155">If you don't see **(localdb)\v11.0** under the **SQL Server** node, right-click the **SQL Server** node and click **Add SQL Server**.</span></span> <span data-ttu-id="9e798-156">Dans le **se connecter au serveur** boîte de dialogue Entrez *(localdb) \v11.0* en tant que le **nom du serveur**, puis cliquez sur **Connect**.</span><span class="sxs-lookup"><span data-stu-id="9e798-156">In the **Connect to Server** dialog box enter *(localdb)\v11.0* as the **Server name**, and then click **Connect**.</span></span>

    <span data-ttu-id="9e798-157">Si vous ne voyez pas **aspnet-ContosoUniversity**, exécutez le projet et connectez-vous à l’aide la *administrateur* informations d’identification (mot de passe est *devpwd*), puis actualisez le  **Explorateur d’objets SQL Server** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="9e798-157">If you don't see **aspnet-ContosoUniversity**, run the project and log in using the *admin* credentials (password is *devpwd*), and then refresh the **SQL Server Object Explorer** window.</span></span>
3. <span data-ttu-id="9e798-158">Avec le bouton droit le **utilisateurs** table, puis cliquez sur **Concepteur de vues**.</span><span class="sxs-lookup"><span data-stu-id="9e798-158">Right-click the **Users** table, and then click **View Designer**.</span></span>

    ![Concepteur de vue de SSOX](deploying-a-database-update/_static/image3.png)
4. <span data-ttu-id="9e798-160">Dans le concepteur, ajoutez un *commentaires* colonne et le rendre *nvarchar (128)* et nullable, puis cliquez sur **mise à jour**.</span><span class="sxs-lookup"><span data-stu-id="9e798-160">In the designer, add a *Comments* column and make it *nvarchar(128)* and nullable, and then click **Update**.</span></span>

    ![Ajout de la colonne commentaires](deploying-a-database-update/_static/image4.png)
5. <span data-ttu-id="9e798-162">Dans le **mises à jour de la base de données aperçu** , cliquez sur **mise à jour la base de données**.</span><span class="sxs-lookup"><span data-stu-id="9e798-162">In the **Preview Database Updates** box, click **Update Database**.</span></span>

    ![Mises à jour de la base de données en version préliminaire](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a><span data-ttu-id="9e798-164">Créer une page pour afficher et modifier la nouvelle colonne</span><span class="sxs-lookup"><span data-stu-id="9e798-164">Create a page to display and edit the new column</span></span>

1. <span data-ttu-id="9e798-165">Dans **l’Explorateur de solutions**, avec le bouton droit le **compte** cliquez sur le dossier dans le projet ContosoUniversity, **ajouter**, puis cliquez sur **un nouvel élément** .</span><span class="sxs-lookup"><span data-stu-id="9e798-165">In **Solution Explorer**, right-click the **Account** folder in the ContosoUniversity project, click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="9e798-166">Créer un nouveau **Page maître à l’aide de formulaire Web** et nommez-le *UserInfo.aspx*.</span><span class="sxs-lookup"><span data-stu-id="9e798-166">Create a new **Web Form Using Master Page** and name it *UserInfo.aspx*.</span></span> <span data-ttu-id="9e798-167">Acceptez la valeur par défaut *Site.Master* fichier en tant que la page maître.</span><span class="sxs-lookup"><span data-stu-id="9e798-167">Accept the default *Site.Master* file as the master page.</span></span>
3. <span data-ttu-id="9e798-168">Copiez le balisage suivant dans le `MainContent` `Content` élément (la dernière de 3 `Content` éléments) :</span><span class="sxs-lookup"><span data-stu-id="9e798-168">Copy the following markup into the `MainContent` `Content` element (the last of the 3 `Content` elements):</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. <span data-ttu-id="9e798-169">Cliquez sur le *UserInfo.aspx* page et cliquez sur **afficher dans le navigateur**.</span><span class="sxs-lookup"><span data-stu-id="9e798-169">Right-click the *UserInfo.aspx* page and click **View in Browser**.</span></span>
5. <span data-ttu-id="9e798-170">Se connecter avec votre *administrateur* informations d’identification utilisateur (mot de passe est *devpwd*) et ajouter des commentaires à un utilisateur pour vérifier que la page fonctionne correctement.</span><span class="sxs-lookup"><span data-stu-id="9e798-170">Log in with your *admin* user credentials (password is *devpwd*) and add some comments to a user to verify that the page works correctly.</span></span>

    ![Page UserInfo](deploying-a-database-update/_static/image6.png)
6. <span data-ttu-id="9e798-172">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="9e798-172">Close the browser.</span></span>

## <a name="deploy-the-database-update"></a><span data-ttu-id="9e798-173">Déployer la mise à jour de la base de données</span><span class="sxs-lookup"><span data-stu-id="9e798-173">Deploy the database update</span></span>

<span data-ttu-id="9e798-174">Pour déployer à l’aide du fournisseur dbDacFx, vous devez simplement sélectionner la **base de données de mise à jour** option dans le profil de publication.</span><span class="sxs-lookup"><span data-stu-id="9e798-174">To deploy by using the dbDacFx provider, you just need to select the **Update database** option in the publish profile.</span></span> <span data-ttu-id="9e798-175">Toutefois, pour le déploiement initial lorsque vous avez utilisé cette option vous avez également configuré des scripts SQL supplémentaires pour exécuter : ceux se trouvent toujours dans le profil et vous serez obligé de les empêcher de s’exécuter à nouveau.</span><span class="sxs-lookup"><span data-stu-id="9e798-175">However, for the initial deployment when you used this option you also configured some additional SQL scripts to run: those are still in the profile and you'll have to prevent them from running again.</span></span>

1. <span data-ttu-id="9e798-176">Ouvrez le **publier le site Web** Assistant en double-cliquant sur le projet ContosoUniversity en cliquant sur **publier**.</span><span class="sxs-lookup"><span data-stu-id="9e798-176">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="9e798-177">Sélectionnez le **Test** profil.</span><span class="sxs-lookup"><span data-stu-id="9e798-177">Select the **Test** profile.</span></span>
3. <span data-ttu-id="9e798-178">Cliquez sur le **paramètres** onglet.</span><span class="sxs-lookup"><span data-stu-id="9e798-178">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="9e798-179">Sous **DefaultConnection**, sélectionnez **base de données de mise à jour**.</span><span class="sxs-lookup"><span data-stu-id="9e798-179">Under **DefaultConnection**, select **Update database**.</span></span>
5. <span data-ttu-id="9e798-180">Désactiver les scripts supplémentaires que vous avez configuré pour s’exécuter pour le déploiement initial :</span><span class="sxs-lookup"><span data-stu-id="9e798-180">Disable the additional scripts that you configured to run for the initial deployment:</span></span>

    1. <span data-ttu-id="9e798-181">Cliquez sur **configurer des mises à jour de la base de données**.</span><span class="sxs-lookup"><span data-stu-id="9e798-181">Click **Configure database updates**.</span></span>
    2. <span data-ttu-id="9e798-182">Dans le **configurer les mises à jour de base de données** boîte de dialogue, désactivez les cases à cocher à côté *Grant.sql* et *aspnet-data-dev.sql*.</span><span class="sxs-lookup"><span data-stu-id="9e798-182">In the **Configure Database Updates** dialog box, clear the check boxes next to *Grant.sql* and *aspnet-data-dev.sql*.</span></span>
    3. <span data-ttu-id="9e798-183">Cliquez sur **Fermer**.</span><span class="sxs-lookup"><span data-stu-id="9e798-183">Click **Close**.</span></span>
6. <span data-ttu-id="9e798-184">Cliquez sur le **aperçu** onglet.</span><span class="sxs-lookup"><span data-stu-id="9e798-184">Click the **Preview** tab.</span></span>
7. <span data-ttu-id="9e798-185">Sous **bases de données** et à droite de **DefaultConnection**, cliquez sur le **base de données de la version préliminaire** lien.</span><span class="sxs-lookup"><span data-stu-id="9e798-185">Under **Databases** and to the right of **DefaultConnection**, click the **Preview database** link.</span></span>

    ![Aperçu de la base de données](deploying-a-database-update/_static/image7.png)

    <span data-ttu-id="9e798-187">La fenêtre d’aperçu affiche le script qui est exécuté dans la base de données de destination pour rendre ce schéma de base de données correspond au schéma de la base de données source.</span><span class="sxs-lookup"><span data-stu-id="9e798-187">The preview window shows the script that will be run in the destination database to make that database schema match the schema of the source database.</span></span> <span data-ttu-id="9e798-188">Le script inclut une commande ALTER TABLE qui ajoute la nouvelle colonne.</span><span class="sxs-lookup"><span data-stu-id="9e798-188">The script includes an ALTER TABLE command that adds the new column.</span></span>
8. <span data-ttu-id="9e798-189">Fermer le **aperçu de la base de données** boîte de dialogue, puis cliquez sur **publier**.</span><span class="sxs-lookup"><span data-stu-id="9e798-189">Close the **Database Preview** dialog box, and then click **Publish**.</span></span>

    <span data-ttu-id="9e798-190">Visual Studio déploie l’application mis à jour, et le navigateur ouvre la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="9e798-190">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
9. <span data-ttu-id="9e798-191">Exécutez la page UserInfo (ajouter *Account/UserInfo.aspx* à l’URL de la page d’accueil) pour vérifier que la mise à jour a été déployé avec succès.</span><span class="sxs-lookup"><span data-stu-id="9e798-191">Run the UserInfo page (add *Account/UserInfo.aspx* to the home page URL) to verify that the update was successfully deployed.</span></span> <span data-ttu-id="9e798-192">Vous devrez vous connecter en entrant *administrateur* et *devpwd*.</span><span class="sxs-lookup"><span data-stu-id="9e798-192">You'll have to log in by entering *admin* and *devpwd*.</span></span>

    <span data-ttu-id="9e798-193">Données dans les tables ne sont pas déployées par défaut, et vous n’avez configuré un script de déploiement de données à exécuter, donc vous ne trouverez pas le commentaire que vous avez ajouté dans le développement.</span><span class="sxs-lookup"><span data-stu-id="9e798-193">Data in tables is not deployed by default, and you didn't configure a data deployment script to run, so you won't find the comment that you added in development.</span></span> <span data-ttu-id="9e798-194">Vous pouvez ajouter un nouveau commentaire maintenant dans l’environnement intermédiaire pour vérifier que la modification a été déployée sur la base de données et la page fonctionne correctement.</span><span class="sxs-lookup"><span data-stu-id="9e798-194">You can add a new comment now in staging to verify that the change was deployed to the database and the page works correctly.</span></span>
10. <span data-ttu-id="9e798-195">Suivez la même procédure pour déployer sur intermédiaire et de production.</span><span class="sxs-lookup"><span data-stu-id="9e798-195">Follow the same procedure to deploy to staging and production.</span></span>

    <span data-ttu-id="9e798-196">N’oubliez pas de désactiver les scripts supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="9e798-196">Don't forget to disable the extra scripts.</span></span> <span data-ttu-id="9e798-197">La seule différence par rapport au profil de Test est que vous désactivez un seul script dans la mise en lots et les profils de Production, car ils ont été configurés pour s’exécuter uniquement *aspnet-prod-data.sql*.</span><span class="sxs-lookup"><span data-stu-id="9e798-197">The only difference compared to the Test profile is that you will disable only one script in the Staging and Production profiles because they were configured to run only *aspnet-prod-data.sql*.</span></span>

    <span data-ttu-id="9e798-198">Les informations d’identification pour la préproduction et production sont admin et prodpwd.</span><span class="sxs-lookup"><span data-stu-id="9e798-198">The credentials for staging and production are admin and prodpwd.</span></span>

    <span data-ttu-id="9e798-199">Pour une mise à jour d’application réel de production qui inclut une modification de la base de données que vous prendriez également généralement l’application en mode hors connexion pendant le déploiement en téléchargeant *application\_offline.htm* avant la publication et en la supprimant par la suite, comme vous l’avez vu dans [le didacticiel précédent](deploying-a-code-update.md).</span><span class="sxs-lookup"><span data-stu-id="9e798-199">For a real production application update that includes a database change you would also typically take the application offline during deployment by uploading *app\_offline.htm* before publishing and deleting it afterward, as you saw in [the previous tutorial](deploying-a-code-update.md).</span></span>

## <a name="summary"></a><span data-ttu-id="9e798-200">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="9e798-200">Summary</span></span>

<span data-ttu-id="9e798-201">Vous avez maintenant déployé une mise à jour d’application qui comportait une modification de la base de données à l’aide des Migrations Code First et le fournisseur dbDacFx.</span><span class="sxs-lookup"><span data-stu-id="9e798-201">You've now deployed an application update that included a database change using both Code First Migrations and the dbDacFx provider.</span></span>

![Page des formateurs avec la date de naissance](deploying-a-database-update/_static/image8.png)

![Page UserInfo](deploying-a-database-update/_static/image9.png)

<span data-ttu-id="9e798-204">Le didacticiel suivant vous montre comment exécuter des déploiements à l’aide de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="9e798-204">The next tutorial shows you how to execute deployments by using the command line.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9e798-205">[Précédent](deploying-a-code-update.md)
> [Suivant](command-line-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="9e798-205">[Previous](deploying-a-code-update.md)
[Next](command-line-deployment.md)</span></span>
