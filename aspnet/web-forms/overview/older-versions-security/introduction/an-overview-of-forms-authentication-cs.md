---
uid: web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs
title: Une vue d’ensemble de l’authentification par formulaire (c#) | Microsoft Docs
author: rick-anderson
description: Création de Routes personnalisées
ms.author: riande
ms.date: 01/14/2008
ms.assetid: de2d65b9-aadc-42ba-abe1-4e87e66521a0
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: 64c8ccbc82a5c80a6f1e3199bfceb62add6cde51
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830920"
---
<a name="an-overview-of-forms-authentication-c"></a>Une vue d’ensemble de l’authentification par formulaire (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_02_CS.zip) ou [télécharger le PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial02_FormsAuth_cs.pdf)

> Dans ce didacticiel nous activera à partir de la discussion simple pour implémentation ; en particulier, nous allons examiner l’implémentation de l’authentification par formulaire. L’application web que nous commençons à construire dans ce didacticiel continueront à être créés dans les didacticiels suivants, nous passons à partir de l’authentification par formulaire simple à l’appartenance et les rôles.
> 
> Consultez cette vidéo pour plus d’informations sur ce sujet : [à l’aide de base l’authentification par formulaire dans ASP.NET](# "using-basic-forms-authentication-in-aspnet").


## <a name="introduction"></a>Introduction

Dans le [didacticiel précédent](security-basics-and-asp-net-support-cs.md) nous avons abordé les différentes d’authentification et d’autorisation utilisateur compte options fournies par ASP.NET. Dans ce didacticiel nous activera à partir de la discussion simple pour implémentation ; en particulier, nous allons examiner l’implémentation de l’authentification par formulaire. L’application web que nous commençons à construire dans ce didacticiel continueront à être créés dans les didacticiels suivants, nous passons à partir de l’authentification par formulaire simple à l’appartenance et les rôles.

Ce didacticiel commence par un examen approfondi pour le workflow de l’authentification de formulaires, une rubrique dont nous avons parlé lors dans le didacticiel précédent. Ensuite, nous allons créer un site Web ASP.NET par le biais duquel les concepts de l’authentification par formulaire de démonstration. Ensuite, nous allons configurer le site pour utiliser l’authentification par formulaire, créez une page de connexion simple et voir comment déterminer, dans le code, si un utilisateur est authentifié et, dans ce cas, le nom d’utilisateur consignés à l’aide.

Comprendre les formulaires de flux de travail de l’authentification, l’activation dans une application web et la création de pages de connexion et de fermeture de session sont vitales toutes les étapes dans la création d’une application ASP.NET qui prend en charge les comptes d’utilisateur et authentifie les utilisateurs via une page web. En raison de cette solution et étant donné que ces didacticiels s’appuient sur eux - je vous encourage à étudier ce didacticiel dans sa totalité avant de passer au prochain même si vous avez déjà eu l’expérience de configuration de l’authentification par formulaire dans les projets antérieurs.

## <a name="understanding-the-forms-authentication-workflow"></a>Comprendre le flux de travail de l’authentification de formulaires

Lorsque le runtime ASP.NET traite une demande pour une ressource ASP.NET, par exemple une page ASP.NET ou un service Web ASP.NET, la demande déclenche un nombre d’événements pendant son cycle de vie. Il existe des événements déclenchés à la fin de début et de très très de la demande, celles déclenché lorsque la demande est authentifiée et autorisé, un événement déclenché dans le cas d’une exception non gérée et ainsi de suite. Pour afficher une liste complète des événements, reportez-vous à la [événements de l’objet HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication_events.aspx).

*Les Modules HTTP* sont des classes managées dont le code est exécuté en réponse à un événement particulier dans le cycle de vie de demande. ASP.NET est livré avec un nombre de Modules HTTP qui effectuent des tâches essentielles en arrière-plan. Deux Modules HTTP intégrés qui sont particulièrement pertinentes pour notre discussion sont :

- **[`FormsAuthenticationModule`](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)** – authentifie l’utilisateur en inspectant le ticket d’authentification forms, qui est généralement inclus dans la collection de cookies de l’utilisateur. Si aucun ticket d’authentification par formulaires n’est présent, l’utilisateur est anonyme.
- **[`UrlAuthorizationModule`](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)** – Détermine si l’utilisateur actuel est autorisé à accéder à l’URL demandée. Ce module détermine l’autorité en consultant les règles d’autorisation spécifiées dans les fichiers de configuration de l’application. ASP.NET inclut également le [ `FileAuthorizationModule` ](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx) qui détermine l’autorité en consultant les ou les fichiers requis ACL.

Le `FormsAuthenticationModule` tente d’authentifier l’utilisateur avant le `UrlAuthorizationModule` (et `FileAuthorizationModule`) l’exécution. Si l’utilisateur qui effectue la requête n’est pas autorisé à accéder à la ressource demandée, le module d’autorisation met fin à la requête et retourne un [HTTP 401 non autorisé](http://www.checkupdown.com/status/E401.html) état. Dans les scénarios d’authentification Windows, l’état HTTP 401 est retournée au navigateur. Ce code d’état provoque le navigateur inviter l’utilisateur ses informations d’identification via une boîte de dialogue modale. Avec l’authentification par formulaire, toutefois, l’état HTTP 401 non autorisé est jamais envoyée au navigateur car FormsAuthenticationModule détecte cet état et le modifie pour rediriger l’utilisateur vers la page de connexion à la place (via un [HTTP 302 redirection](http://www.checkupdown.com/status/E302.html) état).

Responsabilité de la page connexion consiste à déterminer si les informations d’identification sont valides et, dans ce cas, pour créer un ticket d’authentification par formulaires et de rediriger l’utilisateur vers la page qu’ils ont été d’essayer de visiter. Le ticket d’authentification est inclus dans les demandes suivantes pour les pages sur le site Web, ce qui le `FormsAuthenticationModule` utilise pour identifier l’utilisateur.


![Le flux de travail de l’authentification de formulaires](an-overview-of-forms-authentication-cs/_static/image1.png)

**Figure 1**: le flux de travail de l’authentification de formulaires


### <a name="remembering-the-authentication-ticket-across-page-visits"></a>Mémoriser le Ticket d’authentification une visite de Page

Une fois connecté, le ticket d’authentification par formulaires doit être envoyé sur le serveur web à chaque demande afin que l’utilisateur reste connecté lorsqu’ils naviguent sur le site. Cela est généralement effectuée en plaçant le ticket d’authentification dans la collection de cookies de l’utilisateur. [Les cookies](http://en.wikipedia.org/wiki/HTTP_cookie) sont de petits fichiers texte qui résident sur l’ordinateur de l’utilisateur et sont transmis dans les en-têtes HTTP à chaque demande pour le site Web qui l’a créé. Par conséquent, une fois que le ticket d’authentification par formulaires a été créé et stocké dans les cookies de navigateur, chaque visite suivante sur ce site envoie le ticket d’authentification avec la demande, et ainsi identifier l’utilisateur.

Un aspect de cookies est leur délai d’expiration, ce qui est la date et l’heure à laquelle le navigateur ignore le cookie. Lorsque le cookie d’authentification forms expire, l’utilisateur peut n’est plus authentifié et par conséquent devenir anonyme. Lorsqu’un utilisateur visite à partir d’un terminal public, sans doute qu’ils souhaitent leur ticket d’authentification pour qu’ils expirent lorsqu’ils ferment leur navigateur. Lors de la visite à domicile, ce même utilisateur peut cependant pas oublier lors des redémarrages de navigateur afin qu’ils n’ont pas le ticket d’authentification pour re-connecter chaque fois qu’ils visitent le site. Cette décision est souvent prise par l’utilisateur sous la forme d’un « mémoriser » la case à cocher sur la page de connexion. À l’étape 3, nous allons examiner comment implémenter une case à cocher « Mémoriser » dans la page de connexion. Le didacticiel suivant traite les paramètres de délai d’expiration du ticket d’authentification en détail.

> [!NOTE]
> Il est possible que l’agent utilisateur utilisé pour ouvrir une session le site Web, n’acceptent pas les cookies. Dans ce cas, ASP.NET peut utiliser des tickets de l’authentification par formulaire sans cookie. Dans ce mode, le ticket d’authentification est encodé dans l’URL. Nous allons examiner lorsque les tickets d’authentification sans cookies sont utilisés et comment elles sont créées et gérées dans le didacticiel suivant.


### <a name="the-scope-of-forms-authentication"></a>L’étendue de l’authentification par formulaire

Le `FormsAuthenticationModule` est code managé qui fait partie du runtime ASP.NET. Avant la version 7 de Microsoft [Internet Information Services (IIS)](https://www.iis.net/) serveur web, il a été une barrière distincte entre le pipeline HTTP de d’IIS et le pipeline du runtime ASP.NET. En bref, dans IIS 6 et versions antérieures, le `FormsAuthenticationModule` s’exécute uniquement quand une demande est déléguée à partir d’IIS pour le runtime ASP.NET. Par défaut, IIS traite le contenu statique lui-même – tels que des pages HTML et CSS et les fichiers image – et transmet uniquement des demandes pour le runtime ASP.NET lorsqu’une page avec l’extension .aspx, .asmx ou .ashx est demandée.

IIS 7, toutefois, permet intégré IIS et ASP.NET pipelines. Avec quelques paramètres de configuration, vous pouvez configurer IIS 7 pour appeler FormsAuthenticationModule pour *tous les* demandes. En outre, avec IIS 7 vous pouvez définir des règles d’autorisation d’URL pour les fichiers de n’importe quel type. Pour plus d’informations, consultez [sécurité modifications entre IIS6 et IIS7](https://www.iis.net/learn/get-started/whats-new-in-iis-7/changes-in-security-between-iis-60-and-iis-7-and-above), [votre sécurité de la plateforme Web](https://www.iis.net/learn/get-started/whats-new-in-iis-7/iis7-and-above-security-improvements), et [comprendre l’autorisation d’URL IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization).

Résumer, dans les versions antérieures d’IIS 7, vous pouvez uniquement utiliser l’authentification par formulaire pour protéger les ressources gérées par le runtime ASP.NET. De même, les règles d’autorisation d’URL sont appliquées uniquement aux ressources gérées par le runtime ASP.NET. Mais avec IIS 7, il est possible d’intégrer le FormsAuthenticationModule et UrlAuthorizationModule dans le pipeline HTTP de d’IIS, étendant ainsi cette fonctionnalité pour toutes les demandes.

## <a name="step-1-creating-an-aspnet-website-for-this-tutorial-series"></a>Étape 1 : Création d’un site Web ASP.NET pour cette série de didacticiels

Afin d’atteindre l’audience la plus large possible, le site Web ASP.NET que nous allez générer tout au long de cette série sera créé avec la version gratuite de Microsoft Visual Studio 2008, [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/). Nous allons implémenter le `SqlMembershipProvider` magasin de l’utilisateur dans un [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) base de données. Si vous utilisez Visual Studio 2005 ou une autre édition de Visual Studio 2008 ou SQL Server, ne vous inquiétez pas : les étapes sont presque identiques et seront de souligner les différences éventuelles non triviale.

> [!NOTE]
> L’application web de démonstration utilisée dans chaque didacticiel est disponible en téléchargement. Cette application téléchargeable a été créée avec Visual Web Developer 2008 ciblé pour le .NET Framework version 3.5. Dans la mesure où l’application est ciblée pour .NET 3.5, son fichier Web.config inclut des éléments de configuration supplémentaires, spécifiques à 3.5. En résumé, si vous devez encore installer .NET 3.5 sur votre ordinateur, l’application web téléchargeable ne fonctionnera pas sans supprimer au préalable le balisage spécifique 3.5 à partir de Web.config.


Avant que nous pouvons configurer l’authentification par formulaire, nous devons d’abord un site Web ASP.NET. Commencez par créer un nouveau fichier basé sur le système site Web ASP.NET. Pour ce faire, lancez Visual Web Developer et accédez au menu fichier et choisissez Nouveau Site Web, en affichant la boîte de dialogue Nouveau Site Web. Choisissez le modèle de Site Web ASP.NET, la valeur de la liste déroulante emplacement du système de fichiers, choisissez un dossier pour placer le site web, la valeur est le langage c#. Cela créera un nouveau site web avec une page Default.aspx ASP.NET, une application\_dossier de données et un fichier Web.config.

> [!NOTE]
> Visual Studio prend en charge deux modes de gestion de projet : projets de Site Web et projets d’Application Web. Projets de Site Web n’ont pas un fichier projet, alors que les projets d’Application Web imiter l’architecture de projet dans Visual Studio .NET 2002/2003 – ils incluent un fichier projet et compiler le code du projet source dans un assembly unique, qui est placé dans le dossier /bin. Visual Studio 2005 initialement le seul Site Web pris en charge les projets, bien que le modèle de projet d’Application Web a été réintroduit avec Service Pack 1 ; Visual Studio 2008 offre les deux modèles de projet. Visual Web Developer 2005 et 2008 éditions, toutefois, uniquement prennent en charge les projets de Site Web. J’utilise le modèle de projet de Site Web. Si vous utilisez une édition non-Express et que vous souhaitez utiliser le [modèle de projet d’Application Web](https://msdn.microsoft.com/library/aa730880%28vs.80%29.aspx) au lieu de cela, n’hésitez pas à le faire, mais n’oubliez pas qu’il existe peut-être des différences entre ce que vous voyez sur votre écran et les étapes que vous devez prendre par rapport à la captures d’écran indiqués et les instructions fournies dans ces didacticiels.


[![Créer un Site Web de système de nouveau fichier](an-overview-of-forms-authentication-cs/_static/image3.png)](an-overview-of-forms-authentication-cs/_static/image2.png)

**Figure 2**: créer un Site Web de New File System-Based ([cliquez pour afficher l’image en taille réelle](an-overview-of-forms-authentication-cs/_static/image4.png))


### <a name="adding-a-master-page"></a>Ajout d’une Page maître

Ensuite, ajoutez une nouvelle Page maître au site dans le répertoire racine nommé Site.master. [Pages maîtres](https://msdn.microsoft.com/library/wtxbf3hh.aspx) permettent à un développeur définir un modèle de l’échelle du site qui peut être appliqué aux pages ASP.NET. Le principal avantage des pages maîtres est que son apparence globale du site peut être définie dans un emplacement unique, ce qui le rend facile à mettre à jour ou modifier la mise en page du site.


[![Ajouter une Page maître nommée Site.master au site Web](an-overview-of-forms-authentication-cs/_static/image6.png)](an-overview-of-forms-authentication-cs/_static/image5.png)

**Figure 3**: ajouter un Site.master nommé de Page maître au site Web ([cliquez pour afficher l’image en taille réelle](an-overview-of-forms-authentication-cs/_static/image7.png))


Définir la disposition de la page de l’échelle du site ici dans la page maître. Vous pouvez utiliser le mode Design et ajouter quelque vous avez besoin des contrôles de disposition ou Web, ou vous pouvez ajouter manuellement le balisage manuellement dans la vue de Source. J’ai structuré mon maître mise en page pour reproduire la mise en page utilisée dans mon *[utilisation des données dans ASP.NET 2.0](../../data-access/index.md)* série de didacticiels (voir Figure 4). La page maître utilise [des feuilles de style en cascade](http://www.w3schools.com/css/default.asp) de positionnement et de styles avec les paramètres de CSS définies dans le fichier Style.css (qui est inclus dans le téléchargement associé de ce didacticiel). Bien que vous ne pouvez pas indiquer à partir du balisage indiqué ci-dessous, les règles CSS sont définies telles que le volet de navigation &lt;div&gt;du contenu est positionné de façon absolue afin qu’il apparaît à gauche et a une largeur fixe de 200 pixels.

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample1.aspx)]

Une page maître définit la disposition de page statiques et les régions qui peuvent être modifiées par les pages ASP.NET qui utilisent la page maître. Ces zones de contenu modifiables sont indiquées par le `ContentPlaceHolder` contrôle, qui peut être consulté dans le contenu &lt;div&gt;. Notre page maître dispose d’un seul `ContentPlaceHolder` (MainContent), mais la page maître peut avoir plusieurs ContentPlaceHolders.

Avec le balisage ci-dessus, basculer vers la vue de conception montre mise en page de la page maître. Toutes les pages ASP.NET qui utilisent cette page maître aura cette mise en page uniforme, avec la possibilité de spécifier le balisage pour le `MainContent` région.


[![La Page maître, lorsqu’ils sont affichés via la vue de conception](an-overview-of-forms-authentication-cs/_static/image9.png)](an-overview-of-forms-authentication-cs/_static/image8.png)

**Figure 4**: la Page maître, lorsque affichés via le mode création ([cliquez pour afficher l’image en taille réelle](an-overview-of-forms-authentication-cs/_static/image10.png))


### <a name="creating-content-pages"></a>Création de Pages de contenu

À ce stade, nous avons une page Default.aspx dans notre site Web, mais il n’utilise pas la page maître que nous venons de créer. Bien qu’il soit possible de manipuler le balisage déclaratif d’une page web à utiliser une page maître, si la page ne contient aucun contenu il est encore plus facile de simplement supprimer la page et l’ajouter de nouveau au projet, en spécifiant la page maître à utiliser. Ainsi, vous pouvez commencer en supprimant Default.aspx à partir du projet.

Ensuite, avec le bouton droit sur le nom du projet dans l’Explorateur de solutions et choisissez d’ajouter un nouveau formulaire Web nommé Default.aspx. Cette fois-ci, cochez la case « Sélectionner la page maître » et choisissez la page maître Site.master dans la liste.


[![Ajoutez une nouvelle Page Default.aspx choix sélectionner une Page maître](an-overview-of-forms-authentication-cs/_static/image12.png)](an-overview-of-forms-authentication-cs/_static/image11.png)

**Figure 5**: ajouter un nouveau Default.aspx Page choix pour sélectionner une Page maître ([cliquez pour afficher l’image en taille réelle](an-overview-of-forms-authentication-cs/_static/image13.png))


![Utilisez la Page Site.master maître](an-overview-of-forms-authentication-cs/_static/image14.png)

**Figure 6**: utilisez la Page Site.master maître


> [!NOTE]
> Si vous utilisez le modèle de projet d’Application Web la boîte de dialogue Ajouter un nouvel élément n’inclut pas une case à cocher « Sélectionner la page maître ». Au lieu de cela, vous devez ajouter un élément de type « Formulaire de contenu Web. » Après avoir en choisissant l’option « Formulaire de contenu Web » et en cliquant sur Ajouter, Visual Studio affiche le même, sélectionnez un serveur maître boîte de dialogue illustrée à la Figure 6.


Balisage déclaratif de la nouvelle page Default.aspx inclut simplement un @Page en spécifiant le chemin d’accès au maître de la directive page fichier et un contrôle de contenu MainContent ContentPlaceHolder de la page maître.

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample2.aspx)]

Pour l’instant, laissez vide Default.aspx. Nous y reviendrons plus loin dans ce didacticiel pour ajouter du contenu.

> [!NOTE]
> Notre page maître inclut une section d’un menu ou une autre interface de navigation. Nous allons créer une telle interface dans un futur didacticiel.

## <a name="step-2-enabling-forms-authentication"></a>Étape 2 : Activation de l’authentification par formulaire

Avec le site Web ASP.NET créé, notre tâche suivante consiste à activer l’authentification par formulaire. Configuration de l’authentification de l’application est spécifiée via le [ `<authentication>` élément](https://msdn.microsoft.com/library/532aee0e.aspx) dans le fichier Web.config. Le `<authentication>` élément contient un seul attribut nommé mode qui spécifie le modèle d’authentification utilisé par l’application. Cet attribut peut avoir une des quatre valeurs suivantes :

- **Windows** : comme indiqué dans le didacticiel précédent, lorsqu’une application utilise l’authentification Windows il incombe au serveur web pour authentifier le visiteur, et cela s’effectue habituellement par le biais de base, Digest ou intégrée Windows authentification.
- **Formulaires**– les utilisateurs sont authentifiés via un formulaire sur une page web.
- **Passport**– les utilisateurs sont authentifiés à l’aide Passport Network de Microsoft.
- **Aucun**: aucun modèle d’authentification n’est utilisé ; tous les visiteurs sont anonymes.

Par défaut, les applications ASP.NET utilisent l’authentification Windows. Pour modifier le type d’authentification pour l’authentification par formulaire, puis, nous devons modifier la `<authentication>` attribut de mode de l’élément pour les formulaires.

Si votre projet ne contient pas encore un fichier Web.config, ajoutez un maintenant en cliquant sur le nom du projet dans l’Explorateur de solutions, en choisissant Ajouter un nouvel élément et l’ajout d’un fichier de Configuration Web.


[![Si votre projet n’inclut pas encore de Web.config, ajoutez-le maintenant](an-overview-of-forms-authentication-cs/_static/image16.png)](an-overview-of-forms-authentication-cs/_static/image15.png)

**Figure 7**: Si votre projet est pas encore inclure Web.config, ajoutez maintenant ([cliquez pour afficher l’image en taille réelle](an-overview-of-forms-authentication-cs/_static/image17.png))


Ensuite, localisez le `<authentication>` élément et mise à jour pour utiliser l’authentification par formulaire. Après cette modification, les balises de votre fichier Web.config doivent ressembler à ce qui suit :

[!code-xml[Main](an-overview-of-forms-authentication-cs/samples/sample3.xml)]

> [!NOTE]
> Étant donné que le fichier Web.config est un fichier XML, la casse est importante. Assurez-vous que vous définissez l’attribut mode aux formulaires, avec une majuscule « F ». Si vous utilisez une casse différente, telles que « forms », vous recevrez une erreur de configuration lors de la visite le site via un navigateur.


Le `<authentication>` élément peut éventuellement inclure un `<forms>` élément enfant qui contient des paramètres spécifique à l’authentification de formulaires. Pour l’instant, nous allons utiliser de simplement les paramètres d’authentification de formulaires par défaut. Nous explorerons le `<forms>` élément enfant plus en détail dans le didacticiel suivant.

## <a name="step-3-building-the-login-page"></a>Étape 3 : Génération de la Page de connexion

Pour prendre en charge l’authentification par formulaire notre site Web a besoin d’une page de connexion. Comme indiqué dans la section « Understanding the Forms Workflow d’authentification », le `FormsAuthenticationModule` sera automatiquement rediriger l’utilisateur vers la page de connexion s’ils tentent d’accéder à une page qui ne sont pas autorisés à afficher. Il existe également des contrôles Web ASP.NET qui affichent un lien vers la page de connexion aux utilisateurs anonymes. Cela pose la question « Qu’est l’URL de la page de connexion ? »

Par défaut, le système d’authentification forms attend la page de connexion soit nommée Login.aspx et placé dans le répertoire racine de l’application web. Si vous souhaitez utiliser une URL de page de connexion différente, vous pouvez le faire en le spécifiant dans le fichier Web.config. Nous verrons comment effectuer cette opération dans le didacticiel suivant.

La page de connexion a trois rôles :

1. Fournir une interface qui autorise le visiteur à entrer leurs informations d’identification.
2. Détermine si les informations d’identification soumises sont valides.
3. « Se connecter » l’utilisateur en créant les formulaires ticket d’authentification.

### <a name="creating-the-login-pages-user-interface"></a>Création d’Interface utilisateur de la Page de connexion

Commençons par la première tâche. Ajouter une nouvelle page ASP.NET pour le répertoire du site racine nommé Login.aspx et l’associer à la page maître Site.master.


[![Ajoutez une nouvelle Page ASP.NET nommé Login.aspx](an-overview-of-forms-authentication-cs/_static/image19.png)](an-overview-of-forms-authentication-cs/_static/image18.png)

**Figure 8**: ajouter un nouveau Login.aspx de nommé ASP.NET Page ([cliquez pour afficher l’image en taille réelle](an-overview-of-forms-authentication-cs/_static/image20.png))


L’interface de page de connexion classique se compose de deux zones de texte : une pour le nom d’utilisateur, un pour son mot de passe et un bouton pour envoyer le formulaire. Sites Web incluent souvent, une case à cocher « Mémoriser » qui, si elle est activée, conserve le ticket d’authentification qui en résulte un redémarrage du navigateur.

Ajouter deux zones de texte à Login.aspx et définissez leurs `ID` propriétés nom d’utilisateur et mot de passe, respectivement. Également définir de mot de passe `TextMode` propriété au mot de passe. Ensuite, ajoutez un contrôle de case à cocher, en définissant son `ID` propriété RememberMe et son `Text` propriété à « Mémoriser mes informations ». Ensuite, ajoutez un bouton nommé LoginButton dont `Text` propriété est définie sur « Connexion ». Et enfin, ajoutez un contrôle Web de l’étiquette et définissez son `ID` propriété InvalidCredentialsMessage, son `Text` propriété à « votre nom d’utilisateur ou le mot de passe n’est pas valide. Veuillez réessayer. », son `ForeColor` propriété en rouge et sa `Visible` False à la propriété.

À ce stade, votre écran doit ressembler à la capture d’écran de la Figure 9, et la syntaxe déclarative de votre page doit se présenter comme suit :

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample4.aspx)]


[![La Page de connexion contient deux zones de texte, une case à cocher, un bouton et une étiquette](an-overview-of-forms-authentication-cs/_static/image22.png)](an-overview-of-forms-authentication-cs/_static/image21.png)

**Figure 9**: le Login Page contient deux zones de texte, une case à cocher, un bouton et une étiquette ([cliquez pour afficher l’image en taille réelle](an-overview-of-forms-authentication-cs/_static/image23.png))


Enfin, créez un gestionnaire d’événements pour, cliquez sur du LoginButton événement. À partir du concepteur, double-cliquez simplement sur le contrôle bouton pour créer ce gestionnaire d’événements.

### <a name="determining-if-the-supplied-credentials-are-valid"></a>Comment déterminer si les informations d’identification fournies sont valides

Nous devons maintenant mettre en œuvre de la tâche 2 dans Click du bouton Gestionnaire d’événements : déterminer si les informations d’identification fournies sont valides. Pour ce faire, il doit être un magasin d’utilisateurs qui contient l’ensemble des informations d’identification des utilisateurs afin que nous pouvons déterminer si les informations d’identification fournies concordent avec les informations d’identification connues.

Avant ASP.NET 2.0, les développeurs étaient responsables pour implémenter les deux leurs propres magasins d’utilisateurs et d’écriture de code pour valider les informations d’identification fournies par rapport au magasin. La plupart des développeurs implémente le magasin de l’utilisateur dans une base de données, création d’une table nommée d’utilisateurs avec des colonnes comme nom d’utilisateur, mot de passe, E-mail, LastLoginDate et ainsi de suite. Cette table, puis, aurait un enregistrement par le compte d’utilisateur. Vérification des informations d’identification d’un utilisateur impliquerait l’interrogation de la base de données pour un nom d’utilisateur correspondant et vérifier que le mot de passe dans la base de données correspondait au mot de passe.

Avec ASP.NET 2.0, les développeurs doivent utiliser un des fournisseurs d’appartenances pour gérer le magasin d’utilisateurs. Dans cette série de didacticiels, nous allons utiliser SqlMembershipProvider, qui utilise une base de données SQL Server pour le magasin d’utilisateurs. Lorsque vous utilisez SqlMembershipProvider que nous devons implémenter un schéma de base de données spécifique qui inclut les tables, vues et les procédures stockées attendus par le fournisseur. Nous allons examiner comment implémenter ce schéma dans le ***création du schéma d’appartenance dans SQL Server*** didacticiel. Avec le fournisseur d’appartenances en place, la validation des informations d’identification de l’utilisateur est aussi simple que si vous appelez le [classe d’appartenance](https://msdn.microsoft.com/library/system.web.security.membership.aspx)de [ValidateUser (*nom d’utilisateur*, *mot de passe*) méthode](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx), qui retourne une valeur booléenne indiquant si la validité de la *nom d’utilisateur* et *mot de passe* combinaison. Voir que nous n’avons pas encore implémenté de magasin de l’utilisateur de SqlMembershipProvider, nous ne pouvons pas utiliser la méthode de ValidateUser de la classe d’appartenance pour l’instant.

Plutôt que de prendre le temps de créer notre propre table personnalisée de la base de données d’utilisateurs (ce qui serait obsolète une fois que nous avons implémenté SqlMembershipProvider), nous allons plutôt coder en dur les informations d’identification valides au sein de la connexion page lui-même. Dans le LoginButton Gestionnaire d’événements Click, ajoutez le code suivant :

[!code-csharp[Main](an-overview-of-forms-authentication-cs/samples/sample5.cs)]

Comme vous pouvez le voir, il existe trois comptes d’utilisateurs valides – Scott Jisun et Sam – et toutes trois ont le même mot de passe (« password »). Le code effectue une itération sur les groupes d’utilisateurs et mots de passe recherche d’une correspondance de nom d’utilisateur et mot de passe valide. Si le nom d’utilisateur et le mot de passe sont valides, nous avons besoin connecter l’utilisateur et ensuite de les rediriger vers la page appropriée. Si les informations d’identification ne sont pas valides, puis nous affichons l’étiquette InvalidCredentialsMessage.

Lorsqu’un utilisateur entre des informations d’identification valides, je l’ai mentionné qu’ils sont ensuite redirigés vers la « page appropriée ». Qu’est la page appropriée, cependant ? Rappelez-vous que lorsqu’un utilisateur visite une page qu’ils ne sont pas autorisés à afficher, FormsAuthenticationModule redirige automatiquement vers la page de connexion. Ce faisant, il inclut l’URL demandée dans la chaîne de requête via le paramètre ReturnUrl. Autrement dit, si un utilisateur a tenté de visiter ProtectedPage.aspx, et ils n’ont pas été autorisés à le faire, FormsAuthenticationModule serait les redirige vers :

Login.aspx?ReturnUrl=ProtectedPage.aspx

Lors de la connexion avec succès, l’utilisateur doit être redirigé vers ProtectedPage.aspx. Vous pouvez également, les utilisateurs peuvent se connecter la page de connexion sur leur propre chef. Dans ce cas, une fois l’utilisateur connecté ils doivent être envoyés à la page Default.aspx du dossier racine.

### <a name="logging-in-the-user"></a>Journalisation de l’utilisateur

En supposant que les informations d’identification fournies sont valides, nous devons créer un ticket d’authentification par formulaires, journalisation et de l’utilisateur vers le site. Le [classe FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthentication.aspx) dans le [espace de noms System.Web.Security](https://msdn.microsoft.com/library/system.web.security.aspx) fournit des méthodes assorties pour la journalisation de journalisation des utilisateurs via les formulaires d’entrée et de système d’authentification. Bien qu’il existe plusieurs méthodes dans la classe FormsAuthentication, les trois que nous sommes intéressés à partir de là sont :

- [GetAuthCookie (*nom d’utilisateur*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.getauthcookie.aspx) – crée un ticket d’authentification de formulaires pour le nom fourni *nom d’utilisateur*. Ensuite, cette méthode crée et retourne un objet HttpCookie qui contient le contenu du ticket d’authentification. Si *persistCookie* a la valeur true, un cookie persistant est créé.
- [SetAuthCookie (*nom d’utilisateur*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) – appelle le GetAuthCookie (*nom d’utilisateur*, *persistCookie*) méthode permettant de générer le cookie d’authentification de formulaires. Cette méthode ajoute ensuite le cookie retourné par GetAuthCookie à la collection de Cookies (en supposant que l’authentification de formulaires basés sur des cookies est utilisé ; sinon, cette méthode appelle une classe interne qui gère la logique de ticket sans cookies).
- [RedirectFromLoginPage (*nom d’utilisateur*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.redirectfromloginpage.aspx) – cette méthode appelle SetAuthCookie (*nom d’utilisateur*, *persistCookie*) et puis redirige l’utilisateur vers la page appropriée.

GetAuthCookie est pratique lorsque vous devez modifier le ticket d’authentification avant l’écriture du cookie à la collection de Cookies. SetAuthCookie est utile si vous souhaitez créer les formulaires ticket d’authentification et l’ajouter à la collection de Cookies, mais vous ne souhaitez pas rediriger l’utilisateur vers la page appropriée. Vous voulez peut-être les conserver sur la page de connexion ou les envoyer vers une autre page.

Étant donné que nous souhaitons connecter l’utilisateur et les redirige vers la page appropriée, nous allons utiliser RedirectFromLoginPage. Jour, cliquez sur de la LoginButton Gestionnaire d’événements, en remplaçant les deux lignes TODO commentés avec la ligne de code suivante :

FormsAuthentication.RedirectFromLoginPage (UserName.Text, RememberMe.Checked) ;

Lors de la création de formulaires ticket d’authentification, nous utilisons les propriété de texte de la UserName TextBox pour le ticket d’authentification forms *nom d’utilisateur* paramètre et l’état activé du RememberMe CheckBox pour le  *persistCookie* paramètre.

Pour tester la page de connexion, reportez-vous au dans un navigateur. Commencez par entrer des informations d’identification non valides, comme un nom d’utilisateur de « Nope » et un mot de passe de « incorrect ». Lorsque vous cliquez sur le bouton de connexion se produira une publication (postback) et l’étiquette InvalidCredentialsMessage s’affichera.


[![L’étiquette InvalidCredentialsMessage est affichée lors de la saisie informations d’identification erronées](an-overview-of-forms-authentication-cs/_static/image25.png)](an-overview-of-forms-authentication-cs/_static/image24.png)

**Figure 10**: l’étiquette InvalidCredentialsMessage est affiché lors de la saisie informations d’identification erronées ([cliquez pour afficher l’image en taille réelle](an-overview-of-forms-authentication-cs/_static/image26.png))


Ensuite, entrez les informations d’identification valides et cliquez sur le bouton de connexion. Cette fois lorsque la publication (postback) produit un ticket d’authentification par formulaires est créée et vous êtes automatiquement redirigé vers Default.aspx. À ce stade vous êtes connecté au site Web, bien qu’il n’existe aucune signaux visuels pour indiquer que vous êtes actuellement connecté. À l’étape 4, que nous verrons comment déterminer par programmation si un utilisateur est connecté dans ou pas, ainsi que comment identifier l’utilisateur accédant à la page.

Étape 5 examine les techniques pour la journalisation d’un utilisateur sur le site Web.

### <a name="securing-the-login-page"></a>Sécurisation de la Page de connexion

Lorsque l’utilisateur entre ses informations d’identification et soumet le formulaire de la page de connexion, les informations d’identification, y compris son mot de passe, sont transmises via Internet au serveur web dans *texte brut*. Cela signifie que tout pirate d’analyser le trafic réseau peut voir le nom d’utilisateur et le mot de passe. Pour éviter ce problème, il est essentiel pour chiffrer le trafic réseau à l’aide de [couches SSL (Secure Socket)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Cela garantit que les informations d’identification (comme balisage HTML de la page entière) est chiffré dès le moment où qu'ils quittent le navigateur jusqu'à ce qu’ils sont reçus par le serveur web.

À moins que votre site Web contient des informations sensibles, il vous suffit d’utiliser SSL sur la page de connexion et sur d’autres pages où le mot de passe utilisateur serait sinon envoyée sur le réseau en texte brut. Vous n’avez pas besoin à vous soucier de la sécurisation de ticket d’authentification par les formulaires dans la mesure où, par défaut, il est chiffré et signé numériquement (afin d’éviter la falsification). Une discussion plus détaillée sur la sécurité de ticket d’authentification de formulaires est présentée dans ce didacticiel.

> [!NOTE]
> De nombreux sites Web de financières et médicales sont configurés pour utiliser le protocole SSL sur *tous les* pages accessibles à des utilisateurs authentifiés. Si vous créez un site Web vous pouvez configurer le système d’authentification forms afin que le ticket d’authentification par formulaires est transmis uniquement via une connexion sécurisée. Nous allons examiner les différentes options de configuration de l’authentification de formulaires dans le didacticiel suivant,  *[Configuration de l’authentification de formulaires et des sujets avancés](forms-authentication-configuration-and-advanced-topics-cs.md)*.


## <a name="step-4-detecting-authenticated-visitors-and-determining-their-identity"></a>Étape 4 : Détection des visiteurs authentifiés et déterminer leur identité

À ce stade nous avons activé l’authentification par formulaire et créé une page de connexion rudimentaires, mais nous devons encore examiner la façon dont nous pouvons déterminer si un utilisateur est authentifié ou anonyme. Dans certains scénarios nous voulons pouvoir afficher différentes données ou des informations selon si un utilisateur authentifié ou anonyme visite la page. En outre, nous devons souvent connaître l’identité de l’utilisateur authentifié.

Nous allons augmenter la page Default.aspx existante pour illustrer ces techniques. Dans Default.aspx ajoutez deux contrôles de panneau de configuration, un AuthenticatedMessagePanel nommé et un autre AnonymousMessagePanel nommée. Ajoutez un contrôle Label nommé WelcomeBackMessage dans le premier panneau. Dans le deuxième panneau ajouter un contrôle de lien hypertexte, définissez sa propriété Text sur « Log In » et à sa propriété NavigateUrl « ~ / Login.aspx ». À ce stade le balisage déclaratif pour Default.aspx doit ressembler à ce qui suit :

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample6.aspx)]

Comme vous l’avez probablement deviné à ce stade, l’idée consiste à afficher simplement le AuthenticatedMessagePanel visiteurs authentifiés et simplement le AnonymousMessagePanel pour les visiteurs anonymes. Pour ce faire, nous devons définir des ces panneaux visibles en fonction de si l’utilisateur est connecté ou non.

Le [Request.IsAuthenticated propriété](https://msdn.microsoft.com/library/system.web.httprequest.isauthenticated.aspx) retourne une valeur booléenne indiquant si la demande a été authentifiée. Entrez le code suivant dans la Page\_charger du code de gestionnaire d’événements :

[!code-csharp[Main](an-overview-of-forms-authentication-cs/samples/sample7.cs)]

Avec ce code en place, visitez Default.aspx via un navigateur. En supposant que vous avez encore pour vous connecter, vous verrez un lien vers la page de connexion (voir Figure 11). Cliquez sur ce lien et connectez-vous au site. Comme nous l’avons vu à l’étape 3, après avoir entré vos informations d’identification s’affichera à Default.aspx, mais cette fois la page indique la « Welcome retour ! » message (voir Figure 12).


![Lorsque visite de façon anonyme, un lien de journal s’affiche](an-overview-of-forms-authentication-cs/_static/image27.png)

**Figure 11**: lors de la visite de façon anonyme, un lien de journal s’affiche.


![Les utilisateurs authentifiés sont affichés le](an-overview-of-forms-authentication-cs/_static/image28.png)

**Figure 12**: utilisateurs authentifiés sont affichés le « Bienvenue ! » Message


Nous pouvons déterminer identité de l’utilisateur actuellement connecté via le [objet HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)de [propriété utilisateur](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx). L’objet HttpContext représente des informations sur la requête actuelle et est l’emplacement pour ces objets ASP.NET communs en tant que réponse, de demande et de Session, entre autres. La propriété de l’utilisateur représente le contexte de sécurité de la requête HTTP et l’implémente actuel le [interface IPrincipal](https://msdn.microsoft.com/library/system.security.principal.iprincipal.aspx).

La propriété de l’utilisateur est définie par FormsAuthenticationModule. Plus précisément, lorsque FormsAuthenticationModule détecte un ticket d’authentification par formulaires à la demande entrante, il crée un objet GenericPrincipal et affecte à la propriété de l’utilisateur.

Objets principal (comme GenericPrincipal) fournissent des informations sur l’identité de l’utilisateur et les rôles auxquels ils appartiennent. L’interface IPrincipal définit deux membres :

- [IsInRole (*roleName*)](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole.aspx) – une méthode qui retourne une valeur booléenne indiquant si l’objet principal appartient au rôle spécifié.
- [Identité](https://msdn.microsoft.com/library/system.security.principal.iprincipal.identity.aspx) – une propriété qui retourne un objet qui implémente le [interface IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx). L’interface IIdentity définit trois propriétés : [AuthenticationType](https://msdn.microsoft.com/library/system.security.principal.iidentity.authenticationtype.aspx), [IsAuthenticated](https://msdn.microsoft.com/library/system.security.principal.iidentity.isauthenticated.aspx), et [nom](https://msdn.microsoft.com/library/system.security.principal.iidentity.name.aspx).

Nous pouvons déterminer le nom du visiteur actuels en utilisant le code suivant :

chaîne currentUsersName = User.Identity.Name ;

À l’aide des formulaires d’authentification, un [objet FormsIdentity](https://msdn.microsoft.com/library/system.web.security.formsidentity.aspx) est créé pour la propriété d’identité de l’objet GenericPrincipal. La classe FormsIdentity retourne toujours la chaîne « Forms » pour ses la propriété AuthenticationType et la valeur true pour sa propriété IsAuthenticated. La propriété Name retourne le nom d’utilisateur spécifié lors de la création de formulaires ticket d’authentification. En plus de ces trois propriétés FormsIdentity inclut l’accès pour le ticket d’authentification sous-jacent via son [Ticket propriété](https://msdn.microsoft.com/library/system.web.security.formsidentity.ticket.aspx). La propriété Ticket retourne un objet de type [FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx), qui a des propriétés telles que l’Expiration, IsPersistent, IssueDate, nom et ainsi de suite.

Le point important à retenir ici est que le *nom d’utilisateur* paramètre spécifié dans le FormsAuthentication.GetAuthCookie (*nom d’utilisateur*, *persistCookie*), FormsAuthentication.SetAuthCookie (*nom d’utilisateur*, *persistCookie*) et FormsAuthentication.RedirectFromLoginPage (*nom d’utilisateur*, *persistCookie*) méthodes est la valeur retournée par User.Identity.Name. En outre, le ticket d’authentification créé par ces méthodes est disponible en castant User.Identity à un objet FormsIdentity et accéder à la propriété Ticket :

[!code-csharp[Main](an-overview-of-forms-authentication-cs/samples/sample8.cs)]

Nous allons saisir un message personnalisé dans Default.aspx. Mettre à jour de la Page\_charger le Gestionnaire d’événements afin que la chaîne est assignée à la propriété de texte du contrôle WelcomeBackMessage Label « Bienvenue, *nom d’utilisateur*! »

WelcomeBackMessage.Text = « Bienvenue » + User.Identity.Name + « ! » ;

Figure 13 montre l’effet de cette modification (quand vous vous connectez en tant qu’utilisateur Scott).


![Le Message d’accueil comprend actuellement connecté dans le nom de l’utilisateur](an-overview-of-forms-authentication-cs/_static/image29.png)

**Figure 13**: le Message d’accueil comprend actuellement connecté dans le nom de l’utilisateur


### <a name="using-the-loginview-and-loginname-controls"></a>En utilisant le LoginView et les contrôles de LoginName

Affichage du contenu différents aux utilisateurs authentifiés et anonymes est une exigence courante ; Par conséquent, affiche le nom de l’utilisateur actuellement connecté. Pour cette raison, ASP.NET propose deux contrôles Web qui fournissent les mêmes fonctionnalités que celui illustrée à la Figure 13, mais sans avoir à écrire une seule ligne de code.

Le [contrôle LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) est un contrôle Web basé sur un modèle qui le rend plus facile d’afficher des données différentes pour les utilisateurs authentifiés et anonymes. La vue de connexion inclut deux modèles prédéfinis :

- AnonymousTemplate – tout balisage ajouté à ce modèle s’affiche uniquement pour les visiteurs anonymes.
- LoggedInTemplate – balisage de ce modèle est uniquement affiché aux utilisateurs authentifiés.

Nous allons ajouter le contrôle LoginView à la page maître de notre site, Site.master. Au lieu d’ajouter simplement le contrôle LoginView, cependant, nous allons ajouter les deux un nouveau contrôle ContentPlaceHolder et placez le contrôle LoginView au sein de ce nouveau ContentPlaceHolder. La logique de cette décision deviendront bientôt évident.

> [!NOTE]
> En plus des AnonymousTemplate et LoggedInTemplate, le contrôle LoginView peut inclure des modèles de rôle spécifique. Modèles de rôle spécifique montrent balisage uniquement pour les utilisateurs qui appartiennent à un rôle spécifié. Nous allons examiner les fonctionnalités en fonction du rôle du contrôle LoginView dans un futur didacticiel.


Commencez par ajouter un ContentPlaceHolder nommé LoginContent dans la page maître dans le volet de navigation &lt;div&gt; élément. Vous pouvez simplement faire glisser un contrôle ContentPlaceHolder à partir de la boîte à outils vers la vue de Source, placer le balisage qui en résulte au-dessus de la « TODO : Menu ici... » texte.

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample9.aspx)]

Ensuite, ajoutez un contrôle LoginView dans LoginContent ContentPlaceHolder. Contenu placé dans les contrôles ContentPlaceHolder de la page maître sont considérés comme *contenu par défaut* pour ContentPlaceHolder. Autrement dit, les pages ASP.NET qui utilisent cette page maître peuvent spécifier leur propre contenu pour chaque ContentPlaceHolder ou utiliser le contenu de la page maître par défaut.

La vue de connexion et autres contrôles de connexion sont situés dans l’onglet Connexion de la boîte à outils.


![Le contrôle LoginView dans la boîte à outils](an-overview-of-forms-authentication-cs/_static/image30.png)

**Figure 14**: le contrôle LoginView dans la boîte à outils


Ensuite, ajoutez deux &lt;br /&gt; éléments immédiatement après le contrôle LoginView, mais toujours au sein de ContentPlaceHolder. À ce stade, le volet de navigation &lt;div&gt; balisage de l’élément doit ressembler à ce qui suit :

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample10.aspx)]

Les modèles de la vue de connexion peuvent être définis à partir du concepteur ou le balisage déclaratif. À partir du Concepteur de Visual Studio, développez la balise active de la vue de connexion, qui répertorie les modèles configurés dans une liste déroulante. Tapez le texte « Hello, stranger » dans AnonymousTemplate ; Ensuite, ajoutez un contrôle de lien hypertexte et définissez ses propriétés Text et NavigateUrl sur « Log In » et « ~ / Login.aspx », respectivement.

Après avoir configuré le AnonymousTemplate, basculez vers LoggedInTemplate et entrez le texte, « Bienvenue, ». Puis faites glisser un contrôle LoginName à partir de la boîte à outils dans LoggedInTemplate, en le plaçant immédiatement après le « Welcome, « texte. Le [contrôle LoginName](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginname.aspx), comme son nom implique, affiche le nom de l’utilisateur actuellement connecté. En interne, le contrôle LoginName génère simplement la propriété User.Identity.Name

Après avoir apporté ces ajouts pour les modèles de la vue de connexion, le balisage doit ressembler à ce qui suit :

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample11.aspx)]

Avec cet ajout à la page maître Site.master, chaque page de notre site Web affichera un message différent selon que l’utilisateur est authentifié. Figure 15 illustre la page Default.aspx quand consultées via un navigateur par utilisateur Jisun. Le message « Bienvenue, Jisun » est répété deux fois : une fois dans la section de navigation de la page maître sur la gauche (via le contrôle LoginView que nous venons d’ajouter) et une dans le Default.aspx de zone (via les contrôles de panneau et logique de programmation) de contenu.


![Affiche le contrôle LoginView](an-overview-of-forms-authentication-cs/_static/image31.png)

**Figure 15**: l’affiche de contrôle LoginView « Bienvenue, Jisun. »


Étant donné que nous avons ajouté la vue de connexion à la page maître, elle peut apparaître dans chaque page sur notre site. Toutefois, il peut être pages web où nous ne souhaitons pas afficher ce message. Une telle page est la page de connexion, dans la mesure où un lien vers la page de connexion semble hors place il. Étant donné que nous avons placé le contrôle LoginView dans ContentPlaceHolder dans la page maître, nous pouvons remplacer ce balisage par défaut dans notre page de contenu. Ouvrez Login.aspx et accédez au concepteur. Étant donné que nous n’avons pas explicitement défini un contrôle de contenu dans Login.aspx pour LoginContent ContentPlaceHolder dans la page maître, la page de connexion affichera le balisage de la page maître par défaut pour cette ContentPlaceHolder. Vous pouvez voir cela via le concepteur – LoginContent ContentPlaceHolder montre le balisage par défaut (le contrôle LoginView).


[![La Page de connexion affiche la valeur par défaut le contenu de la Page maître LoginContent ContentPlaceHolder](an-overview-of-forms-authentication-cs/_static/image33.png)](an-overview-of-forms-authentication-cs/_static/image32.png)

**Figure 16**: la Page de connexion affiche le contenu par défaut pour LoginContent ContentPlaceHolder's the Master Page ([cliquez pour afficher l’image en taille réelle](an-overview-of-forms-authentication-cs/_static/image34.png))


Pour remplacer le balisage par défaut pour LoginContent ContentPlaceHolder, cliquez simplement avec le bouton droit sur la région dans le concepteur et choisissez l’option créer un contenu personnalisé dans le menu contextuel. (Lors de l’utilisation de Visual Studio 2008 ContentPlaceHolder incluent des balises actives qui, lorsque sélectionné, offre la même option.) Cette opération ajoute un nouveau contrôle de contenu jusqu'à la balise de la page, ce qui nous permet de définir le contenu personnalisé pour cette page. Vous pouvez ajouter un message personnalisé ici, telles que « Veuillez vous connecter in... », mais nous allons simplement laisser ce champ vide.

> [!NOTE]
> Dans Visual Studio 2005, la création de contenu personnalisé crée vide contrôle dans la page ASP.NET de contenu. Dans Visual Studio 2008, toutefois, la création de contenu personnalisé copie par défaut contenu de la page maître dans le contrôle de contenu qui vient d’être créé. Si vous utilisez Visual Studio 2008, puis, après avoir créé le nouveau contrôle de contenu veillez à effacer le contenu copiées à partir de la page maître.


Figure 17 montre la page Login.aspx quand consultées à partir d’un navigateur après avoir apporté cette modification. Notez qu’il n’existe aucun « Hello, stranger » ou « Bienvenue, *nom d’utilisateur*» message dans le volet de navigation gauche &lt;div&gt; qu’il existe lors de la visite de Default.aspx.


[![La Page de connexion masque le balisage de la LoginContent ContentPlaceHolder par défaut](an-overview-of-forms-authentication-cs/_static/image36.png)](an-overview-of-forms-authentication-cs/_static/image35.png)

**Figure 17**: la Page de connexion masque balisage par défaut LoginContent ContentPlaceHolder ([cliquez pour afficher l’image en taille réelle](an-overview-of-forms-authentication-cs/_static/image37.png))


## <a name="step-5-logging-out"></a>Étape 5 : La déconnexion

À l’étape 3 que nous avons vu de la création d’une page de connexion pour connecter un utilisateur dans le site, mais nous devons encore voir comment déconnecter un utilisateur. En plus des méthodes pour la journalisation d’un utilisateur, la classe FormsAuthentication fournit également un [méthode SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx). La méthode SignOut détruit simplement le ticket d’authentification forms, journalisation ainsi l’utilisateur hors du site.

Offre qu'une lien de déconnexion est une fonctionnalité courante de ce type qu’ASP.NET inclut un contrôle conçu spécifiquement pour déconnecter un utilisateur. Le [contrôle LoginStatus](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginstatus.aspx) affiche un LinkButton « Connexion » ou « Déconnexion » LinkButton, en fonction de l’état de l’authentification de l’utilisateur. Un LinkButton « Connexion » est rendu pour les utilisateurs anonymes, tandis qu’un LinkButton « Déconnexion » s’affiche aux utilisateurs authentifiés. Le texte de la « Connexion » et « Déconnexion » de type LinkButton peut être configuré via le LoginStatus propriétés LoginText et LogoutText.

En cliquant sur le LinkButton « Connexion » provoque une publication (postback), qui a émis une redirection vers la page de connexion. En cliquant sur le LinkButton « Déconnexion », le contrôle LoginStatus appeler la méthode FormsAuthentication.SignOff et puis redirige l’utilisateur vers une page. La page connectée hors tension de l’utilisateur est redirigé vers dépend de la propriété LogoutAction, qui peut être assignée à une des trois valeurs suivantes :

- Actualisation : la valeur par défaut ; redirige l’utilisateur vers la page qu’ils ont été visite. Si la page qu’ils ont été visite n’autorise pas les utilisateurs anonymes, FormsAuthenticationModule redirige automatiquement l’utilisateur vers la page de connexion.

Vous pouvez peut-être la curiosité de savoir pourquoi une redirection est effectuée ici. Si l’utilisateur veut rester sur la même page, pourquoi la nécessité pour la redirection explicite ? La raison est que lorsque l’utilisateur clique sur le LinkButton « Déconnexion », l’utilisateur a toujours le ticket d’authentification de formulaires dans leur collection de cookies. Par conséquent, la demande de publication (postback) est une demande authentifiée. Le contrôle LoginStatus appelle la méthode de déconnexion, mais que se passe-t-il après que FormsAuthenticationModule a authentifié l’utilisateur. Par conséquent, une redirection explicite demande au navigateur de redemander la page. Au moment où que le navigateur demande de nouveau la page, le ticket d’authentification par formulaires a été supprimé, et par conséquent, la requête entrante est anonyme.

- Redirection : l’utilisateur est redirigée vers l’URL spécifiée par la propriété de LoginStatus LogoutPageUrl.
- RedirectToLoginPage : l’utilisateur est redirigé vers la page de connexion.

Nous allons ajouter un contrôle LoginStatus à la page maître et le configurer pour utiliser l’option de redirection pour envoyer l’utilisateur vers une page qui affiche un message confirmant que qu’ils ont été déconnectés. Commencez par créer une page dans le répertoire racine nommé déconnexion.aspx. N’oubliez pas d’associer cette page à la page maître Site.master. Ensuite, entrez un message dans le balisage de la page expliquant à l’utilisateur que vous avez été déconnectés.

Ensuite, revenez à la page maître Site.master et ajouter un contrôle LoginStatus sous la vue de connexion dans LoginContent ContentPlaceHolder. Définir la propriété du contrôle LoginStatus LogoutAction redirection et sa propriété LogoutPageUrl sur « ~ / Déconnexion.aspx ».

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample12.aspx)]

Dans la mesure où le LoginStatus se trouve en dehors du contrôle LoginView, il apparaît pour les utilisateurs anonymes ou authentifiés, mais qui est OK, car le LoginStatus correctement affichera une « Connexion » ou « Déconnexion » LinkButton. Avec l’ajout du contrôle LoginStatus, le lien hypertexte « Log In » dans AnonymousTemplate est superflu, par conséquent, supprimez-le.

Figure 18 montre Default.aspx lorsque Jisun visite. Notez que la colonne de gauche affiche le message « Bienvenue, Jisun » ainsi qu’un lien à déconnecter. Cliquant sur la LinkButton de déconnexion provoque une publication (postback), Jisun déconnecte le système et elle redirige ensuite vers déconnexion.aspx. Comme le montre la Figure 19, au terme du délai que jisun atteint déconnexion.aspx elle a déjà été déconnectée et restent donc anonyme. Par conséquent, la colonne de gauche affiche le texte « Bienvenue stranger » et un lien vers la page de connexion.


[![Affiche de default.aspx](an-overview-of-forms-authentication-cs/_static/image39.png)](an-overview-of-forms-authentication-cs/_static/image38.png)

**Figure 18**: Default.aspx affiche « Bienvenue, Jisun » avec un LinkButton « Déconnexion » ([cliquez pour afficher l’image en taille réelle](an-overview-of-forms-authentication-cs/_static/image40.png))


[![Déconnexion.aspx montre](an-overview-of-forms-authentication-cs/_static/image42.png)](an-overview-of-forms-authentication-cs/_static/image41.png)

**Figure 19**: déconnexion.aspx affiche « Bienvenue, stranger » avec un LinkButton « Connexion » ([cliquez pour afficher l’image en taille réelle](an-overview-of-forms-authentication-cs/_static/image43.png))


> [!NOTE]
> Je vous encourage à personnaliser la page déconnexion.aspx pour masquer LoginContent ContentPlaceHolder la page maître (comme nous l’avons fait pour Login.aspx à l’étape 4). La raison est que le LinkButton « Connexion » est restitué par le contrôle LoginStatus (celui sous « Hello, stranger ») envoie l’utilisateur vers la page de connexion en passant l’URL actuelle dans le paramètre de chaîne de requête ReturnUrl. En bref, si un utilisateur a déconnecté clique sur de ce LoginStatus LinkButton de « Connexion », puis journaux et dans, ils seront être redirigés vers déconnexion.aspx, ce qui peut perturber facilement de l’utilisateur.


## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, nous démarrer avec un examen du flux de travail de l’authentification de formulaires et puis activées à l’implémentation de l’authentification par formulaire dans une application ASP.NET. L’authentification par formulaire est alimentée par FormsAuthenticationModule, qui a deux tâches : identification des utilisateurs en fonction de leur ticket d’authentification par formulaires et redirection des utilisateurs non autorisés vers la page de connexion.

Classe de FormsAuthentication du .NET Framework inclut des méthodes pour la création, l’inspection et la suppression des tickets d’authentification de formulaires. La propriété de Request.IsAuthenticated et d’un objet utilisateur fournissent une assistance par programmation supplémentaire pour déterminer si une demande est authentifiée et des informations sur l’identité de l’utilisateur. Il existe également les contrôles LoginView, LoginStatus, LoginName Web et qui permettent aux développeurs un moyen rapide et sans code pour effectuer de nombreuses tâches courantes associées à la connexion. Nous allons examiner ces et autres contrôles Web associées à la connexion de façon plus détaillée dans les didacticiels futures.

Ce didacticiel fourni une vue d’ensemble rapide de l’authentification par formulaire. Nous ne pas examiner les options de configuration assorties, examinez comment cookieless travail de tickets d’authentification forms ou explorez la façon dont ASP.NET protège le contenu du ticket d’authentification de formulaires. Nous aborderons ces sujets et bien plus encore dans le [didacticiel suivant](forms-authentication-configuration-and-advanced-topics-cs.md).

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [Modifications entre la sécurité IIS6 et IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Contrôles de connexion ASP.NET](https://msdn.microsoft.com/library/d51ttbhx.aspx)
- [Professional ASP.NET 2.0 Security, l’appartenance et la gestion de rôle](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN : 978-0-7645-9698 MSSQLServer-8)
- [Le `<authentication>` élément](https://msdn.microsoft.com/library/532aee0e.aspx)
- [Le `<forms>` élément pour `<authentication>`](https://msdn.microsoft.com/library/1d3t3c61.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Vidéos de formation sur les rubriques contenues dans ce didacticiel

- [Utilisation de l’authentification par formulaire de base dans ASP.NET](../../../videos/authentication/using-basic-forms-authentication-in-aspnet.md)

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements particuliers à...

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Entraîner un réviseur pour ce didacticiel a été de ce didacticiel série a été révisée par plusieurs réviseurs utiles. Les réviseurs tête pour ce didacticiel incluent Alicja Maziarz, John Suru et Teresa Murphy. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](security-basics-and-asp-net-support-cs.md)
> [Suivant](forms-authentication-configuration-and-advanced-topics-cs.md)
