---
title: Journalisation et diagnostics dans ASP.NET Core SignalR
author: anurse
description: Découvrez comment recueillir des diagnostics à partir de votre application ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: signalr
ms.date: 06/19/2019
uid: signalr/diagnostics
ms.openlocfilehash: 69dbd057b3dcadeb3ca5d94ede1234530fb447db
ms.sourcegitcommit: 9f11685382eb1f4dd0fb694dea797adacedf9e20
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67313707"
---
# <a name="logging-and-diagnostics-in-aspnet-core-signalr"></a>Journalisation et diagnostics dans ASP.NET Core SignalR

Par [Andrew Stanton-Nurse](https://twitter.com/anurse)

Cet article fournit des conseils pour la collecte des diagnostics à partir de votre application ASP.NET Core SignalR pour aider à résoudre les problèmes.

## <a name="server-side-logging"></a>Journalisation côté serveur

> [!WARNING]
> Journaux côté serveur peuvent contenir des informations sensibles de votre application. **Jamais** publier des journaux bruts à partir d’applications de production dans les forums publics comme GitHub.

Étant donné que SignalR fait partie d’ASP.NET Core, il utilise le système de journalisation ASP.NET Core. Dans la configuration par défaut, les journaux de SignalR très peu d’informations, mais cela peut configurés. Consultez la documentation sur [journalisation ASP.NET Core](xref:fundamentals/logging/index#configuration) pour plus d’informations sur la configuration de journalisation ASP.NET Core.

SignalR utilise deux catégories d’enregistreur d’événements :

* `Microsoft.AspNetCore.SignalR` &ndash; pour les journaux liés aux protocoles de concentrateur, activation de concentrateurs, appel de méthodes et autres activités du Hub.
* `Microsoft.AspNetCore.Http.Connections` &ndash; pour les journaux liés aux transports tels que les WebSockets, d’interrogation longue et les événements et de bas niveau infrastructure SignalR.

Pour activer les journaux détaillés à partir de SignalR, configurez les deux préfixes précédents à la `Debug` niveau dans votre *appsettings.json* fichier en ajoutant les éléments suivants dans le `LogLevel` sous-section dans `Logging`:

[!code-json[](diagnostics/logging-config.json?highlight=7-8)]

Vous pouvez aussi configurer cela dans le code dans votre `CreateWebHostBuilder` méthode :

[!code-csharp[](diagnostics/logging-config-code.cs?highlight=5-6)]

Si vous n’utilisez pas configuration basée sur JSON, définissez les valeurs de configuration suivantes dans votre système de configuration :

* `Logging:LogLevel:Microsoft.AspNetCore.SignalR` = `Debug`
* `Logging:LogLevel:Microsoft.AspNetCore.Http.Connections` = `Debug`

Consultez la documentation de votre système de configuration déterminer comment spécifier des valeurs de configuration imbriquée. Par exemple, lors de l’utilisation de variables d’environnement, deux `_` caractères sont utilisés à la place de la `:` (par exemple, `Logging__LogLevel__Microsoft.AspNetCore.SignalR`).

Nous vous recommandons d’utiliser le `Debug` niveau lors de la collecte plus de diagnostics pour votre application. Le `Trace` au niveau du produit les diagnostics de très bas niveau et est rarement nécessaire pour diagnostiquer les problèmes dans votre application.

## <a name="access-server-side-logs"></a>Accéder aux journaux du côté serveur

Mode d’accès aux journaux côté serveur dépend de l’environnement dans lequel vous êtes en cours d’exécution.

### <a name="as-a-console-app-outside-iis"></a>Une application console en dehors d’IIS

Si vous utilisez dans une application console, le [journal de Console](xref:fundamentals/logging/index#console-provider) doit être activée par défaut. Journaux de SignalR seront affiche dans la console.

### <a name="within-iis-express-from-visual-studio"></a>Dans IIS Express à partir de Visual Studio

Visual Studio affiche la sortie du journal dans le **sortie** fenêtre. Sélectionnez le **ASP.NET Core Web Server** option de liste déroulante.

### <a name="azure-app-service"></a>Azure App Service

Activer la **journal des applications (Filesystem)** option dans le **les journaux de diagnostic** section du portail Azure App Service et configurer le **niveau** à `Verbose`. Les journaux doivent être disponibles à partir de la **diffusion de journaux** service et dans les journaux sur le système de fichiers du Service d’application. Pour plus d’informations, consultez [streaming des journaux Azure](xref:fundamentals/logging/index#azure-log-streaming).

### <a name="other-environments"></a>Autres environnements

Si l’application est déployée sur un autre environnement (par exemple, Docker, Kubernetes ou Service de Windows), consultez <xref:fundamentals/logging/index> pour plus d’informations sur la configuration des fournisseurs de journalisation adaptés à l’environnement.

## <a name="javascript-client-logging"></a>Enregistrement du client JavaScript

> [!WARNING]
> Journaux côté client peuvent contenir des informations sensibles de votre application. **Jamais** publier des journaux bruts à partir d’applications de production dans les forums publics comme GitHub.

Lorsque vous utilisez le client JavaScript, vous pouvez configurer les options de journalisation à l’aide de la `configureLogging` méthode sur `HubConnectionBuilder`:

[!code-javascript[](diagnostics/logging-config-js.js?highlight=3)]

Pour désactiver entièrement la journalisation, spécifiez `signalR.LogLevel.None` dans la méthode `configureLogging`.

Le tableau suivant présente les niveaux de journalisation disponibles pour le client JavaScript. Une des valeurs suivantes affectant le niveau de journal Active la journalisation à ce niveau et tous les niveaux au-dessus de lui dans la table.

| Niveau | Description |
| ----- | ----------- |
| `None` | Aucun message n’est enregistré. |
| `Critical` | Messages qui indiquent un échec dans l’application entière. |
| `Error` | Messages qui indiquent un échec dans l’opération en cours. |
| `Warning` | Messages qui indiquent un problème non irrécupérable. |
| `Information` | Messages d’information. |
| `Debug` | Messages de diagnostic utiles pour le débogage. |
| `Trace` | Messages de diagnostic très détaillés conçus pour diagnostiquer les problèmes spécifiques. |

Une fois que vous avez configuré le niveau de détail, les journaux sont écrits à la Console du navigateur (ou de la sortie Standard dans une application NodeJS).

Si vous souhaitez envoyer des journaux à un système de journalisation personnalisée, vous pouvez fournir un objet JavaScript qui implémente le `ILogger` interface. La seule méthode qui doit être implémentée est `log`, ce qui amène le niveau de l’événement et le message associé à l’événement. Exemple :

[!code-typescript[](diagnostics/custom-logger.ts?highlight=3-7,13)]

## <a name="net-client-logging"></a>Enregistrement du client .NET

> [!WARNING]
> Journaux côté client peuvent contenir des informations sensibles de votre application. **Jamais** publier des journaux bruts à partir d’applications de production dans les forums publics comme GitHub.

Pour obtenir des journaux à partir du client .NET, vous pouvez utiliser la `ConfigureLogging` méthode sur `HubConnectionBuilder`. Cela fonctionne de la même façon que le `ConfigureLogging` méthode sur `WebHostBuilder` et `HostBuilder`. Vous pouvez configurer les mêmes fournisseurs de journalisation que vous utilisez dans ASP.NET Core. Toutefois, vous devez installer et activer les packages NuGet pour les fournisseurs de journalisation individuelles manuellement.

### <a name="console-logging"></a>Journalisation de console

Pour activer la journalisation de la Console, ajoutez le [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) package. Ensuite, utilisez le `AddConsole` méthode pour configurer le journal de console :

[!code-csharp[](diagnostics/net-client-console-log.cs?highlight=6)]

### <a name="debug-output-window-logging"></a>La journalisation de fenêtre de sortie du débogage

Vous pouvez également configurer les journaux pour accéder à la **sortie** fenêtre dans Visual Studio. Installer le [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) empaqueter et utiliser le `AddDebug` méthode :

[!code-csharp[](diagnostics/net-client-debug-log.cs?highlight=6)]

### <a name="other-logging-providers"></a>Autres fournisseurs de journalisation

SignalR prend en charge d’autres fournisseurs de journalisation comme Serilog, Seq, NLog ou tout autre système de journalisation qui s’intègre à `Microsoft.Extensions.Logging`. Si votre système de journalisation fournit une `ILoggerProvider`, vous pouvez l’inscrire avec `AddProvider`:

[!code-csharp[](diagnostics/net-client-custom-log.cs?highlight=6)]

### <a name="control-verbosity"></a>Contrôle le niveau de détail

Si vous vous connectez à partir d’autres emplacements dans votre application, la modification du niveau de la valeur par défaut pour `Debug` est peut-être trop détaillée. Vous pouvez utiliser un filtre pour configurer le niveau de journalisation pour les journaux de SignalR. Cela est possible dans le code, à peu près la même façon que sur le serveur :

[!code-csharp[Controlling verbosity in .NET client](diagnostics/logging-config-client-code.cs?highlight=9-10)]

## <a name="network-traces"></a>Suivis réseau

> [!WARNING]
> Une trace réseau contient le contenu complet de chaque message envoyé par votre application. **Jamais** publier des traces réseau brutes à partir d’applications de production dans les forums publics comme GitHub.

Si vous rencontrez un problème, une trace réseau peut parfois fournir beaucoup d’informations utiles. Cela est particulièrement utile si vous voulez signaler un problème sur notre suivi des problèmes.

## <a name="collect-a-network-trace-with-fiddler-preferred-option"></a>Recueillir une trace réseau avec Fiddler (option par défaut)

Cette méthode fonctionne pour toutes les applications.

Fiddler est un outil très puissant pour la collecte des traces HTTP. Installez-le à partir de [telerik.com/fiddler](https://www.telerik.com/fiddler), lancez-le, puis exécutez votre application et reproduisez le problème. Fiddler est disponible pour Windows, et il existe des versions bêta pour macOS et Linux.

Si vous vous connectez à l’aide de HTTPS, il existe quelques étapes supplémentaires pour vous assurer de que Fiddler peut déchiffrer le trafic HTTPS. Pour plus d’informations, consultez le [documentation Fiddler](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).

Une fois que vous avez collecté la trace, vous pouvez exporter la trace en choisissant **fichier** > **enregistrer** > **toutes les Sessions** à partir de la barre de menus.

![Exportation de toutes les sessions à partir de Fiddler](diagnostics/fiddler-export.png)

## <a name="collect-a-network-trace-with-tcpdump-macos-and-linux-only"></a>Recueillir une trace réseau avec tcpdump (Mac OS et Linux uniquement)

Cette méthode fonctionne pour toutes les applications.

Vous pouvez collecter brutes suivis TCP à l’aide de tcpdump en exécutant la commande suivante à partir d’une invite de commandes. Vous devrez peut-être être `root` ou un préfixe de la commande avec `sudo` si vous obtenez une erreur d’autorisations :

```console
tcpdump -i [interface] -w trace.pcap
```

Remplacez `[interface]` avec l’interface réseau que vous souhaitez capturer sur. En règle générale, il s’agit d’un élément comme `/dev/eth0` (pour votre interface Ethernet standard) ou `/dev/lo0` (pour le trafic de localhost). Pour plus d’informations, consultez le `tcpdump` page man sur votre système hôte.

## <a name="collect-a-network-trace-in-the-browser"></a>Collecter un suivi réseau dans le navigateur

Cette méthode fonctionne uniquement pour les applications basées sur le navigateur.

La plupart des outils de développement de navigateur ont un onglet « Network » qui vous permet de capturer l’activité réseau entre le navigateur et le serveur. Toutefois, ces suivis n’incluent pas les messages WebSocket et les événements. Si vous utilisez ces transports, à l’aide d’un outil comme Fiddler ou TcpDump (décrits ci-dessous) est une meilleure approche.

### <a name="microsoft-edge-and-internet-explorer"></a>Microsoft Edge et Internet Explorer

(Les instructions sont les mêmes pour Edge et Internet Explorer)

1. Appuyez sur F12 pour ouvrir les outils de développement
2. Cliquez sur l’onglet réseau
3. Actualisez la page (si nécessaire) et de reproduire le problème
4. Cliquez sur l’icône Enregistrer dans la barre d’outils pour exporter la trace dans un fichier « HAR » :

![L’enregistrement onglet réseau des outils icône sur le développement de Microsoft Edge](diagnostics/ie-edge-har-export.png)

### <a name="google-chrome"></a>Google Chrome

1. Appuyez sur F12 pour ouvrir les outils de développement
2. Cliquez sur l’onglet réseau
3. Actualisez la page (si nécessaire) et de reproduire le problème
4. Avec le bouton droit sur n’importe où dans la liste de demandes et choisissez « Enregistrer en tant que HAR avec du contenu » :

![Option de « Enregistrer en tant que HAR avec du contenu » dans l’onglet réseau des outils de développement Google Chrome](diagnostics/chrome-har-export.png)

### <a name="mozilla-firefox"></a>Mozilla Firefox

1. Appuyez sur F12 pour ouvrir les outils de développement
2. Cliquez sur l’onglet réseau
3. Actualisez la page (si nécessaire) et de reproduire le problème
4. Avec le bouton droit sur n’importe où dans la liste de demandes et choisissez « Enregistrer toutes en tant que HAR »

![Option « Enregistrer tout en tant que HAR » dans l’onglet réseau des outils de développement Mozilla Firefox](diagnostics/firefox-har-export.png)

## <a name="attach-diagnostics-files-to-github-issues"></a>Joindre des fichiers de diagnostic vers les problèmes GitHub

Vous pouvez joindre des fichiers de diagnostic vers les problèmes GitHub en les renommant afin qu’ils aient un `.txt` extension puis en faisant glisser et en les déposant sur le problème.

> [!NOTE]
> Veuillez ne pas y coller le contenu de fichiers journaux ou des traces réseau dans un problème GitHub. Ces journaux et les traces peuvent être très volumineux et GitHub généralement les tronque.

![En faisant glisser des fichiers journaux à un problème GitHub](diagnostics/attaching-diagnostics-files.png)

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:signalr/configuration>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
