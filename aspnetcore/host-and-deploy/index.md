---
title: Héberger et déployer ASP.NET Core
author: rick-anderson
description: Découvrez comment configurer des environnements d’hébergement et déployer des applications ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/26/2018
uid: host-and-deploy/index
ms.openlocfilehash: f70b05df6bf710e2ab54a1eaafb71b4f9b306cbe
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52450617"
---
# <a name="host-and-deploy-aspnet-core"></a>Héberger et déployer ASP.NET Core

En général, pour déployer une application ASP.NET Core sur un environnement d’hébergement :

* Publier l’application dans un dossier sur le serveur d’hébergement.
* Configurer un gestionnaire de processus qui démarre l’application à l’arrivée des requêtes et redémarre l’application en cas de blocage ou quand le serveur redémarre.
* Si la configuration d’un proxy inverse est souhaitée, configurer un proxy inverse qui transfère les demandes à l’application.

## <a name="publish-to-a-folder"></a>Publier dans un dossier

La commande CLI [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) compile le code d’application et copie les fichiers nécessaires pour exécuter l’application dans un dossier *publish*. Dans le cadre d’un déploiement à partir de Visual Studio, l’étape [dotnet publish](/dotnet/core/tools/dotnet-publish) est effectuée automatiquement avant que les fichiers ne soient copiés dans la destination du déploiement.

### <a name="folder-contents"></a>Contenu du dossier

Le dossier *publish* contient des fichiers *.exe* et *.dll* pour l’application, ses dépendances et éventuellement le runtime .NET.

Une application .NET Core peut être publiée en tant qu’application *autonome* ou *dépendante du framework*. Si l’application est autonome, les fichiers *.dll* qui contiennent le runtime .NET sont inclus dans le dossier *publish*. Si l’application dépend du framework, les fichiers du runtime .NET ne sont pas inclus, car l’application a une référence à une version de .NET installée sur le serveur. Le modèle de déploiement par défaut est « dépendante du framework ». Pour plus d’informations, consultez [Déploiement d’applications .NET Core](/dotnet/articles/core/deploying/index).

En plus des fichiers *.exe* et *.dll*, le dossier *publish* d’une application ASP.NET Core contient généralement des fichiers de configuration, des ressources statiques et des vues MVC. Pour plus d'informations, consultez <xref:host-and-deploy/directory-structure>.

## <a name="set-up-a-process-manager"></a>Configurer un gestionnaire de processus

Une application ASP.NET Core est une application console qui doit être démarrée quand un serveur démarre, et redémarrée si elle se bloque. Pour automatiser le démarrage et le redémarrage, un gestionnaire de processus est nécessaire. Les gestionnaires de processus les plus courants pour ASP.NET Core sont les suivants :

* Linux
  * [Nginx](xref:host-and-deploy/linux-nginx)
  * [Apache](xref:host-and-deploy/linux-apache)
* Windows
  * [IIS](xref:host-and-deploy/iis/index)
  * [Service Windows](xref:host-and-deploy/windows-service)

## <a name="set-up-a-reverse-proxy"></a>Configurer un proxy inverse

::: moniker range=">= aspnetcore-2.0"

Si l’application utilise le serveur web [Kestrel](xref:fundamentals/servers/kestrel), alors [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache) ou [IIS](xref:host-and-deploy/iis/index) peut être utilisé comme serveur proxy inverse. Un serveur proxy inverse reçoit les requêtes HTTP en provenance d’Internet et les transmet à Kestrel après un traitement préliminaire.

Les deux configurations&mdash;avec ou sans serveur proxy inverse&mdash;sont des configurations d’hébergement valides et prises en charge pour les applications ASP.NET Core versions 2.0 ou ultérieures. Pour plus d’informations, consultez [Quand utiliser Kestrel avec un proxy inverse ?](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Si l’application utilise le serveur web [Kestrel](xref:fundamentals/servers/kestrel) et qu’elle est exposée à Internet, utilisez [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache) ou [IIS](xref:host-and-deploy/iis/index) comme serveur proxy inverse. Un serveur proxy inverse reçoit les requêtes HTTP en provenance d’Internet et les transmet à Kestrel après un traitement préliminaire. La principale raison pour utiliser un proxy inverse est la sécurité. Pour plus d’informations, consultez [Quand utiliser Kestrel avec un proxy inverse ?](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy).

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a>Scénarios avec un serveur proxy et un équilibreur de charge

Une configuration supplémentaire peut être nécessaire pour les applications hébergées derrière des serveurs proxy et des équilibreurs de charge. Sans configuration supplémentaire, une application peut ne pas avoir accès au schéma (HTTP/HTTPS) et à l’adresse IP distante d’où provient une requête. Pour plus d’informations, consultez [Configurer ASP.NET Core pour l’utilisation de serveurs proxy et d’équilibreurs de charge](xref:host-and-deploy/proxy-load-balancer).

## <a name="use-visual-studio-and-msbuild-to-automate-deployments"></a>Utiliser Visual Studio et MSBuild pour automatiser les déploiements

Le déploiement nécessite souvent des tâches supplémentaires en plus de la copie de la sortie de [dotnet publish](/dotnet/core/tools/dotnet-publish) vers un serveur. Par exemple, des fichiers supplémentaires peuvent être requis ou exclus du dossier *publish*. Visual Studio utilise MSBuild pour le déploiement web, et MSBuild peut être personnalisé pour effectuer de nombreuses autres tâches pendant le déploiement. Pour plus d’informations, consultez <xref:host-and-deploy/visual-studio-publish-profiles> et l’ouvrage [Using MSBuild and Team Foundation Build](http://msbuildbook.com/) (Utilisation de MSBuild et Team Foundation Build).

Les applications peuvent être déployées directement à partir de Visual Studio sur Azure App Service à l’aide de la [fonctionnalité de publication web](xref:tutorials/publish-to-azure-webapp-using-vs) ou de la [prise en charge intégrée de Git](xref:host-and-deploy/azure-apps/azure-continuous-deployment). Azure DevOps Services prend en charge le [déploiement continu sur Azure App Service](/azure/devops/pipelines/targets/webapp).

## <a name="publish-to-azure"></a>Publier sur Azure

Consultez <xref:tutorials/publish-to-azure-webapp-using-vs> pour obtenir des instructions sur la façon de publier une application sur Azure à l’aide de Visual Studio. Il est également possible de la publier sur Azure à partir de la [ligne de commande](/azure/app-service/app-service-web-get-started-dotnet).

## <a name="host-in-a-web-farm"></a>Héberger dans une batterie de serveurs web

Pour plus d’informations sur la configuration pour héberger des applications ASP.NET Core dans un environnement de batterie de serveurs web (par exemple, le déploiement de plusieurs instances de votre application pour la scalabilité), consultez <xref:host-and-deploy/web-farm>.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:host-and-deploy/docker/index>
* <xref:test/troubleshoot>
