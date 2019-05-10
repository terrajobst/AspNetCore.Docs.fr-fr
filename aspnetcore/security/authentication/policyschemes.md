---
title: Jeux de stratégie dans ASP.NET Core
author: rick-anderson
description: Modèles de stratégie d’authentification facilitent l’ont un schéma d’authentification logique unique
ms.author: riande
ms.date: 2/28/2019
uid: security/authentication/policyschemes
ms.openlocfilehash: c310b61e14df2b7846e32a602bb75914a5850aff
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64895196"
---
# <a name="policy-schemes-in-aspnet-core"></a><span data-ttu-id="98318-103">Jeux de stratégie dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="98318-103">Policy schemes in ASP.NET Core</span></span>

<span data-ttu-id="98318-104">Modèles de stratégie d’authentification facilitent l’ont un schéma d’authentification logiques seule potentiellement utiliser plusieurs approches.</span><span class="sxs-lookup"><span data-stu-id="98318-104">Authentication policy schemes make it easier to have a single logical authentication scheme potentially use multiple approaches.</span></span> <span data-ttu-id="98318-105">Par exemple, un jeu de stratégie peut utiliser l’authentification Google pour défis et l’authentification des cookies pour tout le reste.</span><span class="sxs-lookup"><span data-stu-id="98318-105">For example, a policy scheme might use Google authentication for challenges, and cookie authentication for everything else.</span></span> <span data-ttu-id="98318-106">Les jeux de stratégie d’authentification permettent de :</span><span class="sxs-lookup"><span data-stu-id="98318-106">Authentication policy schemes make it:</span></span>

* <span data-ttu-id="98318-107">Très simple de transférer une action de l’authentification vers un autre schéma.</span><span class="sxs-lookup"><span data-stu-id="98318-107">Easy to forward any authentication action to another scheme.</span></span>
* <span data-ttu-id="98318-108">Avant de façon dynamique en fonction de la demande.</span><span class="sxs-lookup"><span data-stu-id="98318-108">Forward dynamically based on the request.</span></span>

<span data-ttu-id="98318-109">Tous les schémas d’authentification qui utilisez dérivés <xref:Microsoft.AspNetCore.Authentication.AuthenticationSchemeOptions> et associé [ `AuthenticationHandler<TOptions>` ](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1):</span><span class="sxs-lookup"><span data-stu-id="98318-109">All authentication schemes that use derived <xref:Microsoft.AspNetCore.Authentication.AuthenticationSchemeOptions> and the associated [`AuthenticationHandler<TOptions>`](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1):</span></span>

* <span data-ttu-id="98318-110">Sont automatiquement des jeux de stratégie dans ASP.NET Core 2.1 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="98318-110">Are automatically policy schemes in ASP.NET Core 2.1 and later.</span></span>
* <span data-ttu-id="98318-111">Peut être activée via la configuration des options de ce modèle.</span><span class="sxs-lookup"><span data-stu-id="98318-111">Can be enabled via configuring the scheme's options.</span></span>

[!code-csharp[sample](policyschemes/samples/AuthenticationSchemeOptions.cs?name=snippet)]

## <a name="examples"></a><span data-ttu-id="98318-112">Exemples</span><span class="sxs-lookup"><span data-stu-id="98318-112">Examples</span></span>

<span data-ttu-id="98318-113">L’exemple suivant montre un schéma de niveau supérieur qui combine les schémas de niveau inférieur.</span><span class="sxs-lookup"><span data-stu-id="98318-113">The following example shows a higher level scheme that combines lower level schemes.</span></span> <span data-ttu-id="98318-114">L’authentification Google est utilisée pour les défis, et l’authentification des cookies est utilisée pour tout le reste :</span><span class="sxs-lookup"><span data-stu-id="98318-114">Google authentication is used for challenges, and cookie authentication is used for everything else:</span></span>

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet1)]

<span data-ttu-id="98318-115">L’exemple suivant active la sélection dynamique de schémas de base de chaque demande.</span><span class="sxs-lookup"><span data-stu-id="98318-115">The following example enables dynamic selection of schemes on a per request basis.</span></span> <span data-ttu-id="98318-116">Autrement dit, comment combiner les cookies et l’authentification des API.</span><span class="sxs-lookup"><span data-stu-id="98318-116">That is, how to mix cookies and API authentication.</span></span>

 <!-- REVIEW, missing If set in public Func<HttpContext, string> ForwardDefaultSelector -->

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet2)]
