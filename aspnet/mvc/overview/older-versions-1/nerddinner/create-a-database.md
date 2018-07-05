---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Créer une base de données | Microsoft Docs
author: microsoft
description: Étape 2 montre les étapes pour créer la base de données contenant tous le dîner et RSVP des données pour notre application NerdDinner.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: 01c6d3c63780e492b97aa54a92f3982d4c18f9e5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371747"
---
<a name="create-a-database"></a><span data-ttu-id="b4fbe-103">Créer une base de données</span><span class="sxs-lookup"><span data-stu-id="b4fbe-103">Create a Database</span></span>
====================
<span data-ttu-id="b4fbe-104">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="b4fbe-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="b4fbe-105">Télécharger PDF</span><span class="sxs-lookup"><span data-stu-id="b4fbe-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="b4fbe-106">Il s’agit d’étape 2 a gratuit [« « l’application NerdDinner](introducing-the-nerddinner-tutorial.md) qui présente en détail comment créer un petit mais terminé, l’application web à l’aide d’ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="b4fbe-106">This is step 2 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="b4fbe-107">Étape 2 montre les étapes pour créer la base de données contenant tous le dîner et RSVP des données pour notre application NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="b4fbe-107">Step 2 shows the steps to create the database holding all of the dinner and RSVP data for our NerdDinner application.</span></span>
> 
> <span data-ttu-id="b4fbe-108">Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre le [mise en route avec MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [Store de musique MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) didacticiels.</span><span class="sxs-lookup"><span data-stu-id="b4fbe-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-2-creating-the-database"></a><span data-ttu-id="b4fbe-109">NerdDinner étape 2 : Création de la base de données</span><span class="sxs-lookup"><span data-stu-id="b4fbe-109">NerdDinner Step 2: Creating the Database</span></span>

<span data-ttu-id="b4fbe-110">Nous allons utiliser une base de données pour stocker toutes les données dîner et RSVP pour notre application NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="b4fbe-110">We'll be using a database to store all of the Dinner and RSVP data for our NerdDinner application.</span></span>

<span data-ttu-id="b4fbe-111">Les étapes ci-dessous montrent la création de la base de données à l’aide de l’édition gratuite de SQL Server Express (que vous pouvez facilement installer à l’aide de la version 2 de la [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span><span class="sxs-lookup"><span data-stu-id="b4fbe-111">The steps below show creating the database using the free SQL Server Express edition (which you can easily install using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span></span> <span data-ttu-id="b4fbe-112">Tout le code que nous allons écrire fonctionne avec SQL Server Express et SQL Server complète.</span><span class="sxs-lookup"><span data-stu-id="b4fbe-112">All of the code we'll write works with both SQL Server Express and the full SQL Server.</span></span>

### <a name="creating-a-new-sql-server-express-database"></a><span data-ttu-id="b4fbe-113">Création d’une nouvelle base de données SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="b4fbe-113">Creating a new SQL Server Express database</span></span>

<span data-ttu-id="b4fbe-114">Nous allons commencer en cliquant sur notre projet web, puis sélectionnez le **Add -&gt;un nouvel élément** commande de menu :</span><span class="sxs-lookup"><span data-stu-id="b4fbe-114">We'll begin by right-clicking on our web project, and then select the **Add-&gt;New Item** menu command:</span></span>

![](create-a-database/_static/image1.png)

<span data-ttu-id="b4fbe-115">Cela fera apparaître la boîte de dialogue « Ajouter un nouvel élément » de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b4fbe-115">This will bring up Visual Studio's "Add New Item" dialog.</span></span> <span data-ttu-id="b4fbe-116">Nous allons filtrer par la catégorie « Données » et sélectionnez le modèle d’élément « Base de données SQL Server » :</span><span class="sxs-lookup"><span data-stu-id="b4fbe-116">We'll filter by the "Data" category and select the "SQL Server Database" item template:</span></span>

![](create-a-database/_static/image2.png)

<span data-ttu-id="b4fbe-117">Nous allons nommer la base de données SQL Server Express, que nous souhaitons créer « NerdDinner.mdf » et appuyez sur OK.</span><span class="sxs-lookup"><span data-stu-id="b4fbe-117">We'll name the SQL Server Express database we want to create "NerdDinner.mdf" and hit ok.</span></span> <span data-ttu-id="b4fbe-118">Visual Studio vous demande si nous souhaitons ajouter ce fichier à notre \App\_répertoire de données (qui est un répertoire déjà le programme d’installation avec accès en lecture et écriture des ACL de sécurité) :</span><span class="sxs-lookup"><span data-stu-id="b4fbe-118">Visual Studio will then ask us if we want to add this file to our \App\_Data directory (which is a directory already setup with both read and write security ACLs):</span></span>

![](create-a-database/_static/image3.png)

<span data-ttu-id="b4fbe-119">Nous cliquons sur « Oui » et notre nouvelle base de données est créé et ajouté à l’Explorateur de solutions :</span><span class="sxs-lookup"><span data-stu-id="b4fbe-119">We'll click "Yes" and our new database will be created and added to our Solution Explorer:</span></span>

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a><span data-ttu-id="b4fbe-120">Création de Tables au sein de notre base de données</span><span class="sxs-lookup"><span data-stu-id="b4fbe-120">Creating Tables within our Database</span></span>

<span data-ttu-id="b4fbe-121">Nous disposons désormais d’une nouvelle base de données vide.</span><span class="sxs-lookup"><span data-stu-id="b4fbe-121">We now have a new empty database.</span></span> <span data-ttu-id="b4fbe-122">Nous allons y ajouter des tables.</span><span class="sxs-lookup"><span data-stu-id="b4fbe-122">Let's add some tables to it.</span></span>

<span data-ttu-id="b4fbe-123">Pour ce faire, nous allons vous accédez à la fenêtre d’onglet « Explorateur de serveurs » dans Visual Studio, qui permet de gérer les serveurs et bases de données.</span><span class="sxs-lookup"><span data-stu-id="b4fbe-123">To do this we'll navigate to the "Server Explorer" tab window within Visual Studio, which enables us to manage databases and servers.</span></span> <span data-ttu-id="b4fbe-124">Bases de données SQL Server Express, stockées dans le \App\_dossier de données de notre application apparaissent automatiquement dans l’Explorateur de serveurs.</span><span class="sxs-lookup"><span data-stu-id="b4fbe-124">SQL Server Express databases stored in the \App\_Data folder of our application will automatically show up within the Server Explorer.</span></span> <span data-ttu-id="b4fbe-125">Nous pouvons éventuellement utiliser l’icône « Se connecter à la base de données » en haut de la fenêtre « Explorateur de serveurs » pour ajouter d’autres bases de données SQL Server (locales et distants) à la liste ainsi :</span><span class="sxs-lookup"><span data-stu-id="b4fbe-125">We can optionally use the "Connect to Database" icon on the top of the "Server Explorer" window to add additional SQL Server databases (both local and remote) to the list as well:</span></span>

![](create-a-database/_static/image5.png)

<span data-ttu-id="b4fbe-126">Nous allons ajouter deux tables à notre base de données NerdDinner : un pour stocker nos dîners et l’autre pour effectuer le suivi de protocole RSVP acceptations leur.</span><span class="sxs-lookup"><span data-stu-id="b4fbe-126">We will add two tables to our NerdDinner database – one to store our Dinners, and the other to track RSVP acceptances to them.</span></span> <span data-ttu-id="b4fbe-127">Nous pouvons créer des tables en cliquant sur le dossier « Tables » au sein de notre base de données et en choisissant la commande de menu « Ajouter une nouvelle Table » :</span><span class="sxs-lookup"><span data-stu-id="b4fbe-127">We can create new tables by right-clicking on the "Tables" folder within our database and choosing the "Add New Table" menu command:</span></span>

![](create-a-database/_static/image6.png)

<span data-ttu-id="b4fbe-128">Un concepteur de tables qui nous permet de configurer le schéma de la table s’ouvre.</span><span class="sxs-lookup"><span data-stu-id="b4fbe-128">This will open up a table designer that allows us to configure the schema of our table.</span></span> <span data-ttu-id="b4fbe-129">Pour notre table « Dîners », nous allons ajouter 10 colonnes de données :</span><span class="sxs-lookup"><span data-stu-id="b4fbe-129">For our "Dinners" table we will add 10 columns of data:</span></span>

![](create-a-database/_static/image7.png)

<span data-ttu-id="b4fbe-130">Nous voulons la colonne « DinnerID » soit une clé primaire unique pour la table.</span><span class="sxs-lookup"><span data-stu-id="b4fbe-130">We want the "DinnerID" column to be a unique primary key for the table.</span></span> <span data-ttu-id="b4fbe-131">Nous pouvons le configurer en cliquant sur la colonne « DinnerID » et en choisissant l’élément de menu « Définir la clé primaire » :</span><span class="sxs-lookup"><span data-stu-id="b4fbe-131">We can configure this by right-clicking on the "DinnerID" column and choosing the "Set Primary Key" menu item:</span></span>

![](create-a-database/_static/image8.png)

<span data-ttu-id="b4fbe-132">En plus de rendre DinnerID une clé primaire, nous voulons également configurez-le en tant que « identity » colonne dont la valeur est incrémentée automatiquement comme de nouvelles lignes de données sont ajoutées à la table (ce qui signifie que la première ligne dîner insérée aura un DinnerID 1, le deuxième inséré ligne aura un DinnerID de 2, etc.).</span><span class="sxs-lookup"><span data-stu-id="b4fbe-132">In addition to making DinnerID a primary key, we also want configure it as an "identity" column whose value is automatically incremented as new rows of data are added to the table (meaning the first inserted Dinner row will have a DinnerID of 1, the second inserted row will have a DinnerID of 2, etc).</span></span>

<span data-ttu-id="b4fbe-133">Nous pouvons ce faire, sélectionnez la colonne « DinnerID », puis utilisez l’éditeur « Propriétés de colonne » pour définir la propriété « (identité est) » sur la colonne « Oui ».</span><span class="sxs-lookup"><span data-stu-id="b4fbe-133">We can do this by selecting the "DinnerID" column and then use the "Column Properties" editor to set the "(Is Identity)" property on the column to "Yes".</span></span> <span data-ttu-id="b4fbe-134">Nous allons utiliser les valeurs par défaut de l’identité standard (commencent à 1 et incrémente de 1 sur chaque nouvelle ligne dîner) :</span><span class="sxs-lookup"><span data-stu-id="b4fbe-134">We will use the standard identity defaults (start at 1 and increment 1 on each new Dinner row):</span></span>

![](create-a-database/_static/image9.png)

<span data-ttu-id="b4fbe-135">Nous allons ensuite enregistrer notre table en tapant Ctrl + S ou à l’aide de la **fichier -&gt;enregistrer** commande de menu.</span><span class="sxs-lookup"><span data-stu-id="b4fbe-135">We'll then save our table by typing Ctrl-S or by using the **File-&gt;Save** menu command.</span></span> <span data-ttu-id="b4fbe-136">Cela vous invite à nous pour nommer la table.</span><span class="sxs-lookup"><span data-stu-id="b4fbe-136">This will prompt us to name the table.</span></span> <span data-ttu-id="b4fbe-137">Nous nommerons « Dîners » :</span><span class="sxs-lookup"><span data-stu-id="b4fbe-137">We'll name it "Dinners":</span></span>

![](create-a-database/_static/image10.png)

<span data-ttu-id="b4fbe-138">Notre nouvelle table dîners puis apparaîtront dans notre base de données dans l’Explorateur de serveurs.</span><span class="sxs-lookup"><span data-stu-id="b4fbe-138">Our new Dinners table will then show up within our database in the server explorer.</span></span>

<span data-ttu-id="b4fbe-139">Nous allons puis répétez les étapes ci-dessus et créer une table « RSVP ».</span><span class="sxs-lookup"><span data-stu-id="b4fbe-139">We'll then repeat the above steps and create a "RSVP" table.</span></span> <span data-ttu-id="b4fbe-140">Cette table avec comporte 3 colonnes.</span><span class="sxs-lookup"><span data-stu-id="b4fbe-140">This table with have 3 columns.</span></span> <span data-ttu-id="b4fbe-141">Nous le programme d’installation de la colonne RsvpID comme clé primaire et également rendre une colonne d’identité :</span><span class="sxs-lookup"><span data-stu-id="b4fbe-141">We will setup the RsvpID column as the primary key, and also make it an identity column:</span></span>

![](create-a-database/_static/image11.png)

<span data-ttu-id="b4fbe-142">Nous allons enregistrer et attribuez-lui le nom « RSVP ».</span><span class="sxs-lookup"><span data-stu-id="b4fbe-142">We'll save it and give it the name "RSVP".</span></span>

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a><span data-ttu-id="b4fbe-143">Configuration d’une relation de clé étrangère entre les Tables</span><span class="sxs-lookup"><span data-stu-id="b4fbe-143">Setting up a Foreign Key Relationship between Tables</span></span>

<span data-ttu-id="b4fbe-144">Maintenant, nous avons deux tables au sein de notre base de données.</span><span class="sxs-lookup"><span data-stu-id="b4fbe-144">We now have two tables within our database.</span></span> <span data-ttu-id="b4fbe-145">Notre dernière étape de conception de schéma sera pour configurer une relation « un-à-plusieurs » entre ces deux tables – afin que nous pouvons associer chaque ligne Dinner à zéro ou plusieurs lignes RSVP qui s’y appliquent.</span><span class="sxs-lookup"><span data-stu-id="b4fbe-145">Our last schema design step will be to setup a "one-to-many" relationship between these two tables – so that we can associate each Dinner row with zero or more RSVP rows that apply to it.</span></span> <span data-ttu-id="b4fbe-146">Nous le ferons en configurant la RSVP colonne du tableau « DinnerID » pour avoir une relation de clé étrangère à la colonne « DinnerID » dans la table « Dîners ».</span><span class="sxs-lookup"><span data-stu-id="b4fbe-146">We will do this by configuring the RSVP table's "DinnerID" column to have a foreign-key relationship to the "DinnerID" column in the "Dinners" table.</span></span>

<span data-ttu-id="b4fbe-147">Pour ce faire, nous allons ouvrir le tableau RSVP dans le Concepteur de tables en double-cliquant dessus dans l’Explorateur de serveurs.</span><span class="sxs-lookup"><span data-stu-id="b4fbe-147">To do this we'll open up the RSVP table within the table designer by double-clicking it in the server explorer.</span></span> <span data-ttu-id="b4fbe-148">Nous allons ensuite sélectionner la colonne « DinnerID » qu’il contient, avec le bouton droit, puis choisissez la commande de menu contextuel « Relationshps... » :</span><span class="sxs-lookup"><span data-stu-id="b4fbe-148">We'll then select the "DinnerID" column within it, right-click, and choose the "Relationshps…" context menu command:</span></span>

![](create-a-database/_static/image12.png)

<span data-ttu-id="b4fbe-149">Cela fera apparaître une boîte de dialogue que nous pouvons utiliser pour les relations entre les tables d’installation :</span><span class="sxs-lookup"><span data-stu-id="b4fbe-149">This will bring up a dialog that we can use to setup relationships between tables:</span></span>

![](create-a-database/_static/image13.png)

<span data-ttu-id="b4fbe-150">Nous allons cliquez sur le bouton « Ajouter » pour ajouter une nouvelle relation à la boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="b4fbe-150">We'll click the "Add" button to add a new relationship to the dialog.</span></span> <span data-ttu-id="b4fbe-151">Une fois qu’une relation a été ajoutée, nous allons développez le nœud d’arborescence « Tables et spécification de colonne » dans la grille des propriétés à droite de la boîte de dialogue, puis cliquez sur le bouton «... » à droite de celui-ci :</span><span class="sxs-lookup"><span data-stu-id="b4fbe-151">Once a relationship has been added, we'll expand the "Tables and Column Specification" tree-view node within the property grid to the right of the dialog, and then click the "…" button to the right of it:</span></span>

![](create-a-database/_static/image14.png)

<span data-ttu-id="b4fbe-152">En cliquant sur le bouton «... » s’affiche une autre boîte de dialogue qui permet de spécifier les tables et les colonnes sont impliqués dans la relation, ainsi que nous permettent de nommer la relation.</span><span class="sxs-lookup"><span data-stu-id="b4fbe-152">Clicking the "…" button will bring up another dialog that allows us to specify which tables and columns are involved in the relationship, as well as allow us to name the relationship.</span></span>

<span data-ttu-id="b4fbe-153">Nous modifie la Table de clé primaire pour être « Dîners » et sélectionnez la colonne « DinnerID » dans la table dîners comme clé primaire.</span><span class="sxs-lookup"><span data-stu-id="b4fbe-153">We will change the Primary Key Table to be "Dinners", and select the "DinnerID" column within the Dinners table as the primary key.</span></span> <span data-ttu-id="b4fbe-154">Notre table RSVP sera la table de clé étrangère et le protocole RSVP. Colonne de DinnerID sera associé en tant que la clé étrangère :</span><span class="sxs-lookup"><span data-stu-id="b4fbe-154">Our RSVP table will be the foreign-key table, and the RSVP.DinnerID column will be associated as the foreign-key:</span></span>

![](create-a-database/_static/image15.png)

<span data-ttu-id="b4fbe-155">Maintenant chaque ligne dans la table RSVP à associer à une ligne dans la table dîner.</span><span class="sxs-lookup"><span data-stu-id="b4fbe-155">Now each row in the RSVP table will be associated with a row in the Dinner table.</span></span> <span data-ttu-id="b4fbe-156">SQL Server sera maintenir l’intégrité référentielle pour nous et nous empêche d’ajouter une nouvelle ligne RSVP si elle ne pointe pas vers une ligne dîner valide.</span><span class="sxs-lookup"><span data-stu-id="b4fbe-156">SQL Server will maintain referential integrity for us – and prevent us from adding a new RSVP row if it does not point to a valid Dinner row.</span></span> <span data-ttu-id="b4fbe-157">Il nous empêchera également de supprimer une ligne dîner s’il existe toujours RSVP lignes faisant référence à celui-ci.</span><span class="sxs-lookup"><span data-stu-id="b4fbe-157">It will also prevent us from deleting a Dinner row if there are still RSVP rows referring to it.</span></span>

### <a name="adding-data-to-our-tables"></a><span data-ttu-id="b4fbe-158">Ajout de données à nos Tables</span><span class="sxs-lookup"><span data-stu-id="b4fbe-158">Adding Data to our Tables</span></span>

<span data-ttu-id="b4fbe-159">Nous allons terminer en ajoutant des exemples de données à la table dîners.</span><span class="sxs-lookup"><span data-stu-id="b4fbe-159">Let's finish by adding some sample data to our Dinners table.</span></span> <span data-ttu-id="b4fbe-160">Nous pouvons ajouter des données à une table en cliquant dessus dans l’Explorateur de serveurs et en choisissant la commande « Afficher les données de Table » :</span><span class="sxs-lookup"><span data-stu-id="b4fbe-160">We can add data to a table by right-clicking on it within the Server Explorer and choosing the "Show Table Data" command:</span></span>

![](create-a-database/_static/image16.png)

<span data-ttu-id="b4fbe-161">Nous allons ajouter quelques lignes de données dîner que nous pouvons utiliser plus tard que nous commencions à implémenter l’application :</span><span class="sxs-lookup"><span data-stu-id="b4fbe-161">We'll add a few rows of Dinner data that we can use later as we start implementing the application:</span></span>

![](create-a-database/_static/image17.png)

### <a name="next-step"></a><span data-ttu-id="b4fbe-162">Étape suivante</span><span class="sxs-lookup"><span data-stu-id="b4fbe-162">Next Step</span></span>

<span data-ttu-id="b4fbe-163">Nous avons terminé la création de notre base de données.</span><span class="sxs-lookup"><span data-stu-id="b4fbe-163">We've finished creating our database.</span></span> <span data-ttu-id="b4fbe-164">Nous allons maintenant créer des classes de modèle que nous pouvons utiliser pour interroger et mettre à jour.</span><span class="sxs-lookup"><span data-stu-id="b4fbe-164">Let's now create model classes that we can use to query and update it.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b4fbe-165">[Précédent](create-a-new-aspnet-mvc-project.md)
> [Suivant](build-a-model-with-business-rule-validations.md)</span><span class="sxs-lookup"><span data-stu-id="b4fbe-165">[Previous](create-a-new-aspnet-mvc-project.md)
[Next](build-a-model-with-business-rule-validations.md)</span></span>
