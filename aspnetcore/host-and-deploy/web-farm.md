---
title: Héberger ASP.NET Core dans une batterie de serveurs web
author: rick-anderson
description: Découvrez comment héberger plusieurs instances d’une application ASP.NET Core avec des ressources partagées dans un environnement de batterie de serveurs web.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/13/2020
uid: host-and-deploy/web-farm
ms.openlocfilehash: 316c87e5f49593c05991a94cbe5e55d175a49bb3
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659368"
---
# <a name="host-aspnet-core-in-a-web-farm"></a>Héberger ASP.NET Core dans une batterie de serveurs web

Par [Chris Ross](https://github.com/Tratcher)

Une *batterie de serveurs Web* est un groupe d’au moins deux serveurs web (ou *nœuds*) qui héberge plusieurs instances d’une application. Lorsque les requêtes des utilisateurs arrivent à une batterie de serveurs web, un *équilibreur de charge* les répartit sur les nœuds de la batterie de serveurs web. Les batteries de serveurs web apportent les améliorations suivantes :

* **Fiabilité/disponibilité** &ndash; en cas d’échec d’un ou de plusieurs nœuds, l’équilibreur de charge peut acheminer les demandes vers d’autres nœuds opérationnels pour poursuivre le traitement des demandes.
* **Capacité/performance** &ndash; plusieurs nœuds peuvent traiter plus de demandes qu’un seul serveur. L’équilibreur de charge équilibre la charge de travail en répartissant les requêtes sur les nœuds.
* L' **extensibilité** &ndash; lorsque plus ou moins de capacité est nécessaire, le nombre de nœuds actifs peut être augmenté ou réduit pour correspondre à la charge de travail. Les technologies de plateformes de batterie de serveurs web, telles qu’[Azure App Service](https://azure.microsoft.com/services/app-service/), peuvent automatiquement ajouter ou supprimer des nœuds, à la demande de l’administrateur système ou automatiquement, sans intervention humaine.
* La **maintenabilité** &ndash; nœuds d’une batterie de serveurs Web peut reposer sur un ensemble de services partagés, ce qui permet une gestion plus facile du système. Par exemple, les nœuds d’une batterie de serveurs web peuvent reposer sur un serveur de base de données unique et un emplacement réseau commun pour les ressources statiques, telles que les images et les fichiers téléchargeables.

Cette rubrique décrit la configuration et les dépendances des applications ASP.NET Core hébergées dans une batterie de serveurs web qui s’appuie sur des ressources partagées.

## <a name="general-configuration"></a>Configuration générale

<xref:host-and-deploy/index>  
Découvrez comment configurer des environnements d’hébergement et déployer des applications ASP.NET Core. Configurer un gestionnaire de processus sur chaque nœud de la batterie de serveurs web pour automatiser le démarrage le redémarrage des applications. Chaque nœud requiert le runtime ASP.NET Core. Pour plus d’informations, consultez les rubriques de la zone de documentation [Héberger et déployer](xref:host-and-deploy/index).

<xref:host-and-deploy/proxy-load-balancer>  
Découvrez la configuration des applications hébergées derrière des serveurs proxy et des équilibreurs de charge, qui masquent souvent des informations de requête importantes.

<xref:host-and-deploy/azure-apps/index>  
[Azure App Service](https://azure.microsoft.com/services/app-service/) est un [service de plateforme de cloud computing Microsoft](https://azure.microsoft.com/) qui permet d’héberger des applications web, notamment ASP.NET Core. App Service est une plateforme entièrement gérée qui assure la mise à l’échelle automatique, l’équilibrage de charge, la mise à jour corrective et le déploiement continu.

## <a name="app-data"></a>Données d’application

Quand une application est à mise l’échelle pour plusieurs instances, l’état de l’application peut nécessiter le partage entre les nœuds. Si l’état est transitoire, envisagez le partage d’un [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache). Si l’état partagé nécessite une persistance, envisagez de stocker l’état partagé dans une base de données.

## <a name="required-configuration"></a>Configuration nécessaire

La protection des données et la mise en cache nécessitent une configuration des applications déployées sur une batterie de serveurs web.

### <a name="data-protection"></a>Protection des données

Le [système de Protection des données ASP.NET Core](xref:security/data-protection/introduction) est utilisé par les applications pour protéger les données. La protection des données repose sur un ensemble de clés de chiffrement stockées dans un fichier *Key Ring*. Lorsque le système de Protection des données est initialisé, il applique des [paramètres par défaut](xref:security/data-protection/configuration/default-settings) qui stockent le fichier Key Ring localement. Dans la configuration par défaut, un Key Ring unique est stocké sur chaque nœud de la batterie de serveurs web. Par conséquent, chaque nœud de la batterie de serveurs web ne peut pas déchiffrer des données chiffrées par une application sur un autre nœud. La configuration par défaut ne convient généralement pas à l’hébergement des applications dans une batterie de serveurs web. Une alternative à l’implémentation d’un fichier Key Ring partagé consiste à toujours acheminer les requêtes de l’utilisateur vers le même nœud. Pour plus d’informations sur la configuration du système de protection des données pour les déploiements sur des batteries de serveurs web, consultez <xref:security/data-protection/configuration/overview>.

### <a name="caching"></a>Mise en cache

Dans un environnement de batterie de serveurs web, le mécanisme de mise en cache doit partager des éléments mis en cache sur les nœuds de la batterie de serveurs web. La mise en cache doit reposer sur un cache Redis commun, sur une base de données SQL Server partagée, ou sur une implémentation de mise en cache personnalisée qui partage les éléments mis en cache sur la batterie de serveurs web. Pour plus d’informations, consultez <xref:performance/caching/distributed>.

## <a name="dependent-components"></a>Composants dépendants

Les scénarios suivants ne nécessitent pas une configuration supplémentaire, mais dépendent de technologies qui nécessitent une configuration pour les batteries de serveurs web.

| Scénario | Dépend de &hellip; |
| -------- | ------------------- |
| Authentication | Protection des données (voir <xref:security/data-protection/configuration/overview>).<br><br>Pour plus d’informations, consultez <xref:security/authentication/cookie> et <xref:security/cookie-sharing>. |
| Identité | Authentification et configuration de la base de données.<br><br>Pour plus d’informations, consultez <xref:security/authentication/identity>. |
| session | Protection des données (cookies chiffrés) (voir <xref:security/data-protection/configuration/overview>) et Mise en cache (voir <xref:performance/caching/distributed>).<br><br>Pour plus d’informations, consultez [session and State Management : état de session](xref:fundamentals/app-state#session-state). |
| TempData | Protection des données (cookies chiffrés) (voir <xref:security/data-protection/configuration/overview>) ou session (voir la rubrique [gestion de session et d’État : état de session](xref:fundamentals/app-state#session-state)).<br><br>Pour plus d’informations, consultez [session and State Management : TempData](xref:fundamentals/app-state#tempdata). |
| Anti-contrefaçon | Protection des données (voir <xref:security/data-protection/configuration/overview>).<br><br>Pour plus d’informations, consultez <xref:security/anti-request-forgery>. |

## <a name="troubleshoot"></a>Dépanner

### <a name="data-protection-and-caching"></a>Protection et mise en cache des données

Lorsque la protection des données ou la mise en cache ne sont pas configurées pour un environnement de batterie de serveurs web, des erreurs intermittentes se produisent pendant le traitement des requêtes. Ceci est dû au fait que les nœuds ne partagent pas les mêmes ressources et que les requêtes des utilisateurs ne sont pas toujours redirigées vers le même nœud.

Prenons l’exemple d’un utilisateur qui se connecte à l’application à l’aide de l’authentification basée sur les cookies. L’utilisateur se connecte à l’application sur un nœud de batterie de serveurs web. Si sa prochaine requête arrive sur ce même nœud où il est connecté, l’application est en mesure de déchiffrer le cookie d’authentification et autorise l’accès aux ressources de l’application. Si sa prochaine requête arrive sur un autre nœud, l’application ne peut pas déchiffrer le cookie d’authentification à partir du nœud où l’utilisateur est connecté, et autorisation d’accès à la ressource demandée échoue.

En présence de l’un des symptômes suivants **par intermittence**, le problème provient généralement d’une configuration incorrecte de la protection des données ou de la mise en cache dans un environnement de batterie de serveurs web :

* Interruptions d’authentification &ndash; Le cookie d’authentification est mal configuré ou ne peut pas être déchiffré. OAuth (Facebook, Microsoft, Twitter) ou les connexions OpenIdConnect échouent avec l’erreur « Échec de la corrélation ».
* Interruptions d’autorisation &ndash; L’identité est perdue.
* L’état de session perd des données.
* Des éléments mis en cache disparaissent.
* TempData échoue.
* Échec des messages &ndash; la vérification anti-contrefaçon échoue.

Pour plus d’informations sur la configuration de la protection des données pour les déploiements sur des batteries de serveurs web, consultez <xref:security/data-protection/configuration/overview>. Pour plus d’informations sur la configuration de la mise en cache pour les déploiements sur des batteries de serveurs web, consultez <xref:performance/caching/distributed>.

## <a name="obtain-data-from-apps"></a>Obtenir des données à partir d’applications

Si les applications de la batterie de serveurs web sont capables de répondre aux requêtes, obtenez des informations sur une requête, une connexion et d’autres informations supplémentaires à partir des applications à l’aide de l’intergiciel en ligne terminal. Pour obtenir des informations supplémentaires ainsi qu'un code d'exemple, consultez <xref:test/troubleshoot#obtain-data-from-an-app>.

## <a name="additional-resources"></a>Ressources supplémentaires

* L' [extension de script personnalisé pour Windows](/azure/virtual-machines/extensions/custom-script-windows) &ndash; télécharge et exécute des scripts sur des machines virtuelles Azure, ce qui est utile pour la configuration et l’installation de logiciels après le déploiement.
* <xref:host-and-deploy/proxy-load-balancer>
 