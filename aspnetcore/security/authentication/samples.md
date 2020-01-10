---
title: Exemples d’authentification pour ASP.NET Core
author: rick-anderson
description: Fournit des liens vers les exemples d’authentification dans le référentiel ASP.NET Core.
ms.author: riande
ms.date: 01/31/2019
uid: security/authentication/samples
ms.openlocfilehash: 3d7e28f6e501bd8bd3908ca4b314a63cee52ebe3
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828670"
---
# <a name="authentication-samples-for-aspnet-core"></a><span data-ttu-id="c557a-103">Exemples d’authentification pour ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c557a-103">Authentication samples for ASP.NET Core</span></span>

<span data-ttu-id="c557a-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c557a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c557a-105">Le [référentiel ASP.net Core](https://github.com/dotnet/AspNetCore) contient les exemples d’authentification suivants dans le dossier *AspNetCore/SRC/Security/Samples* :</span><span class="sxs-lookup"><span data-stu-id="c557a-105">The [ASP.NET Core repository](https://github.com/dotnet/AspNetCore) contains the following authentication samples in the *AspNetCore/src/Security/samples* folder:</span></span>

* [<span data-ttu-id="c557a-106">Transformation des revendications</span><span class="sxs-lookup"><span data-stu-id="c557a-106">Claims transformation</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/ClaimsTransformation)
* [<span data-ttu-id="c557a-107">Authentification par cookie</span><span class="sxs-lookup"><span data-stu-id="c557a-107">Cookie authentication</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/Cookies)
* [<span data-ttu-id="c557a-108">Fournisseur de stratégie personnalisée-IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="c557a-108">Custom policy provider - IAuthorizationPolicyProvider</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/CustomPolicyProvider)
* [<span data-ttu-id="c557a-109">Options et schémas d’authentification dynamique</span><span class="sxs-lookup"><span data-stu-id="c557a-109">Dynamic authentication schemes and options</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/DynamicSchemes)
* [<span data-ttu-id="c557a-110">Revendications externes</span><span class="sxs-lookup"><span data-stu-id="c557a-110">External claims</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/Identity.ExternalClaims)
* [<span data-ttu-id="c557a-111">Sélection entre un cookie et un autre schéma d’authentification en fonction de la demande</span><span class="sxs-lookup"><span data-stu-id="c557a-111">Selecting between cookie and another authentication scheme based on the request</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/PathSchemeSelection)
* [<span data-ttu-id="c557a-112">Restreint l’accès aux fichiers statiques</span><span class="sxs-lookup"><span data-stu-id="c557a-112">Restricts access to static files</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a><span data-ttu-id="c557a-113">Exécution des exemples</span><span class="sxs-lookup"><span data-stu-id="c557a-113">Run the samples</span></span>

* <span data-ttu-id="c557a-114">Sélectionnez une [branche](https://github.com/dotnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="c557a-114">Select a [branch](https://github.com/dotnet/AspNetCore).</span></span> <span data-ttu-id="c557a-115">Par exemple, `Tag:v3.0.0`.</span><span class="sxs-lookup"><span data-stu-id="c557a-115">For example, `Tag:v3.0.0`</span></span>
* <span data-ttu-id="c557a-116">Clonez ou téléchargez le [référentiel ASP.net Core](https://github.com/dotnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="c557a-116">Clone or download the [ASP.NET Core repository](https://github.com/dotnet/AspNetCore).</span></span>
* <span data-ttu-id="c557a-117">Vérifiez que vous avez installé la version de [Kit SDK .net Core](https://www.microsoft.com/net/download/all) qui correspond au clone du référentiel ASP.net core.</span><span class="sxs-lookup"><span data-stu-id="c557a-117">Verify you have installed the [.NET Core SDK](https://www.microsoft.com/net/download/all) version matching the clone of the ASP.NET Core repository.</span></span>
* <span data-ttu-id="c557a-118">Accédez à un exemple dans *AspNetCore/SRC/Security/Samples* et exécutez l’exemple avec `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="c557a-118">Navigate to a sample in *AspNetCore/src/Security/samples* and run the sample with `dotnet run`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="c557a-119">Le [référentiel ASP.net Core](https://github.com/dotnet/AspNetCore) contient les exemples d’authentification suivants dans le dossier *AspNetCore/SRC/Security/Samples* :</span><span class="sxs-lookup"><span data-stu-id="c557a-119">The [ASP.NET Core repository](https://github.com/dotnet/AspNetCore) contains the following authentication samples in the *AspNetCore/src/Security/samples* folder:</span></span>

* [<span data-ttu-id="c557a-120">Transformation des revendications</span><span class="sxs-lookup"><span data-stu-id="c557a-120">Claims transformation</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [<span data-ttu-id="c557a-121">Authentification par cookie</span><span class="sxs-lookup"><span data-stu-id="c557a-121">Cookie authentication</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [<span data-ttu-id="c557a-122">Fournisseur de stratégie personnalisée-IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="c557a-122">Custom policy provider - IAuthorizationPolicyProvider</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [<span data-ttu-id="c557a-123">Options et schémas d’authentification dynamique</span><span class="sxs-lookup"><span data-stu-id="c557a-123">Dynamic authentication schemes and options</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [<span data-ttu-id="c557a-124">Revendications externes</span><span class="sxs-lookup"><span data-stu-id="c557a-124">External claims</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [<span data-ttu-id="c557a-125">Sélection entre un cookie et un autre schéma d’authentification en fonction de la demande</span><span class="sxs-lookup"><span data-stu-id="c557a-125">Selecting between cookie and another authentication scheme based on the request</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [<span data-ttu-id="c557a-126">Restreint l’accès aux fichiers statiques</span><span class="sxs-lookup"><span data-stu-id="c557a-126">Restricts access to static files</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a><span data-ttu-id="c557a-127">Exécution des exemples</span><span class="sxs-lookup"><span data-stu-id="c557a-127">Run the samples</span></span>

* <span data-ttu-id="c557a-128">Sélectionnez une [branche](https://github.com/dotnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="c557a-128">Select a [branch](https://github.com/dotnet/AspNetCore).</span></span> <span data-ttu-id="c557a-129">Par exemple, `release/2.2`.</span><span class="sxs-lookup"><span data-stu-id="c557a-129">For example, `release/2.2`</span></span>
* <span data-ttu-id="c557a-130">Clonez ou téléchargez le [référentiel ASP.net Core](https://github.com/dotnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="c557a-130">Clone or download the [ASP.NET Core repository](https://github.com/dotnet/AspNetCore).</span></span>
* <span data-ttu-id="c557a-131">Vérifiez que vous avez installé la version de [Kit SDK .net Core](https://www.microsoft.com/net/download/all) qui correspond au clone du référentiel ASP.net core.</span><span class="sxs-lookup"><span data-stu-id="c557a-131">Verify you have installed the [.NET Core SDK](https://www.microsoft.com/net/download/all) version matching the clone of the ASP.NET Core repository.</span></span>
* <span data-ttu-id="c557a-132">Accédez à un exemple dans *AspNetCore/SRC/Security/Samples* et exécutez l’exemple avec `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="c557a-132">Navigate to a sample in *AspNetCore/src/Security/samples* and run the sample with `dotnet run`.</span></span>

::: moniker-end
