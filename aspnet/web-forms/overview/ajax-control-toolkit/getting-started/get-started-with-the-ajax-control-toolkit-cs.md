---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
title: Bien démarrer avec AJAX Control Toolkit (c#) | Microsoft Docs
author: microsoft
description: Apprendre tout ce que vous devez savoir pour commencer à utiliser les outils de contrôle AJAX.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 16dc5c11-65be-4eae-a818-9fad7f8259c6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
msc.type: authoredcontent
ms.openlocfilehash: 6ecf716b78a789ca72e8b35e0be3e1fd0b957052
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828239"
---
<a name="get-started-with-the-ajax-control-toolkit-c"></a>Bien démarrer avec AJAX Control Toolkit (c#)
====================
par [Microsoft](https://github.com/microsoft)

> Apprendre tout ce que vous devez savoir pour commencer à utiliser les outils de contrôle AJAX.


AJAX Control Toolkit contient plus de 30 contrôles gratuits que vous pouvez utiliser dans vos applications ASP.NET. Dans ce didacticiel, vous allez apprendre à télécharger les outils de contrôle AJAX et ajoutez les contrôles de boîte à outils à votre boîte à outils Visual Studio/Visual Web Developer Express.

## <a name="downloading-the-ajax-control-toolkit"></a>Télécharger les outils de contrôle AJAX

Le [AJAX Control Toolkit](http://devexpress.com/act) est un projet open source développé par les membres de la Communauté ASP.NET et de l’équipe ASP.NET. 


[![Télécharger les outils de contrôle AJAX](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)

**Figure 01**: téléchargement AJAX Control Toolkit ([cliquez pour afficher l’image en taille réelle](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))


Après avoir téléchargé le fichier, vous devez débloquer le fichier. Cliquez sur le fichier, sélectionnez Propriétés, puis cliquez sur le **Unblock** bouton (voir Figure 2).


[![Débloquant le fichier ZIP de boîte à outils de contrôle AJAX](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)

**Figure 02**: débloquant le fichier ZIP de boîte à outils de contrôle AJAX ([cliquez pour afficher l’image en taille réelle](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))


Une fois que vous débloquez le fichier, vous pouvez décompresser le fichier : cliquez sur le fichier et sélectionnez le **extraire tout** option de menu. Maintenant, nous sommes prêts à ajouter le Kit de ressources à la boîte à outils Visual Studio/Visual Web Developer.

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a>Ajout d’AJAX Control Toolkit à la boîte à outils

Pour utiliser les outils de contrôle AJAX le plus simple consiste à ajouter le Kit de ressources à votre boîte à outils Visual Studio/Visual Web Developer (voir Figure 3). De cette façon, vous pouvez simplement faire glisser un contrôle de boîte à outils sur une page lorsque vous souhaitez l’utiliser.


[![Outils de contrôle AJAX s’affiche dans la boîte à outils](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)

**Figure 03**: AJAX Control Toolkit apparaît dans la boîte à outils ([cliquez pour afficher l’image en taille réelle](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))


Tout d’abord, vous devez ajouter un onglet de boîte à outils de contrôle AJAX à la boîte à outils. Suivez ces étapes.

1. Créer un site Web ASP.NET en sélectionnant l’option de menu fichier, nouveau site Web. Double-cliquez sur la page Default.aspx dans la fenêtre Explorateur de solutions pour ouvrir le fichier dans l’éditeur.
2. Avec le bouton droit de la boîte à outils sous l’onglet Général et sélectionnez l’option de menu **ajouter un onglet** (voir Figure 4).
3. Entrez un nouvel onglet appelé AJAX Control Toolkit.


[![Ajout d’un nouvel onglet](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)

**Figure 04**: ajout d’un nouvel onglet ([cliquez pour afficher l’image en taille réelle](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))


Ensuite, vous devez ajouter les contrôles AJAX Control Toolkit pour le nouvel onglet. Procédez comme suit :

- Avec le bouton droit sous l’onglet AJAX Control Toolkit et sélectionnez l’option de menu **choisir des éléments (voir Figure 5)**.
- Accédez à l’emplacement où vous avez décompressé AJAX Control Toolkit et sélectionnez l’assembly AjaxControlToolkit.dll.


[![Choisissez les éléments à ajouter à la boîte à outils](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)

**Figure 05**: choisissez les éléments à ajouter à la boîte à outils ([cliquez pour afficher l’image en taille réelle](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))


Après avoir effectué ces étapes, tous les contrôles de boîte à outils seront affiche dans votre boîte à outils.

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a>La mise à niveau vers une nouvelle Version du Kit de ressources

Si vous utilisiez une version antérieure du Kit de ressources et que vous devez maintenant déplacer vers une version ultérieure ici sont les étapes recommandées :

- Les fichiers binaires - supprimer l’ancienne version de l’assembly AjaxControlToolkit.dll à partir de votre dossier d’emplacement de site Web.
- Éléments de boîte à outils - supprimer l’onglet AJAX Control Toolkit et suivez les étapes ci-dessus pour créer de nouveau l’onglet avec la nouvelle version de l’assembly AjaxControlToolkit.dll.

> [!div class="step-by-step"]
> [Next](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
