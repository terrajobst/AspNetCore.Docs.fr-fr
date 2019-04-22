---
title: Introduction à Blazor dans ASP.NET Core
author: guardrex
description: Explorez ASP.NET Core Blazor, un moyen de créer une interface utilisateur web interactive côté client à l’aide de .NET dans une application ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: seoapril2019
ms.date: 04/15/2019
uid: blazor/index
ms.openlocfilehash: a5b12a5c5c10a74ab192d0d67a2ba9a5617232c7
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614571"
---
# <a name="introduction-to-blazor"></a>Introduction à Blazor

Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)

## <a name="razor-components"></a>Composants Razor

Le modèle d’hébergement côté serveur de Blazor, *Razor Components*, constitue un moyen de créer une interface utilisateur web interactive côté client à l’aide de .NET :

* Créez des interfaces utilisateur interactives riches à l’aide de C# plutôt que JavaScript.
* Partagez les logiques d’applications côté serveur et côté client écrites avec .NET.
* Affichez l’interface utilisateur en langage HTML et CSS pour une large prise en charge des navigateurs, y compris les navigateurs mobiles.

Razor Components prend en charge les principales installations que nécessitent la plupart des applications, y compris :

* Paramètres
* Gestion des événements
* Liaison de données
* Routage
* Injection de dépendances
* Dispositions
* Création de modèles
* Valeurs en cascade

Razor Components dissocie la logique de rendu de composant de la manière dont les mises à jour de l’interface utilisateur sont appliquées. ASP.NET Core Razor Components dans .NET Core 3.0 ajoute la prise en charge de l’hébergement de Razor Components sur le serveur dans une application ASP.NET Core. Les mises à jour de l’interface utilisateur sont gérées par le biais d’une connexion SignalR.

L’exécution :

* Gère l’envoi des événements d’interface utilisateur depuis le navigateur vers le serveur.
* Applique les mises à jour de l’interface utilisateur envoyées par le serveur au navigateur après avoir exécuté les composants.

La connexion utilisée par Razor Components pour communiquer avec le navigateur est aussi utilisée pour gérer les appels d’interopérabilité JavaScript.

![Razor Components exécute le code .NET sur le serveur et interagit avec le modèle DOM (Document Object Model) sur le client par le biais d’une connexion SignalR](index/_static/aspnet-core-razor-components.png)

Pour plus d'informations, consultez <xref:blazor/hosting-models#server-side-hosting-model>.

## <a name="components"></a>Composants

Un *composant Razor* est un élément d’interface utilisateur, comme une page, une boîte de dialogue ou un formulaire de saisie de données. Les composants gèrent les événements de l’utilisateur et définissent une logique de rendu de l’interface utilisateur flexible. Les composants peuvent être imbriqués et réutilisés.

Les composants sont des classes .NET intégrées dans des assemblys .NET pouvant être partagées et distribuées comme packages NuGet. La classe est normalement écrite sous la forme d’une page de balisage Razor avec une extension de fichier *.razor* (Razor Components) ou d’une page de balisage Razor avec une extension de fichier *.cshtml* (Blazor).

[Razor](xref:mvc/views/razor) est une syntaxe pour la combinaison de balisage HTML et de code C#. Razor accroît la productivité des développeurs en leur permettant de basculer entre le balisage et le code C# dans le même fichier avec prise en charge d’[IntelliSense](/visualstudio/ide/using-intellisense). Razor Pages et les vues MVC utilisent également Razor. Contrairement à Razor Pages et aux vues MVC, qui reposent sur un modèle requête/réponse, les composants sont utilisés spécifiquement pour gérer la composition de l’interface utilisateur. Razor Components peut être utilisé spécifiquement pour la composition et la logique d’interface utilisateur côté client.

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

Quand ce composant est utilisé ailleurs dans l’application, IntelliSense accélère le développement avec la complétion de syntaxe et de paramètres.

Les composants s’affichent dans une représentation en mémoire du navigateur DOM appelé *arborescence d’affichage*. Elle peut ensuite être utilisée pour mettre à jour l’interface utilisateur de manière flexible et efficace.

## <a name="javascript-interop"></a>Interopérabilité JavaScript

Pour les applications qui nécessitent des bibliothèques JavaScript tierces et des API de navigateur, les composants interagissent avec JavaScript. Les composants peuvent utiliser les mêmes API ou bibliothèques que JavaScript. Le code C# peut appeler du code JavaScript, et le code JavaScript peut appeler du code C#. Pour plus d’informations, consultez [Interopérabilité JavaScript](xref:blazor/javascript-interop).

## <a name="code-sharing-and-net-standard"></a>Partage de code et .NET Standard

Les applications peuvent référencer et utiliser les bibliothèques [.NET Standard](/dotnet/standard/net-standard) existantes. .NET Standard est une spécification formelle d’API .NET qui sont communes aux implémentations .NET. Blazor implémente également .NET Standard 2.0. Les API qui ne sont pas applicables à l’intérieur d’un navigateur web (par exemple l’accès au système de fichiers, l’ouverture d’un socket, le threading et d’autres fonctionnalités) lèvent <xref:System.PlatformNotSupportedException>. Les bibliothèques de classes .NET Standard peuvent être partagées entre les différentes plateformes .NET, notamment Blazor, .NET Framework, .NET Core, Xamarin, Mono et Unity.

## <a name="blazor"></a>Blazor

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

Pour réduire la taille du code inutilisé extrait de l’application téléchargée lorsqu’elle est publiée par l’[éditeur de liens de langage intermédiaire (IL)](xref:host-and-deploy/blazor/configure-linker).

Blazor côté client est un modèle d’hébergement côté client. Étant donné que Blazor sépare la logique de rendu d’un composant de la façon dont les mises à jour des interfaces utilisateurs sont appliquées, vous disposez d’une flexibilité dans la manière dont Blazor peut être hébergé. Utilisez ASP.NET Core [Razor Components](#razor-components) pour héberger Razor Components sur le serveur dans une application ASP.NET Core où les mises à jour de l’interface utilisateur sont gérées par le biais d’une connexion SignalR. Pour plus d'informations, consultez <xref:blazor/hosting-models#server-side-hosting-model>. 

La taille de la charge utile est un facteur de performance essentiel pour qu’une application soit facile d’utilisation. Blazor optimise la taille de la charge utile pour réduire les temps de téléchargement :

* Les parties inutilisées des assemblys .NET sont supprimées pendant le processus de création.
* Réponses HTTP compressées.
* Le runtime .NET et les assemblys sont mis en cache dans le navigateur.

[Razor Components](#razor-components) fournit une taille de charge utile plus petite que Blazor en conservant les assemblys .NET, l’assembly de l’application et le runtime côté serveur. Les applications Razor Components fournissent uniquement les fichiers de balisage et les ressources statiques aux clients.

## <a name="additional-resources"></a>Ressources supplémentaires

* [WebAssembly](http://webassembly.org/)
* [Guide C#](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
