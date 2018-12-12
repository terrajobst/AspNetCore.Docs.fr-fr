---
title: Héberger ASP.NET Core dans un service Windows
author: guardrex
description: Découvrez comment héberger une application ASP.NET Core dans un service Windows.
monikerRange: '>= aspnetcore-2.2'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/26/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: f857e96108b68bb6ec64a85910bf4d889cdf2822
ms.sourcegitcommit: e7fafb153b9de7595c2558a0133f8d1c33a3bddb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52458515"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Héberger ASP.NET Core dans un service Windows

Par [Luke Latham](https://github.com/guardrex) et [Tom Dykstra](https://github.com/tdykstra)

Une application ASP.NET Core peut être hébergée sur Windows en tant que [service Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications) sans utiliser IIS. Lorsqu’elle est hébergée en tant que service Windows, l’application démarre automatiquement après le redémarrage.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="deployment-type"></a>Type de déploiement

Vous pouvez créer un déploiement Windows Service dépendant du framework ou autonome. Pour des informations et des conseils sur les scénarios de déploiement, consultez [Déploiement d’applications .NET Core](/dotnet/core/deploying/).

### <a name="framework-dependent-deployment"></a>Déploiement dépendant du framework

Un déploiement dépendant du framework s’appuie sur la présence d’une version partagée à l’échelle du système de .NET Core sur le système cible. Lorsque le scénario de déploiement dépendant du framework est utilisé avec une application de service Windows ASP.NET Core, le Kit de développement logiciel (SDK) génère un fichier exécutable (*\*.exe*), appelé *exécutable dépendant du framework*.

### <a name="self-contained-deployment"></a>Déploiement autonome

Un déploiement autonome ne s’appuie sur la présence d’aucun composant partagé sur le système cible. Le runtime et les dépendances de l’application sont déployés avec l’application sur le système d’hébergement.

## <a name="convert-a-project-into-a-windows-service"></a>Convertir un projet en service Windows

Apportez les modifications suivantes à un projet ASP.NET Core existant pour exécuter l’application en tant que service :

1. Selon le [type de déploiement](#deployment-type) que vous avez choisi, mettez à jour le fichier projet :

   * **Déploiement dépendant du framework (FDD)** &ndash; Ajoutez un [identificateur de runtime (RID)](/dotnet/core/rid-catalog) Windows à `<PropertyGroup>` qui contient le framework cible. Ajoutez la propriété `<SelfContained>` définie sur `false`. Désactivez la création d’un fichier *web.config* en ajoutant la propriété `<IsTransformWebConfigDisabled>` définie sur `true`.

     ```xml
     <PropertyGroup>
       <TargetFramework>netcoreapp2.2</TargetFramework>
       <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
       <SelfContained>false</SelfContained>
       <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
     </PropertyGroup>
     ```

     **Déploiement autonome (SCD)** &ndash; Vérifiez la présence d’un [identificateur de runtime (RID)](/dotnet/core/rid-catalog) Windows ou ajoutez-le à `<PropertyGroup>` qui contient le framework cible. Désactivez la création d’un fichier *web.config* en ajoutant la propriété `<IsTransformWebConfigDisabled>` définie sur `true`.

     ```xml
     <PropertyGroup>
       <TargetFramework>netcoreapp2.2</TargetFramework>
       <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
       <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
     </PropertyGroup>
     ```

     Pour publier pour plusieurs RID :

     * Fournissez les RID dans une liste séparée par des points-virgules.
     * Utilisez le nom de la propriété `<RuntimeIdentifiers>` (pluriel).

     Pour plus d’informations, consultez le [Catalogue RID .NET Core](/dotnet/core/rid-catalog).

   * Ajoutez une référence de package pour [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).

   * Pour activer l’enregistrement du journal des événements Windows, ajoutez une référence de package pour [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).

     Pour plus d’informations, consultez la section [Gérer les événements de démarrage et d’arrêt](#handle-starting-and-stopping-events).

1. Dans `Program.Main`, effectuez les changements suivants :

   * Pour effectuer des tests et un débogage lors de l’exécution en dehors d’un service, ajoutez un code pour déterminer si l’application s’exécute comme un service ou comme une application console. Vérifiez si le débogueur est attaché ou si un argument de ligne de commande `--console` est présent.

     Si l’une de ces deux conditions est remplie (l’application n’est pas exécutée en tant que service), appelez <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> sur l’hôte web.

     Si les conditions ne sont pas remplies (l’application est exécutée en tant que service) :

     * Appelez <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> et utilisez un chemin vers l’emplacement publié de l’application. N’appelez pas <xref:System.IO.Directory.GetCurrentDirectory*> pour obtenir le chemin d’accès car une application Windows Service retourne le dossier *C:\\WINDOWS\\system32* lorsque `GetCurrentDirectory` est appelée. Pour plus d’informations, consultez la section [Répertoire actif et racine du contenu](#current-directory-and-content-root).
     * Appelez <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> pour exécuter l’application en tant que service.

     Étant donné que le [fournisseur de configuration de ligne de commande](xref:fundamentals/configuration/index#command-line-configuration-provider) nécessite des paires nom/valeur pour les arguments de ligne de commande, le commutateur `--console` est supprimé des arguments avant que <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> ne les reçoive.

   * Pour consigner des informations dans le journal des événements Windows, ajoutez le fournisseur EventLog à <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>. Définissez le niveau de journalisation à l’aide de clé `Logging:LogLevel:Default` dans le fichier *appsettings.Production.json*. À des fins de démonstration et de test, le fichier des paramètres de l’exemple d’application Production définit le niveau de journalisation sur `Information`. En production, la valeur est généralement définie sur `Error`. Pour plus d'informations, consultez <xref:fundamentals/logging/index#windows-eventlog-provider>.

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

1. Publiez l’application avec [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), un [profil de publication Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) ou Visual Studio Code. Si vous utilisez Visual Studio, sélectionnez **FolderProfile** et configurez **Emplacement cible** avant de sélectionner le bouton **Publier**.

   Pour publier l’exemple d’application avec des outils de l’interface de ligne de commande (CLI), exécutez la commande [dotnet publish](/dotnet/core/tools/dotnet-publish) à une invite de commandes à partir du dossier du projet, en passant la configuration de mise en production (Release) à l’option [-c|--configuration](/dotnet/core/tools/dotnet-publish#options). Utilisez l’option [-o|--output](/dotnet/core/tools/dotnet-publish#options) avec un chemin d'accès pour publier dans un dossier à l’extérieur de l’application.

   * **Déploiement dépendant du framework (FDD)**

     Dans l’exemple suivant, l’application est publiée dans le dossier *c:\\svc* :

     ```console
     dotnet publish --configuration Release --output c:\svc
     ```

   * **Déploiement autonome (SCD)** &ndash;Le RID doit être spécifié dans la propriété `<RuntimeIdenfifier>` (ou `<RuntimeIdentifiers>`) du fichier projet. Fournissez le runtime à l’option [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) de la commande `dotnet publish`.

     Dans l’exemple suivant, l’application est publiée pour le runtime `win7-x64` dans le dossier *c:\\svc* :

     ```console
     dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
     ```

1. Créez un compte d’utilisateur pour le service à l’aide de la commande `net user` :

   ```console
   net user {USER ACCOUNT} {PASSWORD} /add
   ```

   Pour l’exemple d’application, créez un compte d’utilisateur avec le nom `ServiceUser` et un mot de passe. Dans la commande suivante, remplacez `{PASSWORD}` par un [mot de passe fort](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).

   ```console
   net user ServiceUser {PASSWORD} /add
   ```

   Si vous devez ajouter l’utilisateur à un groupe, utilisez la commande `net localgroup`, où `{GROUP}` est le nom du groupe :

   ```console
   net localgroup {GROUP} {USER ACCOUNT} /add
   ```

   Pour plus d’informations, consultez [Comptes d’utilisateur de service](/windows/desktop/services/service-user-accounts).

1. Accordez l’accès en écriture/lecture/exécution au dossier de l’application à l’aide de la commande [icacls](/windows-server/administration/windows-commands/icacls) :

   ```console
   icacls "{PATH}" /grant {USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS} /t
   ```

   * `{PATH}` &ndash; Chemin au dossier de l’application.
   * `{USER ACCOUNT}` &ndash; Compte d’utilisateur (SID).
   * `(OI)` &ndash; L’indicateur Object Inherit propage les autorisations aux fichiers subordonnés.
   * `(CI)` &ndash; L’indicateur Container Inherit propage les autorisations aux dossiers subordonnés.
   * `{PERMISSION FLAGS}` &ndash; Définit les autorisations d’accès de l’application.
     * Écriture (`W`)
     * Lecture (`R`)
     * Exécution (`X`)
     * Complet (`F`)
     * Modification (`M`)
   * `/t` &ndash; Appliquer de manière récursive aux dossiers et fichiers subordonnés existants.

   Pour l’exemple d’application publiée dans le dossier *c:\\svc* et le compte `ServiceUser` avec des autorisations en écriture/lecture/exécution, utilisez la commande suivante :

   ```console
   icacls "c:\svc" /grant ServiceUser:(OI)(CI)WRX /t
   ```

   Pour plus d’informations, consultez [icacls](/windows-server/administration/windows-commands/icacls).

1. Utilisez l’outil en ligne de commande [sc.exe](https://technet.microsoft.com/library/bb490995) pour créer le service. La valeur `binPath` est le chemin du fichier exécutable de l’application, qui inclut le nom du fichier exécutable. **L’espace entre le signe égal à la fin du paramètre et le guillemet au début de la valeur est obligatoire.**

   ```console
   sc create {SERVICE NAME} binPath= "{PATH}" obj= "{DOMAIN}\{USER ACCOUNT}" password= "{PASSWORD}"
   ```

   * `{SERVICE NAME}` &ndash; Nom à attribuer au service dans le [Gestionnaire de contrôle des services](/windows/desktop/services/service-control-manager).
   * `{PATH}` &ndash; Chemin à l’exécutable du service.
   * `{DOMAIN}` &ndash; Domaine d’un ordinateur joint au domaine. Si l’ordinateur n’est pas joint au domaine, nom de l’ordinateur local.
   * `{USER ACCOUNT}` &ndash; Compte d’utilisateur sous lequel le service s’exécute.
   * `{PASSWORD}` &ndash; Mot de passe du compte d’utilisateur.

   > [!WARNING]
   > **N’oubliez pas** le paramètre `obj`. `obj` a pour valeur par défaut le compte [LocalSystem](/windows/desktop/services/localsystem-account). L’exécution d’un service sous le compte `LocalSystem` présente un risque de sécurité important. Veillez à toujours exécuter un service sous un compte d’utilisateur disposant de privilèges limités.

   Dans l’exemple suivant pour l’exemple d’application :

   * Le service s’appelle **MyService**.
   * Le service publié réside dans le dossier *c:\\svc*. L’application exécutable s’appelle *SampleApp.exe*. Mettez la valeur `binPath` entre guillemets doubles (").
   * Le service s’exécute sous le compte `ServiceUser`. Remplacez `{DOMAIN}` par le nom de l’ordinateur local ou le domaine du compte d’utilisateur. Mettez la valeur `obj` entre guillemets doubles ("). Exemple : Si le système d’hébergement est un ordinateur local nommé `MairaPC`, définissez `obj` avec la valeur `"MairaPC\ServiceUser"`.
   * Remplacez `{PASSWORD}` par le mot de passe du compte d’utilisateur. Mettez la valeur `password` entre guillemets doubles (").

   ```console
   sc create MyService binPath= "c:\svc\sampleapp.exe" obj= "{DOMAIN}\ServiceUser" password= "{PASSWORD}"
   ```

   > [!IMPORTANT]
   > Vérifiez la présence d’espaces entre les signes égal et les valeurs des paramètres.

1. Démarrez le service avec la commande `sc start {SERVICE NAME}`.

   Pour démarrer l’exemple de service d’application, utilisez la commande suivante :

   ```console
   sc start MyService
   ```

   La commande prend quelques secondes pour démarrer le service.

1. Pour vérifier l’état du service, utilisez la commande `sc query {SERVICE NAME}`. L’état est signalé comme étant l’une des valeurs suivantes :

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

1. Arrêtez le service avec la commande `sc stop {SERVICE NAME}`.

   La commande suivante arrête l’exemple de service d’application :

   ```console
   sc stop MyService
   ```

1. Après un court délai pour arrêter un service, désinstallez le service avec la commande `sc delete {SERVICE NAME}`.

   Vérifiez l’état de l’exemple de service d’application :

   ```console
   sc query MyService
   ```

   Quand l’exemple de service d’application est dans l’état `STOPPED`, utilisez la commande suivante pour le désinstaller :

   ```console
   sc delete MyService
   ```

## <a name="handle-starting-and-stopping-events"></a>Gérer les événements de démarrage et d’arrêt

Pour gérer les événements <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> et <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*>, apportez les modifications supplémentaires suivantes :

1. Créez une classe qui dérive de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> à l’aide des méthodes `OnStarting`, `OnStarted` et `OnStopping` :

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. Créez une méthode d’extension pour <xref:Microsoft.AspNetCore.Hosting.IWebHost> qui transmet `CustomWebHostService` à <xref:System.ServiceProcess.ServiceBase.Run*> :

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. Dans `Program.Main`, appelez la méthode d’extension `RunAsCustomService` au lieu de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> :

   ```csharp
   host.RunAsCustomService();
   ```

   Pour afficher l’emplacement de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> dans `Program.Main`, reportez-vous à l’exemple de code indiqué dans la section [Convertir un projet en service Windows](#convert-a-project-into-a-windows-service).

## <a name="proxy-server-and-load-balancer-scenarios"></a>Scénarios avec un serveur proxy et un équilibreur de charge

Les services qui interagissent avec les requêtes provenant d’Internet ou d’un réseau d’entreprise et qui se trouvent derrière un proxy ou équilibreur de charge peuvent nécessiter une configuration supplémentaire. Pour plus d'informations, consultez <xref:host-and-deploy/proxy-load-balancer>.

## <a name="configure-https"></a>Configurer HTTPS

Pour configurer le service avec un point de terminaison sécurisé :

1. Créez un certificat X.509 pour le système d’hébergement à l’aide des mécanismes d’acquisition et de déploiement de certificat de votre plateforme.

1. Spécifiez la [configuration de point de terminaison HTTPS d’un serveur Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) pour utiliser le certificat.

L’utilisation du certificat de développement HTTPS ASP.NET Core pour sécuriser un point de terminaison de service n’est pas prise en charge.

## <a name="current-directory-and-content-root"></a>Répertoire actif et racine du contenu

Le répertoire de travail actif retourné par l’appel à <xref:System.IO.Directory.GetCurrentDirectory*> pour un service Windows est le dossier *C:\\WINDOWS\\system32*. Le dossier *system32* n’est pas un emplacement approprié pour stocker les fichiers d’un service (tels que les fichiers de paramètres). Utilisez une des approches suivantes pour gérer les ressources ainsi que les fichiers de paramètres d’un service, et y accéder.

### <a name="set-the-content-root-path-to-the-apps-folder"></a>Définir le dossier de l’application comme chemin d’accès racine du contenu

<xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> est le même chemin que celui fourni à l’argument `binPath` quand le service est créé. Au lieu d’appeler `GetCurrentDirectory` pour créer des chemins d’accès aux fichiers de paramètres, appelez <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> en utilisant le chemin d’accès à la racine du contenu de l’application.

Dans `Program.Main`, définissez le chemin d’accès au dossier du fichier exécutable du service ainsi que le chemin d’accès pour établir la racine du contenu de l’application :

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);

CreateWebHostBuilder(args)
    .UseContentRoot(pathToContentRoot)
    .Build()
    .RunAsService();
```

### <a name="store-the-services-files-in-a-suitable-location-on-disk"></a>Stocker les fichiers du service dans un emplacement approprié sur le disque

Spécifiez un chemin absolu avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>, si vous utilisez <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder>, vers le dossier contenant les fichiers.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Configuration de point de terminaison Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (inclut la configuration de HTTPS et la prise en charge de SNI)
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>
