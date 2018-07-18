---
title: Introduction à Identity sur ASP.NET Core
author: rick-anderson
description: Utiliser Identity à une application ASP.NET Core Inclut, exigences de mot de passe de paramètre (RequireDigit, RequiredLength, RequiredUniqueChars et bien plus encore).
ms.author: riande
ms.date: 01/24/2018
uid: security/authentication/identity
ms.openlocfilehash: 50ddb96000e6a3f9e1762e9bb3e1f215f20d4356
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095637"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="591fc-104">Introduction à Identity sur ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="591fc-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="591fc-105">Par [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Nowak](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), et [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="591fc-105">By [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="591fc-106">Identity de ASP.NET Core est un système d’appartenance qui vous permet d’ajouter des fonctionnalités de connexion à votre application.</span><span class="sxs-lookup"><span data-stu-id="591fc-106">ASP.NET Core Identity is a membership system which allows you to add login functionality to your application.</span></span> <span data-ttu-id="591fc-107">Les utilisateurs peuvent créer une connexion et un compte avec un nom d’utilisateur et mot de passe, ou ils peuvent utiliser un fournisseur de connexion externe tels que Facebook, Google, Microsoft Account, Twitter ou d’autres.</span><span class="sxs-lookup"><span data-stu-id="591fc-107">Users can create an account and login with a user name and password or they can use an external login provider such as Facebook, Google, Microsoft Account, Twitter or others.</span></span>

<span data-ttu-id="591fc-108">Vous pouvez configurer Identity du principal ASP.NET pour utiliser une base de données SQL Server pour stocker les noms d’utilisateur, les mots de passe et les données de profil.</span><span class="sxs-lookup"><span data-stu-id="591fc-108">You can configure ASP.NET Core Identity to use a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="591fc-109">Vous pouvez également utiliser votre propre magasin persistant, par exemple, un stockage de tables Azure.</span><span class="sxs-lookup"><span data-stu-id="591fc-109">Alternatively, you can use your own persistent store, for example, an Azure Table Storage.</span></span> <span data-ttu-id="591fc-110">Ce document contient des instructions pour Visual Studio et à l’aide de l’interface CLI.</span><span class="sxs-lookup"><span data-stu-id="591fc-110">This document contains instructions for Visual Studio and for using the CLI.</span></span>

[<span data-ttu-id="591fc-111">Afficher ou télécharger l’exemple de code.</span><span class="sxs-lookup"><span data-stu-id="591fc-111">View or download the sample code.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [<span data-ttu-id="591fc-112">(Comment télécharger)</span><span class="sxs-lookup"><span data-stu-id="591fc-112">(How to download)</span></span>](xref:tutorials/index#how-to-download-a-sample)

## <a name="overview-of-identity"></a><span data-ttu-id="591fc-113">Vue d’ensemble d'Identity</span><span class="sxs-lookup"><span data-stu-id="591fc-113">Overview of Identity</span></span>

<span data-ttu-id="591fc-114">Dans cette rubrique, vous allez apprendre à utiliser ASP.NET Core Identity pour ajouter des fonctionnalités pour vous inscrire, connectez-vous et déconnecter un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="591fc-114">In this topic, you'll learn how to use ASP.NET Core Identity to add functionality to register, log in, and log out a user.</span></span> <span data-ttu-id="591fc-115">Pour obtenir des instructions plus détaillées sur la création d’applications à l’aide d’ASP.NET Core Identity, consultez la section étapes suivantes à la fin de cet article.</span><span class="sxs-lookup"><span data-stu-id="591fc-115">For more detailed instructions about creating apps using ASP.NET Core Identity, see the Next Steps section at the end of this article.</span></span>

1. <span data-ttu-id="591fc-116">Créer un projet d’Application Web ASP.NET Core avec des comptes d’utilisateur individuels.</span><span class="sxs-lookup"><span data-stu-id="591fc-116">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="591fc-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="591fc-117">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="591fc-118">Dans Visual Studio, sélectionnez **fichier** > **New** > **projet**.</span><span class="sxs-lookup"><span data-stu-id="591fc-118">In Visual Studio, select **File** > **New** > **Project**.</span></span> <span data-ttu-id="591fc-119">Sélectionnez **Application Web ASP.NET Core** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="591fc-119">Select **ASP.NET Core Web Application** and click **OK**.</span></span>

   ![Boîte de dialogue Nouveau projet](identity/_static/01-new-project.png)

   <span data-ttu-id="591fc-121">Sélectionnez une ASP.NET Core **l’Application Web (Model-View-Controller)** pour ASP.NET Core 2.x, puis sélectionnez **modifier l’authentification**.</span><span class="sxs-lookup"><span data-stu-id="591fc-121">Select an ASP.NET Core **Web Application (Model-View-Controller)** for ASP.NET Core 2.x, then select **Change Authentication**.</span></span>

   ![Boîte de dialogue Nouveau projet](identity/_static/02-new-project.png)

   <span data-ttu-id="591fc-123">Une boîte de dialogue apparaît offre différentes possibilités d’authentification.</span><span class="sxs-lookup"><span data-stu-id="591fc-123">A dialog appears offering authentication choices.</span></span> <span data-ttu-id="591fc-124">Sélectionnez **comptes d’utilisateur individuels** et cliquez sur **OK** pour revenir à la boîte de dialogue précédente.</span><span class="sxs-lookup"><span data-stu-id="591fc-124">Select **Individual User Accounts** and click **OK** to return to the previous dialog.</span></span>

   ![Boîte de dialogue Nouveau projet](identity/_static/03-new-project-auth.png)

   <span data-ttu-id="591fc-126">En sélectionnant **comptes d’utilisateur individuels** dirige Visual Studio pour créer des modèles, les ViewModels, les vues, les contrôleurs et les autres ressources nécessaires pour l’authentification en tant que partie du modèle de projet.</span><span class="sxs-lookup"><span data-stu-id="591fc-126">Selecting **Individual User Accounts** directs Visual Studio to create Models, ViewModels, Views, Controllers, and other assets required for authentication as part of the project template.</span></span>

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="591fc-127">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="591fc-127">.NET Core CLI</span></span>](#tab/netcore-cli)

   <span data-ttu-id="591fc-128">Si vous utilisez l’interface de ligne de base de .NET, créer le projet à l’aide `dotnet new mvc --auth Individual`.</span><span class="sxs-lookup"><span data-stu-id="591fc-128">If using the .NET Core CLI, create the new project using `dotnet new mvc --auth Individual`.</span></span> <span data-ttu-id="591fc-129">Cette commande crée un nouveau projet avec le même code de modèle d’Identity crée de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="591fc-129">This command creates a new project with the same Identity template code Visual Studio creates.</span></span>

   <span data-ttu-id="591fc-130">Le projet créé contient le `Microsoft.AspNetCore.Identity.EntityFrameworkCore` package, qui conserve les données d’Identity et le schéma à l’aide de SQL Server [Entity Framework Core](https://docs.microsoft.com/ef/).</span><span class="sxs-lookup"><span data-stu-id="591fc-130">The created project contains the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` package, which persists the Identity data and schema to SQL Server using [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>

   ---

2. <span data-ttu-id="591fc-131">Configurer les services d’Identity et d’ajouter l’intergiciel (middleware) dans `Startup`.</span><span class="sxs-lookup"><span data-stu-id="591fc-131">Configure Identity services and add middleware in `Startup`.</span></span>

   <span data-ttu-id="591fc-132">Les services d’Identity sont ajoutés à l’application dans le `ConfigureServices` méthode dans la `Startup` classe :</span><span class="sxs-lookup"><span data-stu-id="591fc-132">The Identity services are added to the application in the `ConfigureServices` method in the `Startup` class:</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="591fc-133">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="591fc-133">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   <span data-ttu-id="591fc-134">Ces services sont accessibles à l’application via [l’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="591fc-134">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="591fc-135">Identity est activée pour l’application en appelant `UseAuthentication` dans le `Configure` (méthode).</span><span class="sxs-lookup"><span data-stu-id="591fc-135">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="591fc-136">`UseAuthentication`Ajoute l’authentification [intergiciel (middleware)](xref:fundamentals/middleware/index) au pipeline de demande.</span><span class="sxs-lookup"><span data-stu-id="591fc-136">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="591fc-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="591fc-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   <span data-ttu-id="591fc-138">Ces services sont accessibles à l’application via [l’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="591fc-138">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="591fc-139">Identity est activée pour l’application en appelant `UseIdentity` dans le `Configure` (méthode).</span><span class="sxs-lookup"><span data-stu-id="591fc-139">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="591fc-140">`UseIdentity`Ajoute l’authentification par cookie [intergiciel (middleware)](xref:fundamentals/middleware/index) au pipeline de demande.</span><span class="sxs-lookup"><span data-stu-id="591fc-140">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

   ---

   <span data-ttu-id="591fc-141">Pour plus d’informations sur le démarrage de l’application des processus, consultez [démarrage de l’Application](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="591fc-141">For more information about the application start up process, see [Application Startup](xref:fundamentals/startup).</span></span>

3. <span data-ttu-id="591fc-142">Créer un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="591fc-142">Create a user.</span></span>

   <span data-ttu-id="591fc-143">Lancer l’application, puis cliquez sur le **inscrire** lien.</span><span class="sxs-lookup"><span data-stu-id="591fc-143">Launch the application and then click on the **Register** link.</span></span>

   <span data-ttu-id="591fc-144">S’il s’agit de la première fois que vous effectuez cette action, vous serez peut-être requis pour exécuter des migrations.</span><span class="sxs-lookup"><span data-stu-id="591fc-144">If this is the first time you're performing this action, you may be required to run migrations.</span></span> <span data-ttu-id="591fc-145">L’application vous invite à **appliquer les Migrations**.</span><span class="sxs-lookup"><span data-stu-id="591fc-145">The application prompts you to **Apply Migrations**.</span></span> <span data-ttu-id="591fc-146">Actualisez la page si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="591fc-146">Refresh the page if needed.</span></span>

   ![Appliquer des Migrations Web Page](identity/_static/apply-migrations.png)

   <span data-ttu-id="591fc-148">Alternativement, vous pouvez tester à l’aide d’ASP.NET Core Identity avec votre application sans une base de données persistante à l’aide d’une base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="591fc-148">Alternately, you can test using ASP.NET Core Identity with your app without a persistent database by using an in-memory database.</span></span> <span data-ttu-id="591fc-149">Pour utiliser une base de données en mémoire, ajoutez le `Microsoft.EntityFrameworkCore.InMemory` package sur votre application, puis modifiez l’appel de votre application à `AddDbContext` dans `ConfigureServices` comme suit :</span><span class="sxs-lookup"><span data-stu-id="591fc-149">To use an in-memory database, add the `Microsoft.EntityFrameworkCore.InMemory` package to your app and modify your app's call to `AddDbContext` in `ConfigureServices` as follows:</span></span>

   ```csharp
   services.AddDbContext<ApplicationDbContext>(options =>
       options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
   ```

   <span data-ttu-id="591fc-150">Lorsque l’utilisateur clique sur le **inscrire** lien, le `Register` action est appelée sur `AccountController`.</span><span class="sxs-lookup"><span data-stu-id="591fc-150">When the user clicks the **Register** link, the `Register` action is invoked on `AccountController`.</span></span> <span data-ttu-id="591fc-151">Le `Register` action crée l’utilisateur en appelant `CreateAsync` sur le `_userManager` objet (fourni à `AccountController` par injection de dépendances) :</span><span class="sxs-lookup"><span data-stu-id="591fc-151">The `Register` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to `AccountController` by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

   <span data-ttu-id="591fc-152">Si l’utilisateur a été créé avec succès, l’utilisateur est connecté par l’appel à `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="591fc-152">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

   <span data-ttu-id="591fc-153">**Remarque :** consultez [confirmation de compte](xref:security/authentication/accconfirm#prevent-login-at-registration) pour savoir comment empêcher la connexion immédiate lors de l’inscription.</span><span class="sxs-lookup"><span data-stu-id="591fc-153">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

4. <span data-ttu-id="591fc-154">Connectez-vous.</span><span class="sxs-lookup"><span data-stu-id="591fc-154">Log in.</span></span>

   <span data-ttu-id="591fc-155">Les utilisateurs peuvent se connecter en cliquant sur le **connectez-vous** lien en haut du site, ou peut être accédés à la page de connexion s’ils tentent d’accéder à une partie du site qui nécessite une autorisation.</span><span class="sxs-lookup"><span data-stu-id="591fc-155">Users can sign in by clicking the **Log in** link at the top of the site, or they may be navigated to the Login page if they attempt to access a part of the site that requires authorization.</span></span> <span data-ttu-id="591fc-156">Lorsque l’utilisateur soumet le formulaire sur la page de connexion, le `AccountController` `Login` action est appelée.</span><span class="sxs-lookup"><span data-stu-id="591fc-156">When the user submits the form on the Login page, the `AccountController` `Login` action is called.</span></span>

   <span data-ttu-id="591fc-157">Le `Login` action appels `PasswordSignInAsync` sur le `_signInManager` objet (fourni à `AccountController` par injection de dépendances).</span><span class="sxs-lookup"><span data-stu-id="591fc-157">The `Login` action calls `PasswordSignInAsync` on the `_signInManager` object (provided to `AccountController` by dependency injection).</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

   <span data-ttu-id="591fc-158">La base de `Controller` classe expose un `User` propriété auxquelles vous pouvez accéder à partir de méthodes de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="591fc-158">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="591fc-159">Par exemple, vous pouvez énumérer `User.Claims` et prendre des décisions d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="591fc-159">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="591fc-160">Pour plus d’informations, consultez [autorisation](xref:security/authorization/index).</span><span class="sxs-lookup"><span data-stu-id="591fc-160">For more information, see [Authorization](xref:security/authorization/index).</span></span>

5. <span data-ttu-id="591fc-161">Se déconnecter.</span><span class="sxs-lookup"><span data-stu-id="591fc-161">Log out.</span></span>

   <span data-ttu-id="591fc-162">En cliquant sur le **déconnecter** lien appelle le `LogOut` action.</span><span class="sxs-lookup"><span data-stu-id="591fc-162">Clicking the **Log out** link calls the `LogOut` action.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   <span data-ttu-id="591fc-163">Le code précédent ci-dessus appelle le `_signInManager.SignOutAsync` (méthode).</span><span class="sxs-lookup"><span data-stu-id="591fc-163">The preceding code above calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="591fc-164">Le `SignOutAsync` méthode efface les revendications de l’utilisateur stockées dans un cookie.</span><span class="sxs-lookup"><span data-stu-id="591fc-164">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>

<a name="pw"></a>
6. <span data-ttu-id="591fc-165">Configuration.</span><span class="sxs-lookup"><span data-stu-id="591fc-165">Configuration.</span></span>

   <span data-ttu-id="591fc-166">L’identité comporte certains comportements par défaut qui peuvent être substituées dans une classe de démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="591fc-166">Identity has some default behaviors that can be overridden in the app's startup class.</span></span> <span data-ttu-id="591fc-167">`IdentityOptions` ne pas devoir être configuré si vous utilisez les comportements par défaut.</span><span class="sxs-lookup"><span data-stu-id="591fc-167">`IdentityOptions` don't need to be configured when using the default behaviors.</span></span> <span data-ttu-id="591fc-168">Le code suivant définit plusieurs options de force de mot de passe :</span><span class="sxs-lookup"><span data-stu-id="591fc-168">The following code sets several password strength options:</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="591fc-169">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="591fc-169">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="591fc-170">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="591fc-170">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-33)]

   ---

   <span data-ttu-id="591fc-171">Pour plus d’informations sur la façon de configurer Identity, consultez [configurer Identity](xref:security/authentication/identity-configuration).</span><span class="sxs-lookup"><span data-stu-id="591fc-171">For more information about how to configure Identity, see [Configure Identity](xref:security/authentication/identity-configuration).</span></span>

   <span data-ttu-id="591fc-172">Vous pouvez également configurer le type de données de la clé primaire, consultez [type de données de clés primaires de configurer Identity](xref:security/authentication/identity-primary-key-configuration).</span><span class="sxs-lookup"><span data-stu-id="591fc-172">You also can configure the data type of the primary key, see [Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>

7. <span data-ttu-id="591fc-173">Afficher la base de données.</span><span class="sxs-lookup"><span data-stu-id="591fc-173">View the database.</span></span>

   <span data-ttu-id="591fc-174">Si votre application utilise une base de données SQL Server (la valeur par défaut sur Windows et pour les utilisateurs de Visual Studio), vous pouvez afficher la base de données de l’application créée.</span><span class="sxs-lookup"><span data-stu-id="591fc-174">If your app is using a SQL Server database (the default on Windows and for Visual Studio users), you can view the database the app created.</span></span> <span data-ttu-id="591fc-175">Vous pouvez utiliser **SQL Server Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="591fc-175">You can use **SQL Server Management Studio**.</span></span> <span data-ttu-id="591fc-176">Vous pouvez également, à partir de Visual Studio, sélectionnez **vue** > **Explorateur d’objets SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="591fc-176">Alternatively, from Visual Studio, select **View** > **SQL Server Object Explorer**.</span></span> <span data-ttu-id="591fc-177">Se connecter à **(localdb) \MSSQLLocalDB**.</span><span class="sxs-lookup"><span data-stu-id="591fc-177">Connect to **(localdb)\MSSQLLocalDB**.</span></span> <span data-ttu-id="591fc-178">La base de données dont le nom correspond `aspnet-<name of your project>-<guid>` s’affiche.</span><span class="sxs-lookup"><span data-stu-id="591fc-178">The database with a name matching `aspnet-<name of your project>-<guid>` is displayed.</span></span>

   ![Menu contextuel sur la table de base de données AspNetUsers](identity/_static/04-db.png)

   <span data-ttu-id="591fc-180">Développez la base de données et ses **Tables**, puis cliquez sur le **dbo. AspNetUsers** de table et sélectionnez **afficher les données**.</span><span class="sxs-lookup"><span data-stu-id="591fc-180">Expand the database and its **Tables**, then right-click the **dbo.AspNetUsers** table and select **View Data**.</span></span>

8. <span data-ttu-id="591fc-181">Vérifier le fonctionnement de l’identité</span><span class="sxs-lookup"><span data-stu-id="591fc-181">Verify Identity works</span></span>

    <span data-ttu-id="591fc-182">La valeur par défaut *Application Web ASP.NET Core* modèle de projet permet aux utilisateurs d’accéder à toute action dans l’application sans avoir à la connexion.</span><span class="sxs-lookup"><span data-stu-id="591fc-182">The default *ASP.NET Core Web Application* project template allows users to access any action in the application without having to login.</span></span> <span data-ttu-id="591fc-183">Pour vérifier que ASP.NET Identity fonctionne, ajoutez un`[Authorize]` attribut le `About` action de la `Home` contrôleur.</span><span class="sxs-lookup"><span data-stu-id="591fc-183">To verify that ASP.NET Identity works, add an`[Authorize]` attribute to the `About` action of the `Home` Controller.</span></span>

    ```csharp
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```

    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="591fc-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="591fc-184">Visual Studio</span></span>](#tab/visual-studio)

    <span data-ttu-id="591fc-185">Exécutez le projet en utilisant **Ctrl** + **F5** et accédez à la **sur** page.</span><span class="sxs-lookup"><span data-stu-id="591fc-185">Run the project using **Ctrl** + **F5** and navigate to the **About** page.</span></span> <span data-ttu-id="591fc-186">Seuls les utilisateurs authentifiés peuvent accéder à la **sur** page maintenant, afin d’ASP.NET vous redirige vers la page de connexion pour la connexion ou une inscription.</span><span class="sxs-lookup"><span data-stu-id="591fc-186">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="591fc-187">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="591fc-187">.NET Core CLI</span></span>](#tab/netcore-cli)

    <span data-ttu-id="591fc-188">Ouvrez une fenêtre de commande et accédez à la racine du projet répertoire contenant le `.csproj` fichier.</span><span class="sxs-lookup"><span data-stu-id="591fc-188">Open a command window and navigate to the project's root directory containing the `.csproj` file.</span></span> <span data-ttu-id="591fc-189">Exécutez le [dotnet exécuter](/dotnet/core/tools/dotnet-run) commande pour exécuter l’application :</span><span class="sxs-lookup"><span data-stu-id="591fc-189">Run the [dotnet run](/dotnet/core/tools/dotnet-run) command to run the app:</span></span>

    ```csharp
    dotnet run 
    ```

    <span data-ttu-id="591fc-190">Parcourir l’URL spécifiée dans la sortie de la [dotnet exécuter](/dotnet/core/tools/dotnet-run) commande.</span><span class="sxs-lookup"><span data-stu-id="591fc-190">Browse the URL specified in the output from the [dotnet run](/dotnet/core/tools/dotnet-run) command.</span></span> <span data-ttu-id="591fc-191">L’URL doit pointer vers `localhost` avec un numéro de port généré.</span><span class="sxs-lookup"><span data-stu-id="591fc-191">The URL should point to `localhost` with a generated port number.</span></span> <span data-ttu-id="591fc-192">Accédez à la **sur** page.</span><span class="sxs-lookup"><span data-stu-id="591fc-192">Navigate to the **About** page.</span></span> <span data-ttu-id="591fc-193">Seuls les utilisateurs authentifiés peuvent accéder à la **sur** page maintenant, afin d’ASP.NET vous redirige vers la page de connexion pour la connexion ou une inscription.</span><span class="sxs-lookup"><span data-stu-id="591fc-193">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    ---

## <a name="identity-components"></a><span data-ttu-id="591fc-194">Composants d'Identity</span><span class="sxs-lookup"><span data-stu-id="591fc-194">Identity Components</span></span>

<span data-ttu-id="591fc-195">L’assembly de référence principale du système d'Identity est `Microsoft.AspNetCore.Identity`.</span><span class="sxs-lookup"><span data-stu-id="591fc-195">The primary reference assembly for the Identity system is `Microsoft.AspNetCore.Identity`.</span></span> <span data-ttu-id="591fc-196">Ce package contient l’ensemble principal d’interfaces pour ASP.NET Core Identity et est inclus par `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="591fc-196">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

<span data-ttu-id="591fc-197">Ces dépendances sont nécessaires pour utiliser le système d'Identity dans les applications ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="591fc-197">These dependencies are needed to use the Identity system in ASP.NET Core applications:</span></span>

* <span data-ttu-id="591fc-198">`Microsoft.AspNetCore.Identity.EntityFrameworkCore`-Contient les types requis pour utiliser Identity avec Entity Framework Core. </span><span class="sxs-lookup"><span data-stu-id="591fc-198">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` - Contains the required types to use Identity with Entity Framework Core.</span></span>

* <span data-ttu-id="591fc-199">`Microsoft.EntityFrameworkCore.SqlServer` -Entity Framework Core est la technologie d’accès aux données recommandée de Microsoft pour les bases de données relationnelles comme SQL Server.</span><span class="sxs-lookup"><span data-stu-id="591fc-199">`Microsoft.EntityFrameworkCore.SqlServer` - Entity Framework Core is Microsoft's recommended data access technology for relational databases like SQL Server.</span></span> <span data-ttu-id="591fc-200">Pour le test, vous pouvez utiliser `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="591fc-200">For testing, you can use `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

* <span data-ttu-id="591fc-201">`Microsoft.AspNetCore.Authentication.Cookies` -Intergiciel (middleware) qui permet à une application utiliser l’authentification basée sur les cookies.</span><span class="sxs-lookup"><span data-stu-id="591fc-201">`Microsoft.AspNetCore.Authentication.Cookies` - Middleware that enables an app to use cookie-based authentication.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="591fc-202">Migration vers ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="591fc-202">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="591fc-203">Pour plus d’informations et des conseils sur la migration de votre identité stockent voir [migrer l’authentification et identité](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="591fc-203">For additional information and guidance on migrating your existing Identity store see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="591fc-204">Définition du niveau de mot de passe</span><span class="sxs-lookup"><span data-stu-id="591fc-204">Setting password strength</span></span>

<span data-ttu-id="591fc-205">Consultez [Configuration](#pw) pour obtenir un exemple qui définit les exigences de mot de passe minimale.</span><span class="sxs-lookup"><span data-stu-id="591fc-205">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="591fc-206">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="591fc-206">Next Steps</span></span>

* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:security/authentication/social/index>
* <xref:host-and-deploy/web-farm>
