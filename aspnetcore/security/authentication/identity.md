---
title: "Introduction à l’identité sur ASP.NET Core"
author: rick-anderson
description: "Utiliser l’identité à une application ASP.NET Core"
ms.author: riande
manager: wpickett
ms.date: 01/02/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity
ms.openlocfilehash: 436a5ecfd126c9660591cd55efc1cc52b9493136
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="ede2e-103">Introduction à l’identité sur ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ede2e-103">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="ede2e-104">Par [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), et [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="ede2e-104">By [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="ede2e-105">Identité de ASP.NET Core est un système d’appartenance qui vous permet d’ajouter des fonctionnalités de connexion à votre application.</span><span class="sxs-lookup"><span data-stu-id="ede2e-105">ASP.NET Core Identity is a membership system which allows you to add login functionality to your application.</span></span> <span data-ttu-id="ede2e-106">Les utilisateurs peuvent créer une connexion et un compte avec un nom d’utilisateur et mot de passe, ou ils peuvent utiliser un fournisseur de connexion externe tels que Facebook, Google, Microsoft Account, Twitter ou d’autres.</span><span class="sxs-lookup"><span data-stu-id="ede2e-106">Users can create an account and login with a user name and password or they can use an external login provider such as Facebook, Google, Microsoft Account, Twitter or others.</span></span>

<span data-ttu-id="ede2e-107">Vous pouvez configurer l’identité du principal ASP.NET pour utiliser une base de données SQL Server pour stocker les noms d’utilisateur, les mots de passe et les données de profil.</span><span class="sxs-lookup"><span data-stu-id="ede2e-107">You can configure ASP.NET Core Identity to use a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="ede2e-108">Vous pouvez également utiliser votre propre magasin persistant, par exemple, un stockage de tables Azure.</span><span class="sxs-lookup"><span data-stu-id="ede2e-108">Alternatively, you can use your own persistent store, for example, an Azure Table Storage.</span></span> <span data-ttu-id="ede2e-109">Ce document contient des instructions pour Visual Studio et à l’aide de l’interface CLI.</span><span class="sxs-lookup"><span data-stu-id="ede2e-109">This document contains instructions for Visual Studio and for using the CLI.</span></span>

[<span data-ttu-id="ede2e-110">Afficher ou télécharger l’exemple de code.</span><span class="sxs-lookup"><span data-stu-id="ede2e-110">View or download the sample code.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [<span data-ttu-id="ede2e-111">(Comment télécharger)</span><span class="sxs-lookup"><span data-stu-id="ede2e-111">(How to download)</span></span>](https://docs.microsoft.com/en-us/aspnet/core/tutorials/index#how-to-download-a-sample)

## <a name="overview-of-identity"></a><span data-ttu-id="ede2e-112">Vue d’ensemble de l’identité</span><span class="sxs-lookup"><span data-stu-id="ede2e-112">Overview of Identity</span></span>

<span data-ttu-id="ede2e-113">Dans cette rubrique, vous allez apprendre à utiliser ASP.NET Core Identity pour ajouter des fonctionnalités pour vous inscrire, connectez-vous et déconnecter un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="ede2e-113">In this topic, you'll learn how to use ASP.NET Core Identity to add functionality to register, log in, and log out a user.</span></span> <span data-ttu-id="ede2e-114">Pour obtenir des instructions plus détaillées sur la création d’applications à l’aide d’ASP.NET Core Identity, consultez la section étapes suivantes à la fin de cet article.</span><span class="sxs-lookup"><span data-stu-id="ede2e-114">For more detailed instructions about creating apps using ASP.NET Core Identity, see the Next Steps section at the end of this article.</span></span>

1.  <span data-ttu-id="ede2e-115">Créez un projet d’Application ASP.NET Core Web avec des comptes d’utilisateur individuels.</span><span class="sxs-lookup"><span data-stu-id="ede2e-115">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ede2e-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ede2e-116">Visual Studio</span></span>](#tab/visual-studio)

    <span data-ttu-id="ede2e-117">Dans Visual Studio, sélectionnez **fichier** > **nouveau** > **projet**.</span><span class="sxs-lookup"><span data-stu-id="ede2e-117">In Visual Studio, select **File** > **New** > **Project**.</span></span> <span data-ttu-id="ede2e-118">Sélectionnez **Application ASP.NET Core Web** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="ede2e-118">Select **ASP.NET Core Web Application** and click **OK**.</span></span>

    ![Boîte de dialogue Nouveau projet](identity/_static/01-new-project.png)

    <span data-ttu-id="ede2e-120">Sélectionnez un ASP.NET Core **l’Application Web (Model-View-Controller)** pour ASP.NET 2.x de base, puis sélectionnez **modifier l’authentification**.</span><span class="sxs-lookup"><span data-stu-id="ede2e-120">Select an ASP.NET Core **Web Application (Model-View-Controller)** for ASP.NET Core 2.x, then select **Change Authentication**.</span></span>

    ![Boîte de dialogue Nouveau projet](identity/_static/02-new-project.png)

    <span data-ttu-id="ede2e-122">Une boîte de dialogue offre différentes possibilités d’authentification.</span><span class="sxs-lookup"><span data-stu-id="ede2e-122">A dialog appears offering authentication choices.</span></span> <span data-ttu-id="ede2e-123">Sélectionnez **comptes d’utilisateur individuels** et cliquez sur **OK** pour revenir à la boîte de dialogue précédente.</span><span class="sxs-lookup"><span data-stu-id="ede2e-123">Select **Individual User Accounts** and click **OK** to return to the previous dialog.</span></span>

    ![Boîte de dialogue Nouveau projet](identity/_static/03-new-project-auth.png)

    <span data-ttu-id="ede2e-125">En sélectionnant **comptes d’utilisateur individuels** dirige Visual Studio pour créer des modèles, ViewModel, vues, contrôleurs et autres composants requis pour l’authentification en tant que partie du modèle de projet.</span><span class="sxs-lookup"><span data-stu-id="ede2e-125">Selecting **Individual User Accounts** directs Visual Studio to create Models, ViewModels, Views, Controllers, and other assets required for authentication as part of the project template.</span></span>

    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ede2e-126">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="ede2e-126">.NET Core CLI</span></span>](#tab/netcore-cli)

    <span data-ttu-id="ede2e-127">Si vous utilisez l’interface de ligne de base de .NET, créer le projet à l’aide ``dotnet new mvc --auth Individual``.</span><span class="sxs-lookup"><span data-stu-id="ede2e-127">If using the .NET Core CLI, create the new project using ``dotnet new mvc --auth Individual``.</span></span> <span data-ttu-id="ede2e-128">Cette commande crée un nouveau projet avec le même code de modèle d’identité crée de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ede2e-128">This command creates a new project with the same Identity template code Visual Studio creates.</span></span>

    <span data-ttu-id="ede2e-129">Le projet créé contient le `Microsoft.AspNetCore.Identity.EntityFrameworkCore` package, qui conserve les données d’identité et le schéma à l’aide de SQL Server [Entity Framework Core](https://docs.microsoft.com/ef/).</span><span class="sxs-lookup"><span data-stu-id="ede2e-129">The created project contains the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` package, which persists the Identity data and schema to SQL Server using [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>

    ---

2.  <span data-ttu-id="ede2e-130">Configurer les services d’identité et d’ajouter l’intergiciel (middleware) dans `Startup`.</span><span class="sxs-lookup"><span data-stu-id="ede2e-130">Configure Identity services and add middleware in `Startup`.</span></span>

    <span data-ttu-id="ede2e-131">Les services d’identité sont ajoutés à l’application dans le `ConfigureServices` méthode dans la `Startup` classe :</span><span class="sxs-lookup"><span data-stu-id="ede2e-131">The Identity services are added to the application in the `ConfigureServices` method in the `Startup` class:</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ede2e-132">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ede2e-132">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    <span data-ttu-id="ede2e-133">Ces services sont accessibles à l’application via [injection de dépendance](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="ede2e-133">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
    
    <span data-ttu-id="ede2e-134">Identité est activée pour l’application en appelant `UseAuthentication` dans le `Configure` (méthode).</span><span class="sxs-lookup"><span data-stu-id="ede2e-134">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="ede2e-135">`UseAuthentication`Ajoute l’authentification [intergiciel (middleware)](xref:fundamentals/middleware) au pipeline de demande.</span><span class="sxs-lookup"><span data-stu-id="ede2e-135">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware) to the request pipeline.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ede2e-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ede2e-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-34)]
    
    <span data-ttu-id="ede2e-137">Ces services sont accessibles à l’application via [injection de dépendance](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="ede2e-137">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
    
    <span data-ttu-id="ede2e-138">Identité est activée pour l’application en appelant `UseIdentity` dans le `Configure` (méthode).</span><span class="sxs-lookup"><span data-stu-id="ede2e-138">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="ede2e-139">`UseIdentity`Ajoute l’authentification par cookie [intergiciel (middleware)](xref:fundamentals/middleware) au pipeline de demande.</span><span class="sxs-lookup"><span data-stu-id="ede2e-139">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware) to the request pipeline.</span></span>
        
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]
    
    ---
     
    <span data-ttu-id="ede2e-140">Pour plus d’informations sur le démarrage de l’application des processus, consultez [démarrage de l’Application](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="ede2e-140">For more information about the application start up process, see [Application Startup](xref:fundamentals/startup).</span></span>

3.  <span data-ttu-id="ede2e-141">Créez un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="ede2e-141">Create a user.</span></span>

    <span data-ttu-id="ede2e-142">Lancer l’application, puis cliquez sur le **inscrire** lien.</span><span class="sxs-lookup"><span data-stu-id="ede2e-142">Launch the application and then click on the **Register** link.</span></span>

    <span data-ttu-id="ede2e-143">S’il s’agit de la première fois que vous effectuez cette action, vous serez peut-être requis pour exécuter des migrations.</span><span class="sxs-lookup"><span data-stu-id="ede2e-143">If this is the first time you're performing this action, you may be required to run migrations.</span></span> <span data-ttu-id="ede2e-144">L’application vous invite à **s’appliquent les Migrations**.</span><span class="sxs-lookup"><span data-stu-id="ede2e-144">The application prompts you to **Apply Migrations**.</span></span> <span data-ttu-id="ede2e-145">Actualisez la page, si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="ede2e-145">Refresh the page if needed.</span></span>
    
    ![Appliquer la Page Web de Migrations](identity/_static/apply-migrations.png)
    
    <span data-ttu-id="ede2e-147">Ou bien, vous pouvez tester à l’aide d’ASP.NET Core Identity avec votre application sans une base de données persistantes à l’aide d’une base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="ede2e-147">Alternately, you can test using ASP.NET Core Identity with your app without a persistent database by using an in-memory database.</span></span> <span data-ttu-id="ede2e-148">Pour utiliser une base de données en mémoire, ajoutez le ``Microsoft.EntityFrameworkCore.InMemory`` le package à votre application et de modifier l’appel de votre application à ``AddDbContext`` dans ``ConfigureServices`` comme suit :</span><span class="sxs-lookup"><span data-stu-id="ede2e-148">To use an in-memory database, add the ``Microsoft.EntityFrameworkCore.InMemory`` package to your app and modify your app's call to ``AddDbContext`` in ``ConfigureServices`` as follows:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
    ```
    
    <span data-ttu-id="ede2e-149">Lorsque l’utilisateur clique sur le **inscrire** lien, le ``Register`` action est appelée sur ``AccountController``.</span><span class="sxs-lookup"><span data-stu-id="ede2e-149">When the user clicks the **Register** link, the ``Register`` action is invoked on ``AccountController``.</span></span> <span data-ttu-id="ede2e-150">Le ``Register`` action crée l’utilisateur en appelant `CreateAsync` sur la `_userManager` objet (fourni à ``AccountController`` par injection de dépendances) :</span><span class="sxs-lookup"><span data-stu-id="ede2e-150">The ``Register`` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to ``AccountController`` by dependency injection):</span></span>
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

    <span data-ttu-id="ede2e-151">Si l’utilisateur a été créé avec succès, l’utilisateur est connecté par l’appel à ``_signInManager.SignInAsync``.</span><span class="sxs-lookup"><span data-stu-id="ede2e-151">If the user was created successfully, the user is logged in by the call to ``_signInManager.SignInAsync``.</span></span>

    <span data-ttu-id="ede2e-152">**Remarque :** consultez [compte confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) pour connaître les étapes empêcher la connexion immédiate lors de l’inscription.</span><span class="sxs-lookup"><span data-stu-id="ede2e-152">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>
 
4.  <span data-ttu-id="ede2e-153">Se connecter.</span><span class="sxs-lookup"><span data-stu-id="ede2e-153">Log in.</span></span>
 
    <span data-ttu-id="ede2e-154">Les utilisateurs peuvent se connecter en cliquant sur le **connecter** lien en haut du site, ou ils peuvent être accédés à la page de connexion s’ils tentent d’accéder à une partie du site qui nécessite une autorisation.</span><span class="sxs-lookup"><span data-stu-id="ede2e-154">Users can sign in by clicking the **Log in** link at the top of the site, or they may be navigated to the Login page if they attempt to access a part of the site that requires authorization.</span></span> <span data-ttu-id="ede2e-155">Lorsque l’utilisateur envoie le formulaire sur la page de connexion, le ``AccountController`` ``Login`` action est appelée.</span><span class="sxs-lookup"><span data-stu-id="ede2e-155">When the user submits the form on the Login page, the ``AccountController`` ``Login`` action is called.</span></span>

    <span data-ttu-id="ede2e-156">Le ``Login`` action appelle ``PasswordSignInAsync`` sur la ``_signInManager`` objet (fourni à ``AccountController`` par injection de dépendance).</span><span class="sxs-lookup"><span data-stu-id="ede2e-156">The ``Login`` action calls ``PasswordSignInAsync`` on the ``_signInManager`` object (provided to ``AccountController`` by dependency injection).</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]
 
    <span data-ttu-id="ede2e-157">La base de ``Controller`` classe expose un ``User`` propriété que vous pouvez accéder à partir de méthodes de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="ede2e-157">The base ``Controller`` class exposes a ``User`` property that you can access from controller methods.</span></span> <span data-ttu-id="ede2e-158">Par exemple, vous pouvez énumérer `User.Claims` et prendre des décisions d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="ede2e-158">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="ede2e-159">Pour plus d’informations, consultez [autorisation](xref:security/authorization/index).</span><span class="sxs-lookup"><span data-stu-id="ede2e-159">For more information, see [Authorization](xref:security/authorization/index).</span></span>
 
5.  <span data-ttu-id="ede2e-160">Fermez la session.</span><span class="sxs-lookup"><span data-stu-id="ede2e-160">Log out.</span></span>
 
    <span data-ttu-id="ede2e-161">En cliquant sur le **déconnecter** lien appelle le `LogOut` action.</span><span class="sxs-lookup"><span data-stu-id="ede2e-161">Clicking the **Log out** link calls the `LogOut` action.</span></span>
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]
 
    <span data-ttu-id="ede2e-162">Le code précédent ci-dessus appelle le `_signInManager.SignOutAsync` (méthode).</span><span class="sxs-lookup"><span data-stu-id="ede2e-162">The preceding code above calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="ede2e-163">Le `SignOutAsync` méthode efface les revendications de l’utilisateur stockées dans un cookie.</span><span class="sxs-lookup"><span data-stu-id="ede2e-163">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>
 
6.  <span data-ttu-id="ede2e-164">Configuration.</span><span class="sxs-lookup"><span data-stu-id="ede2e-164">Configuration.</span></span>

    <span data-ttu-id="ede2e-165">Identité a certains comportements par défaut que vous pouvez substituer dans une classe de démarrage de votre application.</span><span class="sxs-lookup"><span data-stu-id="ede2e-165">Identity has some default behaviors that you can override in your application's startup class.</span></span> <span data-ttu-id="ede2e-166">Vous n’avez pas besoin de configurer ``IdentityOptions`` si vous utilisez les comportements par défaut.</span><span class="sxs-lookup"><span data-stu-id="ede2e-166">You do not need to configure ``IdentityOptions`` if you are using the default behaviors.</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ede2e-167">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ede2e-167">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ede2e-168">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ede2e-168">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-34)]

    ---
    
    <span data-ttu-id="ede2e-169">Pour plus d’informations sur la façon de configurer l’identité, consultez [configurer une identité](xref:security/authentication/identity-configuration).</span><span class="sxs-lookup"><span data-stu-id="ede2e-169">For more information about how to configure Identity, see [Configure Identity](xref:security/authentication/identity-configuration).</span></span>
    
    <span data-ttu-id="ede2e-170">Vous pouvez également configurer le type de données de la clé primaire, consultez [type de données de clés primaires de configurer une identité](xref:security/authentication/identity-primary-key-configuration).</span><span class="sxs-lookup"><span data-stu-id="ede2e-170">You also can configure the data type of the primary key, see [Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>
 
7.  <span data-ttu-id="ede2e-171">Afficher la base de données.</span><span class="sxs-lookup"><span data-stu-id="ede2e-171">View the database.</span></span>

    <span data-ttu-id="ede2e-172">Si votre application utilise une base de données SQL Server (la valeur par défaut sur Windows et pour les utilisateurs de Visual Studio), vous pouvez afficher la base de données de l’application créée.</span><span class="sxs-lookup"><span data-stu-id="ede2e-172">If your app is using a SQL Server database (the default on Windows and for Visual Studio users), you can view the database the app created.</span></span> <span data-ttu-id="ede2e-173">Vous pouvez utiliser **SQL Server Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="ede2e-173">You can use **SQL Server Management Studio**.</span></span> <span data-ttu-id="ede2e-174">Vous pouvez également, à partir de Visual Studio, sélectionnez **vue** > **l’Explorateur d’objets SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="ede2e-174">Alternatively, from Visual Studio, select **View** > **SQL Server Object Explorer**.</span></span> <span data-ttu-id="ede2e-175">Se connecter à **(localdb) \MSSQLLocalDB**.</span><span class="sxs-lookup"><span data-stu-id="ede2e-175">Connect to **(localdb)\MSSQLLocalDB**.</span></span> <span data-ttu-id="ede2e-176">La base de données dont le nom correspond **aspnet - <*nom de votre projet*>-<*chaîne de date* >**  s’affiche.</span><span class="sxs-lookup"><span data-stu-id="ede2e-176">The database with a name matching **aspnet-<*name of your project*>-<*date string*>** is displayed.</span></span>

    ![Menu contextuel sur la table de base de données AspNetUsers](identity/_static/04-db.png)
    
    <span data-ttu-id="ede2e-178">Développez la base de données et ses **Tables**, puis cliquez sur le **dbo. AspNetUsers** de table et sélectionnez **des données d’affichage**.</span><span class="sxs-lookup"><span data-stu-id="ede2e-178">Expand the database and its **Tables**, then right-click the **dbo.AspNetUsers** table and select **View Data**.</span></span>

8. <span data-ttu-id="ede2e-179">Vérifiez le fonctionnement de l’identité</span><span class="sxs-lookup"><span data-stu-id="ede2e-179">Verify Identity works</span></span>

    <span data-ttu-id="ede2e-180">La valeur par défaut *Application ASP.NET Core Web* modèle de projet permet aux utilisateurs d’accéder à toute action dans l’application sans avoir à se connecter.</span><span class="sxs-lookup"><span data-stu-id="ede2e-180">The default *ASP.NET Core Web Application* project template allows users to access any action in the application without having to login.</span></span> <span data-ttu-id="ede2e-181">Pour vérifier que ASP.NET Identity fonctionne, ajoutez une`[Authorize]` d’attribut pour le `About` action de la `Home` contrôleur.</span><span class="sxs-lookup"><span data-stu-id="ede2e-181">To verify that ASP.NET Identity works, add an`[Authorize]` attribute to the `About` action of the `Home` Controller.</span></span>
 
    ```cs
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```
    
    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ede2e-182">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ede2e-182">Visual Studio</span></span>](#tab/visual-studio)

    <span data-ttu-id="ede2e-183">Exécutez le projet à l’aide de **Ctrl** + **F5** et accédez à la **sur** page.</span><span class="sxs-lookup"><span data-stu-id="ede2e-183">Run the project using **Ctrl** + **F5** and navigate to the **About** page.</span></span> <span data-ttu-id="ede2e-184">Seuls les utilisateurs authentifiés peuvent accéder à la **sur** page maintenant, donc ASP.NET vous redirige vers la page de connexion pour la connexion ou une inscription.</span><span class="sxs-lookup"><span data-stu-id="ede2e-184">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ede2e-185">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="ede2e-185">.NET Core CLI</span></span>](#tab/netcore-cli)

    <span data-ttu-id="ede2e-186">Ouvrez une fenêtre de commande et accédez à la racine du projet répertoire contenant le `.csproj` fichier.</span><span class="sxs-lookup"><span data-stu-id="ede2e-186">Open a command window and navigate to the project's root directory containing the `.csproj` file.</span></span> <span data-ttu-id="ede2e-187">Exécutez le `dotnet run` commande pour exécuter l’application :</span><span class="sxs-lookup"><span data-stu-id="ede2e-187">Run the `dotnet run` command to run the app:</span></span>

    ```cs
    dotnet run 
    ```

    <span data-ttu-id="ede2e-188">Parcourir l’URL spécifiée dans la sortie de la `dotnet run` commande.</span><span class="sxs-lookup"><span data-stu-id="ede2e-188">Browse the URL specified in the output from the `dotnet run` command.</span></span> <span data-ttu-id="ede2e-189">L’URL doit pointer vers `localhost` avec un numéro de port généré.</span><span class="sxs-lookup"><span data-stu-id="ede2e-189">The URL should point to `localhost` with a generated port number.</span></span> <span data-ttu-id="ede2e-190">Accédez à la **sur** page.</span><span class="sxs-lookup"><span data-stu-id="ede2e-190">Navigate to the **About** page.</span></span> <span data-ttu-id="ede2e-191">Seuls les utilisateurs authentifiés peuvent accéder à la **sur** page maintenant, donc ASP.NET vous redirige vers la page de connexion pour la connexion ou une inscription.</span><span class="sxs-lookup"><span data-stu-id="ede2e-191">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    ---

## <a name="identity-components"></a><span data-ttu-id="ede2e-192">Composants de l’identité</span><span class="sxs-lookup"><span data-stu-id="ede2e-192">Identity Components</span></span>

<span data-ttu-id="ede2e-193">L’assembly de référence principale du système d’identité est `Microsoft.AspNetCore.Identity`.</span><span class="sxs-lookup"><span data-stu-id="ede2e-193">The primary reference assembly for the Identity system is `Microsoft.AspNetCore.Identity`.</span></span> <span data-ttu-id="ede2e-194">Ce package contient l’ensemble principal d’interfaces pour ASP.NET Core Identity et est inclus par `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="ede2e-194">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

<span data-ttu-id="ede2e-195">Ces dépendances sont nécessaires pour utiliser le système d’identité dans les applications ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="ede2e-195">These dependencies are needed to use the Identity system in ASP.NET Core applications:</span></span>

* <span data-ttu-id="ede2e-196">`Microsoft.AspNetCore.Identity.EntityFrameworkCore`-Contient les types requis pour utiliser l’identité avec Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="ede2e-196">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` - Contains the required types to use Identity with Entity Framework Core.</span></span>

* <span data-ttu-id="ede2e-197">`Microsoft.EntityFrameworkCore.SqlServer`-Entity Framework Core est la technologie d’accès aux données recommandée par Microsoft pour les bases de données relationnelles telles que SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ede2e-197">`Microsoft.EntityFrameworkCore.SqlServer` - Entity Framework Core is Microsoft's recommended data access technology for relational databases like SQL Server.</span></span> <span data-ttu-id="ede2e-198">Pour le test, vous pouvez utiliser `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="ede2e-198">For testing, you can use `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

* <span data-ttu-id="ede2e-199">`Microsoft.AspNetCore.Authentication.Cookies`-Intergiciel (middleware) qui permet à une application utiliser l’authentification basée sur le cookie.</span><span class="sxs-lookup"><span data-stu-id="ede2e-199">`Microsoft.AspNetCore.Authentication.Cookies` - Middleware that enables an app to use cookie-based authentication.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="ede2e-200">Migration vers ASP.NET Core identité</span><span class="sxs-lookup"><span data-stu-id="ede2e-200">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="ede2e-201">Pour plus d’informations et des conseils sur la migration des identités existantes de votre magasin voir [migration de l’authentification et identité](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="ede2e-201">For additional information and guidance on migrating your existing Identity store see [Migrating Authentication and Identity](xref:migration/identity).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ede2e-202">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="ede2e-202">Next Steps</span></span>

* [<span data-ttu-id="ede2e-203">Migration de l’authentification et de l’identité</span><span class="sxs-lookup"><span data-stu-id="ede2e-203">Migrating Authentication and Identity</span></span>](xref:migration/identity)
* [<span data-ttu-id="ede2e-204">Confirmation de compte et récupération de mot de passe</span><span class="sxs-lookup"><span data-stu-id="ede2e-204">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="ede2e-205">Authentification à deux facteurs avec SMS</span><span class="sxs-lookup"><span data-stu-id="ede2e-205">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="ede2e-206">Activation de l’authentification via Facebook, Google et d’autres fournisseurs externes</span><span class="sxs-lookup"><span data-stu-id="ede2e-206">Enabling authentication using Facebook, Google and other external providers</span></span>](xref:security/authentication/social/index)
