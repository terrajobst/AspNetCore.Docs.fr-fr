---
title: ASP.NET Core les modèles d’hébergement éblouissants
author: guardrex
description: Comprendre les modèles d’hébergement de l’assembly et du serveur éblouissants.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: blazor/hosting-models
ms.openlocfilehash: be67c129af4f071d10719e0bbf121de761dde9f4
ms.sourcegitcommit: 16cf016035f0c9acf3ff0ad874c56f82e013d415
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73033991"
---
# <a name="aspnet-core-blazor-hosting-models"></a>ASP.NET Core les modèles d’hébergement éblouissants

Par [Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Éblouissant est un Framework Web conçu pour s’exécuter côté client dans le navigateur sur un Runtime .NET basé sur [Webassembly](https://webassembly.org/)(*éblouissant*) ou côté serveur dans ASP.net Core (*serveur éblouissant*). Quel que soit le modèle d’hébergement, les modèles d’application et de composant *sont les mêmes*.

Pour créer un projet pour les modèles d’hébergement décrits dans cet article, consultez <xref:blazor/get-started>.

## <a name="blazor-webassembly"></a>WebAssembly Blazor

Le modèle d’hébergement principal pour éblouissant s’exécute côté client dans le navigateur sur webassembly. L’application Blazor, ses dépendances et le runtime .NET sont téléchargés sur le navigateur. L’application est exécutée directement sur le thread d’interface utilisateur du navigateur. Les mises à jour de l’interface utilisateur et la gestion des événements se produisent dans le même processus. Les ressources de l’application sont déployées en tant que fichiers statiques sur un serveur Web ou un service qui prend en charge le contenu statique sur les clients.

![Webassembly éblouissant : l’application éblouissant s’exécute sur un thread d’interface utilisateur dans le navigateur.](hosting-models/_static/blazor-webassembly.png)

Pour créer une application éblouissant à l’aide du modèle d’hébergement côté client, utilisez le modèle d' **application éblouissante Webassembly** ([dotnet New blazorwasm](/dotnet/core/tools/dotnet-new)).

Après avoir sélectionné le modèle **application éblouissant Webassembly** , vous avez la possibilité de configurer l’application pour utiliser un serveur principal ASP.net core en activant la case à cocher **ASP.net Core hébergée** ([dotnet New blazorwasm--Hosted](/dotnet/core/tools/dotnet-new)). L’application ASP.NET Core sert l’application éblouissant aux clients. L’application éblouissant webassembly peut interagir avec le serveur sur le réseau à l’aide d’appels d’API Web ou [signalr](xref:signalr/introduction).

Les modèles incluent le script *éblouissant. webassembly. js* qui gère les éléments suivants :

* Téléchargement du Runtime .NET, de l’application et des dépendances de l’application.
* Initialisation du runtime pour exécuter l’application.

Le modèle d’hébergement de webassembly éblouissant offre plusieurs avantages :

* Il n’existe aucune dépendance côté serveur .NET. L’application fonctionne entièrement après son téléchargement sur le client.
* Les ressources et les fonctionnalités du client sont pleinement exploitées.
* Le travail est déchargé du serveur vers le client.
* Un serveur Web ASP.NET Core n’est pas requis pour héberger l’application. Les scénarios de déploiement sans serveur sont possibles (par exemple, pour servir l’application à partir d’un CDN).

L’hébergement d’un webassembly éblouissant présente des inconvénients :

* L’application est limitée aux fonctionnalités du navigateur.
* Le matériel et le logiciel client compatibles (par exemple, la prise en charge de webassembly) sont requis.
* La taille du téléchargement est supérieure, et les applications prennent plus de temps à se charger.
* Le Runtime .NET et la prise en charge des outils sont moins matures. Par exemple, des limitations existent dans la prise en charge de [.NET standard](/dotnet/standard/net-standard) et le débogage.

## <a name="blazor-server"></a>Serveur Blazor

Avec le modèle d’hébergement de serveur éblouissant, l’application est exécutée sur le serveur à partir d’une application ASP.NET Core. Les mises à jour de l’interface utilisateur, la gestion des événements et les appels JavaScript sont gérés par le biais d’une connexion [SignalR](xref:signalr/introduction).

![Le navigateur interagit avec l’application (hébergée à l’intérieur d’une application ASP.NET Core) sur le serveur via une connexion Signalr.](hosting-models/_static/blazor-server.png)

Pour créer une application éblouissant à l’aide du modèle d’hébergement de serveur éblouissant, utilisez le modèle d' **application de serveur ASP.net Core éblouissant** ([dotnet New blazorserver](/dotnet/core/tools/dotnet-new)). L’application ASP.NET Core héberge l’application de serveur éblouissante et crée le point de terminaison Signalr où les clients se connectent.

L’application ASP.NET Core fait référence à la classe `Startup` de l’application à ajouter :

* Services côté serveur.
* L’application vers le pipeline de traitement des demandes.

Le script *éblouissant. Server. js*&dagger; établit la connexion client. Il est de la responsabilité de l’application de conserver et de restaurer l’état de l’application en fonction des besoins (par exemple, en cas de perte de connexion réseau).

Le modèle d’hébergement de serveur éblouissant offre plusieurs avantages :

* La taille du téléchargement est beaucoup plus petite qu’une application de webassembly éblouissante, et l’application se charge beaucoup plus rapidement.
* L’application tire pleinement parti des fonctionnalités du serveur, notamment de l’utilisation de toutes les API compatibles avec .NET Core.
* .NET Core sur le serveur est utilisé pour exécuter l’application, de sorte que les outils .NET existants, tels que le débogage, fonctionnent comme prévu.
* Les clients légers sont pris en charge. Par exemple, les applications de serveur éblouissant fonctionnent avec des navigateurs qui ne prennent pas en charge webassembly et sur des appareils avec des ressources restreintes.
* Le .NET/C# base de code de l’application, y compris le code du composant de l’application, n’est pas pris en charge par les clients.

L’hébergement du serveur éblouissant présente des inconvénients :

* Une latence plus élevée existe généralement. Chaque interaction utilisateur implique un tronçon réseau.
* Il n’existe aucune prise en charge hors connexion. Si la connexion cliente échoue, l’application cesse de fonctionner.
* L’évolutivité est difficile pour les applications avec de nombreux utilisateurs. Le serveur doit gérer plusieurs connexions clientes et gérer l’état du client.
* Un serveur de ASP.NET Core est requis pour servir l’application. Les scénarios de déploiement sans serveur ne sont pas possibles (par exemple, pour servir l’application à partir d’un CDN).

&dagger;The *éblouissant. Server. js* script est pris en charge à partir d’une ressource incorporée dans le ASP.net Core Framework partagé.

### <a name="comparison-to-server-rendered-ui"></a>Comparaison avec l’interface utilisateur du rendu serveur

L’une des façons de comprendre les applications serveur éblouissantes consiste à comprendre comment elles diffèrent des modèles traditionnels pour le rendu de l’interface utilisateur dans les applications de ASP.NET Core à l’aide de vues Razor ou de Razor Pages. Les deux modèles utilisent le langage Razor pour décrire le contenu HTML, mais ils diffèrent considérablement dans le mode de rendu du balisage.

Quand une page Razor ou une vue est restituée, chaque ligne de code Razor émet du code HTML sous forme de texte. Après le rendu, le serveur supprime l’instance de la page ou de la vue, y compris tout état produit. Lorsqu’une autre demande pour la page se produit, par exemple lorsque la validation du serveur échoue et que le résumé des validations s’affiche :

* La page entière est de nouveau restituée au texte HTML.
* La page est envoyée au client.

Une application éblouissant est composée d’éléments réutilisables de l’interface utilisateur, appelés *composants*. Un composant contient C# du code, un balisage et d’autres composants. Quand un composant est rendu, éblouissant produit un graphique des composants inclus similaires à un Document Object Model HTML ou XML (DOM). Ce graphique comprend l’état des composants contenus dans les propriétés et les champs. Éblouissant évalue le graphique du composant pour produire une représentation binaire de la balise. Le format binaire peut être :

* Converti en texte HTML (lors du prérendu).
* Utilisé pour mettre à jour efficacement le balisage pendant le rendu normal.

Une mise à jour de l’interface utilisateur dans éblouissant est déclenchée par :

* Intervention de l’utilisateur, telle que la sélection d’un bouton.
* Déclencheurs d’application, tels qu’un minuteur.

Le graphique est rerendu et *une différence d’interface utilisateur* (différence) est calculée. Cette différence est le plus petit ensemble de modifications DOM requis pour mettre à jour l’interface utilisateur sur le client. Le diff est envoyé au client dans un format binaire et appliqué par le navigateur.

Un composant est supprimé une fois que l’utilisateur l’a quittée sur le client. Lorsqu’un utilisateur interagit avec un composant, l’état du composant (services, ressources) doit être conservé dans la mémoire du serveur. Étant donné que l’état de nombreux composants peut être géré simultanément par le serveur, l’épuisement de la mémoire est un problème qui doit être résolu. Pour obtenir des conseils sur la façon de créer une application de serveur éblouissant pour garantir la meilleure utilisation de la mémoire du serveur, consultez <xref:security/blazor/server>.

### <a name="circuits"></a>Électriques

Une application de serveur éblouissant est basée sur [ASP.net Core signalr](xref:signalr/introduction). Chaque client communique avec le serveur sur une ou plusieurs connexions Signalr nommé *circuit*. Un circuit est l’abstraction de l’éblouissant sur les connexions Signalr qui peuvent tolérer des interruptions réseau temporaires. Lorsqu’un client éblouissant voit que la connexion Signalr est déconnectée, il tente de se reconnecter au serveur à l’aide d’une nouvelle connexion Signalr.

Chaque écran de navigateur (onglet de navigateur ou IFRAME) qui est connecté à une application de serveur éblouissante utilise une connexion Signalr. Il s’agit encore d’une autre distinction importante par rapport aux applications classiques affichées par le serveur. Dans une application affichée sur un serveur, l’ouverture de la même application dans plusieurs écrans de navigateur n’est généralement pas convertie en demandes de ressources supplémentaires sur le serveur. Dans une application de serveur éblouissant, chaque écran de navigateur requiert un circuit distinct et des instances distinctes de l’état du composant à gérer par le serveur.

Éblouissant prend en compte la fermeture d’un onglet de navigateur ou la navigation vers une URL externe un arrêt *normal* . En cas de résiliation appropriée, le circuit et les ressources associées sont immédiatement libérés. Un client peut également se déconnecter de manière non appropriée, par exemple en raison d’une interruption du réseau. Le serveur éblouissant stocke les circuits déconnectés pour un intervalle configurable afin de permettre au client de se reconnecter. Pour plus d’informations, consultez la section [reconnexion au même serveur](#reconnection-to-the-same-server) .

### <a name="ui-latency"></a>Latence de l’interface utilisateur

La latence de l’interface utilisateur est le temps qu’il faut après une action initiée jusqu’au moment où l’interface utilisateur est mise à jour. Des valeurs plus petites pour la latence de l’interface utilisateur sont impératives pour qu’une application soit réactive à un utilisateur. Dans une application de serveur éblouissant, chaque action est envoyée au serveur, traitée et une comparaison d’interface utilisateur est renvoyée. Par conséquent, la latence de l’interface utilisateur est la somme de la latence du réseau et de la latence du serveur lors du traitement de l’action.

Pour une application métier limitée à un réseau d’entreprise privé, l’effet sur les perceptions des utilisateurs de la latence en raison de la latence du réseau est généralement inperceptible. Pour une application déployée sur Internet, la latence peut devenir perceptible pour les utilisateurs, en particulier si les utilisateurs sont largement répartis géographiquement.

L’utilisation de la mémoire peut également contribuer à la latence de l’application. L’augmentation de l’utilisation de la mémoire permet de garbage collection fréquentes ou la pagination de la mémoire sur le disque, qui dégradent les performances des applications et augmentent la latence de l’interface utilisateur. Pour plus d'informations, consultez <xref:security/blazor/server>.

Les applications de serveur éblouissantes doivent être optimisées pour réduire la latence de l’interface utilisateur en réduisant la latence du réseau et l’utilisation de la mémoire. Pour une approche de la mesure de la latence du réseau, consultez <xref:host-and-deploy/blazor/server#measure-network-latency>. Pour plus d’informations sur Signalr et éblouissant, consultez :

* <xref:host-and-deploy/blazor/server>
* <xref:security/blazor/server>

### <a name="reconnection-to-the-same-server"></a>Reconnexion au même serveur

Les applications serveur éblouissantes nécessitent une connexion active Signalr au serveur. Si la connexion est perdue, l’application tente de se reconnecter au serveur. Tant que l’état du client est toujours en mémoire, la session client reprend sans perte d’État.

Lorsque le client détecte que la connexion a été perdue, une interface utilisateur par défaut est affichée à l’utilisateur pendant que le client tente de se reconnecter. En cas d’échec de la reconnexion, l’utilisateur a la possibilité de réessayer. Pour personnaliser l’interface utilisateur, définissez un élément avec `components-reconnect-modal` comme `id` dans la page Razor *_Host. cshtml* . Le client met à jour cet élément avec l’une des classes CSS suivantes en fonction de l’état de la connexion :

* `components-reconnect-show` &ndash; affiche l’interface utilisateur pour indiquer qu’une connexion a été perdue et que le client tente de se reconnecter.
* `components-reconnect-hide` &ndash; le client dispose d’une connexion active, masquez l’interface utilisateur.
* `components-reconnect-failed` &ndash; échec de la reconnexion, probablement en raison d’une défaillance du réseau. Pour tenter une reconnexion, appelez `window.Blazor.reconnect()`.
* `components-reconnect-rejected` la reconnexion &ndash; a été rejetée. Le serveur a été atteint mais a refusé la connexion, et l’état de l’utilisateur sur le serveur a disparu. Pour recharger l’application, appelez `location.reload()`. Cet état de connexion peut se produire dans les cas suivants :
  * Un blocage dans le circuit (code côté serveur) se produit.
  * Le client est déconnecté suffisamment longtemps pour que le serveur supprime l’état de l’utilisateur. Les instances de composants avec lesquelles l’utilisateur interagit sont supprimées.

### <a name="stateful-reconnection-after-prerendering"></a>Reconnexion avec état après le prérendu

Les applications serveur éblouissantes sont configurées par défaut pour prérestituer l’interface utilisateur sur le serveur avant que la connexion cliente au serveur soit établie. Elle est configurée dans la page Razor *_Host. cshtml* :

```cshtml
<body>
    <app>@(await Html.RenderComponentAsync<App>(RenderMode.ServerPrerendered))</app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

`RenderMode` configure si le composant :

* Est prérendu dans la page.
* Est rendu en HTML statique sur la page ou s’il contient les informations nécessaires pour démarrer une application éblouissant à partir de l’agent utilisateur.

| `RenderMode`        | Description |
| ------------------- | ----------- |
| `ServerPrerendered` | Génère le rendu du composant en HTML statique et comprend un marqueur pour une application de serveur éblouissante. Au démarrage de l’agent utilisateur, ce marqueur est utilisé pour démarrer une application éblouissante. Les paramètres ne sont pas pris en charge. |
| `Server`            | Restitue un marqueur pour une application de serveur éblouissante. La sortie du composant n’est pas incluse. Au démarrage de l’agent utilisateur, ce marqueur est utilisé pour démarrer une application éblouissante. Les paramètres ne sont pas pris en charge. |
| `Static`            | Génère le rendu du composant en HTML statique. Les paramètres sont pris en charge. |

Le rendu des composants serveur à partir d’une page HTML statique n’est pas pris en charge.

Le client se reconnecte au serveur avec le même État que celui utilisé pour prégénérer l’application. Si l’état de l’application est toujours en mémoire, l’état du composant n’est pas restitué après l’établissement de la connexion Signalr.

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a>Restituer des composants interactifs avec état à partir de pages et de vues Razor

Les composants interactifs avec état peuvent être ajoutés à une page ou à une vue Razor.

Lors du rendu de la page ou de la vue :

* Le composant est prérendu avec la page ou la vue.
* L’état initial du composant utilisé pour le prérendu est perdu.
* Un nouvel état de composant est créé lorsque la connexion Signalr est établie.

La page Razor suivante affiche un composant `Counter` :

```cshtml
<h1>My Razor Page</h1>

@(await Html.RenderComponentAsync<Counter>(RenderMode.ServerPrerendered))
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a>Rendre des composants non interactifs à partir de pages et de vues Razor

Dans la page Razor suivante, le composant `MyComponent` est restitué statiquement avec une valeur initiale spécifiée à l’aide d’un formulaire :

```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialValue" />
    <button type="submit">Set initial value</button>
</form>

@(await Html.RenderComponentAsync<MyComponent>(RenderMode.Static, 
    new { InitialValue = InitialValue }))

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

Étant donné que `MyComponent` est rendu statiquement, le composant ne peut pas être interactif.

### <a name="detect-when-the-app-is-prerendering"></a>Détecter quand l’application est prérendu

[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-signalr-client-for-blazor-server-apps"></a>Configurer le client Signalr pour les applications serveur éblouissantes

Parfois, vous devez configurer le client Signalr utilisé par les applications serveur éblouissantes. Par exemple, vous pouvez configurer la journalisation sur le client Signalr pour diagnostiquer un problème de connexion.

Pour configurer le client Signalr dans le fichier *pages/_Host. cshtml* :

* Ajoutez un attribut `autostart="false"` à la balise `<script>` pour le script *éblouissant. Server. js* .
* Appelez `Blazor.start` et transmettez un objet de configuration qui spécifie le générateur Signalr.

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
