---
title: Module ASP.NET Core
author: tdykstra
description: "Présente le module ASP.NET Core (ANCM), module IIS qui permet au serveur web Kestrel d’utiliser IIS ou IIS Express en tant que serveur proxy inverse."
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 08/03/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 4337bc42c5454d6a9634a396d9c89f3518af148b
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-aspnet-core-module"></a>Introduction au module ASP.NET Core

By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl) et [Chris Ross](https://github.com/Tratcher) 

Le module ASP.NET Core (ANCM) vous permet d’exécuter des applications ASP.NET Core derrière IIS, en profitant à la fois des qualités d’IIS (sécurité, facilité de gestion, entre autres) et de [Kestrel](kestrel.md) (rapidité). **Le module ANCM fonctionne uniquement avec Kestrel ; il n’est pas compatible avec WebListener (dans ASP.NET Core 1.x) ou HTTP.sys (dans 2.x).** 

Versions Windows prises en charge :

* Windows 7 et Windows Server 2008 R2 et ultérieur

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-aspnet-core-module-does"></a>Ce que fait le module ASP.NET Core

Le module ANCM est un module IIS natif qui se connecte au pipeline IIS et redirige le trafic vers l’application ASP.NET Core backend. La plupart des autres modules, tels que l’authentification Windows, conservent néanmoins la possibilité de s’exécuter. Le module ANCM ne prend le contrôle que quand un gestionnaire est sélectionné pour la demande et qu’un mappage de gestionnaire est défini dans le fichier *web.config* de l’application.

Étant donné que les applications ASP.NET Core s’exécutent dans un processus distinct du processus Worker IIS, le module ANCM fait également de la gestion de processus. Le module ANCM démarre le processus pour l’application ASP.NET Core quand la première demande arrive et le redémarre quand elle s’arrête sur incident. Ce comportement rappelle pour l’essentiel celui des applications ASP.NET classiques qui s’exécutent In-process dans IIS et sont gérées par le service d’activation Windows (WAS).

Voici un diagramme qui illustre les relations entre IIS, le module ANCM et les applications ASP.NET Core.

![Module ASP.NET Core](aspnet-core-module/_static/ancm.png)

Les demandes proviennent du web et atteignent le pilote Http.Sys en mode noyau qui les achemine vers IIS sur le port principal (80) ou le port SSL (443). Le module ANCM transfère les demandes à l’application ASP.NET Core sur le port HTTP configuré pour l’application, qui n’est pas le port 80/443.

Kestrel écoute le trafic en provenance du module ANCM.  Le module ANCM spécifie le port par le biais d’une variable d’environnement au démarrage et la méthode [UseIISIntegration](#call-useiisintegration) configure le serveur d’écoute sur `http://localhost:{port}`. Il existe des vérifications supplémentaires visant à rejeter les demandes qui ne proviennent pas du module ANCM. (Le module ANCM ne prenant pas en charge le transfert HTTPS, les demandes sont transférées via HTTP même si elles sont reçues par IIS via HTTPS.)

Kestrel récupère les demandes en provenance du module ANCM et les transmet au pipeline d’intergiciel (middleware) ASP.NET Core, qui les traite, puis les passe en tant qu’instances `HttpContext` à la logique d’application. Les réponses de l’application sont ensuite passées à IIS, qui les renvoient au client HTTP à l’origine des demandes.

Le module ANCM a d’autres fonctions :

* Il définit des variables d’environnement.
* Il journalise la sortie `stdout` dans un stockage de fichiers.
* Il transfère les jetons d’authentification Windows.

## <a name="how-to-use-ancm-in-aspnet-core-apps"></a>Comment utiliser le module ANCM dans les applications ASP.NET Core

Cette section fournit une vue d’ensemble du processus de configuration d’un serveur IIS et d’une application ASP.NET Core. Pour obtenir des instructions détaillées, consultez [Hôte sur Windows avec IIS](xref:host-and-deploy/iis/index).

### <a name="install-ancm"></a>Installer le module ANCM


Le module ASP.NET Core doit être installé dans IIS sur vos serveurs et dans IIS Express sur vos machines de développement. Pour les serveurs, le module ANCM est inclus dans le [bundle d’hébergement Windows Server .NET Core](https://aka.ms/dotnetcore-2-windowshosting). Pour les machines de développement, Visual Studio installe automatiquement le module ANCM dans IIS Express, ainsi que dans IIS s’il est déjà installé sur les machines.

### <a name="install-the-iisintegration-nuget-package"></a>Installer le package NuGet IISIntegration

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Le package [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) est inclus dans les métapackages ASP.NET Core ([Microsoft.AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore/) et [Microsoft.AspNetCore.All](xref:fundamentals/metapackage)). Si vous n’utilisez aucun métapackage, installez `Microsoft.AspNetCore.Server.IISIntegration` séparément. Le package `IISIntegration` est un pack d’interopérabilité qui lit les variables d’environnement diffusées par le module ANCM pour configurer votre application. Les variables d’environnement fournissent des informations de configuration, telles que le port d’écoute. 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Dans votre application, installez [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/). Le package `IISIntegration` est un pack d’interopérabilité qui lit les variables d’environnement diffusées par le module ANCM pour configurer votre application. Les variables d’environnement fournissent des informations de configuration, telles que le port d’écoute. 

---

### <a name="call-useiisintegration"></a>Appeler UseIISIntegration

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

La méthode d’extension `UseIISIntegration` sur [`WebHostBuilder`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) est appelée automatiquement quand l’exécution recourt à IIS.

Si vous n’utilisez aucun métapackage ASP.NET Core et que vous n’avez pas installé le package `Microsoft.AspNetCore.Server.IISIntegration`, vous obtenez une erreur d’exécution. Si vous appelez `UseIISIntegration` explicitement et que le package n’est pas installé, vous obtenez une erreur de compilation.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Dans la méthode `Main` de votre application, appelez la méthode d’extension `UseIISIntegration` sur [`WebHostBuilder`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder). 

[!code-csharp[](aspnet-core-module/sample/Program.cs?name=snippet_Main&highlight=12)]

---

La méthode `UseIISIntegration` recherche les variables d’environnement définies par le module ANCM et n’effectue aucune opération si elle ne les trouve pas. Ce comportement facilite les scénarios tels que le développement et les tests sur macOS ou Linux et le déploiement sur un serveur qui exécute IIS. Sur macOS ou Linux, Kestrel joue le rôle de serveur web ; mais quand l’application est déployée sur l’environnement IIS, elle utilise automatiquement le module ANCM et IIS.

### <a name="ancm-port-binding-overrides-other-port-bindings"></a>La liaison de port du module ANCM remplace les autres liaisons de port

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Le module ANCM génère un port dynamique à assigner au processus backend. La méthode `UseIISIntegration` récupère ce port dynamique et configure Kestrel pour écouter sur `http://locahost:{dynamicPort}/`. Les autres configurations d’URL sont alors remplacées, telles que les appels à `UseUrls` ou à [l’API d’écoute de Kestrel](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration). Ainsi, vous n’avez pas besoin d’appeler `UseUrls` ou l’API `Listen` de Kestrel quand vous utilisez le module ANCM. Si vous appelez `UseUrls` ou `Listen`, Kestrel est à l’écoute sur le port que vous spécifiez quand vous exécutez l’application sans IIS.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Le module ANCM génère un port dynamique à assigner au processus backend. La méthode `UseIISIntegration` récupère ce port dynamique et configure Kestrel pour écouter sur `http://locahost:{dynamicPort}/`. Les autres configurations d’URL sont alors remplacées, telles que les appels à `UseUrls`. Ainsi, vous n’avez pas besoin d’appeler `UseUrls` quand vous utilisez le module ANCM. Si vous appelez `UseUrls`, Kestrel est à l’écoute sur le port que vous spécifiez quand vous exécutez l’application sans IIS.

Dans ASP.NET Core 1.0, si vous appelez `UseUrls`, faites-le **avant** d’appeler `UseIISIntegration` afin que le port configuré par le module ANCM ne soit pas remplacé. Cet ordre d’appel n’est pas requis dans ASP.NET Core 1.1, car le paramètre du module ANCM remplace `UseUrls`.

---

### <a name="configure-ancm-options-in-webconfig"></a>Configurer les options du module ANCM dans le fichier Web.config

La configuration du module ASP.NET Core est stockée dans le fichier *web.config* qui se trouve dans le dossier racine de l’application. Les paramètres contenus dans ce fichier pointent vers la commande et les arguments de démarrage qui démarrent votre application ASP.NET Core. Pour obtenir un exemple de code *web.config* et des conseils sur les options de configuration, consultez [Informations de référence sur la configuration du module ASP.NET Core](xref:host-and-deploy/aspnet-core-module).

### <a name="run-with-iis-express-in-development"></a>Exécuter avec IIS Express dans l’environnement de développement

IIS Express peut être lancé par Visual Studio à l’aide du profil par défaut défini par les modèles ASP.NET Core.

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>La configuration du proxy utilise le protocole HTTP et un jeton d’appariement

Le proxy créé entre le module ANCM et Kestrel utilise le protocole HTTP. Utiliser HTTP permet d’optimiser les performances si le trafic entre le module ANCM et Kestrel a lieu sur une adresse de bouclage hors de l’interface réseau. Il n’existe aucun risque d’écoute clandestine du trafic entre le module ANCM et Kestrel à partir d’un emplacement hors du serveur.

Un jeton d’appariement est utilisé pour garantir que les demandes reçues par Kestrel sont traitées par IIS et qu’elles ne proviennent pas d’une autre source. Le jeton d’appariement est créé et défini dans une variable d’environnement (`ASPNETCORE_TOKEN`) par le module ANCM. Le jeton d’appariement est également défini dans un en-tête (`MSAspNetCoreToken`) sur toutes les demandes en proxy. L’intergiciel (middleware) IIS vérifie chaque demande qu’il reçoit pour confirmer que la valeur d’en-tête du jeton d’appariement correspond à la valeur de variable d’environnement. Si les valeurs de jeton ne correspondent pas, la demande est journalisée et rejetée. La variable d’environnement du jeton d’appariement et le trafic entre le module ANCM et Kestrel ne sont pas accessibles à partir d’un emplacement hors du serveur. Sans connaître la valeur du jeton d’appariement, une personne malveillante ne peut pas soumettre de demandes qui contournent la vérification effectuée par l’intergiciel (middleware) IIS.

## <a name="next-steps"></a>Étapes suivantes

Pour plus d'informations, reportez-vous aux ressources suivantes :

* [Exemple d’application pour cet article](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample)
* [Code source du module ASP.NET Core](https://github.com/aspnet/AspNetCoreModule)
* [Informations de référence sur la configuration du module ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Héberger sur Windows avec IIS](xref:host-and-deploy/iis/index)
