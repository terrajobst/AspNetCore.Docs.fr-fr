---
title: Utiliser les Services de JavaScript pour créer des Applications à Page unique dans ASP.NET Core
author: scottaddie
description: En savoir plus sur les avantages de l’utilisation des Services de JavaScript pour créer une Application à Page unique (SPA) soutenu par ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 05/28/2019
uid: client-side/spa-services
ms.openlocfilehash: c7cd35865c5bddf0e5efaa9e616832b6755d9227
ms.sourcegitcommit: e7e04a45195d4e0527af6f7cf1807defb56dc3c3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66750120"
---
# <a name="use-javascript-services-to-create-single-page-applications-in-aspnet-core"></a><span data-ttu-id="2a85c-103">Utiliser les Services de JavaScript pour créer des Applications à Page unique dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2a85c-103">Use JavaScript Services to Create Single Page Applications in ASP.NET Core</span></span>

<span data-ttu-id="2a85c-104">Par [Scott Addie](https://github.com/scottaddie) et [Fiyaz Hasan](http://fiyazhasan.me/)</span><span class="sxs-lookup"><span data-stu-id="2a85c-104">By [Scott Addie](https://github.com/scottaddie) and [Fiyaz Hasan](http://fiyazhasan.me/)</span></span>

<span data-ttu-id="2a85c-105">Une Application à Page unique (SPA) est un type d’application web en raison de son expérience utilisateur riche inhérente.</span><span class="sxs-lookup"><span data-stu-id="2a85c-105">A Single Page Application (SPA) is a popular type of web application due to its inherent rich user experience.</span></span> <span data-ttu-id="2a85c-106">L’intégration de frameworks SPA côté client ou les bibliothèques, telles que [Angular](https://angular.io/) ou [réagir](https://facebook.github.io/react/), avec des infrastructures de côté serveur, telles que ASP.NET Core peut être difficile.</span><span class="sxs-lookup"><span data-stu-id="2a85c-106">Integrating client-side SPA frameworks or libraries, such as [Angular](https://angular.io/) or [React](https://facebook.github.io/react/), with server-side frameworks such as ASP.NET Core can be difficult.</span></span> <span data-ttu-id="2a85c-107">Services de JavaScript a été développé pour réduire la friction lors du processus d’intégration.</span><span class="sxs-lookup"><span data-stu-id="2a85c-107">JavaScript Services was developed to reduce friction in the integration process.</span></span> <span data-ttu-id="2a85c-108">Il permet à une opération transparente entre les piles de technologie de serveur et de client.</span><span class="sxs-lookup"><span data-stu-id="2a85c-108">It enables seamless operation between the different client and server technology stacks.</span></span>

## <a name="what-is-javascript-services"></a><span data-ttu-id="2a85c-109">Nouveautés des Services de JavaScript</span><span class="sxs-lookup"><span data-stu-id="2a85c-109">What is JavaScript Services</span></span>

<span data-ttu-id="2a85c-110">Services de JavaScript est un ensemble de technologies côté client pour ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2a85c-110">JavaScript Services is a collection of client-side technologies for ASP.NET Core.</span></span> <span data-ttu-id="2a85c-111">Son objectif est de positionner ASP.NET Core en tant que côté serveur conseillée développeurs pour la création d’applications à page unique.</span><span class="sxs-lookup"><span data-stu-id="2a85c-111">Its goal is to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span>

<span data-ttu-id="2a85c-112">Services de JavaScript se compose de deux packages NuGet distinctes :</span><span class="sxs-lookup"><span data-stu-id="2a85c-112">JavaScript Services consists of two distinct NuGet packages:</span></span>

* <span data-ttu-id="2a85c-113">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span><span class="sxs-lookup"><span data-stu-id="2a85c-113">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span></span>
* <span data-ttu-id="2a85c-114">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span><span class="sxs-lookup"><span data-stu-id="2a85c-114">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span></span>

<span data-ttu-id="2a85c-115">Ces packages sont utiles dans les scénarios suivants :</span><span class="sxs-lookup"><span data-stu-id="2a85c-115">These packages are useful in the following scenarios:</span></span>

* <span data-ttu-id="2a85c-116">Exécuter JavaScript sur le serveur</span><span class="sxs-lookup"><span data-stu-id="2a85c-116">Run JavaScript on the server</span></span>
* <span data-ttu-id="2a85c-117">Utiliser un framework SPA ou une bibliothèque</span><span class="sxs-lookup"><span data-stu-id="2a85c-117">Use a SPA framework or library</span></span>
* <span data-ttu-id="2a85c-118">Générez les ressources côté client avec Webpack</span><span class="sxs-lookup"><span data-stu-id="2a85c-118">Build client-side assets with Webpack</span></span>

<span data-ttu-id="2a85c-119">Une grande partie de cet article met l’accent est placée sur l’utilisation du package SpaServices.</span><span class="sxs-lookup"><span data-stu-id="2a85c-119">Much of the focus in this article is placed on using the SpaServices package.</span></span>

## <a name="what-is-spaservices"></a><span data-ttu-id="2a85c-120">Qu’est SpaServices</span><span class="sxs-lookup"><span data-stu-id="2a85c-120">What is SpaServices</span></span>

<span data-ttu-id="2a85c-121">SpaServices a été créé pour positionner ASP.NET Core en tant que côté serveur conseillée développeurs pour la création d’applications à page unique.</span><span class="sxs-lookup"><span data-stu-id="2a85c-121">SpaServices was created to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span> <span data-ttu-id="2a85c-122">SpaServices n’est pas requis pour développer des applications à page unique avec ASP.NET Core, et il ne verrouille pas les développeurs dans une infrastructure client particulier.</span><span class="sxs-lookup"><span data-stu-id="2a85c-122">SpaServices isn't required to develop SPAs with ASP.NET Core, and it doesn't lock developers into a particular client framework.</span></span>

<span data-ttu-id="2a85c-123">SpaServices fournit infrastructure utile telles que :</span><span class="sxs-lookup"><span data-stu-id="2a85c-123">SpaServices provides useful infrastructure such as:</span></span>

* [<span data-ttu-id="2a85c-124">Côté serveur pré-rendu</span><span class="sxs-lookup"><span data-stu-id="2a85c-124">Server-side prerendering</span></span>](#server-side-prerendering)
* [<span data-ttu-id="2a85c-125">Intergiciel (middleware) de Webpack Dev</span><span class="sxs-lookup"><span data-stu-id="2a85c-125">Webpack Dev Middleware</span></span>](#webpack-dev-middleware)
* [<span data-ttu-id="2a85c-126">Remplacement d’un Module à chaud</span><span class="sxs-lookup"><span data-stu-id="2a85c-126">Hot Module Replacement</span></span>](#hot-module-replacement)
* [<span data-ttu-id="2a85c-127">Programmes d’assistance de routage</span><span class="sxs-lookup"><span data-stu-id="2a85c-127">Routing helpers</span></span>](#routing-helpers)

<span data-ttu-id="2a85c-128">Collectivement, ces composants d’infrastructure améliorent le flux de travail de développement et de l’expérience de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="2a85c-128">Collectively, these infrastructure components enhance both the development workflow and the runtime experience.</span></span> <span data-ttu-id="2a85c-129">Les composants peuvent être adoptés individuellement.</span><span class="sxs-lookup"><span data-stu-id="2a85c-129">The components can be adopted individually.</span></span>

## <a name="prerequisites-for-using-spaservices"></a><span data-ttu-id="2a85c-130">Conditions préalables pour l’utilisation de SpaServices</span><span class="sxs-lookup"><span data-stu-id="2a85c-130">Prerequisites for using SpaServices</span></span>

<span data-ttu-id="2a85c-131">Pour utiliser SpaServices, installez les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="2a85c-131">To work with SpaServices, install the following:</span></span>

* <span data-ttu-id="2a85c-132">[Node.js](https://nodejs.org/) (version 6 ou version ultérieure) avec npm</span><span class="sxs-lookup"><span data-stu-id="2a85c-132">[Node.js](https://nodejs.org/) (version 6 or later) with npm</span></span>

  * <span data-ttu-id="2a85c-133">Pour vérifier ces composants sont installés et sont accessibles, exécutez la commande suivante à partir de la ligne de commande :</span><span class="sxs-lookup"><span data-stu-id="2a85c-133">To verify these components are installed and can be found, run the following from the command line:</span></span>

    ```console
    node -v && npm -v
    ```

  * <span data-ttu-id="2a85c-134">Si vous déployez vers un site web Azure, aucune action n’est requise&mdash;Node.js est installé et disponible dans les environnements de serveur.</span><span class="sxs-lookup"><span data-stu-id="2a85c-134">If deploying to an Azure web site, no action is required&mdash;Node.js is installed and available in the server environments.</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * <span data-ttu-id="2a85c-135">Sur Windows à l’aide de Visual Studio 2017, le SDK est installé en sélectionnant le **.NET Core le développement multiplateforme** charge de travail.</span><span class="sxs-lookup"><span data-stu-id="2a85c-135">On Windows using Visual Studio 2017, the SDK is installed by selecting the **.NET Core cross-platform development** workload.</span></span>

* <span data-ttu-id="2a85c-136">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) package NuGet</span><span class="sxs-lookup"><span data-stu-id="2a85c-136">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet package</span></span>

## <a name="server-side-prerendering"></a><span data-ttu-id="2a85c-137">Côté serveur pré-rendu</span><span class="sxs-lookup"><span data-stu-id="2a85c-137">Server-side prerendering</span></span>

<span data-ttu-id="2a85c-138">Une application universelle (également appelé isomorphes) est une application JavaScript capable d’exécuter à la fois sur le serveur et le client.</span><span class="sxs-lookup"><span data-stu-id="2a85c-138">A universal (also known as isomorphic) application is a JavaScript application capable of running both on the server and the client.</span></span> <span data-ttu-id="2a85c-139">Angular, React et autres infrastructures populaires fournissent une plateforme universelle pour ce style de développement d’application.</span><span class="sxs-lookup"><span data-stu-id="2a85c-139">Angular, React, and other popular frameworks provide a universal platform for this application development style.</span></span> <span data-ttu-id="2a85c-140">L’idée est de rendre tout d’abord les composants d’infrastructure sur le serveur via Node.js, puis de déléguer davantage d’exécution au client.</span><span class="sxs-lookup"><span data-stu-id="2a85c-140">The idea is to first render the framework components on the server via Node.js, and then delegate further execution to the client.</span></span>

<span data-ttu-id="2a85c-141">ASP.NET Core [Tag Helpers](xref:mvc/views/tag-helpers/intro) fournie par SpaServices simplifier l’implémentation de pré-rendu de côté serveur en appelant les fonctions JavaScript sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="2a85c-141">ASP.NET Core [Tag Helpers](xref:mvc/views/tag-helpers/intro) provided by SpaServices simplify the implementation of server-side prerendering by invoking the JavaScript functions on the server.</span></span>

### <a name="server-side-prerendering-prerequisites"></a><span data-ttu-id="2a85c-142">Conditions préalables de pré-rendu côté serveur</span><span class="sxs-lookup"><span data-stu-id="2a85c-142">Server-side prerendering prerequisites</span></span>

<span data-ttu-id="2a85c-143">Installer le [aspnet-pré-rendu](https://www.npmjs.com/package/aspnet-prerendering) package npm :</span><span class="sxs-lookup"><span data-stu-id="2a85c-143">Install the [aspnet-prerendering](https://www.npmjs.com/package/aspnet-prerendering) npm package:</span></span>

```console
npm i -S aspnet-prerendering
```

### <a name="server-side-prerendering-configuration"></a><span data-ttu-id="2a85c-144">Configuration de pré-rendu côté serveur</span><span class="sxs-lookup"><span data-stu-id="2a85c-144">Server-side prerendering configuration</span></span>

<span data-ttu-id="2a85c-145">Les Tag Helpers rendus détectables par le biais de l’inscription d’espace de noms dans le projet *_ViewImports.cshtml* fichier :</span><span class="sxs-lookup"><span data-stu-id="2a85c-145">The Tag Helpers are made discoverable via namespace registration in the project's *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

<span data-ttu-id="2a85c-146">Ces Tag Helpers clarifient les subtilités de communiquer directement avec les API de bas niveau en tirant parti d’une syntaxe de type HTML à l’intérieur de la vue Razor :</span><span class="sxs-lookup"><span data-stu-id="2a85c-146">These Tag Helpers abstract away the intricacies of communicating directly with low-level APIs by leveraging an HTML-like syntax inside the Razor view:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="asp-prerender-module-tag-helper"></a><span data-ttu-id="2a85c-147">module ASP-prerender Tag Helper</span><span class="sxs-lookup"><span data-stu-id="2a85c-147">asp-prerender-module Tag Helper</span></span>

<span data-ttu-id="2a85c-148">Le `asp-prerender-module` Tag Helper, utilisée dans l’exemple de code précédent, exécute *ClientApp/dist/main-server.js* sur le serveur via Node.js.</span><span class="sxs-lookup"><span data-stu-id="2a85c-148">The `asp-prerender-module` Tag Helper, used in the preceding code example, executes *ClientApp/dist/main-server.js* on the server via Node.js.</span></span> <span data-ttu-id="2a85c-149">Par souci de clarté, *main-server.js* fichier est un artefact de la tâche de transpilation de TypeScript et JavaScript dans le [Webpack](http://webpack.github.io/) du processus de génération.</span><span class="sxs-lookup"><span data-stu-id="2a85c-149">For clarity's sake, *main-server.js* file is an artifact of the TypeScript-to-JavaScript transpilation task in the [Webpack](http://webpack.github.io/) build process.</span></span> <span data-ttu-id="2a85c-150">Webpack définit un alias de point d’entrée de `main-server`; et commence le parcours du graphique de dépendance pour cet alias le *ClientApp/démarrage-server.ts* fichier :</span><span class="sxs-lookup"><span data-stu-id="2a85c-150">Webpack defines an entry point alias of `main-server`; and, traversal of the dependency graph for this alias begins at the *ClientApp/boot-server.ts* file:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

<span data-ttu-id="2a85c-151">Dans l’exemple suivant Angular, le *ClientApp/démarrage-server.ts* fichier utilise le `createServerRenderer` (fonction) et `RenderResult` le type de la `aspnet-prerendering` package npm pour configurer le rendu du serveur via Node.js.</span><span class="sxs-lookup"><span data-stu-id="2a85c-151">In the following Angular example, the *ClientApp/boot-server.ts* file utilizes the `createServerRenderer` function and `RenderResult` type of the `aspnet-prerendering` npm package to configure server rendering via Node.js.</span></span> <span data-ttu-id="2a85c-152">Le balisage HTML destiné à rendu côté serveur est passé à un appel de fonction de résolution, lequel est encapsulé dans un script JavaScript fortement typée `Promise` objet.</span><span class="sxs-lookup"><span data-stu-id="2a85c-152">The HTML markup destined for server-side rendering is passed to a resolve function call, which is wrapped in a strongly-typed JavaScript `Promise` object.</span></span> <span data-ttu-id="2a85c-153">Le `Promise` importance de l’objet est qu’il fournit en mode asynchrone le balisage HTML pour la page pour l’injection dans l’élément d’espace réservé du DOM.</span><span class="sxs-lookup"><span data-stu-id="2a85c-153">The `Promise` object's significance is that it asynchronously supplies the HTML markup to the page for injection in the DOM's placeholder element.</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="asp-prerender-data-tag-helper"></a><span data-ttu-id="2a85c-154">ASP-prerender-data Tag Helper</span><span class="sxs-lookup"><span data-stu-id="2a85c-154">asp-prerender-data Tag Helper</span></span>

<span data-ttu-id="2a85c-155">Associé à la `asp-prerender-module` Tag Helper, le `asp-prerender-data` Tag Helper peut être utilisé pour transmettre des informations contextuelles à partir de la vue Razor pour le code JavaScript côté serveur.</span><span class="sxs-lookup"><span data-stu-id="2a85c-155">When coupled with the `asp-prerender-module` Tag Helper, the `asp-prerender-data` Tag Helper can be used to pass contextual information from the Razor view to the server-side JavaScript.</span></span> <span data-ttu-id="2a85c-156">Par exemple, le balisage suivant transmet les données utilisateur à la `main-server` module :</span><span class="sxs-lookup"><span data-stu-id="2a85c-156">For example, the following markup passes user data to the `main-server` module:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

<span data-ttu-id="2a85c-157">La réponse reçue `UserName` argument est sérialisé à l’aide du sérialiseur JSON intégré et est stocké dans le `params.data` objet.</span><span class="sxs-lookup"><span data-stu-id="2a85c-157">The received `UserName` argument is serialized using the built-in JSON serializer and is stored in the `params.data` object.</span></span> <span data-ttu-id="2a85c-158">Dans l’exemple suivant Angular, les données sont utilisées pour construire un message d’accueil personnalisé dans un `h1` élément :</span><span class="sxs-lookup"><span data-stu-id="2a85c-158">In the following Angular example, the data is used to construct a personalized greeting within an `h1` element:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

<span data-ttu-id="2a85c-159">Les noms de propriété passés dans les Tag Helpers sont représentés par **la casse Pascal** notation.</span><span class="sxs-lookup"><span data-stu-id="2a85c-159">Property names passed in Tag Helpers are represented with **PascalCase** notation.</span></span> <span data-ttu-id="2a85c-160">Comparez cela à JavaScript, où les mêmes noms de propriété sont représentés par **une casse mixte**.</span><span class="sxs-lookup"><span data-stu-id="2a85c-160">Contrast that to JavaScript, where the same property names are represented with **camelCase**.</span></span> <span data-ttu-id="2a85c-161">La configuration de sérialisation JSON par défaut est chargée de cette différence.</span><span class="sxs-lookup"><span data-stu-id="2a85c-161">The default JSON serialization configuration is responsible for this difference.</span></span>

<span data-ttu-id="2a85c-162">Pour étendre l’exemple de code précédent, données peuvent être transmises à partir du serveur à la vue par HYDRATATION le `globals` propriété fournie à la `resolve` (fonction) :</span><span class="sxs-lookup"><span data-stu-id="2a85c-162">To expand upon the preceding code example, data can be passed from the server to the view by hydrating the `globals` property provided to the `resolve` function:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

<span data-ttu-id="2a85c-163">Le `postList` tableau défini à l’intérieur de la `globals` objet est attaché à du navigateur global `window` objet.</span><span class="sxs-lookup"><span data-stu-id="2a85c-163">The `postList` array defined inside the `globals` object is attached to the browser's global `window` object.</span></span> <span data-ttu-id="2a85c-164">Cette variable levage à portée globale permet d’éliminer une duplication des efforts, en particulier en ce qui concerne le chargement des données mêmes qu’une seule fois sur le serveur et à nouveau sur le client.</span><span class="sxs-lookup"><span data-stu-id="2a85c-164">This variable hoisting to global scope eliminates duplication of effort, particularly as it pertains to loading the same data once on the server and again on the client.</span></span>

![(variable globale) postList attaché à l’objet de fenêtre](spa-services/_static/global_variable.png)

## <a name="webpack-dev-middleware"></a><span data-ttu-id="2a85c-166">Intergiciel (middleware) de Webpack Dev</span><span class="sxs-lookup"><span data-stu-id="2a85c-166">Webpack Dev Middleware</span></span>

<span data-ttu-id="2a85c-167">[Intergiciel (middleware) de Webpack Dev](https://webpack.js.org/guides/development/#using-webpack-dev-middleware) introduit un flux de travail de développement simplifié par laquelle Webpack génère des ressources à la demande.</span><span class="sxs-lookup"><span data-stu-id="2a85c-167">[Webpack Dev Middleware](https://webpack.js.org/guides/development/#using-webpack-dev-middleware) introduces a streamlined development workflow whereby Webpack builds resources on demand.</span></span> <span data-ttu-id="2a85c-168">L’intergiciel (middleware) compile automatiquement et fournit des ressources de côté client lorsqu’une page est rechargée dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="2a85c-168">The middleware automatically compiles and serves client-side resources when a page is reloaded in the browser.</span></span> <span data-ttu-id="2a85c-169">L’autre approche consiste à appeler manuellement la Webpack via un script de génération du projet npm quand une dépendance tierce ou le code personnalisé change.</span><span class="sxs-lookup"><span data-stu-id="2a85c-169">The alternate approach is to manually invoke Webpack via the project's npm build script when a third-party dependency or the custom code changes.</span></span> <span data-ttu-id="2a85c-170">Script de compilation d’un npm la *package.json* fichier est illustré dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="2a85c-170">An npm build script in the *package.json* file is shown in the following example:</span></span>

```json
"build": "npm run build:vendor && npm run build:custom",
```

### <a name="webpack-dev-middleware-prerequisites"></a><span data-ttu-id="2a85c-171">Conditions préalables d’intergiciel (middleware) de Webpack Dev</span><span class="sxs-lookup"><span data-stu-id="2a85c-171">Webpack Dev Middleware prerequisites</span></span>

<span data-ttu-id="2a85c-172">Installer le [aspnet-webpack](https://www.npmjs.com/package/aspnet-webpack) package npm :</span><span class="sxs-lookup"><span data-stu-id="2a85c-172">Install the [aspnet-webpack](https://www.npmjs.com/package/aspnet-webpack) npm package:</span></span>

```console
npm i -D aspnet-webpack
```

### <a name="webpack-dev-middleware-configuration"></a><span data-ttu-id="2a85c-173">Configuration de l’intergiciel (middleware) de Webpack Dev</span><span class="sxs-lookup"><span data-stu-id="2a85c-173">Webpack Dev Middleware configuration</span></span>

<span data-ttu-id="2a85c-174">Intergiciel (middleware) de Webpack Dev est inscrit dans le pipeline de requêtes HTTP via le code suivant dans le *Startup.cs* du fichier `Configure` méthode :</span><span class="sxs-lookup"><span data-stu-id="2a85c-174">Webpack Dev Middleware is registered into the HTTP request pipeline via the following code in the *Startup.cs* file's `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_WebpackMiddlewareRegistration&highlight=4)]

<span data-ttu-id="2a85c-175">Le `UseWebpackDevMiddleware` méthode d’extension doit être appelée avant [l’inscription de l’hébergement de fichier statique](xref:fundamentals/static-files) via la `UseStaticFiles` méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="2a85c-175">The `UseWebpackDevMiddleware` extension method must be called before [registering static file hosting](xref:fundamentals/static-files) via the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="2a85c-176">Pour des raisons de sécurité, inscrivez le middleware uniquement lorsque l’application s’exécute en mode de développement.</span><span class="sxs-lookup"><span data-stu-id="2a85c-176">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="2a85c-177">Le *webpack.config.js* du fichier `output.publicPath` propriété indique à l’intergiciel (middleware) pour surveiller le `dist` dossier pour les modifications :</span><span class="sxs-lookup"><span data-stu-id="2a85c-177">The *webpack.config.js* file's `output.publicPath` property tells the middleware to watch the `dist` folder for changes:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

## <a name="hot-module-replacement"></a><span data-ttu-id="2a85c-178">Remplacement d’un Module à chaud</span><span class="sxs-lookup"><span data-stu-id="2a85c-178">Hot Module Replacement</span></span>

<span data-ttu-id="2a85c-179">Pensez à Webpack [remplacement à chaud de Module](https://webpack.js.org/concepts/hot-module-replacement/) fonctionnalité (HMR) comme une évolution de [intergiciel (middleware) de Webpack Dev](#webpack-dev-middleware).</span><span class="sxs-lookup"><span data-stu-id="2a85c-179">Think of Webpack's [Hot Module Replacement](https://webpack.js.org/concepts/hot-module-replacement/) (HMR) feature as an evolution of [Webpack Dev Middleware](#webpack-dev-middleware).</span></span> <span data-ttu-id="2a85c-180">HMR présente les mêmes avantages, mais elle simplifie davantage le flux de travail de développement en mettant à jour automatiquement de contenu de la page après la compilation les modifications.</span><span class="sxs-lookup"><span data-stu-id="2a85c-180">HMR introduces all the same benefits, but it further streamlines the development workflow by automatically updating page content after compiling the changes.</span></span> <span data-ttu-id="2a85c-181">À ne pas confondre avec une actualisation du navigateur, ce qui entraînerait une interférence avec l’état en mémoire actuel et la session de débogage de l’application SPA.</span><span class="sxs-lookup"><span data-stu-id="2a85c-181">Don't confuse this with a refresh of the browser, which would interfere with the current in-memory state and debugging session of the SPA.</span></span> <span data-ttu-id="2a85c-182">Il existe un lien direct entre le service de l’intergiciel (middleware) de Webpack développement et le navigateur, ce qui signifie que les modifications sont envoyées au navigateur.</span><span class="sxs-lookup"><span data-stu-id="2a85c-182">There's a live link between the Webpack Dev Middleware service and the browser, which means changes are pushed to the browser.</span></span>

### <a name="hot-module-replacement-prerequisites"></a><span data-ttu-id="2a85c-183">Conditions préalables de remplacement d’un Module à chaud</span><span class="sxs-lookup"><span data-stu-id="2a85c-183">Hot Module Replacement prerequisites</span></span>

<span data-ttu-id="2a85c-184">Installer le [webpack chaud middleware](https://www.npmjs.com/package/webpack-hot-middleware) package npm :</span><span class="sxs-lookup"><span data-stu-id="2a85c-184">Install the [webpack-hot-middleware](https://www.npmjs.com/package/webpack-hot-middleware) npm package:</span></span>

```console
npm i -D webpack-hot-middleware
```

### <a name="hot-module-replacement-configuration"></a><span data-ttu-id="2a85c-185">Configuration de remplacement d’un Module à chaud</span><span class="sxs-lookup"><span data-stu-id="2a85c-185">Hot Module Replacement configuration</span></span>

<span data-ttu-id="2a85c-186">Le composant HMR doit être enregistré dans le pipeline de demande HTTP de MVC dans le `Configure` méthode :</span><span class="sxs-lookup"><span data-stu-id="2a85c-186">The HMR component must be registered into MVC's HTTP request pipeline in the `Configure` method:</span></span>

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

<span data-ttu-id="2a85c-187">En tant qu’était le cas avec [intergiciel (middleware) de Webpack Dev](#webpack-dev-middleware), le `UseWebpackDevMiddleware` méthode d’extension doit être appelée avant la `UseStaticFiles` méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="2a85c-187">As was true with [Webpack Dev Middleware](#webpack-dev-middleware), the `UseWebpackDevMiddleware` extension method must be called before the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="2a85c-188">Pour des raisons de sécurité, inscrivez le middleware uniquement lorsque l’application s’exécute en mode de développement.</span><span class="sxs-lookup"><span data-stu-id="2a85c-188">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="2a85c-189">Le *webpack.config.js* fichier doit définir un `plugins` de tableau, même si celui-ci est laissé vide :</span><span class="sxs-lookup"><span data-stu-id="2a85c-189">The *webpack.config.js* file must define a `plugins` array, even if it's left empty:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

<span data-ttu-id="2a85c-190">Après le chargement de l’application dans le navigateur, onglet de la Console outils de développement fournit la confirmation de l’activation de HMR :</span><span class="sxs-lookup"><span data-stu-id="2a85c-190">After loading the app in the browser, the developer tools' Console tab provides confirmation of HMR activation:</span></span>

![Message de connexion de remplacement d’un Module à chaud](spa-services/_static/hmr_connected.png)

## <a name="routing-helpers"></a><span data-ttu-id="2a85c-192">Programmes d’assistance de routage</span><span class="sxs-lookup"><span data-stu-id="2a85c-192">Routing helpers</span></span>

<span data-ttu-id="2a85c-193">Dans la plupart des SPA basé sur ASP.NET Core, routage côté client est souvent souhaité en plus du routage côté serveur.</span><span class="sxs-lookup"><span data-stu-id="2a85c-193">In most ASP.NET Core-based SPAs, client-side routing is often desired in addition to server-side routing.</span></span> <span data-ttu-id="2a85c-194">Les systèmes de routage SPA et MVC peuvent travailler indépendamment sans interférence.</span><span class="sxs-lookup"><span data-stu-id="2a85c-194">The SPA and MVC routing systems can work independently without interference.</span></span> <span data-ttu-id="2a85c-195">Il existe, toutefois, un bord cas posant défis : identification des réponses HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="2a85c-195">There's, however, one edge case posing challenges: identifying 404 HTTP responses.</span></span>

<span data-ttu-id="2a85c-196">Considérez le scénario dans lequel un itinéraire sans extension de `/some/page` est utilisé.</span><span class="sxs-lookup"><span data-stu-id="2a85c-196">Consider the scenario in which an extensionless route of `/some/page` is used.</span></span> <span data-ttu-id="2a85c-197">Supposons que la demande n’à la correspondance un itinéraire côté serveur, mais son modèle ne correspond pas à un itinéraire côté client.</span><span class="sxs-lookup"><span data-stu-id="2a85c-197">Assume the request doesn't pattern-match a server-side route, but its pattern does match a client-side route.</span></span> <span data-ttu-id="2a85c-198">Examinons à présent une demande entrante pour `/images/user-512.png`, lequel attend généralement rechercher un fichier image sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="2a85c-198">Now consider an incoming request for `/images/user-512.png`, which generally expects to find an image file on the server.</span></span> <span data-ttu-id="2a85c-199">Si ce chemin d’accès de la ressource demandée ne correspond pas à toute approche côté serveur ou un fichier statique, il est peu probable que l’application côté client il traiterait&mdash;généralement retournant un code d’état HTTP 404 est souhaitée.</span><span class="sxs-lookup"><span data-stu-id="2a85c-199">If that requested resource path doesn't match any server-side route or static file, it's unlikely that the client-side application would handle it&mdash;generally returning a 404 HTTP status code is desired.</span></span>

### <a name="routing-helpers-prerequisites"></a><span data-ttu-id="2a85c-200">Conditions préalables de programmes d’assistance routage</span><span class="sxs-lookup"><span data-stu-id="2a85c-200">Routing helpers prerequisites</span></span>

<span data-ttu-id="2a85c-201">Installez le package npm de routage côté client.</span><span class="sxs-lookup"><span data-stu-id="2a85c-201">Install the client-side routing npm package.</span></span> <span data-ttu-id="2a85c-202">À l’aide d’Angular par exemple :</span><span class="sxs-lookup"><span data-stu-id="2a85c-202">Using Angular as an example:</span></span>

```console
npm i -S @angular/router
```

### <a name="routing-helpers-configuration"></a><span data-ttu-id="2a85c-203">Configuration de routage de programmes d’assistance</span><span class="sxs-lookup"><span data-stu-id="2a85c-203">Routing helpers configuration</span></span>

<span data-ttu-id="2a85c-204">Une méthode d’extension nommée `MapSpaFallbackRoute` est utilisé dans le `Configure` méthode :</span><span class="sxs-lookup"><span data-stu-id="2a85c-204">An extension method named `MapSpaFallbackRoute` is used in the `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_MvcRoutingTable&highlight=7-9)]

<span data-ttu-id="2a85c-205">Itinéraires sont évaluées dans l’ordre dans lequel ils sont configurés.</span><span class="sxs-lookup"><span data-stu-id="2a85c-205">Routes are evaluated in the order in which they're configured.</span></span> <span data-ttu-id="2a85c-206">Par conséquent, le `default` itinéraire dans l’exemple de code précédent est utilisé pour les critères spéciaux.</span><span class="sxs-lookup"><span data-stu-id="2a85c-206">Consequently, the `default` route in the preceding code example is used first for pattern matching.</span></span>

## <a name="create-a-new-project"></a><span data-ttu-id="2a85c-207">Créer un nouveau projet</span><span class="sxs-lookup"><span data-stu-id="2a85c-207">Create a new project</span></span>

<span data-ttu-id="2a85c-208">Services de JavaScript fournissent des modèles d’application préconfigurés.</span><span class="sxs-lookup"><span data-stu-id="2a85c-208">JavaScript Services provide pre-configured application templates.</span></span> <span data-ttu-id="2a85c-209">SpaServices est utilisé dans ces modèles conjointement avec différentes infrastructures et bibliothèques, telles que Angular et React avec Redux.</span><span class="sxs-lookup"><span data-stu-id="2a85c-209">SpaServices is used in these templates in conjunction with different frameworks and libraries such as Angular, React, and Redux.</span></span>

<span data-ttu-id="2a85c-210">Ces modèles peuvent être installés par le biais de l’interface CLI .NET Core en exécutant la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="2a85c-210">These templates can be installed via the .NET Core CLI by running the following command:</span></span>

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

<span data-ttu-id="2a85c-211">Une liste des modèles disponibles s’affiche :</span><span class="sxs-lookup"><span data-stu-id="2a85c-211">A list of available SPA templates is displayed:</span></span>

| <span data-ttu-id="2a85c-212">Modèles</span><span class="sxs-lookup"><span data-stu-id="2a85c-212">Templates</span></span>                                 | <span data-ttu-id="2a85c-213">Nom court</span><span class="sxs-lookup"><span data-stu-id="2a85c-213">Short Name</span></span> | <span data-ttu-id="2a85c-214">Langue</span><span class="sxs-lookup"><span data-stu-id="2a85c-214">Language</span></span> | <span data-ttu-id="2a85c-215">Balises</span><span class="sxs-lookup"><span data-stu-id="2a85c-215">Tags</span></span>        |
| ------------------------------------------| :--------: | :------: | :---------: |
| <span data-ttu-id="2a85c-216">MVC ASP.NET Core avec Angular</span><span class="sxs-lookup"><span data-stu-id="2a85c-216">MVC ASP.NET Core with Angular</span></span>             | <span data-ttu-id="2a85c-217">angular</span><span class="sxs-lookup"><span data-stu-id="2a85c-217">angular</span></span>    | <span data-ttu-id="2a85c-218">[C#]</span><span class="sxs-lookup"><span data-stu-id="2a85c-218">[C#]</span></span>     | <span data-ttu-id="2a85c-219">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="2a85c-219">Web/MVC/SPA</span></span> |
| <span data-ttu-id="2a85c-220">MVC ASP.NET Core avec React.js</span><span class="sxs-lookup"><span data-stu-id="2a85c-220">MVC ASP.NET Core with React.js</span></span>            | <span data-ttu-id="2a85c-221">react</span><span class="sxs-lookup"><span data-stu-id="2a85c-221">react</span></span>      | <span data-ttu-id="2a85c-222">[C#]</span><span class="sxs-lookup"><span data-stu-id="2a85c-222">[C#]</span></span>     | <span data-ttu-id="2a85c-223">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="2a85c-223">Web/MVC/SPA</span></span> |
| <span data-ttu-id="2a85c-224">MVC ASP.NET Core avec React.js et Redux</span><span class="sxs-lookup"><span data-stu-id="2a85c-224">MVC ASP.NET Core with React.js and Redux</span></span>  | <span data-ttu-id="2a85c-225">reactredux</span><span class="sxs-lookup"><span data-stu-id="2a85c-225">reactredux</span></span> | <span data-ttu-id="2a85c-226">[C#]</span><span class="sxs-lookup"><span data-stu-id="2a85c-226">[C#]</span></span>     | <span data-ttu-id="2a85c-227">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="2a85c-227">Web/MVC/SPA</span></span> |

<span data-ttu-id="2a85c-228">Pour créer un nouveau projet à l’aide d’un des modèles SPA, incluez le **nom court** du modèle dans le [dotnet nouvelle](/dotnet/core/tools/dotnet-new) commande.</span><span class="sxs-lookup"><span data-stu-id="2a85c-228">To create a new project using one of the SPA templates, include the **Short Name** of the template in the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="2a85c-229">La commande suivante crée une application Angular avec ASP.NET Core MVC est configuré pour le côté serveur :</span><span class="sxs-lookup"><span data-stu-id="2a85c-229">The following command creates an Angular application with ASP.NET Core MVC configured for the server side:</span></span>

```console
dotnet new angular
```

### <a name="set-the-runtime-configuration-mode"></a><span data-ttu-id="2a85c-230">Définir le mode de configuration du runtime</span><span class="sxs-lookup"><span data-stu-id="2a85c-230">Set the runtime configuration mode</span></span>

<span data-ttu-id="2a85c-231">Il existe deux modes de configuration de runtime principal :</span><span class="sxs-lookup"><span data-stu-id="2a85c-231">Two primary runtime configuration modes exist:</span></span>

* <span data-ttu-id="2a85c-232">**Développement**:</span><span class="sxs-lookup"><span data-stu-id="2a85c-232">**Development**:</span></span>
  * <span data-ttu-id="2a85c-233">Il inclut des mappages de source pour faciliter le débogage.</span><span class="sxs-lookup"><span data-stu-id="2a85c-233">Includes source maps to ease debugging.</span></span>
  * <span data-ttu-id="2a85c-234">N’Optimisez le code côté client pour les performances.</span><span class="sxs-lookup"><span data-stu-id="2a85c-234">Doesn't optimize the client-side code for performance.</span></span>
* <span data-ttu-id="2a85c-235">**Production**:</span><span class="sxs-lookup"><span data-stu-id="2a85c-235">**Production**:</span></span>
  * <span data-ttu-id="2a85c-236">Exclut les mappages de sources.</span><span class="sxs-lookup"><span data-stu-id="2a85c-236">Excludes source maps.</span></span>
  * <span data-ttu-id="2a85c-237">Optimise le code côté client par le biais de regroupement et minimisation.</span><span class="sxs-lookup"><span data-stu-id="2a85c-237">Optimizes the client-side code via bundling and minification.</span></span>

<span data-ttu-id="2a85c-238">ASP.NET Core utilise une variable d’environnement nommée `ASPNETCORE_ENVIRONMENT` pour stocker le mode de configuration.</span><span class="sxs-lookup"><span data-stu-id="2a85c-238">ASP.NET Core uses an environment variable named `ASPNETCORE_ENVIRONMENT` to store the configuration mode.</span></span> <span data-ttu-id="2a85c-239">Pour plus d’informations, consultez [définir l’environnement](xref:fundamentals/environments#set-the-environment).</span><span class="sxs-lookup"><span data-stu-id="2a85c-239">For more information, see [Set the environment](xref:fundamentals/environments#set-the-environment).</span></span>

### <a name="run-with-net-core-cli"></a><span data-ttu-id="2a85c-240">Exécuter avec l’interface CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="2a85c-240">Run with .NET Core CLI</span></span>

<span data-ttu-id="2a85c-241">Restaurer le NuGet requis et les packages npm en exécutant la commande suivante à la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="2a85c-241">Restore the required NuGet and npm packages by running the following command at the project root:</span></span>

```console
dotnet restore && npm i
```

<span data-ttu-id="2a85c-242">Générer et exécuter l’application :</span><span class="sxs-lookup"><span data-stu-id="2a85c-242">Build and run the application:</span></span>

```console
dotnet run
```

<span data-ttu-id="2a85c-243">Démarrage de l’application sur l’hôte local en fonction de la [mode de configuration de runtime](#set-the-runtime-configuration-mode).</span><span class="sxs-lookup"><span data-stu-id="2a85c-243">The application starts on localhost according to the [runtime configuration mode](#set-the-runtime-configuration-mode).</span></span> <span data-ttu-id="2a85c-244">Navigation vers `http://localhost:5000` dans le navigateur affiche la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="2a85c-244">Navigating to `http://localhost:5000` in the browser displays the landing page.</span></span>

### <a name="run-with-visual-studio-2017"></a><span data-ttu-id="2a85c-245">Exécuter avec Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="2a85c-245">Run with Visual Studio 2017</span></span>

<span data-ttu-id="2a85c-246">Ouvrez le *.csproj* fichier généré par le [dotnet nouvelle](/dotnet/core/tools/dotnet-new) commande.</span><span class="sxs-lookup"><span data-stu-id="2a85c-246">Open the *.csproj* file generated by the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="2a85c-247">Les packages NuGet et npm nécessaires sont restaurés automatiquement sur le projet ouvert.</span><span class="sxs-lookup"><span data-stu-id="2a85c-247">The required NuGet and npm packages are restored automatically upon project open.</span></span> <span data-ttu-id="2a85c-248">Ce processus de restauration peut prendre quelques minutes, et l’application est prête à exécuter lorsqu’elle est terminée.</span><span class="sxs-lookup"><span data-stu-id="2a85c-248">This restoration process may take up to a few minutes, and the application is ready to run when it completes.</span></span> <span data-ttu-id="2a85c-249">Cliquez sur le bouton d’exécution vert ou appuyez sur `Ctrl + F5`, et le navigateur s’ouvre à la page d’accueil de l’application.</span><span class="sxs-lookup"><span data-stu-id="2a85c-249">Click the green run button or press `Ctrl + F5`, and the browser opens to the application's landing page.</span></span> <span data-ttu-id="2a85c-250">L’application s’exécute sur l’hôte local en fonction de la [mode de configuration de runtime](#set-the-runtime-configuration-mode).</span><span class="sxs-lookup"><span data-stu-id="2a85c-250">The application runs on localhost according to the [runtime configuration mode](#set-the-runtime-configuration-mode).</span></span>

## <a name="test-the-app"></a><span data-ttu-id="2a85c-251">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="2a85c-251">Test the app</span></span>

<span data-ttu-id="2a85c-252">Les modèles SpaServices sont préconfigurées pour exécuter des tests de côté client à l’aide de [Karma](https://karma-runner.github.io/1.0/index.html) et [Jasmine](https://jasmine.github.io/).</span><span class="sxs-lookup"><span data-stu-id="2a85c-252">SpaServices templates are pre-configured to run client-side tests using [Karma](https://karma-runner.github.io/1.0/index.html) and [Jasmine](https://jasmine.github.io/).</span></span> <span data-ttu-id="2a85c-253">Jasmine est une unité de populaires framework de tests pour JavaScript, tandis que Karma est un test runner pour ces tests.</span><span class="sxs-lookup"><span data-stu-id="2a85c-253">Jasmine is a popular unit testing framework for JavaScript, whereas Karma is a test runner for those tests.</span></span> <span data-ttu-id="2a85c-254">KARMA est configuré pour fonctionner avec le [intergiciel (middleware) de Webpack Dev](#webpack-dev-middleware) telles que le développeur n’est pas nécessaire d’arrêter et exécuter le test chaque fois que des modifications.</span><span class="sxs-lookup"><span data-stu-id="2a85c-254">Karma is configured to work with the [Webpack Dev Middleware](#webpack-dev-middleware) such that the developer isn't required to stop and run the test every time changes are made.</span></span> <span data-ttu-id="2a85c-255">Que ce soit le code s’exécutant sur le cas de test ou le cas de test lui-même, le test s’exécute automatiquement.</span><span class="sxs-lookup"><span data-stu-id="2a85c-255">Whether it's the code running against the test case or the test case itself, the test runs automatically.</span></span>

<span data-ttu-id="2a85c-256">À l’aide de l’application Angular par exemple, deux cas de test Jasmine sont déjà fournies pour le `CounterComponent` dans le *counter.component.spec.ts* fichier :</span><span class="sxs-lookup"><span data-stu-id="2a85c-256">Using the Angular application as an example, two Jasmine test cases are already provided for the `CounterComponent` in the *counter.component.spec.ts* file:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

<span data-ttu-id="2a85c-257">Ouvrez l’invite de commandes dans le *ClientApp* directory.</span><span class="sxs-lookup"><span data-stu-id="2a85c-257">Open the command prompt in the *ClientApp* directory.</span></span> <span data-ttu-id="2a85c-258">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="2a85c-258">Run the following command:</span></span>

```console
npm test
```

<span data-ttu-id="2a85c-259">Le script lance le testeur Karma, qui lit les paramètres définis dans le *karma.conf.js* fichier.</span><span class="sxs-lookup"><span data-stu-id="2a85c-259">The script launches the Karma test runner, which reads the settings defined in the *karma.conf.js* file.</span></span> <span data-ttu-id="2a85c-260">Parmi d’autres paramètres, le *karma.conf.js* identifie les fichiers de test doit être exécuté par le biais de son `files` tableau :</span><span class="sxs-lookup"><span data-stu-id="2a85c-260">Among other settings, the *karma.conf.js* identifies the test files to be executed via its `files` array:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

## <a name="publish-the-app"></a><span data-ttu-id="2a85c-261">Publier l'application</span><span class="sxs-lookup"><span data-stu-id="2a85c-261">Publish the app</span></span>

<span data-ttu-id="2a85c-262">Combinant les ressources côté client générés et les artefacts de ASP.NET Core publiées dans un package prêt à déployer peut s’avérer fastidieuse.</span><span class="sxs-lookup"><span data-stu-id="2a85c-262">Combining the generated client-side assets and the published ASP.NET Core artifacts into a ready-to-deploy package can be cumbersome.</span></span> <span data-ttu-id="2a85c-263">Heureusement, SpaServices orchestre ce processus d’ensemble de la publication avec une cible MSBuild personnalisée nommée `RunWebpack`:</span><span class="sxs-lookup"><span data-stu-id="2a85c-263">Thankfully, SpaServices orchestrates that entire publication process with a custom MSBuild target named `RunWebpack`:</span></span>

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

<span data-ttu-id="2a85c-264">La cible MSBuild responsabilités est les suivantes :</span><span class="sxs-lookup"><span data-stu-id="2a85c-264">The MSBuild target has the following responsibilities:</span></span>

1. <span data-ttu-id="2a85c-265">Restaurez les packages npm.</span><span class="sxs-lookup"><span data-stu-id="2a85c-265">Restore the npm packages.</span></span>
1. <span data-ttu-id="2a85c-266">Créez une build de production à l’échelle des ressources par des tiers, côté client.</span><span class="sxs-lookup"><span data-stu-id="2a85c-266">Create a production-grade build of the third-party, client-side assets.</span></span>
1. <span data-ttu-id="2a85c-267">Créez une build de production à l’échelle des ressources côté client personnalisées.</span><span class="sxs-lookup"><span data-stu-id="2a85c-267">Create a production-grade build of the custom client-side assets.</span></span>
1. <span data-ttu-id="2a85c-268">Copier les actifs de Webpack généré dans le dossier de publication.</span><span class="sxs-lookup"><span data-stu-id="2a85c-268">Copy the Webpack-generated assets to the publish folder.</span></span>

<span data-ttu-id="2a85c-269">La cible MSBuild est appelée lors de l’exécution :</span><span class="sxs-lookup"><span data-stu-id="2a85c-269">The MSBuild target is invoked when running:</span></span>

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a><span data-ttu-id="2a85c-270">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="2a85c-270">Additional resources</span></span>

* [<span data-ttu-id="2a85c-271">Docs angulaires</span><span class="sxs-lookup"><span data-stu-id="2a85c-271">Angular Docs</span></span>](https://angular.io/docs)
