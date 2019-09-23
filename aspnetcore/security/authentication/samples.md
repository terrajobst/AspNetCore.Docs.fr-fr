---
title: Exemples d’authentification pour ASP.NET Core
author: rick-anderson
description: Fournit des liens vers les exemples d’authentification dans le référentiel ASP.NET Core.
ms.author: riande
ms.date: 01/31/2019
uid: security/authentication/samples
ms.openlocfilehash: d49aef198e926d88f1a6727f84b06f0861c8812d
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187301"
---
# <a name="authentication-samples-for-aspnet-core"></a><span data-ttu-id="176c2-103">Exemples d’authentification pour ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="176c2-103">Authentication samples for ASP.NET Core</span></span>

<span data-ttu-id="176c2-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="176c2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="176c2-105">Le [référentiel ASP.net Core](https://github.com/aspnet/AspNetCore) contient les exemples d’authentification suivants dans le dossier *AspNetCore/SRC/Security/Samples* :</span><span class="sxs-lookup"><span data-stu-id="176c2-105">The [ASP.NET Core repository](https://github.com/aspnet/AspNetCore) contains the following authentication samples in the *AspNetCore/src/Security/samples* folder:</span></span>

* [<span data-ttu-id="176c2-106">Transformation des revendications</span><span class="sxs-lookup"><span data-stu-id="176c2-106">Claims transformation</span></span>](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/ClaimsTransformation)
* [<span data-ttu-id="176c2-107">Authentification par cookie</span><span class="sxs-lookup"><span data-stu-id="176c2-107">Cookie authentication</span></span>](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/Cookies)
* [<span data-ttu-id="176c2-108">Fournisseur de stratégie personnalisée-IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="176c2-108">Custom policy provider - IAuthorizationPolicyProvider</span></span>](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/CustomPolicyProvider)
* [<span data-ttu-id="176c2-109">Options et schémas d’authentification dynamique</span><span class="sxs-lookup"><span data-stu-id="176c2-109">Dynamic authentication schemes and options</span></span>](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/DynamicSchemes)
* [<span data-ttu-id="176c2-110">Revendications externes</span><span class="sxs-lookup"><span data-stu-id="176c2-110">External claims</span></span>](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/Identity.ExternalClaims)
* [<span data-ttu-id="176c2-111">Sélection entre un cookie et un autre schéma d’authentification en fonction de la demande</span><span class="sxs-lookup"><span data-stu-id="176c2-111">Selecting between cookie and another authentication scheme based on the request</span></span>](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/PathSchemeSelection)
* [<span data-ttu-id="176c2-112">Restreint l’accès aux fichiers statiques</span><span class="sxs-lookup"><span data-stu-id="176c2-112">Restricts access to static files</span></span>](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a><span data-ttu-id="176c2-113">Exécuter les exemples</span><span class="sxs-lookup"><span data-stu-id="176c2-113">Run the samples</span></span>

* <span data-ttu-id="176c2-114">Sélectionnez une [branche](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="176c2-114">Select a [branch](https://github.com/aspnet/AspNetCore).</span></span> <span data-ttu-id="176c2-115">Par exemple, `Tag:v3.0.0`.</span><span class="sxs-lookup"><span data-stu-id="176c2-115">For example, `Tag:v3.0.0`</span></span>
* <span data-ttu-id="176c2-116">Clonez ou téléchargez le [référentiel ASP.net Core](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="176c2-116">Clone or download the [ASP.NET Core repository](https://github.com/aspnet/AspNetCore).</span></span>
* <span data-ttu-id="176c2-117">Vérifiez que vous avez installé la version de [Kit SDK .net Core](https://www.microsoft.com/net/download/all) qui correspond au clone du référentiel ASP.net core.</span><span class="sxs-lookup"><span data-stu-id="176c2-117">Verify you have installed the [.NET Core SDK](https://www.microsoft.com/net/download/all) version matching the clone of the ASP.NET Core repository.</span></span>
* <span data-ttu-id="176c2-118">Accédez à un exemple dans *AspNetCore/SRC/Security/Samples* et exécutez l’exemple `dotnet run`avec.</span><span class="sxs-lookup"><span data-stu-id="176c2-118">Navigate to a sample in *AspNetCore/src/Security/samples* and run the sample with `dotnet run`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="176c2-119">Le [référentiel ASP.net Core](https://github.com/aspnet/AspNetCore) contient les exemples d’authentification suivants dans le dossier *AspNetCore/SRC/Security/Samples* :</span><span class="sxs-lookup"><span data-stu-id="176c2-119">The [ASP.NET Core repository](https://github.com/aspnet/AspNetCore) contains the following authentication samples in the *AspNetCore/src/Security/samples* folder:</span></span>

* [<span data-ttu-id="176c2-120">Transformation des revendications</span><span class="sxs-lookup"><span data-stu-id="176c2-120">Claims transformation</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [<span data-ttu-id="176c2-121">Authentification par cookie</span><span class="sxs-lookup"><span data-stu-id="176c2-121">Cookie authentication</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [<span data-ttu-id="176c2-122">Fournisseur de stratégie personnalisée-IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="176c2-122">Custom policy provider - IAuthorizationPolicyProvider</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [<span data-ttu-id="176c2-123">Options et schémas d’authentification dynamique</span><span class="sxs-lookup"><span data-stu-id="176c2-123">Dynamic authentication schemes and options</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [<span data-ttu-id="176c2-124">Revendications externes</span><span class="sxs-lookup"><span data-stu-id="176c2-124">External claims</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [<span data-ttu-id="176c2-125">Sélection entre un cookie et un autre schéma d’authentification en fonction de la demande</span><span class="sxs-lookup"><span data-stu-id="176c2-125">Selecting between cookie and another authentication scheme based on the request</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [<span data-ttu-id="176c2-126">Restreint l’accès aux fichiers statiques</span><span class="sxs-lookup"><span data-stu-id="176c2-126">Restricts access to static files</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a><span data-ttu-id="176c2-127">Exécuter les exemples</span><span class="sxs-lookup"><span data-stu-id="176c2-127">Run the samples</span></span>

* <span data-ttu-id="176c2-128">Sélectionnez une [branche](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="176c2-128">Select a [branch](https://github.com/aspnet/AspNetCore).</span></span> <span data-ttu-id="176c2-129">Par exemple, `release/2.2`.</span><span class="sxs-lookup"><span data-stu-id="176c2-129">For example, `release/2.2`</span></span>
* <span data-ttu-id="176c2-130">Clonez ou téléchargez le [référentiel ASP.net Core](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="176c2-130">Clone or download the [ASP.NET Core repository](https://github.com/aspnet/AspNetCore).</span></span>
* <span data-ttu-id="176c2-131">Vérifiez que vous avez installé la version de [Kit SDK .net Core](https://www.microsoft.com/net/download/all) qui correspond au clone du référentiel ASP.net core.</span><span class="sxs-lookup"><span data-stu-id="176c2-131">Verify you have installed the [.NET Core SDK](https://www.microsoft.com/net/download/all) version matching the clone of the ASP.NET Core repository.</span></span>
* <span data-ttu-id="176c2-132">Accédez à un exemple dans *AspNetCore/SRC/Security/Samples* et exécutez l’exemple `dotnet run`avec.</span><span class="sxs-lookup"><span data-stu-id="176c2-132">Navigate to a sample in *AspNetCore/src/Security/samples* and run the sample with `dotnet run`.</span></span>

::: moniker-end
