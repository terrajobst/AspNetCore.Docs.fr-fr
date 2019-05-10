---
title: Résoudre les problèmes liés à ASP.NET Core sur Azure App Service
author: guardrex
description: Découvrez comment diagnostiquer les problèmes liés aux déploiements ASP.NET Core sur Azure App Service.
ms.author: riande
ms.custom: mvc
ms.date: 03/06/2019
uid: host-and-deploy/azure-apps/troubleshoot
ms.openlocfilehash: 36c2bdfa585a0fd54ca93bf4c0edb4cf6f7d934a
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64886904"
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service"></a>Résoudre les problèmes liés à ASP.NET Core sur Azure App Service

Par [Luke Latham](https://github.com/guardrex)

[!INCLUDE [Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

Cet article donne des instructions pour diagnostiquer un problème de démarrage de l’application ASP.NET Core avec les outils de diagnostic d’Azure App Service. Pour obtenir des conseils de dépannage supplémentaires, consultez [Vue d’ensemble des diagnostics Azure App Service](/azure/app-service/app-service-diagnostics) et [Comment : Surveiller des applications dans Azure App Service](/azure/app-service/web-sites-monitor) dans la documentation Azure.

## <a name="app-startup-errors"></a>Erreurs de démarrage de l’application

**Échec de processus 502.5** Le processus de travail échoue. L’application ne démarre pas.

Le [module ASP.NET Core](xref:host-and-deploy/aspnet-core-module) tente, en vain, de démarrer le processus de travail. Un examen du Journal des événements de l’application permet souvent de résoudre ce type de problème. L’accès au journal est expliqué dans la section [Journal des événements de l’application](#application-event-log).

La page d’erreur *Échec de processus 502.5* est retournée quand une application mal configurée provoque l’échec du processus de travail :

![Fenêtre de navigateur affichant la page d’échec de processus 502.5](troubleshoot/_static/process-failure-page.png)

**Erreur de serveur interne 500**

L’application démarre, mais une erreur empêche le serveur de répondre à la requête.

Cette erreur se produit dans le code de l’application pendant le démarrage ou durant la création d’une réponse. La réponse peut être dépourvue de contenu ou apparaître sous la forme d’une *erreur de serveur interne 500* dans le navigateur. Le Journal des événements de l’application indique généralement qu’elle a démarré normalement. Du point de vue du serveur, c’est exact. L’application a démarré, mais elle ne parvient pas à générer une réponse valide. [Exécutez l’application dans la console Kudu](#run-the-app-in-the-kudu-console) ou [activez le journal stdout du module ASP.NET Core](#aspnet-core-module-stdout-log) pour résoudre le problème.

**Réinitialisation de la connexion**

Si une erreur se produit après l’envoi des en-têtes, il est trop tard pour que le serveur puisse envoyer une **erreur de serveur interne 500**. C’est souvent le cas quand une erreur se produit pendant la sérialisation des objets complexes d’une réponse. Ce type d’erreur apparaît en tant qu’erreur de *réinitialisation de la connexion* sur le client. La [journalisation des applications](xref:fundamentals/logging/index) peut aider à le résoudre.

## <a name="default-startup-limits"></a>Limites de démarrage par défaut

Le module ASP.NET Core est configuré avec une valeur *startupTimeLimit* par défaut de 120 secondes. Quand cette valeur par défaut est conservée, une application peut mettre jusqu’à deux minutes à démarrer avant que le module ne consigne un échec de processus. Pour plus d’informations sur la configuration du module, voir [Attributs de l’élément aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

## <a name="troubleshoot-app-startup-errors"></a>Résoudre les erreurs de démarrage des applications

### <a name="application-event-log"></a>Journal des événements de l'application

Pour accéder au Journal des événements de l’application, utilisez le panneau **Diagnostiquer et résoudre les problèmes** du Portail Azure :

1. Dans le Portail Azure, ouvrez l’application dans **App Services**.
1. Sélectionnez **Diagnostiquer et résoudre les problèmes**.
1. Sélectionnez le titre **Outils de diagnostic**.
1. Sous **Outils de support**, sélectionnez le bouton **Événements d’application**.
1. Examinez la dernière erreur fournie par l’entrée *IIS AspNetCoreModule* ou *IIS AspNetCoreModule V2* dans la colonne **Source**.

En dehors de la solution consistant à utiliser le panneau **Diagnostiquer et résoudre les problèmes**, vous pouvez examiner directement le fichier Journal des événements de l’application avec [Kudu](https://github.com/projectkudu/kudu/wiki) :

1. Ouvrez les **Outils avancés** dans la zone **Outils de développement**. Sélectionnez le bouton **Atteindre&rarr;**. La console Kudu s’ouvre dans un nouvel onglet ou une nouvelle fenêtre du navigateur.
1. Dans la barre de navigation en haut de la page, ouvrez **Console de débogage** et sélectionnez **CMD**.
1. Ouvrez le dossier **LogFiles**.
1. Sélectionnez l’icône en forme de crayon à côté du fichier *eventlog.xml*.
1. Examinez le journal. Allez en bas du journal pour voir les événements les plus récents.

### <a name="run-the-app-in-the-kudu-console"></a>Exécuter l’application dans la console Kudu

De nombreuses erreurs de démarrage ne produisent pas d’informations utiles dans le Journal des événements de l’application. Vous pouvez exécuter l’application dans la console d’exécution à distance [Kudu](https://github.com/projectkudu/kudu/wiki) pour détecter l’erreur :

1. Ouvrez les **Outils avancés** dans la zone **Outils de développement**. Sélectionnez le bouton **Atteindre&rarr;**. La console Kudu s’ouvre dans un nouvel onglet ou une nouvelle fenêtre du navigateur.
1. Dans la barre de navigation en haut de la page, ouvrez **Console de débogage** et sélectionnez **CMD**.

#### <a name="test-a-32-bit-x86-app"></a>Tester une application 32 bits (x86)

##### <a name="current-release"></a>Version actuelle

1. `cd d:\home\site\wwwroot`
1. Exécutez l’application :
   * Si l’application est un [déploiement dépendant du framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) :

     ```console
     dotnet .\{ASSEMBLY NAME}.dll
     ```

   * Si l’application est un [déploiement autonome](/dotnet/core/deploying/#self-contained-deployments-scd) :

     ```console
     {ASSEMBLY NAME}.exe
     ```

La sortie de console de l’application, affichant toutes les erreurs éventuelles, est transmise à la console Kudu.

##### <a name="framework-dependent-deployment-running-on-a-preview-release"></a>Déploiement dépendant du framework sur une préversion

*Installation de l’extension de site du runtime ASP.NET Core {VERSION} (x86) requise.*

1. `cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` est la version du runtime).
1. Exécutez l'application : `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`.

La sortie de console de l’application, affichant toutes les erreurs éventuelles, est transmise à la console Kudu.

#### <a name="test-a-64-bit-x64-app"></a>Tester une application 64 bits (x64)

##### <a name="current-release"></a>Version actuelle

* Si l’application est un [déploiement dépendant du framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) 64 bits (x64) :
  1. `cd D:\Program Files\dotnet`
  1. Exécutez l'application : `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`.
* Si l’application est un [déploiement autonome](/dotnet/core/deploying/#self-contained-deployments-scd) :
  1. `cd D:\home\site\wwwroot`
  1. Exécutez l'application : `{ASSEMBLY NAME}.exe`.

La sortie de console de l’application, affichant toutes les erreurs éventuelles, est transmise à la console Kudu.

##### <a name="framework-dependent-deployment-running-on-a-preview-release"></a>Déploiement dépendant du framework sur une préversion

*Installation de l’extension de site du runtime ASP.NET Core {VERSION} (x64) requise.*

1. `cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` est la version du runtime).
1. Exécutez l'application : `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`.

La sortie de console de l’application, affichant toutes les erreurs éventuelles, est transmise à la console Kudu.

### <a name="aspnet-core-module-stdout-log"></a>Journal stdout du module ASP.NET Core

Le journal stdout du module ASP.NET Core enregistre souvent des messages d’erreur utiles et absents du Journal des événements de l’application. Pour activer et afficher les journaux stdout :

1. Accédez au panneau **Diagnostiquer et résoudre les problèmes** du Portail Azure.
1. Sous **SÉLECTIONNER UNE CATÉGORIE DE PROBLÈME**, sélectionnez le bouton **Application web en panne**.
1. Sous **Solutions suggérées** > **Activer la redirection de journaux stdout**, sélectionnez le bouton **Ouvrir la console Kudu pour modifier web.config**.
1. Dans la **Console de diagnostic** Kudu, ouvrez les dossiers sur le chemin d’accès **site** > **wwwroot**. Faites défiler la page jusqu’à révéler le fichier *web.config* en bas de la liste.
1. Cliquez sur l’icône en forme de crayon à côté du fichier *web.config*.
1. Définissez **stdoutLogEnabled** sur `true` et remplacez le chemin **stdoutLogFile** par : `\\?\%home%\LogFiles\stdout`.
1. Sélectionnez **Enregistrer** pour enregistrer le fichier *web.config* mis à jour.
1. Adressez une demande à l’application.
1. Revenez sur le Portail Azure. Sélectionnez le panneau **Outils avancés** dans la zone **OUTILS DE DÉVELOPPEMENT**. Sélectionnez le bouton **Atteindre&rarr;**. La console Kudu s’ouvre dans un nouvel onglet ou une nouvelle fenêtre du navigateur.
1. Dans la barre de navigation en haut de la page, ouvrez **Console de débogage** et sélectionnez **CMD**.
1. Sélectionnez le dossier **LogFiles**.
1. Inspectez la colonne **Modifié** et sélectionnez l’icône en forme de crayon pour modifier le journal stdout avec la date de dernière modification.
1. Lorsque le fichier journal s’ouvre, l’erreur s’affiche.

Désactivez la journalisation stdout, une fois les problèmes résolus :

1. Dans la **Console de diagnostic** Kudu, revenez au chemin d’accès **site** > **wwwroot** pour faire apparaître le fichier *web.config*. Ouvrez à nouveau le fichier **web.config** en sélectionnant l’icône en forme de crayon.
1. Définissez **stdoutLogEnabled** sur `false`.
1. Sélectionnez **Enregistrer** pour enregistrer le fichier.

> [!WARNING]
> Si vous ne désactivez pas le journal stdout, l’application ou le serveur risque d’échouer. Il n’existe aucune limite quant à la taille du fichier journal ou au nombre de fichiers journaux créés. N’utilisez la journalisation stdout que pour résoudre les problèmes de démarrage de l’application.
>
> Pour la journalisation générale dans une application ASP.NET Core après le démarrage, utilisez une bibliothèque de journalisation qui limite la taille du fichier journal et applique une rotation aux journaux. Pour plus d’informations, voir [Fournisseurs de journalisation tiers](xref:fundamentals/logging/index#third-party-logging-providers).

::: moniker range=">= aspnetcore-2.2"

### <a name="aspnet-core-module-debug-log"></a>Journal de débogage du module ASP.NET Core

Le journal de débogage du module ASP.NET Core fournit une journalisation supplémentaire, plus approfondie, à partir du module ASP.NET Core. Pour activer et afficher les journaux stdout :

1. Pour activer le journal de diagnostic amélioré, effectuez l’une des opérations suivantes :
   * Suivez les instructions indiquées dans [Journaux de diagnostic améliorés](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) afin de configurer l’application pour une journalisation des diagnostics améliorée. Redéployez l’application.
   * Ajoutez le `<handlerSettings>` présenté dans [Journaux de diagnostic améliorés](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) au fichier *web.config* de l’application en production à l’aide de la console Kudu :
     1. Ouvrez les **Outils avancés** dans la zone **Outils de développement**. Sélectionnez le bouton **Atteindre&rarr;**. La console Kudu s’ouvre dans un nouvel onglet ou une nouvelle fenêtre du navigateur.
     1. Dans la barre de navigation en haut de la page, ouvrez **Console de débogage** et sélectionnez **CMD**.
     1. Ouvrez les dossiers sur le chemin d’accès **site** > **wwwroot**. Modifiez le fichier *web.config* en sélectionnant le bouton représentant un crayon. Ajoutez la section `<handlerSettings>` comme indiqué dans [Journaux de diagnostic améliorés](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs). Sélectionnez le bouton **Enregistrer**.
1. Ouvrez les **Outils avancés** dans la zone **Outils de développement**. Sélectionnez le bouton **Atteindre&rarr;**. La console Kudu s’ouvre dans un nouvel onglet ou une nouvelle fenêtre du navigateur.
1. Dans la barre de navigation en haut de la page, ouvrez **Console de débogage** et sélectionnez **CMD**.
1. Ouvrez les dossiers sur le chemin d’accès **site** > **wwwroot**. Si vous n’avez pas indiqué de chemin pour le fichier *aspnetcore-debug.log*, le fichier apparaît dans la liste. Si vous avez indiqué un chemin, accédez à l’emplacement du fichier journal.
1. Ouvrez le fichier journal à l’aide du bouton représentant un crayon à côté du nom de fichier.

Désactivez la journalisation du débogage, une fois la résolution des problèmes effectuée :

1. Pour désactiver la journalisation de débogage améliorée, effectuez l’une des opérations suivantes :
   * Supprimez `<handlerSettings>` du fichier *web.config* localement, puis redéployez l’application.
   * Utilisez la console Kudu pour modifier le fichier *web.config* et supprimer la section `<handlerSettings>`. Enregistrez le fichier.

> [!WARNING]
> Si le journal de débogage n’est pas désactivé, cela peut entraîner une défaillance de l’application ou du serveur. Il n’existe aucune limite à la taille du fichier journal. Utilisez uniquement la journalisation de débogage pour résoudre les problèmes de démarrage d’application.
>
> Pour la journalisation générale dans une application ASP.NET Core après le démarrage, utilisez une bibliothèque de journalisation qui limite la taille du fichier journal et applique une rotation aux journaux. Pour plus d’informations, voir [Fournisseurs de journalisation tiers](xref:fundamentals/logging/index#third-party-logging-providers).

::: moniker-end

## <a name="common-startup-errors"></a>Erreurs de démarrage courantes

Consultez <xref:host-and-deploy/azure-iis-errors-reference>. La plupart des problèmes courants qui empêchent le démarrage de l’application sont traités dans la rubrique de référence.

## <a name="slow-or-hanging-app"></a>Application lente ou bloquée

Si une application répond lentement ou se bloque sur une requête, voir les articles suivants :

* [Résoudre les problèmes de performances d’une application web lente dans Azure App Service](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [Utiliser l’extension de site Outil de diagnostic des incidents pour capturer une image mémoire des problèmes d’exception intermittente ou de performances sur Azure Web App](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/)

## <a name="remote-debugging"></a>Débogage distant

Consultez les rubriques suivantes :

* [Section de débogage à distance d’applications web de Résoudre les problèmes d’une application web dans Azure App Service avec Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) (documentation Azure)
* [Déboguer à distance ASP.NET Core sur IIS dans Azure dans Visual Studio 2017](/visualstudio/debugger/remote-debugging-azure) (documentation Visual Studio)

## <a name="application-insights"></a>Application Insights

[Application Insights](https://azure.microsoft.com/services/application-insights/) fournit des données de télémétrie des applications hébergées dans Azure App Service, y compris des fonctionnalités de journalisation des erreurs et de création de rapports. Application Insights ne peut générer des rapports que sur les erreurs qui surviennent après le démarrage de l’application quand les fonctionnalités de journalisation de l’application sont disponibles. Pour plus d’informations, voir [Application Insights pour ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).

## <a name="monitoring-blades"></a>Panneaux Monitoring

Les panneaux Monitoring offrent une expérience de résolution des erreurs différente des méthodes décrites précédemment dans la rubrique. Ils peuvent servir à diagnostiquer les erreurs de la série 500.

Vérifiez que les extensions ASP.NET Core sont installées. Si ce n’est pas le cas, installez-les manuellement :

1. Dans la section du panneau **OUTILS DE DÉVELOPPEMENT**, sélectionnez le panneau **Extensions**.
1. Les **Extensions ASP.NET Core** devraient apparaître dans la liste.
1. Si les extensions ne sont pas installées, sélectionnez le bouton **Ajouter**.
1. Choisissez les **Extensions ASP.NET Core** dans la liste.
1. Sélectionnez **OK** pour accepter les conditions légales.
1. Sélectionnez **OK** sur le panneau **Ajouter une extension**.
1. Un message contextuel d’information indique que les extensions sont bien installées.

Si la journalisation stdout n’est pas activée, suivez les étapes ci-dessous :

1. Sur le Portail Azure, sélectionnez le panneau **Outils avancés** dans la zone **OUTILS DE DÉVELOPPEMENT**. Sélectionnez le bouton **Atteindre&rarr;**. La console Kudu s’ouvre dans un nouvel onglet ou une nouvelle fenêtre du navigateur.
1. Dans la barre de navigation en haut de la page, ouvrez **Console de débogage** et sélectionnez **CMD**.
1. Ouvrez les dossiers sur le chemin d’accès **site** > **wwwroot** et faites défiler la page jusqu’à révéler le fichier *web.config* en bas de la liste.
1. Cliquez sur l’icône en forme de crayon à côté du fichier *web.config*.
1. Définissez **stdoutLogEnabled** sur `true` et remplacez le chemin **stdoutLogFile** par : `\\?\%home%\LogFiles\stdout`.
1. Sélectionnez **Enregistrer** pour enregistrer le fichier *web.config* mis à jour.

Maintenant, activez la journalisation des diagnostics :

1. Sur le Portail Azure, sélectionnez le panneau **Journaux de diagnostic**.
1. Basculez **Journalisation des applications (système de fichiers)** et **Messages d’erreur détaillés** sur **Activé**. Sélectionnez le bouton **Enregistrer** en haut du panneau.
1. Pour inclure le suivi des demandes ayant échoué, également appelé Mise en mémoire tampon des événements d’échec des demandes (FREB), basculez **Suivi des demandes ayant échoué** sur **Activé**.
1. Sélectionnez le panneau **Flux du journal** situé juste sous le panneau **Journaux de diagnostic** sur le portail.
1. Adressez une demande à l’application.
1. La cause de l’erreur est indiquée dans les données de flux de journal.

Veillez à désactiver la journalisation stdout une fois la résolution des problèmes effectuée. Consultez les instructions dans la section [Journal stdout du module ASP.NET Core](#aspnet-core-module-stdout-log).

Pour afficher les journaux de suivi des demandes ayant échoué (journaux FREB) :

1. Accédez au panneau **Diagnostiquer et résoudre les problèmes** du Portail Azure.
1. Sélectionnez **Journaux de suivi des demandes ayant échoué** dans la zone **OUTILS DE PRISE EN CHARGE** de la barre latérale.

Voir la [section Traces des demandes ayant échoué de la rubrique Activer la journalisation des diagnostics pour les applications web dans Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) et [FAQ sur les performances des applications web dans Azure : comment activer le suivi des demandes ayant échoué ?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) pour plus d’informations.

Pour plus d’informations, voir [Activer la journalisation des diagnostics pour les applications web dans Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).

> [!WARNING]
> Si vous ne désactivez pas le journal stdout, l’application ou le serveur risque d’échouer. Il n’existe aucune limite quant à la taille du fichier journal ou au nombre de fichiers journaux créés.
>
> Pour les opérations de journalisation courantes dans une application ASP.NET Core, utilisez une bibliothèque de journalisation qui limite la taille du fichier journal et applique une rotation aux journaux. Pour plus d’informations, voir [Fournisseurs de journalisation tiers](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [Résoudre les problèmes d’une application web dans Azure App Service avec Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [Résoudre les erreurs HTTP « 502 Passerelle incorrecte » et « 503 Service non disponible » dans des applications web Azure](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [Résoudre les problèmes de performances d’une application web lente dans Azure App Service](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [FAQ sur les performances des applications web dans Azure](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [Bac à sable des applications web Azure (limitations de l’exécution du runtime App Service)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)
* [Azure Friday : Azure App Service Diagnostic and Troubleshooting Experience (vidéo de 12 minutes)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
