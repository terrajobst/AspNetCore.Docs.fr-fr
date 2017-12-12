---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: "Ajout d’un modèle | Documents Microsoft"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 13aab58e86829a8d4accd1d304420dcb34ffa472
ms.sourcegitcommit: ec9371e2fbfcb8d62e7e7cae69e7752f3f205385
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/23/2017
---
<a name="adding-a-model"></a><span data-ttu-id="8247c-102">Ajout d’un modèle</span><span class="sxs-lookup"><span data-stu-id="8247c-102">Adding a Model</span></span>
====================
<span data-ttu-id="8247c-103">Par [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="8247c-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE[Tutorial Note](sample/code-location.md)]

<span data-ttu-id="8247c-104">Dans cette section, vous allez ajouter des classes pour la gestion des films dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="8247c-104">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="8247c-105">Ces classes seront les &quot;modèle&quot; dans le cadre de l’application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="8247c-105">These classes will be the &quot;model&quot; part of the ASP.NET MVC app.</span></span>

<span data-ttu-id="8247c-106">Vous allez utiliser une technologie d’accès aux données .NET Framework appelée le [Entity Framework](https://docs.microsoft.com/ef/) pour définir et utiliser ces classes de modèle.</span><span class="sxs-lookup"><span data-stu-id="8247c-106">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://docs.microsoft.com/ef/) to define and work with these model classes.</span></span> <span data-ttu-id="8247c-107">Le prend en charge Entity Framework (souvent appelés EF) un paradigme de développement appelé *Code First*.</span><span class="sxs-lookup"><span data-stu-id="8247c-107">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="8247c-108">Code permet tout d’abord vous permet de créer des objets de modèle en écrivant des classes simples.</span><span class="sxs-lookup"><span data-stu-id="8247c-108">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="8247c-109">(Ils sont également appelés classes POCO, à partir de &quot;objets de brut-old CLR.&quot;) Vous pouvez ensuite configurer la base de données créé à la volée à partir de vos classes, ce qui permet un flux de travail de développement très propre et plus rapide.</span><span class="sxs-lookup"><span data-stu-id="8247c-109">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span> <span data-ttu-id="8247c-110">Si vous devez d’abord créer la base de données, vous pouvez toujours suivre ce didacticiel pour en savoir plus sur le développement d’applications MVC et EF.</span><span class="sxs-lookup"><span data-stu-id="8247c-110">If you are required to create the database first, you can still follow this tutorial to learn about MVC and EF app development.</span></span> <span data-ttu-id="8247c-111">Vous pouvez alors suivre Tom Fizmakens [génération de modèles automatique ASP.NET](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) (didacticiel), qui couvre la première approche de base de données.</span><span class="sxs-lookup"><span data-stu-id="8247c-111">You can then follow Tom Fizmakens [ASP.NET Scaffolding](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) tutorial, which covers the database first approach.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="8247c-112">Ajouter des Classes de modèle</span><span class="sxs-lookup"><span data-stu-id="8247c-112">Adding Model Classes</span></span>

<span data-ttu-id="8247c-113">Dans **l’Explorateur de solutions**, bouton droit sur le *modèles* dossier, sélectionnez **ajouter**, puis sélectionnez **classe**.</span><span class="sxs-lookup"><span data-stu-id="8247c-113">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="8247c-114">Entrez le *classe* nom &quot;film&quot;.</span><span class="sxs-lookup"><span data-stu-id="8247c-114">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="8247c-115">Ajoutez les propriétés suivantes de cinq à la `Movie` classe :</span><span class="sxs-lookup"><span data-stu-id="8247c-115">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="8247c-116">Nous allons utiliser la `Movie` classe pour représenter des films dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="8247c-116">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="8247c-117">Chaque instance d’un `Movie` objet correspond à une ligne dans une table de base de données et chaque propriété de la `Movie` classe est mappé à une colonne dans la table.</span><span class="sxs-lookup"><span data-stu-id="8247c-117">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="8247c-118">Remarque : Pour pouvoir utiliser System.Data.Entity et la classe associée, vous devez installer le [Entity Framework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="8247c-118">Note: In order to use System.Data.Entity, and the related class, you need to install the [Entity Framework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="8247c-119">Suivez le lien pour obtenir des instructions.</span><span class="sxs-lookup"><span data-stu-id="8247c-119">Follow the link for further instructions.</span></span>

<span data-ttu-id="8247c-120">Dans le même fichier, ajoutez le code suivant `MovieDBContext` classe :</span><span class="sxs-lookup"><span data-stu-id="8247c-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

<span data-ttu-id="8247c-121">Le `MovieDBContext` classe représente le contexte de base de données de film Entity Framework, qui gère l’extraction, de stockage et de mise à jour `Movie` instances dans une base de données de la classe.</span><span class="sxs-lookup"><span data-stu-id="8247c-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="8247c-122">Le `MovieDBContext` dérive le `DbContext` fourni par Entity Framework de classe de base.</span><span class="sxs-lookup"><span data-stu-id="8247c-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="8247c-123">Afin de pouvoir faire référence à `DbContext` et `DbSet`, vous devez ajouter celle-ci `using` instruction en haut du fichier :</span><span class="sxs-lookup"><span data-stu-id="8247c-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="8247c-124">Ce faire, vous pouvez ajouter manuellement à l’aide de l’instruction, ou vous pouvez survoler les traits rouges ondulés, cliquez sur `Show potential fixes` et cliquez sur`using System.Data.Entity;`</span><span class="sxs-lookup"><span data-stu-id="8247c-124">You can do this by manually adding the using statement, or you can hover over the red squiggly lines, click `Show potential fixes` and click `using System.Data.Entity;`</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="8247c-125">Remarque : Plusieurs inutilisé `using` instructions ont été supprimées.</span><span class="sxs-lookup"><span data-stu-id="8247c-125">Note: Several unused `using` statements have been removed.</span></span> <span data-ttu-id="8247c-126">Visual Studio affiche les dépendances non utilisées en gris.</span><span class="sxs-lookup"><span data-stu-id="8247c-126">Visual Studio will show unused dependencies as gray.</span></span> <span data-ttu-id="8247c-127">Vous pouvez supprimer les dépendances d’unnused en pointant sur les dépendances en gris, cliquez sur `Show potential fixes` et cliquez sur **supprimer les instructions Using obsolètes.**</span><span class="sxs-lookup"><span data-stu-id="8247c-127">You can remove unnused dependencies by hovering over the gray dependencies, click `Show potential fixes` and click **Remove Unused Usings.**</span></span>

![](adding-a-model/_static/image3.png)

<span data-ttu-id="8247c-128">Nous avons ajouté enfin d’un modèle (le M dans MVC).</span><span class="sxs-lookup"><span data-stu-id="8247c-128">We've finally added a model (the M in MVC).</span></span> <span data-ttu-id="8247c-129">Dans la section suivante, vous allez travailler avec la chaîne de connexion de base de données.</span><span class="sxs-lookup"><span data-stu-id="8247c-129">In the next section you'll work with the database connection string.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="8247c-130">[Précédent](adding-a-view.md)
[Suivant](creating-a-connection-string.md)</span><span class="sxs-lookup"><span data-stu-id="8247c-130">[Previous](adding-a-view.md)
[Next](creating-a-connection-string.md)</span></span>
