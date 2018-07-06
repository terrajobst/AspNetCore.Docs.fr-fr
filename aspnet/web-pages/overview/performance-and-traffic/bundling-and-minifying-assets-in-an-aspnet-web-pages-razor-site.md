---
uid: web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
title: Regroupement et minimisation des ressources dans une application Web Pages (Razor) Site | Microsoft Docs
author: microsoft
description: Regroupement et minimisation manières pour accélérer votre site. Regroupement de permet de combiner plusieurs fichiers JavaScript (.js) ou plusieurs styles CSS (...)
ms.author: aspnetcontent
ms.date: 06/21/2012
ms.assetid: 8906f1e9-4b66-4a03-8e8a-9e9debf8ed91
msc.legacyurl: /web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
msc.type: authoredcontent
ms.openlocfilehash: 5799c4a3ae1c36636e0e485361f0f55e62ec7ec7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813438"
---
<a name="bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="0f8b2-104">Regroupement et minimisation des ressources dans un Site ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="0f8b2-104">Bundling and Minifying Assets in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="0f8b2-105">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="0f8b2-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="0f8b2-106">Regroupement et minimisation manières pour accélérer votre site.</span><span class="sxs-lookup"><span data-stu-id="0f8b2-106">Bundling and minification are ways to make your site faster.</span></span> <span data-ttu-id="0f8b2-107">Regroupement vous permet de combiner plusieurs JavaScript (*.js*) fichiers ou la feuille de style en cascade multiples (*.css*) de sorte qu’ils peuvent être téléchargés comme une unité, plutôt qu’une à la fois.</span><span class="sxs-lookup"><span data-stu-id="0f8b2-107">Bundling lets you combine multiple JavaScript (*.js*) files or multiple cascading style sheet (*.css*) files so that they can be downloaded as a unit, rather than one at a time.</span></span> <span data-ttu-id="0f8b2-108">Minimisation resserre les espaces blancs et effectue d’autres types de compression pour rendre les fichiers téléchargés en tant que petit un éventuel.</span><span class="sxs-lookup"><span data-stu-id="0f8b2-108">Minification squeezes out whitespace and performs other types of compression to make the downloaded files as small a possible.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="0f8b2-109">La version RC de ASP.NET Web Pages 2 ne prend pas en charge regroupement et minimisation, car le package qui contient les éléments requis n’est pas encore disponible dans Microsoft WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="0f8b2-109">The RC release of ASP.NET Web Pages 2 does not support bundling and minification because the package that contains the required elements is not yet available in Microsoft WebMatrix.</span></span> <span data-ttu-id="0f8b2-110">Veuillez nous excuser pour ce désagrément.</span><span class="sxs-lookup"><span data-stu-id="0f8b2-110">We apologize for this inconvenience.</span></span> <span data-ttu-id="0f8b2-111">Le package est censé être disponible dans la version finale d’ASP.NET Web Pages 2 et WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="0f8b2-111">The package is expected to be available in the final release of ASP.NET Web Pages 2 and WebMatrix 2.</span></span>
