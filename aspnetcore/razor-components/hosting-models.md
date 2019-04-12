---
title: Composants de Razor modèles d’hébergement
author: guardrex
description: Comprendre les Blazor côté client et côté serveur ASP.NET Core Razor composants, modèles d’hébergement.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/28/2019
uid: razor-components/hosting-models
ms.openlocfilehash: 8ed70117c94bf1a3e4c208f70310bbf0473bae44
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59515528"
---
# <a name="razor-components-hosting-models"></a>Composants de Razor modèles d’hébergement

Par [Daniel Roth](https://github.com/danroth27)

Composants de Razor est une infrastructure web conçue pour s’exécuter côté client dans le navigateur sur un [WebAssembly](http://webassembly.org/)-en fonction du runtime .NET (*Blazor*) ou côté serveur dans ASP.NET Core (*ASP.NET Core Razor Composants*). Quel que soit les modèles d’hébergement modèle, l’application et le composant *restent les mêmes*. Cet article aborde les modèles d’hébergement disponibles :

* [Blazor côté client](#client-side-hosting-model)
* [Composants Razor ASP.NET Core côté serveur](#server-side-hosting-model)

## <a name="client-side-hosting-model"></a>Modèle d’hébergement côté client

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Le modèle d’hébergement principal pour Blazor est en cours d’exécution côté client dans le navigateur sur WebAssembly. L’application Blazor, ses dépendances et le runtime .NET sont téléchargés sur le navigateur. L’application est exécutée directement sur le thread d’interface utilisateur du navigateur. Mises à jour de l’interface utilisateur et de gestion des événements se produisent dans le même processus. Ressources de l’application sont déployés en tant que fichiers statiques à un serveur web ou un service capable de fournir du contenu statique à des clients.

![Blazor côté client : L’application Blazor s’exécute sur un thread d’interface utilisateur dans le navigateur.](hosting-models/_static/client-side.png)

Pour créer une application de Blazor en utilisant le modèle d’hébergement côté client, utilisez un des modèles suivants :

* **Blazor** ([blazor nouveau dotnet](/dotnet/core/tools/dotnet-new)) &ndash; déployé comme un ensemble de fichiers statiques.
* **Blazor (ASP.NET Core hébergées)** ([blazorhosted nouveau dotnet](/dotnet/core/tools/dotnet-new)) &ndash; hébergées par un serveur d’ASP.NET Core. L’application ASP.NET Core sert l’application Blazor aux clients. L’application de Blazor côté client peut interagir avec le serveur sur le réseau à l’aide d’appels d’API web ou [SignalR](xref:signalr/introduction).

Les modèles incluent les *components.webassembly.js* script gère :

* Télécharger le runtime .NET, l’application et les dépendances de l’application.
* Initialisation du runtime pour exécuter l’application.

Le modèle d’hébergement côté client offre les avantages suivants. Côté client Blazor :

* N’a aucune dépendance de côté serveur .NET.
* Entièrement tire parti des fonctionnalités et les ressources client.
* Déchargements fonctionnent à partir du serveur au client.
* Prend en charge les scénarios hors connexion.

Il existe des inconvénients à l’hébergement du côté client. Côté client Blazor :

* Restreint l’application aux fonctionnalités du navigateur.
* Nécessite le matériel capable de client et logiciels (par exemple, WebAssembly de prise en charge).
* A une plus grande taille de téléchargement et l’application plue le temps de chargement.
* A moins de maturité runtime .NET et les outils de prise en charge (par exemple, les limitations dans [.NET Standard](/dotnet/standard/net-standard) prise en charge et le débogage).

## <a name="server-side-hosting-model"></a>Modèle d’hébergement côté serveur

Avec le modèle d’hébergement ASP.NET Core Razor composants côté serveur, l’application est exécutée sur le serveur à partir d’une application ASP.NET Core. Mises à jour de l’interface utilisateur, la gestion des événements et les appels JavaScript sont traités sur un [SignalR](xref:signalr/introduction) connexion.

![ASP.NET Core Razor composants côté serveur : Le navigateur interagit avec l’application (hébergée à l’intérieur d’une application ASP.NET Core) sur le serveur via une connexion SignalR.](hosting-models/_static/server-side.png)

Pour créer une application de composants de Razor en utilisant le modèle d’hébergement côté serveur, utilisez ASP.NET Core **Razor composants** modèle ([razorcomponents nouveau dotnet](/dotnet/core/tools/dotnet-new)). L’application ASP.NET Core héberge l’application côté serveur de composants de Razor et configure le point de terminaison SignalR où les clients se connectent. L’application fait référence à l’application ASP.NET Core `Startup` classe à ajouter :

* Services de composants de Razor côté serveur.
* L’application pour le pipeline de traitement des requêtes.

[!code-csharp[](hosting-models/samples_snapshot/Startup.cs?highlight=5,27)]

Le *components.server.js* script&dagger; établit la connexion cliente. Il est responsable de l’application pour rendre persistantes et restaurer l’état de l’application en fonction des besoins (par exemple, en cas d’une connexion réseau perdues).

Le modèle d’hébergement côté serveur offre plusieurs avantages. Composants côté serveur Razor :

* Avoir une taille d’application beaucoup plus petite qu’une application de Blazor côté client et chargées beaucoup plus rapidement.
* Peut tirer pleinement parti des fonctionnalités de serveur, notamment l’utilisation des API compatibles de .NET Core.
* Exécuter sur .NET Core sur le serveur, afin que .NET existantes des outils, telles que le débogage, fonctionne comme prévu.
* Fonctionne avec les clients légers (par exemple, les navigateurs qui ne prennent pas en charge le WebAssembly et ressources limité des appareils).
* .NET /C# base de code, y compris le code du composant de l’application, n’est pas prise en charge pour les clients.

Il existe des inconvénients à l’hébergement du côté serveur. Composants côté serveur Razor :

* Avoir une latence plus élevée : Chaque interaction utilisateur implique un tronçon réseau.
* N’offre aucune prise en charge hors connexion : Si la connexion du client échoue, l’application cesse de fonctionner.
* Ont réduit l’évolutivité : Le serveur doit gérer plusieurs connexions de client et gérer l’état du client.
* Exiger un serveur d’ASP.NET Core pour servir de l’application. Déploiement sans serveur (par exemple, à partir d’un CDN) n’est pas possible.

&dagger;Le *components.server.js* script est publié dans le chemin d’accès suivant : *bin / {déboguer | Mise en production} / {FRAMEWORK cible} /publish/ {nom de l’APPLICATION}. Dist/application/_framework*.
