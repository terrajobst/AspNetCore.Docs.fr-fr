---
title: commande dotnet aspnet-codegenerator
author: rick-anderson
ms.author: riande
description: La commande dotnet aspnet-codegenerator structure des projets ASP.NET Core
ms.date: 07/04/2019
uid: fundamentals/tools/dotnet-aspnet-codegenerator
ms.openlocfilehash: 55b592d9d203970777c84438e210519957abb35d
ms.sourcegitcommit: f6e6730872a7d6f039f97d1df762f0d0bd5e34cf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561682"
---
# <a name="dotnet-aspnet-codegenerator"></a>dotnet aspnet-codegenerator

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

`dotnet aspnet-codegenerator` - Exécute le moteur de génération de modèles automatique ASP.NET Core. `dotnet aspnet-codegenerator` est uniquement requis pour générer automatiquement des modèles à partir de la ligne de commande, il n’est pas nécessaire d’utiliser la génération de modèles automatique avec Visual Studio.

Cet article s’applique à : [SDK .NET Core 2.2](https://dotnet.microsoft.com/download/dotnet-core/2.2) et versions ultérieures.

## <a name="installing-aspnet-codegenerator"></a>Installation d’aspnet-codegenerator

`aspnet-codegenerator` est un [outil global](/dotnet/core/tools/global-tools) qui doit être installé. La commande suivante installe la dernière version stable de l’outil `aspnet-codegenerator` :

```console
dotnet tool install -g aspnet-codegenerator
```

La commande suivante met à jour `aspnet-codegenerator` vers la dernière version stable disponible à partir du SDK .NET Core installé :

```console
dotnet tool update -g aspnet-codegenerator
```

## <a name="synopsis"></a>Résumé

```
dotnet aspnet-codegenerator [arguments] [-p|--project] [-n|--nuget-package-dir] [-c|--configuration] [-tfm|--target-framework] [-b|--build-base-path] [--no-build] 
dotnet aspnet-codegenerator [-h|--help]
```

## <a name="description"></a>Description

La commande globale `dotnet aspnet-codegenerator ` exécute le générateur de code ASP.NET Core et le moteur de génération de modèles automatique.

## <a name="arguments"></a>Arguments

`generator`

Le générateur de code à effectuer. Les générateurs suivants sont disponibles :

| Générateur | Opération |
| ----------------- | ------------ | 
| superficie      | [Génération de modèles automatique pour une zone](/aspnet/core/mvc/controllers/areas) |
  contrôleur| [Génération de modèles automatique pour un contrôleur](/aspnet/core/tutorials/first-mvc-app/adding-model) |
  identité  | [Génération de modèles automatique pour une identité](/aspnet/core/security/authentication/scaffold-identity) |
  razorpage | [Génération de modèles automatique pour Razor Pages](/aspnet/core/tutorials/razor-pages/model) |
  view      | [Génération de modèles automatique pour une vue](/aspnet/core/mvc/views/overview) |

## <a name="options"></a>Options

`-n|--nuget-package-dir`

Spécifie le répertoire du package NuGet.

`-c|--configuration {Debug|Release}`

Définit la configuration de build. La valeur par défaut est `Debug`.

`-tfm|--target-framework`

[Framework](/dotnet/standard/frameworks) cible à utiliser. Par exemple, `net46`.

`-b|--build-base-path`

Le chemin de base de génération.

`-h|--help`

Affiche une aide brève pour la commande.

`--no-build`

Ne génère pas le projet avant l’exécution. L’indicateur `--no-restore` est également défini implicitement.

`-p|--project <PATH>`

Spécifie le chemin du fichier projet à exécuter (nom de dossier ou chemin complet). Si aucune valeur n’est spécifiée, le répertoire actif est utilisé par défaut.

## <a name="generator-options"></a>Options du générateur

Les sections suivantes décrivent en détail les options disponibles pour les générateurs pris en charge :

* Domaine
* Contrôleur
* Identité  
* Razorpage
* Vue

<a name="area"></a>

### <a name="area-options"></a>Options de zone

Cet outil est conçu pour les projets web ASP.NET Core avec des contrôleurs et des vues. Il n’est pas destinée aux applications Razor Pages.

Utilisation : `dotnet aspnet-codegenerator area AreaNameToGenerate`

La commande précédente génère les dossiers suivants :

* *Les zones (areas)*
  * *AreaNameToGenerate*
    * *Contrôleurs*
    * *Données*
    * *Modèles*
    * *Vues*

<a name="ctl"></a>

### <a name="controller-options"></a>Options de contrôleur

Le tableau ci-dessous répertorie les options pour `aspnet-codegenerator` `controller` et `razorpage` :

[!INCLUDE [aspnet-codegenerator-args-md.md](~/includes/aspnet-codegenerator-args-md.md)]

Le tableau ci-dessous répertorie les options uniques à `aspnet-codegenerator controller` :

| Option               | Description|
| ----------------- | ------------ |
| --controllerName ou -name | Nom du contrôleur. |
| --useAsyncActions ou -async | Générer des actions asynchrones du contrôleur. |
| --noViews ou -nv | Ne générer **aucune** vue. |
| --restWithNoViews ou -api  | Générer un contrôleur avec l’API de style REST. `noViews` est supposé et les options associées à la vue sont ignorées. |
| --readWriteActions ou -actions | Générer un contrôleur avec actions en lecture/écriture sans modèle. |

Utilisez le commutateur `-h` pour obtenir de l’aide sur la commande `aspnet-codegenerator controller` :

```console
dotnet aspnet-codegenerator controller -h
```

Consultez [Générer automatiquement le modèle de film](/aspnet/core/tutorials/razor-pages/model) pour obtenir un exemple de `dotnet aspnet-codegenerator controller`.

### <a name="razorpage"></a>Razorpage

<a name="rp"></a>

Les Razor Pages peuvent être structurées individuellement en spécifiant le nom de la nouvelle page et le modèle à utiliser. Les modèles pris en charge sont :

* `Empty`
* `Create`
* `Edit`
* `Delete`
* `Details`
* `List`

Par exemple, la commande suivante utilise le modèle de modification pour générer *MyEdit.cshtml* et *MyEdit.cshtml.cs* :

```console
dotnet aspnet-codegenerator razorpage MyEdit Edit -m Movie -dc RazorPagesMovieContext -outDir Pages/Movies
```

En règle générale, le modèle et le nom de fichier générés ne sont pas spécifiés, et les modèles suivants sont créés :

* `Create`
* `Edit`
* `Delete`
* `Details`
* `List`

Le tableau ci-dessous répertorie les options pour `aspnet-codegenerator` `razorpage` et `controller` :

[!INCLUDE [aspnet-codegenerator-args-md.md](~/includes/aspnet-codegenerator-args-md.md)]

Le tableau ci-dessous répertorie les options uniques à `aspnet-codegenerator razorpage` :

| Option               | Description|
| ----------------- | ------------ |
|   --namespaceName ou -namespace | Nom de l’espace de noms à utiliser pour le PageModel généré |
| --partialView ou -partial | Générer une vue partielle. Les options de mise en page -l et -udl sont ignorées si ceci est spécifié. |
| --noPageModel ou -npm | Choisir de ne pas générer une classe PageModel pour le modèle vide |

Utilisez le commutateur `-h` pour obtenir de l’aide sur la commande `aspnet-codegenerator razorpage` :

```console
dotnet aspnet-codegenerator razorpage -h
```

Consultez [Générer automatiquement le modèle de film](/aspnet/core/tutorials/razor-pages/model) pour obtenir un exemple de `dotnet aspnet-codegenerator razorpage`.

### <a name="identity"></a>Identité

Voir [Modèle automatique d’identité](/aspnet/core/security/authentication/scaffold-identity)