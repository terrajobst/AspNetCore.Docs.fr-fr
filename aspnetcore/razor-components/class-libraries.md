---
title: Bibliothèques de classes de composants de Razor
author: guardrex
description: Découvrez comment les composants peuvent être inclus dans les composants de Razor des applications à partir d’une bibliothèque de composants externes.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/14/2019
uid: razor-components/class-libraries
ms.openlocfilehash: 1064ad60d90af15af483ba9bca5ed85fb63c2924
ms.sourcegitcommit: 10e14b85490f064395e9b2f423d21e3c2d39ed8b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2019
ms.locfileid: "59515392"
---
# <a name="razor-components-class-libraries"></a>Bibliothèques de classes de composants de Razor

Par [Simon Timms](https://github.com/stimms)

Les composants peuvent être partagées dans les bibliothèques de classes Razor entre projets. Les composants peuvent être inclus dans :

* Un autre projet dans la solution.
* Un package NuGet.
* Une bibliothèque .NET référencée.

Tout comme les composants sont des types .NET standards, les composants fournis par les bibliothèques de classes Razor sont des assemblys .NET normales.

Utilisez le `razorclasslib` modèle (bibliothèque de classes Razor) avec le [dotnet nouvelle](/dotnet/core/tools/dotnet-new) commande :

```console
dotnet new razorclasslib -o MyComponentLib1
```

Ajouter des fichiers de composant de Razor (*.razor*) à la bibliothèque de classes Razor.

Pour ajouter la bibliothèque à un projet existant, utilisez la [dotnet sln](/dotnet/core/tools/dotnet-sln) commande :

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

```console
dotnet sln add .\MyComponentLib1
```

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

```console
dotnet add WebApplication1 reference MyComponentLib1
```

---

> [!NOTE]
> Bibliothèques de classes Razor ne sont pas compatibles avec les applications Blazor dans ASP.NET Core Preview 3.
>
> Pour créer des composants dans une bibliothèque qui peut être partagée avec les applications Blazor et composants de Razor, utilisez une bibliothèque de classes Blazor créée par la `blazorlib` modèle.
>
> Bibliothèques de classes Razor ne prennent en charge les ressources statiques dans ASP.NET Core Preview 3. Les bibliothèques de composants à l’aide de la `blazorlib` modèle peut inclure des fichiers statiques, tels que des images, de JavaScript et de feuilles de style. Au moment de la génération, les fichiers statiques sont incorporés dans le fichier d’assembly généré (*.dll*), ce qui permet la consommation des composants sans avoir à vous soucier de l’inclusion de leurs ressources. Les fichiers inclus dans le `content` répertoire sont marquées comme ressource incorporée.

## <a name="consume-a-library-component"></a>Utiliser un composant de bibliothèque

Pour consommer des composants définis dans une bibliothèque dans un autre projet, le [ @addTagHelper ](xref:mvc/views/tag-helpers/intro#add-helper-label) directive doit être utilisée. Les composants individuels peuvent être ajoutés par nom.

Le format général de la directive est :

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<Component1 />
```

Par exemple, la directive suivante ajoute `Component1` de `MyComponentLib1`:

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1
```

Toutefois, il est courant d’inclure tous les composants à partir d’un assembly à l’aide d’un caractère générique (`*`) :

```cshtml
@addTagHelper *, MyComponentLib1
```

Le `@addTagHelper` directive peut être incluse dans *_ViewImport.cshtml* pour rendre les composants disponibles pour un projet entier ou appliqués à une seule page ou un ensemble de pages au sein d’un dossier. Avec la `@addTagHelper` directive en place, les composants de la bibliothèque de composant peuvent être consommés comme s’ils étaient dans le même assembly que l’application.

## <a name="build-pack-and-ship-to-nuget"></a>Build, les packs et les expédier à NuGet

Étant donné que les bibliothèques de composants sont des bibliothèques .NET standard, empaquetage et leur transfert vers NuGet ne diffère pas d’emballage et d’expédition n’importe quelle bibliothèque à NuGet. Empaquetage est effectué à l’aide de la [dotnet pack](/dotnet/core/tools/dotnet-pack) commande :

```console
dotnet pack
```

Télécharger le package à l’aide de NuGet le [dotnet nuget publier](/dotnet/core/tools/dotnet-nuget-push) commande :

```console
dotnet nuget publish
```

Lorsque vous utilisez la `blazorlib` modèle, les ressources statiques sont inclus dans le package NuGet. Consommateurs de la bibliothèque reçoivent automatiquement des scripts et feuilles de style, les consommateurs ne sont pas nécessaires pour installer manuellement les ressources.
