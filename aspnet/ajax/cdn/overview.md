---
uid: ajax/cdn/overview
title: Réseau de diffusion de contenu Microsoft Ajax | Documents Microsoft
author: rick-anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/14/2017
ms.topic: article
ms.assetid: 8935bf14-ca6d-4a4e-9dbe-b96ce74cef49
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /ajax/cdn
msc.type: content
ms.openlocfilehash: bc5f40746ad6b1ed8a74bcb75def9ff8f08fb789
ms.sourcegitcommit: 7d02ca5f5ddc2ca3eb0258fdd6996fbf538c129a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/03/2018
---
<a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="0fa1b-102">Réseau de diffusion de contenu Microsoft Ajax</span><span class="sxs-lookup"><span data-stu-id="0fa1b-102">Microsoft Ajax Content Delivery Network</span></span>
====================
> [!WARNING]
> <span data-ttu-id="0fa1b-103">Applications de production doivent prendront une dépendance dure sur les ressources CDN.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-103">Production applications should not take a hard dependency on CDN assets.</span></span> <span data-ttu-id="0fa1b-104">Applications doivent vérifier la présence de l’élément multimédia CDN référencé et utiliser une ressource de secours lorsque le CDN n’est pas disponible.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-104">Applications should test for the CDN asset referenced, and use a fallback asset when the CDN is not available.</span></span> 
>
> <span data-ttu-id="0fa1b-105">Le CDN Microsoft Ajax n’a pas de contrat SLA au-dessus et au-delà de l’aide d’un CDN Azure.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-105">The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>
>
> <span data-ttu-id="0fa1b-106">Utilisez [ce problème GitHub](https://github.com/aspnet/Docs/issues/5832) pour signaler des problèmes avec le CDN Microsoft Ajax.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-106">Use [this GitHub issue](https://github.com/aspnet/Docs/issues/5832) to report problems with the Microsoft Ajax CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="0fa1b-107">Sommaire</span><span class="sxs-lookup"><span data-stu-id="0fa1b-107">Table of Contents</span></span>

<span data-ttu-id="0fa1b-108">**[AJAX.Microsoft.com renommé ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="0fa1b-108">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="0fa1b-109">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="0fa1b-109">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="0fa1b-110">**[À l’aide d’ASP.NET Ajax du CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="0fa1b-110">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="0fa1b-111">**[À l’aide de jQuery du CDN](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="0fa1b-111">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="0fa1b-112">**[À l’aide de jQuery UI du CDN](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="0fa1b-112">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="0fa1b-113">**[Fichiers de tiers dans le CDN](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="0fa1b-113">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="0fa1b-114">Versions de jQuery sur le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-114">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="0fa1b-115">jQuery migrer des versions sur le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-115">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="0fa1b-116">jQuery UI mises en production sur le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-116">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="0fa1b-117">jQuery Validation mises en production sur le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-117">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="0fa1b-118">jQuery Mobile mises en production sur le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-118">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="0fa1b-119">jQuery versions de modèles sur le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-119">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="0fa1b-120">jQuery Cycle des versions sur le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-120">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="0fa1b-121">jQuery versions de tables de données sur le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-121">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="0fa1b-122">Versions de Modernizr sur le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-122">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="0fa1b-123">Versions de JSHint sur le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-123">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="0fa1b-124">Versions de Knockout sur le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-124">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="0fa1b-125">Globaliser des versions sur le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-125">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="0fa1b-126">Répondre versions sur le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-126">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="0fa1b-127">Versions d’amorçage sur le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-127">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="0fa1b-128">Versions TouchCarousel d’amorçage sur le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-128">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="0fa1b-129">Versions de Hammer.js sur le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-129">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="0fa1b-130">Web Forms ASP.NET et Ajax mises en production sur le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-130">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="0fa1b-131">ASP.NET MVC publie le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-131">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="0fa1b-132">ASP.NET SignalR publie le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-132">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="0fa1b-133">Le contenu remise réseau CDN Microsoft Ajax () héberge des bibliothèques JavaScript de tiers populaires tels que jQuery et vous permet de facilement les ajouter à vos applications Web.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-133">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="0fa1b-134">Par exemple, commencer à l’aide de jQuery, qui est hébergé sur ce CDN en ajoutant simplement un &lt;script&gt; balise à votre page qui pointe vers ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-134">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="0fa1b-135">En tirant parti du CDN, vous pouvez considérablement améliorer les performances de vos applications Ajax.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-135">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="0fa1b-136">Le contenu du CDN est mis en cache sur les serveurs situés dans le monde entier.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-136">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="0fa1b-137">En outre, le CDN permet aux navigateurs de réutiliser des fichiers JavaScript mis en cache tiers pour les sites web qui se trouvent dans des domaines différents.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-137">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="0fa1b-138">Le CDN prend en charge SSL (HTTPS) au cas où vous deviez traiter une page web à l’aide de Secure Sockets Layer.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-138">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="0fa1b-139">Le CDN héberge les bibliothèques de scripts tiers suivants qui ont été téléchargés et sont concédés sous licence, par les propriétaires de ces bibliothèques :</span><span class="sxs-lookup"><span data-stu-id="0fa1b-139">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="0fa1b-140">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="0fa1b-140">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="0fa1b-141">jQuery UI (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="0fa1b-141">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="0fa1b-142">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="0fa1b-142">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="0fa1b-143">jQuery Validation (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="0fa1b-143">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="0fa1b-144">jQuery Cycle (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="0fa1b-144">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="0fa1b-145">jQuery DataTables)http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="0fa1b-145">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="0fa1b-146">Le CDN Microsoft Ajax inclut également les bibliothèques suivantes qui ont été téléchargés par Microsoft :</span><span class="sxs-lookup"><span data-stu-id="0fa1b-146">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="0fa1b-147">ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="0fa1b-147">ASP.NET Ajax</span></span>
- <span data-ttu-id="0fa1b-148">Fichiers JavaScript ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="0fa1b-148">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="0fa1b-149">Les fichiers ASP.NET SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="0fa1b-149">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="0fa1b-150">Microsoft ne revendique pas la propriété de toutes les bibliothèques tierces hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-150">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="0fa1b-151">Les propriétaires de copyright des bibliothèques de licence ces bibliothèques pour vous.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-151">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="0fa1b-152">Les droits que vous devrez peut-être télécharger et utiliser ces bibliothèques sont accordées exclusivement par les propriétaires de copyright respectifs.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-152">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="0fa1b-153">Étant donné que celles-ci ne sont pas des bibliothèques de Microsoft, Microsoft ne fournit aucune garantie ou les licences de droits de propriété intellectuelle (y compris sans droits de brevet implicites) pour les bibliothèques tierces hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-153">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="0fa1b-154">Si vous souhaitez soumettre votre bibliothèque JavaScript et votre bibliothèque est une des bibliothèques JavaScript supérieur (tel qu’indiqué dans http://trends.builtwith.com) ou extensions/plug-ins pour ces bibliothèques qui sont (a) populaires ; ou (b) utile pour une utilisation sur ASP.NET, puis contactez AjaxCDNSubmission@Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-154">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="0fa1b-155">AJAX.Microsoft.com renommé ajax.aspnetcdn.com</span><span class="sxs-lookup"><span data-stu-id="0fa1b-155">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="0fa1b-156">Le CDN permet d’utiliser le nom de domaine microsoft.com et a été modifié pour utiliser le nom de domaine aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-156">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="0fa1b-157">Cette modification a été apportée pour augmenter les performances, car lorsqu’un navigateur référencé le domaine microsoft.com il envoie tous les cookies à partir de ce domaine sur le réseau avec chaque demande.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-157">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="0fa1b-158">En renommant à un nom de domaine différent de microsoft.com performances peuvent être augmenté par autant de 25 %.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-158">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="0fa1b-159">Remarque ajax.microsoft.com continuera de fonctionner mais ajax.aspnetcdn.com est recommandé.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-159">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="0fa1b-160">Ancien Format : https://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="0fa1b-160">Old Format: https://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="0fa1b-161">Nouveau Format : https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="0fa1b-161">New Format: https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="0fa1b-162">Prise en charge de Visual Studio .vsdoc</span><span class="sxs-lookup"><span data-stu-id="0fa1b-162">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="0fa1b-163">Pour utiliser les fichiers .vsdoc correctement avec Visual Studio 2008, vous devez vous assurer que vous disposez de Visual Studio 2008 SP1 installé et le correctif logiciel pour les fichiers vsdoc installé.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-163">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="0fa1b-164">Vous pouvez obtenir à partir d’ici :</span><span class="sxs-lookup"><span data-stu-id="0fa1b-164">You can get these from here:</span></span>

- [<span data-ttu-id="0fa1b-165">Télécharger Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-165">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "télécharger Visual Studio 2008 SP1")
- [<span data-ttu-id="0fa1b-166">Télécharger le correctif logiciel de .vsdoc pour Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-166">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 "télécharger .vsdoc correctif pour Visual Studio 2008 SP1")

<span data-ttu-id="0fa1b-167">Visual Studio 2010 prend en charge les fichiers .vsdoc sans les correctifs supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-167">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="0fa1b-168">À l’aide d’ASP.NET Ajax du CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-168">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="0fa1b-169">Lorsque vous utilisez ASP.NET 4, vous pouvez rediriger toutes les demandes pour les scripts du framework ASP.NET vers le CDN.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-169">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="0fa1b-170">La récupération des scripts à partir du CDN au lieu de votre serveur web local peuvent considérablement améliorer les performances des sites Web ASP.NET publics.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-170">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="0fa1b-171">Utilisez la propriété ScriptManager EnableCDN pour rediriger toutes les demandes de script framework ASP.NET pour le CDN Microsoft Ajax :</span><span class="sxs-lookup"><span data-stu-id="0fa1b-171">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="0fa1b-172">À l’aide de jQuery du CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-172">Using jQuery from the CDN</span></span>

<span data-ttu-id="0fa1b-173">Vous pouvez utiliser des scripts jQuery hébergés sur CDN dans votre application Web en ajoutant l’élément de script suivant à une page :</span><span class="sxs-lookup"><span data-stu-id="0fa1b-173">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="0fa1b-174">Le CDN inclut également la version réduite du script jQuery, que vous pouvez obtenir à l’aide de l’élément suivant :</span><span class="sxs-lookup"><span data-stu-id="0fa1b-174">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="0fa1b-175">Pour permettre à votre page à revenir au chargement jQuery à partir d’un chemin d’accès local sur votre propre site Web si le CDN est indisponible, ajoutez l’élément suivant immédiatement après l’élément référençant le CDN :</span><span class="sxs-lookup"><span data-stu-id="0fa1b-175">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="0fa1b-176">L’exemple de page suivant utilise la version CDN de la bibliothèque jQuery (avec basculement vers une copie locale) pour afficher le contenu d’un élément div lorsqu’un clic est effectué.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-176">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="0fa1b-177">Vous pouvez en savoir plus sur jQuery et télécharger une copie locale de jQuery en vous rendant sur le [jQuery](http://jquery.com/) site Web.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-177">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="0fa1b-178">À l’aide de jQuery UI du CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-178">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="0fa1b-179">Le CDN héberge également la bibliothèque jQuery UI.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-179">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="0fa1b-180">La bibliothèque jQuery UI comprend un ensemble complet de widgets et les effets que vous pouvez utiliser dans vos applications ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-180">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="0fa1b-181">Par exemple, la page suivante illustre comment vous pouvez utiliser la sélecteur de dates de l’interface utilisateur jQuery dans le contexte d’une application Web Forms ASP.NET pour afficher un calendrier contextuel :</span><span class="sxs-lookup"><span data-stu-id="0fa1b-181">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="0fa1b-182">Lorsque vous déplacez le focus vers la zone de texte à l’aide de votre clavier, un calendrier s’affiche :</span><span class="sxs-lookup"><span data-stu-id="0fa1b-182">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Calendrier contextuel créé avec le sélecteur de dates](overview/_static/image1.png)

<span data-ttu-id="0fa1b-184">Notez que vous devez inclure les trois fichiers du CDN dans le code ci-dessus :</span><span class="sxs-lookup"><span data-stu-id="0fa1b-184">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="0fa1b-185">La bibliothèque jQuery &mdash; la bibliothèque jQuery UI dépend de la bibliothèque jQuery.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-185">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="0fa1b-186">Vous devez ajouter la bibliothèque jQuery à votre page avant d’ajouter la bibliothèque jQuery UI.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-186">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="0fa1b-187">La bibliothèque jQuery UI &mdash; la bibliothèque jQuery UI contient tous les effets d’interface utilisateur jQuery et widgets tels que le widget de sélecteur de dates utilisées dans la page ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-187">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="0fa1b-188">Un thème de l’interface utilisateur jQuery &mdash; jQuery UI prend en charge différents thèmes.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-188">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="0fa1b-189">La page ci-dessus inclut un lien vers un fichier CSS pour importer le thème de Redmond.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-189">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="0fa1b-190">Tous les thèmes de l’interface utilisateur jQuery standard sont hébergés sur le CDN.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-190">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="0fa1b-191">[Visitez cette page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 dans le CDN Microsoft Ajax") pour afficher les miniatures pour chaque thème.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-191">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="0fa1b-192">Pour plus d’informations sur la bibliothèque jQuery UI, visitez officielle [site Web de l’interface utilisateur jQuery](http://jQueryUI.com "site Web de l’interface utilisateur jQuery").</span><span class="sxs-lookup"><span data-stu-id="0fa1b-192">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="0fa1b-193">Fichiers de tiers dans le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-193">Third-Party Files on the CDN</span></span>

<span data-ttu-id="0fa1b-194">Le CDN héberge certains des bibliothèques JavaScript tiers les plus populaires.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-194">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="0fa1b-195">Microsoft ne revendique pas la propriété de toutes les bibliothèques tierces hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-195">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="0fa1b-196">Les propriétaires de copyright des bibliothèques de licence ces bibliothèques pour vous.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-196">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="0fa1b-197">Les droits que vous devrez peut-être télécharger et utiliser ces bibliothèques sont accordées exclusivement par les propriétaires de copyright respectifs.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-197">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="0fa1b-198">Étant donné que celles-ci ne sont pas des bibliothèques de Microsoft, Microsoft ne fournit aucune garantie ou les licences de droits de propriété intellectuelle (y compris sans droits de brevet implicites) pour les bibliothèques tierces hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-198">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="0fa1b-199">Versions de jQuery sur le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-199">jQuery Releases on the CDN</span></span>

<span data-ttu-id="0fa1b-200">Les versions suivantes de jQuery sont hébergées sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="0fa1b-200">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-331"></a><span data-ttu-id="0fa1b-201">version de jQuery 3.3.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-201">jQuery version 3.3.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map

#### <a name="jquery-version-321"></a><span data-ttu-id="0fa1b-202">jQuery version 3.2.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-202">jQuery version 3.2.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map

#### <a name="jquery-version-320"></a><span data-ttu-id="0fa1b-203">version de jQuery 3.2.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-203">jQuery version 3.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map

#### <a name="jquery-version-311"></a><span data-ttu-id="0fa1b-204">version de jQuery 3.1.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-204">jQuery version 3.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map

#### <a name="jquery-version-310"></a><span data-ttu-id="0fa1b-205">jQuery version 3.1.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-205">jQuery version 3.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map

#### <a name="jquery-version-300"></a><span data-ttu-id="0fa1b-206">version de jQuery 3.0.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-206">jQuery version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map

#### <a name="jquery-version-224"></a><span data-ttu-id="0fa1b-207">version de jQuery 2.2.4</span><span class="sxs-lookup"><span data-stu-id="0fa1b-207">jQuery version 2.2.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map

#### <a name="jquery-version-223"></a><span data-ttu-id="0fa1b-208">version de jQuery 2.2.3</span><span class="sxs-lookup"><span data-stu-id="0fa1b-208">jQuery version 2.2.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map

#### <a name="jquery-version-222"></a><span data-ttu-id="0fa1b-209">version de jQuery 2.2.2</span><span class="sxs-lookup"><span data-stu-id="0fa1b-209">jQuery version 2.2.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map

#### <a name="jquery-version-221"></a><span data-ttu-id="0fa1b-210">jQuery version 2.2.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-210">jQuery version 2.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map

#### <a name="jquery-version-220"></a><span data-ttu-id="0fa1b-211">jQuery version2.2.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-211">jQuery version 2.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map

#### <a name="jquery-version-214"></a><span data-ttu-id="0fa1b-212">version de jQuery 2.1.4</span><span class="sxs-lookup"><span data-stu-id="0fa1b-212">jQuery version 2.1.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map

#### <a name="jquery-version-213"></a><span data-ttu-id="0fa1b-213">version de jQuery 2.1.3</span><span class="sxs-lookup"><span data-stu-id="0fa1b-213">jQuery version 2.1.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map

#### <a name="jquery-version-212"></a><span data-ttu-id="0fa1b-214">version de jQuery 2.1.2</span><span class="sxs-lookup"><span data-stu-id="0fa1b-214">jQuery version 2.1.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js

#### <a name="jquery-version-211"></a><span data-ttu-id="0fa1b-215">version de jQuery 2.1.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-215">jQuery version 2.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map

#### <a name="jquery-version-210"></a><span data-ttu-id="0fa1b-216">version de jQuery 2.1.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-216">jQuery version 2.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map

#### <a name="jquery-version-203"></a><span data-ttu-id="0fa1b-217">version de jQuery 2.0.3</span><span class="sxs-lookup"><span data-stu-id="0fa1b-217">jQuery version 2.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map

#### <a name="jquery-version-202"></a><span data-ttu-id="0fa1b-218">version de jQuery 2.0.2</span><span class="sxs-lookup"><span data-stu-id="0fa1b-218">jQuery version 2.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map

#### <a name="jquery-version-201"></a><span data-ttu-id="0fa1b-219">version de jQuery 2.0.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-219">jQuery version 2.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map

#### <a name="jquery-version-200"></a><span data-ttu-id="0fa1b-220">jQuery version 2.0.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-220">jQuery version 2.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map

#### <a name="jquery-version-1124"></a><span data-ttu-id="0fa1b-221">version de jQuery 1.12.4</span><span class="sxs-lookup"><span data-stu-id="0fa1b-221">jQuery version 1.12.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map

#### <a name="jquery-version-1123"></a><span data-ttu-id="0fa1b-222">version de jQuery 1.12.3</span><span class="sxs-lookup"><span data-stu-id="0fa1b-222">jQuery version 1.12.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map

#### <a name="jquery-version-1122"></a><span data-ttu-id="0fa1b-223">version de jQuery 1.12.2</span><span class="sxs-lookup"><span data-stu-id="0fa1b-223">jQuery version 1.12.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map

#### <a name="jquery-version-1121"></a><span data-ttu-id="0fa1b-224">version de jQuery 1.12.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-224">jQuery version 1.12.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map

#### <a name="jquery-version-1120"></a><span data-ttu-id="0fa1b-225">version de jQuery 1.12.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-225">jQuery version 1.12.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map

#### <a name="jquery-version-1113"></a><span data-ttu-id="0fa1b-226">version de jQuery 1.11.3</span><span class="sxs-lookup"><span data-stu-id="0fa1b-226">jQuery version 1.11.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map

#### <a name="jquery-version-1112"></a><span data-ttu-id="0fa1b-227">version de jQuery 1.11.2</span><span class="sxs-lookup"><span data-stu-id="0fa1b-227">jQuery version 1.11.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map

#### <a name="jquery-version-1111"></a><span data-ttu-id="0fa1b-228">version de jQuery 1.11.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-228">jQuery version 1.11.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map

#### <a name="jquery-version-1110"></a><span data-ttu-id="0fa1b-229">version de jQuery 1.11.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-229">jQuery version 1.11.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map

#### <a name="jquery-version-1102"></a><span data-ttu-id="0fa1b-230">version de jQuery 1.10.2</span><span class="sxs-lookup"><span data-stu-id="0fa1b-230">jQuery version 1.10.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map

#### <a name="jquery-version-1101"></a><span data-ttu-id="0fa1b-231">version de jQuery 1.10.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-231">jQuery version 1.10.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map

#### <a name="jquery-version-1100"></a><span data-ttu-id="0fa1b-232">version de jQuery 1.10.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-232">jQuery version 1.10.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map

#### <a name="jquery-version-191"></a><span data-ttu-id="0fa1b-233">version de jQuery 1.9.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-233">jQuery version 1.9.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map

#### <a name="jquery-version-190"></a><span data-ttu-id="0fa1b-234">version de jQuery à 1.9.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-234">jQuery version 1.9.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map

#### <a name="jquery-version-183"></a><span data-ttu-id="0fa1b-235">version de jQuery 1.8.3</span><span class="sxs-lookup"><span data-stu-id="0fa1b-235">jQuery version 1.8.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js

#### <a name="jquery-version-182"></a><span data-ttu-id="0fa1b-236">version de jQuery 1.8.2</span><span class="sxs-lookup"><span data-stu-id="0fa1b-236">jQuery version 1.8.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js

#### <a name="jquery-version-181"></a><span data-ttu-id="0fa1b-237">version de jQuery 1.8.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-237">jQuery version 1.8.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js

#### <a name="jquery-version-180"></a><span data-ttu-id="0fa1b-238">version de jQuery 1.8.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-238">jQuery version 1.8.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js

#### <a name="jquery-version-172"></a><span data-ttu-id="0fa1b-239">version de jQuery 1.7.2</span><span class="sxs-lookup"><span data-stu-id="0fa1b-239">jQuery version 1.7.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js

#### <a name="jquery-version-171"></a><span data-ttu-id="0fa1b-240">jQuery version 1.7.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-240">jQuery version 1.7.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js

#### <a name="jquery-version-17"></a><span data-ttu-id="0fa1b-241">jQuery version 1.7</span><span class="sxs-lookup"><span data-stu-id="0fa1b-241">jQuery version 1.7</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js

#### <a name="jquery-version-164"></a><span data-ttu-id="0fa1b-242">version de jQuery 1.6.4</span><span class="sxs-lookup"><span data-stu-id="0fa1b-242">jQuery version 1.6.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js

#### <a name="jquery-version-163"></a><span data-ttu-id="0fa1b-243">version de jQuery 1.6.3</span><span class="sxs-lookup"><span data-stu-id="0fa1b-243">jQuery version 1.6.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js

#### <a name="jquery-version-162"></a><span data-ttu-id="0fa1b-244">version de jQuery 1.6.2</span><span class="sxs-lookup"><span data-stu-id="0fa1b-244">jQuery version 1.6.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js

#### <a name="jquery-version-161"></a><span data-ttu-id="0fa1b-245">version de jQuery 1.6.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-245">jQuery version 1.6.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js

#### <a name="jquery-version-16"></a><span data-ttu-id="0fa1b-246">jQuery version 1.6</span><span class="sxs-lookup"><span data-stu-id="0fa1b-246">jQuery version 1.6</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js

#### <a name="jquery-version-152"></a><span data-ttu-id="0fa1b-247">version de jQuery 1.5.2</span><span class="sxs-lookup"><span data-stu-id="0fa1b-247">jQuery version 1.5.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js

#### <a name="jquery-version-151"></a><span data-ttu-id="0fa1b-248">version de jQuery 1.5.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-248">jQuery version 1.5.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js

#### <a name="jquery-version-15"></a><span data-ttu-id="0fa1b-249">version de jQuery 1.5</span><span class="sxs-lookup"><span data-stu-id="0fa1b-249">jQuery version 1.5</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js

#### <a name="jquery-version-144"></a><span data-ttu-id="0fa1b-250">version de jQuery 1.4.4</span><span class="sxs-lookup"><span data-stu-id="0fa1b-250">jQuery version 1.4.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js

#### <a name="jquery-version-143"></a><span data-ttu-id="0fa1b-251">version de jQuery 1.4.3</span><span class="sxs-lookup"><span data-stu-id="0fa1b-251">jQuery version 1.4.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js

#### <a name="jquery-version-142"></a><span data-ttu-id="0fa1b-252">version de jQuery 1.4.2</span><span class="sxs-lookup"><span data-stu-id="0fa1b-252">jQuery version 1.4.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js

#### <a name="jquery-version-141"></a><span data-ttu-id="0fa1b-253">version de jQuery 1.4.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-253">jQuery version 1.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js

#### <a name="jquery-version-14"></a><span data-ttu-id="0fa1b-254">jQuery version 1.4</span><span class="sxs-lookup"><span data-stu-id="0fa1b-254">jQuery version 1.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js

#### <a name="jquery-version-132"></a><span data-ttu-id="0fa1b-255">version de jQuery 1.3.2</span><span class="sxs-lookup"><span data-stu-id="0fa1b-255">jQuery version 1.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="0fa1b-256">jQuery migrer des versions sur le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-256">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="0fa1b-257">Les versions suivantes de jQuery migrer sont hébergées sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="0fa1b-257">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="0fa1b-258">jQuery migration version 3.0.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-258">jQuery Migrate version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="0fa1b-259">jQuery migration version 1.2.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-259">jQuery Migrate version 1.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js

<span data-ttu-id="0fa1b-260">jQuery migration version 1.2.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-260">jQuery Migrate version 1.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="0fa1b-261">jQuery migration version 1.1.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-261">jQuery Migrate version 1.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="0fa1b-262">jQuery migration version 1.1.0.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-262">jQuery Migrate version 1.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="0fa1b-263">Migrer la version 1.0.0 de jQuery</span><span class="sxs-lookup"><span data-stu-id="0fa1b-263">jQuery Migrate version 1.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="0fa1b-264">jQuery UI mises en production sur le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-264">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="0fa1b-265">Les versions suivantes de la bibliothèque jQuery UI sont hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-265">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="0fa1b-266">Cliquez sur chaque lien pour afficher la liste réelle des fichiers.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-266">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="0fa1b-267">jQuery UI 1.12.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-267">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery UI 1.12.1 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-268">jQuery UI 1.12.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-268">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "jQuery UI 1.12.0 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-269">jQuery UI 1.11.4</span><span class="sxs-lookup"><span data-stu-id="0fa1b-269">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "jQuery UI 1.11.4 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-270">jQuery UI 1.11.3</span><span class="sxs-lookup"><span data-stu-id="0fa1b-270">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery UI 1.11.3 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-271">jQuery UI 1.11.2</span><span class="sxs-lookup"><span data-stu-id="0fa1b-271">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery UI 1.11.2 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-272">jQuery UI 1.11.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-272">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "jQuery UI 1.11.1 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-273">jQuery UI 1.11.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-273">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "jQuery UI 1.11.0 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-274">jQuery UI 1.10.4</span><span class="sxs-lookup"><span data-stu-id="0fa1b-274">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery UI 1.10.4 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-275">jQuery UI 1.10.3</span><span class="sxs-lookup"><span data-stu-id="0fa1b-275">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery UI 1.10.3 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-276">jQuery UI 1.10.2</span><span class="sxs-lookup"><span data-stu-id="0fa1b-276">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery UI 1.10.2 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-277">jQuery UI 1.10.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-277">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery UI 1.10.1 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-278">jQuery UI 1.10.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-278">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery UI 1.10.0 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-279">jQuery UI 1.9.2</span><span class="sxs-lookup"><span data-stu-id="0fa1b-279">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "jQuery UI 1.9.2 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-280">jQuery UI 1.9.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-280">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "jQuery UI 1.9.1 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-281">jQuery UI à 1.9.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-281">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "jQuery UI à 1.9.0 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-282">jQuery UI 1.8.24</span><span class="sxs-lookup"><span data-stu-id="0fa1b-282">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery UI 1.8.24 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-283">jQuery UI 1.8.23</span><span class="sxs-lookup"><span data-stu-id="0fa1b-283">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery UI 1.8.23 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-284">jQuery UI 1.8.22</span><span class="sxs-lookup"><span data-stu-id="0fa1b-284">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery UI 1.8.22 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-285">jQuery UI 1.8.21</span><span class="sxs-lookup"><span data-stu-id="0fa1b-285">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery UI 1.8.21 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-286">jQuery UI 1.8.20</span><span class="sxs-lookup"><span data-stu-id="0fa1b-286">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery UI 1.8.20 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-287">jQuery UI 1.8.19</span><span class="sxs-lookup"><span data-stu-id="0fa1b-287">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery UI 1.8.19 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-288">jQuery UI 1.8.18</span><span class="sxs-lookup"><span data-stu-id="0fa1b-288">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery UI 1.8.18 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-289">jQuery UI 1.8.17</span><span class="sxs-lookup"><span data-stu-id="0fa1b-289">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery UI 1.8.17 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-290">jQuery UI 1.8.16</span><span class="sxs-lookup"><span data-stu-id="0fa1b-290">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery UI 1.8.16 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-291">jQuery UI 1.8.15</span><span class="sxs-lookup"><span data-stu-id="0fa1b-291">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery UI 1.8.15 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-292">jQuery UI 1.8.14</span><span class="sxs-lookup"><span data-stu-id="0fa1b-292">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery UI 1.8.14 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-293">jQuery UI 1.8.13</span><span class="sxs-lookup"><span data-stu-id="0fa1b-293">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery UI 1.8.13 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-294">jQuery UI 1.8.12</span><span class="sxs-lookup"><span data-stu-id="0fa1b-294">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery UI 1.8.12 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-295">jQuery UI 1.8.11</span><span class="sxs-lookup"><span data-stu-id="0fa1b-295">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery UI 1.8.11 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-296">jQuery UI 1.8.10</span><span class="sxs-lookup"><span data-stu-id="0fa1b-296">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-297">jQuery UI 1.8.9</span><span class="sxs-lookup"><span data-stu-id="0fa1b-297">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "jQuery UI 1.8.9 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-298">jQuery UI 1.8.8</span><span class="sxs-lookup"><span data-stu-id="0fa1b-298">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "jQuery UI 1.8.8 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-299">jQuery UI 1.8.7</span><span class="sxs-lookup"><span data-stu-id="0fa1b-299">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery UI 1.8.7 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-300">jQuery UI 1.8.6</span><span class="sxs-lookup"><span data-stu-id="0fa1b-300">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery UI 1.8.6 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-301">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="0fa1b-301">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="0fa1b-302">jQuery Validation mises en production sur le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-302">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="0fa1b-303">Les versions suivantes de la bibliothèque de Validation jQuery sont hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-303">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="0fa1b-304">Cliquez sur chaque lien pour afficher la liste réelle des fichiers.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-304">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="0fa1b-305">jQuery validation 1.17.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-305">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "jQuery Validation 1.17.0")
- [<span data-ttu-id="0fa1b-306">jQuery validation 1.16.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-306">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "jQuery Validation 1.16.0")
- [<span data-ttu-id="0fa1b-307">jQuery validation 1.15.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-307">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "jQuery Validation 1.15.1")
- [<span data-ttu-id="0fa1b-308">jQuery validation 1.15.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-308">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "jQuery Validation 1.15.0")
- [<span data-ttu-id="0fa1b-309">jQuery validation 1.14.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-309">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "jQuery Validation 1.14.0")
- [<span data-ttu-id="0fa1b-310">jQuery validation 1.13.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-310">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "jQuery Validation 1.13.1")
- [<span data-ttu-id="0fa1b-311">jQuery validation 1.13.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-311">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "jQuery Validation 1.13.0")
- [<span data-ttu-id="0fa1b-312">jQuery validation 1.12.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-312">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "jQuery Validation 1.12.0")
- [<span data-ttu-id="0fa1b-313">jQuery validation 1.11.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-313">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "jQuery Validation 1.11.1")
- [<span data-ttu-id="0fa1b-314">jQuery validation 1.11.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-314">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "jQuery Validation 1.11.0")
- [<span data-ttu-id="0fa1b-315">jQuery validation 1.10.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-315">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "jQuery Validation 1.10.0")
- [<span data-ttu-id="0fa1b-316">jQuery validation 1.9</span><span class="sxs-lookup"><span data-stu-id="0fa1b-316">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate version 1.9")
- [<span data-ttu-id="0fa1b-317">jQuery validation 1.8.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-317">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jquery.validate version 1.8.1")
- [<span data-ttu-id="0fa1b-318">jQuery validation 1.8</span><span class="sxs-lookup"><span data-stu-id="0fa1b-318">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate version 1.8")
- [<span data-ttu-id="0fa1b-319">jQuery validation 1.7</span><span class="sxs-lookup"><span data-stu-id="0fa1b-319">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate version 1.7")
- [<span data-ttu-id="0fa1b-320">jQuery validation 1.6</span><span class="sxs-lookup"><span data-stu-id="0fa1b-320">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery validation 1.6")
- [<span data-ttu-id="0fa1b-321">jQuery validation 1.5.5</span><span class="sxs-lookup"><span data-stu-id="0fa1b-321">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "jQuery validation 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="0fa1b-322">jQuery Mobile mises en production sur le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-322">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="0fa1b-323">Les versions suivantes de la bibliothèque de jQuery Mobile sont hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-323">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="0fa1b-324">Cliquez sur chaque lien pour afficher la liste réelle des fichiers.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-324">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="0fa1b-325">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="0fa1b-325">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-326">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="0fa1b-326">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-327">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-327">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-328">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-328">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-329">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="0fa1b-329">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-330">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-330">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-331">jQuery Mobile version 1.3.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-331">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile version 1.3.0 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-332">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-332">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-333">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="0fa1b-333">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-334">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-334">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-335">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-335">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-336">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="0fa1b-336">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-337">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-337">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-338">jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-338">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-339">jQuery Mobile 1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="0fa1b-339">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-340">jQuery Mobile 1.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-340">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="0fa1b-341">version bêta 3 de jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-341">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 bêta 3 sur le CDN Microsoft Ajax")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="0fa1b-342">jQuery versions de modèles sur le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-342">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="0fa1b-343">Les versions suivantes du plug-in de modèles jQuery sont hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-343">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="0fa1b-344">Cliquez sur chaque lien pour afficher la liste réelle des fichiers.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-344">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="0fa1b-345">jQuery modèles bêta 1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-345">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery modèles bêta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="0fa1b-346">jQuery Cycle des versions sur le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-346">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="0fa1b-347">Les versions suivantes du plug-in du Cycle de jQuery sont hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-347">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="0fa1b-348">Cliquez sur chaque lien pour afficher la liste réelle des fichiers.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-348">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="0fa1b-349">jQuery Cycle 2,99</span><span class="sxs-lookup"><span data-stu-id="0fa1b-349">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery 2,99 de Cycle")
- [<span data-ttu-id="0fa1b-350">jQuery Cycle 2.94</span><span class="sxs-lookup"><span data-stu-id="0fa1b-350">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery Cycle 2.94")
- [<span data-ttu-id="0fa1b-351">jQuery Cycle 2,88</span><span class="sxs-lookup"><span data-stu-id="0fa1b-351">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery 2,88 de Cycle")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="0fa1b-352">jQuery versions de tables de données sur le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-352">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="0fa1b-353">Les versions suivantes du plug-in DataTables de jQuery sont hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-353">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="0fa1b-354">Cliquez sur chaque lien pour afficher la liste réelle des fichiers.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-354">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="0fa1b-355">jQuery DataTables 1.10.5</span><span class="sxs-lookup"><span data-stu-id="0fa1b-355">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [<span data-ttu-id="0fa1b-356">jQuery DataTables 1.10.4</span><span class="sxs-lookup"><span data-stu-id="0fa1b-356">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [<span data-ttu-id="0fa1b-357">jQuery DataTables 1.9.4</span><span class="sxs-lookup"><span data-stu-id="0fa1b-357">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables 1.9.4")
- [<span data-ttu-id="0fa1b-358">jQuery DataTables 1.9.3</span><span class="sxs-lookup"><span data-stu-id="0fa1b-358">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [<span data-ttu-id="0fa1b-359">jQuery DataTables 1.9.2</span><span class="sxs-lookup"><span data-stu-id="0fa1b-359">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [<span data-ttu-id="0fa1b-360">jQuery DataTables 1.9.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-360">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [<span data-ttu-id="0fa1b-361">jQuery DataTables à 1.9.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-361">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables à 1.9.0")
- [<span data-ttu-id="0fa1b-362">jQuery DataTables 1.8.2</span><span class="sxs-lookup"><span data-stu-id="0fa1b-362">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="0fa1b-363">Versions de Modernizr sur le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-363">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="0fa1b-364">Les versions suivantes de [Modernizr](http://www.modernizr.com "Modernizr") sont hébergés sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="0fa1b-364">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="0fa1b-365">Versions de JSHint sur le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-365">JSHint Releases on the CDN</span></span>

<span data-ttu-id="0fa1b-366">Les versions suivantes de [JSHint](http://www.jshint.com "JSHint") sont hébergés sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="0fa1b-366">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="0fa1b-367">Versions de Knockout sur le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-367">Knockout Releases on the CDN</span></span>

<span data-ttu-id="0fa1b-368">Les versions suivantes de [Knockout](http://www.knockoutjs.com "Knockout") sont hébergés sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="0fa1b-368">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="0fa1b-369">Globaliser des versions sur le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-369">Globalize Releases on the CDN</span></span>

<span data-ttu-id="0fa1b-370">Les versions suivantes de [Globalize](https://github.com/jquery/globalize "Globalize") sont hébergés sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="0fa1b-370">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="0fa1b-371">La version 1.0.0 de globalisation</span><span class="sxs-lookup"><span data-stu-id="0fa1b-371">Globalize version 1.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js

#### <a name="globalize-version-011"></a><span data-ttu-id="0fa1b-372">Globaliser version 0.1.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-372">Globalize version 0.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js

    - <span data-ttu-id="0fa1b-373">toutes les cultures</span><span class="sxs-lookup"><span data-stu-id="0fa1b-373">all cultures</span></span>
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js

    - <span data-ttu-id="0fa1b-374">Remplacez « {-code de culture} » avec le code de culture de votre choix, par exemple, les fichiers sur le CDN globalize.culture.en-GB.js== Microsoft == ces bibliothèques ont été téléchargés par Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-374">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="0fa1b-375">Répondre versions sur le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-375">Respond Releases on the CDN</span></span>

<span data-ttu-id="0fa1b-376">Les versions suivantes de [ https://github.com/scottjehl/Respond ] (https://github.com/scottjehl/Respond " https://github.com/scottjehl/Respond ") répondre sont hébergés sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="0fa1b-376">The following releases of [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") Respond are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="0fa1b-377">Répondre version 1.4.2</span><span class="sxs-lookup"><span data-stu-id="0fa1b-377">Respond version 1.4.2</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js

#### <a name="respond-version-141"></a><span data-ttu-id="0fa1b-378">Version 1.4.1 de répondre</span><span class="sxs-lookup"><span data-stu-id="0fa1b-378">Respond version 1.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js

#### <a name="respond-version-140"></a><span data-ttu-id="0fa1b-379">Répondre version1.4.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-379">Respond version 1.4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js

#### <a name="respond-version-130"></a><span data-ttu-id="0fa1b-380">Répondre version version 1.3.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-380">Respond version 1.3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js

#### <a name="respond-version-120"></a><span data-ttu-id="0fa1b-381">Répondre version 1.2.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-381">Respond version 1.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="0fa1b-382">Versions d’amorçage sur le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-382">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="0fa1b-383">Les versions suivantes de [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap sont hébergés sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="0fa1b-383">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-400"></a><span data-ttu-id="0fa1b-384">Amorçage version 4.0.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-384">Bootstrap version 4.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-337"></a><span data-ttu-id="0fa1b-385">Version d’amorçage 3.3.7</span><span class="sxs-lookup"><span data-stu-id="0fa1b-385">Bootstrap version 3.3.7</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-336"></a><span data-ttu-id="0fa1b-386">Version d’amorçage 3.3.6</span><span class="sxs-lookup"><span data-stu-id="0fa1b-386">Bootstrap version 3.3.6</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-335"></a><span data-ttu-id="0fa1b-387">Amorçage version 3.3.5</span><span class="sxs-lookup"><span data-stu-id="0fa1b-387">Bootstrap version 3.3.5</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-334"></a><span data-ttu-id="0fa1b-388">Version d’amorçage 3.3.4</span><span class="sxs-lookup"><span data-stu-id="0fa1b-388">Bootstrap version 3.3.4</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-332"></a><span data-ttu-id="0fa1b-389">Version d’amorçage 3.3.2</span><span class="sxs-lookup"><span data-stu-id="0fa1b-389">Bootstrap version 3.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-331"></a><span data-ttu-id="0fa1b-390">Version d’amorçage 3.3.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-390">Bootstrap version 3.3.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-330"></a><span data-ttu-id="0fa1b-391">Version d’amorçage 3.3.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-391">Bootstrap version 3.3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-320"></a><span data-ttu-id="0fa1b-392">Version d’amorçage 3.2.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-392">Bootstrap version 3.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-311"></a><span data-ttu-id="0fa1b-393">Version d’amorçage 3.1.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-393">Bootstrap version 3.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-310"></a><span data-ttu-id="0fa1b-394">Amorçage version 3.1.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-394">Bootstrap version 3.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-303"></a><span data-ttu-id="0fa1b-395">Version d’amorçage 3.0.3</span><span class="sxs-lookup"><span data-stu-id="0fa1b-395">Bootstrap version 3.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-302"></a><span data-ttu-id="0fa1b-396">Version d’amorçage 3.0.2</span><span class="sxs-lookup"><span data-stu-id="0fa1b-396">Bootstrap version 3.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-301"></a><span data-ttu-id="0fa1b-397">Version d’amorçage 3.0.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-397">Bootstrap version 3.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-300"></a><span data-ttu-id="0fa1b-398">Version d’amorçage 3.0.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-398">Bootstrap version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-232"></a><span data-ttu-id="0fa1b-399">Amorçage version 2.3.2</span><span class="sxs-lookup"><span data-stu-id="0fa1b-399">Bootstrap version 2.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png

#### <a name="bootstrap-version-231"></a><span data-ttu-id="0fa1b-400">Amorçage version 2.3.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-400">Bootstrap version 2.3.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="0fa1b-401">Versions TouchCarousel d’amorçage sur le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-401">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="0fa1b-402">Les versions suivantes de [ https://github.com/ixisio/bootstrap-touch-carousel ] (https://github.com/ixisio/bootstrap-touch-carousel " https://github.com/ixisio/bootstrap-touch-carousel ") TouchCarousel du programme d’amorçage versions sont hébergées sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="0fa1b-402">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="0fa1b-403">Amorçage TouchCarousel version 0.8.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-403">Bootstrap TouchCarousel version 0.8.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css
- https://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="0fa1b-404">Versions de Hammer.js sur le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-404">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="0fa1b-405">Les versions suivantes de [ http://hammerjs.github.io/ ] (http://hammerjs.github.io/ " http://hammerjs.github.io/ ") Hammer.js versions sont hébergées sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="0fa1b-405">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="0fa1b-406">Hammer.js version 2.0.4</span><span class="sxs-lookup"><span data-stu-id="0fa1b-406">Hammer.js version 2.0.4</span></span>

- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js
- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js
- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="0fa1b-407">Web Forms ASP.NET et Ajax mises en production sur le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-407">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="0fa1b-408">Les versions suivantes de la bibliothèque ASP.NET Ajax sont hébergées sur le CDN.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-408">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="0fa1b-409">Cliquez sur chaque lien pour afficher la liste réelle des fichiers.</span><span class="sxs-lookup"><span data-stu-id="0fa1b-409">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="0fa1b-410">Version de Web Forms ASP.NET et Ajax 4.5.2</span><span class="sxs-lookup"><span data-stu-id="0fa1b-410">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "Web Forms ASP.NET et Ajax 4.5.2")
- [<span data-ttu-id="0fa1b-411">Version de Web Forms ASP.NET et Ajax 4</span><span class="sxs-lookup"><span data-stu-id="0fa1b-411">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "Web Forms ASP.NET et Ajax 4")
- [<span data-ttu-id="0fa1b-412">ASP.NET Ajax version 3.5</span><span class="sxs-lookup"><span data-stu-id="0fa1b-412">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "ASP.NET Ajax 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="0fa1b-413">ASP.NET MVC publie le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-413">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="0fa1b-414">Les fichiers ASP.NET MVC JavaScript suivants sont hébergés sur ce CDN :</span><span class="sxs-lookup"><span data-stu-id="0fa1b-414">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="0fa1b-415">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="0fa1b-415">ASP.NET MVC 5.2.3</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="0fa1b-416">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-416">ASP.NET MVC 5.1</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="0fa1b-417">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-417">ASP.NET MVC 5.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="0fa1b-418">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-418">ASP.NET MVC 4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="0fa1b-419">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-419">ASP.NET MVC 3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="0fa1b-420">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-420">ASP.NET MVC 2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="0fa1b-421">ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-421">ASP.NET MVC 1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="0fa1b-422">ASP.NET SignalR publie le CDN</span><span class="sxs-lookup"><span data-stu-id="0fa1b-422">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="0fa1b-423">Les fichiers ASP.NET SignalR JavaScript suivants sont hébergés sur ce CDN :</span><span class="sxs-lookup"><span data-stu-id="0fa1b-423">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="0fa1b-424">ASP.NET SignalR 2.2.2</span><span class="sxs-lookup"><span data-stu-id="0fa1b-424">ASP.NET SignalR 2.2.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="0fa1b-425">ASP.NET SignalR 2.2.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-425">ASP.NET SignalR 2.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="0fa1b-426">ASP.NET SignalR 2.2.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-426">ASP.NET SignalR 2.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="0fa1b-427">ASP.NET SignalR 2.1.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-427">ASP.NET SignalR 2.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="0fa1b-428">ASP.NET SignalR 2.0.3</span><span class="sxs-lookup"><span data-stu-id="0fa1b-428">ASP.NET SignalR 2.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="0fa1b-429">ASP.NET SignalR 2.0.2</span><span class="sxs-lookup"><span data-stu-id="0fa1b-429">ASP.NET SignalR 2.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="0fa1b-430">ASP.NET SignalR 2.0.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-430">ASP.NET SignalR 2.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="0fa1b-431">ASP.NET SignalR 2.0.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-431">ASP.NET SignalR 2.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="0fa1b-432">ASP.NET SignalR 1.1.3</span><span class="sxs-lookup"><span data-stu-id="0fa1b-432">ASP.NET SignalR 1.1.3</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="0fa1b-433">ASP.NET SignalR 1.1.2</span><span class="sxs-lookup"><span data-stu-id="0fa1b-433">ASP.NET SignalR 1.1.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="0fa1b-434">ASP.NET SignalR 1.1.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-434">ASP.NET SignalR 1.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="0fa1b-435">ASP.NET SignalR 1.1.0</span><span class="sxs-lookup"><span data-stu-id="0fa1b-435">ASP.NET SignalR 1.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="0fa1b-436">ASP.NET SignalR 1.0.1</span><span class="sxs-lookup"><span data-stu-id="0fa1b-436">ASP.NET SignalR 1.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js

<span data-ttu-id="0fa1b-437">Pour plus d’informations sur les conditions d’utilisation pour le CDN, consultez [Microsoft Ajax CDN conditions d’utilisation](https://www.asp.net/terms-of-use "Microsoft Ajax CDN conditions d’utilisation").</span><span class="sxs-lookup"><span data-stu-id="0fa1b-437">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
