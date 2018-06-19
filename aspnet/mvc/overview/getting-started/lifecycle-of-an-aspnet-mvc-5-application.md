---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: Cycle de vie d’une Application ASP.NET MVC 5 | Documents Microsoft
author: cephalin
description: Télécharger un document PDF que les graphiques du cycle de vie d’une application ASP.NET MVC 5. Ce document de cycle de vie fournit une vue d’ensemble du cycle de vie MVC un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/28/2014
ms.topic: article
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: 50d58d10c11677fa72ede6a03e686cbde4cbae1d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036490"
---
<a name="lifecycle-of-an-aspnet-mvc-5-application"></a><span data-ttu-id="c25ac-104">Cycle de vie d’une Application ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="c25ac-104">Lifecycle of an ASP.NET MVC 5 Application</span></span>
====================
<span data-ttu-id="c25ac-105">par [Cephas Lin](https://github.com/cephalin)</span><span class="sxs-lookup"><span data-stu-id="c25ac-105">by [Cephas Lin](https://github.com/cephalin)</span></span>

[<span data-ttu-id="c25ac-106">Téléchargez le Document PDF</span><span class="sxs-lookup"><span data-stu-id="c25ac-106">Download PDF Document</span></span>](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

<span data-ttu-id="c25ac-107">Ici, vous pouvez télécharger un document PDF graphiques le cycle de vie des applications ASP.NET MVC 5, à partir de réception HTTP demande d’envoi de la réponse HTTP au client.</span><span class="sxs-lookup"><span data-stu-id="c25ac-107">Here you can download a PDF document that charts the lifecycle of every ASP.NET MVC 5 application, from receiving the HTTP request to sending the HTTP response back to the client.</span></span> <span data-ttu-id="c25ac-108">Il est conçu comme un outil de formation pour ceux qui sont nouveaux pour ASP.NET MVC et également en tant que référence pour ceux qui ont besoin d’Explorer les aspects spécifiques de l’application.</span><span class="sxs-lookup"><span data-stu-id="c25ac-108">It is designed both as an educational tool for those who are new to ASP.NET MVC and also as a reference for those who need to drill into specific aspects of the application.</span></span> <span data-ttu-id="c25ac-109">Le document PDF présente les caractéristiques suivantes :</span><span class="sxs-lookup"><span data-stu-id="c25ac-109">The PDF document has the following features:</span></span>

- <span data-ttu-id="c25ac-110">Applique [HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) étapes pour vous aider à comprendre où MVC intègre les [cycle de vie des applications ASP.NET](https://msdn.microsoft.com/library/bb470252.aspx).</span><span class="sxs-lookup"><span data-stu-id="c25ac-110">Relevant [HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) stages to help you understand where MVC integrates into the [ASP.NET application lifecycle](https://msdn.microsoft.com/library/bb470252.aspx).</span></span>
- <span data-ttu-id="c25ac-111">Une vue d’ensemble du cycle de vie application MVC, où vous pouvez comprendre les étapes majeures que chaque application MVC traverse dans le pipeline de traitement de la requête.</span><span class="sxs-lookup"><span data-stu-id="c25ac-111">A high-level view of the MVC application lifecycle, where you can understand the major stages that every MVC application passes through in the request processing pipeline.</span></span>  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- <span data-ttu-id="c25ac-112">Un affichage des détails qui montre extrait vers le bas dans les détails du pipeline de traitement de la requête.</span><span class="sxs-lookup"><span data-stu-id="c25ac-112">A detail view that shows drills down into the details of the request processing pipeline.</span></span> <span data-ttu-id="c25ac-113">Vous pouvez comparer la vue d’ensemble et l’affichage des détails pour voir comment les détails des cycles de vie sont rassemblés dans les différentes étapes.</span><span class="sxs-lookup"><span data-stu-id="c25ac-113">You can compare the high-level view and the detail view to see how the lifecycles details are collected into the various stages.</span></span> <span data-ttu-id="c25ac-114">[Télécharger le PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) pour obtenir une vue agrandie.</span><span class="sxs-lookup"><span data-stu-id="c25ac-114">[Download PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) to see a larger view.</span></span>
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- <span data-ttu-id="c25ac-115">La sélection élective et l’objectif de toutes les méthodes substituables sur le [contrôleur](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) objet dans le pipeline de traitement de demande.</span><span class="sxs-lookup"><span data-stu-id="c25ac-115">Placement and purpose of all overridable methods on the [Controller](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) object in the request processing pipeline.</span></span> <span data-ttu-id="c25ac-116">Vous pouvez ou ne pouvez pas avoir la nécessité de remplacer n’importe quelle un méthode, mais il est important de comprendre leur rôle dans le cycle de vie d’application afin que vous pouvez écrire du code à l’étape du cycle de vie approprié pour l’effet que vous avez l’intention.</span><span class="sxs-lookup"><span data-stu-id="c25ac-116">You may or may not have the need to override any one method, but it is important for you to understand their role in the application lifecycle so that you can write code at the appropriate life cycle stage for the effect you intend.</span></span>
- <span data-ttu-id="c25ac-117">Diagrammes de haut dirigé montrant comment chacun des types de filtre (l’authentification, l’autorisation, action et résultats) est appelé.</span><span class="sxs-lookup"><span data-stu-id="c25ac-117">Blown-up diagrams showing how each of the filter types (authentication, authorization, action, and result) is invoked.</span></span>
- <span data-ttu-id="c25ac-118">Lier à un article utile ou un blog à partir de chaque point d’intérêt dans l’affichage des détails.</span><span class="sxs-lookup"><span data-stu-id="c25ac-118">Link to a useful article or blog from each point of interest in the detail view.</span></span>


## <a name="next-steps"></a><span data-ttu-id="c25ac-119">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="c25ac-119">Next Steps</span></span>

<span data-ttu-id="c25ac-120">Ce document répond à vos besoins ?</span><span class="sxs-lookup"><span data-stu-id="c25ac-120">Does this document meet your need?</span></span> <span data-ttu-id="c25ac-121">Nous aimerions connaître votre opinion.</span><span class="sxs-lookup"><span data-stu-id="c25ac-121">We'd appreciate your feedback.</span></span> <span data-ttu-id="c25ac-122">Si vous avez des questions sur le cycle de vie ASP.NET MVC dans votre application, [Stackoverflow](http://stackoverflow.com/help) et [forums d’ASP.NET MVC](https://forums.asp.net/1146.aspx) sont des emplacements formidables pour demander.</span><span class="sxs-lookup"><span data-stu-id="c25ac-122">If you have any question on the ASP.NET MVC lifecycle in your application, [Stackoverflow](http://stackoverflow.com/help) and the [ASP.NET MVC forums](https://forums.asp.net/1146.aspx) are great places to ask.</span></span> <span data-ttu-id="c25ac-123">Suivez [me](https://twitter.com/Cephas_MSFT) sur twitter, vous pouvez obtenir les mises à jour sur mon didacticiels plus récente.</span><span class="sxs-lookup"><span data-stu-id="c25ac-123">Follow [me](https://twitter.com/Cephas_MSFT) on twitter so you can get updates on my latest tutorials.</span></span>
