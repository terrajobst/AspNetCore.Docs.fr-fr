---
uid: mvc/overview/older-versions/aspnet-mvc-4-mobile-features
title: Fonctionnalités mobiles d’ASP.NET MVC 4 | Microsoft Docs
author: Rick-Anderson
description: Il existe désormais une version de MVC 5 de ce didacticiel avec des exemples de code à déployer une Application Web de Mobile dans ASP.NET MVC 5 sur les Sites Web Azure.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 27dc4fc8-1b51-43b0-933f-fc1b52476523
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/aspnet-mvc-4-mobile-features
msc.type: authoredcontent
ms.openlocfilehash: 5b029aa7e87f064622d72feacaf7e97ea4da5cca
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384814"
---
<a name="aspnet-mvc-4-mobile-features"></a>Fonctionnalités mobiles ASP.NET MVC 4
====================
par [Rick Anderson](https://github.com/Rick-Anderson)

> Il existe désormais une version de MVC 5 de ce didacticiel avec des exemples de code à [déployer une Application Web de Mobile dans ASP.NET MVC 5 sur les Sites Web Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/).


Ce didacticiel vous apprend les notions de base de l’utilisation des fonctionnalités mobiles dans une application Web ASP.NET MVC 4. Pour ce didacticiel, vous pouvez utiliser [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) ou Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer ou VWD&quot;). Vous pouvez utiliser la version professionnelle de Visual Studio si vous l’avez déjà.

Avant de commencer, assurez-vous que vous avez installé les composants requis listés ci-dessous.

- [Visual Studio 2012 Express](https://www.microsoft.com/visualstudio/11/products/express) (recommandé) ou Visual Studio Web Developer Express SP1. Visual Studio 2012 contient ASP.NET MVC 4. Si vous utilisez Visual Web Developer 2010, vous devez installer [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392).

Vous aurez également besoin d’un émulateur de navigateur mobile. Les éléments suivants ne fonctionnent pas :

- [Émulateur de téléphone Windows 7](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). (Il s’agit de l’émulateur qui est utilisé dans la plupart des captures d’écran dans ce didacticiel.)
- Modifiez la chaîne d’agent utilisateur pour émuler un iPhone. Consultez [cela](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) entrée de blog.
- [Émulateur Mobile Opera](http://www.opera.com/developer/tools/mobile/)
- [Apple Safari](http://www.apple.com/safari/download/) avec l’agent utilisateur défini sur iPhone. Pour obtenir des instructions sur la configuration de l’agent utilisateur dans Safari pour « iPhone », consultez [comment permettre à Safari prétendre qu’il est IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) sur le blog de David Alison.

Projets Visual Studio avec code source c# sont disponibles pour accompagner cette rubrique :

- [Téléchargement du projet de départ](https://go.microsoft.com/fwlink/?linkid=228307&amp;clcid=0x409)
- [Téléchargement du projet terminé](https://go.microsoft.com/fwlink/?linkid=228306&amp;clcid=0x409)

### <a name="what-youll-build"></a>Ce que vous allez générer

Pour ce didacticiel, vous allez ajouter des fonctionnalités mobiles à l’application de listes de conférence simple qui est fournie dans le [projet de démarrage](https://go.microsoft.com/fwlink/?LinkId=228307). La capture d’écran suivante montre la page de balises de l’application terminée, comme indiqué dans le [émulateur de téléphone Windows 7](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). Consultez [clavier mappage pour Windows Phone Emulator](https://msdn.microsoft.com/library/ff754352(v=vs.92).aspx) pour simplifier l’entrée au clavier.

[![p1_Tags_CompletedProj](aspnet-mvc-4-mobile-features/_static/image2.png)](aspnet-mvc-4-mobile-features/_static/image1.png)

Vous pouvez utiliser Internet Explorer version 9 ou 10, FireFox ou Chrome pour développer votre application mobile en définissant le [chaîne d’agent utilisateur](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/). L’illustration suivante montre la fin du didacticiel à l’aide d’Internet Explorer émulant un iPhone. Vous pouvez utiliser les outils de développement Internet Explorer F-12 et [outil Fiddler](http://www.fiddler2.com/fiddler2/) pour aider à déboguer votre application.

![](aspnet-mvc-4-mobile-features/_static/image3.png)

### <a name="skills-youll-learn"></a>Vous allez apprendre des compétences

Voici ce que vous allez apprendre :

- Comment les modèles ASP.NET MVC 4 utilisent HTML5 `viewport` attribut et rendu adaptatif pour améliorer les affichent sur les appareils mobiles.
- Comment créer des vues mobiles spécifiques.
- Comment créer un sélecteur de vue qu’Activer/désactiver des utilisateurs permet entre un affichage mobile et une vue de bureau de l’application.

### <a name="getting-started"></a>Prise en main

Télécharger l’application de listes de conférence pour le projet de démarrage à l’aide du lien suivant : [télécharger](https://go.microsoft.com/fwlink/?LinkId=228307). Puis, dans l’Explorateur Windows, cliquez sur le *MvcMobile.zip* de fichier et choisissez **propriétés**. Dans le **MvcMobile.zip propriétés** boîte de dialogue, sélectionnez le **Unblock** bouton. (Le déblocage empêche un avertissement de sécurité qui se produit lorsque vous essayez d’utiliser un *.zip* fichier que vous avez téléchargé à partir du web.)

![p1_unBlock](aspnet-mvc-4-mobile-features/_static/image4.png)

Avec le bouton droit le *MvcMobile.zip* fichier et sélectionnez **extraire tout** pour décompresser le fichier. Dans Visual Studio, ouvrez le *MvcMobile.sln* fichier.

Appuyez sur CTRL + F5 pour exécuter l’application, qui affiche dans votre navigateur de bureau. Démarrez votre émulateur de navigateur mobile, copiez l’URL de l’application de conférence dans l’émulateur, puis cliquez sur le **Parcourir par balise** lien. Si vous utilisez l’émulateur Windows Phone, cliquez dans la barre d’adresse et appuyez sur la touche Pause pour obtenir l’accès au clavier. L’image ci-dessous montre le *AllTags* vue (choisisse **Parcourir par balise**).

[![p1_browseTag](aspnet-mvc-4-mobile-features/_static/image6.png)](aspnet-mvc-4-mobile-features/_static/image5.png)

L’affichage est très lisible sur un appareil mobile. Cliquez sur le lien ASP.NET.

[![p1_tagged_ASPNET](aspnet-mvc-4-mobile-features/_static/image8.png)](aspnet-mvc-4-mobile-features/_static/image7.png)

La vue de la balise ASP.NET est très encombrée. Par exemple, le **Date** colonne est très difficile à lire. Plus loin dans ce didacticiel, vous allez créer une version de la *AllTags* vue qui est conçue spécifiquement pour les navigateurs mobiles et qui est compréhensible l’affichage.

Remarque : Un bogue existe actuellement dans le moteur de mise en cache mobile. Pour les applications de production, vous devez installer le [DisplayModes fixe](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) package nugget. Consultez [ASP.NET MVC 4 Mobile la mise en cache bogue fixe](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) pour plus d’informations sur le correctif.

## <a name="css-media-queries"></a>Requêtes de média CSS

[Requêtes de média CSS](http://www.w3.org/TR/css3-mediaqueries/) sont une extension pour les types de média CSS. Ils permettent de créer des règles qui remplacent les règles CSS par défaut pour des navigateurs spécifiques (agents utilisateurs). Une règle commune pour CSS qui cible les navigateurs mobiles consiste à définir la taille d’écran maximale. Le *Content\Site.css* fichier est créé lorsque vous créez un nouveau projet ASP.NET MVC 4 Internet contenait la requête de média suivants :

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample1.css)]

Si la fenêtre du navigateur est 850 pixels de larges ou moins, il utilisera les règles CSS à l’intérieur de ce bloc de média. Vous pouvez utiliser des requêtes de média CSS comme suit pour fournir un meilleur affichage de contenu HTML sur petits navigateurs (comme les navigateurs mobiles) que les règles CSS par défaut qui sont conçus pour les affichages plus large des navigateurs de bureau.

## <a name="the-viewport-meta-tag"></a>La balise Meta de la fenêtre d’affichage

Les navigateurs mobiles plus définissent une largeur de fenêtre de navigateur virtuel (le *viewport*) qui est largement supérieur à la largeur réelle de l’appareil mobile. Ainsi, les navigateurs mobiles en fonction de la page web entière à l’intérieur de l’écran virtuel. Les utilisateurs peuvent ensuite effectuer un zoom avant sur contenu intéressant. Toutefois, si vous définissez la largeur de la fenêtre d’affichage de la largeur de l’appareil réel, aucun zoom n’est requise, car le contenu tient dans le navigateur mobile.

La fenêtre d’affichage `<meta>` balise dans le fichier de disposition ASP.NET MVC 4 définit la fenêtre d’affichage de la largeur de l’appareil. La ligne suivante montre la fenêtre d’affichage `<meta>` balise dans le fichier de disposition ASP.NET MVC 4.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample2.html)]

## <a name="examining-the-effect-of-css-media-queries-and-the-viewport-meta-tag"></a>Examen de l’effet de la balise Meta de la fenêtre d’affichage et de requêtes de média CSS

Ouvrez le *Views\Shared\\_Layout.cshtml* fichier dans l’éditeur et le commentaire de la fenêtre d’affichage `<meta>` balise. Le balisage suivant montre la ligne commentée.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample3.cshtml)]

Ouvrez le *MvcMobile\Content\Site.css* dans l’éditeur de fichiers et de modifier la largeur maximale dans la requête de média à zéro pixel. Cela empêchera les règles CSS d’être utilisés dans les navigateurs mobiles. La ligne suivante montre la requête de média modifié :

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample4.css)]

Enregistrez vos modifications et accédez à l’application de conférence dans un émulateur de navigateur mobile. Le texte minuscule dans l’image suivante est le résultat de la suppression de la fenêtre d’affichage `<meta>` balise. Avec aucune fenêtre d’affichage `<meta>` balise, le navigateur est un zoom arrière à la largeur de la fenêtre d’affichage par défaut (850 pixels ou plus large pour les navigateurs mobiles plus.)

[![p1_noViewPort](aspnet-mvc-4-mobile-features/_static/image10.png)](aspnet-mvc-4-mobile-features/_static/image9.png)

Annuler vos modifications, supprimez les commentaires de la fenêtre d’affichage `<meta>` balise dans le fichier de disposition et de restauration de la requête de média à 850 pixels dans le *Site.css* fichier. Enregistrez vos modifications et actualisez le navigateur mobile pour vérifier que l’affichage des appareils mobiles a été restaurée.

La fenêtre d’affichage `<meta>` balise et la requête de média CSS ne sont pas spécifiques à ASP.NET MVC 4, et vous pouvez tirer parti de ces fonctionnalités dans n’importe quelle application web. Mais ils sont désormais intégrés dans les fichiers qui sont générés lorsque vous créez un nouveau projet ASP.NET MVC 4.

Pour plus d’informations sur la fenêtre d’affichage `<meta>` balise, consultez [l’histoire de deux fenêtres : deuxième partie](http://www.quirksmode.org/mobile/viewports2.html).

Dans la section suivante, vous verrez comment fournir des vues spécifiques du navigateur mobile.

## <a name="overriding-views-layouts-and-partial-views"></a>Substitution des vues, des dispositions et des vues partielles

Une nouvelle fonctionnalité importante dans ASP.NET MVC 4 est un mécanisme simple qui vous permet de remplacer n’importe quelle vue (y compris les dispositions et vues partielles) pour les navigateurs mobiles en général, pour un navigateur mobile particulier ou pour n’importe quel navigateur spécifique. Pour fournir un affichage spécifique au mobile, vous pouvez copier un fichier d’Affichage et ajouter *. Mobile* au nom de fichier. Par exemple, pour créer un mobile *Index* afficher, copier *Views\Home\Index.cshtml* à *Views\Home\Index.Mobile.cshtml*.

Dans cette section, vous allez créer un fichier de disposition mobile.

Pour commencer, copiez *Views\Shared\\_Layout.cshtml* à *Views\Shared\\_Layout.Mobile.cshtml*. Ouvrez  *\_Layout.Mobile.cshtml* et remplacez le titre **MVC4 conférence** à **conférence (Mobile)**.

Dans chaque `Html.ActionLink` appeler, supprimez « Parcourir par » dans chaque lien *ActionLink*. Le code suivant montre la section de corps complet du fichier de disposition mobile.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample5.cshtml)]

Copie le *Views\Home\AllTags.cshtml* fichier *Views\Home\AllTags.Mobile.cshtml*. Ouvrez le nouveau fichier et modifiez le `<h2>` élément « Tags » à « Tags (M) » :

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample6.html)]

Accédez à la page des balises à l’aide d’un navigateur de bureau et à l’aide d’émulateur de navigateur mobile. L’émulateur de navigateur mobile affiche les modifications apportées à deux.

[![p2m_layoutTags.mobile](aspnet-mvc-4-mobile-features/_static/image12.png)](aspnet-mvc-4-mobile-features/_static/image11.png)

En revanche, l’affichage du bureau n’a pas changé.

[![p2_layoutTagsDesktop](aspnet-mvc-4-mobile-features/_static/image14.png)](aspnet-mvc-4-mobile-features/_static/image13.png)

## <a name="browser-specific-views"></a>Vues spécifiques du navigateur

En plus des vues spécifiques au bureau et mobiles, vous pouvez créer des vues pour un navigateur en particulier. Par exemple, vous pouvez créer des vues qui sont spécifiquement pour le navigateur iPhone. Dans cette section, vous allez créer une disposition pour le navigateur iPhone et une version iPhone de le *AllTags* vue.

Ouvrez le *Global.asax* fichier, puis ajoutez le code suivant à la `Application_Start` (méthode).

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample7.cs)]

Ce code définit un nouveau mode d’affichage appelé « iPhone » qui sera comparée à chaque demande entrante. Si la demande entrante correspond à la condition que vous avez défini (autrement dit, si l’agent utilisateur contient la chaîne « iPhone »), ASP.NET MVC recherche pour les vues dont le nom contient le suffixe « iPhone ».

Dans le code, avec le bouton droit `DefaultDisplayMode`, choisissez **résoudre**, puis choisissez `using System.Web.WebPages;`. Cette opération ajoute une référence à la `System.Web.WebPages` espace de noms, qui est l’emplacement où le `DisplayModes` et `DefaultDisplayMode` types sont définis.

[![p2_resolve](aspnet-mvc-4-mobile-features/_static/image16.png)](aspnet-mvc-4-mobile-features/_static/image15.png)

Ou bien, vous pouvez ajouter manuellement la ligne suivante à la `using` section du fichier.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample8.cs)]

Le contenu complet de la *Global.asax* fichier est présenté ci-dessous.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample9.cs)]

Enregistrez les modifications. Copie le *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* fichier *MvcMobile\Views\Shared\\_Layout.iPhone.cshtml*. Ouvrez le nouveau fichier et modifiez le `h1` en-tête à partir de `Conference (Mobile)` à `Conference (iPhone)`.

Copie le *MvcMobile\Views\Home\AllTags.Mobile.cshtml* fichier *MvcMobile\Views\Home\AllTags.iPhone.cshtml*. Dans le nouveau fichier, modifiez le `<h2>` élément à partir de « Tags (M) » par « Tags (iPhone) ».

Exécutez l'application. Exécuter un émulateur de navigateur mobile, vérifiez que son agent utilisateur est défini sur « iPhone » et accédez à la *AllTags* vue. La capture d’écran suivante montre le *AllTags* vue restituée dans le [Safari](http://www.apple.com/safari/download/) navigateur. Vous pouvez télécharger Safari pour Windows [ici](https://support.apple.com/kb/DL1531).

[![p2_iphoneView](aspnet-mvc-4-mobile-features/_static/image18.png)](aspnet-mvc-4-mobile-features/_static/image17.png)

Dans cette section, nous avons vu comment créer des vues et dispositions mobiles et comment créer des dispositions et des vues pour des appareils spécifiques tels que l’iPhone. Dans la section suivante, vous verrez comment tirer parti de jQuery Mobile pour les vues mobiles plus attrayantes.

## <a name="using-jquery-mobile"></a>À l’aide de jQuery Mobile

Le [jQuery Mobile](http://jquerymobile.com/demos/1.0b3/#/demos/1.0b3/docs/about/intro.html) bibliothèque fournit une infrastructure d’interface utilisateur qui fonctionne sur tous les principaux navigateurs mobiles. jQuery Mobile applique *amélioration progressive* aux navigateurs mobiles qui prennent en charge de CSS et JavaScript. Amélioration progressive permet à tous les navigateurs afficher le contenu de base d’une page web, tout en permettant aux navigateurs plus puissantes et un affichage plus riche pour les appareils. Les fichiers JavaScript et CSS qui sont inclus avec jQuery Mobile de nombreux éléments selon les navigateurs mobiles sans apporter de modifications balisage style.

Dans cette section, vous allez installer le *jQuery.Mobile.MVC* package NuGet, qui installe jQuery Mobile et un widget sélecteur de vue.

Pour commencer, supprimez le *partagé\\_Layout.Mobile.cshtml* et *partagé\\_Layout.iPhone.cshtml* les fichiers que vous avez créé précédemment.

Renommer *Views\Home\AllTags.Mobile.cshtml* et *Views\Home\AllTags.iPhone.cshtml* fichiers *Views\Home\AllTags.iPhone.cshtml.hide* et  *Views\Home\AllTags.Mobile.cshtml.Hide*. Étant donné que les fichiers n’ont plus un *.cshtml* extension, ils ne sont pas utilisés par le runtime ASP.NET MVC pour restituer le *AllTags* vue.

Installer le *jQuery.Mobile.MVC* package NuGet en procédant ainsi :

1. À partir de la **outils** menu, sélectionnez **Library Package Manager**, puis sélectionnez **Console du Gestionnaire de Package**.

    [![p3_packageMgr](aspnet-mvc-4-mobile-features/_static/image20.png)](aspnet-mvc-4-mobile-features/_static/image19.png)
2. Dans le **Console du Gestionnaire de Package**, entrez `Install-Package jQuery.Mobile.MVC -version 1.0.0`

L’illustration suivante montre les fichiers ajoutés et modifiés au projet MvcMobile par le package jQuery.Mobile.MVC NuGet. Fichiers qui sont ajoutés ont [Ajouter] ajouté après le nom de fichier. L’image n’affiche pas de l’image GIF et PNG fichiers ajoutés à la *Content\images* dossier.

![](aspnet-mvc-4-mobile-features/_static/image21.png)

Le package jQuery.Mobile.MVC NuGet installe les éléments suivants :

- Le *application\_Start\BundleMobileConfig.cs* fichier, qui est nécessaire pour référencer les fichiers JavaScript et CSS jQuery ajoutés. Vous devez suivre les instructions ci-dessous et référencer le regroupement mobile défini dans ce fichier.
- fichiers jQuery Mobile CSS.
- Un `ViewSwitcher` widget du contrôleur (*Controllers\ViewSwitcherController.cs*).
- fichiers JavaScript Mobile jQuery.
- Un fichier de disposition de style Mobile jQuery (*Views\Shared\\_Layout.Mobile.cshtml*).
- Une vue partielle du sélecteur de vue *(MvcMobile\Views\Shared\\_ViewSwitcher.cshtml*) qui fournit un lien en haut de chaque page pour passer à partir de l’affichage du Bureau à l’affichage mobile et vice versa.
- Plusieurs<em>.png</em> et <em>.gif</em> fichiers image dans le <em>Content\images</em> dossier.

Ouvrez le *Global.asax* fichier, puis ajoutez le code suivant en tant que la dernière ligne de la `Application_Start` (méthode).

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample10.cs)]

Le code suivant montre l’ensemble *Global.asax* fichier.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample11.cs?highlight=26)]

> [!NOTE]
> Si vous utilisez Internet Explorer 9 et vous ne voyez pas le `BundleMobileConfig` ligne ci-dessus dans la mise en surbrillance jaune, cliquez sur le [bouton Affichage de compatibilité](https://windows.microsoft.com/windows7/How-to-use-Compatibility-View-in-Internet-Explorer-9)![image du bouton Affichage de compatibilité (désactivé)] (http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg " Image du bouton Affichage de compatibilité (désactivé)") dans Internet Explorer pour modifier l’icône à partir d’un plan ![image du bouton Affichage de compatibilité (désactivé)](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "image du bouton Affichage de compatibilité (désactivé) ") à une couleur unie ![image du bouton Affichage de compatibilité (on)](http://res1.windows.microsoft.com/resbox/en/Windows 7/main/156805ff-3130-481b-a12d-4d3a96470f36_14.jpg "image du bouton Affichage de compatibilité (on)"). Vous pouvez également afficher ce didacticiel dans FireFox ou Chrome.


Ouvrez le *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* fichier, puis ajoutez le balisage suivant directement après le `Html.Partial` appeler :

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample12.cshtml)]

L’ensemble *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* fichier est présenté ci-dessous :

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample13.cshtml)]

Générez l’application et dans votre émulateur de navigateur mobile, accédez à la *AllTags* vue. Vous consultez les rubriques suivantes :

[![p3_afterNuGet](aspnet-mvc-4-mobile-features/_static/image23.png)](aspnet-mvc-4-mobile-features/_static/image22.png)

> [!NOTE]
> Vous pouvez déboguer le code spécifique à mobile par [définissant la chaîne d’agent utilisateur](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) pour Internet Explorer ou Chrome pour iPhone, puis en utilisant les outils de développement F-12. Si votre navigateur mobile n’affiche pas le **accueil**, **haut-parleur**, **balise**, et **Date** liens sous forme de boutons, les références à jQuery Mobile scripts et des fichiers CSS ne sont probablement pas corrects.


En plus des modifications de style, vous voyez **affichage mobile** et un lien qui permet de changer d’affichage mobile en mode bureau. Choisissez le **vue bureau** lien et l’affichage de bureau s’affiche.

[![p3_desktopView](aspnet-mvc-4-mobile-features/_static/image25.png)](aspnet-mvc-4-mobile-features/_static/image24.png)

L’affichage de bureau ne fournit pas un moyen de naviguer directement vers la vue mobile. Vous corrigerez cela maintenant. Ouvrez le *Views\Shared\\_Layout.cshtml* fichier. Juste en dessous de la page `body` élément, ajoutez le code suivant, qui restitue le widget sélecteur de vue :

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample14.cshtml)]

Actualiser le *AllTags* affichage dans le navigateur mobile. Vous pouvez désormais parcourir les vues mobiles et de bureau.

[![p3_desktopViewWithMobileLink](aspnet-mvc-4-mobile-features/_static/image27.png)](aspnet-mvc-4-mobile-features/_static/image26.png)

> [!NOTE]
> Déboguer Remarque : vous pouvez ajouter le code suivant à la fin de la Views\Shared\\_ViewSwitcher.cshtml pour aider à déboguer des vues lors de l’utilisation d’un chaîne d’agent utilisateur du navigateur sur un appareil mobile.
> 
> [!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample15.cs)]
> 
>  et en ajoutant l’en-tête suivant pour le *Views\Shared\\_Layout.cshtml* fichier.  
> 
> [!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample16.html)]


Accédez à la *AllTags* page dans un navigateur de bureau. Le widget sélecteur de vue ne figure pas dans un navigateur de bureau, car il est ajouté uniquement à la page de disposition mobile. Plus loin dans ce didacticiel, vous verrez comment vous pouvez ajouter le widget sélecteur de vue à l’affichage de bureau.

[![p3_desktopBrowser](aspnet-mvc-4-mobile-features/_static/image29.png)](aspnet-mvc-4-mobile-features/_static/image28.png)

## <a name="improving-the-speakers-list"></a>Amélioration de la liste intervenants

Dans le navigateur mobile, sélectionnez le **haut-parleurs** lien. Car il n’existe aucune vue mobile (*AllSpeakers.Mobile.cshtml*), affichent les haut-parleurs par défaut (*AllSpeakers.cshtml*) est restitué à l’aide de la vue disposition mobile ( *\_ Layout.Mobile.cshtml*).

[![p3_speakersDeskTop](aspnet-mvc-4-mobile-features/_static/image31.png)](aspnet-mvc-4-mobile-features/_static/image30.png)

Vous pouvez globalement désactiver une vue de (non mobile) par défaut à partir de rendu à l’intérieur d’une disposition mobile en définissant `RequireConsistentDisplayMode` à `true` dans le *vues\\_ViewStart.cshtml* fichier, comme suit :

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample17.cshtml)]

Lorsque `RequireConsistentDisplayMode` a la valeur `true`, la disposition mobile (<em>\_Layout.Mobile.cshtml</em>) est utilisé uniquement pour les vues mobiles. (Autrement dit, le fichier de vue est au format <em>** ViewName</em><em>. Mobile.cshtml</em>.) Vous souhaiterez peut-être définir `RequireConsistentDisplayMode` à `true` si votre disposition mobile ne fonctionne pas correctement avec les vues non mobiles. La capture d’écran ci-dessous montre comment la <em>haut-parleurs</em> page s’affiche lorsque `RequireConsistentDisplayMode` est défini sur `true`.

[![p3_speakersConsistent](aspnet-mvc-4-mobile-features/_static/image33.png)](aspnet-mvc-4-mobile-features/_static/image32.png)

Vous pouvez désactiver le mode d’affichage cohérent dans une vue en définissant `RequireConsistentDisplayMode` à `false` dans le fichier d’affichage. Le balisage suivant dans le *Views\Home\AllSpeakers.cshtml* fichier définit `RequireConsistentDisplayMode` à `false`:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample18.cshtml)]

## <a name="creating-a-mobile-speakers-view"></a>Création d’une vue de haut-parleurs Mobile

Comme vous venez de voir, le *haut-parleurs* vue est lisible, mais les liens sont petits et sont difficiles à appuyer sur un appareil mobile. Dans cette section, vous allez créer un spécifique de mobile *haut-parleurs* vue qui ressemble à une application mobile moderne, elle affiche volumineux, facile à tap établit un lien et contient une zone de recherche pour trouver des intervenants rapidement.

Copie *AllSpeakers.cshtml* à *AllSpeakers.Mobile.cshtml*. Ouvrez le *AllSpeakers.Mobile.cshtml* de fichiers et de supprimer le `<h2>` élément d’en-tête.

Dans le `<ul>` , ajoutez le `data-role` d’attribut et définissez sa valeur sur `listview`. Comme les autres [ `data-*` attributs](http://html5doctor.com/html5-custom-data-attributes/), `data-role="listview"` rend les éléments de liste volumineux plus facile de tirer parti. Voici à quoi ressemble le balisage complet :

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample19.cshtml)]

Actualisez le navigateur mobile. L’affichage actualisé ressemble à ceci :

[![p3_updatedSpeakerView1](aspnet-mvc-4-mobile-features/_static/image35.png)](aspnet-mvc-4-mobile-features/_static/image34.png)

Bien que la vue mobile a été améliorée, il est difficile de parcourir la longue liste de haut-parleurs. Pour résoudre ce problème, dans le `<ul>` , ajoutez le `data-filter` d’attribut et affectez-lui la valeur `true`. Le code ci-dessous montre le `ul` balisage.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample20.html)]

L’illustration suivante montre la zone de filtre de recherche en haut de la page qui résulte de la `data-filter` attribut.

[![ps_Data_Filter](aspnet-mvc-4-mobile-features/_static/image37.png)](aspnet-mvc-4-mobile-features/_static/image36.png)

Lorsque vous tapez chaque lettre dans la zone de recherche, jQuery Mobile filtre la liste affichée, comme indiqué dans l’image ci-dessous.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image39.png)](aspnet-mvc-4-mobile-features/_static/image38.png)

## <a name="improving-the-tags-list"></a>Amélioration de la liste de balises

Comme la valeur par défaut *haut-parleurs* vue, le *balises* vue est lisible, mais les liens sont petits et difficiles à appuyer sur un appareil mobile. Dans cette section, vous allez résoudre le *balises* afficher de la même façon que vous avez corrigé le *haut-parleurs* vue.

Supprimer le &quot;masquer&quot; suffixe pour le *Views\Home\AllTags.Mobile.cshtml.hide* est le nom de fichier *Views\Home\AllTags.Mobile.cshtml*. Ouvrez le fichier renommé et supprimez le `<h2>` élément.

Ajouter le `data-role` et `data-filter` des attributs pour le `<ul>` balise, comme illustré ici :

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample21.html)]

L’image ci-dessous montre la page de balises filtrage sur la lettre `J`.

[![p3_tags_J](aspnet-mvc-4-mobile-features/_static/image41.png)](aspnet-mvc-4-mobile-features/_static/image40.png)

## <a name="improving-the-dates-list"></a>Amélioration de la liste de Dates

Vous pouvez améliorer la *Dates* afficher la même façon que vous le *haut-parleurs* et *balises* vues, afin qu’il soit plus facile à utiliser sur un appareil mobile.

Copie le *Views\Home\AllDates.cshtml* fichier *Views\Home\AllDates.Mobile.cshtml*. Ouvrez le nouveau fichier et supprimez le `<h2>` élément.

Ajouter `data-role="listview"` à la `<ul>` balise, comme suit :

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample22.html)]

L’image ci-dessous montre à quoi le **Date** page ressemble à la `data-role` attribut en place.

[![p3_dates1](aspnet-mvc-4-mobile-features/_static/image43.png)](aspnet-mvc-4-mobile-features/_static/image42.png) remplacer le contenu de la *Views\Home\AllDates.Mobile.cshtml* fichier par le code suivant :

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample23.cshtml)]

Ce code regroupe toutes les sessions en jours. Elle crée un séparateur de liste pour chaque nouveau jour, et il répertorie toutes les sessions pour chaque jour sous une ligne de séparation. Voici à quoi elle ressemble lorsque ce code s’exécute :

[![p3_dates2](aspnet-mvc-4-mobile-features/_static/image45.png)](aspnet-mvc-4-mobile-features/_static/image44.png)

## <a name="improving-the-sessionstable-view"></a>Amélioration de la vue SessionsTable

Dans cette section, vous allez créer un affichage spécifique au mobile de sessions. Modifications apportées seront plus étendues que dans les autres modes, que nous avons créé.

Dans le navigateur mobile, appuyez sur la **haut-parleur** bouton, puis entrez `Sc` dans la zone de recherche.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image47.png)](aspnet-mvc-4-mobile-features/_static/image46.png)

Appuyez sur la **Scott Hanselman** lien.

[![p3_scottHa](aspnet-mvc-4-mobile-features/_static/image49.png)](aspnet-mvc-4-mobile-features/_static/image48.png)

Comme vous pouvez le voir, l’affichage est difficile à lire sur un navigateur mobile. La colonne de date est difficile à lire et la colonne de balises est en dehors de la vue. Pour résoudre ce problème, copiez *Views\Home\SessionsTable.cshtml* à *Views\Home\SessionsTable.Mobile.cshtml*, puis remplacez le contenu du fichier par le code suivant :

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample24.cshtml)]

Le code supprime la salle d’étiquettes de colonnes et met en forme le titre, l’orateur et date verticalement, afin que toutes ces informations sont lisibles sur un navigateur mobile. L’image ci-dessous reflète les modifications de code.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image51.png)](aspnet-mvc-4-mobile-features/_static/image50.png)

## <a name="improving-the-sessionbycode-view"></a>Amélioration de la vue SessionByCode

Enfin, vous allez créer un affichage spécifique au mobile de le *SessionByCode* vue. Dans le navigateur mobile, appuyez sur la **haut-parleur** bouton, puis entrez `Sc` dans la zone de recherche.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image53.png)](aspnet-mvc-4-mobile-features/_static/image52.png)

Appuyez sur la **Scott Hanselman** lien. Les sessions de Scott Hanselman sont affichées.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image55.png)](aspnet-mvc-4-mobile-features/_static/image54.png)

Choisissez le **une vue d’ensemble de la pile d’amour MS Web** lien.

[![ps_love](aspnet-mvc-4-mobile-features/_static/image57.png)](aspnet-mvc-4-mobile-features/_static/image56.png)

La vue bureau par défaut convient, mais vous pouvez l’améliorer.

Copie le *Views\Home\SessionByCode.cshtml* à *Views\Home\SessionByCode.Mobile.cshtml* et remplacez le contenu de la *Views\Home\SessionByCode.Mobile.cshtml*fichier avec le balisage suivant :

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample25.cshtml)]

Le nouveau balisage utilise le `data-role` attribut pour améliorer la disposition de la vue.

Actualisez le navigateur mobile. L’illustration suivante reflète les modifications de code que vous venez d’apporter :

[![p3_love2](aspnet-mvc-4-mobile-features/_static/image59.png)](aspnet-mvc-4-mobile-features/_static/image58.png)

## <a name="wrapup-and-review"></a>Conclusion et révision

Ce didacticiel a introduit les nouvelles fonctionnalités mobiles d’ASP.NET MVC 4 Developer Preview. Les fonctionnalités mobiles incluent :

- La possibilité de remplacer la mise en page, des vues et des vues partielles, globalement et pour une vue spécifique.
- Contrôler la disposition et la mise en œuvre de substitution partielle à l’aide du `RequireConsistentDisplayMode` propriété.
- Un widget sélecteur de vue pour mobile vues que vous pouvez également afficher dans les vues de bureau.
- Prise en charge pour prendre en charge des navigateurs spécifiques, tels que le navigateur iPhone.

## <a name="see-also"></a>Voir aussi

- [jQuery Mobile](http://jquerymobile.com) site.
- [jQuery Mobile présentation](http://jquerymobile.com/demos/1.0b3/docs/about/intro.html)
- [W3C Web Mobile Application meilleures pratiques pour les recommandations](http://www.w3.org/TR/mwabp/)
- [Recommandation W3C pour les requêtes de média](http://www.w3.org/TR/css3-mediaqueries/)
