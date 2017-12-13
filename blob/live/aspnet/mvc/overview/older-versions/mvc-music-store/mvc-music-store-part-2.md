---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: "Partie 2 : Contrôleurs | Documents Microsoft"
author: jongalloway
description: "Cette série de didacticiels détaille toutes les mesures prises pour générer l’exemple d’application de magasin de musique ASP.NET MVC. Partie 2 couvre les contrôleurs."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: bdafd751e996e759d516d0fa25b09eff21241ed7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="part-2-controllers"></a><span data-ttu-id="3820c-104">Partie 2 : contrôleurs</span><span class="sxs-lookup"><span data-stu-id="3820c-104">Part 2: Controllers</span></span>
====================
<span data-ttu-id="3820c-105">par [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="3820c-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="3820c-106">Le magasin de musique MVC est une application du didacticiel qui présente et explique étape par étape comment utiliser ASP.NET MVC et Visual Studio pour le développement web.</span><span class="sxs-lookup"><span data-stu-id="3820c-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="3820c-107">Le magasin de musique MVC est une implémentation de magasin exemple léger qui vend des albums de musique en ligne et implémente l’administration de site de base, authentification de l’utilisateur et les fonctionnalités de panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="3820c-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="3820c-108">Cette série de didacticiels détaille toutes les mesures prises pour générer l’exemple d’application de magasin de musique ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="3820c-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="3820c-109">Partie 2 couvre les contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="3820c-109">Part 2 covers Controllers.</span></span>


<span data-ttu-id="3820c-110">Avec les infrastructures web classique, les URL entrantes sont généralement mappés à des fichiers sur disque.</span><span class="sxs-lookup"><span data-stu-id="3820c-110">With traditional web frameworks, incoming URLs are typically mapped to files on disk.</span></span> <span data-ttu-id="3820c-111">Par exemple : une demande pour une URL comme « / Products.aspx » ou « / Products.php » peut être traité par un fichier « Products.aspx » ou « Products.php ».</span><span class="sxs-lookup"><span data-stu-id="3820c-111">For example: a request for a URL like "/Products.aspx" or "/Products.php" might be processed by a "Products.aspx" or "Products.php" file.</span></span>

<span data-ttu-id="3820c-112">Infrastructures Web MVC mappent des URL au code serveur d’une manière légèrement différente.</span><span class="sxs-lookup"><span data-stu-id="3820c-112">Web-based MVC frameworks map URLs to server code in a slightly different way.</span></span> <span data-ttu-id="3820c-113">Au lieu de mapper des URL entrantes aux fichiers, ils mappent à la place des URL aux méthodes sur les classes.</span><span class="sxs-lookup"><span data-stu-id="3820c-113">Instead of mapping incoming URLs to files, they instead map URLs to methods on classes.</span></span> <span data-ttu-id="3820c-114">Ces classes sont appelées « Contrôleurs » et qu’ils sont chargés de traiter les requêtes HTTP entrantes, la gestion des entrées d’utilisateur, la récupération et l’enregistrement de données et déterminer la réponse à envoyer au client (afficher le code HTML, télécharger un fichier, rediriger vers un autre URL, etc.).</span><span class="sxs-lookup"><span data-stu-id="3820c-114">These classes are called "Controllers" and they are responsible for processing incoming HTTP requests, handling user input, retrieving and saving data, and determining the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span>

## <a name="adding-a-homecontroller"></a><span data-ttu-id="3820c-115">Ajout d’un HomeController</span><span class="sxs-lookup"><span data-stu-id="3820c-115">Adding a HomeController</span></span>

<span data-ttu-id="3820c-116">Nous allons commencer notre application de magasin de musique MVC en ajoutant une classe de contrôleur qui gérera les URL pour la page d’accueil de notre site.</span><span class="sxs-lookup"><span data-stu-id="3820c-116">We'll begin our MVC Music Store application by adding a Controller class that will handle URLs to the Home page of our site.</span></span> <span data-ttu-id="3820c-117">Nous respectent les conventions d’affectation de noms par défaut d’ASP.NET MVC et appelez-le HomeController.</span><span class="sxs-lookup"><span data-stu-id="3820c-117">We'll follow the default naming conventions of ASP.NET MVC and call it HomeController.</span></span>

<span data-ttu-id="3820c-118">Cliquez sur le dossier « Contrôleurs » dans l’Explorateur de solutions et sélectionnez « Ajouter », puis le « contrôleur... »</span><span class="sxs-lookup"><span data-stu-id="3820c-118">Right-click the "Controllers" folder within the Solution Explorer and select "Add", and then the "Controller…"</span></span> <span data-ttu-id="3820c-119">commande :</span><span class="sxs-lookup"><span data-stu-id="3820c-119">command:</span></span>

![](mvc-music-store-part-2/_static/image1.jpg)

<span data-ttu-id="3820c-120">La boîte de dialogue « Ajouter un contrôleur » s’affiche.</span><span class="sxs-lookup"><span data-stu-id="3820c-120">This will bring up the "Add Controller" dialog.</span></span> <span data-ttu-id="3820c-121">Nommez le contrôleur « HomeController » et appuyez sur le bouton Ajouter.</span><span class="sxs-lookup"><span data-stu-id="3820c-121">Name the controller "HomeController" and press the Add button.</span></span>

![](mvc-music-store-part-2/_static/image1.png)

<span data-ttu-id="3820c-122">Cela va créer un nouveau fichier, HomeController.cs, avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="3820c-122">This will create a new file, HomeController.cs, with the following code:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

<span data-ttu-id="3820c-123">Pour démarrer aussi simples que possible, nous allons remplacer la méthode de l’Index avec une méthode simple qui retourne simplement une chaîne.</span><span class="sxs-lookup"><span data-stu-id="3820c-123">To start as simply as possible, let's replace the Index method with a simple method that just returns a string.</span></span> <span data-ttu-id="3820c-124">Nous allons effectuer deux modifications :</span><span class="sxs-lookup"><span data-stu-id="3820c-124">We'll make two changes:</span></span>

- <span data-ttu-id="3820c-125">Modifier la méthode pour retourner une chaîne au lieu d’une classe ActionResult</span><span class="sxs-lookup"><span data-stu-id="3820c-125">Change the method to return a string instead of an ActionResult</span></span>
- <span data-ttu-id="3820c-126">Modifier l’instruction return pour retourner « Hello de Home »</span><span class="sxs-lookup"><span data-stu-id="3820c-126">Change the return statement to return "Hello from Home"</span></span>

<span data-ttu-id="3820c-127">La méthode doit maintenant ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="3820c-127">The method should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a><span data-ttu-id="3820c-128">Exécution de l'application</span><span class="sxs-lookup"><span data-stu-id="3820c-128">Running the Application</span></span>

<span data-ttu-id="3820c-129">Maintenant, exécutez le site.</span><span class="sxs-lookup"><span data-stu-id="3820c-129">Now let's run the site.</span></span> <span data-ttu-id="3820c-130">Nous pouvons démarrer notre serveur web et tester le site en utilisant l’une des opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="3820c-130">We can start our web-server and try out the site using any of the following::</span></span>

- <span data-ttu-id="3820c-131">Choisissez l’élément de menu Débogage ⇨ démarrer le débogage</span><span class="sxs-lookup"><span data-stu-id="3820c-131">Choose the Debug ⇨ Start Debugging menu item</span></span>
- <span data-ttu-id="3820c-132">Cliquez sur le bouton de flèche verte dans la barre d’outils![](mvc-music-store-part-2/_static/image2.jpg)</span><span class="sxs-lookup"><span data-stu-id="3820c-132">Click the Green arrow button in the toolbar ![](mvc-music-store-part-2/_static/image2.jpg)</span></span>
- <span data-ttu-id="3820c-133">Utilisez le raccourci clavier, F5.</span><span class="sxs-lookup"><span data-stu-id="3820c-133">Use the keyboard shortcut, F5.</span></span>

<span data-ttu-id="3820c-134">En utilisant l’une des étapes ci-dessus Compiler notre projet et forcer le serveur de développement ASP.NET qui est intégrée à Visual Web Developer pour démarrer.</span><span class="sxs-lookup"><span data-stu-id="3820c-134">Using any of the above steps will compile our project, and then cause the ASP.NET Development Server that is built-into Visual Web Developer to start.</span></span> <span data-ttu-id="3820c-135">Une notification s’affiche dans la partie inférieure de l’écran pour indiquer que le serveur de développement ASP.NET a démarré et affiche le numéro de port qu’il s’exécute sous.</span><span class="sxs-lookup"><span data-stu-id="3820c-135">A notification will appear in the bottom corner of the screen to indicate that the ASP.NET Development Server has started up, and will show the port number that it is running under.</span></span>

![](mvc-music-store-part-2/_static/image2.png)

<span data-ttu-id="3820c-136">Visual Web Developer s’ouvre ensuite automatiquement une fenêtre de navigateur dont l’URL pointe vers votre serveur web.</span><span class="sxs-lookup"><span data-stu-id="3820c-136">Visual Web Developer will then automatically open a browser window whose URL points to our web-server.</span></span> <span data-ttu-id="3820c-137">Cela nous permettra de tester rapidement votre application web :</span><span class="sxs-lookup"><span data-stu-id="3820c-137">This will allow us to quickly try out our web application:</span></span>

![](mvc-music-store-part-2/_static/image3.png)

<span data-ttu-id="3820c-138">OK, ce qui était assez rapide – nous avons créé un nouveau site Web, ajouté une fonction de trois lignes, et nous avons le texte dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="3820c-138">Okay, that was pretty quick – we created a new website, added a three line function, and we've got text in a browser.</span></span> <span data-ttu-id="3820c-139">Pas fusée science, mais il s’agit d’un point de départ.</span><span class="sxs-lookup"><span data-stu-id="3820c-139">Not rocket science, but it's a start.</span></span>

<span data-ttu-id="3820c-140">*Remarque : Visual Web Developer inclut le serveur de développement ASP.NET, qui exécutera votre site Web sur un nombre aléatoire libre « port ». Dans la capture d’écran ci-dessus, le site est en cours d’exécution à `http://localhost:26641/`, afin qu’il utilise le port 26641. Votre numéro de port sera différent. Lorsque nous parlons /Store/Browse like de l’URL de ce didacticiel, qui passe une fois le numéro de port. En supposant qu’un numéro de port de 26641, accédant au magasin/Parcourir signifie accédant à `http://localhost:26641/Store/Browse`.*</span><span class="sxs-lookup"><span data-stu-id="3820c-140">*Note: Visual Web Developer includes the ASP.NET Development Server, which will run your website on a random free "port" number. In the screenshot above, the site is running at `http://localhost:26641/`, so it's using port 26641. Your port number will be different. When we talk about URL's like /Store/Browse in this tutorial, that will go after the port number. Assuming a port number of 26641, browsing to /Store/Browse will mean browsing to `http://localhost:26641/Store/Browse`.*</span></span>

## <a name="adding-a-storecontroller"></a><span data-ttu-id="3820c-141">Ajout d’un StoreController</span><span class="sxs-lookup"><span data-stu-id="3820c-141">Adding a StoreController</span></span>

<span data-ttu-id="3820c-142">Nous avons ajouté un HomeController simple qui implémente la Page d’accueil de notre site.</span><span class="sxs-lookup"><span data-stu-id="3820c-142">We added a simple HomeController that implements the Home Page of our site.</span></span> <span data-ttu-id="3820c-143">Vous allez maintenant ajouter un autre contrôleur que nous allons utiliser pour implémenter les fonctionnalités de navigation de notre magasin de musique.</span><span class="sxs-lookup"><span data-stu-id="3820c-143">Let's now add another controller that we'll use to implement the browsing functionality of our music store.</span></span> <span data-ttu-id="3820c-144">Notre contrôleur magasin prendra en charge trois scénarios :</span><span class="sxs-lookup"><span data-stu-id="3820c-144">Our store controller will support three scenarios:</span></span>

- <span data-ttu-id="3820c-145">Une page de liste des genres de musique dans notre magasin de musique</span><span class="sxs-lookup"><span data-stu-id="3820c-145">A listing page of the music genres in our music store</span></span>
- <span data-ttu-id="3820c-146">Une page de navigation qui répertorie tous les albums de musique dans un genre particulier</span><span class="sxs-lookup"><span data-stu-id="3820c-146">A browse page that lists all of the music albums in a particular genre</span></span>
- <span data-ttu-id="3820c-147">Une page de détails qui affiche des informations sur un album de musique spécifique</span><span class="sxs-lookup"><span data-stu-id="3820c-147">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="3820c-148">Nous allons commencer en ajoutant une nouvelle classe StoreController...</span><span class="sxs-lookup"><span data-stu-id="3820c-148">We'll start by adding a new StoreController class..</span></span> <span data-ttu-id="3820c-149">Si vous n’avez pas encore, arrêter l’exécution de l’application soit en fermant le navigateur ou en sélectionnant l’élément de menu Débogage ⇨ arrêter le débogage.</span><span class="sxs-lookup"><span data-stu-id="3820c-149">If you haven't already, stop running the application either by closing the browser or selecting the Debug ⇨ Stop Debugging menu item.</span></span>

<span data-ttu-id="3820c-150">Maintenant, ajoutez un nouveau StoreController.</span><span class="sxs-lookup"><span data-stu-id="3820c-150">Now add a new StoreController.</span></span> <span data-ttu-id="3820c-151">Comme nous l’avons fait HomeController, nous ferez en cliquant sur le dossier « Contrôleurs » dans l’Explorateur de solutions et en choisissant Ajouter -&gt;élément de menu de contrôleur</span><span class="sxs-lookup"><span data-stu-id="3820c-151">Just like we did with HomeController, we'll do this by right-clicking on the "Controllers" folder within the Solution Explorer and choosing the Add-&gt;Controller menu item</span></span>

![](mvc-music-store-part-2/_static/image4.png)

<span data-ttu-id="3820c-152">Notre nouvelle StoreController a déjà une méthode de « Index ».</span><span class="sxs-lookup"><span data-stu-id="3820c-152">Our new StoreController already has an "Index" method.</span></span> <span data-ttu-id="3820c-153">Nous allons utiliser cette méthode de « Index » à implémenter notre page relative à la liste qui répertorie tous les genres dans notre magasin de musique.</span><span class="sxs-lookup"><span data-stu-id="3820c-153">We'll use this "Index" method to implement our listing page that lists all genres in our music store.</span></span> <span data-ttu-id="3820c-154">Nous allons également ajouter deux autres méthodes pour implémenter les deux autres scénarios nous voulons que notre StoreController à gérer : Parcourir et détails.</span><span class="sxs-lookup"><span data-stu-id="3820c-154">We'll also add two additional methods to implement the two other scenarios we want our StoreController to handle: Browse and Details.</span></span>

<span data-ttu-id="3820c-155">Ces méthodes (Index, parcourir et détails) dans notre contrôleur sont appelées « Contrôleur Actions », et que vous avez déjà vu avec la méthode d’action HomeController.Index (), leur travail est pour répondre aux demandes d’URL et déterminer (en règle générale) de contenu doit être envoyé vers le navigateur ou l’utilisateur qui a appelé l’URL.</span><span class="sxs-lookup"><span data-stu-id="3820c-155">These methods (Index, Browse and Details) within our Controller are called "Controller Actions", and as you've already seen with the HomeController.Index()action method, their job is to respond to URL requests and (generally speaking) determine what content should be sent back to the browser or user that invoked the URL.</span></span>

<span data-ttu-id="3820c-156">Nous allons commencer notre implémentation StoreController en modifiant la méthode theIndex() pour retourner la chaîne « Hello de Store.Index() » et nous allons ajouter des méthodes semblables pour Browse() et Details() :</span><span class="sxs-lookup"><span data-stu-id="3820c-156">We'll start our StoreController implementation by changing theIndex() method to return the string "Hello from Store.Index()" and we'll add similar methods for Browse() and Details():</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

<span data-ttu-id="3820c-157">Exécuter à nouveau le projet et parcourir les URL suivantes :</span><span class="sxs-lookup"><span data-stu-id="3820c-157">Run the project again and browse the following URLs:</span></span>

- <span data-ttu-id="3820c-158">/ Store</span><span class="sxs-lookup"><span data-stu-id="3820c-158">/Store</span></span>
- <span data-ttu-id="3820c-159">/ Magasin/Parcourir</span><span class="sxs-lookup"><span data-stu-id="3820c-159">/Store/Browse</span></span>
- <span data-ttu-id="3820c-160">/ / Détails du magasin</span><span class="sxs-lookup"><span data-stu-id="3820c-160">/Store/Details</span></span>

<span data-ttu-id="3820c-161">L’accès à ces URL pour appeler les méthodes d’action dans notre contrôleur et renvoie des réponses de la chaîne :</span><span class="sxs-lookup"><span data-stu-id="3820c-161">Accessing these URLs will invoke the action methods within our Controller and return string responses:</span></span>

![](mvc-music-store-part-2/_static/image5.png)

<span data-ttu-id="3820c-162">Qui est élevée, mais ce sont des chaînes constantes uniquement.</span><span class="sxs-lookup"><span data-stu-id="3820c-162">That's great, but these are just constant strings.</span></span> <span data-ttu-id="3820c-163">Nous allons les rendre dynamique, afin de prendre des informations à partir de l’URL et afficher dans la sortie de page.</span><span class="sxs-lookup"><span data-stu-id="3820c-163">Let's make them dynamic, so they take information from the URL and display it in the page output.</span></span>

<span data-ttu-id="3820c-164">Tout d’abord, nous allons modifier la méthode d’action Parcourir pour récupérer une valeur de chaîne de requête à partir de l’URL.</span><span class="sxs-lookup"><span data-stu-id="3820c-164">First we'll change the Browse action method to retrieve a querystring value from the URL.</span></span> <span data-ttu-id="3820c-165">Pour cela, nous pouvons ajouter un paramètre de « genre » à la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="3820c-165">We can do this by adding a "genre" parameter to our action method.</span></span> <span data-ttu-id="3820c-166">Lorsque cela, nous ASP.NET MVC passe automatiquement les paramètres de publication de chaîne de requête ou un formulaire nommés « genre » à la méthode d’action lorsqu’il est appelé.</span><span class="sxs-lookup"><span data-stu-id="3820c-166">When we do this ASP.NET MVC will automatically pass any querystring or form post parameters named "genre" to our action method when it is invoked.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

<span data-ttu-id="3820c-167">*Remarque : Nous utilisons la méthode utilitaire HttpUtility.HtmlEncode expurgation de l’entrée d’utilisateur. Cela empêche les utilisateurs à partir de Javascript injectant notre vue avec un lien comme /Store/Browse ? Genre =&lt;script&gt;window.location= 'http://hackersite.com'&lt;/script&gt;.*</span><span class="sxs-lookup"><span data-stu-id="3820c-167">*Note: We're using the HttpUtility.HtmlEncode utility method to sanitize the user input. This prevents users from injecting Javascript into our View with a link like /Store/Browse?Genre=&lt;script&gt;window.location='http://hackersite.com'&lt;/script&gt;.*</span></span>

<span data-ttu-id="3820c-168">Maintenant nous allons accédez au magasin/Parcourir ? Genre = Disco</span><span class="sxs-lookup"><span data-stu-id="3820c-168">Now let's browse to /Store/Browse?Genre=Disco</span></span>

![](mvc-music-store-part-2/_static/image6.png)

<span data-ttu-id="3820c-169">Nous allons ensuite de modifier l’action de détails pour lire et afficher un paramètre d’entrée nommé ID.</span><span class="sxs-lookup"><span data-stu-id="3820c-169">Let's next change the Details action to read and display an input parameter named ID.</span></span> <span data-ttu-id="3820c-170">Contrairement à la méthode précédente, nous ne sera pas incorporer la valeur d’ID comme paramètre de chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="3820c-170">Unlike our previous method, we won't be embedding the ID value as a querystring parameter.</span></span> <span data-ttu-id="3820c-171">Nous allons à la place l’incorporer directement dans l’URL proprement dite.</span><span class="sxs-lookup"><span data-stu-id="3820c-171">Instead we'll embed it directly within the URL itself.</span></span> <span data-ttu-id="3820c-172">Par exemple : /Store/Details/5.</span><span class="sxs-lookup"><span data-stu-id="3820c-172">For example: /Store/Details/5.</span></span>

<span data-ttu-id="3820c-173">ASP.NET MVC permet de facilement effectuer cette opération sans avoir à configurer quoi que ce soit.</span><span class="sxs-lookup"><span data-stu-id="3820c-173">ASP.NET MVC lets us easily do this without having to configure anything.</span></span> <span data-ttu-id="3820c-174">Convention de routage par défaut de ASP.NET MVC est à traiter le segment d’URL après le nom de la méthode d’action comme un paramètre nommé « ID ».</span><span class="sxs-lookup"><span data-stu-id="3820c-174">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named "ID".</span></span> <span data-ttu-id="3820c-175">Si votre méthode d’action possède un paramètre nommé ID puis ASP.NET MVC sera automatiquement passer le segment d’URL pour vous en tant que paramètre.</span><span class="sxs-lookup"><span data-stu-id="3820c-175">If your action method has a parameter named ID then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

<span data-ttu-id="3820c-176">Exécutez l’application et accédez à /Store/Details/5 :</span><span class="sxs-lookup"><span data-stu-id="3820c-176">Run the application and browse to /Store/Details/5:</span></span>

![](mvc-music-store-part-2/_static/image7.png)

<span data-ttu-id="3820c-177">Récapitulons ce que nous avons faites jusqu'à présent :</span><span class="sxs-lookup"><span data-stu-id="3820c-177">Let's recap what we've done so far:</span></span>

- <span data-ttu-id="3820c-178">Nous avons créé un nouveau projet ASP.NET MVC dans Visual Web Developer</span><span class="sxs-lookup"><span data-stu-id="3820c-178">We've created a new ASP.NET MVC project in Visual Web Developer</span></span>
- <span data-ttu-id="3820c-179">Nous avons décrit la structure de dossiers de base d’une application ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="3820c-179">We've discussed the basic folder structure of an ASP.NET MVC application</span></span>
- <span data-ttu-id="3820c-180">Nous avons appris comment exécuter notre site Web à l’aide du serveur de développement ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3820c-180">We've learned how to run our website using the ASP.NET Development Server</span></span>
- <span data-ttu-id="3820c-181">Nous avons créé deux classes de contrôleur : un HomeController et un StoreController</span><span class="sxs-lookup"><span data-stu-id="3820c-181">We've created two Controller classes: a HomeController and a StoreController</span></span>
- <span data-ttu-id="3820c-182">Nous avons ajouté des méthodes d’Action à nos contrôleurs qui répondent aux demandes d’URL et retournent du texte dans le navigateur</span><span class="sxs-lookup"><span data-stu-id="3820c-182">We've added Action Methods to our controllers which respond to URL requests and return text to the browser</span></span>


>[!div class="step-by-step"]
<span data-ttu-id="3820c-183">[Précédent](mvc-music-store-part-1.md)
[Suivant](mvc-music-store-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="3820c-183">[Previous](mvc-music-store-part-1.md)
[Next](mvc-music-store-part-3.md)</span></span>
