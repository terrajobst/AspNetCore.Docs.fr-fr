---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: Éléments à ne pas faire dans ASP.NET et comment réagir à la place | Microsoft Docs
author: Rick-Anderson
description: Cette rubrique décrit les erreurs courantes plusieurs personnes effectué dans les projets web ASP.NET. Il fournit des recommandations pour la procédure à suivre pour éviter ces Commu...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 512d2e2b39467635390fa175546f79d8c9f89f4a
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667711"
---
# <a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a><span data-ttu-id="f728b-104">Ce qu’il ne faut pas faire dans ASP.NET et ce qu’il faut faire à la place</span><span class="sxs-lookup"><span data-stu-id="f728b-104">What not to do in ASP.NET, and what to do instead</span></span>

> <span data-ttu-id="f728b-105">Cette rubrique décrit les erreurs courantes plusieurs personnes effectué dans les projets web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f728b-105">This topic describes several common mistakes people make within ASP.NET web projects.</span></span> <span data-ttu-id="f728b-106">Il fournit des recommandations pour la procédure à suivre pour éviter ces erreurs courantes.</span><span class="sxs-lookup"><span data-stu-id="f728b-106">It provides recommendations for what you should do to avoid these common mistakes.</span></span> <span data-ttu-id="f728b-107">Il est basé sur un [présentation](http://vimeo.com/68390507) par **Damian Edwards** à couronne conférence de développeurs.</span><span class="sxs-lookup"><span data-stu-id="f728b-107">It is based on a [presentation](http://vimeo.com/68390507) by **Damian Edwards** at Norwegian Developers Conference.</span></span>


## <a name="disclaimer"></a><span data-ttu-id="f728b-108">Exclusion de responsabilité</span><span class="sxs-lookup"><span data-stu-id="f728b-108">Disclaimer</span></span>

<span data-ttu-id="f728b-109">Cette rubrique n'est pas conçue comme un guide complet pour vous assurer de que votre application est sécurisé et efficace.</span><span class="sxs-lookup"><span data-stu-id="f728b-109">This topic is not intended as a complete guide to ensure your application is secure and efficient.</span></span> <span data-ttu-id="f728b-110">Vous devez toujours suivre les meilleures pratiques pour la sécurité et de performances qui ne sont pas décrites dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="f728b-110">You still need to follow best practices for security and performance that are not outlined in this topic.</span></span> <span data-ttu-id="f728b-111">Il suggère uniquement comment éviter les erreurs courantes liées aux processus et des classes .NET.</span><span class="sxs-lookup"><span data-stu-id="f728b-111">It only suggests how to avoid common mistakes related to .NET classes and processes.</span></span>

## <a name="overview"></a><span data-ttu-id="f728b-112">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="f728b-112">Overview</span></span>

<span data-ttu-id="f728b-113">Cette rubrique contient les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="f728b-113">This topic contains the following sections:</span></span>

- [<span data-ttu-id="f728b-114">Conformité aux normes</span><span class="sxs-lookup"><span data-stu-id="f728b-114">Standards Compliance</span></span>](#standards)

    - [<span data-ttu-id="f728b-115">Adaptateurs de contrôle</span><span class="sxs-lookup"><span data-stu-id="f728b-115">Control Adapters</span></span>](#adapters)
    - [<span data-ttu-id="f728b-116">Propriétés de style des contrôles</span><span class="sxs-lookup"><span data-stu-id="f728b-116">Style Properties on Controls</span></span>](#styleprop)
    - [<span data-ttu-id="f728b-117">Page et les rappels de contrôle</span><span class="sxs-lookup"><span data-stu-id="f728b-117">Page and Control Callbacks</span></span>](#callback)
    - [<span data-ttu-id="f728b-118">Détection de fonctionnalité de navigateur</span><span class="sxs-lookup"><span data-stu-id="f728b-118">Browser Capability Detection</span></span>](#browsercap)
- [<span data-ttu-id="f728b-119">Sécurité</span><span class="sxs-lookup"><span data-stu-id="f728b-119">Security</span></span>](#security)

    - [<span data-ttu-id="f728b-120">Validation de la demande</span><span class="sxs-lookup"><span data-stu-id="f728b-120">Request Validation</span></span>](#validation)
    - [<span data-ttu-id="f728b-121">Session et l’authentification par formulaire sans cookie</span><span class="sxs-lookup"><span data-stu-id="f728b-121">Cookieless Forms Authentication and Session</span></span>](#cookieless)
    - [<span data-ttu-id="f728b-122">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="f728b-122">EnableViewStateMac</span></span>](#viewstatemac)
    - [<span data-ttu-id="f728b-123">Confiance moyenne</span><span class="sxs-lookup"><span data-stu-id="f728b-123">Medium Trust</span></span>](#medium)
    - [<span data-ttu-id="f728b-124">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="f728b-124">&lt;appSettings&gt;</span></span>](#appsettings)
    - [<span data-ttu-id="f728b-125">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="f728b-125">UrlPathEncode</span></span>](#urlpathencode)
- [<span data-ttu-id="f728b-126">Fiabilité et performances</span><span class="sxs-lookup"><span data-stu-id="f728b-126">Reliability and Performance</span></span>](#performance)

    - [<span data-ttu-id="f728b-127">PreSendRequestHeaders et PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="f728b-127">PreSendRequestHeaders and PreSendRequestContent</span></span>](#presend)
    - [<span data-ttu-id="f728b-128">Événements de Page asynchrone avec les Web Forms</span><span class="sxs-lookup"><span data-stu-id="f728b-128">Asynchronous Page Events with Web Forms</span></span>](#asyncevents)
    - [<span data-ttu-id="f728b-129">Déclenchement et oubli de travail</span><span class="sxs-lookup"><span data-stu-id="f728b-129">Fire-and-Forget Work</span></span>](#fire)
    - [<span data-ttu-id="f728b-130">Corps d’entité</span><span class="sxs-lookup"><span data-stu-id="f728b-130">Request Entity Body</span></span>](#requestentity)
    - [<span data-ttu-id="f728b-131">Compatibilité entre Response.Redirect et Response.End</span><span class="sxs-lookup"><span data-stu-id="f728b-131">Response.Redirect and Response.End</span></span>](#redirect)
    - [<span data-ttu-id="f728b-132">EnableViewState et ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="f728b-132">EnableViewState and ViewStateMode</span></span>](#viewstatemode)
    - [<span data-ttu-id="f728b-133">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="f728b-133">SqlMembershipProvider</span></span>](#sqlprovider)
    - [<span data-ttu-id="f728b-134">Les longues demandes en cours d’exécution (> 110 secondes)</span><span class="sxs-lookup"><span data-stu-id="f728b-134">Long Running Requests (>110 seconds)</span></span>](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a><span data-ttu-id="f728b-135">Conformité aux normes</span><span class="sxs-lookup"><span data-stu-id="f728b-135">Standards Compliance</span></span>

<a id="adapters"></a>

### <a name="control-adapters"></a><span data-ttu-id="f728b-136">Adaptateurs de contrôle</span><span class="sxs-lookup"><span data-stu-id="f728b-136">Control adapters</span></span>

<span data-ttu-id="f728b-137">Recommandation : Arrêter d’utiliser les adaptateurs de contrôle pour le rendu adaptatif et utiliser à la place des requêtes de média CSS et HTML conforme aux normes.</span><span class="sxs-lookup"><span data-stu-id="f728b-137">Recommendation: Stop using control adapters for adaptive rendering, and instead use CSS media queries and standards-compliant HTML.</span></span>

<span data-ttu-id="f728b-138">Les adaptateurs de contrôles ont été introduites dans .NET 2.0 pour restituer le code de présentation qui a été personnalisé pour les environnements et les différents appareils.</span><span class="sxs-lookup"><span data-stu-id="f728b-138">Controls Adapters were introduced in .NET 2.0 to render presentation code that was customized for different devices and environments.</span></span> <span data-ttu-id="f728b-139">À présent, ce rendu adaptatif peut être effectué avec CSS et HTML.</span><span class="sxs-lookup"><span data-stu-id="f728b-139">Now, this adaptive rendering can be accomplished with CSS and HTML.</span></span> <span data-ttu-id="f728b-140">Vous devez cesser d’utiliser les adaptateurs de contrôle et convertir les adaptateurs existants vers CSS et HTML.</span><span class="sxs-lookup"><span data-stu-id="f728b-140">You should stop using Control Adapters and convert any existing adapters to CSS and HTML.</span></span>

<span data-ttu-id="f728b-141">Pour plus d’informations, consultez [les requêtes de média](http://www.w3.org/TR/css3-mediaqueries/) et [How To : Ajouter des Pages mobiles à vos formulaires Web ASP.NET / MVC Application](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="f728b-141">For more information, see [Media Queries](http://www.w3.org/TR/css3-mediaqueries/) and [How To: Add Mobile Pages to Your ASP.NET Web Forms / MVC Application](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span></span>

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a><span data-ttu-id="f728b-142">Propriétés de style des contrôles</span><span class="sxs-lookup"><span data-stu-id="f728b-142">Style properties on controls</span></span>

<span data-ttu-id="f728b-143">Recommandation : Arrêter la définition de valeurs de style dans le balisage du contrôle et définir à la place des valeurs de mise en forme dans les feuilles de style CSS.</span><span class="sxs-lookup"><span data-stu-id="f728b-143">Recommendation: Stop setting style values in the control markup, and instead set formatting values in CSS stylesheets.</span></span>

<span data-ttu-id="f728b-144">Les contrôles serveur Web contiennent des dizaines de propriétés qui peuvent être utilisées pour définir les propriétés de style intraligne.</span><span class="sxs-lookup"><span data-stu-id="f728b-144">Web server controls contain dozens of properties which can be used to set in-line style properties.</span></span> <span data-ttu-id="f728b-145">Par exemple, la propriété ForeColor définit la couleur du texte pour un contrôle.</span><span class="sxs-lookup"><span data-stu-id="f728b-145">For example, the ForeColor property sets the color of the text for a control.</span></span> <span data-ttu-id="f728b-146">Vous pouvez obtenir cet effet même plus efficacement par le biais de feuilles de style CSS.</span><span class="sxs-lookup"><span data-stu-id="f728b-146">You can accomplish this same effect more efficiently through CSS stylesheets.</span></span> <span data-ttu-id="f728b-147">Feuilles de style permettent de centraliser les valeurs de style et évitez de définir ces valeurs dans toute votre application.</span><span class="sxs-lookup"><span data-stu-id="f728b-147">Stylesheets enable you to centralize style values and avoid setting these values throughout your application.</span></span>

<span data-ttu-id="f728b-148">L’exemple suivant montre une classe CSS le texte de jeux en rouge.</span><span class="sxs-lookup"><span data-stu-id="f728b-148">The following example shows a CSS class the sets text to red.</span></span>

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

<span data-ttu-id="f728b-149">L’exemple suivant montre comment appliquer de manière dynamique la classe CSS.</span><span class="sxs-lookup"><span data-stu-id="f728b-149">The next example shows how to dynamically apply the CSS class.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a><span data-ttu-id="f728b-150">Rappels de page et de contrôle</span><span class="sxs-lookup"><span data-stu-id="f728b-150">Page and control callbacks</span></span>

<span data-ttu-id="f728b-151">Recommandation : Arrêter d’utiliser les rappels de page et de contrôle et à la place utiliser les éléments suivants : AJAX, UpdatePanel, MVC méthodes d’action, API Web ou SignalR.</span><span class="sxs-lookup"><span data-stu-id="f728b-151">Recommendation: Stop using page and control callbacks, and instead use any of the following: AJAX, UpdatePanel, MVC action methods, Web API, or SignalR.</span></span>

<span data-ttu-id="f728b-152">Dans les versions antérieures d’ASP.NET, Page et contrôle des méthodes de rappel vous autorisé à mettre à jour de la partie de la page web sans actualiser une page entière.</span><span class="sxs-lookup"><span data-stu-id="f728b-152">In earlier versions of ASP.NET, Page and Control callback methods enabled you to update part of the web page without refreshing an entire page.</span></span> <span data-ttu-id="f728b-153">Vous pouvez désormais effectuer des mises à jour de page partielle via [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [API Web](../../../web-api/index.md) ou [SignalR](../../../signalr/index.md).</span><span class="sxs-lookup"><span data-stu-id="f728b-153">You can now accomplish partial-page updates through [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [Web API](../../../web-api/index.md) or [SignalR](../../../signalr/index.md).</span></span> <span data-ttu-id="f728b-154">Vous devez arrêter à l’aide de méthodes de rappel car ils peuvent provoquer des problèmes avec des URL conviviales et le routage.</span><span class="sxs-lookup"><span data-stu-id="f728b-154">You should stop using callback methods because they can cause issues with friendly URLs and routing.</span></span> <span data-ttu-id="f728b-155">Par défaut, les contrôles ne permettent pas de méthodes de rappel, mais si vous avez activé cette fonctionnalité dans un contrôle, vous devez le désactiver.</span><span class="sxs-lookup"><span data-stu-id="f728b-155">By default, controls do not enable callback methods, but if you enabled this feature in a control, you should disable it.</span></span>

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a><span data-ttu-id="f728b-156">Détection de fonctionnalité de navigateur</span><span class="sxs-lookup"><span data-stu-id="f728b-156">Browser capability detection</span></span>

<span data-ttu-id="f728b-157">Recommandation : Arrêter d’utiliser la détection de fonctionnalité du navigateur statique et à la place utiliser la détection de fonctionnalité dynamique.</span><span class="sxs-lookup"><span data-stu-id="f728b-157">Recommendation: Stop using static browser capability detection, and instead use dynamic feature detection.</span></span>

<span data-ttu-id="f728b-158">Dans les versions antérieures d’ASP.NET, les fonctionnalités prises en charge pour chaque navigateur ont été stockées dans un fichier XML.</span><span class="sxs-lookup"><span data-stu-id="f728b-158">In earlier versions of ASP.NET, the supported features for each browser were stored in an XML file.</span></span> <span data-ttu-id="f728b-159">Prise en charge des fonctionnalités Détection via une recherche statique n’est pas la meilleure approche.</span><span class="sxs-lookup"><span data-stu-id="f728b-159">Detecting feature support through a static lookup is not the best approach.</span></span> <span data-ttu-id="f728b-160">Maintenant, vous pouvez détecter dynamiquement un navigateur de fonctionnalités prises en charge en utilisant une infrastructure de détection de fonctionnalité, telle que [Modernizr](http://modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="f728b-160">Now, you can dynamically detect a browser's supported features by using a feature detection framework, such as [Modernizr](http://modernizr.com/).</span></span> <span data-ttu-id="f728b-161">Détection de fonctionnalité détermine la prise en charge en tente d’utiliser une méthode ou propriété, puis cochez pour voir si le navigateur a produit le résultat souhaité.</span><span class="sxs-lookup"><span data-stu-id="f728b-161">Feature detection determines support by attempting to use a method or property and then checking to see if the browser produced the desired result.</span></span> <span data-ttu-id="f728b-162">Par défaut, Modernizr est inclus dans les modèles d’application Web.</span><span class="sxs-lookup"><span data-stu-id="f728b-162">By default, Modernizr is included in the Web application templates.</span></span>

<a id="security"></a>

## <a name="security"></a><span data-ttu-id="f728b-163">Sécurité</span><span class="sxs-lookup"><span data-stu-id="f728b-163">Security</span></span>

<a id="validation"></a>

### <a name="request-validation"></a><span data-ttu-id="f728b-164">Validation de la demande</span><span class="sxs-lookup"><span data-stu-id="f728b-164">Request validation</span></span>

<span data-ttu-id="f728b-165">Recommandation : Valider l’entrée utilisateur et encoder une sortie à partir des utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="f728b-165">Recommendation: Validate user input, and encode output from users.</span></span>

<span data-ttu-id="f728b-166">Validation de la demande est une fonctionnalité d’ASP.NET qui inspecte chaque requête et s’arrête à la demande si une menace perçue est trouvée.</span><span class="sxs-lookup"><span data-stu-id="f728b-166">Request validation is a feature of ASP.NET that inspects each request and stops the request if a perceived threat is found.</span></span> <span data-ttu-id="f728b-167">Ne dépendent pas de validation de la demande pour la sécurisation de votre application contre les attaques de script entre sites.</span><span class="sxs-lookup"><span data-stu-id="f728b-167">Do not depend on request validation for securing your application against cross-site scripting attacks.</span></span> <span data-ttu-id="f728b-168">Au lieu de cela, validez toutes les entrées des utilisateurs et encoder la sortie.</span><span class="sxs-lookup"><span data-stu-id="f728b-168">Instead, validate all input from users and encode the output.</span></span> <span data-ttu-id="f728b-169">Dans certains cas, vous pouvez utiliser des expressions régulières pour valider l’entrée, mais dans les cas plus complexes, vous devez valider l’entrée utilisateur à l’aide de classes .NET qui déterminent si la valeur correspond à des valeurs autorisées.</span><span class="sxs-lookup"><span data-stu-id="f728b-169">In some limited cases, you can use regular expressions to validate the input, but in more complicated cases you should validate user input by using .NET classes that determine if the value matches allowed values.</span></span>

<span data-ttu-id="f728b-170">L’exemple suivant montre comment utiliser une méthode statique dans la classe Uri pour déterminer si l’Uri fourni par un utilisateur est valide.</span><span class="sxs-lookup"><span data-stu-id="f728b-170">The following example shows how to use a static method in the Uri class to determine whether the Uri provided by a user is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

<span data-ttu-id="f728b-171">Toutefois, pour vérifier suffisamment l’Uri, vous devez également vérifier pour vous assurer qu’il spécifie `http` ou `https`.</span><span class="sxs-lookup"><span data-stu-id="f728b-171">However, to sufficiently verify the Uri, you should also check to make sure it specifies `http` or `https`.</span></span> <span data-ttu-id="f728b-172">L’exemple suivant utilise les méthodes d’instance pour vérifier que l’Uri est valide.</span><span class="sxs-lookup"><span data-stu-id="f728b-172">The following example uses instance methods to verify that the Uri is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

<span data-ttu-id="f728b-173">Avant le rendu des entrées d’utilisateur au format HTML ou d’inclure l’entrée d’utilisateur dans une requête SQL, encoder les valeurs pour garantir un code malveillant n’est pas inclus.</span><span class="sxs-lookup"><span data-stu-id="f728b-173">Before rendering user input as HTML or including user input in a SQL query, encode the values to ensure malicious code is not included.</span></span>

<span data-ttu-id="f728b-174">Vous pouvez HTML encoder la valeur dans le balisage avec la &lt;% : %&gt; syntaxe, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="f728b-174">You can HTML encode the value in markup with the &lt;%: %&gt; syntax, as shown below.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

<span data-ttu-id="f728b-175">Ou, dans la syntaxe Razor, vous pouvez HTML Encoder avec @, comme illustré ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="f728b-175">Or, in Razor syntax, you can HTML encode with @, as shown below.</span></span>

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="f728b-176">L’exemple suivant montre comment au format HTML encode une valeur dans le code-behind.</span><span class="sxs-lookup"><span data-stu-id="f728b-176">The next example shows how to HTML encode a value in code-behind.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

<span data-ttu-id="f728b-177">Pour encoder en toute sécurité une valeur pour les commandes SQL, utilisez les paramètres de commande tels que le [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx).</span><span class="sxs-lookup"><span data-stu-id="f728b-177">To safely encode a value for SQL commands, use command parameters such as the [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx).</span></span> <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a><span data-ttu-id="f728b-178">Session et l’authentification par formulaire sans cookie</span><span class="sxs-lookup"><span data-stu-id="f728b-178">Cookieless forms authentication and session</span></span>

<span data-ttu-id="f728b-179">Recommandation : Recours à des cookies.</span><span class="sxs-lookup"><span data-stu-id="f728b-179">Recommendation: Require cookies.</span></span>

<span data-ttu-id="f728b-180">En passant les informations d’authentification dans la chaîne de requête n’est pas sécurisée.</span><span class="sxs-lookup"><span data-stu-id="f728b-180">Passing authentication information in the query string is not secure.</span></span> <span data-ttu-id="f728b-181">Par conséquent, requièrent des cookies lorsque votre application inclut une authentification.</span><span class="sxs-lookup"><span data-stu-id="f728b-181">Therefore, require cookies when your application includes authentication.</span></span> <span data-ttu-id="f728b-182">Si votre cookie stocke des informations sensibles, envisagez d’exiger SSL pour le cookie.</span><span class="sxs-lookup"><span data-stu-id="f728b-182">If your cookie stores sensitive information, consider requiring SSL for the cookie.</span></span>

<span data-ttu-id="f728b-183">L’exemple suivant montre comment spécifier dans le fichier Web.config que l’authentification par formulaire requiert un cookie qui est transmis via le protocole SSL.</span><span class="sxs-lookup"><span data-stu-id="f728b-183">The following example shows how to specify in the Web.config file that Forms Authentication requires a cookie that is transmitted over SSL.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a><span data-ttu-id="f728b-184">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="f728b-184">EnableViewStateMac</span></span>

<span data-ttu-id="f728b-185">Recommandation : Jamais défini sur false.</span><span class="sxs-lookup"><span data-stu-id="f728b-185">Recommendation: Never set to false.</span></span>

<span data-ttu-id="f728b-186">Par défaut, EnbableViewStateMac est définie sur true.</span><span class="sxs-lookup"><span data-stu-id="f728b-186">By default, EnbableViewStateMac is set to true.</span></span> <span data-ttu-id="f728b-187">Même si votre application n’utilise pas l’état d’affichage, ne définissez pas EnableViewStateMac sur false.</span><span class="sxs-lookup"><span data-stu-id="f728b-187">Even if your application is not using view state, do not set EnableViewStateMac to false.</span></span> <span data-ttu-id="f728b-188">La valeur false rendre votre application vulnérable aux scripts entre sites.</span><span class="sxs-lookup"><span data-stu-id="f728b-188">Setting this value to false will make your application vulnerable to cross-site scripting.</span></span>

<span data-ttu-id="f728b-189">À partir de ASP.NET 4.5.2, le runtime applique **EnableViewStateMac = true**.</span><span class="sxs-lookup"><span data-stu-id="f728b-189">Starting with ASP.NET 4.5.2, the runtime enforces **EnableViewStateMac=true**.</span></span> <span data-ttu-id="f728b-190">Même si vous le définissez sur false, le runtime ignore cette valeur et se poursuit avec la valeur définie sur true.</span><span class="sxs-lookup"><span data-stu-id="f728b-190">Even if you set it to false, the runtime ignores this value and proceeds with the value set to true.</span></span> <span data-ttu-id="f728b-191">Pour plus d’informations, consultez [ASP.NET 4.5.2 et EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span><span class="sxs-lookup"><span data-stu-id="f728b-191">For more information, see [ASP.NET 4.5.2 and EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span></span>

<span data-ttu-id="f728b-192">L’exemple suivant montre comment définir EnableViewStateMac sur true.</span><span class="sxs-lookup"><span data-stu-id="f728b-192">The following example shows how to set EnableViewStateMac to true.</span></span> <span data-ttu-id="f728b-193">Il est inutile de réellement définir cette valeur sur true, car il est vrai par défaut.</span><span class="sxs-lookup"><span data-stu-id="f728b-193">You do not need to actually set this value to true because it is true by default.</span></span> <span data-ttu-id="f728b-194">Toutefois, si vous avez affectez-lui false sur n’importe quelle page dans votre application, vous devez corriger immédiatement cette valeur.</span><span class="sxs-lookup"><span data-stu-id="f728b-194">However, if you have set it to false on any page in your application, you must immediately correct this value.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a><span data-ttu-id="f728b-195">Confiance moyenne</span><span class="sxs-lookup"><span data-stu-id="f728b-195">Medium trust</span></span>

<span data-ttu-id="f728b-196">Recommandation : Ne dépendent pas de confiance moyenne (ou tout autre niveau de confiance) comme une limite de sécurité.</span><span class="sxs-lookup"><span data-stu-id="f728b-196">Recommendation: Do not depend on Medium Trust (or any other trust level) as a security boundary.</span></span>

<span data-ttu-id="f728b-197">Confiance partielle ne protège pas correctement votre application et ne doit pas être utilisée.</span><span class="sxs-lookup"><span data-stu-id="f728b-197">Partial trust does not adequately protect your application and should not be used.</span></span> <span data-ttu-id="f728b-198">Au lieu de cela, utilisez la confiance totale et isoler les applications non approuvées dans des pools d’applications distincts.</span><span class="sxs-lookup"><span data-stu-id="f728b-198">Instead, use Full Trust, and isolate untrusted applications in separate application pools.</span></span> <span data-ttu-id="f728b-199">Exécutez en outre, chaque pool d’applications sous une identité unique.</span><span class="sxs-lookup"><span data-stu-id="f728b-199">Also, run each application pool under a unique identity.</span></span> <span data-ttu-id="f728b-200">Pour plus d’informations, consultez [ASP.NET de confiance partielle ne garantit pas l’isolation des applications](https://support.microsoft.com/kb/2698981).</span><span class="sxs-lookup"><span data-stu-id="f728b-200">For more information, see [ASP.NET Partial Trust does not guarantee application isolation](https://support.microsoft.com/kb/2698981).</span></span>

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a><span data-ttu-id="f728b-201">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="f728b-201">&lt;appSettings&gt;</span></span>

<span data-ttu-id="f728b-202">Recommandation : Ne désactivez pas les paramètres de sécurité dans &lt;appSettings&gt; élément.</span><span class="sxs-lookup"><span data-stu-id="f728b-202">Recommendation: Do not disable security settings in &lt;appSettings&gt; element.</span></span>

<span data-ttu-id="f728b-203">L’élément appSettings contient de nombreuses valeurs qui sont nécessaires pour les mises à jour de sécurité.</span><span class="sxs-lookup"><span data-stu-id="f728b-203">The appSettings element contains many values which are required for security updates.</span></span> <span data-ttu-id="f728b-204">Vous ne devez pas modifier ou désactiver ces valeurs.</span><span class="sxs-lookup"><span data-stu-id="f728b-204">You should not change or disable these values.</span></span> <span data-ttu-id="f728b-205">Si vous devez désactiver ces valeurs lors du déploiement d’une mise à jour, réactivez immédiatement après avoir effectué le déploiement.</span><span class="sxs-lookup"><span data-stu-id="f728b-205">If you must disable these values when deploying an update, immediately re-enable after completing deployment.</span></span>

<span data-ttu-id="f728b-206">Pour plus d’informations, consultez [élément appSettings ASP.NET](https://msdn.microsoft.com/library/hh975440.aspx).</span><span class="sxs-lookup"><span data-stu-id="f728b-206">For details, see [ASP.NET appSettings Element](https://msdn.microsoft.com/library/hh975440.aspx).</span></span>

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a><span data-ttu-id="f728b-207">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="f728b-207">UrlPathEncode</span></span>

<span data-ttu-id="f728b-208">Recommandation : Utilisez [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) à la place.</span><span class="sxs-lookup"><span data-stu-id="f728b-208">Recommendation: Use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) instead.</span></span>

<span data-ttu-id="f728b-209">La méthode UrlPathEncode a été ajoutée au .NET Framework pour résoudre un problème de compatibilité de navigateur très spécifique.</span><span class="sxs-lookup"><span data-stu-id="f728b-209">The UrlPathEncode method was added to the .NET Framework to resolve a very specific browser compatibility problem.</span></span> <span data-ttu-id="f728b-210">Il n’encode pas correctement une URL et ne protège pas votre application à partir de scripts entre sites.</span><span class="sxs-lookup"><span data-stu-id="f728b-210">It does not adequately encode a URL, and does not protect your application from cross-site scripting.</span></span> <span data-ttu-id="f728b-211">Vous devez jamais l’utiliser dans votre application.</span><span class="sxs-lookup"><span data-stu-id="f728b-211">You should never use it in your application.</span></span> <span data-ttu-id="f728b-212">Au lieu de cela, utilisez [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).</span><span class="sxs-lookup"><span data-stu-id="f728b-212">Instead, use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).</span></span>

<span data-ttu-id="f728b-213">L’exemple suivant montre comment passer une URL encodée comme un paramètre de chaîne de requête pour un contrôle de lien hypertexte.</span><span class="sxs-lookup"><span data-stu-id="f728b-213">The following example shows how to pass an encoded URL as a query string parameter for a hyperlink control.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a><span data-ttu-id="f728b-214">Fiabilité et performances</span><span class="sxs-lookup"><span data-stu-id="f728b-214">Reliability and performance</span></span>

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a><span data-ttu-id="f728b-215">PreSendRequestHeaders et PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="f728b-215">PreSendRequestHeaders and PreSendRequestContent</span></span>

<span data-ttu-id="f728b-216">Recommandation : N’utilisez pas ces événements avec des modules gérés.</span><span class="sxs-lookup"><span data-stu-id="f728b-216">Recommendation: Do not use these events with managed modules.</span></span> <span data-ttu-id="f728b-217">En revanche, écrire un module IIS natif pour effectuer la tâche requise.</span><span class="sxs-lookup"><span data-stu-id="f728b-217">Instead, write a native IIS module to perform the required task.</span></span> <span data-ttu-id="f728b-218">Consultez [création de Modules de Code natif HTTP](https://msdn.microsoft.com/library/ms693629.aspx).</span><span class="sxs-lookup"><span data-stu-id="f728b-218">See [Creating Native-Code HTTP Modules](https://msdn.microsoft.com/library/ms693629.aspx).</span></span>

<span data-ttu-id="f728b-219">Vous pouvez utiliser la [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) et [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) événements avec des modules IIS natifs.</span><span class="sxs-lookup"><span data-stu-id="f728b-219">You can use the [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) and [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) events with native IIS modules.</span></span>
> [!WARNING]
> <span data-ttu-id="f728b-220">N’utilisez pas `PreSendRequestHeaders` et `PreSendRequestContent` avec des modules managés qui implémentent `IHttpModule`.</span><span class="sxs-lookup"><span data-stu-id="f728b-220">Do not use `PreSendRequestHeaders` and `PreSendRequestContent` with managed modules that implement `IHttpModule`.</span></span> <span data-ttu-id="f728b-221">Définition de ces propriétés peut entraîner des problèmes avec les demandes asynchrones.</span><span class="sxs-lookup"><span data-stu-id="f728b-221">Setting these properties can cause issues with asynchronous requests.</span></span> <span data-ttu-id="f728b-222">La combinaison de l’Application demandée Routing (ARR) et websockets peut entraîner des exceptions de violation d’accès qui peuvent provoquer w3wp se bloque.</span><span class="sxs-lookup"><span data-stu-id="f728b-222">The combination of Application Requested Routing (ARR) and websockets might lead to access violation exceptions that can cause w3wp to crash.</span></span> <span data-ttu-id="f728b-223">Par exemple, iiscore ! W3_CONTEXT_BASE::GetIsLastNotification + 68 dans iiscore.dll a provoqué une exception de violation d’accès (0xC0000005).</span><span class="sxs-lookup"><span data-stu-id="f728b-223">For example, iiscore!W3_CONTEXT_BASE::GetIsLastNotification+68 in iiscore.dll has caused an access violation exception (0xC0000005).</span></span>

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a><span data-ttu-id="f728b-224">Événements de page asynchrone avec les web forms</span><span class="sxs-lookup"><span data-stu-id="f728b-224">Asynchronous page events with web forms</span></span>

<span data-ttu-id="f728b-225">Recommandation : Dans Web Forms, éviter d’écrire async void méthodes pour les événements de cycle de vie de Page et à la place utiliser [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) pour le code asynchrone.</span><span class="sxs-lookup"><span data-stu-id="f728b-225">Recommendation: In Web Forms, avoid writing async void methods for Page lifecycle events, and instead use [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) for asynchronous code.</span></span>

<span data-ttu-id="f728b-226">Lorsque vous marquez un événement de page avec **async** et **void**, vous ne peut pas déterminer lorsque le code asynchrone est terminé.</span><span class="sxs-lookup"><span data-stu-id="f728b-226">When you mark a page event with **async** and **void**, you cannot determine when the asynchronous code has finished.</span></span> <span data-ttu-id="f728b-227">Au lieu de cela, utilisez Page.RegisterAsyncTask pour exécuter le code asynchrone d’une manière qui vous permet d’effectuer le suivi de son achèvement.</span><span class="sxs-lookup"><span data-stu-id="f728b-227">Instead, use Page.RegisterAsyncTask to run the asynchronous code in a way that enables you to track its completion.</span></span>

<span data-ttu-id="f728b-228">L’exemple suivant montre un bouton Cliquez sur Gestionnaire qui contient le code asynchrone.</span><span class="sxs-lookup"><span data-stu-id="f728b-228">The following example shows a button click handler that contains asynchronous code.</span></span> <span data-ttu-id="f728b-229">Cet exemple inclut la lecture d’une valeur de chaîne de façon asynchrone, qui est fourni uniquement comme un exemple simplifié d’une tâche asynchrone et non comme une pratique recommandée.</span><span class="sxs-lookup"><span data-stu-id="f728b-229">This example includes reading a string value asynchronously, which is provided only as a simplified example of an asynchronous task and not as a recommended practice.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

<span data-ttu-id="f728b-230">Si vous utilisez des tâches asynchrones, la valeur est le framework cible de runtime Http 4.5 (ou version ultérieure) dans le fichier Web.config.</span><span class="sxs-lookup"><span data-stu-id="f728b-230">If you are using asynchronous Tasks, set the Http runtime target framework to 4.5 (or later) in the Web.config file.</span></span> <span data-ttu-id="f728b-231">Définition de l’infrastructure cible sur 4.5 tours sur le nouveau contexte de synchronisation qui a été ajoutée dans .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="f728b-231">Setting the target framework to 4.5 turns on the new synchronization context that was added in .NET 4.5.</span></span> <span data-ttu-id="f728b-232">Cette valeur est définie par défaut dans les nouveaux projets dans Visual Studio, mais il n’est ne pas être définie si vous travaillez avec un projet existant.</span><span class="sxs-lookup"><span data-stu-id="f728b-232">This value is set by default in new projects in Visual Studio, but is not be set if you are working with an existing project.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a><span data-ttu-id="f728b-233">Déclenchement et oubli de travail</span><span class="sxs-lookup"><span data-stu-id="f728b-233">Fire-and-forget work</span></span>

<span data-ttu-id="f728b-234">Recommandation : Lors du traitement d’une demande au sein d’ASP.NET, éviter le lancement de travail « fire-et-forget » (ces appelant la méthode ThreadPool.QueueUserWorkItem, ou création d’une minuterie qui appelle à plusieurs reprises un délégué).</span><span class="sxs-lookup"><span data-stu-id="f728b-234">Recommendation: When handling a request within ASP.NET, avoid launching fire-and-forget work (such calling the ThreadPool.QueueUserWorkItem method or creating a timer that repeatedly calls a delegate).</span></span>

<span data-ttu-id="f728b-235">Si votre application a du travail « fire-et-forget » qui s’exécute au sein d’ASP.NET, votre application peut être désynchronisée. À tout moment, le domaine d’application peut être détruit ce qui signifie que votre processus en cours ne corresponde plus à l’état actuel de l’application.</span><span class="sxs-lookup"><span data-stu-id="f728b-235">If your application has fire-and-forget work that runs within ASP.NET, your application can get out of sync. At any time, the app domain can be destroyed which means your ongoing process may no longer match the current state of the application.</span></span>

<span data-ttu-id="f728b-236">Vous devez déplacer ce type de travail en dehors d’ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f728b-236">You should move this type of work outside of ASP.NET.</span></span> <span data-ttu-id="f728b-237">Vous pouvez utiliser les tâches Web, Service de Windows ou un rôle de travail dans Azure pour effectuer le travail en cours et exécuter ce code à partir d’un autre processus.</span><span class="sxs-lookup"><span data-stu-id="f728b-237">You can use a Web Jobs, Windows Service or a Worker role in Azure to perform ongoing work, and run that code from another process.</span></span>

<span data-ttu-id="f728b-238">Si vous devez effectuer ce travail au sein d’ASP.NET, vous pouvez ajouter le package Nuget appelé [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) pour exécuter le code.</span><span class="sxs-lookup"><span data-stu-id="f728b-238">If you must perform this work within ASP.NET, you can add the Nuget package called [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) to run the code.</span></span>

<a id="requestentity"></a>

### <a name="request-entity-body"></a><span data-ttu-id="f728b-239">Corps d’entité</span><span class="sxs-lookup"><span data-stu-id="f728b-239">Request entity body</span></span>

<span data-ttu-id="f728b-240">Recommandation : Éviter la lecture de Request.Form ou Request.InputStream avant d’exécuter l’événement du gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="f728b-240">Recommendation: Avoid reading Request.Form or Request.InputStream before the handler's execute event.</span></span>

<span data-ttu-id="f728b-241">Dès que possible vous devez lire à partir de Request.Form ou Request.InputStream est au cours du gestionnaire exécuter l’événement.</span><span class="sxs-lookup"><span data-stu-id="f728b-241">The earliest you should read from Request.Form or Request.InputStream is during the handler's execute event.</span></span> <span data-ttu-id="f728b-242">Dans MVC, le contrôleur est le gestionnaire et l’événement d’exécution est lorsque la méthode d’action s’exécute.</span><span class="sxs-lookup"><span data-stu-id="f728b-242">In MVC, the Controller is the handler and the execute event is when the action method runs.</span></span> <span data-ttu-id="f728b-243">Dans Web Forms, la Page est le gestionnaire et l’événement d’exécution est lorsque l’événement Page.Init se déclenche.</span><span class="sxs-lookup"><span data-stu-id="f728b-243">In Web Forms, the Page is the handler and the execute event is when the Page.Init event fires.</span></span> <span data-ttu-id="f728b-244">Si vous lisez le corps d’entité de demande antérieure à l’événement d’exécution, vous interférer avec le traitement de la demande.</span><span class="sxs-lookup"><span data-stu-id="f728b-244">If you read the request entity body earlier than the execute event, you interfere with the processing of the request.</span></span>

<span data-ttu-id="f728b-245">Si vous avez besoin lire le corps d’entité de demande avant l’événement d’exécution, utilisez [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) ou [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span><span class="sxs-lookup"><span data-stu-id="f728b-245">If you need to read the request entity body before the execute event, use either [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) or [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span></span> <span data-ttu-id="f728b-246">Lorsque vous utilisez GetBufferlessInputStream, vous obtenez le flux de données brutes à partir de la demande et assumer la responsabilité de traiter la requête entière.</span><span class="sxs-lookup"><span data-stu-id="f728b-246">When you use GetBufferlessInputStream, you get the raw stream from the request, and assume responsibility for processing the entire request.</span></span> <span data-ttu-id="f728b-247">Après avoir appelé GetBufferlessInputStream, Request.Form et Request.InputStream ne sont pas disponibles, car ils n’ont pas été remplies par ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f728b-247">After calling GetBufferlessInputStream, Request.Form and Request.InputStream are not available because they have not been populated by ASP.NET.</span></span> <span data-ttu-id="f728b-248">Lorsque vous utilisez GetBufferedInputStream, vous obtenez une copie du flux de données à partir de la demande.</span><span class="sxs-lookup"><span data-stu-id="f728b-248">When you use GetBufferedInputStream, you get a copy of the stream from the request.</span></span> <span data-ttu-id="f728b-249">Request.Form et Request.InputStream sont toujours disponibles plus loin dans la demande, car ASP.NET remplit l’autre exemplaire.</span><span class="sxs-lookup"><span data-stu-id="f728b-249">Request.Form and Request.InputStream are still available later in the request because ASP.NET populates the other copy.</span></span>

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a><span data-ttu-id="f728b-250">Compatibilité entre Response.Redirect et Response.End</span><span class="sxs-lookup"><span data-stu-id="f728b-250">Response.Redirect and Response.End</span></span>

<span data-ttu-id="f728b-251">Recommandation : Tenez compte des différences dans les modalités de thread après avoir appelé [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span><span class="sxs-lookup"><span data-stu-id="f728b-251">Recommendation: Be aware of differences in how thread is handled after calling [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span></span>

<span data-ttu-id="f728b-252">Le [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) méthode appelle la méthode Response.End.</span><span class="sxs-lookup"><span data-stu-id="f728b-252">The [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) method calls the Response.End method.</span></span> <span data-ttu-id="f728b-253">Dans un processus synchrone, appelant Request.Redirect provoque le thread actuel abandonner immédiatement.</span><span class="sxs-lookup"><span data-stu-id="f728b-253">In a synchronous process, calling Request.Redirect causes the current thread to immediately abort.</span></span> <span data-ttu-id="f728b-254">Toutefois, dans un processus asynchrone, l’appel de Response.Redirect n’abandonne pas le thread actuel, afin de l’exécution de code se poursuit pour la demande.</span><span class="sxs-lookup"><span data-stu-id="f728b-254">However, in an asynchronous process, calling Response.Redirect does not abort the current thread, so code execution continues for the request.</span></span> <span data-ttu-id="f728b-255">Dans un processus asynchrone, vous devez retourner la tâche à partir de la méthode pour arrêter l’exécution de code.</span><span class="sxs-lookup"><span data-stu-id="f728b-255">In an asynchronous process, you must return the Task from the method to stop the code execution.</span></span>

<span data-ttu-id="f728b-256">Dans un projet MVC, n’appelez pas Response.Redirect.</span><span class="sxs-lookup"><span data-stu-id="f728b-256">In an MVC project, you should not call Response.Redirect.</span></span> <span data-ttu-id="f728b-257">Au lieu de cela, retourner un RedirectResult.</span><span class="sxs-lookup"><span data-stu-id="f728b-257">Instead, return a RedirectResult.</span></span>

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a><span data-ttu-id="f728b-258">EnableViewState et ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="f728b-258">EnableViewState and ViewStateMode</span></span>

<span data-ttu-id="f728b-259">Recommandation : Utilisez ViewStateMode, au lieu de EnableViewState, pour fournir un contrôle granulaire sur laquelle les contrôles utilisent l’état d’affichage.</span><span class="sxs-lookup"><span data-stu-id="f728b-259">Recommendation: Use ViewStateMode, instead of EnableViewState, to provide granular control over which controls use view state.</span></span>

<span data-ttu-id="f728b-260">Lorsque vous définissez EnableViewState sur false dans la directive de Page, l’état d’affichage est désactivé pour tous les contrôles dans la page et ne peut pas être activée.</span><span class="sxs-lookup"><span data-stu-id="f728b-260">When you set EnableViewState to false in the Page directive, view state is disabled for all controls within the page and cannot be enabled.</span></span> <span data-ttu-id="f728b-261">Si vous souhaitez activer l’état d’affichage que pour certains contrôles dans votre page, la valeur ViewStateMode désactivé pour la Page.</span><span class="sxs-lookup"><span data-stu-id="f728b-261">If you want to enable view state for only certain controls in your page, set ViewStateMode to Disabled for the Page.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

<span data-ttu-id="f728b-262">Ensuite, définissez ViewStateMode Enabled sur uniquement les contrôles qui doivent en fait état d’affichage.</span><span class="sxs-lookup"><span data-stu-id="f728b-262">Then, set ViewStateMode to Enabled on only the controls that actually need view state.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

<span data-ttu-id="f728b-263">En activant l’état d’affichage pour seulement les contrôles qui en ont besoin, vous pouvez réduire la taille de l’état d’affichage pour vos pages web.</span><span class="sxs-lookup"><span data-stu-id="f728b-263">By enabling view state for only the controls that need it, you can shrink the size of the view state for your web pages.</span></span>

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a><span data-ttu-id="f728b-264">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="f728b-264">SqlMembershipProvider</span></span>

<span data-ttu-id="f728b-265">Recommandation : Utilisez les fournisseurs universels.</span><span class="sxs-lookup"><span data-stu-id="f728b-265">Recommendation: Use Universal Providers.</span></span>

<span data-ttu-id="f728b-266">Dans les modèles de projet actuel, SqlMembershipProvider a été remplacé par [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), qui est disponible comme package NuGet.</span><span class="sxs-lookup"><span data-stu-id="f728b-266">In the current project templates, SqlMembershipProvider has been replaced by [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), which is available as a NuGet package.</span></span> <span data-ttu-id="f728b-267">Si vous utilisez SqlMembershipProvider dans un projet qui a été généré avec une version antérieure des modèles, vous devez basculer vers les fournisseurs universels.</span><span class="sxs-lookup"><span data-stu-id="f728b-267">If you are using SqlMembershipProvider in a project that was built with an earlier version of the templates, you should switch to Universal Providers.</span></span> <span data-ttu-id="f728b-268">Les fournisseurs universels fonctionnent avec toutes les bases de données qui sont prises en charge par Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f728b-268">The Universal Providers work with all databases that are supported by Entity Framework.</span></span>

<span data-ttu-id="f728b-269">Pour plus d’informations, consultez [Introducing ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span><span class="sxs-lookup"><span data-stu-id="f728b-269">For more information, see [Introducing ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span></span>

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a><span data-ttu-id="f728b-270">Requêtes longues (> 110 secondes)</span><span class="sxs-lookup"><span data-stu-id="f728b-270">Long-running requests (>110 seconds)</span></span>

<span data-ttu-id="f728b-271">Recommandation : Utilisez [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) ou [SignalR](../../../signalr/index.md) pour les clients connectés et utilisez les opérations d’e/s asynchrones.</span><span class="sxs-lookup"><span data-stu-id="f728b-271">Recommendation: Use [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) or [SignalR](../../../signalr/index.md) for connected clients, and use asynchronous I/O operations.</span></span>

<span data-ttu-id="f728b-272">Requêtes longues peuvent provoquer des résultats imprévisibles et des performances médiocres dans votre application web.</span><span class="sxs-lookup"><span data-stu-id="f728b-272">Long-running requests can cause unpredictable results and poor performance in your web application.</span></span> <span data-ttu-id="f728b-273">Le paramètre de délai d’expiration par défaut pour une demande est de 110 secondes.</span><span class="sxs-lookup"><span data-stu-id="f728b-273">The default timeout setting for a request is 110 seconds.</span></span> <span data-ttu-id="f728b-274">Si vous utilisez l’état de session avec une requête longue, ASP.NET libère le verrou sur l’objet de Session après 110 secondes.</span><span class="sxs-lookup"><span data-stu-id="f728b-274">If you are using session state with a long-running request, ASP.NET will release the lock on the Session object after 110 seconds.</span></span> <span data-ttu-id="f728b-275">Toutefois, votre application peut être au milieu d’une opération sur l’objet de Session lorsque le verrou est libéré, et l’opération ne peut pas terminer correctement.</span><span class="sxs-lookup"><span data-stu-id="f728b-275">However, your application might be in the middle of an operation on the Session object when the lock is released, and the operation might not complete successfully.</span></span> <span data-ttu-id="f728b-276">Si une deuxième requête à partir de l’utilisateur est bloquée pendant l’exécution de la première demande, la deuxième demande peut accéder à l’objet de Session dans un état incohérent.</span><span class="sxs-lookup"><span data-stu-id="f728b-276">If a second request from the user is blocked while the first request is running, the second request might access the Session object in an inconsistent state.</span></span>

<span data-ttu-id="f728b-277">Si votre application inclut des opérations d’e/s bloquantes (ou synchrones), l’application sera ne répond pas.</span><span class="sxs-lookup"><span data-stu-id="f728b-277">If your application includes blocking (or synchronous) I/O operations, the application will be unresponsive.</span></span>

<span data-ttu-id="f728b-278">Pour améliorer les performances, utilisez les opérations d’e/s asynchrones dans le .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f728b-278">To improve performance, use the asynchronous I/O operations in the .NET Framework.</span></span> <span data-ttu-id="f728b-279">En outre, utilisez WebSockets ou SignalR pour les clients qui se connectent au serveur.</span><span class="sxs-lookup"><span data-stu-id="f728b-279">Also, use WebSockets or SignalR for connecting clients to the server.</span></span> <span data-ttu-id="f728b-280">Ces fonctionnalités sont conçues pour gérer efficacement les requêtes longues.</span><span class="sxs-lookup"><span data-stu-id="f728b-280">These features are designed to efficiently handle long-running requests.</span></span>
