---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Ajout d’un modèle | Microsoft Docs
author: Rick-Anderson
description: 'Remarque : Une version mise à jour de ce didacticiel est disponible ici qui utilise ASP.NET MVC 5 et Visual Studio 2013. Il est plus sécurisé, beaucoup plus simple à suivre et de démonstration...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 5308d2d1d11f954db8a4502adb42223f69e0c675
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578501"
---
<a name="adding-a-model"></a><span data-ttu-id="22283-104">Ajout d’un modèle</span><span class="sxs-lookup"><span data-stu-id="22283-104">Adding a Model</span></span>
====================
<span data-ttu-id="22283-105">par [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="22283-105">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> > [!NOTE]
> > <span data-ttu-id="22283-106">Une version mise à jour de ce didacticiel est disponible [ici](../../getting-started/introduction/getting-started.md) qui utilise ASP.NET MVC 5 et Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="22283-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="22283-107">Il est plus sécurisé, beaucoup plus simple à suivre et illustre plusieurs fonctionnalités.</span><span class="sxs-lookup"><span data-stu-id="22283-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="22283-108">Dans cette section, vous allez ajouter des classes pour la gestion des films dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="22283-108">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="22283-109">Ces classes seront les &quot;modèle&quot; dans le cadre de l’application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="22283-109">These classes will be the &quot;model&quot; part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="22283-110">Vous allez utiliser une technologie d’accès aux données .NET Framework appelée le [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) à définir et utiliser ces classes de modèle.</span><span class="sxs-lookup"><span data-stu-id="22283-110">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) to define and work with these model classes.</span></span> <span data-ttu-id="22283-111">Le prend en charge Entity Framework (souvent appelée EF) un paradigme de développement appelé *Code First*.</span><span class="sxs-lookup"><span data-stu-id="22283-111">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="22283-112">Code tout d’abord vous permet de créer des objets de modèle en écrivant des classes simples.</span><span class="sxs-lookup"><span data-stu-id="22283-112">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="22283-113">(Ils sont également appelés classes POCO, à partir de &quot;objet CLR traditionnel.&quot;) Vous avez ensuite la base de données créé à la volée à partir de vos classes, ce qui permet un flux de travail de développement très concis et rapide.</span><span class="sxs-lookup"><span data-stu-id="22283-113">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="22283-114">Ajout de Classes de modèle</span><span class="sxs-lookup"><span data-stu-id="22283-114">Adding Model Classes</span></span>

<span data-ttu-id="22283-115">Dans **l’Explorateur de solutions**, avec le bouton droit cliquez sur le *modèles* dossier, sélectionnez **ajouter**, puis sélectionnez **classe**.</span><span class="sxs-lookup"><span data-stu-id="22283-115">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="22283-116">Entrez le *classe* nom &quot;film&quot;.</span><span class="sxs-lookup"><span data-stu-id="22283-116">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="22283-117">Ajoutez les propriétés suivantes de cinq à la `Movie` classe :</span><span class="sxs-lookup"><span data-stu-id="22283-117">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="22283-118">Nous allons utiliser le `Movie` classe pour représenter des films dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="22283-118">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="22283-119">Chaque instance d’un `Movie` objet correspond à une ligne d’une table de base de données et chaque propriété de la `Movie` classe est mappé à une colonne dans la table.</span><span class="sxs-lookup"><span data-stu-id="22283-119">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="22283-120">Dans le même fichier, ajoutez le code suivant `MovieDBContext` classe :</span><span class="sxs-lookup"><span data-stu-id="22283-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="22283-121">Le `MovieDBContext` classe représente le contexte de base de données de film de Entity Framework, qui gère l’extraction, le stockage et la mise à jour `Movie` instances dans une base de données de la classe.</span><span class="sxs-lookup"><span data-stu-id="22283-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="22283-122">Le `MovieDBContext` dérive le `DbContext` fourni par Entity Framework de classe de base.</span><span class="sxs-lookup"><span data-stu-id="22283-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="22283-123">Afin de pouvoir faire référence à `DbContext` et `DbSet`, vous devez ajouter le code suivant `using` instruction en haut du fichier :</span><span class="sxs-lookup"><span data-stu-id="22283-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="22283-124">L’ensemble *Movie.cs* fichier est présenté ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="22283-124">The complete *Movie.cs* file is shown below.</span></span> <span data-ttu-id="22283-125">(Plusieurs instructions using qui ne sont pas nécessaires ont été supprimé.)</span><span class="sxs-lookup"><span data-stu-id="22283-125">(Several using statements that are not needed have been removed.)</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="22283-126">Création d’une chaîne de connexion et l’utilisation de SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="22283-126">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="22283-127">Le `MovieDBContext` classe que vous avez créé gère la tâche de connexion à la base de données et de mappage `Movie` objets aux enregistrements de base de données.</span><span class="sxs-lookup"><span data-stu-id="22283-127">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="22283-128">Une question que vous vous demandez peut-être, cependant, consiste à spécifier quelle base de données, il se connecte à.</span><span class="sxs-lookup"><span data-stu-id="22283-128">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="22283-129">Vous le ferez en ajoutant des informations de connexion dans le *Web.config* fichier de l’application.</span><span class="sxs-lookup"><span data-stu-id="22283-129">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="22283-130">Ouvrez la racine de l’application *Web.config* fichier.</span><span class="sxs-lookup"><span data-stu-id="22283-130">Open the application root *Web.config* file.</span></span> <span data-ttu-id="22283-131">(Pas le *Web.config* de fichiers dans le *vues* dossier.) Ouvrez le *Web.config* fichier surlignée en rouge.</span><span class="sxs-lookup"><span data-stu-id="22283-131">(Not the *Web.config* file in the *Views* folder.) Open the *Web.config* file outlined in red.</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="22283-132">Ajoutez la chaîne de connexion suivante à la `<connectionStrings>` élément dans le *Web.config* fichier.</span><span class="sxs-lookup"><span data-stu-id="22283-132">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="22283-133">L’exemple suivant montre une partie de la *Web.config* fichier avec la nouvelle chaîne de connexion ajoutée :</span><span class="sxs-lookup"><span data-stu-id="22283-133">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

<span data-ttu-id="22283-134">Cette petite quantité de code et le XML est tout ce que vous devez écrire pour représenter et de stocker les données de films dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="22283-134">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="22283-135">Ensuite, vous allez générer une nouvelle `MoviesController` classe que vous pouvez utiliser pour afficher les données de film et permettre aux utilisateurs de créer de nouvelles listes de film.</span><span class="sxs-lookup"><span data-stu-id="22283-135">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="22283-136">[Précédent](adding-a-view.md)
> [Suivant](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="22283-136">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
