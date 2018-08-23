---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: La mise à niveau une Application ASP.NET MVC 1.0 vers ASP.NET MVC 2 | Microsoft Docs
author: rick-anderson
description: Ce document décrit à la fois la mise à niveau avec un Assistant et manuellement une Application ASP.NET MVC 1.0 vers ASP.NET MVC 2. Ce document est également disponible pour d...
ms.author: riande
ms.date: 04/08/2010
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: 3de69df7e80037de35c2609232f4574bc9d03c80
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836436"
---
<a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a><span data-ttu-id="5475b-104">La mise à niveau une Application ASP.NET MVC 1.0 vers ASP.NET MVC 2</span><span class="sxs-lookup"><span data-stu-id="5475b-104">Upgrading an ASP.NET MVC 1.0 Application to ASP.NET MVC 2</span></span>
====================
> <span data-ttu-id="5475b-105">Ce document décrit à la fois la mise à niveau avec un Assistant et manuellement une Application ASP.NET MVC 1.0 vers ASP.NET MVC 2.</span><span class="sxs-lookup"><span data-stu-id="5475b-105">This document describes both how to upgrade manually and with a wizard an ASP.NET MVC 1.0 Application to ASP.NET MVC 2.</span></span> <span data-ttu-id="5475b-106">Ce document est également disponible pour [télécharger](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)</span><span class="sxs-lookup"><span data-stu-id="5475b-106">This document is also available for [Download](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)</span></span>


## <a name="introduction"></a><span data-ttu-id="5475b-107">Introduction</span><span class="sxs-lookup"><span data-stu-id="5475b-107">Introduction</span></span>

<span data-ttu-id="5475b-108">ASP.NET MVC 2 peut être installé côte à côte avec ASP.NET MVC 1.0 sur le même serveur.</span><span class="sxs-lookup"><span data-stu-id="5475b-108">ASP.NET MVC 2 can be installed side by side with ASP.NET MVC 1.0 on the same server.</span></span> <span data-ttu-id="5475b-109">Flexibilité application aux développeurs de choisir quand mettre à niveau une application ASP.NET MVC 1.0 vers ASP.NET MVC 2.</span><span class="sxs-lookup"><span data-stu-id="5475b-109">This gives application developers flexibility in choosing when to upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2.</span></span>

<span data-ttu-id="5475b-110">Visual Studio 2010 inclut un Assistant que les mises à niveau des projets ASP.NET MVC 1.0 existants générés avec Visual Studio 2008 pour ASP.NET MVC 2.</span><span class="sxs-lookup"><span data-stu-id="5475b-110">Visual Studio 2010 includes a wizard that upgrades existing ASP.NET MVC 1.0 projects built with Visual Studio 2008 to ASP.NET MVC 2.</span></span> <span data-ttu-id="5475b-111">L’Assistant Mise à niveau est lancé en ouvrant un projet ASP.NET MVC 1.0 dans Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="5475b-111">The upgrade wizard is initiated by opening an ASP.NET MVC 1.0 project in Visual Studio 2010.</span></span>

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a><span data-ttu-id="5475b-112">Mise à niveau de l’Assistant pour ASP.NET MVC 1.0 sur Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="5475b-112">Upgrade Wizard for ASP.NET MVC 1.0 on Visual Studio 2008 SP1</span></span>

<span data-ttu-id="5475b-113">Pour mettre à niveau une application ASP.NET MVC 1.0 vers ASP.NET MVC 2 dans Visual Studio 2008 SP1, utilisez l’application MvcAppConverter (non pris en charge).</span><span class="sxs-lookup"><span data-stu-id="5475b-113">To upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2 in Visual Studio 2008 SP1, use the (unsupported) MvcAppConverter application.</span></span> <span data-ttu-id="5475b-114">Vous pouvez télécharger cette application à partir de l’URL suivante :</span><span class="sxs-lookup"><span data-stu-id="5475b-114">You can download this application from the following URL:</span></span>

[https://go.microsoft.com/fwlink/?LinkID=185351](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a><span data-ttu-id="5475b-115">Mise à niveau manuelle d’un projet ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="5475b-115">Manually Upgrading an ASP.NET MVC 1.0 Project</span></span>

<span data-ttu-id="5475b-116">Pour mettre manuellement à une application ASP.NET MVC 1.0 vers la version 2, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="5475b-116">To manually upgrade an existing ASP.NET MVC 1.0 application to version 2, follow these steps:</span></span>

1. <span data-ttu-id="5475b-117">Effectuez une sauvegarde du projet existant.</span><span class="sxs-lookup"><span data-stu-id="5475b-117">Make a backup of the existing project.</span></span>
2. <span data-ttu-id="5475b-118">Dans un éditeur de texte, ouvrez le fichier de projet (le fichier avec l’extension de fichier .csproj ou .vbproj) et recherchez l’élément ProjectTypeGuid.</span><span class="sxs-lookup"><span data-stu-id="5475b-118">In a text editor, open the project file (the file with the .csproj or .vbproj file extension) and find the ProjectTypeGuid element.</span></span> <span data-ttu-id="5475b-119">En tant que la valeur de cet élément, remplacez le GUID {603c0e0b-db56-11dc-be95-000d561079b0} avec {F85E285D-A4E0-4152-9332-AB1D724D3325}.</span><span class="sxs-lookup"><span data-stu-id="5475b-119">As the value of that element, replace the GUID {603c0e0b-db56-11dc-be95-000d561079b0} with {F85E285D-A4E0-4152-9332-AB1D724D3325}.</span></span> <span data-ttu-id="5475b-120">Lorsque vous avez terminé, il se peut que la valeur de cet élément doit être comme suit :</span><span class="sxs-lookup"><span data-stu-id="5475b-120">When you are done, the value of that element should be as follows:</span></span> 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. <span data-ttu-id="5475b-121">Dans le dossier racine d’application Web, modifiez le fichier Web.config.</span><span class="sxs-lookup"><span data-stu-id="5475b-121">In the Web application root folder, edit the Web.config file.</span></span> <span data-ttu-id="5475b-122">Recherchez System.Web.Mvc, Version = 1.0.0.0 et remplacez toutes les instances avec System.Web.Mvc, Version = 2.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="5475b-122">Search for System.Web.Mvc, Version=1.0.0.0 and replace all instances with System.Web.Mvc, Version=2.0.0.0.</span></span>
4. <span data-ttu-id="5475b-123">Répétez l’étape précédente pour le fichier Web.config situé dans le dossier Views.</span><span class="sxs-lookup"><span data-stu-id="5475b-123">Repeat the previous step for the Web.config file located in the Views folder.</span></span>
5. <span data-ttu-id="5475b-124">Ouvrez le projet à l’aide de Visual Studio, puis, dans **l’Explorateur de solutions**, développez le **références** nœud.</span><span class="sxs-lookup"><span data-stu-id="5475b-124">Open the project using Visual Studio, and in **Solution Explorer**, expand the **References** node.</span></span> <span data-ttu-id="5475b-125">Supprimez la référence à System.Web.Mvc (qui pointe vers l’assembly de la version 1.0).</span><span class="sxs-lookup"><span data-stu-id="5475b-125">Delete the reference to System.Web.Mvc (which points to the version 1.0 assembly).</span></span> <span data-ttu-id="5475b-126">Ajoutez une référence à System.Web.Mvc (v2.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="5475b-126">Add a reference to System.Web.Mvc (v2.0.0.0).</span></span>
6. <span data-ttu-id="5475b-127">Ajoutez l’élément bindingRedirect suivant au fichier Web.config dans la racine de l’application sous la section de configuration :</span><span class="sxs-lookup"><span data-stu-id="5475b-127">Add the following bindingRedirect element to the Web.config file in the application root under the configuraton section:</span></span>   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. <span data-ttu-id="5475b-128">Créez une application ASP.NET MVC 2 vide.</span><span class="sxs-lookup"><span data-stu-id="5475b-128">Create a new empty ASP.NET MVC 2 application.</span></span> <span data-ttu-id="5475b-129">Copiez les fichiers à partir du dossier Scripts de la nouvelle application dans le dossier Scripts de l’application existante.</span><span class="sxs-lookup"><span data-stu-id="5475b-129">Copy the files from the Scripts folder of the new application into the Scripts folder of the existing application.</span></span>
8. <span data-ttu-id="5475b-130">Mettre à jour le € d’application existant™ fichier CSS de s avec les définitions de style CSS dans le fichier Site.css.</span><span class="sxs-lookup"><span data-stu-id="5475b-130">Update the existing applicationâ€™s CSS file with the CSS style definitions in the Site.css file.</span></span>
9. <span data-ttu-id="5475b-131">Compilez l’application et exécutez-la.</span><span class="sxs-lookup"><span data-stu-id="5475b-131">Compile the application and run it.</span></span> <span data-ttu-id="5475b-132">Si des erreurs se produisent, reportez-vous à la section de modifications avec rupture de la [What ' s New in ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) page.</span><span class="sxs-lookup"><span data-stu-id="5475b-132">If any errors occur, refer to the Breaking Changes section of the [What's New in ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) page.</span></span>
