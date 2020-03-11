---
title: Kit de développement logiciel (SDK) Web ASP.NET Core
author: Rick-Anderson
description: Vue d’ensemble de Microsoft. NET. Sdk. Web.
ms.author: riande
ms.date: 01/25/2020
no-loc:
- Blazor
uid: razor-pages/web-sdk
ms.openlocfilehash: 6a9d531efd2188aed525c949bb124914c31119db
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661055"
---
# <a name="aspnet-core-web-sdk"></a><span data-ttu-id="25b1e-103">Kit de développement logiciel (SDK) Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="25b1e-103">ASP.NET Core Web SDK</span></span>

### <a name="overview"></a><span data-ttu-id="25b1e-104">Vue d’ensemble</span><span class="sxs-lookup"><span data-stu-id="25b1e-104">Overview</span></span>

<span data-ttu-id="25b1e-105">`Microsoft.NET.Sdk.Web` est un [Kit de développement logiciel (SDK) de projet MSBuild](https://docs.microsoft.com/visualstudio/msbuild/how-to-use-project-sdk) pour la génération d’applications ASP.net core.</span><span class="sxs-lookup"><span data-stu-id="25b1e-105">`Microsoft.NET.Sdk.Web` is an [MSBuild project SDK](https://docs.microsoft.com/visualstudio/msbuild/how-to-use-project-sdk) for building ASP.NET Core apps.</span></span> <span data-ttu-id="25b1e-106">Il est possible de créer une application ASP.NET Core sans ce kit de développement logiciel (SDK), mais le kit de développement logiciel (SDK) Web est le suivant :</span><span class="sxs-lookup"><span data-stu-id="25b1e-106">It's possible to build an ASP.NET Core app without this SDK, however, the Web SDK is:</span></span>

* <span data-ttu-id="25b1e-107">Adapté à la fourniture d’une expérience de première classe.</span><span class="sxs-lookup"><span data-stu-id="25b1e-107">Tailored towards providing a first-class experience.</span></span>
* <span data-ttu-id="25b1e-108">Cible recommandée pour la plupart des utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="25b1e-108">The recommended target for most users.</span></span>

<span data-ttu-id="25b1e-109">Utilisez le kit de développement logiciel (SDK) Web dans un projet :</span><span class="sxs-lookup"><span data-stu-id="25b1e-109">Use the Web.SDK in a project:</span></span>

  ```xml
  <Project Sdk="Microsoft.NET.Sdk.Web">
    <!-- omitted for brevity -->
  </Project>
  ```

<span data-ttu-id="25b1e-110">Fonctionnalités activées à l’aide du kit de développement logiciel (SDK) Web :</span><span class="sxs-lookup"><span data-stu-id="25b1e-110">Features enabled by using the Web SDK:</span></span>

* <span data-ttu-id="25b1e-111">Les projets ciblant .NET Core 3,0 ou une version ultérieure référencent implicitement :</span><span class="sxs-lookup"><span data-stu-id="25b1e-111">Projects targeting .NET Core 3.0 or later implicitly reference:</span></span>

  * <span data-ttu-id="25b1e-112">[Framework partagé ASP.net Core](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="25b1e-112">The [ASP.NET Core shared framework](xref:fundamentals/metapackage-app).</span></span>
  * <span data-ttu-id="25b1e-113">[Analyseurs](/visualstudio/extensibility/getting-started-with-roslyn-analyzers) conçus pour générer des applications ASP.net core.</span><span class="sxs-lookup"><span data-stu-id="25b1e-113">[Analyzers](/visualstudio/extensibility/getting-started-with-roslyn-analyzers) designed for building ASP.NET Core apps.</span></span>
* <span data-ttu-id="25b1e-114">Le kit de développement logiciel (SDK) Web importe les cibles MSBuild qui permettent l’utilisation de profils de publication et la publication à l’aide de WebDeploy.</span><span class="sxs-lookup"><span data-stu-id="25b1e-114">The Web SDK imports MSBuild targets that enable the use of publish profiles and publishing using WebDeploy.</span></span>

### <a name="properties"></a><span data-ttu-id="25b1e-115">Propriétés</span><span class="sxs-lookup"><span data-stu-id="25b1e-115">Properties</span></span>

| <span data-ttu-id="25b1e-116">Propriété</span><span class="sxs-lookup"><span data-stu-id="25b1e-116">Property</span></span> | <span data-ttu-id="25b1e-117">Description</span><span class="sxs-lookup"><span data-stu-id="25b1e-117">Description</span></span> |
| -------- | ----------- |
| `DisableImplicitFrameworkReferences` | <span data-ttu-id="25b1e-118">Désactive la référence implicite au `Microsoft.AspNetCore.App` Framework partagé.</span><span class="sxs-lookup"><span data-stu-id="25b1e-118">Disables implicit reference to the `Microsoft.AspNetCore.App` shared framework.</span></span> |
| `DisableImplicitAspNetCoreAnalyzers` | <span data-ttu-id="25b1e-119">Désactive la référence implicite aux analyseurs de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="25b1e-119">Disables implicit reference to ASP.NET Core analyzers.</span></span> |
| `DisableImplicitComponentsAnalyzers` | <span data-ttu-id="25b1e-120">Désactive la référence implicite aux analyseurs de composants Razor lors de la création d’applications Blazor (serveur).</span><span class="sxs-lookup"><span data-stu-id="25b1e-120">Disables implicit reference to Razor Components analyzers when building Blazor (server) applications.</span></span> |