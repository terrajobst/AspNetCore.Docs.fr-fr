---
title: Application de SSL dans une application ASP.NET Core
author: rick-anderson
description: "Montre comment exiger le protocole SSL dans un cœur d’ASP.NET web app"
ms.author: riande
manager: wpickett
ms.date: 07/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/enforcing-ssl
ms.openlocfilehash: f248e9c0463cf4a46a447a9c896b3276a50f5f08
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a><span data-ttu-id="5042e-103">Application de SSL dans une application ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5042e-103">Enforcing SSL in an ASP.NET Core app</span></span>

<span data-ttu-id="5042e-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5042e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5042e-105">Ce document montre comment :</span><span class="sxs-lookup"><span data-stu-id="5042e-105">This document shows how to:</span></span>

- <span data-ttu-id="5042e-106">Exiger le protocole SSL pour toutes les demandes (uniquement pour les requêtes HTTPS).</span><span class="sxs-lookup"><span data-stu-id="5042e-106">Require SSL for all requests (HTTPS requests only).</span></span>
- <span data-ttu-id="5042e-107">Rediriger toutes les demandes HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="5042e-107">Redirect all HTTP requests to HTTPS.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="5042e-108">Exiger SSL</span><span class="sxs-lookup"><span data-stu-id="5042e-108">Require SSL</span></span>

<span data-ttu-id="5042e-109">Le [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) est utilisée pour exiger SSL.</span><span class="sxs-lookup"><span data-stu-id="5042e-109">The [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require SSL.</span></span> <span data-ttu-id="5042e-110">Vous pouvez la décorer contrôleurs ou méthodes avec cet attribut, ou vous pouvez l’appliquer globalement, comme indiqué ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="5042e-110">You can decorate controllers or methods with this attribute or you can apply it globally as shown below:</span></span>

<span data-ttu-id="5042e-111">Ajoutez le code suivant à `ConfigureServices` dans `Startup`:</span><span class="sxs-lookup"><span data-stu-id="5042e-111">Add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]

<span data-ttu-id="5042e-112">Le code en surbrillance ci-dessus nécessite l’utilisation de toutes les demandes `HTTPS`, par conséquent, les requêtes HTTP sont ignorés.</span><span class="sxs-lookup"><span data-stu-id="5042e-112">The highlighted code above requires all requests use `HTTPS`, therefore HTTP requests are ignored.</span></span> <span data-ttu-id="5042e-113">Le code en surbrillance suivant redirige toutes les demandes HTTP vers HTTPS :</span><span class="sxs-lookup"><span data-stu-id="5042e-113">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]

<span data-ttu-id="5042e-114">Consultez [intergiciel (middleware) réécriture d’URL](xref:fundamentals/url-rewriting) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="5042e-114">See [URL Rewriting Middleware](xref:fundamentals/url-rewriting) for more information.</span></span>

<span data-ttu-id="5042e-115">Nécessitant HTTPS globalement (`options.Filters.Add(new RequireHttpsAttribute());`) est une meilleure pratique de sécurité.</span><span class="sxs-lookup"><span data-stu-id="5042e-115">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="5042e-116">Appliquer le `[RequireHttps]` attribut à tous les contrôleur n’est pas aussi sécurisée comme nécessitant HTTPS globalement.</span><span class="sxs-lookup"><span data-stu-id="5042e-116">Applying the `[RequireHttps]` attribute to all controller isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="5042e-117">Vous ne pouvez pas garantir la sécurité ajoutées à votre application de nouveaux contrôleurs penser à appliquer le `[RequireHttps]` attribut.</span><span class="sxs-lookup"><span data-stu-id="5042e-117">You can't guarantee new controllers added to your app will remember to apply the `[RequireHttps]` attribute.</span></span>
