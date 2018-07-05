---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
title: 'Déploiement d’une Application de Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio : Introduction - 1 sur 12 | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer une ASP.NET (publier) projet d’application web qui inclut une base de données SQL Server Compact à l’aide de Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a2d7f33b-8c4a-4b48-9fb1-9139cf9b9878
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
msc.type: authoredcontent
ms.openlocfilehash: 4a790fa72568caafdb2fab5efd9f334919c23719
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398269"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-introduction---1-of-12"></a>Déploiement d’une Application de Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio : Introduction - 1 sur 12
====================
par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet de démarrage](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Cette série de didacticiels vous montre comment déployer une ASP.NET (publier) projet d’application web qui inclut une base de données SQL Server Compact à l’aide de Visual Studio 2012 RC ou Visual Studio Express 2012 RC pour le Web. Vous pouvez également utiliser Visual Studio 2010 si vous installez la mise à jour de publication Web.
> 
> Pour obtenir un didacticiel qui montre les fonctionnalités de déploiement introduites après la version RC de Visual Studio 2012, montre comment déployer des éditions de SQL Server autre que SQL Server Compact et montre comment déployer dans Azure App Service Web Apps, consultez [déploiement Web ASP.NET à l’aide de Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).
> 
> Ces didacticiels vous guident tout au long de déployer d’abord vers IIS sur votre ordinateur de développement local pour le test, puis vers un fournisseur d’hébergement tiers. L’application que vous allez déployer utilise une base de données d’application et une base de données d’appartenance ASP.NET. Vous commencez à l’aide de SQL Server Compact et le déploiement sur SQL Server Compact, et les didacticiels suivants vous montrent comment déployer des modifications de base de données et comment migrer vers SQL Server.
> 
> Les didacticiels supposent que vous savez comment travailler avec ASP.NET dans Visual Studio. Si vous n’est pas le cas, un bon point de départ est un [base ASP.NET Web Forms didacticiel](../tailspin-spyworks/tailspin-spyworks-part-1.md) ou un [base didacticiel ASP.NET MVC](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md).
> 
> Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les publier à le [forum de déploiement ASP.NET](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment).


## <a name="overview"></a>Vue d'ensemble

Ces didacticiels vous guident tout au long de déployer d’abord vers IIS sur votre ordinateur de développement local pour le test, puis vers un fournisseur d’hébergement tiers. L’application que vous allez déployer utilise une base de données d’application et une base de données d’appartenance ASP.NET. Vous commencez à l’aide de SQL Server Compact et le déploiement sur SQL Server Compact, et les didacticiels suivants vous montrent comment déployer des modifications de base de données et comment migrer vers SQL Server.

Le nombre de didacticiels – 11 dans toutes les ainsi qu’une page de dépannage – rendent le processus de déploiement sembler décourageant. En fait, les procédures de base pour déployer un site constituent une relativement petite partie de l’ensemble de didacticiels. Toutefois, dans des situations réelles, vous devez souvent d’informations sur certains aspects supplémentaires légère mais importante de déploiement, par exemple, la définition des autorisations de dossier sur le serveur cible. Nous avons inclus la plupart de ces techniques supplémentaires dans les didacticiels, avec l’espoir que les didacticiels ne laissez pas les informations qui peuvent vous empêcher de déployer avec succès une application réelle.

Les didacticiels sont conçus pour s’exécuter dans la séquence, et chaque partie s’appuie sur la partie précédente. Toutefois, vous pouvez ignorer les parties qui ne sont pas appropriées à votre situation. (Ignorer les parties peut-être vous amener à ajuster les procédures dans les didacticiels suivants.)

## <a name="intended-audience"></a>Public visé

Les didacticiels sont destinées aux développeurs ASP.NET qui travaillent dans les petites entreprises ou d’autres environnements où :

- Un processus d’intégration continue (les générations automatisées et déploiement) n’est pas utilisé.
- L’environnement de production est un fournisseur d’hébergement tiers.
- En règle générale, une personne remplit plusieurs rôles (la même personne développe, teste et déploie).

Dans les environnements d’entreprise, il est plus courant d’implémenter des processus d’intégration continue et l’environnement de production est généralement hébergé par les serveurs de la société. Différentes personnes effectuent également généralement les différents rôles. Pour plus d’informations sur le déploiement de l’entreprise, consultez [déploiement d’Applications Web dans les scénarios d’entreprise](../../deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md).

Les organisations de toutes tailles peuvent également déployer des applications web vers Azure, et la plupart des procédures indiquées dans ces didacticiels s’appliquent également à Azure App Services Web Apps. Pour une introduction à Azure, consultez [ https://azure.microsoft.com ](https://azure.microsoft.com).

## <a name="the-hosting-provider-shown-in-the-tutorials"></a>Le fournisseur d’hébergement indiqué dans les didacticiels

Les didacticiels vous guident tout au long du processus de configuration d’un compte avec une société d’hébergement et de déploiement de l’application à ce fournisseur d’hébergement. Une société d’hébergement spécifique a été choisie afin que les didacticiels pourraient illustrer l’expérience complète de déploiement sur un site Web actif. Chaque société d’hébergement fournit différentes fonctionnalités et l’expérience de déploiement sur leurs serveurs varie quelque peu ; Toutefois, la procédure décrite dans ce didacticiel est généralement utilisée pour l’ensemble du processus.

Le fournisseur d’hébergement pour ce didacticiel, Cytanium.com, est un des nombreux qui sont disponibles, et son utilisation dans ce didacticiel ne constitue pas une approbation ou la recommandation.

## <a name="deploying-web-site-projects"></a>Déploiement de projets de Site Web

Contoso University est un projet d’application web Visual Studio. La plupart des méthodes de déploiement et des outils décrites dans ce didacticiel ne s’appliquent pas aux [projets de Site Web](https://msdn.microsoft.com/library/dd547590.aspx). Pour plus d’informations sur la façon de déployer des projets de site web, consultez [ASP.NET Deployment Content Map](https://msdn.microsoft.com/library/bb386521.aspx#deployment_for_web_site_projects).

## <a name="deploying-aspnet-mvc-projects"></a>Déploiement de projets ASP.NET MVC

Pour ce didacticiel, vous déployez un projet Web Forms ASP.NET, mais tout ce dont vous allez apprendre à faire est également applicable à ASP.NET MVC. Un projet MVC Visual Studio est simplement une autre forme de projet d’application web. La seule différence est que si vous effectuez un déploiement sur un fournisseur d’hébergement ne prenant pas en charge ASP.NET MVC ou votre version cible de celui-ci, vous devez vous assurer que vous avez installé approprié ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0) ou [MVC 4](http://nuget.org/packages/aspnetmvc)) Package NuGet dans votre projet.

## <a name="programming-language"></a>Langage de programmation

L’exemple d’application utilise c#, mais les didacticiels ne nécessitent pas de connaissances du langage c# et les techniques de déploiement illustrés par les didacticiels ne sont pas spécifiques au langage.

## <a name="troubleshooting-during-this-tutorial"></a>Résolution des problèmes au cours de ce didacticiel

Quand une erreur se produit pendant le déploiement, ou si le site déployé ne s’exécute pas correctement, les messages d’erreur ne fournissent pas toujours une solution. Pour vous aider dans certains problèmes courants, un [référence page Résolution des problèmes](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md) est disponible. Si vous obtenez un message d’erreur ou quelque chose ne fonctionne pas lorsque vous parcourez les didacticiels, veillez à consulter la page Résolution des problèmes.

## <a name="comments-welcome"></a>Bienvenue dans les commentaires

Commentaires sur les didacticiels sont les bienvenus, et lorsque le didacticiel est mis à jour chaque effort sera fait tenir des corrections de compte ou des suggestions relatives aux améliorations fournies dans les commentaires de didacticiel.

## <a name="prerequisites"></a>Prérequis

Avant de commencer, assurez-vous que vous avez Windows 7 ou version ultérieure et que l’un des produits suivants est installé sur votre ordinateur :

- [Visual Studio 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
- [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VWD2010SP1Pack)
- [Visual Studio 2012 RC ou Visual Studio Express 2012 RC pour le Web](https://go.microsoft.com/fwlink/?LinkId=240162)

Si vous avez Visual Studio 2010 SP1 ou Visual Web Developer Express 2010 SP1, installez également les produits suivants :

- [Azure SDK pour .NET (VS 2010 SP1)](https://go.microsoft.com/fwlink/?LinkID=208120) (inclut la mise à jour Web de publication)
- [Microsoft Visual Studio 2010 SP1 Tools pour SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCEVSTools)

Un autre logiciel est requis afin de terminer le didacticiel, mais vous n’êtes pas obligé d’en avez pas encore chargé. Ce didacticiel vous guidera à travers les étapes permettant de l’installer lorsque vous en avez besoin.

## <a name="downloading-the-sample-application"></a>Télécharger l’exemple d’Application

L’application que vous allez déployer est nommée Contoso University et a déjà été créée pour vous. Il est une version simplifiée d’un site web de l’université, faiblement selon de l’application Contoso University décrite dans le [didacticiels Entity Framework sur le site ASP.NET](https://asp.net/entity-framework/tutorials).

Lorsque vous avez les composants requis installés, téléchargez le [application web Contoso University](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b). Le *.zip* fichier contient plusieurs versions du projet et un fichier PDF qui contient tous les didacticiels de 12. Pour fonctionner à travers les étapes du didacticiel, commencez par ContosoUniversity-début. Pour voir à quoi ressemble le projet à la fin des didacticiels, ouvrez ContosoUniversity-End. Pour voir à quoi ressemble le projet avant la migration vers SQL Server dans le didacticiel 10, ouvrez ContosoUniversity-AfterTutorial09.

Pour préparer travailler avec les étapes du didacticiel, début de l’enregistrement ContosoUniversity-pour le dossier que vous utilisez pour travailler avec des projets Visual Studio. Par défaut, c’est le dossier suivant :

`C:\Users\<username>\Documents\Visual Studio 2012\Projects`

(Pour les captures d’écran dans ce didacticiel, le dossier du projet se trouve dans le répertoire racine sur le `C`: lecteur.)

Démarrez Visual Studio, ouvrez le projet, appuyez sur CTRL-F5 pour l’exécuter.

[![Home_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image2.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image1.png)

Les pages de site Web sont accessibles à partir de la barre de menus et vous permettent d’effectuer les fonctions suivantes :

- Afficher les statistiques des étudiants (la page About).
- Afficher, modifier, supprimer et ajouter des étudiants.
- Afficher et modifier des cours.
- Afficher et modifier des formateurs.
- Afficher et modifier des départements.

Voici les captures d’écran de quelques pages représentatifs.

[![Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image4.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image3.png)

[![Add_Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image6.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image5.png)

## <a name="reviewing-application-features-that-affect-deployment"></a>Examen des fonctionnalités qui affectent le déploiement d’applications

Les fonctionnalités suivantes de l’application affectent le déploiement ou ce que vous devez faire pour le déployer. Chacune d’elles est expliquée plus en détail dans les didacticiels suivants dans la série.

- Contoso University utilise une base de données SQL Server Compact pour stocker les données d’application tels que les noms de student et instructor. La base de données contient un mélange de données de test et les données de production, et lorsque vous déployez en production vous devez exclure les données de test. Plus loin dans cette série de didacticiels, vous allez migrer à partir de SQL Server Compact vers SQL Server.
- L’application utilise le système d’appartenance ASP.NET, qui stocke les informations de compte utilisateur dans une base de données SQL Server Compact. L’application définit un utilisateur administrateur qui a accès à des informations restreintes. Vous devez déployer la base de données d’appartenance sans comptes de test, mais avec un compte d’administrateur.
- Étant donné que la base de données d’application et la base de données d’appartenance utilisent SQL Server Compact comme moteur de base de données, vous devez déployer le moteur de base de données pour le fournisseur d’hébergement, ainsi que les bases de données eux-mêmes.
- L’application utilise des fournisseurs d’appartenances universelle ASP.NET afin que le système d’appartenance peut stocker ses données dans une base de données SQL Server Compact. L’assembly qui contient les fournisseurs d’appartenances universel doit être déployé avec l’application.
- L’application utilise Entity Framework 5.0 pour accéder aux données dans la base de données d’application. L’assembly qui contient l’Entity Framework 5.0 doit être déployé avec l’application.
- L’application utilise un tiers journalisation des erreurs et utilitaire de création de rapports. Cet utilitaire est fourni dans un assembly qui doit être déployé avec l’application.
- L’utilitaire de journalisation erreur écrit les informations d’erreur dans des fichiers XML dans un dossier. Vous devez vous assurer que le compte ASP.NET sous lequel s’exécute dans le site déployé a l’autorisation d’écriture à ce dossier, et vous devez exclure ce dossier de déploiement. (Sinon, données de journal d’erreur à partir de l’environnement de test peuvent être déployées en production et/ou de fichiers de journal des erreurs de production peuvent être supprimés.)
- L’application inclut certains paramètres qui doivent être modifiés dans déployé *Web.config* fichier en fonction de l’environnement de destination (test ou production) et d’autres paramètres qui doivent être modifiées en fonction de la build configuration (Debug ou Release).
- La solution Visual Studio inclut un projet de bibliothèque de classes. Uniquement l’assembly qui génère ce projet doit être déployé, pas le projet lui-même.

Dans ce premier didacticiel de la série, vous avez téléchargé l’exemple de projet Visual Studio et passé en revue les fonctionnalités de site qui affectent la façon dont vous déployez l’application. Dans les didacticiels suivants, vous préparer pour le déploiement en définissant une partie de ces éléments à être gérées automatiquement. D’autres vous prendre soin de manuellement.

> [!div class="step-by-step"]
> [Next](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
