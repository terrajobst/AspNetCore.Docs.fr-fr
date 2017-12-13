---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: "Ajout d’un modèle (VB) | Documents Microsoft"
author: Rick-Anderson
description: "Ce didacticiel, vous allez apprendre les principes fondamentaux de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est en cours..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: efc18dd71e29d12dacc6cf84a1d3c7f7e92f520d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-model-vb"></a><span data-ttu-id="bf00c-103">Ajout d’un modèle (VB)</span><span class="sxs-lookup"><span data-stu-id="bf00c-103">Adding a Model (VB)</span></span>
====================
<span data-ttu-id="bf00c-104">Par [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="bf00c-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="bf00c-105">Ce didacticiel, vous allez apprendre les principes fondamentaux de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est une version gratuite de Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bf00c-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="bf00c-106">Avant de commencer, assurez-vous que vous avez installé les composants requis répertoriés ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="bf00c-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="bf00c-107">Vous pouvez installer tous les en cliquant sur le lien suivant : [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="bf00c-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="bf00c-108">Vous pouvez également installer individuellement les conditions préalables à l’aide des liens suivants :</span><span class="sxs-lookup"><span data-stu-id="bf00c-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="bf00c-109">Conditions préalables requises de Visual Studio Web Developer Express SP1</span><span class="sxs-lookup"><span data-stu-id="bf00c-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="bf00c-110">Mettre à jour des outils ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="bf00c-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="bf00c-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + outils prennent en charge)</span><span class="sxs-lookup"><span data-stu-id="bf00c-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="bf00c-112">Si vous utilisez Visual Studio 2010 au lieu de Visual Web Developer 2010, installez les composants requis en cliquant sur le lien suivant : [composants requis de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="bf00c-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="bf00c-113">Un projet de Visual Web Developer avec code VB.NET source est disponible pour accompagner cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="bf00c-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="bf00c-114">[Télécharger la version VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="bf00c-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="bf00c-115">Si vous préférez c#, basculez vers le [c# version](../cs/adding-a-model.md) de ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="bf00c-115">If you prefer C#, switch to the [C# version](../cs/adding-a-model.md) of this tutorial.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="bf00c-116">Ajout d’un modèle</span><span class="sxs-lookup"><span data-stu-id="bf00c-116">Adding a Model</span></span>

<span data-ttu-id="bf00c-117">Dans cette section, vous allez ajouter des classes pour la gestion des films dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="bf00c-117">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="bf00c-118">Ces classes seront la partie « modèle » de l’application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="bf00c-118">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="bf00c-119">Vous utiliserez une technologie d’accès aux données .NET Framework appelée Entity Framework pour définir et utiliser ces classes de modèle.</span><span class="sxs-lookup"><span data-stu-id="bf00c-119">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="bf00c-120">Le prend en charge Entity Framework (souvent appelés EF) un paradigme de développement appelé *Code First*.</span><span class="sxs-lookup"><span data-stu-id="bf00c-120">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="bf00c-121">Code permet tout d’abord vous permet de créer des objets de modèle en écrivant des classes simples.</span><span class="sxs-lookup"><span data-stu-id="bf00c-121">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="bf00c-122">(Ceux-ci sont également appelés classes POCO, à partir de « plain-old CLR les objets. ») Vous pouvez ensuite configurer la base de données créé à la volée à partir de vos classes, ce qui permet un flux de travail de développement très propre et plus rapide.</span><span class="sxs-lookup"><span data-stu-id="bf00c-122">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="bf00c-123">Ajouter des Classes de modèle</span><span class="sxs-lookup"><span data-stu-id="bf00c-123">Adding Model Classes</span></span>

<span data-ttu-id="bf00c-124">Dans **l’Explorateur de solutions**, bouton droit sur le *modèles* dossier, sélectionnez **ajouter**, puis sélectionnez **classe**.</span><span class="sxs-lookup"><span data-stu-id="bf00c-124">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="bf00c-125">Nom de la classe « Film ».</span><span class="sxs-lookup"><span data-stu-id="bf00c-125">Name the class "Movie".</span></span>

<span data-ttu-id="bf00c-126">Ajoutez les propriétés suivantes de cinq à la `Movie` classe :</span><span class="sxs-lookup"><span data-stu-id="bf00c-126">Add the following five properties to the `Movie` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

<span data-ttu-id="bf00c-127">Nous allons utiliser la `Movie` classe pour représenter des films dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="bf00c-127">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="bf00c-128">Chaque instance d’un `Movie` objet correspond à une ligne dans une table de base de données et chaque propriété de la `Movie` classe est mappé à une colonne dans la table.</span><span class="sxs-lookup"><span data-stu-id="bf00c-128">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="bf00c-129">Dans le même fichier, ajoutez le code suivant `MovieDBContext` classe :</span><span class="sxs-lookup"><span data-stu-id="bf00c-129">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

<span data-ttu-id="bf00c-130">Le `MovieDBContext` classe représente le contexte de base de données de film Entity Framework, qui gère l’extraction, de stockage et de mise à jour `Movie` instances dans une base de données de la classe.</span><span class="sxs-lookup"><span data-stu-id="bf00c-130">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="bf00c-131">Le `MovieDBContext` dérive le `DbContext` fourni par Entity Framework de classe de base.</span><span class="sxs-lookup"><span data-stu-id="bf00c-131">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="bf00c-132">Pour plus d’informations sur `DbContext` et `DbSet`, consultez [améliorations de productivité pour Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="bf00c-132">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="bf00c-133">Afin de pouvoir faire référence à `DbContext` et `DbSet`, vous devez ajouter celle-ci `imports` instruction en haut du fichier :</span><span class="sxs-lookup"><span data-stu-id="bf00c-133">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `imports` statement at the top of the file:</span></span>

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

<span data-ttu-id="bf00c-134">Le texte complet *Movie.vb* fichier est présenté ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="bf00c-134">The complete *Movie.vb* file is shown below.</span></span>

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="bf00c-135">Création d’une chaîne de connexion et l’utilisation de SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="bf00c-135">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="bf00c-136">Le `MovieDBContext` classe créée gère la tâche de connexion à la base de données et de mappage `Movie` objets aux enregistrements de base de données.</span><span class="sxs-lookup"><span data-stu-id="bf00c-136">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="bf00c-137">Une question que vous vous demandez peut-être, cependant, est comment spécifier une base de données auquel il se connecte à.</span><span class="sxs-lookup"><span data-stu-id="bf00c-137">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="bf00c-138">Vous effectuerez qui en ajoutant des informations de connexion dans le *Web.config* fichier de l’application.</span><span class="sxs-lookup"><span data-stu-id="bf00c-138">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="bf00c-139">Ouvrez la racine de l’application *Web.config* fichier.</span><span class="sxs-lookup"><span data-stu-id="bf00c-139">Open the application root *Web.config* file.</span></span> <span data-ttu-id="bf00c-140">(Pas le *Web.config* de fichiers dans le *vues* dossier.) L’image ci-dessous montrent deux *Web.config* fichiers ; ouvrir les *Web.config* fichier entourée en rouge.</span><span class="sxs-lookup"><span data-stu-id="bf00c-140">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image2.png)

## 

<span data-ttu-id="bf00c-141">Ajouter la chaîne de connexion à la `<connectionStrings>` élément dans le *Web.config* fichier.</span><span class="sxs-lookup"><span data-stu-id="bf00c-141">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="bf00c-142">L’exemple suivant montre une partie de la *Web.config* fichier avec la nouvelle chaîne de connexion ajoutée :</span><span class="sxs-lookup"><span data-stu-id="bf00c-142">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="bf00c-143">Cette petite quantité de code et le XML est tout ce que vous devez écrire afin de représenter et de stocker les données de film dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="bf00c-143">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="bf00c-144">Ensuite, vous allez générer un nouveau `MoviesController` classe que vous pouvez utiliser pour afficher les données de film et permettre aux utilisateurs de créer de nouvelles listes de film.</span><span class="sxs-lookup"><span data-stu-id="bf00c-144">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="bf00c-145">[Précédent](adding-a-view.md)
[Suivant](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="bf00c-145">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
