---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Application auxiliaire avec ASP.NET Web Pages Twitter | Documents Microsoft
author: tfitzmac
description: Cette rubrique, l’application montrent comment ajouter une application auxiliaire Twitter à votre projet WebMatrix 3. Il contient le code de l’application auxiliaire Twitter et montre comment appeler l’application d’assistance...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 07d9c4d485c42b78a42c54c9740af5f67cb44763
ms.sourcegitcommit: 1b94305cc79843e2b0866dae811dab61c21980ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/25/2018
---
<a name="twitter-helper-with-aspnet-web-pages"></a><span data-ttu-id="c1190-104">Programme d’assistance de Twitter avec les Pages Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c1190-104">Twitter Helper with ASP.NET Web Pages</span></span>
====================
<span data-ttu-id="c1190-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="c1190-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="c1190-106">Cette rubrique, l’application montrent comment ajouter une application auxiliaire Twitter à votre projet WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="c1190-106">This topic and application show how to add a Twitter Helper to your WebMatrix 3 project.</span></span> <span data-ttu-id="c1190-107">Il contient le code de l’application auxiliaire Twitter et montre comment appeler les méthodes d’assistance.</span><span class="sxs-lookup"><span data-stu-id="c1190-107">It contains the Twitter Helper code and shows how to call the helper methods.</span></span>
> 
> <span data-ttu-id="c1190-108">Ce code pour le fichier Twitter.cshtml a été développé par **Tian panoramique** de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="c1190-108">This code for the Twitter.cshtml file was developed by **Tian Pan** of Microsoft.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c1190-109">Versions du logiciel utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="c1190-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="c1190-110">Pages Web ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="c1190-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="c1190-111">Ce didacticiel fonctionne également avec ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="c1190-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="introduction"></a><span data-ttu-id="c1190-112">Introduction</span><span class="sxs-lookup"><span data-stu-id="c1190-112">Introduction</span></span>

<span data-ttu-id="c1190-113">Cette rubrique montre comment ajouter une application auxiliaire Twitter à votre application et utiliser la syntaxe Razor pour appeler les méthodes d’assistance.</span><span class="sxs-lookup"><span data-stu-id="c1190-113">This topic demonstrates how to add a Twitter Helper to your application and use Razor syntax to call the helper methods.</span></span> <span data-ttu-id="c1190-114">L’application auxiliaire Twitter facilite l’incorporer des widgets dans votre application et les boutons de Twitter.</span><span class="sxs-lookup"><span data-stu-id="c1190-114">The Twitter Helper makes it easy to incorporate Twitter buttons and widgets in your application.</span></span> <span data-ttu-id="c1190-115">Pour utiliser un widget Twitter, tels que la chronologie de l’utilisateur ou les résultats de recherche pour un #sqlhelp, vous devez d’abord créer le [widget sur Twitter](https://twitter.com/settings/widgets).</span><span class="sxs-lookup"><span data-stu-id="c1190-115">To use a Twitter widget, such as a user's timeline or the search results for a hashtag, you must first create the [widget on Twitter](https://twitter.com/settings/widgets).</span></span> <span data-ttu-id="c1190-116">Après avoir créé votre widget, vous recevez un id de widget. Vous passez cet id de widget en tant que paramètre lorsque vous appelez les méthodes d’assistance qui montrent le widget.</span><span class="sxs-lookup"><span data-stu-id="c1190-116">After creating your widget, you will receive a widget id. You pass this widget id as a parameter when calling the helper methods that show widget.</span></span>

<span data-ttu-id="c1190-117">Cette rubrique a été écrit pour la version 1.1 de l’API Twitter.</span><span class="sxs-lookup"><span data-stu-id="c1190-117">This topic was written for version 1.1 of the Twitter API.</span></span> <span data-ttu-id="c1190-118">En ajoutant directement le code de l’application auxiliaire Twitter à votre projet, vous pouvez mettre à jour le code d’assistance si l’API Twitter change.</span><span class="sxs-lookup"><span data-stu-id="c1190-118">By directly adding the Twitter Helper code to your project, you can update the helper code if the Twitter API changes.</span></span>

<span data-ttu-id="c1190-119">Pour plus d’informations sur l’installation de WebMatrix, consultez [Introducing ASP.NET Web Pages 2 - mise en route](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="c1190-119">For information about installing WebMatrix, see [Introducing ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span></span>

## <a name="add-twitter-helper-to-your-project"></a><span data-ttu-id="c1190-120">Ajouter l’application auxiliaire Twitter à votre projet</span><span class="sxs-lookup"><span data-stu-id="c1190-120">Add Twitter Helper to your project</span></span>

<span data-ttu-id="c1190-121">Pour ajouter l’application auxiliaire Twitter, tout d’abord, ajoutez un dossier nommé **application\_Code** à votre projet.</span><span class="sxs-lookup"><span data-stu-id="c1190-121">To add the Twitter Helper, first, add a folder named **App\_Code** to your project.</span></span> <span data-ttu-id="c1190-122">Ensuite, créez un fichier nommé **Twitter.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="c1190-122">Then, create a file named **Twitter.cshtml**.</span></span>

![Dossier App_Code](twitter-helper/_static/image1.png)

<span data-ttu-id="c1190-124">Remplacez le code par défaut dans Twitter.cshtml par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="c1190-124">Replace the default code in Twitter.cshtml with the following code.</span></span>

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a><span data-ttu-id="c1190-125">Appeler des méthodes Twitter à partir de vos pages web</span><span class="sxs-lookup"><span data-stu-id="c1190-125">Call Twitter methods from your web pages</span></span>

<span data-ttu-id="c1190-126">L’exemple suivant montre comment utiliser les méthodes de l’application auxiliaire Twitter à partir d’une page dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="c1190-126">The following example shows how to use the Twitter Helper methods from a page in your project.</span></span> <span data-ttu-id="c1190-127">Dans votre projet, vous devez remplacer les valeurs de paramètre avec des valeurs qui correspondent à vos besoins.</span><span class="sxs-lookup"><span data-stu-id="c1190-127">In your project, you will want to replace the parameter values with values that are relevant to your needs.</span></span> <span data-ttu-id="c1190-128">Vous pouvez utiliser les ID de widget fourni pour Explorer le fonctionnement des méthodes, mais que vous souhaitez générer vos propres widgets pour votre projet.</span><span class="sxs-lookup"><span data-stu-id="c1190-128">You can use the provided widget ids to explore how the methods work, but you will want to generate your own widgets for your project.</span></span>

<span data-ttu-id="c1190-129">Tous les paramètres indiqués ci-dessous sont requises.</span><span class="sxs-lookup"><span data-stu-id="c1190-129">Not all of the parameters shown below are required.</span></span> <span data-ttu-id="c1190-130">Les paramètres facultatifs sont utilisés pour personnaliser la façon dont le bouton ou le widget s’affiche.</span><span class="sxs-lookup"><span data-stu-id="c1190-130">The optional parameters are used to customize how the button or widget is displayed.</span></span> <span data-ttu-id="c1190-131">Par exemple, le bouton suivre requiert uniquement le nom d’utilisateur à suivre, mais l’exemple montre comment inclure le nombre de suivis et comment spécifier la taille du bouton et la langue.</span><span class="sxs-lookup"><span data-stu-id="c1190-131">For example, the Follow Button only requires the user name to follow, but the example shows how to include the number of followers, and how specify the size of the button and the language.</span></span>

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a><span data-ttu-id="c1190-132">Afficher les résultats</span><span class="sxs-lookup"><span data-stu-id="c1190-132">See the results</span></span>

<span data-ttu-id="c1190-133">Le code ci-dessus génère les boutons et les widgets suivants.</span><span class="sxs-lookup"><span data-stu-id="c1190-133">The above code produces the following buttons and widgets.</span></span> <span data-ttu-id="c1190-134">Ces boutons et les widgets sont entièrement fonctionnelles, pas les captures d’écran.</span><span class="sxs-lookup"><span data-stu-id="c1190-134">These buttons and widgets are fully-functional, not screenshots.</span></span> <span data-ttu-id="c1190-135">Le bouton suivre est affiché en espagnol, car le paramètre de langue a été défini sur **es**.</span><span class="sxs-lookup"><span data-stu-id="c1190-135">The Follow Button is displayed in Spanish because the language parameter was set to **es**.</span></span>

### <a name="follow-button"></a><span data-ttu-id="c1190-136">Suivez le bouton</span><span class="sxs-lookup"><span data-stu-id="c1190-136">Follow Button</span></span>

[<span data-ttu-id="c1190-137">Suivez @aspnet)</span><span class="sxs-lookup"><span data-stu-id="c1190-137">Follow @aspnet)</span></span>](https://twitter.com/aspnet)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>

### <a name="tweet-button"></a><span data-ttu-id="c1190-138">Bouton de tweet</span><span class="sxs-lookup"><span data-stu-id="c1190-138">Tweet Button</span></span>

[<span data-ttu-id="c1190-139">Tweet</span><span class="sxs-lookup"><span data-stu-id="c1190-139">Tweet</span></span>](https://twitter.com/share)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>

### <a name="user-timeline-profile"></a><span data-ttu-id="c1190-140">Chronologie de l’utilisateur (profil)</span><span class="sxs-lookup"><span data-stu-id="c1190-140">User Timeline (Profile)</span></span>

[<span data-ttu-id="c1190-141">Tweets par @aspnet</span><span class="sxs-lookup"><span data-stu-id="c1190-141">Tweets by @aspnet</span></span>](https://twitter.com/aspnet)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>

### <a name="favorites"></a><span data-ttu-id="c1190-142">Favoris</span><span class="sxs-lookup"><span data-stu-id="c1190-142">Favorites</span></span>

[<span data-ttu-id="c1190-143">Favoris tweet par @Microsoft</span><span class="sxs-lookup"><span data-stu-id="c1190-143">Favorite Tweets by @Microsoft</span></span>](https://twitter.com/Microsoft/favorites)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>

### <a name="list"></a><span data-ttu-id="c1190-144">Liste</span><span class="sxs-lookup"><span data-stu-id="c1190-144">List</span></span>

[<span data-ttu-id="c1190-145">Tweets de @Microsoft/MS \_consommateur\_bandes</span><span class="sxs-lookup"><span data-stu-id="c1190-145">Tweets from @Microsoft/MS\_Consumer\_Bands</span></span>](https://twitter.com/microsoft/ms-consumer-brands/)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>

### <a name="search"></a><span data-ttu-id="c1190-146">Rechercher</span><span class="sxs-lookup"><span data-stu-id="c1190-146">Search</span></span>

[<span data-ttu-id="c1190-147">Tweets sur &quot;#asp.net&quot;</span><span class="sxs-lookup"><span data-stu-id="c1190-147">Tweets about &quot;#asp.net&quot;</span></span>](https://twitter.com/search?q=%23asp.net)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>
