---
uid: web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-vb
title: Récupération et changement des mots de passe (VB) | Microsoft Docs
author: rick-anderson
description: ASP.NET inclut deux contrôles Web qui contribuent à la récupération et changement des mots de passe. Le contrôle PasswordRecovery Active un visiteur récupérer son pa perdu...
ms.author: aspnetcontent
ms.date: 04/01/2008
ms.assetid: f9adcb5d-6d70-4885-a3bf-ed95efb4da1a
msc.legacyurl: /web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-vb
msc.type: authoredcontent
ms.openlocfilehash: bb52204b58bcdbb3d03390868e52b8a6f74df5e4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822905"
---
<a name="recovering-and-changing-passwords-vb"></a>Récupération et changement des mots de passe (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/VB.13.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial13_ChangingPasswords_vb.pdf)

> ASP.NET inclut deux contrôles Web qui contribuent à la récupération et changement des mots de passe. Le contrôle PasswordRecovery permet un visiteur récupérer son mot de passe perdu. Le contrôle ChangePassword permet à l’utilisateur à mettre à jour son mot de passe. Comme les autres contrôles Web associées à la connexion, nous avons vu tout au long de cette série de didacticiels, le PasswordRecovery et ChangePassword contrôles fonctionnent avec l’infrastructure de l’appartenance dans les coulisses pour réinitialiser ou modifier les mots de passe utilisateur.


## <a name="introduction"></a>Introduction

Entre les sites Web pour mon bank électricité, société de téléphone, les comptes de messagerie et portails web personnalisés, I, comme la plupart des gens, ont des dizaines de différents mots de passe à mémoriser. Avec les informations d’identification autant de mémoriser les extraits de nos jours, il n’est pas rare que les personnes oublient leur mot de passe. Pour éviter cela, sites Web qui offrent des comptes d’utilisateur doivent inclure un moyen pour un utilisateur de récupérer son mot de passe. Ce processus implique généralement la génération d’un nouveau mot de passe aléatoire et l’envoient à l’adresse de messagerie de l’utilisateur sur le fichier. Après avoir reçu leur nouveau mot de passe la plupart des utilisateurs revenez sur le site et modifier leur mot de passe de celui généré de manière aléatoire à un autre plus facile à mémoriser.

ASP.NET inclut deux contrôles Web qui contribuent à la récupération et changement des mots de passe. Le contrôle PasswordRecovery permet un visiteur récupérer son mot de passe perdu. Le contrôle ChangePassword permet à l’utilisateur à mettre à jour son mot de passe. Comme les autres contrôles Web associées à la connexion, nous avons vu tout au long de cette série de didacticiels, le PasswordRecovery et ChangePassword contrôles fonctionnent avec l’infrastructure de l’appartenance dans les coulisses pour réinitialiser ou modifier les mots de passe utilisateur.

Dans ce didacticiel, nous allons examiner à l’aide de ces deux contrôles. Nous verrons également comment changer par programme et de réinitialisation de mot de passe d’un utilisateur via le `MembershipUser` la classe `ChangePassword` et `ResetPassword` méthodes.

## <a name="step-1-helping-users-recover-lost-passwords"></a>Étape 1 : Aider les utilisateurs Recover perdu des mots de passe

Tous les sites Web qui prennent en charge les comptes d’utilisateurs avez besoin fournir aux utilisateurs un mécanisme de récupération de leurs mots de passe oubliés. La bonne nouvelle est que l’implémentation de ces fonctionnalités dans ASP.NET est un jeu d’enfant grâce au contrôle PasswordRecovery Web. Le contrôle PasswordRecovery restitue une interface qui invite l’utilisateur à son nom d’utilisateur et, si nécessaire, la réponse à la question de leur sécurité. Elle envoie par e-mail l’utilisateur son mot de passe.

> [!NOTE]
> Étant donné que les messages électroniques sont transmises en texte brut présente des risques de sécurité à l’envoi d’un mot de passe utilisateur par courrier électronique.


Le contrôle PasswordRecovery se compose de trois vues :

- **Nom d’utilisateur** -vous invite à entrer le visiteur pour son nom d’utilisateur. Il s’agit de la vue initiale.
- **Question**-affiche la question de sécurité et de nom d’utilisateur de l’utilisateur sous forme de texte, ainsi que d’une zone de texte pour l’utilisateur à entrer la réponse à sa question de sécurité.
- **Réussite**-affiche un message informant l’utilisateur son mot de passe a été envoyé par courrier électronique.

Affichent les vues et les actions effectuées par le contrôle PasswordRecovery varient selon les paramètres de configuration d’appartenance suivantes :

- `RequiresQuestionAndAnswer`
- `EnablePasswordRetrieval`
- `EnablePasswordReset`

L’infrastructure de l’appartenance `RequiresQuestionAndAnswer` paramètre indique si les utilisateurs doivent spécifier une question de sécurité et une réponse lors de l’inscription d’un compte. Comme expliqué dans la <a id="_msoanchor_1"> </a> [ *création de comptes utilisateur* ](../membership/creating-user-accounts-vb.md) if (didacticiel), `RequiresQuestionAndAnswer` est True (valeur par défaut), interface de CreateUserWizard inclut la zone de texte contrôles pour le nouvel utilisateur question de sécurité et de la réponse ; Si `RequiresQuestionAndAnswer` est False, aucune information n’est collectée. De même, si `RequiresQuestionAndAnswer` a la valeur True, les affichages de contrôle PasswordRecovery la Question afficher une fois que l’utilisateur a entré son nom d’utilisateur, le mot de passe est récupérée uniquement si l’utilisateur entre la réponse de sécurité appropriées. Si `RequiresQuestionAndAnswer` est False, toutefois, le contrôle PasswordRecovery déplace directement à partir de la vue du nom de la vue Opération réussie.

Une fois que l’utilisateur a fourni son nom d’utilisateur - ou une réponse de son nom d’utilisateur et de sécurité, si `RequiresQuestionAndAnswer` a la valeur True - le PasswordRecovery envoie par e-mail à l’utilisateur son mot de passe. Si le `EnablePasswordRetrieval` option est définie sur True, puis l’utilisateur est envoyé par courrier électronique leur mot de passe actuel. Si elle est définie sur False et `EnablePasswordReset` est définie sur True, le contrôle PasswordRecovery génère un nouveau mot de passe aléatoire pour l’utilisateur et ce nouveau mot de passe qui leur envoie un e-mail. Si les deux `EnablePasswordRetrieval` et `EnablePasswordReset` ont la valeur False, le contrôle PasswordRecovery lève une exception.

> [!NOTE]
> N’oubliez pas que le `SqlMembershipProvider` stocke les mots de passe utilisateurs dans un des trois formats : clair, Hashed (la valeur par défaut) ou chiffré. Le mécanisme de stockage utilisé dépend des paramètres de configuration de l’appartenance ; l’application de démonstration utilise le format de mot de passe haché. Lorsque vous utilisez le format de mot de passe haché la `EnablePasswordRetrieval` option doit être définie sur False, car le système ne peut pas déterminer le mot de passe réel de l’utilisateur à partir de la version hachée stockée dans la base de données.


La figure 1 illustre comment le PasswordRecovery interface et le comportement est influencée par la configuration de l’appartenance.


[![Le RequiresQuestionAndAnswer, EnablePasswordRetrieval et EnablePasswordReset influencent l’apparence et le comportement du contrôle PasswordRecovery](recovering-and-changing-passwords-vb/_static/image2.png)](recovering-and-changing-passwords-vb/_static/image1.png)

**Figure 1**: le `RequiresQuestionAndAnswer`, `EnablePasswordRetrieval`, et `EnablePasswordReset` influencer l’apparence et le comportement du contrôle PasswordRecovery ([cliquez pour afficher l’image en taille réelle](recovering-and-changing-passwords-vb/_static/image3.png))


> [!NOTE]
> Dans le <a id="_msoanchor_2"> </a> [ *création du schéma d’appartenance dans SQL Server* ](../membership/creating-the-membership-schema-in-sql-server-vb.md) didacticiel, nous avons configuré le fournisseur d’appartenances en définissant `RequiresQuestionAndAnswer` sur True, `EnablePasswordRetrieval` à False, et `EnablePasswordReset` sur True.


### <a name="using-the-passwordrecovery-control"></a>Utilisation du contrôle PasswordRecovery

Examinons à présent à l’aide du contrôle PasswordRecovery dans une page ASP.NET. Ouvrez `RecoverPassword.aspx` et faites glisser et déposez un contrôle PasswordRecovery à partir de la boîte à outils vers le concepteur ; définir son `ID` à `RecoverPwd`. Comme les contrôles Web de CreateUserWizard et de connexion, les vues du contrôle PasswordRecovery restituer une riche interface composite qui inclut des étiquettes, des zones de texte, des boutons et des contrôles de validation. Vous pouvez personnaliser l’apparence des vues par le biais des propriétés de style du contrôle ou en convertissant les vues dans des modèles. Je laisse cela en guise d’exercice pour les lecteurs que cela intéresse.

Lorsqu’un utilisateur visite cette page, elle sera entrez son nom d’utilisateur et cliquez sur le bouton Envoyer. Étant donné que nous avons défini le `RequiresQuestionAndAnswer` propriété sur True dans nos paramètres de configuration de l’appartenance, la PasswordRecovery contrôle va ensuite afficher la vue de la Question. Une fois que l’utilisateur entre son réponse de sécurité appropriées et clique sur Envoyer, le contrôle PasswordRecovery mettre à jour le mot de passe à un autre générée de manière aléatoire et ce mot de passe à l’adresse de messagerie sur le fichier de messagerie. Tout cela a été possible sans nous avoir à écrire une seule ligne de code !

Avant de tester cette page, il existe un dernier élément de configuration à ont tendance à : nous devons spécifier les paramètres de remise de courrier dans `Web.config`. Le contrôle PasswordRecovery s’appuie sur ces paramètres pour envoyer le courrier électronique.

La configuration de remise de courrier est spécifiée via la [ `<system.net>` élément](https://msdn.microsoft.com/library/6484zdc1.aspx)de [ `<mailSettings>` élément](https://msdn.microsoft.com/library/w355a94k.aspx). Utilisez le [ `<smtp>` élément](https://msdn.microsoft.com/library/ms164240.aspx) pour indiquer la méthode de remise et la valeur par défaut à partir de l’adresse. Le balisage suivant configure les paramètres de messagerie pour utiliser un serveur SMTP sur le réseau nommé `smtp.example.com` sur le port 25 et avec les informations d’identification de nom d’utilisateur/mot de passe du nom d’utilisateur et mot de passe.

> [!NOTE]
> `<system.net>` est un élément enfant de la racine `<configuration>` élément et un frère de `<system.web>`. Par conséquent, ne placez pas le `<system.net>` élément dans le `<system.web>` élément ; au lieu de cela, le placer au même niveau.


[!code-xml[Main](recovering-and-changing-passwords-vb/samples/sample1.xml)]

En plus d’utiliser un serveur SMTP sur le réseau, vous pouvez également spécifier un répertoire de collecte où envoyer des messages de messagerie doivent être déposés.

Une fois que vous avez configuré les paramètres SMTP, visitez le `RecoverPassword.aspx` page via un navigateur. Essayez tout d’abord entrer un nom d’utilisateur qui n’existe pas dans le magasin d’utilisateurs. Comme le montre la Figure 2, le contrôle PasswordRecovery affiche un message indiquant que les informations de l’utilisateur ne sont pas accessible. Le texte du message peut être personnalisé par le biais du contrôle [ `UserNameFailureText` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.usernamefailuretext.aspx).


[![Un Message d’erreur s’affiche si un nom d’utilisateur non valide est entré.](recovering-and-changing-passwords-vb/_static/image5.png)](recovering-and-changing-passwords-vb/_static/image4.png)

**Figure 2**: un Message d’erreur s’affiche si un nom d’utilisateur non valide est saisi ([cliquez pour afficher l’image en taille réelle](recovering-and-changing-passwords-vb/_static/image6.png))


À présent entrer un nom d’utilisateur. Utilisez le nom d’utilisateur d’un compte dans le système avec une adresse de messagerie que vous pouvez accéder à et dont la sécurité répondre que vous connaissez. Après avoir saisi le nom d’utilisateur et clique sur Envoyer, le contrôle PasswordRecovery affiche sa vue de la Question. Comme avec la vue du nom d’utilisateur, si vous entrez un incorrect répondre à ce contrôle de PasswordRecovery affiche un message d’erreur (voir Figure 3). Utilisez le [ `QuestionFailureText` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.questionfailuretext.aspx) pour personnaliser ce message d’erreur.


[![Un Message d’erreur s’affiche si l’utilisateur entre une réponse de sécurité non valide](recovering-and-changing-passwords-vb/_static/image8.png)](recovering-and-changing-passwords-vb/_static/image7.png)

**Figure 3**: un Message d’erreur s’affiche si l’utilisateur entre une réponse de sécurité non valide ([cliquez pour afficher l’image en taille réelle](recovering-and-changing-passwords-vb/_static/image9.png))


Enfin, entrez la réponse de sécurité appropriées et cliquez sur Envoyer. Dans les coulisses, le contrôle PasswordRecovery génère un mot de passe aléatoire, il attribue au compte d’utilisateur, envoie un message électronique pour informer l’utilisateur de son nouveau mot de passe (voir Figure 4), puis affiche la vue Opération réussie.


[![L’utilisateur reçoit un E-mail avec son nouveau mot de passe](recovering-and-changing-passwords-vb/_static/image11.png)](recovering-and-changing-passwords-vb/_static/image10.png)

**Figure 4**: l’utilisateur reçoit un E-mail avec son nouveau mot de passe ([cliquez pour afficher l’image en taille réelle](recovering-and-changing-passwords-vb/_static/image12.png))


### <a name="customizing-the-email"></a>Personnalisation de l’E-mail

L’adresse de messagerie par défaut envoyé par le contrôle PasswordRecovery est plutôt terne (voir Figure 4). Le message est envoyé à partir du compte spécifié dans le `<smtp>` l’élément `from` attribut avec le mot de passe de l’objet et le corps de texte brut :

Revenez au site et connectez-vous en utilisant les informations suivantes.

Nom d’utilisateur : *nom d’utilisateur*

mot de passe : *mot de passe*

Ce message peut être personnalisé par programmation via un gestionnaire d’événements pour le contrôle PasswordRecovery [ `SendingMail` événement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.sendingmail.aspx), ou de façon déclarative par la [ `MailDefinition` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.maildefinition.aspx). Examinons ces deux options.

Le `SendingMail` événement est déclenché juste avant que le message électronique est envoyé et est notre dernière chance pour ajuster par programmation le message électronique. Lorsque cet événement est déclenché, le Gestionnaire d’événements reçoit un objet de type [ `MailMessageEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.mailmessageeventargs.aspx), dont `Message` propriété contient une référence à l’adresse e-mail sur le point d’être envoyé.

Créer un gestionnaire d’événements pour le `SendingMail` événement et ajoutez le code suivant, qui ajoute par programmation `webmaster@example.com` à la liste CC.

[!code-vb[Main](recovering-and-changing-passwords-vb/samples/sample2.vb)]

Le message électronique peut également être configuré via moyen déclaratif. Le PasswordRecovery `MailDefinition` propriété est un objet de type [ `MailDefinition` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.aspx). Le `MailDefinition` classe offre une multitude de propriétés liées à la messagerie, y compris `From`, `CC`, `Priority`, `Subject`, `IsBodyHtml`, `BodyFileName`et d’autres. Pour commencer, définissez le [ `Subject` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.subject.aspx) un nom plus descriptif que celui utilisé par défaut (mot de passe), telles que votre mot de passe a été réinitialisé...

Pour personnaliser le corps du message électronique, que nous devons créer un fichier de modèle d’e-mail distinct qui contient du corps. Commencez par créer un nouveau dossier dans le site Web nommé `EmailTemplates`. Ensuite, ajoutez un nouveau fichier texte dans ce dossier nommé `PasswordRecovery.txt` et ajoutez le contenu suivant :

[!code-aspx[Main](recovering-and-changing-passwords-vb/samples/sample3.aspx)]

Notez l’utilisation des espaces réservés `<%UserName%>` et `<%Password%>`. Le contrôle PasswordRecovery remplace automatiquement ces deux espaces réservés par nom d’utilisateur et mot de passe récupéré avant d’envoyer l’e-mail de l’utilisateur.

Enfin, pointez le `MailDefinition`de [ `BodyFileName` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) pour le modèle de message électronique que nous venons de créer (`~/EmailTemplates/PasswordRecovery.txt`).

Une fois ces modifications revisiter le `RecoverPassword.aspx` page et entrez votre nom d’utilisateur et de sécurité la réponse. Vous recevez doit un e-mail qui ressemble à celui de la Figure 5. Notez que `webmaster@example.com` a été CC serait et que l’objet et le corps ont été mis à jour.


[![L’objet, le corps et la liste CC ont été mis à jour](recovering-and-changing-passwords-vb/_static/image14.png)](recovering-and-changing-passwords-vb/_static/image13.png)

**Figure 5**: l’objet, le corps et CC liste ont été mis à jour ([cliquez pour afficher l’image en taille réelle](recovering-and-changing-passwords-vb/_static/image15.png))


Pour envoyer un e-mail au format HTML défini [ `IsBodyHtml` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.isbodyhtml.aspx) True (la valeur par défaut est False) et la mise à jour le modèle d’e-mail pour inclure du code HTML.

Le `MailDefinition` propriété n’est pas propre à la classe PasswordRecovery. Comme nous le verrons à l’étape 2, le contrôle ChangePassword offre également une `MailDefinition` propriété. En outre, le contrôle CreateUserWizard inclut une telle propriété que vous pouvez configurer pour envoyer automatiquement un e-mail de bienvenue aux nouveaux utilisateurs.

> [!NOTE]
> Actuellement il n’y aucun lien dans le volet de navigation gauche pour atteindre le `RecoverPassword.aspx` page. Un utilisateur sera uniquement intéressée par un consultant cette page si elle n’a pas pu se connecter correctement le site. Par conséquent, mettre à jour le `Login.aspx` page à inclure un lien vers le `RecoverPassword.aspx` page.


### <a name="programmatically-resetting-a-users-password"></a>Réinitialisation par programmation un mot de passe utilisateur

La réinitialisation de mot de passe d’un utilisateur le PasswordRecovery lorsque des appels de contrôle le `MembershipUser` l’objet [ `ResetPassword` méthode](https://msdn.microsoft.com/library/system.web.security.membershipuser.resetpassword.aspx). Cette méthode a deux surcharges :

- **[`ResetPassword`](https://msdn.microsoft.com/library/d94bdzz2.aspx)** -Réinitialise le mot de passe d’un utilisateur. Utilisez cette surcharge si `RequiresQuestionAndAnswer` a la valeur False.
- **[`ResetPassword(securityAnswer)`](https://msdn.microsoft.com/library/d90zte4w.aspx)** -réinitialise uniquement si de mot de passe d’un utilisateur fourni *securityAnswer* est correct. Utilisez cette surcharge si `RequiresQuestionAndAnswer` a la valeur True.

Les deux surcharges retournent le mot de passe de nouveau, générée de manière aléatoire.

Comme avec les autres méthodes dans l’infrastructure de l’appartenance, la `ResetPassword` méthode délègue au fournisseur configuré. Le `SqlMembershipProvider` appelle le `aspnet_Membership_ResetPassword` procédure stockée, en passant dans le nom d’utilisateur, le nouveau mot de passe et la réponse de mot de passe fourni, parmi les autres champs. La procédure stockée permet de s’assurer que la réponse de mot de passe correspond et puis met à jour le mot de passe.

Quelques remarques sur l’implémentation de bas niveau :

- Un utilisateur verrouillé ne peut pas réinitialiser son mot de passe. Toutefois, un utilisateur non approuvé peut. Nous aborderons le verrouillé et approuvé les États plus en détail dans le <a id="_msoanchor_3"> </a> [ *Unlocking et approbation d’un utilisateur* ](unlocking-and-approving-user-accounts-vb.md) didacticiel de comptes.
- Si la réponse de mot de passe est incorrecte, le nombre de tentatives de réponse de mot de passe de l’utilisateur est incrémenté. Si un nombre spécifié de tentatives de réponse de sécurité non valide se produire dans une fenêtre de temps spécifié, l’utilisateur est verrouillé.

### <a name="a-word-on-how-the-random-passwords-are-generated"></a>Un mot sur la façon dont les mots de passe aléatoires sont générés.

Les mots de passe généré aléatoirement indiqués dans les messages électroniques dans les Figures 4 et 5 sont créés par la classe Membership [ `GeneratePassword` méthode](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx). Cette méthode accepte deux paramètres d’entrée d’entier - *longueur* et *numberOfNonAlphanumericCharacters* - et retourne une chaîne au moins *longueur* caractères long avec à la moins *numberOfNonAlphanumericCharacters* nombre de caractères non alphanumériques. Lorsque cette méthode est appelée à partir de dans les classes d’appartenance ou les contrôles Web associées à la connexion, les valeurs pour ces deux paramètres sont déterminées par la configuration de l’appartenance `MinRequiredPasswordLength` et `MinRequiredNonalphanumericCharacters` propriétés, ce qui nous la valeur 7 et 1, respectivement.

Le `GeneratePassword` méthode utilise un générateur de nombres aléatoires cryptographiquement fort pour vous assurer qu’il n’y a aucun écart dans les caractères aléatoires sont sélectionnés. En outre, `GeneratePassword` est `Public`, ce qui signifie que vous pouvez l’utiliser directement à partir de votre application ASP.NET si vous avez besoin générer des chaînes aléatoires ou les mots de passe.

> [!NOTE]
> Le `SqlMembershipProvider` classe génère toujours un mot de passe aléatoire au moins 14 caractères, par conséquent, si `MinRequiredPasswordLength` est inférieur à 14 alors sa valeur est ignorée.


## <a name="step-2-changing-passwords"></a>Étape 2 : Modification des mots de passe

Les mots de passe généré de façon aléatoire sont difficiles à mémoriser. Envisagez le mot de passe indiqué dans la Figure 4 : `WWGUZv(f2yM:Bd`. Essayez de validation qui vers la mémoire ! Inutile de dire que, après un mot de passe généré de façon aléatoire de ce type est envoyé à un utilisateur, elle souhaiterez modifier le mot de passe à quelque chose de plus facile à mémoriser.

Utilisez le contrôle ChangePassword pour créer une interface pour un utilisateur de modifier son mot de passe. Beaucoup comme le contrôle PasswordRecovery, le contrôle ChangePassword se compose de deux vues : modification de mot de passe et de réussite. La vue de modification de mot de passe invite l’utilisateur pour leurs mots de passe anciennes et nouvelles. Lors de l’ancien mot de passe correct et un nouveau mot de passe qui répond à la longueur minimale en matière de caractère non alphanumérique qui fournit, le contrôle ChangePassword met à jour le mot de passe utilisateur et affiche la vue Opération réussie.

> [!NOTE]
> Le contrôle ChangePassword modifie le mot de passe utilisateur en appelant le `MembershipUser` l’objet [ `ChangePassword` méthode](https://msdn.microsoft.com/library/system.web.security.membershipuser.changepassword.aspx). La méthode ChangePassword accepte deux `String` paramètres - d’entrée *oldPassword* et *newPassword*- et met à jour le compte d’utilisateur avec le *newPassword*, en supposant que fourni *oldPassword* est correct.


Ouvrez le `ChangePassword.aspx` page et ajoutez un contrôle ChangePassword à la page, en nommant `ChangePwd`. À ce stade, la vue de conception doit afficher la modification de mot de passe permet d’afficher (voir Figure 6). Comme avec le contrôle PasswordRecovery, vous pouvez basculer entre les vues par le biais de balises actives du contrôle. En outre, les apparences de ces volets sont personnalisables via les propriétés de style assorties ou en les convertissant en un modèle.


[![Ajouter un contrôle ChangePassword à la Page](recovering-and-changing-passwords-vb/_static/image17.png)](recovering-and-changing-passwords-vb/_static/image16.png)

**Figure 6**: ajouter un contrôle ChangePassword à la Page ([cliquez pour afficher l’image en taille réelle](recovering-and-changing-passwords-vb/_static/image18.png))


Le contrôle ChangePassword peut mettre à jour le mot de passe de l’utilisateur actuellement connecté dans *ou* le mot de passe d’un autre utilisateur spécifié. Comme le montre la Figure 6, la vue de modification de mot de passe par défaut affiche seulement trois contrôles TextBox : un pour l’ancien mot de passe et deux pour le nouveau mot de passe. Cette interface par défaut est utilisée pour mettre à jour le mot de passe de l’utilisateur actuellement connecté.

Pour utiliser le contrôle ChangePassword pour mettre à jour le mot de passe d’un autre utilisateur, définissez le contrôle [ `DisplayUserName` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.changepassword.displayusername.aspx) sur True. Cela ajoute une quatrième zone de texte à la page invitant à dont mot de passe pour modifier le nom d’utilisateur de l’utilisateur.

Paramètre `DisplayUserName` à la valeur True est utile si vous souhaitez permettre à un utilisateur déconnecté de modifier son mot de passe sans avoir à vous connecter. Personnellement, je pense que ne présente aucun problème avec nécessitant un utilisateur de se connecter avant de lui permettant de modifier son mot de passe. Par conséquent, laisser `DisplayUserName` définie sur False (sa valeur par défaut). Dans cette décision, toutefois, nous essentiellement sommes à l’exception des utilisateurs anonymes d’atteindre cette page. Mettre à jour les règles du site URL d’autorisation afin d’interdire aux utilisateurs anonymes visitant `ChangePassword.aspx`. Si vous avez besoin de rafraîchir la mémoire sur la syntaxe de la règle d’autorisation URL, revenir à la <a id="_msoanchor_4"> </a> [ *autorisation basée sur l’utilisateur* ](../membership/user-based-authorization-vb.md) didacticiel.

> [!NOTE]
> Il peut sembler que la `DisplayUserName` propriété est utile pour ce qui permet aux administrateurs de modifier les mots de passe des autres utilisateurs. Toutefois, même lorsque `DisplayUserName` est définie sur True, l’ancien mot de passe correct doit être connu et entré. Nous nous pencherons sur les techniques permettant aux administrateurs de modifier les mots de passe utilisateur à l’étape 3.


Visitez le `ChangePassword.aspx` page via un navigateur et de modifier votre mot de passe. Notez qu’un message d’erreur s’affiche si vous entrez un nouveau mot de passe qui ne parvient pas à répondre à la longueur de mot de passe en matière de caractère non alphanumérique spécifié dans la configuration de l’appartenance (voir la Figure 7).


[![Ajouter un contrôle ChangePassword à la Page](recovering-and-changing-passwords-vb/_static/image20.png)](recovering-and-changing-passwords-vb/_static/image19.png)

**Figure 7**: ajouter un contrôle ChangePassword à la Page ([cliquez pour afficher l’image en taille réelle](recovering-and-changing-passwords-vb/_static/image21.png))


Lors de l’entrée de l’ancien mot de passe correct et un nouveau mot de passe valide, connecté sur l’utilisateur, mot de passe est modifié et affiche la vue Opération réussie.

### <a name="sending-a-confirmation-email"></a>Envoyer un E-mail de Confirmation

Par défaut, le contrôle ChangePassword n’envoie pas d’un message électronique à l’utilisateur dont mot de passe a été mis à jour. Si vous souhaitez envoyer un courrier électronique, il suffit de configurer le contrôle `MailDefinition` propriété. Nous allons configurer le contrôle ChangePassword afin que l’utilisateur reçoit un e-mail au format HTML qui contient le mot de passe.

Commencez par créer un nouveau fichier dans le `EmailTemplates` dossier nommé `ChangePassword.htm`. Ajoutez le balisage suivant :

[!code-html[Main](recovering-and-changing-passwords-vb/samples/sample4.html)]

Ensuite, définissez le contrôle ChangePassword `MailDefinition` la propriété `BodyFileName`, `IsBodyHtml`, et `Subject` propriétés à ~ / EmailTemplates/ChangePassword.htm, la valeur True et votre mot de passe a changé !, respectivement.

Après avoir apporté ces modifications, visitez la page et modifier votre mot de passe à nouveau. Cette fois-ci, le contrôle ChangePassword envoie un e-mail personnalisé, au format HTML à l’adresse de messagerie de l’utilisateur sur le fichier (voir Figure 8).


[![Un Message de courrier électronique informe que leur mot de passe utilisateur a changé.](recovering-and-changing-passwords-vb/_static/image23.png)](recovering-and-changing-passwords-vb/_static/image22.png)

**Figure 8**: courrier électronique un Message informe l’utilisateur que leur mot de passe a changé ([cliquez pour afficher l’image en taille réelle](recovering-and-changing-passwords-vb/_static/image24.png))


## <a name="step-3-allowing-administrators-to-change-users-passwords"></a>Étape 3 : Ce qui permet aux administrateurs de modifier les mots de passe utilisateur

Une fonctionnalité courante dans les applications qui prennent en charge les comptes d’utilisateur est la possibilité pour un utilisateur administratif modifier les mots de passe des autres utilisateurs. Cette fonctionnalité est parfois nécessaire, car le système n’a pas la possibilité pour les utilisateurs à modifier leur mot de passe. Dans ce cas, la seule façon pour un utilisateur de récupérer leur mot de passe oublié serait à l’administrateur pour affecter un nouveau mot de passe. Avec les contrôles PasswordRecovery et ChangePassword, toutefois, les utilisateurs administratifs ne doivent pas occupé eux-mêmes à des modifications de mots de passe utilisateurs, comme les utilisateurs sont capables de faire eux-mêmes.

Mais que se passe-t-il si votre client insiste pour que les utilisateurs administratifs doivent être en mesure de modifier les mots de passe des autres utilisateurs ? Malheureusement, l’ajout de cette fonctionnalité peut être un peu de travail. Pour modifier un mot de passe utilisateur, à la fois l’ancien et le nouveau mot de passe doit être fourni pour le `MembershipUser` l’objet `ChangePassword` (méthode), mais un administrateur ne doit pas obligé de connaître le mot de passe d’un utilisateur afin de le modifier.

Une solution de contournement consiste à tout d’abord réinitialiser le mot de passe, puis remplacez-le par le nouveau mot de passe à l’aide de code semblable au suivant :

[!code-vb[Main](recovering-and-changing-passwords-vb/samples/sample5.vb)]

Ce code commence par récupérer des informations sur *nom d’utilisateur*, qui est l’utilisateur dont l’administrateur souhaite modifier le mot de passe. Ensuite, le `ResetPassword` méthode est appelée, qui affecte et l’utilisateur un nouveau mot de passe aléatoire. Ce mot de passe généré de façon aléatoire est retourné par la méthode et stocké dans la variable `resetPwd`. Maintenant que nous savons le mot de passe, nous pouvons le modifier via un appel à `ChangePassword`.

Le problème est que ce code fonctionne uniquement si la valeur de la configuration du système d’appartenance telle que `RequiresQuestionAndAnswer` a la valeur False. Si `RequiresQuestionAndAnswer` a la valeur True, car il s’agit avec notre application, puis le `ResetPassword` méthode doit être transmis à la réponse de sécurité, sinon elle lève une exception.

Si l’infrastructure de l’appartenance est configuré pour exiger une question de sécurité et une réponse, et encore votre client insiste pour que les administrateurs être en mesure de modifier les mots de passe utilisateurs, vous avez trois options :

- Lever vos mains en l’air et indiquer votre client qu’il s’agit d’une seule chose qui ne peut pas être effectuée.
- Définissez `RequiresQuestionAndAnswer` sur False. Cela entraîne une application moins sécurisée. Imaginez qu’un utilisateur malveillant a eu accès à la boîte de réception d’un autre utilisateur. Peut-être l’utilisateur compromis a quitté leur bureau d’aller déjeuner et n’a pas de verrouiller leur station de travail, ou peut-être qu’ils accéder à leur courrier électronique à partir d’un terminal public et n’a pas de se déconnecter. Dans les deux cas, l’utilisateur malveillant peut visiter le `RecoverPassword.aspx` page et entrez le nom d’utilisateur. Le système envoie ensuite le mot de passe récupéré sans demander de la réponse de sécurité.
- Contourner la couche d’abstraction créée par l’infrastructure de l’appartenance et de travailler directement avec la base de données SQL Server. Le schéma d’appartenance inclut une procédure stockée nommée `aspnet_Membership_SetPassword` qui définit un mot de passe et ne nécessite pas de la réponse de sécurité ou d’un ancien mot de passe pour pouvoir accomplir sa tâche.

Aucune de ces options sont particulièrement attrayante, mais c’est la façon dont la durée de vie d’un développeur va parfois.

J’ai poursuivi et implémenté la troisième méthode, écrivez du code qui contourne le `Membership` et `MembershipUser` classes et opère directement par rapport à la `SecurityTutorials` base de données.

> [!NOTE]
> En travaillant directement avec la base de données, l’encapsulation fournie par l’infrastructure de l’appartenance est cassée. Cette décision lie nous le `SqlMembershipProvider`, ce qui rend notre code moins portable. En outre, ce code ne peut pas fonctionne comme prévu dans les futures versions d’ASP.NET si le schéma de l’appartenance change. Cette approche est une solution de contournement et, comme la plupart des solutions de contournement n’est pas un exemple de meilleures pratiques.


Le code a des bits peu attrayant et est assez long. Par conséquent, je ne veux pas encombrer ce didacticiel avec un examen approfondi de celui-ci. Si vous souhaitez en savoir plus, téléchargez le code pour ce didacticiel, la visite le `~/Administration/ManageUsers.aspx` page. Cette page, nous avons créé dans le <a id="_msoanchor_5"> </a> [didacticiel précédent](building-an-interface-to-select-one-user-account-from-many-vb.md), répertorie tous les utilisateurs. J’ai mis à jour le contrôle GridView pour inclure un lien vers le `UserInformation.aspx` page, en passant le nom d’utilisateur de l’utilisateur sélectionné via la chaîne de requête. Le `UserInformation.aspx` page affiche des informations sur l’utilisateur sélectionné et les zones de texte pour modifier leur mot de passe (voir Figure 9).

Après avoir entrer le nouveau mot de passe, confirmer dans la deuxième zone de texte et en cliquant sur le bouton utilisateur de mise à jour, s’ensuit une publication (postback) et le `aspnet_Membership_SetPassword` procédure stockée est appelée, la mise à jour le mot de passe. J’encourage les lecteurs intéressés par cette fonctionnalité pour vous familiariser avec le code et réessayez d’étendre les fonctionnalités pour inclure l’envoi d’un e-mail à l’utilisateur dont mot de passe a été modifié.


[![Un administrateur peut modifier un mot de passe utilisateur](recovering-and-changing-passwords-vb/_static/image26.png)](recovering-and-changing-passwords-vb/_static/image25.png)

**Figure 9**: un administrateur peut modifier le mot de passe d’un utilisateur ([cliquez pour afficher l’image en taille réelle](recovering-and-changing-passwords-vb/_static/image27.png))


> [!NOTE]
> Le `UserInformation.aspx` page actuellement fonctionne uniquement si l’infrastructure de l’appartenance est configuré pour stocker les mots de passe au format Clear ou Hashed. Il n’a pas le code pour chiffrer le nouveau mot de passe, même si vous êtes invité à ajouter cette fonctionnalité. Le je recommande d’ajouter le code nécessaire consiste à utiliser un décompilateur comme [Reflector](http://www.aisto.com/roeder/dotnet/) pour examiner le code source pour les méthodes dans le .NET Framework ; commencer en examinant le `SqlMembershipProvider` la classe `ChangePassword` (méthode). Il s’agit de la technique que j’ai utilisé pour écrire le code pour la création d’un hachage du mot de passe.


## <a name="summary"></a>Récapitulatif

ASP.NET propose deux contrôles pour aider les utilisateurs à gérer leur mot de passe. Le contrôle PasswordRecovery est utile pour ceux qui ont oublié leur mot de passe. Selon la configuration de l’infrastructure d’appartenance, l’utilisateur est soit envoyé par courrier électronique leur mot de passe existant ou nouveau, générée de manière aléatoire. Le contrôle ChangePassword permet à un utilisateur à mettre à jour son mot de passe.

Comme les contrôles de connexion et le contrôle CreateUserWizard, les contrôles PasswordRecovery et ChangePassword restituent une interface utilisateur riche sans devoir écrire la moindre ligne de balisage déclaratif ou de la ligne de code. Si l’interface utilisateur par défaut ne répond pas à vos besoins, vous pouvez le personnaliser via un large éventail de propriétés de style. Vous pouvez également les interfaces de contrôles peuvent être convertis en modèles, pour un même meilleur niveau de contrôle. Dans les coulisses, ces contrôles utilisent l’API d’appartenance, appeler le `MembershipUser` l’objet `ResetPassword` et `ChangePassword` méthodes.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [Démarrages rapides de contrôle ChangePassword](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/changepassword.aspx)
- [Démarrages rapides de contrôle PasswordRecovery](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/passwordrecovery.aspx)
- [Envoi d’E-mails dans ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [`System.Net.Mail` Forum aux questions sur](http://www.systemnetmail.com/)

### <a name="about-the-author"></a>À propos de l’auteur

Scott Mitchell, auteur de plusieurs livres sur ASP/ASP.NET et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est *[SAM animer vous-même ASP.NET 2.0 des dernières 24 heures](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott peut être atteint à [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou via son blog à [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Les réviseurs tête pour ce didacticiel incluent Michael Emmings et Suchi Banerjee. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](building-an-interface-to-select-one-user-account-from-many-vb.md)
> [Suivant](unlocking-and-approving-user-accounts-vb.md)
