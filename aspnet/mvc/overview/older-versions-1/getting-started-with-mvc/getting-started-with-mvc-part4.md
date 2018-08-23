---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Création d’une base de données | Microsoft Docs
author: shanselman
description: Il s’agit d’un didacticiel de débutant qui présente les principes de base d’ASP.NET MVC. Créer une application web simple qui lit et écrit à partir d’une base de données.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: 596a491b4152da341a7779236dab17967a6de670
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832553"
---
<a name="creating-a-database"></a><span data-ttu-id="72b87-104">Création d’une base de données</span><span class="sxs-lookup"><span data-stu-id="72b87-104">Creating a Database</span></span>
====================
<span data-ttu-id="72b87-105">par [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="72b87-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="72b87-106">Il s’agit d’un didacticiel de débutant qui présente les principes de base d’ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="72b87-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="72b87-107">Vous allez créer une application web simple qui lit et écrit à partir d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="72b87-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="72b87-108">Visitez le [centre d’apprentissage ASP.NET MVC](../../../index.md) pour rechercher d’autres ASP.NET MVC didacticiels et exemples.</span><span class="sxs-lookup"><span data-stu-id="72b87-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="72b87-109">Dans cette section, nous allons créer une nouvelle base de SQL Express que nous allons utiliser pour stocker et récupérer de nos données de film.</span><span class="sxs-lookup"><span data-stu-id="72b87-109">In this section we are going to create a new SQL Express database that we'll use to store and retrieve our movie data.</span></span> <span data-ttu-id="72b87-110">Dans l’IDE Visual Web Developer, sélectionnez Affichage | Explorateur de serveurs.</span><span class="sxs-lookup"><span data-stu-id="72b87-110">From within the Visual Web Developer IDE, select View | Server Explorer.</span></span> <span data-ttu-id="72b87-111">Cliquez avec le bouton droit sur connexions de données et cliquez sur Ajouter une connexion...</span><span class="sxs-lookup"><span data-stu-id="72b87-111">Right click on Data Connections and click Add Connection...</span></span>

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

<span data-ttu-id="72b87-113">Dans la boîte de dialogue Choisir la Source de données, sélectionnez Microsoft SQL Server et cliquez sur Continuer.</span><span class="sxs-lookup"><span data-stu-id="72b87-113">In the Choose Data Source dialog, select Microsoft SQL Server and select Continue.</span></span>

![](getting-started-with-mvc-part4/_static/image2.png)

<span data-ttu-id="72b87-114">Dans la boîte de dialogue Ajouter une connexion, entrez «. \SQLEXPRESS » pour le nom de votre serveur, puis entrez « Movies » comme nom pour votre nouvelle base de données.</span><span class="sxs-lookup"><span data-stu-id="72b87-114">In the Add Connection dialog, enter ".\SQLEXPRESS" for your Server Name, and enter "Movies" as the name for your new database.</span></span>

<span data-ttu-id="72b87-115">[![Ajouter la boîte de dialogue Connexion](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="72b87-115">[![Add Connection dialog](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span></span>

<span data-ttu-id="72b87-116">Cliquez sur OK et vous demandera si vous souhaitez créer cette base de données.</span><span class="sxs-lookup"><span data-stu-id="72b87-116">Click OK and you'll be asked if you want to create that database.</span></span> <span data-ttu-id="72b87-117">Sélectionnez Oui.</span><span class="sxs-lookup"><span data-stu-id="72b87-117">Select yes.</span></span>

<span data-ttu-id="72b87-118">[![Créez des films ?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="72b87-118">[![Create Movies?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span></span>

<span data-ttu-id="72b87-119">Vous avez maintenant une base de données vide dans l’Explorateur de serveurs.</span><span class="sxs-lookup"><span data-stu-id="72b87-119">Now you've got an empty database in Server Explorer.</span></span>

![Ajouter une nouvelle Table](getting-started-with-mvc-part4/_static/image7.png)

<span data-ttu-id="72b87-121">Cliquez avec le bouton droit sur les Tables et cliquez sur Ajouter une Table.</span><span class="sxs-lookup"><span data-stu-id="72b87-121">Right click on Tables and click Add Table.</span></span> <span data-ttu-id="72b87-122">Le Concepteur de tables s’affiche.</span><span class="sxs-lookup"><span data-stu-id="72b87-122">The Table Designer will appear.</span></span> <span data-ttu-id="72b87-123">Ajouter des colonnes pour l’Id, Title, ReleaseDate, Genre et prix.</span><span class="sxs-lookup"><span data-stu-id="72b87-123">Add columns for Id, Title, ReleaseDate, Genre, and Price.</span></span> <span data-ttu-id="72b87-124">Cliquez avec le bouton droit sur la colonne d’ID et cliquez sur définie la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="72b87-124">Right click on the ID column and click set Primary Key.</span></span> <span data-ttu-id="72b87-125">Voici quelles mes zones de conception ressemble.</span><span class="sxs-lookup"><span data-stu-id="72b87-125">Here's what my design areas looks like.</span></span>

<span data-ttu-id="72b87-126">[![Éditeur de Table de base de données](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="72b87-126">[![Database Table Editor](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span></span>

<span data-ttu-id="72b87-127">En outre, sélectionnez la colonne d’Id et sous propriétés de colonne ci-dessous, remplacez « Spécification d’identité » sur « Oui ».</span><span class="sxs-lookup"><span data-stu-id="72b87-127">Also, select the Id column and under Column Properties below change "Identity Specification" to "Yes."</span></span>

<span data-ttu-id="72b87-128">[![IsIdentity - propriétés des colonnes](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="72b87-128">[![IsIdentity - Column Properties](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span></span>

<span data-ttu-id="72b87-129">Lorsque c’est terminé, cliquez sur l’icône Enregistrer dans la barre d’outils ou sélectionnez fichier | Enregistrer dans le menu et nommez votre table «**film**» (singulier).</span><span class="sxs-lookup"><span data-stu-id="72b87-129">When you've got it done, click the Save icon in the toolbar or select File | Save from the menu, and name your table "**Movie**" (singular).</span></span> <span data-ttu-id="72b87-130">Nous avons une base de données et une table !</span><span class="sxs-lookup"><span data-stu-id="72b87-130">We've got a database and a table!</span></span>

<span data-ttu-id="72b87-131">[![Choisissez le nom](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="72b87-131">[![Choose Name](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span></span>

<span data-ttu-id="72b87-132">Revenez à l’Explorateur de serveurs et cliquez avec le bouton droit sur la table Movie, puis sélectionnez « Afficher des données de Table ».</span><span class="sxs-lookup"><span data-stu-id="72b87-132">Go back to Server Explorer and right click the Movie table, then select "Show Table Data."</span></span> <span data-ttu-id="72b87-133">Entrez quelques films par conséquent, notre base de données a des données.</span><span class="sxs-lookup"><span data-stu-id="72b87-133">Enter a few movies so our database has some data.</span></span>

<span data-ttu-id="72b87-134">[![Modification de la Table de base de données](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span><span class="sxs-lookup"><span data-stu-id="72b87-134">[![Database Table Editing](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span></span>

## <a name="creating-a-model"></a><span data-ttu-id="72b87-135">Création d’un modèle</span><span class="sxs-lookup"><span data-stu-id="72b87-135">Creating a Model</span></span>

<span data-ttu-id="72b87-136">Maintenant, revenez à l’Explorateur de solutions sur le côté droit de l’IDE et avec le bouton droit sur le dossier de modèles et sélectionnez Ajouter | Nouvel élément.</span><span class="sxs-lookup"><span data-stu-id="72b87-136">Now, switch back to the Solution Explorer on the right hand side of the IDE and right-click on the Models folder and select Add | New Item.</span></span>

<span data-ttu-id="72b87-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="72b87-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span></span>

<span data-ttu-id="72b87-138">Nous allons créer un modèle d’entité à partir de notre nouvelle base de données.</span><span class="sxs-lookup"><span data-stu-id="72b87-138">We're going to create an Entity Model from our new database.</span></span> <span data-ttu-id="72b87-139">Cette opération ajoute un ensemble de classes à notre projet qui facilite pour nous interroger et manipuler les données au sein de notre base de données.</span><span class="sxs-lookup"><span data-stu-id="72b87-139">This will add a set of classes to our project that makes it easy for us to query and manipulate the data within our database.</span></span> <span data-ttu-id="72b87-140">Sélectionnez le nœud de données sur le côté gauche de la boîte de dialogue, puis sélectionnez le modèle d’élément ADO.NET Entity Data Model.</span><span class="sxs-lookup"><span data-stu-id="72b87-140">Select the Data node on the left-hand side of the dialog, and then select the ADO.NET Entity Data Model item template.</span></span> <span data-ttu-id="72b87-141">Nommez-le Movies.edmx.</span><span class="sxs-lookup"><span data-stu-id="72b87-141">Name it Movies.edmx.</span></span>

<span data-ttu-id="72b87-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span><span class="sxs-lookup"><span data-stu-id="72b87-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span></span>

<span data-ttu-id="72b87-143">Cliquez sur le bouton « Ajouter ».</span><span class="sxs-lookup"><span data-stu-id="72b87-143">Click the "Add" button.</span></span> <span data-ttu-id="72b87-144">Cela lance le « Assistant EDM ».</span><span class="sxs-lookup"><span data-stu-id="72b87-144">This will then launch the "Entity Data Model Wizard".</span></span>

<span data-ttu-id="72b87-145">Dans la nouvelle boîte de dialogue qui s’affiche, sélectionnez Générer à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="72b87-145">In the new dialog that pops up, select Generate from Database.</span></span> <span data-ttu-id="72b87-146">Étant donné que nous avons simplement une base de données, nous devons uniquement indiquer à Entity Framework sur notre nouvelle base de données et sa table.</span><span class="sxs-lookup"><span data-stu-id="72b87-146">Since we've just made a database, we'll only need to tell the Entity Framework about our new database and its table.</span></span> <span data-ttu-id="72b87-147">Cliquez sur Suivant pour enregistrer notre connexion de base de données dans la configuration de notre application web.</span><span class="sxs-lookup"><span data-stu-id="72b87-147">Click Next to save our database connection in our web application's configuration.</span></span> <span data-ttu-id="72b87-148">Maintenant, vérifier les Tables et les films case à cocher et cliquez sur Terminer.</span><span class="sxs-lookup"><span data-stu-id="72b87-148">Now, check the Tables and Movie checkbox and click Finish.</span></span>

<span data-ttu-id="72b87-149">[![Assistant Entity Data Model](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span><span class="sxs-lookup"><span data-stu-id="72b87-149">[![Entity Data Model Wizard](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span></span>

<span data-ttu-id="72b87-150">Maintenant, nous pouvons voir notre nouvelle table Movie dans Entity Framework Designer et y accéder à partir du code.</span><span class="sxs-lookup"><span data-stu-id="72b87-150">Now we can see our new Movie table in the Entity Framework Designer and access it from code.</span></span>

<span data-ttu-id="72b87-151">[![Films - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="72b87-151">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span></span>

<span data-ttu-id="72b87-152">Sur l’aire de conception, vous pouvez voir une classe « Film ».</span><span class="sxs-lookup"><span data-stu-id="72b87-152">On the design surface you can see a "Movie" class.</span></span> <span data-ttu-id="72b87-153">Cette classe correspond à la table « Movie » dans notre base de données, et chaque propriété qu’il contient est mappée à une colonne avec la table.</span><span class="sxs-lookup"><span data-stu-id="72b87-153">This class maps to the "Movie" table in our database, and each property within it maps to a column with the table.</span></span> <span data-ttu-id="72b87-154">Chaque instance d’une classe « Film » correspond à une ligne dans la table « Movie ».</span><span class="sxs-lookup"><span data-stu-id="72b87-154">Each instance of a "Movie" class will correspond to a row within the "Movie" table.</span></span>

<span data-ttu-id="72b87-155">Si vous n’aimez pas la dénomination par défaut et le mappage des conventions utilisées par Entity Framework, vous pouvez utiliser le Concepteur d’Entity Framework pour modifier ou de les personnaliser.</span><span class="sxs-lookup"><span data-stu-id="72b87-155">If you don't like the default naming and mapping conventions used by the Entity Framework, you can use the Entity Framework designer to change or customize them.</span></span> <span data-ttu-id="72b87-156">Pour cette application, nous allons utiliser les valeurs par défaut et enregistrez simplement le fichier en tant que-est.</span><span class="sxs-lookup"><span data-stu-id="72b87-156">For this application we'll use the defaults and just save the file as-is.</span></span>

<span data-ttu-id="72b87-157">Maintenant, nous allons travailler avec des données réelles !</span><span class="sxs-lookup"><span data-stu-id="72b87-157">Now, let's work with some real data!</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="72b87-158">[Précédent](getting-started-with-mvc-part3.md)
> [Suivant](getting-started-with-mvc-part5.md)</span><span class="sxs-lookup"><span data-stu-id="72b87-158">[Previous](getting-started-with-mvc-part3.md)
[Next](getting-started-with-mvc-part5.md)</span></span>
