---
title: Introduction à Identity sur ASP.NET Core
author: rick-anderson
description: Utiliser Identity à une application ASP.NET Core Découvrez comment définir les exigences de mot de passe (RequireDigit, RequiredLength, RequiredUniqueChars, etc.).
ms.author: riande
ms.date: 08/08/2018
uid: security/authentication/identity
ms.openlocfilehash: 355c53e0c957944cb35c37c6b01e724af5f93f44
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265469"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="e5c08-104">Introduction à Identity sur ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e5c08-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="e5c08-105">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e5c08-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e5c08-106">ASP.NET Core Identity est un système d’appartenance qui ajoute des fonctionnalités de connexion pour les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e5c08-106">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="e5c08-107">Les utilisateurs peuvent créer un compte avec les informations de connexion stockées dans l’identité, ou ils peuvent utiliser un fournisseur de connexion externe.</span><span class="sxs-lookup"><span data-stu-id="e5c08-107">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="e5c08-108">Fournisseurs de connexion externe pris en charge incluent [Facebook, Google, Account Microsoft et Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="e5c08-108">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="e5c08-109">Identité peut être configurée à l’aide d’une base de données SQL Server pour stocker les noms d’utilisateur, les mots de passe et les données de profil.</span><span class="sxs-lookup"><span data-stu-id="e5c08-109">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="e5c08-110">Vous pouvez également un autre magasin persistant peut être utilisé, par exemple, stockage Table Azure.</span><span class="sxs-lookup"><span data-stu-id="e5c08-110">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="e5c08-111">[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([comment télécharger)](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="e5c08-111">[View or download the sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="e5c08-112">Dans cette rubrique, vous allez apprendre à utiliser l’identité pour vous inscrire, connectez-vous et déconnecter un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="e5c08-112">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="e5c08-113">Pour obtenir des instructions plus détaillées sur la création d’applications qui utilisent l’identité, consultez la section étapes suivantes à la fin de cet article.</span><span class="sxs-lookup"><span data-stu-id="e5c08-113">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

::: moniker range=">= aspnetcore-2.1"

<a name="adi"></a>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="e5c08-114">AddDefaultIdentity et AddIdentity</span><span class="sxs-lookup"><span data-stu-id="e5c08-114">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="e5c08-115">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) a été introduit dans ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="e5c08-115">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="e5c08-116">Appel `AddDefaultIdentity` revient à appeler ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="e5c08-116">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* [<span data-ttu-id="e5c08-117">AddIdentity</span><span class="sxs-lookup"><span data-stu-id="e5c08-117">AddIdentity</span></span>](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_AddIdentity__2_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__)
* [<span data-ttu-id="e5c08-118">AddDefaultUI</span><span class="sxs-lookup"><span data-stu-id="e5c08-118">AddDefaultUI</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderuiextensions.adddefaultui?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderUIExtensions_AddDefaultUI_Microsoft_AspNetCore_Identity_IdentityBuilder_)
* [<span data-ttu-id="e5c08-119">AddDefaultTokenProviders</span><span class="sxs-lookup"><span data-stu-id="e5c08-119">AddDefaultTokenProviders</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderextensions.adddefaulttokenproviders?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderExtensions_AddDefaultTokenProviders_Microsoft_AspNetCore_Identity_IdentityBuilder_)

<span data-ttu-id="e5c08-120">Consultez [AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="e5c08-120">See [AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

::: moniker-end

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="e5c08-121">Créer une application Web avec l’authentification</span><span class="sxs-lookup"><span data-stu-id="e5c08-121">Create a Web app with authentication</span></span>

<span data-ttu-id="e5c08-122">Créer un projet d’Application Web ASP.NET Core avec des comptes d’utilisateur individuels.</span><span class="sxs-lookup"><span data-stu-id="e5c08-122">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e5c08-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e5c08-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e5c08-124">Sélectionnez **Fichier** > **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="e5c08-124">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="e5c08-125">Sélectionnez **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="e5c08-125">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="e5c08-126">Nommez le projet **application Web 1** pour avoir le même espace de noms en tant que le téléchargement du projet.</span><span class="sxs-lookup"><span data-stu-id="e5c08-126">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="e5c08-127">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="e5c08-127">Click **OK**.</span></span>
* <span data-ttu-id="e5c08-128">Sélectionnez une ASP.NET Core **Web Application**, puis sélectionnez **modifier l’authentification**.</span><span class="sxs-lookup"><span data-stu-id="e5c08-128">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="e5c08-129">Sélectionnez **comptes d’utilisateur individuels** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="e5c08-129">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e5c08-130">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="e5c08-130">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="e5c08-131">Fournit le projet généré [ASP.NET Core Identity](xref:security/authentication/identity) comme un [bibliothèque de classes Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="e5c08-131">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="e5c08-132">La bibliothèque de classes d’identité Razor expose des points de terminaison avec le `Identity` zone.</span><span class="sxs-lookup"><span data-stu-id="e5c08-132">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="e5c08-133">Exemple :</span><span class="sxs-lookup"><span data-stu-id="e5c08-133">For example:</span></span>

* <span data-ttu-id="e5c08-134">/ Identity/Account/Login</span><span class="sxs-lookup"><span data-stu-id="e5c08-134">/Identity/Account/Login</span></span>
* <span data-ttu-id="e5c08-135">Identité/compte/déconnecter</span><span class="sxs-lookup"><span data-stu-id="e5c08-135">/Identity/Account/Logout</span></span>
* <span data-ttu-id="e5c08-136">/Identity/Account/Manage</span><span class="sxs-lookup"><span data-stu-id="e5c08-136">/Identity/Account/Manage</span></span>

### <a name="test-register-and-login"></a><span data-ttu-id="e5c08-137">Registre de test et de connexion</span><span class="sxs-lookup"><span data-stu-id="e5c08-137">Test Register and Login</span></span>

<span data-ttu-id="e5c08-138">Exécutez l’application et inscrire un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="e5c08-138">Run the app and register a user.</span></span> <span data-ttu-id="e5c08-139">Selon la taille de votre écran, vous devrez peut-être sélectionner le bouton de navigation pour afficher le **inscrire** et **connexion** des liens.</span><span class="sxs-lookup"><span data-stu-id="e5c08-139">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="e5c08-140">Configurer les services d’identité</span><span class="sxs-lookup"><span data-stu-id="e5c08-140">Configure Identity services</span></span>

<span data-ttu-id="e5c08-141">Les services sont ajoutés dans `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e5c08-141">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="e5c08-142">Le modèle par défaut consiste à appeler toutes les méthodes `Add{Service}`, puis toutes les méthodes `services.Configure{Service}`.</span><span class="sxs-lookup"><span data-stu-id="e5c08-142">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="e5c08-143">Le code précédent configure l’identité avec des valeurs d’option par défaut.</span><span class="sxs-lookup"><span data-stu-id="e5c08-143">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="e5c08-144">Services sont accessibles à l’application via [l’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e5c08-144">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="e5c08-145">Identity est activé en appelant [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="e5c08-145">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="e5c08-146">`UseAuthentication`Ajoute l’authentification [intergiciel (middleware)](xref:fundamentals/middleware/index) au pipeline de demande.</span><span class="sxs-lookup"><span data-stu-id="e5c08-146">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   <span data-ttu-id="e5c08-147">Services sont accessibles à l’application via [l’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e5c08-147">Services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="e5c08-148">Identity est activée pour l’application en appelant `UseAuthentication` dans le `Configure` (méthode).</span><span class="sxs-lookup"><span data-stu-id="e5c08-148">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="e5c08-149">`UseAuthentication`Ajoute l’authentification [intergiciel (middleware)](xref:fundamentals/middleware/index) au pipeline de demande.</span><span class="sxs-lookup"><span data-stu-id="e5c08-149">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   <span data-ttu-id="e5c08-150">Ces services sont accessibles à l’application via [l’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e5c08-150">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="e5c08-151">Identity est activée pour l’application en appelant `UseIdentity` dans le `Configure` (méthode).</span><span class="sxs-lookup"><span data-stu-id="e5c08-151">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="e5c08-152">`UseIdentity`Ajoute l’authentification par cookie [intergiciel (middleware)](xref:fundamentals/middleware/index) au pipeline de demande.</span><span class="sxs-lookup"><span data-stu-id="e5c08-152">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

<span data-ttu-id="e5c08-153">Pour plus d’informations, consultez le [IdentityOptions classe](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) et [démarrage de l’Application](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="e5c08-153">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="e5c08-154">Inscription d’une structure, de connexion et de déconnexion</span><span class="sxs-lookup"><span data-stu-id="e5c08-154">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="e5c08-155">Suivez le [structurer d’identité dans un projet Razor avec l’autorisation](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) obtenir des instructions pour générer le code indiqué dans cette section.</span><span class="sxs-lookup"><span data-stu-id="e5c08-155">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e5c08-156">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e5c08-156">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e5c08-157">Ajoutez les fichiers de Registre, de connexion et de déconnexion.</span><span class="sxs-lookup"><span data-stu-id="e5c08-157">Add the Register, Login, and LogOut files.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e5c08-158">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="e5c08-158">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="e5c08-159">Si vous avez créé le projet avec le nom **application Web 1**, exécutez les commandes suivantes.</span><span class="sxs-lookup"><span data-stu-id="e5c08-159">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="e5c08-160">Sinon, utilisez l’espace de noms correct pour le `ApplicationDbContext`:</span><span class="sxs-lookup"><span data-stu-id="e5c08-160">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="e5c08-161">PowerShell utilise le point-virgule comme séparateur de commande.</span><span class="sxs-lookup"><span data-stu-id="e5c08-161">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="e5c08-162">Lorsque vous utilisez PowerShell, les points-virgules dans la liste des fichiers de séquence d’échappement ou placez la liste des fichiers dans des guillemets doubles, comme le montre l’exemple précédent.</span><span class="sxs-lookup"><span data-stu-id="e5c08-162">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="e5c08-163">Examinez le Registre</span><span class="sxs-lookup"><span data-stu-id="e5c08-163">Examine Register</span></span>

::: moniker range=">= aspnetcore-2.1"

   <span data-ttu-id="e5c08-164">Lorsqu’un utilisateur clique sur le **inscrire** lien, le `RegisterModel.OnPostAsync` action est appelée.</span><span class="sxs-lookup"><span data-stu-id="e5c08-164">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="e5c08-165">L’utilisateur est créé par [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) sur la `_userManager` objet.</span><span class="sxs-lookup"><span data-stu-id="e5c08-165">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="e5c08-166">`_userManager` est fourni par l’injection de dépendances) :</span><span class="sxs-lookup"><span data-stu-id="e5c08-166">`_userManager` is provided by dependency injection):</span></span>

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="e5c08-167">Lorsqu’un utilisateur clique sur le **inscrire** lien, le `Register` action est appelée sur `AccountController`.</span><span class="sxs-lookup"><span data-stu-id="e5c08-167">When a user clicks the **Register** link, the `Register` action is invoked on `AccountController`.</span></span> <span data-ttu-id="e5c08-168">Le `Register` action crée l’utilisateur en appelant `CreateAsync` sur le `_userManager` objet (fourni à `AccountController` par injection de dépendances) :</span><span class="sxs-lookup"><span data-stu-id="e5c08-168">The `Register` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to `AccountController` by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   <span data-ttu-id="e5c08-169">Si l’utilisateur a été créé avec succès, l’utilisateur est connecté par l’appel à `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="e5c08-169">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

   <span data-ttu-id="e5c08-170">**Remarque :** Consultez [confirmation de compte](xref:security/authentication/accconfirm#prevent-login-at-registration) pour savoir comment empêcher la connexion immédiate lors de l’inscription.</span><span class="sxs-lookup"><span data-stu-id="e5c08-170">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="e5c08-171">Connectez-vous</span><span class="sxs-lookup"><span data-stu-id="e5c08-171">Log in</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="e5c08-172">Le formulaire de connexion s’affiche lorsque :</span><span class="sxs-lookup"><span data-stu-id="e5c08-172">The Login form is displayed when:</span></span>

* <span data-ttu-id="e5c08-173">Le **connectez-vous** lien est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="e5c08-173">The **Log in** link is selected.</span></span>
* <span data-ttu-id="e5c08-174">Un utilisateur tente d’accéder à une page restreinte qu’ils ne sont pas autorisés à accéder aux **ou** lorsqu’ils n’ont pas été authentifiés par le système.</span><span class="sxs-lookup"><span data-stu-id="e5c08-174">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="e5c08-175">Lors de l’envoi du formulaire sur la page de connexion, le `OnPostAsync` action est appelée.</span><span class="sxs-lookup"><span data-stu-id="e5c08-175">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="e5c08-176">`PasswordSignInAsync` est appelée sur le `_signInManager` objet (fourni par l’injection de dépendances).</span><span class="sxs-lookup"><span data-stu-id="e5c08-176">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

   <span data-ttu-id="e5c08-177">La base de `Controller` classe expose un `User` propriété auxquelles vous pouvez accéder à partir de méthodes de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e5c08-177">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="e5c08-178">Par exemple, vous pouvez énumérer `User.Claims` et prendre des décisions d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="e5c08-178">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="e5c08-179">Pour plus d'informations, consultez <xref:security/authorization/introduction>.</span><span class="sxs-lookup"><span data-stu-id="e5c08-179">For more information, see <xref:security/authorization/introduction>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="e5c08-180">Le formulaire de connexion s’affiche lorsque les utilisateurs sélectionnent le **connectez-vous** lier ou sont redirigés lorsqu’ils accèdent à une page qui requiert une authentification.</span><span class="sxs-lookup"><span data-stu-id="e5c08-180">The Login form is displayed when users select the **Log in** link or are redirected when accessing a page that requires authentication.</span></span> <span data-ttu-id="e5c08-181">Lorsque l’utilisateur soumet le formulaire sur la page de connexion, le `AccountController` `Login` action est appelée.</span><span class="sxs-lookup"><span data-stu-id="e5c08-181">When the user submits the form on the Login page, the `AccountController` `Login` action is called.</span></span>

<span data-ttu-id="e5c08-182">Le `Login` action appels `PasswordSignInAsync` sur le `_signInManager` objet (fourni à `AccountController` par injection de dépendances).</span><span class="sxs-lookup"><span data-stu-id="e5c08-182">The `Login` action calls `PasswordSignInAsync` on the `_signInManager` object (provided to `AccountController` by dependency injection).</span></span>

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

<span data-ttu-id="e5c08-183">La base (`Controller` ou `PageModel`) classe expose un `User` propriété.</span><span class="sxs-lookup"><span data-stu-id="e5c08-183">The base (`Controller` or `PageModel`) class exposes a `User` property.</span></span> <span data-ttu-id="e5c08-184">Par exemple, `User.Claims` peuvent être énumérés afin de prendre des décisions d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="e5c08-184">For example, `User.Claims` can be enumerated to make authorization decisions.</span></span>

::: moniker-end

### <a name="log-out"></a><span data-ttu-id="e5c08-185">Se déconnecter</span><span class="sxs-lookup"><span data-stu-id="e5c08-185">Log out</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="e5c08-186">Le **déconnecter** lien appelle le `LogoutModel.OnPost` action.</span><span class="sxs-lookup"><span data-stu-id="e5c08-186">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

<span data-ttu-id="e5c08-187">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) efface les revendications de l’utilisateur stockées dans un cookie.</span><span class="sxs-lookup"><span data-stu-id="e5c08-187">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span> <span data-ttu-id="e5c08-188">Ne pas rediriger après avoir appelé `SignOutAsync` ou l’utilisateur sera **pas** me déconnecter.</span><span class="sxs-lookup"><span data-stu-id="e5c08-188">Don't redirect after calling `SignOutAsync` or the user will **not** be signed out.</span></span>

<span data-ttu-id="e5c08-189">POST est spécifié dans le *Pages/Shared/_LoginPartial.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e5c08-189">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="e5c08-190">En cliquant sur le **déconnecter** lien appelle le `LogOut` action.</span><span class="sxs-lookup"><span data-stu-id="e5c08-190">Clicking the **Log out** link calls the `LogOut` action.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   <span data-ttu-id="e5c08-191">Le code précédent appelle la `_signInManager.SignOutAsync` (méthode).</span><span class="sxs-lookup"><span data-stu-id="e5c08-191">The preceding code calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="e5c08-192">Le `SignOutAsync` méthode efface les revendications de l’utilisateur stockées dans un cookie.</span><span class="sxs-lookup"><span data-stu-id="e5c08-192">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>

::: moniker-end

## <a name="test-identity"></a><span data-ttu-id="e5c08-193">Tester l’identité</span><span class="sxs-lookup"><span data-stu-id="e5c08-193">Test Identity</span></span>

<span data-ttu-id="e5c08-194">Les modèles de projet web par défaut autoriser l’accès anonyme pour les pages d’accueil.</span><span class="sxs-lookup"><span data-stu-id="e5c08-194">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="e5c08-195">Pour tester l’identité, vous devez ajouter [ `[Authorize]` ](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) à la page de la confidentialité.</span><span class="sxs-lookup"><span data-stu-id="e5c08-195">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the Privacy page.</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=6)]

<span data-ttu-id="e5c08-196">Si vous êtes connecté, déconnectez-vous. Exécutez l’application et sélectionnez le **confidentialité** lien.</span><span class="sxs-lookup"><span data-stu-id="e5c08-196">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="e5c08-197">Vous êtes redirigé vers la page de connexion.</span><span class="sxs-lookup"><span data-stu-id="e5c08-197">You are redirected to the login page.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a><span data-ttu-id="e5c08-198">Explorez l’identité</span><span class="sxs-lookup"><span data-stu-id="e5c08-198">Explore Identity</span></span>

<span data-ttu-id="e5c08-199">Pour Explorer d’identité plus en détail :</span><span class="sxs-lookup"><span data-stu-id="e5c08-199">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="e5c08-200">Créer la source de l’interface utilisateur d’identité complète</span><span class="sxs-lookup"><span data-stu-id="e5c08-200">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="e5c08-201">Examinez la source de chaque page, puis parcourez le débogueur.</span><span class="sxs-lookup"><span data-stu-id="e5c08-201">Examine the source of each page and step through the debugger.</span></span>

::: moniker-end

## <a name="identity-components"></a><span data-ttu-id="e5c08-202">Composants d'Identity</span><span class="sxs-lookup"><span data-stu-id="e5c08-202">Identity Components</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="e5c08-203">Tous les identités NuGet packages dépendants sont inclus dans le [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="e5c08-203">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="e5c08-204">Le package principal pour l’identité est [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="e5c08-204">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="e5c08-205">Ce package contient l’ensemble principal d’interfaces pour ASP.NET Core Identity et est inclus par `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="e5c08-205">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="e5c08-206">Migration vers ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="e5c08-206">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="e5c08-207">Pour plus d’informations et des conseils sur la migration de votre magasin d’identités existant, consultez [migrer l’authentification et identité](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="e5c08-207">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="e5c08-208">Définition du niveau de mot de passe</span><span class="sxs-lookup"><span data-stu-id="e5c08-208">Setting password strength</span></span>

<span data-ttu-id="e5c08-209">Consultez [Configuration](#pw) pour obtenir un exemple qui définit les exigences de mot de passe minimale.</span><span class="sxs-lookup"><span data-stu-id="e5c08-209">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e5c08-210">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="e5c08-210">Next Steps</span></span>

* [<span data-ttu-id="e5c08-211">Configurer Identity</span><span class="sxs-lookup"><span data-stu-id="e5c08-211">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
