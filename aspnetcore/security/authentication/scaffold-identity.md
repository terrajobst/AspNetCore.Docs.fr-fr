---
title: Identité de vue de structure dans les projets ASP.NET Core
author: rick-anderson
description: Découvrez comment structurer d’identité dans un projet ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 5/16/2018
uid: security/authentication/scaffold-identity
ms.openlocfilehash: cf6544d8b671f026c8466fa8dff506027b64cf1f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276316"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="89f0d-103">Identité de vue de structure dans les projets ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="89f0d-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="89f0d-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="89f0d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="89f0d-105">ASP.NET Core 2.1 et versions ultérieures fournit [ASP.NET Core Identity](xref:security/authentication/identity) comme un [bibliothèque de classes Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="89f0d-105">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="89f0d-106">Les applications qui incluent l’identité peuvent appliquer le scaffolder pour ajouter du code source contenu dans la bibliothèque de classe de Razor (RCL) identité de manière sélective.</span><span class="sxs-lookup"><span data-stu-id="89f0d-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="89f0d-107">Vous pouvez souhaiter générer le code source pour vous permettre de modifier le code et de modifier le comportement.</span><span class="sxs-lookup"><span data-stu-id="89f0d-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="89f0d-108">Par exemple, vous pouvez demander à le scaffolder pour générer le code utilisé dans l’inscription.</span><span class="sxs-lookup"><span data-stu-id="89f0d-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="89f0d-109">Code généré est prioritaire sur le même code de l’identité RCL.</span><span class="sxs-lookup"><span data-stu-id="89f0d-109">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="89f0d-110">Pour obtenir le contrôle intégral de l’interface utilisateur et n’utilisez pas la valeur par défaut RCL, consultez la section [créer une source de l’interface utilisateur complète identité](#full).</span><span class="sxs-lookup"><span data-stu-id="89f0d-110">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="89f0d-111">Les applications qui effectuent **pas** incluent l’authentification peut appliquer le scaffolder pour ajouter le package RCL identité.</span><span class="sxs-lookup"><span data-stu-id="89f0d-111">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="89f0d-112">Vous avez la possibilité de sélectionner une code d’identité doit être créé.</span><span class="sxs-lookup"><span data-stu-id="89f0d-112">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="89f0d-113">Bien que le scaffolder génère la plupart du code nécessaire, vous devrez mettre à jour votre projet pour terminer le processus.</span><span class="sxs-lookup"><span data-stu-id="89f0d-113">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="89f0d-114">Ce document explique les étapes nécessaires pour effectuer une mise à jour de la génération de modèles automatique identité.</span><span class="sxs-lookup"><span data-stu-id="89f0d-114">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="89f0d-115">Lorsque le scaffolder d’identité est exécuté, un *ScaffoldingReadme.txt* fichier est créé dans le répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="89f0d-115">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="89f0d-116">Le *ScaffoldingReadme.txt* fichier contient des instructions générales sur ce qui est nécessaire pour terminer la mise à jour de la génération de modèles automatique identité.</span><span class="sxs-lookup"><span data-stu-id="89f0d-116">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="89f0d-117">Ce document contient des instructions détaillées que le *ScaffoldingReadme.txt* fichier.</span><span class="sxs-lookup"><span data-stu-id="89f0d-117">This document contains more complete instructions than the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="89f0d-118">Nous recommandons d’utiliser un système de contrôle de source qui montre les différences entre les fichiers et vous permet d’annuler les modifications.</span><span class="sxs-lookup"><span data-stu-id="89f0d-118">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="89f0d-119">Vérifiez que les modifications après avoir exécuté le scaffolder d’identité.</span><span class="sxs-lookup"><span data-stu-id="89f0d-119">Inspect the changes after running the Identity scaffolder.</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="89f0d-120">Identité de vue de structure dans un projet vide</span><span class="sxs-lookup"><span data-stu-id="89f0d-120">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="89f0d-121">Ajouter les appels en surbrillance suivants à la `Startup` classe :</span><span class="sxs-lookup"><span data-stu-id="89f0d-121">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="89f0d-122">Identité de vue de structure dans un projet Razor sans autorisation existante</span><span class="sxs-lookup"><span data-stu-id="89f0d-122">Scaffold identity into a Razor project without existing authorization</span></span>

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

<span data-ttu-id="89f0d-123">Identité est configurée dans *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="89f0d-123">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="89f0d-124">Pour plus d’informations, consultez [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="89f0d-124">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="89f0d-125">Migrations, UseAuthentication et la disposition</span><span class="sxs-lookup"><span data-stu-id="89f0d-125">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="89f0d-126">Dans le `Configure` méthode de la `Startup` classe, appelez [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) après `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="89f0d-126">In the `Configure` method of the `Startup` class, call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="89f0d-127">Changements de disposition</span><span class="sxs-lookup"><span data-stu-id="89f0d-127">Layout changes</span></span>

<span data-ttu-id="89f0d-128">Facultatif : Ajoutez la connexion partielle (`_LoginPartial`) pour le fichier de disposition :</span><span class="sxs-lookup"><span data-stu-id="89f0d-128">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="89f0d-129">Identité de vue de structure dans un projet Razor disposant d’autorisations</span><span class="sxs-lookup"><span data-stu-id="89f0d-129">Scaffold identity into a Razor project with authorization</span></span>

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files Account.Register

[!INCLUDE[](~/includes/webapp-alias-notice.md)]
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
<span data-ttu-id="89f0d-130">Certaines options d’identité sont configurées dans *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="89f0d-130">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="89f0d-131">Pour plus d’informations, consultez [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="89f0d-131">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="89f0d-132">Identité de vue de structure dans un projet MVC sans autorisation existante</span><span class="sxs-lookup"><span data-stu-id="89f0d-132">Scaffold identity into an MVC project without existing authorization</span></span>

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

<span data-ttu-id="89f0d-133">Facultatif : Ajoutez la connexion partielle (`_LoginPartial`) pour le *Views/Shared/_Layout.cshtml* fichier :</span><span class="sxs-lookup"><span data-stu-id="89f0d-133">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="89f0d-134">Déplacer le *Pages/Shared/_LoginPartial.cshtml* le fichier *Views/Shared/_LoginPartial.cshtml*</span><span class="sxs-lookup"><span data-stu-id="89f0d-134">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="89f0d-135">Identité est configurée dans *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="89f0d-135">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="89f0d-136">Pour plus d’informations, consultez IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="89f0d-136">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="89f0d-137">Appelez [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) après `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="89f0d-137">Call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="89f0d-138">Identité de vue de structure dans un projet MVC avec autorisation</span><span class="sxs-lookup"><span data-stu-id="89f0d-138">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="89f0d-139">Supprimer le *Pages/Shared* dossier et les fichiers dans ce dossier.</span><span class="sxs-lookup"><span data-stu-id="89f0d-139">Delete the *Pages/Shared* folder and the files in that folder.</span></span>

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="89f0d-140">Créer la source de l’interface utilisateur d’identité complète</span><span class="sxs-lookup"><span data-stu-id="89f0d-140">Create full identity UI source</span></span>

<span data-ttu-id="89f0d-141">Pour gérer le contrôle complet de l’interface utilisateur d’identité, exécutez le scaffolder d’identité et sélectionnez **remplacer tous les fichiers**.</span><span class="sxs-lookup"><span data-stu-id="89f0d-141">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="89f0d-142">Le code en surbrillance suivant indique les modifications pour remplacer la valeur par défaut de l’interface utilisateur de l’identité avec l’identité dans une application de web ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="89f0d-142">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="89f0d-143">Vous pouvez souhaiter procéder ainsi pour avoir un contrôle total de l’interface utilisateur d’identité.</span><span class="sxs-lookup"><span data-stu-id="89f0d-143">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="89f0d-144">L’identité par défaut est remplacée dans le code suivant :</span><span class="sxs-lookup"><span data-stu-id="89f0d-144">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="89f0d-145">Ce qui suit le code définit les [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [valeur de LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), et [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span><span class="sxs-lookup"><span data-stu-id="89f0d-145">The following the code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="89f0d-146">Inscrire un `IEmailSender` implémentation, par exemple :</span><span class="sxs-lookup"><span data-stu-id="89f0d-146">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]
