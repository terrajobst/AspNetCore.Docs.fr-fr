---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'Partie 7 : Ajout de fonctionnalités | Documents Microsoft'
author: JoeStagner
description: Cette série de didacticiels détaille toutes les mesures prises pour générer l’exemple d’application Tailspin Spyworks. Partie 7 ajoute des fonctionnalités supplémentaires, telles que le volet du compte...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: 17f068155f6726047901e2f7d580d375a4e07c87
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="part-7-adding-features"></a><span data-ttu-id="e1a07-104">Partie 7 : Fonctionnalités d’ajout</span><span class="sxs-lookup"><span data-stu-id="e1a07-104">Part 7: Adding Features</span></span>
====================
<span data-ttu-id="e1a07-105">par [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="e1a07-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="e1a07-106">Tailspin Spyworks montre comment extrêmement simple est de créer des applications puissantes et évolutives pour la plateforme .NET.</span><span class="sxs-lookup"><span data-stu-id="e1a07-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="e1a07-107">Il illustre comment utiliser les nouvelles fonctionnalités dans ASP.NET 4 pour créer un magasin en ligne, y compris les achats, l’extraction et l’administration.</span><span class="sxs-lookup"><span data-stu-id="e1a07-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="e1a07-108">Cette série de didacticiels détaille toutes les mesures prises pour générer l’exemple d’application Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="e1a07-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="e1a07-109">Partie 7 ajoute des fonctionnalités supplémentaires, telles que votre compte, les évaluations de produits et les « éléments populaires » et les contrôles utilisateur « également acheté ».</span><span class="sxs-lookup"><span data-stu-id="e1a07-109">Part 7 adds additional features, such as account review, product reviews, and "popular items" and "also purchased" user controls.</span></span>


## <a id="_Toc260221673"></a>  <span data-ttu-id="e1a07-110">Ajout de fonctionnalités</span><span class="sxs-lookup"><span data-stu-id="e1a07-110">Adding Features</span></span>

<span data-ttu-id="e1a07-111">Bien que les utilisateurs peuvent parcourir notre catalogue, placer des éléments dans son panier d’achat et le processus de validation, il existe qu'un certain nombre de prise en charge des fonctionnalités que nous inclura pour améliorer notre site.</span><span class="sxs-lookup"><span data-stu-id="e1a07-111">Though users can browse our catalog, place items in their shopping cart, and complete the checkout process, there are a number of supporting features that we will include to improve our site.</span></span>

1. <span data-ttu-id="e1a07-112">Examen du compte (liste de commandes placées et afficher les détails.)</span><span class="sxs-lookup"><span data-stu-id="e1a07-112">Account Review (List orders placed and view details.)</span></span>
2. <span data-ttu-id="e1a07-113">Ajouter un contenu spécifique de contexte à la première page.</span><span class="sxs-lookup"><span data-stu-id="e1a07-113">Add some context specific content to the front page.</span></span>
3. <span data-ttu-id="e1a07-114">Ajouter une fonctionnalité pour permettre aux utilisateurs de vérifier les produits dans le catalogue.</span><span class="sxs-lookup"><span data-stu-id="e1a07-114">Add a feature to let users Review the products in the catalog.</span></span>
4. <span data-ttu-id="e1a07-115">Créer un contrôle utilisateur pour afficher les éléments populaires et Place qui contrôlent sur la première page.</span><span class="sxs-lookup"><span data-stu-id="e1a07-115">Create a User Control to display Popular Items and Place that control on the front page.</span></span>
5. <span data-ttu-id="e1a07-116">Créer un contrôle utilisateur « Également acheté » et l’ajouter à la page Détails du produit.</span><span class="sxs-lookup"><span data-stu-id="e1a07-116">Create an "Also Purchased" user control and add it to the product details page.</span></span>
6. <span data-ttu-id="e1a07-117">Ajouter un Contact Page.</span><span class="sxs-lookup"><span data-stu-id="e1a07-117">Add a Contact Page.</span></span>
7. <span data-ttu-id="e1a07-118">Ajouter un sur la Page.</span><span class="sxs-lookup"><span data-stu-id="e1a07-118">Add an About Page.</span></span>
8. <span data-ttu-id="e1a07-119">Erreur globale</span><span class="sxs-lookup"><span data-stu-id="e1a07-119">Global Error</span></span>

## <a id="_Toc260221674"></a>  <span data-ttu-id="e1a07-120">Votre compte</span><span class="sxs-lookup"><span data-stu-id="e1a07-120">Account Review</span></span>

<span data-ttu-id="e1a07-121">Dans le dossier « Compte », créez deux pages .aspx OrderList.aspx nommé un et l’autre OrderDetails.aspx nommé</span><span class="sxs-lookup"><span data-stu-id="e1a07-121">In the "Account" folder create two .aspx pages one named OrderList.aspx and the other named OrderDetails.aspx</span></span>

<span data-ttu-id="e1a07-122">OrderList.aspx reposera sur les contrôles GridView et EntityDataSoure autant que nous avons précédemment.</span><span class="sxs-lookup"><span data-stu-id="e1a07-122">OrderList.aspx will leverage the GridView and EntityDataSoure controls much as we have previously.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

<span data-ttu-id="e1a07-123">Le EntityDataSoure sélectionne les enregistrements de la table Orders filtrée sur le nom d’utilisateur (voir le WhereParameter) qui nous défini dans une variable de session lorsque le journal de l’utilisateur 's.</span><span class="sxs-lookup"><span data-stu-id="e1a07-123">The EntityDataSoure selects records from the Orders table filtered on the UserName (see the WhereParameter) which we set in a session variable when the user log's in.</span></span>

<span data-ttu-id="e1a07-124">Notez également ces paramètres dans le HyperlinkField du contrôle GridView :</span><span class="sxs-lookup"><span data-stu-id="e1a07-124">Note also these parameters in the HyperlinkField of the GridView:</span></span>

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

<span data-ttu-id="e1a07-125">Permet de spécifier le lien vers la vue Détails de commande pour chaque produit en spécifiant le champ OrderID comme un paramètre de chaîne de requête à la page OrderDetails.aspx.</span><span class="sxs-lookup"><span data-stu-id="e1a07-125">These specify the link to the Order details view for each product specifying the OrderID field as a QueryString parameter to the OrderDetails.aspx page.</span></span>

## <a id="_Toc260221675"></a>  <span data-ttu-id="e1a07-126">OrderDetails.aspx</span><span class="sxs-lookup"><span data-stu-id="e1a07-126">OrderDetails.aspx</span></span>

<span data-ttu-id="e1a07-127">Nous allons utiliser un contrôle EntityDataSource pour accéder aux commandes et un FormView pour afficher les données de commande et un autre EntityDataSource avec un GridView pour afficher de toutes les commandes lignes.</span><span class="sxs-lookup"><span data-stu-id="e1a07-127">We will use an EntityDataSource control to access the Orders and a FormView to display the Order data and another EntityDataSource with a GridView to display all the Order's line items.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

<span data-ttu-id="e1a07-128">Dans le fichier Code-Behind (OrderDetails.aspx.cs), nous avons deux bits peu de ménage.</span><span class="sxs-lookup"><span data-stu-id="e1a07-128">In the Code Behind file (OrderDetails.aspx.cs) we have two little bits of housekeeping.</span></span>

<span data-ttu-id="e1a07-129">Nous devons d’abord vous assurer que OrderDetails obtient toujours un OrderId.</span><span class="sxs-lookup"><span data-stu-id="e1a07-129">First we need to make sure that OrderDetails always gets an OrderId.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

<span data-ttu-id="e1a07-130">Nous devons également calculer et afficher le total à partir des éléments de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="e1a07-130">We also need to calculate and display the order total from the line items.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>  <span data-ttu-id="e1a07-131">La Page d’accueil</span><span class="sxs-lookup"><span data-stu-id="e1a07-131">The Home Page</span></span>

<span data-ttu-id="e1a07-132">Vous allez ajouter du contenu statique à la page Default.aspx.</span><span class="sxs-lookup"><span data-stu-id="e1a07-132">Let's add some static content to the Default.aspx page.</span></span>

<span data-ttu-id="e1a07-133">Tout d’abord, je vais créer un dossier « Content » et qu’il contient un dossier Images (et je vais inclure une image à utiliser sur la page d’accueil.)</span><span class="sxs-lookup"><span data-stu-id="e1a07-133">First I'll create a "Content" folder and within it an Images folder (and I'll include an image to be used on the home page.)</span></span>

<span data-ttu-id="e1a07-134">Dans l’espace réservé en bas de la page Default.aspx, ajoutez le balisage suivant.</span><span class="sxs-lookup"><span data-stu-id="e1a07-134">Into the bottom placeholder of the Default.aspx page, add the following markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>  <span data-ttu-id="e1a07-135">Évaluations de produits</span><span class="sxs-lookup"><span data-stu-id="e1a07-135">Product Reviews</span></span>

<span data-ttu-id="e1a07-136">Tout d’abord, nous allons ajouter un bouton avec un lien à un formulaire que nous pouvons utiliser pour entrer une évaluation de produit.</span><span class="sxs-lookup"><span data-stu-id="e1a07-136">First we'll add a button with a link to a form that we can use to enter a product review.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

<span data-ttu-id="e1a07-137">Notez que nous passons ProductID dans la chaîne de requête</span><span class="sxs-lookup"><span data-stu-id="e1a07-137">Note that we are passing the ProductID in the query string</span></span>

<span data-ttu-id="e1a07-138">Suivant nous allons ajouter une page nommée ReviewAdd.aspx</span><span class="sxs-lookup"><span data-stu-id="e1a07-138">Next let's add page named ReviewAdd.aspx</span></span>

<span data-ttu-id="e1a07-139">Cette page utilise les outils de contrôle ASP.NET AJAX.</span><span class="sxs-lookup"><span data-stu-id="e1a07-139">This page will use the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="e1a07-140">Si vous n'avez pas déjà fait donc vous pouvez le télécharger à partir de [DevExpress](http://devexpress.com/act) et il existe des instructions sur la configuration de la boîte à outils à utiliser avec Visual Studio ici [ https://www.asp.net/learn/ajax-videos/video-76.aspx ](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span><span class="sxs-lookup"><span data-stu-id="e1a07-140">If you have not already done so you can download it from [DevExpress](http://devexpress.com/act) and there is guidance on setting up the toolkit for use with Visual Studio here [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span></span>

<span data-ttu-id="e1a07-141">En mode Création, faites glisser des contrôles et des programmes de validation à partir de la boîte à outils et créer un formulaire semblable à celui ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="e1a07-141">In design mode, drag controls and validators from the toolbox and build a form like the one below.</span></span>

![](tailspin-spyworks-part-7/_static/image2.jpg)

<span data-ttu-id="e1a07-142">Le balisage ressemblera à ceci.</span><span class="sxs-lookup"><span data-stu-id="e1a07-142">The markup will look something like this.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

<span data-ttu-id="e1a07-143">Maintenant que nous pouvons entrer revues, permet d’afficher les révisions sur la page du produit.</span><span class="sxs-lookup"><span data-stu-id="e1a07-143">Now that we can enter reviews, lets display those reviews on the product page.</span></span>

<span data-ttu-id="e1a07-144">Ajoutez ce balisage à la page ProductDetails.aspx.</span><span class="sxs-lookup"><span data-stu-id="e1a07-144">Add this markup to the ProductDetails.aspx page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

<span data-ttu-id="e1a07-145">Notre application est maintenant en cours d’exécution et en accédant à un produit affiche les informations de produit, y compris les révisions du client.</span><span class="sxs-lookup"><span data-stu-id="e1a07-145">Running our application now and navigating to a product shows the product information including customer reviews.</span></span>

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>  <span data-ttu-id="e1a07-146">Contrôle des éléments populaires (création de contrôles utilisateur)</span><span class="sxs-lookup"><span data-stu-id="e1a07-146">Popular Items Control (Creating User Controls)</span></span>

<span data-ttu-id="e1a07-147">Afin d’augmenter les ventes sur votre site web, nous allons ajouter quelques fonctionnalités « donneur d’ordre suggérées » des produits populaires ou connexes.</span><span class="sxs-lookup"><span data-stu-id="e1a07-147">In order to increase sales on your web site we will add a couple of features to "suggestive sell" popular or related products.</span></span>

<span data-ttu-id="e1a07-148">La première de ces fonctions sera une liste du produit dans notre catalogue de produits les plus populaires.</span><span class="sxs-lookup"><span data-stu-id="e1a07-148">The first of these features will be a list of the more popular product in our product catalog.</span></span>

<span data-ttu-id="e1a07-149">Nous allons créer un « contrôle utilisateur » pour afficher les meilleurs éléments sur la page d’accueil de notre application.</span><span class="sxs-lookup"><span data-stu-id="e1a07-149">We will create a "User Control" to display the top selling items on the home page of our application.</span></span> <span data-ttu-id="e1a07-150">Dans la mesure où il s’agit d’un contrôle, nous pouvons l’utiliser sur n’importe quelle page par simplement glisser -déplacer le contrôle dans le Concepteur de Visual Studio sur n’importe quelle page qui nous convient.</span><span class="sxs-lookup"><span data-stu-id="e1a07-150">Since this will be a control, we can use it on any page by simply dragging and dropping the control in Visual Studio's designer onto any page that we like.</span></span>

<span data-ttu-id="e1a07-151">Dans l’Explorateur de solutions de Visual Studio, avec le bouton droit sur le nom de la solution et créer un nouveau répertoire nommé « Contrôles ».</span><span class="sxs-lookup"><span data-stu-id="e1a07-151">In Visual Studio's solutions explorer, right-click on the solution name and create a new directory named "Controls".</span></span> <span data-ttu-id="e1a07-152">Il n’est pas nécessaire pour ce faire, nous permettent de conserver notre projet organisé en créant tous les contrôles utilisateur dans le répertoire « Contrôles ».</span><span class="sxs-lookup"><span data-stu-id="e1a07-152">While it is not necessary to do so, we will help keep our project organized by creating all our user controls in the "Controls" directory.</span></span>

<span data-ttu-id="e1a07-153">Avec le bouton droit sur le dossier de contrôles et sélectionnez « Nouvel élément » :</span><span class="sxs-lookup"><span data-stu-id="e1a07-153">Right-click on the controls folder and choose "New Item" :</span></span>

![](tailspin-spyworks-part-7/_static/image4.jpg)

<span data-ttu-id="e1a07-154">Spécifiez un nom pour notre contrôle de « PopularItems ».</span><span class="sxs-lookup"><span data-stu-id="e1a07-154">Specify a name for our control of "PopularItems".</span></span> <span data-ttu-id="e1a07-155">Notez que l’extension de fichier pour les contrôles utilisateur est .ascx pas .aspx.</span><span class="sxs-lookup"><span data-stu-id="e1a07-155">Note that the file extension for user controls is .ascx not .aspx.</span></span>

<span data-ttu-id="e1a07-156">Notre contrôle utilisateurs éléments courants sera défini comme suit.</span><span class="sxs-lookup"><span data-stu-id="e1a07-156">Our Popular Items User control will be defined as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

<span data-ttu-id="e1a07-157">Ici, nous utilisons une méthode que nous n'avons pas encore utilisé dans cette application.</span><span class="sxs-lookup"><span data-stu-id="e1a07-157">Here we're using a method we have not used yet in this application.</span></span> <span data-ttu-id="e1a07-158">Nous utilisons le contrôle du répéteur et au lieu d’utiliser un contrôle de source de données, nous nous lions le contrôle du répéteur aux résultats d’une requête LINQ to Entities.</span><span class="sxs-lookup"><span data-stu-id="e1a07-158">We're using the repeater control and instead of using a data source control we're binding the Repeater Control to the results of a LINQ to Entities query.</span></span>

<span data-ttu-id="e1a07-159">Dans le code-behind de notre contrôle nous le faire comme suit.</span><span class="sxs-lookup"><span data-stu-id="e1a07-159">In the code behind of our control we do that as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

<span data-ttu-id="e1a07-160">Notez également cette ligne importante en haut de la balise de notre contrôle.</span><span class="sxs-lookup"><span data-stu-id="e1a07-160">Note also this important line at the top of our control's markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

<span data-ttu-id="e1a07-161">Étant donné que les éléments les plus populaires ne changer selon les minutes, nous pouvons ajouter une directive aching pour améliorer les performances de votre application.</span><span class="sxs-lookup"><span data-stu-id="e1a07-161">Since the most popular items won't be changing on a minute to minute basis we can add a aching directive to improve the performance of our application.</span></span> <span data-ttu-id="e1a07-162">Cette directive amène le code de contrôles doit uniquement être exécutée lors de la sortie mise en cache du contrôle.</span><span class="sxs-lookup"><span data-stu-id="e1a07-162">This directive will cause the controls code to only be executed when the cached output of the control expires.</span></span> <span data-ttu-id="e1a07-163">Dans le cas contraire, la version mise en cache de la sortie du contrôle servira.</span><span class="sxs-lookup"><span data-stu-id="e1a07-163">Otherwise, the cached version of the control's output will be used.</span></span>

<span data-ttu-id="e1a07-164">À présent que nous devons faire inclure notre nouveau contrôle dans notre page Default.aspc.</span><span class="sxs-lookup"><span data-stu-id="e1a07-164">Now all we have to do is include our new control in our Default.aspc page.</span></span>

<span data-ttu-id="e1a07-165">Utilisation et faites-la glisser pour placer une instance du contrôle dans la colonne ouvrir de notre formulaire par défaut.</span><span class="sxs-lookup"><span data-stu-id="e1a07-165">Use drag and drop to place an instance of the control in the open column of our Default form.</span></span>

![](tailspin-spyworks-part-7/_static/image5.jpg)

<span data-ttu-id="e1a07-166">Quand nous exécutons notre application, la page d’accueil affiche maintenant les éléments les plus populaires.</span><span class="sxs-lookup"><span data-stu-id="e1a07-166">Now when we run our application the home page displays the most popular items.</span></span>

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>  <span data-ttu-id="e1a07-167">« Également acheté » contrôle (contrôles utilisateur avec des paramètres)</span><span class="sxs-lookup"><span data-stu-id="e1a07-167">"Also Purchased" Control (User Controls with Parameters)</span></span>

<span data-ttu-id="e1a07-168">Le deuxième contrôle utilisateur que nous allons créer prendra suggéré de vente au niveau suivant en ajoutant la spécificité de contexte.</span><span class="sxs-lookup"><span data-stu-id="e1a07-168">The second User Control that we'll create will take suggestive selling to the next level by adding context specificity.</span></span>

<span data-ttu-id="e1a07-169">La logique pour calculer les premiers éléments « Également acheté » est non trivial.</span><span class="sxs-lookup"><span data-stu-id="e1a07-169">The logic to calculate the top "Also Purchased" items is non-trivial.</span></span>

<span data-ttu-id="e1a07-170">Notre contrôle « Également acheté » sélectionne les enregistrements OrderDetails (précédemment achetés) pour ProductID actuellement sélectionné et saisissez les OrderIDs pour chaque commande unique est trouvé.</span><span class="sxs-lookup"><span data-stu-id="e1a07-170">Our "Also Purchased" control will select the OrderDetails records (previously purchased) for the currently selected ProductID and grab the OrderIDs for each unique order that is found.</span></span>

<span data-ttu-id="e1a07-171">Ensuite, sélectionnez tous les produits à partir de toutes ces commandes et somme de quantités achetées.</span><span class="sxs-lookup"><span data-stu-id="e1a07-171">Then we will select al the products from all those Orders and sum the quantities purchased.</span></span> <span data-ttu-id="e1a07-172">Nous trier les produits par cette somme de la quantité et afficher les éléments de cinq.</span><span class="sxs-lookup"><span data-stu-id="e1a07-172">We'll sort the products by that quantity sum and display the top five items.</span></span>

<span data-ttu-id="e1a07-173">Étant donné la complexité de cette logique, nous implémenterons cet algorithme en tant qu’une procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="e1a07-173">Given the complexity of this logic, we will implement this algorithm as a stored procedure.</span></span>

<span data-ttu-id="e1a07-174">Le code T-SQL pour la procédure stockée est la suivante.</span><span class="sxs-lookup"><span data-stu-id="e1a07-174">The T-SQL for the stored procedure is as follows.</span></span>

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

<span data-ttu-id="e1a07-175">Notez que cette procédure stockée (SelectPurchasedWithProducts) existe dans la base de données au moment où nous avons inclus dans notre application, lorsque nous avons généré l’Entity Data Model que nous avons spécifié, en plus des Tables et vues il est nécessaire, l’Entity Data Model doit inclure cette procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="e1a07-175">Note that this stored procedure (SelectPurchasedWithProducts) existed in the database when we included it in our application and when we generated the Entity Data Model we specified that, in addition to the Tables and Views that we needed, the Entity Data Model should include this stored procedure.</span></span>

<span data-ttu-id="e1a07-176">Pour accéder à la procédure stockée à partir de l’Entity Data Model que nous avons besoin d’importer la fonction.</span><span class="sxs-lookup"><span data-stu-id="e1a07-176">To access the stored procedure from the Entity Data Model we need to import the function.</span></span>

<span data-ttu-id="e1a07-177">Double-cliquez sur l’Entity Data Model dans l’Explorateur de Solutions pour l’ouvrir dans le concepteur et ouvrez l’Explorateur de modèles, puis avec le bouton droit dans le concepteur et sélectionnez « Ajouter une fonction importation ».</span><span class="sxs-lookup"><span data-stu-id="e1a07-177">Double Click on the Entity Data Model in the Solutions Explorer to open it in the designer and open the Model Browser, then right-click in the designer and select "Add Function Import".</span></span>

![](tailspin-spyworks-part-7/_static/image1.png)

<span data-ttu-id="e1a07-178">Ainsi, cette boîte de dialogue s’ouvre.</span><span class="sxs-lookup"><span data-stu-id="e1a07-178">Doing so will open this dialog.</span></span>

![](tailspin-spyworks-part-7/_static/image2.png)

<span data-ttu-id="e1a07-179">Renseignez les champs comme vous le constater ci-dessus, en sélectionnant la « SelectPurchasedWithProducts » et utilisez le nom de la procédure pour le nom de la fonction importée.</span><span class="sxs-lookup"><span data-stu-id="e1a07-179">Fill out the fields as you see above, selecting the "SelectPurchasedWithProducts" and use the procedure name for the name of our imported function.</span></span>

<span data-ttu-id="e1a07-180">Cliquez sur « Ok ».</span><span class="sxs-lookup"><span data-stu-id="e1a07-180">Click "Ok".</span></span>

<span data-ttu-id="e1a07-181">Avoir fait cela que nous pouvons programmer simplement la procédure stockée comme il peut être n’importe quel autre élément dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="e1a07-181">Having done this we can simply program against the stored procedure as we might any other item in the model.</span></span>

<span data-ttu-id="e1a07-182">Par conséquent, dans notre dossier « Contrôles » vous devez créer un contrôle utilisateur nommé AlsoPurchased.ascx.</span><span class="sxs-lookup"><span data-stu-id="e1a07-182">So, in our "Controls" folder create a new user control named AlsoPurchased.ascx.</span></span>

<span data-ttu-id="e1a07-183">Le balisage pour ce contrôle reconnaîtront au contrôle PopularItems.</span><span class="sxs-lookup"><span data-stu-id="e1a07-183">The markup for this control will look very familiar to the PopularItems control.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

<span data-ttu-id="e1a07-184">La différence notable est qui ne sont pas en cache la sortie dans la mesure où l’élément doit être restitué varient par produit.</span><span class="sxs-lookup"><span data-stu-id="e1a07-184">The notable difference is that are not caching the output since the item's to be rendered will differ by product.</span></span>

<span data-ttu-id="e1a07-185">Le ProductId sera « property » pour le contrôle.</span><span class="sxs-lookup"><span data-stu-id="e1a07-185">The ProductId will be a "property" to the control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

<span data-ttu-id="e1a07-186">Dans le Gestionnaire du contrôle événement PreRender nous seau pour effectuer trois actions.</span><span class="sxs-lookup"><span data-stu-id="e1a07-186">In the control's PreRender event handler we eed to do three things.</span></span>

1. <span data-ttu-id="e1a07-187">Assurez-vous que le ProductID est défini.</span><span class="sxs-lookup"><span data-stu-id="e1a07-187">Make sure the ProductID is set.</span></span>
2. <span data-ttu-id="e1a07-188">Voir s’il existe des produits qui ont été achetés avec l’objet actuel.</span><span class="sxs-lookup"><span data-stu-id="e1a07-188">See if there are any products that have been purchased with the current one.</span></span>
3. <span data-ttu-id="e1a07-189">Certains éléments, comme déterminé dans #2 de sortie.</span><span class="sxs-lookup"><span data-stu-id="e1a07-189">Output some items as determined in #2.</span></span>

<span data-ttu-id="e1a07-190">Notez combien il est facile d’appeler la procédure stockée via le modèle.</span><span class="sxs-lookup"><span data-stu-id="e1a07-190">Note how easy it is to call the stored procedure through the model.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

<span data-ttu-id="e1a07-191">Après avoir déterminé qu’il sont « également acheté », nous pouvons simplement lier le répéteur aux résultats retournés par la requête.</span><span class="sxs-lookup"><span data-stu-id="e1a07-191">After determining that there ARE "also purchased" we can simply bind the repeater to the results returned by the query.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

<span data-ttu-id="e1a07-192">S’il n’existait pas tous les éléments « également acheté » nous affichons simplement autres éléments courants de notre catalogue.</span><span class="sxs-lookup"><span data-stu-id="e1a07-192">If there were not any "also purchased" items we'll simply display other popular items from our catalog.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

<span data-ttu-id="e1a07-193">Pour afficher les éléments « Également acheté », ouvrez la page ProductDetails.aspx et faites glisser le contrôle AlsoPurchased à partir de l’Explorateur de Solutions afin qu’il apparaisse à cette position dans le balisage.</span><span class="sxs-lookup"><span data-stu-id="e1a07-193">To view the "Also Purchased" items, open the ProductDetails.aspx page and drag the AlsoPurchased control from the Solutions Explorer so that it appears in this position in the markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

<span data-ttu-id="e1a07-194">Cela créera une référence au contrôle en haut de la page ProductDetails.</span><span class="sxs-lookup"><span data-stu-id="e1a07-194">Doing so will create a reference to the control at the top of the ProductDetails page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

<span data-ttu-id="e1a07-195">Étant donné que le contrôle utilisateur AlsoPurchased nécessite un nombre ProductId, nous allons définir la propriété ProductID de notre contrôle à l’aide d’une instruction Eval par rapport à l’élément de modèle de données en cours de la page.</span><span class="sxs-lookup"><span data-stu-id="e1a07-195">Since the AlsoPurchased user control requires a ProductId number we will set the ProductID property of our control by using an Eval statement against the current data model item of the page.</span></span>

![](tailspin-spyworks-part-7/_static/image3.png)

<span data-ttu-id="e1a07-196">Lorsque nous créons et exécuter maintenant et accédez à un produit, nous voyons les éléments « Également acheté ».</span><span class="sxs-lookup"><span data-stu-id="e1a07-196">When we build and run now and browse to a product we see the "Also Purchased" items.</span></span>

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> <span data-ttu-id="e1a07-197">[Précédent](tailspin-spyworks-part-6.md)
> [Suivant](tailspin-spyworks-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="e1a07-197">[Previous](tailspin-spyworks-part-6.md)
[Next](tailspin-spyworks-part-8.md)</span></span>
