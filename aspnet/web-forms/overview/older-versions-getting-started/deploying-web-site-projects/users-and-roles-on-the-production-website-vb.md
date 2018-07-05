---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-vb
title: Utilisateurs et rôles sur le site Web de Production (VB) | Microsoft Docs
author: rick-anderson
description: L’outil d’Administration de Microsoft ASP.NET du site Web (WSAT) fournit une interface utilisateur web pour la configuration des paramètres d’appartenance et de rôles et de création, modification, un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 491ed5ae-9be1-4191-87be-65e4e1c57690
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-vb
msc.type: authoredcontent
ms.openlocfilehash: 5b2a5ea84c5c16ad5bf4e3041d31ad29660d965f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380374"
---
<a name="users-and-roles-on-the-production-website-vb"></a>Utilisateurs et rôles sur le site Web de Production (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial16_CustomAWAT_vb.pdf)

> L’outil d’Administration de Microsoft ASP.NET du site Web (WSAT) fournit une interface utilisateur web pour la configuration des paramètres d’appartenance et de rôles et de création, la modification et la suppression des utilisateurs et rôles. Malheureusement, le WSAT fonctionne uniquement quand consultées à partir de localhost, ce qui signifie que vous ne pouvez pas atteindre l’outil d’Administration du site Web de la production via votre navigateur. La bonne nouvelle est qu’il existe des solutions de contournement qui permettent de gérer les utilisateurs et rôles de production. Ce didacticiel aborde ces solutions de contournement et d’autres.


## <a name="introduction"></a>Introduction

ASP.NET 2.0 a introduit un nombre de *les services d’application*, qui sont une suite de services de bloc de construction que vous pouvez ajouter à votre application web. Nous avons ajouté les services d’appartenance et des rôles pour le site Web de critiques de livres de nouveau la [ *configuration d’un site Web qu’utilise les Services d’Application* didacticiel](configuring-a-website-that-uses-application-services-vb.md). Le service d’appartenance facilite la création et la gestion des comptes d’utilisateur ; le service de rôles offre une API pour classer les utilisateurs en groupes. Le site de critiques de livres possède trois comptes d’utilisateurs - Scott, Jisun et Alice - et un seul rôle, l’administrateur, avec Scott et Jisun dans le rôle d’administrateur.

ASP. Les services d’application de NET ne sont pas liés à une implémentation spécifique. Au lieu de cela, vous demandez les services d’application à utiliser un particulier *fournisseur*, et ce fournisseur implémente le service à l’aide d’une technologie particulière. Nous avons configuré l’application web de critiques de livres pour utiliser le `SqlMembershipProvider` et `SqlRoleProvider` fournisseurs pour les services d’appartenance et des rôles. Ces deux fournisseurs de stockant les informations de compte et le rôle utilisateur dans une base de données SQL Server et sont les fournisseurs les plus couramment utilisés pour les applications web basées sur Internet hébergées sur une société d’hébergement web.

Constitue un défi courant pour les développeurs qui utilisent les services d’appartenance et des rôles est la gestion des utilisateurs et des rôles sur l’environnement de production. Comment supprimer un compte d’utilisateur à partir du site Web de production, ajoutez un nouveau rôle ou ajouter un utilisateur existant à un rôle existant ? Ce didacticiel explore des techniques différentes pour la gestion des utilisateurs et rôles sur le site Web de production.

## <a name="using-the-aspnet-web-site-administration-tool"></a>À l’aide de l’outil d’Administration de Site Web ASP.NET

ASP.NET inclut un [outil d’Administration de Site Web](https://msdn.microsoft.com/library/yy40ytx0.aspx) (WSAT) qui facilite pour créer et gérer des comptes d’utilisateur et des rôles et pour spécifier des règles d’autorisation utilisateur - et en fonction du rôle. Pour utiliser le WSAT, cliquez sur l’icône de Configuration ASP.NET dans l’Explorateur de solutions, ou accédez au menu projet ou site Web et choisissez l’option de Configuration ASP.NET. Chacune de ces approches lance un navigateur web et il pointe vers le WSAT à une adresse telle que : `http://localhost:portNumber/asp.netwebadminfiles/default.aspx?applicationPhysicalPath=pathToApplication`

Le WSAT est divisé en trois sections :

- **Sécurité** -gérer les utilisateurs, des rôles et des règles d’autorisation.
- **ApplicationConfiguration** -gérer les &lt;appSettings&gt; et les paramètres SMTP à partir d’ici. Vous pouvez également déconnecter l’application et gérer le débogage et traçage de paramètres à partir d’ici, comme ainsi que la page d’erreur personnalisé par défaut.
- **ProviderConfiguration** -configurer les fournisseurs utilisés par les services d’application.

La section de sécurité (illustrée **Figure 1**) inclut des liens pour créer de nouveaux utilisateurs, la gestion des utilisateurs, la création et la gestion des rôles et de création et la gestion des règles d’accès. À partir de là vous pouvez ajouter un nouveau rôle au système, supprimer un utilisateur existant, ou ajouter ou supprimer des rôles à partir d’un compte d’utilisateur particulier.

[![](users-and-roles-on-the-production-website-vb/_static/image2.png)](users-and-roles-on-the-production-website-vb/_static/image1.png)

**Figure 1**: la Section de sécurité WSAT inclut des Options pour la gestion des utilisateurs et des rôles  
([Cliquez pour afficher l’image en taille réelle](users-and-roles-on-the-production-website-vb/_static/image3.png))

Malheureusement, le WSAT est uniquement accessible localement. Vous ne pouvez pas visiter le WSAT sur votre site Web de production distant ; Si vous visitez `www.yoursite.com/asp.netwebadminfiles/default.aspx` vous obtenez une réponse 404 (introuvable). Le code qui gère les utilisations WSAT le `Membership` et `Roles` classes dans le .NET Framework pour créer, modifier et supprimer des utilisateurs et des rôles. Ces classes, consultez les informations de configuration de l’application web pour déterminer quel fournisseur à utiliser ; dans le [ *configuration d’un site Web qu’utilise les Services d’Application* didacticiel](configuring-a-website-that-uses-application-services-vb.md) nous préparons le site Web de critiques de livres à utiliser le `SqlMembershipProvider` et `SqlRoleProvider` fournisseurs. Cela entraînait ajout `<membership>` et `<roleManager>` sections à `Web.config`.

[!code-xml[Main](users-and-roles-on-the-production-website-vb/samples/sample1.xml)]

Notez que le `<membership>` et `<roleManager>` sections référence le `SqlMembershipProvider` et `SqlRoleProvider` fournisseurs dans leurs `type` d’attribut, respectivement. Ces fournisseurs de stockent les informations de rôle dans une base de données SQL Server spécifiée. La base de données utilisé par ces fournisseurs est spécifié par le `connectionStringName` attribut, `ReviewsConnectionString`, qui est défini dans le `~/ConfigSections/databaseConnectionStrings.config` fichier. N’oubliez pas que le `databaseConnectionStrings.config` fichier dans l’environnement de développement contient la chaîne de connexion à la base de données de développement, alors que le `databaseConnectionStrings.config` fichier sur la production contient la chaîne de connexion à la base de données de production.

En bref, le WSAT doit être accessible localement via l’environnement de développement, et il fonctionne avec les informations utilisateur et le rôle dans la base de données spécifié dans le `databaseConnectionStrings.config` fichier. Par conséquent, si nous modifions les informations de chaîne de connexion dans le `databaseConnectionStrings.config` fichier sur l’environnement de développement nous pouvons utiliser le WSAT localement pour gérer les utilisateurs et rôles dans l’environnement de production.

Pour illustrer cette fonctionnalité, ouvrez le `databaseConnectionStrings.config` fichier dans Visual Studio sur l’environnement de développement et remplacez la chaîne de connexion de base de données de développement avec la chaîne de connexion de base de données de production. Puis lancez le WSAT, accédez l’onglet sécurité et ajouter un nouvel utilisateur nommé Sam avec mot de passe « password » ! (inférieur entre les guillemets). **Figure 2** montre l’écran WSAT lors de la création de ce compte.

[![](users-and-roles-on-the-production-website-vb/_static/image5.png)](users-and-roles-on-the-production-website-vb/_static/image4.png)

**Figure 2**: créer un utilisateur nommé Sam dans l’environnement de Production  
([Cliquez pour afficher l’image en taille réelle](users-and-roles-on-the-production-website-vb/_static/image6.png))

Étant donné que nous avons modifié la chaîne de connexion dans `databaseConnectionStrings.config` pour pointer vers le serveur de base de données de production, Sam a été ajoutée en tant qu’utilisateur dans l’environnement de production. Pour vérifier cela, modifiez la chaîne de connexion dans le `databaseConnectionStrings.config` de fichiers à la base de données de développement, puis visitez le `Login.aspx` page dans l’environnement de développement. Essayez de vous connecter en tant que Sam (consultez **Figure 3**).

[![](users-and-roles-on-the-production-website-vb/_static/image8.png)](users-and-roles-on-the-production-website-vb/_static/image7.png)

**Figure 3**: Impossible de vous connecter en tant que Sam dans l’environnement de développement  
([Cliquez pour afficher l’image en taille réelle](users-and-roles-on-the-production-website-vb/_static/image9.png))

Vous ne pouvez pas connectez-vous en tant que Sam dans l’environnement de développement, car les informations de compte d’utilisateur n’existent pas dans la base de données locale. Au lieu de cela, est a été ajouté à la base de données de production. Pour le vérifier, afficher le contenu de la `aspnet_Users` table dans les bases de données à la fois le développement et de production. Dans l’environnement de développement doit être uniquement trois enregistrements pour les utilisateurs Scott, Jisun et Alice. Toutefois, le `aspnet_Users` table dans la base de données de production a quatre enregistrements : Scott, Jisun, Alice et Sam. Par conséquent, Sam peut se connecter via le site Web en production, mais pas par le biais de l’environnement de développement.

[![](users-and-roles-on-the-production-website-vb/_static/image11.png)](users-and-roles-on-the-production-website-vb/_static/image10.png)

**Figure 4**: Sam peut se connecter sur le site Web de Production  
([Cliquez pour afficher l’image en taille réelle](users-and-roles-on-the-production-website-vb/_static/image12.png))

> [!NOTE]
> N’oubliez pas de modifier la chaîne de connexion dans le `databaseConnectionStrings.config` fichier dans la base de données de développement de chaîne de connexion lorsque vous avez terminé fonctionne avec le WSAT sinon vous travaillerez avec des données de production lorsque vous testez le site via le développement environnement. N’oubliez pas que pendant que la technique que nous venons de parler nous permet d’utiliser le WSAT pour gérer à distance des utilisateurs et des rôles, les modifications à toutes les autres options de configuration WSAT (règles d’accès, SMTP Paramètres, débogage et de traçage paramètres et ainsi de suite) de modifier le `Web.config` fichier. Par conséquent, toutes les modifications apportées aux paramètres s’appliquent à l’environnement de développement et non à l’environnement de production.


## <a name="creating-custom-user-and-role-management-web-pages"></a>Création d’utilisateur personnalisée et des Pages Web de gestion des rôles

Le WSAT fournit un hors du système de zone pour la gestion des utilisateurs et des rôles, mais peut être lancé uniquement localement et nécessite d’apporter des modifications aux informations de chaîne de connexion afin de gérer les utilisateurs et rôles de production. La plupart des sites Web qui prennent en charge les comptes d’utilisateur incluent également un nombre d’utilisateur et le rôle administration web pages qui permettent aux administrateurs de gérer des utilisateurs et des rôles à partir des pages au sein du site. Ces pages de l’administration basée sur le web, ce qui la rendent beaucoup plus facile à gérer les utilisateurs et les rôles et sont essentielles pour les sites où il peut y avoir plusieurs administrateurs ou administrateurs qui n’ont pas accès à ou les compétences techniques à utiliser Visual Studio pour lancer le WSAT.

ASP.NET inclut un nombre de contrôles Web associées à la connexion intégrées qui rendent l’implémentation de nombreuses de ces pages web d’administration aussi simple glisser -déplacer. Par exemple, vous pouvez créer une page pour les administrateurs créer un nouveau compte d’utilisateur en faisant glisser le contrôle CreateUserWizard sur la page et en définissant certaines propriétés. En fait, la page de création d’utilisateurs dans le WSAT illustrée **Figure 2** utilise le même contrôle CreateUserWizard que vous pouvez ajouter à vos pages. En outre, les fonctionnalités de services d’appartenance et les rôles sont disponibles par programme via le `Membership` et `Roles` classes dans le .NET Framework. Avec ces classes vous pouvez écrire du code pour créer, modifier et supprimer des utilisateurs et rôles, ainsi que d’ajouter ou supprimer des utilisateurs aux rôles, afin de déterminer quels utilisateurs se trouvent dans les rôles et à effectuer d’autres tâches relatifs à l’utilisateur et de rôle.

Dans le [ *configuration d’un site Web qu’utilise les Services d’Application* didacticiel](configuring-a-website-that-uses-application-services-vb.md) j’ai ajouté une page à la `Admin` dossier nommé `CreateAccount.aspx`. Cette page permet à un administrateur pour ajouter un nouveau compte d’utilisateur pour le site et de spécifier ou non l’utilisateur nouvellement créé est dans le rôle d’administrateur (consultez **Figure 5**).

[![](users-and-roles-on-the-production-website-vb/_static/image14.png)](users-and-roles-on-the-production-website-vb/_static/image13.png)

**Figure 5**: les administrateurs peuvent créer de nouveaux comptes d’utilisateur  
([Cliquez pour afficher l’image en taille réelle](users-and-roles-on-the-production-website-vb/_static/image15.png))

Pour une étude plus détaillée de création de pages d’administration utilisateur et le rôle, ainsi que des instructions détaillées sur l’utilisation de la `Membership` et `Roles` classes et les contrôles Web de ASP.NET associées à la connexion, veillez à lire mon [sécurité des sites Web Didacticiels](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). Vous y trouverez des conseils sur la façon de créer des pages web pour la création de nouveaux comptes, la création et la gestion des rôles, affectation d’utilisateurs aux rôles et d’autres tâches d’administration courantes.

Pour implémenter des fonctionnalités comme le WSAT sur le site Web de production, vous pouvez toujours créer votre propre série de pages web qui implémentent les fonctionnalités de la WSAT. Pour aider à commencer, consultez le code source WSAT, qui se trouve dans le dossier `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\ASP.NETWebAdminFiles`. Une autre option consiste à utiliser l’alternative WSAT de Dan Clem, lequel il partage dans son article, [propagée votre propre Site Web Administration outil](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx). Dan guide les lecteurs via le processus de création d’un outil personnalisé de type WSAT inclut le code de source de son application au téléchargement (dans c#) et donne des instructions détaillées pour l’ajout de son WSAT personnalisé à un site Web hébergé.

## <a name="summary"></a>Récapitulatif

L’outil d’Administration de Microsoft ASP.NET Web Site (WSAT) peut être utilisé conjointement avec les services d’application d’appartenance et les rôles pour gérer les informations utilisateur et le rôle de votre site Web. Malheureusement, le WSAT est uniquement accessible localement et ne peut pas être consultée à partir de votre site Web de production. Toutefois, en modifiant la chaîne de connexion dans le développement, environnement pour pointer vers la base de données de production, vous pouvez utiliser le WSAT pour gérer les utilisateurs et rôles sur le site Web de production.

Tandis que l’approche WSAT permet la création rapide et facile à gérer les utilisateurs et les rôles, vous devrez lancer le WSAT à partir de Visual Studio, ainsi que des modifications temporaires pour les informations de chaîne de connexion. Le WSAT offre un moyen rapide de gérer les utilisateurs et rôles de production, mais est fastidieuse et ne fonctionne pas pour les sites Web avec plusieurs administrateurs ou administrateurs qui n’ont pas ou n’êtes pas familiarisé avec Visual Studio et le WSAT. Pour ces raisons, la plupart des sites Web qui prennent en charge les comptes d’utilisateur comprennent un ensemble de pages web d’administration. Un ensemble de pages web élimine le besoin du WSAT et utilisé par différents utilisateurs administratifs à partir de n’importe quel ordinateur.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [Examen ASP. Appartenance, rôles et profil du NET](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Déploiement de votre propre outil d’Administration de Site Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Présentation de l’outil Administration Site Web](https://msdn.microsoft.com/library/yy40ytx0.aspx)
- [Didacticiels de sécurité de site Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [Précédent](precompiling-your-website-vb.md)
