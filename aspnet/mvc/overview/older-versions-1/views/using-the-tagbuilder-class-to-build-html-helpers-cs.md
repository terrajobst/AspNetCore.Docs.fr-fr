---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
title: "Pour générer des programmes d’assistance HTML (c#) à l’aide de la classe TagBuilder | Documents Microsoft"
author: StephenWalther
description: "Stephen Walther présente une classe utilitaire utiles dans l’infrastructure ASP.NET MVC nommé de la classe TagBuilder. Vous pouvez utiliser la classe TagBuilder à facilement..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 3975a52f-bd15-4edd-8f3d-1df93672515b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 63f07c3f95c520dbc74f3568aa65dc6a6f34a901
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/12/2018
---
<a name="using-the-tagbuilder-class-to-build-html-helpers-c"></a><span data-ttu-id="4a179-104">À l’aide de la classe TagBuilder pour générer des programmes d’assistance HTML (c#)</span><span class="sxs-lookup"><span data-stu-id="4a179-104">Using the TagBuilder Class to Build HTML Helpers (C#)</span></span>
====================
<span data-ttu-id="4a179-105">par [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="4a179-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="4a179-106">Stephen Walther présente une classe utilitaire utiles dans l’infrastructure ASP.NET MVC nommé de la classe TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="4a179-106">Stephen Walther introduces you to a useful utility class in the ASP.NET MVC framework named the TagBuilder class.</span></span> <span data-ttu-id="4a179-107">Vous pouvez utiliser la classe TagBuilder permet de générer facilement des balises HTML.</span><span class="sxs-lookup"><span data-stu-id="4a179-107">You can use the TagBuilder class to easily build HTML tags.</span></span>


<span data-ttu-id="4a179-108">L’infrastructure ASP.NET MVC inclut une classe utilitaire pratique nommée de la classe TagBuilder que vous pouvez utiliser lors de la création de programmes d’assistance HTML.</span><span class="sxs-lookup"><span data-stu-id="4a179-108">The ASP.NET MVC framework includes a useful utility class named the TagBuilder class that you can use when building HTML helpers.</span></span> <span data-ttu-id="4a179-109">La classe TagBuilder, comme l’indique le nom de la classe, vous permet de créer facilement des balises HTML.</span><span class="sxs-lookup"><span data-stu-id="4a179-109">The TagBuilder class, as the name of the class suggests, enables you to easily build HTML tags.</span></span> <span data-ttu-id="4a179-110">Dans ce court didacticiel vous sont fournies avec une vue d’ensemble de la classe TagBuilder et vous apprenez à utiliser cette classe lors de la création d’un programme d’assistance HTML simple qui effectue le rendu HTML &lt;img&gt; balises.</span><span class="sxs-lookup"><span data-stu-id="4a179-110">In this brief tutorial, you are provided with an overview of the TagBuilder class and you learn how to use this class when building a simple HTML helper that renders HTML &lt;img&gt; tags.</span></span>

## <a name="overview-of-the-tagbuilder-class"></a><span data-ttu-id="4a179-111">Vue d’ensemble de la classe TagBuilder</span><span class="sxs-lookup"><span data-stu-id="4a179-111">Overview of the TagBuilder Class</span></span>

<span data-ttu-id="4a179-112">La classe TagBuilder est contenue dans l’espace de noms System.Web.Mvc.</span><span class="sxs-lookup"><span data-stu-id="4a179-112">The TagBuilder class is contained in the System.Web.Mvc namespace.</span></span> <span data-ttu-id="4a179-113">Il a cinq méthodes :</span><span class="sxs-lookup"><span data-stu-id="4a179-113">It has five methods:</span></span>

- <span data-ttu-id="4a179-114">AddCssClass() - vous permet d’ajouter une nouvelle *classe = « «* à une balise d’attribut.</span><span class="sxs-lookup"><span data-stu-id="4a179-114">AddCssClass() - Enables you to add a new *class=""* attribute to a tag.</span></span>
- <span data-ttu-id="4a179-115">GenerateId() - permet d’ajouter un attribut d’id à une balise.</span><span class="sxs-lookup"><span data-stu-id="4a179-115">GenerateId() - Enables you to add an id attribute to a tag.</span></span> <span data-ttu-id="4a179-116">Cette méthode remplace automatiquement les points dans l’id (par défaut, les points sont remplacés par des traits de soulignement)</span><span class="sxs-lookup"><span data-stu-id="4a179-116">This method automatically replaces periods in the id (by default, periods are replaced by underscores)</span></span>
- <span data-ttu-id="4a179-117">MergeAttribute() - permet d’ajouter des attributs à une balise.</span><span class="sxs-lookup"><span data-stu-id="4a179-117">MergeAttribute() - Enables you to add attributes to a tag.</span></span> <span data-ttu-id="4a179-118">Il existe plusieurs surcharges de cette méthode.</span><span class="sxs-lookup"><span data-stu-id="4a179-118">There are multiple overloads of this method.</span></span>
- <span data-ttu-id="4a179-119">SetInnerText() - vous permet de définir le texte interne de la balise.</span><span class="sxs-lookup"><span data-stu-id="4a179-119">SetInnerText() - Enables you to set the inner text of the tag.</span></span> <span data-ttu-id="4a179-120">Le texte interne est automatiquement encoder en HTML.</span><span class="sxs-lookup"><span data-stu-id="4a179-120">The inner text is HTML encode automatically.</span></span>
- <span data-ttu-id="4a179-121">ToString() - permet de restituer la balise.</span><span class="sxs-lookup"><span data-stu-id="4a179-121">ToString() - Enables you to render the tag.</span></span> <span data-ttu-id="4a179-122">Vous pouvez spécifier si vous souhaitez créer une balise normale, une balise de début, une balise de fin ou une balise de fermeture automatique.</span><span class="sxs-lookup"><span data-stu-id="4a179-122">You can specify whether you want to create a normal tag, a start tag, an end tag, or a self-closing tag.</span></span>
  

<span data-ttu-id="4a179-123">La classe TagBuilder possède quatre propriétés importantes :</span><span class="sxs-lookup"><span data-stu-id="4a179-123">The TagBuilder class has four important properties:</span></span>

- <span data-ttu-id="4a179-124">Attributs - représente tous les attributs de la balise.</span><span class="sxs-lookup"><span data-stu-id="4a179-124">Attributes - Represents all of the attributes of the tag.</span></span>
- <span data-ttu-id="4a179-125">IdAttributeDotReplacement - représente le caractère utilisé par la méthode GenerateId() pour remplacer les points (la valeur par défaut est un trait de soulignement).</span><span class="sxs-lookup"><span data-stu-id="4a179-125">IdAttributeDotReplacement - Represents the character used by the GenerateId() method to replace periods (the default is an underscore).</span></span>
- <span data-ttu-id="4a179-126">InnerHTML - représente le contenu interne de la balise.</span><span class="sxs-lookup"><span data-stu-id="4a179-126">InnerHTML - Represents the inner contents of the tag.</span></span> <span data-ttu-id="4a179-127">Assignez une chaîne à cette propriété *pas* HTML encoder la chaîne.</span><span class="sxs-lookup"><span data-stu-id="4a179-127">Assigning a string to this property *does not* HTML encode the string.</span></span>
- <span data-ttu-id="4a179-128">TagName - représente le nom de la balise.</span><span class="sxs-lookup"><span data-stu-id="4a179-128">TagName - Represents the name of the tag.</span></span>

<span data-ttu-id="4a179-129">Ces méthodes et propriétés vous donnent toutes les méthodes de base et les propriétés dont vous avez besoin pour créer une balise HTML.</span><span class="sxs-lookup"><span data-stu-id="4a179-129">These methods and properties give you all of the basic methods and properties that you need to build up an HTML tag.</span></span> <span data-ttu-id="4a179-130">Vous n’avez pas besoin réellement utiliser la classe TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="4a179-130">You don't really need to use the TagBuilder class.</span></span> <span data-ttu-id="4a179-131">Vous pouvez utiliser une classe StringBuilder à la place.</span><span class="sxs-lookup"><span data-stu-id="4a179-131">You could use a StringBuilder class instead.</span></span> <span data-ttu-id="4a179-132">Toutefois, la classe TagBuilder rend votre vie un peu plus facile.</span><span class="sxs-lookup"><span data-stu-id="4a179-132">However, the TagBuilder class makes your life a little easier.</span></span>

## <a name="creating-an-image-html-helper"></a><span data-ttu-id="4a179-133">Création d’une application d’assistance HTML de Image</span><span class="sxs-lookup"><span data-stu-id="4a179-133">Creating an Image HTML Helper</span></span>

<span data-ttu-id="4a179-134">Lorsque vous créez une instance de la classe TagBuilder, vous passez le nom de la balise que vous souhaitez générer au constructeur TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="4a179-134">When you create an instance of the TagBuilder class, you pass the name of the tag that you want to build to the TagBuilder constructor.</span></span> <span data-ttu-id="4a179-135">Ensuite, vous pouvez appeler des méthodes, telles que les méthodes AddCssClass et MergeAttribute() pour modifier les attributs de la balise.</span><span class="sxs-lookup"><span data-stu-id="4a179-135">Next, you can call methods like the AddCssClass and MergeAttribute() methods to modify the attributes of the tag.</span></span> <span data-ttu-id="4a179-136">Enfin, vous appelez la méthode ToString() pour restituer la balise.</span><span class="sxs-lookup"><span data-stu-id="4a179-136">Finally, you call the ToString() method to render the tag.</span></span>

<span data-ttu-id="4a179-137">Par exemple, le Listing 1 contient une application d’assistance HTML de l’Image.</span><span class="sxs-lookup"><span data-stu-id="4a179-137">For example, Listing 1 contains an Image HTML helper.</span></span> <span data-ttu-id="4a179-138">L’application d’assistance de l’Image est implémentée en interne avec un TagBuilder qui représente un élément HTML &lt;img&gt; balise.</span><span class="sxs-lookup"><span data-stu-id="4a179-138">The Image helper is implemented internally with a TagBuilder that represents an HTML &lt;img&gt; tag.</span></span>

<span data-ttu-id="4a179-139">**La liste 1 - Helpers\ImageHelper.cs**</span><span class="sxs-lookup"><span data-stu-id="4a179-139">**Listing 1 - Helpers\ImageHelper.cs**</span></span>

[!code-csharp[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample1.cs)]

<span data-ttu-id="4a179-140">La classe dans la liste 1 contient deux méthodes surchargées statiques nommés Image.</span><span class="sxs-lookup"><span data-stu-id="4a179-140">The class in Listing 1 contains two static overloaded methods named Image.</span></span> <span data-ttu-id="4a179-141">Lorsque vous appelez la méthode Image(), vous pouvez passer un objet qui représente un ensemble d’attributs HTML ou non.</span><span class="sxs-lookup"><span data-stu-id="4a179-141">When you call the Image() method, you can pass an object which represents a set of HTML attributes or not.</span></span>

<span data-ttu-id="4a179-142">Notez comment la méthode TagBuilder.MergeAttribute() permet d’ajouter des attributs individuels, tels que l’attribut src pour le TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="4a179-142">Notice how the TagBuilder.MergeAttribute() method is used to add individual attributes such as the src attribute to the TagBuilder.</span></span> <span data-ttu-id="4a179-143">En outre, notez comment la méthode TagBuilder.MergeAttributes() permet d’ajouter une collection d’attributs pour le TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="4a179-143">Notice, furthermore, how the TagBuilder.MergeAttributes() method is used to add a collection of attributes to the TagBuilder.</span></span> <span data-ttu-id="4a179-144">La méthode MergeAttributes() accepte un dictionnaire&lt;chaîne, objet&gt; paramètre.</span><span class="sxs-lookup"><span data-stu-id="4a179-144">The MergeAttributes() method accepts a Dictionary&lt;string,object&gt; parameter.</span></span> <span data-ttu-id="4a179-145">La classe RouteValueDictionary est utilisée pour convertir l’objet qui représente la collection d’attributs dans un dictionnaire&lt;chaîne, objet&gt;.</span><span class="sxs-lookup"><span data-stu-id="4a179-145">The RouteValueDictionary class is used to convert the Object representing the collection of attributes into a Dictionary&lt;string,object&gt;.</span></span>

<span data-ttu-id="4a179-146">Après avoir créé l’application d’assistance de l’Image, vous pouvez utiliser l’application d’assistance dans vos vues ASP.NET MVC, tout comme un de l’autres standards programmes d’assistance HTML.</span><span class="sxs-lookup"><span data-stu-id="4a179-146">After you create the Image helper, you can use the helper in your ASP.NET MVC views just like any of the other standard HTML helpers.</span></span> <span data-ttu-id="4a179-147">La vue dans la liste 2 utilise l’application d’assistance de l’Image pour afficher la même image d’une console Xbox deux fois (voir Figure 1).</span><span class="sxs-lookup"><span data-stu-id="4a179-147">The view in Listing 2 uses the Image helper to display the same image of an Xbox twice (see Figure 1).</span></span> <span data-ttu-id="4a179-148">L’application d’assistance Image() est appelée avec et sans une collection d’attributs HTML.</span><span class="sxs-lookup"><span data-stu-id="4a179-148">The Image() helper is called both with and without an HTML attribute collection.</span></span>

<span data-ttu-id="4a179-149">**Listing 2 - Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="4a179-149">**Listing 2 - Home\Index.aspx**</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample2.aspx)]


<span data-ttu-id="4a179-150">[![La boîte de dialogue Nouveau projet](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4a179-150">[![The New Project dialog box](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)</span></span>

<span data-ttu-id="4a179-151">**Figure 01**: à l’aide de l’application d’assistance d’Image ([cliquez pour afficher l’image en taille réelle](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="4a179-151">**Figure 01**: Using the Image helper([Click to view full-size image](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))</span></span>


<span data-ttu-id="4a179-152">Notez que vous devez importer l’espace de noms associé à l’application d’assistance de l’Image en haut de la vue Index.aspx.</span><span class="sxs-lookup"><span data-stu-id="4a179-152">Notice that you must import the namespace associated with the Image helper at the top of the Index.aspx view.</span></span> <span data-ttu-id="4a179-153">L’application d’assistance est importée avec la directive suivante :</span><span class="sxs-lookup"><span data-stu-id="4a179-153">The helper is imported with the following directive:</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample3.aspx)]

>[!div class="step-by-step"]
<span data-ttu-id="4a179-154">[Précédent](creating-custom-html-helpers-cs.md)
[Suivant](creating-page-layouts-with-view-master-pages-cs.md)</span><span class="sxs-lookup"><span data-stu-id="4a179-154">[Previous](creating-custom-html-helpers-cs.md)
[Next](creating-page-layouts-with-view-master-pages-cs.md)</span></span>
