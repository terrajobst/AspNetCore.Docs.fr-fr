---
uid: web-forms/overview/moving-to-aspnet-20/server-controls
title: Contrôles serveur | Microsoft Docs
author: microsoft
description: ASP.NET 2.0 améliore les contrôles de serveur à bien des égards. Dans ce module, nous aborderons certaines des modifications architecturales au moyen d’ASP.NET 2.0 et Visual Studio 200...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 43f6ac47-76fc-4cf7-8e9f-c18ce673dfd8
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/server-controls
msc.type: authoredcontent
ms.openlocfilehash: ecf99fa894c1f662542aa8a613195b828bf2c67b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832620"
---
<a name="server-controls"></a>Contrôles serveur
====================
par [Microsoft](https://github.com/microsoft)

> ASP.NET 2.0 améliore les contrôles de serveur à bien des égards. Dans ce module, nous aborderons certaines des modifications architecturales à la façon dont ASP.NET 2.0 et Visual Studio 2005 traite les contrôles serveur.


ASP.NET 2.0 améliore les contrôles de serveur à bien des égards. Dans ce module, nous aborderons certaines des modifications architecturales à la façon dont ASP.NET 2.0 et Visual Studio 2005 traite les contrôles serveur.

## <a name="view-state"></a>État d’affichage

La principale modification apportée état d’affichage dans ASP.NET 2.0 est une réduction considérable de taille. Considérez une page avec un contrôle de calendrier sur celui-ci. Voici l’état d’affichage dans ASP.NET 1.1.

[!code-css[Main](server-controls/samples/sample1.css)]

Maintenant Voici l’état d’affichage sur une page identiques dans ASP.NET 2.0.

[!code-css[Main](server-controls/samples/sample2.css)]

Qui est une modification assez significative et si vous envisagez que l’état d’affichage est reporté dans les deux sens le câble, cette modification peut permettre aux développeurs une augmentation significative des performances. La réduction de taille de l’état d’affichage est en grande partie en raison de la façon dont nous la gérer en interne. N’oubliez pas que chaîne encodée d’état d’affichage comprend en Base64. Pour mieux comprendre la modification en état d’affichage dans ASP.NET 2.0, intéressons-nous à la valeur décodée de l’exemple précédent.

Voici l’état de 1.1 Affichage décodé :

[!code-css[Main](server-controls/samples/sample3.css)]

Cela peut ressembler à un peu incompréhensibles, mais il existe un modèle ici. Dans ASP.NET 1.x, nous caractères uniques permettant d’identifier les types de données et délimité par des valeurs à l’aide de la &lt; &gt; caractères. Le « t » dans l’exemple d’état d’affichage ci-dessus représente un Triplet. Le Triplet contient une paire de ArrayLists (le « l » représente une liste de tableaux.) Un de ces ArrayLists contient un Int32 (« i ») avec une valeur de 1 et l’autre contient un autre Triplet. Le Triplet contient une paire de ArrayLists, etc. Le point important à retenir est que nous utilisons Triplets qui contiennent des paires, nous identifions les types de données via une lettre, et nous utilisons le &lt; et &gt; caractères comme délimiteurs.

Dans ASP.NET 2.0, l’état d’affichage décodé est légèrement différente.

[!code-powershell[Main](server-controls/samples/sample4.ps1)]

Vous devriez remarquer une importante modification dans l’apparence de l’état d’affichage décodé. Cette modification a plusieurs fonctions sous-jacentes architecturales. État d’affichage dans ASP.NET 1.x a utilisé le LosFormatter pour sérialiser les données. Dans la version 2.0, nous utilisons la nouvelle classe ObjectStateFormatter. Cette classe a été spécialement conçue pour faciliter la sérialisation et désérialisation de l’état d’affichage et l’état du contrôle. (État du contrôle sera développée dans la section suivante.) Il existe de nombreux avantages offerts par la modification de la méthode par laquelle la sérialisation et désérialisation ont lieu. Une des plus importantes est le fait que, contrairement à LosFormatter qui utilise un TextWriter, le ObjectStateFormatter utilise BinaryWriter. Cela permet à ASP.NET 2.0 stocker l’état d’affichage une série d’octets au lieu de chaînes. Prenez, par exemple, un entier. Dans ASP.NET 1.1, un entier obligatoire 4 octets de l’état d’affichage. Dans ASP.NET 2.0, cette même entier nécessite uniquement 1 octet. Autres améliorations ont été apportées afin de réduire la quantité d’état d’affichage est stocké. Valeurs DateTime, par exemple, sont maintenant stockés à l’aide d’un TickCount plutôt qu’une chaîne.

Comme si tout cela ne suffisait pas, a été une attention particulière au fait que l’un des utilisateurs plus grands état d’affichage dans la version 1.x a été le DataGrid et contrôles similaires. Un inconvénient majeur de contrôles tels que le contrôle DataGrid qui concerne l’état d’affichage est qu’il contient souvent de grandes quantités d’informations répétées. Dans ASP.NET 1.x, qui répète les informations ont été stockées simplement et à nouveau résultant dans un état d’affichage surgonflé. Dans ASP.NET 2.0, nous utilisons la nouvelle classe IndexedString pour stocker ces données. Si une chaîne de manière répétée, nous stockons simplement le jeton pour le IndexedString et l’index dans une table en cours d’exécution d’objets de IndexedString.

## <a name="control-state"></a>État du contrôle

Les principales doléances que les développeurs devaient avec l’état d’affichage a la taille elle ajoutée à la charge utile HTTP. Comme mentionné précédemment, l’un des utilisateurs plus grands état d’affichage est le contrôle DataGrid. Pour éviter les grandes quantités d’état d’affichage généré par un contrôle DataGrid, de nombreux développeurs désactivé simplement d’état d’affichage pour ce contrôle. Malheureusement, cette solution n’était pas toujours un bon. État d’affichage dans ASP.NET 1.x contient non seulement les données nécessaires pour le bon fonctionnement du contrôle. Il contient également des informations concernant l’état de l’interface utilisateur du contrôle. Cela signifie que si vous souhaitez autoriser pour la pagination sur une grille de données, vous devez activer l’état d’affichage même si vous n’avez pas besoin de toutes les informations de l’interface utilisateur qui permet d’afficher état contient. Il est un scénario tout ou rien.

Dans ASP.NET 2.0, l’état du contrôle résout ce problème bien via l’introduction de l’état du contrôle. État du contrôle contient les données qui est absolument nécessaires pour le bon fonctionnement d’un contrôle. Contrairement à l’état d’affichage, l’état du contrôle ne peut pas être désactivée. Par conséquent, il est important que les données sont stockées dans l’état du contrôle sont soigneusement contrôlées.

> [!NOTE]
> État de contrôle est conservé, ainsi que l’état d’affichage dans le \_ \_champ de formulaire masqué d’état d’affichage.


Cette vidéo est une procédure pas à pas de l’état d’affichage et l’état du contrôle.


![](server-controls/_static/image1.png)


[Ouvre vidéo plein écran](server-controls/_static/state1.wmv)


Dans l’ordre d’un contrôle serveur lire et écrire pour contrôler l’état, vous devez prendre trois étapes.

## <a name="step-1-call-the-registerrequirescontrolstate-method"></a>Étape 1 : Appeler la méthode RegisterRequiresControlState

La méthode RegisterRequiresControlState informe ASP.NET qu’un contrôle doit rendre persistant l’état du contrôle. Il prend un argument de type contrôle qui est le contrôle est en cours d’inscription.

Il est important de noter que l’inscription n’est pas persistante à partir d’une demande à l’autre. Par conséquent, cette méthode doit être appelée à chaque demande si un contrôle consiste à conserver l’état du contrôle. Il est recommandé que la méthode est appelée par OnInit.

[!code-csharp[Main](server-controls/samples/sample5.cs)]

## <a name="step-2-override-savecontrolstate"></a>Étape 2 : Remplacer SaveControlState

La méthode SaveControlState enregistre les changements d’état pour un contrôle de contrôle depuis la dernière publication. Elle retourne un objet représentant l’état du contrôle.

## <a name="step-3-override-loadcontrolstate"></a>Étape 3 : Remplacer LoadControlState

La méthode LoadControlState charge un état enregistré dans un contrôle. La méthode accepte un argument de type Object, qui contient l’état enregistré pour le contrôle.

## <a name="full-xhtml-compliance"></a>Conformité XHTML complète

N’importe quel développeur Web connaît l’importance des normes dans les applications Web. Afin de maintenir un environnement de développement basé sur des normes, ASP.NET 2.0 est entièrement compatible XHTML. Par conséquent, toutes les balises sont rendus en fonction des normes XHTML dans les navigateurs qui prennent en charge HTML 4.0 ou version ultérieure.

La définition de type de document dans ASP.NET 1.1 était le suivant :

[!code-html[Main](server-controls/samples/sample6.html)]

Dans ASP.NET 2.0, la définition de type de document par défaut est la suivante :

[!code-html[Main](server-controls/samples/sample7.html)]

Si vous choisissez, vous pouvez modifier la conformité de XHML par défaut via le nœud xhtmlConformance dans le fichier de configuration. Par exemple, le nœud suivant dans le fichier web.config sera alors la conformité XHTML XHTML 1.0 Strict :

[!code-xml[Main](server-controls/samples/sample8.xml)]

Si vous choisissez, vous pouvez également configurer ASP.NET pour utiliser la configuration héritée utilisée dans ASP.NET 1.x comme suit :

[!code-xml[Main](server-controls/samples/sample9.xml)]

## <a name="adaptive-rendering-using-adapters"></a>Adaptive rendu à l’aide d’adaptateurs

Dans ASP.NET 1.x, le fichier de configuration contenu un &lt;browserCaps&gt; section qui rempli un objet HttpBrowserCapabilities. Cet objet autorisé à un développeur de déterminer quel périphérique effectue une demande particulière et de restituer le code en conséquence. Dans ASP.NET 2.0, le modèle a amélioré et utilise désormais la nouvelle classe ControlAdapter. La classe ControlAdapter remplace les événements de cycle de vie du contrôle et contrôle la restitution des contrôles en fonction des fonctionnalités de l’agent utilisateur. Les fonctionnalités d’un agent utilisateur spécifiques sont définies par un fichier de définition de navigateur (il s’agit d’un fichier avec une extension de fichier browser) stocké dans le c:\windows\microsoft.net\framework\v2.0. \* \* \* \*Dossier \CONFIG\Browsers.

> [!NOTE]
> La classe ControlAdapter est une classe abstraite.


Comme beaucoup le &lt;browserCaps&gt; section dans la version 1.x, le fichier de définition de navigateur utilise une Expression régulière pour analyser la chaîne d’agent utilisateur afin d’identifier le navigateur demandeur. Il les définit des fonctionnalités particulières pour que l’agent utilisateur. La ControlAdapter restitue le contrôle par le biais de la méthode Render. Par conséquent, si vous substituez la méthode Render, vous ne devez pas appeler rendu sur la classe de base. Cela peut entraîner le rendu de se produire à deux reprises, une fois pour l’adaptateur et une fois pour le contrôle proprement dit.

## <a name="developing-a-custom-adapter"></a>Développez un adaptateur personnalisé

Vous pouvez développer votre propre adaptateur personnalisé en héritant de ControlAdapter. En outre, vous pouvez hériter de la classe abstraite PageAdapter dans les cas où un adaptateur est nécessaire pour une page. Mappage des contrôles à votre adaptateur personnalisé s’effectue le &lt;controlAdapters&gt; élément dans le fichier de définition de navigateur. Par exemple, le code XML suivant à partir d’un fichier de définition de navigateur mappe le contrôle de Menu à la classe MenuAdapter :

[!code-html[Main](server-controls/samples/sample10.html)]

À l’aide de ce modèle, il devient très facile pour un développeur de contrôle cibler un périphérique particulier ou un navigateur. Il est également très simple pour un développeur d’avoir un contrôle complet sur le mode de rendu des pages sur chaque appareil.

## <a name="per-device-rendering"></a>Rendu par appareil

Propriétés du contrôle serveur dans ASP.NET 2.0 peuvent être spécifié par appareil à l’aide d’un préfixe spécifique au navigateur. Par exemple, le code ci-dessous sera modifier le texte d’une étiquette en fonction de périphérique qui est utilisé pour parcourir la page.

[!code-aspx[Main](server-controls/samples/sample11.aspx)]

Lorsque la page contenant cette étiquette est consultée à partir d’Internet Explorer, l’étiquette affiche le texte « Vous explorez à partir d’Internet Explorer. » Lorsque la page est consultée à partir de Firefox, l’étiquette affiche le texte « Vous accédez à partir de Firefox. » Lorsque la page est consultée à partir de n’importe quel autre appareil, il affiche « Vous explorez à partir d’un appareil inconnu. » Toute propriété peut être spécifiée à l’aide de cette syntaxe spéciale.

## <a name="setting-focus"></a>Définition du Focus

Les développeurs ASP.NET 1.x fréquentes sur la façon de définir le focus initial sur un contrôle particulier. Par exemple, sur une page de connexion, il est utile d’avoir à la zone de texte Nom d’utilisateur d’obtenir le focus lors du premier charge de la page. Dans ASP.NET 1.x, cela exigeaient l’écriture d’un script côté client. Même si un tel script est une tâche facile, il n’est plus nécessaire dans ASP.NET 2.0 grâce à la méthode SetFocus. La méthode SetFocus accepte un argument qui indique le contrôle qui doit recevoir le focus. Cet argument peut être l’ID client du contrôle sous forme de chaîne ou le nom du contrôle serveur en tant qu’objet contrôle. Par exemple, pour définir le focus initial à un contrôle TextBox appelé txtUserID lors du premier charge de la page, ajoutez le code suivant à la Page\_charge :

[!code-csharp[Main](server-controls/samples/sample12.cs)]

--ou

[!code-csharp[Main](server-controls/samples/sample13.cs)]

ASP.NET 2.0 utilise le Gestionnaire de Webresource.axd (abordé précédemment) pour restituer une fonction côté client qui définit le focus. Le nom de la fonction côté client est WebForm\_AutoFocus, comme illustré ici :

[!code-html[Main](server-controls/samples/sample14.html)]

Ou bien, vous pouvez utiliser la méthode Focus pour un contrôle pour définir le focus initial à ce contrôle. La méthode Focus dérive de la classe de contrôle et est disponible à tous les contrôles d’ASP.NET 2.0. Il est également possible de définir le focus sur un contrôle particulier lorsqu’une erreur de validation se produit. Qui est abordée dans un autre module.

## <a name="new-server-controls-in-aspnet-20"></a>Nouveaux contrôles serveur dans ASP.NET 2.0

Voici les nouveaux contrôles serveur dans ASP.NET 2.0. Nous verrons plus en détail certains d'entre eux dans les modules.

## <a name="imagemap-control"></a>Contrôle ImageMap

Le contrôle ImageMap vous permet de vous permet d’ajouter des zones réactives d’une image qui peut initier une publication ou accéder à une URL. Il existe trois types de points d’accès disponibles ; CircleHotSpot, RectangleHotSpot et PolygonHotSpot. Zones réactives sont ajoutés via un éditeur de collections dans Visual Studio ou par programme dans le code. Aucune interface utilisateur n’est disponible pour dessiner des zones réactives sur une image. Les coordonnées et la taille ou le rayon de la zone réactive doit être spécifié de façon déclarative. Il n’existe également aucune représentation visuelle d’une zone réactive dans le concepteur. Si une zone réactive est configurée pour accéder à une URL, l’URL est spécifiée par le biais de la propriété NavigateUrl de la zone réactive. Dans le cas d’un billet de sauvegarder la zone réactive, le PostBackValue propriété vous permet de passer une chaîne dans la publication qui peut être récupérée dans le code côté serveur.


![Éditeur de collections hotSpot dans Visual Studio](server-controls/_static/image1.jpg)

**Figure 1**: éditeur de collections HotSpot dans Visual Studio


## <a name="bulletedlist-control"></a>Contrôle BulletedList

Le contrôle BulletedList est une liste à puces qui peut être facilement lié aux données. La liste peut être triée (numérotés) ou non ordonnées par le biais de la propriété BulletStyle. Chaque élément dans la liste est représenté par un objet ListItem.


![Contrôle BulletedList dans Visual Studio](server-controls/_static/image1.gif)

**Figure 2**: contrôle BulletedList dans Visual Studio


## <a name="hiddenfield-control"></a>Contrôle HiddenField

Le contrôle HiddenField ajoute un champ de formulaire masqué à votre page, dont la valeur est disponible dans le code côté serveur. La valeur d’un champ de formulaire masqué doit généralement rester inchangés entre publications. Toutefois, il est possible pour un utilisateur malveillant peut modifier l’avant de valeur pour la publication. Si cela se produit, le contrôle HiddenField déclenche l’événement ValueChanged. Si vous disposez des informations sensibles dans le contrôle HiddenField et vous souhaitez vous assurer qu’il reste inchangé, vous devez gérer l’événement ValueChanged dans votre code.

## <a name="fileupload-control"></a>Contrôle fileUpload

Le contrôle FileUpload d’ASP.NET 2.0 rend possible de télécharger des fichiers vers un serveur Web via une page ASP.NET. Ce contrôle est assez semblable à la classe ASP.NET 1.x HtmlInputFile à quelques exceptions près. Dans ASP.NET 1.x, il était recommandé que la propriété PostedFile valeur null est recherchée afin de déterminer si vous aviez un bon fichier. Le contrôle FileUpload d’ASP.NET 2.0 ajoute une nouveau HasFile, propriété que vous pouvez utiliser dans le même but, et il est un peu plus efficace.

La propriété PostedFile est toujours disponible pour l’accès à un objet HttpPostedFile, mais certaines fonctionnalités de la HttpPostedFile est maintenant disponible intrinsèquement avec le contrôle FileUpload. Par exemple, pour enregistrer un fichier téléchargé dans ASP.NET 1.x, vous appelez la méthode SaveAs sur l’objet HttpPostedFile. À l’aide du contrôle FileUpload d’ASP.NET 2.0, vous appelez la méthode SaveAs sur le contrôle FileUpload proprement dit.

Un autre changement important dans le comportement 2.0 (et probablement le changement le plus significatif) est qu’il n’est plus nécessaire charger un fichier téléchargé entier en mémoire avant de l’enregistrer. Enregistrement d’un fichier qui a été chargé dans la version 1.x, entièrement en mémoire avant d’en cours d’écriture sur le disque. Cette architecture empêche le téléchargement de fichiers volumineux.

Dans ASP.NET 2.0, l’attribut de l’élément httpRuntime requestLengthDiskThreshold vous permet de configurer le nombre de kilo-octets sont conservées dans une mémoire tampon en mémoire avant d’en cours d’écriture sur le disque.

**IMPORTANT**: MSDN documentation (documentation et un autre emplacement) Spécifie que cette valeur est exprimée en octets (et non en kilo-octets) et que la valeur par défaut est 256. La valeur est réellement spécifiée en kilo-octets et la valeur par défaut est 80. En ayant une valeur par défaut de 80 Ko, nous nous assurer que la mémoire tampon n’arrivent pas sur le tas d’objets volumineux.

## <a name="wizard-control"></a>Contrôle de l’Assistant

Il est assez courant de rencontrer des difficultés avec la tentative d’obtention des informations contenues dans une série de « pages » à l’aide des panneaux ou par le transfert d’une page à des développeurs ASP.NET. Plus souvent à pas, le défi est frustrant et demande beaucoup de temps. Le nouveau contrôle de l’Assistant résout les problèmes en permettant aux étapes linéaires et non linéaires dans une interface d’Assistant que les utilisateurs connaissent. Le contrôle de l’Assistant présente des formulaires de saisie dans une série d’étapes. Chaque étape est d’un type particulier, spécifié par la propriété StepType du contrôle. Les types d’étape disponibles sont les suivantes :

| **Type d’étape** | **Explication** |
| --- | --- |
| Auto | L’Assistant détermine automatiquement le type d’étape en fonction de sa position dans la hiérarchie de l’étape. |
| Start | La première étape, souvent utilisée pour présenter une instruction d’introduction. |
| Étape | Une étape normale. |
| Terminer | L’étape finale, généralement utilisée pour présenter un bouton pour terminer l’Assistant. |
| Terminé | Affiche un message à communiquer la réussite ou l’échec. |

> [!NOTE]
> Le contrôle de l’Assistant effectue le suivi de son état en utilisant l’état de contrôle d’ASP.NET. Par conséquent, la propriété EnableViewState peut être définie sur false, sans aucun préjudice.


Cette vidéo est une procédure pas à pas de contrôle de l’Assistant.


![](server-controls/_static/image2.png)


[Ouvre vidéo plein écran](server-controls/_static/wizard1.wmv)


## <a name="localize-control"></a>Localiser le contrôle

Le contrôle Localize est similaire à un contrôle littéral. Toutefois, le contrôle Localize a un **Mode** propriété qui contrôle le rendu de balisage qui est ajouté à ce dernier. La propriété Mode prend en charge les valeurs suivantes :

| **Mode** | **Explication** |
| --- | --- |
| Transformer | Balisage est transformé conformément au protocole du navigateur qui effectue la requête. |
| Transfert direct | Balisage est restitué en tant que-est. |
| Encoder | Balisage qui est ajouté au contrôle est encodé à l’aide de HtmlEncode. |

## <a name="multiview-and-view-controls"></a>Contrôles multiView et View

Le contrôle MultiView agit comme un conteneur pour les contrôles d’affichage et le contrôle agit comme un conteneur (un peu comme un contrôle de panneau) pour d’autres contrôles. Chaque vue dans un contrôle MultiView est représenté par un contrôle d’affichage unique. Le premier contrôle de vue dans le MultiView est 0, le deuxième est le mode 1, etc. Vous pouvez changer de vue en spécifiant le ActiveViewIndex du contrôle MultiView.

## <a name="substitution-control"></a>Contrôle de substitution

Le contrôle de Substitution est utilisé conjointement avec la mise en cache ASP.NET. Dans le cas où vous souhaitez tirer parti de la mise en cache, mais que vous avez des parties d’une page qui doit être mis à jour sur chaque requête (en d’autres termes, des parties d’une page qui sont exempts de la mise en cache), le composant de Substitution fournit une excellente solution. Le contrôle n’a réellement n’affiche aucune sortie sur son propre. Au lieu de cela, elle est liée à une méthode dans le code côté serveur. Lorsque la page est demandée, la méthode est appelée et le balisage retourné est restitué à la place le contrôle de substitution.

La méthode à laquelle le contrôle de Substitution est lié est spécifiée via le **Nom_méthode** propriété. Cette méthode doit respecter les critères suivants :

- Il doit être une méthode statique (shared dans VB).
- Il accepte un paramètre de type HttpContext.
- Elle retourne une chaîne représentant le balisage qui doit remplacer le contrôle sur la page.

Le contrôle de Substitution n’a pas la possibilité de modifier n’importe quel autre contrôle dans la page, mais il n’a pas accès à la classe HttpContext actuelle par le biais de son paramètre.

## <a name="gridview-control"></a>Contrôle GridView

Le contrôle GridView est le remplacement du contrôle DataGrid. Ce contrôle est abordé plus en détail dans un autre module.

## <a name="detailsview-control"></a>Contrôle DetailsView

Le contrôle DetailsView vous permet d’afficher un enregistrement unique d’une source de données et modifier ou la supprimer. Il est couvert plus en détail dans un autre module.

## <a name="formview-control"></a>Contrôle FormView

Le contrôle FormView est utilisé pour afficher un enregistrement unique d’une source de données dans une interface configurable. Il est couvert plus en détail dans un autre module.

## <a name="accessdatasource-control"></a>AccessDataSource (contrôle)

Ce contrôle est utilisé pour lier des données une base de données Access. Il est couvert plus en détail dans un autre module.

## <a name="objectdatasource-control"></a>Contrôle ObjectDataSource

Le contrôle ObjectDataSource est utilisé pour prendre en charge une architecture à trois niveaux afin que les contrôles peuvent être lié aux données à un objet métier de couche intermédiaire, par opposition à un modèle à deux niveaux où les contrôles sont liés directement à la source de données. Il nous reviendrons plus en détail dans un autre module.

## <a name="xmldatasource-control"></a>Contrôle XmlDataSource

Le contrôle XmlDataSource est utilisé pour lier les données à une source de données XML. Il est couvert plus en détail dans un autre module.

## <a name="sitemapdatasource-control"></a>Contrôle SiteMapDataSource

Le contrôle SiteMapDataSource fournit la liaison de données pour les contrôles de navigation de site basé sur un plan de site. Il nous reviendrons plus en détail dans un autre module.

## <a name="sitemappath-control"></a>Contrôle SiteMapPath

Le contrôle SiteMapPath affiche une série de liens de navigation communément appelé fil d’Ariane. Il est couvert plus en détail dans un autre module.

## <a name="menu-control"></a>Contrôle Menu

Le contrôle de Menu affiche des menus dynamiques à l’aide de DHTML. Il est couvert plus en détail dans un autre module.

## <a name="treeview-control"></a>TreeView, contrôle

Le contrôle TreeView est utilisé pour afficher une arborescence hiérarchique de données. Il est couvert plus en détail dans un autre module.

## <a name="login-control"></a>Contrôle de connexion

Le contrôle de connexion fournit un mécanisme pour vous connecter à un site Web. Il est couvert plus en détail dans un autre module.

## <a name="loginview-control"></a>Contrôle LoginView

Le contrôle LoginView permet d’afficher différents modèles basé sur l’état de la connexion d’un utilisateur. Il est couvert plus en détail dans un autre module.

## <a name="passwordrecovery-control"></a>Contrôle PasswordRecovery

Le contrôle PasswordRecovery permet de récupérer les mots de passe oubliés par les utilisateurs d’une application ASP.NET. Il est couvert plus en détail dans un autre module.

## <a name="loginstatus"></a>LoginStatus

Ce contrôle affiche l’état de la connexion d’un utilisateur. Il est couvert plus en détail dans un autre module.

## <a name="loginname"></a>LoginName

Le contrôle LoginName affiche un nom d’utilisateur après étant enregistrées dans une application ASP.NET. Il est couvert plus en détail dans un autre module.

## <a name="createuserwizard"></a>CreateUserWizard

CreateUserWizard est un Assistant configurable qui offre aux utilisateurs la possibilité de créer un compte de l’appartenance ASP.NET pour une utilisation dans une application ASP.NET. Il est couvert plus en détail dans un autre module.

## <a name="changepassword"></a>ChangePassword

Le contrôle ChangePassword permet aux utilisateurs de modifier leur mot de passe pour une application ASP.NET. Il est couvert plus en détail dans un autre module.

## <a name="various-webparts"></a>WebParts différents

ASP.NET 2.0 est livré avec les différentes parties du Web. Ils seront traités en détail dans un autre module.
