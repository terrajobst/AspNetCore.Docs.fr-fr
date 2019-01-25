---
uid: signalr/overview/advanced/dependency-injection
title: L’Injection de dépendances dans SignalR | Microsoft Docs
author: bradygaster
description: Versions des logiciels utilisés dans cette rubrique Visual Studio 2013, .NET 4.5 SignalR les versions précédentes de la version 2 de cette rubrique pour plus d’informations sur les versions antérieures de...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 54e263e277852d2d478ce5bccd4164254498831a
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836608"
---
<a name="dependency-injection-in-signalr"></a><span data-ttu-id="f9048-103">Injection de dépendances dans SignalR</span><span class="sxs-lookup"><span data-stu-id="f9048-103">Dependency Injection in SignalR</span></span>
====================
<span data-ttu-id="f9048-104">par [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="f9048-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="f9048-105">Versions des logiciels utilisées dans cette rubrique</span><span class="sxs-lookup"><span data-stu-id="f9048-105">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="f9048-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="f9048-106">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="f9048-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="f9048-107">.NET 4.5</span></span>
> - <span data-ttu-id="f9048-108">SignalR version 2</span><span class="sxs-lookup"><span data-stu-id="f9048-108">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="f9048-109">Versions précédentes de cette rubrique</span><span class="sxs-lookup"><span data-stu-id="f9048-109">Previous versions of this topic</span></span>
>
> <span data-ttu-id="f9048-110">Pour plus d’informations sur les versions antérieures de SignalR, consultez [les Versions antérieures de SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="f9048-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="f9048-111">Questions et commentaires</span><span class="sxs-lookup"><span data-stu-id="f9048-111">Questions and comments</span></span>
>
> <span data-ttu-id="f9048-112">Veuillez laisser des commentaires sur la façon dont vous avez apprécié ce didacticiel et ce que nous pouvions améliorer dans les commentaires en bas de la page.</span><span class="sxs-lookup"><span data-stu-id="f9048-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="f9048-113">Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les publier à le [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="f9048-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="f9048-114">L’injection de dépendances consiste à supprimer codées en dur les dépendances entre les objets, ce qui facilite pour remplacer les dépendances d’un objet, soit pour le test (à l’aide d’objets fictifs) ou pour modifier le comportement au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="f9048-114">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="f9048-115">Ce didacticiel montre comment effectuer l’injection de dépendances sur les concentrateurs SignalR.</span><span class="sxs-lookup"><span data-stu-id="f9048-115">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="f9048-116">Il montre également comment utiliser des conteneurs IoC avec SignalR.</span><span class="sxs-lookup"><span data-stu-id="f9048-116">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="f9048-117">Un conteneur IoC est une infrastructure générale pour l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="f9048-117">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="f9048-118">Quelle est l’Injection de dépendances ?</span><span class="sxs-lookup"><span data-stu-id="f9048-118">What is Dependency Injection?</span></span>

<span data-ttu-id="f9048-119">Ignorer cette section si vous êtes déjà familiarisé avec l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="f9048-119">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="f9048-120">*L’injection de dépendances* (DI) est un modèle où les objets ne sont pas chargés de créer leurs propres dépendances.</span><span class="sxs-lookup"><span data-stu-id="f9048-120">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="f9048-121">Voici un exemple simple pour motiver l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="f9048-121">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="f9048-122">Supposons que vous avez un objet dont a besoin d’enregistrer des messages.</span><span class="sxs-lookup"><span data-stu-id="f9048-122">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="f9048-123">Vous pouvez définir une interface de journalisation :</span><span class="sxs-lookup"><span data-stu-id="f9048-123">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="f9048-124">Dans votre objet, vous pouvez créer un `ILogger` pour enregistrer des messages :</span><span class="sxs-lookup"><span data-stu-id="f9048-124">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="f9048-125">Cela fonctionne, mais il n’est pas la meilleure conception.</span><span class="sxs-lookup"><span data-stu-id="f9048-125">This works, but it's not the best design.</span></span> <span data-ttu-id="f9048-126">Si vous souhaitez remplacer `FileLogger` avec un autre `ILogger` implémentation, vous devrez modifier `SomeComponent`.</span><span class="sxs-lookup"><span data-stu-id="f9048-126">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="f9048-127">Laisse supposer que beaucoup d’autres objets utilisent `FileLogger`, vous devez modifier l’ensemble d'entre eux.</span><span class="sxs-lookup"><span data-stu-id="f9048-127">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="f9048-128">Ou si vous décidez d’apporter `FileLogger` un singleton, vous devez également apporter des modifications tout au long de l’application.</span><span class="sxs-lookup"><span data-stu-id="f9048-128">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="f9048-129">Une meilleure approche consiste à « injecter » un `ILogger` dans l’objet, par exemple, en utilisant un argument de constructeur :</span><span class="sxs-lookup"><span data-stu-id="f9048-129">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="f9048-130">L’objet n’est pas responsable de la sélection qui `ILogger` à utiliser.</span><span class="sxs-lookup"><span data-stu-id="f9048-130">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="f9048-131">Vous pouvez swich `ILogger` implémentations sans modifier les objets qui en dépendent.</span><span class="sxs-lookup"><span data-stu-id="f9048-131">You can swich `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="f9048-132">Ce modèle est appelé [l’injection de constructeur](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="f9048-132">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="f9048-133">Un autre modèle est l’injection de setter, où vous définissez la dépendance par une méthode setter ou une propriété.</span><span class="sxs-lookup"><span data-stu-id="f9048-133">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="f9048-134">Injection de dépendances simple dans SignalR</span><span class="sxs-lookup"><span data-stu-id="f9048-134">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="f9048-135">Envisagez de l’application de conversation à partir du didacticiel [bien démarrer avec SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="f9048-135">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="f9048-136">Voici la classe hub à partir de cette application :</span><span class="sxs-lookup"><span data-stu-id="f9048-136">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="f9048-137">Supposons que vous souhaitez stocker les messages de conversation sur le serveur avant de les envoyer.</span><span class="sxs-lookup"><span data-stu-id="f9048-137">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="f9048-138">Vous définissez une interface qui isole cette fonctionnalité et utilisez l’injection de dépendances pour injecter l’interface dans le `ChatHub` classe.</span><span class="sxs-lookup"><span data-stu-id="f9048-138">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="f9048-139">Le seul problème est qu’une application SignalR ne crée pas directement les hubs ; SignalR crée pour vous.</span><span class="sxs-lookup"><span data-stu-id="f9048-139">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="f9048-140">Par défaut, SignalR attend une classe de concentrateur pour avoir un constructeur sans paramètre.</span><span class="sxs-lookup"><span data-stu-id="f9048-140">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="f9048-141">Toutefois, vous pouvez facilement enregistrer une fonction pour créer des instances de concentrateur et utiliser cette fonction pour effectuer l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="f9048-141">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="f9048-142">Enregistrez la fonction en appelant **GlobalHost.DependencyResolver.Register**.</span><span class="sxs-lookup"><span data-stu-id="f9048-142">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="f9048-143">Maintenant SignalR appelle cette fonction anonyme chaque fois qu’il a besoin créer un `ChatHub` instance.</span><span class="sxs-lookup"><span data-stu-id="f9048-143">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="f9048-144">Conteneurs IoC</span><span class="sxs-lookup"><span data-stu-id="f9048-144">IoC Containers</span></span>

<span data-ttu-id="f9048-145">Le code précédent est parfait pour les cas simples.</span><span class="sxs-lookup"><span data-stu-id="f9048-145">The previous code is fine for simple cases.</span></span> <span data-ttu-id="f9048-146">Mais vous deviez toujours écrire ceci :</span><span class="sxs-lookup"><span data-stu-id="f9048-146">But you still had to write this:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

<span data-ttu-id="f9048-147">Dans une application complexe avec de nombreuses dépendances, vous devrez peut-être écrire beaucoup de code de cette « connexion ».</span><span class="sxs-lookup"><span data-stu-id="f9048-147">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="f9048-148">Ce code peut être difficile à maintenir, surtout si les dépendances sont imbriqués.</span><span class="sxs-lookup"><span data-stu-id="f9048-148">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="f9048-149">Il est également difficile de test unitaire.</span><span class="sxs-lookup"><span data-stu-id="f9048-149">It is also hard to unit test.</span></span>

<span data-ttu-id="f9048-150">Une solution consiste à utiliser un conteneur IoC.</span><span class="sxs-lookup"><span data-stu-id="f9048-150">One solution is to use an IoC container.</span></span> <span data-ttu-id="f9048-151">Un conteneur IoC est un composant logiciel qui est responsable de la gestion des dépendances. Vous inscrivez des types auprès du conteneur et ensuite utilisez le conteneur pour créer des objets.</span><span class="sxs-lookup"><span data-stu-id="f9048-151">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="f9048-152">Le conteneur détermine automatiquement les relations de dépendance.</span><span class="sxs-lookup"><span data-stu-id="f9048-152">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="f9048-153">De nombreux conteneurs d’inversion de contrôle vous permettent également de vous permettent de contrôler des éléments tels que la durée de vie et la portée.</span><span class="sxs-lookup"><span data-stu-id="f9048-153">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="f9048-154">« IoC » est l’acronyme « d’inversion de contrôle », qui est un modèle général où un framework appelle du code d’application.</span><span class="sxs-lookup"><span data-stu-id="f9048-154">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="f9048-155">Un conteneur IoC construit vos objets, ce qui le flux habituel de contrôle « inverse ».</span><span class="sxs-lookup"><span data-stu-id="f9048-155">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="f9048-156">À l’aide de conteneurs IoC dans SignalR</span><span class="sxs-lookup"><span data-stu-id="f9048-156">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="f9048-157">L’application de conversation est probablement trop simple pour bénéficier d’un conteneur IoC.</span><span class="sxs-lookup"><span data-stu-id="f9048-157">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="f9048-158">Au lieu de cela, nous allons examiner la [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) exemple.</span><span class="sxs-lookup"><span data-stu-id="f9048-158">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="f9048-159">L’exemple StockTicker définit deux classes principales :</span><span class="sxs-lookup"><span data-stu-id="f9048-159">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="f9048-160">`StockTickerHub`: La classe de concentrateur, qui gère les connexions clientes.</span><span class="sxs-lookup"><span data-stu-id="f9048-160">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="f9048-161">`StockTicker`: Un singleton qui conserve des actions et les met à jour régulièrement.</span><span class="sxs-lookup"><span data-stu-id="f9048-161">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="f9048-162">`StockTickerHub` contient une référence à la `StockTicker` singleton, tandis que `StockTicker` contient une référence à la **IHubConnectionContext** pour le `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="f9048-162">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="f9048-163">Il utilise cette interface pour communiquer avec `StockTickerHub` instances.</span><span class="sxs-lookup"><span data-stu-id="f9048-163">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="f9048-164">(Pour plus d’informations, consultez [diffusion par le serveur avec ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span><span class="sxs-lookup"><span data-stu-id="f9048-164">(For more information, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span></span>

<span data-ttu-id="f9048-165">Nous pouvons utiliser un conteneur IoC pour démêler un peu de ces dépendances.</span><span class="sxs-lookup"><span data-stu-id="f9048-165">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="f9048-166">Tout d’abord, nous allons simplifier le `StockTickerHub` et `StockTicker` classes.</span><span class="sxs-lookup"><span data-stu-id="f9048-166">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="f9048-167">Dans le code suivant, j’ai commenté les parties que nous n’avez pas besoin.</span><span class="sxs-lookup"><span data-stu-id="f9048-167">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="f9048-168">Supprimez le constructeur sans paramètre à partir de `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="f9048-168">Remove the parameterless constructor from `StockTickerHub`.</span></span> <span data-ttu-id="f9048-169">Au lieu de cela, nous allons toujours utiliser l’injection de dépendances pour créer le hub.</span><span class="sxs-lookup"><span data-stu-id="f9048-169">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="f9048-170">Pour StockTicker, supprimez l’instance de singleton.</span><span class="sxs-lookup"><span data-stu-id="f9048-170">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="f9048-171">Plus tard, nous allons utiliser le conteneur IoC pour contrôler la durée de vie StockTicker.</span><span class="sxs-lookup"><span data-stu-id="f9048-171">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="f9048-172">En outre, rendre le constructeur public.</span><span class="sxs-lookup"><span data-stu-id="f9048-172">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="f9048-173">Ensuite, nous pouvons refactoriser le code en créant une interface pour `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="f9048-173">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="f9048-174">Nous allons utiliser cette interface pour découpler le `StockTickerHub` à partir de la `StockTicker` classe.</span><span class="sxs-lookup"><span data-stu-id="f9048-174">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="f9048-175">Visual Studio rend facile ce type de refactorisation.</span><span class="sxs-lookup"><span data-stu-id="f9048-175">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="f9048-176">Ouvrez le fichier StockTicker.cs, avec le bouton droit sur le `StockTicker` déclaration de classe, puis sélectionnez **refactoriser** ... **Extraire l’Interface**.</span><span class="sxs-lookup"><span data-stu-id="f9048-176">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="f9048-177">Dans le **extraire l’Interface** boîte de dialogue, cliquez sur **sélectionner tout**.</span><span class="sxs-lookup"><span data-stu-id="f9048-177">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="f9048-178">Laissez les autres valeurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="f9048-178">Leave the other defaults.</span></span> <span data-ttu-id="f9048-179">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="f9048-179">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="f9048-180">Visual Studio crée une nouvelle interface nommée `IStockTicker`et change également `StockTicker` de dériver de `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="f9048-180">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="f9048-181">Ouvrez le fichier IStockTicker.cs et modifiez l’interface à **public**.</span><span class="sxs-lookup"><span data-stu-id="f9048-181">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="f9048-182">Dans le `StockTickerHub` class, modifiez les deux instances de `StockTicker` à `IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="f9048-182">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="f9048-183">Création d’un `IStockTicker` interface n’est pas strictement nécessaire, mais j’ai voulu montrer comment l’injection de dépendances peut aider à réduire le couplage entre les composants dans votre application.</span><span class="sxs-lookup"><span data-stu-id="f9048-183">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="f9048-184">Ajoutez la bibliothèque Ninject</span><span class="sxs-lookup"><span data-stu-id="f9048-184">Add the Ninject Library</span></span>

<span data-ttu-id="f9048-185">Il existe de nombreux conteneurs d’inversion de contrôle open source pour .NET.</span><span class="sxs-lookup"><span data-stu-id="f9048-185">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="f9048-186">Pour ce didacticiel, j’utiliserai [Ninject](http://www.ninject.org/).</span><span class="sxs-lookup"><span data-stu-id="f9048-186">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="f9048-187">(Incluent d’autres bibliothèques populaires [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), et [StructureMap ](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="f9048-187">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="f9048-188">Utilisez le Gestionnaire de Package NuGet pour installer le [Ninject bibliothèque](https://nuget.org/packages/Ninject/3.0.1.10).</span><span class="sxs-lookup"><span data-stu-id="f9048-188">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="f9048-189">Dans Visual Studio, à partir de la **outils** menu, sélectionnez **Gestionnaire de Package NuGet** > **Console du Gestionnaire de Package**.</span><span class="sxs-lookup"><span data-stu-id="f9048-189">In Visual Studio, from the **Tools** menu select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="f9048-190">Dans la fenêtre de Console du Gestionnaire de Package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="f9048-190">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="f9048-191">Remplacez le résolveur de dépendance de SignalR</span><span class="sxs-lookup"><span data-stu-id="f9048-191">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="f9048-192">Pour utiliser Ninject dans SignalR, créez une classe qui dérive de **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="f9048-192">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="f9048-193">Cette classe substitue la **GetService** et **GetServices** méthodes de **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="f9048-193">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="f9048-194">SignalR appelle ces méthodes pour créer les divers objets lors de l’exécution, y compris les instances du hub, ainsi que différents services utilisés en interne par SignalR.</span><span class="sxs-lookup"><span data-stu-id="f9048-194">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="f9048-195">Le **GetService** méthode crée une instance unique d’un type.</span><span class="sxs-lookup"><span data-stu-id="f9048-195">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="f9048-196">Substituez cette méthode pour appeler le noyau Ninject **TryGet** (méthode).</span><span class="sxs-lookup"><span data-stu-id="f9048-196">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="f9048-197">Si cette méthode retourne la valeur null, revenir au programme de résolution par défaut.</span><span class="sxs-lookup"><span data-stu-id="f9048-197">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="f9048-198">Le **GetServices** méthode crée une collection d’objets d’un type spécifié.</span><span class="sxs-lookup"><span data-stu-id="f9048-198">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="f9048-199">Substituez cette méthode pour concaténer les résultats à partir de Ninject avec les résultats à partir du programme de résolution par défaut.</span><span class="sxs-lookup"><span data-stu-id="f9048-199">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="f9048-200">Configurer les liaisons de Ninject</span><span class="sxs-lookup"><span data-stu-id="f9048-200">Configure Ninject Bindings</span></span>

<span data-ttu-id="f9048-201">Maintenant, nous allons utiliser Ninject pour déclarer des liaisons de type.</span><span class="sxs-lookup"><span data-stu-id="f9048-201">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="f9048-202">Ouvrez votre classe de l’application Startup.cs (que vous avez soit créé manuellement en suivant les instructions de package dans `readme.txt`, ou qui a été créé en ajoutant l’authentification à votre projet).</span><span class="sxs-lookup"><span data-stu-id="f9048-202">Open your application's Startup.cs class (that you either created manually as per the package instructions in `readme.txt`, or that was created by adding authentication to your project).</span></span> <span data-ttu-id="f9048-203">Dans le `Startup.Configuration` (méthode), créer le conteneur Ninject, qui appelle Ninject le *noyau*.</span><span class="sxs-lookup"><span data-stu-id="f9048-203">In the `Startup.Configuration` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="f9048-204">Créez une instance de notre programme de résolution de dépendance personnalisée :</span><span class="sxs-lookup"><span data-stu-id="f9048-204">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="f9048-205">Créer une liaison pour `IStockTicker` comme suit :</span><span class="sxs-lookup"><span data-stu-id="f9048-205">Create a binding for `IStockTicker` as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

<span data-ttu-id="f9048-206">Ce code indique que deux choses.</span><span class="sxs-lookup"><span data-stu-id="f9048-206">This code is saying two things.</span></span> <span data-ttu-id="f9048-207">Tout d’abord, chaque fois que l’application a besoin un `IStockTicker`, le noyau doit créer une instance de `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="f9048-207">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="f9048-208">Ensuite, le `StockTicker` classe doit-elle être créés en tant qu’objet singleton.</span><span class="sxs-lookup"><span data-stu-id="f9048-208">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="f9048-209">Ninject crée une instance de l’objet et retournent la même instance pour chaque demande.</span><span class="sxs-lookup"><span data-stu-id="f9048-209">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="f9048-210">Créer une liaison pour **IHubConnectionContext** comme suit :</span><span class="sxs-lookup"><span data-stu-id="f9048-210">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="f9048-211">Ce code de creatres une fonction anonyme qui retourne un **IHubConnection**.</span><span class="sxs-lookup"><span data-stu-id="f9048-211">This code creatres an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="f9048-212">Le **WhenInjectedInto** méthode indique à Ninject à utiliser cette fonction uniquement lors de la création `IStockTicker` instances.</span><span class="sxs-lookup"><span data-stu-id="f9048-212">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="f9048-213">La raison est que SignalR crée **IHubConnectionContext** instances en interne, et nous ne souhaitons pas substituer comment SignalR les crée.</span><span class="sxs-lookup"><span data-stu-id="f9048-213">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="f9048-214">Cette fonction s’applique uniquement à notre `StockTicker` classe.</span><span class="sxs-lookup"><span data-stu-id="f9048-214">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="f9048-215">Passer le résolveur de dépendance dans le **MapSignalR** méthode en ajoutant une configuration de hub :</span><span class="sxs-lookup"><span data-stu-id="f9048-215">Pass the dependency resolver into the **MapSignalR** method by adding a hub configuration:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="f9048-216">Mettre à jour la méthode Startup.ConfigureSignalR dans la classe de démarrage de l’exemple avec le nouveau paramètre :</span><span class="sxs-lookup"><span data-stu-id="f9048-216">Update the Startup.ConfigureSignalR method in the sample's Startup class with the new parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="f9048-217">Maintenant SignalR utilisera l’outil de résolution spécifié dans **MapSignalR**, au lieu du résolveur par défaut.</span><span class="sxs-lookup"><span data-stu-id="f9048-217">Now SignalR will use the resolver specified in **MapSignalR**, instead of the default resolver.</span></span>

<span data-ttu-id="f9048-218">Voici le code complet pour `Startup.Configuration`.</span><span class="sxs-lookup"><span data-stu-id="f9048-218">Here is the complete code listing for `Startup.Configuration`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

<span data-ttu-id="f9048-219">Pour exécuter l’application StockTicker dans Visual Studio, appuyez sur F5.</span><span class="sxs-lookup"><span data-stu-id="f9048-219">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="f9048-220">Dans la fenêtre du navigateur, accédez à `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span><span class="sxs-lookup"><span data-stu-id="f9048-220">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="f9048-221">L’application a exactement les mêmes fonctionnalités qu’avant.</span><span class="sxs-lookup"><span data-stu-id="f9048-221">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="f9048-222">(Pour obtenir une description, consultez [diffusion par le serveur avec ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) Nous n’avons pas modifié le comportement ; Venez le code plus facile à tester, gérer et faire évoluer.</span><span class="sxs-lookup"><span data-stu-id="f9048-222">(For a description, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
