---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Créer une base de données | Documents Microsoft
author: microsoft
description: Étape 2 illustre les étapes pour créer la base de données contenant tous le dîner et RSVP des données pour notre application NerdDinner.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: ba28d671bf13ec54b83b876462e2c23f90310037
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869138"
---
<a name="create-a-database"></a><span data-ttu-id="6a406-103">Créer une base de données</span><span class="sxs-lookup"><span data-stu-id="6a406-103">Create a Database</span></span>
====================
<span data-ttu-id="6a406-104">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="6a406-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="6a406-105">Télécharger le PDF</span><span class="sxs-lookup"><span data-stu-id="6a406-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="6a406-106">Il s’agit d’étape 2 a gratuit [« « l’application NerdDinner](introducing-the-nerddinner-tutorial.md) qui parcours à comment générer un petit, mais se termine, application web à l’aide d’ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="6a406-106">This is step 2 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="6a406-107">Étape 2 illustre les étapes pour créer la base de données contenant tous le dîner et RSVP des données pour notre application NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="6a406-107">Step 2 shows the steps to create the database holding all of the dinner and RSVP data for our NerdDinner application.</span></span>
> 
> <span data-ttu-id="6a406-108">Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre les [mise en route a démarré avec MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [magasin de musique MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) didacticiels.</span><span class="sxs-lookup"><span data-stu-id="6a406-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-2-creating-the-database"></a><span data-ttu-id="6a406-109">NerdDinner étape 2 : Création de la base de données</span><span class="sxs-lookup"><span data-stu-id="6a406-109">NerdDinner Step 2: Creating the Database</span></span>

<span data-ttu-id="6a406-110">Nous utiliserons une base de données pour stocker toutes les données dîner et RSVP pour notre application NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="6a406-110">We'll be using a database to store all of the Dinner and RSVP data for our NerdDinner application.</span></span>

<span data-ttu-id="6a406-111">Les étapes ci-dessous montrent la création de la base de données à l’aide de l’édition gratuite de SQL Server Express (que vous pouvez facilement installer à l’aide de V2 de la [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span><span class="sxs-lookup"><span data-stu-id="6a406-111">The steps below show creating the database using the free SQL Server Express edition (which you can easily install using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span></span> <span data-ttu-id="6a406-112">Tout le code que nous allons écrire fonctionne avec SQL Server Express et SQL Server complète.</span><span class="sxs-lookup"><span data-stu-id="6a406-112">All of the code we'll write works with both SQL Server Express and the full SQL Server.</span></span>

### <a name="creating-a-new-sql-server-express-database"></a><span data-ttu-id="6a406-113">Création d’une nouvelle base de données SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="6a406-113">Creating a new SQL Server Express database</span></span>

<span data-ttu-id="6a406-114">Nous allons commencer en cliquant sur notre projet web, puis sélectionnez le **Add -&gt;un nouvel élément** commande de menu :</span><span class="sxs-lookup"><span data-stu-id="6a406-114">We'll begin by right-clicking on our web project, and then select the **Add-&gt;New Item** menu command:</span></span>

![](create-a-database/_static/image1.png)

<span data-ttu-id="6a406-115">Cela affiche la boîte de dialogue « Ajouter un nouvel élément » de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6a406-115">This will bring up Visual Studio's "Add New Item" dialog.</span></span> <span data-ttu-id="6a406-116">Nous allons filtrer par la catégorie « Données » et sélectionnez le modèle d’élément « Base de données SQL Server » :</span><span class="sxs-lookup"><span data-stu-id="6a406-116">We'll filter by the "Data" category and select the "SQL Server Database" item template:</span></span>

![](create-a-database/_static/image2.png)

<span data-ttu-id="6a406-117">Nous allons nommer la base de données SQL Server Express, que nous voulons créer « NerdDinner.mdf » et appuyez sur OK.</span><span class="sxs-lookup"><span data-stu-id="6a406-117">We'll name the SQL Server Express database we want to create "NerdDinner.mdf" and hit ok.</span></span> <span data-ttu-id="6a406-118">Visual Studio vous demande si vous souhaitez ajouter ce fichier à notre \App\_répertoire de données (qui est un répertoire déjà le programme d’installation avec accès en lecture et écriture des ACL de sécurité) :</span><span class="sxs-lookup"><span data-stu-id="6a406-118">Visual Studio will then ask us if we want to add this file to our \App\_Data directory (which is a directory already setup with both read and write security ACLs):</span></span>

![](create-a-database/_static/image3.png)

<span data-ttu-id="6a406-119">Cliquez sur « Oui » et notre nouvelle base de données est créé et ajouté à l’Explorateur de solutions :</span><span class="sxs-lookup"><span data-stu-id="6a406-119">We'll click "Yes" and our new database will be created and added to our Solution Explorer:</span></span>

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a><span data-ttu-id="6a406-120">Création de Tables au sein de notre base de données</span><span class="sxs-lookup"><span data-stu-id="6a406-120">Creating Tables within our Database</span></span>

<span data-ttu-id="6a406-121">Nous disposons désormais d’une base de données vide.</span><span class="sxs-lookup"><span data-stu-id="6a406-121">We now have a new empty database.</span></span> <span data-ttu-id="6a406-122">Nous allons lui ajouter des tables.</span><span class="sxs-lookup"><span data-stu-id="6a406-122">Let's add some tables to it.</span></span>

<span data-ttu-id="6a406-123">Pour ce faire, nous allez accéder à la fenêtre d’onglet de « Explorateur de serveurs » dans Visual Studio, qui permet de gérer les serveurs et bases de données.</span><span class="sxs-lookup"><span data-stu-id="6a406-123">To do this we'll navigate to the "Server Explorer" tab window within Visual Studio, which enables us to manage databases and servers.</span></span> <span data-ttu-id="6a406-124">Bases de données SQL Server Express, stockées dans le \App\_dossier de données de notre application s’affichent automatiquement dans l’Explorateur de serveurs.</span><span class="sxs-lookup"><span data-stu-id="6a406-124">SQL Server Express databases stored in the \App\_Data folder of our application will automatically show up within the Server Explorer.</span></span> <span data-ttu-id="6a406-125">Nous pouvons éventuellement utiliser l’icône « Se connecter à la base de données » en haut de la fenêtre « Explorateur de serveurs » pour ajouter d’autres bases de données SQL Server (locales et distants) à la liste ainsi :</span><span class="sxs-lookup"><span data-stu-id="6a406-125">We can optionally use the "Connect to Database" icon on the top of the "Server Explorer" window to add additional SQL Server databases (both local and remote) to the list as well:</span></span>

![](create-a-database/_static/image5.png)

<span data-ttu-id="6a406-126">Nous allons ajouter deux tables à notre base de données de NerdDinner : un pour stocker notre préparés et l’autre pour effectuer le suivi de RSVP acceptations leur.</span><span class="sxs-lookup"><span data-stu-id="6a406-126">We will add two tables to our NerdDinner database – one to store our Dinners, and the other to track RSVP acceptances to them.</span></span> <span data-ttu-id="6a406-127">Nous pouvons créer des tables en cliquant sur le dossier « Tables » dans notre base de données et en choisissant la commande de menu « Ajouter une nouvelle Table » :</span><span class="sxs-lookup"><span data-stu-id="6a406-127">We can create new tables by right-clicking on the "Tables" folder within our database and choosing the "Add New Table" menu command:</span></span>

![](create-a-database/_static/image6.png)

<span data-ttu-id="6a406-128">Un concepteur de tables qui nous permet de configurer le schéma de la table s’ouvre.</span><span class="sxs-lookup"><span data-stu-id="6a406-128">This will open up a table designer that allows us to configure the schema of our table.</span></span> <span data-ttu-id="6a406-129">Pour notre table « Préparés », nous allons ajouter 10 colonnes de données :</span><span class="sxs-lookup"><span data-stu-id="6a406-129">For our "Dinners" table we will add 10 columns of data:</span></span>

![](create-a-database/_static/image7.png)

<span data-ttu-id="6a406-130">Nous voulons la colonne « DinnerID » soit une clé primaire unique pour la table.</span><span class="sxs-lookup"><span data-stu-id="6a406-130">We want the "DinnerID" column to be a unique primary key for the table.</span></span> <span data-ttu-id="6a406-131">Nous pouvons le configurer en cliquant sur la colonne « DinnerID » et en choisissant l’élément de menu « Définir la clé primaire » :</span><span class="sxs-lookup"><span data-stu-id="6a406-131">We can configure this by right-clicking on the "DinnerID" column and choosing the "Set Primary Key" menu item:</span></span>

![](create-a-database/_static/image8.png)

<span data-ttu-id="6a406-132">En plus de rendre DinnerID une clé primaire, nous souhaitons également nous configurez-le en tant que « identité » colonne dont la valeur augmente automatiquement lorsque de nouvelles lignes de données sont ajoutées à la table (ce qui signifie la première ligne Dinner insérée aura un DinnerID 1, le deuxième inséré ligne aura un DinnerID de 2, etc.).</span><span class="sxs-lookup"><span data-stu-id="6a406-132">In addition to making DinnerID a primary key, we also want configure it as an "identity" column whose value is automatically incremented as new rows of data are added to the table (meaning the first inserted Dinner row will have a DinnerID of 1, the second inserted row will have a DinnerID of 2, etc).</span></span>

<span data-ttu-id="6a406-133">Nous pouvons ce faire, sélectionnez la colonne « DinnerID », puis utilisez l’éditeur « Propriétés de colonne » pour définir la propriété « (identité est) » sur la colonne « Yes ».</span><span class="sxs-lookup"><span data-stu-id="6a406-133">We can do this by selecting the "DinnerID" column and then use the "Column Properties" editor to set the "(Is Identity)" property on the column to "Yes".</span></span> <span data-ttu-id="6a406-134">Nous allons utiliser les valeurs par défaut de l’identité standard (commencent à 1 et incrémente de 1 sur chaque nouvelle ligne Dinner) :</span><span class="sxs-lookup"><span data-stu-id="6a406-134">We will use the standard identity defaults (start at 1 and increment 1 on each new Dinner row):</span></span>

![](create-a-database/_static/image9.png)

<span data-ttu-id="6a406-135">Nous allons ensuite enregistrer notre table en tapant Ctrl + S ou à l’aide de la **fichier -&gt;enregistrer** commande de menu.</span><span class="sxs-lookup"><span data-stu-id="6a406-135">We'll then save our table by typing Ctrl-S or by using the **File-&gt;Save** menu command.</span></span> <span data-ttu-id="6a406-136">Il vous est demandé pour le nom de la table.</span><span class="sxs-lookup"><span data-stu-id="6a406-136">This will prompt us to name the table.</span></span> <span data-ttu-id="6a406-137">Nous allons nommez-le « Préparés » :</span><span class="sxs-lookup"><span data-stu-id="6a406-137">We'll name it "Dinners":</span></span>

![](create-a-database/_static/image10.png)

<span data-ttu-id="6a406-138">Notre nouvelle table préparés s’affiche alors dans notre base de données dans l’Explorateur de serveurs.</span><span class="sxs-lookup"><span data-stu-id="6a406-138">Our new Dinners table will then show up within our database in the server explorer.</span></span>

<span data-ttu-id="6a406-139">Nous allons puis répétez les étapes ci-dessus et créer une table « RSVP ».</span><span class="sxs-lookup"><span data-stu-id="6a406-139">We'll then repeat the above steps and create a "RSVP" table.</span></span> <span data-ttu-id="6a406-140">Cette table avec avoir 3 colonnes.</span><span class="sxs-lookup"><span data-stu-id="6a406-140">This table with have 3 columns.</span></span> <span data-ttu-id="6a406-141">Nous le programme d’installation de la colonne RsvpID comme clé primaire et également rendre une colonne d’identité :</span><span class="sxs-lookup"><span data-stu-id="6a406-141">We will setup the RsvpID column as the primary key, and also make it an identity column:</span></span>

![](create-a-database/_static/image11.png)

<span data-ttu-id="6a406-142">Nous allons enregistrer et attribuez-lui le nom « RSVP ».</span><span class="sxs-lookup"><span data-stu-id="6a406-142">We'll save it and give it the name "RSVP".</span></span>

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a><span data-ttu-id="6a406-143">Configuration d’une relation de clé étrangère entre les Tables</span><span class="sxs-lookup"><span data-stu-id="6a406-143">Setting up a Foreign Key Relationship between Tables</span></span>

<span data-ttu-id="6a406-144">Nous disposons de deux tables au sein de notre base de données.</span><span class="sxs-lookup"><span data-stu-id="6a406-144">We now have two tables within our database.</span></span> <span data-ttu-id="6a406-145">Notre dernière étape de création du schéma sera pour configurer une relation « un-à-plusieurs » entre ces deux tables, afin que nous pouvons associer chaque ligne Dinner à zéro ou plusieurs lignes RSVP qui s’y appliquent.</span><span class="sxs-lookup"><span data-stu-id="6a406-145">Our last schema design step will be to setup a "one-to-many" relationship between these two tables – so that we can associate each Dinner row with zero or more RSVP rows that apply to it.</span></span> <span data-ttu-id="6a406-146">Nous le ferons en configurant la colonne « DinnerID » de la table RSVP pour avoir une relation de clé étrangère pour la colonne « DinnerID » dans la table « Préparés ».</span><span class="sxs-lookup"><span data-stu-id="6a406-146">We will do this by configuring the RSVP table's "DinnerID" column to have a foreign-key relationship to the "DinnerID" column in the "Dinners" table.</span></span>

<span data-ttu-id="6a406-147">Pour ce faire, nous allons ouvrir la table RSVP dans le Concepteur de tables en double-cliquant dessus dans l’Explorateur de serveurs.</span><span class="sxs-lookup"><span data-stu-id="6a406-147">To do this we'll open up the RSVP table within the table designer by double-clicking it in the server explorer.</span></span> <span data-ttu-id="6a406-148">Sélectionnez la colonne « DinnerID » qu’il contient, puis avec le bouton droit, puis choisissez le « Relationshps... »</span><span class="sxs-lookup"><span data-stu-id="6a406-148">We'll then select the "DinnerID" column within it, right-click, and choose the "Relationshps…"</span></span> <span data-ttu-id="6a406-149">commande de menu contextuel :</span><span class="sxs-lookup"><span data-stu-id="6a406-149">context menu command:</span></span>

![](create-a-database/_static/image12.png)

<span data-ttu-id="6a406-150">Cela affiche une boîte de dialogue que nous pouvons utiliser pour les relations entre les tables du programme d’installation :</span><span class="sxs-lookup"><span data-stu-id="6a406-150">This will bring up a dialog that we can use to setup relationships between tables:</span></span>

![](create-a-database/_static/image13.png)

<span data-ttu-id="6a406-151">Cliquez sur le bouton « Ajouter » pour ajouter une relation à la boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="6a406-151">We'll click the "Add" button to add a new relationship to the dialog.</span></span> <span data-ttu-id="6a406-152">Une fois qu’une relation a été ajoutée, nous allons développer le nœud d’arborescence « Spécification de Tables et de la colonne » dans la grille des propriétés à droite de la boîte de dialogue et puis cliquez sur le bouton «... » à droite de celui-ci :</span><span class="sxs-lookup"><span data-stu-id="6a406-152">Once a relationship has been added, we'll expand the "Tables and Column Specification" tree-view node within the property grid to the right of the dialog, and then click the "…" button to the right of it:</span></span>

![](create-a-database/_static/image14.png)

<span data-ttu-id="6a406-153">Une autre boîte de dialogue qui permet de spécifier les tables et les colonnes sont impliqués dans la relation, ainsi que nous permettent de nommer la relation en cliquant sur le bouton «... » s’affiche.</span><span class="sxs-lookup"><span data-stu-id="6a406-153">Clicking the "…" button will bring up another dialog that allows us to specify which tables and columns are involved in the relationship, as well as allow us to name the relationship.</span></span>

<span data-ttu-id="6a406-154">Nous modifie la Table de clé primaire pour être « Préparés » et sélectionnez la colonne « DinnerID » dans la table préparés comme clé primaire.</span><span class="sxs-lookup"><span data-stu-id="6a406-154">We will change the Primary Key Table to be "Dinners", and select the "DinnerID" column within the Dinners table as the primary key.</span></span> <span data-ttu-id="6a406-155">Notre table RSVP correspond à la table de clé étrangère et le protocole RSVP. Colonne de DinnerID sera associé en tant que la clé étrangère :</span><span class="sxs-lookup"><span data-stu-id="6a406-155">Our RSVP table will be the foreign-key table, and the RSVP.DinnerID column will be associated as the foreign-key:</span></span>

![](create-a-database/_static/image15.png)

<span data-ttu-id="6a406-156">Chaque ligne dans la table RSVP sont désormais associé à une ligne dans la table de dîner.</span><span class="sxs-lookup"><span data-stu-id="6a406-156">Now each row in the RSVP table will be associated with a row in the Dinner table.</span></span> <span data-ttu-id="6a406-157">SQL Server maintenir l’intégrité référentielle pour nous et nous empêcher d’ajouter une nouvelle ligne RSVP si elle ne pointe pas vers une ligne Dinner valide.</span><span class="sxs-lookup"><span data-stu-id="6a406-157">SQL Server will maintain referential integrity for us – and prevent us from adding a new RSVP row if it does not point to a valid Dinner row.</span></span> <span data-ttu-id="6a406-158">Il nous empêchera également de supprimer une ligne Dinner s’il existe toujours RSVP lignes faisant référence à celle-ci.</span><span class="sxs-lookup"><span data-stu-id="6a406-158">It will also prevent us from deleting a Dinner row if there are still RSVP rows referring to it.</span></span>

### <a name="adding-data-to-our-tables"></a><span data-ttu-id="6a406-159">Ajout de données à nos Tables</span><span class="sxs-lookup"><span data-stu-id="6a406-159">Adding Data to our Tables</span></span>

<span data-ttu-id="6a406-160">Nous allons terminer en ajoutant des exemples de données à notre table préparés.</span><span class="sxs-lookup"><span data-stu-id="6a406-160">Let's finish by adding some sample data to our Dinners table.</span></span> <span data-ttu-id="6a406-161">Nous pouvons ajouter des données à une table en cliquant dessus dans l’Explorateur de serveurs et en choisissant la commande « Afficher les données de Table » :</span><span class="sxs-lookup"><span data-stu-id="6a406-161">We can add data to a table by right-clicking on it within the Server Explorer and choosing the "Show Table Data" command:</span></span>

![](create-a-database/_static/image16.png)

<span data-ttu-id="6a406-162">Nous allons ajouter quelques lignes de données Dinner que nous pouvons utiliser ultérieurement comme nous allons commencer l’implémentation de l’application :</span><span class="sxs-lookup"><span data-stu-id="6a406-162">We'll add a few rows of Dinner data that we can use later as we start implementing the application:</span></span>

![](create-a-database/_static/image17.png)

### <a name="next-step"></a><span data-ttu-id="6a406-163">Étape suivante</span><span class="sxs-lookup"><span data-stu-id="6a406-163">Next Step</span></span>

<span data-ttu-id="6a406-164">Nous avons terminé la création de notre base de données.</span><span class="sxs-lookup"><span data-stu-id="6a406-164">We've finished creating our database.</span></span> <span data-ttu-id="6a406-165">Nous allons maintenant créer les classes de modèle que nous pouvons utiliser pour interroger et mettre à jour.</span><span class="sxs-lookup"><span data-stu-id="6a406-165">Let's now create model classes that we can use to query and update it.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6a406-166">[Précédent](create-a-new-aspnet-mvc-project.md)
> [Suivant](build-a-model-with-business-rule-validations.md)</span><span class="sxs-lookup"><span data-stu-id="6a406-166">[Previous](create-a-new-aspnet-mvc-project.md)
[Next](build-a-model-with-business-rule-validations.md)</span></span>
