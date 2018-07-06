---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
title: Déployer des bases de données d’appartenance pour les environnements d’entreprise | Microsoft Docs
author: jrjlee
description: Cette rubrique explique les considérations essentielles et les défis que vous devrez relever lorsque vous configurez les bases de données ASP.NET application services (plus fréquent...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 3cf765df-d311-4f68-a295-c9685ceea830
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
msc.type: authoredcontent
ms.openlocfilehash: 9df152866b54f55c2b00611331e868f98bd2f3e4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827186"
---
<a name="deploying-membership-databases-to-enterprise-environments"></a>Déployer des bases de données d’appartenance pour les environnements d’entreprise
====================
par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique explique les considérations essentielles et les défis que vous devrez résoudre lors de la bases de données (plus communément appelé bases de données d’appartenance) dans les environnements de test, intermédiaire ou de production de services de configuration application ASP.NET. Elle décrit également les approches que vous pouvez utiliser pour relever ces défis.


Cette rubrique fait partie d’une série de didacticiels basées sur les exigences de déploiement d’entreprise de la société fictive Fabrikam, Inc. Cette série de didacticiels utilise un exemple de solution&#x2014;le [solution Gestionnaire de contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;pour représenter une application web avec un niveau réaliste de complexité, y compris une application ASP.NET MVC 3, une Communication de Windows Foundation (WCF) service et un projet de base de données.

La méthode de déploiement au cœur de ces didacticiels est basée sur l’approche de fichier de projet de fractionnement décrite dans [présentation du fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md), dans lequel le processus de génération est contrôlé par deux fichiers de projet&#x2014;contenant un seul les instructions qui s’appliquent à chaque environnement de destination et celui qui contient les paramètres de génération et de déploiement spécifiques à l’environnement de génération. Au moment de la génération, le fichier de projet spécifique à l’environnement est fusionné dans le fichier de projet d’indépendant de l’environnement pour former un ensemble complet d’instructions de génération.

## <a name="what-are-the-issues-when-you-deploy-a-membership-database"></a>Quels sont les problèmes lorsque vous déployez une base de données d’appartenance ?

Dans la plupart des cas, lorsque vous concevez une stratégie de déploiement pour une base de données, la première chose que vous devez prendre en compte est quelles données vous souhaitez déployer. Dans un environnement de développement ou de test, vous souhaiterez déployer des données de compte d’utilisateur pour faciliter les tests rapidement et simplement. Dans un environnement intermédiaire ou de production, il est peu probable que vous pouvez déployer des données de compte d’utilisateur.

Malheureusement, les bases de données d’appartenance ASP.NET introduisent des défis spécifiques que prendre cette décision beaucoup plus complexe :

- Un déploiement de schéma uniquement laisse la base de données d’appartenance dans un état non opérationnel. Il s’agit, car la base de données d’appartenance inclut certaines données de configuration (dans le **aspnet\_SchemaVersions** table) dont la base de données a besoin pour fonctionner. Par conséquent, si vous effectuez un déploiement de schéma uniquement de votre base de données d’appartenance pour exclure les données de compte d’utilisateur, vous devez exécuter un script de post-déploiement pour ajouter les données de configuration essentiels.
- Selon la configuration de votre base de données d’appartenance, le fournisseur d’appartenances peut utiliser la clé d’ordinateur pour chiffrer les mots de passe et les stocker dans la base de données. Dans ce cas, des données de compte utilisateur que vous déployez avec la base de données seront inutilisable sur le serveur de destination. Pour cette raison, il n’est pas un scénario pris en charge de déploiement des données de compte d’utilisateur.

## <a name="choosing-a-membership-database-strategy"></a>Choix d’une stratégie de base de données d’appartenance

Utilisez ces instructions lorsque vous choisissez la configuration d’une base de données d’appartenance dans un environnement de serveur d’entreprise :

- Dans la mesure du possible, ne déployez pas les bases de données d’appartenance. Au lieu de cela, créez la base de données d’appartenance manuellement sur le serveur de base de données cible. Si vous n’avez pas personnalisé votre schéma de base de données d’abonnement, vous pouvez simplement créer un nouveau in situ à la destination à l’aide de la [ASP.NET SQL Server Registration Tool (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).
- Si aucune option mais sur déployer une base de données d’appartenance&#x2014;par exemple, si vous avez apporté d’importantes modifications apportées au schéma de base de données&#x2014;vous devez effectuer un déploiement de schéma uniquement de la base de données d’appartenance, pour exclure les données de compte d’utilisateur, puis exécuter un script de post-déploiement pour ajouter des données de configuration requis. Vous trouverez des conseils de large sur ces approches dans [Comment : déployer l’ASP.NET l’appartenance de base de données sans y compris des comptes utilisateur](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

Il est important de ne pas oublier que *le schéma de votre base de données d’appartenance est susceptible d’être relativement statique*. Même si vous avez personnalisé la base de données d’appartenance, il est peu probable que vous devrez mettre à jour le schéma de manière régulière&#x2014;il ne va pas de changement d’avec la même fréquence que le code dans une application web ou un projet de base de données. Par conséquent, vous devez ne doit pas inclure la base de données d’appartenance dans n’importe quel processus de déploiement automatisé ou seule étape.

## <a name="using-vsdbcmd-to-update-a-membership-database-schema"></a>À l’aide de VSDBCMD pour mettre à jour un schéma de base de données d’appartenance

Si vous modifiez la structure de votre base de données d’appartenance après votre premier déploiement, il pouvez que vous ne souhaitez pas utiliser l’outil de déploiement de Web Internet Information Services (IIS) (Web Deploy) pour redéployer la base de données. La fonctionnalité de déploiement de base de données dans Web Deploy n’inclut pas la possibilité d’apporter des mises à jour différentielles à une base de données de destination&#x2014;au lieu de cela, Web Deploy doit supprimer et recréer la base de données. Cela signifie que vous perdez les données de compte utilisateur existantes, ce qui ne sont généralement pas souhaitable dans les environnements de production ou intermédiaire.

L’alternative consiste à utiliser l’utilitaire VSDBCMD pour mettre à jour le schéma de votre base de données de destination. VSDBCMD inclut deux fonctionnalités importantes. Tout d’abord, il peut importer le schéma de base de données existante dans un fichier .dbschema. En second lieu, il peut déployer un fichier .dbschema à une base de données existante en tant que mise à jour différentiel, ce qui signifie qu’il effectue uniquement les modifications requises pour mettre la base de données cible à jour et vous ne perdez aucune donnée.

Vous pouvez utiliser ces étapes pour mettre à jour un schéma de base de données d’appartenance :

1. Utilisez l’outil VSDBCMD **importation** action permettant de générer un fichier .dbschema pour votre base de données source d’appartenance. Cette procédure est décrite dans [Comment : importer un schéma à partir d’une invite de commandes](https://msdn.microsoft.com/library/dd172135.aspx).
2. Utilisez l’outil VSDBCMD **déployer** actions pour déployer le fichier .dbschema sur votre base de données de l’appartenance de destination. Cette procédure est décrite dans [référence de ligne de commande pour VSDBCMD. EXE (déploiement et importation de schéma)](https://msdn.microsoft.com/library/dd193283.aspx).

## <a name="conclusion"></a>Conclusion

Cette rubrique décrit certains des défis que vous risquez de rencontrer lorsque vous avez besoin approvisionner les bases de données d’appartenance ASP.NET dans différents environnements cibles. En particulier, il expliqué pourquoi les déploiements de schéma uniquement laisse la base de données d’appartenance dans un état non opérationnel et pourquoi le déploiement des données de compte utilisateur n'est pas pris en charge. La rubrique a également présenté des conseils sur la façon de configurer, déployer et mettre à jour des bases de données d’appartenance dans différents scénarios.

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations et des exemples montrant comment utiliser VSDBCMD, consultez [référence de ligne de commande pour VSDBCMD. EXE (déploiement et importation de schéma)](https://msdn.microsoft.com/library/dd193283.aspx) et [Comment : importer un schéma à partir d’une invite de commandes](https://msdn.microsoft.com/library/dd172135.aspx). Pour plus d’informations sur l’utilisation d’aspnet\_regsql.exe pour créer des bases de données d’appartenance, consultez [ASP.NET SQL Server Registration Tool (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx). Pour obtenir des instructions plus générales sur le déploiement de bases de données d’appartenance, consultez [Comment : déployer l’ASP.NET l’appartenance de base de données sans y compris des comptes utilisateur](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

> [!div class="step-by-step"]
> [Précédent](deploying-database-role-memberships-to-test-environments.md)
> [Suivant](excluding-files-and-folders-from-deployment.md)
