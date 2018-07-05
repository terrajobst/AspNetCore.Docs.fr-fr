---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: Accès aux données et les modèles ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: 'Remarque : Ce laboratoire pratique suppose que vous avez une connaissance élémentaire d’ASP.NET MVC. Si vous n’avez pas utilisé ASP.NET MVC avant, nous vous recommandons de consulter sur ASP.NET MVC 4...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: afc03d87431632bbb3ab59241de0edb4bb7af12d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371127"
---
# <a name="aspnet-mvc-4-models-and-data-access"></a>Accès aux données et les modèles ASP.NET MVC 4

par [Web Camps Team](https://twitter.com/webcamps)

[Télécharger le Kit de formation de Web Camps](https://aka.ms/webcamps-training-kit)

Ce laboratoire pratique suppose que vous avez une connaissance élémentaire de **ASP.NET MVC**. Si vous n’avez pas utilisé **ASP.NET MVC** auparavant, nous vous recommandons de consulter **principes de base ASP.NET MVC 4** les ateliers pratiques.

Ce laboratoire vous guide les améliorations et les nouvelles fonctionnalités décrites précédemment en appliquant des modifications mineures à un exemple d’application Web dans le dossier Source.

> [!NOTE]
> Tous les exemples de code et extraits de code sont inclus dans le Kit de formation Camps Web, disponible à l’adresse [les versions de Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Le projet spécifique à ce laboratoire est disponible à l’adresse [les modèles ASP.NET MVC 4 et Data Access](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).

Dans **principes de base ASP.NET MVC** les ateliers pratiques, vous avez été passant codées en dur les données à partir des contrôleurs pour les modèles de vue. Toutefois, pour générer une application Web réelle, vous pouvez souhaiter utiliser une base de données réel.

Cet atelier pratique sur sera vous montrent comment utiliser un moteur de base de données afin de stocker et récupérer les données nécessaires pour l’application de Store de musique. Pour cela, vous démarrez avec une base de données existante, créer l’Entity Data Model à partir de celui-ci. Tout au long de cet atelier, vous répondra à la **Database First** approche mais aussi le **Code First** approche.

Toutefois, vous pouvez également utiliser le **Model First** approche, créer le même modèle à l’aide des outils, puis générer la base de données à partir de celui-ci.

![Vs première base de données. Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "vs première base de données. Tout d’abord de modèle")

*Vs première base de données. Tout d’abord de modèle*

Après avoir généré le modèle, vous effectuerez les ajustements appropriés dans le StoreController pour fournir les vues Store avec les données extraites de la base de données, au lieu d’utiliser des données codées en dur. Il est inutile d’apporter de modification pour les modèles de vue, car le StoreController retournera les ViewModels mêmes pour les modèles de vue, bien que cette fois les données proviendront de la base de données.

**L’approche Code First**

L’approche Code First permet de définir le modèle à partir du code sans générer des classes qui sont généralement couplés avec l’infrastructure.

Dans le code tout d’abord, les objets de modèle sont définies avec Oct, &quot;Plain Old CLR Objects&quot;. Oct est simples classes plain aucun héritage et n’implémentent pas d’interfaces. Nous pouvons générer automatiquement la base de données à partir de celles-ci, ou nous pouvons utiliser une base de données existante et générer le mappage de classe à partir du code.

Les avantages de cette approche est que le modèle reste indépendant de l’infrastructure de persistance (dans ce cas, Entity Framework), comme les classes oct ne sont pas associées à l’infrastructure de mappage.

> [!NOTE]
> Ce laboratoire est basé sur ASP.NET MVC 4 et d’une version de l’exemple d’application Music Store personnalisée et réduite pour s’ajuster à uniquement les fonctionnalités présentées dans ce laboratoire pratique.
> 
> Si vous souhaitez Explorer la totalité **Music Store** application didacticiel, vous pouvez le trouver dans [MVC-musique-Store](https://github.com/evilDave/MVC-Music-Store).


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

Si vous n’êtes pas familiarisé avec les extraits de Code Visual Studio et que vous souhaitiez savoir comment les utiliser, vous pouvez faire référence à l’annexe de ce document &quot; [extraits de Code à l’aide d’annexe c :](#AppendixC)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exercices

Ce laboratoire pratique est constitué par les exercices suivants :

1. [Exercice 1 : Ajout d’une base de données](#Exercise1)
2. [Exercice 2 : Création d’une base de données à l’aide de Code First](#Exercise2)
3. [Exercice 3 : Interrogation de la base de données avec des paramètres](#Exercise3)

> [!NOTE]
> Chaque exercice est accompagné par un **fin** dossier contenant la solution obtenue, vous devez obtenir après avoir effectué les exercices. Si vous avez besoin d’aide supplémentaire sur l’utilisation via les exercices, vous pouvez utiliser cette solution comme guide.


Durée estimée pour effectuer ce laboratoire : **35 minutes**.

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a>Exercice 1 : Ajout d’une base de données

Dans cet exercice, vous allez apprendre à ajouter une base de données avec les tables de l’application du Store musique à la solution pour utiliser ses données. Une fois la base de données est généré avec le modèle et ajouté à la solution, vous allez modifier la classe StoreController pour fournir le modèle de vue avec les données extraites de la base de données, au lieu d’utiliser des valeurs codées en dur.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a>Tâche 1 : ajout d’une base de données

Dans cette tâche, vous allez ajouter une base de données déjà créée avec les principales tables de l’application du Store musique à la solution.

1. Ouvrez le **commencer** solution situé dans **/Ex1-AddingADatabaseDBFirst/début du fichierSource/** dossier.

   1. Vous devrez télécharger certains packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.
   2. Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.

      > [!NOTE]
      > Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
2. Ajouter **MvcMusicStore** fichier de base de données. Dans cet atelier pratique, vous allez utiliser une base de données déjà créée appelée **MvcMusicStore.mdf**. Pour ce faire, cliquez sur **application\_données** dossier, pointez sur **ajouter** puis cliquez sur **élément existant**. Accédez à **\Source\Assets** et sélectionnez le **MvcMusicStore.mdf** fichier.

    ![Ajout d’un élément existant](aspnet-mvc-4-models-and-data-access/_static/image2.png "Ajout d’un élément existant")

    *Ajout d’un élément existant*

    ![Fichier de base de données MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image3.png "fichier de base de données MvcMusicStore.mdf")

    *Fichier de base de données MvcMusicStore.mdf*

    La base de données a été ajoutée au projet. Même lorsque la base de données se trouve à l’intérieur de la solution, vous pouvez interroger et mettre à jour comme il était hébergée dans un serveur de base de données différente.

    ![MvcMusicStore de base de données dans l’Explorateur de solutions](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore de base de données dans l’Explorateur de solutions")

    *MvcMusicStore de base de données dans l’Explorateur de solutions*
3. Vérifier la connexion à la base de données. Pour ce faire, double-cliquez sur **MvcMusicStore.mdf** pour établir une connexion.

    ![Connexion à MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "connexion à MvcMusicStore.mdf")

    *Connexion à MvcMusicStore.mdf*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a>Tâche 2 : création d’un modèle de données

Dans cette tâche, vous allez créer un modèle de données pour interagir avec la base de données ajouté dans la tâche précédente.

1. Créer un modèle de données qui représente la base de données. Pour ce faire, dans le droit de l’Explorateur de solutions le **modèles** dossier, pointez sur **ajouter** puis cliquez sur **un nouvel élément**. Dans le **ajouter un nouvel élément** boîte de dialogue, sélectionnez le **données** modèle, puis le **ADO.NET Entity Data Model** élément. Remplacez le nom de modèle de données par **StoreDB.edmx** et cliquez sur **ajouter**.

    ![Ajout de StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "Ajout StoreDB ADO.NET Entity Data Model")

    *Ajout de StoreDB ADO.NET Entity Data Model*
2. Le **Assistant Entity Data Model** s’affiche. Cet Assistant va vous guider tout au long de la création de la couche de modèle. Étant donné que le modèle doit être créé en fonction de la recentyl existant de base de données ajouté, sélectionnez **générer à partir de la base de données** et cliquez sur **suivant**.

    ![Choisir le contenu du modèle](aspnet-mvc-4-models-and-data-access/_static/image7.png "en choisissant le contenu du modèle")

    *Choisir le contenu du modèle*
3. Étant donné que vous générez un modèle à partir d’une base de données, vous devez spécifier la connexion à utiliser. Cliquez sur **nouvelle connexion**.
4. Sélectionnez **fichier de base de données Microsoft SQL Server** et cliquez sur **continuer**.

    ![Choisir les sources de données](aspnet-mvc-4-models-and-data-access/_static/image8.png "choisir les sources de données")

    *Choisissez la boîte de dialogue données source*
5. Cliquez sur **Parcourir** et sélectionnez la base de données **MvcMusicStore.mdf** vous avez localisé à le **application\_données** dossier puis cliquez sur **OK**.

    ![Propriétés de connexion](aspnet-mvc-4-models-and-data-access/_static/image9.png "propriétés de connexion")

    *Propriétés de connexion*
6. La classe générée doit avoir le même nom que la chaîne de connexion d’entité, par conséquent, remplacez son nom par **MusicStoreEntities** et cliquez sur **suivant**.

    ![Choix de la connexion de données](aspnet-mvc-4-models-and-data-access/_static/image10.png "choix de la connexion de données")

    *Choix de la connexion de données*
7. Choisissez les objets de base de données à utiliser. À mesure que le modèle d’entité utilisera uniquement les tables de la base de données, sélectionnez le **Tables** option et vous assurer que le **inclure les colonnes clés étrangères dans le modèle** et **mettre au pluriel ou au singulier généré noms d’objets** options sont également sélectionnées. Modifier le modèle Namespace pour **MvcMusicStore.Model** et cliquez sur **Terminer**.

    ![Choisir les objets de base de données](aspnet-mvc-4-models-and-data-access/_static/image11.png "en choisissant les objets de base de données")

    *Choisir les objets de base de données*

    > [!NOTE]
    > Si une boîte de dialogue Avertissement de sécurité s’affiche, cliquez sur **OK** pour exécuter le modèle et générer les classes pour les entités de modèle.
8. Un diagramme d’entité pour la base de données s’affiche pendant la création d’une classe séparée qui mappe chaque table à la base de données. Par exemple, le **Albums** table est représentée par un **Album** (classe), où chaque colonne dans la table mappe à une propriété de classe. Cela vous permettra d’interroger et de travailler avec des objets qui représentent des lignes dans la base de données.

    ![Diagramme de l’entité](aspnet-mvc-4-models-and-data-access/_static/image12.png "diagramme de l’entité")

    *Diagramme de l’entité*

    > [!NOTE]
    > Les modèles T4 (.tt) exécuter du code pour générer les classes d’entités et remplaceront les classes existantes portant le même nom. Dans cet exemple, les classes &quot;Album&quot;, &quot;Genre&quot; et &quot;artiste&quot; ont été remplacés par le code généré.


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a>Tâche 3 : génération de l’Application

Dans cette tâche, vous allez vérifier que, bien que la génération du modèle a supprimé le **Album**, **Genre** et **artiste** classes de modèle, le projet correctement généré à l’aide de les nouvelles classes de modèle de données.

1. Générez le projet en sélectionnant le **Build** élément de menu, puis **MvcMusicStore Build**.

    ![Génération du projet](aspnet-mvc-4-models-and-data-access/_static/image13.png "génération du projet")

    *Génération du projet*
2. Le projet est généré avec succès. Pourquoi toujours fonctionne-t-il ? Cela fonctionne parce que les tables de base de données ont des champs qui incluent les propriétés que vous utilisiez dans les classes supprimés **Album** et **Genre**.

    ![Réussi des builds](aspnet-mvc-4-models-and-data-access/_static/image14.png "Builds a réussi")

    *Builds réussies*
3. Tandis que le concepteur affiche les entités sous forme de diagramme, qu’ils sont véritablement de classes c#. Développez le **StoreDB.edmx** nœud dans l’Explorateur de solutions, puis **StoreDB.tt**, vous verrez les nouvelles entités générées.

    ![Les fichiers générés](aspnet-mvc-4-models-and-data-access/_static/image15.png "les fichiers générés")

    *Fichiers générés*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Tâche 4 : interrogation de la base de données

Dans cette tâche, vous mettrez à jour la classe StoreController afin que, au lieu d’utiliser les données codées en dur, il interrogera la base de données pour extraire les informations.

1. Ouvrez **Controllers\StoreController.cs** et ajoutez le champ suivant à la classe pour contenir une instance de la **MusicStoreEntities** classe, nommée **storeDB**:

    (Code Snippet - *modèles et accès aux données - Ex1 storeDB*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. Le **MusicStoreEntities** classe expose une propriété de collection pour chaque table dans la base de données. Mise à jour **Parcourir** méthode d’action pour récupérer un Genre avec toutes les **Albums**.

    (Code Snippet - *modèles et accès aux données - Ex1 Store Parcourir*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > Vous utilisez une fonctionnalité de .NET appelée **LINQ** (language integrated query) pour écrire des expressions de requête fortement typée par rapport à ces collections - qui exécutera le code par rapport à la base de données et retourner des objets que vous pouvez programmer par rapport aux.
    > 
    > Pour plus d’informations sur LINQ, visitez le [site msdn](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).
3. Mise à jour **Index** méthode d’action pour récupérer tous les genres.

    (Code Snippet - *modèles et accès aux données - Ex1 Store Index*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. Mise à jour **Index** méthode d’action pour récupérer tous les genres et de transformer la collection à une liste.

    (Code Snippet - *modèles et accès aux données - Ex1 Store GenreMenu*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tâche 5 : exécution de l’Application

Dans cette tâche, vous allez vérifier que la page d’Index de Store s’affichent désormais les Genres stockées dans la base de données au lieu de celles codées. Il est inutile de modifier le modèle de vue, car le **StoreController** renvoie les mêmes entités comme auparavant, bien que cette fois les données proviendront de la base de données.

1. Régénérer la solution et appuyez sur **F5** pour exécuter l’Application.
2. Le projet démarre dans la page d’accueil. Vérifiez que le menu de **Genres** n’est plus une liste codée en dur, et les données sont récupérées directement à partir de la base de données.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    *Genres de navigation à partir de la base de données*
3. Maintenant, accédez à n’importe quel genre et vérifier que les albums sont remplis à partir de la base de données.

    ![Exploration des Albums à partir de la base de données](aspnet-mvc-4-models-and-data-access/_static/image17.png "Albums de navigation à partir de la base de données")

    *Exploration des Albums à partir de la base de données*

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a>Exercice 2 : Création d’une base de données à l’aide de Code tout d’abord

Dans cet exercice, vous allez apprendre comment utiliser l’approche Code First pour créer une base de données avec les tables de l’application du Store musique et comment accéder à ses données.

Une fois que le modèle est généré, vous allez modifier le StoreController pour fournir le modèle de vue avec les données extraites de la base de données, au lieu d’utiliser des valeurs codées en dur.

> [!NOTE]
> Si vous avez terminé l’exercice 1 et que vous avez déjà travaillé avec l’approche Database First, vous allez maintenant apprendre à obtenir les mêmes résultats avec un autre processus. Les tâches qui sont en commun avec exercice 1 ont été marqués pour faciliter la lecture. Si vous ne sont pas terminées exercice 1 mais que vous souhaitez en savoir plus l’approche Code First, vous pouvez démarrer à partir de cet exercice et obtenir une couverture complète de la rubrique.


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a>Tâche 1 - exemples de données durant le remplissage

Dans cette tâche, vous remplirez la base de données avec des exemples de données lors de sa création commencerons par à l’aide de Code First.

1. Ouvrez le **commencer** solution situé dans **/Ex2-CreatingADatabaseCodeFirst/début du fichierSource/** dossier. Sinon, vous pouvez continuer à utiliser le **fin** solution obtenu par le biais de l’exercice précédent.

   1. Si vous avez ouvert le fourni **commencer** solution, vous devrez télécharger certains packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.
   2. Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.

      > [!NOTE]
      > Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
2. Ajouter le **SampleData.cs** de fichiers à la **modèles** dossier. Pour ce faire, cliquez sur **modèles** dossier, pointez sur **ajouter** puis cliquez sur **élément existant**. Accédez à **\Source\Assets** et sélectionnez le **SampleData.cs** fichier.

    ![Exemples de données remplissent code](aspnet-mvc-4-models-and-data-access/_static/image18.png "code remplissent les exemples de données")

    *Exemples de données remplissent le code*
3. Ouvrez le **Global.asax.cs** fichier et ajoutez le code suivant *à l’aide de* instructions.

    (Code Snippet - *modèles et accès aux données - Ex2 les instructions Using Asax Global*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. Dans le **Application\_Start()** méthode ajoutez la ligne suivante pour définir l’initialiseur de base de données.

    (Code Snippet - *modèles et accès aux données - Ex2 Global Asax SetInitializer*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a>Tâche 2 : configuration de la connexion à la base de données

Maintenant que vous avez déjà ajouté une base de données à notre projet, vous allez écrire le **Web.config** la chaîne de connexion de fichiers.

1. Ajouter une chaîne de connexion à **Web.config**. Pour ce faire, ouvrez **Web.config** à la racine du projet et remplacez la chaîne de connexion nommée DefaultConnection par cette ligne dans le **&lt;connectionStrings&gt;** section :

    ![Emplacement du fichier Web.config](aspnet-mvc-4-models-and-data-access/_static/image19.png "emplacement du fichier Web.config")

    *Emplacement du fichier Web.config*

    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a>Tâche 3 : utilisation du modèle

Maintenant que vous avez déjà configuré la connexion à la base de données, vous allez lier le modèle avec les tables de base de données. Dans cette tâche, vous allez créer une classe qui sera liée à la base de données avec Code First. N’oubliez pas qu’il existe une classe de modèle POCO existante qui doit être modifiée.

> [!NOTE]
> Si vous avez terminé d’exercice 1, vous noterez que cette étape a été effectuée par un Assistant. En procédant comme Code First, vous créerez manuellement des classes qui seront liés à des entités de données.

1. Ouvrez la classe de modèle POCO **Genre** de **modèles** dossier du projet et inclure un code. Utiliser une propriété int avec le nom **GenreId**.

    (Code Snippet - *modèles et accès aux données - Ex2 Code premier Genre*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > Pour travailler avec les conventions Code First, la Genre de classe doit avoir une propriété de clé primaire sera automatiquement détectée.
    > 
    > Vous trouverez plus d’informations sur les Conventions Code First dans ce [article msdn](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).
2. À présent, ouvrez la classe de modèle POCO **Album** de **modèles** dossier du projet et inclure les clés étrangères, créer des propriétés avec les noms **GenreId** et  **ArtistId**. Cette classe a déjà le **GenreId** pour la clé primaire.

    (Code Snippet - *modèles et accès aux données - Ex2 Code premier Album*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. Ouvrez la classe de modèle POCO **artiste** et incluent la **ArtistId** propriété.

    (Code Snippet - *modèles et accès aux données - Ex2 Code premier artiste*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. Cliquez sur le **modèles** dossier du projet, puis sélectionnez **ajouter | Classe**. Nommez le fichier **MusicStoreEntities.cs**. Ensuite, cliquez sur **ajouter.**

    ![Ajout d’une classe](aspnet-mvc-4-models-and-data-access/_static/image20.png "Ajout d’une classe")

    *Ajout d’un nouvel élément*

    ![Ajout d’un classe2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Ajout un classe2")

    *Ajout d’une classe*
5. Ouvrez la classe que vous venez de créer, **MusicStoreEntities.cs**et incluez les espaces de noms **System.Data.Entity** et **System.Data.Entity.Infrastructure**.

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. Remplacez la déclaration de classe pour étendre le **DbContext** classe : déclarer un public **DBSet** et remplacer **OnModelCreating** (méthode). Après cette étape, vous obtiendrez une classe de domaine qui sera liée à votre modèle avec Entity Framework. Pour ce faire, remplacez le code de classe avec les éléments suivants :

    (Code Snippet - *modèles et accès aux données - Ex2 Code première MusicStoreEntities*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> Avec Entity Framework **DbContext** et **DBSet** vous serez en mesure d’interroger la classe POCO Genre. En étendant **OnModelCreating** (méthode), vous spécifiez dans le **code** comment Genre sera mappé à une table de base de données. Vous trouverez plus d’informations sur DBContext et DBSet dans cet article msdn : [lien](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Tâche 4 : interrogation de la base de données

Dans cette tâche, vous mettrez à jour la classe StoreController afin que, au lieu d’utiliser les données codées en dur, il sera le récupérer à partir de la base de données.

> [!NOTE]
> Cette tâche est en commun avec l’exercice 1.
> 
> Si vous avez terminé l’exercice 1 vous noterez que ces étapes sont les mêmes dans les deux approches (tout d’abord la base de données ou de Code first). Elles sont différentes dans la façon dont les données sont liées avec le modèle, mais l’accès aux entités de données est encore transparent à partir du contrôleur.


1. Ouvrez **Controllers\StoreController.cs** et ajoutez le champ suivant à la classe pour contenir une instance de la **MusicStoreEntities** classe, nommée **storeDB**:

    (Code Snippet - *modèles et accès aux données - Ex1 storeDB*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. Le **MusicStoreEntities** classe expose une propriété de collection pour chaque table dans la base de données. Mise à jour **Parcourir** méthode d’action pour récupérer un Genre avec toutes les **Albums**.

    (Code Snippet - *modèles et accès aux données - Ex2 Store Parcourir*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > Vous utilisez une fonctionnalité de .NET appelée **LINQ** (language integrated query) pour écrire des expressions de requête fortement typée par rapport à ces collections - qui exécutera le code par rapport à la base de données et retourner des objets que vous pouvez programmer par rapport aux.
    > 
    > Pour plus d’informations sur LINQ, visitez le [site msdn](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).
3. Mise à jour **Index** méthode d’action pour récupérer tous les genres.

    (Code Snippet - *modèles et accès aux données - Ex2 Store Index*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. Mise à jour **Index** méthode d’action pour récupérer tous les genres et de transformer la collection à une liste.

    (Code Snippet - *modèles et accès aux données - Ex2 Store GenreMenu*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tâche 5 : exécution de l’Application

Dans cette tâche, vous allez vérifier que la page d’Index de Store s’affichent désormais les Genres stockées dans la base de données au lieu de celles codées. Il est inutile de modifier le modèle de vue, car le **StoreController** renvoie les mêmes **StoreIndexViewModel** comme avant, mais cette fois les données proviennent de la base de données.

1. Régénérer la solution et appuyez sur **F5** pour exécuter l’Application.
2. Le projet démarre dans la page d’accueil. Vérifiez que le menu de **Genres** n’est plus une liste codée en dur, et les données sont récupérées directement à partir de la base de données.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    *Genres de navigation à partir de la base de données*
3. Maintenant, accédez à n’importe quel genre et vérifier que les albums sont remplis à partir de la base de données.

    ![Exploration des Albums à partir de la base de données](aspnet-mvc-4-models-and-data-access/_static/image23.png "Albums de navigation à partir de la base de données")

    *Exploration des Albums à partir de la base de données*

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a>Exercice 3 : Interrogation de la base de données avec des paramètres

Dans cet exercice, vous allez apprendre comment interroger la base de données à l’aide de paramètres et comment utiliser la mise en forme du résultat de requête, une fonctionnalité qui permet de réduire la base de données numéro accède aux données lors de la récupération d’une manière plus efficace.

> [!NOTE]
> Pour plus d’informations sur la mise en forme du résultat de requête, visitez le [article msdn](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a>Tâche 1 - modification StoreController pour récupérer les Albums à partir de la base de données

Dans cette tâche, vous allez modifier le **StoreController** classe pour accéder à la base de données pour récupérer les albums d’un genre spécifique.

1. Ouvrez le **commencer** solution situé dans le **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** dossier si vous souhaitez utiliser l’approche Code First ou **Source\ Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** dossier si vous souhaitez utiliser l’approche de la base de données en premier. Sinon, vous pouvez continuer à utiliser le **fin** solution obtenu par le biais de l’exercice précédent.

   1. Si vous avez ouvert le fourni **commencer** solution, vous devrez télécharger certains packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.
   2. Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.

      > [!NOTE]
      > Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
2. Ouvrez le **StoreController** classe pour modifier le **Parcourir** méthode d’action. Pour ce faire, dans le **l’Explorateur de solutions**, développez le **contrôleurs** dossier et double-cliquez sur **StoreController.cs**.
3. Modifier le **Parcourir** méthode d’action pour récupérer les albums pour un genre spécifique. Pour ce faire, remplacez le code suivant :

    (Code Snippet - *modèles et accès aux données - Ex3 StoreController BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> Pour remplir une collection de l’entité, vous devez utiliser le **Include** méthode pour spécifier à extraire les albums trop. Vous pouvez utiliser le. **Single()** extension dans LINQ, car dans ce cas qu’un seul genre est attendu pour un album. Le **Single()** méthode prend une expression Lambda en tant que paramètre, qui spécifie dans ce cas un seul objet de Genre telles que son nom corresponde à la valeur définie.
> 
> Vous serez tirer parti d’une fonctionnalité qui vous permet d’indiquer d’autres entités connexes souhaitées ainsi chargé lors de l’objet de Genre est récupéré. Cette fonctionnalité est appelée **mise en forme du résultat de requête**et vous permet de réduire le nombre de fois que nécessaire pour accéder à la base de données pour récupérer des informations. Dans ce scénario, vous souhaitez pré-extrait les Albums pour le Genre que vous récupérez.
> 
> La requête inclut **Genres.Include (&quot;Albums&quot;)** pour indiquer que vous souhaitez également les albums connexes. Ainsi, une application plus efficace, car elle récupère les données Genre et Album dans une requête de base de données unique.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Tâche 2 : exécution de l’Application

Dans cette tâche, vous exécutez l’application et récupérer des albums d’un genre spécifique à partir de la base de données.

1. Appuyez sur **F5** pour exécuter l’Application.
2. Le projet démarre dans la page d’accueil. Remplacez l’URL par **/Store/Parcourir ? genre = Pop** pour vérifier que les résultats sont récupérés à partir de la base de données.

    ![Navigation par genre](aspnet-mvc-4-models-and-data-access/_static/image24.png "navigation par genre")

    *Navigation/Store/Parcourir ? genre = Pop*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a>Tâche 3 : accéder à des Albums par Id

Dans cette tâche, vous serez Répétez la procédure précédente pour obtenir des albums par leur Id.

1. Fermez le navigateur si nécessaire, pour revenir à Visual Studio. Ouvrez le **StoreController** classe pour modifier le **détails** méthode d’action. Pour ce faire, dans le **l’Explorateur de solutions**, développez le **contrôleurs** dossier et double-cliquez sur **StoreController.cs**.
2. Modifier le **détails** méthode d’action pour récupérer les détails des albums selon leur **Id**. Pour ce faire, remplacez le code suivant :

    (Code Snippet - *modèles et accès aux données - Ex3 StoreController DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tâche 4 : exécution de l’Application

Dans cette tâche, vous exécutez l’Application dans un navigateur web et obtenir des détails de l’album par leur Id.

1. Appuyez sur **F5** pour exécuter l’Application.
2. Le projet démarre dans la page d’accueil. Remplacez l’URL par **/Store/Details/51** ou parcourir les genres et sélectionnez un album pour vérifier que les résultats sont récupérés à partir de la base de données.

    ![Détails de la navigation](aspnet-mvc-4-models-and-data-access/_static/image25.png "navigation détails")

    *Navigation /Store/Details/51*

> [!NOTE]
> En outre, vous pouvez déployer cette application à Sites Web Windows Azure suit [publication annexe b : une Application ASP.NET MVC 4, à l’aide de Web Deploy](#AppendixB).

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Récapitulatif

En fin de cet atelier pratique vous avez appris les principes fondamentaux de l’accès aux données et les modèles ASP.NET MVC, à l’aide de la **Database First** approche mais aussi le **Code First** approche :

- Comment ajouter une base de données à la solution pour utiliser ses données
- Comment mettre à jour des contrôleurs pour fournir des modèles de vue avec les données extraites de la base de données au lieu de codées en dur
- Comment interroger la base de données à l’aide de paramètres
- Comment utiliser la requête résultat mise en forme, une fonctionnalité qui permet de réduire le nombre d’accès de base de données, récupérer des données de manière plus efficace
- Comment utiliser des approches Database First et Code First dans Entity Framework de Microsoft pour lier la base de données avec le modèle

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Annexe a : installation de Visual Studio Express 2012 pour le Web

Vous pouvez installer **Microsoft Visual Studio Express 2012 pour Web** ou un autre &quot;Express&quot; à l’aide de la version du **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Les instructions suivantes vous guident dans les étapes requises pour installer *Visual studio Express 2012 pour Web* à l’aide de *Microsoft Web Platform Installer*.

1. Accédez à [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Si vous avez déjà installé Web Platform Installer, vous pouvez également ouvrir il et recherchez le produit &quot; <em>Visual Studio Express 2012 pour le Web avec Windows Azure SDK</em>&quot;.
2. Cliquez sur **installer maintenant**. Si vous n’avez pas **Web Platform Installer** vous allez être redirigé pour télécharger et installer en premier.
3. Une fois **Web Platform Installer** est ouvert, cliquez sur **installer** pour démarrer le programme d’installation.

    ![Installer Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "installer Visual Studio Express")

    *Installer Visual Studio Express*
4. Lisez les licences et les termes du contrat de tous les produits et cliquez sur **J’accepte** pour continuer.

    ![Accepter les termes du contrat de licence](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    *Accepter les termes du contrat de licence*
5. Attendez que le processus de téléchargement et l’installation se termine.

    ![Progression de l'installation](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    *Progression de l’installation*
6. Une fois l’installation terminée, cliquez sur **Terminer**.

    ![Installation est terminée](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    *Installation est terminée*
7. Cliquez sur **Exit** pour fermer Web Platform Installer.
8. Pour ouvrir Visual Studio Express pour le Web, accédez à la **Démarrer** écran et démarrer l’écriture &quot; **VS Express**&quot;, puis cliquez sur le **Visual Studio Express pour le Web** vignette.

    ![VS Express pour une vignette de Web](aspnet-mvc-4-models-and-data-access/_static/image30.png)

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

    ![Ouvrez une session sur le portail Windows Azure](aspnet-mvc-4-models-and-data-access/_static/image31.png "ouvrez une session sur le portail Windows Azure")

    *Ouvrez une session sur le portail de gestion Azure Windows*
2. Cliquez sur **New** sur la barre de commandes.

    ![Création d’un Site Web](aspnet-mvc-4-models-and-data-access/_static/image32.png "création d’un Site Web")

    *Création d’un Site Web*
3. Cliquez sur **calcul** | **Site Web**. Puis sélectionnez **création rapide** option. Fournir une URL disponible pour le nouveau site web et cliquez sur **créer un Site Web**.

    > [!NOTE]
    > Un Site Web Windows Azure est l’hôte pour une application web en cours d’exécution dans le cloud que vous pouvez contrôler et gérer. L’option Création rapide vous permet de déployer une application web terminée pour le Site Web Windows Azure à partir en dehors du portail. Il n’inclut pas les étapes de configuration d’une base de données.

    ![Création d’un Site Web à l’aide de la création rapide](aspnet-mvc-4-models-and-data-access/_static/image33.png "création d’un Site Web à l’aide de la création rapide")

    *Création d’un Site Web à l’aide de la création rapide*
4. Attendez que la nouvelle **Site Web** est créé.
5. Une fois que le Site Web est créé, cliquez sur le lien situé sous le **URL** colonne. Vérifiez que le nouveau Site Web fonctionne.

    ![Navigation vers le nouveau site web](aspnet-mvc-4-models-and-data-access/_static/image34.png "exploration vers le nouveau site web")

    *Navigation vers le nouveau site web*

    ![Site Web en cours d’exécution](aspnet-mvc-4-models-and-data-access/_static/image35.png "site Web en cours d’exécution")

    *Site Web en cours d’exécution*
6. Revenez au portail et cliquez sur le nom du site web sous le **nom** colonne pour afficher les pages de gestion.

    ![Ouvrir les pages de gestion de site web](aspnet-mvc-4-models-and-data-access/_static/image36.png "ouvrir les pages de gestion de site web")

    *Ouvrir les pages de gestion de Site Web*
7. Dans le **tableau de bord** page, sous la **aperçu rapide** , cliquez sur le **télécharger le profil de publication** lien.

    > [!NOTE]
    > Le *le profil de publication* contient toutes les informations requises pour publier une application web à un site Web Windows Azure pour chaque méthode de publication est activée. Le profil de publication contient les URL, les informations d’identification utilisateur et les chaînes de base de données requis pour la connexion et l’authentification par rapport à chacun des points de terminaison pour lequel une méthode de publication est activée. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express pour Web** et **Microsoft Visual Studio 2012** prise en charge de la lecture des profils pour automatiser la configuration de ces programmes pour de publication publication d’applications web sur sites Web Windows Azure.

    ![Téléchargement du site web le profil de publication](aspnet-mvc-4-models-and-data-access/_static/image37.png "téléchargement du site web le profil de publication")

    *Téléchargement du Site Web le profil de publication*
8. Télécharger le fichier de profil de publication dans un emplacement connu. Davantage dans cet exercice, vous verrez comment utiliser ce fichier pour publier une application web à un Sites Web Windows Azure à partir de Visual Studio.

    ![L’enregistrement du fichier de profil de publication](aspnet-mvc-4-models-and-data-access/_static/image38.png "l’enregistrement du profil de publication")

    *L’enregistrement du fichier de profil de publication*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tâche 2 : configuration du serveur de base de données

Si votre application se sert de SQL Server vous devez créer un serveur de base de données SQL des bases de données. Si vous souhaitez déployer une application simple qui n’utilise pas de SQL Server, vous pouvez ignorer cette tâche.

1. Vous devez un serveur de base de données SQL pour stocker la base de données d’application. Vous pouvez afficher les serveurs de base de données SQL à partir de votre abonnement dans le portail de gestion Windows Azure à l’adresse **bases de données Sql** | **serveurs** | **du serveur Tableau de bord**. Si vous n’avez pas un serveur créé, vous pouvez créer un à l’aide du **ajouter** bouton sur la barre de commandes. Prenez note de la **nom du serveur et les URL, nom de connexion d’administrateur et mot de passe**, car vous les utiliserez dans les tâches suivantes. Ne créez pas encore, la base de données tel qu’il sera créé dans une étape ultérieure.

    ![Tableau de bord de serveur SQL Database](aspnet-mvc-4-models-and-data-access/_static/image39.png "tableau de bord serveur de base de données SQL")

    *Tableau de bord serveur de base de données SQL*
2. Dans la tâche suivante, vous allez tester la connexion de base de données à partir de Visual Studio, pour cette raison, vous devez inclure votre adresse IP locale dans la liste du serveur de **adresses IP autorisées**. Pour ce faire, cliquez sur **configurer**, sélectionnez l’adresse IP à partir de **adresse IP cliente actuelle** et collez-le sur le **adresse IP de début** et **adresse IP de fin** zones de texte et cliquez sur le ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) bouton.

    ![Ajout d’adresse IP du Client](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    *Ajout d’adresse IP du Client*
3. Une fois le **adresse IP du Client** est ajouté à le des adresses IP autorisées de liste, cliquez sur **enregistrer** pour confirmer les modifications.

    ![Confirmer les modifications](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    *Confirmer les modifications*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tâche 3 : publication d’une Application ASP.NET MVC 4, à l’aide de Web Deploy

1. Revenez à la solution ASP.NET MVC 4. Dans le **l’Explorateur de solutions**, cliquez sur le projet de site web et sélectionnez **publier**.

    ![Publication de l’Application](aspnet-mvc-4-models-and-data-access/_static/image43.png "publication de l’Application")

    *Publier le site web*
2. Importer le profil de publication que vous avez enregistré dans la première tâche.

    ![L’importation du profil de publication](aspnet-mvc-4-models-and-data-access/_static/image44.png "l’importation du profil de publication")

    *Importation du profil de publication*
3. Cliquez sur **valider la connexion**. Une fois la Validation terminée. Cliquez sur **suivant**.

    > [!NOTE]
    > La validation est terminée une fois que vous voyez une coche verte apparaît en regard du bouton Valider la connexion.

    ![Validation de la connexion](aspnet-mvc-4-models-and-data-access/_static/image45.png "validation de la connexion")

    *Validation de la connexion*
4. Dans le **paramètres** page, sous la **bases de données** , cliquez sur le bouton en regard de la zone de texte de la connexion de votre base de données (par exemple, **DefaultConnection**).

    ![Configuration de déploiement Web](aspnet-mvc-4-models-and-data-access/_static/image46.png "configuration de déploiement Web")

    *Configuration de déploiement Web*
5. Configurez la connexion de base de données comme suit :

   - Dans le **nom du serveur** tapez votre URL de base de données SQL server à l’aide du *tcp :* préfixe.
   - Dans **nom d’utilisateur** tapez votre nom de connexion d’administrateur serveur.
   - Dans **mot de passe** tapez votre mot de passe du compte de connexion administrateur serveur.
   - Tapez un nouveau nom de base de données.

     ![Configuration de chaîne de connexion de destination](aspnet-mvc-4-models-and-data-access/_static/image47.png "configuration de chaîne de connexion de destination")

     *Configuration de chaîne de connexion de destination*
6. Cliquez ensuite sur **OK**. Lorsque vous êtes invité à créer la base de données, cliquez sur **Oui**.

    ![Création de la base de données](aspnet-mvc-4-models-and-data-access/_static/image48.png "création de la chaîne de la base de données")

    *Création de la base de données*
7. La chaîne de connexion que vous utiliserez pour vous connecter à la base de données SQL dans Windows Azure est indiquée dans la zone de texte de la connexion par défaut. Cliquez ensuite sur **Suivant**.

    ![Chaîne de connexion pointant vers la base de données SQL](aspnet-mvc-4-models-and-data-access/_static/image49.png "chaîne de connexion pointant vers la base de données SQL")

    *Chaîne de connexion pointant vers la base de données SQL*
8. Dans le **aperçu** , cliquez sur **publier**.

    ![Publication de l’application web](aspnet-mvc-4-models-and-data-access/_static/image50.png "publication de l’application web")

    *Publication de l’application web*
9. Une fois le processus de publication terminé, votre navigateur par défaut s’ouvre le site web publié.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Annexe c : l’utilisation des extraits de Code

Avec des extraits de code, vous avez tout le code que vous avez besoin à portée de main. Le document de laboratoire vous indiquera exactement quand vous pouvez les utiliser, comme indiqué dans l’illustration suivante.

![À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet](aspnet-mvc-4-models-and-data-access/_static/image51.png "extraits de code à l’aide de Visual Studio pour insérer du code dans votre projet")

*À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet*

***Pour ajouter un extrait de code à l’aide du clavier (c# uniquement)***

1. Placez le curseur où vous souhaitez insérer le code.
2. Commencez à taper le nom de l’extrait de code (sans espaces ou des traits d’union).
3. Regarder en tant qu’IntelliSense affiche les noms des extraits correspondants.
4. Sélectionnez l’extrait de code correct (ou continuez à taper jusqu'à ce que le nom de l’extrait de code entière est sélectionnée).
5. Appuyez sur la touche Tab à deux reprises pour insérer l’extrait de code à l’emplacement du curseur.

![Commencez à taper le nom de l’extrait de code](aspnet-mvc-4-models-and-data-access/_static/image52.png "commencez à taper le nom de l’extrait de code")

*Commencez à taper le nom de l’extrait de code*

![Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance](aspnet-mvc-4-models-and-data-access/_static/image53.png "appuyez sur Tab pour sélectionner l’extrait de code en surbrillance")

*Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance*

![Appuyez sur Tab à nouveau et l’extrait de code seront développe](aspnet-mvc-4-models-and-data-access/_static/image54.png "appuyez sur Tab à nouveau et l’extrait de code seront développe.")

*Appuyez sur Tab à nouveau et l’extrait de code seront développe.*

***Pour ajouter un extrait de code à l’aide de la souris (c#, Visual Basic et XML)*** 1. Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code.

1. Sélectionnez **insérer un extrait** suivie **mes extraits de Code**.
2. Choisissez l’extrait de code approprié dans la liste, en cliquant dessus.

![Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait](aspnet-mvc-4-models-and-data-access/_static/image55.png "avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait")

*Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait*

![Choisissez l’extrait de code approprié dans la liste, en cliquant dessus](aspnet-mvc-4-models-and-data-access/_static/image56.png "choisir l’extrait de code approprié dans la liste, en cliquant sur celle-ci")

*Choisissez l’extrait de code approprié dans la liste, en cliquant sur celle-ci*
