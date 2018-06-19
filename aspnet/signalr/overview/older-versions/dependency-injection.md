---
uid: signalr/overview/older-versions/dependency-injection
title: Injection de dépendances dans SignalR 1.x | Documents Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/15/2013
ms.topic: article
ms.assetid: eaa206c4-edb3-487e-8fcb-54a3261fed36
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 6fd155adc9a0aa71b66db7a51062a51fb0c1feca
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26505488"
---
<a name="dependency-injection-in-signalr-1x"></a><span data-ttu-id="7aeb4-102">Injection de dépendances dans SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="7aeb4-102">Dependency Injection in SignalR 1.x</span></span>
====================
<span data-ttu-id="7aeb4-103">par [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="7aeb4-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="7aeb4-104">Injection de dépendance est un moyen pour supprimer des dépendances codées en dur entre les objets, facilitant ainsi la pour remplacer des dépendances d’un objet, soit pour le test (à l’aide d’objets fictifs) ou pour modifier le comportement d’exécution.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-104">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="7aeb4-105">Ce didacticiel montre comment effectuer l’injection de dépendances sur les concentrateurs SignalR.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-105">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="7aeb4-106">Il montre également comment utiliser des conteneurs d’inversion de contrôle avec SignalR.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-106">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="7aeb4-107">Un conteneur inversion de contrôle est une infrastructure générale pour l’injection de dépendance.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-107">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="7aeb4-108">Nouveautés d’Injection de dépendance ?</span><span class="sxs-lookup"><span data-stu-id="7aeb4-108">What is Dependency Injection?</span></span>

<span data-ttu-id="7aeb4-109">Ignorez cette section si vous êtes déjà familiarisé avec l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-109">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="7aeb4-110">*Injection de dépendance* (DI) est un modèle où les objets ne sont pas chargées de créer leurs propres dépendances.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-110">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="7aeb4-111">Voici un exemple simple motiver DI.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-111">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="7aeb4-112">Supposons que vous avez un objet qui a besoin d’enregistrer des messages.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-112">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="7aeb4-113">Vous pouvez définir une interface de journalisation :</span><span class="sxs-lookup"><span data-stu-id="7aeb4-113">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="7aeb4-114">Dans votre objet, vous pouvez créer un `ILogger` pour enregistrer des messages :</span><span class="sxs-lookup"><span data-stu-id="7aeb4-114">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="7aeb4-115">Cela fonctionne, mais il n’est pas la meilleure conception.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-115">This works, but it's not the best design.</span></span> <span data-ttu-id="7aeb4-116">Si vous souhaitez remplacer `FileLogger` avec un autre `ILogger` mise en œuvre, vous devez modifier `SomeComponent`.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-116">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="7aeb4-117">Supposer que beaucoup d’autres objets utilisent `FileLogger`, vous devez modifier tous les.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-117">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="7aeb4-118">Ou si vous décidez d’apporter `FileLogger` un singleton, vous devez également apporter des modifications à l’application.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-118">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="7aeb4-119">Une meilleure approche consiste à « injecter » un `ILogger` dans l’objet, par exemple, en utilisant un argument de constructeur :</span><span class="sxs-lookup"><span data-stu-id="7aeb4-119">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="7aeb4-120">À présent l’objet n’est pas responsable de la sélection qui `ILogger` à utiliser.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-120">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="7aeb4-121">Vous pouvez swich `ILogger` implémentations sans modifier les objets qui en dépendent.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-121">You can swich `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="7aeb4-122">Ce modèle est appelé [injection de constructeur](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="7aeb4-122">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="7aeb4-123">Un autre modèle est l’injection de setter, où vous définissez la dépendance via une méthode setter ou une propriété.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-123">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="7aeb4-124">Injection de dépendances simple dans SignalR</span><span class="sxs-lookup"><span data-stu-id="7aeb4-124">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="7aeb4-125">Envisagez de l’application de conversation à partir du didacticiel [mise en route avec SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="7aeb4-125">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="7aeb4-126">Voici la classe de concentrateur à partir de cette application :</span><span class="sxs-lookup"><span data-stu-id="7aeb4-126">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="7aeb4-127">Supposons que vous souhaitez stocker les messages de conversation sur le serveur avant de les envoyer.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-127">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="7aeb4-128">Vous pouvez définir une interface qui s’approprie cette fonctionnalité et utiliser DI injecter l’interface dans le `ChatHub` classe.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-128">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="7aeb4-129">Le seul problème est qu’une application SignalR ne crée pas directement les concentrateurs ; SignalR crée pour vous.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-129">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="7aeb4-130">Par défaut, SignalR attend une classe de concentrateur à avoir un constructeur sans paramètre.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-130">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="7aeb4-131">Toutefois, vous pouvez facilement inscrire une fonction pour créer des instances de concentrateur et utiliser cette fonction pour effectuer DI.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-131">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="7aeb4-132">Inscription de la fonction en appelant **GlobalHost.DependencyResolver.Register**.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-132">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="7aeb4-133">Maintenant SignalR appelle cette fonction anonyme chaque fois qu’il a besoin créer un `ChatHub` instance.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-133">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="7aeb4-134">Conteneurs d’inversion de contrôle</span><span class="sxs-lookup"><span data-stu-id="7aeb4-134">IoC Containers</span></span>

<span data-ttu-id="7aeb4-135">Le code précédent est tout indiquée pour les cas simples.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-135">The previous code is fine for simple cases.</span></span> <span data-ttu-id="7aeb4-136">C’est toujours écrire ceci :</span><span class="sxs-lookup"><span data-stu-id="7aeb4-136">But you still had to write this:</span></span>

[!code-css[Main](dependency-injection/samples/sample8.css)]

<span data-ttu-id="7aeb4-137">Dans une application complexe avec de nombreuses dépendances, vous devrez écrire un lot de ce code « connexion ».</span><span class="sxs-lookup"><span data-stu-id="7aeb4-137">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="7aeb4-138">Ce code peut être difficile à maintenir, particulièrement si les dépendances sont imbriquées.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-138">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="7aeb4-139">Il est également difficile de test unitaire.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-139">It is also hard to unit test.</span></span>

<span data-ttu-id="7aeb4-140">Une solution consiste à utiliser un conteneur inversion de contrôle.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-140">One solution is to use an IoC container.</span></span> <span data-ttu-id="7aeb4-141">Un conteneur inversion de contrôle est un composant logiciel qui est chargé de gérer les dépendances. Vous inscrivez les types avec le conteneur et ensuite utilisez le conteneur pour créer des objets.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-141">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="7aeb4-142">Le conteneur effectue automatiquement les relations de dépendance.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-142">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="7aeb4-143">De nombreux conteneurs d’inversion de contrôle vous permettent également de contrôler les éléments tels que la durée de vie et la portée.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-143">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="7aeb4-144">« IoC » signifie « d’inversion de contrôle », qui est un modèle général où une infrastructure appelle du code d’application.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-144">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="7aeb4-145">Un conteneur IoC construit vos objets, lequel « inverse » le flux habituel de contrôle.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-145">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="7aeb4-146">À l’aide de conteneurs d’inversion de contrôle dans SignalR</span><span class="sxs-lookup"><span data-stu-id="7aeb4-146">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="7aeb4-147">L’application de la conversation est probablement trop simple pour tirer parti d’un conteneur inversion de contrôle.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-147">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="7aeb4-148">Au lieu de cela, examinons la [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) exemple.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-148">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="7aeb4-149">L’exemple StockTicker définit deux classes principales :</span><span class="sxs-lookup"><span data-stu-id="7aeb4-149">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="7aeb4-150">`StockTickerHub`: La classe de concentrateur, qui gère les connexions clientes.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-150">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="7aeb4-151">`StockTicker`: Un singleton qui conserve des actions et les met à jour régulièrement.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-151">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="7aeb4-152">`StockTickerHub`contient une référence à la `StockTicker` singleton, tandis que `StockTicker` contient une référence à la **IHubConnectionContext** pour le `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-152">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="7aeb4-153">Il utilise cette interface pour communiquer avec `StockTickerHub` instances.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-153">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="7aeb4-154">(Pour plus d’informations, consultez [serveur de diffusion avec ASP.NET SignalR](index.md).)</span><span class="sxs-lookup"><span data-stu-id="7aeb4-154">(For more information, see [Server Broadcast with ASP.NET SignalR](index.md).)</span></span>

<span data-ttu-id="7aeb4-155">Nous pouvons utiliser un conteneur inversion de contrôle à ces dépendances un peu.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-155">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="7aeb4-156">Tout d’abord, nous allons simplifier le `StockTickerHub` et `StockTicker` classes.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-156">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="7aeb4-157">Dans le code suivant, j’ai mis en commentaire les parties que nous n’avez pas besoin.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-157">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="7aeb4-158">Supprimez le constructeur sans paramètre à partir de `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-158">Remove the parameterless constructor from `StockTicker`.</span></span> <span data-ttu-id="7aeb4-159">Au lieu de cela, nous allons utiliser toujours DI pour créer le concentrateur.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-159">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="7aeb4-160">Pour StockTicker, supprimez l’instance de singleton.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-160">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="7aeb4-161">Une version ultérieure, nous allons utiliser le conteneur inversion de contrôle pour contrôler la durée de vie StockTicker.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-161">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="7aeb4-162">En outre, rendez le constructeur public.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-162">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="7aeb4-163">Ensuite, nous pouvons refactoriser le code en créant une interface pour `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-163">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="7aeb4-164">Nous allons utiliser cette interface pour découpler le `StockTickerHub` à partir de la `StockTicker` classe.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-164">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="7aeb4-165">Visual Studio apporte facilement ce type de refactorisation.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-165">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="7aeb4-166">Ouvrez le fichier StockTicker.cs, avec le bouton droit sur le `StockTicker` déclaration de classe, puis sélectionnez **refactoriser** ... **Extraire l’Interface**.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-166">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="7aeb4-167">Dans le **extraire l’Interface** boîte de dialogue, cliquez sur **sélectionner tout**.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-167">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="7aeb4-168">Laissez les valeurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-168">Leave the other defaults.</span></span> <span data-ttu-id="7aeb4-169">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-169">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="7aeb4-170">Visual Studio crée une nouvelle interface nommée `IStockTicker`et modifie également `StockTicker` dériver `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-170">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="7aeb4-171">Ouvrez le fichier IStockTicker.cs et de modifier l’interface à **public**.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-171">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="7aeb4-172">Dans le `StockTickerHub` de classe, modifiez les deux instances de `StockTicker` à `IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="7aeb4-172">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="7aeb4-173">Création d’un `IStockTicker` interface n’est pas strictement nécessaire, mais je souhaitais montrer comment les DI peut aider à réduire le couplage entre les composants de votre application.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-173">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="7aeb4-174">Ajoutez la bibliothèque Ninject</span><span class="sxs-lookup"><span data-stu-id="7aeb4-174">Add the Ninject Library</span></span>

<span data-ttu-id="7aeb4-175">Il existe de nombreux conteneurs d’inversion de contrôle open source pour .NET.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-175">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="7aeb4-176">Pour ce didacticiel, je vais utiliser [Ninject](http://www.ninject.org/).</span><span class="sxs-lookup"><span data-stu-id="7aeb4-176">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="7aeb4-177">(Incluent d’autres bibliothèques populaires [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), et [StructureMap ](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="7aeb4-177">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="7aeb4-178">Utilisez le Gestionnaire de Package NuGet pour installer le [Ninject bibliothèque](https://nuget.org/packages/Ninject/3.0.1.10).</span><span class="sxs-lookup"><span data-stu-id="7aeb4-178">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="7aeb4-179">Dans Visual Studio, à partir de la **outils** menu Sélectionnez **Gestionnaire de Package de bibliothèque** | **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-179">In Visual Studio, from the **Tools** menu select **Library Package Manager** | **Package Manager Console**.</span></span> <span data-ttu-id="7aeb4-180">Dans la fenêtre de Console du Gestionnaire de Package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="7aeb4-180">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="7aeb4-181">Remplacez le résolveur de dépendance SignalR</span><span class="sxs-lookup"><span data-stu-id="7aeb4-181">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="7aeb4-182">Pour utiliser Ninject dans SignalR, créez une classe qui dérive de **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-182">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="7aeb4-183">Cette classe substitue le **GetService** et **GetServices** méthodes de **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-183">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="7aeb4-184">SignalR appelle ces méthodes pour créer divers objets lors de l’exécution, y compris les instances de concentrateur, ainsi que divers services utilisés en interne par SignalR.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-184">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="7aeb4-185">Le **GetService** méthode crée une instance unique d’un type.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-185">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="7aeb4-186">Substituez cette méthode pour appeler le noyau de Ninject **TryGet** (méthode).</span><span class="sxs-lookup"><span data-stu-id="7aeb4-186">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="7aeb4-187">Si cette méthode retourne la valeur null, revenir au programme de résolution par défaut.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-187">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="7aeb4-188">Le **GetServices** méthode crée une collection d’objets d’un type spécifié.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-188">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="7aeb4-189">Substituez cette méthode pour concaténer les résultats de Ninject avec les résultats à partir du programme de résolution par défaut.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-189">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="7aeb4-190">Configurer les liaisons de Ninject</span><span class="sxs-lookup"><span data-stu-id="7aeb4-190">Configure Ninject Bindings</span></span>

<span data-ttu-id="7aeb4-191">Maintenant, nous allons utiliser Ninject pour déclarer des liaisons de type.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-191">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="7aeb4-192">Ouvrez le fichier RegisterHubs.cs.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-192">Open the file RegisterHubs.cs.</span></span> <span data-ttu-id="7aeb4-193">Dans le `RegisterHubs.Start` (méthode), créez le conteneur de Ninject Ninject appelle la *noyau*.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-193">In the `RegisterHubs.Start` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="7aeb4-194">Créez une instance de notre programme de résolution de dépendance personnalisée :</span><span class="sxs-lookup"><span data-stu-id="7aeb4-194">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="7aeb4-195">Créer une liaison pour `IStockTicker` comme suit :</span><span class="sxs-lookup"><span data-stu-id="7aeb4-195">Create a binding for `IStockTicker` as follows:</span></span>

[!code-html[Main](dependency-injection/samples/sample17.html)]

<span data-ttu-id="7aeb4-196">Ce code indique deux choses.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-196">This code is saying two things.</span></span> <span data-ttu-id="7aeb4-197">Premier, chaque fois que l’application a besoin un `IStockTicker`, le noyau doit créer une instance de `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-197">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="7aeb4-198">Ensuite, la `StockTicker` classe doit être créé en tant qu’objet singleton.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-198">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="7aeb4-199">Ninject crée une instance de l’objet et retournent la même instance pour chaque demande.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-199">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="7aeb4-200">Créer une liaison pour **IHubConnectionContext** comme suit :</span><span class="sxs-lookup"><span data-stu-id="7aeb4-200">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="7aeb4-201">Ce code de creatres une fonction anonyme qui retourne un **IHubConnection**.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-201">This code creatres an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="7aeb4-202">Le **WhenInjectedInto** méthode indique Ninject pour utiliser cette fonction uniquement lors de la création `IStockTicker` instances.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-202">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="7aeb4-203">La raison est que SignalR crée **IHubConnectionContext** instances en interne, et nous ne souhaitez pas remplacer la SignalR les crée.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-203">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="7aeb4-204">Cette fonction s’applique uniquement à notre `StockTicker` classe.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-204">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="7aeb4-205">Passez le résolveur de dépendance dans le **MapHubs** méthode :</span><span class="sxs-lookup"><span data-stu-id="7aeb4-205">Pass the dependency resolver into the **MapHubs** method:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="7aeb4-206">Présent SignalR utilise le programme de résolution spécifié dans **MapHubs**, au lieu du résolveur par défaut.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-206">Now SignalR will use the resolver specified in **MapHubs**, instead of the default resolver.</span></span>

<span data-ttu-id="7aeb4-207">Voici le code complet pour `RegisterHubs.Start`.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-207">Here is the complete code listing for `RegisterHubs.Start`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="7aeb4-208">Pour exécuter l’application StockTicker dans Visual Studio, appuyez sur F5.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-208">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="7aeb4-209">Dans la fenêtre du navigateur, accédez à `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-209">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="7aeb4-210">L’application a exactement les mêmes fonctionnalités qu’avant.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-210">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="7aeb4-211">(Pour obtenir une description, consultez [serveur de diffusion avec ASP.NET SignalR](index.md).) Nous n’avons pas de modifier le comportement ; simplement rend le code plus facile à tester, gérer et faire évoluer.</span><span class="sxs-lookup"><span data-stu-id="7aeb4-211">(For a description, see [Server Broadcast with ASP.NET SignalR](index.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
