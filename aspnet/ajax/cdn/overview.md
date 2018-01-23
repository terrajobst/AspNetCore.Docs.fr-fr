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
ms.openlocfilehash: f69f707ba64d13fc372b7bc44718c9dcf8cec6e2
ms.sourcegitcommit: 3f491f887074310fc0f145cd01a670aa63b969e3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/22/2018
---
<a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="460ca-102">Réseau de diffusion de contenu Microsoft Ajax</span><span class="sxs-lookup"><span data-stu-id="460ca-102">Microsoft Ajax Content Delivery Network</span></span>
====================
<span data-ttu-id="460ca-103">Remarque : Le CDN Microsoft Ajax n’a aucun contrat SLA au-dessus et au-delà de l’aide d’un CDN Azure.</span><span class="sxs-lookup"><span data-stu-id="460ca-103">Note: The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="460ca-104">Sommaire</span><span class="sxs-lookup"><span data-stu-id="460ca-104">Table of Contents</span></span>

<span data-ttu-id="460ca-105">**[AJAX.Microsoft.com renommé ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="460ca-105">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="460ca-106">**[Prise en charge de Visual Studio .vsdoc](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="460ca-106">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="460ca-107">**[À l’aide d’ASP.NET Ajax du CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="460ca-107">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="460ca-108">**[À l’aide de jQuery du CDN](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="460ca-108">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="460ca-109">**[À l’aide de jQuery UI du CDN](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="460ca-109">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="460ca-110">**[Fichiers de tiers dans le CDN](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="460ca-110">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="460ca-111">Versions de jQuery sur le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-111">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="460ca-112">jQuery migrer des versions sur le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-112">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="460ca-113">jQuery UI mises en production sur le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-113">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="460ca-114">jQuery Validation mises en production sur le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-114">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="460ca-115">jQuery Mobile mises en production sur le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-115">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="460ca-116">jQuery versions de modèles sur le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-116">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="460ca-117">jQuery Cycle des versions sur le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-117">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="460ca-118">jQuery versions de tables de données sur le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-118">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="460ca-119">Versions de Modernizr sur le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-119">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="460ca-120">Versions de JSHint sur le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-120">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="460ca-121">Versions de Knockout sur le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-121">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="460ca-122">Globaliser des versions sur le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-122">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="460ca-123">Répondre versions sur le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-123">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="460ca-124">Versions d’amorçage sur le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-124">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="460ca-125">Versions TouchCarousel d’amorçage sur le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-125">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="460ca-126">Versions de Hammer.js sur le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-126">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="460ca-127">Web Forms ASP.NET et Ajax mises en production sur le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-127">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="460ca-128">ASP.NET MVC publie le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-128">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="460ca-129">ASP.NET SignalR publie le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-129">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="460ca-130">Le contenu remise réseau CDN Microsoft Ajax () héberge des bibliothèques JavaScript de tiers populaires tels que jQuery et vous permet de facilement les ajouter à vos applications Web.</span><span class="sxs-lookup"><span data-stu-id="460ca-130">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="460ca-131">Par exemple, commencer à l’aide de jQuery, qui est hébergé sur ce CDN en ajoutant simplement un &lt;script&gt; balise à votre page qui pointe vers ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="460ca-131">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="460ca-132">En tirant parti du CDN, vous pouvez considérablement améliorer les performances de vos applications Ajax.</span><span class="sxs-lookup"><span data-stu-id="460ca-132">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="460ca-133">Le contenu du CDN est mis en cache sur les serveurs situés dans le monde entier.</span><span class="sxs-lookup"><span data-stu-id="460ca-133">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="460ca-134">En outre, le CDN permet aux navigateurs de réutiliser des fichiers JavaScript mis en cache tiers pour les sites web qui se trouvent dans des domaines différents.</span><span class="sxs-lookup"><span data-stu-id="460ca-134">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="460ca-135">Le CDN prend en charge SSL (HTTPS) au cas où vous deviez traiter une page web à l’aide de Secure Sockets Layer.</span><span class="sxs-lookup"><span data-stu-id="460ca-135">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="460ca-136">Le CDN héberge les bibliothèques de scripts tiers suivants qui ont été téléchargés et sont concédés sous licence, par les propriétaires de ces bibliothèques :</span><span class="sxs-lookup"><span data-stu-id="460ca-136">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="460ca-137">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="460ca-137">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="460ca-138">jQuery UI (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="460ca-138">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="460ca-139">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="460ca-139">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="460ca-140">jQuery Validation (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="460ca-140">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="460ca-141">jQuery Cycle (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="460ca-141">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="460ca-142">jQuery DataTables (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="460ca-142">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="460ca-143">Le CDN Microsoft Ajax inclut également les bibliothèques suivantes qui ont été téléchargés par Microsoft :</span><span class="sxs-lookup"><span data-stu-id="460ca-143">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="460ca-144">ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="460ca-144">ASP.NET Ajax</span></span>
- <span data-ttu-id="460ca-145">Fichiers JavaScript ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="460ca-145">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="460ca-146">Les fichiers ASP.NET SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="460ca-146">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="460ca-147">Microsoft ne revendique pas la propriété de toutes les bibliothèques tierces hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="460ca-147">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="460ca-148">Les propriétaires de copyright des bibliothèques de licence ces bibliothèques pour vous.</span><span class="sxs-lookup"><span data-stu-id="460ca-148">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="460ca-149">Les droits que vous devrez peut-être télécharger et utiliser ces bibliothèques sont accordées exclusivement par les propriétaires de copyright respectifs.</span><span class="sxs-lookup"><span data-stu-id="460ca-149">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="460ca-150">Étant donné que celles-ci ne sont pas des bibliothèques de Microsoft, Microsoft ne fournit aucune garantie ou les licences de droits de propriété intellectuelle (y compris sans droits de brevet implicites) pour les bibliothèques tierces hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="460ca-150">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="460ca-151">Si vous souhaitez soumettre votre bibliothèque JavaScript et votre bibliothèque est un des extensions/plug-ins à ces bibliothèques ou des bibliothèques JavaScript (comme indiqué sur http://trends.builtwith.com) supérieur (a) répandus. ou (b) utile pour les utiliser sur ASP.NET, puis contactez AjaxCDNSubmission@Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="460ca-151">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="460ca-152">AJAX.Microsoft.com renommé ajax.aspnetcdn.com</span><span class="sxs-lookup"><span data-stu-id="460ca-152">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="460ca-153">Le CDN permet d’utiliser le nom de domaine microsoft.com et a été modifié pour utiliser le nom de domaine aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="460ca-153">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="460ca-154">Cette modification a été apportée pour augmenter les performances, car lorsqu’un navigateur référencé le domaine microsoft.com il envoie tous les cookies à partir de ce domaine sur le réseau avec chaque demande.</span><span class="sxs-lookup"><span data-stu-id="460ca-154">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="460ca-155">En renommant à un nom de domaine différent de microsoft.com performances peuvent être augmenté par autant de 25 %.</span><span class="sxs-lookup"><span data-stu-id="460ca-155">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="460ca-156">Remarque ajax.microsoft.com continuera de fonctionner mais ajax.aspnetcdn.com est recommandé.</span><span class="sxs-lookup"><span data-stu-id="460ca-156">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="460ca-157">Ancien Format : http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="460ca-157">Old Format: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="460ca-158">Nouveau Format : http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="460ca-158">New Format: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="460ca-159">Prise en charge de Visual Studio .vsdoc</span><span class="sxs-lookup"><span data-stu-id="460ca-159">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="460ca-160">Pour utiliser les fichiers .vsdoc correctement avec Visual Studio 2008, vous devez vous assurer que vous disposez de Visual Studio 2008 SP1 installé et le correctif logiciel pour les fichiers vsdoc installé.</span><span class="sxs-lookup"><span data-stu-id="460ca-160">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="460ca-161">Vous pouvez obtenir à partir d’ici :</span><span class="sxs-lookup"><span data-stu-id="460ca-161">You can get these from here:</span></span>

- [<span data-ttu-id="460ca-162">Télécharger Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="460ca-162">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "télécharger Visual Studio 2008 SP1")
- [<span data-ttu-id="460ca-163">Télécharger le correctif logiciel de .vsdoc pour Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="460ca-163">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 "télécharger .vsdoc correctif pour Visual Studio 2008 SP1")

<span data-ttu-id="460ca-164">Visual Studio 2010 prend en charge les fichiers .vsdoc sans les correctifs supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="460ca-164">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="460ca-165">À l’aide d’ASP.NET Ajax du CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-165">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="460ca-166">Lorsque vous utilisez ASP.NET 4, vous pouvez rediriger toutes les demandes pour les scripts du framework ASP.NET vers le CDN.</span><span class="sxs-lookup"><span data-stu-id="460ca-166">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="460ca-167">La récupération des scripts à partir du CDN au lieu de votre serveur web local peuvent considérablement améliorer les performances des sites Web ASP.NET publics.</span><span class="sxs-lookup"><span data-stu-id="460ca-167">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="460ca-168">Utilisez la propriété ScriptManager EnableCDN pour rediriger toutes les demandes de script framework ASP.NET pour le CDN Microsoft Ajax :</span><span class="sxs-lookup"><span data-stu-id="460ca-168">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="460ca-169">À l’aide de jQuery du CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-169">Using jQuery from the CDN</span></span>

<span data-ttu-id="460ca-170">Vous pouvez utiliser des scripts jQuery hébergés sur CDN dans votre application Web en ajoutant l’élément de script suivant à une page :</span><span class="sxs-lookup"><span data-stu-id="460ca-170">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="460ca-171">Le CDN inclut également la version réduite du script jQuery, que vous pouvez obtenir à l’aide de l’élément suivant :</span><span class="sxs-lookup"><span data-stu-id="460ca-171">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="460ca-172">Pour permettre à votre page à revenir au chargement jQuery à partir d’un chemin d’accès local sur votre propre site Web si le CDN est indisponible, ajoutez l’élément suivant immédiatement après l’élément référençant le CDN :</span><span class="sxs-lookup"><span data-stu-id="460ca-172">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="460ca-173">L’exemple de page suivant utilise la version CDN de la bibliothèque jQuery (avec basculement vers une copie locale) pour afficher le contenu d’un élément div lorsqu’un clic est effectué.</span><span class="sxs-lookup"><span data-stu-id="460ca-173">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="460ca-174">Vous pouvez en savoir plus sur jQuery et télécharger une copie locale de jQuery en vous rendant sur le [jQuery](http://jquery.com/) site Web.</span><span class="sxs-lookup"><span data-stu-id="460ca-174">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="460ca-175">À l’aide de jQuery UI du CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-175">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="460ca-176">Le CDN héberge également la bibliothèque jQuery UI.</span><span class="sxs-lookup"><span data-stu-id="460ca-176">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="460ca-177">La bibliothèque jQuery UI comprend un ensemble complet de widgets et les effets que vous pouvez utiliser dans vos applications ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="460ca-177">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="460ca-178">Par exemple, la page suivante illustre comment vous pouvez utiliser la sélecteur de dates de l’interface utilisateur jQuery dans le contexte d’une application Web Forms ASP.NET pour afficher un calendrier contextuel :</span><span class="sxs-lookup"><span data-stu-id="460ca-178">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="460ca-179">Lorsque vous déplacez le focus vers la zone de texte à l’aide de votre clavier, un calendrier s’affiche :</span><span class="sxs-lookup"><span data-stu-id="460ca-179">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Calendrier contextuel créé avec le sélecteur de dates](overview/_static/image1.png)

<span data-ttu-id="460ca-181">Notez que vous devez inclure les trois fichiers du CDN dans le code ci-dessus :</span><span class="sxs-lookup"><span data-stu-id="460ca-181">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="460ca-182">La bibliothèque jQuery &mdash; la bibliothèque jQuery UI dépend de la bibliothèque jQuery.</span><span class="sxs-lookup"><span data-stu-id="460ca-182">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="460ca-183">Vous devez ajouter la bibliothèque jQuery à votre page avant d’ajouter la bibliothèque jQuery UI.</span><span class="sxs-lookup"><span data-stu-id="460ca-183">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="460ca-184">La bibliothèque jQuery UI &mdash; la bibliothèque jQuery UI contient tous les effets d’interface utilisateur jQuery et widgets tels que le widget de sélecteur de dates utilisées dans la page ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="460ca-184">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="460ca-185">Un thème de l’interface utilisateur jQuery &mdash; jQuery UI prend en charge différents thèmes.</span><span class="sxs-lookup"><span data-stu-id="460ca-185">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="460ca-186">La page ci-dessus inclut un lien vers un fichier CSS pour importer le thème de Redmond.</span><span class="sxs-lookup"><span data-stu-id="460ca-186">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="460ca-187">Tous les thèmes de l’interface utilisateur jQuery standard sont hébergés sur le CDN.</span><span class="sxs-lookup"><span data-stu-id="460ca-187">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="460ca-188">[Visitez cette page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 dans le CDN Microsoft Ajax") pour afficher les miniatures pour chaque thème.</span><span class="sxs-lookup"><span data-stu-id="460ca-188">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="460ca-189">Pour plus d’informations sur la bibliothèque jQuery UI, visitez officielle [site Web de l’interface utilisateur jQuery](http://jQueryUI.com "site Web de l’interface utilisateur jQuery").</span><span class="sxs-lookup"><span data-stu-id="460ca-189">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="460ca-190">Fichiers de tiers dans le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-190">Third-Party Files on the CDN</span></span>

<span data-ttu-id="460ca-191">Le CDN héberge certains des bibliothèques JavaScript tiers les plus populaires.</span><span class="sxs-lookup"><span data-stu-id="460ca-191">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="460ca-192">Microsoft ne revendique pas la propriété de toutes les bibliothèques tierces hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="460ca-192">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="460ca-193">Les propriétaires de copyright des bibliothèques de licence ces bibliothèques pour vous.</span><span class="sxs-lookup"><span data-stu-id="460ca-193">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="460ca-194">Les droits que vous devrez peut-être télécharger et utiliser ces bibliothèques sont accordées exclusivement par les propriétaires de copyright respectifs.</span><span class="sxs-lookup"><span data-stu-id="460ca-194">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="460ca-195">Étant donné que celles-ci ne sont pas des bibliothèques de Microsoft, Microsoft ne fournit aucune garantie ou les licences de droits de propriété intellectuelle (y compris sans droits de brevet implicites) pour les bibliothèques tierces hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="460ca-195">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="460ca-196">Versions de jQuery sur le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-196">jQuery Releases on the CDN</span></span>

<span data-ttu-id="460ca-197">Les versions suivantes de jQuery sont hébergées sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="460ca-197">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-331"></a><span data-ttu-id="460ca-198">version de jQuery 3.3.1</span><span class="sxs-lookup"><span data-stu-id="460ca-198">jQuery version 3.3.1</span></span>
- <span data-ttu-id="460ca-199">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.js</span><span class="sxs-lookup"><span data-stu-id="460ca-199">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js</span></span>
- <span data-ttu-id="460ca-200">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-200">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js</span></span>
- <span data-ttu-id="460ca-201">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-201">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map</span></span>
- <span data-ttu-id="460ca-202">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.SLIM.js</span><span class="sxs-lookup"><span data-stu-id="460ca-202">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js</span></span>
- <span data-ttu-id="460ca-203">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.SLIM.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-203">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js</span></span>
- <span data-ttu-id="460ca-204">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.SLIM.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-204">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map</span></span>

#### <a name="jquery-version-321"></a><span data-ttu-id="460ca-205">jQuery version 3.2.1</span><span class="sxs-lookup"><span data-stu-id="460ca-205">jQuery version 3.2.1</span></span>
- <span data-ttu-id="460ca-206">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.js</span><span class="sxs-lookup"><span data-stu-id="460ca-206">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js</span></span>
- <span data-ttu-id="460ca-207">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-207">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js</span></span>
- <span data-ttu-id="460ca-208">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-208">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map</span></span>
- <span data-ttu-id="460ca-209">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.SLIM.js</span><span class="sxs-lookup"><span data-stu-id="460ca-209">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js</span></span>
- <span data-ttu-id="460ca-210">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.SLIM.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-210">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js</span></span>
- <span data-ttu-id="460ca-211">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.SLIM.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-211">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map</span></span>

#### <a name="jquery-version-320"></a><span data-ttu-id="460ca-212">version de jQuery 3.2.0</span><span class="sxs-lookup"><span data-stu-id="460ca-212">jQuery version 3.2.0</span></span>

- <span data-ttu-id="460ca-213">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.js</span><span class="sxs-lookup"><span data-stu-id="460ca-213">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js</span></span>
- <span data-ttu-id="460ca-214">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-214">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js</span></span>
- <span data-ttu-id="460ca-215">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-215">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map</span></span>
- <span data-ttu-id="460ca-216">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.SLIM.js</span><span class="sxs-lookup"><span data-stu-id="460ca-216">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js</span></span>
- <span data-ttu-id="460ca-217">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.SLIM.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-217">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js</span></span>
- <span data-ttu-id="460ca-218">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.SLIM.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-218">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map</span></span>

#### <a name="jquery-version-311"></a><span data-ttu-id="460ca-219">version de jQuery 3.1.1</span><span class="sxs-lookup"><span data-stu-id="460ca-219">jQuery version 3.1.1</span></span>

- <span data-ttu-id="460ca-220">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.js</span><span class="sxs-lookup"><span data-stu-id="460ca-220">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js</span></span>
- <span data-ttu-id="460ca-221">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-221">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js</span></span>
- <span data-ttu-id="460ca-222">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-222">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map</span></span>
- <span data-ttu-id="460ca-223">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.SLIM.js</span><span class="sxs-lookup"><span data-stu-id="460ca-223">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js</span></span>
- <span data-ttu-id="460ca-224">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.SLIM.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-224">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js</span></span>
- <span data-ttu-id="460ca-225">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.SLIM.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-225">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map</span></span>

#### <a name="jquery-version-310"></a><span data-ttu-id="460ca-226">jQuery version 3.1.0</span><span class="sxs-lookup"><span data-stu-id="460ca-226">jQuery version 3.1.0</span></span>

- <span data-ttu-id="460ca-227">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.js</span><span class="sxs-lookup"><span data-stu-id="460ca-227">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js</span></span>
- <span data-ttu-id="460ca-228">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-228">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js</span></span>
- <span data-ttu-id="460ca-229">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-229">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map</span></span>
- <span data-ttu-id="460ca-230">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.SLIM.js</span><span class="sxs-lookup"><span data-stu-id="460ca-230">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js</span></span>
- <span data-ttu-id="460ca-231">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.SLIM.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-231">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js</span></span>
- <span data-ttu-id="460ca-232">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.SLIM.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-232">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map</span></span>

#### <a name="jquery-version-300"></a><span data-ttu-id="460ca-233">version de jQuery 3.0.0</span><span class="sxs-lookup"><span data-stu-id="460ca-233">jQuery version 3.0.0</span></span>

- <span data-ttu-id="460ca-234">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="460ca-234">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js</span></span>
- <span data-ttu-id="460ca-235">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-235">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js</span></span>
- <span data-ttu-id="460ca-236">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-236">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map</span></span>
- <span data-ttu-id="460ca-237">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.SLIM.js</span><span class="sxs-lookup"><span data-stu-id="460ca-237">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js</span></span>
- <span data-ttu-id="460ca-238">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.SLIM.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-238">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js</span></span>
- <span data-ttu-id="460ca-239">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.SLIM.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-239">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map</span></span>

#### <a name="jquery-version-224"></a><span data-ttu-id="460ca-240">version de jQuery 2.2.4</span><span class="sxs-lookup"><span data-stu-id="460ca-240">jQuery version 2.2.4</span></span>

- <span data-ttu-id="460ca-241">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.js</span><span class="sxs-lookup"><span data-stu-id="460ca-241">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js</span></span>
- <span data-ttu-id="460ca-242">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-242">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js</span></span>
- <span data-ttu-id="460ca-243">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-243">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map</span></span>

#### <a name="jquery-version-223"></a><span data-ttu-id="460ca-244">version de jQuery 2.2.3</span><span class="sxs-lookup"><span data-stu-id="460ca-244">jQuery version 2.2.3</span></span>

- <span data-ttu-id="460ca-245">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.js</span><span class="sxs-lookup"><span data-stu-id="460ca-245">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js</span></span>
- <span data-ttu-id="460ca-246">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-246">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js</span></span>
- <span data-ttu-id="460ca-247">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-247">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map</span></span>

#### <a name="jquery-version-222"></a><span data-ttu-id="460ca-248">version de jQuery 2.2.2</span><span class="sxs-lookup"><span data-stu-id="460ca-248">jQuery version 2.2.2</span></span>

- <span data-ttu-id="460ca-249">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.js</span><span class="sxs-lookup"><span data-stu-id="460ca-249">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js</span></span>
- <span data-ttu-id="460ca-250">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-250">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js</span></span>
- <span data-ttu-id="460ca-251">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-251">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map</span></span>

#### <a name="jquery-version-221"></a><span data-ttu-id="460ca-252">jQuery version 2.2.1</span><span class="sxs-lookup"><span data-stu-id="460ca-252">jQuery version 2.2.1</span></span>

- <span data-ttu-id="460ca-253">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="460ca-253">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js</span></span>
- <span data-ttu-id="460ca-254">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-254">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js</span></span>
- <span data-ttu-id="460ca-255">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-255">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map</span></span>

#### <a name="jquery-version-220"></a><span data-ttu-id="460ca-256">jQuery version2.2.0</span><span class="sxs-lookup"><span data-stu-id="460ca-256">jQuery version 2.2.0</span></span>

- <span data-ttu-id="460ca-257">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="460ca-257">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js</span></span>
- <span data-ttu-id="460ca-258">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-258">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js</span></span>
- <span data-ttu-id="460ca-259">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-259">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map</span></span>

#### <a name="jquery-version-214"></a><span data-ttu-id="460ca-260">version de jQuery 2.1.4</span><span class="sxs-lookup"><span data-stu-id="460ca-260">jQuery version 2.1.4</span></span>

- <span data-ttu-id="460ca-261">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.js</span><span class="sxs-lookup"><span data-stu-id="460ca-261">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js</span></span>
- <span data-ttu-id="460ca-262">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-262">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js</span></span>
- <span data-ttu-id="460ca-263">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-263">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map</span></span>

#### <a name="jquery-version-213"></a><span data-ttu-id="460ca-264">version de jQuery 2.1.3</span><span class="sxs-lookup"><span data-stu-id="460ca-264">jQuery version 2.1.3</span></span>

- <span data-ttu-id="460ca-265">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.js</span><span class="sxs-lookup"><span data-stu-id="460ca-265">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js</span></span>
- <span data-ttu-id="460ca-266">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-266">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js</span></span>
- <span data-ttu-id="460ca-267">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-267">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map</span></span>

#### <a name="jquery-version-212"></a><span data-ttu-id="460ca-268">version de jQuery 2.1.2</span><span class="sxs-lookup"><span data-stu-id="460ca-268">jQuery version 2.1.2</span></span>

- <span data-ttu-id="460ca-269">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.2.js</span><span class="sxs-lookup"><span data-stu-id="460ca-269">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js</span></span>
- <span data-ttu-id="460ca-270">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.2.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-270">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js</span></span>

#### <a name="jquery-version-211"></a><span data-ttu-id="460ca-271">version de jQuery 2.1.1</span><span class="sxs-lookup"><span data-stu-id="460ca-271">jQuery version 2.1.1</span></span>

- <span data-ttu-id="460ca-272">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.js</span><span class="sxs-lookup"><span data-stu-id="460ca-272">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js</span></span>
- <span data-ttu-id="460ca-273">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-273">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js</span></span>
- <span data-ttu-id="460ca-274">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-274">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map</span></span>

#### <a name="jquery-version-210"></a><span data-ttu-id="460ca-275">version de jQuery 2.1.0</span><span class="sxs-lookup"><span data-stu-id="460ca-275">jQuery version 2.1.0</span></span>

- <span data-ttu-id="460ca-276">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="460ca-276">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js</span></span>
- <span data-ttu-id="460ca-277">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-277">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js</span></span>
- <span data-ttu-id="460ca-278">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="460ca-278">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js</span></span>
- <span data-ttu-id="460ca-279">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-279">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map</span></span>

#### <a name="jquery-version-203"></a><span data-ttu-id="460ca-280">version de jQuery 2.0.3</span><span class="sxs-lookup"><span data-stu-id="460ca-280">jQuery version 2.0.3</span></span>

- <span data-ttu-id="460ca-281">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.js</span><span class="sxs-lookup"><span data-stu-id="460ca-281">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js</span></span>
- <span data-ttu-id="460ca-282">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-282">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js</span></span>
- <span data-ttu-id="460ca-283">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="460ca-283">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js</span></span>
- <span data-ttu-id="460ca-284">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-284">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map</span></span>

#### <a name="jquery-version-202"></a><span data-ttu-id="460ca-285">version de jQuery 2.0.2</span><span class="sxs-lookup"><span data-stu-id="460ca-285">jQuery version 2.0.2</span></span>

- <span data-ttu-id="460ca-286">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.js</span><span class="sxs-lookup"><span data-stu-id="460ca-286">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js</span></span>
- <span data-ttu-id="460ca-287">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-287">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js</span></span>
- <span data-ttu-id="460ca-288">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="460ca-288">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js</span></span>
- <span data-ttu-id="460ca-289">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-289">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map</span></span>

#### <a name="jquery-version-201"></a><span data-ttu-id="460ca-290">version de jQuery 2.0.1</span><span class="sxs-lookup"><span data-stu-id="460ca-290">jQuery version 2.0.1</span></span>

- <span data-ttu-id="460ca-291">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.js</span><span class="sxs-lookup"><span data-stu-id="460ca-291">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js</span></span>
- <span data-ttu-id="460ca-292">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-292">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js</span></span>
- <span data-ttu-id="460ca-293">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="460ca-293">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js</span></span>
- <span data-ttu-id="460ca-294">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-294">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map</span></span>

#### <a name="jquery-version-200"></a><span data-ttu-id="460ca-295">jQuery version 2.0.0</span><span class="sxs-lookup"><span data-stu-id="460ca-295">jQuery version 2.0.0</span></span>

- <span data-ttu-id="460ca-296">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.js</span><span class="sxs-lookup"><span data-stu-id="460ca-296">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js</span></span>
- <span data-ttu-id="460ca-297">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-297">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js</span></span>
- <span data-ttu-id="460ca-298">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="460ca-298">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js</span></span>
- <span data-ttu-id="460ca-299">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-299">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map</span></span>

#### <a name="jquery-version-1124"></a><span data-ttu-id="460ca-300">version de jQuery 1.12.4</span><span class="sxs-lookup"><span data-stu-id="460ca-300">jQuery version 1.12.4</span></span>

- <span data-ttu-id="460ca-301">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.js</span><span class="sxs-lookup"><span data-stu-id="460ca-301">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js</span></span>
- <span data-ttu-id="460ca-302">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-302">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js</span></span>
- <span data-ttu-id="460ca-303">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-303">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map</span></span>

#### <a name="jquery-version-1123"></a><span data-ttu-id="460ca-304">version de jQuery 1.12.3</span><span class="sxs-lookup"><span data-stu-id="460ca-304">jQuery version 1.12.3</span></span>

- <span data-ttu-id="460ca-305">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.js</span><span class="sxs-lookup"><span data-stu-id="460ca-305">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js</span></span>
- <span data-ttu-id="460ca-306">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-306">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js</span></span>
- <span data-ttu-id="460ca-307">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-307">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map</span></span>

#### <a name="jquery-version-1122"></a><span data-ttu-id="460ca-308">version de jQuery 1.12.2</span><span class="sxs-lookup"><span data-stu-id="460ca-308">jQuery version 1.12.2</span></span>

- <span data-ttu-id="460ca-309">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.js</span><span class="sxs-lookup"><span data-stu-id="460ca-309">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js</span></span>
- <span data-ttu-id="460ca-310">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-310">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js</span></span>
- <span data-ttu-id="460ca-311">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-311">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map</span></span>

#### <a name="jquery-version-1121"></a><span data-ttu-id="460ca-312">version de jQuery 1.12.1</span><span class="sxs-lookup"><span data-stu-id="460ca-312">jQuery version 1.12.1</span></span>

- <span data-ttu-id="460ca-313">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.js</span><span class="sxs-lookup"><span data-stu-id="460ca-313">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js</span></span>
- <span data-ttu-id="460ca-314">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-314">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js</span></span>
- <span data-ttu-id="460ca-315">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-315">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map</span></span>

#### <a name="jquery-version-1120"></a><span data-ttu-id="460ca-316">version de jQuery 1.12.0</span><span class="sxs-lookup"><span data-stu-id="460ca-316">jQuery version 1.12.0</span></span>

- <span data-ttu-id="460ca-317">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.js</span><span class="sxs-lookup"><span data-stu-id="460ca-317">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js</span></span>
- <span data-ttu-id="460ca-318">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-318">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js</span></span>
- <span data-ttu-id="460ca-319">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-319">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map</span></span>

#### <a name="jquery-version-1113"></a><span data-ttu-id="460ca-320">version de jQuery 1.11.3</span><span class="sxs-lookup"><span data-stu-id="460ca-320">jQuery version 1.11.3</span></span>

- <span data-ttu-id="460ca-321">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.js</span><span class="sxs-lookup"><span data-stu-id="460ca-321">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js</span></span>
- <span data-ttu-id="460ca-322">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-322">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js</span></span>
- <span data-ttu-id="460ca-323">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-323">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map</span></span>

#### <a name="jquery-version-1112"></a><span data-ttu-id="460ca-324">version de jQuery 1.11.2</span><span class="sxs-lookup"><span data-stu-id="460ca-324">jQuery version 1.11.2</span></span>

- <span data-ttu-id="460ca-325">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.js</span><span class="sxs-lookup"><span data-stu-id="460ca-325">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js</span></span>
- <span data-ttu-id="460ca-326">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-326">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js</span></span>
- <span data-ttu-id="460ca-327">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-327">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map</span></span>

#### <a name="jquery-version-1111"></a><span data-ttu-id="460ca-328">version de jQuery 1.11.1</span><span class="sxs-lookup"><span data-stu-id="460ca-328">jQuery version 1.11.1</span></span>

- <span data-ttu-id="460ca-329">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.js</span><span class="sxs-lookup"><span data-stu-id="460ca-329">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js</span></span>
- <span data-ttu-id="460ca-330">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-330">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js</span></span>
- <span data-ttu-id="460ca-331">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-331">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map</span></span>

#### <a name="jquery-version-1110"></a><span data-ttu-id="460ca-332">version de jQuery 1.11.0</span><span class="sxs-lookup"><span data-stu-id="460ca-332">jQuery version 1.11.0</span></span>

- <span data-ttu-id="460ca-333">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.js</span><span class="sxs-lookup"><span data-stu-id="460ca-333">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js</span></span>
- <span data-ttu-id="460ca-334">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-334">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js</span></span>
- <span data-ttu-id="460ca-335">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="460ca-335">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js</span></span>
- <span data-ttu-id="460ca-336">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-336">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map</span></span>

#### <a name="jquery-version-1102"></a><span data-ttu-id="460ca-337">version de jQuery 1.10.2</span><span class="sxs-lookup"><span data-stu-id="460ca-337">jQuery version 1.10.2</span></span>

- <span data-ttu-id="460ca-338">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.js</span><span class="sxs-lookup"><span data-stu-id="460ca-338">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js</span></span>
- <span data-ttu-id="460ca-339">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-339">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js</span></span>
- <span data-ttu-id="460ca-340">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="460ca-340">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js</span></span>
- <span data-ttu-id="460ca-341">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-341">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map</span></span>

#### <a name="jquery-version-1101"></a><span data-ttu-id="460ca-342">version de jQuery 1.10.1</span><span class="sxs-lookup"><span data-stu-id="460ca-342">jQuery version 1.10.1</span></span>

- <span data-ttu-id="460ca-343">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.js</span><span class="sxs-lookup"><span data-stu-id="460ca-343">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js</span></span>
- <span data-ttu-id="460ca-344">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-344">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js</span></span>
- <span data-ttu-id="460ca-345">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="460ca-345">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js</span></span>
- <span data-ttu-id="460ca-346">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-346">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map</span></span>

#### <a name="jquery-version-1100"></a><span data-ttu-id="460ca-347">version de jQuery 1.10.0</span><span class="sxs-lookup"><span data-stu-id="460ca-347">jQuery version 1.10.0</span></span>

- <span data-ttu-id="460ca-348">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.js</span><span class="sxs-lookup"><span data-stu-id="460ca-348">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js</span></span>
- <span data-ttu-id="460ca-349">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-349">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js</span></span>
- <span data-ttu-id="460ca-350">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="460ca-350">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js</span></span>
- <span data-ttu-id="460ca-351">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-351">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map</span></span>

#### <a name="jquery-version-191"></a><span data-ttu-id="460ca-352">version de jQuery 1.9.1</span><span class="sxs-lookup"><span data-stu-id="460ca-352">jQuery version 1.9.1</span></span>

- <span data-ttu-id="460ca-353">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.js</span><span class="sxs-lookup"><span data-stu-id="460ca-353">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js</span></span>
- <span data-ttu-id="460ca-354">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-354">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js</span></span>
- <span data-ttu-id="460ca-355">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="460ca-355">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js</span></span>
- <span data-ttu-id="460ca-356">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-356">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map</span></span>

#### <a name="jquery-version-190"></a><span data-ttu-id="460ca-357">version de jQuery à 1.9.0</span><span class="sxs-lookup"><span data-stu-id="460ca-357">jQuery version 1.9.0</span></span>

- <span data-ttu-id="460ca-358">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.js</span><span class="sxs-lookup"><span data-stu-id="460ca-358">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js</span></span>
- <span data-ttu-id="460ca-359">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-359">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js</span></span>
- <span data-ttu-id="460ca-360">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="460ca-360">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js</span></span>
- <span data-ttu-id="460ca-361">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-361">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map</span></span>

#### <a name="jquery-version-183"></a><span data-ttu-id="460ca-362">version de jQuery 1.8.3</span><span class="sxs-lookup"><span data-stu-id="460ca-362">jQuery version 1.8.3</span></span>

- <span data-ttu-id="460ca-363">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3.js</span><span class="sxs-lookup"><span data-stu-id="460ca-363">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js</span></span>
- <span data-ttu-id="460ca-364">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-364">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js</span></span>
- <span data-ttu-id="460ca-365">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="460ca-365">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js</span></span>

#### <a name="jquery-version-182"></a><span data-ttu-id="460ca-366">version de jQuery 1.8.2</span><span class="sxs-lookup"><span data-stu-id="460ca-366">jQuery version 1.8.2</span></span>

- <span data-ttu-id="460ca-367">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2.js</span><span class="sxs-lookup"><span data-stu-id="460ca-367">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js</span></span>
- <span data-ttu-id="460ca-368">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-368">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js</span></span>
- <span data-ttu-id="460ca-369">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="460ca-369">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js</span></span>

#### <a name="jquery-version-181"></a><span data-ttu-id="460ca-370">version de jQuery 1.8.1</span><span class="sxs-lookup"><span data-stu-id="460ca-370">jQuery version 1.8.1</span></span>

- <span data-ttu-id="460ca-371">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1.js</span><span class="sxs-lookup"><span data-stu-id="460ca-371">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js</span></span>
- <span data-ttu-id="460ca-372">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-372">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js</span></span>
- <span data-ttu-id="460ca-373">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="460ca-373">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js</span></span>

#### <a name="jquery-version-180"></a><span data-ttu-id="460ca-374">version de jQuery 1.8.0</span><span class="sxs-lookup"><span data-stu-id="460ca-374">jQuery version 1.8.0</span></span>

- <span data-ttu-id="460ca-375">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="460ca-375">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="460ca-376">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-376">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js</span></span>
- <span data-ttu-id="460ca-377">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="460ca-377">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js</span></span>

#### <a name="jquery-version-172"></a><span data-ttu-id="460ca-378">version de jQuery 1.7.2</span><span class="sxs-lookup"><span data-stu-id="460ca-378">jQuery version 1.7.2</span></span>

- <span data-ttu-id="460ca-379">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.2.js</span><span class="sxs-lookup"><span data-stu-id="460ca-379">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js</span></span>
- <span data-ttu-id="460ca-380">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.2.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-380">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js</span></span>

#### <a name="jquery-version-171"></a><span data-ttu-id="460ca-381">jQuery version 1.7.1</span><span class="sxs-lookup"><span data-stu-id="460ca-381">jQuery version 1.7.1</span></span>

- <span data-ttu-id="460ca-382">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1.js</span><span class="sxs-lookup"><span data-stu-id="460ca-382">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js</span></span>
- <span data-ttu-id="460ca-383">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-383">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js</span></span>
- <span data-ttu-id="460ca-384">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="460ca-384">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js</span></span>

#### <a name="jquery-version-17"></a><span data-ttu-id="460ca-385">jQuery version 1.7</span><span class="sxs-lookup"><span data-stu-id="460ca-385">jQuery version 1.7</span></span>

- <span data-ttu-id="460ca-386">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.js</span><span class="sxs-lookup"><span data-stu-id="460ca-386">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js</span></span>
- <span data-ttu-id="460ca-387">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-387">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js</span></span>
- <span data-ttu-id="460ca-388">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="460ca-388">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js</span></span>

#### <a name="jquery-version-164"></a><span data-ttu-id="460ca-389">version de jQuery 1.6.4</span><span class="sxs-lookup"><span data-stu-id="460ca-389">jQuery version 1.6.4</span></span>

- <span data-ttu-id="460ca-390">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4.js</span><span class="sxs-lookup"><span data-stu-id="460ca-390">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js</span></span>
- <span data-ttu-id="460ca-391">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-391">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js</span></span>
- <span data-ttu-id="460ca-392">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="460ca-392">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js</span></span>

#### <a name="jquery-version-163"></a><span data-ttu-id="460ca-393">version de jQuery 1.6.3</span><span class="sxs-lookup"><span data-stu-id="460ca-393">jQuery version 1.6.3</span></span>

- <span data-ttu-id="460ca-394">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3.js</span><span class="sxs-lookup"><span data-stu-id="460ca-394">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js</span></span>
- <span data-ttu-id="460ca-395">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-395">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js</span></span>
- <span data-ttu-id="460ca-396">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="460ca-396">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js</span></span>

#### <a name="jquery-version-162"></a><span data-ttu-id="460ca-397">version de jQuery 1.6.2</span><span class="sxs-lookup"><span data-stu-id="460ca-397">jQuery version 1.6.2</span></span>

- <span data-ttu-id="460ca-398">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2.js</span><span class="sxs-lookup"><span data-stu-id="460ca-398">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js</span></span>
- <span data-ttu-id="460ca-399">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-399">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js</span></span>
- <span data-ttu-id="460ca-400">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="460ca-400">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js</span></span>

#### <a name="jquery-version-161"></a><span data-ttu-id="460ca-401">version de jQuery 1.6.1</span><span class="sxs-lookup"><span data-stu-id="460ca-401">jQuery version 1.6.1</span></span>

- <span data-ttu-id="460ca-402">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1.js</span><span class="sxs-lookup"><span data-stu-id="460ca-402">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js</span></span>
- <span data-ttu-id="460ca-403">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-403">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js</span></span>
- <span data-ttu-id="460ca-404">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="460ca-404">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js</span></span>

#### <a name="jquery-version-16"></a><span data-ttu-id="460ca-405">jQuery version 1.6</span><span class="sxs-lookup"><span data-stu-id="460ca-405">jQuery version 1.6</span></span>

- <span data-ttu-id="460ca-406">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.js</span><span class="sxs-lookup"><span data-stu-id="460ca-406">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js</span></span>
- <span data-ttu-id="460ca-407">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-407">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js</span></span>
- <span data-ttu-id="460ca-408">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="460ca-408">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js</span></span>

#### <a name="jquery-version-152"></a><span data-ttu-id="460ca-409">version de jQuery 1.5.2</span><span class="sxs-lookup"><span data-stu-id="460ca-409">jQuery version 1.5.2</span></span>

- <span data-ttu-id="460ca-410">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2.js</span><span class="sxs-lookup"><span data-stu-id="460ca-410">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js</span></span>
- <span data-ttu-id="460ca-411">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-411">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js</span></span>
- <span data-ttu-id="460ca-412">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="460ca-412">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js</span></span>

#### <a name="jquery-version-151"></a><span data-ttu-id="460ca-413">version de jQuery 1.5.1</span><span class="sxs-lookup"><span data-stu-id="460ca-413">jQuery version 1.5.1</span></span>

- <span data-ttu-id="460ca-414">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1.js</span><span class="sxs-lookup"><span data-stu-id="460ca-414">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js</span></span>
- <span data-ttu-id="460ca-415">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-415">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js</span></span>
- <span data-ttu-id="460ca-416">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="460ca-416">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js</span></span>

#### <a name="jquery-version-15"></a><span data-ttu-id="460ca-417">version de jQuery 1.5</span><span class="sxs-lookup"><span data-stu-id="460ca-417">jQuery version 1.5</span></span>

- <span data-ttu-id="460ca-418">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.js</span><span class="sxs-lookup"><span data-stu-id="460ca-418">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js</span></span>
- <span data-ttu-id="460ca-419">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-419">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js</span></span>
- <span data-ttu-id="460ca-420">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="460ca-420">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js</span></span>

#### <a name="jquery-version-144"></a><span data-ttu-id="460ca-421">version de jQuery 1.4.4</span><span class="sxs-lookup"><span data-stu-id="460ca-421">jQuery version 1.4.4</span></span>

- <span data-ttu-id="460ca-422">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4.js</span><span class="sxs-lookup"><span data-stu-id="460ca-422">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js</span></span>
- <span data-ttu-id="460ca-423">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-423">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js</span></span>
- <span data-ttu-id="460ca-424">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="460ca-424">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js</span></span>

#### <a name="jquery-version-143"></a><span data-ttu-id="460ca-425">version de jQuery 1.4.3</span><span class="sxs-lookup"><span data-stu-id="460ca-425">jQuery version 1.4.3</span></span>

- <span data-ttu-id="460ca-426">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3.js</span><span class="sxs-lookup"><span data-stu-id="460ca-426">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js</span></span>
- <span data-ttu-id="460ca-427">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-427">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js</span></span>
- <span data-ttu-id="460ca-428">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="460ca-428">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js</span></span>

#### <a name="jquery-version-142"></a><span data-ttu-id="460ca-429">version de jQuery 1.4.2</span><span class="sxs-lookup"><span data-stu-id="460ca-429">jQuery version 1.4.2</span></span>

- <span data-ttu-id="460ca-430">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2.js</span><span class="sxs-lookup"><span data-stu-id="460ca-430">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js</span></span>
- <span data-ttu-id="460ca-431">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-431">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js</span></span>
- <span data-ttu-id="460ca-432">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="460ca-432">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js</span></span>

#### <a name="jquery-version-141"></a><span data-ttu-id="460ca-433">version de jQuery 1.4.1</span><span class="sxs-lookup"><span data-stu-id="460ca-433">jQuery version 1.4.1</span></span>

- <span data-ttu-id="460ca-434">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1.js</span><span class="sxs-lookup"><span data-stu-id="460ca-434">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js</span></span>
- <span data-ttu-id="460ca-435">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-435">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js</span></span>
- <span data-ttu-id="460ca-436">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="460ca-436">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js</span></span>

#### <a name="jquery-version-14"></a><span data-ttu-id="460ca-437">jQuery version 1.4</span><span class="sxs-lookup"><span data-stu-id="460ca-437">jQuery version 1.4</span></span>

- <span data-ttu-id="460ca-438">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.js</span><span class="sxs-lookup"><span data-stu-id="460ca-438">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js</span></span>
- <span data-ttu-id="460ca-439">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-439">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js</span></span>

#### <a name="jquery-version-132"></a><span data-ttu-id="460ca-440">version de jQuery 1.3.2</span><span class="sxs-lookup"><span data-stu-id="460ca-440">jQuery version 1.3.2</span></span>

- <span data-ttu-id="460ca-441">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.js</span><span class="sxs-lookup"><span data-stu-id="460ca-441">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js</span></span>
- <span data-ttu-id="460ca-442">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-442">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js</span></span>
- <span data-ttu-id="460ca-443">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="460ca-443">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js</span></span>
- <span data-ttu-id="460ca-444">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.min-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="460ca-444">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js</span></span>

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="460ca-445">jQuery migrer des versions sur le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-445">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="460ca-446">Les versions suivantes de jQuery migrer sont hébergées sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="460ca-446">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="460ca-447">jQuery migration version 3.0.0</span><span class="sxs-lookup"><span data-stu-id="460ca-447">jQuery Migrate version 3.0.0</span></span>

- <span data-ttu-id="460ca-448">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="460ca-448">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js</span></span>
- <span data-ttu-id="460ca-449">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-3.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-449">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js</span></span>

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="460ca-450">jQuery migration version 1.2.1</span><span class="sxs-lookup"><span data-stu-id="460ca-450">jQuery Migrate version 1.2.1</span></span>

- <span data-ttu-id="460ca-451">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.2.1.js</span><span class="sxs-lookup"><span data-stu-id="460ca-451">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js</span></span>
- <span data-ttu-id="460ca-452">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-452">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js</span></span>

<span data-ttu-id="460ca-453">jQuery migration version 1.2.0</span><span class="sxs-lookup"><span data-stu-id="460ca-453">jQuery Migrate version 1.2.0</span></span>

- <span data-ttu-id="460ca-454">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.2.0.js</span><span class="sxs-lookup"><span data-stu-id="460ca-454">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js</span></span>
- <span data-ttu-id="460ca-455">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-455">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js</span></span>

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="460ca-456">jQuery migration version 1.1.1</span><span class="sxs-lookup"><span data-stu-id="460ca-456">jQuery Migrate version 1.1.1</span></span>

- <span data-ttu-id="460ca-457">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.1.1.js</span><span class="sxs-lookup"><span data-stu-id="460ca-457">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js</span></span>
- <span data-ttu-id="460ca-458">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-458">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js</span></span>

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="460ca-459">jQuery migration version 1.1.0.</span><span class="sxs-lookup"><span data-stu-id="460ca-459">jQuery Migrate version 1.1.0</span></span>

- <span data-ttu-id="460ca-460">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.1.0.js</span><span class="sxs-lookup"><span data-stu-id="460ca-460">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js</span></span>
- <span data-ttu-id="460ca-461">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-461">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js</span></span>

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="460ca-462">Migrer la version 1.0.0 de jQuery</span><span class="sxs-lookup"><span data-stu-id="460ca-462">jQuery Migrate version 1.0.0</span></span>

- <span data-ttu-id="460ca-463">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.0.0.js</span><span class="sxs-lookup"><span data-stu-id="460ca-463">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js</span></span>
- <span data-ttu-id="460ca-464">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-464">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js</span></span>

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="460ca-465">jQuery UI mises en production sur le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-465">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="460ca-466">Les versions suivantes de la bibliothèque jQuery UI sont hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="460ca-466">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="460ca-467">Cliquez sur chaque lien pour afficher la liste réelle des fichiers.</span><span class="sxs-lookup"><span data-stu-id="460ca-467">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="460ca-468">jQuery UI 1.12.1</span><span class="sxs-lookup"><span data-stu-id="460ca-468">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery UI 1.12.1 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-469">jQuery UI 1.12.0</span><span class="sxs-lookup"><span data-stu-id="460ca-469">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "jQuery UI 1.12.0 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-470">jQuery UI 1.11.4</span><span class="sxs-lookup"><span data-stu-id="460ca-470">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "jQuery UI 1.11.4 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-471">jQuery UI 1.11.3</span><span class="sxs-lookup"><span data-stu-id="460ca-471">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery UI 1.11.3 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-472">jQuery UI 1.11.2</span><span class="sxs-lookup"><span data-stu-id="460ca-472">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery UI 1.11.2 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-473">jQuery UI 1.11.1</span><span class="sxs-lookup"><span data-stu-id="460ca-473">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "jQuery UI 1.11.1 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-474">jQuery UI 1.11.0</span><span class="sxs-lookup"><span data-stu-id="460ca-474">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "jQuery UI 1.11.0 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-475">jQuery UI 1.10.4</span><span class="sxs-lookup"><span data-stu-id="460ca-475">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery UI 1.10.4 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-476">jQuery UI 1.10.3</span><span class="sxs-lookup"><span data-stu-id="460ca-476">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery UI 1.10.3 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-477">jQuery UI 1.10.2</span><span class="sxs-lookup"><span data-stu-id="460ca-477">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery UI 1.10.2 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-478">jQuery UI 1.10.1</span><span class="sxs-lookup"><span data-stu-id="460ca-478">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery UI 1.10.1 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-479">jQuery UI 1.10.0</span><span class="sxs-lookup"><span data-stu-id="460ca-479">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery UI 1.10.0 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-480">jQuery UI 1.9.2</span><span class="sxs-lookup"><span data-stu-id="460ca-480">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "jQuery UI 1.9.2 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-481">jQuery UI 1.9.1</span><span class="sxs-lookup"><span data-stu-id="460ca-481">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "jQuery UI 1.9.1 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-482">jQuery UI à 1.9.0</span><span class="sxs-lookup"><span data-stu-id="460ca-482">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "jQuery UI à 1.9.0 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-483">jQuery UI 1.8.24</span><span class="sxs-lookup"><span data-stu-id="460ca-483">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery UI 1.8.24 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-484">jQuery UI 1.8.23</span><span class="sxs-lookup"><span data-stu-id="460ca-484">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery UI 1.8.23 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-485">jQuery UI 1.8.22</span><span class="sxs-lookup"><span data-stu-id="460ca-485">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery UI 1.8.22 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-486">jQuery UI 1.8.21</span><span class="sxs-lookup"><span data-stu-id="460ca-486">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery UI 1.8.21 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-487">jQuery UI 1.8.20</span><span class="sxs-lookup"><span data-stu-id="460ca-487">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery UI 1.8.20 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-488">jQuery UI 1.8.19</span><span class="sxs-lookup"><span data-stu-id="460ca-488">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery UI 1.8.19 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-489">jQuery UI 1.8.18</span><span class="sxs-lookup"><span data-stu-id="460ca-489">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery UI 1.8.18 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-490">jQuery UI 1.8.17</span><span class="sxs-lookup"><span data-stu-id="460ca-490">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery UI 1.8.17 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-491">jQuery UI 1.8.16</span><span class="sxs-lookup"><span data-stu-id="460ca-491">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery UI 1.8.16 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-492">jQuery UI 1.8.15</span><span class="sxs-lookup"><span data-stu-id="460ca-492">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery UI 1.8.15 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-493">jQuery UI 1.8.14</span><span class="sxs-lookup"><span data-stu-id="460ca-493">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery UI 1.8.14 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-494">jQuery UI 1.8.13</span><span class="sxs-lookup"><span data-stu-id="460ca-494">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery UI 1.8.13 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-495">jQuery UI 1.8.12</span><span class="sxs-lookup"><span data-stu-id="460ca-495">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery UI 1.8.12 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-496">jQuery UI 1.8.11</span><span class="sxs-lookup"><span data-stu-id="460ca-496">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery UI 1.8.11 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-497">jQuery UI 1.8.10</span><span class="sxs-lookup"><span data-stu-id="460ca-497">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-498">jQuery UI 1.8.9</span><span class="sxs-lookup"><span data-stu-id="460ca-498">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "jQuery UI 1.8.9 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-499">jQuery UI 1.8.8</span><span class="sxs-lookup"><span data-stu-id="460ca-499">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "jQuery UI 1.8.8 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-500">jQuery UI 1.8.7</span><span class="sxs-lookup"><span data-stu-id="460ca-500">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery UI 1.8.7 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-501">jQuery UI 1.8.6</span><span class="sxs-lookup"><span data-stu-id="460ca-501">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery UI 1.8.6 dans le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-502">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="460ca-502">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="460ca-503">jQuery Validation mises en production sur le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-503">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="460ca-504">Les versions suivantes de la bibliothèque de Validation jQuery sont hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="460ca-504">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="460ca-505">Cliquez sur chaque lien pour afficher la liste réelle des fichiers.</span><span class="sxs-lookup"><span data-stu-id="460ca-505">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="460ca-506">jQuery validation 1.17.0</span><span class="sxs-lookup"><span data-stu-id="460ca-506">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "jQuery Validation 1.17.0")
- [<span data-ttu-id="460ca-507">jQuery validation 1.16.0</span><span class="sxs-lookup"><span data-stu-id="460ca-507">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "jQuery Validation 1.16.0")
- [<span data-ttu-id="460ca-508">jQuery validation 1.15.1</span><span class="sxs-lookup"><span data-stu-id="460ca-508">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "jQuery Validation 1.15.1")
- [<span data-ttu-id="460ca-509">jQuery validation 1.15.0</span><span class="sxs-lookup"><span data-stu-id="460ca-509">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "jQuery Validation 1.15.0")
- [<span data-ttu-id="460ca-510">jQuery validation 1.14.0</span><span class="sxs-lookup"><span data-stu-id="460ca-510">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "jQuery Validation 1.14.0")
- [<span data-ttu-id="460ca-511">jQuery validation 1.13.1</span><span class="sxs-lookup"><span data-stu-id="460ca-511">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "jQuery Validation 1.13.1")
- [<span data-ttu-id="460ca-512">jQuery validation 1.13.0</span><span class="sxs-lookup"><span data-stu-id="460ca-512">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "jQuery Validation 1.13.0")
- [<span data-ttu-id="460ca-513">jQuery validation 1.12.0</span><span class="sxs-lookup"><span data-stu-id="460ca-513">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "jQuery Validation 1.12.0")
- [<span data-ttu-id="460ca-514">jQuery validation 1.11.1</span><span class="sxs-lookup"><span data-stu-id="460ca-514">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "jQuery Validation 1.11.1")
- [<span data-ttu-id="460ca-515">jQuery validation 1.11.0</span><span class="sxs-lookup"><span data-stu-id="460ca-515">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "jQuery Validation 1.11.0")
- [<span data-ttu-id="460ca-516">jQuery validation 1.10.0</span><span class="sxs-lookup"><span data-stu-id="460ca-516">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "jQuery Validation 1.10.0")
- [<span data-ttu-id="460ca-517">jQuery validation 1.9</span><span class="sxs-lookup"><span data-stu-id="460ca-517">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate version 1.9")
- [<span data-ttu-id="460ca-518">jQuery validation 1.8.1</span><span class="sxs-lookup"><span data-stu-id="460ca-518">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jquery.validate version 1.8.1")
- [<span data-ttu-id="460ca-519">jQuery validation 1.8</span><span class="sxs-lookup"><span data-stu-id="460ca-519">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate version 1.8")
- [<span data-ttu-id="460ca-520">jQuery validation 1.7</span><span class="sxs-lookup"><span data-stu-id="460ca-520">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate version 1.7")
- [<span data-ttu-id="460ca-521">jQuery validation 1.6</span><span class="sxs-lookup"><span data-stu-id="460ca-521">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery validation 1.6")
- [<span data-ttu-id="460ca-522">jQuery validation 1.5.5</span><span class="sxs-lookup"><span data-stu-id="460ca-522">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "jQuery validation 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="460ca-523">jQuery Mobile mises en production sur le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-523">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="460ca-524">Les versions suivantes de la bibliothèque de jQuery Mobile sont hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="460ca-524">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="460ca-525">Cliquez sur chaque lien pour afficher la liste réelle des fichiers.</span><span class="sxs-lookup"><span data-stu-id="460ca-525">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="460ca-526">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="460ca-526">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-527">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="460ca-527">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-528">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="460ca-528">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-529">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="460ca-529">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-530">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="460ca-530">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-531">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="460ca-531">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-532">jQuery Mobile version 1.3.0</span><span class="sxs-lookup"><span data-stu-id="460ca-532">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile version 1.3.0 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-533">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="460ca-533">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-534">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="460ca-534">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-535">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="460ca-535">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-536">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="460ca-536">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-537">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="460ca-537">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-538">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="460ca-538">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-539">jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="460ca-539">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-540">jQuery Mobile 1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="460ca-540">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-541">jQuery Mobile 1.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="460ca-541">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="460ca-542">version bêta 3 de jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="460ca-542">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 bêta 3 sur le CDN Microsoft Ajax")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="460ca-543">jQuery versions de modèles sur le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-543">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="460ca-544">Les versions suivantes du plug-in de modèles jQuery sont hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="460ca-544">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="460ca-545">Cliquez sur chaque lien pour afficher la liste réelle des fichiers.</span><span class="sxs-lookup"><span data-stu-id="460ca-545">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="460ca-546">jQuery modèles bêta 1</span><span class="sxs-lookup"><span data-stu-id="460ca-546">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery modèles bêta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="460ca-547">jQuery Cycle des versions sur le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-547">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="460ca-548">Les versions suivantes du plug-in du Cycle de jQuery sont hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="460ca-548">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="460ca-549">Cliquez sur chaque lien pour afficher la liste réelle des fichiers.</span><span class="sxs-lookup"><span data-stu-id="460ca-549">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="460ca-550">jQuery Cycle 2,99</span><span class="sxs-lookup"><span data-stu-id="460ca-550">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery 2,99 de Cycle")
- [<span data-ttu-id="460ca-551">jQuery Cycle 2.94</span><span class="sxs-lookup"><span data-stu-id="460ca-551">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery Cycle 2.94")
- [<span data-ttu-id="460ca-552">jQuery Cycle 2,88</span><span class="sxs-lookup"><span data-stu-id="460ca-552">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery 2,88 de Cycle")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="460ca-553">jQuery versions de tables de données sur le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-553">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="460ca-554">Les versions suivantes du plug-in DataTables de jQuery sont hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="460ca-554">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="460ca-555">Cliquez sur chaque lien pour afficher la liste réelle des fichiers.</span><span class="sxs-lookup"><span data-stu-id="460ca-555">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="460ca-556">jQuery DataTables 1.10.5</span><span class="sxs-lookup"><span data-stu-id="460ca-556">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [<span data-ttu-id="460ca-557">jQuery DataTables 1.10.4</span><span class="sxs-lookup"><span data-stu-id="460ca-557">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [<span data-ttu-id="460ca-558">jQuery DataTables 1.9.4</span><span class="sxs-lookup"><span data-stu-id="460ca-558">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables 1.9.4")
- [<span data-ttu-id="460ca-559">jQuery DataTables 1.9.3</span><span class="sxs-lookup"><span data-stu-id="460ca-559">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [<span data-ttu-id="460ca-560">jQuery DataTables 1.9.2</span><span class="sxs-lookup"><span data-stu-id="460ca-560">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [<span data-ttu-id="460ca-561">jQuery DataTables 1.9.1</span><span class="sxs-lookup"><span data-stu-id="460ca-561">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [<span data-ttu-id="460ca-562">jQuery DataTables à 1.9.0</span><span class="sxs-lookup"><span data-stu-id="460ca-562">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables à 1.9.0")
- [<span data-ttu-id="460ca-563">jQuery DataTables 1.8.2</span><span class="sxs-lookup"><span data-stu-id="460ca-563">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="460ca-564">Versions de Modernizr sur le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-564">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="460ca-565">Les versions suivantes de [Modernizr](http://www.modernizr.com "Modernizr") sont hébergés sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="460ca-565">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- <span data-ttu-id="460ca-566">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.8.3.js</span><span class="sxs-lookup"><span data-stu-id="460ca-566">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js</span></span>
- <span data-ttu-id="460ca-567">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.7.2.js</span><span class="sxs-lookup"><span data-stu-id="460ca-567">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js</span></span>
- <span data-ttu-id="460ca-568">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.7.1.js</span><span class="sxs-lookup"><span data-stu-id="460ca-568">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js</span></span>
- <span data-ttu-id="460ca-569">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.6.2.js</span><span class="sxs-lookup"><span data-stu-id="460ca-569">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js</span></span>
- <span data-ttu-id="460ca-570">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-1.7-Development-only.js</span><span class="sxs-lookup"><span data-stu-id="460ca-570">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js</span></span>
- <span data-ttu-id="460ca-571">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.0.6-Development-only.js</span><span class="sxs-lookup"><span data-stu-id="460ca-571">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js</span></span>

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="460ca-572">Versions de JSHint sur le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-572">JSHint Releases on the CDN</span></span>

<span data-ttu-id="460ca-573">Les versions suivantes de [JSHint](http://www.jshint.com "JSHint") sont hébergés sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="460ca-573">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- <span data-ttu-id="460ca-574">http://AJAX.aspnetcdn.com/AJAX/jshint/r07/jshint.js</span><span class="sxs-lookup"><span data-stu-id="460ca-574">http://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js</span></span>

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="460ca-575">Versions de Knockout sur le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-575">Knockout Releases on the CDN</span></span>

<span data-ttu-id="460ca-576">Les versions suivantes de [Knockout](http://www.knockoutjs.com "Knockout") sont hébergés sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="460ca-576">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- <span data-ttu-id="460ca-577">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="460ca-577">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js</span></span>
- <span data-ttu-id="460ca-578">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.2.1.Debug.js</span><span class="sxs-lookup"><span data-stu-id="460ca-578">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js</span></span>
- <span data-ttu-id="460ca-579">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="460ca-579">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js</span></span>
- <span data-ttu-id="460ca-580">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.2.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="460ca-580">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js</span></span>
- <span data-ttu-id="460ca-581">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="460ca-581">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js</span></span>
- <span data-ttu-id="460ca-582">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.1.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="460ca-582">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js</span></span>
- <span data-ttu-id="460ca-583">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="460ca-583">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js</span></span>
- <span data-ttu-id="460ca-584">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.0.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="460ca-584">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js</span></span>
- <span data-ttu-id="460ca-585">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.1.0.js</span><span class="sxs-lookup"><span data-stu-id="460ca-585">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js</span></span>
- <span data-ttu-id="460ca-586">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.1.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="460ca-586">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js</span></span>
- <span data-ttu-id="460ca-587">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.2.0.js</span><span class="sxs-lookup"><span data-stu-id="460ca-587">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js</span></span>
- <span data-ttu-id="460ca-588">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.2.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="460ca-588">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js</span></span>
- <span data-ttu-id="460ca-589">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.3.0.js</span><span class="sxs-lookup"><span data-stu-id="460ca-589">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js</span></span>
- <span data-ttu-id="460ca-590">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.3.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="460ca-590">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js</span></span>
- <span data-ttu-id="460ca-591">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.0.js</span><span class="sxs-lookup"><span data-stu-id="460ca-591">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js</span></span>
- <span data-ttu-id="460ca-592">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="460ca-592">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js</span></span>
- <span data-ttu-id="460ca-593">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.1.js</span><span class="sxs-lookup"><span data-stu-id="460ca-593">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js</span></span>
- <span data-ttu-id="460ca-594">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.1.Debug.js</span><span class="sxs-lookup"><span data-stu-id="460ca-594">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js</span></span>
- <span data-ttu-id="460ca-595">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.2.js</span><span class="sxs-lookup"><span data-stu-id="460ca-595">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js</span></span>
- <span data-ttu-id="460ca-596">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.2.Debug.js</span><span class="sxs-lookup"><span data-stu-id="460ca-596">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js</span></span>

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="460ca-597">Globaliser des versions sur le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-597">Globalize Releases on the CDN</span></span>

<span data-ttu-id="460ca-598">Les versions suivantes de [Globalize](https://github.com/jquery/globalize "Globalize") sont hébergés sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="460ca-598">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="460ca-599">La version 1.0.0 de globalisation</span><span class="sxs-lookup"><span data-stu-id="460ca-599">Globalize version 1.0.0</span></span>

- <span data-ttu-id="460ca-600">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize.js</span><span class="sxs-lookup"><span data-stu-id="460ca-600">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js</span></span>
- <span data-ttu-id="460ca-601">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/Node-main.js</span><span class="sxs-lookup"><span data-stu-id="460ca-601">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js</span></span>
- <span data-ttu-id="460ca-602">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Currency.js</span><span class="sxs-lookup"><span data-stu-id="460ca-602">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js</span></span>
- <span data-ttu-id="460ca-603">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/date.js</span><span class="sxs-lookup"><span data-stu-id="460ca-603">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js</span></span>
- <span data-ttu-id="460ca-604">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/message.js</span><span class="sxs-lookup"><span data-stu-id="460ca-604">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js</span></span>
- <span data-ttu-id="460ca-605">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Number.js</span><span class="sxs-lookup"><span data-stu-id="460ca-605">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js</span></span>
- <span data-ttu-id="460ca-606">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/plural.js</span><span class="sxs-lookup"><span data-stu-id="460ca-606">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js</span></span>
- <span data-ttu-id="460ca-607">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/relative-Time.js</span><span class="sxs-lookup"><span data-stu-id="460ca-607">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js</span></span>

#### <a name="globalize-version-011"></a><span data-ttu-id="460ca-608">Globaliser version 0.1.1</span><span class="sxs-lookup"><span data-stu-id="460ca-608">Globalize version 0.1.1</span></span>

- <span data-ttu-id="460ca-609">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/globalize.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-609">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js</span></span>
- <span data-ttu-id="460ca-610">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/globalize.js</span><span class="sxs-lookup"><span data-stu-id="460ca-610">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js</span></span>
- <span data-ttu-id="460ca-611">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/cultures/globalize.cultures.js</span><span class="sxs-lookup"><span data-stu-id="460ca-611">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js</span></span>

    - <span data-ttu-id="460ca-612">toutes les cultures</span><span class="sxs-lookup"><span data-stu-id="460ca-612">all cultures</span></span>
- <span data-ttu-id="460ca-613">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/cultures/globalize.culture. {code de culture} .js</span><span class="sxs-lookup"><span data-stu-id="460ca-613">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js</span></span>

    - <span data-ttu-id="460ca-614">Remplacez « {-code de culture} » avec le code de culture de votre choix, par exemple, les fichiers sur le CDN globalize.culture.en-GB.js== Microsoft == ces bibliothèques ont été téléchargés par Microsoft.</span><span class="sxs-lookup"><span data-stu-id="460ca-614">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="460ca-615">Répondre versions sur le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-615">Respond Releases on the CDN</span></span>

<span data-ttu-id="460ca-616">Les versions suivantes de [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") répondre sont hébergés sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="460ca-616">The following releases of [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") Respond are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="460ca-617">Répondre version 1.4.2</span><span class="sxs-lookup"><span data-stu-id="460ca-617">Respond version 1.4.2</span></span>

- <span data-ttu-id="460ca-618">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.js</span><span class="sxs-lookup"><span data-stu-id="460ca-618">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js</span></span>
- <span data-ttu-id="460ca-619">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-619">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js</span></span>
- <span data-ttu-id="460ca-620">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="460ca-620">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="460ca-621">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-621">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-141"></a><span data-ttu-id="460ca-622">Version 1.4.1 de répondre</span><span class="sxs-lookup"><span data-stu-id="460ca-622">Respond version 1.4.1</span></span>

- <span data-ttu-id="460ca-623">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.js</span><span class="sxs-lookup"><span data-stu-id="460ca-623">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js</span></span>
- <span data-ttu-id="460ca-624">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-624">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js</span></span>
- <span data-ttu-id="460ca-625">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="460ca-625">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="460ca-626">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-626">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-140"></a><span data-ttu-id="460ca-627">Répondre version1.4.0</span><span class="sxs-lookup"><span data-stu-id="460ca-627">Respond version 1.4.0</span></span>

- <span data-ttu-id="460ca-628">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.js</span><span class="sxs-lookup"><span data-stu-id="460ca-628">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js</span></span>
- <span data-ttu-id="460ca-629">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-629">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js</span></span>
- <span data-ttu-id="460ca-630">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="460ca-630">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="460ca-631">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-631">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-130"></a><span data-ttu-id="460ca-632">Répondre version version 1.3.0</span><span class="sxs-lookup"><span data-stu-id="460ca-632">Respond version 1.3.0</span></span>

- <span data-ttu-id="460ca-633">http://AJAX.aspnetcdn.com/AJAX/Respond/1.3.0/Respond.js</span><span class="sxs-lookup"><span data-stu-id="460ca-633">http://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js</span></span>

#### <a name="respond-version-120"></a><span data-ttu-id="460ca-634">Répondre version 1.2.0</span><span class="sxs-lookup"><span data-stu-id="460ca-634">Respond version 1.2.0</span></span>

- <span data-ttu-id="460ca-635">http://AJAX.aspnetcdn.com/AJAX/Respond/1.2.0/Respond.js</span><span class="sxs-lookup"><span data-stu-id="460ca-635">http://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js</span></span>

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="460ca-636">Versions d’amorçage sur le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-636">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="460ca-637">Les versions suivantes de [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap sont hébergés sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="460ca-637">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-337"></a><span data-ttu-id="460ca-638">Version d’amorçage 3.3.7</span><span class="sxs-lookup"><span data-stu-id="460ca-638">Bootstrap version 3.3.7</span></span>

- <span data-ttu-id="460ca-639">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="460ca-639">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js</span></span>
- <span data-ttu-id="460ca-640">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-640">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js</span></span>
- <span data-ttu-id="460ca-641">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-641">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css</span></span>
- <span data-ttu-id="460ca-642">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-642">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map</span></span>
- <span data-ttu-id="460ca-643">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-643">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css</span></span>
- <span data-ttu-id="460ca-644">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-644">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="460ca-645">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-645">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="460ca-646">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-646">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="460ca-647">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="460ca-647">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="460ca-648">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="460ca-648">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="460ca-649">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="460ca-649">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="460ca-650">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="460ca-650">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="460ca-651">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="460ca-651">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-336"></a><span data-ttu-id="460ca-652">Version d’amorçage 3.3.6</span><span class="sxs-lookup"><span data-stu-id="460ca-652">Bootstrap version 3.3.6</span></span>

- <span data-ttu-id="460ca-653">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="460ca-653">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js</span></span>
- <span data-ttu-id="460ca-654">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-654">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js</span></span>
- <span data-ttu-id="460ca-655">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-655">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css</span></span>
- <span data-ttu-id="460ca-656">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-656">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map</span></span>
- <span data-ttu-id="460ca-657">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-657">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css</span></span>
- <span data-ttu-id="460ca-658">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-658">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="460ca-659">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-659">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="460ca-660">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-660">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="460ca-661">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="460ca-661">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="460ca-662">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="460ca-662">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="460ca-663">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="460ca-663">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="460ca-664">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="460ca-664">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="460ca-665">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="460ca-665">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-335"></a><span data-ttu-id="460ca-666">Amorçage version 3.3.5</span><span class="sxs-lookup"><span data-stu-id="460ca-666">Bootstrap version 3.3.5</span></span>

- <span data-ttu-id="460ca-667">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="460ca-667">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js</span></span>
- <span data-ttu-id="460ca-668">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-668">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js</span></span>
- <span data-ttu-id="460ca-669">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-669">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css</span></span>
- <span data-ttu-id="460ca-670">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-670">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map</span></span>
- <span data-ttu-id="460ca-671">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-671">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css</span></span>
- <span data-ttu-id="460ca-672">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-672">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="460ca-673">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-673">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="460ca-674">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-674">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="460ca-675">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="460ca-675">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="460ca-676">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="460ca-676">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="460ca-677">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="460ca-677">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="460ca-678">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="460ca-678">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="460ca-679">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="460ca-679">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-334"></a><span data-ttu-id="460ca-680">Version d’amorçage 3.3.4</span><span class="sxs-lookup"><span data-stu-id="460ca-680">Bootstrap version 3.3.4</span></span>

- <span data-ttu-id="460ca-681">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="460ca-681">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js</span></span>
- <span data-ttu-id="460ca-682">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-682">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js</span></span>
- <span data-ttu-id="460ca-683">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-683">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css</span></span>
- <span data-ttu-id="460ca-684">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-684">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map</span></span>
- <span data-ttu-id="460ca-685">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-685">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css</span></span>
- <span data-ttu-id="460ca-686">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-686">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="460ca-687">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-687">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="460ca-688">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-688">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="460ca-689">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="460ca-689">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="460ca-690">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="460ca-690">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="460ca-691">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="460ca-691">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="460ca-692">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="460ca-692">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="460ca-693">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="460ca-693">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-332"></a><span data-ttu-id="460ca-694">Version d’amorçage 3.3.2</span><span class="sxs-lookup"><span data-stu-id="460ca-694">Bootstrap version 3.3.2</span></span>

- <span data-ttu-id="460ca-695">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="460ca-695">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js</span></span>
- <span data-ttu-id="460ca-696">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-696">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js</span></span>
- <span data-ttu-id="460ca-697">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-697">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css</span></span>
- <span data-ttu-id="460ca-698">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-698">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map</span></span>
- <span data-ttu-id="460ca-699">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-699">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="460ca-700">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-700">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="460ca-701">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-701">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="460ca-702">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-702">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="460ca-703">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="460ca-703">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="460ca-704">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="460ca-704">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="460ca-705">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="460ca-705">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="460ca-706">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="460ca-706">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="460ca-707">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="460ca-707">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-331"></a><span data-ttu-id="460ca-708">Version d’amorçage 3.3.1</span><span class="sxs-lookup"><span data-stu-id="460ca-708">Bootstrap version 3.3.1</span></span>

- <span data-ttu-id="460ca-709">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="460ca-709">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js</span></span>
- <span data-ttu-id="460ca-710">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-710">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js</span></span>
- <span data-ttu-id="460ca-711">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-711">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css</span></span>
- <span data-ttu-id="460ca-712">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-712">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map</span></span>
- <span data-ttu-id="460ca-713">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-713">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="460ca-714">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-714">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="460ca-715">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-715">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="460ca-716">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-716">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="460ca-717">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="460ca-717">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="460ca-718">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="460ca-718">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="460ca-719">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="460ca-719">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="460ca-720">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="460ca-720">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-330"></a><span data-ttu-id="460ca-721">Version d’amorçage 3.3.0</span><span class="sxs-lookup"><span data-stu-id="460ca-721">Bootstrap version 3.3.0</span></span>

- <span data-ttu-id="460ca-722">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="460ca-722">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js</span></span>
- <span data-ttu-id="460ca-723">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-723">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js</span></span>
- <span data-ttu-id="460ca-724">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-724">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css</span></span>
- <span data-ttu-id="460ca-725">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-725">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="460ca-726">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-726">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="460ca-727">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-727">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="460ca-728">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-728">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="460ca-729">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-729">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="460ca-730">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="460ca-730">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="460ca-731">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="460ca-731">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="460ca-732">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="460ca-732">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="460ca-733">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="460ca-733">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-320"></a><span data-ttu-id="460ca-734">Version d’amorçage 3.2.0</span><span class="sxs-lookup"><span data-stu-id="460ca-734">Bootstrap version 3.2.0</span></span>

- <span data-ttu-id="460ca-735">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="460ca-735">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js</span></span>
- <span data-ttu-id="460ca-736">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-736">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js</span></span>
- <span data-ttu-id="460ca-737">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-737">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css</span></span>
- <span data-ttu-id="460ca-738">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-738">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="460ca-739">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-739">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="460ca-740">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-740">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="460ca-741">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-741">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="460ca-742">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-742">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="460ca-743">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="460ca-743">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="460ca-744">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="460ca-744">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="460ca-745">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="460ca-745">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="460ca-746">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="460ca-746">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-311"></a><span data-ttu-id="460ca-747">Version d’amorçage 3.1.1</span><span class="sxs-lookup"><span data-stu-id="460ca-747">Bootstrap version 3.1.1</span></span>

- <span data-ttu-id="460ca-748">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="460ca-748">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js</span></span>
- <span data-ttu-id="460ca-749">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-749">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js</span></span>
- <span data-ttu-id="460ca-750">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-750">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css</span></span>
- <span data-ttu-id="460ca-751">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-751">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map</span></span>
- <span data-ttu-id="460ca-752">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-752">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="460ca-753">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-753">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="460ca-754">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-754">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="460ca-755">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-755">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="460ca-756">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="460ca-756">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="460ca-757">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="460ca-757">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="460ca-758">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="460ca-758">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="460ca-759">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="460ca-759">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-310"></a><span data-ttu-id="460ca-760">Amorçage version 3.1.0</span><span class="sxs-lookup"><span data-stu-id="460ca-760">Bootstrap version 3.1.0</span></span>

- <span data-ttu-id="460ca-761">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="460ca-761">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js</span></span>
- <span data-ttu-id="460ca-762">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-762">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js</span></span>
- <span data-ttu-id="460ca-763">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-763">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css</span></span>
- <span data-ttu-id="460ca-764">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-764">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="460ca-765">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-765">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="460ca-766">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-766">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="460ca-767">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-767">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="460ca-768">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-768">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="460ca-769">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="460ca-769">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="460ca-770">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="460ca-770">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="460ca-771">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="460ca-771">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="460ca-772">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="460ca-772">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-303"></a><span data-ttu-id="460ca-773">Version d’amorçage 3.0.3</span><span class="sxs-lookup"><span data-stu-id="460ca-773">Bootstrap version 3.0.3</span></span>

- <span data-ttu-id="460ca-774">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="460ca-774">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js</span></span>
- <span data-ttu-id="460ca-775">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-775">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js</span></span>
- <span data-ttu-id="460ca-776">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-776">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css</span></span>
- <span data-ttu-id="460ca-777">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-777">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css</span></span>
- <span data-ttu-id="460ca-778">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-778">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="460ca-779">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-779">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="460ca-780">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="460ca-780">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="460ca-781">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="460ca-781">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="460ca-782">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="460ca-782">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="460ca-783">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="460ca-783">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-302"></a><span data-ttu-id="460ca-784">Version d’amorçage 3.0.2</span><span class="sxs-lookup"><span data-stu-id="460ca-784">Bootstrap version 3.0.2</span></span>

- <span data-ttu-id="460ca-785">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="460ca-785">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js</span></span>
- <span data-ttu-id="460ca-786">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-786">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js</span></span>
- <span data-ttu-id="460ca-787">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-787">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css</span></span>
- <span data-ttu-id="460ca-788">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-788">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="460ca-789">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-789">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="460ca-790">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-790">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="460ca-791">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="460ca-791">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="460ca-792">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="460ca-792">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="460ca-793">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="460ca-793">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="460ca-794">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="460ca-794">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-301"></a><span data-ttu-id="460ca-795">Version d’amorçage 3.0.1</span><span class="sxs-lookup"><span data-stu-id="460ca-795">Bootstrap version 3.0.1</span></span>

- <span data-ttu-id="460ca-796">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="460ca-796">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js</span></span>
- <span data-ttu-id="460ca-797">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-797">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js</span></span>
- <span data-ttu-id="460ca-798">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-798">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css</span></span>
- <span data-ttu-id="460ca-799">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-799">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="460ca-800">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-800">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="460ca-801">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-801">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="460ca-802">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="460ca-802">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="460ca-803">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="460ca-803">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="460ca-804">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="460ca-804">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="460ca-805">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="460ca-805">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-300"></a><span data-ttu-id="460ca-806">Version d’amorçage 3.0.0</span><span class="sxs-lookup"><span data-stu-id="460ca-806">Bootstrap version 3.0.0</span></span>

- <span data-ttu-id="460ca-807">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="460ca-807">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js</span></span>
- <span data-ttu-id="460ca-808">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-808">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js</span></span>
- <span data-ttu-id="460ca-809">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-809">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css</span></span>
- <span data-ttu-id="460ca-810">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-810">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="460ca-811">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-811">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="460ca-812">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-812">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="460ca-813">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="460ca-813">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="460ca-814">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="460ca-814">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="460ca-815">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="460ca-815">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="460ca-816">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="460ca-816">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-232"></a><span data-ttu-id="460ca-817">Amorçage version 2.3.2</span><span class="sxs-lookup"><span data-stu-id="460ca-817">Bootstrap version 2.3.2</span></span>

- <span data-ttu-id="460ca-818">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="460ca-818">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js</span></span>
- <span data-ttu-id="460ca-819">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-819">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js</span></span>
- <span data-ttu-id="460ca-820">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-820">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css</span></span>
- <span data-ttu-id="460ca-821">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-821">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="460ca-822">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap-Responsive.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-822">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css</span></span>
- <span data-ttu-id="460ca-823">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap-Responsive.min.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-823">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css</span></span>
- <span data-ttu-id="460ca-824">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/img/glyphicons-halflings.png</span><span class="sxs-lookup"><span data-stu-id="460ca-824">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png</span></span>
- <span data-ttu-id="460ca-825">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/img/glyphicons-halflings-White.png</span><span class="sxs-lookup"><span data-stu-id="460ca-825">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png</span></span>

#### <a name="bootstrap-version-231"></a><span data-ttu-id="460ca-826">Amorçage version 2.3.1</span><span class="sxs-lookup"><span data-stu-id="460ca-826">Bootstrap version 2.3.1</span></span>

- <span data-ttu-id="460ca-827">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="460ca-827">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js</span></span>
- <span data-ttu-id="460ca-828">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-828">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js</span></span>
- <span data-ttu-id="460ca-829">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-829">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css</span></span>
- <span data-ttu-id="460ca-830">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-830">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="460ca-831">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap-Responsive.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-831">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css</span></span>
- <span data-ttu-id="460ca-832">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap-Responsive.min.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-832">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css</span></span>
- <span data-ttu-id="460ca-833">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/img/glyphicons-halflings.png</span><span class="sxs-lookup"><span data-stu-id="460ca-833">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png</span></span>
- <span data-ttu-id="460ca-834">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/img/glyphicons-halflings-White.png</span><span class="sxs-lookup"><span data-stu-id="460ca-834">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png</span></span>

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="460ca-835">Versions TouchCarousel d’amorçage sur le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-835">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="460ca-836">Les versions suivantes de [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") TouchCarousel du programme d’amorçage versions sont hébergées sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="460ca-836">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="460ca-837">Amorçage TouchCarousel version 0.8.0</span><span class="sxs-lookup"><span data-stu-id="460ca-837">Bootstrap TouchCarousel version 0.8.0</span></span>

- <span data-ttu-id="460ca-838">http://AJAX.aspnetcdn.com/AJAX/Bootstrap-Touch-Carousel/0.8.0/CSS/Bootstrap-Touch-Carousel.CSS</span><span class="sxs-lookup"><span data-stu-id="460ca-838">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css</span></span>
- <span data-ttu-id="460ca-839">http://AJAX.aspnetcdn.com/AJAX/Bootstrap-Touch-Carousel/0.8.0/js/Bootstrap-Touch-Carousel.js</span><span class="sxs-lookup"><span data-stu-id="460ca-839">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js</span></span>

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="460ca-840">Versions de Hammer.js sur le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-840">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="460ca-841">Les versions suivantes de [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js versions sont hébergées sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="460ca-841">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="460ca-842">Hammer.js version 2.0.4</span><span class="sxs-lookup"><span data-stu-id="460ca-842">Hammer.js version 2.0.4</span></span>

- <span data-ttu-id="460ca-843">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.js</span><span class="sxs-lookup"><span data-stu-id="460ca-843">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js</span></span>
- <span data-ttu-id="460ca-844">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-844">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js</span></span>
- <span data-ttu-id="460ca-845">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.min.Map</span><span class="sxs-lookup"><span data-stu-id="460ca-845">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map</span></span>

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="460ca-846">Web Forms ASP.NET et Ajax mises en production sur le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-846">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="460ca-847">Les versions suivantes de la bibliothèque ASP.NET Ajax sont hébergées sur le CDN.</span><span class="sxs-lookup"><span data-stu-id="460ca-847">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="460ca-848">Cliquez sur chaque lien pour afficher la liste réelle des fichiers.</span><span class="sxs-lookup"><span data-stu-id="460ca-848">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="460ca-849">Version de Web Forms ASP.NET et Ajax 4.5.2</span><span class="sxs-lookup"><span data-stu-id="460ca-849">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "Web Forms ASP.NET et Ajax 4.5.2")
- [<span data-ttu-id="460ca-850">Version de Web Forms ASP.NET et Ajax 4</span><span class="sxs-lookup"><span data-stu-id="460ca-850">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "Web Forms ASP.NET et Ajax 4")
- [<span data-ttu-id="460ca-851">ASP.NET Ajax version 3.5</span><span class="sxs-lookup"><span data-stu-id="460ca-851">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "ASP.NET Ajax 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="460ca-852">ASP.NET MVC publie le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-852">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="460ca-853">Les fichiers ASP.NET MVC JavaScript suivants sont hébergés sur ce CDN :</span><span class="sxs-lookup"><span data-stu-id="460ca-853">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="460ca-854">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="460ca-854">ASP.NET MVC 5.2.3</span></span>

- <span data-ttu-id="460ca-855">http://AJAX.aspnetcdn.com/AJAX/MVC/5.2.3/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="460ca-855">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="460ca-856">http://AJAX.aspnetcdn.com/AJAX/MVC/5.2.3/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-856">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="460ca-857">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="460ca-857">ASP.NET MVC 5.1</span></span>

- <span data-ttu-id="460ca-858">http://AJAX.aspnetcdn.com/AJAX/MVC/5.1/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="460ca-858">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="460ca-859">http://AJAX.aspnetcdn.com/AJAX/MVC/5.1/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-859">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="460ca-860">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="460ca-860">ASP.NET MVC 5.0</span></span>

- <span data-ttu-id="460ca-861">http://AJAX.aspnetcdn.com/AJAX/MVC/5.0/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="460ca-861">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="460ca-862">http://AJAX.aspnetcdn.com/AJAX/MVC/5.0/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-862">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="460ca-863">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="460ca-863">ASP.NET MVC 4.0</span></span>

- <span data-ttu-id="460ca-864">http://AJAX.aspnetcdn.com/AJAX/MVC/4.0/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="460ca-864">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="460ca-865">http://AJAX.aspnetcdn.com/AJAX/MVC/4.0/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-865">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="460ca-866">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="460ca-866">ASP.NET MVC 3.0</span></span>

- <span data-ttu-id="460ca-867">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.unobtrusive-AJAX.js</span><span class="sxs-lookup"><span data-stu-id="460ca-867">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js</span></span>
- <span data-ttu-id="460ca-868">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.unobtrusive-AJAX.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-868">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js</span></span>
- <span data-ttu-id="460ca-869">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="460ca-869">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="460ca-870">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-870">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js</span></span>
- <span data-ttu-id="460ca-871">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="460ca-871">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="460ca-872">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="460ca-872">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js</span></span>

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="460ca-873">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="460ca-873">ASP.NET MVC 2.0</span></span>

- <span data-ttu-id="460ca-874">http://AJAX.aspnetcdn.com/AJAX/MVC/2.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="460ca-874">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="460ca-875">http://AJAX.aspnetcdn.com/AJAX/MVC/2.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="460ca-875">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js</span></span>

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="460ca-876">ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="460ca-876">ASP.NET MVC 1.0</span></span>

- <span data-ttu-id="460ca-877">http://AJAX.aspnetcdn.com/AJAX/MVC/1.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="460ca-877">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="460ca-878">http://AJAX.aspnetcdn.com/AJAX/MVC/1.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="460ca-878">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js</span></span>

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="460ca-879">ASP.NET SignalR publie le CDN</span><span class="sxs-lookup"><span data-stu-id="460ca-879">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="460ca-880">Les fichiers ASP.NET SignalR JavaScript suivants sont hébergés sur ce CDN :</span><span class="sxs-lookup"><span data-stu-id="460ca-880">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="460ca-881">ASP.NET SignalR 2.2.2</span><span class="sxs-lookup"><span data-stu-id="460ca-881">ASP.NET SignalR 2.2.2</span></span>

- <span data-ttu-id="460ca-882">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.2.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-882">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js</span></span>
- <span data-ttu-id="460ca-883">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.2.js</span><span class="sxs-lookup"><span data-stu-id="460ca-883">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js</span></span>

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="460ca-884">ASP.NET SignalR 2.2.1</span><span class="sxs-lookup"><span data-stu-id="460ca-884">ASP.NET SignalR 2.2.1</span></span>

- <span data-ttu-id="460ca-885">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-885">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js</span></span>
- <span data-ttu-id="460ca-886">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="460ca-886">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js</span></span>

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="460ca-887">ASP.NET SignalR 2.2.0</span><span class="sxs-lookup"><span data-stu-id="460ca-887">ASP.NET SignalR 2.2.0</span></span>

- <span data-ttu-id="460ca-888">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-888">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js</span></span>
- <span data-ttu-id="460ca-889">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="460ca-889">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js</span></span>

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="460ca-890">ASP.NET SignalR 2.1.0</span><span class="sxs-lookup"><span data-stu-id="460ca-890">ASP.NET SignalR 2.1.0</span></span>

- <span data-ttu-id="460ca-891">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-891">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js</span></span>
- <span data-ttu-id="460ca-892">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="460ca-892">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js</span></span>

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="460ca-893">ASP.NET SignalR 2.0.3</span><span class="sxs-lookup"><span data-stu-id="460ca-893">ASP.NET SignalR 2.0.3</span></span>

- <span data-ttu-id="460ca-894">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.3.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-894">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js</span></span>
- <span data-ttu-id="460ca-895">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.3.js</span><span class="sxs-lookup"><span data-stu-id="460ca-895">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js</span></span>

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="460ca-896">ASP.NET SignalR 2.0.2</span><span class="sxs-lookup"><span data-stu-id="460ca-896">ASP.NET SignalR 2.0.2</span></span>

- <span data-ttu-id="460ca-897">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.2.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-897">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js</span></span>
- <span data-ttu-id="460ca-898">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.2.js</span><span class="sxs-lookup"><span data-stu-id="460ca-898">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js</span></span>

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="460ca-899">ASP.NET SignalR 2.0.1</span><span class="sxs-lookup"><span data-stu-id="460ca-899">ASP.NET SignalR 2.0.1</span></span>

- <span data-ttu-id="460ca-900">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-900">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js</span></span>
- <span data-ttu-id="460ca-901">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.1.js</span><span class="sxs-lookup"><span data-stu-id="460ca-901">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js</span></span>

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="460ca-902">ASP.NET SignalR 2.0.0</span><span class="sxs-lookup"><span data-stu-id="460ca-902">ASP.NET SignalR 2.0.0</span></span>

- <span data-ttu-id="460ca-903">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-903">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js</span></span>
- <span data-ttu-id="460ca-904">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.0.js</span><span class="sxs-lookup"><span data-stu-id="460ca-904">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js</span></span>

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="460ca-905">ASP.NET SignalR 1.1.3</span><span class="sxs-lookup"><span data-stu-id="460ca-905">ASP.NET SignalR 1.1.3</span></span>

- <span data-ttu-id="460ca-906">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.3.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-906">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js</span></span>
- <span data-ttu-id="460ca-907">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.3.js</span><span class="sxs-lookup"><span data-stu-id="460ca-907">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js</span></span>

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="460ca-908">ASP.NET SignalR 1.1.2</span><span class="sxs-lookup"><span data-stu-id="460ca-908">ASP.NET SignalR 1.1.2</span></span>

- <span data-ttu-id="460ca-909">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.2.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-909">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js</span></span>
- <span data-ttu-id="460ca-910">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.2.js</span><span class="sxs-lookup"><span data-stu-id="460ca-910">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js</span></span>

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="460ca-911">ASP.NET SignalR 1.1.1</span><span class="sxs-lookup"><span data-stu-id="460ca-911">ASP.NET SignalR 1.1.1</span></span>

- <span data-ttu-id="460ca-912">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-912">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js</span></span>
- <span data-ttu-id="460ca-913">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.1.js</span><span class="sxs-lookup"><span data-stu-id="460ca-913">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js</span></span>

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="460ca-914">ASP.NET SignalR 1.1.0</span><span class="sxs-lookup"><span data-stu-id="460ca-914">ASP.NET SignalR 1.1.0</span></span>

- <span data-ttu-id="460ca-915">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-915">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js</span></span>
- <span data-ttu-id="460ca-916">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.0.js</span><span class="sxs-lookup"><span data-stu-id="460ca-916">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js</span></span>

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="460ca-917">ASP.NET SignalR 1.0.1</span><span class="sxs-lookup"><span data-stu-id="460ca-917">ASP.NET SignalR 1.0.1</span></span>

- <span data-ttu-id="460ca-918">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="460ca-918">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js</span></span>
- <span data-ttu-id="460ca-919">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.0.1.js</span><span class="sxs-lookup"><span data-stu-id="460ca-919">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js</span></span>

<span data-ttu-id="460ca-920">Pour plus d’informations sur les conditions d’utilisation pour le CDN, consultez [Microsoft Ajax CDN conditions d’utilisation](https://www.asp.net/terms-of-use "Microsoft Ajax CDN conditions d’utilisation").</span><span class="sxs-lookup"><span data-stu-id="460ca-920">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
