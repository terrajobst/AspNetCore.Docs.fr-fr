---
title: Identité d’une structure de projets ASP.NET Core
author: rick-anderson
description: Découvrez comment structurer d’identité dans un projet ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 08/16/2018
uid: security/authentication/scaffold-identity
ms.openlocfilehash: 4237e9084abe6575b1f69b143885e94196902965
ms.sourcegitcommit: e7e1e531b80b3f4117ff119caadbebf4dcf5dcb7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44510348"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="d8c3f-103">Identité d’une structure de projets ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d8c3f-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="d8c3f-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d8c3f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d8c3f-105">ASP.NET Core 2.1 et versions ultérieures offre [ASP.NET Core Identity](xref:security/authentication/identity) comme un [bibliothèque de classes Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="d8c3f-105">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="d8c3f-106">Les applications qui incluent l’identité peuvent s’appliquer à la génération de modèles automatique pour ajouter du code source contenu dans la bibliothèque de classes Razor (RCL) identité de façon sélective.</span><span class="sxs-lookup"><span data-stu-id="d8c3f-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="d8c3f-107">Vous pouvez souhaiter générer le code source afin de pouvoir modifier le code et changer le comportement.</span><span class="sxs-lookup"><span data-stu-id="d8c3f-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="d8c3f-108">Par exemple, vous pouvez demander au générateur de modèles automatique de générer le code utilisé dans l’inscription.</span><span class="sxs-lookup"><span data-stu-id="d8c3f-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="d8c3f-109">Le code généré est prioritaire sur le même code dans la bibliothèque de classes Razor d’identité.</span><span class="sxs-lookup"><span data-stu-id="d8c3f-109">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="d8c3f-110">Pour obtenir le contrôle intégral de l’interface utilisateur et n’utilisez pas la valeur par défaut RCL, consultez la section [créer une source de l’interface utilisateur complète identité](#full).</span><span class="sxs-lookup"><span data-stu-id="d8c3f-110">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="d8c3f-111">Les applications qui effectuent **pas** incluent l’authentification peut s’appliquer à la génération de modèles automatique pour ajouter le package RCL identité.</span><span class="sxs-lookup"><span data-stu-id="d8c3f-111">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="d8c3f-112">Vous pouvez sélectionner le code d’identité à générer.</span><span class="sxs-lookup"><span data-stu-id="d8c3f-112">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="d8c3f-113">Bien que le Générateur de modèles automatique génère la plupart du code nécessaire, vous devrez mettre à jour votre projet pour terminer le processus.</span><span class="sxs-lookup"><span data-stu-id="d8c3f-113">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="d8c3f-114">Ce document explique les étapes nécessaires pour effectuer une mise à jour de la génération de modèles automatique identité.</span><span class="sxs-lookup"><span data-stu-id="d8c3f-114">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="d8c3f-115">Lorsque le Générateur de modèles automatique identité est exécuté, un *ScaffoldingReadme.txt* fichier est créé dans le répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="d8c3f-115">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="d8c3f-116">Le *ScaffoldingReadme.txt* fichier contient des instructions générales sur ce qui est nécessaire pour terminer la mise à jour de la génération de modèles automatique identité.</span><span class="sxs-lookup"><span data-stu-id="d8c3f-116">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="d8c3f-117">Ce document contient des instructions plus complètes que les *ScaffoldingReadme.txt* fichier.</span><span class="sxs-lookup"><span data-stu-id="d8c3f-117">This document contains more complete instructions than the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="d8c3f-118">Nous vous recommandons d’utiliser un système de contrôle de source qui illustre les différences entre les fichiers et vous permet d’annuler les modifications.</span><span class="sxs-lookup"><span data-stu-id="d8c3f-118">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="d8c3f-119">Examiner les modifications après l’exécution de la génération de modèles automatique identité.</span><span class="sxs-lookup"><span data-stu-id="d8c3f-119">Inspect the changes after running the Identity scaffolder.</span></span>

> [!NOTE]
> <span data-ttu-id="d8c3f-120">Services sont requis lorsque vous utilisez [authentification à deux facteurs](xref:security/authentication/identity-enable-qrcodes), [confirmation et le mot de passe de récupération de compte](xref:security/authentication/accconfirm)et d’autres fonctionnalités de sécurité avec l’identité.</span><span class="sxs-lookup"><span data-stu-id="d8c3f-120">Services are required when using [Two Factor Authentication](xref:security/authentication/identity-enable-qrcodes), [Account confirmation and password recovery](xref:security/authentication/accconfirm), and other security features with Identity.</span></span> <span data-ttu-id="d8c3f-121">Services ou stubs de service ne sont pas générés lors de la génération de modèles automatique identité.</span><span class="sxs-lookup"><span data-stu-id="d8c3f-121">Services or service stubs aren't generated when scaffolding Identity.</span></span> <span data-ttu-id="d8c3f-122">Services pour activer ces fonctionnalités doivent être ajoutés manuellement.</span><span class="sxs-lookup"><span data-stu-id="d8c3f-122">Services to enable these features must be added manually.</span></span> <span data-ttu-id="d8c3f-123">Par exemple, consultez [nécessitent une Confirmation de courrier électronique](xref:security/authentication/accconfirm#require-email-confirmation).</span><span class="sxs-lookup"><span data-stu-id="d8c3f-123">For example, see [Require Email Confirmation](xref:security/authentication/accconfirm#require-email-confirmation).</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="d8c3f-124">Identité d’une structure dans un projet vide</span><span class="sxs-lookup"><span data-stu-id="d8c3f-124">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="d8c3f-125">Ajoutez les appels en surbrillance suivants à la `Startup` classe :</span><span class="sxs-lookup"><span data-stu-id="d8c3f-125">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="d8c3f-126">Identité d’une structure dans un projet Razor sans autorisation existant</span><span class="sxs-lookup"><span data-stu-id="d8c3f-126">Scaffold identity into a Razor project without existing authorization</span></span>

<!--
set projNam=RPnoAuth
set projType=razor
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

<span data-ttu-id="d8c3f-127">Identité est configurée dans *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="d8c3f-127">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="d8c3f-128">Pour plus d’informations, consultez [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="d8c3f-128">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="d8c3f-129">Migrations, UseAuthentication et la disposition</span><span class="sxs-lookup"><span data-stu-id="d8c3f-129">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a><span data-ttu-id="d8c3f-130">Activer l’authentification</span><span class="sxs-lookup"><span data-stu-id="d8c3f-130">Enable authentication</span></span>

<span data-ttu-id="d8c3f-131">Dans le `Configure` méthode de la `Startup` classe, appelez [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) après `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="d8c3f-131">In the `Configure` method of the `Startup` class, call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="d8c3f-132">Changements de disposition</span><span class="sxs-lookup"><span data-stu-id="d8c3f-132">Layout changes</span></span>

<span data-ttu-id="d8c3f-133">Facultatif : Ajouter la connexion partielle (`_LoginPartial`) pour le fichier de disposition :</span><span class="sxs-lookup"><span data-stu-id="d8c3f-133">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="d8c3f-134">Identité d’une structure dans un projet Razor disposant d’autorisations</span><span class="sxs-lookup"><span data-stu-id="d8c3f-134">Scaffold identity into a Razor project with authorization</span></span>

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth

dotnet new webapp -au Individual -o RPauth

dotnet new razor -au Individual -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files Account.Register

[!INCLUDE[](~/includes/webapp-alias-notice.md)]
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
<span data-ttu-id="d8c3f-135">Certaines options d’identité sont configurées dans *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="d8c3f-135">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="d8c3f-136">Pour plus d’informations, consultez [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="d8c3f-136">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="d8c3f-137">Identité d’une structure dans un projet MVC sans autorisation existant</span><span class="sxs-lookup"><span data-stu-id="d8c3f-137">Scaffold identity into an MVC project without existing authorization</span></span>

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

<span data-ttu-id="d8c3f-138">Facultatif : Ajouter la connexion partielle (`_LoginPartial`) pour le *Views/Shared/_Layout.cshtml* fichier :</span><span class="sxs-lookup"><span data-stu-id="d8c3f-138">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="d8c3f-139">Déplacer le *Pages/Shared/_LoginPartial.cshtml* fichier *Views/Shared/_LoginPartial.cshtml*</span><span class="sxs-lookup"><span data-stu-id="d8c3f-139">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="d8c3f-140">Identité est configurée dans *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="d8c3f-140">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="d8c3f-141">Pour plus d’informations, consultez IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="d8c3f-141">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="d8c3f-142">Appelez [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) après `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="d8c3f-142">Call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="d8c3f-143">Identité d’une structure dans un projet MVC avec l’autorisation</span><span class="sxs-lookup"><span data-stu-id="d8c3f-143">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="d8c3f-144">Supprimer le *Pages/Shared* dossier et les fichiers dans ce dossier.</span><span class="sxs-lookup"><span data-stu-id="d8c3f-144">Delete the *Pages/Shared* folder and the files in that folder.</span></span>

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="d8c3f-145">Créer la source de l’interface utilisateur d’identité complète</span><span class="sxs-lookup"><span data-stu-id="d8c3f-145">Create full identity UI source</span></span>

<span data-ttu-id="d8c3f-146">Pour conserver le contrôle intégral de l’interface utilisateur d’identité, exécutez le Générateur de modèles automatique identité et sélectionnez **remplacer tous les fichiers**.</span><span class="sxs-lookup"><span data-stu-id="d8c3f-146">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="d8c3f-147">Le code en surbrillance suivant montre les modifications apportées à remplacer la valeur par défaut de l’interface utilisateur de l’identité avec l’identité dans une application web de ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="d8c3f-147">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="d8c3f-148">Vous souhaiterez peut-être effectuer cette opération pour avoir un contrôle total de l’interface utilisateur d’identité.</span><span class="sxs-lookup"><span data-stu-id="d8c3f-148">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="d8c3f-149">L’identité par défaut est remplacé dans le code suivant :</span><span class="sxs-lookup"><span data-stu-id="d8c3f-149">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="d8c3f-150">Ce qui suit le code définit le [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [valeur de LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), et [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span><span class="sxs-lookup"><span data-stu-id="d8c3f-150">The following the code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="d8c3f-151">Inscrire un `IEmailSender` implémentation, par exemple :</span><span class="sxs-lookup"><span data-stu-id="d8c3f-151">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="d8c3f-152">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="d8c3f-152">Additional resources</span></span>

* [<span data-ttu-id="d8c3f-153">Modifications apportées au code d’authentification pour ASP.NET Core 2.1 et versions ultérieures</span><span class="sxs-lookup"><span data-stu-id="d8c3f-153">Changes to authentication code to ASP.NET Core 2.1 and later</span></span>](xref:migration/20_21#changes-to-authentication-code)
