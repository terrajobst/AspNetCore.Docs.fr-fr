---
title: ASP.NET Core du routage éblouissant
author: guardrex
description: Découvrez comment acheminer des requêtes dans des applications et sur le composant NavLink.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/21/2019
uid: blazor/routing
ms.openlocfilehash: d6fb3f03be94ff99ac3ed434265e6cd6b752c625
ms.sourcegitcommit: 04ce94b3c1b01d167f30eed60c1c95446dfe759d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/21/2019
ms.locfileid: "71176401"
---
# <a name="aspnet-core-blazor-routing"></a><span data-ttu-id="c7b3d-103">ASP.NET Core du routage éblouissant</span><span class="sxs-lookup"><span data-stu-id="c7b3d-103">ASP.NET Core Blazor routing</span></span>

<span data-ttu-id="c7b3d-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c7b3d-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="c7b3d-105">Découvrez comment acheminer les demandes et comment utiliser le `NavLink` composant pour créer des liens de navigation dans les applications éblouissantes.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-105">Learn how to route requests and how to use the `NavLink` component to create navigation links in Blazor apps.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="c7b3d-106">Intégration du routage du point de terminaison ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c7b3d-106">ASP.NET Core endpoint routing integration</span></span>

<span data-ttu-id="c7b3d-107">Le serveur éblouissant est intégré à [ASP.net Core routage des points de terminaison](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="c7b3d-107">Blazor Server is integrated into [ASP.NET Core Endpoint Routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="c7b3d-108">Une application ASP.net Core est configurée pour accepter les connexions entrantes pour `MapBlazorHub` les `Startup.Configure`composants interactifs avec dans :</span><span class="sxs-lookup"><span data-stu-id="c7b3d-108">An ASP.NET Core app is configured to accept incoming connections for interactive components with `MapBlazorHub` in `Startup.Configure`:</span></span>

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

<span data-ttu-id="c7b3d-109">La configuration la plus courante consiste à acheminer toutes les demandes vers une page Razor, qui joue le rôle d’hôte pour la partie côté serveur de l’application serveur éblouissante.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-109">The most typical configuration is to route all requests to a Razor page, which acts as the host for the server-side part of the Blazor Server app.</span></span> <span data-ttu-id="c7b3d-110">Par Convention, la page *hôte* est généralement nommée *_Host. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-110">By convention, the *host* page is usually named *_Host.cshtml*.</span></span> <span data-ttu-id="c7b3d-111">L’itinéraire spécifié dans le fichier hôte est appelé *itinéraire de secours* , car il fonctionne avec une priorité basse dans la correspondance d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-111">The route specified in the host file is called a *fallback route* because it operates with a low priority in route matching.</span></span> <span data-ttu-id="c7b3d-112">L’itinéraire de secours est pris en compte lorsque les autres itinéraires ne correspondent pas.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-112">The fallback route is considered when other routes don't match.</span></span> <span data-ttu-id="c7b3d-113">Cela permet à l’application d’utiliser d’autres contrôleurs et pages sans interférer avec l’application serveur éblouissante.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-113">This allows the app to use others controllers and pages without interfering with the Blazor Server app.</span></span>

## <a name="route-templates"></a><span data-ttu-id="c7b3d-114">Modèles de routage</span><span class="sxs-lookup"><span data-stu-id="c7b3d-114">Route templates</span></span>

<span data-ttu-id="c7b3d-115">Le `Router` composant active le routage vers chaque composant avec un itinéraire spécifié.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-115">The `Router` component enables routing to each component with a specified route.</span></span> <span data-ttu-id="c7b3d-116">Le `Router` composant apparaît dans le fichier *app. Razor* :</span><span class="sxs-lookup"><span data-stu-id="c7b3d-116">The `Router` component appears in the *App.razor* file:</span></span>

```cshtml
<Router AppAssembly="typeof(Startup).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <p>Sorry, there's nothing at this address.</p>
    </NotFound>
</Router>
```

<span data-ttu-id="c7b3d-117">Lorsqu’un fichier *. Razor* avec une `@page` directive est compilé, la classe générée est fournie en <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> spécifiant le modèle de routage.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-117">When a *.razor* file with an `@page` directive is compiled, the generated class is provided a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span>

<span data-ttu-id="c7b3d-118">Au moment de l' `RouteView` exécution, le composant :</span><span class="sxs-lookup"><span data-stu-id="c7b3d-118">At runtime, the `RouteView` component:</span></span>

* <span data-ttu-id="c7b3d-119">Reçoit du `RouteData` `Router` avec les paramètres souhaités.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-119">Receives the `RouteData` from the `Router` along with any desired parameters.</span></span>
* <span data-ttu-id="c7b3d-120">Restitue le composant spécifié avec sa disposition (ou une disposition par défaut facultative) à l’aide des paramètres spécifiés.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-120">Renders the specified component with its layout (or an optional default layout) using the specified parameters.</span></span>

<span data-ttu-id="c7b3d-121">Vous pouvez éventuellement spécifier un `DefaultLayout` paramètre avec une classe de disposition à utiliser pour les composants qui ne spécifient pas de disposition.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-121">You can optionally specify a `DefaultLayout` parameter with a layout class to use for components that don't specify a layout.</span></span> <span data-ttu-id="c7b3d-122">Les modèles éblouissants par défaut spécifient le `MainLayout` composant.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-122">The default Blazor templates specify the `MainLayout` component.</span></span> <span data-ttu-id="c7b3d-123">*MainLayout. Razor* se trouve dans le dossier *partagé* du projet de modèle.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-123">*MainLayout.razor* is in the template project's *Shared* folder.</span></span> <span data-ttu-id="c7b3d-124">Pour plus d’informations sur les mises en <xref:blazor/layouts>page, consultez.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-124">For more information on layouts, see <xref:blazor/layouts>.</span></span>

<span data-ttu-id="c7b3d-125">Plusieurs modèles de routage peuvent être appliqués à un composant.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-125">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="c7b3d-126">Le composant suivant répond aux demandes pour `/BlazorRoute` et `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="c7b3d-126">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> <span data-ttu-id="c7b3d-127">Pour que les URL soient correctement résolues, l’application `<base>` doit inclure une balise dans son fichier *wwwroot/index.html* (éblouissant webassembly) ou le fichier *pages/_Host. cshtml* (serveur éblouissant) avec le chemin d' `href` accès de base de l’application spécifié dans l’attribut (`<base href="/">`).</span><span class="sxs-lookup"><span data-stu-id="c7b3d-127">For URLs to resolve correctly, the app must include a `<base>` tag in its *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server) with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="c7b3d-128">Pour plus d'informations, consultez <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-128">For more information, see <xref:host-and-deploy/blazor/index#app-base-path>.</span></span>

## <a name="provide-custom-content-when-content-isnt-found"></a><span data-ttu-id="c7b3d-129">Fournir du contenu personnalisé lorsque le contenu est introuvable</span><span class="sxs-lookup"><span data-stu-id="c7b3d-129">Provide custom content when content isn't found</span></span>

<span data-ttu-id="c7b3d-130">Le `Router` composant permet à l’application de spécifier du contenu personnalisé si le contenu est introuvable pour l’itinéraire demandé.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-130">The `Router` component allows the app to specify custom content if content isn't found for the requested route.</span></span>

<span data-ttu-id="c7b3d-131">Dans le fichier *app. Razor* , définissez le contenu personnalisé dans `NotFound` le paramètre de `Router` modèle du composant :</span><span class="sxs-lookup"><span data-stu-id="c7b3d-131">In the *App.razor* file, set custom content in the `NotFound` template parameter of the `Router` component:</span></span>

```cshtml
<Router AppAssembly="typeof(Startup).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <h1>Sorry</h1>
        <p>Sorry, there's nothing at this address.</p> b
    </NotFound>
</Router>
```

<span data-ttu-id="c7b3d-132">Le contenu des `<NotFound>` balises peut inclure des éléments arbitraires, tels que d’autres composants interactifs.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-132">The content of `<NotFound>` tags can include arbitrary items, such as other interactive components.</span></span> <span data-ttu-id="c7b3d-133">Pour appliquer une disposition par défaut `NotFound` au contenu, <xref:blazor/layouts>consultez.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-133">To apply a default layout to `NotFound` content, see <xref:blazor/layouts>.</span></span>

## <a name="route-to-components-from-multiple-assemblies"></a><span data-ttu-id="c7b3d-134">Acheminer vers des composants à partir de plusieurs assemblys</span><span class="sxs-lookup"><span data-stu-id="c7b3d-134">Route to components from multiple assemblies</span></span>

<span data-ttu-id="c7b3d-135">Utilisez le `AdditionalAssemblies` paramètre pour spécifier des assemblys supplémentaires que `Router` le composant doit prendre en compte lors de la recherche de composants routables.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-135">Use the `AdditionalAssemblies` parameter to specify additional assemblies for the `Router` component to consider when searching for routable components.</span></span> <span data-ttu-id="c7b3d-136">Les assemblys spécifiés sont pris en compte `AppAssembly`en plus de l’assembly spécifié.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-136">Specified assemblies are considered in addition to the `AppAssembly`-specified assembly.</span></span> <span data-ttu-id="c7b3d-137">Dans l’exemple suivant, `Component1` est un composant routable défini dans une bibliothèque de classes référencée.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-137">In the following example, `Component1` is a routable component defined in a referenced class library.</span></span> <span data-ttu-id="c7b3d-138">L’exemple `AdditionalAssemblies` suivant entraîne la prise en charge `Component1`du routage pour :</span><span class="sxs-lookup"><span data-stu-id="c7b3d-138">The following `AdditionalAssemblies` example results in routing support for `Component1`:</span></span>

<span data-ttu-id="c7b3d-139">< routeur AppAssembly = "typeof (Program). Assembly "AdditionalAssemblies =" New [] {typeof (Composant1). Assembly} >...</Router></span><span class="sxs-lookup"><span data-stu-id="c7b3d-139"><Router AppAssembly="typeof(Program).Assembly" AdditionalAssemblies="new[] { typeof(Component1).Assembly }> ... </Router></span></span>

## <a name="route-parameters"></a><span data-ttu-id="c7b3d-140">Paramètres d’itinéraire</span><span class="sxs-lookup"><span data-stu-id="c7b3d-140">Route parameters</span></span>

<span data-ttu-id="c7b3d-141">Le routeur utilise des paramètres de routage pour remplir les paramètres de composant correspondants avec le même nom (sans respect de la casse) :</span><span class="sxs-lookup"><span data-stu-id="c7b3d-141">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

<span data-ttu-id="c7b3d-142">Les paramètres facultatifs ne sont pas pris en charge pour les applications éblouissantes dans ASP.NET Core version préliminaire 3,0.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-142">Optional parameters aren't supported for Blazor apps in ASP.NET Core 3.0 Preview.</span></span> <span data-ttu-id="c7b3d-143">Deux `@page` directives sont appliquées dans l’exemple précédent.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-143">Two `@page` directives are applied in the previous example.</span></span> <span data-ttu-id="c7b3d-144">La première permet de naviguer jusqu’au composant sans paramètre.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-144">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="c7b3d-145">La deuxième `@page` directive prend le `{text}` paramètre d’itinéraire et assigne la valeur à `Text` la propriété.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-145">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="c7b3d-146">Contraintes d’itinéraire</span><span class="sxs-lookup"><span data-stu-id="c7b3d-146">Route constraints</span></span>

<span data-ttu-id="c7b3d-147">Une contrainte d’itinéraire applique la correspondance de type sur un segment de routage à un composant.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-147">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="c7b3d-148">Dans l’exemple suivant, l’itinéraire vers le `Users` composant correspond uniquement à si :</span><span class="sxs-lookup"><span data-stu-id="c7b3d-148">In the following example, the route to the `Users` component only matches if:</span></span>

* <span data-ttu-id="c7b3d-149">Un `Id` segment de routage est présent sur l’URL de la demande.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-149">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="c7b3d-150">Le `Id` segment est un entier (`int`).</span><span class="sxs-lookup"><span data-stu-id="c7b3d-150">The `Id` segment is an integer (`int`).</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

<span data-ttu-id="c7b3d-151">Les contraintes de routage indiquées dans le tableau suivant sont disponibles.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-151">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="c7b3d-152">Pour plus d’informations sur les contraintes d’itinéraire qui correspondent à la culture dite indifférente, consultez l’avertissement sous le tableau.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-152">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="c7b3d-153">Contrainte</span><span class="sxs-lookup"><span data-stu-id="c7b3d-153">Constraint</span></span> | <span data-ttu-id="c7b3d-154">Exemple</span><span class="sxs-lookup"><span data-stu-id="c7b3d-154">Example</span></span>           | <span data-ttu-id="c7b3d-155">Exemples de correspondances</span><span class="sxs-lookup"><span data-stu-id="c7b3d-155">Example Matches</span></span>                                                                  | <span data-ttu-id="c7b3d-156">Invariant</span><span class="sxs-lookup"><span data-stu-id="c7b3d-156">Invariant</span></span><br><span data-ttu-id="c7b3d-157">culture</span><span class="sxs-lookup"><span data-stu-id="c7b3d-157">culture</span></span><br><span data-ttu-id="c7b3d-158">correspondance</span><span class="sxs-lookup"><span data-stu-id="c7b3d-158">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="c7b3d-159">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="c7b3d-159">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="c7b3d-160">Non</span><span class="sxs-lookup"><span data-stu-id="c7b3d-160">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="c7b3d-161">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="c7b3d-161">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="c7b3d-162">Oui</span><span class="sxs-lookup"><span data-stu-id="c7b3d-162">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="c7b3d-163">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="c7b3d-163">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="c7b3d-164">Oui</span><span class="sxs-lookup"><span data-stu-id="c7b3d-164">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="c7b3d-165">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="c7b3d-165">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="c7b3d-166">Oui</span><span class="sxs-lookup"><span data-stu-id="c7b3d-166">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="c7b3d-167">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="c7b3d-167">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="c7b3d-168">Oui</span><span class="sxs-lookup"><span data-stu-id="c7b3d-168">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="c7b3d-169">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="c7b3d-169">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="c7b3d-170">Non</span><span class="sxs-lookup"><span data-stu-id="c7b3d-170">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="c7b3d-171">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="c7b3d-171">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="c7b3d-172">Oui</span><span class="sxs-lookup"><span data-stu-id="c7b3d-172">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="c7b3d-173">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="c7b3d-173">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="c7b3d-174">Oui</span><span class="sxs-lookup"><span data-stu-id="c7b3d-174">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="c7b3d-175">Les contraintes de routage qui vérifient que l’URL peut être convertie en type CLR (comme `int` ou `DateTime`) utilisent toujours la culture invariant.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-175">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="c7b3d-176">ces contraintes partent du principe que l’URL n’est pas localisable.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-176">These constraints assume that the URL is non-localizable.</span></span>

### <a name="routing-with-urls-that-contain-dots"></a><span data-ttu-id="c7b3d-177">Routage avec des URL qui contiennent des points</span><span class="sxs-lookup"><span data-stu-id="c7b3d-177">Routing with URLs that contain dots</span></span>

<span data-ttu-id="c7b3d-178">Dans les applications serveur éblouissantes, l’itinéraire par défaut dans *_Host. cshtml* est `/` (`@page "/"`).</span><span class="sxs-lookup"><span data-stu-id="c7b3d-178">In Blazor Server apps, the default route in *_Host.cshtml* is `/` (`@page "/"`).</span></span> <span data-ttu-id="c7b3d-179">Une URL de demande qui contient un point`.`() ne correspond pas à l’itinéraire par défaut, car l’URL semble demander un fichier.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-179">A request URL that contains a dot (`.`) isn't matched by the default route because the URL appears to request a file.</span></span> <span data-ttu-id="c7b3d-180">Une application éblouissant retourne une réponse *404-introuvable* pour un fichier statique qui n’existe pas.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-180">A Blazor app returns a *404 - Not Found* response for a static file that doesn't exist.</span></span> <span data-ttu-id="c7b3d-181">Pour utiliser des itinéraires qui contiennent un point, configurez *_Host. cshtml* avec le modèle de routage suivant :</span><span class="sxs-lookup"><span data-stu-id="c7b3d-181">To use routes that contain a dot, configure *_Host.cshtml* with the following route template:</span></span>

```cshtml
@page "/{**path}"
```

<span data-ttu-id="c7b3d-182">Le `"/{**path}"` modèle comprend les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="c7b3d-182">The `"/{**path}"` template includes:</span></span>

* <span data-ttu-id="c7b3d-183">Double-astérisque syntaxe *catch-all* (`**`) pour capturer le chemin d’accès dans plusieurs limites de dossiers sans encodage`/`des barres obliques ().</span><span class="sxs-lookup"><span data-stu-id="c7b3d-183">Double-asterisk *catch-all* syntax (`**`) to capture the path across multiple folder boundaries without encoding forward slashes (`/`).</span></span>
* <span data-ttu-id="c7b3d-184">Nom `path` de paramètre d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-184">A `path` route parameter name.</span></span>

<span data-ttu-id="c7b3d-185">Pour plus d'informations, consultez <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-185">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="c7b3d-186">Composant NavLink</span><span class="sxs-lookup"><span data-stu-id="c7b3d-186">NavLink component</span></span>

<span data-ttu-id="c7b3d-187">Utilisez un `NavLink` composant à la place des éléments Hyperlink`<a>`html () lors de la création de liens de navigation.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-187">Use a `NavLink` component in place of HTML hyperlink elements (`<a>`) when creating navigation links.</span></span> <span data-ttu-id="c7b3d-188">Un `NavLink` composant se comporte comme un `<a>` élément, sauf qu’il fait basculer `active` une classe CSS selon que son `href` correspond à l’URL actuelle.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-188">A `NavLink` component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="c7b3d-189">La `active` classe permet à un utilisateur de comprendre quelle page est la page active parmi les liens de navigation affichés.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-189">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="c7b3d-190">Le composant `NavMenu` suivant crée une barre de navigation de [démarrage](https://getbootstrap.com/docs/) qui montre comment `NavLink` utiliser des composants :</span><span class="sxs-lookup"><span data-stu-id="c7b3d-190">The following `NavMenu` component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use `NavLink` components:</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

<span data-ttu-id="c7b3d-191">Il existe deux `NavLinkMatch` options que vous pouvez assigner `Match` à l’attribut `<NavLink>` de l’élément :</span><span class="sxs-lookup"><span data-stu-id="c7b3d-191">There are two `NavLinkMatch` options that you can assign to the `Match` attribute of the `<NavLink>` element:</span></span>

* <span data-ttu-id="c7b3d-192">`NavLinkMatch.All`&ndash; Estactiflorsqu’ilcorrespondàlatotalité`NavLink` de l’URL actuelle.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-192">`NavLinkMatch.All` &ndash; The `NavLink` is active when it matches the entire current URL.</span></span>
* <span data-ttu-id="c7b3d-193">`NavLinkMatch.Prefix`(*par défaut*) &ndash; Estactiflorsqu’ilcorrespondàn’importequelpréfixe`NavLink` de l’URL actuelle.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-193">`NavLinkMatch.Prefix` (*default*) &ndash; The `NavLink` is active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="c7b3d-194">Dans l’exemple précédent, la page `NavLink` d’hébergement `href=""` correspond à l’URL de base `active` et reçoit uniquement la classe CSS à l’URL du chemin de base par `https://localhost:5001/`défaut de l’application (par exemple,).</span><span class="sxs-lookup"><span data-stu-id="c7b3d-194">In the preceding example, the Home `NavLink` `href=""` matches the home URL and only receives the `active` CSS class at the app's default base path URL (for example, `https://localhost:5001/`).</span></span> <span data-ttu-id="c7b3d-195">Le deuxième `NavLink` reçoit la `active` classe lorsque l’utilisateur visite une URL avec un `MyComponent` préfixe (par exemple `https://localhost:5001/MyComponent` , `https://localhost:5001/MyComponent/AnotherSegment`et).</span><span class="sxs-lookup"><span data-stu-id="c7b3d-195">The second `NavLink` receives the `active` class when the user visits any URL with a `MyComponent` prefix (for example, `https://localhost:5001/MyComponent` and `https://localhost:5001/MyComponent/AnotherSegment`).</span></span>

<span data-ttu-id="c7b3d-196">Les `NavLink` attributs de composant supplémentaires sont passés à la balise d’ancrage rendue.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-196">Additional `NavLink` component attributes are passed through to the rendered anchor tag.</span></span> <span data-ttu-id="c7b3d-197">Dans l’exemple suivant, le `NavLink` composant comprend l' `target` attribut :</span><span class="sxs-lookup"><span data-stu-id="c7b3d-197">In the following example, the `NavLink` component includes the `target` attribute:</span></span>

```cshtml
<NavLink href="my-page" target="_blank">My page</NavLink>
```

<span data-ttu-id="c7b3d-198">Le balisage HTML suivant est rendu :</span><span class="sxs-lookup"><span data-stu-id="c7b3d-198">The following HTML markup is rendered:</span></span>

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a><span data-ttu-id="c7b3d-199">Aide des URI et de l’état de navigation</span><span class="sxs-lookup"><span data-stu-id="c7b3d-199">URI and navigation state helpers</span></span>

<span data-ttu-id="c7b3d-200">Utilisez `Microsoft.AspNetCore.Components.NavigationManager` pour travailler avec les URI et la C# navigation dans le code.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-200">Use `Microsoft.AspNetCore.Components.NavigationManager` to work with URIs and navigation in C# code.</span></span> <span data-ttu-id="c7b3d-201">`NavigationManager`fournit l’événement et les méthodes répertoriées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-201">`NavigationManager` provides the event and methods shown in the following table.</span></span>

| <span data-ttu-id="c7b3d-202">Membre</span><span class="sxs-lookup"><span data-stu-id="c7b3d-202">Member</span></span> | <span data-ttu-id="c7b3d-203">Description</span><span class="sxs-lookup"><span data-stu-id="c7b3d-203">Description</span></span> |
| ------ | ----------- |
| `Uri` | <span data-ttu-id="c7b3d-204">Obtient l’URI absolu actuel.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-204">Gets the current absolute URI.</span></span> |
| `BaseUri` | <span data-ttu-id="c7b3d-205">Obtient l’URI de base (avec une barre oblique finale) qui peut être ajouté aux chemins d’accès URI relatifs pour produire un URI absolu.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-205">Gets the base URI (with a trailing slash) that can be prepended to relative URI paths to produce an absolute URI.</span></span> <span data-ttu-id="c7b3d-206">En général `BaseUri` , correspond à `href` l’attribut sur l’élément `<base>` du document dans *wwwroot/index.html* (éblouissante webassembly) ou *pages/_Host. cshtml* (serveur éblouissant).</span><span class="sxs-lookup"><span data-stu-id="c7b3d-206">Typically, `BaseUri` corresponds to the `href` attribute on the document's `<base>` element in *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server).</span></span> |
| `NavigateTo` | <span data-ttu-id="c7b3d-207">Navigue vers l’URI spécifié.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-207">Navigates to the specified URI.</span></span> <span data-ttu-id="c7b3d-208">Si `forceLoad` est `true`:</span><span class="sxs-lookup"><span data-stu-id="c7b3d-208">If `forceLoad` is `true`:</span></span><ul><li><span data-ttu-id="c7b3d-209">Le routage côté client est contourné.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-209">Client-side routing is bypassed.</span></span></li><li><span data-ttu-id="c7b3d-210">Le navigateur est obligé de charger la nouvelle page à partir du serveur, que l’URI soit normalement géré ou non par le routeur côté client.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-210">The browser is forced to load the new page from the server, whether or not the URI is normally handled by the client-side router.</span></span></li></ul> |
| `LocationChanged` | <span data-ttu-id="c7b3d-211">Événement qui se déclenche lorsque l’emplacement de navigation a changé.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-211">An event that fires when the navigation location has changed.</span></span> |
| `ToAbsoluteUri` | <span data-ttu-id="c7b3d-212">Convertit un URI relatif en URI absolu.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-212">Converts a relative URI into an absolute URI.</span></span> |
| `ToBaseRelativePath` | <span data-ttu-id="c7b3d-213">À partir d’un URI de base (par exemple, un URI `GetBaseUri`précédemment retourné par), convertit un URI absolu en un URI relatif au préfixe URI de base.</span><span class="sxs-lookup"><span data-stu-id="c7b3d-213">Given a base URI (for example, a URI previously returned by `GetBaseUri`), converts an absolute URI into a URI relative to the base URI prefix.</span></span> |

<span data-ttu-id="c7b3d-214">Le composant suivant accède au composant de `Counter` l’application lorsque le bouton est sélectionné :</span><span class="sxs-lookup"><span data-stu-id="c7b3d-214">The following component navigates to the app's `Counter` component when the button is selected:</span></span>

```cshtml
@page "/navigate"
@inject NavigationManager NavigationManager

<h1>Navigate in Code Example</h1>

<button class="btn btn-primary" @onclick="NavigateToCounterComponent">
    Navigate to the Counter component
</button>

@code {
    private void NavigateToCounterComponent()
    {
        NavigationManager.NavigateTo("counter");
    }
}
```
