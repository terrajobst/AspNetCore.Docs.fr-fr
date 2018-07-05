---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: À l’aide des API Web avec ASP.NET Web Forms | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2012
ms.topic: article
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: 91811031b937d3ca05888ce8d4f28bcc33537c76
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400877"
---
<a name="using-web-api-with-aspnet-web-forms"></a><span data-ttu-id="084f9-102">À l’aide des API Web avec ASP.NET Web Forms</span><span class="sxs-lookup"><span data-stu-id="084f9-102">Using Web API with ASP.NET Web Forms</span></span>
====================
<span data-ttu-id="084f9-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="084f9-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="084f9-104">Bien que l’API Web ASP.NET est fourni avec ASP.NET MVC, il est facile d’ajouter des API Web à une application ASP.NET Web Forms traditionnelle.</span><span class="sxs-lookup"><span data-stu-id="084f9-104">Although ASP.NET Web API is packaged with ASP.NET MVC, it is easy to add Web API to a traditional ASP.NET Web Forms application.</span></span> <span data-ttu-id="084f9-105">Ce didacticiel vous guide à travers les étapes.</span><span class="sxs-lookup"><span data-stu-id="084f9-105">This tutorial walks you through the steps.</span></span>

## <a name="overview"></a><span data-ttu-id="084f9-106">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="084f9-106">Overview</span></span>

<span data-ttu-id="084f9-107">Pour utiliser les API Web dans une application Web Forms, il existe deux étapes principales :</span><span class="sxs-lookup"><span data-stu-id="084f9-107">To use Web API in a Web Forms application, there are two main steps:</span></span>

- <span data-ttu-id="084f9-108">Ajouter un contrôleur d’API Web qui dérive de la **ApiController** classe.</span><span class="sxs-lookup"><span data-stu-id="084f9-108">Add a Web API controller that derives from the **ApiController** class.</span></span>
- <span data-ttu-id="084f9-109">Ajouter une table de routage pour le **Application\_Démarrer** (méthode).</span><span class="sxs-lookup"><span data-stu-id="084f9-109">Add a route table to the **Application\_Start** method.</span></span>

## <a name="create-a-web-forms-project"></a><span data-ttu-id="084f9-110">Créer un projet Web Forms</span><span class="sxs-lookup"><span data-stu-id="084f9-110">Create a Web Forms Project</span></span>

<span data-ttu-id="084f9-111">Démarrez Visual Studio et sélectionnez **nouveau projet** à partir de la **Démarrer** page.</span><span class="sxs-lookup"><span data-stu-id="084f9-111">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="084f9-112">Ou, à partir de la **fichier** menu, sélectionnez **New** , puis **projet**.</span><span class="sxs-lookup"><span data-stu-id="084f9-112">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="084f9-113">Dans le **modèles** volet, sélectionnez **modèles installés** et développez le **Visual C#** nœud.</span><span class="sxs-lookup"><span data-stu-id="084f9-113">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="084f9-114">Sous **Visual C#**, sélectionnez **Web**.</span><span class="sxs-lookup"><span data-stu-id="084f9-114">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="084f9-115">Dans la liste des modèles de projet, sélectionnez **Application ASP.NET Web Forms**.</span><span class="sxs-lookup"><span data-stu-id="084f9-115">In the list of project templates, select **ASP.NET Web Forms Application**.</span></span> <span data-ttu-id="084f9-116">Entrez un nom pour le projet et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="084f9-116">Enter a name for the project and click **OK**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="084f9-117">Créer le modèle et le contrôleur</span><span class="sxs-lookup"><span data-stu-id="084f9-117">Create the Model and Controller</span></span>

<span data-ttu-id="084f9-118">Ce didacticiel utilise les mêmes classes de modèle et du contrôleur en tant que le [mise en route](tutorial-your-first-web-api.md) didacticiel.</span><span class="sxs-lookup"><span data-stu-id="084f9-118">This tutorial uses the same model and controller classes as the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="084f9-119">Tout d’abord, ajoutez une classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="084f9-119">First, add a model class.</span></span> <span data-ttu-id="084f9-120">Dans **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter une classe**.</span><span class="sxs-lookup"><span data-stu-id="084f9-120">In **Solution Explorer**, right-click the project and select **Add Class**.</span></span> <span data-ttu-id="084f9-121">Nommez la classe produit et ajoutez l’implémentation suivante :</span><span class="sxs-lookup"><span data-stu-id="084f9-121">Name the class Product, and add the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

<span data-ttu-id="084f9-122">Ensuite, ajoutez un contrôleur d’API Web au projet., A *contrôleur* est l’objet qui gère les requêtes HTTP pour l’API Web.</span><span class="sxs-lookup"><span data-stu-id="084f9-122">Next, add a Web API controller to the project., A *controller* is the object that handles HTTP requests for Web API.</span></span>

<span data-ttu-id="084f9-123">Dans **l’Explorateur de solutions**, cliquez sur le projet.</span><span class="sxs-lookup"><span data-stu-id="084f9-123">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="084f9-124">Sélectionnez **ajouter un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="084f9-124">Select **Add New Item**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

<span data-ttu-id="084f9-125">Sous **modèles installés**, développez **Visual C#** et sélectionnez **Web**.</span><span class="sxs-lookup"><span data-stu-id="084f9-125">Under **Installed Templates**, expand **Visual C#** and select **Web**.</span></span> <span data-ttu-id="084f9-126">Ensuite, dans la liste des modèles, sélectionnez **Web API Controller Class**.</span><span class="sxs-lookup"><span data-stu-id="084f9-126">Then, from the list of templates, select **Web API Controller Class**.</span></span> <span data-ttu-id="084f9-127">Nommez le contrôleur « ProductsController » et cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="084f9-127">Name the controller "ProductsController" and click **Add**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

<span data-ttu-id="084f9-128">Le **ajouter un nouvel élément** Assistant va créer un fichier nommé ProductsController.cs.</span><span class="sxs-lookup"><span data-stu-id="084f9-128">The **Add New Item** wizard will create a file named ProductsController.cs.</span></span> <span data-ttu-id="084f9-129">Supprimez les méthodes de l’Assistant inclus et ajoutez les méthodes suivantes :</span><span class="sxs-lookup"><span data-stu-id="084f9-129">Delete the methods that the wizard included and add the following methods:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

<span data-ttu-id="084f9-130">Pour plus d’informations sur le code dans ce contrôleur, consultez le [mise en route](tutorial-your-first-web-api.md) didacticiel.</span><span class="sxs-lookup"><span data-stu-id="084f9-130">For more information about the code in this controller, see the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

## <a name="add-routing-information"></a><span data-ttu-id="084f9-131">Ajouter des informations de routage</span><span class="sxs-lookup"><span data-stu-id="084f9-131">Add Routing Information</span></span>

<span data-ttu-id="084f9-132">Ensuite, nous allons donc ajouter un itinéraire URI cet URI sous la forme &quot;/API/produits/&quot; sont acheminés vers le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="084f9-132">Next, we'll add a URI route so that URIs of the form &quot;/api/products/&quot; are routed to the controller.</span></span>

<span data-ttu-id="084f9-133">Dans **l’Explorateur de solutions**, double-cliquez sur Global.asax pour ouvrir le fichier code-behind Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="084f9-133">In **Solution Explorer**, double-click Global.asax to open the code-behind file Global.asax.cs.</span></span> <span data-ttu-id="084f9-134">Ajoutez le code suivant **à l’aide de** instruction.</span><span class="sxs-lookup"><span data-stu-id="084f9-134">Add the following **using** statement.</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

<span data-ttu-id="084f9-135">Puis ajoutez le code suivant à la **Application\_Démarrer** méthode :</span><span class="sxs-lookup"><span data-stu-id="084f9-135">Then add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

<span data-ttu-id="084f9-136">Pour plus d’informations sur les tables de routage, consultez [routage dans ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="084f9-136">For more information about routing tables, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="add-client-side-ajax"></a><span data-ttu-id="084f9-137">Ajouter côté Client AJAX</span><span class="sxs-lookup"><span data-stu-id="084f9-137">Add Client-Side AJAX</span></span>

<span data-ttu-id="084f9-138">Voilà, il que vous suffit de créer une API web que les clients peuvent accéder.</span><span class="sxs-lookup"><span data-stu-id="084f9-138">That's all you need to create a web API that clients can access.</span></span> <span data-ttu-id="084f9-139">Maintenant nous allons ajouter une page HTML qui utilise jQuery pour appeler l’API.</span><span class="sxs-lookup"><span data-stu-id="084f9-139">Now let's add an HTML page that uses jQuery to call the API.</span></span>

<span data-ttu-id="084f9-140">Assurez-vous que votre page maître (par exemple, *Site.Master*) inclut un `ContentPlaceHolder` avec `ID="HeadContent"`:</span><span class="sxs-lookup"><span data-stu-id="084f9-140">Make sure your master page (for example, *Site.Master*) includes a `ContentPlaceHolder` with `ID="HeadContent"`:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

<span data-ttu-id="084f9-141">Ouvrez le fichier Default.aspx.</span><span class="sxs-lookup"><span data-stu-id="084f9-141">Open the file Default.aspx.</span></span> <span data-ttu-id="084f9-142">Remplacez le texte réutilisable qui se trouve dans la section de contenu principale, comme indiqué :</span><span class="sxs-lookup"><span data-stu-id="084f9-142">Replace the boilerplate text that is in the main content section, as shown:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

<span data-ttu-id="084f9-143">Ensuite, ajoutez une référence au fichier source jQuery dans les `HeaderContent` section :</span><span class="sxs-lookup"><span data-stu-id="084f9-143">Next, add a reference to the jQuery source file in the `HeaderContent` section:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

<span data-ttu-id="084f9-144">Remarque : Vous pouvez facilement ajouter la référence de script en faisant glisser le fichier à partir de **l’Explorateur de solutions** dans la fenêtre d’éditeur de code.</span><span class="sxs-lookup"><span data-stu-id="084f9-144">Note: You can easily add the script reference by dragging and dropping the file from **Solution Explorer** into the code editor window.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

<span data-ttu-id="084f9-145">Sous la balise de script jQuery, ajoutez le bloc de script suivant :</span><span class="sxs-lookup"><span data-stu-id="084f9-145">Below the jQuery script tag, add the following script block:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

<span data-ttu-id="084f9-146">Lorsque le chargement du document, ce script effectue une requête AJAX à &quot;api/produits&quot;.</span><span class="sxs-lookup"><span data-stu-id="084f9-146">When the document loads, this script makes an AJAX request to &quot;api/products&quot;.</span></span> <span data-ttu-id="084f9-147">La requête retourne une liste de produits au format JSON.</span><span class="sxs-lookup"><span data-stu-id="084f9-147">The request returns a list of products in JSON format.</span></span> <span data-ttu-id="084f9-148">Le script ajoute les informations de produit à la table HTML.</span><span class="sxs-lookup"><span data-stu-id="084f9-148">The script adds the product information to the HTML table.</span></span>

<span data-ttu-id="084f9-149">Lorsque vous exécutez l’application, il doit ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="084f9-149">When you run the application, it should look like this:</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
