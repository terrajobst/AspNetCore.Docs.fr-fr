---
title: Introduction à Identity sur ASP.NET Core
author: rick-anderson
description: Utiliser Identity à une application ASP.NET Core Découvrez comment définir les exigences de mot de passe (RequireDigit, RequiredLength, RequiredUniqueChars, etc.).
ms.author: riande
ms.date: 08/08/2018
uid: security/authentication/identity
ms.openlocfilehash: 96f446ad9ec1ef5d807a8648e68308ee20583365
ms.sourcegitcommit: 08bf41d4b3e696ab512b044970e8304816f8cc56
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/06/2018
ms.locfileid: "44040026"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="8adb4-104">Introduction à Identity sur ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8adb4-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="8adb4-105">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8adb4-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8adb4-106">ASP.NET Core Identity est un système d’appartenance qui ajoute des fonctionnalités de connexion pour les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8adb4-106">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="8adb4-107">Les utilisateurs peuvent créer un compte avec les informations de connexion stockées dans l’identité, ou ils peuvent utiliser un fournisseur de connexion externe.</span><span class="sxs-lookup"><span data-stu-id="8adb4-107">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="8adb4-108">Fournisseurs de connexion externe pris en charge incluent [Facebook, Google, Account Microsoft et Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="8adb4-108">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="8adb4-109">Identité peut être configurée à l’aide d’une base de données SQL Server pour stocker les noms d’utilisateur, les mots de passe et les données de profil.</span><span class="sxs-lookup"><span data-stu-id="8adb4-109">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="8adb4-110">Vous pouvez également un autre magasin persistant peut être utilisé, par exemple, stockage Table Azure.</span><span class="sxs-lookup"><span data-stu-id="8adb4-110">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

[<span data-ttu-id="8adb4-111">Afficher ou télécharger l’exemple de code.</span><span class="sxs-lookup"><span data-stu-id="8adb4-111">View or download the sample code.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [<span data-ttu-id="8adb4-112">(Comment télécharger)</span><span class="sxs-lookup"><span data-stu-id="8adb4-112">(How to download)</span></span>](xref:tutorials/index#how-to-download-a-sample)

<span data-ttu-id="8adb4-113">Dans cette rubrique, vous allez apprendre à utiliser l’identité pour vous inscrire, connectez-vous et déconnecter un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="8adb4-113">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="8adb4-114">Pour obtenir des instructions plus détaillées sur la création d’applications qui utilisent l’identité, consultez la section étapes suivantes à la fin de cet article.</span><span class="sxs-lookup"><span data-stu-id="8adb4-114">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="8adb4-115">Créer une application Web avec l’authentification</span><span class="sxs-lookup"><span data-stu-id="8adb4-115">Create a Web app with authentication</span></span>

<span data-ttu-id="8adb4-116">Créer un projet d’Application Web ASP.NET Core avec des comptes d’utilisateur individuels.</span><span class="sxs-lookup"><span data-stu-id="8adb4-116">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8adb4-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8adb4-117">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8adb4-118">Sélectionnez **Fichier** > **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="8adb4-118">Select **File** > **New** > **Project**.</span></span> 
* <span data-ttu-id="8adb4-119">Sélectionnez **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="8adb4-119">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="8adb4-120">Nommez le projet **application Web 1** pour avoir le même espace de noms en tant que le téléchargement du projet.</span><span class="sxs-lookup"><span data-stu-id="8adb4-120">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="8adb4-121">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="8adb4-121">Click **OK**.</span></span>
* <span data-ttu-id="8adb4-122">Sélectionnez une ASP.NET Core **Web Application** pour ASP.NET Core 2.1, puis sélectionnez **modifier l’authentification**.</span><span class="sxs-lookup"><span data-stu-id="8adb4-122">Select an ASP.NET Core **Web Application** for ASP.NET Core 2.1, then select **Change Authentication**.</span></span>
* <span data-ttu-id="8adb4-123">Sélectionnez **comptes d’utilisateur individuels** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="8adb4-123">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8adb4-124">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="8adb4-124">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="8adb4-125">Fournit le projet généré [ASP.NET Core Identity](xref:security/authentication/identity) comme un [bibliothèque de classes Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="8adb4-125">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span>

### <a name="test-register-and-login"></a><span data-ttu-id="8adb4-126">Registre de test et de connexion</span><span class="sxs-lookup"><span data-stu-id="8adb4-126">Test Register and Login</span></span>

<span data-ttu-id="8adb4-127">Exécutez l’application et inscrire un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="8adb4-127">Run the app and register a user.</span></span> <span data-ttu-id="8adb4-128">Selon la taille de votre écran, vous devrez peut-être sélectionner le bouton de navigation pour afficher le **inscrire** et **connexion** des liens.</span><span class="sxs-lookup"><span data-stu-id="8adb4-128">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

![bouton de barre de navigation de bascule](identity/_static/navToggle.png)

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>
### <a name="configure-identity-services"></a><span data-ttu-id="8adb4-130">Configurer les services d’identité</span><span class="sxs-lookup"><span data-stu-id="8adb4-130">Configure Identity services</span></span>

<span data-ttu-id="8adb4-131">Les services sont ajoutés dans `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8adb4-131">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="8adb4-132">Le modèle par défaut consiste à appeler tous les le `Add{Service}` méthodes, puis appelez toutes les le `services.Configure{Service}` méthodes.</span><span class="sxs-lookup"><span data-stu-id="8adb4-132">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span> <span data-ttu-id="8adb4-133">Le code suivant n’inclut pas le modèle généré `CookiePolicyOptions`:</span><span class="sxs-lookup"><span data-stu-id="8adb4-133">The following code doesn't include the template generated `CookiePolicyOptions`:</span></span>

::: moniker range=">= aspnetcore-2.1"

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="8adb4-134">Le code précédent configure l’identité avec des valeurs d’option par défaut.</span><span class="sxs-lookup"><span data-stu-id="8adb4-134">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="8adb4-135">Services sont accessibles à l’application via [l’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="8adb4-135">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="8adb4-136">Identity est activé en appelant [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="8adb4-136">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="8adb4-137">`UseAuthentication`Ajoute l’authentification [intergiciel (middleware)](xref:fundamentals/middleware/index) au pipeline de demande.</span><span class="sxs-lookup"><span data-stu-id="8adb4-137">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   <span data-ttu-id="8adb4-138">Services sont accessibles à l’application via [l’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="8adb4-138">Services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="8adb4-139">Identity est activée pour l’application en appelant `UseAuthentication` dans le `Configure` (méthode).</span><span class="sxs-lookup"><span data-stu-id="8adb4-139">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="8adb4-140">`UseAuthentication`Ajoute l’authentification [intergiciel (middleware)](xref:fundamentals/middleware/index) au pipeline de demande.</span><span class="sxs-lookup"><span data-stu-id="8adb4-140">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   <span data-ttu-id="8adb4-141">Ces services sont accessibles à l’application via [l’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="8adb4-141">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="8adb4-142">Identity est activée pour l’application en appelant `UseIdentity` dans le `Configure` (méthode).</span><span class="sxs-lookup"><span data-stu-id="8adb4-142">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="8adb4-143">`UseIdentity`Ajoute l’authentification par cookie [intergiciel (middleware)](xref:fundamentals/middleware/index) au pipeline de demande.</span><span class="sxs-lookup"><span data-stu-id="8adb4-143">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

<span data-ttu-id="8adb4-144">Pour plus d’informations, consultez le [IdentityOptions classe](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) et [démarrage de l’Application](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="8adb4-144">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="8adb4-145">Inscription d’une structure, de connexion et de déconnexion</span><span class="sxs-lookup"><span data-stu-id="8adb4-145">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="8adb4-146">Suivez le [structurer d’identité dans un projet Razor avec l’autorisation](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) obtenir des instructions.</span><span class="sxs-lookup"><span data-stu-id="8adb4-146">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8adb4-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8adb4-147">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8adb4-148">Ajoutez les fichiers de Registre, de connexion et de déconnexion.</span><span class="sxs-lookup"><span data-stu-id="8adb4-148">Add the Register, Login, and LogOut files.</span></span>


# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8adb4-149">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="8adb4-149">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="8adb4-150">Si vous avez créé le projet avec le nom **application Web 1**, exécutez les commandes suivantes.</span><span class="sxs-lookup"><span data-stu-id="8adb4-150">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="8adb4-151">Sinon, utilisez l’espace de noms correct pour le `ApplicationDbContext`:</span><span class="sxs-lookup"><span data-stu-id="8adb4-151">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>


```cli
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"

```

<span data-ttu-id="8adb4-152">PowerShell utilise le point-virgule comme séparateur de commande.</span><span class="sxs-lookup"><span data-stu-id="8adb4-152">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="8adb4-153">Lorsque vous utilisez PowerShell, les points-virgules dans la liste des fichiers de séquence d’échappement ou placez la liste des fichiers dans des guillemets doubles, comme le montre l’exemple précédent.</span><span class="sxs-lookup"><span data-stu-id="8adb4-153">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="8adb4-154">Examinez le Registre</span><span class="sxs-lookup"><span data-stu-id="8adb4-154">Examine Register</span></span>

::: moniker range=">= aspnetcore-2.1"

   <span data-ttu-id="8adb4-155">Lorsqu’un utilisateur clique sur le **inscrire** lien, le `RegisterModel.OnPostAsync` action est appelée.</span><span class="sxs-lookup"><span data-stu-id="8adb4-155">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="8adb4-156">L’utilisateur est créé par [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) sur la `_userManager` objet.</span><span class="sxs-lookup"><span data-stu-id="8adb4-156">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="8adb4-157">`_userManager` est fourni par l’injection de dépendances) :</span><span class="sxs-lookup"><span data-stu-id="8adb4-157">`_userManager` is provided by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end
::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="8adb4-158">Lorsqu’un utilisateur clique sur le **inscrire** lien, le `Register` action est appelée sur `AccountController`.</span><span class="sxs-lookup"><span data-stu-id="8adb4-158">When a user clicks the **Register** link, the `Register` action is invoked on `AccountController`.</span></span> <span data-ttu-id="8adb4-159">Le `Register` action crée l’utilisateur en appelant `CreateAsync` sur le `_userManager` objet (fourni à `AccountController` par injection de dépendances) :</span><span class="sxs-lookup"><span data-stu-id="8adb4-159">The `Register` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to `AccountController` by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   <span data-ttu-id="8adb4-160">Si l’utilisateur a été créé avec succès, l’utilisateur est connecté par l’appel à `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="8adb4-160">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

   <span data-ttu-id="8adb4-161">**Remarque :** consultez [confirmation de compte](xref:security/authentication/accconfirm#prevent-login-at-registration) pour savoir comment empêcher la connexion immédiate lors de l’inscription.</span><span class="sxs-lookup"><span data-stu-id="8adb4-161">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="8adb4-162">Connectez-vous</span><span class="sxs-lookup"><span data-stu-id="8adb4-162">Log in</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8adb4-163">Le formulaire de connexion s’affiche lorsque :</span><span class="sxs-lookup"><span data-stu-id="8adb4-163">The Login form is displayed when:</span></span>

* <span data-ttu-id="8adb4-164">Le **connectez-vous** lien est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="8adb4-164">The **Log in** link  is selected.</span></span>
* <span data-ttu-id="8adb4-165">Lorsqu’un utilisateur accède à une page où ils ne sont pas authentifiés **ou** autorisés, ils sont redirigés vers la page de connexion.</span><span class="sxs-lookup"><span data-stu-id="8adb4-165">When a user accesses a page where they are not authenticated **or** authorized, they are redirected to the Login page.</span></span> 

<span data-ttu-id="8adb4-166">Lors de l’envoi du formulaire sur la page de connexion, le `OnPostAsync` action est appelée.</span><span class="sxs-lookup"><span data-stu-id="8adb4-166">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="8adb4-167">`PasswordSignInAsync` est appelée sur le `_signInManager` objet (fourni par l’injection de dépendances).</span><span class="sxs-lookup"><span data-stu-id="8adb4-167">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Login.cshtml.cs?name=snippet&highlight=10-11)]

   <span data-ttu-id="8adb4-168">La base de `Controller` classe expose un `User` propriété auxquelles vous pouvez accéder à partir de méthodes de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="8adb4-168">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="8adb4-169">Par exemple, vous pouvez énumérer `User.Claims` et prendre des décisions d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="8adb4-169">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="8adb4-170">Pour plus d’informations, consultez [autorisation](xref:security/authorization/index).</span><span class="sxs-lookup"><span data-stu-id="8adb4-170">For more information, see [Authorization](xref:security/authorization/index).</span></span>

::: moniker-end
::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="8adb4-171">Le formulaire de connexion s’affiche lorsque les utilisateurs sélectionnent le **connectez-vous** lier ou sont redirigés lorsqu’ils accèdent à une page qui requiert une authentification.</span><span class="sxs-lookup"><span data-stu-id="8adb4-171">The Login form is displayed when users select the **Log in** link or are redirected when accessing a page that requires authentication.</span></span> <span data-ttu-id="8adb4-172">Lorsque l’utilisateur soumet le formulaire sur la page de connexion, le `AccountController` `Login` action est appelée.</span><span class="sxs-lookup"><span data-stu-id="8adb4-172">When the user submits the form on the Login page, the `AccountController` `Login` action is called.</span></span>

<span data-ttu-id="8adb4-173">Le `Login` action appels `PasswordSignInAsync` sur le `_signInManager` objet (fourni à `AccountController` par injection de dépendances).</span><span class="sxs-lookup"><span data-stu-id="8adb4-173">The `Login` action calls `PasswordSignInAsync` on the `_signInManager` object (provided to `AccountController` by dependency injection).</span></span>

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

<span data-ttu-id="8adb4-174">La base (`Controller` ou `PageModel`) classe expose un `User` propriété.</span><span class="sxs-lookup"><span data-stu-id="8adb4-174">The base (`Controller` or `PageModel`) class exposes a `User` property.</span></span> <span data-ttu-id="8adb4-175">Par exemple, `User.Claims` peuvent être énumérés afin de prendre des décisions d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="8adb4-175">For example, `User.Claims` can be enumerated to make authorization decisions.</span></span>

::: moniker-end

### <a name="log-out"></a><span data-ttu-id="8adb4-176">Se déconnecter</span><span class="sxs-lookup"><span data-stu-id="8adb4-176">Log out</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8adb4-177">Le **déconnecter** lien appelle le `LogoutModel.OnPost` action.</span><span class="sxs-lookup"><span data-stu-id="8adb4-177">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Logout.cshtml.cs)]

<span data-ttu-id="8adb4-178">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) efface les revendications de l’utilisateur stockées dans un cookie.</span><span class="sxs-lookup"><span data-stu-id="8adb4-178">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span> <span data-ttu-id="8adb4-179">Ne pas rediriger après avoir appelé `SignOutAsync` ou l’utilisateur sera **pas** me déconnecter.</span><span class="sxs-lookup"><span data-stu-id="8adb4-179">Don't redirect after calling `SignOutAsync` or the user will **not** be signed out.</span></span>

<span data-ttu-id="8adb4-180">POST est spécifié dans le *Pages/Shared/_LoginPartial.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8adb4-180">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/_LoginPartial.cshtml?highlight=10)]

::: moniker-end
::: moniker range="= aspnetcore-2.0"
   <span data-ttu-id="8adb4-181">En cliquant sur le **déconnecter** lien appelle le `LogOut` action.</span><span class="sxs-lookup"><span data-stu-id="8adb4-181">Clicking the **Log out** link calls the `LogOut` action.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   <span data-ttu-id="8adb4-182">Le code précédent appelle la `_signInManager.SignOutAsync` (méthode).</span><span class="sxs-lookup"><span data-stu-id="8adb4-182">The preceding code calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="8adb4-183">Le `SignOutAsync` méthode efface les revendications de l’utilisateur stockées dans un cookie.</span><span class="sxs-lookup"><span data-stu-id="8adb4-183">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>
::: moniker-end

## <a name="test-identity"></a><span data-ttu-id="8adb4-184">Tester l’identité</span><span class="sxs-lookup"><span data-stu-id="8adb4-184">Test Identity</span></span>

<span data-ttu-id="8adb4-185">Les modèles de projet web par défaut autoriser l’accès anonyme pour les pages d’accueil.</span><span class="sxs-lookup"><span data-stu-id="8adb4-185">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="8adb4-186">Pour tester l’identité, vous devez ajouter [ `[Authorize]` ](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) à la page About.</span><span class="sxs-lookup"><span data-stu-id="8adb4-186">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the About page.</span></span>

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/About.cshtml.cs)]

<span data-ttu-id="8adb4-187">Si vous êtes connecté, déconnectez-vous. Exécutez l’application et sélectionnez le **sur** lien.</span><span class="sxs-lookup"><span data-stu-id="8adb4-187">If you are signed in, sign out. Run the app and select the **About** link.</span></span> <span data-ttu-id="8adb4-188">Vous êtes redirigé vers la page de connexion.</span><span class="sxs-lookup"><span data-stu-id="8adb4-188">You are redirected to the login page.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a><span data-ttu-id="8adb4-189">Explorez l’identité</span><span class="sxs-lookup"><span data-stu-id="8adb4-189">Explore Identity</span></span>

<span data-ttu-id="8adb4-190">Pour Explorer d’identité plus en détail :</span><span class="sxs-lookup"><span data-stu-id="8adb4-190">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="8adb4-191">Créer la source de l’interface utilisateur d’identité complète</span><span class="sxs-lookup"><span data-stu-id="8adb4-191">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="8adb4-192">Examinez la source de chaque page, puis parcourez le débogueur.</span><span class="sxs-lookup"><span data-stu-id="8adb4-192">Examine the source of each page and step through the debugger.</span></span>

::: moniker-end

## <a name="identity-components"></a><span data-ttu-id="8adb4-193">Composants d'Identity</span><span class="sxs-lookup"><span data-stu-id="8adb4-193">Identity Components</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8adb4-194">Tous les identités NuGet packages dépendants sont inclus dans le [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="8adb4-194">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
::: moniker-end

<span data-ttu-id="8adb4-195">Le package principal pour l’identité est [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="8adb4-195">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="8adb4-196">Ce package contient l’ensemble principal d’interfaces pour ASP.NET Core Identity et est inclus par `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="8adb4-196">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="8adb4-197">Migration vers ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="8adb4-197">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="8adb4-198">Pour plus d’informations et des conseils sur la migration de votre magasin d’identités existant, consultez [migrer l’authentification et identité](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="8adb4-198">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="8adb4-199">Définition du niveau de mot de passe</span><span class="sxs-lookup"><span data-stu-id="8adb4-199">Setting password strength</span></span>

<span data-ttu-id="8adb4-200">Consultez [Configuration](#pw) pour obtenir un exemple qui définit les exigences de mot de passe minimale.</span><span class="sxs-lookup"><span data-stu-id="8adb4-200">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8adb4-201">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="8adb4-201">Next Steps</span></span>

* [<span data-ttu-id="8adb4-202">Configurer Identity</span><span class="sxs-lookup"><span data-stu-id="8adb4-202">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <span data-ttu-id="8adb4-203">[Configurer le type de données de clés primaires identité](xref:security/authentication/identity-primary-key-configuration).</span><span class="sxs-lookup"><span data-stu-id="8adb4-203">[Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
