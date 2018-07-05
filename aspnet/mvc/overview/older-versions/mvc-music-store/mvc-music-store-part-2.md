---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 'Partie 2 : Contrôleurs | Microsoft Docs'
author: jongalloway
description: Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application ASP.NET MVC Music Store. Partie 2 couvre les contrôleurs.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: a854feff675cae302b9927d209808257b7d087cb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381801"
---
<a name="part-2-controllers"></a><span data-ttu-id="a8b8a-104">Partie 2 : contrôleurs</span><span class="sxs-lookup"><span data-stu-id="a8b8a-104">Part 2: Controllers</span></span>
====================
<span data-ttu-id="a8b8a-105">par [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="a8b8a-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="a8b8a-106">Le Store de musique MVC est une application du didacticiel qui présente et explique étape par étape comment utiliser ASP.NET MVC et Visual Studio pour le développement web.</span><span class="sxs-lookup"><span data-stu-id="a8b8a-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="a8b8a-107">Le Store de musique MVC est une implémentation de magasin d’exemple léger qui vend des albums de musique en ligne et implémente l’administration de site de base, connexion de l’utilisateur et les fonctionnalités de panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="a8b8a-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="a8b8a-108">Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="a8b8a-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="a8b8a-109">Partie 2 couvre les contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="a8b8a-109">Part 2 covers Controllers.</span></span>


<span data-ttu-id="a8b8a-110">Avec les infrastructures web traditionnelles, les URL entrantes sont généralement mappés à des fichiers sur disque.</span><span class="sxs-lookup"><span data-stu-id="a8b8a-110">With traditional web frameworks, incoming URLs are typically mapped to files on disk.</span></span> <span data-ttu-id="a8b8a-111">Par exemple : une requête pour une URL semblable à « / Products.aspx » ou « /.PHP » peut être traitée par un fichier « Products.aspx » ou «.php ».</span><span class="sxs-lookup"><span data-stu-id="a8b8a-111">For example: a request for a URL like "/Products.aspx" or "/Products.php" might be processed by a "Products.aspx" or "Products.php" file.</span></span>

<span data-ttu-id="a8b8a-112">Les infrastructures MVC Web mappent des URL au code serveur d’une manière légèrement différente.</span><span class="sxs-lookup"><span data-stu-id="a8b8a-112">Web-based MVC frameworks map URLs to server code in a slightly different way.</span></span> <span data-ttu-id="a8b8a-113">Au lieu de mapper des URL entrantes aux fichiers, ils mappent à la place des URL aux méthodes sur les classes.</span><span class="sxs-lookup"><span data-stu-id="a8b8a-113">Instead of mapping incoming URLs to files, they instead map URLs to methods on classes.</span></span> <span data-ttu-id="a8b8a-114">Ces classes sont appelées « Contrôleurs » et ils sont responsables du traitement des requêtes HTTP entrantes, gestion des entrées utilisateur, de récupérer et de l’enregistrement des données et de déterminer la réponse à envoyer au client (afficher le code HTML, télécharger un fichier, rediriger vers un autre URL, etc.).</span><span class="sxs-lookup"><span data-stu-id="a8b8a-114">These classes are called "Controllers" and they are responsible for processing incoming HTTP requests, handling user input, retrieving and saving data, and determining the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span>

## <a name="adding-a-homecontroller"></a><span data-ttu-id="a8b8a-115">Ajout d’un HomeController</span><span class="sxs-lookup"><span data-stu-id="a8b8a-115">Adding a HomeController</span></span>

<span data-ttu-id="a8b8a-116">Nous allons commencer notre application de Store de musique MVC en ajoutant une classe de contrôleur qui gérera les URL pour la page d’accueil de notre site.</span><span class="sxs-lookup"><span data-stu-id="a8b8a-116">We'll begin our MVC Music Store application by adding a Controller class that will handle URLs to the Home page of our site.</span></span> <span data-ttu-id="a8b8a-117">Nous allons suivre les conventions d’affectation de noms par défaut d’ASP.NET MVC et l’appeler HomeController.</span><span class="sxs-lookup"><span data-stu-id="a8b8a-117">We'll follow the default naming conventions of ASP.NET MVC and call it HomeController.</span></span>

<span data-ttu-id="a8b8a-118">Cliquez sur le dossier « Contrôleurs » dans l’Explorateur de solutions et sélectionnez « Ajouter », puis la commande « Contrôleur... » :</span><span class="sxs-lookup"><span data-stu-id="a8b8a-118">Right-click the "Controllers" folder within the Solution Explorer and select "Add", and then the "Controller…" command:</span></span>

![](mvc-music-store-part-2/_static/image1.jpg)

<span data-ttu-id="a8b8a-119">Cela fera apparaître la boîte de dialogue « Ajouter un contrôleur ».</span><span class="sxs-lookup"><span data-stu-id="a8b8a-119">This will bring up the "Add Controller" dialog.</span></span> <span data-ttu-id="a8b8a-120">Nommez le contrôleur « HomeController » et appuyez sur le bouton Ajouter.</span><span class="sxs-lookup"><span data-stu-id="a8b8a-120">Name the controller "HomeController" and press the Add button.</span></span>

![](mvc-music-store-part-2/_static/image1.png)

<span data-ttu-id="a8b8a-121">Cela va créer un nouveau fichier, HomeController.cs, avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a8b8a-121">This will create a new file, HomeController.cs, with the following code:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

<span data-ttu-id="a8b8a-122">Pour démarrer la façon la plus simple que possible, nous allons remplacer la méthode Index avec une méthode simple qui retourne simplement une chaîne.</span><span class="sxs-lookup"><span data-stu-id="a8b8a-122">To start as simply as possible, let's replace the Index method with a simple method that just returns a string.</span></span> <span data-ttu-id="a8b8a-123">Nous allons apporter deux modifications :</span><span class="sxs-lookup"><span data-stu-id="a8b8a-123">We'll make two changes:</span></span>

- <span data-ttu-id="a8b8a-124">Modifier la méthode pour retourner une chaîne au lieu d’une classe ActionResult</span><span class="sxs-lookup"><span data-stu-id="a8b8a-124">Change the method to return a string instead of an ActionResult</span></span>
- <span data-ttu-id="a8b8a-125">Modifier l’instruction return pour retourner « Hello à partir de Home »</span><span class="sxs-lookup"><span data-stu-id="a8b8a-125">Change the return statement to return "Hello from Home"</span></span>

<span data-ttu-id="a8b8a-126">La méthode doit maintenant ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="a8b8a-126">The method should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a><span data-ttu-id="a8b8a-127">Exécution de l'application</span><span class="sxs-lookup"><span data-stu-id="a8b8a-127">Running the Application</span></span>

<span data-ttu-id="a8b8a-128">Maintenant nous allons exécuter le site.</span><span class="sxs-lookup"><span data-stu-id="a8b8a-128">Now let's run the site.</span></span> <span data-ttu-id="a8b8a-129">Nous pouvons démarrer notre serveur web et testez le site en utilisant l’une des opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="a8b8a-129">We can start our web-server and try out the site using any of the following::</span></span>

- <span data-ttu-id="a8b8a-130">Choisissez l’élément de menu Débogage ⇨ démarrer le débogage</span><span class="sxs-lookup"><span data-stu-id="a8b8a-130">Choose the Debug ⇨ Start Debugging menu item</span></span>
- <span data-ttu-id="a8b8a-131">Cliquez sur le bouton de flèche verte dans la barre d’outils ![](mvc-music-store-part-2/_static/image2.jpg)</span><span class="sxs-lookup"><span data-stu-id="a8b8a-131">Click the Green arrow button in the toolbar ![](mvc-music-store-part-2/_static/image2.jpg)</span></span>
- <span data-ttu-id="a8b8a-132">Utilisez le raccourci clavier, F5.</span><span class="sxs-lookup"><span data-stu-id="a8b8a-132">Use the keyboard shortcut, F5.</span></span>

<span data-ttu-id="a8b8a-133">En utilisant l’une des étapes ci-dessus compilera notre projet et forcer le serveur de développement ASP.NET qui est intégrée à Visual Web Developer pour démarrer.</span><span class="sxs-lookup"><span data-stu-id="a8b8a-133">Using any of the above steps will compile our project, and then cause the ASP.NET Development Server that is built-into Visual Web Developer to start.</span></span> <span data-ttu-id="a8b8a-134">Une notification s’affiche dans l’angle inférieur de l’écran pour indiquer que le serveur de développement ASP.NET a démarré et affiche le numéro de port qu’il s’exécute sous.</span><span class="sxs-lookup"><span data-stu-id="a8b8a-134">A notification will appear in the bottom corner of the screen to indicate that the ASP.NET Development Server has started up, and will show the port number that it is running under.</span></span>

![](mvc-music-store-part-2/_static/image2.png)

<span data-ttu-id="a8b8a-135">Visual Web Developer puis ouvre automatiquement une fenêtre de navigateur dont l’URL pointe vers notre serveur web.</span><span class="sxs-lookup"><span data-stu-id="a8b8a-135">Visual Web Developer will then automatically open a browser window whose URL points to our web-server.</span></span> <span data-ttu-id="a8b8a-136">Cela nous permettra de tester rapidement notre application web :</span><span class="sxs-lookup"><span data-stu-id="a8b8a-136">This will allow us to quickly try out our web application:</span></span>

![](mvc-music-store-part-2/_static/image3.png)

<span data-ttu-id="a8b8a-137">OK, ce qui a été assez rapide : nous avons créé un nouveau site Web, ajouté une fonction de trois lignes, et nous avons le texte dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="a8b8a-137">Okay, that was pretty quick – we created a new website, added a three line function, and we've got text in a browser.</span></span> <span data-ttu-id="a8b8a-138">Pas de fusée science, mais c’est un début.</span><span class="sxs-lookup"><span data-stu-id="a8b8a-138">Not rocket science, but it's a start.</span></span>

<span data-ttu-id="a8b8a-139">*Remarque : Visual Web Developer inclut le serveur de développement ASP.NET, qui exécutera votre site Web sur un nombre aléatoire gratuit « port ». Dans la capture d’écran ci-dessus, le site est en cours d’exécution à `http://localhost:26641/`, il utilise le port 26641. Votre numéro de port sera différent. Lorsque nous parlons /Store/Browse like de l’URL dans ce didacticiel, qui passe une fois le numéro de port. En supposant qu’un numéro de port de 26641, accédez à/Store/Parcourir signifiera accédant à `http://localhost:26641/Store/Browse`.*</span><span class="sxs-lookup"><span data-stu-id="a8b8a-139">*Note: Visual Web Developer includes the ASP.NET Development Server, which will run your website on a random free "port" number. In the screenshot above, the site is running at `http://localhost:26641/`, so it's using port 26641. Your port number will be different. When we talk about URL's like /Store/Browse in this tutorial, that will go after the port number. Assuming a port number of 26641, browsing to /Store/Browse will mean browsing to `http://localhost:26641/Store/Browse`.*</span></span>

## <a name="adding-a-storecontroller"></a><span data-ttu-id="a8b8a-140">Ajout d’un StoreController</span><span class="sxs-lookup"><span data-stu-id="a8b8a-140">Adding a StoreController</span></span>

<span data-ttu-id="a8b8a-141">Nous avons ajouté un HomeController simple qui implémente la Page d’accueil de notre site.</span><span class="sxs-lookup"><span data-stu-id="a8b8a-141">We added a simple HomeController that implements the Home Page of our site.</span></span> <span data-ttu-id="a8b8a-142">Nous allons maintenant ajouter un autre contrôleur que nous allons utiliser pour implémenter les fonctionnalités de navigation de notre magasin de musique.</span><span class="sxs-lookup"><span data-stu-id="a8b8a-142">Let's now add another controller that we'll use to implement the browsing functionality of our music store.</span></span> <span data-ttu-id="a8b8a-143">Notre contrôleur store prendra en charge trois scénarios :</span><span class="sxs-lookup"><span data-stu-id="a8b8a-143">Our store controller will support three scenarios:</span></span>

- <span data-ttu-id="a8b8a-144">Une page de liste de genres de musique dans notre magasin de musique</span><span class="sxs-lookup"><span data-stu-id="a8b8a-144">A listing page of the music genres in our music store</span></span>
- <span data-ttu-id="a8b8a-145">Une page de navigation qui répertorie tous les albums de musique d’un genre particulier</span><span class="sxs-lookup"><span data-stu-id="a8b8a-145">A browse page that lists all of the music albums in a particular genre</span></span>
- <span data-ttu-id="a8b8a-146">Une page de détails qui affiche des informations sur un album musical spécifique</span><span class="sxs-lookup"><span data-stu-id="a8b8a-146">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="a8b8a-147">Nous allons commencer en ajoutant une nouvelle classe StoreController...</span><span class="sxs-lookup"><span data-stu-id="a8b8a-147">We'll start by adding a new StoreController class..</span></span> <span data-ttu-id="a8b8a-148">Si vous n’avez pas déjà, arrêtez l’exécution de l’application soit par la fermeture du navigateur ou en sélectionnant l’élément de menu Débogage ⇨ arrêter le débogage.</span><span class="sxs-lookup"><span data-stu-id="a8b8a-148">If you haven't already, stop running the application either by closing the browser or selecting the Debug ⇨ Stop Debugging menu item.</span></span>

<span data-ttu-id="a8b8a-149">Ajoutez maintenant un nouveau StoreController.</span><span class="sxs-lookup"><span data-stu-id="a8b8a-149">Now add a new StoreController.</span></span> <span data-ttu-id="a8b8a-150">Comme nous l’avons fait avec le HomeController, nous aborderons ce en cliquant sur le dossier « Contrôleurs » dans l’Explorateur de solutions et en choisissant Add -&gt;élément de menu de contrôleur</span><span class="sxs-lookup"><span data-stu-id="a8b8a-150">Just like we did with HomeController, we'll do this by right-clicking on the "Controllers" folder within the Solution Explorer and choosing the Add-&gt;Controller menu item</span></span>

![](mvc-music-store-part-2/_static/image4.png)

<span data-ttu-id="a8b8a-151">Notre nouvelle StoreController a déjà une méthode « Index ».</span><span class="sxs-lookup"><span data-stu-id="a8b8a-151">Our new StoreController already has an "Index" method.</span></span> <span data-ttu-id="a8b8a-152">Nous allons utiliser cette méthode « Index » pour implémenter notre page de liste qui répertorie tous les genres dans notre magasin de musique.</span><span class="sxs-lookup"><span data-stu-id="a8b8a-152">We'll use this "Index" method to implement our listing page that lists all genres in our music store.</span></span> <span data-ttu-id="a8b8a-153">Nous allons également ajouter deux méthodes supplémentaires pour implémenter les deux autres scénarios nous voulons que notre StoreController à gérer : Parcourir et détails.</span><span class="sxs-lookup"><span data-stu-id="a8b8a-153">We'll also add two additional methods to implement the two other scenarios we want our StoreController to handle: Browse and Details.</span></span>

<span data-ttu-id="a8b8a-154">Ces méthodes (Index, parcourir et Details) dans notre contrôleur sont appelées « Contrôleur Actions », et que vous avez déjà vu avec la méthode d’action HomeController.Index (), leur travail consiste à répondre aux demandes d’URL et déterminer (en règle générale) le contenu doit être renvoyé au navigateur ou de l’utilisateur qui a appelé l’URL.</span><span class="sxs-lookup"><span data-stu-id="a8b8a-154">These methods (Index, Browse and Details) within our Controller are called "Controller Actions", and as you've already seen with the HomeController.Index()action method, their job is to respond to URL requests and (generally speaking) determine what content should be sent back to the browser or user that invoked the URL.</span></span>

<span data-ttu-id="a8b8a-155">Nous allons commencer notre implémentation StoreController en modifiant la méthode theIndex() pour retourner la chaîne « Hello de Store.Index() » et nous allons ajouter des méthodes similaires pour Browse() et Details() :</span><span class="sxs-lookup"><span data-stu-id="a8b8a-155">We'll start our StoreController implementation by changing theIndex() method to return the string "Hello from Store.Index()" and we'll add similar methods for Browse() and Details():</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

<span data-ttu-id="a8b8a-156">Exécuter à nouveau le projet et parcourir les URL suivantes :</span><span class="sxs-lookup"><span data-stu-id="a8b8a-156">Run the project again and browse the following URLs:</span></span>

- <span data-ttu-id="a8b8a-157">/ Store</span><span class="sxs-lookup"><span data-stu-id="a8b8a-157">/Store</span></span>
- <span data-ttu-id="a8b8a-158">/ Store/Parcourir</span><span class="sxs-lookup"><span data-stu-id="a8b8a-158">/Store/Browse</span></span>
- <span data-ttu-id="a8b8a-159">/ Store/détails</span><span class="sxs-lookup"><span data-stu-id="a8b8a-159">/Store/Details</span></span>

<span data-ttu-id="a8b8a-160">L’accès à ces URL pour appeler les méthodes d’action dans notre contrôleur et renvoyer des réponses de la chaîne :</span><span class="sxs-lookup"><span data-stu-id="a8b8a-160">Accessing these URLs will invoke the action methods within our Controller and return string responses:</span></span>

![](mvc-music-store-part-2/_static/image5.png)

<span data-ttu-id="a8b8a-161">C’est génial, mais il s’agit de chaînes constantes uniquement.</span><span class="sxs-lookup"><span data-stu-id="a8b8a-161">That's great, but these are just constant strings.</span></span> <span data-ttu-id="a8b8a-162">Nous allons rendre dynamique, afin de tirer des informations à partir de l’URL et affichez-la dans la sortie de page.</span><span class="sxs-lookup"><span data-stu-id="a8b8a-162">Let's make them dynamic, so they take information from the URL and display it in the page output.</span></span>

<span data-ttu-id="a8b8a-163">Tout d’abord, nous allons modifier la méthode d’action Parcourir pour récupérer une valeur de chaîne de requête dans l’URL.</span><span class="sxs-lookup"><span data-stu-id="a8b8a-163">First we'll change the Browse action method to retrieve a querystring value from the URL.</span></span> <span data-ttu-id="a8b8a-164">Pour ce faire, nous pouvons ajouter un paramètre de « genre » à notre méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="a8b8a-164">We can do this by adding a "genre" parameter to our action method.</span></span> <span data-ttu-id="a8b8a-165">Lorsque nous faisons cela ASP.NET MVC passe automatiquement les paramètres post chaîne de requête ou un formulaire nommés « genre » à notre méthode d’action lorsqu’il est appelé.</span><span class="sxs-lookup"><span data-stu-id="a8b8a-165">When we do this ASP.NET MVC will automatically pass any querystring or form post parameters named "genre" to our action method when it is invoked.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

<span data-ttu-id="a8b8a-166">*Remarque : Nous utilisons la méthode d’utilitaire HttpUtility.HtmlEncode à assainir la saisie utilisateur. Cela empêche les utilisateurs d’injecter du Javascript dans notre vue avec un lien comme /Store/Browse ? Genre =&lt;script&gt;Window.location = 'http://hackersite.com'&lt;/script&gt;.*</span><span class="sxs-lookup"><span data-stu-id="a8b8a-166">*Note: We're using the HttpUtility.HtmlEncode utility method to sanitize the user input. This prevents users from injecting Javascript into our View with a link like /Store/Browse?Genre=&lt;script&gt;window.location='http://hackersite.com'&lt;/script&gt;.*</span></span>

<span data-ttu-id="a8b8a-167">Maintenant nous allons accédez à/Store/Parcourir ? Genre = Disco</span><span class="sxs-lookup"><span data-stu-id="a8b8a-167">Now let's browse to /Store/Browse?Genre=Disco</span></span>

![](mvc-music-store-part-2/_static/image6.png)

<span data-ttu-id="a8b8a-168">Nous allons ensuite modifier l’action de détails pour lire et afficher un paramètre d’entrée nommé ID.</span><span class="sxs-lookup"><span data-stu-id="a8b8a-168">Let's next change the Details action to read and display an input parameter named ID.</span></span> <span data-ttu-id="a8b8a-169">Contrairement à notre méthode précédente, nous ne sont pas incorporer la valeur d’ID comme paramètre de chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="a8b8a-169">Unlike our previous method, we won't be embedding the ID value as a querystring parameter.</span></span> <span data-ttu-id="a8b8a-170">Nous allons à la place l’incorporer directement dans l’URL proprement dite.</span><span class="sxs-lookup"><span data-stu-id="a8b8a-170">Instead we'll embed it directly within the URL itself.</span></span> <span data-ttu-id="a8b8a-171">Par exemple : /Store/Details/5.</span><span class="sxs-lookup"><span data-stu-id="a8b8a-171">For example: /Store/Details/5.</span></span>

<span data-ttu-id="a8b8a-172">ASP.NET MVC vous permet de nous le faire facilement sans avoir à configurer quoi que ce soit.</span><span class="sxs-lookup"><span data-stu-id="a8b8a-172">ASP.NET MVC lets us easily do this without having to configure anything.</span></span> <span data-ttu-id="a8b8a-173">Convention de routage par défaut de ASP.NET MVC consiste à traiter le segment d’URL après le nom de la méthode d’action en tant que paramètre nommé « ID ».</span><span class="sxs-lookup"><span data-stu-id="a8b8a-173">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named "ID".</span></span> <span data-ttu-id="a8b8a-174">Si votre méthode d’action possède un paramètre nommé ID puis ASP.NET MVC automatiquement passera le segment d’URL pour vous en tant que paramètre.</span><span class="sxs-lookup"><span data-stu-id="a8b8a-174">If your action method has a parameter named ID then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

<span data-ttu-id="a8b8a-175">Exécutez l’application et accédez à /Store/Details/5 :</span><span class="sxs-lookup"><span data-stu-id="a8b8a-175">Run the application and browse to /Store/Details/5:</span></span>

![](mvc-music-store-part-2/_static/image7.png)

<span data-ttu-id="a8b8a-176">Récapitulons ce que nous avons faites jusqu'à présent :</span><span class="sxs-lookup"><span data-stu-id="a8b8a-176">Let's recap what we've done so far:</span></span>

- <span data-ttu-id="a8b8a-177">Nous avons créé un nouveau projet ASP.NET MVC dans Visual Web Developer</span><span class="sxs-lookup"><span data-stu-id="a8b8a-177">We've created a new ASP.NET MVC project in Visual Web Developer</span></span>
- <span data-ttu-id="a8b8a-178">Nous avons vu la structure de dossiers de base d’une application ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="a8b8a-178">We've discussed the basic folder structure of an ASP.NET MVC application</span></span>
- <span data-ttu-id="a8b8a-179">Nous avons appris à exécuter notre site Web à l’aide du serveur de développement ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a8b8a-179">We've learned how to run our website using the ASP.NET Development Server</span></span>
- <span data-ttu-id="a8b8a-180">Nous avons créé deux classes de contrôleur : un HomeController et un StoreController</span><span class="sxs-lookup"><span data-stu-id="a8b8a-180">We've created two Controller classes: a HomeController and a StoreController</span></span>
- <span data-ttu-id="a8b8a-181">Nous avons ajouté des méthodes d’Action à nos contrôleurs qui répondent aux demandes d’URL et retournent du texte dans le navigateur</span><span class="sxs-lookup"><span data-stu-id="a8b8a-181">We've added Action Methods to our controllers which respond to URL requests and return text to the browser</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="a8b8a-182">[Précédent](mvc-music-store-part-1.md)
> [Suivant](mvc-music-store-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="a8b8a-182">[Previous](mvc-music-store-part-1.md)
[Next](mvc-music-store-part-3.md)</span></span>
