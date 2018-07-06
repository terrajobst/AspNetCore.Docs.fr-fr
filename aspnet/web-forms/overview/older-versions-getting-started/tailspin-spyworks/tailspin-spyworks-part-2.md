---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: 'Partie 2 : Couche d’accès aux données | Microsoft Docs'
author: JoeStagner
description: Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application Tailspin Spyworks. Partie 2 couvre l’ajout de la couche d’accès aux données.
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: c1d61681952cbf1f0587f0e42da21f8436d79ebb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812030"
---
<a name="part-2-data-access-layer"></a>Partie 2 : Couche d’accès aux données
====================
par [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks montre comment extrêmement simple est de créer des applications puissantes et évolutives pour la plate-forme .NET. Il montre comment utiliser les nouvelles fonctionnalités dans ASP.NET 4 pour créer un magasin en ligne, y compris les achats, extraction et administration.
> 
> Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application Tailspin Spyworks. Partie 2 couvre l’ajout de la couche d’accès aux données.


## <a id="_Toc260221668"></a>  Ajout de la couche d’accès aux données

Notre application de commerce électronique dépend de deux bases de données.

Pour plus d’informations client, nous allons utiliser la base de données d’appartenance ASP.NET standard. Pour notre catalogue de produit et de panier d’achat nous allons implémenter une base de données SQL Express comme suit.

![](tailspin-spyworks-part-2/_static/image1.jpg)

Après avoir créé la base de données (Commerce.mdf) dans application l’application\_dossier de données que nous pouvons passer à la création de notre couche d’accès aux données à l’aide de l’entité de .NET Framework.

Nous allons créer un dossier nommé « données\_accès » et cliquez avec le bouton droit sur ce dossier et sélectionnez « Ajouter un nouvel élément ».

Dans « Modèles installés » élément et puis sélectionnez « ADO.NET Entity Data Model » Entrez EDM\_Commerce.edmx comme nom et cliquez sur le bouton « Ajouter ».

![](tailspin-spyworks-part-2/_static/image2.jpg)

Choisissez « Générer à partir de la base de données ».

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

Enregistrez et générez.

Nous sommes désormais prêts à ajouter notre première fonctionnalité – un menu de catégorie de produit.

> [!div class="step-by-step"]
> [Précédent](tailspin-spyworks-part-1.md)
> [Suivant](tailspin-spyworks-part-3.md)
