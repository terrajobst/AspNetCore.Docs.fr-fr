---
uid: web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
title: "Groupement et réduction des ressources dans une application Web Pages (Razor) Site | Documents Microsoft"
author: microsoft
description: "Regroupement et la minimisation manières pour accélérer votre site. Regroupement vous permet de combiner plusieurs fichiers JavaScript (.js) ou plusieurs styles CSS (...)"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/21/2012
ms.topic: article
ms.assetid: 8906f1e9-4b66-4a03-8e8a-9e9debf8ed91
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
msc.type: authoredcontent
ms.openlocfilehash: df00b5c9e741e429011143d04121df97c1930602
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="c6c2e-104">Groupement et réduction des ressources dans un Site de Web Pages (Razor) ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c6c2e-104">Bundling and Minifying Assets in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="c6c2e-105">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="c6c2e-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="c6c2e-106">Regroupement et la minimisation manières pour accélérer votre site.</span><span class="sxs-lookup"><span data-stu-id="c6c2e-106">Bundling and minification are ways to make your site faster.</span></span> <span data-ttu-id="c6c2e-107">Regroupement vous permet de combiner plusieurs JavaScript (*.js*) fichiers ou la feuille de style en cascade multiples (*.css*) de sorte qu’ils peuvent être téléchargés en tant qu’unité, plutôt qu’une à la fois.</span><span class="sxs-lookup"><span data-stu-id="c6c2e-107">Bundling lets you combine multiple JavaScript (*.js*) files or multiple cascading style sheet (*.css*) files so that they can be downloaded as a unit, rather than one at a time.</span></span> <span data-ttu-id="c6c2e-108">Minimisation resserre les espaces blancs et effectue d’autres types de compression pour rendre les fichiers téléchargés en tant que petite possible.</span><span class="sxs-lookup"><span data-stu-id="c6c2e-108">Minification squeezes out whitespace and performs other types of compression to make the downloaded files as small a possible.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="c6c2e-109">La version RC d’ASP.NET Web Pages 2 ne prend pas en charge groupement et la minimisation, car le package qui contient les éléments requis n’est pas disponible dans Microsoft WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="c6c2e-109">The RC release of ASP.NET Web Pages 2 does not support bundling and minification because the package that contains the required elements is not yet available in Microsoft WebMatrix.</span></span> <span data-ttu-id="c6c2e-110">Veuillez nous excuser pour ce désagrément.</span><span class="sxs-lookup"><span data-stu-id="c6c2e-110">We apologize for this inconvenience.</span></span> <span data-ttu-id="c6c2e-111">Le package est censé être disponibles dans la version finale d’ASP.NET Web Pages 2 et WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="c6c2e-111">The package is expected to be available in the final release of ASP.NET Web Pages 2 and WebMatrix 2.</span></span>
