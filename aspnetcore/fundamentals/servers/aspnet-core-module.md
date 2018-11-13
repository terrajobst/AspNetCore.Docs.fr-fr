---
title: Module ASP.NET Core
author: guardrex
description: Découvrez comment le module ASP.NET Core permet au serveur web Kestrel d’utiliser IIS ou IIS Express en tant que serveur proxy inverse.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/21/2018
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 39c1b364f9dab635c79e00561d212c858c0c4395
ms.sourcegitcommit: 09affee3d234cb27ea6fe33bc113b79e68900d22
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2018
ms.locfileid: "51191254"
---
# <a name="aspnet-core-module"></a>Module ASP.NET Core

By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl) et [Chris Ross](https://github.com/Tratcher)

::: moniker range=">= aspnetcore-2.2"

Le module ASP.NET Core permet d’exécuter les applications ASP.NET Core dans un processus de travail IIS (*in-process*), ou derrière IIS dans une configuration de proxy inverse (*out-of-process*). IIS fournit des fonctionnalités avancées de sécurité et de gestion des applications web.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Le module ASP.NET Core permet d’exécuter les applications ASP.NET Core derrière IIS dans une configuration de proxy inverse. IIS fournit des fonctionnalités avancées de sécurité et de gestion des applications web.

::: moniker-end

Versions Windows prises en charge :

* Windows 7 ou version ultérieure
* Windows Server 2008 R2 ou une version ultérieure

::: moniker range=">= aspnetcore-2.2"

Lors de l’hébergement in-process, le module dispose de sa propre implémentation de serveur, `IISHttpServer`.

Lors de l’hébergement out-of-process, le module fonctionne uniquement avec Kestrel. Le module n’est pas compatible avec [HTTP.sys](xref:fundamentals/servers/httpsys) (anciennement [WebListener](xref:fundamentals/servers/weblistener)).

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Le module fonctionne seulement avec Kestrel. Le module n’est pas compatible avec [HTTP.sys](xref:fundamentals/servers/httpsys) (anciennement [WebListener](xref:fundamentals/servers/weblistener)).

::: moniker-end

## <a name="aspnet-core-module-description"></a>Description du module ASP.NET Core

::: moniker range=">= aspnetcore-2.2"

Le module ASP.NET Core est un module IIS natif qui se connecte dans le pipeline IIS pour effectuer l’une des opérations suivantes :

* Héberger une application ASP.NET Core à l’intérieur du processus de travail IIS (`w3wp.exe`), appelé [modèle d’hébergement in-process](#in-process-hosting-model).

* Transférer les requêtes web à une application ASP.NET Core du back-end exécutant le [serveur Kestrel](xref:fundamentals/servers/kestrel), appelé [modèle d’hébergement out-of-process](#out-of-process-hosting-model).

### <a name="in-process-hosting-model"></a>Modèle d’hébergement in-process

En utilisant l’hébergement in-process, une application ASP.NET Core s’exécute dans le même processus que son processus de travail IIS. Une baisse des performances est ainsi évitée dans le traitement par proxy des requêtes sur la carte de bouclage lorsque vous utilisez le modèle d’hébergement out-of-process. IIS s’occupe de la gestion des processus par l’intermédiaire du [service d’activation des processus Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

Le module ASP.NET Core :

* Effectue l’initialisation de l’application.
  * Charge le [CoreCLR](/dotnet/standard/glossary#coreclr).
  * Appelle `Program.Main`.
* Gère la durée de vie de la requête native IIS.

Le schéma suivant montre la relation entre IIS, le module ASP.NET Core et une application hébergée dans un processus :

![Module ASP.NET Core](aspnet-core-module/_static/ancm-inprocess.png)

Une requête arrive du web au pilote HTTP.sys en mode noyau. Le pilote achemine la requête native vers IIS sur le port configuré du site web, généralement 80 (HTTP) ou 443 (HTTPS). Le module reçoit la requête native et passe le contrôle à `IISHttpServer`, ce qui par le fait transforme la requête native en requête managée.

Dès que `IISHttpServer` sélectionne la requête, celle-ci est envoyée (push) dans le pipeline de middlewares d’ASP.NET Core. Le pipeline de middlewares traite la requête et la passe en tant qu’instance de `HttpContext` à la logique de l’application. La réponse de l’application est ensuite repassée à IIS, qui la renvoie au client HTTP à l’origine de la requête.

### <a name="out-of-process-hosting-model"></a>Modèle d’hébergement out-of-process

Étant donné que les applications ASP.NET Core s’exécutent dans un processus distinct du processus de travail IIS, le module s’occupe de la gestion du processus. Le module démarre le processus pour l’application ASP.NET Core quand la première requête arrive, et il redémarre l’application si elle s’arrête ou se bloque. Il s’agit essentiellement du même comportement que celui des applications s’exécutant in-process, et qui sont gérées par le [service d’activation des processus Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

Le schéma suivant illustre la relation entre IIS, le module ASP.NET Core et une application hébergée hors processus :

![Module ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

Les requêtes arrivent du web au pilote HTTP.sys en mode noyau. Le pilote route les requêtes vers IIS sur le port configuré du site web, généralement 80 (HTTP) ou 443 (HTTPS). Le module transfère les requêtes à Kestrel sur un port aléatoire pour l’application, qui n’est pas le port 80/443.

Le module spécifie le port via une variable d’environnement au démarrage, et le middleware d’intégration IIS configure le serveur pour qu’il écoute sur `http://localhost:{port}`. Des vérifications supplémentaires sont effectuées, et les requêtes qui ne proviennent pas du module sont rejetées. Le module ne prend pas en charge le transfert HTTPS : les requêtes sont donc transférées via HTTP, même si IIS les reçoit via HTTPS.

Dès que Kestrel sélectionne la requête dans le module, celle-ci est envoyée (push) dans le pipeline de middlewares d’ASP.NET Core. Le pipeline de middlewares traite la requête et la passe en tant qu’instance de `HttpContext` à la logique de l’application. Le middleware ajouté par l’intégration d’IIS met à jour le schéma, l’adresse IP distante et la base du chemin pour prendre en compte le transfert de la requête à Kestrel. La réponse de l’application est ensuite repassée à IIS, qui la renvoie au client HTTP à l’origine de la requête.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Le module ASP.NET Core est un module IIS natif qui se connecte dans le pipeline IIS pour transférer les requêtes web aux applications ASP.NET Core du back-end.

Étant donné que les applications ASP.NET Core s’exécutent dans un processus distinct du processus de travail IIS, le module traite également la gestion des processus. Le module démarre le processus de l’application ASP.NET Core quand la première requête arrive et redémarre l’application si elle plante. Il s’agit essentiellement du même comportement que celui des applications ASP.NET 4.x qui s’exécutent in-process dans IIS et sont gérées par le [service WAS (Windows Activation Service)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

Le schéma suivant illustre la relation entre IIS, le module ASP.NET Core et une application :

![Module ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

Les requêtes arrivent du web au pilote HTTP.sys en mode noyau. Le pilote route les requêtes vers IIS sur le port configuré du site web, généralement 80 (HTTP) ou 443 (HTTPS). Le module transfère les requêtes à Kestrel sur un port aléatoire pour l’application, qui n’est pas le port 80/443.

Le module spécifie le port via une variable d’environnement au démarrage, et le middleware d’intégration IIS configure le serveur pour qu’il écoute sur `http://localhost:{port}`. Des vérifications supplémentaires sont effectuées, et les requêtes qui ne proviennent pas du module sont rejetées. Le module ne prend pas en charge le transfert HTTPS : les requêtes sont donc transférées via HTTP, même si IIS les reçoit via HTTPS.

Dès que Kestrel sélectionne la requête dans le module, celle-ci est envoyée (push) dans le pipeline de middlewares d’ASP.NET Core. Le pipeline de middlewares traite la requête et la passe en tant qu’instance de `HttpContext` à la logique de l’application. Le middleware ajouté par l’intégration d’IIS met à jour le schéma, l’adresse IP distante et la base du chemin pour prendre en compte le transfert de la requête à Kestrel. La réponse de l’application est ensuite repassée à IIS, qui la renvoie au client HTTP à l’origine de la requête.

::: moniker-end

De nombreux modules natifs, comme l’authentification Windows, restent actifs. Pour obtenir plus d’informations sur les modules IIS actifs avec le module, consultez <xref:host-and-deploy/iis/modules>.

Le module ASP.NET Core a quelques autres fonctions. Le module peut :

* Définir des variables d’environnement pour le processus de travail.
* Journaliser la sortie de stdout dans un stockage de fichiers pour résoudre les problèmes de démarrage.
* Transférer des jetons d’authentification Windows.

## <a name="how-to-install-and-use-the-aspnet-core-module"></a>Comment installer et utiliser le module ASP.NET Core

Pour obtenir des instructions détaillées sur l’installation et l’utilisation du module ASP.NET Core, consultez <xref:host-and-deploy/iis/index>. Pour plus d’informations sur la configuration du module, consultez <xref:host-and-deploy/aspnet-core-module>.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* [Dépôt GitHub du module ASP.NET Core (code source)](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
