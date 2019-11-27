---
title: ASP.NET Core Blazor routage
author: guardrex
description: Découvrez comment acheminer des requêtes dans des applications et sur le composant NavLink.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/23/2019
no-loc:
- Blazor
uid: blazor/routing
ms.openlocfilehash: 2c139db4e44679fbd9f3455a2d2543be0e128765
ms.sourcegitcommit: 918d7000b48a2892750264b852bad9e96a1165a7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/27/2019
ms.locfileid: "74550335"
---
# <a name="aspnet-core-opno-locblazor-routing"></a>ASP.NET Core Blazor routage

Par [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Découvrez comment acheminer les demandes et comment utiliser le composant `NavLink` pour créer des liens de navigation dans les applications Blazor.

## <a name="aspnet-core-endpoint-routing-integration"></a>Intégration du routage du point de terminaison ASP.NET Core

Blazor Server est intégré à [ASP.net Core routage des points de terminaison](xref:fundamentals/routing). Une application ASP.NET Core est configurée pour accepter les connexions entrantes pour les composants interactifs avec `MapBlazorHub` dans `Startup.Configure`:

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

La configuration la plus courante consiste à acheminer toutes les demandes vers une page Razor, qui joue le rôle d’hôte pour la partie côté serveur de l’application Blazor Server. Par Convention, la page *hôte* est généralement nommée *_Host. cshtml*. L’itinéraire spécifié dans le fichier hôte est appelé *itinéraire de secours* , car il fonctionne avec une priorité basse dans la correspondance d’itinéraire. L’itinéraire de secours est pris en compte lorsque les autres itinéraires ne correspondent pas. Cela permet à l’application d’utiliser d’autres contrôleurs et pages sans interférer avec l’application Blazor Server.

## <a name="route-templates"></a>Modèles de routage

Le composant `Router` active le routage vers chaque composant avec un itinéraire spécifié. Le composant `Router` s’affiche dans le fichier *app. Razor* :

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

Lorsqu’un fichier *. Razor* avec une directive `@page` est compilé, la classe générée est fournie une <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> spécifiant le modèle de routage.

Au moment de l’exécution, le composant `RouteView` :

* Reçoit le `RouteData` du `Router` avec les paramètres souhaités.
* Restitue le composant spécifié avec sa disposition (ou une disposition par défaut facultative) à l’aide des paramètres spécifiés.

Vous pouvez éventuellement spécifier un paramètre `DefaultLayout` avec une classe Layout à utiliser pour les composants qui ne spécifient pas de disposition. Les modèles de Blazor par défaut spécifient le composant `MainLayout`. *MainLayout. Razor* se trouve dans le dossier *partagé* du projet de modèle. Pour plus d’informations sur les dispositions, consultez <xref:blazor/layouts>.

Plusieurs modèles de routage peuvent être appliqués à un composant. Le composant suivant répond aux demandes de `/BlazorRoute` et `/DifferentBlazorRoute`:

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> Pour que les URL soient correctement résolues, l’application doit inclure une balise `<base>` dans son fichier *wwwroot/index.html* (Blazor webassembly) ou le fichier *pages/_Host. cshtml* (serveurBlazor) avec le chemin d’accès de base de l’application spécifié dans l’attribut `href` (`<base href="/">`). Pour plus d'informations, consultez <xref:host-and-deploy/blazor/index#app-base-path>.

## <a name="provide-custom-content-when-content-isnt-found"></a>Fournir du contenu personnalisé lorsque le contenu est introuvable

Le composant `Router` permet à l’application de spécifier du contenu personnalisé si le contenu est introuvable pour l’itinéraire demandé.

Dans le fichier *app. Razor* , définissez le contenu personnalisé dans le paramètre de modèle `NotFound` du composant `Router` :

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

Le contenu des balises `<NotFound>` peut inclure des éléments arbitraires, tels que d’autres composants interactifs. Pour appliquer une disposition par défaut au contenu `NotFound`, consultez <xref:blazor/layouts>.

## <a name="route-to-components-from-multiple-assemblies"></a>Acheminer vers des composants à partir de plusieurs assemblys

Utilisez le paramètre `AdditionalAssemblies` pour spécifier des assemblys supplémentaires pour le composant `Router` à prendre en compte lors de la recherche de composants routables. Les assemblys spécifiés sont pris en compte en plus de l’assembly spécifié par `AppAssembly`. Dans l’exemple suivant, `Component1` est un composant routable défini dans une bibliothèque de classes référencée. L’exemple de `AdditionalAssemblies` suivant entraîne la prise en charge du routage pour `Component1`:

```cshtml
<Router
    AppAssembly="typeof(Program).Assembly"
    AdditionalAssemblies="new[] { typeof(Component1).Assembly }">
    ...
</Router>
```

## <a name="route-parameters"></a>Paramètres d’itinéraire

Le routeur utilise des paramètres de routage pour remplir les paramètres de composant correspondants avec le même nom (sans respect de la casse) :

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

Les paramètres facultatifs ne sont pas pris en charge pour les applications Blazor dans ASP.NET Core 3,0. Deux directives `@page` sont appliquées dans l’exemple précédent. La première permet de naviguer jusqu’au composant sans paramètre. La deuxième `@page` directive prend le paramètre d’itinéraire `{text}` et assigne la valeur à la propriété `Text`.

## <a name="route-constraints"></a>Contraintes d’itinéraire

Une contrainte d’itinéraire applique la correspondance de type sur un segment de routage à un composant.

Dans l’exemple suivant, l’itinéraire vers le composant `Users` correspond uniquement si :

* Un segment de route `Id` est présent sur l’URL de la demande.
* Le segment `Id` est un entier (`int`).

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

Les contraintes de routage indiquées dans le tableau suivant sont disponibles. Pour plus d’informations sur les contraintes d’itinéraire qui correspondent à la culture dite indifférente, consultez l’avertissement sous le tableau.

| Contrainte | Exemple           | Exemples de correspondances                                                                  | Invariant<br>culture<br>correspondance |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | `true`, `FALSE`                                                                  | Non                               |
| `datetime` | `{dob:datetime}`  | `2016-12-31`, `2016-12-31 7:32pm`                                                | Oui                              |
| `decimal`  | `{price:decimal}` | `49.99`, `-1,000.01`                                                             | Oui                              |
| `double`   | `{weight:double}` | `1.234`, `-1,001.01e8`                                                           | Oui                              |
| `float`    | `{weight:float}`  | `1.234`, `-1,001.01e8`                                                           | Oui                              |
| `guid`     | `{id:guid}`       | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Non                               |
| `int`      | `{id:int}`        | `123456789`, `-123456789`                                                        | Oui                              |
| `long`     | `{ticks:long}`    | `123456789`, `-123456789`                                                        | Oui                              |

> [!WARNING]
> Les contraintes de routage qui vérifient que l’URL peut être convertie en type CLR (comme `int` ou `DateTime`) utilisent toujours la culture invariant. ces contraintes partent du principe que l’URL n’est pas localisable.

### <a name="routing-with-urls-that-contain-dots"></a>Routage avec des URL qui contiennent des points

Dans Blazor applications serveur, l’itinéraire par défaut dans *_Host. cshtml* est `/` (`@page "/"`). Une URL de demande qui contient un point (`.`) ne correspond pas à l’itinéraire par défaut, car l’URL semble demander un fichier. Une application Blazor retourne une réponse *404-introuvable* pour un fichier statique qui n’existe pas. Pour utiliser des itinéraires qui contiennent un point, configurez *_Host. cshtml* avec le modèle d’itinéraire suivant :

```cshtml
@page "/{**path}"
```

Le modèle `"/{**path}"` comprend les éléments suivants :

* Double *-astérisque (* `**`) pour capturer le chemin d’accès dans plusieurs limites de dossiers sans encodage des barres obliques (`/`).
* `path` nom du paramètre d’itinéraire.

> [!NOTE]
> La syntaxe de paramètre *catch-all* (`*`/`**`) n’est **pas** prise en charge dans les composants Razor ( *. Razor*).

Pour plus d'informations, consultez <xref:fundamentals/routing>.

## <a name="navlink-component"></a>Composant NavLink

Utilisez un composant `NavLink` à la place des éléments Hyperlink HTML (`<a>`) lors de la création de liens de navigation. Un composant `NavLink` se comporte comme un élément `<a>`, sauf qu’il bascule une classe CSS `active` selon que son `href` correspond à l’URL actuelle. La classe `active` permet à l’utilisateur de comprendre quelle page est la page active parmi les liens de navigation affichés.

Le composant `NavMenu` suivant crée une barre de navigation de [démarrage](https://getbootstrap.com/docs/) qui montre comment utiliser des composants `NavLink` :

[!code-cshtml[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

Il existe deux options de `NavLinkMatch` que vous pouvez assigner à l’attribut `Match` de l’élément `<NavLink>` :

* `NavLinkMatch.All` &ndash; la `NavLink` est active lorsqu’elle correspond à l’URL actuelle entière.
* `NavLinkMatch.Prefix` (*valeur par défaut*) &ndash; le `NavLink` est actif lorsqu’il correspond à n’importe quel préfixe de l’URL actuelle.

Dans l’exemple précédent, le `href=""` de `NavLink` d’hébergement correspond à l’URL d’hébergement et reçoit uniquement la classe CSS `active` à l’URL du chemin de base par défaut de l’application (par exemple, `https://localhost:5001/`). La deuxième `NavLink` reçoit la classe `active` lorsque l’utilisateur visite une URL avec un préfixe de `MyComponent` (par exemple, `https://localhost:5001/MyComponent` et `https://localhost:5001/MyComponent/AnotherSegment`).

Les attributs de composant `NavLink` supplémentaires sont passés à la balise d’ancrage rendue. Dans l’exemple suivant, le composant `NavLink` comprend l’attribut `target` :

```cshtml
<NavLink href="my-page" target="_blank">My page</NavLink>
```

Le balisage HTML suivant est rendu :

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a>Aide des URI et de l’état de navigation

Utilisez `Microsoft.AspNetCore.Components.NavigationManager` pour travailler avec les URI et la C# navigation dans le code. `NavigationManager` fournit l’événement et les méthodes répertoriés dans le tableau suivant.

| Member | Description |
| ------ | ----------- |
| `Uri` | Obtient l’URI absolu actuel. |
| `BaseUri` | Obtient l’URI de base (avec une barre oblique finale) qui peut être ajouté aux chemins d’accès URI relatifs pour produire un URI absolu. En général, `BaseUri` correspond à l’attribut `href` sur l’élément `<base>` du document dans *wwwroot/index.html* (Blazor webassembly) ou *pages/_Host. cshtml* (serveurBlazor). |
| `NavigateTo` | Navigue vers l’URI spécifié. Si `forceLoad` est `true`:<ul><li>Le routage côté client est contourné.</li><li>Le navigateur est obligé de charger la nouvelle page à partir du serveur, que l’URI soit normalement géré ou non par le routeur côté client.</li></ul> |
| `LocationChanged` | Événement qui se déclenche lorsque l’emplacement de navigation a changé. |
| `ToAbsoluteUri` | Convertit un URI relatif en URI absolu. |
| `ToBaseRelativePath` | À partir d’un URI de base (par exemple, un URI précédemment retourné par `GetBaseUri`), convertit un URI absolu en un URI relatif au préfixe URI de base. |

Le composant suivant accède au composant `Counter` de l’application lorsque le bouton est sélectionné :

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
