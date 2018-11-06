---
uid: web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-cs
title: Validation des informations d’identification de l’utilisateur sur le Store d’utilisateur d’appartenance (c#) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons examiner comment valider les informations d’identification d’un utilisateur par rapport au magasin d’utilisateur d’appartenance à l’aide de moyens de programmation et le contrôle de connexion...
ms.author: riande
ms.date: 01/18/2008
ms.assetid: 61aa4e08-aa81-4aeb-8ebe-19ba7a65e04c
msc.legacyurl: /web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-cs
msc.type: authoredcontent
ms.openlocfilehash: ac04ee84ad30015ec3dea272c2b143bb2dae25e5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824963"
---
<a name="validating-user-credentials-against-the-membership-user-store-c"></a>Validation des informations d’identification de l’utilisateur sur le Store d’utilisateur d’appartenance (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_06_CS.zip) ou [télécharger le PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial06_LoggingIn_cs.pdf)

> Dans ce didacticiel, nous allons examiner comment valider les informations d’identification d’un utilisateur par rapport au magasin d’utilisateur d’appartenance à l’aide de moyens de programmation et le contrôle de connexion. Nous examinerons également comment personnaliser l’apparence et le comportement du contrôle de connexion.


## <a name="introduction"></a>Introduction

Dans le <a id="Tutorial05"> </a> [didacticiel précédent](creating-user-accounts-cs.md) nous avons vu comment créer un nouveau compte d’utilisateur dans le cadre de l’appartenance. Nous avons examiné tout d’abord créer par programme les comptes d’utilisateurs via le `Membership` la classe `CreateUser` (méthode) et ensuite examiné à l’aide du contrôle CreateUserWizard Web. Toutefois, la page de connexion valide actuellement les informations d’identification fournies par rapport à une liste codée en dur de paires nom d’utilisateur et mot de passe. Nous devons mettre à jour de la logique de la page connexion afin qu’il valide les informations d’identification dans le magasin de l’utilisateur de l’infrastructure d’appartenance.

Beaucoup comme avec la création de comptes d’utilisateur, informations d’identification peuvent être validées par programme ou de façon déclarative. L’API d’appartenance inclut une méthode pour valider par programmation les informations d’identification d’un utilisateur par rapport au magasin de l’utilisateur. Et ASP.NET est livré avec le contrôle de connexion Web, ce qui rend une interface utilisateur avec des zones de texte pour le nom d’utilisateur et mot de passe et un bouton pour vous connecter.

Dans ce didacticiel, nous allons examiner comment valider les informations d’identification d’un utilisateur par rapport au magasin d’utilisateur d’appartenance à l’aide de moyens de programmation et le contrôle de connexion. Nous examinerons également comment personnaliser l’apparence et le comportement du contrôle de connexion. C’est parti !

## <a name="step-1-validating-credentials-against-the-membership-user-store"></a>Étape 1 : Validation des informations d’identification sur le Store d’utilisateur d’appartenance

Pour les sites web qui utilisent l’authentification par formulaire, un utilisateur ouvre une session sur le site Web en accédant à une page de connexion et en entrant leurs informations d’identification. Ces informations d’identification sont ensuite comparées par rapport au magasin de l’utilisateur. Si elles sont valides, l’utilisateur reçoit un ticket d’authentification forms, qui est un jeton de sécurité qui indique l’identité et l’authenticité du visiteur.

Pour valider un utilisateur par rapport à l’infrastructure de l’appartenance, utilisez la `Membership` la classe [ `ValidateUser` méthode](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx). Le `ValidateUser` méthode accepte deux paramètres d’entrée - *`username`* et *`password`* - et retourne une valeur booléenne indiquant si les informations d’identification sont valides. Comme avec la `CreateUser` méthode que nous avons examiné dans le didacticiel précédent, le `ValidateUser` méthode délègue la validation réelle pour le fournisseur d’appartenances configuré.

Le `SqlMembershipProvider` valide les informations d’identification fournies en obtenant le mot de passe de l’utilisateur spécifié par le biais de la `aspnet_Membership_GetPasswordWithFormat` procédure stockée. N’oubliez pas que le `SqlMembershipProvider` stocke les mots de passe utilisateur à l’aide d’un des trois formats : clair, chiffrés ou hachés. Le `aspnet_Membership_GetPasswordWithFormat` procédure stockée retourne le mot de passe dans leur format brut. Pour les mots de passe chiffrés ou hachés, le `SqlMembershipProvider` transforme le *`password`* valeur passée dans le `ValidateUser` méthode en son équivalent chiffrés ou hachés état et puis le compare avec ce qui a été retourné à partir de la base de données. Si le mot de passe stocké dans la base de données correspond à la mise en forme mot de passe entré par l’utilisateur, les informations d’identification sont valides.

Nous allons mettre à jour notre page de connexion (~ /`Login.aspx`) afin qu’il valide les informations d’identification fournies sur le magasin d’utilisateur d’appartenance framework. Nous avons créé cette page de connexion dans le <a id="Tutorial02"> </a> [ *une vue d’ensemble de l’authentification par formulaire* ](../introduction/an-overview-of-forms-authentication-cs.md) didacticiel, la création d’une interface avec deux zones de texte pour le nom d’utilisateur et le mot de passe, un Mémoriser mes informations de case à cocher et un bouton de connexion (voir Figure 1). Le code valide les informations d’identification entrées par rapport à une liste codée en dur de paires nom d’utilisateur et mot de passe (Scott/mot de passe, Jisun/mot de passe et Sam/mot de passe). Dans le <a id="Tutorial03"> </a> [ *Configuration de l’authentification de formulaires et des sujets avancés* ](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) didacticiel nous mis à jour le code de la page de connexion pour stocker des informations supplémentaires dans les formulaires ticket d’authentification `UserData` propriété.


[![Interface de la Page connexion inclut deux zones de texte, CheckBoxList et un bouton](validating-user-credentials-against-the-membership-user-store-cs/_static/image2.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image1.png)

**Figure 1**: Interface inclut deux zones de texte la connexion de la Page, CheckBoxList et un bouton ([cliquez pour afficher l’image en taille réelle](validating-user-credentials-against-the-membership-user-store-cs/_static/image3.png))


Interface utilisateur de la page de connexion peut rester inchangée, mais nous devons remplacer le bouton de connexion `Click` Gestionnaire d’événements avec le code qui valide l’utilisateur sur le magasin d’utilisateur d’appartenance framework. Mettre à jour le Gestionnaire d’événements afin que son code apparaît comme suit :

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample1.cs)]

Ce code est particulièrement simple. Nous commençons en appelant le `Membership.ValidateUser` méthode, en passant le nom d’utilisateur fourni et un mot de passe. Si cette méthode retourne la valeur true, l’utilisateur est connecté au site via le `FormsAuthentication` RedirectFromLoginPage la méthode de classe. (Comme expliqué dans la <a id="Tutorial2"> </a> [ *une vue d’ensemble de l’authentification par formulaire* ](../introduction/an-overview-of-forms-authentication-cs.md) didacticiel, le `FormsAuthentication.RedirectFromLoginPage` crée les formulaires ticket d’authentification, il redirige l’utilisateur à la page appropriée.) Si les informations d’identification ne sont pas valides, toutefois, le `InvalidCredentialsMessage` étiquette est affichée, informant l’utilisateur que leur nom d’utilisateur ou mot de passe incorrect.

C’est tout cela !

Pour tester que la page de connexion fonctionne comme prévu, tentative de connexion avec l’un des comptes d’utilisateur que vous avez créé dans le didacticiel précédent. Ou, si vous n’avez pas encore créé un compte, accédez à l’avance et créer un à partir du `~/Membership/CreatingUserAccounts.aspx` page.

> [!NOTE]
> Lorsque l’utilisateur entre ses informations d’identification et soumet le formulaire de la page de connexion, les informations d’identification, y compris son mot de passe sont transmises via Internet au serveur web dans *texte brut*. Cela signifie que tout pirate d’analyser le trafic réseau peut voir le nom d’utilisateur et le mot de passe. Pour éviter ce problème, il est essentiel pour chiffrer le trafic réseau à l’aide de [couches SSL (Secure Socket)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Cela garantit que les informations d’identification (comme balisage HTML de la page entière) est chiffré dès le moment où qu'ils quittent le navigateur jusqu'à ce qu’ils sont reçus par le serveur web.


### <a name="how-the-membership-framework-handles-invalid-login-attempts"></a>Comment l’infrastructure de l’appartenance gère les tentatives de connexion non valide

Lorsqu’un visiteur atteint la page de connexion et envoie leurs informations d’identification, son navigateur effectue une requête HTTP à la page de connexion. Si les informations d’identification sont valides, la réponse HTTP inclut le ticket d’authentification dans un cookie. Par conséquent, un pirate de s’introduire dans votre site pourrait créer un programme qui envoie des requêtes HTTP vers la page de connexion avec un nom d’utilisateur valide et estimer le mot de passe de façon exhaustive. Si l’estimation de mot de passe est correcte, la page de connexion retourne le cookie de ticket d’authentification, après quoi le programme sait qu’il a tombé sur une paire nom d’utilisateur/mot de passe valide. En force brute, un tel programme pourrez faites-nous signe un mot de passe utilisateur, en particulier si le mot de passe est faible.

Pour empêcher ces attaques en force brute, l’infrastructure de l’appartenance verrouille un utilisateur s’il existe un certain nombre de tentatives de connexion ayant échoué dans un certain laps de temps. Les paramètres exacts sont configurables via les paramètres de configuration de fournisseur de l’appartenance deux suivantes :

- `maxInvalidPasswordAttempts` -Spécifie le mot de passe non valide combien tentatives sont autorisées pour l’utilisateur au sein de la période de temps avant que le compte est verrouillé. La valeur par défaut est 5.
- `passwordAttemptWindow` -Indique la période de temps en minutes pendant lesquelles le nombre spécifié de tentatives de connexion non valide provoque le compte soit verrouillé. La valeur par défaut est 10.

Si un utilisateur a été verrouillé, elle ne peut pas se connecter jusqu'à ce qu’un administrateur déverrouille son compte. Lorsqu’un utilisateur est verrouillé, le `ValidateUser` méthode *toujours* retourner `false`, même si les informations d’identification valides sont fournies. Bien que ce comportement réduit la probabilité qu’un pirate s’introduise dans votre site via des méthodes de force brute, il peut finir de verrouillage d’un utilisateur valide qui a oublié simplement son mot de passe ou accidentellement a le verrouillage des majuscules ou rencontre un mauvais jour frappe.

Malheureusement, il n’existe aucun outil intégré pour le déverrouillage d’un compte d’utilisateur. Pour déverrouiller un compte, vous pouvez modifier la base de données directement - modifier la `IsLockedOut` champ dans le `aspnet_Membership` de table pour le compte d’utilisateur approprié - ou créer une interface basée sur le web répertorie les comptes avec des options pour les déverrouiller verrouillée. Nous allons examiner la création d’interfaces d’administration pour accomplir des tâches relatives au compte et un rôle utilisateur courantes dans un futur didacticiel.

> [!NOTE]
> L’inconvénient de la `ValidateUser` méthode est que lorsque les informations d’identification fournies ne sont pas valides, il ne fournit pas d’explications quant à pourquoi. Les informations d’identification peuvent être non valides, car il n’existe aucune paire nom d’utilisateur/mot de passe correspondant dans le magasin de l’utilisateur, ou parce que l’utilisateur n’a pas encore été approuvée, soit parce que l’utilisateur a été verrouillé. À l’étape 4, nous verrons comment afficher un message plus détaillé à l’utilisateur lors de leur tentative de connexion échoue.


## <a name="step-2-collecting-credentials-through-the-login-web-control"></a>Étape 2 : Collecte des informations d’identification via le contrôle Web de connexion

Le [contrôle Web de connexion](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) restitue une interface d’utilisateur par défaut très semblable à celle que nous avons créé dans le <a id="SKM5"> </a> [ *une vue d’ensemble de l’authentification par formulaire* ](../introduction/an-overview-of-forms-authentication-cs.md) didacticiel. L’utilisation du contrôle de connexion évite le travail de devoir créer l’interface pour collecter les informations d’identification de visiteur s. En outre, le contrôle de connexion se connecte automatiquement l’utilisateur (en supposant que les informations d’identification soumises sont valides), ainsi l’enregistrement nous évite de devoir écrire du code.

Nous allons mettre à jour `Login.aspx`, en remplaçant l’interface créée manuellement et de code avec un contrôle de connexion. Commencez par supprimer le balisage existant et le code dans `Login.aspx`. Vous pouvez supprimer directement ce dernier, ou simplement mettre en commentaire. Pour commenter le balisage déclaratif, encadrez-le avec le `<%--` et `--%>` délimiteurs. Vous pouvez entrer manuellement ces délimiteurs, ou, comme le montre la Figure 2, vous pouvez sélectionner le texte à placer en commentaire, puis cliquez sur le commentaire de l’icône de lignes sélectionnées dans la barre d’outils. De même, vous pouvez utiliser le commentaire de l’icône de lignes sélectionnées en commentaire le code sélectionné dans la classe code-behind.


[![Commentez l’existant de balisage déclaratif et le Code Source dans Login.aspx](validating-user-credentials-against-the-membership-user-store-cs/_static/image5.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image4.png)

**Figure 2**: commentaire Out the existant balisage déclaratif et le Code Source dans `Login.aspx` ([cliquez pour afficher l’image en taille réelle](validating-user-credentials-against-the-membership-user-store-cs/_static/image6.png))


> [!NOTE]
> Le commentaire de l’icône de lignes sélectionnées n’est pas disponible lorsque vous affichez le balisage déclaratif dans Visual Studio 2005. Si vous n’utilisez pas Visual Studio 2008, vous devez ajouter manuellement le `<%--` et `--%>` délimiteurs.


Ensuite, faites glisser un contrôle de connexion à partir de la boîte à outils sur la page et définissez son `ID` propriété `myLogin`. À ce stade votre écran doit ressembler à la Figure 3. Notez que l’interface du contrôle de connexion par défaut inclut les contrôles de zone de texte pour le nom d’utilisateur et mot de passe, un mémoriser case à cocher et un bouton dans le journal. Il existe également `RequiredFieldValidator` contrôles pour les deux zones de texte.


[![Ajouter un contrôle de connexion à la Page](validating-user-credentials-against-the-membership-user-store-cs/_static/image8.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image7.png)

**Figure 3**: ajouter un contrôle de connexion à la Page ([cliquez pour afficher l’image en taille réelle](validating-user-credentials-against-the-membership-user-store-cs/_static/image9.png))


Et nous avons terminé ! Clic sur bouton de connexion du contrôle de connexion, une publication (postback) se produit et le contrôle Login appellera le `Membership.ValidateUser` méthode, en passant le nom d’utilisateur entré et le mot de passe. Si les informations d’identification ne sont pas valides, le contrôle de connexion affiche un message. Si, toutefois, les informations d’identification sont valides, le contrôle Login crée les formulaires ticket d’authentification et redirige l’utilisateur vers la page appropriée.

Le contrôle de connexion utilise quatre facteurs pour déterminer la page appropriée pour rediriger l’utilisateur à en cas de connexion réussie :

- Indique si le contrôle de connexion est sur la page de connexion comme défini par `loginUrl` est de valeur de valeur par défaut de ce paramètre dans la configuration de l’authentification de formulaires ; `Login.aspx`
- La présence d’un `ReturnUrl` paramètre querystring
- La valeur du contrôle de connexion [ `DestinationUrl` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.destinationpageurl.aspx)
- Le `defaultUrl` valeur spécifiée dans les formulaires, les paramètres de configuration de l’authentification ; la valeur par défaut de ce paramètre est `Default.aspx`

Figure 4 illustre la façon dont le contrôle de connexion utilise ces quatre paramètres pour arriver à sa décision de la page appropriée.


[![Ajouter un contrôle de connexion à la Page](validating-user-credentials-against-the-membership-user-store-cs/_static/image11.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image10.png)

**Figure 4**: ajouter un contrôle de connexion à la Page ([cliquez pour afficher l’image en taille réelle](validating-user-credentials-against-the-membership-user-store-cs/_static/image12.png))


Prenez un moment pour tester le contrôle de connexion en visitant le site via un navigateur et de connexion en tant qu’un utilisateur existant dans le cadre de l’appartenance.

Interface de rendu du contrôle de la connexion est hautement configurable. Il existe un certain nombre de propriétés qui influencent son apparence ; de plus, le contrôle de connexion peut être converti les éléments d’interface utilisateur dans un modèle pour un contrôle précis sur la disposition. Le reste de cette étape examine comment personnaliser l’apparence et la disposition.

### <a name="customizing-the-login-controls-appearance"></a>Personnalisation de l’apparence du contrôle de connexion

Paramètres de propriété par défaut du contrôle de connexion restituent une interface utilisateur avec un titre (journal), contrôles d’étiquette et de zone de texte pour les entrées de nom d’utilisateur et mot de passe, un Mémoriser mes informations de votre prochaine case à cocher et un bouton de connexion. Les apparences de ces éléments sont tous les configurables via les propriétés de nombreux du contrôle de la connexion. En outre, les éléments d’interface utilisateur - telles qu’un lien vers une page pour créer un nouveau compte d’utilisateur - peuvent être ajoutés en définissant une propriété ou deux.

Nous allons consacrer quelques instants pour gussy l’apparence de notre contrôle de connexion. Dans la mesure où le `Login.aspx` page possède déjà le texte figurant en haut de la page qui indique que la connexion, le titre du contrôle de la connexion est superflue. Par conséquent, effacez le [ `TitleText` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.titletext.aspx) valeur afin de supprimer le titre du contrôle de la connexion.

: Le nom d’utilisateur et mot de passe : étiquettes à gauche des deux contrôles de zone de texte pouvant être personnalisés via le [ `UserNameLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.usernamelabeltext.aspx) et [ `PasswordLabelText` propriétés](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordlabeltext.aspx), respectivement. Nous allons modifier le nom d’utilisateur : étiquette pour lire le nom d’utilisateur :. Les styles Label et TextBox sont configurables via la [ `LabelStyle` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.labelstyle.aspx) et [ `TextBoxStyle` propriétés](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textboxstyle.aspx), respectivement.

Mémoriser mon adresse propriété de texte suivant temps la case à cocher peut être définie via le contrôle Login [ `RememberMeText property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermetext.aspx), et sa valeur par défaut activée état est configurable via la [ `RememberMeSet property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermeset.aspx) (qui valeur par défaut est False). Accédez à l’avance, puis définissez le `RememberMeSet` propriété sur True afin que le Mémoriser mes informations de votre prochaine case à cocher est activée par défaut.

Le contrôle de connexion offre deux propriétés pour ajuster la disposition de ses contrôles d’interface utilisateur. Le [ `TextLayout property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textlayout.aspx) indique si le nom d’utilisateur : et mot de passe : étiquettes apparaissent à gauche de leurs zones de texte correspondante (il s’agit de la valeur par défaut) ou au-dessus d’elles. Le [ `Orientation property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.orientation.aspx) indique si les entrées de nom d’utilisateur et mot de passe sont situées verticalement (un au-dessus de l’autre) ou horizontalement. Je vais laisser ces deux propriétés leurs valeurs par défaut, mais je vous encourage à essayer de définir ces deux propriétés à leurs valeurs par défaut pour voir l’effet obtenu.

> [!NOTE]
> Dans la section suivante, la configuration de mise en page du contrôle de connexion, nous allons examiner des modèles pour définir la disposition précise des éléments d’interface utilisateur du contrôle de disposition.


Encapsuler les paramètres de propriété du contrôle de connexion en définissant le [ `CreateUserText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createusertext.aspx) et [ `CreateUserUrl` propriétés](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createuserurl.aspx) à ne pas encore inscrit ? Créer un compte ! et `~/Membership/CreatingUserAccounts.aspx`, respectivement. Cela ajoute un lien hypertexte à l’interface du contrôle de connexion pointant vers la page Nous avons créé dans le <a id="SKM6"> </a> [didacticiel précédent](creating-user-accounts-cs.md). Le contrôle Login [ `HelpPageText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppagetext.aspx) et [ `HelpPageUrl` propriétés](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppageurl.aspx) et [ `PasswordRecoveryText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoverytext.aspx) et [ `PasswordRecoveryUrl` propriétés](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoveryurl.aspx) fonctionnent de la même manière, des liens vers une page d’aide et une page de récupération du mot de passe de rendu.

Après avoir apporté ces modifications de propriété, balisage déclaratif et l’apparence de votre contrôle de connexion doivent ressembler à celle illustrée à la Figure 5.


[![Valeurs des propriétés du contrôle de la connexion déterminent son apparence](validating-user-credentials-against-the-membership-user-store-cs/_static/image14.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image13.png)

**Figure 5**: valeurs de dicter son apparence propriétés du contrôle de connexion le ([cliquez pour afficher l’image en taille réelle](validating-user-credentials-against-the-membership-user-store-cs/_static/image15.png))


### <a name="configuring-the-login-controls-layout"></a>Configuration de mise en page du contrôle de connexion

Interface d’utilisateur du contrôle Web de connexion par défaut présente l’interface dans le code HTML `<table>`. Mais que se passe-t-il si nous avons besoin d’un contrôle plus précis sur la sortie rendue ? Nous pouvons remplacer le `<table>` avec une série de `<div>` balises. Ou bien, que se passe-t-il si notre application nécessite des informations d’identification supplémentaires pour l’authentification ? Par exemple, de nombreux sites Web financiers, imposent aux utilisateurs non seulement un nom d’utilisateur et mot de passe, mais également un numéro d’Identification personnel (PIN) ou autres informations d’identification. Quel que soit les raisons, il est possible de convertir le contrôle de connexion dans un modèle à partir de laquelle nous pouvons définir explicitement de balisage déclaratif de l’interface.

Nous devons faire deux choses pour mettre à jour le contrôle de connexion pour collecter des informations d’identification supplémentaires :

1. Mettre à jour l’interface du contrôle de connexion pour inclure les contrôles Web pour collecter les informations d’identification supplémentaires.
2. Substituer la connexion logique du contrôle d’authentification interne afin qu’un utilisateur est authentifié uniquement si leur nom d’utilisateur et le mot de passe est valide et leurs informations d’identification supplémentaires sont valides, trop.

Pour atteindre la première tâche, nous devons convertir le contrôle de connexion dans un modèle et ajoutez les contrôles Web nécessaires. Comme pour la deuxième tâche, la connexion logique du contrôle d’authentification peut être remplacée en créant un gestionnaire d’événements pour le contrôle [ `Authenticate` événement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx).

Nous allons mettre à jour le contrôle de connexion afin qu’il demande aux utilisateurs pour leur nom d’utilisateur, le mot de passe et l’adresse de messagerie et uniquement s’authentifie l’utilisateur si l’adresse de messagerie fournie correspond à leur adresse de messagerie sur fichier. Nous devons d’abord convertir l’interface de contrôle de la connexion à un modèle. À partir de la balise active du contrôle de connexion, choisissez l’option Convertir en modèle.


[![Convertir le contrôle de connexion à un modèle](validating-user-credentials-against-the-membership-user-store-cs/_static/image17.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image16.png)

**Figure 6**: convertir le contrôle de connexion à un modèle ([cliquez pour afficher l’image en taille réelle](validating-user-credentials-against-the-membership-user-store-cs/_static/image18.png))


> [!NOTE]
> Pour rétablir le contrôle de la connexion à sa version préalable template, cliquez sur le lien de réinitialisation à partir de la balise active du contrôle.


Conversion du contrôle de connexion à un modèle ajoute un `LayoutTemplate` pour le balisage du contrôle déclaratif avec des éléments HTML et des contrôles Web de définition de l’interface utilisateur. Comme le montre la Figure 7, conversion le contrôle d’un modèle supprime un nombre de propriétés à partir de la fenêtre Propriétés, telles que `TitleText`, `CreateUserUrl`, et ainsi de suite, dans la mesure où ces valeurs de propriété sont ignorées lors de l’utilisation d’un modèle.


[![Moins de propriétés sont que disponibles lorsque le contrôle de connexion est converti en un modèle](validating-user-credentials-against-the-membership-user-store-cs/_static/image20.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image19.png)

**Figure 7**: moins de propriétés sont disponibles lorsque le contrôle de connexion est converti en un modèle ([cliquez pour afficher l’image en taille réelle](validating-user-credentials-against-the-membership-user-store-cs/_static/image21.png))


Le balisage HTML dans le `LayoutTemplate` peut être modifié en fonction des besoins. De même, n’hésitez pas à ajouter de nouveaux contrôles Web au modèle. Toutefois, il est important que les contrôles Web de ce contrôle de connexion core restent dans le modèle et conserver qui leur est affectée `ID` valeurs. En particulier, ne supprimez pas ni renommer le `UserName` ou `Password` zones de texte, le `RememberMe` case à cocher, le `LoginButton` bouton, le `FailureText` étiquette, ou le `RequiredFieldValidator` contrôles.

Pour collecter l’adresse de messagerie du visiteur, nous devons ajouter une zone de texte pour le modèle. Ajoutez le balisage déclaratif suivant entre la ligne de table (`<tr>`) qui contient le `Password` zone de texte et la ligne de table qui contient le Mémoriser mes informations ensuite heure case à cocher :

[!code-aspx[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample2.aspx)]

Après avoir ajouté le `Email` zone de texte, visitez la page via un navigateur. Comme le montre la Figure 8, interface utilisateur du contrôle de connexion inclut désormais une troisième zone de texte.


[![Le contrôle de connexion inclut à présent une zone de texte pour l’adresse de messagerie de l’utilisateur](validating-user-credentials-against-the-membership-user-store-cs/_static/image23.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image22.png)

**Figure 8**: le contrôle de connexion inclut à présent une zone de texte pour l’adresse de messagerie de l’utilisateur ([cliquez pour afficher l’image en taille réelle](validating-user-credentials-against-the-membership-user-store-cs/_static/image24.png))


À ce stade, le contrôle de connexion est toujours à l’aide de la `Membership.ValidateUser` méthode pour valider les informations d’identification fournies. En conséquence, la valeur entrée dans le `Email` zone de texte n’a aucune incidence sur indique si l’utilisateur peut se connecter. À l’étape 3, nous allons examiner comment remplacer la connexion logique du contrôle d’authentification afin que les informations d’identification sont considéré comme valides si le nom d’utilisateur et le mot de passe sont valides et fait correspondre l’adresse électronique fournie avec l’adresse de messagerie sur fichier.

## <a name="step-3-modifying-the-login-controls-authentication-logic"></a>Étape 3 : Modification de la connexion logique du contrôle d’authentification

Lorsqu’un visiteur fournit ses informations d’identification et clique sur que le bouton se connecter, une publication (postback) s’ensuit et la connexion contrôlent progresse dans son flux de travail de l’authentification. Le démarrage du workflow en déclenchant le [ `LoggingIn` événement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggingin.aspx). Les gestionnaires d’événements associés à cet événement peuvent annuler le journal dans l’opération en définissant le `e.Cancel` propriété `true`.

Si le journal dans l’opération n’est pas annulé, le workflow progresse en déclenchant le [ `Authenticate` événement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx). S’il existe un gestionnaire d’événements pour le `Authenticate` événement, il est chargé de déterminer si les informations d’identification fournies sont valides ou non. Si aucun gestionnaire d’événements n’est spécifié, le contrôle de connexion utilise le `Membership.ValidateUser` méthode pour déterminer la validité des informations d’identification.

Si les informations d’identification fournies sont valides, le ticket d’authentification par formulaires est créé, le [ `LoggedIn` événement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggedin.aspx) est déclenché, et l’utilisateur est redirigé vers la page appropriée. Si, toutefois, les informations d’identification sont considérés comme non valides, puis le [ `LoginError` événement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loginerror.aspx) est déclenché et un message s’affiche pour informer l’utilisateur que leurs informations d’identification n’étaient pas valides. Par défaut, en cas d’échec de la connexion contrôle définit simplement son `FailureText` étiqueter la propriété de texte du contrôle à un message d’échec (votre tentative de connexion n’a pas réussi. Veuillez essayer à nouveau). Toutefois, si le contrôle Login [ `FailureAction` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failureaction.aspx) a la valeur `RedirectToLoginPage`, puis les problèmes de contrôle de connexion un `Response.Redirect` vers la page de connexion ajoutant le paramètre querystring `loginfailure=1` (ce qui entraîne l’ouverture de session contrôle pour afficher le message d’échec).

Figure 9 offre un organigramme du flux de travail de l’authentification.


[![Flux de travail du contrôle de la connexion d’authentification](validating-user-credentials-against-the-membership-user-store-cs/_static/image26.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image25.png)

**Figure 9**: flux de travail du contrôle connexion d’authentification ([cliquez pour afficher l’image en taille réelle](validating-user-credentials-against-the-membership-user-store-cs/_static/image27.png))


> [!NOTE]
> Si vous vous demandez quand vous utiliseriez le `FailureAction`de `RedirectToLogin` option page, considérez le scénario suivant. Dès maintenant notre `Site.master` page maître a actuellement le texte Hello, stranger affiché dans la colonne de gauche quand consultées par un utilisateur anonyme, mais imaginez que nous souhaitons remplacer ce texte avec un contrôle de connexion. Cela permettrait à un utilisateur anonyme pour vous connecter à partir de n’importe quelle page sur le site, au lieu de demander à consulter la page de connexion directement. Toutefois, si un utilisateur n’a pas pu se connecter via le contrôle de connexion affiché par la page maître, il peut être utile pour les rediriger vers la page de connexion (`Login.aspx`), car cette page probable comprend des instructions supplémentaires, les liens et les autres aide - tels que des liens pour créer un nouveau compte ou récupérer un mot de passe perdu - qui n’ont pas été ajoutés à la page maître.


### <a name="creating-theauthenticateevent-handler"></a>Création de la`Authenticate`Gestionnaire d’événements

Pour incorporer notre logique d’authentification personnalisée, nous devons créer un gestionnaire d’événements pour le contrôle Login `Authenticate` événement. Création d’un gestionnaire d’événements pour le `Authenticate` événement génère la définition de gestionnaire d’événements suivantes:

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample3.cs)]

Comme vous pouvez le voir, le `Authenticate` un objet de type est passé au gestionnaire d’événements [ `AuthenticateEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.authenticateeventargs.aspx) en tant que son deuxième paramètre d’entrée. Le `AuthenticateEventArgs` classe contient une propriété booléenne nommée `Authenticated` qui est utilisé pour spécifier si les informations d’identification fournies sont valides. Notre tâche, est ensuite, pour écrire du code ici qui détermine si les informations d’identification fournies sont valides ou non et pour définir le `e.Authenticate` propriété en conséquence.

### <a name="determining-and-validating-the-supplied-credentials"></a>Détermination et valider les informations d’identification fournies

Utiliser le contrôle Login [ `UserName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.username.aspx) et [ `Password` propriétés](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.password.aspx) pour déterminer les informations d’identification de nom d’utilisateur et mot de passe entrées par l’utilisateur. Afin de déterminer les valeurs entrées dans les contrôles Web supplémentaires (telles que la `Email` TextBox, nous avons ajouté à l’étape précédente), utilisez *`LoginControlID`* `.FindControl`(« *`controlID`* ») pour obtenir par programmation font référence au contrôle Web dans le modèle dont `ID` propriété est égale à *`controlID`*. Par exemple, pour obtenir une référence à la `Email` zone de texte, utilisez le code suivant :

`TextBox EmailTextBox = myLogin.FindControl("Email") as TextBox;`

Afin de valider les informations d’identification de l’utilisateur, nous devons faire deux choses :

1. Vérifiez que le nom d’utilisateur fourni et un mot de passe sont valides
2. Vérifiez qu’adresse e-mail entrée correspond à l’adresse de messagerie sur fichier pour l’utilisateur tente de se connecter

Pour accomplir la première vérification, nous pouvons simplement utiliser le `Membership.ValidateUser` (méthode), comme nous l’avons vu à l’étape 1. Pour le second contrôle, nous devons déterminer l’adresse de messagerie de l’utilisateur afin que nous pouvons le comparer à l’adresse de messagerie qu'ils entrés dans le contrôle de zone de texte. Pour obtenir des informations sur un utilisateur particulier, utilisez le `Membership` la classe [ `GetUser` méthode](https://msdn.microsoft.com/library/system.web.security.membership.getuser.aspx).

Le `GetUser` méthode a un nombre de surcharges. Si utilisée sans passer tous les paramètres, il retourne des informations sur l’utilisateur actuellement connecté. Pour obtenir des informations sur un utilisateur particulier, appelez `GetUser` en passant son nom d’utilisateur. Dans les deux cas, `GetUser` retourne un [ `MembershipUser` objet](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx), qui a des propriétés telles que `UserName`, `Email`, `IsApproved`, `IsOnline`, et ainsi de suite.

Le code suivant implémente ces deux contrôles. Si les deux réussissent, puis `e.Authenticate` a la valeur `true`, sinon elle est affectée `false`.

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample4.cs)]

Avec ce code en place, tentent de se connecter en tant qu’un utilisateur valide, en entrant le nom d’utilisateur correct, le mot de passe et l’adresse de messagerie. Essayez de nouveau, mais cette fois utiliser délibérément une adresse e-mail incorrecte (voir Figure 10). Enfin, essayez une troisième fois à l’aide d’un nom d’utilisateur inexistantes. Dans le premier cas vous devez être connecté avec succès vers le site, mais dans les deux derniers cas vous devez voir le message d’informations d’identification non valide du contrôle de la connexion.


[![Tito ne peut pas se connecter lorsque vous fournissez une adresse E-mail incorrecte](validating-user-credentials-against-the-membership-user-store-cs/_static/image29.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image28.png)

**Figure 10**: Tito ne peut pas Log dans lorsque en fournissant une adresse E-mail incorrecte ([cliquez pour afficher l’image en taille réelle](validating-user-credentials-against-the-membership-user-store-cs/_static/image30.png))


> [!NOTE]
> Comme indiqué dans la section de la façon dont l’appartenance Framework gère non valide tentatives de connexion à l’étape 1, lorsque le `Membership.ValidateUser` méthode est appelée et reçoit des informations d’identification non valides, il effectue le suivi de la tentative de connexion non valide et verrouille l’utilisateur si elles dépassent un certain seuil de tentatives non valides dans une fenêtre de temps spécifié. Depuis notre logique d’authentification personnalisée appelle le `ValidateUser` (méthode), un mot de passe incorrect pour un nom d’utilisateur valide incrémente le compteur de tentatives de connexion non valide, mais ce compteur n’est pas incrémenté dans le cas où le nom d’utilisateur et le mot de passe sont valides, mais le adresse de messagerie est incorrecte. Il est probable que sont, ce comportement est approprié, dans la mesure où il est peu probable qu’un pirate doit connaître le nom d’utilisateur et le mot de passe, mais devez utiliser des techniques de force brute pour déterminer l’adresse de messagerie de l’utilisateur.


## <a name="step-4-improving-the-login-controls-invalid-credentials-message"></a>Étape 4 : Amélioration de Message d’informations d’identification non valide du contrôle de connexion

Lorsqu’un utilisateur tente de se connecter avec les informations d’identification non valides, le contrôle de connexion affiche un message expliquant que la tentative de connexion a échoué. En particulier, le contrôle affiche le message spécifié par son [ `FailureText` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failuretext.aspx), qui a la valeur par défaut de votre tentative de connexion n’a pas réussi. Réessayez.

Souvenez-vous qu’il existe de nombreuses raisons, pourquoi les informations d’identification d’un utilisateur peuvent être non valides :

- Le nom d’utilisateur n’existe pas
- Le nom d’utilisateur existe, mais le mot de passe n’est pas valide
- Le nom d’utilisateur et le mot de passe sont valides, mais l’utilisateur n’est pas encore approuvée.
- Le nom d’utilisateur et le mot de passe sont valides, mais l’utilisateur est verrouillé hors (très probablement parce qu’il a dépassé le nombre de tentatives de connexion non valide dans l’intervalle de temps spécifié)

Et peut avoir d’autres raisons lorsque vous utilisez la logique d’authentification personnalisée. Par exemple, avec le code que nous avons écrit à l’étape 3, le nom d’utilisateur et mot de passe est valide, mais l’adresse de messagerie est peut-être incorrecte.

Quelle que soit la raison pour laquelle les informations d’identification ne sont pas valides, le contrôle de connexion affiche le même message d’erreur. Ce manque de commentaires peut prêter à confusion pour un utilisateur dont le compte n’a pas encore été approuvé ou qui a été verrouillé. Avec un peu de travail, cependant, nous pouvons avoir le contrôle de connexion affiche un message plus approprié.

Chaque fois qu’un utilisateur tente de se connecter avec des informations d’identification non valides, le contrôle Login déclenche son `LoginError` événement. Créer un gestionnaire d’événements pour cet événement et ajoutez le code suivant :

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample5.cs)]

Le code ci-dessus commence en définissant le contrôle Login `FailureText` propriété la valeur par défaut (votre tentative de connexion n’a pas réussi. Veuillez essayer à nouveau). Il vérifie ensuite si le nom d’utilisateur fourni est mappé à un compte d’utilisateur existant. Si, par conséquent, il consulte résultant `MembershipUser` l’objet `IsLockedOut` et `IsApproved` propriétés pour déterminer si le compte a été verrouillé ou n’a pas encore été approuvé. Dans les deux cas, le `FailureText` propriété est mise à jour en une valeur correspondante.

Pour tester ce code, volontairement tentative connectez-vous en tant qu’un utilisateur existant, mais utiliser un mot de passe incorrect. Effectuez cette opération cinq fois dans une ligne dans un délai de 10 minutes et le compte est verrouillé. Comme la Figure 11 montre, la suite d’une connexion tentatives seront toujours (même avec le mot de passe), mais ne maintenant affiche plus descriptif votre compte a été verrouillé en raison de trop de tentatives de connexion non valide. Veuillez contacter l’administrateur pour que votre message de déverrouillage de compte.


[![Tito effectué trop de tentatives de connexion non valide et a été verrouillé](validating-user-credentials-against-the-membership-user-store-cs/_static/image32.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image31.png)

**Figure 11**: Tito effectué trop nombreuses tentatives non valides connexion et a été verrouillé ([cliquez pour afficher l’image en taille réelle](validating-user-credentials-against-the-membership-user-store-cs/_static/image33.png))


## <a name="summary"></a>Récapitulatif

Préalable ce didacticiel, notre page de connexion validé les informations d’identification fournies par rapport à une liste codée en dur de paires de nom d’utilisateur/mot de passe. Dans ce didacticiel, nous avons mis à jour la page pour valider les informations d’identification par rapport à l’infrastructure de l’appartenance. À l’étape 1 que nous avons étudié à l’aide de la `Membership.ValidateUser` méthode par programmation. À l’étape 2, nous avons remplacé notre interface utilisateur créé manuellement et le code avec le contrôle Login.

Le contrôle de connexion affiche une interface utilisateur de connexion standard et valide automatiquement les informations d’identification de l’utilisateur par rapport à l’infrastructure de l’appartenance. En outre, en cas d’informations d’identification valides, le contrôle de connexion connecte l’utilisateur via l’authentification par formulaire. En bref, une expérience utilisateur de connexion entièrement fonctionnel est disponible en faisant simplement glisser le contrôle de connexion sur une page, aucun balisage déclaratif supplémentaire ou le code nécessaire. Plus, le contrôle de connexion est hautement personnalisable, ce qui permet un contrôle sur les deux le rendu utilisateur interface logique d’authentification et affiné.

À ce stade les visiteurs de notre site Web peuvent créer un nouveau compte d’utilisateur et le journal dans le site, mais nous devons encore à restreindre l’accès aux pages basées sur l’utilisateur authentifié. Actuellement, aucun utilisateur, authentifié ou anonyme, peut afficher n’importe quelle page sur notre site. Ainsi que de contrôler l’accès aux pages de notre site sur une base par utilisateur, nous pourrions avoir certaines pages dont la fonctionnalité dépend de l’utilisateur. Le didacticiel suivant explique comment limiter l’accès et des fonctionnalités de dans la page en fonction de l’utilisateur connecté.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [Affichage des Messages personnalisés verrouillé et les utilisateurs Non approuvés](http://aspnet.4guysfromrolla.com/articles/050306-1.aspx)
- [Examen d’ASP.NET de 2.0 l’appartenance, rôles et profil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Comment : Créer une Page de connexion ASP.NET](https://msdn.microsoft.com/library/ms178331.aspx)
- [Documentation technique de contrôle de connexion](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx)
- [L’utilisation des contrôles de connexion](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/login.aspx)

### <a name="about-the-author"></a>À propos de l’auteur

Scott Mitchell, auteur de plusieurs livres sur ASP/ASP.NET et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est *[SAM animer vous-même ASP.NET 2.0 des dernières 24 heures](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott peut être atteint à [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou via son blog à [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Les réviseurs tête pour ce didacticiel ont été Teresa Murphy et Michael Olivero. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Précédent](creating-user-accounts-cs.md)
> [Suivant](user-based-authorization-cs.md)
