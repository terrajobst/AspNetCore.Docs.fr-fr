---
title: Héberger ASP.NET Core dans un service Windows
author: rick-anderson
description: Découvrez comment héberger une application ASP.NET Core dans un service Windows.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: 29f83ee585c73aeb57a09f70ea8e28650c05ce69
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34153527"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Héberger ASP.NET Core dans un service Windows

Par [Tom Dykstra](https://github.com/tdykstra)

La méthode recommandée pour héberger une application ASP.NET Core sur Windows sans utiliser IIS consiste à l’exécuter dans un [service Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications). Quand elle est hébergée en tant que service Windows, l’application peut automatiquement démarrer après les redémarrages et les plantages sans nécessiter une intervention humaine.

[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample)). Pour obtenir des instructions sur la façon d’exécuter l’exemple d’application, consultez le fichier *README.md* de l’exemple.

## <a name="prerequisites"></a>Prérequis

* L’application doit s’exécuter sur le runtime .NET Framework. Dans le fichier *.csproj*, spécifiez des valeurs appropriées pour [TargetFramework](/nuget/schema/target-frameworks) et [RuntimeIdentifier](/dotnet/articles/core/rid-catalog). Voici un exemple :

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  Quand vous créez un projet dans Visual Studio, utilisez le modèle **Application ASP.NET Core (.NET Framework)**.

* Si l’application reçoit des requêtes à partir d’Internet (pas uniquement à partir d’un réseau interne), elle doit utiliser le serveur web [HTTP.sys](xref:fundamentals/servers/httpsys) (anciennement [WebListener](xref:fundamentals/servers/weblistener) pour les applications ASP.NET Core 1.x) au lieu de [Kestrel](xref:fundamentals/servers/kestrel). Nous vous recommandons d’utiliser IIS comme serveur proxy inverse avec Kestrel pour les déploiements de périphérie. Pour plus d’informations, consultez [Quand utiliser Kestrel avec un proxy inverse ?](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

## <a name="get-started"></a>Prise en main

Cette section explique les modifications minimales requises pour configurer un projet ASP.NET Core existant afin qu’il s’exécute dans un service.

1. Installez le package NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).

2. Dans `Program.Main`, effectuez les changements suivants :

   * Appelez `host.RunAsService` à la place de `host.Run`.

   * Si le code appelle `UseContentRoot`, utilisez un chemin de l’emplacement de publication au lieu de `Directory.GetCurrentDirectory()`.

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,12)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

   ---

3. Publiez l’application sur un dossier. Utilisez [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) ou un [profil de publication Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) qui publie sur un dossier.

4. Effectuez un test en créant et en démarrant le service.

   Ouvrez un interpréteur de commandes avec des privilèges administratifs afin de créer et de démarrer un service à l’aide de l’outil en ligne de commande [sc.exe](https://technet.microsoft.com/library/bb490995). Si le service est créé sous le nom MyService, qu’il est publié sur `c:\svc` et que son nom est AspNetCoreService, les commandes sont les suivantes :

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

   La valeur `binPath` est le chemin du fichier exécutable de l’application, qui inclut le nom du fichier exécutable.

   ![Exemple de la fenêtre de la console de création et de démarrage](windows-service/_static/create-start.png)

   Une fois ces commandes exécutées, accédez au chemin correspondant à l’exécution en tant qu’application console (par défaut, `http://localhost:5000`) :

   ![Exécution dans un service](windows-service/_static/running-in-service.png)

## <a name="provide-a-way-to-run-outside-of-a-service"></a>Fournir un moyen d’exécution en dehors d’un service

Comme il est plus facile d’effectuer des tests et des débogages pendant une exécution en dehors d’un service, il est habituel d’ajouter du code qui appelle `RunAsService` uniquement sous certaines conditions. Par exemple, l’application peut s’exécuter en tant qu’application console avec un argument de ligne de commande `--console` ou si le débogueur est attaché :

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

---

## <a name="handle-stopping-and-starting-events"></a>Gérer les événements d’arrêt et de démarrage

Pour gérer les événements `OnStarting`, `OnStarted` et `OnStopping`, apportez les modifications supplémentaires suivantes :

1. Créez une classe qui dérive de `WebHostService` :

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. Créez une méthode d’extension pour `IWebHost` qui transmet le `WebHostService` personnalisé à `ServiceBase.Run` :

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. Dans `Program.Main`, appelez la nouvelle méthode d’extension, `RunAsCustomService`, au lieu de `RunAsService` :

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=24)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=26)]

   ---

Si le code `WebHostService` personnalisé nécessite un service à partir d’une injection de dépendances (par exemple, un enregistreur d’événements), obtenez-le à partir de la propriété `Services` d’`IWebHost` :

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>Scénarios avec un serveur proxy et un équilibreur de charge

Les services qui interagissent avec les requêtes provenant d’Internet ou d’un réseau d’entreprise et qui se trouvent derrière un proxy ou équilibreur de charge peuvent nécessiter une configuration supplémentaire. Pour plus d’informations, consultez [Configurer ASP.NET Core pour l’utilisation de serveurs proxy et d’équilibreurs de charge](xref:host-and-deploy/proxy-load-balancer).

## <a name="acknowledgments"></a>Remerciements

Cet article a été écrit à l’aide de sources publiées :

* [Hosting ASP.NET Core as Windows service](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074) (Hébergement d’ASP.NET Core en tant que service Windows)
* [How to host your ASP.NET Core in a Windows Service](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/) (Guide pratique pour héberger ASP.NET Core dans un service Windows)
