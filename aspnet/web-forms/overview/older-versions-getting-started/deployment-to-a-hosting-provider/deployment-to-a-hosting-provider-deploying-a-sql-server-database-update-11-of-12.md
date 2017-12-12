---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: "Déploiement d’Application Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : déploiement d’une mise à jour de base de données de serveur SQL - 11 12 | Documents Microsoft"
author: tdykstra
description: "Cette série de didacticiels vous montre comment déployer (publier) ASP.NET projet d’application web qui inclut une base de données SQL Server Compact à l’aide de Visual Stu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: 898259885da8a089db296bd0f400ee8863877d08
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a><span data-ttu-id="15fa0-103">Déploiement d’Application Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : déploiement d’une mise à jour de base de données de serveur SQL - 11 12</span><span class="sxs-lookup"><span data-stu-id="15fa0-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Deploying a SQL Server Database Update - 11 of 12</span></span>
====================
<span data-ttu-id="15fa0-104">Par [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="15fa0-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="15fa0-105">Télécharger le projet de démarrage</span><span class="sxs-lookup"><span data-stu-id="15fa0-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="15fa0-106">Cette série de didacticiels vous montre comment déployer (publier) ASP.NET projet d’application web qui inclut une base de données SQL Server Compact à l’aide de Visual Studio 2012 RC ou Visual Studio Express 2012 RC pour le Web.</span><span class="sxs-lookup"><span data-stu-id="15fa0-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="15fa0-107">Vous pouvez également utiliser Visual Studio 2010 si vous installez la mise à jour de publication Web.</span><span class="sxs-lookup"><span data-stu-id="15fa0-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="15fa0-108">Pour une présentation de la série, consultez [le premier didacticiel de la série](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="15fa0-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="15fa0-109">Pour obtenir un didacticiel qui montre les fonctionnalités de déploiement introduites après la version RC de Visual Studio 2012, montre comment déployer des éditions de SQL Server autre que SQL Server Compact et montre comment déployer pour les Sites Web Windows Azure, consultez [déploiement de Web ASP.NET à l’aide de Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span><span class="sxs-lookup"><span data-stu-id="15fa0-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Windows Azure Web Sites, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="15fa0-110">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="15fa0-110">Overview</span></span>

<span data-ttu-id="15fa0-111">Ce didacticiel vous montre comment déployer une mise à jour de la base de données à une base de données SQL Server complète.</span><span class="sxs-lookup"><span data-stu-id="15fa0-111">This tutorial shows you how to deploy a database update to a full SQL Server database.</span></span> <span data-ttu-id="15fa0-112">Étant donné que les Migrations Code First fait tout le travail de mise à jour de la base de données, le processus est presque identique à ce que vous avez fait pour SQL Server Compact dans le [déploiement d’une mise à jour de la base de données](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) didacticiel.</span><span class="sxs-lookup"><span data-stu-id="15fa0-112">Because Code First Migrations does all the work of updating the database, the process is almost identical to what you did for SQL Server Compact in the [Deploying a Database Update](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) tutorial.</span></span>

<span data-ttu-id="15fa0-113">Rappel : Si vous obtenez un message d’erreur, ou quelque chose ne fonctionne pas lorsque vous parcourez le didacticiel, veillez à consulter le [page Résolution des problèmes](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="15fa0-113">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span></span>

## <a name="adding-a-new-column-to-a-table"></a><span data-ttu-id="15fa0-114">Ajout d’une nouvelle colonne à une Table</span><span class="sxs-lookup"><span data-stu-id="15fa0-114">Adding a New Column to a Table</span></span>

<span data-ttu-id="15fa0-115">Dans cette section de ce didacticiel, que vous allez modifier une base de données et correspondant aux modifications de code, puis testez-les dans Visual Studio en vue de les déployer dans les environnements de test et de production.</span><span class="sxs-lookup"><span data-stu-id="15fa0-115">In this section of the tutorial you'll make a database change and corresponding code changes, then test them in Visual Studio in preparation for deploying them to the test and production environments.</span></span> <span data-ttu-id="15fa0-116">La modification implique l’ajout d’un `OfficeHours` colonne à la `Instructor` entité et en affichant les nouvelles informations dans le **instructeurs** page web.</span><span class="sxs-lookup"><span data-stu-id="15fa0-116">The change involves adding an `OfficeHours` column to the `Instructor` entity and displaying the new information in the **Instructors** web page.</span></span>

<span data-ttu-id="15fa0-117">Dans le projet ContosoUniversity.DAL, ouvrez *Instructor.cs* et ajoutez la propriété suivante entre les `HireDate` et `Courses` propriétés :</span><span class="sxs-lookup"><span data-stu-id="15fa0-117">In the ContosoUniversity.DAL project, open *Instructor.cs* and add the following property between the `HireDate` and `Courses` properties:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

<span data-ttu-id="15fa0-118">Mettre à jour de la classe de l’initialiseur afin qu’elle est basée sur la nouvelle colonne de données de test.</span><span class="sxs-lookup"><span data-stu-id="15fa0-118">Update the initializer class so that it seeds the new column with test data.</span></span> <span data-ttu-id="15fa0-119">Ouvrez *Migrations\Configuration.cs* et remplacez le bloc de code qui commence `var instructors = new List<Instructor>` avec le bloc de code suivant, qui inclut la nouvelle colonne :</span><span class="sxs-lookup"><span data-stu-id="15fa0-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes the new column:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

<span data-ttu-id="15fa0-120">Dans le projet ContosoUniversity, ouvrez *Instructors.aspx* et ajouter un nouveau champ de modèle pour les heures de bureau juste avant la fermeture `</Columns>` étiquette dans la première `GridView` contrôle :</span><span class="sxs-lookup"><span data-stu-id="15fa0-120">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field for office hours just before the closing `</Columns>` tag in the first `GridView` control:</span></span>

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

<span data-ttu-id="15fa0-121">Générez la solution.</span><span class="sxs-lookup"><span data-stu-id="15fa0-121">Build the solution.</span></span>

<span data-ttu-id="15fa0-122">Ouvrez le **Package Manager Console** fenêtre et sélectionnez ContosoUniversity.DAL comme le **projet par défaut**.</span><span class="sxs-lookup"><span data-stu-id="15fa0-122">Open the **Package Manager Console** window, and select ContosoUniversity.DAL as the **Default project**.</span></span>

<span data-ttu-id="15fa0-123">Entrez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="15fa0-123">Enter the following commands:</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

<span data-ttu-id="15fa0-124">Exécutez l’application et sélectionnez le **instructeurs** page.</span><span class="sxs-lookup"><span data-stu-id="15fa0-124">Run the application and select the **Instructors** page.</span></span> <span data-ttu-id="15fa0-125">La page prend un peu Pluse de temps à charger, car l’Entity Framework recrée la base de données et l’alimente avec les données de test.</span><span class="sxs-lookup"><span data-stu-id="15fa0-125">The page takes a little longer than usual to load, because the Entity Framework re-creates the database and seeds it with test data.</span></span>

<span data-ttu-id="15fa0-126">[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="15fa0-126">[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)</span></span>

## <a name="deploying-the-database-update-to-the-test-environment"></a><span data-ttu-id="15fa0-127">Déploiement de la mise à jour de la base de données à l’environnement de Test</span><span class="sxs-lookup"><span data-stu-id="15fa0-127">Deploying the Database Update to the Test Environment</span></span>

<span data-ttu-id="15fa0-128">Lorsque vous utilisez Migrations Code First, la méthode pour le déploiement d’une modification de la base de données vers SQL Server est la même que pour SQL Server Compact.</span><span class="sxs-lookup"><span data-stu-id="15fa0-128">When you use Code First Migrations, the method for deploying a database change to SQL Server is the same as for SQL Server Compact.</span></span> <span data-ttu-id="15fa0-129">Toutefois, vous devez modifier le Test de profil de publication, car elle est toujours configurée pour migrer à partir de SQL Server Compact à SQL Server.</span><span class="sxs-lookup"><span data-stu-id="15fa0-129">However, you have to change the Test publish profile because it is still set up to migrate from SQL Server Compact to SQL Server.</span></span>

<span data-ttu-id="15fa0-130">La première étape consiste à supprimer les transformations de chaîne de connexion que vous avez créé dans le didacticiel précédent.</span><span class="sxs-lookup"><span data-stu-id="15fa0-130">The first step is to remove the connection string transformations that you created in the previous tutorial.</span></span> <span data-ttu-id="15fa0-131">Ils ne sont plus nécessaires, car vous allez spécifier des transformations de chaîne de connexion dans le profil de publication, comme vous le faisiez avant que vous avez configuré le **Package/Publication SQL** onglet pour la migration vers SQL Server.</span><span class="sxs-lookup"><span data-stu-id="15fa0-131">These are no longer needed because you'll specify connection string transformations in the publish profile, as you did before you configured the **Package/Publish SQL** tab for migration to SQL Server.</span></span>

<span data-ttu-id="15fa0-132">Ouvrez le *Web.Test.config* de fichiers et de supprimer le `connectionStrings` élément.</span><span class="sxs-lookup"><span data-stu-id="15fa0-132">Open the *Web.Test.config* file and remove the `connectionStrings` element.</span></span> <span data-ttu-id="15fa0-133">La seule transformation restante dans le *Web.Test.config* fichier concerne le `Environment` valeur dans le `appSettings` élément.</span><span class="sxs-lookup"><span data-stu-id="15fa0-133">The only remaining transformation in the *Web.Test.config* file is for the `Environment` value in the `appSettings` element.</span></span>

<span data-ttu-id="15fa0-134">Maintenant, vous pouvez modifier le profil de publication et la publier à l’environnement de test.</span><span class="sxs-lookup"><span data-stu-id="15fa0-134">Now you can update the publish profile and publish to the test environment.</span></span>

<span data-ttu-id="15fa0-135">Ouvrez le **publier le site Web** Assistant, puis basculer sur le **profil** onglet.</span><span class="sxs-lookup"><span data-stu-id="15fa0-135">Open the **Publish Web** wizard, and then switch to the **Profile** tab.</span></span>

<span data-ttu-id="15fa0-136">Sélectionnez le **Test** le profil de publication.</span><span class="sxs-lookup"><span data-stu-id="15fa0-136">Select the **Test** publish profile.</span></span>

<span data-ttu-id="15fa0-137">Sélectionnez le **paramètres** onglet.</span><span class="sxs-lookup"><span data-stu-id="15fa0-137">Select the **Settings** tab.</span></span>

<span data-ttu-id="15fa0-138">Cliquez sur **activer la base de données nouvelles améliorations de la publication**.</span><span class="sxs-lookup"><span data-stu-id="15fa0-138">Click **enable the new database publishing improvements**.</span></span>

<span data-ttu-id="15fa0-139">Dans la zone chaîne de connexion pour **SchoolContext**, entrez la même valeur que vous avez utilisé dans le *Web.Test.config* fichier de transformation dans le didacticiel précédent :</span><span class="sxs-lookup"><span data-stu-id="15fa0-139">In the connection string box for **SchoolContext**, enter the same value that you used in the *Web.Test.config* transformation file in the previous tutorial:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

<span data-ttu-id="15fa0-140">Sélectionnez **exécuter fonctionnalité Migrations Code First (s’exécute sur le démarrage de l’application)**.</span><span class="sxs-lookup"><span data-stu-id="15fa0-140">Select **Execute Code First Migrations (runs on application start)**.</span></span> <span data-ttu-id="15fa0-141">(Dans votre version de Visual Studio, la case à cocher doit être intitulé **s’appliquent des Migrations Code First**.)</span><span class="sxs-lookup"><span data-stu-id="15fa0-141">(In your version of Visual Studio, the check box might be labeled **Apply Code First Migrations**.)</span></span>

<span data-ttu-id="15fa0-142">Dans la zone chaîne de connexion pour **DefaultConnection**, entrez la même valeur que vous avez utilisé dans le *Web.Test.config* fichier de transformation dans le didacticiel précédent :</span><span class="sxs-lookup"><span data-stu-id="15fa0-142">In the connection string box for **DefaultConnection**, enter the same value that you used in the *Web.Test.config* transformation file in the previous tutorial:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

<span data-ttu-id="15fa0-143">Laissez **base de données de mise à jour** désactivée.</span><span class="sxs-lookup"><span data-stu-id="15fa0-143">Leave **Update database** cleared.</span></span>

<span data-ttu-id="15fa0-144">Cliquez sur **Publier**.</span><span class="sxs-lookup"><span data-stu-id="15fa0-144">Click **Publish**.</span></span>

<span data-ttu-id="15fa0-145">Visual Studio déploie les modifications de code à l’environnement de test et ouvre le navigateur à la page d’accueil Contoso University.</span><span class="sxs-lookup"><span data-stu-id="15fa0-145">Visual Studio deploys the code changes to the test environment and opens the browser to the Contoso University home page.</span></span>

<span data-ttu-id="15fa0-146">Sélectionnez la page de formateurs.</span><span class="sxs-lookup"><span data-stu-id="15fa0-146">Select the Instructors page.</span></span>

<span data-ttu-id="15fa0-147">Lorsque l’application s’exécute à cette page, il tente d’accéder à la base de données.</span><span class="sxs-lookup"><span data-stu-id="15fa0-147">When the application runs this page, it tries to access the database.</span></span> <span data-ttu-id="15fa0-148">Migrations Code First vérifie si la base de données est en cours et qu’il détecte que la dernière migration n’a pas encore été appliquée.</span><span class="sxs-lookup"><span data-stu-id="15fa0-148">Code First Migrations checks if the database is current, and finds that the latest migration has not been applied yet.</span></span> <span data-ttu-id="15fa0-149">Applique la dernière migration de Migrations Code First, s’exécute le `Seed` (méthode), puis sur la page s’exécute normalement.</span><span class="sxs-lookup"><span data-stu-id="15fa0-149">Code First Migrations applies the latest migration, runs the `Seed` method, and then the page runs normally.</span></span> <span data-ttu-id="15fa0-150">Vous voyez la nouvelle colonne des heures de bureau avec les données de valeurs de départ.</span><span class="sxs-lookup"><span data-stu-id="15fa0-150">You see the new Office Hours column with the seeded data.</span></span>

<span data-ttu-id="15fa0-151">[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="15fa0-151">[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)</span></span>

## <a name="deploying-the-database-update-to-the-production-environment"></a><span data-ttu-id="15fa0-152">Déploiement de la mise à jour de la base de données dans l’environnement de Production</span><span class="sxs-lookup"><span data-stu-id="15fa0-152">Deploying the Database Update to the Production Environment</span></span>

<span data-ttu-id="15fa0-153">Vous devez modifier le profil de publication pour l’environnement de production également.</span><span class="sxs-lookup"><span data-stu-id="15fa0-153">You have to change the publish profile for the production environment also.</span></span> <span data-ttu-id="15fa0-154">Dans ce cas, vous allez supprimer le profil existant et créez-en un en important un fichier .publishsettings mis à jour.</span><span class="sxs-lookup"><span data-stu-id="15fa0-154">In this case you'll remove the existing profile and create a new one by importing an updated .publishsettings file.</span></span> <span data-ttu-id="15fa0-155">Le fichier mis à jour inclut la chaîne de connexion pour la base de données SQL Server Cytanium.</span><span class="sxs-lookup"><span data-stu-id="15fa0-155">The updated file will include the connection string for the SQL Server database at Cytanium.</span></span>

<span data-ttu-id="15fa0-156">Comme vous l’avez vu lorsque vous avez déployé à l’environnement de test, vous n’avez plus besoin transformations de chaîne de connexion dans le *Web.Production.config* fichier de transformation.</span><span class="sxs-lookup"><span data-stu-id="15fa0-156">As you saw when you deployed to the test environment, you no longer need connection string transforms in the *Web.Production.config* transformation file.</span></span> <span data-ttu-id="15fa0-157">Ouvrir ce fichier, supprimez le `connectionStrings` élément.</span><span class="sxs-lookup"><span data-stu-id="15fa0-157">Open that file and remove the `connectionStrings` element.</span></span> <span data-ttu-id="15fa0-158">Les transformations restantes sont pour le `Environment` valeur dans le `appSettings` élément et le `location` élément qui restreint l’accès aux rapports d’erreurs Elmah.</span><span class="sxs-lookup"><span data-stu-id="15fa0-158">The remaining transformations are for the `Environment` value in the `appSettings` element and the `location` element that restricts access to Elmah error reports.</span></span>

<span data-ttu-id="15fa0-159">Avant de créer un nouveau profil de publication pour la production, télécharger un fichier .publishsettings mis à jour la même façon que vous l’avez fait précédemment dans le [déploiement dans l’environnement de Production](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) didacticiel.</span><span class="sxs-lookup"><span data-stu-id="15fa0-159">Before you create a new publish profile for production, download an updated .publishsettings file the same way you did earlier in the [Deploying to the Production Environment](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial.</span></span> <span data-ttu-id="15fa0-160">(Dans le panneau de configuration de Cytanium, cliquez sur **Sites Web**, puis cliquez sur le **contosouniversity.com** site Web.</span><span class="sxs-lookup"><span data-stu-id="15fa0-160">(In the Cytanium control panel, click **Web Sites**, and then click the **contosouniversity.com** website.</span></span> <span data-ttu-id="15fa0-161">Sélectionnez le **Web Publishing** onglet, puis cliquez sur **télécharger le profil de publication pour ce site web**.) La raison pour laquelle que vous effectuez cette opération consiste à sélectionner la chaîne de connexion de base de données dans le fichier .publishsettings.</span><span class="sxs-lookup"><span data-stu-id="15fa0-161">Select the **Web Publishing** tab, and then click **Download Publishing Profile for this web site**.) The reason you are doing this is to pick up the database connection string in the .publishsettings file.</span></span> <span data-ttu-id="15fa0-162">La chaîne de connexion n’était pas disponible la première fois que vous avez téléchargé le fichier, car vous ont été toujours à l’aide de SQL Server Compact et n’avait pas la base de données SQL Server à Cytanium encore créé.</span><span class="sxs-lookup"><span data-stu-id="15fa0-162">The connection string wasn't available the first time you downloaded the file, because you were still using SQL Server Compact and hadn't created the SQL Server database at Cytanium yet.</span></span>

<span data-ttu-id="15fa0-163">Maintenant, vous pouvez modifier le profil de publication et la publier à l’environnement de production.</span><span class="sxs-lookup"><span data-stu-id="15fa0-163">Now you can update the publish profile and publish to the production environment.</span></span>

<span data-ttu-id="15fa0-164">Ouvrez le **publier le site Web** Assistant, puis basculer sur le **profil** onglet.</span><span class="sxs-lookup"><span data-stu-id="15fa0-164">Open the **Publish Web** wizard, and then switch to the **Profile** tab.</span></span>

<span data-ttu-id="15fa0-165">Cliquez sur **gérer les profils**, puis supprimez le profil de Production.</span><span class="sxs-lookup"><span data-stu-id="15fa0-165">Click **Manage Profiles**, and then delete the Production profile.</span></span>

<span data-ttu-id="15fa0-166">Fermer le **publier le site Web** Assistant pour enregistrer cette modification.</span><span class="sxs-lookup"><span data-stu-id="15fa0-166">Close the **Publish Web** wizard to save this change.</span></span>

<span data-ttu-id="15fa0-167">Ouvrez le **publier le site Web** Assistant à nouveau, puis cliquez sur **importation**.</span><span class="sxs-lookup"><span data-stu-id="15fa0-167">Open the **Publish Web** wizard again, and then click **Import**.</span></span>

<span data-ttu-id="15fa0-168">Sur le **connexion** , modifiez **URL de Destination** la valeur appropriée si vous utilisez une URL temporaire.</span><span class="sxs-lookup"><span data-stu-id="15fa0-168">On the **Connection** tab, change **Destination URL** to the appropriate value if you are using a temporary URL.</span></span>

<span data-ttu-id="15fa0-169">Cliquez sur **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="15fa0-169">Click **Next**.</span></span>

<span data-ttu-id="15fa0-170">Sur le **paramètres** , cliquez sur **activer la base de données nouvelles améliorations de la publication**.</span><span class="sxs-lookup"><span data-stu-id="15fa0-170">On the **Settings** tab, click **enable the new database publishing improvements**.</span></span>

<span data-ttu-id="15fa0-171">Dans la liste de liste déroulante de chaîne de connexion pour **SchoolContext**, sélectionnez la chaîne de connexion Cytanium.</span><span class="sxs-lookup"><span data-stu-id="15fa0-171">In the connection string drop-down list for **SchoolContext**, select the Cytanium connection string.</span></span>

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

<span data-ttu-id="15fa0-173">Sélectionnez **migrations exécuter le Code First (s’exécute sur le démarrage de l’application)**.</span><span class="sxs-lookup"><span data-stu-id="15fa0-173">Select **Execute Code First migrations (runs on application start)**.</span></span>

<span data-ttu-id="15fa0-174">Dans la liste de liste déroulante de chaîne de connexion pour **DefaultConnection**, sélectionnez la chaîne de connexion Cytanium.</span><span class="sxs-lookup"><span data-stu-id="15fa0-174">In the connection string drop-down list for **DefaultConnection**, select the Cytanium connection string.</span></span>

<span data-ttu-id="15fa0-175">Sélectionnez le **profil** , cliquez sur **gérer les profils**et renommez le profil à partir de « contosouniversity.com - Web Deploy » à « Production ».</span><span class="sxs-lookup"><span data-stu-id="15fa0-175">Select the **Profile** tab, click **Manage Profiles**, and rename the profile from "contosouniversity.com - Web Deploy" to "Production".</span></span>

<span data-ttu-id="15fa0-176">Fermer le profil de publication pour enregistrer les modifications, puis ouvrez-le à nouveau.</span><span class="sxs-lookup"><span data-stu-id="15fa0-176">Close the publish profile to save the change, then open it again.</span></span>

<span data-ttu-id="15fa0-177">Cliquez sur **Publier**.</span><span class="sxs-lookup"><span data-stu-id="15fa0-177">Click **Publish**.</span></span> <span data-ttu-id="15fa0-178">(Pour un site Web de production réel, vous copiez *application\_offline.htm* de production et put dans votre dossier de projet avant la publication, puis le supprimer lorsque le déploiement est terminé.)</span><span class="sxs-lookup"><span data-stu-id="15fa0-178">(For a real production website, you would copy *app\_offline.htm* to production and put it in your project folder before publishing, then remove it when deployment is complete.)</span></span>

<span data-ttu-id="15fa0-179">Visual Studio déploie les modifications de code à l’environnement de test et ouvre le navigateur à la page d’accueil Contoso University.</span><span class="sxs-lookup"><span data-stu-id="15fa0-179">Visual Studio deploys the code changes to the test environment and opens the browser to the Contoso University home page.</span></span>

<span data-ttu-id="15fa0-180">Sélectionnez la page de formateurs.</span><span class="sxs-lookup"><span data-stu-id="15fa0-180">Select the Instructors page.</span></span>

<span data-ttu-id="15fa0-181">Migrations Code First met à jour la base de données de la même manière que dans l’environnement de Test.</span><span class="sxs-lookup"><span data-stu-id="15fa0-181">Code First Migrations updates the database the same way it did in the Test environment.</span></span> <span data-ttu-id="15fa0-182">Vous voyez la nouvelle colonne des heures de bureau avec les données de valeurs de départ.</span><span class="sxs-lookup"><span data-stu-id="15fa0-182">You see the new Office Hours column with the seeded data.</span></span>

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

<span data-ttu-id="15fa0-184">Vous avez correctement déployé une mise à jour d’application qui comportait une modification de la base de données, à l’aide d’une base de données SQL Server.</span><span class="sxs-lookup"><span data-stu-id="15fa0-184">You have now successfully deployed an application update that included a database change, using a SQL Server database.</span></span>

## <a name="more-information"></a><span data-ttu-id="15fa0-185">Informations complémentaires</span><span class="sxs-lookup"><span data-stu-id="15fa0-185">More Information</span></span>

<span data-ttu-id="15fa0-186">Cette étape termine cette série de didacticiels sur le déploiement d’une application de web ASP.NET sur un fournisseur d’hébergement tiers.</span><span class="sxs-lookup"><span data-stu-id="15fa0-186">This completes this series of tutorials on deploying an ASP.NET web application to a third-party hosting provider.</span></span> <span data-ttu-id="15fa0-187">Pour plus d’informations sur les sujets abordés dans ces didacticiels, consultez le [ASP.NET Deployment Content Map](https://msdn.microsoft.com/en-us/library/bb386521(v=vs.110).aspx) sur le site web MSDN.</span><span class="sxs-lookup"><span data-stu-id="15fa0-187">For more information about any of the topics covered in these tutorials, see the [ASP.NET Deployment Content Map](https://msdn.microsoft.com/en-us/library/bb386521(v=vs.110).aspx) on the MSDN web site.</span></span>

## <a name="acknowledgements"></a><span data-ttu-id="15fa0-188">Remerciements</span><span class="sxs-lookup"><span data-stu-id="15fa0-188">Acknowledgements</span></span>

<span data-ttu-id="15fa0-189">J’aimerais remercier les personnes suivantes qui a effectué des contributions significatives pour le contenu de cette série de didacticiels :</span><span class="sxs-lookup"><span data-stu-id="15fa0-189">I would like to thank the following people who made significant contributions to the content of this tutorial series:</span></span>

- [<span data-ttu-id="15fa0-190">Blog d’Alberto Poblacion, MVP &amp; MCT, Espagne</span><span class="sxs-lookup"><span data-stu-id="15fa0-190">Alberto Poblacion, MVP &amp; MCT, Spain</span></span>](https://mvp.support.microsoft.com/profile/Alberto)
- <span data-ttu-id="15fa0-191">Jarod Ferguson, MVP de développement de plate-forme de données, États-Unis</span><span class="sxs-lookup"><span data-stu-id="15fa0-191">Jarod Ferguson, Data Platform Development MVP, United States</span></span>
- <span data-ttu-id="15fa0-192">Sévères Mittal, Microsoft</span><span class="sxs-lookup"><span data-stu-id="15fa0-192">Harsh Mittal, Microsoft</span></span>
- [<span data-ttu-id="15fa0-193">Kristina Olson, Microsoft</span><span class="sxs-lookup"><span data-stu-id="15fa0-193">Kristina Olson, Microsoft</span></span>](https://blogs.iis.net/krolson/default.aspx)
- [<span data-ttu-id="15fa0-194">Protocole Mike, Microsoft</span><span class="sxs-lookup"><span data-stu-id="15fa0-194">Mike Pope, Microsoft</span></span>](http://www.mikepope.com/blog/DisplayBlog.aspx)
- <span data-ttu-id="15fa0-195">Mohit Srivastava, Microsoft</span><span class="sxs-lookup"><span data-stu-id="15fa0-195">Mohit Srivastava, Microsoft</span></span>
- [<span data-ttu-id="15fa0-196">Raffaele Rialdi, Italie</span><span class="sxs-lookup"><span data-stu-id="15fa0-196">Raffaele Rialdi, Italy</span></span>](http://www.iamraf.net/)
- [<span data-ttu-id="15fa0-197">Rick Anderson, Microsoft</span><span class="sxs-lookup"><span data-stu-id="15fa0-197">Rick Anderson, Microsoft</span></span>](https://blogs.msdn.com/b/rickandy/)
- <span data-ttu-id="15fa0-198">[Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter : [ @sayedihashimi ](http://twitter.com/sayedihashimi))</span><span class="sxs-lookup"><span data-stu-id="15fa0-198">[Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [@sayedihashimi](http://twitter.com/sayedihashimi))</span></span>
- <span data-ttu-id="15fa0-199">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter : [ @shanselman ](http://twitter.com/shanselman))</span><span class="sxs-lookup"><span data-stu-id="15fa0-199">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](http://twitter.com/shanselman))</span></span>
- <span data-ttu-id="15fa0-200">[Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter : [ @coolcsh ](http://twitter.com/coolcsh))</span><span class="sxs-lookup"><span data-stu-id="15fa0-200">[Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [@coolcsh](http://twitter.com/coolcsh))</span></span>
- [<span data-ttu-id="15fa0-201">Srđan Božović, Serbie</span><span class="sxs-lookup"><span data-stu-id="15fa0-201">Srđan Božović, Serbia</span></span>](http://msforge.net/blogs/zmajcek/)
- <span data-ttu-id="15fa0-202">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter : [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))</span><span class="sxs-lookup"><span data-stu-id="15fa0-202">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [@vishalrjoshi](http://twitter.com/vishalrjoshi))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="15fa0-203">[Précédent](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
[Suivant](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)</span><span class="sxs-lookup"><span data-stu-id="15fa0-203">[Previous](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
[Next](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)</span></span>
