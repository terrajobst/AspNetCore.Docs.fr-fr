---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 'Partie 3 : Mise en page et Menu catégorie | Microsoft Docs'
author: JoeStagner
description: Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application Tailspin Spyworks. La partie 3 explique Ajout de mise en page et un menu de la catégorie.
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: 3047c53e21a418aef8617bd772a247bc46edb98f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817523"
---
<a name="part-3-layout-and-category-menu"></a>Partie 3 : Mise en page et Menu catégorie
====================
par [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks montre comment extrêmement simple est de créer des applications puissantes et évolutives pour la plate-forme .NET. Il montre comment utiliser les nouvelles fonctionnalités dans ASP.NET 4 pour créer un magasin en ligne, y compris les achats, extraction et administration.
> 
> Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application Tailspin Spyworks. La partie 3 explique Ajout de mise en page et un menu de la catégorie.


## <a id="_Toc260221669"></a>  Ajout d’une mise en page et un Menu catégorie

Dans notre page maître du site, nous allons ajouter une balise div pour la colonne de gauche qui contient notre menu catégorie de produit.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

Notez que l’alignement souhaité et autres mises en forme seront fournies par la classe CSS que nous avons ajouté à notre fichier Style.css.

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

Le menu de catégorie de produit sera être créé dynamiquement lors de l’exécution en interrogeant la base de données de Commerce pour existant catégories de produits et à créer les éléments de menu et correspondant lie.

Pour ce faire, nous allons utiliser deux des ASP. Contrôles de données puissantes de NET. Le contrôle de « Source de données d’entité » et le contrôle « ListView ».

![](tailspin-spyworks-part-3/_static/image1.jpg)

Nous allons passer en « Mode Design » et utiliser les programmes d’assistance pour configurer nos contrôles.

![](tailspin-spyworks-part-3/_static/image2.jpg)

Nous allons définir la propriété d’ID de contrôle EntityDataSource à EDS\_catégorie\_Menu et cliquez sur « Configurer la Source de données ».

![](tailspin-spyworks-part-3/_static/image3.jpg)

Sélectionnez la connexion CommerceEntities qui a été créé pour nous lorsque nous avons créé le modèle de Source de données Entity pour notre base de données de Commerce, puis cliquez sur « Suivant ».

![](tailspin-spyworks-part-3/_static/image4.jpg)

Sélectionnez le nom de jeu d’entités de « Catégories » et laisser le reste des options comme valeur par défaut. Cliquez sur « Terminer ».

Maintenant nous allons définir la propriété d’ID de l’instance de contrôle ListView que nous avons placé sur notre page de ListView\_ProductsMenu et activer ses d’assistance.

![](tailspin-spyworks-part-3/_static/image5.jpg)

Bien que nous pourrions utiliser des options de contrôle pour mettre en forme l’affichage d’élément de données et mise en forme, notre la création du menu uniquement nécessiteront un balisage simple nous sera de saisir le code dans la vue de source.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

Notez l’instruction « Eval » : &lt;% # Eval("CategoryName") %&gt;

La syntaxe ASP.NET &lt;% # %&gt; est une convention de raccourci qui indique à l’exécution pour exécuter tout ce qui est contenu dans et générer les résultats « en ligne ».

L’instruction Eval("CategoryName") indique que, pour l’entrée actuelle dans la collection liée d’éléments de données, extraire la valeur de noms d’élément de modèle d’entité « CatagoryName ». Il s’agit d’une syntaxe concise pour une fonctionnalité très puissante.

Permet d’exécuter l’application maintenant.

![](tailspin-spyworks-part-3/_static/image6.jpg)

Notez que notre menu catégorie de produit s’affiche désormais lorsque nous déplacez le pointeur sur les catégorie d’éléments de menu nous pouvons voir les points de lien d’élément menu à une page, nous devons encore implémenter nommé ProductsList.aspx et que nous avons créé un argument de chaîne de requête dynamique qui contient le  id de catégorie.

> [!div class="step-by-step"]
> [Précédent](tailspin-spyworks-part-2.md)
> [Suivant](tailspin-spyworks-part-4.md)
