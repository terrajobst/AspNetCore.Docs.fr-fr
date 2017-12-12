---
uid: web-forms/videos/net-4/routing/how-do-i-work-with-urls-in-aspnet-routing
title: "Comment faire Le travail avec des URL dans le routage ASP.NET ? | Microsoft Docs"
author: rick-anderson
description: "Dans cette vidéo, Chris Pels montre comment spécifier des URL dans un site web qui utilise le routage ASP.NET. Tout d’abord, un site web est créé et le routage est défini dans le GL...."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/15/2010
ms.topic: article
ms.assetid: 08f9d0a7-cfa0-4914-a672-8a64295d7ba8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/net-4/routing/how-do-i-work-with-urls-in-aspnet-routing
msc.type: video
ms.openlocfilehash: cf79019d52bbf34bc604ab6289fcc3c48533f40c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-work-with-urls-in-aspnet-routing"></a><span data-ttu-id="f5ed5-105">Comment faire Le travail avec des URL dans le routage ASP.NET ?</span><span class="sxs-lookup"><span data-stu-id="f5ed5-105">How Do I: Work with URLs in ASP.NET Routing?</span></span>
====================
<span data-ttu-id="f5ed5-106">par [Chris PEL](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="f5ed5-106">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="f5ed5-107">Dans cette vidéo, Chris Pels montre comment spécifier des URL dans un site web qui utilise le routage ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f5ed5-107">In this video Chris Pels shows how to specify URLs in a web site that utilizes ASP.NET routing.</span></span> <span data-ttu-id="f5ed5-108">Tout d’abord, un site web est créé et le routage est défini dans la classe d’Application globale (.asax).</span><span class="sxs-lookup"><span data-stu-id="f5ed5-108">First, a web site is created and routing is defined in the Global Application Class (.asax).</span></span> <span data-ttu-id="f5ed5-109">Ensuite, un exemple de page web est créé et une URL basée sur un itinéraire défini est ajoutée à la page à l’aide de le « codées en dur » approche standard, par exemple, « ~/Stats/Visitors ».</span><span class="sxs-lookup"><span data-stu-id="f5ed5-109">Next, a sample web page is created and a URL based upon a defined route is added to the page using the standard "hard coded" approach, e.g., "~/Stats/Visitors".</span></span> <span data-ttu-id="f5ed5-110">Un autre lien est ensuite ajouté à la page qui génère dynamiquement la même URL dans le balisage à l’aide de la méthode RouteValue qui accepte le nom d’itinéraire et les paramètres.</span><span class="sxs-lookup"><span data-stu-id="f5ed5-110">Another link is then added to the page which dynamically generates the same URL in markup using the RouteValue method which accepts the route name and parameters.</span></span> <span data-ttu-id="f5ed5-111">La même URL est, implémentée à l’aide de code au lieu du balisage directement dans la page.</span><span class="sxs-lookup"><span data-stu-id="f5ed5-111">The same URL is then implemented using code rather than markup directly in the page.</span></span> <span data-ttu-id="f5ed5-112">L’itinéraire d’origine et l’emplacement de la page physique sont puis modifiés, ce qui n’est plus le lien codé en dur utilisation tandis que les deux générées de façon dynamique des liens (fonction) correctement.</span><span class="sxs-lookup"><span data-stu-id="f5ed5-112">The original route and physical page location are then changed, resulting in the hard coded link no longer working whereas both dynamically generated links function properly.</span></span> <span data-ttu-id="f5ed5-113">Enfin, la valeur de liens générés de manière dynamique vous trouverez ensuite.</span><span class="sxs-lookup"><span data-stu-id="f5ed5-113">Finally, the value of dynamically generated links is then discussed.</span></span>

[<span data-ttu-id="f5ed5-114">&#9654; Regardez la vidéo (20 minutes)</span><span class="sxs-lookup"><span data-stu-id="f5ed5-114">&#9654; Watch video (20 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-work-with-urls-in-aspnet-routing)

>[!div class="step-by-step"]
[<span data-ttu-id="f5ed5-115">Précédent</span><span class="sxs-lookup"><span data-stu-id="f5ed5-115">Previous</span></span>](how-do-i-use-routing-with-aspnet-web-forms.md)
