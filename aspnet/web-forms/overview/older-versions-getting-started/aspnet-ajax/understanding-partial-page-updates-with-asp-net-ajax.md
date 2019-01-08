---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
title: Présentation des mises à jour de Page partielle avec ASP.NET AJAX | Microsoft Docs
author: scottcate
description: La fonctionnalité la plus visible des Extensions ASP.NET AJAX est peut-être la possibilité d’effectuer des mises à jour une page partielle ou incrémentielle sans effectuer une publication automatique complète en t...
ms.author: riande
ms.date: 03/28/2008
ms.assetid: 54d9df99-1161-4899-b4e8-2679c85915e7
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
msc.type: authoredcontent
ms.openlocfilehash: 4883046aa16d5e67b7f0c92e15c897ef1a933b67
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098933"
---
<a name="understanding-partial-page-updates-with-aspnet-ajax"></a>Met à jour de Page partielle de présentation avec ASP.NET AJAX
====================
par [Scott Cate](https://github.com/scottcate)

[Télécharger PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial01_Partial_Page_Updates_cs.pdf)

> La fonctionnalité la plus visible des Extensions ASP.NET AJAX est peut-être la possibilité d’effectuer des mises à jour une page partielle ou incrémentielle sans effectuer une publication automatique complète sur le serveur, sans modifications de code et les changements de balise minimal. Les avantages sont étendues : l’état de vos éléments multimédias (par exemple, Adobe Flash ou un support de Windows) est inchangé, coûts de bande passante sont réduits, et le client ne connaît pas le scintillement généralement associé à une publication (postback).


## <a name="introduction"></a>Introduction

Technologie de Microsoft ASP.NET offre un modèle de programmation orientée objet et événementiel et il unifie les avantages du code compilé. Toutefois, son modèle de traitement côté serveur présente plusieurs inconvénients inhérents à la technologie :

- Mises à jour de la page nécessitent un aller-retour vers le serveur, ce qui nécessite une actualisation de la page.
- Boucles ne sont pas conservent tous les effets générés par Javascript ou autre technologie côté client (par exemple, Adobe Flash)
- Au cours de la publication (postback), les navigateurs autres que Microsoft Internet Explorer ne gèrent pas restaurer automatiquement la position de défilement. Et même dans Internet Explorer, il existe toujours un scintillement comme la page est actualisée.
- Publications (postbacks) peut impliquer une grande quantité de bande passante, comme le \_ \_peut augmenter de champ de formulaire d’état d’affichage, en particulier lorsque vous traitez des contrôles tels que le contrôle GridView ou répéteurs.
- Il n’existe aucun modèle unifié pour accéder aux Services Web via JavaScript ou autre technologie côté client.

Entrez les extensions ASP.NET AJAX de Microsoft. AJAX, signifiant **A** synchrone **J** avaScript **A** nd **X** ML, est une infrastructure intégrée permettant la page incrémentielle mises à jour via JavaScript inter-plateformes, composée de code côté serveur qui comprend l’infrastructure AJAX de Microsoft et un composant de script appelé Script Microsoft AJAX Library. Les extensions ASP.NET AJAX fournissent également prise en charge multiplateforme pour accéder à des Services Web ASP.NET via JavaScript.

Ce livre blanc examine les fonctionnalités de mises à jour de page partielle des Extensions ASP.NET AJAX, qui inclut le composant ScriptManager, le contrôle UpdatePanel et le contrôle UpdateProgress et prend en compte les scénarios dans lesquels ils doivent ou ne doivent pas être utilisé.

Ce livre blanc est basé sur la version bêta 2 de Visual Studio 2008 et .NET Framework 3.5, qui intègre les Extensions ASP.NET AJAX dans la bibliothèque de classes de Base (où il était précédemment un composant additionnel disponible pour ASP.NET 2.0). Ce livre blanc suppose également que vous utilisez Visual Studio 2008 et pas Visual Web Developer Express Edition ; Certains modèles de projet qui sont référencés n’est peut-être pas disponibles pour les utilisateurs de Visual Web Developer Express.

## <a name="partial-page-updates"></a>Mises à jour de Page partielle

La fonctionnalité la plus visible des Extensions ASP.NET AJAX est peut-être la possibilité d’effectuer des mises à jour une page partielle ou incrémentielle sans effectuer une publication automatique complète sur le serveur, sans modifications de code et les changements de balise minimal. Les avantages sont étendues : l’état de vos éléments multimédias (par exemple, Adobe Flash ou un support de Windows) est inchangé, réduction des coûts de bande passante et le client ne connaît pas le scintillement généralement associé à une publication (postback).

La possibilité d’intégrer le rendu de page partielle est intégrée à ASP.NET avec des modifications minimes dans votre projet.

## <a name="walkthrough-integrating-partial-rendering-into-an-existing-project"></a>Procédure pas à pas : L’intégration de rendu partiel dans un projet existant


1. Dans Microsoft Visual Studio 2008, créez un projet de Site Web ASP.NET en accédant à <em>fichier</em>  <em>- &gt; New</em>  <em>- &gt; le Site Web</em> et en sélectionnant le Site Web ASP.NET à partir de la boîte de dialogue. Vous pouvez le nommer comme vous le souhaitez, et vous pouvez l’installer dans le système de fichiers ou dans les Services Internet (IIS).
2. S’affiche avec la page par défaut vide avec le balisage de base ASP.NET (un formulaire côté serveur et un `@Page` directive). Supprimer une étiquette appelée `Label1` et un bouton appelé `Button1` sur la page dans l’élément de formulaire. Vous pouvez définir leurs propriétés de texte à votre convenance.
3. En mode conception, double-cliquez sur `Button1` pour générer un gestionnaire d’événements de code-behind. Dans ce gestionnaire d’événements, définissez `Label1.Text` à, vous avez cliqué sur le bouton ! .

**Liste 1 : Balisage de default.aspx avant le rendu partiel est activé.**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample1.aspx)]

**Listing 2 : Code-behind (supprimé) dans default.aspx.cs**

[!code-csharp[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample2.cs)]

1. Appuyez sur F5 pour lancer votre site web. Visual Studio vous invite à ajouter un fichier web.config pour activer le débogage ; le faire. Lorsque vous cliquez sur le bouton, notez que la page est actualisée pour modifier le texte dans l’étiquette, et il existe un scintillement brève comme la page est redessinée.
2. Après la fermeture de la fenêtre du navigateur, retournez à Visual Studio et à la page de balisage. Défiler vers le bas dans la boîte à outils Visual Studio et l’onglet Extensions AJAX. (Si vous n’avez pas cet onglet, car vous utilisez une version antérieure des extensions AJAX ou Atlas, reportez-vous à la procédure pas à pas pour enregistrer les éléments de boîte à outils des Extensions AJAX plus loin dans ce livre blanc, ou installez la version actuelle avec le programme d’installation de Windows téléchargeable à partir du site Web).


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image2.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image1.png)

([Cliquez pour afficher l’image en taille réelle](understanding-partial-page-updates-with-asp-net-ajax/_static/image3.png))


1. <em>Problème connu :</em>si vous installez Visual Studio 2008 sur un ordinateur équipé de Visual Studio 2005 est installé avec les Extensions ASP.NET 2.0 AJAX, Visual Studio 2008 importe les éléments de boîte à outils AJAX Extensions. Vous pouvez déterminer si c’est le cas en examinant l’info-bulle des composants ; Il doivent indiquer la Version 3.5.0.0. S’ils disent Version 2.0.0.0, puis vous avez importé vos anciens éléments de boîte à outils et devez les importer manuellement à l’aide de la boîte de dialogue Choisir des éléments de boîte à outils dans Visual Studio. Il se peut que vous ne pourrez pas ajouter des contrôles de Version 2 via le concepteur.

2. Avant du `<asp:Label>` balise commence, créer une ligne de l’espace blanc et double-cliquez sur le contrôle UpdatePanel dans la boîte à outils. Notez qu’un nouveau `@Register` directive est incluse en haut de la page, indiquant que les contrôles dans l’espace de noms System.Web.UI doivent être importées à l’aide de la `asp:` préfixe.
3. Faites glisser la fermeture `</asp:UpdatePanel>` balise après la fin de l’élément Button, afin que l’élément soit bien formé avec les contrôles Label et Button encapsulés.
4. Après l’ouverture `<asp:UpdatePanel>` balise, commencer une nouvelle balise d’ouverture. Notez qu’IntelliSense vous présente deux options. Dans ce cas, créez un `<ContentTemplate>` balise. Veillez à encapsuler cette balise autour de votre étiquette et un bouton afin que le balisage est bien formé.


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image5.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image4.png)

([Cliquez pour afficher l’image en taille réelle](understanding-partial-page-updates-with-asp-net-ajax/_static/image6.png))


1. N’importe où dans le `<form>` élément, inclure un contrôle ScriptManager en double-cliquant sur le `ScriptManager` élément dans la boîte à outils.
2. Modifier le `<asp:ScriptManager>` balise afin qu’il inclue l’attribut `EnablePartialRendering= true`.

**Liste 3 : Balisage de default.aspx avec le rendu partiel est activé**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample3.aspx)]

1. Ouvrez votre fichier web.config. Notez que Visual Studio a ajouté automatiquement une référence de compilation à System.Web.Extensions.dll.

1. Quelles sont les nouveautés dans Visual Studio 2008 : Le fichier web.config qui est fourni avec le Site de Web ASP.NET modèles de projet automatiquement inclut toutes les références nécessaires pour les Extensions ASP.NET AJAX et inclut les sections d’informations de configuration qui peuvent être non commentée pour activer supplémentaires commentées fonctionnalité. Visual Studio 2005 avaient des modèles similaires lorsque ASP.NET 2.0 AJAX Extensions ont été installées. Toutefois, dans Visual Studio 2008, les Extensions AJAX sont opt-out par défaut (autrement dit, ils sont référencés par défaut, mais peuvent être supprimés en tant que références).


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image8.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image7.png)

([Cliquez pour afficher l’image en taille réelle](understanding-partial-page-updates-with-asp-net-ajax/_static/image9.png))


1. Appuyez sur F5 pour lancer votre site Web. Notez comment aucune modification de code source ont été nécessaires pour prendre en charge le rendu partiel - uniquement le balisage a été modifié.

Lorsque vous lancez votre site Web, vous devez voir que le rendu partiel est maintenant activée, car lorsque vous cliquez sur le bouton il ne sera pas de scintillement ni y aura-t-il toute modification de la position de défilement de page (cet exemple ne montre pas qui). Si vous deviez examiner la source de la page affichée après avoir cliqué sur le bouton, il confirme qu’en fait la publication en retour n’a pas eu lieu - le texte d’étiquette d’origine fait toujours partie de la balise source, et l’étiquette a changé via JavaScript.

Visual Studio 2008 ne semble pas être accompagnée d’un modèle prédéfini pour un site web compatible ASP.NET AJAX. Toutefois, un tel modèle était disponible dans Visual Studio 2005 si le Visual Studio 2005 et ASP.NET 2.0 AJAX Extensions ont été installées. Par conséquent, configuration d’un site web et en commençant par le modèle de Site de Web compatible AJAX seront probablement encore plus faciles, comme le modèle doit inclure un fichier web.config entièrement configuré (prenant en charge toutes les Extensions ASP.NET AJAX, notamment l’accès aux Services Web et la sérialisation JSON - JavaScript Object Notation) et inclut un UpdatePanel et le ContentTemplate au sein de la page Web Forms principale par défaut. L’activation de rendu partiel avec cette page par défaut est aussi simple que revisité étape 10 de cette procédure pas à pas et en déposant les contrôles sur la page.

## <a name="the-scriptmanager-control"></a>Le contrôle ScriptManager

## <a name="scriptmanager-control-reference"></a>Référence du contrôle ScriptManager

Propriétés du balisage activé :

| **Nom de la propriété** | **Type** | **Description** |
| --- | --- | --- |
| AllowCustomErrors-redirection | Bool | Spécifie s’il faut utiliser la section d’erreurs personnalisées du fichier web.config pour gérer les erreurs. |
| Message de AsyncPostBackError | Chaîne | Obtient ou définit le message d’erreur envoyé au client si une erreur est générée. |
| Délai d’attente de AsyncPostBack | Int32 | Obtient ou définit la quantité par défaut d’une heure, qu'un client doit attendre la demande asynchrone s’achève. |
| Globalisation EnableScript | Bool | Obtient ou définit si la globalisation de script est activée. |
| EnableScript, localisation | Bool | Obtient ou définit une valeur indiquant si la localisation de script est activée. |
| ScriptLoadTimeout | Int32 | Détermine le nombre de secondes autorisé pour le chargement des scripts dans le client |
| ScriptMode | Enum (Auto, Debug, Release, héritent) | Obtient ou définit s’il faut restituer les versions release des scripts |
| ScriptPath | Chaîne | Obtient ou définit le chemin d’accès racine à l’emplacement des fichiers de script à envoyer au client. |

Propriétés du code uniquement :

| **Nom de la propriété** | **Type** | **Description** |
| --- | --- | --- |
| AuthenticationService | AuthenticationService-Manager | Obtient les détails sur le proxy de Service d’authentification ASP.NET qui sera envoyé au client. |
| IsDebuggingEnabled | Bool | Obtient si le script et le débogage du code est activé. |
| IsInAsyncPostback | Bool | Détermine si la page en cours d’une demande de publication asynchrone. |
| ProfileService | ProfileService-Manager | Obtient les détails sur le proxy de Service de profilage ASP.NET qui sera envoyé au client. |
| scripts ; | Collection&lt;référence de Script&gt; | Obtient une collection de références de script qui sera envoyé au client. |
| Services | Collection&lt;référence de Service&gt; | Obtient une collection de références de proxy de Service Web qui sera envoyé au client. |
| SupportsPartialRendering | Bool | Détermine si le client actuel prend en charge le rendu partiel. Si cette propriété retourne **false**, toutes les demandes de page seront alors des publications (postbacks) standard. |

Méthodes de Code public :

| **Nom de la méthode** | **Type** | **Description** |
| --- | --- | --- |
| SetFocus(string) | Void | Définit le focus du client à un contrôle spécifique lors de la demande est terminée. |

Descendants de balisage :

| **Balise** | **Description** |
| --- | --- |
| &lt;AuthenticationService&gt; | Fournit des détails sur le proxy au service d’authentification ASP.NET. |
| &lt;ProfileService&gt; | Fournit des détails sur le serveur proxy pour le service de profilage de ASP.NET. |
| &lt;Scripts&gt; | Fournit des références de script supplémentaires. |
| &lt;ASP : ScriptReference&gt; | Indique une référence de script spécifiques. |
| &lt;Service&gt; | Fournit des références de Service Web supplémentaires qui disposera des classes proxy générées. |
| &lt;ASP : ServiceReference&gt; | Indique une référence de Service Web spécifique. |

Le contrôle ScriptManager est la base essentielle pour les Extensions ASP.NET AJAX. Il fournit l’accès à la bibliothèque de scripts (y compris le système de type complète un script côté client), prend en charge le rendu partiel et fournit la prise en charge complète pour les services ASP.NET supplémentaires (par exemple, l’authentification et de profilage, mais également d’autres Services Web). Le contrôle ScriptManager fournit également la prise en charge de globalisation et localisation pour les scripts client.

## <a name="providing-alternative-and-supplemental-scripts"></a>En fournissant des Scripts supplémentaires et Alternative

Bien que les Extensions Microsoft ASP.NET 2.0 AJAX incluent le code de la totalité du script dans les versions debug et release des éditions en tant que ressources incorporées dans les assemblys référencés, les développeurs sont libres de rediriger le ScriptManager pour les fichiers de script personnalisé, mais aussi inscrire scripts nécessaires supplémentaires.

Pour remplacer la liaison par défaut pour les scripts sont en général inclus (tels que ceux qui prennent en charge de l’espace de noms Sys.WebForms et le système de typage personnalisé), vous pouvez vous inscrire à la `ResolveScriptReference` événement de la classe ScriptManager. Lorsque cette méthode est appelée, le Gestionnaire d’événements a la possibilité de modifier le chemin d’accès au fichier de script en question ; le Gestionnaire de script envoie ensuite une copie différente ou personnalisée des scripts au client.

En outre, les références de script (représenté par la `ScriptReference` classe) peut être inclus par programme ou par le biais de balisage. Pour ce faire, soit modifier par programmation la `ScriptManager.Scripts` collection, ou incluez `<asp:ScriptReference>` balises sous le `<Scripts>` balise, qui est un enfant de premier niveau du contrôle ScriptManager.

## <a name="custom-error-handling-for-updatepanels"></a>Gestion d’UpdatePanel d’erreur personnalisée

Bien que les mises à jour sont gérées par les déclencheurs spécifiés par les contrôles UpdatePanel, la prise en charge pour la gestion des erreurs et messages d’erreur personnalisés est gérée par instance du contrôle ScriptManager d’une page. Pour cela, exposez un événement, `AsyncPostBackError`, à la page qui peut ensuite fournir une logique de gestion des exceptions personnalisée.

En consommant de l’événement AsyncPostBackError, vous pouvez spécifier le `AsyncPostBackErrorMessage` propriété, ce qui provoque ensuite un message d’alerte à signaler à l’achèvement du rappel.

Personnalisation côté client est également possible au lieu d’utiliser la fenêtre d’alerte par défaut ; par exemple, vous pouvez souhaiter afficher un texte personnalisé `<div>` élément plutôt que par défaut navigateur boîte de dialogue modale. Dans ce cas, vous pouvez gérer l’erreur dans le script client :

**Liste 5 : Un script côté client pour afficher les erreurs personnalisées**

[!code-html[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample4.html)]

Tout simplement, le script ci-dessus inscrit un rappel avec le runtime d’AJAX côté client pour lors de la requête asynchrone est terminée. Vérifie si une erreur a été signalée, ensuite il et dans ce cas, traite les détails de celui-ci, enfin qui indique au runtime que l’erreur a été gérée dans le script personnalisé.

## <a name="globalization-and-localization-support"></a>Prise en charge de la localisation et de globalisation

Le contrôle ScriptManager offre la prise en charge complète pour la localisation de chaînes de script et des composants d’interface utilisateur ; Toutefois, cette rubrique est en dehors de la portée de ce livre blanc. Pour plus d’informations, consultez le livre blanc, prise en charge de la globalisation dans ASP.NET AJAX Extensions.

## <a name="the-updatepanel-control"></a>Le contrôle UpdatePanel

## <a name="updatepanel-control-reference"></a>Référence de contrôle UpdatePanel

Propriétés du balisage activé :

| **Nom de la propriété** | **Type** | **Description** |
| --- | --- | --- |
| ChildrenAsTriggers | bool | Spécifie si les contrôles enfants appellent automatiquement l’actualisation de publication (postback). |
| RenderMode | enum (bloc, Inline) | Spécifie que la façon du contenu s’afficheront visuellement. |
| UpdateMode | enum (toujours, conditionnel) | Spécifie si le contrôle UpdatePanel est toujours actualisé pendant un rendu partiel ou si elles sont actualisées uniquement lorsqu’un déclencheur est atteint. |

Propriétés du code uniquement :

| **Nom de la propriété** | **Type** | **Description** |
| --- | --- | --- |
| IsInPartialRendering | bool | Détermine si le contrôle UpdatePanel prend en charge le rendu partiel pour la requête actuelle. |
| ContentTemplate | ITemplate | Obtient le modèle de balisage pour la demande de mise à jour. |
| ContentTemplateContainer | Contrôle | Obtient le modèle de programmation pour la demande de mise à jour. |
| Déclencheurs | UpdatePanel - TriggerCollection | Obtient la liste des déclencheurs associés à l’UpdatePanel actuel. |

Méthodes de Code public :

| **Nom de la méthode** | **Type** | **Description** |
| --- | --- | --- |
| Update() | Void | Met à jour UpdatePanel spécifié par programmation. Permet à une demande de serveur déclencher un rendu partiel d’un UpdatePanel sinon-non déclenchée. |

Descendants de balisage :

| **Balise** | **Description** |
| --- | --- |
| &lt;ContentTemplate&gt; | Spécifie le balisage à utiliser pour restituer le résultat de rendu partiel. Enfant de &lt;asp : UpdatePanel&gt;. |
| &lt;Déclencheurs&gt; | Spécifie une collection de *n* contrôles associés à la mise à jour cet UpdatePanel. Enfant de &lt;asp : UpdatePanel&gt;. |
| &lt;asp:AsyncPostBackTrigger&gt; | Spécifie un déclencheur qui appelle un rendu de page partielle pour le contrôle UpdatePanel donné. Cela peut ou peut ne pas être un contrôle en tant que descendant de UpdatePanel en question. Granulaire de l’événement. Enfant de &lt;déclencheurs&gt;. |
| &lt;asp:PostBackTrigger&gt; | Spécifie un contrôle qui génère la page entière à actualiser. Cela peut ou peut ne pas être un contrôle en tant que descendant de UpdatePanel en question. Granulaire à l’objet. Enfant de &lt;déclencheurs&gt;. |

Le `UpdatePanel` est le contrôle qui délimite le contenu côté serveur qui participera à la fonctionnalité de rendu partiel des Extensions AJAX. Il n’existe aucune limite au nombre de contrôles UpdatePanel qui peuvent se trouver sur une page, et ils peuvent être imbriqués. Chaque UpdatePanel est isolé, afin que chacun peut travailler indépendamment (vous pouvez avoir deux UpdatePanel en cours d’exécution en même temps, rendu des différentes parties de la page, indépendamment de la publication (postback) de la page).

Le contrôle UpdatePanel contrôler principalement des contrats avec des déclencheurs de contrôle - par défaut, n’importe quel contrôle de contenu au sein d’un UpdatePanel `ContentTemplate` qui crée une publication (postback) est inscrit en tant que déclencheur pour le contrôle UpdatePanel. Cela signifie que le contrôle UpdatePanel peut utiliser les contrôles de lié aux données par défaut (par exemple, le contrôle GridView), des contrôles utilisateur, et ils peuvent être programmés dans le script.

Par défaut, lorsqu’un rendu de page partielle est déclenché, tous les contrôles UpdatePanel sur la page seront actualisées, ou non le contrôle UpdatePanel contrôle défini déclencheurs de cette action. Par exemple, si un UpdatePanel définit un contrôle de bouton, ce bouton est activé, tous les contrôles UpdatePanel sur cette page seront actualisées par défaut. C’est pourquoi, par défaut, le `UpdateMode` propriété du contrôle UpdatePanel est définie sur `Always`. Vous pouvez également définir la propriété UpdateMode `Conditional`, ce qui signifie que le UpdatePanel est actualisé uniquement si un déclencheur spécifique est atteint.

## <a name="custom-control-notes"></a>Notes de contrôle personnalisé

Un UpdatePanel peut être ajouté à n’importe quel contrôle utilisateur ou un contrôle personnalisé ; Toutefois, la page sur laquelle ces contrôles sont inclus doit également inclure un contrôle ScriptManager avec la propriété EnablePartialRendering est définie sur **true**.

Une façon dans laquelle vous pourrez compte lors de l’utilisation de contrôles Web personnalisés consiste à remplacer l’élément protégé `CreateChildControls()` méthode de la `CompositeControl` classe. En procédant ainsi, vous pouvez injecter un UpdatePanel entre les enfants du contrôle et le monde extérieur si vous déterminez que la page prend en charge le rendu partiel ; Sinon, vous pouvez simplement superposer les contrôles enfants dans un conteneur `Control` instance.

## <a name="updatepanel-considerations"></a>Considérations de UpdatePanel

Le contrôle UpdatePanel fonctionne comme un élément d’une boîte noire, encapsulant des publications (postbacks) ASP.NET dans le contexte d’un JavaScript XMLHttpRequest. Toutefois, il existe des considérations de performances significatives à l’esprit, à la fois en termes de comportement et la vitesse. Pour comprendre le fonctionne de l’UpdatePanel, afin que vous pouvez décider mieux lorsque son utilisation est appropriée, vous devez examiner l’échange d’AJAX. L’exemple suivant utilise un site existant et, Mozilla Firefox avec l’extension Firebug (Firebug capture les données de XMLHttpRequest).

Envisagez un formulaire comprenant, entre autres choses, une zone de texte code postal qui devrait permettre de remplir un champ Ville et l’état sur un formulaire ou contrôle. Ce formulaire collecte finalement les informations d’appartenance, y compris le nom d’un utilisateur, adresse et les informations de contact. Il existe de nombreuses considérations de conception à prendre en compte, en fonction des exigences d’un projet spécifique.


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image11.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image10.png)

([Cliquez pour afficher l’image en taille réelle](understanding-partial-page-updates-with-asp-net-ajax/_static/image12.png))


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image14.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image13.png)

([Cliquez pour afficher l’image en taille réelle](understanding-partial-page-updates-with-asp-net-ajax/_static/image15.png))


Dans l’itération d’origine de cette application, un contrôle a été créé incorporé de l’intégralité des données d’inscription utilisateur, y compris le code postal, ville et état. L’ensemble du contrôle a été encapsulé dans un UpdatePanel et déposé sur un formulaire Web. Une fois le code postal est entré par l’utilisateur, le contrôle UpdatePanel détecte l’événement (l’événement TextChanged correspondant dans le serveur principal, en spécifiant des déclencheurs ou à l’aide de la propriété ChildrenAsTriggers a la valeur true). AJAX publie tous les champs dans le contrôle UpdatePanel, telles que capturées par FireBug (voir le diagramme de droite).

Comme l’indique la capture d’écran, les valeurs à partir de chaque contrôle dans le contrôle UpdatePanel sont remis (dans ce cas, ils sont tous vides), ainsi que le champ d’état d’affichage. Au total, plus de 9 Ko de données est envoyé lorsqu’en fait uniquement cinq octets de données étaient nécessaires pour effectuer cette demande particulier. La réponse est encore plus volumineux : au total, 57 Ko est envoyé au client, tout simplement pour mettre à jour un champ de texte et un champ de liste déroulante.

Il peut également être intéressant de voir comment ASP.NET AJAX met à jour la présentation. La partie de la réponse de demande de mise à jour de UpdatePanel est indiquée dans l’affichage de la console Firebug sur la gauche ; C’est une spécialement conçue délimitée une barre verticale chaîne qui est divisée par le script client et puis sont assemblés à nouveau sur la page. Plus précisément, ASP.NET AJAX définit le *innerHTML* propriété de l’élément HTML sur le client qui représente votre UpdatePanel. Comme le navigateur génère à nouveau le modèle DOM, il existe un léger délai, en fonction de la quantité d’informations qui doivent être traitées.

La régénération du modèle DOM déclenche un nombre de problèmes supplémentaires :


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image17.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image16.png)

([Cliquez pour afficher l’image en taille réelle](understanding-partial-page-updates-with-asp-net-ajax/_static/image18.png))


- Si l’élément HTML ayant le focus se trouve dans UpdatePanel, il perd le focus. Par conséquent, pour les utilisateurs enfoncée la touche Tab pour quitter la zone de texte code postal, leur destination suivante aurait été la zone de texte Ville. Toutefois, une fois que le contrôle UpdatePanel actualisé l’affichage, le formulaire aurait dû n’est plus le focus, et en appuyant sur Tab aurait démarré la mise en surbrillance les éléments de la vue (notamment les liens).
- Si n’importe quel type de script côté client personnalisé est en cours d’utilisation que les éléments DOM de l’accès, les références conservées par les fonctions peut-être devenir obsolète après une publication automatique partielle.

UpdatePanel n’est pas destinées à être fourre-tout solutions. Au lieu de cela, ils fournissent une solution rapide pour certaines situations, notamment le prototypage, mises à jour du contrôle de petite et fournissent une interface familière aux développeurs ASP.NET qui peuvent être familiarisé avec le modèle d’objet .NET, mais moins donc avec le DOM. Il existe plusieurs alternatives qui peuvent entraîner de meilleures performances, en fonction du scénario d’application :

- Envisagez d’utiliser les PageMethods et JSON (JavaScript Object Notation) permet au développeur d’appeler des méthodes statiques sur une page comme si un appel de service web a été invoqué. Étant donné que les méthodes sont statiques, aucun état n’est nécessaire ; l’appelant de script fournit les paramètres, et le résultat est renvoyé de façon asynchrone.
- Envisagez d’utiliser un Service Web et le JSON si un seul contrôle doit être utilisé dans plusieurs emplacements dans une application. Cela à nouveau nécessite très peu de travail particulière et fonctionne de façon asynchrone.

Incorporation des fonctionnalités via les Services Web ou des méthodes de Page présente également des inconvénients. Tout d’abord, les développeurs ASP.NET ont généralement tendent à générer de petits composants de fonctionnalité dans des contrôles utilisateur (fichiers .ascx). Les méthodes de page ne peut pas être hébergés dans ces fichiers. ils doivent être hébergés au sein de la classe de page .aspx réelle. De même, les services Web, doivent être hébergés au sein de la classe .asmx. Selon l’application, cette architecture peut violer le principe de responsabilité unique, car la fonctionnalité d’un composant unique est désormais répartie sur deux ou plusieurs composants physiques qui peuvent avoir des liens cohésifs peu ou pas.

Enfin, si une application nécessite qu’UpdatePanel est utilisés, les instructions suivantes doivent faciliter la résolution des problèmes et maintenance.

- **Nest UpdatePanel aussi peu que possible, non seulement au sein d’unités, mais également sur des unités de code.** Par exemple, un UpdatePanel sur une Page qui encapsule un contrôle, tandis que ce contrôle contient également un UpdatePanel, qui contient un autre contrôle qui contient un UpdatePanel, est d’avoir une imbrication cross-unité. Cela évite effacer les éléments qui doit être l’actualisation et empêche les actualisations inattendues à enfant UpdatePanel.
- **Conserver le *ChildrenAsTriggers* propriété définie sur false et définir explicitement le déclenchement d’événements.** Utilisation de la `<Triggers>` collection est un moyen beaucoup plus clair pour gérer les événements et peut empêcher un comportement inattendu, en aidant à avec les tâches de maintenance et de forcer un développeur à participer à un événement.
- **Utilisez la plus petite unité possible pour obtenir une fonctionnalité.** Comme indiqué dans la discussion sur le service de code postal d’habillage qu'uniquement le strict minimum réduit le temps pour le serveur, le traitement total et l’encombrement de l’échange client-serveur, amélioration des performances.

## <a name="the-updateprogress-control"></a>Le contrôle UpdateProgress

## <a name="updateprogress-control-reference"></a>Référence de contrôle UpdateProgress

Propriétés du balisage activé :

| **Nom de la propriété** | **Type** | **Description** |
| --- | --- | --- |
| AssociatedUpdate-PanelID | Chaîne | Spécifie l’ID du contrôle UpdatePanel qui UpdateProgress doivent signaler sur. |
| DisplayAfter | Int | Spécifie le délai d’attente en millisecondes avant l’affichage de ce contrôle une fois que la demande asynchrone commence. |
| DispositionDynamique | bool | Spécifie si la progression est restituée dynamiquement. |

Descendants de balisage :

| **Balise** | **Description** |
| --- | --- |
| &lt;ProgressTemplate&gt; | Contient le modèle de contrôle pour le contenu qui s’affichera avec ce contrôle. |

Le contrôle UpdateProgress vous donne une mesure de vos commentaires à maintenir l’intérêt de vos utilisateurs tout en effectuant le travail nécessaire au transport pour le serveur. Cela peut aider vos utilisateurs à savoir que quelque chose même si elle ne soient pas apparent, en particulier parce que la plupart des utilisateurs sont habitués à la page de l’actualisation et voir la barre d’état à mettre en surbrillance.

En tant que note, les contrôles UpdateProgress peuvent apparaître n’importe où sur une hiérarchie de la page. Toutefois, dans le cas dans lequel une publication automatique partielle est lancée à partir d’un UpdatePanel (où un UpdatePanel est imbriqué dans un autre UpdatePanel) enfant, publications que l’enfant UpdatePanel provoque UpdateProgress modèles soient affichés pour l’enfant le déclencheur UpdatePanel, ainsi que le parent UpdatePanel. Toutefois, si le déclencheur est un enfant direct du parent UpdatePanel, seuls les modèles UpdateProgress associés au parent seront affiche.

## <a name="summary"></a>Récapitulatif

Les extensions Microsoft ASP.NET AJAX sont des produits sophistiqués conçus pour aider à rendre le contenu web plus accessibles et pour fournir une expérience utilisateur plus riches à vos applications web. Dans le cadre des Extensions AJAX ASP.NET, les contrôles de rendu de page partielle, y compris les contrôles ScriptManager, UpdatePanel et UpdateProgress sont certains des composants plus visibles du Kit de ressources.

Le composant ScriptManager s’intègre à la fourniture de client JavaScript pour les Extensions, ainsi que des permet aux différents composants côté serveur et côté client collaborer avec les investissements de développement minimal.

Le contrôle UpdatePanel est la zone magique apparente : balisage dans le contrôle UpdatePanel peut avoir code-behind côté serveur et ne déclenche pas une actualisation de la page. Contrôles UpdatePanel peuvent être imbriqués et peuvent dépendre des contrôles dans les autres UpdatePanel. Par défaut, les UpdatePanel gérer toute publication appelée par leurs contrôles descendants, bien que cette fonctionnalité peut être ajustée, soit de manière déclarative ou par programme.

Lorsque vous utilisez le contrôle UpdatePanel, les développeurs doivent connaître l’impact sur les performances peut se produire potentiellement. Les alternatives potentielles incluent les services web et les méthodes de page, bien que la conception de l’application doit être considéré comme.

Le contrôle autorise l’utilisateur à savoir qu’il n’est pas ignoré, et que la demande en arrière-plan se passe lors de la page est sinon pas de UpdateProgress sert à rien pour répondre à l’entrée utilisateur. Il inclut également la possibilité d’abandonner les résultats de rendu partiel.

Ensemble, ces outils vous aider à la création d’une expérience utilisateur riche et transparente par le bon fonctionnement du serveur moins évident à l’utilisateur et d’interrompre le flux de travail moins.

## <a name="bio"></a>Biographie

Scott Cate travaille avec les technologies Web Microsoft depuis 1997 et est le président de myKB.com ([www.myKB.com](http://www.myKB.com)) où il est spécialisé dans l’écriture d’ASP.NET en fonction des applications axées sur les solutions logicielles de la Base de connaissances. Scott peut être contacté par courrier électronique en [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) ou son blog à l’adresse [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Next](understanding-asp-net-ajax-updatepanel-triggers.md)
