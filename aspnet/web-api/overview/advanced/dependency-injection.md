---
uid: web-api/overview/advanced/dependency-injection
title: L’Injection de dépendances dans ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Ce didacticiel montre comment injecter des dépendances dans votre contrôleur d’API Web ASP.NET. Versions des logiciels utilisées dans le didacticiel Web API 2 Unity Application Block...
ms.author: riande
ms.date: 01/20/2014
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 318a2f1c587feb360212a390bb5de7bdc127513d
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341824"
---
<a name="dependency-injection-in-aspnet-web-api-2"></a><span data-ttu-id="a771e-104">Injection de dépendances dans ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="a771e-104">Dependency Injection in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="a771e-105">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a771e-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="a771e-106">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="a771e-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> <span data-ttu-id="a771e-107">Ce didacticiel montre comment injecter des dépendances dans votre contrôleur d’API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a771e-107">This tutorial shows how to inject dependencies into your ASP.NET Web API controller.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="a771e-108">Versions des logiciels utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="a771e-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="a771e-109">Web API 2</span><span class="sxs-lookup"><span data-stu-id="a771e-109">Web API 2</span></span>
> - [<span data-ttu-id="a771e-110">Unity Application Block</span><span class="sxs-lookup"><span data-stu-id="a771e-110">Unity Application Block</span></span>](https://www.nuget.org/packages/Unity/)
> - <span data-ttu-id="a771e-111">Entity Framework 6 (version 5 fonctionne également)</span><span class="sxs-lookup"><span data-stu-id="a771e-111">Entity Framework 6 (version 5 also works)</span></span>


## <a name="what-is-dependency-injection"></a><span data-ttu-id="a771e-112">Quelle est l’Injection de dépendances ?</span><span class="sxs-lookup"><span data-stu-id="a771e-112">What is Dependency Injection?</span></span>

<span data-ttu-id="a771e-113">Une *dépendance* est un objet qui nécessite un autre objet.</span><span class="sxs-lookup"><span data-stu-id="a771e-113">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="a771e-114">Par exemple, il est courant de définir un [référentiel](http://martinfowler.com/eaaCatalog/repository.html) qui gère l’accès aux données.</span><span class="sxs-lookup"><span data-stu-id="a771e-114">For example, it's common to define a [repository](http://martinfowler.com/eaaCatalog/repository.html) that handles data access.</span></span> <span data-ttu-id="a771e-115">Nous allons illustrer avec un exemple.</span><span class="sxs-lookup"><span data-stu-id="a771e-115">Let's illustrate with an example.</span></span> <span data-ttu-id="a771e-116">Tout d’abord, nous allons définir un modèle de domaine :</span><span class="sxs-lookup"><span data-stu-id="a771e-116">First, we'll define a domain model:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="a771e-117">Voici une classe de référentiel simple qui stocke des éléments dans une base de données, à l’aide d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a771e-117">Here is a simple repository class that stores items in a database, using Entity Framework.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="a771e-118">Maintenant, nous allons définir un contrôleur d’API Web qui prend en charge les requêtes GET `Product` entités.</span><span class="sxs-lookup"><span data-stu-id="a771e-118">Now let's define a Web API controller that supports GET requests for `Product` entities.</span></span> <span data-ttu-id="a771e-119">(Je laisse les POST et les autres méthodes par souci de simplicité.) Voici une première tentative de :</span><span class="sxs-lookup"><span data-stu-id="a771e-119">(I'm leaving out POST and other methods for simplicity.) Here is a first attempt:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="a771e-120">Notez que la classe de contrôleur dépend `ProductRepository`, et nous allons laisser le contrôleur de créer le `ProductRepository` instance.</span><span class="sxs-lookup"><span data-stu-id="a771e-120">Notice that the controller class depends on `ProductRepository`, and we are letting the controller create the `ProductRepository` instance.</span></span> <span data-ttu-id="a771e-121">Toutefois, il est déconseillé de coder en dur la dépendance de cette façon, pour plusieurs raisons.</span><span class="sxs-lookup"><span data-stu-id="a771e-121">However, it's a bad idea to hard code the dependency in this way, for several reasons.</span></span>

- <span data-ttu-id="a771e-122">Si vous souhaitez remplacer `ProductRepository` avec une implémentation différente, vous devez également modifier la classe de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="a771e-122">If you want to replace `ProductRepository` with a different implementation, you also need to modify the controller class.</span></span>
- <span data-ttu-id="a771e-123">Si le `ProductRepository` possède des dépendances, vous devez configurer dans le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="a771e-123">If the `ProductRepository` has dependencies, you must configure these inside the controller.</span></span> <span data-ttu-id="a771e-124">Pour un grand projet avec plusieurs contrôleurs, votre code de configuration devienne répartis sur votre projet.</span><span class="sxs-lookup"><span data-stu-id="a771e-124">For a large project with multiple controllers, your configuration code becomes scattered across your project.</span></span>
- <span data-ttu-id="a771e-125">Il est difficile de test unitaire, étant donné que le contrôleur est codé en dur pour interroger la base de données.</span><span class="sxs-lookup"><span data-stu-id="a771e-125">It is hard to unit test, because the controller is hard-coded to query the database.</span></span> <span data-ttu-id="a771e-126">Pour un test unitaire, vous devez utiliser un référentiel simulacre ou stub, ce qui n’est pas possible avec la conception faites-en.</span><span class="sxs-lookup"><span data-stu-id="a771e-126">For a unit test, you should use a mock or stub repository, which is not possible with the currect design.</span></span>

<span data-ttu-id="a771e-127">Nous pouvons résoudre ces problèmes en *injection* le référentiel dans le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="a771e-127">We can address these problems by *injecting* the repository into the controller.</span></span> <span data-ttu-id="a771e-128">Tout d’abord, refactoriser la `ProductRepository` classe dans une interface :</span><span class="sxs-lookup"><span data-stu-id="a771e-128">First, refactor the `ProductRepository` class into an interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="a771e-129">Puis fournissez le `IProductRepository` comme paramètre de constructeur :</span><span class="sxs-lookup"><span data-stu-id="a771e-129">Then provide the `IProductRepository` as a constructor parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="a771e-130">Cet exemple utilise [l’injection de constructeur](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="a771e-130">This example uses [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="a771e-131">Vous pouvez également utiliser *l’injection de setter*vous permet de définir la dépendance par une méthode setter ou une propriété.</span><span class="sxs-lookup"><span data-stu-id="a771e-131">You can also use *setter injection*, where you set the dependency through a setter method or property.</span></span>

<span data-ttu-id="a771e-132">Mais maintenant il existe un problème, étant donné que votre application ne crée pas directement le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="a771e-132">But now there is a problem, because your application doesn't create the controller directly.</span></span> <span data-ttu-id="a771e-133">API Web crée le contrôleur lorsqu’il achemine la demande et API Web ne sait pas quoi que ce soit sur `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="a771e-133">Web API creates the controller when it routes the request, and Web API doesn't know anything about `IProductRepository`.</span></span> <span data-ttu-id="a771e-134">Il s’agit là qu’intervient le résolveur de dépendance d’API Web.</span><span class="sxs-lookup"><span data-stu-id="a771e-134">This is where the Web API dependency resolver comes in.</span></span>

## <a name="the-web-api-dependency-resolver"></a><span data-ttu-id="a771e-135">Le résolveur de dépendance d’API Web</span><span class="sxs-lookup"><span data-stu-id="a771e-135">The Web API Dependency Resolver</span></span>

<span data-ttu-id="a771e-136">API Web définit les **IDependencyResolver** interface pour la résolution des dépendances.</span><span class="sxs-lookup"><span data-stu-id="a771e-136">Web API defines the **IDependencyResolver** interface for resolving dependencies.</span></span> <span data-ttu-id="a771e-137">Voici la définition de l’interface :</span><span class="sxs-lookup"><span data-stu-id="a771e-137">Here is the definition of the interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="a771e-138">Le **IDependencyScope** interface possède deux méthodes :</span><span class="sxs-lookup"><span data-stu-id="a771e-138">The **IDependencyScope** interface has two methods:</span></span>

- <span data-ttu-id="a771e-139">**GetService** crée une instance d’un type.</span><span class="sxs-lookup"><span data-stu-id="a771e-139">**GetService** creates one instance of a type.</span></span>
- <span data-ttu-id="a771e-140">**GetServices** crée une collection d’objets d’un type spécifié.</span><span class="sxs-lookup"><span data-stu-id="a771e-140">**GetServices** creates a collection of objects of a specified type.</span></span>

<span data-ttu-id="a771e-141">Le **IDependencyResolver** hérite de la méthode **IDependencyScope** et ajoute le **BeginScope** (méthode).</span><span class="sxs-lookup"><span data-stu-id="a771e-141">The **IDependencyResolver** method inherits **IDependencyScope** and adds the **BeginScope** method.</span></span> <span data-ttu-id="a771e-142">Je parlerai étendues plus loin dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="a771e-142">I'll talk about scopes later in this tutorial.</span></span>

<span data-ttu-id="a771e-143">Lors de l’API Web crée une instance de contrôleur, il appelle d’abord **IDependencyResolver.GetService**, en passant le type de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="a771e-143">When Web API creates a controller instance, it first calls **IDependencyResolver.GetService**, passing in the controller type.</span></span> <span data-ttu-id="a771e-144">Vous pouvez utiliser ce hook d’extensibilité pour créer le contrôleur, résolution des dépendances.</span><span class="sxs-lookup"><span data-stu-id="a771e-144">You can use this extensibility hook to create the controller, resolving any dependencies.</span></span> <span data-ttu-id="a771e-145">Si **GetService** retourne null, recherche un constructeur sans paramètre sur la classe de contrôleur API Web.</span><span class="sxs-lookup"><span data-stu-id="a771e-145">If **GetService** returns null, Web API looks for a parameterless constructor on the controller class.</span></span>

## <a name="dependency-resolution-with-the-unity-container"></a><span data-ttu-id="a771e-146">Résolution des dépendances avec le conteneur Unity</span><span class="sxs-lookup"><span data-stu-id="a771e-146">Dependency Resolution with the Unity Container</span></span>

<span data-ttu-id="a771e-147">Bien que vous pouvez écrire un complète **IDependencyResolver** implémentation à partir de zéro, l’interface est vraiment conçue pour agir en tant que pont entre les API Web et les conteneurs d’inversion de contrôle existants.</span><span class="sxs-lookup"><span data-stu-id="a771e-147">Although you could write a complete **IDependencyResolver** implementation from scratch, the interface is really designed to act as bridge between Web API and existing IoC containers.</span></span>

<span data-ttu-id="a771e-148">Un conteneur IoC est un composant logiciel qui est responsable de la gestion des dépendances.</span><span class="sxs-lookup"><span data-stu-id="a771e-148">An IoC container is a software component that is responsible for managing dependencies.</span></span> <span data-ttu-id="a771e-149">Vous inscrivez des types auprès du conteneur et ensuite utilisez le conteneur pour créer des objets.</span><span class="sxs-lookup"><span data-stu-id="a771e-149">You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="a771e-150">Le conteneur détermine automatiquement les relations de dépendance.</span><span class="sxs-lookup"><span data-stu-id="a771e-150">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="a771e-151">De nombreux conteneurs d’inversion de contrôle vous permettent également de vous permettent de contrôler des éléments tels que la durée de vie et la portée.</span><span class="sxs-lookup"><span data-stu-id="a771e-151">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="a771e-152">« IoC » est l’acronyme « d’inversion de contrôle », qui est un modèle général où un framework appelle du code d’application.</span><span class="sxs-lookup"><span data-stu-id="a771e-152">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="a771e-153">Un conteneur IoC construit vos objets, ce qui le flux habituel de contrôle « inverse ».</span><span class="sxs-lookup"><span data-stu-id="a771e-153">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


<span data-ttu-id="a771e-154">Pour ce didacticiel, nous allons utiliser [Unity](https://msdn.microsoft.com/library/ff647202.aspx) à partir de Microsoft Patterns &amp; pratiques.</span><span class="sxs-lookup"><span data-stu-id="a771e-154">For this tutorial, we'll use [Unity](https://msdn.microsoft.com/library/ff647202.aspx) from Microsoft Patterns &amp; Practices.</span></span> <span data-ttu-id="a771e-155">(Incluent d’autres bibliothèques populaires [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), et [StructureMap ](http://structuremap.github.io/documentation/).) Vous pouvez utiliser le Gestionnaire de Package NuGet pour installer Unity.</span><span class="sxs-lookup"><span data-stu-id="a771e-155">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), and [StructureMap](http://structuremap.github.io/documentation/).) You can use NuGet Package Manager to install Unity.</span></span> <span data-ttu-id="a771e-156">À partir de la **outils** menu dans Visual Studio, sélectionnez **Gestionnaire de Package NuGet**, puis sélectionnez **Console du Gestionnaire de Package**.</span><span class="sxs-lookup"><span data-stu-id="a771e-156">From the **Tools** menu in Visual Studio, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="a771e-157">Dans la fenêtre de Console du Gestionnaire de Package, tapez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="a771e-157">In the Package Manager Console window, type the following command:</span></span>

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

<span data-ttu-id="a771e-158">Voici une implémentation de **IDependencyResolver** qui encapsule un conteneur Unity.</span><span class="sxs-lookup"><span data-stu-id="a771e-158">Here is an implementation of **IDependencyResolver** that wraps a Unity container.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="a771e-159">Si le **GetService** méthode ne peut pas résoudre un type, elle doit retourner **null**.</span><span class="sxs-lookup"><span data-stu-id="a771e-159">If the **GetService** method cannot resolve a type, it should return **null**.</span></span> <span data-ttu-id="a771e-160">Si le **GetServices** méthode ne peut pas résoudre un type, elle doit retourner un objet de collection vide.</span><span class="sxs-lookup"><span data-stu-id="a771e-160">If the **GetServices** method cannot resolve a type, it should return an empty collection object.</span></span> <span data-ttu-id="a771e-161">Ne pas lever d’exceptions pour les types inconnus.</span><span class="sxs-lookup"><span data-stu-id="a771e-161">Don't throw exceptions for unknown types.</span></span>


## <a name="configuring-the-dependency-resolver"></a><span data-ttu-id="a771e-162">Configurer le résolveur de dépendance</span><span class="sxs-lookup"><span data-stu-id="a771e-162">Configuring the Dependency Resolver</span></span>

<span data-ttu-id="a771e-163">Définir le résolveur de dépendance sur le **DependencyResolver** propriété de global **HttpConfiguration** objet.</span><span class="sxs-lookup"><span data-stu-id="a771e-163">Set the dependency resolver on the **DependencyResolver** property of the global **HttpConfiguration** object.</span></span>

<span data-ttu-id="a771e-164">Le code suivant enregistre le `IProductRepository` interface avec Unity et crée ensuite un `UnityResolver`.</span><span class="sxs-lookup"><span data-stu-id="a771e-164">The following code registers the `IProductRepository` interface with Unity and then creates a `UnityResolver`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a><span data-ttu-id="a771e-165">Portée des dépendances et durée de vie de contrôleur</span><span class="sxs-lookup"><span data-stu-id="a771e-165">Dependency Scope and Controller Lifetime</span></span>

<span data-ttu-id="a771e-166">Les contrôleurs sont créés par demande.</span><span class="sxs-lookup"><span data-stu-id="a771e-166">Controllers are created per request.</span></span> <span data-ttu-id="a771e-167">Pour gérer les durées de vie des objets, **IDependencyResolver** utilise le concept d’un *étendue*.</span><span class="sxs-lookup"><span data-stu-id="a771e-167">To manage object lifetimes, **IDependencyResolver** uses the concept of a *scope*.</span></span>

<span data-ttu-id="a771e-168">Le résolveur de dépendance attachée à la **HttpConfiguration** objet a une portée globale.</span><span class="sxs-lookup"><span data-stu-id="a771e-168">The dependency resolver attached to the **HttpConfiguration** object has global scope.</span></span> <span data-ttu-id="a771e-169">Lors de l’API Web crée un contrôleur, il appelle **BeginScope**.</span><span class="sxs-lookup"><span data-stu-id="a771e-169">When Web API creates a controller, it calls **BeginScope**.</span></span> <span data-ttu-id="a771e-170">Cette méthode retourne un **IDependencyScope** qui représente une étendue enfant.</span><span class="sxs-lookup"><span data-stu-id="a771e-170">This method returns an **IDependencyScope** that represents a child scope.</span></span>

<span data-ttu-id="a771e-171">API Web appelle ensuite **GetService** sur l’étendue enfant pour créer le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="a771e-171">Web API then calls **GetService** on the child scope to create the controller.</span></span> <span data-ttu-id="a771e-172">Lors de la demande est terminée, les API Web appelle **Dispose** sur l’étendue enfant.</span><span class="sxs-lookup"><span data-stu-id="a771e-172">When request is complete, Web API calls **Dispose** on the child scope.</span></span> <span data-ttu-id="a771e-173">Utilisez le **Dispose** méthode pour supprimer les dépendances du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="a771e-173">Use the **Dispose** method to dispose of the controller's dependencies.</span></span>

<span data-ttu-id="a771e-174">Comment implémenter **BeginScope** varie selon le conteneur IoC.</span><span class="sxs-lookup"><span data-stu-id="a771e-174">How you implement **BeginScope** depends on the IoC container.</span></span> <span data-ttu-id="a771e-175">Pour Unity, étendue correspond à un conteneur enfant :</span><span class="sxs-lookup"><span data-stu-id="a771e-175">For Unity, scope corresponds to a child container:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

<span data-ttu-id="a771e-176">La plupart des conteneurs IoC ont des équivalents similaire.</span><span class="sxs-lookup"><span data-stu-id="a771e-176">Most IoC containers have similar equivalents.</span></span>
