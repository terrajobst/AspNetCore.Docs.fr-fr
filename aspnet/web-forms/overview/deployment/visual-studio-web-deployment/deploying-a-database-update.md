---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: "Déploiement de Web ASP.NET à l’aide de Visual Studio : déploiement d’une mise à jour de la base de données | Documents Microsoft"
author: tdykstra
description: "Cette série de didacticiels vous montre comment déployer (publier) ASP.NET web application Azure App Service Web Apps ou un fournisseur d’hébergement tiers, en utilisant des éléments..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 3fd29a5b26c564d88e4128d1904fab00b57a3b7c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a><span data-ttu-id="e81b7-103">Déploiement de Web ASP.NET à l’aide de Visual Studio : déploiement d’une mise à jour de la base de données</span><span class="sxs-lookup"><span data-stu-id="e81b7-103">ASP.NET Web Deployment using Visual Studio: Deploying a Database Update</span></span>
====================
<span data-ttu-id="e81b7-104">Par [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="e81b7-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="e81b7-105">Télécharger le projet de démarrage</span><span class="sxs-lookup"><span data-stu-id="e81b7-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="e81b7-106">Cette série de didacticiels vous montre comment déployer (publier) ASP.NET web application Azure App Service Web Apps ou un fournisseur d’hébergement tiers, à l’aide de Visual Studio 2012 ou Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="e81b7-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="e81b7-107">Pour plus d’informations sur la série, consultez [le premier didacticiel de la série](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e81b7-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="e81b7-108">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="e81b7-108">Overview</span></span>

<span data-ttu-id="e81b7-109">Dans ce didacticiel, vous apportez une modification de la base de données et les modifications de code associé, les modifications de test dans Visual Studio, puis déployer la mise à jour pour les environnements de test, intermédiaire et de production.</span><span class="sxs-lookup"><span data-stu-id="e81b7-109">In this tutorial, you make a database change and related code changes, test the changes in Visual Studio, then deploy the update to the test, staging, and production environments.</span></span>

<span data-ttu-id="e81b7-110">Ce didacticiel illustre d’abord la mettre à jour une base de données qui est géré par les Migrations Code First et ultérieurement il montre ensuite comment mettre à jour une base de données à l’aide du fournisseur dbDacFx.</span><span class="sxs-lookup"><span data-stu-id="e81b7-110">The tutorial first shows how to update a database that is managed by Code First Migrations, and then later it shows how to update a database by using the dbDacFx provider.</span></span>

<span data-ttu-id="e81b7-111">Rappel : Si vous obtenez un message d’erreur, ou quelque chose ne fonctionne pas lorsque vous parcourez le didacticiel, veillez à consulter le [page Résolution des problèmes](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="e81b7-111">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a><span data-ttu-id="e81b7-112">Déployer une mise à jour de la base de données à l’aide de Migrations Code First</span><span class="sxs-lookup"><span data-stu-id="e81b7-112">Deploy a database update by using Code First Migrations</span></span>

<span data-ttu-id="e81b7-113">Dans cette section, vous ajoutez une colonne de date de naissance à la `Person` classe de base pour le `Student` et `Instructor` entités.</span><span class="sxs-lookup"><span data-stu-id="e81b7-113">In this section, you add a birth date column to the `Person` base class for the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="e81b7-114">Puis vous mettez à jour la page qui affiche les données de formateur afin qu’il affiche la nouvelle colonne.</span><span class="sxs-lookup"><span data-stu-id="e81b7-114">Then you update the page that displays instructor data so that it displays the new column.</span></span> <span data-ttu-id="e81b7-115">Enfin, vous déployez les modifications apportées au test, intermédiaire et de production.</span><span class="sxs-lookup"><span data-stu-id="e81b7-115">Finally, you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-application-database"></a><span data-ttu-id="e81b7-116">Ajouter une colonne à une table dans la base de données d’application</span><span class="sxs-lookup"><span data-stu-id="e81b7-116">Add a column to a table in the application database</span></span>

1. <span data-ttu-id="e81b7-117">Dans le *ContosoUniversity.DAL* projet, ouvrez *Person.cs* et ajoutez la propriété suivante à la fin de la `Person` classe (vous devez voir deux accolades suit) :</span><span class="sxs-lookup"><span data-stu-id="e81b7-117">In the *ContosoUniversity.DAL* project, open *Person.cs* and add the following property at the end of the `Person` class (there should be two closing curly braces following it):</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    <span data-ttu-id="e81b7-118">Ensuite, mettez à jour le `Seed` méthode afin qu’elle fournit une valeur pour la nouvelle colonne.</span><span class="sxs-lookup"><span data-stu-id="e81b7-118">Next, update the `Seed` method so that it provides a value for the new column.</span></span> <span data-ttu-id="e81b7-119">Ouvrez *Migrations\Configuration.cs* et remplacez le bloc de code qui commence `var instructors = new List<Instructor>` avec le bloc de code suivant, qui inclut des informations de date de naissance :</span><span class="sxs-lookup"><span data-stu-id="e81b7-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes birth date information:</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. <span data-ttu-id="e81b7-120">Générez la solution, puis ouvrez le **Package Manager Console** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="e81b7-120">Build the solution, and then open the **Package Manager Console** window.</span></span> <span data-ttu-id="e81b7-121">Assurez-vous que ContosoUniversity.DAL est toujours sélectionné en tant que le **projet par défaut**.</span><span class="sxs-lookup"><span data-stu-id="e81b7-121">Make sure that ContosoUniversity.DAL is still selected as the **Default project**.</span></span>
3. <span data-ttu-id="e81b7-122">Dans le **Package Manager Console** fenêtre, sélectionnez **ContosoUniversity.DAL** comme le **projet par défaut**, puis entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="e81b7-122">In the **Package Manager Console** window, select **ContosoUniversity.DAL** as the **Default project**, and then enter the following command:</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    <span data-ttu-id="e81b7-123">Une fois cette commande, Visual Studio ouvre le fichier de classe qui définit la nouvelle `DbMIgration` (classe), puis, dans le `Up` (méthode), vous pouvez voir le code qui crée la nouvelle colonne.</span><span class="sxs-lookup"><span data-stu-id="e81b7-123">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` class, and in the `Up` method you can see the code that creates the new column.</span></span> <span data-ttu-id="e81b7-124">Le `Up` méthode crée la colonne lorsque vous implémentez la modification et le `Down` méthode supprime la colonne lorsque vous restaurez la modification.</span><span class="sxs-lookup"><span data-stu-id="e81b7-124">The `Up` method creates the column when you are implementing the change, and the `Down` method deletes the column when you are rolling back the change.</span></span>

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. <span data-ttu-id="e81b7-126">Générez la solution et entrez la commande suivante dans le **Package Manager Console** fenêtre (Assurez-vous que le projet ContosoUniversity.DAL est toujours sélectionné) :</span><span class="sxs-lookup"><span data-stu-id="e81b7-126">Build the solution, and then enter the following command in the **Package Manager Console** window (make sure the ContosoUniversity.DAL project is still selected):</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    <span data-ttu-id="e81b7-127">Entity Framework exécute le `Up` (méthode), puis exécute le `Seed` (méthode).</span><span class="sxs-lookup"><span data-stu-id="e81b7-127">The Entity Framework runs the `Up` method and then runs the `Seed` method.</span></span>

### <a name="display-the-new-column-in-the-instructors-page"></a><span data-ttu-id="e81b7-128">Afficher la nouvelle colonne dans la page des instructeurs</span><span class="sxs-lookup"><span data-stu-id="e81b7-128">Display the new column in the Instructors page</span></span>

1. <span data-ttu-id="e81b7-129">Dans le projet ContosoUniversity, ouvrez *Instructors.aspx* et ajouter un nouveau champ de modèle pour afficher la date de naissance.</span><span class="sxs-lookup"><span data-stu-id="e81b7-129">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field to display the birth date.</span></span> <span data-ttu-id="e81b7-130">Ajoutez-le entre celles pour l’attribution d’embauche de date et d’office :</span><span class="sxs-lookup"><span data-stu-id="e81b7-130">Add it between the ones for hire date and office assignment:</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    <span data-ttu-id="e81b7-131">(Si la mise en retrait du code est désynchronisé, vous pouvez appuyer sur CTRL-K, puis sur CTRL + D pour automatiquement remettre le fichier.)</span><span class="sxs-lookup"><span data-stu-id="e81b7-131">(If code indentation gets out of sync, you can press CTRL-K and then CTRL-D to automatically reformat the file.)</span></span>
2. <span data-ttu-id="e81b7-132">Exécutez l’application et cliquez sur le **instructeurs** lien.</span><span class="sxs-lookup"><span data-stu-id="e81b7-132">Run the application and click the **Instructors** link.</span></span>

    <span data-ttu-id="e81b7-133">Lors du chargement de la page, vous constatez qu’il comporte le nouveau champ de date de naissance.</span><span class="sxs-lookup"><span data-stu-id="e81b7-133">When the page loads, you see that it has the new birth date field.</span></span>

    ![Page instructeurs avec la date de naissance](deploying-a-database-update/_static/image2.png)
3. <span data-ttu-id="e81b7-135">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="e81b7-135">Close the browser.</span></span>

### <a name="deploy-the-database-update"></a><span data-ttu-id="e81b7-136">Déployer la mise à jour de la base de données</span><span class="sxs-lookup"><span data-stu-id="e81b7-136">Deploy the database update</span></span>

1. <span data-ttu-id="e81b7-137">Dans **l’Explorateur de solutions** sélectionnez le projet ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="e81b7-137">In **Solution Explorer** select the ContosoUniversity project.</span></span>
2. <span data-ttu-id="e81b7-138">Dans le **publication Web en un clic** barre d’outils, cliquez sur le **Test** le profil de publication, puis cliquez sur **publier le site Web**.</span><span class="sxs-lookup"><span data-stu-id="e81b7-138">In the **Web One Click Publish** toolbar, click the **Test** publish profile, and then click **Publish Web**.</span></span> <span data-ttu-id="e81b7-139">(Si la barre d’outils est désactivé, sélectionnez le projet ContosoUniversity dans **l’Explorateur de solutions**.)</span><span class="sxs-lookup"><span data-stu-id="e81b7-139">(If the toolbar is disabled, select the ContosoUniversity project in **Solution Explorer**.)</span></span>

    <span data-ttu-id="e81b7-140">Visual Studio déploie l’application mis à jour, et le navigateur s’ouvre à la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="e81b7-140">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
3. <span data-ttu-id="e81b7-141">Exécutez le **instructeurs** page pour vérifier que la mise à jour a été déployé.</span><span class="sxs-lookup"><span data-stu-id="e81b7-141">Run the **Instructors** page to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="e81b7-142">Lorsque l’application tente d’accéder à la base de données pour cette page, le premier Code met à jour le schéma de base de données et exécute le `Seed` (méthode).</span><span class="sxs-lookup"><span data-stu-id="e81b7-142">When the application tries to access the database for this page, Code First updates the database schema and runs the `Seed` method.</span></span> <span data-ttu-id="e81b7-143">Lorsque la page s’affiche, vous voyez attendu **Date de naissance** colonne de dates qu’il contient.</span><span class="sxs-lookup"><span data-stu-id="e81b7-143">When the page displays, you see the expected **Birth Date** column with dates in it.</span></span>
4. <span data-ttu-id="e81b7-144">Dans le **publication Web en un clic** barre d’outils, cliquez sur le **intermédiaire** le profil de publication, puis cliquez sur **publier le site Web**.</span><span class="sxs-lookup"><span data-stu-id="e81b7-144">In the **Web One Click Publish** toolbar, click the **Staging** publish profile, and then click **Publish Web**.</span></span>
5. <span data-ttu-id="e81b7-145">Exécutez le **instructeurs** page de la zone de transit pour vérifier que la mise à jour a été déployé.</span><span class="sxs-lookup"><span data-stu-id="e81b7-145">Run the **Instructors** page in staging to verify that the update was successfully deployed.</span></span>
6. <span data-ttu-id="e81b7-146">Dans le **publication Web en un clic** barre d’outils, cliquez sur le **Production** le profil de publication, puis cliquez sur **publier le site Web**.</span><span class="sxs-lookup"><span data-stu-id="e81b7-146">In the **Web One Click Publish** toolbar, click the **Production** publish profile, and then click **Publish Web**.</span></span>
7. <span data-ttu-id="e81b7-147">Exécutez le **instructeurs** page en production pour vérifier que la mise à jour a été déployé.</span><span class="sxs-lookup"><span data-stu-id="e81b7-147">Run the **Instructors** page in production to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="e81b7-148">Pour une une mise à jour d’application réelles de production qui inclut une modification de la base de données en général également suivre l’application en mode hors connexion durant le déploiement à l’aide de *application\_offline.htm*, comme vous l’avez vu dans le didacticiel précédent.</span><span class="sxs-lookup"><span data-stu-id="e81b7-148">For a a real production application update that includes a database change you would also typically take the application offline during deployment by using *app\_offline.htm*, as you saw in the previous tutorial.</span></span>

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a><span data-ttu-id="e81b7-149">Déployer une mise à jour de la base de données en utilisant le fournisseur dbDacFx</span><span class="sxs-lookup"><span data-stu-id="e81b7-149">Deploy a database update by using the dbDacFx provider</span></span>

<span data-ttu-id="e81b7-150">Dans cette section, vous ajoutez un *commentaires* colonne à la *utilisateur* de table dans la base de données d’appartenance et de créer une page qui vous permet d’afficher et modifier les commentaires pour chaque utilisateur.</span><span class="sxs-lookup"><span data-stu-id="e81b7-150">In this section, you add a *Comments* column to the *User* table in the membership database and create a page that lets you display and edit comments for each user.</span></span> <span data-ttu-id="e81b7-151">Vous déployez les modifications sur le test, intermédiaire et de production.</span><span class="sxs-lookup"><span data-stu-id="e81b7-151">Then you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-membership-database"></a><span data-ttu-id="e81b7-152">Ajouter une colonne à une table dans la base de données d’appartenance</span><span class="sxs-lookup"><span data-stu-id="e81b7-152">Add a column to a table in the membership database</span></span>

1. <span data-ttu-id="e81b7-153">Dans Visual Studio, ouvrez **l’Explorateur d’objets SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="e81b7-153">In Visual Studio, open **SQL Server Object Explorer**.</span></span>
2. <span data-ttu-id="e81b7-154">Développez **(localdb) \v11.0**, développez **bases de données**, développez **aspnet-ContosoUniversity** (pas **aspnet-ContosoUniversity-Prod**) puis développez **Tables**.</span><span class="sxs-lookup"><span data-stu-id="e81b7-154">Expand **(localdb)\v11.0**, expand **Databases**, expand **aspnet-ContosoUniversity** (not **aspnet-ContosoUniversity-Prod**) and then expand **Tables**.</span></span>

    <span data-ttu-id="e81b7-155">Si vous ne voyez pas **(localdb) \v11.0** sous le **SQL Server** nœud, avec le bouton droit le **SQL Server** nœud et cliquez sur **ajouter SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="e81b7-155">If you don't see **(localdb)\v11.0** under the **SQL Server** node, right-click the **SQL Server** node and click **Add SQL Server**.</span></span> <span data-ttu-id="e81b7-156">Dans le **se connecter au serveur** boîte de dialogue Entrez *(localdb) \v11.0* comme le **nom du serveur**, puis cliquez sur **connexion**.</span><span class="sxs-lookup"><span data-stu-id="e81b7-156">In the **Connect to Server** dialog box enter *(localdb)\v11.0* as the **Server name**, and then click **Connect**.</span></span>

    <span data-ttu-id="e81b7-157">Si vous ne voyez pas **aspnet-ContosoUniversity**, exécutez le projet et connectez-vous à l’aide de la *admin* informations d’identification (mot de passe est *devpwd*), puis actualisez le  **L’Explorateur d’objets SQL Server** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="e81b7-157">If you don't see **aspnet-ContosoUniversity**, run the project and log in using the *admin* credentials (password is *devpwd*), and then refresh the **SQL Server Object Explorer** window.</span></span>
3. <span data-ttu-id="e81b7-158">Avec le bouton droit le **utilisateurs** de table, puis cliquez sur **Concepteur de vue**.</span><span class="sxs-lookup"><span data-stu-id="e81b7-158">Right-click the **Users** table, and then click **View Designer**.</span></span>

    ![Concepteur de vue SSOX](deploying-a-database-update/_static/image3.png)
4. <span data-ttu-id="e81b7-160">Dans le concepteur, ajoutez un *commentaires* colonne et le rendre *nvarchar (128)* et accepte les valeurs NULL, puis cliquez sur **mise à jour**.</span><span class="sxs-lookup"><span data-stu-id="e81b7-160">In the designer, add a *Comments* column and make it *nvarchar(128)* and nullable, and then click **Update**.</span></span>

    ![Ajout de la colonne commentaires](deploying-a-database-update/_static/image4.png)
5. <span data-ttu-id="e81b7-162">Dans le **mises à jour de la base de données aperçu** , cliquez sur **mise à jour de la base de données**.</span><span class="sxs-lookup"><span data-stu-id="e81b7-162">In the **Preview Database Updates** box, click **Update Database**.</span></span>

    ![Aperçu des mises à jour de base de données](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a><span data-ttu-id="e81b7-164">Créer une page pour afficher et modifier la nouvelle colonne</span><span class="sxs-lookup"><span data-stu-id="e81b7-164">Create a page to display and edit the new column</span></span>

1. <span data-ttu-id="e81b7-165">Dans **l’Explorateur de solutions**, avec le bouton droit le **compte** cliquez sur le dossier dans le projet ContosoUniversity, **ajouter**, puis cliquez sur **un nouvel élément** .</span><span class="sxs-lookup"><span data-stu-id="e81b7-165">In **Solution Explorer**, right-click the **Account** folder in the ContosoUniversity project, click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="e81b7-166">Créer un nouveau **Page maître à l’aide de formulaire Web** et nommez-le *UserInfo.aspx*.</span><span class="sxs-lookup"><span data-stu-id="e81b7-166">Create a new **Web Form Using Master Page** and name it *UserInfo.aspx*.</span></span> <span data-ttu-id="e81b7-167">Acceptez la valeur par défaut *Site.Master* fichier en tant que la page maître.</span><span class="sxs-lookup"><span data-stu-id="e81b7-167">Accept the default *Site.Master* file as the master page.</span></span>
3. <span data-ttu-id="e81b7-168">Copiez le balisage suivant dans le `MainContent` `Content` élément (la dernière de 3 `Content` éléments) :</span><span class="sxs-lookup"><span data-stu-id="e81b7-168">Copy the following markup into the `MainContent` `Content` element (the last of the 3 `Content` elements):</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. <span data-ttu-id="e81b7-169">Cliquez sur le *UserInfo.aspx* page, puis cliquez sur **afficher dans le navigateur**.</span><span class="sxs-lookup"><span data-stu-id="e81b7-169">Right-click the *UserInfo.aspx* page and click **View in Browser**.</span></span>
5. <span data-ttu-id="e81b7-170">Ouvrez une session avec votre *admin* informations d’identification de l’utilisateur (mot de passe est *devpwd*) et ajouter des commentaires à un utilisateur pour vérifier que la page fonctionne correctement.</span><span class="sxs-lookup"><span data-stu-id="e81b7-170">Log in with your *admin* user credentials (password is *devpwd*) and add some comments to a user to verify that the page works correctly.</span></span>

    ![Page UserInfo](deploying-a-database-update/_static/image6.png)
6. <span data-ttu-id="e81b7-172">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="e81b7-172">Close the browser.</span></span>

## <a name="deploy-the-database-update"></a><span data-ttu-id="e81b7-173">Déployer la mise à jour de la base de données</span><span class="sxs-lookup"><span data-stu-id="e81b7-173">Deploy the database update</span></span>

<span data-ttu-id="e81b7-174">Pour déployer à l’aide du fournisseur dbDacFx, il vous suffit de sélectionner le **base de données de mise à jour** option dans le profil de publication.</span><span class="sxs-lookup"><span data-stu-id="e81b7-174">To deploy by using the dbDacFx provider, you just need to select the **Update database** option in the publish profile.</span></span> <span data-ttu-id="e81b7-175">Toutefois, pour le déploiement initial lorsque vous avez utilisé cette option vous avez également configuré certains scripts SQL supplémentaires pour exécuter : ceux sont toujours dans le profil et vous devrez les empêcher de s’exécuter à nouveau.</span><span class="sxs-lookup"><span data-stu-id="e81b7-175">However, for the initial deployment when you used this option you also configured some additional SQL scripts to run: those are still in the profile and you'll have to prevent them from running again.</span></span>

1. <span data-ttu-id="e81b7-176">Ouvrez le **publier le site Web** Assistant en cliquant sur le projet ContosoUniversity en cliquant sur **publier**.</span><span class="sxs-lookup"><span data-stu-id="e81b7-176">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="e81b7-177">Sélectionnez le **Test** profil.</span><span class="sxs-lookup"><span data-stu-id="e81b7-177">Select the **Test** profile.</span></span>
3. <span data-ttu-id="e81b7-178">Cliquez sur le **paramètres** onglet.</span><span class="sxs-lookup"><span data-stu-id="e81b7-178">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="e81b7-179">Sous **DefaultConnection**, sélectionnez **base de données de mise à jour**.</span><span class="sxs-lookup"><span data-stu-id="e81b7-179">Under **DefaultConnection**, select **Update database**.</span></span>
5. <span data-ttu-id="e81b7-180">Désactiver les scripts supplémentaires que vous avez configuré pour s’exécuter pour le déploiement initial :</span><span class="sxs-lookup"><span data-stu-id="e81b7-180">Disable the additional scripts that you configured to run for the initial deployment:</span></span>

    1. <span data-ttu-id="e81b7-181">Cliquez sur **configurer des mises à jour de la base de données**.</span><span class="sxs-lookup"><span data-stu-id="e81b7-181">Click **Configure database updates**.</span></span>
    2. <span data-ttu-id="e81b7-182">Dans le **configurer les mises à jour de base de données** boîte de dialogue, désactivez les cases à cocher à côté *Grant.sql* et *aspnet-données-dev.sql*.</span><span class="sxs-lookup"><span data-stu-id="e81b7-182">In the **Configure Database Updates** dialog box, clear the check boxes next to *Grant.sql* and *aspnet-data-dev.sql*.</span></span>
    3. <span data-ttu-id="e81b7-183">Cliquez sur **Fermer**.</span><span class="sxs-lookup"><span data-stu-id="e81b7-183">Click **Close**.</span></span>
6. <span data-ttu-id="e81b7-184">Cliquez sur le **aperçu** onglet.</span><span class="sxs-lookup"><span data-stu-id="e81b7-184">Click the **Preview** tab.</span></span>
7. <span data-ttu-id="e81b7-185">Sous **bases de données** et à droite de **DefaultConnection**, cliquez sur le **base de données préliminaire** lien.</span><span class="sxs-lookup"><span data-stu-id="e81b7-185">Under **Databases** and to the right of **DefaultConnection**, click the **Preview database** link.</span></span>

    ![Aperçu de la base de données](deploying-a-database-update/_static/image7.png)

    <span data-ttu-id="e81b7-187">La fenêtre d’aperçu affiche le script qui sera exécuté dans la base de données de destination pour rendre ce schéma de base de données correspond au schéma de la base de données source.</span><span class="sxs-lookup"><span data-stu-id="e81b7-187">The preview window shows the script that will be run in the destination database to make that database schema match the schema of the source database.</span></span> <span data-ttu-id="e81b7-188">Le script inclut une commande ALTER TABLE qui ajoute la nouvelle colonne.</span><span class="sxs-lookup"><span data-stu-id="e81b7-188">The script includes an ALTER TABLE command that adds the new column.</span></span>
8. <span data-ttu-id="e81b7-189">Fermer le **aperçu de la base de données** boîte de dialogue, puis cliquez sur **publier**.</span><span class="sxs-lookup"><span data-stu-id="e81b7-189">Close the **Database Preview** dialog box, and then click **Publish**.</span></span>

    <span data-ttu-id="e81b7-190">Visual Studio déploie l’application mis à jour, et le navigateur s’ouvre à la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="e81b7-190">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
9. <span data-ttu-id="e81b7-191">Exécutez la page UserInfo (ajouter *Account/UserInfo.aspx* à l’URL de la page d’accueil) pour vérifier que la mise à jour a été déployé.</span><span class="sxs-lookup"><span data-stu-id="e81b7-191">Run the UserInfo page (add *Account/UserInfo.aspx* to the home page URL) to verify that the update was successfully deployed.</span></span> <span data-ttu-id="e81b7-192">Vous devrez vous connecter en entrant *admin* et *devpwd*.</span><span class="sxs-lookup"><span data-stu-id="e81b7-192">You'll have to log in by entering *admin* and *devpwd*.</span></span>

    <span data-ttu-id="e81b7-193">Les données dans les tables ne sont pas déployées par défaut, et vous n’a pas configuré un script de déploiement de données à exécuter, donc vous ne trouverez pas les commentaires que vous avez ajouté dans le développement.</span><span class="sxs-lookup"><span data-stu-id="e81b7-193">Data in tables is not deployed by default, and you didn't configure a data deployment script to run, so you won't find the comment that you added in development.</span></span> <span data-ttu-id="e81b7-194">Vous pouvez ajouter un nouveau commentaire maintenant la zone de transit pour vérifier que la modification a été déployée sur la base de données et que la page fonctionne correctement.</span><span class="sxs-lookup"><span data-stu-id="e81b7-194">You can add a new comment now in staging to verify that the change was deployed to the database and the page works correctly.</span></span>
10. <span data-ttu-id="e81b7-195">Suivez la même procédure pour déployer sur intermédiaire et de production.</span><span class="sxs-lookup"><span data-stu-id="e81b7-195">Follow the same procedure to deploy to staging and production.</span></span>

    <span data-ttu-id="e81b7-196">N’oubliez pas de désactiver les scripts supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="e81b7-196">Don't forget to disable the extra scripts.</span></span> <span data-ttu-id="e81b7-197">La seule différence par rapport au profil de Test est que vous allez désactiver un seul script dans la mise en transit et des profils de Production, car ils ont été configurés pour s’exécuter uniquement *aspnet-op-data.sql*.</span><span class="sxs-lookup"><span data-stu-id="e81b7-197">The only difference compared to the Test profile is that you will disable only one script in the Staging and Production profiles because they were configured to run only *aspnet-prod-data.sql*.</span></span>

    <span data-ttu-id="e81b7-198">Les informations d’identification intermédiaire et de production sont admin et prodpwd.</span><span class="sxs-lookup"><span data-stu-id="e81b7-198">The credentials for staging and production are admin and prodpwd.</span></span>

    <span data-ttu-id="e81b7-199">Pour une mise à jour d’application réelles de production qui inclut une modification de la base de données en général également suivre l’application en mode hors connexion durant le déploiement en téléchargeant *application\_offline.htm* avant la publication et en la supprimant Ensuite, comme vous l’avez vu dans [du didacticiel précédent](deploying-a-code-update.md).</span><span class="sxs-lookup"><span data-stu-id="e81b7-199">For a real production application update that includes a database change you would also typically take the application offline during deployment by uploading *app\_offline.htm* before publishing and deleting it afterward, as you saw in [the previous tutorial](deploying-a-code-update.md).</span></span>

## <a name="summary"></a><span data-ttu-id="e81b7-200">Résumé</span><span class="sxs-lookup"><span data-stu-id="e81b7-200">Summary</span></span>

<span data-ttu-id="e81b7-201">Vous avez maintenant déployé une mise à jour d’application qui comportait une modification de la base de données à l’aide de Migrations Code First et le fournisseur dbDacFx.</span><span class="sxs-lookup"><span data-stu-id="e81b7-201">You've now deployed an application update that included a database change using both Code First Migrations and the dbDacFx provider.</span></span>

![Page instructeurs avec la date de naissance](deploying-a-database-update/_static/image8.png)

![Page UserInfo](deploying-a-database-update/_static/image9.png)

<span data-ttu-id="e81b7-204">Le didacticiel suivant vous montre comment exécuter les déploiements à l’aide de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="e81b7-204">The next tutorial shows you how to execute deployments by using the command line.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="e81b7-205">[Précédent](deploying-a-code-update.md)
[Suivant](command-line-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="e81b7-205">[Previous](deploying-a-code-update.md)
[Next](command-line-deployment.md)</span></span>
