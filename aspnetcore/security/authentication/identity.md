---
title: Introduction à Identity sur ASP.NET Core
author: rick-anderson
description: Utiliser Identity à une application ASP.NET Core Découvrez comment définir les exigences de mot de passe (RequireDigit, RequiredLength, RequiredUniqueChars, etc.).
ms.author: riande
ms.date: 03/26/2019
uid: security/authentication/identity
ms.openlocfilehash: 979681cfc196aca9fb5097583d99a086e1c597ba
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082451"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="665c9-104">Introduction à Identity sur ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="665c9-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="665c9-105">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="665c9-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="665c9-106">ASP.NET Core identité est un système d’appartenance qui ajoute des fonctionnalités de connexion aux applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="665c9-106">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="665c9-107">Les utilisateurs peuvent créer un compte avec les informations de connexion stockées dans l’identité, ou ils peuvent utiliser un fournisseur de connexion externe.</span><span class="sxs-lookup"><span data-stu-id="665c9-107">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="665c9-108">Les fournisseurs de connexion externes pris en charge incluent [Facebook, Google, Microsoft Account et Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="665c9-108">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="665c9-109">L’identité peut être configurée à l’aide d’une base de données SQL Server pour stocker les noms d’utilisateur, les mots de passe et les données de profil.</span><span class="sxs-lookup"><span data-stu-id="665c9-109">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="665c9-110">Vous pouvez également utiliser un autre magasin persistant, par exemple, le stockage table Azure.</span><span class="sxs-lookup"><span data-stu-id="665c9-110">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="665c9-111">[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([procédure de téléchargement)](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="665c9-111">[View or download the sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="665c9-112">Dans cette rubrique, vous allez apprendre à utiliser l’identité pour vous inscrire, vous connecter et déconnecter un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="665c9-112">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="665c9-113">Pour obtenir des instructions plus détaillées sur la création d’applications qui utilisent l’identité, consultez la section étapes suivantes à la fin de cet article.</span><span class="sxs-lookup"><span data-stu-id="665c9-113">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

::: moniker range=">= aspnetcore-2.1"

<a name="adi"></a>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="665c9-114">AddDefaultIdentity et AddIdentity</span><span class="sxs-lookup"><span data-stu-id="665c9-114">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="665c9-115">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) a été introduite dans ASP.net Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="665c9-115">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="665c9-116">L' `AddDefaultIdentity` appel de est similaire à l’appel de ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="665c9-116">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* [<span data-ttu-id="665c9-117">AddIdentity</span><span class="sxs-lookup"><span data-stu-id="665c9-117">AddIdentity</span></span>](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_AddIdentity__2_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__)
* [<span data-ttu-id="665c9-118">AddDefaultUI</span><span class="sxs-lookup"><span data-stu-id="665c9-118">AddDefaultUI</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderuiextensions.adddefaultui?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderUIExtensions_AddDefaultUI_Microsoft_AspNetCore_Identity_IdentityBuilder_)
* [<span data-ttu-id="665c9-119">AddDefaultTokenProviders</span><span class="sxs-lookup"><span data-stu-id="665c9-119">AddDefaultTokenProviders</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderextensions.adddefaulttokenproviders?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderExtensions_AddDefaultTokenProviders_Microsoft_AspNetCore_Identity_IdentityBuilder_)

<span data-ttu-id="665c9-120">Pour plus d’informations, consultez [source AddDefaultIdentity](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) .</span><span class="sxs-lookup"><span data-stu-id="665c9-120">See [AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

::: moniker-end

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="665c9-121">Créer une application Web avec l’authentification</span><span class="sxs-lookup"><span data-stu-id="665c9-121">Create a Web app with authentication</span></span>

<span data-ttu-id="665c9-122">Créez un projet d’application Web ASP.NET Core avec des comptes d’utilisateur individuels.</span><span class="sxs-lookup"><span data-stu-id="665c9-122">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="665c9-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="665c9-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="665c9-124">Sélectionnez **Fichier** > **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="665c9-124">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="665c9-125">Sélectionnez **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="665c9-125">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="665c9-126">Nommez le projet **application Web 1** pour avoir le même espace de noms que le projet à télécharger.</span><span class="sxs-lookup"><span data-stu-id="665c9-126">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="665c9-127">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="665c9-127">Click **OK**.</span></span>
* <span data-ttu-id="665c9-128">Sélectionnez une **application Web**ASP.net Core, puis sélectionnez **modifier l’authentification**.</span><span class="sxs-lookup"><span data-stu-id="665c9-128">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="665c9-129">Sélectionnez **comptes d’utilisateur individuels** , puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="665c9-129">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="665c9-130">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="665c9-130">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="665c9-131">Le projet généré fournit une [identité de ASP.net Core](xref:security/authentication/identity) sous forme de bibliothèque de [classes Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="665c9-131">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="665c9-132">La bibliothèque de classes Razor d’identité expose des points `Identity` de terminaison avec la zone.</span><span class="sxs-lookup"><span data-stu-id="665c9-132">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="665c9-133">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="665c9-133">For example:</span></span>

* <span data-ttu-id="665c9-134">/Identity/Account/Login</span><span class="sxs-lookup"><span data-stu-id="665c9-134">/Identity/Account/Login</span></span>
* <span data-ttu-id="665c9-135">/Identity/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="665c9-135">/Identity/Account/Logout</span></span>
* <span data-ttu-id="665c9-136">/Identity/Account/Manage</span><span class="sxs-lookup"><span data-stu-id="665c9-136">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="665c9-137">Appliquer des migrations</span><span class="sxs-lookup"><span data-stu-id="665c9-137">Apply migrations</span></span>

<span data-ttu-id="665c9-138">Appliquez les migrations pour initialiser la base de données.</span><span class="sxs-lookup"><span data-stu-id="665c9-138">Apply the migrations to initialise the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="665c9-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="665c9-139">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="665c9-140">Exécutez la commande suivante dans la console du gestionnaire de package (PMC) :</span><span class="sxs-lookup"><span data-stu-id="665c9-140">Run the following command in the Package Manager Console (PMC):</span></span>

```PM> Update-Database```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="665c9-141">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="665c9-141">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="665c9-142">Registre de test et connexion</span><span class="sxs-lookup"><span data-stu-id="665c9-142">Test Register and Login</span></span>

<span data-ttu-id="665c9-143">Exécutez l’application et inscrivez un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="665c9-143">Run the app and register a user.</span></span> <span data-ttu-id="665c9-144">Selon la taille de votre écran, vous devrez peut-être sélectionner le bouton bascule de navigation pour afficher les liens de **connexion** et de **Registre** .</span><span class="sxs-lookup"><span data-stu-id="665c9-144">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="665c9-145">Configurer les services d’identité</span><span class="sxs-lookup"><span data-stu-id="665c9-145">Configure Identity services</span></span>

<span data-ttu-id="665c9-146">Les services sont ajoutés `ConfigureServices`dans.</span><span class="sxs-lookup"><span data-stu-id="665c9-146">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="665c9-147">Le modèle par défaut consiste à appeler toutes les méthodes `Add{Service}`, puis toutes les méthodes `services.Configure{Service}`.</span><span class="sxs-lookup"><span data-stu-id="665c9-147">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="665c9-148">Le code précédent configure l’identité avec les valeurs d’option par défaut.</span><span class="sxs-lookup"><span data-stu-id="665c9-148">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="665c9-149">Les services sont mis à la disposition de l’application via l' [injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="665c9-149">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="665c9-150">L’identité est activée en appelant [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="665c9-150">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="665c9-151">`UseAuthentication`Ajoute l’authentification [intergiciel (middleware)](xref:fundamentals/middleware/index) au pipeline de demande.</span><span class="sxs-lookup"><span data-stu-id="665c9-151">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   <span data-ttu-id="665c9-152">Les services sont mis à la disposition de l’application via l' [injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="665c9-152">Services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="665c9-153">Identity est activée pour l’application en appelant `UseAuthentication` dans le `Configure` (méthode).</span><span class="sxs-lookup"><span data-stu-id="665c9-153">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="665c9-154">`UseAuthentication`Ajoute l’authentification [intergiciel (middleware)](xref:fundamentals/middleware/index) au pipeline de demande.</span><span class="sxs-lookup"><span data-stu-id="665c9-154">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   <span data-ttu-id="665c9-155">Ces services sont mis à la disposition de l’application via l' [injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="665c9-155">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="665c9-156">Identity est activée pour l’application en appelant `UseIdentity` dans le `Configure` (méthode).</span><span class="sxs-lookup"><span data-stu-id="665c9-156">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="665c9-157">`UseIdentity`Ajoute l’authentification par cookie [intergiciel (middleware)](xref:fundamentals/middleware/index) au pipeline de demande.</span><span class="sxs-lookup"><span data-stu-id="665c9-157">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

<span data-ttu-id="665c9-158">Pour plus d’informations, consultez la [classe IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) et le démarrage de l' [application](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="665c9-158">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="665c9-159">Registre de génération de modèles, connexion et déconnexion</span><span class="sxs-lookup"><span data-stu-id="665c9-159">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="665c9-160">Suivez l' [identité de l’échafaudage dans un projet Razor avec des instructions d’autorisation](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) pour générer le code présenté dans cette section.</span><span class="sxs-lookup"><span data-stu-id="665c9-160">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="665c9-161">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="665c9-161">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="665c9-162">Ajoutez les fichiers Register, login et LogOut.</span><span class="sxs-lookup"><span data-stu-id="665c9-162">Add the Register, Login, and LogOut files.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="665c9-163">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="665c9-163">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="665c9-164">Si vous avez créé le projet avec le nom **application Web 1**, exécutez les commandes suivantes.</span><span class="sxs-lookup"><span data-stu-id="665c9-164">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="665c9-165">Sinon, utilisez l’espace de noms correct `ApplicationDbContext`pour :</span><span class="sxs-lookup"><span data-stu-id="665c9-165">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="665c9-166">PowerShell utilise un point-virgule comme séparateur de commande.</span><span class="sxs-lookup"><span data-stu-id="665c9-166">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="665c9-167">Quand vous utilisez PowerShell, échappez les points-virgules dans la liste de fichiers ou placez la liste de fichiers entre guillemets doubles, comme le montre l’exemple précédent.</span><span class="sxs-lookup"><span data-stu-id="665c9-167">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="665c9-168">Examiner le registre</span><span class="sxs-lookup"><span data-stu-id="665c9-168">Examine Register</span></span>

::: moniker range=">= aspnetcore-2.1"

   <span data-ttu-id="665c9-169">Quand un utilisateur clique sur le lien register `RegisterModel.OnPostAsync` , l’action est appelée.</span><span class="sxs-lookup"><span data-stu-id="665c9-169">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="665c9-170">L’utilisateur est créé par [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) sur l' `_userManager` objet.</span><span class="sxs-lookup"><span data-stu-id="665c9-170">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="665c9-171">`_userManager`est fourni par l’injection de dépendances) :</span><span class="sxs-lookup"><span data-stu-id="665c9-171">`_userManager` is provided by dependency injection):</span></span>

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="665c9-172">Quand un utilisateur clique sur le lien register `Register` , l’action est appelée `AccountController`sur.</span><span class="sxs-lookup"><span data-stu-id="665c9-172">When a user clicks the **Register** link, the `Register` action is invoked on `AccountController`.</span></span> <span data-ttu-id="665c9-173">L' `Register` action crée l’utilisateur en appelant `CreateAsync` sur l' `_userManager` objet (fourni à `AccountController` par l’injection de dépendances) :</span><span class="sxs-lookup"><span data-stu-id="665c9-173">The `Register` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to `AccountController` by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   <span data-ttu-id="665c9-174">Si l’utilisateur a été créé avec succès, l’utilisateur est connecté par l’appel `_signInManager.SignInAsync`à.</span><span class="sxs-lookup"><span data-stu-id="665c9-174">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

   <span data-ttu-id="665c9-175">**Remarque :** Consultez [confirmation du compte](xref:security/authentication/accconfirm#prevent-login-at-registration) pour connaître les étapes permettant d’empêcher la connexion immédiate lors de l’inscription.</span><span class="sxs-lookup"><span data-stu-id="665c9-175">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="665c9-176">Se connecter</span><span class="sxs-lookup"><span data-stu-id="665c9-176">Log in</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="665c9-177">Le formulaire de connexion s’affiche lorsque :</span><span class="sxs-lookup"><span data-stu-id="665c9-177">The Login form is displayed when:</span></span>

* <span data-ttu-id="665c9-178">Le lien **connexion** est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="665c9-178">The **Log in** link is selected.</span></span>
* <span data-ttu-id="665c9-179">Un utilisateur tente d’accéder à une page restreinte à laquelle il n’est pas autorisé à accéder **ou** lorsqu’il n’a pas été authentifié par le système.</span><span class="sxs-lookup"><span data-stu-id="665c9-179">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="665c9-180">Lorsque le formulaire de la page de connexion est envoyé, `OnPostAsync` l’action est appelée.</span><span class="sxs-lookup"><span data-stu-id="665c9-180">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="665c9-181">`PasswordSignInAsync`est appelé sur l' `_signInManager` objet (fourni par l’injection de dépendances).</span><span class="sxs-lookup"><span data-stu-id="665c9-181">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

   <span data-ttu-id="665c9-182">La classe `Controller` de base expose `User` une propriété à laquelle vous pouvez accéder à partir des méthodes de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="665c9-182">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="665c9-183">Par exemple, vous pouvez énumérer `User.Claims` et prendre des décisions d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="665c9-183">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="665c9-184">Pour plus d'informations, consultez <xref:security/authorization/introduction>.</span><span class="sxs-lookup"><span data-stu-id="665c9-184">For more information, see <xref:security/authorization/introduction>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="665c9-185">Le formulaire de connexion s’affiche lorsque les utilisateurs sélectionnent le lien **de** connexion ou sont redirigés lors de l’accès à une page qui requiert une authentification.</span><span class="sxs-lookup"><span data-stu-id="665c9-185">The Login form is displayed when users select the **Log in** link or are redirected when accessing a page that requires authentication.</span></span> <span data-ttu-id="665c9-186">Lorsque l’utilisateur envoie le formulaire sur la page de connexion, l' `AccountController` `Login` action est appelée.</span><span class="sxs-lookup"><span data-stu-id="665c9-186">When the user submits the form on the Login page, the `AccountController` `Login` action is called.</span></span>

<span data-ttu-id="665c9-187">L' `Login` action appelle `PasswordSignInAsync` sur l' `_signInManager` objet (fourni à `AccountController` par l’injection de dépendances).</span><span class="sxs-lookup"><span data-stu-id="665c9-187">The `Login` action calls `PasswordSignInAsync` on the `_signInManager` object (provided to `AccountController` by dependency injection).</span></span>

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

<span data-ttu-id="665c9-188">La classe de`Controller` base `PageModel`(ou) expose `User` une propriété.</span><span class="sxs-lookup"><span data-stu-id="665c9-188">The base (`Controller` or `PageModel`) class exposes a `User` property.</span></span> <span data-ttu-id="665c9-189">Par exemple, `User.Claims` peut être énuméré pour prendre des décisions d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="665c9-189">For example, `User.Claims` can be enumerated to make authorization decisions.</span></span>

::: moniker-end

### <a name="log-out"></a><span data-ttu-id="665c9-190">Se déconnecter</span><span class="sxs-lookup"><span data-stu-id="665c9-190">Log out</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="665c9-191">Le lien de **déconnexion** appelle l' `LogoutModel.OnPost` action.</span><span class="sxs-lookup"><span data-stu-id="665c9-191">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

<span data-ttu-id="665c9-192">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) efface les revendications de l’utilisateur stockées dans un cookie.</span><span class="sxs-lookup"><span data-stu-id="665c9-192">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="665c9-193">La publication est spécifiée dans *pages/Shared/_LoginPartial. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="665c9-193">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="665c9-194">Cliquer sur le lien **déconnexion** appelle `LogOut` l’action.</span><span class="sxs-lookup"><span data-stu-id="665c9-194">Clicking the **Log out** link calls the `LogOut` action.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   <span data-ttu-id="665c9-195">Le code précédent appelle la `_signInManager.SignOutAsync` méthode.</span><span class="sxs-lookup"><span data-stu-id="665c9-195">The preceding code calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="665c9-196">La `SignOutAsync` méthode efface les revendications de l’utilisateur stockées dans un cookie.</span><span class="sxs-lookup"><span data-stu-id="665c9-196">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>

::: moniker-end

## <a name="test-identity"></a><span data-ttu-id="665c9-197">Identité du test</span><span class="sxs-lookup"><span data-stu-id="665c9-197">Test Identity</span></span>

<span data-ttu-id="665c9-198">Les modèles de projet Web par défaut autorisent l’accès anonyme aux pages d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="665c9-198">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="665c9-199">Pour tester l’identité, [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) ajoutez à la page confidentialité.</span><span class="sxs-lookup"><span data-stu-id="665c9-199">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the Privacy page.</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=6)]

<span data-ttu-id="665c9-200">Si vous êtes connecté, déconnectez-vous. Exécutez l’application et sélectionnez le lien **confidentialité** .</span><span class="sxs-lookup"><span data-stu-id="665c9-200">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="665c9-201">Vous êtes redirigé vers la page de connexion.</span><span class="sxs-lookup"><span data-stu-id="665c9-201">You are redirected to the login page.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a><span data-ttu-id="665c9-202">Explorer l’identité</span><span class="sxs-lookup"><span data-stu-id="665c9-202">Explore Identity</span></span>

<span data-ttu-id="665c9-203">Pour explorer l’identité plus en détail :</span><span class="sxs-lookup"><span data-stu-id="665c9-203">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="665c9-204">Créer une source d’interface utilisateur d’identité complète</span><span class="sxs-lookup"><span data-stu-id="665c9-204">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="665c9-205">Examinez la source de chaque page et parcourez le débogueur.</span><span class="sxs-lookup"><span data-stu-id="665c9-205">Examine the source of each page and step through the debugger.</span></span>

::: moniker-end

## <a name="identity-components"></a><span data-ttu-id="665c9-206">Composants d'Identity</span><span class="sxs-lookup"><span data-stu-id="665c9-206">Identity Components</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="665c9-207">Tous les packages NuGet dépendants de l’identité sont inclus dans le sous- [package Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="665c9-207">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="665c9-208">Le package principal pour l’identité est [Microsoft. AspNetCore. Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="665c9-208">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="665c9-209">Ce package contient l’ensemble principal d’interfaces pour ASP.NET Core Identity et est inclus par `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="665c9-209">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="665c9-210">Migration vers ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="665c9-210">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="665c9-211">Pour plus d’informations et de conseils sur la migration de votre magasin d’identités existant, consultez [migrer l’authentification et l’identité](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="665c9-211">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="665c9-212">Définition de la force du mot de passe</span><span class="sxs-lookup"><span data-stu-id="665c9-212">Setting password strength</span></span>

<span data-ttu-id="665c9-213">Consultez [configuration](#pw) d’un exemple qui définit la configuration minimale requise pour le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="665c9-213">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="665c9-214">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="665c9-214">Next Steps</span></span>

* [<span data-ttu-id="665c9-215">Configurer Identity</span><span class="sxs-lookup"><span data-stu-id="665c9-215">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
