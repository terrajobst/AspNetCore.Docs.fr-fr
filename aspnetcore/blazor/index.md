---
title: Présentation de ASP.NET Core Blazor
author: guardrex
description: Explorez ASP.NET Core Blazor, un moyen de créer une interface utilisateur Web interactive côté client avec .NET dans une application ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc, seoapril2019
ms.date: 11/12/2019
no-loc:
- Blazor
- SignalR
uid: blazor/index
ms.openlocfilehash: 8b656a7461c78475432722540ad628258cfe19c4
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73962912"
---
# <a name="introduction-to-aspnet-core-opno-locblazor"></a>Présentation de ASP.NET Core Blazor

Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)

*Bienvenue dans Blazor!*

Blazor est une infrastructure pour la création d’une interface utilisateur Web interactive côté client avec .NET :

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

les applications Blazor sont basées sur des *composants*. Un composant de Blazor est un élément de l’interface utilisateur, tel qu’une page, une boîte de dialogue ou un formulaire de saisie de données.

Les composants sont des classes .NET intégrées dans des assemblys .NET qui :

* Définissent la logique de rendu de l’interface utilisateur flexible.
* Gèrent les événements de l’utilisateur.
* Peuvent être imbriqués et réutilisés.
* Peuvent être partagés et distribués en tant que [bibliothèques de classes Razor](xref:razor-pages/ui-class) ou [packages NuGet](/nuget/what-is-nuget).

La classe de composant est généralement écrite sous la forme d’une page de balisage [Razor](xref:mvc/views/razor) avec une extension de fichier *.razor*. Les composants de Blazor sont officiellement désignés sous le terme de « *composants Razor*». Razor est une syntaxe pour la combinaison de balisage HTML et de code C# conçu pour la productivité des développeurs. Razor permet de basculer entre le balisage HTML et C# dans le même fichier avec le support d’[IntelliSense](/visualstudio/ide/using-intellisense). Razor Pages et MVC utilisent également Razor. Contrairement à Razor Pages et à MVC, qui reposent sur un modèle requête/réponse, les composants sont utilisés spécifiquement pour la logique et la composition de l’interface utilisateur côté client.

Le balisage Razor suivant montre un composant (*Dialog.razor*), qui peut être imbriqué dans un autre composant :

```cshtml
<div>
    <h1>@Title</h1>

    @ChildContent

    <button @onclick="OnYes">Yes!</button>
</div>

@code {
    [Parameter]
    public string Title { get; set; }

    [Parameter]
    public RenderFragment ChildContent { get; set; }

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

## <a name="opno-locblazor-webassembly"></a>Blazor webassembly

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor webassembly est une infrastructure d’application à page unique pour la création d’applications Web interactives côté client avec .NET. Blazor webassembly utilise des normes Web ouvertes sans plug-ins ni code transpilation et fonctionne dans tous les navigateurs Web modernes, y compris les navigateurs mobiles.

L’exécution de code .NET à l’intérieur des navigateurs web est rendue possible grâce à [WebAssembly](https://webassembly.org) (abrégé en *wasm*). WebAssembly est un format bytecode compact optimisé pour un téléchargement rapide et une vitesse d’exécution maximale. WebAssembly est un standard web ouvert pris en charge dans les navigateurs web sans plug-in.

Le code WebAssembly peut accéder à toutes les fonctionnalités du navigateur via JavaScript, appelé *interopérabilité JavaScript* (ou *interop JavaScript*). Le code .NET exécuté via WebAssembly dans le navigateur s’exécute dans le bac à sable JavaScript du navigateur avec les protections offertes par le bac à sable contre les actions malveillantes sur l’ordinateur client.

![[! Opérationnel. NO-LOC (éblouissant)] webassembly exécute du code .NET dans le navigateur avec webassembly.](index/_static/blazor-webassembly.png)

Quand une application Blazor webassembly est générée et exécutée dans un navigateur :

* Les fichiers de code C# et les fichiers Razor sont compilés dans des assemblys .NET.
* Les assemblys et le runtime .NET sont téléchargés dans le navigateur.
* Blazor webassembly amorce le Runtime .NET et configure le runtime pour charger les assemblys de l’application. Le runtime webassembly Blazor utilise l’interopérabilité JavaScript pour gérer les appels de l’API de manipulation et du navigateur DOM.

La taille de l’application publiée, sa *taille de charge utile*, est un facteur de performance essentiel pour qu’une application soit facile à utiliser. Le téléchargement d’une application volumineuse dans un navigateur prend un certain temps, ce qui nuit à l’expérience utilisateur. Blazor webassembly optimise la taille de la charge utile pour réduire les temps de téléchargement :

* Du code inutilisé est extrait de l’application lorsqu’elle est publiée par l’[éditeur de liens de langage intermédiaire (IL)](xref:host-and-deploy/blazor/configure-linker).
* Réponses HTTP compressées.
* Le runtime .NET et les assemblys sont mis en cache dans le navigateur.

## <a name="opno-locblazor-server"></a>Serveur de Blazor

Blazor dissocie la logique de rendu des composants de l’application des mises à jour de l’interface utilisateur. Blazor Server prend en charge l’hébergement de composants Razor sur le serveur dans une application ASP.NET Core. Les mises à jour de l’interface utilisateur sont gérées via une connexion [SignalR](xref:signalr/introduction) .

Le runtime gère l’envoi des événements d’interface utilisateur du navigateur au serveur et applique les mises à jour de l’interface utilisateur renvoyées par le serveur dans le navigateur après avoir exécuté les composants.

La connexion utilisée par Blazor serveur pour communiquer avec le navigateur est également utilisée pour gérer les appels Interop JavaScript.

![[! Opérationnel. NO-LOC (éblouissant)] le serveur exécute le code .NET sur le serveur et interagit avec le Document Object Model sur le client via un [ ! Opérationnel. Connexion NO-LOC (Signalr)]](index/_static/blazor-server.png)

## <a name="javascript-interop"></a>Interopérabilité de JavaScript

Pour les applications qui nécessitent des bibliothèques JavaScript tierces et l’accès à des API de navigateur, les composants interagissent avec JavaScript. Les composants peuvent utiliser les mêmes API ou bibliothèques que JavaScript. Le code C# peut appeler du code JavaScript, et le code JavaScript peut appeler du code C#. Pour plus d'informations, consultez <xref:blazor/javascript-interop>.

## <a name="code-sharing-and-net-standard"></a>Partage de code et .NET Standard

Blazor implémente [.NET Standard 2,0](/dotnet/standard/net-standard). .NET Standard est une spécification formelle d’API .NET qui sont communes aux implémentations .NET. .NET Standard bibliothèques de classes peuvent être partagées entre différentes plateformes .NET, telles que Blazor, .NET Framework, .NET Core, Xamarin, mono et Unity.

Les API qui ne sont pas applicables à l’intérieur d’un navigateur web (par exemple l’accès au système de fichiers, l’ouverture d’un socket et le threading) lèvent une <xref:System.PlatformNotSupportedException>.

## <a name="additional-resources"></a>Ressources supplémentaires

* [WebAssembly](https://webassembly.org/)
* <xref:blazor/hosting-models>
* [Guide C#](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
* Liens de la communauté [isard Blazor](https://github.com/AdrienTorris/awesome-blazor)
