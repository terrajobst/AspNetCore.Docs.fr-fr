---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: Le suivi des informations sur les visiteurs (Analytique) pour ASP.NET Web Pages (Razor) Site | Documents Microsoft
author: tfitzmac
description: Une fois que vous avez obtenu votre site Web va, vous souhaiterez analyser le trafic de votre site Web.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 9a381ebaed30325fdfa5f0f558910d3002c61559
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="72dc3-103">Informations de suivi visiteur (Analytique) pour un Site de Pages (Razor) Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="72dc3-103">Tracking Visitor Information (Analytics) for an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="72dc3-104">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="72dc3-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="72dc3-105">Cet article décrit comment utiliser un programme d’assistance pour ajouter analytique de site Web vers les pages dans un site Web ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="72dc3-105">This article describes how to use a helper to add website analytics to pages in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="72dc3-106">Ce que vous allez apprendre :</span><span class="sxs-lookup"><span data-stu-id="72dc3-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="72dc3-107">Comment envoyer des informations sur le trafic de votre site Web à un fournisseur de l’analytique.</span><span class="sxs-lookup"><span data-stu-id="72dc3-107">How to send information about your website traffic to an analytics provider.</span></span>
> 
> <span data-ttu-id="72dc3-108">Il s’agit de programmation des fonctionnalités introduites dans l’article ASP.NET :</span><span class="sxs-lookup"><span data-stu-id="72dc3-108">These are the ASP.NET programming features introduced in the article:</span></span>
> 
> - <span data-ttu-id="72dc3-109">Le `Analytics` helper.</span><span class="sxs-lookup"><span data-stu-id="72dc3-109">The `Analytics` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="72dc3-110">Versions du logiciel utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="72dc3-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="72dc3-111">Pages Web ASP.NET (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="72dc3-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="72dc3-112">ASP.NET Web Helpers Library (package NuGet)</span><span class="sxs-lookup"><span data-stu-id="72dc3-112">ASP.NET Web Helpers Library (NuGet package)</span></span>


<span data-ttu-id="72dc3-113">Analytique est un terme général qui désigne la technologie qui mesure le trafic sur votre site Web pour vous pouvez de comprendre comment les personnes utilisent le site.</span><span class="sxs-lookup"><span data-stu-id="72dc3-113">Analytics is a general term for technology that measures traffic on your website so you can understand how people use the site.</span></span> <span data-ttu-id="72dc3-114">De nombreux services analytique sont disponibles, y compris les services à partir de Google, Yahoo, StatCounter et d’autres.</span><span class="sxs-lookup"><span data-stu-id="72dc3-114">Many analytics services are available, including services from Google, Yahoo, StatCounter, and others.</span></span>

<span data-ttu-id="72dc3-115">L’analytique de façon fonctionne est que vous vous inscrivez pour un compte avec le fournisseur analytique, dans lequel vous inscrivez le site que vous souhaitez effectuer le suivi. Le fournisseur envoie un extrait de code JavaScript qui inclut un ID ou le code de votre compte de suivi.</span><span class="sxs-lookup"><span data-stu-id="72dc3-115">The way analytics works is that you sign up for an account with the analytics provider, where you register the site that you want to track. The provider sends you a snippet of JavaScript code that includes an ID or tracking code for your account.</span></span> <span data-ttu-id="72dc3-116">Vous ajoutez l’extrait de code JavaScript pour les pages web sur le site que vous souhaitez suivre. (Vous ajoutez généralement l’extrait de code analytique à une page mise en page ou de pied de page ou un autre balisage HTML qui s’affiche sur chaque page de votre site.) Lorsque des utilisateurs demandent une page qui contient l’un de ces extraits de code JavaScript, l’extrait de code envoie des informations sur la page actuelle au fournisseur analytique, qui enregistre les détails des différents sur la page.</span><span class="sxs-lookup"><span data-stu-id="72dc3-116">You add the JavaScript snippet to the web pages on the site that you want to track. (You typically add the analytics snippet to a footer or layout page or other HTML markup that appears on every page in your site.) When users request a page that contains one of these JavaScript snippets, the snippet sends information about the current page to the analytics provider, who records various details about the page.</span></span>

<span data-ttu-id="72dc3-117">Lorsque vous souhaitez examiner vos statistiques de site, vous connecter au site Web du fournisseur de l’analytique.</span><span class="sxs-lookup"><span data-stu-id="72dc3-117">When you want to have a look at your site statistics, you log into the analytics provider's website.</span></span> <span data-ttu-id="72dc3-118">Vous pouvez ensuite afficher toutes sortes de rapports sur votre site, tels que :</span><span class="sxs-lookup"><span data-stu-id="72dc3-118">You can then view all sorts of reports about your site, like:</span></span>

- <span data-ttu-id="72dc3-119">Le nombre de vues de page pour des pages individuelles.</span><span class="sxs-lookup"><span data-stu-id="72dc3-119">The number of page views for individual pages.</span></span> <span data-ttu-id="72dc3-120">Vous pouvez en déduire (environ) combien de personnes visite du site et les pages de votre site sont les plus populaires.</span><span class="sxs-lookup"><span data-stu-id="72dc3-120">This tells you (roughly) how many people are visiting the site, and which pages on your site are the most popular.</span></span>
- <span data-ttu-id="72dc3-121">La durée pendant laquelle des personnes passent sur des pages spécifiques.</span><span class="sxs-lookup"><span data-stu-id="72dc3-121">How long people spend on specific pages.</span></span> <span data-ttu-id="72dc3-122">Cela peut vous indiquer les choses comme si votre page d’accueil consiste à maintenir l’intérêt des utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="72dc3-122">This can tell you things like whether your home page is keeping people's interest.</span></span>
- <span data-ttu-id="72dc3-123">Les personnes de sites ont été avant que les visiteurs de votre site.</span><span class="sxs-lookup"><span data-stu-id="72dc3-123">What sites people were on before they visited your site.</span></span> <span data-ttu-id="72dc3-124">Cela vous permet de comprendre si votre trafic provient de liens, à partir de la recherche et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="72dc3-124">This helps you understand whether your traffic is coming from links, from searches, and so on.</span></span>
- <span data-ttu-id="72dc3-125">Lorsque les utilisateurs visitent votre site et la durée pendant laquelle elles restent.</span><span class="sxs-lookup"><span data-stu-id="72dc3-125">When people visit your site and how long they stay.</span></span>
- <span data-ttu-id="72dc3-126">Quels sont les pays les visiteurs de votre proviennent.</span><span class="sxs-lookup"><span data-stu-id="72dc3-126">What countries your visitors are from.</span></span>
- <span data-ttu-id="72dc3-127">Les navigateurs et les systèmes d’exploitation utilisent les visiteurs.</span><span class="sxs-lookup"><span data-stu-id="72dc3-127">What browsers and operating systems your visitors are using.</span></span>

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a><span data-ttu-id="72dc3-129">À l’aide d’un programme d’assistance pour ajouter l’Analytique sur une Page</span><span class="sxs-lookup"><span data-stu-id="72dc3-129">Using a Helper to Add Analytics to a Page</span></span>

<span data-ttu-id="72dc3-130">Les Pages Web ASP.NET inclut plusieurs programmes d’assistance d’analytique (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, et `Analytics.GetStatCounterHtml`) qui la rendent facile à gérer les extraits de code JavaScript utilisés pour l’analytique.</span><span class="sxs-lookup"><span data-stu-id="72dc3-130">ASP.NET Web Pages includes several analytics helpers (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, and `Analytics.GetStatCounterHtml`) that make it easy to manage the JavaScript snippets used for analytics.</span></span> <span data-ttu-id="72dc3-131">Au lieu d’essayer de comprendre comment et où placer le code JavaScript, il vous suffit consiste à ajouter l’application d’assistance à une page.</span><span class="sxs-lookup"><span data-stu-id="72dc3-131">Instead of figuring out how and where to put the JavaScript code, all you have to do is add the helper to a page.</span></span> <span data-ttu-id="72dc3-132">Les seules informations que vous devez fournir sont votre nom de compte, un ID ou un code de suivi.</span><span class="sxs-lookup"><span data-stu-id="72dc3-132">The only information you need to provide is your account name, ID, or tracking code.</span></span> <span data-ttu-id="72dc3-133">(Pour StatCounter, vous pouvez fournir des valeurs supplémentaires quelques.)</span><span class="sxs-lookup"><span data-stu-id="72dc3-133">(For StatCounter, you also have to provide a few additional values.)</span></span>

<span data-ttu-id="72dc3-134">Dans cette procédure, vous allez créer une page de disposition qui utilise le `GetGoogleHtml` helper.</span><span class="sxs-lookup"><span data-stu-id="72dc3-134">In this procedure, you'll create a layout page that uses the `GetGoogleHtml` helper.</span></span> <span data-ttu-id="72dc3-135">Si vous avez déjà un compte avec l’un des autres fournisseurs analytique, vous pouvez utiliser ce compte à la place et ajuster légèrement en fonction des besoins.</span><span class="sxs-lookup"><span data-stu-id="72dc3-135">If you already have an account with one of the other analytics providers, you can use that account instead and make slight adjustments as needed.</span></span>

> [!NOTE]
> <span data-ttu-id="72dc3-136">Lorsque vous créez un compte analytique, vous inscrire l’URL du site que vous souhaitez suivre.</span><span class="sxs-lookup"><span data-stu-id="72dc3-136">When you create an analytics account, you register the URL of the site that you want to be tracking.</span></span> <span data-ttu-id="72dc3-137">Si vous testez tous les éléments sur votre ordinateur local, vous ne le suivi du trafic réel (est le seul trafic vous), donc vous ne pourrez enregistrement et affichage des statistiques de site.</span><span class="sxs-lookup"><span data-stu-id="72dc3-137">If you're testing everything on your local computer, you won't be tracking actual traffic (the only traffic is you), so you won't be able to record and view site statistics.</span></span> <span data-ttu-id="72dc3-138">Mais cette procédure montre comment vous ajoutez une application d’assistance de l’analytique sur une page.</span><span class="sxs-lookup"><span data-stu-id="72dc3-138">But this procedure shows how you add an analytics helper to a page.</span></span> <span data-ttu-id="72dc3-139">Lorsque vous publiez votre site, le site actif envoie des informations à votre fournisseur d’analytique.</span><span class="sxs-lookup"><span data-stu-id="72dc3-139">When you publish your site, the live site will send information to your analytics provider.</span></span>


1. <span data-ttu-id="72dc3-140">Ajoutez la bibliothèque de programmes d’assistance ASP.NET Web à votre site Web, comme décrit dans [programmes d’assistance de l’installation dans un Site de Pages Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), si vous n’avez pas déjà ajouté.</span><span class="sxs-lookup"><span data-stu-id="72dc3-140">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="72dc3-141">Créer un compte avec Google Analytique et enregistrez le nom de compte.</span><span class="sxs-lookup"><span data-stu-id="72dc3-141">Create an account with Google Analytics and record the account name.</span></span>
3. <span data-ttu-id="72dc3-142">Créer une page de disposition nommée *Analytics.cshtml* et ajoutez le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="72dc3-142">Create a layout page named *Analytics.cshtml* and add the following markup:</span></span>

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="72dc3-143">Vous devez placer l’appel à la `Analytics` helper dans le corps de votre page web (avant les `</body>` balise).</span><span class="sxs-lookup"><span data-stu-id="72dc3-143">You must place the call to the `Analytics` helper in the body of your web page (before the `</body>` tag).</span></span> <span data-ttu-id="72dc3-144">Sinon, le navigateur n’exécute le script.</span><span class="sxs-lookup"><span data-stu-id="72dc3-144">Otherwise, the browser will not run the script.</span></span>

    <span data-ttu-id="72dc3-145">Si vous utilisez un fournisseur analytique différents, utilisez une des applications auxiliaires :</span><span class="sxs-lookup"><span data-stu-id="72dc3-145">If you're using a different analytics provider, use one of the following helpers instead:</span></span>

    - <span data-ttu-id="72dc3-146">(Yahoo)`@Analytics.GetYahooHtml("myaccount")`</span><span class="sxs-lookup"><span data-stu-id="72dc3-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span></span>
    - <span data-ttu-id="72dc3-147">(StatCounter)`@Analytics.GetStatCounterHtml("project", "security")`</span><span class="sxs-lookup"><span data-stu-id="72dc3-147">(StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`</span></span>
4. <span data-ttu-id="72dc3-148">Remplacez `myaccount` avec le nom du compte, ID ou du code de suivi que vous avez créé à l’étape 1.</span><span class="sxs-lookup"><span data-stu-id="72dc3-148">Replace `myaccount` with the name of the account, ID, or tracking code that you created in step 1.</span></span>
5. <span data-ttu-id="72dc3-149">Exécutez la page dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="72dc3-149">Run the page in the browser.</span></span> <span data-ttu-id="72dc3-150">(Assurez-vous que la page est sélectionnée dans le **fichiers** espace de travail avant de l’exécuter.)</span><span class="sxs-lookup"><span data-stu-id="72dc3-150">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
6. <span data-ttu-id="72dc3-151">Dans le navigateur, affichez la source de la page.</span><span class="sxs-lookup"><span data-stu-id="72dc3-151">In the browser, view the page source.</span></span> <span data-ttu-id="72dc3-152">Vous serez en mesure de voir le code de rendu analytique :</span><span class="sxs-lookup"><span data-stu-id="72dc3-152">You'll be able to see the rendered analytics code:</span></span>

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. <span data-ttu-id="72dc3-153">Connectez-vous au site Google Analytique et examiner les statistiques de votre site.</span><span class="sxs-lookup"><span data-stu-id="72dc3-153">Log onto the Google Analytics site and examine the statistics for your site.</span></span> <span data-ttu-id="72dc3-154">Si vous utilisez la page sur un site en direct, vous voyez une entrée qui enregistre la visite à votre page.</span><span class="sxs-lookup"><span data-stu-id="72dc3-154">If you're running the page on a live site, you see an entry that logs the visit to your page.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="72dc3-155">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="72dc3-155">Additional Resources</span></span>

- [<span data-ttu-id="72dc3-156">Site de Google Analytique</span><span class="sxs-lookup"><span data-stu-id="72dc3-156">Google Analytics site</span></span>](https://www.google.com/analytics/)
- [<span data-ttu-id="72dc3-157">Yahoo! Site Web d’Analytique</span><span class="sxs-lookup"><span data-stu-id="72dc3-157">Yahoo! Web Analytics site</span></span>](http://help.yahoo.com/l/us/yahoo/ywa/)
- [<span data-ttu-id="72dc3-158">StatCounter site</span><span class="sxs-lookup"><span data-stu-id="72dc3-158">StatCounter site</span></span>](http://statcounter.com/)
