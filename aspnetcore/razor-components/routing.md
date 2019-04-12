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
# <a name="razor-components-routing"></a>Composants de Razor routage

Par [Luke Latham](https://github.com/guardrex)

Découvrez comment acheminer les demandes dans les applications et sur le composant NavLink.

## <a name="aspnet-core-endpoint-routing-integration"></a>Intégration de routage de point de terminaison ASP.NET Core

Composants de Razor sont intégrés à [routage ASP.NET Core](xref:fundamentals/routing). Une application ASP.NET Core est configurée pour accepter les connexions entrantes pour les composants de Razor interactive avec `MapComponentHub<TComponent>` dans `Startup.Configure`. `MapComponentHub` Spécifie que le composant racine `App` doit être restitué dans un élément DOM correspondant le sélecteur `app`:

```csharp
app.UseRouting(routes =>
{
    routes.MapRazorPages();
    routes.MapComponentHub<App>("app");
});
```

## <a name="route-templates"></a>Modèles de routes

Le `<Router>` composant Active le routage, et un modèle d’itinéraire est fourni pour chaque composant accessible. Le `<Router>` composant apparaît dans le *Components/App.razor* fichier :

```cshtml
<Router AppAssembly="typeof(Program).Assembly" />
```

Quand un *.razor* ou *.cshtml* de fichiers avec un `@page` directive est compilée, la classe générée est donnée un <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> spécifiant le modèle d’itinéraire. Lors de l’exécution, le routeur recherche de classes de composant avec un `RouteAttribute` et restitue le composant a un modèle d’itinéraire qui correspond à l’URL demandée.

Plusieurs modèles d’itinéraire peuvent être appliqués à un composant. Le composant suivant répond aux demandes de `/BlazorRoute` et `/DifferentBlazorRoute`:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?name=snippet_BlazorRoute)]

`<Router>` prend en charge la définition d’un composant de secours pour le rendu lorsqu’un itinéraire demandé n’est pas résolu. Activer ce scénario participer en définissant le `FallbackComponent` paramètre vers le type de la classe de composant de secours.

L’exemple suivant définit un composant défini dans *Pages/MyFallbackRazorComponent.cshtml* en tant que le composant de secours pour un `<Router>`:

```cshtml
<Router ... FallbackComponent="typeof(Pages.MyFallbackRazorComponent)" />
```

> [!IMPORTANT]
> Pour générer des itinéraires correctement, l’application doit inclure un `<base>` balise dans son *wwwroot/index.HTML* fichier avec le chemin de base application spécifié dans le `href` attribut (`<base href="/">`). Pour plus d'informations, consultez <xref:host-and-deploy/razor-components-blazor/blazor#app-base-path>.

## <a name="route-parameters"></a>Paramètres d’itinéraire

Le routeur utilise les paramètres d’itinéraire pour remplir les paramètres correspondants de composant portant le même nom (non respect de la casse) :

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?name=snippet_RouteParameter&highlight=2,7-8)]

Paramètres facultatifs ne sont pas pris en charge encore, par conséquent, deux `@page` directives sont appliquées dans l’exemple ci-dessus. La première permet la navigation vers le composant sans paramètre. La seconde `@page` directive prend le `{text}` paramètre d’itinéraire et affecte la valeur à la `Text` propriété.

## <a name="route-constraints"></a>Contraintes de routage

Une contrainte de route applique la mise en correspondance sur un segment de routage à un composant de type.

Dans l’exemple suivant, l’itinéraire vers le composant utilisateurs correspond uniquement si :

* Un `Id` segment de routage est présent sur l’URL de requête.
* Le `Id` segment est un entier (`int`).

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.cshtml?highlight=1)]

Les contraintes d’itinéraire indiqués dans le tableau suivant sont disponibles. Pour les contraintes d’itinéraire qui correspondent à la culture dite indifférente, consultez l’avertissement le tableau ci-dessous pour plus d’informations.

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

## <a name="navlink-component"></a>Composant de NavLink

Utiliser un composant NavLink à la place de HTML `<a>` éléments lors de la création de liens de navigation. Un composant NavLink se comporte comme un `<a>` élément, à ceci près qu’il active ou désactive un `active` classe CSS en fonction de son `href` correspond à l’URL actuelle. Le `active` classe permet à un utilisateur de comprendre quelle page est la page active parmi les liens de navigation affichées.

Le composant NavMenu suivant crée un [Bootstrap](https://getbootstrap.com/docs/) barre de navigation qui montre comment utiliser les composants NavLink :

[!code-cshtml[](common/samples/3.x/BlazorSample/Shared/NavMenu.cshtml?name=snippet_NavLinks&highlight=4-6,9-11)]

Il existe deux `NavLinkMatch` options :

* `NavLinkMatch.All` &ndash; Spécifie que le NavLink doit être actif lorsqu’il correspond à l’URL entière en cours.
* `NavLinkMatch.Prefix` &ndash; Spécifie que le NavLink doit être actif lorsqu’il correspond à n’importe quel préfixe de l’URL actuelle.

Dans l’exemple précédent, le NavLink accueil (`href=""`) correspond à toutes les URL et reçoit toujours la `active` classe CSS. Le deuxième NavLink reçoit uniquement le `active` classe lorsque l’utilisateur visite le composant de routage de Blazor (`href="BlazorRoute"`).
