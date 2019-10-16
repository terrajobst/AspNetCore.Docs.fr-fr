---
title: Présentation de l’identité sur ASP.NET Core
author: rick-anderson
description: Utilisez l’identité avec une application ASP.NET Core. Découvrez comment définir les exigences de mot de passe (RequireDigit, RequiredLength, RequiredUniqueChars, etc.).
ms.author: riande
ms.date: 10/15/2019
uid: security/authentication/identity
ms.openlocfilehash: 8da13ca5f74a9c829eb8137d33af0684ff88266d
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72333555"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="fa191-104">Présentation de l’identité sur ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fa191-104">Introduction to Identity on ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="fa191-105">De [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fa191-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fa191-106">ASP.NET Core identité est un système d’appartenance qui prend en charge la fonctionnalité de connexion de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="fa191-106">ASP.NET Core Identity is a membership system that supports user interface (UI) login functionality.</span></span> <span data-ttu-id="fa191-107">Les utilisateurs peuvent créer un compte avec les informations de connexion stockées dans l’identité, ou ils peuvent utiliser un fournisseur de connexion externe.</span><span class="sxs-lookup"><span data-stu-id="fa191-107">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="fa191-108">Les fournisseurs de connexion externes pris en charge incluent [Facebook, Google, Microsoft Account et Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="fa191-108">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="fa191-109">L’identité peut être configurée à l’aide d’une base de données SQL Server pour stocker les noms d’utilisateur, les mots de passe et les données de profil.</span><span class="sxs-lookup"><span data-stu-id="fa191-109">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="fa191-110">Vous pouvez également utiliser un autre magasin persistant, par exemple, le stockage table Azure.</span><span class="sxs-lookup"><span data-stu-id="fa191-110">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="fa191-111">Dans cette rubrique, vous allez apprendre à utiliser l’identité pour vous inscrire, vous connecter et déconnecter un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="fa191-111">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="fa191-112">Pour obtenir des instructions plus détaillées sur la création d’applications qui utilisent l’identité, consultez la section étapes suivantes à la fin de cet article.</span><span class="sxs-lookup"><span data-stu-id="fa191-112">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

[!INCLUDE[](~/includes/IdentityServer4.md)]

<span data-ttu-id="fa191-113">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample) ([procédure de téléchargement)](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="fa191-113">[View or download the sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<a name="adi"></a>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="fa191-114">Créer une application Web avec l’authentification</span><span class="sxs-lookup"><span data-stu-id="fa191-114">Create a Web app with authentication</span></span>

<span data-ttu-id="fa191-115">Créez un projet d’application Web ASP.NET Core avec des comptes d’utilisateur individuels.</span><span class="sxs-lookup"><span data-stu-id="fa191-115">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fa191-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fa191-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="fa191-117">Sélectionnez **fichier** > **nouveau** **projet**>.</span><span class="sxs-lookup"><span data-stu-id="fa191-117">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="fa191-118">Sélectionnez **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="fa191-118">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="fa191-119">Nommez le projet **application Web 1** pour avoir le même espace de noms que le projet à télécharger.</span><span class="sxs-lookup"><span data-stu-id="fa191-119">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="fa191-120">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="fa191-120">Click **OK**.</span></span>
* <span data-ttu-id="fa191-121">Sélectionnez une **application Web**ASP.net Core, puis sélectionnez **modifier l’authentification**.</span><span class="sxs-lookup"><span data-stu-id="fa191-121">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="fa191-122">Sélectionnez **comptes d’utilisateur individuels** , puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="fa191-122">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="fa191-123">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="fa191-123">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

<span data-ttu-id="fa191-124">La commande précédente crée une application Web Razor à l’aide de SQLite.</span><span class="sxs-lookup"><span data-stu-id="fa191-124">The preceding command creates a Razor web app using SQLite.</span></span> <span data-ttu-id="fa191-125">Pour créer l’application Web avec la base de données locale, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="fa191-125">To create the web app with LocalDB, run the following command:</span></span>

```dotnetcli
dotnet new webapp --auth Individual -uld -o WebApp1
```

---

<span data-ttu-id="fa191-126">Le projet généré fournit une [identité de ASP.net Core](xref:security/authentication/identity) sous forme de bibliothèque de [classes Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="fa191-126">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="fa191-127">La bibliothèque de classes Razor d’identité expose des points de terminaison avec la zone `Identity`.</span><span class="sxs-lookup"><span data-stu-id="fa191-127">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="fa191-128">Exemple :</span><span class="sxs-lookup"><span data-stu-id="fa191-128">For example:</span></span>

* <span data-ttu-id="fa191-129">/Identity/Account/Login</span><span class="sxs-lookup"><span data-stu-id="fa191-129">/Identity/Account/Login</span></span>
* <span data-ttu-id="fa191-130">/Identity/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="fa191-130">/Identity/Account/Logout</span></span>
* <span data-ttu-id="fa191-131">/Identity/Account/Manage</span><span class="sxs-lookup"><span data-stu-id="fa191-131">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="fa191-132">Appliquer des migrations</span><span class="sxs-lookup"><span data-stu-id="fa191-132">Apply migrations</span></span>

<span data-ttu-id="fa191-133">Appliquez les migrations pour initialiser la base de données.</span><span class="sxs-lookup"><span data-stu-id="fa191-133">Apply the migrations to initialize the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fa191-134">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fa191-134">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fa191-135">Exécutez la commande suivante dans la console du gestionnaire de package (PMC) :</span><span class="sxs-lookup"><span data-stu-id="fa191-135">Run the following command in the Package Manager Console (PMC):</span></span>

`PM> Update-Database`

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="fa191-136">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="fa191-136">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="fa191-137">Les migrations ne sont pas nécessaires pour cette étape lors de l’utilisation de SQLite.</span><span class="sxs-lookup"><span data-stu-id="fa191-137">Migrations are not necessary at this step when using SQLite.</span></span> <span data-ttu-id="fa191-138">Pour la base de données locale, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="fa191-138">For LocalDB, run the following command:</span></span>

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="fa191-139">Registre de test et connexion</span><span class="sxs-lookup"><span data-stu-id="fa191-139">Test Register and Login</span></span>

<span data-ttu-id="fa191-140">Exécutez l’application et inscrivez un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="fa191-140">Run the app and register a user.</span></span> <span data-ttu-id="fa191-141">Selon la taille de votre écran, vous devrez peut-être sélectionner le bouton bascule de navigation pour afficher les liens de **connexion** et de **Registre** .</span><span class="sxs-lookup"><span data-stu-id="fa191-141">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="fa191-142">Configurer les services d’identité</span><span class="sxs-lookup"><span data-stu-id="fa191-142">Configure Identity services</span></span>

<span data-ttu-id="fa191-143">Les services sont ajoutés dans `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="fa191-143">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="fa191-144">Le modèle par défaut consiste à appeler toutes les méthodes `Add{Service}`, puis toutes les méthodes `services.Configure{Service}`.</span><span class="sxs-lookup"><span data-stu-id="fa191-144">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

[!code-csharp[](identity/sample/WebApp3/Startup.cs?name=snippet_configureservices&highlight=10-99)]

<span data-ttu-id="fa191-145">Le code en surbrillance précédent configure l’identité avec les valeurs d’option par défaut.</span><span class="sxs-lookup"><span data-stu-id="fa191-145">The preceding highlighted code configures Identity with default option values.</span></span> <span data-ttu-id="fa191-146">Les services sont mis à la disposition de l’application via l' [injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="fa191-146">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="fa191-147">L’identité est activée en appelant <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>.</span><span class="sxs-lookup"><span data-stu-id="fa191-147">Identity is enabled by calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>.</span></span> <span data-ttu-id="fa191-148">`UseAuthentication` ajoute l' [intergiciel (middleware](xref:fundamentals/middleware/index) ) d’authentification au pipeline de requête.</span><span class="sxs-lookup"><span data-stu-id="fa191-148">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

[!code-csharp[](identity/sample/WebApp3/Startup.cs?name=snippet_configure&highlight=19)]

<span data-ttu-id="fa191-149">L’application générée par un modèle n’utilise pas [d’autorisation](xref:security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="fa191-149">The template-generated app doesn't use [authorization](xref:security/authorization/secure-data).</span></span> <span data-ttu-id="fa191-150">`app.UseAuthorization` est inclus pour s’assurer qu’il est ajouté dans le bon ordre si l’application ajoute une autorisation.</span><span class="sxs-lookup"><span data-stu-id="fa191-150">`app.UseAuthorization` is included to ensure it's added in the correct order should the app add authorization.</span></span> <span data-ttu-id="fa191-151">`UseRouting`, `UseAuthentication`, `UseAuthorization` et `UseEndpoints` doivent être appelés dans l’ordre indiqué dans le code précédent.</span><span class="sxs-lookup"><span data-stu-id="fa191-151">`UseRouting`, `UseAuthentication`, `UseAuthorization`, and `UseEndpoints` must be called in the order shown in the preceding code.</span></span>

<span data-ttu-id="fa191-152">Pour plus d’informations sur `IdentityOptions` et `Startup`, consultez <xref:Microsoft.AspNetCore.Identity.IdentityOptions> et démarrage de l' [application](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="fa191-152">For more information on `IdentityOptions` and `Startup`, see <xref:Microsoft.AspNetCore.Identity.IdentityOptions> and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="fa191-153">Registre de génération de modèles, connexion et déconnexion</span><span class="sxs-lookup"><span data-stu-id="fa191-153">Scaffold Register, Login, and LogOut</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fa191-154">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fa191-154">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fa191-155">Ajoutez les fichiers Register, login et LogOut.</span><span class="sxs-lookup"><span data-stu-id="fa191-155">Add the Register, Login, and LogOut files.</span></span> <span data-ttu-id="fa191-156">Suivez l' [identité de l’échafaudage dans un projet Razor avec des instructions d’autorisation](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) pour générer le code présenté dans cette section.</span><span class="sxs-lookup"><span data-stu-id="fa191-156">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="fa191-157">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="fa191-157">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="fa191-158">Si vous avez créé le projet avec le nom **application Web 1**, exécutez les commandes suivantes.</span><span class="sxs-lookup"><span data-stu-id="fa191-158">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="fa191-159">Sinon, utilisez l’espace de noms correct pour le `ApplicationDbContext` :</span><span class="sxs-lookup"><span data-stu-id="fa191-159">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="fa191-160">PowerShell utilise un point-virgule comme séparateur de commande.</span><span class="sxs-lookup"><span data-stu-id="fa191-160">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="fa191-161">Quand vous utilisez PowerShell, échappez les points-virgules dans la liste de fichiers ou placez la liste de fichiers entre guillemets doubles, comme le montre l’exemple précédent.</span><span class="sxs-lookup"><span data-stu-id="fa191-161">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

<span data-ttu-id="fa191-162">Pour plus d’informations sur la génération de modèles automatique d’identité, consultez identification de l' [échafaudage dans un projet Razor avec autorisation](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization).</span><span class="sxs-lookup"><span data-stu-id="fa191-162">For more information on scaffolding Identity, see [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization).</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="fa191-163">Examiner le registre</span><span class="sxs-lookup"><span data-stu-id="fa191-163">Examine Register</span></span>

<span data-ttu-id="fa191-164">Quand un utilisateur clique sur le lien **Register** , l’action `RegisterModel.OnPostAsync` est appelée.</span><span class="sxs-lookup"><span data-stu-id="fa191-164">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="fa191-165">L’utilisateur est créé par [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) sur l’objet `_userManager`.</span><span class="sxs-lookup"><span data-stu-id="fa191-165">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="fa191-166">`_userManager` est fourni par l’injection de dépendances) :</span><span class="sxs-lookup"><span data-stu-id="fa191-166">`_userManager` is provided by dependency injection):</span></span>

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=9)]

<span data-ttu-id="fa191-167">Si l’utilisateur a été créé avec succès, l’utilisateur est connecté par l’appel à `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="fa191-167">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

<span data-ttu-id="fa191-168">Consultez [confirmation du compte](xref:security/authentication/accconfirm#prevent-login-at-registration) pour connaître les étapes permettant d’empêcher la connexion immédiate lors de l’inscription.</span><span class="sxs-lookup"><span data-stu-id="fa191-168">See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="fa191-169">Se connecter</span><span class="sxs-lookup"><span data-stu-id="fa191-169">Log in</span></span>

<span data-ttu-id="fa191-170">Le formulaire de connexion s’affiche lorsque :</span><span class="sxs-lookup"><span data-stu-id="fa191-170">The Login form is displayed when:</span></span>

* <span data-ttu-id="fa191-171">Le lien **connexion** est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="fa191-171">The **Log in** link is selected.</span></span>
* <span data-ttu-id="fa191-172">Un utilisateur tente d’accéder à une page restreinte à laquelle il n’est pas autorisé à accéder **ou** lorsqu’il n’a pas été authentifié par le système.</span><span class="sxs-lookup"><span data-stu-id="fa191-172">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="fa191-173">Lorsque le formulaire de la page de connexion est envoyé, l’action `OnPostAsync` est appelée.</span><span class="sxs-lookup"><span data-stu-id="fa191-173">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="fa191-174">`PasswordSignInAsync` est appelé sur l’objet `_signInManager` (fourni par l’injection de dépendances).</span><span class="sxs-lookup"><span data-stu-id="fa191-174">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

<span data-ttu-id="fa191-175">La classe `Controller` de base expose une propriété `User` qui est accessible à partir des méthodes de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="fa191-175">The base `Controller` class exposes a `User` property that can be accessed from controller methods.</span></span> <span data-ttu-id="fa191-176">Par exemple, vous pouvez énumérer `User.Claims` et prendre des décisions d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="fa191-176">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="fa191-177">Pour plus d'informations, consultez <xref:security/authorization/introduction>.</span><span class="sxs-lookup"><span data-stu-id="fa191-177">For more information, see <xref:security/authorization/introduction>.</span></span>

### <a name="log-out"></a><span data-ttu-id="fa191-178">Se déconnecter</span><span class="sxs-lookup"><span data-stu-id="fa191-178">Log out</span></span>

<span data-ttu-id="fa191-179">Le lien **déconnexion** appelle l’action `LogoutModel.OnPost`.</span><span class="sxs-lookup"><span data-stu-id="fa191-179">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Logout.cshtml.cs?highlight=36)]

<span data-ttu-id="fa191-180">Dans le code précédent, le code `return RedirectToPage();` doit être une redirection afin que le navigateur exécute une nouvelle demande et que l’identité de l’utilisateur soit mise à jour.</span><span class="sxs-lookup"><span data-stu-id="fa191-180">In the preceding code, the code `return RedirectToPage();` needs to be a redirect so that the browser performs a new request and the identity for the user gets updated.</span></span>

<span data-ttu-id="fa191-181">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) efface les revendications de l’utilisateur stockées dans un cookie.</span><span class="sxs-lookup"><span data-stu-id="fa191-181">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="fa191-182">La publication est spécifiée dans *pages/Shared/_LoginPartial. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="fa191-182">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp3/Pages/Shared/_LoginPartial.cshtml?highlight=15)]

## <a name="test-identity"></a><span data-ttu-id="fa191-183">Identité du test</span><span class="sxs-lookup"><span data-stu-id="fa191-183">Test Identity</span></span>

<span data-ttu-id="fa191-184">Les modèles de projet Web par défaut autorisent l’accès anonyme aux pages d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="fa191-184">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="fa191-185">Pour tester l’identité, ajoutez [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute):</span><span class="sxs-lookup"><span data-stu-id="fa191-185">To test Identity, add [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute):</span></span>

[!code-csharp[](identity/sample/WebApp3/Pages/Privacy.cshtml.cs?highlight=7)]

<span data-ttu-id="fa191-186">Si vous êtes connecté, déconnectez-vous. Exécutez l’application et sélectionnez le lien **confidentialité** .</span><span class="sxs-lookup"><span data-stu-id="fa191-186">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="fa191-187">Vous êtes redirigé vers la page de connexion.</span><span class="sxs-lookup"><span data-stu-id="fa191-187">You are redirected to the login page.</span></span>

### <a name="explore-identity"></a><span data-ttu-id="fa191-188">Explorer l’identité</span><span class="sxs-lookup"><span data-stu-id="fa191-188">Explore Identity</span></span>

<span data-ttu-id="fa191-189">Pour explorer l’identité plus en détail :</span><span class="sxs-lookup"><span data-stu-id="fa191-189">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="fa191-190">Créer une source d’interface utilisateur d’identité complète</span><span class="sxs-lookup"><span data-stu-id="fa191-190">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="fa191-191">Examinez la source de chaque page et parcourez le débogueur.</span><span class="sxs-lookup"><span data-stu-id="fa191-191">Examine the source of each page and step through the debugger.</span></span>

## <a name="identity-components"></a><span data-ttu-id="fa191-192">Composants d’identité</span><span class="sxs-lookup"><span data-stu-id="fa191-192">Identity Components</span></span>

<span data-ttu-id="fa191-193">Tous les packages NuGet dépendants de l’identité sont inclus dans le [ASP.net Core Framework partagé](xref:aspnetcore-3.0#use-the-aspnet-core-shared-framework).</span><span class="sxs-lookup"><span data-stu-id="fa191-193">All the Identity-dependent NuGet packages are included in the [ASP.NET Core shared framework](xref:aspnetcore-3.0#use-the-aspnet-core-shared-framework).</span></span>

<span data-ttu-id="fa191-194">Le package principal pour l’identité est [Microsoft. AspNetCore. Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="fa191-194">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="fa191-195">Ce package contient l’ensemble principal d’interfaces pour ASP.NET Core identité et est inclus par `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="fa191-195">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="fa191-196">Migration vers ASP.NET Core identité</span><span class="sxs-lookup"><span data-stu-id="fa191-196">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="fa191-197">Pour plus d’informations et de conseils sur la migration de votre magasin d’identités existant, consultez [migrer l’authentification et l’identité](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="fa191-197">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="fa191-198">Définition de la force du mot de passe</span><span class="sxs-lookup"><span data-stu-id="fa191-198">Setting password strength</span></span>

<span data-ttu-id="fa191-199">Consultez [configuration](#pw) d’un exemple qui définit la configuration minimale requise pour le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="fa191-199">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="fa191-200">AddDefaultIdentity et AddIdentity</span><span class="sxs-lookup"><span data-stu-id="fa191-200">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="fa191-201"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> a été introduite dans ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="fa191-201"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="fa191-202">L’appel de `AddDefaultIdentity` est semblable à l’appel de ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="fa191-202">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentity*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderExtensions.AddDefaultTokenProviders*>

<span data-ttu-id="fa191-203">Pour plus d’informations, consultez [source AddDefaultIdentity](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) .</span><span class="sxs-lookup"><span data-stu-id="fa191-203">See [AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fa191-204">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="fa191-204">Next Steps</span></span>

* [<span data-ttu-id="fa191-205">Configurer Identity</span><span class="sxs-lookup"><span data-stu-id="fa191-205">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="fa191-206">De [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fa191-206">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fa191-207">ASP.NET Core identité est un système d’appartenance qui ajoute des fonctionnalités de connexion aux applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fa191-207">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="fa191-208">Les utilisateurs peuvent créer un compte avec les informations de connexion stockées dans l’identité, ou ils peuvent utiliser un fournisseur de connexion externe.</span><span class="sxs-lookup"><span data-stu-id="fa191-208">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="fa191-209">Les fournisseurs de connexion externes pris en charge incluent [Facebook, Google, Microsoft Account et Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="fa191-209">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="fa191-210">L’identité peut être configurée à l’aide d’une base de données SQL Server pour stocker les noms d’utilisateur, les mots de passe et les données de profil.</span><span class="sxs-lookup"><span data-stu-id="fa191-210">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="fa191-211">Vous pouvez également utiliser un autre magasin persistant, par exemple, le stockage table Azure.</span><span class="sxs-lookup"><span data-stu-id="fa191-211">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="fa191-212">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([procédure de téléchargement)](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="fa191-212">[View or download the sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="fa191-213">Dans cette rubrique, vous allez apprendre à utiliser l’identité pour vous inscrire, vous connecter et déconnecter un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="fa191-213">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="fa191-214">Pour obtenir des instructions plus détaillées sur la création d’applications qui utilisent l’identité, consultez la section étapes suivantes à la fin de cet article.</span><span class="sxs-lookup"><span data-stu-id="fa191-214">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

<a name="adi"></a>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="fa191-215">AddDefaultIdentity et AddIdentity</span><span class="sxs-lookup"><span data-stu-id="fa191-215">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="fa191-216"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> a été introduite dans ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="fa191-216"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="fa191-217">L’appel de `AddDefaultIdentity` est semblable à l’appel de ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="fa191-217">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentity*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderExtensions.AddDefaultTokenProviders*>

<span data-ttu-id="fa191-218">Pour plus d’informations, consultez [source AddDefaultIdentity](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) .</span><span class="sxs-lookup"><span data-stu-id="fa191-218">See [AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="fa191-219">Créer une application Web avec l’authentification</span><span class="sxs-lookup"><span data-stu-id="fa191-219">Create a Web app with authentication</span></span>

<span data-ttu-id="fa191-220">Créez un projet d’application Web ASP.NET Core avec des comptes d’utilisateur individuels.</span><span class="sxs-lookup"><span data-stu-id="fa191-220">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fa191-221">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fa191-221">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="fa191-222">Sélectionnez **fichier** > **nouveau** **projet**>.</span><span class="sxs-lookup"><span data-stu-id="fa191-222">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="fa191-223">Sélectionnez **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="fa191-223">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="fa191-224">Nommez le projet **application Web 1** pour avoir le même espace de noms que le projet à télécharger.</span><span class="sxs-lookup"><span data-stu-id="fa191-224">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="fa191-225">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="fa191-225">Click **OK**.</span></span>
* <span data-ttu-id="fa191-226">Sélectionnez une **application Web**ASP.net Core, puis sélectionnez **modifier l’authentification**.</span><span class="sxs-lookup"><span data-stu-id="fa191-226">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="fa191-227">Sélectionnez **comptes d’utilisateur individuels** , puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="fa191-227">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="fa191-228">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="fa191-228">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="fa191-229">Le projet généré fournit une [identité de ASP.net Core](xref:security/authentication/identity) sous forme de bibliothèque de [classes Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="fa191-229">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="fa191-230">La bibliothèque de classes Razor d’identité expose des points de terminaison avec la zone `Identity`.</span><span class="sxs-lookup"><span data-stu-id="fa191-230">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="fa191-231">Exemple :</span><span class="sxs-lookup"><span data-stu-id="fa191-231">For example:</span></span>

* <span data-ttu-id="fa191-232">/Identity/Account/Login</span><span class="sxs-lookup"><span data-stu-id="fa191-232">/Identity/Account/Login</span></span>
* <span data-ttu-id="fa191-233">/Identity/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="fa191-233">/Identity/Account/Logout</span></span>
* <span data-ttu-id="fa191-234">/Identity/Account/Manage</span><span class="sxs-lookup"><span data-stu-id="fa191-234">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="fa191-235">Appliquer des migrations</span><span class="sxs-lookup"><span data-stu-id="fa191-235">Apply migrations</span></span>

<span data-ttu-id="fa191-236">Appliquez les migrations pour initialiser la base de données.</span><span class="sxs-lookup"><span data-stu-id="fa191-236">Apply the migrations to initialize the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fa191-237">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fa191-237">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fa191-238">Exécutez la commande suivante dans la console du gestionnaire de package (PMC) :</span><span class="sxs-lookup"><span data-stu-id="fa191-238">Run the following command in the Package Manager Console (PMC):</span></span>

```PM> Update-Database```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="fa191-239">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="fa191-239">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="fa191-240">Registre de test et connexion</span><span class="sxs-lookup"><span data-stu-id="fa191-240">Test Register and Login</span></span>

<span data-ttu-id="fa191-241">Exécutez l’application et inscrivez un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="fa191-241">Run the app and register a user.</span></span> <span data-ttu-id="fa191-242">Selon la taille de votre écran, vous devrez peut-être sélectionner le bouton bascule de navigation pour afficher les liens de **connexion** et de **Registre** .</span><span class="sxs-lookup"><span data-stu-id="fa191-242">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="fa191-243">Configurer les services d’identité</span><span class="sxs-lookup"><span data-stu-id="fa191-243">Configure Identity services</span></span>

<span data-ttu-id="fa191-244">Les services sont ajoutés dans `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="fa191-244">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="fa191-245">Le modèle par défaut consiste à appeler toutes les méthodes `Add{Service}`, puis toutes les méthodes `services.Configure{Service}`.</span><span class="sxs-lookup"><span data-stu-id="fa191-245">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="fa191-246">Le code précédent configure l’identité avec les valeurs d’option par défaut.</span><span class="sxs-lookup"><span data-stu-id="fa191-246">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="fa191-247">Les services sont mis à la disposition de l’application via l' [injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="fa191-247">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="fa191-248">L’identité est activée en appelant [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="fa191-248">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="fa191-249">`UseAuthentication` ajoute l' [intergiciel (middleware](xref:fundamentals/middleware/index) ) d’authentification au pipeline de requête.</span><span class="sxs-lookup"><span data-stu-id="fa191-249">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

<span data-ttu-id="fa191-250">Pour plus d’informations, consultez la [classe IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) et le démarrage de l' [application](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="fa191-250">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="fa191-251">Registre de génération de modèles, connexion et déconnexion</span><span class="sxs-lookup"><span data-stu-id="fa191-251">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="fa191-252">Suivez l' [identité de l’échafaudage dans un projet Razor avec des instructions d’autorisation](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) pour générer le code présenté dans cette section.</span><span class="sxs-lookup"><span data-stu-id="fa191-252">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fa191-253">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fa191-253">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fa191-254">Ajoutez les fichiers Register, login et LogOut.</span><span class="sxs-lookup"><span data-stu-id="fa191-254">Add the Register, Login, and LogOut files.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="fa191-255">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="fa191-255">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="fa191-256">Si vous avez créé le projet avec le nom **application Web 1**, exécutez les commandes suivantes.</span><span class="sxs-lookup"><span data-stu-id="fa191-256">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="fa191-257">Sinon, utilisez l’espace de noms correct pour le `ApplicationDbContext` :</span><span class="sxs-lookup"><span data-stu-id="fa191-257">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="fa191-258">PowerShell utilise un point-virgule comme séparateur de commande.</span><span class="sxs-lookup"><span data-stu-id="fa191-258">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="fa191-259">Quand vous utilisez PowerShell, échappez les points-virgules dans la liste de fichiers ou placez la liste de fichiers entre guillemets doubles, comme le montre l’exemple précédent.</span><span class="sxs-lookup"><span data-stu-id="fa191-259">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="fa191-260">Examiner le registre</span><span class="sxs-lookup"><span data-stu-id="fa191-260">Examine Register</span></span>

<span data-ttu-id="fa191-261">Quand un utilisateur clique sur le lien **Register** , l’action `RegisterModel.OnPostAsync` est appelée.</span><span class="sxs-lookup"><span data-stu-id="fa191-261">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="fa191-262">L’utilisateur est créé par [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) sur l’objet `_userManager`.</span><span class="sxs-lookup"><span data-stu-id="fa191-262">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="fa191-263">`_userManager` est fourni par l’injection de dépendances) :</span><span class="sxs-lookup"><span data-stu-id="fa191-263">`_userManager` is provided by dependency injection):</span></span>

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7)]

<span data-ttu-id="fa191-264">Si l’utilisateur a été créé avec succès, l’utilisateur est connecté par l’appel à `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="fa191-264">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

<span data-ttu-id="fa191-265">**Remarque :** Consultez [confirmation du compte](xref:security/authentication/accconfirm#prevent-login-at-registration) pour connaître les étapes permettant d’empêcher la connexion immédiate lors de l’inscription.</span><span class="sxs-lookup"><span data-stu-id="fa191-265">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="fa191-266">Se connecter</span><span class="sxs-lookup"><span data-stu-id="fa191-266">Log in</span></span>

<span data-ttu-id="fa191-267">Le formulaire de connexion s’affiche lorsque :</span><span class="sxs-lookup"><span data-stu-id="fa191-267">The Login form is displayed when:</span></span>

* <span data-ttu-id="fa191-268">Le lien **connexion** est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="fa191-268">The **Log in** link is selected.</span></span>
* <span data-ttu-id="fa191-269">Un utilisateur tente d’accéder à une page restreinte à laquelle il n’est pas autorisé à accéder **ou** lorsqu’il n’a pas été authentifié par le système.</span><span class="sxs-lookup"><span data-stu-id="fa191-269">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="fa191-270">Lorsque le formulaire de la page de connexion est envoyé, l’action `OnPostAsync` est appelée.</span><span class="sxs-lookup"><span data-stu-id="fa191-270">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="fa191-271">`PasswordSignInAsync` est appelé sur l’objet `_signInManager` (fourni par l’injection de dépendances).</span><span class="sxs-lookup"><span data-stu-id="fa191-271">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

<span data-ttu-id="fa191-272">La classe `Controller` de base expose une propriété `User` à laquelle vous pouvez accéder à partir des méthodes de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="fa191-272">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="fa191-273">Par exemple, vous pouvez énumérer `User.Claims` et prendre des décisions d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="fa191-273">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="fa191-274">Pour plus d'informations, consultez <xref:security/authorization/introduction>.</span><span class="sxs-lookup"><span data-stu-id="fa191-274">For more information, see <xref:security/authorization/introduction>.</span></span>

### <a name="log-out"></a><span data-ttu-id="fa191-275">Se déconnecter</span><span class="sxs-lookup"><span data-stu-id="fa191-275">Log out</span></span>

<span data-ttu-id="fa191-276">Le lien **déconnexion** appelle l’action `LogoutModel.OnPost`.</span><span class="sxs-lookup"><span data-stu-id="fa191-276">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

<span data-ttu-id="fa191-277">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) efface les revendications de l’utilisateur stockées dans un cookie.</span><span class="sxs-lookup"><span data-stu-id="fa191-277">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="fa191-278">La publication est spécifiée dans *pages/Shared/_LoginPartial. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="fa191-278">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

## <a name="test-identity"></a><span data-ttu-id="fa191-279">Identité du test</span><span class="sxs-lookup"><span data-stu-id="fa191-279">Test Identity</span></span>

<span data-ttu-id="fa191-280">Les modèles de projet Web par défaut autorisent l’accès anonyme aux pages d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="fa191-280">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="fa191-281">Pour tester l’identité, ajoutez [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) à la page de confidentialité.</span><span class="sxs-lookup"><span data-stu-id="fa191-281">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the Privacy page.</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=7)]

<span data-ttu-id="fa191-282">Si vous êtes connecté, déconnectez-vous. Exécutez l’application et sélectionnez le lien **confidentialité** .</span><span class="sxs-lookup"><span data-stu-id="fa191-282">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="fa191-283">Vous êtes redirigé vers la page de connexion.</span><span class="sxs-lookup"><span data-stu-id="fa191-283">You are redirected to the login page.</span></span>

### <a name="explore-identity"></a><span data-ttu-id="fa191-284">Explorer l’identité</span><span class="sxs-lookup"><span data-stu-id="fa191-284">Explore Identity</span></span>

<span data-ttu-id="fa191-285">Pour explorer l’identité plus en détail :</span><span class="sxs-lookup"><span data-stu-id="fa191-285">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="fa191-286">Créer une source d’interface utilisateur d’identité complète</span><span class="sxs-lookup"><span data-stu-id="fa191-286">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="fa191-287">Examinez la source de chaque page et parcourez le débogueur.</span><span class="sxs-lookup"><span data-stu-id="fa191-287">Examine the source of each page and step through the debugger.</span></span>

## <a name="identity-components"></a><span data-ttu-id="fa191-288">Composants d’identité</span><span class="sxs-lookup"><span data-stu-id="fa191-288">Identity Components</span></span>

<span data-ttu-id="fa191-289">Tous les packages NuGet dépendants de l’identité sont inclus dans le sous- [package Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="fa191-289">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="fa191-290">Le package principal pour l’identité est [Microsoft. AspNetCore. Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="fa191-290">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="fa191-291">Ce package contient l’ensemble principal d’interfaces pour ASP.NET Core identité et est inclus par `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="fa191-291">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="fa191-292">Migration vers ASP.NET Core identité</span><span class="sxs-lookup"><span data-stu-id="fa191-292">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="fa191-293">Pour plus d’informations et de conseils sur la migration de votre magasin d’identités existant, consultez [migrer l’authentification et l’identité](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="fa191-293">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="fa191-294">Définition de la force du mot de passe</span><span class="sxs-lookup"><span data-stu-id="fa191-294">Setting password strength</span></span>

<span data-ttu-id="fa191-295">Consultez [configuration](#pw) d’un exemple qui définit la configuration minimale requise pour le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="fa191-295">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fa191-296">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="fa191-296">Next Steps</span></span>

* [<span data-ttu-id="fa191-297">Configurer Identity</span><span class="sxs-lookup"><span data-stu-id="fa191-297">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>

::: moniker-end
