---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: "Utiliser AJAX pour fournir des mises à jour dynamiques | Documents Microsoft"
author: microsoft
description: "Étape 10 prend en charge les utilisateurs connectés à RSVP leur intérêt de participer à un dîner, à l’aide d’une approche basée sur Ajax intégrée dans le détail dîner..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 7b75f8c6cf08112eb77d1a9a40222ed1425ef3a7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="use-ajax-to-deliver-dynamic-updates"></a><span data-ttu-id="10253-103">Utiliser AJAX pour fournir des mises à jour dynamiques</span><span class="sxs-lookup"><span data-stu-id="10253-103">Use AJAX to Deliver Dynamic Updates</span></span>
====================
<span data-ttu-id="10253-104">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="10253-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="10253-105">Télécharger le PDF</span><span class="sxs-lookup"><span data-stu-id="10253-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="10253-106">Il s’agit d’étape 10 a gratuit [« « l’application NerdDinner](introducing-the-nerddinner-tutorial.md) qui parcours à comment générer un petit, mais se termine, application web à l’aide d’ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="10253-106">This is step 10 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="10253-107">Étape 10 prend en charge les utilisateurs connectés à RSVP leur intérêt de participer à un dîner, à l’aide d’une approche basée sur Ajax intégrée dans la page de détails dîner.</span><span class="sxs-lookup"><span data-stu-id="10253-107">Step 10 implements support for logged-in users to RSVP their interest in attending a dinner, using an Ajax-based approach integrated within the dinner details page.</span></span>
> 
> <span data-ttu-id="10253-108">Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre les [mise en route a démarré avec MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [magasin de musique MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) didacticiels.</span><span class="sxs-lookup"><span data-stu-id="10253-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a><span data-ttu-id="10253-109">NerdDinner étape 10 : Accepte les AJAX Activation RSVP</span><span class="sxs-lookup"><span data-stu-id="10253-109">NerdDinner Step 10: AJAX Enabling RSVPs Accepts</span></span>

<span data-ttu-id="10253-110">Nous allons maintenant implémenter la prise en charge pour les utilisateurs connectés à RSVP leur intérêt de participer à un dîner.</span><span class="sxs-lookup"><span data-stu-id="10253-110">Let's now implement support for logged-in users to RSVP their interest in attending a dinner.</span></span> <span data-ttu-id="10253-111">Nous allons activer cette option à l’aide d’une approche basée sur AJAX intégrée dans la page de détails dîner.</span><span class="sxs-lookup"><span data-stu-id="10253-111">We'll enable this using an AJAX-based approach integrated within the dinner details page.</span></span>

### <a name="indicating-whether-the-user-is-rsvpd"></a><span data-ttu-id="10253-112">Qui indique si l’utilisateur est auxquels elle répondu</span><span class="sxs-lookup"><span data-stu-id="10253-112">Indicating whether the user is RSVP'd</span></span>

<span data-ttu-id="10253-113">Les utilisateurs peuvent visiter le */Dinners/détails / [id*] URL pour afficher les détails sur un dîner particulier :</span><span class="sxs-lookup"><span data-stu-id="10253-113">Users can visit the */Dinners/Details/[id*] URL to see details about a particular dinner:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

<span data-ttu-id="10253-114">La méthode d’action est implémentée de Details() comme suit :</span><span class="sxs-lookup"><span data-stu-id="10253-114">The Details() action method is implemented like so:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

<span data-ttu-id="10253-115">La première étape pour implémenter la prise en charge RSVP seront pour ajouter une méthode d’assistance de « IsUserRegistered(username) » à notre objet Dinner (au sein de la classe partielle Dinner.cs que nous avons créé précédemment).</span><span class="sxs-lookup"><span data-stu-id="10253-115">Our first step to implement RSVP support will be to add an "IsUserRegistered(username)" helper method to our Dinner object (within the Dinner.cs partial class we built earlier).</span></span> <span data-ttu-id="10253-116">Cette méthode d’assistance retourne true ou false selon si l’utilisateur est auxquels elle répondu actuellement pour le dîner :</span><span class="sxs-lookup"><span data-stu-id="10253-116">This helper method returns true or false depending on whether the user is currently RSVP'd for the Dinner:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

<span data-ttu-id="10253-117">Nous pouvons ensuite ajouter le code suivant à notre modèle de vue Details.aspx pour afficher un message approprié indiquant si l’utilisateur est inscrit ou non pour l’événement :</span><span class="sxs-lookup"><span data-stu-id="10253-117">We can then add the following code to our Details.aspx view template to display an appropriate message indicating whether the user is registered or not for the event:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

<span data-ttu-id="10253-118">Et maintenant lorsqu’un utilisateur visite un dîner, ils sont inscrits pour qu’ils s’affiche ce message :</span><span class="sxs-lookup"><span data-stu-id="10253-118">And now when a user visits a Dinner they are registered for they'll see this message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

<span data-ttu-id="10253-119">Et lorsqu’ils visitent un dîner ne sont pas inscrits pour ils verront le message ci-après :</span><span class="sxs-lookup"><span data-stu-id="10253-119">And when they visit a Dinner they are not registered for they'll see the below message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a><span data-ttu-id="10253-120">Implémentation de la méthode d’Action de Registre</span><span class="sxs-lookup"><span data-stu-id="10253-120">Implementing the Register Action Method</span></span>

<span data-ttu-id="10253-121">Ajoutons maintenant les fonctionnalités nécessaires pour permettre aux utilisateurs RSVP pour un dîner à partir de la page de détails.</span><span class="sxs-lookup"><span data-stu-id="10253-121">Let's now add the functionality necessary to enable users to RSVP for a dinner from the details page.</span></span>

<span data-ttu-id="10253-122">Pour l’implémenter, nous allons créer une nouvelle classe « RSVPController » en cliquant sur le répertoire \Controllers et en choisissant Ajouter -&gt;commande de menu de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="10253-122">To implement this, we'll create a new "RSVPController" class by right-clicking on the \Controllers directory and choosing the Add-&gt;Controller menu command.</span></span>

<span data-ttu-id="10253-123">Nous allons implémenter une méthode d’action « Register » dans la nouvelle classe RSVPController qui accepte un id pour un dîner en tant qu’argument, récupère l’objet Dinner approprié, vérifie si l’utilisateur connecté est actuellement dans la liste des utilisateurs qui se sont inscrits à elle et si pas ajoute un objet RSVP pour eux :</span><span class="sxs-lookup"><span data-stu-id="10253-123">We'll implement a "Register" action method within the new RSVPController class that takes an id for a Dinner as an argument, retrieves the appropriate Dinner object, checks to see if the logged-in user is currently in the list of users who have registered for it, and if not adds an RSVP object for them:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

<span data-ttu-id="10253-124">Notez que ci-dessus comment nous sommes retournant une simple chaîne en tant que la sortie de la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="10253-124">Notice above how we are returning a simple string as the output of the action method.</span></span> <span data-ttu-id="10253-125">Ce message dans un modèle d’affichage : nous avons ont incorporé, car il est si petit nous suivrons simplement la méthode d’assistance Content() sur la classe de base de contrôleur et le retour d’un message de type chaîne comme ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="10253-125">We could have embedded this message within a view template – but since it is so small we'll just use the Content() helper method on the controller base class and return a string message like above.</span></span>

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a><span data-ttu-id="10253-126">Appel de la méthode d’Action RSVPForEvent à l’aide d’AJAX</span><span class="sxs-lookup"><span data-stu-id="10253-126">Calling the RSVPForEvent Action Method using AJAX</span></span>

<span data-ttu-id="10253-127">Nous allons utiliser AJAX pour appeler la méthode d’action de Registre à partir de la vue des détails.</span><span class="sxs-lookup"><span data-stu-id="10253-127">We'll use AJAX to invoke the Register action method from our Details view.</span></span> <span data-ttu-id="10253-128">Cette implémentation est assez simple.</span><span class="sxs-lookup"><span data-stu-id="10253-128">Implementing this is pretty easy.</span></span> <span data-ttu-id="10253-129">Tout d’abord, nous allons ajouter deux références de bibliothèque de script :</span><span class="sxs-lookup"><span data-stu-id="10253-129">First we'll add two script library references:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

<span data-ttu-id="10253-130">La première bibliothèque fait référence à la bibliothèque de script côté client de base ASP.NET AJAX.</span><span class="sxs-lookup"><span data-stu-id="10253-130">The first library references the core ASP.NET AJAX client-side script library.</span></span> <span data-ttu-id="10253-131">Ce fichier est d’environ 24 Ko taille (compressé) et contient des fonctionnalités AJAX core côté client.</span><span class="sxs-lookup"><span data-stu-id="10253-131">This file is approximately 24k in size (compressed) and contains core client-side AJAX functionality.</span></span> <span data-ttu-id="10253-132">La deuxième bibliothèque contient des fonctions utilitaires qui s’intègrent intégrés AJAX méthodes d’assistance de ASP.NET MVC (ce que nous allons utiliser peu de temps).</span><span class="sxs-lookup"><span data-stu-id="10253-132">The second library contains utility functions that integrate with ASP.NET MVC's built-in AJAX helper methods (which we'll use shortly).</span></span>

<span data-ttu-id="10253-133">Vous pouvez ensuite mettre à jour le code ajouté précédemment afin qu’au lieu d’outputing un message « Vous n’êtes pas inscrit pour cet événement », nous à la place d’afficher un lien que lors de l’objet d’un push du modèle de vue effectue un appel AJAX qui appelle la méthode d’action RSVPForEvent sur notre contrôleur RSVP et RSVPs l’utilisateur :</span><span class="sxs-lookup"><span data-stu-id="10253-133">We can then update the view template code we added earlier so that instead of outputing a "You are not registered for this event" message, we instead render a link that when pushed performs an AJAX call that invokes our RSVPForEvent action method on our RSVP controller and RSVPs the user:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

<span data-ttu-id="10253-134">La méthode d’assistance Ajax.ActionLink() ci-dessus est intégrée à ASP.NET MVC et est semblable à la méthode d’assistance Html.ActionLink(), sauf qu’au lieu d’effectuer une navigation standard rend un appel AJAX de la méthode d’action lorsque vous cliquez sur le lien.</span><span class="sxs-lookup"><span data-stu-id="10253-134">The Ajax.ActionLink() helper method used above is built-into ASP.NET MVC and is similar to the Html.ActionLink() helper method except that instead of performing a standard navigation it makes an AJAX call to the action method when the link is clicked.</span></span> <span data-ttu-id="10253-135">Nous sommes au-dessus appelant la méthode d’action « Register » sur le contrôleur « RSVP » et le DinnerID en tant que le paramètre « id » en lui passant.</span><span class="sxs-lookup"><span data-stu-id="10253-135">Above we are calling the "Register" action method on the "RSVP" controller and passing the DinnerID as the "id" parameter to it.</span></span> <span data-ttu-id="10253-136">Le dernier paramètre AjaxOptions nous passons indique que vous voulez prendre le contenu retourné à partir de la méthode d’action et de mettre à jour le code HTML &lt;div&gt; élément sur la page dont l’id est « rsvpmsg ».</span><span class="sxs-lookup"><span data-stu-id="10253-136">The final AjaxOptions parameter we are passing indicates that we want to take the content returned from the action method and update the HTML &lt;div&gt; element on the page whose id is "rsvpmsg".</span></span>

<span data-ttu-id="10253-137">Et maintenant lorsqu’un utilisateur accède à un dîner qu’ils ne sont pas enregistrés pour encore un lien vers RSVP pour qu’il s’affichent :</span><span class="sxs-lookup"><span data-stu-id="10253-137">And now when a user browses to a dinner they aren't registered for yet they'll see a link to RSVP for it:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

<span data-ttu-id="10253-138">S’il clique sur le lien « RSVP pour cet événement » ils veulent effectuer un appel AJAX de la méthode d’action de Registre sur le contrôleur RSVP, et quand elle se termine, il voit un message mis à jour comme ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="10253-138">If they click the "RSVP for this event" link they'll make an AJAX call to the Register action method on the RSVP controller, and when it completes they'll see an updated message like below:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

<span data-ttu-id="10253-139">La bande passante réseau et le trafic impliqué lors de l’établissement de cet appel AJAX est très légère.</span><span class="sxs-lookup"><span data-stu-id="10253-139">The network bandwidth and traffic involved when making this AJAX call is really lightweight.</span></span> <span data-ttu-id="10253-140">Lorsque l’utilisateur clique sur le lien « RSVP pour cet événement », un petit HTTP POST réseau est demandé pour le */Dinners/Register/1* URL ressemble à ci-dessous sur le câble :</span><span class="sxs-lookup"><span data-stu-id="10253-140">When the user clicks on the "RSVP for this event" link, a small HTTP POST network request is made to the */Dinners/Register/1* URL that looks like below on the wire:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

<span data-ttu-id="10253-141">Et la réponse à partir de la méthode d’action Register est simplement :</span><span class="sxs-lookup"><span data-stu-id="10253-141">And the response from our Register action method is simply:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

<span data-ttu-id="10253-142">Cet appel léger est rapide et fonctionne même sur un réseau lent.</span><span class="sxs-lookup"><span data-stu-id="10253-142">This lightweight call is fast and will work even over a slow network.</span></span>

### <a name="adding-a-jquery-animation"></a><span data-ttu-id="10253-143">Ajout d’une Animation de jQuery</span><span class="sxs-lookup"><span data-stu-id="10253-143">Adding a jQuery Animation</span></span>

<span data-ttu-id="10253-144">Nous avons implémenté les fonctionnalités AJAX fonctionnent bien et rapide.</span><span class="sxs-lookup"><span data-stu-id="10253-144">The AJAX functionality we implemented works well and fast.</span></span> <span data-ttu-id="10253-145">Il peut parfois se produire si vite, cependant, qu’un utilisateur en ne rendiez pas compte que le lien RSVP a été remplacé par un nouveau texte.</span><span class="sxs-lookup"><span data-stu-id="10253-145">Sometimes it can happen so fast, though, that a user might not notice that the RSVP link has been replaced with new text.</span></span> <span data-ttu-id="10253-146">Pour rendre le résultat soit un peu plus évidente, nous pouvons ajouter une animation simple pour attirer l’attention sur le message de mise à jour.</span><span class="sxs-lookup"><span data-stu-id="10253-146">To make the outcome a little more obvious we can add a simple animation to draw attention to the update message.</span></span>

<span data-ttu-id="10253-147">La valeur par défaut du modèle de projet ASP.NET MVC inclut jQuery : une bibliothèque de JavaScript d’excellent (et très populaire) open source qui est également pris en charge par Microsoft.</span><span class="sxs-lookup"><span data-stu-id="10253-147">The default ASP.NET MVC project template includes jQuery – an excellent (and very popular) open source JavaScript library that is also supported by Microsoft.</span></span> <span data-ttu-id="10253-148">jQuery fournit un nombre de fonctionnalités, y compris une bibliothèque de sélection et effets nice DOM HTML.</span><span class="sxs-lookup"><span data-stu-id="10253-148">jQuery provides a number of features, including a nice HTML DOM selection and effects library.</span></span>

<span data-ttu-id="10253-149">Pour utiliser jQuery, nous allons tout d’abord ajouter une référence de script à celui-ci.</span><span class="sxs-lookup"><span data-stu-id="10253-149">To use jQuery we'll first add a script reference to it.</span></span> <span data-ttu-id="10253-150">Étant donné que nous allons utiliser jQuery dans différents emplacements au sein de notre site, nous allons ajouter la référence de script au sein de notre fichier de page maître Site.master afin que toutes les pages de l’utiliser.</span><span class="sxs-lookup"><span data-stu-id="10253-150">Because we are going to be using jQuery within a variety of places within our site, we'll add the script reference within our Site.master master page file so that all pages can use it.</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

<span data-ttu-id="10253-151">*Conseil : Vérifiez que vous avez installé le correctif du JavaScript intellisense pour Visual Studio 2008 SP1 qui permet la prise en charge d’intellisense plus riche pour les fichiers JavaScript (y compris jQuery). Vous pouvez le télécharger à partir de : http://tinyurl.com/vs2008javascripthotfix*</span><span class="sxs-lookup"><span data-stu-id="10253-151">*Tip: make sure you have installed the JavaScript intellisense hotfix for VS 2008 SP1 that enables richer intellisense support for JavaScript files (including jQuery). You can download it from: http://tinyurl.com/vs2008javascripthotfix*</span></span>

<span data-ttu-id="10253-152">Le code écrit à l’aide de JQuery souvent utilise un « $() » global méthode JavaScript qui Récupère un ou plusieurs des éléments HTML à l’aide d’un sélecteur CSS.</span><span class="sxs-lookup"><span data-stu-id="10253-152">Code written using JQuery often uses a global "$()" JavaScript method that retrieves one or more HTML elements using a CSS selector.</span></span> <span data-ttu-id="10253-153">Par exemple, *$("#rsvpmsg")* sélectionne un élément HTML portant l’id rsvpmsg, tandis que *$(".something")* Sélectionnez tous les éléments dont le « quelque chose « CSS nom de la classe.</span><span class="sxs-lookup"><span data-stu-id="10253-153">For example, *$("#rsvpmsg")* selects any HTML element with the id of rsvpmsg, while *$(".something")* would select all elements with the "something" CSS class name.</span></span> <span data-ttu-id="10253-154">Vous pouvez également écrire des requêtes plus avancées telles que « retourner tous les boutons radio activé » à l’aide d’une requête de sélecteur comme : *$(« entrée [@type= radio] [@checked] »)*.</span><span class="sxs-lookup"><span data-stu-id="10253-154">You can also write more advanced queries like "return all of the checked radio buttons" using a selector query like: *$("input[@type=radio][@checked]")*.</span></span>

<span data-ttu-id="10253-155">Une fois que vous avez sélectionné les éléments, vous pouvez appeler des méthodes sur ces derniers pour effectuer une action, comme les masquer : *$(#rsvpmsg").hide() » ;*</span><span class="sxs-lookup"><span data-stu-id="10253-155">Once you've selected elements, you can call methods on them to take action, like hiding them: *$("#rsvpmsg").hide();*</span></span>

<span data-ttu-id="10253-156">Dans notre scénario RSVP, nous allons définir une fonction JavaScript simple nommée « AnimateRSVPMessage » qui sélectionne la « rsvpmsg » &lt;div&gt; et réalise une animation de la taille de son contenu de texte.</span><span class="sxs-lookup"><span data-stu-id="10253-156">For our RSVP scenario, we'll define a simple JavaScript function named "AnimateRSVPMessage" that selects the "rsvpmsg" &lt;div&gt; and animates the size of its text content.</span></span> <span data-ttu-id="10253-157">Le code ci-dessous démarre le texte de petite, puis les causes augmentation sur un laps de temps 400 millisecondes :</span><span class="sxs-lookup"><span data-stu-id="10253-157">The below code starts the text small and then causes it to increase over a 400 milliseconds timeframe:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

<span data-ttu-id="10253-158">Nous pouvons ensuite câble de cette fonction JavaScript à appeler après exécution de notre appel AJAX en passant son nom à la méthode d’assistance de Ajax.ActionLink() (via les AjaxOptions « OnSuccess » propriété d’événement) :</span><span class="sxs-lookup"><span data-stu-id="10253-158">We can then wire-up this JavaScript function to be called after our AJAX call successfully completes by passing its name to our Ajax.ActionLink() helper method (via the AjaxOptions "OnSuccess" event property):</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

<span data-ttu-id="10253-159">Et maintenant lorsque vous cliquez sur le lien « RSVP pour cet événement » et notre appel AJAX termine correctement, le message contenu envoyé précédent sera animer et devenir volumineux :</span><span class="sxs-lookup"><span data-stu-id="10253-159">And now when the "RSVP for this event" link is clicked and our AJAX call completes successfully, the content message sent back will animate and grow large:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

<span data-ttu-id="10253-160">En plus de fournir un événement « OnSuccess », l’objet AjaxOptions expose les méthodes OnBegin OnFailure, événements et OnComplete que vous pouvez gérer (ainsi que diverses autres propriétés et les options utiles).</span><span class="sxs-lookup"><span data-stu-id="10253-160">In addition to providing an "OnSuccess" event, the AjaxOptions object exposes OnBegin, OnFailure, and OnComplete events that you can handle (along with a variety of other properties and useful options).</span></span>

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a><span data-ttu-id="10253-161">Nettoyage - refactoriser une vue partielle RSVP</span><span class="sxs-lookup"><span data-stu-id="10253-161">Cleanup - Refactor out a RSVP Partial View</span></span>

<span data-ttu-id="10253-162">Notre modèle de vue Détails commence à obtenir un peu de temps, les heures supplémentaires rend un peu plus difficile à comprendre.</span><span class="sxs-lookup"><span data-stu-id="10253-162">Our details view template is starting to get a little long, which overtime will make it a little harder to understand.</span></span> <span data-ttu-id="10253-163">Pour améliorer la lisibilité du code, nous allons terminer en créant une vue partielle – RSVPStatus.ascx – qui encapsulent le code de vue RSVP pour notre page de détails.</span><span class="sxs-lookup"><span data-stu-id="10253-163">To help improve the code readability, let's finish up by creating a partial view – RSVPStatus.ascx – that encapsulate all of the RSVP view code for our Details page.</span></span>

<span data-ttu-id="10253-164">Vous pouvez le faire en cliquant sur le dossier \Views\Dinners puis en choisissant Ajouter -&gt;afficher la commande de menu.</span><span class="sxs-lookup"><span data-stu-id="10253-164">We can do this by right-clicking on the \Views\Dinners folder and then choosing the Add-&gt;View menu command.</span></span> <span data-ttu-id="10253-165">Nous allons qu’il procède à un objet dîner comme son ViewModel fortement typée.</span><span class="sxs-lookup"><span data-stu-id="10253-165">We'll have it take a Dinner object as its strongly-typed ViewModel.</span></span> <span data-ttu-id="10253-166">Nous pouvons ensuite copier/coller le contenu RSVP à partir de notre fichier details.aspx dans celui-ci.</span><span class="sxs-lookup"><span data-stu-id="10253-166">We can then copy/paste the RSVP content from our Details.aspx view into it.</span></span>

<span data-ttu-id="10253-167">Une fois que nous avons terminé, nous allons également créer une autre vue partielle – EditAndDeleteLinks.ascx - qui encapsule notre modifier et supprimer lien Afficher le code.</span><span class="sxs-lookup"><span data-stu-id="10253-167">Once we've done that, let's also create another partial view – EditAndDeleteLinks.ascx - that encapsulates our Edit and Delete link view code.</span></span> <span data-ttu-id="10253-168">Nous allons également l’avoir prennent un objet dîner comme son ViewModel fortement typée et copier/coller la logique de modifier et supprimer à partir de notre fichier details.aspx dans celui-ci.</span><span class="sxs-lookup"><span data-stu-id="10253-168">We'll also have it take a Dinner object as its strongly-typed ViewModel, and copy/paste the Edit and Delete logic from our Details.aspx view into it.</span></span>

<span data-ttu-id="10253-169">Détails Afficher le modèle peut ensuitent simplement incluent deux appels de méthode Html.RenderPartial() en bas :</span><span class="sxs-lookup"><span data-stu-id="10253-169">Our details view template can then just include two Html.RenderPartial() method calls at the bottom:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

<span data-ttu-id="10253-170">Cela rend le code plus propre pour lire et mettre à jour.</span><span class="sxs-lookup"><span data-stu-id="10253-170">This makes the code cleaner to read and maintain.</span></span>

### <a name="next-step"></a><span data-ttu-id="10253-171">Étape suivante</span><span class="sxs-lookup"><span data-stu-id="10253-171">Next Step</span></span>

<span data-ttu-id="10253-172">Examinons à présent comment nous pouvons utiliser AJAX davantage et ajouter la prise en charge du mappage interactif à notre application.</span><span class="sxs-lookup"><span data-stu-id="10253-172">Let's now look at how we can use AJAX even further and add interactive mapping support to our application.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="10253-173">[Précédent](secure-applications-using-authentication-and-authorization.md)
[Suivant](use-ajax-to-implement-mapping-scenarios.md)</span><span class="sxs-lookup"><span data-stu-id="10253-173">[Previous](secure-applications-using-authentication-and-authorization.md)
[Next](use-ajax-to-implement-mapping-scenarios.md)</span></span>
