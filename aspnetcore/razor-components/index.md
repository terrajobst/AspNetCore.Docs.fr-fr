---
title: Introduction à Razor Components
author: guardrex
description: Explorez ASP.NET Core Razor Components, un moyen de créer une interface utilisateur web interactive côté client à l’aide de .NET dans une application ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/27/2019
uid: razor-components/index
ms.openlocfilehash: 8b2e87fe856598a5ac231e3bc1d413957829b448
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/30/2019
ms.locfileid: "58751015"
---
# <a name="introduction-to-razor-components"></a>Introduction à Razor Components

Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)

*Razor Components* constitue un moyen de créer une interface utilisateur web interactive côté client à l’aide de .NET :

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

Pour plus d'informations, consultez <xref:razor-components/hosting-models#server-side-hosting-model>.

*Blazor* est le modèle d’hébergement expérimental côté client de Razor Components. Blazor s’exécute sur .NET dans le navigateur à l’aide de standards web ouverts, sans plug-in ni aucune transpilation de code. Pour plus d’informations, consultez <xref:spa/blazor/index> et <xref:razor-components/hosting-models#client-side-hosting-model>.

## <a name="components"></a>Composants

Un *composant Razor* est un élément d’interface utilisateur, comme une page, une boîte de dialogue ou un formulaire de saisie de données. Les composants gèrent les événements de l’utilisateur et définissent une logique de rendu de l’interface utilisateur flexible. Les composants peuvent être imbriqués et réutilisés.

Les composants sont des classes .NET intégrées dans des assemblys .NET pouvant être partagées et distribuées comme packages NuGet. La classe est normalement écrite sous la forme d’une page de balisage Razor avec une extension de fichier *.razor*.

[Razor](xref:mvc/views/razor) est une syntaxe pour la combinaison de balisage HTML et de code C#. Razor accroît la productivité des développeurs en leur permettant de basculer entre le balisage et le code C# dans le même fichier avec prise en charge d’[IntelliSense](/visualstudio/ide/using-intellisense). Razor Pages et les vues MVC utilisent également Razor. Contrairement à Razor Pages et aux vues MVC, qui reposent sur un modèle requête/réponse, les composants sont utilisés spécifiquement pour gérer la composition de l’interface utilisateur. Razor Components peut être utilisé spécifiquement pour la composition et la logique d’interface utilisateur côté client.

Le balisage suivant est un exemple de composant de boîte de dialogue personnalisée dans un fichier Razor (*DialogComponent.razor*) :

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

Pour les applications qui nécessitent des bibliothèques JavaScript tierces et des API de navigateur, les composants interagissent avec JavaScript. Les composants peuvent utiliser les mêmes API ou bibliothèques que JavaScript. Le code C# peut appeler du code JavaScript, et le code JavaScript peut appeler du code C#. Pour plus d’informations, consultez [Interopérabilité JavaScript](xref:razor-components/javascript-interop).

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:spa/blazor/index>
* [WebAssembly](http://webassembly.org/)
* [Guide C#](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
