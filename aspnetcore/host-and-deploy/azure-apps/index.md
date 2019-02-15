---
title: Déployer des applications ASP.NET Core sur Azure App Service
author: guardrex
description: Cet article contient des liens vers des ressources d’hébergement et de déploiement Azure.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2018
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: 4c7f2348b13207fb6f2ff830bbf67ffa30e5489c
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248236"
---
# <a name="deploy-aspnet-core-apps-to-azure-app-service"></a>Déployer des applications ASP.NET Core sur Azure App Service

[Azure App Service](https://azure.microsoft.com/services/app-service/) est un [service de plateforme de cloud computing Microsoft](https://azure.microsoft.com/) qui permet d’héberger des applications web, notamment ASP.NET Core.

## <a name="useful-resources"></a>Ressources utiles

La documentation d’[Azure Web Apps](/azure/app-service/) contient la documentation, les didacticiels, les exemples, les guides pratiques et d’autres ressources sur les applications Azure. Voici deux didacticiels importants qui abordent l’hébergement d’applications ASP.NET Core :

[Démarrage rapide : Créer une application web ASP.NET Core dans Azure](/azure/app-service/app-service-web-get-started-dotnet)  
Utilisez Visual Studio pour créer et déployer une application web ASP.NET Core dans Azure App Service sur Windows.

[Démarrage rapide : Créer une application web .NET Core dans App Service sur Linux](/azure/app-service/containers/quickstart-dotnetcore)  
Utilisez la ligne de commande pour créer et déployer une application web ASP.NET Core dans Azure App Service sur Linux.

Les articles suivants sont disponibles dans la documentation d’ASP.NET Core :

<xref:tutorials/publish-to-azure-webapp-using-vs>  
Découvrez comment publier une application ASP.NET Core sur Azure App Service à l’aide de Visual Studio.

<xref:host-and-deploy/azure-apps/azure-continuous-deployment>  
Découvrez comment créer une application web ASP.NET Core à l’aide de Visual Studio et comment la déployer sur Azure App Service en utilisant Git pour le déploiement continu.

[Créer son premier pipeline avec Azure Pipelines](/azure/devops/pipelines/get-started-yaml)  
Configurez une build CI pour une application ASP.NET Core, puis créez une version de déploiement continu sur Azure App Service.

[Bac à sable Azure Web App](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
Découvrez les limitations d’exécution du runtime Azure App Service appliquées par la plateforme Azure Apps.

## <a name="application-configuration"></a>Configuration d’application

### <a name="platform"></a>Plateforme

::: moniker range=">= aspnetcore-2.2"

Des runtimes pour les applications 64 bits (x64) et 32 bits (x 86) sont présents sur Azure App Service. Le [SDK .NET Core](/dotnet/core/sdk) disponible sur App Service est 32 bits, mais vous pouvez déployer des applications 64 bits à l’aide de la console [Kudu](https://github.com/projectkudu/kudu/wiki) ou via [MSDeploy avec un profil de publication Visual Studio ou une commande CLI](xref:host-and-deploy/visual-studio-publish-profiles).

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Concernant les applications avec des dépendances natives, des runtimes pour les applications 32 bits (x86) sont présents sur Azure App Service. Le [SDK .NET Core](/dotnet/core/sdk) disponible sur App Service est 32 bits.

::: moniker-end

### <a name="packages"></a>Packages

Ajoutez les packages NuGet suivants pour fournir des fonctionnalités de journalisation automatique aux applications déployées sur Azure App Service :

* [Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) utilise [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) pour intégrer la fonction d’éclairage ASP.NET Core dans Azure App Service. Les fonctionnalités de journalisation ajoutées sont fournies par le package `Microsoft.AspNetCore.AzureAppServicesIntegration`.
* [Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) exécute [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) pour ajouter des fournisseurs de journalisation de diagnostics Azure App Service dans le package `Microsoft.Extensions.Logging.AzureAppServices`.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) fournit des implémentations de journaliseur pour prendre en charge les journaux de diagnostics et les fonctionnalités de streaming de journal Azure App Service.

Les packages précédents ne sont pas disponibles dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). Les applications qui ciblent le .NET Framework ou référencent le métapackage `Microsoft.AspNetCore.App` doivent référencer explicitement les packages individuels dans le fichier projet de l’application.

## <a name="override-app-configuration-using-the-azure-portal"></a>Remplacer la configuration de l’application à l’aide du Portail Azure

Les paramètres d’application dans le portail Azure vous permettent de définir des variables d’environnement pour l’application. Celles-ci peuvent être utilisées par le [Fournisseur de configuration des variables d’environnement](xref:fundamentals/configuration/index#environment-variables-configuration-provider).

Quand un paramètre d’application est créé ou modifié dans le portail Azure et que le bouton **Enregistrer** est sélectionné, Azure App redémarre. La variable d’environnement est à la disposition de l’application après le redémarrage du service.

Quand une application utilise [l’Hôte web](xref:fundamentals/host/web-host) et le génère avec [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), les variables d’environnement qui le configurent utilisent le préfixe `ASPNETCORE_`. Pour plus d’informations, voir <xref:fundamentals/host/web-host> et [Fournisseur de configuration des variables d’environnement](xref:fundamentals/configuration/index#environment-variables-configuration-provider).

Quand une application utilise [l’Hôte générique](xref:fundamentals/host/generic-host), les variables d’environnement ne sont par défaut pas chargées dans une configuration de l’application ; le fournisseur de configuration doit être ajouté par le développeur. Ce dernier détermine le préfixe de variable d’environnement lors de l’ajout du fournisseur de configuration. Pour plus d’informations, voir <xref:fundamentals/host/generic-host> et [Fournisseur de configuration des variables d’environnement](xref:fundamentals/configuration/index#environment-variables-configuration-provider).

## <a name="proxy-server-and-load-balancer-scenarios"></a>Scénarios avec un serveur proxy et un équilibreur de charge

L’intergiciel (middleware) d’intégration IIS, qui configure l’intergiciel des en-têtes transférés, et le module ASP.NET Core, sont configurés pour transférer le schéma (HTTP/HTTPS) et l’adresse IP distante d’où provient la requête. Une configuration supplémentaire peut être nécessaire pour les applications hébergées derrière des serveurs proxy et des équilibreurs de charge supplémentaires. Pour plus d’informations, consultez [Configurer ASP.NET Core pour l’utilisation de serveurs proxy et d’équilibreurs de charge](xref:host-and-deploy/proxy-load-balancer).

## <a name="monitoring-and-logging"></a>Surveillance et journalisation

Les applications ASP.NET Core déployées sur App Service reçoivent automatiquement une extension App Service, **Extensions de journalisation ASP.NET Core**. L’extension active la journalisation Azure.

Pour des informations de surveillance, de journalisation et de dépannage, consultez les articles suivants :

[Guide pratique pour surveiller des applications dans Azure App Service](/azure/app-service/web-sites-monitor)  
Découvrez comment consulter les quotas et les métriques des applications et des plans App Service.

[Activer la journalisation des diagnostics pour les applications web dans Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log)  
Découvrez comment activer et accéder à la journalisation des diagnostics pour les codes d’état HTTP, les requêtes en échec et les activités de serveur web.

<xref:fundamentals/error-handling>  
Découvrez les approches courantes permettant de gérer les erreurs dans les applications ASP.NET Core.

<xref:host-and-deploy/azure-apps/troubleshoot>  
Découvrez comment diagnostiquer les problèmes de déploiements Azure App Service avec les applications ASP.NET Core.

<xref:host-and-deploy/azure-iis-errors-reference>  
Découvrez les erreurs de configuration de déploiement courantes dans les applications hébergées par Azure App Service/IIS, ainsi que des conseils de dépannage.

## <a name="data-protection-key-ring-and-deployment-slots"></a>Porte-clés de Protection des données et emplacements de déploiement

Les [clés de Protection des données](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) sont persistantes dans le dossier *%HOME%\ASP.NET\DataProtection-Keys*. Ce dossier est alimenté par le stockage réseau et synchronisé sur tous les ordinateurs hébergeant l’application. Les clés ne sont pas protégées au repos. Ce dossier fournit le porte-clés à toutes les instances d’une application dans un seul emplacement de déploiement. Les emplacements de déploiement distincts, tels que Préproduction et Production, ne partagent pas de porte-clés.

Lors d’une permutation entre les emplacements de déploiement, aucun système utilisant la protection des données ne peut déchiffrer les données stockées à l’aide du porte-clés au sein de l’emplacement précédent. L’intergiciel (middleware) ASP.NET Cookie utilise la protection des données pour protéger ses cookies. Cela entraîne la déconnexion des utilisateurs des applications qui utilisent l’intergiciel ASP.NET Cookie standard. Pour une solution de porte-clés indépendante de l’emplacement, utilisez un fournisseur de porte-clés externe, tel que :

* Stockage Blob Azure
* Azure Key Vault
* Magasin SQL
* Cache Redis

Pour plus d'informations, consultez <xref:security/data-protection/implementation/key-storage-providers>.

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a>Déployer la version préliminaire d’ASP.NET Core sur Azure App Service

Utilisez l'une des approches suivantes :

* [Installer l’extension de site de préversion](#install-the-preview-site-extension).
* [Déployer l’application autonome](#deploy-the-app-self-contained).
* [Utiliser Docker avec Web Apps pour conteneurs](#use-docker-with-web-apps-for-containers).

### <a name="install-the-preview-site-extension"></a>Installer l’extension de site de version Preview

Si un problème se produit quand vous utilisez l’extension de site de version Preview, ouvrez un problème sur [GitHub](https://github.com/aspnet/azureintegration/issues/new).

1. Dans le portail Azure, accédez à App Service.
1. Sélectionnez l’application web.
1. Tapez « ex » dans la zone de recherche pour filtrer sur les « Extensions » ou faites défiler la liste outils de gestion.
1. Sélectionner **Extensions**.
1. Sélectionnez **Ajouter**.
1. Sélectionnez l’extension **ASP.NET Core {X.Y} ({x64|x86}) Runtime** dans la liste, où `{X.Y}` correspond à la préversion d’ASP.NET Core et `{x64|x86}` spécifie la plateforme.
1. Sélectionnez **OK** pour accepter les conditions légales.
1. Sélectionnez **OK** pour installer l’extension.

Une fois l’opération effectuée, la dernière préversion de .NET Core est installée. Vérifiez l’installation :

1. Sélectionnez **Outils avancés**.
1. Sélectionnez **Go** dans **Outils avancés**.
1. Sélectionnez l’élément de menu **Console de débogage** > **PowerShell**.
1. À l’invite PowerShell, exécutez la commande suivante. Remplacez `{X.Y}` par la version du runtime ASP.NET Core et `{PLATFORM}` par la plateforme dans la commande :

   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.{PLATFORM}\
   ```
   La commande retourne `True` quand le runtime de la préversion x64 est installée.

> [!NOTE]
> L’architecture de plateforme (x86/x64) d’une application App Services est définie dans les paramètres de l’application dans le portail Azure pour les applications hébergées sur une instance de calcul de série A ou supérieure. Si l’application s’exécute en mode in-process et si la plateforme est configurée pour une architecture 64 bits (x64), le module ASP.NET Core utilise le runtime de la préversion 64 bits, le cas échéant. Installez l’extension **ASP.NET Core {X.Y} (x64) Runtime**.
>
> Après avoir installé le runtime de la préversion x64, exécutez la commande suivante dans la fenêtre Commande Kudu PowerShell pour vérifier l’installation. Remplacez `{X.Y}` par la version du runtime ASP.NET Core dans la commande :
>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64\
> ```
> La commande retourne `True` quand le runtime de la préversion x64 est installée.

> [!NOTE]
> Les **extensions ASP.NET Core** permettent d’activer des fonctionnalités supplémentaires pour ASP.NET Core sur Azure App Services, par exemple la journalisation Azure. L’extension est installée automatiquement quand vous effectuez le déploiement à partir de Visual Studio. Si l’extension n’est pas installée, installez-la pour l’application.

**Utiliser l’extension de site de la version Preview avec un modèle ARM**

Si un modèle ARM est utilisé pour créer et déployer des applications, le type de ressource `siteextensions` peut être utilisé pour ajouter l’extension de site à une application web. Par exemple :

[!code-json[](index/sample/arm.json?highlight=2)]

### <a name="deploy-the-app-self-contained"></a>Déployer l’application autonome

Un [déploiement autonome (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) qui cible une préversion runtime exécute le runtime de la préversion dans le déploiement.

Pendant le déploiement d’une application autonome :

* Le site dans Azure App Service ne requiert pas l’[extension du site de préversion](#install-the-preview-site-extension).
* L’application doit être publiée suivant une autre approche que dans le cadre de la publication d’un [déploiement en fonction de l’infrastructure (FDD)](/dotnet/core/deploying#framework-dependent-deployments-fdd).

#### <a name="publish-from-visual-studio"></a>Publier à partir de Visual Studio

1. Sélectionnez **Build** > **Publier {nom de l’application}** à partir de la barre d’outils de Visual Studio.
1. Dans la boîte de dialogue **Choisir une cible de publication**, confirmez qu’**App Service** est sélectionné.
1. Sélectionnez **Avancé**. La boîte de dialogue **Publier** s’ouvre.
1. Dans la boîte de dialogue **Publier** :
   * Confirmez que la configuration **Mise en production** est sélectionnée.
   * Ouvrez la liste déroulante **Mode de déploiement** et sélectionnez **Autonome**.
   * Sélectionnez le runtime cible à partir de la liste déroulante **Runtime cible**. La valeur par défaut est `win-x86`.
   * Si vous devez supprimer des fichiers supplémentaires lors du déploiement, ouvrez **Options de publication de fichiers** et sélectionnez la case à cocher pour supprimer des fichiers supplémentaires à la destination.
   * Sélectionnez **Enregistrer**.
1. Créez un nouveau site ou mettez à jour un site existant en suivant les autres invites de l'Assistant de publication.

#### <a name="publish-using-command-line-interface-cli-tools"></a>Publiez à l'aide d'outils d'interface de ligne de commande (CLI)

1. Dans le fichier projet, spécifiez un ou plusieurs [identificateurs de runtime (RID)](/dotnet/core/rid-catalog). Utilisez `<RuntimeIdentifier>` (singulier) pour un seul RID, ou `<RuntimeIdentifiers>` (pluriel) pour fournir une liste de RID délimitée par des points-virgules. Dans l’exemple suivant, le RID `win-x86` est spécifié :

   ```xml
   <PropertyGroup>
     <TargetFramework>netcoreapp2.1</TargetFramework>
     <RuntimeIdentifier>win-x86</RuntimeIdentifier>
   </PropertyGroup>
   ```
1. A partir d'un interpréteur de commandes, publiez l'application dans la configuration Mise en production pour le runtime de l'hôte avec la commande [dotnet publish](/dotnet/core/tools/dotnet-publish). Dans l’exemple suivant, l’application est publiée pour le RID `win-x86`. Le RID fourni à l’option `--runtime` doit être fourni dans la propriété `<RuntimeIdentifier>` (ou `<RuntimeIdentifiers>`) du fichier projet.

   ```console
   dotnet publish --configuration Release --runtime win-x86
   ```
1. Déplacez le contenu du répertoire *bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* vers le site dans App Service.

### <a name="use-docker-with-web-apps-for-containers"></a>Utiliser Docker avec Web Apps pour conteneurs

[Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contient les dernières images de Docker en préversion. Les images peuvent être utilisées comme images de base. Utilisez l’image pour effectuer un déploiement sur Web Apps pour conteneurs normalement.

## <a name="protocol-settings-https"></a>Paramètres de protocole (HTTPS)

Les liaisons de protocole sécurisées permettent de spécifier un certificat à utiliser pour répondre à des requêtes sur HTTPS. La liaison nécessite un certificat privé valide (*.pfx*) émis pour le nom d’hôte spécifique. Pour plus d'informations, consultez le [Tutoriel : Lier un certificat SSL personnalisé existant à Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl).

## <a name="transform-webconfig"></a>Transformer web.config

Si vous devez transformer *web.config* lors de la publication (par exemple, définir les variables d’environnement basées sur la configuration, le profil ou l’environnement), consultez <xref:host-and-deploy/iis/transform-webconfig>.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Vue d’ensemble de Web Apps (vidéo de 5 minutes)](/azure/app-service/app-service-web-overview)
* [Azure App Service : The Best Place to Host your .NET Apps (vidéo de présentation de 55 minutes)](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [Azure Friday : Azure App Service Diagnostic and Troubleshooting Experience (vidéo de 12 minutes)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [Vue d’ensemble des diagnostics Azure App Service](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

Azure App Service sur Windows Server utilise [IIS (Internet Information Services)](https://www.iis.net/). Les rubriques suivantes se rapportent à la technologie IIS sous-jacente :

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [Bibliothèque Microsoft TechNet : Windows Server](/windows-server/windows-server-versions)
