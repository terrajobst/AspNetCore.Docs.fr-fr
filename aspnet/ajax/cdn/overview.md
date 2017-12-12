---
uid: ajax/cdn/overview
title: "Réseau de diffusion de contenu Microsoft Ajax | Documents Microsoft"
author: rick-anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/14/2017
ms.topic: article
ms.assetid: 8935bf14-ca6d-4a4e-9dbe-b96ce74cef49
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /ajax/cdn
msc.type: content
ms.openlocfilehash: 2955888bc745fa3d49e40d364196283f16ad95ef
ms.sourcegitcommit: 9ecd4e9fb0c40c3693dab079eab1ff94b461c922
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2017
---
<a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="5b11c-102">Réseau de diffusion de contenu Microsoft Ajax</span><span class="sxs-lookup"><span data-stu-id="5b11c-102">Microsoft Ajax Content Delivery Network</span></span>
====================
<span data-ttu-id="5b11c-103">Remarque : Le CDN Microsoft Ajax n’a aucun contrat SLA au-dessus et au-delà de l’aide d’un CDN Azure.</span><span class="sxs-lookup"><span data-stu-id="5b11c-103">Note: The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="5b11c-104">Sommaire</span><span class="sxs-lookup"><span data-stu-id="5b11c-104">Table of Contents</span></span>

<span data-ttu-id="5b11c-105">**[AJAX.Microsoft.com renommé ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="5b11c-105">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="5b11c-106">**[Prise en charge de Visual Studio .vsdoc](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="5b11c-106">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="5b11c-107">**[À l’aide d’ASP.NET Ajax du CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="5b11c-107">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="5b11c-108">**[À l’aide de jQuery du CDN](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="5b11c-108">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="5b11c-109">**[À l’aide de jQuery UI du CDN](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="5b11c-109">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="5b11c-110">**[Fichiers de tiers dans le CDN](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="5b11c-110">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="5b11c-111">Versions de jQuery sur le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-111">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="5b11c-112">jQuery migrer des versions sur le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-112">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="5b11c-113">jQuery UI mises en production sur le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-113">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="5b11c-114">jQuery Validation mises en production sur le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-114">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="5b11c-115">jQuery Mobile mises en production sur le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-115">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="5b11c-116">jQuery versions de modèles sur le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-116">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="5b11c-117">jQuery Cycle des versions sur le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-117">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="5b11c-118">jQuery versions de tables de données sur le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-118">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="5b11c-119">Versions de Modernizr sur le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-119">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="5b11c-120">Versions de JSHint sur le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-120">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="5b11c-121">Versions de Knockout sur le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-121">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="5b11c-122">Globaliser des versions sur le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-122">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="5b11c-123">Répondre versions sur le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-123">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="5b11c-124">Versions d’amorçage sur le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-124">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="5b11c-125">Versions TouchCarousel d’amorçage sur le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-125">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="5b11c-126">Versions de Hammer.js sur le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-126">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="5b11c-127">Web Forms ASP.NET et Ajax mises en production sur le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-127">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="5b11c-128">ASP.NET MVC publie le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-128">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="5b11c-129">ASP.NET SignalR publie le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-129">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="5b11c-130">Le contenu remise réseau CDN Microsoft Ajax () héberge des bibliothèques JavaScript de tiers populaires tels que jQuery et vous permet de facilement les ajouter à vos applications Web.</span><span class="sxs-lookup"><span data-stu-id="5b11c-130">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="5b11c-131">Par exemple, commencer à l’aide de jQuery, qui est hébergé sur ce CDN en ajoutant simplement un &lt;script&gt; balise à votre page qui pointe vers ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="5b11c-131">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="5b11c-132">En tirant parti du CDN, vous pouvez considérablement améliorer les performances de vos applications Ajax.</span><span class="sxs-lookup"><span data-stu-id="5b11c-132">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="5b11c-133">Le contenu du CDN est mis en cache sur les serveurs situés dans le monde entier.</span><span class="sxs-lookup"><span data-stu-id="5b11c-133">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="5b11c-134">En outre, le CDN permet aux navigateurs de réutiliser des fichiers JavaScript mis en cache tiers pour les sites web qui se trouvent dans des domaines différents.</span><span class="sxs-lookup"><span data-stu-id="5b11c-134">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="5b11c-135">Le CDN prend en charge SSL (HTTPS) au cas où vous deviez traiter une page web à l’aide de Secure Sockets Layer.</span><span class="sxs-lookup"><span data-stu-id="5b11c-135">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="5b11c-136">Le CDN héberge les bibliothèques de scripts tiers suivants qui ont été téléchargés et sont concédés sous licence, par les propriétaires de ces bibliothèques :</span><span class="sxs-lookup"><span data-stu-id="5b11c-136">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="5b11c-137">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="5b11c-137">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="5b11c-138">jQuery UI (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="5b11c-138">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="5b11c-139">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="5b11c-139">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="5b11c-140">jQuery Validation (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="5b11c-140">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="5b11c-141">jQuery Cycle (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="5b11c-141">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="5b11c-142">jQuery DataTables (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="5b11c-142">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="5b11c-143">Le CDN Microsoft Ajax inclut également les bibliothèques suivantes qui ont été téléchargés par Microsoft :</span><span class="sxs-lookup"><span data-stu-id="5b11c-143">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="5b11c-144">ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="5b11c-144">ASP.NET Ajax</span></span>
- <span data-ttu-id="5b11c-145">Fichiers JavaScript ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="5b11c-145">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="5b11c-146">Les fichiers ASP.NET SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="5b11c-146">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="5b11c-147">Microsoft ne revendique pas la propriété de toutes les bibliothèques tierces hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="5b11c-147">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="5b11c-148">Les propriétaires de copyright des bibliothèques de licence ces bibliothèques pour vous.</span><span class="sxs-lookup"><span data-stu-id="5b11c-148">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="5b11c-149">Les droits que vous devrez peut-être télécharger et utiliser ces bibliothèques sont accordées exclusivement par les propriétaires de copyright respectifs.</span><span class="sxs-lookup"><span data-stu-id="5b11c-149">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="5b11c-150">Étant donné que celles-ci ne sont pas des bibliothèques de Microsoft, Microsoft ne fournit aucune garantie ou les licences de droits de propriété intellectuelle (y compris sans droits de brevet implicites) pour les bibliothèques tierces hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="5b11c-150">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="5b11c-151">Si vous souhaitez soumettre votre bibliothèque JavaScript et votre bibliothèque est un des extensions/plug-ins à ces bibliothèques ou des bibliothèques JavaScript (comme indiqué sur http://trends.builtwith.com) supérieur (a) répandus. ou (b) utile pour les utiliser sur ASP.NET, puis contactez AjaxCDNSubmission@Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="5b11c-151">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="5b11c-152">AJAX.Microsoft.com renommé ajax.aspnetcdn.com</span><span class="sxs-lookup"><span data-stu-id="5b11c-152">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="5b11c-153">Le CDN permet d’utiliser le nom de domaine microsoft.com et a été modifié pour utiliser le nom de domaine aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="5b11c-153">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="5b11c-154">Cette modification a été apportée pour augmenter les performances, car lorsqu’un navigateur référencé le domaine microsoft.com il envoie tous les cookies à partir de ce domaine sur le réseau avec chaque demande.</span><span class="sxs-lookup"><span data-stu-id="5b11c-154">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="5b11c-155">En renommant à un nom de domaine différent de microsoft.com performances peuvent être augmenté par autant de 25 %.</span><span class="sxs-lookup"><span data-stu-id="5b11c-155">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="5b11c-156">Remarque ajax.microsoft.com continuera de fonctionner mais ajax.aspnetcdn.com est recommandé.</span><span class="sxs-lookup"><span data-stu-id="5b11c-156">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="5b11c-157">Ancien Format : http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-157">Old Format: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="5b11c-158">Nouveau Format : http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-158">New Format: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="5b11c-159">Prise en charge de Visual Studio .vsdoc</span><span class="sxs-lookup"><span data-stu-id="5b11c-159">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="5b11c-160">Pour utiliser les fichiers .vsdoc correctement avec Visual Studio 2008, vous devez vous assurer que vous disposez de Visual Studio 2008 SP1 installé et le correctif logiciel pour les fichiers vsdoc installé.</span><span class="sxs-lookup"><span data-stu-id="5b11c-160">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="5b11c-161">Vous pouvez obtenir à partir d’ici :</span><span class="sxs-lookup"><span data-stu-id="5b11c-161">You can get these from here:</span></span>

- [<span data-ttu-id="5b11c-162">Télécharger Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="5b11c-162">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "télécharger Visual Studio 2008 SP1")
- [<span data-ttu-id="5b11c-163">Télécharger le correctif logiciel de .vsdoc pour Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="5b11c-163">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 "télécharger .vsdoc correctif pour Visual Studio 2008 SP1")

<span data-ttu-id="5b11c-164">Visual Studio 2010 prend en charge les fichiers .vsdoc sans les correctifs supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="5b11c-164">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="5b11c-165">À l’aide d’ASP.NET Ajax du CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-165">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="5b11c-166">Lorsque vous utilisez ASP.NET 4, vous pouvez rediriger toutes les demandes pour les scripts du framework ASP.NET vers le CDN.</span><span class="sxs-lookup"><span data-stu-id="5b11c-166">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="5b11c-167">La récupération des scripts à partir du CDN au lieu de votre serveur web local peuvent considérablement améliorer les performances des sites Web ASP.NET publics.</span><span class="sxs-lookup"><span data-stu-id="5b11c-167">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="5b11c-168">Utilisez la propriété ScriptManager EnableCDN pour rediriger toutes les demandes de script framework ASP.NET pour le CDN Microsoft Ajax :</span><span class="sxs-lookup"><span data-stu-id="5b11c-168">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="5b11c-169">À l’aide de jQuery du CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-169">Using jQuery from the CDN</span></span>

<span data-ttu-id="5b11c-170">Vous pouvez utiliser des scripts jQuery hébergés sur CDN dans votre application Web en ajoutant l’élément de script suivant à une page :</span><span class="sxs-lookup"><span data-stu-id="5b11c-170">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="5b11c-171">Le CDN inclut également la version réduite du script jQuery, que vous pouvez obtenir à l’aide de l’élément suivant :</span><span class="sxs-lookup"><span data-stu-id="5b11c-171">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="5b11c-172">Pour permettre à votre page à revenir au chargement jQuery à partir d’un chemin d’accès local sur votre propre site Web si le CDN est indisponible, ajoutez l’élément suivant immédiatement après l’élément référençant le CDN :</span><span class="sxs-lookup"><span data-stu-id="5b11c-172">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="5b11c-173">L’exemple de page suivant utilise la version CDN de la bibliothèque jQuery (avec basculement vers une copie locale) pour afficher le contenu d’un élément div lorsqu’un clic est effectué.</span><span class="sxs-lookup"><span data-stu-id="5b11c-173">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="5b11c-174">Vous pouvez en savoir plus sur jQuery et télécharger une copie locale de jQuery en vous rendant sur le [jQuery](http://jquery.com/) site Web.</span><span class="sxs-lookup"><span data-stu-id="5b11c-174">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="5b11c-175">À l’aide de jQuery UI du CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-175">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="5b11c-176">Le CDN héberge également la bibliothèque jQuery UI.</span><span class="sxs-lookup"><span data-stu-id="5b11c-176">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="5b11c-177">La bibliothèque jQuery UI comprend un ensemble complet de widgets et les effets que vous pouvez utiliser dans vos applications ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5b11c-177">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="5b11c-178">Par exemple, la page suivante illustre comment vous pouvez utiliser la sélecteur de dates de l’interface utilisateur jQuery dans le contexte d’une application Web Forms ASP.NET pour afficher un calendrier contextuel :</span><span class="sxs-lookup"><span data-stu-id="5b11c-178">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="5b11c-179">Lorsque vous déplacez le focus vers la zone de texte à l’aide de votre clavier, un calendrier s’affiche :</span><span class="sxs-lookup"><span data-stu-id="5b11c-179">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Calendrier contextuel créé avec le sélecteur de dates](overview/_static/image1.png)

<span data-ttu-id="5b11c-181">Notez que vous devez inclure les trois fichiers du CDN dans le code ci-dessus :</span><span class="sxs-lookup"><span data-stu-id="5b11c-181">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="5b11c-182">La bibliothèque jQuery &mdash; la bibliothèque jQuery UI dépend de la bibliothèque jQuery.</span><span class="sxs-lookup"><span data-stu-id="5b11c-182">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="5b11c-183">Vous devez ajouter la bibliothèque jQuery à votre page avant d’ajouter la bibliothèque jQuery UI.</span><span class="sxs-lookup"><span data-stu-id="5b11c-183">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="5b11c-184">La bibliothèque jQuery UI &mdash; la bibliothèque jQuery UI contient tous les effets d’interface utilisateur jQuery et widgets tels que le widget de sélecteur de dates utilisées dans la page ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="5b11c-184">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="5b11c-185">Un thème de l’interface utilisateur jQuery &mdash; jQuery UI prend en charge différents thèmes.</span><span class="sxs-lookup"><span data-stu-id="5b11c-185">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="5b11c-186">La page ci-dessus inclut un lien vers un fichier CSS pour importer le thème de Redmond.</span><span class="sxs-lookup"><span data-stu-id="5b11c-186">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="5b11c-187">Tous les thèmes de l’interface utilisateur jQuery standard sont hébergés sur le CDN.</span><span class="sxs-lookup"><span data-stu-id="5b11c-187">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="5b11c-188">[Visitez cette page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 dans le CDN Microsoft Ajax") pour afficher les miniatures pour chaque thème.</span><span class="sxs-lookup"><span data-stu-id="5b11c-188">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="5b11c-189">Pour plus d’informations sur la bibliothèque jQuery UI, visitez officielle [site Web de l’interface utilisateur jQuery](http://jQueryUI.com "site Web de l’interface utilisateur jQuery").</span><span class="sxs-lookup"><span data-stu-id="5b11c-189">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="5b11c-190">Fichiers de tiers dans le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-190">Third-Party Files on the CDN</span></span>

<span data-ttu-id="5b11c-191">Le CDN héberge certains des bibliothèques JavaScript tiers les plus populaires.</span><span class="sxs-lookup"><span data-stu-id="5b11c-191">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="5b11c-192">Microsoft ne revendique pas la propriété de toutes les bibliothèques tierces hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="5b11c-192">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="5b11c-193">Les propriétaires de copyright des bibliothèques de licence ces bibliothèques pour vous.</span><span class="sxs-lookup"><span data-stu-id="5b11c-193">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="5b11c-194">Les droits que vous devrez peut-être télécharger et utiliser ces bibliothèques sont accordées exclusivement par les propriétaires de copyright respectifs.</span><span class="sxs-lookup"><span data-stu-id="5b11c-194">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="5b11c-195">Étant donné que celles-ci ne sont pas des bibliothèques de Microsoft, Microsoft ne fournit aucune garantie ou les licences de droits de propriété intellectuelle (y compris sans droits de brevet implicites) pour les bibliothèques tierces hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="5b11c-195">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="5b11c-196">Versions de jQuery sur le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-196">jQuery Releases on the CDN</span></span>

<span data-ttu-id="5b11c-197">Les versions suivantes de jQuery sont hébergées sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="5b11c-197">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-321"></a><span data-ttu-id="5b11c-198">jQuery version 3.2.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-198">jQuery version 3.2.1</span></span>
- <span data-ttu-id="5b11c-199">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-199">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js</span></span>
- <span data-ttu-id="5b11c-200">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-200">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js</span></span>
- <span data-ttu-id="5b11c-201">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-201">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map</span></span>
- <span data-ttu-id="5b11c-202">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.SLIM.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-202">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js</span></span>
- <span data-ttu-id="5b11c-203">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.SLIM.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-203">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js</span></span>
- <span data-ttu-id="5b11c-204">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.SLIM.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-204">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map</span></span>

#### <a name="jquery-version-320"></a><span data-ttu-id="5b11c-205">version de jQuery 3.2.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-205">jQuery version 3.2.0</span></span>

- <span data-ttu-id="5b11c-206">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-206">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js</span></span>
- <span data-ttu-id="5b11c-207">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-207">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js</span></span>
- <span data-ttu-id="5b11c-208">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-208">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map</span></span>
- <span data-ttu-id="5b11c-209">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.SLIM.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-209">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js</span></span>
- <span data-ttu-id="5b11c-210">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.SLIM.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-210">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js</span></span>
- <span data-ttu-id="5b11c-211">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.SLIM.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-211">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map</span></span>

#### <a name="jquery-version-311"></a><span data-ttu-id="5b11c-212">version de jQuery 3.1.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-212">jQuery version 3.1.1</span></span>

- <span data-ttu-id="5b11c-213">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-213">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js</span></span>
- <span data-ttu-id="5b11c-214">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-214">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js</span></span>
- <span data-ttu-id="5b11c-215">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-215">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map</span></span>
- <span data-ttu-id="5b11c-216">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.SLIM.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-216">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js</span></span>
- <span data-ttu-id="5b11c-217">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.SLIM.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-217">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js</span></span>
- <span data-ttu-id="5b11c-218">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.SLIM.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-218">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map</span></span>

#### <a name="jquery-version-310"></a><span data-ttu-id="5b11c-219">jQuery version 3.1.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-219">jQuery version 3.1.0</span></span>

- <span data-ttu-id="5b11c-220">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-220">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js</span></span>
- <span data-ttu-id="5b11c-221">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-221">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js</span></span>
- <span data-ttu-id="5b11c-222">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-222">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map</span></span>
- <span data-ttu-id="5b11c-223">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.SLIM.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-223">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js</span></span>
- <span data-ttu-id="5b11c-224">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.SLIM.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-224">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js</span></span>
- <span data-ttu-id="5b11c-225">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.SLIM.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-225">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map</span></span>

#### <a name="jquery-version-300"></a><span data-ttu-id="5b11c-226">version de jQuery 3.0.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-226">jQuery version 3.0.0</span></span>

- <span data-ttu-id="5b11c-227">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-227">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js</span></span>
- <span data-ttu-id="5b11c-228">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-228">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js</span></span>
- <span data-ttu-id="5b11c-229">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-229">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map</span></span>
- <span data-ttu-id="5b11c-230">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.SLIM.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-230">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js</span></span>
- <span data-ttu-id="5b11c-231">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.SLIM.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-231">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js</span></span>
- <span data-ttu-id="5b11c-232">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.SLIM.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-232">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map</span></span>

#### <a name="jquery-version-224"></a><span data-ttu-id="5b11c-233">version de jQuery 2.2.4</span><span class="sxs-lookup"><span data-stu-id="5b11c-233">jQuery version 2.2.4</span></span>

- <span data-ttu-id="5b11c-234">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-234">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js</span></span>
- <span data-ttu-id="5b11c-235">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-235">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js</span></span>
- <span data-ttu-id="5b11c-236">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-236">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map</span></span>

#### <a name="jquery-version-223"></a><span data-ttu-id="5b11c-237">version de jQuery 2.2.3</span><span class="sxs-lookup"><span data-stu-id="5b11c-237">jQuery version 2.2.3</span></span>

- <span data-ttu-id="5b11c-238">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-238">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js</span></span>
- <span data-ttu-id="5b11c-239">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-239">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js</span></span>
- <span data-ttu-id="5b11c-240">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-240">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map</span></span>

#### <a name="jquery-version-222"></a><span data-ttu-id="5b11c-241">version de jQuery 2.2.2</span><span class="sxs-lookup"><span data-stu-id="5b11c-241">jQuery version 2.2.2</span></span>

- <span data-ttu-id="5b11c-242">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-242">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js</span></span>
- <span data-ttu-id="5b11c-243">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-243">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js</span></span>
- <span data-ttu-id="5b11c-244">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-244">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map</span></span>

#### <a name="jquery-version-221"></a><span data-ttu-id="5b11c-245">jQuery version 2.2.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-245">jQuery version 2.2.1</span></span>

- <span data-ttu-id="5b11c-246">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-246">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js</span></span>
- <span data-ttu-id="5b11c-247">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-247">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js</span></span>
- <span data-ttu-id="5b11c-248">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-248">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map</span></span>

#### <a name="jquery-version-220"></a><span data-ttu-id="5b11c-249">jQuery version2.2.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-249">jQuery version 2.2.0</span></span>

- <span data-ttu-id="5b11c-250">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-250">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js</span></span>
- <span data-ttu-id="5b11c-251">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-251">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js</span></span>
- <span data-ttu-id="5b11c-252">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-252">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map</span></span>

#### <a name="jquery-version-214"></a><span data-ttu-id="5b11c-253">version de jQuery 2.1.4</span><span class="sxs-lookup"><span data-stu-id="5b11c-253">jQuery version 2.1.4</span></span>

- <span data-ttu-id="5b11c-254">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-254">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js</span></span>
- <span data-ttu-id="5b11c-255">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-255">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js</span></span>
- <span data-ttu-id="5b11c-256">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-256">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map</span></span>

#### <a name="jquery-version-213"></a><span data-ttu-id="5b11c-257">version de jQuery 2.1.3</span><span class="sxs-lookup"><span data-stu-id="5b11c-257">jQuery version 2.1.3</span></span>

- <span data-ttu-id="5b11c-258">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-258">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js</span></span>
- <span data-ttu-id="5b11c-259">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-259">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js</span></span>
- <span data-ttu-id="5b11c-260">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-260">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map</span></span>

#### <a name="jquery-version-212"></a><span data-ttu-id="5b11c-261">version de jQuery 2.1.2</span><span class="sxs-lookup"><span data-stu-id="5b11c-261">jQuery version 2.1.2</span></span>

- <span data-ttu-id="5b11c-262">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.2.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-262">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js</span></span>
- <span data-ttu-id="5b11c-263">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.2.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-263">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js</span></span>

#### <a name="jquery-version-211"></a><span data-ttu-id="5b11c-264">version de jQuery 2.1.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-264">jQuery version 2.1.1</span></span>

- <span data-ttu-id="5b11c-265">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-265">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js</span></span>
- <span data-ttu-id="5b11c-266">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-266">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js</span></span>
- <span data-ttu-id="5b11c-267">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-267">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map</span></span>

#### <a name="jquery-version-210"></a><span data-ttu-id="5b11c-268">version de jQuery 2.1.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-268">jQuery version 2.1.0</span></span>

- <span data-ttu-id="5b11c-269">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-269">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js</span></span>
- <span data-ttu-id="5b11c-270">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-270">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js</span></span>
- <span data-ttu-id="5b11c-271">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-271">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js</span></span>
- <span data-ttu-id="5b11c-272">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-272">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map</span></span>

#### <a name="jquery-version-203"></a><span data-ttu-id="5b11c-273">version de jQuery 2.0.3</span><span class="sxs-lookup"><span data-stu-id="5b11c-273">jQuery version 2.0.3</span></span>

- <span data-ttu-id="5b11c-274">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-274">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js</span></span>
- <span data-ttu-id="5b11c-275">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-275">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js</span></span>
- <span data-ttu-id="5b11c-276">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-276">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js</span></span>
- <span data-ttu-id="5b11c-277">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-277">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map</span></span>

#### <a name="jquery-version-202"></a><span data-ttu-id="5b11c-278">version de jQuery 2.0.2</span><span class="sxs-lookup"><span data-stu-id="5b11c-278">jQuery version 2.0.2</span></span>

- <span data-ttu-id="5b11c-279">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-279">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js</span></span>
- <span data-ttu-id="5b11c-280">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-280">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js</span></span>
- <span data-ttu-id="5b11c-281">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-281">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js</span></span>
- <span data-ttu-id="5b11c-282">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-282">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map</span></span>

#### <a name="jquery-version-201"></a><span data-ttu-id="5b11c-283">version de jQuery 2.0.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-283">jQuery version 2.0.1</span></span>

- <span data-ttu-id="5b11c-284">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-284">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js</span></span>
- <span data-ttu-id="5b11c-285">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-285">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js</span></span>
- <span data-ttu-id="5b11c-286">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-286">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js</span></span>
- <span data-ttu-id="5b11c-287">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-287">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map</span></span>

#### <a name="jquery-version-200"></a><span data-ttu-id="5b11c-288">jQuery version 2.0.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-288">jQuery version 2.0.0</span></span>

- <span data-ttu-id="5b11c-289">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-289">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js</span></span>
- <span data-ttu-id="5b11c-290">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-290">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js</span></span>
- <span data-ttu-id="5b11c-291">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-291">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js</span></span>
- <span data-ttu-id="5b11c-292">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-292">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map</span></span>

#### <a name="jquery-version-1124"></a><span data-ttu-id="5b11c-293">version de jQuery 1.12.4</span><span class="sxs-lookup"><span data-stu-id="5b11c-293">jQuery version 1.12.4</span></span>

- <span data-ttu-id="5b11c-294">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-294">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js</span></span>
- <span data-ttu-id="5b11c-295">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-295">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js</span></span>
- <span data-ttu-id="5b11c-296">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-296">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map</span></span>

#### <a name="jquery-version-1123"></a><span data-ttu-id="5b11c-297">version de jQuery 1.12.3</span><span class="sxs-lookup"><span data-stu-id="5b11c-297">jQuery version 1.12.3</span></span>

- <span data-ttu-id="5b11c-298">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-298">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js</span></span>
- <span data-ttu-id="5b11c-299">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-299">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js</span></span>
- <span data-ttu-id="5b11c-300">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-300">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map</span></span>

#### <a name="jquery-version-1122"></a><span data-ttu-id="5b11c-301">version de jQuery 1.12.2</span><span class="sxs-lookup"><span data-stu-id="5b11c-301">jQuery version 1.12.2</span></span>

- <span data-ttu-id="5b11c-302">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-302">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js</span></span>
- <span data-ttu-id="5b11c-303">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-303">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js</span></span>
- <span data-ttu-id="5b11c-304">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-304">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map</span></span>

#### <a name="jquery-version-1121"></a><span data-ttu-id="5b11c-305">version de jQuery 1.12.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-305">jQuery version 1.12.1</span></span>

- <span data-ttu-id="5b11c-306">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-306">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js</span></span>
- <span data-ttu-id="5b11c-307">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-307">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js</span></span>
- <span data-ttu-id="5b11c-308">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-308">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map</span></span>

#### <a name="jquery-version-1120"></a><span data-ttu-id="5b11c-309">version de jQuery 1.12.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-309">jQuery version 1.12.0</span></span>

- <span data-ttu-id="5b11c-310">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-310">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js</span></span>
- <span data-ttu-id="5b11c-311">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-311">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js</span></span>
- <span data-ttu-id="5b11c-312">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-312">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map</span></span>

#### <a name="jquery-version-1113"></a><span data-ttu-id="5b11c-313">version de jQuery 1.11.3</span><span class="sxs-lookup"><span data-stu-id="5b11c-313">jQuery version 1.11.3</span></span>

- <span data-ttu-id="5b11c-314">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-314">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js</span></span>
- <span data-ttu-id="5b11c-315">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-315">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js</span></span>
- <span data-ttu-id="5b11c-316">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-316">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map</span></span>

#### <a name="jquery-version-1112"></a><span data-ttu-id="5b11c-317">version de jQuery 1.11.2</span><span class="sxs-lookup"><span data-stu-id="5b11c-317">jQuery version 1.11.2</span></span>

- <span data-ttu-id="5b11c-318">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-318">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js</span></span>
- <span data-ttu-id="5b11c-319">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-319">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js</span></span>
- <span data-ttu-id="5b11c-320">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-320">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map</span></span>

#### <a name="jquery-version-1111"></a><span data-ttu-id="5b11c-321">version de jQuery 1.11.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-321">jQuery version 1.11.1</span></span>

- <span data-ttu-id="5b11c-322">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-322">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js</span></span>
- <span data-ttu-id="5b11c-323">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-323">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js</span></span>
- <span data-ttu-id="5b11c-324">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-324">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map</span></span>

#### <a name="jquery-version-1110"></a><span data-ttu-id="5b11c-325">version de jQuery 1.11.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-325">jQuery version 1.11.0</span></span>

- <span data-ttu-id="5b11c-326">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-326">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js</span></span>
- <span data-ttu-id="5b11c-327">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-327">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js</span></span>
- <span data-ttu-id="5b11c-328">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-328">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js</span></span>
- <span data-ttu-id="5b11c-329">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-329">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map</span></span>

#### <a name="jquery-version-1102"></a><span data-ttu-id="5b11c-330">version de jQuery 1.10.2</span><span class="sxs-lookup"><span data-stu-id="5b11c-330">jQuery version 1.10.2</span></span>

- <span data-ttu-id="5b11c-331">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-331">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js</span></span>
- <span data-ttu-id="5b11c-332">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-332">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js</span></span>
- <span data-ttu-id="5b11c-333">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-333">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js</span></span>
- <span data-ttu-id="5b11c-334">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-334">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map</span></span>

#### <a name="jquery-version-1101"></a><span data-ttu-id="5b11c-335">version de jQuery 1.10.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-335">jQuery version 1.10.1</span></span>

- <span data-ttu-id="5b11c-336">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-336">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js</span></span>
- <span data-ttu-id="5b11c-337">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-337">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js</span></span>
- <span data-ttu-id="5b11c-338">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-338">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js</span></span>
- <span data-ttu-id="5b11c-339">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-339">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map</span></span>

#### <a name="jquery-version-1100"></a><span data-ttu-id="5b11c-340">version de jQuery 1.10.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-340">jQuery version 1.10.0</span></span>

- <span data-ttu-id="5b11c-341">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-341">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js</span></span>
- <span data-ttu-id="5b11c-342">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-342">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js</span></span>
- <span data-ttu-id="5b11c-343">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-343">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js</span></span>
- <span data-ttu-id="5b11c-344">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-344">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map</span></span>

#### <a name="jquery-version-191"></a><span data-ttu-id="5b11c-345">version de jQuery 1.9.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-345">jQuery version 1.9.1</span></span>

- <span data-ttu-id="5b11c-346">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-346">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js</span></span>
- <span data-ttu-id="5b11c-347">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-347">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js</span></span>
- <span data-ttu-id="5b11c-348">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-348">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js</span></span>
- <span data-ttu-id="5b11c-349">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-349">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map</span></span>

#### <a name="jquery-version-190"></a><span data-ttu-id="5b11c-350">version de jQuery à 1.9.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-350">jQuery version 1.9.0</span></span>

- <span data-ttu-id="5b11c-351">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-351">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js</span></span>
- <span data-ttu-id="5b11c-352">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-352">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js</span></span>
- <span data-ttu-id="5b11c-353">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-353">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js</span></span>
- <span data-ttu-id="5b11c-354">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-354">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map</span></span>

#### <a name="jquery-version-183"></a><span data-ttu-id="5b11c-355">version de jQuery 1.8.3</span><span class="sxs-lookup"><span data-stu-id="5b11c-355">jQuery version 1.8.3</span></span>

- <span data-ttu-id="5b11c-356">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-356">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js</span></span>
- <span data-ttu-id="5b11c-357">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-357">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js</span></span>
- <span data-ttu-id="5b11c-358">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-358">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js</span></span>

#### <a name="jquery-version-182"></a><span data-ttu-id="5b11c-359">version de jQuery 1.8.2</span><span class="sxs-lookup"><span data-stu-id="5b11c-359">jQuery version 1.8.2</span></span>

- <span data-ttu-id="5b11c-360">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-360">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js</span></span>
- <span data-ttu-id="5b11c-361">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-361">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js</span></span>
- <span data-ttu-id="5b11c-362">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-362">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js</span></span>

#### <a name="jquery-version-181"></a><span data-ttu-id="5b11c-363">version de jQuery 1.8.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-363">jQuery version 1.8.1</span></span>

- <span data-ttu-id="5b11c-364">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-364">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js</span></span>
- <span data-ttu-id="5b11c-365">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-365">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js</span></span>
- <span data-ttu-id="5b11c-366">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-366">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js</span></span>

#### <a name="jquery-version-180"></a><span data-ttu-id="5b11c-367">version de jQuery 1.8.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-367">jQuery version 1.8.0</span></span>

- <span data-ttu-id="5b11c-368">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-368">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="5b11c-369">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-369">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js</span></span>
- <span data-ttu-id="5b11c-370">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-370">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js</span></span>

#### <a name="jquery-version-172"></a><span data-ttu-id="5b11c-371">version de jQuery 1.7.2</span><span class="sxs-lookup"><span data-stu-id="5b11c-371">jQuery version 1.7.2</span></span>

- <span data-ttu-id="5b11c-372">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.2.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-372">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js</span></span>
- <span data-ttu-id="5b11c-373">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.2.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-373">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js</span></span>

#### <a name="jquery-version-171"></a><span data-ttu-id="5b11c-374">jQuery version 1.7.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-374">jQuery version 1.7.1</span></span>

- <span data-ttu-id="5b11c-375">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-375">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js</span></span>
- <span data-ttu-id="5b11c-376">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-376">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js</span></span>
- <span data-ttu-id="5b11c-377">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-377">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js</span></span>

#### <a name="jquery-version-17"></a><span data-ttu-id="5b11c-378">jQuery version 1.7</span><span class="sxs-lookup"><span data-stu-id="5b11c-378">jQuery version 1.7</span></span>

- <span data-ttu-id="5b11c-379">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-379">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js</span></span>
- <span data-ttu-id="5b11c-380">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-380">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js</span></span>
- <span data-ttu-id="5b11c-381">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-381">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js</span></span>

#### <a name="jquery-version-164"></a><span data-ttu-id="5b11c-382">version de jQuery 1.6.4</span><span class="sxs-lookup"><span data-stu-id="5b11c-382">jQuery version 1.6.4</span></span>

- <span data-ttu-id="5b11c-383">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-383">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js</span></span>
- <span data-ttu-id="5b11c-384">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-384">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js</span></span>
- <span data-ttu-id="5b11c-385">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-385">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js</span></span>

#### <a name="jquery-version-163"></a><span data-ttu-id="5b11c-386">version de jQuery 1.6.3</span><span class="sxs-lookup"><span data-stu-id="5b11c-386">jQuery version 1.6.3</span></span>

- <span data-ttu-id="5b11c-387">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-387">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js</span></span>
- <span data-ttu-id="5b11c-388">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-388">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js</span></span>
- <span data-ttu-id="5b11c-389">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-389">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js</span></span>

#### <a name="jquery-version-162"></a><span data-ttu-id="5b11c-390">version de jQuery 1.6.2</span><span class="sxs-lookup"><span data-stu-id="5b11c-390">jQuery version 1.6.2</span></span>

- <span data-ttu-id="5b11c-391">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-391">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js</span></span>
- <span data-ttu-id="5b11c-392">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-392">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js</span></span>
- <span data-ttu-id="5b11c-393">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-393">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js</span></span>

#### <a name="jquery-version-161"></a><span data-ttu-id="5b11c-394">version de jQuery 1.6.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-394">jQuery version 1.6.1</span></span>

- <span data-ttu-id="5b11c-395">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-395">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js</span></span>
- <span data-ttu-id="5b11c-396">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-396">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js</span></span>
- <span data-ttu-id="5b11c-397">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-397">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js</span></span>

#### <a name="jquery-version-16"></a><span data-ttu-id="5b11c-398">jQuery version 1.6</span><span class="sxs-lookup"><span data-stu-id="5b11c-398">jQuery version 1.6</span></span>

- <span data-ttu-id="5b11c-399">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-399">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js</span></span>
- <span data-ttu-id="5b11c-400">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-400">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js</span></span>
- <span data-ttu-id="5b11c-401">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-401">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js</span></span>

#### <a name="jquery-version-152"></a><span data-ttu-id="5b11c-402">version de jQuery 1.5.2</span><span class="sxs-lookup"><span data-stu-id="5b11c-402">jQuery version 1.5.2</span></span>

- <span data-ttu-id="5b11c-403">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-403">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js</span></span>
- <span data-ttu-id="5b11c-404">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-404">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js</span></span>
- <span data-ttu-id="5b11c-405">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-405">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js</span></span>

#### <a name="jquery-version-151"></a><span data-ttu-id="5b11c-406">version de jQuery 1.5.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-406">jQuery version 1.5.1</span></span>

- <span data-ttu-id="5b11c-407">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-407">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js</span></span>
- <span data-ttu-id="5b11c-408">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-408">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js</span></span>
- <span data-ttu-id="5b11c-409">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-409">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js</span></span>

#### <a name="jquery-version-15"></a><span data-ttu-id="5b11c-410">version de jQuery 1.5</span><span class="sxs-lookup"><span data-stu-id="5b11c-410">jQuery version 1.5</span></span>

- <span data-ttu-id="5b11c-411">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-411">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js</span></span>
- <span data-ttu-id="5b11c-412">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-412">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js</span></span>
- <span data-ttu-id="5b11c-413">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-413">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js</span></span>

#### <a name="jquery-version-144"></a><span data-ttu-id="5b11c-414">version de jQuery 1.4.4</span><span class="sxs-lookup"><span data-stu-id="5b11c-414">jQuery version 1.4.4</span></span>

- <span data-ttu-id="5b11c-415">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-415">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js</span></span>
- <span data-ttu-id="5b11c-416">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-416">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js</span></span>
- <span data-ttu-id="5b11c-417">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-417">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js</span></span>

#### <a name="jquery-version-143"></a><span data-ttu-id="5b11c-418">version de jQuery 1.4.3</span><span class="sxs-lookup"><span data-stu-id="5b11c-418">jQuery version 1.4.3</span></span>

- <span data-ttu-id="5b11c-419">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-419">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js</span></span>
- <span data-ttu-id="5b11c-420">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-420">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js</span></span>
- <span data-ttu-id="5b11c-421">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-421">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js</span></span>

#### <a name="jquery-version-142"></a><span data-ttu-id="5b11c-422">version de jQuery 1.4.2</span><span class="sxs-lookup"><span data-stu-id="5b11c-422">jQuery version 1.4.2</span></span>

- <span data-ttu-id="5b11c-423">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-423">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js</span></span>
- <span data-ttu-id="5b11c-424">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-424">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js</span></span>
- <span data-ttu-id="5b11c-425">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-425">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js</span></span>

#### <a name="jquery-version-141"></a><span data-ttu-id="5b11c-426">version de jQuery 1.4.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-426">jQuery version 1.4.1</span></span>

- <span data-ttu-id="5b11c-427">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-427">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js</span></span>
- <span data-ttu-id="5b11c-428">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-428">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js</span></span>
- <span data-ttu-id="5b11c-429">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-429">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js</span></span>

#### <a name="jquery-version-14"></a><span data-ttu-id="5b11c-430">jQuery version 1.4</span><span class="sxs-lookup"><span data-stu-id="5b11c-430">jQuery version 1.4</span></span>

- <span data-ttu-id="5b11c-431">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-431">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js</span></span>
- <span data-ttu-id="5b11c-432">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-432">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js</span></span>

#### <a name="jquery-version-132"></a><span data-ttu-id="5b11c-433">version de jQuery 1.3.2</span><span class="sxs-lookup"><span data-stu-id="5b11c-433">jQuery version 1.3.2</span></span>

- <span data-ttu-id="5b11c-434">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-434">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js</span></span>
- <span data-ttu-id="5b11c-435">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-435">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js</span></span>
- <span data-ttu-id="5b11c-436">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-436">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js</span></span>
- <span data-ttu-id="5b11c-437">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.min-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-437">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js</span></span>

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="5b11c-438">jQuery migrer des versions sur le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-438">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="5b11c-439">Les versions suivantes de jQuery migrer sont hébergées sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="5b11c-439">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="5b11c-440">jQuery migration version 3.0.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-440">jQuery Migrate version 3.0.0</span></span>

- <span data-ttu-id="5b11c-441">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-441">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js</span></span>
- <span data-ttu-id="5b11c-442">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-3.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-442">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js</span></span>

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="5b11c-443">jQuery migration version 1.2.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-443">jQuery Migrate version 1.2.1</span></span>

- <span data-ttu-id="5b11c-444">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.2.1.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-444">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js</span></span>
- <span data-ttu-id="5b11c-445">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-445">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js</span></span>

<span data-ttu-id="5b11c-446">jQuery migration version 1.2.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-446">jQuery Migrate version 1.2.0</span></span>

- <span data-ttu-id="5b11c-447">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.2.0.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-447">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js</span></span>
- <span data-ttu-id="5b11c-448">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-448">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js</span></span>

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="5b11c-449">jQuery migration version 1.1.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-449">jQuery Migrate version 1.1.1</span></span>

- <span data-ttu-id="5b11c-450">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.1.1.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-450">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js</span></span>
- <span data-ttu-id="5b11c-451">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-451">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js</span></span>

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="5b11c-452">jQuery migration version 1.1.0.</span><span class="sxs-lookup"><span data-stu-id="5b11c-452">jQuery Migrate version 1.1.0</span></span>

- <span data-ttu-id="5b11c-453">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.1.0.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-453">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js</span></span>
- <span data-ttu-id="5b11c-454">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-454">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js</span></span>

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="5b11c-455">Migrer la version 1.0.0 de jQuery</span><span class="sxs-lookup"><span data-stu-id="5b11c-455">jQuery Migrate version 1.0.0</span></span>

- <span data-ttu-id="5b11c-456">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.0.0.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-456">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js</span></span>
- <span data-ttu-id="5b11c-457">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-457">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js</span></span>

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="5b11c-458">jQuery UI mises en production sur le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-458">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="5b11c-459">Les versions suivantes de la bibliothèque jQuery UI sont hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="5b11c-459">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="5b11c-460">Cliquez sur chaque lien pour afficher la liste réelle des fichiers.</span><span class="sxs-lookup"><span data-stu-id="5b11c-460">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="5b11c-461">jQuery UI 1.12.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-461">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery UI 1.12.1 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-462">jQuery UI 1.12.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-462">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "jQuery UI 1.12.0 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-463">jQuery UI 1.11.4</span><span class="sxs-lookup"><span data-stu-id="5b11c-463">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "jQuery UI 1.11.4 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-464">jQuery UI 1.11.3</span><span class="sxs-lookup"><span data-stu-id="5b11c-464">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery UI 1.11.3 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-465">jQuery UI 1.11.2</span><span class="sxs-lookup"><span data-stu-id="5b11c-465">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery UI 1.11.2 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-466">jQuery UI 1.11.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-466">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "jQuery UI 1.11.1 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-467">jQuery UI 1.11.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-467">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "jQuery UI 1.11.0 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-468">jQuery UI 1.10.4</span><span class="sxs-lookup"><span data-stu-id="5b11c-468">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery UI 1.10.4 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-469">jQuery UI 1.10.3</span><span class="sxs-lookup"><span data-stu-id="5b11c-469">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery UI 1.10.3 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-470">jQuery UI 1.10.2</span><span class="sxs-lookup"><span data-stu-id="5b11c-470">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery UI 1.10.2 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-471">jQuery UI 1.10.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-471">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery UI 1.10.1 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-472">jQuery UI 1.10.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-472">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery UI 1.10.0 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-473">jQuery UI 1.9.2</span><span class="sxs-lookup"><span data-stu-id="5b11c-473">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "jQuery UI 1.9.2 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-474">jQuery UI 1.9.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-474">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "jQuery UI 1.9.1 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-475">jQuery UI à 1.9.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-475">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "jQuery UI à 1.9.0 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-476">jQuery UI 1.8.24</span><span class="sxs-lookup"><span data-stu-id="5b11c-476">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery UI 1.8.24 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-477">jQuery UI 1.8.23</span><span class="sxs-lookup"><span data-stu-id="5b11c-477">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery UI 1.8.23 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-478">jQuery UI 1.8.22</span><span class="sxs-lookup"><span data-stu-id="5b11c-478">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery UI 1.8.22 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-479">jQuery UI 1.8.21</span><span class="sxs-lookup"><span data-stu-id="5b11c-479">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery UI 1.8.21 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-480">jQuery UI 1.8.20</span><span class="sxs-lookup"><span data-stu-id="5b11c-480">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery UI 1.8.20 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-481">jQuery UI 1.8.19</span><span class="sxs-lookup"><span data-stu-id="5b11c-481">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery UI 1.8.19 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-482">jQuery UI 1.8.18</span><span class="sxs-lookup"><span data-stu-id="5b11c-482">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery UI 1.8.18 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-483">jQuery UI 1.8.17</span><span class="sxs-lookup"><span data-stu-id="5b11c-483">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery UI 1.8.17 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-484">jQuery UI 1.8.16</span><span class="sxs-lookup"><span data-stu-id="5b11c-484">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery UI 1.8.16 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-485">jQuery UI 1.8.15</span><span class="sxs-lookup"><span data-stu-id="5b11c-485">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery UI 1.8.15 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-486">jQuery UI 1.8.14</span><span class="sxs-lookup"><span data-stu-id="5b11c-486">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery UI 1.8.14 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-487">jQuery UI 1.8.13</span><span class="sxs-lookup"><span data-stu-id="5b11c-487">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery UI 1.8.13 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-488">jQuery UI 1.8.12</span><span class="sxs-lookup"><span data-stu-id="5b11c-488">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery UI 1.8.12 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-489">jQuery UI 1.8.11</span><span class="sxs-lookup"><span data-stu-id="5b11c-489">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery UI 1.8.11 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-490">jQuery UI 1.8.10</span><span class="sxs-lookup"><span data-stu-id="5b11c-490">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-491">jQuery UI 1.8.9</span><span class="sxs-lookup"><span data-stu-id="5b11c-491">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "jQuery UI 1.8.9 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-492">jQuery UI 1.8.8</span><span class="sxs-lookup"><span data-stu-id="5b11c-492">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "jQuery UI 1.8.8 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-493">jQuery UI 1.8.7</span><span class="sxs-lookup"><span data-stu-id="5b11c-493">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery UI 1.8.7 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-494">jQuery UI 1.8.6</span><span class="sxs-lookup"><span data-stu-id="5b11c-494">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery UI 1.8.6 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-495">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="5b11c-495">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="5b11c-496">jQuery Validation mises en production sur le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-496">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="5b11c-497">Les versions suivantes de la bibliothèque de Validation jQuery sont hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="5b11c-497">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="5b11c-498">Cliquez sur chaque lien pour afficher la liste réelle des fichiers.</span><span class="sxs-lookup"><span data-stu-id="5b11c-498">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="5b11c-499">jQuery validation 1.17.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-499">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "jQuery Validation 1.17.0")
- [<span data-ttu-id="5b11c-500">jQuery validation 1.16.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-500">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "jQuery Validation 1.16.0")
- [<span data-ttu-id="5b11c-501">jQuery validation 1.15.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-501">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "jQuery Validation 1.15.1")
- [<span data-ttu-id="5b11c-502">jQuery validation 1.15.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-502">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "jQuery Validation 1.15.0")
- [<span data-ttu-id="5b11c-503">jQuery validation 1.14.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-503">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "jQuery Validation 1.14.0")
- [<span data-ttu-id="5b11c-504">jQuery validation 1.13.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-504">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "jQuery Validation 1.13.1")
- [<span data-ttu-id="5b11c-505">jQuery validation 1.13.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-505">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "jQuery Validation 1.13.0")
- [<span data-ttu-id="5b11c-506">jQuery validation 1.12.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-506">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "jQuery Validation 1.12.0")
- [<span data-ttu-id="5b11c-507">jQuery validation 1.11.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-507">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "jQuery Validation 1.11.1")
- [<span data-ttu-id="5b11c-508">jQuery validation 1.11.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-508">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "jQuery Validation 1.11.0")
- [<span data-ttu-id="5b11c-509">jQuery validation 1.10.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-509">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "jQuery Validation 1.10.0")
- [<span data-ttu-id="5b11c-510">jQuery validation 1.9</span><span class="sxs-lookup"><span data-stu-id="5b11c-510">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate version 1.9")
- [<span data-ttu-id="5b11c-511">jQuery validation 1.8.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-511">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jquery.validate version 1.8.1")
- [<span data-ttu-id="5b11c-512">jQuery validation 1.8</span><span class="sxs-lookup"><span data-stu-id="5b11c-512">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate version 1.8")
- [<span data-ttu-id="5b11c-513">jQuery validation 1.7</span><span class="sxs-lookup"><span data-stu-id="5b11c-513">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate version 1.7")
- [<span data-ttu-id="5b11c-514">jQuery validation 1.6</span><span class="sxs-lookup"><span data-stu-id="5b11c-514">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery validation 1.6")
- [<span data-ttu-id="5b11c-515">jQuery validation 1.5.5</span><span class="sxs-lookup"><span data-stu-id="5b11c-515">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "jQuery validation 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="5b11c-516">jQuery Mobile mises en production sur le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-516">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="5b11c-517">Les versions suivantes de la bibliothèque de jQuery Mobile sont hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="5b11c-517">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="5b11c-518">Cliquez sur chaque lien pour afficher la liste réelle des fichiers.</span><span class="sxs-lookup"><span data-stu-id="5b11c-518">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="5b11c-519">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="5b11c-519">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-520">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="5b11c-520">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-521">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-521">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-522">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-522">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-523">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="5b11c-523">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-524">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-524">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-525">jQuery Mobile version 1.3.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-525">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile version 1.3.0 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-526">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-526">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-527">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="5b11c-527">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-528">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-528">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-529">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-529">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-530">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="5b11c-530">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-531">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-531">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-532">jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-532">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-533">jQuery Mobile 1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="5b11c-533">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-534">jQuery Mobile 1.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="5b11c-534">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="5b11c-535">version bêta 3 de jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-535">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 bêta 3 sur le CDN Microsoft Ajax")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="5b11c-536">jQuery versions de modèles sur le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-536">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="5b11c-537">Les versions suivantes du plug-in de modèles jQuery sont hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="5b11c-537">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="5b11c-538">Cliquez sur chaque lien pour afficher la liste réelle des fichiers.</span><span class="sxs-lookup"><span data-stu-id="5b11c-538">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="5b11c-539">jQuery modèles bêta 1</span><span class="sxs-lookup"><span data-stu-id="5b11c-539">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery modèles bêta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="5b11c-540">jQuery Cycle des versions sur le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-540">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="5b11c-541">Les versions suivantes du plug-in du Cycle de jQuery sont hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="5b11c-541">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="5b11c-542">Cliquez sur chaque lien pour afficher la liste réelle des fichiers.</span><span class="sxs-lookup"><span data-stu-id="5b11c-542">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="5b11c-543">jQuery Cycle 2,99</span><span class="sxs-lookup"><span data-stu-id="5b11c-543">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery 2,99 de Cycle")
- [<span data-ttu-id="5b11c-544">jQuery Cycle 2.94</span><span class="sxs-lookup"><span data-stu-id="5b11c-544">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery Cycle 2.94")
- [<span data-ttu-id="5b11c-545">jQuery Cycle 2,88</span><span class="sxs-lookup"><span data-stu-id="5b11c-545">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery 2,88 de Cycle")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="5b11c-546">jQuery versions de tables de données sur le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-546">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="5b11c-547">Les versions suivantes du plug-in DataTables de jQuery sont hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="5b11c-547">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="5b11c-548">Cliquez sur chaque lien pour afficher la liste réelle des fichiers.</span><span class="sxs-lookup"><span data-stu-id="5b11c-548">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="5b11c-549">jQuery DataTables 1.10.5</span><span class="sxs-lookup"><span data-stu-id="5b11c-549">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [<span data-ttu-id="5b11c-550">jQuery DataTables 1.10.4</span><span class="sxs-lookup"><span data-stu-id="5b11c-550">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [<span data-ttu-id="5b11c-551">jQuery DataTables 1.9.4</span><span class="sxs-lookup"><span data-stu-id="5b11c-551">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables 1.9.4")
- [<span data-ttu-id="5b11c-552">jQuery DataTables 1.9.3</span><span class="sxs-lookup"><span data-stu-id="5b11c-552">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [<span data-ttu-id="5b11c-553">jQuery DataTables 1.9.2</span><span class="sxs-lookup"><span data-stu-id="5b11c-553">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [<span data-ttu-id="5b11c-554">jQuery DataTables 1.9.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-554">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [<span data-ttu-id="5b11c-555">jQuery DataTables à 1.9.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-555">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables à 1.9.0")
- [<span data-ttu-id="5b11c-556">jQuery DataTables 1.8.2</span><span class="sxs-lookup"><span data-stu-id="5b11c-556">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="5b11c-557">Versions de Modernizr sur le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-557">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="5b11c-558">Les versions suivantes de [Modernizr](http://www.modernizr.com "Modernizr") sont hébergés sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="5b11c-558">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- <span data-ttu-id="5b11c-559">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.8.3.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-559">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js</span></span>
- <span data-ttu-id="5b11c-560">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.7.2.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-560">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js</span></span>
- <span data-ttu-id="5b11c-561">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.7.1.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-561">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js</span></span>
- <span data-ttu-id="5b11c-562">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.6.2.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-562">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js</span></span>
- <span data-ttu-id="5b11c-563">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-1.7-Development-only.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-563">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js</span></span>
- <span data-ttu-id="5b11c-564">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.0.6-Development-only.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-564">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js</span></span>

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="5b11c-565">Versions de JSHint sur le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-565">JSHint Releases on the CDN</span></span>

<span data-ttu-id="5b11c-566">Les versions suivantes de [JSHint](http://www.jshint.com "JSHint") sont hébergés sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="5b11c-566">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- <span data-ttu-id="5b11c-567">http://AJAX.aspnetcdn.com/AJAX/jshint/r07/jshint.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-567">http://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js</span></span>

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="5b11c-568">Versions de Knockout sur le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-568">Knockout Releases on the CDN</span></span>

<span data-ttu-id="5b11c-569">Les versions suivantes de [Knockout](http://www.knockoutjs.com "Knockout") sont hébergés sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="5b11c-569">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- <span data-ttu-id="5b11c-570">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-570">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js</span></span>
- <span data-ttu-id="5b11c-571">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.2.1.Debug.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-571">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js</span></span>
- <span data-ttu-id="5b11c-572">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-572">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js</span></span>
- <span data-ttu-id="5b11c-573">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.2.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-573">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js</span></span>
- <span data-ttu-id="5b11c-574">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-574">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js</span></span>
- <span data-ttu-id="5b11c-575">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.1.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-575">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js</span></span>
- <span data-ttu-id="5b11c-576">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-576">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js</span></span>
- <span data-ttu-id="5b11c-577">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.0.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-577">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js</span></span>
- <span data-ttu-id="5b11c-578">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.1.0.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-578">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js</span></span>
- <span data-ttu-id="5b11c-579">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.1.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-579">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js</span></span>
- <span data-ttu-id="5b11c-580">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.2.0.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-580">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js</span></span>
- <span data-ttu-id="5b11c-581">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.2.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-581">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js</span></span>
- <span data-ttu-id="5b11c-582">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.3.0.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-582">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js</span></span>
- <span data-ttu-id="5b11c-583">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.3.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-583">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js</span></span>
- <span data-ttu-id="5b11c-584">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.0.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-584">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js</span></span>
- <span data-ttu-id="5b11c-585">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-585">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js</span></span>
- <span data-ttu-id="5b11c-586">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.1.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-586">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js</span></span>
- <span data-ttu-id="5b11c-587">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.1.Debug.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-587">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js</span></span>
- <span data-ttu-id="5b11c-588">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.2.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-588">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js</span></span>
- <span data-ttu-id="5b11c-589">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.2.Debug.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-589">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js</span></span>

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="5b11c-590">Globaliser des versions sur le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-590">Globalize Releases on the CDN</span></span>

<span data-ttu-id="5b11c-591">Les versions suivantes de [Globalize](https://github.com/jquery/globalize "Globalize") sont hébergés sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="5b11c-591">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="5b11c-592">La version 1.0.0 de globalisation</span><span class="sxs-lookup"><span data-stu-id="5b11c-592">Globalize version 1.0.0</span></span>

- <span data-ttu-id="5b11c-593">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-593">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js</span></span>
- <span data-ttu-id="5b11c-594">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/Node-main.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-594">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js</span></span>
- <span data-ttu-id="5b11c-595">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Currency.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-595">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js</span></span>
- <span data-ttu-id="5b11c-596">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/date.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-596">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js</span></span>
- <span data-ttu-id="5b11c-597">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/message.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-597">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js</span></span>
- <span data-ttu-id="5b11c-598">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Number.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-598">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js</span></span>
- <span data-ttu-id="5b11c-599">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/plural.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-599">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js</span></span>
- <span data-ttu-id="5b11c-600">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/relative-Time.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-600">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js</span></span>

#### <a name="globalize-version-011"></a><span data-ttu-id="5b11c-601">Globaliser version 0.1.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-601">Globalize version 0.1.1</span></span>

- <span data-ttu-id="5b11c-602">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/globalize.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-602">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js</span></span>
- <span data-ttu-id="5b11c-603">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/globalize.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-603">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js</span></span>
- <span data-ttu-id="5b11c-604">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/cultures/globalize.cultures.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-604">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js</span></span>

    - <span data-ttu-id="5b11c-605">toutes les cultures</span><span class="sxs-lookup"><span data-stu-id="5b11c-605">all cultures</span></span>
- <span data-ttu-id="5b11c-606">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/cultures/globalize.culture. {code de culture} .js</span><span class="sxs-lookup"><span data-stu-id="5b11c-606">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js</span></span>

    - <span data-ttu-id="5b11c-607">Remplacez « {-code de culture} » avec le code de culture de votre choix, par exemple, les fichiers sur le CDN globalize.culture.en-GB.js== Microsoft == ces bibliothèques ont été téléchargés par Microsoft.</span><span class="sxs-lookup"><span data-stu-id="5b11c-607">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="5b11c-608">Répondre versions sur le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-608">Respond Releases on the CDN</span></span>

<span data-ttu-id="5b11c-609">Les versions suivantes de [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") répondre sont hébergés sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="5b11c-609">The following releases of [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") Respond are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="5b11c-610">Répondre version 1.4.2</span><span class="sxs-lookup"><span data-stu-id="5b11c-610">Respond version 1.4.2</span></span>

- <span data-ttu-id="5b11c-611">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-611">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js</span></span>
- <span data-ttu-id="5b11c-612">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-612">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js</span></span>
- <span data-ttu-id="5b11c-613">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-613">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="5b11c-614">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-614">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-141"></a><span data-ttu-id="5b11c-615">Version 1.4.1 de répondre</span><span class="sxs-lookup"><span data-stu-id="5b11c-615">Respond version 1.4.1</span></span>

- <span data-ttu-id="5b11c-616">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-616">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js</span></span>
- <span data-ttu-id="5b11c-617">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-617">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js</span></span>
- <span data-ttu-id="5b11c-618">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-618">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="5b11c-619">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-619">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-140"></a><span data-ttu-id="5b11c-620">Répondre version1.4.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-620">Respond version 1.4.0</span></span>

- <span data-ttu-id="5b11c-621">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-621">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js</span></span>
- <span data-ttu-id="5b11c-622">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-622">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js</span></span>
- <span data-ttu-id="5b11c-623">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-623">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="5b11c-624">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-624">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-130"></a><span data-ttu-id="5b11c-625">Répondre version version 1.3.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-625">Respond version 1.3.0</span></span>

- <span data-ttu-id="5b11c-626">http://AJAX.aspnetcdn.com/AJAX/Respond/1.3.0/Respond.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-626">http://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js</span></span>

#### <a name="respond-version-120"></a><span data-ttu-id="5b11c-627">Répondre version 1.2.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-627">Respond version 1.2.0</span></span>

- <span data-ttu-id="5b11c-628">http://AJAX.aspnetcdn.com/AJAX/Respond/1.2.0/Respond.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-628">http://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js</span></span>

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="5b11c-629">Versions d’amorçage sur le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-629">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="5b11c-630">Les versions suivantes de [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap sont hébergés sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="5b11c-630">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-337"></a><span data-ttu-id="5b11c-631">Version d’amorçage 3.3.7</span><span class="sxs-lookup"><span data-stu-id="5b11c-631">Bootstrap version 3.3.7</span></span>

- <span data-ttu-id="5b11c-632">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-632">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js</span></span>
- <span data-ttu-id="5b11c-633">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-633">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js</span></span>
- <span data-ttu-id="5b11c-634">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-634">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css</span></span>
- <span data-ttu-id="5b11c-635">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-635">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map</span></span>
- <span data-ttu-id="5b11c-636">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-636">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css</span></span>
- <span data-ttu-id="5b11c-637">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-637">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="5b11c-638">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-638">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="5b11c-639">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-639">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="5b11c-640">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="5b11c-640">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="5b11c-641">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="5b11c-641">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="5b11c-642">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="5b11c-642">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="5b11c-643">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="5b11c-643">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="5b11c-644">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="5b11c-644">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-336"></a><span data-ttu-id="5b11c-645">Version d’amorçage 3.3.6</span><span class="sxs-lookup"><span data-stu-id="5b11c-645">Bootstrap version 3.3.6</span></span>

- <span data-ttu-id="5b11c-646">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-646">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js</span></span>
- <span data-ttu-id="5b11c-647">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-647">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js</span></span>
- <span data-ttu-id="5b11c-648">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-648">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css</span></span>
- <span data-ttu-id="5b11c-649">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-649">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map</span></span>
- <span data-ttu-id="5b11c-650">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-650">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css</span></span>
- <span data-ttu-id="5b11c-651">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-651">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="5b11c-652">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-652">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="5b11c-653">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-653">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="5b11c-654">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="5b11c-654">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="5b11c-655">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="5b11c-655">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="5b11c-656">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="5b11c-656">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="5b11c-657">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="5b11c-657">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="5b11c-658">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="5b11c-658">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-335"></a><span data-ttu-id="5b11c-659">Amorçage version 3.3.5</span><span class="sxs-lookup"><span data-stu-id="5b11c-659">Bootstrap version 3.3.5</span></span>

- <span data-ttu-id="5b11c-660">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-660">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js</span></span>
- <span data-ttu-id="5b11c-661">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-661">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js</span></span>
- <span data-ttu-id="5b11c-662">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-662">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css</span></span>
- <span data-ttu-id="5b11c-663">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-663">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map</span></span>
- <span data-ttu-id="5b11c-664">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-664">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css</span></span>
- <span data-ttu-id="5b11c-665">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-665">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="5b11c-666">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-666">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="5b11c-667">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-667">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="5b11c-668">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="5b11c-668">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="5b11c-669">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="5b11c-669">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="5b11c-670">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="5b11c-670">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="5b11c-671">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="5b11c-671">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="5b11c-672">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="5b11c-672">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-334"></a><span data-ttu-id="5b11c-673">Version d’amorçage 3.3.4</span><span class="sxs-lookup"><span data-stu-id="5b11c-673">Bootstrap version 3.3.4</span></span>

- <span data-ttu-id="5b11c-674">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-674">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js</span></span>
- <span data-ttu-id="5b11c-675">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-675">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js</span></span>
- <span data-ttu-id="5b11c-676">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-676">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css</span></span>
- <span data-ttu-id="5b11c-677">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-677">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map</span></span>
- <span data-ttu-id="5b11c-678">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-678">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css</span></span>
- <span data-ttu-id="5b11c-679">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-679">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="5b11c-680">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-680">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="5b11c-681">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-681">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="5b11c-682">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="5b11c-682">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="5b11c-683">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="5b11c-683">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="5b11c-684">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="5b11c-684">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="5b11c-685">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="5b11c-685">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="5b11c-686">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="5b11c-686">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-332"></a><span data-ttu-id="5b11c-687">Version d’amorçage 3.3.2</span><span class="sxs-lookup"><span data-stu-id="5b11c-687">Bootstrap version 3.3.2</span></span>

- <span data-ttu-id="5b11c-688">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-688">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js</span></span>
- <span data-ttu-id="5b11c-689">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-689">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js</span></span>
- <span data-ttu-id="5b11c-690">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-690">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css</span></span>
- <span data-ttu-id="5b11c-691">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-691">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map</span></span>
- <span data-ttu-id="5b11c-692">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-692">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="5b11c-693">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-693">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="5b11c-694">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-694">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="5b11c-695">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-695">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="5b11c-696">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="5b11c-696">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="5b11c-697">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="5b11c-697">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="5b11c-698">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="5b11c-698">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="5b11c-699">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="5b11c-699">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="5b11c-700">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="5b11c-700">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-331"></a><span data-ttu-id="5b11c-701">Version d’amorçage 3.3.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-701">Bootstrap version 3.3.1</span></span>

- <span data-ttu-id="5b11c-702">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-702">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js</span></span>
- <span data-ttu-id="5b11c-703">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-703">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js</span></span>
- <span data-ttu-id="5b11c-704">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-704">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css</span></span>
- <span data-ttu-id="5b11c-705">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-705">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map</span></span>
- <span data-ttu-id="5b11c-706">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-706">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="5b11c-707">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-707">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="5b11c-708">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-708">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="5b11c-709">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-709">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="5b11c-710">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="5b11c-710">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="5b11c-711">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="5b11c-711">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="5b11c-712">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="5b11c-712">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="5b11c-713">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="5b11c-713">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-330"></a><span data-ttu-id="5b11c-714">Version d’amorçage 3.3.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-714">Bootstrap version 3.3.0</span></span>

- <span data-ttu-id="5b11c-715">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-715">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js</span></span>
- <span data-ttu-id="5b11c-716">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-716">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js</span></span>
- <span data-ttu-id="5b11c-717">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-717">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css</span></span>
- <span data-ttu-id="5b11c-718">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-718">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="5b11c-719">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-719">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="5b11c-720">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-720">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="5b11c-721">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-721">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="5b11c-722">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-722">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="5b11c-723">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="5b11c-723">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="5b11c-724">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="5b11c-724">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="5b11c-725">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="5b11c-725">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="5b11c-726">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="5b11c-726">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-320"></a><span data-ttu-id="5b11c-727">Version d’amorçage 3.2.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-727">Bootstrap version 3.2.0</span></span>

- <span data-ttu-id="5b11c-728">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-728">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js</span></span>
- <span data-ttu-id="5b11c-729">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-729">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js</span></span>
- <span data-ttu-id="5b11c-730">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-730">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css</span></span>
- <span data-ttu-id="5b11c-731">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-731">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="5b11c-732">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-732">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="5b11c-733">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-733">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="5b11c-734">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-734">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="5b11c-735">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-735">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="5b11c-736">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="5b11c-736">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="5b11c-737">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="5b11c-737">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="5b11c-738">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="5b11c-738">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="5b11c-739">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="5b11c-739">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-311"></a><span data-ttu-id="5b11c-740">Version d’amorçage 3.1.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-740">Bootstrap version 3.1.1</span></span>

- <span data-ttu-id="5b11c-741">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-741">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js</span></span>
- <span data-ttu-id="5b11c-742">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-742">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js</span></span>
- <span data-ttu-id="5b11c-743">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-743">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css</span></span>
- <span data-ttu-id="5b11c-744">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-744">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map</span></span>
- <span data-ttu-id="5b11c-745">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-745">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="5b11c-746">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-746">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="5b11c-747">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-747">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="5b11c-748">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-748">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="5b11c-749">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="5b11c-749">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="5b11c-750">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="5b11c-750">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="5b11c-751">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="5b11c-751">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="5b11c-752">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="5b11c-752">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-310"></a><span data-ttu-id="5b11c-753">Amorçage version 3.1.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-753">Bootstrap version 3.1.0</span></span>

- <span data-ttu-id="5b11c-754">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-754">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js</span></span>
- <span data-ttu-id="5b11c-755">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-755">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js</span></span>
- <span data-ttu-id="5b11c-756">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-756">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css</span></span>
- <span data-ttu-id="5b11c-757">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-757">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="5b11c-758">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-758">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="5b11c-759">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-759">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="5b11c-760">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-760">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="5b11c-761">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-761">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="5b11c-762">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="5b11c-762">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="5b11c-763">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="5b11c-763">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="5b11c-764">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="5b11c-764">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="5b11c-765">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="5b11c-765">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-303"></a><span data-ttu-id="5b11c-766">Version d’amorçage 3.0.3</span><span class="sxs-lookup"><span data-stu-id="5b11c-766">Bootstrap version 3.0.3</span></span>

- <span data-ttu-id="5b11c-767">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-767">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js</span></span>
- <span data-ttu-id="5b11c-768">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-768">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js</span></span>
- <span data-ttu-id="5b11c-769">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-769">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css</span></span>
- <span data-ttu-id="5b11c-770">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-770">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css</span></span>
- <span data-ttu-id="5b11c-771">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-771">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="5b11c-772">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-772">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="5b11c-773">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="5b11c-773">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="5b11c-774">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="5b11c-774">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="5b11c-775">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="5b11c-775">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="5b11c-776">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="5b11c-776">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-302"></a><span data-ttu-id="5b11c-777">Version d’amorçage 3.0.2</span><span class="sxs-lookup"><span data-stu-id="5b11c-777">Bootstrap version 3.0.2</span></span>

- <span data-ttu-id="5b11c-778">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-778">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js</span></span>
- <span data-ttu-id="5b11c-779">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-779">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js</span></span>
- <span data-ttu-id="5b11c-780">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-780">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css</span></span>
- <span data-ttu-id="5b11c-781">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-781">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="5b11c-782">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-782">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="5b11c-783">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-783">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="5b11c-784">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="5b11c-784">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="5b11c-785">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="5b11c-785">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="5b11c-786">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="5b11c-786">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="5b11c-787">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="5b11c-787">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-301"></a><span data-ttu-id="5b11c-788">Version d’amorçage 3.0.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-788">Bootstrap version 3.0.1</span></span>

- <span data-ttu-id="5b11c-789">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-789">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js</span></span>
- <span data-ttu-id="5b11c-790">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-790">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js</span></span>
- <span data-ttu-id="5b11c-791">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-791">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css</span></span>
- <span data-ttu-id="5b11c-792">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-792">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="5b11c-793">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-793">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="5b11c-794">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-794">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="5b11c-795">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="5b11c-795">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="5b11c-796">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="5b11c-796">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="5b11c-797">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="5b11c-797">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="5b11c-798">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="5b11c-798">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-300"></a><span data-ttu-id="5b11c-799">Version d’amorçage 3.0.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-799">Bootstrap version 3.0.0</span></span>

- <span data-ttu-id="5b11c-800">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-800">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js</span></span>
- <span data-ttu-id="5b11c-801">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-801">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js</span></span>
- <span data-ttu-id="5b11c-802">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-802">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css</span></span>
- <span data-ttu-id="5b11c-803">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-803">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="5b11c-804">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-804">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="5b11c-805">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-805">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="5b11c-806">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="5b11c-806">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="5b11c-807">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="5b11c-807">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="5b11c-808">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="5b11c-808">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="5b11c-809">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="5b11c-809">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-232"></a><span data-ttu-id="5b11c-810">Amorçage version 2.3.2</span><span class="sxs-lookup"><span data-stu-id="5b11c-810">Bootstrap version 2.3.2</span></span>

- <span data-ttu-id="5b11c-811">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-811">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js</span></span>
- <span data-ttu-id="5b11c-812">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-812">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js</span></span>
- <span data-ttu-id="5b11c-813">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-813">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css</span></span>
- <span data-ttu-id="5b11c-814">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-814">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="5b11c-815">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap-Responsive.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-815">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css</span></span>
- <span data-ttu-id="5b11c-816">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap-Responsive.min.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-816">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css</span></span>
- <span data-ttu-id="5b11c-817">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/img/glyphicons-halflings.png</span><span class="sxs-lookup"><span data-stu-id="5b11c-817">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png</span></span>
- <span data-ttu-id="5b11c-818">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/img/glyphicons-halflings-White.png</span><span class="sxs-lookup"><span data-stu-id="5b11c-818">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png</span></span>

#### <a name="bootstrap-version-231"></a><span data-ttu-id="5b11c-819">Amorçage version 2.3.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-819">Bootstrap version 2.3.1</span></span>

- <span data-ttu-id="5b11c-820">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-820">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js</span></span>
- <span data-ttu-id="5b11c-821">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-821">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js</span></span>
- <span data-ttu-id="5b11c-822">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-822">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css</span></span>
- <span data-ttu-id="5b11c-823">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-823">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="5b11c-824">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap-Responsive.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-824">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css</span></span>
- <span data-ttu-id="5b11c-825">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap-Responsive.min.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-825">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css</span></span>
- <span data-ttu-id="5b11c-826">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/img/glyphicons-halflings.png</span><span class="sxs-lookup"><span data-stu-id="5b11c-826">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png</span></span>
- <span data-ttu-id="5b11c-827">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/img/glyphicons-halflings-White.png</span><span class="sxs-lookup"><span data-stu-id="5b11c-827">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png</span></span>

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="5b11c-828">Versions TouchCarousel d’amorçage sur le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-828">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="5b11c-829">Les versions suivantes de [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") TouchCarousel du programme d’amorçage versions sont hébergées sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="5b11c-829">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="5b11c-830">Amorçage TouchCarousel version 0.8.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-830">Bootstrap TouchCarousel version 0.8.0</span></span>

- <span data-ttu-id="5b11c-831">http://AJAX.aspnetcdn.com/AJAX/Bootstrap-Touch-Carousel/0.8.0/CSS/Bootstrap-Touch-Carousel.CSS</span><span class="sxs-lookup"><span data-stu-id="5b11c-831">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css</span></span>
- <span data-ttu-id="5b11c-832">http://AJAX.aspnetcdn.com/AJAX/Bootstrap-Touch-Carousel/0.8.0/js/Bootstrap-Touch-Carousel.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-832">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js</span></span>

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="5b11c-833">Versions de Hammer.js sur le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-833">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="5b11c-834">Les versions suivantes de [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js versions sont hébergées sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="5b11c-834">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="5b11c-835">Hammer.js version 2.0.4</span><span class="sxs-lookup"><span data-stu-id="5b11c-835">Hammer.js version 2.0.4</span></span>

- <span data-ttu-id="5b11c-836">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-836">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js</span></span>
- <span data-ttu-id="5b11c-837">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-837">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js</span></span>
- <span data-ttu-id="5b11c-838">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.min.Map</span><span class="sxs-lookup"><span data-stu-id="5b11c-838">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map</span></span>

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="5b11c-839">Web Forms ASP.NET et Ajax mises en production sur le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-839">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="5b11c-840">Les versions suivantes de la bibliothèque ASP.NET Ajax sont hébergées sur le CDN.</span><span class="sxs-lookup"><span data-stu-id="5b11c-840">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="5b11c-841">Cliquez sur chaque lien pour afficher la liste réelle des fichiers.</span><span class="sxs-lookup"><span data-stu-id="5b11c-841">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="5b11c-842">Version de Web Forms ASP.NET et Ajax 4.5.2</span><span class="sxs-lookup"><span data-stu-id="5b11c-842">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "Web Forms ASP.NET et Ajax 4.5.2")
- [<span data-ttu-id="5b11c-843">Version de Web Forms ASP.NET et Ajax 4</span><span class="sxs-lookup"><span data-stu-id="5b11c-843">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "Web Forms ASP.NET et Ajax 4")
- [<span data-ttu-id="5b11c-844">ASP.NET Ajax version 3.5</span><span class="sxs-lookup"><span data-stu-id="5b11c-844">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "ASP.NET Ajax 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="5b11c-845">ASP.NET MVC publie le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-845">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="5b11c-846">Les fichiers ASP.NET MVC JavaScript suivants sont hébergés sur ce CDN :</span><span class="sxs-lookup"><span data-stu-id="5b11c-846">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="5b11c-847">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="5b11c-847">ASP.NET MVC 5.2.3</span></span>

- <span data-ttu-id="5b11c-848">http://AJAX.aspnetcdn.com/AJAX/MVC/5.2.3/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-848">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="5b11c-849">http://AJAX.aspnetcdn.com/AJAX/MVC/5.2.3/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-849">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="5b11c-850">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-850">ASP.NET MVC 5.1</span></span>

- <span data-ttu-id="5b11c-851">http://AJAX.aspnetcdn.com/AJAX/MVC/5.1/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-851">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="5b11c-852">http://AJAX.aspnetcdn.com/AJAX/MVC/5.1/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-852">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="5b11c-853">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-853">ASP.NET MVC 5.0</span></span>

- <span data-ttu-id="5b11c-854">http://AJAX.aspnetcdn.com/AJAX/MVC/5.0/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-854">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="5b11c-855">http://AJAX.aspnetcdn.com/AJAX/MVC/5.0/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-855">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="5b11c-856">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-856">ASP.NET MVC 4.0</span></span>

- <span data-ttu-id="5b11c-857">http://AJAX.aspnetcdn.com/AJAX/MVC/4.0/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-857">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="5b11c-858">http://AJAX.aspnetcdn.com/AJAX/MVC/4.0/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-858">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="5b11c-859">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-859">ASP.NET MVC 3.0</span></span>

- <span data-ttu-id="5b11c-860">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.unobtrusive-AJAX.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-860">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js</span></span>
- <span data-ttu-id="5b11c-861">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.unobtrusive-AJAX.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-861">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js</span></span>
- <span data-ttu-id="5b11c-862">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-862">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="5b11c-863">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-863">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js</span></span>
- <span data-ttu-id="5b11c-864">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-864">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="5b11c-865">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-865">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js</span></span>

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="5b11c-866">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-866">ASP.NET MVC 2.0</span></span>

- <span data-ttu-id="5b11c-867">http://AJAX.aspnetcdn.com/AJAX/MVC/2.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-867">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="5b11c-868">http://AJAX.aspnetcdn.com/AJAX/MVC/2.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-868">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js</span></span>

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="5b11c-869">ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-869">ASP.NET MVC 1.0</span></span>

- <span data-ttu-id="5b11c-870">http://AJAX.aspnetcdn.com/AJAX/MVC/1.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-870">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="5b11c-871">http://AJAX.aspnetcdn.com/AJAX/MVC/1.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-871">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js</span></span>

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="5b11c-872">ASP.NET SignalR publie le CDN</span><span class="sxs-lookup"><span data-stu-id="5b11c-872">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="5b11c-873">Les fichiers ASP.NET SignalR JavaScript suivants sont hébergés sur ce CDN :</span><span class="sxs-lookup"><span data-stu-id="5b11c-873">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="5b11c-874">ASP.NET SignalR 2.2.2</span><span class="sxs-lookup"><span data-stu-id="5b11c-874">ASP.NET SignalR 2.2.2</span></span>

- <span data-ttu-id="5b11c-875">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.2.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-875">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js</span></span>
- <span data-ttu-id="5b11c-876">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.2.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-876">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js</span></span>

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="5b11c-877">ASP.NET SignalR 2.2.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-877">ASP.NET SignalR 2.2.1</span></span>

- <span data-ttu-id="5b11c-878">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-878">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js</span></span>
- <span data-ttu-id="5b11c-879">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-879">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js</span></span>

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="5b11c-880">ASP.NET SignalR 2.2.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-880">ASP.NET SignalR 2.2.0</span></span>

- <span data-ttu-id="5b11c-881">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-881">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js</span></span>
- <span data-ttu-id="5b11c-882">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-882">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js</span></span>

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="5b11c-883">ASP.NET SignalR 2.1.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-883">ASP.NET SignalR 2.1.0</span></span>

- <span data-ttu-id="5b11c-884">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-884">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js</span></span>
- <span data-ttu-id="5b11c-885">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-885">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js</span></span>

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="5b11c-886">ASP.NET SignalR 2.0.3</span><span class="sxs-lookup"><span data-stu-id="5b11c-886">ASP.NET SignalR 2.0.3</span></span>

- <span data-ttu-id="5b11c-887">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.3.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-887">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js</span></span>
- <span data-ttu-id="5b11c-888">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.3.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-888">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js</span></span>

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="5b11c-889">ASP.NET SignalR 2.0.2</span><span class="sxs-lookup"><span data-stu-id="5b11c-889">ASP.NET SignalR 2.0.2</span></span>

- <span data-ttu-id="5b11c-890">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.2.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-890">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js</span></span>
- <span data-ttu-id="5b11c-891">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.2.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-891">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js</span></span>

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="5b11c-892">ASP.NET SignalR 2.0.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-892">ASP.NET SignalR 2.0.1</span></span>

- <span data-ttu-id="5b11c-893">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-893">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js</span></span>
- <span data-ttu-id="5b11c-894">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.1.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-894">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js</span></span>

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="5b11c-895">ASP.NET SignalR 2.0.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-895">ASP.NET SignalR 2.0.0</span></span>

- <span data-ttu-id="5b11c-896">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-896">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js</span></span>
- <span data-ttu-id="5b11c-897">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.0.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-897">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js</span></span>

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="5b11c-898">ASP.NET SignalR 1.1.3</span><span class="sxs-lookup"><span data-stu-id="5b11c-898">ASP.NET SignalR 1.1.3</span></span>

- <span data-ttu-id="5b11c-899">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.3.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-899">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js</span></span>
- <span data-ttu-id="5b11c-900">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.3.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-900">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js</span></span>

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="5b11c-901">ASP.NET SignalR 1.1.2</span><span class="sxs-lookup"><span data-stu-id="5b11c-901">ASP.NET SignalR 1.1.2</span></span>

- <span data-ttu-id="5b11c-902">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.2.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-902">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js</span></span>
- <span data-ttu-id="5b11c-903">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.2.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-903">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js</span></span>

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="5b11c-904">ASP.NET SignalR 1.1.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-904">ASP.NET SignalR 1.1.1</span></span>

- <span data-ttu-id="5b11c-905">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-905">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js</span></span>
- <span data-ttu-id="5b11c-906">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.1.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-906">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js</span></span>

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="5b11c-907">ASP.NET SignalR 1.1.0</span><span class="sxs-lookup"><span data-stu-id="5b11c-907">ASP.NET SignalR 1.1.0</span></span>

- <span data-ttu-id="5b11c-908">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-908">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js</span></span>
- <span data-ttu-id="5b11c-909">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.0.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-909">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js</span></span>

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="5b11c-910">ASP.NET SignalR 1.0.1</span><span class="sxs-lookup"><span data-stu-id="5b11c-910">ASP.NET SignalR 1.0.1</span></span>

- <span data-ttu-id="5b11c-911">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-911">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js</span></span>
- <span data-ttu-id="5b11c-912">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.0.1.js</span><span class="sxs-lookup"><span data-stu-id="5b11c-912">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js</span></span>

<span data-ttu-id="5b11c-913">Pour plus d’informations sur les conditions d’utilisation pour le CDN, consultez [Microsoft Ajax CDN conditions d’utilisation](https://www.asp.net/terms-of-use "Microsoft Ajax CDN conditions d’utilisation").</span><span class="sxs-lookup"><span data-stu-id="5b11c-913">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
