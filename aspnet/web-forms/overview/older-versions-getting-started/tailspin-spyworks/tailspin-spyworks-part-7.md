---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'Partie 7 : Ajout de fonctionnalités | Microsoft Docs'
author: JoeStagner
description: Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application Tailspin Spyworks. Partie 7 ajoute des fonctionnalités supplémentaires, telles que le volet du compte...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: 8cdde10981835877e5ac2f65860010920a68d0a2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389174"
---
<a name="part-7-adding-features"></a><span data-ttu-id="5e1c0-104">Partie 7 : Ajout de fonctionnalités</span><span class="sxs-lookup"><span data-stu-id="5e1c0-104">Part 7: Adding Features</span></span>
====================
<span data-ttu-id="5e1c0-105">par [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="5e1c0-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="5e1c0-106">Tailspin Spyworks montre comment extrêmement simple est de créer des applications puissantes et évolutives pour la plate-forme .NET.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="5e1c0-107">Il montre comment utiliser les nouvelles fonctionnalités dans ASP.NET 4 pour créer un magasin en ligne, y compris les achats, extraction et administration.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="5e1c0-108">Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="5e1c0-109">Partie 7 ajoute des fonctionnalités supplémentaires, telles que la révision de compte, les évaluations de produits et les « éléments populaires » et les contrôles utilisateur « également acheté ».</span><span class="sxs-lookup"><span data-stu-id="5e1c0-109">Part 7 adds additional features, such as account review, product reviews, and "popular items" and "also purchased" user controls.</span></span>


## <a id="_Toc260221673"></a>  <span data-ttu-id="5e1c0-110">Ajout de fonctionnalités</span><span class="sxs-lookup"><span data-stu-id="5e1c0-110">Adding Features</span></span>

<span data-ttu-id="5e1c0-111">Bien que les utilisateurs peuvent parcourir notre catalogue, placer des éléments dans leur panier d’achat et le processus de validation, il existe qu'un nombre de prise en charge des fonctionnalités que nous inclurons pour améliorer notre site.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-111">Though users can browse our catalog, place items in their shopping cart, and complete the checkout process, there are a number of supporting features that we will include to improve our site.</span></span>

1. <span data-ttu-id="5e1c0-112">Examen du compte (liste de commandes placées et afficher les détails.)</span><span class="sxs-lookup"><span data-stu-id="5e1c0-112">Account Review (List orders placed and view details.)</span></span>
2. <span data-ttu-id="5e1c0-113">Ajouter un contenu spécifique de contexte à la première page.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-113">Add some context specific content to the front page.</span></span>
3. <span data-ttu-id="5e1c0-114">Ajouter une fonctionnalité pour permettre aux utilisateurs de vérifier les produits dans le catalogue.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-114">Add a feature to let users Review the products in the catalog.</span></span>
4. <span data-ttu-id="5e1c0-115">Créer un contrôle utilisateur pour afficher les éléments les plus courants et sur Place qui contrôlent sur la première page.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-115">Create a User Control to display Popular Items and Place that control on the front page.</span></span>
5. <span data-ttu-id="5e1c0-116">Créer un contrôle utilisateur « Également acheté » et l’ajouter à la page Détails du produit.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-116">Create an "Also Purchased" user control and add it to the product details page.</span></span>
6. <span data-ttu-id="5e1c0-117">Ajouter un Contact Page.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-117">Add a Contact Page.</span></span>
7. <span data-ttu-id="5e1c0-118">Ajouter un sur la Page.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-118">Add an About Page.</span></span>
8. <span data-ttu-id="5e1c0-119">Erreur globale</span><span class="sxs-lookup"><span data-stu-id="5e1c0-119">Global Error</span></span>

## <a id="_Toc260221674"></a>  <span data-ttu-id="5e1c0-120">Votre compte</span><span class="sxs-lookup"><span data-stu-id="5e1c0-120">Account Review</span></span>

<span data-ttu-id="5e1c0-121">Dans le dossier « Compte », créez deux pages .aspx un OrderList.aspx nommés et les autres OrderDetails.aspx nommé</span><span class="sxs-lookup"><span data-stu-id="5e1c0-121">In the "Account" folder create two .aspx pages one named OrderList.aspx and the other named OrderDetails.aspx</span></span>

<span data-ttu-id="5e1c0-122">OrderList.aspx s’appuieront sur les contrôles GridView et EntityDataSoure autant que nous avons précédemment.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-122">OrderList.aspx will leverage the GridView and EntityDataSoure controls much as we have previously.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

<span data-ttu-id="5e1c0-123">Le EntityDataSoure sélectionne les enregistrements de la table Orders filtrée sur le nom d’utilisateur (voir la WhereParameter) qui nous défini dans une variable de session lorsque le journal utilisateur 's.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-123">The EntityDataSoure selects records from the Orders table filtered on the UserName (see the WhereParameter) which we set in a session variable when the user log's in.</span></span>

<span data-ttu-id="5e1c0-124">Notez également ces paramètres dans le HyperlinkField du contrôle GridView :</span><span class="sxs-lookup"><span data-stu-id="5e1c0-124">Note also these parameters in the HyperlinkField of the GridView:</span></span>

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

<span data-ttu-id="5e1c0-125">Ils spécifient le lien vers la vue de détails de commande pour chaque produit spécifiant le champ OrderID comme un paramètre de chaîne de requête à la page OrderDetails.aspx.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-125">These specify the link to the Order details view for each product specifying the OrderID field as a QueryString parameter to the OrderDetails.aspx page.</span></span>

## <a id="_Toc260221675"></a>  <span data-ttu-id="5e1c0-126">OrderDetails.aspx</span><span class="sxs-lookup"><span data-stu-id="5e1c0-126">OrderDetails.aspx</span></span>

<span data-ttu-id="5e1c0-127">Nous allons utiliser un contrôle EntityDataSource pour accéder aux commandes et un FormView afin d’afficher les données de commande et un autre contrôle EntityDataSource avec un GridView pour afficher les éléments de ligne de toutes les la commande.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-127">We will use an EntityDataSource control to access the Orders and a FormView to display the Order data and another EntityDataSource with a GridView to display all the Order's line items.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

<span data-ttu-id="5e1c0-128">Dans le fichier Code-Behind (OrderDetails.aspx.cs), nous avons deux bits peu de ménage.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-128">In the Code Behind file (OrderDetails.aspx.cs) we have two little bits of housekeeping.</span></span>

<span data-ttu-id="5e1c0-129">Nous devons d’abord vous assurer que OrderDetails obtient toujours un OrderId.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-129">First we need to make sure that OrderDetails always gets an OrderId.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

<span data-ttu-id="5e1c0-130">Nous devons également calculer et afficher le total à partir des éléments de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-130">We also need to calculate and display the order total from the line items.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>  <span data-ttu-id="5e1c0-131">La Page d’accueil</span><span class="sxs-lookup"><span data-stu-id="5e1c0-131">The Home Page</span></span>

<span data-ttu-id="5e1c0-132">Nous allons ajouter du contenu statique à la page Default.aspx.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-132">Let's add some static content to the Default.aspx page.</span></span>

<span data-ttu-id="5e1c0-133">Tout d’abord, je vais créer un dossier « Contenu » et qu’il contient un dossier Images (et je vais inclure une image à utiliser sur la page d’accueil.)</span><span class="sxs-lookup"><span data-stu-id="5e1c0-133">First I'll create a "Content" folder and within it an Images folder (and I'll include an image to be used on the home page.)</span></span>

<span data-ttu-id="5e1c0-134">Dans l’espace réservé au bas de la page Default.aspx, ajoutez le balisage suivant.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-134">Into the bottom placeholder of the Default.aspx page, add the following markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>  <span data-ttu-id="5e1c0-135">Évaluations de produits</span><span class="sxs-lookup"><span data-stu-id="5e1c0-135">Product Reviews</span></span>

<span data-ttu-id="5e1c0-136">Tout d’abord, nous allons ajouter un bouton avec un lien à un formulaire que nous pouvons utiliser pour entrer une évaluation de produit.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-136">First we'll add a button with a link to a form that we can use to enter a product review.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

<span data-ttu-id="5e1c0-137">Notez que nous passons ProductID dans la chaîne de requête</span><span class="sxs-lookup"><span data-stu-id="5e1c0-137">Note that we are passing the ProductID in the query string</span></span>

<span data-ttu-id="5e1c0-138">Suivant nous allons ajouter une page nommée ReviewAdd.aspx</span><span class="sxs-lookup"><span data-stu-id="5e1c0-138">Next let's add page named ReviewAdd.aspx</span></span>

<span data-ttu-id="5e1c0-139">Cette page utilise ASP.NET AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-139">This page will use the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="5e1c0-140">Si vous n'avez pas déjà fait donc vous pouvez le télécharger à partir de [DevExpress](http://devexpress.com/act) et il existe des conseils sur la configuration de la boîte à outils à utiliser avec Visual Studio ici [ https://www.asp.net/learn/ajax-videos/video-76.aspx ](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span><span class="sxs-lookup"><span data-stu-id="5e1c0-140">If you have not already done so you can download it from [DevExpress](http://devexpress.com/act) and there is guidance on setting up the toolkit for use with Visual Studio here [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span></span>

<span data-ttu-id="5e1c0-141">En mode Création, faites glisser des contrôles et des validateurs à partir de la boîte à outils et créer un formulaire similaire à celui ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-141">In design mode, drag controls and validators from the toolbox and build a form like the one below.</span></span>

![](tailspin-spyworks-part-7/_static/image2.jpg)

<span data-ttu-id="5e1c0-142">Le balisage ressemblera à ceci.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-142">The markup will look something like this.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

<span data-ttu-id="5e1c0-143">Maintenant que nous pouvons saisir des révisions, permet d’afficher ces avis sur la page du produit.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-143">Now that we can enter reviews, lets display those reviews on the product page.</span></span>

<span data-ttu-id="5e1c0-144">Ajoutez ce balisage à la page ProductDetails.aspx.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-144">Add this markup to the ProductDetails.aspx page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

<span data-ttu-id="5e1c0-145">À présent, notre application en cours d’exécution et en accédant à un produit affiche les informations de produit, y compris les avis des utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-145">Running our application now and navigating to a product shows the product information including customer reviews.</span></span>

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>  <span data-ttu-id="5e1c0-146">Contrôle les éléments demandés (création de contrôles utilisateur)</span><span class="sxs-lookup"><span data-stu-id="5e1c0-146">Popular Items Control (Creating User Controls)</span></span>

<span data-ttu-id="5e1c0-147">Afin d’augmenter les ventes sur votre site web, nous allons ajouter quelques fonctionnalités aux produits populaires ou connexes de « sell suggérées ».</span><span class="sxs-lookup"><span data-stu-id="5e1c0-147">In order to increase sales on your web site we will add a couple of features to "suggestive sell" popular or related products.</span></span>

<span data-ttu-id="5e1c0-148">La première de ces fonctionnalités sera une liste de produits plus populaires dans notre catalogue de produits.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-148">The first of these features will be a list of the more popular product in our product catalog.</span></span>

<span data-ttu-id="5e1c0-149">Nous allons créer un « contrôle utilisateur » pour afficher le haut de vente des articles sur la page d’accueil de notre application.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-149">We will create a "User Control" to display the top selling items on the home page of our application.</span></span> <span data-ttu-id="5e1c0-150">Dans la mesure où il s’agit d’un contrôle, nous pouvons l’utiliser sur n’importe quelle page en faisant simplement glisser le contrôle dans le Concepteur de Visual Studio sur n’importe quelle page Nous aimons.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-150">Since this will be a control, we can use it on any page by simply dragging and dropping the control in Visual Studio's designer onto any page that we like.</span></span>

<span data-ttu-id="5e1c0-151">Dans l’Explorateur de solutions de Visual Studio, avec le bouton droit sur le nom de la solution et créez un répertoire nommé « Contrôles ».</span><span class="sxs-lookup"><span data-stu-id="5e1c0-151">In Visual Studio's solutions explorer, right-click on the solution name and create a new directory named "Controls".</span></span> <span data-ttu-id="5e1c0-152">Il n’est pas nécessaire pour ce faire, nous permettent de maintenir notre projet organisé en créant tous nos contrôles utilisateur dans le répertoire « Contrôles ».</span><span class="sxs-lookup"><span data-stu-id="5e1c0-152">While it is not necessary to do so, we will help keep our project organized by creating all our user controls in the "Controls" directory.</span></span>

<span data-ttu-id="5e1c0-153">Avec le bouton droit sur le dossier de contrôles et choisir « Nouvel élément » :</span><span class="sxs-lookup"><span data-stu-id="5e1c0-153">Right-click on the controls folder and choose "New Item" :</span></span>

![](tailspin-spyworks-part-7/_static/image4.jpg)

<span data-ttu-id="5e1c0-154">Spécifiez un nom pour notre contrôle de « PopularItems ».</span><span class="sxs-lookup"><span data-stu-id="5e1c0-154">Specify a name for our control of "PopularItems".</span></span> <span data-ttu-id="5e1c0-155">Notez que l’extension de fichier pour les contrôles utilisateur est .ascx pas .aspx.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-155">Note that the file extension for user controls is .ascx not .aspx.</span></span>

<span data-ttu-id="5e1c0-156">Notre contrôle utilisateurs éléments courants est définie comme suit.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-156">Our Popular Items User control will be defined as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

<span data-ttu-id="5e1c0-157">Ici, nous utilisons une méthode que nous n'avons pas encore utilisé dans cette application.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-157">Here we're using a method we have not used yet in this application.</span></span> <span data-ttu-id="5e1c0-158">Nous utilisons le contrôle repeater et au lieu d’utiliser un contrôle de source de données nous le lions le contrôle Repeater avec les résultats d’une requête LINQ to Entities.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-158">We're using the repeater control and instead of using a data source control we're binding the Repeater Control to the results of a LINQ to Entities query.</span></span>

<span data-ttu-id="5e1c0-159">Dans le code-behind de notre contrôle nous le faire comme suit.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-159">In the code behind of our control we do that as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

<span data-ttu-id="5e1c0-160">Notez également cette ligne importante en haut du balisage de notre contrôle.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-160">Note also this important line at the top of our control's markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

<span data-ttu-id="5e1c0-161">Dans la mesure où les articles les plus populaires ne modifiera pas les minutes à toutes les minutes, nous pouvons ajouter une directive aching pour améliorer les performances de notre application.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-161">Since the most popular items won't be changing on a minute to minute basis we can add a aching directive to improve the performance of our application.</span></span> <span data-ttu-id="5e1c0-162">Cette directive amène le code de contrôles être exécutée uniquement lorsque la sortie mise en cache du contrôle arrive à expiration.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-162">This directive will cause the controls code to only be executed when the cached output of the control expires.</span></span> <span data-ttu-id="5e1c0-163">Sinon, la version mise en cache de sortie du contrôle servira.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-163">Otherwise, the cached version of the control's output will be used.</span></span>

<span data-ttu-id="5e1c0-164">Il nous suffit est maintenant inclure notre nouveau contrôle dans notre page Default.aspc.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-164">Now all we have to do is include our new control in our Default.aspc page.</span></span>

<span data-ttu-id="5e1c0-165">Utilisation et faites-la glisser pour placer une instance du contrôle dans la colonne ouvre notre formulaire par défaut.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-165">Use drag and drop to place an instance of the control in the open column of our Default form.</span></span>

![](tailspin-spyworks-part-7/_static/image5.jpg)

<span data-ttu-id="5e1c0-166">Lorsque nous exécutons notre application, la page d’accueil affiche maintenant les articles les plus populaires.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-166">Now when we run our application the home page displays the most popular items.</span></span>

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>  <span data-ttu-id="5e1c0-167">« Également acheté » (contrôles utilisateur avec des paramètres) du contrôle</span><span class="sxs-lookup"><span data-stu-id="5e1c0-167">"Also Purchased" Control (User Controls with Parameters)</span></span>

<span data-ttu-id="5e1c0-168">Le deuxième contrôle utilisateur que nous allons créer prendra suggéré convaincre le niveau suivant en ajoutant la spécificité du contexte.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-168">The second User Control that we'll create will take suggestive selling to the next level by adding context specificity.</span></span>

<span data-ttu-id="5e1c0-169">La logique pour calculer les premiers éléments « Également acheté » est non trivial.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-169">The logic to calculate the top "Also Purchased" items is non-trivial.</span></span>

<span data-ttu-id="5e1c0-170">Notre contrôle « Également acheté » sélectionne les enregistrements OrderDetails (précédemment achetés) pour l’ID de produit actuellement sélectionné et saisir la OrderIDs pour chaque commande unique est trouvé.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-170">Our "Also Purchased" control will select the OrderDetails records (previously purchased) for the currently selected ProductID and grab the OrderIDs for each unique order that is found.</span></span>

<span data-ttu-id="5e1c0-171">Ensuite, sélectionnez al les produits à partir de toutes ces commandes et somme de quantités achetées.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-171">Then we will select al the products from all those Orders and sum the quantities purchased.</span></span> <span data-ttu-id="5e1c0-172">Nous allons trier les produits par cette somme de la quantité, afficher les éléments de cinq.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-172">We'll sort the products by that quantity sum and display the top five items.</span></span>

<span data-ttu-id="5e1c0-173">Étant donné la complexité de cette logique, nous implémenterons cet algorithme comme une procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-173">Given the complexity of this logic, we will implement this algorithm as a stored procedure.</span></span>

<span data-ttu-id="5e1c0-174">Le T-SQL pour la procédure stockée est comme suit.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-174">The T-SQL for the stored procedure is as follows.</span></span>

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

<span data-ttu-id="5e1c0-175">Notez que cette procédure stockée (SelectPurchasedWithProducts) existait dans la base de données lorsque nous l’inclus dans notre application et lorsque nous avons généré l’Entity Data Model que nous avons spécifié qui, en plus des Tables et vues que nous devions, l’Entity Data Model doit inclure cette procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-175">Note that this stored procedure (SelectPurchasedWithProducts) existed in the database when we included it in our application and when we generated the Entity Data Model we specified that, in addition to the Tables and Views that we needed, the Entity Data Model should include this stored procedure.</span></span>

<span data-ttu-id="5e1c0-176">Pour accéder à la procédure stockée à partir de l’Entity Data Model que nous avons besoin d’importer la fonction.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-176">To access the stored procedure from the Entity Data Model we need to import the function.</span></span>

<span data-ttu-id="5e1c0-177">Double-cliquez sur l’Entity Data Model dans l’Explorateur de Solutions pour l’ouvrir dans le concepteur et ouvrez l’Explorateur de modèles, puis avec le bouton droit dans le concepteur et sélectionnez « Ajouter une importation de fonction ».</span><span class="sxs-lookup"><span data-stu-id="5e1c0-177">Double Click on the Entity Data Model in the Solutions Explorer to open it in the designer and open the Model Browser, then right-click in the designer and select "Add Function Import".</span></span>

![](tailspin-spyworks-part-7/_static/image1.png)

<span data-ttu-id="5e1c0-178">Ce faisant, cette boîte de dialogue s’ouvre.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-178">Doing so will open this dialog.</span></span>

![](tailspin-spyworks-part-7/_static/image2.png)

<span data-ttu-id="5e1c0-179">Renseignez les champs comme vous le constater ci-dessus, en sélectionnant le « SelectPurchasedWithProducts » et utiliser le nom de la procédure pour le nom de notre fonction importée.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-179">Fill out the fields as you see above, selecting the "SelectPurchasedWithProducts" and use the procedure name for the name of our imported function.</span></span>

<span data-ttu-id="5e1c0-180">Cliquez sur « Ok ».</span><span class="sxs-lookup"><span data-stu-id="5e1c0-180">Click "Ok".</span></span>

<span data-ttu-id="5e1c0-181">Avoir effectué cette opération que nous pouvons simplement programmer par rapport à la procédure stockée comme nous peut être n’importe quel autre élément dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-181">Having done this we can simply program against the stored procedure as we might any other item in the model.</span></span>

<span data-ttu-id="5e1c0-182">Par conséquent, dans notre dossier « Contrôles » vous devez créer un contrôle utilisateur nommé AlsoPurchased.ascx.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-182">So, in our "Controls" folder create a new user control named AlsoPurchased.ascx.</span></span>

<span data-ttu-id="5e1c0-183">Le balisage de ce contrôle sera sembler très familière pour le contrôle PopularItems.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-183">The markup for this control will look very familiar to the PopularItems control.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

<span data-ttu-id="5e1c0-184">La différence notable est qui ne sont pas de mise en cache la sortie dans la mesure où l’élément doit être restitué varient par produit.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-184">The notable difference is that are not caching the output since the item's to be rendered will differ by product.</span></span>

<span data-ttu-id="5e1c0-185">L’ID de produit sera une « property » au contrôle.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-185">The ProductId will be a "property" to the control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

<span data-ttu-id="5e1c0-186">Dans le Gestionnaire du contrôle événement PreRender nous seau faire trois choses.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-186">In the control's PreRender event handler we eed to do three things.</span></span>

1. <span data-ttu-id="5e1c0-187">Assurez-vous que l’ID de produit est défini.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-187">Make sure the ProductID is set.</span></span>
2. <span data-ttu-id="5e1c0-188">Voir s’il existe des produits qui ont été achetés avec l’instance actuelle.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-188">See if there are any products that have been purchased with the current one.</span></span>
3. <span data-ttu-id="5e1c0-189">Sortie de certains éléments, comme déterminé dans #2.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-189">Output some items as determined in #2.</span></span>

<span data-ttu-id="5e1c0-190">Remarquez combien il est facile d’appeler la procédure stockée via le modèle.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-190">Note how easy it is to call the stored procedure through the model.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

<span data-ttu-id="5e1c0-191">Après avoir déterminé qu’il sont « également acheté », nous pouvons simplement lier le contrôle repeater aux résultats retournés par la requête.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-191">After determining that there ARE "also purchased" we can simply bind the repeater to the results returned by the query.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

<span data-ttu-id="5e1c0-192">S’il n’existait pas tous les éléments « également acheté » nous affichons simplement des autres éléments les plus courants à partir de notre catalogue.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-192">If there were not any "also purchased" items we'll simply display other popular items from our catalog.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

<span data-ttu-id="5e1c0-193">Pour afficher les éléments « Également acheté », ouvrez la page ProductDetails.aspx et faites glisser le contrôle AlsoPurchased à partir de l’Explorateur de Solutions afin qu’il apparaisse dans cette position dans le balisage.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-193">To view the "Also Purchased" items, open the ProductDetails.aspx page and drag the AlsoPurchased control from the Solutions Explorer so that it appears in this position in the markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

<span data-ttu-id="5e1c0-194">Cela créera une référence au contrôle en haut de la page ProductDetails.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-194">Doing so will create a reference to the control at the top of the ProductDetails page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

<span data-ttu-id="5e1c0-195">Étant donné que le contrôle utilisateur AlsoPurchased requiert un nombre de ProductId, nous allons définir la propriété ProductID de notre contrôle à l’aide d’une instruction Eval par rapport à l’élément de modèle de données en cours de la page.</span><span class="sxs-lookup"><span data-stu-id="5e1c0-195">Since the AlsoPurchased user control requires a ProductId number we will set the ProductID property of our control by using an Eval statement against the current data model item of the page.</span></span>

![](tailspin-spyworks-part-7/_static/image3.png)

<span data-ttu-id="5e1c0-196">Lorsque nous créons et exécuter maintenant et accédez à un produit, nous voyons les éléments « Également acheté ».</span><span class="sxs-lookup"><span data-stu-id="5e1c0-196">When we build and run now and browse to a product we see the "Also Purchased" items.</span></span>

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> <span data-ttu-id="5e1c0-197">[Précédent](tailspin-spyworks-part-6.md)
> [Suivant](tailspin-spyworks-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="5e1c0-197">[Previous](tailspin-spyworks-part-6.md)
[Next](tailspin-spyworks-part-8.md)</span></span>
