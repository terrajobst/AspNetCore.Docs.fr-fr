---
title: Héberger ASP.NET Core sur Windows avec IIS
author: guardrex
description: Découvrez comment héberger des applications ASP.NET Core sur Windows Server Internet Information Services (IIS).
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2018
uid: host-and-deploy/iis/index
ms.openlocfilehash: 4356d986731f915c2e76a4c4863f951572820de0
ms.sourcegitcommit: 816f39e852a8f453e8682081871a31bc66db153a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53637872"
---
# <a name="host-aspnet-core-on-windows-with-iis"></a>Héberger ASP.NET Core sur Windows avec IIS

Par [Luke Latham](https://github.com/guardrex)

[Installer le bundle d’hébergement .NET Core](#install-the-net-core-hosting-bundle)

## <a name="supported-operating-systems"></a>Systèmes d'exploitation pris en charge

Les systèmes d’exploitation suivants sont pris en charge :

* Windows 7 ou version ultérieure
* Windows Server 2008 R2 ou une version ultérieure

Le [serveur HTTP.sys](xref:fundamentals/servers/httpsys) (anciennement WebListener) ne fonctionne pas dans une configuration de proxy inverse avec IIS. Utilisez le [serveur Kestrel](xref:fundamentals/servers/kestrel).

Pour plus d’informations sur l’hébergement dans Azure, consultez <xref:host-and-deploy/azure-apps/index>.

## <a name="supported-platforms"></a>Plateformes prises en charge

Les applications publiées pour les déploiements 32 bits (x86) et 64 bits (x64) sont prises en charge. Déployez une application 32 bits, sauf si l’application :

* Nécessite l’espace d’adressage de mémoire virtuelle le plus grand disponible pour une application 64 bits.
* Nécessite la taille de pile IIS la plus grande disponible.
* A des dépendances natives 64 bits.

## <a name="application-configuration"></a>Configuration d’application

### <a name="enable-the-iisintegration-components"></a>Activer les composants IISIntegration

::: moniker range=">= aspnetcore-2.1"

Un fichier *Program.cs* standard appelle <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> pour commencer à configurer un hôte :

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Un fichier *Program.cs* standard appelle <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> pour commencer à configurer un hôte :

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

**Modèle d’hébergement in-process**

`CreateDefaultBuilder` appelle la méthode `UseIIS` pour démarrer le [CoreCLR](/dotnet/standard/glossary#coreclr) et héberger l’application à l’intérieur du processus Worker (*w3wp.exe* ou *iisexpress.exe*). Les tests de performance indiquent que l’hébergement d’une application.NET Core in-process fournit un débit de requêtes beaucoup plus élevé que l’hébergement de l’application out-of-process et la redirection par le biais d’un proxy des requêtes vers le serveur [Kestrel](xref:fundamentals/servers/kestrel).

**Modèle d’hébergement out-of-process**

Pour l’hébergement out-of-process avec IIS, `CreateDefaultBuilder` configure le serveur [Kestrel](xref:fundamentals/servers/kestrel) comme serveur web et permet l’intégration IIS en configurant le chemin de base et le port du [module ASP.NET Core ](xref:host-and-deploy/aspnet-core-module).

Le module ASP.NET Core génère un port dynamique à assigner au processus backend. `CreateDefaultBuilder` appelle la méthode <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*>. `UseIISIntegration` configure Kestrel pour écouter sur le port dynamique à l’adresse IP localhost (`127.0.0.1`). Si le port dynamique est 1234, Kestrel écoute sur `127.0.0.1:1234`. Cette configuration remplace les autres configurations URL fournies par :

* `UseUrls`
* [API d’écoute Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [Configuration](xref:fundamentals/configuration/index) (ou [options --url de ligne de commande](xref:fundamentals/host/web-host#override-configuration))

L’utilisation du module évite les appels à `UseUrls` ou à l’API `Listen` de Kestrel. Si `UseUrls` ou `Listen` est appelé, Kestrel écoute sur les ports spécifiés uniquement lors de l’exécution de l’application sans IIS.

Pour plus d’informations sur les modèles d’hébergement in-process et out-of-process, consultez [Module ASP.NET Core](xref:host-and-deploy/aspnet-core-module) et [Informations de référence sur la configuration du module ASP.NET Core](xref:host-and-deploy/aspnet-core-module).

::: moniker-end

::: moniker range="= aspnetcore-2.1"

`CreateDefaultBuilder` configure le serveur [Kestrel](xref:fundamentals/servers/kestrel) comme serveur web et active l’intégration d’IIS en configurant le chemin et le port de base pour le [module ASP.NET Core](xref:host-and-deploy/aspnet-core-module).

Le module ASP.NET Core génère un port dynamique à assigner au processus backend. `CreateDefaultBuilder` appelle la méthode [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration). `UseIISIntegration` configure Kestrel pour écouter sur le port dynamique à l’adresse IP localhost (`127.0.0.1`). Si le port dynamique est 1234, Kestrel écoute sur `127.0.0.1:1234`. Cette configuration remplace les autres configurations URL fournies par :

* `UseUrls`
* [API d’écoute Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [Configuration](xref:fundamentals/configuration/index) (ou [options --url de ligne de commande](xref:fundamentals/host/web-host#override-configuration))

L’utilisation du module évite les appels à `UseUrls` ou à l’API `Listen` de Kestrel. Si `UseUrls` ou `Listen` est appelé, Kestrel écoute sur le port spécifié uniquement lors de l’exécution de l’application sans IIS.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

`CreateDefaultBuilder` configure le serveur [Kestrel](xref:fundamentals/servers/kestrel) comme serveur web et active l’intégration d’IIS en configurant le chemin et le port de base pour le [module ASP.NET Core](xref:host-and-deploy/aspnet-core-module).

Le module ASP.NET Core génère un port dynamique à assigner au processus backend. `CreateDefaultBuilder` appelle la méthode [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration). `UseIISIntegration` configure Kestrel pour écouter sur le port dynamique à l’adresse IP localhost (`localhost`). Si le port dynamique est 1234, Kestrel écoute sur `localhost:1234`. Cette configuration remplace les autres configurations URL fournies par :

* `UseUrls`
* [API d’écoute Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [Configuration](xref:fundamentals/configuration/index) (ou [options --url de ligne de commande](xref:fundamentals/host/web-host#override-configuration))

L’utilisation du module évite les appels à `UseUrls` ou à l’API `Listen` de Kestrel. Si `UseUrls` ou `Listen` est appelé, Kestrel écoute sur le port spécifié uniquement lors de l’exécution de l’application sans IIS.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Incluez une dépendance envers le package [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) dans les dépendances de l’application. Utilisez l’intergiciel (middleware) d’intégration IIS en ajoutant la méthode d’extension [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) à [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) :

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

[UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) et [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) sont tous les deux requis. Le code qui appelle `UseIISIntegration` n’affecte pas la portabilité du code. Si l’application n’est pas exécutée derrière IIS (par exemple si l’application est exécutée directement sur Kestrel), `UseIISIntegration` n’effectue pas d’opérations.

Le module ASP.NET Core génère un port dynamique à assigner au processus backend. `UseIISIntegration` configure Kestrel pour écouter sur le port dynamique à l’adresse IP localhost (`localhost`). Si le port dynamique est 1234, Kestrel écoute sur `localhost:1234`. Cette configuration remplace les autres configurations URL fournies par :

* `UseUrls`
* [Configuration](xref:fundamentals/configuration/index) (ou [options --url de ligne de commande](xref:fundamentals/host/web-host#override-configuration))

L’utilisation du module rend inutile tout appel à `UseUrls`. Si `UseUrls` est appelé, Kestrel écoute sur le port spécifié uniquement lors de l’exécution de l’application sans IIS.

Si `UseUrls` est appelé dans une application ASP.NET Core 1.0, appelez-le **avant** d’appeler `UseIISIntegration` pour que le port configuré par le module ne soit pas remplacé. Cet ordre d’appel est inutile avec ASP.NET Core 1.1, car le paramètre du module remplace `UseUrls`.

::: moniker-end

Pour plus d’informations sur l’hébergement, consultez [Héberger dans ASP.NET Core](xref:fundamentals/host/index).

### <a name="iis-options"></a>Options IIS

::: moniker range=">= aspnetcore-2.2"

**Modèle d’hébergement in-process**

Pour configurer des options IIS Server, ajoutez une configuration de services pour [IISServerOptions](/dotnet/api/microsoft.aspnetcore.builder.iisserveroptions) dans [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices). L’exemple suivant désactive AutomaticAuthentication :

```csharp
services.Configure<IISServerOptions>(options => 
{
    options.AutomaticAuthentication = false;
});
```

| Option                         | Par défaut | Paramètre |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | Si `true`, IIS Server définit le `HttpContext.User` authentifié par [Authentification Windows](xref:security/authentication/windowsauth). Si `false`, le serveur fournit uniquement une identité pour `HttpContext.User` et répond aux questions explicitement posées par `AuthenticationScheme`. L’authentification Windows doit être activée dans IIS pour que `AutomaticAuthentication` fonctionne. Pour plus d’informations, consultez [Authentification Windows](xref:security/authentication/windowsauth). |
| `AuthenticationDisplayName`    | `null`  | Définit le nom d’affichage montré aux utilisateurs sur les pages de connexion. |

**Modèle d’hébergement out-of-process**

::: moniker-end

Pour configurer des options IIS, ajoutez une configuration de services pour [IISOptions](/dotnet/api/microsoft.aspnetcore.builder.iisoptions) dans [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices). L’exemple suivant empêche l’application d’être renseignée `HttpContext.Connection.ClientCertificate` :

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| Option                         | Par défaut | Paramètre |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | Si `true`, l’Intergiciel (Middleware) d’intégration IIS définit le `HttpContext.User` authentifié par [Authentification Windows](xref:security/authentication/windowsauth). Si `false`, l’intergiciel (middleware) fournit uniquement une identité pour `HttpContext.User` et répond aux questions explicitement posées par `AuthenticationScheme`. L’authentification Windows doit être activée dans IIS pour que `AutomaticAuthentication` fonctionne. Pour plus d'informations, consultez la rubrique [Authentification Windows](xref:security/authentication/windowsauth). |
| `AuthenticationDisplayName`    | `null`  | Définit le nom d’affichage montré aux utilisateurs sur les pages de connexion. |
| `ForwardClientCertificate`     | `true`  | Si la valeur est `true` et que l’en-tête de requête `MS-ASPNETCORE-CLIENTCERT` est présent, `HttpContext.Connection.ClientCertificate` est renseigné. |

### <a name="proxy-server-and-load-balancer-scenarios"></a>Scénarios avec un serveur proxy et un équilibreur de charge

L’intergiciel (middleware) d’intégration IIS, qui configure l’intergiciel des en-têtes transférés, et le module ASP.NET Core, sont configurés pour transférer le schéma (HTTP/HTTPS) et l’adresse IP distante d’où provient la requête. Une configuration supplémentaire peut être nécessaire pour les applications hébergées derrière des serveurs proxy et des équilibreurs de charge supplémentaires. Pour plus d’informations, consultez [Configurer ASP.NET Core pour l’utilisation de serveurs proxy et d’équilibreurs de charge](xref:host-and-deploy/proxy-load-balancer).

### <a name="webconfig-file"></a>fichier web.config

Le fichier *web.config* configure le [Module ASP.NET Core](xref:host-and-deploy/aspnet-core-module). La création, la transformation et la publication du fichier *web.config* sont gérées par une cible MSBuild (`_TransformWebConfig`) quand le projet est publié. Cette cible est présente dans les cibles du SDK web (`Microsoft.NET.Sdk.Web`). Le SDK est défini en haut du fichier projet :

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

Si le projet ne contient pas de fichier *web.config*, le fichier est créé avec le *processPath* et les *arguments* corrects pour configurer le [Module ASP.NET Core](xref:host-and-deploy/aspnet-core-module), puis il est déplacé vers la [sortie publiée](xref:host-and-deploy/directory-structure).

Si un fichier *web.config* se trouve dans le projet, il est transformé avec le *processPath* et les *arguments* corrects pour configurer le Module ASP.NET Core, puis il est déplacé vers la sortie publiée. La transformation ne modifie pas les paramètres de configuration IIS dans le fichier.

Le fichier *web.config* peut fournir des paramètres de configuration IIS supplémentaires qui contrôlent les modules IIS actifs. Pour plus d’informations sur les modules IIS capables de traiter les requêtes avec des applications ASP.NET Core, consultez la rubrique [Modules IIS](xref:host-and-deploy/iis/modules).

Pour empêcher le Kit de développement logiciel (SDK) Web de transformer le fichier *web.config*, ajoutez la propriété **\<IsTransformWebConfigDisabled>** au fichier projet :

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

Lorsque vous désactivez le Kit de développement logiciel (SDK) Web en transformant le fichier, le *processPath* et les *arguments* doivent être définis manuellement par le développeur. Pour plus d’informations, consultez les [Informations de référence sur la configuration du module ASP.NET Core](xref:host-and-deploy/aspnet-core-module).

### <a name="webconfig-file-location"></a>emplacement du fichier web.config

Pour configurer le [module ASP.NET Core](xref:host-and-deploy/aspnet-core-module) correctement, le fichier *web.config* doit être présent dans le chemin de racine de contenu (généralement le chemin de base de l’application) de l’application déployée. Il s’agit du même emplacement que le chemin physique du site Web fourni à IIS. Le fichier *web.config* est nécessaire à la racine de l’application pour permettre la publication de plusieurs applications à l’aide de Web Deploy.

Il existe des fichiers sensibles sur le chemin physique de l’application, notamment *\<assembly>.runtimeconfig.json*, *\<assembly>.xml* (commentaires de documentation XML) et *\<assembly>.deps.json*. Lorsque le fichier *web.config* est présent et que le site démarre normalement, IIS ne traite pas ces fichiers sensibles s’ils sont requis. Si le fichier *web.config* est absent, nommé de manière incorrecte ou s’il est incapable de configurer le site pour un démarrage normal, IIS peut traiter des fichiers sensibles publiquement.

**Le fichier *web.config* doit être présent dans le déploiement en permanence, correctement nommé et capable de configurer le site pour un démarrage normal. Ne retirez jamais le fichier *web.config* d’un déploiement de production.**

## <a name="iis-configuration"></a>Configuration d’IIS

**Systèmes d’exploitation Windows Server**

Activez le rôle serveur **Serveur Web (IIS)** et établissez des services de rôle.

1. Utilisez l’Assistant **Ajouter des rôles et des fonctionnalités** par le biais du menu **Gérer** ou du lien dans **Gestionnaire de serveur**. À l’étape **Rôles de serveurs**, cochez la case **Serveur Web (IIS)**.

   ![Le rôle Serveur Web IIS est sélectionné à l’étape Sélectionner des rôles de serveurs.](index/_static/server-roles-ws2016.png)

1. Après l’étape **Fonctionnalités**, l’étape **Services de rôle** se charge pour le serveur Web (IIS). Sélectionnez les services de rôle IIS souhaités ou acceptez les services de rôle par défaut fournis.

   ![Les services de rôle par défaut sont sélectionnés à l’étape Sélectionner des services de rôle.](index/_static/role-services-ws2016.png)

   **Authentification Windows (facultatif)**  
   Pour activer l’authentification Windows, développez les nœuds suivants : **Serveur web** > **Sécurité**. Sélectionnez la fonctionnalité **Authentification Windows**. Pour plus d’informations, consultez [Authentification Windows\<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) et [Configurer l’authentification Windows](xref:security/authentication/windowsauth).

   **WebSockets (facultatif)**  
   WebSockets est pris en charge avec ASP.NET Core 1.1 ou version ultérieure. Pour activer WebSockets, développez les nœuds suivants : **Serveur web** > **Développement d’applications**. Sélectionnez la fonctionnalité **Protocole WebSocket**. Pour plus d’informations, consultez [WebSockets](xref:fundamentals/websockets).

1. Validez l’étape de **Confirmation** pour installer les services et le rôle de serveur web. Un redémarrage du serveur/d’IIS n’est pas nécessaire après l’installation du rôle **Serveur Web (IIS)**.

**Systèmes d’exploitation Windows Desktop**

Activez la **Console de gestion IIS** et les **Services World Wide Web**.

1. Accédez à **Panneau de configuration** > **Programmes** > **Programmes et fonctionnalités** > **Activer ou désactiver des fonctionnalités Windows** (à gauche de l’écran).

1. Ouvrez le nœud **Internet Information Services**. Ouvrez le nœud **Outils de gestion Web**.

1. Cochez la case **Console de gestion IIS**.

1. Cochez la case **Services World Wide Web**.

1. Acceptez les fonctionnalités par défaut pour **Services World Wide Web** ou personnalisez les fonctionnalités d’IIS.

   **Authentification Windows (facultatif)**  
   Pour activer l’authentification Windows, développez les nœuds suivants : **Services World Wide Web** > **Sécurité**. Sélectionnez la fonctionnalité **Authentification Windows**. Pour plus d’informations, consultez [Authentification Windows\<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) et [Configurer l’authentification Windows](xref:security/authentication/windowsauth).

   **WebSockets (facultatif)**  
   WebSockets est pris en charge avec ASP.NET Core 1.1 ou version ultérieure. Pour activer WebSockets, développez les nœuds suivants : **Services World Wide Web** > **Fonctionnalités de développement d’applications**. Sélectionnez la fonctionnalité **Protocole WebSocket**. Pour plus d’informations, consultez [WebSockets](xref:fundamentals/websockets).

1. Si l’installation d’IIS requiert un redémarrage, redémarrez le système.

![Console de gestion IIS et Services World Wide Web sont sélectionnés dans Fonctionnalités de Windows.](index/_static/windows-features-win10.png)

## <a name="install-the-net-core-hosting-bundle"></a>Installer le bundle d’hébergement .NET Core

Installez le *bundle d’hébergement .NET Core* sur le système hôte. Le bundle installe le Runtime .NET Core, la bibliothèque .NET Core et le [Module ASP.NET Core](xref:host-and-deploy/aspnet-core-module). Le module permet aux applications ASP.NET Core de s’exécuter derrière IIS. Si le système n’a pas de connexion Internet, obtenez et installez [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) avant d’installer le bundle d’hébergement .NET Core.

> [!IMPORTANT]
> Si le bundle d’hébergement est installé avant IIS, l’installation du bundle doit être réparée. Après avoir installé IIS, réexécutez le programme d’installation du bundle d’hébergement.

### <a name="direct-download-current-version"></a>Téléchargement direct (version actuelle)

Téléchargez le programme d’installation à l’aide du lien suivant :

[Programme d’installation du bundle d’hébergement .NET Core actuel (téléchargement direct)](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

### <a name="earlier-versions-of-the-installer"></a>Versions antérieures du programme d’installation

Pour obtenir une version antérieure du programme d’installation :

1. Accédez aux [archives des téléchargements .NET](https://www.microsoft.com/net/download/archives).
1. Sous **.NET Core**, sélectionnez la version de .NET Core.
1. Dans la colonne **Run apps - Runtime**, recherchez la ligne de la version du runtime .NET Core souhaitée.
1. Téléchargez le programme d’installation à l’aide du lien **Runtime & Hosting Bundle**.

> [!WARNING]
> Certains programmes d’installation contiennent des versions qui sont arrivées à leur fin de vie (EOL) et qui ne sont plus prises en charge par Microsoft. Pour plus d’informations, consultez la [politique de support](https://www.microsoft.com/net/download/dotnet-core/2.0).

### <a name="install-the-hosting-bundle"></a>Installer le bundle d’hébergement

1. Exécutez le programme d’installation sur le serveur. Les commutateurs suivants sont disponibles lorsque vous exécutez le programme d’installation à partir d’une invite de commandes administrateur :

   * `OPT_NO_ANCM=1` &ndash; Sauter l’installation du module ASP.NET Core.
   * `OPT_NO_RUNTIME=1` &ndash;Sauter l'installation du runtime .NET Core.
   * `OPT_NO_SHAREDFX=1` &ndash; Sauter l'installation de l’infrastructure partagée ASP.NET (runtime ASP.NET).
   * `OPT_NO_X86=1` &ndash; Ignorer l’installation des runtimes x86. Utilisez ce commutateur lorsque vous savez que vous n’hébergerez pas d’applications 32 bits. Si vous n’excluez pas d’avoir à héberger des applications 32 bits et 64 bits dans le futur, n'utilisez pas ce commutateur et installez les deux runtimes.
1. Redémarrez le système ou exécutez **net stop was /y** suivi de **net start w3svc** à partir d’une invite de commandes. Le redémarrage d’IIS récupère une modification apportée au CHEMIN D’ACCÈS du système, qui est une variable d’environnement, par le programme d’installation.

Si le programme d'installation du pack d'hébergement Windows détecte qu’IIS requiert une réinitialisation pour terminer l’installation, le programme d’installation réinitialise IIS. Si le programme d’installation déclenche une réinitialisation d’IIS, tous les pools d’applications et sites Web IIS sont redémarrés.

> [!NOTE]
> Pour plus d’informations sur la configuration partagée IIS, consultez [Module ASP.NET Core avec configuration partagée des services Internet (IIS)](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a>Installer Web Deploy lors de la publication avec Visual Studio

Quand vous déployez des applications sur un serveur avec [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), installez la dernière version de Web Deploy sur le serveur. Pour installer Web Deploy, utilisez [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) ou obtenez un programme d’installation directement à partir du [Centre de téléchargement Microsoft](https://www.microsoft.com/download/details.aspx?id=43717). La méthode recommandée est d’utiliser WebPI. WebPI offre une installation autonome et une configuration pour les fournisseurs d’hébergement.

## <a name="create-the-iis-site"></a>Créer le site IIS

1. Sur le système d’hébergement, créez un dossier pour contenir les fichiers et dossiers publiés de l’application. Une disposition du déploiement de l’application est décrite dans la rubrique [Structure des répertoires](xref:host-and-deploy/directory-structure).

1. Dans le nouveau dossier, créez un dossier *journaux* où stocker les journaux stdout du Module ASP.NET Core quand la journalisation stdout est activée. Si l’application est déployée avec un dossier *logs* dans la charge utile, ignorez cette étape. Pour obtenir des instructions sur l’activation de MSBuild et créer le dossier *journaux* automatiquement lorsque le projet est généré localement, consultez la rubrique [Structure des répertoires](xref:host-and-deploy/directory-structure).

   > [!IMPORTANT]
   > Utilisez uniquement le journal stdout pour résoudre les échecs de démarrage de l’application. N’utilisez jamais la journalisation stdout pour la journalisation de l’application de routine. Il n’existe aucune limite quant à la taille du fichier journal ou au nombre de fichiers journaux créés. Le pool d’applications doit avoir un accès en écriture à l’emplacement auquel les journaux sont écrits. Tous les dossiers sur le chemin de l’emplacement des journaux doivent exister. Pour plus d’informations sur le journal stdout, consultez [Création et redirection des journaux](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection). Pour plus d’informations sur la journalisation dans une application ASP.NET Core, consultez la rubrique [Journalisation](xref:fundamentals/logging/index).

1. Dans **Gestionnaire IIS**, ouvrez le nœud du serveur dans le panneau **Connexions**. Cliquez avec le bouton de droite sur le dossier **Sites**. Sélectionnez **Ajouter un site Web** dans le menu contextuel.

1. Spécifiez le **Nom du site** et définissez le **Chemin physique** sur le dossier de déploiement de l’application. Spécifiez la configuration **Liaison** et créez le site Web en sélectionnant **OK** :

   ![Spécifiez le nom du site, le chemin physique et le nom d’hôte à l’étape Ajouter un site Web.](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > Les liaisons génériques de niveau supérieur (`http://*:80/` et `http://+:80`) ne doivent **pas** être utilisées. Les liaisons génériques de niveau supérieur peuvent exposer votre application à des failles de sécurité. Cela s’applique aux caractères génériques forts et faibles. Utilisez des noms d’hôte explicites plutôt que des caractères génériques. Une liaison générique de sous-domaine (par exemple, `*.mysub.com`) ne présente pas ce risque de sécurité si vous contrôlez le domaine parent en entier (par opposition à `*.com`, qui est vulnérable). Consultez la [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) pour plus d’informations.

1. Sous le nœud du serveur, sélectionnez **Pools d’applications**.

1. Cliquez avec le bouton de droite sur le pool d’applications du site et sélectionnez **Paramètres de base** dans le menu contextuel.

1. Dans la fenêtre **Modifier le pool d’applications**, définissez la **version .NET CLR** sur **Aucun code managé** :

   ![Définissez Aucun code managé pour la version CLR .NET.](index/_static/edit-apppool-ws2016.png)

    ASP.NET Core s’exécute dans un processus séparé et gère l’exécution. ASP.NET Core ne repose pas sur le chargement du CLR de bureau. La configuration de la **version CLR .NET** sur **Aucun code managé** est facultative.

1. *ASP.NET Core 2.2 ou version ultérieure* : Pour un [déploiement autonome](/dotnet/core/deploying/#self-contained-deployments-scd) 64 bits (x64) qui utilise le [modèle d’hébergement In-process](xref:fundamentals/servers/index#in-process-hosting-model), désactivez le pool d’applications pour les processus 32 bits (x86).

   Dans la barre latérale **Actions** de **Pools d’applications** du gestionnaire IIS, sélectionnez **Définir les valeurs par défaut du pool d’applications** ou **Paramètres avancés**. Recherchez **Activer les applications 32 bits** et définissez la valeur sur `False`. Ce paramètre n’affecte pas les applications déployées pour l’[hébergement Out-of-process](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model).

1. Vérifiez que l’identité de modèle de processus dispose des autorisations appropriées.

   Si l’identité par défaut du pool d’applications (**Modèle de processus** > **Identité**) **ApplicationPoolIdentity** est remplacée par une autre identité, vérifiez que la nouvelle identité dispose des autorisations nécessaires pour accéder au dossier de l’application, à la base de données et aux autres ressources nécessaires. Par exemple, le pool d’applications nécessite l’accès en lecture et en écriture aux dossiers dans lesquels l’application lit et écrit des fichiers.

**Configuration de l’authentification Windows (facultatif)**  
Pour plus d'informations, consultez la rubrique [Configurer l’authentification Windows](xref:security/authentication/windowsauth).

## <a name="deploy-the-app"></a>Déployer l’application

Déployez l’application vers le dossier créé sur le système d’hébergement. [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) est le mécanisme recommandé pour le déploiement.

### <a name="web-deploy-with-visual-studio"></a>Web Deploy avec Visual Studio

Pour découvrir comment créer un profil de publication pour une utilisation avec Web Deploy, consultez la rubrique [Profils de publication Visual Studio pour le déploiement d’applications ASP.NET Core](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles). Si le fournisseur d’hébergement fournit un profil de publication ou prend en charge sa création, téléchargez son profil et importez-le à l’aide de la boîte de dialogue **Publier** de Visual Studio.

![Boîte de dialogue Publier](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a>Web Deploy en dehors de Visual Studio

[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) peut également être utilisé en dehors de Visual Studio à partir de la ligne de commande. Pour plus d’informations, consultez [Web Deployment Tool (Outil de déploiement Web)](/iis/publish/using-web-deploy/use-the-web-deployment-tool).

### <a name="alternatives-to-web-deploy"></a>Alternatives à Web Deploy

Utilisez la méthode de votre choix, telle que la copie manuelle, Xcopy, Robocopy ou PowerShell, pour déplacer l’application vers le système d’hébergement.

Pour plus d’informations sur le déploiement d’ASP.NET Core sur IIS, consultez la section [Déploiement de ressources pour les administrateurs IIS](#deployment-resources-for-iis-administrators).

## <a name="browse-the-website"></a>Parcourir le site web

![Le navigateur Microsoft Edge a chargé la page de démarrage IIS.](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a>Fichiers de déploiement verrouillés

Les fichiers dans le dossier de déploiement sont verrouillés quand l’application est en cours d’exécution. Les fichiers verrouillés ne peuvent pas être remplacés au cours du déploiement. Pour libérer des fichiers verrouillés dans un déploiement, arrêtez le pool d’applications à l’aide **d’une** des approches suivantes :

* Utilisez Web Deploy et référencez `Microsoft.NET.Sdk.Web` dans le fichier projet. Un fichier *app_offline.htm* est placé à la racine du répertoire de l’application web. Quand le fichier est présent, le module ASP.NET Core ferme l’application normalement et sert le fichier *app_offline.htm* pendant le déploiement. Pour plus d’informations, consultez les [Informations de référence sur la configuration du module ASP.NET Core](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).
* Arrêtez manuellement le pool d’applications dans le Gestionnaire IIS sur le serveur.
* Utilisez PowerShell pour supprimer *app_offline.html* (nécessite PowerShell 5 ou ultérieur) :

  ```PowerShell
  $pathToApp = 'PATH_TO_APP'

  # Stop the AppPool
  New-Item -Path $pathToApp app_offline.htm

  # Provide script commands here to deploy the app

  # Restart the AppPool
  Remove-Item -Path $pathToApp app_offline.htm

  ```

## <a name="data-protection"></a>Protection des données

La [pile de protection des données ASP.NET Core](xref:security/data-protection/introduction) est utilisée par plusieurs [intergiciels (middlewares)](xref:fundamentals/middleware/index) ASP.NET Core, y compris l’intergiciel utilisé dans l’authentification. Même si les API de protection des données ne sont pas appelées par le code de l’utilisateur, la protection des données doit être configurée avec un script de déploiement ou dans un code utilisateur pour créer un [magasin de clés](xref:security/data-protection/implementation/key-management) de chiffrement persistantes. Si la protection des données n’est pas configurée, les clés sont conservées en mémoire et ignorées au redémarrage de l’application.

Si le Key Ring est stocké en mémoire, au redémarrage de l’application :

* tous les jetons d’authentification basés sur des cookies sont invalidés 
* Les utilisateurs doivent se reconnecter pour envoyer leur prochaine demande. 
* toutes les données protégées par le Key Ring ne peuvent plus être déchiffrées. Ceci peut inclure des [jetons CSRF](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) et des [cookies TempData ASP.NET Core MVC](xref:fundamentals/app-state#tempdata).

Pour configurer la protection des données sous IIS afin de rendre persistante le Key Ring, adoptez **l’une** des approches suivantes :

* **Créer des clés de Registre de la protection des données**

  Les clés de la protection des données utilisées par les applications ASP.NET Core sont stockées dans le registre externe aux applications. Afin de conserver les clés pour une application donnée, créez des clés de Registre pour le pool d’applications.

  Pour les installations IIS autonomes hors d’une batterie de serveurs, le [script PowerShell Provision-AutoGenKeys.ps1 de protection des données](https://github.com/aspnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) peut être utilisé pour chaque pool d’applications utilisé avec une application ASP.NET Core. Ce script crée une clé de Registre dans le Registre HKLM, accessible uniquement pour le compte du processus Worker du pool d’applications de l’application. Les clés sont chiffrées au repos à l’aide de DPAPI avec une clé à l’échelle de la machine.

  Dans les scénarios de batterie de serveurs Web, vous pouvez configurer une application afin qu’elle utilise un chemin UNC pour stocker son Key Ring de protection des données. Par défaut, les clés de protection des données ne sont pas chiffrées. Vérifiez que les autorisations de fichiers pour le partage réseau sont limitées au compte Windows sous lequel s’exécute l’application. Un certificat X509 peut être utilisé pour protéger les clés au repos. Mettez en œuvre un mécanisme permettant aux utilisateurs de charger des certificats : Placez les certificats dans le magasin de certificats approuvés de l’utilisateur et vérifiez qu’ils sont disponibles sur toutes les machines où s’exécute l’application de l’utilisateur. Pour plus d’informations, consultez [Configurer la protection des données ASP.NET Core](xref:security/data-protection/configuration/overview).

* **Configurer le pool d’applications IIS pour charger le profil utilisateur**

  Ce paramètre se trouve dans la section **Modèle de processus** sous les **Paramètres avancés** du pool d’applications. Définissez Charger le profil utilisateur sur `True`. Cela permet de stocker les clés sous le répertoire du profil utilisateur et de les protéger à l’aide de DPAPI avec une clé propre au compte d’utilisateur utilisé par le pool d’applications.

* **Utiliser le système de fichiers comme magasin de Key Ring**

  Ajustez le code d’application pour [utiliser le système de fichiers comme magasin de porte-clés](xref:security/data-protection/configuration/overview). Utilisez un certificat X509 pour protéger le Key Ring et vérifiez qu’il s’agit d’un certificat approuvé. S’il s’agit d’un certificat auto-signé, placez-le dans le magasin Racine approuvée.

  Quand vous utilisez IIS dans une batterie de serveurs web :

  * Utilisez un partage de fichiers accessible par tous les ordinateurs.
  * Déployez un certificat X509 sur chaque ordinateur. Configurez la [protection des données dans le code](xref:security/data-protection/configuration/overview).

* **Définir une stratégie au niveau de l’ordinateur pour la protection des données**

  Le système de protection des données offre une prise en charge limitée de la définition d’une [stratégie au niveau de l’ordinateur](xref:security/data-protection/configuration/machine-wide-policy) par défaut pour toutes les applications qui utilisent les API de protection des données. Pour plus d'informations, consultez <xref:security/data-protection/introduction>.

## <a name="virtual-directories"></a>Répertoires virtuels

Les [répertoires virtuels IIS](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories) ne sont pas pris en charge avec les applications ASP.NET Core. Une application peut être hébergée en tant que [sous-application](#sub-applications).

## <a name="sub-applications"></a>Sous-applications

Une application ASP.NET Core peut être hébergée en tant que [sous-application IIS](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications). Le chemin d'accès de la sous-application devient partie intégrante de l’URL de l’application racine.

::: moniker range="< aspnetcore-2.2"

Une sous-application ne doit pas inclure le module ASP.NET Core en tant que gestionnaire. Si le module est ajouté en guise de gestionnaire dans le fichier *web.config* d’une sous-application, une *Erreur de serveur interne 500.19* faisant référence au fichier config défaillant est reçue lors de la tentative de navigation dans la sous-application.

L’exemple suivant présente un fichier *web.config* publié pour une sous-application ASP.NET Core :

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Si vous hébergez une sous-application non-ASP.NET Core sous une application ASP.NET Core, supprimez explicitement le gestionnaire hérité dans le fichier *web.config* de la sous-application :

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore" />
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

Les liens de ressources statiques au sein de la sous-application doivent utiliser la notation tilde-barre oblique (`~/`). La notation tilde-barre oblique déclenche un [Tag Helper](xref:mvc/views/tag-helpers/intro) afin d’ajouter l’élément pathbase de la sous-application au lien relatif rendu. Pour une sous-application dans `/subapp_path`, une image liée à `src="~/image.png"` est restituée sous la forme `src="/subapp_path/image.png"`. Le composant Static File Middleware de l’application racine ne traite la demande de fichiers statiques. La demande est traitée par le composant Static File Middleware de la sous-application.

Si l’attribut `src` d’une ressource statique est défini sur un chemin d’accès absolu (par exemple, `src="/image.png"`), le lien apparaît sans l’élément pathbase de la sous-application. Le composant Static File Middleware de l’application racine tente de traiter la ressource à partir de l’élément [webroot](xref:fundamentals/index#web-root-webroot) de l’application racine, ce qui entraîne une erreur *404 - Introuvable*, sauf si la ressource statique est disponible depuis l’application racine.

Pour héberger une application ASP.NET Core en tant que sous-application d’une autre application ASP.NET Core :

1. Établissez un pool d’applications pour la sous-application. Affectez la valeur **Aucun code managé** à **Version du CLR .NET**.

1. Ajoutez le site racine dans IIS Manager en plaçant la sous-application dans un dossier du site racine.

1. Cliquez avec le bouton droit sur le dossier de la sous-application dans IIS Manager, puis sélectionnez **Convertir en application**.

1. Dans la boîte de dialogue **Ajouter une application**, utilisez le bouton **Sélectionner** de l’option **Pool d’applications** pour affecter le pool d’applications que vous avez créé pour la sous-application. Sélectionnez **OK**.

L’attribution d’un pool d’applications distinct à la sous-application est obligatoire lorsque vous utilisez le modèle d’hébergement in-process.

Pour plus d’informations sur le modèle d’hébergement in-process et la configuration du module ASP.NET Core, consultez <xref:host-and-deploy/aspnet-core-module> et <xref:host-and-deploy/aspnet-core-module>.

## <a name="configuration-of-iis-with-webconfig"></a>Configuration d’IIS avec web.config

La configuration d’IIS est influencée par la section `<system.webServer>` de *web.config* pour les scénarios IIS qui sont fonctionnels pour les applications ASP.NET Core avec le module ASP.NET Core. Par exemple, la configuration IIS est fonctionnelle pour la compression dynamique. Si IIS est configuré au niveau du serveur pour utiliser la compression dynamique, l’élément `<urlCompression>` dans le fichier *web.config* de l’application peut le désactiver pour une application ASP.NET Core.

Pour plus d’informations, consultez [Informations de référence sur la configuration de \<system.webServer>](/iis/configuration/system.webServer/), [Informations de référence sur la configuration du module ASP.NET Core](xref:host-and-deploy/aspnet-core-module) et [Modules IIS avec ASP.NET Core](xref:host-and-deploy/iis/modules). Pour définir des variables d’environnement pour des applications individuelles exécutées dans des pools de données isolés, (prises en charge pour IIS 10.0 ou version ultérieure), consultez la section *AppCmd.exe command* de la rubrique [Variables d’environnement \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) dans la documentation de référence IIS.

## <a name="configuration-sections-of-webconfig"></a>Sections de configuration de web.config

Les sections de configuration des applications ASP.NET 4.x dans *web.config* ne sont pas utilisées par les applications ASP.NET Core pour la configuration :

* `<system.web>`
* `<appSettings>`
* `<connectionStrings>`
* `<location>`

Les applications ASP.NET Core sont configurées à l’aide d’autres fournisseurs de configuration. Pour plus d’informations, consultez [Configuration](xref:fundamentals/configuration/index).

## <a name="application-pools"></a>Pools d'applications

::: moniker range=">= aspnetcore-2.2"

L’isolation des pools d’applications est déterminée par le modèle d’hébergement :

* Hébergement in-process &ndash; Les applications doivent s’exécuter dans des pools d’applications distincts.
* Hébergement out-of-process &ndash; Nous vous recommandons d’isoler les applications les unes des autres en exécutant chaque application dans son propre pool d’applications.

La boîte de dialogue **Ajouter un site web** d’IIS applique un seul pool d’applications par application par défaut. Quand un **Nom du site** est fourni, le texte est automatiquement transféré vers la zone de texte **Pool d’applications**. Un nouveau pool d’applications est créé avec le nom du site une fois qu’il est ajouté.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Quand vous hébergez plusieurs sites Web sur un même serveur, nous vous recommandons d’isoler les applications les unes des autres en exécutant chaque application dans son propre pool d’applications. La boîte de dialogue **Ajouter un site Web** d’IIS applique cette configuration par défaut. Quand un **Nom du site** est fourni, le texte est automatiquement transféré vers la zone de texte **Pool d’applications**. Un nouveau pool d’applications est créé avec le nom du site une fois qu’il est ajouté.

::: moniker-end

## <a name="application-pool-identity"></a>Identité du pool d’applications

Un compte d’identité du pool d’applications permet à une application de s’exécuter sous un compte unique sans qu’il soit nécessaire de créer et de gérer des domaines ou des comptes locaux. Sur IIS 8.0 ou version ultérieure, le processus Worker d’administration IIS (WAS) crée un compte virtuel avec le nom du nouveau pool d’applications et exécute les processus Worker du pool d’applications sous ce compte par défaut. Dans la console de gestion IIS, sous **Paramètres avancés** pour le pool d’applications, vérifiez que **l’Identité** est configurée pour utiliser **ApplicationPoolIdentity** :

![Boîte de dialogue Paramètres avancés du pool applications](index/_static/apppool-identity.png)

Le processus de gestion IIS crée un identificateur sécurisé avec le nom du pool d’applications dans le système de sécurité Windows. Les ressources peuvent être sécurisées à l’aide de cette identité. Toutefois, cette identité n’est pas un compte d’utilisateur réel et n’apparaît pas dans la console de gestion d’utilisateurs Windows.

Si le processus Worker IIS a besoin d’un accès élevé à l’application, modifiez la liste de contrôle d’accès (ACL) du répertoire contenant l’application :

1. Ouvrez l’Explorateur Windows et accédez au répertoire.

1. Cliquez avec le bouton droit sur le répertoire et sélectionnez **Propriétés**.

1. Sous l’onglet **Sécurité**, sélectionnez le bouton **Modifier**, puis le bouton **Ajouter**.

1. Sélectionnez le bouton **Emplacements**, puis vérifiez que le système est sélectionné.

1. Entrez **IIS AppPool\\<app_pool_name>** dans la zone **Entrer les noms des objets à sélectionner**. Sélectionnez le bouton **Vérifier les noms**. Pour le *DefaultAppPool*, vérifiez les noms à l’aide de **IIS AppPool\DefaultAppPool**. Lorsque le bouton **Vérifier les noms** est sélectionné, une valeur de **DefaultAppPool** est indiquée dans la zone des noms d’objets. Il n’est pas possible d’entrer le nom du pool d’applications directement dans la zone des noms d’objets. Utilisez le format **IIS AppPool\\<app_pool_name>** lors de la vérification du nom de l’objet.

   ![Boîte de dialogue de sélection d’utilisateurs ou de groupes pour le dossier d’application : Ajoutez le nom du pool d’applications « DefaultAppPool » à « IIS AppPool\" dans la zone des noms d’objets avant de sélectionner « Vérifier les noms ».](index/_static/select-users-or-groups-1.png)

1. Sélectionnez **OK**.

   ![Boîte de dialogue de sélection d’utilisateurs ou de groupes pour le dossier d’application : Après avoir sélectionné « Vérifier les noms », le nom d’objet « DefaultAppPool » est indiqué dans la zone des noms d’objets.](index/_static/select-users-or-groups-2.png)

1. Les autorisations Lire &amp; exécuter doivent être accordées par défaut. Fournissez des autorisations supplémentaires si nécessaire.

L’accès peut également être octroyé par le biais d’une invite de commandes à l’aide de l’outil **ICACLS**. À l’aide de *DefaultAppPool* en exemple, la commande suivante est utilisée :

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

Pour plus d’informations, consultez la rubrique [icacls](/windows-server/administration/windows-commands/icacls).

## <a name="http2-support"></a>Prise en charge de HTTP/2

::: moniker range=">= aspnetcore-2.2"

[HTTP/2](https://httpwg.org/specs/rfc7540.html) est pris en charge avec ASP.NET Core dans les scénarios de déploiement IIS suivants :

* In-process
  * Windows Server 2016/Windows 10 ou version ultérieure ; IIS 10 ou version ultérieure
  * TLS 1.2 ou connexion ultérieure
* Out-of-process
  * Windows Server 2016/Windows 10 ou version ultérieure ; IIS 10 ou version ultérieure
  * Les connexions au serveur périphérique public utilisent HTTP/2, mais la connexion de proxy inverse pour le [serveur Kestrel](xref:fundamentals/servers/kestrel) utilise HTTP/1.1.
  * TLS 1.2 ou connexion ultérieure

Pour un déploiement in-process lorsqu’une connexion HTTP/2 est établie, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) retourne `HTTP/2`. Pour un déploiement in-process lorsqu’une connexion HTTP/2 est établie, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) retourne `HTTP/1.1`.

Pour plus d’informations sur les modèles d’hébergement in-process et out-of-process, consultez la rubrique <xref:host-and-deploy/aspnet-core-module> et <xref:host-and-deploy/aspnet-core-module>.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

[HTTP/2](https://httpwg.org/specs/rfc7540.html) est pris en charge pour les déploiements out-of-process qui répondent aux exigences de base suivantes :

* Windows Server 2016/Windows 10 ou version ultérieure ; IIS 10 ou version ultérieure
* Les connexions au serveur périphérique public utilisent HTTP/2, mais la connexion de proxy inverse pour le [serveur Kestrel](xref:fundamentals/servers/kestrel) utilise HTTP/1.1.
* Framework cible : Non applicable aux déploiements out-of-process, étant donné que la connexion HTTP/2 est gérée entièrement par IIS.
* TLS 1.2 ou connexion ultérieure

Si une connexion HTTP/2 est établie, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) retourne `HTTP/1.1`.

::: moniker-end

HTTP/2 est activé par défaut. Les connexions reviennent à HTTP/1.1 si une connexion HTTP/2 n’est pas établie. Pour plus d’informations sur la configuration de HTTP/2 avec les déploiements IIS, consultez [HTTP/2 sur IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis).

## <a name="deployment-resources-for-iis-administrators"></a>Déploiement de ressources pour les administrateurs IIS

Pour en savoir plus sur IIS, consultez la documentation IIS.  
[Documentation IIS](/iis)

Découvrez les modèles de déploiement d’application .NET Core.  
[Déploiement d’applications .NET Core](/dotnet/core/deploying/)

Découvrez comment le module ASP.NET Core permet au serveur web Kestrel d’utiliser IIS ou IIS Express en tant que serveur proxy inverse.  
[Module ASP.NET Core](xref:host-and-deploy/aspnet-core-module)

Découvrez comment configurer le module ASP.NET Core pour héberger des applications ASP.NET Core.  
[Informations de référence sur la configuration du Module ASP.NET Core](xref:host-and-deploy/aspnet-core-module)

Découvrez la structure de répertoires des applications ASP.NET Core publiées.  
[Structure de répertoires](xref:host-and-deploy/directory-structure)

Découvrez les modules IIS actifs et inactifs pour les applications ASP.NET Core et comment gérer les modules IIS.  
[Modules IIS](xref:host-and-deploy/iis/troubleshoot)

Découvrez comment diagnostiquer les problèmes liés aux déploiements IIS d’applications ASP.NET Core.  
[Résoudre les problèmes](xref:host-and-deploy/iis/troubleshoot)

Repérer les erreurs courantes liées à l’hébergement d’applications ASP.NET Core sur IIS.  
[Informations de référence sur les erreurs courantes pour Azure App Service et IIS](xref:host-and-deploy/azure-iis-errors-reference)

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:test/troubleshoot>
* [Présentation d’ASP.NET Core](xref:index)
* [Site officiel de Microsoft IIS](https://www.iis.net/)
* [Bibliothèque de contenu technique Windows Server](/windows-server/windows-server)
* [HTTP/2 sur IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis)
