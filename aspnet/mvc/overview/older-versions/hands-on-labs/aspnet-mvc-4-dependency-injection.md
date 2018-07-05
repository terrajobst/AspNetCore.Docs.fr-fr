---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: Injection de dépendances d’ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: 'Remarque : Ce laboratoire pratique suppose que vous avez une connaissance élémentaire des filtres ASP.NET MVC et ASP.NET MVC 4. Si vous n’avez pas utilisé les filtres ASP.NET MVC 4 avant, nous rec...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 715444a6fbf491d7b99918294cfd2d0d0216cd09
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388042"
---
# <a name="aspnet-mvc-4-dependency-injection"></a><span data-ttu-id="17ec5-104">Injection de dépendances d’ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="17ec5-104">ASP.NET MVC 4 Dependency Injection</span></span>

<span data-ttu-id="17ec5-105">par [Web Camps Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="17ec5-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="17ec5-106">Télécharger le Kit de formation de Web Camps</span><span class="sxs-lookup"><span data-stu-id="17ec5-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="17ec5-107">Ce laboratoire pratique suppose que vous avez une connaissance élémentaire de **ASP.NET MVC** et **filtres ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="17ec5-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC** and **ASP.NET MVC 4 filters**.</span></span> <span data-ttu-id="17ec5-108">Si vous n’avez pas utilisé **filtres ASP.NET MVC 4** auparavant, nous vous recommandons de consulter **filtres d’Action ASP.NET MVC personnalisée** les ateliers pratiques.</span><span class="sxs-lookup"><span data-stu-id="17ec5-108">If you have not used **ASP.NET MVC 4 filters** before, we recommend you to go over **ASP.NET MVC Custom Action Filters** Hands-on Lab.</span></span>

> [!NOTE]
> <span data-ttu-id="17ec5-109">Tous les exemples de code et extraits de code sont inclus dans le Kit de formation Camps Web, disponible à partir d’à l’adresse [les versions de Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="17ec5-109">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="17ec5-110">Le projet spécifique à ce laboratoire est disponible à l’adresse [Injection de dépendances d’ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="17ec5-110">The project specific to this lab is available at [ASP.NET MVC 4 Dependency Injection](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).</span></span>

<span data-ttu-id="17ec5-111">Dans **la programmation orientée objet** paradigme, objets fonctionnent conjointement dans un modèle de collaboration où il existe des contributeurs et les consommateurs.</span><span class="sxs-lookup"><span data-stu-id="17ec5-111">In **Object Oriented Programming** paradigm, objects work together in a collaboration model where there are contributors and consumers.</span></span> <span data-ttu-id="17ec5-112">Naturellement, ce modèle de communication génère des dépendances entre les objets et composants, devient difficile à gérer la complexité augmente.</span><span class="sxs-lookup"><span data-stu-id="17ec5-112">Naturally, this communication model generates dependencies between objects and components, becoming difficult to manage when complexity increases.</span></span>

<span data-ttu-id="17ec5-113">![Classe de dépendances et la complexité du modèle](aspnet-mvc-4-dependency-injection/_static/image1.png "classe les dépendances et la complexité du modèle")</span><span class="sxs-lookup"><span data-stu-id="17ec5-113">![Class dependencies and model complexity](aspnet-mvc-4-dependency-injection/_static/image1.png "Class dependencies and model complexity")</span></span>

<span data-ttu-id="17ec5-114">*Dépendances de la classe et la complexité du modèle*</span><span class="sxs-lookup"><span data-stu-id="17ec5-114">*Class dependencies and model complexity*</span></span>

<span data-ttu-id="17ec5-115">Vous avez probablement entendu parler du **modèle de fabrique** et la séparation entre l’interface et l’implémentation à l’aide de services, où les objets de client sont souvent responsables pour l’emplacement du service.</span><span class="sxs-lookup"><span data-stu-id="17ec5-115">You have probably heard about the **Factory Pattern** and the separation between the interface and the implementation using services, where the client objects are often responsible for service location.</span></span>

<span data-ttu-id="17ec5-116">Le modèle d’Injection de dépendance est une implémentation particulière d’Inversion de contrôle.</span><span class="sxs-lookup"><span data-stu-id="17ec5-116">The Dependency Injection pattern is a particular implementation of Inversion of Control.</span></span> <span data-ttu-id="17ec5-117">**L’inversion de contrôle (IoC)** signifie que les objets ne créent pas d’autres objets sur lesquels elles s’appuient pour faire leur travail.</span><span class="sxs-lookup"><span data-stu-id="17ec5-117">**Inversion of Control (IoC)** means that objects do not create other objects on which they rely to do their work.</span></span> <span data-ttu-id="17ec5-118">Au lieu de cela, ils obtiennent les objets dont ils ont besoin d’une source externe (par exemple, un fichier de configuration xml).</span><span class="sxs-lookup"><span data-stu-id="17ec5-118">Instead, they get the objects that they need from an outside source (for example, an xml configuration file).</span></span>

<span data-ttu-id="17ec5-119">**L’Injection de dépendance (DI)** signifie qu’il est fait sans l’intervention de l’objet, généralement par un composant d’infrastructure qui transmet les paramètres de constructeur et définir les propriétés.</span><span class="sxs-lookup"><span data-stu-id="17ec5-119">**Dependency Injection (DI)** means that this is done without the object intervention, usually by a framework component that passes constructor parameters and set properties.</span></span>

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a><span data-ttu-id="17ec5-120">Le modèle de conception de dépendance (DI) de l’injection de code</span><span class="sxs-lookup"><span data-stu-id="17ec5-120">The Dependency Injection (DI) Design Pattern</span></span>

<span data-ttu-id="17ec5-121">À un niveau élevé, l’objectif de l’Injection de dépendance qui est une classe de client (par exemple, *le dsmessages*) a besoin d’un élément qui satisfait à une interface (par exemple, *IClub*).</span><span class="sxs-lookup"><span data-stu-id="17ec5-121">At a high level, the goal of Dependency Injection is that a client class (e.g. *the golfer*) needs something that satisfies an interface (e.g. *IClub*).</span></span> <span data-ttu-id="17ec5-122">Il ne soucie pas ce qui est le type concret (par exemple, *WoodClub, IronClub, WedgeClub* ou *PutterClub*), qu’il veut que quelqu'un d’autre pour qui gérer (par exemple, une bonne *plateau*).</span><span class="sxs-lookup"><span data-stu-id="17ec5-122">It doesn't care what the concrete type is (e.g. *WoodClub, IronClub, WedgeClub* or *PutterClub*), it wants someone else to handle that (e.g. a good *caddy*).</span></span> <span data-ttu-id="17ec5-123">Le résolveur de dépendance dans ASP.NET MVC peut vous permettre d’inscrire votre logique de dépendance vers un autre emplacement (par exemple, un conteneur ou un *sac de trèfle*).</span><span class="sxs-lookup"><span data-stu-id="17ec5-123">The Dependency Resolver in ASP.NET MVC can allow you to register your dependency logic somewhere else (e.g. a container or a *bag of clubs*).</span></span>

<span data-ttu-id="17ec5-124">![Diagramme de l’Injection de dépendance](aspnet-mvc-4-dependency-injection/_static/image2.png "illustration de l’Injection de dépendances")</span><span class="sxs-lookup"><span data-stu-id="17ec5-124">![Dependency Injection diagram](aspnet-mvc-4-dependency-injection/_static/image2.png "Dependency Injection illustration")</span></span>

<span data-ttu-id="17ec5-125">*Injection de dépendances - analogie de Golf*</span><span class="sxs-lookup"><span data-stu-id="17ec5-125">*Dependency Injection - Golf analogy*</span></span>

<span data-ttu-id="17ec5-126">Les avantages de l’utilisation d’injection de dépendance et Inversion de contrôle sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="17ec5-126">The advantages of using Dependency Injection pattern and Inversion of Control are the following:</span></span>

- <span data-ttu-id="17ec5-127">Réduit les couplages de classe</span><span class="sxs-lookup"><span data-stu-id="17ec5-127">Reduces class coupling</span></span>
- <span data-ttu-id="17ec5-128">Augmente la réutilisation de code</span><span class="sxs-lookup"><span data-stu-id="17ec5-128">Increases code reusing</span></span>
- <span data-ttu-id="17ec5-129">Améliore la maintenabilité du code</span><span class="sxs-lookup"><span data-stu-id="17ec5-129">Improves code maintainability</span></span>
- <span data-ttu-id="17ec5-130">Améliore le test des applications</span><span class="sxs-lookup"><span data-stu-id="17ec5-130">Improves application testing</span></span>

> [!NOTE]
> <span data-ttu-id="17ec5-131">L’Injection de dépendances est parfois comparée avec le modèle de Design Factory abstraite, mais il existe une légère différence entre les deux approches.</span><span class="sxs-lookup"><span data-stu-id="17ec5-131">Dependency Injection is sometimes compared with Abstract Factory Design Pattern, but there is a slight difference between both approaches.</span></span> <span data-ttu-id="17ec5-132">L’injection de dépendances a une infrastructure fonctionne derrière pour résoudre les dépendances en appelant les fabriques et les services inscrits.</span><span class="sxs-lookup"><span data-stu-id="17ec5-132">DI has a Framework working behind to solve dependencies by calling the factories and the registered services.</span></span>


<span data-ttu-id="17ec5-133">Maintenant que vous comprenez le modèle d’Injection de dépendance, vous allez apprendre tout au long de ce laboratoire pour l’appliquer dans ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="17ec5-133">Now that you understand the Dependency Injection Pattern, you will learn throughout this lab how to apply it in ASP.NET MVC 4.</span></span> <span data-ttu-id="17ec5-134">Vous commencerez à l’aide de l’Injection de dépendances dans le **contrôleurs** d’inclure un service d’accès de base de données.</span><span class="sxs-lookup"><span data-stu-id="17ec5-134">You will start using Dependency Injection in the **Controllers** to include a database access service.</span></span> <span data-ttu-id="17ec5-135">Ensuite, vous appliquerez l’Injection de dépendances pour le **vues** pour consommer un service et afficher des informations.</span><span class="sxs-lookup"><span data-stu-id="17ec5-135">Next, you will apply Dependency Injection to the **Views** to consume a service and show information.</span></span> <span data-ttu-id="17ec5-136">Enfin, vous allez étendre l’injection de dépendances pour les filtres ASP.NET MVC 4, injecter un filtre d’action personnalisé dans la solution.</span><span class="sxs-lookup"><span data-stu-id="17ec5-136">Finally, you will extend the DI to ASP.NET MVC 4 Filters, injecting a custom action filter in the solution.</span></span>

<span data-ttu-id="17ec5-137">Dans cet atelier pratique, vous allez apprendre comment :</span><span class="sxs-lookup"><span data-stu-id="17ec5-137">In this Hands-on Lab, you will learn how to:</span></span>

- <span data-ttu-id="17ec5-138">Intégrer d’ASP.NET MVC 4 avec Unity pour l’Injection de dépendances à l’aide de Packages NuGet</span><span class="sxs-lookup"><span data-stu-id="17ec5-138">Integrate ASP.NET MVC 4 with Unity for Dependency Injection using NuGet Packages</span></span>
- <span data-ttu-id="17ec5-139">Utiliser l’Injection de dépendance à l’intérieur d’un contrôleur ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="17ec5-139">Use Dependency Injection inside an ASP.NET MVC Controller</span></span>
- <span data-ttu-id="17ec5-140">Utiliser l’Injection de dépendance à l’intérieur d’une vue ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="17ec5-140">Use Dependency Injection inside an ASP.NET MVC View</span></span>
- <span data-ttu-id="17ec5-141">Utiliser l’Injection de dépendance à l’intérieur d’un filtre d’Action ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="17ec5-141">Use Dependency Injection inside an ASP.NET MVC Action Filter</span></span>

> [!NOTE]
> <span data-ttu-id="17ec5-142">Ce laboratoire est à l’aide de Unity.Mvc3 le NuGet Package pour la résolution des dépendances, mais il est possible de s’adapter à n’importe quel Framework de l’Injection de dépendance pour travailler avec ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="17ec5-142">This Lab is using Unity.Mvc3 NuGet Package for dependency resolution, but it is possible to adapt any Dependency Injection Framework to work with ASP.NET MVC 4.</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="17ec5-143">Prérequis</span><span class="sxs-lookup"><span data-stu-id="17ec5-143">Prerequisites</span></span>

<span data-ttu-id="17ec5-144">Vous devez disposer des éléments suivants pour terminer ce laboratoire :</span><span class="sxs-lookup"><span data-stu-id="17ec5-144">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="17ec5-145">[Microsoft Visual Studio Express 2012 pour Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou supérieure (lire [annexe A](#AppendixA) pour obtenir des instructions sur la façon d’installer).</span><span class="sxs-lookup"><span data-stu-id="17ec5-145">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="17ec5-146">Installation</span><span class="sxs-lookup"><span data-stu-id="17ec5-146">Setup</span></span>

<span data-ttu-id="17ec5-147">**L’installation des extraits de Code**</span><span class="sxs-lookup"><span data-stu-id="17ec5-147">**Installing Code Snippets**</span></span>

<span data-ttu-id="17ec5-148">Pour des raisons pratiques, une grande partie du code que vous gérez le long de ce laboratoire est disponible en tant que les extraits de code Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="17ec5-148">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="17ec5-149">Pour installer les extraits de code exécuter **.\Source\Setup\CodeSnippets.vsi** fichier.</span><span class="sxs-lookup"><span data-stu-id="17ec5-149">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="17ec5-150">Si vous n’êtes pas familiarisé avec les extraits de Code Visual Studio et que vous souhaitiez savoir comment les utiliser, vous pouvez faire référence à l’annexe de ce document &quot; [extraits de Code à l’aide d’annexe b :](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="17ec5-150">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="17ec5-151">Exercices</span><span class="sxs-lookup"><span data-stu-id="17ec5-151">Exercises</span></span>

<span data-ttu-id="17ec5-152">Ce laboratoire pratique est constitué par les exercices suivants :</span><span class="sxs-lookup"><span data-stu-id="17ec5-152">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="17ec5-153">Exercice 1 : Injection d’un contrôleur</span><span class="sxs-lookup"><span data-stu-id="17ec5-153">Exercise 1: Injecting a Controller</span></span>](#Exercise1)
2. [<span data-ttu-id="17ec5-154">Exercice 2 : Injecter une vue</span><span class="sxs-lookup"><span data-stu-id="17ec5-154">Exercise 2: Injecting a View</span></span>](#Exercise2)
3. [<span data-ttu-id="17ec5-155">Exercice 3 : Injection de filtres</span><span class="sxs-lookup"><span data-stu-id="17ec5-155">Exercise 3: Injecting Filters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="17ec5-156">Chaque exercice est accompagné par un **fin** dossier contenant la solution obtenue, vous devez obtenir après avoir effectué les exercices.</span><span class="sxs-lookup"><span data-stu-id="17ec5-156">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="17ec5-157">Si vous avez besoin d’aide supplémentaire sur l’utilisation via les exercices, vous pouvez utiliser cette solution comme guide.</span><span class="sxs-lookup"><span data-stu-id="17ec5-157">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="17ec5-158">Durée estimée pour effectuer ce laboratoire : **30 minutes**.</span><span class="sxs-lookup"><span data-stu-id="17ec5-158">Estimated time to complete this lab: **30 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a><span data-ttu-id="17ec5-159">Exercice 1 : Injection d’un contrôleur</span><span class="sxs-lookup"><span data-stu-id="17ec5-159">Exercise 1: Injecting a Controller</span></span>

<span data-ttu-id="17ec5-160">Dans cet exercice, vous allez apprendre à utiliser l’Injection de dépendances dans ASP.NET MVC Controllers en intégrant Unity à l’aide d’un NuGet Package.</span><span class="sxs-lookup"><span data-stu-id="17ec5-160">In this exercise, you will learn how to use Dependency Injection in ASP.NET MVC Controllers by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="17ec5-161">Pour cette raison, vous inclurez des services dans vos contrôleurs MvcMusicStore à séparer la logique de l’accès aux données.</span><span class="sxs-lookup"><span data-stu-id="17ec5-161">For that reason, you will include services into your MvcMusicStore controllers to separate the logic from the data access.</span></span> <span data-ttu-id="17ec5-162">Les services créera une nouvelle dépendance dans le constructeur de contrôleur, qui est résolu à l’aide de l’Injection de dépendances à l’aide de **Unity**.</span><span class="sxs-lookup"><span data-stu-id="17ec5-162">The services will create a new dependency in the controller constructor, which will be resolved using Dependency Injection with the help of **Unity**.</span></span>

<span data-ttu-id="17ec5-163">Cette approche est vous montrer comment générer moins des applications couplées sont plus flexibles et plus facile à maintenir et à tester.</span><span class="sxs-lookup"><span data-stu-id="17ec5-163">This approach will show you how to generate less coupled applications, which are more flexible and easier to maintain and test.</span></span> <span data-ttu-id="17ec5-164">Vous allez également apprendre à intégrer ASP.NET MVC avec Unity.</span><span class="sxs-lookup"><span data-stu-id="17ec5-164">You will also learn how to integrate ASP.NET MVC with Unity.</span></span>

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a><span data-ttu-id="17ec5-165">Sur le Service de StoreManager</span><span class="sxs-lookup"><span data-stu-id="17ec5-165">About StoreManager Service</span></span>

<span data-ttu-id="17ec5-166">Le Store de musique MVC fourni dans la solution begin maintenant inclut un service qui gère les données de Store contrôleur nommées **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="17ec5-166">The MVC Music Store provided in the begin solution now includes a service that manages the Store Controller data named **StoreService**.</span></span> <span data-ttu-id="17ec5-167">Vous trouverez ci-dessous l’implémentation du Service de Store.</span><span class="sxs-lookup"><span data-stu-id="17ec5-167">Below you will find the Store Service implementation.</span></span> <span data-ttu-id="17ec5-168">Notez que toutes les méthodes retournent des entités du modèle.</span><span class="sxs-lookup"><span data-stu-id="17ec5-168">Note that all the methods return Model entities.</span></span>

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

<span data-ttu-id="17ec5-169">**StoreController** à partir du début de la solution consomme **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="17ec5-169">**StoreController** from the begin solution now consumes **StoreService**.</span></span> <span data-ttu-id="17ec5-170">Toutes les références de données ont été supprimées à partir de **StoreController**et il est désormais possible de modifier le fournisseur d’accès aux données en cours sans modifier n’importe quelle méthode consomme **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="17ec5-170">All the data references were removed from **StoreController**, and now possible to modify the current data access provider without changing any method that consumes **StoreService**.</span></span>

<span data-ttu-id="17ec5-171">Vous trouverez ci-dessous la **StoreController** implémentation a une dépendance avec **StoreService** dans le constructeur de classe.</span><span class="sxs-lookup"><span data-stu-id="17ec5-171">You will find below that the **StoreController** implementation has a dependency with **StoreService** inside the class constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="17ec5-172">La dépendance introduite dans cet exercice est liée à **Inversion de contrôle** (IoC).</span><span class="sxs-lookup"><span data-stu-id="17ec5-172">The dependency introduced in this exercise is related to **Inversion of Control** (IoC).</span></span>
> 
> <span data-ttu-id="17ec5-173">Le **StoreController** constructeur de classe reçoit un **IStoreService** paramètre de type, ce qui est essentiel d’effectuer des appels de service à l’intérieur de la classe.</span><span class="sxs-lookup"><span data-stu-id="17ec5-173">The **StoreController** class constructor receives an **IStoreService** type parameter, which is essential to perform service calls from inside the class.</span></span> <span data-ttu-id="17ec5-174">Toutefois, **StoreController** n’implémente pas le constructeur par défaut (sans aucun paramètre) qui n’importe quel contrôleur doit comporter pour fonctionner avec ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="17ec5-174">However, **StoreController** does not implement the default constructor (with no parameters) that any controller must have to work with ASP.NET MVC.</span></span>
> 
> <span data-ttu-id="17ec5-175">Pour résoudre la dépendance, le contrôleur doit être créée par une fabrique abstraite (une classe qui retourne un objet du type spécifié).</span><span class="sxs-lookup"><span data-stu-id="17ec5-175">To resolve the dependency, the controller has to be created by an abstract factory (a class that returns any object of the specified type).</span></span>


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="17ec5-176">Vous obtiendrez une erreur lors de la classe tente de créer le StoreController sans envoyer l’objet de service, car il n’existe aucun constructeur sans paramètre déclaré.</span><span class="sxs-lookup"><span data-stu-id="17ec5-176">You will get an error when the class tries to create the StoreController without sending the service object, as there is no parameterless constructor declared.</span></span>


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a><span data-ttu-id="17ec5-177">Tâche 1 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="17ec5-177">Task 1 - Running the Application</span></span>

<span data-ttu-id="17ec5-178">Dans cette tâche, vous allez exécuter l’application Begin, qui inclut le service dans le contrôleur de Store qui sépare l’accès aux données à partir de la logique d’application.</span><span class="sxs-lookup"><span data-stu-id="17ec5-178">In this task, you will run the Begin application, which includes the service into the Store Controller that separates the data access from the application logic.</span></span>

<span data-ttu-id="17ec5-179">Lorsque vous exécutez l’application, vous recevrez une exception, comme le service du contrôleur n’est pas passé en tant que paramètre par défaut :</span><span class="sxs-lookup"><span data-stu-id="17ec5-179">When running the application, you will receive an exception, as the controller service is not passed as a parameter by default:</span></span>

1. <span data-ttu-id="17ec5-180">Ouvrez le **commencer** solution situé dans **Controller\Begin injection Source\Ex01**.</span><span class="sxs-lookup"><span data-stu-id="17ec5-180">Open the **Begin** solution located in **Source\Ex01-Injecting Controller\Begin**.</span></span>

   1. <span data-ttu-id="17ec5-181">Vous devrez télécharger certains packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="17ec5-181">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="17ec5-182">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="17ec5-182">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="17ec5-183">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="17ec5-183">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="17ec5-184">Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="17ec5-184">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="17ec5-185">Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="17ec5-185">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="17ec5-186">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="17ec5-186">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="17ec5-187">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="17ec5-187">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="17ec5-188">Appuyez sur **Ctrl + F5** pour exécuter l’application sans débogage.</span><span class="sxs-lookup"><span data-stu-id="17ec5-188">Press **Ctrl + F5** to run the application without debugging.</span></span> <span data-ttu-id="17ec5-189">Vous obtiendrez le message d’erreur &quot; **aucun constructeur sans paramètre défini pour cet objet**&quot;:</span><span class="sxs-lookup"><span data-stu-id="17ec5-189">You will get the error message &quot;**No parameterless constructor defined for this object**&quot;:</span></span>

    <span data-ttu-id="17ec5-190">![Erreur lors de l’exécution d’Application ASP.NET MVC commencer](aspnet-mvc-4-dependency-injection/_static/image3.png "erreur lors de l’exécution d’Application ASP.NET MVC commencer")</span><span class="sxs-lookup"><span data-stu-id="17ec5-190">![Error while running ASP.NET MVC Begin Application](aspnet-mvc-4-dependency-injection/_static/image3.png "Error while running ASP.NET MVC Begin Application")</span></span>

    <span data-ttu-id="17ec5-191">*Erreur lors de l’exécution d’Application ASP.NET MVC commencer*</span><span class="sxs-lookup"><span data-stu-id="17ec5-191">*Error while running ASP.NET MVC Begin Application*</span></span>
3. <span data-ttu-id="17ec5-192">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="17ec5-192">Close the browser.</span></span>

<span data-ttu-id="17ec5-193">Dans les étapes suivantes, vous allez travailler sur la Solution du Store musique pour injecter la dépendance qu'a besoin de ce contrôleur.</span><span class="sxs-lookup"><span data-stu-id="17ec5-193">In the following steps you will work on the Music Store Solution to inject the dependency this controller needs.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a><span data-ttu-id="17ec5-194">Tâche 2 - y compris de Unity dans MvcMusicStore Solution</span><span class="sxs-lookup"><span data-stu-id="17ec5-194">Task 2 - Including Unity into MvcMusicStore Solution</span></span>

<span data-ttu-id="17ec5-195">Dans cette tâche, vous inclurez **Unity.Mvc3** NuGet Package à la solution.</span><span class="sxs-lookup"><span data-stu-id="17ec5-195">In this task, you will include **Unity.Mvc3** NuGet Package to the solution.</span></span>

> [!NOTE]
> <span data-ttu-id="17ec5-196">Unity.Mvc3 package a été conçu pour ASP.NET MVC 3, mais il est entièrement compatible avec ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="17ec5-196">Unity.Mvc3 package was designed for ASP.NET MVC 3, but it is fully compatible with ASP.NET MVC 4.</span></span>
> 
> <span data-ttu-id="17ec5-197">Unity est un conteneur d’injection de dépendance de légère et extensible avec prise en charge facultative par exemple et de types.</span><span class="sxs-lookup"><span data-stu-id="17ec5-197">Unity is a lightweight, extensible dependency injection container with optional support for instance and type interception.</span></span> <span data-ttu-id="17ec5-198">Il est un conteneur à usage général pour une utilisation dans n’importe quel type d’application .NET.</span><span class="sxs-lookup"><span data-stu-id="17ec5-198">It is a general-purpose container for use in any type of .NET application.</span></span> <span data-ttu-id="17ec5-199">Il fournit toutes les fonctionnalités courantes trouvées dans les mécanismes de l’injection de dépendance, y compris : la création d’objets, l’abstraction des exigences par spécification de dépendances au démarrage et de flexibilité, en différant la configuration du composant au conteneur.</span><span class="sxs-lookup"><span data-stu-id="17ec5-199">It provides all the common features found in dependency injection mechanisms including: object creation, abstraction of requirements by specifying dependencies at runtime and flexibility, by deferring the component configuration to the container.</span></span>


1. <span data-ttu-id="17ec5-200">Installer **Unity.Mvc3** NuGet Package dans le **MvcMusicStore** projet.</span><span class="sxs-lookup"><span data-stu-id="17ec5-200">Install **Unity.Mvc3** NuGet Package in the **MvcMusicStore** project.</span></span> <span data-ttu-id="17ec5-201">Pour ce faire, ouvrez le **Console du Gestionnaire de Package** de **vue** | **Windows autres**.</span><span class="sxs-lookup"><span data-stu-id="17ec5-201">To do this, open the **Package Manager Console** from **View** | **Other Windows**.</span></span>
2. <span data-ttu-id="17ec5-202">Exécutez la commande suivante.</span><span class="sxs-lookup"><span data-stu-id="17ec5-202">Run the following command.</span></span>

    <span data-ttu-id="17ec5-203">PMC</span><span class="sxs-lookup"><span data-stu-id="17ec5-203">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    <span data-ttu-id="17ec5-204">![Installer le Package NuGet de Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "l’installation de Package NuGet de Unity.Mvc3")</span><span class="sxs-lookup"><span data-stu-id="17ec5-204">![Installing Unity.Mvc3 NuGet Package](aspnet-mvc-4-dependency-injection/_static/image4.png "Installing Unity.Mvc3 NuGet Package")</span></span>

    <span data-ttu-id="17ec5-205">*Installation du Package NuGet Unity.Mvc3*</span><span class="sxs-lookup"><span data-stu-id="17ec5-205">*Installing Unity.Mvc3 NuGet Package*</span></span>
3. <span data-ttu-id="17ec5-206">Une fois le **Unity.Mvc3** package est installé, d’Explorer les fichiers et dossiers, il ajoute automatiquement afin de simplifier la configuration de Unity.</span><span class="sxs-lookup"><span data-stu-id="17ec5-206">Once the **Unity.Mvc3** package is installed, explore the files and folders it automatically adds in order to simplify Unity configuration.</span></span>

    <span data-ttu-id="17ec5-207">![Package Unity.Mvc3 installé](aspnet-mvc-4-dependency-injection/_static/image5.png "package Unity.Mvc3 installé")</span><span class="sxs-lookup"><span data-stu-id="17ec5-207">![Unity.Mvc3 package installed](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 package installed")</span></span>

    <span data-ttu-id="17ec5-208">*Package Unity.Mvc3 installé*</span><span class="sxs-lookup"><span data-stu-id="17ec5-208">*Unity.Mvc3 package installed*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a><span data-ttu-id="17ec5-209">Tâche 3 : inscription de Unity dans Global.asax.cs Application\_Démarrer</span><span class="sxs-lookup"><span data-stu-id="17ec5-209">Task 3 - Registering Unity in Global.asax.cs Application\_Start</span></span>

<span data-ttu-id="17ec5-210">Dans cette tâche, vous mettrez à jour la **Application\_Démarrer** méthode situé dans **Global.asax.cs** pour appeler l’initialiseur de programme d’amorçage de Unity et ensuite, mettez à jour le fichier de programme d’amorçage inscrit le Service et le contrôleur, vous allez utiliser l’Injection de dépendance.</span><span class="sxs-lookup"><span data-stu-id="17ec5-210">In this task, you will update the **Application\_Start** method located in **Global.asax.cs** to call the Unity Bootstrapper initializer and then, update the Bootstrapper file registering the Service and Controller you will use for Dependency Injection.</span></span>

1. <span data-ttu-id="17ec5-211">Maintenant, vous raccorder le programme d’amorçage qui est le fichier qui initialise le conteneur Unity et résolveur de dépendance.</span><span class="sxs-lookup"><span data-stu-id="17ec5-211">Now, you will hook up the Bootstrapper which is the file that initializes the Unity container and Dependency Resolver.</span></span> <span data-ttu-id="17ec5-212">Pour ce faire, ouvrez **Global.asax.cs** et ajoutez le code en surbrillance suivant au sein de la **Application\_Démarrer** (méthode).</span><span class="sxs-lookup"><span data-stu-id="17ec5-212">To do this, open **Global.asax.cs** and add the following highlighted code within the **Application\_Start** method.</span></span>

    <span data-ttu-id="17ec5-213">(Code Snippet - *Lab de l’Injection de dépendance ASP.NET - Ex01 - initialiser Unity*)</span><span class="sxs-lookup"><span data-stu-id="17ec5-213">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Initialize Unity*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. <span data-ttu-id="17ec5-214">Ouvrez **Bootstrapper.cs** fichier.</span><span class="sxs-lookup"><span data-stu-id="17ec5-214">Open **Bootstrapper.cs** file.</span></span>
3. <span data-ttu-id="17ec5-215">Inclure des espaces de noms suivants : **MvcMusicStore.Services** et **MusicStore.Controllers**.</span><span class="sxs-lookup"><span data-stu-id="17ec5-215">Include the following namespaces: **MvcMusicStore.Services** and **MusicStore.Controllers**.</span></span>

    <span data-ttu-id="17ec5-216">(Code Snippet - *ASP.NET dépendance Injection Lab - Ex01 - programme d’amorçage Ajout d’espaces de noms*)</span><span class="sxs-lookup"><span data-stu-id="17ec5-216">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. <span data-ttu-id="17ec5-217">Remplacez **BuildUnityContainer** de contenu par le code suivant qui enregistre le contrôleur de Store et Service de Store de la méthode.</span><span class="sxs-lookup"><span data-stu-id="17ec5-217">Replace **BuildUnityContainer** method's content with the following code that registers Store Controller and Store Service.</span></span>

    <span data-ttu-id="17ec5-218">(Code Snippet - *ASP.NET dépendance Injection Lab - Ex01 - Register Store contrôleur et Service*)</span><span class="sxs-lookup"><span data-stu-id="17ec5-218">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Register Store Controller and Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="17ec5-219">Tâche 4 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="17ec5-219">Task 4 - Running the Application</span></span>

<span data-ttu-id="17ec5-220">Dans cette tâche, vous allez exécuter l’application pour vérifier qu’il peut maintenant être chargé après avoir inclus Unity.</span><span class="sxs-lookup"><span data-stu-id="17ec5-220">In this task, you will run the application to verify that it can now be loaded after including Unity.</span></span>

1. <span data-ttu-id="17ec5-221">Appuyez sur **F5** pour exécuter l’application, l’application doit maintenant se charge sans afficher de message d’erreur.</span><span class="sxs-lookup"><span data-stu-id="17ec5-221">Press **F5** to run the application, the application should now load without showing any error message.</span></span>

    <span data-ttu-id="17ec5-222">![Application en cours d’exécution avec l’Injection de dépendance](aspnet-mvc-4-dependency-injection/_static/image6.png "Application en cours d’exécution avec l’Injection de dépendance")</span><span class="sxs-lookup"><span data-stu-id="17ec5-222">![Running Application with Dependency Injection](aspnet-mvc-4-dependency-injection/_static/image6.png "Running Application with Dependency Injection")</span></span>

    <span data-ttu-id="17ec5-223">*Application en cours d’exécution avec l’Injection de dépendances*</span><span class="sxs-lookup"><span data-stu-id="17ec5-223">*Running Application with Dependency Injection*</span></span>
2. <span data-ttu-id="17ec5-224">Accédez à **/Store**.</span><span class="sxs-lookup"><span data-stu-id="17ec5-224">Browse to **/Store**.</span></span> <span data-ttu-id="17ec5-225">Cette action appelle **StoreController**, qui est maintenant créé à l’aide de **Unity**.</span><span class="sxs-lookup"><span data-stu-id="17ec5-225">This will invoke **StoreController**, which is now created using **Unity**.</span></span>

    <span data-ttu-id="17ec5-226">![Store de musique MVC](aspnet-mvc-4-dependency-injection/_static/image7.png "Store de musique MVC")</span><span class="sxs-lookup"><span data-stu-id="17ec5-226">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span></span>

    <span data-ttu-id="17ec5-227">*Store de musique MVC*</span><span class="sxs-lookup"><span data-stu-id="17ec5-227">*MVC Music Store*</span></span>
3. <span data-ttu-id="17ec5-228">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="17ec5-228">Close the browser.</span></span>

<span data-ttu-id="17ec5-229">Dans les exercices suivants, vous allez apprendre à étendre la portée de l’Injection de dépendances pour l’utiliser à l’intérieur des vues ASP.NET MVC et les filtres d’Action.</span><span class="sxs-lookup"><span data-stu-id="17ec5-229">In the following exercises you will learn how to extend the Dependency Injection scope to use it inside ASP.NET MVC Views and Action Filters.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a><span data-ttu-id="17ec5-230">Exercice 2 : Injecter une vue</span><span class="sxs-lookup"><span data-stu-id="17ec5-230">Exercise 2: Injecting a View</span></span>

<span data-ttu-id="17ec5-231">Dans cet exercice, vous allez apprendre à utiliser l’Injection de dépendances dans une vue avec les nouvelles fonctionnalités d’ASP.NET MVC 4 pour l’intégration de Unity.</span><span class="sxs-lookup"><span data-stu-id="17ec5-231">In this exercise, you will learn how to use Dependency Injection in a view with the new features of ASP.NET MVC 4 for Unity integration.</span></span> <span data-ttu-id="17ec5-232">Pour ce faire, vous appelez un service personnalisé à l’intérieur de la vue de parcourir Store, qui affiche un message et une image ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="17ec5-232">In order to do that, you will call a custom service inside the Store Browse View, which will show a message and an image below.</span></span>

<span data-ttu-id="17ec5-233">Ensuite, vous intégrer le projet Unity et créer un résolveur de dépendance personnalisées pour injecter des dépendances.</span><span class="sxs-lookup"><span data-stu-id="17ec5-233">Then, you will integrate the project with Unity and create a custom dependency resolver to inject the dependencies.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a><span data-ttu-id="17ec5-234">Tâche 1 : création d’une vue qui utilise un Service</span><span class="sxs-lookup"><span data-stu-id="17ec5-234">Task 1 - Creating a View that Consumes a Service</span></span>

<span data-ttu-id="17ec5-235">Dans cette tâche, vous allez créer une vue qui effectue un appel de service pour générer une nouvelle dépendance.</span><span class="sxs-lookup"><span data-stu-id="17ec5-235">In this task, you will create a view that performs a service call to generate a new dependency.</span></span> <span data-ttu-id="17ec5-236">Le service est composé dans un service de messagerie simple inclus dans cette solution.</span><span class="sxs-lookup"><span data-stu-id="17ec5-236">The service consists in a simple messaging service included in this solution.</span></span>

1. <span data-ttu-id="17ec5-237">Ouvrez le **commencer** solution situé dans le **View\Begin injection Source\Ex02** dossier.</span><span class="sxs-lookup"><span data-stu-id="17ec5-237">Open the **Begin** solution located in the **Source\Ex02-Injecting View\Begin** folder.</span></span> <span data-ttu-id="17ec5-238">Sinon, vous pouvez continuer à utiliser le **fin** solution obtenu par le biais de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="17ec5-238">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="17ec5-239">Si vous avez ouvert le fourni **commencer** solution, vous devrez télécharger certains packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="17ec5-239">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="17ec5-240">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="17ec5-240">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="17ec5-241">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="17ec5-241">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="17ec5-242">Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="17ec5-242">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="17ec5-243">Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="17ec5-243">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="17ec5-244">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="17ec5-244">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="17ec5-245">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="17ec5-245">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="17ec5-246">Pour plus d’informations, consultez cet article : [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="17ec5-246">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="17ec5-247">Inclure le **MessageService.cs** et le **IMessageService.cs** classes situé dans le **Source \Assets** dossier dans **/Services**.</span><span class="sxs-lookup"><span data-stu-id="17ec5-247">Include the **MessageService.cs** and the **IMessageService.cs** classes located in the **Source \Assets** folder in **/Services**.</span></span> <span data-ttu-id="17ec5-248">Pour ce faire, cliquez sur **Services** dossier et sélectionnez **ajouter un élément existant**.</span><span class="sxs-lookup"><span data-stu-id="17ec5-248">To do this, right-click **Services** folder and select **Add Existing Item**.</span></span> <span data-ttu-id="17ec5-249">Accédez à l’emplacement des fichiers et les inclure.</span><span class="sxs-lookup"><span data-stu-id="17ec5-249">Browse to the files' location and include them.</span></span>

    <span data-ttu-id="17ec5-250">![Ajout de Service de Message et l’Interface de Service](aspnet-mvc-4-dependency-injection/_static/image8.png "Ajout du Service de Message et l’Interface de Service")</span><span class="sxs-lookup"><span data-stu-id="17ec5-250">![Adding Message Service and Service Interface](aspnet-mvc-4-dependency-injection/_static/image8.png "Adding Message Service and Service Interface")</span></span>

    <span data-ttu-id="17ec5-251">*Ajout de Message Service et l’Interface de Service*</span><span class="sxs-lookup"><span data-stu-id="17ec5-251">*Adding Message Service and Service Interface*</span></span>

    > [!NOTE]
    > <span data-ttu-id="17ec5-252">Le **IMessageService** interface définit deux propriétés implémentées par le **MessageService** classe.</span><span class="sxs-lookup"><span data-stu-id="17ec5-252">The **IMessageService** interface defines two properties implemented by the **MessageService** class.</span></span> <span data-ttu-id="17ec5-253">Ces propriétés -**Message** et **ImageUrl**-stocker le message et l’URL de l’image à afficher.</span><span class="sxs-lookup"><span data-stu-id="17ec5-253">These properties -**Message** and **ImageUrl**- store the message and the URL of the image to be displayed.</span></span>
3. <span data-ttu-id="17ec5-254">Créez le dossier **/Pages** dans le projet dossier racine, puis ajoutez la classe existante **MyBasePage.cs** de **Source\Assets**.</span><span class="sxs-lookup"><span data-stu-id="17ec5-254">Create the folder **/Pages** in the project's root folder, and then add the existing class **MyBasePage.cs** from **Source\Assets**.</span></span> <span data-ttu-id="17ec5-255">La page de base de que vous héritera présente la structure suivante.</span><span class="sxs-lookup"><span data-stu-id="17ec5-255">The base page you will inherit from has the following structure.</span></span>

    <span data-ttu-id="17ec5-256">![Dossier pages](aspnet-mvc-4-dependency-injection/_static/image9.png "dossier Pages")</span><span class="sxs-lookup"><span data-stu-id="17ec5-256">![Pages folder](aspnet-mvc-4-dependency-injection/_static/image9.png "Pages folder")</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. <span data-ttu-id="17ec5-257">Ouvrez **Browse.cshtml** afficher à partir de **/vues/Store** dossier et faites en sorte qu’il hérite **MyBasePage.cs**.</span><span class="sxs-lookup"><span data-stu-id="17ec5-257">Open **Browse.cshtml** view from **/Views/Store** folder, and make it inherit from **MyBasePage.cs**.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. <span data-ttu-id="17ec5-258">Dans le **Parcourir** afficher, ajouter un appel à **MessageService** pour afficher une image et un message récupéré par le service.</span><span class="sxs-lookup"><span data-stu-id="17ec5-258">In the **Browse** view, add a call to **MessageService** to display an image and a message retrieved by the service.</span></span>
   <span data-ttu-id="17ec5-259">(C#)</span><span class="sxs-lookup"><span data-stu-id="17ec5-259">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a><span data-ttu-id="17ec5-260">Tâche 2 - y compris un résolveur de dépendance personnalisée et un activateur de Page de vue personnalisée</span><span class="sxs-lookup"><span data-stu-id="17ec5-260">Task 2 - Including a Custom Dependency Resolver and a Custom View Page Activator</span></span>

<span data-ttu-id="17ec5-261">Dans la tâche précédente, vous avez injecté une nouvelle dépendance à l’intérieur d’une vue pour effectuer un appel de service qu’il contient.</span><span class="sxs-lookup"><span data-stu-id="17ec5-261">In the previous task, you injected a new dependency inside a view to perform a service call inside it.</span></span> <span data-ttu-id="17ec5-262">À présent, vous allez résoudre cette dépendance en implémentant les interfaces de l’Injection de dépendances ASP.NET MVC **IViewPageActivator** et **IDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="17ec5-262">Now, you will resolve that dependency by implementing the ASP.NET MVC Dependency Injection interfaces **IViewPageActivator** and **IDependencyResolver**.</span></span> <span data-ttu-id="17ec5-263">Vous allez inclure dans la solution à une implémentation de **IDependencyResolver** qui traitera la récupération de service à l’aide de Unity.</span><span class="sxs-lookup"><span data-stu-id="17ec5-263">You will include in the solution an implementation of **IDependencyResolver** that will deal with the service retrieval by using Unity.</span></span> <span data-ttu-id="17ec5-264">Ensuite, vous allez inclure une autre implémentation personnalisée de **IViewPageActivator** interface qui résout la création des vues.</span><span class="sxs-lookup"><span data-stu-id="17ec5-264">Then, you will include another custom implementation of **IViewPageActivator** interface that will solve the creation of the views.</span></span>

> [!NOTE]
> <span data-ttu-id="17ec5-265">Depuis ASP.NET MVC 3, l’implémentation de l’Injection de dépendance a simplifié les interfaces pour inscrire les services.</span><span class="sxs-lookup"><span data-stu-id="17ec5-265">Since ASP.NET MVC 3, the implementation for Dependency Injection had simplified the interfaces to register services.</span></span> <span data-ttu-id="17ec5-266">**IDependencyResolver** et **IViewPageActivator** font partie des fonctionnalités d’ASP.NET MVC 3 pour l’Injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="17ec5-266">**IDependencyResolver** and **IViewPageActivator** are part of ASP.NET MVC 3 features for Dependency Injection.</span></span>
> 
> <span data-ttu-id="17ec5-267">**-IDependencyResolver** interface remplace la précédente IMvcServiceLocator.</span><span class="sxs-lookup"><span data-stu-id="17ec5-267">**- IDependencyResolver** interface replaces the previous IMvcServiceLocator.</span></span> <span data-ttu-id="17ec5-268">Les implémenteurs de IDependencyResolver doivent retourner une instance du service ou une collection de service.</span><span class="sxs-lookup"><span data-stu-id="17ec5-268">Implementers of IDependencyResolver must return an instance of the service or a service collection.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> <span data-ttu-id="17ec5-269">**-IViewPageActivator** interface fournit un contrôle plus précis sur la façon dont les pages de vue sont instanciés par le biais de l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="17ec5-269">**- IViewPageActivator** interface provides more fine-grained control over how view pages are instantiated via dependency injection.</span></span> <span data-ttu-id="17ec5-270">Les classes qui implémentent **IViewPageActivator** interface peut créer des instances d’affichages à l’aide des informations de contexte.</span><span class="sxs-lookup"><span data-stu-id="17ec5-270">The classes that implement **IViewPageActivator** interface can create view instances using context information.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. <span data-ttu-id="17ec5-271">Créer le /**fabriques** dossier dans le dossier du projet racine.</span><span class="sxs-lookup"><span data-stu-id="17ec5-271">Create the /**Factories** folder in the project's root folder.</span></span>
2. <span data-ttu-id="17ec5-272">Inclure **CustomViewPageActivator.cs** à votre solution à partir de **/Sources/ressources/** à **fabriques** dossier.</span><span class="sxs-lookup"><span data-stu-id="17ec5-272">Include **CustomViewPageActivator.cs** to your solution from **/Sources/Assets/** to **Factories** folder.</span></span> <span data-ttu-id="17ec5-273">Pour ce faire, cliquez sur le **/Factories** dossier, puis sélectionnez **ajouter | Un élément existant** , puis sélectionnez **CustomViewPageActivator.cs**.</span><span class="sxs-lookup"><span data-stu-id="17ec5-273">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **CustomViewPageActivator.cs**.</span></span> <span data-ttu-id="17ec5-274">Cette classe implémente le **IViewPageActivator** interface pour contenir le conteneur Unity.</span><span class="sxs-lookup"><span data-stu-id="17ec5-274">This class implements the **IViewPageActivator** interface to hold the Unity Container.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > <span data-ttu-id="17ec5-275">**CustomViewPageActivator** est responsable de la gestion de la création d’une vue à l’aide d’un conteneur Unity.</span><span class="sxs-lookup"><span data-stu-id="17ec5-275">**CustomViewPageActivator** is responsible for managing the creation of a view by using a Unity container.</span></span>
3. <span data-ttu-id="17ec5-276">Inclure **UnityDependencyResolver.cs** à partir de fichiers **/Sources/ressources** à **/Factories** dossier.</span><span class="sxs-lookup"><span data-stu-id="17ec5-276">Include **UnityDependencyResolver.cs** file from **/Sources/Assets** to **/Factories** folder.</span></span> <span data-ttu-id="17ec5-277">Pour ce faire, cliquez sur le **/Factories** dossier, puis sélectionnez **ajouter | Un élément existant** , puis sélectionnez **UnityDependencyResolver.cs** fichier.</span><span class="sxs-lookup"><span data-stu-id="17ec5-277">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **UnityDependencyResolver.cs** file.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > <span data-ttu-id="17ec5-278">**UnityDependencyResolver** classe est un DependencyResolver personnalisé pour Unity.</span><span class="sxs-lookup"><span data-stu-id="17ec5-278">**UnityDependencyResolver** class is a custom DependencyResolver for Unity.</span></span> <span data-ttu-id="17ec5-279">Lorsqu’un service est introuvable dans le conteneur Unity, le programme de résolution de base est invocated.</span><span class="sxs-lookup"><span data-stu-id="17ec5-279">When a service cannot be found inside the Unity container, the base resolver is invocated.</span></span>

<span data-ttu-id="17ec5-280">Les deux implémentations seront inscrit pour permettre au modèle de connaître l’emplacement des services et les vues dans la tâche suivante.</span><span class="sxs-lookup"><span data-stu-id="17ec5-280">In the following task both implementations will be registered to let the model know the location of the services and the views.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a><span data-ttu-id="17ec5-281">Tâche 3 : l’inscription pour l’Injection de dépendances au sein du conteneur Unity</span><span class="sxs-lookup"><span data-stu-id="17ec5-281">Task 3 - Registering for Dependency Injection within Unity container</span></span>

<span data-ttu-id="17ec5-282">Dans cette tâche, vous allez placer toutes les opérations précédentes pour optimiser l’Injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="17ec5-282">In this task, you will put all the previous things together to make Dependency Injection work.</span></span>

<span data-ttu-id="17ec5-283">Jusqu'à présent, votre solution comporte les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="17ec5-283">Up to now your solution has the following elements:</span></span>

- <span data-ttu-id="17ec5-284">Un **Parcourir** vue hérite **MyBaseClass** et consomme **MessageService**.</span><span class="sxs-lookup"><span data-stu-id="17ec5-284">A **Browse** View that inherits from **MyBaseClass** and consumes **MessageService**.</span></span>
- <span data-ttu-id="17ec5-285">Une classe intermédiaire -**MyBaseClass**-qui a déclaré pour l’interface de service de l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="17ec5-285">An intermediate class -**MyBaseClass**- that has dependency injection declared for the service interface.</span></span>
- <span data-ttu-id="17ec5-286">Un service - **MessageService** - et son interface **IMessageService**.</span><span class="sxs-lookup"><span data-stu-id="17ec5-286">A service - **MessageService** - and its interface **IMessageService**.</span></span>
- <span data-ttu-id="17ec5-287">Un résolveur de dépendance personnalisées pour Unity - **UnityDependencyResolver** -qui porte sur la récupération de service.</span><span class="sxs-lookup"><span data-stu-id="17ec5-287">A custom dependency resolver for Unity - **UnityDependencyResolver** - that deals with service retrieval.</span></span>
- <span data-ttu-id="17ec5-288">Un activateur de Page de vue - **CustomViewPageActivator** -qui crée la page.</span><span class="sxs-lookup"><span data-stu-id="17ec5-288">A View Page activator - **CustomViewPageActivator** - that creates the page.</span></span>

<span data-ttu-id="17ec5-289">Pour injecter **Parcourir** vue, vous allez maintenant inscrire le résolveur de dépendance personnalisées dans le conteneur Unity.</span><span class="sxs-lookup"><span data-stu-id="17ec5-289">To inject **Browse** View, you will now register the custom dependency resolver in the Unity container.</span></span>

1. <span data-ttu-id="17ec5-290">Ouvrez **Bootstrapper.cs** fichier.</span><span class="sxs-lookup"><span data-stu-id="17ec5-290">Open **Bootstrapper.cs** file.</span></span>
2. <span data-ttu-id="17ec5-291">Inscrire une instance de **MessageService** dans le conteneur Unity pour initialiser le service :</span><span class="sxs-lookup"><span data-stu-id="17ec5-291">Register an instance of **MessageService** into the Unity container to initialize the service:</span></span>

    <span data-ttu-id="17ec5-292">(Code Snippet - *ASP.NET dépendance Injection Lab - Ex02 - Register Message Service*)</span><span class="sxs-lookup"><span data-stu-id="17ec5-292">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register Message Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. <span data-ttu-id="17ec5-293">Ajoutez une référence à **MvcMusicStore.Factories** espace de noms.</span><span class="sxs-lookup"><span data-stu-id="17ec5-293">Add a reference to **MvcMusicStore.Factories** namespace.</span></span>

    <span data-ttu-id="17ec5-294">(Code Snippet - *ASP.NET dépendance Injection Lab - Ex02 - fabriques Namespace*)</span><span class="sxs-lookup"><span data-stu-id="17ec5-294">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Factories Namespace*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. <span data-ttu-id="17ec5-295">Inscrire **CustomViewPageActivator** comme un activateur de Page de vue dans le conteneur Unity :</span><span class="sxs-lookup"><span data-stu-id="17ec5-295">Register **CustomViewPageActivator** as a View Page activator into the Unity container:</span></span>

    <span data-ttu-id="17ec5-296">(Code Snippet - *ASP.NET dépendance Injection Lab - Ex02 - Register CustomViewPageActivator*)</span><span class="sxs-lookup"><span data-stu-id="17ec5-296">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register CustomViewPageActivator*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. <span data-ttu-id="17ec5-297">Remplacez le résolveur de dépendance ASP.NET MVC 4 avec une instance de **UnityDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="17ec5-297">Replace ASP.NET MVC 4 default dependency resolver with an instance of **UnityDependencyResolver**.</span></span> <span data-ttu-id="17ec5-298">Pour ce faire, remplacez **Initialise** méthode contenu par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="17ec5-298">To do this, replace **Initialise** method content with the following code:</span></span>

    <span data-ttu-id="17ec5-299">(Code Snippet - *résolveur de dépendance de mise à jour ASP.NET dépendance Injection Lab - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="17ec5-299">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Update Dependency Resolver*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="17ec5-300">ASP.NET MVC fournit une classe de programme de résolution de dépendance par défaut.</span><span class="sxs-lookup"><span data-stu-id="17ec5-300">ASP.NET MVC provides a default dependency resolver class.</span></span> <span data-ttu-id="17ec5-301">Pour utiliser des programmes de résolution de dépendance personnalisées que celui que nous avons créé pour unity, ce programme de résolution doit être remplacé.</span><span class="sxs-lookup"><span data-stu-id="17ec5-301">To work with custom dependency resolvers as the one we have created for unity, this resolver has to be replaced.</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="17ec5-302">Tâche 4 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="17ec5-302">Task 4 - Running the Application</span></span>

<span data-ttu-id="17ec5-303">Dans cette tâche, vous allez exécuter l’application pour vérifier que le navigateur Store utilise le service et montre l’image et le message récupéré :</span><span class="sxs-lookup"><span data-stu-id="17ec5-303">In this task, you will run the application to verify that the Store Browser consumes the service and shows the image and the message retrieved:</span></span>

1. <span data-ttu-id="17ec5-304">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="17ec5-304">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="17ec5-305">Cliquez sur **Rock** dans le Menu de Genres et voir comment la **MessageService** a été injecté dans la vue de chargement et le message d’accueil et l’image.</span><span class="sxs-lookup"><span data-stu-id="17ec5-305">Click **Rock** within the Genres Menu and see how the **MessageService** was injected to the view and loaded the welcome message and the image.</span></span> <span data-ttu-id="17ec5-306">Dans cet exemple, nous entrons dans à &quot; **Rock**&quot;:</span><span class="sxs-lookup"><span data-stu-id="17ec5-306">In this example, we are entering to &quot;**Rock**&quot;:</span></span>

    <span data-ttu-id="17ec5-307">![Store de musique MVC - une Injection de vue](aspnet-mvc-4-dependency-injection/_static/image10.png "Store de musique MVC - une Injection de vue")</span><span class="sxs-lookup"><span data-stu-id="17ec5-307">![MVC Music Store - View Injection](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - View Injection")</span></span>

    <span data-ttu-id="17ec5-308">*Store de musique MVC - une Injection de vue*</span><span class="sxs-lookup"><span data-stu-id="17ec5-308">*MVC Music Store - View Injection*</span></span>
3. <span data-ttu-id="17ec5-309">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="17ec5-309">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a><span data-ttu-id="17ec5-310">Exercice 3 : Injection de filtres d’Action</span><span class="sxs-lookup"><span data-stu-id="17ec5-310">Exercise 3: Injecting Action Filters</span></span>

<span data-ttu-id="17ec5-311">Dans l’atelier précédent **filtres d’Action personnalisé** vous avez travaillé avec la personnalisation des filtres et injection de code.</span><span class="sxs-lookup"><span data-stu-id="17ec5-311">In the previous Hands-On lab **Custom Action Filters** you have worked with filters customization and injection.</span></span> <span data-ttu-id="17ec5-312">Dans cet exercice, vous allez apprendre à injecter des filtres avec l’Injection de dépendances à l’aide du conteneur Unity.</span><span class="sxs-lookup"><span data-stu-id="17ec5-312">In this exercise, you will learn how to inject filters with Dependency Injection by using the Unity container.</span></span> <span data-ttu-id="17ec5-313">Pour ce faire, vous allez ajouter à la solution de musique Store un filtre d’action personnalisé qui analyse l’activité du site.</span><span class="sxs-lookup"><span data-stu-id="17ec5-313">To do that, you will add to the Music Store solution a custom action filter that will trace the activity of the site.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a><span data-ttu-id="17ec5-314">Tâche 1 - y compris le filtre de suivi dans la Solution</span><span class="sxs-lookup"><span data-stu-id="17ec5-314">Task 1 - Including the Tracking Filter in the Solution</span></span>

<span data-ttu-id="17ec5-315">Dans cette tâche, vous inclurez dans le Store musique un filtre d’action personnalisé à des événements de trace.</span><span class="sxs-lookup"><span data-stu-id="17ec5-315">In this task, you will include in the Music Store a custom action filter to trace events.</span></span> <span data-ttu-id="17ec5-316">En tant que filtre d’action personnalisé concepts sont déjà traitées dans l’atelier précédent &quot;filtres d’Action personnalisée&quot;, vous incluez simplement la classe de filtre à partir du dossier de ressources de ce laboratoire et puis créer un fournisseur de filtre pour Unity :</span><span class="sxs-lookup"><span data-stu-id="17ec5-316">As custom action filter concepts are already treated in the previous Lab &quot;Custom Action Filters&quot;, you will just include the filter class from the Assets folder of this lab, and then create a Filter Provider for Unity:</span></span>

1. <span data-ttu-id="17ec5-317">Ouvrir le **commencer** solution situé dans le **Source\Ex03 - injectant des Filter\Begin Action** dossier.</span><span class="sxs-lookup"><span data-stu-id="17ec5-317">Open the **Begin** solution located in the **Source\Ex03 - Injecting Action Filter\Begin** folder.</span></span> <span data-ttu-id="17ec5-318">Sinon, vous pouvez continuer à utiliser le **fin** solution obtenu par le biais de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="17ec5-318">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="17ec5-319">Si vous avez ouvert le fourni **commencer** solution, vous devrez télécharger certains packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="17ec5-319">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="17ec5-320">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="17ec5-320">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="17ec5-321">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="17ec5-321">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="17ec5-322">Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="17ec5-322">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="17ec5-323">Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="17ec5-323">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="17ec5-324">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="17ec5-324">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="17ec5-325">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="17ec5-325">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="17ec5-326">Pour plus d’informations, consultez cet article : [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="17ec5-326">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="17ec5-327">Inclure **TraceActionFilter.cs** à partir de fichiers **/Sources/ressources** à **/filtre** dossier.</span><span class="sxs-lookup"><span data-stu-id="17ec5-327">Include **TraceActionFilter.cs** file from **/Sources/Assets** to **/Filters** folder.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > <span data-ttu-id="17ec5-328">Ce filtre d’action personnalisé effectue le suivi ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="17ec5-328">This custom action filter performs ASP.NET tracing.</span></span> <span data-ttu-id="17ec5-329">Vous pouvez vérifier &quot;local d’ASP.NET MVC 4 et les filtres d’Action dynamique&quot; Lab pour plus de détails sur.</span><span class="sxs-lookup"><span data-stu-id="17ec5-329">You can check &quot;ASP.NET MVC 4 local and Dynamic Action Filters&quot; Lab for more reference.</span></span>
3. <span data-ttu-id="17ec5-330">Ajoutez la classe vide **FilterProvider.cs** au projet dans le dossier   **/filtre.**</span><span class="sxs-lookup"><span data-stu-id="17ec5-330">Add the empty class **FilterProvider.cs** to the project in the folder **/Filters.**</span></span>
4. <span data-ttu-id="17ec5-331">Ajouter le **System.Web.Mvc** et **Microsoft.Practices.Unity** espaces de noms dans **FilterProvider.cs**.</span><span class="sxs-lookup"><span data-stu-id="17ec5-331">Add the **System.Web.Mvc** and **Microsoft.Practices.Unity** namespaces in **FilterProvider.cs**.</span></span>

    <span data-ttu-id="17ec5-332">(Code Snippet - *fournisseur filtre ASP.NET dépendance Injection Lab - Ex03 - Ajout d’espaces de noms*)</span><span class="sxs-lookup"><span data-stu-id="17ec5-332">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. <span data-ttu-id="17ec5-333">Faites en sorte que la classe hérite **IFilterProvider** Interface.</span><span class="sxs-lookup"><span data-stu-id="17ec5-333">Make the class inherit from **IFilterProvider** Interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. <span data-ttu-id="17ec5-334">Ajouter un **IUnityContainer** propriété dans le **FilterProvider** classe et ensuite créer un constructeur de classe pour affecter le conteneur.</span><span class="sxs-lookup"><span data-stu-id="17ec5-334">Add a **IUnityContainer** property in the **FilterProvider** class, and then create a class constructor to assign the container.</span></span>

    <span data-ttu-id="17ec5-335">(Code Snippet - *ASP.NET dépendance Injection Lab - Ex03 - filtre fournisseur constructeur*)</span><span class="sxs-lookup"><span data-stu-id="17ec5-335">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Constructor*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="17ec5-336">Le constructeur de classe de fournisseur de filtre n’est pas Création un **nouvelle** à l’intérieur de l’objet.</span><span class="sxs-lookup"><span data-stu-id="17ec5-336">The filter provider class constructor is not creating a **new** object inside.</span></span> <span data-ttu-id="17ec5-337">Le conteneur est passé comme paramètre, et la dépendance est résolue par Unity.</span><span class="sxs-lookup"><span data-stu-id="17ec5-337">The container is passed as a parameter, and the dependency is solved by Unity.</span></span>
7. <span data-ttu-id="17ec5-338">Dans le **FilterProvider** de classe, implémentez la méthode **GetFilters** de **IFilterProvider** interface.</span><span class="sxs-lookup"><span data-stu-id="17ec5-338">In the **FilterProvider** class, implement the method **GetFilters** from **IFilterProvider** interface.</span></span>

    <span data-ttu-id="17ec5-339">(Code Snippet - *ASP.NET dépendance Injection Lab - Ex03 - filtre fournisseur GetFilters*)</span><span class="sxs-lookup"><span data-stu-id="17ec5-339">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider GetFilters*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a><span data-ttu-id="17ec5-340">Tâche 2 : inscription et d’activer le filtre</span><span class="sxs-lookup"><span data-stu-id="17ec5-340">Task 2 - Registering and Enabling the Filter</span></span>

<span data-ttu-id="17ec5-341">Dans cette tâche, vous allez activer le suivi de site.</span><span class="sxs-lookup"><span data-stu-id="17ec5-341">In this task, you will enable site tracking.</span></span> <span data-ttu-id="17ec5-342">Pour ce faire, vous inscrirez le filtre dans **Bootstrapper.cs BuildUnityContainer** méthode pour démarrer le suivi :</span><span class="sxs-lookup"><span data-stu-id="17ec5-342">To do that, you will register the filter in **Bootstrapper.cs BuildUnityContainer** method to start tracing:</span></span>

1. <span data-ttu-id="17ec5-343">Ouvrez **Web.config** situé dans la racine du projet et activer le suivi de trace au niveau groupe de System.Web.</span><span class="sxs-lookup"><span data-stu-id="17ec5-343">Open **Web.config** located in the project root and enable trace tracking at System.Web group.</span></span>

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. <span data-ttu-id="17ec5-344">Ouvrez **Bootstrapper.cs** à la racine du projet.</span><span class="sxs-lookup"><span data-stu-id="17ec5-344">Open **Bootstrapper.cs** at project root.</span></span>
3. <span data-ttu-id="17ec5-345">Ajoutez une référence à la **MvcMusicStore.Filters** espace de noms.</span><span class="sxs-lookup"><span data-stu-id="17ec5-345">Add a reference to the **MvcMusicStore.Filters** namespace.</span></span>

    <span data-ttu-id="17ec5-346">(Code Snippet - *ASP.NET dépendance Injection Lab - Ex03 - programme d’amorçage Ajout d’espaces de noms*)</span><span class="sxs-lookup"><span data-stu-id="17ec5-346">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. <span data-ttu-id="17ec5-347">Sélectionnez le **BuildUnityContainer** (méthode) et enregistrez le filtre dans le conteneur Unity.</span><span class="sxs-lookup"><span data-stu-id="17ec5-347">Select the **BuildUnityContainer** method and register the filter in the Unity Container.</span></span> <span data-ttu-id="17ec5-348">Vous devez inscrire le fournisseur de filtre, ainsi que le filtre d’action.</span><span class="sxs-lookup"><span data-stu-id="17ec5-348">You will have to register the filter provider as well as the action filter.</span></span>

    <span data-ttu-id="17ec5-349">(Code Snippet - *ASP.NET dépendance Injection Lab - Ex03 - Register FilterProvider et ActionFilter*)</span><span class="sxs-lookup"><span data-stu-id="17ec5-349">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Register FilterProvider and ActionFilter*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="17ec5-350">Tâche 3 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="17ec5-350">Task 3 - Running the Application</span></span>

<span data-ttu-id="17ec5-351">Dans cette tâche, vous exécutez l’application et que le filtre d’action personnalisé est le suivi de l’activité de test :</span><span class="sxs-lookup"><span data-stu-id="17ec5-351">In this task, you will run the application and test that the custom action filter is tracing the activity:</span></span>

1. <span data-ttu-id="17ec5-352">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="17ec5-352">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="17ec5-353">Cliquez sur **Rock** dans le Menu de Genres.</span><span class="sxs-lookup"><span data-stu-id="17ec5-353">Click **Rock** within the Genres Menu.</span></span> <span data-ttu-id="17ec5-354">Si vous le souhaitez, vous pouvez accéder à plusieurs genres.</span><span class="sxs-lookup"><span data-stu-id="17ec5-354">You can browse to more genres if you want to.</span></span>

    <span data-ttu-id="17ec5-355">![Musique Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span><span class="sxs-lookup"><span data-stu-id="17ec5-355">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span></span>

    <span data-ttu-id="17ec5-356">*Magasin de musique*</span><span class="sxs-lookup"><span data-stu-id="17ec5-356">*Music Store*</span></span>
3. <span data-ttu-id="17ec5-357">Accédez à **/trace.axd** pour afficher la Trace de l’Application de page, puis cliquez sur **afficher les détails**.</span><span class="sxs-lookup"><span data-stu-id="17ec5-357">Browse to **/Trace.axd** to see the Application Trace page, and then click **View Details**.</span></span>

    <span data-ttu-id="17ec5-358">![Journal des traces application](aspnet-mvc-4-dependency-injection/_static/image12.png "journal de suivi d’Application")</span><span class="sxs-lookup"><span data-stu-id="17ec5-358">![Application Trace Log](aspnet-mvc-4-dependency-injection/_static/image12.png "Application Trace Log")</span></span>

    <span data-ttu-id="17ec5-359">*Journal de suivi d’application*</span><span class="sxs-lookup"><span data-stu-id="17ec5-359">*Application Trace Log*</span></span>

    <span data-ttu-id="17ec5-360">![Trace de l’application - détails de la demande](aspnet-mvc-4-dependency-injection/_static/image13.png "Application Trace - détails de la demande")</span><span class="sxs-lookup"><span data-stu-id="17ec5-360">![Application Trace - Request Details](aspnet-mvc-4-dependency-injection/_static/image13.png "Application Trace - Request Details")</span></span>

    <span data-ttu-id="17ec5-361">*Trace de l’application - détails de la demande*</span><span class="sxs-lookup"><span data-stu-id="17ec5-361">*Application Trace - Request Details*</span></span>
4. <span data-ttu-id="17ec5-362">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="17ec5-362">Close the browser.</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="17ec5-363">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="17ec5-363">Summary</span></span>

<span data-ttu-id="17ec5-364">En fin de cet atelier pratique, vous avez appris à utiliser l’Injection de dépendances dans ASP.NET MVC 4 en intégrant Unity à l’aide d’un NuGet Package.</span><span class="sxs-lookup"><span data-stu-id="17ec5-364">By completing this Hands-On Lab you have learned how to use Dependency Injection in ASP.NET MVC 4 by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="17ec5-365">Pour ce faire, vous avez utilisé l’Injection de dépendances à l’intérieur des contrôleurs, des vues et des filtres d’action.</span><span class="sxs-lookup"><span data-stu-id="17ec5-365">To achieve that, you have used Dependency Injection inside controllers, views and action filters.</span></span>

<span data-ttu-id="17ec5-366">Les concepts suivants ont été traités :</span><span class="sxs-lookup"><span data-stu-id="17ec5-366">The following concepts were covered:</span></span>

- <span data-ttu-id="17ec5-367">Fonctionnalités d’ASP.NET MVC 4, Injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="17ec5-367">ASP.NET MVC 4 Dependency Injection features</span></span>
- <span data-ttu-id="17ec5-368">Intégration de Unity à l’aide de NuGet Package de Unity.Mvc3</span><span class="sxs-lookup"><span data-stu-id="17ec5-368">Unity integration using Unity.Mvc3 NuGet Package</span></span>
- <span data-ttu-id="17ec5-369">Injection de dépendances dans les contrôleurs</span><span class="sxs-lookup"><span data-stu-id="17ec5-369">Dependency Injection in Controllers</span></span>
- <span data-ttu-id="17ec5-370">Injection de dépendances dans les vues</span><span class="sxs-lookup"><span data-stu-id="17ec5-370">Dependency Injection in Views</span></span>
- <span data-ttu-id="17ec5-371">Injection de dépendances des filtres d’Action</span><span class="sxs-lookup"><span data-stu-id="17ec5-371">Dependency injection of Action Filters</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="17ec5-372">Annexe a : installation de Visual Studio Express 2012 pour le Web</span><span class="sxs-lookup"><span data-stu-id="17ec5-372">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="17ec5-373">Vous pouvez installer **Microsoft Visual Studio Express 2012 pour Web** ou un autre &quot;Express&quot; à l’aide de la version du **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="17ec5-373">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="17ec5-374">Les instructions suivantes vous guident dans les étapes requises pour installer *Visual studio Express 2012 pour Web* à l’aide de *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="17ec5-374">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="17ec5-375">Accédez à [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="17ec5-375">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="17ec5-376">Si vous avez déjà installé Web Platform Installer, vous pouvez également ouvrir il et recherchez le produit &quot; <em>Visual Studio Express 2012 pour le Web avec Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="17ec5-376">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="17ec5-377">Cliquez sur **installer maintenant**.</span><span class="sxs-lookup"><span data-stu-id="17ec5-377">Click on **Install Now**.</span></span> <span data-ttu-id="17ec5-378">Si vous n’avez pas **Web Platform Installer** vous allez être redirigé pour télécharger et installer en premier.</span><span class="sxs-lookup"><span data-stu-id="17ec5-378">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="17ec5-379">Une fois **Web Platform Installer** est ouvert, cliquez sur **installer** pour démarrer le programme d’installation.</span><span class="sxs-lookup"><span data-stu-id="17ec5-379">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="17ec5-380">![Installer Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "installer Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="17ec5-380">![Install Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="17ec5-381">*Installer Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="17ec5-381">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="17ec5-382">Lisez les licences et les termes du contrat de tous les produits et cliquez sur **J’accepte** pour continuer.</span><span class="sxs-lookup"><span data-stu-id="17ec5-382">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Accepter les termes du contrat de licence](aspnet-mvc-4-dependency-injection/_static/image15.png)

    <span data-ttu-id="17ec5-384">*Accepter les termes du contrat de licence*</span><span class="sxs-lookup"><span data-stu-id="17ec5-384">*Accepting the license terms*</span></span>
5. <span data-ttu-id="17ec5-385">Attendez que le processus de téléchargement et l’installation se termine.</span><span class="sxs-lookup"><span data-stu-id="17ec5-385">Wait until the downloading and installation process completes.</span></span>

    ![Progression de l'installation](aspnet-mvc-4-dependency-injection/_static/image16.png)

    <span data-ttu-id="17ec5-387">*Progression de l’installation*</span><span class="sxs-lookup"><span data-stu-id="17ec5-387">*Installation progress*</span></span>
6. <span data-ttu-id="17ec5-388">Une fois l’installation terminée, cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="17ec5-388">When the installation completes, click **Finish**.</span></span>

    ![Installation est terminée](aspnet-mvc-4-dependency-injection/_static/image17.png)

    <span data-ttu-id="17ec5-390">*Installation est terminée*</span><span class="sxs-lookup"><span data-stu-id="17ec5-390">*Installation completed*</span></span>
7. <span data-ttu-id="17ec5-391">Cliquez sur **Exit** pour fermer Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="17ec5-391">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="17ec5-392">Pour ouvrir Visual Studio Express pour le Web, accédez à la **Démarrer** écran et démarrer l’écriture &quot; **VS Express**&quot;, puis cliquez sur le **Visual Studio Express pour le Web** vignette.</span><span class="sxs-lookup"><span data-stu-id="17ec5-392">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express pour une vignette de Web](aspnet-mvc-4-dependency-injection/_static/image18.png)

    <span data-ttu-id="17ec5-394">*VS Express pour une vignette de Web*</span><span class="sxs-lookup"><span data-stu-id="17ec5-394">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="17ec5-395">Annexe b : utilisation d’extraits de Code</span><span class="sxs-lookup"><span data-stu-id="17ec5-395">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="17ec5-396">Avec des extraits de code, vous avez tout le code que vous avez besoin à portée de main.</span><span class="sxs-lookup"><span data-stu-id="17ec5-396">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="17ec5-397">Le document de laboratoire vous indiquera exactement quand vous pouvez les utiliser, comme indiqué dans l’illustration suivante.</span><span class="sxs-lookup"><span data-stu-id="17ec5-397">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="17ec5-398">![À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet](aspnet-mvc-4-dependency-injection/_static/image19.png "extraits de code à l’aide de Visual Studio pour insérer du code dans votre projet")</span><span class="sxs-lookup"><span data-stu-id="17ec5-398">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-dependency-injection/_static/image19.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="17ec5-399">*À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet*</span><span class="sxs-lookup"><span data-stu-id="17ec5-399">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="17ec5-400">***Pour ajouter un extrait de code à l’aide du clavier (c# uniquement)***</span><span class="sxs-lookup"><span data-stu-id="17ec5-400">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="17ec5-401">Placez le curseur où vous souhaitez insérer le code.</span><span class="sxs-lookup"><span data-stu-id="17ec5-401">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="17ec5-402">Commencez à taper le nom de l’extrait de code (sans espaces ou des traits d’union).</span><span class="sxs-lookup"><span data-stu-id="17ec5-402">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="17ec5-403">Regarder en tant qu’IntelliSense affiche les noms des extraits correspondants.</span><span class="sxs-lookup"><span data-stu-id="17ec5-403">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="17ec5-404">Sélectionnez l’extrait de code correct (ou continuez à taper jusqu'à ce que le nom de l’extrait de code entière est sélectionnée).</span><span class="sxs-lookup"><span data-stu-id="17ec5-404">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="17ec5-405">Appuyez sur la touche Tab à deux reprises pour insérer l’extrait de code à l’emplacement du curseur.</span><span class="sxs-lookup"><span data-stu-id="17ec5-405">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="17ec5-406">![Commencez à taper le nom de l’extrait de code](aspnet-mvc-4-dependency-injection/_static/image20.png "commencez à taper le nom de l’extrait de code")</span><span class="sxs-lookup"><span data-stu-id="17ec5-406">![Start typing the snippet name](aspnet-mvc-4-dependency-injection/_static/image20.png "Start typing the snippet name")</span></span>

<span data-ttu-id="17ec5-407">*Commencez à taper le nom de l’extrait de code*</span><span class="sxs-lookup"><span data-stu-id="17ec5-407">*Start typing the snippet name*</span></span>

<span data-ttu-id="17ec5-408">![Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance](aspnet-mvc-4-dependency-injection/_static/image21.png "appuyez sur Tab pour sélectionner l’extrait de code en surbrillance")</span><span class="sxs-lookup"><span data-stu-id="17ec5-408">![Press Tab to select the highlighted snippet](aspnet-mvc-4-dependency-injection/_static/image21.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="17ec5-409">*Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance*</span><span class="sxs-lookup"><span data-stu-id="17ec5-409">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="17ec5-410">![Appuyez sur Tab à nouveau et l’extrait de code seront développe](aspnet-mvc-4-dependency-injection/_static/image22.png "appuyez sur Tab à nouveau et l’extrait de code seront développe.")</span><span class="sxs-lookup"><span data-stu-id="17ec5-410">![Press Tab again and the snippet will expand](aspnet-mvc-4-dependency-injection/_static/image22.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="17ec5-411">*Appuyez sur Tab à nouveau et l’extrait de code seront développe.*</span><span class="sxs-lookup"><span data-stu-id="17ec5-411">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="17ec5-412">***Pour ajouter un extrait de code à l’aide de la souris (c#, Visual Basic et XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="17ec5-412">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="17ec5-413">Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code.</span><span class="sxs-lookup"><span data-stu-id="17ec5-413">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="17ec5-414">Sélectionnez **insérer un extrait** suivie **mes extraits de Code**.</span><span class="sxs-lookup"><span data-stu-id="17ec5-414">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="17ec5-415">Choisissez l’extrait de code approprié dans la liste, en cliquant dessus.</span><span class="sxs-lookup"><span data-stu-id="17ec5-415">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="17ec5-416">![Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait](aspnet-mvc-4-dependency-injection/_static/image23.png "avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait")</span><span class="sxs-lookup"><span data-stu-id="17ec5-416">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-dependency-injection/_static/image23.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="17ec5-417">*Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait*</span><span class="sxs-lookup"><span data-stu-id="17ec5-417">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="17ec5-418">![Choisissez l’extrait de code approprié dans la liste, en cliquant dessus](aspnet-mvc-4-dependency-injection/_static/image24.png "choisir l’extrait de code approprié dans la liste, en cliquant sur celle-ci")</span><span class="sxs-lookup"><span data-stu-id="17ec5-418">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-dependency-injection/_static/image24.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="17ec5-419">*Choisissez l’extrait de code approprié dans la liste, en cliquant sur celle-ci*</span><span class="sxs-lookup"><span data-stu-id="17ec5-419">*Pick the relevant snippet from the list, by clicking on it*</span></span>
