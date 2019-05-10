---
title: Composants d’application dans ASP.NET Core
author: ardalis
description: Apprenez à utiliser les composants d’application, c’est-à-dire les abstractions des ressources d’une application, pour découvrir ou éviter de charger les fonctionnalités d’un assembly.
ms.author: riande
ms.date: 01/04/2017
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 5d5e79d3afe0eee6d0b126d4642ccd5f61bff409
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64888134"
---
# <a name="application-parts-in-aspnet-core"></a>Composants d’application dans ASP.NET Core

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

Un *composant d’application* est une abstraction des ressources d’une application, qui permet de découvrir des fonctionnalités MVC telles que les contrôleurs, les composants de vue ou les Tag Helpers. AssemblyPart est un exemple de composant d’application qui encapsule une référence d’assembly, et expose les types et les références de compilation. Les *fournisseurs de fonctionnalités* utilisent les composants d’application pour remplir les fonctionnalités d’une application ASP.NET Core MVC. Le cas d’usage principal des composants d’application est de vous permettre de configurer votre application pour découvrir (ou éviter de charger) les fonctionnalités MVC d’un assembly.

## <a name="introducing-application-parts"></a>Présentation des composants d’application

Les applications MVC chargent leurs fonctionnalités à partir des [composants d’application](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart). En l’occurrence, la classe [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) représente un composant d’application qui s’appuie sur un assembly. Vous pouvez utiliser ces classes pour découvrir et charger des fonctionnalités MVC, par exemple les contrôleurs, les composants de vues, les Tag Helpers et les sources de compilation Razor. [ApplicationPartManager](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) est responsable du suivi des composants d’application et des fournisseurs de fonctionnalités accessibles à l’application MVC. Vous pouvez interagir avec `ApplicationPartManager` dans `Startup` quand vous configurez MVC :

```csharp
// create an assembly part from a class's assembly
var assembly = typeof(Startup).GetTypeInfo().Assembly;
services.AddMvc()
    .AddApplicationPart(assembly);

// OR
var assembly = typeof(Startup).GetTypeInfo().Assembly;
var part = new AssemblyPart(assembly);
services.AddMvc()
    .ConfigureApplicationPartManager(apm => apm.ApplicationParts.Add(part));
```

Par défaut, MVC recherche les contrôleurs dans l’arborescence des dépendances (même dans d’autres assemblys). Pour charger un assembly arbitraire (par exemple, à partir d’un plug-in qui n’est pas référencé au moment de la compilation), vous pouvez utiliser un composant d’application.

Vous pouvez utiliser des composants d’application pour *éviter* de rechercher des contrôleurs dans un assembly ou un emplacement particulier. Vous pouvez contrôler les composants (ou assemblys) accessibles à l’application en modifiant la collection `ApplicationParts` de `ApplicationPartManager`. L’ordre des entrées dans la collection `ApplicationParts` n’est pas important. Il est important de configurer complètement `ApplicationPartManager` avant de l’utiliser pour configurer les services dans le conteneur. Par exemple, vous devez configurer entièrement `ApplicationPartManager` avant d’appeler `AddControllersAsServices`. Si vous ne le faites pas, les contrôleurs des composants d’application ajoutés après cet appel de méthode ne seront pas affectés (ne seront pas inscrits en tant que services), ce qui risque d’entraîner un comportement incorrect de votre application.

Si vous avez un assembly qui contient des contrôleurs que vous ne souhaitez pas utiliser, supprimez-le de `ApplicationPartManager` :

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(apm =>
    {
        var dependentLibrary = apm.ApplicationParts
            .FirstOrDefault(part => part.Name == "DependentLibrary");

        if (dependentLibrary != null)
        {
           apm.ApplicationParts.Remove(dependentLibrary);
        }
    })
```

En plus de l’assembly de votre projet et de ses assemblys dépendants, `ApplicationPartManager` inclut des composants pour `Microsoft.AspNetCore.Mvc.TagHelpers` et `Microsoft.AspNetCore.Mvc.Razor` par défaut.

## <a name="application-feature-providers"></a>Fournisseurs de fonctionnalités d’application

Les fournisseurs de fonctionnalités d’application examinent les composants d’application et fournissent des fonctionnalités pour ces composants. Il existe des fournisseurs de fonctionnalités intégrés pour les fonctionnalités MVC suivantes :

* [Contrôleurs](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [Référence de métadonnées](/dotnet/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [Les Tag Helpers](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [Les composants de vues](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

Les fournisseurs de fonctionnalités héritent de `IApplicationFeatureProvider<T>`, où `T` correspond au type de la fonctionnalité. Vous pouvez implémenter vos propres fournisseurs de fonctionnalités pour tous les types de fonctionnalité de MVC listés ci-dessus. L’ordre des fournisseurs de fonctionnalités dans la collection `ApplicationPartManager.FeatureProviders` peut avoir son importance, car les fournisseurs suivants peuvent réagir à des actions entreprises par les fournisseurs précédents.

### <a name="sample-generic-controller-feature"></a>Aperçu : Fonctionnalité de contrôleur générique

Par défaut, ASP.NET Core MVC ignore les contrôleurs génériques (par exemple `SomeController<T>`). Cet exemple utilise un fournisseur de fonctionnalités de contrôleur qui s’exécute après le fournisseur par défaut, et qui ajoute des instances de contrôleurs génériques pour une liste spécifique de types (définis dans `EntityTypes.Types`) :

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

Types d’entité :

[!code-csharp[](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

Le fournisseur de fonctionnalités est ajouté dans `Startup` :

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(apm => 
        apm.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

Par défaut, les noms de contrôleurs génériques utilisés pour le routage se présentent sous la forme *GenericController`1[Widget]* au lieu de *Widget*. L’attribut suivant est utilisé pour modifier le nom afin qu’il corresponde au type générique utilisé par le contrôleur :

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

Classe `GenericController` :

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

Voici le résultat, quand un routage correspondant est demandé :

![L’exemple de sortie de l’exemple d’application est : « Hello from a generic Sprocket controller. »](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a>Aperçu : Afficher les fonctionnalités disponibles

Vous pouvez effectuer une itération parmi les fonctionnalités renseignées accessibles à votre application en demandant un `ApplicationPartManager` via une [injection de dépendances](../../fundamentals/dependency-injection.md) et en l’utilisant pour remplir les instances des fonctionnalités appropriées :

[!code-csharp[](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

Exemple de sortie :

![Exemple de sortie de l’exemple d’application](app-parts/_static/available-features.png)
