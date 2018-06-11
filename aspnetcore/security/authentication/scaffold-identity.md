---
title: Identité de vue de structure dans les projets ASP.NET Core
author: rick-anderson
description: Découvrez comment structurer d’identité dans un projet ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 5/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/scaffold-identity
ms.openlocfilehash: e7a2cf3633ed48a0d2030739cdc092441fcae2ff
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252033"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="a4a2a-103">Identité de vue de structure dans les projets ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a4a2a-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="a4a2a-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a4a2a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a4a2a-105">ASP.NET Core 2.1 et versions ultérieures fournit [ASP.NET Core Identity](xref:security/authentication/identity) comme un [bibliothèque de classes Razor](xref:mvc/razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="a4a2a-105">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:mvc/razor-pages/ui-class).</span></span> <span data-ttu-id="a4a2a-106">Les applications qui incluent l’identité peuvent appliquer le scaffolder pour ajouter du code source contenu dans la bibliothèque de classe de Razor (RCL) identité de manière sélective.</span><span class="sxs-lookup"><span data-stu-id="a4a2a-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="a4a2a-107">Vous pouvez souhaiter générer le code source pour vous permettre de modifier le code et de modifier le comportement.</span><span class="sxs-lookup"><span data-stu-id="a4a2a-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="a4a2a-108">Par exemple, vous pouvez demander à le scaffolder pour générer le code utilisé dans l’inscription.</span><span class="sxs-lookup"><span data-stu-id="a4a2a-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="a4a2a-109">Code généré est prioritaire sur le même code de l’identité RCL.</span><span class="sxs-lookup"><span data-stu-id="a4a2a-109">Generated code takes precedence over the same code in the Identity RCL.</span></span>

<span data-ttu-id="a4a2a-110">Les applications qui effectuent **pas** incluent l’authentification peut appliquer le scaffolder pour ajouter le package RCL identité.</span><span class="sxs-lookup"><span data-stu-id="a4a2a-110">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="a4a2a-111">Vous avez la possibilité de sélectionner une code d’identité doit être créé.</span><span class="sxs-lookup"><span data-stu-id="a4a2a-111">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="a4a2a-112">Bien que le scaffolder génère la plupart du code nécessaire, vous devrez mettre à jour votre projet pour terminer le processus.</span><span class="sxs-lookup"><span data-stu-id="a4a2a-112">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="a4a2a-113">Ce document explique les étapes nécessaires pour effectuer une mise à jour de la génération de modèles automatique identité.</span><span class="sxs-lookup"><span data-stu-id="a4a2a-113">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="a4a2a-114">Lorsque le scaffolder d’identité est exécuté, un *ScaffoldingReadme.txt* fichier est créé dans le répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="a4a2a-114">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="a4a2a-115">Le *ScaffoldingReadme.txt* fichier contient des instructions générales sur ce qui est nécessaire pour terminer la mise à jour de la génération de modèles automatique identité.</span><span class="sxs-lookup"><span data-stu-id="a4a2a-115">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="a4a2a-116">Ce document contient des instructions détaillées que le *ScaffoldingReadme.txt* fichier.</span><span class="sxs-lookup"><span data-stu-id="a4a2a-116">This document contains more complete instructions than the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="a4a2a-117">Nous recommandons d’utiliser un système de contrôle de source qui montre les différences entre les fichiers et vous permet d’annuler les modifications.</span><span class="sxs-lookup"><span data-stu-id="a4a2a-117">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="a4a2a-118">Vérifiez que les modifications après avoir exécuté le scaffolder d’identité.</span><span class="sxs-lookup"><span data-stu-id="a4a2a-118">Inspect the changes after running the Identity scaffolder.</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="a4a2a-119">Identité de vue de structure dans un projet vide</span><span class="sxs-lookup"><span data-stu-id="a4a2a-119">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="a4a2a-120">Ajouter les appels en surbrillance suivants à la `Startup` classe :</span><span class="sxs-lookup"><span data-stu-id="a4a2a-120">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="a4a2a-121">Identité de vue de structure dans un projet Razor sans autorisation existante</span><span class="sxs-lookup"><span data-stu-id="a4a2a-121">Scaffold identity into a Razor project without existing authorization</span></span>

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

<span data-ttu-id="a4a2a-122">Identité est configurée dans *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="a4a2a-122">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="a4a2a-123">Pour plus d’informations, consultez [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="a4a2a-123">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="a4a2a-124">Migrations, UseAuthentication et la disposition</span><span class="sxs-lookup"><span data-stu-id="a4a2a-124">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="a4a2a-125">Dans le `Configure` méthode de la `Startup` classe, appelez [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) après `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="a4a2a-125">In the `Configure` method of the `Startup` class, call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="a4a2a-126">Changements de disposition</span><span class="sxs-lookup"><span data-stu-id="a4a2a-126">Layout changes</span></span>

<span data-ttu-id="a4a2a-127">Facultatif : Ajoutez la connexion partielle (`_LoginPartial`) pour le fichier de disposition :</span><span class="sxs-lookup"><span data-stu-id="a4a2a-127">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="a4a2a-128">Identité de vue de structure dans un projet Razor disposant d’autorisations</span><span class="sxs-lookup"><span data-stu-id="a4a2a-128">Scaffold identity into a Razor project with authorization</span></span>

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
<span data-ttu-id="a4a2a-129">Certaines options d’identité sont configurées dans *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="a4a2a-129">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="a4a2a-130">Pour plus d’informations, consultez [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="a4a2a-130">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="a4a2a-131">Identité de vue de structure dans un projet MVC sans autorisation existante</span><span class="sxs-lookup"><span data-stu-id="a4a2a-131">Scaffold identity into an MVC project without existing authorization</span></span>

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

<span data-ttu-id="a4a2a-132">Facultatif : Ajoutez la connexion partielle (`_LoginPartial`) pour le *Views/Shared/_Layout.cshtml* fichier :</span><span class="sxs-lookup"><span data-stu-id="a4a2a-132">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[Main](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="a4a2a-133">Déplacer le *Pages/Shared/_LoginPartial.cshtml* le fichier *Views/Shared/_LoginPartial.cshtml*</span><span class="sxs-lookup"><span data-stu-id="a4a2a-133">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="a4a2a-134">Identité est configurée dans *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="a4a2a-134">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="a4a2a-135">Pour plus d’informations, consultez IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="a4a2a-135">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="a4a2a-136">Appelez [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) après `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="a4a2a-136">Call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="a4a2a-137">Identité de vue de structure dans un projet MVC avec autorisation</span><span class="sxs-lookup"><span data-stu-id="a4a2a-137">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="a4a2a-138">Supprimer le *Pages/Shared* dossier et les fichiers dans ce dossier.</span><span class="sxs-lookup"><span data-stu-id="a4a2a-138">Delete the *Pages/Shared* folder and the files in that folder.</span></span>
