---
uid: web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-cs
title: Utilisation de HoverMenu avec un contrôle Repeater (c#) | Microsoft Docs
author: wenz
description: 'Le contrôle HoverMenu dans AJAX Control Toolkit fournit un effet de la fenêtre contextuelle simple : lorsque le pointeur de la souris pointe sur un élément, une fenêtre contextuelle s’affiche à un seront...'
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: e7700e7b-edc3-4183-a713-70e507cc7490
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: f3135eb52bab1550b1c89dd6ce62044640e80198
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831995"
---
<a name="using-hovermenu-with-a-repeater-control-c"></a>Utilisation de HoverMenu avec un contrôle Repeater (c#)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1CS.pdf)

> Le contrôle HoverMenu dans AJAX Control Toolkit fournit un effet de la fenêtre contextuelle simple : lorsque le pointeur de la souris pointe sur un élément, une fenêtre contextuelle s’affiche à la position spécifiée. Il est également possible d’utiliser ce contrôle dans un répéteur.


## <a name="overview"></a>Vue d'ensemble

Le `HoverMenu` contrôle dans la boîte à outils de contrôle AJAX fournit un effet de la fenêtre contextuelle simple : lorsque le pointeur de la souris pointe sur un élément, une fenêtre contextuelle s’affiche à la position spécifiée. Il est également possible d’utiliser ce contrôle dans un répéteur.

## <a name="steps"></a>Étapes

Tout d’abord une source de données est obligatoire. Cet exemple utilise la base de données AdventureWorks et Microsoft SQL Server 2005 Express Edition. La base de données est une partie facultative d’une installation de Visual Studio (y compris l’édition expresse) et est également disponible en téléchargement séparé sous [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). La base de données AdventureWorks fait partie des exemples SQL Server 2005 et exemples de bases de données (téléchargement à l’adresse [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = fr](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Pour configurer la base de données le plus simple consiste à utiliser le Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = fr](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) et attacher le `AdventureWorks.mdf` fichier de base de données.

Pour cet exemple, nous partons du principe que l’instance de SQL Server 2005 Express Edition est appelée `SQLEXPRESS` et se trouve sur le même ordinateur que le serveur web ; il s’agit également de la configuration par défaut. Si votre programme d’installation est différente, vous devez adapter les informations de connexion pour la base de données.

Pour activer la fonctionnalité d’ASP.NET AJAX et les outils de contrôle, le `ScriptManager` contrôle doit être placé n’importe où sur la page (mais dans le `<form>` élément) :

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample1.aspx)]

Ensuite, ajoutez une source de données à la page. Pour pouvoir utiliser une quantité limitée de données, nous allons sélectionner uniquement les cinq premières entrées dans la table Vendor de la base de données AdventureWorks. Si vous utilisez l’assistant de Visual Studio pour créer la source de données, à l’esprit qu’un bogue dans la version actuelle ne précède pas le nom de table (`Vendor`) avec `Purchasing`. Le balisage suivant montre la syntaxe correcte :

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample2.aspx)]

Ensuite, ajoutez un panneau qui sert de la fenêtre contextuelle modale :

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample3.aspx)]

À présent, le `HoverMenuExtender` entre en jeu. Afin que tous les éléments de la source de données obtient sa propre fenêtre contextuelle, l’extendeur doit être placé dans le répéteur `<ItemTemplate>` section. Voici le code XAML :

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample4.aspx)]

Maintenant chaque élément dans la source de données affiche une fenêtre contextuelle à droite (`PopupPosition` attribut) après un délai de 50 millisecondes (`PopDelay` attribut).


[![Le menu sensitif qui s’affiche en regard de chaque élément dans le répéteur](using-hovermenu-with-a-repeater-control-cs/_static/image2.png)](using-hovermenu-with-a-repeater-control-cs/_static/image1.png)

Le menu sensitif qui s’affiche en regard de chaque élément dans le répéteur ([cliquez pour afficher l’image en taille réelle](using-hovermenu-with-a-repeater-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](using-hovermenu-with-a-repeater-control-vb.md)
