---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: L’activation des requêtes Cross-Origin dans ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Montre comment prendre en charge le partage des ressources Cross-Origin (CORS) dans l’API Web ASP.NET.
ms.author: riande
ms.date: 10/10/2018
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 118b779c89edb874f7f928315d1094738be5f097
ms.sourcegitcommit: 6e6002de467cd135a69e5518d4ba9422d693132a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49348518"
---
<a name="enable-cross-origin-requests-in-aspnet-web-api-2"></a><span data-ttu-id="e3740-103">Activer les demandes cross-origin dans ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="e3740-103">Enable cross-origin requests in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="e3740-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e3740-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="e3740-105">La sécurité du navigateur empêche une page web d’effectuer des demandes AJAX vers un autre domaine.</span><span class="sxs-lookup"><span data-stu-id="e3740-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="e3740-106">Cette restriction est appelée la *stratégie de même origine* et empêche un site malveillant de lire des données sensibles à partir d’un autre site.</span><span class="sxs-lookup"><span data-stu-id="e3740-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="e3740-107">Toutefois, vous pouvez parfois laisser les autres sites à appeler votre API web.</span><span class="sxs-lookup"><span data-stu-id="e3740-107">However, sometimes you might want to let other sites call your web API.</span></span>
>
> <span data-ttu-id="e3740-108">[Cross-origine partage de ressources](http://www.w3.org/TR/cors/) (CORS) est une norme W3C qui permet à un serveur d’abaisser la stratégie de même origine.</span><span class="sxs-lookup"><span data-stu-id="e3740-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="e3740-109">À l’aide de CORS, un serveur peut autoriser explicitement certaines demandes cross-origin lors du refus d’autres.</span><span class="sxs-lookup"><span data-stu-id="e3740-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="e3740-110">CORS est plus sûre et plus flexible que des techniques antérieures telles que [JSONP](http://en.wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="e3740-110">CORS is safer and more flexible than earlier techniques such as [JSONP](http://en.wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="e3740-111">Ce didacticiel montre comment activer CORS dans votre application API Web.</span><span class="sxs-lookup"><span data-stu-id="e3740-111">This tutorial shows how to enable CORS in your Web API application.</span></span>
>
> ## <a name="software-used-in-the-tutorial"></a><span data-ttu-id="e3740-112">Logiciels utilisés dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="e3740-112">Software used in the tutorial</span></span>
>
> - [<span data-ttu-id="e3740-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e3740-113">Visual Studio</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="e3740-114">Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="e3740-114">Web API 2.2</span></span>

## <a name="introduction"></a><span data-ttu-id="e3740-115">Introduction</span><span class="sxs-lookup"><span data-stu-id="e3740-115">Introduction</span></span>

<span data-ttu-id="e3740-116">Ce didacticiel illustre la que prise en charge de CORS dans l’API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e3740-116">This tutorial demonstrates CORS support in ASP.NET Web API.</span></span> <span data-ttu-id="e3740-117">Nous allons commencer en créant deux projets ASP.NET : une appelée « WebService », qui héberge un contrôleur d’API Web, et l’autre appelée « WebClient », qui appelle le service Web.</span><span class="sxs-lookup"><span data-stu-id="e3740-117">We'll start by creating two ASP.NET projects – one called "WebService", which hosts a Web API controller, and the other called "WebClient", which calls WebService.</span></span> <span data-ttu-id="e3740-118">Étant donné que les deux applications sont hébergées sur des domaines différents, une requête AJAX à partir de WebClient pour le service Web est une demande de cross-origin.</span><span class="sxs-lookup"><span data-stu-id="e3740-118">Because the two applications are hosted at different domains, an AJAX request from WebClient to WebService is a cross-origin request.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a><span data-ttu-id="e3740-119">Que veut dire « même origine » ?</span><span class="sxs-lookup"><span data-stu-id="e3740-119">What is "same origin"?</span></span>

<span data-ttu-id="e3740-120">Deux URL ont la même origine que s’ils ont des ports, des hôtes et des schémas identiques.</span><span class="sxs-lookup"><span data-stu-id="e3740-120">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="e3740-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="e3740-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="e3740-122">Ces deux URL ayant la même origine :</span><span class="sxs-lookup"><span data-stu-id="e3740-122">These two URLs have the same origin:</span></span>

- `http://example.com/foo.html`
- `http://example.com/bar.html`

<span data-ttu-id="e3740-123">Ces URL ont des origines différentes aux deux précédentes :</span><span class="sxs-lookup"><span data-stu-id="e3740-123">These URLs have different origins than the previous two:</span></span>

- <span data-ttu-id="e3740-124">`http://example.net` -Autre domaine</span><span class="sxs-lookup"><span data-stu-id="e3740-124">`http://example.net` - Different domain</span></span>
- <span data-ttu-id="e3740-125">`http://example.com:9000/foo.html` -Autre port</span><span class="sxs-lookup"><span data-stu-id="e3740-125">`http://example.com:9000/foo.html` - Different port</span></span>
- <span data-ttu-id="e3740-126">`https://example.com/foo.html` -Schéma différent</span><span class="sxs-lookup"><span data-stu-id="e3740-126">`https://example.com/foo.html` - Different scheme</span></span>
- <span data-ttu-id="e3740-127">`http://www.example.com/foo.html` -Autre sous-domaine</span><span class="sxs-lookup"><span data-stu-id="e3740-127">`http://www.example.com/foo.html` - Different subdomain</span></span>

> [!NOTE]
> <span data-ttu-id="e3740-128">Internet Explorer ne considère pas le port lors de la comparaison des origines.</span><span class="sxs-lookup"><span data-stu-id="e3740-128">Internet Explorer does not consider the port when comparing origins.</span></span>

## <a name="create-the-webservice-project"></a><span data-ttu-id="e3740-129">Créer le projet WebService</span><span class="sxs-lookup"><span data-stu-id="e3740-129">Create the WebService project</span></span>

> [!NOTE]
> <span data-ttu-id="e3740-130">Cette section suppose que vous savez déjà comment créer des projets d’API Web.</span><span class="sxs-lookup"><span data-stu-id="e3740-130">This section assumes you already know how to create Web API projects.</span></span> <span data-ttu-id="e3740-131">Dans le cas contraire, consultez [mise en route avec ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="e3740-131">If not, see [Getting Started with ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>

1. <span data-ttu-id="e3740-132">Démarrez Visual Studio et créez un nouveau **Application Web ASP.NET (.NET Framework)** projet.</span><span class="sxs-lookup"><span data-stu-id="e3740-132">Start Visual Studio and create a new **ASP.NET Web Application (.NET Framework)** project.</span></span>
2. <span data-ttu-id="e3740-133">Dans le **nouvelle Application Web ASP.NET** boîte de dialogue, sélectionnez le **vide** modèle de projet.</span><span class="sxs-lookup"><span data-stu-id="e3740-133">In the **New ASP.NET Web Application** dialog box, select the **Empty** project template.</span></span> <span data-ttu-id="e3740-134">Sous **ajouter des dossiers et les références principales pour**, sélectionnez le **API Web** case à cocher.</span><span class="sxs-lookup"><span data-stu-id="e3740-134">Under **Add folders and core references for**, select the **Web API** checkbox.</span></span>

   ![Nouvelle boîte de dialogue de projet ASP.NET dans Visual Studio](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog.png)

3. <span data-ttu-id="e3740-136">Ajouter un contrôleur d’API Web nommé `TestController` avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e3740-136">Add a Web API controller named `TestController` with the following code:</span></span>

   [!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

4. <span data-ttu-id="e3740-137">Vous pouvez exécuter l’application localement ou déployez-la dans Azure.</span><span class="sxs-lookup"><span data-stu-id="e3740-137">You can run the application locally or deploy to Azure.</span></span> <span data-ttu-id="e3740-138">(Pour les captures d’écran dans ce didacticiel, l’application se déploie sur Azure App Service Web Apps.) Pour vérifier l’utilisation de l’API web, accédez à `http://hostname/api/test/`, où *nom d’hôte* est le domaine où vous avez déployé l’application.</span><span class="sxs-lookup"><span data-stu-id="e3740-138">(For the screenshots in this tutorial, the app deploys to Azure App Service Web Apps.) To verify that the web API is working, navigate to `http://hostname/api/test/`, where *hostname* is the domain where you deployed the application.</span></span> <span data-ttu-id="e3740-139">Vous devez voir le texte de réponse, &quot;obtenir : Message de Test&quot;.</span><span class="sxs-lookup"><span data-stu-id="e3740-139">You should see the response text, &quot;GET: Test Message&quot;.</span></span>

   ![Message de test Web navigateur affichant](enabling-cross-origin-requests-in-web-api/_static/image4.png)

## <a name="create-the-webclient-project"></a><span data-ttu-id="e3740-141">Créer le projet de WebClient</span><span class="sxs-lookup"><span data-stu-id="e3740-141">Create the WebClient project</span></span>

1. <span data-ttu-id="e3740-142">Créer un autre **Application Web ASP.NET (.NET Framework)** de projet et sélectionnez le **MVC** modèle de projet.</span><span class="sxs-lookup"><span data-stu-id="e3740-142">Create another **ASP.NET Web Application (.NET Framework)** project and select the **MVC** project template.</span></span> <span data-ttu-id="e3740-143">Si vous le souhaitez, sélectionnez **modifier l’authentification** > **aucune authentification**.</span><span class="sxs-lookup"><span data-stu-id="e3740-143">Optionally, select **Change Authentication** > **No Authentication**.</span></span> <span data-ttu-id="e3740-144">Vous n’avez pas besoin d’authentification pour ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="e3740-144">You don't need authentication for this tutorial.</span></span>

   ![Modèle MVC dans la boîte de dialogue Nouveau projet ASP.NET dans Visual Studio](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog-mvc.png)

2. <span data-ttu-id="e3740-146">Dans **l’Explorateur de solutions**, ouvrez le fichier *Views/Home/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="e3740-146">In **Solution Explorer**, open the file *Views/Home/Index.cshtml*.</span></span> <span data-ttu-id="e3740-147">Remplacez le code dans ce fichier avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="e3740-147">Replace the code in this file with the following:</span></span>

   [!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

   <span data-ttu-id="e3740-148">Pour le *serviceUrl* variable, utilisez l’URI de l’application de service Web.</span><span class="sxs-lookup"><span data-stu-id="e3740-148">For the *serviceUrl* variable, use the URI of the WebService app.</span></span>

3. <span data-ttu-id="e3740-149">Exécuter l’application de WebClient localement ou la publier sur un autre site Web.</span><span class="sxs-lookup"><span data-stu-id="e3740-149">Run the WebClient app locally or publish it to another website.</span></span>

<span data-ttu-id="e3740-150">Lorsque vous cliquez sur le bouton « essayer », une requête AJAX est soumise à l’application de service Web à l’aide de la méthode HTTP répertoriée dans la liste déroulante (GET, POST ou PUT).</span><span class="sxs-lookup"><span data-stu-id="e3740-150">When you click the "Try It" button, an AJAX request is submitted to the WebService app using the HTTP method listed in the dropdown box (GET, POST, or PUT).</span></span> <span data-ttu-id="e3740-151">Cela vous permet d’examiner les différentes demandes cross-origin.</span><span class="sxs-lookup"><span data-stu-id="e3740-151">This lets you examine different cross-origin requests.</span></span> <span data-ttu-id="e3740-152">Actuellement, l’application de service Web ne prend pas en charge CORS, si vous cliquez sur le bouton, vous obtiendrez une erreur.</span><span class="sxs-lookup"><span data-stu-id="e3740-152">Currently, the WebService app does not support CORS, so if you click the button you'll get an error.</span></span>

![Erreur de « Essayer » dans le navigateur](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="e3740-154">Si vous regardez le trafic HTTP dans un outil tel que [Fiddler](http://www.telerik.com/fiddler), vous verrez que le navigateur envoie la demande GET et la demande réussit, mais l’appel AJAX retourne une erreur.</span><span class="sxs-lookup"><span data-stu-id="e3740-154">If you watch the HTTP traffic in a tool like [Fiddler](http://www.telerik.com/fiddler), you'll see that the browser does send the GET request, and the request succeeds, but the AJAX call returns an error.</span></span> <span data-ttu-id="e3740-155">Il est important de comprendre que stratégie de même origine n’empêche pas le navigateur à partir de *envoi* la demande.</span><span class="sxs-lookup"><span data-stu-id="e3740-155">It's important to understand that same-origin policy does not prevent the browser from *sending* the request.</span></span> <span data-ttu-id="e3740-156">Au lieu de cela, elle empêche l’application de voir les *réponse*.</span><span class="sxs-lookup"><span data-stu-id="e3740-156">Instead, it prevents the application from seeing the *response*.</span></span>

![Débogueur web Fiddler de requêtes web](enabling-cross-origin-requests-in-web-api/_static/image8.png)

## <a name="enable-cors"></a><span data-ttu-id="e3740-158">Activer CORS</span><span class="sxs-lookup"><span data-stu-id="e3740-158">Enable CORS</span></span>

<span data-ttu-id="e3740-159">Maintenant nous allons activer CORS dans l’application de service Web.</span><span class="sxs-lookup"><span data-stu-id="e3740-159">Now let's enable CORS in the WebService app.</span></span> <span data-ttu-id="e3740-160">Tout d’abord, ajoutez le package NuGet de CORS.</span><span class="sxs-lookup"><span data-stu-id="e3740-160">First, add the CORS NuGet package.</span></span> <span data-ttu-id="e3740-161">Dans Visual Studio, à partir de la **outils** menu, sélectionnez **Gestionnaire de Package NuGet**, puis sélectionnez **Console du Gestionnaire de Package**.</span><span class="sxs-lookup"><span data-stu-id="e3740-161">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="e3740-162">Dans la fenêtre de Console du Gestionnaire de Package, tapez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="e3740-162">In the Package Manager Console window, type the following command:</span></span>

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

<span data-ttu-id="e3740-163">Cette commande installe le dernier package et met à jour toutes les dépendances, y compris les bibliothèques d’API Web core.</span><span class="sxs-lookup"><span data-stu-id="e3740-163">This command installs the latest package and updates all dependencies, including the core Web API libraries.</span></span> <span data-ttu-id="e3740-164">Utilisez le `-Version` indicateur pour cibler une version spécifique.</span><span class="sxs-lookup"><span data-stu-id="e3740-164">Use the `-Version` flag to target a specific version.</span></span> <span data-ttu-id="e3740-165">Le package CORS requiert Web API 2.0 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="e3740-165">The CORS package requires Web API 2.0 or later.</span></span>

<span data-ttu-id="e3740-166">Ouvrez le fichier *application\_Start/WebApiConfig.cs*.</span><span class="sxs-lookup"><span data-stu-id="e3740-166">Open the file *App\_Start/WebApiConfig.cs*.</span></span> <span data-ttu-id="e3740-167">Ajoutez le code suivant à la **WebApiConfig.Register** méthode :</span><span class="sxs-lookup"><span data-stu-id="e3740-167">Add the following code to the **WebApiConfig.Register** method:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

<span data-ttu-id="e3740-168">Ensuite, ajoutez le **[EnableCors]** attribut le `TestController` classe :</span><span class="sxs-lookup"><span data-stu-id="e3740-168">Next, add the **[EnableCors]** attribute to the `TestController` class:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

<span data-ttu-id="e3740-169">Pour le *origines* paramètre, utilisez l’URI où vous avez déployé l’application de WebClient.</span><span class="sxs-lookup"><span data-stu-id="e3740-169">For the *origins* parameter, use the URI where you deployed the WebClient application.</span></span> <span data-ttu-id="e3740-170">Cela permet des requêtes cross-origin à partir de WebClient, tout toujours interdit toutes les autres demandes inter-domaines.</span><span class="sxs-lookup"><span data-stu-id="e3740-170">This allows cross-origin requests from WebClient, while still disallowing all other cross-domain requests.</span></span> <span data-ttu-id="e3740-171">Plus tard, je vais décrire les paramètres pour **[EnableCors]** plus en détail.</span><span class="sxs-lookup"><span data-stu-id="e3740-171">Later, I'll describe the parameters for **[EnableCors]** in more detail.</span></span>

<span data-ttu-id="e3740-172">N’incluez pas une barre oblique à la fin de la *origines* URL.</span><span class="sxs-lookup"><span data-stu-id="e3740-172">Do not include a forward slash at the end of the *origins* URL.</span></span>

<span data-ttu-id="e3740-173">Redéployer l’application de service Web mis à jour.</span><span class="sxs-lookup"><span data-stu-id="e3740-173">Redeploy the updated WebService application.</span></span> <span data-ttu-id="e3740-174">Vous n’avez pas besoin de mettre à jour de WebClient.</span><span class="sxs-lookup"><span data-stu-id="e3740-174">You don't need to update WebClient.</span></span> <span data-ttu-id="e3740-175">La requête AJAX à partir de WebClient doit réussir.</span><span class="sxs-lookup"><span data-stu-id="e3740-175">Now the AJAX request from WebClient should succeed.</span></span> <span data-ttu-id="e3740-176">Toutes les méthodes GET, PUT et POST sont autorisées.</span><span class="sxs-lookup"><span data-stu-id="e3740-176">The GET, PUT, and POST methods are all allowed.</span></span>

![Message de test réussi d’affichant de navigateur Web](enabling-cross-origin-requests-in-web-api/_static/image9.png)

## <a name="how-cors-works"></a><span data-ttu-id="e3740-178">Fonctionnement de CORS</span><span class="sxs-lookup"><span data-stu-id="e3740-178">How CORS Works</span></span>

<span data-ttu-id="e3740-179">Cette section décrit ce qui se passe dans une demande CORS, au niveau des messages HTTP.</span><span class="sxs-lookup"><span data-stu-id="e3740-179">This section describes what happens in a CORS request, at the level of the HTTP messages.</span></span> <span data-ttu-id="e3740-180">Il est important de comprendre le fonctionnement de CORS, afin que vous puissiez configurer le **[EnableCors]** attribut correctement et résoudre les problèmes si les choses ne fonctionnent pas comme prévu.</span><span class="sxs-lookup"><span data-stu-id="e3740-180">It's important to understand how CORS works, so that you can configure the **[EnableCors]** attribute correctly and troubleshoot if things don't work as you expect.</span></span>

<span data-ttu-id="e3740-181">La spécification CORS introduit plusieurs nouveaux en-têtes HTTP qui permettent les demandes cross-origin.</span><span class="sxs-lookup"><span data-stu-id="e3740-181">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="e3740-182">Si un navigateur prend en charge CORS, il définit ces en-têtes automatiquement pour les demandes cross-origin ; vous n’avez pas besoin de rien de spécial dans votre code JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e3740-182">If a browser supports CORS, it sets these headers automatically for cross-origin requests; you don't need to do anything special in your JavaScript code.</span></span>

<span data-ttu-id="e3740-183">Voici un exemple d’une demande de cross-origin.</span><span class="sxs-lookup"><span data-stu-id="e3740-183">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="e3740-184">L’en-tête « Origin » donne le domaine du site qui effectue la demande.</span><span class="sxs-lookup"><span data-stu-id="e3740-184">The "Origin" header gives the domain of the site that is making the request.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

<span data-ttu-id="e3740-185">Si le serveur autorise la demande, il définit l’en-tête Access-Control-Allow-Origin.</span><span class="sxs-lookup"><span data-stu-id="e3740-185">If the server allows the request, it sets the Access-Control-Allow-Origin header.</span></span> <span data-ttu-id="e3740-186">La valeur de cet en-tête correspond à l’en-tête d’origine, soit est la valeur de caractère générique «\*», ce qui signifie que toute origine est autorisée.</span><span class="sxs-lookup"><span data-stu-id="e3740-186">The value of this header either matches the Origin header, or is the wildcard value "\*", meaning that any origin is allowed.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

<span data-ttu-id="e3740-187">Si la réponse n’inclut pas l’en-tête Access-Control-Allow-Origin, la requête AJAX échoue.</span><span class="sxs-lookup"><span data-stu-id="e3740-187">If the response does not include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="e3740-188">Plus précisément, le navigateur n’autorise pas la demande.</span><span class="sxs-lookup"><span data-stu-id="e3740-188">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="e3740-189">Même si le serveur retourne une réponse correcte, le navigateur ne rend pas la réponse disponibles à l’application cliente.</span><span class="sxs-lookup"><span data-stu-id="e3740-189">Even if the server returns a successful response, the browser does not make the response available to the client application.</span></span>

<span data-ttu-id="e3740-190">**Demandes préliminaires**</span><span class="sxs-lookup"><span data-stu-id="e3740-190">**Preflight Requests**</span></span>

<span data-ttu-id="e3740-191">Pour certaines requêtes CORS, le navigateur envoie une demande supplémentaire, appelée « demande préliminaire, » avant d’envoyer la demande réelle de la ressource.</span><span class="sxs-lookup"><span data-stu-id="e3740-191">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span>

<span data-ttu-id="e3740-192">Le navigateur peut ignorer la demande préliminaire si les conditions suivantes sont remplies :</span><span class="sxs-lookup"><span data-stu-id="e3740-192">The browser can skip the preflight request if the following conditions are true:</span></span>

- <span data-ttu-id="e3740-193">La méthode de demande est GET, HEAD ou POST, *et*</span><span class="sxs-lookup"><span data-stu-id="e3740-193">The request method is GET, HEAD, or POST, *and*</span></span>
- <span data-ttu-id="e3740-194">L’application ne définit pas les en-têtes de demande autre que Accept, Accept-Language, Content-Language, Content-Type ou dernière--ID d’événement, *et*</span><span class="sxs-lookup"><span data-stu-id="e3740-194">The application does not set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, *and*</span></span>
- <span data-ttu-id="e3740-195">L’en-tête `Content-Type` (si défini) est une des opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="e3740-195">The Content-Type header (if set) is one of the following:</span></span>

    - <span data-ttu-id="e3740-196">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="e3740-196">application/x-www-form-urlencoded</span></span>
    - <span data-ttu-id="e3740-197">multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="e3740-197">multipart/form-data</span></span>
    - <span data-ttu-id="e3740-198">texte/brut</span><span class="sxs-lookup"><span data-stu-id="e3740-198">text/plain</span></span>

<span data-ttu-id="e3740-199">La règle sur les en-têtes de demande s’applique aux en-têtes de l’application définit en appelant **setRequestHeader** sur le **XMLHttpRequest** objet.</span><span class="sxs-lookup"><span data-stu-id="e3740-199">The rule about request headers applies to headers that the application sets by calling **setRequestHeader** on the **XMLHttpRequest** object.</span></span> <span data-ttu-id="e3740-200">(La spécification CORS les appelle « en-têtes de demande auteur ».) La règle ne s’applique pas aux en-têtes de la *navigateur* pouvez définir, telles que l’Agent utilisateur, hôte ou Content-Length.</span><span class="sxs-lookup"><span data-stu-id="e3740-200">(The CORS specification calls these "author request headers".) The rule does not apply to headers the *browser* can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="e3740-201">Voici un exemple d’une requête préliminaire :</span><span class="sxs-lookup"><span data-stu-id="e3740-201">Here is an example of a preflight request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

<span data-ttu-id="e3740-202">La demande de contrôle préliminaire utilise la méthode HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="e3740-202">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="e3740-203">Il comprend deux en-têtes spéciaux :</span><span class="sxs-lookup"><span data-stu-id="e3740-203">It includes two special headers:</span></span>

- <span data-ttu-id="e3740-204">Access-Control-Request-Method : Méthode HTTP qui sera utilisée pour la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="e3740-204">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>
- <span data-ttu-id="e3740-205">Access-Control-Request-Headers : Une liste d’en-têtes de demande qui les *application* définie sur la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="e3740-205">Access-Control-Request-Headers: A list of request headers that the *application* set on the actual request.</span></span> <span data-ttu-id="e3740-206">(Là encore, cela n’inclut pas les en-têtes qui définit le navigateur.)</span><span class="sxs-lookup"><span data-stu-id="e3740-206">(Again, this does not include headers that the browser sets.)</span></span>

<span data-ttu-id="e3740-207">Voici un exemple de réponse, en supposant que le serveur autorise la demande :</span><span class="sxs-lookup"><span data-stu-id="e3740-207">Here is an example response, assuming that the server allows the request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

<span data-ttu-id="e3740-208">La réponse inclut un en-tête `Access-Control-Allow-Methods` qui répertorie les méthodes autorisées et éventuellement un en-tête `Access-Control-Allow-Headers`, qui répertorie les en-têtes autorisés.</span><span class="sxs-lookup"><span data-stu-id="e3740-208">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="e3740-209">Si la demande préliminaire réussit, le navigateur envoie la demande réelle, comme décrit précédemment.</span><span class="sxs-lookup"><span data-stu-id="e3740-209">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>

## <a name="scope-rules-for-enablecors"></a><span data-ttu-id="e3740-210">Règles de portée pour [EnableCors]</span><span class="sxs-lookup"><span data-stu-id="e3740-210">Scope Rules for [EnableCors]</span></span>

<span data-ttu-id="e3740-211">Vous pouvez activer CORS par action, par contrôleur, ou globalement pour tous les contrôleurs d’API Web dans votre application.</span><span class="sxs-lookup"><span data-stu-id="e3740-211">You can enable CORS per action, per controller, or globally for all Web API controllers in your application.</span></span>

<span data-ttu-id="e3740-212">**Par Action**</span><span class="sxs-lookup"><span data-stu-id="e3740-212">**Per Action**</span></span>

<span data-ttu-id="e3740-213">Pour activer CORS pour une seule action, définissez la **[EnableCors]** attribut sur la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="e3740-213">To enable CORS for a single action, set the **[EnableCors]** attribute on the action method.</span></span> <span data-ttu-id="e3740-214">L’exemple suivant active CORS pour le `GetItem` méthode uniquement.</span><span class="sxs-lookup"><span data-stu-id="e3740-214">The following example enables CORS for the `GetItem` method only.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

<span data-ttu-id="e3740-215">**Par contrôleur**</span><span class="sxs-lookup"><span data-stu-id="e3740-215">**Per Controller**</span></span>

<span data-ttu-id="e3740-216">Si vous définissez **[EnableCors]** sur la classe de contrôleur, il s’applique à toutes les actions sur le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e3740-216">If you set **[EnableCors]** on the controller class, it applies to all the actions on the controller.</span></span> <span data-ttu-id="e3740-217">Pour désactiver CORS pour une action, ajoutez le **[DisableCors]** d’attribut à l’action.</span><span class="sxs-lookup"><span data-stu-id="e3740-217">To disable CORS for an action, add the **[DisableCors]** attribute to the action.</span></span> <span data-ttu-id="e3740-218">L’exemple suivant active CORS pour chaque méthode, à l’exception `PutItem`.</span><span class="sxs-lookup"><span data-stu-id="e3740-218">The following example enables CORS for every method except `PutItem`.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

<span data-ttu-id="e3740-219">**Dans le monde entier**</span><span class="sxs-lookup"><span data-stu-id="e3740-219">**Globally**</span></span>

<span data-ttu-id="e3740-220">Pour activer CORS pour tous les contrôleurs d’API Web dans votre application, passez un **EnableCorsAttribute** l’instance à la **EnableCors** méthode :</span><span class="sxs-lookup"><span data-stu-id="e3740-220">To enable CORS for all Web API controllers in your application, pass an **EnableCorsAttribute** instance to the **EnableCors** method:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

<span data-ttu-id="e3740-221">Si vous définissez l’attribut à plusieurs étendues, l’ordre de priorité est :</span><span class="sxs-lookup"><span data-stu-id="e3740-221">If you set the attribute at more than one scope, the order of precedence is:</span></span>

1. <span data-ttu-id="e3740-222">Action</span><span class="sxs-lookup"><span data-stu-id="e3740-222">Action</span></span>
2. <span data-ttu-id="e3740-223">Contrôleur</span><span class="sxs-lookup"><span data-stu-id="e3740-223">Controller</span></span>
3. <span data-ttu-id="e3740-224">Global</span><span class="sxs-lookup"><span data-stu-id="e3740-224">Global</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="e3740-225">Définir les origines autorisées</span><span class="sxs-lookup"><span data-stu-id="e3740-225">Set the allowed origins</span></span>

<span data-ttu-id="e3740-226">Le *origines* paramètre de la **[EnableCors]** attribut spécifie les origines autorisées à accéder à la ressource.</span><span class="sxs-lookup"><span data-stu-id="e3740-226">The *origins* parameter of the **[EnableCors]** attribute specifies which origins are allowed to access the resource.</span></span> <span data-ttu-id="e3740-227">La valeur est une liste séparée par des virgules des origines autorisées.</span><span class="sxs-lookup"><span data-stu-id="e3740-227">The value is a comma-separated list of the allowed origins.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

<span data-ttu-id="e3740-228">Vous pouvez également utiliser la valeur de caractère générique «\*» pour autoriser les demandes à partir de n’importe quel origines.</span><span class="sxs-lookup"><span data-stu-id="e3740-228">You can also use the wildcard value "\*" to allow requests from any origins.</span></span>

<span data-ttu-id="e3740-229">Réfléchissez bien avant d’autoriser des demandes à partir de n’importe quelle origine.</span><span class="sxs-lookup"><span data-stu-id="e3740-229">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="e3740-230">Cela signifie que littéralement n’importe quel site Web peut effectuer les appels AJAX à votre API web.</span><span class="sxs-lookup"><span data-stu-id="e3740-230">It means that literally any website can make AJAX calls to your web API.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

## <a name="set-the-allowed-http-methods"></a><span data-ttu-id="e3740-231">Définir les méthodes HTTP autorisées</span><span class="sxs-lookup"><span data-stu-id="e3740-231">Set the allowed HTTP methods</span></span>

<span data-ttu-id="e3740-232">Le *méthodes* paramètre de la **[EnableCors]** attribut spécifie les méthodes HTTP sont autorisés à accéder à la ressource.</span><span class="sxs-lookup"><span data-stu-id="e3740-232">The *methods* parameter of the **[EnableCors]** attribute specifies which HTTP methods are allowed to access the resource.</span></span> <span data-ttu-id="e3740-233">Pour autoriser toutes les méthodes, utilisez la valeur de caractère générique «\*».</span><span class="sxs-lookup"><span data-stu-id="e3740-233">To allow all methods, use the wildcard value "\*".</span></span> <span data-ttu-id="e3740-234">L’exemple suivant autorise uniquement les requêtes GET et POST.</span><span class="sxs-lookup"><span data-stu-id="e3740-234">The following example allows only GET and POST requests.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

## <a name="set-the-allowed-request-headers"></a><span data-ttu-id="e3740-235">Définir les en-têtes de requête</span><span class="sxs-lookup"><span data-stu-id="e3740-235">Set the allowed request headers</span></span>

<span data-ttu-id="e3740-236">Cet article décrit précédemment comment une requête préliminaire peut inclure un en-tête Access-Control-Request-Headers, répertoriant les en-têtes HTTP définis par l’application (la soi-disant « author des en-têtes de demande »).</span><span class="sxs-lookup"><span data-stu-id="e3740-236">This article described earlier how a preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span> <span data-ttu-id="e3740-237">Le *en-têtes* paramètre de la **[EnableCors]** attribut spécifie les en-têtes de requête d’auteur sont autorisés.</span><span class="sxs-lookup"><span data-stu-id="e3740-237">The *headers* parameter of the **[EnableCors]** attribute specifies which author request headers are allowed.</span></span> <span data-ttu-id="e3740-238">Pour autoriser tous les en-têtes, définissez *en-têtes* à «\*».</span><span class="sxs-lookup"><span data-stu-id="e3740-238">To allow any headers, set *headers* to "\*".</span></span> <span data-ttu-id="e3740-239">À la liste verte des en-têtes spécifiques, définissez *en-têtes* à une liste séparée par des virgules d’en-têtes autorisés :</span><span class="sxs-lookup"><span data-stu-id="e3740-239">To whitelist specific headers, set *headers* to a comma-separated list of the allowed headers:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

<span data-ttu-id="e3740-240">Toutefois, les navigateurs ne sont pas entièrement cohérents dans la façon dont elles définies Access-Control-Request-Headers.</span><span class="sxs-lookup"><span data-stu-id="e3740-240">However, browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="e3740-241">Par exemple, Chrome inclut actuellement « origin ».</span><span class="sxs-lookup"><span data-stu-id="e3740-241">For example, Chrome currently includes "origin".</span></span> <span data-ttu-id="e3740-242">FireFox n’inclut pas les en-têtes standard tels que « Accepter », même lorsque l’application les définit dans le script.</span><span class="sxs-lookup"><span data-stu-id="e3740-242">FireFox does not include standard headers such as "Accept", even when the application sets them in script.</span></span>

<span data-ttu-id="e3740-243">Si vous définissez *en-têtes* sur n’importe quelle autre que «\*», vous devez inclure au moins « accepter », « content-type » et « origin », ainsi que tous les en-têtes personnalisés que vous souhaitez prendre en charge.</span><span class="sxs-lookup"><span data-stu-id="e3740-243">If you set *headers* to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

## <a name="set-the-allowed-response-headers"></a><span data-ttu-id="e3740-244">Définir les en-têtes de réponse autorisés</span><span class="sxs-lookup"><span data-stu-id="e3740-244">Set the allowed response headers</span></span>

<span data-ttu-id="e3740-245">Par défaut, le navigateur n’expose pas tous les en-têtes de réponse à l’application.</span><span class="sxs-lookup"><span data-stu-id="e3740-245">By default, the browser does not expose all of the response headers to the application.</span></span> <span data-ttu-id="e3740-246">Les en-têtes de réponse sont disponibles par défaut sont :</span><span class="sxs-lookup"><span data-stu-id="e3740-246">The response headers that are available by default are:</span></span>

- <span data-ttu-id="e3740-247">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="e3740-247">Cache-Control</span></span>
- <span data-ttu-id="e3740-248">Content-Language</span><span class="sxs-lookup"><span data-stu-id="e3740-248">Content-Language</span></span>
- <span data-ttu-id="e3740-249">Content-Type</span><span class="sxs-lookup"><span data-stu-id="e3740-249">Content-Type</span></span>
- <span data-ttu-id="e3740-250">Arrive à expiration</span><span class="sxs-lookup"><span data-stu-id="e3740-250">Expires</span></span>
- <span data-ttu-id="e3740-251">Dernière modification</span><span class="sxs-lookup"><span data-stu-id="e3740-251">Last-Modified</span></span>
- <span data-ttu-id="e3740-252">pragma</span><span class="sxs-lookup"><span data-stu-id="e3740-252">Pragma</span></span>

<span data-ttu-id="e3740-253">La spécification CORS appelle ces en-têtes : [les en-têtes de réponse simple](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="e3740-253">The CORS spec calls these [simple response headers](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span></span> <span data-ttu-id="e3740-254">Pour que les autres en-têtes accessibles à l’application, définissez la *exposedHeaders* paramètre de **[EnableCors]**.</span><span class="sxs-lookup"><span data-stu-id="e3740-254">To make other headers available to the application, set the *exposedHeaders* parameter of **[EnableCors]**.</span></span>

<span data-ttu-id="e3740-255">Dans de l’exemple suivant, le contrôleur `Get` méthode définit un en-tête personnalisé nommé « X-Custom-Header ».</span><span class="sxs-lookup"><span data-stu-id="e3740-255">In the following example, the controller's `Get` method sets a custom header named ‘X-Custom-Header'.</span></span> <span data-ttu-id="e3740-256">Par défaut, le navigateur exposera pas cet en-tête dans une demande de cross-origin.</span><span class="sxs-lookup"><span data-stu-id="e3740-256">By default, the browser will not expose this header in a cross-origin request.</span></span> <span data-ttu-id="e3740-257">Pour libérer de l’en-tête, inclure « X-Custom-Header » dans *exposedHeaders*.</span><span class="sxs-lookup"><span data-stu-id="e3740-257">To make the header available, include ‘X-Custom-Header' in *exposedHeaders*.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

## <a name="pass-credentials-in-cross-origin-requests"></a><span data-ttu-id="e3740-258">Transmettre des informations d’identification dans les demandes cross-origin</span><span class="sxs-lookup"><span data-stu-id="e3740-258">Pass credentials in cross-origin requests</span></span>

<span data-ttu-id="e3740-259">Les informations d’identification nécessitent un traitement particulier dans une demande CORS.</span><span class="sxs-lookup"><span data-stu-id="e3740-259">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="e3740-260">Par défaut, le navigateur n’envoie pas d’informations d’identification avec une demande de cross-origin.</span><span class="sxs-lookup"><span data-stu-id="e3740-260">By default, the browser does not send any credentials with a cross-origin request.</span></span> <span data-ttu-id="e3740-261">Les informations d’identification incluent les cookies, ainsi que des schémas d’authentification HTTP.</span><span class="sxs-lookup"><span data-stu-id="e3740-261">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="e3740-262">Pour envoyer des informations d’identification avec une demande de cross-origin, le client doit définir **XMLHttpRequest.withCredentials** sur true.</span><span class="sxs-lookup"><span data-stu-id="e3740-262">To send credentials with a cross-origin request, the client must set **XMLHttpRequest.withCredentials** to true.</span></span>

<span data-ttu-id="e3740-263">À l’aide de **XMLHttpRequest** directement :</span><span class="sxs-lookup"><span data-stu-id="e3740-263">Using **XMLHttpRequest** directly:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

<span data-ttu-id="e3740-264">Dans jQuery :</span><span class="sxs-lookup"><span data-stu-id="e3740-264">In jQuery:</span></span>

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

<span data-ttu-id="e3740-265">En outre, le serveur doit autoriser les informations d’identification.</span><span class="sxs-lookup"><span data-stu-id="e3740-265">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="e3740-266">Pour autoriser les informations d’identification de cross-origine dans l’API Web, définissez la **SupportsCredentials** propriété sur true sur le **[EnableCors]** attribut :</span><span class="sxs-lookup"><span data-stu-id="e3740-266">To allow cross-origin credentials in Web API, set the **SupportsCredentials** property to true on the **[EnableCors]** attribute:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

<span data-ttu-id="e3740-267">Si cette propriété est true, la réponse HTTP inclut un en-tête Access-Control-Allow-Credentials.</span><span class="sxs-lookup"><span data-stu-id="e3740-267">If this property is true, the HTTP response will include an Access-Control-Allow-Credentials header.</span></span> <span data-ttu-id="e3740-268">Cet en-tête indique au navigateur que le serveur autorise les informations d’identification pour une demande de cross-origin.</span><span class="sxs-lookup"><span data-stu-id="e3740-268">This header tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="e3740-269">Si le navigateur envoie les informations d’identification, mais la réponse n’inclut pas un en-tête Access-Control-Allow-Credentials valid, le navigateur n’expose la réponse à l’application, et la requête AJAX échoue.</span><span class="sxs-lookup"><span data-stu-id="e3740-269">If the browser sends credentials, but the response does not include a valid Access-Control-Allow-Credentials header, the browser will not expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="e3740-270">Soyez prudent sur la configuration **SupportsCredentials** sur true, car cela signifie que d’un site Web à un autre domaine peut envoyer connecté de l’utilisateur à votre API Web sur l’utilisateur, sans que l’utilisateur en cours prenant en charge.</span><span class="sxs-lookup"><span data-stu-id="e3740-270">Be careful about setting **SupportsCredentials** to true, because it means a website at another domain can send a logged-in user's credentials to your Web API on the user's behalf, without the user being aware.</span></span> <span data-ttu-id="e3740-271">La spécification CORS indique également ce paramètre *origines* à &quot; \* &quot; n’est pas valide si **SupportsCredentials** a la valeur true.</span><span class="sxs-lookup"><span data-stu-id="e3740-271">The CORS spec also states that setting *origins* to &quot;\*&quot; is invalid if **SupportsCredentials** is true.</span></span>

## <a name="custom-cors-policy-providers"></a><span data-ttu-id="e3740-272">Fournisseurs de stratégie CORS personnalisés</span><span class="sxs-lookup"><span data-stu-id="e3740-272">Custom CORS policy providers</span></span>

<span data-ttu-id="e3740-273">Le **[EnableCors]** attribut implémente le **ICorsPolicyProvider** interface.</span><span class="sxs-lookup"><span data-stu-id="e3740-273">The **[EnableCors]** attribute implements the **ICorsPolicyProvider** interface.</span></span> <span data-ttu-id="e3740-274">Vous pouvez fournir votre propre implémentation en créant une classe qui dérive de **attribut** et implémente **ICorsProlicyProvider**.</span><span class="sxs-lookup"><span data-stu-id="e3740-274">You can provide your own implementation by creating a class that derives from **Attribute** and implements **ICorsProlicyProvider**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

<span data-ttu-id="e3740-275">Vous pouvez maintenant appliquer l’attribut partout, que vous devez placer **[EnableCors]**.</span><span class="sxs-lookup"><span data-stu-id="e3740-275">Now you can apply the attribute any place that you would put **[EnableCors]**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

<span data-ttu-id="e3740-276">Par exemple, un fournisseur de stratégie CORS personnalisé peut lire les paramètres à partir d’un fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="e3740-276">For example, a custom CORS policy provider could read the settings from a configuration file.</span></span>

<span data-ttu-id="e3740-277">Comme alternative à l’aide d’attributs, vous pouvez inscrire un **ICorsPolicyProviderFactory** objet crée **ICorsPolicyProvider** objets.</span><span class="sxs-lookup"><span data-stu-id="e3740-277">As an alternative to using attributes, you can register an **ICorsPolicyProviderFactory** object that creates **ICorsPolicyProvider** objects.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

<span data-ttu-id="e3740-278">Pour définir le **ICorsPolicyProviderFactory**, appelez le **SetCorsPolicyProviderFactory** méthode d’extension au démarrage, comme suit :</span><span class="sxs-lookup"><span data-stu-id="e3740-278">To set the **ICorsPolicyProviderFactory**, call the **SetCorsPolicyProviderFactory** extension method at startup, as follows:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

## <a name="browser-support"></a><span data-ttu-id="e3740-279">Prise en charge du navigateur</span><span class="sxs-lookup"><span data-stu-id="e3740-279">Browser support</span></span>

<span data-ttu-id="e3740-280">Le package de l’API Web CORS est une technologie côté serveur.</span><span class="sxs-lookup"><span data-stu-id="e3740-280">The Web API CORS package is a server-side technology.</span></span> <span data-ttu-id="e3740-281">Navigateur de l’utilisateur doit également prendre en charge de CORS.</span><span class="sxs-lookup"><span data-stu-id="e3740-281">The user's browser also needs to support CORS.</span></span> <span data-ttu-id="e3740-282">Heureusement, les versions actuelles de tous les principaux navigateurs incluent [prise en charge de CORS](http://caniuse.com/cors).</span><span class="sxs-lookup"><span data-stu-id="e3740-282">Fortunately, the current versions of all major browsers include [support for CORS](http://caniuse.com/cors).</span></span>
