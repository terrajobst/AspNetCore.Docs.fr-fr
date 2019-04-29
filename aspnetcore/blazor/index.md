---
title: Introduction à Blazor dans ASP.NET Core
author: guardrex
description: Explorez ASP.NET Core Blazor, un moyen de créer une interface utilisateur web interactive côté client à l’aide de .NET dans une application ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: seoapril2019
ms.date: 04/18/2019
uid: blazor/index
ms.openlocfilehash: 74eeb003c249fc9ff8267ac855455f875806ccd9
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982995"
---
# <a name="introduction-to-blazor"></a>Introduction à Blazor

Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)

Bienvenue dans Blazor !

Créez une interface utilisateur web interactive côté client à l’aide de .NET :

* Créez des interfaces utilisateur interactives riches à l’aide de C# plutôt que JavaScript.
* Partagez la logique d’application côté serveur et côté client écrite avec .NET.
* Affichez l’interface utilisateur en langage HTML et CSS pour une large prise en charge des navigateurs, y compris les navigateurs mobiles.

Blazor prend en charge les principaux scénarios que nécessitent la plupart des applications :

* Paramètres
* Gestion des événements
* Liaison de données
* Routage
* Injection de dépendances
* Dispositions
* Modèles
* Valeurs en cascade

## <a name="components"></a>Composants

Dans Blazor, un *composant* est un élément d’interface utilisateur, comme une page, une boîte de dialogue ou un formulaire de saisie de données. Les composants gèrent les événements de l’utilisateur et définissent une logique de rendu de l’interface utilisateur flexible. Les composants peuvent être imbriqués et réutilisés.

Les composants sont des classes .NET intégrées dans des assemblys .NET pouvant être partagées et distribuées comme packages NuGet. La classe de composant est généralement écrite sous la forme d’une page de balisage Razor avec une extension de fichier *.razor*. Les composants dans Blazor sont parfois appelés composants Razor.

[Razor](xref:mvc/views/razor) est une syntaxe pour la combinaison de balisage HTML et de code C#. Razor accroît la productivité des développeurs en leur permettant de basculer entre le balisage et le code C# dans le même fichier avec prise en charge d’[IntelliSense](/visualstudio/ide/using-intellisense). Razor Pages et les vues MVC utilisent également Razor. Contrairement à Razor Pages et aux vues MVC, qui reposent sur un modèle requête/réponse, les composants sont utilisés spécifiquement pour gérer la composition de l’interface utilisateur. Les composants Razor peuvent être utilisés spécifiquement pour la composition et la logique d’interface utilisateur côté client.

Le balisage suivant est un exemple de composant de boîte de dialogue personnalisée :

```cshtml
<div>
    <h2>@Title</h2>
    @BodyContent
    <button onclick="@OnOK">OK</button>
</div>

@functions {
    public string Title { get; set; }
    public RenderFragment BodyContent { get; set; }
    public Action OnOK { get; set; }
}
```

Quand ce composant est utilisé ailleurs dans l’application, IntelliSense dans [Visual Studio](https://visualstudio.microsoft.com/vs/) accélère le développement avec la complétion de syntaxe et de paramètres.

Les composants s’affichent dans une représentation en mémoire du navigateur DOM appelé *arborescence d’affichage*. Elle peut ensuite être utilisée pour mettre à jour l’interface utilisateur de manière flexible et efficace.

## <a name="blazor-server-side"></a>Blazor côté serveur

Blazor dissocie la logique de rendu de composant de la manière dont les mises à jour de l’interface utilisateur sont appliquées. Blazor côté serveur prend en charge l’hébergement des composants Razor sur le serveur dans une application ASP.NET Core. Les mises à jour de l’interface utilisateur sont gérées par le biais d’une connexion SignalR.

L’exécution :

* Gère l’envoi des événements d’interface utilisateur depuis le navigateur vers le serveur.
* Applique les mises à jour de l’interface utilisateur envoyées par le serveur au navigateur après avoir exécuté les composants.

La connexion utilisée par Blazor côté serveur pour communiquer avec le navigateur est aussi utilisée pour gérer les appels d’interopérabilité JavaScript.

![Blazor côté serveur exécute le code .NET sur le serveur et interagit avec le modèle DOM (Document Object Model) sur le client par le biais d’une connexion SignalR.](index/_static/blazor-server-side.png)

Pour plus d'informations, consultez <xref:blazor/hosting-models#server-side>.

## <a name="blazor-client-side"></a>Blazor côté client

Blazor côté client est un framework d’application monopage pour la création d’applications web interactives côté client avec .NET. Blazor côté client utilise des standards web ouverts sans transpilation de plug-ins ou de code. Blazor côté client fonctionne dans tous les navigateurs web modernes, y compris les navigateurs mobiles.

L’utilisation de .NET dans le navigateur pour le développement web côté client offre de nombreux avantages :

* **Langage C#** : Écriture de code dans C# plutôt que dans JavaScript.
* **Écosystème .NET** : Tirez parti de l’écosystème existant des bibliothèques .NET.
* **Développement de l’empilement complet** : Partager la logique côté client et côté serveur.
* **Rapidité et scalabilité** : .NET a été conçu pour la performance, la fiabilité et la sécurité.
* **Outils de pointe** : Restez productif grâce à Visual Studio sur Windows, Linux et macOS.
* **Stabilité et cohérence** :  Développez avec un ensemble commun de langages, de frameworks et d’outils stables, riches en fonctionnalités et faciles à utiliser.

L’exécution de code .NET à l’intérieur des navigateurs web est rendue possible grâce à [WebAssembly](http://webassembly.org) (abrégé en *wasm*). WebAssembly est un standard web ouvert pris en charge dans les navigateurs web sans plug-in. WebAssembly est un format bytecode compact optimisé pour un téléchargement rapide et une vitesse d’exécution maximale.

Le code WebAssembly peut accéder à toutes les fonctionnalités du navigateur par le biais de l’interopérabilité JavaScript. Parallèlement, le code .NET exécuté via WebAssembly s’exécute dans le même bac à sable approuvé JavaScript, afin de prévenir toute action malveillante sur l’ordinateur client.

![Blazor côté client exécute du code .NET dans le navigateur avec WebAssembly.](index/_static/blazor-client-side.png)

Quand une application Blazor côté client est créée et exécutée dans un navigateur :

* Les fichiers de code C# et les fichiers Razor sont compilés dans des assemblys .NET.
* Les assemblys et le runtime .NET sont téléchargés dans le navigateur.
* Blazor côté client amorce le runtime .NET et configure le runtime pour charger les assemblys de l’application. La manipulation d’objets DOM (Document Object Model) et les appels d’API du navigateur sont gérés par le runtime Blazor côté client avec l’interopérabilité de JavaScript.

Pour réduire la taille du code inutilisé extrait de l’application téléchargée lorsqu’elle est publiée par l’[éditeur de liens de langage intermédiaire (IL)](xref:host-and-deploy/blazor/configure-linker).

Blazor côté client est un modèle d’hébergement côté client. Étant donné que Blazor sépare la logique de rendu d’un composant de la façon dont les mises à jour des interfaces utilisateurs sont appliquées, vous disposez d’une flexibilité dans la manière dont Blazor peut être hébergé. Utilisez [Blazor côté serveur](#blazor-server-side) pour héberger Blazor sur le serveur dans une application ASP.NET Core où les mises à jour de l’interface utilisateur sont gérées via une connexion SignalR. Pour plus d'informations, consultez <xref:blazor/hosting-models#server-side>. 

La taille de la charge utile est un facteur de performance essentiel pour qu’une application soit facile d’utilisation. Blazor côté client optimise la taille de la charge utile pour réduire les temps de téléchargement :

* Les parties inutilisées des assemblys .NET sont supprimées pendant le processus de création.
* Réponses HTTP compressées.
* Le runtime .NET et les assemblys sont mis en cache dans le navigateur.

[Blazor côté serveur](#blazor-server-side) offre une plus petite taille de charge utile que Blazor côté client en conservant les assemblys .NET, l’assembly de l’application et le runtime côté serveur. Les applications Blazor côté serveur servent uniquement les fichiers de balisage et les ressources statiques aux clients.

## <a name="javascript-interop"></a>Interopérabilité JavaScript

Pour les applications qui nécessitent des bibliothèques JavaScript tierces et des API de navigateur, les composants interagissent avec JavaScript. Les composants peuvent utiliser les mêmes API ou bibliothèques que JavaScript. Le code C# peut appeler du code JavaScript, et le code JavaScript peut appeler du code C#. Pour plus d’informations, consultez [Interopérabilité JavaScript](xref:blazor/javascript-interop).

## <a name="code-sharing-and-net-standard"></a>Partage de code et .NET Standard

Les applications peuvent référencer et utiliser les bibliothèques [.NET Standard](/dotnet/standard/net-standard) existantes. .NET Standard est une spécification formelle d’API .NET qui sont communes aux implémentations .NET. Blazor implémente également .NET Standard 2.0. Les API qui ne sont pas applicables à l’intérieur d’un navigateur web (par exemple l’accès au système de fichiers, l’ouverture d’un socket, le threading et d’autres fonctionnalités) lèvent <xref:System.PlatformNotSupportedException>. Vous pouvez partager les bibliothèques de classes .NET Standard entre les différentes plateformes .NET, comme Blazor, .NET Framework, .NET Core, Xamarin, Mono et Unity.

## <a name="additional-resources"></a>Ressources supplémentaires

* [WebAssembly](http://webassembly.org/)
* [Guide C#](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
