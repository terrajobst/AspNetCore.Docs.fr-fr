---
title: Utiliser JavaScriptServices pour créer des Applications à Page unique dans ASP.NET Core
author: scottaddie
description: En savoir plus sur les avantages de l’utilisation de JavaScriptServices pour créer une Application à Page unique (SPA) soutenu par ASP.NET Core.
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
uid: client-side/spa-services
ms.openlocfilehash: b0fc6be29e3ecedd9706238f439f229377bb5a63
ms.sourcegitcommit: ad28d1bc6657a743d5c2fa8902f82740689733bb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52256548"
---
# <a name="use-javascriptservices-to-create-single-page-applications-in-aspnet-core"></a><span data-ttu-id="9d549-103">Utiliser JavaScriptServices pour créer des Applications à Page unique dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9d549-103">Use JavaScriptServices to Create Single Page Applications in ASP.NET Core</span></span>

<span data-ttu-id="9d549-104">Par [Scott Addie](https://github.com/scottaddie) et [Fiyaz Hasan](http://fiyazhasan.me/)</span><span class="sxs-lookup"><span data-stu-id="9d549-104">By [Scott Addie](https://github.com/scottaddie) and [Fiyaz Hasan](http://fiyazhasan.me/)</span></span>

<span data-ttu-id="9d549-105">Une Application à Page unique (SPA) est un type d’application web en raison de son expérience utilisateur riche inhérente.</span><span class="sxs-lookup"><span data-stu-id="9d549-105">A Single Page Application (SPA) is a popular type of web application due to its inherent rich user experience.</span></span> <span data-ttu-id="9d549-106">L’intégration de frameworks SPA côté client ou les bibliothèques, telles que [Angular](https://angular.io/) ou [réagir](https://facebook.github.io/react/), avec les infrastructures côté serveur comme ASP.NET Core peut s’avérer difficile.</span><span class="sxs-lookup"><span data-stu-id="9d549-106">Integrating client-side SPA frameworks or libraries, such as [Angular](https://angular.io/) or [React](https://facebook.github.io/react/), with server-side frameworks like ASP.NET Core can be difficult.</span></span> <span data-ttu-id="9d549-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) a été développé pour réduire la friction lors du processus d’intégration.</span><span class="sxs-lookup"><span data-stu-id="9d549-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) was developed to reduce friction in the integration process.</span></span> <span data-ttu-id="9d549-108">Il permet à une opération transparente entre les piles de technologie de serveur et de client.</span><span class="sxs-lookup"><span data-stu-id="9d549-108">It enables seamless operation between the different client and server technology stacks.</span></span>

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a><span data-ttu-id="9d549-109">Qu’est JavaScriptServices</span><span class="sxs-lookup"><span data-stu-id="9d549-109">What is JavaScriptServices</span></span>

<span data-ttu-id="9d549-110">JavaScriptServices est une collection de technologies côté client pour ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9d549-110">JavaScriptServices is a collection of client-side technologies for ASP.NET Core.</span></span> <span data-ttu-id="9d549-111">Son objectif est de positionner ASP.NET Core en tant que côté serveur conseillée développeurs pour la création d’applications à page unique.</span><span class="sxs-lookup"><span data-stu-id="9d549-111">Its goal is to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span>

<span data-ttu-id="9d549-112">JavaScriptServices se compose de trois packages NuGet distinctes :</span><span class="sxs-lookup"><span data-stu-id="9d549-112">JavaScriptServices consists of three distinct NuGet packages:</span></span>

* <span data-ttu-id="9d549-113">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span><span class="sxs-lookup"><span data-stu-id="9d549-113">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span></span>
* <span data-ttu-id="9d549-114">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span><span class="sxs-lookup"><span data-stu-id="9d549-114">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span></span>
* <span data-ttu-id="9d549-115">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span><span class="sxs-lookup"><span data-stu-id="9d549-115">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span></span>

<span data-ttu-id="9d549-116">Ces packages sont utiles si vous :</span><span class="sxs-lookup"><span data-stu-id="9d549-116">These packages are useful if you:</span></span>

* <span data-ttu-id="9d549-117">Exécuter JavaScript sur le serveur</span><span class="sxs-lookup"><span data-stu-id="9d549-117">Run JavaScript on the server</span></span>
* <span data-ttu-id="9d549-118">Utiliser un framework SPA ou une bibliothèque</span><span class="sxs-lookup"><span data-stu-id="9d549-118">Use a SPA framework or library</span></span>
* <span data-ttu-id="9d549-119">Générez les ressources côté client avec Webpack</span><span class="sxs-lookup"><span data-stu-id="9d549-119">Build client-side assets with Webpack</span></span>

<span data-ttu-id="9d549-120">Une grande partie de cet article met l’accent est placée sur l’utilisation du package SpaServices.</span><span class="sxs-lookup"><span data-stu-id="9d549-120">Much of the focus in this article is placed on using the SpaServices package.</span></span>

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a><span data-ttu-id="9d549-121">Qu’est SpaServices</span><span class="sxs-lookup"><span data-stu-id="9d549-121">What is SpaServices</span></span>

<span data-ttu-id="9d549-122">SpaServices a été créé pour positionner ASP.NET Core en tant que côté serveur conseillée développeurs pour la création d’applications à page unique.</span><span class="sxs-lookup"><span data-stu-id="9d549-122">SpaServices was created to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span> <span data-ttu-id="9d549-123">SpaServices n’est pas requis pour développer des applications à page unique avec ASP.NET Core, et il ne vous dans une infrastructure client particulier.</span><span class="sxs-lookup"><span data-stu-id="9d549-123">SpaServices isn't required to develop SPAs with ASP.NET Core, and it doesn't lock you into a particular client framework.</span></span>

<span data-ttu-id="9d549-124">SpaServices fournit infrastructure utile telles que :</span><span class="sxs-lookup"><span data-stu-id="9d549-124">SpaServices provides useful infrastructure such as:</span></span>

* [<span data-ttu-id="9d549-125">Côté serveur pré-rendu</span><span class="sxs-lookup"><span data-stu-id="9d549-125">Server-side prerendering</span></span>](#server-prerendering)
* [<span data-ttu-id="9d549-126">Intergiciel (middleware) de Webpack Dev</span><span class="sxs-lookup"><span data-stu-id="9d549-126">Webpack Dev Middleware</span></span>](#webpack-dev-middleware)
* [<span data-ttu-id="9d549-127">Remplacement d’un Module à chaud</span><span class="sxs-lookup"><span data-stu-id="9d549-127">Hot Module Replacement</span></span>](#hot-module-replacement)
* [<span data-ttu-id="9d549-128">Programmes d’assistance de routage</span><span class="sxs-lookup"><span data-stu-id="9d549-128">Routing helpers</span></span>](#routing-helpers)

<span data-ttu-id="9d549-129">Collectivement, ces composants d’infrastructure améliorent le flux de travail de développement et de l’expérience de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="9d549-129">Collectively, these infrastructure components enhance both the development workflow and the runtime experience.</span></span> <span data-ttu-id="9d549-130">Les composants peuvent être adoptés individuellement.</span><span class="sxs-lookup"><span data-stu-id="9d549-130">The components can be adopted individually.</span></span>

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a><span data-ttu-id="9d549-131">Conditions préalables pour l’utilisation de SpaServices</span><span class="sxs-lookup"><span data-stu-id="9d549-131">Prerequisites for using SpaServices</span></span>

<span data-ttu-id="9d549-132">Pour utiliser SpaServices, installez les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="9d549-132">To work with SpaServices, install the following:</span></span>

* <span data-ttu-id="9d549-133">[Node.js](https://nodejs.org/) (version 6 ou version ultérieure) avec npm</span><span class="sxs-lookup"><span data-stu-id="9d549-133">[Node.js](https://nodejs.org/) (version 6 or later) with npm</span></span>
  * <span data-ttu-id="9d549-134">Pour vérifier ces composants sont installés et sont accessibles, exécutez la commande suivante à partir de la ligne de commande :</span><span class="sxs-lookup"><span data-stu-id="9d549-134">To verify these components are installed and can be found, run the following from the command line:</span></span>

    ```console
    node -v && npm -v
    ```

<span data-ttu-id="9d549-135">Remarque : Si vous effectuez un déploiement vers un site web Azure, vous n’avez à faire quoi que ce soit ici &mdash; Node.js est installé et disponible dans les environnements de serveur.</span><span class="sxs-lookup"><span data-stu-id="9d549-135">Note: If you're deploying to an Azure web site, you don't need to do anything here &mdash; Node.js is installed and available in the server environments.</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * <span data-ttu-id="9d549-136">Si vous êtes sur Windows à l’aide de Visual Studio 2017, le SDK est installé en sélectionnant le **.NET Core le développement multiplateforme** charge de travail.</span><span class="sxs-lookup"><span data-stu-id="9d549-136">If you're on Windows using Visual Studio 2017, the SDK is installed by selecting the **.NET Core cross-platform development** workload.</span></span>

* <span data-ttu-id="9d549-137">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) package NuGet</span><span class="sxs-lookup"><span data-stu-id="9d549-137">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet package</span></span>

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a><span data-ttu-id="9d549-138">Côté serveur pré-rendu</span><span class="sxs-lookup"><span data-stu-id="9d549-138">Server-side prerendering</span></span>

<span data-ttu-id="9d549-139">Une application universelle (également appelé isomorphes) est une application JavaScript capable d’exécuter à la fois sur le serveur et le client.</span><span class="sxs-lookup"><span data-stu-id="9d549-139">A universal (also known as isomorphic) application is a JavaScript application capable of running both on the server and the client.</span></span> <span data-ttu-id="9d549-140">Angular, React et autres infrastructures populaires fournissent une plateforme universelle pour ce style de développement d’application.</span><span class="sxs-lookup"><span data-stu-id="9d549-140">Angular, React, and other popular frameworks provide a universal platform for this application development style.</span></span> <span data-ttu-id="9d549-141">L’idée est de rendre tout d’abord les composants d’infrastructure sur le serveur via Node.js, puis de déléguer davantage d’exécution au client.</span><span class="sxs-lookup"><span data-stu-id="9d549-141">The idea is to first render the framework components on the server via Node.js, and then delegate further execution to the client.</span></span>

<span data-ttu-id="9d549-142">ASP.NET Core [Tag Helpers](xref:mvc/views/tag-helpers/intro) fournie par SpaServices simplifier l’implémentation de pré-rendu de côté serveur en appelant les fonctions JavaScript sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="9d549-142">ASP.NET Core [Tag Helpers](xref:mvc/views/tag-helpers/intro) provided by SpaServices simplify the implementation of server-side prerendering by invoking the JavaScript functions on the server.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="9d549-143">Prérequis</span><span class="sxs-lookup"><span data-stu-id="9d549-143">Prerequisites</span></span>

<span data-ttu-id="9d549-144">Installez les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="9d549-144">Install the following:</span></span>

* <span data-ttu-id="9d549-145">[ASPNET-pré-rendu](https://www.npmjs.com/package/aspnet-prerendering) package npm :</span><span class="sxs-lookup"><span data-stu-id="9d549-145">[aspnet-prerendering](https://www.npmjs.com/package/aspnet-prerendering) npm package:</span></span>

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a><span data-ttu-id="9d549-146">Configuration</span><span class="sxs-lookup"><span data-stu-id="9d549-146">Configuration</span></span>

<span data-ttu-id="9d549-147">Les Tag Helpers rendus détectables par le biais de l’inscription d’espace de noms dans le projet *_ViewImports.cshtml* fichier :</span><span class="sxs-lookup"><span data-stu-id="9d549-147">The Tag Helpers are made discoverable via namespace registration in the project's *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

<span data-ttu-id="9d549-148">Ces Tag Helpers clarifient les subtilités de communiquer directement avec les API de bas niveau en tirant parti d’une syntaxe de type HTML à l’intérieur de la vue Razor :</span><span class="sxs-lookup"><span data-stu-id="9d549-148">These Tag Helpers abstract away the intricacies of communicating directly with low-level APIs by leveraging an HTML-like syntax inside the Razor view:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a><span data-ttu-id="9d549-149">Le `asp-prerender-module` Tag Helper</span><span class="sxs-lookup"><span data-stu-id="9d549-149">The `asp-prerender-module` Tag Helper</span></span>

<span data-ttu-id="9d549-150">Le `asp-prerender-module` Tag Helper, utilisée dans l’exemple de code précédent, exécute *ClientApp/dist/main-server.js* sur le serveur via Node.js.</span><span class="sxs-lookup"><span data-stu-id="9d549-150">The `asp-prerender-module` Tag Helper, used in the preceding code example, executes *ClientApp/dist/main-server.js* on the server via Node.js.</span></span> <span data-ttu-id="9d549-151">Par souci de clarté, *main-server.js* fichier est un artefact de la tâche de transpilation de TypeScript et JavaScript dans le [Webpack](http://webpack.github.io/) du processus de génération.</span><span class="sxs-lookup"><span data-stu-id="9d549-151">For clarity's sake, *main-server.js* file is an artifact of the TypeScript-to-JavaScript transpilation task in the [Webpack](http://webpack.github.io/) build process.</span></span> <span data-ttu-id="9d549-152">Webpack définit un alias de point d’entrée de `main-server`; et commence le parcours du graphique de dépendance pour cet alias le *ClientApp/démarrage-server.ts* fichier :</span><span class="sxs-lookup"><span data-stu-id="9d549-152">Webpack defines an entry point alias of `main-server`; and, traversal of the dependency graph for this alias begins at the *ClientApp/boot-server.ts* file:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

<span data-ttu-id="9d549-153">Dans l’exemple suivant Angular, le *ClientApp/démarrage-server.ts* fichier utilise le `createServerRenderer` (fonction) et `RenderResult` le type de la `aspnet-prerendering` package npm pour configurer le rendu du serveur via Node.js.</span><span class="sxs-lookup"><span data-stu-id="9d549-153">In the following Angular example, the *ClientApp/boot-server.ts* file utilizes the `createServerRenderer` function and `RenderResult` type of the `aspnet-prerendering` npm package to configure server rendering via Node.js.</span></span> <span data-ttu-id="9d549-154">Le balisage HTML destiné à rendu côté serveur est passé à un appel de fonction de résolution, lequel est encapsulé dans un script JavaScript fortement typée `Promise` objet.</span><span class="sxs-lookup"><span data-stu-id="9d549-154">The HTML markup destined for server-side rendering is passed to a resolve function call, which is wrapped in a strongly-typed JavaScript `Promise` object.</span></span> <span data-ttu-id="9d549-155">Le `Promise` importance de l’objet est qu’il fournit en mode asynchrone le balisage HTML pour la page pour l’injection dans l’élément d’espace réservé du DOM.</span><span class="sxs-lookup"><span data-stu-id="9d549-155">The `Promise` object's significance is that it asynchronously supplies the HTML markup to the page for injection in the DOM's placeholder element.</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a><span data-ttu-id="9d549-156">Le `asp-prerender-data` Tag Helper</span><span class="sxs-lookup"><span data-stu-id="9d549-156">The `asp-prerender-data` Tag Helper</span></span>

<span data-ttu-id="9d549-157">Associé à la `asp-prerender-module` Tag Helper, le `asp-prerender-data` Tag Helper peut être utilisé pour transmettre des informations contextuelles à partir de la vue Razor pour le code JavaScript côté serveur.</span><span class="sxs-lookup"><span data-stu-id="9d549-157">When coupled with the `asp-prerender-module` Tag Helper, the `asp-prerender-data` Tag Helper can be used to pass contextual information from the Razor view to the server-side JavaScript.</span></span> <span data-ttu-id="9d549-158">Par exemple, le balisage suivant transmet les données utilisateur à la `main-server` module :</span><span class="sxs-lookup"><span data-stu-id="9d549-158">For example, the following markup passes user data to the `main-server` module:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

<span data-ttu-id="9d549-159">La réponse reçue `UserName` argument est sérialisé à l’aide du sérialiseur JSON intégré et est stocké dans le `params.data` objet.</span><span class="sxs-lookup"><span data-stu-id="9d549-159">The received `UserName` argument is serialized using the built-in JSON serializer and is stored in the `params.data` object.</span></span> <span data-ttu-id="9d549-160">Dans l’exemple suivant Angular, les données sont utilisées pour construire un message d’accueil personnalisé dans un `h1` élément :</span><span class="sxs-lookup"><span data-stu-id="9d549-160">In the following Angular example, the data is used to construct a personalized greeting within an `h1` element:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

<span data-ttu-id="9d549-161">Remarque : Les noms de propriété passés dans les Tag Helpers sont représentés par **la casse Pascal** notation.</span><span class="sxs-lookup"><span data-stu-id="9d549-161">Note: Property names passed in Tag Helpers are represented with **PascalCase** notation.</span></span> <span data-ttu-id="9d549-162">Comparez cela à JavaScript, où les mêmes noms de propriété sont représentés par **une casse mixte**.</span><span class="sxs-lookup"><span data-stu-id="9d549-162">Contrast that to JavaScript, where the same property names are represented with **camelCase**.</span></span> <span data-ttu-id="9d549-163">La configuration de sérialisation JSON par défaut est chargée de cette différence.</span><span class="sxs-lookup"><span data-stu-id="9d549-163">The default JSON serialization configuration is responsible for this difference.</span></span>

<span data-ttu-id="9d549-164">Pour étendre l’exemple de code précédent, données peuvent être transmises à partir du serveur à la vue par HYDRATATION le `globals` propriété fournie à la `resolve` (fonction) :</span><span class="sxs-lookup"><span data-stu-id="9d549-164">To expand upon the preceding code example, data can be passed from the server to the view by hydrating the `globals` property provided to the `resolve` function:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

<span data-ttu-id="9d549-165">Le `postList` tableau défini à l’intérieur de la `globals` objet est attaché à du navigateur global `window` objet.</span><span class="sxs-lookup"><span data-stu-id="9d549-165">The `postList` array defined inside the `globals` object is attached to the browser's global `window` object.</span></span> <span data-ttu-id="9d549-166">Cette variable levage à portée globale permet d’éliminer une duplication des efforts, en particulier en ce qui concerne le chargement des données mêmes qu’une seule fois sur le serveur et à nouveau sur le client.</span><span class="sxs-lookup"><span data-stu-id="9d549-166">This variable hoisting to global scope eliminates duplication of effort, particularly as it pertains to loading the same data once on the server and again on the client.</span></span>

![(variable globale) postList attaché à l’objet de fenêtre](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a><span data-ttu-id="9d549-168">Intergiciel (middleware) de Webpack Dev</span><span class="sxs-lookup"><span data-stu-id="9d549-168">Webpack Dev Middleware</span></span>

<span data-ttu-id="9d549-169">[Intergiciel (middleware) de Webpack Dev](https://webpack.github.io/docs/webpack-dev-middleware.html) introduit un flux de travail de développement simplifié par laquelle Webpack génère des ressources à la demande.</span><span class="sxs-lookup"><span data-stu-id="9d549-169">[Webpack Dev Middleware](https://webpack.github.io/docs/webpack-dev-middleware.html) introduces a streamlined development workflow whereby Webpack builds resources on demand.</span></span> <span data-ttu-id="9d549-170">L’intergiciel (middleware) compile automatiquement et fournit des ressources de côté client lorsqu’une page est rechargée dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="9d549-170">The middleware automatically compiles and serves client-side resources when a page is reloaded in the browser.</span></span> <span data-ttu-id="9d549-171">L’autre approche consiste à appeler manuellement la Webpack via un script de génération du projet npm quand une dépendance tierce ou le code personnalisé change.</span><span class="sxs-lookup"><span data-stu-id="9d549-171">The alternate approach is to manually invoke Webpack via the project's npm build script when a third-party dependency or the custom code changes.</span></span> <span data-ttu-id="9d549-172">Script de compilation d’un npm la *package.json* fichier est illustré dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="9d549-172">An npm build script in the *package.json* file is shown in the following example:</span></span>

```json
"build": "npm run build:vendor && npm run build:custom",
```

### <a name="prerequisites"></a><span data-ttu-id="9d549-173">Prérequis</span><span class="sxs-lookup"><span data-stu-id="9d549-173">Prerequisites</span></span>

<span data-ttu-id="9d549-174">Installez les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="9d549-174">Install the following:</span></span>

* <span data-ttu-id="9d549-175">[ASPNET-webpack](https://www.npmjs.com/package/aspnet-webpack) package npm :</span><span class="sxs-lookup"><span data-stu-id="9d549-175">[aspnet-webpack](https://www.npmjs.com/package/aspnet-webpack) npm package:</span></span>

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a><span data-ttu-id="9d549-176">Configuration</span><span class="sxs-lookup"><span data-stu-id="9d549-176">Configuration</span></span>

<span data-ttu-id="9d549-177">Intergiciel (middleware) de Webpack Dev est inscrit dans le pipeline de requêtes HTTP via le code suivant dans le *Startup.cs* du fichier `Configure` méthode :</span><span class="sxs-lookup"><span data-stu-id="9d549-177">Webpack Dev Middleware is registered into the HTTP request pipeline via the following code in the *Startup.cs* file's `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=webpack-middleware-registration&highlight=4)]

<span data-ttu-id="9d549-178">Le `UseWebpackDevMiddleware` méthode d’extension doit être appelée avant [l’inscription de l’hébergement de fichier statique](xref:fundamentals/static-files) via la `UseStaticFiles` méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="9d549-178">The `UseWebpackDevMiddleware` extension method must be called before [registering static file hosting](xref:fundamentals/static-files) via the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="9d549-179">Pour des raisons de sécurité, inscrivez le middleware uniquement lorsque l’application s’exécute en mode de développement.</span><span class="sxs-lookup"><span data-stu-id="9d549-179">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="9d549-180">Le *webpack.config.js* du fichier `output.publicPath` propriété indique à l’intergiciel (middleware) pour surveiller le `dist` dossier pour les modifications :</span><span class="sxs-lookup"><span data-stu-id="9d549-180">The *webpack.config.js* file's `output.publicPath` property tells the middleware to watch the `dist` folder for changes:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a><span data-ttu-id="9d549-181">Remplacement d’un Module à chaud</span><span class="sxs-lookup"><span data-stu-id="9d549-181">Hot Module Replacement</span></span>

<span data-ttu-id="9d549-182">Pensez à Webpack [remplacement à chaud de Module](https://webpack.js.org/concepts/hot-module-replacement/) fonctionnalité (HMR) comme une évolution de [intergiciel (middleware) de Webpack Dev](#webpack-dev-middleware).</span><span class="sxs-lookup"><span data-stu-id="9d549-182">Think of Webpack's [Hot Module Replacement](https://webpack.js.org/concepts/hot-module-replacement/) (HMR) feature as an evolution of [Webpack Dev Middleware](#webpack-dev-middleware).</span></span> <span data-ttu-id="9d549-183">HMR présente les mêmes avantages, mais elle simplifie davantage le flux de travail de développement en mettant à jour automatiquement de contenu de la page après la compilation les modifications.</span><span class="sxs-lookup"><span data-stu-id="9d549-183">HMR introduces all the same benefits, but it further streamlines the development workflow by automatically updating page content after compiling the changes.</span></span> <span data-ttu-id="9d549-184">À ne pas confondre avec une actualisation du navigateur, ce qui entraînerait une interférence avec l’état en mémoire actuel et la session de débogage de l’application SPA.</span><span class="sxs-lookup"><span data-stu-id="9d549-184">Don't confuse this with a refresh of the browser, which would interfere with the current in-memory state and debugging session of the SPA.</span></span> <span data-ttu-id="9d549-185">Il existe un lien direct entre le service de l’intergiciel (middleware) de Webpack développement et le navigateur, ce qui signifie que les modifications sont envoyées au navigateur.</span><span class="sxs-lookup"><span data-stu-id="9d549-185">There's a live link between the Webpack Dev Middleware service and the browser, which means changes are pushed to the browser.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="9d549-186">Prérequis</span><span class="sxs-lookup"><span data-stu-id="9d549-186">Prerequisites</span></span>

<span data-ttu-id="9d549-187">Installez les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="9d549-187">Install the following:</span></span>

* <span data-ttu-id="9d549-188">[intergiciel chaud Webpack](https://www.npmjs.com/package/webpack-hot-middleware) package npm :</span><span class="sxs-lookup"><span data-stu-id="9d549-188">[webpack-hot-middleware](https://www.npmjs.com/package/webpack-hot-middleware) npm package:</span></span>

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a><span data-ttu-id="9d549-189">Configuration</span><span class="sxs-lookup"><span data-stu-id="9d549-189">Configuration</span></span>

<span data-ttu-id="9d549-190">Le composant HMR doit être enregistré dans le pipeline de demande HTTP de MVC dans le `Configure` méthode :</span><span class="sxs-lookup"><span data-stu-id="9d549-190">The HMR component must be registered into MVC's HTTP request pipeline in the `Configure` method:</span></span>

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

<span data-ttu-id="9d549-191">En tant qu’était le cas avec [intergiciel (middleware) de Webpack Dev](#webpack-dev-middleware), le `UseWebpackDevMiddleware` méthode d’extension doit être appelée avant la `UseStaticFiles` méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="9d549-191">As was true with [Webpack Dev Middleware](#webpack-dev-middleware), the `UseWebpackDevMiddleware` extension method must be called before the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="9d549-192">Pour des raisons de sécurité, inscrivez le middleware uniquement lorsque l’application s’exécute en mode de développement.</span><span class="sxs-lookup"><span data-stu-id="9d549-192">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="9d549-193">Le *webpack.config.js* fichier doit définir un `plugins` de tableau, même si celui-ci est laissé vide :</span><span class="sxs-lookup"><span data-stu-id="9d549-193">The *webpack.config.js* file must define a `plugins` array, even if it's left empty:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

<span data-ttu-id="9d549-194">Après le chargement de l’application dans le navigateur, onglet de la Console outils de développement fournit la confirmation de l’activation de HMR :</span><span class="sxs-lookup"><span data-stu-id="9d549-194">After loading the app in the browser, the developer tools' Console tab provides confirmation of HMR activation:</span></span>

![Message de connexion de remplacement d’un Module à chaud](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a><span data-ttu-id="9d549-196">Programmes d’assistance de routage</span><span class="sxs-lookup"><span data-stu-id="9d549-196">Routing helpers</span></span>

<span data-ttu-id="9d549-197">Dans la plupart des SPA basé sur ASP.NET Core, vous souhaiterez routage côté client en plus du routage côté serveur.</span><span class="sxs-lookup"><span data-stu-id="9d549-197">In most ASP.NET Core-based SPAs, you'll want client-side routing in addition to server-side routing.</span></span> <span data-ttu-id="9d549-198">Les systèmes de routage SPA et MVC peuvent travailler indépendamment sans interférence.</span><span class="sxs-lookup"><span data-stu-id="9d549-198">The SPA and MVC routing systems can work independently without interference.</span></span> <span data-ttu-id="9d549-199">Il existe, toutefois, un bord cas posant défis : identification des réponses HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="9d549-199">There's, however, one edge case posing challenges: identifying 404 HTTP responses.</span></span>

<span data-ttu-id="9d549-200">Considérez le scénario dans lequel un itinéraire sans extension de `/some/page` est utilisé.</span><span class="sxs-lookup"><span data-stu-id="9d549-200">Consider the scenario in which an extensionless route of `/some/page` is used.</span></span> <span data-ttu-id="9d549-201">Supposons que la demande n’à la correspondance un itinéraire côté serveur, mais son modèle ne correspond pas à un itinéraire côté client.</span><span class="sxs-lookup"><span data-stu-id="9d549-201">Assume the request doesn't pattern-match a server-side route, but its pattern does match a client-side route.</span></span> <span data-ttu-id="9d549-202">Examinons à présent une demande entrante pour `/images/user-512.png`, lequel attend généralement rechercher un fichier image sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="9d549-202">Now consider an incoming request for `/images/user-512.png`, which generally expects to find an image file on the server.</span></span> <span data-ttu-id="9d549-203">Si ce chemin d’accès de la ressource demandée ne correspond pas à toute approche côté serveur ou un fichier statique, il est peu probable que l’application côté client il traiterait — il est généralement conseillé de retourner un code d’état HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="9d549-203">If that requested resource path doesn't match any server-side route or static file, it's unlikely that the client-side application would handle it — you generally want to return a 404 HTTP status code.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="9d549-204">Prérequis</span><span class="sxs-lookup"><span data-stu-id="9d549-204">Prerequisites</span></span>

<span data-ttu-id="9d549-205">Installez les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="9d549-205">Install the following:</span></span>

* <span data-ttu-id="9d549-206">Le package npm de routage côté client.</span><span class="sxs-lookup"><span data-stu-id="9d549-206">The client-side routing npm package.</span></span> <span data-ttu-id="9d549-207">À l’aide d’Angular par exemple :</span><span class="sxs-lookup"><span data-stu-id="9d549-207">Using Angular as an example:</span></span>

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a><span data-ttu-id="9d549-208">Configuration</span><span class="sxs-lookup"><span data-stu-id="9d549-208">Configuration</span></span>

<span data-ttu-id="9d549-209">Une méthode d’extension nommée `MapSpaFallbackRoute` est utilisé dans le `Configure` méthode :</span><span class="sxs-lookup"><span data-stu-id="9d549-209">An extension method named `MapSpaFallbackRoute` is used in the `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=mvc-routing-table&highlight=7-9)]

<span data-ttu-id="9d549-210">Conseil : Les itinéraires sont évaluées dans l’ordre dans lequel ils sont configurés.</span><span class="sxs-lookup"><span data-stu-id="9d549-210">Tip: Routes are evaluated in the order in which they're configured.</span></span> <span data-ttu-id="9d549-211">Par conséquent, le `default` itinéraire dans l’exemple de code précédent est utilisé pour les critères spéciaux.</span><span class="sxs-lookup"><span data-stu-id="9d549-211">Consequently, the `default` route in the preceding code example is used first for pattern matching.</span></span>

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a><span data-ttu-id="9d549-212">Création d’un projet</span><span class="sxs-lookup"><span data-stu-id="9d549-212">Creating a new project</span></span>

<span data-ttu-id="9d549-213">JavaScriptServices fournit des modèles d’application préconfigurés.</span><span class="sxs-lookup"><span data-stu-id="9d549-213">JavaScriptServices provides pre-configured application templates.</span></span> <span data-ttu-id="9d549-214">SpaServices est utilisé dans ces modèles, conjointement avec différentes infrastructures et bibliothèques, telles que Angular et React avec Redux.</span><span class="sxs-lookup"><span data-stu-id="9d549-214">SpaServices is used in these templates, in conjunction with different frameworks and libraries such as Angular, React, and Redux.</span></span>

<span data-ttu-id="9d549-215">Ces modèles peuvent être installés par le biais de l’interface CLI .NET Core en exécutant la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="9d549-215">These templates can be installed via the .NET Core CLI by running the following command:</span></span>

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

<span data-ttu-id="9d549-216">Une liste des modèles disponibles s’affiche :</span><span class="sxs-lookup"><span data-stu-id="9d549-216">A list of available SPA templates is displayed:</span></span>

| <span data-ttu-id="9d549-217">Modèles</span><span class="sxs-lookup"><span data-stu-id="9d549-217">Templates</span></span>                                 | <span data-ttu-id="9d549-218">Nom court</span><span class="sxs-lookup"><span data-stu-id="9d549-218">Short Name</span></span> | <span data-ttu-id="9d549-219">Langue</span><span class="sxs-lookup"><span data-stu-id="9d549-219">Language</span></span> | <span data-ttu-id="9d549-220">Balises</span><span class="sxs-lookup"><span data-stu-id="9d549-220">Tags</span></span>        |
|:------------------------------------------|:-----------|:---------|:------------|
| <span data-ttu-id="9d549-221">MVC ASP.NET Core avec Angular</span><span class="sxs-lookup"><span data-stu-id="9d549-221">MVC ASP.NET Core with Angular</span></span>             | <span data-ttu-id="9d549-222">angular</span><span class="sxs-lookup"><span data-stu-id="9d549-222">angular</span></span>    | <span data-ttu-id="9d549-223">[C#]</span><span class="sxs-lookup"><span data-stu-id="9d549-223">[C#]</span></span>     | <span data-ttu-id="9d549-224">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="9d549-224">Web/MVC/SPA</span></span> |
| <span data-ttu-id="9d549-225">MVC ASP.NET Core avec React.js</span><span class="sxs-lookup"><span data-stu-id="9d549-225">MVC ASP.NET Core with React.js</span></span>            | <span data-ttu-id="9d549-226">react</span><span class="sxs-lookup"><span data-stu-id="9d549-226">react</span></span>      | <span data-ttu-id="9d549-227">[C#]</span><span class="sxs-lookup"><span data-stu-id="9d549-227">[C#]</span></span>     | <span data-ttu-id="9d549-228">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="9d549-228">Web/MVC/SPA</span></span> |
| <span data-ttu-id="9d549-229">MVC ASP.NET Core avec React.js et Redux</span><span class="sxs-lookup"><span data-stu-id="9d549-229">MVC ASP.NET Core with React.js and Redux</span></span>  | <span data-ttu-id="9d549-230">reactredux</span><span class="sxs-lookup"><span data-stu-id="9d549-230">reactredux</span></span> | <span data-ttu-id="9d549-231">[C#]</span><span class="sxs-lookup"><span data-stu-id="9d549-231">[C#]</span></span>     | <span data-ttu-id="9d549-232">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="9d549-232">Web/MVC/SPA</span></span> |

<span data-ttu-id="9d549-233">Pour créer un nouveau projet à l’aide d’un des modèles SPA, incluez le **nom court** du modèle dans le [dotnet nouvelle](/dotnet/core/tools/dotnet-new) commande.</span><span class="sxs-lookup"><span data-stu-id="9d549-233">To create a new project using one of the SPA templates, include the **Short Name** of the template in the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="9d549-234">La commande suivante crée une application Angular avec ASP.NET Core MVC est configuré pour le côté serveur :</span><span class="sxs-lookup"><span data-stu-id="9d549-234">The following command creates an Angular application with ASP.NET Core MVC configured for the server side:</span></span>

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a><span data-ttu-id="9d549-235">Définir le mode de configuration du runtime</span><span class="sxs-lookup"><span data-stu-id="9d549-235">Set the runtime configuration mode</span></span>

<span data-ttu-id="9d549-236">Il existe deux modes de configuration de runtime principal :</span><span class="sxs-lookup"><span data-stu-id="9d549-236">Two primary runtime configuration modes exist:</span></span>

* <span data-ttu-id="9d549-237">**Développement**:</span><span class="sxs-lookup"><span data-stu-id="9d549-237">**Development**:</span></span>
  * <span data-ttu-id="9d549-238">Il inclut des mappages de source pour faciliter le débogage.</span><span class="sxs-lookup"><span data-stu-id="9d549-238">Includes source maps to ease debugging.</span></span>
  * <span data-ttu-id="9d549-239">N’Optimisez le code côté client pour les performances.</span><span class="sxs-lookup"><span data-stu-id="9d549-239">Doesn't optimize the client-side code for performance.</span></span>
* <span data-ttu-id="9d549-240">**Production**:</span><span class="sxs-lookup"><span data-stu-id="9d549-240">**Production**:</span></span>
  * <span data-ttu-id="9d549-241">Exclut les mappages de sources.</span><span class="sxs-lookup"><span data-stu-id="9d549-241">Excludes source maps.</span></span>
  * <span data-ttu-id="9d549-242">Optimise le code côté client via le regroupement et minimisation.</span><span class="sxs-lookup"><span data-stu-id="9d549-242">Optimizes the client-side code via bundling & minification.</span></span>

<span data-ttu-id="9d549-243">ASP.NET Core utilise une variable d’environnement nommée `ASPNETCORE_ENVIRONMENT` pour stocker le mode de configuration.</span><span class="sxs-lookup"><span data-stu-id="9d549-243">ASP.NET Core uses an environment variable named `ASPNETCORE_ENVIRONMENT` to store the configuration mode.</span></span> <span data-ttu-id="9d549-244">Consultez **[définir l’environnement](xref:fundamentals/environments#set-the-environment)** pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="9d549-244">See **[Set the environment](xref:fundamentals/environments#set-the-environment)** for more information.</span></span>

### <a name="running-with-net-core-cli"></a><span data-ttu-id="9d549-245">En cours d’exécution avec l’interface CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="9d549-245">Running with .NET Core CLI</span></span>

<span data-ttu-id="9d549-246">Restaurer le NuGet requis et les packages npm en exécutant la commande suivante à la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="9d549-246">Restore the required NuGet and npm packages by running the following command at the project root:</span></span>

```console
dotnet restore && npm i
```

<span data-ttu-id="9d549-247">Générer et exécuter l’application :</span><span class="sxs-lookup"><span data-stu-id="9d549-247">Build and run the application:</span></span>

```console
dotnet run
```

<span data-ttu-id="9d549-248">Démarrage de l’application sur l’hôte local en fonction de la [mode de configuration de runtime](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="9d549-248">The application starts on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> <span data-ttu-id="9d549-249">Navigation vers `http://localhost:5000` dans le navigateur affiche la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="9d549-249">Navigating to `http://localhost:5000` in the browser displays the landing page.</span></span>

### <a name="running-with-visual-studio-2017"></a><span data-ttu-id="9d549-250">En cours d’exécution avec Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="9d549-250">Running with Visual Studio 2017</span></span>

<span data-ttu-id="9d549-251">Ouvrez le *.csproj* fichier généré par le [dotnet nouvelle](/dotnet/core/tools/dotnet-new) commande.</span><span class="sxs-lookup"><span data-stu-id="9d549-251">Open the *.csproj* file generated by the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="9d549-252">Les packages NuGet et npm nécessaires sont restaurés automatiquement sur le projet ouvert.</span><span class="sxs-lookup"><span data-stu-id="9d549-252">The required NuGet and npm packages are restored automatically upon project open.</span></span> <span data-ttu-id="9d549-253">Ce processus de restauration peut prendre quelques minutes, et l’application est prête à exécuter lorsqu’elle est terminée.</span><span class="sxs-lookup"><span data-stu-id="9d549-253">This restoration process may take up to a few minutes, and the application is ready to run when it completes.</span></span> <span data-ttu-id="9d549-254">Cliquez sur le bouton d’exécution vert ou appuyez sur `Ctrl + F5`, et le navigateur s’ouvre à la page d’accueil de l’application.</span><span class="sxs-lookup"><span data-stu-id="9d549-254">Click the green run button or press `Ctrl + F5`, and the browser opens to the application's landing page.</span></span> <span data-ttu-id="9d549-255">L’application s’exécute sur l’hôte local en fonction de la [mode de configuration de runtime](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="9d549-255">The application runs on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span>

<a name="app-testing"></a>

## <a name="testing-the-app"></a><span data-ttu-id="9d549-256">Test de l’application</span><span class="sxs-lookup"><span data-stu-id="9d549-256">Testing the app</span></span>

<span data-ttu-id="9d549-257">Les modèles SpaServices sont préconfigurées pour exécuter des tests de côté client à l’aide de [Karma](https://karma-runner.github.io/1.0/index.html) et [Jasmine](https://jasmine.github.io/).</span><span class="sxs-lookup"><span data-stu-id="9d549-257">SpaServices templates are pre-configured to run client-side tests using [Karma](https://karma-runner.github.io/1.0/index.html) and [Jasmine](https://jasmine.github.io/).</span></span> <span data-ttu-id="9d549-258">Jasmine est une unité de populaires framework de tests pour JavaScript, tandis que Karma est un test runner pour ces tests.</span><span class="sxs-lookup"><span data-stu-id="9d549-258">Jasmine is a popular unit testing framework for JavaScript, whereas Karma is a test runner for those tests.</span></span> <span data-ttu-id="9d549-259">KARMA est configuré pour fonctionner avec le [intergiciel (middleware) de Webpack Dev](#webpack-dev-middleware) telles que le développeur n’est pas nécessaire d’arrêter et exécuter le test chaque fois que des modifications.</span><span class="sxs-lookup"><span data-stu-id="9d549-259">Karma is configured to work with the [Webpack Dev Middleware](#webpack-dev-middleware) such that the developer isn't required to stop and run the test every time changes are made.</span></span> <span data-ttu-id="9d549-260">Que ce soit le code s’exécutant sur le cas de test ou le cas de test lui-même, le test s’exécute automatiquement.</span><span class="sxs-lookup"><span data-stu-id="9d549-260">Whether it's the code running against the test case or the test case itself, the test runs automatically.</span></span>

<span data-ttu-id="9d549-261">À l’aide de l’application Angular par exemple, deux cas de test Jasmine sont déjà fournies pour le `CounterComponent` dans le *counter.component.spec.ts* fichier :</span><span class="sxs-lookup"><span data-stu-id="9d549-261">Using the Angular application as an example, two Jasmine test cases are already provided for the `CounterComponent` in the *counter.component.spec.ts* file:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

<span data-ttu-id="9d549-262">Ouvrez l’invite de commandes dans le *ClientApp* directory.</span><span class="sxs-lookup"><span data-stu-id="9d549-262">Open the command prompt in the *ClientApp* directory.</span></span> <span data-ttu-id="9d549-263">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="9d549-263">Run the following command:</span></span>

```console
npm test
```

<span data-ttu-id="9d549-264">Le script lance le testeur Karma, qui lit les paramètres définis dans le *karma.conf.js* fichier.</span><span class="sxs-lookup"><span data-stu-id="9d549-264">The script launches the Karma test runner, which reads the settings defined in the *karma.conf.js* file.</span></span> <span data-ttu-id="9d549-265">Parmi d’autres paramètres, le *karma.conf.js* identifie les fichiers de test doit être exécuté par le biais de son `files` tableau :</span><span class="sxs-lookup"><span data-stu-id="9d549-265">Among other settings, the *karma.conf.js* identifies the test files to be executed via its `files` array:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a><span data-ttu-id="9d549-266">Publication de l’application</span><span class="sxs-lookup"><span data-stu-id="9d549-266">Publishing the application</span></span>

<span data-ttu-id="9d549-267">Combinant les ressources côté client générés et les artefacts de ASP.NET Core publiées dans un package prêt à déployer peut s’avérer fastidieuse.</span><span class="sxs-lookup"><span data-stu-id="9d549-267">Combining the generated client-side assets and the published ASP.NET Core artifacts into a ready-to-deploy package can be cumbersome.</span></span> <span data-ttu-id="9d549-268">Heureusement, SpaServices orchestre ce processus d’ensemble de la publication avec une cible MSBuild personnalisée nommée `RunWebpack`:</span><span class="sxs-lookup"><span data-stu-id="9d549-268">Thankfully, SpaServices orchestrates that entire publication process with a custom MSBuild target named `RunWebpack`:</span></span>

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

<span data-ttu-id="9d549-269">La cible MSBuild responsabilités est les suivantes :</span><span class="sxs-lookup"><span data-stu-id="9d549-269">The MSBuild target has the following responsibilities:</span></span>

1. <span data-ttu-id="9d549-270">Restaurer les packages npm</span><span class="sxs-lookup"><span data-stu-id="9d549-270">Restore the npm packages</span></span>
1. <span data-ttu-id="9d549-271">Créer une build de production à l’échelle des ressources par des tiers, côté client</span><span class="sxs-lookup"><span data-stu-id="9d549-271">Create a production-grade build of the third-party, client-side assets</span></span>
1. <span data-ttu-id="9d549-272">Créer une build de production à l’échelle des ressources côté client personnalisés</span><span class="sxs-lookup"><span data-stu-id="9d549-272">Create a production-grade build of the custom client-side assets</span></span>
1. <span data-ttu-id="9d549-273">Copier les actifs Webpack généré dans le dossier de publication</span><span class="sxs-lookup"><span data-stu-id="9d549-273">Copy the Webpack-generated assets to the publish folder</span></span>

<span data-ttu-id="9d549-274">La cible MSBuild est appelée lors de l’exécution :</span><span class="sxs-lookup"><span data-stu-id="9d549-274">The MSBuild target is invoked when running:</span></span>

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a><span data-ttu-id="9d549-275">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="9d549-275">Additional resources</span></span>

* [<span data-ttu-id="9d549-276">Docs angulaires</span><span class="sxs-lookup"><span data-stu-id="9d549-276">Angular Docs</span></span>](https://angular.io/docs)
