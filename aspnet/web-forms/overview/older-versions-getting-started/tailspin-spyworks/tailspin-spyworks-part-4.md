---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'Partie 4 : Liste des produits | Microsoft Docs'
author: JoeStagner
description: Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application Tailspin Spyworks. Partie 4 couvre la liste des produits avec le contrat de GridView...
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: ec349a2564a63fd950ca5f6a171e6ffd1199c28a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813071"
---
<a name="part-4-listing-products"></a>Partie 4 : Liste des produits
====================
par [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks montre comment extrêmement simple est de créer des applications puissantes et évolutives pour la plate-forme .NET. Il montre comment utiliser les nouvelles fonctionnalités dans ASP.NET 4 pour créer un magasin en ligne, y compris les achats, extraction et administration.
> 
> Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application Tailspin Spyworks. Partie 4 couvre la liste des produits avec le contrôle GridView.


## <a id="_Toc260221670"></a>  Liste des produits avec le contrôle GridView

Nous allons commencer l’implémentation de notre page ProductsList.aspx en « Cliquant avec le bouton droit sur » sur notre solution et en sélectionnant « Ajouter » et « Nouvel élément ».

![](tailspin-spyworks-part-4/_static/image1.jpg)

Choisissez « Formulaire à l’aide de Page maître Web » et entrez un nom de la page de ProductsList.aspx ».

Cliquez sur « Ajouter ».

![](tailspin-spyworks-part-4/_static/image2.jpg)

Ensuite, choisissez le dossier « Styles » où nous avons placé la page Site.Master et sélectionnez-le dans la fenêtre « Contenu du dossier ».

![](tailspin-spyworks-part-4/_static/image3.jpg)

Cliquez sur « Ok » pour créer la page.

Notre base de données est remplie avec les données de produit, comme indiqué ci-dessous.

![](tailspin-spyworks-part-4/_static/image4.jpg)

Après la création de notre page Nous allons utiliser à nouveau d’une Source de données d’entité pour accéder aux données de ce produit, mais dans ce cas, nous avons besoin sélectionner les entités Product et nous avons besoin limiter les éléments qui sont retournés à ceux de la catégorie sélectionnée.

Pour effectuer cette opération nous indiquerons contrôle EntityDataSource génération automatique de la clause WHERE, et nous indiquons le WhereParameter.

Vous vous rappellerez que lorsque nous avons créé les éléments de Menu dans notre « Menu de catégorie de produit » nous créée dynamiquement le lien en ajoutant le CatagoryID à la chaîne de requête pour chaque lien. Nous vous indique la Source de données d’entité de dériver le paramètre d’emplacement de ce paramètre de chaîne de requête.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

Ensuite, nous allons configurer le contrôle ListView pour afficher une liste de produits. Pour créer une expérience d’achat optimale, que nous allons compact plusieurs fonctionnalités concises dans chaque produit affiché dans notre ListVew.

- Le nom du produit sera un lien vers l’affichage des détails du produit.
- Le prix du produit s’affiche.
- Une image du produit s’affiche et nous allons sélectionner dynamiquement l’image à partir d’un répertoire d’images de catalogue dans notre application.
- Nous inclurons un lien pour ajouter immédiatement le produit spécifique pour le panier d’achat.

Voici le balisage de notre instance de contrôle ListView.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

Nous allons créer dynamiquement plusieurs liens pour chaque produit affiché.

En outre, avant de tester le propre nouvelle page Nous devons créer, la structure de répertoire pour le produit des images de catalogue comme suit.

![](tailspin-spyworks-part-4/_static/image1.png)

Une fois que nos images de produit sont accessibles, nous pouvons tester notre page de liste de produits.

![](tailspin-spyworks-part-4/_static/image5.jpg)

À partir de la page d’accueil du site, cliquez sur l’un des liens de liste de catégorie.

![](tailspin-spyworks-part-4/_static/image6.jpg)

Nous devons à présent implémenter la page ProductDetials.apsx et la fonctionnalité AddToCart.

Utilisez fichier -&gt;nouveau pour créer un nom de page ProductDetails.aspx à l’aide de la Page maître du site comme nous l’avons fait précédemment.

Nous allons utiliser à nouveau d’un contrôle EntityDataSource pour accéder à l’enregistrement de produit spécifique dans la base de données et nous allons utiliser un contrôle FormView d’ASP.NET pour afficher les données de produit comme suit.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

Ne vous inquiétez pas si la mise en forme semble un peu amusantes à vous. Le balisage ci-dessus laisse la place dans la disposition de l’affichage pour quelques fonctionnalités, nous allons implémenter par la suite.

Le panier représentera une logique plus complexe dans notre application. Pour commencer, utilisez fichier -&gt;nouveau pour créer une page appelée MyShoppingCart.aspx.

Notez que nous ne choisissons pas le nom ShoppingCart.aspx.

Notre base de données contient une table nommée « ShoppingCart ». Lorsque nous avons généré un Entity Data Model, une classe a été créée pour chaque table dans la base de données. Par conséquent, l’Entity Data Model généré une classe d’entité nommée « ShoppingCart ». Nous pouvons modifier le modèle afin que nous pourrions utiliser ce nom pour notre implémentation de panier d’achat ou l’étendre pour nos besoins, mais nous choisissons d’au lieu de cela pour simplement sélectionnez un nom qui permet d’éviter le conflit.

Il est également important de noter que nous allons créer un panier d’achat simple et en incorporant la logique de panier d’achat avec l’affichage de panier d’achat. Nous pouvons également choisir d’implémenter notre panier d’achat dans une couche métier totalement distincte.

> [!div class="step-by-step"]
> [Précédent](tailspin-spyworks-part-3.md)
> [Suivant](tailspin-spyworks-part-5.md)
