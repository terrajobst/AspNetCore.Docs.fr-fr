---
title: ASP.NET Core Blazor des modèles d’hébergement
author: guardrex
description: Découvrez les modèles d’hébergement Blazor webassembly et Blazor Server.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/18/2020
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-models
ms.openlocfilehash: e6ce2be53c35268854e0e8d408b649a8c6ef497e
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658318"
---
# <a name="aspnet-core-opno-locblazor-hosting-models"></a>ASP.NET Core Blazor des modèles d’hébergement

Par [Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor est un Framework Web conçu pour s’exécuter côté client dans le navigateur sur un Runtime .NET basé sur [Webassembly](https://webassembly.org/)( *Blazor webassembly*) ou côté serveur dans ASP.net Core ( *serveurBlazor* ). Quel que soit le modèle d’hébergement, les modèles d’application et de composant *sont les mêmes*.

Pour créer un projet pour les modèles d’hébergement décrits dans cet article, consultez <xref:blazor/get-started>.

Pour une configuration avancée, consultez <xref:blazor/hosting-model-configuration>.

## <a name="opno-locblazor-webassembly"></a>Blazor webassembly

Le modèle d’hébergement principal pour Blazor s’exécute côté client dans le navigateur sur webassembly. L’application Blazor, ses dépendances et le Runtime .NET sont téléchargés dans le navigateur. L’application est exécutée directement sur le thread d’interface utilisateur du navigateur. Les mises à jour de l’interface utilisateur et la gestion des événements se produisent dans le même processus. Les ressources de l’application sont déployées en tant que fichiers statiques sur un serveur Web ou un service qui prend en charge le contenu statique sur les clients.

![Blazor webassembly : l’application Blazor s’exécute sur un thread d’interface utilisateur dans le navigateur.](hosting-models/_static/blazor-webassembly.png)

Pour créer une application Blazor à l’aide du modèle d’hébergement côté client, utilisez le modèle d' **applicationBlazor Webassembly** ([dotnet New blazorwasm](/dotnet/core/tools/dotnet-new)).

Après avoir sélectionné le modèle d' **applicationBlazor Webassembly** , vous avez la possibilité de configurer l’application pour utiliser un serveur principal ASP.net core en activant la case à cocher **ASP.net Core hébergé** ([dotnet New blazorwasm--Hosted](/dotnet/core/tools/dotnet-new)). L’application ASP.NET Core sert l’application Blazor aux clients. L’application Blazor webassembly peut interagir avec le serveur sur le réseau à l’aide d’appels d’API Web ou de [SignalR](xref:signalr/introduction) (<xref:tutorials/signalr-blazor-webassembly>).

Les modèles incluent le `blazor.webassembly.js` script qui gère :

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

## <a name="opno-locblazor-server"></a>Serveur de Blazor

Avec le modèle d’hébergement Blazor Server, l’application est exécutée sur le serveur à partir d’une application ASP.NET Core. Les mises à jour de l’interface utilisateur, la gestion des événements et les appels JavaScript sont gérés via une connexion [SignalR](xref:signalr/introduction) .

![le navigateur interagit avec l’application (hébergée dans une application ASP.NET Core) sur le serveur via une connexion SignalR.](hosting-models/_static/blazor-server.png)

Pour créer une application Blazor à l’aide du modèle d’hébergement Blazor Server, utilisez le modèle d' **application ASP.NET CoreBlazor Server** ([dotnet New blazorserver](/dotnet/core/tools/dotnet-new)). L’application ASP.NET Core héberge l’application Blazor Server et crée le point de terminaison SignalR où les clients se connectent.

L’application ASP.NET Core fait référence à la classe de `Startup` de l’application à ajouter :

* Services côté serveur.
* L’application vers le pipeline de traitement des demandes.

Le script de `blazor.server.js`&dagger; établit la connexion client. Il est de la responsabilité de l’application de conserver et de restaurer l’état de l’application en fonction des besoins (par exemple, en cas de perte de connexion réseau).

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

&dagger;le script `blazor.server.js` est pris en charge à partir d’une ressource incorporée dans le ASP.NET Core Framework partagé.

### <a name="comparison-to-server-rendered-ui"></a>Comparaison avec l’interface utilisateur du rendu serveur

L’une des façons de comprendre les applications Blazor Server consiste à comprendre la différence entre les modèles traditionnels pour le rendu de l’interface utilisateur dans les applications de ASP.NET Core à l’aide de vues Razor ou Razor Pages. Les deux modèles utilisent le langage Razor pour décrire le contenu HTML, mais ils diffèrent considérablement dans le mode de rendu du balisage.

Quand une page Razor ou une vue est restituée, chaque ligne de code Razor émet du code HTML sous forme de texte. Après le rendu, le serveur supprime l’instance de la page ou de la vue, y compris tout état produit. Lorsqu’une autre demande pour la page se produit, par exemple lorsque la validation du serveur échoue et que le résumé des validations s’affiche :

* La page entière est de nouveau restituée au texte HTML.
* La page est envoyée au client.

Une application Blazor est composée d’éléments réutilisables de l’interface utilisateur appelée *composants*. Un composant contient C# du code, un balisage et d’autres composants. Quand un composant est rendu, Blazor produit un graphique des composants inclus similaires à un Document Object Model XML ou XML (DOM). Ce graphique comprend l’état des composants contenus dans les propriétés et les champs. Blazor évalue le graphique du composant pour produire une représentation binaire de la balise. Le format binaire peut être :

* Converti en texte HTML (lors du prérendu&dagger;).
* Utilisé pour mettre à jour efficacement le balisage pendant le rendu normal.

&dagger;le *prérendu* &ndash; le composant Razor demandé est compilé sur le serveur en code HTML statique et envoyé au client, où il est rendu à l’utilisateur. Une fois la connexion établie entre le client et le serveur, les éléments statiques prérendus du composant sont remplacés par des éléments interactifs. Le prérendu rend l’application plus réactive pour l’utilisateur.

Une mise à jour de l’interface utilisateur dans Blazor est déclenchée par :

* Intervention de l’utilisateur, telle que la sélection d’un bouton.
* Déclencheurs d’application, tels qu’un minuteur.

Le graphique est rerendu et *une différence d’interface utilisateur* (différence) est calculée. Cette différence est le plus petit ensemble de modifications DOM requis pour mettre à jour l’interface utilisateur sur le client. Le diff est envoyé au client dans un format binaire et appliqué par le navigateur.

Un composant est supprimé une fois que l’utilisateur l’a quittée sur le client. Lorsqu’un utilisateur interagit avec un composant, l’état du composant (services, ressources) doit être conservé dans la mémoire du serveur. Étant donné que l’état de nombreux composants peut être géré simultanément par le serveur, l’épuisement de la mémoire est un problème qui doit être résolu. Pour obtenir des conseils sur la création d’une application Blazor Server afin de garantir la meilleure utilisation de la mémoire du serveur, consultez <xref:security/blazor/server>.

### <a name="circuits"></a>Circuits

Une application Blazor Server s’appuie sur [ASP.NET Core SignalR](xref:signalr/introduction). Chaque client communique avec le serveur sur une ou plusieurs connexions SignalR appelées *circuit*. Un circuit est l’abstraction de Blazorsur SignalR connexions qui peuvent tolérer des interruptions réseau temporaires. Lorsqu’un client Blazor constate que la connexion SignalR est déconnectée, il tente de se reconnecter au serveur à l’aide d’une nouvelle connexion SignalR.

Chaque écran de navigateur (onglet ou IFRAME) qui est connecté à une application Blazor Server utilise une connexion SignalR. Il s’agit encore d’une autre distinction importante par rapport aux applications classiques affichées par le serveur. Dans une application affichée sur un serveur, l’ouverture de la même application dans plusieurs écrans de navigateur n’est généralement pas convertie en demandes de ressources supplémentaires sur le serveur. Dans une application Blazor Server, chaque écran du navigateur requiert un circuit distinct et des instances distinctes de l’état du composant à gérer par le serveur.

Blazor envisage de fermer un onglet de navigateur ou de naviguer vers une URL externe un arrêt *normal* . En cas de résiliation appropriée, le circuit et les ressources associées sont immédiatement libérés. Un client peut également se déconnecter de manière non appropriée, par exemple en raison d’une interruption du réseau. Blazor Server stocke les circuits déconnectés pour un intervalle configurable afin de permettre au client de se reconnecter.

Blazor Server permet au code de définir un *Gestionnaire de circuit*qui permet d’exécuter du code sur les modifications de l’état du circuit d’un utilisateur. Pour plus d’informations, consultez <xref:blazor/advanced-scenarios#blazor-server-circuit-handler>.

### <a name="ui-latency"></a>Latence de l’interface utilisateur

La latence de l’interface utilisateur est le temps qu’il faut après une action initiée jusqu’au moment où l’interface utilisateur est mise à jour. Des valeurs plus petites pour la latence de l’interface utilisateur sont impératives pour qu’une application soit réactive à un utilisateur. Dans une application Blazor Server, chaque action est envoyée au serveur, traitée et une comparaison d’interface utilisateur est renvoyée. Par conséquent, la latence de l’interface utilisateur est la somme de la latence du réseau et de la latence du serveur lors du traitement de l’action.

Pour une application métier limitée à un réseau d’entreprise privé, l’effet sur les perceptions des utilisateurs de la latence en raison de la latence du réseau est généralement inperceptible. Pour une application déployée sur Internet, la latence peut devenir perceptible pour les utilisateurs, en particulier si les utilisateurs sont largement répartis géographiquement.

L’utilisation de la mémoire peut également contribuer à la latence de l’application. L’augmentation de l’utilisation de la mémoire permet de garbage collection fréquentes ou la pagination de la mémoire sur le disque, qui dégradent les performances des applications et augmentent la latence de l’interface utilisateur. Pour plus d’informations, consultez <xref:security/blazor/server>.

Blazor applications serveur doivent être optimisées pour réduire la latence de l’interface utilisateur en réduisant la latence du réseau et l’utilisation de la mémoire. Pour une approche de la mesure de la latence du réseau, consultez <xref:host-and-deploy/blazor/server#measure-network-latency>. Pour plus d’informations sur les SignalR et les Blazor, consultez :

* <xref:host-and-deploy/blazor/server>
* <xref:security/blazor/server>

### <a name="connection-to-the-server"></a>Connexion au serveur

Blazor applications serveur requièrent une connexion SignalR active au serveur. Si la connexion est perdue, l’application tente de se reconnecter au serveur. Tant que l’état du client est toujours en mémoire, la session client reprend sans perte d’État.

Une application Blazor Server effectue un prérendu en réponse à la première demande du client, qui configure l’état de l’interface utilisateur sur le serveur. Lorsque le client tente de créer une connexion SignalR, le client doit se reconnecter au même serveur. Blazor applications serveur qui utilisent plusieurs serveurs principaux doivent implémenter des *sessions rémanentes* pour les connexions SignalR.

Nous vous recommandons d’utiliser le [service Azure SignalR](/azure/azure-signalr) pour les applications Blazor Server. Le service permet la mise à l’échelle d’une application Blazor Server vers un grand nombre de connexions SignalR simultanées. Les sessions rémanentes sont activées pour le service Azure SignalR en définissant l’option de `ServerStickyMode` ou la valeur de configuration du service sur `Required`. Pour plus d’informations, consultez <xref:host-and-deploy/blazor/server#signalr-configuration>.

Lorsque vous utilisez IIS, les sessions rémanentes sont activées avec Application Request Routing. Pour plus d’informations, consultez [équilibrage de charge http à l’aide de application Request Routing](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:blazor/get-started>
* <xref:signalr/introduction>
* <xref:blazor/hosting-model-configuration>
* <xref:tutorials/signalr-blazor-webassembly>
