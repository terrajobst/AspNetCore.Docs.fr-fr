---
title: Mise en route avec ASP.NET Core et Entity Framework 6
author: tdykstra
description: Cet article explique comment utiliser Entity Framework 6 dans une application ASP.NET Core.
ms.author: tdykstra
manager: wpickett
ms.date: 02/24/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/entity-framework-6
ms.openlocfilehash: 51445b8c110ad618aeb680148ccf4304a45ee16e
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2018
---
# <a name="getting-started-with-aspnet-core-and-entity-framework-6"></a><span data-ttu-id="12027-103">Mise en route avec ASP.NET Core et Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="12027-103">Getting started with ASP.NET Core and Entity Framework 6</span></span>

<span data-ttu-id="12027-104">Par [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), et [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="12027-104">By [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="12027-105">Cet article explique comment utiliser Entity Framework 6 dans une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="12027-105">This article shows how to use Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="overview"></a><span data-ttu-id="12027-106">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="12027-106">Overview</span></span>

<span data-ttu-id="12027-107">Pour utiliser Entity Framework 6, votre projet doit compiler par rapport à .NET Framework, comme Entity Framework 6 ne prend pas en charge le .NET Core.</span><span class="sxs-lookup"><span data-stu-id="12027-107">To use Entity Framework 6, your project has to compile against .NET Framework, as Entity Framework 6 does not support .NET Core.</span></span> <span data-ttu-id="12027-108">Si vous avez besoin de fonctionnalités d’inter-plateformes, vous devrez mettre à niveau vers [Entity Framework Core](https://docs.microsoft.com/ef/).</span><span class="sxs-lookup"><span data-stu-id="12027-108">If you need cross-platform features you will need to upgrade to [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>

<span data-ttu-id="12027-109">La méthode recommandée pour utiliser Entity Framework 6 dans une application ASP.NET Core consiste à placer le contexte EF6 et classes de modèle dans une bibliothèque de classes du projet qui cible le framework complet.</span><span class="sxs-lookup"><span data-stu-id="12027-109">The recommended way to use Entity Framework 6 in an ASP.NET Core application is to put the EF6 context and model classes in a class library project that targets the full framework.</span></span> <span data-ttu-id="12027-110">Ajoutez une référence à la bibliothèque de classes à partir du projet ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="12027-110">Add a reference to the class library from the ASP.NET Core project.</span></span> <span data-ttu-id="12027-111">Consultez l’exemple [solution Visual Studio avec des projets EF6 et ASP.NET Core](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).</span><span class="sxs-lookup"><span data-stu-id="12027-111">See the sample [Visual Studio solution with EF6 and ASP.NET Core projects](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).</span></span>

<span data-ttu-id="12027-112">Vous ne pouvez pas mettre un contexte EF6 dans un projet ASP.NET Core, car les projets .NET Core ne prend pas en charge toutes les fonctionnalités EF6 commandes telles que *Enable-Migrations* requièrent.</span><span class="sxs-lookup"><span data-stu-id="12027-112">You can't put an EF6 context in an ASP.NET Core project because .NET Core projects don't support all of the functionality that EF6 commands such as *Enable-Migrations* require.</span></span>

<span data-ttu-id="12027-113">Quel que soit le type de projet dans lequel vous localisez votre contexte EF6, EF6 uniquement les outils de ligne de commande fonctionnent avec un contexte EF6.</span><span class="sxs-lookup"><span data-stu-id="12027-113">Regardless of project type in which you locate your EF6 context, only EF6 command-line tools work with an EF6 context.</span></span> <span data-ttu-id="12027-114">Par exemple, `Scaffold-DbContext` est disponible uniquement dans Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="12027-114">For example, `Scaffold-DbContext` is only available in Entity Framework Core.</span></span> <span data-ttu-id="12027-115">Si vous devez à l’ingénierie à une base de données dans un modèle EF6, consultez [Code First pour une base de données](https://msdn.microsoft.com/jj200620).</span><span class="sxs-lookup"><span data-stu-id="12027-115">If you need to do reverse engineering of a database into an EF6 model, see [Code First to an Existing Database](https://msdn.microsoft.com/jj200620).</span></span>

## <a name="reference-full-framework-and-ef6-in-the-aspnet-core-project"></a><span data-ttu-id="12027-116">Framework complet de référence et EF6 dans le projet ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="12027-116">Reference full framework and EF6 in the ASP.NET Core project</span></span>

<span data-ttu-id="12027-117">Votre projet ASP.NET Core doit faire référence à .NET framework et EF6.</span><span class="sxs-lookup"><span data-stu-id="12027-117">Your ASP.NET Core project needs to reference .NET framework and EF6.</span></span> <span data-ttu-id="12027-118">Par exemple, le *.csproj* fichier de votre projet ASP.NET Core doit ressembler à l’exemple suivant (uniquement les parties pertinentes du fichier sont affichés).</span><span class="sxs-lookup"><span data-stu-id="12027-118">For example, the *.csproj* file of your ASP.NET Core project will look similar to the following example (only relevant parts of the file are shown).</span></span>

[!code-xml[](entity-framework-6/sample/MVCCore/MVCCore.csproj?range=3-9&highlight=2)]

<span data-ttu-id="12027-119">Si vous créez un nouveau projet, utilisez le **Application ASP.NET Core Web (.NET Framework)** modèle.</span><span class="sxs-lookup"><span data-stu-id="12027-119">If you’re creating a new project, use the **ASP.NET Core Web Application (.NET Framework)** template.</span></span>

## <a name="handle-connection-strings"></a><span data-ttu-id="12027-120">Traiter les chaînes de connexion</span><span class="sxs-lookup"><span data-stu-id="12027-120">Handle connection strings</span></span>

<span data-ttu-id="12027-121">Les outils de ligne de commande EF6 que vous allez utiliser dans le projet de bibliothèque de classes EF6 nécessitent un constructeur par défaut afin qu’ils ne peuvent instancier le contexte.</span><span class="sxs-lookup"><span data-stu-id="12027-121">The EF6 command-line tools that you'll use in the EF6 class library project require a default constructor so they can instantiate the context.</span></span> <span data-ttu-id="12027-122">Mais vous souhaiterez probablement spécifier la chaîne de connexion à utiliser dans le projet ASP.NET Core, auquel cas votre constructeur de contexte doit avoir un paramètre qui vous permet de passer la chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="12027-122">But you'll probably want to specify the connection string to use in the ASP.NET Core project, in which case your context constructor must have a parameter that lets you pass in the connection string.</span></span> <span data-ttu-id="12027-123">Voici un exemple.</span><span class="sxs-lookup"><span data-stu-id="12027-123">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContext.cs?name=snippet_Constructor)]

<span data-ttu-id="12027-124">Puisque le contexte EF6 n’a pas un constructeur sans paramètre, votre projet EF6 doit fournir une implémentation de [IDbContextFactory](https://msdn.microsoft.com/library/hh506876).</span><span class="sxs-lookup"><span data-stu-id="12027-124">Since your EF6 context doesn't have a parameterless constructor, your EF6 project has to provide an implementation of [IDbContextFactory](https://msdn.microsoft.com/library/hh506876).</span></span> <span data-ttu-id="12027-125">Les outils de ligne de commande EF6 recherchera et utiliser cette implémentation afin qu’ils ne peuvent instancier le contexte.</span><span class="sxs-lookup"><span data-stu-id="12027-125">The EF6 command-line tools will find and use that implementation so they can instantiate the context.</span></span> <span data-ttu-id="12027-126">Voici un exemple.</span><span class="sxs-lookup"><span data-stu-id="12027-126">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContextFactory.cs?name=snippet_IDbContextFactory)]

<span data-ttu-id="12027-127">Dans cet exemple de code, la `IDbContextFactory` implémentation transmet une chaîne de connexion codées en dur.</span><span class="sxs-lookup"><span data-stu-id="12027-127">In this sample code, the `IDbContextFactory` implementation passes in a hard-coded connection string.</span></span> <span data-ttu-id="12027-128">Il s’agit de la chaîne de connexion qui utilisent les outils de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="12027-128">This is the connection string that the command-line tools will use.</span></span> <span data-ttu-id="12027-129">Que vous souhaitez implémenter une stratégie pour vous assurer que la bibliothèque de classes utilise la même chaîne de connexion qui utilise de l’application appelante.</span><span class="sxs-lookup"><span data-stu-id="12027-129">You'll want to implement a strategy to ensure that the class library uses the same connection string that the calling application uses.</span></span> <span data-ttu-id="12027-130">Par exemple, vous pouvez obtenir la valeur d’une variable d’environnement dans les deux projets.</span><span class="sxs-lookup"><span data-stu-id="12027-130">For example, you could get the value from an environment variable in both projects.</span></span>

## <a name="set-up-dependency-injection-in-the-aspnet-core-project"></a><span data-ttu-id="12027-131">Configurer l’injection de dépendances dans le projet ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="12027-131">Set up dependency injection in the ASP.NET Core project</span></span>

<span data-ttu-id="12027-132">Dans le projet de base *Startup.cs* fichier, définissez le contexte EF6 pour l’injection de dépendance (DI) `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="12027-132">In the Core project's *Startup.cs* file, set up the EF6 context for dependency injection (DI) in `ConfigureServices`.</span></span> <span data-ttu-id="12027-133">Doivent porter les objets de contexte EF pour une durée de vie de chaque demande.</span><span class="sxs-lookup"><span data-stu-id="12027-133">EF context objects should be scoped for a per-request lifetime.</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

<span data-ttu-id="12027-134">Vous pouvez ensuite obtenir une instance du contexte de vos contrôleurs à l’aide de DI.</span><span class="sxs-lookup"><span data-stu-id="12027-134">You can then get an instance of the context in your controllers by using DI.</span></span> <span data-ttu-id="12027-135">Le code est similaire à ce que vous pourriez écrire pour un contexte EF :</span><span class="sxs-lookup"><span data-stu-id="12027-135">The code is similar to what you'd write for an EF Core context:</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Controllers/StudentsController.cs?name=snippet_ContextInController)]

## <a name="sample-application"></a><span data-ttu-id="12027-136">Exemple d’application</span><span class="sxs-lookup"><span data-stu-id="12027-136">Sample application</span></span>

<span data-ttu-id="12027-137">Pour un exemple d’application fonctionnel, consultez le [exemple de solution de Visual Studio](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) qui accompagne cet article.</span><span class="sxs-lookup"><span data-stu-id="12027-137">For a working sample application, see the [sample Visual Studio solution](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) that accompanies this article.</span></span>

<span data-ttu-id="12027-138">Cet exemple peut être créé à partir de zéro par les étapes suivantes dans Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="12027-138">This sample can be created from scratch by the following steps in Visual Studio:</span></span>

* <span data-ttu-id="12027-139">Créer une solution.</span><span class="sxs-lookup"><span data-stu-id="12027-139">Create a solution.</span></span>

* <span data-ttu-id="12027-140">**Ajouter un nouveau projet > Web > ASP.NET Core Web Application (.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="12027-140">**Add New Project > Web > ASP.NET Core Web Application (.NET Framework)**</span></span>

* <span data-ttu-id="12027-141">**Ajouter un nouveau projet > de bureau Windows classique > classe de bibliothèque (.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="12027-141">**Add New Project > Windows Classic Desktop > Class Library (.NET Framework)**</span></span>

* <span data-ttu-id="12027-142">Dans **Package Manager Console** (PMC) pour les deux projets, exécutez la commande `Install-Package Entityframework`.</span><span class="sxs-lookup"><span data-stu-id="12027-142">In **Package Manager Console** (PMC) for both projects, run the command `Install-Package Entityframework`.</span></span>

* <span data-ttu-id="12027-143">Dans le projet de bibliothèque de classes, créer des classes de modèle de données et une classe de contexte et l’implémentation de `IDbContextFactory`.</span><span class="sxs-lookup"><span data-stu-id="12027-143">In the class library project, create data model classes and a context class, and an implementation of `IDbContextFactory`.</span></span>

* <span data-ttu-id="12027-144">Dans PMC pour le projet de bibliothèque de classes, exécutez les commandes `Enable-Migrations` et `Add-Migration Initial`.</span><span class="sxs-lookup"><span data-stu-id="12027-144">In PMC for the class library project, run the commands `Enable-Migrations` and `Add-Migration Initial`.</span></span> <span data-ttu-id="12027-145">Si vous avez défini le projet ASP.NET Core comme projet de démarrage, ajoutez `-StartupProjectName EF6` à ces commandes.</span><span class="sxs-lookup"><span data-stu-id="12027-145">If you have set the ASP.NET Core project as the startup project, add `-StartupProjectName EF6` to these commands.</span></span>

* <span data-ttu-id="12027-146">Dans le projet de base, ajoutez une référence de projet au projet de bibliothèque de classes.</span><span class="sxs-lookup"><span data-stu-id="12027-146">In the Core project, add a project reference to the class library project.</span></span>

* <span data-ttu-id="12027-147">Dans le projet de base, dans *Startup.cs*, enregistrer le contexte pour DI.</span><span class="sxs-lookup"><span data-stu-id="12027-147">In the Core project, in *Startup.cs*, register the context for DI.</span></span>

* <span data-ttu-id="12027-148">Dans le projet de base, dans *appsettings.json*, ajoutez la chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="12027-148">In the Core project, in *appsettings.json*, add the connection string.</span></span>

* <span data-ttu-id="12027-149">Dans le projet de base, ajoutez un contrôleur et les vues pour vérifier que vous pouvez lire et écrire des données.</span><span class="sxs-lookup"><span data-stu-id="12027-149">In the Core project, add a controller and view(s) to verify that you can read and write data.</span></span> <span data-ttu-id="12027-150">(Notez que la structure de base de ASP.NET MVC ne fonctionne pas avec le contexte EF6 référencé à partir de la bibliothèque de classes).</span><span class="sxs-lookup"><span data-stu-id="12027-150">(Note that ASP.NET Core MVC scaffolding won't work with the EF6 context referenced from the class library.)</span></span>

## <a name="summary"></a><span data-ttu-id="12027-151">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="12027-151">Summary</span></span>

<span data-ttu-id="12027-152">Cet article a fourni des conseils de base pour l’utilisation d’Entity Framework 6 dans une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="12027-152">This article has provided basic guidance for using Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="12027-153">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="12027-153">Additional Resources</span></span>

* [<span data-ttu-id="12027-154">Entity Framework - Configuration basée sur le Code</span><span class="sxs-lookup"><span data-stu-id="12027-154">Entity Framework - Code-Based Configuration</span></span>](https://msdn.microsoft.com/data/jj680699.aspx)
