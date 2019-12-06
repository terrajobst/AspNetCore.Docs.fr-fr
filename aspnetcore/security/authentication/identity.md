---
title: Introduction à Identity sur ASP.NET Core
author: rick-anderson
description: Utiliser Identity à une application ASP.NET Core Découvrez comment définir les exigences de mot de passe (RequireDigit, RequiredLength, RequiredUniqueChars, etc.).
ms.author: riande
ms.date: 12/05/2019
uid: security/authentication/identity
ms.openlocfilehash: c867b73a96fd081f6e2ca17fef561ac539c0a129
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880731"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="4845a-104">Introduction à Identity sur ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4845a-104">Introduction to Identity on ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="4845a-105">De [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4845a-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4845a-106">ASP.NET Core l’identité :</span><span class="sxs-lookup"><span data-stu-id="4845a-106">ASP.NET Core Identity:</span></span>

* <span data-ttu-id="4845a-107">Est une API qui prend en charge la fonctionnalité de connexion de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="4845a-107">Is an API that supports user interface (UI) login functionality.</span></span>
* <span data-ttu-id="4845a-108">Gère les utilisateurs, les mots de passe, les données de profil, les rôles, les revendications, les jetons, la confirmation par e-mail et bien plus encore.</span><span class="sxs-lookup"><span data-stu-id="4845a-108">Manages users, passwords, profile data, roles, claims, tokens, email confirmation, and more.</span></span>

<span data-ttu-id="4845a-109">Les utilisateurs peuvent créer un compte avec les informations de connexion stockées dans l’identité, ou ils peuvent utiliser un fournisseur de connexion externe.</span><span class="sxs-lookup"><span data-stu-id="4845a-109">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="4845a-110">Les fournisseurs de connexion externes pris en charge incluent [Facebook, Google, Microsoft Account et Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="4845a-110">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="4845a-111">Le [code source](https://github.com/aspnet/AspNetCore/tree/master/src/Identity) de l’identité est disponible sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="4845a-111">The [Identity source code](https://github.com/aspnet/AspNetCore/tree/master/src/Identity) is available on GitHub.</span></span> <span data-ttu-id="4845a-112">[Identification de l’échafaudage](xref:security/authentication/scaffold-identity) et affichage des fichiers générés pour examiner l’interaction du modèle avec l’identité.</span><span class="sxs-lookup"><span data-stu-id="4845a-112">[Scaffold Identity](xref:security/authentication/scaffold-identity) and view the generated files to review the template interaction with Identity.</span></span>

<span data-ttu-id="4845a-113">L’identité est généralement configurée à l’aide d’une base de données SQL Server pour stocker les noms d’utilisateur, les mots de passe et les données de profil.</span><span class="sxs-lookup"><span data-stu-id="4845a-113">Identity is typically configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="4845a-114">Vous pouvez également utiliser un autre magasin persistant, par exemple, le stockage table Azure.</span><span class="sxs-lookup"><span data-stu-id="4845a-114">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="4845a-115">Dans cette rubrique, vous allez apprendre à utiliser l’identité pour vous inscrire, vous connecter et déconnecter un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="4845a-115">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="4845a-116">Pour obtenir des instructions plus détaillées sur la création d’applications qui utilisent l’identité, consultez la section étapes suivantes à la fin de cet article.</span><span class="sxs-lookup"><span data-stu-id="4845a-116">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

<span data-ttu-id="4845a-117">[Plateforme d’identité Microsoft](/azure/active-directory/develop/) :</span><span class="sxs-lookup"><span data-stu-id="4845a-117">[Microsoft identity platform](/azure/active-directory/develop/) is:</span></span>

* <span data-ttu-id="4845a-118">Évolution de la plateforme de développement Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="4845a-118">An evolution of the Azure Active Directory (Azure AD) developer platform.</span></span>
* <span data-ttu-id="4845a-119">Sans rapport avec ASP.NET Core identité.</span><span class="sxs-lookup"><span data-stu-id="4845a-119">Unrelated to ASP.NET Core Identity.</span></span>

[!INCLUDE[](~/includes/IdentityServer4.md)]

<span data-ttu-id="4845a-120">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample) ([procédure de téléchargement)](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="4845a-120">[View or download the sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<a name="adi"></a>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="4845a-121">Créer une application Web avec l’authentification</span><span class="sxs-lookup"><span data-stu-id="4845a-121">Create a Web app with authentication</span></span>

<span data-ttu-id="4845a-122">Créez un projet d’application Web ASP.NET Core avec des comptes d’utilisateur individuels.</span><span class="sxs-lookup"><span data-stu-id="4845a-122">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4845a-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4845a-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4845a-124">Sélectionnez **Fichier** > **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="4845a-124">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="4845a-125">Sélectionnez **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="4845a-125">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="4845a-126">Nommez le projet **application Web 1** pour avoir le même espace de noms que le projet à télécharger.</span><span class="sxs-lookup"><span data-stu-id="4845a-126">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="4845a-127">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="4845a-127">Click **OK**.</span></span>
* <span data-ttu-id="4845a-128">Sélectionnez une **application Web**ASP.net Core, puis sélectionnez **modifier l’authentification**.</span><span class="sxs-lookup"><span data-stu-id="4845a-128">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="4845a-129">Sélectionnez **comptes d’utilisateur individuels** , puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="4845a-129">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4845a-130">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="4845a-130">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

<span data-ttu-id="4845a-131">La commande précédente crée une application Web Razor à l’aide de SQLite.</span><span class="sxs-lookup"><span data-stu-id="4845a-131">The preceding command creates a Razor web app using SQLite.</span></span> <span data-ttu-id="4845a-132">Pour créer l’application Web avec la base de données locale, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="4845a-132">To create the web app with LocalDB, run the following command:</span></span>

```dotnetcli
dotnet new webapp --auth Individual -uld -o WebApp1
```

---

<span data-ttu-id="4845a-133">Le projet généré fournit une [identité de ASP.net Core](xref:security/authentication/identity) sous forme de bibliothèque de [classes Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="4845a-133">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="4845a-134">La bibliothèque de classes Razor d’identité expose des points de terminaison avec la zone de `Identity`.</span><span class="sxs-lookup"><span data-stu-id="4845a-134">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="4845a-135">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="4845a-135">For example:</span></span>

* <span data-ttu-id="4845a-136">/Identity/Account/Login</span><span class="sxs-lookup"><span data-stu-id="4845a-136">/Identity/Account/Login</span></span>
* <span data-ttu-id="4845a-137">/Identity/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="4845a-137">/Identity/Account/Logout</span></span>
* <span data-ttu-id="4845a-138">/Identity/Account/Manage</span><span class="sxs-lookup"><span data-stu-id="4845a-138">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="4845a-139">Appliquer des migrations</span><span class="sxs-lookup"><span data-stu-id="4845a-139">Apply migrations</span></span>

<span data-ttu-id="4845a-140">Appliquez les migrations pour initialiser la base de données.</span><span class="sxs-lookup"><span data-stu-id="4845a-140">Apply the migrations to initialize the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4845a-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4845a-141">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4845a-142">Exécutez la commande suivante dans la console du gestionnaire de package (PMC) :</span><span class="sxs-lookup"><span data-stu-id="4845a-142">Run the following command in the Package Manager Console (PMC):</span></span>

`PM> Update-Database`

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4845a-143">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="4845a-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="4845a-144">Les migrations ne sont pas nécessaires pour cette étape lors de l’utilisation de SQLite.</span><span class="sxs-lookup"><span data-stu-id="4845a-144">Migrations are not necessary at this step when using SQLite.</span></span> <span data-ttu-id="4845a-145">Pour la base de données locale, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="4845a-145">For LocalDB, run the following command:</span></span>

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="4845a-146">Registre de test et connexion</span><span class="sxs-lookup"><span data-stu-id="4845a-146">Test Register and Login</span></span>

<span data-ttu-id="4845a-147">Exécutez l’application et inscrivez un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="4845a-147">Run the app and register a user.</span></span> <span data-ttu-id="4845a-148">Selon la taille de votre écran, vous devrez peut-être sélectionner le bouton bascule de navigation pour afficher les liens de **connexion** et de **Registre** .</span><span class="sxs-lookup"><span data-stu-id="4845a-148">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="4845a-149">Configurer les services d’identité</span><span class="sxs-lookup"><span data-stu-id="4845a-149">Configure Identity services</span></span>

<span data-ttu-id="4845a-150">Les services sont ajoutés dans `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="4845a-150">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="4845a-151">Le modèle par défaut consiste à appeler toutes les méthodes `Add{Service}`, puis toutes les méthodes `services.Configure{Service}`.</span><span class="sxs-lookup"><span data-stu-id="4845a-151">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

[!code-csharp[](identity/sample/WebApp3/Startup.cs?name=snippet_configureservices&highlight=10-99)]

<span data-ttu-id="4845a-152">Le code en surbrillance précédent configure l’identité avec les valeurs d’option par défaut.</span><span class="sxs-lookup"><span data-stu-id="4845a-152">The preceding highlighted code configures Identity with default option values.</span></span> <span data-ttu-id="4845a-153">Les services sont mis à la disposition de l’application via l' [injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="4845a-153">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="4845a-154">L’identité est activée en appelant <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>.</span><span class="sxs-lookup"><span data-stu-id="4845a-154">Identity is enabled by calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>.</span></span> <span data-ttu-id="4845a-155">`UseAuthentication`Ajoute l’authentification [intergiciel (middleware)](xref:fundamentals/middleware/index) au pipeline de demande.</span><span class="sxs-lookup"><span data-stu-id="4845a-155">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

[!code-csharp[](identity/sample/WebApp3/Startup.cs?name=snippet_configure&highlight=19)]

<span data-ttu-id="4845a-156">L’application générée par un modèle n’utilise pas [d’autorisation](xref:security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="4845a-156">The template-generated app doesn't use [authorization](xref:security/authorization/secure-data).</span></span> <span data-ttu-id="4845a-157">`app.UseAuthorization` est inclus pour s’assurer qu’il est ajouté dans le bon ordre si l’application ajoute une autorisation.</span><span class="sxs-lookup"><span data-stu-id="4845a-157">`app.UseAuthorization` is included to ensure it's added in the correct order should the app add authorization.</span></span> <span data-ttu-id="4845a-158">`UseRouting`, `UseAuthentication`, `UseAuthorization`et `UseEndpoints` doivent être appelés dans l’ordre indiqué dans le code précédent.</span><span class="sxs-lookup"><span data-stu-id="4845a-158">`UseRouting`, `UseAuthentication`, `UseAuthorization`, and `UseEndpoints` must be called in the order shown in the preceding code.</span></span>

<span data-ttu-id="4845a-159">Pour plus d’informations sur les `IdentityOptions` et les `Startup`, consultez <xref:Microsoft.AspNetCore.Identity.IdentityOptions> et démarrage de l' [application](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="4845a-159">For more information on `IdentityOptions` and `Startup`, see <xref:Microsoft.AspNetCore.Identity.IdentityOptions> and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="4845a-160">Registre de génération de modèles, connexion et déconnexion</span><span class="sxs-lookup"><span data-stu-id="4845a-160">Scaffold Register, Login, and LogOut</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4845a-161">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4845a-161">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4845a-162">Ajoutez les fichiers Register, login et LogOut.</span><span class="sxs-lookup"><span data-stu-id="4845a-162">Add the Register, Login, and LogOut files.</span></span> <span data-ttu-id="4845a-163">Suivez l' [identité de l’échafaudage dans un projet Razor avec des instructions d’autorisation](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) pour générer le code présenté dans cette section.</span><span class="sxs-lookup"><span data-stu-id="4845a-163">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4845a-164">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="4845a-164">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="4845a-165">Si vous avez créé le projet avec le nom **application Web 1**, exécutez les commandes suivantes.</span><span class="sxs-lookup"><span data-stu-id="4845a-165">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="4845a-166">Sinon, utilisez l’espace de noms correct pour l' `ApplicationDbContext`:</span><span class="sxs-lookup"><span data-stu-id="4845a-166">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="4845a-167">PowerShell utilise un point-virgule comme séparateur de commande.</span><span class="sxs-lookup"><span data-stu-id="4845a-167">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="4845a-168">Quand vous utilisez PowerShell, échappez les points-virgules dans la liste de fichiers ou placez la liste de fichiers entre guillemets doubles, comme le montre l’exemple précédent.</span><span class="sxs-lookup"><span data-stu-id="4845a-168">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

<span data-ttu-id="4845a-169">Pour plus d’informations sur la génération de modèles automatique d’identité, consultez identification de l' [échafaudage dans un projet Razor avec autorisation](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization).</span><span class="sxs-lookup"><span data-stu-id="4845a-169">For more information on scaffolding Identity, see [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization).</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="4845a-170">Examiner le registre</span><span class="sxs-lookup"><span data-stu-id="4845a-170">Examine Register</span></span>

<span data-ttu-id="4845a-171">Quand un utilisateur clique sur le lien **Register** , l’action `RegisterModel.OnPostAsync` est appelée.</span><span class="sxs-lookup"><span data-stu-id="4845a-171">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="4845a-172">L’utilisateur est créé par [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) sur l’objet `_userManager`.</span><span class="sxs-lookup"><span data-stu-id="4845a-172">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="4845a-173">`_userManager` est fournie par l’injection de dépendances) :</span><span class="sxs-lookup"><span data-stu-id="4845a-173">`_userManager` is provided by dependency injection):</span></span>

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=9)]

<span data-ttu-id="4845a-174">Si l’utilisateur a été créé avec succès, l’utilisateur est connecté par l’appel à `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="4845a-174">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

<span data-ttu-id="4845a-175">Consultez [confirmation du compte](xref:security/authentication/accconfirm#prevent-login-at-registration) pour connaître les étapes permettant d’empêcher la connexion immédiate lors de l’inscription.</span><span class="sxs-lookup"><span data-stu-id="4845a-175">See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="4845a-176">Log in</span><span class="sxs-lookup"><span data-stu-id="4845a-176">Log in</span></span>

<span data-ttu-id="4845a-177">Le formulaire de connexion s’affiche lorsque :</span><span class="sxs-lookup"><span data-stu-id="4845a-177">The Login form is displayed when:</span></span>

* <span data-ttu-id="4845a-178">Le lien **connexion** est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="4845a-178">The **Log in** link is selected.</span></span>
* <span data-ttu-id="4845a-179">Un utilisateur tente d’accéder à une page restreinte à laquelle il n’est pas autorisé à accéder **ou** lorsqu’il n’a pas été authentifié par le système.</span><span class="sxs-lookup"><span data-stu-id="4845a-179">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="4845a-180">Lorsque le formulaire de la page de connexion est envoyé, l’action `OnPostAsync` est appelée.</span><span class="sxs-lookup"><span data-stu-id="4845a-180">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="4845a-181">`PasswordSignInAsync` est appelé sur l’objet `_signInManager` (fourni par l’injection de dépendances).</span><span class="sxs-lookup"><span data-stu-id="4845a-181">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

<span data-ttu-id="4845a-182">La classe de `Controller` de base expose une propriété `User` qui est accessible à partir des méthodes de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="4845a-182">The base `Controller` class exposes a `User` property that can be accessed from controller methods.</span></span> <span data-ttu-id="4845a-183">Par exemple, vous pouvez énumérer `User.Claims` et prendre des décisions d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="4845a-183">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="4845a-184">Pour plus d'informations, consultez <xref:security/authorization/introduction>.</span><span class="sxs-lookup"><span data-stu-id="4845a-184">For more information, see <xref:security/authorization/introduction>.</span></span>

### <a name="log-out"></a><span data-ttu-id="4845a-185">Déconnexion</span><span class="sxs-lookup"><span data-stu-id="4845a-185">Log out</span></span>

<span data-ttu-id="4845a-186">Le lien **déconnexion** appelle l’action `LogoutModel.OnPost`.</span><span class="sxs-lookup"><span data-stu-id="4845a-186">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Logout.cshtml.cs?highlight=36)]

<span data-ttu-id="4845a-187">Dans le code précédent, le code `return RedirectToPage();` doit être une redirection afin que le navigateur exécute une nouvelle demande et que l’identité de l’utilisateur soit mise à jour.</span><span class="sxs-lookup"><span data-stu-id="4845a-187">In the preceding code, the code `return RedirectToPage();` needs to be a redirect so that the browser performs a new request and the identity for the user gets updated.</span></span>

<span data-ttu-id="4845a-188">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) efface les revendications de l’utilisateur stockées dans un cookie.</span><span class="sxs-lookup"><span data-stu-id="4845a-188">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="4845a-189">La publication est spécifiée dans *pages/Shared/_LoginPartial. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="4845a-189">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp3/Pages/Shared/_LoginPartial.cshtml?highlight=15)]

## <a name="test-identity"></a><span data-ttu-id="4845a-190">Identité du test</span><span class="sxs-lookup"><span data-stu-id="4845a-190">Test Identity</span></span>

<span data-ttu-id="4845a-191">Les modèles de projet Web par défaut autorisent l’accès anonyme aux pages d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="4845a-191">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="4845a-192">Pour tester l’identité, ajoutez [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute):</span><span class="sxs-lookup"><span data-stu-id="4845a-192">To test Identity, add [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute):</span></span>

[!code-csharp[](identity/sample/WebApp3/Pages/Privacy.cshtml.cs?highlight=7)]

<span data-ttu-id="4845a-193">Si vous êtes connecté, déconnectez-vous. Exécutez l’application et sélectionnez le lien **confidentialité** .</span><span class="sxs-lookup"><span data-stu-id="4845a-193">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="4845a-194">Vous êtes redirigé vers la page de connexion.</span><span class="sxs-lookup"><span data-stu-id="4845a-194">You are redirected to the login page.</span></span>

### <a name="explore-identity"></a><span data-ttu-id="4845a-195">Explorer l’identité</span><span class="sxs-lookup"><span data-stu-id="4845a-195">Explore Identity</span></span>

<span data-ttu-id="4845a-196">Pour explorer l’identité plus en détail :</span><span class="sxs-lookup"><span data-stu-id="4845a-196">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="4845a-197">Créer une source d’interface utilisateur d’identité complète</span><span class="sxs-lookup"><span data-stu-id="4845a-197">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="4845a-198">Examinez la source de chaque page et parcourez le débogueur.</span><span class="sxs-lookup"><span data-stu-id="4845a-198">Examine the source of each page and step through the debugger.</span></span>

## <a name="identity-components"></a><span data-ttu-id="4845a-199">Composants d'Identity</span><span class="sxs-lookup"><span data-stu-id="4845a-199">Identity Components</span></span>

<span data-ttu-id="4845a-200">Tous les packages NuGet dépendants de l’identité sont inclus dans le [ASP.net Core Framework partagé](xref:aspnetcore-3.0#use-the-aspnet-core-shared-framework).</span><span class="sxs-lookup"><span data-stu-id="4845a-200">All the Identity-dependent NuGet packages are included in the [ASP.NET Core shared framework](xref:aspnetcore-3.0#use-the-aspnet-core-shared-framework).</span></span>

<span data-ttu-id="4845a-201">Le package principal pour l’identité est [Microsoft. AspNetCore. Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="4845a-201">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="4845a-202">Ce package contient l’ensemble principal d’interfaces pour ASP.NET Core Identity et est inclus par `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="4845a-202">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="4845a-203">Migration vers ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="4845a-203">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="4845a-204">Pour plus d’informations et de conseils sur la migration de votre magasin d’identités existant, consultez [migrer l’authentification et l’identité](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="4845a-204">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="4845a-205">Définition de la force du mot de passe</span><span class="sxs-lookup"><span data-stu-id="4845a-205">Setting password strength</span></span>

<span data-ttu-id="4845a-206">Consultez [configuration](#pw) d’un exemple qui définit la configuration minimale requise pour le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="4845a-206">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="4845a-207">AddDefaultIdentity et AddIdentity</span><span class="sxs-lookup"><span data-stu-id="4845a-207">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="4845a-208"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> a été introduite dans ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="4845a-208"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="4845a-209">L’appel de `AddDefaultIdentity` est semblable à l’appel de ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="4845a-209">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentity*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderExtensions.AddDefaultTokenProviders*>

<span data-ttu-id="4845a-210">Pour plus d’informations, consultez [source AddDefaultIdentity](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) .</span><span class="sxs-lookup"><span data-stu-id="4845a-210">See [AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4845a-211">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="4845a-211">Next Steps</span></span>

* [<span data-ttu-id="4845a-212">Configurer Identity</span><span class="sxs-lookup"><span data-stu-id="4845a-212">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="4845a-213">De [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4845a-213">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4845a-214">ASP.NET Core identité est un système d’appartenance qui ajoute des fonctionnalités de connexion aux applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4845a-214">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="4845a-215">Les utilisateurs peuvent créer un compte avec les informations de connexion stockées dans l’identité, ou ils peuvent utiliser un fournisseur de connexion externe.</span><span class="sxs-lookup"><span data-stu-id="4845a-215">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="4845a-216">Les fournisseurs de connexion externes pris en charge incluent [Facebook, Google, Microsoft Account et Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="4845a-216">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="4845a-217">L’identité peut être configurée à l’aide d’une base de données SQL Server pour stocker les noms d’utilisateur, les mots de passe et les données de profil.</span><span class="sxs-lookup"><span data-stu-id="4845a-217">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="4845a-218">Vous pouvez également utiliser un autre magasin persistant, par exemple, le stockage table Azure.</span><span class="sxs-lookup"><span data-stu-id="4845a-218">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="4845a-219">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([procédure de téléchargement)](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="4845a-219">[View or download the sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="4845a-220">Dans cette rubrique, vous allez apprendre à utiliser l’identité pour vous inscrire, vous connecter et déconnecter un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="4845a-220">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="4845a-221">Pour obtenir des instructions plus détaillées sur la création d’applications qui utilisent l’identité, consultez la section étapes suivantes à la fin de cet article.</span><span class="sxs-lookup"><span data-stu-id="4845a-221">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

<a name="adi"></a>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="4845a-222">AddDefaultIdentity et AddIdentity</span><span class="sxs-lookup"><span data-stu-id="4845a-222">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="4845a-223"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> a été introduite dans ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="4845a-223"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="4845a-224">L’appel de `AddDefaultIdentity` est semblable à l’appel de ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="4845a-224">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentity*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderExtensions.AddDefaultTokenProviders*>

<span data-ttu-id="4845a-225">Pour plus d’informations, consultez [source AddDefaultIdentity](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) .</span><span class="sxs-lookup"><span data-stu-id="4845a-225">See [AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="4845a-226">Créer une application Web avec l’authentification</span><span class="sxs-lookup"><span data-stu-id="4845a-226">Create a Web app with authentication</span></span>

<span data-ttu-id="4845a-227">Créez un projet d’application Web ASP.NET Core avec des comptes d’utilisateur individuels.</span><span class="sxs-lookup"><span data-stu-id="4845a-227">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4845a-228">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4845a-228">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4845a-229">Sélectionnez **Fichier** > **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="4845a-229">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="4845a-230">Sélectionnez **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="4845a-230">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="4845a-231">Nommez le projet **application Web 1** pour avoir le même espace de noms que le projet à télécharger.</span><span class="sxs-lookup"><span data-stu-id="4845a-231">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="4845a-232">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="4845a-232">Click **OK**.</span></span>
* <span data-ttu-id="4845a-233">Sélectionnez une **application Web**ASP.net Core, puis sélectionnez **modifier l’authentification**.</span><span class="sxs-lookup"><span data-stu-id="4845a-233">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="4845a-234">Sélectionnez **comptes d’utilisateur individuels** , puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="4845a-234">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4845a-235">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="4845a-235">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="4845a-236">Le projet généré fournit une [identité de ASP.net Core](xref:security/authentication/identity) sous forme de bibliothèque de [classes Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="4845a-236">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="4845a-237">La bibliothèque de classes Razor d’identité expose des points de terminaison avec la zone de `Identity`.</span><span class="sxs-lookup"><span data-stu-id="4845a-237">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="4845a-238">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="4845a-238">For example:</span></span>

* <span data-ttu-id="4845a-239">/Identity/Account/Login</span><span class="sxs-lookup"><span data-stu-id="4845a-239">/Identity/Account/Login</span></span>
* <span data-ttu-id="4845a-240">/Identity/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="4845a-240">/Identity/Account/Logout</span></span>
* <span data-ttu-id="4845a-241">/Identity/Account/Manage</span><span class="sxs-lookup"><span data-stu-id="4845a-241">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="4845a-242">Appliquer des migrations</span><span class="sxs-lookup"><span data-stu-id="4845a-242">Apply migrations</span></span>

<span data-ttu-id="4845a-243">Appliquez les migrations pour initialiser la base de données.</span><span class="sxs-lookup"><span data-stu-id="4845a-243">Apply the migrations to initialize the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4845a-244">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4845a-244">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4845a-245">Exécutez la commande suivante dans la console du gestionnaire de package (PMC) :</span><span class="sxs-lookup"><span data-stu-id="4845a-245">Run the following command in the Package Manager Console (PMC):</span></span>

```PM> Update-Database```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4845a-246">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="4845a-246">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="4845a-247">Registre de test et connexion</span><span class="sxs-lookup"><span data-stu-id="4845a-247">Test Register and Login</span></span>

<span data-ttu-id="4845a-248">Exécutez l’application et inscrivez un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="4845a-248">Run the app and register a user.</span></span> <span data-ttu-id="4845a-249">Selon la taille de votre écran, vous devrez peut-être sélectionner le bouton bascule de navigation pour afficher les liens de **connexion** et de **Registre** .</span><span class="sxs-lookup"><span data-stu-id="4845a-249">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="4845a-250">Configurer les services d’identité</span><span class="sxs-lookup"><span data-stu-id="4845a-250">Configure Identity services</span></span>

<span data-ttu-id="4845a-251">Les services sont ajoutés dans `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="4845a-251">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="4845a-252">Le modèle par défaut consiste à appeler toutes les méthodes `Add{Service}`, puis toutes les méthodes `services.Configure{Service}`.</span><span class="sxs-lookup"><span data-stu-id="4845a-252">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="4845a-253">Le code précédent configure l’identité avec les valeurs d’option par défaut.</span><span class="sxs-lookup"><span data-stu-id="4845a-253">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="4845a-254">Les services sont mis à la disposition de l’application via l' [injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="4845a-254">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="4845a-255">L’identité est activée en appelant [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="4845a-255">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="4845a-256">`UseAuthentication`Ajoute l’authentification [intergiciel (middleware)](xref:fundamentals/middleware/index) au pipeline de demande.</span><span class="sxs-lookup"><span data-stu-id="4845a-256">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

<span data-ttu-id="4845a-257">Pour plus d’informations, consultez la [classe IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) et le démarrage de l' [application](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="4845a-257">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="4845a-258">Registre de génération de modèles, connexion et déconnexion</span><span class="sxs-lookup"><span data-stu-id="4845a-258">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="4845a-259">Suivez l' [identité de l’échafaudage dans un projet Razor avec des instructions d’autorisation](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) pour générer le code présenté dans cette section.</span><span class="sxs-lookup"><span data-stu-id="4845a-259">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4845a-260">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4845a-260">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4845a-261">Ajoutez les fichiers Register, login et LogOut.</span><span class="sxs-lookup"><span data-stu-id="4845a-261">Add the Register, Login, and LogOut files.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4845a-262">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="4845a-262">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="4845a-263">Si vous avez créé le projet avec le nom **application Web 1**, exécutez les commandes suivantes.</span><span class="sxs-lookup"><span data-stu-id="4845a-263">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="4845a-264">Sinon, utilisez l’espace de noms correct pour l' `ApplicationDbContext`:</span><span class="sxs-lookup"><span data-stu-id="4845a-264">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="4845a-265">PowerShell utilise un point-virgule comme séparateur de commande.</span><span class="sxs-lookup"><span data-stu-id="4845a-265">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="4845a-266">Quand vous utilisez PowerShell, échappez les points-virgules dans la liste de fichiers ou placez la liste de fichiers entre guillemets doubles, comme le montre l’exemple précédent.</span><span class="sxs-lookup"><span data-stu-id="4845a-266">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="4845a-267">Examiner le registre</span><span class="sxs-lookup"><span data-stu-id="4845a-267">Examine Register</span></span>

<span data-ttu-id="4845a-268">Quand un utilisateur clique sur le lien **Register** , l’action `RegisterModel.OnPostAsync` est appelée.</span><span class="sxs-lookup"><span data-stu-id="4845a-268">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="4845a-269">L’utilisateur est créé par [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) sur l’objet `_userManager`.</span><span class="sxs-lookup"><span data-stu-id="4845a-269">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="4845a-270">`_userManager` est fournie par l’injection de dépendances) :</span><span class="sxs-lookup"><span data-stu-id="4845a-270">`_userManager` is provided by dependency injection):</span></span>

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7)]

<span data-ttu-id="4845a-271">Si l’utilisateur a été créé avec succès, l’utilisateur est connecté par l’appel à `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="4845a-271">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

<span data-ttu-id="4845a-272">**Remarque :** Consultez [confirmation du compte](xref:security/authentication/accconfirm#prevent-login-at-registration) pour connaître les étapes permettant d’empêcher la connexion immédiate lors de l’inscription.</span><span class="sxs-lookup"><span data-stu-id="4845a-272">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="4845a-273">Log in</span><span class="sxs-lookup"><span data-stu-id="4845a-273">Log in</span></span>

<span data-ttu-id="4845a-274">Le formulaire de connexion s’affiche lorsque :</span><span class="sxs-lookup"><span data-stu-id="4845a-274">The Login form is displayed when:</span></span>

* <span data-ttu-id="4845a-275">Le lien **connexion** est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="4845a-275">The **Log in** link is selected.</span></span>
* <span data-ttu-id="4845a-276">Un utilisateur tente d’accéder à une page restreinte à laquelle il n’est pas autorisé à accéder **ou** lorsqu’il n’a pas été authentifié par le système.</span><span class="sxs-lookup"><span data-stu-id="4845a-276">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="4845a-277">Lorsque le formulaire de la page de connexion est envoyé, l’action `OnPostAsync` est appelée.</span><span class="sxs-lookup"><span data-stu-id="4845a-277">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="4845a-278">`PasswordSignInAsync` est appelé sur l’objet `_signInManager` (fourni par l’injection de dépendances).</span><span class="sxs-lookup"><span data-stu-id="4845a-278">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

<span data-ttu-id="4845a-279">La classe de `Controller` de base expose une propriété `User` à laquelle vous pouvez accéder à partir de méthodes de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="4845a-279">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="4845a-280">Par exemple, vous pouvez énumérer `User.Claims` et prendre des décisions d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="4845a-280">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="4845a-281">Pour plus d'informations, consultez <xref:security/authorization/introduction>.</span><span class="sxs-lookup"><span data-stu-id="4845a-281">For more information, see <xref:security/authorization/introduction>.</span></span>

### <a name="log-out"></a><span data-ttu-id="4845a-282">Déconnexion</span><span class="sxs-lookup"><span data-stu-id="4845a-282">Log out</span></span>

<span data-ttu-id="4845a-283">Le lien **déconnexion** appelle l’action `LogoutModel.OnPost`.</span><span class="sxs-lookup"><span data-stu-id="4845a-283">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

<span data-ttu-id="4845a-284">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) efface les revendications de l’utilisateur stockées dans un cookie.</span><span class="sxs-lookup"><span data-stu-id="4845a-284">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="4845a-285">La publication est spécifiée dans *pages/Shared/_LoginPartial. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="4845a-285">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

## <a name="test-identity"></a><span data-ttu-id="4845a-286">Identité du test</span><span class="sxs-lookup"><span data-stu-id="4845a-286">Test Identity</span></span>

<span data-ttu-id="4845a-287">Les modèles de projet Web par défaut autorisent l’accès anonyme aux pages d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="4845a-287">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="4845a-288">Pour tester l’identité, ajoutez [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) à la page confidentialité.</span><span class="sxs-lookup"><span data-stu-id="4845a-288">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the Privacy page.</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=7)]

<span data-ttu-id="4845a-289">Si vous êtes connecté, déconnectez-vous. Exécutez l’application et sélectionnez le lien **confidentialité** .</span><span class="sxs-lookup"><span data-stu-id="4845a-289">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="4845a-290">Vous êtes redirigé vers la page de connexion.</span><span class="sxs-lookup"><span data-stu-id="4845a-290">You are redirected to the login page.</span></span>

### <a name="explore-identity"></a><span data-ttu-id="4845a-291">Explorer l’identité</span><span class="sxs-lookup"><span data-stu-id="4845a-291">Explore Identity</span></span>

<span data-ttu-id="4845a-292">Pour explorer l’identité plus en détail :</span><span class="sxs-lookup"><span data-stu-id="4845a-292">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="4845a-293">Créer une source d’interface utilisateur d’identité complète</span><span class="sxs-lookup"><span data-stu-id="4845a-293">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="4845a-294">Examinez la source de chaque page et parcourez le débogueur.</span><span class="sxs-lookup"><span data-stu-id="4845a-294">Examine the source of each page and step through the debugger.</span></span>

## <a name="identity-components"></a><span data-ttu-id="4845a-295">Composants d'Identity</span><span class="sxs-lookup"><span data-stu-id="4845a-295">Identity Components</span></span>

<span data-ttu-id="4845a-296">Tous les packages NuGet dépendants de l’identité sont inclus dans le sous- [package Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="4845a-296">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="4845a-297">Le package principal pour l’identité est [Microsoft. AspNetCore. Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="4845a-297">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="4845a-298">Ce package contient l’ensemble principal d’interfaces pour ASP.NET Core Identity et est inclus par `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="4845a-298">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="4845a-299">Migration vers ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="4845a-299">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="4845a-300">Pour plus d’informations et de conseils sur la migration de votre magasin d’identités existant, consultez [migrer l’authentification et l’identité](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="4845a-300">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="4845a-301">Définition de la force du mot de passe</span><span class="sxs-lookup"><span data-stu-id="4845a-301">Setting password strength</span></span>

<span data-ttu-id="4845a-302">Consultez [configuration](#pw) d’un exemple qui définit la configuration minimale requise pour le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="4845a-302">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4845a-303">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="4845a-303">Next Steps</span></span>

* [<span data-ttu-id="4845a-304">Configurer Identity</span><span class="sxs-lookup"><span data-stu-id="4845a-304">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>

::: moniker-end
