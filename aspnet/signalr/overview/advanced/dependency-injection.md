---
uid: signalr/overview/advanced/dependency-injection
title: "Injection de dépendances dans SignalR | Documents Microsoft"
author: MikeWasson
description: "Versions du logiciel utilisé dans cette rubrique Visual Studio 2013 .NET 4.5 SignalR les versions précédentes de la version 2 de cette rubrique pour plus d’informations sur les versions antérieures de..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 3732b5d0ea6de841a6c402bfd5ef4dfb7b7a9162
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="dependency-injection-in-signalr"></a><span data-ttu-id="165f1-103">Injection de dépendances dans SignalR</span><span class="sxs-lookup"><span data-stu-id="165f1-103">Dependency Injection in SignalR</span></span>
====================
<span data-ttu-id="165f1-104">par [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="165f1-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="165f1-105">Versions du logiciel utilisées dans cette rubrique</span><span class="sxs-lookup"><span data-stu-id="165f1-105">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="165f1-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="165f1-106">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="165f1-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="165f1-107">.NET 4.5</span></span>
> - <span data-ttu-id="165f1-108">SignalR version 2</span><span class="sxs-lookup"><span data-stu-id="165f1-108">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="165f1-109">Versions précédentes de cette rubrique</span><span class="sxs-lookup"><span data-stu-id="165f1-109">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="165f1-110">Pour plus d’informations sur les versions antérieures de SignalR, consultez [SignalR des Versions antérieures](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="165f1-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="165f1-111">Questions et des commentaires</span><span class="sxs-lookup"><span data-stu-id="165f1-111">Questions and comments</span></span>
> 
> <span data-ttu-id="165f1-112">Veuillez laisser des commentaires sur la façon dont vous avez aimé ce didacticiel et nous pouvons améliorer dans les commentaires en bas de la page.</span><span class="sxs-lookup"><span data-stu-id="165f1-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="165f1-113">Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les valider pour le [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="165f1-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="165f1-114">Injection de dépendance est un moyen pour supprimer des dépendances codées en dur entre les objets, facilitant ainsi la pour remplacer des dépendances d’un objet, soit pour le test (à l’aide d’objets fictifs) ou pour modifier le comportement d’exécution.</span><span class="sxs-lookup"><span data-stu-id="165f1-114">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="165f1-115">Ce didacticiel montre comment effectuer l’injection de dépendances sur les concentrateurs SignalR.</span><span class="sxs-lookup"><span data-stu-id="165f1-115">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="165f1-116">Il montre également comment utiliser des conteneurs d’inversion de contrôle avec SignalR.</span><span class="sxs-lookup"><span data-stu-id="165f1-116">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="165f1-117">Un conteneur inversion de contrôle est une infrastructure générale pour l’injection de dépendance.</span><span class="sxs-lookup"><span data-stu-id="165f1-117">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="165f1-118">Nouveautés d’Injection de dépendance ?</span><span class="sxs-lookup"><span data-stu-id="165f1-118">What is Dependency Injection?</span></span>

<span data-ttu-id="165f1-119">Ignorez cette section si vous êtes déjà familiarisé avec l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="165f1-119">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="165f1-120">*Injection de dépendance* (DI) est un modèle où les objets ne sont pas chargées de créer leurs propres dépendances.</span><span class="sxs-lookup"><span data-stu-id="165f1-120">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="165f1-121">Voici un exemple simple motiver DI.</span><span class="sxs-lookup"><span data-stu-id="165f1-121">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="165f1-122">Supposons que vous avez un objet qui a besoin d’enregistrer des messages.</span><span class="sxs-lookup"><span data-stu-id="165f1-122">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="165f1-123">Vous pouvez définir une interface de journalisation :</span><span class="sxs-lookup"><span data-stu-id="165f1-123">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="165f1-124">Dans votre objet, vous pouvez créer un `ILogger` pour enregistrer des messages :</span><span class="sxs-lookup"><span data-stu-id="165f1-124">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="165f1-125">Cela fonctionne, mais il n’est pas la meilleure conception.</span><span class="sxs-lookup"><span data-stu-id="165f1-125">This works, but it's not the best design.</span></span> <span data-ttu-id="165f1-126">Si vous souhaitez remplacer `FileLogger` avec un autre `ILogger` mise en œuvre, vous devez modifier `SomeComponent`.</span><span class="sxs-lookup"><span data-stu-id="165f1-126">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="165f1-127">Supposer que beaucoup d’autres objets utilisent `FileLogger`, vous devez modifier tous les.</span><span class="sxs-lookup"><span data-stu-id="165f1-127">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="165f1-128">Ou si vous décidez d’apporter `FileLogger` un singleton, vous devez également apporter des modifications à l’application.</span><span class="sxs-lookup"><span data-stu-id="165f1-128">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="165f1-129">Une meilleure approche consiste à « injecter » un `ILogger` dans l’objet, par exemple, en utilisant un argument de constructeur :</span><span class="sxs-lookup"><span data-stu-id="165f1-129">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="165f1-130">À présent l’objet n’est pas responsable de la sélection qui `ILogger` à utiliser.</span><span class="sxs-lookup"><span data-stu-id="165f1-130">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="165f1-131">Vous pouvez swich `ILogger` implémentations sans modifier les objets qui en dépendent.</span><span class="sxs-lookup"><span data-stu-id="165f1-131">You can swich `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="165f1-132">Ce modèle est appelé [injection de constructeur](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="165f1-132">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="165f1-133">Un autre modèle est l’injection de setter, où vous définissez la dépendance via une méthode setter ou une propriété.</span><span class="sxs-lookup"><span data-stu-id="165f1-133">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="165f1-134">Injection de dépendances simple dans SignalR</span><span class="sxs-lookup"><span data-stu-id="165f1-134">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="165f1-135">Envisagez de l’application de conversation à partir du didacticiel [mise en route avec SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="165f1-135">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="165f1-136">Voici la classe de concentrateur à partir de cette application :</span><span class="sxs-lookup"><span data-stu-id="165f1-136">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="165f1-137">Supposons que vous souhaitez stocker les messages de conversation sur le serveur avant de les envoyer.</span><span class="sxs-lookup"><span data-stu-id="165f1-137">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="165f1-138">Vous pouvez définir une interface qui s’approprie cette fonctionnalité et utiliser DI injecter l’interface dans le `ChatHub` classe.</span><span class="sxs-lookup"><span data-stu-id="165f1-138">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="165f1-139">Le seul problème est qu’une application SignalR ne crée pas directement les concentrateurs ; SignalR crée pour vous.</span><span class="sxs-lookup"><span data-stu-id="165f1-139">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="165f1-140">Par défaut, SignalR attend une classe de concentrateur à avoir un constructeur sans paramètre.</span><span class="sxs-lookup"><span data-stu-id="165f1-140">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="165f1-141">Toutefois, vous pouvez facilement inscrire une fonction pour créer des instances de concentrateur et utiliser cette fonction pour effectuer DI.</span><span class="sxs-lookup"><span data-stu-id="165f1-141">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="165f1-142">Inscription de la fonction en appelant **GlobalHost.DependencyResolver.Register**.</span><span class="sxs-lookup"><span data-stu-id="165f1-142">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="165f1-143">Maintenant SignalR appelle cette fonction anonyme chaque fois qu’il a besoin créer un `ChatHub` instance.</span><span class="sxs-lookup"><span data-stu-id="165f1-143">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="165f1-144">Conteneurs d’inversion de contrôle</span><span class="sxs-lookup"><span data-stu-id="165f1-144">IoC Containers</span></span>

<span data-ttu-id="165f1-145">Le code précédent est tout indiquée pour les cas simples.</span><span class="sxs-lookup"><span data-stu-id="165f1-145">The previous code is fine for simple cases.</span></span> <span data-ttu-id="165f1-146">C’est toujours écrire ceci :</span><span class="sxs-lookup"><span data-stu-id="165f1-146">But you still had to write this:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

<span data-ttu-id="165f1-147">Dans une application complexe avec de nombreuses dépendances, vous devrez écrire un lot de ce code « connexion ».</span><span class="sxs-lookup"><span data-stu-id="165f1-147">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="165f1-148">Ce code peut être difficile à maintenir, particulièrement si les dépendances sont imbriquées.</span><span class="sxs-lookup"><span data-stu-id="165f1-148">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="165f1-149">Il est également difficile de test unitaire.</span><span class="sxs-lookup"><span data-stu-id="165f1-149">It is also hard to unit test.</span></span>

<span data-ttu-id="165f1-150">Une solution consiste à utiliser un conteneur inversion de contrôle.</span><span class="sxs-lookup"><span data-stu-id="165f1-150">One solution is to use an IoC container.</span></span> <span data-ttu-id="165f1-151">Un conteneur inversion de contrôle est un composant logiciel qui est chargé de gérer les dépendances. Vous inscrivez les types avec le conteneur et ensuite utilisez le conteneur pour créer des objets.</span><span class="sxs-lookup"><span data-stu-id="165f1-151">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="165f1-152">Le conteneur effectue automatiquement les relations de dépendance.</span><span class="sxs-lookup"><span data-stu-id="165f1-152">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="165f1-153">De nombreux conteneurs d’inversion de contrôle vous permettent également de contrôler les éléments tels que la durée de vie et la portée.</span><span class="sxs-lookup"><span data-stu-id="165f1-153">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="165f1-154">« IoC » signifie « d’inversion de contrôle », qui est un modèle général où une infrastructure appelle du code d’application.</span><span class="sxs-lookup"><span data-stu-id="165f1-154">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="165f1-155">Un conteneur IoC construit vos objets, lequel « inverse » le flux habituel de contrôle.</span><span class="sxs-lookup"><span data-stu-id="165f1-155">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="165f1-156">À l’aide de conteneurs d’inversion de contrôle dans SignalR</span><span class="sxs-lookup"><span data-stu-id="165f1-156">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="165f1-157">L’application de la conversation est probablement trop simple pour tirer parti d’un conteneur inversion de contrôle.</span><span class="sxs-lookup"><span data-stu-id="165f1-157">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="165f1-158">Au lieu de cela, examinons la [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) exemple.</span><span class="sxs-lookup"><span data-stu-id="165f1-158">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="165f1-159">L’exemple StockTicker définit deux classes principales :</span><span class="sxs-lookup"><span data-stu-id="165f1-159">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="165f1-160">`StockTickerHub`: La classe de concentrateur, qui gère les connexions clientes.</span><span class="sxs-lookup"><span data-stu-id="165f1-160">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="165f1-161">`StockTicker`: Un singleton qui conserve des actions et les met à jour régulièrement.</span><span class="sxs-lookup"><span data-stu-id="165f1-161">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="165f1-162">`StockTickerHub`contient une référence à la `StockTicker` singleton, tandis que `StockTicker` contient une référence à la **IHubConnectionContext** pour le `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="165f1-162">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="165f1-163">Il utilise cette interface pour communiquer avec `StockTickerHub` instances.</span><span class="sxs-lookup"><span data-stu-id="165f1-163">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="165f1-164">(Pour plus d’informations, consultez [serveur de diffusion avec ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span><span class="sxs-lookup"><span data-stu-id="165f1-164">(For more information, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span></span>

<span data-ttu-id="165f1-165">Nous pouvons utiliser un conteneur inversion de contrôle à ces dépendances un peu.</span><span class="sxs-lookup"><span data-stu-id="165f1-165">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="165f1-166">Tout d’abord, nous allons simplifier le `StockTickerHub` et `StockTicker` classes.</span><span class="sxs-lookup"><span data-stu-id="165f1-166">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="165f1-167">Dans le code suivant, j’ai mis en commentaire les parties que nous n’avez pas besoin.</span><span class="sxs-lookup"><span data-stu-id="165f1-167">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="165f1-168">Supprimez le constructeur sans paramètre à partir de `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="165f1-168">Remove the parameterless constructor from `StockTickerHub`.</span></span> <span data-ttu-id="165f1-169">Au lieu de cela, nous allons utiliser toujours DI pour créer le concentrateur.</span><span class="sxs-lookup"><span data-stu-id="165f1-169">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="165f1-170">Pour StockTicker, supprimez l’instance de singleton.</span><span class="sxs-lookup"><span data-stu-id="165f1-170">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="165f1-171">Une version ultérieure, nous allons utiliser le conteneur inversion de contrôle pour contrôler la durée de vie StockTicker.</span><span class="sxs-lookup"><span data-stu-id="165f1-171">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="165f1-172">En outre, rendez le constructeur public.</span><span class="sxs-lookup"><span data-stu-id="165f1-172">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="165f1-173">Ensuite, nous pouvons refactoriser le code en créant une interface pour `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="165f1-173">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="165f1-174">Nous allons utiliser cette interface pour découpler le `StockTickerHub` à partir de la `StockTicker` classe.</span><span class="sxs-lookup"><span data-stu-id="165f1-174">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="165f1-175">Visual Studio apporte facilement ce type de refactorisation.</span><span class="sxs-lookup"><span data-stu-id="165f1-175">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="165f1-176">Ouvrez le fichier StockTicker.cs, avec le bouton droit sur le `StockTicker` déclaration de classe, puis sélectionnez **refactoriser** ... **Extraire l’Interface**.</span><span class="sxs-lookup"><span data-stu-id="165f1-176">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="165f1-177">Dans le **extraire l’Interface** boîte de dialogue, cliquez sur **sélectionner tout**.</span><span class="sxs-lookup"><span data-stu-id="165f1-177">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="165f1-178">Laissez les valeurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="165f1-178">Leave the other defaults.</span></span> <span data-ttu-id="165f1-179">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="165f1-179">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="165f1-180">Visual Studio crée une nouvelle interface nommée `IStockTicker`et modifie également `StockTicker` dériver `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="165f1-180">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="165f1-181">Ouvrez le fichier IStockTicker.cs et de modifier l’interface à **public**.</span><span class="sxs-lookup"><span data-stu-id="165f1-181">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="165f1-182">Dans le `StockTickerHub` de classe, modifiez les deux instances de `StockTicker` à `IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="165f1-182">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="165f1-183">Création d’un `IStockTicker` interface n’est pas strictement nécessaire, mais je souhaitais montrer comment les DI peut aider à réduire le couplage entre les composants de votre application.</span><span class="sxs-lookup"><span data-stu-id="165f1-183">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="165f1-184">Ajoutez la bibliothèque Ninject</span><span class="sxs-lookup"><span data-stu-id="165f1-184">Add the Ninject Library</span></span>

<span data-ttu-id="165f1-185">Il existe de nombreux conteneurs d’inversion de contrôle open source pour .NET.</span><span class="sxs-lookup"><span data-stu-id="165f1-185">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="165f1-186">Pour ce didacticiel, je vais utiliser [Ninject](http://www.ninject.org/).</span><span class="sxs-lookup"><span data-stu-id="165f1-186">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="165f1-187">(Incluent d’autres bibliothèques populaires [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), et [StructureMap ](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="165f1-187">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="165f1-188">Utilisez le Gestionnaire de Package NuGet pour installer le [Ninject bibliothèque](https://nuget.org/packages/Ninject/3.0.1.10).</span><span class="sxs-lookup"><span data-stu-id="165f1-188">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="165f1-189">Dans Visual Studio, à partir de la **outils** menu Sélectionnez **Gestionnaire de Package de bibliothèque** | **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="165f1-189">In Visual Studio, from the **Tools** menu select **Library Package Manager** | **Package Manager Console**.</span></span> <span data-ttu-id="165f1-190">Dans la fenêtre de Console du Gestionnaire de Package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="165f1-190">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="165f1-191">Remplacez le résolveur de dépendance SignalR</span><span class="sxs-lookup"><span data-stu-id="165f1-191">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="165f1-192">Pour utiliser Ninject dans SignalR, créez une classe qui dérive de **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="165f1-192">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="165f1-193">Cette classe substitue le **GetService** et **GetServices** méthodes de **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="165f1-193">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="165f1-194">SignalR appelle ces méthodes pour créer divers objets lors de l’exécution, y compris les instances de concentrateur, ainsi que divers services utilisés en interne par SignalR.</span><span class="sxs-lookup"><span data-stu-id="165f1-194">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="165f1-195">Le **GetService** méthode crée une instance unique d’un type.</span><span class="sxs-lookup"><span data-stu-id="165f1-195">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="165f1-196">Substituez cette méthode pour appeler le noyau de Ninject **TryGet** (méthode).</span><span class="sxs-lookup"><span data-stu-id="165f1-196">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="165f1-197">Si cette méthode retourne la valeur null, revenir au programme de résolution par défaut.</span><span class="sxs-lookup"><span data-stu-id="165f1-197">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="165f1-198">Le **GetServices** méthode crée une collection d’objets d’un type spécifié.</span><span class="sxs-lookup"><span data-stu-id="165f1-198">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="165f1-199">Substituez cette méthode pour concaténer les résultats de Ninject avec les résultats à partir du programme de résolution par défaut.</span><span class="sxs-lookup"><span data-stu-id="165f1-199">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="165f1-200">Configurer les liaisons de Ninject</span><span class="sxs-lookup"><span data-stu-id="165f1-200">Configure Ninject Bindings</span></span>

<span data-ttu-id="165f1-201">Maintenant, nous allons utiliser Ninject pour déclarer des liaisons de type.</span><span class="sxs-lookup"><span data-stu-id="165f1-201">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="165f1-202">Ouvrez votre classe de l’application Startup.cs (que vous avez soit créé manuellement selon les instructions de package de `readme.txt`, ou qui a été créé en ajoutant l’authentification à votre projet).</span><span class="sxs-lookup"><span data-stu-id="165f1-202">Open your application's Startup.cs class (that you either created manually as per the package instructions in `readme.txt`, or that was created by adding authentication to your project).</span></span> <span data-ttu-id="165f1-203">Dans le `Startup.Configuration` (méthode), créez le conteneur de Ninject Ninject appelle la *noyau*.</span><span class="sxs-lookup"><span data-stu-id="165f1-203">In the `Startup.Configuration` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="165f1-204">Créez une instance de notre programme de résolution de dépendance personnalisée :</span><span class="sxs-lookup"><span data-stu-id="165f1-204">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="165f1-205">Créer une liaison pour `IStockTicker` comme suit :</span><span class="sxs-lookup"><span data-stu-id="165f1-205">Create a binding for `IStockTicker` as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

<span data-ttu-id="165f1-206">Ce code indique deux choses.</span><span class="sxs-lookup"><span data-stu-id="165f1-206">This code is saying two things.</span></span> <span data-ttu-id="165f1-207">Premier, chaque fois que l’application a besoin un `IStockTicker`, le noyau doit créer une instance de `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="165f1-207">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="165f1-208">Ensuite, la `StockTicker` classe doit être créé en tant qu’objet singleton.</span><span class="sxs-lookup"><span data-stu-id="165f1-208">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="165f1-209">Ninject crée une instance de l’objet et retournent la même instance pour chaque demande.</span><span class="sxs-lookup"><span data-stu-id="165f1-209">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="165f1-210">Créer une liaison pour **IHubConnectionContext** comme suit :</span><span class="sxs-lookup"><span data-stu-id="165f1-210">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="165f1-211">Ce code de creatres une fonction anonyme qui retourne un **IHubConnection**.</span><span class="sxs-lookup"><span data-stu-id="165f1-211">This code creatres an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="165f1-212">Le **WhenInjectedInto** méthode indique Ninject pour utiliser cette fonction uniquement lors de la création `IStockTicker` instances.</span><span class="sxs-lookup"><span data-stu-id="165f1-212">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="165f1-213">La raison est que SignalR crée **IHubConnectionContext** instances en interne, et nous ne souhaitez pas remplacer la SignalR les crée.</span><span class="sxs-lookup"><span data-stu-id="165f1-213">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="165f1-214">Cette fonction s’applique uniquement à notre `StockTicker` classe.</span><span class="sxs-lookup"><span data-stu-id="165f1-214">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="165f1-215">Passez le résolveur de dépendance dans le **MapSignalR** méthode en ajoutant une configuration de concentrateur :</span><span class="sxs-lookup"><span data-stu-id="165f1-215">Pass the dependency resolver into the **MapSignalR** method by adding a hub configuration:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="165f1-216">Mettre à jour de la méthode Startup.ConfigureSignalR dans la classe de démarrage de l’exemple avec le nouveau paramètre :</span><span class="sxs-lookup"><span data-stu-id="165f1-216">Update the Startup.ConfigureSignalR method in the sample's Startup class with the new parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="165f1-217">Présent SignalR utilise le programme de résolution spécifié dans **MapSignalR**, au lieu du résolveur par défaut.</span><span class="sxs-lookup"><span data-stu-id="165f1-217">Now SignalR will use the resolver specified in **MapSignalR**, instead of the default resolver.</span></span>

<span data-ttu-id="165f1-218">Voici le code complet pour `Startup.Configuration`.</span><span class="sxs-lookup"><span data-stu-id="165f1-218">Here is the complete code listing for `Startup.Configuration`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

<span data-ttu-id="165f1-219">Pour exécuter l’application StockTicker dans Visual Studio, appuyez sur F5.</span><span class="sxs-lookup"><span data-stu-id="165f1-219">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="165f1-220">Dans la fenêtre du navigateur, accédez à `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span><span class="sxs-lookup"><span data-stu-id="165f1-220">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="165f1-221">L’application a exactement les mêmes fonctionnalités qu’avant.</span><span class="sxs-lookup"><span data-stu-id="165f1-221">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="165f1-222">(Pour obtenir une description, consultez [serveur de diffusion avec ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) Nous n’avons pas de modifier le comportement ; simplement rend le code plus facile à tester, gérer et faire évoluer.</span><span class="sxs-lookup"><span data-stu-id="165f1-222">(For a description, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
