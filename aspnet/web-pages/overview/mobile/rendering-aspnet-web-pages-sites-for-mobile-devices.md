---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: Rendu de ASP.NET Web Pages (Razor) Sites pour les appareils mobiles | Documents Microsoft
author: tfitzmac
description: "Cet article décrit comment créer des pages dans un site ASP.NET Web Pages (Razor) qui apparaîtront correctement sur les appareils mobiles. Vous apprendrez : comment vous..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: 08b714eb2ffaefc7c7e2e5c9a7428106b231e5b7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a><span data-ttu-id="5b10a-104">Rendu des Sites Web ASP.NET (Razor) de Pages pour les appareils mobiles</span><span class="sxs-lookup"><span data-stu-id="5b10a-104">Rendering ASP.NET Web Pages (Razor) Sites for Mobile Devices</span></span>
====================
<span data-ttu-id="5b10a-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="5b10a-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="5b10a-106">Cet article décrit comment créer des pages dans un site ASP.NET Web Pages (Razor) qui apparaîtront correctement sur les appareils mobiles.</span><span class="sxs-lookup"><span data-stu-id="5b10a-106">This article describes how to create pages in an ASP.NET Web Pages (Razor) site that will render appropriately on mobile devices.</span></span>
> 
> <span data-ttu-id="5b10a-107">Ce que vous allez apprendre :</span><span class="sxs-lookup"><span data-stu-id="5b10a-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="5b10a-108">Comment utiliser une convention d’affectation de noms pour spécifier qu’une page est conçue spécifiquement pour les appareils mobiles.</span><span class="sxs-lookup"><span data-stu-id="5b10a-108">How to use a naming convention to specify that a page is designed specifically for mobile devices.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="5b10a-109">Versions du logiciel utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="5b10a-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="5b10a-110">Pages Web ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="5b10a-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="5b10a-111">Ce didacticiel fonctionne également avec ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="5b10a-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="5b10a-112">Les Pages Web ASP.NET vous permet de créer des affichages personnalisés pour restituer le contenu sur mobile ou d’autres périphériques.</span><span class="sxs-lookup"><span data-stu-id="5b10a-112">ASP.NET Web Pages lets you create custom displays for rendering content on mobile or other devices.</span></span>

<span data-ttu-id="5b10a-113">Le plus simple pour créer une page de périphérique spécifiques dans un site ASP.NET Web Pages est à l’aide d’un modèle d’affectation de noms de fichier comme celui-ci : *nom de fichier.* *Mobile**.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5b10a-113">The simplest way to create device-specific page in an ASP.NET Web Pages site is by using a file-naming pattern like this: *FileName.**Mobile**.cshtml*.</span></span> <span data-ttu-id="5b10a-114">Vous pouvez créer deux versions d’une page (par exemple, un nommé *MyFile.cshtml* et l’autre nommé *MyFile.Mobile.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="5b10a-114">You can create two versions of a page (for example, one named *MyFile.cshtml* and one named *MyFile.Mobile.cshtml*).</span></span> <span data-ttu-id="5b10a-115">À l’exécution, quand un appareil mobile demande *MyFile.cshtml*, ASP.NET restitue le contenu à partir de *MyFile.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5b10a-115">At run time, when a mobile device requests *MyFile.cshtml*, ASP.NET renders the content from *MyFile.Mobile.cshtml*.</span></span> <span data-ttu-id="5b10a-116">Dans le cas contraire, *MyFile.cshtml* est rendu.</span><span class="sxs-lookup"><span data-stu-id="5b10a-116">Otherwise, *MyFile.cshtml* is rendered.</span></span>

<span data-ttu-id="5b10a-117">L’exemple suivant montre comment activer le rendu mobile en ajoutant une page de contenu pour les appareils mobiles.</span><span class="sxs-lookup"><span data-stu-id="5b10a-117">The following example shows how to enable mobile rendering by adding a content page for mobile devices.</span></span> <span data-ttu-id="5b10a-118">*Page1.cshtml* contient le contenu ainsi qu’une barre latérale de navigation.</span><span class="sxs-lookup"><span data-stu-id="5b10a-118">*Page1.cshtml* contains content plus a navigation sidebar.</span></span> <span data-ttu-id="5b10a-119">*Page1.Mobile.cshtml* contient le même contenu, mais omet la barre latérale.</span><span class="sxs-lookup"><span data-stu-id="5b10a-119">*Page1.Mobile.cshtml* contains the same content, but omits the sidebar.</span></span>

1. <span data-ttu-id="5b10a-120">Dans un site ASP.NET Web Pages, créez un fichier nommé *Page1.cshtml* et remplacer le contenu actuel avec le balisage suivant.</span><span class="sxs-lookup"><span data-stu-id="5b10a-120">In an ASP.NET Web Pages site, create a file named *Page1.cshtml* and replace the current content with following markup.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. <span data-ttu-id="5b10a-121">Créez un fichier nommé *Page1.Mobile.cshtml* et remplacer le contenu existant par le balisage suivant.</span><span class="sxs-lookup"><span data-stu-id="5b10a-121">Create a file named *Page1.Mobile.cshtml* and replace the existing content with the following markup.</span></span> <span data-ttu-id="5b10a-122">Notez que la version mobile de la page omet la section de navigation pour optimiser le rendu sur un écran de petite taille.</span><span class="sxs-lookup"><span data-stu-id="5b10a-122">Notice that the mobile version of the page omits the navigation section for better rendering on a smaller screen.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. <span data-ttu-id="5b10a-123">Exécuter un navigateur et accédez à *Page1.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5b10a-123">Run a desktop browser and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="5b10a-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5b10a-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span></span>
4. <span data-ttu-id="5b10a-125">Exécuter un navigateur mobile (ou un émulateur d’appareil mobile) et accédez à *Page1.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5b10a-125">Run a mobile browser (or a mobile device emulator) and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="5b10a-126">(Notez que vous n’incluez pas *.mobile.*</span><span class="sxs-lookup"><span data-stu-id="5b10a-126">(Notice that you do not include *.mobile.*</span></span> <span data-ttu-id="5b10a-127">dans le cadre de l’URL). Même si la demande est de *Page1.cshtml*, ASP.NET restitue *Page1.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5b10a-127">as part of the URL.) Even though the request is to *Page1.cshtml*, ASP.NET renders *Page1.Mobile.cshtml*.</span></span>

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="5b10a-129">Pour tester les pages mobiles, vous pouvez utiliser un simulateur d’appareil mobile qui s’exécute sur un ordinateur de bureau.</span><span class="sxs-lookup"><span data-stu-id="5b10a-129">To test mobile pages, you can use a mobile device simulator that runs on a desktop computer.</span></span> <span data-ttu-id="5b10a-130">Cet outil vous permet de tester les pages web comme elles ressemblent sur les appareils mobiles (autrement dit, généralement avec un beaucoup plus petite afficher la zone).</span><span class="sxs-lookup"><span data-stu-id="5b10a-130">This tool lets you test web pages as they would look on mobile devices (that is, typically with a much smaller display area).</span></span> <span data-ttu-id="5b10a-131">Par exemple, un simulateur est la [un module complémentaire du sélecteur de l’Agent utilisateur](http://addons.mozilla.org/en-us/firefox/addon/user-agent-switcher/) pour Mozilla Firefox, qui vous permet d’émuler différents navigateurs des appareils mobiles à partir d’une version de bureau de Firefox.</span><span class="sxs-lookup"><span data-stu-id="5b10a-131">One example of a simulator is the [User Agent Switcher add-on](http://addons.mozilla.org/en-us/firefox/addon/user-agent-switcher/) for Mozilla Firefox, which lets you emulate various mobile browsers from a desktop version of Firefox.</span></span>


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="5b10a-132">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="5b10a-132">Additional Resources</span></span>


<span data-ttu-id="5b10a-133">[Émulateur de Windows Phone](https://msdn.microsoft.com/en-us/library/ff402563(v=VS.92).aspx)</span><span class="sxs-lookup"><span data-stu-id="5b10a-133">[Windows Phone Emulator](https://msdn.microsoft.com/en-us/library/ff402563(v=VS.92).aspx)</span></span>
