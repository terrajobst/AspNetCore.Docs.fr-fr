---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Ajout d’une colonne au modèle | Documents Microsoft
author: shanselman
description: Il s’agit d’un didacticiel débutant qui présente les notions de base d’ASP.NET MVC. Créez une application web simple qui lit et écrit à partir d’une base de données.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 7a0b64ce00fc5ee6d49990f1d4d93a154c467bf5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/10/2018
---
<a name="adding-a-column-to-the-model"></a><span data-ttu-id="bbd8a-104">Ajout d’une colonne au modèle</span><span class="sxs-lookup"><span data-stu-id="bbd8a-104">Adding a Column to the Model</span></span>
====================
<span data-ttu-id="bbd8a-105">par [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="bbd8a-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="bbd8a-106">Il s’agit d’un didacticiel débutant qui présente les notions de base d’ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="bbd8a-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="bbd8a-107">Vous allez créer une application web simple qui lit et écrit à partir d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="bbd8a-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="bbd8a-108">Visitez le [centre d’apprentissage ASP.NET MVC](../../../index.md) pour rechercher d’autres ASP.NET MVC didacticiels et exemples.</span><span class="sxs-lookup"><span data-stu-id="bbd8a-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="bbd8a-109">Dans cette section, nous allons guident comment nous pouvons apporter des modifications au schéma de notre base de données et gérer les modifications de l’application.</span><span class="sxs-lookup"><span data-stu-id="bbd8a-109">In this section we are going to walk through how we can make changes to the schema of our database, and handle the changes within our application.</span></span>

<span data-ttu-id="bbd8a-110">Vous allez ajouter une colonne « Évaluation » à la table de film.</span><span class="sxs-lookup"><span data-stu-id="bbd8a-110">Let's add a "Rating" Colum to the Movie table.</span></span> <span data-ttu-id="bbd8a-111">Revenez à l’IDE, puis cliquez sur l’Explorateur de base de données.</span><span class="sxs-lookup"><span data-stu-id="bbd8a-111">Go back to the IDE and click the Database Explorer.</span></span> <span data-ttu-id="bbd8a-112">Cliquez avec le bouton droit sur la table de film et sélectionnez Ouvrir la définition de Table.</span><span class="sxs-lookup"><span data-stu-id="bbd8a-112">Right click the Movie table and select Open Table Definition.</span></span>

<span data-ttu-id="bbd8a-113">Ajouter une colonne « Évaluation » comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="bbd8a-113">Add a "Rating" column as seen below.</span></span> <span data-ttu-id="bbd8a-114">Étant donné que nous n’avons pas toutes les évaluations maintenant, la colonne peut autoriser les valeurs NULL.</span><span class="sxs-lookup"><span data-stu-id="bbd8a-114">Since we don't have any Ratings now, the column can allow nulls.</span></span> <span data-ttu-id="bbd8a-115">Cliquez sur Enregistrer.</span><span class="sxs-lookup"><span data-stu-id="bbd8a-115">Click Save.</span></span>

<span data-ttu-id="bbd8a-116">[![Modification de Table de films](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="bbd8a-116">[![Editing Movies Table](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span></span>

<span data-ttu-id="bbd8a-117">Ensuite, revenez à l’Explorateur de solutions et ouvrez le fichier Movies.edmx (qui se trouve dans le dossier \Models).</span><span class="sxs-lookup"><span data-stu-id="bbd8a-117">Next, return to the Solution Explorer and open up the Movies.edmx file (which is in the \Models folder).</span></span> <span data-ttu-id="bbd8a-118">Cliquez avec le bouton droit sur l’aire de conception (la zone blanche) et sélectionnez le modèle de mise à jour à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="bbd8a-118">Right click on the design surface (the white area) and select Update Model from Database.</span></span>

<span data-ttu-id="bbd8a-119">[![Films - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="bbd8a-119">[![Movies - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span></span>

<span data-ttu-id="bbd8a-120">Cette action lance l’Assistant de mise à jour « ».</span><span class="sxs-lookup"><span data-stu-id="bbd8a-120">This will launch the "Update Wizard".</span></span> <span data-ttu-id="bbd8a-121">Cliquez sur l’onglet de l’actualisation dans celui-ci, puis cliquez sur Terminer.</span><span class="sxs-lookup"><span data-stu-id="bbd8a-121">Click the Refresh tab within it and click Finish.</span></span> <span data-ttu-id="bbd8a-122">Notre classe de modèle de film sera ensuite être mis à jour avec la nouvelle colonne.</span><span class="sxs-lookup"><span data-stu-id="bbd8a-122">Our Movie model class will then be updated with the new column.</span></span>

![Assistant Mise à jour (2)](getting-started-with-mvc-part8/_static/image5.png)

<span data-ttu-id="bbd8a-124">Après avoir cliqué sur Terminer, vous pouvez voir que la nouvelle colonne de classement a été ajoutée à l’entité de film dans notre modèle.</span><span class="sxs-lookup"><span data-stu-id="bbd8a-124">After clicking Finish, you can see the new Rating Column has been added to the Movie Entity in our model.</span></span>

<span data-ttu-id="bbd8a-125">[![Entité de film](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="bbd8a-125">[![Movie Entity](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span></span>

<span data-ttu-id="bbd8a-126">Nous avons ajouté une colonne dans le modèle de base de données, mais ne conscient pas les vues à son sujet.</span><span class="sxs-lookup"><span data-stu-id="bbd8a-126">We've added a column in the database model, but the Views don't know about it.</span></span>

## <a name="update-views-with-model-changes"></a><span data-ttu-id="bbd8a-127">Vues de la mise à jour avec les modifications apportées au modèle</span><span class="sxs-lookup"><span data-stu-id="bbd8a-127">Update Views with Model Changes</span></span>

<span data-ttu-id="bbd8a-128">Il existe plusieurs façons, nous pouvons mettre à jour nos modèles de vue afin de refléter la nouvelle colonne de classement.</span><span class="sxs-lookup"><span data-stu-id="bbd8a-128">There are a few ways we could update our view templates to reflect the new Rating column.</span></span> <span data-ttu-id="bbd8a-129">Étant donné que nous avons créé ces vues en les générant via la boîte de dialogue Ajouter une vue, nous avons supprimez-les et recréez-les à nouveau.</span><span class="sxs-lookup"><span data-stu-id="bbd8a-129">Since we created these Views by generating them via the Add View dialog, we could delete them and recreate them again.</span></span> <span data-ttu-id="bbd8a-130">Toutefois, en général personnes seront déjà modifications apportées à leurs modèles d’affichage à partir de la génération de modèle généré automatiquement initiale et souhaiteront ajouter ou supprimer des champs manuellement, comme nous l’avons fait pour créer le champ ID.</span><span class="sxs-lookup"><span data-stu-id="bbd8a-130">However, typically people will have already made modifications to their View templates from the initial scaffolded generation and will want to add or delete fields manually, just as we did with the ID field for Create.</span></span>

<span data-ttu-id="bbd8a-131">Ouvrez le modèle \Views\Movies\Index.aspx et ajoutez un &lt;th&gt;évaluation&lt;/th&gt; au début de la table de film.</span><span class="sxs-lookup"><span data-stu-id="bbd8a-131">Open up the \Views\Movies\Index.aspx template and add a &lt;th&gt;Rating&lt;/th&gt; to the head of the Movie table.</span></span> <span data-ttu-id="bbd8a-132">J’ai ajouté mine après le Genre.</span><span class="sxs-lookup"><span data-stu-id="bbd8a-132">I added mine after Genre.</span></span> <span data-ttu-id="bbd8a-133">Puis, dans la même position de colonne, mais plus bas, ajouter une ligne pour notre nouvelle évaluation de la sortie.</span><span class="sxs-lookup"><span data-stu-id="bbd8a-133">Then, in the same column position but lower down, add a line to output our new Rating.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

<span data-ttu-id="bbd8a-134">Notre modèle Index.aspx final doit ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="bbd8a-134">Our final Index.aspx template will look like this:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

<span data-ttu-id="bbd8a-135">Nous allons ensuite ouvrir le modèle \Views\Movies\Create.aspx et ajouter une étiquette et une zone de texte pour notre nouvelle propriété de contrôle d’accès :</span><span class="sxs-lookup"><span data-stu-id="bbd8a-135">Let's then open up the \Views\Movies\Create.aspx template and add a Label and Textbox for our new Rating property:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

<span data-ttu-id="bbd8a-136">Notre modèle Create.aspx final ressembler à ceci et nous allons modifier le titre et la base de données secondaire de notre navigateur &lt;h2&gt; titre à quelque chose comme « Créer un film » pendant que nous sommes ici !</span><span class="sxs-lookup"><span data-stu-id="bbd8a-136">Our final Create.aspx template will look like this, and let's change our browser's title and secondary &lt;h2&gt; title to something like "Create a Movie" while we're in here!</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

<span data-ttu-id="bbd8a-137">Exécuter votre application et que vous disposez désormais un nouveau champ dans la base de données qui a été ajouté à la page de création.</span><span class="sxs-lookup"><span data-stu-id="bbd8a-137">Run your app and now you've got a new field in the database that's been added to the Create page.</span></span> <span data-ttu-id="bbd8a-138">Ajouter un nouveau film - cette fois avec une évaluation égale à - et cliquez sur Créer.</span><span class="sxs-lookup"><span data-stu-id="bbd8a-138">Add a new Movie - this time with a Rating - and click Create.</span></span>

<span data-ttu-id="bbd8a-139">[![Créer un film - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="bbd8a-139">[![Create a Movie - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span></span>

<span data-ttu-id="bbd8a-140">Une fois que vous cliquez sur Créer, vous êtes envoyé à la page d’Index où vous nouveau film est répertorié avec la nouvelle colonne de classement dans la base de données</span><span class="sxs-lookup"><span data-stu-id="bbd8a-140">After you click Create, you're sent to the Index page where you new Movie is listed with the new Rating Column in the database</span></span>

<span data-ttu-id="bbd8a-141">[![Liste de films - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="bbd8a-141">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span></span>

<span data-ttu-id="bbd8a-142">Ce didacticiel de base obtenu vous lancer dans la création de contrôleurs, leur association avec des vues et en passant autour des données codées en dur.</span><span class="sxs-lookup"><span data-stu-id="bbd8a-142">This basic tutorial got you started making Controllers, associating them with Views and passing around hard-coded data.</span></span> <span data-ttu-id="bbd8a-143">Ensuite, nous avons créé et conçu une base de données et placer des données dans.</span><span class="sxs-lookup"><span data-stu-id="bbd8a-143">Then we created and designed a Database and put some data it in.</span></span> <span data-ttu-id="bbd8a-144">Nous les données récupérées à partir de la base de données et elle affiche dans une table HTML.</span><span class="sxs-lookup"><span data-stu-id="bbd8a-144">We retrieved the data from the database and displayed it in an HTML table.</span></span> <span data-ttu-id="bbd8a-145">Ensuite, nous avons ajouté un formulaire de création qui permettent à l’utilisateur d’ajouter des données à la base de données eux-mêmes à partir de l’Application Web.</span><span class="sxs-lookup"><span data-stu-id="bbd8a-145">Then we added a Create form that let the user add data to the database themselves from within the Web Application.</span></span> <span data-ttu-id="bbd8a-146">Nous avons ajouté la validation, puis apportées à la validation d’utiliser JavaScript côté client.</span><span class="sxs-lookup"><span data-stu-id="bbd8a-146">We added validation, then made the validation use JavaScript on the client-side.</span></span> <span data-ttu-id="bbd8a-147">Enfin, nous avons modifié la base de données pour inclure une colonne de données, puis nos deux pages pour créer et afficher ces nouvelles données mises à jour.</span><span class="sxs-lookup"><span data-stu-id="bbd8a-147">Finally, we changed the database to include a new column of data, then updated our two pages to create and display this new data.</span></span>

<span data-ttu-id="bbd8a-148">Je maintenant vous encourage à passer à notre didacticiel de niveau intermédiaire «[magasin de musique MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)», ainsi que des vidéos et des ressources au nombre [ https://asp.net/mvc ](https://asp.net/mvc) pour en savoir plus sur ASP.NET MVC !</span><span class="sxs-lookup"><span data-stu-id="bbd8a-148">I now encourage you to move on to our intermediate-level tutorial "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" as well as the many videos and resources at [https://asp.net/mvc](https://asp.net/mvc) to learn even more about ASP.NET MVC!</span></span>

<span data-ttu-id="bbd8a-149">Bonne lecture !</span><span class="sxs-lookup"><span data-stu-id="bbd8a-149">Enjoy!</span></span>

- <span data-ttu-id="bbd8a-150">Scott Hanselman - [ http://hanselman.com ](http://hanselman.com) et [ @shanselman ](http://twitter.com/shanselman) sur Twitter.</span><span class="sxs-lookup"><span data-stu-id="bbd8a-150">Scott Hanselman - [http://hanselman.com](http://hanselman.com) and [@shanselman](http://twitter.com/shanselman) on Twitter.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="bbd8a-151">Précédent</span><span class="sxs-lookup"><span data-stu-id="bbd8a-151">Previous</span></span>](getting-started-with-mvc-part7.md)
