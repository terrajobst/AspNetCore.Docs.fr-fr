---
uid: mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-cs
title: Creating Page Layouts with View Master Pages (c#) | Microsoft Docs
author: microsoft
description: Dans ce didacticiel, vous allez apprendre à créer une disposition commune pour plusieurs pages dans votre application en tirant parti de la vue de pages maîtres. Vous pouvez utiliser un...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: dff54fcb-68b1-4488-89a2-ca97532d6a4c
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: acf304d0f1cf349f7e9c30acb9271e602a47d3d9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831321"
---
<a name="creating-page-layouts-with-view-master-pages-c"></a>Creating Page Layouts with View Master Pages (c#)
====================
par [Microsoft](https://github.com/microsoft)

[Télécharger PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_12_CS.pdf)

> Dans ce didacticiel, vous allez apprendre à créer une disposition commune pour plusieurs pages dans votre application en tirant parti de la vue de pages maîtres. Vous pouvez utiliser une page maître de vue, par exemple, pour définir une mise en page de deux colonnes et utiliser la disposition de deux colonnes pour toutes les pages de votre application web.


## <a name="creating-page-layouts-with-view-master-pages"></a>Creating Page Layouts with View Master Pages

Dans ce didacticiel, vous allez apprendre à créer une disposition commune pour plusieurs pages dans votre application en tirant parti de la vue de pages maîtres. Vous pouvez utiliser une page maître de vue, par exemple, pour définir une mise en page de deux colonnes et utiliser la disposition de deux colonnes pour toutes les pages de votre application web.

Vous également pourrez tirer parti de la vue des pages maîtres pour partager du contenu commun sur plusieurs pages dans votre application. Par exemple, vous pouvez placer votre logo du site Web, les liens de navigation et les publications de bannière dans une page maître de vue. De cette façon, chaque page dans votre application afficherait ce contenu automatiquement.

Dans ce didacticiel, vous allez apprendre à créer une nouvelle page maître de vue et de créer une nouvelle page de contenu de vue basée sur la page maître.

### <a name="creating-a-view-master-page"></a>Création d’une Page maître de vue

Nous allons commencer en créant une page maître de vue qui définit une disposition à deux colonnes. Vous ajoutez une nouvelle page maître de vue à un projet MVC en double-cliquant sur le dossier Views\Shared, en sélectionnant l’option de menu **ajouter, nouvel élément**et en sélectionnant le **Page maître de vue MVC** modèle (voir Figure 1).


[![Ajout d’une page maître de vue](creating-page-layouts-with-view-master-pages-cs/_static/image2.png)](creating-page-layouts-with-view-master-pages-cs/_static/image1.png)

**Figure 01**: ajout d’une page maître de vue ([cliquez pour afficher l’image en taille réelle](creating-page-layouts-with-view-master-pages-cs/_static/image3.png))


Vous pouvez créer plusieurs pages de vue maître dans une application. Chaque page maître de vue peut définir une mise en page différentes. Par exemple, vous souhaiterez peut-être certaines pages d’avoir une disposition à deux colonnes et d’autres pages pour avoir une disposition à trois colonnes.

Une page maître de vue ressemble beaucoup à une vue ASP.NET MVC standard. Toutefois, contrairement à un affichage normal, une page maître de vue contient un ou plusieurs `<asp:ContentPlaceHolder>` balises. Le `<contentplaceholder>` balises sont utilisées pour marquer les zones de la page maître qui peut être substituée dans une page de contenu individuelle.

Par exemple, la page maître de la vue dans la liste 1 définit une disposition à deux colonnes. Il contient deux `<contentplaceholder>` balises. Un `<ContentPlaceHolder>` pour chaque colonne.

**Liste 1 : `Views\Shared\Site.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample1.aspx)]

Le corps de la vue de page maître dans le Listing 1 contient deux `<div>` balises qui correspondent aux deux colonnes. La classe de colonne de feuille de Style en cascade est appliquée aux deux `<div>` balises. Cette classe est définie dans la feuille de style déclarée en haut de la page maître. Vous pouvez prévisualiser l’affichage de la page maître en mode par passer en mode Design. Cliquez sur l’onglet conception en bas à gauche de l’éditeur de code source (voir Figure 2).


[![Afficher un aperçu d’une page maître dans le Concepteur](creating-page-layouts-with-view-master-pages-cs/_static/image5.png)](creating-page-layouts-with-view-master-pages-cs/_static/image4.png)

**Figure 02**: afficher un aperçu d’une page maître dans le concepteur ([cliquez pour afficher l’image en taille réelle](creating-page-layouts-with-view-master-pages-cs/_static/image6.png))


### <a name="creating-a-view-content-page"></a>Création d’une Page de contenu de vue

Après avoir créé une page maître de vue, vous pouvez créer les pages de contenu basés sur la page maître en mode affichage d’un ou plusieurs. Par exemple, vous pouvez créer une page de contenu de vue Index pour le contrôleur Home en double-cliquant sur le dossier Views\Home, en sélectionnant **ajouter, nouvel élément**, en sélectionnant le **Page de contenu de vue MVC** modèle, entrant le nom Index.aspx, puis en cliquant sur le **ajouter** bouton (voir Figure 3).


[![Ajout d’une page de contenu de vue](creating-page-layouts-with-view-master-pages-cs/_static/image8.png)](creating-page-layouts-with-view-master-pages-cs/_static/image7.png)

**Figure 03**: ajout d’une page de contenu de vue ([cliquez pour afficher l’image en taille réelle](creating-page-layouts-with-view-master-pages-cs/_static/image9.png))


Après avoir cliqué sur le bouton Ajouter, une nouvelle boîte de dialogue s’affiche qui vous permet de sélectionner une page maître de la vue à associer à la page de contenu de vue (voir Figure 4). Vous pouvez accéder à la page maître de vue Site.master que nous avons créé dans la section précédente.


[![Sélection d’une page maître](creating-page-layouts-with-view-master-pages-cs/_static/image11.png)](creating-page-layouts-with-view-master-pages-cs/_static/image10.png)

**Figure 04**: sélection d’une page maître ([cliquez pour afficher l’image en taille réelle](creating-page-layouts-with-view-master-pages-cs/_static/image12.png))


Une fois que vous créez une nouvelle page de contenu de vue basée sur la page maître Site.master, vous obtenez le fichier dans le Listing 2.

**Listing 2 : `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample2.aspx)]

Notez que cette vue contient un `<asp:Content>` balise correspondant à chacun de la `<asp:ContentPlaceHolder>` balises dans la page maître de vue. Chaque `<asp:Content>` balise inclut un attribut ContentPlaceHolderID qui pointe vers le particulier `<asp:ContentPlaceHolder>` qu’elle remplace.

En outre, notez que la page d’affichage de contenu dans le Listing 2 ne contient pas toutes les normales d’ouverture et les balises de fermeture HTML. Par exemple, il ne contient pas de l’ouverture et la fermeture `<html>` ou `<head>` balises. Toutes les balises de fermeture et normales d’ouverture sont contenus dans la page maître de vue.

Tout contenu que vous souhaitez afficher dans une page de contenu de vue doit être placé dans un `<asp:Content>` balise. Si vous placez le code HTML ou tout autre contenu en dehors de ces balises, vous obtenez une erreur lorsque vous essayez d’afficher la page.

Vous n’avez pas besoin de remplacer chaque `<asp:ContentPlaceHolder>` balise à partir d’une page maître dans une page d’affichage de contenu. Il vous suffit de remplacer un `<asp:ContentPlaceHolder>` baliser lorsque vous souhaitez remplacer la balise avec un contenu particulier.

Par exemple, la vue Index modifiée dans le Listing 3 contient uniquement deux `<asp:Content>` balises. Chacun de la `<asp:Content>` balises inclut du texte.

**Liste 3 : `Views\Home\Index.aspx (modified)`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample3.aspx)]

Lorsque la vue dans la liste 3 est demandée, il restitue la page à la Figure 5. Notez que la vue restitue une page avec deux colonnes. En outre, notez que le contenu à partir de la page de vue de contenu est fusionné avec le contenu à partir de la page maître de vue


[![La page de contenu de la vue Index](creating-page-layouts-with-view-master-pages-cs/_static/image14.png)](creating-page-layouts-with-view-master-pages-cs/_static/image13.png)

**Figure 05**: page de contenu de vue de l’Index ([cliquez pour afficher l’image en taille réelle](creating-page-layouts-with-view-master-pages-cs/_static/image15.png))


### <a name="modifying-view-master-page-content"></a>Modification du contenu des pages maîtres de vue

Un problème que vous rencontrez presque immédiatement lorsque vous travaillez avec les pages maîtres de vue est le problème de la modification de contenu de page maître de vue lorsque les pages de contenu de vue différents sont demandés. Par exemple, vous souhaitez que chaque page dans votre application web pour avoir un titre unique. Toutefois, le titre est déclaré dans la page maître de vue et pas dans la page de contenu d’affichage. Par conséquent, comment personnaliser le titre de page pour chaque page de contenu d’affichage ?

Il existe deux méthodes que vous pouvez modifier le titre affiché par une page de contenu de vue. Tout d’abord, vous pouvez affecter un titre de page à l’attribut de titre de la `<%@ page %>` directive déclarés au début d’une page de contenu de vue. Par exemple, si vous souhaitez affecter le titre de la page « Super excellent site Web » à la vue Index, vous pouvez inclure la directive suivante en haut de la vue Index :

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample4.aspx)]

Lorsque la vue Index est restituée dans le navigateur, le titre de votre choix apparaît dans la barre de titre du navigateur :


[![Barre de titre de navigateur](creating-page-layouts-with-view-master-pages-cs/_static/image17.png)](creating-page-layouts-with-view-master-pages-cs/_static/image16.png)


Il existe un besoin important une page de vue maître doit satisfaire afin que l’attribut de titre travailler. La page maître de vue doit contenir un `<head runat="server">` balise au lieu d’un élément normal `<head>` balise pour son en-tête. Si le `<head>` balise n’inclut pas le runat = attribut de « serveur », puis le titre ne s’affiche. La vue par défaut page maître inclut requis `<head runat="server">` balise.

Une approche alternative à la modification du contenu de la page maître à partir d’une page de contenu de vue individuels consiste à encapsuler la région que vous souhaitez modifier dans un `<asp:ContentPlaceHolder>` balise. Par exemple, imaginez que vous souhaitez modifier le titre, mais également les balises meta, rendues par une page de vue maître. La page de vue maître sur la liste 4 contient un `<asp:ContentPlaceHolder>` balise dans son `<head>` balise.

**Liste 4 – `Views\Shared\Site2.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample5.aspx)]

Notez que le `<asp:ContentPlaceHolder>` balise sur la liste 4 inclut le contenu par défaut : un titre par défaut et les balises de métadonnées par défaut. Si vous ne substituez pas cela `<asp:ContentPlaceHolder>` balise dans une page de contenu de vue individuelle, le contenu par défaut s’affiche.

La page d’affichage de contenu dans la liste 5 remplace le `<asp:ContentPlaceHolder>` balise afin d’afficher un titre personnalisé et des balises meta personnalisées.

**Liste 5 – `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample6.aspx)]

### <a name="summary"></a>Récapitulatif

Ce didacticiel vous a fourni avec une présentation de base pour afficher les pages maîtres et les pages de contenu. Vous avez appris à créer des pages maîtres de vue et de créer des pages de contenu de vue basées sur les. Aussi, nous avons examiné comment vous pouvez modifier le contenu d’une page maître de la vue à partir d’une page de contenu de vue particulière.

> [!div class="step-by-step"]
> [Précédent](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
> [Suivant](passing-data-to-view-master-pages-cs.md)
