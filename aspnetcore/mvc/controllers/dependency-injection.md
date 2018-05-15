---
title: Injection de dépendances dans les contrôleurs dans ASP.NET Core
author: ardalis
description: Découvrez comment les contrôleurs ASP.NET Core MVC demandent explicitement leurs dépendances par le biais de leurs constructeurs avec l’injection de dépendances dans ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: c3e26d294d51dc7044158b05c1ac39015c494610
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="dependency-injection-into-controllers-in-aspnet-core"></a><span data-ttu-id="92d47-103">Injection de dépendances dans les contrôleurs dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="92d47-103">Dependency injection into controllers in ASP.NET Core</span></span>

<a name="dependency-injection-controllers"></a>

<span data-ttu-id="92d47-104">Par [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="92d47-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="92d47-105">Les contrôleurs ASP.NET Core MVC doivent demander leurs dépendances explicitement via leurs constructeurs.</span><span class="sxs-lookup"><span data-stu-id="92d47-105">ASP.NET Core MVC controllers should request their dependencies explicitly via their constructors.</span></span> <span data-ttu-id="92d47-106">Dans certains cas, les actions d’un contrôleur individuel peuvent nécessiter un service, et il peut ne pas être judicieux de faire les demandes au niveau du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="92d47-106">In some instances, individual controller actions may require a service, and it may not make sense to request at the controller level.</span></span> <span data-ttu-id="92d47-107">Dans ce cas, vous pouvez également choisir d’injecter un service comme paramètre sur la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="92d47-107">In this case, you can also choose to inject a service as a parameter on the action method.</span></span>

<span data-ttu-id="92d47-108">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="92d47-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="92d47-109">Injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="92d47-109">Dependency Injection</span></span>

<span data-ttu-id="92d47-110">L’injection de dépendances est une technique qui suit le [principe d’inversion de dépendance](http://deviq.com/dependency-inversion-principle/) et permet de composer des applications avec des modules faiblement couplés.</span><span class="sxs-lookup"><span data-stu-id="92d47-110">Dependency injection is a technique that follows the [Dependency Inversion Principle](http://deviq.com/dependency-inversion-principle/), allowing for applications to be composed of loosely coupled modules.</span></span> <span data-ttu-id="92d47-111">ASP.NET Core a une prise en charge intégrée de [l’injection de dépendances](../../fundamentals/dependency-injection.md), ce qui facilite le test et la gestion des applications.</span><span class="sxs-lookup"><span data-stu-id="92d47-111">ASP.NET Core has built-in support for [dependency injection](../../fundamentals/dependency-injection.md), which makes applications easier to test and maintain.</span></span>

## <a name="constructor-injection"></a><span data-ttu-id="92d47-112">Injection de constructeurs</span><span class="sxs-lookup"><span data-stu-id="92d47-112">Constructor Injection</span></span>

<span data-ttu-id="92d47-113">La prise en charge intégrée dans ASP.NET Core de l’injection de dépendances basée sur les constructeurs s’étend aux contrôleurs MVC.</span><span class="sxs-lookup"><span data-stu-id="92d47-113">ASP.NET Core's built-in support for constructor-based dependency injection extends to MVC controllers.</span></span> <span data-ttu-id="92d47-114">En ajoutant simplement un type de service à votre contrôleur en tant que paramètre de constructeur, ASP.NET Core tente de résoudre ce type en utilisant son conteneur de services intégrés.</span><span class="sxs-lookup"><span data-stu-id="92d47-114">By simply adding a service type to your controller as a constructor parameter, ASP.NET Core will attempt to resolve that type using its built in service container.</span></span> <span data-ttu-id="92d47-115">Les services sont, en général mais pas toujours, définis en utilisant des interfaces.</span><span class="sxs-lookup"><span data-stu-id="92d47-115">Services are typically, but not always, defined using interfaces.</span></span> <span data-ttu-id="92d47-116">Par exemple, si votre application a une logique métier qui dépend de l’heure, vous pouvez injecter un service qui récupère l’heure (au lieu de la coder en dur), ce qui permet à vos tests de réussir dans des implémentations qui utilisent une heure définie.</span><span class="sxs-lookup"><span data-stu-id="92d47-116">For example, if your application has business logic that depends on the current time, you can inject a service that retrieves the time (rather than hard-coding it), which would allow your tests to pass in implementations that use a set time.</span></span>

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


<span data-ttu-id="92d47-117">L’implémentation d’une interface comme celle-ci pour qu’elle utilise l’horloge système lors de l’exécution est simple :</span><span class="sxs-lookup"><span data-stu-id="92d47-117">Implementing an interface like this one so that it uses the system clock at runtime is trivial:</span></span>

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


<span data-ttu-id="92d47-118">Ceci étant en place, nous pouvons utiliser le service dans notre contrôleur.</span><span class="sxs-lookup"><span data-stu-id="92d47-118">With this in place, we can use the service in our controller.</span></span> <span data-ttu-id="92d47-119">Dans ce cas, nous avons ajouté de la logique pour la méthode `HomeController` `Index` permettant d’afficher un message d’accueil à l’utilisateur en fonction de l’heure du jour.</span><span class="sxs-lookup"><span data-stu-id="92d47-119">In this case, we have added some logic to the `HomeController` `Index` method to display a greeting to the user based on the time of day.</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

<span data-ttu-id="92d47-120">Si nous exécutons l’application maintenant, nous allons très probablement rencontrer une erreur :</span><span class="sxs-lookup"><span data-stu-id="92d47-120">If we run the application now, we will most likely encounter an error:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

<span data-ttu-id="92d47-121">Cette erreur se produit si nous n’avons pas configuré un service dans la méthode `ConfigureServices` de notre classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="92d47-121">This error occurs when we have not configured a service in the `ConfigureServices` method in our `Startup` class.</span></span> <span data-ttu-id="92d47-122">Pour spécifier que les demandes de `IDateTime` doivent être résolues en utilisant une instance de `SystemDateTime`, ajoutez la ligne en surbrillance dans la liste ci-dessous à votre méthode `ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="92d47-122">To specify that requests for `IDateTime` should be resolved using an instance of `SystemDateTime`, add the highlighted line in the listing below to your `ConfigureServices` method:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> <span data-ttu-id="92d47-123">Ce service particulier peut être implémenté en utilisant plusieurs options de durée de vie différentes (`Transient`, `Scoped` ou `Singleton`).</span><span class="sxs-lookup"><span data-stu-id="92d47-123">This particular service could be implemented using any of several different lifetime options (`Transient`, `Scoped`, or `Singleton`).</span></span> <span data-ttu-id="92d47-124">Consultez [Injection de dépendances](../../fundamentals/dependency-injection.md) pour comprendre comment chacune de ces options d’étendue affecte le comportement de votre service.</span><span class="sxs-lookup"><span data-stu-id="92d47-124">See [Dependency Injection](../../fundamentals/dependency-injection.md) to understand how each of these scope options will affect the behavior of your service.</span></span>

<span data-ttu-id="92d47-125">Une fois que le service a été configuré, l’exécution de l’application et la navigation jusqu’à la page d’accueil doivent afficher le message basé sur l’heure comme attendu :</span><span class="sxs-lookup"><span data-stu-id="92d47-125">Once the service has been configured, running the application and navigating to the home page should display the time-based message as expected:</span></span>

![Message d’accueil du serveur](dependency-injection/_static/server-greeting.png)

>[!TIP]
> <span data-ttu-id="92d47-127">Consultez [Tester la logique du contrôleur](testing.md) pour découvrir comment demander explicitement les dépendances ([http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/)) dans les contrôleurs afin de rendre le code plus facile à tester.</span><span class="sxs-lookup"><span data-stu-id="92d47-127">See [Test controller logic](testing.md) to learn how to explicitly request dependencies [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) in controllers makes code easier to test.</span></span>

<span data-ttu-id="92d47-128">L’injection de dépendances intégrée d’ASP.NET Core ne prend en charge qu’un seul constructeur pour les classes demandant des services.</span><span class="sxs-lookup"><span data-stu-id="92d47-128">ASP.NET Core's built-in dependency injection supports having only a single constructor for classes requesting services.</span></span> <span data-ttu-id="92d47-129">Si vous avez plusieurs constructeurs, vous pouvez recevoir une exception indiquant :</span><span class="sxs-lookup"><span data-stu-id="92d47-129">If you have more than one constructor, you may get an exception stating:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

<span data-ttu-id="92d47-130">Comme le message d’erreur l’indique, vous pouvez corriger ce problème en n’ayant qu’un seul constructeur.</span><span class="sxs-lookup"><span data-stu-id="92d47-130">As the error message states, you can correct this problem having just a single constructor.</span></span> <span data-ttu-id="92d47-131">Vous pouvez également [remplacer la prise en charge de l’injection de dépendances par défaut par une implémentation de tiers](../../fundamentals/dependency-injection.md#replacing-the-default-services-container), la plupart de ces implémentations prenant en charge plusieurs constructeurs.</span><span class="sxs-lookup"><span data-stu-id="92d47-131">You can also [replace the default dependency injection support with a third party implementation](../../fundamentals/dependency-injection.md#replacing-the-default-services-container), many of which support multiple constructors.</span></span>

## <a name="action-injection-with-fromservices"></a><span data-ttu-id="92d47-132">Injection d’actions avec FromServices</span><span class="sxs-lookup"><span data-stu-id="92d47-132">Action Injection with FromServices</span></span>

<span data-ttu-id="92d47-133">Parfois, vous n’avez pas besoin d’un service pour plusieurs actions dans votre contrôleur.</span><span class="sxs-lookup"><span data-stu-id="92d47-133">Sometimes you don't need a service for more than one action within your controller.</span></span> <span data-ttu-id="92d47-134">Dans ce cas, il peut être judicieux d’injecter le service comme paramètre de la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="92d47-134">In this case, it may make sense to inject the service as a parameter to the action method.</span></span> <span data-ttu-id="92d47-135">Vous faites cela en marquant le paramètre avec l’attribut `[FromServices]`, comme illustré ici :</span><span class="sxs-lookup"><span data-stu-id="92d47-135">This is done by marking the parameter with the attribute `[FromServices]` as shown here:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a><span data-ttu-id="92d47-136">Accès aux paramètres à partir d’un contrôleur</span><span class="sxs-lookup"><span data-stu-id="92d47-136">Accessing Settings from a Controller</span></span>

<span data-ttu-id="92d47-137">L’accès aux paramètres de configuration ou d’application à partir d’un contrôleur est un modèle commun.</span><span class="sxs-lookup"><span data-stu-id="92d47-137">Accessing application or configuration settings from within a controller is a common pattern.</span></span> <span data-ttu-id="92d47-138">Cet accès doit utiliser le modèle Options décrit dans [Configuration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="92d47-138">This access should use the Options pattern described in [configuration](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="92d47-139">D’une façon générale, vous ne devez pas demander les paramètres directement à partir de votre contrôleur avec l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="92d47-139">You generally shouldn't request settings directly from your controller using dependency injection.</span></span> <span data-ttu-id="92d47-140">Une meilleure approche consiste à demander une instance de `IOptions<T>`, où `T` est la classe de configuration dont vous avez besoin.</span><span class="sxs-lookup"><span data-stu-id="92d47-140">A better approach is to request an `IOptions<T>` instance, where `T` is the configuration class you need.</span></span>

<span data-ttu-id="92d47-141">Pour utiliser le modèle Options, vous devez créer une classe qui représente les options, comme celle-ci :</span><span class="sxs-lookup"><span data-stu-id="92d47-141">To work with the options pattern, you need to create a class that represents the options, such as this one:</span></span>

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

<span data-ttu-id="92d47-142">Vous devez ensuite configurer l’application pour qu’elle utilise le modèle Options et ajouter votre classe de configuration à la collection de services dans `ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="92d47-142">Then you need to configure the application to use the options model and add your configuration class to the services collection in `ConfigureServices`:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> <span data-ttu-id="92d47-143">Dans la liste ci-dessus, nous configurons l’application pour qu’elle lise les paramètres dans un fichier au format JSON.</span><span class="sxs-lookup"><span data-stu-id="92d47-143">In the above listing, we are configuring the application to read the settings from a JSON-formatted file.</span></span> <span data-ttu-id="92d47-144">Vous pouvez également configurer les paramètres entièrement dans le code, comme illustré dans le code commenté ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="92d47-144">You can also configure the settings entirely in code, as is shown in the commented code above.</span></span> <span data-ttu-id="92d47-145">Consultez [Configuration](xref:fundamentals/configuration/index) pour d’autres options de configuration.</span><span class="sxs-lookup"><span data-stu-id="92d47-145">See [Configuration](xref:fundamentals/configuration/index) for further configuration options.</span></span>

<span data-ttu-id="92d47-146">Une fois que vous avez spécifié un objet de configuration fortement typé (dans ce cas, `SampleWebSettings`) et que vous l’avez ajouté à la collection de services, vous pouvez le demander à partir de n’importe quelle méthode de contrôleur ou d’action en demandant une instance de `IOptions<T>` (dans ce cas, `IOptions<SampleWebSettings>`).</span><span class="sxs-lookup"><span data-stu-id="92d47-146">Once you've specified a strongly-typed configuration object (in this case, `SampleWebSettings`) and added it to the services collection, you can request it from any Controller or Action method by requesting an instance of `IOptions<T>` (in this case, `IOptions<SampleWebSettings>`).</span></span> <span data-ttu-id="92d47-147">Le code suivant montre comment demander les paramètres à partir d’un contrôleur :</span><span class="sxs-lookup"><span data-stu-id="92d47-147">The following code shows how one would request the settings from a controller:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

<span data-ttu-id="92d47-148">Suivre le modèle Options permet de découpler les paramètres et la configuration, et garantit que le contrôleur est conforme à la [séparation des problèmes](http://deviq.com/separation-of-concerns/), car il n’a pas besoin de savoir ni comment ni où rechercher les informations des paramètres.</span><span class="sxs-lookup"><span data-stu-id="92d47-148">Following the Options pattern allows settings and configuration to be decoupled from one another, and ensures the controller is following [separation of concerns](http://deviq.com/separation-of-concerns/), since it doesn't need to know how or where to find the settings information.</span></span> <span data-ttu-id="92d47-149">Cela simplifie également les tests unitaires du contrôleur ([Tester la logique du contrôleur](testing.md)), car il n’y a pas de [couplage des fonctionnalités statiques](http://deviq.com/static-cling/) ni d’instanciation directe de classes de paramètres au sein de la classe du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="92d47-149">It also makes the controller easier to unit test [Test controller logic](testing.md), since there's no [static cling](http://deviq.com/static-cling/) or direct instantiation of settings classes within the controller class.</span></span>
