---
title: ASP.NET Core du routage éblouissant
author: guardrex
description: Découvrez comment acheminer des requêtes dans des applications et sur le composant NavLink.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2019
uid: blazor/routing
ms.openlocfilehash: 70cae6b3a21fe3537d6841a6716398a5fc45db62
ms.sourcegitcommit: f30b18442ed12831c7e86b0db249183ccd749f59
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68948309"
---
# <a name="aspnet-core-blazor-routing"></a>ASP.NET Core du routage éblouissant

Par [Luke Latham](https://github.com/guardrex)

Découvrez comment acheminer les demandes et comment utiliser le `NavLink` composant pour créer des liens de navigation dans les applications éblouissantes.

## <a name="aspnet-core-endpoint-routing-integration"></a>Intégration du routage du point de terminaison ASP.NET Core

Le côté serveur éblouissant est intégré à [ASP.net Core routage des points de terminaison](xref:fundamentals/routing). Une application ASP.net Core est configurée pour accepter les connexions entrantes pour `MapBlazorHub` les `Startup.Configure`composants interactifs avec dans:

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

## <a name="route-templates"></a>Modèles de routage

Le `Router` composant active le routage et un modèle de routage est fourni à chaque composant accessible. Le `Router` composant apparaît dans le fichier *app. Razor* :

Dans une application côté serveur éblouissante:

```cshtml
<Router AppAssembly="typeof(Startup).Assembly" />
```

Dans une application éblouissante côté client:

```cshtml
<Router AppAssembly="typeof(Program).Assembly" />
```

Lorsqu’un fichier *. Razor* avec une `@page` directive est compilé, la classe générée est fournie en <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> spécifiant le modèle de routage. Lors de l’exécution, le routeur recherche les classes de `RouteAttribute` composant avec un et restitue le composant avec un modèle de routage qui correspond à l’URL demandée.

Plusieurs modèles de routage peuvent être appliqués à un composant. Le composant suivant répond aux demandes pour `/BlazorRoute` et `/DifferentBlazorRoute`:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> Pour générer correctement des itinéraires, l’application doit inclure `<base>` une balise dans son fichier *wwwroot/index.html* (le client éblouissant) ou le fichier *pages/_Host. cshtml* (le côté serveur éblouissant) avec le chemin d’accès de base de `href` l’application spécifié dans l’attribut ( `<base href="/">`). Pour plus d'informations, consultez <xref:host-and-deploy/blazor/client-side#app-base-path>.

## <a name="provide-custom-content-when-content-isnt-found"></a>Fournir du contenu personnalisé lorsque le contenu est introuvable

Le `Router` composant permet à l’application de spécifier du contenu personnalisé si le contenu est introuvable pour l’itinéraire demandé.

Dans le fichier *app. Razor* , définissez le contenu personnalisé dans `<NotFoundContent>` l’élément du `Router` composant:

```cshtml
<Router AppAssembly="typeof(Startup).Assembly">
    <NotFoundContent>
        <h1>Sorry</h1>
        <p>Sorry, there's nothing at this address.</p> b
    </NotFoundContent>
</Router>
```

Le contenu de `<NotFoundContent>` peut inclure des éléments arbitraires, tels que d’autres composants interactifs.

## <a name="route-parameters"></a>Paramètres d’itinéraire

Le routeur utilise des paramètres de routage pour remplir les paramètres de composant correspondants avec le même nom (sans respect de la casse):

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

Les paramètres facultatifs ne sont pas pris en charge pour les applications éblouissantes dans ASP.NET Core version préliminaire 3,0. Deux `@page` directives sont appliquées dans l’exemple précédent. La première permet de naviguer jusqu’au composant sans paramètre. La deuxième `@page` directive prend le `{text}` paramètre d’itinéraire et assigne la valeur à `Text` la propriété.

## <a name="route-constraints"></a>Contraintes d’itinéraire

Une contrainte d’itinéraire applique la correspondance de type sur un segment de routage à un composant.

Dans l’exemple suivant, l’itinéraire vers le `Users` composant correspond uniquement à si:

* Un `Id` segment de routage est présent sur l’URL de la demande.
* Le `Id` segment est un entier (`int`).

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

## <a name="navlink-component"></a>Composant NavLink

Utilisez un `NavLink` composant à la place des éléments Hyperlink`<a>`html () lors de la création de liens de navigation. Un `NavLink` composant se comporte comme un `<a>` élément, sauf qu’il fait basculer `active` une classe CSS selon que son `href` correspond à l’URL actuelle. La `active` classe permet à un utilisateur de comprendre quelle page est la page active parmi les liens de navigation affichés.

Le composant `NavMenu` suivant crée une barre de navigation de [démarrage](https://getbootstrap.com/docs/) qui montre comment `NavLink` utiliser des composants:

[!code-cshtml[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

Il existe deux `NavLinkMatch` options que vous pouvez assigner `Match` à l’attribut `<NavLink>` de l’élément:

* `NavLinkMatch.All`&ndash; Estactiflorsqu’ilcorrespondàlatotalité`NavLink` de l’URL actuelle.
* `NavLinkMatch.Prefix`(*par défaut*) &ndash; Estactiflorsqu’ilcorrespondàn’importequelpréfixe`NavLink` de l’URL actuelle.

Dans l’exemple précédent, la page `NavLink` d’hébergement `href=""` correspond à l’URL de base `active` et reçoit uniquement la classe CSS à l’URL du chemin de base par `https://localhost:5001/`défaut de l’application (par exemple,). Le deuxième `NavLink` reçoit la `active` classe lorsque l’utilisateur visite une URL avec un `MyComponent` préfixe (par exemple `https://localhost:5001/MyComponent` , `https://localhost:5001/MyComponent/AnotherSegment`et).

## <a name="uri-and-navigation-state-helpers"></a>Aide des URI et de l’état de navigation

Utilisez `Microsoft.AspNetCore.Components.IUriHelper` pour travailler avec les URI et la C# navigation dans le code. `IUriHelper`fournit l’événement et les méthodes répertoriées dans le tableau suivant.

| Membre | Description |
| ------ | ----------- |
| `GetAbsoluteUri` | Obtient l’URI absolu actuel. |
| `GetBaseUri` | Obtient l’URI de base (avec une barre oblique finale) qui peut être ajouté aux chemins d’accès URI relatifs pour produire un URI absolu. En général `GetBaseUri` , correspond à `href` l’attribut sur l’élément `<base>` du document dans *wwwroot/index.html* (éblouissant Client-Side) ou *pages/_Host. cshtml* (le côté serveur de éblouissant). |
| `NavigateTo` | Navigue vers l’URI spécifié. Si `forceLoad` est `true`:<ul><li>Le routage côté client est contourné.</li><li>Le navigateur est obligé de charger la nouvelle page à partir du serveur, que l’URI soit normalement géré ou non par le routeur côté client.</li></ul> |
| `OnLocationChanged` | Événement qui se déclenche lorsque l’emplacement de navigation a changé. |
| `ToAbsoluteUri` | Convertit un URI relatif en URI absolu. |
| `ToBaseRelativePath` | À partir d’un URI de base (par exemple, un URI `GetBaseUri`précédemment retourné par), convertit un URI absolu en un URI relatif au préfixe URI de base. |

Le composant suivant accède au composant de `Counter` l’application lorsque le bouton est sélectionné:

```cshtml
@page "/navigate"
@using Microsoft.AspNetCore.Components
@inject IUriHelper UriHelper

<h1>Navigate in Code Example</h1>

<button class="btn btn-primary" @onclick="NavigateToCounterComponent">
    Navigate to the Counter component
</button>

@code {
    private void NavigateToCounterComponent()
    {
        UriHelper.NavigateTo("counter");
    }
}
```
