---
title: Informations de référence sur la configuration du Module ASP.NET Core
author: guardrex
description: Découvrez comment configurer le module ASP.NET Core pour héberger des applications ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 954841a1b1465c80e60d5745ad9e22294a88fdf4
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/18/2018
ms.locfileid: "31483745"
---
# <a name="aspnet-core-module-configuration-reference"></a>Informations de référence sur la configuration du Module ASP.NET Core

Par [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT) et [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Ce document fournit des instructions permettant de configurer le module ASP.NET Core pour héberger des applications ASP.NET Core. Pour obtenir une présentation du module ASP.NET Core et des instructions d’installation, consultez la [vue d’ensemble du module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).

## <a name="configuration-with-webconfig"></a>Configuration avec web.config

Le module ASP.NET Core est configuré avec la section `aspNetCore` du nœud `system.webServer` dans le fichier *web.config* du site.

Le fichier *web.config* suivant est publié pour un [déploiement dépendant du framework](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) et configure le module ASP.NET Core pour gérer les demandes du site :

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="dotnet" 
                arguments=".\MyApp.dll" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Le fichier *web.config* suivant est publié pour un [déploiement autonome](/dotnet/articles/core/deploying/#self-contained-deployments-scd) :

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Lorsqu’une application est déployée sur [Azure App Service](https://azure.microsoft.com/services/app-service/), le chemin d’accès `stdoutLogFile` est défini sur `\\?\%home%\LogFiles\stdout`. Le chemin d’accès enregistre les journaux stdout dans le dossier *LogFiles*, un emplacement automatiquement créé par le service.

Consultez [Configuration de la sous-application](xref:host-and-deploy/iis/index#sub-application-configuration) pour lire une note importante concernant la configuration des fichiers *web.config* dans les sous-applications.

### <a name="attributes-of-the-aspnetcore-element"></a>Attributs de l’élément aspNetCore

::: moniker range="<= aspnetcore-2.0"
| Attribut | Description | Par défaut |
| --------- | ----------- | :-----: |
| `arguments` | <p>Attribut de chaîne facultatif.</p><p>Arguments pour l’exécutable spécifié dans **processPath**.</p>| |
| `disableStartUpErrorPage` | vrai ou faux.</p><p>Si la valeur est true, la page **502.5 - Échec du processus** est supprimée, et la page de code d’état 502 configurée dans le fichier *web.config* est prioritaire.</p> | `false` |
| `forwardWindowsAuthToken` | vrai ou faux.</p><p>Si la valeur est true, le jeton est transmis au processus enfant qui écoute sur %ASPNETCORE_PORT% sous la forme d’un en-tête 'MS-ASPNETCORE-WINAUTHTOKEN' par demande. Il incombe à ce processus d’appeler CloseHandle sur ce jeton par demande.</p> | `true` |
| `processPath` | <p>Attribut de chaîne requis.</p><p>Chemin d’accès au fichier exécutable lançant un processus d’écoute des requêtes HTTP. Les chemins d’accès relatifs sont pris en charge. Si le chemin d’accès commence par `.`, il est considéré comme étant relatif par rapport à la racine du site.</p> | |
| `rapidFailsPerMinute` | <p>Attribut entier facultatif.</p><p>Indique le nombre de fois où le processus spécifié dans **processPath** est autorisé à se bloquer par minute. Si cette limite est dépassée, le module arrête le lancement du processus pour le reste de la minute.</p> | `10` |
| `requestTimeout` | <p>Attribut timespan facultatif.</p><p>Spécifie la durée pendant laquelle le module ASP.NET Core attend une réponse de la part du processus à l’écoute sur %ASPNETCORE_PORT%.</p><p>Dans les versions du module ASP.NET Core fournies avec ASP.NET Core 2.0 ou version antérieure, la durée `requestTimeout` doit être spécifiée en minutes entières, sinon la valeur par défaut est de 2 minutes.</p> | `00:02:00` |
| `shutdownTimeLimit` | <p>Attribut entier facultatif.</p><p>Durée en secondes pendant laquelle le module attend que l’exécutable s’arrête normalement lorsque le fichier *app_offline.htm* est détecté.</p> | `10` |
| `startupTimeLimit` | <p>Attribut entier facultatif.</p><p>Durée en secondes pendant laquelle le module attend que le fichier exécutable démarre un processus à l’écoute sur le port. Si cette limite de temps est dépassée, le module met fin au processus. Le module tente de relancer le processus lorsqu’il reçoit une nouvelle requête, puis continue d’essayer de redémarrer le processus pour les requêtes entrantes suivantes, sauf si l’application ne démarre pas **rapidFailsPerMinute** un certain nombre de fois au cours de la dernière minute.</p> | `120` |
| `stdoutLogEnabled` | <p>Attribut booléen facultatif.</p><p>Si la valeur est true, les attributs **stdout** et **stderr** du processus spécifié dans **processPath** sont redirigés vers le fichier spécifié dans **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>Attribut de chaîne facultatif.</p><p>Spécifie le chemin d’accès relatif ou absolu pour lequel les attributs **stdout** et **stderr** du processus spécifié dans **processPath** sont consignés. Les chemins d’accès relatifs sont relatifs par rapport à la racine du site. Tout chemin d’accès commençant par `.` est relatif par rapport à la racine du site et tous les autres chemins d’accès sont traités comme des chemins d’accès absolus. Tous les dossiers spécifiés dans le chemin d’accès doivent exister pour permettre au module de créer le fichier journal. Si vous utilisez des délimiteurs de trait de soulignement, un horodatage, un ID de processus et une extension de fichier (*.log*) sont ajoutés au dernier segment du chemin d'accès **stdoutLogFile**. Si `.\logs\stdout` est fourni en tant que valeur, un exemple de journal stdout est enregistré en tant que *stdout_20180205194132_1934.log* dans le dossier *journaux* avec un enregistrement effectué le 05/02/2018 à 19:41:32 et un ID de processus de 1934.</p> | `aspnetcore-stdout` |
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
| Attribut | Description | Par défaut |
| --------- | ----------- | :-----: |
| `arguments` | <p>Attribut de chaîne facultatif.</p><p>Arguments pour l’exécutable spécifié dans **processPath**.</p>| |
| `disableStartUpErrorPage` | vrai ou faux.</p><p>Si la valeur est true, la page **502.5 - Échec du processus** est supprimée, et la page de code d’état 502 configurée dans le fichier *web.config* est prioritaire.</p> | `false` |
| `forwardWindowsAuthToken` | vrai ou faux.</p><p>Si la valeur est true, le jeton est transmis au processus enfant qui écoute sur %ASPNETCORE_PORT% sous la forme d’un en-tête 'MS-ASPNETCORE-WINAUTHTOKEN' par demande. Il incombe à ce processus d’appeler CloseHandle sur ce jeton par demande.</p> | `true` |
| `processPath` | <p>Attribut de chaîne requis.</p><p>Chemin d’accès au fichier exécutable lançant un processus d’écoute des requêtes HTTP. Les chemins d’accès relatifs sont pris en charge. Si le chemin d’accès commence par `.`, il est considéré comme étant relatif par rapport à la racine du site.</p> | |
| `rapidFailsPerMinute` | <p>Attribut entier facultatif.</p><p>Indique le nombre de fois où le processus spécifié dans **processPath** est autorisé à se bloquer par minute. Si cette limite est dépassée, le module arrête le lancement du processus pour le reste de la minute.</p> | `10` |
| `requestTimeout` | <p>Attribut timespan facultatif.</p><p>Spécifie la durée pendant laquelle le module ASP.NET Core attend une réponse de la part du processus à l’écoute sur %ASPNETCORE_PORT%.</p><p>Dans les versions du module ASP.NET Core fournies avec ASP.NET Core 2.1 ou version ultérieure, la valeur `requestTimeout` est spécifiée en heures, minutes et secondes.</p> | `00:02:00` |
| `shutdownTimeLimit` | <p>Attribut entier facultatif.</p><p>Durée en secondes pendant laquelle le module attend que l’exécutable s’arrête normalement lorsque le fichier *app_offline.htm* est détecté.</p> | `10` |
| `startupTimeLimit` | <p>Attribut entier facultatif.</p><p>Durée en secondes pendant laquelle le module attend que le fichier exécutable démarre un processus à l’écoute sur le port. Si cette limite de temps est dépassée, le module met fin au processus. Le module tente de relancer le processus lorsqu’il reçoit une nouvelle requête, puis continue d’essayer de redémarrer le processus pour les requêtes entrantes suivantes, sauf si l’application ne démarre pas **rapidFailsPerMinute** un certain nombre de fois au cours de la dernière minute.</p> | `120` |
| `stdoutLogEnabled` | <p>Attribut booléen facultatif.</p><p>Si la valeur est true, les attributs **stdout** et **stderr** du processus spécifié dans **processPath** sont redirigés vers le fichier spécifié dans **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>Attribut de chaîne facultatif.</p><p>Spécifie le chemin d’accès relatif ou absolu pour lequel les attributs **stdout** et **stderr** du processus spécifié dans **processPath** sont consignés. Les chemins d’accès relatifs sont relatifs par rapport à la racine du site. Tout chemin d’accès commençant par `.` est relatif par rapport à la racine du site et tous les autres chemins d’accès sont traités comme des chemins d’accès absolus. Tous les dossiers spécifiés dans le chemin d’accès doivent exister pour permettre au module de créer le fichier journal. Si vous utilisez des délimiteurs de trait de soulignement, un horodatage, un ID de processus et une extension de fichier (*.log*) sont ajoutés au dernier segment du chemin d'accès **stdoutLogFile**. Si `.\logs\stdout` est fourni en tant que valeur, un exemple de journal stdout est enregistré en tant que *stdout_20180205194132_1934.log* dans le dossier *journaux* avec un enregistrement effectué le 05/02/2018 à 19:41:32 et un ID de processus de 1934.</p> | `aspnetcore-stdout` |
::: moniker-end

### <a name="setting-environment-variables"></a>Définition des variables d'environnement

Des variables d’environnement peuvent être spécifiées pour le processus dans l’attribut `processPath`. Spécifiez une variable d’environnement à l’aide de l’élément enfant `environmentVariable` d’un élément de collection `environmentVariables`. Les variables d’environnement définies dans cette section prévalent sur les variables d’environnement système.

L’exemple suivant définit deux variables d'environnement. `ASPNETCORE_ENVIRONMENT` définit l’environnement de l’application sur `Development`. Un développeur peut définir temporairement cette valeur dans le fichier *web.config* afin de forcer le chargement de la [Page d’exception de développeur](xref:fundamentals/error-handling) lors du débogage d’une exception d’application. `CONFIG_DIR` est un exemple de variable d’environnement définie par l’utilisateur, dans laquelle le développeur a écrit du code qui lit la valeur de démarrage afin de former un chemin d’accès pour le chargement du fichier de configuration de l’application.

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

> [!WARNING]
> Définissez uniquement la variable d’environnement `ASPNETCORE_ENVIRONMENT` sur `Development` sur les serveurs non accessibles aux réseaux non approuvés, par exemple Internet.

## <a name="appofflinehtm"></a>app_offline.htm

Si un fichier portant le nom *app_offline.htm* est détecté dans le répertoire racine d’une application, le module ASP.NET Core tente d’arrêter normalement l’application, puis interrompt le traitement des requêtes entrantes. Si l’application est toujours active après le nombre de secondes défini dans `shutdownTimeLimit`, le module ASP.NET Core met fin au processus en cours d’exécution.

Tant que le fichier *app_offline.htm* est présent, le module ASP.NET Core répond aux requêtes en renvoyant le contenu du fichier *app_offline.htm*. Lorsque le fichier *app_offline.htm* est supprimé, la requête suivante démarre l’application.

## <a name="start-up-error-page"></a>Page d’erreur de démarrage

Si le module ASP.NET Core ne peut pas lancer le processus principal ou que ce processus principal démarre mais ne parvient pas à écouter sur le port configuré, une page de code d’état *502.5 Échec du processus* s’affiche. Pour supprimer cette page et revenir à la page IIS de code d’état 502 par défaut, utilisez l’attribut `disableStartUpErrorPage`. Pour plus d’informations sur la configuration des messages d’erreur personnalisés, consultez [Erreurs HTTP `<httpErrors>`](/iis/configuration/system.webServer/httpErrors/).

![Page de code d’état 502.5 Échec du processus](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a>Création et redirection de journal

Le module ASP.NET Core redirige les journaux stdout et stderr vers le disque si les attributs `stdoutLogEnabled` et `stdoutLogFile` de l’élément `aspNetCore` sont définis. Tous les dossiers spécifiés dans le chemin d’accès `stdoutLogFile` doivent exister pour permettre au module de créer le fichier journal. Le pool d’applications doit avoir un accès en écriture à l’emplacement auquel les journaux sont écrits (utilisez `IIS AppPool\<app_pool_name>` pour fournir les autorisations d’écriture).

Aucune rotation n’est appliquée aux journaux, sauf en cas de recyclage/redémarrage du processus. Il incombe à l’hébergeur de limiter l’espace disque utilisé par les journaux.

L’utilisation du journal stdout est uniquement recommandée pour résoudre les problèmes de démarrage de l’application. N’utilisez pas le journal stdout à des fins de journalisation d’application générale. Pour journaliser la routine d’une application ASP.NET Core, utilisez une bibliothèque de journalisation qui limite la taille du fichier journal et appliquez une rotation aux journaux. Pour plus d’informations, consultez [Fournisseurs de journalisation tiers](xref:fundamentals/logging/index#third-party-logging-providers).

Un horodatage et une extension de fichier sont ajoutés automatiquement quand le fichier journal est créé. Le nom du fichier journal est créé en ajoutant l’horodatage, un ID de processus et une extension de fichier (*.log*) au dernier segment du chemin d'accès `stdoutLogFile` (généralement *stdout*), séparés par des traits de soulignement. Si le chemin d'accès `stdoutLogFile` se termine par *stdout*, un journal pour une application avec un PID de 1934 créé le 5/2/2018 à 19:42:32 affiche le nom de fichier *stdout_20180205194132_1934.log*.

L’exemple d’élément `aspNetCore` suivant configure la journalisation stdout pour une application hébergée dans Azure App Service. Un chemin d’accès local ou un chemin de partage réseau peut être utilisé pour une journalisation locale. Vérifiez que l’identité de l’utilisateur AppPool à l’autorisation d’écrire dans le chemin d’accès fourni.

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

Consultez [Configuration avec web.config](#configuration-with-webconfig) pour obtenir un exemple d’élément `aspNetCore` dans le fichier *web.config*.

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>La configuration du proxy utilise le protocole HTTP et un jeton d’appariement

Le proxy créé entre le module ASP.NET Core et Kestrel utilise le protocole HTTP. Utiliser HTTP est une optimisation des performances où le trafic entre le module et Kestrel a lieu sur une adresse de bouclage hors de l’interface réseau. Il n’existe aucun risque d’écoute clandestine du trafic entre le module et Kestrel à partir d’un emplacement sur le serveur.

Un jeton d’appariement est utilisé pour garantir que les demandes reçues par Kestrel ont été traitées par IIS et ne proviennent pas d’une autre source. Le jeton d’appariement est créé et défini dans une variable d’environnement (`ASPNETCORE_TOKEN`) par le module. Le jeton d’appariement est également défini dans un en-tête (`MSAspNetCoreToken`) sur toutes les demandes traitées. L'intergiciel IIS vérifie chaque demande qu’il reçoit pour confirmer que la valeur d’en-tête du jeton d'appariement correspond à la valeur de la variable d’environnement. Si les valeurs de jeton ne correspondent pas, la demande est connectée et rejetée. La variable d’environnement du jeton d'appariement et le trafic entre le module et Kestrel ne sont pas accessibles à partir d’un emplacement sur le serveur. Sans connaître la valeur du jeton d'appariement, une personne malveillante ne peut pas soumettre des demandes qui contournent la vérification de l’intergiciel IIS.

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>Module ASP.NET Core avec une configuration partagée IIS

Le programme d’installation du module ASP.NET Core s’exécute avec les privilèges du compte **système**. Comme le compte système local n’a pas l’autorisation de modifier le chemin d’accès de partage utilisé par la configuration partagée IIS, le programme d’installation rencontre une erreur d’accès refusé lorsque vous tentez de configurer les paramètres du module dans *applicationHost.config* sur le partage. Lorsque vous utilisez une configuration partagée IIS, procédez comme suit :

1. Désactivez la configuration partagée IIS.
1. Exécutez le programme d’installation.
1. Exportez le fichier *applicationHost.config* mis à jour vers le partage.
1. Réactivez la configuration partagée IIS.

## <a name="module-version-and-hosting-bundle-installer-logs"></a>Version du module et journaux du programme d’installation du bundle d’hébergement

Pour déterminer la version du module ASP.NET Core installé :

1. Sur le système hôte, accédez à *%windir%\System32\inetsrv*.
1. Recherchez le fichier *aspnetcore.dll*.
1. Cliquez avec le bouton droit sur le fichier, puis sélectionnez **Propriétés** dans le menu contextuel.
1. Sélectionnez l'onglet **Détails**. Le **version du fichier** et la **version du produit** représentent la version installée du module.

Les journaux du programme d’installation du bundle d’hébergement du module se trouvent dans *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. Le fichier est nommé *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.

## <a name="module-schema-and-configuration-file-locations"></a>Emplacements des fichiers du module, du schéma et de configuration

### <a name="module"></a>Module

**IIS (x86/amd64) :**

   * %windir%\System32\inetsrv\aspnetcore.dll

   * %windir%\SysWOW64\inetsrv\aspnetcore.dll

**IIS Express (x86/amd64) :**

   * %ProgramFiles%\IIS Express\aspnetcore.dll

   * %ProgramFiles(x86)%\IIS Express\aspnetcore.dll

### <a name="schema"></a>Schéma

**IIS**

   * %windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml

**IIS Express**

   * %ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml

### <a name="configuration"></a>Configuration

**IIS**

   * %windir%\System32\inetsrv\config\applicationHost.config

**IIS Express**

   * .vs\config\applicationHost.config

Vous trouverez les fichiers en recherchant *aspnetcore.dll* dans le fichier *applicationHost.config*. Pour IIS Express, le fichier *applicationHost.config* n’existe pas par défaut. Le fichier est créé dans *\<application_root >\\.vs\\config* lors du démarrage d’un projet d’application web dans la solution Visual Studio.
