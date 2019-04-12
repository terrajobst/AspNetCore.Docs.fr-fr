---
title: Composants de Razor routage
author: guardrex
description: Découvrez comment acheminer les demandes dans les applications et sur le composant NavLink.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: razor-components/routing
ms.openlocfilehash: ef82fa7e0d571979a43fd8ce712bf4f22c06f9a7
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/10/2019
ms.locfileid: "59515547"
---
# <a name="razor-components-routing"></a><span data-ttu-id="f7745-103">Composants de Razor routage</span><span class="sxs-lookup"><span data-stu-id="f7745-103">Razor Components routing</span></span>

<span data-ttu-id="f7745-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f7745-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f7745-105">Découvrez comment acheminer les demandes dans les applications et sur le composant NavLink.</span><span class="sxs-lookup"><span data-stu-id="f7745-105">Learn how to route requests in apps and about the NavLink component.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="f7745-106">Intégration de routage de point de terminaison ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f7745-106">ASP.NET Core endpoint routing integration</span></span>

<span data-ttu-id="f7745-107">Composants de Razor sont intégrés à [routage ASP.NET Core](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="f7745-107">Razor Components are integrated into [ASP.NET Core routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="f7745-108">Une application ASP.NET Core est configurée pour accepter les connexions entrantes pour les composants de Razor interactive avec `MapComponentHub<TComponent>` dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="f7745-108">An ASP.NET Core app is configured to accept incoming connections for interactive Razor Components with `MapComponentHub<TComponent>` in `Startup.Configure`.</span></span> `MapComponentHub` <span data-ttu-id="f7745-109">Spécifie que le composant racine `App` doit être restitué dans un élément DOM correspondant le sélecteur `app`:</span><span class="sxs-lookup"><span data-stu-id="f7745-109">specifies that the root component `App` should be rendered within a DOM element matching the selector `app`:</span></span>

```csharp
app.UseRouting(routes =>
{
    routes.MapRazorPages();
    routes.MapComponentHub<App>("app");
});
```

## <a name="route-templates"></a><span data-ttu-id="f7745-110">Modèles de routes</span><span class="sxs-lookup"><span data-stu-id="f7745-110">Route templates</span></span>

<span data-ttu-id="f7745-111">Le `<Router>` composant Active le routage, et un modèle d’itinéraire est fourni pour chaque composant accessible.</span><span class="sxs-lookup"><span data-stu-id="f7745-111">The `<Router>` component enables routing, and a route template is provided to each accessible component.</span></span> <span data-ttu-id="f7745-112">Le `<Router>` composant apparaît dans le *Components/App.razor* fichier :</span><span class="sxs-lookup"><span data-stu-id="f7745-112">The `<Router>` component appears in the *Components/App.razor* file:</span></span>

```cshtml
<Router AppAssembly="typeof(Program).Assembly" />
```

<span data-ttu-id="f7745-113">Quand un *.razor* ou *.cshtml* de fichiers avec un `@page` directive est compilée, la classe générée est donnée un <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> spécifiant le modèle d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="f7745-113">When a *.razor* or *.cshtml* file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="f7745-114">Lors de l’exécution, le routeur recherche de classes de composant avec un `RouteAttribute` et restitue le composant a un modèle d’itinéraire qui correspond à l’URL demandée.</span><span class="sxs-lookup"><span data-stu-id="f7745-114">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="f7745-115">Plusieurs modèles d’itinéraire peuvent être appliqués à un composant.</span><span class="sxs-lookup"><span data-stu-id="f7745-115">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="f7745-116">Le composant suivant répond aux demandes de `/BlazorRoute` et `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="f7745-116">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?name=snippet_BlazorRoute)]

`<Router>` <span data-ttu-id="f7745-117">prend en charge la définition d’un composant de secours pour le rendu lorsqu’un itinéraire demandé n’est pas résolu.</span><span class="sxs-lookup"><span data-stu-id="f7745-117">supports setting a fallback component for rendering when a requested route isn't resolved.</span></span> <span data-ttu-id="f7745-118">Activer ce scénario participer en définissant le `FallbackComponent` paramètre vers le type de la classe de composant de secours.</span><span class="sxs-lookup"><span data-stu-id="f7745-118">Enable this opt-in scenario by setting the `FallbackComponent` parameter to the type of the fallback component class.</span></span>

<span data-ttu-id="f7745-119">L’exemple suivant définit un composant défini dans *Pages/MyFallbackRazorComponent.cshtml* en tant que le composant de secours pour un `<Router>`:</span><span class="sxs-lookup"><span data-stu-id="f7745-119">The following example sets a component defined in *Pages/MyFallbackRazorComponent.cshtml* as the fallback component for a `<Router>`:</span></span>

```cshtml
<Router ... FallbackComponent="typeof(Pages.MyFallbackRazorComponent)" />
```

> [!IMPORTANT]
> <span data-ttu-id="f7745-120">Pour générer des itinéraires correctement, l’application doit inclure un `<base>` balise dans son *wwwroot/index.HTML* fichier avec le chemin de base application spécifié dans le `href` attribut (`<base href="/">`).</span><span class="sxs-lookup"><span data-stu-id="f7745-120">To generate routes properly, the app must include a `<base>` tag in its *wwwroot/index.html* file with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="f7745-121">Pour plus d'informations, consultez <xref:host-and-deploy/razor-components-blazor/blazor#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="f7745-121">For more information, see <xref:host-and-deploy/razor-components-blazor/blazor#app-base-path>.</span></span>

## <a name="route-parameters"></a><span data-ttu-id="f7745-122">Paramètres d’itinéraire</span><span class="sxs-lookup"><span data-stu-id="f7745-122">Route parameters</span></span>

<span data-ttu-id="f7745-123">Le routeur utilise les paramètres d’itinéraire pour remplir les paramètres correspondants de composant portant le même nom (non respect de la casse) :</span><span class="sxs-lookup"><span data-stu-id="f7745-123">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?name=snippet_RouteParameter&highlight=2,7-8)]

<span data-ttu-id="f7745-124">Paramètres facultatifs ne sont pas pris en charge encore, par conséquent, deux `@page` directives sont appliquées dans l’exemple ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="f7745-124">Optional parameters aren't supported yet, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="f7745-125">La première permet la navigation vers le composant sans paramètre.</span><span class="sxs-lookup"><span data-stu-id="f7745-125">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="f7745-126">La seconde `@page` directive prend le `{text}` paramètre d’itinéraire et affecte la valeur à la `Text` propriété.</span><span class="sxs-lookup"><span data-stu-id="f7745-126">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="f7745-127">Contraintes de routage</span><span class="sxs-lookup"><span data-stu-id="f7745-127">Route constraints</span></span>

<span data-ttu-id="f7745-128">Une contrainte de route applique la mise en correspondance sur un segment de routage à un composant de type.</span><span class="sxs-lookup"><span data-stu-id="f7745-128">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="f7745-129">Dans l’exemple suivant, l’itinéraire vers le composant utilisateurs correspond uniquement si :</span><span class="sxs-lookup"><span data-stu-id="f7745-129">In the following example, the route to the Users component only matches if:</span></span>

* <span data-ttu-id="f7745-130">Un `Id` segment de routage est présent sur l’URL de requête.</span><span class="sxs-lookup"><span data-stu-id="f7745-130">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="f7745-131">Le `Id` segment est un entier (`int`).</span><span class="sxs-lookup"><span data-stu-id="f7745-131">The `Id` segment is an integer (`int`).</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.cshtml?highlight=1)]

<span data-ttu-id="f7745-132">Les contraintes d’itinéraire indiqués dans le tableau suivant sont disponibles.</span><span class="sxs-lookup"><span data-stu-id="f7745-132">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="f7745-133">Pour les contraintes d’itinéraire qui correspondent à la culture dite indifférente, consultez l’avertissement le tableau ci-dessous pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="f7745-133">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="f7745-134">Contrainte</span><span class="sxs-lookup"><span data-stu-id="f7745-134">Constraint</span></span> | <span data-ttu-id="f7745-135">Exemple</span><span class="sxs-lookup"><span data-stu-id="f7745-135">Example</span></span>           | <span data-ttu-id="f7745-136">Exemples de correspondances</span><span class="sxs-lookup"><span data-stu-id="f7745-136">Example Matches</span></span>                                                                  | <span data-ttu-id="f7745-137">Invariant</span><span class="sxs-lookup"><span data-stu-id="f7745-137">Invariant</span></span><br><span data-ttu-id="f7745-138">culture</span><span class="sxs-lookup"><span data-stu-id="f7745-138">culture</span></span><br><span data-ttu-id="f7745-139">correspondance</span><span class="sxs-lookup"><span data-stu-id="f7745-139">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | `true`<span data-ttu-id="f7745-140">,</span><span class="sxs-lookup"><span data-stu-id="f7745-140">,</span></span> `FALSE`                                                                  | <span data-ttu-id="f7745-141">Non</span><span class="sxs-lookup"><span data-stu-id="f7745-141">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | `2016-12-31`<span data-ttu-id="f7745-142">,</span><span class="sxs-lookup"><span data-stu-id="f7745-142">,</span></span> `2016-12-31 7:32pm`                                                | <span data-ttu-id="f7745-143">Oui</span><span class="sxs-lookup"><span data-stu-id="f7745-143">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | `49.99`<span data-ttu-id="f7745-144">,</span><span class="sxs-lookup"><span data-stu-id="f7745-144">,</span></span> `-1,000.01`                                                             | <span data-ttu-id="f7745-145">Oui</span><span class="sxs-lookup"><span data-stu-id="f7745-145">Yes</span></span>                              |
| `double`   | `{weight:double}` | `1.234`<span data-ttu-id="f7745-146">,</span><span class="sxs-lookup"><span data-stu-id="f7745-146">,</span></span> `-1,001.01e8`                                                           | <span data-ttu-id="f7745-147">Oui</span><span class="sxs-lookup"><span data-stu-id="f7745-147">Yes</span></span>                              |
| `float`    | `{weight:float}`  | `1.234`<span data-ttu-id="f7745-148">,</span><span class="sxs-lookup"><span data-stu-id="f7745-148">,</span></span> `-1,001.01e8`                                                           | <span data-ttu-id="f7745-149">Oui</span><span class="sxs-lookup"><span data-stu-id="f7745-149">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | `CD2C1638-1638-72D5-1638-DEADBEEF1638`<span data-ttu-id="f7745-150">,</span><span class="sxs-lookup"><span data-stu-id="f7745-150">,</span></span> `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | <span data-ttu-id="f7745-151">Non</span><span class="sxs-lookup"><span data-stu-id="f7745-151">No</span></span>                               |
| `int`      | `{id:int}`        | `123456789`<span data-ttu-id="f7745-152">,</span><span class="sxs-lookup"><span data-stu-id="f7745-152">,</span></span> `-123456789`                                                        | <span data-ttu-id="f7745-153">Oui</span><span class="sxs-lookup"><span data-stu-id="f7745-153">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | `123456789`<span data-ttu-id="f7745-154">,</span><span class="sxs-lookup"><span data-stu-id="f7745-154">,</span></span> `-123456789`                                                        | <span data-ttu-id="f7745-155">Oui</span><span class="sxs-lookup"><span data-stu-id="f7745-155">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="f7745-156">Les contraintes de routage qui vérifient que l’URL peut être convertie en type CLR (comme `int` ou `DateTime`) utilisent toujours la culture invariant.</span><span class="sxs-lookup"><span data-stu-id="f7745-156">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="f7745-157">ces contraintes partent du principe que l’URL n’est pas localisable.</span><span class="sxs-lookup"><span data-stu-id="f7745-157">These constraints assume that the URL is non-localizable.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="f7745-158">Composant de NavLink</span><span class="sxs-lookup"><span data-stu-id="f7745-158">NavLink component</span></span>

<span data-ttu-id="f7745-159">Utiliser un composant NavLink à la place de HTML `<a>` éléments lors de la création de liens de navigation.</span><span class="sxs-lookup"><span data-stu-id="f7745-159">Use a NavLink component in place of HTML `<a>` elements when creating navigation links.</span></span> <span data-ttu-id="f7745-160">Un composant NavLink se comporte comme un `<a>` élément, à ceci près qu’il active ou désactive un `active` classe CSS en fonction de son `href` correspond à l’URL actuelle.</span><span class="sxs-lookup"><span data-stu-id="f7745-160">A NavLink component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="f7745-161">Le `active` classe permet à un utilisateur de comprendre quelle page est la page active parmi les liens de navigation affichées.</span><span class="sxs-lookup"><span data-stu-id="f7745-161">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="f7745-162">Le composant NavMenu suivant crée un [Bootstrap](https://getbootstrap.com/docs/) barre de navigation qui montre comment utiliser les composants NavLink :</span><span class="sxs-lookup"><span data-stu-id="f7745-162">The following NavMenu component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use NavLink components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Shared/NavMenu.cshtml?name=snippet_NavLinks&highlight=4-6,9-11)]

<span data-ttu-id="f7745-163">Il existe deux `NavLinkMatch` options :</span><span class="sxs-lookup"><span data-stu-id="f7745-163">There are two `NavLinkMatch` options:</span></span>

* `NavLinkMatch.All` <span data-ttu-id="f7745-164">&ndash; Spécifie que le NavLink doit être actif lorsqu’il correspond à l’URL entière en cours.</span><span class="sxs-lookup"><span data-stu-id="f7745-164">&ndash; Specifies that the NavLink should be active when it matches the entire current URL.</span></span>
* `NavLinkMatch.Prefix` <span data-ttu-id="f7745-165">&ndash; Spécifie que le NavLink doit être actif lorsqu’il correspond à n’importe quel préfixe de l’URL actuelle.</span><span class="sxs-lookup"><span data-stu-id="f7745-165">&ndash; Specifies that the NavLink should be active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="f7745-166">Dans l’exemple précédent, le NavLink accueil (`href=""`) correspond à toutes les URL et reçoit toujours la `active` classe CSS.</span><span class="sxs-lookup"><span data-stu-id="f7745-166">In the preceding example, the Home NavLink (`href=""`) matches all URLs and always receives the `active` CSS class.</span></span> <span data-ttu-id="f7745-167">Le deuxième NavLink reçoit uniquement le `active` classe lorsque l’utilisateur visite le composant de routage de Blazor (`href="BlazorRoute"`).</span><span class="sxs-lookup"><span data-stu-id="f7745-167">The second NavLink only receives the `active` class when the user visits the Blazor Route component (`href="BlazorRoute"`).</span></span>
