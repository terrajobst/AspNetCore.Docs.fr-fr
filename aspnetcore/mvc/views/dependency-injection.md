---
title: "Injection de dépendance dans les vues"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/dependency-injection
ms.openlocfilehash: cade61b1ebdb2b845b07117384475638c0227f7f
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2018
---
# <a name="dependency-injection-into-views"></a><span data-ttu-id="afeb5-102">Injection de dépendance dans les vues</span><span class="sxs-lookup"><span data-stu-id="afeb5-102">Dependency injection into views</span></span>

<span data-ttu-id="afeb5-103">Par [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="afeb5-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="afeb5-104">Prend en charge par ASP.NET Core [injection de dépendance](xref:fundamentals/dependency-injection) dans les vues.</span><span class="sxs-lookup"><span data-stu-id="afeb5-104">ASP.NET Core supports [dependency injection](xref:fundamentals/dependency-injection) into views.</span></span> <span data-ttu-id="afeb5-105">Cela peut être utile pour les services spécifiques à la vue, telles que la localisation ou les données requises pour le remplissage d’éléments de la vue.</span><span class="sxs-lookup"><span data-stu-id="afeb5-105">This can be useful for view-specific services, such as localization or data required only for populating view elements.</span></span> <span data-ttu-id="afeb5-106">Vous devez tenter de mettre à jour [séparation des préoccupations](http://deviq.com/separation-of-concerns/) entre vos contrôleurs et les vues.</span><span class="sxs-lookup"><span data-stu-id="afeb5-106">You should try to maintain [separation of concerns](http://deviq.com/separation-of-concerns/) between your controllers and views.</span></span> <span data-ttu-id="afeb5-107">La plupart de vos affichages de données doit être passée à partir du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="afeb5-107">Most of the data your views display should be passed in from the controller.</span></span>

<span data-ttu-id="afeb5-108">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="afeb5-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="a-simple-example"></a><span data-ttu-id="afeb5-109">Un exemple Simple</span><span class="sxs-lookup"><span data-stu-id="afeb5-109">A Simple Example</span></span>

<span data-ttu-id="afeb5-110">Vous pouvez injecter un service dans une vue à l’aide de la `@inject` la directive.</span><span class="sxs-lookup"><span data-stu-id="afeb5-110">You can inject a service into a view using the `@inject` directive.</span></span> <span data-ttu-id="afeb5-111">Vous pouvez considérer `@inject` en tant que l’ajout d’une propriété à votre affichage et pour renseigner la propriété à l’aide de DI.</span><span class="sxs-lookup"><span data-stu-id="afeb5-111">You can think of `@inject` as adding a property to your view, and populating the property using DI.</span></span>

<span data-ttu-id="afeb5-112">La syntaxe de `@inject`:`@inject <type> <name>`</span><span class="sxs-lookup"><span data-stu-id="afeb5-112">The syntax for `@inject`: `@inject <type> <name>`</span></span>

<span data-ttu-id="afeb5-113">Un exemple de `@inject` en action :</span><span class="sxs-lookup"><span data-stu-id="afeb5-113">An example of `@inject` in action:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

<span data-ttu-id="afeb5-114">Cette vue affiche une liste de `ToDoItem` instances, ainsi qu’un résumé montrant les statistiques globales.</span><span class="sxs-lookup"><span data-stu-id="afeb5-114">This view displays a list of `ToDoItem` instances, along with a summary showing overall statistics.</span></span> <span data-ttu-id="afeb5-115">Le résumé est rempli à partir du texte injecté `StatisticsService`.</span><span class="sxs-lookup"><span data-stu-id="afeb5-115">The summary is populated from the injected `StatisticsService`.</span></span> <span data-ttu-id="afeb5-116">Ce service est inscrit pour l’injection de dépendance dans `ConfigureServices` dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="afeb5-116">This service is registered for dependency injection in `ConfigureServices` in *Startup.cs*:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

<span data-ttu-id="afeb5-117">Le `StatisticsService` effectue des calculs sur l’ensemble de `ToDoItem` instances, auquel elle accède via un référentiel :</span><span class="sxs-lookup"><span data-stu-id="afeb5-117">The `StatisticsService` performs some calculations on the set of `ToDoItem` instances, which it accesses via a repository:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,26)]

<span data-ttu-id="afeb5-118">Le référentiel de l’exemple utilise une collection en mémoire.</span><span class="sxs-lookup"><span data-stu-id="afeb5-118">The sample repository uses an in-memory collection.</span></span> <span data-ttu-id="afeb5-119">L’implémentation ci-dessus (qui fonctionne sur toutes les données en mémoire) n’est pas recommandée pour les jeux de données volumineux, accessible à distance.</span><span class="sxs-lookup"><span data-stu-id="afeb5-119">The implementation shown above (which operates on all of the data in memory) is not recommended for large, remotely accessed data sets.</span></span>

<span data-ttu-id="afeb5-120">L’exemple affiche les données du modèle lié à la vue et le service injectés dans la vue :</span><span class="sxs-lookup"><span data-stu-id="afeb5-120">The sample displays data from the model bound to the view and the service injected into the view:</span></span>

![Pour afficher la liste des éléments de total, terminer des éléments de priorité moyenne et une liste de tâches avec leurs niveaux de priorité et les valeurs booléennes indiquant l’achèvement.](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a><span data-ttu-id="afeb5-122">Remplissage des données de recherche</span><span class="sxs-lookup"><span data-stu-id="afeb5-122">Populating Lookup Data</span></span>

<span data-ttu-id="afeb5-123">Injection de la vue peut être utile pour remplir les options dans les éléments d’interface utilisateur, telles que des listes déroulantes.</span><span class="sxs-lookup"><span data-stu-id="afeb5-123">View injection can be useful to populate options in UI elements, such as dropdown lists.</span></span> <span data-ttu-id="afeb5-124">Envisagez d’un formulaire de profil utilisateur qui inclut des options permettant de spécifier le sexe, l’état et autres préférences.</span><span class="sxs-lookup"><span data-stu-id="afeb5-124">Consider a user profile form that includes options for specifying gender, state, and other preferences.</span></span> <span data-ttu-id="afeb5-125">Rendu d’une forme à l’aide d’une approche MVC standard nécessiterait le contrôleur pour demander des services d’accès aux données pour chacun de ces jeux d’options et puis remplir un modèle ou `ViewBag` avec chaque ensemble d’options pour être lié.</span><span class="sxs-lookup"><span data-stu-id="afeb5-125">Rendering such a form using a standard MVC approach would require the controller to request data access services for each of these sets of options, and then populate a model or `ViewBag` with each set of options to be bound.</span></span>

<span data-ttu-id="afeb5-126">Une autre approche injecte services directement dans la vue pour obtenir les options.</span><span class="sxs-lookup"><span data-stu-id="afeb5-126">An alternative approach injects services directly into the view to obtain the options.</span></span> <span data-ttu-id="afeb5-127">Cela réduit la quantité de code requis par le contrôleur, le déplacement de cette logique de construction d’élément vue dans la vue proprement dite.</span><span class="sxs-lookup"><span data-stu-id="afeb5-127">This minimizes the amount of code required by the controller, moving this view element construction logic into the view itself.</span></span> <span data-ttu-id="afeb5-128">L’action du contrôleur pour afficher un formulaire de modification de profil ne doit passer l’instance de profil :</span><span class="sxs-lookup"><span data-stu-id="afeb5-128">The controller action to display a profile editing form only needs to pass the form the profile instance:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

<span data-ttu-id="afeb5-129">Le formulaire HTML utilisé pour mettre à jour ces préférences inclut les listes déroulantes pour trois propriétés :</span><span class="sxs-lookup"><span data-stu-id="afeb5-129">The HTML form used to update these preferences includes dropdown lists for three of the properties:</span></span>

![Mettre à jour la vue profil avec un formulaire permettant l’entrée de nom, sexe, état et couleur préférée.](dependency-injection/_static/updateprofile.png)

<span data-ttu-id="afeb5-131">Ces listes sont remplies par un service qui a été injecté dans la vue :</span><span class="sxs-lookup"><span data-stu-id="afeb5-131">These lists are populated by a service that has been injected into the view:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

<span data-ttu-id="afeb5-132">Le `ProfileOptionsService` est un service au niveau de l’interface utilisateur conçu pour fournir les données nécessaires pour ce formulaire :</span><span class="sxs-lookup"><span data-stu-id="afeb5-132">The `ProfileOptionsService` is a UI-level service designed to provide just the data needed for this form:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

>[!TIP]
> <span data-ttu-id="afeb5-133">N’oubliez pas d’enregistrer les types de demande par le biais d’injection de dépendance dans le `ConfigureServices` méthode dans *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="afeb5-133">Don't forget to register types you will request through dependency injection in the  `ConfigureServices` method in *Startup.cs*.</span></span>

## <a name="overriding-services"></a><span data-ttu-id="afeb5-134">Substitution des Services</span><span class="sxs-lookup"><span data-stu-id="afeb5-134">Overriding Services</span></span>

<span data-ttu-id="afeb5-135">En plus de l’injection de nouveaux services, cette technique peut également être utilisée pour remplacer des services injectées précédemment sur une page.</span><span class="sxs-lookup"><span data-stu-id="afeb5-135">In addition to injecting new services, this technique can also be used to override previously injected services on a page.</span></span> <span data-ttu-id="afeb5-136">La figure ci-dessous montre tous les champs disponibles dans la page utilisée dans le premier exemple :</span><span class="sxs-lookup"><span data-stu-id="afeb5-136">The figure below shows all of the fields available on the page used in the first example:</span></span>

![Menu contextuel d’IntelliSense sur typé qui répertorie les champs Html, composant, StatsService et Url de symbole @](dependency-injection/_static/razor-fields.png)

<span data-ttu-id="afeb5-138">Comme vous pouvez le voir, les champs par défaut incluent `Html`, `Component`, et `Url` (ainsi que le `StatsService` qui nous injectées).</span><span class="sxs-lookup"><span data-stu-id="afeb5-138">As you can see, the default fields include `Html`, `Component`, and `Url` (as well as the `StatsService` that we injected).</span></span> <span data-ttu-id="afeb5-139">Si vous souhaitez par exemple remplacer les programmes d’assistance HTML de valeur par défaut par les vôtres, facilement procéder à l’aide de `@inject`:</span><span class="sxs-lookup"><span data-stu-id="afeb5-139">If for instance you wanted to replace the default HTML Helpers with your own, you could easily do so using `@inject`:</span></span>

[!code-html[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

<span data-ttu-id="afeb5-140">Si vous souhaitez étendre les services existants, vous pouvez simplement utiliser cette technique lors héritant ou encapsulant l’implémentation existante par les vôtres.</span><span class="sxs-lookup"><span data-stu-id="afeb5-140">If you want to extend existing services, you can simply use this technique while inheriting from or wrapping the existing implementation with your own.</span></span>

## <a name="see-also"></a><span data-ttu-id="afeb5-141">Voir aussi</span><span class="sxs-lookup"><span data-stu-id="afeb5-141">See Also</span></span>

* <span data-ttu-id="afeb5-142">Blog de Simon Timms : [mise en route de rechercher des données dans votre vue](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span><span class="sxs-lookup"><span data-stu-id="afeb5-142">Simon Timms Blog: [Getting Lookup Data Into Your View](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span></span>
