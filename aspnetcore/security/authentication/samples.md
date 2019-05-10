---
title: Exemples d’authentification pour ASP.NET Core
author: rick-anderson
description: Fournit des liens vers les exemples d’authentification dans le référentiel d’ASP.NET Core.
ms.author: riande
ms.date: 01/31/2019
uid: security/authentication/samples
ms.openlocfilehash: 7b3c911d60ad4737ebd12ce6f7628ad624b11658
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897156"
---
# <a name="authentication-samples-for-aspnet-core"></a><span data-ttu-id="724d2-103">Exemples d’authentification pour ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="724d2-103">Authentication samples for ASP.NET Core</span></span>

<span data-ttu-id="724d2-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="724d2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="724d2-105">Le [référentiel d’ASP.NET Core](https://github.com/aspnet/AspNetCore) contient les exemples d’authentification suivants dans le *AspNetCore/src/sécurité/samples* dossier :</span><span class="sxs-lookup"><span data-stu-id="724d2-105">The [ASP.NET Core repository](https://github.com/aspnet/AspNetCore) contains the following authentication samples in the *AspNetCore/src/Security/samples* folder:</span></span>

* [<span data-ttu-id="724d2-106">Transformation des revendications</span><span class="sxs-lookup"><span data-stu-id="724d2-106">Claims transformation</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [<span data-ttu-id="724d2-107">Authentification des cookies</span><span class="sxs-lookup"><span data-stu-id="724d2-107">Cookie authentication</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [<span data-ttu-id="724d2-108">Fournisseur de stratégie personnalisée - IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="724d2-108">Custom policy provider - IAuthorizationPolicyProvider</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [<span data-ttu-id="724d2-109">Options et les schémas d’authentification dynamique</span><span class="sxs-lookup"><span data-stu-id="724d2-109">Dynamic authentication schemes and options</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [<span data-ttu-id="724d2-110">Revendications externes</span><span class="sxs-lookup"><span data-stu-id="724d2-110">External claims</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [<span data-ttu-id="724d2-111">En sélectionnant entre cookie et un autre schéma d’authentification en fonction de la demande</span><span class="sxs-lookup"><span data-stu-id="724d2-111">Selecting between cookie and another authentication scheme based on the request</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [<span data-ttu-id="724d2-112">Restreint l’accès aux fichiers statiques</span><span class="sxs-lookup"><span data-stu-id="724d2-112">Restricts access to static files</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a><span data-ttu-id="724d2-113">Exécuter les exemples</span><span class="sxs-lookup"><span data-stu-id="724d2-113">Run the samples</span></span>

* <span data-ttu-id="724d2-114">Sélectionnez un [branche](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="724d2-114">Select a [branch](https://github.com/aspnet/AspNetCore).</span></span> <span data-ttu-id="724d2-115">Par exemple, `release/2.2`.</span><span class="sxs-lookup"><span data-stu-id="724d2-115">For example, `release/2.2`</span></span>
* <span data-ttu-id="724d2-116">Clonez ou téléchargez le [référentiel d’ASP.NET Core](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="724d2-116">Clone or download the [ASP.NET Core repository](https://github.com/aspnet/AspNetCore).</span></span>
* <span data-ttu-id="724d2-117">Vérifiez que vous avez installé le [du SDK .NET Core](https://www.microsoft.com/net/download/all) version mise en correspondance le clone du référentiel ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="724d2-117">Verify you have installed the [.NET Core SDK](https://www.microsoft.com/net/download/all) version matching the clone of the ASP.NET Core repository.</span></span>
* <span data-ttu-id="724d2-118">Accédez à un exemple dans *AspNetCore/src/sécurité/samples* et exécuter l’exemple avec `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="724d2-118">Navigate to a sample in *AspNetCore/src/Security/samples* and run the sample with `dotnet run`.</span></span>
