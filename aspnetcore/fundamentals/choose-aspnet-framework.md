---
title: Choisir entre ASP.NET 4.x et ASP.NET Core
author: rick-anderson
description: Explique ASP.NET Core par rapport à ASP.NET 4.x, et comment choisir entre les deux.
ms.author: riande
ms.date: 09/11/2018
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 79e56e10b756677431ceff289300c251e54bf632
ms.sourcegitcommit: bdfba5e7575b2a786ef27c0edf688c7dbd09ee95
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/22/2018
ms.locfileid: "52288666"
---
# <a name="choose-between-aspnet-4x-and-aspnet-core"></a>Choisir entre ASP.NET 4.x et ASP.NET Core

ASP.NET Core est une refonte d’ASP.NET 4.x. Cet article liste les différences existant entre eux.

## <a name="aspnet-core"></a>ASP.NET Core

ASP.NET Core est un framework open source multiplateforme qui permet de créer des applications web cloud modernes sur Windows, macOS et Linux.

[!INCLUDE[](~/includes/benefits.md)]

## <a name="aspnet-4x"></a>ASP.NET 4.x

ASP.NET 4.x est un framework abouti qui offre les services nécessaires pour créer sur Windows des applications web, basées sur serveur et destinées à l’entreprise.

## <a name="framework-selection"></a>Sélection du Framework

Le tableau suivant compare ASP.NET Core à ASP.NET 4.x.

| ASP.NET Core | ASP.NET 4.x |
|---|---|
|Générer pour Windows, macOS ou Linux|Générer pour Windows|
|Nous vous recommandons d’utiliser les [pages Razor](xref:razor-pages/index) pour créer une interface utilisateur web à partir d’ASP.NET Core 2. Voir aussi [MVC](xref:mvc/overview), [API web](xref:tutorials/first-web-api) et [SignalR](xref:signalr/introduction).|Utilisez [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/) ou [Pages Web](/aspnet/web-pages)|
|Plusieurs versions par machine|Une seule version par machine|
|Développer avec Visual Studio, [Visual Studio pour Mac](https://www.visualstudio.com/vs/visual-studio-mac/) ou [Visual Studio Code](https://code.visualstudio.com/) en utilisant C# ou F#|Développer avec Visual Studio en utilisant C#, VB ou F#|
|Performances supérieures à celles d’ASP.NET 4.x|Bonnes performances|
|[Choisir le runtime .NET Framework ou .NET Core](/dotnet/articles/standard/choosing-core-framework-server)|Utiliser le runtime .NET Framework|

Consultez [ASP.NET Core ciblant le .NET Framework](xref:index#target-framework) pour plus d’informations sur la prise en charge d’ASP.NET Core 2.x sur le .NET Framework.

## <a name="aspnet-core-scenarios"></a>Scénarios ASP.NET Core

* Nous vous recommandons d’utiliser les [pages Razor](xref:razor-pages/index) pour créer une interface utilisateur web à partir d’ASP.NET Core 2.
* [Sites web](xref:tutorials/first-mvc-app/index)
* [API](xref:tutorials/first-web-api)
* [Temps réel](xref:signalr/index)
* [Déployer une application ASP.NET Core sur Azure](/azure/app-service/app-service-web-get-started-dotnet)

## <a name="aspnet-4x-scenarios"></a>Scénarios ASP.NET 4.x

* [Sites web](/aspnet/mvc)
* [API](/aspnet/web-api)
* [Temps réel](/aspnet/signalr)
* [Créer une application web ASP.NET 4.x dans Azure](/azure/app-service/app-service-web-get-started-dotnet-framework)

## <a name="additional-resources"></a>Ressources supplémentaires

* [Présentation d’ASP.NET](/aspnet/overview)
* [Présentation d’ASP.NET Core](xref:index)
* <xref:host-and-deploy/azure-apps/index>

> [!NOTE]
> Nous testons la facilité d’utilisation d’une nouvelle structure proposée pour la table des matières d’ASP.NET Core.  Si vous avez quelques minutes pour essayer un exercice de recherche de 7 différentes rubriques dans la table des matières actuelle ou proposée, [cliquez ici pour participer à l’étude](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).