---
title: Introduction à Razor Components
author: guardrex
description: Explorez Blazor, framework web .NET en C#/Razor et HTML qui s’exécute dans le navigateur avec WebAssembly.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/index
ms.openlocfilehash: 0f7a4b2ec404b6ceec2e643fea9e0635de6ad716
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667919"
---
# <a name="introduction-to-razor-components"></a>Introduction à Razor Components

Par [Steve Sanderson](http://blog.stevensanderson.com), [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

*ASP.NET Core Razor Components* s’exécute côté serveur dans ASP.NET Core, tandis que *Blazor* (Razor Components exécuté côté client) est un framework web .NET expérimental en C#/Razor et HTML qui s’exécute dans le navigateur avec WebAssembly. Blazor fournit tous les avantages d’un framework d’interface utilisateur web côté client en utilisant .NET sur le client.

Le développement web a connu de nombreuses améliorations au fil des ans, mais la création d’applications web modernes présentent toujours certains défis. L’utilisation de .NET dans le navigateur offre de nombreux avantages susceptibles de simplifier le développement web et d’accroître la productivité :

* **Stabilité et cohérence** : .NET fournit des frameworks de programmation standardisés sur plusieurs plateformes qui sont stables, riches en fonctionnalités et faciles à utiliser.
* **Langages modernes et innovants** : les langages .NET sont constamment améliorés avec de nouvelles fonctionnalités de langage novatrices.
* **Outils de pointe** : la famille de produits Visual Studio fournit une fantastique expérience de développement .NET sur plusieurs plateformes sur Windows, Linux et macOS.
* **Vitesse et scalabilité** : .NET a un solide historique en matière de performances, de fiabilité et de sécurité pour le développement d’applications. L’utilisation de .NET en tant que solution de pile complète facilite la création d’applications rapides, fiables et sécurisées.
* **Développement de pile complète qui tire parti des compétences existantes** : les développeurs C#/Razor utilisent leurs compétences C#/Razor existantes pour écrire du code côté client et partager la logique côté serveur et côté client entre les applications.
* **Prise en charge étendue des navigateurs** : Razor Components restitue l’interface utilisateur sous forme de balisage ordinaire et JavaScript. Blazor s’exécute sur .NET dans le navigateur à l’aide de standards web ouverts, sans aucun plug-in ni aucune transpilation de code. Blazor fonctionne dans tous les navigateurs web modernes, notamment les navigateurs mobiles.

## <a name="hosting-models"></a>Modèles d'hébergement

### <a name="server-side-hosting-model"></a>Modèle d’hébergement côté serveur

Étant donné que Razor Components sépare la logique de rendu d’un composant de la façon dont les mises à jour de l’interface utilisateur sont appliquées, vous disposez d’une flexibilité dans la manière dont Razor Components peut être hébergé. ASP.NET Core Razor Components dans .NET Core 3.0 ajoute la prise en charge de l’hébergement de Razor Components sur le serveur dans une application ASP.NET Core où toutes les mises à jour de l’interface utilisateur sont gérées par le biais d’une connexion SignalR. Le runtime gère l’envoi des événements d’interface utilisateur du navigateur au serveur, puis applique les mises à jour de l’interface utilisateur renvoyées par le serveur dans le navigateur après avoir exécuté les composants. La même connexion est également utilisée pour traiter les appels d’interopérabilité JavaScript.

![Razor Components exécute le code .NET sur le serveur et interagit avec le modèle DOM (Document Object Model) sur le client par le biais d’une connexion SignalR](index/_static/aspnet-core-razor-components.png)

Pour plus d'informations, consultez <xref:razor-components/hosting-models#server-side-hosting-model>.

### <a name="client-side-hosting-model"></a>Modèle d’hébergement côté client

L’exécution de code .NET à l’intérieur des navigateurs web est rendue possible grâce à une technologie relativement récente, [WebAssembly](http://webassembly.org) (abrégé en *wasm*). WebAssembly est un standard web ouvert pris en charge dans les navigateurs web sans plug-in. WebAssembly est un format bytecode compact optimisé pour un téléchargement rapide et une vitesse d’exécution maximale.

Le code WebAssembly peut accéder à toutes les fonctionnalités du navigateur par le biais de l’interopérabilité JavaScript. En même temps, il s’exécute dans le même bac à sable approuvé JavaScript, afin de prévenir toute action malveillante sur l’ordinateur client.

![Blazor exécute le code .NET dans le navigateur avec WebAssembly.](index/_static/blazor.png)

Quand une application Blazor est créée et s’exécute dans un navigateur :

1. Les fichiers de code C# et les fichiers Razor sont compilés dans des assemblys .NET.
1. Les assemblys et le runtime .NET sont téléchargés dans le navigateur.
1. Blazor utilise JavaScript pour démarrer le runtime .NET et configure le runtime pour charger les références d’assemblys nécessaires. La manipulation du modèle DOM et les appels d’API de navigateur sont gérés par le runtime Blazor par le biais de l’interopérabilité JavaScript.

Pour prendre en charge les navigateurs plus anciens qui ne prennent pas en charge WebAssembly, vous pouvez utiliser le [modèle d’hébergement côté serveur](#server-side-hosting-model) d’ASP.NET Core Razor Components.

Pour plus d'informations, consultez <xref:razor-components/hosting-models#client-side-hosting-model>.

## <a name="components"></a>Composants

Les applications sont générées avec des *composants*. Un composant est un élément d’interface utilisateur, comme une page, une boîte de dialogue ou un formulaire de saisie de données. Les composants peuvent être imbriqués, réutilisés et partagés entre les projets.

Un *composant* est une classe .NET. La classe peut être écrite directement, en tant que classe C# (*\*.cs*), ou plus souvent sous la forme d’une page de balisage Razor (*\*.cshtml*).

[Razor](/aspnet/core/mvc/views/razor) est une syntaxe pour la combinaison de balisage HTML et de code C#. Razor accroît la productivité des développeurs en leur permettant de basculer entre le balisage et le code C# dans le même fichier avec prise en charge d’[IntelliSense](/visualstudio/ide/using-intellisense). Le balisage suivant est un exemple de composant de boîte de dialogue personnalisée de base dans un fichier Razor (*DialogComponent.cshtml*) :

```cshtml
<div>
    <h2>@Title</h2>
    @BodyContent
    <button onclick=@OnOK>OK</button>
</div>

@functions {
    public string Title { get; set; }
    public RenderFragment BodyContent { get; set; }
    public Action OnOK { get; set; }
}
```

Quand ce composant est utilisé ailleurs dans l’application, IntelliSense accélère le développement avec la complétion de syntaxe et de paramètres.

Les composants peuvent être :

* Imbriqués.
* Créés avec du code C# (*\*.cs*) ou Razor (*\*.cshtml*).
* Partagés par le biais de bibliothèques de classes.
* Soumis à des tests unitaires sans nécessiter de modèle DOM de navigateur.

## <a name="infrastructure"></a>Infrastructure

Razor Components offre les fonctions de base nécessaires à la plupart des applications, notamment :

* Dispositions
* Routage
* Injection de dépendances

Toutes ces fonctionnalités sont facultatives. Quand l’une de ces fonctionnalités n’est pas utilisée dans une application, l’implémentation est supprimée de l’application lors de la publication par l’[éditeur de liens de langage intermédiaire (IL)](xref:host-and-deploy/razor-components/configure-linker).

## <a name="code-sharing-and-net-standard"></a>Partage de code et .NET Standard

Les applications peuvent référencer et utiliser les bibliothèques [.NET Standard](/dotnet/standard/net-standard) existantes. .NET Standard est une spécification formelle d’API .NET qui sont communes aux implémentations .NET. .NET Standard 2.0 ou ultérieur est pris en charge. Les API qui ne sont pas applicables à l’intérieur d’un navigateur web (par exemple l’accès au système de fichiers, l’ouverture d’un socket, le threading et d’autres fonctionnalités) lèvent [PlatformNotSupportedException](/dotnet/api/system.platformnotsupportedexception). Les bibliothèques de classes .NET Standard peuvent être partagées entre le code serveur et dans les applications basées sur le navigateur.

## <a name="javascript-interop"></a>Interopérabilité JavaScript

Pour les applications qui nécessitent des API de navigateur et des bibliothèques JavaScript tierces, WebAssembly est conçu pour une interopérabilité avec JavaScript. Razor Components est capable d’utiliser les mêmes API ou bibliothèques que JavaScript. Le code C# peut appeler du code JavaScript, et le code JavaScript peut appeler du code C#. Pour plus d’informations, consultez [Interopérabilité JavaScript](xref:razor-components/javascript-interop).

## <a name="optimization"></a>Optimisation

Pour les applications côté client, la taille de la charge utile est critique. Blazor optimise la taille de la charge utile pour réduire les durées de téléchargement. Par exemple, les parties inutilisées des assemblys .NET sont supprimées pendant le processus de génération, les réponses HTTP sont compressées, et les assemblys et le runtime .NET sont mis en cache dans le navigateur.

Razor Components fournit une taille de charge utile encore plus petite que Blazor en conservant les assemblys .NET, l’assembly de l’application et le runtime côté serveur. Les applications Razor Components fournissent uniquement le balisage, les scripts et les feuilles de style aux clients.

## <a name="deployment"></a>Déploiement

Utilisez Blazor pour générer une application côté client autonome pure ou une application ASP.NET Core à pile complète qui contient à la fois des applications serveur et client :

* Dans une **application côté client autonome**, l’application Blazor est compilée dans un dossier *dist* qui contient uniquement des fichiers statiques. Les fichiers peuvent être hébergés sur Azure App Service, des pages GitHub, IIS (configuré en tant que serveur de fichiers statiques), des serveurs Node.js et de nombreux autres serveurs et services. .NET n’est pas nécessaire sur le serveur de production.
* Dans une **application ASP.NET Core à pile complète**, le code peut être partagé par les applications cliente et serveur. L’application ASP.NET Core Razor Components obtenue, qui fournit l’interface utilisateur côté client et d’autres points de terminaison d’API côté serveur, peut être générée et déployée sur n’importe quel hôte cloud ou local pris en charge par ASP.NET Core.

## <a name="suggest-a-feature-or-file-a-bug-report"></a>Suggérer une fonctionnalité ou soumettre un rapport de bogue

Pour effectuer des suggestions et soumettre des rapports de bogues, veuillez [signaler un problème](https://github.com/aspnet/AspNetCore/issues/new). Pour obtenir une aide générale et obtenir des réponses de la Communauté, rejoignez la conversation sur [Gitter](https://gitter.im/aspnet/Blazor).

## <a name="additional-resources"></a>Ressources supplémentaires

* [WebAssembly](http://webassembly.org/)
* [Guide C#](/dotnet/csharp/)
* [Razor](/aspnet/core/mvc/views/razor)
* [HTML](https://www.w3.org/html/)
