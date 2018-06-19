---
title: Module ASP.NET Core
author: rick-anderson
description: Découvrez comment le module ASP.NET Core permet au serveur web Kestrel d’utiliser IIS ou IIS Express en tant que serveur proxy inverse.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/23/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 789b0b2e74405256bb0e247ae5ea9697e3cb28e5
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34153703"
---
# <a name="aspnet-core-module"></a>Module ASP.NET Core

By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl) et [Chris Ross](https://github.com/Tratcher) 

Le module ASP.NET Core permet d’exécuter les applications ASP.NET Core derrière IIS dans une configuration de proxy inverse. IIS fournit des fonctionnalités avancées de sécurité et de gestion des applications web.

Versions Windows prises en charge :

* Windows 7 ou version ultérieure
* Windows Server 2008 R2 ou une version ultérieure

Le module ASP.NET Core fonctionne seulement avec Kestrel. Le module n’est pas compatible avec [HTTP.sys](xref:fundamentals/servers/httpsys) (anciennement [WebListener](xref:fundamentals/servers/weblistener)).

## <a name="aspnet-core-module-description"></a>Description du module ASP.NET Core

Le module ASP.NET Core est un module IIS natif qui se connecte dans le pipeline IIS pour rediriger les requêtes web aux applications ASP.NET Core du backend. De nombreux modules natifs, comme l’authentification Windows, restent actifs. Pour plus d’informations sur les modules IIS actifs avec le module, consultez [Modules IIS](xref:host-and-deploy/iis/modules).

Étant donné que les applications ASP.NET Core s’exécutent dans un processus distinct du processus de travail IIS, le module traite également la gestion des processus. Le module démarre le processus de l’application ASP.NET Core quand la première requête arrive et redémarre l’application si elle plante. Il s’agit essentiellement du même comportement que celui des applications ASP.NET 4.x qui s’exécutent in-process dans IIS et sont gérées par le [service WAS (Windows Activation Service)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

Le diagramme suivant montre la relation entre IIS, le module ASP.NET Core et les applications ASP.NET Core :

![Module ASP.NET Core](aspnet-core-module/_static/ancm.png)

Les requêtes arrivent du web au pilote HTTP.sys en mode noyau. Le pilote route les requêtes vers IIS sur le port configuré du site web, généralement 80 (HTTP) ou 443 (HTTPS). Le module transfère les requêtes à Kestrel sur un port aléatoire pour l’application, qui n’est pas le port 80/443.

Le module spécifie le port via une variable d’environnement au démarrage, et le middleware d’intégration IIS configure le serveur pour qu’il écoute sur `http://localhost:{port}`. Des vérifications supplémentaires sont effectuées, et les requêtes qui ne proviennent pas du module sont rejetées. Le module ne prend pas en charge le transfert HTTPS : les requêtes sont donc transférées via HTTP, même si IIS les reçoit via HTTPS.

Une fois que Kestrel prend une requête dans le module, cette requête est envoyée (push) dans le pipeline de middlewares d’ASP.NET Core. Le pipeline de middlewares traite la requête et la passe en tant qu’instance de `HttpContext` à la logique de l’application. La réponse de l’application est ensuite repassée à IIS, qui la renvoie au client HTTP à l’origine de la requête.

Le module ASP.NET Core a quelques autres fonctions. Le module peut :

* Définir des variables d’environnement pour le processus de travail.
* Journaliser la sortie de stdout dans un stockage de fichiers pour résoudre les problèmes de démarrage.
* Transférer des jetons d’authentification Windows.

## <a name="how-to-install-and-use-the-aspnet-core-module"></a>Comment installer et utiliser le module ASP.NET Core

Pour obtenir des instructions détaillées sur l’installation et l’utilisation du module ASP.NET Core, consultez [Héberger sur Windows avec IIS](xref:host-and-deploy/iis/index). Pour plus d’informations sur la configuration du module, consultez les [Informations de référence sur la configuration du module ASP.NET Core](xref:host-and-deploy/aspnet-core-module).

## <a name="additional-resources"></a>Ressources supplémentaires

* [Héberger sur Windows avec IIS](xref:host-and-deploy/iis/index)
* [Informations de référence sur la configuration du Module ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Dépôt GitHub du module ASP.NET Core (code source)](https://github.com/aspnet/AspNetCoreModule)
