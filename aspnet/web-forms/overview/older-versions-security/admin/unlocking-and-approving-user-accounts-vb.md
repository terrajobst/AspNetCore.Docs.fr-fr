---
uid: web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-vb
title: Le déverrouillage et l’approbation des comptes d’utilisateur (VB) | Microsoft Docs
author: rick-anderson
description: Ce didacticiel montre comment créer une page web pour les administrateurs gérer les utilisateurs verrouillés et approuvé les États. Nous verrons également comment approuver les nouveaux utilisateurs o...
ms.author: aspnetcontent
ms.date: 04/01/2008
ms.assetid: 041854a5-ea8c-4de0-82f1-121ba6cb2893
msc.legacyurl: /web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-vb
msc.type: authoredcontent
ms.openlocfilehash: 4e41cee6f722d3fe6c19d8179b3f27800de80077
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811294"
---
<a name="unlocking-and-approving-user-accounts-vb"></a>Déblocage et approbation des comptes utilisateur (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/VB.14.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial14_UnlockAndApprove_vb.pdf)

> Ce didacticiel montre comment créer une page web pour les administrateurs gérer les utilisateurs verrouillés et approuvé les États. Nous verrons également comment approuver les nouveaux utilisateurs uniquement une fois qu’ils ont vérifié leur e-mail.


## <a name="introduction"></a>Introduction

Avec un nom d’utilisateur, le mot de passe et le courrier électronique, chaque compte d’utilisateur a deux champs d’état qui déterminent si l’utilisateur peut se connecter au site : verrouillé et approuvé. Un utilisateur est automatiquement verrouillé s’ils fournissent des informations d’identification non valides, un nombre spécifié de fois au sein d’un nombre spécifié de minutes (les paramètres par défaut verrouiller un utilisateur après 5 tentatives de connexion non valide dans les 10 minutes). L’état approuvé est utile dans les scénarios où une action doit s’écouler avant un nouvel utilisateur est en mesure d’ouvrir une session sur le site. Par exemple, un utilisateur peut avoir besoin de vérifier tout d’abord leur adresse de messagerie ou être approuvé par un administrateur avant de pouvoir se connecter.

Étant donné qu’un verrouillé out ou non approuvés l’utilisateur ne peut pas se connecter, il est naturel uniquement à se demander comment ces États peuvent être réinitialisés. ASP.NET n’inclut pas toutes les fonctionnalités intégrées ou les contrôles Web pour la gestion des utilisateurs verrouillés et approuvé les États, en partie parce que ces décisions doivent être traitées sur une base site par site. Certains sites peuvent approuver automatiquement tous les nouveaux comptes d’utilisateur (le comportement par défaut). D’autres ont un administrateur approuve les nouveaux comptes ou n’approuvez pas les utilisateurs jusqu'à ce qu’ils visitent un lien envoyé à l’adresse de messagerie fournie lors de leur inscription. De même, certains sites peuvent verrouiller les utilisateurs jusqu'à ce qu’un administrateur réinitialise leur état, tandis que les autres sites envoyer un e-mail à l’utilisateur bloqué avec une URL qu’ils peuvent visiter pour déverrouiller son compte.

Ce didacticiel montre comment créer une page web pour les administrateurs gérer les utilisateurs verrouillés et approuvé les États. Nous verrons également comment approuver les nouveaux utilisateurs uniquement une fois qu’ils ont vérifié leur e-mail.

## <a name="step-1-managing-users-locked-out-and-approved-statuses"></a>Étape 1 : Gestion des utilisateurs verrouillés et approuvé les États

Dans le <a id="Tutorial12"> </a> [ *création d’une Interface pour sélectionner un compte d’utilisateur de nombreux* ](building-an-interface-to-select-one-user-account-from-many-vb.md) didacticiel, nous avons également élaboré une page qui répertorié chaque compte d’utilisateur dans un paginée filtré GridView. La grille répertorie chaque nom de l’utilisateur et e-mail, leurs États approuvées et verrouillés, qu’ils se trouvent actuellement en ligne et des commentaires sur l’utilisateur. Pour gérer les utilisateurs approuvés et verrouillé de statuts, nous pourrions éventuellement effectuer cette grille modifiable. Pour modifier le statut d’un utilisateur, l’administrateur serait d’abord localiser le compte d’utilisateur et puis modifiez la ligne correspondante de GridView, activant ou désactivant la case à cocher approuvée. Nous pourrions également gérer les États approuvées et verrouillés via une page ASP.NET distincte.

Pour ce didacticiel, nous allons utiliser deux pages ASP.NET : `ManageUsers.aspx` et `UserInformation.aspx`. L’idée ici est que `ManageUsers.aspx` répertorie les comptes d’utilisateur dans le système, tandis que `UserInformation.aspx` permet à l’administrateur gérer les États approuvées et verrouillés pour un utilisateur spécifique. Notre première chose consiste à augmenter le contrôle GridView dans `ManageUsers.aspx` pour inclure un HyperLinkField, qui effectue le rendu en tant que colonne de liens. Nous voulons chaque lien pour pointer vers `UserInformation.aspx?user=UserName`, où *nom d’utilisateur* est le nom de l’utilisateur à modifier.

> [!NOTE]
> Si vous avez téléchargé le code pour le <a id="Tutorial13"> </a> [ *récupération et modification des mots de passe* ](recovering-and-changing-passwords-vb.md) didacticiel, vous avez peut-être remarqué que le `ManageUsers.aspx` page contient déjà un ensemble de » Gérer les liens » et le `UserInformation.aspx` page fournit une interface pour la modification de mot de passe de l’utilisateur sélectionné. J’ai décidé de ne pas répliquer cette fonctionnalité dans le code associé à ce didacticiel, car cela a fonctionné en contournant l’API d’appartenance et fonctionne directement avec la base de données SQL Server pour modifier un mot de passe utilisateur. Ce didacticiel commence à partir de zéro avec le `UserInformation.aspx` page.


### <a name="adding-manage-links-to-theuseraccountsgridview"></a>Ajout de « gérer » des liens vers les`UserAccounts`GridView

Ouvrez le `ManageUsers.aspx` page et ajoutez un HyperLinkField à la `UserAccounts` GridView. Définir le HyperLinkField `Text` propriété à « Gérer » et ses `DataNavigateUrlFields` et `DataNavigateUrlFormatString` propriétés à `UserName` et « UserInformation.aspx?user={0}», respectivement. Ces paramètres configurent le HyperLinkField de sorte que tous les liens hypertexte affichent le texte « Gérer », mais chaque lien transmet le *nom d’utilisateur* valeur dans la chaîne de requête.

Après avoir ajouté le HyperLinkField GridView, prenez un moment pour afficher la `ManageUsers.aspx` page via un navigateur. Comme le montre la Figure 1, chaque ligne GridView inclut désormais un lien « Gérer ». Le lien « Gérer » de Bruce pointe vers `UserInformation.aspx?user=Bruce`, tandis que le lien « Gérer » de Dave pointe vers `UserInformation.aspx?user=Dave`.


[![Ajoute le HyperLinkField un](unlocking-and-approving-user-accounts-vb/_static/image2.png)](unlocking-and-approving-user-accounts-vb/_static/image1.png)

**Figure 1**: le HyperLinkField ajoute un lien « Gérer » pour chaque compte d’utilisateur ([cliquez pour afficher l’image en taille réelle](unlocking-and-approving-user-accounts-vb/_static/image3.png))


Nous créer l’interface utilisateur et de code pour le `UserInformation.aspx` page dans un moment, mais le premier nous allons parler sur comment changer par programmation un utilisateur verrouillé et approuvé les États. Le [ `MembershipUser` classe](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx) a [ `IsLockedOut` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx) et [ `IsApproved` propriétés](https://msdn.microsoft.com/library/system.web.security.membershipuser.isapproved.aspx). Le `IsLockedOut` propriété est en lecture seule. Il n’existe aucun mécanisme pour par programmation verrouiller un utilisateur ; Pour déverrouiller un utilisateur, utilisez la `MembershipUser` la classe [ `UnlockUser` méthode](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx). Le `IsApproved` propriété est accessible en lecture et en écriture. Pour enregistrer les modifications à cette propriété, nous devons appeler le `Membership` la classe [ `UpdateUser` méthode](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx), en passant le texte modifié `MembershipUser` objet.

Étant donné que le `IsApproved` propriété est accessible en lecture et en écriture, un contrôle de case à cocher est probablement le meilleur élément d’interface utilisateur pour la configuration de cette propriété. Toutefois, une case à cocher ne fonctionnera pas pour le `IsLockedOut` propriété, car un administrateur ne peut pas verrouiller un utilisateur, elle peut uniquement déverrouiller un utilisateur. Une interface utilisateur appropriée pour le `IsLockedOut` propriété est un bouton qui, lorsque vous cliquez dessus, déverrouille le compte d’utilisateur. Ce bouton doit être activé uniquement si l’utilisateur est verrouillé.

### <a name="creating-theuserinformationaspxpage"></a>Création de le`UserInformation.aspx`Page

Nous sommes maintenant prêts à implémenter l’interface utilisateur dans `UserInformation.aspx`. Ouvrez cette page et ajoutez les contrôles Web suivants :

- Contrôler un lien hypertexte qui, en un clic, retourne l’administrateur de le `ManageUsers.aspx` page.
- Un contrôle Web Label pour afficher le nom de l’utilisateur sélectionné. Valeur de cette étiquette `ID` à `UserNameLabel` et d’effacer les sa propriété Text.
- Un contrôle de case à cocher nommé `IsApproved`. Définissez ses `AutoPostBack` propriété `True`.
- Un contrôle d’étiquette pour l’affichage de l’utilisateur du dernier verrouillage de date. Nommez cette étiquette `LastLockedOutDateLabel` et effacer son `Text` propriété.
- Un bouton pour le déverrouillage de l’utilisateur. Nommez ce bouton `UnlockUserButton` et définissez son `Text` propriété « Déverrouiller User ».
- Un contrôle Label pour afficher les messages d’état, telles que « statut approuvé de l’utilisateur a été mis à jour. » Nommez ce contrôle `StatusMessage`, désactivez out son `Text` propriété et définissez son `CssClass` propriété `Important`. (Le `Important` classe CSS est défini dans le `Styles.css` fichier de feuille de style ; il affiche le texte correspondant dans une grande police rouge.)

Après avoir ajouté ces contrôles, la vue de conception dans Visual Studio doit ressembler à la capture d’écran de la Figure 2.


[![Créer l’Interface utilisateur pour UserInformation.aspx](unlocking-and-approving-user-accounts-vb/_static/image5.png)](unlocking-and-approving-user-accounts-vb/_static/image4.png)

**Figure 2**: créer l’Interface utilisateur pour `UserInformation.aspx` ([cliquez pour afficher l’image en taille réelle](unlocking-and-approving-user-accounts-vb/_static/image6.png))


Avec l’interface utilisateur complète, la tâche suivante consiste à définir le `IsApproved` case à cocher et autres contrôles selon les informations de l’utilisateur sélectionné. Créer un gestionnaire d’événements de la page `Load` événement et ajoutez le code suivant :

[!code-vb[Main](unlocking-and-approving-user-accounts-vb/samples/sample1.vb)]

Le code ci-dessus commence en veillant à ce qu’il s’agit de la première visite sur la page et pas une publication suivante. Elle lit ensuite le nom d’utilisateur transmis via le `user` champ querystring et récupère des informations sur ce compte d’utilisateur par le biais de la `Membership.GetUser(username)` (méthode). Si aucun nom d’utilisateur a été fourni par la chaîne de requête, ou si l’utilisateur spécifié est introuvable, l’administrateur est renvoyé à la `ManageUsers.aspx` page.

Le `MembershipUser` l’objet `UserName` valeur est ensuite affichée dans le `UserNameLabel` et le `IsApproved` case à cocher est activée en fonction de la `IsApproved` valeur de propriété.

Le `MembershipUser` l’objet [ `LastLockoutDate` propriété](https://msdn.microsoft.com/library/system.web.security.membershipuser.lastlockoutdate.aspx) retourne un `DateTime` valeur qui indique quand l’utilisateur a été verrouillé. Si l’utilisateur n’a jamais été verrouillé, la valeur retournée varie selon le fournisseur d’appartenances. Lorsqu’un nouveau compte est créé, le `SqlMembershipProvider` définit le `aspnet_Membership` la table `LastLockoutDate` champ `1754-01-01 12:00:00 AM`. Le code ci-dessus affiche une chaîne vide dans le `LastLockoutDateLabel` si le `LastLockoutDate` propriété se produit avant l’année 2000 ; sinon, la partie date de la `LastLockoutDate` propriété s’affiche dans l’étiquette. Le `UnlockUserButton`de `Enabled` propriété est définie à l’utilisateur verrouillé d’état, ce qui signifie que ce bouton sera activé uniquement si l’utilisateur est verrouillé.

Prenez un moment pour tester le `UserInformation.aspx` page via un navigateur. Vous, bien sûr, devez commencent à `ManageUsers.aspx` et sélectionnez un compte d’utilisateur à gérer. Fonction arrivant à `UserInformation.aspx`, notez que le `IsApproved` case à cocher est activée uniquement si l’utilisateur est approuvé. Si l’utilisateur a déjà été verrouillé, leur dernier verrouillé de date s’affiche. Le bouton de déverrouiller un utilisateur est activé uniquement si l’utilisateur est actuellement verrouillé. Activant ou désactivant la `IsApproved` case à cocher ou en cliquant sur le bouton de déverrouiller un utilisateur entraîne une publication, mais aucun modifications ne sont apportées au compte d’utilisateur, car il nous faut encore à créer des gestionnaires d’événements pour ces événements.

Revenez à Visual Studio et créer des gestionnaires d’événements pour le `IsApproved` la case à cocher `CheckedChanged` événement et le `UnlockUser` du bouton `Click` événement. Dans le `CheckedChanged` Gestionnaire d’événements, définissez l’utilisateur `IsApproved` propriété le `Checked` propriété de la case à cocher, puis enregistrez les modifications apportées via un appel à `Membership.UpdateUser`. Dans le `Click` Gestionnaire d’événements, il vous suffit d’appel la `MembershipUser` l’objet `UnlockUser` (méthode). Dans les deux gestionnaires d’événements, afficher un message approprié dans le `StatusMessage` étiquette.

[!code-vb[Main](unlocking-and-approving-user-accounts-vb/samples/sample2.vb)]

### <a name="testing-theuserinformationaspxpage"></a>Tester le`UserInformation.aspx`Page

Ces gestionnaires d’événements en place, visitez la page et non approuvées un utilisateur. Comme le montre la Figure 3, vous devez voir un bref message sur la page indiquant que l’utilisateur `IsApproved` propriété a été correctement modifiée.


[![Chris a été non approuvé](unlocking-and-approving-user-accounts-vb/_static/image8.png)](unlocking-and-approving-user-accounts-vb/_static/image7.png)

**Figure 3**: Chris a été non approuvé ([cliquez pour afficher l’image en taille réelle](unlocking-and-approving-user-accounts-vb/_static/image9.png))


Ensuite, déconnexion et essayez de vous connecter en tant que l’utilisateur dont le compte a été simplement non approuvées. Étant donné que l’utilisateur n’est pas approuvé, ils ne peuvent pas se connecter. Par défaut, le contrôle de connexion affiche le message même si l’utilisateur ne peut pas ouvrir une session, quelle que soit la raison. Mais, dans le <a id="Tutorial6"> </a> [ *validation utilisateur informations d’identification par rapport à l’appartenance utilisateur Store* ](../membership/validating-user-credentials-against-the-membership-user-store-vb.md) didacticiel, nous avons recherché à améliorer le contrôle de connexion pour afficher un message plus approprié. Comme le montre la Figure 4, Chris voit un message expliquant qu’il ne peut pas se connecter, car son compte n’est pas encore approuvé.


[![Chris impossible, car son compte de connexion est non approuvé](unlocking-and-approving-user-accounts-vb/_static/image11.png)](unlocking-and-approving-user-accounts-vb/_static/image10.png)

**Figure 4**: Chris impossible, car son compte de connexion est non approuvé ([cliquez pour afficher l’image en taille réelle](unlocking-and-approving-user-accounts-vb/_static/image12.png))


Pour tester la fonctionnalité verrouillée, tentez de vous connecter en tant qu’un utilisateur approuvé, mais utiliser un mot de passe incorrect. Répétez ce processus, le nombre de fois jusqu'à ce que le compte d’utilisateur a été verrouillé nécessaires. Le contrôle de connexion a été également mis à jour pour afficher une personnalisée du message si vous tentez de vous connecter à partir d’un compte verrouillé. Vous savez qu’un compte a été verrouillé une fois que vous commencer à voir le message suivant à la page de connexion : « votre compte a été verrouillé en raison de trop de tentatives de connexion non valide. Contactez l’administrateur pour que votre compte déverrouillé. »

Retour à la `ManageUsers.aspx` page et cliquez sur le lien gérer pour l’utilisateur verrouillé. Comme le montre la Figure 5, vous devez voir une valeur dans le `LastLockedOutDateLabel` le bouton de déverrouiller un utilisateur doit être activé. Cliquez sur le bouton de déverrouiller un utilisateur pour déverrouiller le compte d’utilisateur. Une fois que vous avez déverrouillé l’utilisateur, ils seront en mesure de se reconnecter.


[![Dave a été verrouillé pour le système](unlocking-and-approving-user-accounts-vb/_static/image14.png)](unlocking-and-approving-user-accounts-vb/_static/image13.png)

**Figure 5**: Dave a été verrouillé hors du système ([cliquez pour afficher l’image en taille réelle](unlocking-and-approving-user-accounts-vb/_static/image15.png))


## <a name="step-2-specifying-new-users-approved-status"></a>Étape 2 : Spécification des nouveaux utilisateurs approuvés état

L’état approuvé est utile dans les scénarios où vous souhaitez qu’une action à effectuer avant un nouvel utilisateur est en mesure de se connecter et accéder aux fonctionnalités spécifiques à l’utilisateur du site. Par exemple, vous exécutez peut-être un site de Web privé où toutes les pages, sauf pour les pages d’inscription et la connexion, sont uniquement accessibles aux utilisateurs authentifiés. Mais que se passe-t-il si un étranger atteint votre site Web, recherche de la page d’abonnement et crée un compte ? Pour éviter ce problème, vous pouvez déplacer la page d’abonnement à un `Administration` dossier et nécessitent qu’un administrateur crée manuellement chaque compte. Vous pouvez également autoriser tout le monde à l’inscription, mais interdire l’accès de site jusqu'à ce qu’un administrateur approuve le compte d’utilisateur.

Par défaut, le contrôle CreateUserWizard approuve les nouveaux comptes. Vous pouvez configurer ce comportement à l’aide du contrôle [ `DisableCreatedUser` propriété](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx). Définissez cette propriété sur `True` de ne pas approuver les nouveaux comptes d’utilisateur.

> [!NOTE]
> Par défaut le contrôle CreateUserWizard enregistre automatiquement sur le nouveau compte d’utilisateur. Ce comportement est déterminé par le contrôle [ `LoginCreatedUser` propriété](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx). Étant donné que les utilisateurs non approuvés ne peuvent pas se connecter au site, lorsque `DisableCreatedUser` est `True` le nouveau compte d’utilisateur n’est pas connecté au site, quel que soit la valeur de la `LoginCreatedUser` propriété.


Si vous créez par programme des nouveaux comptes d’utilisateur par le biais de la `Membership.CreateUser` (méthode), pour créer un compte d’utilisateur non approuvé utilisez une des surcharges qui acceptent le nouvel utilisateur `IsApproved` valeur de propriété comme un paramètre d’entrée.

## <a name="step-3-approving-users-by-verifying-their-email-address"></a>Étape 3 : Approuver les utilisateurs en vérifiant leur adresse de messagerie

De nombreux sites Web qui prennent en charge les comptes d’utilisateur n’approuvent pas les nouveaux utilisateurs jusqu'à ce qu’elles vérifient l’adresse de messagerie qu’ils fournis lors de l’inscription. Ce processus de vérification est couramment utilisé pour contrecarrer les robots, les expéditeurs de courrier indésirable et les autres ne'er-do-wells car elle requiert une adresse électronique unique et vérifiées et ajoute une étape supplémentaire dans le processus d’inscription. Avec ce modèle, quand un nouvel utilisateur s’inscrit, elles sont envoyées un message électronique qui inclut un lien vers une page de vérification. En vous rendant sur le lien, l’utilisateur s’est avérée qu’ils reçu l’e-mail et que l’adresse de messagerie fournie n’est donc valide. La page de vérification est responsable de l’approbation de l’utilisateur. Cela peut se produire d’automatiquement, ainsi l’approbation n’importe quel utilisateur qui a atteint cette page, ou uniquement une fois que l’utilisateur fournit des informations supplémentaires, comme un [CAPTCHA](http://en.wikipedia.org/wiki/Captcha).

Pour prendre en charge ce flux de travail, nous devons tout d’abord mettre à jour de la page de création de compte afin que les nouveaux utilisateurs sont non approuvées. Ouvrez le `EnhancedCreateUserWizard.aspx` page dans le `Membership` dossier et appliquez le contrôle CreateUserWizard `DisableCreatedUser` propriété `True`.

Ensuite, nous devons configurer le contrôle CreateUserWizard pour envoyer un e-mail au nouvel utilisateur avec des instructions sur la façon de vérifier son compte. En particulier, nous inclurons un lien dans l’e-mail pour le `Verification.aspx` page (dont nous avons encore à créer), en passant le nouvel utilisateur `UserId` via la chaîne de requête. Le `Verification.aspx` page Rechercher l’utilisateur spécifié et les marquer approuvée.

### <a name="sending-a-verification-email-to-new-users"></a>Envoyer un E-mail de vérification à de nouveaux utilisateurs

Pour envoyer un e-mail à partir du contrôle CreateUserWizard, configurer ses `MailDefinition` propriété correctement. Comme indiqué dans le <a id="Tutorial13"> </a> [didacticiel précédent](recovering-and-changing-passwords-vb.md), les contrôles ChangePassword et PasswordRecovery incluent un [ `MailDefinition` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) qui fonctionne de la même manière que CreateUserWizard du contrôle.

> [!NOTE]
> Pour utiliser le `MailDefinition` options de propriété, vous devez spécifier la remise du courrier dans `Web.config`. Pour plus d’informations, consultez [envoi de courrier électronique dans ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).


Commencez par créer un modèle d’e-mail nommé `CreateUserWizard.txt` dans le `EmailTemplates` dossier. Pour le modèle, utilisez le texte suivant :

[!code-aspx[Main](unlocking-and-approving-user-accounts-vb/samples/sample3.aspx)]

Définir le `MailDefinition`de `BodyFileName` propriété à « ~ / EmailTemplates/CreateUserWizard.txt » et son `Subject` propriété à « Bienvenue dans mon site Web ! Activez votre compte. »

Notez que le `CreateUserWizard.txt` modèle de message électronique inclut un `<%VerificationUrl%>` espace réservé. C’est là où l’URL pour le `Verification.aspx` page est placée. CreateUserWizard remplace automatiquement le `<%UserName%>` et `<%Password%>` des espaces réservés avec le nouveau compte nom d’utilisateur et mot de passe, mais il n’intègre aucune `<%VerificationUrl%>` espace réservé. Nous devons remplacer manuellement par l’URL de vérification appropriées.

Pour ce faire, créez un gestionnaire d’événements pour le CreateUserWizard [ `SendingMail` événement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.sendingmail.aspx) et ajoutez le code suivant :

[!code-vb[Main](unlocking-and-approving-user-accounts-vb/samples/sample4.vb)]

Le `SendingMail` événement est déclenché après le `CreatedUser` événement, ce qui signifie qu’au moment où le Gestionnaire d’événements ci-dessus exécute le nouvel utilisateur compte a déjà été créé. Nous pouvons accéder à du nouvel utilisateur `UserId` valeur en appelant le `Membership.GetUser` méthode, en passant le `UserName` entré dans le contrôle CreateUserWizard. Ensuite, l’URL de vérification est formée. L’instruction `Request.Url.GetLeftPart(UriPartial.Authority)` retourne la `http://yourserver.com` partie de l’URL ; `Request.ApplicationPath` renvoie le chemin où l’application est associé à une racine. La vérification d’URL est alors définie en tant que `Verification.aspx?ID=userId`. Ces deux chaînes sont ensuite concaténées pour former l’URL complète. Enfin, le corps du message électronique (`e.Message.Body`) a toutes les occurrences de `<%VerificationUrl%>` remplacé par l’URL complète.

L’effet net est que les nouveaux utilisateurs sont non approuvés, ce qui signifie qu’ils ne peuvent pas se connecter au site. En outre, ils sont envoyés automatiquement un e-mail contenant un lien vers l’URL de vérification (voir Figure 6).


[![Le nouvel utilisateur reçoit un E-mail contenant un lien vers l’URL de vérification](unlocking-and-approving-user-accounts-vb/_static/image17.png)](unlocking-and-approving-user-accounts-vb/_static/image16.png)

**Figure 6**: le nouvel utilisateur reçoit un E-mail contenant un lien vers l’URL de vérification ([cliquez pour afficher l’image en taille réelle](unlocking-and-approving-user-accounts-vb/_static/image18.png))


> [!NOTE]
> Étape de CreateUserWizard par défaut du contrôle CreateUserWizard affiche un message informant l’utilisateur que son compte a été créé et affiche un bouton Continuer. Cela amène l’utilisateur vers l’URL spécifiée par le contrôle `ContinueDestinationPageUrl` propriété. CreateUserWizard dans `EnhancedCreateUserWizard.aspx` est configuré pour envoyer les nouveaux utilisateurs à la `~/Membership/AdditionalUserInfo.aspx`, qui invite l’utilisateur pour leur ville natale, l’URL de la page d’accueil et signature. Étant donné que ces informations ne peuvent être ajoutées par utilisateurs connectés, il est judicieux pour mettre à jour de cette propriété pour envoyer les utilisateurs vers la page d’accueil du site (`~/Default.aspx`). En outre, le `EnhancedCreateUserWizard.aspx` page ou l’étape CreateUserWizard doit être augmentée pour informer l’utilisateur qu’ils ont reçu un e-mail de vérification et son compte n’est pas activé jusqu'à ce qu’ils suivent les instructions de cet e-mail. Je laisse ces modifications en guise d’exercice pour le lecteur.


### <a name="creating-the-verification-page"></a>Création de la Page de vérification

La dernière tâche consiste à créer le `Verification.aspx` page. Ajouter cette page dans le dossier racine, en association avec le `Site.master` page maître. Comme nous l’avons fait avec la plupart des pages de contenu précédents ajoutés sur le site, supprimez le contrôle de contenu qui référence le `LoginContent` ContentPlaceHolder afin que la page de contenu utilise la page maître de contenu par défaut.

Ajouter un contrôle étiquette à la `Verification.aspx` , définissez son `ID` à `StatusMessage` et d’effacer les sa propriété text. Ensuite, créez le `Page_Load` Gestionnaire d’événements et ajoutez le code suivant :

[!code-vb[Main](unlocking-and-approving-user-accounts-vb/samples/sample5.vb)]

La majeure partie du code ci-dessus vérifie que le nom d’utilisateur fourni par la chaîne de requête existe, qu’il est valide `Guid` valeur, et qu’il fait référence à un compte d’utilisateur existant. Si toutes ces vérifications réussissent, le compte d’utilisateur est approuvé ; Sinon, un message d’état approprié est affiché.

La figure 7 illustre le `Verification.aspx` page quand consultées via un navigateur.


[![Le nouveau compte d’utilisateur est maintenant approuvées](unlocking-and-approving-user-accounts-vb/_static/image20.png)](unlocking-and-approving-user-accounts-vb/_static/image19.png)

**Figure 7**: compte le nouveau d’utilisateur est maintenant approuvées ([cliquez pour afficher l’image en taille réelle](unlocking-and-approving-user-accounts-vb/_static/image21.png))


## <a name="summary"></a>Récapitulatif

Tous les comptes d’utilisateur d’appartenance ont deux États qui déterminent si l’utilisateur peut se connecter au site : `IsLockedOut` et `IsApproved`. Ces deux propriétés doivent être `True` pour l’utilisateur de connexion.

L’utilisateur de verrouillage de statut est utilisé comme mesure de sécurité pour réduire la probabilité d’un pirate avec rupture dans un site via des méthodes de force brute. Plus précisément, un utilisateur est verrouillé s’il existe un certain nombre de tentatives de connexion non valide dans une certaine fenêtre de temps. Ces limites sont configurables via les paramètres du fournisseur d’appartenances dans `Web.config`.

L’état approuvé est couramment utilisé comme un moyen pour interdire les nouveaux utilisateurs de se connecter jusqu'à ce qu’une action a été dépassé. Par exemple le site nécessite que des comptes de tout d’abord être approuvée par l’administrateur ou, comme nous l’avons vu à l’étape 3, en vérifiant son adresse e-mail.

Bonne programmation !

### <a name="about-the-author"></a>À propos de l’auteur

Scott Mitchell, auteur de plusieurs livres sur ASP/ASP.NET et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est *[SAM animer vous-même ASP.NET 2.0 des dernières 24 heures](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott peut être atteint à [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou via son blog à [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements particuliers à...

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](recovering-and-changing-passwords-vb.md)
