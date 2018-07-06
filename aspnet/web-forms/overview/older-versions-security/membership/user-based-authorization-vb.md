---
uid: web-forms/overview/older-versions-security/membership/user-based-authorization-vb
title: Autorisation basée sur l’utilisateur (VB) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous examinerons limitant l’accès aux pages et en limitant les fonctionnalités de niveau de la page par le biais de diverses techniques.
ms.author: aspnetcontent
ms.date: 01/18/2008
ms.assetid: bc937e9d-5c14-4fc4-aec7-440da924dd18
msc.legacyurl: /web-forms/overview/older-versions-security/membership/user-based-authorization-vb
msc.type: authoredcontent
ms.openlocfilehash: 107983494350ddc06b6d3a20557baff4f4e6f9f4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834372"
---
<a name="user-based-authorization-vb"></a>Autorisation basée sur l’utilisateur (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_07_VB.zip) ou [télécharger le PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial07_UserAuth_vb.pdf)

> Dans ce didacticiel, nous examinerons limitant l’accès aux pages et en limitant les fonctionnalités de niveau de la page par le biais de diverses techniques.


## <a name="introduction"></a>Introduction

La plupart des applications web qui offrent des comptes d’utilisateur le faire dans la partie pour restreindre certains visiteurs d’accéder à certaines pages au sein du site. Dans les sites messageboard plus en ligne, par exemple, tous les utilisateurs - anonymes ou authentifiés - sont en mesure d’afficher les billets de le messageboard, mais seuls les utilisateurs authentifiés peuvent visiter la page web pour créer un nouveau billet. Et il peut y avoir des pages d’administration qui sont uniquement accessibles à un utilisateur particulier (ou un ensemble particulier d’utilisateurs). En outre, les fonctionnalités de niveau de la page peuvent être différent sur une base utilisateur par l’utilisateur. Lorsque vous affichez une liste des publications, les utilisateurs authentifiés sont affichés une interface d’évaluation de chaque publication, tandis que cette interface n’est pas disponible pour les visiteurs anonymes.

ASP.NET rend plus facile de définir des règles d’autorisation basées sur l’utilisateur. Avec simplement un peu de balisage dans `Web.config`, des pages web spécifiques ou des répertoires entiers peuvent être verrouillés afin qu’ils soient uniquement accessibles à un sous-ensemble spécifié d’utilisateurs. Fonctionnalités de niveau de la page peuvent être activée ou désactivée en fonction de l’utilisateur actuellement connecté par des moyens par programmation et déclaration.

Dans ce didacticiel, nous examinerons limitant l’accès aux pages et en limitant les fonctionnalités de niveau de la page par le biais de diverses techniques. C’est parti !

## <a name="a-look-at-the-url-authorization-workflow"></a>Examinons le flux de travail d’autorisation URL

Comme indiqué dans le [ *une vue d’ensemble de l’authentification par formulaire* ](../introduction/an-overview-of-forms-authentication-vb.md) (didacticiel), lorsque le runtime ASP.NET traite une demande pour une ressource ASP.NET la demande déclenche un nombre d’événements pendant son cycle de vie. *Les Modules HTTP* sont des classes managées dont le code est exécuté en réponse à un événement particulier dans le cycle de vie de demande. ASP.NET est livré avec un nombre de Modules HTTP qui effectuent des tâches essentielles en arrière-plan.

Un de ces modules HTTP est [ `FormsAuthenticationModule` ](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx). Comme indiqué dans les didacticiels précédents, la fonction principale de la `FormsAuthenticationModule` consiste à déterminer l’identité de la requête actuelle. Cela s’effectue en inspectant le ticket d’authentification forms, qui est situé dans un cookie ou incorporé dans l’URL. Cette identification a lieu pendant la [ `AuthenticateRequest` événement](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

Un autre HTTP Module important est le [ `UrlAuthorizationModule` ](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx), qui est déclenché en réponse à la [ `AuthorizeRequest` événement](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx) (ce qui se produit après le `AuthenticateRequest` événement). Le `UrlAuthorizationModule` examine le balisage de configuration dans `Web.config` pour déterminer si l’identité actuelle a autorité pour visiter la page spécifiée. Ce processus est appelé *l’autorisation d’URL*.

Nous allons examiner la syntaxe pour les règles d’autorisation d’URL à l’étape 1, mais tout d’abord nous allons voir à quoi le `UrlAuthorizationModule` est selon si la demande est autorisée ou non. Si le `UrlAuthorizationModule` détermine que la demande est autorisée, elle ne fait rien, puis la demande continue via son cycle de vie. Toutefois, si la demande est *pas* autorisé, puis le `UrlAuthorizationModule` abandonne le cycle de vie et indique à la `Response` objet à retourner un [HTTP 401 non autorisé](http://www.checkupdown.com/status/E401.html) état. Lorsque vous utilisez l’authentification par formulaire cet état HTTP 401 n’est jamais retourné au client, car si le `FormsAuthenticationModule` détecte un état est de HTTP 401 modifie un [HTTP 302 redirection](http://www.checkupdown.com/status/E302.html) à la page de connexion.

La figure 1 illustre le flux de travail du pipeline ASP.NET, le `FormsAuthenticationModule`et le `UrlAuthorizationModule` lors de l’arrivée une demande non autorisée. En particulier, la Figure 1 illustre une demande par un visiteur anonyme pour `ProtectedPage.aspx`, qui est une page qui refuse l’accès aux utilisateurs anonymes. Dans la mesure où le visiteur est anonyme, le `UrlAuthorizationModule` abandonne la demande et retourne un état HTTP 401 non autorisé. Le `FormsAuthenticationModule` puis convertit l’état 401 une redirection 302 à la page de connexion. Une fois que l’utilisateur est authentifié par le biais de la page de connexion, il est redirigé vers `ProtectedPage.aspx`. Cette fois le `FormsAuthenticationModule` identifie l’utilisateur en fonction de son ticket d’authentification. Maintenant que le visiteur est authentifié, le `UrlAuthorizationModule` permet l’accès à la page.


[![L’authentification par formulaire et le Workflow de l’autorisation d’URL](user-based-authorization-vb/_static/image2.png)](user-based-authorization-vb/_static/image1.png)

**Figure 1**: l’authentification de formulaires et flux de travail URL d’autorisation ([cliquez pour afficher l’image en taille réelle](user-based-authorization-vb/_static/image3.png))


Figure 1 illustre l’interaction qui se produit lorsqu’un visiteur anonyme tente d’accéder à une ressource qui n’est pas disponible aux utilisateurs anonymes. Dans ce cas, le visiteur anonyme est redirigé vers la page de connexion avec la page, qu'elle a essayé de visiter spécifié dans la chaîne de requête. Une fois que l’utilisateur a connecté avec succès, elle sera automatiquement redirigée vers la ressource, qu'elle a initialement tenté d’afficher.

Lorsque la demande non autorisée est effectuée par un utilisateur anonyme, ce flux de travail est simple et qu’il est facile pour le visiteur de comprendre ce qui est arrivé et pourquoi. Mais gardez à l’esprit que le `FormsAuthenticationModule` redirigera *n’importe quel* non autorisé de l’utilisateur vers la page de connexion, même si la demande est effectuée par un utilisateur authentifié. Cela peut entraîner une expérience utilisateur à confusion si un utilisateur authentifié tente de visiter une page pour laquelle elle ne dispose pas d’autorité.

Imaginez que notre site Web avait ses règles d’autorisation d’URL configurés telles que la page ASP.NET `OnlyTito.aspx` était accessible uniquement aux Tito. À présent, imaginez que Sam visite le site, ouvre une session et tente ensuite de visiter `OnlyTito.aspx`. Le `UrlAuthorizationModule` arrêtera le cycle de vie de requête et retourner un état HTTP 401 non autorisé, ce qui le `FormsAuthenticationModule` détectera et Sam pour rediriger vers la page de connexion. Étant donné que Sam est déjà connecté, cependant, elle a peut-être vous demandez-vous pourquoi elle a été envoyée à la page de connexion. Elle peut analyser que ses informations d’identification de connexion ont été perdues d’une certaine manière, ou qu’il a entré les informations d’identification non valides. Si Sam entrent à nouveau dans ses informations d’identification à partir de la page de connexion qu’elle est consignée (nouveau) et redirigée vers `OnlyTito.aspx`. Le `UrlAuthorizationModule` détectera que Sam ne peut pas visitez cette page, et elle s’affichera à la page de connexion.

La figure 2 illustre ce flux de travail à confusion.


[![Le flux de travail par défaut peut entraîner un Cycle à confusion](user-based-authorization-vb/_static/image5.png)](user-based-authorization-vb/_static/image4.png)

**Figure 2**: The par défaut du flux de travail peut entraîner un Cycle à confusion ([cliquez pour afficher l’image en taille réelle](user-based-authorization-vb/_static/image6.png))


Le workflow illustré en Figure 2 permettre befuddle rapidement même la plupart des ordinateurs expérimenté visiteur. Nous allons examiner les façons d’éviter ce problème déroutant cycle à l’étape 2.

> [!NOTE]
> ASP.NET utilise deux mécanismes pour déterminer si l’utilisateur actuel peut accéder à une page web particulière : autorisation d’URL et l’autorisation de fichier. Autorisation de fichier est implémentée par le [ `FileAuthorizationModule` ](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx), qui détermine l’autorité en consultant les ou les fichiers requis ACL. Autorisation de fichier est plus couramment utilisée avec l’authentification Windows, car les ACL sont les autorisations qui s’appliquent aux comptes de Windows. Lorsque vous utilisez l’authentification par formulaire, toutes les demandes d’au niveau du système de système d’exploitation et de fichier sont exécutées par le même compte Windows, quel que soit l’utilisateur visite le site. Étant donné que cette série de didacticiels se concentre sur l’authentification par formulaire, nous aborderons pas l’autorisation de fichier.


### <a name="the-scope-of-url-authorization"></a>L’étendue d’autorisation d’URL

Le `UrlAuthorizationModule` est code managé qui fait partie du runtime ASP.NET. Avant la version 7 de Microsoft [Internet Information Services (IIS)](https://www.iis.net/) serveur web, il a été une barrière distincte entre le pipeline HTTP de d’IIS et le pipeline du runtime ASP.NET. En bref, dans IIS 6 et versions antérieures, ASP. NET `UrlAuthorizationModule` s’exécute uniquement quand une demande est déléguée à partir d’IIS pour le runtime ASP.NET. Par défaut, IIS traite le contenu statique, telles que des pages HTML et CSS, JavaScript et les fichiers image - et transmet uniquement les demandes pour le runtime ASP.NET lorsqu’une page avec une extension de `.aspx`, `.asmx`, ou `.ashx` est demandée.

IIS 7, toutefois, permet intégré IIS et ASP.NET pipelines. Avec quelques paramètres de configuration, vous pouvez configurer IIS 7 pour appeler le `UrlAuthorizationModule` pour *tous les* demandes, ce qui signifie que les règles d’autorisation d’URL peuvent être définies pour les fichiers de n’importe quel type. En outre, IIS 7 inclut son propre moteur d’autorisation d’URL. Pour plus d’informations sur l’intégration d’ASP.NET et les fonctionnalités d’autorisation URL native d’IIS 7, consultez [comprendre l’autorisation d’URL IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization). Pour une présentation plus approfondie à l’intégration d’ASP.NET et IIS 7, procurez-vous une copie du livre de Shahram Khosravi *Professional IIS 7 et programmation intégré ASP.NET* (ISBN : 978-0470152539).

En bref, dans les versions antérieures d’IIS 7, règles d’autorisation d’URL sont appliquées uniquement aux ressources gérées par le runtime ASP.NET. Mais avec IIS 7, il est possible d’utiliser native fonctionnalité de l’autorisation d’URL d’IIS ou intégrer ASP. NET `UrlAuthorizationModule` dans le pipeline HTTP de d’IIS, étendant ainsi cette fonctionnalité pour toutes les demandes.

> [!NOTE]
> Il existe quelques différences subtiles mais importantes sur la façon d’ASP. NET `UrlAuthorizationModule` et fonctionnalité de l’autorisation d’URL IIS 7 traiter les règles d’autorisation. Ce didacticiel n’examine pas les fonctionnalités d’autorisation d’URL IIS 7 ou les différences dans la façon dont il analyse les règles d’autorisation par rapport à la `UrlAuthorizationModule`. Pour plus d’informations sur ces sujets, reportez-vous à la documentation de IIS 7 sur MSDN ou au [www.iis.net](https://www.iis.net/).


## <a name="step-1-defining-url-authorization-rules-inwebconfig"></a>Étape 1 : Définition des règles d’autorisation d’URL dans`Web.config`

Le `UrlAuthorizationModule` détermine s’il faut accorder ou refuser l’accès à une ressource demandée pour une identité particulière en fonction des règles d’autorisation URL définis dans la configuration de l’application. Les règles d’autorisation sont précisés dans le [ `<authorization>` élément](https://msdn.microsoft.com/library/8d82143t.aspx) sous la forme de `<allow>` et `<deny>` éléments enfants. Chaque `<allow>` et `<deny>` élément enfant peut spécifier :

- Un utilisateur particulier
- Une liste délimitée par des virgules des utilisateurs
- Tous les utilisateurs anonymes, indiqués par un point d’interrogation ( ?)
- Tous les utilisateurs, indiqués par un astérisque (\*)

Le balisage suivant illustre comment utiliser les règles d’autorisation d’URL pour autoriser les utilisateurs Tito et Scott et refuser tous les autres :

[!code-xml[Main](user-based-authorization-vb/samples/sample1.xml)]

Le `<allow>` élément définit quels utilisateurs sont autorisés, Tito et Scott, tandis que le `<deny>` élément indique que *tous les* refusé aux utilisateurs.

> [!NOTE]
> Le `<allow>` et `<deny>` éléments peuvent également spécifier des règles d’autorisation pour les rôles. Nous allons examiner en fonction du rôle d’autorisation dans un futur didacticiel.


Le paramètre suivant accorde l’accès à toute personne autre que Sam (y compris les visiteurs anonymes) :

[!code-xml[Main](user-based-authorization-vb/samples/sample2.xml)]

Pour autoriser uniquement les utilisateurs authentifiés, utilisez la configuration suivante, qui refuse l’accès à tous les utilisateurs anonymes :

[!code-xml[Main](user-based-authorization-vb/samples/sample3.xml)]

Les règles d’autorisation sont définis dans le `<system.web>` élément `Web.config` et s’appliquent à toutes les ressources dans l’application web ASP.NET. Souvent, une application a différentes règles d’autorisation pour les différentes sections. Par exemple, sur un site de commerce électronique, tous les visiteurs peuvent consulter les produits, consultez les évaluations de produits, recherche dans le catalogue et ainsi de suite. Toutefois, seuls les utilisateurs authentifiés peuvent atteindre la récupération ou les pages pour gérer l’historique de l’expédition. En outre, il peut exister des portions du site qui sont uniquement accessibles aux utilisateurs de sélectionner, tels que les administrateurs de site.

ASP.NET rend plus facile définir des règles d’autorisation différents pour différents fichiers et dossiers dans le site. Les règles d’autorisation spécifiés dans le dossier racine `Web.config` fichier s’appliquent à toutes les ressources ASP.NET dans le site. Toutefois, ces paramètres d’autorisation par défaut peuvent être remplacés pour un dossier spécifique en ajoutant un `Web.config` avec un `<authorization>` section.

Nous allons mettre à jour de notre site Web afin que seuls les utilisateurs authentifiés peuvent visiter les pages ASP.NET dans le `Membership` dossier. Pour cela nous devons ajouter un `Web.config` de fichiers à la `Membership` dossier et définissez ses paramètres d’autorisation Refuser à des utilisateurs anonymes. Cliquez sur le `Membership` dossier dans l’Explorateur de solutions, choisissez le menu Ajouter un nouvel élément dans le menu contextuel, ajoutez un nouveau fichier de Configuration Web nommé `Web.config`.


[![Ajouter un fichier Web.config dans le dossier de l’appartenance](user-based-authorization-vb/_static/image8.png)](user-based-authorization-vb/_static/image7.png)

**Figure 3**: ajouter un `Web.config` de fichiers à la `Membership` dossier ([cliquez pour afficher l’image en taille réelle](user-based-authorization-vb/_static/image9.png))


À ce stade votre projet doit contenir deux `Web.config` fichiers : un dans le répertoire racine et l’autre dans le `Membership` dossier.


[![Votre Application doit maintenant contenir deux fichiers Web.config](user-based-authorization-vb/_static/image11.png)](user-based-authorization-vb/_static/image10.png)

**Figure 4**: votre Application doit maintenant contenir deux `Web.config` fichiers ([cliquez pour afficher l’image en taille réelle](user-based-authorization-vb/_static/image12.png))


Mettre à jour le fichier de configuration dans le `Membership` dossier afin qu’il interdit l’accès aux utilisateurs anonymes.

[!code-xml[Main](user-based-authorization-vb/samples/sample4.xml)]

C’est tout cela !

Pour tester cette modification, visitez la page d’accueil dans un navigateur et assurez-vous que vous déconnecte. Étant donné que le comportement par défaut d’une application ASP.NET consiste à autoriser tous les visiteurs, et étant donné que nous n’avez pas apporter de modifications d’autorisation dans le répertoire racine `Web.config` fichier, nous sommes en mesure de visiter les fichiers dans le répertoire racine en tant qu’un visiteur anonyme.

Cliquez sur le lien de création de comptes d’utilisateur trouvé dans la colonne de gauche. Ceci vous dirigera vers le `~/Membership/CreatingUserAccounts.aspx`. Dans la mesure où le `Web.config` de fichiers dans le `Membership` dossier définit des règles d’autorisation pour interdire l’accès anonyme, le `UrlAuthorizationModule` abandonne la demande et retourne un état HTTP 401 non autorisé. Le `FormsAuthenticationModule` modifie cet aspect à un état de redirection 302, en nous envoyant à la page de connexion. Notez que la page nous étions tente d’accéder (`CreatingUserAccounts.aspx`) est passée à la page de connexion via le `ReturnUrl` paramètre querystring.


[![Depuis l’URL d’autorisation règles interdire l’accès anonyme, nous sommes redirigés vers la Page de connexion](user-based-authorization-vb/_static/image14.png)](user-based-authorization-vb/_static/image13.png)

**Figure 5**: étant donné que l’autorisation d’URL de règles d’empêcher l’accès anonyme, nous sommes redirigés vers la Page de connexion ([cliquez pour afficher l’image en taille réelle](user-based-authorization-vb/_static/image15.png))


Lors de la connexion avec succès, nous sommes redirigés vers le `CreatingUserAccounts.aspx` page. Cette fois le `UrlAuthorizationModule` autorise l’accès à la page, car nous ne sont plus anonymes.

### <a name="applying-url-authorization-rules-to-a-specific-location"></a>Application des règles d’autorisation d’URL à un emplacement spécifique

Les paramètres d’autorisation définis dans le `<system.web>` section de `Web.config` s’appliquent à toutes les ressources ASP.NET dans ce répertoire et ses sous-répertoires (jusqu'à ce que sinon remplacés par un autre `Web.config` fichier). Dans certains cas, cependant, nous souhaiterons peut-être toutes les ressources ASP.NET dans un répertoire donné pour avoir une configuration d’autorisation spécifique à l’exception d’une ou deux pages spécifiques. Cela est possible en ajoutant un `<location>` élément `Web.config`, qu’il pointe vers le fichier diffèrent dont les règles d’autorisation et de définition de ses règles d’autorisation unique dans celui-ci.

Pour illustrer l’utilisation de la `<location>` élément à remplacer les paramètres de configuration pour une ressource spécifique, nous allons personnaliser les paramètres d’autorisation afin que seuls Tito peut visiter `CreatingUserAccounts.aspx`. Pour ce faire, ajoutez un `<location>` élément à la `Membership` du dossier `Web.config` de fichiers et de mettre à jour de son balisage afin qu’il ressemble à ceci :

[!code-xml[Main](user-based-authorization-vb/samples/sample5.xml)]

Le `<authorization>` élément `<system.web>` définit les règles de l’autorisation d’URL par défaut pour les ressources ASP.NET dans le `Membership` dossier et ses sous-dossiers. Le `<location>` élément permet de remplacer ces règles pour une ressource particulière. Dans le balisage ci-dessus le `<location>` références de l’élément le `CreatingUserAccounts.aspx` page et spécifie les règles de son autorisation telle que Tito, mais refuser tout le monde.

Pour tester cette modification d’autorisation, démarrez en visitant le site Web en tant qu’utilisateur anonyme. Si vous tentez de visiter n’importe quelle page dans le `Membership` dossier, tel que `UserBasedAuthorization.aspx`, le `UrlAuthorizationModule` rejette la demande et vous êtes redirigé vers la page de connexion. Après vous être connecté en tant que, par exemple, Scott, vous pouvez visiter n’importe quelle page dans le `Membership` dossier *sauf* pour `CreatingUserAccounts.aspx`. Si vous tentez de visiter `CreatingUserAccounts.aspx` connecté en tant que tout le monde mais Tito entraîne une tentative d’accès non autorisé, vous rediriger vers la page de connexion.

> [!NOTE]
> Le `<location>` élément doit apparaître en dehors de la configuration `<system.web>` élément. Vous devez recourir à un `<location>` élément pour chaque ressource dont vous souhaitez remplacer les paramètres d’autorisation.


### <a name="a-look-at-how-theurlauthorizationmoduleuses-the-authorization-rules-to-grant-or-deny-access"></a>Examinons comment le`UrlAuthorizationModule`utilise les règles d’autorisation pour accorder ou refuser l’accès

Le `UrlAuthorizationModule` détermine si pour autoriser une identité particulière pour une URL particulière en analysant l’autorisation d’URL des règles à la fois, en commençant à partir du premier et l’utilisation de sa façon vers le bas. Dès qu’une correspondance est trouvée, l’utilisateur est accordé ou refusé l’accès, selon que la correspondance a été trouvée dans un `<allow>` ou `<deny>` élément. <strong>Si aucune correspondance n’est trouvée, l’utilisateur est autorisé à accéder.</strong> Par conséquent, si vous souhaitez restreindre l’accès, il est impératif d’utiliser un `<deny>` élément en tant que le dernier élément dans la configuration de l’autorisation d’URL. <strong>Si vous omettez un</strong><strong>`<deny>`</strong><strong>élément, tous les utilisateurs auront accès.</strong>

Pour mieux comprendre le processus utilisé par le `UrlAuthorizationModule` pour déterminer l’autorité, prenons l’exemple de règles d’autorisation d’URL nous avons vu plus haut dans cette étape. La première règle est un `<allow>` élément qui permet d’accéder à Tito et Scott. Les règles des deuxième est un `<deny>` élément qui refuse l’accès à tout le monde. Si un utilisateur anonyme visite, le `UrlAuthorizationModule` commence par demander, est anonyme Scott ou Tito ? La réponse est évidemment, non, il se poursuit pour la deuxième règle. Est anonyme dans l’ensemble de tout le monde ? Depuis la réponse ici est Oui, le `<deny>` règle est placée en vigueur et le visiteur est redirigé vers la page de connexion. De même, si la visite de Jisun, le `UrlAuthorizationModule` commence par demander, Jisun est Scott ou Tito ? Dans la mesure où elle n’est pas, le `UrlAuthorizationModule` passe à la deuxième question, est de Jisun dans l’ensemble de tout le monde ? Elle est, elle, aussi, l’accès est refusée. Enfin, si Tito visite, la première question que pose le `UrlAuthorizationModule` étant une réponse affirmative, Tito a accès.

Dans la mesure où le `UrlAuthorizationModule` processus l’autorisation de règles à partir du haut vers le bas, l’arrêt à n’importe quelle correspondance, il est important de disposer les règles plus spécifiques sont placés avant les moins spécifiques. Autrement dit, pour définir des règles d’autorisation qui interdisent Jisun et les utilisateurs anonymes, mais autoriser tous les autres utilisateurs authentifiés, vous commencez par la règle plus spécifique - Jisun qui ont un impact un - et puis passez aux règles moins spécifiques - ceux ce qui permet à tous les autres les utilisateurs authentifiés, mais refuser tous les utilisateurs anonymes. Les règles de d’autorisation d’URL suivants implémente cette stratégie en refusant tout d’abord Jisun et puis en refuser tout utilisateur anonyme. Tout utilisateur autre que Jisun authentifié est autorisé, car aucune de ces `<deny>` instructions correspondra.

[!code-xml[Main](user-based-authorization-vb/samples/sample6.xml)]

## <a name="step-2-fixing-the-workflow-for-unauthorized-authenticated-users"></a>Étape 2 : Correction du flux de travail pour les utilisateurs non autorisés, authentifiés

Comme expliqué précédemment dans ce didacticiel dans le A Examinez la section de Workflow de l’autorisation d’URL, à tout moment une demande non autorisée se passe réellement, le `UrlAuthorizationModule` abandonne la demande et retourne un état HTTP 401 non autorisé. Cet état 401 est modifié par le `FormsAuthenticationModule` dans un message 302 redirection état qui envoie l’utilisateur à la page de connexion. Ce flux de travail se produit sur une demande non autorisée, même si l’utilisateur est authentifié.

Retour d’un utilisateur authentifié à la page de connexion est susceptible de les confondre dans la mesure où ils ont déjà connecté au système. Avec un peu de travail, nous pouvons améliorer ce flux de travail en redirigeant les utilisateurs authentifiés effectuant des demandes non autorisées vers une page qui explique qu’ils ont essayé d’accéder à une page restreinte.

Commencez par créer une nouvelle page ASP.NET dans le dossier racine de l’application web nommé `UnauthorizedAccess.aspx`; n’oubliez pas d’associer cette page avec le `Site.master` page maître. Après avoir créé cette page, supprimez le contrôle de contenu qui référence le `LoginContent` ContentPlaceHolder afin que la page maître contenu par défaut s’affichera. Ensuite, ajoutez un message qui explique la situation, à savoir que l’utilisateur a tenté d’accéder à une ressource protégée. Après avoir ajouté un tel message, le `UnauthorizedAccess.aspx` balisage déclaratif de la page doit ressembler à ce qui suit :

[!code-aspx[Main](user-based-authorization-vb/samples/sample7.aspx)]

Nous devons maintenant modifier le flux de travail afin que si une demande non autorisée est effectuée par un utilisateur authentifié, ils sont envoyés à la `UnauthorizedAccess.aspx` page au lieu de la page de connexion. La logique qui redirige les demandes non autorisées vers la page de connexion est comprises dans une méthode privée de la `FormsAuthenticationModule` classe, afin que nous ne pouvons pas personnaliser ce comportement. Ce que nous pouvons faire, cependant, est d’ajouter notre propre logique vers la page de connexion qui redirige l’utilisateur vers `UnauthorizedAccess.aspx`, si nécessaire.

Lorsque le `FormsAuthenticationModule` redirige un visiteur non autorisé à la page de connexion, il ajoute l’URL demandée, non autorisé pour la chaîne de requête avec le nom `ReturnUrl`. Par exemple, si un utilisateur non autorisé a essayé de visiter `OnlyTito.aspx`, le `FormsAuthenticationModule` redirige leur `Login.aspx?ReturnUrl=OnlyTito.aspx`. Par conséquent, si la page de connexion est atteinte par un utilisateur authentifié avec une chaîne de requête qui inclut le `ReturnUrl` paramètre, puis nous savoir que cet utilisateur non authentifié a tenté de simplement à visiter une page, elle n’est pas autorisée à afficher. Dans ce cas, nous souhaitons lui permettent de rediriger `UnauthorizedAccess.aspx`.

Pour ce faire, ajoutez le code suivant à la page de connexion `Page_Load` Gestionnaire d’événements :

[!code-vb[Main](user-based-authorization-vb/samples/sample8.vb)]

Le code ci-dessus redirige les utilisateurs authentifiés, non autorisés au `UnauthorizedAccess.aspx` page. Pour voir cette logique en action, visitez le site comme un visiteur anonyme et cliquez sur le lien de création de comptes utilisateur dans la colonne de gauche. Ceci vous dirigera vers le `~/Membership/CreatingUserAccounts.aspx` page, qui, à l’étape 1, nous avons configuré uniquement autoriser l’accès à Tito. Étant donné que les utilisateurs anonymes sont interdites, le `FormsAuthenticationModule` nous redirige vers la page de connexion.

À ce stade, nous sommes anonymes, de sorte que `Request.IsAuthenticated` retourne `False` et nous n’avons pas redirigés vers `UnauthorizedAccess.aspx`. Au lieu de cela, la page de connexion s’affiche. Connectez-vous en tant qu’utilisateur autre que Tito, telles que Bruce. Après avoir entré les informations d’identification appropriées, la page de connexion nous redirige vers `~/Membership/CreatingUserAccounts.aspx`. Toutefois, étant donné que cette page est uniquement accessible au Tito, nous sont non autorisés pour l’afficher et sont directement retournés à la page de connexion. Cette fois, cependant, `Request.IsAuthenticated` retourne `True` (et le `ReturnUrl` paramètre querystring existe), de sorte que nous sommes redirigés vers le `UnauthorizedAccess.aspx` page.


[![Authentifié, les utilisateurs non autorisés sont redirigées vers UnauthorizedAccess.aspx](user-based-authorization-vb/_static/image17.png)](user-based-authorization-vb/_static/image16.png)

**Figure 6**: authentifié, les utilisateurs non autorisés sont redirigées vers `UnauthorizedAccess.aspx` ([cliquez pour afficher l’image en taille réelle](user-based-authorization-vb/_static/image18.png))


Ce flux de travail personnalisé présente une expérience utilisateur plus réalistes et simple illustré à la Figure 2 le cycle de court-circuit.

## <a name="step-3-limiting-functionality-based-on-the-currently-logged-in-user"></a>Étape 3 : Limitation de fonctionnalités en fonction de l’utilisateur actuellement connecté

Autorisation d’URL rend plus facile de spécifier des règles d’autorisation grossier. Comme nous l’avons vu à l’étape 1, avec l’autorisation d’URL nous pouvons de façon succincte les identités sont autorisées et celles qui est refusées à partir de l’affichage d’une page particulière ou toutes les pages dans un dossier. Dans certains scénarios, cependant, nous souhaiterons autoriser tous les utilisateurs à visiter une page, mais limiter les fonctionnalités de la page en fonction de l’utilisateur qu’il visite.

Prenons le cas d’un site Web de commerce électronique qui permet des visiteurs authentifiés passer en revue leurs produits. Lorsqu’un utilisateur anonyme visite les page du produit, ils verriez seulement les informations de produit et ne seraient pas être la possibilité laissent un commentaire. Toutefois, un utilisateur authentifié, visitez la page même verriez l’interface de révision. Si l’utilisateur authentifié n’avait pas encore révisé ce produit, l’interface leur permettant de soumettre une revue ; Sinon, il serait leur montrer leur révision envoyée précédemment. Pour passer ce scénario en outre, la page de produit peut afficher des informations supplémentaires et offrent des fonctionnalités étendues pour les utilisateurs qui fonctionnent pour l’entreprise de commerce électronique. Par exemple, la page de produit peut répertorier l’inventaire en stock et incluent des options pour modifier le prix du produit et une description quand consultées par un employé.

Ces règles d’autorisation de granularité fine peuvent être implémentées de façon déclarative ou par programmation (ou via une combinaison des deux). Dans la section suivante, nous verrons comment implémenter l’autorisation de granularité fine via le contrôle LoginView. Ensuite, nous explorerons les techniques de programmation. Avant que nous pouvons examiner d’application des règles d’autorisation précis, toutefois, nous devons d’abord créer une page dont la fonctionnalité dépend de l’utilisateur qu’il visite.

Nous allons créer une page qui répertorie les fichiers dans un répertoire particulier au sein d’un GridView. En même temps que la liste le nom de chaque fichier, de taille et d’autres informations, le contrôle GridView inclut deux colonnes de type LinkButton : un intitulé de vue et une suppression intitulée. Si l’utilisateur clique sur le LinkButton de vue, le contenu du fichier sélectionné est affiché ; Si l’utilisateur clique sur le LinkButton supprimer, le fichier sera supprimé. Nous allons créer initialement cette page de telle sorte que ses fonctionnalités d’affichage et de suppression soient disponible pour tous les utilisateurs. Utilisation les sections de contrôle LoginView et limitant par programme les fonctionnalités que nous verrons comment activer ou désactiver ces fonctionnalités en fonction de l’utilisateur accédant à la page.

> [!NOTE]
> La page ASP.NET que nous sommes sur le point de build utilise un contrôle GridView pour afficher une liste de fichiers. Depuis ce didacticiel série se concentre sur l’authentification par formulaire, l’autorisation, comptes d’utilisateurs et rôles, je ne souhaite pas consacrer trop longtemps décrivant le fonctionnement interne du contrôle GridView. Alors que ce didacticiel fournit des instructions spécifiques pour la configuration de cette page, il ne pas étudier en détail pourquoi certains choix ont été apportées, ou qu’ont les propriétés particulières d’effet sur la sortie rendue. Pour un examen approfondi du contrôle GridView, consultez mon *[utilisation des données dans ASP.NET 2.0](../../data-access/index.md)* série de didacticiels.


Commencez par ouvrir le `UserBasedAuthorization.aspx` de fichiers dans le `Membership` dossier et en ajoutant un contrôle GridView à la page nommée `FilesGrid`. À partir de la balise active le contrôle GridView, cliquez sur le lien Modifier les colonnes pour ouvrir la boîte de dialogue champs. À ce stade, désactivez les générer automatiquement des champs dans l’angle inférieur gauche. Ensuite, ajoutez un bouton Sélectionner un bouton Supprimer et deux BoundFields à partir de l’angle supérieur gauche (les boutons Select et Delete est accessible sous le type CommandField). Définir le bouton de sélection `SelectText` propriété pour afficher et de la première BoundField `HeaderText` et `DataField` propriétés au nom. Définir le BoundField deuxième `HeaderText` propriété à la taille en octets, ses `DataField` propriété de longueur, son `DataFormatString` propriété {0:N0} et son `HtmlEncode` False à la propriété.

Après avoir configuré les colonnes du contrôle GridView, cliquez sur OK pour fermer la boîte de dialogue champs. À partir de la fenêtre Propriétés, définissez le GridView `DataKeyNames` propriété `FullName`. À ce stade balisage déclaratif de GridView doit ressembler à ce qui suit :

[!code-aspx[Main](user-based-authorization-vb/samples/sample9.aspx)]

Balise le contrôle GridView étant créé, nous sommes prêts à écrire le code qui Récupère les fichiers dans un répertoire particulier et les lier au contrôle GridView. Ajoutez le code suivant à la page `Page_Load` Gestionnaire d’événements :

[!code-vb[Main](user-based-authorization-vb/samples/sample10.vb)]

Le code ci-dessus utilise la [ `DirectoryInfo` classe](https://msdn.microsoft.com/library/system.io.directoryinfo.aspx) pour obtenir une liste des fichiers dans le dossier racine de l’application. Le [ `GetFiles()` méthode](https://msdn.microsoft.com/library/system.io.directoryinfo.getfiles.aspx) retourne tous les fichiers dans le répertoire en tant que tableau de [ `FileInfo` objets](https://msdn.microsoft.com/library/system.io.fileinfo.aspx), qui est ensuite lié au GridView. Le `FileInfo` objet possède un large éventail de propriétés, telles que `Name`, `Length`, et `IsReadOnly`, entre autres. Comme vous pouvez le voir à partir de son balisage déclaratif, le contrôle GridView affiche simplement le `Name` et `Length` propriétés.

> [!NOTE]
> Le `DirectoryInfo` et `FileInfo` classes se trouvent dans le [ `System.IO` espace de noms](https://msdn.microsoft.com/library/system.io.aspx). Par conséquent, vous aurez besoin pour faire précéder ces noms de classes par leurs noms d’espace de noms ou disposer de l’espace de noms importé dans le fichier de classe (via `Imports System.IO`).


Prenez un moment pour consulter cette page via un navigateur. Il affichera la liste des fichiers qui résident dans le répertoire racine de l’application. Cliquez simplement sur la vue ou la supprimer de type LinkButton entraîne une publication (postback), mais aucune action ne se produit, car il nous faut encore pour créer les gestionnaires d’événements nécessaires.


[![Le contrôle GridView répertorie les fichiers dans le répertoire racine de l’Application Web](user-based-authorization-vb/_static/image20.png)](user-based-authorization-vb/_static/image19.png)

**Figure 7**: le contrôle GridView répertorie les fichiers dans le répertoire racine de l’Application Web ([cliquez pour afficher l’image en taille réelle](user-based-authorization-vb/_static/image21.png))


Nous avons besoin d’un moyen pour afficher le contenu du fichier sélectionné. Revenez à Visual Studio et ajouter une zone de texte nommée `FileContents` ci-dessus le GridView. Définissez ses `TextMode` propriété `MultiLine` et son `Columns` et `Rows` propriétés à 95 % et 10, respectivement.

[!code-aspx[Main](user-based-authorization-vb/samples/sample11.aspx)]

Ensuite, créez un gestionnaire d’événements pour le contrôle GridView [ `SelectedIndexChanged` événement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindexchanged.aspx) et ajoutez le code suivant :

[!code-vb[Main](user-based-authorization-vb/samples/sample12.vb)]

Ce code utilise le GridView `SelectedValue` propriété afin de déterminer le nom de fichier complet du fichier sélectionné. En interne, le `DataKeys` collection est référencée afin d’obtenir le `SelectedValue`, il est donc impératif que vous avez défini le GridView `DataKeyNames` propriété nom, comme décrit précédemment dans cette étape. Le [ `File` classe](https://msdn.microsoft.com/library/system.io.file.aspx) est utilisé pour lire le contenu du fichier sélectionné dans une chaîne, qui est ensuite assigné à la `FileContents` la zone de texte `Text` propriété, ce qui affiche le contenu du fichier sélectionné dans la page.


[![Contenu du fichier sélectionné est affiché dans la zone de texte](user-based-authorization-vb/_static/image23.png)](user-based-authorization-vb/_static/image22.png)

**Figure 8**: du contenu du fichier sélectionné The est affichés dans la zone de texte ([cliquez pour afficher l’image en taille réelle](user-based-authorization-vb/_static/image24.png))


> [!NOTE]
> Si vous affichez le contenu d’un fichier qui contient le balisage HTML, puis essayez d’afficher ou supprimer un fichier, vous recevrez un `HttpRequestValidationException` erreur. Cela se produit, car sur la publication (postback) le contenu de la zone de texte est envoyées sur le serveur web. Par défaut, ASP.NET génère un `HttpRequestValidationException` erreur chaque fois que le contenu de publication (postback) potentiellement dangereuse, telles que des balises HTML, est détecté. Pour désactiver cette erreur ne se produise, désactiver validation de la demande pour la page en ajoutant `ValidateRequest="false"` à la `@Page` directive. Pour plus d’informations sur les avantages de la validation de la demande en tant qu’ainsi que les précautions, vous devez prendre lorsque sa désactivation, consultez [Validation de demande - empêcher les attaques de Script](https://asp.net/learn/whitepapers/request-validation/).


Enfin, ajoutez un gestionnaire d’événements par le code suivant pour le contrôle GridView [ `RowDeleting` événement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx):

[!code-vb[Main](user-based-authorization-vb/samples/sample13.vb)]

Le code affiche simplement le nom complet du fichier à supprimer dans le `FileContents` zone de texte *sans* vraiment supprimer le fichier.


[![En cliquant sur le bouton de suppression ne supprime pas réellement le fichier](user-based-authorization-vb/_static/image26.png)](user-based-authorization-vb/_static/image25.png)

**Figure 9**: cliquant sur le supprimer bouton ne supprime ne pas réellement le fichier ([cliquez pour afficher l’image en taille réelle](user-based-authorization-vb/_static/image27.png))


À l’étape 1, nous avons configuré les règles de d’autorisation d’URL pour interdire aux utilisateurs anonymes à partir de l’affichage des pages dans le `Membership` dossier. Afin de mieux présenter l’authentification précis, nous allons autoriser les utilisateurs anonymes à visiter le `UserBasedAuthorization.aspx` page, mais avec des fonctionnalités limitées. Pour ouvrir cette page accessible par tous les utilisateurs, ajoutez le code suivant `<location>` élément à la `Web.config` de fichiers dans le `Membership` dossier :

[!code-xml[Main](user-based-authorization-vb/samples/sample14.xml)]

Après avoir ajouté cela `<location>` élément, tester les nouvelles règles de l’autorisation d’URL en vous connectant en dehors du site. En tant qu’utilisateur anonyme vous devez être autorisées à visiter le `UserBasedAuthorization.aspx` page.

Actuellement, tout utilisateur authentifié ou anonyme peut visiter le `UserBasedAuthorization.aspx` page et afficher ou supprimer des fichiers. Nous allons faire en sorte que seuls les utilisateurs authentifiés peuvent afficher le contenu d’un fichier et Tito uniquement peut supprimer un fichier. Ces règles d’autorisation de granularité fine applicables de manière déclarative, par programmation ou via une combinaison des deux méthodes. Nous allons utiliser l’approche déclarative pour limiter qui peut afficher le contenu d’un fichier. Nous allons utiliser l’approche par programmation pour limiter les utilisateurs permettre supprimer un fichier.

### <a name="using-the-loginview-control"></a>Utilisation du contrôle LoginView

Comme nous l’avons vu dans les didacticiels passées, le contrôle LoginView est utile pour afficher différentes interfaces pour les utilisateurs authentifiés et anonymes et offre un moyen aisé de masquer des fonctionnalités qui ne sont pas accessible aux utilisateurs anonymes. Étant donné que les utilisateurs anonymes ne peuvent pas afficher ou supprimer des fichiers, il nous suffit afficher le `FileContents` zone de texte lorsque la page est visitée par un utilisateur authentifié. Pour ce faire, ajoutez un contrôle LoginView à la page, nommez-le `LoginViewForFileContentsTextBox`et déplacer le `FileContents` balisage déclaratif de la zone de texte dans le contrôle LoginView `LoggedInTemplate`.

[!code-aspx[Main](user-based-authorization-vb/samples/sample15.aspx)]

Les contrôles Web dans les modèles de la vue de connexion ne sont plus directement accessibles à partir de la classe code-behind. Par exemple, le `FilesGrid` contrôle GridView `SelectedIndexChanged` et `RowDeleting` gestionnaires d’événements font actuellement référencent le `FileContents` comme contrôle de zone de texte avec le code :

[!code-aspx[Main](user-based-authorization-vb/samples/sample16.aspx)]

Toutefois, ce code n’est plus valide. En déplaçant le `FileContents` zone de texte dans le `LoggedInTemplate` la zone de texte ne sont pas accessibles directement. Au lieu de cela, nous devons utiliser la `FindControl("controlId")` méthode à référencer par programmation le contrôle. Mise à jour le `FilesGrid` gestionnaires d’événements pour faire référence à la zone de texte comme suit :

[!code-vb[Main](user-based-authorization-vb/samples/sample17.vb)]

Après avoir déplacé la zone de texte vers la vue de la connexion `LoggedInTemplate` et mise à jour de code de la page vers la zone de texte à l’aide de référence le `FindControl("controlId")` de modèle, visitez la page en tant qu’utilisateur anonyme. Comme le montre la Figure 10, la `FileContents` zone de texte n’est pas affiché. Toutefois, le LinkButton vue reste affiché.


[![Le contrôle LoginView affiche uniquement la zone de texte FileContents pour les utilisateurs authentifiés](user-based-authorization-vb/_static/image29.png)](user-based-authorization-vb/_static/image28.png)

**Figure 10**: The LoginView contrôle uniquement restitue le `FileContents` zone de texte pour les utilisateurs authentifiés ([cliquez pour afficher l’image en taille réelle](user-based-authorization-vb/_static/image30.png))


Un pour masquer le bouton d’affichage pour les utilisateurs anonymes consiste à convertir le champ GridView en TemplateField. Cela générera un modèle qui contient le balisage déclaratif pour le LinkButton de vue. Nous pouvons ajouter un contrôle LoginView au TemplateField contenu, puis placez le LinkButton au sein de la vue de la connexion `LoggedInTemplate`, d'où masquer le bouton de la vue à partir de visiteurs anonymes. Pour ce faire, cliquez sur le lien Modifier les colonnes dans de balise le contrôle GridView active pour lancer la boîte de dialogue champs. Ensuite, sélectionnez le bouton Sélectionner dans la liste dans l’angle inférieur gauche, puis sur la convertir ce champ en TemplateField lien. Cela modifiera le balisage déclaratif du champ à partir de :

[!code-aspx[Main](user-based-authorization-vb/samples/sample18.aspx)]

 À : 

[!code-aspx[Main](user-based-authorization-vb/samples/sample19.aspx)]

 À ce stade, nous pouvons ajouter un LoginView au TemplateField contenu. Le balisage suivant affiche le LinkButton vue uniquement pour les utilisateurs authentifiés. 

[!code-aspx[Main](user-based-authorization-vb/samples/sample20.aspx)]

Comme le montre la Figure 11, le résultat final n’est pas qu’assez que la vue colonne est toujours affichée même si le LinkButton de vue dans la colonne sont masqués. Nous allons examiner comment masquer la colonne de GridView entière (et pas seulement le LinkButton) dans la section suivante.


[![Le contrôle LoginView masque la vue de type LinkButton pour les visiteurs anonymes](user-based-authorization-vb/_static/image32.png)](user-based-authorization-vb/_static/image31.png)

**Figure 11**: le contrôle LoginView masque la vue de type LinkButton pour les visiteurs anonymes ([cliquez pour afficher l’image en taille réelle](user-based-authorization-vb/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>Limitation par programmation des fonctionnalités

Dans certains cas, les techniques déclaratives sont insuffisantes pour limitant les fonctionnalités à une page. Par exemple, la disponibilité de certaines fonctionnalités de la page peut dépendre critères au-delà si l’utilisateur accédant à la page est anonyme ou authentifié. Dans ce cas, les divers éléments d’interface utilisateur peuvent être affichés ou masqués moyens par programme.

Afin de limiter les fonctionnalités par programmation, nous devons effectuer deux tâches :

1. Déterminer si l’utilisateur accédant à la page peut accéder à la fonctionnalité, et
2. Modifier par programmation l’interface utilisateur selon que l’utilisateur a accès à la fonctionnalité en question.

Pour illustrer l’application de ces deux tâches, nous allons autoriser Tito supprimer des fichiers à partir de la GridView. Notre première tâche, est ensuite, pour déterminer s’il s’agit de Tito visitant la page. Une fois que qui a été déterminé, nous avons besoin masquer (ou afficher) supprimer une colonne de GridView. Les colonnes du contrôle GridView sont accessibles via son `Columns` propriété ; une colonne est rendue uniquement si son `Visible` propriété est définie sur `True` (la valeur par défaut).

Ajoutez le code suivant à la `Page_Load` Gestionnaire d’événements avant la liaison des données au GridView :

[!code-vb[Main](user-based-authorization-vb/samples/sample21.vb)]

Comme expliqué dans la [ *une vue d’ensemble de l’authentification par formulaire* ](../introduction/an-overview-of-forms-authentication-vb.md) didacticiel, `User.Identity.Name` retourne le nom de l’identité. Cela correspond au nom d’utilisateur entré dans le contrôle de connexion. S’il s’agit de Tito visite de la page, deuxième colonne du contrôle GridView `Visible` propriété est définie sur `True`; sinon, elle est définie sur `False`. Le résultat net est que lorsque quelqu'un d’autre que Tito visite la page, un autre utilisateur authentifié ou un utilisateur anonyme, la colonne de suppression n’est pas restituée (voir Figure 12) ; Toutefois, lorsque Tito consulte la page, la colonne de suppression est présente (voir Figure 13).


[![La colonne supprimer n’est pas restitué lorsque visité par quelqu'un d’autre que Tito (par exemple, Bruce)](user-based-authorization-vb/_static/image35.png)](user-based-authorization-vb/_static/image34.png)

**Figure 12**: supprimer la colonne l’est pas rendu lorsque visité par quelqu'un d’autre que Tito (par exemple, Bruce) ([cliquez pour afficher l’image en taille réelle](user-based-authorization-vb/_static/image36.png))


[![La colonne à supprimer est rendue pour Tito](user-based-authorization-vb/_static/image38.png)](user-based-authorization-vb/_static/image37.png)

**Figure 13**: la suppression d’une colonne est rendue pour Tito ([cliquez pour afficher l’image en taille réelle](user-based-authorization-vb/_static/image39.png))


## <a name="step-4-applying-authorization-rules-to-classes-and-methods"></a>Étape 4 : Appliquer des règles d’autorisation à des Classes et méthodes

À l’étape 3, nous interdit les utilisateurs anonymes d’afficher du contenu d’un fichier et interdit tous les utilisateurs mais Tito à partir de la suppression de fichiers. Cela a été accompli en masquant les éléments d’interface utilisateur associé pour les visiteurs non autorisés via des techniques déclaratives et par programme. Pour notre exemple, masquage correctement les éléments d’interface utilisateur était simple, mais qu’en est-il des sites plus complexes où il peut y avoir différentes manières d’effectuer les mêmes fonctionnalités ? Dans la limitation de cette fonctionnalité aux utilisateurs non autorisés, que se passe-t-il si nous oublions masquer ou désactiver tous les éléments d’interface utilisateur applicable ?

Un moyen simple pour vous assurer qu’une partie spécifique des fonctionnalités ne peut pas être accessible par un utilisateur non autorisé consiste à décorer cette classe ou une méthode avec le [ `PrincipalPermission` attribut](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx). Lorsque le runtime .NET utilise une classe ou exécute l’une de ses méthodes, il vérifie que le contexte de sécurité actuel est autorisé à utiliser la classe ou d’exécuter la méthode. Le `PrincipalPermission` attribut fournit un mécanisme par lequel nous pouvons définir ces règles.

Nous allons illustrer l’utilisation de la `PrincipalPermission` attribut sur le contrôle GridView `SelectedIndexChanged` et `RowDeleting` gestionnaires d’événements pour interdire l’exécution par les utilisateurs anonymes et les utilisateurs autres que Tito, respectivement. Tout nous devons faire est d’ajouter l’attribut approprié située en haut de chaque définition de fonction :

[!code-vb[Main](user-based-authorization-vb/samples/sample22.vb)]

L’attribut pour le `SelectedIndexChanged` contraintes de gestionnaire d’événements que seuls les utilisateurs authentifiés peuvent exécuter le Gestionnaire d’événements, alors que l’attribut sur le `RowDeleting` Gestionnaire d’événements limite l’exécution du Tito.

> [!NOTE]
> Un attribut peut être appliqué à une classe, méthode, propriété ou l’événement. Lorsque vous ajoutez un attribut, il doit faire partie de la classe, une méthode, une propriété ou une instruction de déclaration d’événement. Étant donné que Visual Basic utilise des sauts de ligne comme séparateurs d’instruction, attributs doivent apparaître sur la même ligne que la déclaration ou directement au-dessus de lui avec un caractère de continuation de ligne (le caractère de soulignement). Dans l’extrait de code ci-dessus, le caractère de continuation de ligne est utilisé pour placer l’attribut sur une seule ligne et la déclaration de méthode sur un autre.


Si, d’une certaine manière, un utilisateur autre que Tito tente d’exécuter le `RowDeleting` Gestionnaire d’événements ou un utilisateur non authentifié tente d’exécuter le `SelectedIndexChanged` Gestionnaire d’événements, le runtime .NET déclenchera un `SecurityException`.


[![Si le contexte de sécurité n’est pas autorisé à exécuter la méthode, une SecurityException est levée.](user-based-authorization-vb/_static/image41.png)](user-based-authorization-vb/_static/image40.png)

**Figure 14**: si le contexte de sécurité n’est pas autorisé à exécuter la méthode, un `SecurityException` est levée ([cliquez pour afficher l’image en taille réelle](user-based-authorization-vb/_static/image42.png))


> [!NOTE]
> Pour permettre à plusieurs contextes de sécurité accéder à une classe ou une méthode, décorez la classe ou une méthode avec un `PrincipalPermission` attribut pour chaque contexte de sécurité. Autrement dit, pour permettre à la fois Tito et Bruce exécuter le `RowDeleting` Gestionnaire d’événements, ajoutez *deux* `PrincipalPermission` attributs :


[!code-vb[Main](user-based-authorization-vb/samples/sample23.vb)]

En plus de pages ASP.NET, de nombreuses applications ont également une architecture qui inclut les différentes couches, telles que la logique métier et couches d’accès aux données. Ces couches sont généralement implémentés en tant que bibliothèques de classes et offrent des classes et méthodes permettant d’exécuter la logique et les données relatives des fonctionnalités métier. Le `PrincipalPermission` attribut est utile pour appliquer des règles d’autorisation à ces couches.

Pour plus d’informations sur l’utilisation de la `PrincipalPermission` attribut à définir des règles d’autorisation sur les classes et méthodes, reportez-vous à [Scott Guthrie](https://weblogs.asp.net/scottgu/)d’entrée de blog [Ajout de règles d’autorisation pour l’entreprise et les couches de données à l’aide de `PrincipalPermissionAttributes` ](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, nous avons vu comment appliquer des règles d’autorisation basées sur l’utilisateur. Nous avons commencé par examiner ASP. Infrastructure de NET URL d’autorisation. Sur chaque demande, le moteur ASP.NET `UrlAuthorizationModule` inspecte les règles de d’autorisation d’URL définis dans la configuration de l’application pour déterminer si l’identité est autorisée à accéder à la ressource demandée. En bref, l’autorisation d’URL rend plus facile de spécifier des règles d’autorisation pour une page spécifique ou pour toutes les pages dans un répertoire particulier.

Le framework de l’autorisation d’URL s’applique des règles d’autorisation sur une base de page par page. Avec l’autorisation d’URL, soit l’identité du demandeuse est autorisée à accéder à une ressource particulière ou non. Nombreux scénarios, cependant, appellent pour les règles d’autorisation plus précis. Au lieu de la définition qui est autorisé à accéder à une page, nous devons laisser tout le monde à accéder à une page, mais afficher des données différentes ou offrent des fonctionnalités différentes en fonction de l’utilisateur accédant à la page. Autorisation au niveau de la page implique généralement le masquage des éléments d’interface utilisateur spécifique afin d’empêcher les utilisateurs non autorisés d’accéder aux fonctionnalités interdites. En outre, il est possible d’utiliser des attributs pour restreindre l’accès aux classes et l’exécution de ses méthodes pour certains utilisateurs.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [Ajout de règles d’autorisation à l’entreprise et les couches de données à l’aide de `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [Autorisation ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [Modifications entre la sécurité IIS6 et IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Configuration des fichiers et sous-répertoires](https://msdn.microsoft.com/library/6hbkh9s7.aspx)
- [Limitation des fonctionnalités de Modification de données en fonction de l’utilisateur](../../data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb.md)
- [Démarrages rapides de contrôle LoginView](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/loginview.aspx)
- [Présentation de l’autorisation d’URL IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)
- [`UrlAuthorizationModule` Documentation technique](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)
- [Utilisation des données dans ASP.NET 2.0](../../data-access/index.md)

### <a name="about-the-author"></a>À propos de l’auteur

Scott Mitchell, auteur de plusieurs livres sur ASP/ASP.NET et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est *[SAM animer vous-même ASP.NET 2.0 des dernières 24 heures](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott peut être atteint à [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou via son blog à [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Précédent](validating-user-credentials-against-the-membership-user-store-vb.md)
> [Suivant](storing-additional-user-information-vb.md)
