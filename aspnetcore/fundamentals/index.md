---
title: Notions de base d’ASP.NET Core
author: rick-anderson
description: Découvrez les concepts de base permettant de créer des applications ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: fundamentals/index
ms.openlocfilehash: a6c848987c97103864fd5410922346e85a68c353
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67856236"
---
# <a name="aspnet-core-fundamentals"></a>Notions de base d’ASP.NET Core

Cet article est une vue d’ensemble des sujets clés pour comprendre comment développer des applications ASP.NET Core.

## <a name="the-startup-class"></a>Classe Startup

La classe `Startup` est l’endroit où :

* Les services nécessaires à l’application sont configurés.
* Le pipeline de traitement des requêtes est défini.

Les *services* sont des composants utilisés par l’application. Par exemple, un composant de journalisation est un service. Le code pour configurer (ou *enregistrer*) des services est ajouté à la méthode `Startup.ConfigureServices`.

Le pipeline de traitement des requêtes est composé d’une série de composants d’*intergiciel (middleware)* . Par exemple, un intergiciel (middleware) peut gérer les demandes de fichiers statiques ou rediriger les requêtes HTTP vers HTTPS. Chaque intergiciel (middleware) effectue des opérations asynchrones sur `HttpContext`, puis appelle l’intergiciel (middleware) suivant dans le pipeline ou met fin à la requête. Le code pour configurer le pipeline de traitement des requêtes est ajouté à la méthode `Startup.Configure`.

Voici un exemple de classe `Startup` :

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

Pour plus d’informations, consultez <xref:fundamentals/startup>.

## <a name="dependency-injection-services"></a>Injection de dépendances (services)

ASP.NET Core offre une infrastructure d’injection de dépendances (DI) intégrée qui rend disponibles les services configurés aux classes d’une application. Pour obtenir une instance d’un service dans une classe, vous pouvez créer un constructeur avec un paramètre du type requis. Le paramètre peut être le type de service ou une interface. Le système d’injection de dépendances fournit le service lors de l’exécution.

Voici une classe qui utilise l’injection de dépendances pour obtenir un objet de contexte Entity Framework Core. La ligne en surbrillance est un exemple d’injection de constructeur :

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

Si l’injection de dépendances est intégrée, elle est conçue pour vous permettre d’incorporer un conteneur d’inversion de contrôle (IoC) tiers si vous préférez.

Pour plus d’informations, consultez <xref:fundamentals/dependency-injection>.

## <a name="middleware"></a>Intergiciel (middleware)

Le pipeline de traitement des requêtes est composé comme une série de composants d’intergiciel (middleware). Chaque composant effectue des opérations asynchrones sur `HttpContext`, puis appelle l’intergiciel (middleware) suivant dans le pipeline ou met fin à la requête.

Par convention, un composant d’intergiciel (middleware) est ajouté au pipeline en appelant sa méthode d’extension `Use...` dans la méthode `Startup.Configure`. Par exemple, pour activer le rendu des fichiers statiques, appelez `UseStaticFiles`.

Le code en surbrillance dans l’exemple suivant configure le pipeline de traitement des requêtes :

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

ASP.NET Core inclut un ensemble complet de middlewares intégrés, et vous pouvez écrire un middleware personnalisé.

Pour plus d’informations, consultez <xref:fundamentals/middleware/index>.

## <a name="host"></a>Hôte

Une application ASP.NET Core génère un *hôte* au démarrage. L’hôte est un objet qui encapsule toutes les ressources de l’application, telles que :

* Une implémentation serveur HTTP
* Composants d’intergiciel (middleware)
* Journalisation
* INJECTION DE DÉPENDANCES
* Configuration

La principale raison d’inclure toutes les ressources interdépendantes de l’application dans un objet est la gestion de la durée de vie : contrôler le démarrage de l’application et l’arrêt approprié.

::: moniker range=">= aspnetcore-3.0"

Deux hôtes sont disponibles : l’hôte générique et l’hôte web. L’hôte générique est recommandé. L’hôte web est disponible uniquement pour des raisons de compatibilité descendante.

Le code permettant de créer un hôte se trouve dans `Program.Main` :

[!code-csharp[](index/snapshots/3.x/Program1.cs)]

Les méthodes `CreateDefaultBuilder` et `ConfigureWebHostDefaults` permettent de configurer un hôte avec les options fréquemment utilisées, notamment :

* Utilisez [Kestrel](#servers) en tant que serveur web et activez l’intégration IIS.
* Chargez la configuration à partir de *appsettings.json*, *appsettings.{Environment Name}.json*, des variables d’environnement, des arguments de ligne de commande et d’autres sources de configuration.
* Envoyez la sortie de journalisation aux fournisseurs Console et Debug.

Pour plus d’informations, consultez <xref:fundamentals/host/generic-host>.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Deux hôtes sont disponibles : l’hôte web et l’hôte générique. En ASP.NET Core 2.x, l’hôte générique est destiné uniquement aux scénarios non basés sur le web.

Le code permettant de créer un hôte se trouve dans `Program.Main` :

[!code-csharp[](index/snapshots/2.x/Program1.cs)]

La méthode `CreateDefaultBuilder` permet de configurer un hôte avec les options fréquemment utilisées, notamment :

* Utilisez [Kestrel](#servers) en tant que serveur web et activez l’intégration IIS.
* Chargez la configuration à partir de *appsettings.json*, *appsettings.{Environment Name}.json*, des variables d’environnement, des arguments de ligne de commande et d’autres sources de configuration.
* Envoyez la sortie de journalisation aux fournisseurs Console et Debug.

Pour plus d’informations, consultez <xref:fundamentals/host/web-host>.

::: moniker-end

### <a name="non-web-scenarios"></a>Scénarios non basés sur le web

L’hôte générique permet à d’autres types d’application d’utiliser des extensions de framework composites, par exemple la journalisation, l’injection de dépendance, la configuration et la gestion de la durée de vie de l’application. Pour plus d’informations, consultez <xref:fundamentals/host/generic-host> et <xref:fundamentals/host/hosted-services>.

## <a name="servers"></a>Serveurs

Une application ASP.NET Core utilise une implémentation de serveur HTTP pour écouter les requêtes HTTP. Les surfaces de serveur envoient des requêtes à l’application comme un ensemble de [fonctionnalités de requête](xref:fundamentals/request-features) composées dans un `HttpContext`.

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[Fenêtres](#tab/windows)

ASP.NET Core fournit les implémentations de serveur suivantes :

* *Kestrel* est un serveur web multiplateforme. Kestrel est souvent exécuté dans une configuration de proxy inverse à l’aide d’[IIS](https://www.iis.net/). Dans ASP.NET Core 2.0 ou versions ultérieures, Kestrel peut être exécuté en tant que serveur de périphérie public exposé directement à Internet.
* *Le serveur HTTP IIS* est un serveur pour Windows qui utilise IIS. Avec ce serveur, l’application ASP.NET Core et IIS s’exécutent dans le même processus.
* *HTTP.sys* est un serveur Windows qui n’est pas utilisé avec IIS.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

ASP.NET Core fournit l’implémentation du serveur multiplateforme *Kestrel*. Dans ASP.NET Core 2.0 ou versions ultérieures, Kestrel peut être exécuté en tant que serveur de périphérie public exposé directement à Internet. Kestrel est souvent exécuté dans une configuration de proxy inverse avec [Nginx](https://nginx.org) ou [Apache](https://httpd.apache.org/).

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

ASP.NET Core fournit l’implémentation du serveur multiplateforme *Kestrel*. Dans ASP.NET Core 2.0 ou versions ultérieures, Kestrel peut être exécuté en tant que serveur de périphérie public exposé directement à Internet. Kestrel est souvent exécuté dans une configuration de proxy inverse avec [Nginx](https://nginx.org) ou [Apache](https://httpd.apache.org/).

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[Fenêtres](#tab/windows)

ASP.NET Core fournit les implémentations de serveur suivantes :

* *Kestrel* est un serveur web multiplateforme. Kestrel est souvent exécuté dans une configuration de proxy inverse à l’aide d’[IIS](https://www.iis.net/). Dans ASP.NET Core 2.0 ou versions ultérieures, Kestrel peut être exécuté en tant que serveur de périphérie public exposé directement à Internet.
* *HTTP.sys* est un serveur Windows qui n’est pas utilisé avec IIS.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

ASP.NET Core fournit l’implémentation du serveur multiplateforme *Kestrel*. Dans ASP.NET Core 2.0 ou versions ultérieures, Kestrel peut être exécuté en tant que serveur de périphérie public exposé directement à Internet. Kestrel est souvent exécuté dans une configuration de proxy inverse avec [Nginx](https://nginx.org) ou [Apache](https://httpd.apache.org/).

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

ASP.NET Core fournit l’implémentation du serveur multiplateforme *Kestrel*. Dans ASP.NET Core 2.0 ou versions ultérieures, Kestrel peut être exécuté en tant que serveur de périphérie public exposé directement à Internet. Kestrel est souvent exécuté dans une configuration de proxy inverse avec [Nginx](https://nginx.org) ou [Apache](https://httpd.apache.org/).

---

::: moniker-end

Pour plus d’informations, consultez <xref:fundamentals/servers/index>.

## <a name="configuration"></a>Configuration

ASP.NET Core fournit une infrastructure de configuration qui obtient des paramètres en tant que paires nom-valeur à partir d’un ensemble ordonné de fournisseurs de configuration. Il existe des fournisseurs de configuration intégrés pour une grande variété de sources, tels que des fichiers *.json*, *.xml*, des variables d’environnement et des arguments de ligne de commande. Vous pouvez également écrire des fournisseurs de configuration.

Par exemple, vous pouvez spécifier que la configuration provient de *appsettings.json* et de variables d’environnement. Lorsque la valeur de *ConnectionString* est demandée, l’infrastructure recherche d’abord dans le fichier *appsettings.json*. Si la valeur est trouvée dans ce fichier, mais également dans une variable d’environnement, la valeur de la variable d’environnement est prioritaire.

Pour gérer des données de configuration confidentielles telles que les mots de passe, ASP.NET Core fournit un [outil Secret Manager](xref:security/app-secrets). Pour les secrets de production, nous vous recommandons [Azure Key Vault](xref:security/key-vault-configuration).

Pour plus d’informations, consultez <xref:fundamentals/configuration/index>.

## <a name="options"></a>Options

Lorsque cela est possible, ASP.NET Core suit le *modèle d’options* pour stocker et récupérer des valeurs de configuration. Le modèle d’options utilise des classes pour représenter les groupes de paramètres associés.

Par exemple, le code suivant définit des options WebSockets :

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

Pour plus d’informations, consultez <xref:fundamentals/configuration/options>.

## <a name="environments"></a>Environnements

Les environnements d’exécution, tels que *Développement*, *Mise en lots* et *Production*, sont une notion de premier plan dans ASP.NET Core. Vous pouvez spécifier l’environnement d’exécution d’une application en définissant la variable d’environnement `ASPNETCORE_ENVIRONMENT`. ASP.NET Core lit la variable d’environnement au démarrage de l’application et stocke la valeur dans une implémentation `IHostingEnvironment`. L’objet d’environnement est disponible partout dans l’application par le biais de l’injection de dépendances.

L’exemple de code suivant de la classe `Startup` configure l’application pour fournir des informations d’erreur détaillées uniquement lorsqu’elle s’exécute en développement :

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

Pour plus d’informations, consultez <xref:fundamentals/environments>.

## <a name="logging"></a>Journalisation

ASP.NET Core prend en charge une API de journalisation qui fonctionne avec différents fournisseurs de journalisation tiers et intégrés. Les fournisseurs disponibles sont les suivants :

* Console
* Débogage
* Suivi des événements sur Windows
* Journal des événements Windows
* TraceSource
* Azure App Service
* Azure Application Insights

Écrivez des journaux à partir de n’importe quel emplacement dans le code d’une application en obtenant un objet `ILogger` à partir de l’injection de dépendances et en appelant les méthodes de journal.

Voici un exemple de code qui utilise un objet `ILogger`, avec l’injection de constructeur et les appels de méthode de journalisation mis en surbrillance.

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

L’interface `ILogger` vous permet de passer un certain nombre de champs au fournisseur de journalisation. Les champs sont couramment utilisés pour construire une chaîne de message, mais le fournisseur peut également les envoyer en tant que champs distincts dans un magasin de données. Cette fonctionnalité permet aux fournisseurs de journalisation d’implémenter la [journalisation sémantique, également appelée journalisation structurée](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).

Pour plus d’informations, consultez <xref:fundamentals/logging/index>.

## <a name="routing"></a>Routage

Un *itinéraire* est un modèle d’URL qui est mappé à un gestionnaire. Le gestionnaire est généralement une page Razor, une méthode d’action dans un contrôleur MVC, ou un intergiciel (middleware). Le routage ASP.NET Core vous permet de contrôler les URL utilisées par votre application.

Pour plus d’informations, consultez <xref:fundamentals/routing>.

## <a name="error-handling"></a>Gestion des erreurs

ASP.NET Core offre des fonctionnalités intégrées pour gérer des erreurs, telles que :

* Une page d’exceptions du développeur
* Pages d’erreurs personnalisées
* Pages de codes d’état statique
* Gestion des exceptions de démarrage

Pour plus d’informations, consultez <xref:fundamentals/error-handling>.

## <a name="make-http-requests"></a>Effectuer des requêtes HTTP

Une implémentation de `IHttpClientFactory` est disponible pour la création d’instances `HttpClient`. La fabrique :

* Fournit un emplacement central pour le nommage et la configuration d’instance de `HttpClient` logiques. Par exemple, un client *github* peut être inscrit et configuré pour accéder à GitHub. Un client par défaut peut être inscrit à d’autres fins.
* Prend en charge l’inscription et le chaînage de plusieurs gestionnaires de délégation pour créer un pipeline de middlewares pour les requêtes sortantes. Ce modèle est similaire au pipeline de middlewares entrants dans ASP.NET Core. Le modèle fournit un mécanisme permettant de gérer les problèmes transversaux liés aux des requêtes HTTP, notamment la mise en cache, la gestion des erreurs, la sérialisation et la journalisation.
* S’intègre à *Polly*, une bibliothèque tierce populaire pour la gestion des erreurs temporaires.
* Gère le regroupement et la durée de vie des instances de `HttpClientMessageHandler` sous-jacentes pour éviter les problèmes DNS courants qui se produisent lors de la gestion manuelle des durées de vie de `HttpClient`.
* Ajoute une expérience de journalisation configurable (via `ILogger`) pour toutes les requêtes envoyées via des clients créés par la fabrique.

Pour plus d’informations, consultez <xref:fundamentals/http-requests>.

## <a name="content-root"></a>Racine de contenu

La racine de contenu est le chemin de base de tout contenu privé utilisé par l’application, comme les fichiers Razor. Par défaut, la racine de contenu est le chemin de base pour l’exécutable qui héberge l’application. Un autre emplacement peut être spécifié lors de la [création de l’hôte](#host).

::: moniker range=">= aspnetcore-3.0"

Pour plus d’informations, consultez [Racine de contenu](xref:fundamentals/host/generic-host#content-root).

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Pour plus d’informations, consultez [Racine de contenu](xref:fundamentals/host/web-host#content-root).

::: moniker-end

## <a name="web-root"></a>Racine web

La racine web (également appelée *webroot*) est le chemin d’accès de base aux ressources statiques publiques, telles que les fichiers CSS, JavaScript et image. Par défaut, l’intergiciel (middleware) des fichiers statiques traite uniquement les fichiers provenant du répertoire racine web (et des sous-répertoires). Le chemin d’accès par défaut de la racine web est *{Content Root}/wwwroot*, mais un autre emplacement peut être spécifié lors de la [création de l’hôte](#host).

Dans les fichiers Razor ( *.cshtml*), la barre oblique tilde `~/` pointe vers la racine web. Les chemins commençant par `~/` sont appelés chemins virtuels.

Pour plus d’informations, consultez <xref:fundamentals/static-files>.
