---
title: Héberger ASP.NET Core dans un service Windows
author: guardrex
description: Découvrez comment héberger une application ASP.NET Core dans un service Windows.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/25/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: f9b1c3fbfafa839c116688e0ac63804afcd5dbe0
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206671"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Héberger ASP.NET Core dans un service Windows

Par [Luke Latham](https://github.com/guardrex) et [Tom Dykstra](https://github.com/tdykstra)

Une application ASP.NET Core peut être hébergée sur Windows sans utiliser IIS en tant que [service Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications). Lorsqu’elle est hébergée en tant que service Windows, l’application démarre automatiquement après le redémarrage.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="convert-a-project-into-a-windows-service"></a>Convertir un projet en service Windows

Les modifications minimales suivantes sont nécessaires pour configurer un projet ASP.NET Core existant afin qu’il s’exécute en tant que service :

1. Dans le fichier projet :

   * Vérifiez la présence d’un [identificateur de runtime (RID)](/dotnet/core/rid-catalog) Windows ou ajoutez-le à `<PropertyGroup>` qui contient la version cible du .Net Framework :

      ::: moniker range=">= aspnetcore-2.1"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="= aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="< aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp1.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      Pour publier pour plusieurs RID :

      * Fournissez les RID dans une liste séparée par des points-virgules.
      * Utilisez le nom de la propriété `<RuntimeIdentifiers>` (pluriel).

      Pour plus d’informations, consultez le [Catalogue RID .NET Core](/dotnet/core/rid-catalog).

   * Ajoutez une référence de package pour [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).

1. Dans `Program.Main`, effectuez les changements suivants :

   * Appelez [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) au lieu de `host.Run`.

   * Appelez [UseContentRoot](xref:fundamentals/host/web-host#content-root) et utilisez un chemin vers l’emplacement publié de l’application au lieu de `Directory.GetCurrentDirectory()`.

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. Publiez l’application. Utilisez [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) ou un [profil de publication Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles). Si vous utilisez Visual Studio, sélectionnez **FolderProfile**.

   Pour publier l’exemple d’application avec des outils de l’interface de ligne de commande (CLI), exécutez la commande [dotnet publish](/dotnet/core/tools/dotnet-publish) à une invite de commandes à partir du dossier du projet. Le RID doit être spécifié dans la propriété `<RuntimeIdenfifier>` (ou `<RuntimeIdentifiers>`) du fichier projet. Dans l’exemple suivant, l’application est publiée dans la configuration Release pour le runtime `win7-x64` :

   ```console
   dotnet publish --configuration Release --runtime win7-x64
   ```

1. Utilisez l’outil en ligne de commande [sc.exe](https://technet.microsoft.com/library/bb490995) pour créer le service. La valeur `binPath` est le chemin du fichier exécutable de l’application, qui inclut le nom du fichier exécutable. **L’espace entre le signe égal et le guillemet au début du chemin est obligatoire.**

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   Pour un service publié dans le dossier du projet, utilisez le chemin du dossier *publish* pour créer le service. Dans l’exemple suivant :

   * Le projet réside dans le dossier *c:\\my_services\\AspNetCoreService*.
   * Le projet est publié dans la configuration `Release`.
   * Le moniker de framework cible (TFM) est `netcoreapp2.1`.
   * L’identificateur du runtime (RID) est `win7-x64`.
   * L’exécutable de l’application s’appelle *AspNetCoreService.exe*.
   * Le service s’appelle **MyService**.

   Exemple :

   ```console
   sc create MyService binPath= "c:\my_services\AspNetCoreService\bin\Release\netcoreapp2.1\win7-x64\publish\AspNetCoreService.exe"
   ```

   > [!IMPORTANT]
   > Vérifiez que l’espace est présent entre l’argument `binPath=` et sa valeur.

   Pour publier et démarrer le service à partir d’un autre dossier :

      * Utilisez l’option [--output &lt;RÉPERTOIRE_SORTIE&gt;](/dotnet/core/tools/dotnet-publish#options) dans la commande `dotnet publish`. Si vous utilisez Visual Studio, configurez l’**Emplacement cible** dans la page de propriétés de publication **FolderProfile** avant de sélectionner le bouton **Publier**.
      * Créez le service avec la commande `sc.exe` en utilisant le chemin du dossier de sortie. Ajoutez le nom de l’exécutable du service dans le chemin fourni dans `binPath`.

1. Démarrez le service avec la commande `sc start <SERVICE_NAME>`.

   Pour démarrer l’exemple de service d’application, utilisez la commande suivante :

   ```console
   sc start MyService
   ```

   La commande prend quelques secondes pour démarrer le service.

1. Pour vérifier l’état du service, utilisez la commande `sc query <SERVICE_NAME>`. L’état est signalé comme étant l’une des valeurs suivantes :

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   Utilisez la commande suivante pour vérifier l’état de l’exemple de service d’application :

   ```console
   sc query MyService
   ```

1. Quand le service est une application web et que son état est `RUNNING`, accédez au chemin de l'application (par défaut, `http://localhost:5000`, qui redirige vers `https://localhost:5001` si le [middleware (intergiciel) de redirection HTTPS](xref:security/enforcing-ssl) est utilisé).

   Pour l’exemple de service d’application, accédez au chemin de l'application sur `http://localhost:5000`.

1. Arrêtez le service avec la commande `sc stop <SERVICE_NAME>`.

   La commande suivante arrête l’exemple de service d’application :

   ```console
   sc stop MyService
   ```

1. Après un court délai pour arrêter un service, désinstallez le service avec la commande `sc delete <SERVICE_NAME>`.

   Vérifiez l’état de l’exemple de service d’application :

   ```console
   sc query MyService
   ```

   Quand l’exemple de service d’application est dans l’état `STOPPED`, utilisez la commande suivante pour le désinstaller :

   ```console
   sc delete MyService
   ```

## <a name="run-the-app-outside-of-a-service"></a>Exécuter l’application en dehors d’un service

Comme il est plus facile d’effectuer des tests et des débogages pendant une exécution en dehors d’un service, il est habituel d’ajouter du code qui appelle `RunAsService` uniquement sous certaines conditions. Par exemple, l’application peut s’exécuter en tant qu’application console avec un argument de ligne de commande `--console` ou si le débogueur est attaché :

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

Étant donné que la configuration d’ASP.NET Core nécessite des paires nom/valeur pour les arguments de ligne de commande, le commutateur `--console` est supprimé avant que les arguments ne soient passés à [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).

> [!NOTE]
> `isService` n’est pas passé de `Main` vers `CreateWebHostBuilder`, car la signature de `CreateWebHostBuilder` doit être `CreateWebHostBuilder(string[])` afin que les [tests d’intégration](xref:test/integration-tests) fonctionnent correctement.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a>Gérer les événements d’arrêt et de démarrage

Pour gérer les événements [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted) et [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping), apportez les modifications supplémentaires suivantes :

1. Créez une classe qui dérive de [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice) :

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. Créez une méthode d’extension pour [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) qui transmet le `WebHostService` personnalisé à [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run) :

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. Dans `Program.Main`, appelez la nouvelle méthode d’extension, `RunAsCustomService`, au lieu de [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) :

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > `isService` n’est pas passé de `Main` vers `CreateWebHostBuilder`, car la signature de `CreateWebHostBuilder` doit être `CreateWebHostBuilder(string[])` afin que les [tests d’intégration](xref:test/integration-tests) fonctionnent correctement.

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

Si le code `WebHostService` personnalisé nécessite un service à partir d’une injection de dépendances (par exemple, un enregistreur d’événements), obtenez-le à partir de la propriété [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) :

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>Scénarios avec un serveur proxy et un équilibreur de charge

Les services qui interagissent avec les requêtes provenant d’Internet ou d’un réseau d’entreprise et qui se trouvent derrière un proxy ou équilibreur de charge peuvent nécessiter une configuration supplémentaire. Pour plus d'informations, consultez <xref:host-and-deploy/proxy-load-balancer>.

## <a name="configure-https"></a>Configurer HTTPS

Spécifiez la [configuration de point de terminaison HTTPS d’un serveur Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).

## <a name="current-directory-and-content-root"></a>Répertoire actif et racine du contenu

Le répertoire de travail actif retourné par l’appel à `Directory.GetCurrentDirectory()` pour un service Windows est le dossier *C:\\WINDOWS\\system32*. Le dossier *system32* n’est pas un emplacement approprié pour stocker les fichiers d’un service (tels que les fichiers de paramètres). Adoptez l’une des approches suivantes pour tenir à jour et accéder aux ressources et aux fichiers de paramètres d’un service avec [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) quand vous utilisez un [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) :

* Utilisez le chemin racine du contenu. `IHostingEnvironment.ContentRootPath` est le même chemin que celui fourni à l’argument `binPath` quand le service est créé. Au lieu d’utiliser `Directory.GetCurrentDirectory()` pour créer des chemins aux fichiers de paramètres, utilisez le chemin racine du contenu et tenez à jour les fichiers dans la racine du contenu de l’application.
* Stockez les fichiers à un emplacement approprié sur le disque. Spécifiez un chemin absolu avec `SetBasePath` au dossier contenant les fichiers.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Configuration de point de terminaison Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (inclut la configuration de HTTPS et la prise en charge de SNI)
* <xref:fundamentals/host/web-host>
