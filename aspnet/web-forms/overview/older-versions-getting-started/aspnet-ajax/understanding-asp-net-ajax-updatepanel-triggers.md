---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
title: Présentation des déclencheurs UpdatePanel d’ASP.NET AJAX | Microsoft Docs
author: scottcate
description: Lorsque vous travaillez dans l’éditeur de balisage dans Visual Studio, vous pouvez remarquer (à partir d’IntelliSense) qu’il existe deux éléments enfants d’un contrôle UpdatePanel. Une des lorsqu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2008
ms.topic: article
ms.assetid: faab8503-2984-48a9-8a40-7728461abc50
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
msc.type: authoredcontent
ms.openlocfilehash: 180491b6aee188a9dca750d6d325217f1ad5212c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363719"
---
<a name="understanding-aspnet-ajax-updatepanel-triggers"></a>Présentation des déclencheurs UpdatePanel d’ASP.NET AJAX
====================
par [Scott Cate](https://github.com/scottcate)

[Télécharger PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial02_Triggers_cs.pdf)

> Lorsque vous travaillez dans l’éditeur de balisage dans Visual Studio, vous pouvez remarquer (à partir d’IntelliSense) qu’il existe deux éléments enfants d’un contrôle UpdatePanel. Un d’eux est l’élément de déclencheurs, qui spécifie les contrôles sur la page (ou le contrôle utilisateur, si vous en utilisez un) qui déclenchera un rendu partiel du contrôle UpdatePanel dans lequel réside l’élément.


## <a name="introduction"></a>Introduction

Technologie de Microsoft ASP.NET offre un modèle de programmation orientée objet et événementiel et il unifie les avantages du code compilé. Toutefois, son modèle de traitement côté serveur présente plusieurs inconvénients inhérents à la technologie, bon nombre d'entre elles peuvent être adressés par les nouvelles fonctionnalités incluses dans les Extensions AJAX Microsoft ASP.NET 3.5. Ces extensions permettent de nombreuses nouvelles fonctionnalités de client riche, notamment le rendu partiel de pages sans nécessiter une actualisation de la page entière, la possibilité d’accéder aux Services Web via le script client (y compris les API de profilage ASP.NET) et une API côté client étendue conçu pour mettre en miroir de la plupart des schémas de contrôle indiqués dans l’ensemble du contrôle côté serveur ASP.NET.

Ce livre blanc examine les fonctionnalités de déclencheurs de XML d’ASP.NET AJAX `UpdatePanel` composant. Déclencheurs de XML offrent un contrôle granulaire sur les composants qui peuvent provoquer un rendu partiel des contrôles UpdatePanel spécifiques.

Ce livre blanc est basé sur la version bêta 2 de .NET Framework 3.5 et Visual Studio 2008. Les Extensions ASP.NET AJAX, précédemment un assembly de module complémentaire ciblé sur ASP.NET 2.0, sont désormais intégrées dans la bibliothèque de classes de Base .NET Framework. Ce livre blanc suppose également que vous allez travailler avec Visual Studio 2008, pas Visual Web Developer Express et fournissez des procédures pas à pas en fonction de l’interface utilisateur de Visual Studio (bien que les listes de codes sera entièrement compatibles, indépendamment du environnement de développement).

## <a name="triggers"></a>*Déclencheurs*

Déclencheurs d’un UpdatePanel donné, par défaut, incluent automatiquement tous les contrôles enfants qui appellent une publication (postback), y compris (par exemple) des contrôles de zone de texte qui ont leur `AutoPostBack` propriété définie sur **true**. Toutefois, il est possible que les déclencheurs peuvent être également inclus de manière déclarative à l’aide de balisage ; Cela s’effectue dans le `<triggers>` section de la déclaration du contrôle UpdatePanel. Bien que les déclencheurs sont accessibles le `Triggers` propriété de collection, il est recommandé d’enregistrer des déclencheurs de rendu partiel au moment de l’exécution (par exemple, si un contrôle n’est pas disponible au moment du design) à l’aide de la `RegisterAsyncPostBackControl(Control)` méthode de la Objet de ScriptManager pour votre page, dans le `Page_Load` événement. N’oubliez pas que les Pages sont sans état, et par conséquent, vous devez réinscrire ces contrôles chaque fois qu’ils sont créés.

Inclusion de déclencheur enfant automatique peut également être désactivée (afin que les contrôles enfants qui créent des publications (postback) ne déclenchent pas automatiquement rendus partielles) en définissant le `ChildrenAsTriggers` propriété **false**. Cela vous permet la plus grande flexibilité en assignant des contrôles spécifiques peut invoquer un rendu de page et il est recommandée, afin qu’un développeur sera participer pour répondre à un événement, plutôt que de gérer tous les événements qui peuvent survenir.

Notez que lorsque les contrôles UpdatePanel sont imbriqués, quand l’UpdateMode est définie sur **conditionnel**, si l’enfant UpdatePanel est déclenchée, mais le parent n’est pas, puis uniquement l’enfant actualise UpdatePanel. Toutefois, si le parent UpdatePanel est actualisé, puis l’UpdatePanel enfant sera également actualisé.

## <a name="the-lttriggersgt-element"></a>*Le &lt;déclencheurs&gt; élément*

Lorsque vous travaillez dans l’éditeur de balisage dans Visual Studio, vous pouvez remarquer (à partir d’IntelliSense) qu’il existe deux éléments enfants d’un `UpdatePanel` contrôle. Est l’élément plus souvent vu le `<ContentTemplate>` élément, qui encapsule essentiellement le contenu qui se déroulera par le panneau de mise à jour (le contenu pour lequel nous mettons en place le rendu partiel). L’autre élément est la `<Triggers>` élément, qui spécifie les contrôles sur la page (ou le contrôle utilisateur, si vous en utilisez un) qui déclenchera un rendu partiel du contrôle UpdatePanel dans lequel le &lt;déclencheurs&gt; élément réside.

Le `<Triggers>` élément peut contenir n’importe quel nombre de deux nœuds enfants : `<asp:AsyncPostBackTrigger>` et `<asp:PostBackTrigger>`. Les deux acceptent deux attributs, `ControlID` et `EventName`et vous pouvez spécifier n’importe quel contrôle dans l’unité actuelle d’encapsulation (par exemple, si votre contrôle UpdatePanel se trouve dans un contrôle utilisateur Web, vous n’essayez pas de référencer un contrôle sur la Page sur laquelle réside le contrôle utilisateur).

Le `<asp:AsyncPostBackTrigger>` élément est particulièrement utile dans la mesure où il peut cibler n’importe quel événement d’un contrôle qui existe en tant qu’enfant de *n’importe quel* contrôle UpdatePanel dans l’unité d’encapsulation, pas seulement le UpdatePanel sous lequel ce déclencheur est un enfant . Par conséquent, n’importe quel contrôle réalisables pour déclencher une mise à jour de page partielle.

De même, le `<asp:PostBackTrigger>` élément peut être utilisé pour restituer une page partielle de déclencheur, mais qui nécessite un aller-retour complet sur le serveur. Cet élément de déclencheur permettre également être utilisé pour forcer un rendu de page complète quand un contrôle sinon normalement déclenche un rendu de page partielle (par exemple, lorsqu’un `Button` contrôle existe dans le `<ContentTemplate>` élément d’un contrôle UpdatePanel). Là encore, l’élément PostBackTrigger peut spécifier n’importe quel contrôle qui est un enfant de n’importe quel contrôle UpdatePanel dans l’unité actuelle d’encapsulation.

## <a name="lttriggersgt-element-reference"></a>*&lt;Déclencheurs&gt; référence des éléments*

*Descendants de balisage :*

| **Balise** | **Description** |
| --- | --- |
| &lt;asp:AsyncPostBackTrigger&gt; | Spécifie un événement qui entraîne une mise à jour de page partielle pour le contrôle UpdatePanel qui contient cette référence de déclencheur et contrôle. |
| &lt;asp:PostBackTrigger&gt; | Spécifie un contrôle et un événement qui entraîne une mise à jour de page entière (une actualisation complète de la page). Cette balise peut être utilisée pour forcer une actualisation complète quand un contrôle déclenche sinon le rendu partiel. |

## <a name="walkthrough-cross-updatepanel-triggers"></a>*Procédure pas à pas : Cross-UpdatePanel déclencheurs*

1. Créer une nouvelle page ASP.NET avec un objet de ScriptManager définir pour activer le rendu partiel. Ajouter deux UpdatePanel à cette page - dans la première, inclure une étiquette (Label1) et deux contrôles bouton (Button1 et Button2). Button1 doit indiquer Cliquez pour mettre à jour à la fois et Button2 doit indiquer Cliquez pour mettre à jour de cela, ou quelque chose le long de ces lignes. Dans la deuxième UpdatePanel, inclure uniquement un contrôle Label (Label2), mais définissez sa propriété de couleur de premier plan sur autre chose que la valeur par défaut pour les distinguer.
2. Définissez la propriété UpdateMode les deux balises UpdatePanel pour **conditionnel**.

**Liste 1 : Le balisage de default.aspx :** 

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample1.aspx)]

1. Dans le Gestionnaire d’événements Click de Button1, la valeur est Label1.Text et Label2.Text quelque chose dépendant du temps (par exemple, DateTime.Now.ToLongTimeString()). Le Gestionnaire d’événements Click pour Button2, à remplacer Label1.Text uniquement la valeur dépendant du temps.

**Listing 2 : Code-behind (supprimé) dans default.aspx.cs :** 

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample2.cs)]

1. Appuyez sur F5 pour générer et exécuter le projet. Notez que, lorsque vous cliquez sur les panneaux de mise à jour à la fois, les deux étiquettes modifier du texte ; Cependant, lorsque vous cliquez sur Panneau de cette mise à jour, Label1 uniquement mises à jour.


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image2.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image1.png)

([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-updatepanel-triggers/_static/image3.png))


## <a name="under-the-hood"></a>*Rouages du système*

Utilisation de l’exemple que nous venez de créer, nous pouvons Examinons ce que fait ASP.NET AJAX et de fonctionnement de notre déclencheurs UpdatePanel d’entre-Panneau de configuration. Pour ce faire, nous nous efforcerons de la source HTML de la page générée, ainsi que l’extension de Mozilla Firefox appelé FireBug - avec celui-ci, nous pouvons facilement examiner les renvois AJAX. Nous allons également utiliser l’outil de .NET Reflector de Lutz Roeder. Ces deux outils sont disponibles gratuitement en ligne et accessible avec une recherche sur internet.

Un examen du code source page montre presque aucune configuration particulière ; les contrôles UpdatePanel sont rendus sous forme `<div>` conteneurs et nous pouvons voir inclut de la ressource de script fourni par le `<asp:ScriptManager>`. Il existe également certains nouveaux appels spécifique à AJAX à PageRequestManager qui sont internes à la bibliothèque de scripts client AJAX. Enfin, nous voyons les deux conteneurs UpdatePanel - un avec le rendu `<input>` boutons avec les deux `<asp:Label>` contrôles rendus sous forme de `<span>` conteneurs. (Si vous examinez l’arborescence DOM dans le FireBug, vous remarquerez que les étiquettes sont estompés pour indiquer qu’ils ne sont pas produit contenu visible).

Cliquez sur le bouton de mise à jour ce panneau de configuration et notez que le contrôle UpdatePanel supérieur sera actualisée avec l’heure actuelle du serveur. Dans le FireBug, choisissez l’onglet Console afin que vous puissiez examiner la demande. Examinez tout d’abord les paramètres de la demande POST :


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image5.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image4.png)

([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-updatepanel-triggers/_static/image6.png))


Notez que le contrôle UpdatePanel a indiqué au code côté serveur AJAX précisément quelle arborescence de contrôle a été déclenché via le paramètre ScriptManager1 : `Button1` de la `UpdatePanel1` contrôle. Maintenant, cliquez sur le bouton de mise à jour des panneaux à la fois. Puis, examiner la réponse, nous voyons une série délimitée une barre verticale de variables définies dans une chaîne ; plus précisément, nous voyons l’UpdatePanel supérieur, `UpdatePanel1`, a l’intégralité de son code HTML envoyé au navigateur. La bibliothèque de scripts client AJAX substitue d’origine contenu HTML l’UpdatePanel avec le nouveau contenu via le `.innerHTML` propriété, et par conséquent, le serveur envoie le contenu est modifié à partir du serveur au format HTML.

Maintenant, cliquez sur le bouton de mise à jour des panneaux à la fois et examiner les résultats à partir du serveur. Les résultats sont très similaires, les deux UpdatePanel recevoir la nouvelle page HTML à partir du serveur. Comme avec le rappel précédent, état de la page supplémentaire est envoyée.

Comme nous pouvons le constater, car aucun code spécial n’est utilisé pour effectuer un postback AJAX, la bibliothèque de scripts client AJAX est en mesure d’intercepter les publications de formulaire sans code supplémentaire. Contrôles serveur utilisent automatiquement JavaScript afin qu’ils n’envoient pas automatiquement le formulaire - ASP.NET injecte automatiquement le code de validation de formulaire et l’état déjà, principalement obtenue par inclusion de ressources de script automatique, la classe PostBackOptions et la classe ClientScriptManager.

Prenons l’exemple d’un contrôle de case à cocher ; Examinez le code machine de classe dans .NET Reflector. Pour ce faire, vérifiez que votre assembly System.Web est ouvert et accédez à la `System.Web.UI.WebControls.CheckBox` classe, en ouvrant le `RenderInputTag` (méthode). Recherchez un conditionnel qui vérifie le `AutoPostBack` propriété :


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image8.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image7.png)

([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-updatepanel-triggers/_static/image9.png))


Lorsque la publication (postback) automatique est activée sur un `CheckBox` contrôler (via la propriété AutoPostBack ayant pour valeur true), la résultante `<input>` balise est donc rendue avec un événement ASP.NET gère le script dans son `onclick` attribut. L’interception de soumission du formulaire, puis, permet à ASP.NET AJAX devant être injectés dans la page nonintrusively, contribuant à éviter les éventuelles modifications avec rupture qui peuvent se produire en utilisant un remplacement de chaîne éventuellement imprécise. En outre, cela permet *n’importe quel* contrôle ASP.NET personnalisé à utiliser la puissance d’ASP.NET AJAX sans code supplémentaire pour prendre en charge son utilisation dans un conteneur d’UpdatePanel.

Le `<triggers>` fonctionnalité correspond aux valeurs initialisées dans l’appel de PageRequestManager à \_updateControls (Notez que la bibliothèque de scripts client ASP.NET AJAX utilise la convention que des méthodes, événements et les noms de champs qui commencent un trait de soulignement sont marqués comme internes et ne sont pas destiné à une utilisation en dehors de la bibliothèque elle-même). Avec ce dernier, nous pouvons observer les contrôles qui sont destinées à provoquer des publications (postback) AJAX.

Par exemple, nous allons ajouter deux contrôles supplémentaires à la page, en conservant un contrôle en dehors de l’UpdatePanel entièrement et en conservant une fois dans un UpdatePanel. Nous ajouter un contrôle de case à cocher dans le contrôle UpdatePanel supérieur et supprimer un contrôle DropDownList avec un nombre de couleurs sont définies dans la liste. Voici le balisage :

**Liste 3 : Nouveau balisage**

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample3.aspx)]

Et Voici le nouveau code-behind :

**Liste 4 : code-behind**

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample4.cs)]

L’idée derrière cette page est que la liste déroulante sélectionne une des trois couleurs pour afficher le deuxième contrôle label, que la case à cocher détermine à la fois s’il est en gras et indique si les étiquettes d’affichent la date, ainsi que le temps. La case à cocher ne doivent pas provoquer une mise à jour AJAX, mais la liste déroulante doit, même si elle n’est pas hébergée dans un UpdatePanel.


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image11.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image10.png)

([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-updatepanel-triggers/_static/image12.png))


Comme c’est évident dans la capture d’écran ci-dessus, le bouton de la plus récente sur lequel cliquer a été le bouton droit de mise à jour de ce panneau de configuration, qui le temps supérieur, indépendamment du temps en bas de mise à jour. La date a été désactivée également entre les clics, comme la date est visible dans l’étiquette du bas. Enfin, d’intérêt est la couleur de l’étiquette bas : il a été mis à jour plus récemment que le texte du libellé, qui montre que l’état du contrôle est important, et les utilisateurs s’attendent à pouvoir être conservé dans les renvois AJAX. *Toutefois*, l’heure n’a pas été mis à jour. Le temps a été automatiquement complétée par le biais de la persistance du \_ \_champ d’état d’affichage de la page en cours d’interprétation par le runtime ASP.NET lorsque le contrôle a été à nouveau rendu sur le serveur. Le code de serveur ASP.NET AJAX ne reconnaît pas dans lequel les contrôles de méthodes modifiez état ; Il remplit simplement à partir de l’état d’affichage et exécute ensuite les événements qui sont appropriés.

Il convient de remarquer, cependant, qu’avais j’initialisé dans la Page de l’heure\_événement de chargement, l’heure est ont été correctement incrémentée. Par conséquent, les développeurs doivent Méfiez-vous que le code approprié est en cours d’exécution pendant les gestionnaires d’événements appropriée et éviter l’utilisation de la Page\_charger lorsqu’un gestionnaire d’événements de contrôle serait approprié.

## <a name="summary"></a>Récapitulatif

Le contrôle UpdatePanel d’ASP.NET AJAX Extensions est souple et peut utiliser plusieurs méthodes pour identifier les événements de contrôle qui doivent entraîner sa mise à jour. Il prend en charge la mise à jour automatiquement par ses contrôles enfants, mais peut également répondre aux événements de contrôle ailleurs sur la page.

Pour réduire le risque de charge de traitement de serveur, il est recommandé que le `ChildrenAsTriggers` propriété d’un UpdatePanel être définie sur `false`, et que les événements être choisi à plutôt qu’inclus par défaut. Cela empêche également que tous les événements inutiles à l’origine des effets potentiellement indésirables, notamment les validations et les modifications apportées aux champs d’entrée. Ces types de bogues peuvent être difficiles à isoler, étant donné que la page des mises à jour en toute transparence à l’utilisateur, et la cause peut donc pas immédiatement évidente.

En examinant le fonctionnement interne de la forme d’ASP.NET AJAX publier le modèle de l’interception, nous avons pu déterminer qu’elle utilise l’infrastructure déjà fournie par ASP.NET. Ce faisant, il préserve la compatibilité maximale avec les contrôles conçus à l’aide de la même infrastructure et impose au minimum sur n’importe quel code JavaScript écrit pour la page.

## <a name="bio"></a>Biographie

Rob Paveza est un développeur d’applications .NET senior à Terralever ([www.terralever.com](http://www.terralever.com)), des principaux interactive marketing cabinets dans Tempe, en Arizona. Il peut être contacté à [ robpaveza@gmail.com ](mailto:robpaveza@gmail.com), et son blog se trouve dans [ http://geekswithblogs.net/robp/ ](http://geekswithblogs.net/robp/).

Scott Cate travaille avec les technologies Web Microsoft depuis 1997 et est le président de myKB.com ([www.myKB.com](http://www.myKB.com)) où il est spécialisé dans l’écriture d’ASP.NET en fonction des applications axées sur les solutions logicielles de la Base de connaissances. Scott peut être contacté par courrier électronique en [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) ou son blog à l’adresse [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Précédent](understanding-partial-page-updates-with-asp-net-ajax.md)
> [Suivant](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
