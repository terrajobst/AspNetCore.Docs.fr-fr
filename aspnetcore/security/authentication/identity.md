---
title: "Introduction à l’identité sur ASP.NET Core"
author: rick-anderson
description: "Utiliser l’identité à une application ASP.NET Core"
keywords: "Autorisation ASP.NET Core, identité, sécurité"
ms.author: riande
manager: wpickett
ms.date: 01/02/2018
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-547ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity
ms.openlocfilehash: 4a5d3622a22b70daa22333cafe58f8831bf0918e
ms.sourcegitcommit: fc98e93464ccf37d9904e89a71cdddbd4bbdb86a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/05/2018
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="32c8f-104">Introduction à l’identité sur ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="32c8f-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="32c8f-105">Par [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), et [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="32c8f-105">By [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="32c8f-106">Identité de ASP.NET Core est un système d’appartenance qui vous permet d’ajouter des fonctionnalités de connexion à votre application.</span><span class="sxs-lookup"><span data-stu-id="32c8f-106">ASP.NET Core Identity is a membership system which allows you to add login functionality to your application.</span></span> <span data-ttu-id="32c8f-107">Les utilisateurs peuvent créer une connexion et un compte avec un nom d’utilisateur et mot de passe, ou ils peuvent utiliser un fournisseur de connexion externe tels que Facebook, Google, Microsoft Account, Twitter ou d’autres.</span><span class="sxs-lookup"><span data-stu-id="32c8f-107">Users can create an account and login with a user name and password or they can use an external login provider such as Facebook, Google, Microsoft Account, Twitter or others.</span></span>

<span data-ttu-id="32c8f-108">Vous pouvez configurer l’identité du principal ASP.NET pour utiliser une base de données SQL Server pour stocker les noms d’utilisateur, les mots de passe et les données de profil.</span><span class="sxs-lookup"><span data-stu-id="32c8f-108">You can configure ASP.NET Core Identity to use a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="32c8f-109">Vous pouvez également utiliser votre propre magasin persistant, par exemple, un stockage de tables Azure.</span><span class="sxs-lookup"><span data-stu-id="32c8f-109">Alternatively, you can use your own persistent store, for example, an Azure Table Storage.</span></span> <span data-ttu-id="32c8f-110">Ce document contient des instructions pour Visual Studio et à l’aide de l’interface CLI.</span><span class="sxs-lookup"><span data-stu-id="32c8f-110">This document contains instructions for Visual Studio and for using the CLI.</span></span>

## <a name="overview-of-identity"></a><span data-ttu-id="32c8f-111">Vue d’ensemble de l’identité</span><span class="sxs-lookup"><span data-stu-id="32c8f-111">Overview of Identity</span></span>

<span data-ttu-id="32c8f-112">Dans cette rubrique, vous allez apprendre à utiliser ASP.NET Core Identity pour ajouter des fonctionnalités pour vous inscrire, connectez-vous et déconnecter un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="32c8f-112">In this topic, you'll learn how to use ASP.NET Core Identity to add functionality to register, log in, and log out a user.</span></span> <span data-ttu-id="32c8f-113">Pour obtenir des instructions plus détaillées sur la création d’applications à l’aide d’ASP.NET Core Identity, consultez la section étapes suivantes à la fin de cet article.</span><span class="sxs-lookup"><span data-stu-id="32c8f-113">For more detailed instructions about creating apps using ASP.NET Core Identity, see the Next Steps section at the end of this article.</span></span>

1.  <span data-ttu-id="32c8f-114">Créez un projet d’Application ASP.NET Core Web avec des comptes d’utilisateur individuels.</span><span class="sxs-lookup"><span data-stu-id="32c8f-114">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="32c8f-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="32c8f-115">Visual Studio</span></span>](#tab/visual-studio)

    <span data-ttu-id="32c8f-116">Dans Visual Studio, sélectionnez **fichier** -> **nouveau** -> **projet**.</span><span class="sxs-lookup"><span data-stu-id="32c8f-116">In Visual Studio, select **File** -> **New** -> **Project**.</span></span> <span data-ttu-id="32c8f-117">Sélectionnez **Application ASP.NET Core Web** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="32c8f-117">Select **ASP.NET Core Web Application** and click **OK**.</span></span>

    ![Boîte de dialogue Nouveau projet](identity/_static/01-new-project.png)

    <span data-ttu-id="32c8f-119">Sélectionnez un ASP.NET Core **l’Application Web (Model-View-Controller)** pour ASP.NET 2.x de base, puis sélectionnez **modifier l’authentification**.</span><span class="sxs-lookup"><span data-stu-id="32c8f-119">Select an ASP.NET Core **Web Application (Model-View-Controller)** for ASP.NET Core 2.x, then select **Change Authentication**.</span></span>

    ![Boîte de dialogue Nouveau projet](identity/_static/02-new-project.png)

    <span data-ttu-id="32c8f-121">Une boîte de dialogue offre différentes possibilités d’authentification.</span><span class="sxs-lookup"><span data-stu-id="32c8f-121">A dialog appears offering authentication choices.</span></span> <span data-ttu-id="32c8f-122">Sélectionnez **comptes d’utilisateur individuels** et cliquez sur **OK** pour revenir à la boîte de dialogue précédente.</span><span class="sxs-lookup"><span data-stu-id="32c8f-122">Select **Individual User Accounts** and click **OK** to return to the previous dialog.</span></span>

    ![Boîte de dialogue Nouveau projet](identity/_static/03-new-project-auth.png)

    <span data-ttu-id="32c8f-124">En sélectionnant **comptes d’utilisateur individuels** dirige Visual Studio pour créer des modèles, ViewModel, vues, contrôleurs et autres composants requis pour l’authentification en tant que partie du modèle de projet.</span><span class="sxs-lookup"><span data-stu-id="32c8f-124">Selecting **Individual User Accounts** directs Visual Studio to create Models, ViewModels, Views, Controllers, and other assets required for authentication as part of the project template.</span></span>

    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="32c8f-125">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="32c8f-125">.NET Core CLI</span></span>](#tab/netcore-cli)

    <span data-ttu-id="32c8f-126">Si vous utilisez l’interface de ligne de base de .NET, créer le projet à l’aide ``dotnet new mvc --auth Individual``.</span><span class="sxs-lookup"><span data-stu-id="32c8f-126">If using the .NET Core CLI, create the new project using ``dotnet new mvc --auth Individual``.</span></span> <span data-ttu-id="32c8f-127">Cette commande crée un nouveau projet avec le même code de modèle d’identité crée de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="32c8f-127">This command creates a new project with the same Identity template code Visual Studio creates.</span></span>

    <span data-ttu-id="32c8f-128">Le projet créé contient le `Microsoft.AspNetCore.Identity.EntityFrameworkCore` package, qui conserve les données d’identité et le schéma à l’aide de SQL Server [Entity Framework Core](https://docs.microsoft.com/ef/).</span><span class="sxs-lookup"><span data-stu-id="32c8f-128">The created project contains the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` package, which persists the Identity data and schema to SQL Server using [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>

    ---

2.  <span data-ttu-id="32c8f-129">Configurer les services d’identité et d’ajouter l’intergiciel (middleware) dans `Startup`.</span><span class="sxs-lookup"><span data-stu-id="32c8f-129">Configure Identity services and add middleware in `Startup`.</span></span>

    <span data-ttu-id="32c8f-130">Les services d’identité sont ajoutés à l’application dans le `ConfigureServices` méthode dans la `Startup` classe :</span><span class="sxs-lookup"><span data-stu-id="32c8f-130">The Identity services are added to the application in the `ConfigureServices` method in the `Startup` class:</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="32c8f-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="32c8f-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    <span data-ttu-id="32c8f-132">Ces services sont accessibles à l’application via [injection de dépendance](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="32c8f-132">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
    
    <span data-ttu-id="32c8f-133">Identité est activée pour l’application en appelant `UseAuthentication` dans le `Configure` (méthode).</span><span class="sxs-lookup"><span data-stu-id="32c8f-133">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="32c8f-134">`UseAuthentication`Ajoute l’authentification [intergiciel (middleware)](xref:fundamentals/middleware) au pipeline de demande.</span><span class="sxs-lookup"><span data-stu-id="32c8f-134">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware) to the request pipeline.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="32c8f-135">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="32c8f-135">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-34)]
    
    <span data-ttu-id="32c8f-136">Ces services sont accessibles à l’application via [injection de dépendance](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="32c8f-136">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
    
    <span data-ttu-id="32c8f-137">Identité est activée pour l’application en appelant `UseIdentity` dans le `Configure` (méthode).</span><span class="sxs-lookup"><span data-stu-id="32c8f-137">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="32c8f-138">`UseIdentity`Ajoute l’authentification par cookie [intergiciel (middleware)](xref:fundamentals/middleware) au pipeline de demande.</span><span class="sxs-lookup"><span data-stu-id="32c8f-138">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware) to the request pipeline.</span></span>
        
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]
    
    ---
     
    <span data-ttu-id="32c8f-139">Pour plus d’informations sur le démarrage de l’application des processus, consultez [démarrage de l’Application](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="32c8f-139">For more information about the application start up process, see [Application Startup](xref:fundamentals/startup).</span></span>

3.  <span data-ttu-id="32c8f-140">Créez un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="32c8f-140">Create a user.</span></span>

    <span data-ttu-id="32c8f-141">Lancer l’application, puis cliquez sur le **inscrire** lien.</span><span class="sxs-lookup"><span data-stu-id="32c8f-141">Launch the application and then click on the **Register** link.</span></span>

    <span data-ttu-id="32c8f-142">S’il s’agit de la première fois que vous effectuez cette action, vous serez peut-être requis pour exécuter des migrations.</span><span class="sxs-lookup"><span data-stu-id="32c8f-142">If this is the first time you're performing this action, you may be required to run migrations.</span></span> <span data-ttu-id="32c8f-143">L’application vous invite à **s’appliquent les Migrations**.</span><span class="sxs-lookup"><span data-stu-id="32c8f-143">The application prompts you to **Apply Migrations**.</span></span> <span data-ttu-id="32c8f-144">Actualisez la page, si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="32c8f-144">Refresh the page if needed.</span></span>
    
    ![Appliquer la Page Web de Migrations](identity/_static/apply-migrations.png)
    
    <span data-ttu-id="32c8f-146">Ou bien, vous pouvez tester à l’aide d’ASP.NET Core Identity avec votre application sans une base de données persistantes à l’aide d’une base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="32c8f-146">Alternately, you can test using ASP.NET Core Identity with your app without a persistent database by using an in-memory database.</span></span> <span data-ttu-id="32c8f-147">Pour utiliser une base de données en mémoire, ajoutez le ``Microsoft.EntityFrameworkCore.InMemory`` le package à votre application et de modifier l’appel de votre application à ``AddDbContext`` dans ``ConfigureServices`` comme suit :</span><span class="sxs-lookup"><span data-stu-id="32c8f-147">To use an in-memory database, add the ``Microsoft.EntityFrameworkCore.InMemory`` package to your app and modify your app's call to ``AddDbContext`` in ``ConfigureServices`` as follows:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
    ```
    
    <span data-ttu-id="32c8f-148">Lorsque l’utilisateur clique sur le **inscrire** lien, le ``Register`` action est appelée sur ``AccountController``.</span><span class="sxs-lookup"><span data-stu-id="32c8f-148">When the user clicks the **Register** link, the ``Register`` action is invoked on ``AccountController``.</span></span> <span data-ttu-id="32c8f-149">Le ``Register`` action crée l’utilisateur en appelant `CreateAsync` sur la `_userManager` objet (fourni à ``AccountController`` par injection de dépendances) :</span><span class="sxs-lookup"><span data-stu-id="32c8f-149">The ``Register`` action creates the user by calling `CreateAsync` on the  `_userManager` object (provided to ``AccountController`` by dependency injection):</span></span>
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

    <span data-ttu-id="32c8f-150">Si l’utilisateur a été créé avec succès, l’utilisateur est connecté par l’appel à ``_signInManager.SignInAsync``.</span><span class="sxs-lookup"><span data-stu-id="32c8f-150">If the user was created successfully, the user is logged in by the call to ``_signInManager.SignInAsync``.</span></span>

    <span data-ttu-id="32c8f-151">**Remarque :** consultez [compte confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) pour connaître les étapes empêcher la connexion immédiate lors de l’inscription.</span><span class="sxs-lookup"><span data-stu-id="32c8f-151">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>
 
4.  <span data-ttu-id="32c8f-152">Se connecter.</span><span class="sxs-lookup"><span data-stu-id="32c8f-152">Log in.</span></span>
 
    <span data-ttu-id="32c8f-153">Les utilisateurs peuvent se connecter en cliquant sur le **connecter** lien en haut du site, ou ils peuvent être accédés à la page de connexion s’ils tentent d’accéder à une partie du site qui nécessite une autorisation.</span><span class="sxs-lookup"><span data-stu-id="32c8f-153">Users can sign in by clicking the **Log in** link at the top of the site, or they may be navigated to the Login page if they attempt to access a part of the site that requires authorization.</span></span> <span data-ttu-id="32c8f-154">Lorsque l’utilisateur envoie le formulaire sur la page de connexion, le ``AccountController`` ``Login`` action est appelée.</span><span class="sxs-lookup"><span data-stu-id="32c8f-154">When the user submits the form on the Login page, the ``AccountController`` ``Login`` action is called.</span></span>

    <span data-ttu-id="32c8f-155">Le ``Login`` action appelle ``PasswordSignInAsync`` sur la ``_signInManager`` objet (fourni à ``AccountController`` par injection de dépendance).</span><span class="sxs-lookup"><span data-stu-id="32c8f-155">The ``Login`` action calls ``PasswordSignInAsync`` on the ``_signInManager`` object (provided to ``AccountController`` by dependency injection).</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]
 
    <span data-ttu-id="32c8f-156">La base de ``Controller`` classe expose un ``User`` propriété que vous pouvez accéder à partir de méthodes de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="32c8f-156">The base ``Controller`` class exposes a ``User`` property that you can access from controller methods.</span></span> <span data-ttu-id="32c8f-157">Par exemple, vous pouvez énumérer `User.Claims` et prendre des décisions d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="32c8f-157">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="32c8f-158">Pour plus d’informations, consultez [autorisation](xref:security/authorization/index).</span><span class="sxs-lookup"><span data-stu-id="32c8f-158">For more information, see [Authorization](xref:security/authorization/index).</span></span>
 
5.  <span data-ttu-id="32c8f-159">Fermez la session.</span><span class="sxs-lookup"><span data-stu-id="32c8f-159">Log out.</span></span>
 
    <span data-ttu-id="32c8f-160">En cliquant sur le **déconnecter** lien appelle le `LogOut` action.</span><span class="sxs-lookup"><span data-stu-id="32c8f-160">Clicking the **Log out** link calls the `LogOut` action.</span></span>
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]
 
    <span data-ttu-id="32c8f-161">Le code précédent ci-dessus appelle le `_signInManager.SignOutAsync` (méthode).</span><span class="sxs-lookup"><span data-stu-id="32c8f-161">The preceding code above calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="32c8f-162">Le `SignOutAsync` méthode efface les revendications de l’utilisateur stockées dans un cookie.</span><span class="sxs-lookup"><span data-stu-id="32c8f-162">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>
 
6.  <span data-ttu-id="32c8f-163">Configuration.</span><span class="sxs-lookup"><span data-stu-id="32c8f-163">Configuration.</span></span>

    <span data-ttu-id="32c8f-164">Identité a certains comportements par défaut que vous pouvez substituer dans une classe de démarrage de votre application.</span><span class="sxs-lookup"><span data-stu-id="32c8f-164">Identity has some default behaviors that you can override in your application's startup class.</span></span> <span data-ttu-id="32c8f-165">Vous n’avez pas besoin de configurer ``IdentityOptions`` si vous utilisez les comportements par défaut.</span><span class="sxs-lookup"><span data-stu-id="32c8f-165">You do not need to configure ``IdentityOptions`` if you are using the default behaviors.</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="32c8f-166">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="32c8f-166">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="32c8f-167">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="32c8f-167">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-34)]

    ---
    
    <span data-ttu-id="32c8f-168">Pour plus d’informations sur la façon de configurer l’identité, consultez [configurer une identité](xref:security/authentication/identity-configuration).</span><span class="sxs-lookup"><span data-stu-id="32c8f-168">For more information about how to configure Identity, see [Configure Identity](xref:security/authentication/identity-configuration).</span></span>
    
    <span data-ttu-id="32c8f-169">Vous pouvez également configurer le type de données de la clé primaire, consultez [type de données de clés primaires de configurer une identité](xref:security/authentication/identity-primary-key-configuration).</span><span class="sxs-lookup"><span data-stu-id="32c8f-169">You also can configure the data type of the primary key, see [Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>
 
7.  <span data-ttu-id="32c8f-170">Afficher la base de données.</span><span class="sxs-lookup"><span data-stu-id="32c8f-170">View the database.</span></span>

    <span data-ttu-id="32c8f-171">Si votre application utilise une base de données SQL Server (la valeur par défaut sur Windows et pour les utilisateurs de Visual Studio), vous pouvez afficher la base de données de l’application créée.</span><span class="sxs-lookup"><span data-stu-id="32c8f-171">If your app is using a SQL Server database (the default on Windows and for Visual Studio users), you can view the database the app created.</span></span> <span data-ttu-id="32c8f-172">Vous pouvez utiliser **SQL Server Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="32c8f-172">You can use **SQL Server Management Studio**.</span></span> <span data-ttu-id="32c8f-173">Vous pouvez également, à partir de Visual Studio, sélectionnez **vue** -> **l’Explorateur d’objets SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="32c8f-173">Alternatively, from Visual Studio, select **View** -> **SQL Server Object Explorer**.</span></span> <span data-ttu-id="32c8f-174">Se connecter à **(localdb) \MSSQLLocalDB**.</span><span class="sxs-lookup"><span data-stu-id="32c8f-174">Connect to **(localdb)\MSSQLLocalDB**.</span></span> <span data-ttu-id="32c8f-175">La base de données dont le nom correspond  **aspnet - <*nom de votre projet*>-<*chaîne de date*> ** s’affiche.</span><span class="sxs-lookup"><span data-stu-id="32c8f-175">The database with a name matching **aspnet-<*name of your project*>-<*date string*>** is displayed.</span></span>

    ![Menu contextuel sur la table de base de données AspNetUsers](identity/_static/04-db.png)
    
    <span data-ttu-id="32c8f-177">Développez la base de données et ses **Tables**, puis cliquez sur le **dbo. AspNetUsers** de table et sélectionnez **des données d’affichage**.</span><span class="sxs-lookup"><span data-stu-id="32c8f-177">Expand the database and its **Tables**, then right-click the **dbo.AspNetUsers** table and select **View Data**.</span></span>

8. <span data-ttu-id="32c8f-178">Vérifiez le fonctionnement de l’identité</span><span class="sxs-lookup"><span data-stu-id="32c8f-178">Verify Identity works</span></span>

    <span data-ttu-id="32c8f-179">La valeur par défaut *Application ASP.NET Core Web* modèle de projet permet aux utilisateurs d’accéder à toute action dans l’application sans avoir à se connecter.</span><span class="sxs-lookup"><span data-stu-id="32c8f-179">The default *ASP.NET Core Web Application* project template allows users to access any action in the application without having to login.</span></span> <span data-ttu-id="32c8f-180">Pour vérifier que ASP.NET Identity fonctionne, ajoutez une`[Authorize]` d’attribut pour le `About` action de la `Home` contrôleur.</span><span class="sxs-lookup"><span data-stu-id="32c8f-180">To verify that ASP.NET Identity works, add an`[Authorize]` attribute to the `About` action of the `Home` Controller.</span></span>
 
    ```cs
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```
    
    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="32c8f-181">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="32c8f-181">Visual Studio</span></span>](#tab/visual-studio)

    <span data-ttu-id="32c8f-182">Exécutez le projet à l’aide de **Ctrl** + **F5** et accédez à la **sur** page.</span><span class="sxs-lookup"><span data-stu-id="32c8f-182">Run the project using **Ctrl** + **F5** and navigate to the **About** page.</span></span> <span data-ttu-id="32c8f-183">Seuls les utilisateurs authentifiés peuvent accéder à la **sur** page maintenant, donc ASP.NET vous redirige vers la page de connexion pour la connexion ou une inscription.</span><span class="sxs-lookup"><span data-stu-id="32c8f-183">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="32c8f-184">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="32c8f-184">.NET Core CLI</span></span>](#tab/netcore-cli)

    <span data-ttu-id="32c8f-185">Ouvrez une fenêtre de commande et accédez à la racine du projet répertoire contenant le `.csproj` fichier.</span><span class="sxs-lookup"><span data-stu-id="32c8f-185">Open a command window and navigate to the project's root directory containing the `.csproj` file.</span></span> <span data-ttu-id="32c8f-186">Exécutez le `dotnet run` commande pour exécuter l’application :</span><span class="sxs-lookup"><span data-stu-id="32c8f-186">Run the `dotnet run` command to run the app:</span></span>

    ```cs
    dotnet run 
    ```

    <span data-ttu-id="32c8f-187">Parcourir l’URL spécifiée dans la sortie de la `dotnet run` commande.</span><span class="sxs-lookup"><span data-stu-id="32c8f-187">Browse the URL specified in the output from the `dotnet run` command.</span></span> <span data-ttu-id="32c8f-188">L’URL doit pointer vers `localhost` avec un numéro de port généré.</span><span class="sxs-lookup"><span data-stu-id="32c8f-188">The URL should point to `localhost` with a generated port number.</span></span> <span data-ttu-id="32c8f-189">Accédez à la **sur** page.</span><span class="sxs-lookup"><span data-stu-id="32c8f-189">Navigate to the **About** page.</span></span> <span data-ttu-id="32c8f-190">Seuls les utilisateurs authentifiés peuvent accéder à la **sur** page maintenant, donc ASP.NET vous redirige vers la page de connexion pour la connexion ou une inscription.</span><span class="sxs-lookup"><span data-stu-id="32c8f-190">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    ---

## <a name="identity-components"></a><span data-ttu-id="32c8f-191">Composants de l’identité</span><span class="sxs-lookup"><span data-stu-id="32c8f-191">Identity Components</span></span>

<span data-ttu-id="32c8f-192">L’assembly de référence principale du système d’identité est `Microsoft.AspNetCore.Identity`.</span><span class="sxs-lookup"><span data-stu-id="32c8f-192">The primary reference assembly for the Identity system is `Microsoft.AspNetCore.Identity`.</span></span> <span data-ttu-id="32c8f-193">Ce package contient l’ensemble principal d’interfaces pour ASP.NET Core Identity et est inclus par `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="32c8f-193">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

<span data-ttu-id="32c8f-194">Ces dépendances sont nécessaires pour utiliser le système d’identité dans les applications ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="32c8f-194">These dependencies are needed to use the Identity system in ASP.NET Core applications:</span></span>

* <span data-ttu-id="32c8f-195">`Microsoft.AspNetCore.Identity.EntityFrameworkCore`-Contient les types requis pour utiliser l’identité avec Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="32c8f-195">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` - Contains the required types to use Identity with Entity Framework Core.</span></span>

* <span data-ttu-id="32c8f-196">`Microsoft.EntityFrameworkCore.SqlServer`-Entity Framework Core est la technologie d’accès aux données recommandée par Microsoft pour les bases de données relationnelles telles que SQL Server.</span><span class="sxs-lookup"><span data-stu-id="32c8f-196">`Microsoft.EntityFrameworkCore.SqlServer` - Entity Framework Core is Microsoft's recommended data access technology for relational databases like SQL Server.</span></span> <span data-ttu-id="32c8f-197">Pour le test, vous pouvez utiliser `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="32c8f-197">For testing, you can use `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

* <span data-ttu-id="32c8f-198">`Microsoft.AspNetCore.Authentication.Cookies`-Intergiciel (middleware) qui permet à une application utiliser l’authentification basée sur le cookie.</span><span class="sxs-lookup"><span data-stu-id="32c8f-198">`Microsoft.AspNetCore.Authentication.Cookies` - Middleware that enables an app to use cookie-based authentication.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="32c8f-199">Migration vers ASP.NET Core identité</span><span class="sxs-lookup"><span data-stu-id="32c8f-199">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="32c8f-200">Pour plus d’informations et des conseils sur la migration des identités existantes de votre magasin voir [migration de l’authentification et identité](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="32c8f-200">For additional information and guidance on migrating your existing Identity store see [Migrating Authentication and Identity](xref:migration/identity).</span></span>

## <a name="next-steps"></a><span data-ttu-id="32c8f-201">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="32c8f-201">Next Steps</span></span>

* [<span data-ttu-id="32c8f-202">Migration de l’authentification et de l’identité</span><span class="sxs-lookup"><span data-stu-id="32c8f-202">Migrating Authentication and Identity</span></span>](xref:migration/identity)
* [<span data-ttu-id="32c8f-203">Confirmation de compte et récupération de mot de passe</span><span class="sxs-lookup"><span data-stu-id="32c8f-203">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="32c8f-204">Authentification à deux facteurs avec SMS</span><span class="sxs-lookup"><span data-stu-id="32c8f-204">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="32c8f-205">Activation de l’authentification via Facebook, Google et d’autres fournisseurs externes</span><span class="sxs-lookup"><span data-stu-id="32c8f-205">Enabling authentication using Facebook, Google and other external providers</span></span>](xref:security/authentication/social/index)
