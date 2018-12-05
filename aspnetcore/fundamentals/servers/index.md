---
title: Implémentations de serveurs web dans ASP.NET Core
author: guardrex
description: Découvrez les serveurs web Kestrel et HTTP.sys pour ASP.NET Core. Découvrez comment choisir un serveur et quand utiliser un serveur proxy inverse.
ms.author: tdykstra
ms.custom: mvc
ms.date: 12/01/2018
uid: fundamentals/servers/index
ms.openlocfilehash: 965d69dd071ec71d283284d58e6e1a6e78604f90
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861354"
---
# <a name="web-server-implementations-in-aspnet-core"></a>Implémentations de serveurs web dans ASP.NET Core

De [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73) et [Chris Ross](https://github.com/Tratcher)

Une application ASP.NET Core s’exécute avec une implémentation de serveur HTTP in-process. L’implémentation du serveur écoute les requêtes HTTP et les expose à l’application sous forme d’ensembles de [fonctionnalités de requêtes](xref:fundamentals/request-features) composées dans un <xref:Microsoft.AspNetCore.Http.HttpContext>.

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[Fenêtres](#tab/windows)

ASP.NET Core est fourni avec les composants suivants :

* [Serveur Kestrel](xref:fundamentals/servers/kestrel) : serveur HTTP multiplateforme par défaut.
* Serveur HTTP IIS (`IISHttpServer`) : implémentation d’un [serveur in-process IIS](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model) utilisée avec le [module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).
* [Serveur HTTP.sys](xref:fundamentals/servers/httpsys) : serveur HTTP exclusivement Windows basé sur le [pilote de noyau HTTP.Sys et l’API Serveur HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). HTTP.sys est appelé [WebListener](xref:fundamentals/servers/weblistener) dans ASP.NET Core 1.x.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

ASP.NET Core est fourni avec le [serveur Kestrel](xref:fundamentals/servers/kestrel) qui est le serveur HTTP multiplateforme par défaut.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

ASP.NET Core est fourni avec le [serveur Kestrel](xref:fundamentals/servers/kestrel) qui est le serveur HTTP multiplateforme par défaut.

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[Fenêtres](#tab/windows)

ASP.NET Core est fourni avec les composants suivants :

* [Serveur Kestrel](xref:fundamentals/servers/kestrel) : serveur HTTP multiplateforme par défaut.
* [Serveur HTTP.sys](xref:fundamentals/servers/httpsys) : serveur HTTP exclusivement Windows basé sur le [pilote de noyau HTTP.Sys et l’API Serveur HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). HTTP.sys est appelé [WebListener](xref:fundamentals/servers/weblistener) dans ASP.NET Core 1.x.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

ASP.NET Core est fourni avec le [serveur Kestrel](xref:fundamentals/servers/kestrel) qui est le serveur HTTP multiplateforme par défaut.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

ASP.NET Core est fourni avec le [serveur Kestrel](xref:fundamentals/servers/kestrel) qui est le serveur HTTP multiplateforme par défaut.

---

::: moniker-end

## <a name="kestrel"></a>Kestrel

Kestrel est le serveur web par défaut inclus dans les modèles de projet ASP.NET Core.

::: moniker range=">= aspnetcore-2.0"

Kestrel peut être utilisé :

* Par lui même, c’est-à-dire en tant que serveur de périphérie traitant les requêtes en provenance directe d’un réseau, notamment d’Internet.
* Avec un *serveur proxy inverse*, comme [IIS (Internet Information Services)](https://www.iis.net/), [Nginx](http://nginx.org) ou [Apache](https://httpd.apache.org/). Un serveur proxy inverse reçoit les requêtes HTTP en provenance d’Internet et les transmet à Kestrel.

![Kestrel communique directement avec Internet sans serveur proxy inverse](kestrel/_static/kestrel-to-internet2.png)

![Kestrel communique indirectement avec Internet via un serveur proxy inverse, par exemple IIS, Nginx ou Apache](kestrel/_static/kestrel-to-internet.png)

Les deux configurations&mdash;avec ou sans serveur proxy inverse&mdash;sont des configurations d’hébergement valides et prises en charge pour les applications ASP.NET Core versions 2.0 ou ultérieures. Pour plus d’informations, consultez [Quand utiliser Kestrel avec un proxy inverse ?](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Si l’application accepte seulement les requêtes provenant d’un réseau interne, Kestrel peut être utilisé par lui-même.

![Kestrel communique directement avec le réseau interne](kestrel/_static/kestrel-to-internal.png)

Si l’application est exposée à Internet, Kestrel doit utiliser un *serveur proxy inverse* comme[IIS (Internet Information Services)](https://www.iis.net/), [Nginx](http://nginx.org) ou [Apache](https://httpd.apache.org/). Un serveur proxy inverse reçoit les requêtes HTTP en provenance d’Internet et les transmet à Kestrel.

![Kestrel communique indirectement avec Internet via un serveur proxy inverse, par exemple IIS, Nginx ou Apache](kestrel/_static/kestrel-to-internet.png)

La raison la plus importante de l’utilisation d’un proxy inversé pour les déploiements de serveurs de périphérie publics directement exposés à Internet est la sécurité. Les versions 1.x de Kestrel n’incluent pas de fonctionnalités de sécurité suffisantes pour vous défendre contre les attaques provenant d’Internet. Ceci inclut, entre autres choses, des délais d’attente appropriés, des limites de taille des requêtes et des limites du nombre de connexions simultanées.

Pour plus d’informations, consultez [Quand utiliser Kestrel avec un proxy inverse ?](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

::: moniker-end

### <a name="iis-with-kestrel"></a>IIS avec Kestrel

::: moniker range=">= aspnetcore-2.2"

Lorsque vous utilisez [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) ou [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), l'application ASP.NET Core s'exécute soit dans le même processus que le processus Worker IIS (modèle d'hébergement *in-process*), soit dans un processus distinct du processus Worker IIS (modèle *out-of-process*).

Le [module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) est un module IIS natif qui gère les requêtes IIS natives entre le serveur HTTP IIS in-process ou le serveur Kestrel out-of-process. Pour plus d'informations, consultez <xref:fundamentals/servers/aspnet-core-module>.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Quand vous utilisez [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) ou [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) comme proxy inverse pour ASP.NET Core, l’application ASP.NET Core s’exécute dans un processus distinct du processus de travail IIS. Dans le processus IIS, le [module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) coordonne la relation du proxy inverse. Les principales fonctions du module ASP.NET Core consistent à démarrer l’application, à la redémarrer quand elle plante et à lui transférer le trafic HTTP. Pour plus d'informations, consultez <xref:fundamentals/servers/aspnet-core-module>.

::: moniker-end

### <a name="nginx-with-kestrel"></a>Nginx avec Kestrel

Pour plus d’informations sur l’utilisation de Nginx sur Linux comme serveur proxy inverse pour Kestrel, consultez <xref:host-and-deploy/linux-nginx>.

### <a name="apache-with-kestrel"></a>Apache avec Kestrel

Pour plus d’informations sur l’utilisation d’Apache sur Linux comme serveur proxy inverse pour Kestrel, consultez <xref:host-and-deploy/linux-apache>.

## <a name="httpsys"></a>HTTP.sys

::: moniker range=">= aspnetcore-2.0"

Si vos applications ASP.NET Core sont exécutées sur Windows, HTTP.sys est une alternative à Kestrel. Kestrel est généralement recommandé pour de meilleures performances. HTTP.sys peut être utilisé dans les scénarios où l’application est exposée à Internet et où des fonctionnalités requises sont prises en charge par HTTP.sys, mais pas par Kestrel. Pour plus d'informations, consultez <xref:fundamentals/servers/httpsys>.

![HTTP.sys communique directement avec Internet](httpsys/_static/httpsys-to-internet.png)

HTTP.sys peut également être utilisé pour les applications qui sont exposées seulement à un réseau interne.

![HTTP.sys communique directement avec le réseau interne](httpsys/_static/httpsys-to-internal.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

HTTP.sys est nommé [WebListener](xref:fundamentals/servers/weblistener) dans ASP.NET Core 1.x. Si les applications ASP.NET Core sont exécutées sur Windows, WebListener est une alternative pour les scénarios où IIS n’est pas disponible pour héberger les applications.

![WebListener communique directement avec Internet](weblistener/_static/weblistener-to-internet.png)

WebListener peut également être utilisé à la place de Kestrel pour les applications exposées à un réseau interne, si des fonctionnalités requises sont prises en charge par WebListener, mais pas par Kestrel. Pour plus d’informations sur WebListener, consultez [WebListener](xref:fundamentals/servers/weblistener).

![WebListener communique directement avec le réseau interne](weblistener/_static/weblistener-to-internal.png)

::: moniker-end

## <a name="aspnet-core-server-infrastructure"></a>Infrastructure serveur d’ASP.NET Core

Le [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) disponible dans la méthode `Startup.Configure` expose la propriété [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) de type [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection). Kestrel et HTTP.sys (WebListener dans ASP.NET Core 1.x) n’exposent chacun qu’une seule fonctionnalité, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), mais des implémentations serveur différentes peuvent exposer des fonctionnalités supplémentaires.

`IServerAddressesFeature` permet de déterminer quel est le port lié à l’implémentation serveur au moment de l’exécution.

## <a name="custom-servers"></a>Serveurs personnalisés

Si les serveurs intégrés ne répondent pas aux spécifications de l’application, vous pouvez créer une implémentation serveur personnalisée. Le [guide OWIN (Open Web Interface pour .NET)](xref:fundamentals/owin) montre comment écrire une implémentation [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) basée sur [Nowin](https://github.com/Bobris/Nowin). Seules les interfaces des fonctionnalités utilisées par l’application nécessitent une implémentation, bien qu’au minimum, [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) et [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) doivent être pris en charge.

## <a name="server-startup"></a>Démarrage du serveur

Quand vous utilisez [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio pour Mac](https://www.visualstudio.com/vs/mac/) ou [Visual Studio Code](https://code.visualstudio.com/), le serveur est lancé quand l’application est démarrée par l’IDE. Dans Visual Studio sur Windows, des profils de lancement peuvent être utilisés pour démarrer l’application et le serveur avec [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[Module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) ou la console. Dans Visual Studio Code, l’application et le serveur sont démarrés par [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), qui active le débogueur CoreCLR. Avec Visual Studio pour Mac, l’application et le serveur sont démarrés par le [débogueur Mono Soft-Mode](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).

Lors du lancement d’une application à partir d’une invite de commandes dans le dossier du projet, [dotnet run](/dotnet/core/tools/dotnet-run) lance l’application et le serveur (Kestrel et HTTP.sys uniquement). La configuration est spécifiée par l’option `-c|--configuration`, qui est définie sur `Debug` (par défaut) ou sur `Release`. Si les profils de lancement sont présents dans un fichier *launchSettings.json*, utilisez l’option `--launch-profile <NAME>` pour définir le profil de lancement (par exemple `Development` ou `Production`). Pour plus d’informations, consultez les rubriques [dotnet run](/dotnet/core/tools/dotnet-run) et [Empaquetage de la distribution de .NET Core](/dotnet/core/build/distribution-packaging).

## <a name="http2-support"></a>Prise en charge de HTTP/2

[HTTP/2](https://httpwg.org/specs/rfc7540.html) est pris en charge avec ASP.NET Core dans les scénarios de déploiement suivants :

::: moniker range=">= aspnetcore-2.2"

* [Kestrel](xref:fundamentals/servers/kestrel#http2-support)
  * Système d'exploitation
    * Windows Server 2016/Windows 10 ou version ultérieure&dagger;
    * Linux avec OpenSSL 1.0.2 ou version ultérieure (par exemple, Ubuntu 16.04 ou version ultérieure)
    * HTTP/2 sera pris en charge sur macOS dans une prochaine version.
  * Version cible de .Net Framework : .NET Core 2.2 ou version ultérieure
* [HTTP.sys](xref:fundamentals/servers/httpsys#http2-support)
  * Windows Server 2016/Windows 10 ou version ultérieure
  * Version cible de .Net Framework : non applicable aux déploiements HTTP.sys.
* [IIS (in-process)](xref:host-and-deploy/iis/index#http2-support)
  * Windows Server 2016/Windows 10 ou version ultérieure ; IIS 10 ou version ultérieure
  * Version cible de .Net Framework : .NET Core 2.2 ou version ultérieure
* [IIS (out-of-process)](xref:host-and-deploy/iis/index#http2-support)
  * Windows Server 2016/Windows 10 ou version ultérieure ; IIS 10 ou version ultérieure
  * Les connexions au serveur périphérique public utilisent HTTP/2, mais la connexion de proxy inverse à Kestrel utilise HTTP/1.1.
  * Version cible de .Net Framework : non applicable aux déploiements IIS out-of-process.

&dagger;Kestrel propose une prise en charge limitée de HTTP/2 sous Windows Server 2012 R2 et Windows 8.1. La prise en charge est limitée car la liste des suites de chiffrement TLS prises en charge sur ces systèmes d’exploitation est limitée. Un certificat généré à l’aide d’Elliptic Curve Digital Signature algorithme (ECDSA) peut être requis pour sécuriser les connexions TLS.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [HTTP.sys](xref:fundamentals/servers/httpsys#http2-support)
  * Windows Server 2016/Windows 10 ou version ultérieure
  * Version cible de .Net Framework : non applicable aux déploiements HTTP.sys.
* [IIS (out-of-process)](xref:host-and-deploy/iis/index#http2-support)
  * Windows Server 2016/Windows 10 ou version ultérieure ; IIS 10 ou version ultérieure
  * Les connexions au serveur périphérique public utilisent HTTP/2, mais la connexion de proxy inverse à Kestrel utilise HTTP/1.1.
  * Version cible de .Net Framework : non applicable aux déploiements IIS out-of-process.

::: moniker-end

Une connexion HTTP/2 doit utiliser la [négociation du protocole de la couche d’application (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) et TLS 1.2 ou version ultérieure. Pour plus d’informations, consultez les rubriques qui se rapportent à vos scénarios de déploiement de serveur.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:fundamentals/servers/kestrel>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:fundamentals/servers/httpsys> (pour ASP.NET Core 1.x, consultez<xref:fundamentals/servers/weblistener>)
