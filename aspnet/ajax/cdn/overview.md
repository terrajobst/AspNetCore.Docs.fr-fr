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
ms.openlocfilehash: f1225f06e5218d893e3f49b2ccc67d56365b30e5
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
---
<a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="f2db6-102">Réseau de diffusion de contenu Microsoft Ajax</span><span class="sxs-lookup"><span data-stu-id="f2db6-102">Microsoft Ajax Content Delivery Network</span></span>
====================
<span data-ttu-id="f2db6-103">Remarque : Le CDN Microsoft Ajax n’a aucun contrat SLA au-dessus et au-delà de l’aide d’un CDN Azure.</span><span class="sxs-lookup"><span data-stu-id="f2db6-103">Note: The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="f2db6-104">Sommaire</span><span class="sxs-lookup"><span data-stu-id="f2db6-104">Table of Contents</span></span>

<span data-ttu-id="f2db6-105">**[AJAX.Microsoft.com renommé ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="f2db6-105">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="f2db6-106">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="f2db6-106">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="f2db6-107">**[À l’aide d’ASP.NET Ajax du CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="f2db6-107">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="f2db6-108">**[À l’aide de jQuery du CDN](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="f2db6-108">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="f2db6-109">**[À l’aide de jQuery UI du CDN](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="f2db6-109">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="f2db6-110">**[Fichiers de tiers dans le CDN](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="f2db6-110">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="f2db6-111">Versions de jQuery sur le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-111">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="f2db6-112">jQuery migrer des versions sur le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-112">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="f2db6-113">jQuery UI mises en production sur le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-113">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="f2db6-114">jQuery Validation mises en production sur le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-114">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="f2db6-115">jQuery Mobile mises en production sur le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-115">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="f2db6-116">jQuery versions de modèles sur le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-116">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="f2db6-117">jQuery Cycle des versions sur le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-117">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="f2db6-118">jQuery versions de tables de données sur le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-118">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="f2db6-119">Versions de Modernizr sur le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-119">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="f2db6-120">Versions de JSHint sur le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-120">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="f2db6-121">Versions de Knockout sur le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-121">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="f2db6-122">Globaliser des versions sur le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-122">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="f2db6-123">Répondre versions sur le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-123">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="f2db6-124">Versions d’amorçage sur le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-124">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="f2db6-125">Versions TouchCarousel d’amorçage sur le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-125">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="f2db6-126">Versions de Hammer.js sur le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-126">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="f2db6-127">Web Forms ASP.NET et Ajax mises en production sur le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-127">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="f2db6-128">ASP.NET MVC publie le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-128">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="f2db6-129">ASP.NET SignalR publie le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-129">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="f2db6-130">Le contenu remise réseau CDN Microsoft Ajax () héberge des bibliothèques JavaScript de tiers populaires tels que jQuery et vous permet de facilement les ajouter à vos applications Web.</span><span class="sxs-lookup"><span data-stu-id="f2db6-130">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="f2db6-131">Par exemple, commencer à l’aide de jQuery, qui est hébergé sur ce CDN en ajoutant simplement un &lt;script&gt; balise à votre page qui pointe vers ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="f2db6-131">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="f2db6-132">En tirant parti du CDN, vous pouvez considérablement améliorer les performances de vos applications Ajax.</span><span class="sxs-lookup"><span data-stu-id="f2db6-132">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="f2db6-133">Le contenu du CDN est mis en cache sur les serveurs situés dans le monde entier.</span><span class="sxs-lookup"><span data-stu-id="f2db6-133">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="f2db6-134">En outre, le CDN permet aux navigateurs de réutiliser des fichiers JavaScript mis en cache tiers pour les sites web qui se trouvent dans des domaines différents.</span><span class="sxs-lookup"><span data-stu-id="f2db6-134">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="f2db6-135">Le CDN prend en charge SSL (HTTPS) au cas où vous deviez traiter une page web à l’aide de Secure Sockets Layer.</span><span class="sxs-lookup"><span data-stu-id="f2db6-135">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="f2db6-136">Le CDN héberge les bibliothèques de scripts tiers suivants qui ont été téléchargés et sont concédés sous licence, par les propriétaires de ces bibliothèques :</span><span class="sxs-lookup"><span data-stu-id="f2db6-136">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="f2db6-137">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="f2db6-137">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="f2db6-138">jQuery UI (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="f2db6-138">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="f2db6-139">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="f2db6-139">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="f2db6-140">jQuery Validation (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="f2db6-140">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="f2db6-141">jQuery Cycle (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="f2db6-141">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="f2db6-142">jQuery DataTables)http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="f2db6-142">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="f2db6-143">Le CDN Microsoft Ajax inclut également les bibliothèques suivantes qui ont été téléchargés par Microsoft :</span><span class="sxs-lookup"><span data-stu-id="f2db6-143">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="f2db6-144">ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="f2db6-144">ASP.NET Ajax</span></span>
- <span data-ttu-id="f2db6-145">Fichiers JavaScript ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="f2db6-145">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="f2db6-146">Les fichiers ASP.NET SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="f2db6-146">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="f2db6-147">Microsoft ne revendique pas la propriété de toutes les bibliothèques tierces hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="f2db6-147">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="f2db6-148">Les propriétaires de copyright des bibliothèques de licence ces bibliothèques pour vous.</span><span class="sxs-lookup"><span data-stu-id="f2db6-148">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="f2db6-149">Les droits que vous devrez peut-être télécharger et utiliser ces bibliothèques sont accordées exclusivement par les propriétaires de copyright respectifs.</span><span class="sxs-lookup"><span data-stu-id="f2db6-149">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="f2db6-150">Étant donné que celles-ci ne sont pas des bibliothèques de Microsoft, Microsoft ne fournit aucune garantie ou les licences de droits de propriété intellectuelle (y compris sans droits de brevet implicites) pour les bibliothèques tierces hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="f2db6-150">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="f2db6-151">Si vous souhaitez soumettre votre bibliothèque JavaScript et votre bibliothèque est une des bibliothèques JavaScript supérieur (tel qu’indiqué dans http://trends.builtwith.com) ou extensions/plug-ins pour ces bibliothèques qui sont (a) populaires ; ou (b) utile pour une utilisation sur ASP.NET, puis contactez AjaxCDNSubmission@Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="f2db6-151">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="f2db6-152">AJAX.Microsoft.com renommé ajax.aspnetcdn.com</span><span class="sxs-lookup"><span data-stu-id="f2db6-152">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="f2db6-153">Le CDN permet d’utiliser le nom de domaine microsoft.com et a été modifié pour utiliser le nom de domaine aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="f2db6-153">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="f2db6-154">Cette modification a été apportée pour augmenter les performances, car lorsqu’un navigateur référencé le domaine microsoft.com il envoie tous les cookies à partir de ce domaine sur le réseau avec chaque demande.</span><span class="sxs-lookup"><span data-stu-id="f2db6-154">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="f2db6-155">En renommant à un nom de domaine différent de microsoft.com performances peuvent être augmenté par autant de 25 %.</span><span class="sxs-lookup"><span data-stu-id="f2db6-155">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="f2db6-156">Remarque ajax.microsoft.com continuera de fonctionner mais ajax.aspnetcdn.com est recommandé.</span><span class="sxs-lookup"><span data-stu-id="f2db6-156">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="f2db6-157">Ancien Format : http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="f2db6-157">Old Format: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="f2db6-158">Nouveau Format : http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="f2db6-158">New Format: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="f2db6-159">Prise en charge de Visual Studio .vsdoc</span><span class="sxs-lookup"><span data-stu-id="f2db6-159">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="f2db6-160">Pour utiliser les fichiers .vsdoc correctement avec Visual Studio 2008, vous devez vous assurer que vous disposez de Visual Studio 2008 SP1 installé et le correctif logiciel pour les fichiers vsdoc installé.</span><span class="sxs-lookup"><span data-stu-id="f2db6-160">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="f2db6-161">Vous pouvez obtenir à partir d’ici :</span><span class="sxs-lookup"><span data-stu-id="f2db6-161">You can get these from here:</span></span>

- [<span data-ttu-id="f2db6-162">Télécharger Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="f2db6-162">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "télécharger Visual Studio 2008 SP1")
- [<span data-ttu-id="f2db6-163">Télécharger le correctif logiciel de .vsdoc pour Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="f2db6-163">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 "télécharger .vsdoc correctif pour Visual Studio 2008 SP1")

<span data-ttu-id="f2db6-164">Visual Studio 2010 prend en charge les fichiers .vsdoc sans les correctifs supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="f2db6-164">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="f2db6-165">À l’aide d’ASP.NET Ajax du CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-165">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="f2db6-166">Lorsque vous utilisez ASP.NET 4, vous pouvez rediriger toutes les demandes pour les scripts du framework ASP.NET vers le CDN.</span><span class="sxs-lookup"><span data-stu-id="f2db6-166">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="f2db6-167">La récupération des scripts à partir du CDN au lieu de votre serveur web local peuvent considérablement améliorer les performances des sites Web ASP.NET publics.</span><span class="sxs-lookup"><span data-stu-id="f2db6-167">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="f2db6-168">Utilisez la propriété ScriptManager EnableCDN pour rediriger toutes les demandes de script framework ASP.NET pour le CDN Microsoft Ajax :</span><span class="sxs-lookup"><span data-stu-id="f2db6-168">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="f2db6-169">À l’aide de jQuery du CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-169">Using jQuery from the CDN</span></span>

<span data-ttu-id="f2db6-170">Vous pouvez utiliser des scripts jQuery hébergés sur CDN dans votre application Web en ajoutant l’élément de script suivant à une page :</span><span class="sxs-lookup"><span data-stu-id="f2db6-170">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="f2db6-171">Le CDN inclut également la version réduite du script jQuery, que vous pouvez obtenir à l’aide de l’élément suivant :</span><span class="sxs-lookup"><span data-stu-id="f2db6-171">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="f2db6-172">Pour permettre à votre page à revenir au chargement jQuery à partir d’un chemin d’accès local sur votre propre site Web si le CDN est indisponible, ajoutez l’élément suivant immédiatement après l’élément référençant le CDN :</span><span class="sxs-lookup"><span data-stu-id="f2db6-172">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="f2db6-173">L’exemple de page suivant utilise la version CDN de la bibliothèque jQuery (avec basculement vers une copie locale) pour afficher le contenu d’un élément div lorsqu’un clic est effectué.</span><span class="sxs-lookup"><span data-stu-id="f2db6-173">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="f2db6-174">Vous pouvez en savoir plus sur jQuery et télécharger une copie locale de jQuery en vous rendant sur le [jQuery](http://jquery.com/) site Web.</span><span class="sxs-lookup"><span data-stu-id="f2db6-174">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="f2db6-175">À l’aide de jQuery UI du CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-175">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="f2db6-176">Le CDN héberge également la bibliothèque jQuery UI.</span><span class="sxs-lookup"><span data-stu-id="f2db6-176">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="f2db6-177">La bibliothèque jQuery UI comprend un ensemble complet de widgets et les effets que vous pouvez utiliser dans vos applications ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f2db6-177">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="f2db6-178">Par exemple, la page suivante illustre comment vous pouvez utiliser la sélecteur de dates de l’interface utilisateur jQuery dans le contexte d’une application Web Forms ASP.NET pour afficher un calendrier contextuel :</span><span class="sxs-lookup"><span data-stu-id="f2db6-178">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="f2db6-179">Lorsque vous déplacez le focus vers la zone de texte à l’aide de votre clavier, un calendrier s’affiche :</span><span class="sxs-lookup"><span data-stu-id="f2db6-179">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Calendrier contextuel créé avec le sélecteur de dates](overview/_static/image1.png)

<span data-ttu-id="f2db6-181">Notez que vous devez inclure les trois fichiers du CDN dans le code ci-dessus :</span><span class="sxs-lookup"><span data-stu-id="f2db6-181">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="f2db6-182">La bibliothèque jQuery &mdash; la bibliothèque jQuery UI dépend de la bibliothèque jQuery.</span><span class="sxs-lookup"><span data-stu-id="f2db6-182">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="f2db6-183">Vous devez ajouter la bibliothèque jQuery à votre page avant d’ajouter la bibliothèque jQuery UI.</span><span class="sxs-lookup"><span data-stu-id="f2db6-183">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="f2db6-184">La bibliothèque jQuery UI &mdash; la bibliothèque jQuery UI contient tous les effets d’interface utilisateur jQuery et widgets tels que le widget de sélecteur de dates utilisées dans la page ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="f2db6-184">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="f2db6-185">Un thème de l’interface utilisateur jQuery &mdash; jQuery UI prend en charge différents thèmes.</span><span class="sxs-lookup"><span data-stu-id="f2db6-185">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="f2db6-186">La page ci-dessus inclut un lien vers un fichier CSS pour importer le thème de Redmond.</span><span class="sxs-lookup"><span data-stu-id="f2db6-186">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="f2db6-187">Tous les thèmes de l’interface utilisateur jQuery standard sont hébergés sur le CDN.</span><span class="sxs-lookup"><span data-stu-id="f2db6-187">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="f2db6-188">[Visitez cette page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 dans le CDN Microsoft Ajax") pour afficher les miniatures pour chaque thème.</span><span class="sxs-lookup"><span data-stu-id="f2db6-188">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="f2db6-189">Pour plus d’informations sur la bibliothèque jQuery UI, visitez officielle [site Web de l’interface utilisateur jQuery](http://jQueryUI.com "site Web de l’interface utilisateur jQuery").</span><span class="sxs-lookup"><span data-stu-id="f2db6-189">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="f2db6-190">Fichiers de tiers dans le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-190">Third-Party Files on the CDN</span></span>

<span data-ttu-id="f2db6-191">Le CDN héberge certains des bibliothèques JavaScript tiers les plus populaires.</span><span class="sxs-lookup"><span data-stu-id="f2db6-191">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="f2db6-192">Microsoft ne revendique pas la propriété de toutes les bibliothèques tierces hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="f2db6-192">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="f2db6-193">Les propriétaires de copyright des bibliothèques de licence ces bibliothèques pour vous.</span><span class="sxs-lookup"><span data-stu-id="f2db6-193">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="f2db6-194">Les droits que vous devrez peut-être télécharger et utiliser ces bibliothèques sont accordées exclusivement par les propriétaires de copyright respectifs.</span><span class="sxs-lookup"><span data-stu-id="f2db6-194">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="f2db6-195">Étant donné que celles-ci ne sont pas des bibliothèques de Microsoft, Microsoft ne fournit aucune garantie ou les licences de droits de propriété intellectuelle (y compris sans droits de brevet implicites) pour les bibliothèques tierces hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="f2db6-195">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="f2db6-196">Versions de jQuery sur le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-196">jQuery Releases on the CDN</span></span>

<span data-ttu-id="f2db6-197">Les versions suivantes de jQuery sont hébergées sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="f2db6-197">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-331"></a><span data-ttu-id="f2db6-198">version de jQuery 3.3.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-198">jQuery version 3.3.1</span></span>
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map

#### <a name="jquery-version-321"></a><span data-ttu-id="f2db6-199">jQuery version 3.2.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-199">jQuery version 3.2.1</span></span>
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map

#### <a name="jquery-version-320"></a><span data-ttu-id="f2db6-200">version de jQuery 3.2.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-200">jQuery version 3.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map

#### <a name="jquery-version-311"></a><span data-ttu-id="f2db6-201">version de jQuery 3.1.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-201">jQuery version 3.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map

#### <a name="jquery-version-310"></a><span data-ttu-id="f2db6-202">jQuery version 3.1.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-202">jQuery version 3.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map

#### <a name="jquery-version-300"></a><span data-ttu-id="f2db6-203">version de jQuery 3.0.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-203">jQuery version 3.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map

#### <a name="jquery-version-224"></a><span data-ttu-id="f2db6-204">version de jQuery 2.2.4</span><span class="sxs-lookup"><span data-stu-id="f2db6-204">jQuery version 2.2.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map

#### <a name="jquery-version-223"></a><span data-ttu-id="f2db6-205">version de jQuery 2.2.3</span><span class="sxs-lookup"><span data-stu-id="f2db6-205">jQuery version 2.2.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map

#### <a name="jquery-version-222"></a><span data-ttu-id="f2db6-206">version de jQuery 2.2.2</span><span class="sxs-lookup"><span data-stu-id="f2db6-206">jQuery version 2.2.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map

#### <a name="jquery-version-221"></a><span data-ttu-id="f2db6-207">jQuery version 2.2.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-207">jQuery version 2.2.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map

#### <a name="jquery-version-220"></a><span data-ttu-id="f2db6-208">jQuery version2.2.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-208">jQuery version 2.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map

#### <a name="jquery-version-214"></a><span data-ttu-id="f2db6-209">version de jQuery 2.1.4</span><span class="sxs-lookup"><span data-stu-id="f2db6-209">jQuery version 2.1.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map

#### <a name="jquery-version-213"></a><span data-ttu-id="f2db6-210">version de jQuery 2.1.3</span><span class="sxs-lookup"><span data-stu-id="f2db6-210">jQuery version 2.1.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map

#### <a name="jquery-version-212"></a><span data-ttu-id="f2db6-211">version de jQuery 2.1.2</span><span class="sxs-lookup"><span data-stu-id="f2db6-211">jQuery version 2.1.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js

#### <a name="jquery-version-211"></a><span data-ttu-id="f2db6-212">version de jQuery 2.1.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-212">jQuery version 2.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map

#### <a name="jquery-version-210"></a><span data-ttu-id="f2db6-213">version de jQuery 2.1.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-213">jQuery version 2.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map

#### <a name="jquery-version-203"></a><span data-ttu-id="f2db6-214">version de jQuery 2.0.3</span><span class="sxs-lookup"><span data-stu-id="f2db6-214">jQuery version 2.0.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map

#### <a name="jquery-version-202"></a><span data-ttu-id="f2db6-215">version de jQuery 2.0.2</span><span class="sxs-lookup"><span data-stu-id="f2db6-215">jQuery version 2.0.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map

#### <a name="jquery-version-201"></a><span data-ttu-id="f2db6-216">version de jQuery 2.0.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-216">jQuery version 2.0.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map

#### <a name="jquery-version-200"></a><span data-ttu-id="f2db6-217">jQuery version 2.0.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-217">jQuery version 2.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map

#### <a name="jquery-version-1124"></a><span data-ttu-id="f2db6-218">version de jQuery 1.12.4</span><span class="sxs-lookup"><span data-stu-id="f2db6-218">jQuery version 1.12.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map

#### <a name="jquery-version-1123"></a><span data-ttu-id="f2db6-219">version de jQuery 1.12.3</span><span class="sxs-lookup"><span data-stu-id="f2db6-219">jQuery version 1.12.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map

#### <a name="jquery-version-1122"></a><span data-ttu-id="f2db6-220">version de jQuery 1.12.2</span><span class="sxs-lookup"><span data-stu-id="f2db6-220">jQuery version 1.12.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map

#### <a name="jquery-version-1121"></a><span data-ttu-id="f2db6-221">version de jQuery 1.12.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-221">jQuery version 1.12.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map

#### <a name="jquery-version-1120"></a><span data-ttu-id="f2db6-222">version de jQuery 1.12.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-222">jQuery version 1.12.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map

#### <a name="jquery-version-1113"></a><span data-ttu-id="f2db6-223">version de jQuery 1.11.3</span><span class="sxs-lookup"><span data-stu-id="f2db6-223">jQuery version 1.11.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map

#### <a name="jquery-version-1112"></a><span data-ttu-id="f2db6-224">version de jQuery 1.11.2</span><span class="sxs-lookup"><span data-stu-id="f2db6-224">jQuery version 1.11.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map

#### <a name="jquery-version-1111"></a><span data-ttu-id="f2db6-225">version de jQuery 1.11.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-225">jQuery version 1.11.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map

#### <a name="jquery-version-1110"></a><span data-ttu-id="f2db6-226">version de jQuery 1.11.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-226">jQuery version 1.11.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map

#### <a name="jquery-version-1102"></a><span data-ttu-id="f2db6-227">version de jQuery 1.10.2</span><span class="sxs-lookup"><span data-stu-id="f2db6-227">jQuery version 1.10.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map

#### <a name="jquery-version-1101"></a><span data-ttu-id="f2db6-228">version de jQuery 1.10.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-228">jQuery version 1.10.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map

#### <a name="jquery-version-1100"></a><span data-ttu-id="f2db6-229">version de jQuery 1.10.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-229">jQuery version 1.10.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map

#### <a name="jquery-version-191"></a><span data-ttu-id="f2db6-230">version de jQuery 1.9.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-230">jQuery version 1.9.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map

#### <a name="jquery-version-190"></a><span data-ttu-id="f2db6-231">version de jQuery à 1.9.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-231">jQuery version 1.9.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map

#### <a name="jquery-version-183"></a><span data-ttu-id="f2db6-232">version de jQuery 1.8.3</span><span class="sxs-lookup"><span data-stu-id="f2db6-232">jQuery version 1.8.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js

#### <a name="jquery-version-182"></a><span data-ttu-id="f2db6-233">version de jQuery 1.8.2</span><span class="sxs-lookup"><span data-stu-id="f2db6-233">jQuery version 1.8.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js

#### <a name="jquery-version-181"></a><span data-ttu-id="f2db6-234">version de jQuery 1.8.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-234">jQuery version 1.8.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js

#### <a name="jquery-version-180"></a><span data-ttu-id="f2db6-235">version de jQuery 1.8.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-235">jQuery version 1.8.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js

#### <a name="jquery-version-172"></a><span data-ttu-id="f2db6-236">version de jQuery 1.7.2</span><span class="sxs-lookup"><span data-stu-id="f2db6-236">jQuery version 1.7.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js

#### <a name="jquery-version-171"></a><span data-ttu-id="f2db6-237">jQuery version 1.7.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-237">jQuery version 1.7.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js

#### <a name="jquery-version-17"></a><span data-ttu-id="f2db6-238">jQuery version 1.7</span><span class="sxs-lookup"><span data-stu-id="f2db6-238">jQuery version 1.7</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js

#### <a name="jquery-version-164"></a><span data-ttu-id="f2db6-239">version de jQuery 1.6.4</span><span class="sxs-lookup"><span data-stu-id="f2db6-239">jQuery version 1.6.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js

#### <a name="jquery-version-163"></a><span data-ttu-id="f2db6-240">version de jQuery 1.6.3</span><span class="sxs-lookup"><span data-stu-id="f2db6-240">jQuery version 1.6.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js

#### <a name="jquery-version-162"></a><span data-ttu-id="f2db6-241">version de jQuery 1.6.2</span><span class="sxs-lookup"><span data-stu-id="f2db6-241">jQuery version 1.6.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js

#### <a name="jquery-version-161"></a><span data-ttu-id="f2db6-242">version de jQuery 1.6.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-242">jQuery version 1.6.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js

#### <a name="jquery-version-16"></a><span data-ttu-id="f2db6-243">jQuery version 1.6</span><span class="sxs-lookup"><span data-stu-id="f2db6-243">jQuery version 1.6</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js

#### <a name="jquery-version-152"></a><span data-ttu-id="f2db6-244">version de jQuery 1.5.2</span><span class="sxs-lookup"><span data-stu-id="f2db6-244">jQuery version 1.5.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js

#### <a name="jquery-version-151"></a><span data-ttu-id="f2db6-245">version de jQuery 1.5.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-245">jQuery version 1.5.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js

#### <a name="jquery-version-15"></a><span data-ttu-id="f2db6-246">version de jQuery 1.5</span><span class="sxs-lookup"><span data-stu-id="f2db6-246">jQuery version 1.5</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js

#### <a name="jquery-version-144"></a><span data-ttu-id="f2db6-247">version de jQuery 1.4.4</span><span class="sxs-lookup"><span data-stu-id="f2db6-247">jQuery version 1.4.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js

#### <a name="jquery-version-143"></a><span data-ttu-id="f2db6-248">version de jQuery 1.4.3</span><span class="sxs-lookup"><span data-stu-id="f2db6-248">jQuery version 1.4.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js

#### <a name="jquery-version-142"></a><span data-ttu-id="f2db6-249">version de jQuery 1.4.2</span><span class="sxs-lookup"><span data-stu-id="f2db6-249">jQuery version 1.4.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js

#### <a name="jquery-version-141"></a><span data-ttu-id="f2db6-250">version de jQuery 1.4.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-250">jQuery version 1.4.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js

#### <a name="jquery-version-14"></a><span data-ttu-id="f2db6-251">jQuery version 1.4</span><span class="sxs-lookup"><span data-stu-id="f2db6-251">jQuery version 1.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js

#### <a name="jquery-version-132"></a><span data-ttu-id="f2db6-252">version de jQuery 1.3.2</span><span class="sxs-lookup"><span data-stu-id="f2db6-252">jQuery version 1.3.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="f2db6-253">jQuery migrer des versions sur le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-253">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="f2db6-254">Les versions suivantes de jQuery migrer sont hébergées sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="f2db6-254">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="f2db6-255">jQuery migration version 3.0.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-255">jQuery Migrate version 3.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="f2db6-256">jQuery migration version 1.2.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-256">jQuery Migrate version 1.2.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js

<span data-ttu-id="f2db6-257">jQuery migration version 1.2.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-257">jQuery Migrate version 1.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="f2db6-258">jQuery migration version 1.1.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-258">jQuery Migrate version 1.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="f2db6-259">jQuery migration version 1.1.0.</span><span class="sxs-lookup"><span data-stu-id="f2db6-259">jQuery Migrate version 1.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="f2db6-260">Migrer la version 1.0.0 de jQuery</span><span class="sxs-lookup"><span data-stu-id="f2db6-260">jQuery Migrate version 1.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="f2db6-261">jQuery UI mises en production sur le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-261">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="f2db6-262">Les versions suivantes de la bibliothèque jQuery UI sont hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="f2db6-262">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="f2db6-263">Cliquez sur chaque lien pour afficher la liste réelle des fichiers.</span><span class="sxs-lookup"><span data-stu-id="f2db6-263">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="f2db6-264">jQuery UI 1.12.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-264">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery UI 1.12.1 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-265">jQuery UI 1.12.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-265">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "jQuery UI 1.12.0 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-266">jQuery UI 1.11.4</span><span class="sxs-lookup"><span data-stu-id="f2db6-266">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "jQuery UI 1.11.4 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-267">jQuery UI 1.11.3</span><span class="sxs-lookup"><span data-stu-id="f2db6-267">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery UI 1.11.3 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-268">jQuery UI 1.11.2</span><span class="sxs-lookup"><span data-stu-id="f2db6-268">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery UI 1.11.2 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-269">jQuery UI 1.11.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-269">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "jQuery UI 1.11.1 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-270">jQuery UI 1.11.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-270">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "jQuery UI 1.11.0 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-271">jQuery UI 1.10.4</span><span class="sxs-lookup"><span data-stu-id="f2db6-271">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery UI 1.10.4 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-272">jQuery UI 1.10.3</span><span class="sxs-lookup"><span data-stu-id="f2db6-272">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery UI 1.10.3 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-273">jQuery UI 1.10.2</span><span class="sxs-lookup"><span data-stu-id="f2db6-273">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery UI 1.10.2 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-274">jQuery UI 1.10.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-274">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery UI 1.10.1 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-275">jQuery UI 1.10.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-275">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery UI 1.10.0 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-276">jQuery UI 1.9.2</span><span class="sxs-lookup"><span data-stu-id="f2db6-276">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "jQuery UI 1.9.2 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-277">jQuery UI 1.9.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-277">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "jQuery UI 1.9.1 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-278">jQuery UI à 1.9.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-278">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "jQuery UI à 1.9.0 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-279">jQuery UI 1.8.24</span><span class="sxs-lookup"><span data-stu-id="f2db6-279">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery UI 1.8.24 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-280">jQuery UI 1.8.23</span><span class="sxs-lookup"><span data-stu-id="f2db6-280">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery UI 1.8.23 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-281">jQuery UI 1.8.22</span><span class="sxs-lookup"><span data-stu-id="f2db6-281">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery UI 1.8.22 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-282">jQuery UI 1.8.21</span><span class="sxs-lookup"><span data-stu-id="f2db6-282">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery UI 1.8.21 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-283">jQuery UI 1.8.20</span><span class="sxs-lookup"><span data-stu-id="f2db6-283">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery UI 1.8.20 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-284">jQuery UI 1.8.19</span><span class="sxs-lookup"><span data-stu-id="f2db6-284">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery UI 1.8.19 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-285">jQuery UI 1.8.18</span><span class="sxs-lookup"><span data-stu-id="f2db6-285">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery UI 1.8.18 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-286">jQuery UI 1.8.17</span><span class="sxs-lookup"><span data-stu-id="f2db6-286">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery UI 1.8.17 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-287">jQuery UI 1.8.16</span><span class="sxs-lookup"><span data-stu-id="f2db6-287">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery UI 1.8.16 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-288">jQuery UI 1.8.15</span><span class="sxs-lookup"><span data-stu-id="f2db6-288">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery UI 1.8.15 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-289">jQuery UI 1.8.14</span><span class="sxs-lookup"><span data-stu-id="f2db6-289">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery UI 1.8.14 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-290">jQuery UI 1.8.13</span><span class="sxs-lookup"><span data-stu-id="f2db6-290">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery UI 1.8.13 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-291">jQuery UI 1.8.12</span><span class="sxs-lookup"><span data-stu-id="f2db6-291">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery UI 1.8.12 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-292">jQuery UI 1.8.11</span><span class="sxs-lookup"><span data-stu-id="f2db6-292">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery UI 1.8.11 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-293">jQuery UI 1.8.10</span><span class="sxs-lookup"><span data-stu-id="f2db6-293">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-294">jQuery UI 1.8.9</span><span class="sxs-lookup"><span data-stu-id="f2db6-294">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "jQuery UI 1.8.9 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-295">jQuery UI 1.8.8</span><span class="sxs-lookup"><span data-stu-id="f2db6-295">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "jQuery UI 1.8.8 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-296">jQuery UI 1.8.7</span><span class="sxs-lookup"><span data-stu-id="f2db6-296">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery UI 1.8.7 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-297">jQuery UI 1.8.6</span><span class="sxs-lookup"><span data-stu-id="f2db6-297">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery UI 1.8.6 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-298">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="f2db6-298">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="f2db6-299">jQuery Validation mises en production sur le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-299">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="f2db6-300">Les versions suivantes de la bibliothèque de Validation jQuery sont hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="f2db6-300">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="f2db6-301">Cliquez sur chaque lien pour afficher la liste réelle des fichiers.</span><span class="sxs-lookup"><span data-stu-id="f2db6-301">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="f2db6-302">jQuery validation 1.17.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-302">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "jQuery Validation 1.17.0")
- [<span data-ttu-id="f2db6-303">jQuery validation 1.16.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-303">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "jQuery Validation 1.16.0")
- [<span data-ttu-id="f2db6-304">jQuery validation 1.15.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-304">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "jQuery Validation 1.15.1")
- [<span data-ttu-id="f2db6-305">jQuery validation 1.15.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-305">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "jQuery Validation 1.15.0")
- [<span data-ttu-id="f2db6-306">jQuery validation 1.14.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-306">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "jQuery Validation 1.14.0")
- [<span data-ttu-id="f2db6-307">jQuery validation 1.13.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-307">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "jQuery Validation 1.13.1")
- [<span data-ttu-id="f2db6-308">jQuery validation 1.13.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-308">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "jQuery Validation 1.13.0")
- [<span data-ttu-id="f2db6-309">jQuery validation 1.12.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-309">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "jQuery Validation 1.12.0")
- [<span data-ttu-id="f2db6-310">jQuery validation 1.11.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-310">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "jQuery Validation 1.11.1")
- [<span data-ttu-id="f2db6-311">jQuery validation 1.11.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-311">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "jQuery Validation 1.11.0")
- [<span data-ttu-id="f2db6-312">jQuery validation 1.10.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-312">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "jQuery Validation 1.10.0")
- [<span data-ttu-id="f2db6-313">jQuery validation 1.9</span><span class="sxs-lookup"><span data-stu-id="f2db6-313">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate version 1.9")
- [<span data-ttu-id="f2db6-314">jQuery validation 1.8.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-314">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jquery.validate version 1.8.1")
- [<span data-ttu-id="f2db6-315">jQuery validation 1.8</span><span class="sxs-lookup"><span data-stu-id="f2db6-315">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate version 1.8")
- [<span data-ttu-id="f2db6-316">jQuery validation 1.7</span><span class="sxs-lookup"><span data-stu-id="f2db6-316">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate version 1.7")
- [<span data-ttu-id="f2db6-317">jQuery validation 1.6</span><span class="sxs-lookup"><span data-stu-id="f2db6-317">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery validation 1.6")
- [<span data-ttu-id="f2db6-318">jQuery validation 1.5.5</span><span class="sxs-lookup"><span data-stu-id="f2db6-318">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "jQuery validation 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="f2db6-319">jQuery Mobile mises en production sur le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-319">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="f2db6-320">Les versions suivantes de la bibliothèque de jQuery Mobile sont hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="f2db6-320">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="f2db6-321">Cliquez sur chaque lien pour afficher la liste réelle des fichiers.</span><span class="sxs-lookup"><span data-stu-id="f2db6-321">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="f2db6-322">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="f2db6-322">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-323">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="f2db6-323">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-324">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-324">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-325">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-325">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-326">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="f2db6-326">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-327">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-327">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-328">jQuery Mobile version 1.3.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-328">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile version 1.3.0 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-329">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-329">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-330">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="f2db6-330">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-331">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-331">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-332">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-332">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-333">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="f2db6-333">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-334">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-334">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-335">jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-335">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-336">jQuery Mobile 1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="f2db6-336">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-337">jQuery Mobile 1.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="f2db6-337">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="f2db6-338">version bêta 3 de jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-338">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 bêta 3 sur le CDN Microsoft Ajax")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="f2db6-339">jQuery versions de modèles sur le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-339">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="f2db6-340">Les versions suivantes du plug-in de modèles jQuery sont hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="f2db6-340">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="f2db6-341">Cliquez sur chaque lien pour afficher la liste réelle des fichiers.</span><span class="sxs-lookup"><span data-stu-id="f2db6-341">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="f2db6-342">jQuery modèles bêta 1</span><span class="sxs-lookup"><span data-stu-id="f2db6-342">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery modèles bêta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="f2db6-343">jQuery Cycle des versions sur le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-343">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="f2db6-344">Les versions suivantes du plug-in du Cycle de jQuery sont hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="f2db6-344">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="f2db6-345">Cliquez sur chaque lien pour afficher la liste réelle des fichiers.</span><span class="sxs-lookup"><span data-stu-id="f2db6-345">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="f2db6-346">jQuery Cycle 2,99</span><span class="sxs-lookup"><span data-stu-id="f2db6-346">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery 2,99 de Cycle")
- [<span data-ttu-id="f2db6-347">jQuery Cycle 2.94</span><span class="sxs-lookup"><span data-stu-id="f2db6-347">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery Cycle 2.94")
- [<span data-ttu-id="f2db6-348">jQuery Cycle 2,88</span><span class="sxs-lookup"><span data-stu-id="f2db6-348">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery 2,88 de Cycle")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="f2db6-349">jQuery versions de tables de données sur le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-349">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="f2db6-350">Les versions suivantes du plug-in DataTables de jQuery sont hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="f2db6-350">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="f2db6-351">Cliquez sur chaque lien pour afficher la liste réelle des fichiers.</span><span class="sxs-lookup"><span data-stu-id="f2db6-351">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="f2db6-352">jQuery DataTables 1.10.5</span><span class="sxs-lookup"><span data-stu-id="f2db6-352">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [<span data-ttu-id="f2db6-353">jQuery DataTables 1.10.4</span><span class="sxs-lookup"><span data-stu-id="f2db6-353">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [<span data-ttu-id="f2db6-354">jQuery DataTables 1.9.4</span><span class="sxs-lookup"><span data-stu-id="f2db6-354">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables 1.9.4")
- [<span data-ttu-id="f2db6-355">jQuery DataTables 1.9.3</span><span class="sxs-lookup"><span data-stu-id="f2db6-355">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [<span data-ttu-id="f2db6-356">jQuery DataTables 1.9.2</span><span class="sxs-lookup"><span data-stu-id="f2db6-356">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [<span data-ttu-id="f2db6-357">jQuery DataTables 1.9.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-357">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [<span data-ttu-id="f2db6-358">jQuery DataTables à 1.9.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-358">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables à 1.9.0")
- [<span data-ttu-id="f2db6-359">jQuery DataTables 1.8.2</span><span class="sxs-lookup"><span data-stu-id="f2db6-359">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="f2db6-360">Versions de Modernizr sur le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-360">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="f2db6-361">Les versions suivantes de [Modernizr](http://www.modernizr.com "Modernizr") sont hébergés sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="f2db6-361">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="f2db6-362">Versions de JSHint sur le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-362">JSHint Releases on the CDN</span></span>

<span data-ttu-id="f2db6-363">Les versions suivantes de [JSHint](http://www.jshint.com "JSHint") sont hébergés sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="f2db6-363">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- http://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="f2db6-364">Versions de Knockout sur le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-364">Knockout Releases on the CDN</span></span>

<span data-ttu-id="f2db6-365">Les versions suivantes de [Knockout](http://www.knockoutjs.com "Knockout") sont hébergés sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="f2db6-365">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="f2db6-366">Globaliser des versions sur le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-366">Globalize Releases on the CDN</span></span>

<span data-ttu-id="f2db6-367">Les versions suivantes de [Globalize](https://github.com/jquery/globalize "Globalize") sont hébergés sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="f2db6-367">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="f2db6-368">La version 1.0.0 de globalisation</span><span class="sxs-lookup"><span data-stu-id="f2db6-368">Globalize version 1.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js

#### <a name="globalize-version-011"></a><span data-ttu-id="f2db6-369">Globaliser version 0.1.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-369">Globalize version 0.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js
- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js
- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js

    - <span data-ttu-id="f2db6-370">toutes les cultures</span><span class="sxs-lookup"><span data-stu-id="f2db6-370">all cultures</span></span>
- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js

    - <span data-ttu-id="f2db6-371">Remplacez « {-code de culture} » avec le code de culture de votre choix, par exemple, les fichiers sur le CDN globalize.culture.en-GB.js== Microsoft == ces bibliothèques ont été téléchargés par Microsoft.</span><span class="sxs-lookup"><span data-stu-id="f2db6-371">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="f2db6-372">Répondre versions sur le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-372">Respond Releases on the CDN</span></span>

<span data-ttu-id="f2db6-373">Les versions suivantes de [ https://github.com/scottjehl/Respond ] (https://github.com/scottjehl/Respond " https://github.com/scottjehl/Respond ") répondre sont hébergés sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="f2db6-373">The following releases of [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") Respond are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="f2db6-374">Répondre version 1.4.2</span><span class="sxs-lookup"><span data-stu-id="f2db6-374">Respond version 1.4.2</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js

#### <a name="respond-version-141"></a><span data-ttu-id="f2db6-375">Version 1.4.1 de répondre</span><span class="sxs-lookup"><span data-stu-id="f2db6-375">Respond version 1.4.1</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js

#### <a name="respond-version-140"></a><span data-ttu-id="f2db6-376">Répondre version1.4.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-376">Respond version 1.4.0</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js

#### <a name="respond-version-130"></a><span data-ttu-id="f2db6-377">Répondre version version 1.3.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-377">Respond version 1.3.0</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js

#### <a name="respond-version-120"></a><span data-ttu-id="f2db6-378">Répondre version 1.2.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-378">Respond version 1.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="f2db6-379">Versions d’amorçage sur le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-379">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="f2db6-380">Les versions suivantes de [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap sont hébergés sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="f2db6-380">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-400"></a><span data-ttu-id="f2db6-381">Amorçage version 4.0.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-381">Bootstrap version 4.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-337"></a><span data-ttu-id="f2db6-382">Version d’amorçage 3.3.7</span><span class="sxs-lookup"><span data-stu-id="f2db6-382">Bootstrap version 3.3.7</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-336"></a><span data-ttu-id="f2db6-383">Version d’amorçage 3.3.6</span><span class="sxs-lookup"><span data-stu-id="f2db6-383">Bootstrap version 3.3.6</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-335"></a><span data-ttu-id="f2db6-384">Amorçage version 3.3.5</span><span class="sxs-lookup"><span data-stu-id="f2db6-384">Bootstrap version 3.3.5</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-334"></a><span data-ttu-id="f2db6-385">Version d’amorçage 3.3.4</span><span class="sxs-lookup"><span data-stu-id="f2db6-385">Bootstrap version 3.3.4</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-332"></a><span data-ttu-id="f2db6-386">Version d’amorçage 3.3.2</span><span class="sxs-lookup"><span data-stu-id="f2db6-386">Bootstrap version 3.3.2</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-331"></a><span data-ttu-id="f2db6-387">Version d’amorçage 3.3.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-387">Bootstrap version 3.3.1</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-330"></a><span data-ttu-id="f2db6-388">Version d’amorçage 3.3.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-388">Bootstrap version 3.3.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-320"></a><span data-ttu-id="f2db6-389">Version d’amorçage 3.2.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-389">Bootstrap version 3.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-311"></a><span data-ttu-id="f2db6-390">Version d’amorçage 3.1.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-390">Bootstrap version 3.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-310"></a><span data-ttu-id="f2db6-391">Amorçage version 3.1.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-391">Bootstrap version 3.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-303"></a><span data-ttu-id="f2db6-392">Version d’amorçage 3.0.3</span><span class="sxs-lookup"><span data-stu-id="f2db6-392">Bootstrap version 3.0.3</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-302"></a><span data-ttu-id="f2db6-393">Version d’amorçage 3.0.2</span><span class="sxs-lookup"><span data-stu-id="f2db6-393">Bootstrap version 3.0.2</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-301"></a><span data-ttu-id="f2db6-394">Version d’amorçage 3.0.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-394">Bootstrap version 3.0.1</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-300"></a><span data-ttu-id="f2db6-395">Version d’amorçage 3.0.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-395">Bootstrap version 3.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-232"></a><span data-ttu-id="f2db6-396">Amorçage version 2.3.2</span><span class="sxs-lookup"><span data-stu-id="f2db6-396">Bootstrap version 2.3.2</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png

#### <a name="bootstrap-version-231"></a><span data-ttu-id="f2db6-397">Amorçage version 2.3.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-397">Bootstrap version 2.3.1</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="f2db6-398">Versions TouchCarousel d’amorçage sur le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-398">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="f2db6-399">Les versions suivantes de [ https://github.com/ixisio/bootstrap-touch-carousel ] (https://github.com/ixisio/bootstrap-touch-carousel " https://github.com/ixisio/bootstrap-touch-carousel ") TouchCarousel du programme d’amorçage versions sont hébergées sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="f2db6-399">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="f2db6-400">Amorçage TouchCarousel version 0.8.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-400">Bootstrap TouchCarousel version 0.8.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css
- http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="f2db6-401">Versions de Hammer.js sur le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-401">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="f2db6-402">Les versions suivantes de [ http://hammerjs.github.io/ ] (http://hammerjs.github.io/ " http://hammerjs.github.io/ ") Hammer.js versions sont hébergées sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="f2db6-402">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="f2db6-403">Hammer.js version 2.0.4</span><span class="sxs-lookup"><span data-stu-id="f2db6-403">Hammer.js version 2.0.4</span></span>

- http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js
- http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js
- http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="f2db6-404">Web Forms ASP.NET et Ajax mises en production sur le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-404">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="f2db6-405">Les versions suivantes de la bibliothèque ASP.NET Ajax sont hébergées sur le CDN.</span><span class="sxs-lookup"><span data-stu-id="f2db6-405">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="f2db6-406">Cliquez sur chaque lien pour afficher la liste réelle des fichiers.</span><span class="sxs-lookup"><span data-stu-id="f2db6-406">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="f2db6-407">Version de Web Forms ASP.NET et Ajax 4.5.2</span><span class="sxs-lookup"><span data-stu-id="f2db6-407">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "Web Forms ASP.NET et Ajax 4.5.2")
- [<span data-ttu-id="f2db6-408">Version de Web Forms ASP.NET et Ajax 4</span><span class="sxs-lookup"><span data-stu-id="f2db6-408">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "Web Forms ASP.NET et Ajax 4")
- [<span data-ttu-id="f2db6-409">ASP.NET Ajax version 3.5</span><span class="sxs-lookup"><span data-stu-id="f2db6-409">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "ASP.NET Ajax 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="f2db6-410">ASP.NET MVC publie le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-410">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="f2db6-411">Les fichiers ASP.NET MVC JavaScript suivants sont hébergés sur ce CDN :</span><span class="sxs-lookup"><span data-stu-id="f2db6-411">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="f2db6-412">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="f2db6-412">ASP.NET MVC 5.2.3</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="f2db6-413">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-413">ASP.NET MVC 5.1</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="f2db6-414">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-414">ASP.NET MVC 5.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="f2db6-415">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-415">ASP.NET MVC 4.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="f2db6-416">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-416">ASP.NET MVC 3.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="f2db6-417">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-417">ASP.NET MVC 2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js
- http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="f2db6-418">ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-418">ASP.NET MVC 1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js
- http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="f2db6-419">ASP.NET SignalR publie le CDN</span><span class="sxs-lookup"><span data-stu-id="f2db6-419">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="f2db6-420">Les fichiers ASP.NET SignalR JavaScript suivants sont hébergés sur ce CDN :</span><span class="sxs-lookup"><span data-stu-id="f2db6-420">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="f2db6-421">ASP.NET SignalR 2.2.2</span><span class="sxs-lookup"><span data-stu-id="f2db6-421">ASP.NET SignalR 2.2.2</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="f2db6-422">ASP.NET SignalR 2.2.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-422">ASP.NET SignalR 2.2.1</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="f2db6-423">ASP.NET SignalR 2.2.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-423">ASP.NET SignalR 2.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="f2db6-424">ASP.NET SignalR 2.1.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-424">ASP.NET SignalR 2.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="f2db6-425">ASP.NET SignalR 2.0.3</span><span class="sxs-lookup"><span data-stu-id="f2db6-425">ASP.NET SignalR 2.0.3</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="f2db6-426">ASP.NET SignalR 2.0.2</span><span class="sxs-lookup"><span data-stu-id="f2db6-426">ASP.NET SignalR 2.0.2</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="f2db6-427">ASP.NET SignalR 2.0.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-427">ASP.NET SignalR 2.0.1</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="f2db6-428">ASP.NET SignalR 2.0.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-428">ASP.NET SignalR 2.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="f2db6-429">ASP.NET SignalR 1.1.3</span><span class="sxs-lookup"><span data-stu-id="f2db6-429">ASP.NET SignalR 1.1.3</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="f2db6-430">ASP.NET SignalR 1.1.2</span><span class="sxs-lookup"><span data-stu-id="f2db6-430">ASP.NET SignalR 1.1.2</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="f2db6-431">ASP.NET SignalR 1.1.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-431">ASP.NET SignalR 1.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="f2db6-432">ASP.NET SignalR 1.1.0</span><span class="sxs-lookup"><span data-stu-id="f2db6-432">ASP.NET SignalR 1.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="f2db6-433">ASP.NET SignalR 1.0.1</span><span class="sxs-lookup"><span data-stu-id="f2db6-433">ASP.NET SignalR 1.0.1</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js

<span data-ttu-id="f2db6-434">Pour plus d’informations sur les conditions d’utilisation pour le CDN, consultez [Microsoft Ajax CDN conditions d’utilisation](https://www.asp.net/terms-of-use "Microsoft Ajax CDN conditions d’utilisation").</span><span class="sxs-lookup"><span data-stu-id="f2db6-434">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
