---
uid: web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-cs
title: À l’aide de HoverMenu avec un contrôle du répéteur (c#) | Documents Microsoft
author: wenz
description: 'Le contrôle HoverMenu dans la boîte à outils de contrôle AJAX fournit un effet de la fenêtre contextuelle simple : lorsque le pointeur de la souris pointe sur un élément, une fenêtre contextuelle s’affiche en un caractéristiques...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e7700e7b-edc3-4183-a713-70e507cc7490
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: ff7a7ce3469a020df069c1339993d8893092d875
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872154"
---
<a name="using-hovermenu-with-a-repeater-control-c"></a><span data-ttu-id="f26dc-103">À l’aide de HoverMenu avec un contrôle du répéteur (c#)</span><span class="sxs-lookup"><span data-stu-id="f26dc-103">Using HoverMenu with a Repeater Control (C#)</span></span>
====================
<span data-ttu-id="f26dc-104">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f26dc-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f26dc-105">[Télécharger le Code](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="f26dc-105">[Download Code](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1CS.pdf)</span></span>

> <span data-ttu-id="f26dc-106">Le contrôle HoverMenu dans la boîte à outils de contrôle AJAX fournit un effet de la fenêtre contextuelle simple : lorsque le pointeur de la souris pointe sur un élément, une fenêtre contextuelle s’affiche à la position spécifiée.</span><span class="sxs-lookup"><span data-stu-id="f26dc-106">The HoverMenu control in the AJAX Control Toolkit provides a simple popup effect: When the mouse pointer hovers over an element, a popup appears at a specified position.</span></span> <span data-ttu-id="f26dc-107">Il est également possible d’utiliser ce contrôle dans un répéteur.</span><span class="sxs-lookup"><span data-stu-id="f26dc-107">It is also possible to use this control within a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="f26dc-108">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="f26dc-108">Overview</span></span>

<span data-ttu-id="f26dc-109">Le `HoverMenu` contrôle dans la boîte à outils de contrôle AJAX fournit un effet de la fenêtre contextuelle simple : lorsque le pointeur de la souris pointe sur un élément, une fenêtre contextuelle s’affiche à la position spécifiée.</span><span class="sxs-lookup"><span data-stu-id="f26dc-109">The `HoverMenu` control in the AJAX Control Toolkit provides a simple popup effect: When the mouse pointer hovers over an element, a popup appears at a specified position.</span></span> <span data-ttu-id="f26dc-110">Il est également possible d’utiliser ce contrôle dans un répéteur.</span><span class="sxs-lookup"><span data-stu-id="f26dc-110">It is also possible to use this control within a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="f26dc-111">Étapes</span><span class="sxs-lookup"><span data-stu-id="f26dc-111">Steps</span></span>

<span data-ttu-id="f26dc-112">Tout d’abord, une source de données est requise.</span><span class="sxs-lookup"><span data-stu-id="f26dc-112">First of all, a data source is required.</span></span> <span data-ttu-id="f26dc-113">Cet exemple utilise la base de données AdventureWorks et Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="f26dc-113">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="f26dc-114">La base de données est une partie facultative d’une installation de Visual Studio (y compris l’édition expresse) et est également disponible en tant que téléchargement séparé sous [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="f26dc-114">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="f26dc-115">La base de données AdventureWorks fait partie des exemples de SQL Server 2005 et exemples de bases de données (téléchargement à l’adresse [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = fr](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="f26dc-115">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="f26dc-116">Pour configurer la base de données le plus simple consiste à utiliser le Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = fr](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) et d’attacher le `AdventureWorks.mdf` le fichier de base de données.</span><span class="sxs-lookup"><span data-stu-id="f26dc-116">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="f26dc-117">Pour cet exemple, nous partons du principe que l’instance de SQL Server 2005 Express Edition est appelée `SQLEXPRESS` et réside sur le même ordinateur que le serveur web ; il s’agit également de la configuration par défaut.</span><span class="sxs-lookup"><span data-stu-id="f26dc-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="f26dc-118">Si votre programme d’installation est différente, vous devez adapter les informations de connexion pour la base de données.</span><span class="sxs-lookup"><span data-stu-id="f26dc-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="f26dc-119">Pour activer la fonctionnalité d’ASP.NET AJAX et les outils de contrôle, le `ScriptManager` contrôle doit être placé n’importe où sur la page (mais dans le `<form>` élément) :</span><span class="sxs-lookup"><span data-stu-id="f26dc-119">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample1.aspx)]

<span data-ttu-id="f26dc-120">Ensuite, ajoutez une source de données à la page.</span><span class="sxs-lookup"><span data-stu-id="f26dc-120">Then, add a data source to the page.</span></span> <span data-ttu-id="f26dc-121">Pour utiliser une quantité limitée de données, nous allons sélectionner uniquement les cinq premières entrées dans la table du fournisseur de la base de données AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="f26dc-121">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="f26dc-122">Si vous utilisez l’assistant de Visual Studio pour créer la source de données, à l’esprit qu’un bogue dans la version actuelle ne précède pas le nom de table (`Vendor`) avec `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="f26dc-122">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="f26dc-123">Le balisage suivant illustre la syntaxe correcte :</span><span class="sxs-lookup"><span data-stu-id="f26dc-123">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample2.aspx)]

<span data-ttu-id="f26dc-124">Ensuite, ajoutez un panneau qui sert de la fenêtre contextuelle modale :</span><span class="sxs-lookup"><span data-stu-id="f26dc-124">Next, add a panel which serves as the modal popup:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample3.aspx)]

<span data-ttu-id="f26dc-125">À présent, le `HoverMenuExtender` entrent en jeu.</span><span class="sxs-lookup"><span data-stu-id="f26dc-125">Now, the `HoverMenuExtender` comes into play.</span></span> <span data-ttu-id="f26dc-126">Afin que tous les éléments de la source de données obtient son propre menu contextuel, l’extendeur doit être placé au sein du répéteur `<ItemTemplate>` section.</span><span class="sxs-lookup"><span data-stu-id="f26dc-126">So that every element in the data source gets its own popup, the extender must be put within the repeater's `<ItemTemplate>` section.</span></span> <span data-ttu-id="f26dc-127">Voici le code XAML :</span><span class="sxs-lookup"><span data-stu-id="f26dc-127">Here is the markup:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample4.aspx)]

<span data-ttu-id="f26dc-128">Maintenant chaque élément dans la source de données affiche un menu contextuel à droite (`PopupPosition` attribut) après un délai de 50 millisecondes (`PopDelay` attribut).</span><span class="sxs-lookup"><span data-stu-id="f26dc-128">Now every item in the data source displays a popup to the right (`PopupPosition` attribute) after a delay of 50 milliseconds (`PopDelay` attribute).</span></span>


<span data-ttu-id="f26dc-129">[![Le menu sensitif s’affiche en regard de chaque élément dans le répéteur](using-hovermenu-with-a-repeater-control-cs/_static/image2.png)](using-hovermenu-with-a-repeater-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f26dc-129">[![The hover menu appears next to each item in the repeater](using-hovermenu-with-a-repeater-control-cs/_static/image2.png)](using-hovermenu-with-a-repeater-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="f26dc-130">Le menu sensitif s’affiche en regard de chaque élément dans le répéteur ([cliquez pour afficher l’image en taille réelle](using-hovermenu-with-a-repeater-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f26dc-130">The hover menu appears next to each item in the repeater ([Click to view full-size image](using-hovermenu-with-a-repeater-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f26dc-131">Next</span><span class="sxs-lookup"><span data-stu-id="f26dc-131">Next</span></span>](using-hovermenu-with-a-repeater-control-vb.md)
