---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: "Injection de dépendances d’ASP.NET MVC 4 | Documents Microsoft"
author: rick-anderson
description: "Remarque : Cet atelier pratique suppose que vous avez une connaissance élémentaire des filtres ASP.NET MVC et ASP.NET MVC 4. Si vous n’avez pas utilisé les filtres ASP.NET MVC 4 avant, nous rec..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: af4967f642ba4615f3392c0c404d2ec62edaaae8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-4-dependency-injection"></a><span data-ttu-id="b01b4-104">Injection de dépendances d’ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="b01b4-104">ASP.NET MVC 4 Dependency Injection</span></span>
====================
<span data-ttu-id="b01b4-105">par [Web Camps équipe](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="b01b4-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> [!NOTE]
> <span data-ttu-id="b01b4-106">Cet atelier pratique suppose que vous avez une connaissance élémentaire des **ASP.NET MVC** et **les filtres ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="b01b4-106">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC** and **ASP.NET MVC 4 filters**.</span></span> <span data-ttu-id="b01b4-107">Si vous n’avez pas utilisé **les filtres ASP.NET MVC 4** auparavant, nous vous recommandons de dépasser **filtres d’Action ASP.NET MVC personnalisée** atelier pratique.</span><span class="sxs-lookup"><span data-stu-id="b01b4-107">If you have not used **ASP.NET MVC 4 filters** before, we recommend you to go over **ASP.NET MVC Custom Action Filters** Hands-on Lab.</span></span>
> 
> <span data-ttu-id="b01b4-108">Tous les exemples de code et des extraits de code sont inclus dans le Kit de formation Camps Web, disponible à l’adresse [https://www.microsoft.com/en-us/download/29843](https://www.microsoft.com/en-us/download/29843).</span><span class="sxs-lookup"><span data-stu-id="b01b4-108">All sample code and snippets are included in the Web Camps Training Kit, available at [https://www.microsoft.com/en-us/download/29843](https://www.microsoft.com/en-us/download/29843).</span></span>


<span data-ttu-id="b01b4-109">Dans **objet orienté programmation** paradigme, objets fonctionnent ensemble dans un modèle de collaboration où il existe des collaborateurs et les consommateurs.</span><span class="sxs-lookup"><span data-stu-id="b01b4-109">In **Object Oriented Programming** paradigm, objects work together in a collaboration model where there are contributors and consumers.</span></span> <span data-ttu-id="b01b4-110">Naturellement, ce modèle de communication génère des dépendances entre les objets et composants, devient difficile à gérer lorsque la complexité augmente.</span><span class="sxs-lookup"><span data-stu-id="b01b4-110">Naturally, this communication model generates dependencies between objects and components, becoming difficult to manage when complexity increases.</span></span>

<span data-ttu-id="b01b4-111">![Dépendances de classe et la complexité de modèle](aspnet-mvc-4-dependency-injection/_static/image1.png "classe les dépendances et la complexité de modèle")</span><span class="sxs-lookup"><span data-stu-id="b01b4-111">![Class dependencies and model complexity](aspnet-mvc-4-dependency-injection/_static/image1.png "Class dependencies and model complexity")</span></span>

<span data-ttu-id="b01b4-112">*Dépendances de classe et la complexité du modèle*</span><span class="sxs-lookup"><span data-stu-id="b01b4-112">*Class dependencies and model complexity*</span></span>

<span data-ttu-id="b01b4-113">Vous avez probablement entendu parler la **modèle de fabrique** et la séparation entre l’interface et l’implémentation à l’aide de services, où les objets de client sont souvent responsables de l’emplacement du service.</span><span class="sxs-lookup"><span data-stu-id="b01b4-113">You have probably heard about the **Factory Pattern** and the separation between the interface and the implementation using services, where the client objects are often responsible for service location.</span></span>

<span data-ttu-id="b01b4-114">Le modèle de l’Injection de dépendances est une implémentation particulière d’Inversion de contrôle.</span><span class="sxs-lookup"><span data-stu-id="b01b4-114">The Dependency Injection pattern is a particular implementation of Inversion of Control.</span></span> <span data-ttu-id="b01b4-115">**Inversion de contrôle (IoC)** signifie que les objets ne créent pas d’autres objets sur laquelle ils reposent pour effectuer leur travail.</span><span class="sxs-lookup"><span data-stu-id="b01b4-115">**Inversion of Control (IoC)** means that objects do not create other objects on which they rely to do their work.</span></span> <span data-ttu-id="b01b4-116">Au lieu de cela, ils obtiennent les objets dont ils ont besoin d’une source externe (par exemple, un fichier de configuration xml).</span><span class="sxs-lookup"><span data-stu-id="b01b4-116">Instead, they get the objects that they need from an outside source (for example, an xml configuration file).</span></span>

<span data-ttu-id="b01b4-117">**Injection de dépendance (DI)** signifie qu’il est effectuée sans l’intervention de l’objet, généralement par un composant d’infrastructure qui transmet les paramètres du constructeur et définir les propriétés.</span><span class="sxs-lookup"><span data-stu-id="b01b4-117">**Dependency Injection (DI)** means that this is done without the object intervention, usually by a framework component that passes constructor parameters and set properties.</span></span>

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a><span data-ttu-id="b01b4-118">Le modèle de conception de dépendance (DI) de l’injection de code</span><span class="sxs-lookup"><span data-stu-id="b01b4-118">The Dependency Injection (DI) Design Pattern</span></span>

<span data-ttu-id="b01b4-119">À un niveau élevé, l’objectif de l’Injection de dépendance qui est une classe de client (par exemple, *le dsmessages*) a besoin d’un élément qui répond à une interface (par exemple, *IClub*).</span><span class="sxs-lookup"><span data-stu-id="b01b4-119">At a high level, the goal of Dependency Injection is that a client class (e.g. *the golfer*) needs something that satisfies an interface (e.g. *IClub*).</span></span> <span data-ttu-id="b01b4-120">Il ne tient pas compte que le type concret est (par exemple, *WoodClub, IronClub, WedgeClub* ou *PutterClub*), il souhaite que quelqu'un d’autre que gérer (par exemple, une bonne *chariot*).</span><span class="sxs-lookup"><span data-stu-id="b01b4-120">It doesn't care what the concrete type is (e.g. *WoodClub, IronClub, WedgeClub* or *PutterClub*), it wants someone else to handle that (e.g. a good *caddy*).</span></span> <span data-ttu-id="b01b4-121">Le résolveur de dépendance dans ASP.NET MVC peut vous permettre d’inscrire votre logique de dépendance vers un autre emplacement (par exemple, un conteneur ou un *conteneur de service*).</span><span class="sxs-lookup"><span data-stu-id="b01b4-121">The Dependency Resolver in ASP.NET MVC can allow you to register your dependency logic somewhere else (e.g. a container or a *bag of clubs*).</span></span>

<span data-ttu-id="b01b4-122">![Diagramme d’Injection de dépendance](aspnet-mvc-4-dependency-injection/_static/image2.png "illustration de l’Injection de dépendance")</span><span class="sxs-lookup"><span data-stu-id="b01b4-122">![Dependency Injection diagram](aspnet-mvc-4-dependency-injection/_static/image2.png "Dependency Injection illustration")</span></span>

<span data-ttu-id="b01b4-123">*Injection de dépendance - analogie de Golf*</span><span class="sxs-lookup"><span data-stu-id="b01b4-123">*Dependency Injection - Golf analogy*</span></span>

<span data-ttu-id="b01b4-124">Les avantages d’utiliser un modèle d’Injection de dépendance et Inversion de contrôle sont les suivants :</span><span class="sxs-lookup"><span data-stu-id="b01b4-124">The advantages of using Dependency Injection pattern and Inversion of Control are the following:</span></span>

- <span data-ttu-id="b01b4-125">Réduit les couplages de classe</span><span class="sxs-lookup"><span data-stu-id="b01b4-125">Reduces class coupling</span></span>
- <span data-ttu-id="b01b4-126">Augmente la réutilisation de code</span><span class="sxs-lookup"><span data-stu-id="b01b4-126">Increases code reusing</span></span>
- <span data-ttu-id="b01b4-127">Améliore la maintenabilité du code</span><span class="sxs-lookup"><span data-stu-id="b01b4-127">Improves code maintainability</span></span>
- <span data-ttu-id="b01b4-128">Améliore le test des applications</span><span class="sxs-lookup"><span data-stu-id="b01b4-128">Improves application testing</span></span>

> [!NOTE]
> <span data-ttu-id="b01b4-129">Injection de dépendances est parfois comparée avec le modèle de Design Factory abstraite, mais il existe une légère différence entre les deux approches.</span><span class="sxs-lookup"><span data-stu-id="b01b4-129">Dependency Injection is sometimes compared with Abstract Factory Design Pattern, but there is a slight difference between both approaches.</span></span> <span data-ttu-id="b01b4-130">DI possède une infrastructure travaillant derrière pour résoudre les dépendances en appelant les fabriques et les services inscrits.</span><span class="sxs-lookup"><span data-stu-id="b01b4-130">DI has a Framework working behind to solve dependencies by calling the factories and the registered services.</span></span>


<span data-ttu-id="b01b4-131">Maintenant que vous comprenez le modèle d’Injection de dépendance, vous allez apprendre tout au long de ce laboratoire pour l’appliquer dans ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="b01b4-131">Now that you understand the Dependency Injection Pattern, you will learn throughout this lab how to apply it in ASP.NET MVC 4.</span></span> <span data-ttu-id="b01b4-132">Vous allez démarrer en utilisant l’Injection de dépendance dans le **contrôleurs** d’inclure un service d’accès de base de données.</span><span class="sxs-lookup"><span data-stu-id="b01b4-132">You will start using Dependency Injection in the **Controllers** to include a database access service.</span></span> <span data-ttu-id="b01b4-133">Ensuite, vous allez appliquer l’Injection de dépendance du **vues** pour consommer un service et afficher des informations.</span><span class="sxs-lookup"><span data-stu-id="b01b4-133">Next, you will apply Dependency Injection to the **Views** to consume a service and show information.</span></span> <span data-ttu-id="b01b4-134">Enfin, vous allez étendre la DI aux filtres de ASP.NET MVC 4, injecter un filtre d’action personnalisé dans la solution.</span><span class="sxs-lookup"><span data-stu-id="b01b4-134">Finally, you will extend the DI to ASP.NET MVC 4 Filters, injecting a custom action filter in the solution.</span></span>

<span data-ttu-id="b01b4-135">Dans cet atelier pratique, vous allez apprendre comment :</span><span class="sxs-lookup"><span data-stu-id="b01b4-135">In this Hands-on Lab, you will learn how to:</span></span>

- <span data-ttu-id="b01b4-136">Intégration d’ASP.NET MVC 4 avec Unity pour l’Injection de dépendance à l’aide de Packages NuGet</span><span class="sxs-lookup"><span data-stu-id="b01b4-136">Integrate ASP.NET MVC 4 with Unity for Dependency Injection using NuGet Packages</span></span>
- <span data-ttu-id="b01b4-137">Utiliser l’Injection de dépendance à l’intérieur d’un contrôleur ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="b01b4-137">Use Dependency Injection inside an ASP.NET MVC Controller</span></span>
- <span data-ttu-id="b01b4-138">Utiliser l’Injection de dépendance à l’intérieur d’une vue ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="b01b4-138">Use Dependency Injection inside an ASP.NET MVC View</span></span>
- <span data-ttu-id="b01b4-139">Utiliser l’Injection de dépendance à l’intérieur d’un filtre d’Action ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="b01b4-139">Use Dependency Injection inside an ASP.NET MVC Action Filter</span></span>

> [!NOTE]
> <span data-ttu-id="b01b4-140">Cet atelier est à l’aide de NuGet Package de Unity.Mvc3 pour la résolution de dépendance, mais il est possible d’adapter de n’importe quelle infrastructure d’Injection de dépendance pour travailler avec ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="b01b4-140">This Lab is using Unity.Mvc3 NuGet Package for dependency resolution, but it is possible to adapt any Dependency Injection Framework to work with ASP.NET MVC 4.</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="b01b4-141">Conditions préalables</span><span class="sxs-lookup"><span data-stu-id="b01b4-141">Prerequisites</span></span>

<span data-ttu-id="b01b4-142">Vous devez disposer des éléments suivants pour effectuer ce laboratoire :</span><span class="sxs-lookup"><span data-stu-id="b01b4-142">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="b01b4-143">[Microsoft Visual Studio Express 2012 pour Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou supérieure (lecture [annexe A](#AppendixA) pour obtenir des instructions sur l’installation).</span><span class="sxs-lookup"><span data-stu-id="b01b4-143">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="b01b4-144">Installation</span><span class="sxs-lookup"><span data-stu-id="b01b4-144">Setup</span></span>

<span data-ttu-id="b01b4-145">**L’installation des extraits de Code**</span><span class="sxs-lookup"><span data-stu-id="b01b4-145">**Installing Code Snippets**</span></span>

<span data-ttu-id="b01b4-146">Pour plus de commodité, une grande partie du code que vous gérez le long de cet atelier est disponible sous les extraits de code Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b01b4-146">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="b01b4-147">Pour installer les extraits de code exécuter **.\Source\Setup\CodeSnippets.vsi** fichier.</span><span class="sxs-lookup"><span data-stu-id="b01b4-147">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="b01b4-148">Si vous n’êtes pas familiarisé avec les extraits de Code Visual Studio et vous souhaitez savoir comment les utiliser, vous pouvez faire référence à l’annexe de ce document &quot; [extraits de Code à l’aide de l’annexe b :](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="b01b4-148">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="b01b4-149">Exercices</span><span class="sxs-lookup"><span data-stu-id="b01b4-149">Exercises</span></span>

<span data-ttu-id="b01b4-150">Cet atelier pratique est constitué par les exercices suivants :</span><span class="sxs-lookup"><span data-stu-id="b01b4-150">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="b01b4-151">Exercice 1 : Injecter un contrôleur</span><span class="sxs-lookup"><span data-stu-id="b01b4-151">Exercise 1: Injecting a Controller</span></span>](#Exercise1)
2. [<span data-ttu-id="b01b4-152">Exercice 2 : Injection d’une vue</span><span class="sxs-lookup"><span data-stu-id="b01b4-152">Exercise 2: Injecting a View</span></span>](#Exercise2)
3. [<span data-ttu-id="b01b4-153">Exercice 3 : Injection de filtres</span><span class="sxs-lookup"><span data-stu-id="b01b4-153">Exercise 3: Injecting Filters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="b01b4-154">Chaque exercice est accompagné par un **fin** dossier qui contient la solution résultante, vous devez obtenir après avoir effectué les exercices.</span><span class="sxs-lookup"><span data-stu-id="b01b4-154">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="b01b4-155">Si vous avez besoin d’aide fonctionne via les exercices, vous pouvez utiliser cette solution comme guide.</span><span class="sxs-lookup"><span data-stu-id="b01b4-155">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="b01b4-156">Durée estimée pour effectuer ce laboratoire : **30 minutes**.</span><span class="sxs-lookup"><span data-stu-id="b01b4-156">Estimated time to complete this lab: **30 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a><span data-ttu-id="b01b4-157">Exercice 1 : Injecter un contrôleur</span><span class="sxs-lookup"><span data-stu-id="b01b4-157">Exercise 1: Injecting a Controller</span></span>

<span data-ttu-id="b01b4-158">Dans cet exercice, vous allez apprendre à utiliser l’Injection de dépendances dans ASP.NET MVC Controllers en intégrant Unity à l’aide d’un NuGet Package.</span><span class="sxs-lookup"><span data-stu-id="b01b4-158">In this exercise, you will learn how to use Dependency Injection in ASP.NET MVC Controllers by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="b01b4-159">Pour cette raison, vous allez inclure des services dans vos contrôleurs MvcMusicStore pour séparer la logique de l’accès aux données.</span><span class="sxs-lookup"><span data-stu-id="b01b4-159">For that reason, you will include services into your MvcMusicStore controllers to separate the logic from the data access.</span></span> <span data-ttu-id="b01b4-160">Les services créera une nouvelle dépendance dans le constructeur du contrôleur, qui est résolu à l’aide de l’Injection de dépendances à l’aide de **Unity**.</span><span class="sxs-lookup"><span data-stu-id="b01b4-160">The services will create a new dependency in the controller constructor, which will be resolved using Dependency Injection with the help of **Unity**.</span></span>

<span data-ttu-id="b01b4-161">Cette approche vous indiquera comment générer moins des applications à couplage sont plus flexibles et plus facile à maintenir et à tester.</span><span class="sxs-lookup"><span data-stu-id="b01b4-161">This approach will show you how to generate less coupled applications, which are more flexible and easier to maintain and test.</span></span> <span data-ttu-id="b01b4-162">Vous allez également apprendre comment intégrer ASP.NET MVC avec Unity.</span><span class="sxs-lookup"><span data-stu-id="b01b4-162">You will also learn how to integrate ASP.NET MVC with Unity.</span></span>

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a><span data-ttu-id="b01b4-163">À propos du Service de StoreManager</span><span class="sxs-lookup"><span data-stu-id="b01b4-163">About StoreManager Service</span></span>

<span data-ttu-id="b01b4-164">Le magasin de musique MVC fourni dans la solution begin maintenant inclut un service qui gère les données de magasin contrôleur nommées **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="b01b4-164">The MVC Music Store provided in the begin solution now includes a service that manages the Store Controller data named **StoreService**.</span></span> <span data-ttu-id="b01b4-165">Vous trouverez ci-dessous l’implémentation du Service de banque d’informations.</span><span class="sxs-lookup"><span data-stu-id="b01b4-165">Below you will find the Store Service implementation.</span></span> <span data-ttu-id="b01b4-166">Notez que toutes les méthodes retournent des entités de modèle.</span><span class="sxs-lookup"><span data-stu-id="b01b4-166">Note that all the methods return Model entities.</span></span>

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

<span data-ttu-id="b01b4-167">**StoreController** à partir du début de la solution consomme **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="b01b4-167">**StoreController** from the begin solution now consumes **StoreService**.</span></span> <span data-ttu-id="b01b4-168">Toutes les références de données ont été supprimés de **StoreController**et il est désormais possible de modifier le fournisseur d’accès aux données en cours sans modifier n’importe quelle méthode consomme **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="b01b4-168">All the data references were removed from **StoreController**, and now possible to modify the current data access provider without changing any method that consumes **StoreService**.</span></span>

<span data-ttu-id="b01b4-169">Vous trouverez ci-dessous la **StoreController** implémentation comporte une dépendance avec **StoreService** dans le constructeur de classe.</span><span class="sxs-lookup"><span data-stu-id="b01b4-169">You will find below that the **StoreController** implementation has a dependency with **StoreService** inside the class constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="b01b4-170">La dépendance introduite dans cet exercice est liée à **Inversion de contrôle** (IoC).</span><span class="sxs-lookup"><span data-stu-id="b01b4-170">The dependency introduced in this exercise is related to **Inversion of Control** (IoC).</span></span>
> 
> <span data-ttu-id="b01b4-171">Le **StoreController** constructeur de classe reçoit un **IStoreService** paramètre de type, qui est essentiel pour effectuer des appels de service à l’intérieur de la classe.</span><span class="sxs-lookup"><span data-stu-id="b01b4-171">The **StoreController** class constructor receives an **IStoreService** type parameter, which is essential to perform service calls from inside the class.</span></span> <span data-ttu-id="b01b4-172">Toutefois, **StoreController** n’implémente pas le constructeur par défaut (sans aucun paramètre) qui n’importe quel contrôleur doit comporter pour fonctionner avec ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b01b4-172">However, **StoreController** does not implement the default constructor (with no parameters) that any controller must have to work with ASP.NET MVC.</span></span>
> 
> <span data-ttu-id="b01b4-173">Pour résoudre la dépendance, le contrôleur doit être créé par une fabrique abstraite (une classe qui retourne un objet du type spécifié).</span><span class="sxs-lookup"><span data-stu-id="b01b4-173">To resolve the dependency, the controller has to be created by an abstract factory (a class that returns any object of the specified type).</span></span>


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="b01b4-174">Vous obtiendrez une erreur lors de la classe tente de créer le StoreController sans envoyer l’objet de service, car il n’existe aucun constructeur sans paramètre déclaré.</span><span class="sxs-lookup"><span data-stu-id="b01b4-174">You will get an error when the class tries to create the StoreController without sending the service object, as there is no parameterless constructor declared.</span></span>


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a><span data-ttu-id="b01b4-175">Tâche 1 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="b01b4-175">Task 1 - Running the Application</span></span>

<span data-ttu-id="b01b4-176">Dans cette tâche, vous allez exécuter l’application de début, ce qui inclut le service dans le contrôleur de magasin qui sépare l’accès aux données à partir de la logique d’application.</span><span class="sxs-lookup"><span data-stu-id="b01b4-176">In this task, you will run the Begin application, which includes the service into the Store Controller that separates the data access from the application logic.</span></span>

<span data-ttu-id="b01b4-177">Lorsque vous exécutez l’application, vous recevez une exception, comme le service du contrôleur n’est pas passé en tant que paramètre par défaut :</span><span class="sxs-lookup"><span data-stu-id="b01b4-177">When running the application, you will receive an exception, as the controller service is not passed as a parameter by default:</span></span>

1. <span data-ttu-id="b01b4-178">Ouvrez le **commencer** solution situé dans **Controller\Begin d’injection Source\Ex01**.</span><span class="sxs-lookup"><span data-stu-id="b01b4-178">Open the **Begin** solution located in **Source\Ex01-Injecting Controller\Begin**.</span></span>

    1. <span data-ttu-id="b01b4-179">Vous devez télécharger des packages NuGet manquants avant de poursuivre.</span><span class="sxs-lookup"><span data-stu-id="b01b4-179">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b01b4-180">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b01b4-180">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="b01b4-181">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="b01b4-181">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="b01b4-182">Enfin, générez la solution en cliquant sur **générer** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="b01b4-182">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b01b4-183">Un des avantages de l’utilisation de NuGet est que vous ne devez expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="b01b4-183">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b01b4-184">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques requises à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="b01b4-184">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b01b4-185">C’est pourquoi vous devez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="b01b4-185">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="b01b4-186">Appuyez sur **Ctrl + F5** pour exécuter l’application sans débogage.</span><span class="sxs-lookup"><span data-stu-id="b01b4-186">Press **Ctrl + F5** to run the application without debugging.</span></span> <span data-ttu-id="b01b4-187">Vous obtiendrez le message d’erreur &quot; **aucun constructeur sans paramètre défini pour cet objet**&quot;:</span><span class="sxs-lookup"><span data-stu-id="b01b4-187">You will get the error message &quot;**No parameterless constructor defined for this object**&quot;:</span></span>

    <span data-ttu-id="b01b4-188">![Erreur lors de l’exécution d’ASP.NET MVC commencer Application](aspnet-mvc-4-dependency-injection/_static/image3.png "erreur lors de l’exécution d’Application de commencer à ASP.NET MVC")</span><span class="sxs-lookup"><span data-stu-id="b01b4-188">![Error while running ASP.NET MVC Begin Application](aspnet-mvc-4-dependency-injection/_static/image3.png "Error while running ASP.NET MVC Begin Application")</span></span>

    <span data-ttu-id="b01b4-189">*Erreur lors de l’exécution d’Application de commencer à ASP.NET MVC*</span><span class="sxs-lookup"><span data-stu-id="b01b4-189">*Error while running ASP.NET MVC Begin Application*</span></span>
3. <span data-ttu-id="b01b4-190">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="b01b4-190">Close the browser.</span></span>

<span data-ttu-id="b01b4-191">Dans les étapes suivantes, vous allez travailler sur la Solution de magasin de musique d’injecter la dépendance qu'a besoin de ce contrôleur.</span><span class="sxs-lookup"><span data-stu-id="b01b4-191">In the following steps you will work on the Music Store Solution to inject the dependency this controller needs.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a><span data-ttu-id="b01b4-192">Tâche 2 - y compris les Unity dans MvcMusicStore Solution</span><span class="sxs-lookup"><span data-stu-id="b01b4-192">Task 2 - Including Unity into MvcMusicStore Solution</span></span>

<span data-ttu-id="b01b4-193">Dans cette tâche, vous allez inclure **Unity.Mvc3** NuGet Package pour la solution.</span><span class="sxs-lookup"><span data-stu-id="b01b4-193">In this task, you will include **Unity.Mvc3** NuGet Package to the solution.</span></span>

> [!NOTE]
> <span data-ttu-id="b01b4-194">Unity.Mvc3 package a été conçu pour ASP.NET MVC 3, mais il est entièrement compatible avec ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="b01b4-194">Unity.Mvc3 package was designed for ASP.NET MVC 3, but it is fully compatible with ASP.NET MVC 4.</span></span>
> 
> <span data-ttu-id="b01b4-195">Unity est un conteneur d’injection de dépendance légère et extensible, avec prise en charge facultative pour l’instance et tapez l’interception.</span><span class="sxs-lookup"><span data-stu-id="b01b4-195">Unity is a lightweight, extensible dependency injection container with optional support for instance and type interception.</span></span> <span data-ttu-id="b01b4-196">Il s’agit d’un conteneur à usage général pour une utilisation dans n’importe quel type d’application .NET.</span><span class="sxs-lookup"><span data-stu-id="b01b4-196">It is a general-purpose container for use in any type of .NET application.</span></span> <span data-ttu-id="b01b4-197">Il fournit toutes les fonctionnalités courantes dans les mécanismes d’injection de dépendance, y compris : création d’objet, l’abstraction des spécifications en spécifiant des dépendances au moment de l’exécution et de flexibilité, en différant la configuration du composant au conteneur.</span><span class="sxs-lookup"><span data-stu-id="b01b4-197">It provides all the common features found in dependency injection mechanisms including: object creation, abstraction of requirements by specifying dependencies at runtime and flexibility, by deferring the component configuration to the container.</span></span>


1. <span data-ttu-id="b01b4-198">Installer **Unity.Mvc3** NuGet Package dans le **MvcMusicStore** projet.</span><span class="sxs-lookup"><span data-stu-id="b01b4-198">Install **Unity.Mvc3** NuGet Package in the **MvcMusicStore** project.</span></span> <span data-ttu-id="b01b4-199">Pour ce faire, ouvrez le **Package Manager Console** de **vue** | **autres fenêtres**.</span><span class="sxs-lookup"><span data-stu-id="b01b4-199">To do this, open the **Package Manager Console** from **View** | **Other Windows**.</span></span>
2. <span data-ttu-id="b01b4-200">Exécutez la commande suivante.</span><span class="sxs-lookup"><span data-stu-id="b01b4-200">Run the following command.</span></span>

    <span data-ttu-id="b01b4-201">PMC</span><span class="sxs-lookup"><span data-stu-id="b01b4-201">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    <span data-ttu-id="b01b4-202">![Installation du Package NuGet de Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "l’installation de Package NuGet de Unity.Mvc3")</span><span class="sxs-lookup"><span data-stu-id="b01b4-202">![Installing Unity.Mvc3 NuGet Package](aspnet-mvc-4-dependency-injection/_static/image4.png "Installing Unity.Mvc3 NuGet Package")</span></span>

    <span data-ttu-id="b01b4-203">*Installation du Package NuGet de Unity.Mvc3*</span><span class="sxs-lookup"><span data-stu-id="b01b4-203">*Installing Unity.Mvc3 NuGet Package*</span></span>
3. <span data-ttu-id="b01b4-204">Une fois la **Unity.Mvc3** package est installé, explorez les fichiers et dossiers, il ajoute automatiquement afin de simplifier la configuration de l’unité.</span><span class="sxs-lookup"><span data-stu-id="b01b4-204">Once the **Unity.Mvc3** package is installed, explore the files and folders it automatically adds in order to simplify Unity configuration.</span></span>

    <span data-ttu-id="b01b4-205">![Package Unity.Mvc3 installé](aspnet-mvc-4-dependency-injection/_static/image5.png "package Unity.Mvc3 installé")</span><span class="sxs-lookup"><span data-stu-id="b01b4-205">![Unity.Mvc3 package installed](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 package installed")</span></span>

    <span data-ttu-id="b01b4-206">*Package Unity.Mvc3 installé*</span><span class="sxs-lookup"><span data-stu-id="b01b4-206">*Unity.Mvc3 package installed*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a><span data-ttu-id="b01b4-207">Tâche 3 - enregistrement de Unity dans Global.asax.cs Application\_Démarrer</span><span class="sxs-lookup"><span data-stu-id="b01b4-207">Task 3 - Registering Unity in Global.asax.cs Application\_Start</span></span>

<span data-ttu-id="b01b4-208">Dans cette tâche, vous mettrez à jour la **Application\_Démarrer** méthode situé dans **Global.asax.cs** pour appeler l’initialiseur du programme d’amorçage Unity et ensuite, mettez à jour l’enregistrement de fichier du programme d’amorçage le Service et le contrôleur, vous allez utiliser pour l’Injection de dépendance.</span><span class="sxs-lookup"><span data-stu-id="b01b4-208">In this task, you will update the **Application\_Start** method located in **Global.asax.cs** to call the Unity Bootstrapper initializer and then, update the Bootstrapper file registering the Service and Controller you will use for Dependency Injection.</span></span>

1. <span data-ttu-id="b01b4-209">Maintenant, vous raccordez le programme d’amorçage qui est le fichier qui initialise le conteneur Unity et résolveur de dépendance.</span><span class="sxs-lookup"><span data-stu-id="b01b4-209">Now, you will hook up the Bootstrapper which is the file that initializes the Unity container and Dependency Resolver.</span></span> <span data-ttu-id="b01b4-210">Pour ce faire, ouvrez **Global.asax.cs** et ajoutez le code en surbrillance suivant au sein de la **Application\_Démarrer** (méthode).</span><span class="sxs-lookup"><span data-stu-id="b01b4-210">To do this, open **Global.asax.cs** and add the following highlighted code within the **Application\_Start** method.</span></span>

    <span data-ttu-id="b01b4-211">(Code d’extrait de code - *Lab d’Injection de dépendance ASP.NET - Ex01 - initialiser Unity*)</span><span class="sxs-lookup"><span data-stu-id="b01b4-211">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Initialize Unity*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. <span data-ttu-id="b01b4-212">Ouvrez **Bootstrapper.cs** fichier.</span><span class="sxs-lookup"><span data-stu-id="b01b4-212">Open **Bootstrapper.cs** file.</span></span>
3. <span data-ttu-id="b01b4-213">Inclure des espaces de noms suivants : **MvcMusicStore.Services** et **MusicStore.Controllers**.</span><span class="sxs-lookup"><span data-stu-id="b01b4-213">Include the following namespaces: **MvcMusicStore.Services** and **MusicStore.Controllers**.</span></span>

    <span data-ttu-id="b01b4-214">(Code d’extrait de code - *ASP.NET dépendance Injection Lab - Ex01 - programme d’amorçage Ajout d’espaces de noms*)</span><span class="sxs-lookup"><span data-stu-id="b01b4-214">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. <span data-ttu-id="b01b4-215">Remplacez **BuildUnityContainer** de contenu par le code suivant qui enregistre le magasin de contrôleur et le Service de banque de la méthode.</span><span class="sxs-lookup"><span data-stu-id="b01b4-215">Replace **BuildUnityContainer** method's content with the following code that registers Store Controller and Store Service.</span></span>

    <span data-ttu-id="b01b4-216">(Code d’extrait de code - *ASP.NET dépendance Injection Lab - Ex01 - Register magasin contrôleur et le Service*)</span><span class="sxs-lookup"><span data-stu-id="b01b4-216">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Register Store Controller and Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="b01b4-217">Tâche 4 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="b01b4-217">Task 4 - Running the Application</span></span>

<span data-ttu-id="b01b4-218">Dans cette tâche, vous allez exécuter l’application pour vérifier qu’il peut maintenant être chargé après l’unité.</span><span class="sxs-lookup"><span data-stu-id="b01b4-218">In this task, you will run the application to verify that it can now be loaded after including Unity.</span></span>

1. <span data-ttu-id="b01b4-219">Appuyez sur **F5** pour exécuter l’application, l’application doit charger sans afficher de message d’erreur.</span><span class="sxs-lookup"><span data-stu-id="b01b4-219">Press **F5** to run the application, the application should now load without showing any error message.</span></span>

    <span data-ttu-id="b01b4-220">![Application en cours d’exécution avec l’Injection de dépendances](aspnet-mvc-4-dependency-injection/_static/image6.png "Application en cours d’exécution avec l’Injection de dépendance")</span><span class="sxs-lookup"><span data-stu-id="b01b4-220">![Running Application with Dependency Injection](aspnet-mvc-4-dependency-injection/_static/image6.png "Running Application with Dependency Injection")</span></span>

    <span data-ttu-id="b01b4-221">*Application en cours d’exécution avec l’Injection de dépendance*</span><span class="sxs-lookup"><span data-stu-id="b01b4-221">*Running Application with Dependency Injection*</span></span>
2. <span data-ttu-id="b01b4-222">Accédez à **/stockages**.</span><span class="sxs-lookup"><span data-stu-id="b01b4-222">Browse to **/Store**.</span></span> <span data-ttu-id="b01b4-223">Cela appelle **StoreController**, qui est maintenant créé à l’aide de **Unity**.</span><span class="sxs-lookup"><span data-stu-id="b01b4-223">This will invoke **StoreController**, which is now created using **Unity**.</span></span>

    <span data-ttu-id="b01b4-224">![Magasin de musique MVC](aspnet-mvc-4-dependency-injection/_static/image7.png "magasin de musique MVC")</span><span class="sxs-lookup"><span data-stu-id="b01b4-224">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span></span>

    <span data-ttu-id="b01b4-225">*Magasin de musique MVC*</span><span class="sxs-lookup"><span data-stu-id="b01b4-225">*MVC Music Store*</span></span>
3. <span data-ttu-id="b01b4-226">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="b01b4-226">Close the browser.</span></span>

<span data-ttu-id="b01b4-227">Dans les exercices suivants, vous allez apprendre à étendre la portée de l’Injection de dépendances pour l’utiliser à l’intérieur des vues ASP.NET MVC et les filtres d’Action.</span><span class="sxs-lookup"><span data-stu-id="b01b4-227">In the following exercises you will learn how to extend the Dependency Injection scope to use it inside ASP.NET MVC Views and Action Filters.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a><span data-ttu-id="b01b4-228">Exercice 2 : Injection d’une vue</span><span class="sxs-lookup"><span data-stu-id="b01b4-228">Exercise 2: Injecting a View</span></span>

<span data-ttu-id="b01b4-229">Dans cet exercice, vous allez apprendre à utiliser l’Injection de dépendances dans une vue avec les nouvelles fonctionnalités d’ASP.NET MVC 4 pour l’intégration d’Unity.</span><span class="sxs-lookup"><span data-stu-id="b01b4-229">In this exercise, you will learn how to use Dependency Injection in a view with the new features of ASP.NET MVC 4 for Unity integration.</span></span> <span data-ttu-id="b01b4-230">Pour ce faire, vous allez appeler un service personnalisé à l’intérieur de la banque de parcourir la vue, qui affiche un message et une image ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="b01b4-230">In order to do that, you will call a custom service inside the Store Browse View, which will show a message and an image below.</span></span>

<span data-ttu-id="b01b4-231">Ensuite, vous intégrer le projet Unity et créer un résolveur de dépendance personnalisée pour injecter des dépendances.</span><span class="sxs-lookup"><span data-stu-id="b01b4-231">Then, you will integrate the project with Unity and create a custom dependency resolver to inject the dependencies.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a><span data-ttu-id="b01b4-232">Tâche 1 : création d’une vue qui utilise un Service</span><span class="sxs-lookup"><span data-stu-id="b01b4-232">Task 1 - Creating a View that Consumes a Service</span></span>

<span data-ttu-id="b01b4-233">Dans cette tâche, vous allez créer une vue qui effectue un appel de service pour générer une nouvelle dépendance.</span><span class="sxs-lookup"><span data-stu-id="b01b4-233">In this task, you will create a view that performs a service call to generate a new dependency.</span></span> <span data-ttu-id="b01b4-234">Le service est composé dans un service de messagerie simple inclus dans cette solution.</span><span class="sxs-lookup"><span data-stu-id="b01b4-234">The service consists in a simple messaging service included in this solution.</span></span>

1. <span data-ttu-id="b01b4-235">Ouvrez le **commencer** solution situé dans le **View\Begin d’injection Source\Ex02** dossier.</span><span class="sxs-lookup"><span data-stu-id="b01b4-235">Open the **Begin** solution located in the **Source\Ex02-Injecting View\Begin** folder.</span></span> <span data-ttu-id="b01b4-236">Dans le cas contraire, vous pouvez continuer à utiliser le **fin** solution obtenue par la fin de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="b01b4-236">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="b01b4-237">Si vous avez ouvert le **commencer** solution, vous devez télécharger des packages NuGet manquants avant de poursuivre.</span><span class="sxs-lookup"><span data-stu-id="b01b4-237">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b01b4-238">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b01b4-238">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="b01b4-239">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="b01b4-239">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="b01b4-240">Enfin, générez la solution en cliquant sur **générer** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="b01b4-240">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b01b4-241">Un des avantages de l’utilisation de NuGet est que vous ne devez expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="b01b4-241">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b01b4-242">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques requises à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="b01b4-242">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b01b4-243">C’est pourquoi vous devez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="b01b4-243">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
    > 
    > <span data-ttu-id="b01b4-244">Pour plus d’informations, consultez l’article : [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="b01b4-244">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="b01b4-245">Inclure le **MessageService.cs** et le **IMessageService.cs** classes situé dans le **Source \Assets** dossier dans **/Services**.</span><span class="sxs-lookup"><span data-stu-id="b01b4-245">Include the **MessageService.cs** and the **IMessageService.cs** classes located in the **Source \Assets** folder in **/Services**.</span></span> <span data-ttu-id="b01b4-246">Pour ce faire, cliquez sur **Services** et sélectionnez **ajouter un élément existant**.</span><span class="sxs-lookup"><span data-stu-id="b01b4-246">To do this, right-click **Services** folder and select **Add Existing Item**.</span></span> <span data-ttu-id="b01b4-247">Recherchez emplacement les fichiers et les inclure.</span><span class="sxs-lookup"><span data-stu-id="b01b4-247">Browse to the files' location and include them.</span></span>

    <span data-ttu-id="b01b4-248">![Ajout de Service de Message et l’Interface de Service](aspnet-mvc-4-dependency-injection/_static/image8.png "Ajout du Service de Message et l’Interface de Service")</span><span class="sxs-lookup"><span data-stu-id="b01b4-248">![Adding Message Service and Service Interface](aspnet-mvc-4-dependency-injection/_static/image8.png "Adding Message Service and Service Interface")</span></span>

    <span data-ttu-id="b01b4-249">*Ajout de Service de Message et l’Interface de Service*</span><span class="sxs-lookup"><span data-stu-id="b01b4-249">*Adding Message Service and Service Interface*</span></span>

    > [!NOTE]
    > <span data-ttu-id="b01b4-250">Le **IMessageService** interface définit deux propriétés implémentées par le **MessageService** classe.</span><span class="sxs-lookup"><span data-stu-id="b01b4-250">The **IMessageService** interface defines two properties implemented by the **MessageService** class.</span></span> <span data-ttu-id="b01b4-251">Ces propriétés -**Message** et **ImageUrl**-stocker le message et l’URL de l’image à afficher.</span><span class="sxs-lookup"><span data-stu-id="b01b4-251">These properties -**Message** and **ImageUrl**- store the message and the URL of the image to be displayed.</span></span>
3. <span data-ttu-id="b01b4-252">Créez le dossier **/Pages** dans le fichier racine du dossier, puis ajoutez la classe existante **MyBasePage.cs** de **Source\Assets**.</span><span class="sxs-lookup"><span data-stu-id="b01b4-252">Create the folder **/Pages** in the project's root folder, and then add the existing class **MyBasePage.cs** from **Source\Assets**.</span></span> <span data-ttu-id="b01b4-253">La page de base de que vous hérite structure est la suivante.</span><span class="sxs-lookup"><span data-stu-id="b01b4-253">The base page you will inherit from has the following structure.</span></span>

    <span data-ttu-id="b01b4-254">![Dossier pages](aspnet-mvc-4-dependency-injection/_static/image9.png "dossier Pages")</span><span class="sxs-lookup"><span data-stu-id="b01b4-254">![Pages folder](aspnet-mvc-4-dependency-injection/_static/image9.png "Pages folder")</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. <span data-ttu-id="b01b4-255">Ouvrez **Browse.cshtml** depuis **/vues/Store** dossier et faites en sorte qu’elle hérite **MyBasePage.cs**.</span><span class="sxs-lookup"><span data-stu-id="b01b4-255">Open **Browse.cshtml** view from **/Views/Store** folder, and make it inherit from **MyBasePage.cs**.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. <span data-ttu-id="b01b4-256">Dans le **Parcourir** afficher, ajouter un appel à **MessageService** pour afficher une image et un message récupéré par le service.</span><span class="sxs-lookup"><span data-stu-id="b01b4-256">In the **Browse** view, add a call to **MessageService** to display an image and a message retrieved by the service.</span></span>
<span data-ttu-id="b01b4-257">(C#)</span><span class="sxs-lookup"><span data-stu-id="b01b4-257">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a><span data-ttu-id="b01b4-258">Tâche 2 - y compris un résolveur de dépendance personnalisée et un activateur de Page de vue personnalisée</span><span class="sxs-lookup"><span data-stu-id="b01b4-258">Task 2 - Including a Custom Dependency Resolver and a Custom View Page Activator</span></span>

<span data-ttu-id="b01b4-259">Dans la tâche précédente, vous introduit une nouvelle dépendance à l’intérieur d’une vue pour effectuer un appel de service qu’il contient.</span><span class="sxs-lookup"><span data-stu-id="b01b4-259">In the previous task, you injected a new dependency inside a view to perform a service call inside it.</span></span> <span data-ttu-id="b01b4-260">Maintenant, vous allez résoudre cette dépendance en implémentant les interfaces d’Injection de dépendances ASP.NET MVC **IViewPageActivator** et **IDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="b01b4-260">Now, you will resolve that dependency by implementing the ASP.NET MVC Dependency Injection interfaces **IViewPageActivator** and **IDependencyResolver**.</span></span> <span data-ttu-id="b01b4-261">Vous allez inclure dans la solution à une implémentation de **IDependencyResolver** qui traitera la récupération de service à l’aide de Unity.</span><span class="sxs-lookup"><span data-stu-id="b01b4-261">You will include in the solution an implementation of **IDependencyResolver** that will deal with the service retrieval by using Unity.</span></span> <span data-ttu-id="b01b4-262">Ensuite, vous allez inclure une autre implémentation personnalisée de **IViewPageActivator** interface qui corrige la création des vues.</span><span class="sxs-lookup"><span data-stu-id="b01b4-262">Then, you will include another custom implementation of **IViewPageActivator** interface that will solve the creation of the views.</span></span>

> [!NOTE]
> <span data-ttu-id="b01b4-263">Depuis ASP.NET MVC 3, l’implémentation pour l’Injection de dépendance a simplifié les interfaces pour inscrire les services.</span><span class="sxs-lookup"><span data-stu-id="b01b4-263">Since ASP.NET MVC 3, the implementation for Dependency Injection had simplified the interfaces to register services.</span></span> <span data-ttu-id="b01b4-264">**IDependencyResolver** et **IViewPageActivator** font partie des fonctionnalités d’ASP.NET MVC 3 pour l’Injection de dépendance.</span><span class="sxs-lookup"><span data-stu-id="b01b4-264">**IDependencyResolver** and **IViewPageActivator** are part of ASP.NET MVC 3 features for Dependency Injection.</span></span>
> 
> <span data-ttu-id="b01b4-265">**-IDependencyResolver** interface remplace la précédente IMvcServiceLocator.</span><span class="sxs-lookup"><span data-stu-id="b01b4-265">**- IDependencyResolver** interface replaces the previous IMvcServiceLocator.</span></span> <span data-ttu-id="b01b4-266">Les implémenteurs de IDependencyResolver doivent retourner une instance du service ou d’une collection de service.</span><span class="sxs-lookup"><span data-stu-id="b01b4-266">Implementers of IDependencyResolver must return an instance of the service or a service collection.</span></span>
>
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> <span data-ttu-id="b01b4-267">**-IViewPageActivator** interface fournit un contrôle plus précis sur la manière dont les pages de vue sont instanciés via l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="b01b4-267">**- IViewPageActivator** interface provides more fine-grained control over how view pages are instantiated via dependency injection.</span></span> <span data-ttu-id="b01b4-268">Les classes qui implémentent **IViewPageActivator** interface permettre créer des instances de vue à l’aide des informations de contexte.</span><span class="sxs-lookup"><span data-stu-id="b01b4-268">The classes that implement **IViewPageActivator** interface can create view instances using context information.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. <span data-ttu-id="b01b4-269">Créer la /**fabriques** dossier dans le dossier du projet racine.</span><span class="sxs-lookup"><span data-stu-id="b01b4-269">Create the /**Factories** folder in the project's root folder.</span></span>
2. <span data-ttu-id="b01b4-270">Inclure **CustomViewPageActivator.cs** à votre solution à partir de **/Sources/actifs/** à **fabriques** dossier.</span><span class="sxs-lookup"><span data-stu-id="b01b4-270">Include **CustomViewPageActivator.cs** to your solution from **/Sources/Assets/** to **Factories** folder.</span></span> <span data-ttu-id="b01b4-271">Pour ce faire, cliquez sur le **/Factories** dossier, sélectionnez **ajouter | Un élément existant** , puis sélectionnez **CustomViewPageActivator.cs**.</span><span class="sxs-lookup"><span data-stu-id="b01b4-271">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **CustomViewPageActivator.cs**.</span></span> <span data-ttu-id="b01b4-272">Cette classe implémente la **IViewPageActivator** interface pour contenir le conteneur Unity.</span><span class="sxs-lookup"><span data-stu-id="b01b4-272">This class implements the **IViewPageActivator** interface to hold the Unity Container.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > <span data-ttu-id="b01b4-273">**CustomViewPageActivator** est responsable de la gestion de la création d’une vue à l’aide d’un conteneur Unity.</span><span class="sxs-lookup"><span data-stu-id="b01b4-273">**CustomViewPageActivator** is responsible for managing the creation of a view by using a Unity container.</span></span>
3. <span data-ttu-id="b01b4-274">Inclure **UnityDependencyResolver.cs** à partir du fichier **/Sources/actifs** à **/Factories** dossier.</span><span class="sxs-lookup"><span data-stu-id="b01b4-274">Include **UnityDependencyResolver.cs** file from **/Sources/Assets** to **/Factories** folder.</span></span> <span data-ttu-id="b01b4-275">Pour ce faire, cliquez sur le **/Factories** dossier, sélectionnez **ajouter | Un élément existant** , puis sélectionnez **UnityDependencyResolver.cs** fichier.</span><span class="sxs-lookup"><span data-stu-id="b01b4-275">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **UnityDependencyResolver.cs** file.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > <span data-ttu-id="b01b4-276">**UnityDependencyResolver** classe est un DependencyResolver personnalisé pour Unity.</span><span class="sxs-lookup"><span data-stu-id="b01b4-276">**UnityDependencyResolver** class is a custom DependencyResolver for Unity.</span></span> <span data-ttu-id="b01b4-277">Lorsqu’un service est introuvable dans le conteneur Unity, le programme de résolution de base est invocated.</span><span class="sxs-lookup"><span data-stu-id="b01b4-277">When a service cannot be found inside the Unity container, the base resolver is invocated.</span></span>

<span data-ttu-id="b01b4-278">Les deux implémentations seront inscrit dans la tâche suivante pour permettre au modèle de connaître l’emplacement des services et les vues.</span><span class="sxs-lookup"><span data-stu-id="b01b4-278">In the following task both implementations will be registered to let the model know the location of the services and the views.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a><span data-ttu-id="b01b4-279">Tâche 3 : l’inscription pour l’Injection de dépendances au sein du conteneur Unity</span><span class="sxs-lookup"><span data-stu-id="b01b4-279">Task 3 - Registering for Dependency Injection within Unity container</span></span>

<span data-ttu-id="b01b4-280">Dans cette tâche, vous allez placer toutes les opérations précédentes pour utiliser l’Injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="b01b4-280">In this task, you will put all the previous things together to make Dependency Injection work.</span></span>

<span data-ttu-id="b01b4-281">Jusqu'à présent, votre solution comporte les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="b01b4-281">Up to now your solution has the following elements:</span></span>

- <span data-ttu-id="b01b4-282">A **Parcourir** vue hérite **MyBaseClass** et consomme **MessageService**.</span><span class="sxs-lookup"><span data-stu-id="b01b4-282">A **Browse** View that inherits from **MyBaseClass** and consumes **MessageService**.</span></span>
- <span data-ttu-id="b01b4-283">Une classe intermédiaire -**MyBaseClass**-qui a déclaré pour l’interface de service l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="b01b4-283">An intermediate class -**MyBaseClass**- that has dependency injection declared for the service interface.</span></span>
- <span data-ttu-id="b01b4-284">Un service - **MessageService** - et son interface **IMessageService**.</span><span class="sxs-lookup"><span data-stu-id="b01b4-284">A service - **MessageService** - and its interface **IMessageService**.</span></span>
- <span data-ttu-id="b01b4-285">Un résolveur de dépendance personnalisée pour Unity - **UnityDependencyResolver** -qui porte sur la récupération de service.</span><span class="sxs-lookup"><span data-stu-id="b01b4-285">A custom dependency resolver for Unity - **UnityDependencyResolver** - that deals with service retrieval.</span></span>
- <span data-ttu-id="b01b4-286">Un activateur de Page de vue - **CustomViewPageActivator** -qui crée la page.</span><span class="sxs-lookup"><span data-stu-id="b01b4-286">A View Page activator - **CustomViewPageActivator** - that creates the page.</span></span>

<span data-ttu-id="b01b4-287">Injecter **Parcourir** vue, vous allez maintenant inscrire le résolveur de dépendance personnalisée dans le conteneur Unity.</span><span class="sxs-lookup"><span data-stu-id="b01b4-287">To inject **Browse** View, you will now register the custom dependency resolver in the Unity container.</span></span>

1. <span data-ttu-id="b01b4-288">Ouvrez **Bootstrapper.cs** fichier.</span><span class="sxs-lookup"><span data-stu-id="b01b4-288">Open **Bootstrapper.cs** file.</span></span>
2. <span data-ttu-id="b01b4-289">Inscrire une instance de **MessageService** dans le conteneur Unity pour initialiser le service :</span><span class="sxs-lookup"><span data-stu-id="b01b4-289">Register an instance of **MessageService** into the Unity container to initialize the service:</span></span>

    <span data-ttu-id="b01b4-290">(Code d’extrait de code - *ASP.NET dépendance Injection Lab - Ex02 - Register Message Service*)</span><span class="sxs-lookup"><span data-stu-id="b01b4-290">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register Message Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. <span data-ttu-id="b01b4-291">Ajoutez une référence à **MvcMusicStore.Factories** espace de noms.</span><span class="sxs-lookup"><span data-stu-id="b01b4-291">Add a reference to **MvcMusicStore.Factories** namespace.</span></span>

    <span data-ttu-id="b01b4-292">(Code d’extrait de code - *ASP.NET dépendance Injection Lab - Ex02 - fabriques Namespace*)</span><span class="sxs-lookup"><span data-stu-id="b01b4-292">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Factories Namespace*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. <span data-ttu-id="b01b4-293">Inscrire **CustomViewPageActivator** comme un activateur de Page de vue dans le conteneur Unity :</span><span class="sxs-lookup"><span data-stu-id="b01b4-293">Register **CustomViewPageActivator** as a View Page activator into the Unity container:</span></span>

    <span data-ttu-id="b01b4-294">(Code d’extrait de code - *ASP.NET dépendance Injection Lab - Ex02 - Register CustomViewPageActivator*)</span><span class="sxs-lookup"><span data-stu-id="b01b4-294">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register CustomViewPageActivator*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. <span data-ttu-id="b01b4-295">Remplacez le résolveur de dépendance ASP.NET MVC 4 avec une instance de **UnityDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="b01b4-295">Replace ASP.NET MVC 4 default dependency resolver with an instance of **UnityDependencyResolver**.</span></span> <span data-ttu-id="b01b4-296">Pour ce faire, remplacez **Initialise** méthode contenu par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b01b4-296">To do this, replace **Initialise** method content with the following code:</span></span>

    <span data-ttu-id="b01b4-297">(Code d’extrait de code - *résolveur de dépendance de mise à jour ASP.NET dépendance Injection Lab - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="b01b4-297">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Update Dependency Resolver*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="b01b4-298">ASP.NET MVC fournit une classe de programme de résolution de dépendance par défaut.</span><span class="sxs-lookup"><span data-stu-id="b01b4-298">ASP.NET MVC provides a default dependency resolver class.</span></span> <span data-ttu-id="b01b4-299">Pour utiliser des programmes de résolution de dépendance personnalisée que celui que nous avons créé pour unity, ce programme de résolution doit être remplacé.</span><span class="sxs-lookup"><span data-stu-id="b01b4-299">To work with custom dependency resolvers as the one we have created for unity, this resolver has to be replaced.</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="b01b4-300">Tâche 4 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="b01b4-300">Task 4 - Running the Application</span></span>

<span data-ttu-id="b01b4-301">Dans cette tâche, vous allez exécuter l’application pour vérifier que le navigateur de la banque utilise le service et montre l’image et le message récupéré :</span><span class="sxs-lookup"><span data-stu-id="b01b4-301">In this task, you will run the application to verify that the Store Browser consumes the service and shows the image and the message retrieved:</span></span>

1. <span data-ttu-id="b01b4-302">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="b01b4-302">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="b01b4-303">Cliquez sur **Rock** dans le Menu de Genres et voir comment la **MessageService** a été injecté dans la vue de chargement et le message d’accueil et l’image.</span><span class="sxs-lookup"><span data-stu-id="b01b4-303">Click **Rock** within the Genres Menu and see how the **MessageService** was injected to the view and loaded the welcome message and the image.</span></span> <span data-ttu-id="b01b4-304">Dans cet exemple, nous allons entrant à &quot; **Rock**&quot;:</span><span class="sxs-lookup"><span data-stu-id="b01b4-304">In this example, we are entering to &quot;**Rock**&quot;:</span></span>

    <span data-ttu-id="b01b4-305">![Magasin de musique MVC - vue Injection](aspnet-mvc-4-dependency-injection/_static/image10.png "magasin de musique MVC - Injection d’affichage")</span><span class="sxs-lookup"><span data-stu-id="b01b4-305">![MVC Music Store - View Injection](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - View Injection")</span></span>

    <span data-ttu-id="b01b4-306">*Magasin de musique MVC - Injection d’affichage*</span><span class="sxs-lookup"><span data-stu-id="b01b4-306">*MVC Music Store - View Injection*</span></span>
3. <span data-ttu-id="b01b4-307">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="b01b4-307">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a><span data-ttu-id="b01b4-308">Exercice 3 : Injecter des filtres d’Action</span><span class="sxs-lookup"><span data-stu-id="b01b4-308">Exercise 3: Injecting Action Filters</span></span>

<span data-ttu-id="b01b4-309">Dans l’atelier pratique précédent **filtres d’Action personnalisée** vous avez travaillé avec des filtres personnalisation et injection de code.</span><span class="sxs-lookup"><span data-stu-id="b01b4-309">In the previous Hands-On lab **Custom Action Filters** you have worked with filters customization and injection.</span></span> <span data-ttu-id="b01b4-310">Dans cet exercice, vous allez apprendre à injecter des filtres avec l’Injection de dépendances à l’aide du conteneur Unity.</span><span class="sxs-lookup"><span data-stu-id="b01b4-310">In this exercise, you will learn how to inject filters with Dependency Injection by using the Unity container.</span></span> <span data-ttu-id="b01b4-311">Pour ce faire, vous allez ajouter à la solution de magasin de musique un filtre d’action personnalisé qui analyse l’activité du site.</span><span class="sxs-lookup"><span data-stu-id="b01b4-311">To do that, you will add to the Music Store solution a custom action filter that will trace the activity of the site.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a><span data-ttu-id="b01b4-312">Tâche 1 - y compris le suivi de filtre dans la Solution</span><span class="sxs-lookup"><span data-stu-id="b01b4-312">Task 1 - Including the Tracking Filter in the Solution</span></span>

<span data-ttu-id="b01b4-313">Dans cette tâche, vous allez inclure dans le magasin de musique un filtre d’action personnalisé à des événements de trace.</span><span class="sxs-lookup"><span data-stu-id="b01b4-313">In this task, you will include in the Music Store a custom action filter to trace events.</span></span> <span data-ttu-id="b01b4-314">En tant que filtre d’action personnalisé des concepts sont déjà traités dans l’atelier précédent &quot;des filtres d’Action personnalisés&quot;, vous allez inclure simplement la classe de filtre à partir du dossier de ressources de ce laboratoire et ensuite créer un fournisseur de filtre pour Unity :</span><span class="sxs-lookup"><span data-stu-id="b01b4-314">As custom action filter concepts are already treated in the previous Lab &quot;Custom Action Filters&quot;, you will just include the filter class from the Assets folder of this lab, and then create a Filter Provider for Unity:</span></span>

1. <span data-ttu-id="b01b4-315">Ouvrez le **commencer** solution situé dans le **Source\Ex03 - Filter\Begin de Action injection** dossier.</span><span class="sxs-lookup"><span data-stu-id="b01b4-315">Open the **Begin** solution located in the **Source\Ex03 - Injecting Action Filter\Begin** folder.</span></span> <span data-ttu-id="b01b4-316">Dans le cas contraire, vous pouvez continuer à utiliser le **fin** solution obtenue par la fin de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="b01b4-316">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="b01b4-317">Si vous avez ouvert le **commencer** solution, vous devez télécharger des packages NuGet manquants avant de poursuivre.</span><span class="sxs-lookup"><span data-stu-id="b01b4-317">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b01b4-318">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b01b4-318">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="b01b4-319">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="b01b4-319">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="b01b4-320">Enfin, générez la solution en cliquant sur **générer** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="b01b4-320">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b01b4-321">Un des avantages de l’utilisation de NuGet est que vous ne devez expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="b01b4-321">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b01b4-322">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques requises à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="b01b4-322">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b01b4-323">C’est pourquoi vous devez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="b01b4-323">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
    > 
    > <span data-ttu-id="b01b4-324">Pour plus d’informations, consultez l’article : [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="b01b4-324">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="b01b4-325">Inclure **TraceActionFilter.cs** à partir du fichier **/Sources/actifs** à **/filtre** dossier.</span><span class="sxs-lookup"><span data-stu-id="b01b4-325">Include **TraceActionFilter.cs** file from **/Sources/Assets** to **/Filters** folder.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > <span data-ttu-id="b01b4-326">Ce filtre d’action personnalisé effectue le suivi ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b01b4-326">This custom action filter performs ASP.NET tracing.</span></span> <span data-ttu-id="b01b4-327">Vous pouvez vérifier &quot;local d’ASP.NET MVC 4 et les filtres d’Action dynamique&quot; Lab pour plus de référence.</span><span class="sxs-lookup"><span data-stu-id="b01b4-327">You can check &quot;ASP.NET MVC 4 local and Dynamic Action Filters&quot; Lab for more reference.</span></span>
3. <span data-ttu-id="b01b4-328">Ajoutez la classe vide **FilterProvider.cs** au projet dans le dossier   **/filtre.**</span><span class="sxs-lookup"><span data-stu-id="b01b4-328">Add the empty class **FilterProvider.cs** to the project in the folder **/Filters.**</span></span>
4. <span data-ttu-id="b01b4-329">Ajouter le **System.Web.Mvc** et **Microsoft.Practices.Unity** espaces de noms dans **FilterProvider.cs**.</span><span class="sxs-lookup"><span data-stu-id="b01b4-329">Add the **System.Web.Mvc** and **Microsoft.Practices.Unity** namespaces in **FilterProvider.cs**.</span></span>

    <span data-ttu-id="b01b4-330">(Code d’extrait de code - *fournisseur filtre ASP.NET dépendance Injection Lab - Ex03 - Ajout d’espaces de noms*)</span><span class="sxs-lookup"><span data-stu-id="b01b4-330">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. <span data-ttu-id="b01b4-331">Faites en sorte que la classe hérite **IFilterProvider** Interface.</span><span class="sxs-lookup"><span data-stu-id="b01b4-331">Make the class inherit from **IFilterProvider** Interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. <span data-ttu-id="b01b4-332">Ajouter un **IUnityContainer** propriété dans le **FilterProvider** de classe et ensuite créer un constructeur de classe pour affecter le conteneur.</span><span class="sxs-lookup"><span data-stu-id="b01b4-332">Add a **IUnityContainer** property in the **FilterProvider** class, and then create a class constructor to assign the container.</span></span>

    <span data-ttu-id="b01b4-333">(Code d’extrait de code - *constructeur du fournisseur filtre ASP.NET dépendance Injection Lab - Ex03 -*)</span><span class="sxs-lookup"><span data-stu-id="b01b4-333">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Constructor*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="b01b4-334">Le constructeur de classe de fournisseur de filtre n’est pas créer un **nouvelle** à l’intérieur de l’objet.</span><span class="sxs-lookup"><span data-stu-id="b01b4-334">The filter provider class constructor is not creating a **new** object inside.</span></span> <span data-ttu-id="b01b4-335">Le conteneur est passé en tant que paramètre et la dépendance peut être résolue en Unity.</span><span class="sxs-lookup"><span data-stu-id="b01b4-335">The container is passed as a parameter, and the dependency is solved by Unity.</span></span>
7. <span data-ttu-id="b01b4-336">Dans le **FilterProvider** de classe, implémentez la méthode **GetFilters** de **IFilterProvider** interface.</span><span class="sxs-lookup"><span data-stu-id="b01b4-336">In the **FilterProvider** class, implement the method **GetFilters** from **IFilterProvider** interface.</span></span>

    <span data-ttu-id="b01b4-337">(Code d’extrait de code - *ASP.NET dépendance Injection Lab - Ex03 - filtre fournisseur GetFilters*)</span><span class="sxs-lookup"><span data-stu-id="b01b4-337">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider GetFilters*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a><span data-ttu-id="b01b4-338">Tâche 2 : l’inscription et l’activation du filtre</span><span class="sxs-lookup"><span data-stu-id="b01b4-338">Task 2 - Registering and Enabling the Filter</span></span>

<span data-ttu-id="b01b4-339">Dans cette tâche, vous devez activer le suivi du site.</span><span class="sxs-lookup"><span data-stu-id="b01b4-339">In this task, you will enable site tracking.</span></span> <span data-ttu-id="b01b4-340">Pour ce faire, vous inscrirez le filtre dans **Bootstrapper.cs BuildUnityContainer** méthode pour démarrer le suivi :</span><span class="sxs-lookup"><span data-stu-id="b01b4-340">To do that, you will register the filter in **Bootstrapper.cs BuildUnityContainer** method to start tracing:</span></span>

1. <span data-ttu-id="b01b4-341">Ouvrez **Web.config** se trouvent dans la racine du projet et activer le suivi de trace au niveau groupe de System.Web.</span><span class="sxs-lookup"><span data-stu-id="b01b4-341">Open **Web.config** located in the project root and enable trace tracking at System.Web group.</span></span>

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. <span data-ttu-id="b01b4-342">Ouvrez **Bootstrapper.cs** à la racine du projet.</span><span class="sxs-lookup"><span data-stu-id="b01b4-342">Open **Bootstrapper.cs** at project root.</span></span>
3. <span data-ttu-id="b01b4-343">Ajoutez une référence à la **MvcMusicStore.Filters** espace de noms.</span><span class="sxs-lookup"><span data-stu-id="b01b4-343">Add a reference to the **MvcMusicStore.Filters** namespace.</span></span>

    <span data-ttu-id="b01b4-344">(Code d’extrait de code - *ASP.NET dépendance Injection Lab - Ex03 - programme d’amorçage Ajout d’espaces de noms*)</span><span class="sxs-lookup"><span data-stu-id="b01b4-344">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. <span data-ttu-id="b01b4-345">Sélectionnez le **BuildUnityContainer** (méthode) et inscrire le filtre dans le conteneur Unity.</span><span class="sxs-lookup"><span data-stu-id="b01b4-345">Select the **BuildUnityContainer** method and register the filter in the Unity Container.</span></span> <span data-ttu-id="b01b4-346">Vous devez inscrire le fournisseur du filtre, ainsi que le filtre d’action.</span><span class="sxs-lookup"><span data-stu-id="b01b4-346">You will have to register the filter provider as well as the action filter.</span></span>

    <span data-ttu-id="b01b4-347">(Code d’extrait de code - *ASP.NET dépendance Injection Lab - Ex03 - Register FilterProvider et ActionFilter*)</span><span class="sxs-lookup"><span data-stu-id="b01b4-347">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Register FilterProvider and ActionFilter*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="b01b4-348">Tâche 3 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="b01b4-348">Task 3 - Running the Application</span></span>

<span data-ttu-id="b01b4-349">Dans cette tâche, vous exécutez l’application et que le filtre d’action personnalisé est le suivi de l’activité de test :</span><span class="sxs-lookup"><span data-stu-id="b01b4-349">In this task, you will run the application and test that the custom action filter is tracing the activity:</span></span>

1. <span data-ttu-id="b01b4-350">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="b01b4-350">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="b01b4-351">Cliquez sur **Rock** dans le Menu de Genres.</span><span class="sxs-lookup"><span data-stu-id="b01b4-351">Click **Rock** within the Genres Menu.</span></span> <span data-ttu-id="b01b4-352">Vous pouvez accéder à plusieurs genres si vous le souhaitez.</span><span class="sxs-lookup"><span data-stu-id="b01b4-352">You can browse to more genres if you want to.</span></span>

    <span data-ttu-id="b01b4-353">![Magasin de musique](aspnet-mvc-4-dependency-injection/_static/image11.png "magasin de musique")</span><span class="sxs-lookup"><span data-stu-id="b01b4-353">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span></span>

    <span data-ttu-id="b01b4-354">*Magasin de musique*</span><span class="sxs-lookup"><span data-stu-id="b01b4-354">*Music Store*</span></span>
3. <span data-ttu-id="b01b4-355">Accédez à **/trace.axd** pour afficher la Trace de l’Application de page, puis cliquez sur **afficher les détails**.</span><span class="sxs-lookup"><span data-stu-id="b01b4-355">Browse to **/Trace.axd** to see the Application Trace page, and then click **View Details**.</span></span>

    <span data-ttu-id="b01b4-356">![Journal des traces application](aspnet-mvc-4-dependency-injection/_static/image12.png "journal de suivi d’Application")</span><span class="sxs-lookup"><span data-stu-id="b01b4-356">![Application Trace Log](aspnet-mvc-4-dependency-injection/_static/image12.png "Application Trace Log")</span></span>

    <span data-ttu-id="b01b4-357">*Journal de suivi d’application*</span><span class="sxs-lookup"><span data-stu-id="b01b4-357">*Application Trace Log*</span></span>

    <span data-ttu-id="b01b4-358">![Trace de l’application - détails de la demande](aspnet-mvc-4-dependency-injection/_static/image13.png "Application Trace - détails de la requête")</span><span class="sxs-lookup"><span data-stu-id="b01b4-358">![Application Trace - Request Details](aspnet-mvc-4-dependency-injection/_static/image13.png "Application Trace - Request Details")</span></span>

    <span data-ttu-id="b01b4-359">*Trace de l’application - détails de la demande*</span><span class="sxs-lookup"><span data-stu-id="b01b4-359">*Application Trace - Request Details*</span></span>
4. <span data-ttu-id="b01b4-360">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="b01b4-360">Close the browser.</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="b01b4-361">Résumé</span><span class="sxs-lookup"><span data-stu-id="b01b4-361">Summary</span></span>

<span data-ttu-id="b01b4-362">À la fin de cet atelier pratique, vous avez appris à utiliser l’Injection de dépendances dans ASP.NET MVC 4 en intégrant Unity à l’aide d’un NuGet Package.</span><span class="sxs-lookup"><span data-stu-id="b01b4-362">By completing this Hands-On Lab you have learned how to use Dependency Injection in ASP.NET MVC 4 by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="b01b4-363">Pour ce faire, vous avez utilisé l’Injection de dépendances à l’intérieur des contrôleurs, des vues et des filtres d’action.</span><span class="sxs-lookup"><span data-stu-id="b01b4-363">To achieve that, you have used Dependency Injection inside controllers, views and action filters.</span></span>

<span data-ttu-id="b01b4-364">Les concepts suivants ont été traités :</span><span class="sxs-lookup"><span data-stu-id="b01b4-364">The following concepts were covered:</span></span>

- <span data-ttu-id="b01b4-365">Fonctionnalités de l’Injection de dépendances d’ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="b01b4-365">ASP.NET MVC 4 Dependency Injection features</span></span>
- <span data-ttu-id="b01b4-366">Intégration Unity à l’aide du NuGet Package de Unity.Mvc3</span><span class="sxs-lookup"><span data-stu-id="b01b4-366">Unity integration using Unity.Mvc3 NuGet Package</span></span>
- <span data-ttu-id="b01b4-367">Injection de dépendances dans les contrôleurs</span><span class="sxs-lookup"><span data-stu-id="b01b4-367">Dependency Injection in Controllers</span></span>
- <span data-ttu-id="b01b4-368">Injection de dépendances dans les vues</span><span class="sxs-lookup"><span data-stu-id="b01b4-368">Dependency Injection in Views</span></span>
- <span data-ttu-id="b01b4-369">Injection de dépendances de filtres d’Action</span><span class="sxs-lookup"><span data-stu-id="b01b4-369">Dependency injection of Action Filters</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="b01b4-370">Annexe a : installation de Visual Studio Express 2012 pour le Web</span><span class="sxs-lookup"><span data-stu-id="b01b4-370">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="b01b4-371">Vous pouvez installer **Microsoft Visual Studio Express 2012 pour Web** ou un autre &quot;Express&quot; à l’aide de la version du  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="b01b4-371">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="b01b4-372">Les instructions suivantes vous guident à travers les étapes requises pour installer *Visual studio Express 2012 pour le Web* à l’aide de *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="b01b4-372">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="b01b4-373">Accédez à [ [https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="b01b4-373">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="b01b4-374">Sinon, si vous avez déjà installé Web Platform Installer, vous pouvez ouvrir il et recherchez le produit &quot; *Visual Studio Express 2012 pour le Web avec Windows Azure SDK*&quot;.</span><span class="sxs-lookup"><span data-stu-id="b01b4-374">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="b01b4-375">Cliquez sur **installer maintenant**.</span><span class="sxs-lookup"><span data-stu-id="b01b4-375">Click on **Install Now**.</span></span> <span data-ttu-id="b01b4-376">Si vous n’avez pas **Web Platform Installer** vous allez être redirigé pour télécharger et installer tout d’abord.</span><span class="sxs-lookup"><span data-stu-id="b01b4-376">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="b01b4-377">Une fois **Web Platform Installer** est ouvert, cliquez sur **installer** pour démarrer le programme d’installation.</span><span class="sxs-lookup"><span data-stu-id="b01b4-377">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="b01b4-378">![Installer Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "installer Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="b01b4-378">![Install Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="b01b4-379">*Installer Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="b01b4-379">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="b01b4-380">Lisez les termes et les licences de tous les produits et cliquez sur **J’accepte** pour continuer.</span><span class="sxs-lookup"><span data-stu-id="b01b4-380">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Accepter les termes du contrat de licence](aspnet-mvc-4-dependency-injection/_static/image15.png)

    <span data-ttu-id="b01b4-382">*Accepter les termes du contrat de licence*</span><span class="sxs-lookup"><span data-stu-id="b01b4-382">*Accepting the license terms*</span></span>
5. <span data-ttu-id="b01b4-383">Attendez que le processus de téléchargement et l’installation se termine.</span><span class="sxs-lookup"><span data-stu-id="b01b4-383">Wait until the downloading and installation process completes.</span></span>

    ![Progression de l'installation](aspnet-mvc-4-dependency-injection/_static/image16.png)

    <span data-ttu-id="b01b4-385">*Progression de l’installation*</span><span class="sxs-lookup"><span data-stu-id="b01b4-385">*Installation progress*</span></span>
6. <span data-ttu-id="b01b4-386">Une fois l’installation terminée, cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="b01b4-386">When the installation completes, click **Finish**.</span></span>

    ![Installation est terminée](aspnet-mvc-4-dependency-injection/_static/image17.png)

    <span data-ttu-id="b01b4-388">*Installation est terminée*</span><span class="sxs-lookup"><span data-stu-id="b01b4-388">*Installation completed*</span></span>
7. <span data-ttu-id="b01b4-389">Cliquez sur **Exit** fermer Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="b01b4-389">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="b01b4-390">Pour ouvrir Visual Studio Express pour le Web, accédez à la **Démarrer** écran et démarrer l’écriture &quot; **VS Express**&quot;, puis cliquez sur le **Visual Studio Express pour le Web** vignette.</span><span class="sxs-lookup"><span data-stu-id="b01b4-390">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express pour la vignette du Web](aspnet-mvc-4-dependency-injection/_static/image18.png)

    <span data-ttu-id="b01b4-392">*VS Express pour la vignette du Web*</span><span class="sxs-lookup"><span data-stu-id="b01b4-392">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="b01b4-393">Annexe b : à l’aide d’extraits de Code</span><span class="sxs-lookup"><span data-stu-id="b01b4-393">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="b01b4-394">Avec des extraits de code, vous avez tout le code que vous avez besoin.</span><span class="sxs-lookup"><span data-stu-id="b01b4-394">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="b01b4-395">Le document lab vous indique exactement quand vous pouvez les utiliser, comme indiqué dans l’illustration suivante.</span><span class="sxs-lookup"><span data-stu-id="b01b4-395">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="b01b4-396">![À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet](aspnet-mvc-4-dependency-injection/_static/image19.png "des extraits de code à l’aide de Visual Studio pour insérer du code dans votre projet")</span><span class="sxs-lookup"><span data-stu-id="b01b4-396">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-dependency-injection/_static/image19.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="b01b4-397">*À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet*</span><span class="sxs-lookup"><span data-stu-id="b01b4-397">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="b01b4-398">***Pour ajouter un extrait de code à l’aide du clavier (c# uniquement)***</span><span class="sxs-lookup"><span data-stu-id="b01b4-398">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="b01b4-399">Placez le curseur où vous souhaitez insérer le code.</span><span class="sxs-lookup"><span data-stu-id="b01b4-399">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="b01b4-400">Commencez à taper le nom de l’extrait de code (sans espaces ou des traits d’union).</span><span class="sxs-lookup"><span data-stu-id="b01b4-400">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="b01b4-401">Observez comment IntelliSense affiche les noms des extraits de code de mise en correspondance.</span><span class="sxs-lookup"><span data-stu-id="b01b4-401">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="b01b4-402">Sélectionnez l’extrait de code correct (ou continuez à taper jusqu'à ce que le nom de l’extrait de code entier est sélectionné).</span><span class="sxs-lookup"><span data-stu-id="b01b4-402">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="b01b4-403">Appuyez sur la touche Tab à deux reprises pour insérer l’extrait de code à l’emplacement du curseur.</span><span class="sxs-lookup"><span data-stu-id="b01b4-403">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="b01b4-404">![Commencez à taper le nom de l’extrait de code](aspnet-mvc-4-dependency-injection/_static/image20.png "commencez à taper le nom de l’extrait de code")</span><span class="sxs-lookup"><span data-stu-id="b01b4-404">![Start typing the snippet name](aspnet-mvc-4-dependency-injection/_static/image20.png "Start typing the snippet name")</span></span>

<span data-ttu-id="b01b4-405">*Commencez à taper le nom de l’extrait de code*</span><span class="sxs-lookup"><span data-stu-id="b01b4-405">*Start typing the snippet name*</span></span>

<span data-ttu-id="b01b4-406">![Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance](aspnet-mvc-4-dependency-injection/_static/image21.png "appuyez sur Tab pour sélectionner l’extrait de code en surbrillance")</span><span class="sxs-lookup"><span data-stu-id="b01b4-406">![Press Tab to select the highlighted snippet](aspnet-mvc-4-dependency-injection/_static/image21.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="b01b4-407">*Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance*</span><span class="sxs-lookup"><span data-stu-id="b01b4-407">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="b01b4-408">![Appuyez sur Tab à nouveau et l’extrait de code sont développés](aspnet-mvc-4-dependency-injection/_static/image22.png "appuyez sur Tab à nouveau et l’extrait de code seront développe.")</span><span class="sxs-lookup"><span data-stu-id="b01b4-408">![Press Tab again and the snippet will expand](aspnet-mvc-4-dependency-injection/_static/image22.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="b01b4-409">*Appuyez sur Tab à nouveau et l’extrait de code seront développe.*</span><span class="sxs-lookup"><span data-stu-id="b01b4-409">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="b01b4-410">***Pour ajouter un extrait de code à l’aide de la souris (c#, Visual Basic et XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="b01b4-410">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="b01b4-411">Clic droit où vous souhaitez insérer l’extrait de code.</span><span class="sxs-lookup"><span data-stu-id="b01b4-411">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="b01b4-412">Sélectionnez **insérer un extrait** suivie **mes extraits de Code**.</span><span class="sxs-lookup"><span data-stu-id="b01b4-412">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="b01b4-413">Sélectionnez l’extrait de code approprié dans la liste, en cliquant dessus.</span><span class="sxs-lookup"><span data-stu-id="b01b4-413">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="b01b4-414">![Avec le bouton sur lequel vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait](aspnet-mvc-4-dependency-injection/_static/image23.png "avec le bouton sur lequel vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait")</span><span class="sxs-lookup"><span data-stu-id="b01b4-414">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-dependency-injection/_static/image23.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="b01b4-415">*Avec le bouton droit sur lequel vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait*</span><span class="sxs-lookup"><span data-stu-id="b01b4-415">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="b01b4-416">![Sélectionnez l’extrait de code approprié dans la liste, en cliquant dessus](aspnet-mvc-4-dependency-injection/_static/image24.png "choisir l’extrait de code approprié dans la liste, en cliquant sur celle-ci")</span><span class="sxs-lookup"><span data-stu-id="b01b4-416">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-dependency-injection/_static/image24.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="b01b4-417">*Sélectionnez l’extrait de code approprié dans la liste, en cliquant sur celle-ci*</span><span class="sxs-lookup"><span data-stu-id="b01b4-417">*Pick the relevant snippet from the list, by clicking on it*</span></span>
