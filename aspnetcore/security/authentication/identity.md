---
title: Introduction à Identity sur ASP.NET Core
author: rick-anderson
description: Utiliser Identity à une application ASP.NET Core Découvrez comment définir les exigences de mot de passe (RequireDigit, RequiredLength, RequiredUniqueChars, etc.).
ms.author: riande
ms.date: 08/08/2018
uid: security/authentication/identity
ms.openlocfilehash: d427932bb175c09105534379be4d71760f4e04e5
ms.sourcegitcommit: 13940eb53c68664b11a2d685ee17c78faab1945d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/01/2018
ms.locfileid: "47860951"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="d3ead-104">Introduction à Identity sur ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d3ead-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="d3ead-105">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d3ead-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d3ead-106">ASP.NET Core Identity est un système d’appartenance qui ajoute des fonctionnalités de connexion pour les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d3ead-106">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="d3ead-107">Les utilisateurs peuvent créer un compte avec les informations de connexion stockées dans l’identité, ou ils peuvent utiliser un fournisseur de connexion externe.</span><span class="sxs-lookup"><span data-stu-id="d3ead-107">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="d3ead-108">Fournisseurs de connexion externe pris en charge incluent [Facebook, Google, Account Microsoft et Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="d3ead-108">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="d3ead-109">Identité peut être configurée à l’aide d’une base de données SQL Server pour stocker les noms d’utilisateur, les mots de passe et les données de profil.</span><span class="sxs-lookup"><span data-stu-id="d3ead-109">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="d3ead-110">Vous pouvez également un autre magasin persistant peut être utilisé, par exemple, stockage Table Azure.</span><span class="sxs-lookup"><span data-stu-id="d3ead-110">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

[<span data-ttu-id="d3ead-111">Afficher ou télécharger l’exemple de code.</span><span class="sxs-lookup"><span data-stu-id="d3ead-111">View or download the sample code.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [<span data-ttu-id="d3ead-112">(Comment télécharger)</span><span class="sxs-lookup"><span data-stu-id="d3ead-112">(How to download)</span></span>](xref:tutorials/index#how-to-download-a-sample)

<span data-ttu-id="d3ead-113">Dans cette rubrique, vous allez apprendre à utiliser l’identité pour vous inscrire, connectez-vous et déconnecter un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="d3ead-113">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="d3ead-114">Pour obtenir des instructions plus détaillées sur la création d’applications qui utilisent l’identité, consultez la section étapes suivantes à la fin de cet article.</span><span class="sxs-lookup"><span data-stu-id="d3ead-114">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

::: moniker range=">= aspnetcore-2.1"

<a name="adi"></a>
## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="d3ead-115">AddDefaultIdentity et AddIdentity</span><span class="sxs-lookup"><span data-stu-id="d3ead-115">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="d3ead-116">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) a été introduite dans ASP. Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="d3ead-116">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) was introduced in ASP.Core 2.1.</span></span> <span data-ttu-id="d3ead-117">Appel `AddDefaultIdentity` revient à appeler ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="d3ead-117">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* [<span data-ttu-id="d3ead-118">AddIdentity</span><span class="sxs-lookup"><span data-stu-id="d3ead-118">AddIdentity</span></span>](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_AddIdentity__2_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__)
* [<span data-ttu-id="d3ead-119">AddDefaultUI</span><span class="sxs-lookup"><span data-stu-id="d3ead-119">AddDefaultUI</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderuiextensions.adddefaultui?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderUIExtensions_AddDefaultUI_Microsoft_AspNetCore_Identity_IdentityBuilder_)
* [<span data-ttu-id="d3ead-120">AddDefaultTokenProviders</span><span class="sxs-lookup"><span data-stu-id="d3ead-120">AddDefaultTokenProviders</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderextensions.adddefaulttokenproviders?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderExtensions_AddDefaultTokenProviders_Microsoft_AspNetCore_Identity_IdentityBuilder_)

<span data-ttu-id="d3ead-121">Consultez [AddDefaultIdentity source](https://github.com/aspnet/Identity/blob/2634637fd535b229762b5e4a49cdd128f4d8f12e/src/UI/IdentityServiceCollectionUIExtensions.cs#L47-L64) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="d3ead-121">See [AddDefaultIdentity source](https://github.com/aspnet/Identity/blob/2634637fd535b229762b5e4a49cdd128f4d8f12e/src/UI/IdentityServiceCollectionUIExtensions.cs#L47-L64) for more information.</span></span>

::: moniker-end

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="d3ead-122">Créer une application Web avec l’authentification</span><span class="sxs-lookup"><span data-stu-id="d3ead-122">Create a Web app with authentication</span></span>

<span data-ttu-id="d3ead-123">Créer un projet d’Application Web ASP.NET Core avec des comptes d’utilisateur individuels.</span><span class="sxs-lookup"><span data-stu-id="d3ead-123">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d3ead-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d3ead-124">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d3ead-125">Sélectionnez **Fichier** > **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="d3ead-125">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="d3ead-126">Sélectionnez **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="d3ead-126">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="d3ead-127">Nommez le projet **application Web 1** pour avoir le même espace de noms en tant que le téléchargement du projet.</span><span class="sxs-lookup"><span data-stu-id="d3ead-127">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="d3ead-128">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="d3ead-128">Click **OK**.</span></span>
* <span data-ttu-id="d3ead-129">Sélectionnez une ASP.NET Core **Web Application** pour ASP.NET Core 2.1, puis sélectionnez **modifier l’authentification**.</span><span class="sxs-lookup"><span data-stu-id="d3ead-129">Select an ASP.NET Core **Web Application** for ASP.NET Core 2.1, then select **Change Authentication**.</span></span>
* <span data-ttu-id="d3ead-130">Sélectionnez **comptes d’utilisateur individuels** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="d3ead-130">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d3ead-131">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="d3ead-131">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="d3ead-132">Fournit le projet généré [ASP.NET Core Identity](xref:security/authentication/identity) comme un [bibliothèque de classes Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="d3ead-132">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span>

### <a name="test-register-and-login"></a><span data-ttu-id="d3ead-133">Registre de test et de connexion</span><span class="sxs-lookup"><span data-stu-id="d3ead-133">Test Register and Login</span></span>

<span data-ttu-id="d3ead-134">Exécutez l’application et inscrire un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="d3ead-134">Run the app and register a user.</span></span> <span data-ttu-id="d3ead-135">Selon la taille de votre écran, vous devrez peut-être sélectionner le bouton de navigation pour afficher le **inscrire** et **connexion** des liens.</span><span class="sxs-lookup"><span data-stu-id="d3ead-135">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

![bouton de barre de navigation de bascule](identity/_static/navToggle.png)

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>
### <a name="configure-identity-services"></a><span data-ttu-id="d3ead-137">Configurer les services d’identité</span><span class="sxs-lookup"><span data-stu-id="d3ead-137">Configure Identity services</span></span>

<span data-ttu-id="d3ead-138">Les services sont ajoutés dans `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d3ead-138">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="d3ead-139">Le modèle par défaut consiste à appeler toutes les méthodes `Add{Service}`, puis toutes les méthodes `services.Configure{Service}`.</span><span class="sxs-lookup"><span data-stu-id="d3ead-139">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span> <span data-ttu-id="d3ead-140">Le code suivant n’inclut pas le modèle généré `CookiePolicyOptions`:</span><span class="sxs-lookup"><span data-stu-id="d3ead-140">The following code doesn't include the template generated `CookiePolicyOptions`:</span></span>

::: moniker range=">= aspnetcore-2.1"

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="d3ead-141">Le code précédent configure l’identité avec des valeurs d’option par défaut.</span><span class="sxs-lookup"><span data-stu-id="d3ead-141">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="d3ead-142">Services sont accessibles à l’application via [l’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="d3ead-142">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="d3ead-143">Identity est activé en appelant [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="d3ead-143">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="d3ead-144">`UseAuthentication`Ajoute l’authentification [intergiciel (middleware)](xref:fundamentals/middleware/index) au pipeline de demande.</span><span class="sxs-lookup"><span data-stu-id="d3ead-144">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   <span data-ttu-id="d3ead-145">Services sont accessibles à l’application via [l’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="d3ead-145">Services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="d3ead-146">Identity est activée pour l’application en appelant `UseAuthentication` dans le `Configure` (méthode).</span><span class="sxs-lookup"><span data-stu-id="d3ead-146">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="d3ead-147">`UseAuthentication`Ajoute l’authentification [intergiciel (middleware)](xref:fundamentals/middleware/index) au pipeline de demande.</span><span class="sxs-lookup"><span data-stu-id="d3ead-147">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   <span data-ttu-id="d3ead-148">Ces services sont accessibles à l’application via [l’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="d3ead-148">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="d3ead-149">Identity est activée pour l’application en appelant `UseIdentity` dans le `Configure` (méthode).</span><span class="sxs-lookup"><span data-stu-id="d3ead-149">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="d3ead-150">`UseIdentity`Ajoute l’authentification par cookie [intergiciel (middleware)](xref:fundamentals/middleware/index) au pipeline de demande.</span><span class="sxs-lookup"><span data-stu-id="d3ead-150">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

<span data-ttu-id="d3ead-151">Pour plus d’informations, consultez le [IdentityOptions classe](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) et [démarrage de l’Application](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="d3ead-151">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="d3ead-152">Inscription d’une structure, de connexion et de déconnexion</span><span class="sxs-lookup"><span data-stu-id="d3ead-152">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="d3ead-153">Suivez le [structurer d’identité dans un projet Razor avec l’autorisation](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) obtenir des instructions.</span><span class="sxs-lookup"><span data-stu-id="d3ead-153">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d3ead-154">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d3ead-154">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d3ead-155">Ajoutez les fichiers de Registre, de connexion et de déconnexion.</span><span class="sxs-lookup"><span data-stu-id="d3ead-155">Add the Register, Login, and LogOut files.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d3ead-156">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="d3ead-156">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="d3ead-157">Si vous avez créé le projet avec le nom **application Web 1**, exécutez les commandes suivantes.</span><span class="sxs-lookup"><span data-stu-id="d3ead-157">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="d3ead-158">Sinon, utilisez l’espace de noms correct pour le `ApplicationDbContext`:</span><span class="sxs-lookup"><span data-stu-id="d3ead-158">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```cli
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="d3ead-159">PowerShell utilise le point-virgule comme séparateur de commande.</span><span class="sxs-lookup"><span data-stu-id="d3ead-159">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="d3ead-160">Lorsque vous utilisez PowerShell, les points-virgules dans la liste des fichiers de séquence d’échappement ou placez la liste des fichiers dans des guillemets doubles, comme le montre l’exemple précédent.</span><span class="sxs-lookup"><span data-stu-id="d3ead-160">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="d3ead-161">Examinez le Registre</span><span class="sxs-lookup"><span data-stu-id="d3ead-161">Examine Register</span></span>

::: moniker range=">= aspnetcore-2.1"

   <span data-ttu-id="d3ead-162">Lorsqu’un utilisateur clique sur le **inscrire** lien, le `RegisterModel.OnPostAsync` action est appelée.</span><span class="sxs-lookup"><span data-stu-id="d3ead-162">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="d3ead-163">L’utilisateur est créé par [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) sur la `_userManager` objet.</span><span class="sxs-lookup"><span data-stu-id="d3ead-163">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="d3ead-164">`_userManager` est fourni par l’injection de dépendances) :</span><span class="sxs-lookup"><span data-stu-id="d3ead-164">`_userManager` is provided by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="d3ead-165">Lorsqu’un utilisateur clique sur le **inscrire** lien, le `Register` action est appelée sur `AccountController`.</span><span class="sxs-lookup"><span data-stu-id="d3ead-165">When a user clicks the **Register** link, the `Register` action is invoked on `AccountController`.</span></span> <span data-ttu-id="d3ead-166">Le `Register` action crée l’utilisateur en appelant `CreateAsync` sur le `_userManager` objet (fourni à `AccountController` par injection de dépendances) :</span><span class="sxs-lookup"><span data-stu-id="d3ead-166">The `Register` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to `AccountController` by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   <span data-ttu-id="d3ead-167">Si l’utilisateur a été créé avec succès, l’utilisateur est connecté par l’appel à `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="d3ead-167">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

   <span data-ttu-id="d3ead-168">**Remarque :** consultez [confirmation de compte](xref:security/authentication/accconfirm#prevent-login-at-registration) pour savoir comment empêcher la connexion immédiate lors de l’inscription.</span><span class="sxs-lookup"><span data-stu-id="d3ead-168">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="d3ead-169">Connectez-vous</span><span class="sxs-lookup"><span data-stu-id="d3ead-169">Log in</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="d3ead-170">Le formulaire de connexion s’affiche lorsque :</span><span class="sxs-lookup"><span data-stu-id="d3ead-170">The Login form is displayed when:</span></span>

* <span data-ttu-id="d3ead-171">Le **connectez-vous** lien est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="d3ead-171">The **Log in** link  is selected.</span></span>
* <span data-ttu-id="d3ead-172">Lorsqu’un utilisateur accède à une page où ils ne sont pas authentifiés **ou** autorisés, ils sont redirigés vers la page de connexion.</span><span class="sxs-lookup"><span data-stu-id="d3ead-172">When a user accesses a page where they are not authenticated **or** authorized, they are redirected to the Login page.</span></span>

<span data-ttu-id="d3ead-173">Lors de l’envoi du formulaire sur la page de connexion, le `OnPostAsync` action est appelée.</span><span class="sxs-lookup"><span data-stu-id="d3ead-173">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="d3ead-174">`PasswordSignInAsync` est appelée sur le `_signInManager` objet (fourni par l’injection de dépendances).</span><span class="sxs-lookup"><span data-stu-id="d3ead-174">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Login.cshtml.cs?name=snippet&highlight=10-11)]

   <span data-ttu-id="d3ead-175">La base de `Controller` classe expose un `User` propriété auxquelles vous pouvez accéder à partir de méthodes de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="d3ead-175">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="d3ead-176">Par exemple, vous pouvez énumérer `User.Claims` et prendre des décisions d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="d3ead-176">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="d3ead-177">Pour plus d’informations, consultez [autorisation](xref:security/authorization/index).</span><span class="sxs-lookup"><span data-stu-id="d3ead-177">For more information, see [Authorization](xref:security/authorization/index).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="d3ead-178">Le formulaire de connexion s’affiche lorsque les utilisateurs sélectionnent le **connectez-vous** lier ou sont redirigés lorsqu’ils accèdent à une page qui requiert une authentification.</span><span class="sxs-lookup"><span data-stu-id="d3ead-178">The Login form is displayed when users select the **Log in** link or are redirected when accessing a page that requires authentication.</span></span> <span data-ttu-id="d3ead-179">Lorsque l’utilisateur soumet le formulaire sur la page de connexion, le `AccountController` `Login` action est appelée.</span><span class="sxs-lookup"><span data-stu-id="d3ead-179">When the user submits the form on the Login page, the `AccountController` `Login` action is called.</span></span>

<span data-ttu-id="d3ead-180">Le `Login` action appels `PasswordSignInAsync` sur le `_signInManager` objet (fourni à `AccountController` par injection de dépendances).</span><span class="sxs-lookup"><span data-stu-id="d3ead-180">The `Login` action calls `PasswordSignInAsync` on the `_signInManager` object (provided to `AccountController` by dependency injection).</span></span>

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

<span data-ttu-id="d3ead-181">La base (`Controller` ou `PageModel`) classe expose un `User` propriété.</span><span class="sxs-lookup"><span data-stu-id="d3ead-181">The base (`Controller` or `PageModel`) class exposes a `User` property.</span></span> <span data-ttu-id="d3ead-182">Par exemple, `User.Claims` peuvent être énumérés afin de prendre des décisions d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="d3ead-182">For example, `User.Claims` can be enumerated to make authorization decisions.</span></span>

::: moniker-end

### <a name="log-out"></a><span data-ttu-id="d3ead-183">Se déconnecter</span><span class="sxs-lookup"><span data-stu-id="d3ead-183">Log out</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="d3ead-184">Le **déconnecter** lien appelle le `LogoutModel.OnPost` action.</span><span class="sxs-lookup"><span data-stu-id="d3ead-184">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Logout.cshtml.cs)]

<span data-ttu-id="d3ead-185">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) efface les revendications de l’utilisateur stockées dans un cookie.</span><span class="sxs-lookup"><span data-stu-id="d3ead-185">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span> <span data-ttu-id="d3ead-186">Ne pas rediriger après avoir appelé `SignOutAsync` ou l’utilisateur sera **pas** me déconnecter.</span><span class="sxs-lookup"><span data-stu-id="d3ead-186">Don't redirect after calling `SignOutAsync` or the user will **not** be signed out.</span></span>

<span data-ttu-id="d3ead-187">POST est spécifié dans le *Pages/Shared/_LoginPartial.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d3ead-187">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/_LoginPartial.cshtml?highlight=10)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="d3ead-188">En cliquant sur le **déconnecter** lien appelle le `LogOut` action.</span><span class="sxs-lookup"><span data-stu-id="d3ead-188">Clicking the **Log out** link calls the `LogOut` action.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   <span data-ttu-id="d3ead-189">Le code précédent appelle la `_signInManager.SignOutAsync` (méthode).</span><span class="sxs-lookup"><span data-stu-id="d3ead-189">The preceding code calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="d3ead-190">Le `SignOutAsync` méthode efface les revendications de l’utilisateur stockées dans un cookie.</span><span class="sxs-lookup"><span data-stu-id="d3ead-190">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>

::: moniker-end

## <a name="test-identity"></a><span data-ttu-id="d3ead-191">Tester l’identité</span><span class="sxs-lookup"><span data-stu-id="d3ead-191">Test Identity</span></span>

<span data-ttu-id="d3ead-192">Les modèles de projet web par défaut autoriser l’accès anonyme pour les pages d’accueil.</span><span class="sxs-lookup"><span data-stu-id="d3ead-192">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="d3ead-193">Pour tester l’identité, vous devez ajouter [ `[Authorize]` ](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) à la page About.</span><span class="sxs-lookup"><span data-stu-id="d3ead-193">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the About page.</span></span>

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/About.cshtml.cs)]

<span data-ttu-id="d3ead-194">Si vous êtes connecté, déconnectez-vous. Exécutez l’application et sélectionnez le **sur** lien.</span><span class="sxs-lookup"><span data-stu-id="d3ead-194">If you are signed in, sign out. Run the app and select the **About** link.</span></span> <span data-ttu-id="d3ead-195">Vous êtes redirigé vers la page de connexion.</span><span class="sxs-lookup"><span data-stu-id="d3ead-195">You are redirected to the login page.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a><span data-ttu-id="d3ead-196">Explorez l’identité</span><span class="sxs-lookup"><span data-stu-id="d3ead-196">Explore Identity</span></span>

<span data-ttu-id="d3ead-197">Pour Explorer d’identité plus en détail :</span><span class="sxs-lookup"><span data-stu-id="d3ead-197">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="d3ead-198">Créer la source de l’interface utilisateur d’identité complète</span><span class="sxs-lookup"><span data-stu-id="d3ead-198">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="d3ead-199">Examinez la source de chaque page, puis parcourez le débogueur.</span><span class="sxs-lookup"><span data-stu-id="d3ead-199">Examine the source of each page and step through the debugger.</span></span>

::: moniker-end

## <a name="identity-components"></a><span data-ttu-id="d3ead-200">Composants d'Identity</span><span class="sxs-lookup"><span data-stu-id="d3ead-200">Identity Components</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="d3ead-201">Tous les identités NuGet packages dépendants sont inclus dans le [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d3ead-201">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="d3ead-202">Le package principal pour l’identité est [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="d3ead-202">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="d3ead-203">Ce package contient l’ensemble principal d’interfaces pour ASP.NET Core Identity et est inclus par `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="d3ead-203">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="d3ead-204">Migration vers ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="d3ead-204">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="d3ead-205">Pour plus d’informations et des conseils sur la migration de votre magasin d’identités existant, consultez [migrer l’authentification et identité](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="d3ead-205">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="d3ead-206">Définition du niveau de mot de passe</span><span class="sxs-lookup"><span data-stu-id="d3ead-206">Setting password strength</span></span>

<span data-ttu-id="d3ead-207">Consultez [Configuration](#pw) pour obtenir un exemple qui définit les exigences de mot de passe minimale.</span><span class="sxs-lookup"><span data-stu-id="d3ead-207">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d3ead-208">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="d3ead-208">Next Steps</span></span>

* [<span data-ttu-id="d3ead-209">Configurer Identity</span><span class="sxs-lookup"><span data-stu-id="d3ead-209">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
