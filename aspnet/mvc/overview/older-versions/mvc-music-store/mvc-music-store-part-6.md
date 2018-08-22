---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'Partie 6 : À l’aide des Annotations de données pour la Validation de modèle | Microsoft Docs'
author: jongalloway
description: Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application ASP.NET MVC Music Store. Partie 6 couvre à l’aide des Annotations de données pour le modèle V...
ms.author: riande
ms.date: 04/21/2011
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: b666c3cef0b09c6d68cee581a3c27c08e3357cca
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834633"
---
<a name="part-6-using-data-annotations-for-model-validation"></a><span data-ttu-id="1251d-104">Partie 6 : À l’aide des Annotations de données pour la Validation de modèle</span><span class="sxs-lookup"><span data-stu-id="1251d-104">Part 6: Using Data Annotations for Model Validation</span></span>
====================
<span data-ttu-id="1251d-105">par [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="1251d-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="1251d-106">Le Store de musique MVC est une application du didacticiel qui présente et explique étape par étape comment utiliser ASP.NET MVC et Visual Studio pour le développement web.</span><span class="sxs-lookup"><span data-stu-id="1251d-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="1251d-107">Le Store de musique MVC est une implémentation de magasin d’exemple léger qui vend des albums de musique en ligne et implémente l’administration de site de base, connexion de l’utilisateur et les fonctionnalités de panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="1251d-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="1251d-108">Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="1251d-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="1251d-109">Partie 6 couvre à l’aide des Annotations de données pour la Validation de modèle.</span><span class="sxs-lookup"><span data-stu-id="1251d-109">Part 6 covers Using Data Annotations for Model Validation.</span></span>


<span data-ttu-id="1251d-110">Nous avons un problème majeur avec nos formulaires Create et Edit : qu’elles ne font pas de toute opération de validation.</span><span class="sxs-lookup"><span data-stu-id="1251d-110">We have a major issue with our Create and Edit forms: they're not doing any validation.</span></span> <span data-ttu-id="1251d-111">Nous pouvons effectuer des opérations comme laisser les champs obligatoires vides ou des lettres de type dans le champ prix, et la première erreur, que nous allons voir provient de la base de données.</span><span class="sxs-lookup"><span data-stu-id="1251d-111">We can do things like leave required fields blank or type letters in the Price field, and the first error we'll see is from the database.</span></span>

<span data-ttu-id="1251d-112">Nous pouvons facilement ajouter la validation à notre application en ajoutant des Annotations de données à nos classes de modèle.</span><span class="sxs-lookup"><span data-stu-id="1251d-112">We can easily add validation to our application by adding Data Annotations to our model classes.</span></span> <span data-ttu-id="1251d-113">Annotations de données nous permettent de décrire les règles que nous voulons appliquées aux propriétés de notre modèle et ASP.NET MVC s’occupera de les appliquer et d’afficher les messages appropriés à nos utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="1251d-113">Data Annotations allow us to describe the rules we want applied to our model properties, and ASP.NET MVC will take care of enforcing them and displaying appropriate messages to our users.</span></span>

## <a name="adding-validation-to-our-album-forms"></a><span data-ttu-id="1251d-114">Ajout d’une Validation à nos formulaires Album</span><span class="sxs-lookup"><span data-stu-id="1251d-114">Adding Validation to our Album Forms</span></span>

<span data-ttu-id="1251d-115">Nous allons utiliser les attributs d’Annotation de données suivants :</span><span class="sxs-lookup"><span data-stu-id="1251d-115">We'll use the following Data Annotation attributes:</span></span>

- <span data-ttu-id="1251d-116">**Requis** – indique que la propriété est un champ obligatoire</span><span class="sxs-lookup"><span data-stu-id="1251d-116">**Required** – Indicates that the property is a required field</span></span>
- <span data-ttu-id="1251d-117">**DisplayName** – définit le texte que nous voulons utilisés sur les champs de formulaire et les messages de validation</span><span class="sxs-lookup"><span data-stu-id="1251d-117">**DisplayName** – Defines the text we want used on form fields and validation messages</span></span>
- <span data-ttu-id="1251d-118">**StringLength** – définit une longueur maximale pour un champ de chaîne</span><span class="sxs-lookup"><span data-stu-id="1251d-118">**StringLength** – Defines a maximum length for a string field</span></span>
- <span data-ttu-id="1251d-119">**Plage** – donne la valeur minimale et maximale pour un champ numérique</span><span class="sxs-lookup"><span data-stu-id="1251d-119">**Range** – Gives a maximum and minimum value for a numeric field</span></span>
- <span data-ttu-id="1251d-120">**Lier** – répertorie les champs à exclure ou inclure lors de la liaison de paramètre ou la forme de valeurs aux propriétés de modèle</span><span class="sxs-lookup"><span data-stu-id="1251d-120">**Bind** – Lists fields to exclude or include when binding parameter or form values to model properties</span></span>
- <span data-ttu-id="1251d-121">**ScaffoldColumn** – permet de masquer les champs de formulaires de l’éditeur</span><span class="sxs-lookup"><span data-stu-id="1251d-121">**ScaffoldColumn** – Allows hiding fields from editor forms</span></span>

<span data-ttu-id="1251d-122">*Remarque : Pour plus d’informations sur la Validation de modèle à l’aide des attributs d’Annotation de données, consultez la documentation MSDN à l’adresse*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span><span class="sxs-lookup"><span data-stu-id="1251d-122">*Note: For more information on Model Validation using Data Annotation attributes, see the MSDN documentation at*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span></span>

<span data-ttu-id="1251d-123">Ouvrez la classe Album et ajoutez le code suivant *à l’aide de* instructions au début.</span><span class="sxs-lookup"><span data-stu-id="1251d-123">Open the Album class and add the following *using* statements to the top.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

<span data-ttu-id="1251d-124">Ensuite, mettez à jour les propriétés pour ajouter des attributs d’affichage et la validation comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="1251d-124">Next, update the properties to add display and validation attributes as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

<span data-ttu-id="1251d-125">Parallèlement, nous avons également modifié le Genre et l’artiste propriétés virtuelles.</span><span class="sxs-lookup"><span data-stu-id="1251d-125">While we're there, we've also changed the Genre and Artist to virtual properties.</span></span> <span data-ttu-id="1251d-126">Cela permet à Entity Framework chargés en différé-les en fonction des besoins.</span><span class="sxs-lookup"><span data-stu-id="1251d-126">This allows Entity Framework to lazy-load them as necessary.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

<span data-ttu-id="1251d-127">Après avoir ajouté ces attributs à notre modèle Album, notre écran Create et Edit commencer immédiatement la validation des champs et en utilisant les noms d’affichage, nous avons choisi (par exemple, Album Art Url au lieu de AlbumArtUrl).</span><span class="sxs-lookup"><span data-stu-id="1251d-127">After having added these attributes to our Album model, our Create and Edit screen immediately begin validating fields and using the Display Names we've chosen (e.g. Album Art Url instead of AlbumArtUrl).</span></span> <span data-ttu-id="1251d-128">Exécutez l’application et accédez à /StoreManager/Create.</span><span class="sxs-lookup"><span data-stu-id="1251d-128">Run the application and browse to /StoreManager/Create.</span></span>

![](mvc-music-store-part-6/_static/image1.png)

<span data-ttu-id="1251d-129">Ensuite, nous allons répartir des règles de validation.</span><span class="sxs-lookup"><span data-stu-id="1251d-129">Next, we'll break some validation rules.</span></span> <span data-ttu-id="1251d-130">Entrez un prix de 0 et ne renseignez pas le titre.</span><span class="sxs-lookup"><span data-stu-id="1251d-130">Enter a price of 0 and leave the Title blank.</span></span> <span data-ttu-id="1251d-131">Lorsque nous cliquons sur le bouton Créer, nous verrons le formulaire affiché avec la validation des messages d’erreur affichant les champs qui ne répondait pas aux règles de validation, nous avons défini.</span><span class="sxs-lookup"><span data-stu-id="1251d-131">When we click on the Create button, we will see the form displayed with validation error messages showing which fields did not meet the validation rules we have defined.</span></span>

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a><span data-ttu-id="1251d-132">Test de la Validation côté Client</span><span class="sxs-lookup"><span data-stu-id="1251d-132">Testing the Client-Side Validation</span></span>

<span data-ttu-id="1251d-133">Validation côté serveur est très importante du point de vue de l’application, car les utilisateurs peuvent contourner la validation côté client.</span><span class="sxs-lookup"><span data-stu-id="1251d-133">Server-side validation is very important from an application perspective, because users can circumvent client-side validation.</span></span> <span data-ttu-id="1251d-134">Cependant, les formulaires de page Web qui implémentent uniquement la validation côté serveur comportent trois problèmes importants.</span><span class="sxs-lookup"><span data-stu-id="1251d-134">However, webpage forms which only implement server-side validation exhibit three significant problems.</span></span>

1. <span data-ttu-id="1251d-135">L’utilisateur doit attendre pour le formulaire à valider, validé sur le serveur et pour la réponse à envoyer à leur navigateur.</span><span class="sxs-lookup"><span data-stu-id="1251d-135">The user has to wait for the form to be posted, validated on the server, and for the response to be sent to their browser.</span></span>
2. <span data-ttu-id="1251d-136">L’utilisateur n’obtenir des retours immédiats lorsqu’elles corrigent un champ afin qu’il réussit maintenant les règles de validation.</span><span class="sxs-lookup"><span data-stu-id="1251d-136">The user doesn't get immediate feedback when they correct a field so that it now passes the validation rules.</span></span>
3. <span data-ttu-id="1251d-137">Nous sommes gaspiller des ressources du serveur pour exécuter la logique de validation au lieu d’en tirant parti du navigateur de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="1251d-137">We are wasting server resources to perform validation logic instead of leveraging the user's browser.</span></span>

<span data-ttu-id="1251d-138">Heureusement, les modèles de structure ASP.NET MVC 3 ont la validation côté client intégrée, ne nécessitant aucun travail supplémentaire que ce soit.</span><span class="sxs-lookup"><span data-stu-id="1251d-138">Fortunately, the ASP.NET MVC 3 scaffold templates have client-side validation built in, requiring no additional work whatsoever.</span></span>

<span data-ttu-id="1251d-139">Tapez une lettre unique dans le champ titre satisfait les exigences de validation, donc le message de validation est immédiatement supprimé.</span><span class="sxs-lookup"><span data-stu-id="1251d-139">Typing a single letter in the Title field satisfies the validation requirements, so the validation message is immediately removed.</span></span>

![](mvc-music-store-part-6/_static/image3.png)


> [!div class="step-by-step"]
> <span data-ttu-id="1251d-140">[Précédent](mvc-music-store-part-5.md)
> [Suivant](mvc-music-store-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="1251d-140">[Previous](mvc-music-store-part-5.md)
[Next](mvc-music-store-part-7.md)</span></span>
