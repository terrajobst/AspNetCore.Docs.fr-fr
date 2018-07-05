---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: Éléments à ne pas faire dans ASP.NET et comment réagir à la place | Microsoft Docs
author: tfitzmac
description: Cette rubrique décrit les erreurs courantes plusieurs personnes effectué dans les projets web ASP.NET. Il fournit des recommandations pour la procédure à suivre pour éviter ces Commu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/08/2014
ms.topic: article
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
ms.technology: ''
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: bf46d0b4997d9816071df20fb1884dd76dce8903
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371880"
---
<a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a><span data-ttu-id="300e2-104">Éléments à ne pas faire dans ASP.NET et comment réagir à la place</span><span class="sxs-lookup"><span data-stu-id="300e2-104">What not to do in ASP.NET, and what to do instead</span></span>
====================
<span data-ttu-id="300e2-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="300e2-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="300e2-106">Cette rubrique décrit les erreurs courantes plusieurs personnes effectué dans les projets web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="300e2-106">This topic describes several common mistakes people make within ASP.NET web projects.</span></span> <span data-ttu-id="300e2-107">Il fournit des recommandations pour la procédure à suivre pour éviter ces erreurs courantes.</span><span class="sxs-lookup"><span data-stu-id="300e2-107">It provides recommendations for what you should do to avoid these common mistakes.</span></span> <span data-ttu-id="300e2-108">Il est basé sur un [présentation](http://vimeo.com/68390507) par **Damian Edwards** à couronne conférence de développeurs.</span><span class="sxs-lookup"><span data-stu-id="300e2-108">It is based on a [presentation](http://vimeo.com/68390507) by **Damian Edwards** at Norwegian Developers Conference.</span></span>


## <a name="disclaimer"></a><span data-ttu-id="300e2-109">Exclusion de responsabilité</span><span class="sxs-lookup"><span data-stu-id="300e2-109">Disclaimer</span></span>

<span data-ttu-id="300e2-110">Cette rubrique n'est pas conçue comme un guide complet pour vous assurer de que votre application est sécurisé et efficace.</span><span class="sxs-lookup"><span data-stu-id="300e2-110">This topic is not intended as a complete guide to ensure your application is secure and efficient.</span></span> <span data-ttu-id="300e2-111">Vous devez toujours suivre les meilleures pratiques pour la sécurité et de performances qui ne sont pas décrites dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="300e2-111">You still need to follow best practices for security and performance that are not outlined in this topic.</span></span> <span data-ttu-id="300e2-112">Il suggère uniquement comment éviter les erreurs courantes liées aux processus et des classes .NET.</span><span class="sxs-lookup"><span data-stu-id="300e2-112">It only suggests how to avoid common mistakes related to .NET classes and processes.</span></span>

## <a name="overview"></a><span data-ttu-id="300e2-113">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="300e2-113">Overview</span></span>

<span data-ttu-id="300e2-114">Cette rubrique contient les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="300e2-114">This topic contains the following sections:</span></span>

- [<span data-ttu-id="300e2-115">Conformité aux normes</span><span class="sxs-lookup"><span data-stu-id="300e2-115">Standards Compliance</span></span>](#standards)

    - [<span data-ttu-id="300e2-116">Adaptateurs de contrôle</span><span class="sxs-lookup"><span data-stu-id="300e2-116">Control Adapters</span></span>](#adapters)
    - [<span data-ttu-id="300e2-117">Propriétés de style des contrôles</span><span class="sxs-lookup"><span data-stu-id="300e2-117">Style Properties on Controls</span></span>](#styleprop)
    - [<span data-ttu-id="300e2-118">Page et les rappels de contrôle</span><span class="sxs-lookup"><span data-stu-id="300e2-118">Page and Control Callbacks</span></span>](#callback)
    - [<span data-ttu-id="300e2-119">Détection de fonctionnalité de navigateur</span><span class="sxs-lookup"><span data-stu-id="300e2-119">Browser Capability Detection</span></span>](#browsercap)
- [<span data-ttu-id="300e2-120">Sécurité</span><span class="sxs-lookup"><span data-stu-id="300e2-120">Security</span></span>](#security)

    - [<span data-ttu-id="300e2-121">Validation de la demande</span><span class="sxs-lookup"><span data-stu-id="300e2-121">Request Validation</span></span>](#validation)
    - [<span data-ttu-id="300e2-122">Session et l’authentification par formulaire sans cookie</span><span class="sxs-lookup"><span data-stu-id="300e2-122">Cookieless Forms Authentication and Session</span></span>](#cookieless)
    - [<span data-ttu-id="300e2-123">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="300e2-123">EnableViewStateMac</span></span>](#viewstatemac)
    - [<span data-ttu-id="300e2-124">Confiance moyenne</span><span class="sxs-lookup"><span data-stu-id="300e2-124">Medium Trust</span></span>](#medium)
    - [<span data-ttu-id="300e2-125">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="300e2-125">&lt;appSettings&gt;</span></span>](#appsettings)
    - [<span data-ttu-id="300e2-126">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="300e2-126">UrlPathEncode</span></span>](#urlpathencode)
- [<span data-ttu-id="300e2-127">Fiabilité et performances</span><span class="sxs-lookup"><span data-stu-id="300e2-127">Reliability and Performance</span></span>](#performance)

    - [<span data-ttu-id="300e2-128">PreSendRequestHeaders et PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="300e2-128">PreSendRequestHeaders and PreSendRequestContent</span></span>](#presend)
    - [<span data-ttu-id="300e2-129">Événements de Page asynchrone avec les Web Forms</span><span class="sxs-lookup"><span data-stu-id="300e2-129">Asynchronous Page Events with Web Forms</span></span>](#asyncevents)
    - [<span data-ttu-id="300e2-130">Déclenchement et oubli de travail</span><span class="sxs-lookup"><span data-stu-id="300e2-130">Fire-and-Forget Work</span></span>](#fire)
    - [<span data-ttu-id="300e2-131">Corps d’entité</span><span class="sxs-lookup"><span data-stu-id="300e2-131">Request Entity Body</span></span>](#requestentity)
    - [<span data-ttu-id="300e2-132">Compatibilité entre Response.Redirect et Response.End</span><span class="sxs-lookup"><span data-stu-id="300e2-132">Response.Redirect and Response.End</span></span>](#redirect)
    - [<span data-ttu-id="300e2-133">EnableViewState et ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="300e2-133">EnableViewState and ViewStateMode</span></span>](#viewstatemode)
    - [<span data-ttu-id="300e2-134">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="300e2-134">SqlMembershipProvider</span></span>](#sqlprovider)
    - [<span data-ttu-id="300e2-135">Les longues demandes en cours d’exécution (> 110 secondes)</span><span class="sxs-lookup"><span data-stu-id="300e2-135">Long Running Requests (>110 seconds)</span></span>](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a><span data-ttu-id="300e2-136">Conformité aux normes</span><span class="sxs-lookup"><span data-stu-id="300e2-136">Standards Compliance</span></span>

<a id="adapters"></a>

### <a name="control-adapters"></a><span data-ttu-id="300e2-137">Adaptateurs de contrôle</span><span class="sxs-lookup"><span data-stu-id="300e2-137">Control Adapters</span></span>

<span data-ttu-id="300e2-138">Recommandation : Arrêtez à l’aide des adaptateurs de contrôle pour le rendu adaptatif et utiliser à la place des requêtes de média CSS et HTML conforme aux normes.</span><span class="sxs-lookup"><span data-stu-id="300e2-138">Recommendation: Stop using control adapters for adaptive rendering, and instead use CSS media queries and standards-compliant HTML.</span></span>

<span data-ttu-id="300e2-139">Les adaptateurs de contrôles ont été introduites dans .NET 2.0 pour restituer le code de présentation qui a été personnalisé pour les environnements et les différents appareils.</span><span class="sxs-lookup"><span data-stu-id="300e2-139">Controls Adapters were introduced in .NET 2.0 to render presentation code that was customized for different devices and environments.</span></span> <span data-ttu-id="300e2-140">À présent, ce rendu adaptatif peut être effectué avec CSS et HTML.</span><span class="sxs-lookup"><span data-stu-id="300e2-140">Now, this adaptive rendering can be accomplished with CSS and HTML.</span></span> <span data-ttu-id="300e2-141">Vous devez cesser d’utiliser les adaptateurs de contrôle et convertir les adaptateurs existants vers CSS et HTML.</span><span class="sxs-lookup"><span data-stu-id="300e2-141">You should stop using Control Adapters and convert any existing adapters to CSS and HTML.</span></span>

<span data-ttu-id="300e2-142">Pour plus d’informations, consultez [les requêtes de média](http://www.w3.org/TR/css3-mediaqueries/) et [Comment : ajouter des Pages mobiles à vos pages Web Forms ASP.NET / MVC Application](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="300e2-142">For more information, see [Media Queries](http://www.w3.org/TR/css3-mediaqueries/) and [How To: Add Mobile Pages to Your ASP.NET Web Forms / MVC Application](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span></span>

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a><span data-ttu-id="300e2-143">Propriétés de style des contrôles</span><span class="sxs-lookup"><span data-stu-id="300e2-143">Style Properties on Controls</span></span>

<span data-ttu-id="300e2-144">Recommandation : Arrêter de valeurs de style dans le balisage du contrôle et définir à la place des valeurs de mise en forme dans les feuilles de style CSS.</span><span class="sxs-lookup"><span data-stu-id="300e2-144">Recommendation: Stop setting style values in the control markup, and instead set formatting values in CSS stylesheets.</span></span>

<span data-ttu-id="300e2-145">Les contrôles serveur Web contiennent des dizaines de propriétés qui peuvent être utilisées pour définir les propriétés de style intraligne.</span><span class="sxs-lookup"><span data-stu-id="300e2-145">Web server controls contain dozens of properties which can be used to set in-line style properties.</span></span> <span data-ttu-id="300e2-146">Par exemple, la propriété ForeColor définit la couleur du texte pour un contrôle.</span><span class="sxs-lookup"><span data-stu-id="300e2-146">For example, the ForeColor property sets the color of the text for a control.</span></span> <span data-ttu-id="300e2-147">Vous pouvez obtenir cet effet même plus efficacement par le biais de feuilles de style CSS.</span><span class="sxs-lookup"><span data-stu-id="300e2-147">You can accomplish this same effect more efficiently through CSS stylesheets.</span></span> <span data-ttu-id="300e2-148">Feuilles de style permettent de centraliser les valeurs de style et évitez de définir ces valeurs dans toute votre application.</span><span class="sxs-lookup"><span data-stu-id="300e2-148">Stylesheets enable you to centralize style values and avoid setting these values throughout your application.</span></span>

<span data-ttu-id="300e2-149">L’exemple suivant montre une classe CSS le texte de jeux en rouge.</span><span class="sxs-lookup"><span data-stu-id="300e2-149">The following example shows a CSS class the sets text to red.</span></span>

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

<span data-ttu-id="300e2-150">L’exemple suivant montre comment appliquer de manière dynamique la classe CSS.</span><span class="sxs-lookup"><span data-stu-id="300e2-150">The next example shows how to dynamically apply the CSS class.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a><span data-ttu-id="300e2-151">Page et les rappels de contrôle</span><span class="sxs-lookup"><span data-stu-id="300e2-151">Page and Control Callbacks</span></span>

<span data-ttu-id="300e2-152">Recommandation : Arrêter d’utiliser les rappels de page et de contrôle et à la place utiliser les éléments suivants : AJAX, UpdatePanel, méthodes d’action MVC, API Web ou SignalR.</span><span class="sxs-lookup"><span data-stu-id="300e2-152">Recommendation: Stop using page and control callbacks, and instead use any of the following: AJAX, UpdatePanel, MVC action methods, Web API, or SignalR.</span></span>

<span data-ttu-id="300e2-153">Dans les versions antérieures d’ASP.NET, Page et contrôle des méthodes de rappel vous autorisé à mettre à jour de la partie de la page web sans actualiser une page entière.</span><span class="sxs-lookup"><span data-stu-id="300e2-153">In earlier versions of ASP.NET, Page and Control callback methods enabled you to update part of the web page without refreshing an entire page.</span></span> <span data-ttu-id="300e2-154">Vous pouvez désormais effectuer des mises à jour de page partielle via [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [API Web](../../../web-api/index.md) ou [SignalR](../../../signalr/index.md).</span><span class="sxs-lookup"><span data-stu-id="300e2-154">You can now accomplish partial-page updates through [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [Web API](../../../web-api/index.md) or [SignalR](../../../signalr/index.md).</span></span> <span data-ttu-id="300e2-155">Vous devez arrêter à l’aide de méthodes de rappel car ils peuvent provoquer des problèmes avec des URL conviviales et le routage.</span><span class="sxs-lookup"><span data-stu-id="300e2-155">You should stop using callback methods because they can cause issues with friendly URLs and routing.</span></span> <span data-ttu-id="300e2-156">Par défaut, les contrôles ne permettent pas de méthodes de rappel, mais si vous avez activé cette fonctionnalité dans un contrôle, vous devez le désactiver.</span><span class="sxs-lookup"><span data-stu-id="300e2-156">By default, controls do not enable callback methods, but if you enabled this feature in a control, you should disable it.</span></span>

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a><span data-ttu-id="300e2-157">Détection de fonctionnalité de navigateur</span><span class="sxs-lookup"><span data-stu-id="300e2-157">Browser Capability Detection</span></span>

<span data-ttu-id="300e2-158">Recommandation : Arrêtez à l’aide de la détection de fonctionnalité de navigateur statique et à la place utiliser la détection de fonctionnalité dynamique.</span><span class="sxs-lookup"><span data-stu-id="300e2-158">Recommendation: Stop using static browser capability detection, and instead use dynamic feature detection.</span></span>

<span data-ttu-id="300e2-159">Dans les versions antérieures d’ASP.NET, les fonctionnalités prises en charge pour chaque navigateur ont été stockées dans un fichier XML.</span><span class="sxs-lookup"><span data-stu-id="300e2-159">In earlier versions of ASP.NET, the supported features for each browser were stored in an XML file.</span></span> <span data-ttu-id="300e2-160">Prise en charge des fonctionnalités Détection via une recherche statique n’est pas la meilleure approche.</span><span class="sxs-lookup"><span data-stu-id="300e2-160">Detecting feature support through a static lookup is not the best approach.</span></span> <span data-ttu-id="300e2-161">Maintenant, vous pouvez détecter dynamiquement un navigateur de fonctionnalités prises en charge en utilisant une infrastructure de détection de fonctionnalité, telle que [Modernizr](http://modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="300e2-161">Now, you can dynamically detect a browser's supported features by using a feature detection framework, such as [Modernizr](http://modernizr.com/).</span></span> <span data-ttu-id="300e2-162">Détection de fonctionnalité détermine la prise en charge en tente d’utiliser une méthode ou propriété, puis cochez pour voir si le navigateur a produit le résultat souhaité.</span><span class="sxs-lookup"><span data-stu-id="300e2-162">Feature detection determines support by attempting to use a method or property and then checking to see if the browser produced the desired result.</span></span> <span data-ttu-id="300e2-163">Par défaut, Modernizr est inclus dans les modèles d’application Web.</span><span class="sxs-lookup"><span data-stu-id="300e2-163">By default, Modernizr is included in the Web application templates.</span></span>

<a id="security"></a>

## <a name="security"></a><span data-ttu-id="300e2-164">Sécurité</span><span class="sxs-lookup"><span data-stu-id="300e2-164">Security</span></span>

<a id="validation"></a>

### <a name="request-validation"></a><span data-ttu-id="300e2-165">Validation de la demande</span><span class="sxs-lookup"><span data-stu-id="300e2-165">Request Validation</span></span>

<span data-ttu-id="300e2-166">Recommandation : Valider l’entrée utilisateur et encoder une sortie à partir des utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="300e2-166">Recommendation: Validate user input, and encode output from users.</span></span>

<span data-ttu-id="300e2-167">Validation de la demande est une fonctionnalité d’ASP.NET qui inspecte chaque requête et s’arrête à la demande si une menace perçue est trouvée.</span><span class="sxs-lookup"><span data-stu-id="300e2-167">Request validation is a feature of ASP.NET that inspects each request and stops the request if a perceived threat is found.</span></span> <span data-ttu-id="300e2-168">Ne dépendent pas de validation de la demande pour la sécurisation de votre application contre les attaques de script entre sites.</span><span class="sxs-lookup"><span data-stu-id="300e2-168">Do not depend on request validation for securing your application against cross-site scripting attacks.</span></span> <span data-ttu-id="300e2-169">Au lieu de cela, validez toutes les entrées des utilisateurs et encoder la sortie.</span><span class="sxs-lookup"><span data-stu-id="300e2-169">Instead, validate all input from users and encode the output.</span></span> <span data-ttu-id="300e2-170">Dans certains cas, vous pouvez utiliser des expressions régulières pour valider l’entrée, mais dans les cas plus complexes, vous devez valider l’entrée utilisateur à l’aide de classes .NET qui déterminent si la valeur correspond à des valeurs autorisées.</span><span class="sxs-lookup"><span data-stu-id="300e2-170">In some limited cases, you can use regular expressions to validate the input, but in more complicated cases you should validate user input by using .NET classes that determine if the value matches allowed values.</span></span>

<span data-ttu-id="300e2-171">L’exemple suivant montre comment utiliser une méthode statique dans la classe Uri pour déterminer si l’Uri fourni par un utilisateur est valide.</span><span class="sxs-lookup"><span data-stu-id="300e2-171">The following example shows how to use a static method in the Uri class to determine whether the Uri provided by a user is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

<span data-ttu-id="300e2-172">Toutefois, pour vérifier suffisamment l’Uri, vous devez également vérifier pour vous assurer qu’il spécifie `http` ou `https`.</span><span class="sxs-lookup"><span data-stu-id="300e2-172">However, to sufficiently verify the Uri, you should also check to make sure it specifies `http` or `https`.</span></span> <span data-ttu-id="300e2-173">L’exemple suivant utilise les méthodes d’instance pour vérifier que l’Uri est valide.</span><span class="sxs-lookup"><span data-stu-id="300e2-173">The following example uses instance methods to verify that the Uri is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

<span data-ttu-id="300e2-174">Avant le rendu des entrées d’utilisateur au format HTML ou d’inclure l’entrée d’utilisateur dans une requête SQL, encoder les valeurs pour garantir un code malveillant n’est pas inclus.</span><span class="sxs-lookup"><span data-stu-id="300e2-174">Before rendering user input as HTML or including user input in a SQL query, encode the values to ensure malicious code is not included.</span></span>

<span data-ttu-id="300e2-175">Vous pouvez HTML encoder la valeur dans le balisage avec la &lt;% : %&gt; syntaxe, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="300e2-175">You can HTML encode the value in markup with the &lt;%: %&gt; syntax, as shown below.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

<span data-ttu-id="300e2-176">Ou, dans la syntaxe Razor, vous pouvez HTML Encoder avec @, comme illustré ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="300e2-176">Or, in Razor syntax, you can HTML encode with @, as shown below.</span></span>

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="300e2-177">L’exemple suivant montre comment au format HTML encode une valeur dans le code-behind.</span><span class="sxs-lookup"><span data-stu-id="300e2-177">The next example shows how to HTML encode a value in code-behind.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

<span data-ttu-id="300e2-178">Pour encoder en toute sécurité une valeur pour les commandes SQL, utilisez les paramètres de commande tels que le [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx).</span><span class="sxs-lookup"><span data-stu-id="300e2-178">To safely encode a value for SQL commands, use command parameters such as the [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx).</span></span> <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a><span data-ttu-id="300e2-179">Session et l’authentification par formulaire sans cookie</span><span class="sxs-lookup"><span data-stu-id="300e2-179">Cookieless Forms Authentication and Session</span></span>

<span data-ttu-id="300e2-180">Recommandation : Recours à des cookies.</span><span class="sxs-lookup"><span data-stu-id="300e2-180">Recommendation: Require cookies.</span></span>

<span data-ttu-id="300e2-181">En passant les informations d’authentification dans la chaîne de requête n’est pas sécurisée.</span><span class="sxs-lookup"><span data-stu-id="300e2-181">Passing authentication information in the query string is not secure.</span></span> <span data-ttu-id="300e2-182">Par conséquent, requièrent des cookies lorsque votre application inclut une authentification.</span><span class="sxs-lookup"><span data-stu-id="300e2-182">Therefore, require cookies when your application includes authentication.</span></span> <span data-ttu-id="300e2-183">Si votre cookie stocke des informations sensibles, envisagez d’exiger SSL pour le cookie.</span><span class="sxs-lookup"><span data-stu-id="300e2-183">If your cookie stores sensitive information, consider requiring SSL for the cookie.</span></span>

<span data-ttu-id="300e2-184">L’exemple suivant montre comment spécifier dans le fichier Web.config que l’authentification par formulaire requiert un cookie qui est transmis via le protocole SSL.</span><span class="sxs-lookup"><span data-stu-id="300e2-184">The following example shows how to specify in the Web.config file that Forms Authentication requires a cookie that is transmitted over SSL.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a><span data-ttu-id="300e2-185">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="300e2-185">EnableViewStateMac</span></span>

<span data-ttu-id="300e2-186">Recommandation : Jamais a la valeur false.</span><span class="sxs-lookup"><span data-stu-id="300e2-186">Recommendation: Never set to false.</span></span>

<span data-ttu-id="300e2-187">Par défaut, EnbableViewStateMac est définie sur true.</span><span class="sxs-lookup"><span data-stu-id="300e2-187">By default, EnbableViewStateMac is set to true.</span></span> <span data-ttu-id="300e2-188">Même si votre application n’utilise pas l’état d’affichage, ne définissez pas EnableViewStateMac sur false.</span><span class="sxs-lookup"><span data-stu-id="300e2-188">Even if your application is not using view state, do not set EnableViewStateMac to false.</span></span> <span data-ttu-id="300e2-189">La valeur false rendre votre application vulnérable aux scripts entre sites.</span><span class="sxs-lookup"><span data-stu-id="300e2-189">Setting this value to false will make your application vulnerable to cross-site scripting.</span></span>

<span data-ttu-id="300e2-190">À partir de ASP.NET 4.5.2, le runtime applique **EnableViewStateMac = true**.</span><span class="sxs-lookup"><span data-stu-id="300e2-190">Starting with ASP.NET 4.5.2, the runtime enforces **EnableViewStateMac=true**.</span></span> <span data-ttu-id="300e2-191">Même si vous le définissez sur false, le runtime ignore cette valeur et se poursuit avec la valeur définie sur true.</span><span class="sxs-lookup"><span data-stu-id="300e2-191">Even if you set it to false, the runtime ignores this value and proceeds with the value set to true.</span></span> <span data-ttu-id="300e2-192">Pour plus d’informations, consultez [ASP.NET 4.5.2 et EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span><span class="sxs-lookup"><span data-stu-id="300e2-192">For more information, see [ASP.NET 4.5.2 and EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span></span>

<span data-ttu-id="300e2-193">L’exemple suivant montre comment définir EnableViewStateMac sur true.</span><span class="sxs-lookup"><span data-stu-id="300e2-193">The following example shows how to set EnableViewStateMac to true.</span></span> <span data-ttu-id="300e2-194">Il est inutile de réellement définir cette valeur sur true, car il est vrai par défaut.</span><span class="sxs-lookup"><span data-stu-id="300e2-194">You do not need to actually set this value to true because it is true by default.</span></span> <span data-ttu-id="300e2-195">Toutefois, si vous avez affectez-lui false sur n’importe quelle page dans votre application, vous devez corriger immédiatement cette valeur.</span><span class="sxs-lookup"><span data-stu-id="300e2-195">However, if you have set it to false on any page in your application, you must immediately correct this value.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a><span data-ttu-id="300e2-196">Confiance moyenne</span><span class="sxs-lookup"><span data-stu-id="300e2-196">Medium Trust</span></span>

<span data-ttu-id="300e2-197">Recommandation : Ne dépendent pas de confiance moyenne (ou tout autre niveau de confiance) comme une limite de sécurité.</span><span class="sxs-lookup"><span data-stu-id="300e2-197">Recommendation: Do not depend on Medium Trust (or any other trust level) as a security boundary.</span></span>

<span data-ttu-id="300e2-198">Confiance partielle ne protège pas correctement votre application et ne doit pas être utilisée.</span><span class="sxs-lookup"><span data-stu-id="300e2-198">Partial trust does not adequately protect your application and should not be used.</span></span> <span data-ttu-id="300e2-199">Au lieu de cela, utilisez la confiance totale et isoler les applications non approuvées dans des pools d’applications distincts.</span><span class="sxs-lookup"><span data-stu-id="300e2-199">Instead, use Full Trust, and isolate untrusted applications in separate application pools.</span></span> <span data-ttu-id="300e2-200">Exécutez en outre, chaque pool d’applications sous une identité unique.</span><span class="sxs-lookup"><span data-stu-id="300e2-200">Also, run each application pool under a unique identity.</span></span> <span data-ttu-id="300e2-201">Pour plus d’informations, consultez [ASP.NET de confiance partielle ne garantit pas l’isolation des applications](https://support.microsoft.com/kb/2698981).</span><span class="sxs-lookup"><span data-stu-id="300e2-201">For more information, see [ASP.NET Partial Trust does not guarantee application isolation](https://support.microsoft.com/kb/2698981).</span></span>

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a><span data-ttu-id="300e2-202">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="300e2-202">&lt;appSettings&gt;</span></span>

<span data-ttu-id="300e2-203">Recommandation : Ne désactivez pas les paramètres de sécurité dans &lt;appSettings&gt; élément.</span><span class="sxs-lookup"><span data-stu-id="300e2-203">Recommendation: Do not disable security settings in &lt;appSettings&gt; element.</span></span>

<span data-ttu-id="300e2-204">L’élément appSettings contient de nombreuses valeurs qui sont nécessaires pour les mises à jour de sécurité.</span><span class="sxs-lookup"><span data-stu-id="300e2-204">The appSettings element contains many values which are required for security updates.</span></span> <span data-ttu-id="300e2-205">Vous ne devez pas modifier ou désactiver ces valeurs.</span><span class="sxs-lookup"><span data-stu-id="300e2-205">You should not change or disable these values.</span></span> <span data-ttu-id="300e2-206">Si vous devez désactiver ces valeurs lors du déploiement d’une mise à jour, réactivez immédiatement après avoir effectué le déploiement.</span><span class="sxs-lookup"><span data-stu-id="300e2-206">If you must disable these values when deploying an update, immediately re-enable after completing deployment.</span></span>

<span data-ttu-id="300e2-207">Pour plus d’informations, consultez [élément appSettings ASP.NET](https://msdn.microsoft.com/library/hh975440.aspx).</span><span class="sxs-lookup"><span data-stu-id="300e2-207">For details, see [ASP.NET appSettings Element](https://msdn.microsoft.com/library/hh975440.aspx).</span></span>

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a><span data-ttu-id="300e2-208">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="300e2-208">UrlPathEncode</span></span>

<span data-ttu-id="300e2-209">Recommandation : Utilisez [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) à la place.</span><span class="sxs-lookup"><span data-stu-id="300e2-209">Recommendation: Use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) instead.</span></span>

<span data-ttu-id="300e2-210">La méthode UrlPathEncode a été ajoutée au .NET Framework pour résoudre un problème de compatibilité de navigateur très spécifique.</span><span class="sxs-lookup"><span data-stu-id="300e2-210">The UrlPathEncode method was added to the .NET Framework to resolve a very specific browser compatibility problem.</span></span> <span data-ttu-id="300e2-211">Il n’encode pas correctement une URL et ne protège pas votre application à partir de scripts entre sites.</span><span class="sxs-lookup"><span data-stu-id="300e2-211">It does not adequately encode a URL, and does not protect your application from cross-site scripting.</span></span> <span data-ttu-id="300e2-212">Vous devez jamais l’utiliser dans votre application.</span><span class="sxs-lookup"><span data-stu-id="300e2-212">You should never use it in your application.</span></span> <span data-ttu-id="300e2-213">Au lieu de cela, utilisez [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).</span><span class="sxs-lookup"><span data-stu-id="300e2-213">Instead, use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).</span></span>

<span data-ttu-id="300e2-214">L’exemple suivant montre comment passer une URL encodée comme un paramètre de chaîne de requête pour un contrôle de lien hypertexte.</span><span class="sxs-lookup"><span data-stu-id="300e2-214">The following example shows how to pass an encoded URL as a query string parameter for a hyperlink control.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a><span data-ttu-id="300e2-215">Fiabilité et performances</span><span class="sxs-lookup"><span data-stu-id="300e2-215">Reliability and Performance</span></span>

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a><span data-ttu-id="300e2-216">PreSendRequestHeaders et PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="300e2-216">PreSendRequestHeaders and PreSendRequestContent</span></span>

<span data-ttu-id="300e2-217">Recommandation : N’utilisez pas ces événements avec des modules gérés.</span><span class="sxs-lookup"><span data-stu-id="300e2-217">Recommendation: Do not use these events with managed modules.</span></span> <span data-ttu-id="300e2-218">En revanche, écrire un module IIS natif pour effectuer la tâche requise.</span><span class="sxs-lookup"><span data-stu-id="300e2-218">Instead, write a native IIS module to perform the required task.</span></span> <span data-ttu-id="300e2-219">Consultez [création de Modules de Code natif HTTP](https://msdn.microsoft.com/library/ms693629.aspx).</span><span class="sxs-lookup"><span data-stu-id="300e2-219">See [Creating Native-Code HTTP Modules](https://msdn.microsoft.com/library/ms693629.aspx).</span></span>

<span data-ttu-id="300e2-220">Vous pouvez utiliser la [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) et [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) événements avec des modules IIS natifs.</span><span class="sxs-lookup"><span data-stu-id="300e2-220">You can use the [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) and [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) events with native IIS modules.</span></span>
> [!WARNING]
> <span data-ttu-id="300e2-221">N’utilisez pas `PreSendRequestHeaders` et `PreSendRequestContent` avec des modules managés qui implémentent `IHttpModule`.</span><span class="sxs-lookup"><span data-stu-id="300e2-221">Do not use `PreSendRequestHeaders` and `PreSendRequestContent` with managed modules that implement `IHttpModule`.</span></span> <span data-ttu-id="300e2-222">Définition de ces propriétés peut entraîner des problèmes avec les demandes asynchrones.</span><span class="sxs-lookup"><span data-stu-id="300e2-222">Setting these properties can cause issues with asynchronous requests.</span></span> <span data-ttu-id="300e2-223">La combinaison de l’Application demandée Routing (ARR) et websockets peut entraîner des exceptions de violation d’accès qui peuvent provoquer w3wp se bloque.</span><span class="sxs-lookup"><span data-stu-id="300e2-223">The combination of Application Requested Routing (ARR) and websockets might lead to access violation exceptions that can cause w3wp to crash.</span></span> <span data-ttu-id="300e2-224">Par exemple, iiscore ! W3_CONTEXT_BASE::GetIsLastNotification + 68 dans iiscore.dll a provoqué une exception de violation d’accès (0xC0000005).</span><span class="sxs-lookup"><span data-stu-id="300e2-224">For example, iiscore!W3_CONTEXT_BASE::GetIsLastNotification+68 in iiscore.dll has caused an access violation exception (0xC0000005).</span></span>

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a><span data-ttu-id="300e2-225">Événements de Page asynchrone avec les Web Forms</span><span class="sxs-lookup"><span data-stu-id="300e2-225">Asynchronous Page Events with Web Forms</span></span>

<span data-ttu-id="300e2-226">Recommandation : Dans Web Forms, éviter d’écrire async void méthodes pour les événements de cycle de vie de Page et à la place utiliser [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) pour le code asynchrone.</span><span class="sxs-lookup"><span data-stu-id="300e2-226">Recommendation: In Web Forms, avoid writing async void methods for Page lifecycle events, and instead use [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) for asynchronous code.</span></span>

<span data-ttu-id="300e2-227">Lorsque vous marquez un événement de page avec **async** et **void**, vous ne peut pas déterminer lorsque le code asynchrone est terminé.</span><span class="sxs-lookup"><span data-stu-id="300e2-227">When you mark a page event with **async** and **void**, you cannot determine when the asynchronous code has finished.</span></span> <span data-ttu-id="300e2-228">Au lieu de cela, utilisez Page.RegisterAsyncTask pour exécuter le code asynchrone d’une manière qui vous permet d’effectuer le suivi de son achèvement.</span><span class="sxs-lookup"><span data-stu-id="300e2-228">Instead, use Page.RegisterAsyncTask to run the asynchronous code in a way that enables you to track its completion.</span></span>

<span data-ttu-id="300e2-229">L’exemple suivant montre un bouton Cliquez sur Gestionnaire qui contient le code asynchrone.</span><span class="sxs-lookup"><span data-stu-id="300e2-229">The following example shows a button click handler that contains asynchronous code.</span></span> <span data-ttu-id="300e2-230">Cet exemple inclut la lecture d’une valeur de chaîne de façon asynchrone, qui est fourni uniquement comme un exemple simplifié d’une tâche asynchrone et non comme une pratique recommandée.</span><span class="sxs-lookup"><span data-stu-id="300e2-230">This example includes reading a string value asynchronously, which is provided only as a simplified example of an asynchronous task and not as a recommended practice.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

<span data-ttu-id="300e2-231">Si vous utilisez des tâches asynchrones, la valeur est le framework cible de runtime Http 4.5 dans le fichier Web.config.</span><span class="sxs-lookup"><span data-stu-id="300e2-231">If you are using asynchronous Tasks, set the Http runtime target framework to 4.5 in the Web.config file.</span></span> <span data-ttu-id="300e2-232">Définition de l’infrastructure cible sur 4.5 tours sur le nouveau contexte de synchronisation qui a été ajoutée dans .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="300e2-232">Setting the target framework to 4.5 turns on the new synchronization context that was added in .NET 4.5.</span></span> <span data-ttu-id="300e2-233">Cette valeur est définie par défaut dans les nouveaux projets dans Visual Studio 2012, mais n’est ne pas être définie si vous travaillez avec un projet existant.</span><span class="sxs-lookup"><span data-stu-id="300e2-233">This value is set by default in new projects in Visual Studio 2012, but is not be set if you are working with an existing project.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a><span data-ttu-id="300e2-234">Déclenchement et oubli de travail</span><span class="sxs-lookup"><span data-stu-id="300e2-234">Fire-and-Forget Work</span></span>

<span data-ttu-id="300e2-235">Recommandation : Lors du traitement d’une demande au sein d’ASP.NET, éviter le lancement de travail « fire-et-forget » (ces appelant la méthode ThreadPool.QueueUserWorkItem, ou création d’une minuterie qui appelle à plusieurs reprises un délégué).</span><span class="sxs-lookup"><span data-stu-id="300e2-235">Recommendation: When handling a request within ASP.NET, avoid launching fire-and-forget work (such calling the ThreadPool.QueueUserWorkItem method or creating a timer that repeatedly calls a delegate).</span></span>

<span data-ttu-id="300e2-236">Si votre application a du travail « fire-et-forget » qui s’exécute au sein d’ASP.NET, votre application peut être désynchronisée. À tout moment, le domaine d’application peut être détruit ce qui signifie que votre processus en cours ne corresponde plus à l’état actuel de l’application.</span><span class="sxs-lookup"><span data-stu-id="300e2-236">If your application has fire-and-forget work that runs within ASP.NET, your application can get out of sync. At any time, the app domain can be destroyed which means your ongoing process may no longer match the current state of the application.</span></span>

<span data-ttu-id="300e2-237">Vous devez déplacer ce type de travail en dehors d’ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="300e2-237">You should move this type of work outside of ASP.NET.</span></span> <span data-ttu-id="300e2-238">Vous pouvez utiliser les tâches Web, Service de Windows ou un rôle de travail dans Azure pour effectuer le travail en cours et exécuter ce code à partir d’un autre processus.</span><span class="sxs-lookup"><span data-stu-id="300e2-238">You can use a Web Jobs, Windows Service or a Worker role in Azure to perform ongoing work, and run that code from another process.</span></span>

<span data-ttu-id="300e2-239">Si vous devez effectuer ce travail au sein d’ASP.NET, vous pouvez ajouter le package Nuget appelé [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) pour exécuter le code.</span><span class="sxs-lookup"><span data-stu-id="300e2-239">If you must perform this work within ASP.NET, you can add the Nuget package called [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) to run the code.</span></span>

<a id="requestentity"></a>

### <a name="request-entity-body"></a><span data-ttu-id="300e2-240">Corps d’entité</span><span class="sxs-lookup"><span data-stu-id="300e2-240">Request Entity Body</span></span>

<span data-ttu-id="300e2-241">Recommandation : Éviter la lecture de Request.Form ou Request.InputStream avant d’exécuter l’événement du gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="300e2-241">Recommendation: Avoid reading Request.Form or Request.InputStream before the handler's execute event.</span></span>

<span data-ttu-id="300e2-242">Dès que possible vous devez lire à partir de Request.Form ou Request.InputStream est au cours du gestionnaire exécuter l’événement.</span><span class="sxs-lookup"><span data-stu-id="300e2-242">The earliest you should read from Request.Form or Request.InputStream is during the handler's execute event.</span></span> <span data-ttu-id="300e2-243">Dans MVC, le contrôleur est le gestionnaire et l’événement d’exécution est lorsque la méthode d’action s’exécute.</span><span class="sxs-lookup"><span data-stu-id="300e2-243">In MVC, the Controller is the handler and the execute event is when the action method runs.</span></span> <span data-ttu-id="300e2-244">Dans Web Forms, la Page est le gestionnaire et l’événement d’exécution est lorsque l’événement Page.Init se déclenche.</span><span class="sxs-lookup"><span data-stu-id="300e2-244">In Web Forms, the Page is the handler and the execute event is when the Page.Init event fires.</span></span> <span data-ttu-id="300e2-245">Si vous lisez le corps d’entité de demande antérieure à l’événement d’exécution, vous interférer avec le traitement de la demande.</span><span class="sxs-lookup"><span data-stu-id="300e2-245">If you read the request entity body earlier than the execute event, you interfere with the processing of the request.</span></span>

<span data-ttu-id="300e2-246">Si vous avez besoin lire le corps d’entité de demande avant l’événement d’exécution, utilisez [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) ou [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span><span class="sxs-lookup"><span data-stu-id="300e2-246">If you need to read the request entity body before the execute event, use either [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) or [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span></span> <span data-ttu-id="300e2-247">Lorsque vous utilisez GetBufferlessInputStream, vous obtenez le flux de données brutes à partir de la demande et assumer la responsabilité de traiter la requête entière.</span><span class="sxs-lookup"><span data-stu-id="300e2-247">When you use GetBufferlessInputStream, you get the raw stream from the request, and assume responsibility for processing the entire request.</span></span> <span data-ttu-id="300e2-248">Après avoir appelé GetBufferlessInputStream, Request.Form et Request.InputStream ne sont pas disponibles, car ils n’ont pas été remplies par ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="300e2-248">After calling GetBufferlessInputStream, Request.Form and Request.InputStream are not available because they have not been populated by ASP.NET.</span></span> <span data-ttu-id="300e2-249">Lorsque vous utilisez GetBufferedInputStream, vous obtenez une copie du flux de données à partir de la demande.</span><span class="sxs-lookup"><span data-stu-id="300e2-249">When you use GetBufferedInputStream, you get a copy of the stream from the request.</span></span> <span data-ttu-id="300e2-250">Request.Form et Request.InputStream sont toujours disponibles plus loin dans la demande, car ASP.NET remplit l’autre exemplaire.</span><span class="sxs-lookup"><span data-stu-id="300e2-250">Request.Form and Request.InputStream are still available later in the request because ASP.NET populates the other copy.</span></span>

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a><span data-ttu-id="300e2-251">Compatibilité entre Response.Redirect et Response.End</span><span class="sxs-lookup"><span data-stu-id="300e2-251">Response.Redirect and Response.End</span></span>

<span data-ttu-id="300e2-252">Recommandation : Tenez compte des différences dans les modalités de thread après avoir appelé [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span><span class="sxs-lookup"><span data-stu-id="300e2-252">Recommendation: Be aware of differences in how thread is handled after calling [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span></span>

<span data-ttu-id="300e2-253">Le [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) méthode appelle la méthode Response.End.</span><span class="sxs-lookup"><span data-stu-id="300e2-253">The [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) method calls the Response.End method.</span></span> <span data-ttu-id="300e2-254">Dans un processus synchrone, appelant Request.Redirect provoque le thread actuel abandonner immédiatement.</span><span class="sxs-lookup"><span data-stu-id="300e2-254">In a synchronous process, calling Request.Redirect causes the current thread to immediately abort.</span></span> <span data-ttu-id="300e2-255">Toutefois, dans un processus asynchrone, l’appel de Response.Redirect n’abandonne pas le thread actuel, afin de l’exécution de code se poursuit pour la demande.</span><span class="sxs-lookup"><span data-stu-id="300e2-255">However, in an asynchronous process, calling Response.Redirect does not abort the current thread, so code execution continues for the request.</span></span> <span data-ttu-id="300e2-256">Dans un processus asynchrone, vous devez retourner la tâche à partir de la méthode pour arrêter l’exécution de code.</span><span class="sxs-lookup"><span data-stu-id="300e2-256">In an asynchronous process, you must return the Task from the method to stop the code execution.</span></span>

<span data-ttu-id="300e2-257">Dans un projet MVC, n’appelez pas Response.Redirect.</span><span class="sxs-lookup"><span data-stu-id="300e2-257">In an MVC project, you should not call Response.Redirect.</span></span> <span data-ttu-id="300e2-258">Au lieu de cela, retourner un RedirectResult.</span><span class="sxs-lookup"><span data-stu-id="300e2-258">Instead, return a RedirectResult.</span></span>

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a><span data-ttu-id="300e2-259">EnableViewState et ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="300e2-259">EnableViewState and ViewStateMode</span></span>

<span data-ttu-id="300e2-260">Recommandation : Utilisez ViewStateMode, au lieu de EnableViewState, pour fournir un contrôle granulaire sur laquelle les contrôles utilisent l’état d’affichage.</span><span class="sxs-lookup"><span data-stu-id="300e2-260">Recommendation: Use ViewStateMode, instead of EnableViewState, to provide granular control over which controls use view state.</span></span>

<span data-ttu-id="300e2-261">Lorsque vous définissez EnableViewState sur false dans la directive de Page, l’état d’affichage est désactivé pour tous les contrôles dans la page et ne peut pas être activée.</span><span class="sxs-lookup"><span data-stu-id="300e2-261">When you set EnableViewState to false in the Page directive, view state is disabled for all controls within the page and cannot be enabled.</span></span> <span data-ttu-id="300e2-262">Si vous souhaitez activer l’état d’affichage que pour certains contrôles dans votre page, la valeur ViewStateMode désactivé pour la Page.</span><span class="sxs-lookup"><span data-stu-id="300e2-262">If you want to enable view state for only certain controls in your page, set ViewStateMode to Disabled for the Page.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

<span data-ttu-id="300e2-263">Ensuite, définissez ViewStateMode Enabled sur uniquement les contrôles qui doivent en fait état d’affichage.</span><span class="sxs-lookup"><span data-stu-id="300e2-263">Then, set ViewStateMode to Enabled on only the controls that actually need view state.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

<span data-ttu-id="300e2-264">En activant l’état d’affichage pour seulement les contrôles qui en ont besoin, vous pouvez réduire la taille de l’état d’affichage pour vos pages web.</span><span class="sxs-lookup"><span data-stu-id="300e2-264">By enabling view state for only the controls that need it, you can shrink the size of the view state for your web pages.</span></span>

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a><span data-ttu-id="300e2-265">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="300e2-265">SqlMembershipProvider</span></span>

<span data-ttu-id="300e2-266">Recommandation : Utilisez les fournisseurs universels.</span><span class="sxs-lookup"><span data-stu-id="300e2-266">Recommendation: Use Universal Providers.</span></span>

<span data-ttu-id="300e2-267">Dans les modèles de projet actuel, SqlMembershipProvider a été remplacé par [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), qui est disponible comme package NuGet.</span><span class="sxs-lookup"><span data-stu-id="300e2-267">In the current project templates, SqlMembershipProvider has been replaced by [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), which is available as a NuGet package.</span></span> <span data-ttu-id="300e2-268">Si vous utilisez SqlMembershipProvider dans un projet qui a été généré avec une version antérieure des modèles, vous devez basculer vers les fournisseurs universels.</span><span class="sxs-lookup"><span data-stu-id="300e2-268">If you are using SqlMembershipProvider in a project that was built with an earlier version of the templates, you should switch to Universal Providers.</span></span> <span data-ttu-id="300e2-269">Les fournisseurs universels fonctionnent avec toutes les bases de données qui sont prises en charge par Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="300e2-269">The Universal Providers work with all databases that are supported by Entity Framework.</span></span>

<span data-ttu-id="300e2-270">Pour plus d’informations, consultez [Introducing ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span><span class="sxs-lookup"><span data-stu-id="300e2-270">For more information, see [Introducing ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span></span>

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a><span data-ttu-id="300e2-271">Requêtes longues (> 110 secondes)</span><span class="sxs-lookup"><span data-stu-id="300e2-271">Long-running Requests (>110 seconds)</span></span>

<span data-ttu-id="300e2-272">Recommandation : Utilisez [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) ou [SignalR](../../../signalr/index.md) pour les clients connectés et utilisez les opérations d’e/s asynchrones.</span><span class="sxs-lookup"><span data-stu-id="300e2-272">Recommendation: Use [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) or [SignalR](../../../signalr/index.md) for connected clients, and use asynchronous I/O operations.</span></span>

<span data-ttu-id="300e2-273">Requêtes longues peuvent provoquer des résultats imprévisibles et des performances médiocres dans votre application web.</span><span class="sxs-lookup"><span data-stu-id="300e2-273">Long-running requests can cause unpredictable results and poor performance in your web application.</span></span> <span data-ttu-id="300e2-274">Le paramètre de délai d’expiration par défaut pour une demande est de 110 secondes.</span><span class="sxs-lookup"><span data-stu-id="300e2-274">The default timeout setting for a request is 110 seconds.</span></span> <span data-ttu-id="300e2-275">Si vous utilisez l’état de session avec une requête longue, ASP.NET libère le verrou sur l’objet de Session après 110 secondes.</span><span class="sxs-lookup"><span data-stu-id="300e2-275">If you are using session state with a long-running request, ASP.NET will release the lock on the Session object after 110 seconds.</span></span> <span data-ttu-id="300e2-276">Toutefois, votre application peut être au milieu d’une opération sur l’objet de Session lorsque le verrou est libéré, et l’opération ne peut pas terminer correctement.</span><span class="sxs-lookup"><span data-stu-id="300e2-276">However, your application might be in the middle of an operation on the Session object when the lock is released, and the operation might not complete successfully.</span></span> <span data-ttu-id="300e2-277">Si une deuxième requête à partir de l’utilisateur est bloquée pendant l’exécution de la première demande, la deuxième demande peut accéder à l’objet de Session dans un état incohérent.</span><span class="sxs-lookup"><span data-stu-id="300e2-277">If a second request from the user is blocked while the first request is running, the second request might access the Session object in an inconsistent state.</span></span>

<span data-ttu-id="300e2-278">Si votre application inclut des opérations d’e/s bloquantes (ou synchrones), l’application sera ne répond pas.</span><span class="sxs-lookup"><span data-stu-id="300e2-278">If your application includes blocking (or synchronous) I/O operations, the application will be unresponsive.</span></span>

<span data-ttu-id="300e2-279">Pour améliorer les performances, utilisez les opérations d’e/s asynchrones dans le .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="300e2-279">To improve performance, use the asynchronous I/O operations in the .NET Framework.</span></span> <span data-ttu-id="300e2-280">En outre, utilisez WebSockets ou SignalR pour les clients qui se connectent au serveur.</span><span class="sxs-lookup"><span data-stu-id="300e2-280">Also, use WebSockets or SignalR for connecting clients to the server.</span></span> <span data-ttu-id="300e2-281">Ces fonctionnalités sont conçues pour gérer efficacement les requêtes longues.</span><span class="sxs-lookup"><span data-stu-id="300e2-281">These features are designed to efficiently handle long-running requests.</span></span>
