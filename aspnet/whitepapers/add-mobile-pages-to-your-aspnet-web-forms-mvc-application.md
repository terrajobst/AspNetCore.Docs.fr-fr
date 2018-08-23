---
uid: whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
title: 'Comment : Ajouter des Pages mobiles à vos formulaires Web ASP.NET / MVC Application | Microsoft Docs'
author: rick-anderson
description: Cette procédure décrit différentes façons de servir des pages optimisées pour les appareils mobiles à partir de vos pages Web Forms ASP.NET / application MVC et suggère architecturales et...
ms.author: riande
ms.date: 01/20/2011
ms.assetid: 3124f28e-cc32-418a-afe3-519fa56f4c36
msc.legacyurl: /whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
msc.type: content
ms.openlocfilehash: 1693838a74f0564e38e11a2827cceb3d6474677b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832414"
---
<a name="how-to-add-mobile-pages-to-your-aspnet-web-forms--mvc-application"></a>Comment : Ajouter des Pages mobiles à vos formulaires Web ASP.NET / MVC Application
====================
> **S’applique à**
> 
> - Version de Web Forms ASP.NET 4.0
> - ASP.NET MVC version 3.0
> 
> **Résumé**
> 
> Cette procédure décrit différentes façons de servir des pages optimisées pour les appareils mobiles à partir de vos pages Web Forms ASP.NET / application MVC et suggère architecturales et les problèmes à prendre en compte lorsque vous ciblez une large gamme de périphériques de conception. Ce document explique également pourquoi ASP.NET Mobile Controls à partir d’ASP.NET 2.0 à 3.5 sont désormais obsolètes et présente des alternatives modernes.


## <a name="contents"></a>Sommaire

- Vue d'ensemble
- Options architecturales
- Détection de navigateurs et de périphériques
- Comment les applications Web Forms ASP.NET peuvent présenter des pages spécifiques à mobile
- Comment les applications ASP.NET MVC peuvent présenter des pages spécifiques à mobile
- Ressources supplémentaires

Pour obtenir des exemples de code téléchargeables illustrant les techniques de ce livre blanc pour ASP.NET Web Forms et MVC, consultez [Mobile Apps & Sites avec ASP.NET](https://docs.microsoft.com/aspnet/mobile/overview).

## <a name="overview"></a>Vue d'ensemble

Les appareils mobiles – smartphones, téléphones et tablettes – continuent de croître en popularité comme approche visant à accéder au Web. Pour de nombreux développeurs web et orientées sur le web des entreprises, cela signifie qu’il est plus en plus important de fournir une formidable expérience de navigation pour les visiteurs à l’aide de ces appareils.

### <a name="how-earlier-versions-of-aspnet-supported-mobile-browsers"></a>Versions antérieures des navigateurs mobiles ASP.NET pris en charge

Les versions d’ASP.NET 2.0 à 3.5 inclus *ASP.NET Mobile Controls*: un ensemble de contrôles de serveur pour les appareils mobiles dans le *System.Web.Mobile.dll* assembly et le  *System.Web.UI.MobileControls* espace de noms. L’assembly est toujours inclus dans ASP.NET 4, mais il est déconseillé. Les développeurs sont invités à migrer vers des approches plus modernes, tels que ceux décrits dans ce document.

Pourquoi ASP.NET Mobile Controls ont été marqués comme étant obsolète parce que leur conception est axée sur les téléphones mobiles qui étaient courantes autour de 2005 et versions antérieures. Les contrôles sont principalement conçus pour restituer un balisage WML ou cHTML (au lieu de HTML standard) pour les navigateurs WAP de l’époque. Mais WAP, WML et cHTML ne sont plus pertinentes pour la plupart des projets, étant donné que HTML est devenu le langage de balisage omniprésent pour les navigateurs mobiles et de bureau similaires.

### <a name="the-challenges-of-supporting-mobile-devices-today"></a>Les problèmes de prise en charge des appareils mobiles aujourd'hui

Bien que les navigateurs mobiles prennent désormais en charge quasiment tous les HTML, vous toujours pose nombreux défis visant à créer des expériences exceptionnelles de navigation mobiles :

- ***Taille de l’écran*** : appareils mobiles varient considérablement dans le formulaire, et leurs écrans sont souvent beaucoup plus petite que les moniteurs de bureau. Par conséquent, vous devrez peut-être concevoir des dispositions de page complètement différente pour eux.
- ***Entrée des méthodes*** – certains appareils ont les claviers, certains ont des pointes de lecture, d’autres personnes utilisent la fonctionnalité tactile. Vous devrez peut-être envisager de navigation de plusieurs méthodes d’entrée de données et des mécanismes.
- ***Conformité aux normes*** – nombreux navigateurs mobiles ne prennent pas en charge les dernières normes HTML, CSS et JavaScript.
- ***La bande passante*** : performances du réseau données cellulaires varie fortement et certains utilisateurs finaux se trouvent sur les tarifs qui facturent en mégaoctets.

Il n’existe aucune solution uniformisée ; votre application devra ressemblent et se comportent différemment en fonction de l’appareil qui y accède. Selon le niveau de prise en charge mobile que vous souhaitez, cela peut être un défi pour les développeurs web plus grand que le bureau « entre les navigateurs » a toujours été.

Développeurs qui approchent de prise en charge de navigateur mobile pour la première fois souvent initialement pense qu’il est seulement important de prendre en charge peut-être les smartphones plus sophistiquées et plus récents (par exemple, Windows Phone 7, iPhone ou Android), étant donné que les développeurs possèdent souvent personnellement par appareils. Toutefois, les téléphones moins chers sont toujours extrêmement populaires, et leurs propriétaires les utilisez pour parcourir le web – en particulier dans les pays où les téléphones mobiles sont plus faciles à obtenir qu’une connexion haut débit. Votre entreprise devrez décider quelles gamme d’appareils pour prendre en charge en tenant compte de ses clients susceptibles de. Si vous créez une brochure en ligne pour une application spa d’intégrité de luxe, vous pouvez être une décision économique seulement pour cibler des smartphones avancées, tandis que si vous créez un système de réservation de ticket pour le cinéma, vous devez probablement pour prendre en compte pour les visiteurs avec fonctionnalité moins puissante téléphones.

## <a name="architectural-options"></a>Options architecturales

Avant de passer aux détails techniques spécifiques de ASP.NET Web Forms ou MVC, notez que les développeurs web ont en général les trois options principales pour prendre en charge des navigateurs mobiles :

1. ***Ne faites rien :*** vous pouvez simplement créer une application web standard, orienté bureautique et s’appuient sur les navigateurs mobiles pour la rendre de façon acceptable. 

    - **Avantage**: c’est l’option la moins coûteux à implémenter et gérer : des tâches ne supplémentaires
    - **Inconvénient**: offre l’expérience utilisateur final pire : 

        - Les smartphones dernier peut rendre votre code HTML tout aussi bien en tant qu’un navigateur de bureau, mais les utilisateurs seront obligés toujours effectuer un zoom et faire défiler horizontalement et verticalement pour consommer votre contenu sur un petit écran. Ceci est loin d’être optimale.
        - Appareils plus anciens et les téléphones peuvent échouer rendre votre balisage d’une manière satisfaisante.
        - Même sur les périphériques tablette dernière (dont les écrans peuvent être aussi volumineux que les écrans d’ordinateurs portables), interaction différentes règles s’appliquent. Entrées tactiles fonctionnement mieux avec des boutons plus volumineux et des liens spread plus éloigné, et il n’existe aucun moyen de la souris un curseur de la souris au-dessus d’un menu volant.
2. ***Résoudre le problème sur le client* –** avec usage modéré des CSS et [amélioration progressive](http://en.wikipedia.org/wiki/Progressive_enhancement) vous pouvez créer le balisage, les styles et les scripts qui s’adaptent à n’importe quel navigateur les exécute. Par exemple, avec [les requêtes de média CSS 3](http://www.w3.org/TR/2010/CR-css3-mediaqueries-20100727/), vous pouvez créer une disposition qui transforme en une disposition de colonne unique sur les appareils dont les écrans sont plus étroits qu’un seuil défini. 

    - **Avantage**: optimise le rendu de l’appareil en cours d’utilisation, même pour les appareils de futures inconnus en fonction de toutes les caractéristiques d’entrée et écran sont-ils spécifique
    - **Avantage**: facilement vous permet de partager la logique côté serveur sur tous les types d’appareils – duplication minimale de code ou l’effort
    - **Inconvénient**: les appareils mobiles sont donc différents à partir d’appareils de bureau que pourrait vraiment vous vos pages mobiles à être totalement différente de vos pages de bureau, affichant des informations différentes. Ces variations peuvent être inefficaces ou impossible d’atteindre efficacement via CSS seul, surtout en considérant comment incohérente anciens appareils interprètent les règles CSS. Cela est particulièrement vrai pour les attributs de CSS 3.
    - **Inconvénient**: ne fournit aucun support pour différentes logique côté serveur et des flux de travail pour différents appareils. Vous ne peut pas, par exemple, implémenter un workflow simplifié du retrait panier d’achat pour les utilisateurs mobiles au moyen de CSS seul.
    - **Inconvénient**: utilisation de la bande passante inefficace. Serveur vous devrez peut-être transmettre le balisage et les styles qui s’appliquent à tous les appareils possible, même si l’appareil cible utilise uniquement un sous-ensemble de ces informations.
3. ***Résoudre le problème sur le serveur* –** si votre serveur sait quel appareil accède à, ou du moins les caractéristiques de ces périphériques, tels que sa taille d’écran et la méthode d’entrée, et qu’il s’agisse d’un appareil mobile : il peut s’exécuter une logique différente et balisage HTML différent sortie. 

    - **Avantage :** une flexibilité maximale. Il n’existe aucune limite à combien vous pouvez varier votre logique côté serveur pour les mobiles ou optimiser votre balisage pour la disposition souhaitée, spécifique au périphérique.
    - **Avantage :** utilisation efficace de la bande passante. Il vous suffit de vous transmettre le balisage et les informations de style qui va utiliser l’appareil cible.
    - **Inconvénient :** parfois force la répétition d’effort ou de code (par exemple, ce qui vous créer des copies similaires mais légèrement différentes de vos pages Web Forms ou les vues MVC). Où possible que vous serez factoriser logique commune dans une couche sous-jacente ou service, mais toujours, certaines parties de votre code d’interface utilisateur ou de balisage peut-être être dupliqués et de la maintenance en parallèle.
    - **Inconvénient :** détection de l’appareil n’est pas simple. Il requiert une liste ou une base de données de types d’appareils connus et de leurs caractéristiques (qui ne peuvent toujours être parfaitement à jour) et n’est pas garanti pour faire correspondre chaque requête entrante. Ce document décrit certaines options et leurs pièges ultérieurement.

Pour obtenir les meilleurs résultats, la plupart des développeurs trouver que dont ils ont besoin de combiner des options (2) et (3). Des différences mineures stylistiques sont mieux prise en charge sur le client à l’aide de CSS ou même JavaScript, alors que les différences majeures dans le balisage, les flux de travail ou les données sont plus efficacement implémentés dans le code côté serveur.

### <a name="this-paper-focuses-on-server-side-techniques"></a>Ce document se concentre sur les techniques de côté serveur

Dans la mesure où ASP.NET Web Forms et MVC sont les deux technologies principalement côté serveur, ce livre blanc se concentrera sur les techniques de côté serveur qui permettent de générer différents balisages et une logique pour les navigateurs mobiles. Bien sûr, vous pouvez également combiner cela avec n’importe quelle technique côté client (par exemple, CSS 3 les requêtes de média, amélioration progressive JavaScript), mais qui est plus une question de conception web de développement ASP.NET.

## <a name="browser-and-device-detection"></a>Détection de navigateurs et de périphériques

Le composant clé requis pour toutes les techniques de côté serveur pour prendre en charge des appareils mobiles consiste à savoir de quel appareil à l’aide de votre visiteur. En fait, encore mieux que de savoir le nombre de fabricant et modèle de cet appareil est de connaître le *caractéristiques* de l’appareil. Caractéristiques peuvent inclure :

- Il est un appareil mobile ?
- Méthode d’entrée (clavier/souris, tactile, clavier, joystick,...)
- Taille de l’écran (physiquement et en pixels)
- Formats de supports multimédias et données pris en charge
- Et ainsi de suite

Il est préférable de prendre des décisions basées sur les caractéristiques que le numéro de modèle, car, vous serez mieux équipé pour contrôler les appareils.

### <a name="using-aspnets-built-in-browser-detection-support"></a>Utilisation d’ASP. Prise en charge de la détection de navigateur intégré du NET

Les développeurs ASP.NET Web Forms et MVC peuvent découvrir immédiatement des caractéristiques importantes d’un navigateur visite en inspectant les propriétés de la *Request.Browser* objet. Par exemple, consultez

- Request.Browser.IsMobileDevice
- Request.Browser.MobileDeviceManufacturer, Request.Browser.MobileDeviceModel
- Request.Browser.ScreenPixelsWidth
- Request.Browser.SupportsXmlHttp
- ...et beaucoup d’autres

Dans les coulisses, la plate-forme ASP.NET correspond à entrant *Agent utilisateur* en-tête HTTP de (UA) par rapport aux expressions régulières dans un ensemble de fichiers XML de définition de navigateur. Par défaut la plateforme comprend des définitions pour de nombreux périphériques mobiles courants, et vous pouvez ajouter des fichiers de définition de navigateur personnalisés pour d’autres que vous voulez reconnaître. Pour plus d’informations, consultez la page MSDN [contrôles serveur Web ASP.NET et les fonctionnalités du navigateur](https://msdn.microsoft.com/library/x3k2ssx2.aspx).

### <a name="using-the-wurfl-device-database-via-51degreesmobi-foundation"></a>À l’aide de la base de données WURFL appareil via 51Degrees.mobi Foundation

Bien que ASP. Prise en charge de la détection de navigateur intégré du NET sera suffisant pour de nombreuses applications, il existe deux principaux cas lorsqu’il ne peut pas être suffisamment :

- ***Vous souhaitez reconnaître les derniers appareils***(sans création manuelle des fichiers de définition de navigateur pour eux). Notez que les fichiers de définition de navigateur du .NET 4 ne sont pas assez récentes pour reconnaître le Windows Phone 7, les téléphones Android, les navigateurs Opera Mobile ou iPad d’Apple.
- ***Vous avez besoin des informations plus détaillées sur les fonctionnalités de l’appareil***. Vous devrez peut-être savoir sur la méthode d’entrée d’un appareil (par exemple, tactiles vs du pavé numérique) ou quel audio met en forme le navigateur prend en charge. Ces informations n’est pas disponibles dans les fichiers de définition de navigateur standards.

Le [ *Wireless Universal Resource File* projet (WURFL)](http://wurfl.sourceforge.net/) conserve beaucoup plus à jour et détaillées d’informations sur les appareils mobiles en cours d’utilisation dès aujourd'hui.

La bonne nouvelle pour les développeurs .NET est que ASP. Fonctionnalité de détection de navigateur du NET étant extensible, il est possible d’améliorer afin de surmonter ces problèmes. Par exemple, vous pouvez ajouter l’open source [ *51Degrees.mobi Foundation* ](https://github.com/51Degrees/dotNET-Device-Detection) bibliothèque à votre projet. Il s’agit une IHttpModule d’ASP.NET ou d’un navigateur capacités du fournisseur (utilisable sur les applications Web Forms et MVC), qui lit les données WURFL et le raccorde à ASP directement. Mécanisme de détection du navigateur intégré du NET. Une fois que vous avez installé le module, *Request.Browser* contiendra soudain beaucoup plus précises des informations détaillées : il correctement reconnaîtra la plupart des appareils précédemment mentionnés et répertorier leurs capacités (y compris fonctionnalités supplémentaires telles que de la méthode d’entrée). Consultez la documentation du projet pour plus d’informations.

## <a name="how-web-forms-applications-can-present-mobile-specific-pages"></a>Comment les applications Web Forms peuvent présenter des pages spécifiques à mobile

Par défaut, voici comment une toute nouvelle application Web Forms affiche sur les appareils mobiles courants :

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image1.png)

Clairement, ni mise en forme soit très mobile-friendly : cette page a été conçue pour un moniteur de grande taille, en mode paysage, pas pour un petit écran orienté en mode portrait. Par conséquent, que pouvez-vous faire à ce sujet ?

Comme indiqué plus haut dans ce document, il existe de nombreuses façons de personnaliser vos pages pour les appareils mobiles. Certaines techniques sont basées sur serveur, d’autres exécutent sur le client.

### <a name="creating-a-mobile-specific-master-page"></a>Création d’une page maître spécifique mobile

Selon vos besoins, vous pourrez peut-être utiliser le même Web Forms pour tous les visiteurs, mais deux séparer les pages maîtres : un pour les visiteurs du bureau, un autre pour les visiteurs mobiles. Cela vous donne la flexibilité de la modification de la feuille de style CSS ou votre balisage HTML de niveau supérieur en fonction des appareils mobiles, sans vous obliger à dupliquer la logique de page.

Il est facile à réaliser. Par exemple, vous pouvez ajouter un gestionnaire PreInit telle que la suivante à un formulaire Web :

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample1.cs)]

Maintenant, créez une page maître nommée Mobile.Master dans le dossier de niveau supérieur de votre application, et il sera utilisé lors de la détection d’un appareil mobile. Votre page maître mobile peut faire référence à une feuille de style CSS spécifiques à mobile si nécessaire. Visiteurs bureau continueront de voir votre page maître par défaut, pas celui mobile.

### <a name="creating-independent-mobile-specific-web-forms"></a>Création de formulaires de Web mobiles spécifiques indépendants

Pour une flexibilité maximale, vous pouvez aller plus loin que le simple fait de disposer des pages maîtres distincts pour différents types d’appareil. Vous pouvez implémenter deux *totalement séparer les ensembles de pages Web Forms* – un défini pour les navigateurs de bureau, un autre défini pour les mobiles. Cela fonctionne mieux si vous souhaitez présenter des informations très différentes ou des flux de travail aux visiteurs mobiles. Le reste de cette section décrit cette approche en détail.

En supposant que vous disposez déjà d’une application Web Forms conçue pour les navigateurs de bureau, le plus simple pour continuer consiste à créer un sous-dossier appelé « Mobile » au sein de votre projet et créer vos pages mobiles il. Vous pouvez construire un sous-site entière, avec ses propres pages maîtres, les feuilles de style et les pages, en utilisant les mêmes techniques que vous utiliseriez pour toute autre application Web Forms. Vous ne devez pas nécessairement produire un mobile équivalent pour *chaque* page dans votre site de bureau ; vous pouvez choisir de quel sous-ensemble de fonctionnalités de sens pour les visiteurs mobiles.

Vos pages mobiles peuvent partager des ressources de statiques courants (tels que des images, JavaScript ou CSS fichiers) avec vos pages régulières si vous le souhaitez. Étant donné que votre dossier « Mobile » sera *pas* être marquée comme une application distincte quand ils sont hébergés dans IIS (c’est juste un sous-dossier simple des pages Web Forms), il sera également partager les mêmes configuration, les données de Session et les autres infrastructure en tant que votre pages de bureau.

> [!NOTE]
> Étant donné que cette approche implique généralement une duplication de code (pages mobiles sont susceptibles de partager certaines similitudes avec pages bureau), il est important de factoriser toute entreprise logique ou des données accès code commun dans une couche sous-jacente partagée ou un service. Sinon, vous allez double l’effort de création et gestion de votre application.


#### <a name="redirecting-mobile-visitors-to-your-mobile-pages"></a>Redirection des visiteurs mobiles à vos pages mobiles

Il est souvent utile de rediriger les visiteurs mobiles vers les pages mobiles uniquement sur le *premier* demander dans leur session de navigation (et non à chaque requête dans leur session), car :

- Vous pouvez autoriser puis facilement des visiteurs mobiles d’accès à vos pages de bureau s’ils le souhaitent – placez simplement un lien sur votre page maître qui mène à « Version de bureau ». Le visiteur n’être redirigé vers une page mobile, car il n’est plus la première demande dans sa session.
- Il permet d’éviter le risque d’interférence avec des demandes de ressources dynamiques partagées entre les parties mobiles et de bureau de votre site (par exemple, si vous disposez d’un formulaire Web courantes qui affichent des parties de bureau et mobiles de votre site dans un IFRAME, ou que certains gestionnaires d’Ajax)

Pour ce faire, vous pouvez placer votre logique de redirection dans un **Session\_Démarrer** (méthode). Par exemple, ajoutez la méthode suivante à votre fichier Global.asax.cs :

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample2.cs)]

#### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Configuration de l’authentification par formulaire afin de respecter vos pages mobiles

Notez que l’authentification par formulaire fait certaines hypothèses sur où il peut rediriger les visiteurs pendant et après le processus d’authentification :

- Lorsqu’un utilisateur doit être authentifié, l’authentification par formulaire les redirige vers votre page de connexion du bureau, qu’ils soient un utilisateur de bureau ou mobile (car il a seulement un concept de *un* URL de connexion). En supposant que vous souhaitez appliquer un style votre page de connexion mobile différemment, vous avez besoin améliorer votre page de connexion de bureau afin qu’il redirige les utilisateurs mobiles vers une page de connexion de mobile distinct. Par exemple, ajoutez le code suivant à votre **desktop** login page code-behind : 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample3.cs)]
- Une fois un utilisateur se connecte, l’authentification par formulaire sera par défaut les redirige vers votre page d’accueil bureau (car il a seulement un concept de *un* page par défaut). Vous avez besoin améliorer votre page de connexion mobile afin qu’il redirige vers la page d’accueil de mobile après un journal réussie. Par exemple, ajoutez le code suivant à votre **mobile** login page code-behind : 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample4.cs)]
  
  Ce code suppose que votre page comporte un contrôle de serveur de connexion appelé LoginUser, comme dans le modèle de projet par défaut.

### <a name="working-with-output-caching"></a>Utilisation de la mise en cache de sortie

Si vous utilisez la mise en cache de sortie, prenez garde que par défaut qu'il est possible pour un utilisateur du Bureau à visiter une certaine URL (à l’origine de sa sortie doit être mis en cache), suivie d’un utilisateur mobile puis reçoit la sortie de postes de travail mis en cache. Cet avertissement s’applique si vous êtes simplement varying votre page maître par type de périphérique ou l’implémentation de totalement distincts de Web Forms par type d’appareil.

Pour éviter ce problème, vous pouvez demander à ASP.NET pour faire varier l’entrée du cache selon que le visiteur utilise un appareil mobile. Ajoutez un paramètre VaryByCustom à la déclaration de OutputCache de votre page comme suit :

[!code-aspx[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample5.aspx)]

Ensuite, définissez *isMobileDevice* comme un cache personnalisé substituer des paramètre en ajoutant la méthode suivante à votre fichier Global.asax.cs :

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample6.cs)]

Il s’assure que les visiteurs mobiles de la page ne reçoivent des sortie précédemment placée dans le cache par un visiteur de bureau.

### <a name="a-working-example"></a>Un exemple fonctionnel

Pour découvrir ces techniques en action, téléchargez [exemples de code de ce livre blanc](https://docs.microsoft.com/aspnet/mobile/overview). L’exemple d’application Web Forms redirige automatiquement les utilisateurs mobiles à un ensemble de pages mobiles spécifiques dans un sous-dossier appelé Mobile. Le balisage et le style de ces pages est optimisée pour les navigateurs mobiles, comme vous pouvez le constater dans les captures d’écran suivante :

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image2.png)

Pour plus de conseils sur l’optimisation de votre balisage et du CSS pour les navigateurs mobiles, consultez la section « Styling pages mobiles pour les navigateurs mobiles » plus loin dans ce document.

## <a name="how-aspnet-mvc-applications-can-present-mobile-specific-pages"></a>Comment les applications ASP.NET MVC peuvent présenter des pages spécifiques à mobile

Dans la mesure où le modèle Model-View-Controller dissocie la logique d’application (dans les contrôleurs) de la logique de présentation (dans les vues), vous pouvez choisir à partir d’une des approches suivantes pour gérer la prise en charge mobile dans le code côté serveur :

1. ***Utiliser les contrôleurs et les vues même pour les navigateurs de bureau et mobiles, mais afficher les vues différentes mises en page Razor selon le type de périphérique *.** Cette option fonctionne mieux si vous affichez des données identiques sur tous les appareils, mais que vous souhaitez simplement fournir différentes feuilles de style CSS ou de modifier quelques éléments HTML de niveau supérieur pour les mobiles.
2. ***Utiliser les contrôleurs de mêmes pour les navigateurs de bureau et mobiles, mais en effectuer le rendu des vues différentes selon le type d’appareil***. Cette option fonctionne mieux si vous êtes afficher à peu près les mêmes données et de fournir aux mêmes workflows pour les utilisateurs finaux, mais souhaitez restituer un balisage HTML très différent en fonction de l’appareil utilisé.
3. ***Créer des zones séparées pour les navigateurs de bureau et mobiles, mise en œuvre des contrôleurs indépendants et les vues pour chaque *.** Cette option fonctionne mieux si vous êtes affichage très différents écrans, contenant des informations différentes et début de l’utilisateur à travers différents flux de travail optimisé pour les types de périphériques. Cela peut signifier une répétition de code, mais vous pouvez réduire qui en factorisant logique commune dans une couche ou un service sous-jacent.

Si vous souhaitez prendre le **premier** option et modifier uniquement la mise en page Razor par type d’appareil, il est très facile. Il suffit de modifier votre \_ViewStart.cshtml de fichiers comme suit :

[!code-cshtml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample7.cshtml)]

Vous pouvez désormais créer une disposition mobile appelée \_LayoutMobile.cshtml avec une structure de page et CSS règles optimisé pour les appareils mobiles.

Si vous souhaitez prendre le **deuxième** option rendu des vues totalement différente en fonction du type d’appareil du visiteur, consultez [billet de blog de Scott Hanselman](http://www.hanselman.com/blog/ABetterASPNETMVCMobileDeviceCapabilitiesViewEngine.aspx).

Le reste de ce document se concentre sur la **troisième** option-Création des contrôleurs distincts *et* vues pour les appareils mobiles, afin de contrôler exactement quel sous-ensemble de fonctionnalités est disponible pour les visiteurs mobiles.

### <a name="setting-up-a-mobile-area-within-your-aspnet-mvc-application"></a>Configuration d’une zone Mobile au sein de votre application ASP.NET MVC

Vous pouvez ajouter une zone appelée « Mobile » pour une application ASP.NET MVC de façon normale : avec le bouton droit sur le nom de votre projet dans l’Explorateur de solutions, puis cliquez sur Ajouter à zone. Vous pouvez ensuite ajouter des contrôleurs et des vues comme vous le feriez pour n’importe quel autre zone dans une application ASP.NET MVC. Par exemple, ajouter à votre zone Mobile un nouveau contrôleur appelé HomeController pour agir en tant que page d’accueil pour les visiteurs mobiles.

### <a name="ensuring-the-url-mobile-reaches-the-mobile-homepage"></a>En garantissant la /Mobile URL atteint la page d’accueil mobile

Si vous souhaitez que l’URL /Mobile pour atteindre l’action de l’Index sur HomeController à l’intérieur de votre zone Mobile, vous devez apporter deux modifications mineures à votre configuration de routage. Tout d’abord, mettez à jour votre classe MobileAreaRegistration afin que HomeController est le contrôleur par défaut dans votre région Mobile, comme indiqué dans le code suivant :

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample8.cs)]

Cela signifie que la page d’accueil mobile maintenant se trouve à /Mobile, plutôt que/Mobile/Home, car « Home » est désormais l’implicitement les nom de contrôleur par défaut pour les pages mobiles.

Ensuite, notez qu’en ajoutant un deuxième HomeController à votre application (par exemple, mobile celle qui, en plus un bureau existant), vous allez avez interrompu votre page d’accueil de postes de travail régulière. Elle échoue avec l’erreur «*plusieurs types ont été trouvés, qui correspondent le contrôleur nommé « Accueil »*». Pour résoudre ce problème, vous devez mettre à jour votre configuration de routage de niveau supérieur (dans Global.asax.cs) pour spécifier que votre bureau HomeController doit être prioritaires lorsqu’il existe une ambiguïté :

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample9.cs)]

Maintenant l’erreur passera et l’URL http://<em>votresite</em>/ atteindra la page d’accueil de bureau et http://<em>yoursite</em>/mobile/ va atteindre la page d’accueil mobile.

### <a name="redirecting-mobile-visitors-to-your-mobile-area"></a>Redirection des visiteurs mobiles à votre zone mobile

Il existe plusieurs points d’extensibilité différents dans ASP.NET MVC, donc il existe de nombreuses méthodes possibles pour injecter la logique de redirection. Une option intéressante consiste à créer un attribut de filtre, tel que [RedirectMobileDevicesToMobileArea], qui effectue une redirection si les conditions suivantes sont remplies :

1. Il est la première requête dans la session utilisateur (par exemple, Session.IsNewSession est égal à true)
2. La demande provient d’un navigateur mobile (par exemple, Request.Browser.IsMobileDevice est égal à true)
3. L’utilisateur n’est pas déjà demandant une ressource dans la zone mobile (par exemple, le *chemin d’accès* partie de l’URL ne commence pas par /Mobile)

L’exemple téléchargeable accompagnant ce livre blanc inclut une implémentation de cette logique. Il est implémenté comme un filtre d’autorisation, dérivé de AuthorizeAttribute, ce qui signifie qu’il puisse fonctionner correctement même si vous utilisez la mise en cache de sortie (dans le cas contraire, si un accès de première visiteur bureau certaine une URL, la sortie de postes de travail peuvent être mis en cache et les prise en charge pour visiteurs mobiles suivantes).

Comme il s’agit d’un filtre, vous pouvez choisir d’appliquer à des contrôleurs spécifiques et des actions, par exemple,

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample10.cs)]

… ou vous pouvez l’appliquer à tous les contrôleurs et les actions comme un MVC 3 *filtre global* dans votre fichier Global.asax.cs :

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample11.cs)]

L’exemple téléchargeable montre également comment vous pouvez créer des sous-classes de cet attribut qui redirigent vers des emplacements spécifiques au sein de votre zone mobile. Cela signifie, par exemple, que vous pouvez :

- Inscrire un filtre global comme indiqué ci-dessus qui envoie des visiteurs mobiles à la page d’accueil mobile par défaut.
- Également appliquer un filtre de [RedirectMobileDevicesToMobileProductPage] spéciaux à une action « Afficher le produit » qui accepte les visiteurs mobiles à la version mobile de n’importe quelle page de produit qu’ils avaient demandé.
- S’appliquent également autres sous-classes spéciales du filtre à des actions spécifiques, en redirigeant les visiteurs mobiles vers la page mobile équivalente

### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Configuration de l’authentification par formulaire afin de respecter vos pages mobiles

Si vous utilisez l’authentification par formulaire, vous devez noter que lorsqu’un utilisateur doit se connecter, il redirige automatiquement l’utilisateur à une seule « session » URL spécifique, qui est par défaut **/compte / d’ouverture de session**. Cela signifie que les utilisateurs mobiles peuvent être redirigés vers votre action bureau « connexion ».

Pour éviter ce problème, ajouter une logique à votre bureau action « session » afin qu’il redirige les utilisateurs mobiles à nouveau à une action mobile « connexion ». Si vous utilisez la valeur par défaut du modèle d’application ASP.NET MVC, action mettre à jour du contrôle AccountController d’ouverture de session comme suit :

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample12.cs)]

… puis implémentez une action de « connexion » spécifiques à mobile approprié sur un contrôleur appelé AccountController dans votre zone Mobile.

### <a name="working-with-output-caching"></a>Utilisation de la mise en cache de sortie

Si vous utilisez le filtre [OutputCache], vous devez forcer l’entrée de cache à varient selon le type d’appareil. Par exemple, écrire :

[!code-javascript[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample13.js)]

Ensuite, ajoutez la méthode suivante à votre fichier Global.asax.cs :

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample14.cs)]

Il s’assure que les visiteurs mobiles de la page ne reçoivent des sortie précédemment placée dans le cache par un visiteur de bureau.

### <a name="a-working-example"></a>Un exemple fonctionnel

Pour découvrir ces techniques en action, téléchargez [code de ce livre blanc associé exemples](https://docs.microsoft.com/aspnet/mobile/overview). L’exemple inclut une application ASP.NET MVC 3 (Release Candidate) améliorée pour prendre en charge des appareils mobiles à l’aide des méthodes décrites ci-dessus.

## <a name="further-guidance-and-suggestions"></a>Autres conseils et suggestions

La discussion suivante s’applique à la fois aux Web Forms et les développeurs MVC qui utilisent les techniques décrites dans ce document.

### <a name="enhancing-your-redirection-logic-using-51degreesmobi-foundation"></a>Amélioration de votre logique de redirection à l’aide de 51Degrees.mobi Foundation

La logique de redirection indiquée dans ce document peut être tout à fait suffisante pour votre application, mais elle ne fonctionnera pas si vous avez besoin désactiver des sessions, soit avec les navigateurs mobiles qui rejettent les cookies (il ne peut pas avoir de sessions), car il ne sait pas si une requête donnée la première condition à partir de ce visiteur.

Vous avez appris déjà comment le 51Degrees.mobi open source Foundation peut améliorer la précision de ASP. Détection du navigateur du NET. Il a également une fonctionnalité intégrée pour rediriger les visiteurs mobiles à des emplacements spécifiques configurés dans le fichier Web.config. Il est en mesure de fonctionner sans selon les Sessions ASP.NET (et par conséquent, les cookies) en stockant un journal temporaire des hachages des en-têtes HTTP et les adresses IP de visiteurs, par conséquent, il sait ou non chaque requête est la première à partir d’un visiteur donné.

L’élément suivant est ajouté à la section fiftyOne du fichier web.config redirige la première demande à partir d’un appareil mobile détecté à la page ~ / Mobile/Default.aspx. Toutes les demandes vers les pages du dossier Mobile seront *pas* redirigé, quel que soit le type d’appareil. Si le périphérique mobile a été inactif pendant une période de 20 minutes ou plus de l’appareil seront oubliées et les demandes suivantes seront traités en tant que nouveaux dans le cadre de la redirection.

[!code-xml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample15.xml)]

Pour plus d’informations, consultez [51degrees.mobi documentation Foundation](https://github.com/51Degrees/dotNET-Device-Detection).

> [!NOTE]
> Vous *pouvez* fonctionnalité de redirection de la Fondation utilisation 51Degrees.mobi sur les applications ASP.NET MVC, mais vous devez définir votre configuration de redirection en termes d’URL simples, pas en termes de paramètres de routage ou en plaçant les filtres MVC sur les actions. Il s’agit, car (au moment de la rédaction) 51Degrees.mobi Foundation ne reconnaît pas les filtres ou routage.


### <a name="disabling-transcoders-and-proxy-servers"></a>La désactivation des transcodeurs et les serveurs Proxy

Opérateurs de réseau mobile ont deux objectifs large dans leur approche internet mobile :

1. Fournir en tant que le contenu approprié autant que possible
2. Maximiser le nombre de clients qui peuvent partager la bande passante limitée radio

Étant donné que la plupart des pages web ont été conçus pour les grands écrans en taille réelle de bureau et les connexions haut débit rapide-ligne fixe, de nombreux opérateurs utilisent *transcodeurs* ou *serveurs proxy* qui modifier dynamiquement le contenu web. Ils peuvent modifier votre balisage HTML ou les CSS en fonction des écrans plus petits (en particulier pour « téléphones » qui ne disposent pas de la puissance de traitement pour gérer des dispositions complexes), et ils peuvent recompresser vos images (vous réduisez considérablement leur qualité) pour améliorer les vitesses de remise de page.

Mais si vous avez pris l’effort nécessaire pour produire une version optimisés pour mobile de votre site, vous ne souhaitez probablement l’opérateur de réseau à interférer avec lui les autres. Vous pouvez ajouter la ligne suivante à la Page\_événement de chargement dans n’importe quel formulaire de Web ASP.NET :

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample16.cs)]

Ou, pour un contrôleur ASP.NET MVC, vous pouvez ajouter la substitution de méthode suivante pour qu’il s’applique à toutes les actions sur ce contrôleur :

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample17.cs)]

Le message HTTP résultant informe transcodeurs conformes W3C et les proxys ne pas de modifier un contenu. Bien sûr, il n’y a aucune garantie que les opérateurs de réseau mobile respectera ce message.

### <a name="styling-mobile-pages-for-mobile-browsers"></a>Style des pages mobiles pour les navigateurs mobiles

N’entre pas dans le cadre de ce document pour décrire en détail quels types de travail de balisage HTML correctement ou que les techniques de conception web optimiser la facilité d’utilisation sur des appareils. Il a à vous permet de trouver une disposition suffisamment simple, optimisé pour un écran en taille réelle mobile, sans utiliser non fiables HTML ou des astuces de positionnement CSS. Une technique importante intéressant de mentionner, cependant, est la *balise méta du viewport*.

Certains navigateurs mobiles modernes, dans un effort affichage des pages web destiné à des moniteurs de bureau, restituent la page sur un canevas virtuel, également appelée « fenêtre d’affichage » (par exemple, la fenêtre d’affichage virtuel est 980 pixels de large sur iPhone et 850 pixels de larges sur Opera Mobile par défaut), puis mettre à l’échelle le résultat vers le bas pour tenir sur pixels physiques de l’écran. L’utilisateur peut ensuite faire un zoom et un panoramique autour de cette fenêtre d’affichage. Cela présente l’avantage qu’il permet au navigateur pour afficher la page dans sa disposition prévue, mais il est également présente un inconvénient : qu’il force le zoom et le panoramique, qui n’est pas pratique pour l’utilisateur. Si vous créez pour mobile, il est préférable de conception pour un écran étroit afin qu’aucun zoom ou le défilement horizontal n’est nécessaire.

Indiquer le navigateur mobile à la largeur la fenêtre d’affichage doit être est via le non standard *viewport* balise meta. Par exemple, si vous ajoutez le code suivant à la section HEAD de votre page,

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample18.html)]

… prise en charge des navigateurs de smartphone puis disposer de la page sur un canevas large virtuel 480 pixels. Cela signifie que si vos éléments HTML définissent leurs largeurs en termes de pourcentage, les pourcentages sont interprétés en ce qui concerne cette largeur de 480 pixels, pas la largeur de la fenêtre d’affichage par défaut. Par conséquent, l’utilisateur est moins susceptible d’avoir à zoomer et faire défiler horizontalement – considérablement améliorer l’expérience de navigation mobile.

Si vous souhaitez que la largeur de la fenêtre d’affichage pour faire correspondre les pixels physiques de l’appareil, vous pouvez spécifier les éléments suivants :

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample19.html)]

Pour que cela fonctionne correctement, vous devez forcer pas explicitement les éléments de dépasser cette largeur (par exemple, en utilisant un *largeur* attribut ou propriété CSS), sinon le navigateur est contraint d’utiliser une plus grande fenêtre d’affichage, quel que soit. Voir aussi : [plus de détails sur la balise viewport non standard](https://developer.apple.com/library/safari/#documentation/AppleApplications/Reference/SafariHTMLRef/Articles/MetaTags.html).

Prise en charge de la plupart des smartphones *orientation double*: ils peuvent être conservées en mode portrait ou paysage. Par conséquent, il est important de ne pas faire d’hypothèses concernant la largeur d’écran en pixels. Même dites-vous que la largeur d’écran est fixe, car l’utilisateur permettre réorienter leur appareil pendant qu’elles sont sur votre page.

Les appareils Windows Mobile et Blackberry plus anciens peuvent également accepter les balises de métadonnées suivantes dans l’en-tête de page pour indiquer leur contenu a été optimisé pour les mobiles et ne doivent donc pas être transformés.

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample20.html)]

## <a name="additional-resources"></a>Ressources supplémentaires

Pour obtenir la liste des émulateurs de périphérique mobile et de simulateurs, vous pouvez utiliser pour tester votre application de web mobile ASP.NET, consultez la page [simuler des appareils mobiles courants pour le test](../mobile/device-simulators.md).

## <a name="credits"></a>Crédits

- Auteur principal : Steven Sanderson
- Réviseurs / autres rédacteurs de contenu : James Rosewell, Mikael Söderström, Scott Hanselman, Scott Hunter
