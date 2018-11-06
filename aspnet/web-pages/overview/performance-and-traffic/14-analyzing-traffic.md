---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: Suivi des informations sur les visiteurs (Analytique) pour un ASP.NET Web Pages (Razor) Site | Microsoft Docs
author: Rick-Anderson
description: Une fois que vous avez obtenu votre site Web accédant, vous souhaiterez peut-être analyser le trafic de votre site Web.
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 57e6a0d4681f147faa5e9ca3b6ed0ef287d6a381
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020882"
---
<a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a>Informations de suivi visiteur (Analytique) pour un Site ASP.NET Web Pages (Razor)
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article décrit comment utiliser une application d’assistance pour ajouter l’analytique de site Web vers des pages dans un site Web ASP.NET Web Pages (Razor).
> 
> Ce que vous allez apprendre :
> 
> - Comment envoyer des informations sur le trafic de votre site Web à un fournisseur d’analytique.
> 
> Il s’agit de la programmation des fonctionnalités introduites dans l’article ASP.NET :
> 
> - Le `Analytics` helper.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - ASP.NET Web Helpers Library (package NuGet)


Analytique est un terme général qui désigne la technologie qui mesure le trafic sur votre site Web pour vous pouvez de comprendre comment les personnes utilisent le site. De nombreux services d’analytique sont disponibles, y compris les services à partir de Google, Yahoo, StatCounter et autres.

L’analytique de façon fonctionne est que vous vous inscrivez pour un compte avec le fournisseur d’analytique, dans lequel vous inscrivez le site que vous souhaitez effectuer le suivi. Le fournisseur envoie un extrait de code JavaScript qui inclut un ID ou le code de votre compte de suivi. Vous ajoutez l’extrait de code JavaScript pour les pages web sur le site que vous souhaitez suivre. (Vous ajoutez généralement l’extrait de code analytique à une page de disposition ou de pied de page ou autre balisage HTML qui s’affiche sur chaque page de votre site.) Lorsque des utilisateurs demandent une page qui contient l’un de ces extraits de code JavaScript, l’extrait de code envoie des informations sur la page actuelle pour le fournisseur d’analytique, qui enregistre des détails différents sur la page.

Lorsque vous souhaitez consulter les statistiques de votre site, vous connecter au site Web du fournisseur de l’analytique. Vous pouvez ensuite afficher toutes sortes de rapports sur votre site, telles que :

- Le nombre d’affichages de page pour des pages individuelles. Cela vous indique (à peu près) nombre de personnes qui visitent le site et les pages de votre site sont les plus populaires.
- La durée pendant laquelle les employés passent sur des pages spécifiques. Cela peut vous indiquer éléments tels que si votre page d’accueil consiste à conserver un intérêt populaire.
- Quelles personnes des sites ont été sur avant que les visiteurs de votre site. Cela vous permet de comprendre si votre trafic provient des liens, à partir de recherches et ainsi de suite.
- Lorsque les utilisateurs visitent votre site et la durée pendant laquelle elles restent.
- Les pays dans lesquels vos visiteurs proviennent.
- Quels systèmes d’exploitation et navigateurs utilisent vos visiteurs.

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a>À l’aide d’un programme d’assistance pour ajouter Analytique à une Page

Les Pages Web ASP.NET inclut plusieurs programmes d’assistance analytique (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, et `Analytics.GetStatCounterHtml`) qui la rendent facile à gérer les extraits de code JavaScript utilisés pour l’analytique. Au lieu d’essayer de comprendre comment et où placer le code JavaScript, vous avez à faire d’ajouter l’application d’assistance à une page. La seule information que vous devez fournir est votre nom de compte, un ID ou un code de suivi. (Pour StatCounter, vous devez également fournir quelques valeurs supplémentaires.)

Dans cette procédure, vous allez créer une page de disposition qui utilise le `GetGoogleHtml` helper. Si vous avez déjà un compte avec l’un des autres fournisseurs analytique, vous pouvez utiliser ce compte au lieu de cela et ajuster légèrement en fonction des besoins.

> [!NOTE]
> Lorsque vous créez un compte analytique, vous inscrivez l’URL du site que vous souhaitez suivre. Si vous testez tous les éléments sur votre ordinateur local, vous ne suivi trafic réelle (seul le trafic est à vous), donc vous ne pourrez pas à enregistrer et voir les statistiques de site. Mais cette procédure montre comment vous ajoutez une application d’assistance analytique à une page. Lorsque vous publiez votre site, le site actif enverra des informations à votre fournisseur d’analytique.


1. Ajoutez la bibliothèque de programmes d’assistance de Web ASP.NET à votre site Web, comme décrit dans [l’installation des programmes d’assistance dans un Site ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si vous n’avez pas déjà ajouté.
2. Créer un compte avec Google Analytique et enregistrez le nom du compte.
3. Créez une page de disposition nommée *Analytics.cshtml* et ajoutez le balisage suivant :

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > Vous devez placer l’appel à la `Analytics` helper dans le corps de votre page web (avant le `</body>` balise). Sinon, le navigateur n’exécute le script.

    Si vous utilisez un fournisseur d’analytique différents, utilisez un des programmes d’assistance suivants :

    - (Yahoo) `@Analytics.GetYahooHtml("myaccount")`
    - (StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`
4. Remplacez `myaccount` par le nom du compte, ID ou du code de suivi que vous avez créé à l’étape 1.
5. Exécutez la page dans le navigateur. (Assurez-vous que la page est sélectionnée dans le **fichiers** espace de travail avant de l’exécuter.)
6. Dans le navigateur, affichez la source de la page. Vous serez en mesure de voir le code de rendu analytique :

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. Ouvrez une session sur le site Google Analytique et examiner les statistiques de votre site. Si vous utilisez la page sur un site de production, vous verrez d’entrée qui enregistre la visite à votre page.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

- [Site de Google Analytique](https://www.google.com/analytics/)
- [Yahoo! Site de Web Analytics](http://help.yahoo.com/l/us/yahoo/ywa/)
- [Site de StatCounter](http://statcounter.com/)
