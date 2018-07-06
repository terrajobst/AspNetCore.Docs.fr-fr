---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: Utiliser AJAX pour fournir des mises à jour dynamiques | Microsoft Docs
author: microsoft
description: Étape 10 prend en charge les utilisateurs connectés à RSVP leur souhait de participer à un dîner, à l’aide d’une approche basée sur Ajax intégrée dans le détail dîner...
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 9f11c4c15c0ac9bab8d53b18a4e07be4b864b2c7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825199"
---
<a name="use-ajax-to-deliver-dynamic-updates"></a><span data-ttu-id="8aa9b-103">Utiliser AJAX pour fournir des mises à jour dynamiques</span><span class="sxs-lookup"><span data-stu-id="8aa9b-103">Use AJAX to Deliver Dynamic Updates</span></span>
====================
<span data-ttu-id="8aa9b-104">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="8aa9b-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="8aa9b-105">Télécharger PDF</span><span class="sxs-lookup"><span data-stu-id="8aa9b-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="8aa9b-106">Il s’agit d’étape 10 a gratuit [« « l’application NerdDinner](introducing-the-nerddinner-tutorial.md) qui présente en détail comment créer un petit mais terminé, l’application web à l’aide d’ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-106">This is step 10 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="8aa9b-107">Étape 10 prend en charge les utilisateurs connectés à RSVP leur souhait de participer à un dîner, à l’aide d’une approche basée sur Ajax intégrée dans la page de détails dîner.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-107">Step 10 implements support for logged-in users to RSVP their interest in attending a dinner, using an Ajax-based approach integrated within the dinner details page.</span></span>
> 
> <span data-ttu-id="8aa9b-108">Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre le [mise en route avec MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [Store de musique MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) didacticiels.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a><span data-ttu-id="8aa9b-109">NerdDinner étape 10 : Accepte d’AJAX, l’activation des RSVP</span><span class="sxs-lookup"><span data-stu-id="8aa9b-109">NerdDinner Step 10: AJAX Enabling RSVPs Accepts</span></span>

<span data-ttu-id="8aa9b-110">Nous allons maintenant implémenter la prise en charge pour les utilisateurs connectés à RSVP leur souhait de participer à un dîner.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-110">Let's now implement support for logged-in users to RSVP their interest in attending a dinner.</span></span> <span data-ttu-id="8aa9b-111">Nous allons activer cette option à l’aide d’une approche basée sur AJAX est intégrée dans la page de détails dinner.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-111">We'll enable this using an AJAX-based approach integrated within the dinner details page.</span></span>

### <a name="indicating-whether-the-user-is-rsvpd"></a><span data-ttu-id="8aa9b-112">Qui indique si l’utilisateur est répondu</span><span class="sxs-lookup"><span data-stu-id="8aa9b-112">Indicating whether the user is RSVP'd</span></span>

<span data-ttu-id="8aa9b-113">Les utilisateurs peuvent visiter le */Dinners/détails / [id*] URL pour afficher les détails sur un dîner particulier :</span><span class="sxs-lookup"><span data-stu-id="8aa9b-113">Users can visit the */Dinners/Details/[id*] URL to see details about a particular dinner:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

<span data-ttu-id="8aa9b-114">La méthode d’action est implémentée de Details() comme suit :</span><span class="sxs-lookup"><span data-stu-id="8aa9b-114">The Details() action method is implemented like so:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

<span data-ttu-id="8aa9b-115">Notre première étape pour implémenter la prise en charge du protocole RSVP sera pour ajouter une méthode d’assistance de « IsUserRegistered(username) » à notre objet dîner (au sein de la classe partielle Dinner.cs que nous avons créé précédemment).</span><span class="sxs-lookup"><span data-stu-id="8aa9b-115">Our first step to implement RSVP support will be to add an "IsUserRegistered(username)" helper method to our Dinner object (within the Dinner.cs partial class we built earlier).</span></span> <span data-ttu-id="8aa9b-116">Cette méthode d’assistance retourne true ou false selon si l’utilisateur est actuellement répondu pour le dîner :</span><span class="sxs-lookup"><span data-stu-id="8aa9b-116">This helper method returns true or false depending on whether the user is currently RSVP'd for the Dinner:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

<span data-ttu-id="8aa9b-117">Nous pouvons ensuite ajouter le code suivant à notre modèle de vue Details.aspx affiche un message approprié indiquant si l’utilisateur est inscrit ou non pour l’événement :</span><span class="sxs-lookup"><span data-stu-id="8aa9b-117">We can then add the following code to our Details.aspx view template to display an appropriate message indicating whether the user is registered or not for the event:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

<span data-ttu-id="8aa9b-118">Et maintenant lorsqu’un utilisateur visite un dîner, ils sont inscrits pour ils voient ce message :</span><span class="sxs-lookup"><span data-stu-id="8aa9b-118">And now when a user visits a Dinner they are registered for they'll see this message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

<span data-ttu-id="8aa9b-119">Et lorsqu’ils accèdent à un dîner ne sont pas inscrits pour ils voient le message ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="8aa9b-119">And when they visit a Dinner they are not registered for they'll see the below message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a><span data-ttu-id="8aa9b-120">Implémentation de la méthode d’Action Register</span><span class="sxs-lookup"><span data-stu-id="8aa9b-120">Implementing the Register Action Method</span></span>

<span data-ttu-id="8aa9b-121">Nous allons maintenant ajouter les fonctionnalités nécessaires pour permettre aux utilisateurs RSVP pour un dîner à partir de la page de détails.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-121">Let's now add the functionality necessary to enable users to RSVP for a dinner from the details page.</span></span>

<span data-ttu-id="8aa9b-122">Pour implémenter cela, nous allons créer une nouvelle classe « RSVPController » en cliquant sur le répertoire \Controllers et en choisissant Add -&gt;commande de menu de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-122">To implement this, we'll create a new "RSVPController" class by right-clicking on the \Controllers directory and choosing the Add-&gt;Controller menu command.</span></span>

<span data-ttu-id="8aa9b-123">Nous allons implémenter une méthode d’action « S’inscrire » au sein de la nouvelle classe RSVPController qui prend un id pour un dîner en tant qu’argument, récupère l’objet dîner approprié, vérifie si l’utilisateur connecté est actuellement dans la liste des utilisateurs qui se sont inscrits pour celui-ci et si pas ajoute un objet RSVP pour eux :</span><span class="sxs-lookup"><span data-stu-id="8aa9b-123">We'll implement a "Register" action method within the new RSVPController class that takes an id for a Dinner as an argument, retrieves the appropriate Dinner object, checks to see if the logged-in user is currently in the list of users who have registered for it, and if not adds an RSVP object for them:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

<span data-ttu-id="8aa9b-124">Notez que ci-dessus comment nous allons retourner une chaîne simple en tant que la sortie de la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-124">Notice above how we are returning a simple string as the output of the action method.</span></span> <span data-ttu-id="8aa9b-125">Nous pourrions comporter ce message au sein d’un modèle de vue – mais dans la mesure où il est si petit nous utilisons simplement la méthode d’assistance Content() sur la classe de base de contrôleur et retour un message de chaîne du type ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-125">We could have embedded this message within a view template – but since it is so small we'll just use the Content() helper method on the controller base class and return a string message like above.</span></span>

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a><span data-ttu-id="8aa9b-126">Appel de la méthode d’Action RSVPForEvent à l’aide d’AJAX</span><span class="sxs-lookup"><span data-stu-id="8aa9b-126">Calling the RSVPForEvent Action Method using AJAX</span></span>

<span data-ttu-id="8aa9b-127">Nous allons utiliser AJAX pour appeler la méthode d’action Register notre mode Détails.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-127">We'll use AJAX to invoke the Register action method from our Details view.</span></span> <span data-ttu-id="8aa9b-128">L’implémentation de cela est assez facile.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-128">Implementing this is pretty easy.</span></span> <span data-ttu-id="8aa9b-129">Tout d’abord, nous allons ajouter deux références de bibliothèque de script :</span><span class="sxs-lookup"><span data-stu-id="8aa9b-129">First we'll add two script library references:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

<span data-ttu-id="8aa9b-130">La première bibliothèque fait référence à la bibliothèque de script côté client AJAX ASP.NET core.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-130">The first library references the core ASP.NET AJAX client-side script library.</span></span> <span data-ttu-id="8aa9b-131">Ce fichier est d’environ 24 Ko (compressé) et contient des fonctionnalités AJAX core côté client.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-131">This file is approximately 24k in size (compressed) and contains core client-side AJAX functionality.</span></span> <span data-ttu-id="8aa9b-132">La deuxième bibliothèque contient des fonctions utilitaires qui s’intègrent AJAX d’assistance méthodes intégrées de ASP.NET MVC (que nous allons utiliser peu de temps).</span><span class="sxs-lookup"><span data-stu-id="8aa9b-132">The second library contains utility functions that integrate with ASP.NET MVC's built-in AJAX helper methods (which we'll use shortly).</span></span>

<span data-ttu-id="8aa9b-133">Nous pouvons ensuite la mise à jour le code du modèle de vue nous avons ajouté précédemment afin qu’au lieu de la génération d’un message « Vous n’êtes pas inscrit pour cet événement », nous avons à la place d’afficher un lien que lors de l’objet d’un push effectue un appel AJAX qui appelle notre méthode d’action RSVPForEvent sur notre contrôleur RSVP et RSVPs l’utilisateur :</span><span class="sxs-lookup"><span data-stu-id="8aa9b-133">We can then update the view template code we added earlier so that instead of outputing a "You are not registered for this event" message, we instead render a link that when pushed performs an AJAX call that invokes our RSVPForEvent action method on our RSVP controller and RSVPs the user:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

<span data-ttu-id="8aa9b-134">La méthode d’assistance Ajax.ActionLink() utilisée ci-dessus est intégrée à ASP.NET MVC et est similaire à la méthode d’assistance Html.ActionLink(), sauf qu’au lieu d’effectuer une navigation standard rend un appel AJAX de la méthode d’action lorsque l’utilisateur clique sur le lien.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-134">The Ajax.ActionLink() helper method used above is built-into ASP.NET MVC and is similar to the Html.ActionLink() helper method except that instead of performing a standard navigation it makes an AJAX call to the action method when the link is clicked.</span></span> <span data-ttu-id="8aa9b-135">Ci-dessus nous allons appeler la méthode d’action « S’inscrire » sur le contrôleur « RSVP » et le DinnerID en tant que le paramètre « id » en lui transmettant.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-135">Above we are calling the "Register" action method on the "RSVP" controller and passing the DinnerID as the "id" parameter to it.</span></span> <span data-ttu-id="8aa9b-136">Le dernier paramètre AjaxOptions que nous passons indique que nous voulons prendre le contenu retourné à partir de la méthode d’action et de mettre à jour le code HTML &lt;div&gt; élément sur la page dont l’id est « rsvpmsg ».</span><span class="sxs-lookup"><span data-stu-id="8aa9b-136">The final AjaxOptions parameter we are passing indicates that we want to take the content returned from the action method and update the HTML &lt;div&gt; element on the page whose id is "rsvpmsg".</span></span>

<span data-ttu-id="8aa9b-137">Et maintenant quand un utilisateur accède à un dîner qu’ils ne sont pas inscrits mais ils voient un lien vers le protocole RSVP pour qu’il :</span><span class="sxs-lookup"><span data-stu-id="8aa9b-137">And now when a user browses to a dinner they aren't registered for yet they'll see a link to RSVP for it:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

<span data-ttu-id="8aa9b-138">S’il clique sur le lien « Inscrivez-vous à cet événement », ils veulent effectuer un appel AJAX à la méthode d’action de s’inscrire sur le contrôleur RSVP, et lorsqu’elle est terminée, ils voient un message mis à jour comme ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="8aa9b-138">If they click the "RSVP for this event" link they'll make an AJAX call to the Register action method on the RSVP controller, and when it completes they'll see an updated message like below:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

<span data-ttu-id="8aa9b-139">La bande passante réseau et le trafic impliqués lors de l’établissement de cet appel AJAX est très légère.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-139">The network bandwidth and traffic involved when making this AJAX call is really lightweight.</span></span> <span data-ttu-id="8aa9b-140">Lorsque l’utilisateur clique sur le lien « Inscrivez-vous à cet événement », un petit HTTP POST réseau est demandé pour le */Dinners/Register/1* URL se présente comme ci-dessous sur le câble :</span><span class="sxs-lookup"><span data-stu-id="8aa9b-140">When the user clicks on the "RSVP for this event" link, a small HTTP POST network request is made to the */Dinners/Register/1* URL that looks like below on the wire:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

<span data-ttu-id="8aa9b-141">Et la réponse à partir de notre méthode d’action Register est simplement :</span><span class="sxs-lookup"><span data-stu-id="8aa9b-141">And the response from our Register action method is simply:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

<span data-ttu-id="8aa9b-142">Cet appel léger est rapide et fonctionne même sur un réseau lent.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-142">This lightweight call is fast and will work even over a slow network.</span></span>

### <a name="adding-a-jquery-animation"></a><span data-ttu-id="8aa9b-143">Ajout d’une Animation de jQuery</span><span class="sxs-lookup"><span data-stu-id="8aa9b-143">Adding a jQuery Animation</span></span>

<span data-ttu-id="8aa9b-144">Nous avons implémenté les fonctionnalités AJAX fonctionnent bien et rapide.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-144">The AJAX functionality we implemented works well and fast.</span></span> <span data-ttu-id="8aa9b-145">Il peut parfois se produire si vite, cependant, qu’un utilisateur n’avez peut-être remarqué que le lien RSVP a été remplacé par un nouveau texte.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-145">Sometimes it can happen so fast, though, that a user might not notice that the RSVP link has been replaced with new text.</span></span> <span data-ttu-id="8aa9b-146">Pour rendre le résultat un peu plus évident, nous pouvons ajouter une animation simple pour attirer l’attention sur le message de mise à jour.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-146">To make the outcome a little more obvious we can add a simple animation to draw attention to the update message.</span></span>

<span data-ttu-id="8aa9b-147">La valeur par défaut du modèle de projet ASP.NET MVC inclut jQuery – une bibliothèque JavaScript open source excellente (et très populaire) qui est également pris en charge par Microsoft.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-147">The default ASP.NET MVC project template includes jQuery – an excellent (and very popular) open source JavaScript library that is also supported by Microsoft.</span></span> <span data-ttu-id="8aa9b-148">jQuery fournit un nombre de fonctionnalités, notamment une bibliothèque de sélection et effets nice DOM HTML.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-148">jQuery provides a number of features, including a nice HTML DOM selection and effects library.</span></span>

<span data-ttu-id="8aa9b-149">Pour utiliser jQuery, nous allons tout d’abord ajouter une référence de script à celui-ci.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-149">To use jQuery we'll first add a script reference to it.</span></span> <span data-ttu-id="8aa9b-150">Étant donné que nous allons utiliser jQuery dans différents emplacements au sein de notre site, nous allons ajouter la référence de script au sein de notre fichier de page maître Site.master afin que toutes les pages de l’utiliser.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-150">Because we are going to be using jQuery within a variety of places within our site, we'll add the script reference within our Site.master master page file so that all pages can use it.</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

<span data-ttu-id="8aa9b-151">*Conseil : Vérifiez que vous avez installé le correctif de logiciel JavaScript intellisense pour Visual Studio 2008 SP1 qui permet une prise en charge intellisense pour les fichiers JavaScript (y compris jQuery). Vous pouvez le télécharger à partir de : http://tinyurl.com/vs2008javascripthotfix*</span><span class="sxs-lookup"><span data-stu-id="8aa9b-151">*Tip: make sure you have installed the JavaScript intellisense hotfix for VS 2008 SP1 that enables richer intellisense support for JavaScript files (including jQuery). You can download it from: http://tinyurl.com/vs2008javascripthotfix*</span></span>

<span data-ttu-id="8aa9b-152">Le code écrit à l’aide de JQuery souvent utilise un « $() » global méthode JavaScript qui Récupère un ou plusieurs éléments HTML à l’aide d’un sélecteur CSS.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-152">Code written using JQuery often uses a global "$()" JavaScript method that retrieves one or more HTML elements using a CSS selector.</span></span> <span data-ttu-id="8aa9b-153">Par exemple, <em>$("#rsvpmsg")</em> sélectionne tout élément HTML avec l’id de rsvpmsg, tandis que <em>$(".something")</em> sélectionneriez tous les éléments avec le « quelque chose » CSS nom de la classe.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-153">For example, <em>$("#rsvpmsg")</em> selects any HTML element with the id of rsvpmsg, while <em>$(".something")</em> would select all elements with the "something" CSS class name.</span></span> <span data-ttu-id="8aa9b-154">Vous pouvez également écrire des requêtes plus avancées telles que « retourner tous les boutons radio activé » à l’aide d’une requête de sélecteur comme : <em>$(« entrée [@type= radio] [@checked] »)</em>.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-154">You can also write more advanced queries like "return all of the checked radio buttons" using a selector query like: <em>$("input[@type=radio][@checked]")</em>.</span></span>

<span data-ttu-id="8aa9b-155">Une fois que vous avez sélectionné des éléments, vous pouvez appeler des méthodes sur ces derniers entrent en action, comme les masquer : *$(#rsvpmsg").hide() » ;*</span><span class="sxs-lookup"><span data-stu-id="8aa9b-155">Once you've selected elements, you can call methods on them to take action, like hiding them: *$("#rsvpmsg").hide();*</span></span>

<span data-ttu-id="8aa9b-156">Pour notre scénario RSVP, nous allons définir une fonction JavaScript simple nommée « AnimateRSVPMessage » qui sélectionne le « rsvpmsg » &lt;div&gt; et anime la taille de son contenu de texte.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-156">For our RSVP scenario, we'll define a simple JavaScript function named "AnimateRSVPMessage" that selects the "rsvpmsg" &lt;div&gt; and animates the size of its text content.</span></span> <span data-ttu-id="8aa9b-157">Le code ci-dessous démarre le texte de petite, puis les causes à augmenter sur un laps de temps 400 millisecondes :</span><span class="sxs-lookup"><span data-stu-id="8aa9b-157">The below code starts the text small and then causes it to increase over a 400 milliseconds timeframe:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

<span data-ttu-id="8aa9b-158">Nous pouvons ensuite AutoEventWireup cette fonction JavaScript à appeler après notre appel AJAX en passant son nom à notre méthode d’assistance de Ajax.ActionLink() (via le « OnSuccess » AjaxOptions propriété d’événement) :</span><span class="sxs-lookup"><span data-stu-id="8aa9b-158">We can then wire-up this JavaScript function to be called after our AJAX call successfully completes by passing its name to our Ajax.ActionLink() helper method (via the AjaxOptions "OnSuccess" event property):</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

<span data-ttu-id="8aa9b-159">Et maintenant lorsque l’utilisateur clique sur le lien « Inscrivez-vous à cet événement » et notre appel AJAX termine correctement, le message contenu envoyé sera animer et devenir volumineux :</span><span class="sxs-lookup"><span data-stu-id="8aa9b-159">And now when the "RSVP for this event" link is clicked and our AJAX call completes successfully, the content message sent back will animate and grow large:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

<span data-ttu-id="8aa9b-160">En plus de fournir un événement « OnSuccess », l’objet AjaxOptions expose les méthodes OnBegin OnFailure, événements et OnComplete que vous pouvez gérer (ainsi que divers autres propriétés et les options utiles).</span><span class="sxs-lookup"><span data-stu-id="8aa9b-160">In addition to providing an "OnSuccess" event, the AjaxOptions object exposes OnBegin, OnFailure, and OnComplete events that you can handle (along with a variety of other properties and useful options).</span></span>

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a><span data-ttu-id="8aa9b-161">Nettoyage - refactoriser une vue partielle RSVP</span><span class="sxs-lookup"><span data-stu-id="8aa9b-161">Cleanup - Refactor out a RSVP Partial View</span></span>

<span data-ttu-id="8aa9b-162">Notre modèle de vue Détails commence à obtenir un peu long, les heures supplémentaires rend un peu plus difficile à comprendre.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-162">Our details view template is starting to get a little long, which overtime will make it a little harder to understand.</span></span> <span data-ttu-id="8aa9b-163">Pour aider à améliorer la lisibilité du code, nous allons terminer en créant une vue partielle – RSVPStatus.ascx – qui encapsule tout le code de vue RSVP pour notre page de détails.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-163">To help improve the code readability, let's finish up by creating a partial view – RSVPStatus.ascx – that encapsulate all of the RSVP view code for our Details page.</span></span>

<span data-ttu-id="8aa9b-164">Nous pouvons le faire en cliquant sur le dossier \Views\Dinners puis en choisissant Add -&gt;afficher la commande de menu.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-164">We can do this by right-clicking on the \Views\Dinners folder and then choosing the Add-&gt;View menu command.</span></span> <span data-ttu-id="8aa9b-165">Nous en aurons il prennent un objet dîner comme son ViewModel fortement typée.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-165">We'll have it take a Dinner object as its strongly-typed ViewModel.</span></span> <span data-ttu-id="8aa9b-166">Nous pouvons ensuite copier/coller le contenu RSVP à partir de notre fichier details.aspx dedans.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-166">We can then copy/paste the RSVP content from our Details.aspx view into it.</span></span>

<span data-ttu-id="8aa9b-167">Une fois que nous avons fait cela, nous allons également créer une autre vue partielle – EditAndDeleteLinks.ascx - qui encapsule notre code de vue de lien Edit et Delete.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-167">Once we've done that, let's also create another partial view – EditAndDeleteLinks.ascx - that encapsulates our Edit and Delete link view code.</span></span> <span data-ttu-id="8aa9b-168">Nous allons également l’avoir prennent un objet dîner comme son modèle de vues fortement typées et copier/coller la logique de modifier et supprimer à partir de notre fichier details.aspx dedans.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-168">We'll also have it take a Dinner object as its strongly-typed ViewModel, and copy/paste the Edit and Delete logic from our Details.aspx view into it.</span></span>

<span data-ttu-id="8aa9b-169">Notre détails afficher le modèle peut ensuitent simplement incluent deux appels de méthode Html.RenderPartial() en bas :</span><span class="sxs-lookup"><span data-stu-id="8aa9b-169">Our details view template can then just include two Html.RenderPartial() method calls at the bottom:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

<span data-ttu-id="8aa9b-170">Cela rend le code plus clair à lire et à gérer.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-170">This makes the code cleaner to read and maintain.</span></span>

### <a name="next-step"></a><span data-ttu-id="8aa9b-171">Étape suivante</span><span class="sxs-lookup"><span data-stu-id="8aa9b-171">Next Step</span></span>

<span data-ttu-id="8aa9b-172">Examinons à présent comment nous pouvons utiliser AJAX encore plus loin et ajouter la prise en charge du mappage interactives à notre application.</span><span class="sxs-lookup"><span data-stu-id="8aa9b-172">Let's now look at how we can use AJAX even further and add interactive mapping support to our application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8aa9b-173">[Précédent](secure-applications-using-authentication-and-authorization.md)
> [Suivant](use-ajax-to-implement-mapping-scenarios.md)</span><span class="sxs-lookup"><span data-stu-id="8aa9b-173">[Previous](secure-applications-using-authentication-and-authorization.md)
[Next](use-ajax-to-implement-mapping-scenarios.md)</span></span>
