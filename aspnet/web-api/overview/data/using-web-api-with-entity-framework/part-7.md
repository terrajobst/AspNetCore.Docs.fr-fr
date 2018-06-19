---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Création de la vue (UI) | Documents Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: 5052d7cca4a5c12a9ea56eb929d4794b19e82603
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878800"
---
<a name="create-the-view-ui"></a><span data-ttu-id="3e164-102">Création de la vue (UI)</span><span class="sxs-lookup"><span data-stu-id="3e164-102">Create the View (UI)</span></span>
====================
<span data-ttu-id="3e164-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3e164-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="3e164-104">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="3e164-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="3e164-105">Dans cette section, vous commencerez à définir le code HTML pour l’application et ajouter une liaison de données entre le code HTML et le modèle d’affichage.</span><span class="sxs-lookup"><span data-stu-id="3e164-105">In this section, you will start to define the HTML for the app, and add data binding between the HTML and the view model.</span></span>

<span data-ttu-id="3e164-106">Ouvrez le fichier Views/Home/Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="3e164-106">Open the file Views/Home/Index.cshtml.</span></span> <span data-ttu-id="3e164-107">Remplacez tout le contenu de ce fichier avec les éléments suivants.</span><span class="sxs-lookup"><span data-stu-id="3e164-107">Replace the entire contents of that file with the following.</span></span>

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

<span data-ttu-id="3e164-108">La plupart de la `div` sont les éléments de [Bootstrap](http://getbootstrap.com/) styles.</span><span class="sxs-lookup"><span data-stu-id="3e164-108">Most of the `div` elements are there for [Bootstrap](http://getbootstrap.com/) styling.</span></span> <span data-ttu-id="3e164-109">Les éléments importants sont ceux avec `data-bind` attributs.</span><span class="sxs-lookup"><span data-stu-id="3e164-109">The important elements are the ones with `data-bind` attributes.</span></span> <span data-ttu-id="3e164-110">Cet attribut lie le code HTML pour le modèle d’affichage.</span><span class="sxs-lookup"><span data-stu-id="3e164-110">This attribute links the HTML to the view model.</span></span>

<span data-ttu-id="3e164-111">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="3e164-111">For example:</span></span>

[!code-html[Main](part-7/samples/sample2.html)]

<span data-ttu-id="3e164-112">Dans cet exemple, le &quot; `text` &quot; liaison provoque la `<p>` élément pour afficher la valeur de la `error` propriété à partir du modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="3e164-112">In this example, the &quot;`text`&quot; binding causes the `<p>` element to show the value of the `error` property from the view model.</span></span> <span data-ttu-id="3e164-113">N’oubliez pas que `error` a été déclaré comme un `ko.observable`:</span><span class="sxs-lookup"><span data-stu-id="3e164-113">Recall that `error` was declared as a `ko.observable`:</span></span>

[!code-javascript[Main](part-7/samples/sample3.js)]

<span data-ttu-id="3e164-114">Chaque fois qu’une nouvelle valeur est attribuée à `error`, Knockout met à jour le texte dans le `<p>` élément.</span><span class="sxs-lookup"><span data-stu-id="3e164-114">Whenever a new value is assigned to `error`, Knockout updates the text in the `<p>` element.</span></span>

<span data-ttu-id="3e164-115">Le `foreach` liaison indique Knockout pour parcourir le contenu de le `books` tableau.</span><span class="sxs-lookup"><span data-stu-id="3e164-115">The `foreach` binding tells Knockout to loop through the contents of the `books` array.</span></span> <span data-ttu-id="3e164-116">Pour chaque élément du tableau, Knockout crée un &lt;li&gt; élément.</span><span class="sxs-lookup"><span data-stu-id="3e164-116">For each item in the array, Knockout creates a new &lt;li&gt; element.</span></span> <span data-ttu-id="3e164-117">Les liaisons dans le contexte de la `foreach` font référence aux propriétés sur l’élément de tableau.</span><span class="sxs-lookup"><span data-stu-id="3e164-117">Bindings inside the context of the `foreach` refer to properties on the array item.</span></span> <span data-ttu-id="3e164-118">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="3e164-118">For example:</span></span>

[!code-html[Main](part-7/samples/sample4.html)]

<span data-ttu-id="3e164-119">Ici le `text` liaison lit la propriété Author de chaque ouvrage.</span><span class="sxs-lookup"><span data-stu-id="3e164-119">Here the `text` binding reads the Author property of each book.</span></span>

<span data-ttu-id="3e164-120">Si vous exécutez l’application maintenant, il doit ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="3e164-120">If you run the application now, it should look like this:</span></span>

![](part-7/_static/image1.png)

<span data-ttu-id="3e164-121">La liste des livres charge en mode asynchrone, une fois le chargement de la page.</span><span class="sxs-lookup"><span data-stu-id="3e164-121">The list of books loads asynchronously, after the page loads.</span></span> <span data-ttu-id="3e164-122">Actuellement, le &quot;détails&quot; liens ne sont pas fonctionnels.</span><span class="sxs-lookup"><span data-stu-id="3e164-122">Right now, the &quot;Details&quot; links are not functional.</span></span> <span data-ttu-id="3e164-123">Nous allons ajouter cette fonctionnalité dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="3e164-123">We'll add this functionality in the next section.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3e164-124">[Précédent](part-6.md)
> [Suivant](part-8.md)</span><span class="sxs-lookup"><span data-stu-id="3e164-124">[Previous](part-6.md)
[Next](part-8.md)</span></span>
