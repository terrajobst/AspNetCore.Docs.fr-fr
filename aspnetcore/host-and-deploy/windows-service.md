---
title: Héberger ASP.NET Core dans un service Windows
author: guardrex
description: Découvrez comment héberger une application ASP.NET Core dans un service Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/30/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 11913019bfe5d06c259b806fce9cc580a8280ad5
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758191"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Héberger ASP.NET Core dans un service Windows

Par [Luke Latham](https://github.com/guardrex) et [Tom Dykstra](https://github.com/tdykstra)

Une application ASP.NET Core peut être hébergée sur Windows sans utiliser IIS en tant que [service Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications). Lorsqu’elle est hébergée en tant que service Windows, l’application démarre automatiquement après le redémarrage.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="convert-a-project-into-a-windows-service"></a>Convertir un projet en service Windows

Les modifications minimales suivantes sont nécessaires pour configurer un projet ASP.NET Core existant afin qu’il s’exécute en tant que service :

1. Dans le fichier projet :

   * Vérifiez la présence d’un [identificateur de runtime (RID)](/dotnet/core/rid-catalog) Windows ou ajoutez-le à `<PropertyGroup>` qui contient la version cible du .Net Framework :

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.2</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      Pour publier pour plusieurs RID :

      * Fournissez les RID dans une liste séparée par des points-virgules.
      * Utilisez le nom de la propriété `<RuntimeIdentifiers>` (pluriel).

      Pour plus d’informations, consultez le [Catalogue RID .NET Core](/dotnet/core/rid-catalog).

   * Ajoutez une référence de package pour [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).

1. Dans `Program.Main`, effectuez les changements suivants :

   * Appelez [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) au lieu de `host.Run`.

   * Appelez [UseContentRoot](xref:fundamentals/host/web-host#content-root) et utilisez un chemin vers l’emplacement publié de l’application au lieu de `Directory.GetCurrentDirectory()`.

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

1. Publiez l’application avec [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), un [profil de publication Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) ou Visual Studio Code. Si vous utilisez Visual Studio, sélectionnez **FolderProfile** et configurez **Emplacement cible** avant de sélectionner le bouton **Publier**.

   Pour publier l’exemple d’application avec des outils de l’interface de ligne de commande (CLI), exécutez la commande [dotnet publish](/dotnet/core/tools/dotnet-publish) à une invite de commandes à partir du dossier du projet. Le RID doit être spécifié dans la propriété `<RuntimeIdenfifier>` (ou `<RuntimeIdentifiers>`) du fichier projet. Dans l’exemple suivant, l’application est publiée dans la configuration Release pour le runtime `win7-x64` sur un dossier créé à l’emplacement *c:\\svc* :

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
   * `{DOMAIN}` (ou, si l’ordinateur n’est pas joint à un domaine, nom de l’ordinateur local) et `{USER ACCOUNT}` &ndash; Domaine (ou nom de l’ordinateur local) et compte d’utilisateur sous lesquels le service s’exécute. **N’oubliez pas** le paramètre `obj`. `obj` a pour valeur par défaut le compte [LocalSystem](/windows/desktop/services/localsystem-account). L’exécution d’un service sous le compte `LocalSystem` présente un risque de sécurité important. Veillez à toujours exécuter un service sous un compte d’utilisateur disposant de privilèges limités sur le serveur.
   * `{PASSWORD}` &ndash; Mot de passe du compte d’utilisateur.

   Dans l’exemple suivant :

   * Le service s’appelle **MyService**.
   * Le service publié réside dans le dossier *c:\\svc*. L’exécutable de l’application s’appelle *AspNetCoreService.exe*. La valeur `binPath` est placée entre des guillemets droits (").
   * Le service s’exécute sous le compte `ServiceUser`. Remplacez `{DOMAIN}` par le nom de l’ordinateur local ou le domaine du compte d’utilisateur. Placez la valeur `obj` entre des guillemets droits ("). Exemple : Si le système d’hébergement est un ordinateur local nommé `MairaPC`, définissez `obj` avec la valeur `"MairaPC\ServiceUser"`.
   * Remplacez `{PASSWORD}` par le mot de passe du compte d’utilisateur. La valeur `password` est placée entre des guillemets droits (").

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe" obj= "{DOMAIN}\ServiceUser" password= "{PASSWORD}"
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

## <a name="run-the-app-outside-of-a-service"></a>Exécuter l’application en dehors d’un service

Comme il est plus facile d’effectuer des tests et des débogages pendant une exécution en dehors d’un service, il est habituel d’ajouter du code qui appelle `RunAsService` uniquement sous certaines conditions. Par exemple, l’application peut s’exécuter en tant qu’application console avec un argument de ligne de commande `--console` ou si le débogueur est attaché :

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

Étant donné que la configuration d’ASP.NET Core nécessite des paires nom/valeur pour les arguments de ligne de commande, le commutateur `--console` est supprimé avant que les arguments ne soient passés à [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).

> [!NOTE]
> `isService` n’est pas passé de `Main` vers `CreateWebHostBuilder`, car la signature de `CreateWebHostBuilder` doit être `CreateWebHostBuilder(string[])` afin que les [tests d’intégration](xref:test/integration-tests) fonctionnent correctement.

## <a name="handle-stopping-and-starting-events"></a>Gérer les événements d’arrêt et de démarrage

Pour gérer les événements [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted) et [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping), apportez les modifications supplémentaires suivantes :

1. Créez une classe qui dérive de [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice) :

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. Créez une méthode d’extension pour [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) qui transmet le `WebHostService` personnalisé à [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run) :

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. Dans `Program.Main`, appelez la nouvelle méthode d’extension, `RunAsCustomService`, au lieu de [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) :

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > `isService` n’est pas passé de `Main` vers `CreateWebHostBuilder`, car la signature de `CreateWebHostBuilder` doit être `CreateWebHostBuilder(string[])` afin que les [tests d’intégration](xref:test/integration-tests) fonctionnent correctement.

Si le code `WebHostService` personnalisé nécessite un service à partir d’une injection de dépendances (par exemple, un enregistreur d’événements), obtenez-le à partir de la propriété [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) :

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>Scénarios avec un serveur proxy et un équilibreur de charge

Les services qui interagissent avec les requêtes provenant d’Internet ou d’un réseau d’entreprise et qui se trouvent derrière un proxy ou équilibreur de charge peuvent nécessiter une configuration supplémentaire. Pour plus d'informations, consultez <xref:host-and-deploy/proxy-load-balancer>.

## <a name="configure-https"></a>Configurer HTTPS

Pour configurer le service avec un point de terminaison sécurisé :

1. Créez un certificat X.509 pour le système d’hébergement à l’aide des mécanismes d’acquisition et de déploiement de certificat de votre plateforme.
1. Spécifiez la [configuration de point de terminaison HTTPS d’un serveur Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) pour utiliser le certificat.

L’utilisation du certificat de développement HTTPS ASP.NET Core pour sécuriser un point de terminaison de service n’est pas prise en charge.

## <a name="current-directory-and-content-root"></a>Répertoire actif et racine du contenu

Le répertoire de travail actif retourné par l’appel à `Directory.GetCurrentDirectory()` pour un service Windows est le dossier *C:\\WINDOWS\\system32*. Le dossier *system32* n’est pas un emplacement approprié pour stocker les fichiers d’un service (tels que les fichiers de paramètres). Adoptez l’une des approches suivantes pour tenir à jour et accéder aux ressources et aux fichiers de paramètres d’un service avec [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) quand vous utilisez un [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) :

* Utilisez le chemin racine du contenu. `IHostingEnvironment.ContentRootPath` est le même chemin que celui fourni à l’argument `binPath` quand le service est créé. Au lieu d’utiliser `Directory.GetCurrentDirectory()` pour créer des chemins aux fichiers de paramètres, utilisez le chemin racine du contenu et tenez à jour les fichiers dans la racine du contenu de l’application.
* Stockez les fichiers à un emplacement approprié sur le disque. Spécifiez un chemin absolu avec `SetBasePath` au dossier contenant les fichiers.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Configuration de point de terminaison Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (inclut la configuration de HTTPS et la prise en charge de SNI)
* <xref:fundamentals/host/web-host>
