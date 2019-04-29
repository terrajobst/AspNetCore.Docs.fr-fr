---
title: Héberger ASP.NET Core dans un service Windows
author: guardrex
description: Découvrez comment héberger une application ASP.NET Core dans un service Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/04/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: 544eefa87898e82ec2bf8f9f61ce4e26dd554bb7
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59068334"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Héberger ASP.NET Core dans un service Windows

Par [Luke Latham](https://github.com/guardrex) et [Tom Dykstra](https://github.com/tdykstra)

Une application ASP.NET Core peut être hébergée sur Windows en tant que [service Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications) sans utiliser IIS. Lorsqu’elle est hébergée en tant que service Windows, l’application démarre automatiquement après le redémarrage.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prérequis

* [PowerShell version 6.2 ou ultérieure](https://github.com/PowerShell/PowerShell)

> [!NOTE]
> Pour une version du système d’exploitation Windows antérieure à la Mise à jour d’octobre 2018 de Windows 10 (version 1809/build 10.0.17763), le module [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts) doit être importé avec le module [WindowsCompatibility](https://github.com/PowerShell/WindowsCompatibility) pour permettre l’accès à la cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) utilisée dans la section [Créer un compte d’utilisateur](#create-a-user-account) :
>
> ```powershell
> Install-Module WindowsCompatibility -Scope CurrentUser
> Import-WinModule Microsoft.PowerShell.LocalAccounts
> ```

## <a name="deployment-type"></a>Type de déploiement

Vous pouvez créer un déploiement Windows Service dépendant du framework ou autonome. Pour des informations et des conseils sur les scénarios de déploiement, consultez [Déploiement d’applications .NET Core](/dotnet/core/deploying/).

### <a name="framework-dependent-deployment"></a>Déploiement dépendant du framework

Un déploiement dépendant du framework s’appuie sur la présence d’une version partagée à l’échelle du système de .NET Core sur le système cible. Lorsque le scénario de déploiement dépendant du framework est utilisé avec une application de service Windows ASP.NET Core, le Kit de développement logiciel (SDK) génère un fichier exécutable (*\*.exe*), appelé *exécutable dépendant du framework*.

### <a name="self-contained-deployment"></a>Déploiement autonome

Un déploiement autonome ne s’appuie sur la présence d’aucun composant partagé sur le système cible. Le runtime et les dépendances de l’application sont déployés avec l’application sur le système d’hébergement.

## <a name="convert-a-project-into-a-windows-service"></a>Convertir un projet en service Windows

Apportez les modifications suivantes à un projet ASP.NET Core existant pour exécuter l’application en tant que service :

### <a name="project-file-updates"></a>Mises à jour du fichier projet

Selon le [type de déploiement](#deployment-type) que vous avez choisi, mettez à jour le fichier projet :

#### <a name="framework-dependent-deployment-fdd"></a>Déploiement dépendant du framework

Ajoutez un [identificateur de runtime (RID)](/dotnet/core/rid-catalog) Windows au `<PropertyGroup>` qui contient la version cible du .NET Framework. Dans l’exemple suivant, le RID est défini sur `win7-x64`. Ajoutez la propriété `<SelfContained>` définie sur `false`. Ces propriétés demandent au Kit SDK de générer un fichier exécutable (*.exe*) pour Windows.

Un fichier *web.config*, qui est normalement produit lors de la publication d’une application ASP.NET Core, n’est pas nécessaire pour une application de Windows Services. Pour désactiver la création d’un fichier *web.config*, ajoutez la propriété `<IsTransformWebConfigDisabled>` définie sur `true`.

::: moniker range=">= aspnetcore-2.2"

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.1"

Ajoutez la propriété `<UseAppHost>` définie sur `true`. Cette propriété fournit au service un chemin d’activation (un fichier exécutable *.exe*) pour un déploiement dépendant du framework (FDD).

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.1</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <UseAppHost>true</UseAppHost>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

#### <a name="self-contained-deployment-scd"></a>Déploiement autonome

Vérifiez la présence d’un [identificateur de runtime (RID)](/dotnet/core/rid-catalog) Windows ou ajoutez-le au `<PropertyGroup>` qui contient la version cible du .NET Framework. Désactivez la création d’un fichier *web.config* en ajoutant la propriété `<IsTransformWebConfigDisabled>` définie sur `true`.

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

Ajoutez une référence de package pour [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).

Pour activer l’enregistrement du journal des événements Windows, ajoutez une référence de package pour [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).

Pour plus d’informations, consultez la section [Gérer les événements de démarrage et d’arrêt](#handle-starting-and-stopping-events).

### <a name="programmain-updates"></a>Mises à jour Program.Main

Dans `Program.Main`, effectuez les changements suivants :

* Pour effectuer des tests et un débogage lors de l’exécution en dehors d’un service, ajoutez un code pour déterminer si l’application s’exécute comme un service ou comme une application console. Vérifiez si le débogueur est attaché ou si un argument de ligne de commande `--console` est présent.

  Si l’une de ces deux conditions est remplie (l’application n’est pas exécutée en tant que service), appelez <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> sur l’hôte web.

  Si les conditions ne sont pas remplies (l’application est exécutée en tant que service) :

  * Appelez <xref:System.IO.Directory.SetCurrentDirectory*> et utilisez un chemin vers l’emplacement publié de l’application. N’appelez pas <xref:System.IO.Directory.GetCurrentDirectory*> pour obtenir le chemin d’accès car une application Windows Service retourne le dossier *C:\\WINDOWS\\system32* lorsque <xref:System.IO.Directory.GetCurrentDirectory*> est appelée. Pour plus d’informations, consultez la section [Répertoire actif et racine du contenu](#current-directory-and-content-root).
  * Appelez <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> pour exécuter l’application en tant que service.

  Étant donné que le [fournisseur de configuration de ligne de commande](xref:fundamentals/configuration/index#command-line-configuration-provider) nécessite des paires nom/valeur pour les arguments de ligne de commande, le commutateur `--console` est supprimé des arguments avant que <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> ne les reçoive.

* Pour consigner des informations dans le journal des événements Windows, ajoutez le fournisseur EventLog à <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>. Définissez le niveau de journalisation à l’aide de clé `Logging:LogLevel:Default` dans le fichier *appsettings.Production.json*. À des fins de démonstration et de test, le fichier des paramètres de l’exemple d’application Production définit le niveau de journalisation sur `Information`. En production, la valeur est généralement définie sur `Error`. Pour plus d'informations, consultez <xref:fundamentals/logging/index#windows-eventlog-provider>.

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

## <a name="publish-the-app"></a>Publier l'application

Publiez l’application avec [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), un [profil de publication Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) ou Visual Studio Code. Si vous utilisez Visual Studio, sélectionnez **FolderProfile** et configurez **Emplacement cible** avant de sélectionner le bouton **Publier**.

Pour publier l’exemple d’application avec des outils d’interface de ligne de commande (CLI), exécutez la commande [dotnet publish](/dotnet/core/tools/dotnet-publish) dans une interface de commande Windows à partir du dossier du projet, en passant la configuration de mise en production (Release) à l’option [-c|--configuration](/dotnet/core/tools/dotnet-publish#options). Utilisez l’option [-o|--output](/dotnet/core/tools/dotnet-publish#options) avec un chemin d'accès pour publier dans un dossier à l’extérieur de l’application.

### <a name="publish-a-framework-dependent-deployment-fdd"></a>Publier un déploiement dépendant du framework

Dans l’exemple suivant, l’application est publiée dans le dossier *c:\\svc* :

```console
dotnet publish --configuration Release --output c:\svc
```

### <a name="publish-a-self-contained-deployment-scd"></a>Publier un déploiement autonome

Le RID doit être spécifié dans la propriété `<RuntimeIdenfifier>` (ou `<RuntimeIdentifiers>`) du fichier projet. Fournissez le runtime à l’option [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) de la commande `dotnet publish`.

Dans l’exemple suivant, l’application est publiée pour le runtime `win7-x64` dans le dossier *c:\\svc* :

```console
dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
```

## <a name="create-a-user-account"></a>Créer un compte d’utilisateur

Créez un compte d’utilisateur pour le service avec la cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) dans une interface de commande d’administration PowerShell 6 :

```powershell
New-LocalUser -Name {NAME}
```

Fournissez un [mot de passe fort](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) à l’invite.

Pour l’exemple d’application, créez un compte d’utilisateur portant le nom `ServiceUser`.

```powershell
New-LocalUser -Name ServiceUser
```

Le compte n’expire pas, sauf si le paramètre `-AccountExpires` est fourni à la cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) avec un <xref:System.DateTime> d’expiration.

Pour plus d’informations, voir [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) et [Comptes d’utilisateurs de service](/windows/desktop/services/service-user-accounts).

Une approche alternative à la gestion des utilisateurs lors de l’utilisation d’Active Directory consiste à utiliser des Comptes de service administrés. Pour plus d’informations, consultez [Vue d’ensemble des comptes de service administrés de groupe](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).

## <a name="set-permission-log-on-as-a-service"></a>Définir l’autorisation : Ouvrir une session en tant que service

Accordez l’accès en écriture/lecture/exécution au dossier de l’application avec la commande [icacls](/windows-server/administration/windows-commands/icacls) dans une interface de commande d’administration PowerShell 6.

```powershell
icacls "{PATH}" /grant "{USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS}" /t
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

Pour l’exemple d’application publiée dans le dossier *c:\\svc* et le compte `ServiceUser` avec des autorisations en écriture/lecture/exécution, utilisez la commande suivante dans une interface de commande d’administration PowerShell 6.

```powershell
icacls "c:\svc" /grant "ServiceUser:(OI)(CI)WRX" /t
```

Pour plus d’informations, consultez [icacls](/windows-server/administration/windows-commands/icacls).

## <a name="create-the-service"></a>Créer le service

Utilisez le script PowerShell [RegisterService.ps1](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/scripts) pour inscrire le service. Dans une interface de commande d’administration PowerShell 6, exécutez le script avec la commande suivante :

```powershell
.\RegisterService.ps1 
    -Name {NAME} 
    -DisplayName "{DISPLAY NAME}" 
    -Description "{DESCRIPTION}" 
    -Exe "{PATH TO EXE}\{ASSEMBLY NAME}.exe" 
    -User {DOMAIN\USER}
```

Dans l’exemple suivant pour l’exemple d’application :

* Le service s’appelle **MyService**.
* Le service publié réside dans le dossier *c:\\svc*. L’application exécutable s’appelle *SampleApp.exe*.
* Le service s’exécute sous le compte `ServiceUser`. Dans l’exemple de commande suivant, le nom de l’ordinateur local est `Desktop-PC`. Remplacez `Desktop-PC` par le nom de l’ordinateur ou le domaine de votre système.

```powershell
.\RegisterService.ps1 
    -Name MyService 
    -DisplayName "My Cool Service" 
    -Description "This is the Sample App service." 
    -Exe "c:\svc\SampleApp.exe" 
    -User Desktop-PC\ServiceUser
```

## <a name="manage-the-service"></a>Gérer le service

### <a name="start-the-service"></a>Démarrer le service

Démarrez le service avec la commande PowerShell 6 `Start-Service -Name {NAME}`.

Pour démarrer l’exemple de service d’application, utilisez la commande suivante :

```powershell
Start-Service -Name MyService
```

La commande prend quelques secondes pour démarrer le service.

### <a name="determine-the-service-status"></a>Déterminer l’état du service

Pour vérifier l’état du service, utilisez la commande PowerShell 6 `Get-Service -Name {NAME}`. L’état est signalé comme étant l’une des valeurs suivantes :

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

Utilisez la commande suivante pour vérifier l’état de l’exemple de service d’application :

```powershell
Get-Service -Name MyService
```

### <a name="browse-a-web-app-service"></a>Parcourir un service d’application web

Quand le service est une application web et que son état est `RUNNING`, accédez au chemin de l'application (par défaut, `http://localhost:5000`, qui redirige vers `https://localhost:5001` si le [middleware (intergiciel) de redirection HTTPS](xref:security/enforcing-ssl) est utilisé).

Pour l’exemple de service d’application, accédez au chemin de l'application sur `http://localhost:5000`.

### <a name="stop-the-service"></a>Arrêter le service

Arrêtez le service avec la commande PowerShell 6 `Stop-Service -Name {NAME}`.

La commande suivante arrête l’exemple de service d’application :

```powershell
Stop-Service -Name MyService
```

### <a name="remove-the-service"></a>Supprimer le service

Après un court délai pour arrêter un service, supprimez le service avec la commande PowerShell 6 `Remove-Service -Name {NAME}`.

Vérifiez l’état de l’exemple de service d’application :

```powershell
Remove-Service -Name MyService
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

<xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> est le même chemin que celui fourni à l’argument `binPath` quand le service est créé. Au lieu d’appeler `GetCurrentDirectory` pour créer des chemins d’accès aux fichiers de paramètres, appelez <xref:System.IO.Directory.SetCurrentDirectory*> en utilisant le chemin d’accès à la racine du contenu de l’application.

Dans `Program.Main`, définissez le chemin d’accès au dossier du fichier exécutable du service ainsi que le chemin d’accès pour établir la racine du contenu de l’application :

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

### <a name="store-the-services-files-in-a-suitable-location-on-disk"></a>Stocker les fichiers du service dans un emplacement approprié sur le disque

Spécifiez un chemin absolu avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>, si vous utilisez <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder>, vers le dossier contenant les fichiers.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Configuration de point de terminaison Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (inclut la configuration de HTTPS et la prise en charge de SNI)
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>
