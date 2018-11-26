---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Helper Twitter avec ASP.NET Web Pages | Microsoft Docs
author: Rick-Anderson
description: Cette rubrique et l’application montrent comment ajouter une application auxiliaire Twitter à votre projet WebMatrix 3. Il contient le code de l’application auxiliaire Twitter et montre comment appeler l’assistance...
ms.author: riande
ms.date: 11/26/2018
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: fabe9f2b84d278a766dc8d8b7bfc00e1eb967127
ms.sourcegitcommit: 710fc5fcac258cc8415976dc66bdb355b3e061d5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52299428"
---
<a name="twitter-helper-with-aspnet-web-pages"></a><span data-ttu-id="3b4be-104">Helper Twitter avec les Pages Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3b4be-104">Twitter Helper with ASP.NET Web Pages</span></span>
====================
<span data-ttu-id="3b4be-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="3b4be-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3b4be-106">Programmes d’assistance Twitter sont obsolètes.</span><span class="sxs-lookup"><span data-stu-id="3b4be-106">Twitter Helpers are obsolete.</span></span> <span data-ttu-id="3b4be-107">Pour les outils de Twitter dernière engagement pour les sites Web, consultez [Twitter pour une vue d’ensemble de sites Web](https://developer.twitter.com/en/docs/twitter-for-websites/overview).</span><span class="sxs-lookup"><span data-stu-id="3b4be-107">For Twitter's latest engagement tools for websites, see [Twitter for Websites Overview](https://developer.twitter.com/en/docs/twitter-for-websites/overview).</span></span>

> <span data-ttu-id="3b4be-108">Cette rubrique et l’application montrent comment ajouter une application auxiliaire Twitter à votre projet WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="3b4be-108">This topic and application show how to add a Twitter Helper to your WebMatrix 3 project.</span></span> <span data-ttu-id="3b4be-109">Il contient le code de l’application auxiliaire Twitter et montre comment appeler les méthodes d’assistance.</span><span class="sxs-lookup"><span data-stu-id="3b4be-109">It contains the Twitter Helper code and shows how to call the helper methods.</span></span>
> 
> <span data-ttu-id="3b4be-110">Ce code pour le fichier Twitter.cshtml a été développé par **Tian panoramique** de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="3b4be-110">This code for the Twitter.cshtml file was developed by **Tian Pan** of Microsoft.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="3b4be-111">Versions des logiciels utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="3b4be-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="3b4be-112">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="3b4be-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="3b4be-113">Ce didacticiel fonctionne également avec ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="3b4be-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="introduction"></a><span data-ttu-id="3b4be-114">Introduction</span><span class="sxs-lookup"><span data-stu-id="3b4be-114">Introduction</span></span>

<span data-ttu-id="3b4be-115">Cette rubrique montre comment ajouter une application auxiliaire Twitter à votre application et utiliser la syntaxe Razor pour appeler les méthodes d’assistance.</span><span class="sxs-lookup"><span data-stu-id="3b4be-115">This topic demonstrates how to add a Twitter Helper to your application and use Razor syntax to call the helper methods.</span></span> <span data-ttu-id="3b4be-116">L’application auxiliaire Twitter vous permet de facilement incorporer des widgets dans votre application et les boutons de Twitter.</span><span class="sxs-lookup"><span data-stu-id="3b4be-116">The Twitter Helper makes it easy to incorporate Twitter buttons and widgets in your application.</span></span> <span data-ttu-id="3b4be-117">Pour utiliser un widget de Twitter, telles que la chronologie d’un utilisateur ou les résultats de recherche pour un mot-dièse, vous devez d’abord créer le [widget sur Twitter](https://twitter.com/settings/widgets).</span><span class="sxs-lookup"><span data-stu-id="3b4be-117">To use a Twitter widget, such as a user's timeline or the search results for a hashtag, you must first create the [widget on Twitter](https://twitter.com/settings/widgets).</span></span> <span data-ttu-id="3b4be-118">Après avoir créé votre widget, vous recevrez un id de widget. Vous passez cet id de widget en tant que paramètre lorsque vous appelez les méthodes d’assistance qui montrent le widget.</span><span class="sxs-lookup"><span data-stu-id="3b4be-118">After creating your widget, you will receive a widget id. You pass this widget id as a parameter when calling the helper methods that show widget.</span></span>

<span data-ttu-id="3b4be-119">Cette rubrique a été écrit pour la version 1.1 de l’API Twitter.</span><span class="sxs-lookup"><span data-stu-id="3b4be-119">This topic was written for version 1.1 of the Twitter API.</span></span> <span data-ttu-id="3b4be-120">En ajoutant directement le code de l’application auxiliaire Twitter à votre projet, vous pouvez mettre à jour le code d’assistance si l’API Twitter change.</span><span class="sxs-lookup"><span data-stu-id="3b4be-120">By directly adding the Twitter Helper code to your project, you can update the helper code if the Twitter API changes.</span></span>

<span data-ttu-id="3b4be-121">Pour plus d’informations sur l’installation de WebMatrix, consultez [Introducing ASP.NET Web Pages 2 - mise en route](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="3b4be-121">For information about installing WebMatrix, see [Introducing ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span></span>

## <a name="add-twitter-helper-to-your-project"></a><span data-ttu-id="3b4be-122">Ajouter l’application auxiliaire Twitter à votre projet</span><span class="sxs-lookup"><span data-stu-id="3b4be-122">Add Twitter Helper to your project</span></span>

<span data-ttu-id="3b4be-123">Pour ajouter l’application auxiliaire Twitter, tout d’abord, ajoutez un dossier nommé **application\_Code** à votre projet.</span><span class="sxs-lookup"><span data-stu-id="3b4be-123">To add the Twitter Helper, first, add a folder named **App\_Code** to your project.</span></span> <span data-ttu-id="3b4be-124">Ensuite, créez un fichier nommé **Twitter.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="3b4be-124">Then, create a file named **Twitter.cshtml**.</span></span>

![Dossier App_Code](twitter-helper/_static/image1.png)

<span data-ttu-id="3b4be-126">Remplacez le code par défaut dans Twitter.cshtml par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="3b4be-126">Replace the default code in Twitter.cshtml with the following code.</span></span>

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a><span data-ttu-id="3b4be-127">Appelez les méthodes de Twitter à partir de vos pages web</span><span class="sxs-lookup"><span data-stu-id="3b4be-127">Call Twitter methods from your web pages</span></span>

<span data-ttu-id="3b4be-128">L’exemple suivant montre comment utiliser les méthodes d’assistance de Twitter à partir d’une page dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="3b4be-128">The following example shows how to use the Twitter Helper methods from a page in your project.</span></span> <span data-ttu-id="3b4be-129">Dans votre projet, vous devez remplacer les valeurs des paramètres avec des valeurs qui correspondent à vos besoins.</span><span class="sxs-lookup"><span data-stu-id="3b4be-129">In your project, you will want to replace the parameter values with values that are relevant to your needs.</span></span> <span data-ttu-id="3b4be-130">Vous pouvez utiliser les ID de widget fourni pour Explorer le fonctionnement des méthodes, mais vous pouvez générer vos propres widgets pour votre projet.</span><span class="sxs-lookup"><span data-stu-id="3b4be-130">You can use the provided widget ids to explore how the methods work, but you will want to generate your own widgets for your project.</span></span>

<span data-ttu-id="3b4be-131">Tous les paramètres indiqués ci-dessous sont requis.</span><span class="sxs-lookup"><span data-stu-id="3b4be-131">Not all of the parameters shown below are required.</span></span> <span data-ttu-id="3b4be-132">Les paramètres facultatifs sont utilisés pour personnaliser la façon dont le bouton ou un widget s’affiche.</span><span class="sxs-lookup"><span data-stu-id="3b4be-132">The optional parameters are used to customize how the button or widget is displayed.</span></span> <span data-ttu-id="3b4be-133">Par exemple, le bouton suivre requiert uniquement le nom d’utilisateur à suivre, mais l’exemple montre comment inclure le nombre d’abonnés et comment spécifier la taille du bouton et la langue.</span><span class="sxs-lookup"><span data-stu-id="3b4be-133">For example, the Follow Button only requires the user name to follow, but the example shows how to include the number of followers, and how specify the size of the button and the language.</span></span>

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a><span data-ttu-id="3b4be-134">Afficher les résultats</span><span class="sxs-lookup"><span data-stu-id="3b4be-134">See the results</span></span>

<span data-ttu-id="3b4be-135">Le code ci-dessus génère les boutons et les widgets suivants.</span><span class="sxs-lookup"><span data-stu-id="3b4be-135">The above code produces the following buttons and widgets.</span></span> <span data-ttu-id="3b4be-136">Ces boutons et les widgets sont entièrement fonctionnelles, pas des captures d’écran.</span><span class="sxs-lookup"><span data-stu-id="3b4be-136">These buttons and widgets are fully-functional, not screenshots.</span></span> <span data-ttu-id="3b4be-137">Le bouton suivre est affiché en espagnol, car le paramètre de langue a été défini sur **es**.</span><span class="sxs-lookup"><span data-stu-id="3b4be-137">The Follow Button is displayed in Spanish because the language parameter was set to **es**.</span></span>

### <a name="follow-button"></a><span data-ttu-id="3b4be-138">Suivez le bouton</span><span class="sxs-lookup"><span data-stu-id="3b4be-138">Follow Button</span></span>

<span data-ttu-id="3b4be-139">[Suivez @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="3b4be-139">[Follow @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="tweet-button"></a><span data-ttu-id="3b4be-140">Bouton de tweet</span><span class="sxs-lookup"><span data-stu-id="3b4be-140">Tweet Button</span></span>

<span data-ttu-id="3b4be-141">[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="3b4be-141">[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="user-timeline-profile"></a><span data-ttu-id="3b4be-142">Chronologie de l’utilisateur (profil)</span><span class="sxs-lookup"><span data-stu-id="3b4be-142">User Timeline (Profile)</span></span>

<span data-ttu-id="3b4be-143">[Tweets par @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="3b4be-143">[Tweets by @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="favorites"></a><span data-ttu-id="3b4be-144">Favoris</span><span class="sxs-lookup"><span data-stu-id="3b4be-144">Favorites</span></span>

<span data-ttu-id="3b4be-145">[Favoris Tweets par @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="3b4be-145">[Favorite Tweets by @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="list"></a><span data-ttu-id="3b4be-146">Liste</span><span class="sxs-lookup"><span data-stu-id="3b4be-146">List</span></span>

<span data-ttu-id="3b4be-147">[À partir des tweets @Microsoft/MS \_consommateur\_bandes](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="3b4be-147">[Tweets from @Microsoft/MS\_Consumer\_Bands](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="search"></a><span data-ttu-id="3b4be-148">Rechercher</span><span class="sxs-lookup"><span data-stu-id="3b4be-148">Search</span></span>

<span data-ttu-id="3b4be-149">[Un Tweet sur &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="3b4be-149">[Tweets about &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>
