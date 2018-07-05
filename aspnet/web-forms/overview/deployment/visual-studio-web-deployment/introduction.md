---
uid: web-forms/overview/deployment/visual-studio-web-deployment/introduction
title: 'Déploiement de Web ASP.NET à l’aide de Visual Studio : Introduction | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer (publier) ASP.NET application web vers Azure App Service Web Apps ou un fournisseur d’hébergement tiers, par l’utilisation de V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 24ad086d-865e-433c-9ac9-05f1a553da16
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/introduction
msc.type: authoredcontent
ms.openlocfilehash: ad06639db9c5de78fb9926c7ab00e348bcbb20cb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382554"
---
<a name="aspnet-web-deployment-using-visual-studio-introduction"></a>Déploiement de Web ASP.NET à l’aide de Visual Studio : Introduction
====================
par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet de démarrage](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Cette série de didacticiels vous montre comment déployer (publier) ASP.NET application web vers Azure App Service Web Apps ou un fournisseur d’hébergement tiers, à l’aide de Visual Studio 2012 avec le SDK Azure pour .NET. La plupart des procédures est similaire pour Visual Studio 2013.
> 
> Vous développez une application web pour le rendre disponible pour les personnes sur Internet. Mais les didacticiels de programmation web arrête généralement juste après que qu’ils vous ai montré comment obtenir quelque chose fonctionne sur votre ordinateur de développement. Cette série de didacticiels commence où les autres omettez : vous avez créé une application web, testés, et il est prêt à l’emploi. Étapes suivantes Ces didacticiels vous montrent comment déployer tout d’abord vers IIS sur votre ordinateur de développement local pour le test, puis vers Azure ou un fournisseur d’hébergement tiers pour la préproduction et production. L’exemple d’application que vous allez déployer est un projet d’application web qui utilise Entity Framework, SQL Server et le système d’appartenance ASP.NET. L’exemple d’application utilise ASP.NET Web Forms, mais les procédures indiquées s’appliquent également à ASP.NET MVC et API Web.
> 
> Ces didacticiels supposent que vous savez comment travailler avec ASP.NET dans Visual Studio. Si vous n’est pas le cas, un bon point de départ est un [base ASP.NET Web Forms didacticiel](../../older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1.md) ou un [base didacticiel ASP.NET MVC](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).
> 
> Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les publier à le [forum ASP.NET déploiement](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) ou [StackOverflow](http://stackoverflow.com).
> 
> Ce contenu est également disponible comme un livre électronique gratuit dans [la galerie de E-Book TechNet](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#ASPNETWebDeploymentusingVisualStudio).


## <a name="overview"></a>Vue d'ensemble

Ces didacticiels vous guident dans le processus de déploiement d’une application web ASP.NET qui inclut les bases de données SQL Server. Vous allez déployer tout d’abord à IIS sur votre ordinateur de développement local pour le test, puis vers Web Apps dans Azure App Service et de la base de données SQL Azure pour la préproduction et production. Vous allez apprendre à déployer à l’aide de Visual Studio publication en un clic, et vous allez apprendre à déployer à l’aide de la ligne de commande.

Le nombre de didacticiels peut rendre le processus de déploiement sembler décourageant. En fait, les procédures de base sont simples. Toutefois, dans des situations réelles, vous devez souvent à effectuer des tâches de déploiement supplémentaires, par exemple, la définition des autorisations de dossier sur le serveur cible. Nous avons illustré certaines de ces tâches supplémentaires, dans l’espoir que les didacticiels ne laissez pas les informations qui peuvent vous empêcher de déployer avec succès une application réelle.

Les didacticiels sont conçus pour s’exécuter dans la séquence, et chaque partie s’appuie sur la partie précédente. Vous pouvez ignorer les parties qui ne sont pas appropriées à votre situation, mais ensuite vous devrez peut-être ajuster les procédures dans les didacticiels suivants.

## <a name="intended-audience"></a>Public visé

Les didacticiels sont destinées aux développeurs ASP.NET qui travaillent dans des environnements où :

- L’environnement de production est Azure App Service Web Apps ou un fournisseur d’hébergement tiers.
- Déploiement n’est pas limité à un processus d’intégration continue, mais peut être effectué directement à partir de Visual Studio.

Déploiement à partir de [contrôle de code source](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md) à l’aide un [livraison continue](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) processus n’est pas couverte dans ces didacticiels à l’exception d’un didacticiel qui montre comment déployer à partir de la ligne de commande. Pour plus d’informations sur la livraison continue, consultez les ressources suivantes :

- [Intégration continue et livraison continues (génération d’applications Cloud réalistes avec Windows Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)
- [Déployer une application web dans Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-deploy/)
- [Déploiement d’Applications Web dans les scénarios d’entreprise](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md) (un ancien ensemble de didacticiels écrit pour Visual Studio 2010, qui a toujours des informations utiles pour les environnements d’entreprise.)

## <a name="using-a-third-party-hosting-provider"></a>À l’aide d’un fournisseur d’hébergement tiers

Les didacticiels vous guident tout au long du processus de configuration d’un compte Azure et de déploiement de l’application aux applications Web dans Azure App Service pour la préproduction et production. Toutefois, vous pouvez utiliser les mêmes procédures de base pour le déploiement sur un fournisseur d’hébergement tiers de votre choix. Où les didacticiels pas établies via le processus uniques vers Azure, ils expliquent cela et signaler les différences que vous pouvez vous attendre à un fournisseur d’hébergement tiers.

## <a name="deploying-web-app-projects"></a>Déploiement de projets d’application web

L’exemple d’application que vous téléchargez et déployez pour ces didacticiels est un projet d’application web Visual Studio. Toutefois, si vous installez la dernière version [Web publier mise à jour pour Visual Studio](https://msdn.microsoft.com/tr-tr/library/jj161045.aspx), vous pouvez utiliser les mêmes méthodes de déploiement et les outils pour les projets d’application web.

## <a name="deploying-aspnet-mvc-projects"></a>Déploiement de projets ASP.NET MVC

L’exemple d’application est un projet Web Forms ASP.NET, mais tout ce dont vous allez apprendre à faire est également applicable à ASP.NET MVC. Un projet MVC Visual Studio est simplement une autre forme de projet d’application web. La seule différence est que si vous effectuez un déploiement sur un fournisseur d’hébergement ne prenant pas en charge ASP.NET MVC ou votre version cible de celui-ci, vous devez vous assurer que vous avez installé approprié ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0), [MVC 4](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/4.0.30506.0) ou [MVC 5](http://www.nuget.org/packages/Microsoft.AspNet.Mvc)) package NuGet dans votre projet.

## <a name="programming-language"></a>Langage de programmation

L’exemple d’application utilise c#, mais les didacticiels ne nécessitent pas de connaissances du langage c# et les techniques de déploiement illustrés par les didacticiels ne sont pas spécifiques au langage.

## <a name="database-deployment-methods"></a>Méthodes de déploiement de base de données

Il existe trois façons que vous pouvez déployer une base de données SQL Server, ainsi que le déploiement web dans Visual Studio :

- Entity Framework Code First Migrations
- Le fournisseur de Web Deploy dbDacFx
- Le fournisseur de Web Deploy dbFullSql

Dans ce didacticiel, vous allez utiliser les deux premiers de ces méthodes. Le fournisseur de Web Deploy dbFullSql est une méthode héritée qui n’est plus recommandée à l’exception de certains scénarios spécifiques, telles que la migration à partir de SQL Server Compact vers SQL Server.

Les méthodes indiquées dans ce didacticiel sont des bases de données SQL Server, pas SQL Server Compact. Pour plus d’informations sur la façon de déployer une base de données SQL Server Compact, consultez [Visual Studio Web déploiement avec SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md).

Les méthodes indiquées dans ce didacticiel nécessitent que vous utilisez le Web Deploy méthode de publication. Si vous préférez publier d’une autre méthode, telle que FTP, système de fichiers ou extensions serveur FrontPage, consultez [déploiement d’une base de données séparément de déploiement d’applications web](https://go.microsoft.com/fwlink/p/?LinkId=282413#databaseseparate) dans le plan de contenu de déploiement Web pour Visual Studio et ASP.NET.

### <a name="entity-framework-code-first-migrations"></a>Entity Framework Code First Migrations

Dans la version 4.3 du Entity Framework, Microsoft a introduit les Migrations Code First. Migrations Code First automatise le processus d’apporter des modifications incrémentielles à un modèle de données et de propager ces modifications à la base de données. Dans les versions antérieures de Code First, vous permettent généralement de Entity Framework supprimer et recréer la base de données chaque fois que vous modifiez le modèle de données. Cela n’est pas un problème dans le développement, car les données de test sont recréées facilement, mais en production généralement à mettre à jour le schéma de base de données sans supprimer la base de données. Permet à la fonctionnalité Migrations Code First mettre à jour de la base de données sans devoir supprimer et recréer. Vous pouvez laisser Code First automatiquement décider comment apporter les modifications de schéma requis, ou vous pouvez écrire du code qui personnalise les modifications. Pour une introduction aux Migrations Code First, consultez [Migrations Code First](https://msdn.microsoft.com/library/hh770484.aspx).

Lorsque vous déployez un projet web, Visual Studio peut automatiser le processus de déploiement d’une base de données qui est géré par les Migrations Code First. Lorsque vous créez le profil de publication, vous sélectionnez une case à cocher intitulée exécuter les Migrations Code First (s’exécute sur le démarrage de l’application). Ce paramètre entraîne le processus de déploiement configurer automatiquement le fichier de Web.config d’application sur le serveur de destination, afin que le Code First utilise le `MigrateDatabaseToLatestVersion` classe d’initialiseur.

Visual Studio ne fait rien avec la base de données pendant le processus de déploiement. Lorsque l’application déployée accède à la base de données pour la première fois après le déploiement, Code First crée automatiquement la base de données ou met à jour le schéma de base de données vers la dernière version. Si l’application implémente une méthode de la valeur initiale de Migrations, la méthode s’exécute après la création de la base de données ou le schéma est mis à jour.

Dans ce didacticiel, vous allez utiliser les Migrations Code First pour déployer la base de données d’application.

### <a name="the-dbdacfx-web-deploy-provider"></a>Le fournisseur de Web Deploy dbDacFx

Pour une base de données SQL Server qui n’est pas géré par Entity Framework Code First, vous pouvez sélectionner une case à cocher qui est l’étiquette de la base de données mise à jour lorsque vous configurez le profil de publication. Lors du déploiement initial, le fournisseur dbDacFx crée les tables et autres objets de base de données dans la base de données de destination pour correspondre à la base de données source. Sur les déploiements suivants, le fournisseur détermine quelle est la différence entre les bases de données source et de destination, et il met à jour le schéma de la base de données de destination pour correspondre à la base de données source. Par défaut, le fournisseur ne procédez aux modifications qui provoquent la perte de données, telles que lorsqu’une table ou une colonne est supprimée.

Cette méthode n’automatise pas le déploiement de données dans les tables de base de données, mais vous pouvez créer des scripts pour ce faire et configurer Visual Studio pour les exécuter pendant le déploiement. Une autre raison pour exécuter des scripts lors du déploiement consiste à apporter des modifications de schéma ne peut pas être effectuées automatiquement, car ils provoqueraient la perte de données.

Dans ce didacticiel, vous allez utiliser le fournisseur dbDacFx pour déployer la base de données d’appartenance ASP.NET.

## <a name="troubleshooting-during-this-tutorial"></a>Résolution des problèmes au cours de ce didacticiel

Quand une erreur se produit pendant le déploiement, ou si le site déployé ne s’exécute pas correctement, les messages d’erreur ne fournissent pas toujours une solution évidente. Pour vous aider dans certains problèmes courants, un [référence page Résolution des problèmes](troubleshooting.md) est disponible. Si vous obtenez un message d’erreur ou quelque chose ne fonctionne pas lorsque vous parcourez les didacticiels, veillez à consulter la page Résolution des problèmes.

## <a name="comments-welcome"></a>Bienvenue dans les commentaires

Commentaires sur les didacticiels sont les bienvenus, et lorsque le didacticiel est mis à jour chaque effort sera fait tenir des corrections de compte ou des suggestions relatives aux améliorations fournies dans les commentaires de didacticiel.

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Prérequis

Ce didacticiel a été écrite pour les produits suivants :

- Windows 8 ou Windows 7.
- Visual Studio 2012 ou Visual Studio 2012 Express pour le Web avec [la dernière mise à jour](https://go.microsoft.com/fwlink/?LinkId=272486).
- [Azure SDK pour Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=254364)

Vous pouvez suivre le didacticiel à l’aide de Visual Studio 2010 SP1 ou Visual Studio 2013, mais certaines captures d’écran sera différentes et certaines fonctionnalités seront différents.

Si vous utilisez Visual Studio 2013, installez [Azure SDK pour Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkID=324322).

Si vous utilisez Visual Studio 2010 SP1, installez les logiciels suivants :

- [Azure SDK pour Visual Studio 2010](https://go.microsoft.com/fwlink/?LinkID=254269)
- [SQL Server Express LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=SQLLocalDBOnly_11_0)
- [SQL Server Data Tools](https://msdn.microsoft.com/library/hh500335.aspx).

En fonction du nombre des dépendances du Kit de développement logiciel vous disposez sur votre ordinateur, l’installation du SDK Azure peut prendre beaucoup de temps, à partir de plusieurs minutes à une demi-heure, voire plus. Vous devez le kit SDK Azure même si vous prévoyez de publier sur un fournisseur d’hébergement tiers au lieu de vers Azure, fonctionnalités de publication, car le SDK inclut les dernières mises à jour sur le web de Visual Studio.

> [!NOTE]
> Ce didacticiel a été écrit avec version 1.8.1 du SDK Azure. Depuis des versions plus récentes avec des fonctionnalités supplémentaires ont été publiées. Les didacticiels ont été mis à jour pour mentionner ces fonctionnalités et un lien vers les ressources qui ont plus d’informations à leur sujet.


Les instructions et les captures d’écran sont basées sur Windows 8, mais les didacticiels expliquent les différences pour Windows 7.

Un autre logiciel est requis afin de terminer le didacticiel, mais vous n’êtes pas obligé d’en avez pas encore installé. Ce didacticiel vous guidera à travers les étapes permettant de l’installer lorsque vous en avez besoin.

## <a name="download-the-sample-application"></a>Télécharger l’exemple d’application

L’application que vous allez déployer est nommée Contoso University et a déjà été créée pour vous. Il est une version simplifiée d’un site web de l’université, faiblement selon de l’application Contoso University décrite dans le [didacticiels Entity Framework sur le site ASP.NET](https://asp.net/entity-framework/tutorials).

Lorsque vous avez les composants requis installés, téléchargez le [application web Contoso University](https://go.microsoft.com/fwlink/p/?LinkId=282627). Le *.zip* fichier contient plusieurs versions du projet. Pour les étapes du didacticiel, commencez avec le projet situé dans le dossier/C#. Pour voir à quoi ressemble le projet à la fin des didacticiels, ouvrez le projet dans le dossier ContosoUniversity-End.

Pour préparer le projet pour étudier les étapes du didacticiel, procédez comme suit :

1. Enregistrer les fichiers de solution ContosoUniversity à partir du dossier c# dans un dossier nommé ContosoUniversity dans le dossier que vous utilisez pour travailler avec des projets Visual Studio.

    Par défaut, cela est le dossier suivant pour Visual Studio 2012 :

    `C:\Users\<username>\Documents\Visual Studio 2012\Projects`

    (Pour les captures d’écran dans ce didacticiel, le dossier du projet se trouve dans le répertoire racine sur le `C`: lecteur.)
2. Démarrez Visual Studio et ouvrez le projet.
3. Dans **l’Explorateur de solutions**, avec le bouton droit de la solution, puis cliquez sur **la restauration des packages EnableNuGet**.
4. Générez la solution.
5. Si vous obtenez des erreurs de compilation, restaurer manuellement les packages NuGet :

    1. Dans **l’Explorateur de solutions**, avec le bouton droit de la solution, puis cliquez sur **gérer les Packages NuGet pour la Solution**.
    2. En haut de la **gérer les Packages NuGet** boîte de dialogue vous verrez **NuGet certains packages sont manquants à partir de cette solution. Cliquez pour restaurer.** Cliquez sur le **restaurer** bouton.
    3. Régénérez la solution.
6. Appuyez sur CTRL + F5 pour exécuter l’application.

    L’application s’ouvre sur la page d’accueil de Contoso University.

    ![Développement de la Page d’accueil](introduction/_static/image1.png)

    (Il peut y avoir un délai d’attente pendant que Visual Studio démarre l’instance de SQL Server Express LocalDB, et vous pouvez obtenir une erreur de délai d’expiration si qui processus prend trop de temps. Dans ce cas simplement démarrer le projet à nouveau.)

Les pages de site Web sont accessibles à partir de la barre de menus et vous permettent d’effectuer les fonctions suivantes :

- Afficher les statistiques des étudiants (la page About).
- Afficher, modifier, supprimer et ajouter des étudiants.
- Afficher et modifier des cours.
- Afficher et modifier des formateurs.
- Afficher et modifier des départements.

Voici les captures d’écran de quelques pages représentatifs.

![Développement de Page d’étudiants](introduction/_static/image2.png)

![Ajouter les étudiants Page développement](introduction/_static/image3.png)

## <a name="review-application-features-that-affect-deployment"></a>Passez en revue les fonctionnalités qui affectent le déploiement d’applications

Les fonctionnalités suivantes de l’application affectent le déploiement ou ce que vous devez faire pour le déployer. Chacune d’elles est expliquée plus en détail dans les didacticiels suivants dans la série.

- Contoso University utilise une base de données SQL Server pour stocker les données d’application tels que les noms de student et instructor. La base de données contient un mélange de données de test et les données de production, et lorsque vous déployez en production vous devez exclure les données de test.
- L’application utilise le système d’appartenance ASP.NET, qui stocke les informations de compte utilisateur dans une base de données SQL Server. L’application définit un utilisateur administrateur qui a accès à des informations restreintes. Vous devez déployer la base de données d’appartenance sans comptes de test, mais avec un compte d’administrateur.
- L’application utilise un tiers journalisation des erreurs et utilitaire de création de rapports. Cet utilitaire est fourni dans un assembly qui doit être déployé avec l’application.
- L’utilitaire de journalisation erreur écrit les informations d’erreur dans des fichiers XML dans un dossier. Vous devez vous assurer que le compte ASP.NET sous lequel s’exécute dans le site déployé a l’autorisation d’écriture à ce dossier, et vous devez exclure ce dossier de déploiement. (Sinon, données de journal d’erreur à partir de l’environnement de test peuvent être déployées en production et/ou de fichiers de journal des erreurs de production peuvent être supprimés.)
- L’application inclut certains paramètres qui doivent être modifiés dans déployé *Web.config* fichier en fonction de l’environnement de destination (test, intermédiaire ou production) et d’autres paramètres qui doivent être modifiées en fonction de la build configuration (Debug ou Release).
- La solution Visual Studio inclut un projet de bibliothèque de classes. Uniquement l’assembly qui génère ce projet doit être déployé, pas le projet lui-même.

## <a name="summary"></a>Récapitulatif

Dans ce premier didacticiel de la série, vous avez téléchargé l’exemple de projet Visual Studio et passé en revue les fonctionnalités de site qui affectent la façon dont vous déployez l’application. Dans les didacticiels suivants, vous préparer pour le déploiement en définissant une partie de ces éléments à être gérées automatiquement. D’autres vous prendre soin de manuellement.

> [!div class="step-by-step"]
> [Next](preparing-databases.md)
