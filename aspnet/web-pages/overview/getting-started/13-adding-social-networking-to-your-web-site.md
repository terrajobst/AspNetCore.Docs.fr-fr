---
uid: web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
title: Ajout de réseaux sociaux pour ASP.NET Web Pages (Razor) Sites | Microsoft Docs
author: Rick-Anderson
description: Ce chapitre explique comment intégrer votre site avec les services de réseaux sociaux. Dans ce chapitre, vous allez apprendre à permettre aux utilisateurs de votre site Web de signet/lien...
ms.author: riande
ms.date: 02/21/2014
ms.assetid: 03c342f9-b35c-4d7c-b9ed-cd9aaaffedb6
msc.legacyurl: /web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: d2b970e7b80e4129d0a912f648f9c4a54df531b2
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021155"
---
<a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a>Ajout de réseaux sociaux Sites de réseau pour les Pages Web ASP.NET (Razor)
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article explique comment ajouter des liens de réseau sociales pour Facebook, Twitter, Reddit et Digg vers des pages dans un site Web ASP.NET Web Pages (Razor) et comment inclure des flux Twitter, les cartes de joueur Xbox et les images Gravatar.
> 
> Ce que vous allez apprendre :
> 
> - Explique comment permettre aux utilisateurs de lien de signet/votre site.
> - Comment ajouter un flux Twitter.
> - Comment ajouter un Facebook **comme** bouton aux pages.
> - Comment rendre des images de Gravatar.com.
> - Comment afficher une carte de joueur Xbox sur votre site.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - Bibliothèque d’assistance de Web ASP.NET (package NuGet)
>   
> 
> Ce didacticiel fonctionne également avec ASP.NET Web Pages 3, à l’exception des parties qui utilisent la bibliothèque d’assistance de Web ASP.NET.


<a id="Linking_Your_Website"></a>
## <a name="linking-your-website-on-social-networking-sites"></a>Liaison de votre site Web sur les Sites de réseau Social

Si les personnes comme quelque chose sur votre site, ils souhaitent souvent partager avec vos amis. Vous pouvez faciliter la tâche en affichant les glyphes (icônes) qui personnes peuvent cliquer pour partager une page de Digg, Reddit, Facebook, Twitter ou sites similaires.

Pour afficher ces glyphes, ajoutez le `LinkSharecode` helper à une page. Les personnes qui visitent votre page peuvent cliquer sur un glyphe individuel. S’ils ont un compte avec ce site de réseau social, ils peuvent ensuite valider un lien vers votre page sur ce site.

![Image 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. Ajoutez la bibliothèque de programmes d’assistance de Web ASP.NET à votre site Web, comme décrit dans [l’installation des programmes d’assistance dans un Site ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si vous n’avez pas déjà ajouté y - créer une page nommée *ListLinkShare.cshtml* et ajouter le balisage suivant :

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    Dans cet exemple, lorsque le `LinkShare` s’exécute, le titre de page est passé en tant que paramètre, qui à son tour transmet le titre de la page vers le site de réseau social. Toutefois, vous pouvez transmettre n’importe quelle chaîne souhaitée. Cet exemple spécifie également les sites de réseau social à inclure dans la liste. Vous pouvez spécifier les sites de réseau social qui s’appliquent à votre site.
2. Exécutez le *ListLinkShare.cshtml* page dans un navigateur. (Assurez-vous que la page est sélectionnée dans le **fichiers** espace de travail avant de l’exécuter.)
3. Cliquez sur un glyphe pour l’un des sites que vous êtes inscrit pour. Ce lien vous mène à la page sur le site de réseau social sélectionné où vous pouvez partager un lien. Par exemple, si vous cliquez sur le lien Reddit, vous êtes redirigé vers le `submit to reddit` sur le site Web Reddit.

     ![Image 2](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a>Ajout d’un Twitter du flux

Pour plus d’informations sur l’utilisation d’un helper Twitter qui est compatible avec la version actuelle de l’API Twitter, consultez [application auxiliaire Twitter](../ui-layouts-and-themes/twitter-helper.md). Cet exemple montre comment écrire votre propre application auxiliaire, vous pouvez facilement réutiliser le code à partir de nombreuses pages.

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a>Affichage de Facebook &quot;comme&quot; bouton

Dans certains cas, votre meilleure option est pour obtenir le code directement depuis le fournisseur de mise en réseau social, plutôt que de compter sur une application auxiliaire. Cela est particulièrement vrai si le fournisseur de réseau social met à jour ses options plus rapidement que l’application d’assistance est mis à jour.

Pour ajouter des fonctionnalités de Facebook (par exemple, le bouton Like) à votre site, vous pouvez récupérer des extraits de code à partir de la [developers.facebook.com](https://developers.facebook.com/) site. Sur le site Facebook, vous utilisez leurs outils pour générer un extrait de code qui s’applique à votre site.

Le code en surbrillance suivant est le code qui a été récupéré à partir de l’outil comme bouton sur le site developers.facebook.com. Vous devez fournir votre propre ID d’application.

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a>Rendu d’une Image Gravatar

Un *Gravatar* (un &quot;avatar mondialement reconnue&quot;) est une image qui peut être utilisée sur plusieurs sites Web en tant que votre avatar &#8212; , autrement dit, une image qui vous représente. Par exemple, un Gravatar peut identifier une personne dans un billet de forum, dans un commentaire de blog et ainsi de suite. (Vous pouvez inscrire votre propre Gravatar sur le site Web Gravatar à [ http://www.gravatar.com/ ](http://www.gravatar.com/).) Si vous souhaitez afficher des images en regard des noms de personnes ou les adresses de messagerie sur votre site Web, vous pouvez utiliser l’application d’assistance Gravatar.

Dans cet exemple, vous utilisez un Gravatar unique qui représente vous-même. Une autre façon d’utiliser un Gravatar consiste à permettre aux utilisateurs de spécifier leur adresse Gravatar lorsqu’ils s’inscrivent sur votre site. (Vous pouvez apprendre à permettre aux utilisateurs d’inscrire dans [Ajout de la sécurité et l’appartenance à un Site ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202904).) Puis chaque fois que vous affichez des informations pour cet utilisateur, il vous suffit d’ajouter le Gravatar à où vous affichez le nom d’utilisateur.

1. Ajoutez la bibliothèque de programmes d’assistance de Web ASP.NET à votre site Web, comme décrit dans [l’installation des programmes d’assistance dans un Site ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si vous n’avez pas déjà.
2. Créer une nouvelle page web nommée *Gravatar.cshtml*.
3. Ajoutez le balisage suivant au fichier : 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    Le `Gravatar.GetHtml` méthode affiche l’image Gravatar sur la page. Pour modifier la taille de l’image, vous pouvez inclure un nombre en tant que deuxième paramètre. La taille par défaut est 80. Nombres inférieur à 80 marque l’image plus petits. Les nombres supérieurs à 80 agrandir l’image.
4. Dans le `Gravatar.GetHtml` méthodes, remplacez `<Your Gravatar account here>` avec l’adresse de messagerie que vous utilisez pour votre compte de Gravatar. (Si vous n’avez pas un compte Gravatar, vous pouvez utiliser l’adresse de messagerie d’un utilisateur.)
5. Exécutez la page dans votre navigateur. La page affiche deux images Gravatar pour l’adresse de messagerie que vous avez spécifié. La deuxième image est inférieure à la première. 

    ![Image 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a>Affichage d’une carte de joueur Xbox

Lorsque les personnes jouent Microsoft Xbox en ligne, chaque utilisateur dispose d’un ID unique. Statistiques sont conservées pour chaque joueur sous la forme d’une carte de joueur, qui indique leur réputation, le score de joueur et récemment lu jeux. Si vous êtes un passionné de Xbox, vous pouvez afficher votre carte de joueur sur les pages de votre site à l’aide de la `GamerCard` helper.

1. Ajoutez la bibliothèque de programmes d’assistance de Web ASP.NET à votre site Web, comme décrit dans [l’installation des programmes d’assistance dans un Site ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si vous n’avez pas déjà.
2. Créer une page nommée *XboxGamer.cshtml* et ajoutez le balisage suivant.

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    Vous utilisez le `GamerCard.GetHtml` propriété pour spécifier l’alias pour la carte de joueur à afficher.
3. Exécutez la page dans votre navigateur. La page affiche la carte de joueur Xbox que vous avez spécifié.

    ![Image 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)
