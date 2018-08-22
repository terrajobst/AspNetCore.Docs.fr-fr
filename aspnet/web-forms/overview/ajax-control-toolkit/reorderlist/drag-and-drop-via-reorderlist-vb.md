---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
title: Glisser -déplacer via ReorderList (VB) | Microsoft Docs
author: wenz
description: /Data-Access/Tutorials/Master-Detail-Using-a-Bulleted-List-of-Master-Records-with-a-Details-DataList-VB
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 848e6bcf-4c3f-4d14-974d-e45b9444ab79
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 12c5aa60aa6bb71c99e267a1a71b40ed718df8cf
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829259"
---
<a name="drag-and-drop-via-reorderlist-vb"></a><span data-ttu-id="4e7a6-103">Glisser -déplacer via ReorderList (VB)</span><span class="sxs-lookup"><span data-stu-id="4e7a6-103">Drag and Drop via ReorderList (VB)</span></span>
====================
<span data-ttu-id="4e7a6-104">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4e7a6-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4e7a6-105">[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="4e7a6-105">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)</span></span>

> <span data-ttu-id="4e7a6-106">Le contrôle ReorderList dans AJAX Control Toolkit fournit une liste qui peut être réorganisée par l’utilisateur via la fonction glisser- déposer.</span><span class="sxs-lookup"><span data-stu-id="4e7a6-106">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="4e7a6-107">L’ordre actuel de la liste doit être conservé sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="4e7a6-107">The current order of the list shall be persisted on the server.</span></span>


## <a name="overview"></a><span data-ttu-id="4e7a6-108">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="4e7a6-108">Overview</span></span>

<span data-ttu-id="4e7a6-109">Le `ReorderList` contrôle dans AJAX Control Toolkit fournit une liste qui peut être réorganisée par l’utilisateur via la fonction glisser- déposer.</span><span class="sxs-lookup"><span data-stu-id="4e7a6-109">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="4e7a6-110">L’ordre actuel de la liste doit être conservé sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="4e7a6-110">The current order of the list shall be persisted on the server.</span></span>

## <a name="steps"></a><span data-ttu-id="4e7a6-111">Étapes</span><span class="sxs-lookup"><span data-stu-id="4e7a6-111">Steps</span></span>

<span data-ttu-id="4e7a6-112">Le `ReorderList` contrôle prend en charge la liaison de données à partir d’une base de données à la liste.</span><span class="sxs-lookup"><span data-stu-id="4e7a6-112">The `ReorderList` control supports binding data from a database to the list.</span></span> <span data-ttu-id="4e7a6-113">De plus, il prend également en charge écriture des modifications à l’ordre de l’élément de liste dans le magasin de données.</span><span class="sxs-lookup"><span data-stu-id="4e7a6-113">Best of all, it also supports writing changes to the order of the list element back to the data store.</span></span>

<span data-ttu-id="4e7a6-114">Cet exemple utilise Microsoft SQL Server 2005 Express Edition en tant que le magasin de données.</span><span class="sxs-lookup"><span data-stu-id="4e7a6-114">This sample uses Microsoft SQL Server 2005 Express Edition as the data store.</span></span> <span data-ttu-id="4e7a6-115">La base de données est une partie facultative (et gratuite) d’une installation de Visual Studio, y compris l’édition expresse.</span><span class="sxs-lookup"><span data-stu-id="4e7a6-115">The database is an optional (and free) part of a Visual Studio installation, including express edition.</span></span> <span data-ttu-id="4e7a6-116">Il est également disponible en téléchargement séparé sous [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="4e7a6-116">It is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="4e7a6-117">Pour cet exemple, nous partons du principe que l’instance de SQL Server 2005 Express Edition est appelée `SQLEXPRESS` et se trouve sur le même ordinateur que le serveur web ; il s’agit également de la configuration par défaut.</span><span class="sxs-lookup"><span data-stu-id="4e7a6-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="4e7a6-118">Si votre programme d’installation est différente, vous devez adapter les informations de connexion pour la base de données.</span><span class="sxs-lookup"><span data-stu-id="4e7a6-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="4e7a6-119">Pour configurer la base de données le plus simple consiste à utiliser le Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = fr](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span><span class="sxs-lookup"><span data-stu-id="4e7a6-119">The easiest way to set up the database is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span></span> <span data-ttu-id="4e7a6-120">Se connecter au serveur, double-cliquez sur `Databases` et créer une base de données (avec le bouton droit et choisissez `New Database`) appelée `Tutorials`.</span><span class="sxs-lookup"><span data-stu-id="4e7a6-120">Connect to the server, double-click on `Databases` and create a new database (right-click and choose `New Database`) called `Tutorials`.</span></span>

<span data-ttu-id="4e7a6-121">Dans cette base de données, créez une nouvelle table appelée `AJAX` avec les quatre colonnes suivantes :</span><span class="sxs-lookup"><span data-stu-id="4e7a6-121">In this database, create a new table called `AJAX` with the following four columns:</span></span>

- <span data-ttu-id="4e7a6-122">`id` (principal clés, de type entier, identité, non NULL)</span><span class="sxs-lookup"><span data-stu-id="4e7a6-122">`id` (primary key, integer, identity, not NULL)</span></span>
- <span data-ttu-id="4e7a6-123">`char` (char (1), NULL)</span><span class="sxs-lookup"><span data-stu-id="4e7a6-123">`char` (char(1), NULL)</span></span>
- <span data-ttu-id="4e7a6-124">`description` (varchar (50), NULL)</span><span class="sxs-lookup"><span data-stu-id="4e7a6-124">`description` (varchar(50), NULL)</span></span>
- <span data-ttu-id="4e7a6-125">`position` (int, NULL)</span><span class="sxs-lookup"><span data-stu-id="4e7a6-125">`position` (int, NULL)</span></span>


<span data-ttu-id="4e7a6-126">[![La disposition de la table d’AJAX](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4e7a6-126">[![The layout of the AJAX table](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)</span></span>

<span data-ttu-id="4e7a6-127">La disposition de la table AJAX ([cliquez pour afficher l’image en taille réelle](drag-and-drop-via-reorderlist-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4e7a6-127">The layout of the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image3.png))</span></span>


<span data-ttu-id="4e7a6-128">Ensuite, remplissez la table avec deux valeurs.</span><span class="sxs-lookup"><span data-stu-id="4e7a6-128">Next, fill the table with a couple of values.</span></span> <span data-ttu-id="4e7a6-129">Notez que le `position` colonne contient l’ordre de tri des éléments.</span><span class="sxs-lookup"><span data-stu-id="4e7a6-129">Note that the `position` column holds the sort order of the elements.</span></span>


<span data-ttu-id="4e7a6-130">[![Les données initiales dans la table d’AJAX](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="4e7a6-130">[![The initial data in the AJAX table](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)</span></span>

<span data-ttu-id="4e7a6-131">Les données initiales dans la table AJAX ([cliquez pour afficher l’image en taille réelle](drag-and-drop-via-reorderlist-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="4e7a6-131">The initial data in the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image6.png))</span></span>


<span data-ttu-id="4e7a6-132">L’étape suivante a besoin pour générer un `SqlDataSource` contrôle pour communiquer avec la nouvelle base de données et sa table.</span><span class="sxs-lookup"><span data-stu-id="4e7a6-132">The next step requires to generate an `SqlDataSource` control to communicate with the new database and its table.</span></span> <span data-ttu-id="4e7a6-133">La source de données doit prendre en charge la `SELECT` et `UPDATE` commandes SQL.</span><span class="sxs-lookup"><span data-stu-id="4e7a6-133">The data source must support the `SELECT` and `UPDATE` SQL commands.</span></span> <span data-ttu-id="4e7a6-134">Lorsque l’ordre des éléments de liste est modifié, le `ReorderList` contrôle envoie automatiquement des deux valeurs à la source de données `Update` commande : la nouvelle position et l’ID de l’élément.</span><span class="sxs-lookup"><span data-stu-id="4e7a6-134">When the order of the list elements is later changed, the `ReorderList` control automatically submits two values to the data source's `Update` command: the new position and the ID of the element.</span></span> <span data-ttu-id="4e7a6-135">Par conséquent, les besoins de source de données un `<UpdateParameters>` section pour ces deux valeurs :</span><span class="sxs-lookup"><span data-stu-id="4e7a6-135">Therefore, the data source needs an `<UpdateParameters>` section for these two values:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample1.aspx)]

<span data-ttu-id="4e7a6-136">Le `ReorderList` contrôle doit définir les attributs suivants :</span><span class="sxs-lookup"><span data-stu-id="4e7a6-136">The `ReorderList` control needs to set the following attributes:</span></span>

- <span data-ttu-id="4e7a6-137">`AllowReorder`: Indique si les éléments de liste peuvent être réorganisés</span><span class="sxs-lookup"><span data-stu-id="4e7a6-137">`AllowReorder`: Whether the list items may be rearranged</span></span>
- <span data-ttu-id="4e7a6-138">`DataSourceID`: L’ID de la source de données</span><span class="sxs-lookup"><span data-stu-id="4e7a6-138">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="4e7a6-139">`DataKeyField`: Nom de la colonne de clé primaire dans la source de données</span><span class="sxs-lookup"><span data-stu-id="4e7a6-139">`DataKeyField`: The name of the primary key column in the data source</span></span>
- <span data-ttu-id="4e7a6-140">`SortOrderField`: La colonne de source de données qui fournit l’ordre de tri pour les éléments de liste</span><span class="sxs-lookup"><span data-stu-id="4e7a6-140">`SortOrderField`: The data source column that provides the sort order for the list items</span></span>

<span data-ttu-id="4e7a6-141">Dans le `<DragHandleTemplate>` et `<ItemTemplate>` sections, la disposition de la liste peut être ajustée.</span><span class="sxs-lookup"><span data-stu-id="4e7a6-141">In the `<DragHandleTemplate>` and `<ItemTemplate>` sections, the layout of the list can be fine-tuned.</span></span> <span data-ttu-id="4e7a6-142">En outre, la liaison de données est possible à l’aide de la `Eval()` méthode, comme illustré ici :</span><span class="sxs-lookup"><span data-stu-id="4e7a6-142">Also, databinding is possible using the `Eval()` method, as seen here:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample2.aspx)]

<span data-ttu-id="4e7a6-143">Le code CSS suivant des informations de style (référencé dans le `<DragHandleTemplate>` section de la `ReorderList` contrôle) permet de s’assurer que le pointeur de la souris change en conséquence lorsqu’il est placé sur la poignée de déplacement :</span><span class="sxs-lookup"><span data-stu-id="4e7a6-143">The following CSS style information (referenced in the `<DragHandleTemplate>` section of the `ReorderList` control) makes sure that the mouse pointer changes appropriately when it hovers over the drag handle:</span></span>

[!code-css[Main](drag-and-drop-via-reorderlist-vb/samples/sample3.css)]

<span data-ttu-id="4e7a6-144">Enfin, un `ScriptManager` contrôle initialise ASP.NET AJAX pour la page :</span><span class="sxs-lookup"><span data-stu-id="4e7a6-144">Finally, a `ScriptManager` control initializes ASP.NET AJAX for the page:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample4.aspx)]

<span data-ttu-id="4e7a6-145">Exécuter cet exemple dans le navigateur et réorganiser les éléments de liste un peu.</span><span class="sxs-lookup"><span data-stu-id="4e7a6-145">Run this example in the browser and rearrange the list items a bit.</span></span> <span data-ttu-id="4e7a6-146">Ensuite, rechargez la page et/ou un coup de œil à la base de données.</span><span class="sxs-lookup"><span data-stu-id="4e7a6-146">Then, reload the page and/or have a look at the database.</span></span> <span data-ttu-id="4e7a6-147">Les positions modifiées ont été maintenues et sont également appliquées par les valeurs de la `position` colonne dans la base de données et tout cela sans aucun code, juste à l’aide de balisage.</span><span class="sxs-lookup"><span data-stu-id="4e7a6-147">The altered positions have been maintained and are also reflected by the values in the `position` column in the database and that all without any code, just by using markup.</span></span>


<span data-ttu-id="4e7a6-148">[![Les données dans les modifications de base de données en fonction de la nouvelle commande d’élément de liste](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="4e7a6-148">[![The data in the database changes according to the new list item order](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)</span></span>

<span data-ttu-id="4e7a6-149">Élément de données dans les modifications de base de données en fonction de la nouvelle liste d’ordre ([cliquez pour afficher l’image en taille réelle](drag-and-drop-via-reorderlist-vb/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="4e7a6-149">The data in the database changes according to the new list item order ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image9.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4e7a6-150">Précédent</span><span class="sxs-lookup"><span data-stu-id="4e7a6-150">Previous</span></span>](using-postbacks-with-reorderlist-vb.md)
