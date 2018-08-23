---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 'Partie 3 : Vues et ViewModels | Microsoft Docs'
author: jongalloway
description: Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application ASP.NET MVC Music Store. La partie 3 explique Views et ViewModels.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 828ff18abcc5932f82be71a45ebde589eeb051fa
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824896"
---
<a name="part-3-views-and-viewmodels"></a><span data-ttu-id="df07b-104">Partie 3 : Vues et ViewModels</span><span class="sxs-lookup"><span data-stu-id="df07b-104">Part 3: Views and ViewModels</span></span>
====================
<span data-ttu-id="df07b-105">par [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="df07b-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="df07b-106">Le Store de musique MVC est une application du didacticiel qui présente et explique étape par étape comment utiliser ASP.NET MVC et Visual Studio pour le développement web.</span><span class="sxs-lookup"><span data-stu-id="df07b-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="df07b-107">Le Store de musique MVC est une implémentation de magasin d’exemple léger qui vend des albums de musique en ligne et implémente l’administration de site de base, connexion de l’utilisateur et les fonctionnalités de panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="df07b-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="df07b-108">Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="df07b-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="df07b-109">La partie 3 explique Views et ViewModels.</span><span class="sxs-lookup"><span data-stu-id="df07b-109">Part 3 covers Views and ViewModels.</span></span>


<span data-ttu-id="df07b-110">Jusqu'à présent, nous avons simplement été retourner des chaînes à partir d’actions de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="df07b-110">So far we've just been returning strings from controller actions.</span></span> <span data-ttu-id="df07b-111">Qui constitue un excellent moyen pour avoir une idée du fonctionnement des contrôleurs, mais il n’est pas comment vous souhaitez créer une application web réelle.</span><span class="sxs-lookup"><span data-stu-id="df07b-111">That's a nice way to get an idea of how controllers work, but it's not how you'd want to build a real web application.</span></span> <span data-ttu-id="df07b-112">Nous allons un meilleur moyen de générer du code HTML à visiter notre site de navigateurs : un où nous pouvons utiliser des fichiers de modèle pour personnaliser plus facilement le contenu HTML renvoyer.</span><span class="sxs-lookup"><span data-stu-id="df07b-112">We are going to want a better way to generate HTML back to browsers visiting our site – one where we can use template files to more easily customize the HTML content send back.</span></span> <span data-ttu-id="df07b-113">C’est exactement ce que font les affichages.</span><span class="sxs-lookup"><span data-stu-id="df07b-113">That's exactly what Views do.</span></span>

## <a name="adding-a-view-template"></a><span data-ttu-id="df07b-114">Ajout d’un modèle de vue</span><span class="sxs-lookup"><span data-stu-id="df07b-114">Adding a View template</span></span>

<span data-ttu-id="df07b-115">Pour utiliser un modèle de vue, nous allons modifier la méthode Index du HomeController pour renvoyer un ActionResult et qu’il renvoie View(), comme ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="df07b-115">To use a view-template, we'll change the HomeController Index method to return an ActionResult, and have it return View(), like below:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

<span data-ttu-id="df07b-116">Les modifications ci-dessus indique au lieu de retourné une chaîne, nous souhaitons à la place une « vue » permet de générer un résultat de retour.</span><span class="sxs-lookup"><span data-stu-id="df07b-116">The above change indicates that instead of returned a string, we instead want to use a "View" to generate a result back.</span></span>

<span data-ttu-id="df07b-117">Nous allons maintenant ajouter un modèle de vue approprié à notre projet.</span><span class="sxs-lookup"><span data-stu-id="df07b-117">We'll now add an appropriate View template to our project.</span></span> <span data-ttu-id="df07b-118">Pour ce faire, nous allons positionner le curseur de texte au sein de la méthode d’action Index, puis avec le bouton droit et sélectionnez « Ajouter une vue ».</span><span class="sxs-lookup"><span data-stu-id="df07b-118">To do this we'll position the text cursor within the Index action method, then right-click and select "Add View".</span></span> <span data-ttu-id="df07b-119">Cela fera apparaître la boîte de dialogue Ajouter une vue :</span><span class="sxs-lookup"><span data-stu-id="df07b-119">This will bring up the Add View dialog:</span></span>

<span data-ttu-id="df07b-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="df07b-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span></span>

<span data-ttu-id="df07b-121">La boîte de dialogue « Ajouter une vue » permet de générer rapidement et facilement les fichiers de modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="df07b-121">The "Add View" dialog allows us to quickly and easily generate View template files.</span></span> <span data-ttu-id="df07b-122">Par défaut, la « vue d’ajouter » boîte de dialogue pré-remplit le nom du modèle de vue à créer afin qu’elle corresponde à la méthode d’action qui va l’utiliser.</span><span class="sxs-lookup"><span data-stu-id="df07b-122">By default the "Add View" dialog pre-populates the name of the View template to create so that it matches the action method that will use it.</span></span> <span data-ttu-id="df07b-123">Étant donné que nous avons utilisé le menu contextuel « Ajouter une vue » au sein de la méthode d’action Index() de notre HomeController, la boîte de dialogue « Ajouter une vue » ci-dessus a « Index » comme nom de la vue prérempli par défaut.</span><span class="sxs-lookup"><span data-stu-id="df07b-123">Because we used the "Add View" context menu within the Index() action method of our HomeController, the "Add View" dialog above has "Index" as the view name pre-populated by default.</span></span> <span data-ttu-id="df07b-124">Nous n’avez pas besoin de modifier les options sur cette boîte de dialogue, cliquez sur le bouton Ajouter.</span><span class="sxs-lookup"><span data-stu-id="df07b-124">We don't need to change any of the options on this dialog, so click the Add button.</span></span>

<span data-ttu-id="df07b-125">Lorsque nous cliquons sur le bouton Ajouter, Visual Web Developer crée une nouvelle Index.cshtml afficher le modèle pour nous dans le répertoire \Views\Home, si la création du dossier n’existe pas déjà.</span><span class="sxs-lookup"><span data-stu-id="df07b-125">When we click the Add button, Visual Web Developer will create a new Index.cshtml view template for us in the \Views\Home directory, creating the folder if doesn't already exist.</span></span>

![](mvc-music-store-part-3/_static/image2.png)

<span data-ttu-id="df07b-126">Le nom et dossier l’emplacement du fichier « Index.cshtml » est important et suit les conventions de nommage d’ASP.NET MVC par défaut.</span><span class="sxs-lookup"><span data-stu-id="df07b-126">The name and folder location of the "Index.cshtml" file is important, and follows the default ASP.NET MVC naming conventions.</span></span> <span data-ttu-id="df07b-127">Le nom de répertoire, \Views\Home, correspond au contrôleur - ce qui est nommé HomeController.</span><span class="sxs-lookup"><span data-stu-id="df07b-127">The directory name, \Views\Home, matches the controller - which is named HomeController.</span></span> <span data-ttu-id="df07b-128">Le nom du modèle de vue, Index, correspond à la méthode d’action de contrôleur qui affichent la vue.</span><span class="sxs-lookup"><span data-stu-id="df07b-128">The view template name, Index, matches the controller action method which will be displaying the view.</span></span>

<span data-ttu-id="df07b-129">ASP.NET MVC vous permet de nous éviter d’avoir à spécifier explicitement le nom ou l’emplacement d’un modèle de vue lorsque nous utilisons cette convention de nommage pour retourner une vue.</span><span class="sxs-lookup"><span data-stu-id="df07b-129">ASP.NET MVC allows us to avoid having to explicitly specify the name or location of a view template when we use this naming convention to return a view.</span></span> <span data-ttu-id="df07b-130">Il doit par défaut rendre le modèle de vue \Views\Home\Index.cshtml lorsque nous écrire le code ci-dessous au sein de notre HomeController :</span><span class="sxs-lookup"><span data-stu-id="df07b-130">It will by default render the \Views\Home\Index.cshtml view template when we write code like below within our HomeController:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

<span data-ttu-id="df07b-131">Visual Web Developer créé et ouvert le modèle de vue « Index.cshtml » une fois que nous avons cliqué sur le bouton « Ajouter » dans la boîte de dialogue « Ajouter une vue ».</span><span class="sxs-lookup"><span data-stu-id="df07b-131">Visual Web Developer created and opened the "Index.cshtml" view template after we clicked the "Add" button within the "Add View" dialog.</span></span> <span data-ttu-id="df07b-132">Le contenu de Index.cshtml est présenté ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="df07b-132">The contents of Index.cshtml are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

<span data-ttu-id="df07b-133">Cette vue est à l’aide de la syntaxe Razor, qui est plus concise que le moteur d’affichage Web Forms utilisé dans ASP.NET Web Forms et des versions antérieures d’ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="df07b-133">This view is using the Razor syntax, which is more concise than the Web Forms view engine used in ASP.NET Web Forms and previous versions of ASP.NET MVC.</span></span> <span data-ttu-id="df07b-134">Le moteur d’affichage Web Forms est toujours disponible dans ASP.NET MVC 3, mais de nombreux développeurs trouvent que le moteur d’affichage Razor tient vraiment bien de développement ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="df07b-134">The Web Forms view engine is still available in ASP.NET MVC 3, but many developers find that the Razor view engine fits ASP.NET MVC development really well.</span></span>

<span data-ttu-id="df07b-135">Les trois premières lignes définissent le titre de page à l’aide de ViewBag.Title.</span><span class="sxs-lookup"><span data-stu-id="df07b-135">The first three lines set the page title using ViewBag.Title.</span></span> <span data-ttu-id="df07b-136">Nous allons examiner comment cela fonctionne plus en détail plus rapidement, mais tout d’abord nous allons mettre à jour le texte d’en-tête de texte et afficher la page.</span><span class="sxs-lookup"><span data-stu-id="df07b-136">We'll look at how this works in more detail soon, but first let's update the text heading text and view the page.</span></span> <span data-ttu-id="df07b-137">Mise à jour le &lt;h2&gt; balise à dire « This est la Page d’accueil » comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="df07b-137">Update the &lt;h2&gt; tag to say "This is the Home Page" as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

<span data-ttu-id="df07b-138">L’application montre que notre nouveau texte est visible sur la page d’accueil en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="df07b-138">Running the application shows that our new text is visible on the home page.</span></span>

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a><span data-ttu-id="df07b-139">À l’aide d’une disposition pour les éléments communs de site</span><span class="sxs-lookup"><span data-stu-id="df07b-139">Using a Layout for common site elements</span></span>

<span data-ttu-id="df07b-140">La plupart des sites Web ont du contenu qui est partagé entre plusieurs pages : navigation, les pieds de page, les images de logo, les références de feuille de style, etc. Le moteur d’affichage Razor facilite la gestion à l’aide d’une page appelée \_Layout.cshtml qui a été créé automatiquement pour nous dans le dossier Shared/vues /.</span><span class="sxs-lookup"><span data-stu-id="df07b-140">Most websites have content which is shared between many pages: navigation, footers, logo images, stylesheet references, etc. The Razor view engine makes this easy to manage using a page called \_Layout.cshtml which has automatically been created for us inside the /Views/Shared folder.</span></span>

![](mvc-music-store-part-3/_static/image4.png)

<span data-ttu-id="df07b-141">Double-cliquez sur ce dossier pour afficher le contenu, qui sont présentées ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="df07b-141">Double-click on this folder to view the contents, which are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

<span data-ttu-id="df07b-142">Le contenu à partir de notre vues individuelles est affiché par le @RenderBody() commande et n’importe quel contenu commun que nous souhaitons figurer en dehors de qui peuvent être ajoutés à la \_Layout.cshtml balisage.</span><span class="sxs-lookup"><span data-stu-id="df07b-142">The content from our individual views will be displayed by the @RenderBody() command, and any common content that we want to appear outside of that can be added to the \_Layout.cshtml markup.</span></span> <span data-ttu-id="df07b-143">Nous devrons attraper notre Store de musique MVC pour avoir un en-tête commun avec des liens à notre zone de page d’accueil et Store sur toutes les pages du site, donc nous allons ajouter que pour le modèle directement au-dessus qui @RenderBody() instruction.</span><span class="sxs-lookup"><span data-stu-id="df07b-143">We'll want our MVC Music Store to have a common header with links to our Home page and Store area on all pages in the site, so we'll add that to the template directly above that @RenderBody() statement.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a><span data-ttu-id="df07b-144">La mise à jour de la feuille de style</span><span class="sxs-lookup"><span data-stu-id="df07b-144">Updating the StyleSheet</span></span>

<span data-ttu-id="df07b-145">Le modèle de projet vide inclut un fichier CSS très simplifié qui inclut uniquement les styles utilisés pour afficher les messages de validation.</span><span class="sxs-lookup"><span data-stu-id="df07b-145">The empty project template includes a very streamlined CSS file which just includes styles used to display validation messages.</span></span> <span data-ttu-id="df07b-146">Notre concepteur a fourni du CSS et des images supplémentaires pour définir l’apparence de notre site, donc nous allons ajouter ceux maintenant.</span><span class="sxs-lookup"><span data-stu-id="df07b-146">Our designer has provided some additional CSS and images to define the look and feel for our site, so we'll add those in now.</span></span>

<span data-ttu-id="df07b-147">Le fichier CSS et les Images mis à jour sont inclus dans le répertoire de contenu de Assets.zip MvcMusicStore qui est disponible à l’adresse [MVC-musique-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="df07b-147">The updated CSS file and Images are included in the Content directory of MvcMusicStore-Assets.zip which is available at [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span> <span data-ttu-id="df07b-148">Nous allons sélectionner les deux d'entre eux dans l’Explorateur Windows et les déposer dans le dossier de contenu de notre Solution dans Visual Web Developer, comme indiqué ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="df07b-148">We'll select both of them in Windows Explorer and drop them into our Solution's Content folder in Visual Web Developer, as shown below:</span></span>

![](mvc-music-store-part-3/_static/image5.png)

<span data-ttu-id="df07b-149">Vous allez être invité à confirmer si vous souhaitez remplacer le fichier Site.css existant.</span><span class="sxs-lookup"><span data-stu-id="df07b-149">You'll be asked to confirm if you want to overwrite the existing Site.css file.</span></span> <span data-ttu-id="df07b-150">Cliquez sur Oui.</span><span class="sxs-lookup"><span data-stu-id="df07b-150">Click Yes.</span></span>

![](mvc-music-store-part-3/_static/image6.png)

<span data-ttu-id="df07b-151">Le dossier de contenu de votre application s’affiche maintenant comme suit :</span><span class="sxs-lookup"><span data-stu-id="df07b-151">The Content folder of your application will now appear as follows:</span></span>

![](mvc-music-store-part-3/_static/image7.png)

<span data-ttu-id="df07b-152">Maintenant nous allons exécuter l’application et visualiser l’aspect de nos modifications sur la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="df07b-152">Now let's run the application and see how our changes look on the Home page.</span></span>

![](mvc-music-store-part-3/_static/image8.png)

- <span data-ttu-id="df07b-153">Passons en revue ce qui a changé : méthode d’action le HomeController Index trouvé et affiche le modèle \Views\Home\Index.cshtmlView, même si notre code appelé « retour View() », car la convention d’affectation de noms standard de suivi de notre modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="df07b-153">Let's review what's changed: The HomeController's Index action method found and displayed the \Views\Home\Index.cshtmlView template, even though our code called "return View()", because our View template followed the standard naming convention.</span></span>
- <span data-ttu-id="df07b-154">La Page d’accueil affiche un message d’accueil simple qui est défini dans le modèle de vue \Views\Home\Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="df07b-154">The Home Page is displaying a simple welcome message that is defined within the \Views\Home\Index.cshtml view template.</span></span>
- <span data-ttu-id="df07b-155">À l’aide de la Page d’accueil est notre \_Layout.cshtml modèle, et par conséquent, le message d’accueil est contenu dans la mise en page de site standard HTML.</span><span class="sxs-lookup"><span data-stu-id="df07b-155">The Home Page is using our \_Layout.cshtml template, and so the welcome message is contained within the standard site HTML layout.</span></span>

## <a name="using-a-model-to-pass-information-to-our-view"></a><span data-ttu-id="df07b-156">À l’aide d’un modèle pour transmettre des informations à notre vue</span><span class="sxs-lookup"><span data-stu-id="df07b-156">Using a Model to pass information to our View</span></span>

<span data-ttu-id="df07b-157">Un modèle de vue qui affiche simplement codé en dur HTML ne va pas faire un site web très intéressant.</span><span class="sxs-lookup"><span data-stu-id="df07b-157">A View template that just displays hardcoded HTML isn't going to make a very interesting web site.</span></span> <span data-ttu-id="df07b-158">Pour créer un site web dynamique, nous devrons au lieu de cela passer des informations à partir de nos actions de contrôleur à nos modèles de vue.</span><span class="sxs-lookup"><span data-stu-id="df07b-158">To create a dynamic web site, we'll instead want to pass information from our controller actions to our view templates.</span></span>

<span data-ttu-id="df07b-159">Dans le modèle Model-View-Controller, le terme À que modèle fait référence des objets qui représentent les données dans l’application.</span><span class="sxs-lookup"><span data-stu-id="df07b-159">In the Model-View-Controller pattern, the term Model refers to objects which represent the data in the application.</span></span> <span data-ttu-id="df07b-160">Souvent, les objets de modèle correspondent aux tables dans votre base de données, mais ils n’êtes pas obligé.</span><span class="sxs-lookup"><span data-stu-id="df07b-160">Often, model objects correspond to tables in your database, but they don't have to.</span></span>

<span data-ttu-id="df07b-161">Méthodes d’action de contrôleur qui retournent un ActionResult peuvent passer un objet de modèle à la vue.</span><span class="sxs-lookup"><span data-stu-id="df07b-161">Controller action methods which return an ActionResult can pass a model object to the view.</span></span> <span data-ttu-id="df07b-162">Cela permet à un contrôleur proprement regrouper toutes les informations nécessaires pour générer une réponse, puis à transmettre ces informations à un modèle de vue à utiliser pour générer la réponse HTML appropriée.</span><span class="sxs-lookup"><span data-stu-id="df07b-162">This allows a Controller to cleanly package up all the information needed to generate a response, and then pass this information off to a View template to use to generate the appropriate HTML response.</span></span> <span data-ttu-id="df07b-163">Il s’agit le plus simple à comprendre par voir en action, nous allons donc commencer.</span><span class="sxs-lookup"><span data-stu-id="df07b-163">This is easiest to understand by seeing it in action, so let's get started.</span></span>

<span data-ttu-id="df07b-164">Tout d’abord, nous allons créer des classes de modèle pour représenter les Genres et Albums dans notre magasin.</span><span class="sxs-lookup"><span data-stu-id="df07b-164">First we'll create some Model classes to represent Genres and Albums within our store.</span></span> <span data-ttu-id="df07b-165">Nous allons commencer en créant une classe de Genre.</span><span class="sxs-lookup"><span data-stu-id="df07b-165">Let's start by creating a Genre class.</span></span> <span data-ttu-id="df07b-166">Cliquez sur le dossier « Models » au sein de votre projet, choisissez l’option « Ajouter une classe », nommez le fichier « Genre.cs ».</span><span class="sxs-lookup"><span data-stu-id="df07b-166">Right-click the "Models" folder within your project, choose the "Add Class" option, and name the file "Genre.cs".</span></span>

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

<span data-ttu-id="df07b-167">Ensuite, ajoutez une propriété de nom de chaîne publique à la classe qui a été créée :</span><span class="sxs-lookup"><span data-stu-id="df07b-167">Then add a public string Name property to the class that was created:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

<span data-ttu-id="df07b-168">*Remarque : dans le cas, vous vous poseriez la question, le {get ; définir ;} effectue la notation utilisation de fonctionnalité de propriétés implémentées automatiquement de #. Cela nous donne les avantages d’une propriété sans nécessiter de déclarer un champ de stockage.*</span><span class="sxs-lookup"><span data-stu-id="df07b-168">*Note: In case you're wondering, the { get; set; } notation is making use of C#'s auto-implemented properties feature. This gives us the benefits of a property without requiring us to declare a backing field.*</span></span>

<span data-ttu-id="df07b-169">Ensuite, suivez les mêmes étapes pour créer une classe d’Album (nommée Album.cs) qui a un titre et une propriété du Genre :</span><span class="sxs-lookup"><span data-stu-id="df07b-169">Next, follow the same steps to create an Album class (named Album.cs) that has a Title and a Genre property:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

<span data-ttu-id="df07b-170">Nous pouvons maintenant modifier le StoreController pour utiliser des vues qui affichent des informations dynamiques à partir de notre modèle.</span><span class="sxs-lookup"><span data-stu-id="df07b-170">Now we can modify the StoreController to use Views which display dynamic information from our Model.</span></span> <span data-ttu-id="df07b-171">Si le paramètre - à des fins de démonstration maintenant - nous avons nommé notre Albums basés sur l’ID de demande, nous pouvons afficher ces informations comme dans la vue ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="df07b-171">If - for demonstration purposes right now - we named our Albums based on the request ID, we could display that information as in the view below.</span></span>

![](mvc-music-store-part-3/_static/image10.png)

<span data-ttu-id="df07b-172">Nous allons commencer en modifiant l’action des détails de Store pour qu’il affiche les informations pour un seul album.</span><span class="sxs-lookup"><span data-stu-id="df07b-172">We'll start by changing the Store Details action so it shows the information for a single album.</span></span> <span data-ttu-id="df07b-173">Ajoutez une instruction « using » en haut de la **StoreControllers** classe pour inclure l’espace de noms MvcMusicStore.Models, nous n’avez pas besoin de taper MvcMusicStore.Models.Album chaque fois que nous souhaitons utiliser la classe album.</span><span class="sxs-lookup"><span data-stu-id="df07b-173">Add a "using" statement to the top of the **StoreControllers** class to include the MvcMusicStore.Models namespace, so we don't need to type MvcMusicStore.Models.Album every time we want to use the album class.</span></span> <span data-ttu-id="df07b-174">La section instructions « using » de cette classe doit maintenant apparaître comme suit.</span><span class="sxs-lookup"><span data-stu-id="df07b-174">The "usings" section of that class should now appear as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

<span data-ttu-id="df07b-175">Ensuite, nous mettrons à jour l’action du contrôleur détails afin qu’elle retourne un ActionResult plutôt qu’une chaîne, comme nous l’avons fait avec la méthode d’Index du HomeController.</span><span class="sxs-lookup"><span data-stu-id="df07b-175">Next, we'll update the Details controller action so that it returns an ActionResult rather than a string, as we did with the HomeController's Index method.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

<span data-ttu-id="df07b-176">Nous pouvons maintenant modifier la logique pour retourner un objet d’Album à la vue.</span><span class="sxs-lookup"><span data-stu-id="df07b-176">Now we can modify the logic to return an Album object to the view.</span></span> <span data-ttu-id="df07b-177">Plus loin dans ce didacticiel nous récupérons les données à partir d’une base de données – mais pour l’instant, nous allons utiliser « factice de données » pour commencer.</span><span class="sxs-lookup"><span data-stu-id="df07b-177">Later in this tutorial we will be retrieving the data from a database – but for right now we will use "dummy data" to get started.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

<span data-ttu-id="df07b-178">*Remarque : Si vous n’êtes pas familiarisé avec c#, vous pouvez supposer que var reprenant que notre variable album est à liaison tardive. Qui n’est pas correct : le compilateur c# à l’aide de-l’inférence de type en fonction de ce que nous allons affecter à la variable pour déterminer que cet album est de type Album et compilation de la variable locale album comme type d’Album, donc nous obtenons la vérification au moment de la compilation et l’éditeur de code Visual Studio prise en charge.*</span><span class="sxs-lookup"><span data-stu-id="df07b-178">*Note: If you're unfamiliar with C#, you may assume that using var means that our album variable is late-bound. That's not correct – the C# compiler is using type-inference based on what we're assigning to the variable to determine that album is of type Album and compiling the local album variable as an Album type, so we get compile-time checking and Visual Studio code-editor support.*</span></span>

<span data-ttu-id="df07b-179">Nous allons maintenant créer un modèle de vue qui utilise notre Album pour générer une réponse HTML.</span><span class="sxs-lookup"><span data-stu-id="df07b-179">Let's now create a View template that uses our Album to generate an HTML response.</span></span> <span data-ttu-id="df07b-180">Avant cela, nous devons générer le projet afin que la boîte de dialogue Ajouter une vue connaît de notre classe Album nouvellement créé.</span><span class="sxs-lookup"><span data-stu-id="df07b-180">Before we do that we need to build the project so that the Add View dialog knows about our newly created Album class.</span></span> <span data-ttu-id="df07b-181">Vous pouvez générer le projet en sélectionnant le Debug⇨Build MvcMusicStore élément de menu (pour plus de crédibilité, vous pouvez utiliser le raccourci Ctrl-Maj-B de numéros pour générer le projet).</span><span class="sxs-lookup"><span data-stu-id="df07b-181">You can build the project by selecting the Debug⇨Build MvcMusicStore menu item (for extra credit, you can use the Ctrl-Shift-B shortcut to build the project).</span></span>

![](mvc-music-store-part-3/_static/image11.png)

<span data-ttu-id="df07b-182">Maintenant que nous avons créé nos classes de prise en charge, nous sommes prêts à créer notre modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="df07b-182">Now that we've set up our supporting classes, we're ready to build our View template.</span></span> <span data-ttu-id="df07b-183">Avec le bouton droit dans la méthode de détails et sélectionnez « Ajouter un affichage... » dans le menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="df07b-183">Right-click within the Details method and select "Add View…" from the context menu.</span></span>

![](mvc-music-store-part-3/_static/image12.png)

<span data-ttu-id="df07b-184">Nous allons créer un nouveau modèle de vue, comme nous l’avons fait avant avec le HomeController.</span><span class="sxs-lookup"><span data-stu-id="df07b-184">We are going to create a new View template like we did before with the HomeController.</span></span> <span data-ttu-id="df07b-185">Étant donné que nous allons la créer à partir de la StoreController générée ultérieurement utilisera par défaut dans un fichier \Views\Store\Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="df07b-185">Because we are creating it from the StoreController it will by default be generated in a \Views\Store\Index.cshtml file.</span></span>

<span data-ttu-id="df07b-186">Contrairement à avant, nous allons vérifier la case à cocher de la vue « Créer un fortement typée ».</span><span class="sxs-lookup"><span data-stu-id="df07b-186">Unlike before, we are going to check the "Create a strongly-typed" view checkbox.</span></span> <span data-ttu-id="df07b-187">Ensuite, nous allons sélectionner notre classe « Album » dans la liste « Vue données-class »-downlist.</span><span class="sxs-lookup"><span data-stu-id="df07b-187">We are then going to select our "Album" class within the "View data-class" drop-downlist.</span></span> <span data-ttu-id="df07b-188">Cela entraîne la boîte de dialogue « Ajouter une vue » créer un modèle de vue qui attend un Album objet sera passé à ce dernier à utiliser.</span><span class="sxs-lookup"><span data-stu-id="df07b-188">This will cause the "Add View" dialog to create a View template that expects that an Album object will be passed to it to use.</span></span>

![](mvc-music-store-part-3/_static/image13.png)

<span data-ttu-id="df07b-189">Lorsque nous cliquons sur le bouton « Ajouter » notre modèle de vue \Views\Store\Details.cshtml sera créé, qui contient le code suivant.</span><span class="sxs-lookup"><span data-stu-id="df07b-189">When we click the "Add" button our \Views\Store\Details.cshtml View template will be created, containing the following code.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

<span data-ttu-id="df07b-190">Notez que la première ligne, ce qui indique que cette vue est fortement typée à notre classe Album.</span><span class="sxs-lookup"><span data-stu-id="df07b-190">Notice the first line, which indicates that this view is strongly-typed to our Album class.</span></span> <span data-ttu-id="df07b-191">Le moteur d’affichage Razor comprend qu’a été passée un objet Album, afin que nous puissions accéder facilement aux propriétés de modèle et même ont l’avantage d’IntelliSense dans l’éditeur de Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="df07b-191">The Razor view engine understands that it has been passed an Album object, so we can easily access model properties and even have the benefit of IntelliSense in the Visual Web Developer editor.</span></span>

<span data-ttu-id="df07b-192">Mise à jour le &lt;h2&gt; balise afin qu’elle affiche la propriété de titre d’Album en modifiant cette ligne doit s’afficher comme suit.</span><span class="sxs-lookup"><span data-stu-id="df07b-192">Update the &lt;h2&gt; tag so it displays the Album's Title property by modifying that line to appear as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

<span data-ttu-id="df07b-193">Notez qu’IntelliSense est déclenchée lorsque vous entrez la période après la @Model mot clé, en affichant les propriétés et méthodes qui prend en charge de la classe Album.</span><span class="sxs-lookup"><span data-stu-id="df07b-193">Notice that IntelliSense is triggered when you enter the period after the @Model keyword, showing the properties and methods that the Album class supports.</span></span>

<span data-ttu-id="df07b-194">Nous allons maintenant exécuter à nouveau notre projet et accédez à l’URL de détails/Store/5.</span><span class="sxs-lookup"><span data-stu-id="df07b-194">Let's now re-run our project and visit the /Store/Details/5 URL.</span></span> <span data-ttu-id="df07b-195">Nous allons voir les détails d’un Album comme ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="df07b-195">We'll see details of an Album like below.</span></span>

![](mvc-music-store-part-3/_static/image14.png)

<span data-ttu-id="df07b-196">Nous allons maintenant rendre une mise à jour similaire pour la méthode d’action Parcourir Store.</span><span class="sxs-lookup"><span data-stu-id="df07b-196">Now we'll make a similar update for the Store Browse action method.</span></span> <span data-ttu-id="df07b-197">Mettre à jour de la méthode afin qu’elle retourne un ActionResult et modifiez la logique de la méthode afin qu’il crée un nouvel objet de Genre et renvoie à la vue.</span><span class="sxs-lookup"><span data-stu-id="df07b-197">Update the method so it returns an ActionResult, and modify the method logic so it creates a new Genre object and returns it to the View.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

<span data-ttu-id="df07b-198">Avec le bouton droit dans la méthode de parcourir et sélectionnez « Ajouter un affichage... » dans le menu contextuel, puis ajouter une vue qui est fortement typée ajoutez fortement typé à la classe de Genre.</span><span class="sxs-lookup"><span data-stu-id="df07b-198">Right-click in the Browse method and select "Add View…" from the context menu, then add a View that is strongly-typed add a strongly typed to the Genre class.</span></span>

![](mvc-music-store-part-3/_static/image15.png)

<span data-ttu-id="df07b-199">Mise à jour le &lt;h2&gt; élément dans la vue de code (dans /Views/Store/Browse.cshtml) pour afficher les informations de Genre.</span><span class="sxs-lookup"><span data-stu-id="df07b-199">Update the &lt;h2&gt; element in the view code (in /Views/Store/Browse.cshtml) to display the Genre information.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

<span data-ttu-id="df07b-200">Maintenant nous allons exécuter à nouveau notre projet et accédez à/Store/Parcourir ? Genre = URL Disco.</span><span class="sxs-lookup"><span data-stu-id="df07b-200">Now let's re-run our project and browse to the /Store/Browse?Genre=Disco URL.</span></span> <span data-ttu-id="df07b-201">Nous allons voir la page de navigation affichée comme ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="df07b-201">We'll see the Browse page displayed like below.</span></span>

![](mvc-music-store-part-3/_static/image16.png)

<span data-ttu-id="df07b-202">Enfin, nous allons effectuer une mise à jour légèrement plus complexe le **Index Store** méthode d’action et pour afficher une liste de tous les Genres dans notre magasin.</span><span class="sxs-lookup"><span data-stu-id="df07b-202">Finally, let's make a slightly more complex update to the **Store Index** action method and view to display a list of all the Genres in our store.</span></span> <span data-ttu-id="df07b-203">Nous allons le faire à l’aide d’une liste de Genres comme notre objet de modèle, plutôt que simplement un Genre unique.</span><span class="sxs-lookup"><span data-stu-id="df07b-203">We'll do that by using a List of Genres as our model object, rather than just a single Genre.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

<span data-ttu-id="df07b-204">Avec le bouton droit dans la méthode d’action Store Index et sélectionnez Ajouter une vue comme avant, sélectionnez Genre comme la classe de modèle, puis appuyez sur le bouton Ajouter.</span><span class="sxs-lookup"><span data-stu-id="df07b-204">Right-click in the Store Index action method and select Add View as before, select Genre as the Model class, and press the Add button.</span></span>

![](mvc-music-store-part-3/_static/image17.png)

<span data-ttu-id="df07b-205">Tout d’abord, nous allons modifier le @model déclaration pour indiquer que la vue attend Genre plusieurs objets plutôt que simplement un.</span><span class="sxs-lookup"><span data-stu-id="df07b-205">First we'll change the @model declaration to indicate that the view will be expecting several Genre objects rather than just one.</span></span> <span data-ttu-id="df07b-206">Modifiez la première ligne de /Store/Index.cshtml comme suit :</span><span class="sxs-lookup"><span data-stu-id="df07b-206">Change the first line of /Store/Index.cshtml to read as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

<span data-ttu-id="df07b-207">Cela indique le moteur d’affichage Razor vous travaillerez avec un objet de modèle qui peut contenir plusieurs objets de Genre.</span><span class="sxs-lookup"><span data-stu-id="df07b-207">This tells the Razor view engine that it will be working with a model object that can hold several Genre objects.</span></span> <span data-ttu-id="df07b-208">Nous utilisons un IEnumerable&lt;Genre&gt; au lieu d’une liste&lt;Genre&gt; dans la mesure où il est plus générique, ce qui nous permet de modifier ultérieurement le type de notre modèle pour tout type d’objet qui prend en charge l’interface IEnumerable.</span><span class="sxs-lookup"><span data-stu-id="df07b-208">We're using an IEnumerable&lt;Genre&gt; rather than a List&lt;Genre&gt; since it's more generic, allowing us to change our model type later to any object type that supports the IEnumerable interface.</span></span>

<span data-ttu-id="df07b-209">Ensuite, nous allons parcourez en boucle les objets de Genre dans le modèle comme indiqué dans le code de vue terminée ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="df07b-209">Next, we'll loop through the Genre objects in the model as shown in the completed view code below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

<span data-ttu-id="df07b-210">Notez que nous avons une prise en charge IntelliSense complète comme nous Entrez ce code, afin que lorsque vous tapez «@Model. »</span><span class="sxs-lookup"><span data-stu-id="df07b-210">Notice that we have full IntelliSense support as we enter this code, so that when we type "@Model."</span></span> <span data-ttu-id="df07b-211">Nous voyons toutes les méthodes et propriétés prises en charge par un IEnumerable de type Genre.</span><span class="sxs-lookup"><span data-stu-id="df07b-211">we see all methods and properties supported by an IEnumerable of type Genre.</span></span>

![](mvc-music-store-part-3/_static/image18.png)

<span data-ttu-id="df07b-212">Dans notre boucle « foreach », Visual Web Developer sait que chaque élément est de type Genre, nous voir IntelliSense pour chaque type de Genre.</span><span class="sxs-lookup"><span data-stu-id="df07b-212">Within our "foreach" loop, Visual Web Developer knows that each item is of type Genre, so we see IntelliSense for each the Genre type.</span></span>

![](mvc-music-store-part-3/_static/image19.png)

<span data-ttu-id="df07b-213">Ensuite, la fonctionnalité de génération de modèles automatique examiné l’objet de Genre et déterminé que chacune aura une propriété Name, afin qu’il parcourt en boucle et les écrit. Il génère également des liens à la modifier, les détails et les supprimer pour chaque élément individuel.</span><span class="sxs-lookup"><span data-stu-id="df07b-213">Next, the scaffolding feature examined the Genre object and determined that each will have a Name property, so it loops through and writes them out. It also generates Edit, Details, and Delete links to each individual item.</span></span> <span data-ttu-id="df07b-214">Nous allons en tirer parti plus loin dans notre responsable de magasin, mais pour l’instant, nous souhaitons avoir une liste simple à la place.</span><span class="sxs-lookup"><span data-stu-id="df07b-214">We'll take advantage of that later in our store manager, but for now we'd like to have a simple list instead.</span></span>

<span data-ttu-id="df07b-215">Lorsque nous pouvons exécuter l’application et que vous accédez à /Store, nous voyons que le nombre et la liste de Genres est affiché.</span><span class="sxs-lookup"><span data-stu-id="df07b-215">When we run the application and browse to /Store, we see that both the count and list of Genres is displayed.</span></span>

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a><span data-ttu-id="df07b-216">Ajout de liens entre les pages</span><span class="sxs-lookup"><span data-stu-id="df07b-216">Adding Links between pages</span></span>

<span data-ttu-id="df07b-217">Notre URL /Store qui répertorie les Genres actuellement répertorie les noms de Genre simplement en tant que texte brut.</span><span class="sxs-lookup"><span data-stu-id="df07b-217">Our /Store URL that lists Genres currently lists the Genre names simply as plain text.</span></span> <span data-ttu-id="df07b-218">Nous allons modifier cela afin qu’au lieu de texte brut à la place le lien de noms de Genre à l’URL du Store et parcourir appropriés, afin que le clic sur un genre de musique, comme « Disco » permet d’accéder à/Store/Parcourir ? genre = Disco URL.</span><span class="sxs-lookup"><span data-stu-id="df07b-218">Let's change this so that instead of plain text we instead have the Genre names link to the appropriate /Store/Browse URL, so that clicking on a music genre like "Disco" will navigate to the /Store/Browse?genre=Disco URL.</span></span> <span data-ttu-id="df07b-219">Nous pouvons mettre à jour notre modèle de vue \Views\Store\Index.cshtml à la sortie de ces liens à l’aide de code semblable au suivant **(ne tapez pas cela dans, nous allons améliorer dessus)**:</span><span class="sxs-lookup"><span data-stu-id="df07b-219">We could update our \Views\Store\Index.cshtml View template to output these links using code like below **(don't type this in - we're going to improve on it)**:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

<span data-ttu-id="df07b-220">Cela fonctionne, mais elle risque de problèmes plus tard dans la mesure où elle s’appuie sur une chaîne codée en dur.</span><span class="sxs-lookup"><span data-stu-id="df07b-220">That works, but it could lead to trouble later since it relies on a hardcoded string.</span></span> <span data-ttu-id="df07b-221">Par exemple, si nous voulions renommer le contrôleur, nous devons effectuer des recherches dans notre code à la recherche pour les liens qui doivent être mis à jour.</span><span class="sxs-lookup"><span data-stu-id="df07b-221">For instance, if we wanted to rename the Controller, we'd need to search through our code looking for links that need to be updated.</span></span>

<span data-ttu-id="df07b-222">Une autre approche que nous pouvons utiliser consiste à tirer parti d’une méthode d’assistance HTML.</span><span class="sxs-lookup"><span data-stu-id="df07b-222">An alternative approach we can use is to take advantage of an HTML Helper method.</span></span> <span data-ttu-id="df07b-223">ASP.NET MVC inclut des méthodes d’assistance HTML qui sont disponibles à partir de notre code de modèle de vue pour effectuer diverses tâches courantes, comme cela.</span><span class="sxs-lookup"><span data-stu-id="df07b-223">ASP.NET MVC includes HTML Helper methods which are available from our View template code to perform a variety of common tasks just like this.</span></span> <span data-ttu-id="df07b-224">La méthode d’assistance Html.ActionLink() est particulièrement utile et permet de facilement générer HTML &lt;un&gt; établit un lien et s’occupe de détails ennuyeux tels que de s’assurer que les chemins d’URL sont correctement codées URL.</span><span class="sxs-lookup"><span data-stu-id="df07b-224">The Html.ActionLink() helper method is a particularly useful one, and makes it easy to build HTML &lt;a&gt; links and takes care of annoying details like making sure URL paths are properly URL encoded.</span></span>

<span data-ttu-id="df07b-225">Html.ActionLink() a plusieurs surcharges différentes pour autoriser la spécification de toutes les informations que vous avez besoin pour vos liens.</span><span class="sxs-lookup"><span data-stu-id="df07b-225">Html.ActionLink() has several different overloads to allow specifying as much information as you need for your links.</span></span> <span data-ttu-id="df07b-226">Dans le cas le plus simple, vous allez fournir le texte du lien et la méthode d’Action pour accéder à un clic sur le lien hypertexte sur le client.</span><span class="sxs-lookup"><span data-stu-id="df07b-226">In the simplest case, you'll supply just the link text and the Action method to go to when the hyperlink is clicked on the client.</span></span> <span data-ttu-id="df07b-227">Par exemple, nous pouvons lier à « / Store / « méthode Index() sur la page de détails de Store avec le texte du lien « Aller vers le Store Index » à l’aide de l’appel suivant :</span><span class="sxs-lookup"><span data-stu-id="df07b-227">For example, we can link to "/Store/" Index() method on the Store Details page with the link text "Go to the Store Index" using the following call:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

<span data-ttu-id="df07b-228">*Remarque : dans ce cas, nous ne devons spécifier le nom du contrôleur, car nous allons simplement lier à une autre action au sein du même contrôleur qui est rendu l’affichage actuel.*</span><span class="sxs-lookup"><span data-stu-id="df07b-228">*Note: In this case, we didn't need to specify the controller name because we're just linking to another action within the same controller that's rendering the current view.*</span></span>

<span data-ttu-id="df07b-229">Notre liens vers la page Parcourir devez passer un paramètre, cependant, nous allons utiliser une autre surcharge de la méthode Html.ActionLink qui accepte trois paramètres :</span><span class="sxs-lookup"><span data-stu-id="df07b-229">Our links to the Browse page will need to pass a parameter, though, so we'll use another overload of the Html.ActionLink method that takes three parameters:</span></span>

- 1. <span data-ttu-id="df07b-230">Texte du lien, qui affiche le nom du Genre</span><span class="sxs-lookup"><span data-stu-id="df07b-230">Link text, which will display the Genre name</span></span>
- 2. <span data-ttu-id="df07b-231">Nom d’action de contrôleur (Parcourir)</span><span class="sxs-lookup"><span data-stu-id="df07b-231">Controller action name (Browse)</span></span>
- 3. <span data-ttu-id="df07b-232">Valeurs de paramètre d’itinéraire, en spécifiant le nom (Genre) et la valeur (nom du Genre)</span><span class="sxs-lookup"><span data-stu-id="df07b-232">Route parameter values, specifying both the name (Genre) and the value (Genre name)</span></span>

<span data-ttu-id="df07b-233">Placement que tous ensemble, voici comment nous allons écrire ces liens à la vue Index de Store :</span><span class="sxs-lookup"><span data-stu-id="df07b-233">Putting that all together, here's how we'll write those links to the Store Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

<span data-ttu-id="df07b-234">Lorsque nous pouvons exécuter notre projet à nouveau et que vous accédez à l’URL /Store/ nous voyez à présent une liste de genres.</span><span class="sxs-lookup"><span data-stu-id="df07b-234">Now when we run our project again and access the /Store/ URL we will see a list of genres.</span></span> <span data-ttu-id="df07b-235">Chaque genre est un lien hypertexte : lorsque vous cliquez sur prendra nous notre/Store/Parcourir ? genre =*[genre]* URL.</span><span class="sxs-lookup"><span data-stu-id="df07b-235">Each genre is a hyperlink – when clicked it will take us to our /Store/Browse?genre=*[genre]* URL.</span></span>

![](mvc-music-store-part-3/_static/image3.jpg)

<span data-ttu-id="df07b-236">Le code HTML de la liste de genre ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="df07b-236">The HTML for the genre list looks like this:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


> [!div class="step-by-step"]
> <span data-ttu-id="df07b-237">[Précédent](mvc-music-store-part-2.md)
> [Suivant](mvc-music-store-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="df07b-237">[Previous](mvc-music-store-part-2.md)
[Next](mvc-music-store-part-4.md)</span></span>
