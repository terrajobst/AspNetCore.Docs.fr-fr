---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Ajout d’une colonne au modèle | Microsoft Docs
author: shanselman
description: Il s’agit d’un didacticiel de débutant qui présente les principes de base d’ASP.NET MVC. Créer une application web simple qui lit et écrit à partir d’une base de données.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 22a6c4e5a07e81d5876cc442e68926094e3a243d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824285"
---
<a name="adding-a-column-to-the-model"></a><span data-ttu-id="01596-104">Ajout d’une colonne au modèle</span><span class="sxs-lookup"><span data-stu-id="01596-104">Adding a Column to the Model</span></span>
====================
<span data-ttu-id="01596-105">par [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="01596-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="01596-106">Il s’agit d’un didacticiel de débutant qui présente les principes de base d’ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="01596-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="01596-107">Vous allez créer une application web simple qui lit et écrit à partir d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="01596-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="01596-108">Visitez le [centre d’apprentissage ASP.NET MVC](../../../index.md) pour rechercher d’autres ASP.NET MVC didacticiels et exemples.</span><span class="sxs-lookup"><span data-stu-id="01596-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="01596-109">Dans cette section, nous allons présenter comment nous pouvons apporter des modifications au schéma de notre base de données et gérer les modifications au sein de notre application.</span><span class="sxs-lookup"><span data-stu-id="01596-109">In this section we are going to walk through how we can make changes to the schema of our database, and handle the changes within our application.</span></span>

<span data-ttu-id="01596-110">Nous allons ajouter une colonne « Évaluation » à la table Movie.</span><span class="sxs-lookup"><span data-stu-id="01596-110">Let's add a "Rating" Colum to the Movie table.</span></span> <span data-ttu-id="01596-111">Revenez à l’IDE, puis cliquez sur l’Explorateur de base de données.</span><span class="sxs-lookup"><span data-stu-id="01596-111">Go back to the IDE and click the Database Explorer.</span></span> <span data-ttu-id="01596-112">Cliquez avec le bouton droit sur la table Movie et sélectionnez Ouvrir la définition de Table.</span><span class="sxs-lookup"><span data-stu-id="01596-112">Right click the Movie table and select Open Table Definition.</span></span>

<span data-ttu-id="01596-113">Ajouter une colonne « Rating » comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="01596-113">Add a "Rating" column as seen below.</span></span> <span data-ttu-id="01596-114">Étant donné que nous n’avons pas n’importe quel contrôle d’accès maintenant, la colonne peut accepter les valeurs NULL.</span><span class="sxs-lookup"><span data-stu-id="01596-114">Since we don't have any Ratings now, the column can allow nulls.</span></span> <span data-ttu-id="01596-115">Cliquez sur Enregistrer.</span><span class="sxs-lookup"><span data-stu-id="01596-115">Click Save.</span></span>

<span data-ttu-id="01596-116">[![Modification de Table de films](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="01596-116">[![Editing Movies Table](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span></span>

<span data-ttu-id="01596-117">Ensuite, revenez à l’Explorateur de solutions et ouvrez le fichier Movies.edmx (qui se trouve dans le dossier \Models).</span><span class="sxs-lookup"><span data-stu-id="01596-117">Next, return to the Solution Explorer and open up the Movies.edmx file (which is in the \Models folder).</span></span> <span data-ttu-id="01596-118">Cliquez avec le bouton droit sur l’aire de conception (la zone blanche) et sélectionnez le modèle de mise à jour à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="01596-118">Right click on the design surface (the white area) and select Update Model from Database.</span></span>

<span data-ttu-id="01596-119">[![Films - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="01596-119">[![Movies - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span></span>

<span data-ttu-id="01596-120">Cette action lance l’Assistant de mise à jour « ».</span><span class="sxs-lookup"><span data-stu-id="01596-120">This will launch the "Update Wizard".</span></span> <span data-ttu-id="01596-121">Cliquez sur l’onglet de l’actualisation dans celui-ci et cliquez sur Terminer.</span><span class="sxs-lookup"><span data-stu-id="01596-121">Click the Refresh tab within it and click Finish.</span></span> <span data-ttu-id="01596-122">Notre classe de modèle de film sera ensuite être mis à jour avec la nouvelle colonne.</span><span class="sxs-lookup"><span data-stu-id="01596-122">Our Movie model class will then be updated with the new column.</span></span>

![Assistant de mise à jour (2)](getting-started-with-mvc-part8/_static/image5.png)

<span data-ttu-id="01596-124">Après avoir cliqué sur Terminer, vous pouvez voir que la nouvelle colonne de classement a été ajoutée à l’entité de film dans notre modèle.</span><span class="sxs-lookup"><span data-stu-id="01596-124">After clicking Finish, you can see the new Rating Column has been added to the Movie Entity in our model.</span></span>

<span data-ttu-id="01596-125">[![Entité de film](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="01596-125">[![Movie Entity](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span></span>

<span data-ttu-id="01596-126">Nous avons ajouté une colonne dans le modèle de base de données, mais ne conscient pas les vues à son sujet.</span><span class="sxs-lookup"><span data-stu-id="01596-126">We've added a column in the database model, but the Views don't know about it.</span></span>

## <a name="update-views-with-model-changes"></a><span data-ttu-id="01596-127">Vues de mise à jour avec les modifications apportées au modèle</span><span class="sxs-lookup"><span data-stu-id="01596-127">Update Views with Model Changes</span></span>

<span data-ttu-id="01596-128">Il existe plusieurs façons, nous pouvons mettre à jour nos modèles de vue pour refléter la nouvelle colonne de contrôle d’accès.</span><span class="sxs-lookup"><span data-stu-id="01596-128">There are a few ways we could update our view templates to reflect the new Rating column.</span></span> <span data-ttu-id="01596-129">Étant donné que nous avons créé ces vues en les générant par le biais de la boîte de dialogue Ajouter une vue, nous pourrions supprimez-les et recréez-les à nouveau.</span><span class="sxs-lookup"><span data-stu-id="01596-129">Since we created these Views by generating them via the Add View dialog, we could delete them and recreate them again.</span></span> <span data-ttu-id="01596-130">Toutefois, généralement personnes sera déjà modifications apportées à leurs modèles de vue à partir de la génération initiale généré automatiquement et souhaitez ajouter ou supprimer des champs manuellement, tout comme nous l’avons fait avec le champ ID pour créer.</span><span class="sxs-lookup"><span data-stu-id="01596-130">However, typically people will have already made modifications to their View templates from the initial scaffolded generation and will want to add or delete fields manually, just as we did with the ID field for Create.</span></span>

<span data-ttu-id="01596-131">Ouvrez le modèle \Views\Movies\Index.aspx et ajoutez un &lt;th&gt;évaluation&lt;/th&gt; au début de la table Movie.</span><span class="sxs-lookup"><span data-stu-id="01596-131">Open up the \Views\Movies\Index.aspx template and add a &lt;th&gt;Rating&lt;/th&gt; to the head of the Movie table.</span></span> <span data-ttu-id="01596-132">J’ai ajouté mes après Genre.</span><span class="sxs-lookup"><span data-stu-id="01596-132">I added mine after Genre.</span></span> <span data-ttu-id="01596-133">Puis, dans la même position de colonne, mais plus bas, ajoutez une ligne pour notre nouvelle évaluation de la sortie.</span><span class="sxs-lookup"><span data-stu-id="01596-133">Then, in the same column position but lower down, add a line to output our new Rating.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

<span data-ttu-id="01596-134">Notre modèle Index.aspx final doit ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="01596-134">Our final Index.aspx template will look like this:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

<span data-ttu-id="01596-135">Nous allons ouvrir le modèle \Views\Movies\Create.aspx, puis ajoutez une étiquette et une zone de texte pour notre nouvelle propriété de contrôle d’accès :</span><span class="sxs-lookup"><span data-stu-id="01596-135">Let's then open up the \Views\Movies\Create.aspx template and add a Label and Textbox for our new Rating property:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

<span data-ttu-id="01596-136">Notre modèle Create.aspx final ressembler à ceci et nous allons modifier le titre et la base de données secondaire de notre navigateur &lt;h2&gt; titre à quelque chose comme « Créer un film » pendant que nous sommes ici !</span><span class="sxs-lookup"><span data-stu-id="01596-136">Our final Create.aspx template will look like this, and let's change our browser's title and secondary &lt;h2&gt; title to something like "Create a Movie" while we're in here!</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

<span data-ttu-id="01596-137">Exécutez votre application et que vous avez maintenant un nouveau champ dans la base de données qui a été ajouté à la page Create.</span><span class="sxs-lookup"><span data-stu-id="01596-137">Run your app and now you've got a new field in the database that's been added to the Create page.</span></span> <span data-ttu-id="01596-138">Ajouter un nouveau film - cette fois avec une évaluation égale à - et cliquez sur Créer.</span><span class="sxs-lookup"><span data-stu-id="01596-138">Add a new Movie - this time with a Rating - and click Create.</span></span>

<span data-ttu-id="01596-139">[![Créer un film - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="01596-139">[![Create a Movie - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span></span>

<span data-ttu-id="01596-140">Après avoir cliqué sur Créer, vous êtes envoyé à la page d’Index où vous nouveau film est répertorié avec la nouvelle colonne de classement dans la base de données</span><span class="sxs-lookup"><span data-stu-id="01596-140">After you click Create, you're sent to the Index page where you new Movie is listed with the new Rating Column in the database</span></span>

<span data-ttu-id="01596-141">[![Liste de films - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="01596-141">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span></span>

<span data-ttu-id="01596-142">Ce didacticiel de base a été de vous lancer dans la création de contrôleurs, leur association avec des vues et la transmission autour des données codées en dur.</span><span class="sxs-lookup"><span data-stu-id="01596-142">This basic tutorial got you started making Controllers, associating them with Views and passing around hard-coded data.</span></span> <span data-ttu-id="01596-143">Puis nous avons créé et conçu une base de données et insérer des données dans.</span><span class="sxs-lookup"><span data-stu-id="01596-143">Then we created and designed a Database and put some data it in.</span></span> <span data-ttu-id="01596-144">Nous avons extrait les données de la base de données et il affiché dans un tableau HTML.</span><span class="sxs-lookup"><span data-stu-id="01596-144">We retrieved the data from the database and displayed it in an HTML table.</span></span> <span data-ttu-id="01596-145">Ensuite, nous avons ajouté un formulaire de création qui permettent à l’utilisateur d’ajouter des données à la base de données eux-mêmes à partir de l’Application Web.</span><span class="sxs-lookup"><span data-stu-id="01596-145">Then we added a Create form that let the user add data to the database themselves from within the Web Application.</span></span> <span data-ttu-id="01596-146">Nous avons ajouté la validation, puis apportées à la validation d’utiliser JavaScript côté client.</span><span class="sxs-lookup"><span data-stu-id="01596-146">We added validation, then made the validation use JavaScript on the client-side.</span></span> <span data-ttu-id="01596-147">Enfin, nous modifié la base de données pour inclure une nouvelle colonne de données, puis mis à jour de nos deux pages pour créer et afficher ces nouvelles données.</span><span class="sxs-lookup"><span data-stu-id="01596-147">Finally, we changed the database to include a new column of data, then updated our two pages to create and display this new data.</span></span>

<span data-ttu-id="01596-148">J’ai maintenant vous encourageons à passer à notre didacticiel de niveau intermédiaire «[Store de musique MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)», ainsi que les nombreuses vidéos et à vos ressources [ https://asp.net/mvc ](https://asp.net/mvc) pour en savoir plus sur ASP.NET MVC !</span><span class="sxs-lookup"><span data-stu-id="01596-148">I now encourage you to move on to our intermediate-level tutorial "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" as well as the many videos and resources at [https://asp.net/mvc](https://asp.net/mvc) to learn even more about ASP.NET MVC!</span></span>

<span data-ttu-id="01596-149">Bonne lecture !</span><span class="sxs-lookup"><span data-stu-id="01596-149">Enjoy!</span></span>

- <span data-ttu-id="01596-150">Scott Hanselman - [ http://hanselman.com ](http://hanselman.com) et [ @shanselman ](http://twitter.com/shanselman) sur Twitter.</span><span class="sxs-lookup"><span data-stu-id="01596-150">Scott Hanselman - [http://hanselman.com](http://hanselman.com) and [@shanselman](http://twitter.com/shanselman) on Twitter.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="01596-151">Précédent</span><span class="sxs-lookup"><span data-stu-id="01596-151">Previous</span></span>](getting-started-with-mvc-part7.md)
