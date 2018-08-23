---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'Partie 4 : Accès aux données et des modèles | Microsoft Docs'
author: jongalloway
description: Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application ASP.NET MVC Music Store. Partie 4 couvre l’accès aux données et modèles.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 6a07bf6c8a3fb926ae25fe1f6c9359e64cd7a290
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834391"
---
<a name="part-4-models-and-data-access"></a><span data-ttu-id="4c3ca-104">Partie 4 : Modèles et accès aux données</span><span class="sxs-lookup"><span data-stu-id="4c3ca-104">Part 4: Models and Data Access</span></span>
====================
<span data-ttu-id="4c3ca-105">par [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="4c3ca-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="4c3ca-106">Le Store de musique MVC est une application du didacticiel qui présente et explique étape par étape comment utiliser ASP.NET MVC et Visual Studio pour le développement web.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="4c3ca-107">Le Store de musique MVC est une implémentation de magasin d’exemple léger qui vend des albums de musique en ligne et implémente l’administration de site de base, connexion de l’utilisateur et les fonctionnalités de panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="4c3ca-108">Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="4c3ca-109">Partie 4 couvre l’accès aux données et modèles.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-109">Part 4 covers Models and Data Access.</span></span>


<span data-ttu-id="4c3ca-110">Jusqu’ici, nous avons simplement été passant « données factices » à partir de nos contrôleurs à nos modèles de vue.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-110">So far, we've just been passing "dummy data" from our Controllers to our View templates.</span></span> <span data-ttu-id="4c3ca-111">Nous sommes maintenant prêts à raccorder une véritable base de données.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-111">Now we're ready to hook up a real database.</span></span> <span data-ttu-id="4c3ca-112">Dans ce didacticiel présente l’utilisation de SQL Server Compact Edition (souvent appelée SQL CE) en tant que notre moteur de base de données.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-112">In this tutorial we'll be covering how to use SQL Server Compact Edition (often called SQL CE) as our database engine.</span></span> <span data-ttu-id="4c3ca-113">SQL CE est une base de données de fichier gratuite et intégrée, en ne nécessitant pas installation ni configuration, ce qui facilite réellement la pour un développement local.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-113">SQL CE is a free, embedded, file based database that doesn't require any installation or configuration, which makes it really convenient for local development.</span></span>

## <a name="database-access-with-entity-framework-code-first"></a><span data-ttu-id="4c3ca-114">Accès de base de données avec le Code First Entity Framework</span><span class="sxs-lookup"><span data-stu-id="4c3ca-114">Database access with Entity Framework Code-First</span></span>

<span data-ttu-id="4c3ca-115">Nous allons utiliser la prise en charge Entity Framework (EF) qui est inclus dans les projets ASP.NET MVC 3 pour interroger et mettre à jour de la base de données.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-115">We'll use the Entity Framework (EF) support that is included in ASP.NET MVC 3 projects to query and update the database.</span></span> <span data-ttu-id="4c3ca-116">EF est un objet flexible relationnel de mappage des données (ORM) API qui permet aux développeurs d’interroger et mise à jour des données stockées dans une base de données d’une manière orientée objet.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-116">EF is a flexible object relational mapping (ORM) data API that enables developers to query and update data stored in a database in an object-oriented way.</span></span>

<span data-ttu-id="4c3ca-117">Entity Framework version 4 prend en charge un paradigme de développement appelé code first.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-117">Entity Framework version 4 supports a development paradigm called code-first.</span></span> <span data-ttu-id="4c3ca-118">Code first vous permet de créer un objet de modèle en écrivant des classes simples (appelé aussi POCO à partir de « brut » objets CLR) et pouvez même créer la base de données à la volée à partir de vos classes.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-118">Code-first allows you to create model object by writing simple classes (also known as POCO from "plain-old" CLR objects), and can even create the database on the fly from your classes.</span></span>

### <a name="changes-to-our-model-classes"></a><span data-ttu-id="4c3ca-119">Modifications apportées à nos Classes de modèle</span><span class="sxs-lookup"><span data-stu-id="4c3ca-119">Changes to our Model Classes</span></span>

<span data-ttu-id="4c3ca-120">Dans ce didacticiel, nous va exploiter la fonctionnalité de création de base de données dans Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-120">We will be leveraging the database creation feature in Entity Framework in this tutorial.</span></span> <span data-ttu-id="4c3ca-121">Avant cela, cependant, nous allons apporter quelques modifications mineures à nos classes de modèle à ajouter dans certaines choses que nous utiliserons plus tard.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-121">Before we do that, though, let's make a few minor changes to our model classes to add in some things we'll be using later on.</span></span>

#### <a name="adding-the-artist-model-classes"></a><span data-ttu-id="4c3ca-122">Ajout des Classes de modèle artiste</span><span class="sxs-lookup"><span data-stu-id="4c3ca-122">Adding the Artist Model Classes</span></span>

<span data-ttu-id="4c3ca-123">Notre Albums sera associés à des artistes, donc nous allons ajouter une classe de modèle simple pour décrire un artiste.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-123">Our Albums will be associated with Artists, so we'll add a simple model class to describe an Artist.</span></span> <span data-ttu-id="4c3ca-124">Ajoutez une nouvelle classe dans le dossier de modèles nommé Artist.cs en utilisant le code indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-124">Add a new class to the Models folder named Artist.cs using the code shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a><span data-ttu-id="4c3ca-125">La mise à jour nos Classes de modèle</span><span class="sxs-lookup"><span data-stu-id="4c3ca-125">Updating our Model Classes</span></span>

<span data-ttu-id="4c3ca-126">Mettre à jour la classe Album comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-126">Update the Album class as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

<span data-ttu-id="4c3ca-127">Ensuite, vérifiez les mises à jour suivantes à la classe de Genre.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-127">Next, make the following updates to the Genre class.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a><span data-ttu-id="4c3ca-128">Ajout de l’application\_dossier de données</span><span class="sxs-lookup"><span data-stu-id="4c3ca-128">Adding the App\_Data folder</span></span>

<span data-ttu-id="4c3ca-129">Nous allons ajouter une application\_répertoire de données à notre projet pour stocker nos fichiers de base de données SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-129">We'll add an App\_Data directory to our project to hold our SQL Server Express database files.</span></span> <span data-ttu-id="4c3ca-130">Application\_données sont un répertoire spécial dans ASP.NET qui possède déjà les autorisations d’accès de sécurité correct pour l’accès de base de données.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-130">App\_Data is a special directory in ASP.NET which already has the correct security access permissions for database access.</span></span> <span data-ttu-id="4c3ca-131">Dans le menu projet, sélectionnez Ajouter le dossier ASP.NET, puis application\_données.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-131">From the Project menu, select Add ASP.NET Folder, then App\_Data.</span></span>

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a><span data-ttu-id="4c3ca-132">Création d’une chaîne de connexion dans le fichier web.config</span><span class="sxs-lookup"><span data-stu-id="4c3ca-132">Creating a Connection String in the web.config file</span></span>

<span data-ttu-id="4c3ca-133">Nous allons ajouter quelques lignes au fichier de configuration du site Web afin qu’Entity Framework sache comment vous connecter à notre base de données.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-133">We will add a few lines to the website's configuration file so that Entity Framework knows how to connect to our database.</span></span> <span data-ttu-id="4c3ca-134">Double-cliquez sur le fichier Web.config situé à la racine du projet.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-134">Double-click on the Web.config file located in the root of the project.</span></span>

![](mvc-music-store-part-4/_static/image2.png)

<span data-ttu-id="4c3ca-135">Faites défiler vers le bas de ce fichier et ajoutez un &lt;connectionStrings&gt; section directement au-dessus de la dernière ligne, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-135">Scroll to the bottom of this file and add a &lt;connectionStrings&gt; section directly above the last line, as shown below.</span></span>

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a><span data-ttu-id="4c3ca-136">Ajout d’une classe de contexte</span><span class="sxs-lookup"><span data-stu-id="4c3ca-136">Adding a Context Class</span></span>

<span data-ttu-id="4c3ca-137">Cliquez sur le dossier de modèles et ajoutez une nouvelle classe nommée MusicStoreEntities.cs.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-137">Right-click the Models folder and add a new class named MusicStoreEntities.cs.</span></span>

![](mvc-music-store-part-4/_static/image3.png)

<span data-ttu-id="4c3ca-138">Cette classe sera représentent le contexte de base de données Entity Framework et sera gérer notre créer, lire, mettre à jour et supprimer les opérations pour nous.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-138">This class will represent the Entity Framework database context, and will handle our create, read, update, and delete operations for us.</span></span> <span data-ttu-id="4c3ca-139">Le code de cette classe est indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-139">The code for this class is shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

<span data-ttu-id="4c3ca-140">Et voilà, il n’existe aucune autre configuration, des interfaces spéciales, etc. En étendant la classe de base DbContext, notre classe MusicStoreEntities est en mesure de gérer nos opérations de base de données pour nous.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-140">That's it - there's no other configuration, special interfaces, etc. By extending the DbContext base class, our MusicStoreEntities class is able to handle our database operations for us.</span></span> <span data-ttu-id="4c3ca-141">Maintenant que nous avons raccordé, nous allons ajouter quelques propriétés supplémentaires pour nos classes de modèle pour tirer parti de certaines des informations supplémentaires dans notre base de données.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-141">Now that we've got that hooked up, let's add a few more properties to our model classes to take advantage of some of the additional information in our database.</span></span>

### <a name="adding-our-store-catalog-data"></a><span data-ttu-id="4c3ca-142">Ajout de nos données de catalogue du magasin</span><span class="sxs-lookup"><span data-stu-id="4c3ca-142">Adding our store catalog data</span></span>

<span data-ttu-id="4c3ca-143">Nous tireront parti d’une fonctionnalité dans Entity Framework qui ajoute des données de « valeur initiale » à une base de données nouvellement créé.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-143">We will take advantage of a feature in Entity Framework which adds "seed" data to a newly created database.</span></span> <span data-ttu-id="4c3ca-144">Cela est prédéfinie notre catalogue du magasin avec une liste de Genres, les artistes et les Albums.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-144">This will pre-populate our store catalog with a list of Genres, Artists, and Albums.</span></span> <span data-ttu-id="4c3ca-145">Le téléchargement du MvcMusicStore-Assets.zip - inclus nos fichiers de conception de site utilisés précédemment dans ce didacticiel - dispose d’un fichier de classe avec ces données de valeur initiale, situés dans un dossier nommé Code.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-145">The MvcMusicStore-Assets.zip download - which included our site design files used earlier in this tutorial - has a class file with this seed data, located in a folder named Code.</span></span>

<span data-ttu-id="4c3ca-146">Dans le Code / dossier Modèles, recherchez le fichier SampleData.cs et déposez-le dans le dossier de modèles dans notre projet, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-146">Within the Code / Models folder, locate the SampleData.cs file and drop it into the Models folder in our project, as shown below.</span></span>

![](mvc-music-store-part-4/_static/image4.png)

<span data-ttu-id="4c3ca-147">Nous devons maintenant ajouter une ligne de code pour indiquer à Entity Framework relatives à cette classe SampleData.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-147">Now we need to add one line of code to tell Entity Framework about that SampleData class.</span></span> <span data-ttu-id="4c3ca-148">Double-cliquez sur le fichier Global.asax à la racine du projet pour l’ouvrir et ajoutez la ligne suivante vers le haut l’Application\_Start (méthode).</span><span class="sxs-lookup"><span data-stu-id="4c3ca-148">Double-click on the Global.asax file in the root of the project to open it and add the following line to the top the Application\_Start method.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

<span data-ttu-id="4c3ca-149">À ce stade, nous avons terminé le travail nécessaire pour configurer notre projet Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-149">At this point, we've completed the work necessary to configure Entity Framework for our project.</span></span>

## <a name="querying-the-database"></a><span data-ttu-id="4c3ca-150">Interrogation de la base de données</span><span class="sxs-lookup"><span data-stu-id="4c3ca-150">Querying the Database</span></span>

<span data-ttu-id="4c3ca-151">Maintenant nous allons mettre à jour notre StoreController afin qu’au lieu d’utiliser « factice de données » il appelle à la place dans notre base de données pour interroger toutes ses informations.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-151">Now let's update our StoreController so that instead of using "dummy data" it instead calls into our database to query all of its information.</span></span> <span data-ttu-id="4c3ca-152">Nous allons commencer par déclarer un champ sur le **StoreController** devant contenir une instance de la classe MusicStoreEntities, nommée storeDB :</span><span class="sxs-lookup"><span data-stu-id="4c3ca-152">We'll start by declaring a field on the **StoreController** to hold an instance of the MusicStoreEntities class, named storeDB:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a><span data-ttu-id="4c3ca-153">La mise à jour de l’Index de Store pour interroger la base de données</span><span class="sxs-lookup"><span data-stu-id="4c3ca-153">Updating the Store Index to query the database</span></span>

<span data-ttu-id="4c3ca-154">La classe MusicStoreEntities est gérée par Entity Framework et expose une propriété de collection pour chaque table dans notre base de données.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-154">The MusicStoreEntities class is maintained by the Entity Framework and exposes a collection property for each table in our database.</span></span> <span data-ttu-id="4c3ca-155">Nous allons mettre à jour action sur l’Index de notre StoreController pour récupérer tous les Genres dans notre base de données.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-155">Let's update our StoreController's Index action to retrieve all Genres in our database.</span></span> <span data-ttu-id="4c3ca-156">Précédemment cela, nous avons coder en dur les données de chaîne.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-156">Previously we did this by hard-coding string data.</span></span> <span data-ttu-id="4c3ca-157">Nous pouvons maintenant à la place simplement utiliser le contexte Entity Framework Generes collection :</span><span class="sxs-lookup"><span data-stu-id="4c3ca-157">Now we can instead just use the Entity Framework context Generes collection:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

<span data-ttu-id="4c3ca-158">Aucune modification ne doit se produire pour notre modèle de vue dans la mesure où nous renvoyons toujours le même StoreIndexViewModel retourné avant - nous avons retourne simplement les données en direct à partir de notre base de données maintenant.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-158">No changes need to happen to our View template since we're still returning the same StoreIndexViewModel we returned before - we're just returning live data from our database now.</span></span>

<span data-ttu-id="4c3ca-159">Lorsque nous exécuter à nouveau le projet et accédez à l’URL « / Store », nous verrons maintenant une liste de tous les Genres dans notre base de données :</span><span class="sxs-lookup"><span data-stu-id="4c3ca-159">When we run the project again and visit the "/Store" URL, we'll now see a list of all Genres in our database:</span></span>

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a><span data-ttu-id="4c3ca-160">La mise à jour Store Parcourir et détails à utiliser des données actives</span><span class="sxs-lookup"><span data-stu-id="4c3ca-160">Updating Store Browse and Details to use live data</span></span>

<span data-ttu-id="4c3ca-161">Avec/Store/Parcourir ? genre =*[certains genre]* méthode d’action, nous recherchons un Genre par nom.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-161">With the /Store/Browse?genre=*[some-genre]* action method, we're searching for a Genre by name.</span></span> <span data-ttu-id="4c3ca-162">Nous nous attendons uniquement un seul résultat, étant donné que nous ne devrions pas jamais avoir deux entrées pour le nom du même Genre, et donc nous pouvons utiliser le. Extension de Single() dans LINQ pour interroger l’objet de Genre approprié comme suit (ne tapez pas cela encore) :</span><span class="sxs-lookup"><span data-stu-id="4c3ca-162">We only expect one result, since we shouldn't ever have two entries for the same Genre name, and so we can use the .Single() extension in LINQ to query for the appropriate Genre object like this (don't type this yet):</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

<span data-ttu-id="4c3ca-163">La méthode unique prend une expression Lambda en tant que paramètre, qui spécifie que nous voulons un seul objet de Genre de telle sorte que son nom correspond à la valeur que nous avons défini.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-163">The Single method takes a Lambda expression as a parameter, which specifies that we want a single Genre object such that its name matches the value we've defined.</span></span> <span data-ttu-id="4c3ca-164">Dans le cas ci-dessus, nous chargeons un objet de Genre unique avec une valeur de nom correspondant Disco.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-164">In the case above, we are loading a single Genre object with a Name value matching Disco.</span></span>

<span data-ttu-id="4c3ca-165">Nous allons tirer parti d’une fonctionnalité Entity Framework qui nous permet d’indiquer d’autres entités connexes que nous voulons également chargés lors de l’objet de Genre est récupéré.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-165">We'll take advantage of an Entity Framework feature that allows us to indicate other related entities we want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="4c3ca-166">Cette fonctionnalité est appelée mise en forme du résultat de requête et nous permet de réduire le nombre de fois où que nous devons accéder à la base de données pour récupérer toutes les informations que nous avons besoin.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-166">This feature is called Query Result Shaping, and enables us to reduce the number of times we need to access the database to retrieve all of the information we need.</span></span> <span data-ttu-id="4c3ca-167">Nous souhaitons Prérécupérer des Albums de Genre nous récupérons, donc nous allons mettre à jour notre requête à inclure à partir de Genres.Include("Albums") pour indiquer que nous voulons également les albums connexes.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-167">We want to pre-fetch the Albums for Genre we retrieve, so we'll update our query to include from Genres.Include("Albums") to indicate that we want related albums as well.</span></span> <span data-ttu-id="4c3ca-168">Cela est plus efficace, dans la mesure où il récupérera notre Genre et l’Album des données dans une requête de base de données unique.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-168">This is more efficient, since it will retrieve both our Genre and Album data in a single database request.</span></span>

<span data-ttu-id="4c3ca-169">Avec les explications disparaître, voici à quoi ressemble notre action de contrôleur Parcourir mis à jour :</span><span class="sxs-lookup"><span data-stu-id="4c3ca-169">With the explanations out of the way, here's how our updated Browse controller action looks:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

<span data-ttu-id="4c3ca-170">Nous pouvons maintenant mettre à jour le Store parcourir la vue pour afficher les albums qui sont disponibles dans chaque Genre.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-170">We can now update the Store Browse View to display the albums which are available in each Genre.</span></span> <span data-ttu-id="4c3ca-171">Ouvrez le modèle de vue (/Views/Store/Browse.cshtml trouvé dans) et ajoutez une liste à puces d’Albums, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-171">Open the view template (found in /Views/Store/Browse.cshtml) and add a bulleted list of Albums as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

<span data-ttu-id="4c3ca-172">Notre application en cours d’exécution et en accédant à/Store/Parcourir ? genre = montre de Jazz que nos résultats sont maintenant chargés à partir de la base de données, affichant tous les albums dans notre Genre sélectionné.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-172">Running our application and browsing to /Store/Browse?genre=Jazz shows that our results are now being pulled from the database, displaying all albums in our selected Genre.</span></span>

![](mvc-music-store-part-4/_static/image2.jpg)

<span data-ttu-id="4c3ca-173">Nous allons rendre les mêmes remplacez notre /Store/détails / [id] URL, puis remplacer nos données factices par une requête de base de données qui charge un Album dont l’ID correspond à la valeur du paramètre.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-173">We'll make the same change to our /Store/Details/[id] URL, and replace our dummy data with a database query which loads an Album whose ID matches the parameter value.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

<span data-ttu-id="4c3ca-174">Notre application en cours d’exécution et en accédant à /Store/Details/1 montre que nos résultats sont maintenant chargés à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-174">Running our application and browsing to /Store/Details/1 shows that our results are now being pulled from the database.</span></span>

![](mvc-music-store-part-4/_static/image5.png)

<span data-ttu-id="4c3ca-175">Maintenant que notre page de détails de Store est configuré pour afficher un album par l’ID de l’Album, mettons à jour le **Parcourir** vue à lier à la vue de détails.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-175">Now that our Store Details page is set up to display an album by the Album ID, let's update the **Browse** view to link to the Details view.</span></span> <span data-ttu-id="4c3ca-176">Nous allons utiliser Html.ActionLink, exactement comme nous l’avons fait pour créer un lien à partir de l’Index de Store pour parcourir de Store à la fin de la section précédente.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-176">We will use Html.ActionLink, exactly as we did to link from Store Index to Store Browse at the end of the previous section.</span></span> <span data-ttu-id="4c3ca-177">La source complète pour le mode de navigation apparaît ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-177">The complete source for the Browse view appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

<span data-ttu-id="4c3ca-178">Nous sommes maintenant en mesure d’accéder à partir de notre page Store à une page de Genre, qui répertorie les albums disponibles, et en cliquant sur un album nous pouvons afficher les détails de cet album.</span><span class="sxs-lookup"><span data-stu-id="4c3ca-178">We're now able to browse from our Store page to a Genre page, which lists the available albums, and by clicking on an album we can view details for that album.</span></span>

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> <span data-ttu-id="4c3ca-179">[Précédent](mvc-music-store-part-3.md)
> [Suivant](mvc-music-store-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="4c3ca-179">[Previous](mvc-music-store-part-3.md)
[Next](mvc-music-store-part-5.md)</span></span>
