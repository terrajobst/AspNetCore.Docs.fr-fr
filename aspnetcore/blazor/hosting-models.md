---
title: ASP.NET Core Blazor des modèles d’hébergement
author: guardrex
description: Découvrez les modèles d’hébergement Blazor webassembly et Blazor Server.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-models
ms.openlocfilehash: 7676d16bddf146ea38619ed35c5e32c5bce731de
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74943760"
---
# <a name="aspnet-core-opno-locblazor-hosting-models"></a>ASP.NET Core Blazor des modèles d’hébergement

Par [Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor est un Framework Web conçu pour s’exécuter côté client dans le navigateur sur un Runtime .NET basé sur [Webassembly](https://webassembly.org/)( *Blazor webassembly*) ou côté serveur dans ASP.net Core ( *serveurBlazor* ). Quel que soit le modèle d’hébergement, les modèles d’application et de composant *sont les mêmes*.

Pour créer un projet pour les modèles d’hébergement décrits dans cet article, consultez <xref:blazor/get-started>.

## <a name="opno-locblazor-webassembly"></a>Blazor webassembly

Le modèle d’hébergement principal pour Blazor s’exécute côté client dans le navigateur sur webassembly. L’application Blazor, ses dépendances et le Runtime .NET sont téléchargés dans le navigateur. L’application est exécutée directement sur le thread d’interface utilisateur du navigateur. Les mises à jour de l’interface utilisateur et la gestion des événements se produisent dans le même processus. Les ressources de l’application sont déployées en tant que fichiers statiques sur un serveur Web ou un service qui prend en charge le contenu statique sur les clients.

![[! Opérationnel. NO-LOC (éblouissant)] webassembly : [ ! Opérationnel. NO-LOC (éblouissant)] l’application s’exécute sur un thread d’interface utilisateur dans le navigateur.](hosting-models/_static/blazor-webassembly.png)

Pour créer une application Blazor à l’aide du modèle d’hébergement côté client, utilisez le modèle d' **applicationBlazor Webassembly** ([dotnet New blazorwasm](/dotnet/core/tools/dotnet-new)).

Après avoir sélectionné le modèle d' **applicationBlazor Webassembly** , vous avez la possibilité de configurer l’application pour utiliser un serveur principal ASP.net core en activant la case à cocher **ASP.net Core hébergé** ([dotnet New blazorwasm--Hosted](/dotnet/core/tools/dotnet-new)). L’application ASP.NET Core sert l’application Blazor aux clients. L’application Blazor webassembly peut interagir avec le serveur sur le réseau à l’aide d’appels d’API Web ou de [SignalR](xref:signalr/introduction).

Les modèles incluent le script *éblouissant. webassembly. js* qui gère les éléments suivants :

* Téléchargement du Runtime .NET, de l’application et des dépendances de l’application.
* Initialisation du runtime pour exécuter l’application.

Le modèle d’hébergement webassembly Blazor offre plusieurs avantages :

* Il n’existe aucune dépendance côté serveur .NET. L’application fonctionne entièrement après son téléchargement sur le client.
* Les ressources et les fonctionnalités du client sont pleinement exploitées.
* Le travail est déchargé du serveur vers le client.
* Un serveur Web ASP.NET Core n’est pas requis pour héberger l’application. Les scénarios de déploiement sans serveur sont possibles (par exemple, pour servir l’application à partir d’un CDN).

Il existe des inconvénients à Blazor l’hébergement webassembly :

* L’application est limitée aux fonctionnalités du navigateur.
* Le matériel et le logiciel client compatibles (par exemple, la prise en charge de webassembly) sont requis.
* La taille du téléchargement est supérieure, et les applications prennent plus de temps à se charger.
* Le Runtime .NET et la prise en charge des outils sont moins matures. Par exemple, des limitations existent dans la prise en charge de [.NET standard](/dotnet/standard/net-standard) et le débogage.

## <a name="opno-locblazor-server"></a>Serveur Blazor

Avec le modèle d’hébergement Blazor Server, l’application est exécutée sur le serveur à partir d’une application ASP.NET Core. Les mises à jour de l’interface utilisateur, la gestion des événements et les appels JavaScript sont gérés via une connexion [SignalR](xref:signalr/introduction) .

![Le navigateur interagit avec l’application (hébergée à l’intérieur d’une application ASP.NET Core) sur le serveur via un [ ! Opérationnel. Connexion NO-LOC (Signalr)].](hosting-models/_static/blazor-server.png)

Pour créer une application Blazor à l’aide du modèle d’hébergement Blazor Server, utilisez le modèle d' **application ASP.NET CoreBlazor Server** ([dotnet New blazorserver](/dotnet/core/tools/dotnet-new)). L’application ASP.NET Core héberge l’application Blazor Server et crée le point de terminaison SignalR où les clients se connectent.

L’application ASP.NET Core fait référence à la classe de `Startup` de l’application à ajouter :

* Services côté serveur.
* L’application vers le pipeline de traitement des demandes.

Le script *éblouissant. Server. js*&dagger; établit la connexion client. Il est de la responsabilité de l’application de conserver et de restaurer l’état de l’application en fonction des besoins (par exemple, en cas de perte de connexion réseau).

Le modèle d’hébergement Blazor Server offre plusieurs avantages :

* La taille du téléchargement est beaucoup plus petite qu’une application Blazor webassembly, et l’application est chargée beaucoup plus rapidement.
* L’application tire pleinement parti des fonctionnalités du serveur, notamment de l’utilisation de toutes les API compatibles avec .NET Core.
* .NET Core sur le serveur est utilisé pour exécuter l’application, de sorte que les outils .NET existants, tels que le débogage, fonctionnent comme prévu.
* Les clients légers sont pris en charge. Par exemple, les applications Blazor Server fonctionnent avec des navigateurs qui ne prennent pas en charge webassembly et sur des appareils à ressources restreintes.
* Le .NET/C# base de code de l’application, y compris le code du composant de l’application, n’est pas pris en charge par les clients.

Blazor Hébergement de serveur présente des inconvénients :

* Une latence plus élevée existe généralement. Chaque interaction utilisateur implique un tronçon réseau.
* Il n’existe aucune prise en charge hors connexion. Si la connexion cliente échoue, l’application cesse de fonctionner.
* L’évolutivité est difficile pour les applications avec de nombreux utilisateurs. Le serveur doit gérer plusieurs connexions clientes et gérer l’état du client.
* Un serveur de ASP.NET Core est requis pour servir l’application. Les scénarios de déploiement sans serveur ne sont pas possibles (par exemple, pour servir l’application à partir d’un CDN).

&dagger;le script *éblouissant. Server. js* est pris en charge à partir d’une ressource incorporée dans le ASP.net Core Framework partagé.

### <a name="comparison-to-server-rendered-ui"></a>Comparaison avec l’interface utilisateur du rendu serveur

L’une des façons de comprendre les applications Blazor Server consiste à comprendre la différence entre les modèles traditionnels pour le rendu de l’interface utilisateur dans les applications de ASP.NET Core à l’aide de vues Razor ou Razor Pages. Les deux modèles utilisent le langage Razor pour décrire le contenu HTML, mais ils diffèrent considérablement dans le mode de rendu du balisage.

Quand une page Razor ou une vue est restituée, chaque ligne de code Razor émet du code HTML sous forme de texte. Après le rendu, le serveur supprime l’instance de la page ou de la vue, y compris tout état produit. Lorsqu’une autre demande pour la page se produit, par exemple lorsque la validation du serveur échoue et que le résumé des validations s’affiche :

* La page entière est de nouveau restituée au texte HTML.
* La page est envoyée au client.

Une application Blazor est composée d’éléments réutilisables de l’interface utilisateur appelée *composants*. Un composant contient C# du code, un balisage et d’autres composants. Quand un composant est rendu, Blazor produit un graphique des composants inclus similaires à un Document Object Model XML ou XML (DOM). Ce graphique comprend l’état des composants contenus dans les propriétés et les champs. Blazor évalue le graphique du composant pour produire une représentation binaire de la balise. Le format binaire peut être :

* Converti en texte HTML (lors du prérendu).
* Utilisé pour mettre à jour efficacement le balisage pendant le rendu normal.

Une mise à jour de l’interface utilisateur dans Blazor est déclenchée par :

* Intervention de l’utilisateur, telle que la sélection d’un bouton.
* Déclencheurs d’application, tels qu’un minuteur.

Le graphique est rerendu et *une différence d’interface utilisateur* (différence) est calculée. Cette différence est le plus petit ensemble de modifications DOM requis pour mettre à jour l’interface utilisateur sur le client. Le diff est envoyé au client dans un format binaire et appliqué par le navigateur.

Un composant est supprimé une fois que l’utilisateur l’a quittée sur le client. Lorsqu’un utilisateur interagit avec un composant, l’état du composant (services, ressources) doit être conservé dans la mémoire du serveur. Étant donné que l’état de nombreux composants peut être géré simultanément par le serveur, l’épuisement de la mémoire est un problème qui doit être résolu. Pour obtenir des conseils sur la création d’une application Blazor Server afin de garantir la meilleure utilisation de la mémoire du serveur, consultez <xref:security/blazor/server>.

### <a name="circuits"></a>Électriques

Une application Blazor Server s’appuie sur [ASP.NET Core SignalR](xref:signalr/introduction). Chaque client communique avec le serveur sur une ou plusieurs connexions SignalR appelées *circuit*. Un circuit est l’abstraction de Blazorsur SignalR connexions qui peuvent tolérer des interruptions réseau temporaires. Lorsqu’un client Blazor constate que la connexion SignalR est déconnectée, il tente de se reconnecter au serveur à l’aide d’une nouvelle connexion SignalR.

Chaque écran de navigateur (onglet ou IFRAME) qui est connecté à une application Blazor Server utilise une connexion SignalR. Il s’agit encore d’une autre distinction importante par rapport aux applications classiques affichées par le serveur. Dans une application affichée sur un serveur, l’ouverture de la même application dans plusieurs écrans de navigateur n’est généralement pas convertie en demandes de ressources supplémentaires sur le serveur. Dans une application Blazor Server, chaque écran du navigateur requiert un circuit distinct et des instances distinctes de l’état du composant à gérer par le serveur.

Blazor envisage de fermer un onglet de navigateur ou de naviguer vers une URL externe un arrêt *normal* . En cas de résiliation appropriée, le circuit et les ressources associées sont immédiatement libérés. Un client peut également se déconnecter de manière non appropriée, par exemple en raison d’une interruption du réseau. Blazor Server stocke les circuits déconnectés pour un intervalle configurable afin de permettre au client de se reconnecter. Pour plus d’informations, consultez la section [reconnexion au même serveur](#reconnection-to-the-same-server) .

### <a name="ui-latency"></a>Latence de l’interface utilisateur

La latence de l’interface utilisateur est le temps qu’il faut après une action initiée jusqu’au moment où l’interface utilisateur est mise à jour. Des valeurs plus petites pour la latence de l’interface utilisateur sont impératives pour qu’une application soit réactive à un utilisateur. Dans une application Blazor Server, chaque action est envoyée au serveur, traitée et une comparaison d’interface utilisateur est renvoyée. Par conséquent, la latence de l’interface utilisateur est la somme de la latence du réseau et de la latence du serveur lors du traitement de l’action.

Pour une application métier limitée à un réseau d’entreprise privé, l’effet sur les perceptions des utilisateurs de la latence en raison de la latence du réseau est généralement inperceptible. Pour une application déployée sur Internet, la latence peut devenir perceptible pour les utilisateurs, en particulier si les utilisateurs sont largement répartis géographiquement.

L’utilisation de la mémoire peut également contribuer à la latence de l’application. L’augmentation de l’utilisation de la mémoire permet de garbage collection fréquentes ou la pagination de la mémoire sur le disque, qui dégradent les performances des applications et augmentent la latence de l’interface utilisateur. Pour plus d'informations, consultez <xref:security/blazor/server>.

Blazor applications serveur doivent être optimisées pour réduire la latence de l’interface utilisateur en réduisant la latence du réseau et l’utilisation de la mémoire. Pour une approche de la mesure de la latence du réseau, consultez <xref:host-and-deploy/blazor/server#measure-network-latency>. Pour plus d’informations sur les SignalR et les Blazor, consultez :

* <xref:host-and-deploy/blazor/server>
* <xref:security/blazor/server>

### <a name="connection-to-the-server"></a>Connexion au serveur

Blazor applications serveur requièrent une connexion SignalR active au serveur. Si la connexion est perdue, l’application tente de se reconnecter au serveur. Tant que l’état du client est toujours en mémoire, la session client reprend sans perte d’État.

#### <a name="reconnection-to-the-same-server"></a>Reconnexion au même serveur

Une application Blazor Server effectue un prérendu en réponse à la première demande du client, qui configure l’état de l’interface utilisateur sur le serveur. Lorsque le client tente de créer une connexion SignalR, le client doit se reconnecter au même serveur. Blazor applications serveur qui utilisent plusieurs serveurs principaux doivent implémenter des *sessions rémanentes* pour les connexions SignalR.

Nous vous recommandons d’utiliser le [service Azure SignalR](/azure/azure-signalr) pour les applications Blazor Server. Le service permet la mise à l’échelle d’une application Blazor Server vers un grand nombre de connexions SignalR simultanées. Les sessions rémanentes sont activées pour le service Azure SignalR en définissant l’option de `ServerStickyMode` ou la valeur de configuration du service sur `Required`. Pour plus d'informations, consultez <xref:host-and-deploy/blazor/server#signalr-configuration>.

Lorsque vous utilisez IIS, les sessions rémanentes sont activées avec Application Request Routing. Pour plus d’informations, consultez [équilibrage de charge http à l’aide de application Request Routing](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).

#### <a name="reflect-the-connection-state-in-the-ui"></a>Refléter l’état de la connexion dans l’interface utilisateur

Lorsque le client détecte que la connexion a été perdue, une interface utilisateur par défaut est affichée à l’utilisateur pendant que le client tente de se reconnecter. En cas d’échec de la reconnexion, l’utilisateur a la possibilité de réessayer.

Pour personnaliser l’interface utilisateur, définissez un élément avec un `id` de `components-reconnect-modal` dans la `<body>` de la page Razor *_Host. cshtml* :

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

Le tableau suivant décrit les classes CSS appliquées à l’élément `components-reconnect-modal`.

| Classe CSS                       | Indique&hellip; |
| ------------------------------- | ------------------------- |
| `components-reconnect-show`     | Connexion perdue. Le client tente de se reconnecter. Affichez le modal. |
| `components-reconnect-hide`     | Une connexion active est rétablie sur le serveur. Masquez le modal. |
| `components-reconnect-failed`   | Échec de la reconnexion, probablement en raison d’une défaillance du réseau. Pour tenter une reconnexion, appelez `window.Blazor.reconnect()`. |
| `components-reconnect-rejected` | Reconnexion refusée. Le serveur a été atteint mais a refusé la connexion, et l’état de l’utilisateur sur le serveur est perdu. Pour recharger l’application, appelez `location.reload()`. Cet état de connexion peut se produire dans les cas suivants :<ul><li>Un blocage dans le circuit côté serveur se produit.</li><li>Le client est déconnecté suffisamment longtemps pour que le serveur supprime l’état de l’utilisateur. Les instances des composants avec lesquels l’utilisateur interagit sont supprimées.</li><li>Le serveur est redémarré ou le processus de travail de l’application est recyclé.</li></ul> |

### <a name="stateful-reconnection-after-prerendering"></a>Reconnexion avec état après le prérendu

Blazor applications serveur sont configurées par défaut pour prérestituer l’interface utilisateur sur le serveur avant que la connexion cliente au serveur soit établie. Elle est configurée dans la page Razor *_Host. cshtml* :

::: moniker range=">= aspnetcore-3.1"

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

::: moniker-end

::: moniker range="< aspnetcore-3.1"

```cshtml
<body>
    <app>@(await Html.RenderComponentAsync<App>(RenderMode.ServerPrerendered))</app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

::: moniker-end

`RenderMode` configure si le composant :

* Est prérendu dans la page.
* Est rendu en HTML statique sur la page ou s’il contient les informations nécessaires pour démarrer un Blazor application à partir de l’agent utilisateur.

::: moniker range=">= aspnetcore-3.1"

| `RenderMode`        | Description |
| ------------------- | ----------- |
| `ServerPrerendered` | Génère le rendu du composant en HTML statique et comprend un marqueur pour une application Blazor Server. Au démarrage de l’agent utilisateur, ce marqueur est utilisé pour démarrer une application Blazor. |
| `Server`            | Restitue un marqueur pour une application Blazor Server. La sortie du composant n’est pas incluse. Au démarrage de l’agent utilisateur, ce marqueur est utilisé pour démarrer une application Blazor. |
| `Static`            | Génère le rendu du composant en HTML statique. |

::: moniker-end

::: moniker range="< aspnetcore-3.1"

| `RenderMode`        | Description |
| ------------------- | ----------- |
| `ServerPrerendered` | Génère le rendu du composant en HTML statique et comprend un marqueur pour une application Blazor Server. Au démarrage de l’agent utilisateur, ce marqueur est utilisé pour démarrer une application Blazor. Les paramètres ne sont pas pris en charge. |
| `Server`            | Restitue un marqueur pour une application Blazor Server. La sortie du composant n’est pas incluse. Au démarrage de l’agent utilisateur, ce marqueur est utilisé pour démarrer une application Blazor. Les paramètres ne sont pas pris en charge. |
| `Static`            | Génère le rendu du composant en HTML statique. Les paramètres sont pris en charge. |

::: moniker-end

Le rendu des composants serveur à partir d’une page HTML statique n’est pas pris en charge.

Lorsque `RenderMode` est `ServerPrerendered`, le composant est initialement restitué de manière statique dans le cadre de la page. Une fois que le navigateur a établi une connexion au serveur, le composant est *à nouveau*rendu et le composant est maintenant interactif. Si la méthode de cycle de vie [OnInitialized {Async}](xref:blazor/lifecycle#component-initialization-methods) pour initialiser le composant est présente, la méthode est exécutée *deux fois*:

* Lorsque le composant est prérendu statiquement.
* Après l’établissement de la connexion au serveur.

Cela peut entraîner une modification notable des données affichées dans l’interface utilisateur lorsque le composant est finalement restitué.

Pour éviter le double rendu du scénario dans une application Blazor Server :

* Transmettez un identificateur qui peut être utilisé pour mettre en cache l’État pendant le prérendu et pour récupérer l’état après le redémarrage de l’application.
* Utilisez l’identificateur pendant le prérendu pour enregistrer l’état du composant.
* Utilisez l’identificateur après le prérendu pour récupérer l’État mis en cache.

Le code suivant illustre une `WeatherForecastService` mise à jour dans une application de serveur Blazor basée sur un modèle qui évite le double rendu :

```csharp
public class WeatherForecastService
{
    private static readonly string[] Summaries = new[]
    {
        "Freezing", "Bracing", "Chilly", "Cool", "Mild",
        "Warm", "Balmy", "Hot", "Sweltering", "Scorching"
    };
    
    public WeatherForecastService(IMemoryCache memoryCache)
    {
        MemoryCache = memoryCache;
    }
    
    public IMemoryCache MemoryCache { get; }

    public Task<WeatherForecast[]> GetForecastAsync(DateTime startDate)
    {
        return MemoryCache.GetOrCreateAsync(startDate, async e =>
        {
            e.SetOptions(new MemoryCacheEntryOptions
            {
                AbsoluteExpirationRelativeToNow = 
                    TimeSpan.FromSeconds(30)
            });

            var rng = new Random();

            await Task.Delay(TimeSpan.FromSeconds(10));

            return Enumerable.Range(1, 5).Select(index => new WeatherForecast
            {
                Date = startDate.AddDays(index),
                TemperatureC = rng.Next(-20, 55),
                Summary = Summaries[rng.Next(Summaries.Length)]
            }).ToArray();
        });
    }
}
```

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a>Restituer des composants interactifs avec état à partir de pages et de vues Razor

Les composants interactifs avec état peuvent être ajoutés à une page ou à une vue Razor.

Lors du rendu de la page ou de la vue :

* Le composant est prérendu avec la page ou la vue.
* L’état initial du composant utilisé pour le prérendu est perdu.
* Un nouvel état de composant est créé lorsque la connexion SignalR est établie.

La page Razor suivante affiche un composant `Counter` :

::: moniker range=">= aspnetcore-3.1"

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.1"

```cshtml
<h1>My Razor Page</h1>

@(await Html.RenderComponentAsync<Counter>(RenderMode.ServerPrerendered))

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

::: moniker-end

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a>Rendre des composants non interactifs à partir de pages et de vues Razor

Dans la page Razor suivante, le composant `Counter` est restitué statiquement avec une valeur initiale spécifiée à l’aide d’un formulaire :

::: moniker range=">= aspnetcore-3.1"

```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialValue" />
    <button type="submit">Set initial value</button>
</form>

<component type="typeof(Counter)" render-mode="Static" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.1"

```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialValue" />
    <button type="submit">Set initial value</button>
</form>

@(await Html.RenderComponentAsync<Counter>(RenderMode.Static, 
    new { InitialValue = InitialValue }))

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

::: moniker-end

Étant donné que `MyComponent` est rendu statiquement, le composant ne peut pas être interactif.

### <a name="detect-when-the-app-is-prerendering"></a>Détecter quand l’application est prérendu

[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a>Configurer le client SignalR pour les applications Blazor Server

Parfois, vous devez configurer le client SignalR utilisé par les applications Blazor Server. Par exemple, vous pouvez configurer la journalisation sur le client SignalR pour diagnostiquer un problème de connexion.

Pour configurer le client SignalR dans le fichier *pages/_Host. cshtml* :

* Ajoutez un attribut `autostart="false"` à la balise `<script>` pour le script *éblouissant. Server. js* .
* Appelez `Blazor.start` et transmettez un objet de configuration qui spécifie le générateur de SignalR.

```html
<script src="_framework/blazor.server.js" autostart="false"></script>
<script>
  Blazor.start({
    configureSignalR: function (builder) {
      builder.configureLogging("information"); // LogLevel.Information
    }
  });
</script>
```

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:blazor/get-started>
* <xref:signalr/introduction>
