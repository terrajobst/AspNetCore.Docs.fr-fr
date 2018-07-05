---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-vb
title: Configuration d’un site Web qui utilise les Services d’Application (VB) | Microsoft Docs
author: rick-anderson
description: ASP.NET version 2.0 a introduit une série de services d’application, qui font partie du .NET Framework et de répondre à une suite de bloc de construction de services qui yo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 9c31a42f-d8bb-4c0f-9ccc-597d4f70ac42
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-vb
msc.type: authoredcontent
ms.openlocfilehash: 69c8bb4061c2ed7fcc44038aa3bf0268ac86c9ab
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387492"
---
<a name="configuring-a-website-that-uses-application-services-vb"></a>Configuration d’un site Web qui utilise les Services d’Application (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_09_VB.zip) ou [télécharger le PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial09_AppServicesConfig_vb.pdf)

> ASP.NET version 2.0 a introduit une série de services d’application, qui font partie du .NET Framework et servent à une suite de services de bloc de construction que vous pouvez utiliser pour ajouter des fonctionnalités enrichies à votre application web. Ce didacticiel explique comment configurer un site Web dans l’environnement de production à utiliser les services d’application et résout les problèmes courants avec la gestion des comptes d’utilisateurs et rôles sur l’environnement de production.


## <a name="introduction"></a>Introduction

ASP.NET version 2.0 a introduit une série de *les services d’application*, qui font partie du .NET Framework et constituent une suite de bloc de construction de services que vous pouvez utiliser pour ajouter des fonctionnalités enrichies à votre application web. Les services d’application sont les suivantes :

- **L’appartenance** : API pour créer et gérer des comptes d’utilisateur.
- **Rôles** : API pour classer les utilisateurs en groupes.
- **Profil** : API de stockage du contenu personnalisé, spécifiques à l’utilisateur.
- **Plan du site** : API permettant de définir une structure logique du site s sous la forme d’une hiérarchie, ce qui peut ensuite être affichée via des contrôles de navigation, notamment les menus et barres de navigation.
- **Personnalisation** : API pour gérer les préférences de personnalisation, plus souvent utilisés avec [ *WebParts*](https://msdn.microsoft.com/library/e0s9t4ck.aspx).
- **Analyse d’intégrité** : API pour la surveillance des performances, la sécurité, les erreurs et autres mesures de contrôle d’intégrité système pour une application web en cours d’exécution.
  

Les services d’application API ne sont pas liés à une implémentation spécifique. Au lieu de cela, vous demandez les services d’application à utiliser un particulier *fournisseur*, et ce fournisseur implémente le service à l’aide d’une technologie particulière. Les fournisseurs couramment utilisés pour les applications web basées sur Internet hébergées sur une société d’hébergement web sont les fournisseurs qui utilisent une implémentation de base de données SQL Server. Par exemple, le `SqlMembershipProvider` est un fournisseur pour l’API d’appartenance qui stocke les informations de compte utilisateur dans une base de données Microsoft SQL Server.

Les services d’application et les fournisseurs de SQL Server ajoute des défis lors du déploiement de l’application. Pour commencer, les objets de base de données d’application services doivent être créées correctement sur les bases de données à la fois le développement et de production et initialisés correctement. Il existe également des paramètres de configuration importants qui doivent être apportées.

> [!NOTE]
> Les services d’application API ont été conçues à l’aide de la [ *modèle de fournisseur*](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), un modèle de conception qui permet une API s détails d’implémentation doit être fourni lors de l’exécution. Le .NET Framework est livré avec un nombre de fournisseurs de services d’application qui peut être utilisée, telle que la `SqlMembershipProvider` et `SqlRoleProvider`, qui sont des fournisseurs pour l’appartenance et les API de rôles qui utilisent un serveur SQL Server de base de données mise en œuvre. Vous pouvez également créer et plug-in un fournisseur personnalisé. En fait, les critiques de livres web application contient déjà un fournisseur personnalisé pour l’API de mappage de Site (`ReviewSiteMapProvider`), qui construit le plan de site à partir des données dans le `Genres` et `Books` tables dans la base de données.


Ce didacticiel commence par examiner comment j’ai étendu de l’application web de critiques de livres à utiliser l’appartenance et l’API de rôles. Ensuite, il guide de déploiement d’une application web qui utilise les services d’application avec une implémentation de base de données SQL Server et se termine en résolvant les problèmes courants liés à la gestion des comptes d’utilisateurs et rôles sur l’environnement de production.

## <a name="updates-to-the-book-reviews-application"></a>Mises à jour de l’Application d’évaluations de livre

Dernières didacticiels de que l’application web passe en revue du livre a été mis à jour à partir d’un site Web statique à une application web dynamique piloté par les données se termine avec un ensemble de pages d’administration pour la gestion des genres et révisions. Toutefois, cette section d’administration n’est actuellement pas protégée : tout utilisateur qui sait (ou devine) l’URL de page d’administration permettre s’introduire dans et créer, modifier ou supprimer les révisions sur notre site. Une méthode courante pour protéger certaines parties d’un site Web consiste à implémenter des comptes d’utilisateur et ensuite utiliser des règles d’autorisation d’URL pour restreindre l’accès à certains utilisateurs ou les rôles. L’application web critiques de livres disponible pour téléchargement avec ce didacticiel prend en charge les rôles et les comptes d’utilisateur. Il dispose d’un seul rôle défini nommé Administrateur, et seuls les utilisateurs de ce rôle peuvent accéder les pages d’administration.

> [!NOTE]
> Je ve créé trois comptes d’utilisateurs dans l’application web de critiques de livres : Scott, Jisun et Alice. Tous les utilisateurs de trois ont le même mot de passe : **mot de passe !** Scott et Jisun se trouvent dans le rôle d’administrateur, Alice n’est pas. Les pages de non-administration de site s sont toujours accessibles aux utilisateurs anonymes. Autrement dit, vous n’avez pas besoin pour vous connecter à visiter le site, sauf si vous souhaitez administrer, auquel cas vous devez être connecté en tant qu’utilisateur dans le rôle d’administrateur.


La page maître de critiques de livres application s a été mis à jour pour inclure une interface utilisateur différente pour les utilisateurs authentifiés et anonymes. Si un utilisateur anonyme visite le site, il voit un lien de connexion dans le coin supérieur droit. Un utilisateur authentifié voit le message « Bienvenue, *nom d’utilisateur*! » et un lien pour se déconnecter. Cet emplacement s également une page de connexion (`~/Login.aspx`), qui contient un contrôle Web de connexion qui fournit l’interface utilisateur et la logique d’authentification d’un visiteur. Seuls les administrateurs peuvent créer de nouveaux comptes. (Il existe des pages pour créer et gérer des comptes d’utilisateur dans le `~/Admin` dossier.)

### <a name="configuring-the-membership-and-roles-apis"></a>Configuration de l’appartenance et l’API de rôles

L’application web de critiques de livres utilise l’appartenance et l’API de rôles pour prendre en charge les comptes d’utilisateur et pour regrouper ces utilisateurs dans des rôles (à savoir, le rôle d’administrateur). Le `SqlMembershipProvider` et `SqlRoleProvider` des classes de fournisseur sont utilisées, car nous voulons stocker les informations de compte et le rôle dans une base de données SQL Server.

> [!NOTE]
> Ce n’est pas vocation à être un examen détaillé à la configuration d’une application web pour prendre en charge l’appartenance et l’API de rôles. Pour obtenir une présentation approfondie à ces API et les étapes à suivre pour configurer un site Web pour les utiliser, lisez mon [ *didacticiels de sécurité de site Web*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


Pour utiliser les services d’application avec une base de données SQL Server, vous devez tout d’abord ajouter les objets de base de données utilisés par ces fournisseurs pour la base de données où vous souhaitez que le compte d’utilisateur et les informations de rôle stockées. Ces objets de base de données requis incluent une variété de tables, vues et procédures stockées. Sauf indication contraire, le `SqlMembershipProvider` et `SqlRoleProvider` des classes de fournisseur utilisent une base de données SQL Server Express Edition nommé `ASPNETDB` situé dans le s application `App_Data` dossier ; si une base de données n’existe pas, il est automatiquement créé avec les objets de base de données nécessaires par ces fournisseurs lors de l’exécution.

Il est possible, et généralement idéal pour créer des objets de base de données dans la même base de données où sont stockées les données spécifiques à l’application de s site Web services de l’application. Le .NET Framework est livré avec un outil nommé `aspnet_regsql.exe` qui installe les objets de base de données sur une base de données spécifiée. J’ai poursuivi et cet outil permet d’ajouter ces objets à la `Reviews.mdf` de base de données dans le `App_Data` dossier (la base de données de développement). Nous verrons comment utiliser cet outil plus loin dans ce didacticiel lorsque nous ajoutons à ces objets à la base de données de production.

Si vous ajoutez l’application services autres que les objets de base de données à une base de données `ASPNETDB` vous devez personnaliser le `SqlMembershipProvider` et `SqlRoleProvider` fournisseur classes configurations afin qu’ils utilisent la base de données approprié. Pour personnaliser le fournisseur d’appartenances ajouter un [  *&lt;appartenance&gt; élément* ](https://msdn.microsoft.com/library/1b9hw62f.aspx) au sein de la `<system.web>` section dans `Web.config`; utiliser le [  *&lt;roleManager&gt; élément* ](https://msdn.microsoft.com/library/ms164660.aspx) pour configurer le fournisseur de rôles. L’extrait de code suivant provient de la s d’application critiques de livres `Web.config` et affiche les paramètres de configuration pour l’appartenance et l’API de rôles. Notez que les deux inscrire un nouveau fournisseur - `ReviewMembership` et `ReviewRole` -qui utilisent le `SqlMembershipProvider` et `SqlRoleProvider` fournisseurs, respectivement.

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample1.xml)]

Le `Web.config` fichier s `<authentication>` élément a également été configuré pour prendre en charge l’authentification basée sur les formulaires.
  

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample2.xml)]

### <a name="limiting-access-to-the-administration-pages"></a>Limiter l’accès aux Pages d’Administration

ASP.NET facilite l’accorder ou refuser l’accès à un fichier particulier ou un dossier par utilisateur ou par rôle via son *l’autorisation d’URL* fonctionnalité. (Nous avons abordé brièvement l’autorisation d’URL dans le *principales différences entre IIS et le serveur de développement ASP.NET* didacticiel et vous a montré comment IIS et le serveur de développement ASP.NET appliquent des règles d’autorisation d’URL différemment pour statique par rapport à un contenu dynamique.) Étant donné que nous souhaitons interdire l’accès à la `~/Admin` dossier à l’exception de ces utilisateurs dans le rôle d’administrateur, nous devons ajouter des règles d’autorisation d’URL dans ce dossier. Plus précisément, les règles d’autorisation d’URL devront autoriser les utilisateurs dans le rôle d’administrateur et refuser tous les autres utilisateurs. Cela s’effectue en ajoutant un `Web.config` de fichiers à la `~/Admin` dossier avec le contenu suivant :

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample3.xml)]

Pour plus d’informations sur la fonctionnalité d’autorisation ASP.NET s URL et comment l’utiliser pour écrire des règles d’autorisation pour les utilisateurs et pour les rôles, veillez à lire le [ *autorisation basée sur l’utilisateur* ](../../older-versions-security/membership/user-based-authorization-vb.md) et [ *Autorisation basée sur le rôle* ](../../older-versions-security/roles/role-based-authorization-vb.md) didacticiels à partir de mon [ *didacticiels de sécurité de site Web*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

## <a name="deploying-a-web-application-that-uses-application-services"></a>Déploiement d’une Application Web qui utilise les Services d’Application

Lorsque vous déployez un site Web qui utilise les services d’application et un fournisseur qui stocke les informations de services d’application dans une base de données, il est impératif que les objets de base de données requis par les services d’application créés sur la base de données de production. Initialement la base de données de production ne contient pas de ces objets, donc lorsque l’application est déployée tout d’abord (ou lorsqu’elle est déployée pour la première fois après ont ajouté des services d’application), vous devez prendre des mesures supplémentaires pour obtenir ces objets de base de données requis sur le base de données de production.

Un autre défi peut se produire lors du déploiement d’un site Web qui utilise les services d’application si vous souhaitez répliquer les comptes d’utilisateur créés dans l’environnement de développement à l’environnement de production. Selon la configuration de l’appartenance et les rôles, il est possible que, même si vous copiez avec succès les comptes d’utilisateurs qui ont été créés dans l’environnement de développement pour la base de données de production, ces utilisateurs ne peuvent pas se connecter à l’application web en production. Nous allons examiner la cause de ce problème et discuter des méthodes pour l’empêcher de se produire.

ASP.NET est livré avec une belle [ *outil d’Administration de Site Web (WSAT)* ](https://msdn.microsoft.com/library/yy40ytx0.aspx) qui peut être lancé à partir de Visual Studio et permet à l’utilisateur compte, rôles, règles d’autorisation et d’être gérés via un sur le web interface. Malheureusement, le WSAT fonctionne uniquement pour les sites Web locales, ce qui signifie qu’il ne peut pas être utilisé pour gérer à distance des comptes d’utilisateur, les rôles et les règles d’autorisation pour l’application web dans l’environnement de production. Nous allons examiner les différentes façons d’implémenter le comportement de WSAT à partir de votre site Web de production.

### <a name="adding-the-database-objects-using-aspnetregsqlexe"></a>Ajouter le compte aspnet à l’aide des objets de base de données\_regsql.exe

Le *déploiement d’une base de données* didacticiel vous a montré comment copier les tables et les données à partir de la base de données de développement à la base de données de production, et ces techniques peuvent certainement être utilisées pour copier les objets de base de données de services application à la base de données de production. Une autre option est la `aspnet_regsql.exe` outil, qui ajoute ou supprime les objets de base de données d’application services à partir d’une base de données.

> [!NOTE]
> Le `aspnet_regsql.exe` outil crée les objets de base de données sur une base de données spécifiée. Il ne migre pas les données dans ces objets de base de données à partir de la base de données de développement à la base de données de production. Si vous souhaitez copier les informations de compte et le rôle d’utilisateur dans la base de données de développement à la base de données de production utiliser les techniques décrites dans le *déploiement d’une base de données* didacticiel.


S permettent de voir comment ajouter les objets de base de données à la base de données de production à l’aide du `aspnet_regsql.exe` outil. Commencez par ouvrir l’Explorateur Windows et accédez au répertoire .NET Framework version 2.0 sur votre ordinateur, %WINDIR%\ Microsoft.NET\Framework\v2.0.50727. Vous devriez voir le `aspnet_regsql.exe` outil. Cet outil peut être utilisé à partir de la ligne de commande, mais il inclut également une interface utilisateur graphique ; Double-cliquez sur le `aspnet_regsql.exe` fichier pour lancer son composant graphique.

L’outil démarre en affichant un écran de démarrage qui explique son objectif. Cliquez sur Suivant pour passer à l’écran « Sélectionnez une Option de configuration », qui est indiqué dans la Figure 1. À partir de là, vous pouvez choisir d’ajouter les services d’application les objets de base de données ou les supprimer à partir d’une base de données. Étant donné que nous souhaitons ajouter ces objets à la base de données de production, sélectionnez l’option « Configurer SQL Server pour les services d’application » et cliquez sur Suivant.


[![Choisissez de configurer SQL Server pour les Services d’Application](configuring-a-website-that-uses-application-services-vb/_static/image2.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image1.jpg)

**Figure 1**: choisir de configurer SQL Server pour les Services d’Application ([cliquez pour afficher l’image en taille réelle](configuring-a-website-that-uses-application-services-vb/_static/image3.jpg))


« Sélectionnez le serveur et base de données » invites pour plus d’informations pour vous connecter à la base de données. Entrez le nom de la base de données fournie par votre société d’hébergement web, le serveur de base de données et les informations d’identification de sécurité et cliquez sur Suivant.

> [!NOTE]
> Après avoir entré votre serveur de base de données et les informations d’identification, vous pouvez obtenir une erreur lors du développement de la liste déroulante de base de données. Le `aspnet_regsql.exe` outil requêtes le `sysdatabases` (table système) pour récupérer une liste de bases de données sur le serveur, mais certains web hébergeant les verrouiller de sociétés de leurs serveurs de base de données afin que ces informations ne sont pas disponibles publiquement. Si vous obtenez cette erreur, vous pouvez taper le nom de la base de données directement dans la liste déroulante.


[![Fournir l’outil avec vos informations de connexion de base de données s](configuring-a-website-that-uses-application-services-vb/_static/image5.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image4.jpg)

**Figure 2**: fournir les informations de connexion de s avec votre base de l’outil ([cliquez pour afficher l’image en taille réelle](configuring-a-website-that-uses-application-services-vb/_static/image6.jpg))


L’écran suivant récapitule les actions qui doivent avoir lieu, à savoir ce qui les objets de base de données d’application services vont être ajoutés à la base de données spécifié. Cliquez sur Suivant pour terminer cette action. Après quelques instants, le dernier écran s’affiche, en notant que les objets de base de données ont été ajoutés (voir Figure 3).


[![Succès ! Les objets de base de données d’Application Services ont été ajoutés à la base de données de Production](configuring-a-website-that-uses-application-services-vb/_static/image8.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image7.jpg)

**Figure 3**: succès ! L’Application Services base de données objets ont été ajoutés à la base de données de Production ([cliquez pour afficher l’image en taille réelle](configuring-a-website-that-uses-application-services-vb/_static/image9.jpg))


Pour vérifier que les objets de base de données d’application services ont été ajoutés à la base de données de production, ouvrez SQL Server Management Studio et connectez-vous à votre base de données de production. Comme le montre la Figure 4, vous devez maintenant voir les tables de base de données d’application services dans votre base de données, `aspnet_Applications`, `aspnet_Membership`, `aspnet_Users`, et ainsi de suite.


[![Vérifier que les objets de base de données ont été ajoutés à la base de données de Production](configuring-a-website-that-uses-application-services-vb/_static/image11.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image10.jpg)

**Figure 4**: vérifier que les objets de base de données ont été ajoutés à la base de données de Production ([cliquez pour afficher l’image en taille réelle](configuring-a-website-that-uses-application-services-vb/_static/image12.jpg))


Il vous suffit d’utiliser le `aspnet_regsql.exe` outil lors du déploiement de votre application web pour la première fois ou pour la première fois après avoir démarré à l’aide des services d’application. Une fois ces objets de base de données sur la base de données de production qu’ils ont remporté t devoir être ajouté de nouveau ou modifié.

### <a name="copying-user-accounts-from-development-to-production"></a>Copie des comptes d’utilisateur à partir de développement en Production

Lorsque vous utilisez le `SqlMembershipProvider` et `SqlRoleProvider` des classes de fournisseur pour stocker les informations de services d’application dans une base de données SQL Server, les informations de compte et le rôle utilisateur sont stockées dans une variété de tables de base de données, y compris `aspnet_Users`, `aspnet_Membership`, `aspnet_Roles`, et `aspnet_UsersInRoles`, entre autres. Si au cours du développement, vous créez des comptes d’utilisateur dans l’environnement de développement, vous pouvez répliquer ces comptes d’utilisateurs en production en copiant les enregistrements correspondants à partir des tables de base de données applicable. Si vous avez utilisé l’Assistant Publication de base de données pour déployer les objets de base de données de services application que vous pourrez également ont choisi de copier les enregistrements, ce qui entraîneraient les comptes d’utilisateur créés dans le développement pour également être mise en production. Mais, en fonction de vos paramètres de configuration, vous découvrez que les utilisateurs dont les comptes ont été créés dans le développement et copiés en production ne parvenez pas à se connecter depuis le site Web de production. Ce qui donne ?

Le `SqlMembershipProvider` et `SqlRoleProvider` classes de fournisseur ont été conçus pour une base de données unique peut servir un magasin d’utilisateurs pour doivent de plusieurs applications, où chaque application peut, en théorie, les utilisateurs avec des noms d’utilisateur qui se chevauchent et les rôles portant le même nom. Pour permettre cette flexibilité, la base de données gère une liste d’applications dans le `aspnet_Applications` table et chaque utilisateur est associé à un de ces applications. Plus précisément, le `aspnet_Users` table a un `ApplicationId` colonne qui lie chaque utilisateur à un enregistrement dans le `aspnet_Applications` table.

Outre le `ApplicationId` colonne, le `aspnet_Applications` table inclut également un `ApplicationName` colonne, qui fournit un nom plus convivial pour l’application. Lorsqu’un site Web tente de travailler avec un compte d’utilisateur, telles que la validation d’informations d’identification utilisateur s à partir de la page de connexion, il doit indiquer la `SqlMembershipProvider` classe application à utiliser. Il généralement effectue cela en fournissant le nom de l’application et cette valeur provient de la configuration du fournisseur s dans `Web.config` , en particulier via la `applicationName` attribut.

Mais que se passe-t-il si le `applicationName` attribut n’est pas spécifié dans `Web.config`? Dans ce cas l’appartenance au système utilise le chemin d’accès racine en tant que le `applicationName` valeur. Si le `applicationName` attribut n’est pas explicitement défini `Web.config`, puis, il existe un risque que l’environnement de développement et l’environnement de production utilisent une racine d’application différents et par conséquent à associer à une autre application noms dans les services d’application. Si une telle non-correspondance se produit ensuite ces utilisateurs créés dans l’environnement de développement aura une `ApplicationId` valeur ne correspond pas à la `ApplicationId` valeur pour l’environnement de production. Le résultat net est que ces utilisateurs ont remporté le t être autorisés à se connecter.

> [!NOTE]
> Si vous vous trouvez dans cette situation - comptes d’utilisateur copié en production avec un incompatibles `ApplicationId` valeur - vous pouvez écrire une requête pour mettre à jour ces incorrect `ApplicationId` valeurs à la `ApplicationId` utilisé sur la production. Une fois la mise à jour, les utilisateurs dont les comptes ont été créés sur l’environnement de développement seraient maintenant être en mesure de se connecter à l’application web sur la production.


La bonne nouvelle est qu’il existe une étape simple, vous pouvez prendre pour vous assurer que les deux environnements utilisent les mêmes `ApplicationId` - définissez explicitement le `applicationName` d’attribut dans `Web.config` pour l’ensemble de vos fournisseurs de services d’application. Définir explicitement la `applicationName` attribut « BookReviews » dans le `<membership>` et `<roleManager>` éléments en tant que cet extrait de code à partir de `Web.config` montre.

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample4.xml)]

Pour plus d’informations sur la configuration de la `applicationName` attribut et le raisonnement derrière lui, reportez-vous à [ *Scott Guthrie* ](https://weblogs.asp.net/scottgu/) billet de blog de s, [ *toujours défini applicationName propriété lors de la configuration de l’appartenance d’ASP.NET et d’autres fournisseurs*](https://weblogs.asp.net/scottgu/443634).

### <a name="managing-user-accounts-in-the-production-environment"></a>La gestion des comptes d’utilisateur dans l’environnement de Production

L’outil d’Administration de Microsoft ASP.NET Web Site (WSAT) rend plus facile créer et gérer des comptes d’utilisateurs, définir et appliquer des rôles et épeler utilisateur - et en fonction du rôle des règles d’autorisation. Vous pouvez lancer le WSAT à partir de Visual Studio en accédant à l’Explorateur de solutions et en cliquant sur l’icône de Configuration ASP.NET ou en sélectionnant les menus de site Web ou un projet et en sélectionnant l’élément de menu de Configuration ASP.NET. Malheureusement, le WSAT ne peut fonctionner avec les sites Web locales. Par conséquent, vous ne pouvez pas utiliser le WSAT à partir de votre station de travail pour gérer le site Web dans l’environnement de production.

La bonne nouvelle est que toutes les fonctionnalités exposées fournies par le WSAT sont disponibles par programmation via l’appartenance et l’API de rôles ; en outre, la plupart des écrans WSAT utilisent les contrôles standards ASP.NET-associées à la connexion. En bref, vous pouvez ajouter des pages ASP.NET à votre site Web qui offrent les fonctionnalités de gestion nécessaires.

Rappelez-vous qu’un tutoriel précédent mis à jour l’application web de critiques de livres d’inclure un `~/Admin` dossier et que ce dossier a été configuré pour autoriser uniquement les utilisateurs dans le rôle d’administrateur. J’ai ajouté une page dans ce dossier nommé `CreateAccount.aspx` à partir de laquelle un administrateur peut créer un nouveau compte d’utilisateur. Cette page utilise le contrôle CreateUserWizard pour afficher la logique utilisateur de l’interface et le serveur principal pour la création d’un nouveau compte d’utilisateur. Nouveautés plus, j’ai personnalisé le contrôle afin d’inclure une case à cocher qui vous demande si le nouvel utilisateur doit également être ajouté au rôle d’administrateur (voir Figure 5). Avec un peu de travail, vous pouvez créer un jeu personnalisé de pages qui implémente les tâches liées à la gestion de rôles et des utilisateurs qui seraient sinon fournies par le WSAT.

> [!NOTE]
> Pour plus d’informations sur l’utilisation de l’appartenance et l’API de rôles, ainsi que les contrôles Web de ASP.NET associées à la connexion, veillez à lire mon [ *didacticiels de sécurité de site Web*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). Pour plus d’informations sur la personnalisation du contrôle CreateUserWizard, consultez le [ *création de comptes utilisateur* ](../../older-versions-security/membership/creating-user-accounts-vb.md) et [ *stockant des informations utilisateur supplémentaires* ](../../older-versions-security/membership/storing-additional-user-information-vb.md) didacticiels ou extraction [ *Erich Peterson* ](http://www.erichpeterson.com/) article s, [ *personnalisation du contrôle CreateUserWizard* ](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx).


[![Les administrateurs peuvent créer de nouveaux comptes d’utilisateur](configuring-a-website-that-uses-application-services-vb/_static/image14.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image13.jpg)

**Figure 5**: les administrateurs peuvent créer des comptes d’utilisateur ([cliquez pour afficher l’image en taille réelle](configuring-a-website-that-uses-application-services-vb/_static/image15.jpg))


Si vous avez besoin de toutes les fonctionnalités de l’extraction WSAT [ *propagée votre propre Site Web Administration outil*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx), dans lequel l’auteur Dan Clem vous explique le processus de création d’un outil WSAT de type personnalisé. Dan partage son code source d’application s (en c#) et fournit des instructions détaillées pour l’ajouter à votre site Web hébergé.

## <a name="summary"></a>Récapitulatif

Lorsque vous déployez une application web qui utilise l’implémentation de base de données de services application vous devez vous assurer que la base de données de production dispose les objets de base de données requis. Ces objets peuvent être ajoutés à l’aide de techniques abordées dans le *déploiement d’une base de données* didacticiel ; vous pouvez également utiliser le `aspnet_regsql.exe` outil, comme nous l’avons vu dans ce didacticiel. Autres défis dont nous avons parlé sur se concentrent autour de la synchronisation du nom d’application utilisé dans les environnements de développement et de production (ce qui est important si vous souhaitez que les utilisateurs et rôles créés dans l’environnement de développement soit valide sur la production) et les techniques de la gestion des utilisateurs et des rôles dans l’environnement de production.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [*ASP.NET SQL Server Registration Tool (aspnet_regsql.exe)*](https://msdn.microsoft.com/library/ms229862.aspx)
- [*Création de la base de données de Services d’Application pour SQL Server*](https://msdn.microsoft.com/library/x28wfk74.aspx)
- [*Création du schéma d’appartenance dans SQL Server*](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb.md)
- [*Examen de l’appartenance ASP.NET s, les rôles et profil*](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [*Déploiement de votre propre outil d’Administration de Site Web*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [*Didacticiels de sécurité de site Web*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)
- [*Présentation de l’outil Administration Site Web*](https://msdn.microsoft.com/library/yy40ytx0.aspx)

> [!div class="step-by-step"]
> [Précédent](configuring-the-production-web-application-to-use-the-production-database-vb.md)
> [Suivant](strategies-for-database-development-and-deployment-vb.md)
