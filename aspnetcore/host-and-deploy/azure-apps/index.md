---
title: Héberger ASP.NET Core sur Azure App Service
author: guardrex
description: Découvrez comment héberger des applications ASP.NET Core dans Azure App Service avec des liens vers des ressources utiles.
ms.author: riande
ms.custom: mvc
ms.date: 07/24/2018
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: 9a7d20378cac597b748d8a60eb0f0bf17c9ba082
ms.sourcegitcommit: d27317c16f113e7c111583042ec7e4c5a26adf6f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/20/2018
ms.locfileid: "41746417"
---
# <a name="host-aspnet-core-on-azure-app-service"></a>Héberger ASP.NET Core sur Azure App Service

[Azure App Service](https://azure.microsoft.com/services/app-service/) est un [service de plateforme de cloud computing Microsoft](https://azure.microsoft.com/) qui permet d’héberger des applications web, notamment ASP.NET Core.

## <a name="useful-resources"></a>Ressources utiles

La documentation d’[Azure Web Apps](/azure/app-service/) contient la documentation, les didacticiels, les exemples, les guides pratiques et d’autres ressources sur les applications Azure. Voici deux didacticiels importants qui abordent l’hébergement d’applications ASP.NET Core :

[Démarrage rapide : créer une application web ASP.NET Core dans Azure](/azure/app-service/app-service-web-get-started-dotnet)  
Utilisez Visual Studio pour créer et déployer une application web ASP.NET Core dans Azure App Service sur Windows.

[Démarrage rapide : créer une application web .NET Core dans App Service sur Linux](/azure/app-service/containers/quickstart-dotnetcore)  
Utilisez la ligne de commande pour créer et déployer une application web ASP.NET Core dans Azure App Service sur Linux.

Les articles suivants sont disponibles dans la documentation d’ASP.NET Core :

[Publier sur Azure avec Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs)  
Découvrez comment publier une application ASP.NET Core sur Azure App Service à l’aide de Visual Studio.

[Publier sur Azure avec les outils CLI](xref:tutorials/publish-to-azure-webapp-using-cli)  
Découvrez comment publier une application ASP.NET Core sur Azure App Service à l’aide du client de ligne de commande Git.

[Déploiement continu sur Azure avec Visual Studio et Git](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
Découvrez comment créer une application web ASP.NET Core à l’aide de Visual Studio et comment la déployer sur Azure App Service en utilisant Git pour le déploiement continu.

[Déploiement continu sur Azure avec VSTS](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
Configurez une build CI pour une application ASP.NET Core, puis créez une version de déploiement continu sur Azure App Service.

[Bac à sable Azure Web App](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
Découvrez les limitations d’exécution du runtime Azure App Service appliquées par la plateforme Azure Apps.

::: moniker range=">= aspnetcore-2.0"

## <a name="application-configuration"></a>Configuration d’application

Dans ASP.NET Core 2.0 ou version ultérieure, les packages NuGet suivants fournissent des fonctionnalités de journalisation automatique pour les applications déployées sur Azure App Service :

* [Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) utilise [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) pour intégrer la fonction d’éclairage ASP.NET Core dans Azure App Service. Les fonctionnalités de journalisation ajoutées sont fournies par le package `Microsoft.AspNetCore.AzureAppServicesIntegration`.
* [Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) exécute [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) pour ajouter des fournisseurs de journalisation de diagnostics Azure App Service dans le package `Microsoft.Extensions.Logging.AzureAppServices`.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) fournit des implémentations de journaliseur pour prendre en charge les journaux de diagnostics et les fonctionnalités de streaming de journal Azure App Service.

Si vous ciblez .NET Core et référencez le [métapackage Microsoft.AspNetCore.All](xref:fundamentals/metapackage), les packages sont toujours inclus. Les packages sont absents du [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) plus récent. Si vous ciblez le .NET Framework ou référencez le métapackage `Microsoft.AspNetCore.App`, référencez individuellement les packages de journalisation.

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a>Scénarios avec un serveur proxy et un équilibreur de charge

L’intergiciel (middleware) d’intégration IIS, qui configure l’intergiciel des en-têtes transférés, et le module ASP.NET Core, sont configurés pour transférer le schéma (HTTP/HTTPS) et l’adresse IP distante d’où provient la requête. Une configuration supplémentaire peut être nécessaire pour les applications hébergées derrière des serveurs proxy et des équilibreurs de charge supplémentaires. Pour plus d’informations, consultez [Configurer ASP.NET Core pour l’utilisation de serveurs proxy et d’équilibreurs de charge](xref:host-and-deploy/proxy-load-balancer).

## <a name="monitoring-and-logging"></a>Surveillance et journalisation

Pour des informations de surveillance, de journalisation et de dépannage, consultez les articles suivants :

[Guide pratique pour surveiller des applications dans Azure App Service](/azure/app-service/web-sites-monitor)  
Découvrez comment consulter les quotas et les métriques des applications et des plans App Service.

[Activer la journalisation des diagnostics pour les applications web dans Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log)  
Découvrez comment activer et accéder à la journalisation des diagnostics pour les codes d’état HTTP, les requêtes en échec et les activités de serveur web.

[Présentation de la gestion des erreurs dans ASP.NET Core](xref:fundamentals/error-handling)  
Découvrez les approches courantes permettant de gérer les erreurs dans les applications ASP.NET Core.

[Résoudre les problèmes liés à ASP.NET Core sur Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot)  
Découvrez comment diagnostiquer les problèmes de déploiements Azure App Service avec les applications ASP.NET Core.

[Informations de référence sur les erreurs courantes pour Azure App Service et IIS avec ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)  
Découvrez les erreurs de configuration de déploiement courantes dans les applications hébergées par Azure App Service/IIS, ainsi que des conseils de dépannage.

## <a name="data-protection-key-ring-and-deployment-slots"></a>Porte-clés de Protection des données et emplacements de déploiement

Les [clés de Protection des données](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) sont persistantes dans le dossier *%HOME%\ASP.NET\DataProtection-Keys*. Ce dossier est alimenté par le stockage réseau et synchronisé sur tous les ordinateurs hébergeant l’application. Les clés ne sont pas protégées au repos. Ce dossier fournit le porte-clés à toutes les instances d’une application dans un seul emplacement de déploiement. Les emplacements de déploiement distincts, tels que Préproduction et Production, ne partagent pas de porte-clés.

Lors d’une permutation entre les emplacements de déploiement, aucun système utilisant la protection des données ne peut déchiffrer les données stockées à l’aide du porte-clés au sein de l’emplacement précédent. L’intergiciel (middleware) ASP.NET Cookie utilise la protection des données pour protéger ses cookies. Cela entraîne la déconnexion des utilisateurs des applications qui utilisent l’intergiciel ASP.NET Cookie standard. Pour une solution de porte-clés indépendante de l’emplacement, utilisez un fournisseur de porte-clés externe, tel que :

* Stockage Blob Azure
* Azure Key Vault
* Magasin SQL
* Cache Redis

Pour plus d’informations, consultez [Fournisseurs de stockage de clés](xref:security/data-protection/implementation/key-storage-providers).

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a>Déployer la version préliminaire d’ASP.NET Core sur Azure App Service

Les applications basées sur la version préliminaire d’ASP.NET Core peuvent être déployées sur Azure App Service avec les approches suivantes :

* [Installer l’extension de site de version Preview](#install-the-preview-site-extension)
<!-- * [Deploy the app self-contained](#deploy-the-app-self-contained) -->
* [Utiliser Docker avec Web Apps pour conteneurs](#use-docker-with-web-apps-for-containers)

Si un problème se produit quand vous utilisez l’extension de site de version Preview, ouvrez un problème sur [GitHub](https://github.com/aspnet/azureintegration/issues/new).

### <a name="install-the-preview-site-extension"></a>Installer l’extension de site de version Preview

1. Dans le portail Azure, accédez au panneau App Service.
1. Sélectionnez l’application web.
1. Entrez « ex » dans la zone de recherche ou faites défiler la liste des volets de gestion jusqu’à **OUTILS DE DEVELOPPEMENT**.
1. Sélectionnez **OUTILS DE DEVELOPPEMENT** > **Extensions**.
1. Sélectionnez **Ajouter**.

   ![Panneau de l’application Azure avec les étapes précédentes](index/_static/x1.png)

1. Sélectionnez **Extensions ASP.NET Core**.
1. Sélectionnez **OK** pour accepter les conditions légales.
1. Sélectionnez **OK** pour installer l’extension.

Quand les opérations d’ajout sont terminées, la dernière préversion de .NET Core est installée. Vérifiez l’installation en exécutant `dotnet --info` dans la console. Dans le panneau **App Service** :

1. Entrez « con » dans la zone de recherche ou faites défiler la liste des volets de gestion jusqu’à **OUTILS DE DEVELOPPEMENT**.
1. Sélectionnez **OUTILS DE DEVELOPPEMENT** > **Console**.
1. Entrez `dotnet --info` dans la console.

Si la version `2.1.300-preview1-008174` est la dernière préversion, vous obtenez le résultat suivant en exécutant `dotnet --info` depuis l’invite de commandes :

![Panneau de l’application Azure avec les étapes précédentes](index/_static/cons.png)

La version d’ASP.NET Core indiquée dans l’image précédente, `2.1.300-preview1-008174`, est un exemple. La dernière préversion d’ASP.NET Core au moment de la configuration de l’extension de site s’affiche quand vous exécutez `dotnet --info`.

La commande `dotnet --info` affiche le chemin de l’extension de site où la préversion a été installée. Il indique que l’application s’exécute depuis l’extension de site au lieu de l’emplacement de *ProgramFiles* par défaut. Si vous voyez *ProgramFiles*, redémarrez le site et exécutez `dotnet --info`.

**Utiliser l’extension de site de la version Preview avec un modèle ARM**

Si un modèle ARM est utilisé pour créer et déployer des applications, le type de ressource `siteextensions` peut être utilisé pour ajouter l’extension de site à une application web. Exemple :

[!code-json[Main](index/sample/arm.json?highlight=2)]

<!--
### Deploy the app self-contained

A [self-contained app](/dotnet/core/deploying/#self-contained-deployments-scd) can be deployed that carries the preview runtime in the deployment. When deploying a self-contained app:

* The site doesn't need to be prepared.
* The app must be published differently than when publishing for a framework-dependent deployment with the shared runtime and host on the server.

Self-contained apps are an option for all ASP.NET Core apps.
-->

### <a name="use-docker-with-web-apps-for-containers"></a>Utiliser Docker avec Web Apps pour conteneurs

[Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contient les dernières images de Docker en préversion. Les images peuvent être utilisées comme images de base. Utilisez l’image pour effectuer un déploiement sur Web Apps pour conteneurs normalement.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Vue d’ensemble de Web Apps (vidéo de 5 minutes)](/azure/app-service/app-service-web-overview)
* [Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [Vue d’ensemble des diagnostics Azure App Service](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

Azure App Service sur Windows Server utilise [IIS (Internet Information Services)](https://www.iis.net/). Les rubriques suivantes se rapportent à la technologie IIS sous-jacente :

* <xref:host-and-deploy/iis/index>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [Bibliothèque Microsoft TechNet : Windows Server](/windows-server/windows-server-versions)
