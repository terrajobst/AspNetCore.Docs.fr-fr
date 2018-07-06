---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: Filtres d’Action personnalisés ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC fournit des filtres d’Action pour exécuter la logique de filtrage avant ou après l’appel d’une méthode d’action. Filtres d’action sont des attributs personnalisés tha...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: c435fef624d526ceb01dbc370c5df52e2a1e8350
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807933"
---
# <a name="aspnet-mvc-4-custom-action-filters"></a><span data-ttu-id="7e438-104">Filtres d’Action personnalisés ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="7e438-104">ASP.NET MVC 4 Custom Action Filters</span></span>

<span data-ttu-id="7e438-105">par [Web Camps Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="7e438-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="7e438-106">Télécharger le Kit de formation de Web Camps</span><span class="sxs-lookup"><span data-stu-id="7e438-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="7e438-107">ASP.NET MVC fournit des filtres d’Action pour exécuter la logique de filtrage avant ou après l’appel d’une méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="7e438-107">ASP.NET MVC provides Action Filters for executing filtering logic either before or after an action method is called.</span></span> <span data-ttu-id="7e438-108">Filtres d’action sont des attributs personnalisés qui fournissent un moyen déclaratif pour ajouter un comportement pré et post-action aux méthodes d’action du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="7e438-108">Action Filters are custom attributes that provide declarative means to add pre-action and post-action behavior to the controller's action methods.</span></span>

<span data-ttu-id="7e438-109">Dans ce laboratoire pratique, vous allez créer un attribut de filtre d’action personnalisée dans la solution MvcMusicStore pour intercepter les demandes du contrôleur et les journaux de l’activité d’un site dans une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="7e438-109">In this Hands-on Lab you will create a custom action filter attribute into MvcMusicStore solution to catch controller's requests and log the activity of a site into a database table.</span></span> <span data-ttu-id="7e438-110">Vous serez en mesure d’ajouter votre filtre de journalisation par injection à n’importe quel contrôleur ou d’action.</span><span class="sxs-lookup"><span data-stu-id="7e438-110">You will be able to add your logging filter by injection to any controller or action.</span></span> <span data-ttu-id="7e438-111">Enfin, vous voyez s’afficher le journal qui affiche la liste des visiteurs.</span><span class="sxs-lookup"><span data-stu-id="7e438-111">Finally, you will see the log view that shows the list of visitors.</span></span>

<span data-ttu-id="7e438-112">Ce laboratoire pratique suppose que vous avez une connaissance élémentaire de **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="7e438-112">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="7e438-113">Si vous n’avez pas utilisé **ASP.NET MVC** auparavant, nous vous recommandons de consulter **principes de base ASP.NET MVC 4** les ateliers pratiques.</span><span class="sxs-lookup"><span data-stu-id="7e438-113">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

> [!NOTE]
> <span data-ttu-id="7e438-114">Tous les exemples de code et extraits de code sont inclus dans le Kit de formation Camps Web, disponible à partir d’à l’adresse [les versions de Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="7e438-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="7e438-115">Le projet spécifique à ce laboratoire est disponible à l’adresse [filtres d’Action ASP.NET MVC 4 personnalisé](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).</span><span class="sxs-lookup"><span data-stu-id="7e438-115">The project specific to this lab is available at [ASP.NET MVC 4 Custom Action Filters](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="7e438-116">Objectifs</span><span class="sxs-lookup"><span data-stu-id="7e438-116">Objectives</span></span>

<span data-ttu-id="7e438-117">Dans cet atelier pratique, vous allez apprendre comment :</span><span class="sxs-lookup"><span data-stu-id="7e438-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="7e438-118">Créer un attribut de filtre d’action personnalisée pour étendre les fonctionnalités de filtrage</span><span class="sxs-lookup"><span data-stu-id="7e438-118">Create a custom action filter attribute to extend filtering capabilities</span></span>
- <span data-ttu-id="7e438-119">Appliquer un attribut de filtre personnalisé à un niveau spécifique de l’injection de code</span><span class="sxs-lookup"><span data-stu-id="7e438-119">Apply a custom filter attribute by injection to a specific level</span></span>
- <span data-ttu-id="7e438-120">Inscrire un filtre d’action personnalisée dans le monde entier</span><span class="sxs-lookup"><span data-stu-id="7e438-120">Register a custom action filters globally</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="7e438-121">Prérequis</span><span class="sxs-lookup"><span data-stu-id="7e438-121">Prerequisites</span></span>

<span data-ttu-id="7e438-122">Vous devez disposer des éléments suivants pour terminer ce laboratoire :</span><span class="sxs-lookup"><span data-stu-id="7e438-122">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="7e438-123">[Microsoft Visual Studio Express 2012 pour Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou supérieure (lire [annexe A](#AppendixA) pour obtenir des instructions sur la façon d’installer).</span><span class="sxs-lookup"><span data-stu-id="7e438-123">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="7e438-124">Installation</span><span class="sxs-lookup"><span data-stu-id="7e438-124">Setup</span></span>

<span data-ttu-id="7e438-125">**L’installation des extraits de Code**</span><span class="sxs-lookup"><span data-stu-id="7e438-125">**Installing Code Snippets**</span></span>

<span data-ttu-id="7e438-126">Pour des raisons pratiques, une grande partie du code que vous gérez le long de ce laboratoire est disponible en tant que les extraits de code Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7e438-126">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="7e438-127">Pour installer les extraits de code exécuter **.\Source\Setup\CodeSnippets.vsi** fichier.</span><span class="sxs-lookup"><span data-stu-id="7e438-127">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="7e438-128">Si vous n’êtes pas familiarisé avec les extraits de Code Visual Studio et que vous souhaitiez savoir comment les utiliser, vous pouvez faire référence à l’annexe de ce document &quot; [extraits de Code à l’aide d’annexe c :](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="7e438-128">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="7e438-129">Exercices</span><span class="sxs-lookup"><span data-stu-id="7e438-129">Exercises</span></span>

<span data-ttu-id="7e438-130">Ce laboratoire pratique est constitué par les exercices suivants :</span><span class="sxs-lookup"><span data-stu-id="7e438-130">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="7e438-131">Exercice 1 : Enregistrement des actions</span><span class="sxs-lookup"><span data-stu-id="7e438-131">Exercise 1: Logging actions</span></span>](#Exercise1)
2. [<span data-ttu-id="7e438-132">Exercice 2 : Gestion de plusieurs filtres d’Action</span><span class="sxs-lookup"><span data-stu-id="7e438-132">Exercise 2: Managing Multiple Action Filters</span></span>](#Exercise2)

<span data-ttu-id="7e438-133">Durée estimée pour effectuer ce laboratoire : **30 minutes**.</span><span class="sxs-lookup"><span data-stu-id="7e438-133">Estimated time to complete this lab: **30 minutes**.</span></span>

> [!NOTE]
> <span data-ttu-id="7e438-134">Chaque exercice est accompagné par un **fin** dossier contenant la solution obtenue, vous devez obtenir après avoir effectué les exercices.</span><span class="sxs-lookup"><span data-stu-id="7e438-134">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="7e438-135">Si vous avez besoin d’aide supplémentaire sur l’utilisation via les exercices, vous pouvez utiliser cette solution comme guide.</span><span class="sxs-lookup"><span data-stu-id="7e438-135">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a><span data-ttu-id="7e438-136">Exercice 1 : Enregistrement des Actions</span><span class="sxs-lookup"><span data-stu-id="7e438-136">Exercise 1: Logging Actions</span></span>

<span data-ttu-id="7e438-137">Dans cet exercice, vous allez apprendre à créer un filtre de journal d’action personnalisé à l’aide de fournisseurs de filtres ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="7e438-137">In this exercise, you will learn how to create a custom action log filter by using ASP.NET MVC 4 Filter Providers.</span></span> <span data-ttu-id="7e438-138">Pour ce faire, vous appliquerez un filtre de journalisation sur le site du Store musique qui enregistre toutes les activités dans les contrôleurs sélectionnés.</span><span class="sxs-lookup"><span data-stu-id="7e438-138">For that purpose you will apply a logging filter to the MusicStore site that will record all the activities in the selected controllers.</span></span>

<span data-ttu-id="7e438-139">Le filtre étendra **ActionFilterAttributeClass** et remplacer **OnActionExecuting** méthode pour intercepter chaque requête, puis effectuez les actions de journalisation.</span><span class="sxs-lookup"><span data-stu-id="7e438-139">The filter will extend **ActionFilterAttributeClass** and override **OnActionExecuting** method to catch each request and then perform the logging actions.</span></span> <span data-ttu-id="7e438-140">Informations de contexte sur les requêtes HTTP, l’exécution de méthodes, les résultats et les paramètres est fournie par ASP.NET MVC **ActionExecutingContext** classe **.**</span><span class="sxs-lookup"><span data-stu-id="7e438-140">The context information about HTTP requests, executing methods, results and parameters will be provided by ASP.NET MVC **ActionExecutingContext** class **.**</span></span>

> [!NOTE]
> <span data-ttu-id="7e438-141">ASP.NET MVC 4 a également des fournisseurs de filtres par défaut que vous pouvez utiliser sans créer un filtre personnalisé.</span><span class="sxs-lookup"><span data-stu-id="7e438-141">ASP.NET MVC 4 also has default filters providers you can use without creating a custom filter.</span></span> <span data-ttu-id="7e438-142">ASP.NET MVC 4 fournit les types de filtres suivants :</span><span class="sxs-lookup"><span data-stu-id="7e438-142">ASP.NET MVC 4 provides the following types of filters:</span></span>
> 
> - <span data-ttu-id="7e438-143">**Autorisation** filtrer, ce qui rend les décisions de sécurité sur la nécessité d’exécuter une méthode d’action, telles que l’exécution de l’authentification ou la validation des propriétés de la requête.</span><span class="sxs-lookup"><span data-stu-id="7e438-143">**Authorization** filter, which makes security decisions about whether to execute an action method, such as performing authentication or validating properties of the request.</span></span>
> - <span data-ttu-id="7e438-144">**Action** filtre, qui encapsule l’exécution de méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="7e438-144">**Action** filter, which wraps the action method execution.</span></span> <span data-ttu-id="7e438-145">Ce filtre peut effectuer un traitement supplémentaire, tel que fournir des données supplémentaires à la méthode d’action, inspecter la valeur de retour ou annuler l’exécution de la méthode d’action</span><span class="sxs-lookup"><span data-stu-id="7e438-145">This filter can perform additional processing, such as providing extra data to the action method, inspecting the return value, or canceling execution of the action method</span></span>
> - <span data-ttu-id="7e438-146">**Résultat** filtre, qui encapsule l’exécution de l’objet ActionResult.</span><span class="sxs-lookup"><span data-stu-id="7e438-146">**Result** filter, which wraps execution of the ActionResult object.</span></span> <span data-ttu-id="7e438-147">Ce filtre peut exécuter d’autres traitements du résultat, telles que la modification de la réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="7e438-147">This filter can perform additional processing of the result, such as modifying the HTTP response.</span></span>
> - <span data-ttu-id="7e438-148">**Exception** filtre, qui s’exécute si une exception non gérée levée dans une méthode d’action, en commençant par les filtres d’autorisation et se terminant par l’exécution du résultat.</span><span class="sxs-lookup"><span data-stu-id="7e438-148">**Exception** filter, which executes if there is an unhandled exception thrown somewhere in action method, starting with the authorization filters and ending with the execution of the result.</span></span> <span data-ttu-id="7e438-149">Filtres d’exception peuvent être utilisés pour des tâches telles que la journalisation ou l’affichage d’une page d’erreur.</span><span class="sxs-lookup"><span data-stu-id="7e438-149">Exception filters can be used for tasks such as logging or displaying an error page.</span></span>
> 
> <span data-ttu-id="7e438-150">Pour plus d’informations sur les fournisseurs de filtres, visitez ce lien MSDN : ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).</span><span class="sxs-lookup"><span data-stu-id="7e438-150">For more information about Filters Providers please visit this MSDN link: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)) .</span></span>


<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a><span data-ttu-id="7e438-151">À propos de la fonctionnalité de journalisation de l’Application du Store musique MVC</span><span class="sxs-lookup"><span data-stu-id="7e438-151">About MVC Music Store Application logging feature</span></span>

<span data-ttu-id="7e438-152">Cette solution Music Store a une nouvelle table de modèle de données pour la journalisation du site, **ActionLog**, avec les champs suivants : nom du contrôleur qui a reçu une demande, l’appelées action, l’adresse IP du Client et l’horodatage.</span><span class="sxs-lookup"><span data-stu-id="7e438-152">This Music Store solution has a new data model table for site logging, **ActionLog**, with the following fields: Name of the controller that received a request, Called action, Client IP and Time stamp.</span></span>

<span data-ttu-id="7e438-153">![Modèle de données. Table ActionLog. ](aspnet-mvc-4-custom-action-filters/_static/image1.png "Modèle de données. Table ActionLog.")</span><span class="sxs-lookup"><span data-stu-id="7e438-153">![Data model. ActionLog table.](aspnet-mvc-4-custom-action-filters/_static/image1.png "Data model. ActionLog table.")</span></span>

<span data-ttu-id="7e438-154">*Modèle de données - ActionLog table*</span><span class="sxs-lookup"><span data-stu-id="7e438-154">*Data model - ActionLog table*</span></span>

<span data-ttu-id="7e438-155">La solution fournit une vue de MVC ASP.NET pour le journal des actions qui se trouve à **MvcMusicStores/vues/ActionLog**:</span><span class="sxs-lookup"><span data-stu-id="7e438-155">The solution provides an ASP.NET MVC View for the Action log that can be found at **MvcMusicStores/Views/ActionLog**:</span></span>

<span data-ttu-id="7e438-156">![Affichage du journal action](aspnet-mvc-4-custom-action-filters/_static/image2.png "affichage du journal des actions")</span><span class="sxs-lookup"><span data-stu-id="7e438-156">![Action Log view](aspnet-mvc-4-custom-action-filters/_static/image2.png "Action Log view")</span></span>

<span data-ttu-id="7e438-157">*Affichage de journal d’action*</span><span class="sxs-lookup"><span data-stu-id="7e438-157">*Action Log view*</span></span>

<span data-ttu-id="7e438-158">Avec cette structure de données, tout le travail portent sur interrompre les demande du contrôleur et l’exécution de la journalisation à l’aide de filtrage personnalisé.</span><span class="sxs-lookup"><span data-stu-id="7e438-158">With this given structure, all the work will be focused on interrupting controller's request and performing the logging by using custom filtering.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a><span data-ttu-id="7e438-159">Tâche 1 : création d’un filtre personnalisé pour intercepter les demande d’un contrôleur</span><span class="sxs-lookup"><span data-stu-id="7e438-159">Task 1 - Creating a Custom Filter to Catch a Controller's Request</span></span>

<span data-ttu-id="7e438-160">Dans cette tâche, vous allez créer une classe d’attributs de filtre personnalisé qui contiendra la logique de journalisation.</span><span class="sxs-lookup"><span data-stu-id="7e438-160">In this task you will create a custom filter attribute class that will contain the logging logic.</span></span> <span data-ttu-id="7e438-161">Pour ce faire, vous allez étendre ASP.NET MVC **ActionFilterAttribute** et implémentez l’interface **IActionFilter**.</span><span class="sxs-lookup"><span data-stu-id="7e438-161">For that purpose you will extend ASP.NET MVC **ActionFilterAttribute** Class and implement the interface **IActionFilter**.</span></span>

> [!NOTE]
> <span data-ttu-id="7e438-162">Le **ActionFilterAttribute** est la classe de base pour tous les filtres d’attribut.</span><span class="sxs-lookup"><span data-stu-id="7e438-162">The **ActionFilterAttribute** is the base class for all the attribute filters.</span></span> <span data-ttu-id="7e438-163">Il fournit les méthodes suivantes pour exécuter une logique spécifique après et avant l’exécution de l’action de contrôleur :</span><span class="sxs-lookup"><span data-stu-id="7e438-163">It provides the following methods to execute a specific logic after and before controller action's execution:</span></span>
> 
> - <span data-ttu-id="7e438-164">**OnActionExecuting**(ActionExecutingContext filterContext) : juste avant l’action de la méthode est appelée.</span><span class="sxs-lookup"><span data-stu-id="7e438-164">**OnActionExecuting**(ActionExecutingContext filterContext): Just before the action method is called.</span></span>
> - <span data-ttu-id="7e438-165">**OnActionExecuted**(ActionExecutedContext filterContext) : une fois que la méthode d’action est appelée et avant l’exécution du résultat (avant le rendu de la vue).</span><span class="sxs-lookup"><span data-stu-id="7e438-165">**OnActionExecuted**(ActionExecutedContext filterContext): After the action method is called and before the result is executed (before view render).</span></span>
> - <span data-ttu-id="7e438-166">**OnResultExecuting**(ResultExecutingContext filterContext) : juste avant l’exécution du résultat (avant le rendu de la vue).</span><span class="sxs-lookup"><span data-stu-id="7e438-166">**OnResultExecuting**(ResultExecutingContext filterContext): Just before the result is executed (before view render).</span></span>
> - <span data-ttu-id="7e438-167">**OnResultExecuted**(ResultExecutedContext filterContext) : après l’exécution du résultat (une fois que la vue est restituée).</span><span class="sxs-lookup"><span data-stu-id="7e438-167">**OnResultExecuted**(ResultExecutedContext filterContext): After the result is executed (after the view is rendered).</span></span>
> 
> <span data-ttu-id="7e438-168">En substituant une de ces méthodes dans une classe dérivée, vous pouvez exécuter votre propre code de filtrage.</span><span class="sxs-lookup"><span data-stu-id="7e438-168">By overriding any of these methods into a derived class, you can execute your own filtering code.</span></span>


1. <span data-ttu-id="7e438-169">Ouvrez le **commencer** solution situé dans **\Source\Ex01-LoggingActions\Begin** dossier.</span><span class="sxs-lookup"><span data-stu-id="7e438-169">Open the **Begin** solution located at **\Source\Ex01-LoggingActions\Begin** folder.</span></span>

   1. <span data-ttu-id="7e438-170">Vous devrez télécharger certains packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="7e438-170">You will need to download some missing NuGet packages before you continue.</span></span> <span data-ttu-id="7e438-171">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="7e438-171">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="7e438-172">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="7e438-172">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="7e438-173">Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="7e438-173">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="7e438-174">Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="7e438-174">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="7e438-175">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="7e438-175">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="7e438-176">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="7e438-176">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="7e438-177">Pour plus d’informations, consultez cet article : [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="7e438-177">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="7e438-178">Ajoutez une nouvelle classe c# dans le **filtres** dossier et nommez-le *CustomActionFilter.cs*.</span><span class="sxs-lookup"><span data-stu-id="7e438-178">Add a new C# class into the **Filters** folder and name it *CustomActionFilter.cs*.</span></span> <span data-ttu-id="7e438-179">Ce dossier stocke tous les filtres personnalisés.</span><span class="sxs-lookup"><span data-stu-id="7e438-179">This folder will store all the custom filters.</span></span>
3. <span data-ttu-id="7e438-180">Ouvrez **CustomActionFilter.cs** et ajoutez une référence à **System.Web.Mvc** et **MvcMusicStore.Models** espaces de noms :</span><span class="sxs-lookup"><span data-stu-id="7e438-180">Open **CustomActionFilter.cs** and add a reference to **System.Web.Mvc** and **MvcMusicStore.Models** namespaces:</span></span>

    <span data-ttu-id="7e438-181">(Code Snippet - *filtres d’Action personnalisés ASP.NET MVC 4 - Ex1-CustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="7e438-181">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-CustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. <span data-ttu-id="7e438-182">Hériter la **CustomActionFilter** classe **ActionFilterAttribute** et apportez **CustomActionFilter** implémentent **IActionFilter** interface.</span><span class="sxs-lookup"><span data-stu-id="7e438-182">Inherit the **CustomActionFilter** class from **ActionFilterAttribute** and then make **CustomActionFilter** class implement **IActionFilter** interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. <span data-ttu-id="7e438-183">Rendre **CustomActionFilter** classe substituer la méthode **OnActionExecuting** et ajouter la logique nécessaire pour journaliser l’exécution du filtre.</span><span class="sxs-lookup"><span data-stu-id="7e438-183">Make **CustomActionFilter** class override the method **OnActionExecuting** and add the necessary logic to log the filter's execution.</span></span> <span data-ttu-id="7e438-184">Pour ce faire, ajoutez le code en surbrillance suivant au sein de **CustomActionFilter** classe.</span><span class="sxs-lookup"><span data-stu-id="7e438-184">To do this, add the following highlighted code within **CustomActionFilter** class.</span></span>

    <span data-ttu-id="7e438-185">(Code Snippet - *filtres d’Action personnalisés ASP.NET MVC 4 - Ex1-LoggingActions*)</span><span class="sxs-lookup"><span data-stu-id="7e438-185">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-LoggingActions*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > <span data-ttu-id="7e438-186">**OnActionExecuting** à l’aide de la méthode **Entity Framework** pour ajouter un nouvel historique ActionLog.</span><span class="sxs-lookup"><span data-stu-id="7e438-186">**OnActionExecuting** method is using **Entity Framework** to add a new ActionLog register.</span></span> <span data-ttu-id="7e438-187">Il crée et remplit une instance d’entité avec les informations de contexte à partir de **filterContext**.</span><span class="sxs-lookup"><span data-stu-id="7e438-187">It creates and fills a new entity instance with the context information from **filterContext**.</span></span>
    > 
    > <span data-ttu-id="7e438-188">Vous pouvez en savoir plus sur **ControllerContext** classe au [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).</span><span class="sxs-lookup"><span data-stu-id="7e438-188">You can read more about **ControllerContext** class at [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a><span data-ttu-id="7e438-189">Tâche 2 : injection d’un intercepteur de Code dans la classe de contrôleur Store</span><span class="sxs-lookup"><span data-stu-id="7e438-189">Task 2 - Injecting a Code Interceptor into the Store Controller Class</span></span>

<span data-ttu-id="7e438-190">Dans cette tâche, vous allez ajouter le filtre personnalisé en injectant à toutes les classes de contrôleur et les actions de contrôleur qui seront enregistrées.</span><span class="sxs-lookup"><span data-stu-id="7e438-190">In this task you will add the custom filter by injecting it to all controller classes and controller actions that will be logged.</span></span> <span data-ttu-id="7e438-191">Pour les besoins de cet exercice, la classe de contrôleur de Store aura un journal.</span><span class="sxs-lookup"><span data-stu-id="7e438-191">For the purpose of this exercise, the Store Controller class will have a log.</span></span>

<span data-ttu-id="7e438-192">La méthode **OnActionExecuting** de **ActionLogFilterAttribute** filtre personnalisé s’exécute lorsqu’un élément injecté est appelé.</span><span class="sxs-lookup"><span data-stu-id="7e438-192">The method **OnActionExecuting** from **ActionLogFilterAttribute** custom filter runs when an injected element is called.</span></span>

<span data-ttu-id="7e438-193">Il est également possible d’intercepter une méthode de contrôleur spécifique.</span><span class="sxs-lookup"><span data-stu-id="7e438-193">It is also possible to intercept a specific controller method.</span></span>

1. <span data-ttu-id="7e438-194">Ouvrez le **StoreController** à **MvcMusicStore\Controllers** et ajoutez une référence à la **filtres** espace de noms :</span><span class="sxs-lookup"><span data-stu-id="7e438-194">Open the **StoreController** at **MvcMusicStore\Controllers** and add a reference to the **Filters** namespace:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. <span data-ttu-id="7e438-195">Injecter le filtre personnalisé **CustomActionFilter** dans **StoreController** classe en ajoutant **[CustomActionFilter]** attribut avant la déclaration de classe.</span><span class="sxs-lookup"><span data-stu-id="7e438-195">Inject the custom filter **CustomActionFilter** into **StoreController** class by adding **[CustomActionFilter]** attribute before the class declaration.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > <span data-ttu-id="7e438-196">Lorsqu’un filtre est injecté dans une classe de contrôleur, tous ses actions sont également injectées.</span><span class="sxs-lookup"><span data-stu-id="7e438-196">When a filter is injected into a controller class, all its actions are also injected.</span></span> <span data-ttu-id="7e438-197">Si vous souhaitez appliquer le filtre uniquement pour un ensemble d’actions, vous seriez obligé d’injecter **[CustomActionFilter]** pour chacun d'entre eux :</span><span class="sxs-lookup"><span data-stu-id="7e438-197">If you would like to apply the filter only for a set of actions, you would have to inject **[CustomActionFilter]** to each one of them:</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="7e438-198">Tâche 3 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="7e438-198">Task 3 - Running the Application</span></span>

<span data-ttu-id="7e438-199">Dans cette tâche, vous allez tester que le filtre de journalisation fonctionne.</span><span class="sxs-lookup"><span data-stu-id="7e438-199">In this task, you will test that the logging filter is working.</span></span> <span data-ttu-id="7e438-200">Vous allez démarrer l’application et visitez le magasin, puis vous allez vérifier les activités journalisées.</span><span class="sxs-lookup"><span data-stu-id="7e438-200">You will start the application and visit the store, and then you will check logged activities.</span></span>

1. <span data-ttu-id="7e438-201">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="7e438-201">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="7e438-202">Accédez à **/ActionLog** pour afficher l’état initial de vue de journal :</span><span class="sxs-lookup"><span data-stu-id="7e438-202">Browse to **/ActionLog** to see log view initial state:</span></span>

    <span data-ttu-id="7e438-203">![État de mise hors tension avant l’activité de la page du journal](aspnet-mvc-4-custom-action-filters/_static/image3.png "état mise hors tension avant l’activité de la page du journal")</span><span class="sxs-lookup"><span data-stu-id="7e438-203">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image3.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="7e438-204">*État de suivi du journal avant l’activité de la page*</span><span class="sxs-lookup"><span data-stu-id="7e438-204">*Log tracker status before page activity*</span></span>

   > [!NOTE]
   > <span data-ttu-id="7e438-205">Par défaut, elle affichera toujours un élément qui est généré lors de la récupération des genres existants pour le menu.</span><span class="sxs-lookup"><span data-stu-id="7e438-205">By default, it will always show one item that is generated when retrieving the existing genres for the menu.</span></span>
   > 
   > <span data-ttu-id="7e438-206">Pour des raisons de simplicité nous nettoyons le **ActionLog** chaque fois que l’application s’exécute, elle s’affiche uniquement les journaux de vérification de chaque tâche particulière la table.</span><span class="sxs-lookup"><span data-stu-id="7e438-206">For simplicity purposes we're cleaning up the **ActionLog** table each time the application runs so it will only show the logs of each particular task's verification.</span></span>
   > 
   > <span data-ttu-id="7e438-207">Vous devrez peut-être supprimer le code suivant à partir de la **Session\_Démarrer** (méthode) (dans le **Global.asax** classe), afin d’enregistrer un journal d’historique pour toutes les actions exécutées dans le Store Contrôleur.</span><span class="sxs-lookup"><span data-stu-id="7e438-207">You might need to remove the following code from the **Session\_Start** method (in the **Global.asax** class), in order to save an historical log for all the actions executed within the Store Controller.</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. <span data-ttu-id="7e438-208">Cliquez sur une de le **Genres** à partir du menu et d’effectuer certaines actions, telles que la navigation d’un album disponible.</span><span class="sxs-lookup"><span data-stu-id="7e438-208">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
4. <span data-ttu-id="7e438-209">Accédez à **/ActionLog** et si le journal est vide press **F5** pour actualiser la page.</span><span class="sxs-lookup"><span data-stu-id="7e438-209">Browse to **/ActionLog** and if the log is empty press **F5** to refresh the page.</span></span> <span data-ttu-id="7e438-210">Vérifiez que vos visites ont été suivies :</span><span class="sxs-lookup"><span data-stu-id="7e438-210">Check that your visits were tracked:</span></span>

    <span data-ttu-id="7e438-211">![Journal des actions avec l’activité connectée](aspnet-mvc-4-custom-action-filters/_static/image4.png "journal des actions avec l’activité connectée")</span><span class="sxs-lookup"><span data-stu-id="7e438-211">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image4.png "Action log with activity logged")</span></span>

    <span data-ttu-id="7e438-212">*Journal des actions avec l’activité connectée*</span><span class="sxs-lookup"><span data-stu-id="7e438-212">*Action log with activity logged*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a><span data-ttu-id="7e438-213">Exercice 2 : Gestion de plusieurs filtres d’Action</span><span class="sxs-lookup"><span data-stu-id="7e438-213">Exercise 2: Managing Multiple Action Filters</span></span>

<span data-ttu-id="7e438-214">Dans cet exercice, vous ajoutez un deuxième filtre d’Action personnalisé à la classe StoreController et définir l’ordre spécifique dans lequel les deux filtres seront exécutés.</span><span class="sxs-lookup"><span data-stu-id="7e438-214">In this exercise you will add a second Custom Action Filter to the StoreController class and define the specific order in which both filters will be executed.</span></span> <span data-ttu-id="7e438-215">Ensuite, vous mettrez à jour le code pour enregistrer le filtre dans le monde entier.</span><span class="sxs-lookup"><span data-stu-id="7e438-215">Then you will update the code to register the filter Globally.</span></span>

<span data-ttu-id="7e438-216">Il existe différentes options à prendre en compte lors de la définition de l’ordre d’exécution des filtres.</span><span class="sxs-lookup"><span data-stu-id="7e438-216">There are different options to take into account when defining the Filters' execution order.</span></span> <span data-ttu-id="7e438-217">Par exemple, la propriété de l’ordre et la portée des filtres :</span><span class="sxs-lookup"><span data-stu-id="7e438-217">For example, the Order property and the Filters' scope:</span></span>

<span data-ttu-id="7e438-218">Vous pouvez définir un **étendue** pour chacun des filtres, par exemple, vous pouvez définir l’étendue tous les filtres d’Action à exécuter dans le **étendue du contrôleur**et tous les filtres d’autorisation pour exécuter **portée globale** .</span><span class="sxs-lookup"><span data-stu-id="7e438-218">You can define a **Scope** for each of the Filters, for example, you could scope all the Action Filters to run within the **Controller Scope**, and all Authorization Filters to run in **Global scope**.</span></span> <span data-ttu-id="7e438-219">Les étendues ont un ordre d’exécution défini.</span><span class="sxs-lookup"><span data-stu-id="7e438-219">The scopes have a defined execution order.</span></span>

<span data-ttu-id="7e438-220">En outre, chaque filtre d’action a une propriété de commande qui est utilisée pour déterminer l’ordre d’exécution dans la portée du filtre.</span><span class="sxs-lookup"><span data-stu-id="7e438-220">Additionally, each action filter has an Order property which is used to determine the execution order in the scope of the filter.</span></span>

<span data-ttu-id="7e438-221">Pour plus d’informations sur l’ordre d’exécution de filtres d’Action personnalisée, consultez cet article MSDN : ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).</span><span class="sxs-lookup"><span data-stu-id="7e438-221">For more information about Custom Action Filters execution order, please visit this MSDN article: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a><span data-ttu-id="7e438-222">Tâche 1 : Création d’un nouveau filtre d’Action personnalisé</span><span class="sxs-lookup"><span data-stu-id="7e438-222">Task 1: Creating a new Custom Action Filter</span></span>

<span data-ttu-id="7e438-223">Dans cette tâche, vous allez créer un nouveau filtre d’Action personnalisé à injecter dans la classe StoreController, apprendre à gérer l’ordre d’exécution des filtres.</span><span class="sxs-lookup"><span data-stu-id="7e438-223">In this task, you will create a new Custom Action Filter to inject into the StoreController class, learning how to manage the execution order of the filters.</span></span>

1. <span data-ttu-id="7e438-224">Ouvrez le **commencer** solution situé dans **\Source\Ex02-ManagingMultipleActionFilters\Begin** dossier.</span><span class="sxs-lookup"><span data-stu-id="7e438-224">Open the **Begin** solution located at **\Source\Ex02-ManagingMultipleActionFilters\Begin** folder.</span></span> <span data-ttu-id="7e438-225">Sinon, vous pouvez continuer à utiliser le **fin** solution obtenu par le biais de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="7e438-225">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="7e438-226">Si vous avez ouvert le fourni **commencer** solution, vous devrez télécharger certains packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="7e438-226">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="7e438-227">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="7e438-227">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="7e438-228">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="7e438-228">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="7e438-229">Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="7e438-229">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

        > [!NOTE]
        > <span data-ttu-id="7e438-230">Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="7e438-230">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="7e438-231">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="7e438-231">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="7e438-232">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="7e438-232">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
        > 
        > <span data-ttu-id="7e438-233">Pour plus d’informations, consultez cet article : [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="7e438-233">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="7e438-234">Ajoutez une nouvelle classe c# dans le **filtres** dossier et nommez-le *MyNewCustomActionFilter.cs*</span><span class="sxs-lookup"><span data-stu-id="7e438-234">Add a new C# class into the **Filters** folder and name it *MyNewCustomActionFilter.cs*</span></span>
3. <span data-ttu-id="7e438-235">Ouvrez **MyNewCustomActionFilter.cs** et ajoutez une référence à **System.Web.Mvc** et **MvcMusicStore.Models** espace de noms :</span><span class="sxs-lookup"><span data-stu-id="7e438-235">Open **MyNewCustomActionFilter.cs** and add a reference to **System.Web.Mvc** and the **MvcMusicStore.Models** namespace:</span></span>

    <span data-ttu-id="7e438-236">(Code Snippet - *filtres d’Action personnalisés ASP.NET MVC 4 - Ex2-MyNewCustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="7e438-236">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. <span data-ttu-id="7e438-237">Remplacez la déclaration de classe par défaut par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="7e438-237">Replace the default class declaration with the following code.</span></span>

    <span data-ttu-id="7e438-238">(Code Snippet - *filtres d’Action personnalisés ASP.NET MVC 4 - Ex2-MyNewCustomActionFilterClass*)</span><span class="sxs-lookup"><span data-stu-id="7e438-238">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterClass*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="7e438-239">Ce filtre d’Action personnalisé est presque identique à celle que vous avez créé dans l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="7e438-239">This Custom Action Filter is almost the same than the one you created in the previous exercise.</span></span> <span data-ttu-id="7e438-240">La principale différence est qu’il a le *&quot;connecté par&quot;* attribut mis à jour avec le nom de cette nouvelle classe pour identifier le filtre de qui inscrit le journal.</span><span class="sxs-lookup"><span data-stu-id="7e438-240">The main difference is that it has the *&quot;Logged By&quot;* attribute updated with this new class' name to identify wich filter registered the log.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a><span data-ttu-id="7e438-241">Tâche 2 : Injectant un intercepteur de Code de la classe StoreController</span><span class="sxs-lookup"><span data-stu-id="7e438-241">Task 2: Injecting a new Code Interceptor into the StoreController Class</span></span>

<span data-ttu-id="7e438-242">Dans cette tâche, vous ajoutez un nouveau filtre personnalisé dans la classe StoreController et exécuter la solution pour vérifier la façon dont les deux filtres fonctionnent ensemble.</span><span class="sxs-lookup"><span data-stu-id="7e438-242">In this task, you will add a new custom filter into the StoreController Class and run the solution to verify how both filters work together.</span></span>

1. <span data-ttu-id="7e438-243">Ouvrez le **StoreController** classe situé dans **MvcMusicStore\Controllers** et injecter le nouveau filtre personnalisé **MyNewCustomActionFilter** dans  **StoreController** classe comme est indiqué dans le code suivant.</span><span class="sxs-lookup"><span data-stu-id="7e438-243">Open the **StoreController** class located at **MvcMusicStore\Controllers** and inject the new custom filter **MyNewCustomActionFilter** into **StoreController** class like is shown in the following code.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. <span data-ttu-id="7e438-244">Maintenant, exécutez l’application afin de voir le fonctionnement de ces deux filtres d’Action personnalisée.</span><span class="sxs-lookup"><span data-stu-id="7e438-244">Now, run the application in order to see how these two Custom Action Filters work.</span></span> <span data-ttu-id="7e438-245">Pour ce faire, appuyez sur **F5** et attendez que l’application démarre.</span><span class="sxs-lookup"><span data-stu-id="7e438-245">To do this, press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="7e438-246">Accédez à **/ActionLog** pour afficher l’état initial d’affichage journal.</span><span class="sxs-lookup"><span data-stu-id="7e438-246">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="7e438-247">![État de mise hors tension avant l’activité de la page du journal](aspnet-mvc-4-custom-action-filters/_static/image5.png "état mise hors tension avant l’activité de la page du journal")</span><span class="sxs-lookup"><span data-stu-id="7e438-247">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image5.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="7e438-248">*État de suivi du journal avant l’activité de la page*</span><span class="sxs-lookup"><span data-stu-id="7e438-248">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="7e438-249">Cliquez sur une de le **Genres** à partir du menu et d’effectuer certaines actions, telles que la navigation d’un album disponible.</span><span class="sxs-lookup"><span data-stu-id="7e438-249">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="7e438-250">Vérifiez que cette fois ; vos visites ont été suivies à deux reprises : une fois pour chacun des filtres d’Action personnalisée que vous avez ajouté dans le **StorageController** classe.</span><span class="sxs-lookup"><span data-stu-id="7e438-250">Check that this time; your visits were tracked twice: once for each of the Custom Action Filters you added in the **StorageController** class.</span></span>

    <span data-ttu-id="7e438-251">![Journal des actions avec l’activité connectée](aspnet-mvc-4-custom-action-filters/_static/image6.png "journal des actions avec l’activité connectée")</span><span class="sxs-lookup"><span data-stu-id="7e438-251">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image6.png "Action log with activity logged")</span></span>

    <span data-ttu-id="7e438-252">*Journal des actions avec l’activité connectée*</span><span class="sxs-lookup"><span data-stu-id="7e438-252">*Action log with activity logged*</span></span>
6. <span data-ttu-id="7e438-253">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="7e438-253">Close the browser.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a><span data-ttu-id="7e438-254">Tâche 3 : La gestion de l’ordre de filtre</span><span class="sxs-lookup"><span data-stu-id="7e438-254">Task 3: Managing Filter Ordering</span></span>

<span data-ttu-id="7e438-255">Dans cette tâche, vous allez apprendre à gérer l’ordre d’exécution des filtres à l’aide de la propriété d’ordre.</span><span class="sxs-lookup"><span data-stu-id="7e438-255">In this task, you will learn how to manage the filters' execution order by using the Order propery.</span></span>

1. <span data-ttu-id="7e438-256">Ouvrez le **StoreController** classe situé dans **MvcMusicStore\Controllers** et spécifiez le **ordre** comme propriété dans les deux filtres illustré ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="7e438-256">Open the **StoreController** class located at **MvcMusicStore\Controllers** and specify the **Order** property in both filters like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. <span data-ttu-id="7e438-257">Maintenant, vérifiez comment les filtres sont exécutés en fonction de la valeur de la propriété de son ordre.</span><span class="sxs-lookup"><span data-stu-id="7e438-257">Now, verify how the filters are executed depending on its Order property's value.</span></span> <span data-ttu-id="7e438-258">Vous constaterez que le filtre avec la valeur d’ordre plus petite (**CustomActionFilter**) est la première qui est exécutée.</span><span class="sxs-lookup"><span data-stu-id="7e438-258">You will find that the filter with the smallest Order value (**CustomActionFilter**) is the first one that is executed.</span></span> <span data-ttu-id="7e438-259">Appuyez sur **F5** et attendez que l’application démarre.</span><span class="sxs-lookup"><span data-stu-id="7e438-259">Press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="7e438-260">Accédez à **/ActionLog** pour afficher l’état initial d’affichage journal.</span><span class="sxs-lookup"><span data-stu-id="7e438-260">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="7e438-261">![État de mise hors tension avant l’activité de la page du journal](aspnet-mvc-4-custom-action-filters/_static/image7.png "état mise hors tension avant l’activité de la page du journal")</span><span class="sxs-lookup"><span data-stu-id="7e438-261">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image7.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="7e438-262">*État de suivi du journal avant l’activité de la page*</span><span class="sxs-lookup"><span data-stu-id="7e438-262">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="7e438-263">Cliquez sur une de le **Genres** à partir du menu et d’effectuer certaines actions, telles que la navigation d’un album disponible.</span><span class="sxs-lookup"><span data-stu-id="7e438-263">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="7e438-264">Vérifiez que cette fois, vos visites ont été suivies classés par valeur d’ordre des filtres : **CustomActionFilter** journaux premier.</span><span class="sxs-lookup"><span data-stu-id="7e438-264">Check that this time, your visits were tracked ordered by the filters' Order value: **CustomActionFilter** logs' first.</span></span>

    <span data-ttu-id="7e438-265">![Journal des actions avec l’activité connectée](aspnet-mvc-4-custom-action-filters/_static/image8.png "journal des actions avec l’activité connectée")</span><span class="sxs-lookup"><span data-stu-id="7e438-265">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image8.png "Action log with activity logged")</span></span>

    <span data-ttu-id="7e438-266">*Journal des actions avec l’activité connectée*</span><span class="sxs-lookup"><span data-stu-id="7e438-266">*Action log with activity logged*</span></span>
6. <span data-ttu-id="7e438-267">Maintenant, vous mettez à jour la valeur d’ordre des filtres et vérifier la façon dont l’ordre de journalisation change.</span><span class="sxs-lookup"><span data-stu-id="7e438-267">Now, you will update the Filters' order value and verify how the logging order changes.</span></span> <span data-ttu-id="7e438-268">Dans le **StoreController** class, mettre à jour de valeur d’ordre des filtres comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="7e438-268">In the **StoreController** class, update the Filters' Order value like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. <span data-ttu-id="7e438-269">Réexécutez l’application en appuyant sur **F5**.</span><span class="sxs-lookup"><span data-stu-id="7e438-269">Run the application again by pressing **F5**.</span></span>
8. <span data-ttu-id="7e438-270">Cliquez sur une de le **Genres** à partir du menu et d’effectuer certaines actions, telles que la navigation d’un album disponible.</span><span class="sxs-lookup"><span data-stu-id="7e438-270">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
9. <span data-ttu-id="7e438-271">Vérifiez que cette fois, les journaux créés par **MyNewCustomActionFilter** filtre apparaît en premier.</span><span class="sxs-lookup"><span data-stu-id="7e438-271">Check that this time, the logs created by **MyNewCustomActionFilter** filter appears first.</span></span>

    <span data-ttu-id="7e438-272">![Journal des actions avec l’activité connectée](aspnet-mvc-4-custom-action-filters/_static/image9.png "journal des actions avec l’activité connectée")</span><span class="sxs-lookup"><span data-stu-id="7e438-272">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image9.png "Action log with activity logged")</span></span>

    <span data-ttu-id="7e438-273">*Journal des actions avec l’activité connectée*</span><span class="sxs-lookup"><span data-stu-id="7e438-273">*Action log with activity logged*</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a><span data-ttu-id="7e438-274">Tâche 4 : L’inscription des filtres dans le monde entier</span><span class="sxs-lookup"><span data-stu-id="7e438-274">Task 4: Registering Filters Globally</span></span>

<span data-ttu-id="7e438-275">Dans cette tâche, vous mettrez à jour la solution pour inscrire le nouveau filtre (**MyNewCustomActionFilter**) comme un filtre global.</span><span class="sxs-lookup"><span data-stu-id="7e438-275">In this task, you will update the solution to register the new filter (**MyNewCustomActionFilter**) as a global filter.</span></span> <span data-ttu-id="7e438-276">Ce faisant, il est déclenché par tous les effectuées d’actions dans l’application et pas seulement en StoreController celles comme dans la tâche précédente.</span><span class="sxs-lookup"><span data-stu-id="7e438-276">By doing this, it will be triggered by all the actions perfomed in the application and not only in the StoreController ones as in the previous task.</span></span>

1. <span data-ttu-id="7e438-277">Dans **StoreController** classe, supprimez **[MyNewCustomActionFilter]** attribut et la propriété d’ordre à partir de **[CustomActionFilter]**.</span><span class="sxs-lookup"><span data-stu-id="7e438-277">In **StoreController** class, remove **[MyNewCustomActionFilter]** attribute and the order property from **[CustomActionFilter]**.</span></span> <span data-ttu-id="7e438-278">Il doit ressembler à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="7e438-278">It should look like the following:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. <span data-ttu-id="7e438-279">Ouvrez **Global.asax** de fichiers et recherchez le **Application\_Démarrer** (méthode).</span><span class="sxs-lookup"><span data-stu-id="7e438-279">Open **Global.asax** file and locate the **Application\_Start** method.</span></span> <span data-ttu-id="7e438-280">Notez que chaque fois que l’application démarre, elle inscrit les filtres globaux en appelant **RegisterGlobalFilters** méthode au sein de **FilterConfig** classe.</span><span class="sxs-lookup"><span data-stu-id="7e438-280">Notice that each time the application starts it is registering the global filters by calling **RegisterGlobalFilters** method within **FilterConfig** class.</span></span>

    <span data-ttu-id="7e438-281">![Enregistrement de filtres globaux dans Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "l’enregistrement de filtres globaux dans Global.asax")</span><span class="sxs-lookup"><span data-stu-id="7e438-281">![Registering Global Filters in Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "Registering Global Filters in Global.asax")</span></span>

    <span data-ttu-id="7e438-282">*Enregistrement de filtres globaux dans Global.asax*</span><span class="sxs-lookup"><span data-stu-id="7e438-282">*Registering Global Filters in Global.asax*</span></span>
3. <span data-ttu-id="7e438-283">Ouvrez **FilterConfig.cs** au sein du fichier **application\_Démarrer** dossier.</span><span class="sxs-lookup"><span data-stu-id="7e438-283">Open **FilterConfig.cs** file within **App\_Start** folder.</span></span>
4. <span data-ttu-id="7e438-284">Ajoutez une référence à l’aide de System.Web.Mvc ; à l’aide de MvcMusicStore.Filters ; espace de noms.</span><span class="sxs-lookup"><span data-stu-id="7e438-284">Add a reference to using System.Web.Mvc; using MvcMusicStore.Filters; namespace.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. <span data-ttu-id="7e438-285">Mise à jour **RegisterGlobalFilters** méthode en ajoutant votre filtre personnalisé.</span><span class="sxs-lookup"><span data-stu-id="7e438-285">Update **RegisterGlobalFilters** method adding your custom filter.</span></span> <span data-ttu-id="7e438-286">Pour ce faire, ajoutez le code en surbrillance :</span><span class="sxs-lookup"><span data-stu-id="7e438-286">To do this, add the highlighted code:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. <span data-ttu-id="7e438-287">Exécutez l’application en appuyant sur **F5**.</span><span class="sxs-lookup"><span data-stu-id="7e438-287">Run the application by pressing **F5**.</span></span>
7. <span data-ttu-id="7e438-288">Cliquez sur une de le **Genres** à partir du menu et d’effectuer certaines actions, telles que la navigation d’un album disponible.</span><span class="sxs-lookup"><span data-stu-id="7e438-288">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
8. <span data-ttu-id="7e438-289">Vérification qu’à présent **[MyNewCustomActionFilter]** est injectées dans le HomeController et ActionLogController trop.</span><span class="sxs-lookup"><span data-stu-id="7e438-289">Check that now **[MyNewCustomActionFilter]** is being injected in HomeController and ActionLogController too.</span></span>

    <span data-ttu-id="7e438-290">![Journal des actions avec l’activité connectée](aspnet-mvc-4-custom-action-filters/_static/image11.png "journal des actions avec l’activité connectée")</span><span class="sxs-lookup"><span data-stu-id="7e438-290">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image11.png "Action log with activity logged")</span></span>

    <span data-ttu-id="7e438-291">*Journal des actions avec l’activité globale connectée*</span><span class="sxs-lookup"><span data-stu-id="7e438-291">*Action log with global activity logged*</span></span>

> [!NOTE]
> <span data-ttu-id="7e438-292">En outre, vous pouvez déployer cette application à Sites Web Windows Azure suit [publication annexe b : une Application ASP.NET MVC 4, à l’aide de Web Deploy](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="7e438-292">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="7e438-293">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="7e438-293">Summary</span></span>

<span data-ttu-id="7e438-294">En fin de cet atelier pratique, vous avez appris à étendre un filtre d’action pour exécuter des actions personnalisées.</span><span class="sxs-lookup"><span data-stu-id="7e438-294">By completing this Hands-On Lab you have learned how to extend an action filter to execute custom actions.</span></span> <span data-ttu-id="7e438-295">Vous avez également appris à injecter les filtres à vos contrôleurs de page.</span><span class="sxs-lookup"><span data-stu-id="7e438-295">You have also learned how to inject any filter to your page controllers.</span></span> <span data-ttu-id="7e438-296">Les concepts suivants ont été utilisés :</span><span class="sxs-lookup"><span data-stu-id="7e438-296">The following concepts were used:</span></span>

- <span data-ttu-id="7e438-297">Comment créer des filtres d’Action personnalisée avec la classe ActionFilterAttribute de MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7e438-297">How to create Custom Action filters with the ASP.NET MVC ActionFilterAttribute class</span></span>
- <span data-ttu-id="7e438-298">Comment injecter des filtres dans les contrôleurs MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7e438-298">How to inject filters into ASP.NET MVC controllers</span></span>
- <span data-ttu-id="7e438-299">Comment gérer le filtre classement à l’aide de la propriété Order</span><span class="sxs-lookup"><span data-stu-id="7e438-299">How to manage filter ordering using the Order property</span></span>
- <span data-ttu-id="7e438-300">Comment inscrire des filtres dans le monde entier</span><span class="sxs-lookup"><span data-stu-id="7e438-300">How to register filters globally</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="7e438-301">Annexe a : installation de Visual Studio Express 2012 pour le Web</span><span class="sxs-lookup"><span data-stu-id="7e438-301">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="7e438-302">Vous pouvez installer **Microsoft Visual Studio Express 2012 pour Web** ou un autre &quot;Express&quot; à l’aide de la version du **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="7e438-302">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="7e438-303">Les instructions suivantes vous guident dans les étapes requises pour installer *Visual studio Express 2012 pour Web* à l’aide de *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="7e438-303">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="7e438-304">Accédez à [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="7e438-304">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="7e438-305">Si vous avez déjà installé Web Platform Installer, vous pouvez également ouvrir il et recherchez le produit &quot; <em>Visual Studio Express 2012 pour le Web avec Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="7e438-305">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="7e438-306">Cliquez sur **installer maintenant**.</span><span class="sxs-lookup"><span data-stu-id="7e438-306">Click on **Install Now**.</span></span> <span data-ttu-id="7e438-307">Si vous n’avez pas **Web Platform Installer** vous allez être redirigé pour télécharger et installer en premier.</span><span class="sxs-lookup"><span data-stu-id="7e438-307">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="7e438-308">Une fois **Web Platform Installer** est ouvert, cliquez sur **installer** pour démarrer le programme d’installation.</span><span class="sxs-lookup"><span data-stu-id="7e438-308">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="7e438-309">![Installer Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "installer Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="7e438-309">![Install Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="7e438-310">*Installer Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="7e438-310">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="7e438-311">Lisez les licences et les termes du contrat de tous les produits et cliquez sur **J’accepte** pour continuer.</span><span class="sxs-lookup"><span data-stu-id="7e438-311">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Accepter les termes du contrat de licence](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    <span data-ttu-id="7e438-313">*Accepter les termes du contrat de licence*</span><span class="sxs-lookup"><span data-stu-id="7e438-313">*Accepting the license terms*</span></span>
5. <span data-ttu-id="7e438-314">Attendez que le processus de téléchargement et l’installation se termine.</span><span class="sxs-lookup"><span data-stu-id="7e438-314">Wait until the downloading and installation process completes.</span></span>

    ![Progression de l'installation](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    <span data-ttu-id="7e438-316">*Progression de l’installation*</span><span class="sxs-lookup"><span data-stu-id="7e438-316">*Installation progress*</span></span>
6. <span data-ttu-id="7e438-317">Une fois l’installation terminée, cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="7e438-317">When the installation completes, click **Finish**.</span></span>

    ![Installation est terminée](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    <span data-ttu-id="7e438-319">*Installation est terminée*</span><span class="sxs-lookup"><span data-stu-id="7e438-319">*Installation completed*</span></span>
7. <span data-ttu-id="7e438-320">Cliquez sur **Exit** pour fermer Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="7e438-320">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="7e438-321">Pour ouvrir Visual Studio Express pour le Web, accédez à la **Démarrer** écran et démarrer l’écriture &quot; **VS Express**&quot;, puis cliquez sur le **Visual Studio Express pour le Web** vignette.</span><span class="sxs-lookup"><span data-stu-id="7e438-321">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express pour une vignette de Web](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    <span data-ttu-id="7e438-323">*VS Express pour une vignette de Web*</span><span class="sxs-lookup"><span data-stu-id="7e438-323">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="7e438-324">Annexe b : publication d’une Application ASP.NET MVC 4, à l’aide de Web Deploy</span><span class="sxs-lookup"><span data-stu-id="7e438-324">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="7e438-325">Cette annexe sera vous montrent comment créer un nouveau site web à partir du portail de gestion Windows Azure et publiez l’application que vous avez obtenu en suivant le laboratoire, en tirant parti de la fonctionnalité de publication Web Deploy fournie par Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="7e438-325">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="7e438-326">Tâche 1 : création d’un nouveau Site Web à partir de Windows Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7e438-326">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="7e438-327">Accédez à la [portail de gestion Windows Azure](https://manage.windowsazure.com/) et connectez-vous en utilisant les informations d’identification Microsoft associées à votre abonnement.</span><span class="sxs-lookup"><span data-stu-id="7e438-327">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7e438-328">Avec Windows Azure, vous pouvez héberger 10 Sites Web ASP.NET gratuitement et faites ensuite évoluer que votre trafic augmente.</span><span class="sxs-lookup"><span data-stu-id="7e438-328">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="7e438-329">Vous pouvez vous inscrire [ici](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="7e438-329">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="7e438-330">![Ouvrez une session sur le portail Windows Azure](aspnet-mvc-4-custom-action-filters/_static/image17.png "ouvrez une session sur le portail Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="7e438-330">![Log on to Windows Azure portal](aspnet-mvc-4-custom-action-filters/_static/image17.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="7e438-331">*Ouvrez une session sur le portail de gestion Azure Windows*</span><span class="sxs-lookup"><span data-stu-id="7e438-331">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="7e438-332">Cliquez sur **New** sur la barre de commandes.</span><span class="sxs-lookup"><span data-stu-id="7e438-332">Click **New** on the command bar.</span></span>

    <span data-ttu-id="7e438-333">![Création d’un Site Web](aspnet-mvc-4-custom-action-filters/_static/image18.png "création d’un Site Web")</span><span class="sxs-lookup"><span data-stu-id="7e438-333">![Creating a new Web Site](aspnet-mvc-4-custom-action-filters/_static/image18.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="7e438-334">*Création d’un Site Web*</span><span class="sxs-lookup"><span data-stu-id="7e438-334">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="7e438-335">Cliquez sur **calcul** | **Site Web**.</span><span class="sxs-lookup"><span data-stu-id="7e438-335">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="7e438-336">Puis sélectionnez **création rapide** option.</span><span class="sxs-lookup"><span data-stu-id="7e438-336">Then select **Quick Create** option.</span></span> <span data-ttu-id="7e438-337">Fournir une URL disponible pour le nouveau site web et cliquez sur **créer un Site Web**.</span><span class="sxs-lookup"><span data-stu-id="7e438-337">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7e438-338">Un Site Web Windows Azure est l’hôte pour une application web en cours d’exécution dans le cloud que vous pouvez contrôler et gérer.</span><span class="sxs-lookup"><span data-stu-id="7e438-338">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="7e438-339">L’option Création rapide vous permet de déployer une application web terminée pour le Site Web Windows Azure à partir en dehors du portail.</span><span class="sxs-lookup"><span data-stu-id="7e438-339">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="7e438-340">Il n’inclut pas les étapes de configuration d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="7e438-340">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="7e438-341">![Création d’un Site Web à l’aide de la création rapide](aspnet-mvc-4-custom-action-filters/_static/image19.png "création d’un Site Web à l’aide de la création rapide")</span><span class="sxs-lookup"><span data-stu-id="7e438-341">![Creating a new Web Site using Quick Create](aspnet-mvc-4-custom-action-filters/_static/image19.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="7e438-342">*Création d’un Site Web à l’aide de la création rapide*</span><span class="sxs-lookup"><span data-stu-id="7e438-342">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="7e438-343">Attendez que la nouvelle **Site Web** est créé.</span><span class="sxs-lookup"><span data-stu-id="7e438-343">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="7e438-344">Une fois que le Site Web est créé, cliquez sur le lien situé sous le **URL** colonne.</span><span class="sxs-lookup"><span data-stu-id="7e438-344">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="7e438-345">Vérifiez que le nouveau Site Web fonctionne.</span><span class="sxs-lookup"><span data-stu-id="7e438-345">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="7e438-346">![Navigation vers le nouveau site web](aspnet-mvc-4-custom-action-filters/_static/image20.png "exploration vers le nouveau site web")</span><span class="sxs-lookup"><span data-stu-id="7e438-346">![Browsing to the new web site](aspnet-mvc-4-custom-action-filters/_static/image20.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="7e438-347">*Navigation vers le nouveau site web*</span><span class="sxs-lookup"><span data-stu-id="7e438-347">*Browsing to the new web site*</span></span>

    <span data-ttu-id="7e438-348">![Site Web en cours d’exécution](aspnet-mvc-4-custom-action-filters/_static/image21.png "site Web en cours d’exécution")</span><span class="sxs-lookup"><span data-stu-id="7e438-348">![Web site running](aspnet-mvc-4-custom-action-filters/_static/image21.png "Web site running")</span></span>

    <span data-ttu-id="7e438-349">*Site Web en cours d’exécution*</span><span class="sxs-lookup"><span data-stu-id="7e438-349">*Web site running*</span></span>
6. <span data-ttu-id="7e438-350">Revenez au portail et cliquez sur le nom du site web sous le **nom** colonne pour afficher les pages de gestion.</span><span class="sxs-lookup"><span data-stu-id="7e438-350">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="7e438-351">![Ouvrir les pages de gestion de site web](aspnet-mvc-4-custom-action-filters/_static/image22.png "ouvrir les pages de gestion de site web")</span><span class="sxs-lookup"><span data-stu-id="7e438-351">![Opening the web site management pages](aspnet-mvc-4-custom-action-filters/_static/image22.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="7e438-352">*Ouvrir les pages de gestion de Site Web*</span><span class="sxs-lookup"><span data-stu-id="7e438-352">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="7e438-353">Dans le **tableau de bord** page, sous la **aperçu rapide** , cliquez sur le **télécharger le profil de publication** lien.</span><span class="sxs-lookup"><span data-stu-id="7e438-353">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7e438-354">Le *le profil de publication* contient toutes les informations requises pour publier une application web à un site Web Windows Azure pour chaque méthode de publication est activée.</span><span class="sxs-lookup"><span data-stu-id="7e438-354">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="7e438-355">Le profil de publication contient les URL, les informations d’identification utilisateur et les chaînes de base de données requis pour la connexion et l’authentification par rapport à chacun des points de terminaison pour lequel une méthode de publication est activée.</span><span class="sxs-lookup"><span data-stu-id="7e438-355">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="7e438-356">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express pour Web** et **Microsoft Visual Studio 2012** prise en charge de la lecture des profils pour automatiser la configuration de ces programmes pour de publication publication d’applications web sur sites Web Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="7e438-356">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="7e438-357">![Téléchargement du site web le profil de publication](aspnet-mvc-4-custom-action-filters/_static/image23.png "téléchargement du site web le profil de publication")</span><span class="sxs-lookup"><span data-stu-id="7e438-357">![Downloading the web site publish profile](aspnet-mvc-4-custom-action-filters/_static/image23.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="7e438-358">*Téléchargement du Site Web le profil de publication*</span><span class="sxs-lookup"><span data-stu-id="7e438-358">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="7e438-359">Télécharger le fichier de profil de publication dans un emplacement connu.</span><span class="sxs-lookup"><span data-stu-id="7e438-359">Download the publish profile file to a known location.</span></span> <span data-ttu-id="7e438-360">Davantage dans cet exercice, vous verrez comment utiliser ce fichier pour publier une application web à un Sites Web Windows Azure à partir de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7e438-360">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="7e438-361">![L’enregistrement du fichier de profil de publication](aspnet-mvc-4-custom-action-filters/_static/image24.png "l’enregistrement du profil de publication")</span><span class="sxs-lookup"><span data-stu-id="7e438-361">![Saving the publish profile file](aspnet-mvc-4-custom-action-filters/_static/image24.png "Saving the publish profile")</span></span>

    <span data-ttu-id="7e438-362">*L’enregistrement du fichier de profil de publication*</span><span class="sxs-lookup"><span data-stu-id="7e438-362">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="7e438-363">Tâche 2 : configuration du serveur de base de données</span><span class="sxs-lookup"><span data-stu-id="7e438-363">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="7e438-364">Si votre application se sert de SQL Server vous devez créer un serveur de base de données SQL des bases de données.</span><span class="sxs-lookup"><span data-stu-id="7e438-364">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="7e438-365">Si vous souhaitez déployer une application simple qui n’utilise pas de SQL Server, vous pouvez ignorer cette tâche.</span><span class="sxs-lookup"><span data-stu-id="7e438-365">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="7e438-366">Vous devez un serveur de base de données SQL pour stocker la base de données d’application.</span><span class="sxs-lookup"><span data-stu-id="7e438-366">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="7e438-367">Vous pouvez afficher les serveurs de base de données SQL à partir de votre abonnement dans le portail de gestion Windows Azure à l’adresse **bases de données Sql** | **serveurs** | **du serveur Tableau de bord**.</span><span class="sxs-lookup"><span data-stu-id="7e438-367">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="7e438-368">Si vous n’avez pas un serveur créé, vous pouvez créer un à l’aide du **ajouter** bouton sur la barre de commandes.</span><span class="sxs-lookup"><span data-stu-id="7e438-368">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="7e438-369">Prenez note de la **nom du serveur et les URL, nom de connexion d’administrateur et mot de passe**, car vous les utiliserez dans les tâches suivantes.</span><span class="sxs-lookup"><span data-stu-id="7e438-369">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="7e438-370">Ne créez pas encore, la base de données tel qu’il sera créé dans une étape ultérieure.</span><span class="sxs-lookup"><span data-stu-id="7e438-370">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="7e438-371">![Tableau de bord de serveur SQL Database](aspnet-mvc-4-custom-action-filters/_static/image25.png "tableau de bord serveur de base de données SQL")</span><span class="sxs-lookup"><span data-stu-id="7e438-371">![SQL Database Server Dashboard](aspnet-mvc-4-custom-action-filters/_static/image25.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="7e438-372">*Tableau de bord serveur de base de données SQL*</span><span class="sxs-lookup"><span data-stu-id="7e438-372">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="7e438-373">Dans la tâche suivante, vous allez tester la connexion de base de données à partir de Visual Studio, pour cette raison, vous devez inclure votre adresse IP locale dans la liste du serveur de **adresses IP autorisées**.</span><span class="sxs-lookup"><span data-stu-id="7e438-373">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="7e438-374">Pour ce faire, cliquez sur **configurer**, sélectionnez l’adresse IP à partir de **adresse IP cliente actuelle** et collez-le sur le **adresse IP de début** et **adresse IP de fin** zones de texte et cliquez sur le ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) bouton.</span><span class="sxs-lookup"><span data-stu-id="7e438-374">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) button.</span></span>

    ![Ajout d’adresse IP du Client](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    <span data-ttu-id="7e438-376">*Ajout d’adresse IP du Client*</span><span class="sxs-lookup"><span data-stu-id="7e438-376">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="7e438-377">Une fois le **adresse IP du Client** est ajouté à le des adresses IP autorisées de liste, cliquez sur **enregistrer** pour confirmer les modifications.</span><span class="sxs-lookup"><span data-stu-id="7e438-377">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confirmer les modifications](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    <span data-ttu-id="7e438-379">*Confirmer les modifications*</span><span class="sxs-lookup"><span data-stu-id="7e438-379">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="7e438-380">Tâche 3 : publication d’une Application ASP.NET MVC 4, à l’aide de Web Deploy</span><span class="sxs-lookup"><span data-stu-id="7e438-380">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="7e438-381">Revenez à la solution ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="7e438-381">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="7e438-382">Dans le **l’Explorateur de solutions**, cliquez sur le projet de site web et sélectionnez **publier**.</span><span class="sxs-lookup"><span data-stu-id="7e438-382">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="7e438-383">![Publication de l’Application](aspnet-mvc-4-custom-action-filters/_static/image29.png "publication de l’Application")</span><span class="sxs-lookup"><span data-stu-id="7e438-383">![Publishing the Application](aspnet-mvc-4-custom-action-filters/_static/image29.png "Publishing the Application")</span></span>

    <span data-ttu-id="7e438-384">*Publier le site web*</span><span class="sxs-lookup"><span data-stu-id="7e438-384">*Publishing the web site*</span></span>
2. <span data-ttu-id="7e438-385">Importer le profil de publication que vous avez enregistré dans la première tâche.</span><span class="sxs-lookup"><span data-stu-id="7e438-385">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="7e438-386">![L’importation du profil de publication](aspnet-mvc-4-custom-action-filters/_static/image30.png "l’importation du profil de publication")</span><span class="sxs-lookup"><span data-stu-id="7e438-386">![Importing the publish profile](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importing the publish profile")</span></span>

    <span data-ttu-id="7e438-387">*Importation du profil de publication*</span><span class="sxs-lookup"><span data-stu-id="7e438-387">*Importing publish profile*</span></span>
3. <span data-ttu-id="7e438-388">Cliquez sur **valider la connexion**.</span><span class="sxs-lookup"><span data-stu-id="7e438-388">Click **Validate Connection**.</span></span> <span data-ttu-id="7e438-389">Une fois la Validation terminée. Cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="7e438-389">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7e438-390">La validation est terminée une fois que vous voyez une coche verte apparaît en regard du bouton Valider la connexion.</span><span class="sxs-lookup"><span data-stu-id="7e438-390">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="7e438-391">![Validation de la connexion](aspnet-mvc-4-custom-action-filters/_static/image31.png "validation de la connexion")</span><span class="sxs-lookup"><span data-stu-id="7e438-391">![Validating connection](aspnet-mvc-4-custom-action-filters/_static/image31.png "Validating connection")</span></span>

    <span data-ttu-id="7e438-392">*Validation de la connexion*</span><span class="sxs-lookup"><span data-stu-id="7e438-392">*Validating connection*</span></span>
4. <span data-ttu-id="7e438-393">Dans le **paramètres** page, sous la **bases de données** , cliquez sur le bouton en regard de la zone de texte de la connexion de votre base de données (par exemple, **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="7e438-393">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="7e438-394">![Configuration de déploiement Web](aspnet-mvc-4-custom-action-filters/_static/image32.png "configuration de déploiement Web")</span><span class="sxs-lookup"><span data-stu-id="7e438-394">![Web deploy configuration](aspnet-mvc-4-custom-action-filters/_static/image32.png "Web deploy configuration")</span></span>

    <span data-ttu-id="7e438-395">*Configuration de déploiement Web*</span><span class="sxs-lookup"><span data-stu-id="7e438-395">*Web deploy configuration*</span></span>
5. <span data-ttu-id="7e438-396">Configurez la connexion de base de données comme suit :</span><span class="sxs-lookup"><span data-stu-id="7e438-396">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="7e438-397">Dans le **nom du serveur** tapez votre URL de base de données SQL server à l’aide du *tcp :* préfixe.</span><span class="sxs-lookup"><span data-stu-id="7e438-397">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="7e438-398">Dans **nom d’utilisateur** tapez votre nom de connexion d’administrateur serveur.</span><span class="sxs-lookup"><span data-stu-id="7e438-398">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="7e438-399">Dans **mot de passe** tapez votre mot de passe du compte de connexion administrateur serveur.</span><span class="sxs-lookup"><span data-stu-id="7e438-399">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="7e438-400">Tapez un nouveau nom de base de données.</span><span class="sxs-lookup"><span data-stu-id="7e438-400">Type a new database name.</span></span>

     <span data-ttu-id="7e438-401">![Configuration de chaîne de connexion de destination](aspnet-mvc-4-custom-action-filters/_static/image33.png "configuration de chaîne de connexion de destination")</span><span class="sxs-lookup"><span data-stu-id="7e438-401">![Configuring destination connection string](aspnet-mvc-4-custom-action-filters/_static/image33.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="7e438-402">*Configuration de chaîne de connexion de destination*</span><span class="sxs-lookup"><span data-stu-id="7e438-402">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="7e438-403">Cliquez ensuite sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="7e438-403">Then click **OK**.</span></span> <span data-ttu-id="7e438-404">Lorsque vous êtes invité à créer la base de données, cliquez sur **Oui**.</span><span class="sxs-lookup"><span data-stu-id="7e438-404">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="7e438-405">![Création de la base de données](aspnet-mvc-4-custom-action-filters/_static/image34.png "création de la chaîne de la base de données")</span><span class="sxs-lookup"><span data-stu-id="7e438-405">![Creating the database](aspnet-mvc-4-custom-action-filters/_static/image34.png "Creating the database string")</span></span>

    <span data-ttu-id="7e438-406">*Création de la base de données*</span><span class="sxs-lookup"><span data-stu-id="7e438-406">*Creating the database*</span></span>
7. <span data-ttu-id="7e438-407">La chaîne de connexion que vous utiliserez pour vous connecter à la base de données SQL dans Windows Azure est indiquée dans la zone de texte de la connexion par défaut.</span><span class="sxs-lookup"><span data-stu-id="7e438-407">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="7e438-408">Cliquez ensuite sur **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="7e438-408">Then click **Next**.</span></span>

    <span data-ttu-id="7e438-409">![Chaîne de connexion pointant vers la base de données SQL](aspnet-mvc-4-custom-action-filters/_static/image35.png "chaîne de connexion pointant vers la base de données SQL")</span><span class="sxs-lookup"><span data-stu-id="7e438-409">![Connection string pointing to SQL Database](aspnet-mvc-4-custom-action-filters/_static/image35.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="7e438-410">*Chaîne de connexion pointant vers la base de données SQL*</span><span class="sxs-lookup"><span data-stu-id="7e438-410">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="7e438-411">Dans le **aperçu** , cliquez sur **publier**.</span><span class="sxs-lookup"><span data-stu-id="7e438-411">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="7e438-412">![Publication de l’application web](aspnet-mvc-4-custom-action-filters/_static/image36.png "publication de l’application web")</span><span class="sxs-lookup"><span data-stu-id="7e438-412">![Publishing the web application](aspnet-mvc-4-custom-action-filters/_static/image36.png "Publishing the web application")</span></span>

    <span data-ttu-id="7e438-413">*Publication de l’application web*</span><span class="sxs-lookup"><span data-stu-id="7e438-413">*Publishing the web application*</span></span>
9. <span data-ttu-id="7e438-414">Une fois le processus de publication terminé, votre navigateur par défaut s’ouvre le site web publié.</span><span class="sxs-lookup"><span data-stu-id="7e438-414">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="7e438-415">Annexe c : l’utilisation des extraits de Code</span><span class="sxs-lookup"><span data-stu-id="7e438-415">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="7e438-416">Avec des extraits de code, vous avez tout le code que vous avez besoin à portée de main.</span><span class="sxs-lookup"><span data-stu-id="7e438-416">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="7e438-417">Le document de laboratoire vous indiquera exactement quand vous pouvez les utiliser, comme indiqué dans l’illustration suivante.</span><span class="sxs-lookup"><span data-stu-id="7e438-417">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="7e438-418">![À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet](aspnet-mvc-4-custom-action-filters/_static/image37.png "extraits de code à l’aide de Visual Studio pour insérer du code dans votre projet")</span><span class="sxs-lookup"><span data-stu-id="7e438-418">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-custom-action-filters/_static/image37.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="7e438-419">*À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet*</span><span class="sxs-lookup"><span data-stu-id="7e438-419">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="7e438-420">***Pour ajouter un extrait de code à l’aide du clavier (c# uniquement)***</span><span class="sxs-lookup"><span data-stu-id="7e438-420">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="7e438-421">Placez le curseur où vous souhaitez insérer le code.</span><span class="sxs-lookup"><span data-stu-id="7e438-421">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="7e438-422">Commencez à taper le nom de l’extrait de code (sans espaces ou des traits d’union).</span><span class="sxs-lookup"><span data-stu-id="7e438-422">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="7e438-423">Regarder en tant qu’IntelliSense affiche les noms des extraits correspondants.</span><span class="sxs-lookup"><span data-stu-id="7e438-423">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="7e438-424">Sélectionnez l’extrait de code correct (ou continuez à taper jusqu'à ce que le nom de l’extrait de code entière est sélectionnée).</span><span class="sxs-lookup"><span data-stu-id="7e438-424">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="7e438-425">Appuyez sur la touche Tab à deux reprises pour insérer l’extrait de code à l’emplacement du curseur.</span><span class="sxs-lookup"><span data-stu-id="7e438-425">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="7e438-426">![Commencez à taper le nom de l’extrait de code](aspnet-mvc-4-custom-action-filters/_static/image38.png "commencez à taper le nom de l’extrait de code")</span><span class="sxs-lookup"><span data-stu-id="7e438-426">![Start typing the snippet name](aspnet-mvc-4-custom-action-filters/_static/image38.png "Start typing the snippet name")</span></span>

<span data-ttu-id="7e438-427">*Commencez à taper le nom de l’extrait de code*</span><span class="sxs-lookup"><span data-stu-id="7e438-427">*Start typing the snippet name*</span></span>

<span data-ttu-id="7e438-428">![Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance](aspnet-mvc-4-custom-action-filters/_static/image39.png "appuyez sur Tab pour sélectionner l’extrait de code en surbrillance")</span><span class="sxs-lookup"><span data-stu-id="7e438-428">![Press Tab to select the highlighted snippet](aspnet-mvc-4-custom-action-filters/_static/image39.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="7e438-429">*Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance*</span><span class="sxs-lookup"><span data-stu-id="7e438-429">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="7e438-430">![Appuyez sur Tab à nouveau et l’extrait de code seront développe](aspnet-mvc-4-custom-action-filters/_static/image40.png "appuyez sur Tab à nouveau et l’extrait de code seront développe.")</span><span class="sxs-lookup"><span data-stu-id="7e438-430">![Press Tab again and the snippet will expand](aspnet-mvc-4-custom-action-filters/_static/image40.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="7e438-431">*Appuyez sur Tab à nouveau et l’extrait de code seront développe.*</span><span class="sxs-lookup"><span data-stu-id="7e438-431">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="7e438-432">***Pour ajouter un extrait de code à l’aide de la souris (c#, Visual Basic et XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="7e438-432">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="7e438-433">Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code.</span><span class="sxs-lookup"><span data-stu-id="7e438-433">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="7e438-434">Sélectionnez **insérer un extrait** suivie **mes extraits de Code**.</span><span class="sxs-lookup"><span data-stu-id="7e438-434">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="7e438-435">Choisissez l’extrait de code approprié dans la liste, en cliquant dessus.</span><span class="sxs-lookup"><span data-stu-id="7e438-435">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="7e438-436">![Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait](aspnet-mvc-4-custom-action-filters/_static/image41.png "avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait")</span><span class="sxs-lookup"><span data-stu-id="7e438-436">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-custom-action-filters/_static/image41.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="7e438-437">*Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait*</span><span class="sxs-lookup"><span data-stu-id="7e438-437">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="7e438-438">![Choisissez l’extrait de code approprié dans la liste, en cliquant dessus](aspnet-mvc-4-custom-action-filters/_static/image42.png "choisir l’extrait de code approprié dans la liste, en cliquant sur celle-ci")</span><span class="sxs-lookup"><span data-stu-id="7e438-438">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-custom-action-filters/_static/image42.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="7e438-439">*Choisissez l’extrait de code approprié dans la liste, en cliquant sur celle-ci*</span><span class="sxs-lookup"><span data-stu-id="7e438-439">*Pick the relevant snippet from the list, by clicking on it*</span></span>
