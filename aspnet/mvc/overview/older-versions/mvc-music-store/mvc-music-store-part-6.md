---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'Partie 6 : À l’aide des Annotations de données pour la Validation de modèle | Documents Microsoft'
author: jongalloway
description: Cette série de didacticiels détaille toutes les mesures prises pour générer l’exemple d’application de magasin de musique ASP.NET MVC. Partie 6 traite à l’aide des Annotations de données pour le modèle V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: 328eccb4324bb10a7e8dec819a70129fc14c42c4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="part-6-using-data-annotations-for-model-validation"></a><span data-ttu-id="85c35-104">Partie 6 : À l’aide des Annotations de données pour la Validation de modèle</span><span class="sxs-lookup"><span data-stu-id="85c35-104">Part 6: Using Data Annotations for Model Validation</span></span>
====================
<span data-ttu-id="85c35-105">par [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="85c35-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="85c35-106">Le magasin de musique MVC est une application du didacticiel qui présente et explique étape par étape comment utiliser ASP.NET MVC et Visual Studio pour le développement web.</span><span class="sxs-lookup"><span data-stu-id="85c35-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="85c35-107">Le magasin de musique MVC est une implémentation de magasin exemple léger qui vend des albums de musique en ligne et implémente l’administration de site de base, authentification de l’utilisateur et les fonctionnalités de panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="85c35-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="85c35-108">Cette série de didacticiels détaille toutes les mesures prises pour générer l’exemple d’application de magasin de musique ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="85c35-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="85c35-109">Partie 6 couvre à l’aide des Annotations de données pour la Validation de modèle.</span><span class="sxs-lookup"><span data-stu-id="85c35-109">Part 6 covers Using Data Annotations for Model Validation.</span></span>


<span data-ttu-id="85c35-110">Nous avons un problème majeur avec notre formulaire de créer et modifier : ils ne sont pas effectuées aucune validation.</span><span class="sxs-lookup"><span data-stu-id="85c35-110">We have a major issue with our Create and Edit forms: they're not doing any validation.</span></span> <span data-ttu-id="85c35-111">Nous pouvons faire laisser vides les champs obligatoires ou des lettres de type dans le champ prix, et la première erreur, que nous verrons provient de la base de données.</span><span class="sxs-lookup"><span data-stu-id="85c35-111">We can do things like leave required fields blank or type letters in the Price field, and the first error we'll see is from the database.</span></span>

<span data-ttu-id="85c35-112">Nous pouvons facilement ajouter une validation à notre application en ajoutant des Annotations de données à notre classes du modèle.</span><span class="sxs-lookup"><span data-stu-id="85c35-112">We can easily add validation to our application by adding Data Annotations to our model classes.</span></span> <span data-ttu-id="85c35-113">Annotations de données nous permettent de décrire les règles que nous souhaitons appliqués aux propriétés de notre modèle, et ASP.NET MVC s’occupe de l’appliquer et d’afficher les messages appropriés aux utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="85c35-113">Data Annotations allow us to describe the rules we want applied to our model properties, and ASP.NET MVC will take care of enforcing them and displaying appropriate messages to our users.</span></span>

## <a name="adding-validation-to-our-album-forms"></a><span data-ttu-id="85c35-114">Ajout d’une Validation à notre Forms Album</span><span class="sxs-lookup"><span data-stu-id="85c35-114">Adding Validation to our Album Forms</span></span>

<span data-ttu-id="85c35-115">Nous allons utiliser les attributs de l’Annotation de données suivants :</span><span class="sxs-lookup"><span data-stu-id="85c35-115">We'll use the following Data Annotation attributes:</span></span>

- <span data-ttu-id="85c35-116">**Requis** – indique que la propriété est un champ obligatoire</span><span class="sxs-lookup"><span data-stu-id="85c35-116">**Required** – Indicates that the property is a required field</span></span>
- <span data-ttu-id="85c35-117">**DisplayName** – définit le texte que nous souhaitons utilisés sur les champs de formulaire et des messages de validation</span><span class="sxs-lookup"><span data-stu-id="85c35-117">**DisplayName** – Defines the text we want used on form fields and validation messages</span></span>
- <span data-ttu-id="85c35-118">**StringLength** – définit une longueur maximale pour un champ de chaîne</span><span class="sxs-lookup"><span data-stu-id="85c35-118">**StringLength** – Defines a maximum length for a string field</span></span>
- <span data-ttu-id="85c35-119">**Plage** – donne la valeur minimale et maximale pour un champ numérique</span><span class="sxs-lookup"><span data-stu-id="85c35-119">**Range** – Gives a maximum and minimum value for a numeric field</span></span>
- <span data-ttu-id="85c35-120">**Lier** – répertorie les champs à exclure ou inclure lors de la liaison de paramètre ou la forme de valeurs aux propriétés de modèle</span><span class="sxs-lookup"><span data-stu-id="85c35-120">**Bind** – Lists fields to exclude or include when binding parameter or form values to model properties</span></span>
- <span data-ttu-id="85c35-121">**ScaffoldColumn** : autorise le masquage des champs à partir de formulaires de l’éditeur</span><span class="sxs-lookup"><span data-stu-id="85c35-121">**ScaffoldColumn** – Allows hiding fields from editor forms</span></span>

<span data-ttu-id="85c35-122">*Remarque : Pour plus d’informations sur la Validation de modèle à l’aide des attributs d’annotations de données, consultez la documentation MSDN à l’adresse*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span><span class="sxs-lookup"><span data-stu-id="85c35-122">*Note: For more information on Model Validation using Data Annotation attributes, see the MSDN documentation at*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span></span>

<span data-ttu-id="85c35-123">Ouvrez la classe Album et ajoutez le code suivant *à l’aide de* instructions au début.</span><span class="sxs-lookup"><span data-stu-id="85c35-123">Open the Album class and add the following *using* statements to the top.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

<span data-ttu-id="85c35-124">Ensuite, mettez à jour les propriétés pour ajouter des attributs d’affichage et de validation comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="85c35-124">Next, update the properties to add display and validation attributes as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

<span data-ttu-id="85c35-125">Pendant que nous sommes il, nous avons aussi modifié le Genre et artiste propriétés virtuelles.</span><span class="sxs-lookup"><span data-stu-id="85c35-125">While we're there, we've also changed the Genre and Artist to virtual properties.</span></span> <span data-ttu-id="85c35-126">Cela permet à Entity Framework charger en différé-les si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="85c35-126">This allows Entity Framework to lazy-load them as necessary.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

<span data-ttu-id="85c35-127">Après avoir ajouté des ces attributs à notre modèle Album, notre écran de créer et modifier immédiatement la validation des champs et en utilisant les noms d’affichage, nous avons choisi (par exemple, Album Art Url au lieu de AlbumArtUrl).</span><span class="sxs-lookup"><span data-stu-id="85c35-127">After having added these attributes to our Album model, our Create and Edit screen immediately begin validating fields and using the Display Names we've chosen (e.g. Album Art Url instead of AlbumArtUrl).</span></span> <span data-ttu-id="85c35-128">Exécutez l’application et accédez à /StoreManager/Create.</span><span class="sxs-lookup"><span data-stu-id="85c35-128">Run the application and browse to /StoreManager/Create.</span></span>

![](mvc-music-store-part-6/_static/image1.png)

<span data-ttu-id="85c35-129">Ensuite, nous allons arrêter des règles de validation.</span><span class="sxs-lookup"><span data-stu-id="85c35-129">Next, we'll break some validation rules.</span></span> <span data-ttu-id="85c35-130">Entrez un prix de 0 et ne renseignez pas le titre.</span><span class="sxs-lookup"><span data-stu-id="85c35-130">Enter a price of 0 and leave the Title blank.</span></span> <span data-ttu-id="85c35-131">Après avoir cliqué sur le bouton Créer, nous allons voir le formulaire affiché avec les messages d’erreur indiquant les champs qui ne répondait pas aux règles de validation que nous avons défini.</span><span class="sxs-lookup"><span data-stu-id="85c35-131">When we click on the Create button, we will see the form displayed with validation error messages showing which fields did not meet the validation rules we have defined.</span></span>

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a><span data-ttu-id="85c35-132">Test de la Validation côté Client</span><span class="sxs-lookup"><span data-stu-id="85c35-132">Testing the Client-Side Validation</span></span>

<span data-ttu-id="85c35-133">La validation côté serveur est très importante du point de vue de l’application, car les utilisateurs peuvent contourner la validation côté client.</span><span class="sxs-lookup"><span data-stu-id="85c35-133">Server-side validation is very important from an application perspective, because users can circumvent client-side validation.</span></span> <span data-ttu-id="85c35-134">Toutefois, les formulaires de page Web qui implémentent uniquement la validation côté serveur présentent trois problèmes importants.</span><span class="sxs-lookup"><span data-stu-id="85c35-134">However, webpage forms which only implement server-side validation exhibit three significant problems.</span></span>

1. <span data-ttu-id="85c35-135">L’utilisateur se trouve en attente pour le formulaire à valider, validé sur le serveur et pour la réponse à envoyer au navigateur.</span><span class="sxs-lookup"><span data-stu-id="85c35-135">The user has to wait for the form to be posted, validated on the server, and for the response to be sent to their browser.</span></span>
2. <span data-ttu-id="85c35-136">L’utilisateur n’immédiatement obtenir des commentaires lorsqu’ils corriger un champ pour qu’il passe désormais les règles de validation.</span><span class="sxs-lookup"><span data-stu-id="85c35-136">The user doesn't get immediate feedback when they correct a field so that it now passes the validation rules.</span></span>
3. <span data-ttu-id="85c35-137">Nous sommes gaspiller des ressources du serveur pour exécuter la logique de validation au lieu de tirer parti du navigateur de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="85c35-137">We are wasting server resources to perform validation logic instead of leveraging the user's browser.</span></span>

<span data-ttu-id="85c35-138">Heureusement, les modèles de vue de structure d’ASP.NET MVC 3 ont la validation côté client intégrée, ne nécessitant aucun travail supplémentaire que ce soit.</span><span class="sxs-lookup"><span data-stu-id="85c35-138">Fortunately, the ASP.NET MVC 3 scaffold templates have client-side validation built in, requiring no additional work whatsoever.</span></span>

<span data-ttu-id="85c35-139">Tapez une lettre unique dans le champ titre est conforme aux exigences de validation, par conséquent, le message de validation est supprimé immédiatement.</span><span class="sxs-lookup"><span data-stu-id="85c35-139">Typing a single letter in the Title field satisfies the validation requirements, so the validation message is immediately removed.</span></span>

![](mvc-music-store-part-6/_static/image3.png)


> [!div class="step-by-step"]
> <span data-ttu-id="85c35-140">[Précédent](mvc-music-store-part-5.md)
> [Suivant](mvc-music-store-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="85c35-140">[Previous](mvc-music-store-part-5.md)
[Next](mvc-music-store-part-7.md)</span></span>
