---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: Génération de modèles automatique ASP.NET MVC 4 Entity Framework et les Migrations | Microsoft Docs
author: rick-anderson
description: Si vous êtes familiarisé avec les méthodes de contrôleur ASP.NET MVC 4, ou s’est terminé le &quot;Helpers, formulaires et la Validation&quot; atelier pratique, vous devez être conscient...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 31f593f294c4865f621a8556cb43d0d9c42f2660
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814116"
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a>Migrations et la génération de modèles automatique ASP.NET MVC 4 Entity Framework

par [Web Camps Team](https://twitter.com/webcamps)

[Télécharger le Kit de formation de Web Camps](https://aka.ms/webcamps-training-kit)

Si vous êtes familiarisé avec les méthodes de contrôleur ASP.NET MVC 4, ou s’est terminé le &quot;Helpers, formulaires et la Validation&quot; atelier pratique, vous devez être conscient que la plupart de la logique permettant de créer, mettre à jour, répertorier et supprimer une entité de données qu’il est répété entre l’application. Ne pas de mentionner que, si votre modèle comporte plusieurs classes à manipuler, vous serez susceptible de prendre une écriture les méthodes d’action POST et GET pour chaque opération de l’entité, ainsi que chacune des vues.

Dans cet atelier, vous allez apprendre à utiliser la génération de modèles automatique ASP.NET MVC 4 pour générer automatiquement la ligne de base CRUD de votre application (Create, Read, Update et Delete). À partir d’une classe de modèle simple et sans écrire une seule ligne de code, vous allez créer un contrôleur qui contiendra toutes les opérations CRUD, ainsi que toutes les vues nécessaires. Après la génération et exécution de la solution simple, vous disposez de la base de données de l’application générée, ainsi que la logique MVC et les vues pour la manipulation des données.

En outre, vous allez apprendre combien il est facile à utiliser des Migrations Entity Framework pour effectuer des mises à jour du modèle dans toute votre application. Migrations Entity Framework vous permet de modifier votre base de données une fois que le modèle a changé avec étapes simples. Avec tous ces cas à l’esprit, vous serez en mesure de créer et maintenir des applications web plus efficacement, en tirant parti des dernières fonctionnalités d’ASP.NET MVC 4.

> [!NOTE]
> Tous les exemples de code et extraits de code sont inclus dans le Kit de formation Camps Web, disponible à partir d’à l’adresse [les versions de Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Le projet spécifique à ce laboratoire est disponible à l’adresse [génération de modèles automatique ASP.NET MVC 4 Entity Framework et les Migrations](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).

<a id="Objectives"></a>
### <a name="objectives"></a>Objectifs

Dans cet atelier pratique, vous allez apprendre comment :

- Utiliser génération de modèles automatique ASP.NET pour les opérations CRUD dans les contrôleurs.
- Modifier le modèle de base de données à l’aide des Migrations Entity Framework.

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prérequis

Vous devez disposer des éléments suivants pour terminer ce laboratoire :

- [Microsoft Visual Studio Express 2012 pour Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou supérieure (lire [annexe A](#AppendixA) pour obtenir des instructions sur la façon d’installer).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Installation

**L’installation des extraits de Code**

Pour des raisons pratiques, une grande partie du code que vous gérez le long de ce laboratoire est disponible en tant que les extraits de code Visual Studio. Pour installer les extraits de code exécuter **.\Source\Setup\CodeSnippets.vsi** fichier.

Si vous n’êtes pas familiarisé avec les extraits de Code Visual Studio et que vous souhaitiez savoir comment les utiliser, vous pouvez faire référence à l’annexe de ce document &quot; [extraits de Code à l’aide d’annexe b :](#AppendixB)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exercices

L’exercice suivant composent cet atelier pratique sur :

1. [À l’aide de la génération de modèles automatique ASP.NET MVC 4 avec des Migrations Entity Framework](#Exercise1)

> [!NOTE]
> Cet exercice est accompagné par un **fin** dossier contenant la solution obtenue, vous devez obtenir à l’issue de l’exercice. Si vous avez besoin d’aide supplémentaire, utilisation de cet exercice, vous pouvez utiliser cette solution comme guide.


Durée estimée pour effectuer ce laboratoire : **30 minutes**

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a>Exercice 1 : À l’aide de la structure ASP.NET MVC 4 avec des Migrations Entity Framework

Génération de modèles automatique ASP.NET MVC offre un moyen rapide pour générer les opérations CRUD dans une méthode standardisée, création de la logique nécessaire qui permet à votre application d’interagir avec la couche de base de données.

Dans cet exercice, vous allez apprendre à utiliser la structure ASP.NET MVC 4 avec le code en premier pour créer les méthodes CRUD. Ensuite, vous allez apprendre à mettre à jour votre modèle en appliquant les modifications dans la base de données à l’aide des Migrations Entity Framework.

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a>Tâche 1-Création un ASP.NET MVC 4 nouveau projet à l’aide de la génération de modèles automatique

1. Si elle est déjà ouverte, démarrez **Visual Studio 2012**.
2. Sélectionnez **fichier | Nouveau projet**. Dans la nouvelle boîte de dialogue projet, sous la **Visual C# | Web** section, sélectionnez **ASP.NET MVC 4 Web Application**. Nommez le projet à **MVC4andEFMigrations** et définissez l’emplacement sur **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** dossier de ce laboratoire. Définir le **nom de la Solution** à **commencer** et vérifiez **créer le répertoire pour la solution** est activée. Cliquez sur **OK**.

    ![Nouvelle boîte de dialogue projet ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "nouvelle boîte de dialogue projet ASP.NET MVC 4")

    *Nouvelle boîte de dialogue projet ASP.NET MVC 4*
3. Dans le **nouveau projet ASP.NET MVC 4** boîte de dialogue Sélectionnez le **Application Internet** modèle et vous assurer que **Razor** est sélectionné **moteur d’affichage**. Cliquez sur **OK** pour créer le projet.

    ![Nouvelle Application Internet de ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "nouvelle Application Internet de ASP.NET MVC 4")

    *Nouvelle Application Internet de ASP.NET MVC 4*
4. Dans l’Explorateur de solutions, cliquez sur **modèles** et sélectionnez **ajouter | Classe** pour créer une personne de classe simple (POCO). Nommez-le **personne** et cliquez sur **OK**.
5. Ouvrez la classe Person et insérer les propriétés suivantes.

    (Code Snippet - *ASP.NET MVC 4 et Migrations Entity Framework - propriétés de personne Ex1*)

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. Cliquez sur **générer | Générer la Solution** pour enregistrer les modifications et générez le projet.

    ![Génération de l’application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "génération de l’application")

    *Génération de l’application*
7. Dans l’Explorateur de solutions, cliquez sur le dossier contrôleurs, puis sélectionnez **ajouter | Contrôleur**.
8. Nommez le contrôleur *PersonController* et terminez le **les options de génération de modèles automatique** avec les valeurs suivantes.

   1. Dans le **modèle** liste déroulante, sélectionnez le **contrôleur MVC avec des actions de lecture/écriture et de vues, utilisant Entity Framework** option.
   2. Dans le **classe de modèle** liste déroulante, sélectionnez le **personne** classe.
   3. Dans le **classe de contexte de données** liste, sélectionnez  **&lt;nouveau contexte de données... &gt;**. Choisissez n’importe quel nom et cliquez sur **OK**.
   4. Dans le **vues** déroulante liste, assurez-vous que l’option **Razor** est sélectionné.

      ![Ajout du contrôleur de la personne avec la structure](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Ajout du contrôleur de la personne avec la génération de modèles automatique")

      *Ajout du contrôleur de la personne avec la génération de modèles automatique*
9. Cliquez sur **ajouter** pour créer le nouveau contrôleur de personne avec la génération de modèles automatique. Vous avez généré les actions de contrôleur, ainsi que les vues.

    ![Après avoir créé le contrôleur de la personne avec la structure](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "après avoir créé le contrôleur de la personne avec la structure")

    *Après avoir créé le contrôleur de la personne avec la structure*
10. Ouvrez **PersonController** classe. Notez que les méthodes d’action CRUD complètes ont été générées automatiquement.

   ![À l’intérieur du contrôleur de personne](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "contrôleur d’à l’intérieur de la personne")

   *À l’intérieur du contrôleur de personne*

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a>Tâche 2-en cours d’exécution l’application

À ce stade, la base de données n’est pas encore créé. Dans cette tâche, vous exécutez l’application pour la première fois et tester les opérations CRUD. La base de données est créé à la volée avec Code First.

1. Appuyez sur **F5** pour exécuter l’application.
2. Dans le navigateur, ajoutez **/Person** à l’URL pour ouvrir la page de la personne.

    ![Tout d’abord l’exécution de l’application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "tout d’abord l’exécution de l’Application")

    *L’application : exécutez d’abord*
3. Maintenant vous parcourez les pages de personne et tester les opérations CRUD.

    1. Cliquez sur **créer un nouveau** pour ajouter une nouvelle personne. Entrez un prénom et nom de famille et cliquez sur **créer**.

        ![Ajout d’une nouvelle personne](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Ajout d’une nouvelle personne")

        *Ajout d’une nouvelle personne*
    2. Dans la liste de la personne, vous pouvez supprimer, modifier ou ajouter des éléments.

        ![liste de personnes](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "liste de personnes")

        *Liste de personnes*
    3. Cliquez sur **détails** pour ouvrir les détails de la personne.

        ![Détails de la personne](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "les détails de la personne")

        *Détails de la personne*
4. Fermez le navigateur et revenir à Visual Studio. Notez que vous avez créé le CRUD entière pour l’entité person dans toute votre application - à partir du modèle pour les vues, sans avoir à écrire une seule ligne de code !

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a>3 de tâche-mise à jour la base de données à l’aide des Migrations Entity Framework

Dans cette tâche, vous mettrez à jour la base de données à l’aide des Migrations Entity Framework. Vous allez découvrir combien il est facile à modifier le modèle et de refléter les modifications dans vos bases de données à l’aide de la fonctionnalité de Migrations Entity Framework.

1. Ouvrez la Console du Gestionnaire de Package. Sélectionnez **outils | Gestionnaire de Package de bibliothèque | Console du Gestionnaire de package**.
2. Dans la Console du Gestionnaire de Package, entrez la commande suivante :

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    ![L’activation de Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "autorisant la migration")

    *L’activation des migrations*

    La commande d’activation de la Migration crée le **Migrations** dossier qui contient un script pour initialiser la base de données.

    ![Dossier migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "dossier Migrations")

    *Cette suite de ce didacticiel vous montrera comment le faire dans SSMS, mais si vous ne souhaitez pas installer et utiliser SSMS vous pouvez obtenir les scripts à partir de la version complète du projet et passer à la section où les stocker dans le dossier de solution.*
3. Ouvrez le **Configuration.cs** fichier dans le dossier Migrations. Recherchez le constructeur de classe et modifiez le **AutomaticMigrationsEnabled** valeur *true*.

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. Ouvrez la classe Person et ajoutez un attribut pour le deuxième prénom de la personne. Avec ce nouvel attribut, vous modifiez le modèle.

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. Sélectionnez **générer | Générer la Solution** dans le menu pour générer l’application.

    ![Génération de l’application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "génération de l’application")

    *Génération de l’application*
6. Dans la Console du Gestionnaire de Package, entrez la commande suivante :

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    Cette commande recherche les modifications dans les objets de données, et ensuite, il ajoute les commandes nécessaires pour modifier la base de données en conséquence.

    ![Ajout d’un deuxième prénom](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Ajout d’un deuxième prénom")

    *Ajout d’un deuxième prénom*
7. (Facultatif) Vous pouvez exécuter la commande suivante pour générer un script SQL avec la mise à jour différentiel. Ainsi, vous pouvez mettre à jour manuellement de la base de données (dans ce cas il n’est pas nécessaire), ou appliquer les modifications dans les autres bases de données :

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    ![Génération d’un script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "génération d’un script SQL")

    *Génération d’un script SQL*

    ![Mise à jour du Script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "mise à jour du Script SQL")

    *Mise à jour du Script SQL*
8. Dans la Console du Gestionnaire de Package, entrez la commande suivante pour mettre à jour de la base de données :

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    ![La mise à jour de la base de données](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "mise à jour de la base de données")

    *La mise à jour de la base de données*

    Cela ajoutera le **MiddleName** colonne dans le **personnes** table pour correspondre à la définition actuelle de la **personne** classe.
9. Une fois que la base de données est mis à jour, cliquez sur le dossier Controller et sélectionnez **ajouter | Contrôleur** pour ajouter le contrôleur de personne (complète avec les mêmes valeurs). Cela met à jour les méthodes existantes et les vues Ajout du nouvel attribut.

    ![Ajout d’une mise à jour du contrôleur](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Ajout d’une mise à jour du contrôleur")

    *La mise à jour le contrôleur*
10. Cliquez sur **Ajouter**. Ensuite, sélectionnez les valeurs **PersonController.cs remplacer** et **remplacer les vues associées** et cliquez sur **OK**.

   ![Ajout d’un remplacement de contrôleur](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   *La mise à jour le contrôleur*

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a>Task4 - exécution de l’application

1. Appuyez sur **F5** pour exécuter l’application.
2. Ouvrez **/Person**. Notez que les données ont été préservées, tandis que la colonne Prénom a été ajoutée.

    ![Deuxième prénom ajouté](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "prénom ajouté")

    *Deuxième prénom ajouté*
3. Si vous cliquez sur **modifier**, vous serez en mesure d’ajouter un deuxième prénom à la personne actuelle.

    ![Deuxième prénom édition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "édition du deuxième prénom")

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Récapitulatif

Dans cet atelier, vous avez appris les étapes simples pour créer des opérations CRUD avec ASP.NET MVC 4 génération de modèles automatique à l’aide de n’importe quelle classe de modèle. Ensuite, vous avez appris à effectuer une mise à jour de bout en bout dans votre application - à partir de la base de données aux vues - à l’aide de Migrations Entity Framework.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Annexe a : installation de Visual Studio Express 2012 pour le Web

Vous pouvez installer **Microsoft Visual Studio Express 2012 pour Web** ou un autre &quot;Express&quot; à l’aide de la version du **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Les instructions suivantes vous guident dans les étapes requises pour installer *Visual studio Express 2012 pour Web* à l’aide de *Microsoft Web Platform Installer*.

1. Accédez à [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Si vous avez déjà installé Web Platform Installer, vous pouvez également ouvrir il et recherchez le produit &quot; <em>Visual Studio Express 2012 pour le Web avec Windows Azure SDK</em>&quot;.
2. Cliquez sur **installer maintenant**. Si vous n’avez pas **Web Platform Installer** vous allez être redirigé pour télécharger et installer en premier.
3. Une fois **Web Platform Installer** est ouvert, cliquez sur **installer** pour démarrer le programme d’installation.

    ![Installer Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "installer Visual Studio Express")

    *Installer Visual Studio Express*
4. Lisez les licences et les termes du contrat de tous les produits et cliquez sur **J’accepte** pour continuer.

    ![Accepter les termes du contrat de licence](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    *Accepter les termes du contrat de licence*
5. Attendez que le processus de téléchargement et l’installation se termine.

    ![Progression de l'installation](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    *Progression de l’installation*
6. Une fois l’installation terminée, cliquez sur **Terminer**.

    ![Installation est terminée](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    *Installation est terminée*
7. Cliquez sur **Exit** pour fermer Web Platform Installer.
8. Pour ouvrir Visual Studio Express pour le Web, accédez à la **Démarrer** écran et démarrer l’écriture &quot; **VS Express**&quot;, puis cliquez sur le **Visual Studio Express pour le Web** vignette.

    ![VS Express pour une vignette de Web](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    *VS Express pour une vignette de Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Annexe b : utilisation d’extraits de Code

Avec des extraits de code, vous avez tout le code que vous avez besoin à portée de main. Le document de laboratoire vous indiquera exactement quand vous pouvez les utiliser, comme indiqué dans l’illustration suivante.

![À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "extraits de code à l’aide de Visual Studio pour insérer du code dans votre projet")

*À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet*

***Pour ajouter un extrait de code à l’aide du clavier (c# uniquement)***

1. Placez le curseur où vous souhaitez insérer le code.
2. Commencez à taper le nom de l’extrait de code (sans espaces ou des traits d’union).
3. Regarder en tant qu’IntelliSense affiche les noms des extraits correspondants.
4. Sélectionnez l’extrait de code correct (ou continuez à taper jusqu'à ce que le nom de l’extrait de code entière est sélectionnée).
5. Appuyez sur la touche Tab à deux reprises pour insérer l’extrait de code à l’emplacement du curseur.

![Commencez à taper le nom de l’extrait de code](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "commencez à taper le nom de l’extrait de code")

*Commencez à taper le nom de l’extrait de code*

![Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "appuyez sur Tab pour sélectionner l’extrait de code en surbrillance")

*Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance*

![Appuyez sur Tab à nouveau et l’extrait de code seront développe](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "appuyez sur Tab à nouveau et l’extrait de code seront développe.")

*Appuyez sur Tab à nouveau et l’extrait de code seront développe.*

***Pour ajouter un extrait de code à l’aide de la souris (c#, Visual Basic et XML)*** 1. Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code.

1. Sélectionnez **insérer un extrait** suivie **mes extraits de Code**.
2. Choisissez l’extrait de code approprié dans la liste, en cliquant dessus.

![Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait")

*Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait*

![Choisissez l’extrait de code approprié dans la liste, en cliquant dessus](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "choisir l’extrait de code approprié dans la liste, en cliquant sur celle-ci")

*Choisissez l’extrait de code approprié dans la liste, en cliquant sur celle-ci*
