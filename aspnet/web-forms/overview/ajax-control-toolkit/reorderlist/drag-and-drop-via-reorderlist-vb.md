---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
title: Glisser -déplacer via ReorderList (VB) | Microsoft Docs
author: wenz
description: /Data-Access/Tutorials/Master-Detail-Using-a-Bulleted-List-of-Master-Records-with-a-Details-DataList-VB
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 848e6bcf-4c3f-4d14-974d-e45b9444ab79
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 78cdb53ce6cbf32ccdf913f30439734f94922e8f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814208"
---
<a name="drag-and-drop-via-reorderlist-vb"></a>Glisser -déplacer via ReorderList (VB)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)

> Le contrôle ReorderList dans AJAX Control Toolkit fournit une liste qui peut être réorganisée par l’utilisateur via la fonction glisser- déposer. L’ordre actuel de la liste doit être conservé sur le serveur.


## <a name="overview"></a>Vue d'ensemble

Le `ReorderList` contrôle dans AJAX Control Toolkit fournit une liste qui peut être réorganisée par l’utilisateur via la fonction glisser- déposer. L’ordre actuel de la liste doit être conservé sur le serveur.

## <a name="steps"></a>Étapes

Le `ReorderList` contrôle prend en charge la liaison de données à partir d’une base de données à la liste. De plus, il prend également en charge écriture des modifications à l’ordre de l’élément de liste dans le magasin de données.

Cet exemple utilise Microsoft SQL Server 2005 Express Edition en tant que le magasin de données. La base de données est une partie facultative (et gratuite) d’une installation de Visual Studio, y compris l’édition expresse. Il est également disponible en téléchargement séparé sous [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Pour cet exemple, nous partons du principe que l’instance de SQL Server 2005 Express Edition est appelée `SQLEXPRESS` et se trouve sur le même ordinateur que le serveur web ; il s’agit également de la configuration par défaut. Si votre programme d’installation est différente, vous devez adapter les informations de connexion pour la base de données.

Pour configurer la base de données le plus simple consiste à utiliser le Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = fr](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ). Se connecter au serveur, double-cliquez sur `Databases` et créer une base de données (avec le bouton droit et choisissez `New Database`) appelée `Tutorials`.

Dans cette base de données, créez une nouvelle table appelée `AJAX` avec les quatre colonnes suivantes :

- `id` (principal clés, de type entier, identité, non NULL)
- `char` (char (1), NULL)
- `description` (varchar (50), NULL)
- `position` (int, NULL)


[![La disposition de la table d’AJAX](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)

La disposition de la table AJAX ([cliquez pour afficher l’image en taille réelle](drag-and-drop-via-reorderlist-vb/_static/image3.png))


Ensuite, remplissez la table avec deux valeurs. Notez que le `position` colonne contient l’ordre de tri des éléments.


[![Les données initiales dans la table d’AJAX](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)

Les données initiales dans la table AJAX ([cliquez pour afficher l’image en taille réelle](drag-and-drop-via-reorderlist-vb/_static/image6.png))


L’étape suivante a besoin pour générer un `SqlDataSource` contrôle pour communiquer avec la nouvelle base de données et sa table. La source de données doit prendre en charge la `SELECT` et `UPDATE` commandes SQL. Lorsque l’ordre des éléments de liste est modifié, le `ReorderList` contrôle envoie automatiquement des deux valeurs à la source de données `Update` commande : la nouvelle position et l’ID de l’élément. Par conséquent, les besoins de source de données un `<UpdateParameters>` section pour ces deux valeurs :

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample1.aspx)]

Le `ReorderList` contrôle doit définir les attributs suivants :

- `AllowReorder`: Indique si les éléments de liste peuvent être réorganisés
- `DataSourceID`: L’ID de la source de données
- `DataKeyField`: Nom de la colonne de clé primaire dans la source de données
- `SortOrderField`: La colonne de source de données qui fournit l’ordre de tri pour les éléments de liste

Dans le `<DragHandleTemplate>` et `<ItemTemplate>` sections, la disposition de la liste peut être ajustée. En outre, la liaison de données est possible à l’aide de la `Eval()` méthode, comme illustré ici :

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample2.aspx)]

Le code CSS suivant des informations de style (référencé dans le `<DragHandleTemplate>` section de la `ReorderList` contrôle) permet de s’assurer que le pointeur de la souris change en conséquence lorsqu’il est placé sur la poignée de déplacement :

[!code-css[Main](drag-and-drop-via-reorderlist-vb/samples/sample3.css)]

Enfin, un `ScriptManager` contrôle initialise ASP.NET AJAX pour la page :

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample4.aspx)]

Exécuter cet exemple dans le navigateur et réorganiser les éléments de liste un peu. Ensuite, rechargez la page et/ou un coup de œil à la base de données. Les positions modifiées ont été maintenues et sont également appliquées par les valeurs de la `position` colonne dans la base de données et tout cela sans aucun code, juste à l’aide de balisage.


[![Les données dans les modifications de base de données en fonction de la nouvelle commande d’élément de liste](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)

Élément de données dans les modifications de base de données en fonction de la nouvelle liste d’ordre ([cliquez pour afficher l’image en taille réelle](drag-and-drop-via-reorderlist-vb/_static/image9.png))

> [!div class="step-by-step"]
> [Précédent](using-postbacks-with-reorderlist-vb.md)
