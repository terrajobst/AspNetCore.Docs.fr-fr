---
title: Héberger ASP.NET Core dans un service Windows
author: guardrex
description: Découvrez comment héberger une application ASP.NET Core dans un service Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: b02e627af875f15a81d68b0d625a2eccf25c0657
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72333805"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Héberger ASP.NET Core dans un service Windows

Par [Luke Latham](https://github.com/guardrex) et [Tom Dykstra](https://github.com/tdykstra)

Une application ASP.NET Core peut être hébergée sur Windows en tant que [service Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications) sans utiliser IIS. Lorsqu’elle est hébergée en tant que service Windows, l’application se lance automatiquement après le redémarrage du serveur.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Configuration requise

* [Kit de développement logiciel (SDK) ASP.NET Core 2.1 ou plus](https://dotnet.microsoft.com/download)
* [PowerShell version 6.2 ou ultérieure](https://github.com/PowerShell/PowerShell)

::: moniker range=">= aspnetcore-3.0"

## <a name="worker-service-template"></a>Modèle Service Worker

Le modèle Service Worker ASP.NET Core fournit un point de départ pour l’écriture d’applications de service durables. Pour utiliser le modèle en tant que base d’une application de service Windows :

1. Créez une application Service Worker à partir du modèle .NET Core.
1. Suivez l’aide fournie dans la section [Configuration d’application](#app-configuration) pour mettre à jour l’application Service Worker afin qu’elle puisse s’exécuter en tant que service Windows.

[!INCLUDE[](~/includes/worker-template-instructions.md)]

---

::: moniker-end

## <a name="app-configuration"></a>Configuration de l’application

::: moniker range=">= aspnetcore-3.0"

L’élément `IHostBuilder.UseWindowsService`, fourni par le package [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices), est appelé lors de la création de l’hôte. Si l’application s’exécute comme un service Windows, la méthode :

* définit la durée de vie de l’hôte sur `WindowsServiceLifetime` ;
* Définit la [racine du contenu](xref:fundamentals/index#content-root).
* Permet la journalisation dans le journal d’événements en utilisant le nom d’application en tant que nom de source par défaut.
  * Vous pouvez configurer le niveau de journalisation à l’aide de la clé `Logging:LogLevel:Default` dans le fichier *appsettings.Production.json*.
  * Seuls les administrateurs peuvent créer des sources d’événement. Si une source d’événement ne peut pas être créée en utilisant le nom de l’application, un avertissement est consigné dans la source *Application* source et les journaux d’événements sont désactivés.

[!code-csharp[](windows-service/samples/3.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

L’application nécessite des références de package pour [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) et [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).

Pour effectuer des tests et un débogage lors de l’exécution en dehors d’un service, ajoutez un code pour déterminer si l’application s’exécute comme un service ou comme une application console. Vérifiez si le débogueur est attaché ou si un switch `--console` est présent. Si l’une de ces deux conditions est remplie (l’application n’est pas exécutée en tant que service), appelez <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>. Si les conditions ne sont pas remplies (l’application est exécutée en tant que service) :

* Appelez <xref:System.IO.Directory.SetCurrentDirectory*> et utilisez un chemin vers l’emplacement publié de l’application. N’appelez pas <xref:System.IO.Directory.GetCurrentDirectory*> pour obtenir le chemin d’accès car une application Windows Service retourne le dossier *C:\\WINDOWS\\system32* lorsque <xref:System.IO.Directory.GetCurrentDirectory*> est appelée. Pour plus d’informations, consultez la section [Répertoire actif et racine du contenu](#current-directory-and-content-root). Cette étape est effectuée avant la configuration de l’application dans `CreateWebHostBuilder`.
* Appelez <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> pour exécuter l’application en tant que service.

Étant donné que le [fournisseur de configuration de ligne de commande](xref:fundamentals/configuration/index#command-line-configuration-provider) nécessite des paires nom/valeur pour les arguments de ligne de commande, le switch `--console` est supprimé des arguments avant que <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> ne les reçoive.

Pour consigner des informations dans le journal des événements Windows, ajoutez le fournisseur EventLog à <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>. Définissez le niveau de journalisation à l’aide de clé `Logging:LogLevel:Default` dans le fichier *appsettings.Production.json*.

Dans l’exemple suivant, qui provient de l’exemple d’application, l’élément `RunAsCustomService` est appelé à la place de l’élément <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> afin de gérer les événements de durée de vie au sein de l’application. Pour plus d’informations, consultez la section [Gérer les événements de démarrage et d’arrêt](#handle-starting-and-stopping-events).

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

## <a name="deployment-type"></a>Type de déploiement

Pour des informations et des conseils sur les scénarios de déploiement, consultez [Déploiement d’applications .NET Core](/dotnet/core/deploying/).

### <a name="framework-dependent-deployment-fdd"></a>Déploiement dépendant du framework

Un déploiement dépendant du framework s’appuie sur la présence d’une version partagée à l’échelle du système de .NET Core sur le système cible. Lorsque vous effectuez le scénario de déploiement dépendant du framework en suivant les conseils du présent article, le Kit de développement logiciel (SDK) produit un fichier exécutable ( *.exe*), appelé *fichier exécutable dépendant du framework*.

::: moniker range=">= aspnetcore-3.0"

Ajoutez les éléments de propriété suivants au fichier projet :

* `<OutputType>` &ndash; Le type de sortie de l’application (`Exe` pour le fichier exécutable)
* `<LangVersion>` &ndash; La version du langage C# (`latest` ou `preview`)

Un fichier *web.config*, qui est normalement produit lors de la publication d’une application ASP.NET Core, n’est pas nécessaire pour une application de Windows Services. Pour désactiver la création d’un fichier *web.config*, ajoutez la propriété `<IsTransformWebConfigDisabled>` définie sur `true`.

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp3.0</TargetFramework>
  <OutputType>Exe</OutputType>
  <LangVersion>preview</LangVersion>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[L’identificateur relatif (RID)](/dotnet/core/rid-catalog) Windows ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contient la version cible de .Net Framework. Dans l’exemple suivant, le RID est défini sur `win7-x64`. La propriété `<SelfContained>` a la valeur `false`. Ces propriétés demandent au Kit de développement logiciel (SDK) de générer un fichier exécutable ( *.exe*) pour Windows et une application qui dépend du framework .NET Core partagé.

Un fichier *web.config*, qui est normalement produit lors de la publication d’une application ASP.NET Core, n’est pas nécessaire pour une application de Windows Services. Pour désactiver la création d’un fichier *web.config*, ajoutez la propriété `<IsTransformWebConfigDisabled>` définie sur `true`.

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

[L’identificateur relatif (RID)](/dotnet/core/rid-catalog) Windows ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contient la version cible de .Net Framework. Dans l’exemple suivant, le RID est défini sur `win7-x64`. La propriété `<SelfContained>` a la valeur `false`. Ces propriétés demandent au Kit de développement logiciel (SDK) de générer un fichier exécutable ( *.exe*) pour Windows et une application qui dépend du framework .NET Core partagé.

La propriété `<UseAppHost>` a la valeur `true`. Cette propriété fournit au service un chemin d’activation (un fichier exécutable *.exe*) pour un déploiement dépendant du framework (FDD).

Un fichier *web.config*, qui est normalement produit lors de la publication d’une application ASP.NET Core, n’est pas nécessaire pour une application de Windows Services. Pour désactiver la création d’un fichier *web.config*, ajoutez la propriété `<IsTransformWebConfigDisabled>` définie sur `true`.

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

### <a name="self-contained-deployment-scd"></a>Déploiement autonome

Un déploiement autonome ne s’appuie sur la présence d’aucune infrastructure partagée sur le système hôte. Le runtime et les dépendances de l’application sont déployés avec l’application.

Un [identificateur de runtime (RID)](/dotnet/core/rid-catalog) Windows est inclus dans l’élément `<PropertyGroup>` qui contient la version cible de .NET Framework :

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

Pour publier pour plusieurs RID :

* Fournissez les RID dans une liste séparée par des points-virgules.
* Utilisez le nom de propriété [\<RuntimeIdentifiers >](/dotnet/core/tools/csproj#runtimeidentifiers) (au pluriel).

Pour plus d’informations, consultez le [Catalogue RID .NET Core](/dotnet/core/rid-catalog).

::: moniker range="< aspnetcore-3.0"

Une propriété `<SelfContained>` est définie sur `true` :

```xml
<SelfContained>true</SelfContained>
```

::: moniker-end

## <a name="service-user-account"></a>Compte d’utilisateur du service

Pour créer un compte d’utilisateur du service, utilisez la cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) depuis un interpréteur de commandes d’administration PowerShell 6.

Dans la Mise à jour de Windows 10 d’octobre 2018 (version 1809/build 10.0.17763) ou plus :

```PowerShell
New-LocalUser -Name {NAME}
```

Dans un système d’exploitation Windows antérieur à la Mise à jour de Windows 10 d’octobre 2018 (version 1809/build 10.0.17763) :

```console
powershell -Command "New-LocalUser -Name {NAME}"
```

Fournissez un [mot de passe fort](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) à l’invite.

Le compte n’expire pas, sauf si le paramètre `-AccountExpires` est fourni à la cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) avec un <xref:System.DateTime> d’expiration.

Pour plus d’informations, voir [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) et [Comptes d’utilisateurs de service](/windows/desktop/services/service-user-accounts).

Une approche alternative à la gestion des utilisateurs lors de l’utilisation d’Active Directory consiste à utiliser des Comptes de service administrés. Pour plus d’informations, consultez [Vue d’ensemble des comptes de service administrés de groupe](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).

## <a name="log-on-as-a-service-rights"></a>Droits d’ouverture de session en tant que service

Pour établir des droits *d’ouverture de session en tant que service* pour un compte d’utilisateur de service, procédez comme suit :

1. Ouvrez l’éditeur de stratégie de sécurité locale en exécutant le fichier *secpool.msc*.
1. Développez le nœud **Stratégies locales** et sélectionnez **Attribution des droits utilisateur**.
1. Ouvrez la stratégie **Ouvrir une session en tant que service**.
1. Sélectionnez **Ajouter un utilisateur ou un groupe**.
1. Fournissez le nom d’objet (compte d’utilisateur) avec l’une des approches suivantes :
   1. Tapez le compte d’utilisateur (`{DOMAIN OR COMPUTER NAME\USER}`) dans le champ du nom d’objet et sélectionnez **OK** pour ajouter l’utilisateur à la stratégie.
   1. Sélectionnez **Avancé**. Sélectionnez **Rechercher maintenant**. Sélectionnez le compte d’utilisateur dans la liste. Sélectionnez **OK**. Cliquez à nouveau sur **OK** pour ajouter l’utilisateur à la stratégie.
1. Sélectionnez **OK** ou **Appliquer** pour accepter les modifications.

## <a name="create-and-manage-the-windows-service"></a>Créer et gérer le service Windows

### <a name="create-a-service"></a>Créer un service

Utilisez les commandes PowerShell pour enregistrer un service. À partir d’un interpréteur de commandes d’administration PowerShell 6, exécutez les commandes suivantes :

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* `{EXE PATH}` &ndash; Chemin d’accès au dossier de l’application sur l’hôte (par exemple, `d:\myservice`). N’incluez pas le fichier exécutable de l’application dans le chemin. Aucune barre oblique de fin n’est nécessaire.
* `{DOMAIN OR COMPUTER NAME\USER}` &ndash; Compte d’utilisateur du service (par exemple, `Contoso\ServiceUser`).
* `{NAME}` &ndash; Nom du service (par exemple, `MyService`).
* `{EXE FILE PATH}` &ndash; Chemin du fichier exécutable de l’application (par exemple, `d:\myservice\myservice.exe`). Incluez le nom de fichier de l’exécutable avec l’extension.
* `{DESCRIPTION}` &ndash; Description du service (par exemple, `My sample service`).
* `{DISPLAY NAME}` &ndash; Nom d’affichage du service (par exemple, `My Service`).

### <a name="start-a-service"></a>Démarrer un service

Démarrez un service avec la commande PowerShell 6 suivante :

```powershell
Start-Service -Name {NAME}
```

La commande prend quelques secondes pour démarrer le service.

### <a name="determine-a-services-status"></a>Déterminer l’état d’un service

Pour vérifier l’état d’un service, utilisez la commande PowerShell 6 suivante :

```powershell
Get-Service -Name {NAME}
```

L’état est signalé comme étant l’une des valeurs suivantes :

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a>Arrêter un service

Arrêtez un service avec la commande Powershell 6 suivante :

```powershell
Stop-Service -Name {NAME}
```

### <a name="remove-a-service"></a>Supprimer un service

Après un court délai pour arrêter un service, supprimez-le avec la commande PowerShell 6 suivante :

```powershell
Remove-Service -Name {NAME}
```

::: moniker range="< aspnetcore-3.0"

## <a name="handle-starting-and-stopping-events"></a>Gérer les événements de démarrage et d’arrêt

Pour gérer les événements <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> et <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*>, procédez comme suit :

1. Créez une classe qui dérive de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> à l’aide des méthodes `OnStarting`, `OnStarted` et `OnStopping` :

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. Créez une méthode d’extension pour <xref:Microsoft.AspNetCore.Hosting.IWebHost> qui transmet `CustomWebHostService` à <xref:System.ServiceProcess.ServiceBase.Run*> :

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. Dans `Program.Main`, appelez la méthode d’extension `RunAsCustomService` au lieu de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> :

   ```csharp
   host.RunAsCustomService();
   ```

   Pour afficher l’emplacement de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> dans `Program.Main`, reportez-vous à l’exemple de code indiqué dans la section [Type de déploiement](#deployment-type).

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a>Scénarios avec un serveur proxy et un équilibreur de charge

Les services qui interagissent avec les requêtes provenant d’Internet ou d’un réseau d’entreprise et qui se trouvent derrière un proxy ou équilibreur de charge peuvent nécessiter une configuration supplémentaire. Pour plus d'informations, consultez <xref:host-and-deploy/proxy-load-balancer>.

## <a name="configure-endpoints"></a>Configurer des points de terminaison

Par défaut, ASP.NET Core est lié à `http://localhost:5000`. Configurez l’URL et le port en définissant la variable d’environnement `ASPNETCORE_URLS`.

Pour obtenir d’autres approches de configuration des ports et des URL, consultez l’article approprié sur le serveur :

* <xref:fundamentals/servers/kestrel#endpoint-configuration>
* <xref:fundamentals/servers/httpsys#configure-windows-server>

L’aide précédente couvre la prise en charge des points de terminaison HTTPs. Par exemple, configurez l’application pour HTTPs lorsque l’authentification est utilisée avec un service Windows.

> [!NOTE]
> L’utilisation du certificat de développement HTTPS ASP.NET Core pour sécuriser un point de terminaison de service n’est pas prise en charge.

## <a name="current-directory-and-content-root"></a>Répertoire actif et racine du contenu

Le répertoire de travail actif retourné par l’appel à <xref:System.IO.Directory.GetCurrentDirectory*> pour un service Windows est le dossier *C:\\WINDOWS\\system32*. Le dossier *system32* n’est pas un emplacement approprié pour stocker les fichiers d’un service (tels que les fichiers de paramètres). Utilisez une des approches suivantes pour gérer les ressources ainsi que les fichiers de paramètres d’un service, et y accéder.

::: moniker range=">= aspnetcore-3.0"

### <a name="use-contentrootpath-or-contentrootfileprovider"></a>Utiliser ContentRootPath ou ContentRootFileProvider

Utilisez les éléments [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) ou <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> pour localiser les ressources d’une application.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="set-the-content-root-path-to-the-apps-folder"></a>Définir le dossier de l’application comme chemin d’accès racine du contenu

La chaîne <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> correspond au même chemin que celui fourni à l’argument `binPath` lorsqu’un service est créé. Au lieu d’appeler `GetCurrentDirectory` pour créer des chemins d’accès aux fichiers de paramètres, appelez <xref:System.IO.Directory.SetCurrentDirectory*> avec le chemin d’accès à la [racine de contenu](xref:fundamentals/index#content-root)de l’application.

Dans `Program.Main`, définissez le chemin d’accès au dossier du fichier exécutable du service ainsi que le chemin d’accès pour établir la racine du contenu de l’application :

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

::: moniker-end

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a>Stocker les fichiers d’un service dans un emplacement approprié sur le disque

Spécifiez un chemin absolu avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>, si vous utilisez <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder>, vers le dossier contenant les fichiers.

## <a name="additional-resources"></a>Ressources supplémentaires

::: moniker range=">= aspnetcore-3.0"

* [Configuration de point de terminaison Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (inclut la configuration de HTTPS et la prise en charge de SNI)
* <xref:fundamentals/host/generic-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* [Configuration de point de terminaison Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (inclut la configuration de HTTPS et la prise en charge de SNI)
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end
