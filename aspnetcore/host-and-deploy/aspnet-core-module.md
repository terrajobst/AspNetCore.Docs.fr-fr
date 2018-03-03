---
title: "Référence de configuration du Module de base ASP.NET"
author: guardrex
description: "Découvrez comment configurer le Module de base ASP.NET pour l’hébergement d’applications ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 5aac5cf2b8fd4bc53ba7201645b9bb02a5d1ecae
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/02/2018
---
# <a name="aspnet-core-module-configuration-reference"></a>Référence de configuration du Module de base ASP.NET

Par [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), et [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Ce document fournit des instructions sur la façon de configurer le Module de base ASP.NET pour l’hébergement d’applications ASP.NET Core. Pour obtenir une présentation du Module de base ASP.NET et des instructions d’installation, consultez le [vue d’ensemble du Module de base ASP.NET](xref:fundamentals/servers/aspnet-core-module).

## <a name="configuration-with-webconfig"></a>Configuration de web.config

Le Module de base ASP.NET est configuré avec le `aspNetCore` section de la `system.webServer` dans du site *web.config* fichier.

Les éléments suivants *web.config* fichier est publié pour un [dépendant du framework déploiement](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) et configure le Module de base ASP.NET pour gérer les demandes de site :

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

Les éléments suivants *web.config* est publiée pour une [déploiement autonome](/dotnet/articles/core/deploying/#self-contained-deployments-scd):

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

Lorsqu’une application est déployée sur [Azure App Service](https://azure.microsoft.com/services/app-service/), le `stdoutLogFile` chemin d’accès est défini sur `\\?\%home%\LogFiles\stdout`. Le chemin d’accès enregistre les journaux de stdout le *LogFiles* dossier, c'est-à-dire un emplacement est automatiquement créé par le service.

Consultez [configuration des application](xref:host-and-deploy/iis/index#sub-application-configuration) pour une note importante se rapportant à la configuration de *web.config* fichiers dans les applications secondaire.

### <a name="attributes-of-the-aspnetcore-element"></a>Attributs de l’élément aspNetCore

| Attribut | Description | Par défaut |
| --------- | ----------- | :-----: |
| `arguments` | <p>Attribut de chaîne facultatif.</p><p>Arguments pour l’exécutable spécifié dans **processPath**.</p>| |
| `disableStartUpErrorPage` | vrai ou faux.</p><p>Si la valeur est true, le **502.5 - Échec d’un processus** page est supprimée, et la page de codes de 502 état configurés dans le *web.config* est prioritaire.</p> | `false` |
| `forwardWindowsAuthToken` | vrai ou faux.</p><p>Si la valeur est true, le jeton est transmis au processus enfant à l’écoute sur % ASPNETCORE_PORT % en tant qu’en-tête 'MS-ASPNETCORE-WINAUTHTOKEN' par demande. Il incombe à ce processus pour appeler CloseHandle sur ce jeton par demande.</p> | `true` |
| `processPath` | <p>Attribut de chaîne requis.</p><p>Chemin d’accès au fichier exécutable qui lance un processus d’écoute les requêtes HTTP. Chemins d’accès relatifs sont pris en charge. Si le chemin d’accès commence par `.`, le chemin d’accès est considéré comme étant relatif à la racine du site.</p> | |
| `rapidFailsPerMinute` | <p>Attribut entier facultatif.</p><p>Spécifie le nombre de fois spécifié par le processus dans **processPath** est autorisé à se bloquer par minute. Si cette limite est dépassée, le module arrête le lancement du processus pour le reste de la minute.</p> | `10` |
| `requestTimeout` | <p>Attribut de timespan facultatif.</p><p>Spécifie la durée pour laquelle le Module de base ASP.NET attend une réponse du processus à l’écoute sur % ASPNETCORE_PORT %.</p><p>Le `requestTimeout` doit être spécifié en minutes entières, sinon la valeur par défaut à 2 minutes.</p> | `00:02:00` |
| `shutdownTimeLimit` | <p>Attribut entier facultatif.</p><p>Durée en secondes pendant laquelle le module attend que l’exécutable d’arrêt en douceur lorsque le *app_offline.htm* fichier est détecté.</p> | `10` |
| `startupTimeLimit` | <p>Attribut entier facultatif.</p><p>Durée en secondes pendant laquelle le module attend que le fichier exécutable démarrer un processus à l’écoute sur le port. Si cette limite de temps est dépassée, le module met fin au processus. Le module tente de relancer le processus lorsqu’il reçoit une nouvelle demande et continue à tenter de redémarrer le processus pour les demandes entrantes suivantes, sauf si l’application ne démarre pas **rapidFailsPerMinute** nombre de fois au cours de la dernière déploiement d’une minute.</p> | `120` |
| `stdoutLogEnabled` | <p>Attribut booléen facultatif.</p><p>Si la valeur est true, **stdout** et **stderr** pour le processus spécifié dans **processPath** sont redirigées vers le fichier spécifié dans **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>Attribut de chaîne facultatif.</p><p>Spécifie le chemin d’accès relatif ou absolu pour lequel **stdout** et **stderr** du processus spécifié dans **processPath** sont enregistrés. Chemins d’accès relatifs sont relatif à la racine du site. N’importe quel chemin d’accès commençant par `.` sont relatif au site racine et tous les autres chemins d’accès sont traités comme des chemins d’accès absolus. Tous les dossiers du chemin d’accès doivent exister dans l’ordre pour le module créer le fichier journal. À l’aide des délimiteurs de trait de soulignement, un horodatage, l’ID de processus et l’extension de fichier (*.log*) sont ajoutés au dernier segment de la **stdoutLogFile** chemin d’accès. Si `.\logs\stdout` est fournie en tant que valeur, un exemple de journal de stdout est enregistré en tant que *stdout_20180205194132_1934.log* dans les *journaux* dossier lorsque enregistrés sur 2/5/2018 à 19:41:32 avec un ID de processus de 1934.</p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a>Définition des variables d’environnement

Variables d’environnement peuvent être spécifiés pour le processus dans le `processPath` attribut. Spécifiez une variable d’environnement avec le `environmentVariable` élément enfant d’un `environmentVariables` l’élément de collection. Variables d’environnement définies dans cette section sont prioritaires sur système variables d’environnement.

L’exemple suivant définit deux variables d’environnement. `ASPNETCORE_ENVIRONMENT` Configure l’environnement de l’application à `Development`. Un développeur peut définir temporairement cette valeur dans la *web.config* fichier afin de forcer le [développeur Exception Page](xref:fundamentals/error-handling) à charger lors du débogage d’une exception d’application. `CONFIG_DIR` est un exemple d’une variable d’environnement définie par l’utilisateur, où le développeur a écrit du code qui lit la valeur de démarrage pour former un chemin d’accès pour le chargement du fichier de configuration de l’application.

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
> Définissez uniquement le `ASPNETCORE_ENVIRONMENT` variable envirnonment à `Development` sur les serveurs qui ne sont pas accessibles aux réseaux non approuvés, tels qu’Internet de test et de mise en lots.

## <a name="appofflinehtm"></a>app_offline.htm

Si un fichier portant le nom *app_offline.htm* est détecté dans le répertoire racine d’une application, le Module de base ASP.NET tente arrêt en douceur de l’application, puis arrêter le traitement des demandes entrantes. Si l’application est en cours d’exécution après le nombre de secondes défini dans `shutdownTimeLimit`, le Module de base ASP.NET met fin au processus en cours d’exécution.

Alors que le *app_offline.htm* fichier est présent, le Module de base ASP.NET répond aux requêtes en renvoyant le contenu de la *app_offline.htm* fichier. Lorsque le *app_offline.htm* fichier est supprimé, la demande suivante démarre l’application.

## <a name="start-up-error-page"></a>Page d’erreur de démarrage

Si le Module de base ASP.NET ne peut pas lancer le processus principal ou que le début de la procédure principale mais ne parvient pas à l’écoute sur le port configuré, un *502.5 Échec d’un processus* page de codes d’état s’affiche. Pour supprimer cette page et revenir à la page de codes par défaut IIS 502 état, utiliser le `disableStartUpErrorPage` attribut. Pour plus d’informations sur la configuration des messages d’erreur personnalisés, consultez [erreurs HTTP `<httpErrors>` ](/iis/configuration/system.webServer/httpErrors/).

![502.5 Page de codes de statut de défaillance processus](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a>Création d’un journal et la redirection

Le Module de base ASP.NET redirige `stdout` et `stderr` journaux sur le disque si le `stdoutLogEnabled` et `stdoutLogFile` les attributs de la `aspNetCore` élément sont définies. Tous les dossiers dans le `stdoutLogFile` chemin doit exister dans l’ordre pour le module créer le fichier journal. Une horodatage et extension de fichier sont ajoutés automatiquement lorsque le fichier journal est créé. Les journaux ne sont pas pivotés, sauf en cas de recyclage/redémarrage du processus. Il incombe à l’hébergeur pour limiter l’espace disque que les connexions à consommer. À l’aide de la `stdout` journal est recommandé uniquement pour résoudre les problèmes de démarrage d’application. N’utilisez pas le journal de stdout à des fins de journalisation de l’application générale. Pour la routine de journalisation dans une application ASP.NET Core, utilisez une bibliothèque de journalisation qui limite la taille du fichier journal et la faire pivoter des journaux. Pour plus d’informations, consultez [modules fournisseurs d’informations de tiers](xref:fundamentals/logging/index#third-party-logging-providers).

Nom du fichier journal est composé en ajoutant le timestamp, un ID de processus et une extension de fichier (*.log*) pour le dernier segment de la `stdoutLogFile` chemin d’accès (généralement *stdout*) délimités par des traits de soulignement. Si le `stdoutLogFile` chemin d’accès se termine par *stdout*, un journal pour une application avec un PID de 1934 créé sur 2/5/2018 à 19:42:32 porte le nom de fichier *stdout_20180205194132_1934.log*.

L’exemple suivant `aspNetCore` élément configure `stdout` journalisation pour une application hébergée dans Azure App Service. Un chemin d’accès local ou un chemin d’accès du partage réseau est acceptable pour la connexion locale. Vérifiez que l’identité de pool d’applications l’utilisateur a l’autorisation d’écrire dans le chemin d’accès fourni.

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

Consultez [Configuration avec web.config](#configuration-with-webconfig) pour obtenir un exemple de la `aspNetCore` élément dans le *web.config* fichier.

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>La configuration du proxy utilise le protocole HTTP et un jeton d’appariement

Le proxy créé entre le Module de base ASP.NET et Kestrel utilise le protocole HTTP. À l’aide de HTTP est une optimisation des performances, d’où le trafic entre le module et Kestrel a lieu sur une adresse de bouclage hors de l’interface réseau. Il n’existe aucun risque d’écoute clandestine le trafic entre le module et Kestrel à partir d’un emplacement sur le serveur.

Un jeton d’appariement est utilisé pour garantir que les demandes reçues par Kestrel sont traitées par IIS et qu’elles ne proviennent pas d’une autre source. Le jeton d’appariement est créé et défini dans une variable d’environnement (`ASPNETCORE_TOKEN`) par le module. Le jeton d’appariement est également défini dans un en-tête (`MSAspNetCoreToken`) sur toutes les demandes en proxy. L’intergiciel (middleware) IIS vérifie chaque demande qu’il reçoit pour confirmer que la valeur d’en-tête du jeton d’appariement correspond à la valeur de variable d’environnement. Si les valeurs de jeton ne correspondent pas, la demande est journalisée et rejetée. La variable d’environnement jeton appariement et le trafic entre le module et Kestrel ne sont pas accessibles à partir d’un emplacement sur le serveur. Sans connaître la valeur du jeton d’appariement, une personne malveillante ne peut pas soumettre de demandes qui contournent la vérification effectuée par l’intergiciel (middleware) IIS.

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>Configuration de Module ASP.NET Core avec un IIS partagée

Le programme d’installation du Module de base ASP.NET s’exécute avec les privilèges de le **système** compte. Étant donné que le compte système local ont ne modifie pas l’autorisation pour le chemin d’accès de partage utilisé par la Configuration partagée IIS, le programme d’installation rencontre une erreur d’accès refusé lorsque vous tentez de configurer les paramètres de module dans *applicationHost.config* sur le partage. Lorsque vous utilisez une Configuration partagée d’IIS, procédez comme suit :

1. Désactiver la Configuration partagée IIS.
1. Exécutez le programme d’installation.
1. Exporter la mise à jour *applicationHost.config* le partage de fichier.
1. Activer de nouveau la Configuration partagée IIS.

## <a name="module-version-and-hosting-bundle-installer-logs"></a>Version du module et l’hébergement des journaux du programme d’installation offre groupée

Pour déterminer la version de ASP.NET Core Module installé :

1. Sur le système hôte, accédez à *%windir%\System32\inetsrv*.
1. Recherchez le *aspnetcore.dll* fichier.
1. Cliquez sur le fichier et sélectionnez **propriétés** dans le menu contextuel.
1. Sélectionnez le **détails** onglet. Le **version du fichier** et **version du produit** représentent la version installée du module.

Les journaux de programme d’installation offre l’hébergement de Windows Server pour le module sont trouvent dans *C:\\utilisateurs\\%UserName%\\AppData\\Local\\Temp*. Le fichier est nommé *dd_DotNetCoreWinSvrHosting__\<timestamp > _000_AspNetCoreModule_x64.log*.

## <a name="module-schema-and-configuration-file-locations"></a>Emplacements des fichiers de module, de schéma et de configuration

### <a name="module"></a>Module

**IIS (x86/amd64) :**

   * %windir%\System32\inetsrv\aspnetcore.dll

   * %windir%\SysWOW64\inetsrv\aspnetcore.dll

**IIS Express (x86/amd64) :**

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

Vous trouverez les fichiers en recherchant des *aspnetcore.dll* dans les *applicationHost.config* fichier. Pour IIS Express, le *applicationHost.config* fichier n’existe pas par défaut. Le fichier est créé au  *\<application_root >\\.vs\\config* lors du démarrage d’un projet d’application web dans la solution Visual Studio.
