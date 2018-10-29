---
title: Injection de dépendances dans les contrôleurs dans ASP.NET Core
author: ardalis
description: Découvrez comment les contrôleurs ASP.NET Core MVC demandent explicitement leurs dépendances par le biais de leurs constructeurs avec l’injection de dépendances dans ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 12247dbbbb6de3f8feb7bc37caec4ecf4bd21719
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206339"
---
# <a name="dependency-injection-into-controllers-in-aspnet-core"></a>Injection de dépendances dans les contrôleurs dans ASP.NET Core

<a name="dependency-injection-controllers"></a>

Par [Steve Smith](https://ardalis.com/)

Les contrôleurs ASP.NET Core MVC doivent demander leurs dépendances explicitement via leurs constructeurs. Dans certains cas, les actions d’un contrôleur individuel peuvent nécessiter un service, et il peut ne pas être judicieux de faire les demandes au niveau du contrôleur. Dans ce cas, vous pouvez également choisir d’injecter un service comme paramètre sur la méthode d’action.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="dependency-injection"></a>Injection de dépendances

L’injection de dépendances est une technique qui suit le [principe d’inversion de dépendance](http://deviq.com/dependency-inversion-principle/) et permet de composer des applications avec des modules faiblement couplés. ASP.NET Core a une prise en charge intégrée de [l’injection de dépendances](../../fundamentals/dependency-injection.md), ce qui facilite le test et la gestion des applications.

## <a name="constructor-injection"></a>Injection de constructeurs

La prise en charge intégrée dans ASP.NET Core de l’injection de dépendances basée sur les constructeurs s’étend aux contrôleurs MVC. En ajoutant simplement un type de service à votre contrôleur en tant que paramètre de constructeur, ASP.NET Core tente de résoudre ce type en utilisant son conteneur de services intégrés. Les services sont, en général mais pas toujours, définis en utilisant des interfaces. Par exemple, si votre application a une logique métier qui dépend de l’heure, vous pouvez injecter un service qui récupère l’heure (au lieu de la coder en dur), ce qui permet à vos tests de réussir dans des implémentations qui utilisent une heure définie.

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


L’implémentation d’une interface comme celle-ci pour qu’elle utilise l’horloge système lors de l’exécution est simple :

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


Ceci étant en place, nous pouvons utiliser le service dans notre contrôleur. Dans ce cas, nous avons ajouté de la logique pour la méthode `HomeController` `Index` permettant d’afficher un message d’accueil à l’utilisateur en fonction de l’heure du jour.

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

Si nous exécutons l’application maintenant, nous allons très probablement rencontrer une erreur :

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

Cette erreur se produit si nous n’avons pas configuré un service dans la méthode `ConfigureServices` de notre classe `Startup`. Pour spécifier que les demandes de `IDateTime` doivent être résolues en utilisant une instance de `SystemDateTime`, ajoutez la ligne en surbrillance dans la liste ci-dessous à votre méthode `ConfigureServices` :

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> Ce service particulier peut être implémenté en utilisant plusieurs options de durée de vie différentes (`Transient`, `Scoped` ou `Singleton`). Consultez [Injection de dépendances](../../fundamentals/dependency-injection.md) pour comprendre comment chacune de ces options d’étendue affecte le comportement de votre service.

Une fois que le service a été configuré, l’exécution de l’application et la navigation jusqu’à la page d’accueil doivent afficher le message basé sur l’heure comme attendu :

![Message d’accueil du serveur](dependency-injection/_static/server-greeting.png)

>[!TIP]
> Consultez [Tester la logique du contrôleur](testing.md) pour découvrir comment demander explicitement les dépendances ([http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/)) dans les contrôleurs afin de rendre le code plus facile à tester.

L’injection de dépendances intégrée d’ASP.NET Core ne prend en charge qu’un seul constructeur pour les classes demandant des services. Si vous avez plusieurs constructeurs, vous pouvez recevoir une exception indiquant :

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

Comme le message d’erreur l’indique, vous pouvez remédier à ce problème en n’utilisant un seul constructeur. Vous pouvez également [remplacer le conteneur d’injection de dépendances par défaut par une implémentation de tiers](xref:fundamentals/dependency-injection#default-service-container-replacement), la plupart de ces implémentations prenant en charge plusieurs constructeurs.

## <a name="action-injection-with-fromservices"></a>Injection d’actions avec FromServices

Parfois, vous n’avez pas besoin d’un service pour plusieurs actions dans votre contrôleur. Dans ce cas, il peut être judicieux d’injecter le service comme paramètre de la méthode d’action. Vous faites cela en marquant le paramètre avec l’attribut `[FromServices]`, comme illustré ici :

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a>Accès aux paramètres à partir d’un contrôleur

L’accès aux paramètres de configuration ou d’application à partir d’un contrôleur est un modèle commun. Cet accès doit utiliser le modèle Options décrit dans [Configuration](xref:fundamentals/configuration/index). D’une façon générale, vous ne devez pas demander les paramètres directement à partir de votre contrôleur avec l’injection de dépendances. Une meilleure approche consiste à demander une instance de `IOptions<T>`, où `T` est la classe de configuration dont vous avez besoin.

Pour utiliser le modèle Options, vous devez créer une classe qui représente les options, comme celle-ci :

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

Vous devez ensuite configurer l’application pour qu’elle utilise le modèle Options et ajouter votre classe de configuration à la collection de services dans `ConfigureServices` :

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> Dans la liste ci-dessus, nous configurons l’application pour qu’elle lise les paramètres dans un fichier au format JSON. Vous pouvez également configurer les paramètres entièrement dans le code, comme illustré dans le code commenté ci-dessus. Consultez [Configuration](xref:fundamentals/configuration/index) pour d’autres options de configuration.

Une fois que vous avez spécifié un objet de configuration fortement typé (dans ce cas, `SampleWebSettings`) et que vous l’avez ajouté à la collection de services, vous pouvez le demander à partir de n’importe quelle méthode de contrôleur ou d’action en demandant une instance de `IOptions<T>` (dans ce cas, `IOptions<SampleWebSettings>`). Le code suivant montre comment demander les paramètres à partir d’un contrôleur :

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

Suivre le modèle Options permet de découpler les paramètres et la configuration, et garantit que le contrôleur est conforme à la [séparation des problèmes](http://deviq.com/separation-of-concerns/), car il n’a pas besoin de savoir ni comment ni où rechercher les informations des paramètres. Cela simplifie également les tests unitaires du contrôleur ([Tester la logique du contrôleur](testing.md)), car il n’y a pas de [couplage des fonctionnalités statiques](http://deviq.com/static-cling/) ni d’instanciation directe de classes de paramètres au sein de la classe du contrôleur.
