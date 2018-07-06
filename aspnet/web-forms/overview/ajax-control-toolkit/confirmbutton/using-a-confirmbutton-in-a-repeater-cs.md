---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-cs
title: Utilisation d’un ConfirmButton dans un répéteur (c#) | Microsoft Docs
author: wenz
description: L’extendeur ConfirmButton dans AJAX Control Toolkit crée un Oui/pas de fenêtre contextuelle lorsque l’utilisateur clique sur un bouton (y compris contrôle LinkButton). Uniquement si Oui est...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: a973ed3e-400c-4925-ace2-0b086b479301
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-cs
msc.type: authoredcontent
ms.openlocfilehash: fd0cdde5db44d1437000008f1fb5e7b13f0ca77a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805234"
---
<a name="using-a-confirmbutton-in-a-repeater-c"></a>Utilisation d’un ConfirmButton dans un répéteur (c#)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1CS.pdf)

> L’extendeur ConfirmButton dans AJAX Control Toolkit crée un Oui/pas de fenêtre contextuelle lorsque l’utilisateur clique sur un bouton (y compris contrôle LinkButton). Uniquement si l’utilisateur est cliqué sur Oui, l’action du bouton est exécutée, sinon annulée. Cela est également possible dans un répéteur.


## <a name="overview"></a>Vue d'ensemble

L’extendeur ConfirmButton dans AJAX Control Toolkit crée un Oui/pas de fenêtre contextuelle lorsque l’utilisateur clique sur un bouton (y compris contrôle LinkButton). Uniquement si l’utilisateur est cliqué sur Oui, l’action du bouton est exécutée, sinon annulée. Cela est également possible dans un répéteur.

## <a name="steps"></a>Étapes

Tout d’abord une source de données est obligatoire. Cet exemple utilise la base de données AdventureWorks et Microsoft SQL Server 2005 Express Edition. La base de données est une partie facultative d’une installation de Visual Studio (y compris l’édition expresse) et est également disponible en téléchargement séparé sous [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). La base de données AdventureWorks fait partie des exemples SQL Server 2005 et exemples de bases de données (téléchargement à l’adresse [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = fr](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Pour configurer la base de données le plus simple consiste à utiliser le Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = fr](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) et attacher le `AdventureWorks.mdf` fichier de base de données.

Pour cet exemple, nous partons du principe que l’instance de SQL Server 2005 Express Edition est appelée `SQLEXPRESS` et se trouve sur le même ordinateur que le serveur web ; il s’agit également de la configuration par défaut. Si votre programme d’installation est différente, vous devez adapter les informations de connexion pour la base de données.

Pour activer la fonctionnalité d’ASP.NET AJAX et les outils de contrôle, le `ScriptManager` contrôle doit être placé n’importe où sur la page (mais dans le `<form>` élément) :

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample1.aspx)]

Ensuite, une source de données est obligatoire. Par souci de simplicité, uniquement les cinq premières entrées dans la table de fournisseurs AdventureWorks sont récupérées. Notez que lorsque vous utilisez l’Assistant de Visual Studio pour créer la source de données, le nom de table (`Vendors`) est actuellement pas correctement préfixés avec `Purchasing`. Le balisage suivant est celui qui convient :

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample2.aspx)]

Cette source de données peut ensuite être utilisée dans un répéteur. Comme d’habitude, le `DataBinder.Eval()` méthode récupère les données à partir de la source de données. Le `ConfirmButtonExtender` contrôle puis doit être placé dans le `<ItemTemplate>` section du répéteur afin qu’elle s’affiche pour chaque entrée de la source de données.

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample3.aspx)]


[![Le bouton de confirmation s’affiche en regard de chaque entrée à partir de la source de données](using-a-confirmbutton-in-a-repeater-cs/_static/image2.png)](using-a-confirmbutton-in-a-repeater-cs/_static/image1.png)

Le bouton de confirmation s’affiche en regard de chaque entrée à partir de la source de données ([cliquez pour afficher l’image en taille réelle](using-a-confirmbutton-in-a-repeater-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](using-a-confirmbutton-in-a-repeater-vb.md)
