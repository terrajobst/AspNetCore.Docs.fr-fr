---
title: Introduction à Blazor
author: guardrex
description: Explorez ASP.NET Core Blazor, une nouvelle méthode pour créer des applications interactives côté client à l’aide de .NET s’exécutant dans le navigateur avec WebAssembly.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/28/2019
uid: spa/blazor/index
ms.openlocfilehash: 87e294a975e4168add046166a95e02e485db131d
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068246"
---
# <a name="introduction-to-blazor"></a>Introduction à Blazor

Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Blazor est une infrastructure d’application monopage pour la création d’applications web interactives côté client avec .NET. Blazor utilise les normes Web ouvertes sans plug-ins ou transpilation de code. Blazor fonctionne dans tous les navigateurs web modernes, notamment les navigateurs mobiles.

L’utilisation de .NET dans le navigateur pour le développement web côté client offre de nombreux avantages :

* **Langage C#** : Écriture de code dans C# plutôt que dans JavaScript.
* **Écosystème .NET** : Tirez parti de l’écosystème existant des bibliothèques .NET.
* **Développement de l’empilement complet** : Partager la logique côté client et côté serveur.
* **Rapidité et scalabilité** : .NET a été conçu pour la performance, la fiabilité et la sécurité.
* **Outils de pointe** : Restez productif grâce à Visual Studio sur Windows, Linux et macOS.
* **Stabilité et cohérence** :  Conçu sur un ensemble de langages, d’infrastructures et des outils stables, aux caractéristiques riches et faciles d’utilisation.

L’exécution de code .NET à l’intérieur des navigateurs web est rendue possible grâce à [WebAssembly](http://webassembly.org) (abrégé en *wasm*). WebAssembly est un standard web ouvert pris en charge dans les navigateurs web sans plug-in. WebAssembly est un format bytecode compact optimisé pour un téléchargement rapide et une vitesse d’exécution maximale.

Le code WebAssembly peut accéder à toutes les fonctionnalités du navigateur par le biais de l’interopérabilité JavaScript. Parallèlement, le code .NET exécuté via WebAssembly s’exécute dans le même bac à sable approuvé JavaScript, afin de prévenir toute action malveillante sur l’ordinateur client.

![Blazor exécute le code .NET dans le navigateur avec WebAssembly.](index/_static/blazor.png)

Quand une application Blazor est créée et s’exécute dans un navigateur :

* Les fichiers de code C# et les fichiers Razor sont compilés dans des assemblys .NET.
* Les assemblys et le runtime .NET sont téléchargés dans le navigateur.
* Blazor démarre le runtime .NET et le configure pour charger les assemblys de l’application. La manipulation du modèle DOM et les appels d’API de navigateur sont gérés par le runtime Blazor par le biais de l’interopérabilité JavaScript.

Blazor prend en charge les principales installations que nécessitent la plupart des applications, y compris :

* Paramètres
* Gestion des événements
* Liaison de données
* Routage
* Injection de dépendances
* Dispositions
* Création de modèles
* Valeurs en cascade

Pour réduire la taille du code inutilisé extrait de l’application téléchargée lorsqu’elle est publiée par l’[éditeur de liens de langage intermédiaire (IL)](xref:host-and-deploy/razor-components-blazor/configure-linker).

Blazor est le modèle d’hébergement côté client pour Razor Components. Étant donné que Razor Components sépare la logique de rendu d’un composant de la façon dont les mises à jour des interfaces utilisateurs sont appliquées, vous disposez d’une flexibilité dans la manière dont Razor Components peut être hébergé. Utilisez ASP.NET Core Razor Components pour héberger Razor Components sur le serveur dans une application ASP.NET Core où les mises à jour de l’interface utilisateur sont gérées par le biais d’une connexion SignalR. Pour plus d’informations, consultez <xref:razor-components/index> et <xref:razor-components/hosting-models#server-side-hosting-model>. 

## <a name="components"></a>Composants

Un *composant Razor* est un élément d’interface utilisateur, comme une page, une boîte de dialogue ou un formulaire de saisie de données. Les composants gèrent les événements de l’utilisateur et définissent une logique de rendu de l’interface utilisateur flexible. Les composants peuvent être imbriqués et réutilisés.

Les composants sont des classes .NET intégrées dans des assemblys .NET pouvant être partagées et distribuées comme packages NuGet. La classe peut être écrite sous la forme d’une page de balisage Razor (*.cshtml*), ou en tant que classe C# (*.cs*).

[Razor](xref:mvc/views/razor) est une syntaxe pour la combinaison de balisage HTML et de code C#. Razor accroît la productivité des développeurs en leur permettant de basculer entre le balisage et le code C# dans le même fichier avec prise en charge d’[IntelliSense](/visualstudio/ide/using-intellisense). Razor Pages et les vues MVC utilisent également Razor. Contrairement à Razor Pages et aux vues MVC, qui reposent sur un modèle requête/réponse, les composants sont utilisés spécifiquement pour gérer la composition de l’interface utilisateur. Razor Components peut être utilisé spécifiquement pour la composition et la logique d’interface utilisateur côté client.

Le balisage suivant est un exemple de composant de boîte de dialogue personnalisée dans un fichier Razor (*DialogComponent.cshtml*) :

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

Les composants s’affichent dans une représentation en mémoire du navigateur DOM appelé *arborescence d’affichage*. Elle peut ensuite être utilisée pour mettre à jour l’interface utilisateur de manière flexible et efficace.

## <a name="javascript-interop"></a>Interopérabilité JavaScript

Pour les applications qui nécessitent des bibliothèques JavaScript tierces et des API de navigateur, Blazor interagit avec JavaScript. Les composants peuvent utiliser les mêmes API ou bibliothèques que JavaScript. Le code C# peut appeler du code JavaScript, et le code JavaScript peut appeler du code C#. Pour plus d’informations, consultez [Interopérabilité JavaScript](xref:razor-components/javascript-interop).

## <a name="code-sharing-and-net-standard"></a>Partage de code et .NET Standard

Les applications peuvent référencer et utiliser les bibliothèques [.NET Standard](/dotnet/standard/net-standard) existantes. .NET Standard est une spécification formelle d’API .NET qui sont communes aux implémentations .NET. Blazor implémente également .NET Standard 2.0. Les API qui ne sont pas applicables à l’intérieur d’un navigateur web (par exemple l’accès au système de fichiers, l’ouverture d’un socket, le threading et d’autres fonctionnalités) lèvent <xref:System.PlatformNotSupportedException>. Les bibliothèques de classes .NET Standard peuvent être partagées entre les différentes plateformes .NET, notamment Blazor, .NET Framework, .NET Core, Xamarin, Mono et Unity.

## <a name="optimization"></a>Optimisation

La taille de la charge utile est un facteur de performance essentiel pour qu’une application soit facile d’utilisation. Blazor optimise la taille de la charge utile pour réduire les temps de téléchargement :

* Les parties inutilisées des assemblys .NET sont supprimées pendant le processus de création.
* Réponses HTTP compressées.
* Le runtime .NET et les assemblys sont mis en cache dans le navigateur.

[Razor Components côté serveur](xref:razor-components/index) fournit une taille de charge utile encore plus petite que Blazor en conservant les assemblys .NET, l’assembly de l’application et le runtime côté serveur. Les applications Razor Components fournissent uniquement les fichiers de balisage et les ressources statiques aux clients.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:razor-components/index>
* [WebAssembly](http://webassembly.org/)
* [Guide C#](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
