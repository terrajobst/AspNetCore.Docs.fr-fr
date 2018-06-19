---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
title: Ajout de contenu dynamique à une Page mise en cache (VB) | Documents Microsoft
author: microsoft
description: Découvrez comment combiner le contenu dynamique et mise en cache dans la même page. Post-cache vous permet d’afficher le contenu dynamique, tels que bannière publications o...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 68acd884-fb57-4486-a1be-aaa93e380780
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 89421b4bec2170e408ded87ccc918a7a16844a98
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879762"
---
<a name="adding-dynamic-content-to-a-cached-page-vb"></a><span data-ttu-id="d630b-104">Ajout de contenu dynamique à une Page mise en cache (VB)</span><span class="sxs-lookup"><span data-stu-id="d630b-104">Adding Dynamic Content to a Cached Page (VB)</span></span>
====================
<span data-ttu-id="d630b-105">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="d630b-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="d630b-106">Découvrez comment combiner le contenu dynamique et mise en cache dans la même page.</span><span class="sxs-lookup"><span data-stu-id="d630b-106">Learn how to mix dynamic and cached content in the same page.</span></span> <span data-ttu-id="d630b-107">Post-cache permet d’afficher du contenu dynamique, tels que des annonces ou des éléments de news, au sein d’une page qui a été sortie mise en cache.</span><span class="sxs-lookup"><span data-stu-id="d630b-107">Post-cache substitution enables you to display dynamic content, such as banner advertisements or news items, within a page that has been output cached.</span></span>


<span data-ttu-id="d630b-108">En tirant parti de la mise en cache de sortie, vous pouvez améliorer considérablement les performances d’une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="d630b-108">By taking advantage of output caching, you can dramatically improve the performance of an ASP.NET MVC application.</span></span> <span data-ttu-id="d630b-109">Au lieu de la régénération d’une page chaque fois que la page est demandée, la page peut être générée une fois et mis en cache en mémoire pour plusieurs utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="d630b-109">Instead of regenerating a page each and every time the page is requested, the page can be generated once and cached in memory for multiple users.</span></span>

<span data-ttu-id="d630b-110">Mais il existe un problème.</span><span class="sxs-lookup"><span data-stu-id="d630b-110">But there is a problem.</span></span> <span data-ttu-id="d630b-111">Que se passe-t-il si vous avez besoin afficher le contenu dynamique dans la page ?</span><span class="sxs-lookup"><span data-stu-id="d630b-111">What if you need to display dynamic content in the page?</span></span> <span data-ttu-id="d630b-112">Par exemple, imaginez que vous souhaitez afficher une publicité de bannière dans la page.</span><span class="sxs-lookup"><span data-stu-id="d630b-112">For example, imagine that you want to display a banner advertisement in the page.</span></span> <span data-ttu-id="d630b-113">Vous ne souhaitez pas que la publication de bannière doit être mis en cache afin que chaque utilisateur visualise la même publication.</span><span class="sxs-lookup"><span data-stu-id="d630b-113">You don't want the banner advertisement to be cached so that every user sees the very same advertisement.</span></span> <span data-ttu-id="d630b-114">Vous partageraient argent de cette façon.</span><span class="sxs-lookup"><span data-stu-id="d630b-114">You wouldn't make any money that way!</span></span>

<span data-ttu-id="d630b-115">Heureusement, il existe une solution simple.</span><span class="sxs-lookup"><span data-stu-id="d630b-115">Fortunately, there is an easy solution.</span></span> <span data-ttu-id="d630b-116">Vous pouvez tirer parti d’une fonctionnalité de l’infrastructure ASP.NET appelé *post-cache*.</span><span class="sxs-lookup"><span data-stu-id="d630b-116">You can take advantage of a feature of the ASP.NET framework called *post-cache substitution*.</span></span> <span data-ttu-id="d630b-117">Substitution post-cache vous permet de remplacer le contenu dynamique dans une page qui a été mis en cache en mémoire.</span><span class="sxs-lookup"><span data-stu-id="d630b-117">Post-cache substitution enables you to substitute dynamic content in a page that has been cached in memory.</span></span>


<span data-ttu-id="d630b-118">Normalement, lors de la sortie du cache une page à l’aide de la &lt;OutputCache&gt; attribut, la page est mise en cache sur le serveur et le client (navigateur web).</span><span class="sxs-lookup"><span data-stu-id="d630b-118">Normally, when you output cache a page by using the &lt;OutputCache&gt; attribute, the page is cached on both the server and the client (the web browser).</span></span> <span data-ttu-id="d630b-119">Lorsque vous utilisez post-cache, une page est mise en cache uniquement sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="d630b-119">When you use post-cache substitution, a page is cached only on the server.</span></span>


#### <a name="using-post-cache-substitution"></a><span data-ttu-id="d630b-120">À l’aide de la substitution post-cache</span><span class="sxs-lookup"><span data-stu-id="d630b-120">Using Post-Cache Substitution</span></span>

<span data-ttu-id="d630b-121">À l’aide de la substitution post-cache nécessite deux étapes.</span><span class="sxs-lookup"><span data-stu-id="d630b-121">Using post-cache substitution requires two steps.</span></span> <span data-ttu-id="d630b-122">Tout d’abord, vous devez définir une méthode qui retourne une chaîne qui représente le contenu dynamique que vous souhaitez afficher dans la page mise en cache.</span><span class="sxs-lookup"><span data-stu-id="d630b-122">First, you need to define a method that returns a string that represents the dynamic content that you want to display in the cached page.</span></span> <span data-ttu-id="d630b-123">Ensuite, vous appelez la méthode HttpResponse.WriteSubstitution() pour injecter du contenu dynamique dans la page.</span><span class="sxs-lookup"><span data-stu-id="d630b-123">Next, you call the HttpResponse.WriteSubstitution() method to inject the dynamic content into the page.</span></span>

<span data-ttu-id="d630b-124">Par exemple, imaginez que vous souhaitez afficher au hasard des actualités différents dans une page mise en cache.</span><span class="sxs-lookup"><span data-stu-id="d630b-124">Imagine, for example, that you want to randomly display different news items in a cached page.</span></span> <span data-ttu-id="d630b-125">La classe dans la liste 1 expose une méthode unique nommée RenderNews(), qui aléatoirement retourne un élément d’information dans une liste de trois éléments d’actualité.</span><span class="sxs-lookup"><span data-stu-id="d630b-125">The class in Listing 1 exposes a single method, named RenderNews(), that randomly returns one news item from a list of three news items.</span></span>

<span data-ttu-id="d630b-126">**La liste 1 – Models\News.vb**</span><span class="sxs-lookup"><span data-stu-id="d630b-126">**Listing 1 – Models\News.vb**</span></span>

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample1.vb)]

<span data-ttu-id="d630b-127">Pour tirer parti de substitution post-cache, vous appelez la méthode HttpResponse.WriteSubstitution().</span><span class="sxs-lookup"><span data-stu-id="d630b-127">To take advantage of post-cache substitution, you call the HttpResponse.WriteSubstitution() method.</span></span> <span data-ttu-id="d630b-128">La méthode WriteSubstitution() définit le code pour remplacer une région de la page mise en cache avec un contenu dynamique.</span><span class="sxs-lookup"><span data-stu-id="d630b-128">The WriteSubstitution() method sets up the code to replace a region of the cached page with dynamic content.</span></span> <span data-ttu-id="d630b-129">La méthode WriteSubstitution() est utilisée pour afficher l’article aléatoire dans la vue dans la liste 2.</span><span class="sxs-lookup"><span data-stu-id="d630b-129">The WriteSubstitution() method is used to display the random news item in the view in Listing 2.</span></span>

<span data-ttu-id="d630b-130">**Listing 2 – Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="d630b-130">**Listing 2 – Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample2.aspx)]

<span data-ttu-id="d630b-131">La méthode RenderNews est passée à la méthode WriteSubstitution().</span><span class="sxs-lookup"><span data-stu-id="d630b-131">The RenderNews method is passed to the WriteSubstitution() method.</span></span> <span data-ttu-id="d630b-132">Notez que la méthode RenderNews n’est pas appelée.</span><span class="sxs-lookup"><span data-stu-id="d630b-132">Notice that the RenderNews method is not called.</span></span> <span data-ttu-id="d630b-133">Au lieu de cela, une référence à la méthode est passée à WriteSubstitution() à l’aide de l’opérateur AddressOf.</span><span class="sxs-lookup"><span data-stu-id="d630b-133">Instead a reference to the method is passed to WriteSubstitution() with the help of the AddressOf operator.</span></span>

<span data-ttu-id="d630b-134">La vue de l’Index est mis en cache.</span><span class="sxs-lookup"><span data-stu-id="d630b-134">The Index view is cached.</span></span> <span data-ttu-id="d630b-135">La vue est retournée par le contrôleur dans la liste 3.</span><span class="sxs-lookup"><span data-stu-id="d630b-135">The view is returned by the controller in Listing 3.</span></span> <span data-ttu-id="d630b-136">Notez que l’action Index() est décorée avec un &lt;OutputCache&gt; attribut qui provoque l’affichage de l’Index doit être mis en cache pendant 60 secondes.</span><span class="sxs-lookup"><span data-stu-id="d630b-136">Notice that the Index() action is decorated with an &lt;OutputCache&gt; attribute that causes the Index view to be cached for 60 seconds.</span></span>

<span data-ttu-id="d630b-137">**La liste 3 – Controllers\HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="d630b-137">**Listing 3 – Controllers\HomeController.vb**</span></span>

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample3.vb)]

<span data-ttu-id="d630b-138">Même si la vue de l’Index est mis en cache, les éléments de news aléatoires différents sont affichés lorsque vous demandez la page d’Index.</span><span class="sxs-lookup"><span data-stu-id="d630b-138">Even though the Index view is cached, different random news items are displayed when you request the Index page.</span></span> <span data-ttu-id="d630b-139">Lorsque vous demandez la page d’Index, l’heure affichée dans la page ne change pas pendant 60 secondes (voir Figure 1).</span><span class="sxs-lookup"><span data-stu-id="d630b-139">When you request the Index page, the time displayed by the page does not change for 60 seconds (see Figure 1).</span></span> <span data-ttu-id="d630b-140">Le fait que l’heure ne change pas afin de garantir que la page est mise en cache.</span><span class="sxs-lookup"><span data-stu-id="d630b-140">The fact that the time does not change proves that the page is cached.</span></span> <span data-ttu-id="d630b-141">Toutefois, le contenu injectées par les modifications de la méthode : l’élément aléatoire – WriteSubstitution() avec chaque demande.</span><span class="sxs-lookup"><span data-stu-id="d630b-141">However, the content injected by the WriteSubstitution() method – the random news item – changes with each request .</span></span>

<span data-ttu-id="d630b-142">**Figure 1 – injection actualités dynamique dans une page mise en cache**</span><span class="sxs-lookup"><span data-stu-id="d630b-142">**Figure 1 – Injecting dynamic news items in a cached page**</span></span>

![clip_image002](adding-dynamic-content-to-a-cached-page-vb/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a><span data-ttu-id="d630b-144">À l’aide de la substitution post-cache dans les méthodes d’assistance</span><span class="sxs-lookup"><span data-stu-id="d630b-144">Using Post-Cache Substitution in Helper Methods</span></span>

<span data-ttu-id="d630b-145">Un moyen plus simple de tirer parti de substitution post-cache est d’encapsuler l’appel à la méthode WriteSubstitution() au sein d’une méthode d’assistance personnalisé.</span><span class="sxs-lookup"><span data-stu-id="d630b-145">An easier way to take advantage of post-cache substitution is to encapsulate the call to the WriteSubstitution() method within a custom helper method.</span></span> <span data-ttu-id="d630b-146">Cette approche est illustrée par la méthode d’assistance dans la liste 4.</span><span class="sxs-lookup"><span data-stu-id="d630b-146">This approach is illustrated by the helper method in Listing 4.</span></span>

<span data-ttu-id="d630b-147">**La liste 4 – Helpers\AdHelper.vb**</span><span class="sxs-lookup"><span data-stu-id="d630b-147">**Listing 4 – Helpers\AdHelper.vb**</span></span>

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample4.vb)]

<span data-ttu-id="d630b-148">La liste 4 contient un module de Visual Basic qui expose deux méthodes : RenderBanner() et RenderBannerInternal().</span><span class="sxs-lookup"><span data-stu-id="d630b-148">Listing 4 contains a Visual Basic module that exposes two methods: RenderBanner() and RenderBannerInternal().</span></span> <span data-ttu-id="d630b-149">La méthode RenderBanner() représente la méthode d’assistance réel.</span><span class="sxs-lookup"><span data-stu-id="d630b-149">The RenderBanner() method represents the actual helper method.</span></span> <span data-ttu-id="d630b-150">Cette méthode étend la classe HtmlHelper de MVC ASP.NET standard qui vous pouvez d’appeler Html.RenderBanner() dans une vue comme toute autre méthode d’assistance.</span><span class="sxs-lookup"><span data-stu-id="d630b-150">This method extends the standard ASP.NET MVC HtmlHelper class so that you can call Html.RenderBanner() in a view just like any other helper method.</span></span>

<span data-ttu-id="d630b-151">La méthode RenderBanner() appelle la méthode de HttpResponse.WriteSubstitution() en passant de la méthode RenderBannerInternal() à la méthode WriteSubsitution().</span><span class="sxs-lookup"><span data-stu-id="d630b-151">The RenderBanner() method calls the HttpResponse.WriteSubstitution() method passing the RenderBannerInternal() method to the WriteSubsitution() method.</span></span>

<span data-ttu-id="d630b-152">La méthode RenderBannerInternal() est une méthode privée.</span><span class="sxs-lookup"><span data-stu-id="d630b-152">The RenderBannerInternal() method is a private method.</span></span> <span data-ttu-id="d630b-153">Cette méthode ne sera pas exposée comme une méthode d’assistance.</span><span class="sxs-lookup"><span data-stu-id="d630b-153">This method won't be exposed as a helper method.</span></span> <span data-ttu-id="d630b-154">La méthode RenderBannerInternal() retourne aléatoirement une bannière de publication dans la liste des trois images de publication de bannière.</span><span class="sxs-lookup"><span data-stu-id="d630b-154">The RenderBannerInternal() method randomly returns one banner advertisement image from a list of three banner advertisement images.</span></span>

<span data-ttu-id="d630b-155">La vue Index modifiée dans la liste 5 illustre comment vous pouvez utiliser la méthode d’assistance RenderBanner().</span><span class="sxs-lookup"><span data-stu-id="d630b-155">The modified Index view in Listing 5 illustrates how you can use the RenderBanner() helper method.</span></span> <span data-ttu-id="d630b-156">Notez que supplémentaire &lt;% @ Import %&gt; la directive est incluse en haut de la vue à importer l’espace de noms MvcApplication1.Helpers.</span><span class="sxs-lookup"><span data-stu-id="d630b-156">Notice that an additional &lt;%@ Import %&gt; directive is included at the top of the view to import the MvcApplication1.Helpers namespace.</span></span> <span data-ttu-id="d630b-157">Si vous oubliez d’importer cet espace de noms, la méthode RenderBanner() ne s’affiche comme une méthode sur la propriété Html.</span><span class="sxs-lookup"><span data-stu-id="d630b-157">If you neglect to import this namespace, then the RenderBanner() method won't appear as a method on the Html property.</span></span>

<span data-ttu-id="d630b-158">**La liste 5 – Views\Home\Index.aspx (avec la méthode RenderBanner())**</span><span class="sxs-lookup"><span data-stu-id="d630b-158">**Listing 5 – Views\Home\Index.aspx (with RenderBanner() method)**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample5.aspx)]

<span data-ttu-id="d630b-159">Lorsque vous demandez la page rendue par la vue dans la liste 5, une publication de bannière différent est affichée avec chaque requête (voir Figure 2).</span><span class="sxs-lookup"><span data-stu-id="d630b-159">When you request the page rendered by the view in Listing 5, a different banner advertisement is displayed with each request (see Figure 2).</span></span> <span data-ttu-id="d630b-160">La page est mise en cache, mais la publication de la bannière est injectée dynamiquement par la méthode d’assistance RenderBanner().</span><span class="sxs-lookup"><span data-stu-id="d630b-160">The page is cached, but the banner advertisement is injected dynamically by the RenderBanner() helper method.</span></span>

<span data-ttu-id="d630b-161">**Figure 2 : la vue Index affichant une publication de bannière aléatoire**</span><span class="sxs-lookup"><span data-stu-id="d630b-161">**Figure 2 – The Index view displaying a random banner advertisement**</span></span>

![clip_image004](adding-dynamic-content-to-a-cached-page-vb/_static/image2.jpg)

#### <a name="summary"></a><span data-ttu-id="d630b-163">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="d630b-163">Summary</span></span>

<span data-ttu-id="d630b-164">Ce didacticiel explique comment vous pouvez mettre à jour dynamiquement le contenu dans une page mise en cache.</span><span class="sxs-lookup"><span data-stu-id="d630b-164">This tutorial explained how you can dynamically update content in a cached page.</span></span> <span data-ttu-id="d630b-165">Vous avez appris comment utiliser la méthode HttpResponse.WriteSubstitution() pour permettre à injecter dans une page mise en cache du contenu dynamique.</span><span class="sxs-lookup"><span data-stu-id="d630b-165">You learned how to use the HttpResponse.WriteSubstitution() method to enable dynamic content to be injected in a cached page.</span></span> <span data-ttu-id="d630b-166">Vous avez également appris à encapsuler l’appel à la méthode WriteSubstitution() dans une méthode d’assistance HTML.</span><span class="sxs-lookup"><span data-stu-id="d630b-166">You also learned how to encapsulate the call to the WriteSubstitution() method within an HTML helper method.</span></span>

<span data-ttu-id="d630b-167">Tirer parti de la mise en cache chaque fois que possible : il peut avoir un impact considérable sur les performances de vos applications web.</span><span class="sxs-lookup"><span data-stu-id="d630b-167">Take advantage of caching whenever possible – it can have a dramatic impact on the performance of your web applications.</span></span> <span data-ttu-id="d630b-168">Comme expliqué dans ce didacticiel, vous pouvez tirer parti de la mise en cache même si vous avez besoin afficher le contenu dynamique dans vos pages.</span><span class="sxs-lookup"><span data-stu-id="d630b-168">As explained in this tutorial, you can take advantage of caching even when you need to display dynamic content in your pages.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d630b-169">[Précédent](improving-performance-with-output-caching-vb.md)
> [Suivant](creating-a-controller-vb.md)</span><span class="sxs-lookup"><span data-stu-id="d630b-169">[Previous](improving-performance-with-output-caching-vb.md)
[Next](creating-a-controller-vb.md)</span></span>
