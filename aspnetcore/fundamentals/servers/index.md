---
title: Implémentations de serveurs web dans ASP.NET Core
author: guardrex
description: Découvrez les serveurs web Kestrel et HTTP.sys pour ASP.NET Core. Découvrez comment choisir un serveur et quand utiliser un serveur proxy inverse.
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/11/2019
uid: fundamentals/servers/index
ms.openlocfilehash: 4210d67397c85a1608f79fc4ed9d283521356226
ms.sourcegitcommit: ec71fd5a988f927ae301813aae5ff764feb3bb6a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/12/2019
ms.locfileid: "54249488"
---
# <a name="web-server-implementations-in-aspnet-core"></a>Implémentations de serveurs web dans ASP.NET Core

De [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73) et [Chris Ross](https://github.com/Tratcher)

Une application ASP.NET Core s’exécute avec une implémentation de serveur HTTP in-process. L’implémentation du serveur écoute les requêtes HTTP et les expose à l’application sous forme d’ensemble de [fonctionnalités de requêtes](xref:fundamentals/request-features) composées dans un <xref:Microsoft.AspNetCore.Http.HttpContext>.

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[Fenêtres](#tab/windows)

ASP.NET Core est fourni avec les composants suivants :

* [Serveur Kestrel](xref:fundamentals/servers/kestrel) : implémentation de serveur HTTP multiplateforme par défaut.
* Le serveur HTTP IIS est un [serveur in-process](#in-process-hosting-model) pour IIS.
* [Serveur HTTP.sys](xref:fundamentals/servers/httpsys) : serveur HTTP exclusivement Windows basé sur le [pilote de noyau HTTP.Sys et l’API Serveur HTTP](/windows/desktop/Http/http-api-start-page).

Lorsque vous utilisez [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) ou [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), l’application s’exécute :

* dans le même processus que le processus Worker IIS (le [modèle d’hébergement in-process](#in-process-hosting-model)) avec le [serveur HTTP IIS](#iis-http-server). *In-process* est la configuration recommandée.
* Ou dans un processus séparé du processus Worker IIS (le [mode d’hébergement out-of-process](#out-of-process-hosting-model)) avec le [serveur Kestrel](#kestrel).

Le [module ASP.NET Core](xref:host-and-deploy/aspnet-core-module) est un module IIS natif qui gère les requêtes IIS natives entre IIS et le serveur HTTP IIS in-process ou Kestrel. Pour plus d'informations, consultez <xref:host-and-deploy/aspnet-core-module>.

## <a name="hosting-models"></a>Modèles d'hébergement

### <a name="in-process-hosting-model"></a>Modèle d’hébergement in-process

En utilisant l’hébergement in-process, une application ASP.NET Core s’exécute dans le même processus que son processus de travail IIS. Cette opération supprime la baisse des performances out-of-process des requêtes proxy sur l’adaptateur de bouclage, une interface réseau qui renvoie du trafic réseau sortant à la même machine. IIS s’occupe de la gestion des processus par l’intermédiaire du [service d’activation des processus Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

Le module ASP.NET Core :

* Effectue l’initialisation de l’application.
  * Charge le [CoreCLR](/dotnet/standard/glossary#coreclr).
  * Appelle `Program.Main`.
* Gère la durée de vie de la requête native IIS.

Le modèle d’hébergement in-process n’est pas pris en charge pour les applications ASP.NET Core qui ciblent le .NET Framework.

Le schéma suivant montre la relation entre IIS, le module ASP.NET Core et une application hébergée dans un processus :

![Module ASP.NET Core](_static/ancm-inprocess.png)

Une requête arrive du web au pilote HTTP.sys en mode noyau. Le pilote achemine la requête native vers IIS sur le port configuré du site web, généralement 80 (HTTP) ou 443 (HTTPS). Le module reçoit la requête native et la passe à IIS HTTP Server (`IISHttpServer`). Le serveur HTTP IIS est une implémentation du serveur in-process pour IIS qui convertit la requête native en requête managée.

Une fois que IIS HTTP Server a traité la requête, celle-ci est envoyée (push) dans le pipeline de middleware (intergiciel) d’ASP.NET Core. Le pipeline de middlewares traite la requête et la passe en tant qu’instance de `HttpContext` à la logique de l’application. La réponse de l’application est repassée à IIS, qui la renvoie au client à l’origine de la requête.

L’hébergement in-process est au choix pour les applications existantes, mais les modèles [dotnet new](/dotnet/core/tools/dotnet-new) sont définis par défaut sur le modèle d’hébergement in-process pour tous les scénarios IIS et IIS Express.

### <a name="out-of-process-hosting-model"></a>Modèle d’hébergement out-of-process

Étant donné que les applications ASP.NET Core s’exécutent dans un processus distinct du processus de travail IIS, le module s’occupe de la gestion du processus. Le module démarre le processus pour l’application ASP.NET Core quand la première requête arrive, et il redémarre l’application si elle s’arrête ou se bloque. Il s’agit essentiellement du même comportement que celui des applications s’exécutant in-process, et qui sont gérées par le [service d’activation des processus Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

Le schéma suivant illustre la relation entre IIS, le module ASP.NET Core et une application hébergée hors processus :

![Module ASP.NET Core](_static/ancm-outofprocess.png)

Les requêtes arrivent du web au pilote HTTP.sys en mode noyau. Le pilote route les requêtes vers IIS sur le port configuré du site web, généralement 80 (HTTP) ou 443 (HTTPS). Le module transfère les requêtes à Kestrel sur un port aléatoire pour l’application, qui n’est ni le port 80 ni le port 443.

Le module spécifie le port via une variable d’environnement au démarrage, et le middleware d’intégration IIS configure le serveur pour qu’il écoute sur `http://localhost:{PORT}`. Des vérifications supplémentaires sont effectuées, et les requêtes qui ne proviennent pas du module sont rejetées. Le module ne prend pas en charge le transfert HTTPS : les requêtes sont donc transférées via HTTP, même si IIS les reçoit via HTTPS.

Dès que Kestrel sélectionne la requête dans le module, celle-ci est envoyée (push) dans le pipeline de middlewares d’ASP.NET Core. Le pipeline de middlewares traite la requête et la passe en tant qu’instance de `HttpContext` à la logique de l’application. Le middleware ajouté par l’intégration d’IIS met à jour le schéma, l’adresse IP distante et la base du chemin pour prendre en compte le transfert de la requête à Kestrel. La réponse de l’application est ensuite repassée à IIS, qui la renvoie au client HTTP à l’origine de la requête.

Pour des conseils de configuration d’IIS et du module ASP.NET Core, consultez les rubriques suivantes :

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>

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
* [Serveur HTTP.sys](xref:fundamentals/servers/httpsys) : serveur HTTP exclusivement Windows basé sur le [pilote de noyau HTTP.Sys et l’API Serveur HTTP](/windows/desktop/Http/http-api-start-page).

Lorsqu’[IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) ou [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) sont utilisés, l’application s’exécute dans un processus séparé du processus Worker IIS (le mode d’hébergement *out-of-process*) avec le [serveur Kestrel](#kestrel).

Étant donné que les applications ASP.NET Core s’exécutent dans un processus distinct du processus de travail IIS, le module s’occupe de la gestion du processus. Le module démarre le processus pour l’application ASP.NET Core quand la première requête arrive, et il redémarre l’application si elle s’arrête ou se bloque. Il s’agit essentiellement du même comportement que celui des applications s’exécutant in-process, et qui sont gérées par le [service d’activation des processus Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

Le schéma suivant illustre la relation entre IIS, le module ASP.NET Core et une application hébergée hors processus :

![Module ASP.NET Core](_static/ancm-outofprocess.png)

Les requêtes arrivent du web au pilote HTTP.sys en mode noyau. Le pilote route les requêtes vers IIS sur le port configuré du site web, généralement 80 (HTTP) ou 443 (HTTPS). Le module transfère les requêtes à Kestrel sur un port aléatoire pour l’application, qui n’est ni le port 80 ni le port 443.

Le module spécifie le port via une variable d’environnement au démarrage, et le middleware d’intégration IIS configure le serveur pour qu’il écoute sur `http://localhost:{port}`. Des vérifications supplémentaires sont effectuées, et les requêtes qui ne proviennent pas du module sont rejetées. Le module ne prend pas en charge le transfert HTTPS : les requêtes sont donc transférées via HTTP, même si IIS les reçoit via HTTPS.

Dès que Kestrel sélectionne la requête dans le module, celle-ci est envoyée (push) dans le pipeline de middlewares d’ASP.NET Core. Le pipeline de middlewares traite la requête et la passe en tant qu’instance de `HttpContext` à la logique de l’application. Le middleware ajouté par l’intégration d’IIS met à jour le schéma, l’adresse IP distante et la base du chemin pour prendre en compte le transfert de la requête à Kestrel. La réponse de l’application est ensuite repassée à IIS, qui la renvoie au client HTTP à l’origine de la requête.

Pour des conseils de configuration d’IIS et du module ASP.NET Core, consultez les rubriques suivantes :

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>

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

  ![Kestrel communique directement avec Internet sans serveur proxy inverse](kestrel/_static/kestrel-to-internet2.png)

* Avec un *serveur proxy inverse*, comme [IIS (Internet Information Services)](https://www.iis.net/), [Nginx](http://nginx.org) ou [Apache](https://httpd.apache.org/). Un serveur proxy inverse reçoit les requêtes HTTP en provenance d’Internet et les transmet à Kestrel.

  ![Kestrel communique indirectement avec Internet via un serveur proxy inverse, par exemple IIS, Nginx ou Apache](kestrel/_static/kestrel-to-internet.png)

Les deux configurations d’hébergement, &mdash;avec ou sans serveur proxy inverse&mdash;, sont prises en charge pour les applications ASP.NET Core 2.1 ou des versions ultérieures.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Si l’application accepte seulement les requêtes provenant d’un réseau interne, Kestrel peut être utilisé par lui-même.

![Kestrel communique directement avec le réseau interne](kestrel/_static/kestrel-to-internal.png)

Si l’application est exposée à Internet, Kestrel doit utiliser un *serveur proxy inverse* comme[IIS (Internet Information Services)](https://www.iis.net/), [Nginx](http://nginx.org) ou [Apache](https://httpd.apache.org/). Un serveur proxy inverse reçoit les requêtes HTTP en provenance d’Internet et les transmet à Kestrel.

![Kestrel communique indirectement avec Internet via un serveur proxy inverse, par exemple IIS, Nginx ou Apache](kestrel/_static/kestrel-to-internet.png)

La raison la plus importante de l’utilisation d’un proxy inversé pour les déploiements de serveurs de périphérie publics directement exposés à Internet est la sécurité. Les versions 1.x de Kestrel n’incluent pas de fonctionnalités de sécurité suffisantes pour vous défendre contre les attaques provenant d’Internet. Ceci inclut, entre autres choses, des délais d’attente appropriés, des limites de taille des requêtes et des limites du nombre de connexions simultanées.

::: moniker-end

Pour des conseils de configuration de Kestrel et des informations sur le moment d’utiliser Kestrel dans une configuration de proxy inverse, consultez <xref:fundamentals/servers/kestrel>.

### <a name="nginx-with-kestrel"></a>Nginx avec Kestrel

Pour plus d’informations sur l’utilisation de Nginx sur Linux comme serveur proxy inverse pour Kestrel, consultez <xref:host-and-deploy/linux-nginx>.

### <a name="apache-with-kestrel"></a>Apache avec Kestrel

Pour plus d’informations sur l’utilisation d’Apache sur Linux comme serveur proxy inverse pour Kestrel, consultez <xref:host-and-deploy/linux-apache>.

::: moniker range=">= aspnetcore-2.2"

## <a name="iis-http-server"></a>Serveur HTTP IIS

Le serveur HTTP IIS est un [serveur in-process](#in-process-hosting-model) pour IIS requis pour les déploiements in-process. Le [module ASP.NET Core](xref:host-and-deploy/aspnet-core-module) gère des requêtes IIS natives entre IIS et le serveur HTTP IIS. Pour plus d'informations, consultez <xref:host-and-deploy/aspnet-core-module>.

::: moniker-end

## <a name="httpsys"></a>HTTP.sys

Si vos applications ASP.NET Core sont exécutées sur Windows, HTTP.sys est une alternative à Kestrel. Kestrel est généralement recommandé pour de meilleures performances. HTTP.sys peut être utilisé dans les scénarios où l’application est exposée à Internet et où des fonctionnalités requises sont prises en charge par HTTP.sys, mais pas par Kestrel. Pour plus d'informations, consultez <xref:fundamentals/servers/httpsys>.

![HTTP.sys communique directement avec Internet](httpsys/_static/httpsys-to-internet.png)

HTTP.sys peut également être utilisé pour les applications qui sont exposées seulement à un réseau interne.

![HTTP.sys communique directement avec le réseau interne](httpsys/_static/httpsys-to-internal.png)

Pour obtenir des conseils de configuration de HTTP.sys, consultez <xref:fundamentals/servers/httpsys>.

## <a name="aspnet-core-server-infrastructure"></a>Infrastructure serveur d’ASP.NET Core

Le [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) disponible dans la méthode `Startup.Configure` expose la propriété [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) de type [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection). Kestrel et HTTP.sys n’exposent qu’une seule fonctionnalité chacun, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), mais des implémentations serveur différentes peuvent exposer des fonctionnalités supplémentaires.

`IServerAddressesFeature` permet de déterminer quel est le port lié à l’implémentation serveur au moment de l’exécution.

## <a name="custom-servers"></a>Serveurs personnalisés

Si les serveurs intégrés ne répondent pas aux spécifications de l’application, vous pouvez créer une implémentation serveur personnalisée. Le [guide OWIN (Open Web Interface pour .NET)](xref:fundamentals/owin) montre comment écrire une implémentation [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) basée sur [Nowin](https://github.com/Bobris/Nowin). Seules les interfaces des fonctionnalités utilisées par l’application nécessitent une implémentation, bien qu’au minimum, [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) et [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) doivent être pris en charge.

## <a name="server-startup"></a>Démarrage du serveur

Le serveur est lancé lorsque l’environnement de développement intégré (IDE) ou l’éditeur démarre l’application :

* Des &ndash;profils de lancement [Visual Studio](https://www.visualstudio.com/vs/) peuvent être utilisés pour démarrer l’application et le serveur avec [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[Module ASP.NET Core](xref:host-and-deploy/aspnet-core-module) ou avec la console.
* [Visual Studio Code](https://code.visualstudio.com/) &ndash;L’application et le serveur sont démarrés par [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), qui active le débogueur CoreCLR.
* [Visual Studio pour Mac](https://www.visualstudio.com/vs/mac/) &ndash; L’application et le serveur sont démarrés par le [débogueur Mono Soft-Mode](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).

Lors du lancement de l’application à partir d’une invite de commandes dans le dossier du projet, [dotnet run](/dotnet/core/tools/dotnet-run) lance l’application et le serveur (Kestrel et HTTP.sys uniquement). La configuration est spécifiée par l’option `-c|--configuration`, qui est définie sur `Debug` (par défaut) ou sur `Release`. Si les profils de lancement sont présents dans un fichier *launchSettings.json*, utilisez l’option `--launch-profile <NAME>` pour définir le profil de lancement (par exemple `Development` ou `Production`). Pour plus d’informations, consultez les rubriques [dotnet run](/dotnet/core/tools/dotnet-run) et [Package de distribution de .NET Core](/dotnet/core/build/distribution-packaging).

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
  * Framework cible : non applicable aux déploiements HTTP.sys.
* [IIS (in-process)](xref:host-and-deploy/iis/index#http2-support)
  * Windows Server 2016/Windows 10 ou version ultérieure ; IIS 10 ou version ultérieure
  * Version cible de .Net Framework : .NET Core 2.2 ou version ultérieure
* [IIS (out-of-process)](xref:host-and-deploy/iis/index#http2-support)
  * Windows Server 2016/Windows 10 ou version ultérieure ; IIS 10 ou version ultérieure
  * Les connexions au serveur périphérique public utilisent HTTP/2, mais la connexion de proxy inverse à Kestrel utilise HTTP/1.1.
  * Framework cible : non applicable aux déploiements IIS out-of-process.

&dagger;Kestrel propose une prise en charge limitée de HTTP/2 sous Windows Server 2012 R2 et Windows 8.1. La prise en charge est limitée car la liste des suites de chiffrement TLS prises en charge sur ces systèmes d’exploitation est limitée. Un certificat généré à l’aide d’Elliptic Curve Digital Signature algorithme (ECDSA) peut être requis pour sécuriser les connexions TLS.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [HTTP.sys](xref:fundamentals/servers/httpsys#http2-support)
  * Windows Server 2016/Windows 10 ou version ultérieure
  * Framework cible : non applicable aux déploiements HTTP.sys.
* [IIS (out-of-process)](xref:host-and-deploy/iis/index#http2-support)
  * Windows Server 2016/Windows 10 ou version ultérieure ; IIS 10 ou version ultérieure
  * Les connexions au serveur périphérique public utilisent HTTP/2, mais la connexion de proxy inverse à Kestrel utilise HTTP/1.1.
  * Framework cible : non applicable aux déploiements IIS out-of-process.

::: moniker-end

Une connexion HTTP/2 doit utiliser la [négociation du protocole de la couche d’application (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) et TLS 1.2 ou version ultérieure. Pour plus d’informations, consultez les rubriques qui se rapportent à vos scénarios de déploiement de serveur.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:fundamentals/servers/kestrel>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:fundamentals/servers/httpsys>
