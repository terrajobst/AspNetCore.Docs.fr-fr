---
uid: web-forms/overview/older-versions-security/membership/creating-user-accounts-cs
title: Création de comptes d’utilisateur (c#) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons Explorer à l’aide de l’infrastructure de l’appartenance (via SqlMembershipProvider) pour créer de nouveaux comptes d’utilisateur. Nous verrons comment créer de nouveaux nous...
ms.author: aspnetcontent
ms.date: 01/18/2008
ms.assetid: f175278c-6079-4d91-b9b4-2493ed43d9ec
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-user-accounts-cs
msc.type: authoredcontent
ms.openlocfilehash: 7caa6b614bbe6545929a9b201de7f30fe95e6481
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842622"
---
<a name="creating-user-accounts-c"></a>Création de comptes d’utilisateur (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_05_CS.zip) ou [télécharger le PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial05_CreatingUsers_cs.pdf)

> Dans ce didacticiel, nous allons Explorer à l’aide de l’infrastructure de l’appartenance (via SqlMembershipProvider) pour créer de nouveaux comptes d’utilisateur. Nous verrons comment créer des utilisateurs via les pages ASP et par programme. Contrôle intégré CreateUserWizard du NET.


## <a name="introduction"></a>Introduction

Dans le <a id="_msoanchor_1"> </a> [didacticiel précédent](creating-the-membership-schema-in-sql-server-cs.md) nous avons installé le schéma de services d’application dans une base de données, ajouté les tables, vues et procédures requises par le `SqlMembershipProvider` et `SqlRoleProvider`. Cela créé l’infrastructure que nous aurons besoin pour le reste des didacticiels de cette série. Dans ce didacticiel, nous allons Explorer à l’aide de l’infrastructure de l’appartenance (via le `SqlMembershipProvider`) pour créer de nouveaux comptes d’utilisateur. Nous verrons comment créer des utilisateurs via les pages ASP et par programme. Contrôle intégré CreateUserWizard du NET.

En plus de savoir comment créer des comptes d’utilisateur, vous devrez également mettre à jour le site Web de démonstration que nous avons tout d’abord créé dans le *<a id="_msoanchor_2"> </a> [une vue d’ensemble de l’authentification par formulaire](../introduction/an-overview-of-forms-authentication-cs.md)* didacticiel puis l’améliorer dans le  *<a id="https://www.asp.net/learn/security/tutorial-03-cs.aspx"> </a> Configuration de l’authentification de formulaires et des sujets avancés* didacticiel. Notre application web de démonstration comprend une page de connexion qui valide les informations d’identification des utilisateurs contre les paires codées en dur nom d’utilisateur/mot de passe. En outre, `Global.asax` inclut du code qui crée personnalisé `IPrincipal` et `IIdentity` objets pour les utilisateurs authentifiés. Nous mettrons à jour la page de connexion pour valider les informations d’identification des utilisateurs par rapport à l’infrastructure de l’appartenance et supprimez la logique personnalisée principal et identity.

C’est parti !

## <a name="the-forms-authentication-and-membership-checklist"></a>L’authentification par formulaire et la liste de vérification de l’appartenance

Avant de commencer à travailler avec l’infrastructure de l’appartenance, prenons un moment pour passer en revue les étapes importantes, que nous avons emprunté pour atteindre ce point. Lors de l’utilisation de l’infrastructure de l’appartenance avec le `SqlMembershipProvider` dans un scénario de l’authentification basée sur les formulaires, les étapes suivantes doivent être effectuées avant d’implémenter la fonctionnalité d’appartenance dans votre application web :

1. **Activer l’authentification basée sur les formulaires.** Comme expliqué dans  *<a id="_msoanchor_4"> </a> [une vue d’ensemble de l’authentification par formulaire](../introduction/an-overview-of-forms-authentication-cs.md)*, l’authentification par formulaire est activée en modifiant `Web.config` et en définissant le `<authentication>` l’élément `mode` attribut `Forms`. Avec l’authentification par formulaire est activée, chaque requête entrante est examiné pour un *ticket d’authentification de formulaires*, qui, le cas échéant, identifie le demandeur.
2. **Ajouter le schéma de services d’application à la base de données approprié.** Lorsque vous utilisez la `SqlMembershipProvider` nous devons installer le schéma de services d’application à une base de données. Généralement, ce schéma est ajouté à la même base de données qui contient le modèle de données de l’application. Le *<a id="_msoanchor_5"> </a> [création du schéma d’appartenance dans SQL Server](creating-the-membership-schema-in-sql-server-cs.md)* didacticiel examiné à l’aide de la `aspnet_regsql.exe` outil pour effectuer cette opération.
3. **Personnaliser les paramètres de l’Application Web pour faire référence à la base de données de l’étape 2.** Le *création du schéma d’appartenance dans SQL Server* didacticiel vous a montré les deux façons de configurer l’application web afin que le `SqlMembershipProvider` utiliserait la base de données sélectionnée à l’étape 2 : en modifiant le `LocalSqlServer` nom de chaîne de connexion ; ou en ajoutant un nouveau fournisseur inscrit à la liste des fournisseurs d’appartenances framework et de personnalisation de ce nouveau fournisseur pour utiliser la base de données à partir de l’étape 2.

Lorsque la création d’une application web qui utilise le `SqlMembershipProvider` et l’authentification basée sur les formulaires, vous devez effectuer ces trois étapes avant d’utiliser la `Membership` classe ou les contrôles Web de connexion ASP.NET. Étant donné que nous avons déjà effectué ces étapes dans les didacticiels précédents, nous sommes prêts à commencer à utiliser l’infrastructure Membership !

## <a name="step-1-adding-new-aspnet-pages"></a>Étape 1 : Ajout de nouvelles Pages ASP.NET

Dans ce didacticiel et les trois suivants, nous étudierons diverses fonctionnalités et fonctions d’appartenance. Nous aurons besoin d’une série de pages ASP.NET pour implémenter les rubriques examinés tout au long de ces didacticiels. Nous allons créer ces pages, puis un fichier de mappage de site `(Web.sitemap)`.

Commencez par créer un nouveau dossier dans le projet nommé `Membership`. Ensuite, ajoutez cinq nouvelles pages ASP.NET à le `Membership` dossier, lier chaque page avec le `Site.master` page maître. Nommez les pages :

- `CreatingUserAccounts.aspx`
- `UserBasedAuthorization.aspx`
- `EnhancedCreateUserWizard.aspx`
- `AdditionalUserInfo.aspx`
- `Guestbook.aspx`

À ce stade l’Explorateur de solutions de votre projet doit ressembler à l’écran illustré à la Figure 1.


[![Cinq nouvelles Pages ont été ajoutés dans le dossier de l’appartenance](creating-user-accounts-cs/_static/image2.png)](creating-user-accounts-cs/_static/image1.png)

**Figure 1**: cinq nouvelles Pages ont été ajoutées à la `Membership` dossier ([cliquez pour afficher l’image en taille réelle](creating-user-accounts-cs/_static/image3.png))


Chaque page doit avoir à ce stade, les deux contrôles de contenu, une pour chaque ContentPlaceHolders de la page maître : `MainContent` et `LoginContent`.

[!code-aspx[Main](creating-user-accounts-cs/samples/sample1.aspx)]

N’oubliez pas que le `LoginContent` balisage par défaut de ContentPlaceHolder affiche un lien pour se connecter ou se déconnecter du site, selon si l’utilisateur est authentifié. La présence de la `Content2` contrôle de contenu, remplace toutefois, le balisage de la page maître par défaut. Comme expliqué dans *<a id="_msoanchor_6"> </a> [une vue d’ensemble de l’authentification par formulaire](../introduction/an-overview-of-forms-authentication-cs.md)* (didacticiel), cela est utile dans les pages où nous ne souhaitez pas afficher les options relatives à la connexion dans la colonne de gauche.

Pour ces cinq pages, toutefois, nous souhaitons afficher le balisage par défaut de la page maître pour la `LoginContent` ContentPlaceHolder. Par conséquent, supprimez le balisage déclaratif pour la `Content2` contrôle de contenu. Après cela, chacun de balisage de la page cinq doit contenir qu’un seul contrôle de contenu.

## <a name="step-2-creating-the-site-map"></a>Étape 2 : Création du plan du Site

Tous, mais les sites Web plus anecdotiques doivent implémenter une certaine forme d’une interface utilisateur de navigation. L’interface utilisateur de navigation peut être une simple liste de liens vers les différentes sections du site. Vous pouvez également ces liens peuvent être réorganisées dans les menus ou les vues de l’arborescence. Les développeurs de pages, création de l’interface utilisateur de navigation est seulement la moitié de l’histoire. Nous devons également des moyens permettant de définir la structure du site logique de manière plus facile à gérer et mettre à jour. Comme les nouvelles pages sont ajoutés ou supprimés des pages existantes, nous voulons être en mesure de mettre à jour d’une source unique – le plan du site – et répercuter ces modifications sur l’interface d’utilisateur de navigation du site.

Ces deux tâches – définissant le mappage de site et l’implémentation d’une interface utilisateur de navigation basée sur la carte de site – sont faciles à accomplir grâce à l’infrastructure de plan de Site et les contrôles de Navigation Web ajoutées dans ASP.NET version 2.0. Permet à l’infrastructure de plan de Site pour un développeur de définir un plan de site, puis y accéder via une API programmatique (le [ `SiteMap` classe](https://msdn.microsoft.com/library/system.web.sitemap.aspx)). Contrôles de Navigation Web intégrés incluent un [contrôle Menu](https://msdn.microsoft.com/library/bz09dy46.aspx), le [contrôle TreeView](https://msdn.microsoft.com/library/3eafky27.aspx)et le [contrôle SiteMapPath](https://msdn.microsoft.com/library/3eafky27.aspx).

Comme les infrastructures d’appartenance et des rôles, l’infrastructure de plan de Site est construit sur le [modèle de fournisseur](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx). Le travail de la classe de fournisseur de plan de Site consiste à générer la structure en mémoire utilisée par le `SiteMap` classe à partir d’un magasin de données persistantes, comme un fichier XML ou une table de base de données. Le .NET Framework est livré avec un fournisseur de plan de Site par défaut qui lit les données de plan de site à partir d’un fichier XML ([`XmlSiteMapProvider`](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx)), et c’est le fournisseur, nous allons utiliser dans ce didacticiel. Pour certaines implémentations de fournisseur de plan du Site secondaires, consultez la section relevés supplémentaire à la fin de ce didacticiel.

Le fournisseur de plan de Site par défaut attend un fichier XML correctement mis en forme nommé `Web.sitemap` existe le répertoire racine. Étant donné que nous utilisons ce fournisseur par défaut, nous devons ajouter ce type de fichier et de définir la structure de la carte site dans le format XML approprié. Pour ajouter le fichier, avec le bouton droit sur le nom du projet dans l’Explorateur de solutions et choisissez Ajouter un nouvel élément. À partir de la boîte de dialogue Choisir d’ajouter un fichier de type de plan de Site nommé `Web.sitemap`.


[![Ajoutez un fichier nommé Web.sitemap vers le répertoire du projet racine](creating-user-accounts-cs/_static/image5.png)](creating-user-accounts-cs/_static/image4.png)

**Figure 2**: ajouter un fichier nommé `Web.sitemap` vers le répertoire du projet racine ([cliquez pour afficher l’image en taille réelle](creating-user-accounts-cs/_static/image6.png))


Le fichier de mappage de site XML définit la structure du site Web sous forme de hiérarchie. Cette relation hiérarchique est modélisée dans le fichier XML par le biais de l’ascendance de la `<siteMapNode>` éléments. Le `Web.sitemap` doit commencer par un `<siteMap>` nœud parent qui a précisément un `<siteMapNode>` enfant. Ce niveau supérieur `<siteMapNode>` élément représente la racine de la hiérarchie et peut avoir un nombre arbitraire de nœuds descendants. Chaque `<siteMapNode>` élément doit inclure un `title` d’attribut et peut éventuellement inclure `url` et `description` attributs, entre autres ; chaque vide `url` attribut doit être unique.

Entrez le code XML suivant dans le `Web.sitemap` fichier :

[!code-xml[Main](creating-user-accounts-cs/samples/sample2.xml)]

Le balisage de carte de site ci-dessus définit la hiérarchie affichée dans la Figure 3.


[![Le plan de Site représente une Structure de navigation hiérarchique](creating-user-accounts-cs/_static/image8.png)](creating-user-accounts-cs/_static/image7.png)

**Figure 3**: le plan de Site représente une Structure de navigation hiérarchique ([cliquez pour afficher l’image en taille réelle](creating-user-accounts-cs/_static/image9.png))


## <a name="step-3-updating-the-master-page-to-include-a-navigational-user-interface"></a>Étape 3 : Mise à jour de la Page maître pour inclure une Interface utilisateur de navigation

ASP.NET inclut un nombre de contrôles Web navigation pour la conception d’une interface utilisateur. Celles-ci incluent le Menu, TreeView et les contrôles SiteMapPath. Les contrôles Menu et TreeView restituent la structure de plan de site dans un menu ou une arborescence, respectivement, tandis que le contrôle SiteMapPath affiche un fil d’Ariane qui affiche le nœud actuel qui est visité, ainsi que ses ancêtres. Les données de plan de site peut être lié à d’autres données à l’aide de SiteMapDataSource de contrôles Web et accessibles par programme le `SiteMap` classe.

Dans la mesure où une étude approfondie de l’infrastructure de plan de Site et les contrôles de Navigation est dépasse le cadre de cette série de didacticiels, plutôt que de consacrer du temps créant notre propre interface utilisateur de navigation nous allons plutôt empruntez celui utilisé dans mon *[ Utilisation des données dans ASP.NET 2.0](../../data-access/index.md)* série de didacticiels, qui utilise un contrôle Repeater pour afficher une liste à puces deux-deep de liens de navigation, comme illustré dans la Figure 4.

### <a name="adding-a-two-level-list-of-links-in-the-left-column"></a>Ajout d’une liste de deux niveaux de liens dans la colonne de gauche

Pour créer cette interface, ajoutez le balisage déclaratif suivant à la `Site.master` colonne gauche de la page maître où le texte « TODO : Menu ici... » réside actuellement.

[!code-aspx[Main](creating-user-accounts-cs/samples/sample3.aspx)]

Le balisage ci-dessus lie un contrôle Repeater nommé `menu` à un SiteMapDataSource, qui retourne la hiérarchie de plan de site définie dans `Web.sitemap`. Depuis le contrôle SiteMapDataSource [ `ShowStartingNode` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) est définie sur False commencent à être renvoyées la hiérarchie de la carte de site en commençant par les descendants du nœud « Home ». Le Repeater affiche chacun de ces nœuds (actuellement uniquement « appartenance ») dans un `<li>` élément. Répéteur interne et une autre affiche ensuite les enfants du nœud actuel dans une liste non triée imbriquée.

Figure 4 montre la sortie de rendu du balisage ci-dessus avec la structure de plan de site que nous avons créé à l’étape 2. Le Repeater restitue le balisage de la liste non triée vanille ; les règles de feuille de style en cascade définies dans `Styles.css` sont responsables de la mise en page esthétiques. Pour obtenir une description plus détaillée du fonctionne de la balise ci-dessus, reportez-vous à la [Pages maîtres et Navigation du Site](https://asp.net/learn/data-access/tutorial-03-cs.aspx) didacticiel.


[![L’Interface utilisateur de navigation est restitué à l’aide d’imbriqué non triée de listes](creating-user-accounts-cs/_static/image11.png)](creating-user-accounts-cs/_static/image10.png)

**Figure 4**: l’Interface utilisateur de navigation est restitué à l’aide d’imbriqué non triée de listes ([cliquez pour afficher l’image en taille réelle](creating-user-accounts-cs/_static/image12.png))


### <a name="adding-breadcrumb-navigation"></a>Ajout d’arborescence de Navigation

En plus de la liste de liens dans la colonne de gauche, nous allons ont également chaque affichage de page un [fil d’Ariane](http://en.wikipedia.org/wiki/Breadcrumb_%28navigation%29). Un fil d’Ariane est un élément d’interface utilisateur de navigation qui rapidement indique aux utilisateurs de leur position actuelle dans la hiérarchie du site. Le contrôle SiteMapPath utilise l’infrastructure de plan de Site pour déterminer l’emplacement de la page actuelle dans le plan du site, puis affiche un fil d’Ariane en fonction de ces informations.

Plus précisément, ajouter un `<span>` élément à l’en-tête de la page maître `<div>` élément et définir le nouveau `<span>` l’élément `class` attribut « fil d’Ariane ». (La `Styles.css` classe contient une règle pour une classe « fil d’Ariane ».) Ensuite, ajoutez un SiteMapPath vers ce nouveau `<span>` élément.

[!code-aspx[Main](creating-user-accounts-cs/samples/sample4.aspx)]

La figure 5 illustre la sortie de la SiteMapPath lors de la visite `~/Membership/CreatingUserAccounts.aspx`.


[![La barre de navigation affiche la Page actuelle et de mappent ses ancêtres du site](creating-user-accounts-cs/_static/image14.png)](creating-user-accounts-cs/_static/image13.png)

**Figure 5**: la barre de navigation affiche la Page actuelle et ses ancêtres dans le plan de Site ([cliquez pour afficher l’image en taille réelle](creating-user-accounts-cs/_static/image15.png))


## <a name="step-4-removing-the-custom-principal-and-identity-logic"></a>Étape 4 : Suppression de l’objet Principal personnalisé et la logique d’identité

Dans le *<a id="_msoanchor_7"> </a> [Configuration de l’authentification de formulaires et des sujets avancés](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md)* didacticiel, nous avons vu comment associer les objets principal et identity personnalisés à l’utilisateur authentifié. Nous y sommes parvenus en créant un gestionnaire d’événements dans `Global.asax` pour l’application `PostAuthenticateRequest` événement, qui se déclenche après la `FormsAuthenticationModule` a authentifié l’utilisateur. Dans ce gestionnaire d’événements, nous avons remplacé le `GenericPrincipal` et `FormsIdentity` objets ajoutés par le `FormsAuthenticationModule` avec la `CustomPrincipal` et `CustomIdentity` objets nous créé dans ce didacticiel.

Tandis que les objets principal et identity personnalisés sont utiles dans certains scénarios, dans la plupart des cas le `GenericPrincipal` et `FormsIdentity` objets sont suffisantes. Par conséquent, je pense qu’il serait utile de revenir au comportement par défaut. Apporter cette modification en supprimant ou en commentant la `PostAuthenticateRequest` Gestionnaire d’événements ou en supprimant le `Global.asax` fichier entièrement.

## <a name="step-5-programmatically-creating-a-new-user"></a>Étape 5 : Création d’un utilisateur par programmation

Pour créer un nouveau compte d’utilisateur grâce à l’utilisation de framework de l’appartenance du `Membership` la classe [ `CreateUser` méthode](https://msdn.microsoft.com/library/system.web.security.membership.createuser.aspx). Cette méthode a d’entrée de paramètres pour le nom d’utilisateur, mot de passe et autres champs relatifs à l’utilisateur. Lors de l’appel, elle délègue la création du nouveau compte d’utilisateur pour le fournisseur d’appartenances configuré, puis renvoie un [ `MembershipUser` objet](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx) représentant le compte d’utilisateur récemment créée.

Le `CreateUser` méthode a quatre surcharges, chacune acceptant un nombre différent de paramètres d’entrée :

- [`CreateUser(username, password)`](https://msdn.microsoft.com/library/d8t4h2es.aspx)
- [`CreateUser(username, password, email)`](https://msdn.microsoft.com/library/t8yy6w3h.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, MembershipCreateStatus)`](https://msdn.microsoft.com/library/82xx2e62.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, providerUserKey, MembershipCreateStatus)`](https://msdn.microsoft.com/library/ms152012.aspx)

Ces quatre surcharges diffèrent sur la quantité d’informations collectées. La première surcharge, par exemple, nécessite simplement le nom d’utilisateur et le mot de passe du nouveau compte d’utilisateur, tandis que le second nécessite également l’adresse de messagerie de l’utilisateur.

Ces surcharges existent, car les informations nécessaires pour créer un nouveau compte d’utilisateur dépendent des paramètres de configuration du fournisseur d’appartenances. Dans le *<a id="_msoanchor_8"> </a> [création du schéma d’appartenance dans SQL Server](creating-the-membership-schema-in-sql-server-cs.md)* didacticiel, nous avons examiné les paramètres de configuration en spécifiant l’appartenance au fournisseur dans `Web.config`. Tableau 2 inclus une liste complète des paramètres de configuration.

Une telle l’appartenance au fournisseur paramètre de configuration qui a une incidence sur ce que `CreateUser` surcharges peuvent être utilisées est la `requiresQuestionAndAnswer` paramètre. Si `requiresQuestionAndAnswer` a la valeur `true` (la valeur par défaut), puis lors de la création d’un nouveau compte d’utilisateur, nous devons spécifier une question de sécurité et une réponse. Ces informations sont utilisées ultérieurement si l’utilisateur doit réinitialiser ou changer leur mot de passe. En particulier, à ce moment-là qu’ils sont affichés à la question de sécurité, et ils doivent entrer la réponse correcte pour réinitialiser ou modifier leur mot de passe. Par conséquent, si le `requiresQuestionAndAnswer` a la valeur `true` puis appelez une des deux premières `CreateUser` surcharges conduit à une exception, car la question de sécurité et la réponse sont manquants. Étant donné que notre application est actuellement configurée pour exiger une question de sécurité et une réponse, nous devrons utiliser une des deux surcharges ce dernier lors de la création de l’utilisateur par programmation.

Pour illustrer l’utilisation de la `CreateUser` (méthode), nous allons créer une interface utilisateur où nous invite l’utilisateur à leur nom, mot de passe, e-mail et une réponse à une question de sécurité prédéfinis. Ouvrez le `CreatingUserAccounts.aspx` page dans le `Membership` dossier et ajoutez les contrôles Web suivants pour le contrôle de contenu :

- Une zone de texte nommé `Username`
- Une zone de texte nommée `Password`, dont `TextMode` propriété est définie sur `Password`
- Une zone de texte nommé `Email`
- Une étiquette nommée `SecurityQuestion` avec son `Text` propriété nettoyés
- Une zone de texte nommé `SecurityAnswer`
- Un bouton nommé `CreateAccountButton` dont la propriété Text est définie à « Créer le compte d’utilisateur »
- Un contrôle Label nommé `CreateAccountResults` avec son `Text` propriété nettoyés

À ce stade votre écran doit ressembler à l’écran illustré à la Figure 6.


[![Ajouter les différents contrôles Web à la Page CreatingUserAccounts.aspx](creating-user-accounts-cs/_static/image17.png)](creating-user-accounts-cs/_static/image16.png)

**Figure 6**: ajouter les contrôles Web différents à la `CreatingUserAccounts.aspx` Page ([cliquez pour afficher l’image en taille réelle](creating-user-accounts-cs/_static/image18.png))


Le `SecurityQuestion` étiquette et `SecurityAnswer` zone de texte sont destinées à afficher une question de sécurité prédéfinis et de collecter la réponse de l’utilisateur. Notez que la question de sécurité et la réponse sont stockés sur une base utilisateur par l’utilisateur, il est donc possible de permettre à chaque utilisateur définir leur propre question de sécurité. Toutefois, pour cet exemple, j’ai décidé d’utiliser une question de sécurité universel, à savoir : « Quel est votre couleur préférée ? »

Pour implémenter cette question de sécurité prédéfinis, ajoutez une constante à la classe de code-behind de la page nommée `passwordQuestion`, affectez-la à la question de sécurité. Ensuite, dans le `Page_Load` Gestionnaire d’événements, affectez cette constante pour le `SecurityQuestion` l’étiquette `Text` propriété :

[!code-csharp[Main](creating-user-accounts-cs/samples/sample5.cs)]

Ensuite, créez un gestionnaire d’événements pour le `CreateAccountButton`de `Click` événement et ajoutez le code suivant :

[!code-csharp[Main](creating-user-accounts-cs/samples/sample6.cs)]

Le `Click` Gestionnaire d’événements démarre en définissant une variable nommée `createStatus` de type [ `MembershipCreateStatus` ](https://msdn.microsoft.com/library/system.web.security.membershipcreatestatus.aspx). `MembershipCreateStatus` est une énumération qui indique l’état de la `CreateUser` opération. Par exemple, si le compte d’utilisateur est créé avec succès, résultant `MembershipCreateStatus` instance définit une valeur de `Success`; sur l’autre main, si l’opération échoue, car il existe déjà un utilisateur avec le même nom d’utilisateur, elle sera définie sur la valeur `DuplicateUserName`. Dans le `CreateUser` , nous utilisons la surcharge, nous devons transmettre un `MembershipCreateStatus` instance dans la méthode comme un `out` paramètre. Ce paramètre est défini sur la valeur appropriée dans le `CreateUser` (méthode) et nous pouvons examiner sa valeur après l’appel de méthode pour déterminer si le compte d’utilisateur a été créé avec succès.

Après avoir appelé `CreateUser`, en passant dans `createStatus`, un `switch` instruction est utilisée pour générer un message approprié en fonction de la valeur affectée à `createStatus`. Figures 7 illustre la sortie lorsqu’un nouvel utilisateur a été correctement créé. Les figures 8 et 9 illustrent la sortie lorsque le compte d’utilisateur n’est pas créé. Dans la Figure 8, le visiteur entré un mot de passe de cinq lettres ne répond pas aux exigences de force de mot de passe en toutes lettres dans les paramètres de configuration du fournisseur d’appartenances. Dans la Figure 9, le visiteur tente de créer un compte d’utilisateur avec un nom d’utilisateur existant (celui créé dans la Figure 7).


[![Un nouveau compte d’utilisateur est créé avec succès](creating-user-accounts-cs/_static/image20.png)](creating-user-accounts-cs/_static/image19.png)

**Figure 7**: un nouveau compte d’utilisateur est créé avec succès ([cliquez pour afficher l’image en taille réelle](creating-user-accounts-cs/_static/image21.png))


[![Le compte d’utilisateur n’est pas créé, car le mot de passe fourni est trop faible](creating-user-accounts-cs/_static/image23.png)](creating-user-accounts-cs/_static/image22.png)

**Figure 8**: le compte d’utilisateur n’est pas créé, car le mot de passe fourni est trop faible ([cliquez pour afficher l’image en taille réelle](creating-user-accounts-cs/_static/image24.png))


[![Le compte utilisateur n’est que pas créé, car le nom d’utilisateur est déjà en cours d’utilisation](creating-user-accounts-cs/_static/image26.png)](creating-user-accounts-cs/_static/image25.png)

**Figure 9**: le compte d’utilisateur est pas créé, car le nom d’utilisateur est déjà en cours d’utilisation ([cliquez pour afficher l’image en taille réelle](creating-user-accounts-cs/_static/image27.png))


> [!NOTE]
> Vous vous demandez peut-être comment déterminer la réussite ou l’échec lorsque l’un des deux premiers `CreateUser` surcharges de méthode, ni de qui a un paramètre de type `MembershipCreateStatus`. Ces deux premières surcharges lever un [ `MembershipCreateUserException` exception](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.aspx) face à une défaillance, ce qui inclut un [ `StatusCode` propriété](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.statuscode.aspx) de type `MembershipCreateStatus`.


Après avoir créé quelques comptes d’utilisateur, vérifiez que les comptes ont été créés en répertoriant le contenu de la `aspnet_Users` et `aspnet_Membership` tables dans le `SecurityTutorials.mdf` base de données. Comme le montre la Figure 10, j’ai ajouté deux utilisateurs via la `CreatingUserAccounts.aspx` page : Tito et Bruce.


[![Il existe deux utilisateurs dans le Store d’utilisateur d’appartenance : Tito et Bruce](creating-user-accounts-cs/_static/image29.png)](creating-user-accounts-cs/_static/image28.png)

**Figure 10**: il existe deux utilisateurs dans le Store d’utilisateur d’appartenance : Tito et Bruce ([cliquez pour afficher l’image en taille réelle](creating-user-accounts-cs/_static/image30.png))


Bien que le magasin d’utilisateurs d’appartenance inclut désormais les informations de compte de Bruce et de Tito, nous devons encore implémenter une fonctionnalité qui permet de Bruce ou Tito ouvrir une session le site. Actuellement, `Login.aspx` valide les informations d’identification de l’utilisateur par rapport à un jeu codé en dur de paires de nom d’utilisateur/mot de passe : c’est le cas *pas* valider les informations d’identification fournies par rapport à l’infrastructure de l’appartenance. Pour voir maintenant les nouveaux comptes d’utilisateur dans le `aspnet_Users` et `aspnet_Membership` tables doivent suffire. Dans le didacticiel suivant,  *<a id="_msoanchor_9"> </a> [validation utilisateur informations d’identification par rapport à l’appartenance utilisateur Store](validating-user-credentials-against-the-membership-user-store-cs.md)*, nous mettrons à jour la page de connexion pour valider par rapport au magasin d’appartenance.

> [!NOTE]
> Si vous ne voyez pas tous les utilisateurs dans votre `SecurityTutorials.mdf` base de données, il est possible que votre application web utilise le fournisseur d’appartenances par défaut, `AspNetSqlMembershipProvider`, qui utilise le `ASPNETDB.mdf` base de données comme magasin de l’utilisateur. Pour déterminer si c’est le problème, cliquez sur le bouton Actualiser dans l’Explorateur de solutions. Si une base de données nommée `ASPNETDB.mdf` a été ajouté à la `App_Data` dossier, c’est le problème. Retourner à l’étape 4 de la *<a id="_msoanchor_10"> </a> [création du schéma d’appartenance dans SQL Server](creating-the-membership-schema-in-sql-server-cs.md)* didacticiel pour obtenir des instructions sur la façon de configurer correctement le fournisseur d’appartenances.


Dans la plupart des créer des scénarios de compte d’utilisateur, le visiteur est présenté avec une interface à entrer son nom d’utilisateur, mot de passe, messagerie et autres informations essentielles, à quel point un nouveau compte est créé. Dans cette étape nous avons examiné la création manuelle d’une telle interface et puis que vous avez vu comment utiliser le `Membership.CreateUser` méthode pour ajouter par programmation le nouveau compte d’utilisateur en fonction des entrées de l’utilisateur. Notre code, cependant, venez de créer le nouveau compte d’utilisateur. Il n’a pas effectué les actions, telles que la journalisation de l’utilisateur vers le site sous le compte d’utilisateur récemment créée, ou en envoyant un e-mail de confirmation à l’utilisateur de suivi. Ces étapes supplémentaires nécessiterait un code supplémentaire dans le bouton `Click` Gestionnaire d’événements.

ASP.NET est livré avec le contrôle CreateUserWizard, qui est conçu pour gérer le processus de création de compte utilisateur, à partir d’une interface utilisateur pour la création de nouveaux comptes d’utilisateur, à la création du compte dans le cadre de l’appartenance et l’exécution postérieur au compte de rendu tâches de création, telles qu’envoyer un e-mail de confirmation et connectez l’utilisateur nouvellement créé au site. En utilisant le contrôle CreateUserWizard, il vous suffit de faire glisser le contrôle CreateUserWizard à partir de la boîte à outils vers une page, puis en définissant certaines propriétés. Dans la plupart des cas, vous ne devrez écrire une seule ligne de code. Nous allons Explorer ce contrôle judicieuse en détail à l’étape 6.

Si de nouveaux comptes d’utilisateur sont uniquement créés via une page web de créer un compte standard, il est peu probable que vous devrez jamais écrire du code qui utilise le `CreateUser` (méthode), comme le contrôle CreateUserWizard probablement répondra à vos besoins. Toutefois, le `CreateUser` méthode est pratique dans les scénarios où vous avez besoin d’une expérience utilisateur de créer un compte hautement personnalisée ou que vous deviez créer par programmation des nouveaux comptes d’utilisateur via une autre interface. Par exemple, vous pouvez avoir une page qui permet à un utilisateur de charger un fichier XML qui contient des informations utilisateur à partir d’une autre application. La page peut analyse le contenu du fichier XML téléchargé de fichiers et créer un nouveau compte pour chaque utilisateur représenté dans le code XML en appelant le `CreateUser` (méthode).

## <a name="step-6-creating-a-new-user-with-the-createuserwizard-control"></a>Étape 6 : Création d’un utilisateur avec le contrôle CreateUserWizard

ASP.NET est livré avec un nombre de contrôles Web de connexion. Ces contrôles vous aider dans nombreux scénarios utilisateur courants liés au compte et un compte de connexion. Le [contrôle CreateUserWizard](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx) est ce genre de contrôle qui est conçu pour présenter une interface utilisateur pour l’ajout d’un nouveau compte d’utilisateur à l’infrastructure de l’appartenance.

Comme de nombreux autres contrôles Web associées à la connexion, CreateUserWizard peut être utilisé sans écrire une seule ligne de code. Il fournit une interface utilisateur basée sur le fournisseur d’appartenances paramètres de configuration et appelle en interne pour l’intuitivement la `Membership` la classe `CreateUser` méthode une fois que l’utilisateur entre les informations nécessaires et qu’il clique sur le bouton « Create User ». Le contrôle CreateUserWizard est extrêmement personnalisable. Il existe une multitude d’événements qui se déclenchent au cours des différentes étapes du processus de création de compte. Nous pouvons créer des gestionnaires d’événements, en fonction des besoins, pour injecter une logique personnalisée dans le flux de travail de création de compte. En outre, l’apparence de CreateUserWizard est très flexible. Il existe un certain nombre de propriétés qui définissent l’apparence de l’interface par défaut ; Si nécessaire, le contrôle peut être converti en un modèle ou d’inscription des utilisateurs supplémentaires « étapes » peut-être être ajoutée.

Commençons par examiner à l’aide d’interface par défaut et le comportement du contrôle CreateUserWizard. Nous allons Explorer puis comment personnaliser l’apparence via les propriétés et les événements du contrôle.

### <a name="examining-the-createuserwizards-default-interface-and-behavior"></a>Examen d’Interface par défaut et le comportement de CreateUserWizard

Retour à la `CreatingUserAccounts.aspx` page dans le `Membership` dossier, basculez vers le mode Design ou fractionné et puis ajoutez un contrôle CreateUserWizard vers le haut de la page. Le contrôle CreateUserWizard est classé sous la section contrôles de connexion de la boîte à outils. Après avoir ajouté le contrôle, définissez son `ID` propriété `RegisterUser`. Comme la capture d’écran dans la Figure 11 montre, CreateUserWizard restitue une interface avec les zones de texte pour le nouvel utilisateur nom d’utilisateur, mot de passe, l’adresse de messagerie et question de sécurité et la réponse.


[![Les convertisseurs de contrôle CreateUserWizard générique créer l’Interface utilisateur](creating-user-accounts-cs/_static/image32.png)](creating-user-accounts-cs/_static/image31.png)

**Figure 11**: le contrôle CreateUserWizard restitue une Interface utilisateur de créer générique ([cliquez pour afficher l’image en taille réelle](creating-user-accounts-cs/_static/image33.png))


Prenons un moment pour comparer l’interface utilisateur par défaut généré par le contrôle CreateUserWizard avec l’interface que nous avons créé à l’étape 5. Pour commencer, le contrôle CreateUserWizard permet le visiteur spécifier la question de sécurité et de la réponse, alors que notre interface créés manuellement utilisé une question de sécurité prédéfinis. L’interface du contrôle CreateUserWizard inclut également des contrôles de validation, tandis que nous devions encore implémenter la validation les champs de notre interface de formulaire. Et l’interface de contrôle CreateUserWizard inclut une zone de texte « Confirmer le mot de passe » (avec un CompareValidator pour vous assurer que le texte saisi le « Password » et « Comparer le mot de passe » de zones de texte sont égales).

Ce qui est intéressant, c’est que le contrôle CreateUserWizard consulte les paramètres de configuration du fournisseur d’appartenances lors du rendu de son interface utilisateur. Par exemple, les zones de texte question et la réponse du sécurité sont affichées uniquement si `requiresQuestionAndAnswer` est définie sur True. De même, CreateUserWizard ajoute automatiquement un RegularExpressionValidator (contrôle) pour vous assurer que les exigences de force de mot de passe sont remplies et définit sa `ErrorMessage` et `ValidationExpression` propriétés basées sur le `minRequiredPasswordLength`, `minRequiredNonalphanumericCharacters`et `passwordStrengthRegularExpression` paramètres de configuration.

Le contrôle CreateUserWizard, comme son nom l’indique, est dérivé le [contrôle de l’Assistant](https://msdn.microsoft.com/library/s2etd1ek.aspx). Contrôles de l’Assistant sont conçus pour fournir une interface permettant d’effectuer des tâches de plusieurs étapes. Un contrôle de l’Assistant peut avoir un nombre arbitraire de `WizardSteps`, chacun d’eux est un modèle qui définit le code HTML et les contrôles Web pour cette étape. Le contrôle de l’Assistant affiche initialement le premier `WizardStep`, ainsi que les contrôles de navigation qui permettent à l’utilisateur pour passer d’une étape à la suivante, ou pour revenir aux étapes précédentes.

Comme le montre le balisage déclaratif dans la Figure 11, l’interface par défaut du contrôle CreateUserWizard inclut deux `WizardSteps:`

- [`CreateUserWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizardstep.aspx) : restitue l’interface pour collecter les informations nécessaires pour créer le nouveau compte d’utilisateur. Il s’agit de l’étape illustrée à la Figure 11.
- [`CompleteWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.completewizardstep.aspx) – affiche un message indiquant que le compte a été créé correctement.

Apparence et le comportement du CreateUserWizard sont modifiables en convertissant une de ces étapes pour les modèles, ou en ajoutant vos propres `WizardSteps`. Nous allons examiner l’ajout d’un `WizardStep` à l’interface d’inscription dans le *stockant des informations utilisateur supplémentaires* didacticiel.

Nous allons voir le contrôle CreateUserWizard en action. Visitez le `CreatingUserAccounts.aspx` page via un navigateur. Commencez par entrer des valeurs non valides dans l’interface de CreateUserWizard. Essayez d’entrer un mot de passe qui ne sont pas conformes aux exigences de force de mot de passe ou textbox en laissant le « nom d’utilisateur » vide. CreateUserWizard affichera un message d’erreur approprié. La figure 12 illustre la sortie lorsque vous tentez de créer un utilisateur avec un mot de passe fort insuffisamment.


[![CreateUserWizard injecte automatiquement les contrôles de Validation](creating-user-accounts-cs/_static/image35.png)](creating-user-accounts-cs/_static/image34.png)

**Figure 12**: le contrôle CreateUserWizard automatiquement injecte les contrôles de Validation ([cliquez pour afficher l’image en taille réelle](creating-user-accounts-cs/_static/image36.png))


Ensuite, entrez les valeurs appropriées dans le contrôle CreateUserWizard et cliquez sur le bouton « Create User ». En supposant que les champs obligatoires ont été entrées et les force du mot de passe est suffisant, CreateUserWizard créer un nouveau compte d’utilisateur via l’infrastructure Membership et afficher le `CompleteWizardStep`de l’interface (voir la Figure 13). Dans les coulisses, CreateUserWizard appelle le `Membership.CreateUser` méthode, comme nous l’avons fait à l’étape 5.


[![Un nouveau compte d’utilisateur a été correctement créée](creating-user-accounts-cs/_static/image38.png)](creating-user-accounts-cs/_static/image37.png)

**Figure 13**: un nouveau compte d’utilisateur a été correctement créée ([cliquez pour afficher l’image en taille réelle](creating-user-accounts-cs/_static/image39.png))


> [!NOTE]
> Comme le montre la Figure 13, le `CompleteWizardStep`d’interface comprend un bouton Continuer. Toutefois, à ce stade cliquant dessus juste effectue une publication (postback), en laissant le visiteur sur la même page. Dans la section « Personnalisation de la CreateUserWizard apparence et comportement via ses propriétés » nous allons examiner comment vous pouvez avoir ce bouton Envoyer l’utilisateur à `Default.aspx` (ou une autre page).


Après avoir créé un nouveau compte d’utilisateur, revenez à Visual Studio et examiner les `aspnet_Users` et `aspnet_Membership` tables, comme nous l’avons fait dans la Figure 10 pour vérifier que le compte a été créé avec succès.

### <a name="customizing-the-createuserwizards-behavior-and-appearance-through-its-properties"></a>Personnaliser le CreateUserWizard comportement et apparence grâce à ses propriétés

CreateUserWizard peut être personnalisé de plusieurs façons, via les propriétés, `WizardSteps`et les gestionnaires d’événements. Dans cette section, nous allons examiner comment personnaliser l’apparence du contrôle grâce à ses propriétés ; la section suivante examine d’étendre le comportement du contrôle via les gestionnaires d’événements.

Pratiquement tout le texte affiché dans l’interface utilisateur du contrôle CreateUserWizard par défaut pouvant être personnalisé via sa multitude de propriétés. Par exemple, les étiquettes « Nom d’utilisateur », « Password », « Confirmer le mot de passe », « Courrier électronique », « Question de sécurité » et « Sécurité Answer » affichées à gauche des zones de texte peuvent être personnalisés par le [ `UserNameLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.usernamelabeltext.aspx), [ `PasswordLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.passwordlabeltext.aspx), [ `ConfirmPasswordLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.confirmpasswordlabeltext.aspx), [ `EmailLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.emaillabeltext.aspx), [ `QuestionLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.questionlabeltext.aspx), et [ `AnswerLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.answerlabeltext.aspx)propriétés, respectivement. De même, il existe des propriétés pour spécifier le texte pour les boutons « Create User » et « Continuer » dans le `CreateUserWizardStep` et `CompleteWizardStep`, ainsi que si ces boutons sont rendus sous forme de boutons, de type LinkButton ou ImageButtons.

Les couleurs, des bordures, des polices et autres éléments visuels sont configurables via un hôte de propriétés de style. Le contrôle CreateUserWizard lui-même a les propriétés de style de contrôle Web courantes : `BackColor`, `BorderStyle`, `CssClass`, `Font`, et ainsi de suite, et il existe un nombre de propriétés de style pour définir l’apparence des sections particulières de la Interface de CreateUserWizard. Le [ `TextBoxStyle` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.textboxstyle.aspx), par exemple, définit les styles pour les zones de texte dans le `CreateUserWizardStep`, tandis que le [ `TitleTextStyle` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.titletextstyle.aspx) définit le style du titre (« s’inscrire pour votre nouveau Compte »).

Outre les propriétés relatives à l’apparence, il existe un nombre de propriétés qui affectent le comportement du contrôle CreateUserWizard. Le [ `DisplayCancelButton` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.displaycancelbutton.aspx), si la valeur True, affiche un bouton Annuler en regard du bouton « Create User » (la valeur par défaut est False). Si vous affichez le bouton Annuler, veillez à définir également la [ `CancelDestinationPageUrl` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx), qui spécifie la page de l’utilisateur est envoyé à après avoir cliqué sur Annuler. Comme indiqué dans la section précédente, le bouton Continuer situé dans le `CompleteWizardStep`d’interface entraîne une publication, mais laisse le visiteur sur la même page. Pour envoyer le visiteur vers une autre page après avoir cliqué sur le bouton Continuer, spécifiez simplement l’URL dans le [ `ContinueDestinationPageUrl` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx).

Nous allons mettre à jour le `RegisterUser` contrôle CreateUserWizard pour afficher un bouton Annuler et envoyer le visiteur à `Default.aspx` lorsque vous cliquez sur les boutons Annuler ou continuer. Pour ce faire, affectez la `DisplayCancelButton` propriété sur True et à la fois le `CancelDestinationPageUrl` et `ContinueDestinationPageUrl` propriétés à « ~ / Default.aspx ». Figure 14 illustre la mise à jour CreateUserWizard lorsqu’ils sont affichés via un navigateur.


[![Le CreateUserWizardStep inclut un bouton Annuler](creating-user-accounts-cs/_static/image41.png)](creating-user-accounts-cs/_static/image40.png)

**Figure 14**: le `CreateUserWizardStep` inclut un bouton Annuler ([cliquez pour afficher l’image en taille réelle](creating-user-accounts-cs/_static/image42.png))


Lorsqu’un visiteur entre un nom d’utilisateur, mot de passe, l’adresse de messagerie et questions et réponses secrètes et clique sur « Créer un utilisateur », un nouveau compte d’utilisateur est créé et le visiteur est connecté en tant que cet utilisateur nouvellement créé. En supposant que la personne visitant la page crée un nouveau compte pour eux-mêmes, cela est probablement le comportement souhaité. Toutefois, vous souhaiterez permettent aux administrateurs d’ajouter de nouveaux comptes d’utilisateur. Ce faisant, le compte d’utilisateur serait créé, mais l’administrateur est toujours connecté en tant qu’administrateur (et non comme le compte qui vient d’être créé). Ce comportement peut être modifié via la valeur booléenne [ `LoginCreatedUser` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx).

Comptes d’utilisateur dans le cadre de l’appartenance contiennent un indicateur approuvé ; les utilisateurs qui ne sont pas approuvées ne peuvent pas se connecter au site. Par défaut, un compte nouvellement créé est marqué comme approuvé, permettant à l’utilisateur à se connecter au site immédiatement. Toutefois, il est possible, pour que les nouveaux comptes d’utilisateurs marqués comme non approuvées. Vous pouvez choisir un administrateur d’approuver manuellement les nouveaux utilisateurs avant qu’ils peuvent se connecter ; ou peut-être que vous souhaitez vérifier que l’adresse e-mail entrée lors de votre inscription est valide avant d’autoriser un utilisateur de se connecter. Quel que soit le cas, vous pouvez avoir le compte d’utilisateur nouvellement créé marqué comme non approuvées en définissant le contrôle CreateUserWizard [ `DisableCreatedUser` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx) sur True (la valeur par défaut est False).

Incluent d’autres propriétés relatives au comportement de note `AutoGeneratePassword` et `MailDefinition`. Si le [ `AutoGeneratePassword` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.autogeneratepassword.aspx) est définie sur True, le `CreateUserWizardStep` n’affiche pas les zones de texte « Password » et « Confirmer le mot de passe » ; au lieu de cela, mot de passe de l’utilisateur nouvellement créé est automatiquement généré à l’aide de la `Membership` la classe [ `GeneratePassword` méthode](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx). Le `GeneratePassword` méthode construit un mot de passe d’une longueur spécifiée et avec un nombre suffisant de caractères non alphanumériques satisfaire les exigences de force de mot de passe configuré.

Le [ `MailDefinition` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) est utile si vous souhaitez envoyer un e-mail à l’adresse e-mail spécifiée pendant le processus de création de compte. Le `MailDefinition` propriété inclut une série de sous-propriétés pour définir les informations sur le message électronique construit. Ces sous-propriétés incluent des options telles que `Subject`, `Priority`, `IsBodyHtml`, `From`, `CC`, et `BodyFileName`. Le [ `BodyFileName` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) pointe vers un fichier texte ou HTML qui contient le corps de l’e-mail. Le corps de la prend en charge les deux espaces réservés prédéfinis : `<%UserName%>` et `<%Password%>`. Ces espaces réservés, s’il est présent dans le `BodyFileName` de fichiers, est remplacé par le nom et mot de passe de l’utilisateur nouvellement créé.

> [!NOTE]
> Le `CreateUserWizard` du contrôle `MailDefinition` propriété spécifie uniquement des informations sur le message électronique qui est envoyé lorsqu’un nouveau compte est créé. Il n’inclut pas tous les détails sur la façon dont le message électronique est envoyé réellement (autrement dit, si un répertoire du serveur ou de la boîte aux lettres SMTP est utilisé, les informations d’authentification et ainsi de suite). Ces détails de bas niveau doivent être définis dans le `<system.net>` section `Web.config`. Pour plus d’informations sur ces paramètres de configuration et sur l’envoi de courrier électronique à partir d’ASP.NET 2.0 en général, reportez-vous à la [FAQ à SystemNetMail.com](http://www.systemnetmail.com/) et mon article, [envoi de courrier électronique dans ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).


### <a name="extending-the-createuserwizards-behavior-using-event-handlers"></a>Extension de comportement de CreateUserWizard à l’aide de gestionnaires d’événements

Le contrôle CreateUserWizard élève un nombre d’événements au cours de son flux de travail. Par exemple, une fois un visiteur entre son nom d’utilisateur, mot de passe et d’autres informations pertinentes et clique sur le bouton « Create User », le contrôle CreateUserWizard déclenche son [ `CreatingUser` événement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). S’il existe un problème pendant le processus de création, le [ `CreateUserError` événement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) est déclenché ; Toutefois, si l’utilisateur est créé avec succès, puis le [ `CreatedUser` événement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) est déclenché. Il existe des événements de contrôle CreateUserWizard supplémentaires qui soit déclenchés, mais ce sont trois celles plus pertinentes.

Dans certains scénarios, nous souhaiterons à exploiter le flux de travail CreateUserWizard, nous pouvons faire en créant un gestionnaire d’événements pour l’événement approprié. Pour illustrer cela, nous allons améliorer la `RegisterUser` contrôle CreateUserWizard à inclure une validation personnalisée sur le nom d’utilisateur et le mot de passe. En particulier, nous allons améliorer notre CreateUserWizard afin que les noms d’utilisateur ne peut pas contenir d’espaces de début ou de fin et le nom d’utilisateur ne peut pas apparaître n’importe où dans le mot de passe. En bref, nous souhaitons empêcher un utilisateur de créer un nom d’utilisateur comme « Scott », ou avoir une combinaison nom d’utilisateur/mot de passe tels que « Scott » et « Scott.1234 ».

Pour cela nous allons créer un gestionnaire d’événements pour le `CreatingUser` événement pour effectuer des vérifications de notre validation supplémentaire. Si les données fournies ne sont pas valides, nous avons besoin d’annuler le processus de création. Nous devons également ajouter un contrôle Web Label à la page pour afficher un message expliquant que le nom d’utilisateur ou mot de passe n’est pas valide. Commencez par ajouter un contrôle Label sous le contrôle CreateUserWizard, en définissant son `ID` propriété `InvalidUserNameOrPasswordMessage` et son `ForeColor` propriété `Red`. Effacez son `Text` propriété et définissez son `EnableViewState` et `Visible` propriétés sur False.

[!code-aspx[Main](creating-user-accounts-cs/samples/sample7.aspx)]

Ensuite, créez un gestionnaire d’événements pour le contrôle CreateUserWizard `CreatingUser` événement. Pour créer un gestionnaire d’événements, sélectionnez le contrôle dans le concepteur et passez à la fenêtre Propriétés. À partir de là, cliquez sur l’icône d’éclair et puis double-cliquez sur l’événement approprié pour créer le Gestionnaire d’événements.

Ajoutez le code suivant au gestionnaire d'événements `CreatingUser` :

[!code-csharp[Main](creating-user-accounts-cs/samples/sample8.cs)]

Notez que le nom d’utilisateur et le mot de passe entré dans le contrôle CreateUserWizard sont disponibles via son [ `UserName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx) et [ `Password` propriétés](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.password.aspx), respectivement. Nous utilisons ces propriétés dans le Gestionnaire d’événements ci-dessus pour déterminer si le nom d’utilisateur fourni contient des espaces de début ou de fin et si le nom d’utilisateur est trouvé dans le mot de passe. Si une de ces conditions est remplie, un message d’erreur s’affiche dans le `InvalidUserNameOrPasswordMessage` étiquette et le Gestionnaire d’événements `e.Cancel` propriété est définie sur `true`. Si `e.Cancel` a la valeur `true`, CreateUserWizard court-circuite son flux de travail, annulation efficacement le processus de création de compte utilisateur.

La figure 15 illustre la capture d’écran `CreatingUserAccounts.aspx` lorsque l’utilisateur entre un nom d’utilisateur avec les espaces à gauche.


[![Noms d’utilisateur avec tête ou des espaces de fin ne sont pas autorisées](creating-user-accounts-cs/_static/image44.png)](creating-user-accounts-cs/_static/image43.png)

**Figure 15**: noms d’utilisateur avec tête ou des espaces de fin ne sont pas autorisées ([cliquez pour afficher l’image en taille réelle](creating-user-accounts-cs/_static/image45.png))


> [!NOTE]
> Nous verrons un exemple d’utilisation du contrôle CreateUserWizard `CreatedUser` événement dans le *<a id="_msoanchor_11"> </a> [stockant des informations utilisateur supplémentaires](storing-additional-user-information-cs.md)* didacticiel.


## <a name="summary"></a>Récapitulatif

Le `Membership` la classe `CreateUser` méthode crée un nouveau compte d’utilisateur dans le cadre de l’appartenance. Il le fait en déléguant l’appel au fournisseur d’appartenances configuré. Dans le cas de la `SqlMembershipProvider`, le `CreateUser` méthode ajoute un enregistrement à la `aspnet_Users` et `aspnet_Membership` tables de base de données.

Bien que les nouveaux comptes d’utilisateur peuvent être créés par programme (comme nous l’avons vu à l’étape 5), une approche plus rapide et plus facile consiste à utiliser le contrôle CreateUserWizard. Ce contrôle affiche une interface utilisateur de plusieurs étapes pour la collecte des informations utilisateur et la création d’un nouvel utilisateur dans le cadre de l’appartenance. Dans les coulisses, ce contrôle utilise le même `Membership.CreateUser` examinées dans l’étape 5, mais le contrôle de la méthode crée l’interface utilisateur, les contrôles de validation et répond aux erreurs de création de compte utilisateur sans devoir écrire la moindre ligne de code.

À ce stade, nous avons la fonctionnalité en place pour créer de nouveaux comptes d’utilisateur. Toutefois, la page de connexion toujours est la validation par rapport à ces informations d’identification codées en dur, que nous avons spécifié dans le deuxième didacticiel. Dans le <a id="_msoanchor_12"> </a> [didacticiel suivant](validating-user-credentials-against-the-membership-user-store-cs.md) nous mettrons à jour `Login.aspx` pour valider l’utilisateur de fourni des informations d’identification par rapport à l’infrastructure de l’appartenance.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [`CreateUser` Documentation technique](https://msdn.microsoft.com/library/system.web.security.membershipprovider.createuser.aspx)
- [Vue d’ensemble du contrôle CreateUserWizard](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx)
- [Création d’un fournisseur de plan de Site basé sur le système de fichiers](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx)
- [Création d’une Interface utilisateur pas à pas avec le contrôle de l’Assistant 2.0 ASP.NET](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Examen d’ASP.NET 2.0 de Navigation de Site](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Pages maîtres et Navigation de Site](https://asp.net/learn/data-access/tutorial-03-vb.aspx)
- [Le fournisseur de plan de Site SQL que vous attendiez pour](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx)

### <a name="about-the-author"></a>À propos de l’auteur

Scott Mitchell, auteur de plusieurs livres sur ASP/ASP.NET et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est *[SAM animer vous-même ASP.NET 2.0 des dernières 24 heures](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott peut être atteint à [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou via son blog à [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements particuliers à...

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Entraîner un réviseur pour ce didacticiel a été Teresa Murphy. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Précédent](creating-the-membership-schema-in-sql-server-cs.md)
> [Suivant](validating-user-credentials-against-the-membership-user-store-cs.md)
