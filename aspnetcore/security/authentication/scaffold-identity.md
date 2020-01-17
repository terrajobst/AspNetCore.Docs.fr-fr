---
title: Identité de l’échafaudage dans les projets ASP.NET Core
author: rick-anderson
description: Découvrez comment générer une structure d’identité dans un projet ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/15/2020
uid: security/authentication/scaffold-identity
ms.openlocfilehash: a0e9603cbca8c7f5771b0acf1a60839dffc89d4e
ms.sourcegitcommit: cbd30479f42cbb3385000ef834d9c7d021fd218d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76146483"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="dc28f-103">Identité de l’échafaudage dans les projets ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dc28f-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="dc28f-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="dc28f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="dc28f-105">ASP.NET Core fournit [ASP.net Core identité](xref:security/authentication/identity) sous forme de [bibliothèque de classes Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="dc28f-105">ASP.NET Core provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="dc28f-106">Les applications qui incluent une identité peuvent appliquer l’échafaudage pour ajouter de manière sélective le code source contenu dans la bibliothèque de classes Razor d’identité (RCL).</span><span class="sxs-lookup"><span data-stu-id="dc28f-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="dc28f-107">Vous pouvez souhaiter générer le code source afin de pouvoir modifier le code et changer le comportement.</span><span class="sxs-lookup"><span data-stu-id="dc28f-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="dc28f-108">Par exemple, vous pouvez demander au générateur de modèles automatique de générer le code utilisé dans l’inscription.</span><span class="sxs-lookup"><span data-stu-id="dc28f-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="dc28f-109">Le code généré est prioritaire sur le même code dans la bibliothèque de classes Razor d’identité.</span><span class="sxs-lookup"><span data-stu-id="dc28f-109">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="dc28f-110">Pour obtenir le contrôle total de l’interface utilisateur et ne pas utiliser le RCL par défaut, consultez la section [créer une source d’interface utilisateur d’identité complète](#full).</span><span class="sxs-lookup"><span data-stu-id="dc28f-110">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="dc28f-111">Les applications qui n’incluent **pas** l’authentification peuvent appliquer l’échafaudage pour ajouter le package d’identité RCL.</span><span class="sxs-lookup"><span data-stu-id="dc28f-111">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="dc28f-112">Vous pouvez sélectionner le code d’identité à générer.</span><span class="sxs-lookup"><span data-stu-id="dc28f-112">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="dc28f-113">Bien que le générateur de modèles génère la majeure partie du code nécessaire, vous devez mettre à jour votre projet pour terminer le processus.</span><span class="sxs-lookup"><span data-stu-id="dc28f-113">Although the scaffolder generates most of the necessary code, you need to update your project to complete the process.</span></span> <span data-ttu-id="dc28f-114">Ce document explique les étapes nécessaires pour effectuer une mise à jour de l’échafaudage d’identité.</span><span class="sxs-lookup"><span data-stu-id="dc28f-114">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="dc28f-115">Nous vous recommandons d’utiliser un système de contrôle de code source qui affiche les différences entre les fichiers et vous permet d’annuler les modifications.</span><span class="sxs-lookup"><span data-stu-id="dc28f-115">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="dc28f-116">Inspectez les modifications après avoir exécuté l’échafaudage d’identité.</span><span class="sxs-lookup"><span data-stu-id="dc28f-116">Inspect the changes after running the Identity scaffolder.</span></span>

<span data-ttu-id="dc28f-117">Les services sont requis lors de l’utilisation de [l’authentification à deux facteurs](xref:security/authentication/identity-enable-qrcodes), la [confirmation de compte et la récupération de mot de passe](xref:security/authentication/accconfirm), ainsi que d’autres fonctionnalités de sécurité avec identité.</span><span class="sxs-lookup"><span data-stu-id="dc28f-117">Services are required when using [Two Factor Authentication](xref:security/authentication/identity-enable-qrcodes), [Account confirmation and password recovery](xref:security/authentication/accconfirm), and other security features with Identity.</span></span> <span data-ttu-id="dc28f-118">Les services ou stubs de service ne sont pas générés lors de la génération de modèles automatique d’identité.</span><span class="sxs-lookup"><span data-stu-id="dc28f-118">Services or service stubs aren't generated when scaffolding Identity.</span></span> <span data-ttu-id="dc28f-119">Les services pour activer ces fonctionnalités doivent être ajoutés manuellement.</span><span class="sxs-lookup"><span data-stu-id="dc28f-119">Services to enable these features must be added manually.</span></span> <span data-ttu-id="dc28f-120">Par exemple, consultez [exiger une confirmation par courrier électronique](xref:security/authentication/accconfirm#require-email-confirmation).</span><span class="sxs-lookup"><span data-stu-id="dc28f-120">For example, see [Require Email Confirmation](xref:security/authentication/accconfirm#require-email-confirmation).</span></span>

<span data-ttu-id="dc28f-121">Ce document contient des instructions plus complètes que le fichier *ScaffoldingReadme. txt* qui est généré lors de l’exécution du générateur de modèles.</span><span class="sxs-lookup"><span data-stu-id="dc28f-121">This document contains more complete instructions than the *ScaffoldingReadme.txt* file which is generated when running the the scaffolder.</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="dc28f-122">Identité de l’échafaudage dans un projet vide</span><span class="sxs-lookup"><span data-stu-id="dc28f-122">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="dc28f-123">Mettez à jour la classe `Startup` avec un code similaire à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="dc28f-123">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupMVC.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="dc28f-124">Identité de l’échafaudage dans un projet Razor sans autorisation existante</span><span class="sxs-lookup"><span data-stu-id="dc28f-124">Scaffold identity into a Razor project without existing authorization</span></span>

<!--  Updated for 3.0
set projNam=RPnoAuth
set projType=webapp

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Identity.UI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef database drop
dotnet ef migrations add CreateIdentitySchema0
dotnet ef database update
-->

<!-- ERROR
There is already an object named 'AspNetRoles' in the database.

Fixed via dotnet ef database drop
before dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="dc28f-125">L’identité est configurée dans *Areas/Identity/IdentityHostingStartup. cs*.</span><span class="sxs-lookup"><span data-stu-id="dc28f-125">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="dc28f-126">Pour plus d’informations, consultez [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="dc28f-126">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="dc28f-127">Migrations, UseAuthentication et disposition</span><span class="sxs-lookup"><span data-stu-id="dc28f-127">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a><span data-ttu-id="dc28f-128">Activez l’authentification</span><span class="sxs-lookup"><span data-stu-id="dc28f-128">Enable authentication</span></span>

<span data-ttu-id="dc28f-129">Mettez à jour la classe `Startup` avec un code similaire à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="dc28f-129">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupRP.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="dc28f-130">Modifications de la disposition</span><span class="sxs-lookup"><span data-stu-id="dc28f-130">Layout changes</span></span>

<span data-ttu-id="dc28f-131">Facultatif : ajoutez la connexion partielle (`_LoginPartial`) au fichier de disposition :</span><span class="sxs-lookup"><span data-stu-id="dc28f-131">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/3.1sample/_Layout.cshtml?highlight=20)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="dc28f-132">Identité de l’échafaudage dans un projet Razor avec autorisation</span><span class="sxs-lookup"><span data-stu-id="dc28f-132">Scaffold identity into a Razor project with authorization</span></span>

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
<span data-ttu-id="dc28f-133">Certaines options d’identité sont configurées dans *Areas/Identity/IdentityHostingStartup. cs*.</span><span class="sxs-lookup"><span data-stu-id="dc28f-133">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="dc28f-134">Pour plus d’informations, consultez [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="dc28f-134">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="dc28f-135">Identité de l’échafaudage dans un projet MVC sans autorisation existante</span><span class="sxs-lookup"><span data-stu-id="dc28f-135">Scaffold identity into an MVC project without existing authorization</span></span>

<!--
set projNam=MvcNoAuth
set projType=mvc
set version=2.1.0

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="dc28f-136">Facultatif : ajoutez la connexion partielle (`_LoginPartial`) au fichier *Views/Shared/_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="dc28f-136">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[Main](scaffold-identity/3.1sample/_Layout.cshtml?highlight=20)]

* <span data-ttu-id="dc28f-137">Déplacez le fichier *pages/Shared/_LoginPartial. cshtml* vers *views/shared/_LoginPartial. cshtml*</span><span class="sxs-lookup"><span data-stu-id="dc28f-137">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="dc28f-138">L’identité est configurée dans *Areas/Identity/IdentityHostingStartup. cs*.</span><span class="sxs-lookup"><span data-stu-id="dc28f-138">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="dc28f-139">Pour plus d’informations, consultez IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="dc28f-139">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="dc28f-140">Mettez à jour la classe `Startup` avec un code similaire à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="dc28f-140">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupMVC.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="dc28f-141">Identité de l’échafaudage dans un projet MVC avec autorisation</span><span class="sxs-lookup"><span data-stu-id="dc28f-141">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext  --files "Account.Login;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="dc28f-142">Créer une source d’interface utilisateur d’identité complète</span><span class="sxs-lookup"><span data-stu-id="dc28f-142">Create full identity UI source</span></span>

<span data-ttu-id="dc28f-143">Pour conserver le contrôle total de l’interface utilisateur de l’identité, exécutez le générateur de modèles d’identité et sélectionnez **remplacer tous les fichiers**.</span><span class="sxs-lookup"><span data-stu-id="dc28f-143">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="dc28f-144">Le code en surbrillance suivant montre les modifications pour remplacer l’interface utilisateur d’identité par défaut par l’identité dans une application Web ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="dc28f-144">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="dc28f-145">Vous pouvez le faire pour avoir le contrôle total de l’interface utilisateur de l’identité.</span><span class="sxs-lookup"><span data-stu-id="dc28f-145">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="dc28f-146">L’identité par défaut est remplacée dans le code suivant :</span><span class="sxs-lookup"><span data-stu-id="dc28f-146">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="dc28f-147">Le code suivant définit les [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)et [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span><span class="sxs-lookup"><span data-stu-id="dc28f-147">The following code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="dc28f-148">Inscrire une implémentation de `IEmailSender`, par exemple :</span><span class="sxs-lookup"><span data-stu-id="dc28f-148">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

<!--
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
-->
## <a name="disable-register-page"></a><span data-ttu-id="dc28f-149">Désactiver la page de Registre</span><span class="sxs-lookup"><span data-stu-id="dc28f-149">Disable register page</span></span>

<span data-ttu-id="dc28f-150">Pour désactiver l’inscription des utilisateurs :</span><span class="sxs-lookup"><span data-stu-id="dc28f-150">To disable user registration:</span></span>

* <span data-ttu-id="dc28f-151">Identité de l’échafaudage.</span><span class="sxs-lookup"><span data-stu-id="dc28f-151">Scaffold Identity.</span></span> <span data-ttu-id="dc28f-152">Incluez Account. Register, Account. Login et Account. RegisterConfirmation.</span><span class="sxs-lookup"><span data-stu-id="dc28f-152">Include Account.Register, Account.Login, and Account.RegisterConfirmation.</span></span> <span data-ttu-id="dc28f-153">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="dc28f-153">For example:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
  ```

* <span data-ttu-id="dc28f-154">Mettez à jour *Areas/Identity/pages/Account/Register. cshtml. cs* afin que les utilisateurs ne puissent pas s’inscrire à partir de ce point de terminaison :</span><span class="sxs-lookup"><span data-stu-id="dc28f-154">Update *Areas/Identity/Pages/Account/Register.cshtml.cs* so users can't register from this endpoint:</span></span>

  [!code-csharp[](scaffold-identity/sample/Register.cshtml.cs?name=snippet)]

* <span data-ttu-id="dc28f-155">Mettez à jour *Areas/Identity/pages/Account/Register. cshtml* pour qu’il soit cohérent avec les modifications précédentes :</span><span class="sxs-lookup"><span data-stu-id="dc28f-155">Update *Areas/Identity/Pages/Account/Register.cshtml* to be consistent with the preceding changes:</span></span>

  [!code-cshtml[](scaffold-identity/sample/Register.cshtml)]

* <span data-ttu-id="dc28f-156">Commentez ou supprimez le lien d’inscription de *Areas/Identity/pages/Account/login. cshtml*</span><span class="sxs-lookup"><span data-stu-id="dc28f-156">Comment out or remove the registration link from *Areas/Identity/Pages/Account/Login.cshtml*</span></span>

```cshtml
@*
<p>
    <a asp-page="./Register" asp-route-returnUrl="@Model.ReturnUrl">Register as a new user</a>
</p>
*@
```

* <span data-ttu-id="dc28f-157">Mettez à jour la page *Areas/Identity/pages/Account/RegisterConfirmation* .</span><span class="sxs-lookup"><span data-stu-id="dc28f-157">Update the *Areas/Identity/Pages/Account/RegisterConfirmation* page.</span></span>

  * <span data-ttu-id="dc28f-158">Supprimez le code et les liens du fichier cshtml.</span><span class="sxs-lookup"><span data-stu-id="dc28f-158">Remove the code and links from the cshtml file.</span></span>
  * <span data-ttu-id="dc28f-159">Supprimez le code de confirmation du `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="dc28f-159">Remove the confirmation code from the `PageModel`:</span></span>

  ```csharp
   [AllowAnonymous]
    public class RegisterConfirmationModel : PageModel
    {
        public IActionResult OnGet()
        {  
            return Page();
        }
    }
  ```
  
### <a name="use-another-app-to-add-users"></a><span data-ttu-id="dc28f-160">Utiliser une autre application pour ajouter des utilisateurs</span><span class="sxs-lookup"><span data-stu-id="dc28f-160">Use another app to add users</span></span>

<span data-ttu-id="dc28f-161">Fournissez un mécanisme pour ajouter des utilisateurs en dehors de l’application Web.</span><span class="sxs-lookup"><span data-stu-id="dc28f-161">Provide a mechanism to add users outside the web app.</span></span> <span data-ttu-id="dc28f-162">Les options permettant d’ajouter des utilisateurs sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="dc28f-162">Options to add users include:</span></span>

* <span data-ttu-id="dc28f-163">Une application Web d’administration dédiée.</span><span class="sxs-lookup"><span data-stu-id="dc28f-163">A dedicated admin web app.</span></span>
* <span data-ttu-id="dc28f-164">Application console.</span><span class="sxs-lookup"><span data-stu-id="dc28f-164">A console app.</span></span>

<span data-ttu-id="dc28f-165">Le code suivant décrit une approche de l’ajout d’utilisateurs :</span><span class="sxs-lookup"><span data-stu-id="dc28f-165">The following code outlines one approach to adding users:</span></span>

* <span data-ttu-id="dc28f-166">Une liste d’utilisateurs est lue en mémoire.</span><span class="sxs-lookup"><span data-stu-id="dc28f-166">A list of users is read into memory.</span></span>
* <span data-ttu-id="dc28f-167">Un mot de passe fort unique est généré pour chaque utilisateur.</span><span class="sxs-lookup"><span data-stu-id="dc28f-167">A strong unique password is generated for each user.</span></span>
* <span data-ttu-id="dc28f-168">L’utilisateur est ajouté à la base de données d’identité.</span><span class="sxs-lookup"><span data-stu-id="dc28f-168">The user is added to the Identity database.</span></span>
* <span data-ttu-id="dc28f-169">L’utilisateur est averti et a demandé à modifier le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="dc28f-169">The user is notified and told to change the password.</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Program.cs?name=snippet)]

<span data-ttu-id="dc28f-170">L’exemple de code suivant présente l’ajout d’un utilisateur :</span><span class="sxs-lookup"><span data-stu-id="dc28f-170">The following code outlines adding a user:</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Data/SeedData.cs?name=snippet)]

<span data-ttu-id="dc28f-171">Une approche similaire peut être suivie pour les scénarios de production.</span><span class="sxs-lookup"><span data-stu-id="dc28f-171">A similar approach can be followed for production scenarios.</span></span>

## <a name="prevent-publish-of-static-identity-assets"></a><span data-ttu-id="dc28f-172">Empêcher la publication de ressources d’identité statiques</span><span class="sxs-lookup"><span data-stu-id="dc28f-172">Prevent publish of static Identity assets</span></span>

<span data-ttu-id="dc28f-173">Pour empêcher la publication de ressources d’identité statiques sur la racine Web, consultez <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>.</span><span class="sxs-lookup"><span data-stu-id="dc28f-173">To prevent publishing static Identity assets to the web root, see <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dc28f-174">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="dc28f-174">Additional resources</span></span>

* [<span data-ttu-id="dc28f-175">Modifications apportées au code d’authentification pour ASP.NET Core 2,1 et versions ultérieures</span><span class="sxs-lookup"><span data-stu-id="dc28f-175">Changes to authentication code to ASP.NET Core 2.1 and later</span></span>](xref:migration/20_21#changes-to-authentication-code)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="dc28f-176">ASP.NET Core 2,1 et versions ultérieures fournissent [ASP.net Core identité](xref:security/authentication/identity) sous forme de [bibliothèque de classes Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="dc28f-176">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="dc28f-177">Les applications qui incluent une identité peuvent appliquer l’échafaudage pour ajouter de manière sélective le code source contenu dans la bibliothèque de classes Razor d’identité (RCL).</span><span class="sxs-lookup"><span data-stu-id="dc28f-177">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="dc28f-178">Vous pouvez souhaiter générer le code source afin de pouvoir modifier le code et changer le comportement.</span><span class="sxs-lookup"><span data-stu-id="dc28f-178">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="dc28f-179">Par exemple, vous pouvez demander au générateur de modèles automatique de générer le code utilisé dans l’inscription.</span><span class="sxs-lookup"><span data-stu-id="dc28f-179">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="dc28f-180">Le code généré est prioritaire sur le même code dans la bibliothèque de classes Razor d’identité.</span><span class="sxs-lookup"><span data-stu-id="dc28f-180">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="dc28f-181">Pour obtenir le contrôle total de l’interface utilisateur et ne pas utiliser le RCL par défaut, consultez la section [créer une source d’interface utilisateur d’identité complète](#full).</span><span class="sxs-lookup"><span data-stu-id="dc28f-181">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="dc28f-182">Les applications qui n’incluent **pas** l’authentification peuvent appliquer l’échafaudage pour ajouter le package d’identité RCL.</span><span class="sxs-lookup"><span data-stu-id="dc28f-182">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="dc28f-183">Vous pouvez sélectionner le code d’identité à générer.</span><span class="sxs-lookup"><span data-stu-id="dc28f-183">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="dc28f-184">Bien que le générateur de modèles génère la plus grande partie du code nécessaire, vous devez mettre à jour votre projet pour terminer le processus.</span><span class="sxs-lookup"><span data-stu-id="dc28f-184">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="dc28f-185">Ce document explique les étapes nécessaires pour effectuer une mise à jour de l’échafaudage d’identité.</span><span class="sxs-lookup"><span data-stu-id="dc28f-185">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="dc28f-186">Lorsque l’échafaudage d’identité est exécuté, un fichier *ScaffoldingReadme. txt* est créé dans le répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="dc28f-186">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="dc28f-187">Le fichier *ScaffoldingReadme. txt* contient des instructions générales sur ce qui est nécessaire pour effectuer la mise à jour de l’échafaudage d’identité.</span><span class="sxs-lookup"><span data-stu-id="dc28f-187">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="dc28f-188">Ce document contient des instructions plus complètes que le fichier *ScaffoldingReadme. txt* .</span><span class="sxs-lookup"><span data-stu-id="dc28f-188">This document contains more complete instructions than the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="dc28f-189">Nous vous recommandons d’utiliser un système de contrôle de code source qui affiche les différences entre les fichiers et vous permet d’annuler les modifications.</span><span class="sxs-lookup"><span data-stu-id="dc28f-189">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="dc28f-190">Inspectez les modifications après avoir exécuté l’échafaudage d’identité.</span><span class="sxs-lookup"><span data-stu-id="dc28f-190">Inspect the changes after running the Identity scaffolder.</span></span>

> [!NOTE]
> <span data-ttu-id="dc28f-191">Les services sont requis lors de l’utilisation de [l’authentification à deux facteurs](xref:security/authentication/identity-enable-qrcodes), la [confirmation de compte et la récupération de mot de passe](xref:security/authentication/accconfirm), ainsi que d’autres fonctionnalités de sécurité avec identité.</span><span class="sxs-lookup"><span data-stu-id="dc28f-191">Services are required when using [Two Factor Authentication](xref:security/authentication/identity-enable-qrcodes), [Account confirmation and password recovery](xref:security/authentication/accconfirm), and other security features with Identity.</span></span> <span data-ttu-id="dc28f-192">Les services ou stubs de service ne sont pas générés lors de la génération de modèles automatique d’identité.</span><span class="sxs-lookup"><span data-stu-id="dc28f-192">Services or service stubs aren't generated when scaffolding Identity.</span></span> <span data-ttu-id="dc28f-193">Les services pour activer ces fonctionnalités doivent être ajoutés manuellement.</span><span class="sxs-lookup"><span data-stu-id="dc28f-193">Services to enable these features must be added manually.</span></span> <span data-ttu-id="dc28f-194">Par exemple, consultez [exiger une confirmation par courrier électronique](xref:security/authentication/accconfirm#require-email-confirmation).</span><span class="sxs-lookup"><span data-stu-id="dc28f-194">For example, see [Require Email Confirmation](xref:security/authentication/accconfirm#require-email-confirmation).</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="dc28f-195">Identité de l’échafaudage dans un projet vide</span><span class="sxs-lookup"><span data-stu-id="dc28f-195">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="dc28f-196">Ajoutez les appels en surbrillance suivants à la classe `Startup` :</span><span class="sxs-lookup"><span data-stu-id="dc28f-196">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="dc28f-197">Identité de l’échafaudage dans un projet Razor sans autorisation existante</span><span class="sxs-lookup"><span data-stu-id="dc28f-197">Scaffold identity into a Razor project without existing authorization</span></span>

<!--  Updated for 3.0
set projNam=RPnoAuth
set projType=webapp

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Identity.UI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="dc28f-198">L’identité est configurée dans *Areas/Identity/IdentityHostingStartup. cs*.</span><span class="sxs-lookup"><span data-stu-id="dc28f-198">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="dc28f-199">Pour plus d’informations, consultez [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="dc28f-199">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="dc28f-200">Migrations, UseAuthentication et disposition</span><span class="sxs-lookup"><span data-stu-id="dc28f-200">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a><span data-ttu-id="dc28f-201">Activez l’authentification</span><span class="sxs-lookup"><span data-stu-id="dc28f-201">Enable authentication</span></span>

<span data-ttu-id="dc28f-202">Dans la méthode `Configure` de la classe `Startup`, appelez [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) après `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="dc28f-202">In the `Configure` method of the `Startup` class, call [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="dc28f-203">Modifications de la disposition</span><span class="sxs-lookup"><span data-stu-id="dc28f-203">Layout changes</span></span>

<span data-ttu-id="dc28f-204">Facultatif : ajoutez la connexion partielle (`_LoginPartial`) au fichier de disposition :</span><span class="sxs-lookup"><span data-stu-id="dc28f-204">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="dc28f-205">Identité de l’échafaudage dans un projet Razor avec autorisation</span><span class="sxs-lookup"><span data-stu-id="dc28f-205">Scaffold identity into a Razor project with authorization</span></span>

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
<span data-ttu-id="dc28f-206">Certaines options d’identité sont configurées dans *Areas/Identity/IdentityHostingStartup. cs*.</span><span class="sxs-lookup"><span data-stu-id="dc28f-206">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="dc28f-207">Pour plus d’informations, consultez [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="dc28f-207">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="dc28f-208">Identité de l’échafaudage dans un projet MVC sans autorisation existante</span><span class="sxs-lookup"><span data-stu-id="dc28f-208">Scaffold identity into an MVC project without existing authorization</span></span>

<!--
set projNam=MvcNoAuth
set projType=mvc
set version=2.1.0

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="dc28f-209">Facultatif : ajoutez la connexion partielle (`_LoginPartial`) au fichier *Views/Shared/_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="dc28f-209">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="dc28f-210">Déplacez le fichier *pages/Shared/_LoginPartial. cshtml* vers *views/shared/_LoginPartial. cshtml*</span><span class="sxs-lookup"><span data-stu-id="dc28f-210">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="dc28f-211">L’identité est configurée dans *Areas/Identity/IdentityHostingStartup. cs*.</span><span class="sxs-lookup"><span data-stu-id="dc28f-211">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="dc28f-212">Pour plus d’informations, consultez IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="dc28f-212">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="dc28f-213">Appelez [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) après `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="dc28f-213">Call [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="dc28f-214">Identité de l’échafaudage dans un projet MVC avec autorisation</span><span class="sxs-lookup"><span data-stu-id="dc28f-214">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext  --files "Account.Login;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="dc28f-215">Supprimez le dossier *pages/Shared* et les fichiers de ce dossier.</span><span class="sxs-lookup"><span data-stu-id="dc28f-215">Delete the *Pages/Shared* folder and the files in that folder.</span></span>

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="dc28f-216">Créer une source d’interface utilisateur d’identité complète</span><span class="sxs-lookup"><span data-stu-id="dc28f-216">Create full identity UI source</span></span>

<span data-ttu-id="dc28f-217">Pour conserver le contrôle total de l’interface utilisateur de l’identité, exécutez le générateur de modèles d’identité et sélectionnez **remplacer tous les fichiers**.</span><span class="sxs-lookup"><span data-stu-id="dc28f-217">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="dc28f-218">Le code en surbrillance suivant montre les modifications pour remplacer l’interface utilisateur d’identité par défaut par l’identité dans une application Web ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="dc28f-218">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="dc28f-219">Vous pouvez le faire pour avoir le contrôle total de l’interface utilisateur de l’identité.</span><span class="sxs-lookup"><span data-stu-id="dc28f-219">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="dc28f-220">L’identité par défaut est remplacée dans le code suivant :</span><span class="sxs-lookup"><span data-stu-id="dc28f-220">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="dc28f-221">Le code suivant définit les [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)et [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span><span class="sxs-lookup"><span data-stu-id="dc28f-221">The following code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="dc28f-222">Inscrire une implémentation de `IEmailSender`, par exemple :</span><span class="sxs-lookup"><span data-stu-id="dc28f-222">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

<!--
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
-->
## <a name="disable-register-page"></a><span data-ttu-id="dc28f-223">Désactiver la page de Registre</span><span class="sxs-lookup"><span data-stu-id="dc28f-223">Disable register page</span></span>

<span data-ttu-id="dc28f-224">Pour désactiver l’inscription des utilisateurs :</span><span class="sxs-lookup"><span data-stu-id="dc28f-224">To disable user registration:</span></span>

* <span data-ttu-id="dc28f-225">Identité de l’échafaudage.</span><span class="sxs-lookup"><span data-stu-id="dc28f-225">Scaffold Identity.</span></span> <span data-ttu-id="dc28f-226">Incluez Account. Register, Account. Login et Account. RegisterConfirmation.</span><span class="sxs-lookup"><span data-stu-id="dc28f-226">Include Account.Register, Account.Login, and Account.RegisterConfirmation.</span></span> <span data-ttu-id="dc28f-227">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="dc28f-227">For example:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
  ```

* <span data-ttu-id="dc28f-228">Mettez à jour *Areas/Identity/pages/Account/Register. cshtml. cs* afin que les utilisateurs ne puissent pas s’inscrire à partir de ce point de terminaison :</span><span class="sxs-lookup"><span data-stu-id="dc28f-228">Update *Areas/Identity/Pages/Account/Register.cshtml.cs* so users can't register from this endpoint:</span></span>

  [!code-csharp[](scaffold-identity/sample/Register.cshtml.cs?name=snippet)]

* <span data-ttu-id="dc28f-229">Mettez à jour *Areas/Identity/pages/Account/Register. cshtml* pour qu’il soit cohérent avec les modifications précédentes :</span><span class="sxs-lookup"><span data-stu-id="dc28f-229">Update *Areas/Identity/Pages/Account/Register.cshtml* to be consistent with the preceding changes:</span></span>

  [!code-cshtml[](scaffold-identity/sample/Register.cshtml)]

* <span data-ttu-id="dc28f-230">Commentez ou supprimez le lien d’inscription de *Areas/Identity/pages/Account/login. cshtml*</span><span class="sxs-lookup"><span data-stu-id="dc28f-230">Comment out or remove the registration link from *Areas/Identity/Pages/Account/Login.cshtml*</span></span>

```cshtml
@*
<p>
    <a asp-page="./Register" asp-route-returnUrl="@Model.ReturnUrl">Register as a new user</a>
</p>
*@
```

* <span data-ttu-id="dc28f-231">Mettez à jour la page *Areas/Identity/pages/Account/RegisterConfirmation* .</span><span class="sxs-lookup"><span data-stu-id="dc28f-231">Update the *Areas/Identity/Pages/Account/RegisterConfirmation* page.</span></span>

  * <span data-ttu-id="dc28f-232">Supprimez le code et les liens du fichier cshtml.</span><span class="sxs-lookup"><span data-stu-id="dc28f-232">Remove the code and links from the cshtml file.</span></span>
  * <span data-ttu-id="dc28f-233">Supprimez le code de confirmation du `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="dc28f-233">Remove the confirmation code from the `PageModel`:</span></span>

  ```csharp
   [AllowAnonymous]
    public class RegisterConfirmationModel : PageModel
    {
        public IActionResult OnGet()
        {  
            return Page();
        }
    }
  ```
  
### <a name="use-another-app-to-add-users"></a><span data-ttu-id="dc28f-234">Utiliser une autre application pour ajouter des utilisateurs</span><span class="sxs-lookup"><span data-stu-id="dc28f-234">Use another app to add users</span></span>

<span data-ttu-id="dc28f-235">Fournissez un mécanisme pour ajouter des utilisateurs en dehors de l’application Web.</span><span class="sxs-lookup"><span data-stu-id="dc28f-235">Provide a mechanism to add users outside the web app.</span></span> <span data-ttu-id="dc28f-236">Les options permettant d’ajouter des utilisateurs sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="dc28f-236">Options to add users include:</span></span>

* <span data-ttu-id="dc28f-237">Une application Web d’administration dédiée.</span><span class="sxs-lookup"><span data-stu-id="dc28f-237">A dedicated admin web app.</span></span>
* <span data-ttu-id="dc28f-238">Application console.</span><span class="sxs-lookup"><span data-stu-id="dc28f-238">A console app.</span></span>

<span data-ttu-id="dc28f-239">Le code suivant décrit une approche de l’ajout d’utilisateurs :</span><span class="sxs-lookup"><span data-stu-id="dc28f-239">The following code outlines one approach to adding users:</span></span>

* <span data-ttu-id="dc28f-240">Une liste d’utilisateurs est lue en mémoire.</span><span class="sxs-lookup"><span data-stu-id="dc28f-240">A list of users is read into memory.</span></span>
* <span data-ttu-id="dc28f-241">Un mot de passe fort unique est généré pour chaque utilisateur.</span><span class="sxs-lookup"><span data-stu-id="dc28f-241">A strong unique password is generated for each user.</span></span>
* <span data-ttu-id="dc28f-242">L’utilisateur est ajouté à la base de données d’identité.</span><span class="sxs-lookup"><span data-stu-id="dc28f-242">The user is added to the Identity database.</span></span>
* <span data-ttu-id="dc28f-243">L’utilisateur est averti et a demandé à modifier le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="dc28f-243">The user is notified and told to change the password.</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Program.cs?name=snippet)]

<span data-ttu-id="dc28f-244">L’exemple de code suivant présente l’ajout d’un utilisateur :</span><span class="sxs-lookup"><span data-stu-id="dc28f-244">The following code outlines adding a user:</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Data/SeedData.cs?name=snippet)]

<span data-ttu-id="dc28f-245">Une approche similaire peut être suivie pour les scénarios de production.</span><span class="sxs-lookup"><span data-stu-id="dc28f-245">A similar approach can be followed for production scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dc28f-246">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="dc28f-246">Additional resources</span></span>

* [<span data-ttu-id="dc28f-247">Modifications apportées au code d’authentification pour ASP.NET Core 2,1 et versions ultérieures</span><span class="sxs-lookup"><span data-stu-id="dc28f-247">Changes to authentication code to ASP.NET Core 2.1 and later</span></span>](xref:migration/20_21#changes-to-authentication-code)

::: moniker-end