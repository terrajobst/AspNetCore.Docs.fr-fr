---
uid: web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-vb
title: Principes élémentaires de sécurité et la prise en charge de ASP.NET (VB) | Microsoft Docs
author: rick-anderson
description: Ceci est le premier didacticiel dans une série de didacticiels présentant des techniques pour l’authentification des visiteurs via un formulaire web, autoriser l’accès à partic...
ms.author: aspnetcontent
ms.date: 01/13/2008
ms.assetid: ab68a92b-fc81-40a4-a7dc-406625d2c5d4
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-vb
msc.type: authoredcontent
ms.openlocfilehash: ebd4e52720fc36bfcf86b7ef4205afcca7e2bc4a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820874"
---
<a name="security-basics-and-aspnet-support-vb"></a>Principes élémentaires de sécurité et la prise en charge de ASP.NET (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial01_Basics_vb.pdf)

> Il s’agit du premier didacticiel d’une série de didacticiels présentant des techniques pour l’authentification des visiteurs via un formulaire web, autoriser l’accès à certaines pages et fonctionnalités et la gestion des comptes d’utilisateur dans une application ASP.NET.


## <a name="introduction"></a>Introduction

Nouveautés, les forums d’une chose, sites de commerce électronique, sites Web de messagerie en ligne, sites Web du portail, les sites de réseau social que toutes ont en commun ? Elles offrent *comptes d’utilisateur*. Les sites qui proposent des comptes d’utilisateur doivent fournir un certain nombre de services. Au minimum, nouveaux visiteurs doivent être en mesure de créer un compte et le retour de visiteurs doivent être en mesure de se connecter. Ces applications web peuvent prendre des décisions basées sur l’utilisateur connecté : certaines pages ou les actions peuvent être limitées à uniquement ouvert une session des utilisateurs ou à un sous-ensemble d’utilisateurs ; autres pages peuvent afficher des informations spécifiques à l’utilisateur connecté, ou peuvent afficher plus ou moins d’informations, en fonction de l’utilisateur qui consulte la page.

Il s’agit du premier didacticiel d’une série de didacticiels présentant des techniques pour l’authentification des visiteurs via un formulaire web, autoriser l’accès à certaines pages et fonctionnalités et la gestion des comptes d’utilisateur dans une application ASP.NET. Au cours de ces didacticiels, nous allons examiner comment :

- Identifier et connecter les utilisateurs à un site Web
- Utilisez ASP. Infrastructure de l’appartenance du NET pour gérer les comptes d’utilisateur
- Créer, mettre à jour et supprimer des comptes d’utilisateur
- Limiter l’accès à une page web, directory ou des fonctionnalités spécifiques en fonction de l’utilisateur connecté
- Utilisez ASP. Framework de rôles de NET à associer des comptes d’utilisateurs aux rôles
- Gérer les rôles d’utilisateur
- Limiter l’accès à une page web, directory ou des fonctionnalités spécifiques en fonction du rôle de l’utilisateur connecté
- Personnaliser et étendre ASP. Contrôles Web de sécurité du NET

Ces didacticiels sont conçues pour être concis et fournissent des instructions pas à pas avec de nombreuses captures d’écran pour vous guident tout au long du processus visuellement. Chaque didacticiel est disponible dans les versions de Visual Basic et c# et inclut un téléchargement de l’intégralité du code utilisé. (Ce premier didacticiel se concentre sur les concepts de sécurité à partir d’un point de vue de haut niveau et par conséquent ne contient-elle pas de code associé).

Dans ce didacticiel, nous aborderons les concepts de sécurité importantes et quelles fonctionnalités sont disponibles dans ASP.NET pour vous aider à implémenter l’authentification par formulaire, l’autorisation, comptes d’utilisateurs et rôles. C’est parti !

> [!NOTE]
> La sécurité est un aspect important de toute application qui s’étend sur physiques, technologiques et les décisions de stratégie et exige un degré élevé de connaissances de planification et de domaine. Cette série de didacticiels ne vise pas comme guide pour le développement d’applications web sécurisées. Au lieu de cela, il se concentre spécifiquement sur les rôles, l’autorisation, les comptes d’utilisateur et l’authentification par formulaire. Bien que certains concepts de sécurité tournant autour de ces problèmes sont décrits dans cette série, d’autres sont laissés non explorés.


## <a name="authentication-authorization-user-accounts-and-roles"></a>L’authentification, l’autorisation, comptes d’utilisateurs et rôles

L’authentification, l’autorisation, comptes d’utilisateurs et rôles sont des quatre termes qui sera très souvent utilisé tout au long de cette série de didacticiels, donc j’aimerais prendre un moment rapide pour définir ces termes dans le contexte de la sécurité web. Dans un modèle client-serveur, tel qu’Internet, il existe de nombreux scénarios dans lesquels le serveur doit identifier le client qui effectue la requête. *Authentification* est le processus permettant de vérifier l’identité du client. Un client qui a été correctement identifié est dite *authentifié*. Un client non identifié est dite *non authentifiées* ou *anonyme*.

Systèmes d’authentification sécurisé impliquent au moins un des trois facettes suivantes : [quelque chose que vous savez, quelque chose que vous avez, ou quelque chose que vous êtes](http://www.cs.cornell.edu/Courses/cs513/2005fa/NNLauthPeople.html). La plupart des applications web s’appuient sur quelque chose que le client sait, par exemple un mot de passe ou un code confidentiel. Les informations utilisées pour identifier un utilisateur - son nom d’utilisateur et mot de passe, par exemple - sont appelés *informations d’identification*. Cette série de didacticiels se concentre sur *l’authentification par formulaire*, qui est un modèle d’authentification où les utilisateurs se connecteront au site en fournissant leurs informations d’identification dans un formulaire de page web. Nous avons rencontré ce type d’authentification avant. Accédez à n’importe quel site de commerce électronique. Lorsque vous êtes prêt à consulter, vous êtes invité à vous connecter en entrant votre nom d’utilisateur et le mot de passe dans les zones de texte sur une page web.

En plus d’identifier les clients, un serveur devrez peut-être limiter les ressources ou les fonctionnalités sont accessibles selon le client qui effectue la requête. *Autorisation* est le processus permettant de déterminer si un utilisateur particulier est autorisé à accéder à une ressource spécifique ou une fonctionnalité.

Un *compte d’utilisateur* est un magasin pour conserver les informations relatives à un utilisateur particulier. Comptes d’utilisateur doivent inclure au minimum les informations qui identifient de façon unique l’utilisateur, telles que le nom de connexion utilisateur et mot de passe. Ainsi que les informations essentielles, les comptes d’utilisateur peuvent inclure des éléments tels que : adresse de messagerie de l’utilisateur ; date et heure de que la création du compte ; date et heure de que leur dernière connectés ; nom et prénom ; numéro de téléphone ; et son adresse postale. Lorsque vous utilisez l’authentification par formulaire, les informations de compte utilisateur sont généralement stockées dans une base de données relationnel telles que Microsoft SQL Server.

Les applications Web qui prennent en charge les comptes d’utilisateur peuvent éventuellement regrouper des utilisateurs dans *rôles*. Un rôle est simplement une étiquette qui est appliquée à un utilisateur et fournit une abstraction pour la définition des règles d’autorisation et fonctionnalités au niveau de la page. Par exemple, un site Web peut inclure un rôle d’administrateur avec des règles d’autorisation qui interdisent tout le monde, mais un administrateur pour accéder à un ensemble particulier de pages web. En outre, les diverses pages qui sont accessibles à tous les utilisateurs (y compris les non administrateurs) peuvent afficher des données supplémentaires ou offrent des fonctionnalités supplémentaires quand consultées par les utilisateurs du rôle Administrateurs. À l’aide de rôles, nous pouvons définir ces règles d’autorisation sur un rôle par rôle plutôt que par l’utilisateur.

## <a name="authenticating-users-in-an-aspnet-application"></a>L’authentification des utilisateurs dans une Application ASP.NET

Lorsqu’un utilisateur entre une URL dans la fenêtre d’adresse de son navigateur ou les clics sur un lien, le navigateur rend une [protocole HTTP (Hypertext Transfer)](http://en.wikipedia.org/wiki/HTTP) demande au serveur web pour le contenu spécifié, que ce soit ASP.NET page, une image, un code JavaScript fichier, ou tout autre type de contenu. Le serveur web est chargé de retourner le contenu demandé. Ce faisant, il doit déterminer un certain nombre de choses sur la demande, y compris l’émetteur de la requête et si l’identité est autorisée à récupérer le contenu demandé.

Par défaut, les navigateurs envoient des requêtes HTTP qui ne disposent pas de toute sorte d’informations d’identification. Mais si le navigateur n’inclut pas les informations d’authentification du serveur web démarre le workflow d’authentification, qui tente d’identifier le client qui effectue la requête. Les étapes du flux de travail d’authentification varient selon le type d’authentification utilisé par l’application web. ASP.NET prend en charge trois types d’authentification : formulaires, Passport et Windows. Cette série de didacticiels se concentre sur l’authentification par formulaire, mais nous allons prendre une minute pour comparer et comparez les flux de travail et des magasins d’utilisateurs l’authentification Windows.

### <a name="authentication-via-windows-authentication"></a>Authentification via l’authentification Windows

Le flux de travail de l’authentification Windows utilise l’une des techniques d’authentification suivantes :

- Authentification de base
- Authentification Digest
- Authentification intégrée de Windows

Ces trois techniques fonctionnent dans à peu près la même façon : quand un non autorisé, une demande anonyme arrive, le serveur web envoie une réponse HTTP qui indique que l’autorisation est nécessaire pour continuer. Ensuite, le navigateur affiche une boîte de dialogue modale qui invite l’utilisateur pour leur nom d’utilisateur et le mot de passe (voir Figure 1). Ces informations sont ensuite envoyées au serveur web via un en-tête HTTP.


![Une boîte de dialogue invite l’utilisateur ses informations d’authentification](security-basics-and-asp-net-support-vb/_static/image1.png)

**Figure 1**: une boîte de dialogue invite l’utilisateur ses informations d’authentification


Les informations d’identification fournies sont validées via le Store d’utilisateur du serveur web Windows. Cela signifie que chaque utilisateur authentifié dans votre application web doit avoir un compte Windows dans votre organisation. Il s’agit de courants dans les scénarios intranet. En fait, lorsque vous utilisez l’authentification intégrée Windows dans un environnement intranet, le navigateur fournit automatiquement le serveur web avec les informations d’identification utilisées pour se connecter au réseau, en supprimant le boîte de dialogue illustrée dans la Figure 1. L’authentification Windows est idéale pour les applications intranet, il est généralement irréalisable pour applications Internet dans la mesure où vous ne souhaitez pas créer des comptes Windows pour chaque utilisateur qui s’inscrit sur votre site.

### <a name="authentication-via-forms-authentication"></a>Authentification via l’authentification par formulaire

L’authentification par formulaire, en revanche, est idéale pour les applications web Internet. Souvenez-vous que l’authentification par formulaire identifie l’utilisateur en les invitant à entrer leurs informations d’identification via un formulaire web. Par conséquent, lorsqu’un utilisateur tente d’accéder à une ressource non autorisée, ils sont automatiquement redirigés vers la page de connexion où ils peuvent entrer leurs informations d’identification. Les informations d’identification soumises sont ensuite comparées à un magasin d’utilisateurs personnalisé - généralement une base de données.

Après avoir vérifié les informations d’identification soumises, un *ticket d’authentification de formulaires* est créé pour l’utilisateur. Ce ticket indique que l’utilisateur a été authentifié et inclut des informations d’identification, telles que le nom d’utilisateur. (Le ticket d’authentification par formulaires est généralement) stocké en tant que cookie sur l’ordinateur client. Par conséquent, des visites suivantes sur le site Web incluent le ticket d’authentification de formulaires dans la requête HTTP, ce qui permet l’application web identifier l’utilisateur une fois qu’ils se sont connectés.

La figure 2 illustre le flux de travail de l’authentification de formulaires à partir d’un point de vue de haut niveau. Notez la façon dont les éléments d’authentification et autorisation dans ASP.NET jouer le rôle de deux entités distinctes. Le système d’authentification forms identifie l’utilisateur (ou des rapports qu’ils sont anonymes). Le système d’autorisation est ce qui détermine si l’utilisateur a accès à la ressource demandée. Si l’utilisateur n’est pas autorisé (comme dans la Figure 2 lorsque vous tentez de visiter anonymement ProtectedPage.aspx), le système d’autorisation signale que l’utilisateur est refusé, ce qui provoque les formulaires de système d’authentification pour rediriger automatiquement l’utilisateur à la page de connexion.

Une fois que l’utilisateur s’est connecté, les demandes HTTP suivantes incluent le ticket d’authentification par formulaires. Le système d’authentification forms identifie simplement l’utilisateur, c’est le système d’autorisation qui détermine si l’utilisateur peut accéder à la ressource demandée.


![Le flux de travail de l’authentification de formulaires](security-basics-and-asp-net-support-vb/_static/image2.png)

**Figure 2**: le flux de travail de l’authentification de formulaires


Aller dans l’authentification par formulaire beaucoup plus en détail dans les deux didacticiels,[une vue d’ensemble de l’authentification par formulaire](an-overview-of-forms-authentication-vb.md) et [Configuration de l’authentification de formulaires et des sujets avancés](forms-authentication-configuration-and-advanced-topics-vb.md). Pour plus d’informations sur ASP. Options d’authentification du NET, consultez [l’authentification ASP.NET](https://msdn.microsoft.com/library/eeyk640h.aspx).

## <a name="limiting-access-to-web-pages-directories-and-page-functionality"></a>Limiter l’accès aux Pages Web, des répertoires et des fonctionnalités de la Page

ASP.NET propose deux façons de déterminer si un utilisateur particulier a autorité pour accéder à un fichier spécifique ou un répertoire :

- **Fichier d’autorisation** : étant donné que les pages ASP.NET et les services web sont implémentées comme les fichiers qui résident sur le système de fichiers du serveur web, accéder à ces fichiers peuvent être spécifiés via les listes de contrôle d’accès (ACL). Autorisation de fichier est plus couramment utilisée avec l’authentification Windows, car les ACL sont les autorisations qui s’appliquent aux comptes de Windows. Lorsque vous utilisez l’authentification par formulaire, toutes les demandes d’au niveau du système de système d’exploitation et de fichier sont exécutées par le même compte Windows, quel que soit l’utilisateur visite le site.
- **Autorisation d’URL**-avec l’autorisation d’URL, le développeur de pages spécifie les règles d’autorisation dans Web.config. Ces règles d’autorisation spécifient ce que les utilisateurs ou les rôles sont autorisés à accéder ou soient refusées d’accéder à certaines pages ou des répertoires dans l’application.

Autorisation de fichier et l’autorisation d’URL définissent les règles d’autorisation pour accéder à une page ASP.NET particulière ou pour toutes les pages ASP.NET dans un répertoire particulier. À l’aide de ces techniques nous pouvons indiquer à ASP.NET afin de refuser les demandes sur une page particulière pour un utilisateur particulier, ou autoriser l’accès à un ensemble d’utilisateurs et refuser l’accès à tous les autres. Qu’en est-il des scénarios où tous les utilisateurs peuvent accéder à la page, mais les fonctionnalités de la page dépendent de l’utilisateur ? Par exemple, de nombreux sites qui prennent en charge les comptes d’utilisateur ont des pages qui affichent un contenu différent ou des données pour les utilisateurs authentifiés par rapport aux utilisateurs anonymes. Un utilisateur anonyme peut voir un lien pour vous connecter au site, tandis qu’un utilisateur authentifié voyez à la place un message comme, heureux de vous retrouver *nom d’utilisateur* ainsi qu’un lien à déconnecter. Un autre exemple : lors de l’affichage d’un élément à un site d’enchères vous consultez des informations différentes selon que vous êtes un enchérisseur ou celui enchères l’élément.

Ces ajustements de niveau de la page peuvent être accomplis de manière déclarative ou par programmation. Pour afficher un contenu différent pour anonyme que les utilisateurs authentifiés, il vous suffit de glisser un [contrôle LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) sur votre page, puis entrez le contenu approprié dans ses modèles AnonymousTemplate et LoggedInTemplate. Ou bien, vous pouvez déterminer par programmation si la requête actuelle est authentifiée, qui est l’utilisateur et les rôles auxquels ils appartiennent (le cas échéant). Vous pouvez utiliser ces informations pour puis afficher ou masquer des colonnes dans une grille ou de panneaux dans la page.

Cette série inclut trois didacticiels qui vous concentrer sur l’autorisation. ***Autorisation basée sur l’utilisateur***examine comment limiter l’accès à une page ou des pages dans un répertoire pour les comptes d’utilisateurs spécifiques ; ***L’autorisation basée sur des rôles*** examine en fournissant des règles d’autorisation au niveau du rôle niveau ; Enfin, le ***affichage de contenu en fonction de l’actuellement connecté utilisateur*** didacticiel explore la modification d’un particulier la page contenu et les fonctionnalités en fonction de l’utilisateur accédant à la page. Pour plus d’informations sur ASP. Les options d’autorisation de NET, consultez [autorisation ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx).

## <a name="user-accounts-and-roles"></a>Comptes d’utilisateur et des rôles

ASP. Authentification de formulaires du NET fournit une infrastructure pour permettre aux utilisateurs de se connecter à un site et leur état authentifié mémorisé une visite de page. Et l’autorisation d’URL offre une infrastructure pour limiter l’accès à des fichiers ou dossiers dans une application ASP.NET. Aucune de ces fonctionnalités, cependant, fournit un moyen pour stocker des informations de compte d’utilisateur ou de la gestion des rôles.

Avant ASP.NET 2.0, les développeurs ont été chargés de créer leurs propres magasins de l’utilisateur et le rôle. Ils étaient également sur le raccordement pour concevoir des interfaces utilisateur et écrire le code pour l’utilisateur essentiel relatives aux comptes des pages telles que la page de connexion et de la page pour créer un nouveau compte, entre autres. Sans n’importe quel framework de compte utilisateur intégrés dans ASP.NET, chaque développeur d’implémentation des comptes d’utilisateur avaient pour arriver à son propre décisions de conception sur les questions, comment puis-je stocker des mots de passe ou d’autres informations sensibles ? et quelles instructions dois-je imposent concernant la puissance et la longueur de mot de passe ?

Aujourd'hui, l’implémentation de comptes d’utilisateur dans une application ASP.NET est beaucoup plus simple grâce à la *infrastructure de l’appartenance* et intégrés de contrôles Web de connexion. L’infrastructure de l’appartenance est un petit nombre de classes dans le [espace de noms System.Web.Security](https://msdn.microsoft.com/library/system.web.security.aspx) qui fournissent des fonctionnalités pour effectuer des tâches relatives aux comptes d’utilisateur essentiels. La classe de clé dans le cadre de l’appartenance est la [classe d’appartenance](https://msdn.microsoft.com/library/system.web.security.membership.aspx), qui a des méthodes telles que :

- CreateUser
- DeleteUser
- GetAllUsers
- GetUser
- UpdateUser
- ValidateUser

L’infrastructure d’appartenance utilise le [modèle de fournisseur](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), qui sépare les API du framework de l’appartenance à partir de son implémentation. Cela permet aux développeurs d’utiliser une API commune, mais les autorise à utiliser une implémentation qui répond aux besoins personnalisés de leur application. En bref, la classe d’appartenance définit la fonctionnalité essentielle de l’infrastructure (méthodes, propriétés et événements), mais ne fournit pas réellement aucun détail d’implémentation. Au lieu de cela, les méthodes de la classe Membership appellent le fournisseur configuré, qui exécute le travail réel. Par exemple, lors de la méthode CreateUser de la classe d’appartenance est appelée, la classe d’appartenance ne sait pas les détails de la banque de l’utilisateur. Il ne sait pas si les utilisateurs sont en cours conservés dans une base de données ou dans un fichier XML ou dans un autre magasin. La classe Membership examine la configuration de l’application web pour déterminer quel fournisseur de déléguer l’appel à, et cette classe de fournisseur est chargée de réellement créer le nouveau compte d’utilisateur dans le magasin de l’utilisateur approprié. Cette interaction est illustrée dans la Figure 3.

Microsoft fournit deux classes de fournisseur d’appartenance dans le .NET Framework :

- [ActiveDirectoryMembershipProvider](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) -implémente l’API d’appartenance dans les serveurs Active Directory et Active Directory Application Mode (ADAM).
- [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) -implémente l’API d’appartenance dans une base de données SQL Server.

Cette série de didacticiels consacrée exclusivement SqlMembershipProvider.


[![Le modèle permet à différentes implémentations du fournisseur en toute transparence sur secteur dans l’infrastructure](security-basics-and-asp-net-support-vb/_static/image4.png)](security-basics-and-asp-net-support-vb/_static/image3.png)

**Figure 03**: le modèle permet à différentes implémentations du fournisseur en toute transparence sur secteur dans l’infrastructure ([cliquez pour afficher l’image en taille réelle](security-basics-and-asp-net-support-vb/_static/image5.png))


L’avantage du modèle de fournisseur est qu’autres implémentations peuvent être développées par Microsoft, des fournisseurs tiers ou les développeurs individuels et en toute transparence branchées sur l’infrastructure de l’appartenance. Par exemple, Microsoft a publié [un fournisseur d’appartenances pour les bases de données Microsoft Access](https://download.microsoft.com/download/5/5/b/55bc291f-4316-4fd7-9269-dbf9edbaada8/sampleaccessproviders.vsi). Pour plus d’informations sur les fournisseurs d’appartenances, reportez-vous à la [Provider Toolkit](https://msdn.microsoft.com/asp.net/aa336558.aspx), qui inclut une procédure pas à pas de fournisseurs d’appartenances, les exemples de fournisseurs personnalisés, plus de 100 pages de documentation sur le modèle de fournisseur et le Terminer le code source pour les fournisseurs d’appartenances intégrés (à savoir, ActiveDirectoryMembershipProvider et SqlMembershipProvider).

ASP.NET 2.0 introduit également l’infrastructure de rôles. Comme l’infrastructure de l’appartenance, l’infrastructure de rôles est construit sur le modèle de fournisseur. Son API est exposée via la [classe rôles](https://msdn.microsoft.com/library/system.web.security.roles.aspx) et le .NET Framework est fourni avec trois classes de fournisseur :

- [AuthorizationStoreRoleProvider](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) -gère les informations de rôle dans un magasin de stratégies de gestionnaire d’autorisations, telles que Active Directory ou ADAM.
- [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) -implémente les rôles dans une base de données SQL Server.
- [WindowsTokenRoleProvider](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx) -associe les informations de rôle en fonction de groupe de Windows du visiteur. Cette méthode est généralement utilisée avec l’authentification Windows.

Cette série de didacticiels consacrée exclusivement le SqlRoleProvider.

Étant donné que le modèle de fournisseur inclut une seule API orientés (les classes d’appartenance et des rôles), il est possible de générer des fonctionnalités autour de cette API sans avoir à vous soucier des détails d’implémentation - celles sont gérées par les fournisseurs sélectionnés par la page développeur. Cette API unifiée permet de Microsoft et des fournisseurs tiers de créer des contrôles Web qui interface avec les infrastructures d’appartenance et des rôles. ASP.NET est livré avec un nombre de [contrôles Web de connexion](https://msdn.microsoft.com/library/ms178329.aspx) pour l’implémentation des interfaces utilisateur de compte utilisateur courantes. Par exemple, le [contrôle Login](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) invite un utilisateur ses informations d’identification, les valide, puis les enregistre dans via l’authentification par formulaire. Le [contrôle LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) propose des modèles d’affichage différents balisages pour les utilisateurs anonymes et les utilisateurs authentifiés ou différents balisages en fonction du rôle de l’utilisateur. Et le [contrôle CreateUserWizard](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.aspx) fournit une interface utilisateur pas à pas pour la création d’un nouveau compte d’utilisateur.

Dans les coulisses les divers contrôles de connexion interagissent avec les infrastructures d’appartenance et des rôles. La plupart des contrôles de connexion peuvent être implémentées sans avoir à écrire une seule ligne de code. Nous allons examiner ces contrôles plus en détail dans les didacticiels futures, dont des techniques d’extension et personnalisation de leurs fonctionnalités.

## <a name="summary"></a>Récapitulatif

Toutes les applications web qui prennent en charge les comptes d’utilisateurs nécessitent des fonctionnalités similaires, telles que : la possibilité pour les utilisateurs peuvent se connecter et leur journaux dans état mémorisé une visite de page ; une page web pour les visiteurs à nouveau créer un compte ; et la possibilité de spécifier les ressources, les données et les fonctionnalités sont disponibles pour les utilisateurs ou les rôles pour le développeur de pages. Les tâches d’authentifier et autoriser les utilisateurs et de gestion des rôles et des comptes d’utilisateur est remarquablement facile à accomplir dans les applications ASP.NET grâce à l’authentification par formulaire, l’autorisation d’URL et les infrastructures d’appartenance et des rôles.

Au cours des prochaine plusieurs didacticiels, nous allons examiner ces aspects en créant une application web de travail à partir de zéro étape par étape. Dans le didacticiel de deux, nous allons explorer l’authentification par formulaire en détail. Nous verrons le flux de travail de l’authentification de formulaires en action, analysez le ticket d’authentification par formulaires, discuter des problèmes de sécurité et voir comment configurer le système d’authentification forms - tous lors de la création d’une application web qui permet aux visiteurs de se connecter à et.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [ASP.NET 2.0 appartenance, rôles, l’authentification par formulaire et ressources de sécurité](https://weblogs.asp.net/scottgu/ASP.NET-2.0-Membership_2C00_-Roles_2C00_-Forms-Authentication_2C00_-and-Security-Resources-)
- [Conseils de sécurité 2.0 ASP.NET](https://msdn.microsoft.com/library/ms998258.aspx)
- [Authentification ASP.NET](https://msdn.microsoft.com/library/eeyk640h.aspx)
- [Autorisation ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [Vue d’ensemble des contrôles de connexion ASP.NET](https://msdn.microsoft.com/library/ms178329.aspx)
- [Examen d’ASP.NET de 2.0 l’appartenance, rôles et profil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Comment faire pour sécuriser mon Site à l’aide de l’appartenance et les rôles ?](https://asp.net/learn/videos/video-45.aspx) (Vidéo)
- [Introduction à l’appartenance](https://msdn.microsoft.com/library/yh26yfzy.aspx)
- [Centre de développement de sécurité MSDN](https://msdn.microsoft.com/security/default.aspx)
- [Professional ASP.NET 2.0 Security, l’appartenance et la gestion de rôle](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN : 978-0-7645-9698 MSSQLServer-8)
- [Kit de ressources de fournisseur](https://msdn.microsoft.com/asp.net/aa336558.aspx)

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Entraîner un réviseur pour ce didacticiel a été de ce didacticiel série a été révisée par plusieurs réviseurs utiles. Les réviseurs tête pour ce didacticiel incluent Alicja Maziarz, John Suru et Teresa Murphy. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](forms-authentication-configuration-and-advanced-topics-cs.md)
> [Suivant](an-overview-of-forms-authentication-vb.md)
