---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
title: Créer le projet | Microsoft Docs
author: Erikre
description: Cette série de didacticiels vous apprend les notions de base de la création d’une application Web Forms ASP.NET à l’aide de ASP.NET 4.5 et Microsoft Visual Studio Express 2013 pour nous...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 2ce36f78-8ecb-4ab1-b748-6d0ab633ea3f
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
msc.type: authoredcontent
ms.openlocfilehash: de0f8092342a8ba8979a31e9a97b603e44e6a85d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831403"
---
<a name="create-the-project"></a>Créer le projet
====================
par [Erik Reitan](https://github.com/Erikre)

[Télécharger le projet de Wingtip Toys exemple (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [télécharger l’E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Cette série de didacticiels vous apprend les notions de base de la création d’une application Web Forms ASP.NET à l’aide de ASP.NET 4.5 et Microsoft Visual Studio Express 2013 pour le Web. Un Visual Studio 2013 [projet avec du code source c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) est disponible pour accompagner cette série de didacticiels.


Dans ce didacticiel vous créez, passez en revue et exécuter le projet par défaut dans Visual Studio, qui vous permettra de vous familiariser avec les fonctionnalités d’ASP.NET. En outre, vous allez examiner l’environnement Visual Studio.

## <a name="what-youll-learn"></a>Ce que vous allez apprendre :

- Comment créer un nouveau projet Web Forms.
- La structure de fichiers du projet Web Forms.
- Guide pratique pour exécuter le projet dans Visual Studio.
- Les différentes fonctionnalités de l’application de formulaires Web par défaut.
- Les notions de base sur l’utilisation de l’environnement Visual Studio.

## <a name="creating-the-project"></a>Création du projet

1. Ouvrez Visual Studio.
2. Sélectionnez **nouveau projet** à partir de la **fichier** menu dans Visual Studio. 

    ![Créer le projet - nouvel élément de Menu projet](create-the-project/_static/image1.png)
3. Sélectionnez le **modèles**  - &gt; **Visual C#**  - &gt; **Web** groupe de modèles sur la gauche.
4. Choisissez le **Application Web ASP.NET** modèle dans la colonne centrale.  
 Cette série de didacticiels à l’aide de .NET Framework 4.5.2.
5. Nommez votre projet *WingtipToys* et choisissez le **OK** bouton. 

    ![Créer le projet - boîte de dialogue Nouveau projet](create-the-project/_static/image2.png)

    > [!NOTE]
    > Le nom du projet dans cette série de didacticiels est **WingtipToys**. Il est recommandé d’utiliser ce *exacte* nom du projet afin que le code fourni dans l’ensemble de la série de didacticiels fonctionne comme prévu.

6. Cliquez sur le **modifier l’authentification** bouton. Sélectionnez **comptes d’utilisateur individuels** et cliquez sur le **OK** bouton.

7. Sélectionnez le **Web Forms** modèle et cliquez sur le **OK** bouton.

    ![Créer le projet - modèle de projet](create-the-project/_static/image3.png)

Le projet prendra un peu de temps à créer. Lorsqu’il est prêt, ouvrez le **Default.aspx** page.

![Créer le projet - modèle de projet](create-the-project/_static/image4.png)

Vous pouvez basculer entre **conception** vue et **Source** vue en sélectionnant une option en bas de la fenêtre centrale. **Conception** vue affiche les pages Web ASP.NET, les pages maîtres, les pages de contenu, les pages HTML, et des contrôles utilisateur à l’aide d’un affichage proche du WYSIWYG. **Source** mode affiche le balisage HTML de votre page Web, que vous pouvez modifier.

> [!TIP] 
> 
> **Comprendre les infrastructures ASP.NET**
> 
> ASP.NET Web Forms vous permet de générer des sites Web dynamiques à l’aide d’un modèle familier de glisser-déplacer, pilotée par événements. Une aire de conception et des centaines de composants et contrôles vous permettent de créer rapidement des sites gérés par l’interface utilisateur sophistiqués et puissants avec accès aux données. Le Store de Jouets Wingtip est basée sur des Web Forms ASP.NET, mais la plupart des concepts que vous allez apprendre dans cette série de didacticiels sont applicables à l’ensemble d’ASP.NET.
> 
> ASP.NET propose quatre structures de développement principal :
> 
> - [ASP.NET Web Forms](../../../index.md)  
>  L’infrastructure Web Forms destiné aux développeurs qui préfèrent la programmation déclarative et basée sur le contrôle, tels que Microsoft Windows Forms (WinForms) et WPF/XAML/Silverlight. Il offre un modèle de développement concepteur WYSIWYG, donc il est fréquemment utilisé avec les développeurs qui souhaitent pour un environnement de développement rapide d’applications pour le développement web. Si vous êtes novice en programmation du web et que vous êtes familiarisé avec les outils de développement de clients Microsoft RAD traditionnels (par exemple, pour Visual Basic et Visual c#), vous pouvez rapidement créer une application web sans avoir d’expérience en HTML et JavaScript.
> - [ASP.NET MVC](../../../../mvc/index.md)  
>  ASP.NET MVC destiné aux développeurs qui souhaitent dans les modèles et principes tels que le développement piloté par test, la séparation des préoccupations, inversion de contrôle (IoC) et l’injection de dépendance (DI). Cette infrastructure encourage en séparant la couche de logique métier d’une application web à partir de sa couche de présentation.
> - [Pages Web ASP.NET](../../../../web-pages/index.md)  
>  ASP.NET Web Pages destiné aux développeurs qui veulent un récit de développement web simple, le long des lignes de PHP. Dans le modèle de Pages Web, vous créez des pages HTML, puis ajoutez les code basé sur le serveur à la page afin de contrôler dynamiquement la façon dont ce balisage est rendu. Pages Web est spécifiquement conçu pour être un framework léger, et il est le point d’entrée le plus simple dans ASP.NET pour les personnes qui connaissez HTML mais de ne pas avoir une expérience de programmation large - par exemple, les étudiants ou amateurs. Il est également un bon moyen pour les développeurs web qui connaissent PHP ou des infrastructures similaires pour commencer à utiliser ASP.NET.
> - [Application de Page ASP.NET unique](../../../../single-page-application/index.md)  
>  Application de Page ASP.NET unique (SPA) vous permet de créer des applications qui incluent des interactions côté client significatives à l’aide de HTML 5, 3 de CSS et JavaScript. ASP.NET et Web Tools 2012.2 Update est livré un nouveau modèle pour la création d’applications à page unique à l’aide de knockout.js et API Web ASP.NET. Outre le nouveau modèle SPA, nouveaux modèles SPA créés par la Communauté sont également disponibles au téléchargement.
> 
> Outre les infrastructures de développement principal quatre, ASP.NET offre également des technologies supplémentaires qui sont importantes à connaître et maîtriser, mais ne sont pas couverts dans cette série de didacticiels :
> 
> - [API Web ASP.NET](../../../../web-api/index.md) -une infrastructure pour générer des services HTTP qui atteignent une large gamme de clients, y compris les navigateurs et appareils mobiles.
> - [ASP.NET SignalR](../../../../signalr/index.md) -une bibliothèque qui facilite le développement des fonctionnalités web en temps réel.


### <a name="reviewing-the-project"></a>Examen du projet

Dans Visual Studio, le **l’Explorateur de solutions** fenêtre vous permet de gérer des fichiers pour le projet. Jetons un œil au niveau des dossiers qui ont été ajoutés à votre application dans **l’Explorateur de solutions**. Le modèle d’application web ajoute une structure de dossiers de base :

![Créer le projet - Explorateur de solutions](create-the-project/_static/image5.png)

Visual Studio crée certains dossiers initiales et les fichiers de votre projet. Les premiers fichiers vous travaillerez avec plus loin dans ce didacticiel sont les suivantes :

| **Fichier** | **Fonction** |
| --- | --- |
| *Default.aspx* | En général, la première page affichée lorsque l’application est exécutée dans un navigateur. |
| *Site.Master* | Une page qui vous permet de créer une disposition et l’utilisation standard un comportement cohérent pour les pages dans votre application. |
| *Global.asax* | Un fichier facultatif qui contient le code pour répondre aux événements de niveau application et de niveau session déclenchés par ASP.NET ou par des modules HTTP. |
| *Web.config* | Les données de configuration pour une application. |

### <a name="running-the-default-web-application"></a>L’Application Web par défaut en cours d’exécution

L’application Web par défaut fournit une expérience riche basée sur la prise en charge et de fonctionnalités intégrées. Sans aucune modification au projet de formulaires Web par défaut, l’application est prête à s’exécuter sur votre navigateur Web local.

1. Appuyez sur la ***F5*** enfoncée tout dans Visual Studio.   
 L’application est générée et l’afficher dans votre navigateur Web.  

    ![Créer le projet : par défaut de Page](create-the-project/_static/image6.png)
2. Une fois que vous avez terminé de révision de l’application en cours d’exécution, fermez la fenêtre du navigateur.

Il existe trois pages principales dans cette application de Web par défaut : *Default.aspx* (accueil), *About.aspx*, et *Contact.aspx*. Chacune de ces pages est accessible à partir de la barre de navigation supérieure. Il existe également deux pages supplémentaires contenues dans le dossier Account, la page Register.aspx et la page Login.aspx. Ces deux pages permettent d’utiliser les fonctionnalités de l’appartenance d’ASP.NET pour créer, stocker et valider les informations d’identification de l’utilisateur.

## <a name="aspnet-web-forms-background"></a>Web Forms ASP.NET en arrière-plan

ASP.NET Web Forms sont des pages qui sont basées sur la technologie Microsoft ASP.NET, dans laquelle code qui s’exécute sur le serveur de manière dynamique génère la sortie de page Web sur le navigateur ou le périphérique client. Une page Web Forms ASP.NET restitue automatiquement le HTML conforme au navigateur correct des fonctionnalités telles que les styles, disposition et ainsi de suite. Web Forms sont compatibles avec n’importe quel langage pris en charge par le common language runtime .NET, tels que Microsoft Visual Basic et Microsoft Visual c#. En outre, les Web Forms reposent sur le [Microsoft .NET Framework](https://msdn.microsoft.com/vstudio/aa496123), qui fournit des avantages tels que d’un environnement géré, la sécurité de type et l’héritage.

Quand une page Web Forms ASP.NET s’exécute, la page passe par un cycle de vie dans lequel il effectue une série d’étapes de traitement. Ces étapes incluent l’initialisation, l’instanciation des contrôles, restauration et la gestion de l’état, code gestionnaire d’événements en cours d’exécution et de rendu. Lorsque vous serez familiarisé avec la puissance d’ASP.NET Web Forms, il est important de comprendre le [cycle de vie de page ASP.NET](https://msdn.microsoft.com/library/ms178472(v=vs.100).aspx) afin que vous pouvez écrire du code à l’étape de cycle de vie approprié à l’effet que vous avez l’intention.

Lorsqu’un serveur Web reçoit une demande pour une page, il recherche la page, traite, il envoie au navigateur et ignore ensuite toutes les informations de page. Si l’utilisateur demande à nouveau la même page, le serveur répète toute la séquence, le retraitement de la page à partir de zéro. Autrement dit, un serveur n’a aucune mémoire de pages dont les pages traitées sont sans état. L’infrastructure de page ASP.NET gère automatiquement la tâche de maintenance de l’état de votre page et ses contrôles, et il vous offre des moyens explicite pour maintenir l’état des informations spécifiques aux applications.

> [!TIP] 
> 
> **Fonctionnalités des applications Web dans le modèle d’Application Web Forms**
> 
> Le modèle d’Application ASP.NET Web Forms fournit un ensemble complet de fonctionnalités intégrées. Il vous fournit non seulement un *Home.aspx* page, un *About.aspx* page, un *Contact.aspx* page, mais inclut également des fonctionnalités d’appartenance qui enregistre les utilisateurs et l’enregistre leurs informations d’identification afin qu’ils peuvent se connecter à votre site Web. Cette présentation fournit plus d’informations sur certaines des fonctionnalités contenues dans le modèle d’Application Web Forms ASP.NET et comment elles sont utilisées dans l’application Wingtip Toys.
> 
> **Appartenance**
> 
> [ASP.NET](https://msdn.microsoft.com/library/yh26yfzy.aspx) Identity stocke les informations d’identification de vos utilisateurs dans une base de données créée par l’application. Quand vos utilisateurs de se connectent, l’application valide le fait de leurs informations d’identification en lisant la base de données. De votre projet *compte* dossier contient les fichiers qui implémentent différentes parties d’appartenance : l’inscription, connexion, modification d’un mot de passe et autoriser l’accès. En outre, ASP.NET Web Forms prend en charge OAuth et OpenID. Ces améliorations de l’authentification autoriser les utilisateurs à se connecter à votre site à l’aide des informations d’identification existantes, à partir de ces comptes, ainsi que Facebook, Twitter, Windows Live, Google.
> 
> ![Créer le projet - Explorateur de solutions (identité ASP.NET)](create-the-project/_static/image7.png)
> 
> Par défaut, le modèle crée une base de données d’appartenance à l’aide d’un nom de base de données par défaut sur une instance de SQL Server Express LocalDB, le serveur de base de données de développement qui est fourni avec Visual Studio Express 2013 pour le Web.
> 
> **SQL Server Express LocalDB**
> 
> [SQL Server Express LocalDB](https://technet.microsoft.com/library/hh510202.aspx) est une version légère de SQL Server qui possède de nombreuses fonctionnalités de programmabilité d’une base de données SQL Server. SQL Server Express LocalDB s’exécute en mode utilisateur et doté d’une installation rapide, sans aucune configuration qui contient une liste courte de conditions préalables d’installation. Dans Microsoft SQL Server, toute base de données ou le code Transact-SQL peut être déplacé à partir de SQL Server Express LocalDB pour SQL Server et SQL Azure sans aucune étape de mise à niveau. Par conséquent, SQL Server Express LocalDB peut être utilisé comme un environnement de développement pour les applications ciblant toutes les éditions de SQL Server. SQL Server Express LocalDB permet des fonctionnalités telles que des procédures stockées, fonctions définies par l’utilisateur et les agrégats, intégration de .NET Framework, les types spatiaux et d’autres qui ne sont pas disponibles dans SQL Server Compact.
> 
> **Pages maîtres**
> 
> Un [page maître ASP.NET](https://msdn.microsoft.com/library/wtxbf3hh.aspx) définit une apparence cohérente et le comportement de toutes les pages de votre application. La disposition de la page maître fusionne avec le contenu d’une page de contenu individuel pour produire la dernière page que l’utilisateur voit. Dans l’application Wingtip Toys, vous allez modifier le *Site.master* page maître afin que toutes les pages dans le site Web de Wingtip Toys partagent la même barre logo distinctive de navigation.
> 
> **HTML5**
> 
> Le modèle d’Application ASP.NET Web Forms prend en charge [HTML5](http://www.w3schools.com/html/html5_intro.asp), qui est la dernière version du langage de balisage HTML. HTML5 prend en charge les nouveaux éléments et des fonctionnalités qui facilitent la création de sites Web.
> 
> **Modernizr**
> 
> Pour les navigateurs qui ne prennent pas en charge HTML5, vous pouvez utiliser [Modernizr](http://www.modernizr.com/). Modernizr est une bibliothèque JavaScript open source qui peut détecter si un navigateur prend en charge des fonctionnalités HTML5 et activez-les si elle n’est pas le cas. Dans le modèle d’Application ASP.NET Web Forms, Modernizr est installé sous forme de package NuGet.
> 
> **Bootstrap**
> 
> Utilisent les modèles de projet Visual Studio 2013 [Bootstrap](http://getbootstrap.com/), une infrastructure de mise en page et des thèmes créée par Twitter. Programme d’amorçage utilise CSS3 pour fournir une conception réactive, ce qui signifie que des dispositions puissent s’adapter dynamiquement aux tailles de fenêtre de navigateur différents. Vous pouvez également utiliser la fonctionnalité de thèmes de Bootstrap pour facilement effectuer un changement dans l’apparence de l’application. Par défaut, le modèle d’Application Web ASP.NET dans Visual Studio 2013 inclut Bootstrap comme package NuGet.
> 
> **Packages NuGet**
> 
> Le modèle d’Application Web Forms ASP.NET inclut un ensemble de [NuGet](http://www.nuget.org/) packages. Ces packages fournissent des fonctionnalités basées sur des composants sous la forme d’outils et bibliothèques open source. Il existe une grande variété de packages pour vous aider à créer et tester vos applications. Visual Studio facilite l’ajouter, supprimer et mettre à jour les packages NuGet. Les développeurs peuvent créer et ajouter des packages pour NuGet également.
> 
> ![Créer le projet - boîte de dialogue de NuGet](create-the-project/_static/image8.png)
> 
> Lorsque vous installez un package, NuGet copie des fichiers à votre solution et crée automatiquement les modifications nécessaires, telles que l’ajout de références et en modifiant la configuration associée à votre application Web. Si vous décidez de supprimer la bibliothèque, NuGet supprime les fichiers et annule toutes les modifications dans votre projet afin qu’aucun encombrement n’est laissé. NuGet est disponible à partir de la **outils** menu dans Visual Studio.
> 
> **jQuery**
> 
> [jQuery](http://jquery.com/) est une bibliothèque JavaScript rapide et concise qui simplifie la traversée de document HTML, la gestion des événements, l’animation et interactions Ajax pour le développement web rapide. La bibliothèque JavaScript jQuery est incluse dans le modèle d’Application Web Forms ASP.NET sous forme de package NuGet.
> 
> **Validation non obstrusive**
> 
> Contrôles de validation intégrés ont été configurés pour utiliser JavaScript non obstrusif pour la logique de validation côté client. Ainsi considérablement réduit la quantité de JavaScript rendu inline dans le balisage de page et réduit la taille globale de la page. Validation non obstrusive est ajoutée dans le monde entier pour le modèle d’Application Web Forms ASP.NET en fonction du paramètre dans le &lt;appSettings&gt; élément de la *Web.config* fichier à la racine de l’application.
> 
> **Entity Framework Code First**
> 
> Outre les fonctionnalités dans le modèle d’Application ASP.NET Web Forms, l’application Wingtip Toys utilise [Entity Framework Code First](https://weblogs.asp.net/scottgu/archive/2010/12/08/announcing-entity-framework-code-first-ctp5-release.aspx), qui est une bibliothèque NuGet qui permet le développement centrée sur le code lorsque vous travaillez avec des données. En réalité, il crée la partie de la base de données de votre application pour vous selon le code que vous écrivez. À l’aide d’Entity Framework, récupérer et manipuler des données en tant qu’objets fortement typés. Ainsi vous concentrer sur la logique métier dans votre application plutôt que les détails de la façon dont les données sont accessibles.
> 
> Pour plus d’informations sur les bibliothèques installées et les packages inclus avec le modèle Web Forms ASP.NET, consultez la liste des packages NuGet installés. Pour ce faire, dans Visual Studio créer un nouveau projet Web Forms, sélectionnez **outils**  - &gt; **Library Package Manager**  - &gt; **gérer Les Packages NuGet pour la Solution**, puis sélectionnez **packages installés** dans le **gérer les Packages NuGet** boîte de dialogue.


### <a name="touring-visual-studio"></a>Touring Visual Studio

Les fenêtres principales dans Visual Studio incluent le **l’Explorateur de solutions**, le **Explorateur de serveurs** (**Explorateur de base de données** dans Express), la **propriétés Fenêtre**, le **boîte à outils**, le **barre d’outils**et le **fenêtre de Document**.

![Créer le projet - boîte de dialogue de NuGet](create-the-project/_static/image9.png)

Pour plus d’informations sur Visual Studio, consultez [Guide visuel vers Visual Web Developer](https://msdn.microsoft.com/library/ee410104.aspx).

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous avez créé, révisé et exécuter l’application de Web Forms par défaut. Vous avez passé en revue les différentes fonctionnalités de l’application de formulaires Web par défaut et l’avez appris les notions de base sur l’utilisation de l’environnement Visual Studio. Dans les didacticiels suivants, vous allez créer la couche d’accès aux données.

## <a name="additional-resources"></a>Ressources supplémentaires

[Choisir le bon modèle de programmation](../../../videos/how-do-i/choosing-the-right-programming-model.md)   
[Projets d’Application Web et projets de Site Web](https://msdn.microsoft.com/library/dd547590.aspx)   
[Vue d’ensemble de Pages de formulaires Web ASP.NET](https://msdn.microsoft.com/library/428509ah.aspx)

> [!div class="step-by-step"]
> [Précédent](introduction-and-overview.md)
> [Suivant](create_the_data_access_layer.md)
