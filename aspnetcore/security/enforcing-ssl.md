---
title: "L’application HTTPS dans une application ASP.NET Core"
author: rick-anderson
description: "Montre comment exiger HTTPS/TLS dans un cœur d’ASP.NET application web."
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: dc320faf0048200412f131ea816f33f29ac023e1
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/02/2018
---
# <a name="enforcing-https-in-an-aspnet-core-app"></a><span data-ttu-id="30faf-103">L’application HTTPS dans une application ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="30faf-103">Enforcing HTTPS in an ASP.NET Core app</span></span>

<span data-ttu-id="30faf-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="30faf-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="30faf-105">Ce document montre comment :</span><span class="sxs-lookup"><span data-stu-id="30faf-105">This document shows how to:</span></span>

- <span data-ttu-id="30faf-106">Exiger HTTPS pour toutes les demandes.</span><span class="sxs-lookup"><span data-stu-id="30faf-106">Require HTTPS for all requests.</span></span>
- <span data-ttu-id="30faf-107">Rediriger toutes les demandes HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="30faf-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="30faf-108">Faire **pas** utiliser `RequireHttpsAttribute` sur les API Web qui reçoivent des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="30faf-108">Do **not** use `RequireHttpsAttribute` on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="30faf-109">`RequireHttpsAttribute` utilise les codes d’état HTTP pour rediriger les navigateurs de HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="30faf-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="30faf-110">Les clients d’API ne peuvent pas comprendre ou obéissent aux règles de redirections HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="30faf-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="30faf-111">Ces clients peuvent envoyer des informations sur HTTP.</span><span class="sxs-lookup"><span data-stu-id="30faf-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="30faf-112">API Web doit soit :</span><span class="sxs-lookup"><span data-stu-id="30faf-112">Web APIs should either:</span></span>
>
>* <span data-ttu-id="30faf-113">Pas écouter sur HTTP.</span><span class="sxs-lookup"><span data-stu-id="30faf-113">Not listen on HTTP.</span></span>
>* <span data-ttu-id="30faf-114">Fermer la connexion avec le code d’état 400 (demande incorrecte) et pas desservir la demande.</span><span class="sxs-lookup"><span data-stu-id="30faf-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

## <a name="require-https"></a><span data-ttu-id="30faf-115">Exiger HTTPS</span><span class="sxs-lookup"><span data-stu-id="30faf-115">Require HTTPS</span></span>

<span data-ttu-id="30faf-116">Le [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) est utilisée pour exiger HTTPS.</span><span class="sxs-lookup"><span data-stu-id="30faf-116">The [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) is used to require HTTPS.</span></span> <span data-ttu-id="30faf-117">`[RequireHttpsAttribute]` peut décorer contrôleurs ou méthodes, ou peut être appliqué globalement.</span><span class="sxs-lookup"><span data-stu-id="30faf-117">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="30faf-118">Pour appliquer l’attribut global, ajoutez le code suivant à la méthode `ConfigureServices` dans la classe `Startup`: </span><span class="sxs-lookup"><span data-stu-id="30faf-118">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="30faf-119">Le code en surbrillance précédent requiert que toutes les demandes utilisent `HTTPS`; par conséquent, les requêtes HTTP sont ignorés.</span><span class="sxs-lookup"><span data-stu-id="30faf-119">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="30faf-120">Le code en surbrillance suivant redirige toutes les demandes HTTP vers HTTPS :</span><span class="sxs-lookup"><span data-stu-id="30faf-120">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="30faf-121">Pour plus d’informations, consultez [intergiciel (middleware) réécriture d’URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="30faf-121">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="30faf-122">Nécessitant HTTPS globalement (`options.Filters.Add(new RequireHttpsAttribute());`) est une meilleure pratique de sécurité.</span><span class="sxs-lookup"><span data-stu-id="30faf-122">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="30faf-123">Appliquer le `[RequireHttps]` attribut à toutes les Pages de contrôleurs/Razor n’est pas aussi sécurisée comme nécessitant HTTPS globalement.</span><span class="sxs-lookup"><span data-stu-id="30faf-123">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="30faf-124">Vous ne pouvez pas garantir la `[RequireHttps]` attribut est appliqué lors de l’ajout de nouveaux contrôleurs et les Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="30faf-124">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>