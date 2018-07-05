---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-vb
title: Utilisation de ModalPopup avec un contrôle Repeater (VB) | Microsoft Docs
author: wenz
description: Le contrôle ModalPopup dans AJAX Control Toolkit offre un moyen simple de créer une contextuelle modale à l’aide de moyens de côté client. Il est également possible d’utiliser cette contr...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0c8e74f1-b3ba-4ca9-a1c5-f5c4831a359a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 2854cc8fcc9845cb4a4f97cfaf2f3c79ba031a93
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384068"
---
<a name="using-modalpopup-with-a-repeater-control-vb"></a>Utilisation de ModalPopup avec un contrôle Repeater (VB)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2VB.pdf)

> Le contrôle ModalPopup dans AJAX Control Toolkit offre un moyen simple de créer une contextuelle modale à l’aide de moyens de côté client. Il est également possible d’utiliser ce contrôle dans un répéteur.


## <a name="overview"></a>Vue d'ensemble

Le contrôle ModalPopup dans AJAX Control Toolkit offre un moyen simple de créer une contextuelle modale à l’aide de moyens de côté client. Il est également possible d’utiliser ce contrôle dans un répéteur.

## <a name="steps"></a>Étapes

Tout d’abord une source de données est obligatoire. Cet exemple utilise la base de données AdventureWorks et Microsoft SQL Server 2005 Express Edition. La base de données est une partie facultative d’une installation de Visual Studio (y compris l’édition expresse) et est également disponible en téléchargement séparé sous [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). La base de données AdventureWorks fait partie des exemples SQL Server 2005 et exemples de bases de données (téléchargement à l’adresse [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = fr](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Pour configurer la base de données le plus simple consiste à utiliser le Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = fr](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) et attacher le `AdventureWorks.mdf` fichier de base de données. Pour cet exemple, nous partons du principe que l’instance de SQL Server 2005 Express Edition est appelée `SQLEXPRESS` et se trouve sur le même ordinateur que le serveur web ; il s’agit également de la configuration par défaut. Si votre programme d’installation est différente, vous devez adapter les informations de connexion pour la base de données. Pour activer la fonctionnalité d’ASP.NET AJAX et les outils de contrôle, le `ScriptManager` contrôle doit être placé n’importe où sur la page (mais dans le `<form>` élément) :

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample1.aspx)]

Ensuite, ajoutez une source de données à la page. Pour pouvoir utiliser une quantité limitée de données, nous allons sélectionner uniquement les cinq premières entrées dans la table Vendor de la base de données AdventureWorks. Si vous utilisez l’assistant de Visual Studio pour créer la source de données, à l’esprit qu’un bogue dans la version actuelle ne précède pas le nom de table (`Vendor`) avec `Purchasing`. Le balisage suivant montre la syntaxe correcte :

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample2.aspx)]

Ensuite, ajoutez un panneau qui sert de la fenêtre contextuelle modale. Il contient un `Button` contrôle pour fermer la fenêtre contextuelle à nouveau :

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample3.aspx)]

Afin d’optimiser la fenêtre contextuelle du Repeater le `ModalPopupExtender` contrôle doit être placé dans le `<ItemTemplate>` section du répéteur. Par conséquent, le panneau est en dehors de la répétition, mais l’extendeur à l’intérieur. Voici le balisage pour le contrôle repeater :

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample4.aspx)]

Ensuite, chaque élément dans la source de données s’affiche avec un bouton en regard de celui-ci qui déclenche la fenêtre contextuelle modale.


[![La fenêtre contextuelle modale peut être déclenchée pour chaque entrée de source de données](using-modalpopup-with-a-repeater-control-vb/_static/image2.png)](using-modalpopup-with-a-repeater-control-vb/_static/image1.png)

La fenêtre contextuelle modale peut être déclenchée pour chaque entrée de source de données ([cliquez pour afficher l’image en taille réelle](using-modalpopup-with-a-repeater-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](launching-a-modal-popup-window-from-server-code-vb.md)
> [Suivant](handling-postbacks-from-a-modalpopup-vb.md)
