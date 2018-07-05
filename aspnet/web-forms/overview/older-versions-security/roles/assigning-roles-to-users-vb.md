---
uid: web-forms/overview/older-versions-security/roles/assigning-roles-to-users-vb
title: Attribution de rôles aux utilisateurs (VB) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons créer deux pages ASP.NET afin de faciliter la gestion de ce que les utilisateurs appartiennent à quels rôles. La première page comprend les ressources nécessaires pour voir ce que...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: fd208ee9-69cc-4467-9783-b4e039bdd1d3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/roles/assigning-roles-to-users-vb
msc.type: authoredcontent
ms.openlocfilehash: e0c653a53e63fd533d4d5490a08249d33edc94fc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397047"
---
<a name="assigning-roles-to-users-vb"></a>Attribution de rôles aux utilisateurs (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.10.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial10_AssigningRoles_vb.pdf)

> Dans ce didacticiel, nous allons créer deux pages ASP.NET afin de faciliter la gestion de ce que les utilisateurs appartiennent à quels rôles. La première page comprend les ressources nécessaires pour voir quels utilisateurs appartiennent à un rôle donné, les rôles qu’un utilisateur particulier appartient et la capacité d’affecter ou supprimer un utilisateur particulier à partir d’un rôle particulier. Dans la deuxième page, nous augmenterons le contrôle CreateUserWizard afin qu’il inclue une étape pour spécifier quels rôles appartient l’utilisateur nouvellement créé. Cela est utile dans les scénarios où un administrateur est en mesure de créer de nouveaux comptes d’utilisateur.


## <a name="introduction"></a>Introduction

Le <a id="_msoanchor_1"> </a> [didacticiel précédent](creating-and-managing-roles-vb.md) examiné le framework de rôles et les `SqlRoleProvider`; nous avons vu comment utiliser le `Roles` classe à créer, récupérer et supprimer des rôles. Outre la création et la suppression de rôles, nous devons être en mesure d’attribuer ou supprimer des utilisateurs d’un rôle. Malheureusement, ASP.NET n’est pas fourni avec tous les contrôles Web pour la gestion de ce que les utilisateurs appartiennent à quels rôles. Au lieu de cela, nous devons créer nos propres pages ASP.NET pour gérer ces associations. La bonne nouvelle est que l’ajout et suppression d’utilisateurs aux rôles est très simple. Le `Roles` classe contient un nombre de méthodes d’ajout d’un ou plusieurs utilisateurs à un ou plusieurs rôles.

Dans ce didacticiel, nous allons créer deux pages ASP.NET afin de faciliter la gestion de ce que les utilisateurs appartiennent à quels rôles. La première page comprend les ressources nécessaires pour voir quels utilisateurs appartiennent à un rôle donné, les rôles qu’un utilisateur particulier appartient et la capacité d’affecter ou supprimer un utilisateur particulier à partir d’un rôle particulier. Dans la deuxième page, nous augmenterons le contrôle CreateUserWizard afin qu’il inclue une étape pour spécifier quels rôles appartient l’utilisateur nouvellement créé. Cela est utile dans les scénarios où un administrateur est en mesure de créer de nouveaux comptes d’utilisateur.

C’est parti !

## <a name="listing-what-users-belong-to-what-roles"></a>Liste de ce que les utilisateurs appartiennent à quels rôles

La première chose pour ce didacticiel consiste à créer une page web à partir de laquelle les utilisateurs peuvent être affectés aux rôles. Avant de nous concernent nous-mêmes comment affecter des utilisateurs aux rôles, nous allons d’abord vous concentrer sur la façon de déterminer quels utilisateurs appartiennent à quels rôles. Il existe deux façons d’afficher ces informations : « par rôle » ou « par utilisateur. » Nous pouvons autoriser le visiteur à sélectionner un rôle et les afficher tous les utilisateurs qui appartiennent au rôle (l’affichage « par rôle »), ou nous pouvons inviter le visiteur pour sélectionner un utilisateur et de leur montrer les rôles attribués à cet utilisateur (l’affichage « par utilisateur »).

La vue « par rôle » est utile dans les cas où le visiteur souhaite connaître l’ensemble des utilisateurs qui appartiennent à un rôle particulier ; la vue « par utilisateur » est idéale lorsque le visiteur doit connaître l’ou les rôles d’un utilisateur particulier. Nous allons inclure dans notre page « par un rôle » et « par utilisateur » interfaces.

Nous allons commencer par la création de l’interface « par utilisateur ». Cette interface se compose d’une liste déroulante et une liste de cases à cocher. La liste déroulante contiendra l’ensemble des utilisateurs dans le système ; les cases à cocher énumère les rôles. Sélection d’un utilisateur dans la liste déroulante vérifie ces rôles de que l’utilisateur appartient. La personne visitant la page puis vérifiez ou désactivez les cases à cocher pour ajouter ou supprimer l’utilisateur sélectionné à partir des rôles correspondants.

> [!NOTE]
> À l’aide d’une liste déroulante liste à la liste les comptes d’utilisateur n’est pas un choix idéal pour les sites Web où il peut y avoir des centaines de comptes d’utilisateur. Une liste déroulante est conçue pour permettre à un utilisateur de choisir un élément dans une liste relativement courte des options. Il peut devenir difficile à gérer que le nombre d’éléments de liste augmente. Si vous créez un site Web qui aura un grand nombre de comptes d’utilisateur, vous souhaiterez à l’aide d’une autre interface utilisateur, telles que les un GridView paginable ou une interface filtrable qui répertorie les invites visiteur à choisir une lettre, puis uniquement Affiche les utilisateurs dont nom d’utilisateur commence par la lettre sélectionnée.


## <a name="step-1-building-the-by-user-user-interface"></a>Étape 1 : Génération de l’Interface utilisateur « Par utilisateur »

Ouvrez le `UsersAndRoles.aspx` page. En haut de la page, ajoutez un contrôle Web Label nommé `ActionStatus` et effacer son `Text` propriété. Nous allons utiliser cette étiquette pour fournir des commentaires sur les actions effectuées, affichage des messages comme, « utilisateur Tito a été ajouté au rôle Administrateurs » ou « Jisun de l’utilisateur a été supprimé du rôle superviseurs. » Afin de rendre ces messages se distinguent, définie l’étiquette `CssClass` propriété à « Important ».

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample1.aspx)]

Ensuite, ajoutez la définition de classe CSS suivante à la `Styles.css` feuille de style :

[!code-css[Main](assigning-roles-to-users-vb/samples/sample2.css)]

Cette définition CSS indique au navigateur pour afficher l’étiquette à l’aide d’une police rouge volumineuse. Figure 1 montre à cet effet par le biais du concepteur Visual Studio.


[![La propriété l’étiquette CssClass entraîne une grande police rouge](assigning-roles-to-users-vb/_static/image2.png)](assigning-roles-to-users-vb/_static/image1.png)

**Figure 1**: le contrôle Label `CssClass` propriété entraîne une grande taille, police de rouge ([cliquez pour afficher l’image en taille réelle](assigning-roles-to-users-vb/_static/image3.png))


Ensuite, ajoutez un contrôle DropDownList à la page, définissez son `ID` propriété `UserList`et définissez son `AutoPostBack` True à la propriété. Nous utiliserons cette DropDownList pour répertorier tous les utilisateurs dans le système. Cette DropDownList sera lié à une collection d’objets de MembershipUser. Étant donné que nous voulons DropDownList pour afficher la propriété de nom d’utilisateur de l’objet MembershipUser (et l’utiliser comme la valeur des éléments de liste), définissez la DropDownList `DataTextField` et `DataValueField` propriétés « UserName ».

Sous l’objet DropDownList, ajoutez un répéteur nommé `UsersRoleList`. Ce Repeater répertorie tous les rôles dans le système en tant que série de cases à cocher. Définir le Repeater `ItemTemplate` utilisant le balisage déclaratif suivant :

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample3.aspx)]

Le `ItemTemplate` balisage inclut un contrôle de case à cocher Web unique nommé `RoleCheckBox`. La case à cocher `AutoPostBack` propriété est définie sur True et la `Text` propriété est liée à `Container.DataItem`. La raison de la syntaxe de liaison de données est simplement `Container.DataItem` effet, le framework de rôles renvoie la liste des noms de rôles en tant que tableau de chaînes, et il est ce tableau de chaînes qui nous sera contraignant pour le contrôle Repeater. Une description détaillée de la raison pour laquelle cette syntaxe est utilisée pour afficher le contenu d’un tableau lié à un contrôle Web de données n’entre pas dans le cadre de ce didacticiel. Pour plus d’informations sur ce sujet, reportez-vous à [un tableau scalaire de liaison à un contrôle Web de données](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).

À ce stade balisage déclaratif de votre interface de « par utilisateur » doit ressembler à ce qui suit :

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample4.aspx)]

Nous sommes maintenant prêts à écrire le code pour lier l’ensemble des comptes d’utilisateur à la liste DropDownList et de l’ensemble de rôles pour le contrôle Repeater. Dans la classe de code-behind de la page, ajoutez une méthode nommée `BindUsersToUserList` et un autre nommé `BindRolesList`, en utilisant le code suivant :

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample5.vb)]

Le `BindUsersToUserList` méthode récupère tous les comptes d’utilisateur dans le système via le [ `Membership.GetAllUsers` méthode](https://msdn.microsoft.com/library/dy8swhya.aspx). Cette commande renvoie un [ `MembershipUserCollection` objet](https://msdn.microsoft.com/library/system.web.security.membershipusercollection.aspx), qui est une collection de [ `MembershipUser` instances](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx). Cette collection est ensuite liée à la `UserList` DropDownList. Le `MembershipUser` cette composition de la collection contient un certain nombre de propriétés, telles que les instances `UserName`, `Email`, `CreationDate`, et `IsOnline`. Pour indiquer à l’objet DropDownList pour afficher la valeur de la `UserName` propriété, vérifiez que le `UserList` de DropDownList `DataTextField` et `DataValueField` propriétés ont été définies à « UserName ».

> [!NOTE]
> Le `Membership.GetAllUsers` méthode a deux surcharges : une qui n’accepte aucun paramètre d’entrée et retourne tous les utilisateurs et une qui prend les valeurs entières pour les index et la taille de la page et retourne uniquement le sous-ensemble spécifié d’utilisateurs. Lorsqu’il existe de grandes quantités de comptes d’utilisateur qui est affichés dans un élément d’interface utilisateur paginable, la deuxième surcharge peut être utilisé de parcourir plus efficacement les utilisateurs dans la mesure où elle retourne uniquement le sous-ensemble précis de comptes d’utilisateurs plutôt que tous les.


Le `BindRolesToList` méthode démarre en appelant le `Roles` la classe [ `GetAllRoles` méthode](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx), qui retourne un tableau de chaînes contenant les rôles dans le système. Ce tableau de chaînes est ensuite lié au répéteur.

Enfin, nous devons appeler ces deux méthodes lorsque la page est chargée. Ajoutez le code suivant au gestionnaire d'événements `Page_Load` :

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample6.vb)]

Avec ce code en place, prenez un moment pour visiter la page via un navigateur ; votre écran doit ressembler à la Figure 2. Tous les comptes d’utilisateurs sont remplis dans la liste déroulante et, en dessous, chaque rôle apparaît sous la forme d’une case à cocher. Car nous avons défini le `AutoPostBack` propriétés des cases à cocher et DropDownList sur True, modification de l’utilisateur sélectionné ou en activant ou en désactivant un rôle provoque une publication (postback). Aucune action n’est effectuée, toutefois, étant donné que nous devons encore écrire du code pour gérer ces actions. Nous les aborderons ces tâches dans les deux sections suivantes.


[![La Page affiche les utilisateurs et rôles](assigning-roles-to-users-vb/_static/image5.png)](assigning-roles-to-users-vb/_static/image4.png)

**Figure 2**: la Page affiche les utilisateurs et les rôles ([cliquez pour afficher l’image en taille réelle](assigning-roles-to-users-vb/_static/image6.png))


### <a name="checking-the-roles-the-selected-user-belongs-to"></a>Vérifier les rôles de l’utilisateur sélectionné appartient

Lorsque la page est chargée, ou chaque fois que le visiteur sélectionne un nouvel utilisateur dans la liste déroulante, nous devons mettre à jour le `UsersRoleList`de cases à cocher afin qu’une case à cocher du rôle donné est activée uniquement si l’utilisateur sélectionné appartient à ce rôle. Pour ce faire, créez une méthode nommée `CheckRolesForSelectedUser` avec le code suivant :

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample7.vb)]

Le code ci-dessus commence par déterminer qui est l’utilisateur sélectionné. Il utilise ensuite la classe rôles [ `GetRolesForUser(userName)` méthode](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx) pour renvoyer l’ensemble de l’utilisateur spécifié des rôles en tant que tableau de chaînes. Ensuite, les éléments de répéteur sont énumérées et de chaque élément `RoleCheckBox` case à cocher est référencé par programmation. La case à cocher est activée uniquement si le rôle qu’il correspond à est contenu dans le `selectedUsersRoles` tableau de chaînes.

> [!NOTE]
> Le `Linq.Enumerable.Contains(Of String)(...)` syntaxe ne compilera pas si vous n’utilisez pas ASP.NET version 2.0. Le `Contains(Of String)` méthode fait partie de la [bibliothèque LINQ](http://en.wikipedia.org/wiki/Language_Integrated_Query), qui est une nouveauté pour ASP.NET 3.5. Si vous continuez d’utiliser ASP.NET version 2.0, utilisez la [ `Array.IndexOf(Of String)` méthode](https://msdn.microsoft.com/library/eha9t187.aspx) à la place.


Le `CheckRolesForSelectedUser` méthode doit être appelée dans deux cas : lorsque la page est chargée et chaque fois que le `UserList` de DropDownList sélectionné est modifié. Par conséquent, appelez cette méthode à partir de la `Page_Load` Gestionnaire d’événements (après les appels à `BindUsersToUserList` et `BindRolesToList`). En outre, créez un gestionnaire d’événements pour la DropDownList `SelectedIndexChanged` événement et appeler cette méthode à partir de là.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample8.vb)]

Avec ce code en place, vous pouvez tester la page via le navigateur. Toutefois, étant donné que le `UsersAndRoles.aspx` page ne dispose pas actuellement la possibilité d’affecter des utilisateurs aux rôles, aucun utilisateur n’a de rôles. Nous allons créer l’interface pour l’affectation d’utilisateurs aux rôles dans un instant, afin de prendre mon mot que ce code fonctionne et vérifiez qu’il le fait ultérieurement, ou vous pouvez ajouter manuellement des utilisateurs aux rôles en insérant des enregistrements dans la `aspnet_UsersInRoles` table afin de tester cette functi onality maintenant.

### <a name="assigning-and-removing-users-from-roles"></a>Affectation et la suppression des utilisateurs des rôles

Lorsque le visiteur vérifie ou désactive une case à cocher dans la `UsersRoleList` Repeater, nous devons ajouter ou supprimer l’utilisateur sélectionné dans le rôle correspondant. La case à cocher `AutoPostBack` propriété est actuellement définie sur True, ce qui provoque une publication (postback) chaque fois qu’une case à cocher dans le contrôle Repeater est activée ou désactivée. En bref, nous devons créer un gestionnaire d’événements pour la case à cocher `CheckChanged` événement. Étant donné que la case à cocher est dans un contrôle Repeater, nous devons ajouter manuellement la plomberie de gestionnaire d’événements. Commencez par ajouter le Gestionnaire d’événements à la classe code-behind, comme un `Protected` (méthode), comme suit :

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample9.vb)]

Nous retournera pour écrire le code pour ce gestionnaire d’événements dans un instant. Mais tout d’abord terminons la plomberie de gestion d’événements. À partir de la case à cocher dans le répéteur `ItemTemplate`, ajoutez `OnCheckedChanged="RoleCheckBox_CheckChanged"`. Cette syntaxe associe la `RoleCheckBox_CheckChanged` Gestionnaire d’événements à la `RoleCheckBox`de `CheckedChanged` événements.

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample10.aspx)]

La dernière tâche consiste à effectuer la `RoleCheckBox_CheckChanged` Gestionnaire d’événements. Nous devons commencer en référençant le contrôle de case à cocher qui a déclenché l’événement, car cette instance de la case à cocher indique que le rôle a été activée ou désactivée par le biais de son `Text` et `Checked` propriétés. À l’aide de ces informations avec le nom d’utilisateur de l’utilisateur sélectionné, nous ajouter ou supprimer l’utilisateur du rôle via le `Roles` la classe [ `AddUserToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx) ou [ `RemoveUserFromRole` méthode](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx).

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample11.vb)]

Le code ci-dessus commence par référencer par programmation de la case à cocher qui a déclenché l’événement, qui est disponible via le `sender` paramètre d’entrée. Si la case à cocher est activée, l’utilisateur sélectionné est ajouté au rôle spécifié, sinon qu'ils sont supprimés du rôle. Dans les deux cas, le `ActionStatus` étiquette affiche un message qui résume l’action qui vient d’être exécutée.

Prenez un moment pour tester cette page via un navigateur. Sélectionnez utilisateur Tito, puis ajoutez Tito aux rôles les administrateurs et les superviseurs.


[![Tito a été ajoutée pour les administrateurs et les rôles de superviseurs](assigning-roles-to-users-vb/_static/image8.png)](assigning-roles-to-users-vb/_static/image7.png)

**Figure 3**: Tito a été ajoutée pour les administrateurs et les rôles de superviseurs ([cliquez pour afficher l’image en taille réelle](assigning-roles-to-users-vb/_static/image9.png))


Ensuite, sélectionnez utilisateur Bruce dans la liste déroulante. Il existe une publication (postback) et les cases à cocher du répéteur sont mis à jour le `CheckRolesForSelectedUser`. Dans la mesure où Bruce n’appartient pas encore à tous les rôles, les deux cases sont désactivés. Ensuite, ajoutez Bruce au rôle superviseurs.


[![Bruce a été ajouté au rôle superviseurs](assigning-roles-to-users-vb/_static/image11.png)](assigning-roles-to-users-vb/_static/image10.png)

**Figure 4**: Bruce a été ajouté au rôle superviseurs ([cliquez pour afficher l’image en taille réelle](assigning-roles-to-users-vb/_static/image12.png))


Pour vérifier les fonctionnalités de la `CheckRolesForSelectedUser` (méthode), sélectionnez un utilisateur autre que Tito ou Bruce. Notez la façon dont les cases à cocher sont automatiquement désactivés, indiquant qu’ils n’appartiennent pas à tous les rôles. Revenir à Tito. Les administrateurs et les superviseurs cases à cocher doit être vérifiée.

## <a name="step-2-building-the-by-roles-user-interface"></a>Étape 2 : Génération de l’Interface utilisateur « Par les rôles »

À ce stade nous l’interface « par les utilisateurs » s’est terminé et que vous êtes prêts à commencer à vous attaquer l’interface « par rôles ». L’interface « par rôles » invite l’utilisateur à sélectionner un rôle dans la liste déroulante, puis affiche l’ensemble des utilisateurs qui appartiennent à ce rôle dans un GridView.

Ajoutez un autre contrôle DropDownList pour la `UsersAndRoles.aspx page`. Placer celle-ci sous le contrôle Repeater, nommez-le `RoleList`et définissez son `AutoPostBack` True à la propriété. En dessous, ajoutez un GridView et nommez-le `RolesUserList`. Ce GridView répertorie les utilisateurs qui appartiennent au rôle sélectionné. Définir le GridView `AutoGenerateColumns` propriété sur False, ajouter un TemplateField à la grille `Columns` collection et définissez son `HeaderText` propriété à « Utilisateurs ». Définir le TemplateField `ItemTemplate` afin qu’il affiche la valeur de l’expression de liaison de données `Container.DataItem` dans le `Text` propriété d’une étiquette nommée `UserNameLabel`.

Après avoir ajouté et configuré le contrôle GridView, balisage déclaratif de votre interface de « par le rôle » doit ressembler à ce qui suit :

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample12.aspx)]

Nous avons besoin remplir le `RoleList` DropDownList avec l’ensemble de rôles dans le système. Pour ce faire, mettez à jour le `BindRolesToList` méthode voilà lie le tableau de chaînes retourné par la `Roles.GetAllRoles` méthode à la `RolesList` DropDownList (ainsi que le `UsersRoleList` Repeater).

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample13.vb)]

Les deux dernières lignes dans le `BindRolesToList` méthode ont été ajoutées pour lier l’ensemble de rôles à le `RoleList` contrôle DropDownList. La figure 5 illustre le résultat final, lorsqu’ils sont affichés via un navigateur : une liste déroulante remplie avec les rôles du système.


[![Les rôles sont affichés dans RoleList DropDownList](assigning-roles-to-users-vb/_static/image14.png)](assigning-roles-to-users-vb/_static/image13.png)

**Figure 5**: les rôles s’affichent dans le `RoleList` DropDownList ([cliquez pour afficher l’image en taille réelle](assigning-roles-to-users-vb/_static/image15.png))


### <a name="displaying-the-users-that-belong-to-the-selected-role"></a>Afficher les utilisateurs qui appartiennent au rôle sélectionné

Lorsque la page est chargée, ou quand un nouveau rôle est sélectionné dans le `RoleList` DropDownList, nous avons besoin afficher la liste des utilisateurs qui appartiennent à ce rôle dans le contrôle GridView. Créez une méthode nommée `DisplayUsersBelongingToRole` utilisant le code suivant :

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample14.vb)]

Cette méthode démarre en obtenant le rôle sélectionné à partir de la `RoleList` DropDownList. Il utilise ensuite le [ `Roles.GetUsersInRole(roleName)` méthode](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx) pour récupérer un tableau de chaînes des noms d’utilisateur des utilisateurs qui appartiennent à ce rôle. Ce tableau est ensuite lié à la `RolesUserList` GridView.

Cette méthode doit être appelée dans deux cas : lorsque la page est chargée initialement et lorsque le rôle sélectionné dans la `RoleList` DropDownList modifications. Par conséquent, mettre à jour le `Page_Load` Gestionnaire d’événements afin que cette méthode est appelée après l’appel à `CheckRolesForSelectedUser`. Ensuite, créez un gestionnaire d’événements pour le `RoleList`de `SelectedIndexChanged` événement et appeler cette méthode à partir de là, trop.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample15.vb)]

Avec ce code en place, le `RolesUserList` GridView doit afficher les utilisateurs qui appartiennent au rôle sélectionné. Comme le montre la Figure 6, le rôle superviseurs se compose de deux membres : Bruce et Tito.


[![Le contrôle GridView répertorie les utilisateurs qui appartiennent au rôle sélectionné](assigning-roles-to-users-vb/_static/image17.png)](assigning-roles-to-users-vb/_static/image16.png)

**Figure 6**: le GridView répertorie ceux les utilisateurs qu’appartiennent au rôle sélectionné ([cliquez pour afficher l’image en taille réelle](assigning-roles-to-users-vb/_static/image18.png))


### <a name="removing-users-from-the-selected-role"></a>Suppression d’utilisateurs dans le rôle sélectionné

Nous allons augmenter la `RolesUserList` GridView afin qu’il inclue une colonne de « supprimer » de boutons. En cliquant sur le bouton « Supprimer » pour un utilisateur particulier supprime les de ce rôle.

Commencez par ajouter un champ de bouton de suppression pour le contrôle GridView. Rendre ce champ apparaissent sous la forme la plus archivé de gauche et de modifier son `DeleteText` propriété à partir de « Delete » (la valeur par défaut) à « Supprimer ».


[![Ajouter le](assigning-roles-to-users-vb/_static/image20.png)](assigning-roles-to-users-vb/_static/image19.png)

**Figure 7**: ajouter le bouton « Supprimer » pour le contrôle GridView ([cliquez pour afficher l’image en taille réelle](assigning-roles-to-users-vb/_static/image21.png))


Un clic sur le bouton « Supprimer » est une publication (postback) s’ensuit et le GridView `RowDeleting` événement est déclenché. Nous devons créer un gestionnaire d’événements pour cet événement et d’écrire du code qui supprime l’utilisateur le rôle sélectionné. Créer le Gestionnaire d’événements et ajoutez le code suivant :

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample16.vb)]

Le code commence par déterminer le nom du rôle sélectionné. Puis par programmation références le `UserNameLabel` contrôle à partir de la ligne dont bouton « Supprimer » a été utilisé afin de déterminer le nom d’utilisateur de l’utilisateur à supprimer. L’utilisateur est ensuite supprimé le rôle via un appel à la `Roles.RemoveUserFromRole` (méthode). Le `RolesUserList` GridView est alors actualisée et un message s’affiche la `ActionStatus` contrôle Label.

> [!NOTE]
> Le bouton « Supprimer » ne nécessite pas une forme quelconque de confirmation de l’utilisateur avant de supprimer l’utilisateur du rôle. Je vous invite à ajouter un niveau de confirmation de l’utilisateur. Une des méthodes plus simples pour confirmer une action est via une boîte de dialogue de confirmation du côté client. Pour plus d’informations sur cette technique, consultez [Ajout côté Client Confirmation lors de la suppression](https://asp.net/learn/data-access/tutorial-42-vb.aspx).


La figure 8 illustre la page une fois que l’utilisateur Tito a été supprimé du groupe superviseurs.


[![Hélas, Tito n’est plus un superviseur](assigning-roles-to-users-vb/_static/image23.png)](assigning-roles-to-users-vb/_static/image22.png)

**Figure 8**: Hélas, Tito n’est plus un superviseur ([cliquez pour afficher l’image en taille réelle](assigning-roles-to-users-vb/_static/image24.png))


### <a name="adding-new-users-to-the-selected-role"></a>Ajout de nouveaux utilisateurs au rôle sélectionné

Ainsi que la suppression d’utilisateurs dans le rôle sélectionné, le visiteur à cette page doit également être en mesure d’ajouter un utilisateur au rôle sélectionné. La meilleure interface pour l’ajout d’un utilisateur au rôle sélectionné varie selon le nombre de comptes d’utilisateur que vous prévoyez d’avoir. Si votre site Web va héberger quelques comptes d’utilisateur douzaines ou moins, vous pouvez utiliser un DropDownList ici. S’il peut y avoir des milliers de comptes d’utilisateur, vous pouvez inclure une interface utilisateur qui autorise le visiteur pour parcourir les comptes, recherche d’un compte particulier, ou de filtrer les comptes d’utilisateur d’une autre façon.

Pour cette page Nous allons utiliser une interface très simple qui fonctionne indépendamment du nombre de comptes d’utilisateur dans le système. À savoir, nous allons utiliser une zone de texte, en invitant le visiteur à taper le nom d’utilisateur de l’utilisateur que vous souhaitez ajouter au rôle sélectionné. Si aucun utilisateur portant ce nom n’existe, ou si l’utilisateur est déjà un membre du rôle, nous affichons un message dans `ActionStatus` étiquette. Mais si l’utilisateur existe et n’est pas un membre du rôle, nous allons les ajouter au rôle et actualiser la grille.

Ajoutez une zone de texte et un bouton sous le contrôle GridView. Définir la zone de texte `ID` à `UserNameToAddToRole` et affectez à la `ID` et `Text` propriétés à `AddUserToRoleButton` et « Ajouter au rôle d’utilisateur », respectivement.

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample17.aspx)]

Ensuite, créez un `Click` Gestionnaire d’événements pour le `AddUserToRoleButton` et ajoutez le code suivant :

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample18.vb)]

La majorité du code dans le `Click` Gestionnaire d’événements effectue plusieurs vérifications de validation. Elle garantit que le visiteur fourni un nom d’utilisateur dans le `UserNameToAddToRole` zone de texte, que l’utilisateur existe dans le système, et qui ils n’appartiennent pas déjà pour le rôle sélectionné. Si aucune de ces vérifications échoue, un message approprié s’affiche dans `ActionStatus` et que le Gestionnaire d’événements est quitté. Si toutes les vérifications réussissent, l’utilisateur est ajouté au rôle via le `Roles.AddUserToRole` (méthode). Après de cela, la zone de texte `Text` nettoyée de propriété, le contrôle GridView est actualisé et le `ActionStatus` étiquette affiche un message indiquant que l’utilisateur spécifié a été ajouté au rôle sélectionné.

> [!NOTE]
> Pour vous assurer que l’utilisateur spécifié n’appartient pas déjà pour le rôle sélectionné, nous utilisons le [ `Roles.IsUserInRole(userName, roleName)` méthode](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx), qui retourne une valeur booléenne indiquant si *nom d’utilisateur* est un membre de *roleName*. Nous allons utiliser cette méthode à nouveau dans le <a id="_msoanchor_2"> </a> [didacticiel suivant](role-based-authorization-vb.md) lorsque nous examinons en fonction du rôle de l’autorisation.


Visitez la page via un navigateur et sélectionnez le rôle superviseurs à partir de la `RoleList` DropDownList. Essayez d’entrer un nom d’utilisateur non valide : vous devez voir un message expliquant que l’utilisateur n’existe pas dans le système.


[![Impossible d’ajouter un utilisateur Non existants à un rôle](assigning-roles-to-users-vb/_static/image26.png)](assigning-roles-to-users-vb/_static/image25.png)

**Figure 9**: Impossible d’ajouter un utilisateur inexistant à un rôle ([cliquez pour afficher l’image en taille réelle](assigning-roles-to-users-vb/_static/image27.png))


Maintenant essayez d’ajouter un utilisateur valide. Continuez et ajoutez de nouveau Tito au rôle superviseurs.


[![Tito est à nouveau un superviseur !](assigning-roles-to-users-vb/_static/image29.png)](assigning-roles-to-users-vb/_static/image28.png)

**Figure 10**: Tito est à nouveau un superviseur !  ([Cliquez pour afficher l’image en taille réelle](assigning-roles-to-users-vb/_static/image30.png))


## <a name="step-3-cross-updating-the-by-user-and-by-role-interfaces"></a>Étape 3 : Cross-la mise à jour le « Par utilisateur » et « Par rôle » Interfaces

Le `UsersAndRoles.aspx` page offre deux interfaces distinctes pour la gestion des utilisateurs et des rôles. Actuellement, ces deux interfaces agissent indépendamment les uns des autres, il est possible qu’une modification effectuée dans une interface n’est pas répercutée immédiatement dans l’autre. Par exemple, imaginez que le visiteur à la page Sélectionne le rôle de superviseurs à partir de la `RoleList` DropDownList, qui répertorie les Bruce et Tito en tant que ses membres. Ensuite, le visiteur sélectionne Tito à partir de la `UserList` DropDownList, qui vérifie les cases à cocher des contrôleurs et des administrateurs dans le `UsersRoleList` Repeater. Si le visiteur désactive ensuite le rôle superviseur de répéteur, Tito est supprimé du rôle superviseurs, mais cette modification n’est pas répercutée dans l’interface « par rôle ». Le contrôle GridView apparaît toujours Tito comme étant un membre du rôle superviseurs.

Pour corriger cela nous devons actualiser le GridView chaque fois qu’un rôle est activé ou désactivé à partir de la `UsersRoleList` Repeater. De même, nous devons actualiser le Repeater chaque fois qu’un utilisateur est supprimé ou ajouté à un rôle à partir de l’interface « par rôle ».

Le Repeater dans l’interface « par utilisateur » est actualisé en appelant le `CheckRolesForSelectedUser` (méthode). L’interface « par rôle » peut être modifié dans le `RolesUserList` contrôle GridView `RowDeleting` Gestionnaire d’événements et le `AddUserToRoleButton` du bouton `Click` Gestionnaire d’événements. Par conséquent, nous devons appeler le `CheckRolesForSelectedUser` méthode à partir de chacune de ces méthodes.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample19.vb)]

De même, le contrôle GridView dans l’interface « par rôle » est actualisé en appelant le `DisplayUsersBelongingToRole` (méthode) et l’interface « par utilisateur » est modifié via le `RoleCheckBox_CheckChanged` Gestionnaire d’événements. Par conséquent, nous devons appeler le `DisplayUsersBelongingToRole` méthode à partir de ce gestionnaire d’événements.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample20.vb)]

Avec ces modifications mineures au code, le « par utilisateur » et « par rôle » interfaces désormais correctement entre la mise à jour. Pour vérifier ceci, visitez la page via un navigateur et sélectionnez Tito et superviseurs à partir de la `UserList` et `RoleList` DropDownList, respectivement. Notez que comme vous désactivez le rôle superviseurs Tito à partir de l’opération de répétition dans l’interface « par utilisateur », Tito est automatiquement supprimé de la GridView dans l’interface « par rôle ». Ajout automatique Tito au rôle superviseurs à partir de l’interface « par le rôle » vérifie à nouveau la case à cocher superviseurs dans l’interface « par utilisateur ».

## <a name="step-4-customizing-the-createuserwizard-to-include-a-specify-roles-step"></a>Étape 4 : Personnalisation CreateUserWizard pour inclure une étape « Spécifier les rôles »

Dans le <a id="_msoanchor_3"> </a> [ *création de comptes utilisateur* ](../membership/creating-user-accounts-vb.md) didacticiel, nous avons vu comment utiliser le contrôle CreateUserWizard Web pour fournir une interface pour la création d’un nouveau compte d’utilisateur. Le contrôle CreateUserWizard peut être utilisé de deux manières :

- Comme un moyen pour les visiteurs de créer leur propre compte d’utilisateur sur le site, et
- Comme un moyen pour les administrateurs de créer des comptes

Dans le premier cas d’utilisation, un visiteur est fourni pour le site et remplit le contrôle CreateUserWizard, saisie des informations afin de s’inscrire sur le site. Dans le deuxième cas, un administrateur crée un nouveau compte pour une autre personne.

Lorsqu’un compte est créé par un administrateur d’une autre personne, il peut être utile d’autoriser l’administrateur de spécifier les rôles que le nouveau compte d’utilisateur appartient. Dans le <a id="_msoanchor_4"> </a> [ *stockage* *des informations utilisateur supplémentaires* ](../membership/storing-additional-user-information-vb.md) didacticiel, nous avons vu comment personnaliser le contrôle CreateUserWizard en ajoutant supplémentaires `WizardSteps`. Nous allons examiner comment ajouter une étape supplémentaire pour le contrôle CreateUserWizard afin de spécifier les rôles du nouvel utilisateur.

Ouvrez le `CreateUserWizardWithRoles.aspx` page et ajoutez un contrôle CreateUserWizard nommé `RegisterUserWithRoles`. Définir le contrôle `ContinueDestinationPageUrl` propriété à « ~ / Default.aspx ». Étant donné que l’idée ici est qu’un administrateur l’utiliserez ce contrôle CreateUserWizard pour créer de nouveaux comptes d’utilisateur, définissez le contrôle `LoginCreatedUser` False à la propriété. Cela `LoginCreatedUser` propriété spécifie si le visiteur est automatiquement connecté en tant que l’utilisateur récemment créée, et sa valeur par défaut sur True. Nous affectez-lui la valeur False, car lorsqu’un administrateur crée un nouveau compte nous souhaitons lui connecté en tant que lui-même.

Ensuite, sélectionnez le « Ajout/suppression `WizardSteps`... » option à partir de la balise active de CreateUserWizard et ajouter un nouveau `WizardStep`, ce qui affecte son `ID` à `SpecifyRolesStep`. Déplacer le `SpecifyRolesStep WizardStep` afin qu’il s’agit d’après l’étape « Sign Up for votre nouveau compte », mais avant l’étape « Complete ». Définir le `WizardStep`de `Title` propriété à « Spécifier les rôles », son `StepType` propriété `Step`et son `AllowReturn` False à la propriété.


[![Ajouter le](assigning-roles-to-users-vb/_static/image32.png)](assigning-roles-to-users-vb/_static/image31.png)

**Figure 11**: ajouter la « spécifier les rôles » `WizardStep` à CreateUserWizard ([cliquez pour afficher l’image en taille réelle](assigning-roles-to-users-vb/_static/image33.png))


Après cette modification balisage déclaratif de votre CreateUserWizard doit ressembler à ce qui suit :

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample21.aspx)]

Dans le « spécifier les rôles » `WizardStep`, ajouter CheckBoxList nommé `RoleList.` CheckBoxList This répertorie les rôles disponibles, l’activation de la personne visitant la page vérifier quels rôles de l’utilisateur nouvellement créé appartient.

Nous restons confrontés à deux tâches de codage : nous devons tout d’abord remplir la `RoleList` CheckBoxList avec les rôles dans le système ; ensuite, nous devons ajouter l’utilisateur créé pour les rôles sélectionnés lorsque l’utilisateur déplace à partir de l’étape « Spécifier les rôles » à l’étape « Terminé ». Nous pouvons atteindre la première tâche dans le `Page_Load` Gestionnaire d’événements. Le code suivant par programmation références le `RoleList` case à cocher dans la première, visitez la page et lie les rôles dans le système à celle-ci.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample22.vb)]

Le code ci-dessus doit sembler familière. Dans le <a id="_msoanchor_5"> </a> [ *stockage* *des informations utilisateur supplémentaires* ](../membership/storing-additional-user-information-vb.md) didacticiel, nous avons utilisé deux `FindControl` instructions pour référencer un contrôle Web à partir de dans un personnalisé `WizardStep`. Et le code qui lie les rôles à CheckBoxList a été extraite précédemment dans ce didacticiel.

Pour effectuer la seconde tâche de programmation, nous devons savoir quand l’étape « Spécifier les rôles » a été effectuée. Souvenez-vous que le contrôle CreateUserWizard a un `ActiveStepChanged` événement qui se déclenche chaque fois que le visiteur accède à partir d’une seule étape à l’autre. Ici, nous pouvons déterminer si l’utilisateur a atteint l’étape « Terminé ». Dans ce cas, nous devons ajouter l’utilisateur pour les rôles sélectionnés.

Créer un gestionnaire d’événements pour le `ActiveStepChanged` événement et ajoutez le code suivant :

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample23.vb)]

Si l’utilisateur a atteint uniquement l’étape « Terminé », le Gestionnaire d’événements énumère les éléments de la `RoleList` CheckBoxList et l’utilisateur nouvellement créé est affectée aux rôles sélectionnés.

Visitez cette page via un navigateur. La première étape dans le contrôle CreateUserWizard est l’étape « Sign Up for votre nouveau compte » standard, qui invite à entrer le nouveau nom d’utilisateur, mot de passe, messagerie et autres informations clés. Entrez les informations pour créer un utilisateur nommé Wanda.


[![Créer un utilisateur nommé Wanda](assigning-roles-to-users-vb/_static/image35.png)](assigning-roles-to-users-vb/_static/image34.png)

**Figure 12**: créer un nouveau Wanda de nommé utilisateur ([cliquez pour afficher l’image en taille réelle](assigning-roles-to-users-vb/_static/image36.png))


Cliquez sur le bouton « Create User ». CreateUserWizard appelle en interne la `Membership.CreateUser` méthode, la création du nouveau compte d’utilisateur, puis passe à l’étape suivante, « spécifier les rôles ». Ici, les rôles de système sont répertoriés. Activez la case à cocher superviseurs et cliquez sur Suivant.


[![Faire Wanda un membre du rôle superviseurs](assigning-roles-to-users-vb/_static/image38.png)](assigning-roles-to-users-vb/_static/image37.png)

**Figure 13**: faire Wanda un membre du rôle superviseurs ([cliquez pour afficher l’image en taille réelle](assigning-roles-to-users-vb/_static/image39.png))


Clic sur suivant provoque une publication (postback) et les mises à jour le `ActiveStep` à l’étape « Terminé ». Dans le `ActiveStepChanged` Gestionnaire d’événements, le compte d’utilisateur récemment créé est affecté au rôle superviseurs. Pour vérifier ceci, revenez à la `UsersAndRoles.aspx` page et sélectionnez les superviseurs à partir de la `RoleList` DropDownList. Comme le montre la Figure 14, les superviseurs sont désormais composés de trois utilisateurs : Bruce, Tito et Wanda.


[![Bruce, Tito et Wanda sont tous les superviseurs](assigning-roles-to-users-vb/_static/image41.png)](assigning-roles-to-users-vb/_static/image40.png)

**Figure 14**: Bruce, Tito et Wanda sont tous les superviseurs ([cliquez pour afficher l’image en taille réelle](assigning-roles-to-users-vb/_static/image42.png))


## <a name="summary"></a>Récapitulatif

L’infrastructure de rôles offre des méthodes pour récupérer des informations sur les rôles et des méthodes pour déterminer quels utilisateurs appartiennent à un rôle spécifié d’un utilisateur particulier. En outre, il existe plusieurs méthodes pour ajouter et supprimer un ou plusieurs utilisateurs à un ou plusieurs rôles. Dans ce didacticiel, nous nous sommes concentrés sur deux de ces méthodes : `AddUserToRole` et `RemoveUserFromRole`. Il existe des variantes supplémentaires conçues pour ajouter plusieurs utilisateurs à un rôle unique et pour affecter plusieurs rôles à un seul utilisateur.

Ce didacticiel comprend également un coup de œil à étendre le contrôle CreateUserWizard à inclure un `WizardStep` pour spécifier des rôles de l’utilisateur nouvellement créé. Une étape de ce type peut aider l’administrateur à rationaliser le processus de création de comptes d’utilisateur pour les nouveaux utilisateurs.

À ce stade, nous avons vu comment créer et supprimer des rôles et comment ajouter et supprimer des utilisateurs des rôles. Mais nous devons encore examiner l’application d’autorisation en fonction du rôle. Dans le <a id="_msoanchor_6"> </a> [didacticiel suivant](role-based-authorization-vb.md) nous allons examiner la définition de règles d’autorisation d’URL sur une base de rôle par rôle, ainsi que la manière dont pour limiter la fonctionnalité de niveau de page basée sur les rôles de l’utilisateur actuellement connecté.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [Présentation de l’outil Administration Site Web ASP.NET](https://msdn.microsoft.com/library/ms228053.aspx)
- [Examen ASP. Appartenance, rôles et profil du NET](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Déploiement de votre propre outil d’Administration de site Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>À propos de l’auteur

Scott Mitchell, auteur de plusieurs livres sur ASP/ASP.NET et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est *[SAM animer vous-même ASP.NET 2.0 des dernières 24 heures](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott peut être atteint à [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou via son blog à [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements particuliers à...

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Entraîner un réviseur pour ce didacticiel a été Teresa Murphy. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](creating-and-managing-roles-vb.md)
> [Suivant](role-based-authorization-vb.md)
