---
uid: web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-vb
title: Création d’une Interface pour sélectionner un compte d’utilisateur à partir de nombreux (VB) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons créer une interface utilisateur avec une grille paginée, filtrable. En particulier, l’interface utilisateur se compose d’une série de type LinkButton pour...
ms.author: riande
ms.date: 04/01/2008
ms.assetid: da53380c-a16b-41c7-a20d-24343c735c52
msc.legacyurl: /web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-vb
msc.type: authoredcontent
ms.openlocfilehash: bb30c5d3ce6e04f60d8192e8ed0404b89031b4b9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824531"
---
<a name="building-an-interface-to-select-one-user-account-from-many-vb"></a>Création d’une Interface pour sélectionner un compte d’utilisateur à partir de nombreux (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/VB.12.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial12_SelectUser_vb.pdf)

> Dans ce didacticiel, nous allons créer une interface utilisateur avec une grille paginée, filtrable. En particulier, l’interface utilisateur se compose d’une série de type LinkButton pour filtrer les résultats en fonction de la lettre initiale du nom d’utilisateur et un contrôle GridView pour afficher les utilisateurs correspondants. Nous allons commencer en répertoriant tous les comptes d’utilisateur dans un GridView. Ensuite, à l’étape 3, nous allons ajouter le filtre de type LinkButton. Étape 4 examine la pagination des résultats filtrés. L’interface construit dans les étapes 2 à 4 sera utilisé dans les didacticiels suivants pour effectuer des tâches administratives pour un compte d’utilisateur particulier.


## <a name="introduction"></a>Introduction

Dans le <a id="_msoanchor_1"> </a> [ *affectation de rôles aux utilisateurs* ](../roles/assigning-roles-to-users-vb.md) didacticiel, nous avons créé une interface rudimentaire pour un administrateur de sélectionner un utilisateur et de gérer ses rôles. Plus précisément, l’interface affiche l’administrateur une liste déroulante de tous les utilisateurs. Une telle interface est appropriée quand il existe mais utilisateur une douzaine de comptes, mais il est difficile à gérer pour les sites avec des centaines voire des milliers de comptes. Une grille paginée, filtrable est plus adapté interface utilisateur pour les sites Web avec des bases de l’utilisateur de grande taille.

Dans ce didacticiel, nous allons créer une interface utilisateur de ce type. En particulier, l’interface utilisateur se compose d’une série de type LinkButton pour filtrer les résultats en fonction de la lettre initiale du nom d’utilisateur et un contrôle GridView pour afficher les utilisateurs correspondants. Nous allons commencer en répertoriant tous les comptes d’utilisateur dans un GridView. Ensuite, à l’étape 3, nous allons ajouter le filtre de type LinkButton. Étape 4 examine la pagination des résultats filtrés. L’interface construit dans les étapes 2 à 4 sera utilisé dans les didacticiels suivants pour effectuer des tâches administratives pour un compte d’utilisateur particulier.

C’est parti !

## <a name="step-1-adding-new-aspnet-pages"></a>Étape 1 : Ajout de nouvelles Pages ASP.NET

Dans ce didacticiel et les deux suivants, nous étudierons diverses fonctions associées à l’administration et les fonctionnalités. Nous aurons besoin d’une série de pages ASP.NET pour implémenter les rubriques examinés tout au long de ces didacticiels. Nous allons créer ces pages et mettre à jour le plan du site.

Commencez par créer un nouveau dossier dans le projet nommé `Administration`. Ensuite, ajoutez deux nouvelles pages ASP.NET dans le dossier, en liant chaque page avec le `Site.master` page maître. Nommez les pages :

- `ManageUsers.aspx`
- `UserInformation.aspx`

Également ajouter deux pages pour le répertoire de racine du site Web : `ChangePassword.aspx` et `RecoverPassword.aspx`.

Ces quatre pages devez à ce stade, deux contrôles de contenu, une pour chaque ContentPlaceHolders de la page maître : `MainContent` et `LoginContent`.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample1.aspx)]

Nous voulons afficher le balisage par défaut de la page maître pour la `LoginContent` ContentPlaceHolder pour ces pages. Par conséquent, supprimez le balisage déclaratif pour la `Content2` contrôle de contenu. Après cela, les balises des pages doivent contenir qu’un seul contrôle de contenu.

Les pages ASP.NET dans le `Administration` dossier sont uniquement destinés aux utilisateurs administratifs. Nous avons ajouté un rôle d’administrateurs du système dans le <a id="_msoanchor_2"> </a> [ *création et gestion des rôles* ](../roles/creating-and-managing-roles-vb.md) didacticiel ; restreindre l’accès à ces deux pages à ce rôle. Pour ce faire, ajoutez un `Web.config` de fichiers à la `Administration` dossier et configurer ses `<authorization>` élément admettez que les utilisateurs du rôle Administrateurs et à refuser tous les autres.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample2.xml)]

À ce stade l’Explorateur de solutions de votre projet doit ressembler à l’écran illustré à la Figure 1.


[![Un fichier Web.config et les quatre nouvelles Pages ont été ajoutés au site Web](building-an-interface-to-select-one-user-account-from-many-vb/_static/image2.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image1.png)

**Figure 1**: quatre nouvelles Pages et une `Web.config` fichier ont été ajoutés au site Web ([cliquez pour afficher l’image en taille réelle](building-an-interface-to-select-one-user-account-from-many-vb/_static/image3.png))


Enfin, mettez à jour le plan du site (`Web.sitemap`) pour inclure une entrée à la `ManageUsers.aspx` page. Ajoutez le code XML suivant après le `<siteMapNode>` nous avons ajouté pour les didacticiels de rôles.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample3.xml)]

Avec le plan de site mis à jour, visitez le site via un navigateur. Comme le montre la Figure 2, le volet de navigation de gauche maintenant inclut des éléments pour les didacticiels d’Administration.


[![Le plan du Site comporte un nœud intitulé Administration des utilisateurs](building-an-interface-to-select-one-user-account-from-many-vb/_static/image5.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image4.png)

**Figure 2**: le plan de Site inclut un nœud intitulé l’Administration des utilisateurs ([cliquez pour afficher l’image en taille réelle](building-an-interface-to-select-one-user-account-from-many-vb/_static/image6.png))


## <a name="step-2-listing-all-user-accounts-in-a-gridview"></a>Étape 2 : Répertorier tous les comptes d’utilisateur dans un GridView

Notre objectif final pour ce didacticiel consiste à créer une grille paginée, filtrable par le biais duquel un administrateur peut sélectionner un compte d’utilisateur à gérer. Commençons par liste *tous les* les utilisateurs dans un GridView. Une fois cette opération terminée, nous allons ajouter le filtrage et de pagination interfaces et de fonctionnalités.

Ouvrir le `ManageUsers.aspx` page dans le `Administration` dossier et en ajouter un GridView, définissant son `ID` à `UserAccounts` dans un instant, nous allons écrire du code pour lier l’ensemble des comptes d’utilisateur pour le GridView à l’aide la `Membership` la classe de `GetAllUsers` (méthode). Comme indiqué dans les didacticiels précédents, le `GetAllUsers` méthode retourne un `MembershipUserCollection` objet, qui est une collection de `MembershipUser` objets. Chaque `MembershipUser` dans la collection inclut des propriétés telles que `UserName`, `Email`, `IsApproved`, et ainsi de suite.

Pour afficher les informations de compte d’utilisateur de votre choix dans le contrôle GridView, définissez le GridView `AutoGenerateColumns` False à la propriété et ajoutez BoundFields pour le `UserName`, `Email`, et `Comment` propriétés et CheckBoxFields pour le `IsApproved`, `IsLockedOut`, et `IsOnline` propriétés. Cette configuration peut être appliquée par un balisage déclaratif du contrôle ou par le biais de la boîte de dialogue champs. Figure 3 illustre une capture d’écran des champs de la boîte de dialogue une fois que la case à cocher des champs de génération automatique a été désactivée et les BoundFields CheckBoxFields ont été ajouté et configuré.


[![Ajouter trois BoundFields et trois CheckBoxFields au GridView](building-an-interface-to-select-one-user-account-from-many-vb/_static/image8.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image7.png)

**Figure 3**: ajouter un trois BoundFields et trois CheckBoxFields au GridView ([cliquez pour afficher l’image en taille réelle](building-an-interface-to-select-one-user-account-from-many-vb/_static/image9.png))


Après avoir configuré votre GridView, assurez-vous que son balisage déclaratif ressemble à ceci :

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample4.aspx)]

Ensuite, nous devons écrire du code qui lie les comptes d’utilisateur pour le contrôle GridView. Créez une méthode nommée `BindUserAccounts` pour effectuer cette tâche et appelez-la à partir du `Page_Load` Gestionnaire d’événements sur la première visite de page.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample5.vb)]

Prenez un moment pour tester la page via un navigateur. Comme le montre la Figure 4, le `UserAccounts` GridView répertorie le nom d’utilisateur, adresse de messagerie et autres informations pertinentes de compte pour tous les utilisateurs dans le système.


[![Les comptes d’utilisateur sont répertoriés dans le contrôle GridView](building-an-interface-to-select-one-user-account-from-many-vb/_static/image11.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image10.png)

**Figure 4**: les comptes d’utilisateur sont répertoriés dans le contrôle GridView ([cliquez pour afficher l’image en taille réelle](building-an-interface-to-select-one-user-account-from-many-vb/_static/image12.png))


## <a name="step-3-filtering-the-results-by-the-first-letter-of-the-username"></a>Étape 3 : Filtrage des résultats selon la première lettre du nom d’utilisateur

Actuellement le `UserAccounts` GridView affiche *tous les* des comptes d’utilisateur. Pour les sites Web avec des centaines voire des milliers de comptes d’utilisateur, il est impératif que l’utilisateur soit en mesure de réduire rapidement la taille les comptes affichés. Cela est possible en ajoutant le filtrage de type LinkButton à la page. Nous allons ajouter 27 LinkButton à la page : un intitulé tous en même temps qu’un LinkButton pour chaque lettre de l’alphabet. Si un visiteur clique sur le LinkButton tous, le contrôle GridView affiche tous les utilisateurs. S’il clique sur une lettre particulier, seuls les utilisateurs dont nom d’utilisateur commence par la lettre sélectionnée seront affichera.

Notre première tâche consiste à ajouter des contrôles LinkButton 27. Une option serait de créer le 27 LinkButton de façon déclarative, à la fois. Une approche plus souple consiste à utiliser un contrôle Repeater avec un `ItemTemplate` qui restitue le LinkButton et lie ensuite les options de filtrage pour le contrôle Repeater comme un `String` tableau.

Commencez par ajouter un contrôle Repeater à la page ci-dessus le `UserAccounts` GridView. Définir le Repeater `ID` propriété `FilteringUI` configurer des modèles de répéteur afin que son `ItemTemplate` restitue un LinkButton dont `Text` et `CommandName` propriétés sont liées à l’élément de tableau actuel. Comme nous l’avons vu dans la <a id="_msoanchor_3"> </a> [ *affectation de rôles aux utilisateurs* ](../roles/assigning-roles-to-users-vb.md) didacticiel, cela peut être accompli à l’aide de la `Container.DataItem` syntaxe de liaison de données. Utiliser le Repeater `SeparatorTemplate` pour afficher une ligne verticale entre chaque lien.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample6.aspx)]

Pour remplir ce Repeater avec les options de filtrage de votre choisis, créez une méthode nommée `BindFilteringUI`. Veillez à appeler cette méthode à partir de la `Page_Load` Gestionnaire d’événements sur le premier chargement de page.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample7.vb)]

Cette méthode spécifie les options de filtrage en tant qu’éléments dans le `String` tableau `filterOptions` pour chaque élément du tableau, le contrôle Repeater affichera un LinkButton avec son `Text` et `CommandName` affectées à la valeur du tableau de propriétés élément.

La figure 5 illustre le `ManageUsers.aspx` page lorsqu’ils sont affichés via un navigateur.


[![Le contrôle Repeater répertorie 27 LinkButton de filtrage](building-an-interface-to-select-one-user-account-from-many-vb/_static/image14.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image13.png)

**Figure 5**: le Repeater répertorie 27 filtrage LinkButton ([cliquez pour afficher l’image en taille réelle](building-an-interface-to-select-one-user-account-from-many-vb/_static/image15.png))


> [!NOTE]
> Noms d’utilisateur peut commencer par n’importe quel caractère, y compris des nombres et signes de ponctuation. Pour afficher ces comptes, l’administrateur devra utiliser l’option LinkButton tous les. Sinon, vous pouvez ajouter un LinkButton pour retourner tous les comptes d’utilisateur qui commencent par un nombre. Je laisse cela en guise d’exercice pour le lecteur.


Cliquez simplement sur le filtrage de type LinkButton entraîne une publication et déclenche le Repeater `ItemCommand` événement, mais il n’existe aucune modification dans la grille, car il nous faut encore à écrire du code pour filtrer les résultats. Le `Membership` classe inclut un [ `FindUsersByName` méthode](https://technet.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) qui retourne ces comptes d’utilisateur dont nom d’utilisateur correspond à un modèle de recherche spécifié. Nous pouvons utiliser cette méthode pour récupérer uniquement les comptes d’utilisateurs dont les noms d’utilisateur commencent par la lettre spécifiée par la `CommandName` du contrôle LinkButton filtré qui a été cliqué.

Commencez par la mise à jour le `ManageUser.aspx` de classe code-behind de la page afin qu’il inclue une propriété nommée `UsernameToMatch` cette propriété persiste la chaîne de filtre de nom d’utilisateur sur des publications :

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample8.vb)]

Le `UsernameToMatch` propriété stocke sa valeur, elle est assignée dans le `ViewState` collection à l’aide de la clé UsernameToMatch. Lorsque la valeur de cette propriété est en lecture, il vérifie si une valeur existe dans le `ViewState` collection ; sinon, elle retourne la valeur par défaut, une chaîne vide. Le `UsernameToMatch` propriété présente un modèle courant, à savoir rendre une valeur d’état d’affichage afin que toutes les modifications à la propriété sont conservées entre les postbacks. Pour plus d’informations sur ce modèle, consultez [état d’affichage ASP.NET compréhension](https://msdn.microsoftn-us/library/ms972976.aspx).

Ensuite, mettez à jour le `BindUserAccounts` méthode afin qu’au lieu de l’appel `Membership.GetAllUsers`, il appelle `Membership.FindUsersByName`, en passant la valeur de la `UsernameToMatch` propriété ajoutée avec le caractère générique SQL, %.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample9.vb)]

Pour afficher uniquement les utilisateurs dont nom d’utilisateur commence par la lettre A, affectez la `UsernameToMatch` à une propriété, puis appelez `BindUserAccounts` cela entraînerait un appel à `Membership.FindUsersByName("A%")`, qui retournera tous les utilisateurs dont nom d’utilisateur démarre avec A. de même, pour retourner *tous les* affecter des utilisateurs, une chaîne vide à la `UsernameToMatch` propriété afin que le `BindUserAccounts` méthode appelle `Membership.FindUsersByName("%")`, d'où retournant tous les comptes d’utilisateur.

Créer un gestionnaire d’événements pour le Repeater `ItemCommand` événement. Cet événement est déclenché chaque fois qu’un du filtre de type LinkButton est activé ; Il est passé de l’utilisateur a cliqué dessu LinkButton `CommandName` valeur via le `RepeaterCommandEventArgs` objet. Vous devez affecter la valeur appropriée pour le `UsernameToMatch` propriété, puis appelez le `BindUserAccounts` (méthode). Si le `CommandName` est All, affectez une chaîne vide à `UsernameToMatch` afin que tous les comptes d’utilisateur sont affichés. Sinon, assignez la `CommandName` valeur `UsernameToMatch`

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample10.vb)]

Avec ce code en place, testez la fonctionnalité de filtrage. Lorsque la page est visitée en premier, tous les comptes d’utilisateur sont affichés (voir la Figure 5). En cliquant sur le LinkButton A entraîne une publication (postback) et filtre les résultats, affichant uniquement les comptes d’utilisateur qui commencent par un.


[![Le filtrage de type LinkButton permet d’afficher les utilisateurs dont nom d’utilisateur commence par une lettre](building-an-interface-to-select-one-user-account-from-many-vb/_static/image17.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image16.png)

**Figure 6**: utiliser le filtrage de type LinkButton pour afficher ces utilisateurs dont nom d’utilisateur commence par une lettre certains ([cliquez pour afficher l’image en taille réelle](building-an-interface-to-select-one-user-account-from-many-vb/_static/image18.png))


## <a name="step-4-updating-the-gridview-to-use-paging"></a>Étape 4 : La mise à jour le contrôle GridView pour utiliser la pagination

Le contrôle GridView indiqué Figures 5 et 6 répertorie tous les enregistrements retournés à partir de la `FindUsersByName` (méthode). S’il existe des centaines voire des milliers de comptes d’utilisateur, cela peut entraîner de surcharge d’informations lorsque vous affichez tous les comptes (comme c’est le cas lorsque vous cliquez sur le LinkButton tous ou lors de la visite initialement la page). Pour aider à présenter les comptes d’utilisateur dans des segments plus gérables, nous allons configurer le contrôle GridView pour afficher les 10 comptes d’utilisateur à la fois.

Le contrôle GridView propose deux types de pagination :

- **La pagination par défaut** - facile à implémenter, mais inefficace. En bref, avec le contrôle GridView d’échange par défaut attend *tous les* des enregistrements à partir de sa source de données. Il affiche ensuite uniquement la page d’enregistrements appropriée.
- **La pagination personnalisée** -nécessite plus de travail à implémenter, mais est plus efficace que la pagination par défaut, car avec personnalisé la pagination des données source retourne uniquement l’ensemble précis d’enregistrements à afficher.

La différence de performances entre la valeur par défaut et la pagination personnalisée peut être très importante lors de la pagination par le biais des milliers d’enregistrements. Étant donné que nous créons cette interface en supposant qu’il est peut-être des centaines voire des milliers de comptes d’utilisateur, nous allons utiliser la pagination personnalisée.

> [!NOTE]
> Pour une discussion plus détaillée sur les différences entre la valeur par défaut et la pagination personnalisée, ainsi que les problèmes liés à l’implémentation de la pagination personnalisée, consultez [efficacement la pagination par le biais d’importants volumes de données](https://asp.net/learn/data-access/tutorial-25-vb.aspx). Pour une analyse de la différence de performances entre la valeur par défaut et la pagination personnalisée, consultez [la pagination personnalisée dans ASP.NET avec SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx).


Pour implémenter la pagination personnalisée, nous devons tout d’abord un mécanisme permettant de récupérer le sous-ensemble précis d’enregistrements affichés par le contrôle GridView. La bonne nouvelle est que le `Membership` la classe `FindUsersByName` méthode a une surcharge qui nous permet de spécifier l’index de page et la taille de page et retourne uniquement les comptes d’utilisateur qui se situent dans la plage d’enregistrements.

En particulier, cette surcharge possède la signature suivante : [ `FindUsersByName(usernameToMatch, pageIndex, pageSize, totalRecords)` ](https://msdn.microsoft.com/library/fa5st8b2.aspx).

Le *pageIndex* paramètre spécifie la page des comptes d’utilisateur à renvoyer ; *pageSize* indique le nombre d’enregistrements à afficher par page. Le *totalRecords* paramètre est un `ByRef` paramètre qui retourne le nombre de comptes utilisateur total dans le magasin de l’utilisateur.

> [!NOTE]
> Les données retournées par `FindUsersByName` est triée par nom d’utilisateur ; les critères de tri ne peut pas être personnalisées.


Le contrôle GridView peut être configuré pour utiliser la pagination personnalisée, mais uniquement lorsqu’elle est liée à un contrôle ObjectDataSource. Pour le contrôle ObjectDataSource implémenter la pagination personnalisée, elle nécessite deux méthodes : une qui est passé à un index de ligne de début et le nombre maximal d’enregistrements à afficher, et retourne le sous-ensemble précis d’enregistrements qui sont comprises dans cette étendue ; et une méthode qui retourne le nombre total d’enregistrements en cours de pagination via. Le `FindUsersByName` surcharge accepte un index de page et la taille de la page et retourne le nombre total d’enregistrements via un `ByRef` paramètre. Par conséquent, il existe une incompatibilité d’interface ici.

Une option serait de créer une classe proxy qui expose l’interface ObjectDataSource attend et appelle en interne la `FindUsersByName` (méthode). Une autre option - et celui que nous allons utiliser pour cet article - sont créer notre propre interface d’échange et l’utiliser au lieu de l’interface de pagination intégrée du contrôle GridView.

### <a name="creating-a-first-previous-next-last-paging-interface"></a>Création d’un premier, précédent, ensuite, la dernière Interface de pagination

Commençons par créer une interface de pagination en premier, précédent, suivant et dernier LinkButton. Le premier LinkButton, lorsque vous cliquez dessus, dirige l’utilisateur à la première page de données, tandis que le précédent lui retourne à la page précédente. De même, suivant et dernier déplacera l’utilisateur à la page suivante et dernière, respectivement. Ajoutez les quatre contrôles LinkButton sous le `UserAccounts` GridView.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample11.aspx)]

Ensuite, créez un gestionnaire d’événements pour chacun du LinkButton `Click` événements.

La figure 7 illustre le quatre LinkButton lorsqu’ils sont affichés via la vue de Visual Web Developer Design.


[![Ajouter tout d’abord, précédent, suivant, et le dernier type LinkButton sous le contrôle GridView](building-an-interface-to-select-one-user-account-from-many-vb/_static/image20.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image19.png)

**Figure 7**: ajouter le premier, précédent, suivant et dernier LinkButton sous GridView ([cliquez pour afficher l’image en taille réelle](building-an-interface-to-select-one-user-account-from-many-vb/_static/image21.png))


### <a name="keeping-track-of-the-current-page-index"></a>Suivi de l’Index de Page en cours

Lorsqu’un utilisateur visite tout d’abord le `ManageUsers.aspx` page ou clique sur un du filtrage boutons, nous souhaitons afficher la première page de données dans le contrôle GridView. Toutefois, lorsque l’utilisateur clique sur un de la navigation de type LinkButton, nous devons mettre à jour l’index de page. Pour conserver l’index de page et le nombre d’enregistrements à afficher par page, ajoutez les deux propriétés suivantes à la classe code-behind de la page :

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample12.vb)]

Comme le `UsernameToMatch` propriété, le `PageIndex` propriété conserve sa valeur à l’état d’affichage. En lecture seule `PageSize` propriété retourne une valeur codée en dur, 10. Inviter les lecteurs que cela intéresse pour mettre à jour de cette propriété pour utiliser le même modèle que `PageIndex`, puis d’augmenter la `ManageUsers.aspx` page telles que la personne visitant la page puisse spécifier combien de comptes utilisateur à afficher par page.

### <a name="retrieving-just-the-current-pages-records-updating-the-page-index-and-enabling-and-disabling-the-paging-interface-linkbuttons"></a>Récupérer uniquement les enregistrements de la Page active, la mise à jour de l’Index de Page et l’activation et désactivation de l’Interface de pagination type LinkButton

Avec l’interface de pagination en place et la `PageIndex` et `PageSize` propriétés ajoutées, nous sommes prêts à mettre à jour le `BindUserAccounts` méthode afin qu’il utilise approprié `FindUsersByName` de surcharge. En outre, nous devons avoir cette méthode activer ou désactiver l’interface de pagination selon quelle page est affiché. Lorsque vous affichez la première page de données, les liens de premier et précédent doivent être désactivées ; Ensuite, dernière doit être désactivée lors de l’affichage de la dernière page.

Mettez à jour la méthode `BindUserAccounts` avec le code suivant :

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample13.vb)]

Notez que le nombre total d’enregistrements en cours de pagination via est déterminé par le dernier paramètre de la `FindUsersByName` (méthode). Une fois que la page spécifiée des comptes d’utilisateur sont retournés, le quatre LinkButton est activés ou désactivés, selon si la première ou dernière page de données est affichée.

La dernière étape consiste à écrire le code pour le type LinkButton quatre `Click` gestionnaires d’événements. Ces gestionnaires d’événements doivent mettre à jour le `PageIndex` propriété, puis à relier les données au GridView via un appel à `BindUserAccounts` le prénom, le précédent et suivant gestionnaires d’événements sont très simples. Le `Click` Gestionnaire d’événements pour le dernier LinkButton, toutefois, est un peu plus complexe, car nous devons déterminer combien d’enregistrements est affichés afin de déterminer le dernier index de page.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample14.vb)]

Les figures 8 et 9 montrant l’interface de pagination personnalisée en action. La figure 8 illustre la `ManageUsers.aspx` page lors de l’affichage de la première page de données pour tous les comptes d’utilisateur. Notez que seuls 10 des 13 comptes sont affichés. Cliquant sur le lien suivant ou dernière provoque une publication (postback), les mises à jour le `PageIndex` à 1 et lie la deuxième page de l’utilisateur des comptes à la grille (voir Figure 9).


[![Les comptes d’utilisateur 10 premier sont affichés.](building-an-interface-to-select-one-user-account-from-many-vb/_static/image23.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image22.png)

**Figure 8**: les comptes d’utilisateur 10 premier sont affichés ([cliquez pour afficher l’image en taille réelle](building-an-interface-to-select-one-user-account-from-many-vb/_static/image24.png))


[![Cliquez sur le lien suivant pour afficher la deuxième Page de comptes d’utilisateur](building-an-interface-to-select-one-user-account-from-many-vb/_static/image26.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image25.png)

**Figure 9**: cliquez sur le lien suivant pour afficher la deuxième Page de comptes d’utilisateur ([cliquez pour afficher l’image en taille réelle](building-an-interface-to-select-one-user-account-from-many-vb/_static/image27.png))


## <a name="summary"></a>Récapitulatif

Les administrateurs ont souvent besoin sélectionner un utilisateur à partir de la liste des comptes. Dans les didacticiels précédents que nous avons étudié à l’aide d’une liste déroulante remplie avec les utilisateurs, mais cette approche n’évolue pas bien. Dans ce didacticiel, nous avons exploré une meilleure alternative : une interface filtrable dont les résultats sont affichés dans un GridView paginé. Avec cette interface utilisateur, les administrateurs peuvent rapidement et efficacement localiser et sélectionner un compte d’utilisateur parmi des milliers.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [Pagination personnalisée dans ASP.NET avec SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)
- [Pagination efficace dans de grandes quantités de données](https://asp.net/learn/data-access/tutorial-25-vb.aspx)
- [Déploiement de votre propre outil d’Administration de site Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>À propos de l’auteur

Scott Mitchell, auteur de plusieurs livres sur ASP/ASP.NET et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est *[SAM animer vous-même ASP.NET 2.0 des dernières 24 heures](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott peut être atteint à [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou via son blog à [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Entraîner un réviseur pour ce didacticiel a été Alicja Maziarz. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à

> [!div class="step-by-step"]
> [Précédent](unlocking-and-approving-user-accounts-cs.md)
> [Suivant](recovering-and-changing-passwords-vb.md)
