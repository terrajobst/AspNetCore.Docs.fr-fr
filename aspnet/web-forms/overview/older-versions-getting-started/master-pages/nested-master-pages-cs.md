---
uid: web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-cs
title: Imbriqué des Pages maîtres (c#) | Microsoft Docs
author: rick-anderson
description: Montre comment imbriquer une page maître dans une autre.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/28/2008
ms.topic: article
ms.assetid: 32b7fb6e-d74b-4048-91f8-70631b2523ee
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 1a93f8a0dc829f45eaa08a356f01f51df5443e29
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379071"
---
<a name="nested-master-pages-c"></a>Pages maîtres imbriquées (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_CS.zip) ou [télécharger le PDF](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_CS.pdf)

> Montre comment imbriquer une page maître dans une autre.


## <a name="introduction"></a>Introduction

Au cours des neuf derniers didacticiels, nous avons vu comment implémenter une mise en page de l’échelle du site avec les pages maîtres. En bref, les pages maîtres nous permettent, le développeur de pages, pour définir le balisage courantes dans la page maître, ainsi que des régions spécifiques qui peuvent être personnalisées sur une base de la page de contenu-par-contenu de la page. Les contrôles ContentPlaceHolder dans une page maître indiquent les régions personnalisables ; le code personnalisé pour les contrôles ContentPlaceHolder sont définies dans la page de contenu par le biais de contrôles de contenu.

Les techniques de page maître que nous avons exploré jusqu'à présent sont idéales si vous avez une disposition unique utilisée sur l’ensemble du site. Toutefois, de nombreux sites Web de grande taille ont une disposition de site personnalisé entre les différentes sections. Par exemple, considérez une application de soins de santé utilisée par le personnel de l’hôpital pour gérer la facturation, les activités et les informations relatives aux patients. Il peut y avoir trois types de pages web dans cette application :

- Personnel membre spécifiques où les membres du personnel peuvent mettre à jour la disponibilité, afficher les planifications, ou demander des congés.
- Pages de patient spécifiques où les membres du personnel permet d’afficher ou modifier les informations d’un patient spécifique.
- Pages de facturation spécifique où comptables examiner actuel revendication d’états et les rapports financiers.

Chaque page susceptibles de partager une disposition commune, tel qu’un menu sur le haut et une série de liens fréquemment utilisées dans la partie inférieure. Mais le personnel-, patient-et les pages de facturation spécifiques peuvent avoir besoin personnaliser cette disposition générique. Par exemple, peut-être toutes les pages du personnel spécifique doivent inclure une liste de calendrier et des tâches montrant la disponibilité et la planification quotidienne de l’utilisateur actuellement connecté. Peut-être toutes les pages spécifiques à un patient besoin d’afficher le nom, adresse et les informations d’assurance pour le patient dont les informations sont en cours de modification.

Il est possible de créer de telles dispositions personnalisées à l’aide de *des pages maîtres imbriquées*. Pour implémenter le scénario ci-dessus, nous démarre en créant une page maître qui a défini le contenu de mise en page, le menu et le pied de page à l’échelle du site, avec ContentPlaceHolders définissant les zones personnalisables. Nous créerions ensuite trois pages maîtres imbriquées, une pour chaque type de page web. Chaque page maître imbriquée définirait le contenu entre le type des pages de contenu qui utilisent la page maître. En d’autres termes, la page maître imbriquée pour les pages de contenu spécifique d’un patient inclurait balisage et la logique de programmation pour l’affichage d’informations sur le patient en cours de modification. Lors de la création d’une page spécifique d’un patient nous serait le lier à cette page maître imbriquée.

Ce didacticiel commence par la mise en évidence les avantages des pages maîtres imbriquées. Il montre ensuite comment créer et utiliser les pages maîtres imbriquées.

> [!NOTE]
> Les pages maîtres imbriquées ont été possibles depuis la version 2.0 du .NET Framework. Toutefois, Visual Studio 2005 n’incluent pas de prise en charge au moment du design pour les pages maîtres imbriquées. La bonne nouvelle est que Visual Studio 2008 offre une riche expérience au moment du design pour les pages maîtres imbriquées. Si vous souhaitez utiliser les pages maîtres imbriquées mais que vous utilisez Visual Studio 2005, consultez [Scott Guthrie](https://weblogs.asp.net/scottgu/)d’entrée de blog, [astuces pour les Pages maîtres imbriquées dans VS 2005 conception](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx).


## <a name="the-benefits-of-nested-master-pages"></a>Les avantages des Pages maîtres imbriquées

De nombreux sites Web ont une conception de site principal, mais aussi plus des conceptions personnalisées spécifiques à certains types de pages. Par exemple, dans notre application web de démonstration, nous avons créé une section d’Administration rudimentaire (les pages dans le `~/Admin` dossier). Actuellement les pages web dans le `~/Admin` dossier utilisent la même page maître en tant que ces pages pas dans la section administration (à savoir, `Site.master` ou `Alternate.master`, en fonction de sélection de l’utilisateur).

> [!NOTE]
> Pour l’instant, supposons que notre site a qu’une seule page maître, `Site.master`. Nous aborderons à l’aide de pages maîtres imbriquées avec les pages maîtres de deux (ou plus) en commençant par « À l’aide un imbriquée Master pour l’Administration Section de la Page » plus loin dans ce didacticiel.


Imaginez que nous avons invités à personnaliser la disposition des pages d’Administration pour inclure des informations supplémentaires ou des liens qui sinon ne serait pas présents dans les autres pages du site. Il existe quatre techniques pour implémenter cette exigence :

1. Ajouter manuellement les informations spécifiques à l’Administration et les liens à chaque page de contenu dans le `~/Admin` dossier.
2. Mise à jour le `Site.master` page maître à inclure les informations spécifiques à la section d’Administration et les liens, puis ajoutez le code à la page maître pour afficher ou masquer ces sections selon si une des pages d’Administration est visitée.
3. Créer une page maître spécifiquement pour la section Administration, recopiez les balises de `Site.master`, ajoutez les informations spécifiques à la section d’Administration et les liens, puis mettez à jour les pages de contenu dans le `~/Admin` dossier à utiliser ce nouveau maître page.
4. Créer une page maître imbriquée qui lie à `Site.master` et avoir les pages de contenu dans le `~/Admin` cette nouvelle utilisation des dossiers imbriqués page maître. Cette page maître imbriquée inclurait simplement les informations et liens supplémentaires spécifiques aux pages d’Administration et qu’il serait inutile de répéter le balisage prédéfinie dans `Site.master`.

La première option est moins aisée. L’objectif de l’utilisation des pages maîtres est à s’éloigner de devoir copier et coller le balisage courants pour les nouvelles pages ASP.NET manuellement. La deuxième option est acceptable, mais rend la moins facile à gérer comme il épaissit les pages maîtres avec le balisage qui s’affiche qu’occasionnellement et nécessite que les développeurs modifiez la page maître pour contourner ce balisage et d’avoir à mémoriser, de l’application exactement, certaine balisage s’affiche et lorsqu’il est masqué. Cette approche serait moins tenable en tant que personnalisations à partir de types plus et plus de pages web nécessaires pour être prise en charge par cette page maître unique.

La troisième option supprime l’encombrement et la complexité émet les remontées avec la deuxième option. Toutefois, option de trois principal inconvénient est qu’il nous oblige à copier et coller la mise en page courantes à partir de `Site.master` vers la nouvelle page maître de spécifique à la section d’Administration. Si vous décidez plus tard modifier la disposition de l’échelle du site que nous devons penser à modifier dans deux emplacements.

La quatrième option, des pages maîtres imbriquées, nous permettent de profiter des deuxième et troisième options. Les informations de disposition de l’échelle du site sont conservées dans un fichier : la page maître de niveau supérieur - alors que le contenu spécifique de régions particulières est réparti dans différents fichiers.

Ce didacticiel commence par un coup de œil à la création et l’utilisation d’une page maître imbriquée simple. Nous créons une toute nouvelle page maître de niveau supérieur, les deux pages maîtres imbriquées et les deux pages de contenu. À partir de « À l’aide un imbriqués Master Page pour la Section Administration », nous allons examiner la mise à jour de notre architecture existante de la page maître pour inclure l’utilisation des pages maîtres imbriquées. Plus précisément, nous créer une page maître imbriquée et utilisez-le pour inclure du contenu personnalisé supplémentaire pour les pages de contenu dans le `~/Admin` dossier.

## <a name="step-1-creating-a-simple-top-level-master-page"></a>Étape 1 : Création d’une Page maître de niveau supérieur Simple

Création d’un serveur maître imbriqué à partir de la principale existante pages et ensuite la mise à jour une page de contenu existante pour utiliser cette nouvelle page maître imbriquée au lieu de la page maître de niveau supérieur qu’il comporte peu de complexité, car les pages de contenu existants attendent déjà certains Contrôles ContentPlaceHolder définis dans la page maître de niveau supérieur. Par conséquent, la page maître imbriquée doit également inclure les mêmes contrôles ContentPlaceHolder portant le même nom. En outre, notre application de démonstration particulier a deux pages maîtres (`Site.master` et `Alternate.master`) qui sont affectées dynamiquement à une page de contenu en fonction des préférences de l’utilisateur, qui ajoute davantage à cette complexité. Nous allons examiner la mise à jour de l’application existante à utiliser les pages maîtres imbriquées plus loin dans ce didacticiel, mais nous allons concentrer tout d’abord sur une simple imbriqués exemple des pages maîtres.

Créez un dossier nommé `NestedMasterPages` puis ajoutez un nouveau fichier de page maître dans ce dossier nommé `Simple.master`. (Voir Figure 1 pour une capture d’écran de l’Explorateur de solutions après l’ajout de ce dossier et le fichier.) Faites glisser le `AlternateStyles.css` fichier de feuille de style à partir de l’Explorateur de solutions sur le concepteur. Cette opération ajoute une `<link>` élément vers le fichier de feuille de style dans le `<head>` élément, après laquelle la page maître `<head>` balisage de l’élément doit ressembler à :


[!code-aspx[Main](nested-master-pages-cs/samples/sample1.aspx)]

Ensuite, ajoutez le balisage suivant dans le formulaire Web de `Simple.master`:


[!code-aspx[Main](nested-master-pages-cs/samples/sample2.aspx)]

Ce balisage affiche un lien intitulé « Pages maîtres imbriquées (Simple) » en haut de la page dans une grande taille de police blanche sur fond bleu foncé. Sous qui est le `MainContent` ContentPlaceHolder. La figure 1 illustre le `Simple.master` page maître lors du chargement dans le concepteur Visual Studio.


[![La Page maître imbriquée définit le contenu spécifique aux Pages dans la Section Administration](nested-master-pages-cs/_static/image2.png)](nested-master-pages-cs/_static/image1.png)

**Figure 01**: The imbriqués Master Page définit contenu spécifique aux Pages dans la Section Administration ([cliquez pour afficher l’image en taille réelle](nested-master-pages-cs/_static/image3.png))


## <a name="step-2-creating-a-simple-nested-master-page"></a>Étape 2 : Création d’une Page maître imbriquée Simple

`Simple.master` contient deux contrôles ContentPlaceHolder : le `MainContent` ContentPlaceHolder, nous avons ajouté dans un formulaire Web avec le `head` ContentPlaceHolder dans le `<head>` élément. Si vous deviez créer une page de contenu, puis liez-le à `Simple.master` la page de contenu aurait deux contrôles de contenu faisant référence aux deux ContentPlaceHolders. De même, si nous créer une page maître imbriquée et liez-le à `Simple.master` puis la page maître imbriquée aura deux contrôles de contenu.

Nous allons ajouter une nouvelle page maître imbriquée à la `NestedMasterPages` dossier nommé `SimpleNested.master`. Avec le bouton droit sur le `NestedMasterPages` dossier et choisissez Ajouter un nouvel élément. Ceci fait apparaître la boîte de dialogue Ajouter un nouvel élément indiquée dans la Figure 2. Sélectionnez le type de modèle de Page maître et tapez le nom de la nouvelle page maître. Pour indiquer que la nouvelle page maître doit être une page maître imbriquée, cochez la case à cocher « Sélectionner la page maître ».

Ensuite, cliquez sur le bouton Ajouter. Ceci affichera le même, sélectionnez une boîte de dialogue de Page maître vous voyez lors de la liaison d’une page de contenu à une page maître (voir Figure 3). Choisissez le `Simple.master` page maître dans le `NestedMasterPages` dossier et cliquez sur OK.

> [!NOTE]
> Si vous avez créé votre site Web ASP.NET à l’aide du modèle de projet d’Application Web au lieu du modèle de projet de Site Web, vous ne verrez pas la case à cocher « Sélectionner la page maître » dans la boîte de dialogue Ajouter un nouvel élément indiquée dans la Figure 2. Pour créer une page maître imbriquée lorsque vous utilisez le modèle de projet d’Application Web, vous devez choisir le modèle de Page maître imbriquée (au lieu du modèle de Page maître). Après avoir sélectionné le modèle de Page maître imbriquée et en cliquant sur Ajouter, le même sélectionner une Page maître boîte de dialogue illustrée dans la Figure 3 s’affiche.


[![Vérifier le &quot;sélectionner la page maître&quot; case à cocher pour ajouter une Page maître imbriquée](nested-master-pages-cs/_static/image5.png)](nested-master-pages-cs/_static/image4.png)

**Figure 02**: cochez la case « Sélectionner la page maître » pour ajouter une Page maître imbriquée ([cliquez pour afficher l’image en taille réelle](nested-master-pages-cs/_static/image6.png))


[![Lier la Page maître imbriquée à la Page maître de Simple.master](nested-master-pages-cs/_static/image8.png)](nested-master-pages-cs/_static/image7.png)

**Figure 03**: lier la Page maître imbriquée à la `Simple.master` Page maître ([cliquez pour afficher l’image en taille réelle](nested-master-pages-cs/_static/image9.png))


Balisage déclaratif de la page maître imbriquée, illustré ci-dessous, contient deux contrôles de contenu faisant référence à des contrôles ContentPlaceHolder deux le niveau supérieur de la page maître.


[!code-aspx[Main](nested-master-pages-cs/samples/sample3.aspx)]

À l’exception de la `<%@ Master %>` directive, balisage déclaratif initiale de la page maître imbriquée est identique à la balise qui est initialement générée lors de la liaison d’une page de contenu à la même page maître de niveau supérieur. Comme une page de contenu `<%@ Page %>` directive, le `<%@ Master %>` directive ici inclut un `MasterPageFile` attribut qui spécifie la page maître du parent de la page maître imbriquée. La principale différence entre la page maître imbriquée et une page de contenu lié à la même page maître de niveau supérieur est que la page maître imbriquée peut inclure des contrôles ContentPlaceHolder. Les contrôles ContentPlaceHolder de la page maître imbriquée définissent les régions où les pages de contenu peuvent personnaliser le balisage.

Mettre à jour cette page maître imbriquée afin qu’il affiche le texte « Hello, from SimpleNested ! » dans le contrôle de contenu qui correspond à la `MainContent` contrôle ContentPlaceHolder.


[!code-aspx[Main](nested-master-pages-cs/samples/sample4.aspx)]

Après avoir apporté cet ajout, enregistrez la page maître imbriquée et puis ajoutez une nouvelle page de contenu pour le `NestedMasterPages` dossier nommé `Default.aspx`, puis liez-le à le `SimpleNested.master` page maître. Lors de l’ajout de cette page, vous pouvez être surpris de voir qu’elle contient sans contrôles de contenu (voir Figure 4) ! Une page de contenu peut accéder uniquement à son *parent* maître ContentPlaceHolders de la page. `SimpleNested.master` ne contient pas tous les contrôles ContentPlaceHolder ; Par conséquent, n’importe quelle page de contenu liée à cette page maître ne peut pas contenir les contrôles de contenu.


[![La nouvelle Page de contenu ne contient aucun contrôle de contenu](nested-master-pages-cs/_static/image11.png)](nested-master-pages-cs/_static/image10.png)

**Figure 04**: la nouvelle Page contient aucun contenu contrôles de contenu ([cliquez pour afficher l’image en taille réelle](nested-master-pages-cs/_static/image12.png))


Ce que nous devons faire est de mettre à jour de la page maître imbriquée (`SimpleNested.master`) pour inclure les contrôles ContentPlaceHolder. En général, vous allez que vos pages maîtres imbriquées pour inclure un ContentPlaceHolder pour chaque ContentPlaceHolder défini par sa page maître parente, ce qui permet sa page maître enfant ou une page de contenu pour travailler avec du niveau supérieur de la page maître ContentPlaceHolder contrôles.

Mise à jour le `SimpleNested.master` page maître pour inclure un ContentPlaceHolder dans ses deux contrôles de contenu. Nommez les contrôles ContentPlaceHolder le même que le contrôle ContentPlaceHolder à que leur contrôle de contenu fait référence. Autrement dit, ajoutez un contrôle ContentPlaceHolder nommé `MainContent` pour le contenu de contrôle dans `SimpleNested.master` qui fait référence à la `MainContent` ContentPlaceHolder dans `Simple.master`. Faire la même chose dans le contrôle de contenu qui référence le `head` ContentPlaceHolder.

> [!NOTE]
> Bien que je recommande d’affectation de noms les contrôles ContentPlaceHolder dans la page maître imbriquée identique aux ContentPlaceHolders dans la page maître de niveau supérieur, cette symétrie d’affectation de noms n’est pas nécessaire. Vous pouvez donner les contrôles ContentPlaceHolder dans votre page maître imbriquée de n’importe quel nom de que votre choix. Toutefois, j’ai s’avérer plus facile à retenir que ContentPlaceHolders correspondent avec les zones de la page si ma page maître de niveau supérieur et les pages maîtres imbriquées utilisent les mêmes noms.


Après avoir apporté ces ajouts votre `SimpleNested.master` balisage déclaratif de la page maître doit ressembler à ce qui suit :


[!code-aspx[Main](nested-master-pages-cs/samples/sample5.aspx)]

Supprimer le `Default.aspx` page que nous venons de créer de contenu, puis ajouter de nouveau, en le liant à le `SimpleNested.master` page maître. Cette fois Visual Studio ajoute deux contrôles de contenu pour le `Default.aspx`, référençant le ContentPlaceHolders maintenant définie dans `SimpleNested.master` (voir Figure 6). Ajoutez le texte, « Hello, from Default.aspx ! » dans le contenu du contrôle référencé `MainContent`.

La figure 5 illustre les trois entités impliquées dans ce cas - `Simple.master`, `SimpleNested.master`, et `Default.aspx` - et leurs relations avec les uns des autres. Comme le montre le diagramme, la page maître imbriquée implémente des contrôles de contenu pour ContentPlaceHolder de son parent. Si ces régions doivent être accessibles à la page de contenu, la page maître imbriquée doit ajouter son propre ContentPlaceHolders aux contrôles de contenu.


[![Les Pages maîtres de niveau supérieur et imbriqués dictent le contenu mise en Page](nested-master-pages-cs/_static/image14.png)](nested-master-pages-cs/_static/image13.png)

**Figure 05**: les Pages maîtres imbriquées et de niveau supérieur dictent le mise contenu en Page ([cliquez pour afficher l’image en taille réelle](nested-master-pages-cs/_static/image15.png))


Ce comportement illustre comment une page de contenu ou la page maître est uniquement informé de sa page maître parente. Ce comportement est également indiqué par le concepteur Visual Studio. La figure 6 illustre le concepteur pour `Default.aspx`. Bien que le concepteur affiche clairement quelles régions sont modifiables à partir de la page de contenu et les parties ne sont pas, il ne lever l’ambiguïté quelles sont les zones non modifiables de la page maître imbriquée et quelles sont les régions à partir de la page maître de niveau supérieur.


[![Le contenu maintenant de Page inclut des contrôles de contenu pour ContentPlaceHolders la Page maître imbriquée](nested-master-pages-cs/_static/image17.png)](nested-master-pages-cs/_static/image16.png)

**Figure 06**: le contenu Page maintenant inclut des contrôles de contenu pour ContentPlaceHolders de la Page maître imbriquée ([cliquez pour afficher l’image en taille réelle](nested-master-pages-cs/_static/image18.png))


## <a name="step-3-adding-a-second-simple-nested-master-page"></a>Étape 3 : Ajout d’une deuxième Page maître imbriquée Simple

L’avantage de pages maîtres imbriquées est plus évident lorsqu’il y a plusieurs pages maîtres imbriquées. Pour illustrer cet avantage, créez une autre page maître imbriquée dans la `NestedMasterPages` dossier ; page maître nommez cette nouvelle imbriqué `SimpleNestedAlternate.master` et liez-le à le `Simple.master` page maître. Ajout de contrôles ContentPlaceHolder dans deux contrôles de contenu de la page maître imbriquée, comme nous l’avons fait à l’étape 2. Également ajouter le texte, « Hello, from SimpleNestedAlternate ! » dans le contrôle de contenu qui correspond à la page maître niveau supérieur `MainContent` ContentPlaceHolder. Après avoir apporté ces modifications balisage déclaratif de votre nouvelle page maître imbriquée doit ressembler à ce qui suit :


[!code-aspx[Main](nested-master-pages-cs/samples/sample6.aspx)]

Créer une page de contenu nommée `Alternate.aspx` dans le `NestedMasterPages` dossier et liez-le à le `SimpleNestedAlternate.master` page maître imbriquée. Ajoutez le texte, « Hello, émis par d’autres ! » dans le contrôle de contenu qui correspond à `MainContent`. La figure 7 illustre `Alternate.aspx` lorsqu’ils sont affichés par le biais du concepteur Visual Studio.


[![Alternate.aspx est lié à la Page maître de SimpleNestedAlternate.master](nested-master-pages-cs/_static/image20.png)](nested-master-pages-cs/_static/image19.png)

**Figure 07**: `Alternate.aspx` est lié à la `SimpleNestedAlternate.master` Page maître ([cliquez pour afficher l’image en taille réelle](nested-master-pages-cs/_static/image21.png))


Comparez le concepteur dans la Figure 7 vers le concepteur dans la Figure 6. Les deux pages de contenu partagent la même disposition définie dans la page maître de niveau supérieur (`Simple.master`), à savoir le titre « Nested Master Pages didacticiel (Simple) ». Les deux ont encore contenu distinct défini dans leurs pages maîtres parent - le texte « Hello, from SimpleNested ! » dans la Figure 6 et « Hello, from SimpleNestedAlternate ! » dans la Figure 7. Certes, sont simples à comprendre ces différences ici, mais vous pouvez étendre cet exemple pour inclure des différences plus significatives. Par exemple, le `SimpleNested.master` page peut inclure un menu avec des options spécifiques à ses pages de contenu, tandis que `SimpleNestedAlternate.master` peut-être des informations pertinentes sur les pages de contenu qui lui sont liés.

Imaginons maintenant que nous avions besoin apporter une modification à la disposition du site global. Par exemple, imaginez que nous voulions ajouter une liste de liens communes à toutes les pages de contenu. Pour cela nous mettons à jour la page maître de niveau supérieur, `Simple.master`. Les modifications sont immédiatement répercutées dans ses pages maîtres imbriquées et, par extension, leurs pages de contenu.

Pour illustrer la facilité avec laquelle nous pouvons modifier la disposition de site principal, ouvrez le `Simple.master` page maître et ajoutez le balisage suivant entre la `topContent` et `mainContent` `<div>` éléments :


[!code-aspx[Main](nested-master-pages-cs/samples/sample7.aspx)]

Cela ajoute deux liens en haut de chaque page qui lie à `Simple.master`, `SimpleNested.master`, ou `SimpleNestedAlternate.master`; ces modifications s’appliquent immédiatement à des pages maîtres imbriqués et leurs pages de contenu. La figure 8 illustre `Alternate.aspx` lorsqu’ils sont affichés via un navigateur. Notez l’ajout des liens en haut de la page (par rapport à la Figure 7).


[![Passé à la Page maître de niveau supérieur sont immédiatement répercutées dans son imbriqués des Pages maîtres et de leurs Pages de contenu](nested-master-pages-cs/_static/image23.png)](nested-master-pages-cs/_static/image22.png)

**Figure 08**: modifié à la Page maître de niveau supérieur sont immédiatement répercutées dans son imbriqués des Pages maîtres et de leurs Pages de contenu ([cliquez pour afficher l’image en taille réelle](nested-master-pages-cs/_static/image24.png))


## <a name="using-a-nested-master-page-for-the-administration-section"></a>À l’aide d’une Page maître imbriquée pour la Section Administration

À ce stade nous avoir examiné les avantages du maître imbriquée des pages et que vous avez vu comment créer et les utiliser dans une application ASP.NET. Les exemples dans les étapes 1, 2 et 3, était cependant, la création d’une nouvelle page maître de niveau supérieur, les nouvelles pages maîtres imbriquées et les nouvelles pages de contenu. Nous allons ajouter une nouvelle page maître imbriquée à un site Web avec une page maître niveau supérieur et les pages de contenu ?

L’intégration d’une page maître imbriquée dans un site Web existant et en l’associant avec des pages de contenu existants nécessitent un peu plus d’efforts qu’en partant de zéro. Les étapes 4, 5, 6 et 7 Explorer ces défis, comme nous améliorer notre application de démonstration pour inclure une page maître imbriquée nommée `AdminNested.master` qui contient des instructions pour l’administrateur et est utilisé par les pages ASP.NET dans le `~/Admin` dossier.

L’intégration d’une page maître imbriquée dans notre application de démonstration présente les problèmes suivants :

- Le contenu existant des pages dans le `~/Admin` dossier avoir certaines attentes à partir de leur page maître. Pour commencer, ils s’attendent certains contrôles ContentPlaceHolder à être présent. En outre, le `~/Admin/AddProduct.aspx` et `~/Admin/Products.aspx` pages appellent public de la page maître `RefreshRecentProductsGrid` (méthode), définissez son `GridMessageText` propriété, ou avoir un gestionnaire d’événements pour son `PricesDoubled` événement. Par conséquent, notre page maître imbriquée doit fournir le mêmes ContentPlaceHolders et les membres publics.
- Dans le didacticiel précédent, nous avons amélioré la `BasePage` classe pour définir dynamiquement le `Page` l’objet `MasterPageFile` propriété basée sur une variable de Session. Comment pour nous prenons en charge des pages maîtres dynamiques lors de l’utilisation des pages maîtres imbriquées ?

Ces deux défis ferez apparaître comme nous générer la page maître imbriquée et l’utiliser à partir de nos pages de contenu existants. Nous allons examiner et supprimer ces problèmes lorsqu’ils surviennent.

## <a name="step-4-creating-the-nested-master-page"></a>Étape 4 : Création de la Page maître imbriquée

Notre première tâche consiste à créer la page maître imbriquée à utiliser par les pages dans la section Administration. Comme nous l’avons vu à l’étape 2, lors de l’ajout d’une nouvelle nested page maître, nous devons spécifier la page maître du parent de la page maître imbriquée. Mais nous avons deux pages maîtres de niveau supérieur : `Site.master` et `Alternate.master`. Souvenez-vous que nous avons créé `Alternate.master` dans le didacticiel précédent et écrit du code le `BasePage` classe que définir l’objet Page `MasterPageFile` propriété lors de l’exécution soit `Site.master` ou `Alternate.master` selon la valeur de la `MyMasterPage` Variable de session.

Comment nous configurer notre page maître imbriquée afin qu’il utilise la page maître de niveau supérieur approprié ? Nous avons deux options :

- Créer deux pages maîtres imbriquées, `AdminNestedSite.master` et `AdminNestedAlternate.master`et les lier aux pages maîtres niveau supérieur `Site.master` et `Alternate.master`, respectivement. Dans `BasePage`, ensuite, nous affectons la `Page` l’objet `MasterPageFile` à la page maître imbriquée appropriée.
- Créer une page maître imbriquée unique et les pages de contenu utilisent cette page maître particulier. Ensuite, lors de l’exécution, nous devons définir la page maître imbriquée `MasterPageFile` propriété à la page maître de niveau supérieur appropriée lors de l’exécution. (Comme vous pouvez avez deviné, pages maîtres ont également un `MasterPageFile` propriété.)

Nous allons utiliser la deuxième option. Créer un fichier page maître imbriquée unique dans le `~/Admin` dossier nommé `AdminNested.master`. Étant donné que les deux `Site.master` et `Alternate.master` ont le même ensemble de contrôles ContentPlaceHolder, peu importe quelle page maître vous liez à, bien que je vous encourage à lier à `Site.master` pour des raisons de sécurité de la cohérence.


[![Ajouter une Page maître imbriquée dans le dossier ~/Admin.](nested-master-pages-cs/_static/image26.png)](nested-master-pages-cs/_static/image25.png)

**Figure 09**: ajouter une Page maître imbriquée à la `~/Admin` dossier. ([Cliquez pour afficher l’image en taille réelle](nested-master-pages-cs/_static/image27.png))


Étant donné que la page maître imbriquée est liée à une page maître avec quatre contrôles ContentPlaceHolder, Visual Studio ajoute quatre contrôles de contenu pour un balisage initial du nouveau fichier page maître imbriquée. Comme nous l’avons fait dans les étapes 2 et 3, ajoutez un contrôle ContentPlaceHolder dans chaque contrôle de contenu, en lui donnant le même nom qu’un contrôle ContentPlaceHolder le niveau supérieur de la page maître. Ajoutez également le balisage suivant au contrôle de contenu qui correspond à la `MainContent` ContentPlaceHolder :


[!code-html[Main](nested-master-pages-cs/samples/sample8.html)]

Ensuite, définissez le `instructions` de classe CSS dans le `Styles.css` et `AlternateStyles.css` fichiers CSS. Les règles CSS suivantes provoquent des éléments HTML mise en forme en la `instructions` classe à afficher avec une couleur d’arrière-plan jaune et une bordure noire unie :


[!code-css[Main](nested-master-pages-cs/samples/sample9.css)]

Étant donné que ce balisage a été ajouté à la page maître imbriquée, il apparaît uniquement dans les pages qui utilisent cette page maître imbriquée (à savoir, les pages dans la section Administration).

Après avoir apporté ces ajouts à votre page maître imbriquée, son balisage déclaratif doit ressembler à ce qui suit :


[!code-aspx[Main](nested-master-pages-cs/samples/sample10.aspx)]

Notez que chaque contrôle de contenu a un contrôle ContentPlaceHolder et que les contrôles ContentPlaceHolder `ID` propriétés reçoivent les mêmes valeurs que les contrôles ContentPlaceHolder correspondants dans la page maître de niveau supérieur. En outre, le balisage spécifique à la section Administration apparaît dans le `MainContent` ContentPlaceHolder.

La figure 10 illustre le `AdminNested.master` page maître imbriquée lorsqu’ils sont affichés via le Concepteur de Visual Studio. Vous pouvez voir les instructions dans la zone jaune en haut de la `MainContent` contrôle de contenu.


[![La Page maître imbriquée s’étend à la Page maître de niveau supérieur pour inclure des Instructions pour l’administrateur.](nested-master-pages-cs/_static/image29.png)](nested-master-pages-cs/_static/image28.png)

**Figure 10**: la Page maître imbriquée étend la Page maître de niveau supérieur pour inclure des Instructions pour l’administrateur. ([Cliquez pour afficher l’image en taille réelle](nested-master-pages-cs/_static/image30.png))


## <a name="step-5-updating-the-existing-content-pages-to-use-the-new-nested-master-page"></a>Étape 5 : Mise à jour les Pages de contenu existants pour utiliser la nouvelle Page maître imbriquée

Chaque fois que nous ajoutons une nouvelle page de contenu à la section d’Administration que nous devons liez-le à le `AdminNested.master` page maître que nous venons de créer. Mais qu’en est-il des existant pages de contenu ? Actuellement, toutes les pages de contenu sur le site de dérivent à partir de la `BasePage` classe, qui définit par programme le contenu de page maître lors de l’exécution. Cela n’est pas le comportement souhaité pour les pages de contenu dans la section Administration. Au lieu de cela, nous voulons ces pages de contenu à toujours utiliser le `AdminNested.master` page. Il sera la responsabilité de la page maître imbriquée pour choisir la page de contenu à droite de niveau supérieur lors de l’exécution.

À la meilleure façon d’atteindre ce souhaité comportement consiste à créer une nouvelle classe de page de base personnalisée nommée `AdminBasePage` qui étend la `BasePage` classe. `AdminBasePage` pouvez ensuite substituer les `SetMasterPageFile` et définir le `Page` l’objet `MasterPageFile` à la valeur codée en dur « ~ / Admin/AdminNested.master ». De cette façon, n’importe quelle page qui dérive de `AdminBasePage` utilisera `AdminNested.master`, tandis que n’importe quelle page qui dérive de `BasePage` aura son `MasterPageFile` propriété la valeur dynamiquement » ~ / Site.master » ou « ~ / Alternate.master » selon la valeur de la `MyMasterPage` Variable de session.

Commencez par ajouter un nouveau fichier de classe à la `App_Code` dossier nommé `AdminBasePage.cs`. Avez `AdminBasePage` étendre `BasePage` , puis vous remplacez le `SetMasterPageFile` (méthode). Dans cette méthode affecter le `MasterPageFile` la valeur « ~ / Admin/AdminNested.master ». Après avoir apporté ces modifications à votre classe de fichier doit ressembler à ce qui suit :


[!code-csharp[Main](nested-master-pages-cs/samples/sample11.cs)]

Nous devons maintenant avoir les pages de contenu existants dans la section dériver à partir de l’Administration `AdminBasePage` au lieu de `BasePage`. Accédez au fichier de classe code-behind pour chaque page de contenu dans le `~/Admin` dossier et apporter cette modification. Par exemple, dans `~/Admin/Default.aspx` vous modifieriez la déclaration de classe code-behind à partir de :


[!code-csharp[Main](nested-master-pages-cs/samples/sample12.cs)]

À :


[!code-csharp[Main](nested-master-pages-cs/samples/sample13.cs)]

Figure 11 illustre comment la page maître de niveau supérieur (`Site.master` ou `Alternate.master`), la page maître imbriquée (`AdminNested.master`), et les pages de contenu de la section Administration sont liés entre eux.


[![La Page maître imbriquée définit le contenu spécifique aux Pages dans la Section Administration](nested-master-pages-cs/_static/image32.png)](nested-master-pages-cs/_static/image31.png)

**Figure 11**: The imbriqués Master Page définit contenu spécifique aux Pages dans la Section Administration ([cliquez pour afficher l’image en taille réelle](nested-master-pages-cs/_static/image33.png))


## <a name="step-6-mirroring-the-master-pages-public-methods-and-properties"></a>Étape 6 : Mise en miroir des propriétés et méthodes publiques de la Page maître

N’oubliez pas que le `~/Admin/AddProduct.aspx` et `~/Admin/Products.aspx` pages interagissent par programmation avec la page maître : `~/Admin/AddProduct.aspx` appelle la page maître publique du `RefreshRecentProductsGrid` (méthode) et définit ses `GridMessageText` propriété ; `~/Admin/Products.aspx` a un gestionnaire d’événements pour le `PricesDoubled` événement. Dans le didacticiel précédent, nous avons créé abstraite `BaseMasterPage` classe qui a défini ces membres publics.

Le `~/Admin/AddProduct.aspx` et `~/Admin/Products.aspx` pages supposent que leur page maître dérive le `BaseMasterPage` classe. Le `AdminNested.master` page, cependant, actuellement étend la `System.Web.UI.MasterPage` classe. Par conséquent, lors de la visite `~/Admin/Products.aspx` un `InvalidCastException` est levée avec le message : « Impossible convertit un objet de type ' ASP.admin\_adminnested\_maître ' en type 'BaseMasterPage'. »

Pour corriger cela nous devons disposer le `AdminNested.master` étendre la classe code-behind `BaseMasterPage`. Mettre à jour de la déclaration de classe code-behind de la page maître imbriquée à partir de :


[!code-csharp[Main](nested-master-pages-cs/samples/sample14.cs)]

À :


[!code-csharp[Main](nested-master-pages-cs/samples/sample15.cs)]

Nous n’avons pas encore terminé. Étant donné que le `BaseMasterPage` classe est abstraite, nous devons remplacer le `abstract` membres, `RefreshRecentProductsGrid` et `GridMessageText`. Ces membres sont utilisés par les pages maîtres de niveau supérieur pour mettre à jour leurs interfaces utilisateur. (En fait, seul le `Site.master` page maître utilise ces méthodes, bien que les deux pages maîtres de niveau supérieur implémentent ces méthodes, étant donné que les deux étendent `BaseMasterPage`.)

Bien que nous devons implémenter ces membres dans `AdminNested.master`, il suffit ces implémentations est simplement appeler le même membre dans la page maître de niveau supérieur utilisée par la page maître imbriquée. Par exemple, lorsqu’une page de contenu dans la section Administration appelle la page maître imbriquée `RefreshRecentProductsGrid` (méthode), la page maître imbriquée suffit à faire est, à son tour, appelez `Site.master` ou `Alternate.master`de `RefreshRecentProductsGrid` (méthode).

Pour ce faire, démarrez en ajoutant le code suivant `@MasterType` directive au début du `AdminNested.master`:


[!code-aspx[Main](nested-master-pages-cs/samples/sample16.aspx)]

N’oubliez pas que le `@MasterType` directive ajoute une propriété fortement typée à la classe code-behind nommée `Master`. Substituez ensuite la `RefreshRecentProductsGrid` et `GridMessageText` membres et vous déléguez simplement l’appel à la `Master`correspondante de méthode du :


[!code-csharp[Main](nested-master-pages-cs/samples/sample17.cs)]

Avec ce code en place, vous pourrez consultez et utilisez les pages de contenu dans la section Administration. La figure 12 illustre le `~/Admin/Products.aspx` page lorsqu’ils sont affichés via un navigateur. Comme vous pouvez le voir, cette page inclut la zone des Instructions d’Administration, qui est définie dans la page maître imbriquée.


[![Les Pages de contenu dans la Section Administration incluent des Instructions en haut de chaque Page](nested-master-pages-cs/_static/image35.png)](nested-master-pages-cs/_static/image34.png)

**Figure 12**: les Pages de contenu dans l’Administration Section incluent des Instructions sur le haut de chaque Page ([cliquez pour afficher l’image en taille réelle](nested-master-pages-cs/_static/image36.png))


## <a name="step-7-using-the-appropriate-top-level-master-page-at-runtime"></a>Étape 7 : À l’aide de la Page principale de niveau supérieur appropriée lors de l’exécution

Alors que toutes les pages de contenu dans la section Administration entièrement fonctionnels, ils utilisent tous la même page maître de niveau supérieur et ignorer la page maître sélectionnée par l’utilisateur au niveau `ChooseMasterPage.aspx`. Ce comportement est dû au fait que la page maître imbriquée a son `MasterPageFile` propriété à affecter statiquement aux `Site.master` dans son `<%@ Master %>` directive.

Pour utiliser la page maître de niveau supérieur sélectionnée par l’utilisateur final, nous devons définir la `AdminNested.master`de `MasterPageFile` valeur à la propriété dans le `MyMasterPage` variable de Session. Car nous avons défini les pages de contenu `MasterPageFile` propriétés dans `BasePage`, vous pouvez penser que nous affectons la page maître imbriquée `MasterPageFile` propriété dans `BaseMasterPage` ou dans le `AdminNested.master`de classe code-behind. Cela ne fonctionne pas, toutefois, étant donné que nous devons avoir défini le `MasterPageFile` propriété à la fin de la phase PreInit. L’heure plus tôt que nous pouvons exploiter par programmation sur le cycle de vie de page à partir d’une page maître est l’étape Init (ce qui se produit après l’étape PreInit).

Par conséquent, nous devons définir la page maître imbriquée `MasterPageFile` propriété à partir des pages de contenu. Seul le contenu des pages qui utilisent le `AdminNested.master` dérivent de la page maître `AdminBasePage`. Par conséquent, nous pouvons placées cette logique. À l’étape 5 nous a remplacé le `SetMasterPageFile` méthode, en affectant la `Page` l’objet `MasterPageFile` propriété à « ~ / Admin/AdminNested.master ». Mise à jour `SetMasterPageFile` également définir la page maître `MasterPageFile` propriété pour le résultat stocké dans la Session :


[!code-csharp[Main](nested-master-pages-cs/samples/sample18.cs)]

Le `GetMasterPageFileFromSession` (méthode), ce qui nous avons ajouté à la `BasePage` classe dans le didacticiel précédent, retourne le chemin d’accès du fichier de page maître approprié selon la valeur de variable de Session.

Avec cette modification en place, sélection de page maître de l’utilisateur s’appliquent à la section Administration. La figure 13 montre la même page, comme la Figure 12, mais une fois que l’utilisateur a changé sa sélection de page maître pour `Alternate.master`.


[![La Page d’Administration imbriquée utilise la Page principale de niveau supérieur sélectionné par l’utilisateur](nested-master-pages-cs/_static/image38.png)](nested-master-pages-cs/_static/image37.png)

**Figure 13**: la Page d’Administration Nested utilise la Page de principale de niveau supérieur sélectionné par l’utilisateur ([cliquez pour afficher l’image en taille réelle](nested-master-pages-cs/_static/image39.png))


## <a name="summary"></a>Récapitulatif

Quantité pages de contenu comment like peuvent lier à une page maître, il est possible de créer une prédiction imbriquée des pages maîtres par une page maître enfant lient à une page maître parente. La page maître enfant peut définir des contrôles de contenu pour chacun des ContentPlaceHolders de son parent ; elle peut ensuite ajouter ses propres contrôles ContentPlaceHolder (ainsi que d’autres balises) pour ces contrôles de contenu. Les pages maîtres imbriquées sont très utiles dans les applications web de grande taille dont toutes les pages partagent un aspect global, mais certaines sections du site requièrent des personnalisations uniques.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [Pages maître ASP.NET imbriquées](https://msdn.microsoft.com/library/x2b3ktt7.aspx)
- [Conseils pour les Pages maîtres imbriquées et de conception de Visual Studio 2005](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx)
- [Visual Studio 2008 imbriqués prise en charge de la Page maître](https://weblogs.asp.net/scottgu/archive/2007/07/09/vs-2008-nested-master-page-support.aspx)

### <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de plusieurs livres de sur ASP/ASP.NET et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier livre s’intitule [ *Sams Teach vous-même ASP.NET 3.5 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott peut être atteint à [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) ou via son blog à [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](specifying-the-master-page-programmatically-cs.md)
> [Suivant](creating-a-site-wide-layout-using-master-pages-vb.md)
