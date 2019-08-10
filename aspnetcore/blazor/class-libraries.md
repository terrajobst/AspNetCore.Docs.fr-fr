---
title: Bibliothèques de classes des composants Razor ASP.NET Core
author: guardrex
description: Découvrez comment les composants peuvent être inclus dans des applications éblouissantes à partir d’une bibliothèque de composants externes.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2019
uid: blazor/class-libraries
ms.openlocfilehash: 402b7b072554f63f85e7cf5e55336104d235a071
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68948439"
---
# <a name="aspnet-core-razor-components-class-libraries"></a>Bibliothèques de classes des composants Razor ASP.NET Core

Par [Simon Timms](https://github.com/stimms)

Les composants peuvent être partagés dans une [bibliothèque de classes Razor (RCL)](xref:razor-pages/ui-class) entre les projets. Une *bibliothèque de classes de composants Razor* peut être incluse à partir de:

* Un autre projet dans la solution.
* Package NuGet.
* Bibliothèque .NET référencée.

Tout comme les composants sont des types .NET standard, les composants fournis par un RCL sont des assemblys .NET normaux.

## <a name="create-an-rcl"></a>Créer un RCL

Suivez les instructions de l' <xref:blazor/get-started> article pour configurer votre environnement pour éblouissant.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Créer un nouveau projet.
1. Sélectionnez **Nouvelle application web ASP.NET Core**. Sélectionnez **Suivant**.
1. Indiquez un nom de projet dans le champ **Nom du projet**, ou acceptez le nom de projet par défaut. Les exemples de cette rubrique utilisent le nom `MyComponentLib1`du projet. Sélectionnez **Créer**.
1. Dans la boîte de dialogue **Créer une application web ASP.NET Core**, vérifiez que **.NET Core** et **ASP.NET Core 3.0** sont sélectionnés.
1. Sélectionnez le modèle **bibliothèque de classes Razor** . Sélectionnez **Créer**.
1. Ajouter RCL à une solution:
   1. Cliquez avec le bouton droit sur la solution. Sélectionnez **Ajouter** > un**projet existant**.
   1. Accédez au fichier projet de RCL.
   1. Sélectionnez le fichier projet de RCL ( *. csproj*).
1. Ajoutez une référence à l’RCL à partir de l’application:
   1. Cliquez avec le bouton droit sur le projet d’application. Sélectionnez **Ajouter** > une**référence**.
   1. Sélectionnez le projet RCL. Sélectionnez **OK**.

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

1. Utilisez le modèle de **bibliothèque de classes Razor** (`razorclasslib`) avec la commande [dotnet New](/dotnet/core/tools/dotnet-new) dans un interpréteur de commandes. Dans l’exemple suivant, un RCL est créé nommé `MyComponentLib1`. Le dossier qui contient `MyComponentLib1` est créé automatiquement lors de l’exécution de la commande:

   ```console
   dotnet new razorclasslib -o MyComponentLib1
   ```

1. Pour ajouter la bibliothèque à un projet existant, utilisez la commande [dotnet Add Reference](/dotnet/core/tools/dotnet-add-reference) dans un interpréteur de commandes. Dans l’exemple suivant, le RCL est ajouté à l’application. Exécutez la commande suivante à partir du dossier du projet de l’application avec le chemin d’accès à la bibliothèque:

   ```console
   dotnet add reference {PATH TO LIBRARY}
   ```

---

## <a name="rcls-not-supported-for-client-side-apps"></a>RCLs non pris en charge pour les applications côté client

Dans la version préliminaire actuelle ASP.NET Core 3,0, les bibliothèques de classes Razor ne sont pas compatibles avec les applications côté client éblouissantes. Pour les applications côté client éblouissantes, utilisez une bibliothèque de composants éblouissante créée par `blazorlib` le modèle dans une interface de commande:

```console
dotnet new blazorlib -o MyComponentLib1
```

Les bibliothèques de composants `blazorlib` qui utilisent le modèle peuvent inclure des fichiers statiques, tels que des images, du JavaScript et des feuilles de style. Au moment de la génération, les fichiers statiques sont incorporés dans le fichier d’assembly ( *. dll*) généré, ce qui permet la consommation des composants sans avoir à se soucier de l’inclusion de leurs ressources. Tous les fichiers inclus dans `content` le répertoire sont marqués comme ressource incorporée.

## <a name="consume-a-library-component"></a>Consommer un composant de bibliothèque

Pour utiliser des composants définis dans une bibliothèque dans un autre projet, utilisez l’une des approches suivantes:

* Utilisez le nom de type complet avec l’espace de noms.
* Utilisez la directive [ \@using](xref:mvc/views/razor#using) de Razor. Des composants individuels peuvent être ajoutés par nom.

Dans les exemples suivants, `MyComponentLib1` est une bibliothèque de composants contenant `SalesReport` un composant.

Le `SalesReport` composant peut être référencé à l’aide de son nom de type complet avec l’espace de noms:

```cshtml
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

Le composant peut également être référencé si la bibliothèque est placée dans la portée avec une `@using` directive:

```cshtml
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

Incluez `@using MyComponentLib1` la directive dans le fichier *_Import. Razor* de niveau supérieur pour mettre les composants de la bibliothèque à la disposition d’un projet entier. Ajoutez la directive à un fichier *_Import. Razor* à n’importe quel niveau pour appliquer l’espace de noms à une seule page ou à un ensemble de pages dans un dossier.

## <a name="build-pack-and-ship-to-nuget"></a>Générer, empaqueter et envoyer à NuGet

Étant donné que les bibliothèques de composants sont des bibliothèques .NET standard, leur empaquetage et leur envoi à NuGet ne sont pas différents de l’empaquetage et de l’expédition d’une bibliothèque à NuGet. L’empaquetage est effectué à l’aide de la commande [dotnet Pack](/dotnet/core/tools/dotnet-pack) dans une interface de commande:

```console
dotnet pack
```

Téléchargez le package dans NuGet à l’aide de la commande [dotnet NuGet Publish](/dotnet/core/tools/dotnet-nuget-push) dans une interface de commande:

```console
dotnet nuget publish
```

Lorsque vous utilisez `blazorlib` le modèle, les ressources statiques sont incluses dans le package NuGet. Les consommateurs de bibliothèques reçoivent automatiquement des scripts et des feuilles de style, si bien que les consommateurs ne sont pas tenus d’installer manuellement les ressources.

## <a name="create-a-razor-components-class-library-with-static-assets"></a>Créer une bibliothèque de classes de composants Razor avec des ressources statiques

Un RCL peut inclure des ressources statiques. Les ressources statiques sont disponibles pour toutes les applications qui consomment la bibliothèque. Pour plus d'informations, consultez <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:razor-pages/ui-class>
