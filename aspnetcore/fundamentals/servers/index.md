---
title: Implémentations de serveurs web dans ASP.NET Core
author: rick-anderson
description: Découvrez les serveurs web Kestrel et HTTP.sys pour ASP.NET Core. Découvrez comment choisir un serveur et quand utiliser un serveur proxy inverse.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/index
ms.openlocfilehash: c9ed385208df083f631174c7071ca31ed2114350
ms.sourcegitcommit: 1b94305cc79843e2b0866dae811dab61c21980ad
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2018
ms.locfileid: "34473186"
---
# <a name="web-server-implementations-in-aspnet-core"></a>Implémentations de serveurs web dans ASP.NET Core

De [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73) et [Chris Ross](https://github.com/Tratcher)

Une application ASP.NET Core s’exécute avec une implémentation de serveur HTTP in-process. L’implémentation du serveur écoute les requêtes HTTP et les expose à l’application sous forme d’ensembles de [fonctionnalités de requêtes](xref:fundamentals/request-features) composées dans un [HttpContext](/dotnet/api/system.web.httpcontext).

ASP.NET Core est livré avec deux implémentations de serveurs :

* [Kestrel](xref:fundamentals/servers/kestrel) est le serveur HTTP multiplateforme par défaut pour ASP.NET Core.
* [HTTP.sys](xref:fundamentals/servers/httpsys) est un serveur HTTP exclusivement Windows basé sur le [pilote de noyau HTTP.Sys et l’API Serveur HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). (HTTP.sys est appelé [WebListener](xref:fundamentals/servers/weblistener) dans ASP.NET Core 1.x.)

## <a name="kestrel"></a>Kestrel

Kestrel est le serveur web par défaut inclus dans les modèles de projet ASP.NET Core.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Vous pouvez utiliser Kestrel par lui-même ou l’associer à un *serveur proxy inverse*, comme IIS, Nginx ou Apache. Un serveur proxy inverse reçoit les requêtes HTTP en provenance d’Internet et les transmet à Kestrel après un traitement préliminaire.

![Kestrel communique directement avec Internet sans serveur proxy inverse](kestrel/_static/kestrel-to-internet2.png)

![Kestrel communique indirectement avec Internet via un serveur proxy inverse, par exemple IIS, Nginx ou Apache](kestrel/_static/kestrel-to-internet.png)

Les deux configurations&mdash;avec ou sans serveur proxy inverse&mdash;sont des configurations d’hébergement valides et prises en charge pour les applications ASP.NET Core versions 2.0 ou ultérieures. Pour plus d’informations, consultez [Quand utiliser Kestrel avec un proxy inverse ?](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Si l’application accepte seulement les requêtes provenant d’un réseau interne, Kestrel peut être utilisé par lui-même.

![Kestrel communique directement avec le réseau interne](kestrel/_static/kestrel-to-internal.png)

Si l’application est exposée à Internet, Kestrel doit utiliser IIS, Nginx ou Apache comme *serveur proxy inverse*. Un serveur proxy inverse reçoit les requêtes HTTP provenant d’Internet et les transfère à Kestrel après un traitement préliminaire, comme illustré dans le diagramme suivant :

![Kestrel communique indirectement avec Internet via un serveur proxy inverse, par exemple IIS, Nginx ou Apache](kestrel/_static/kestrel-to-internet.png)

La raison la plus importante qui justifie l’utilisation d’un proxy inverse pour les déploiements Edge (exposés au trafic en provenance d’Internet) est la sécurité. Les versions 1.x de Kestrel n’ont pas de fonctionnalités de sécurité suffisantes pour vous défendre contre les attaques provenant d’Internet. Ceci inclut, entre autres choses, des délais d’attente appropriés, des limites de taille des requêtes et des limites du nombre de connexions simultanées.

Pour plus d’informations, consultez [Quand utiliser Kestrel avec un proxy inverse ?](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

---

Vous ne pouvez pas utiliser IIS, Nginx ou Apache sans Kestrel ou sans une [implémentation de serveur personnalisée](#custom-servers). ASP.NET Core a été conçu pour s’exécuter dans son propre processus et se comporter de manière cohérente entre les plateformes. IIS, Nginx et Apache imposent leur propre procédure de démarrage et leur environnement. Pour utiliser ces technologies serveur directement, ASP.NET Core doit s’adapter aux spécifications de chaque serveur. Avec une implémentation de serveur web comme Kestrel, ASP.NET Core peut contrôler le processus de démarrage et l’environnement quand il est hébergé sur des technologies serveur différentes.

### <a name="iis-with-kestrel"></a>IIS avec Kestrel

Quand vous utilisez [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) ou [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) comme proxy inverse pour ASP.NET Core, l’application ASP.NET Core s’exécute dans un processus distinct du processus de travail IIS. Dans le processus IIS, le [module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) coordonne la relation du proxy inverse. Les principales fonctions du module ASP.NET Core consistent à démarrer l’application ASP.NET Core, à la redémarrer quand elle se bloque et à lui transférer le trafic HTTP. Pour plus d’informations, consultez [Module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module). 

### <a name="nginx-with-kestrel"></a>Nginx avec Kestrel

Pour plus d’informations sur l’utilisation de Nginx sur Linux comme serveur proxy inverse pour Kestrel, consultez [Héberger sur Linux avec Nginx](xref:host-and-deploy/linux-nginx).

### <a name="apache-with-kestrel"></a>Apache avec Kestrel

Pour plus d’informations sur l’utilisation d’Apache sur Linux comme serveur proxy inverse pour Kestrel, consultez [Héberger sur Linux avec Apache](xref:host-and-deploy/linux-apache).

## <a name="httpsys"></a>HTTP.sys

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Si vos applications ASP.NET Core sont exécutées sur Windows, HTTP.sys est une alternative à Kestrel. Kestrel est généralement recommandé pour de meilleures performances. HTTP.sys peut être utilisé dans les scénarios où l’application est exposée à Internet et où des fonctionnalités nécessaires sont prises en charge par HTTP.sys, mais pas par Kestrel. Pour plus d’informations sur les fonctionnalités de HTTP.sys, consultez [HTTP.sys](xref:fundamentals/servers/httpsys).

![HTTP.sys communique directement avec Internet](httpsys/_static/httpsys-to-internet.png)

HTTP.sys peut également être utilisé pour les applications qui sont exposées seulement à un réseau interne. 

![HTTP.sys communique directement avec le réseau interne](httpsys/_static/httpsys-to-internal.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

HTTP.sys est nommé [WebListener](xref:fundamentals/servers/weblistener) dans ASP.NET Core 1.x. Si les applications ASP.NET Core sont exécutées sur Windows, WebListener est une alternative pour les scénarios où IIS n’est pas disponible pour héberger les applications.

![WebListener communique directement avec Internet](weblistener/_static/weblistener-to-internet.png)

WebListener peut également être utilisé à la place de Kestrel pour les applications exposées à un réseau interne, si des fonctionnalités nécessaires sont prises en charge par WebListener, mais pas par Kestrel. Pour plus d’informations sur les fonctionnalités de WebListener, consultez [WebListener](xref:fundamentals/servers/weblistener).

![WebListener communique directement avec le réseau interne](weblistener/_static/weblistener-to-internal.png)

---

## <a name="aspnet-core-server-infrastructure"></a>Infrastructure serveur d’ASP.NET Core

Le [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) disponible dans la méthode `Startup.Configure` expose la propriété [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) de type [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection). Kestrel et HTTP.sys (WebListener dans ASP.NET Core 1.x) n’exposent chacun qu’une seule fonctionnalité, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), mais des implémentations serveur différentes peuvent exposer des fonctionnalités supplémentaires.

`IServerAddressesFeature` permet de déterminer quel est le port lié à l’implémentation serveur au moment de l’exécution.

## <a name="custom-servers"></a>Serveurs personnalisés

Si les serveurs intégrés ne répondent pas aux spécifications de l’application, vous pouvez créer une implémentation serveur personnalisée. Le [guide OWIN (Open Web Interface pour .NET)](xref:fundamentals/owin) montre comment écrire une implémentation [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) basée sur [Nowin](https://github.com/Bobris/Nowin). Seules les interfaces des fonctionnalités utilisées par l’application nécessitent une implémentation, bien qu’au minimum, [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) et [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) doivent être pris en charge.

## <a name="server-startup"></a>Démarrage du serveur

Quand vous utilisez [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio pour Mac](https://www.visualstudio.com/vs/mac/) ou [Visual Studio Code](https://code.visualstudio.com/), le serveur est lancé quand l’application est démarrée par l’IDE. Dans Visual Studio sur Windows, des profils de lancement peuvent être utilisés pour démarrer l’application et le serveur avec [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[Module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) ou la console. Dans Visual Studio Code, l’application et le serveur sont démarrés par [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), qui active le débogueur CoreCLR. Avec Visual Studio pour Mac, l’application et le serveur sont démarrés par le [débogueur Mono Soft-Mode](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).

Lors du lancement d’une application à partir d’une invite de commandes dans le dossier du projet, [dotnet run](/dotnet/core/tools/dotnet-run) lance l’application et le serveur (Kestrel et HTTP.sys uniquement). La configuration est spécifiée par l’option `-c|--configuration`, qui est définie sur `Debug` (par défaut) ou sur `Release`. Si les profils de lancement sont présents dans un fichier *launchSettings.json*, utilisez l’option `--launch-profile <NAME>` pour définir le profil de lancement (par exemple `Development` ou `Production`). Pour plus d’informations, consultez les rubriques [dotnet run](/dotnet/core/tools/dotnet-run) et [Empaquetage de la distribution de .NET Core](/dotnet/core/build/distribution-packaging).

## <a name="additional-resources"></a>Ressources supplémentaires

* [Kestrel](xref:fundamentals/servers/kestrel)
* [Kestrel avec IIS](xref:fundamentals/servers/aspnet-core-module)
* [Héberger sur Linux avec Nginx](xref:host-and-deploy/linux-nginx)
* [Héberger sur Linux avec Apache](xref:host-and-deploy/linux-apache)
* [HTTP.sys](xref:fundamentals/servers/httpsys) (pour ASP.NET Core 1.x, consultez [WebListener](xref:fundamentals/servers/weblistener))
