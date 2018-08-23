---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
title: Single Sign-On (génération d’applications Cloud réalistes avec Azure) | Microsoft Docs
author: MikeWasson
description: Building Real World Cloud Apps with Azure e-book est basé sur une présentation développée par Scott Guthrie. Il explique 13 modèles et pratiques qui peuvent il...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 7d82d5e9-0619-4f22-9e03-32a6d52940a5
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
msc.type: authoredcontent
ms.openlocfilehash: feace28c373a767453b1b984f6fdc5a3d97a9217
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831742"
---
<a name="single-sign-on-building-real-world-cloud-apps-with-azure"></a>Single Sign-On (génération d’applications Cloud réalistes avec Azure)
====================
par [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Téléchargement Fix It projet](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [télécharger l’E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Le **Building Real World Cloud Apps with Azure** e-book est basé sur une présentation développée par Scott Guthrie. Il explique 13 modèles et pratiques qui peuvent vous aider à réussir le développement d’applications web pour le cloud. Pour plus d’informations sur l’e-book, consultez [le premier chapitre](introduction.md).


Pensez lorsque vous développez une application cloud de nombreux problèmes de sécurité, mais pour cette série nous allons nous concentrer sur un seul : authentification unique. Une question souvent questions des utilisateurs s’agit-il : « je suis principalement création d’applications pour les employés de ma société ; comment héberger ces applications dans le cloud et toujours leur permettre d’utiliser le même modèle de sécurité Mes employés connaître et à utilisent dans l’environnement local quand ils s’exécutent des applications qui sont hébergés au sein du pare-feu ? » Une des manières de que nous permettre ce scénario est appelée Azure Active Directory (Azure AD). Azure AD vous permet de proposer des enterprise line of business (LOB) applications via Internet, et il vous permet de proposer ces applications à des partenaires commerciaux.

## <a name="introduction-to-azure-ad"></a>Présentation d’Azure AD

[Azure AD](https://docs.microsoft.com/azure/active-directory/) fournit [Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx) dans le cloud. Fonctionnalités clés sont les suivantes :

- Il s’intègre avec Active Directory en local.
- Il permet l’authentification unique avec vos applications.
- Il prend en charge des normes ouvertes telles que [SAML](http://en.wikipedia.org/wiki/SAML_2.0), [WS-Fed](http://en.wikipedia.org/wiki/WS-Federation), et [OAuth 2.0](http://oauth.net/2/).
- Il prend en charge Enterprise [API REST Graph](https://msdn.microsoft.com/library/hh974476.aspx).

Supposons que vous avez un environnement Windows Server Active Directory en local que vous utilisez pour permettre aux employés se connectent à des applications Intranet :

![](single-sign-on/_static/image1.png)

Ce que Azure AD vous permet de faire est créer un répertoire dans le cloud. Il est une fonctionnalité gratuite et facile à configurer.

Il peut être entièrement indépendant à partir d’Active Directory en local ; Vous pouvez placer tout le monde vous souhaitez qu’il contient et authentifiez les applications Internet.

![Windows Azure Active Directory](single-sign-on/_static/image2.png)

Ou vous pouvez l’intégrer avec votre réseau local AD.

![AD et l’intégration de WAAD](single-sign-on/_static/image3.png)

Maintenant tous les employés qui peuvent s’authentifier sur site peuvent également s’authentifier sur l’Internet, sans avoir à ouvrir un pare-feu ou de déployer de nouveaux serveurs dans votre centre de données. Vous pouvez continuer à tirer parti de tout l’environnement Active Directory existant que vous connaissez et que vous utilisez aujourd'hui pour donner à vos applications internes-l’authentification unique sur la fonctionnalité.

Une fois que vous avez apportées à cette connexion entre AD et Azure AD, vous pouvez également activer vos applications web et vos appareils mobiles pour authentifier vos employés dans le cloud, et vous pouvez activer des applications tierces, telles que les applications Office 365, SalesForce.com ou Google, pour accepter votre informations d’identification des employés. Si vous utilisez Office 365, vous êtes déjà configuré avec Azure AD comme Office 365 utilise Azure AD pour l’authentification et l’autorisation.

![3e applications tierces](single-sign-on/_static/image4.png)

L’avantage de cette approche est que chaque fois que votre organisation ajoute ou supprime un utilisateur, ou un utilisateur modifie un mot de passe, vous utilisez le même processus que vous utilisez aujourd'hui dans votre environnement local. Tous les de votre réseau local AD modifications sont automatiquement propagées à l’environnement de cloud.

Si votre entreprise est à l’aide ou le déplacement vers Office 365, la bonne nouvelle est que vous avez Azure AD configuré automatiquement, car Office 365 utilise Azure AD pour l’authentification. Par conséquent, vous pouvez facilement utiliser dans vos propres applications la même authentification qui utilise Office 365.

## <a name="set-up-an-azure-ad-tenant"></a>Configurer un client Azure AD

un annuaire Azure AD est appelé une annonce Azure [locataire](https://technet.microsoft.com/library/jj573650.aspx), et la configuration d’un client est assez simple. Nous allons vous montrer comment procéder dans le portail de gestion Azure afin d’illustrer les concepts, mais bien sûr comme les autres fonctions de portail vous pouvez également le faire à l’aide d’un script ou une API de gestion.

Dans le portail de gestion, cliquez sur l’onglet Active Directory.

![WAAD dans le portail](single-sign-on/_static/image5.png)

Vous disposez automatiquement d’un locataire Azure AD pour votre compte Azure, et vous pouvez cliquer sur le **ajouter** bouton en bas de la page pour créer d’autres annuaires. Vous souhaiterez un pour un environnement de test et un environnement de production, par exemple. Réfléchir soigneusement à ce que vous nommez un nouveau répertoire. Si vous utilisez votre nom pour le répertoire et puis vous utilisez votre nom encore pour l’un des utilisateurs, ce qui peuvent prêter à confus.

![Ajouter un répertoire](single-sign-on/_static/image6.png)

Le portail offre une prise en charge complète pour la création, suppression et la gestion des utilisateurs au sein de cet environnement. Par exemple, pour ajouter un utilisateur doit accéder à la **utilisateurs** onglet et cliquez sur le **ajouter un utilisateur** bouton.

![Bouton Ajouter un utilisateur](single-sign-on/_static/image7.png)

![Ajouter la boîte de dialogue utilisateur](single-sign-on/_static/image8.png)

Vous pouvez créer un nouvel utilisateur qui existe uniquement dans ce répertoire, ou vous pouvez inscrire un Account Microsoft en tant qu’un utilisateur dans ce répertoire, ou le Registre ou un utilisateur à partir d’un autre annuaire Azure AD en tant qu’utilisateur dans ce répertoire. (Dans un répertoire réel, le domaine par défaut serait ContosoTest.onmicrosoft.com. Vous pouvez également utiliser un domaine de votre choix, comme contoso.com.)

![Types utilisateur](single-sign-on/_static/image9.png)

![Ajouter la boîte de dialogue utilisateur](single-sign-on/_static/image10.png)

Vous pouvez affecter l’utilisateur à un rôle.

![Profil utilisateur](single-sign-on/_static/image11.png)

Et le compte est créé avec un mot de passe temporaire.

![Mot de passe temporaire](single-sign-on/_static/image12.png)

Les utilisateurs de créez de cette façon, vous peuvent vous connecter immédiatement à vos applications web à l’aide de cet annuaire de cloud.

Ce qui est idéal pour l’entreprise l’authentification unique, cependant, est la **intégration d’annuaire** onglet :

![Onglet de l’intégration d’annuaire](single-sign-on/_static/image13.png)

Si vous activez l’intégration d’annuaire, et [Téléchargez un outil](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx), vous pouvez synchroniser cet annuaire de cloud avec votre local Active Directory existant que vous utilisez déjà à l’intérieur de votre organisation. Puis tous les utilisateurs stockés dans votre annuaire seront afficheront dans cet annuaire de cloud. Vos applications cloud peuvent désormais s’authentifier tous les employés à l’aide de leurs informations d’identification Active Directory existantes. Et tout cela est gratuit – l’outil de synchronisation et Azure AD elle-même.

L’outil est un Assistant qui est facile à utiliser, comme vous pouvez le voir à partir de ces captures d’écran. Il ne s’agit pas d’obtenir des instructions complètes, juste un exemple vous montrant le processus de base. Pour plus d’informations how-to--it, consultez les liens dans le [ressources](#resources) section à la fin de ce chapitre.

![Assistant de configuration d’outil de synchronisation de WAAD](single-sign-on/_static/image14.png)

Cliquez sur **suivant**, puis entrez vos informations d’identification Azure Active Directory.

![Assistant de configuration d’outil de synchronisation de WAAD](single-sign-on/_static/image15.png)

Cliquez sur **suivant**, puis entrez votre réseau local informations d’identification AD.

![Assistant de configuration d’outil de synchronisation de WAAD](single-sign-on/_static/image16.png)

Cliquez sur **suivant**, puis indiquez si vous souhaitez stocker un hachage de vos mots de passe AD dans le cloud.

![Assistant de configuration d’outil de synchronisation de WAAD](single-sign-on/_static/image17.png)

Le hachage de mot de passe que vous pouvez stocker dans le cloud est un hachage à sens unique ; les mots de passe réels ne sont jamais stockés dans Azure AD. Si vous décidez par rapport à stocker les hachages dans le cloud, vous devrez utiliser [Active Directory Federation Services](https://technet.microsoft.com/library/hh831502.aspx) (ADFS). Il existe également [autres facteurs à prendre en compte quand choisir s’il faut utiliser ADFS](https://technet.microsoft.com/library/jj573653.aspx). L’option ADFS nécessite quelques étapes de configuration supplémentaires.

Si vous choisissez de stocker les hachages dans le cloud, vous avez terminé, et l’outil démarre la synchronisation des annuaires lorsque vous cliquez sur **suivant**.

![Assistant de configuration d’outil de synchronisation de WAAD](single-sign-on/_static/image18.png)

Et en quelques minutes, vous avez terminé.

![Assistant de configuration d’outil de synchronisation de WAAD](single-sign-on/_static/image19.png)

Vous devez uniquement exécuter cette procédure sur un contrôleur de domaine de l’organisation, sur Windows 2003 ou version ultérieure. Et sans devoir redémarrer. Lorsque vous avez terminé, tous vos utilisateurs se trouvent dans le cloud et vous pouvez effectuer l’authentification unique à partir de toute application web ou mobile, à l’aide de SAML, OAuth ou WS-Fed.

Parfois, nous demande souvent sur la sécurisation il s’agit de : Microsoft l’utiliser pour leurs propres données métier sensibles ? Et la réponse est Oui, que nous faisons. Par exemple, si vous consultez le site Microsoft SharePoint interne à [ https://microsoft.sharepoint.com/ ](https://microsoft.sharepoint.com/), vous êtes invité à se connecter.

![Connexion à Office 365](single-sign-on/_static/image20.png)

Microsoft a activé l’ADFS, donc lorsque vous entrez un ID Microsoft, vous êtes redirigé vers une page de connexion AD FS.

![L’authentification dans AD FS](single-sign-on/_static/image21.png)

Et une fois que vous entrez des informations d’identification stockées dans un compte Microsoft AD interne, vous avez accès à cette application interne.

![Site SharePoint de MS](single-sign-on/_static/image22.png)

Nous utilisons un serveur de connexion AD principalement parce que nous avions déjà ADFS configurer avant d’Azure AD est devenue disponible, mais le processus de connexion est en cours sur un annuaire Azure AD dans le cloud. Nous nos documents importants, contrôle de code source, des fichiers de gestion des performances, rapports de ventes, etc., dans le cloud et sont à l’aide de cette même solution exacte afin de les sécuriser.

## <a name="create-an-aspnet-app-that-uses-azure-ad-for-single-sign-on"></a>Créer une application ASP.NET qui utilise Azure AD pour l’authentification unique

Visual Studio vous permet de vraiment facile de créer une application qui utilise Azure AD pour l’authentification unique, comme vous pouvez le voir à partir de quelques captures d’écran.

Lorsque vous créez une nouvelle application ASP.NET, MVC ou Web Forms, la méthode d’authentification par défaut est ASP.NET Identity. Pour remplacer cette Azure AD, vous cliquez sur un **modifier l’authentification** bouton.

![Modifier l’authentification](single-sign-on/_static/image23.png)

Sélectionnez les comptes de société, entrez votre nom de domaine, puis sélectionnez l’authentification unique.

![Configurer la boîte de dialogue authentification](single-sign-on/_static/image24.png)

Vous pouvez également donner à la lecture de l’application ou en lecture/écriture pour les données d’annuaire. Si vous procédez ainsi, il peut utiliser le [Azure Graph API REST](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx) pour rechercher le numéro de téléphone des utilisateurs, déterminer s’ils sont au bureau, lors de leur dernière ouverture de session, etc.

C’est qu’il vous suffit - Visual Studio vous demande les informations d’identification pour un administrateur pour votre locataire Azure AD, et il configure ensuite votre projet et votre client Azure AD pour la nouvelle application.

Lorsque vous exécutez le projet, vous verrez une page de connexion, et vous pouvez connecter avec les informations d’identification d’un utilisateur dans votre annuaire Azure AD.

![Organisation compte connectez-vous](single-sign-on/_static/image25.png)

![Connecté](single-sign-on/_static/image26.png)

Lorsque vous déployez l’application sur Azure, il vous suffit est sélectionné un **activer l’authentification d’organisation** case à cocher et une fois encore Visual Studio s’occupe de toute la configuration pour vous.

![Publier le site Web](single-sign-on/_static/image27.png)

Ces captures d’écran proviennent d’un didacticiel pas à pas complet qui montre comment générer une application qui utilise l’authentification Azure AD : [développement des applications ASP.NET avec Azure Active Directory](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md).

## <a name="summary"></a>Récapitulatif

Dans ce chapitre, vous avez vu qu’Azure Active Directory, Visual Studio et ASP.NET, rendent facile à configurer l’authentification unique dans les applications Internet pour les utilisateurs de votre organisation. Vos utilisateurs à se connecter les applications Internet utilisant les informations d’identification qu’ils utilisent pour se connecter à l’aide d’Active Directory dans votre réseau interne.

Le [chapitre suivant](data-storage-options.md) examine les options de stockage de données disponibles pour une application cloud.

<a id="resources"></a>
## <a name="resources"></a>Ressources

Pour plus d'informations, reportez-vous aux ressources suivantes :

- [Documentation Azure Active Directory](https://docs.microsoft.com/azure/active-directory/). Page du portail de documentation d’Azure AD sur le site windowsazure.com. Pour les guides étape par étape, consultez la **développer** section.
- [Azure multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/). Page du portail de la documentation sur l’authentification multifacteur dans Azure.
- [Options d’authentification de compte de société](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions). Explication des options d’authentification Azure AD dans la boîte de dialogue Nouveau projet Visual Studio 2013.
- [Modèle d’identité fédérée de Microsoft Patterns and Practices -](https://msdn.microsoft.com/library/dn589790.aspx).
- [Comment faire : Installer l’outil Azure Directory Sync Active](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx).
- [Active Directory Federation Services 2.0 Content Map](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx). Liens vers la documentation sur AD FS 2.0.
- [Autorisation basée sur les ACL et les rôles dans une Application Windows Azure AD](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1). Exemple d’application.
- [Blog de l’API Graph Active Directory Azure](https://blogs.msdn.com/b/aadgraphteam/).
- [Contrôle d’accès dans BYOD et intégration d’annuaire dans une Infrastructure d’identité hybride](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=). Tech Ed 2014 session vidéo par Gayana Bagdasaryan.

> [!div class="step-by-step"]
> [Précédent](web-development-best-practices.md)
> [Suivant](data-storage-options.md)
