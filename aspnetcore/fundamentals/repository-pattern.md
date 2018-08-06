---
title: Modèle de référentiel avec ASP.NET Core
author: ardalis
description: Découvrez comment implémenter le modèle de conception d’application de référentiel dans une application ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2018
uid: fundamentals/repository-pattern
ms.openlocfilehash: d64c3d090f62c580ebc05a6bbc2744a4508bb9bc
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342595"
---
# <a name="repository-pattern-with-aspnet-core"></a>Modèle de référentiel avec ASP.NET Core

Par [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com) et [Luke Latham](https://github.com/guardrex)

Le *modèle de référentiel* est un modèle de conception qui isole l’accès aux données derrière des abstractions d’interface. La connexion à la base de données et la manipulation d’objets de stockage de données sont assurées via des méthodes fournies par l’implémentation de l’interface. Par conséquent, il n’est pas nécessaire d’appeler du code pour gérer les problèmes de base de données, tels que les connexions, les commandes et les lecteurs.

L’implémentation du modèle de référentiel avec ASP.NET Core présente les avantages suivants :

* L’organisation de l’application est moins complexe, sans interdépendance directe entre l’entreprise et les couches d’accès aux données.
* Il est plus facile de réutiliser du code d’accès de base de données, car le code est géré de manière centralisée par un ou plusieurs référentiels.
* Le domaine d’entreprise peut être testé indépendamment par unité à partir de la couche de base de données.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

L’[échantillon d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) utilise le modèle de référentiel pour initialiser et afficher une liste de noms de personnages de films. L’application utilise [Entity Framework Core](/ef/core/) et une classe `ApplicationDbContext` pour la persistance de ses données, mais l’infrastructure de la base de données n’est pas apparente à l’endroit où les données sont accessibles. L’accès aux données et les objets de base de données sont abstraits derrière un [référentiel](https://martinfowler.com/eaaCatalog/repository.html).

## <a name="repository-interface"></a>Interface de référentiel

Une interface de référentiel définit les propriétés et les méthodes d’implémentation. Dans l’exemple d’application, l’interface de référentiel pour les données de personnages de films est `ICharacterRepository`. `ICharacterRepository` définit les méthodes `ListAll` et `Add` requises pour fonctionner avec des instances `Character` dans l’application :

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

`Character` est défini comme :

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

## <a name="repository-concrete-type"></a>Type concret du référentiel

L’interface est implémentée par un type concret. Dans l’échantillon d’application, `CharacterRepository` gère le contexte de base de données et met en œuvre les méthodes `ListAll` et `Add` :

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

## <a name="register-the-repository-service"></a>Inscrire le service de référentiel

Le référentiel et le contexte de base de données sont inscrits avec le conteneur de service dans `Startup.ConfigureServices`. Dans l’échantillon d’application, `ApplicationDbContext` est configuré avec l’appel à la méthode d’extension [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext). `ICharacterRepository` est inscrit en tant que service étendu :

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,18)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,12)]

::: moniker-end

## <a name="inject-an-instance-of-the-repository"></a>Injecter une instance du référentiel

Dans une classe où l’accès à la base de données est requis, une instance du référentiel est demandée via le constructeur et attribuée à un champ privé pour une utilisation par les méthodes de la classe. Dans l’échantillon de l’application, `ICharacterRepository` est utilisé pour :

* Remplir la base de données si aucun personnage n’existe.
* Obtenir une liste des personnages pour l’affichage.

Noter comment le code d’appel interagit uniquement avec l’implémentation de l’interface, `CharacterRepository`. L’appel de code n’utilise pas directement le `ApplicationDbContext` :

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Pages/Index.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Controllers/HomeController.cs?name=snippet1)]

::: moniker-end

## <a name="generic-repository-interface"></a>Interface de référentiel générique

Cette rubrique et son échantillon d’application montrent l’implémentation la plus simple du modèle de référentiel, où un référentiel est créé pour chaque objet métier. Si l’application grandit au-delà de quelques objets, une *interface de référentiel générique* peut réduire la quantité de code nécessaire pour implémenter le modèle de référentiel. Pour plus d’informations, consultez [DevIQ : modèle de référentiel : interface de référentiel générique](http://deviq.com/repository-pattern/).

## <a name="additional-resources"></a>Ressources supplémentaires

* [DevIQ : modèle de référentiel](https://deviq.com/repository-pattern/)
* [Injection de dépendances](xref:fundamentals/dependency-injection)
* [Injection de dépendances dans les vues](xref:mvc/views/dependency-injection)
* [Injection de dépendances dans les contrôleurs](xref:mvc/controllers/dependency-injection)
* [Injection de dépendances dans les gestionnaires d’exigences](xref:security/authorization/dependencyinjection)
* [Inversion des conteneurs de contrôle et modèle d’injection de dépendance](https://www.martinfowler.com/articles/injection.html)
