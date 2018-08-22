---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: Introduction au didacticiel NerdDinner | Microsoft Docs
author: shanselman
description: La meilleure façon d’apprendre une nouvelle infrastructure consiste à créer quelque chose avec lui. Ce didacticiel vous montre comment créer une application légère, mais complète, à l’aide de ASP.NE...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: d5efab525841b5c526aa3b656f27b1c42cc74648
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825336"
---
<a name="introducing-the-nerddinner-tutorial"></a><span data-ttu-id="8cb88-104">Introduction au didacticiel NerdDinner</span><span class="sxs-lookup"><span data-stu-id="8cb88-104">Introducing the NerdDinner Tutorial</span></span>
====================
<span data-ttu-id="8cb88-105">par [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="8cb88-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

[<span data-ttu-id="8cb88-106">Télécharger PDF</span><span class="sxs-lookup"><span data-stu-id="8cb88-106">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="8cb88-107">La meilleure façon d’apprendre une nouvelle infrastructure consiste à créer quelque chose avec lui.</span><span class="sxs-lookup"><span data-stu-id="8cb88-107">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="8cb88-108">Ce didacticiel vous montre comment créer un petit mais terminé, l’application à l’aide d’ASP.NET MVC 1 et présente certains des principaux concepts derrière lui.</span><span class="sxs-lookup"><span data-stu-id="8cb88-108">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC 1, and introduces some of the core concepts behind it.</span></span>
> 
> <span data-ttu-id="8cb88-109">Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre le [mise en route avec MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [Store de musique MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) didacticiels.</span><span class="sxs-lookup"><span data-stu-id="8cb88-109">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-tutorial"></a><span data-ttu-id="8cb88-110">Didacticiel NerdDinner</span><span class="sxs-lookup"><span data-stu-id="8cb88-110">NerdDinner Tutorial</span></span>

<span data-ttu-id="8cb88-111">La meilleure façon d’apprendre une nouvelle infrastructure consiste à créer quelque chose avec lui.</span><span class="sxs-lookup"><span data-stu-id="8cb88-111">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="8cb88-112">Ce didacticiel vous montre comment créer un petit mais terminé, l’application à l’aide d’ASP.NET MVC et présente certains des principaux concepts derrière lui.</span><span class="sxs-lookup"><span data-stu-id="8cb88-112">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC, and introduces some of the core concepts behind it.</span></span>

<span data-ttu-id="8cb88-113">L’application que nous allons générer est appelée « NerdDinner ».</span><span class="sxs-lookup"><span data-stu-id="8cb88-113">The application we are going to build is called "NerdDinner".</span></span> <span data-ttu-id="8cb88-114">NerdDinner offre un moyen facile pour les personnes rechercher et organiser dîners en ligne :</span><span class="sxs-lookup"><span data-stu-id="8cb88-114">NerdDinner provides an easy way for people to find and organize dinners online:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image1.png)

<span data-ttu-id="8cb88-115">NerdDinner permet à des utilisateurs inscrits créer, modifier et supprimer dîners.</span><span class="sxs-lookup"><span data-stu-id="8cb88-115">NerdDinner enables registered users to create, edit and delete dinners.</span></span> <span data-ttu-id="8cb88-116">Il applique un ensemble cohérent de validation et les règles métier dans toute l’application :</span><span class="sxs-lookup"><span data-stu-id="8cb88-116">It enforces a consistent set of validation and business rules across the application:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image2.png)

<span data-ttu-id="8cb88-117">Visiteurs peuvent utiliser une carte basée sur AJAX pour rechercher des prochains dîners qui se trouve près d’eux :</span><span class="sxs-lookup"><span data-stu-id="8cb88-117">Visitors can use an AJAX-based map to search for upcoming dinners being held near them:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image3.png)

<span data-ttu-id="8cb88-118">Un clic sur un dîner mène à une page de détails où ils peuvent en savoir plus à ce sujet :</span><span class="sxs-lookup"><span data-stu-id="8cb88-118">Clicking a dinner will take them to a details page where they can learn more about it:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image4.png)

<span data-ttu-id="8cb88-119">Si elles sont intéressé par le dîner qu’ils puissent se connecter ou s’inscrire sur le site :</span><span class="sxs-lookup"><span data-stu-id="8cb88-119">If they are interested in attending the dinner they can login or register on the site:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image5.png)

<span data-ttu-id="8cb88-120">Ils peuvent puis cliquez sur un lien RSVP basée sur AJAX pour assister à l’événement :</span><span class="sxs-lookup"><span data-stu-id="8cb88-120">They can then click an AJAX-based RSVP link to attend the event:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a><span data-ttu-id="8cb88-121">Implémentation de NerdDinner</span><span class="sxs-lookup"><span data-stu-id="8cb88-121">Implementing NerdDinner</span></span>

<span data-ttu-id="8cb88-122">Nous allons commencer notre application NerdDinner en utilisant le fichier -&gt;commande Nouveau projet dans Visual Studio pour créer un tout nouveau projet ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="8cb88-122">We are going to begin our NerdDinner application by using the File-&gt;New Project command within Visual Studio to create a brand new ASP.NET MVC project.</span></span> <span data-ttu-id="8cb88-123">Nous allons ajouter puis par incréments de fonctionnalités.</span><span class="sxs-lookup"><span data-stu-id="8cb88-123">We will then incrementally add functionality and features.</span></span> <span data-ttu-id="8cb88-124">Tout au long du processus, nous aborderons :</span><span class="sxs-lookup"><span data-stu-id="8cb88-124">Along the way we'll cover:</span></span>

1. [<span data-ttu-id="8cb88-125">Comment créer un nouveau projet ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="8cb88-125">How to create a new ASP.NET MVC Project</span></span>](# "créer un nouveau projet ASP.NET MVC")
2. [<span data-ttu-id="8cb88-126">Comment créer une base de données</span><span class="sxs-lookup"><span data-stu-id="8cb88-126">How to create a database</span></span>](# "créer une base de données")
3. [<span data-ttu-id="8cb88-127">Comment créer un modèle avec des validations de règles métier</span><span class="sxs-lookup"><span data-stu-id="8cb88-127">How to build a model with business rule validations</span></span>](# "créer un modèle avec des Validations de règles d’entreprise")
4. [<span data-ttu-id="8cb88-128">Comment utiliser des contrôleurs et des vues pour implémenter une interface utilisateur liste/détails</span><span class="sxs-lookup"><span data-stu-id="8cb88-128">How to use controllers and views to implement a listing/details UI</span></span>](# "utiliser les contrôleurs et les vues pour implémenter une interface utilisateur liste/détails")
5. <span data-ttu-id="8cb88-129">[Comment fournir CRUD (créer, lire, mettre à jour, supprimer) les formulaires de données prise en charge de l’entrée](# "fournir CRUD (Create, Read, Update, Delete) données formulaire entrée prend en charge")</span><span class="sxs-lookup"><span data-stu-id="8cb88-129">[How to provide CRUD (create, read, update, delete) data form entry support](# "Provide CRUD (Create, Read, Update, Delete) Data Form Entry Support")</span></span>
6. [<span data-ttu-id="8cb88-130">Comment utiliser ViewData et implémenter des classes ViewModel</span><span class="sxs-lookup"><span data-stu-id="8cb88-130">How to use ViewData and implement ViewModel classes</span></span>](# "utiliser un ViewData et implémenter des Classes ViewModel")
7. [<span data-ttu-id="8cb88-131">Comment réutiliser l’interface utilisateur à l’aide de pages maîtres et des vues partielles</span><span class="sxs-lookup"><span data-stu-id="8cb88-131">How to re-use UI using master pages and partials</span></span>](# "réutiliser d’interface utilisateur à l’aide des Pages maîtres et des vues partielles")
8. [<span data-ttu-id="8cb88-132">Comment implémenter la pagination des données efficace</span><span class="sxs-lookup"><span data-stu-id="8cb88-132">How to implement efficient data paging</span></span>](# "implémenter de données efficace la pagination")
9. [<span data-ttu-id="8cb88-133">Comment sécuriser des applications à l’aide de l’authentification et l’autorisation</span><span class="sxs-lookup"><span data-stu-id="8cb88-133">How to secure applications using authentication and authorization</span></span>](# "sécurisé Applications à l’aide de l’authentification et autorisation")
10. [<span data-ttu-id="8cb88-134">Comment utiliser AJAX pour fournir des mises à jour dynamiques</span><span class="sxs-lookup"><span data-stu-id="8cb88-134">How to use AJAX to deliver dynamic updates</span></span>](# "utiliser AJAX pour fournir des mises à jour dynamiques")
11. [<span data-ttu-id="8cb88-135">Comment utiliser AJAX pour implémenter des scénarios de mappage</span><span class="sxs-lookup"><span data-stu-id="8cb88-135">How to use AJAX to implement mapping scenarios</span></span>](# "utiliser AJAX pour implémenter les scénarios de mappage")
12. [<span data-ttu-id="8cb88-136">Comment activer les tests d’unités automatisés</span><span class="sxs-lookup"><span data-stu-id="8cb88-136">How to enable automated unit testing</span></span>](# "activer le test unitaire automatisé")

<span data-ttu-id="8cb88-137">Vous pouvez créer votre propre copie de NerdDinner à partir de zéro à la fin de chaque étape nous procédure pas à pas dans ce chapitre.</span><span class="sxs-lookup"><span data-stu-id="8cb88-137">You can build your own copy of NerdDinner from scratch by completing each step we walkthrough in this chapter.</span></span> <span data-ttu-id="8cb88-138">Vous pouvez également télécharger une version complète du code source ici : [NerdDinner sur GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span><span class="sxs-lookup"><span data-stu-id="8cb88-138">Alternatively, you can download a completed version of the source code here: [NerdDinner on GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span></span> <span data-ttu-id="8cb88-139">Vous pouvez également éventuellement également [télécharger une version PDF de ce didacticiel](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) si vous souhaitez lire le didacticiel en mode hors connexion.</span><span class="sxs-lookup"><span data-stu-id="8cb88-139">You can also optionally also [download a free PDF version of this tutorial](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) if you want to read the tutorial offline.</span></span>

<span data-ttu-id="8cb88-140">Vous pouvez utiliser Visual Studio 2008 ou sur le gratuit Visual Web Developer 2008 Express pour générer l’application.</span><span class="sxs-lookup"><span data-stu-id="8cb88-140">You can use either Visual Studio 2008 or the free Visual Web Developer 2008 Express to build the application.</span></span> <span data-ttu-id="8cb88-141">Vous pouvez utiliser SQL Server ou la version gratuite SQL Server Express pour la base de données.</span><span class="sxs-lookup"><span data-stu-id="8cb88-141">You can use either SQL Server or the free SQL Server Express for the database.</span></span>

<span data-ttu-id="8cb88-142">Vous pouvez installer ASP.NET MVC, Visual Web Developer 2008 Express et SQL Server Express (gratuit) ce à l’aide de la version 2 de la [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)</span><span class="sxs-lookup"><span data-stu-id="8cb88-142">You can install ASP.NET MVC, Visual Web Developer 2008 Express, and SQL Server Express (all free) using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)</span></span>

### <a name="now-lets-get-started"></a><span data-ttu-id="8cb88-143">Maintenant nous allons commencer...</span><span class="sxs-lookup"><span data-stu-id="8cb88-143">Now let's get started....</span></span>

<span data-ttu-id="8cb88-144">Maintenant que nous avons couvert NerdDinner What ' s, nous allons notre manches et écrire du code.</span><span class="sxs-lookup"><span data-stu-id="8cb88-144">Now that we've covered what NerdDinner is, let's roll up our sleeves and write some code.</span></span>

<span data-ttu-id="8cb88-145">Nous allons commencer à l’aide de fichier -&gt;nouveau projet dans Visual Studio pour créer l’application NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="8cb88-145">We'll begin by using File-&gt;New Project within Visual Studio to create the NerdDinner application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="8cb88-146">Next</span><span class="sxs-lookup"><span data-stu-id="8cb88-146">Next</span></span>](create-a-new-aspnet-mvc-project.md)
