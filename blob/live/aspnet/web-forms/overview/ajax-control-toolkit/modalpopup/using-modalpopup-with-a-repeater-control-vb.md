---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-vb
title: "À l’aide de ModalPopup avec un contrôle de répéteur (VB) | Documents Microsoft"
author: wenz
description: "Le contrôle ModalPopup dans la boîte à outils de contrôle AJAX offre un moyen simple de créer une fenêtre modale, à l’aide des moyens de côté client. Il est également possible d’utiliser ce contrat..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0c8e74f1-b3ba-4ca9-a1c5-f5c4831a359a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: a1e49fdebfb3ad62667ffd5a979d366730a097bd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="using-modalpopup-with-a-repeater-control-vb"></a><span data-ttu-id="eb5f9-104">À l’aide de ModalPopup avec un contrôle de répéteur (VB)</span><span class="sxs-lookup"><span data-stu-id="eb5f9-104">Using ModalPopup with a Repeater Control (VB)</span></span>
====================
<span data-ttu-id="eb5f9-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="eb5f9-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="eb5f9-106">[Télécharger le Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="eb5f9-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2VB.pdf)</span></span>

> <span data-ttu-id="eb5f9-107">Le contrôle ModalPopup dans la boîte à outils de contrôle AJAX offre un moyen simple de créer une fenêtre modale, à l’aide des moyens de côté client.</span><span class="sxs-lookup"><span data-stu-id="eb5f9-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="eb5f9-108">Il est également possible d’utiliser ce contrôle dans un répéteur.</span><span class="sxs-lookup"><span data-stu-id="eb5f9-108">It is also possible to use this control within a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="eb5f9-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="eb5f9-109">Overview</span></span>

<span data-ttu-id="eb5f9-110">Le contrôle ModalPopup dans la boîte à outils de contrôle AJAX offre un moyen simple de créer une fenêtre modale, à l’aide des moyens de côté client.</span><span class="sxs-lookup"><span data-stu-id="eb5f9-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="eb5f9-111">Il est également possible d’utiliser ce contrôle dans un répéteur.</span><span class="sxs-lookup"><span data-stu-id="eb5f9-111">It is also possible to use this control within a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="eb5f9-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="eb5f9-112">Steps</span></span>

<span data-ttu-id="eb5f9-113">Tout d’abord, une source de données est requise.</span><span class="sxs-lookup"><span data-stu-id="eb5f9-113">First of all, a data source is required.</span></span> <span data-ttu-id="eb5f9-114">Cet exemple utilise la base de données AdventureWorks et Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="eb5f9-114">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="eb5f9-115">La base de données est une partie facultative d’une installation de Visual Studio (y compris l’édition expresse) et est également disponible en tant que téléchargement séparé sous [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="eb5f9-115">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="eb5f9-116">La base de données AdventureWorks fait partie des exemples de SQL Server 2005 et exemples de bases de données (téléchargement à l’adresse [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = fr](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="eb5f9-116">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="eb5f9-117">Pour configurer la base de données le plus simple consiste à utiliser le Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx? FamilyID = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = fr](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) et d’attacher le `AdventureWorks.mdf` le fichier de base de données.</span><span class="sxs-lookup"><span data-stu-id="eb5f9-117">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span> <span data-ttu-id="eb5f9-118">Pour cet exemple, nous partons du principe que l’instance de SQL Server 2005 Express Edition est appelée `SQLEXPRESS` et réside sur le même ordinateur que le serveur web ; il s’agit également de la configuration par défaut.</span><span class="sxs-lookup"><span data-stu-id="eb5f9-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="eb5f9-119">Si votre programme d’installation est différente, vous devez adapter les informations de connexion pour la base de données.</span><span class="sxs-lookup"><span data-stu-id="eb5f9-119">If your setup differs, you have to adapt the connection information for the database.</span></span> <span data-ttu-id="eb5f9-120">Pour activer la fonctionnalité d’ASP.NET AJAX et les outils de contrôle, le `ScriptManager` contrôle doit être placé n’importe où sur la page (mais dans le `<form>` élément) :</span><span class="sxs-lookup"><span data-stu-id="eb5f9-120">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample1.aspx)]

<span data-ttu-id="eb5f9-121">Ensuite, ajoutez une source de données à la page.</span><span class="sxs-lookup"><span data-stu-id="eb5f9-121">Then, add a data source to the page.</span></span> <span data-ttu-id="eb5f9-122">Pour utiliser une quantité limitée de données, nous allons sélectionner uniquement les cinq premières entrées dans la table du fournisseur de la base de données AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="eb5f9-122">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="eb5f9-123">Si vous utilisez l’assistant de Visual Studio pour créer la source de données, à l’esprit qu’un bogue dans la version actuelle ne précède pas le nom de table (`Vendor`) avec `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="eb5f9-123">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="eb5f9-124">Le balisage suivant illustre la syntaxe correcte :</span><span class="sxs-lookup"><span data-stu-id="eb5f9-124">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample2.aspx)]

<span data-ttu-id="eb5f9-125">Ensuite, ajoutez un panneau qui sert de la fenêtre contextuelle modale.</span><span class="sxs-lookup"><span data-stu-id="eb5f9-125">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="eb5f9-126">Il contient un `Button` contrôle pour fermer la fenêtre contextuelle à nouveau :</span><span class="sxs-lookup"><span data-stu-id="eb5f9-126">It contains a `Button` control to close the popup again:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample3.aspx)]

<span data-ttu-id="eb5f9-127">Pour utiliser le menu contextuel dans le répéteur, le `ModalPopupExtender` contrôle doit être placé dans la `<ItemTemplate>` section du répéteur.</span><span class="sxs-lookup"><span data-stu-id="eb5f9-127">In order to make the popup work within the repeater, the `ModalPopupExtender` control must be put within the `<ItemTemplate>` section of the repeater.</span></span> <span data-ttu-id="eb5f9-128">Par conséquent, le panneau de configuration est en dehors de la répétition, mais l’extendeur est à l’intérieur.</span><span class="sxs-lookup"><span data-stu-id="eb5f9-128">So the panel is outside the repeater, but the extender is inside.</span></span> <span data-ttu-id="eb5f9-129">Voici le balisage de répéteur :</span><span class="sxs-lookup"><span data-stu-id="eb5f9-129">Here is the markup for the repeater:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample4.aspx)]

<span data-ttu-id="eb5f9-130">Ensuite, chaque élément dans la source de données s’affiche avec un bouton en regard de celui-ci qui déclenche la fenêtre contextuelle modale.</span><span class="sxs-lookup"><span data-stu-id="eb5f9-130">Then, every item in the data source is displayed with a button next to it that triggers the modal popup.</span></span>


<span data-ttu-id="eb5f9-131">[![La fenêtre contextuelle modale peut être déclenchée pour chaque entrée de source de données](using-modalpopup-with-a-repeater-control-vb/_static/image2.png)](using-modalpopup-with-a-repeater-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="eb5f9-131">[![The modal popup can be triggered for every data source entry](using-modalpopup-with-a-repeater-control-vb/_static/image2.png)](using-modalpopup-with-a-repeater-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="eb5f9-132">La fenêtre contextuelle modale peut être déclenchée pour chaque entrée de source de données ([cliquez pour afficher l’image en taille réelle](using-modalpopup-with-a-repeater-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="eb5f9-132">The modal popup can be triggered for every data source entry ([Click to view full-size image](using-modalpopup-with-a-repeater-control-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="eb5f9-133">[Précédent](launching-a-modal-popup-window-from-server-code-vb.md)
[Suivant](handling-postbacks-from-a-modalpopup-vb.md)</span><span class="sxs-lookup"><span data-stu-id="eb5f9-133">[Previous](launching-a-modal-popup-window-from-server-code-vb.md)
[Next](handling-postbacks-from-a-modalpopup-vb.md)</span></span>
