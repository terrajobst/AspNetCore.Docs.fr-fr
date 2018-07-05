---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: Cycle de vie d’une Application ASP.NET MVC 5 | Microsoft Docs
author: cephalin
description: Télécharger un document PDF que les graphiques du cycle de vie d’une application ASP.NET MVC 5. Ce document de cycle de vie fournit une vue d’ensemble du cycle de vie MVC un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/28/2014
ms.topic: article
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: 98735f2e04bdd0f2fec19524e59f6272dbc4ca57
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393913"
---
<a name="lifecycle-of-an-aspnet-mvc-5-application"></a><span data-ttu-id="f9e01-104">Cycle de vie d’une Application ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="f9e01-104">Lifecycle of an ASP.NET MVC 5 Application</span></span>
====================
<span data-ttu-id="f9e01-105">par [Cephas Lin](https://github.com/cephalin)</span><span class="sxs-lookup"><span data-stu-id="f9e01-105">by [Cephas Lin](https://github.com/cephalin)</span></span>

[<span data-ttu-id="f9e01-106">Téléchargez le Document PDF</span><span class="sxs-lookup"><span data-stu-id="f9e01-106">Download PDF Document</span></span>](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

<span data-ttu-id="f9e01-107">Ici, vous pouvez télécharger un document PDF qui soutiennent les graphiques du cycle de vie de chaque application ASP.NET MVC 5, à partir de réception HTTP demande à l’envoi de la réponse HTTP au client.</span><span class="sxs-lookup"><span data-stu-id="f9e01-107">Here you can download a PDF document that charts the lifecycle of every ASP.NET MVC 5 application, from receiving the HTTP request to sending the HTTP response back to the client.</span></span> <span data-ttu-id="f9e01-108">Il est conçu comme un outil de formation pour ceux qui débutent avec ASP.NET MVC et également comme une référence pour ceux qui ont besoin pour Explorer les aspects spécifiques de l’application.</span><span class="sxs-lookup"><span data-stu-id="f9e01-108">It is designed both as an educational tool for those who are new to ASP.NET MVC and also as a reference for those who need to drill into specific aspects of the application.</span></span> <span data-ttu-id="f9e01-109">Le document PDF présente les caractéristiques suivantes :</span><span class="sxs-lookup"><span data-stu-id="f9e01-109">The PDF document has the following features:</span></span>

- <span data-ttu-id="f9e01-110">Pertinentes [HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) étapes pour vous aider à comprendre où MVC intègre le [le cycle de vie ASP.NET application](https://msdn.microsoft.com/library/bb470252.aspx).</span><span class="sxs-lookup"><span data-stu-id="f9e01-110">Relevant [HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) stages to help you understand where MVC integrates into the [ASP.NET application lifecycle](https://msdn.microsoft.com/library/bb470252.aspx).</span></span>
- <span data-ttu-id="f9e01-111">Une vue d’ensemble du cycle de vie d’application MVC, où vous pouvez comprendre les principales phases que chaque application MVC traverse dans le pipeline de traitement de la requête.</span><span class="sxs-lookup"><span data-stu-id="f9e01-111">A high-level view of the MVC application lifecycle, where you can understand the major stages that every MVC application passes through in the request processing pipeline.</span></span>  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- <span data-ttu-id="f9e01-112">Une vue de détail qui affiche des exercices vers le bas dans les détails du pipeline de traitement de la demande.</span><span class="sxs-lookup"><span data-stu-id="f9e01-112">A detail view that shows drills down into the details of the request processing pipeline.</span></span> <span data-ttu-id="f9e01-113">Vous pouvez comparer la vue d’ensemble et la vue de détail pour voir comment les détails de cycles de vie sont rassemblés dans les différentes étapes.</span><span class="sxs-lookup"><span data-stu-id="f9e01-113">You can compare the high-level view and the detail view to see how the lifecycles details are collected into the various stages.</span></span> <span data-ttu-id="f9e01-114">[Télécharger le PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) pour obtenir une vue agrandie.</span><span class="sxs-lookup"><span data-stu-id="f9e01-114">[Download PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) to see a larger view.</span></span>
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- <span data-ttu-id="f9e01-115">Positionnement et l’objectif de toutes les méthodes substituables sur la [contrôleur](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) objet dans le pipeline de traitement de demande.</span><span class="sxs-lookup"><span data-stu-id="f9e01-115">Placement and purpose of all overridable methods on the [Controller](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) object in the request processing pipeline.</span></span> <span data-ttu-id="f9e01-116">Vous pouvez ou peut-être pas la nécessité de remplacer n’importe quelle un méthode, mais il est important de comprendre leur rôle dans le cycle de vie d’application afin que vous pouvez écrire du code à l’étape de cycle de vie approprié à l’effet que vous avez l’intention.</span><span class="sxs-lookup"><span data-stu-id="f9e01-116">You may or may not have the need to override any one method, but it is important for you to understand their role in the application lifecycle so that you can write code at the appropriate life cycle stage for the effect you intend.</span></span>
- <span data-ttu-id="f9e01-117">Diagrammes de haut soufflé montrant comment chacun des types de filtre (authentification, autorisation, action et résultats) est appelée.</span><span class="sxs-lookup"><span data-stu-id="f9e01-117">Blown-up diagrams showing how each of the filter types (authentication, authorization, action, and result) is invoked.</span></span>
- <span data-ttu-id="f9e01-118">Lier à un article utile ou un blog à partir de chaque point d’intérêt dans la vue détail.</span><span class="sxs-lookup"><span data-stu-id="f9e01-118">Link to a useful article or blog from each point of interest in the detail view.</span></span>


## <a name="next-steps"></a><span data-ttu-id="f9e01-119">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="f9e01-119">Next Steps</span></span>

<span data-ttu-id="f9e01-120">Ce document répond à vos besoins ?</span><span class="sxs-lookup"><span data-stu-id="f9e01-120">Does this document meet your need?</span></span> <span data-ttu-id="f9e01-121">Nous aimerions connaître votre opinion.</span><span class="sxs-lookup"><span data-stu-id="f9e01-121">We'd appreciate your feedback.</span></span> <span data-ttu-id="f9e01-122">Si vous avez des questions sur le cycle de vie ASP.NET MVC dans votre application, [Stackoverflow](http://stackoverflow.com/help) et [forums d’ASP.NET MVC](https://forums.asp.net/1146.aspx) formidable pour poser.</span><span class="sxs-lookup"><span data-stu-id="f9e01-122">If you have any question on the ASP.NET MVC lifecycle in your application, [Stackoverflow](http://stackoverflow.com/help) and the [ASP.NET MVC forums](https://forums.asp.net/1146.aspx) are great places to ask.</span></span> <span data-ttu-id="f9e01-123">Suivez [me](https://twitter.com/Cephas_MSFT) sur twitter afin d’obtenir les mises à jour sur mon derniers didacticiels.</span><span class="sxs-lookup"><span data-stu-id="f9e01-123">Follow [me](https://twitter.com/Cephas_MSFT) on twitter so you can get updates on my latest tutorials.</span></span>
