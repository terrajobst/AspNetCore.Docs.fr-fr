---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-cs
title: Faites glisser et déposez via ReorderList fournissant (c#) | Documents Microsoft
author: wenz
description: Le contrôle ReorderList fournissant dans la boîte à outils de contrôle AJAX fournit une liste qui peut être réorganisée par l’utilisateur via la fonction glisser- déposer. L’ordre actuel de la liste est en cours...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 6350ee8e-11d6-4aff-b51c-942878014835
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 42464d10f119e0ba51d5eebf2a67e76e3e419bda
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873454"
---
<a name="drag-and-drop-via-reorderlist-c"></a><span data-ttu-id="8addc-104">Faites glisser et déposez via ReorderList fournissant (c#)</span><span class="sxs-lookup"><span data-stu-id="8addc-104">Drag and Drop via ReorderList (C#)</span></span>
====================
<span data-ttu-id="8addc-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8addc-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8addc-106">[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="8addc-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5CS.pdf)</span></span>

> <span data-ttu-id="8addc-107">Le contrôle ReorderList fournissant dans la boîte à outils de contrôle AJAX fournit une liste qui peut être réorganisée par l’utilisateur via la fonction glisser- déposer.</span><span class="sxs-lookup"><span data-stu-id="8addc-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="8addc-108">L’ordre actuel de la liste doit être conservé sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="8addc-108">The current order of the list shall be persisted on the server.</span></span>


## <a name="overview"></a><span data-ttu-id="8addc-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="8addc-109">Overview</span></span>

<span data-ttu-id="8addc-110">Le `ReorderList` contrôle dans la boîte à outils de contrôle AJAX fournit une liste qui peut être réorganisée par l’utilisateur via la fonction glisser- déposer.</span><span class="sxs-lookup"><span data-stu-id="8addc-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="8addc-111">L’ordre actuel de la liste doit être conservé sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="8addc-111">The current order of the list shall be persisted on the server.</span></span>

## <a name="steps"></a><span data-ttu-id="8addc-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="8addc-112">Steps</span></span>

<span data-ttu-id="8addc-113">Le `ReorderList` contrôle prend en charge la liaison de données à partir d’une base de données à la liste.</span><span class="sxs-lookup"><span data-stu-id="8addc-113">The `ReorderList` control supports binding data from a database to the list.</span></span> <span data-ttu-id="8addc-114">De plus, il prend également en charge l’écriture des modifications à l’ordre de l’élément de liste dans le magasin de données.</span><span class="sxs-lookup"><span data-stu-id="8addc-114">Best of all, it also supports writing changes to the order of the list element back to the data store.</span></span>

<span data-ttu-id="8addc-115">Cet exemple utilise Microsoft SQL Server 2005 Express Edition en tant que le magasin de données.</span><span class="sxs-lookup"><span data-stu-id="8addc-115">This sample uses Microsoft SQL Server 2005 Express Edition as the data store.</span></span> <span data-ttu-id="8addc-116">La base de données est une partie facultative (et gratuite) d’une installation de Visual Studio, y compris l’édition expresse.</span><span class="sxs-lookup"><span data-stu-id="8addc-116">The database is an optional (and free) part of a Visual Studio installation, including express edition.</span></span> <span data-ttu-id="8addc-117">Il est également disponible en téléchargement séparé sous [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="8addc-117">It is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="8addc-118">Pour cet exemple, nous partons du principe que l’instance de SQL Server 2005 Express Edition est appelée `SQLEXPRESS` et réside sur le même ordinateur que le serveur web ; il s’agit également de la configuration par défaut.</span><span class="sxs-lookup"><span data-stu-id="8addc-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="8addc-119">Si votre programme d’installation est différente, vous devez adapter les informations de connexion pour la base de données.</span><span class="sxs-lookup"><span data-stu-id="8addc-119">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="8addc-120">Pour configurer la base de données le plus simple consiste à utiliser le Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = fr](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span><span class="sxs-lookup"><span data-stu-id="8addc-120">The easiest way to set up the database is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span></span> <span data-ttu-id="8addc-121">Se connecter au serveur, double-cliquez sur `Databases` et créer une base de données (avec le bouton droit et choisissez `New Database`) appelée `Tutorials`.</span><span class="sxs-lookup"><span data-stu-id="8addc-121">Connect to the server, double-click on `Databases` and create a new database (right-click and choose `New Database`) called `Tutorials`.</span></span>

<span data-ttu-id="8addc-122">Dans cette base de données, créez une nouvelle table appelée `AJAX` avec les quatre colonnes suivantes :</span><span class="sxs-lookup"><span data-stu-id="8addc-122">In this database, create a new table called `AJAX` with the following four columns:</span></span>

- <span data-ttu-id="8addc-123">`id` (entier de clés, primaire, identité, non NULL)</span><span class="sxs-lookup"><span data-stu-id="8addc-123">`id` (primary key, integer, identity, not NULL)</span></span>
- <span data-ttu-id="8addc-124">`char` (char (1), valeur NULL)</span><span class="sxs-lookup"><span data-stu-id="8addc-124">`char` (char(1), NULL)</span></span>
- <span data-ttu-id="8addc-125">`description` (varchar (50), valeur NULL)</span><span class="sxs-lookup"><span data-stu-id="8addc-125">`description` (varchar(50), NULL)</span></span>
- <span data-ttu-id="8addc-126">`position` (int, NULL)</span><span class="sxs-lookup"><span data-stu-id="8addc-126">`position` (int, NULL)</span></span>


<span data-ttu-id="8addc-127">[![La disposition de la table AJAX](drag-and-drop-via-reorderlist-cs/_static/image2.png)](drag-and-drop-via-reorderlist-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8addc-127">[![The layout of the AJAX table](drag-and-drop-via-reorderlist-cs/_static/image2.png)](drag-and-drop-via-reorderlist-cs/_static/image1.png)</span></span>

<span data-ttu-id="8addc-128">La disposition de la table AJAX ([cliquez pour afficher l’image en taille réelle](drag-and-drop-via-reorderlist-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="8addc-128">The layout of the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image3.png))</span></span>


<span data-ttu-id="8addc-129">Ensuite, remplir la table avec deux valeurs.</span><span class="sxs-lookup"><span data-stu-id="8addc-129">Next, fill the table with a couple of values.</span></span> <span data-ttu-id="8addc-130">Notez que la `position` colonne contient l’ordre de tri des éléments.</span><span class="sxs-lookup"><span data-stu-id="8addc-130">Note that the `position` column holds the sort order of the elements.</span></span>


<span data-ttu-id="8addc-131">[![Les données initiales dans la table AJAX](drag-and-drop-via-reorderlist-cs/_static/image5.png)](drag-and-drop-via-reorderlist-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="8addc-131">[![The initial data in the AJAX table](drag-and-drop-via-reorderlist-cs/_static/image5.png)](drag-and-drop-via-reorderlist-cs/_static/image4.png)</span></span>

<span data-ttu-id="8addc-132">Les données initiales dans la table AJAX ([cliquez pour afficher l’image en taille réelle](drag-and-drop-via-reorderlist-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="8addc-132">The initial data in the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image6.png))</span></span>


<span data-ttu-id="8addc-133">L’étape suivante a besoin pour générer un `SqlDataSource` contrôle de communiquer avec la nouvelle base de données et sa table.</span><span class="sxs-lookup"><span data-stu-id="8addc-133">The next step requires to generate an `SqlDataSource` control to communicate with the new database and its table.</span></span> <span data-ttu-id="8addc-134">La source de données doit prendre en charge la `SELECT` et `UPDATE` des commandes SQL.</span><span class="sxs-lookup"><span data-stu-id="8addc-134">The data source must support the `SELECT` and `UPDATE` SQL commands.</span></span> <span data-ttu-id="8addc-135">Lorsque l’ordre des éléments de liste est modifié, le `ReorderList` contrôle envoie automatiquement des deux valeurs à la source de données `Update` commande : la nouvelle position et l’ID de l’élément.</span><span class="sxs-lookup"><span data-stu-id="8addc-135">When the order of the list elements is later changed, the `ReorderList` control automatically submits two values to the data source's `Update` command: the new position and the ID of the element.</span></span> <span data-ttu-id="8addc-136">Par conséquent, la source de données doit un `<UpdateParameters>` section pour ces deux valeurs :</span><span class="sxs-lookup"><span data-stu-id="8addc-136">Therefore, the data source needs an `<UpdateParameters>` section for these two values:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample1.aspx)]

<span data-ttu-id="8addc-137">Le `ReorderList` contrôle doit définir les attributs suivants :</span><span class="sxs-lookup"><span data-stu-id="8addc-137">The `ReorderList` control needs to set the following attributes:</span></span>

- <span data-ttu-id="8addc-138">`AllowReorder`: Indique si les éléments de liste peuvent être réorganisées</span><span class="sxs-lookup"><span data-stu-id="8addc-138">`AllowReorder`: Whether the list items may be rearranged</span></span>
- <span data-ttu-id="8addc-139">`DataSourceID`: L’ID de la source de données</span><span class="sxs-lookup"><span data-stu-id="8addc-139">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="8addc-140">`DataKeyField`: Le nom de la colonne de clé primaire dans la source de données</span><span class="sxs-lookup"><span data-stu-id="8addc-140">`DataKeyField`: The name of the primary key column in the data source</span></span>
- <span data-ttu-id="8addc-141">`SortOrderField`: La colonne de source de données qui fournit l’ordre de tri pour les éléments de liste</span><span class="sxs-lookup"><span data-stu-id="8addc-141">`SortOrderField`: The data source column that provides the sort order for the list items</span></span>

<span data-ttu-id="8addc-142">Dans le `<DragHandleTemplate>` et `<ItemTemplate>` sections, la disposition de la liste peut être ajustée.</span><span class="sxs-lookup"><span data-stu-id="8addc-142">In the `<DragHandleTemplate>` and `<ItemTemplate>` sections, the layout of the list can be fine-tuned.</span></span> <span data-ttu-id="8addc-143">En outre, la liaison de données est possible à l’aide de la `Eval()` méthode, comme illustré ci-après :</span><span class="sxs-lookup"><span data-stu-id="8addc-143">Also, databinding is possible using the `Eval()` method, as seen here:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample2.aspx)]

<span data-ttu-id="8addc-144">Informations de style CSS suivantes (référencé dans le `<DragHandleTemplate>` section de la `ReorderList` contrôle) permet de s’assurer que le pointeur de la souris change en conséquence lorsqu’il pointe sur la poignée de déplacement :</span><span class="sxs-lookup"><span data-stu-id="8addc-144">The following CSS style information (referenced in the `<DragHandleTemplate>` section of the `ReorderList` control) makes sure that the mouse pointer changes appropriately when it hovers over the drag handle:</span></span>

[!code-css[Main](drag-and-drop-via-reorderlist-cs/samples/sample3.css)]

<span data-ttu-id="8addc-145">Enfin, un `ScriptManager` contrôle initialise ASP.NET AJAX pour la page :</span><span class="sxs-lookup"><span data-stu-id="8addc-145">Finally, a `ScriptManager` control initializes ASP.NET AJAX for the page:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample4.aspx)]

<span data-ttu-id="8addc-146">Exécuter cet exemple dans le navigateur et réorganiser les éléments de liste un peu.</span><span class="sxs-lookup"><span data-stu-id="8addc-146">Run this example in the browser and rearrange the list items a bit.</span></span> <span data-ttu-id="8addc-147">Ensuite, rechargez la page et/ou un coup de œil à la base de données.</span><span class="sxs-lookup"><span data-stu-id="8addc-147">Then, reload the page and/or have a look at the database.</span></span> <span data-ttu-id="8addc-148">Les positions modifiées ont été conservées et sont également reflétées par les valeurs de la `position` colonne dans la base de données et que tout sans code, juste à l’aide de balisage.</span><span class="sxs-lookup"><span data-stu-id="8addc-148">The altered positions have been maintained and are also reflected by the values in the `position` column in the database and that all without any code, just by using markup.</span></span>


<span data-ttu-id="8addc-149">[![Les données dans les modifications de base de données en fonction de la nouvelle commande d’élément de liste](drag-and-drop-via-reorderlist-cs/_static/image8.png)](drag-and-drop-via-reorderlist-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="8addc-149">[![The data in the database changes according to the new list item order](drag-and-drop-via-reorderlist-cs/_static/image8.png)](drag-and-drop-via-reorderlist-cs/_static/image7.png)</span></span>

<span data-ttu-id="8addc-150">Élément de données dans les modifications de base de données en fonction de la nouvelle liste d’ordre ([cliquez pour afficher l’image en taille réelle](drag-and-drop-via-reorderlist-cs/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="8addc-150">The data in the database changes according to the new list item order ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image9.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8addc-151">[Précédent](using-postbacks-with-reorderlist-cs.md)
> [Suivant](using-postbacks-with-reorderlist-vb.md)</span><span class="sxs-lookup"><span data-stu-id="8addc-151">[Previous](using-postbacks-with-reorderlist-cs.md)
[Next](using-postbacks-with-reorderlist-vb.md)</span></span>
