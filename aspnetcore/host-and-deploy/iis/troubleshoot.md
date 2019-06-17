---
title: Résoudre les problèmes liés à ASP.NET Core sur IIS
author: guardrex
description: Découvrez comment diagnostiquer les problèmes liés aux déploiements Internet Information Services (IIS) d’applications ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/28/2019
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: cb42a262c89c27fa350e936184f8ddb3a02788f0
ms.sourcegitcommit: 335a88c1b6e7f0caa8a3a27db57c56664d676d34
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/12/2019
ms.locfileid: "67034743"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a>Résoudre les problèmes liés à ASP.NET Core sur IIS

Par [Luke Latham](https://github.com/guardrex)

Cet article fournit des instructions sur la façon de diagnostiquer un problème de démarrage d’application ASP.NET Core dans le cas d’un hébergement avec [Internet Information Services (IIS)](/iis). Les informations contenues dans cet article s’appliquent à l’hébergement dans IIS sur Windows Server et Windows Desktop.

::: moniker range=">= aspnetcore-2.2"

Dans Visual Studio, un projet ASP.NET Core est par défaut hébergé sur [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) pendant une opération de débogage. Vous pouvez suivre les conseils indiqués dans cette rubrique pour résoudre un *échec de processus 502.5* ou un *échec de démarrage 500.30* qui se produit pendant un débogage local.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Dans Visual Studio, un projet ASP.NET Core est par défaut hébergé sur [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) pendant une opération de débogage. Vous pouvez suivre les conseils indiqués dans cette rubrique pour résoudre un *échec de processus 502.5* qui se produit pendant un débogage local.

::: moniker-end

Rubriques de dépannage supplémentaires :

<xref:host-and-deploy/azure-apps/troubleshoot> Bien qu’App Service utilise le [module ASP.NET Core](xref:host-and-deploy/aspnet-core-module) et IIS pour héberger des applications, consultez la rubrique dédiée pour obtenir des instructions qui concernent spécifiquement App Service.

<xref:fundamentals/error-handling> Découvrez comment gérer les erreurs dans les applications ASP.NET Core pendant le développement sur un système local.

[Apprendre à déboguer avec Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger) Cette rubrique présente les fonctionnalités du débogueur Visual Studio.

[Débogage avec Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) Découvrez la prise en charge du débogage intégrée à Visual Studio Code.

## <a name="app-startup-errors"></a>Erreurs de démarrage de l’application

### <a name="5025-process-failure"></a>Échec de processus 502.5

Le processus de travail échoue. L’application ne démarre pas.

Le module ASP.NET Core tente, en vain, de démarrer le processus dotnet backend. Vous pouvez généralement déterminer la cause d’un échec de démarrage du processus à partir des entrées du [Journal des événements de l’application](#application-event-log) et du [journal stdout du module ASP.NET Core](#aspnet-core-module-stdout-log).

Une condition d’échec courante est une application mal configurée qui cible une version du framework partagé ASP.NET Core non présente. Vérifiez les versions du framework partagé ASP.NET Core qui sont installées sur l’ordinateur cible. Le *framework partagé* est le jeu d’assemblys (fichiers *.dll*) qui sont installés sur l’ordinateur et référencés par un métapaquet comme `Microsoft.AspNetCore.App`. La référence de métapaquet peut spécifier une version minimale requise. Pour plus d’informations, consultez [Le framework partagé](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).

La page d’erreur d’un *échec de processus 502.5* est retournée quand une erreur de configuration d’hébergement ou d’application entraîne l’échec du processus de travail :

![Fenêtre de navigateur affichant la page d’échec de processus 502.5](troubleshoot/_static/process-failure-page.png)

::: moniker range="= aspnetcore-2.2"

### <a name="50030-in-process-startup-failure"></a>500.30 - Échec du démarrage in-process

Le processus de travail échoue. L’application ne démarre pas.

Le module ASP.NET Core tente, en vain, de démarrer le CLR .NET Core in-process. Vous pouvez généralement déterminer la cause d’un échec de démarrage du processus à partir des entrées du [Journal des événements de l’application](#application-event-log) et du [journal stdout du module ASP.NET Core](#aspnet-core-module-stdout-log).

Une condition d’échec courante est une application mal configurée qui cible une version du framework partagé ASP.NET Core non présente. Vérifiez les versions du framework partagé ASP.NET Core qui sont installées sur l’ordinateur cible.

### <a name="5000-in-process-handler-load-failure"></a>500.0 - Échec de chargement du gestionnaire in-process

Le processus de travail échoue. L’application ne démarre pas.

Le module ASP.NET Core ne peut pas trouver le CLR .NET Core et le gestionnaire de requêtes in-process (*aspnetcorev2_inprocess.dll*). Vérifiez que :

* l’application cible le package NuGet [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) ou le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ;
* la version du framework partagé ASP.NET Core que l’application cible est installée sur l’ordinateur cible.

### <a name="5000-out-of-process-handler-load-failure"></a>500.0 - Échec de chargement du gestionnaire out-of-process

Le processus de travail échoue. L’application ne démarre pas.

Le module ASP.NET Core ne peut pas trouver le gestionnaire de requêtes d’hébergement out-of-process. Vérifiez que le fichier *aspnetcorev2_outofprocess.dll* est présent dans un sous-dossier en regard de *aspnetcorev2.dll*.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="50031-ancm-failed-to-find-native-dependencies"></a>500.31 - Échec de la recherche de dépendances natives par ANCM

Le processus de travail échoue. L’application ne démarre pas.

Le module ASP.NET Core tente de démarrer le runtime .NET Core in-process, mais sans succès. La cause la plus courante de cet échec de démarrage est liée à la non-installation du runtime `Microsoft.NETCore.App` ou `Microsoft.AspNetCore.App`. Si l’application est déployée pour cibler ASP.NET Core 3.0, et si cette version n’existe pas sur la machine, cette erreur se produit. Voici un exemple de message d’erreur :

```
The specified framework 'Microsoft.NETCore.App', version '3.0.0' was not found.
  - The following frameworks were found:
      2.2.1 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview5-27626-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27713-13 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27714-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27723-08 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
```

Le message d’erreur liste toutes les versions installées de .NET Core ainsi que la version demandée par l’application. Pour corriger cette erreur, vous avez le choix entre plusieurs possibilités :

* Installez la version appropriée de .NET Core sur la machine.
* Changez l’application pour cibler une version de .NET Core présente sur la machine.
* Publiez l’application sous forme de [déploiement autonome](/dotnet/core/deploying/#self-contained-deployments-scd).

Durant l’exécution en phase de développement (la variable d’environnement `ASPNETCORE_ENVIRONMENT` a la valeur `Development`), l’erreur spécifique est écrite dans la réponse HTTP. La cause d’un échec de démarrage du processus se trouve également dans le [Journal des événements de l’application](#application-event-log).

### <a name="50032-ancm-failed-to-load-dll"></a>500.32 - Échec du chargement de la dll par ANCM

Le processus de travail échoue. L’application ne démarre pas.

Le plus souvent, cette erreur provient du fait que l’application est publiée pour une architecture de processeur incompatible. Si le processus de travail s’exécute en tant qu’application 32 bits et si l’application a été publiée pour cibler une architecture 64 bits, cette erreur se produit.

Pour corriger cette erreur, vous avez le choix entre plusieurs possibilités :

* Republiez l’application pour la même architecture de processeur que celle du processus de travail.
* Publiez l’application sous forme de [déploiement dépendant du framework](/dotnet/core/deploying/#framework-dependent-executables-fde).

### <a name="50033-ancm-request-handler-load-failure"></a>500.33 - Échec du chargement du gestionnaire de requêtes ANCM

Le processus de travail échoue. L’application ne démarre pas.

L’application n’a pas référencé le framework `Microsoft.AspNetCore.App`. Seules les applications ciblant le framework `Microsoft.AspNetCore.App` peuvent être hébergées par le module ASP.NET Core.

Pour corriger cette erreur, vérifiez que l’application cible le framework `Microsoft.AspNetCore.App`. Examinez le fichier `.runtimeconfig.json` pour vérifier le framework ciblé par l’application.

### <a name="50034-ancm-mixed-hosting-models-not-supported"></a>500.34 - Modèles d’hébergement mixtes ANCM non pris en charge

Le processus de travail ne peut pas exécuter à la fois une application in-process et une application out-of-process dans le même processus.

Pour corriger cette erreur, exécutez les applications dans des pools d’applications IIS distincts.

### <a name="50035-ancm-multiple-in-process-applications-in-same-process"></a>500.35 - Applications in-process multiples dans le même processus ANCM

Le processus de travail ne peut pas exécuter à la fois une application in-process et une application out-of-process dans le même processus.

Pour corriger cette erreur, exécutez les applications dans des pools d’applications IIS distincts.

### <a name="50036-ancm-out-of-process-handler-load-failure"></a>500.36 - Échec de chargement du gestionnaire out-of-process ANCM

Le gestionnaire de requêtes out-of-process, *aspnetcorev2_outofprocess.dll*, ne se trouve pas aux côtés du fichier *aspnetcorev2.dll*. Cela indique une installation endommagée du module ASP.NET Core.

Pour corriger cette erreur, réparez l’installation du [bundle d’hébergement .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (pour IIS) ou Visual Studio (pour IIS Express).

### <a name="50037-ancm-failed-to-start-within-startup-time-limit"></a>500.37 - Échec du démarrage d’ANCM dans le délai imparti

ANCM n’a pas pu démarrer dans le délai imparti spécifié. Par défaut, le délai d’expiration est de 120 secondes.

Cette erreur peut se produire quand un grand nombre d’applications démarrent sur la même machine. Recherchez les pics d’utilisation du processeur/de la mémoire sur le serveur durant le démarrage. Vous devrez peut-être décaler le processus de démarrage de plusieurs applications.

### <a name="50030-in-process-startup-failure"></a>500.30 - Échec du démarrage in-process

Le processus de travail échoue. L’application ne démarre pas.

Le module ASP.NET Core tente de démarrer le runtime .NET Core in-process, mais sans succès. Vous pouvez généralement déterminer la cause d’un échec de démarrage de processus à partir des entrées du [Journal des événements de l’application](#application-event-log) et du [journal stdout du module ASP.NET Core](#aspnet-core-module-stdout-log).

### <a name="5000-in-process-handler-load-failure"></a>500.0 - Échec de chargement du gestionnaire in-process

Le processus de travail échoue. L’application ne démarre pas.

Une erreur inconnue s’est produite durant le chargement des composants du module ASP.NET Core. Effectuez l’une des actions suivantes :

* Contactez le [Support Microsoft](https://support.microsoft.com/oas/default.aspx?prid=15832) (sélectionnez **Outils de développement**, puis **ASP.NET Core**).
* Posez une question sur Stack Overflow.
* Signalez un problème sur notre [dépôt GitHub](https://github.com/aspnet/AspNetCore).

::: moniker-end

### <a name="500-internal-server-error"></a>Erreur de serveur interne 500

L’application démarre, mais une erreur empêche le serveur de répondre à la requête.

Cette erreur se produit dans le code de l’application pendant le démarrage ou durant la création d’une réponse. La réponse peut être dépourvue de contenu ou apparaître sous la forme d’une *erreur de serveur interne 500* dans le navigateur. Le Journal des événements de l’application indique généralement qu’elle a démarré normalement. Du point de vue du serveur, c’est exact. L’application a démarré, mais elle ne peut pas générer de réponse valide. [Exécutez l’application depuis une invite de commandes](#run-the-app-at-a-command-prompt) sur le serveur ou [activez le journal stdout du module ASP.NET Core](#aspnet-core-module-stdout-log) pour résoudre le problème.

### <a name="failed-to-start-application-errorcode-0x800700c1"></a>Échec du démarrage de l’application (code d’erreur « 0x800700c1 »)

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

L’application n’a pas pu démarrer, car l’assembly de l’application ( *.dll*) n’a pas pu être chargé.

Cette erreur se produit lorsqu’il existe une incompatibilité au niveau du nombre de bits entre l’application publiée et le processus w3wp/iisexpress.

Vérifiez que le paramètre 32 bits du pool d’applications est correct :

1. Sélectionnez le pool d’applications parmi les **Pools d’applications** IIS Manager.
1. Sélectionnez **Paramètres avancés** sous **Modifier le pool d’applications** dans le volet **Actions**.
1. Définissez **Activer les applications 32 bits** :
   * Si vous déployez une application 32 bits (x86), définissez la valeur sur `True`.
   * Si vous déployez une application 64 bits (x64), définissez la valeur sur `False`.

### <a name="connection-reset"></a>Réinitialisation de la connexion

Si une erreur se produit après l’envoi des en-têtes, il est trop tard pour que le serveur puisse envoyer une **erreur de serveur interne 500**. C’est souvent le cas quand une erreur se produit pendant la sérialisation des objets complexes d’une réponse. Ce type d’erreur apparaît en tant qu’erreur de *réinitialisation de la connexion* sur le client. La [journalisation des applications](xref:fundamentals/logging/index) peut aider à le résoudre.

## <a name="default-startup-limits"></a>Limites de démarrage par défaut

Le module ASP.NET Core est configuré avec une valeur *startupTimeLimit* par défaut de 120 secondes. Quand cette valeur par défaut est conservée, une application peut mettre jusqu’à deux minutes à démarrer avant que le module ne consigne un échec de processus. Pour plus d’informations sur la configuration du module, voir [Attributs de l’élément aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

## <a name="troubleshoot-app-startup-errors"></a>Résoudre les erreurs de démarrage des applications

### <a name="enable-the-aspnet-core-module-debug-log"></a>Activer le journal de débogage du module ASP.NET Core

Ajoutez les paramètres de gestionnaire suivants au fichier *web.config* de l’application pour activer les journaux de débogage du module ASP.NET Core :

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

Vérifiez que le chemin spécifié pour le journal existe, et que l’identité du pool d’applications dispose des autorisations en écriture pour l’emplacement.

### <a name="application-event-log"></a>Journal des événements de l'application

Accédez au Journal des événements de l’application :

1. Ouvrez le menu Démarrer, recherchez **Observateur d’événements**, puis sélectionnez l’application **Observateur d’événements**.
1. Dans **Observateur d’événements**, ouvrez le nœud **Journaux Windows**.
1. Sélectionnez **Application** pour ouvrir le Journal des événements de l’application.
1. Recherchez les erreurs liées à l’application défectueuse. Les erreurs sont signalées par une valeur *IIS AspNetCore Module* (Module AspNetCore IIS) ou *IIS Express AspNetCore Module* (Module AspNetCore IIS Express) dans la colonne *Source*.

### <a name="run-the-app-at-a-command-prompt"></a>Exécuter l’application depuis une invite de commandes

De nombreuses erreurs de démarrage ne produisent pas d’informations utiles dans le Journal des événements de l’application. Vous pouvez trouver la cause de certaines erreurs en exécutant l’application depuis une invite de commandes sur le système hôte.

#### <a name="framework-dependent-deployment"></a>Déploiement dépendant du framework

Si l’application est un [déploiement dépendant du framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) :

1. Depuis une invite de commandes, accédez au dossier de déploiement et exécutez l’application en exécutant l’assembly de l’application avec *dotnet.exe*. Dans la commande suivante, substituez le nom de l’assembly de l’application à \<assembly_name>: `dotnet .\<assembly_name>.dll`.
1. La sortie de console de l’application est écrite dans la fenêtre de console, affichant toutes les erreurs éventuelles.
1. Si les erreurs se produisent pendant qu’une requête est adressée à l’application, effectuez une requête en direction de l’hôte et du port sur lequel Kestrel écoute. À l’aide de l’hôte et du port par défaut, faites une requête en direction de `http://localhost:5000/`. Si l’application répond normalement à l’adresse de point de terminaison Kestrel, le problème est probablement lié à la configuration de l’hébergement plutôt qu’à l’application.

#### <a name="self-contained-deployment"></a>Déploiement autonome

Si l’application est un [déploiement autonome](/dotnet/core/deploying/#self-contained-deployments-scd) :

1. Depuis une invite de commandes, accédez au dossier de déploiement et exécutez l’exécutable de l’application. Dans la commande suivante, substituez le nom de l’assembly de l’application à \<assembly_name>: `<assembly_name>.exe`.
1. La sortie de console de l’application est écrite dans la fenêtre de console, affichant toutes les erreurs éventuelles.
1. Si les erreurs se produisent pendant qu’une requête est adressée à l’application, effectuez une requête en direction de l’hôte et du port sur lequel Kestrel écoute. À l’aide de l’hôte et du port par défaut, faites une requête en direction de `http://localhost:5000/`. Si l’application répond normalement à l’adresse de point de terminaison Kestrel, le problème est probablement lié à la configuration de l’hébergement plutôt qu’à l’application.

### <a name="aspnet-core-module-stdout-log"></a>Journal stdout du module ASP.NET Core

Pour activer et afficher les journaux stdout :

1. Accédez au dossier de déploiement du site sur le système hôte.
1. Si le dossier *logs* n’est pas présent, créez-le. Pour obtenir des instructions sur l’activation de MSBuild et créer le dossier *logs* dans le déploiement automatiquement, consultez la rubrique [Structure des répertoires](xref:host-and-deploy/directory-structure).
1. Modifiez le fichier *web.config*. Définissez **stdoutLogEnabled** sur `true` et faites pointer le chemin **stdoutLogFile** vers le dossier *logs* (par exemple, `.\logs\stdout`). Dans le chemin, `stdout` est le préfixe du nom du fichier journal. Un horodatage, un ID de processus et une extension de fichier sont ajoutés automatiquement quand le journal est créé. Si `stdout` est utilisé comme préfixe du nom de fichier, un fichier journal classique porte le nom *stdout_20180205184032_5412.log*.
1. Vérifiez que l’identité de votre pool d’applications dispose des autorisations d’écriture sur le dossier *logs*.
1. Enregistrez le fichier *web.config* mis à jour.
1. Adressez une requête à l’application.
1. Accédez au dossier *logs*. Recherchez et ouvrez le journal stdout le plus récent.
1. Déterminez si le journal contient des erreurs.

> [!IMPORTANT]
> Désactivez la journalisation stdout une fois la résolution des problèmes effectuée.

1. Modifiez le fichier *web.config*.
1. Définissez **stdoutLogEnabled** sur `false`.
1. Enregistrez le fichier.

> [!WARNING]
> Si vous ne désactivez pas le journal stdout, l’application ou le serveur risque d’échouer. Il n’existe aucune limite quant à la taille du fichier journal ou au nombre de fichiers journaux créés.
>
> Pour les opérations de journalisation courantes dans une application ASP.NET Core, utilisez une bibliothèque de journalisation qui limite la taille du fichier journal et applique une rotation aux journaux. Pour plus d’informations, voir [Fournisseurs de journalisation tiers](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="enable-the-developer-exception-page"></a>Afficher la page d’exception de développeur

Vous pouvez ajouter la [variable d’environnement `ASPNETCORE_ENVIRONMENT` au fichier web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) pour exécuter l’application dans l’environnement de développement. Tant que l’environnement n’est pas substitué dans le démarrage de l’application par `UseEnvironment` sur le générateur de l’hôte, la définition de la variable d’environnement permet à la [page d’exception de développeur](xref:fundamentals/error-handling) d’apparaître quand l’application est exécutée.

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout"
      hostingModel="InProcess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

La définition de la variable d’environnement `ASPNETCORE_ENVIRONMENT` est recommandée uniquement en vue d’une utilisation sur les serveurs de préproduction et de test qui ne sont pas exposés à Internet. Supprimez la variable d’environnement du fichier *web.config* une fois la résolution des problèmes effectuée. Pour plus d’informations sur la définition des variables d’environnement dans *web.config*, consultez la section sur [l’élément enfant environmentVariables d’aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).

## <a name="common-startup-errors"></a>Erreurs de démarrage courantes

Consultez <xref:host-and-deploy/azure-iis-errors-reference>. La plupart des problèmes courants qui empêchent le démarrage de l’application sont traités dans la rubrique de référence.

## <a name="obtain-data-from-an-app"></a>Obtenir des données à partir d’une application

Si une application est capable de répondre aux requêtes, obtenez des informations sur une requête, une connexion et d’autres informations supplémentaires à partir d’une application à l’aide de l’intergiciel en ligne terminal. Pour obtenir des informations supplémentaires ainsi qu'un code d'exemple, consultez <xref:test/troubleshoot#obtain-data-from-an-app>.

## <a name="create-a-dump"></a>Créer un fichier dump

Un fichier *dump* est un instantané de la mémoire système et peut aider à déterminer la cause de l’arrêt d’une application, d’un échec de démarrage ou de la lenteur d’une application.

### <a name="app-crashes-or-encounters-an-exception"></a>L’application cesse de fonctionner ou rencontre une exception

Obtenez un fichier dump et analysez-le depuis le [Rapport d'erreurs Windows](/windows/desktop/wer/windows-error-reporting) :

1. Créez un dossier pour accueillir les fichiers d’image mémoire dans `c:\dumps`. Le pool d’application doit disposer des accès d’écriture dans le dossier.
1. Exécutez le [script PowerShell EnableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1) :
   * Si l’application utilise le [modèle d’hébergement in-process](xref:host-and-deploy/iis/index#in-process-hosting-model), exécutez le script pour *w3wp.exe* :

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * Si l’application utilise le [modèle d’hébergement out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model), exécutez le script pour *dotnet.exe* :

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. Exécutez l’application en reproduisant les conditions ayant entraîné l’incident.
1. Une fois l’incident survenu, exécutez le [script PowerShell DisableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1) :
   * Si l’application utilise le [modèle d’hébergement in-process](xref:host-and-deploy/iis/index#in-process-hosting-model), exécutez le script pour *w3wp.exe* :

     ```console
     .\DisableDumps w3wp.exe
     ```

   * Si l’application utilise le [modèle d’hébergement out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model), exécutez le script pour *dotnet.exe* :

     ```console
     .\DisableDumps dotnet.exe
     ```

Après l’arrêt de l’application et après avoir terminé la collection dump, l’application peut être fermée normalement. Le script PowerShell configure le rapport d’erreurs Windows pour collecter jusqu'à cinq fichiers dump par application.

> [!WARNING]
> Les fichiers dump d’incident peuvent occuper plus d’espace disque (jusqu’à plusieurs gigaoctets par fichier).

### <a name="app-hangs-fails-during-startup-or-runs-normally"></a>L’application se bloque, ne démarre pas ou s’exécute normalement

Lorsqu’une application *se bloque* (ne répond plus mais ne s’arrête pas), ne démarre pas ou s’exécute normalement, consultez [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) (Fichiers dump en mode utilisateur : choisir le meilleur outil) afin de sélectionner l’outil adapté pour produire le fichier dump.

### <a name="analyze-the-dump"></a>Analyser le fichier dump

Un fichier dump peut être analysé à l’aide de plusieurs approches. Pour plus d’informations, consultez [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file) (Analyser un fichier dump en mode utilisateur).

## <a name="remote-debugging"></a>Débogage distant

Consultez [Déboguer à distance ASP.NET Core sur un ordinateur IIS distant dans Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) dans la documentation de Visual Studio.

## <a name="application-insights"></a>Application Insights

[Application Insights](/azure/application-insights/) fournit des données de télémétrie des applications hébergées par IIS, y compris des fonctionnalités de journalisation des erreurs et de création de rapports. Application Insights peut uniquement générer des rapports sur les erreurs qui surviennent après le démarrage de l’application quand les fonctionnalités de journalisation de l’application sont disponibles. Pour plus d’informations, voir [Application Insights pour ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).

## <a name="additional-advice"></a>Conseils supplémentaires

Parfois, une application opérationnelle échoue après la mise à niveau du SDK .NET Core sur l’ordinateur de développement ou des versions de package dans l’application. Dans certains cas, les packages incohérents peuvent bloquer une application quand vous effectuez des mises à niveau majeures. Vous pouvez résoudre la plupart de ces problèmes en suivant les instructions suivantes :

1. Supprimez les dossiers *bin* et *obj*.
1. Effacez les caches de package aux emplacements *%UserProfile%\\.nuget\\packages* et *%LocalAppData%\\Nuget\\v3-cache*.
1. Restaurez et regénérez le projet.
1. Vérifiez que le déploiement préalable sur le serveur a été supprimé complètement avant de redéployer l’application.

> [!TIP]
> Un moyen pratique pour effacer les caches de package consiste à exécuter `dotnet nuget locals all --clear` à partir d’une invite de commandes.
>
> Vous pouvez également effacer les caches de package en utilisant l’outil [nuget.exe](https://www.nuget.org/downloads) et en exécutant la commande `nuget locals all -clear`. *NuGet.exe* n’étant pas une installation fournie avec le système d’exploitation de bureau Windows, il doit être obtenu séparément à partir du [site web de NuGet](https://www.nuget.org/downloads).

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:test/troubleshoot>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/azure-apps/troubleshoot>
