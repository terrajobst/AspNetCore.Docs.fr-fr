---
title: ASP.NET Core les modèles de Blazor
author: guardrex
description: En savoir plus sur les ASP.NET Core Blazor les modèles d’application et la structure de projet Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2020
no-loc:
- Blazor
- SignalR
uid: blazor/templates
ms.openlocfilehash: 71a9d9eee8637dda0b3cecac82ff96a0c3bfedb5
ms.sourcegitcommit: f3b1bcfd108e5d53f73abc0bf2555890869d953b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80320981"
---
# <a name="aspnet-core-opno-locblazor-templates"></a>ASP.NET Core les modèles de Blazor

Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Le Blazor Framework fournit des modèles pour développer des applications pour chacun des modèles d’hébergement Blazor :

* Blazor webassembly (`blazorwasm`)
* Serveur de Blazor (`blazorserver`)

Pour plus d’informations sur les modèles d’hébergement de Blazor, consultez <xref:blazor/hosting-models>.

Pour obtenir des instructions pas à pas sur la création d’une application Blazor à partir d’un modèle, consultez <xref:blazor/get-started>.

## <a name="opno-locblazor-project-structure"></a>structure de projet Blazor

Les fichiers et dossiers suivants composent une Blazor application générée à partir d’un modèle de Blazor :

* *Program.cs* &ndash; le point d’entrée de l’application qui installe les éléments suivants :

  * [Hôte](xref:fundamentals/host/generic-host) ASP.net Core (serveurBlazor)
  * Hôte webassembly (Blazor webassembly) &ndash; le code de ce fichier est propre aux applications créées à partir du modèle Blazor webassembly (`blazorwasm`).
    * Le composant `App`, qui est le composant racine de l’application, est spécifié en tant qu’élément DOM `app` à la méthode `Add`.
    * Les services peuvent être configurés avec la méthode `ConfigureServices` sur le générateur de l’hôte (par exemple, `builder.Services.AddSingleton<IMyDependency, MyDependency>();`).
    * La configuration peut être fournie par le biais du générateur d’ordinateur hôte (`builder.Configuration`).

* *Startup.cs* (serveurBlazor) &ndash; contient la logique de démarrage de l’application. La classe `Startup` définit deux méthodes :

  * `ConfigureServices` &ndash; configure les services d' [injection de dépendances](xref:fundamentals/dependency-injection) de l’application. Dans Blazor applications serveur, les services sont ajoutés en appelant <xref:Microsoft.Extensions.DependencyInjection.ComponentServiceCollectionExtensions.AddServerSideBlazor*>, et le `WeatherForecastService` est ajouté au conteneur de service pour une utilisation par l’exemple de composant `FetchData`.
  * `Configure` &ndash; configure le pipeline de traitement des demandes de l’application :
    * <xref:Microsoft.AspNetCore.Builder.ComponentEndpointRouteBuilderExtensions.MapBlazorHub*> est appelé pour configurer un point de terminaison pour la connexion en temps réel avec le navigateur. La connexion est créée avec [SignalR](xref:signalr/introduction), qui est une infrastructure permettant d’ajouter des fonctionnalités Web en temps réel aux applications.
    * [MapFallbackToPage (« /_Host »)](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapFallbackToPage*) est appelé pour configurer la page racine de l’application (*pages/_Host. cshtml*) et activer la navigation.

* *wwwroot/index.html* (Blazor webassembly) &ndash; la page racine de l’application implémentée en tant que page HTML :
  * Quand une page de l’application est initialement demandée, cette page est rendue et renvoyée dans la réponse.
  * La page spécifie l’emplacement où le composant racine `App` est restitué. Le composant `App` (*app. Razor*) est spécifié en tant qu’élément DOM `app` à la méthode `AddComponent` dans `Startup.Configure`.
  * Le fichier JavaScript `_framework/blazor.webassembly.js` est chargé, ce qui suit :
    * Télécharge le Runtime .NET, l’application et les dépendances de l’application.
    * Initialise le runtime pour exécuter l’application.

* *App. razor* &ndash; le composant racine de l’application qui configure le routage côté client à l’aide du composant <xref:Microsoft.AspNetCore.Components.Routing.Router>. Le composant `Router` intercepte la navigation dans le navigateur et restitue la page qui correspond à l’adresse demandée.

* Le dossier *Pages* &ndash; contient les composants/pages routables ( *. Razor*) qui composent l’application Blazor et la page Razor racine d’une application serveur Blazor. L’itinéraire de chaque page est spécifié à l’aide de la directive [`@page`](xref:mvc/views/razor#page) . Le modèle comprend les éléments suivants :
  * *_Host. cshtml* (serveurBlazor) &ndash; la page racine de l’application implémentée en tant que page Razor :
    * Quand une page de l’application est initialement demandée, cette page est rendue et renvoyée dans la réponse.
    * Le `_framework/blazor.server.js` fichier JavaScript est chargé, ce qui configure la connexion SignalR en temps réel entre le navigateur et le serveur.
    * La page hôte spécifie où le composant racine `App` (*app. Razor*) est affiché.
  * `Counter` (*Counter. Razor*) &ndash; implémente la page du compteur.
  * `Error` (*Error. Razor*, Blazor application serveur uniquement) &ndash; rendu lorsqu’une exception non gérée se produit dans l’application.
  * `FetchData` (*fetchData. Razor*) &ndash; implémente la page extraire des données.
  * `Index` (*index. Razor*) &ndash; implémente la page d’hébergement.

* Le dossier *partagé* &ndash; contient d’autres composants d’interface utilisateur ( *. Razor*) utilisés par l’application :
  * `MainLayout` (*MainLayout. Razor*) &ndash; le composant de disposition de l’application.
  * `NavMenu` (*NavMenu. Razor*) &ndash; implémente la navigation dans l’encadré. Comprend le [composant NavLink](xref:blazor/routing#navlink-component) (<xref:Microsoft.AspNetCore.Components.Routing.NavLink>), qui restitue des liens de navigation vers d’autres composants Razor. Le composant `NavLink` indique automatiquement un état sélectionné lors du chargement de son composant, ce qui aide l’utilisateur à comprendre quel composant est actuellement affiché.

* *_Imports. razor* &ndash; inclut des directives Razor courantes à inclure dans les composants de l’application ( *. Razor*), tels que les directives [`@using`](xref:mvc/views/razor#using) pour les espaces de noms.

* Dossier de *données* (Blazor Server) &ndash; contient la classe `WeatherForecast` et l’implémentation du `WeatherForecastService` qui fournissent des exemples de données météorologiques au composant `FetchData` de l’application.

* *wwwroot* &ndash; le dossier [racine Web](xref:fundamentals/index#web-root) de l’application contenant les ressources statiques publiques de l’application.

* *appSettings. JSON* (serveurBlazor) &ndash; les paramètres de configuration de l’application.
