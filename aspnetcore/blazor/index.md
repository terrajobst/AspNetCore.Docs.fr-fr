---
title: Introduction à Blazor dans ASP.NET Core
author: guardrex
description: Explorez ASP.NET Core Blazor, un moyen de créer une interface utilisateur web interactive côté client à l’aide de .NET dans une application ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc, seoapril2019
ms.date: 07/01/2019
uid: blazor/index
ms.openlocfilehash: d91ba4fd5ada714a539375715745241f05e9fc70
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67813369"
---
# <a name="introduction-to-blazor"></a>Introduction à Blazor

Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)

*Bienvenue dans Blazor !*

Blazor est une infrastructure pour la création d’interfaces utilisateur d’applications web interactives côté client avec .NET :

* Créez des interfaces utilisateur interactives riches à l’aide de C# plutôt que JavaScript.
* Partagez la logique d’application côté serveur et côté client écrite dans .NET.
* Affichez l’interface utilisateur en langage HTML et CSS pour une large prise en charge des navigateurs, y compris les navigateurs mobiles.

L’utilisation de .NET dans le développement web côté client offre les avantages suivants :

* Écriture de code dans C# plutôt que dans JavaScript.
* Tirez parti de l’écosystème .NET existant des bibliothèques .NET.
* Partagez la logique de l’application sur le serveur et le client.
* Bénéficiez des performances, de la fiabilité et de la sécurité de .NET.
* Restez productif grâce à Visual Studio sur Windows, Linux et macOS.
* Développez avec un ensemble commun de langages, de frameworks et d’outils stables, riches en fonctionnalités et faciles à utiliser.

## <a name="components"></a>Composants

Les applications Blazor sont basées sur des *composants*. Dans Blazor, un composant est un élément d’interface utilisateur, comme une page, une boîte de dialogue ou un formulaire de saisie de données.

Les composants sont des classes .NET intégrées dans des assemblys .NET qui :

* Définissent la logique de rendu de l’interface utilisateur flexible.
* Gèrent les événements de l’utilisateur.
* Peuvent être imbriqués et réutilisés.
* Peuvent être partagés et distribués en tant que [bibliothèques de classes Razor](xref:razor-pages/ui-class) ou [packages NuGet](/nuget/what-is-nuget).

La classe de composant est généralement écrite sous la forme d’une page de balisage [Razor](xref:mvc/views/razor) avec une extension de fichier *.razor*. Les composants dans Blazor sont appelés *composants Razor*. Razor est une syntaxe pour la combinaison de balisage HTML et de code C# conçu pour la productivité des développeurs. Razor permet de basculer entre le balisage HTML et C# dans le même fichier avec le support d’[IntelliSense](/visualstudio/ide/using-intellisense). Razor Pages et MVC utilisent également Razor. Contrairement à Razor Pages et à MVC, qui reposent sur un modèle requête/réponse, les composants sont utilisés spécifiquement pour la logique et la composition de l’interface utilisateur côté client.

Le balisage Razor suivant montre un composant (*Dialog.razor*), qui peut être imbriqué dans un autre composant :

```cshtml
<div>
    <h1>@Title</h1>

    @ChildContent

    <button @onclick="@OnYes">Yes!</button>
</div>

@code {
    [Parameter]
    private string Title { get; set; }

    [Parameter]
    private RenderFragment ChildContent { get; set; }

    private void OnYes()
    {
        Console.WriteLine("Write to the console in C#! 'Yes' button was selected.");
    }
}
```

Le contenu du corps (`ChildContent`) et le titre (`Title`) de la boîte de dialogue sont fournis par le composant qui utilise ce composant dans son interface utilisateur. `OnYes` est une méthode C# déclenchée par l’événement `onclick` du bouton.

Blazor utilise des balises HTML naturelles pour la composition de l’interface utilisateur. Les éléments HTML spécifient des composants et les attributs d’une balise passent les valeurs aux propriétés d’un composant.

Dans l’exemple suivant, le composant `Index` utilise le composant `Dialog`. `ChildContent` et `Title` sont définis par les attributs et le contenu de l’élément `<Dialog>`.

*Index.razor* :

```cshtml
@page "/"

<h1>Hello, world!</h1>

Welcome to your new app.

<Dialog Title="Blazor">
    Do you want to <i>learn more</i> about Blazor?
</Dialog>
```

La boîte de dialogue est affichée lorsque le parent (*Index.razor*) est accessible dans un navigateur :

![Composant de boîte de dialogue affiché dans le navigateur](index/_static/dialog.png)

Quand ce composant est utilisé dans l’application, IntelliSense dans [Visual Studio](/visualstudio/ide/using-intellisense)et [Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense) accélère le développement avec l’achèvement de la syntaxe et des paramètres.

Les composants s’affichent dans une représentation en mémoire du DOM (Document Object Model) du navigateur appelé *arborescence de rendu*, utilisée pour mettre à jour l’interface utilisateur de manière flexible et efficace.

## <a name="blazor-client-side"></a>Blazor côté client

Blazor côté client est un framework d’application monopage pour la création d’applications web interactives côté client avec .NET. Blazor côté client utilise les normes web ouvertes sans transpilation de plug-ins ou de codes et fonctionne dans tous les navigateurs web modernes, y compris les navigateurs mobiles.

L’exécution de code .NET à l’intérieur des navigateurs web est rendue possible grâce à [WebAssembly](http://webassembly.org) (abrégé en *wasm*). WebAssembly est un format bytecode compact optimisé pour un téléchargement rapide et une vitesse d’exécution maximale. WebAssembly est un standard web ouvert pris en charge dans les navigateurs web sans plug-in.

Le code WebAssembly peut accéder à toutes les fonctionnalités du navigateur via JavaScript, appelé *interopérabilité JavaScript* (ou *interop JavaScript*). Le code .NET exécuté via WebAssembly dans le navigateur s’exécute dans le bac à sable JavaScript du navigateur avec les protections offertes par le bac à sable contre les actions malveillantes sur l’ordinateur client.

![Blazor côté client exécute du code .NET dans le navigateur avec WebAssembly.](index/_static/blazor-client-side.png)

Quand une application Blazor côté client est créée et exécutée dans un navigateur :

* Les fichiers de code C# et les fichiers Razor sont compilés dans des assemblys .NET.
* Les assemblys et le runtime .NET sont téléchargés dans le navigateur.
* Blazor côté client amorce le runtime .NET et configure le runtime pour charger les assemblys de l’application. Le runtime Blazor côté client utilise l’interopérabilité de JavaScript pour traiter la manipulation DOM et les appels d’API du navigateur.

La taille de l’application publiée, sa *taille de charge utile*, est un facteur de performance essentiel pour qu’une application soit facile à utiliser. Le téléchargement d’une application volumineuse dans un navigateur prend un certain temps, ce qui nuit à l’expérience utilisateur. Blazor côté client optimise la taille de la charge utile pour réduire les temps de téléchargement :

* Du code inutilisé est extrait de l’application lorsqu’elle est publiée par l’[éditeur de liens de langage intermédiaire (IL)](xref:host-and-deploy/blazor/configure-linker).
* Réponses HTTP compressées.
* Le runtime .NET et les assemblys sont mis en cache dans le navigateur.

## <a name="blazor-server-side"></a>Blazor côté serveur

Blazor dissocie la logique de rendu de composant de la manière dont les mises à jour de l’interface utilisateur sont appliquées. Blazor côté serveur prend en charge l’hébergement des composants Razor sur le serveur dans une application ASP.NET Core. Les mises à jour de l’interface utilisateur sont gérées par le biais d’une connexion [SignalR](xref:signalr/introduction).

Le runtime gère l’envoi des événements d’interface utilisateur du navigateur au serveur et applique les mises à jour de l’interface utilisateur renvoyées par le serveur dans le navigateur après avoir exécuté les composants.

La connexion utilisée par Blazor côté serveur pour communiquer avec le navigateur est aussi utilisée pour gérer les appels d’interopérabilité JavaScript.

![Blazor côté serveur exécute le code .NET sur le serveur et interagit avec le modèle DOM (Document Object Model) sur le client par le biais d’une connexion SignalR.](index/_static/blazor-server-side.png)

## <a name="javascript-interop"></a>Interopérabilité JavaScript

Pour les applications qui nécessitent des bibliothèques JavaScript tierces et l’accès à des API de navigateur, les composants interagissent avec JavaScript. Les composants peuvent utiliser les mêmes API ou bibliothèques que JavaScript. Le code C# peut appeler du code JavaScript, et le code JavaScript peut appeler du code C#. Pour plus d’informations, consultez <xref:blazor/javascript-interop>.

## <a name="code-sharing-and-net-standard"></a>Partage de code et .NET Standard

Blazor implémente également [.NET Standard 2.0](/dotnet/standard/net-standard). .NET Standard est une spécification formelle d’API .NET qui sont communes aux implémentations .NET. Vous pouvez partager les bibliothèques de classes .NET Standard entre les différentes plateformes .NET, comme Blazor, .NET Framework, .NET Core, Xamarin, Mono et Unity.

Les API qui ne sont pas applicables à l’intérieur d’un navigateur web (par exemple l’accès au système de fichiers, l’ouverture d’un socket et le threading) lèvent une <xref:System.PlatformNotSupportedException>.

## <a name="additional-resources"></a>Ressources supplémentaires

* [WebAssembly](https://webassembly.org/)
* <xref:blazor/hosting-models>
* [Guide C#](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
