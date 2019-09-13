---
title: ASP.NET Core du routage éblouissant
author: guardrex
description: Découvrez comment acheminer des requêtes dans des applications et sur le composant NavLink.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/06/2019
uid: blazor/routing
ms.openlocfilehash: 1c61eedf7dbf0bbc8796eaa11360783b9d7aba6c
ms.sourcegitcommit: 092061c4f6ef46ed2165fa84de6273d3786fb97e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70963871"
---
# <a name="aspnet-core-blazor-routing"></a><span data-ttu-id="d3a06-103">ASP.NET Core du routage éblouissant</span><span class="sxs-lookup"><span data-stu-id="d3a06-103">ASP.NET Core Blazor routing</span></span>

<span data-ttu-id="d3a06-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d3a06-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d3a06-105">Découvrez comment acheminer les demandes et comment utiliser le `NavLink` composant pour créer des liens de navigation dans les applications éblouissantes.</span><span class="sxs-lookup"><span data-stu-id="d3a06-105">Learn how to route requests and how to use the `NavLink` component to create navigation links in Blazor apps.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="d3a06-106">Intégration du routage du point de terminaison ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d3a06-106">ASP.NET Core endpoint routing integration</span></span>

<span data-ttu-id="d3a06-107">Le serveur éblouissant est intégré à [ASP.net Core routage des points de terminaison](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="d3a06-107">Blazor Server is integrated into [ASP.NET Core Endpoint Routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="d3a06-108">Une application ASP.net Core est configurée pour accepter les connexions entrantes pour `MapBlazorHub` les `Startup.Configure`composants interactifs avec dans :</span><span class="sxs-lookup"><span data-stu-id="d3a06-108">An ASP.NET Core app is configured to accept incoming connections for interactive components with `MapBlazorHub` in `Startup.Configure`:</span></span>

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

## <a name="route-templates"></a><span data-ttu-id="d3a06-109">Modèles de routage</span><span class="sxs-lookup"><span data-stu-id="d3a06-109">Route templates</span></span>

<span data-ttu-id="d3a06-110">Le `Router` composant active le routage vers chaque composant avec un itinéraire spécifié.</span><span class="sxs-lookup"><span data-stu-id="d3a06-110">The `Router` component enables routing to each component with a specified route.</span></span> <span data-ttu-id="d3a06-111">Le `Router` composant apparaît dans le fichier *app. Razor* :</span><span class="sxs-lookup"><span data-stu-id="d3a06-111">The `Router` component appears in the *App.razor* file:</span></span>

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

<span data-ttu-id="d3a06-112">Lorsqu’un fichier *. Razor* avec une `@page` directive est compilé, la classe générée est fournie en <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> spécifiant le modèle de routage.</span><span class="sxs-lookup"><span data-stu-id="d3a06-112">When a *.razor* file with an `@page` directive is compiled, the generated class is provided a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span>

<span data-ttu-id="d3a06-113">Au moment de l' `RouteView` exécution, le composant :</span><span class="sxs-lookup"><span data-stu-id="d3a06-113">At runtime, the `RouteView` component:</span></span>

* <span data-ttu-id="d3a06-114">Reçoit du `RouteData` `Router` avec les paramètres souhaités.</span><span class="sxs-lookup"><span data-stu-id="d3a06-114">Receives the `RouteData` from the `Router` along with any desired parameters.</span></span>
* <span data-ttu-id="d3a06-115">Restitue le composant spécifié avec sa disposition (ou une disposition par défaut facultative) à l’aide des paramètres spécifiés.</span><span class="sxs-lookup"><span data-stu-id="d3a06-115">Renders the specified component with its layout (or an optional default layout) using the specified parameters.</span></span>

<span data-ttu-id="d3a06-116">Vous pouvez éventuellement spécifier un `DefaultLayout` paramètre avec une classe de disposition à utiliser pour les composants qui ne spécifient pas de disposition.</span><span class="sxs-lookup"><span data-stu-id="d3a06-116">You can optionally specify a `DefaultLayout` parameter with a layout class to use for components that don't specify a layout.</span></span> <span data-ttu-id="d3a06-117">Les modèles éblouissants par défaut spécifient le `MainLayout` composant.</span><span class="sxs-lookup"><span data-stu-id="d3a06-117">The default Blazor templates specify the `MainLayout` component.</span></span> <span data-ttu-id="d3a06-118">*MainLayout. Razor* se trouve dans le dossier *partagé* du projet de modèle.</span><span class="sxs-lookup"><span data-stu-id="d3a06-118">*MainLayout.razor* is in the template project's *Shared* folder.</span></span> <span data-ttu-id="d3a06-119">Pour plus d’informations sur les mises en <xref:blazor/layouts>page, consultez.</span><span class="sxs-lookup"><span data-stu-id="d3a06-119">For more information on layouts, see <xref:blazor/layouts>.</span></span>

<span data-ttu-id="d3a06-120">Plusieurs modèles de routage peuvent être appliqués à un composant.</span><span class="sxs-lookup"><span data-stu-id="d3a06-120">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="d3a06-121">Le composant suivant répond aux demandes pour `/BlazorRoute` et `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="d3a06-121">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> <span data-ttu-id="d3a06-122">Pour que les URL soient correctement résolues, l’application `<base>` doit inclure une balise dans son fichier *wwwroot/index.html* (éblouissant webassembly) ou le fichier *pages/_Host. cshtml* (serveur éblouissant) avec le chemin d' `href` accès de base de l’application spécifié dans l’attribut (`<base href="/">`).</span><span class="sxs-lookup"><span data-stu-id="d3a06-122">For URLs to resolve correctly, the app must include a `<base>` tag in its *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server) with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="d3a06-123">Pour plus d'informations, consultez <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="d3a06-123">For more information, see <xref:host-and-deploy/blazor/index#app-base-path>.</span></span>

## <a name="provide-custom-content-when-content-isnt-found"></a><span data-ttu-id="d3a06-124">Fournir du contenu personnalisé lorsque le contenu est introuvable</span><span class="sxs-lookup"><span data-stu-id="d3a06-124">Provide custom content when content isn't found</span></span>

<span data-ttu-id="d3a06-125">Le `Router` composant permet à l’application de spécifier du contenu personnalisé si le contenu est introuvable pour l’itinéraire demandé.</span><span class="sxs-lookup"><span data-stu-id="d3a06-125">The `Router` component allows the app to specify custom content if content isn't found for the requested route.</span></span>

<span data-ttu-id="d3a06-126">Dans le fichier *app. Razor* , définissez le contenu personnalisé dans `NotFound` le paramètre de `Router` modèle du composant :</span><span class="sxs-lookup"><span data-stu-id="d3a06-126">In the *App.razor* file, set custom content in the `NotFound` template parameter of the `Router` component:</span></span>

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

<span data-ttu-id="d3a06-127">Le contenu des `<NotFound>` balises peut inclure des éléments arbitraires, tels que d’autres composants interactifs.</span><span class="sxs-lookup"><span data-stu-id="d3a06-127">The content of `<NotFound>` tags can include arbitrary items, such as other interactive components.</span></span> <span data-ttu-id="d3a06-128">Pour appliquer une disposition par défaut `NotFound` au contenu, <xref:blazor/layouts>consultez.</span><span class="sxs-lookup"><span data-stu-id="d3a06-128">To apply a default layout to `NotFound` content, see <xref:blazor/layouts>.</span></span>

## <a name="route-to-components-from-multiple-assemblies"></a><span data-ttu-id="d3a06-129">Acheminer vers des composants à partir de plusieurs assemblys</span><span class="sxs-lookup"><span data-stu-id="d3a06-129">Route to components from multiple assemblies</span></span>

<span data-ttu-id="d3a06-130">Utilisez le `AdditionalAssemblies` paramètre pour spécifier des assemblys supplémentaires que `Router` le composant doit prendre en compte lors de la recherche de composants routables.</span><span class="sxs-lookup"><span data-stu-id="d3a06-130">Use the `AdditionalAssemblies` parameter to specify additional assemblies for the `Router` component to consider when searching for routable components.</span></span> <span data-ttu-id="d3a06-131">Les assemblys spécifiés sont pris en compte `AppAssembly`en plus de l’assembly spécifié.</span><span class="sxs-lookup"><span data-stu-id="d3a06-131">Specified assemblies are considered in addition to the `AppAssembly`-specified assembly.</span></span> <span data-ttu-id="d3a06-132">Dans l’exemple suivant, `Component1` est un composant routable défini dans une bibliothèque de classes référencée.</span><span class="sxs-lookup"><span data-stu-id="d3a06-132">In the following example, `Component1` is a routable component defined in a referenced class library.</span></span> <span data-ttu-id="d3a06-133">L’exemple `AdditionalAssemblies` suivant entraîne la prise en charge `Component1`du routage pour :</span><span class="sxs-lookup"><span data-stu-id="d3a06-133">The following `AdditionalAssemblies` example results in routing support for `Component1`:</span></span>

<span data-ttu-id="d3a06-134">< routeur AppAssembly = "typeof (Program). Assembly "AdditionalAssemblies =" New [] {typeof (Composant1). Assembly} >...</Router></span><span class="sxs-lookup"><span data-stu-id="d3a06-134"><Router AppAssembly="typeof(Program).Assembly" AdditionalAssemblies="new[] { typeof(Component1).Assembly }> ... </Router></span></span>

## <a name="route-parameters"></a><span data-ttu-id="d3a06-135">Paramètres d’itinéraire</span><span class="sxs-lookup"><span data-stu-id="d3a06-135">Route parameters</span></span>

<span data-ttu-id="d3a06-136">Le routeur utilise des paramètres de routage pour remplir les paramètres de composant correspondants avec le même nom (sans respect de la casse) :</span><span class="sxs-lookup"><span data-stu-id="d3a06-136">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

<span data-ttu-id="d3a06-137">Les paramètres facultatifs ne sont pas pris en charge pour les applications éblouissantes dans ASP.NET Core version préliminaire 3,0.</span><span class="sxs-lookup"><span data-stu-id="d3a06-137">Optional parameters aren't supported for Blazor apps in ASP.NET Core 3.0 Preview.</span></span> <span data-ttu-id="d3a06-138">Deux `@page` directives sont appliquées dans l’exemple précédent.</span><span class="sxs-lookup"><span data-stu-id="d3a06-138">Two `@page` directives are applied in the previous example.</span></span> <span data-ttu-id="d3a06-139">La première permet de naviguer jusqu’au composant sans paramètre.</span><span class="sxs-lookup"><span data-stu-id="d3a06-139">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="d3a06-140">La deuxième `@page` directive prend le `{text}` paramètre d’itinéraire et assigne la valeur à `Text` la propriété.</span><span class="sxs-lookup"><span data-stu-id="d3a06-140">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="d3a06-141">Contraintes d’itinéraire</span><span class="sxs-lookup"><span data-stu-id="d3a06-141">Route constraints</span></span>

<span data-ttu-id="d3a06-142">Une contrainte d’itinéraire applique la correspondance de type sur un segment de routage à un composant.</span><span class="sxs-lookup"><span data-stu-id="d3a06-142">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="d3a06-143">Dans l’exemple suivant, l’itinéraire vers le `Users` composant correspond uniquement à si :</span><span class="sxs-lookup"><span data-stu-id="d3a06-143">In the following example, the route to the `Users` component only matches if:</span></span>

* <span data-ttu-id="d3a06-144">Un `Id` segment de routage est présent sur l’URL de la demande.</span><span class="sxs-lookup"><span data-stu-id="d3a06-144">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="d3a06-145">Le `Id` segment est un entier (`int`).</span><span class="sxs-lookup"><span data-stu-id="d3a06-145">The `Id` segment is an integer (`int`).</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

<span data-ttu-id="d3a06-146">Les contraintes de routage indiquées dans le tableau suivant sont disponibles.</span><span class="sxs-lookup"><span data-stu-id="d3a06-146">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="d3a06-147">Pour plus d’informations sur les contraintes d’itinéraire qui correspondent à la culture dite indifférente, consultez l’avertissement sous le tableau.</span><span class="sxs-lookup"><span data-stu-id="d3a06-147">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="d3a06-148">Contrainte</span><span class="sxs-lookup"><span data-stu-id="d3a06-148">Constraint</span></span> | <span data-ttu-id="d3a06-149">Exemple</span><span class="sxs-lookup"><span data-stu-id="d3a06-149">Example</span></span>           | <span data-ttu-id="d3a06-150">Exemples de correspondances</span><span class="sxs-lookup"><span data-stu-id="d3a06-150">Example Matches</span></span>                                                                  | <span data-ttu-id="d3a06-151">Invariant</span><span class="sxs-lookup"><span data-stu-id="d3a06-151">Invariant</span></span><br><span data-ttu-id="d3a06-152">culture</span><span class="sxs-lookup"><span data-stu-id="d3a06-152">culture</span></span><br><span data-ttu-id="d3a06-153">correspondance</span><span class="sxs-lookup"><span data-stu-id="d3a06-153">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="d3a06-154">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="d3a06-154">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="d3a06-155">Non</span><span class="sxs-lookup"><span data-stu-id="d3a06-155">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="d3a06-156">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="d3a06-156">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="d3a06-157">Oui</span><span class="sxs-lookup"><span data-stu-id="d3a06-157">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="d3a06-158">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="d3a06-158">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="d3a06-159">Oui</span><span class="sxs-lookup"><span data-stu-id="d3a06-159">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="d3a06-160">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="d3a06-160">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="d3a06-161">Oui</span><span class="sxs-lookup"><span data-stu-id="d3a06-161">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="d3a06-162">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="d3a06-162">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="d3a06-163">Oui</span><span class="sxs-lookup"><span data-stu-id="d3a06-163">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="d3a06-164">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="d3a06-164">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="d3a06-165">Non</span><span class="sxs-lookup"><span data-stu-id="d3a06-165">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="d3a06-166">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="d3a06-166">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="d3a06-167">Oui</span><span class="sxs-lookup"><span data-stu-id="d3a06-167">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="d3a06-168">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="d3a06-168">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="d3a06-169">Oui</span><span class="sxs-lookup"><span data-stu-id="d3a06-169">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="d3a06-170">Les contraintes de routage qui vérifient que l’URL peut être convertie en type CLR (comme `int` ou `DateTime`) utilisent toujours la culture invariant.</span><span class="sxs-lookup"><span data-stu-id="d3a06-170">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="d3a06-171">ces contraintes partent du principe que l’URL n’est pas localisable.</span><span class="sxs-lookup"><span data-stu-id="d3a06-171">These constraints assume that the URL is non-localizable.</span></span>

### <a name="routing-with-urls-that-contain-dots"></a><span data-ttu-id="d3a06-172">Routage avec des URL qui contiennent des points</span><span class="sxs-lookup"><span data-stu-id="d3a06-172">Routing with URLs that contain dots</span></span>

<span data-ttu-id="d3a06-173">Dans les applications serveur éblouissantes, l’itinéraire par défaut dans *_Host. cshtml* est `/` (`@page "/"`).</span><span class="sxs-lookup"><span data-stu-id="d3a06-173">In Blazor Server apps, the default route in *_Host.cshtml* is `/` (`@page "/"`).</span></span> <span data-ttu-id="d3a06-174">Une URL de demande qui contient un point`.`() ne correspond pas à l’itinéraire par défaut, car l’URL semble demander un fichier.</span><span class="sxs-lookup"><span data-stu-id="d3a06-174">A request URL that contains a dot (`.`) isn't matched by the default route because the URL appears to request a file.</span></span> <span data-ttu-id="d3a06-175">Une application éblouissant retourne une réponse *404-introuvable* pour un fichier statique qui n’existe pas.</span><span class="sxs-lookup"><span data-stu-id="d3a06-175">A Blazor app returns a *404 - Not Found* response for a static file that doesn't exist.</span></span> <span data-ttu-id="d3a06-176">Pour utiliser des itinéraires qui contiennent un point, configurez *_Host. cshtml* avec le modèle de routage suivant :</span><span class="sxs-lookup"><span data-stu-id="d3a06-176">To use routes that contain a dot, configure *_Host.cshtml* with the following route template:</span></span>

```cshtml
@page "/{**path}"
```

<span data-ttu-id="d3a06-177">Le `"/{**path}"` modèle comprend les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="d3a06-177">The `"/{**path}"` template includes:</span></span>

* <span data-ttu-id="d3a06-178">Double-astérisque syntaxe *catch-all* (`**`) pour capturer le chemin d’accès dans plusieurs limites de dossiers sans encodage`/`des barres obliques ().</span><span class="sxs-lookup"><span data-stu-id="d3a06-178">Double-asterisk *catch-all* syntax (`**`) to capture the path across multiple folder boundaries without encoding forward slashes (`/`).</span></span>
* <span data-ttu-id="d3a06-179">Nom `path` de paramètre d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="d3a06-179">A `path` route parameter name.</span></span>

<span data-ttu-id="d3a06-180">Pour plus d'informations, consultez <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="d3a06-180">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="d3a06-181">Composant NavLink</span><span class="sxs-lookup"><span data-stu-id="d3a06-181">NavLink component</span></span>

<span data-ttu-id="d3a06-182">Utilisez un `NavLink` composant à la place des éléments Hyperlink`<a>`html () lors de la création de liens de navigation.</span><span class="sxs-lookup"><span data-stu-id="d3a06-182">Use a `NavLink` component in place of HTML hyperlink elements (`<a>`) when creating navigation links.</span></span> <span data-ttu-id="d3a06-183">Un `NavLink` composant se comporte comme un `<a>` élément, sauf qu’il fait basculer `active` une classe CSS selon que son `href` correspond à l’URL actuelle.</span><span class="sxs-lookup"><span data-stu-id="d3a06-183">A `NavLink` component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="d3a06-184">La `active` classe permet à un utilisateur de comprendre quelle page est la page active parmi les liens de navigation affichés.</span><span class="sxs-lookup"><span data-stu-id="d3a06-184">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="d3a06-185">Le composant `NavMenu` suivant crée une barre de navigation de [démarrage](https://getbootstrap.com/docs/) qui montre comment `NavLink` utiliser des composants :</span><span class="sxs-lookup"><span data-stu-id="d3a06-185">The following `NavMenu` component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use `NavLink` components:</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

<span data-ttu-id="d3a06-186">Il existe deux `NavLinkMatch` options que vous pouvez assigner `Match` à l’attribut `<NavLink>` de l’élément :</span><span class="sxs-lookup"><span data-stu-id="d3a06-186">There are two `NavLinkMatch` options that you can assign to the `Match` attribute of the `<NavLink>` element:</span></span>

* <span data-ttu-id="d3a06-187">`NavLinkMatch.All`&ndash; Estactiflorsqu’ilcorrespondàlatotalité`NavLink` de l’URL actuelle.</span><span class="sxs-lookup"><span data-stu-id="d3a06-187">`NavLinkMatch.All` &ndash; The `NavLink` is active when it matches the entire current URL.</span></span>
* <span data-ttu-id="d3a06-188">`NavLinkMatch.Prefix`(*par défaut*) &ndash; Estactiflorsqu’ilcorrespondàn’importequelpréfixe`NavLink` de l’URL actuelle.</span><span class="sxs-lookup"><span data-stu-id="d3a06-188">`NavLinkMatch.Prefix` (*default*) &ndash; The `NavLink` is active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="d3a06-189">Dans l’exemple précédent, la page `NavLink` d’hébergement `href=""` correspond à l’URL de base `active` et reçoit uniquement la classe CSS à l’URL du chemin de base par `https://localhost:5001/`défaut de l’application (par exemple,).</span><span class="sxs-lookup"><span data-stu-id="d3a06-189">In the preceding example, the Home `NavLink` `href=""` matches the home URL and only receives the `active` CSS class at the app's default base path URL (for example, `https://localhost:5001/`).</span></span> <span data-ttu-id="d3a06-190">Le deuxième `NavLink` reçoit la `active` classe lorsque l’utilisateur visite une URL avec un `MyComponent` préfixe (par exemple `https://localhost:5001/MyComponent` , `https://localhost:5001/MyComponent/AnotherSegment`et).</span><span class="sxs-lookup"><span data-stu-id="d3a06-190">The second `NavLink` receives the `active` class when the user visits any URL with a `MyComponent` prefix (for example, `https://localhost:5001/MyComponent` and `https://localhost:5001/MyComponent/AnotherSegment`).</span></span>

<span data-ttu-id="d3a06-191">Les `NavLink` attributs de composant supplémentaires sont passés à la balise d’ancrage rendue.</span><span class="sxs-lookup"><span data-stu-id="d3a06-191">Additional `NavLink` component attributes are passed through to the rendered anchor tag.</span></span> <span data-ttu-id="d3a06-192">Dans l’exemple suivant, le `NavLink` composant comprend l' `target` attribut :</span><span class="sxs-lookup"><span data-stu-id="d3a06-192">In the following example, the `NavLink` component includes the `target` attribute:</span></span>

```cshtml
<NavLink href="my-page" target="_blank">My page</NavLink>
```

<span data-ttu-id="d3a06-193">Le balisage HTML suivant est rendu :</span><span class="sxs-lookup"><span data-stu-id="d3a06-193">The following HTML markup is rendered:</span></span>

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a><span data-ttu-id="d3a06-194">Aide des URI et de l’état de navigation</span><span class="sxs-lookup"><span data-stu-id="d3a06-194">URI and navigation state helpers</span></span>

<span data-ttu-id="d3a06-195">Utilisez `Microsoft.AspNetCore.Components.NavigationManager` pour travailler avec les URI et la C# navigation dans le code.</span><span class="sxs-lookup"><span data-stu-id="d3a06-195">Use `Microsoft.AspNetCore.Components.NavigationManager` to work with URIs and navigation in C# code.</span></span> <span data-ttu-id="d3a06-196">`NavigationManager`fournit l’événement et les méthodes répertoriées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="d3a06-196">`NavigationManager` provides the event and methods shown in the following table.</span></span>

| <span data-ttu-id="d3a06-197">Membre</span><span class="sxs-lookup"><span data-stu-id="d3a06-197">Member</span></span> | <span data-ttu-id="d3a06-198">Description</span><span class="sxs-lookup"><span data-stu-id="d3a06-198">Description</span></span> |
| ------ | ----------- |
| `Uri` | <span data-ttu-id="d3a06-199">Obtient l’URI absolu actuel.</span><span class="sxs-lookup"><span data-stu-id="d3a06-199">Gets the current absolute URI.</span></span> |
| `BaseUri` | <span data-ttu-id="d3a06-200">Obtient l’URI de base (avec une barre oblique finale) qui peut être ajouté aux chemins d’accès URI relatifs pour produire un URI absolu.</span><span class="sxs-lookup"><span data-stu-id="d3a06-200">Gets the base URI (with a trailing slash) that can be prepended to relative URI paths to produce an absolute URI.</span></span> <span data-ttu-id="d3a06-201">En général `BaseUri` , correspond à `href` l’attribut sur l’élément `<base>` du document dans *wwwroot/index.html* (éblouissante webassembly) ou *pages/_Host. cshtml* (serveur éblouissant).</span><span class="sxs-lookup"><span data-stu-id="d3a06-201">Typically, `BaseUri` corresponds to the `href` attribute on the document's `<base>` element in *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server).</span></span> |
| `NavigateTo` | <span data-ttu-id="d3a06-202">Navigue vers l’URI spécifié.</span><span class="sxs-lookup"><span data-stu-id="d3a06-202">Navigates to the specified URI.</span></span> <span data-ttu-id="d3a06-203">Si `forceLoad` est `true`:</span><span class="sxs-lookup"><span data-stu-id="d3a06-203">If `forceLoad` is `true`:</span></span><ul><li><span data-ttu-id="d3a06-204">Le routage côté client est contourné.</span><span class="sxs-lookup"><span data-stu-id="d3a06-204">Client-side routing is bypassed.</span></span></li><li><span data-ttu-id="d3a06-205">Le navigateur est obligé de charger la nouvelle page à partir du serveur, que l’URI soit normalement géré ou non par le routeur côté client.</span><span class="sxs-lookup"><span data-stu-id="d3a06-205">The browser is forced to load the new page from the server, whether or not the URI is normally handled by the client-side router.</span></span></li></ul> |
| `LocationChanged` | <span data-ttu-id="d3a06-206">Événement qui se déclenche lorsque l’emplacement de navigation a changé.</span><span class="sxs-lookup"><span data-stu-id="d3a06-206">An event that fires when the navigation location has changed.</span></span> |
| `ToAbsoluteUri` | <span data-ttu-id="d3a06-207">Convertit un URI relatif en URI absolu.</span><span class="sxs-lookup"><span data-stu-id="d3a06-207">Converts a relative URI into an absolute URI.</span></span> |
| `ToBaseRelativePath` | <span data-ttu-id="d3a06-208">À partir d’un URI de base (par exemple, un URI `GetBaseUri`précédemment retourné par), convertit un URI absolu en un URI relatif au préfixe URI de base.</span><span class="sxs-lookup"><span data-stu-id="d3a06-208">Given a base URI (for example, a URI previously returned by `GetBaseUri`), converts an absolute URI into a URI relative to the base URI prefix.</span></span> |

<span data-ttu-id="d3a06-209">Le composant suivant accède au composant de `Counter` l’application lorsque le bouton est sélectionné :</span><span class="sxs-lookup"><span data-stu-id="d3a06-209">The following component navigates to the app's `Counter` component when the button is selected:</span></span>

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
