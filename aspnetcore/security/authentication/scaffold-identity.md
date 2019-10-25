---
title: Identité de l’échafaudage dans les projets ASP.NET Core
author: rick-anderson
description: Découvrez comment générer une structure d’identité dans un projet ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/authentication/scaffold-identity
ms.openlocfilehash: ca2046563281efc3c1cd8f4fec73fe4f8d3fbdda
ms.sourcegitcommit: 383017d7060a6d58f6a79cf4d7335d5b4b6c5659
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72816109"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="3bff9-103">Identité de l’échafaudage dans les projets ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3bff9-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="3bff9-104">De [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3bff9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3bff9-105">ASP.NET Core 2,1 et versions ultérieures fournissent [ASP.net Core identité](xref:security/authentication/identity) sous forme de [bibliothèque de classes Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="3bff9-105">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="3bff9-106">Les applications qui incluent une identité peuvent appliquer l’échafaudage pour ajouter de manière sélective le code source contenu dans la bibliothèque de classes Razor d’identité (RCL).</span><span class="sxs-lookup"><span data-stu-id="3bff9-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="3bff9-107">Vous pouvez souhaiter générer le code source afin de pouvoir modifier le code et changer le comportement.</span><span class="sxs-lookup"><span data-stu-id="3bff9-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="3bff9-108">Par exemple, vous pouvez demander au générateur de modèles automatique de générer le code utilisé dans l’inscription.</span><span class="sxs-lookup"><span data-stu-id="3bff9-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="3bff9-109">Le code généré est prioritaire sur le même code dans la bibliothèque de classes Razor d’identité.</span><span class="sxs-lookup"><span data-stu-id="3bff9-109">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="3bff9-110">Pour obtenir le contrôle total de l’interface utilisateur et ne pas utiliser le RCL par défaut, consultez la section [créer une source d’interface utilisateur d’identité complète](#full).</span><span class="sxs-lookup"><span data-stu-id="3bff9-110">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="3bff9-111">Les applications qui n’incluent **pas** l’authentification peuvent appliquer l’échafaudage pour ajouter le package d’identité RCL.</span><span class="sxs-lookup"><span data-stu-id="3bff9-111">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="3bff9-112">Vous pouvez sélectionner le code d’identité à générer.</span><span class="sxs-lookup"><span data-stu-id="3bff9-112">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="3bff9-113">Bien que le générateur de modèles génère la plus grande partie du code nécessaire, vous devez mettre à jour votre projet pour terminer le processus.</span><span class="sxs-lookup"><span data-stu-id="3bff9-113">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="3bff9-114">Ce document explique les étapes nécessaires pour effectuer une mise à jour de l’échafaudage d’identité.</span><span class="sxs-lookup"><span data-stu-id="3bff9-114">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="3bff9-115">Lorsque l’échafaudage d’identité est exécuté, un fichier *ScaffoldingReadme. txt* est créé dans le répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="3bff9-115">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="3bff9-116">Le fichier *ScaffoldingReadme. txt* contient des instructions générales sur ce qui est nécessaire pour effectuer la mise à jour de l’échafaudage d’identité.</span><span class="sxs-lookup"><span data-stu-id="3bff9-116">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="3bff9-117">Ce document contient des instructions plus complètes que le fichier *ScaffoldingReadme. txt* .</span><span class="sxs-lookup"><span data-stu-id="3bff9-117">This document contains more complete instructions than the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="3bff9-118">Nous vous recommandons d’utiliser un système de contrôle de code source qui affiche les différences entre les fichiers et vous permet d’annuler les modifications.</span><span class="sxs-lookup"><span data-stu-id="3bff9-118">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="3bff9-119">Inspectez les modifications après avoir exécuté l’échafaudage d’identité.</span><span class="sxs-lookup"><span data-stu-id="3bff9-119">Inspect the changes after running the Identity scaffolder.</span></span>

> [!NOTE]
> <span data-ttu-id="3bff9-120">Les services sont requis lors de l’utilisation de [l’authentification à deux facteurs](xref:security/authentication/identity-enable-qrcodes), la [confirmation de compte et la récupération de mot de passe](xref:security/authentication/accconfirm), ainsi que d’autres fonctionnalités de sécurité avec identité.</span><span class="sxs-lookup"><span data-stu-id="3bff9-120">Services are required when using [Two Factor Authentication](xref:security/authentication/identity-enable-qrcodes), [Account confirmation and password recovery](xref:security/authentication/accconfirm), and other security features with Identity.</span></span> <span data-ttu-id="3bff9-121">Les services ou stubs de service ne sont pas générés lors de la génération de modèles automatique d’identité.</span><span class="sxs-lookup"><span data-stu-id="3bff9-121">Services or service stubs aren't generated when scaffolding Identity.</span></span> <span data-ttu-id="3bff9-122">Les services pour activer ces fonctionnalités doivent être ajoutés manuellement.</span><span class="sxs-lookup"><span data-stu-id="3bff9-122">Services to enable these features must be added manually.</span></span> <span data-ttu-id="3bff9-123">Par exemple, consultez [exiger une confirmation par courrier électronique](xref:security/authentication/accconfirm#require-email-confirmation).</span><span class="sxs-lookup"><span data-stu-id="3bff9-123">For example, see [Require Email Confirmation](xref:security/authentication/accconfirm#require-email-confirmation).</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="3bff9-124">Identité de l’échafaudage dans un projet vide</span><span class="sxs-lookup"><span data-stu-id="3bff9-124">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="3bff9-125">Ajoutez les appels en surbrillance suivants à la classe `Startup` :</span><span class="sxs-lookup"><span data-stu-id="3bff9-125">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="3bff9-126">Identité de l’échafaudage dans un projet Razor sans autorisation existante</span><span class="sxs-lookup"><span data-stu-id="3bff9-126">Scaffold identity into a Razor project without existing authorization</span></span>

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

<span data-ttu-id="3bff9-127">L’identité est configurée dans *Areas/Identity/IdentityHostingStartup. cs*.</span><span class="sxs-lookup"><span data-stu-id="3bff9-127">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="3bff9-128">Pour plus d’informations, consultez [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="3bff9-128">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="3bff9-129">Migrations, UseAuthentication et disposition</span><span class="sxs-lookup"><span data-stu-id="3bff9-129">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a><span data-ttu-id="3bff9-130">Activer l’authentification</span><span class="sxs-lookup"><span data-stu-id="3bff9-130">Enable authentication</span></span>

<span data-ttu-id="3bff9-131">Dans la méthode `Configure` de la classe `Startup`, appelez [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) après `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="3bff9-131">In the `Configure` method of the `Startup` class, call [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="3bff9-132">Modifications de la disposition</span><span class="sxs-lookup"><span data-stu-id="3bff9-132">Layout changes</span></span>

<span data-ttu-id="3bff9-133">Facultatif : ajoutez la connexion partielle (`_LoginPartial`) au fichier de disposition :</span><span class="sxs-lookup"><span data-stu-id="3bff9-133">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="3bff9-134">Identité de l’échafaudage dans un projet Razor avec autorisation</span><span class="sxs-lookup"><span data-stu-id="3bff9-134">Scaffold identity into a Razor project with authorization</span></span>

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
<span data-ttu-id="3bff9-135">Certaines options d’identité sont configurées dans *Areas/Identity/IdentityHostingStartup. cs*.</span><span class="sxs-lookup"><span data-stu-id="3bff9-135">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="3bff9-136">Pour plus d’informations, consultez [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="3bff9-136">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="3bff9-137">Identité de l’échafaudage dans un projet MVC sans autorisation existante</span><span class="sxs-lookup"><span data-stu-id="3bff9-137">Scaffold identity into an MVC project without existing authorization</span></span>

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

<span data-ttu-id="3bff9-138">Facultatif : ajoutez la connexion partielle (`_LoginPartial`) au fichier *Views/Shared/_ Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="3bff9-138">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="3bff9-139">Déplacez le fichier *pages/Shared/_LoginPartial. cshtml* vers *Views/Shared/_LoginPartial. cshtml*</span><span class="sxs-lookup"><span data-stu-id="3bff9-139">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="3bff9-140">L’identité est configurée dans *Areas/Identity/IdentityHostingStartup. cs*.</span><span class="sxs-lookup"><span data-stu-id="3bff9-140">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="3bff9-141">Pour plus d’informations, consultez IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="3bff9-141">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="3bff9-142">Appelez [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) après `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="3bff9-142">Call [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="3bff9-143">Identité de l’échafaudage dans un projet MVC avec autorisation</span><span class="sxs-lookup"><span data-stu-id="3bff9-143">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext  --files "Account.Login;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="3bff9-144">Supprimez le dossier *pages/Shared* et les fichiers de ce dossier.</span><span class="sxs-lookup"><span data-stu-id="3bff9-144">Delete the *Pages/Shared* folder and the files in that folder.</span></span>

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="3bff9-145">Créer une source d’interface utilisateur d’identité complète</span><span class="sxs-lookup"><span data-stu-id="3bff9-145">Create full identity UI source</span></span>

<span data-ttu-id="3bff9-146">Pour conserver le contrôle total de l’interface utilisateur de l’identité, exécutez le générateur de modèles d’identité et sélectionnez **remplacer tous les fichiers**.</span><span class="sxs-lookup"><span data-stu-id="3bff9-146">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="3bff9-147">Le code en surbrillance suivant montre les modifications pour remplacer l’interface utilisateur d’identité par défaut par l’identité dans une application Web ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="3bff9-147">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="3bff9-148">Vous pouvez le faire pour avoir le contrôle total de l’interface utilisateur de l’identité.</span><span class="sxs-lookup"><span data-stu-id="3bff9-148">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="3bff9-149">L’identité par défaut est remplacée dans le code suivant :</span><span class="sxs-lookup"><span data-stu-id="3bff9-149">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="3bff9-150">Le code suivant définit les [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)et [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span><span class="sxs-lookup"><span data-stu-id="3bff9-150">The following code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="3bff9-151">Inscrire une implémentation de `IEmailSender`, par exemple :</span><span class="sxs-lookup"><span data-stu-id="3bff9-151">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

<!--
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
-->
## <a name="disable-register-page"></a><span data-ttu-id="3bff9-152">Désactiver la page de Registre</span><span class="sxs-lookup"><span data-stu-id="3bff9-152">Disable register page</span></span>

<span data-ttu-id="3bff9-153">Pour désactiver l’inscription des utilisateurs :</span><span class="sxs-lookup"><span data-stu-id="3bff9-153">To disable user registration:</span></span>

* <span data-ttu-id="3bff9-154">Identité de l’échafaudage.</span><span class="sxs-lookup"><span data-stu-id="3bff9-154">Scaffold Identity.</span></span> <span data-ttu-id="3bff9-155">Incluez Account. Register, Account. Login et Account. RegisterConfirmation.</span><span class="sxs-lookup"><span data-stu-id="3bff9-155">Include Account.Register, Account.Login, and Account.RegisterConfirmation.</span></span> <span data-ttu-id="3bff9-156">Exemple :</span><span class="sxs-lookup"><span data-stu-id="3bff9-156">For example:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
  ```

* <span data-ttu-id="3bff9-157">Mettez à jour *Areas/Identity/pages/Account/Register. cshtml. cs* afin que les utilisateurs ne puissent pas s’inscrire à partir de ce point de terminaison :</span><span class="sxs-lookup"><span data-stu-id="3bff9-157">Update *Areas/Identity/Pages/Account/Register.cshtml.cs* so users can't register from this endpoint:</span></span>

  [!code-csharp[](scaffold-identity/sample/Register.cshtml.cs?name=snippet)]

* <span data-ttu-id="3bff9-158">Mettez à jour *Areas/Identity/pages/Account/Register. cshtml* pour qu’il soit cohérent avec les modifications précédentes :</span><span class="sxs-lookup"><span data-stu-id="3bff9-158">Update *Areas/Identity/Pages/Account/Register.cshtml* to be consistent with the preceding changes:</span></span>

  [!code-cshtml[](scaffold-identity/sample/Register.cshtml)]

* <span data-ttu-id="3bff9-159">Commentez ou supprimez le lien d’inscription de *Areas/Identity/pages/Account/login. cshtml*</span><span class="sxs-lookup"><span data-stu-id="3bff9-159">Comment out or remove the registration link from *Areas/Identity/Pages/Account/Login.cshtml*</span></span>

```cshtml
@*
<p>
    <a asp-page="./Register" asp-route-returnUrl="@Model.ReturnUrl">Register as a new user</a>
</p>
*@
```

* <span data-ttu-id="3bff9-160">Mettez à jour la page *Areas/Identity/pages/Account/RegisterConfirmation* .</span><span class="sxs-lookup"><span data-stu-id="3bff9-160">Update the *Areas/Identity/Pages/Account/RegisterConfirmation* page.</span></span>

  * <span data-ttu-id="3bff9-161">Supprimez le code et les liens du fichier cshtml.</span><span class="sxs-lookup"><span data-stu-id="3bff9-161">Remove the code and links from the cshtml file.</span></span>
  * <span data-ttu-id="3bff9-162">Supprimez le code de confirmation du `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="3bff9-162">Remove the confirmation code from the `PageModel`:</span></span>

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
  
### <a name="use-another-app-to-add-users"></a><span data-ttu-id="3bff9-163">Utiliser une autre application pour ajouter des utilisateurs</span><span class="sxs-lookup"><span data-stu-id="3bff9-163">Use another app to add users</span></span>

<span data-ttu-id="3bff9-164">Fournissez un mécanisme pour ajouter des utilisateurs en dehors de l’application Web.</span><span class="sxs-lookup"><span data-stu-id="3bff9-164">Provide a mechanism to add users outside the web app.</span></span> <span data-ttu-id="3bff9-165">Les options permettant d’ajouter des utilisateurs sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="3bff9-165">Options to add users include:</span></span>

* <span data-ttu-id="3bff9-166">Une application Web d’administration dédiée.</span><span class="sxs-lookup"><span data-stu-id="3bff9-166">A dedicated admin web app.</span></span>
* <span data-ttu-id="3bff9-167">Application console.</span><span class="sxs-lookup"><span data-stu-id="3bff9-167">A console app.</span></span>

<span data-ttu-id="3bff9-168">Le code suivant décrit une approche de l’ajout d’utilisateurs :</span><span class="sxs-lookup"><span data-stu-id="3bff9-168">The following code outlines one approach to adding users:</span></span>

* <span data-ttu-id="3bff9-169">Une liste d’utilisateurs est lue en mémoire.</span><span class="sxs-lookup"><span data-stu-id="3bff9-169">A list of users is read into memory.</span></span>
* <span data-ttu-id="3bff9-170">Un mot de passe fort unique est généré pour chaque utilisateur.</span><span class="sxs-lookup"><span data-stu-id="3bff9-170">A strong unique password is generated for each user.</span></span>
* <span data-ttu-id="3bff9-171">L’utilisateur est ajouté à la base de données d’identité.</span><span class="sxs-lookup"><span data-stu-id="3bff9-171">The user is added to the Identity database.</span></span>
* <span data-ttu-id="3bff9-172">L’utilisateur est averti et a demandé à modifier le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="3bff9-172">The user is notified and told to change the password.</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Program.cs?name=snippet)]

<span data-ttu-id="3bff9-173">L’exemple de code suivant présente l’ajout d’un utilisateur :</span><span class="sxs-lookup"><span data-stu-id="3bff9-173">The following code outlines adding a user:</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Data/SeedData.cs?name=snippet)]

<span data-ttu-id="3bff9-174">Une approche similaire peut être suivie pour les scénarios de production.</span><span class="sxs-lookup"><span data-stu-id="3bff9-174">A similar approach can be followed for production scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3bff9-175">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="3bff9-175">Additional resources</span></span>

* [<span data-ttu-id="3bff9-176">Modifications apportées au code d’authentification pour ASP.NET Core 2,1 et versions ultérieures</span><span class="sxs-lookup"><span data-stu-id="3bff9-176">Changes to authentication code to ASP.NET Core 2.1 and later</span></span>](xref:migration/20_21#changes-to-authentication-code)
