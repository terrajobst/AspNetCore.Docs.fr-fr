---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: Affichage des mappages dans un site ASP.NET Web Pages (Razor) Site | Microsoft Docs
author: tfitzmac
description: Cet article explique comment afficher des cartes interactives sur des pages dans un site Web de Pages Web ASP.NET (Razor) en fonction de mappage des services fournis par Bing, Google, Ma...
ms.author: aspnetcontent
ms.date: 02/20/2014
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 4c64791a77f2a72c227ce74e796340c6d8c7316c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812780"
---
<a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a>Affichage des mappages dans un Site ASP.NET Web Pages (Razor)
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article explique comment afficher des cartes interactives sur des pages dans un site Web de Pages Web ASP.NET (Razor) en fonction de mappage des services fournis par Bing, Google, MapQuest et Yahoo.
> 
> Ce que vous allez apprendre :
> 
> - Comment générer une table basée sur une adresse.
> - Comment générer une table basée sur des coordonnées de latitude et longitude.
> - Comment inscrire un compte de développeur de Bing Maps et d’obtenir une clé à utiliser avec Bing Maps.
> 
> Il s’agit de la fonctionnalité d’ASP.NET introduite dans l’article :
> 
> - Le `Maps` helper.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> Ce didacticiel fonctionne également avec WebMatrix 3.


Dans les Pages Web, vous pouvez afficher les mappages sur une page à l’aide de `Maps` helper. Vous pouvez générer des mappages en vous basés sur une adresse ou sur un jeu de coordonnées de longitude et latitude. Le `Maps` classe vous permet d’appeler dans les moteurs de carte populaires, y compris de Bing, Google, MapQuest et Yahoo.

Les étapes d’ajout de mappage à une page sont les mêmes quel que soit les moteurs de carte que vous appelez. Vous ajoutez simplement une référence de fichier JavaScript qui rend les méthodes disponibles pour afficher la carte, puis vous appelez les méthodes de la `Maps` helper.

Vous choisissez une carte de service en fonction duquel `Maps` méthode d’assistance que vous utilisez. Vous pouvez utiliser une de ces :

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a>Installer les éléments que vous avez besoin

Pour afficher les mappages, vous avez besoin de ces éléments :

- Le `Maps` helper. Ce programme d’assistance est dans la version 2 de la bibliothèque de programmes d’assistance de Web ASP.NET. Si vous n’avez pas déjà ajouté la bibliothèque, vous pouvez l’installer dans votre site sous forme de package NuGet. Pour plus d’informations, consultez [l’installation des programmes d’assistance dans un Site ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372). (Dans la galerie, recherchez le `microsoft-web-helpers` package.)
- La bibliothèque jQuery. Plusieurs des modèles de site WebMatrix incluent déjà des bibliothèques jQuery dans leurs *Script* dossiers. Si vous n’avez pas ces bibliothèques, vous pouvez télécharger la dernière bibliothèque jQuery directement à partir de la [jQuery.org](http://jQuery.org) site. Ou vous pouvez créer un nouveau site à l’aide d’un modèle (par exemple, le **Starter Site** modèle) puis copiez les fichiers de jQuery à partir de ce site dans votre site actuel.

Enfin, si vous souhaitez utiliser Bing maps, vous devez tout d’abord créer un compte (gratuit) et obtenir une clé. Pour obtenir une clé, procédez comme suit :

1. Créer un compte sur le [compte de développeur de Bing Maps](https://www.microsoft.com/maps/developers/web.aspx). Vous devez disposer d’un compte de Microsoft (Windows Live ID) également.

    Vous pouvez spécifier que vous souhaitez utiliser la clé pour **/Test d’évaluation de**. Si vous testez la fonction de mappage sur votre ordinateur à l’aide de WebMatrix et IIS Express, accédez la **Site** espace de travail et notez l’URL de votre site (par exemple, `http://localhost:50408`, bien que votre numéro de port sera probablement différente). Vous pouvez utiliser cette *localhost* adresse que le site lorsque vous inscrivez.
2. Une fois que vous êtes inscrit pour un compte, accédez au centre des comptes Bing Maps, puis cliquez sur **créer ou afficher les clés**:

    ![mappage-2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. Enregistrez la clé Bing crée.

## <a name="creating-a-map-based-on-an-address-using-google"></a>Création d’une table basée sur une adresse (à l’aide de Google)

L’exemple suivant montre comment créer une page qui affiche une table basée sur une adresse. Cet exemple montre comment utiliser Google Maps.

1. Créez un fichier nommé *MapAddress.cshtml* à la racine du site. Cette page génère une table basée sur une adresse que vous lui transmettez.
2. Copiez le code suivant dans le fichier, en remplaçant le contenu existant.

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    Notez les fonctionnalités suivantes de la page :

    - Le `<script>` élément dans le `<head>` élément. Dans l’exemple, le `<script>` références de l’élément le *jquery-1.6.4.min.js* fichier, qui est une version réduite (compressée) de la bibliothèque jQuery, version 1.6.4. Notez que la référence suppose que le *.js* fichier se trouve dans le *Scripts* dossier de votre site. 

        > [!NOTE]
        > Si vous utilisez une version différente de la bibliothèque jQuery, assurez-vous simplement que vous pointez vers cette version correctement.
    - L’appel à la `@Maps.GetGoogleHtml` dans le corps de la page. Pour mapper une adresse, vous devez passer une chaîne d’adresse. Les méthodes pour les autres moteurs de carte fonctionnent de manière similaire (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).
3. Exécutez la page et entrez une adresse. La page affiche une table, basée sur Google Maps, qui indique l’emplacement que vous avez spécifié.

     ![mappage de-1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a>Création d’une table basée sur une Latitude et Longitude coordonne (à l’aide de Bing)

Cet exemple montre comment créer une table basée sur des coordonnées. Cet exemple montre comment utiliser Bing maps et comment inclure votre clé Bing. (Vous pouvez créer une table basée sur des coordonnées à l’aide d’autres moteurs de carte également, sans utiliser une clé Bing).

1. Créez un fichier nommé *MapCoordinates.cshtml* à la racine du site et remplacez le contenu existant avec le code et le balisage suivant :

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. Remplacez `your-key-here` avec la clé Bing Maps que vous avez créé précédemment.
3. Exécutez le *MapCoordinates.cshtml* page, entrez les coordonnées de latitude et longitude, puis cliquez sur le **carte !** disproportionnée. (Si vous ne connaissez pas toutes les coordonnées, essayez ce qui suit. C’est un emplacement sur le campus Microsoft Redmond).

   - Latitude : 47.6781005859375
   - Longitude :-122.158317565918

     La page s’affiche à l’aide des coordonnées que vous avez spécifié.

     ![mappage-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ressources supplémentaires


[Référence de l’API Microsoft.Maps](https://msdn.microsoft.com/library/gg427611.aspx)
