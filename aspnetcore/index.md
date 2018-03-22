---
title: "Présentation d’ASP.NET Core"
author: rick-anderson
description: "Découvrez une introduction à ASP.NET Core, framework multiplateforme à hautes performances et open source qui permet de créer des applications cloud modernes et connectées à Internet."
manager: wpickett
ms.author: riande
ms.date: 02/28/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: index
ms.openlocfilehash: 103b7862900e08488dcc0f5fc78c08fefcfa17f3
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
---
# <a name="introduction-to-aspnet-core"></a>Présentation d’ASP.NET Core

Par [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) et [Shaun Luttin](https://twitter.com/dicshaunary)

ASP.NET Core est un framework multiplateforme à hautes performances et [open source](https://github.com/aspnet/home) pour créer des applications cloud modernes et connectées à Internet. Avec ASP.NET Core, vous pouvez :

* Créer des applications et des services web, des applications [IoT](https://www.microsoft.com/internet-of-things/) et des back-ends mobiles.
* Utiliser vos outils de développement préférés sur Windows, macOS et Linux.
* Déployer dans le cloud ou localement.
* Exécuter sur [.NET Core ou .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).

## <a name="why-use-aspnet-core"></a>Pourquoi utiliser ASP.NET Core ?

Des millions de développeurs ont utilisé (et continuent d’utiliser) [ASP.NET 4.x](https://docs.microsoft.com/aspnet/overview) pour créer des applications web. ASP.NET Core est une refonte d’ASP.NET 4.x, avec des modifications d’architecture qui aboutissent à un framework plus léger et modulaire.

ASP.NET Core offre les avantages suivants :

* Un scénario unifié pour créer une interface utilisateur web et des API web.
* L’intégration de [frameworks modernes, côté client](xref:client-side/index) et des workflows de développement.
* Un environnement prêt pour le cloud et basé sur des fichiers de [configuration système](xref:fundamentals/configuration/index).
* Une [Injection de dépendances](xref:fundamentals/dependency-injection) intégrée.
* Un pipeline de requête HTTP léger, [haute performance](https://github.com/aspnet/benchmarks) et modulaire.
* La capacité à être hébergé sur [IIS](xref:host-and-deploy/iis/index), [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), [Docker](xref:host-and-deploy/docker/index), ou sur votre propre hébergement.
* La gestion de version des applications côte à côte lorsque la cible est [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
* des outils qui simplifient le développement web moderne.
* La capacité à générer et à exécuter sur Windows, macOS et Linux.
* Open source et [centré sur la communauté](https://live.asp.net/).

ASP.NET Core est fourni entièrement sous forme de packages [NuGet](https://www.nuget.org/). Ceci vous permet d’optimiser votre application pour y inclure seulement les packages NuGet nécessaires. En fait, les applications ASP.NET Core 2.x ciblant .NET Core ne nécessitent qu’un [seul package NuGet](xref:fundamentals/metapackage). Les avantages des applications ayant une surface d’exposition plus petite incluent une sécurité accrue, une maintenance réduite et des performances améliorées.

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a>Créer des API web et une interface utilisateur web en utilisant le modèle MVC d’ASP.NET Core

Le modèle MVC d’ASP.NET Core fournit des fonctionnalités pour créer des [API web](xref:tutorials/index#build-web-apis) et des [applications web](xref:tutorials/index#build-web-apps) :

* Le [modèle MVC (Modèle-Vue-Contrôleur)](xref:mvc/overview) permet de rendre vos API web et vos applications web [testables](testing/index.md).
* [Razor Pages](xref:mvc/razor-pages/index) (nouveauté dans ASP.NET Core 2.0) est un modèle de programmation basé sur les pages qui rend plus facile et plus productive la création d’une interface utilisateur web.
* Le [balisage Razor](xref:mvc/views/razor) fournit une syntaxe efficace pour [Razor Pages](xref:mvc/razor-pages/index) et les [vues MVC](xref:mvc/views/overview).
* Les [Tag Helpers](xref:mvc/views/tag-helpers/intro) permettent au code côté serveur de participer à la création et au rendu des éléments HTML dans les fichiers Razor.
* La prise en charge intégrée de [plusieurs formats de données et de la négociation de contenu](mvc/models/formatting.md) permet à vos API web d’être utilisées par un large éventail de clients, notamment des navigateurs et des appareils mobiles.
* Le principe de la [liaison de modèle](xref:mvc/models/model-binding) permet le mappage automatiquement des données des requêtes HTTP aux paramètres des méthodes d’action.
* La [validation de modèle](xref:mvc/models/validation) effectue automatiquement la validation côté client et côté serveur.

## <a name="client-side-development"></a>Développement côté client

ASP.NET Core s’intègre parfaitement avec les frameworks et les bibliothèques populaires côté client, notamment [Angular](xref:spa/angular), [React](xref:spa/react) et [Bootstrap](xref:client-side/bootstrap). Pour plus d’informations, consultez [Développement côté client](xref:client-side/index).

## <a name="aspnet-core-targeting-net-framework"></a>ASP.NET Core ciblant .NET Framework

ASP.NET Core peut cibler .NET Core ou .NET Framework. Les applications ASP.NET Core ciblant .NET Framework ne sont pas multiplateformes : elles s’exécutent seulement sur Windows. Il n’est pas prévu de supprimer la prise en charge du ciblage de .NET Framework dans ASP.NET Core. D’une façon générale, ASP.NET Core est constitué de bibliothèques [.NET Standard](/dotnet/standard/net-standard). Les applications écrites avec .NET Standard 2.0 s’exécutent partout où .NET Standard 2.0 est pris en charge.

Le ciblage de .NET Core présente plusieurs avantages, qui sont plus nombreux à chaque version. Voici certains avantages de .NET Core par rapport à .NET Framework :

* Multiplateforme S’exécute sur macOS, Linux et Windows
* Performances améliorées
* Gestion des versions côte à côte
* Nouvelles API
* Ouvrir la source

Nous nous efforçons de combler l’écart d’API qui existe entre .NET Framework et .NET Core. Le [Pack de compatibilité Windows](/dotnet/core/porting/windows-compat-pack) a rendu disponible dans .NET Core des milliers d’API fonctionnant seulement dans Windows. Ces API n’étaient pas disponibles dans .NET Core 1.x.

## <a name="next-steps"></a>Étapes suivantes

Pour plus d'informations, reportez-vous aux ressources suivantes :

* [Bien démarrer avec les pages Razor](xref:tutorials/razor-pages/razor-pages-start)
* [Didacticiels ASP.NET Core](xref:tutorials/index)
* [Notions de base d’ASP.NET Core](xref:fundamentals/index)
* [Le point hebdomadaire de la communauté ASP.NET](https://live.asp.net/) couvre l’avancement et les plans des équipes de développement. Il comprend de nouveaux blogs et des logiciels de tiers.
