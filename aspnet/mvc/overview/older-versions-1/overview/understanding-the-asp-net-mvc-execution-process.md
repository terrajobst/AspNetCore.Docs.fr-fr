---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: Présentation du processus de l’exécution d’ASP.NET MVC | Microsoft Docs
author: microsoft
description: Découvrez comment l’infrastructure ASP.NET MVC traite une demande de navigateur pas à pas.
ms.author: riande
ms.date: 01/27/2009
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 3a3bcf4b78e3b19fb4d293f67b68c3a790d05221
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832572"
---
<a name="understanding-the-aspnet-mvc-execution-process"></a><span data-ttu-id="03621-103">Présentation du processus de l’exécution d’ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="03621-103">Understanding the ASP.NET MVC Execution Process</span></span>
====================
<span data-ttu-id="03621-104">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="03621-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="03621-105">Découvrez comment l’infrastructure ASP.NET MVC traite une demande de navigateur pas à pas.</span><span class="sxs-lookup"><span data-stu-id="03621-105">Learn how the ASP.NET MVC framework processes a browser request step-by-step.</span></span>


<span data-ttu-id="03621-106">Demandes à une application Web basée sur ASP.NET MVC traversent d’abord le **UrlRoutingModule** objet, qui est un module HTTP.</span><span class="sxs-lookup"><span data-stu-id="03621-106">Requests to an ASP.NET MVC-based Web application first pass through the **UrlRoutingModule** object, which is an HTTP module.</span></span> <span data-ttu-id="03621-107">Ce module analyse la demande et effectue une sélection de l’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="03621-107">This module parses the request and performs route selection.</span></span> <span data-ttu-id="03621-108">Le **UrlRoutingModule** objet sélectionne le premier objet d’itinéraire qui correspond à la requête actuelle.</span><span class="sxs-lookup"><span data-stu-id="03621-108">The **UrlRoutingModule** object selects the first route object that matches the current request.</span></span> <span data-ttu-id="03621-109">(Un objet d’itinéraire est une classe qui implémente **RouteBase**, et est généralement une instance de la **itinéraire** classe.) Si aucun itinéraire ne correspond, le **UrlRoutingModule** objet ne fait rien et la demande vous permet de revenir à la demande ASP.NET ou IIS normal traitement.</span><span class="sxs-lookup"><span data-stu-id="03621-109">(A route object is a class that implements **RouteBase**, and is typically an instance of the **Route** class.) If no routes match, the **UrlRoutingModule** object does nothing and lets the request fall back to the regular ASP.NET or IIS request processing.</span></span>

<span data-ttu-id="03621-110">À partir de l’élément sélectionné **itinéraire** objet, le **UrlRoutingModule** Obtient le **IRouteHandler** objet auquel est associé le **itinéraire**objet.</span><span class="sxs-lookup"><span data-stu-id="03621-110">From the selected **Route** object, the **UrlRoutingModule** object obtains the **IRouteHandler** object that is associated with the **Route** object.</span></span> <span data-ttu-id="03621-111">En règle générale, dans une application MVC, ce sera une instance de **MvcRouteHandler**.</span><span class="sxs-lookup"><span data-stu-id="03621-111">Typically, in an MVC application, this will be an instance of **MvcRouteHandler**.</span></span> <span data-ttu-id="03621-112">Le **IRouteHandler** instance crée un **IHttpHandler** objet et lui passe la **IHttpContext** objet.</span><span class="sxs-lookup"><span data-stu-id="03621-112">The **IRouteHandler** instance creates an **IHttpHandler** object and passes it the **IHttpContext** object.</span></span> <span data-ttu-id="03621-113">Par défaut, le **IHttpHandler** d’instance pour MVC est la **MvcHandler** objet.</span><span class="sxs-lookup"><span data-stu-id="03621-113">By default, the **IHttpHandler** instance for MVC is the **MvcHandler** object.</span></span> <span data-ttu-id="03621-114">Le **MvcHandler** objet sélectionne ensuite le contrôleur qui gérera finalement la demande.</span><span class="sxs-lookup"><span data-stu-id="03621-114">The **MvcHandler** object then selects the controller that will ultimately handle the request.</span></span>

> [!NOTE]
> <span data-ttu-id="03621-115">Quand une application Web ASP.NET MVC s’exécute dans IIS 7.0, aucune extension de nom de fichier n’est requise pour les projets MVC.</span><span class="sxs-lookup"><span data-stu-id="03621-115">When an ASP.NET MVC Web application runs in IIS 7.0, no file name extension is required for MVC projects.</span></span> <span data-ttu-id="03621-116">Toutefois, dans IIS 6.0, le gestionnaire requiert que vous mapper l’extension de nom de fichier .mvc à la DLL ISAPI ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="03621-116">However, in IIS 6.0, the handler requires that you map the .mvc file name extension to the ASP.NET ISAPI DLL.</span></span>


<span data-ttu-id="03621-117">Le module et le gestionnaire sont les points d’entrée à l’infrastructure ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="03621-117">The module and handler are the entry points to the ASP.NET MVC framework.</span></span> <span data-ttu-id="03621-118">Ils effectuent les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="03621-118">They perform the following actions:</span></span>

- <span data-ttu-id="03621-119">Sélection du contrôleur approprié dans une application Web MVC.</span><span class="sxs-lookup"><span data-stu-id="03621-119">Select the appropriate controller in an MVC Web application.</span></span>
- <span data-ttu-id="03621-120">Pour obtenir une instance de contrôleur spécifique.</span><span class="sxs-lookup"><span data-stu-id="03621-120">Obtain a specific controller instance.</span></span>
- <span data-ttu-id="03621-121">Appeler le contrôleur **Execute** (méthode).</span><span class="sxs-lookup"><span data-stu-id="03621-121">Call the controller's **Execute** method.</span></span>

<span data-ttu-id="03621-122">La liste suivante décrit les étapes d’exécution d’un projet Web MVC :</span><span class="sxs-lookup"><span data-stu-id="03621-122">The following lists the stages of execution for an MVC Web project:</span></span>

- <span data-ttu-id="03621-123">Première demande concernant l’application de réception</span><span class="sxs-lookup"><span data-stu-id="03621-123">Receive first request for the application</span></span> 

    - <span data-ttu-id="03621-124">Dans le fichier Global.asax, **itinéraire** objets sont ajoutés à la **RouteTable** objet.</span><span class="sxs-lookup"><span data-stu-id="03621-124">In the Global.asax file, **Route** objects are added to the **RouteTable** object.</span></span>
- <span data-ttu-id="03621-125">Effectuer un routage</span><span class="sxs-lookup"><span data-stu-id="03621-125">Perform routing</span></span> 

    - <span data-ttu-id="03621-126">Le **UrlRoutingModule** module utilise la première correspondance **itinéraire** de l’objet dans le **RouteTable** collection pour créer le **RouteData** objet, qu’il utilise ensuite pour créer un **RequestContext** (**IHttpContext**) objet.</span><span class="sxs-lookup"><span data-stu-id="03621-126">The **UrlRoutingModule** module uses the first matching **Route** object in the **RouteTable** collection to create the **RouteData** object, which it then uses to create a **RequestContext** (**IHttpContext**) object.</span></span>
- <span data-ttu-id="03621-127">Créer le Gestionnaire de demandes MVC</span><span class="sxs-lookup"><span data-stu-id="03621-127">Create MVC request handler</span></span> 

    - <span data-ttu-id="03621-128">Le **MvcRouteHandler** objet crée une instance de la **MvcHandler** classe et le transmet le **RequestContext** instance.</span><span class="sxs-lookup"><span data-stu-id="03621-128">The **MvcRouteHandler** object creates an instance of the **MvcHandler** class and passes it the **RequestContext** instance.</span></span>
- <span data-ttu-id="03621-129">Créer le contrôleur</span><span class="sxs-lookup"><span data-stu-id="03621-129">Create controller</span></span> 

    - <span data-ttu-id="03621-130">Le **MvcHandler** objet utilise le **RequestContext** instance pour identifier le **IControllerFactory** objet (en général, une instance de la  **DefaultControllerFactory** classe) pour créer l’instance de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="03621-130">The **MvcHandler** object uses the **RequestContext** instance to identify the **IControllerFactory** object (typically an instance of the **DefaultControllerFactory** class) to create the controller instance with.</span></span>
- <span data-ttu-id="03621-131">Execute controller - le **MvcHandler** instance appelle le contrôleur s **Execute** (méthode).</span><span class="sxs-lookup"><span data-stu-id="03621-131">Execute controller - The **MvcHandler** instance calls the controller s **Execute** method.</span></span> |
- <span data-ttu-id="03621-132">Appeler l’action</span><span class="sxs-lookup"><span data-stu-id="03621-132">Invoke action</span></span> 

    - <span data-ttu-id="03621-133">La plupart des contrôleurs héritent le **contrôleur** classe de base.</span><span class="sxs-lookup"><span data-stu-id="03621-133">Most controllers inherit from the **Controller** base class.</span></span> <span data-ttu-id="03621-134">Pour les contrôleurs qui le faire, le **ControllerActionInvoker** objet qui est associé au contrôleur détermine la méthode d’action de la classe de contrôleur à appeler, puis appelle cette méthode.</span><span class="sxs-lookup"><span data-stu-id="03621-134">For controllers that do so, the **ControllerActionInvoker** object that is associated with the controller determines which action method of the controller class to call, and then calls that method.</span></span>
- <span data-ttu-id="03621-135">Exécution du résultat</span><span class="sxs-lookup"><span data-stu-id="03621-135">Execute result</span></span> 

    - <span data-ttu-id="03621-136">Une méthode d’action par défaut peut recevoir l’entrée d’utilisateur, préparer les données de la réponse appropriée, puis exécutez le résultat en retournant un type de résultat.</span><span class="sxs-lookup"><span data-stu-id="03621-136">A typical action method might receive user input, prepare the appropriate response data, and then execute the result by returning a result type.</span></span> <span data-ttu-id="03621-137">Les types de résultats intégrés qui peuvent être exécutés incluent les éléments suivants : **ViewResult** (qui restitue une vue et est le type de résultat plus fréquemment utilisé), **RedirectToRouteResult**,  **RedirectResult**, **ContentResult**, **JsonResult**, et **EmptyResult**.</span><span class="sxs-lookup"><span data-stu-id="03621-137">The built-in result types that can be executed include the following: **ViewResult** (which renders a view and is the most-often used result type), **RedirectToRouteResult**, **RedirectResult**, **ContentResult**, **JsonResult**, and **EmptyResult**.</span></span>
