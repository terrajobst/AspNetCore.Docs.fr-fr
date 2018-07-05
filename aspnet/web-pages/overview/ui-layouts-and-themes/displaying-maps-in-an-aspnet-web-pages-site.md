---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: Affichage des mappages dans un site ASP.NET Web Pages (Razor) Site | Microsoft Docs
author: tfitzmac
description: Cet article explique comment afficher des cartes interactives sur des pages dans un site Web de Pages Web ASP.NET (Razor) en fonction de mappage des services fournis par Bing, Google, Ma...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 210cc37427024a4e8cae309634141900bb28b55e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368833"
---
<a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="82b22-103">Affichage des mappages dans un Site ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="82b22-103">Displaying Maps in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="82b22-104">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="82b22-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="82b22-105">Cet article explique comment afficher des cartes interactives sur des pages dans un site Web de Pages Web ASP.NET (Razor) en fonction de mappage des services fournis par Bing, Google, MapQuest et Yahoo.</span><span class="sxs-lookup"><span data-stu-id="82b22-105">This article explains how to display interactive maps on pages in an ASP.NET Web Pages (Razor) website based on mapping services provided by Bing, Google, MapQuest, and Yahoo.</span></span>
> 
> <span data-ttu-id="82b22-106">Ce que vous allez apprendre :</span><span class="sxs-lookup"><span data-stu-id="82b22-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="82b22-107">Comment générer une table basée sur une adresse.</span><span class="sxs-lookup"><span data-stu-id="82b22-107">How to generate a map based on an address.</span></span>
> - <span data-ttu-id="82b22-108">Comment générer une table basée sur des coordonnées de latitude et longitude.</span><span class="sxs-lookup"><span data-stu-id="82b22-108">How to generate a map based on latitude and longitude coordinates.</span></span>
> - <span data-ttu-id="82b22-109">Comment inscrire un compte de développeur de Bing Maps et d’obtenir une clé à utiliser avec Bing Maps.</span><span class="sxs-lookup"><span data-stu-id="82b22-109">How to register a Bing Maps Developer Account and get a key to use with Bing Maps.</span></span>
> 
> <span data-ttu-id="82b22-110">Il s’agit de la fonctionnalité d’ASP.NET introduite dans l’article :</span><span class="sxs-lookup"><span data-stu-id="82b22-110">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="82b22-111">Le `Maps` helper.</span><span class="sxs-lookup"><span data-stu-id="82b22-111">The `Maps` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="82b22-112">Versions des logiciels utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="82b22-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="82b22-113">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="82b22-113">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="82b22-114">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="82b22-114">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="82b22-115">Ce didacticiel fonctionne également avec WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="82b22-115">This tutorial also works with WebMatrix 3.</span></span>


<span data-ttu-id="82b22-116">Dans les Pages Web, vous pouvez afficher les mappages sur une page à l’aide de `Maps` helper.</span><span class="sxs-lookup"><span data-stu-id="82b22-116">In Web Pages, you can display maps on a page by using `Maps` helper.</span></span> <span data-ttu-id="82b22-117">Vous pouvez générer des mappages en vous basés sur une adresse ou sur un jeu de coordonnées de longitude et latitude.</span><span class="sxs-lookup"><span data-stu-id="82b22-117">You can generate maps based either on an address or on a set of longitude and latitude coordinates.</span></span> <span data-ttu-id="82b22-118">Le `Maps` classe vous permet d’appeler dans les moteurs de carte populaires, y compris de Bing, Google, MapQuest et Yahoo.</span><span class="sxs-lookup"><span data-stu-id="82b22-118">The `Maps` class lets you call into popular map engines including Bing, Google, MapQuest, and Yahoo.</span></span>

<span data-ttu-id="82b22-119">Les étapes d’ajout de mappage à une page sont les mêmes quel que soit les moteurs de carte que vous appelez.</span><span class="sxs-lookup"><span data-stu-id="82b22-119">The steps for adding mapping to a page are the same regardless of which of the map engines you call.</span></span> <span data-ttu-id="82b22-120">Vous ajoutez simplement une référence de fichier JavaScript qui rend les méthodes disponibles pour afficher la carte, puis vous appelez les méthodes de la `Maps` helper.</span><span class="sxs-lookup"><span data-stu-id="82b22-120">You just add a JavaScript file reference that makes available methods to display the map, and then you call methods of the `Maps` helper.</span></span>

<span data-ttu-id="82b22-121">Vous choisissez une carte de service en fonction duquel `Maps` méthode d’assistance que vous utilisez.</span><span class="sxs-lookup"><span data-stu-id="82b22-121">You choose a map service based on which `Maps` helper method you use.</span></span> <span data-ttu-id="82b22-122">Vous pouvez utiliser une de ces :</span><span class="sxs-lookup"><span data-stu-id="82b22-122">You can use any of these:</span></span>

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a><span data-ttu-id="82b22-123">Installer les éléments que vous avez besoin</span><span class="sxs-lookup"><span data-stu-id="82b22-123">Installing the Pieces You Need</span></span>

<span data-ttu-id="82b22-124">Pour afficher les mappages, vous avez besoin de ces éléments :</span><span class="sxs-lookup"><span data-stu-id="82b22-124">To display maps, you need these pieces:</span></span>

- <span data-ttu-id="82b22-125">Le `Maps` helper.</span><span class="sxs-lookup"><span data-stu-id="82b22-125">The `Maps` helper.</span></span> <span data-ttu-id="82b22-126">Ce programme d’assistance est dans la version 2 de la bibliothèque de programmes d’assistance de Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="82b22-126">This helper is in version 2 of the ASP.NET Web Helpers Library.</span></span> <span data-ttu-id="82b22-127">Si vous n’avez pas déjà ajouté la bibliothèque, vous pouvez l’installer dans votre site sous forme de package NuGet.</span><span class="sxs-lookup"><span data-stu-id="82b22-127">If you haven't already added the library, you can install it in your site as a NuGet package.</span></span> <span data-ttu-id="82b22-128">Pour plus d’informations, consultez [l’installation des programmes d’assistance dans un Site ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372).</span><span class="sxs-lookup"><span data-stu-id="82b22-128">For details, see [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372).</span></span> <span data-ttu-id="82b22-129">(Dans la galerie, recherchez le `microsoft-web-helpers` package.)</span><span class="sxs-lookup"><span data-stu-id="82b22-129">(In the Gallery, search for the `microsoft-web-helpers` package.)</span></span>
- <span data-ttu-id="82b22-130">La bibliothèque jQuery.</span><span class="sxs-lookup"><span data-stu-id="82b22-130">The jQuery library.</span></span> <span data-ttu-id="82b22-131">Plusieurs des modèles de site WebMatrix incluent déjà des bibliothèques jQuery dans leurs *Script* dossiers.</span><span class="sxs-lookup"><span data-stu-id="82b22-131">Several of the WebMatrix site templates already include jQuery libraries in their *Script* folders.</span></span> <span data-ttu-id="82b22-132">Si vous n’avez pas ces bibliothèques, vous pouvez télécharger la dernière bibliothèque jQuery directement à partir de la [jQuery.org](http://jQuery.org) site.</span><span class="sxs-lookup"><span data-stu-id="82b22-132">If you do not have these libraries, you can download the latest jQuery library directly from the [jQuery.org](http://jQuery.org) site.</span></span> <span data-ttu-id="82b22-133">Ou vous pouvez créer un nouveau site à l’aide d’un modèle (par exemple, le **Starter Site** modèle) puis copiez les fichiers de jQuery à partir de ce site dans votre site actuel.</span><span class="sxs-lookup"><span data-stu-id="82b22-133">Or you can create a new site using a template (for example, the **Starter Site** template) and then copy the jQuery files from that site to your current site.</span></span>

<span data-ttu-id="82b22-134">Enfin, si vous souhaitez utiliser Bing maps, vous devez tout d’abord créer un compte (gratuit) et obtenir une clé.</span><span class="sxs-lookup"><span data-stu-id="82b22-134">Finally, if you want to use Bing maps, you must first create a (free) account and get a key.</span></span> <span data-ttu-id="82b22-135">Pour obtenir une clé, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="82b22-135">To get a key, follow these steps:</span></span>

1. <span data-ttu-id="82b22-136">Créer un compte sur le [compte de développeur de Bing Maps](https://www.microsoft.com/maps/developers/web.aspx).</span><span class="sxs-lookup"><span data-stu-id="82b22-136">Create an account on the [Bing Maps Developer Account](https://www.microsoft.com/maps/developers/web.aspx).</span></span> <span data-ttu-id="82b22-137">Vous devez disposer d’un compte de Microsoft (Windows Live ID) également.</span><span class="sxs-lookup"><span data-stu-id="82b22-137">You must have a Microsoft account (Windows Live ID) as well.</span></span>

    <span data-ttu-id="82b22-138">Vous pouvez spécifier que vous souhaitez utiliser la clé pour **/Test d’évaluation de**.</span><span class="sxs-lookup"><span data-stu-id="82b22-138">You can specify that you want to use the key for **Evaluation/Test**.</span></span> <span data-ttu-id="82b22-139">Si vous testez la fonction de mappage sur votre ordinateur à l’aide de WebMatrix et IIS Express, accédez la **Site** espace de travail et notez l’URL de votre site (par exemple, `http://localhost:50408`, bien que votre numéro de port sera probablement différente).</span><span class="sxs-lookup"><span data-stu-id="82b22-139">If you are testing the mapping function on your own computer using WebMatrix and IIS Express, go the **Site** workspace and note the URL of your site (for example, `http://localhost:50408`, although your port number will probably be different).</span></span> <span data-ttu-id="82b22-140">Vous pouvez utiliser cette *localhost* adresse que le site lorsque vous inscrivez.</span><span class="sxs-lookup"><span data-stu-id="82b22-140">You can use this *localhost* address as the site when you register.</span></span>
2. <span data-ttu-id="82b22-141">Une fois que vous êtes inscrit pour un compte, accédez au centre des comptes Bing Maps, puis cliquez sur **créer ou afficher les clés**:</span><span class="sxs-lookup"><span data-stu-id="82b22-141">After you have registered for an account, go to the Bing Maps Account Center and click **Create or view keys**:</span></span>

    ![mappage-2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. <span data-ttu-id="82b22-143">Enregistrez la clé Bing crée.</span><span class="sxs-lookup"><span data-stu-id="82b22-143">Record the key that Bing creates.</span></span>

## <a name="creating-a-map-based-on-an-address-using-google"></a><span data-ttu-id="82b22-144">Création d’une table basée sur une adresse (à l’aide de Google)</span><span class="sxs-lookup"><span data-stu-id="82b22-144">Creating a Map Based on an Address (Using Google)</span></span>

<span data-ttu-id="82b22-145">L’exemple suivant montre comment créer une page qui affiche une table basée sur une adresse.</span><span class="sxs-lookup"><span data-stu-id="82b22-145">The following example shows how to create a page that renders a map based on an address.</span></span> <span data-ttu-id="82b22-146">Cet exemple montre comment utiliser Google Maps.</span><span class="sxs-lookup"><span data-stu-id="82b22-146">This example shows how to use Google Maps.</span></span>

1. <span data-ttu-id="82b22-147">Créez un fichier nommé *MapAddress.cshtml* à la racine du site.</span><span class="sxs-lookup"><span data-stu-id="82b22-147">Create a file named *MapAddress.cshtml* in the root of the site.</span></span> <span data-ttu-id="82b22-148">Cette page génère une table basée sur une adresse que vous lui transmettez.</span><span class="sxs-lookup"><span data-stu-id="82b22-148">This page will generate a map based on an address that you pass to it.</span></span>
2. <span data-ttu-id="82b22-149">Copiez le code suivant dans le fichier, en remplaçant le contenu existant.</span><span class="sxs-lookup"><span data-stu-id="82b22-149">Copy the following code into the file, overwriting the existing content.</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="82b22-150">Notez les fonctionnalités suivantes de la page :</span><span class="sxs-lookup"><span data-stu-id="82b22-150">Notice the following features of the page:</span></span>

    - <span data-ttu-id="82b22-151">Le `<script>` élément dans le `<head>` élément.</span><span class="sxs-lookup"><span data-stu-id="82b22-151">The `<script>` element in the `<head>` element.</span></span> <span data-ttu-id="82b22-152">Dans l’exemple, le `<script>` références de l’élément le *jquery-1.6.4.min.js* fichier, qui est une version réduite (compressée) de la bibliothèque jQuery, version 1.6.4.</span><span class="sxs-lookup"><span data-stu-id="82b22-152">In the example, the `<script>` element references the *jquery-1.6.4.min.js* file, which is a minified (compressed) version of the jQuery library, version 1.6.4.</span></span> <span data-ttu-id="82b22-153">Notez que la référence suppose que le *.js* fichier se trouve dans le *Scripts* dossier de votre site.</span><span class="sxs-lookup"><span data-stu-id="82b22-153">Note that the reference assumes that the *.js* file is in the *Scripts* folder of your site.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="82b22-154">Si vous utilisez une version différente de la bibliothèque jQuery, assurez-vous simplement que vous pointez vers cette version correctement.</span><span class="sxs-lookup"><span data-stu-id="82b22-154">If you're using a different version of the jQuery library, just make sure that you're pointing to that version correctly.</span></span>
    - <span data-ttu-id="82b22-155">L’appel à la `@Maps.GetGoogleHtml` dans le corps de la page.</span><span class="sxs-lookup"><span data-stu-id="82b22-155">The call to the `@Maps.GetGoogleHtml` in the body of the page.</span></span> <span data-ttu-id="82b22-156">Pour mapper une adresse, vous devez passer une chaîne d’adresse.</span><span class="sxs-lookup"><span data-stu-id="82b22-156">To map an address, you must pass an address string.</span></span> <span data-ttu-id="82b22-157">Les méthodes pour les autres moteurs de carte fonctionnent de manière similaire (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span><span class="sxs-lookup"><span data-stu-id="82b22-157">The methods for the other map engines work in a similar way (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span></span>
3. <span data-ttu-id="82b22-158">Exécutez la page et entrez une adresse.</span><span class="sxs-lookup"><span data-stu-id="82b22-158">Run the page and enter an address.</span></span> <span data-ttu-id="82b22-159">La page affiche une table, basée sur Google Maps, qui indique l’emplacement que vous avez spécifié.</span><span class="sxs-lookup"><span data-stu-id="82b22-159">The page displays a map, based on Google Maps, that shows the location that you specified.</span></span>

     ![mappage de-1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a><span data-ttu-id="82b22-161">Création d’une table basée sur une Latitude et Longitude coordonne (à l’aide de Bing)</span><span class="sxs-lookup"><span data-stu-id="82b22-161">Creating a Map Based on Latitude and Longitude Coordinates (Using Bing)</span></span>

<span data-ttu-id="82b22-162">Cet exemple montre comment créer une table basée sur des coordonnées.</span><span class="sxs-lookup"><span data-stu-id="82b22-162">This example shows how to create a map based on coordinates.</span></span> <span data-ttu-id="82b22-163">Cet exemple montre comment utiliser Bing maps et comment inclure votre clé Bing.</span><span class="sxs-lookup"><span data-stu-id="82b22-163">This example shows how to use Bing maps and how to include your Bing key.</span></span> <span data-ttu-id="82b22-164">(Vous pouvez créer une table basée sur des coordonnées à l’aide d’autres moteurs de carte également, sans utiliser une clé Bing).</span><span class="sxs-lookup"><span data-stu-id="82b22-164">(You can create a map based on coordinates using the other map engines also, without using a Bing key.)</span></span>

1. <span data-ttu-id="82b22-165">Créez un fichier nommé *MapCoordinates.cshtml* à la racine du site et remplacez le contenu existant avec le code et le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="82b22-165">Create a file named *MapCoordinates.cshtml* in the root of the site and replace the existing content with the following code and markup:</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. <span data-ttu-id="82b22-166">Remplacez `your-key-here` avec la clé Bing Maps que vous avez créé précédemment.</span><span class="sxs-lookup"><span data-stu-id="82b22-166">Replace `your-key-here` with the Bing Maps key that you generated earlier.</span></span>
3. <span data-ttu-id="82b22-167">Exécutez le *MapCoordinates.cshtml* page, entrez les coordonnées de latitude et longitude, puis cliquez sur le **carte !**</span><span class="sxs-lookup"><span data-stu-id="82b22-167">Run the *MapCoordinates.cshtml* page, enter latitude and longitude coordinates, and then click the **Map It!**</span></span> <span data-ttu-id="82b22-168">disproportionnée.</span><span class="sxs-lookup"><span data-stu-id="82b22-168">button.</span></span> <span data-ttu-id="82b22-169">(Si vous ne connaissez pas toutes les coordonnées, essayez ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="82b22-169">(If you don't know any coordinates, try the following.</span></span> <span data-ttu-id="82b22-170">C’est un emplacement sur le campus Microsoft Redmond).</span><span class="sxs-lookup"><span data-stu-id="82b22-170">This is a location on the Microsoft Redmond campus.)</span></span>

   - <span data-ttu-id="82b22-171">Latitude : 47.6781005859375</span><span class="sxs-lookup"><span data-stu-id="82b22-171">Latitude: 47.6781005859375</span></span>
   - <span data-ttu-id="82b22-172">Longitude :-122.158317565918</span><span class="sxs-lookup"><span data-stu-id="82b22-172">Longitude: -122.158317565918</span></span>

     <span data-ttu-id="82b22-173">La page s’affiche à l’aide des coordonnées que vous avez spécifié.</span><span class="sxs-lookup"><span data-stu-id="82b22-173">The page is displayed using the coordinates that you specified.</span></span>

     ![mappage-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="82b22-175">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="82b22-175">Additional Resources</span></span>


[<span data-ttu-id="82b22-176">Référence de l’API Microsoft.Maps</span><span class="sxs-lookup"><span data-stu-id="82b22-176">Microsoft.Maps API Reference</span></span>](https://msdn.microsoft.com/library/gg427611.aspx)
