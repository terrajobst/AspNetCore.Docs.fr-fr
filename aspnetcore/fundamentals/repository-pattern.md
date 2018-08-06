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
# <a name="repository-pattern-with-aspnet-core"></a><span data-ttu-id="d6dc2-103">Modèle de référentiel avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d6dc2-103">Repository pattern with ASP.NET Core</span></span>

<span data-ttu-id="d6dc2-104">Par [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d6dc2-104">By [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d6dc2-105">Le *modèle de référentiel* est un modèle de conception qui isole l’accès aux données derrière des abstractions d’interface.</span><span class="sxs-lookup"><span data-stu-id="d6dc2-105">The *repository pattern* is a design pattern that isolates data access behind interface abstractions.</span></span> <span data-ttu-id="d6dc2-106">La connexion à la base de données et la manipulation d’objets de stockage de données sont assurées via des méthodes fournies par l’implémentation de l’interface.</span><span class="sxs-lookup"><span data-stu-id="d6dc2-106">Connecting to the database and manipulating data storage objects is performed through methods provided by the interface's implementation.</span></span> <span data-ttu-id="d6dc2-107">Par conséquent, il n’est pas nécessaire d’appeler du code pour gérer les problèmes de base de données, tels que les connexions, les commandes et les lecteurs.</span><span class="sxs-lookup"><span data-stu-id="d6dc2-107">Consequently, there's no need for calling code to deal with database concerns, such as connections, commands, and readers.</span></span>

<span data-ttu-id="d6dc2-108">L’implémentation du modèle de référentiel avec ASP.NET Core présente les avantages suivants :</span><span class="sxs-lookup"><span data-stu-id="d6dc2-108">Implementation of the repository pattern with ASP.NET Core has the following benefits:</span></span>

* <span data-ttu-id="d6dc2-109">L’organisation de l’application est moins complexe, sans interdépendance directe entre l’entreprise et les couches d’accès aux données.</span><span class="sxs-lookup"><span data-stu-id="d6dc2-109">Organization of the app is less complex with no direct interdependency between the business and data access layers.</span></span>
* <span data-ttu-id="d6dc2-110">Il est plus facile de réutiliser du code d’accès de base de données, car le code est géré de manière centralisée par un ou plusieurs référentiels.</span><span class="sxs-lookup"><span data-stu-id="d6dc2-110">It's easier to reuse database access code because the code is centrally managed by one or more repositories.</span></span>
* <span data-ttu-id="d6dc2-111">Le domaine d’entreprise peut être testé indépendamment par unité à partir de la couche de base de données.</span><span class="sxs-lookup"><span data-stu-id="d6dc2-111">The business domain can be independently unit tested from the database layer.</span></span>

<span data-ttu-id="d6dc2-112">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d6dc2-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="d6dc2-113">L’[échantillon d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) utilise le modèle de référentiel pour initialiser et afficher une liste de noms de personnages de films.</span><span class="sxs-lookup"><span data-stu-id="d6dc2-113">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) uses the repository pattern to initialize and display a list of movie character names.</span></span> <span data-ttu-id="d6dc2-114">L’application utilise [Entity Framework Core](/ef/core/) et une classe `ApplicationDbContext` pour la persistance de ses données, mais l’infrastructure de la base de données n’est pas apparente à l’endroit où les données sont accessibles.</span><span class="sxs-lookup"><span data-stu-id="d6dc2-114">The app uses [Entity Framework Core](/ef/core/) and an `ApplicationDbContext` class for its data persistence, but the database infrastructure isn't apparent where the data is accessed.</span></span> <span data-ttu-id="d6dc2-115">L’accès aux données et les objets de base de données sont abstraits derrière un [référentiel](https://martinfowler.com/eaaCatalog/repository.html).</span><span class="sxs-lookup"><span data-stu-id="d6dc2-115">Data access and database objects are abstracted behind a [repository](https://martinfowler.com/eaaCatalog/repository.html).</span></span>

## <a name="repository-interface"></a><span data-ttu-id="d6dc2-116">Interface de référentiel</span><span class="sxs-lookup"><span data-stu-id="d6dc2-116">Repository interface</span></span>

<span data-ttu-id="d6dc2-117">Une interface de référentiel définit les propriétés et les méthodes d’implémentation.</span><span class="sxs-lookup"><span data-stu-id="d6dc2-117">A repository interface defines the properties and methods for implementation.</span></span> <span data-ttu-id="d6dc2-118">Dans l’exemple d’application, l’interface de référentiel pour les données de personnages de films est `ICharacterRepository`.</span><span class="sxs-lookup"><span data-stu-id="d6dc2-118">In the sample app, the repository interface for movie character data is `ICharacterRepository`.</span></span> <span data-ttu-id="d6dc2-119">`ICharacterRepository` définit les méthodes `ListAll` et `Add` requises pour fonctionner avec des instances `Character` dans l’application :</span><span class="sxs-lookup"><span data-stu-id="d6dc2-119">`ICharacterRepository` defines the `ListAll` and `Add` methods required to work with `Character` instances in the app:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="d6dc2-120">`Character` est défini comme :</span><span class="sxs-lookup"><span data-stu-id="d6dc2-120">`Character` is defined as:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

## <a name="repository-concrete-type"></a><span data-ttu-id="d6dc2-121">Type concret du référentiel</span><span class="sxs-lookup"><span data-stu-id="d6dc2-121">Repository concrete type</span></span>

<span data-ttu-id="d6dc2-122">L’interface est implémentée par un type concret.</span><span class="sxs-lookup"><span data-stu-id="d6dc2-122">The interface is implemented by a concrete type.</span></span> <span data-ttu-id="d6dc2-123">Dans l’échantillon d’application, `CharacterRepository` gère le contexte de base de données et met en œuvre les méthodes `ListAll` et `Add` :</span><span class="sxs-lookup"><span data-stu-id="d6dc2-123">In the sample app, `CharacterRepository` manages the database context and implements the `ListAll` and `Add` methods:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

## <a name="register-the-repository-service"></a><span data-ttu-id="d6dc2-124">Inscrire le service de référentiel</span><span class="sxs-lookup"><span data-stu-id="d6dc2-124">Register the repository service</span></span>

<span data-ttu-id="d6dc2-125">Le référentiel et le contexte de base de données sont inscrits avec le conteneur de service dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d6dc2-125">The repository and database context are registered with the service container in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d6dc2-126">Dans l’échantillon d’application, `ApplicationDbContext` est configuré avec l’appel à la méthode d’extension [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext).</span><span class="sxs-lookup"><span data-stu-id="d6dc2-126">In the sample app, `ApplicationDbContext` is configured with the call to the extension method [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext).</span></span> <span data-ttu-id="d6dc2-127">`ICharacterRepository` est inscrit en tant que service étendu :</span><span class="sxs-lookup"><span data-stu-id="d6dc2-127">`ICharacterRepository` is registered as a scoped service:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,18)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,12)]

::: moniker-end

## <a name="inject-an-instance-of-the-repository"></a><span data-ttu-id="d6dc2-128">Injecter une instance du référentiel</span><span class="sxs-lookup"><span data-stu-id="d6dc2-128">Inject an instance of the repository</span></span>

<span data-ttu-id="d6dc2-129">Dans une classe où l’accès à la base de données est requis, une instance du référentiel est demandée via le constructeur et attribuée à un champ privé pour une utilisation par les méthodes de la classe.</span><span class="sxs-lookup"><span data-stu-id="d6dc2-129">In a class where database access is required, an instance of the repository is requested via the constructor and assigned to a private field for use by class methods.</span></span> <span data-ttu-id="d6dc2-130">Dans l’échantillon de l’application, `ICharacterRepository` est utilisé pour :</span><span class="sxs-lookup"><span data-stu-id="d6dc2-130">In the sample app, `ICharacterRepository` is used to:</span></span>

* <span data-ttu-id="d6dc2-131">Remplir la base de données si aucun personnage n’existe.</span><span class="sxs-lookup"><span data-stu-id="d6dc2-131">Populate the database if no characters exist.</span></span>
* <span data-ttu-id="d6dc2-132">Obtenir une liste des personnages pour l’affichage.</span><span class="sxs-lookup"><span data-stu-id="d6dc2-132">Obtain a list of the characters for display.</span></span>

<span data-ttu-id="d6dc2-133">Noter comment le code d’appel interagit uniquement avec l’implémentation de l’interface, `CharacterRepository`.</span><span class="sxs-lookup"><span data-stu-id="d6dc2-133">Notice how the calling code only interacts with the interface's implementation, `CharacterRepository`.</span></span> <span data-ttu-id="d6dc2-134">L’appel de code n’utilise pas directement le `ApplicationDbContext` :</span><span class="sxs-lookup"><span data-stu-id="d6dc2-134">Calling code doesn't use the `ApplicationDbContext` directly:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Pages/Index.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Controllers/HomeController.cs?name=snippet1)]

::: moniker-end

## <a name="generic-repository-interface"></a><span data-ttu-id="d6dc2-135">Interface de référentiel générique</span><span class="sxs-lookup"><span data-stu-id="d6dc2-135">Generic repository interface</span></span>

<span data-ttu-id="d6dc2-136">Cette rubrique et son échantillon d’application montrent l’implémentation la plus simple du modèle de référentiel, où un référentiel est créé pour chaque objet métier.</span><span class="sxs-lookup"><span data-stu-id="d6dc2-136">This topic and its sample app demonstrate the simplest implementation of the repository pattern, where one repository is created for each business object.</span></span> <span data-ttu-id="d6dc2-137">Si l’application grandit au-delà de quelques objets, une *interface de référentiel générique* peut réduire la quantité de code nécessaire pour implémenter le modèle de référentiel.</span><span class="sxs-lookup"><span data-stu-id="d6dc2-137">If the app grows beyond a few objects, a *generic repository interface* may reduce the amount of code required to implement the repository pattern.</span></span> <span data-ttu-id="d6dc2-138">Pour plus d’informations, consultez [DevIQ : modèle de référentiel : interface de référentiel générique](http://deviq.com/repository-pattern/).</span><span class="sxs-lookup"><span data-stu-id="d6dc2-138">For more information, see [DevIQ: Repository Pattern: Generic Repository Interface](http://deviq.com/repository-pattern/).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d6dc2-139">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="d6dc2-139">Additional resources</span></span>

* [<span data-ttu-id="d6dc2-140">DevIQ : modèle de référentiel</span><span class="sxs-lookup"><span data-stu-id="d6dc2-140">DevIQ: Repository Pattern</span></span>](https://deviq.com/repository-pattern/)
* [<span data-ttu-id="d6dc2-141">Injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="d6dc2-141">Dependency injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="d6dc2-142">Injection de dépendances dans les vues</span><span class="sxs-lookup"><span data-stu-id="d6dc2-142">Dependency injection into views</span></span>](xref:mvc/views/dependency-injection)
* [<span data-ttu-id="d6dc2-143">Injection de dépendances dans les contrôleurs</span><span class="sxs-lookup"><span data-stu-id="d6dc2-143">Dependency injection into controllers</span></span>](xref:mvc/controllers/dependency-injection)
* [<span data-ttu-id="d6dc2-144">Injection de dépendances dans les gestionnaires d’exigences</span><span class="sxs-lookup"><span data-stu-id="d6dc2-144">Dependency injection in requirement handlers</span></span>](xref:security/authorization/dependencyinjection)
* [<span data-ttu-id="d6dc2-145">Inversion des conteneurs de contrôle et modèle d’injection de dépendance</span><span class="sxs-lookup"><span data-stu-id="d6dc2-145">Inversion of Control Containers and the Dependency Injection Pattern</span></span>](https://www.martinfowler.com/articles/injection.html)
