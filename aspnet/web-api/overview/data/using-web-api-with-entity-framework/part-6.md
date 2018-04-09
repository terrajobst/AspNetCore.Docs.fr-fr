---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: Créer le Client JavaScript | Documents Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: 29d50e448e6d282c7db06b9d1946ac221347e1ea
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="create-the-javascript-client"></a><span data-ttu-id="a2c71-102">Créer le Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="a2c71-102">Create the JavaScript Client</span></span>
====================
<span data-ttu-id="a2c71-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a2c71-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="a2c71-104">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="a2c71-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="a2c71-105">Dans cette section, vous allez créer le client pour l’application, à l’aide de HTML, JavaScript et le [Knockout.js](http://knockoutjs.com/) bibliothèque.</span><span class="sxs-lookup"><span data-stu-id="a2c71-105">In this section, you will create the client for the application, using HTML, JavaScript, and the [Knockout.js](http://knockoutjs.com/) library.</span></span> <span data-ttu-id="a2c71-106">Nous allons construire l’application cliente en étapes :</span><span class="sxs-lookup"><span data-stu-id="a2c71-106">We'll build the client app in stages:</span></span>

- <span data-ttu-id="a2c71-107">Affichage d’une liste de livres.</span><span class="sxs-lookup"><span data-stu-id="a2c71-107">Showing a list of books.</span></span>
- <span data-ttu-id="a2c71-108">Affiche un détail de livre.</span><span class="sxs-lookup"><span data-stu-id="a2c71-108">Showing a book detail.</span></span>
- <span data-ttu-id="a2c71-109">Ajout d’un nouveau livre.</span><span class="sxs-lookup"><span data-stu-id="a2c71-109">Adding a new book.</span></span>

<span data-ttu-id="a2c71-110">La bibliothèque de Knockout utilise le modèle Model-View-ViewModel (MVVM) :</span><span class="sxs-lookup"><span data-stu-id="a2c71-110">The Knockout library uses the Model-View-ViewModel (MVVM) pattern:</span></span>

- <span data-ttu-id="a2c71-111">Le **modèle** est la représentation sous forme de côté serveur des données dans le domaine d’entreprise (dans notre cas, la documentation et d’auteurs).</span><span class="sxs-lookup"><span data-stu-id="a2c71-111">The **model** is the server-side representation of the data in the business domain (in our case, books and authors).</span></span>
- <span data-ttu-id="a2c71-112">Le **vue** est la couche de présentation (HTML).</span><span class="sxs-lookup"><span data-stu-id="a2c71-112">The **view** is the presentation layer (HTML).</span></span>
- <span data-ttu-id="a2c71-113">Le **le modèle de vue** est un objet JavaScript qui contient les modèles.</span><span class="sxs-lookup"><span data-stu-id="a2c71-113">The **view model** is a JavaScript object that holds the models.</span></span> <span data-ttu-id="a2c71-114">Le modèle d’affichage est une abstraction de code de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="a2c71-114">The view model is a code abstraction of the UI.</span></span> <span data-ttu-id="a2c71-115">Il n’a aucune connaissance de la représentation sous forme de code HTML.</span><span class="sxs-lookup"><span data-stu-id="a2c71-115">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="a2c71-116">Au lieu de cela, il représente fonctionnalités abstraites de la vue, telles que &quot;une liste de livres&quot;.</span><span class="sxs-lookup"><span data-stu-id="a2c71-116">Instead, it represents abstract features of the view, such as &quot;a list of books&quot;.</span></span>

<span data-ttu-id="a2c71-117">La vue est lié aux données pour le modèle d’affichage.</span><span class="sxs-lookup"><span data-stu-id="a2c71-117">The view is data-bound to the view model.</span></span> <span data-ttu-id="a2c71-118">Mises à jour le modèle d’affichage sont automatiquement reflétées dans la vue.</span><span class="sxs-lookup"><span data-stu-id="a2c71-118">Updates to the view model are automatically reflected in the view.</span></span> <span data-ttu-id="a2c71-119">Le modèle d’affichage obtient également les événements à partir de la vue, tels que les clics de bouton.</span><span class="sxs-lookup"><span data-stu-id="a2c71-119">The view model also gets events from the view, such as button clicks.</span></span>

![](part-6/_static/image1.png)

<span data-ttu-id="a2c71-120">Cette approche facilite modifier la disposition et l’interface utilisateur de votre application, comme vous pouvez modifier les liaisons, sans réécrire le code.</span><span class="sxs-lookup"><span data-stu-id="a2c71-120">This approach makes it easy to change the layout and UI of your app, because you can change the bindings, without rewriting any code.</span></span> <span data-ttu-id="a2c71-121">Par exemple, vous pouvez afficher une liste d’éléments en tant qu’un `<ul>`, puis le modifier ultérieurement à une table.</span><span class="sxs-lookup"><span data-stu-id="a2c71-121">For example, you might show a list of items as a `<ul>`, then change it later to a table.</span></span>

## <a name="add-the-knockout-library"></a><span data-ttu-id="a2c71-122">Ajoutez la bibliothèque Knockout</span><span class="sxs-lookup"><span data-stu-id="a2c71-122">Add the Knockout Library</span></span>

<span data-ttu-id="a2c71-123">Dans Visual Studio, à partir de la **outils** menu, sélectionnez **Gestionnaire de Package de bibliothèque**.</span><span class="sxs-lookup"><span data-stu-id="a2c71-123">In Visual Studio, from the **Tools** menu, select **Library Package Manager**.</span></span> <span data-ttu-id="a2c71-124">Puis sélectionnez **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="a2c71-124">Then select **Package Manager Console**.</span></span> <span data-ttu-id="a2c71-125">Dans la fenêtre de Console du Gestionnaire de Package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="a2c71-125">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-6/samples/sample1.cmd)]

<span data-ttu-id="a2c71-126">Cette commande ajoute les fichiers Knockout dans le dossier Scripts.</span><span class="sxs-lookup"><span data-stu-id="a2c71-126">This command adds the Knockout files to the Scripts folder.</span></span>

## <a name="create-the-view-model"></a><span data-ttu-id="a2c71-127">Créer le modèle d’affichage</span><span class="sxs-lookup"><span data-stu-id="a2c71-127">Create the View Model</span></span>

<span data-ttu-id="a2c71-128">Ajouter un fichier JavaScript nommé app.js dans le dossier Scripts.</span><span class="sxs-lookup"><span data-stu-id="a2c71-128">Add a JavaScript file named app.js to the Scripts folder.</span></span> <span data-ttu-id="a2c71-129">(Dans l’Explorateur de solutions, cliquez sur le dossier Scripts, sélectionnez **ajouter**, puis sélectionnez **fichier JavaScript**.) Collez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a2c71-129">(In Solution Explorer, right-click the Scripts folder, select **Add**, then select **JavaScript File**.) Paste in the following code:</span></span>

[!code-javascript[Main](part-6/samples/sample2.js)]

<span data-ttu-id="a2c71-130">Dans Knockout, la `observable` classe permet la liaison de données.</span><span class="sxs-lookup"><span data-stu-id="a2c71-130">In Knockout, the `observable` class enables data-binding.</span></span> <span data-ttu-id="a2c71-131">Lorsque le contenu d’un observable change, l’observable avertit tous les contrôles liés aux données, ils peuvent mettre à jour eux-mêmes.</span><span class="sxs-lookup"><span data-stu-id="a2c71-131">When the contents of an observable change, the observable notifies all of the data-bound controls, so they can update themselves.</span></span> <span data-ttu-id="a2c71-132">(Le `observableArray` classe est la version du tableau de *observable*.) Pour commencer, notre modèle de vue possède deux observables :</span><span class="sxs-lookup"><span data-stu-id="a2c71-132">(The `observableArray` class is the array version of *observable*.) To start with, our view model has two observables:</span></span>

- <span data-ttu-id="a2c71-133">`books` contient la liste de livres.</span><span class="sxs-lookup"><span data-stu-id="a2c71-133">`books` holds the list of books.</span></span>
- <span data-ttu-id="a2c71-134">`error` contient un message d’erreur si un appel AJAX échoue.</span><span class="sxs-lookup"><span data-stu-id="a2c71-134">`error` contains an error message if an AJAX call fails.</span></span>

<span data-ttu-id="a2c71-135">Le `getAllBooks` méthode effectue un appel AJAX pour obtenir la liste de livres.</span><span class="sxs-lookup"><span data-stu-id="a2c71-135">The `getAllBooks` method makes an AJAX call to get the list of books.</span></span> <span data-ttu-id="a2c71-136">Puis il exécute un push du résultat dans la `books` tableau.</span><span class="sxs-lookup"><span data-stu-id="a2c71-136">Then it pushes the result onto the `books` array.</span></span>

<span data-ttu-id="a2c71-137">Le `ko.applyBindings` méthode fait partie de la bibliothèque de masquage.</span><span class="sxs-lookup"><span data-stu-id="a2c71-137">The `ko.applyBindings` method is part of the Knockout library.</span></span> <span data-ttu-id="a2c71-138">Il prend le modèle d’affichage en tant que paramètre et configure la liaison de données.</span><span class="sxs-lookup"><span data-stu-id="a2c71-138">It takes the view model as a parameter and sets up the data binding.</span></span>

## <a name="add-a-script-bundle"></a><span data-ttu-id="a2c71-139">Ajouter un regroupement de Script</span><span class="sxs-lookup"><span data-stu-id="a2c71-139">Add a Script Bundle</span></span>

<span data-ttu-id="a2c71-140">Regroupement est une fonctionnalité de ASP.NET 4.5 qui vous permettent de combiner ou de regrouper plusieurs fichiers dans un seul fichier.</span><span class="sxs-lookup"><span data-stu-id="a2c71-140">Bundling is a feature in ASP.NET 4.5 that makes it easy to combine or bundle multiple files into a single file.</span></span> <span data-ttu-id="a2c71-141">Regroupement permet de réduire le nombre de demandes au serveur, ce qui peut améliorer les temps de chargement de page.</span><span class="sxs-lookup"><span data-stu-id="a2c71-141">Bundling reduces the number of requests to the server, which can improve page load time.</span></span>

<span data-ttu-id="a2c71-142">Ouvrez le fichier App\_Start/BundleConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="a2c71-142">Open the file App\_Start/BundleConfig.cs.</span></span> <span data-ttu-id="a2c71-143">Ajoutez le code suivant à la méthode RegisterBundles.</span><span class="sxs-lookup"><span data-stu-id="a2c71-143">Add the following code to the RegisterBundles method.</span></span>

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> <span data-ttu-id="a2c71-144">[Précédent](part-5.md)
> [Suivant](part-7.md)</span><span class="sxs-lookup"><span data-stu-id="a2c71-144">[Previous](part-5.md)
[Next](part-7.md)</span></span>
