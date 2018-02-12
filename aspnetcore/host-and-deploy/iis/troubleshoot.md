---
title: "Résoudre les problèmes de base ASP.NET sur IIS"
author: guardrex
description: "Découvrez comment diagnostiquer les problèmes avec les déploiements d’applications ASP.NET Core Internet Information Services (IIS)."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: 65173e0101a17c64f4cde583e5bbb9fb0a9c7718
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/11/2018
---
# <a name="troubleshoot-aspnet-core-on-iis"></a>Résoudre les problèmes de base ASP.NET sur IIS

Par [Luke Latham](https://github.com/guardrex)

Cet article fournit des instructions sur la façon de diagnostiquer un ASP.NET Core problème de démarrage d’application lors de l’hébergement avec [Internet Information Services (IIS)](/iis). Les informations contenues dans cet article s’applique à l’hébergement dans IIS sur Windows Server et Windows Desktop.

Dans Visual Studio, un projet ASP.NET Core par défaut [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hébergement pendant le débogage. A *502.5 Échec d’un processus* qui se produit lorsque le débogage local peut être devez, à l’aide de l’avis de cette rubrique.

Rubriques de dépannage supplémentaires :

[Résoudre les problèmes liés à ASP.NET Core sur Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot)  
Bien que le Service de l’application utilise le [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) et IIS pour les applications de l’hôte, consultez la rubrique dédiée pour obtenir des instructions spécifiques au Service de l’application.

[Gestion des erreurs](xref:fundamentals/error-handling)  
Découvrez comment gérer les erreurs dans les applications ASP.NET Core pendant le développement sur un système local.

[Apprendre à déboguer avec Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger)  
Cette rubrique présente les fonctionnalités du débogueur Visual Studio.

## <a name="app-startup-errors"></a>Erreurs de démarrage d’application

**502.5 Échec du processus**  
Le processus de travail échoue. L’application ne démarre pas.

Le Module de base ASP.NET tente de démarrer le processus de travail, mais il ne démarre. La cause d’un échec de démarrage du processus peut généralement être déterminée à partir des entrées dans le [journal des événements Application](#application-event-log) et [journal stdout de Module de base ASP.NET](#aspnet-core-module-stdout-log).

Le *502.5 Échec d’un processus* page d’erreur est retournée lorsqu’une erreur de configuration d’hébergement ou application entraîne l’échec du processus de travail :

![Fenêtre de navigateur affichant la page d’échec d’un processus 502.5](troubleshoot/_static/process-failure-page.png)

**Erreur de serveur interne 500**  
L’application démarre, mais une erreur empêche le serveur de répondre à la demande.

Cette erreur se produit dans le code de l’application lors du démarrage ou lors de la création d’une réponse. La réponse ne peut contenir aucun contenu ou la réponse ne peut apparaître comme un *500 Erreur interne au serveur* dans le navigateur. Le journal des événements indique généralement que l’application a démarré normalement. Du point de vue du serveur, qui est correct. L’application n’a pas démarré, mais il ne peut pas générer une réponse valide. [Exécuter l’application à une invite de commandes](#run-the-app-at-a-command-prompt) sur le serveur ou [activer le journal de stdout ASP.NET Core Module](#aspnet-core-module-stdout-log) pour résoudre le problème.

**Réinitialisation de la connexion**

Si une erreur se produit après l’envoi des en-têtes, il est trop tard pour le serveur envoyer un **500 Erreur interne au serveur** lorsqu’une erreur se produit. Cela se produit souvent quand une erreur se produit pendant la sérialisation d’objets complexes d’une réponse. Ce type d’erreur apparaît comme un *réinitialisation de la connexion* erreur sur le client. [Journal des applications](xref:fundamentals/logging/index) peuvent aider à résoudre ces types d’erreurs.

## <a name="default-startup-limits"></a>Limites de démarrage par défaut

Le Module de base ASP.NET est configuré avec une valeur par défaut *startupTimeLimit* de 120 secondes. Lors de la gauche au niveau de la valeur par défaut, une application peut prendre jusqu'à deux minutes de démarrer avant que le module connecte à un échec d’un processus. Pour plus d’informations sur la configuration du module, consultez [attributs de l’élément aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

## <a name="troubleshoot-app-startup-errors"></a>Résoudre les erreurs de démarrage d’application

### <a name="application-event-log"></a>Journal des événements de l'application

Accéder au journal des événements Application :

1. Ouvrez le menu Démarrer, recherchez **Observateur d’événements**, puis sélectionnez le **Observateur d’événements** application.
1. Dans **Observateur d’événements**, ouvrez le **journaux Windows** nœud.
1. Sélectionnez **Application** pour ouvrir le journal des événements.
1. Rechercher les erreurs liées à l’application défectueuse. Les erreurs ont une valeur de *IIS AspNetCore Module* ou *IIS Express AspNetCore Module* dans les *Source* colonne.

### <a name="run-the-app-at-a-command-prompt"></a>Exécuter l’application à une invite de commandes

De nombreuses erreurs de démarrage ne produisent pas des informations utiles dans le journal des événements. Vous pouvez trouver la cause de certaines erreurs en exécutant l’application à une invite de commandes sur le système hôte.

**Déploiement d’infrastructure dépendant**

Si l’application est un [dépendant du framework déploiement](/dotnet/core/deploying/#framework-dependent-deployments-fdd):

1. À l’invite de commandes, accédez au dossier de déploiement, et exécuter l’application en exécutant l’assembly de l’application avec *dotnet.exe*. Dans la commande suivante, remplacez le nom de l’assembly de l’application pour \<assembly_name > : `dotnet .\<assembly_name>.dll`.
1. La sortie de l’application, en affichant les erreurs, de console est écrite dans la fenêtre de console.
1. Si les erreurs se produisent lors d’une demande à l’application, effectuez une demande à l’hôte et le port sur lequel Kestrel écoute. À l’aide de l’hôte par défaut et post, faites une demande pour `http://localhost:5000/`. Si l’application répond généralement à l’adresse de point de terminaison Kestrel, le problème est probablement lié à la configuration de proxy inverse et moins probable au sein de l’application.

**Déploiement autonome**

Si l’application est un [déploiement autonome](/dotnet/core/deploying/#self-contained-deployments-scd):

1. À l’invite de commandes, accédez au dossier de déploiement et exécutez le fichier exécutable de l’application. Dans la commande suivante, remplacez le nom de l’assembly de l’application pour \<assembly_name > : `<assembly_name>.exe`.
1. La sortie de l’application, en affichant les erreurs, de console est écrite dans la fenêtre de console.
1. Si les erreurs se produisent lors d’une demande à l’application, effectuez une demande à l’hôte et le port sur lequel Kestrel écoute. À l’aide de l’hôte par défaut et post, faites une demande pour `http://localhost:5000/`. Si l’application répond généralement à l’adresse de point de terminaison Kestrel, le problème est probablement lié à la configuration de proxy inverse et moins probable au sein de l’application.

### <a name="aspnet-core-module-stdout-log"></a>Journal de stdout de Module de base ASP.NET

Pour activer et afficher les journaux de stdout :

1. Accédez au dossier de déploiement du site sur le système hôte.
1. Si le *journaux* dossier n’est pas présent, créez le dossier. Pour obtenir des instructions sur l’activation de MSBuild créer le *journaux* dossier dans le déploiement automatique, voir la [structure de répertoires](xref:host-and-deploy/directory-structure) rubrique.
1. Modifier la *web.config* fichier. Définir **stdoutLogEnabled** à `true` et modifiez le **stdoutLogFile** chemin d’accès pour pointer vers le *journaux* dossier (par exemple, `.\logs\stdout`). `stdout`dans le chemin d’accès est le préfixe de nom de fichier journal. Un horodatage, un id de processus et une extension de fichier sont ajoutés automatiquement lorsque le journal est créé. À l’aide de `stdout` en tant que préfixe du nom de fichier, un fichier journal par défaut est nommé *stdout_20180205184032_5412.log*. 
1. Enregistrer les mises à jour *web.config* fichier.
1. Effectuer une demande à l’application.
1. Accédez à la *journaux* dossier. Recherchez et ouvrez le journal de stdout plus récent.
1. Analysez le journal des erreurs.

**Important !** Désactiver la journalisation de stdout lors de la résolution des problèmes sont terminée.

1. Modifier la *web.config* fichier.
1. Définissez **stdoutLogEnabled** à `false`.
1. Enregistrez le fichier.

> [!WARNING]
> Échec pour désactiver le journal de stdout risque d’échec de l’application ou le serveur. Il existe aucune limite quant à la taille du fichier journal ou le nombre de fichiers journaux créés.
>
> Pour la routine de journalisation dans une application ASP.NET Core, utilisez une bibliothèque de journalisation qui limite la taille du fichier journal et la faire pivoter des journaux. Pour plus d’informations, consultez [modules fournisseurs d’informations de tiers](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="enabling-the-developer-exception-page"></a>L’activation de la Page d’Exception de développeur

Le `ASPNETCORE_ENVIRONMENT` [variable d’environnement peut être ajoutée au fichier web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) pour exécuter l’application dans l’environnement de développement. Tant que l’environnement n’est pas substitué dans le démarrage de l’application par `UseEnvironment` sur le Générateur de l’hôte, définissant la variable d’environnement permet la [développeur Exception Page](xref:fundamentals/error-handling) apparaissent lorsque l’application est exécutée.

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

Définition de la variable d’environnement `ASPNETCORE_ENVIRONMENT` est recommandée uniquement pour une utilisation sur les serveurs qui ne sont pas exposés à Internet de test et de mise en lots. Supprimer la variable d’environnement à partir de la *web.config* fichier après la résolution des problèmes. Pour plus d’informations sur la définition des variables d’environnement dans *web.config*, consultez [élément enfant d’environmentVariables d’aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).

## <a name="common-startup-errors"></a>Erreurs courantes de démarrage 

Consultez le [référence des erreurs commune ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference). La plupart des problèmes courants qui empêchent le démarrage de l’application est traitée dans la rubrique de référence.

## <a name="slow-or-hanging-app"></a>Application lente ou bloquée

Lorsqu’une application répond lentement ou se bloque sur demande, obtenir et analyser un [fichier dump](/visualstudio/debugger/using-dump-files). Fichiers de vidage peuvent être obtenus à l’aide des outils suivants :

* [ProcDump](/sysinternals/downloads/procdump)
* [DebugDiag](https://www.microsoft.com/download/details.aspx?id=49924)
* WinDbg : [télécharger les outils de débogage pour Windows](https://developer.microsoft.com/windows/hardware/download-windbg), [WinDbg d’à l’aide de débogage](/windows-hardware/drivers/debugger/debugging-using-windbg)

## <a name="remote-debugging"></a>Débogage distant

Consultez [Core d’ASP.NET déboguer à distance sur un ordinateur IIS distant dans Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) dans la documentation de Visual Studio.

## <a name="application-insights"></a>Application Insights

[Application Insights](/azure/application-insights/) fournit les données de télémétrie des applications hébergées par IIS, y compris la journalisation des erreurs et les fonctions de rapport. Application Insights peuvent uniquement générer des rapports sur les erreurs qui surviennent après le démarrage de l’application lors de la disponibilité des fonctionnalités de journalisation de l’application. Pour plus d’informations, consultez [Application Insights pour ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).

## <a name="additional-troubleshooting-advice"></a>Conseils de dépannage supplémentaires

Parfois, une application opérationnelle échoue immédiatement après la mise à niveau soit du SDK .NET Core sur les versions de machine ou un package de développement au sein de l’application. Dans certains cas, les packages incohérents peuvent bloquer une application quand vous effectuez des mises à niveau majeures. La plupart de ces problèmes permettre être résolue en suivant ces instructions :

1. Supprimer le *bin* et *obj* dossiers.
1. Désactivez le package met en cache à *% USERPROFILE%\\.nuget\\packages* et *%LocalAppData%\\Nuget\\v3-cache*.
1. Restaurez et régénérez le projet.
1. Vérifiez que le déploiement préalable sur le serveur a été supprimé complètement avant de redéployer l’application.

> [!TIP]
> Un moyen pratique pour effacer les caches de package consiste à exécuter `dotnet nuget locals all --clear` à partir d’une invite de commandes.
> 
> Effacer les caches de package peut également être accompli en utilisant le [nuget.exe](https://www.nuget.org/downloads) outil et l’exécution de la commande `nuget locals all -clear`. *NuGet.exe* n’est pas une installation fournie avec le système d’exploitation de bureau Windows et doivent être obtenues séparément à partir de la [site Web de NuGet](https://www.nuget.org/downloads).

## <a name="additional-resources"></a>Ressources supplémentaires

* [Présentation de la gestion des erreurs dans ASP.NET Core](xref:fundamentals/error-handling)
* [Informations de référence sur les erreurs courantes pour Azure App Service et IIS avec ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Informations de référence sur la configuration du Module ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Résoudre les problèmes liés à ASP.NET Core sur Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot)
