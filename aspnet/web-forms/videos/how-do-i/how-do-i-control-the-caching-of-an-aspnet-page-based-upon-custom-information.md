---
uid: web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
title: '[Comment faire] Contrôle la mise en cache d’une Page ASP.NET en fonction des informations personnalisées | Documents Microsoft'
author: rick-anderson
description: Dans cette Chris Pels vidéo montre comment contrôler les critères de mise en cache d’une page ASP.NET en fonction des informations personnalisées. Un exemple de page est créée et ensuite l’o...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/19/2009
ms.topic: article
ms.assetid: f230c316-1313-4b8f-967c-62f9684fe378
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
msc.type: video
ms.openlocfilehash: b785de4e1161ae82ee458148aee4b30820801147
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26528128"
---
<a name="how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information"></a><span data-ttu-id="2412c-104">[Comment faire] Contrôle la mise en cache d’une Page ASP.NET en fonction des informations personnalisées</span><span class="sxs-lookup"><span data-stu-id="2412c-104">[How Do I:] Control the Caching of an ASP.NET Page Based Upon Custom Information</span></span>
====================
<span data-ttu-id="2412c-105">par [Chris PEL](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="2412c-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="2412c-106">Dans cette Chris Pels vidéo montre comment contrôler les critères de mise en cache d’une page ASP.NET en fonction des informations personnalisées.</span><span class="sxs-lookup"><span data-stu-id="2412c-106">In this video Chris Pels shows how to control the criteria for caching an ASP.NET page based upon custom information.</span></span> <span data-ttu-id="2412c-107">Un exemple de page est créé et ensuite la directive OutputCache est utilisée avec l’attribut VaryByCustom qui contient une valeur personnalisée.</span><span class="sxs-lookup"><span data-stu-id="2412c-107">A sample page is created and then the OutputCache directive is used with the VaryByCustom attribute which contains a custom value.</span></span> <span data-ttu-id="2412c-108">Ensuite, la méthode GetVaryCustomByString() est remplacée dans le module global.asax qui fournit la gestion de l’attribut personnalisé.</span><span class="sxs-lookup"><span data-stu-id="2412c-108">Next, the GetVaryCustomByString() method is overridden in the global.asax module which provides the handling of the custom attribute.</span></span> <span data-ttu-id="2412c-109">Dans cette méthode, une chaîne est retournée qui identifie de façon unique la version mise en cache de la page.</span><span class="sxs-lookup"><span data-stu-id="2412c-109">In that method a string is returned that uniquely identifies the cached version of the page.</span></span> <span data-ttu-id="2412c-110">Enfin, il existe une discussion à propos de la façon dont la mise en cache à l’aide d’une valeur personnalisée peut servir de plusieurs façons pour un site web.</span><span class="sxs-lookup"><span data-stu-id="2412c-110">Finally, there is a discussion about how caching using a custom value can be used in several ways for a web site.</span></span>

[<span data-ttu-id="2412c-111">&#9654; Regardez la vidéo (12 minutes)</span><span class="sxs-lookup"><span data-stu-id="2412c-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information)
