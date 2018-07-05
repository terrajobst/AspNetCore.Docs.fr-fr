---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: Principes de base ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Ce laboratoire pratique est basé sur le Store de musique MVC (Model View Controller), une application du didacticiel qui présente et explique étape par étape comment utiliser ASP.NET MV...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 3a282d02ba929eb86571e92f190550614962524d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386573"
---
# <a name="aspnet-mvc-4-fundamentals"></a>Principes de base ASP.NET MVC 4

par [Web Camps Team](https://twitter.com/webcamps)

[Télécharger le Kit de formation de Web Camps](https://aka.ms/webcamps-training-kit)

Ce laboratoire pratique est basé sur le Store de musique MVC (Model View Controller), une application du didacticiel qui présente et explique étape par étape comment utiliser ASP.NET MVC et Visual Studio. Tout au long de l’atelier, vous allez découvrir la simplicité, encore alimentation de l’utilisation conjointe de ces technologies. Vous démarrez avec une application simple et générez jusqu'à ce que vous avez une Application Web ASP.NET MVC 4 entièrement fonctionnelle.

Ce laboratoire fonctionne avec ASP.NET MVC 4.

Si vous souhaitez Explorer la version d’ASP.NET MVC 3 de l’application du didacticiel, vous pouvez le trouver dans [MVC-musique-Store](https://github.com/evilDave/MVC-Music-Store).

Ce laboratoire pratique suppose que le développeur a une expérience dans les technologies de développement Web, tels que HTML et JavaScript.

> [!NOTE]
> Tous les exemples de code et extraits de code sont inclus dans le Kit de formation Camps Web, disponible à l’adresse [les versions de Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Le projet spécifique à ce laboratoire est disponible à l’adresse [principes de base ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a>L’application Music Store

L’application web Store de musique qui est construite tout au long de ce laboratoire comprend trois parties principales : shopping, extraction et administration. Visiteurs sera en mesure de parcourir des albums par genre, ajouter des albums à son panier, passez en revue leur sélection et enfin passer à l’extraction pour vous connecter et terminer la commande. En outre, les administrateurs de magasin seront en mesure de gérer les albums disponibles, ainsi que leurs principales propriétés.

![Écrans de musique Store](aspnet-mvc-4-fundamentals/_static/image1.png "écrans Music Store")

*Écrans de musique Store*

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a>ASP.NET MVC 4 Essentials

Application de Store de musique est générée à l’aide de **contrôleur MVC (Model View)**, un modèle d’architecture qui sépare une application en trois composants principaux :

- **Modèles**: objets de modèle sont les parties de l’application qui implémentent la logique de domaine. Souvent, les objets de modèle également récupèrent et de stockent l’état de modèle dans une base de données.
- **Vues :** vues sont les composants qui affichent l’interface utilisateur de l’application (IU). En règle générale, cette interface utilisateur est créée à partir des données de modèle. Un exemple serait la vue edit d’Albums qui affiche les zones de texte et une liste déroulante, selon l’état actuel d’un objet de l’Album.
- **Contrôleurs :** contrôleurs sont les composants qui gèrent l’interaction utilisateur, manipulent le modèle et finalement sélectionnent une vue à restituer l’interface utilisateur. Dans une application MVC, la vue affiche uniquement des informations ; le contrôleur gère les entrées et interactions des utilisateurs, et y répond.

Le modèle MVC vous aide à créer des applications qui séparent les différents aspects de l’application (logique d’entrée, logique métier et logique d’interface utilisateur), tout en assurant un couplage lâche entre ces éléments. Cette séparation vous permet de gérer la complexité lorsque vous générez une application, car elle permet de vous concentrer sur l’un des aspects de l’implémentation à la fois. En outre, le modèle MVC rend plus facile de tester des applications, encourage également l’utilisation du développement piloté par tests (TDD) pour créer des applications.

Le **ASP.NET MVC** framework offre une alternative au modèle Web Forms ASP.NET pour la création d’applications Web basées sur ASP.NET MVC. Le **ASP.NET MVC** est une infrastructure de présentation simple et facilement testable, qui (comme les applications Web basées sur des formulaires) est intégré avec les fonctionnalités ASP.NET existantes, telles que des pages maîtres et basée sur l’appartenance authentification et vous offre toute la puissance de .NET core. Cela est utile si vous êtes déjà familiarisé avec ASP.NET Web Forms, car toutes les bibliothèques que vous utilisez déjà sont également disponibles dans ASP.NET MVC 4.

En outre, le couplage lâche entre les trois principaux composants d’une application MVC favorise également développement en parallèle. Par exemple, un développeur peut travailler sur la vue, un deuxième développeur peut travailler sur la logique du contrôleur et un troisième peut se concentrer sur la logique métier dans le modèle.

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Objectifs

Dans cet atelier pratique, vous allez apprendre comment :

- Créer une application ASP.NET MVC à partir de zéro, basée sur le didacticiel d’Application du Store musique
- Ajouter des contrôleurs pour gérer des URL à la page d’accueil du site et pour ses principales fonctionnalités de navigation
- Ajouter une vue pour personnaliser le contenu affiché, ainsi que son style
- Ajouter des classes de modèle pour contenir et gérer la logique des données et de domaine
- Utiliser un modèle de vue pour passer des informations à partir d’actions de contrôleur pour les modèles de vue
- Explorez le nouveau modèle de ASP.NET MVC 4 pour les applications internet

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prérequis

Vous devez disposer des éléments suivants pour terminer ce laboratoire :

- [Visual Studio 2012 Express pour Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (lire [annexe A](#AppendixA) pour obtenir des instructions sur la façon d’installer)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Installation

**L’installation des extraits de Code**

Pour des raisons pratiques, une grande partie du code que vous gérez le long de ce laboratoire est disponible en tant que les extraits de code Visual Studio. Pour installer les extraits de code exécuter **.\Source\Setup\CodeSnippets.vsi** fichier.

Si vous n’êtes pas familiarisé avec les extraits de Code Visual Studio et que vous souhaitiez savoir comment les utiliser, vous pouvez faire référence à l’annexe de ce document &quot; [extraits de Code à l’aide d’annexe c :](#AppendixC)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exercices

Ce laboratoire pratique est constitué par les exercices suivants :

1. [Exercice 1 : Création de projet d’Application Web du Store musique ASP.NET MVC](#Exercise1)
2. [Exercice 2 : Création d’un contrôleur](#Exercise2)
3. [Exercice 3 : Passage de paramètres à un contrôleur](#Exercise3)
4. [Exercice 4 : Création d’une vue](#Exercise4)
5. [Exercice 5 : Création d’un modèle de vue](#Exercise5)
6. [Exercice 6 : Utilisation de paramètres dans la vue](#Exercise6)
7. [Exercice 7 : Tour d’horizon nouveau modèle ASP.NET MVC 4](#Exercise7)

> [!NOTE]
> Chaque exercice est accompagné par un **fin** dossier contenant la solution obtenue, vous devez obtenir après avoir effectué les exercices. Si vous avez besoin d’aide supplémentaire sur l’utilisation via les exercices, vous pouvez utiliser cette solution comme guide.


Durée estimée pour effectuer ce laboratoire : **60 minutes**.

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a>Exercice 1 : Création de projet d’Application Web du Store musique ASP.NET MVC

Dans cet exercice, vous allez apprendre à créer une application ASP.NET MVC dans Visual Studio 2012 Express pour Web, ainsi que son organisation des dossiers principaux. En outre, vous allez apprendre à ajouter un nouveau contrôleur et les rendre afficher une chaîne simple dans la page d’accueil de l’application.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a>Tâche 1 : création du projet d’Application Web ASP.NET MVC

1. Dans cette tâche, vous allez créer un projet d’application ASP.NET MVC vide à l’aide du modèle MVC Visual Studio. Démarrer **Visual Studio Express pour Web**.
2. Dans le menu **Fichier**, cliquez sur **Nouveau projet**.
3. Dans le **nouveau projet** boîte de dialogue Sélectionnez le **ASP.NET MVC 4 Web Application** projet type, situé sous **Visual c#,** **Web** modèle liste.
4. Modifier le **nom** à *MvcMusicStore*.
5. Définir l’emplacement de la solution à l’intérieur d’un nouveau **commencer** dossier dans le dossier Source de cet exercice, par exemple **[YOUR-HOL-PATH] \Source\Ex01-CreatingMusicStoreProject\Begin**. Cliquez sur **OK**.

    ![Créer la boîte de dialogue Nouveau projet](aspnet-mvc-4-fundamentals/_static/image2.png "créer la boîte de dialogue Nouveau projet")

    *Créer la boîte de dialogue Nouveau projet*
6. Dans le **nouveau projet ASP.NET MVC 4** sélectionnez boîte de dialogue le **base** modèle et vous assurer que le **moteur d’affichage** sélectionné est **Razor**. Cliquez sur **OK**.

    ![Nouvelle boîte de dialogue projet ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image3.png "nouvelle boîte de dialogue projet ASP.NET MVC 4")

    *Nouvelle boîte de dialogue projet ASP.NET MVC 4*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a>Tâche 2 : Explorer la Structure de Solution

L’infrastructure ASP.NET MVC inclut un modèle de projet Visual Studio qui vous permet de créer des applications Web prenant en charge le modèle MVC. Ce modèle crée une nouvelle application Web ASP.NET MVC avec les dossiers requis, les modèles d’élément et les entrées de fichier de configuration.

Dans cette tâche, vous allez examiner la structure de solution pour comprendre les éléments qui sont impliqués et leurs relations. Les dossiers suivants sont inclus dans toute l’application ASP.NET MVC, car l’infrastructure ASP.NET MVC par défaut utilise un &quot;convention sur configuration&quot; approche et en fait des hypothèses par défaut en fonction de dossier d’affectation de noms conventions.

1. Une fois que le projet est créé, passez en revue la structure de dossiers a été créée dans l’Explorateur de solutions sur le côté droit :

    ![Structure de dossier de MVC ASP.NET dans l’Explorateur de solutions](aspnet-mvc-4-fundamentals/_static/image4.png "structure ASP.NET MVC dossier dans l’Explorateur de solutions")

    *Structure de dossier de MVC ASP.NET dans l’Explorateur de solutions*

   1. **Contrôleurs**. Ce dossier contient les classes de contrôleur. Dans une application basée sur MVC, les contrôleurs sont chargés de gérer l’interaction de l’utilisateur final, manipuler le modèle et finalement en choisissant une vue à restituer l’interface utilisateur.

       > [!NOTE]
       > L’infrastructure MVC requiert les noms de tous les contrôleurs se terminent par &quot;contrôleur&quot;-par exemple, HomeController, LoginController ou ProductController.
   2. **Modèles**. Ce dossier est fourni pour les classes qui représentent le modèle d’application pour l’application Web MVC. Cela inclut généralement le code qui définit les objets et la logique permettant d’interagir avec le magasin de données. En règle générale, les objets de modèle réels seront dans les bibliothèques de classes séparées. Toutefois, lorsque vous créez une nouvelle application, vous pouvez inclure les classes et les déplacer dans les bibliothèques de classes séparées ultérieurement dans le cycle de développement.
   3. **Vues**. Ce dossier est l’emplacement recommandé pour les vues, les composants responsables de l’affichage de l’interface utilisateur de l’application. Les vues utilisent des fichiers .aspx, .ascx, .cshtml et .master, en plus de tous les fichiers qui sont liées au rendu des vues. Dossier Views contient un dossier pour chaque contrôleur ; le dossier est nommé avec le préfixe du nom de contrôleur. Par exemple, si vous disposez d’un contrôleur nommé **HomeController**, le dossier Views contient un dossier appelé Home. Par défaut, lorsque l’infrastructure ASP.NET MVC charge une vue, il recherche un fichier .aspx avec le nom de vue requis dans le dossier Views\controllerName (**vues [Nom_contrôleur] [Action] .aspx**) ou (**vues [Nom_contrôleur] [Action] .cshtml**) pour les vues Razor.

      > [!NOTE]
      > Outre les dossiers indiqués précédemment, une application Web MVC utilise le **Global.asax** de fichier pour définir le routage d’URL globales par défaut et elle utilise le **Web.config** fichier pour configurer l’application.

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a>Tâche 3 : ajout d’un HomeController

Dans les applications ASP.NET qui n’utilisent pas l’infrastructure MVC, l’intervention de l’utilisateur est organisée autour des pages et déclenchement et la gestion des événements à partir de ces pages. En revanche, l’interaction utilisateur avec les applications ASP.NET MVC est organisée autour des contrôleurs et leurs méthodes d’action.

En revanche, infrastructure ASP.NET MVC mappe des URL aux classes qui sont désignés comme contrôleurs. Contrôleurs de traiter les demandes entrantes, gèrent les entrées d’utilisateur et les interactions, exécuter la logique d’application appropriée et de déterminer la réponse à envoyer au client (afficher le code HTML, télécharger un fichier, rediriger vers une URL différente, un etc.). Dans le cas d’affichage HTML, une classe de contrôleur appelle généralement un composant de vue séparé afin de générer le balisage HTML pour la demande. Dans une application MVC, la vue affiche uniquement des informations ; le contrôleur gère les entrées et interactions des utilisateurs, et y répond.

Dans cette tâche, vous allez ajouter une classe de contrôleur qui gérera les URL pour la page d’accueil du site Music Store.

1. Avec le bouton droit **contrôleurs** dossier dans l’Explorateur de solutions, sélectionnez **ajouter** , puis **contrôleur** commande :

    ![Ajouter une commande de contrôleur](aspnet-mvc-4-fundamentals/_static/image5.png "ajouter une commande de contrôleur")

    *Ajouter la commande de contrôleur*
2. Le **ajouter un contrôleur** boîte de dialogue apparaît. Nommez le contrôleur *HomeController* et appuyez sur **ajouter**.

    ![Ajouter la boîte de dialogue contrôleur](aspnet-mvc-4-fundamentals/_static/image6.png "contrôleur la boîte de dialogue Ajouter")

    *Ajouter la boîte de dialogue contrôleur*
3. Le fichier **HomeController.cs** est créé dans le **contrôleurs** dossier. Pour disposer le **HomeController** retourner une chaîne sur son action Index, remplacez le **Index** méthode avec le code suivant :

    (Code Snippet - *bases d’ASP.NET MVC 4 - Ex1 HomeController Index*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tâche 4 : exécution de l’Application

Dans cette tâche, vous allez essayer l’application dans un navigateur web.

1. Appuyez sur **F5** pour exécuter l’Application. Le projet est compilé et démarre de serveur Web IIS local. Local serveur Web IIS s’ouvre automatiquement un navigateur web pointant vers l’URL du serveur Web.

    ![Application qui s’exécute dans un navigateur web](aspnet-mvc-4-fundamentals/_static/image7.png "Application s’exécutant dans un navigateur web")

    *Application qui s’exécute dans un navigateur web*

    > [!NOTE]
    > Le serveur Web IIS local s’exécutera le site Web sur un numéro de port libre aléatoire. Dans la figure ci-dessus, le site est en cours d’exécution à `http://localhost:50103/`, il utilise le port 50103. Votre numéro de port peut varier.
2. Fermez le navigateur.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a>Exercice 2 : Création d’un contrôleur

Dans cet exercice, vous allez apprendre à mettre à jour le contrôleur pour implémenter une fonctionnalité simple de l’application de Store de musique. Ce contrôleur de définir les méthodes d’action pour gérer chacune des requêtes spécifiques suivantes :

- Une page de liste de genres de musique dans le Store musique
- Une page de navigation qui répertorie tous les albums de musique pour un genre particulier
- Une page de détails qui affiche des informations sur un album musical spécifique

Pour la portée de cet exercice, ces actions renvoie simplement une chaîne à ce stade.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a>Tâche 1 : ajout d’une nouvelle classe StoreController

Dans cette tâche, vous allez ajouter un nouveau contrôleur.

1. Si elle est déjà ouverte, démarrez **Visual Studio Express 2012 pour le Web**.
2. Dans le **fichier** menu, choisissez **ouvrir le projet**. Dans la boîte de dialogue Ouvrir un projet, accédez à **Source\Ex02-CreatingAController\Begin**, sélectionnez **Begin.sln** et cliquez sur **Open**. Ou bien, vous pouvez continuer avec la solution que vous avez obtenue à l’issue de l’exercice précédent.

   1. Si vous avez ouvert le fourni **commencer** solution, vous devrez télécharger certains packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.
   2. Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.

      > [!NOTE]
      > Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
3. Ajoutez le nouveau contrôleur. Pour ce faire, cliquez sur le **contrôleurs** dossier dans l’Explorateur de solutions, sélectionnez **ajouter** , puis le **contrôleur** commande. Modifier le **nom du contrôleur** à *StoreController*, puis cliquez sur **ajouter**.

    ![Ajouter la boîte de dialogue contrôleur](aspnet-mvc-4-fundamentals/_static/image8.png "contrôleur la boîte de dialogue Ajouter")

    *Ajouter la boîte de dialogue contrôleur*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a>Tâche 2 : modification Actions de la StoreController

Dans cette tâche, vous allez modifier les méthodes de contrôleur sont appelées **actions**. Actions sont responsables de la gestion des demandes d’URL et déterminer le contenu qui doit être envoyé au navigateur ou de l’utilisateur qui a appelé l’URL.

1. Le **StoreController** classe possède déjà un **Index** (méthode). Vous l’utiliserez plus loin dans cet atelier pour implémenter la page qui répertorie tous les genres du magasin de musique. Pour l’instant, remplacez simplement le **Index** méthode avec le code suivant qui retourne une chaîne &quot;Hello from Store.Index()&quot;:

    (Code Snippet - *bases d’ASP.NET MVC 4 - Ex2 StoreController Index*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. Ajouter **Parcourir** et **détails** méthodes. Pour ce faire, ajoutez le code suivant à la **StoreController**:

    (Code Snippet - *bases d’ASP.NET MVC 4 - Ex2 StoreController BrowseAndDetails*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tâche 3 : exécution de l’Application

Dans cette tâche, vous allez essayer l’application dans un navigateur web.

1. Appuyez sur **F5** pour exécuter l’Application.
2. Le projet démarre dans le **accueil** page. Modifier l’URL pour vérifier la mise en œuvre de chaque action.

    1. **/ Store**. Vous verrez  **&quot;Hello from Store.Index()&quot;**.
    2. **/ Store/Parcourir**. Vous verrez  **&quot;Hello from Store.Browse()&quot;**.
    3. **/ Store/détails**. Vous verrez  **&quot;Hello from Store.Details()&quot;**.

        ![Navigation StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "StoreBrowse de navigation")

        *Navigation /Store/Browse*
3. Fermez le navigateur.

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a>Exercice 3 : Passage de paramètres à un contrôleur

Jusqu'à présent, vous avez retournait les chaînes constantes à partir des contrôleurs. Dans cet exercice, vous allez apprendre à passer des paramètres à un contrôleur à l’aide de l’URL et la chaîne de requête, puis effectue les actions de la méthode répondre avec le texte dans le navigateur.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a>Tâche 1 - Ajout de paramètre de Genre à StoreController

Dans cette tâche, vous allez utiliser le **querystring** pour envoyer des paramètres pour le **Parcourir** méthode d’action dans le **StoreController**.

1. Si elle est déjà ouverte, démarrez **Visual Studio Express pour Web**.
2. Dans le **fichier** menu, choisissez **ouvrir le projet**. Dans la boîte de dialogue Ouvrir un projet, accédez à **Source\Ex03-PassingParametersToAController\Begin**, sélectionnez **Begin.sln** et cliquez sur **Open**. Ou bien, vous pouvez continuer avec la solution que vous avez obtenue à l’issue de l’exercice précédent.

   1. Si vous avez ouvert le fourni **commencer** solution, vous devrez télécharger certains packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.
   2. Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.

      > [!NOTE]
      > Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
3. Ouvrez **StoreController** classe. Pour ce faire, dans le **l’Explorateur de solutions**, développez le **contrôleurs** dossier et double-cliquez sur **StoreController.cs**.
4. Modifier le **Parcourir** méthode, en ajoutant un paramètre de chaîne pour demander un genre spécifique. ASP.NET MVC automatiquement passer toute chaîne de requête ou paramètres nommés de publication de formulaire **genre** à cette méthode d’action lorsqu’elle est appelée. Pour ce faire, remplacez le **Parcourir** méthode avec le code suivant :

    (Code Snippet - *bases d’ASP.NET MVC 4 - Ex3 StoreController BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> Vous utilisez le **HttpUtility.HtmlEncode** méthode utilitaire pour empêche les utilisateurs d’injecter du Javascript dans la vue avec un lien comme   **/Store/Parcourir ? Genre =&lt;script&gt;Window.location = '[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.
> 
> Pour en savoir plus, visitez [cet article msdn](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Tâche 2 : exécution de l’Application

Dans cette tâche, vous essayez de l’Application dans un navigateur web et utiliser le **genre** paramètre.

1. Appuyez sur **F5** pour exécuter l’Application.
2. Le projet démarre dans le **accueil** page. Remplacez l’URL par   */Store/Parcourir ? Genre = Disco* pour vérifier que l’action reçoit le paramètre de genre.

    ![Navigation StoreBrowseGenre = Disco](aspnet-mvc-4-fundamentals/_static/image10.png "navigation StoreBrowseGenre = Disco")

    *Navigation /Store/Browse ? Genre = Disco*
3. Fermez le navigateur.

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a>Tâche 3 : ajout d’un paramètre d’Id incorporé dans l’URL

Dans cette tâche, vous allez utiliser le **URL** à passer un **Id** paramètre à la **détails** méthode d’action de la **StoreController**. Convention de routage consiste à traiter le segment d’URL après le nom de la méthode d’action en tant que paramètre nommé de la valeur par défaut de ASP.NET MVC **Id**. Si votre méthode d’action a un paramètre nommé Id, puis ASP.NET MVC automatiquement passera le segment d’URL pour vous en tant que paramètre. Dans l’URL **détails/Store/5**, **Id** sera interprété comme **5**.

1. Modifier le **détails** méthode de la **StoreController**, l’ajout un **int** paramètre appelé **id**. Pour ce faire, remplacez le **détails** méthode avec le code suivant :

    (Code Snippet - *bases d’ASP.NET MVC 4 - Ex3 StoreController DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tâche 4 : exécution de l’Application

Dans cette tâche, vous essayez de l’Application dans un navigateur web et utiliser le **Id** paramètre.

1. Appuyez sur **F5** pour exécuter l’Application.
2. Le projet démarre dans le **accueil** page. Remplacez l’URL par */Store/Details/5* pour vérifier que l’action reçoit le paramètre id.

    ![Navigation StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "StoreDetails5 de navigation")

    *Navigation /Store/Details/5*

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a>Exercice 4 : Création d’une vue

Jusqu'à présent, vous avez retournait chaînes à partir d’actions de contrôleur. Bien que ce soit un moyen utile de comprendre le fonctionnement des contrôleurs, il n’est pas comment vos applications Web réelles sont générées. Les vues sont des composants qui fournissent une meilleure approche pour générer le code HTML au navigateur à l’aide de fichiers de modèle.

Dans cet exercice, vous allez apprendre à ajouter une page maître de disposition pour configurer un modèle pour le contenu HTML commun, une feuille de style pour améliorer l’apparence du site et, enfin, un modèle de vue pour activer le HomeController renvoyer le HTML.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a>Tâche 1 : modification du fichier \_layout.cshtml

Le fichier **~/Views/Shared/\_layout.cshtml** vous permet de configurer un modèle pour le code HTML commun à utiliser sur le site Web complet. Dans cette tâche, vous allez ajouter une page maître de disposition avec un en-tête commun avec des liens vers la zone de page d’accueil et Store.

1. Si elle est déjà ouverte, démarrez **Visual Studio Express pour Web**.
2. Dans le **fichier** menu, choisissez **ouvrir le projet**. Dans la boîte de dialogue Ouvrir un projet, accédez à **Source\Ex04-CreatingAView\Begin**, sélectionnez **Begin.sln** et cliquez sur **Open**. Ou bien, vous pouvez continuer avec la solution que vous avez obtenue à l’issue de l’exercice précédent.

   1. Si vous avez ouvert le fourni **commencer** solution, vous devrez télécharger certains packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.
   2. Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.

      > [!NOTE]
      > Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
3. Le fichier  <strong>\_layout.cshtml</strong> contient la disposition du conteneur HTML pour toutes les pages sur le site. Il inclut le <strong>&lt;html&gt;</strong> élément pour la réponse HTML, ainsi que le <strong>&lt;head&gt;</strong> et <strong>&lt;corps&gt;</strong> éléments. <strong>@RenderBody()</strong> dans le code HTML corps identifier les zones cette vue modèles seront en mesure de le remplir avec le contenu dynamique.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. Ajouter un en-tête commun avec des liens vers la zone de page d’accueil et Store sur toutes les pages du site. Pour ce faire, ajoutez le code suivant ci-dessous &lt;corps&gt; instruction.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. Inclure une balise div pour restituer la section de corps de chaque page. Remplacez  <strong>@RenderBody()</strong> avec le code higlighted suivant : (c#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > Saviez-vous ? Visual Studio 2012 offre des extraits de code qui la rendent facile d’ajouter le code utilisé couramment dans HTML, les fichiers de code et bien plus encore ! L’essayer en tapant **&lt;div&gt;** et en appuyant sur **onglet** à deux reprises pour insérer un complète **div** balise.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a>Tâche 2 - Ajout feuille de style CSS

Le modèle de projet vide inclut un fichier CSS très simplifié qui inclut uniquement les styles utilisés pour afficher les formes de base et les messages de validation. Vous allez utiliser CSS et des images (potentiellement fournis par un concepteur) supplémentaires pour améliorer l’apparence du site.

Dans cette tâche, vous allez ajouter une feuille de style CSS pour définir les styles du site.

1. Le fichier CSS et les images sont inclus dans le **Source\Assets\Content** dossier de ce laboratoire. Afin de les ajouter à l’application, faites glisser leur contenu à partir d’un **Windows Explorer** fenêtre dans le **l’Explorateur de solutions** dans Visual Studio Express pour le Web, comme indiqué ci-dessous :

    ![En faisant glisser le contenu de style](aspnet-mvc-4-fundamentals/_static/image12.png "en faisant glisser le contenu de style")

    *En faisant glisser le contenu de style*
2. Une boîte de dialogue d’avertissement s’affiche, vous demandant de confirmer à remplacer **Site.css** fichier et certaines images existantes. Vérifiez **appliquer à tous les éléments** et cliquez sur **Oui**.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a>Tâche 3 : ajout d’un modèle de vue

Dans cette tâche, vous allez ajouter un modèle de vue pour générer la réponse HTML qui utilise la page maître de mise en page et ajouté des CSS dans cet exercice.

1. Pour utiliser un modèle de vue lorsque vous parcourez la page d’accueil du site, vous devez d’abord indiquer que, au lieu de renvoyer une chaîne, le **HomeController Index** méthode retournera un **vue**. Ouvrez **HomeController** classe et modifier son **Index** méthode pour retourner un **ActionResult**, et qu’il renvoie **View()**.

    (Code Snippet - *bases d’ASP.NET MVC 4 - Ex4 HomeController Index*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. Maintenant, vous devez ajouter un modèle de vue approprié. Pour ce faire, **avec le bouton droit** à l’intérieur de la **Index** méthode d’action, puis **ajouter une vue**. Cela fera apparaître le **ajouter une vue** boîte de dialogue.

    ![Ajout d’une vue à partir de la méthode Index](aspnet-mvc-4-fundamentals/_static/image13.png "Ajout d’une vue à partir de la méthode Index")

    *Ajout d’une vue à partir de la méthode Index*
3. Le **ajouter une vue** boîte de dialogue apparaît pour générer un fichier de modèle de vue. Par défaut, cette boîte de dialogue pré-remplit le nom du modèle de vue afin qu’elle corresponde à la méthode d’action qui va l’utiliser. Étant donné que vous avez utilisé le **ajouter une vue** menu contextuel dans le **Index** méthode d’action dans le HomeController, le **ajouter une vue** boîte de dialogue comporte des Index en tant que le nom de la vue par défaut. Cliquez sur **Ajouter**.

    ![Afficher la boîte de dialogue Ajouter](aspnet-mvc-4-fundamentals/_static/image14.png "afficher la boîte de dialogue Ajouter")

    *Afficher la boîte de dialogue Ajouter*
4. Visual Studio génère un **Index.cshtml** afficher le modèle à l’intérieur de la **Views\Home** dossier, puis l’ouvre.

    ![Vue de l’Index créé d’accueil](aspnet-mvc-4-fundamentals/_static/image15.png "vue accueil Index créé")

    *Vue d’accueil Index créé*

    > [!NOTE]
    > nom et l’emplacement de la **Index.cshtml** fichier concerne et suit les conventions de nommage d’ASP.NET MVC par défaut.
    > 
    > Le dossier \Views\**accueil** correspond au nom du contrôleur (**accueil** contrôleur). Le nom de modèle de vue (**Index**), correspond à la méthode d’action de contrôleur qui affichent la vue.
    > 
    > De cette façon, ASP.NET MVC évite d’avoir à spécifier explicitement le nom ou l’emplacement d’un modèle de vue lorsque vous utilisez cette convention de nommage pour retourner une vue.
5. Le modèle de vue généré est basé sur le  **\_layout.cshtml** modèle précédemment défini. Mettre à jour la propriété ViewBag.Title à **accueil**et modifier le contenu principal **c’est la Page d’accueil**, comme illustré dans le code ci-dessous :

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. Sélectionnez **MvcMusicStore** projet dans l’Explorateur de solutions et appuyez sur **F5** pour exécuter l’Application.

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a>Tâche 4 : vérification

Afin de vérifier que vous avez correctement effectué toutes les étapes dans l’exercice précédent, procédez comme suit :

Avec l’application ouverte dans un navigateur, vous devez noter que :

1. Méthode d’action du HomeController Index trouvé et affiché le **\Views\Home\Index.cshtml** afficher le modèle, même si le code appelé **retourner View()**, car le modèle de vue suivi le convention d’affectation de noms standard.
2. La Page d’accueil affiche le message d’accueil défini dans le **\Views\Home\Index.cshtml** afficher le modèle.
3. À l’aide de la Page d’accueil est la  **\_layout.cshtml** modèle, et par conséquent, le message d’accueil est contenu dans la mise en page de site standard HTML.

    ![Vue d’Index à l’aide de la LayoutPage et le style défini d’accueil](aspnet-mvc-4-fundamentals/_static/image16.png "accueil vue de l’Index à l’aide de la LayoutPage et le style défini")

    *Vue d’Index accueil à l’aide de la LayoutPage et le style défini*

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a>Exercice 5 : Création d’un modèle de vue

Jusqu’ici, vous avez apportées à vos vues afficher codée en dur HTML, mais, pour créer des applications web dynamiques, le modèle de vue doit recevoir des informations à partir du contrôleur. Est une technique courante pour être utilisée à cet effet la **ViewModel** modèle, ce qui permet à un contrôleur créer un package avec toutes les informations nécessaires pour générer la réponse HTML appropriée.

Dans cet exercice, vous allez apprendre à créer une classe ViewModel et ajoutez les propriétés requises : le nombre de genres dans le magasin et une liste de ces genres. Vous met également à jour le StoreController pour utiliser le ViewModel créé, et enfin, vous allez créer un nouveau modèle de vue qui affiche les propriétés mentionnées dans la page.

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a>Tâche 1 : création d’une classe ViewModel

Dans cette tâche, vous allez créer une classe ViewModel qui implémentera le scénario de la liste de genre Store.

1. Si elle est déjà ouverte, démarrez **Visual Studio Express pour Web**.
2. Dans le **fichier** menu, choisissez **ouvrir le projet**. Dans la boîte de dialogue Ouvrir un projet, accédez à **Source\Ex05-CreatingAViewModel\Begin**, sélectionnez **Begin.sln** et cliquez sur **Open**. Ou bien, vous pouvez continuer avec la solution que vous avez obtenue à l’issue de l’exercice précédent.

   1. Si vous avez ouvert le fourni **commencer** solution, vous devrez télécharger certains packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.
   2. Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.

      > [!NOTE]
      > Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
3. Créer un **ViewModels** dossier pour contenir le ViewModel. Pour ce faire, cliquez sur le niveau supérieur **MvcMusicStore** projet, sélectionnez **ajouter** , puis **nouveau dossier**.

    ![Ajout d’un nouveau dossier](aspnet-mvc-4-fundamentals/_static/image17.png "Ajout d’un nouveau dossier")

    *Ajout d’un nouveau dossier*
4. Nommez le dossier *ViewModels*.

    ![Dossier ViewModels dans l’Explorateur de solutions](aspnet-mvc-4-fundamentals/_static/image18.png "dossier ViewModels dans l’Explorateur de solutions")

    *Dossier ViewModels dans l’Explorateur de solutions*
5. Créer un **ViewModel** classe. Pour ce faire, cliquez sur le **ViewModels** dossier créé récemment, sélectionnez **ajouter** , puis **un nouvel élément**. Sous **Code**, choisissez le **classe** d’élément et nommez le fichier *StoreIndexViewModel.cs*, puis cliquez sur **ajouter**.

    ![Ajout d’une nouvelle classe](aspnet-mvc-4-fundamentals/_static/image19.png "ajoutant une nouvelle classe")

    *Ajout d’une nouvelle classe*

    ![Création de classe de StoreIndexViewModel](aspnet-mvc-4-fundamentals/_static/image20.png "StoreIndexViewModel de création de classe")

    *Création de classe de StoreIndexViewModel*

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a>Tâche 2 : ajout de propriétés à la classe ViewModel

Il existe deux paramètres à passer à partir de la StoreController pour le modèle de vue afin de générer la réponse HTML attendue : le nombre de genres dans le magasin et une liste de ces genres.

Dans cette tâche, vous allez ajouter ces 2 propriétés à la **StoreIndexViewModel** classe : **NumberOfGenres** (entier) et **Genres** (il s’agit d’une liste de chaînes).

1. Ajouter **NumberOfGenres** et **Genres** propriétés pour le **StoreIndexViewModel** classe. Pour ce faire, ajoutez les 2 lignes suivantes à la définition de classe :

    (Code Snippet - *ASP.NET MVC 4 principes de base - propriétés de Ex5 StoreIndexViewModel*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> Le **{get ; définir ;}**  notation utilise C# fonctionnalité des propriétés implémentées automatiquement. Il offre les avantages d’une propriété sans nécessiter de déclarer un champ de stockage.

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a>Tâche 3 : mise à jour StoreController à utiliser le StoreIndexViewModel

Le **StoreIndexViewModel** classe encapsule les informations nécessaires pour passer de **StoreController**de **Index** à un modèle de vue afin de générer une réponse (méthode) .

Dans cette tâche, vous mettrez à jour la **StoreController** à utiliser le **StoreIndexViewModel**.

1. Ouvrez **StoreController** classe.

    ![Ouverture de la classe de StoreController](aspnet-mvc-4-fundamentals/_static/image21.png "ouverture StoreController classe")

    *Classe de StoreController ouverture*
2. Pour pouvoir utiliser le **StoreIndexViewModel** classe à partir de la **StoreController**, ajouter l’espace de noms suivante en haut de la **StoreController** code :

    (Code Snippet - *ASP.NET MVC 4 principes de base - StoreIndexViewModel Ex5 à l’aide de ViewModels*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. Modifier le **StoreController**de **Index** méthode d’action afin qu’elle crée et remplit un **StoreIndexViewModel** de l’objet et le transmet à un modèle de vue génère une réponse HTML avec lui.

    > [!NOTE]
    > Dans le laboratoire &quot;accès aux données et les modèles ASP.NET MVC&quot; vous écrivez du code qui Récupère la liste des genres de magasin à partir d’une base de données. Dans le code suivant, vous allez créer un **liste** de genres de données factice qui rempliront le **StoreIndexViewModel**.
    > 
    > Après la création et la configuration de la **StoreIndexViewModel** de l’objet, il sera passé en tant qu’argument à la **vue** (méthode). Cela indique que le modèle de vue utilise cet objet pour générer une réponse HTML avec lui.
4. Remplacez le **Index** méthode avec le code suivant :

    (Code Snippet - *ASP.NET MVC 4 principes de base - méthode Ex5 StoreController Index*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> Si vous n’êtes pas familiarisé avec c#, vous pouvez supposer que l’utilisation **var** signifie que le **viewModel** variable est à liaison tardive. Qui n’est pas correct - le compilateur c# à l’aide en fonction de ce que vous affectez à la variable d’inférence de type pour déterminer si **viewModel** est de type **StoreIndexViewModel**. Par ailleurs, de compilation local **viewModel** variable comme un **StoreIndexViewModel** vous tapez get vérification de la compilation et la prise en charge de Visual Studio-éditeur de code.

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a>Tâche 4 : création d’un modèle de vue qui utilise StoreIndexViewModel

Dans cette tâche, vous allez créer un modèle de vue qui utilise un objet StoreIndexViewModel passé à partir du contrôleur pour afficher une liste de genres.

1. Avant de créer le nouveau modèle de vue, nous allons générer le projet afin que le **boîte de dialogue Vue ajouter** connaît le **StoreIndexViewModel** classe. Générez le projet en sélectionnant le **Build** élément de menu, puis **MvcMusicStore Build**.

    ![Génération du projet](aspnet-mvc-4-fundamentals/_static/image22.png "génération du projet")

    *Génération du projet*
2. Créer un nouveau modèle de vue. Pour ce faire, le bouton droit dans le **Index** (méthode), puis sélectionnez **ajouter une vue**.

    ![Ajout d’une vue](aspnet-mvc-4-fundamentals/_static/image23.png "Ajout d’une vue")

    *Ajout d’une vue*
3. Étant donné que le **boîte de dialogue Vue ajouter** a été appelée à partir de la **StoreController**, il ajoutera le modèle de vue par défaut dans un **\Views\Store\Index.cshtml** fichier. Vérifier le **créer une fortement--vue typée** case à cocher, puis sélectionnez **StoreIndexViewModel** en tant que le **classe de modèle**. En outre, assurez-vous que le moteur d’affichage sélectionné est **Razor**. Cliquez sur **Ajouter**.

    ![Afficher la boîte de dialogue Ajouter](aspnet-mvc-4-fundamentals/_static/image24.png "afficher la boîte de dialogue Ajouter")

    *Afficher la boîte de dialogue Ajouter*

    Le **\Views\Store\Index.cshtml** afficher le fichier de modèle est créé et ouvert. Selon les informations fournies à la **ajouter une vue** boîte de dialogue de la dernière étape, la vue de modèle s’attend à recevoir un **StoreIndexViewModel** instance en tant que les données à utiliser pour générer une réponse HTML. Vous remarquerez que le modèle hérite d’un `ViewPage<musicstore.viewmodels.storeindexviewmodel>` en c#.

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a>Tâche 5 : mise à jour le modèle de vue

Dans cette tâche, vous mettrez à jour le modèle de vue créé dans la dernière tâche pour récupérer le nombre de genres et leurs noms dans la page.

> [!NOTE]
> Vous utiliserez @ syntaxe (souvent appelé &quot;pépites de code&quot;) pour exécuter du code dans le modèle de vue.

1. Dans le **Index.cshtml** de fichiers, dans le **Store** dossier, remplacez son code par le suivant :

[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *Getting Model properties and methods with Visual Studio's IntelliSense*
    > 
    > The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template. This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.
    > 
    > You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.
2. Boucle sur la liste de genre dans **StoreIndexViewModel** et créer un élément HTML **&lt;ul&gt;** à l’aide de la liste un **foreach** boucle.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. Appuyez sur **F5** pour exécuter l’Application et de parcourir **/Store**. Vous verrez la liste des genres passé dans le **StoreIndexViewModel** de l’objet à partir de la **StoreController** pour le modèle de vue.

    ![Vue affichant une liste de genres](aspnet-mvc-4-fundamentals/_static/image26.png "vue affichant une liste de genres")

    *Vue affichant une liste de genres*
4. Fermez le navigateur.

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a>Exercice 6 : Utilisation de paramètres dans la vue

Dans l’exercice 3, vous avez appris comment passer des paramètres au contrôleur. Dans cet exercice, vous allez apprendre à utiliser ces paramètres dans le modèle de vue. Pour ce faire, vous serez présentées tout d’abord aux classes de modèle qui vous aideront à gérer votre logique de données et de domaine. En outre, vous allez apprendre à créer des liens vers les pages à l’intérieur de l’application ASP.NET MVC sans se préoccuper des éléments comme les chemins d’accès de l’URL d’encodage.

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a>Tâche 1 : ajout de Classes de modèle

Contrairement aux ViewModels, qui sont créés uniquement à transmettre des informations à partir du contrôleur à la vue, les classes de modèle sont conçues pour contenir et de gérer la logique des données et de domaine. Dans cette tâche, vous allez ajouter deux classes de modèle pour représenter ces concepts : **Genre** et **Album**.

1. Si elle est déjà ouverte, démarrez **Visual Studio Express pour le Web**
2. Dans le **fichier** menu, choisissez **ouvrir le projet**. Dans la boîte de dialogue Ouvrir un projet, accédez à **Source\Ex06-UsingParametersInView\Begin**, sélectionnez **Begin.sln** et cliquez sur **Open**. Ou bien, vous pouvez continuer avec la solution que vous avez obtenue à l’issue de l’exercice précédent.

   1. Si vous avez ouvert le fourni **commencer** solution, vous devrez télécharger certains packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.
   2. Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.

      > [!NOTE]
      > Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
3. Ajouter un **Genre** classe de modèle. Pour ce faire, cliquez sur le **modèles** dossier dans le **l’Explorateur de solutions**, sélectionnez **ajouter** , puis le **un nouvel élément** option. Sous **Code**, choisissez le **classe** d’élément et nommez le fichier *Genre.cs*, puis cliquez sur **ajouter**.

    ![Ajout d’une classe](aspnet-mvc-4-fundamentals/_static/image27.png "Ajout d’une classe")

    *Ajout d’un nouvel élément*

    ![Ajouter une classe de modèle de Genre](aspnet-mvc-4-fundamentals/_static/image28.png "ajouter une classe de modèle de Genre")

    *Ajouter une classe de modèle de Genre*
4. Ajouter un **nom** propriété à la classe de Genre. Pour ce faire, ajoutez le code suivant :

    (Code Snippet - *bases d’ASP.NET MVC 4 - Ex6 Genre*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. Suivant la même procédure que précédemment, ajoutez un **Album** classe. Pour ce faire, cliquez sur le **modèles** dossier dans le **l’Explorateur de solutions**, sélectionnez **ajouter** , puis le **un nouvel élément** option. Sous **Code**, choisissez le **classe** d’élément et nommez le fichier *Album.cs*, puis cliquez sur **ajouter**.
6. Ajouter deux propriétés à la classe Album : **Genre** et **titre**. Pour ce faire, ajoutez le code suivant :

    (Code Snippet - *bases d’ASP.NET MVC 4 - Ex6 Album*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a>Tâche 2 : ajout d’un StoreBrowseViewModel

Un **StoreBrowseViewModel** servira dans cette tâche pour afficher les Albums qui correspondent à un Genre sélectionné. Dans cette tâche, vous créez cette classe et puis ajoutez deux propriétés permettant de gérer le **Genre** et son **Album**de liste.

1. Ajouter un **StoreBrowseViewModel** classe. Pour ce faire, cliquez sur le **ViewModels** dossier dans le **l’Explorateur de solutions**, sélectionnez **ajouter** , puis le **un nouvel élément** option. Sous **Code**, choisissez le **classe** d’élément et nommez le fichier *StoreBrowseViewModel.cs*, puis cliquez sur **ajouter**.
2. Ajoutez une référence aux modèles dans **StoreBrowseViewModel** classe. Pour ce faire, ajoutez le code suivant à l’aide d’espace de noms :

    (Code Snippet - *bases d’ASP.NET MVC 4 - Ex6 UsingModel*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. Ajoutez deux propriétés à **StoreBrowseViewModel** classe : **Genre** et **Albums**. Pour ce faire, ajoutez le code suivant :

    (Code Snippet - *bases d’ASP.NET MVC 4 - Ex6 ModelProperties*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> What ' s **liste&lt;Album&gt;**  ? : à l’aide de cette définition de la **liste&lt;T&gt;**  type, où **T** contraint le type à des éléments de ce **liste** appartiennent dans ce cas, **Album** (ou un de ses descendants).
> 
> Cette capacité à concevoir des classes et méthodes qui diffèrent la spécification d’un ou plusieurs types jusqu'à ce que la classe ou la méthode est déclarée et instanciée par le code client est une fonctionnalité du langage c# appelé **génériques**.
> 
> **Liste&lt;T&gt;**  est l’équivalent générique de la **ArrayList** de type et est disponible dans le **System.Collections.Generic** espace de noms. Un des avantages de l’utilisation de **génériques** est que dans la mesure où le type est spécifié, vous n’avez pas besoin prendre en charge de la vérification des opérations telles que le cast des éléments dans des types **Album** comme vous le feriez avec un **ArrayList**.

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a>Tâche 3 : à l’aide de nouveau ViewModel dans le StoreController

Dans cette tâche, vous allez modifier le **StoreController**de **Parcourir** et **détails** méthodes d’action pour utiliser le nouveau **StoreBrowseViewModel** .

1. Ajouter une référence dans le dossier de modèles dans **StoreController** classe. Pour ce faire, développez le **contrôleurs** dossier dans le **l’Explorateur de solutions** et ouvrez le **StoreController** classe. Puis ajoutez le code suivant :

    (Code Snippet - *bases d’ASP.NET MVC 4 - Ex6 UsingModelInController*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. Remplacez le **Parcourir** méthode d’action à utiliser le **StoreViewBrowseController** classe. Vous allez créer un Genre et deux nouveaux objets Albums avec données factices (dans l’atelier pratique suivant vous allez consommer des données réelles à partir d’une base de données). Pour ce faire, remplacez le **Parcourir** méthode avec le code suivant :

    (Code Snippet - *bases d’ASP.NET MVC 4 - Ex6 BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. Remplacez le **détails** méthode d’action à utiliser le **StoreViewBrowseController** classe. Vous allez créer un nouveau **Album** objet à retourner à la **vue**. Pour ce faire, remplacez le **détails** méthode avec le code suivant :

    (Code Snippet - *bases d’ASP.NET MVC 4 - Ex6 DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a>Tâche 4 : ajout d’un modèle d’affichage Parcourir

Dans cette tâche, vous allez ajouter un **Parcourir** pour afficher des Albums trouvées pour un Genre spécifique.

1. Avant de créer le nouveau modèle de vue, vous devez créer le projet afin que le **ajouter une vue** dialogue connaît le **ViewModel** classe à utiliser. Générez le projet en sélectionnant le **Build** élément de menu, puis **MvcMusicStore Build**.
2. Ajouter un **Parcourir** vue. Pour ce faire, cliquez sur le **Parcourir** méthode d’action de la **StoreController** et cliquez sur **ajouter une vue**.
3. Dans le **ajouter une vue** boîte de dialogue, vérifiez que le nom de la vue est **Parcourir**. Vérifier le **créer une vue fortement typée** case à cocher et sélectionnez **StoreBrowseViewModel** à partir de la **classe de modèle** liste déroulante. Laissez les autres champs avec leur valeur par défaut. Cliquez ensuite sur **Ajouter**.

    ![Ajout d’une vue Parcourir](aspnet-mvc-4-fundamentals/_static/image29.png "Ajout d’une vue Parcourir")

    *Ajout d’une vue Parcourir*
4. Modifier le **Browse.cshtml** pour afficher des informations du Genre l’accès à la **StoreBrowseViewModel** objet qui est passé au modèle de vue. Pour ce faire, remplacez le contenu par le suivant : (c#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tâche 5 : exécution de l’Application

Dans cette tâche, vous allez tester que le **Parcourir** méthode récupère les Albums à partir de la **Parcourir** action de la méthode.

1. Appuyez sur **F5** pour exécuter l’Application.
2. Le projet démarre dans la page d’accueil. Remplacez l’URL par   **/Store/Parcourir ? Genre = Disco** pour vérifier que l’action retourne deux Albums.

    ![Exploration des Albums Disco Store](aspnet-mvc-4-fundamentals/_static/image30.png "navigation Albums Disco Store")

    *Navigation Albums Disco Store*

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a>Tâche 6 - Affichage des informations sur un Album spécifique

Dans cette tâche, vous allez implémenter le **Store/détails** pour afficher les informations sur un album spécifique. Dans cet atelier pratique, tout ce que vous afficherez l’album sur la figure déjà dans le **vue** modèle. C’est le cas, au lieu de créer un **StoreDetailsViewModel** (classe), vous allez utiliser actuel **StoreBrowseViewModel** modèle en passant l’Album à celui-ci.

1. Fermez le navigateur si nécessaire, pour revenir à la fenêtre Visual Studio. Ajouter un nouveau **détails** afficher pour le **StoreController**de **détails** méthode d’action. Pour ce faire, cliquez sur le **détails** méthode dans le **StoreController** classe et cliquez sur **ajouter une vue**.
2. Dans le **ajouter une vue** boîte de dialogue, vérifiez que le **nom de la vue** est **détails**. Vérifier le **créer une vue fortement typée** case à cocher et sélectionnez **Album** à partir de la **classe de modèle** liste déroulante. Laissez les autres champs avec leur valeur par défaut. Cliquez ensuite sur **Ajouter**. Cela crée et ouvre un **\Views\Store\Details.cshtml** fichier.

    ![Ajout d’une vue de détails](aspnet-mvc-4-fundamentals/_static/image31.png "Ajout d’une vue de détails")

    *Ajout d’une vue de détails*
3. Modifier le **Details.cshtml** fichier pour afficher des informations de l’Album, l’accès à la **Album** objet qui est passé au modèle de vue. Pour ce faire, remplacez le contenu par le suivant : (c#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Tâche 7 : exécution de l’Application

Dans cette tâche, vous allez tester que le **détails** View récupère les informations d’Album à partir de la **détaille action** (méthode).

1. Appuyez sur **F5** pour exécuter l’Application.
2. Le projet démarre dans le **accueil** page. Remplacez l’URL par **/Store/Details/5** pour vérifier les informations de l’album.

    ![Exploration en détail les Albums](aspnet-mvc-4-fundamentals/_static/image32.png "exploration en détail les Albums")

    *Exploration des détails de l’Album*

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a>Tâche 8 : ajout de liens entre les Pages

Dans cette tâche, vous allez ajouter un lien dans la vue de Store pour avoir un lien dans le nom de chaque Genre appropriés **/Store/Parcourir** URL. Ainsi, lorsque vous cliquez sur un Genre, par exemple **Disco**, il permet d’accéder à **/Store/Parcourir ? genre = Disco** URL.

1. Fermez le navigateur si nécessaire, pour revenir à la fenêtre Visual Studio. Mise à jour le **Index** page pour ajouter un lien vers le **Parcourir** page. Pour ce faire, dans le **l’Explorateur de solutions** développez la **vues** dossier, puis le **Store** dossier et double-cliquez sur le **Index.cshtml** page.
2. Ajouter un lien vers la vue Parcourir indiquant le genre sélectionné. Pour ce faire, remplacez le code en surbrillance suivant au sein de la **&lt;li&gt;** balises : (c#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > une autre approche serait liaison directement à la page, avec un code semblable au suivant :
   > 
   > &lt;un href =&quot;/Store/Parcourir ? genre =@genreName&quot;&gt;@genreName&lt;/a&gt;
   > 
   > Bien que cette approche fonctionne, elle dépend d’une chaîne codée en dur. Si vous renommez ultérieurement le contrôleur, vous devrez modifier cette instruction manuellement. Une meilleure alternative consiste à utiliser un **programme d’assistance HTML** (méthode). ASP.NET MVC inclut une méthode d’assistance HTML qui est disponible pour des tâches. Le **Html.ActionLink()** méthode d’assistance permet de facilement générer HTML **&lt;un&gt;** liens, s’assurer que les chemins d’URL sont correctement codées URL.
   > 
   > Htlm.ActionLink a plusieurs surcharges. Dans cet exercice, vous utiliserez une fonction qui accepte trois paramètres :
   > 
   > 1. Texte du lien, qui affiche le nom du Genre
   > 2. Nom d’action de contrôleur (**Parcourir**)
   > 3. Des valeurs de paramètre, en spécifiant le nom d’itinéraire (**Genre**) et la valeur (**nom du Genre**)

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a>Tâche 9 - exécution de l’Application

Dans cette tâche, vous allez tester que chaque Genre s’affiche avec un lien vers le texte approprié **/Store/Parcourir** URL.

1. Appuyez sur **F5** pour exécuter l’Application.
2. Le projet démarre dans la page d’accueil. Remplacez l’URL par **/Store** pour vérifier que chaque Genre des liens appropriés **/Store/Parcourir** URL.

    ![Genres de navigation avec des liens vers la page Parcourir](aspnet-mvc-4-fundamentals/_static/image33.png "Genres de navigation avec des liens vers la page Parcourir")

    *Genres de navigation avec des liens vers la page Parcourir*

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a>Tâche 10 : à l’aide de la Collection de ViewModel dynamique pour transmettre des valeurs

Dans cette tâche, vous allez apprendre à une méthode simple et efficace pour transmettre des valeurs entre le contrôleur et la vue sans apporter de modifications dans le modèle. ASP.NET MVC 4 fournit la collection &quot;ViewModel&quot;, ce qui peut être affecté à n’importe quelle valeur dynamique et accessible à l’intérieur des contrôleurs et les vues ainsi.

Vous allez maintenant utiliser le regroupement dynamique ViewBag pour passer d’une liste de &quot; **marqué d’une étoile genres** &quot; à partir du contrôleur à la vue. La vue Index de Store accédera à **ViewModel** et afficher les informations.

1. Fermez le navigateur si nécessaire, pour revenir à la fenêtre Visual Studio. Ouvrez **StoreController.cs** et modifier **Index** méthode pour créer une liste de marqué d’une étoile genres dans la collection de ViewModel :

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > Vous pouvez également utiliser la syntaxe **ViewBag [&quot;Starred&quot;]** pour accéder aux propriétés.
2. L’icône d’étoile **&quot;starred.png&quot;** est inclus dans le **Source\Assets\Images** dossier de ce laboratoire. Pour l’ajouter à l’application, faites glisser leur contenu à partir d’un **Windows Explorer** fenêtre dans le **l’Explorateur de solutions** dans Visual Web Developer Express, comme indiqué ci-dessous :

    ![Image étoile d’ajout à la solution](aspnet-mvc-4-fundamentals/_static/image34.png "image étoile d’ajout à la solution")

    *Ajout de l’image en étoile à la solution*
3. Ouvrez la vue **Store/Index.cshtml** et modifier le contenu. Vous allez lire les &quot;marqué d’une étoile&quot; propriété dans le **ViewBag** collection et demandez si le nom du genre en cours est dans la liste. Dans ce cas, vous afficherez une icône représentant une étoile direct sur le lien de genre.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a>Tâche 11 : exécution de l’Application

Dans cette tâche, vous allez tester que les genres avec une astérisque affichent une icône représentant une étoile.

1. Appuyez sur **F5** pour exécuter l’Application.
2. Le projet démarre dans le **accueil** page. Remplacez l’URL par **/Store** pour vérifier que chaque genre proposée est l’étiquette digne :

    ![Genres de navigation avec des éléments avec une astérisque](aspnet-mvc-4-fundamentals/_static/image35.png "Genres de navigation avec des éléments avec une astérisque")

    *Genres de navigation avec des éléments avec une astérisque*

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a>Exercice 7 : Tour d’horizon nouveau modèle d’ASP.NET MVC 4

Dans cet exercice, vous allez explorer les améliorations dans les modèles de projet ASP.NET MVC 4, jeter un coup de œil des fonctionnalités les plus pertinentes du nouveau modèle.

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a>Tâche 1 : Explorer le modèle d’Application ASP.NET MVC 4 Internet

1. Si elle est déjà ouverte, démarrez **Visual Studio Express pour le Web**
2. Sélectionnez le **fichier | Nouveau | Projet** commande de menu. Dans le **nouveau projet** boîte de dialogue, sélectionnez le **Visual C# | Web** modèle dans le volet gauche arborescence, puis choisissez le **ASP.NET MVC 4 Web Application**. **Nom** le projet *MusicStore* et mettre à jour le **nom de la solution** à *commencer*, puis sélectionnez un emplacement (ou laissez la valeur par défaut) et cliquez sur **OK** .

    ![Création d’un nouveau projet ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image36.png "création d’un nouveau projet ASP.NET MVC 4")

    *Création d’un nouveau projet ASP.NET MVC 4*
3. Dans le **nouveau projet ASP.NET MVC 4** boîte de dialogue, sélectionnez le **Application Internet** modèle de projet et cliquez sur **OK**. Notez que vous pouvez sélectionner Razor ou ASPX comme le moteur d’affichage.

    ![Création d’une Application ASP.NET MVC 4 Internet](aspnet-mvc-4-fundamentals/_static/image37.png "création d’une Application ASP.NET MVC 4 Internet")

    *Création d’une Application ASP.NET MVC 4 Internet*

    > [!NOTE]
    > Syntaxe Razor a été introduite dans ASP.NET MVC 3. Son objectif est de réduire le nombre de caractères et séquences de touches requis dans un fichier, en activant un rapide et le fluide de flux de travail de codage. Razor utilise existant C# /Visual Basic (ou autre) compétences linguistiques et offre une syntaxe de balisage de modèle qui permet un flux de travail de construction HTML exceptionnel.
4. Appuyez sur **F5** pour exécuter la solution et voir le modèle renouvelé. Vous pouvez consulter les fonctionnalités suivantes :

    1. **Modèles de moderne**

        Les modèles ont été renouvelés, en fournissant plus de styles modernes.

        ![Modèles ASP.NET MVC 4 redessiné](aspnet-mvc-4-fundamentals/_static/image38.png "redessiné modèles ASP.NET MVC 4")

        *Modèles ASP.NET MVC 4 redessiné*
    2. **Rendu adaptatif**

        Découvrez de redimensionnement de la fenêtre du navigateur et notez la façon dont la mise en page s’adapte dynamiquement à la nouvelle taille de fenêtre. Ces modèles utilisent la technique de rendu adaptatif pour afficher correctement dans les plateformes de bureau et mobiles sans aucune personnalisation.

        ![Modèle de projet ASP.NET MVC 4 dans les tailles de navigateur différents](aspnet-mvc-4-fundamentals/_static/image39.png "le modèle de projet ASP.NET MVC 4 dans les tailles autre navigateur")

        *Modèle de projet ASP.NET MVC 4 dans les tailles autre navigateur*
5. Fermez le navigateur pour arrêter le débogueur, revenez à Visual Studio.
6. Vous êtes maintenant en mesure d’Explorer la solution et découvrez quelques-unes des nouvelles fonctionnalités introduites par ASP.NET MVC 4 dans le modèle de projet.

    ![ASP.NET MVC 4-internet-application--modèle de projet](aspnet-mvc-4-fundamentals/_static/image40.png "le modèle de projet Application Internet ASP.NET MVC 4")

    *Le modèle de projet Application Internet ASP.NET MVC 4*

   1. **Balisage HTML5**

       Parcourir les vues de modèle pour rechercher le nouveau thème le balisage, par exemple ouvrir **About.cshtml** afficher au sein de **accueil** dossier.

       ![Nouveau modèle, en utilisant le balisage Razor et HTML5](aspnet-mvc-4-fundamentals/_static/image41.png "nouveau modèle, en utilisant le balisage Razor et HTML5")

       *Nouveau modèle, en utilisant le balisage Razor et HTML5*
   2. **Bibliothèques JavaScript inclus**

      1. **jQuery**: jQuery simplifie la traversée de document HTML, la gestion des événements, l’animation et les interactions Ajax.
      2. **jQuery UI**: cette bibliothèque fournit des abstractions d’interaction de bas niveau et d’avancée de l’animation, effets et de widgets de thème, reposant sur la bibliothèque JavaScript jQuery.

         > [!NOTE]
         > Vous pouvez en savoir plus sur jQuery et l’interface utilisateur jQuery dans [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).
      3. **KnockoutJS**: modèle par défaut de l’ASP.NET MVC 4 inclut désormais **KnockoutJS**, une infrastructure MVVM JavaScript qui vous permet de créer des applications web riches et hautement réactives à l’aide de JavaScript et HTML. Comme dans ASP.NET MVC 3, jQuery et bibliothèques d’interface utilisateur jQuery sont également inclus dans ASP.NET MVC 4.

          > [!NOTE]
          > Vous pouvez obtenir plus d’informations sur la bibliothèque KnockOutJS dans ce lien : [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/).
      4. **Modernizr**: cette bibliothèque s’exécute automatiquement, rendre votre site compatible avec les navigateurs plus anciens lors de l’utilisation des technologies HTML5 et CSS3.

          > [!NOTE]
          > Vous pouvez obtenir plus d’informations sur la bibliothèque Modernizr dans ce lien : [ http://www.modernizr.com/ ](http://www.modernizr.com/).
   3. **SimpleMembership inclus dans la solution**

       SimpleMembership a été conçu comme un remplacement pour le système de fournisseur de rôle ASP.NET et l’appartenance au précédent. Il possède de nombreuses nouvelles fonctionnalités qui facilitent pour les développeurs à sécuriser les pages web de manière plus flexible.

       Le modèle Internet a déjà configuré quelques éléments à intégrer SimpleMembership, par exemple, le contrôle AccountController est prêt à utiliser la OAuthWebSecurity (pour l’inscription du compte OAuth, login, gestion, etc.) et la sécurité Web.

       ![SimpleMembership inclus dans la solution](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership inclus dans la solution")

       *SimpleMembership inclus dans la solution*

       > [!NOTE]
       > Obtenir des informations supplémentaires [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) dans MSDN.

> [!NOTE]
> En outre, vous pouvez déployer cette application à Sites Web Windows Azure suit [publication annexe b : une Application ASP.NET MVC 4, à l’aide de Web Deploy](#AppendixB).


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Récapitulatif

En fin de cet atelier pratique, vous avez appris les notions de base d’ASP.NET MVC :

- Les éléments principaux d’une application MVC et comment ils interagissent
- Comment créer une Application ASP.NET MVC
- Comment ajouter et configurer des contrôleurs pour gérer les paramètres transmis à l’URL et la chaîne de requête
- Comment ajouter une page maître de disposition pour configurer un modèle pour le contenu HTML commun, une feuille de style pour améliorer l’apparence et un modèle de vue pour afficher le contenu HTML
- Comment utiliser le modèle ViewModel pour passer des propriétés pour le modèle de vue pour afficher des informations dynamiques
- Comment utiliser les paramètres passés aux contrôleurs dans le modèle de vue
- Comment ajouter des liens vers des pages à l’intérieur de l’application ASP.NET MVC
- Comment ajouter et utiliser des propriétés dynamiques dans une vue
- Les améliorations dans les modèles de projet ASP.NET MVC 4

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Annexe a : installation de Visual Studio Express 2012 pour le Web

Vous pouvez installer **Microsoft Visual Studio Express 2012 pour Web** ou un autre &quot;Express&quot; à l’aide de la version du **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Les instructions suivantes vous guident dans les étapes requises pour installer *Visual studio Express 2012 pour Web* à l’aide de *Microsoft Web Platform Installer*.

1. Accédez à [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Si vous avez déjà installé Web Platform Installer, vous pouvez également ouvrir il et recherchez le produit &quot; <em>Visual Studio Express 2012 pour le Web avec Windows Azure SDK</em>&quot;.
2. Cliquez sur **installer maintenant**. Si vous n’avez pas **Web Platform Installer** vous allez être redirigé pour télécharger et installer en premier.
3. Une fois **Web Platform Installer** est ouvert, cliquez sur **installer** pour démarrer le programme d’installation.

    ![Installer Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "installer Visual Studio Express")

    *Installer Visual Studio Express*
4. Lisez les licences et les termes du contrat de tous les produits et cliquez sur **J’accepte** pour continuer.

    ![Accepter les termes du contrat de licence](aspnet-mvc-4-fundamentals/_static/image44.png)

    *Accepter les termes du contrat de licence*
5. Attendez que le processus de téléchargement et l’installation se termine.

    ![Progression de l'installation](aspnet-mvc-4-fundamentals/_static/image45.png)

    *Progression de l’installation*
6. Une fois l’installation terminée, cliquez sur **Terminer**.

    ![Installation est terminée](aspnet-mvc-4-fundamentals/_static/image46.png)

    *Installation est terminée*
7. Cliquez sur **Exit** pour fermer Web Platform Installer.
8. Pour ouvrir Visual Studio Express pour le Web, accédez à la **Démarrer** écran et démarrer l’écriture &quot; **VS Express**&quot;, puis cliquez sur le **Visual Studio Express pour le Web** vignette.

    ![VS Express pour une vignette de Web](aspnet-mvc-4-fundamentals/_static/image47.png)

    *VS Express pour une vignette de Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Annexe b : publication d’une Application ASP.NET MVC 4, à l’aide de Web Deploy

Cette annexe sera vous montrent comment créer un nouveau site web à partir du portail de gestion Windows Azure et publiez l’application que vous avez obtenu en suivant le laboratoire, en tirant parti de la fonctionnalité de publication Web Deploy fournie par Windows Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Tâche 1 : création d’un nouveau Site Web à partir de Windows Azure Portal

1. Accédez à la [portail de gestion Windows Azure](https://manage.windowsazure.com/) et connectez-vous en utilisant les informations d’identification Microsoft associées à votre abonnement.

    > [!NOTE]
    > Avec Windows Azure, vous pouvez héberger 10 Sites Web ASP.NET gratuitement et faites ensuite évoluer que votre trafic augmente. Vous pouvez vous inscrire [ici](http://aka.ms/aspnet-hol-azure).

    ![Ouvrez une session sur le portail Windows Azure](aspnet-mvc-4-fundamentals/_static/image48.png "ouvrez une session sur le portail Windows Azure")

    *Ouvrez une session sur le portail de gestion Azure Windows*
2. Cliquez sur **New** sur la barre de commandes.

    ![Création d’un Site Web](aspnet-mvc-4-fundamentals/_static/image49.png "création d’un Site Web")

    *Création d’un Site Web*
3. Cliquez sur **calcul** | **Site Web**. Puis sélectionnez **création rapide** option. Fournir une URL disponible pour le nouveau site web et cliquez sur **créer un Site Web**.

    > [!NOTE]
    > Un Site Web Windows Azure est l’hôte pour une application web en cours d’exécution dans le cloud que vous pouvez contrôler et gérer. L’option Création rapide vous permet de déployer une application web terminée pour le Site Web Windows Azure à partir en dehors du portail. Il n’inclut pas les étapes de configuration d’une base de données.

    ![Création d’un Site Web à l’aide de la création rapide](aspnet-mvc-4-fundamentals/_static/image50.png "création d’un Site Web à l’aide de la création rapide")

    *Création d’un Site Web à l’aide de la création rapide*
4. Attendez que la nouvelle **Site Web** est créé.
5. Une fois que le Site Web est créé, cliquez sur le lien situé sous le **URL** colonne. Vérifiez que le nouveau Site Web fonctionne.

    ![Navigation vers le nouveau site web](aspnet-mvc-4-fundamentals/_static/image51.png "exploration vers le nouveau site web")

    *Navigation vers le nouveau site web*

    ![Site Web en cours d’exécution](aspnet-mvc-4-fundamentals/_static/image52.png "site Web en cours d’exécution")

    *Site Web en cours d’exécution*
6. Revenez au portail et cliquez sur le nom du site web sous le **nom** colonne pour afficher les pages de gestion.

    ![Ouvrir les pages de gestion de site web](aspnet-mvc-4-fundamentals/_static/image53.png "ouvrir les pages de gestion de site web")

    *Ouvrir les pages de gestion de Site Web*
7. Dans le **tableau de bord** page, sous la **aperçu rapide** , cliquez sur le **télécharger le profil de publication** lien.

    > [!NOTE]
    > Le *le profil de publication* contient toutes les informations requises pour publier une application web à un site Web Windows Azure pour chaque méthode de publication est activée. Le profil de publication contient les URL, les informations d’identification utilisateur et les chaînes de base de données requis pour la connexion et l’authentification par rapport à chacun des points de terminaison pour lequel une méthode de publication est activée. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express pour Web** et **Microsoft Visual Studio 2012** prise en charge de la lecture des profils pour automatiser la configuration de ces programmes pour de publication publication d’applications web sur sites Web Windows Azure.

    ![Téléchargement du site web le profil de publication](aspnet-mvc-4-fundamentals/_static/image54.png "téléchargement du site web le profil de publication")

    *Téléchargement du Site Web le profil de publication*
8. Télécharger le fichier de profil de publication dans un emplacement connu. Davantage dans cet exercice, vous verrez comment utiliser ce fichier pour publier une application web à un Sites Web Windows Azure à partir de Visual Studio.

    ![L’enregistrement du fichier de profil de publication](aspnet-mvc-4-fundamentals/_static/image55.png "l’enregistrement du profil de publication")

    *L’enregistrement du fichier de profil de publication*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tâche 2 : configuration du serveur de base de données

Si votre application se sert de SQL Server vous devez créer un serveur de base de données SQL des bases de données. Si vous souhaitez déployer une application simple qui n’utilise pas de SQL Server, vous pouvez ignorer cette tâche.

1. Vous devez un serveur de base de données SQL pour stocker la base de données d’application. Vous pouvez afficher les serveurs de base de données SQL à partir de votre abonnement dans le portail de gestion Windows Azure à l’adresse **bases de données Sql** | **serveurs** | **du serveur Tableau de bord**. Si vous n’avez pas un serveur créé, vous pouvez créer un à l’aide du **ajouter** bouton sur la barre de commandes. Prenez note de la **nom du serveur et les URL, nom de connexion d’administrateur et mot de passe**, car vous les utiliserez dans les tâches suivantes. Ne créez pas encore, la base de données tel qu’il sera créé dans une étape ultérieure.

    ![Tableau de bord de serveur SQL Database](aspnet-mvc-4-fundamentals/_static/image56.png "tableau de bord serveur de base de données SQL")

    *Tableau de bord serveur de base de données SQL*
2. Dans la tâche suivante, vous allez tester la connexion de base de données à partir de Visual Studio, pour cette raison, vous devez inclure votre adresse IP locale dans la liste du serveur de **adresses IP autorisées**. Pour ce faire, cliquez sur **configurer**, sélectionnez l’adresse IP à partir de **adresse IP cliente actuelle** et collez-le sur le **adresse IP de début** et **adresse IP de fin** zones de texte et cliquez sur le ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) bouton.

    ![Ajout d’adresse IP du Client](aspnet-mvc-4-fundamentals/_static/image58.png)

    *Ajout d’adresse IP du Client*
3. Une fois le **adresse IP du Client** est ajouté à le des adresses IP autorisées de liste, cliquez sur **enregistrer** pour confirmer les modifications.

    ![Confirmer les modifications](aspnet-mvc-4-fundamentals/_static/image59.png)

    *Confirmer les modifications*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tâche 3 : publication d’une Application ASP.NET MVC 4, à l’aide de Web Deploy

1. Revenez à la solution ASP.NET MVC 4. Dans le **l’Explorateur de solutions**, cliquez sur le projet de site web et sélectionnez **publier**.

    ![Publication de l’Application](aspnet-mvc-4-fundamentals/_static/image60.png "publication de l’Application")

    *Publier le site web*
2. Importer le profil de publication que vous avez enregistré dans la première tâche.

    ![L’importation du profil de publication](aspnet-mvc-4-fundamentals/_static/image61.png "l’importation du profil de publication")

    *Importation du profil de publication*
3. Cliquez sur **valider la connexion**. Une fois la Validation terminée. Cliquez sur **suivant**.

    > [!NOTE]
    > La validation est terminée une fois que vous voyez une coche verte apparaît en regard du bouton Valider la connexion.

    ![Validation de la connexion](aspnet-mvc-4-fundamentals/_static/image62.png "validation de la connexion")

    *Validation de la connexion*
4. Dans le **paramètres** page, sous la **bases de données** , cliquez sur le bouton en regard de la zone de texte de la connexion de votre base de données (par exemple, **DefaultConnection**).

    ![Configuration de déploiement Web](aspnet-mvc-4-fundamentals/_static/image63.png "configuration de déploiement Web")

    *Configuration de déploiement Web*
5. Configurez la connexion de base de données comme suit :

   - Dans le **nom du serveur** tapez votre URL de base de données SQL server à l’aide du *tcp :* préfixe.
   - Dans **nom d’utilisateur** tapez votre nom de connexion d’administrateur serveur.
   - Dans **mot de passe** tapez votre mot de passe du compte de connexion administrateur serveur.
   - Tapez un nouveau nom de base de données, par exemple : *MVC4SampleDB*.

     ![Configuration de chaîne de connexion de destination](aspnet-mvc-4-fundamentals/_static/image64.png "configuration de chaîne de connexion de destination")

     *Configuration de chaîne de connexion de destination*
6. Cliquez ensuite sur **OK**. Lorsque vous êtes invité à créer la base de données, cliquez sur **Oui**.

    ![Création de la base de données](aspnet-mvc-4-fundamentals/_static/image65.png "création de la chaîne de la base de données")

    *Création de la base de données*
7. La chaîne de connexion que vous utiliserez pour vous connecter à la base de données SQL dans Windows Azure est indiquée dans la zone de texte de la connexion par défaut. Cliquez ensuite sur **Suivant**.

    ![Chaîne de connexion pointant vers la base de données SQL](aspnet-mvc-4-fundamentals/_static/image66.png "chaîne de connexion pointant vers la base de données SQL")

    *Chaîne de connexion pointant vers la base de données SQL*
8. Dans le **aperçu** , cliquez sur **publier**.

    ![Publication de l’application web](aspnet-mvc-4-fundamentals/_static/image67.png "publication de l’application web")

    *Publication de l’application web*
9. Une fois le processus de publication terminé, votre navigateur par défaut s’ouvre le site web publié.

    ![Application publiée dans Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Application publiée dans Windows Azure")

    *Application soit publiée dans Windows Azure*

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Annexe c : l’utilisation des extraits de Code

Avec des extraits de code, vous avez tout le code que vous avez besoin à portée de main. Le document de laboratoire vous indiquera exactement quand vous pouvez les utiliser, comme indiqué dans l’illustration suivante.

![À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet](aspnet-mvc-4-fundamentals/_static/image69.png "extraits de code à l’aide de Visual Studio pour insérer du code dans votre projet")

*À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet*

***Pour ajouter un extrait de code à l’aide du clavier (c# uniquement)***

1. Placez le curseur où vous souhaitez insérer le code.
2. Commencez à taper le nom de l’extrait de code (sans espaces ou des traits d’union).
3. Regarder en tant qu’IntelliSense affiche les noms des extraits correspondants.
4. Sélectionnez l’extrait de code correct (ou continuez à taper jusqu'à ce que le nom de l’extrait de code entière est sélectionnée).
5. Appuyez sur la touche Tab à deux reprises pour insérer l’extrait de code à l’emplacement du curseur.

![Commencez à taper le nom de l’extrait de code](aspnet-mvc-4-fundamentals/_static/image70.png "commencez à taper le nom de l’extrait de code")

*Commencez à taper le nom de l’extrait de code*

![Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance](aspnet-mvc-4-fundamentals/_static/image71.png "appuyez sur Tab pour sélectionner l’extrait de code en surbrillance")

*Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance*

![Appuyez sur Tab à nouveau et l’extrait de code seront développe](aspnet-mvc-4-fundamentals/_static/image72.png "appuyez sur Tab à nouveau et l’extrait de code seront développe.")

*Appuyez sur Tab à nouveau et l’extrait de code seront développe.*

***Pour ajouter un extrait de code à l’aide de la souris (c#, Visual Basic et XML)*** 1. Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code.

1. Sélectionnez **insérer un extrait** suivie **mes extraits de Code**.
2. Choisissez l’extrait de code approprié dans la liste, en cliquant dessus.

![Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait](aspnet-mvc-4-fundamentals/_static/image73.png "avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait")

*Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait*

![Choisissez l’extrait de code approprié dans la liste, en cliquant dessus](aspnet-mvc-4-fundamentals/_static/image74.png "choisir l’extrait de code approprié dans la liste, en cliquant sur celle-ci")

*Choisissez l’extrait de code approprié dans la liste, en cliquant sur celle-ci*
