---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Installation d’un Helper dans une application Web Pages (Razor) Site | Microsoft Docs
author: tfitzmac
description: Cet article décrit comment installer une application auxiliaire dans un site Web ASP.NET Web Pages (Razor). Une application d’assistance est un composant réutilisable qui inclut le code et la balise par...
ms.author: aspnetcontent
ms.date: 02/18/2014
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: db3dff9f2d70577bb0618335c0100b9899e87727
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838612"
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a>Installation d’un Helper dans un Site ASP.NET Web Pages (Razor)
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article décrit comment installer une application auxiliaire dans un site Web ASP.NET Web Pages (Razor). Un *helper* est un composant réutilisable qui inclut le code et le balisage pour effectuer une tâche qui peut être fastidieux ou complexes.
> 
> Ce que vous allez apprendre :
> 
> - Comment installer une application auxiliaire dans un site Web créé à l’aide de WebMatrix 3.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
> 
> 
> - WebMatrix 3


## <a name="overview-of-helpers"></a>Vue d’ensemble des programmes d’assistance

Certaines tâches que les gens souhaitent souvent à faire sur les pages web ont besoin de beaucoup de code ou requièrent des connaissances supplémentaires. Exemples incluent l’affichage d’un graphique pour les données ; placer un bouton « Suivre » de Twitter sur une page ; envoi d’e-mails à partir de votre site Web ; rognage ou redimensionnement d’images ; à l’aide de PayPal pour votre site. Pour le rendre facile à réaliser ce types d’éléments différents, les Pages Web ASP.NET vous permet d’utiliser *helpers*. Programmes d’assistance sont des composants que vous installez pour un site et qui permettent de vous effectuent les tâches standard en utilisant simplement une ou deux lignes de code Razor.

Les Pages Web ASP.NET a quelques helpers intégrés. Toutefois, nombreux helpers sont disponibles dans les packages (compléments) qui sont fournies à l’aide du Gestionnaire de package NuGet. NuGet vous permet de sélectionner un package à installer, et il s’occupe ensuite de tous les détails de l’installation.

## <a name="installing-a-helper-in-webmatrix-3"></a>Installation d’un Helper dans WebMatrix 3

1. Dans WebMatrix 3, cliquez sur le **NuGet** bouton.

    ![Boîte de dialogue galerie NuGet dans WebMatrix](installing-helpers/_static/image1.png)
2. Cela lance le Gestionnaire de package NuGet et affiche les packages disponibles. Dans la zone de recherche, entrez un mot clé pour l’application d’assistance que vous souhaitez installer.

    ![Boîte de dialogue galerie NuGet dans WebMatrix](installing-helpers/_static/image2.png)
3. Sélectionnez le package, puis cliquez **installer**. Cliquez sur **Oui** lorsque vous êtes invité à installer le package et d’indiquer que vous acceptez les termes du contrat.

     S’il s’agit de la première fois que vous avez installé une application d’assistance, NuGet crée des dossiers dans votre site Web pour le code de l’application d’assistance.
4. Pour désinstaller un programme d’assistance, cliquez sur le **galerie** bouton, cliquez sur le **installé** onglet et sélectionnez le package que vous souhaitez désinstaller.

## <a name="installing-the-twitter-helper"></a>L’installation du programme d’assistance de Twitter

La dernière version de l’API Twitter n’est pas compatible avec l’application d’assistance Twitter que vous installez via NuGet. Au lieu de cela, consultez le [application auxiliaire Twitter avec WebMatrix](twitter-helper.md) rubrique pour plus d’informations sur la configuration du programme d’assistance de Twitter dans votre projet.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ressources supplémentaires


[Présentation d’ASP.NET Web Pages 2 - notions de programmation](../getting-started/introducing-razor-syntax-c.md)

[Helper Twitter avec WebMatrix](twitter-helper.md)
