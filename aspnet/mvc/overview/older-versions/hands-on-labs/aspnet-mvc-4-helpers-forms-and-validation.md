---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: Applications auxiliaires ASP.NET MVC 4, formulaires et la Validation | Microsoft Docs
author: rick-anderson
description: Dans ASP.NET MVC 4 modèles et pratiques de données Access, vous avez été le chargement et affichage des données à partir de la base de données. Dans cet atelier pratique, vous allez ajouter à la...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: f2eb624e72d6f52d1694b5753ee2b1f8117c2851
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376735"
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a>Applications auxiliaires ASP.NET MVC 4, formulaires et validation

par [Web Camps Team](https://twitter.com/webcamps)

[Télécharger le Kit de formation de Web Camps](https://aka.ms/webcamps-training-kit)

Dans **accès aux données et les modèles ASP.NET MVC 4** les ateliers pratiques, vous avez été le chargement et l’affichage des données à partir de la base de données. Dans cet atelier pratique, vous allez ajouter à la **Music Store** application la possibilité de modifier ces données.

Dans ce but à l’esprit, vous allez d’abord créer le contrôleur qui prendra en charge les actions de création, lecture, mise à jour et suppression (CRUD) des albums. Vous allez générer un modèle de vue de l’Index en tirant parti de la fonctionnalité de génération de modèles automatique de ASP.NET MVC pour afficher les propriétés des albums dans une table HTML. Pour des raisons de cette vue, vous allez ajouter un programme d’assistance HTML personnalisée qui va tronquer les longues descriptions.

Vous ajouterez par la suite, le modifier et créer des vues qui vous permettra d’altèrent albums dans la base de données, à l’aide des éléments de formulaire tels que des listes déroulantes.

Enfin, vous permettra aux utilisateurs de supprimer un album et vous serez également les empêcher d’entrer des données erronées en validant leur entrée.

Ce laboratoire pratique suppose que vous avez une connaissance élémentaire de **ASP.NET MVC**. Si vous n’avez pas utilisé **ASP.NET MVC** auparavant, nous vous recommandons de consulter **principes de base ASP.NET MVC** les ateliers pratiques.

Ce laboratoire vous guide les améliorations et les nouvelles fonctionnalités décrites précédemment en appliquant des modifications mineures à un exemple d’application Web dans le dossier Source.

> [!NOTE]
> Tous les exemples de code et extraits de code sont inclus dans le Kit de formation Camps Web, disponible à l’adresse [les versions de Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Le projet spécifique à ce laboratoire est disponible à l’adresse [ASP.NET MVC 4 Helpers, formulaires et la Validation](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).

<a id="Objectives"></a>
### <a name="objectives"></a>Objectifs

Dans cet atelier pratique, vous allez apprendre comment :

- Créer un contrôleur pour prendre en charge les opérations CRUD
- Générer une vue d’Index pour afficher les propriétés de l’entité dans un tableau HTML
- Ajouter un programme d’assistance HTML personnalisée
- Créer et personnaliser une vue Modifier
- Faire la distinction entre les méthodes d’action qui réagissent aux HTTP-GET ou d’appels HTTP-POST
- Ajouter et personnaliser une instruction Create View
- Handle de la suppression d’une entité
- Valider l’entrée utilisateur

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

Les exercices suivants composent ce laboratoire pratique :

1. [Création du contrôleur de Store Manager et sa vue Index](#Exercise1)
2. [Ajout d’une application d’assistance HTML](#Exercise2)
3. [Création de la vue de modification](#Exercise3)
4. [Ajout d’une vue de créer](#Exercise4)
5. [Suppression de la gestion](#Exercise5)
6. [Ajout de la validation](#Exercise6)
7. [À l’aide de jQuery non obstructive côté Client](#Exercise7)

> [!NOTE]
> Chaque exercice est accompagné par un **fin** dossier contenant la solution obtenue, vous devez obtenir après avoir effectué les exercices. Si vous avez besoin d’aide supplémentaire sur l’utilisation via les exercices, vous pouvez utiliser cette solution comme guide.


Durée estimée pour effectuer ce laboratoire : **60 minutes**

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a>Exercice 1 : Création du contrôleur de Store Manager et sa vue Index

Dans cet exercice, vous allez apprendre à créer un nouveau contrôleur de prendre en charge les opérations CRUD, personnaliser sa méthode d’action Index pour retourner une liste d’albums à partir de la base de données et enfin générer un modèle de vue de l’Index en tirant parti de la structure de ASP.NET MVC fonction pour afficher les propriétés des albums dans une table HTML.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a>Tâche 1 : création de la StoreManagerController

Dans cette tâche, vous allez créer un nouveau contrôleur appelé **StoreManagerController** pour prendre en charge les opérations CRUD.

1. Ouvrez le **commencer** solution situé dans **/Ex1-CreatingTheStoreManagerController/début du fichierSource/** dossier.

   1. Vous devrez télécharger certains packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.
   2. Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.

      > [!NOTE]
      > Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
2. Ajoutez un nouveau contrôleur. Pour ce faire, cliquez sur le **contrôleurs** dossier dans l’Explorateur de solutions, sélectionnez **ajouter** , puis le **contrôleur** commande. Modifier le **contrôleur** **nom** à **StoreManagerController** et assurez-vous que l’option **contrôleur MVC avec des actions de lecture/écriture vides**est sélectionné. Cliquez sur **Ajouter**.

    ![Boîte de dialogue Ajouter contrôleur](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "boîte de dialogue Ajouter contrôleur")

    *Ajouter la boîte de dialogue contrôleur*

    Une nouvelle classe de contrôleur est générée. Étant donné que vous avez indiqué pour ajouter des actions de lecture/écriture, les méthodes stub pour ceux, des actions CRUD courantes sont créées avec des commentaires TODO renseignés, vous invitant à inclure la logique d’application spécifique.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a>Tâche 2 - Personnalisation de l’Index StoreManager

Dans cette tâche, vous allez personnaliser la méthode d’action StoreManager Index pour retourner une vue avec la liste des albums à partir de la base de données.

1. Dans la classe StoreManagerController, ajoutez le code suivant *à l’aide de* directives.

    (Code Snippet - *formulaires et Validation - Ex1 de programmes d’assistance ASP.NET MVC 4 et à l’aide de MvcMusicStore*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. Ajoutez un champ à la **StoreManagerController** devant contenir une instance de **MusicStoreEntities.**

    (Code Snippet - *applications auxiliaires ASP.NET MVC 4 et de formulaires et de Validation - Ex1 MusicStoreEntities*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. Implémenter l’action de l’Index de StoreManagerController pour retourner une vue avec la liste des albums.

    La logique d’action de contrôleur sera très similaire à l’action d’Index de la StoreController écrite précédemment. Utiliser LINQ pour récupérer tous les albums, y compris les informations de Genre et artiste pour l’affichage.

    (Code Snippet - *applications auxiliaires ASP.NET MVC 4 et de formulaires et de Validation - Ex1 StoreManagerController Index*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a>Tâche 3 : création de la vue Index

Dans cette tâche, vous allez créer le modèle de vue de l’Index pour afficher la liste des albums retourné par la **StoreManager** contrôleur.

1. Avant de créer le nouveau modèle de vue, vous devez créer le projet afin que le **boîte de dialogue Vue ajouter** connaît le **Album** classe à utiliser. Sélectionnez **générer | Build MvcMusicStore** pour générer le projet.
2. Avec le bouton droit à l’intérieur de la **Index** méthode d’action, puis **ajouter une vue**. Cela fera apparaître le **ajouter une vue** boîte de dialogue.

    ![Ajouter une vue](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "ajouter une vue")

    *Ajout d’une vue à partir de la méthode Index*
3. Dans la boîte de dialogue Ajouter une vue, vérifiez que le nom de la vue est **Index**. Sélectionnez le **créer une vue fortement typée** option, puis sélectionnez **Album (MvcMusicStore.Models)** à partir de la **classe de modèle** liste déroulante. Sélectionnez **liste** à partir de la **modèle de structure** liste déroulante. Laissez le **moteur d’affichage** à **Razor** et les autres champs leur valeur par défaut de valeur, puis cliquez **ajouter**.

    ![Ajout d’une vue index](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Ajout d’une vue d’index")

    *Ajout d’une vue d’Index*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a>Tâche 4 : personnalisation de la structure de la vue Index

Dans cette tâche, vous allez ajuster le modèle de vue simple créé avec la fonctionnalité de génération de modèles automatique ASP.NET MVC pour qu’il affiche les champs souhaités.

> [!NOTE]
> Le **la structure** prise en charge dans ASP.NET MVC génère un simple modèle de vue qui répertorie tous les champs dans le modèle de l’Album. **Génération de modèles automatique** offre un moyen rapide pour commencer sur une vue fortement typée : plutôt que d’écrire le modèle de vue manuellement, la génération de modèles automatique rapidement génère un modèle par défaut et vous pouvez ensuite modifier le code généré.


1. Passez en revue le code créé. La liste de champs générée fera partie des éléments suivants de table HTML qui **la structure** utilise pour afficher les données tabulaires.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. Remplacez le **&lt;table&gt;** code avec le code suivant pour afficher uniquement les **Genre**, **artiste**, **titre d’Album**, et **prix** champs. Cette opération supprime le **AlbumId** et **Album Art URL** colonnes. En outre, il modifie les colonnes GenreId et ArtistId pour afficher leurs propriétés de classe lié de **Artist.Name** et **Genre.Name**et supprime la **détails** lien.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. Modifier les descriptions suivantes.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tâche 5 : exécution de l’Application

Dans cette tâche, vous allez tester que le **StoreManager** **Index** modèle de vue affiche une liste d’albums en fonction de la conception des étapes précédentes.

1. Appuyez sur **F5** pour exécuter l’Application.
2. Le projet démarre dans la page d’accueil. Remplacez l’URL par **/StoreManager** pour vérifier que la liste des albums est affichée, indiquant leur **titre**, **artiste** et **Genre**.

    ![Parcourir la liste des albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "parcourir la liste des albums")

    *Parcourir la liste des albums*

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a>Exercice 2 : Ajout d’une application d’assistance HTML

La page d’Index de StoreManager a un problème potentiel : propriétés du titre et un nom d’artiste peuvent tous deux être suffisamment longue pour décaler la mise en forme de table. Dans cet exercice, vous allez apprendre à ajouter un programme d’assistance HTML personnalisée pour tronquer ce texte.

Dans l’illustration suivante, vous pouvez voir la manière dont le format est modifié en raison de la longueur du texte lorsque vous utilisez une taille petite navigateur.

![Parcourir la liste des Albums avec pas tronqué texte](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "parcourir la liste des Albums avec texte pas est tronqué")

*Parcourir la liste des Albums avec texte pas est tronqué*

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a>Tâche 1 - extension du programme d’assistance HTML

Dans cette tâche, vous allez ajouter une nouvelle méthode **Truncate** à la **HTML** objet exposé dans les vues ASP.NET MVC. Pour ce faire, vous allez implémenter un **méthode d’extension** à intégrés **System.Web.Mvc.HtmlHelper** classe fournie par ASP.NET MVC.

> [!NOTE]
> Pour en savoir plus sur **méthodes d’Extension**, consultez cet article de msdn. [https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).


1. Ouvrez le **commencer** solution situé dans **/Ex2-AddingAnHTMLHelper/début du fichierSource/** dossier. Sinon, vous pouvez continuer à utiliser le **fin** solution obtenu par le biais de l’exercice précédent.

   1. Si vous avez ouvert le fourni **commencer** solution, vous devrez télécharger certains packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.
   2. Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.

      > [!NOTE]
      > Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
2. Ouvrez la vue de l’Index de StoreManager. Pour ce faire, dans l’Explorateur de solutions, développez le **vues** dossier, puis le **StoreManager** et ouvrez le **Index.cshtml** fichier.
3. Ajoutez le code suivant ci-dessous le <strong>@model</strong> directive pour définir le <strong>Truncate</strong> méthode d’assistance.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a>Tâche 2 - troncation de texte dans la Page

Dans cette tâche, vous allez utiliser le **Truncate** méthode pour tronquer le texte dans le modèle de vue.

1. Ouvrez la vue de l’Index de StoreManager. Pour ce faire, dans l’Explorateur de solutions, développez le **vues** dossier, puis le **StoreManager** et ouvrez le **Index.cshtml** fichier.
2. Remplacez les lignes qui indiquent la **un nom d’artiste** et d’Album **titre**. Pour ce faire, remplacez les lignes suivantes.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tâche 3 : exécution de l’Application

Dans cette tâche, vous allez tester que le **StoreManager** **Index** afficher le modèle tronque le titre et le nom de l’artiste l’Album.

1. Appuyez sur **F5** pour exécuter l’Application.
2. Le projet démarre dans la page d’accueil. Remplacez l’URL par **/StoreManager** pour vérifier ce long textes en le **titre** et **artiste** colonne sont tronqués.

    ![Tronqué les noms des titres et des artistes](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "tronqué les noms des titres et des artistes")

    *Titres tronquées et des artistes*

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a>Exercice 3 : Création de la vue de modification

Dans cet exercice, vous allez apprendre à créer un formulaire pour permettre aux responsables de magasin modifier un Album. Ils seront parcourent le **/StoreManager/Edit/id** URL (**id** en cours de l’id unique de l’album à modifier), ce qui rend un appel HTTP GET au serveur.

La méthode d’action contrôleur modifier se récupérer l’Album approprié à partir de la base de données, créez un **StoreManagerViewModel** objet à encapsuler (avec une liste d’artistes et Genres) et à transmettre à un modèle de vue afficher la page HTML à l’utilisateur. Cette page contient un **&lt;formulaire&gt;** élément avec les zones de texte et les listes déroulantes pour modifier les propriétés de l’Album.

Une fois que l’utilisateur met à jour les valeurs de formulaire d’Album et clique sur le **enregistrer** bouton, les modifications sont soumises une requête HTTP-POST rappeler **/StoreManager/Edit/id**. Bien que l’URL reste le même que dans le dernier appel, ASP.NET MVC qui identifie cette fois, il est une requête HTTP-POST et par conséquent s’exécute une autre méthode d’action de modification (une décorée avec **[HttpPost]**).

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a>Tâche 1 : implémentation de la méthode d’Action de modification HTTP-GET

Dans cette tâche, vous allez implémenter la version de HTTP-GET de la méthode d’action de modification pour récupérer l’Album approprié à partir de la base de données, ainsi qu’une liste de tous les Genres et artistes. Il sera empaqueter ces données dans le **StoreManagerViewModel** objet défini dans la dernière étape, qui est ensuite transmise à un modèle de vue pour afficher la réponse avec.

1. Ouvrez le **commencer** solution situé dans **/Ex3-CreatingTheEditView/début du fichierSource/** dossier. Sinon, vous pouvez continuer à utiliser le **fin** solution obtenu par le biais de l’exercice précédent.

   1. Si vous avez ouvert le fourni **commencer** solution, vous devrez télécharger certains packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.
   2. Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.

      > [!NOTE]
      > Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
2. Ouvrez le **StoreManagerController** classe. Pour ce faire, développez le **contrôleurs** dossier et double-cliquez sur **StoreManagerController.cs**.
3. Remplacez le **HTTP-GET modifier** méthode d’action avec le code suivant pour récupérer le texte approprié **Album** ainsi que le **Genres** et **artistes**répertorie.

    (Code Snippet - *Validation - Ex3 StoreManagerController HTTP-GET modifier l’action et des formulaires et des programmes d’assistance ASP.NET MVC 4*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > Vous utilisez **System.Web.Mvc** **SelectList** pour les artistes et Genres au lieu du **System.Collections.Generic** liste.
    > 
    > **SelectList** est un moyen plus propre pour remplir les listes déroulantes HTML et de gérer des éléments comme la sélection actuelle. Instanciation et la configuration ultérieure ces objets ViewModel dans l’action du contrôleur rendra le scénario de formulaire d’édition plus propre.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a>Tâche 2 : création de la vue de modification

Dans cette tâche, vous allez créer un modèle de modifier la vue qui affiche les propriétés de l’album ultérieurement.

1. Créer la vue Edit. Pour ce faire, le bouton droit dans le **modifier** méthode d’action, puis **ajouter une vue**.
2. Dans la boîte de dialogue Ajouter une vue, vérifiez que le nom de la vue est **modifier**. Vérifier le **créer une vue fortement typée** case à cocher et sélectionnez **Album (MvcMusicStore.Models)** à partir de la **afficher la classe de données** liste déroulante. Sélectionnez **modifier** à partir de la **modèle de structure** liste déroulante. Laissez les autres champs avec leur valeur par défaut, puis **ajouter**.

    ![Ajout d’une vue d’édition](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Ajout d’une vue d’édition")

    *Ajout d’une vue d’édition*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tâche 3 : exécution de l’Application

Dans cette tâche, vous allez tester que le **StoreManager** **modifier** afficher la page affiche les valeurs des propriétés de l’album passée comme paramètre.

1. Appuyez sur **F5** pour exécuter l’Application.
2. Le projet démarre dans la page d’accueil. Remplacez l’URL par **/StoreManager/Edit/1** pour vérifier que les valeurs des propriétés de l’album passé sont affichés.

    ![Parcourir la vue de modification d’Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "parcourir la vue de modification d’Album")

    *Vue d’édition d’Album de navigation*

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a>Tâche 4 : implémentation des listes déroulantes sur le modèle d’éditeur d’Album

Dans cette tâche, vous allez ajouter des listes déroulantes du modèle de vue créé dans la dernière tâche, afin que l’utilisateur peut sélectionner à partir d’une liste d’artistes et Genres.

1. Remplacez tout le **Album** code fieldset par le code suivant :

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > Un **Html.DropDownList** helper a été ajouté pour afficher les listes déroulantes pour le choix d’artistes et Genres. Les paramètres passés à **Html.DropDownList** sont :
    > 
    > 1. Le nom du champ de formulaire (**&quot;ArtistId&quot;**).
    > 2. Le **SelectList** des valeurs pour la liste déroulante.

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tâche 5 : exécution de l’Application

Dans cette tâche, vous allez tester que le **StoreManager** **modifier** afficher la page affiche les listes déroulantes au lieu de champs de texte artiste et de l’ID du Genre.

1. Appuyez sur **F5** pour exécuter l’Application.
2. Le projet démarre dans la page d’accueil. Remplacez l’URL par **/StoreManager/Edit/1** pour vérifier qu’il affiche des listes déroulantes au lieu de champs de texte artiste et de l’ID du Genre.

    ![Exploration modifier la vue d’Album avec les listes déroulantes](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "modifier la vue d’Album de navigation avec les listes déroulantes")

    *Vue de modification d’Album, cette fois avec des listes déroulantes de navigation*

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a>Tâche 6 : implémentation de la méthode d’action HTTP-POST modifier

Maintenant que la vue Modifier affiche comme prévu, vous devez implémenter la méthode de modifier l’Action HTTP-POST pour enregistrer les modifications apportées à l’Album.

1. Fermez le navigateur si nécessaire, pour revenir à la fenêtre Visual Studio. Ouvrez **StoreManagerController** à partir de la **contrôleurs** dossier.
2. Remplacez **HTTP-POST modifier** code de méthode d’action par le code suivant (Notez que la méthode qui doit être remplacée est la version surchargée qui reçoit deux paramètres) :

    (Code Snippet - *Validation - Ex3 StoreManagerController HTTP-POST modifier l’action et des formulaires et des programmes d’assistance ASP.NET MVC 4*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > Cette méthode est exécutée quand l’utilisateur clique sur le **enregistrer** bouton de la vue et effectue une requête HTTP-POST des valeurs de formulaire sur le serveur pour les conserver dans la base de données. L’élément décoratif **[HttpPost]** indique que la méthode doit être utilisée pour les scénarios HTTP-POST. La méthode accepte un **Album** objet. ASP.NET MVC crée automatiquement l’objet d’Album à partir de la validé &lt;formulaire&gt; valeurs.
    > 
    > La méthode effectuera ces étapes :
    > 
    > 1. Si le modèle est valide :
    > 
    >     1. Mettre à jour l’entrée de l’album dans le contexte pour la marquer comme un objet modifié.
    >     2. Enregistrer les modifications et rediriger vers la vue index.
    > 2. Si le modèle n’est pas valid, elle remplira ViewBag avec le **GenreId** et **ArtistId**, il retourne la vue avec l’objet Album reçu pour autoriser l’utilisateur effectuer une mise à jour obligatoire.

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Tâche 7 : exécution de l’Application

Dans cette tâche, vous allez tester que le **StoreManager modifier** page de vue enregistre réellement les données d’Album mis à jour dans la base de données.

1. Appuyez sur **F5** pour exécuter l’Application.
2. Le projet démarre dans la page d’accueil. Remplacez l’URL par **/StoreManager/Edit/1**. Remplacez le titre d’Album par **charge** , puis cliquez sur **enregistrer**. Vérifiez que le titre d’album réellement modifié dans la liste des albums.

    ![La mise à jour un album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "mise à jour d’un album")

    *La mise à jour d’un Album*

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a>Exercice 4 : Ajout d’une vue de créer

Maintenant que le **StoreManagerController** prend en charge la **modifier** capacité, dans cet exercice, vous allez apprendre à ajouter un modèle Create View pour permettent de stocker des gestionnaires Ajouter nouveau Albums à l’application.

Comme vous l’avez fait avec la fonctionnalité d’édition, vous allez implémenter le scénario de créer à l’aide de deux méthodes distinctes au sein de la **StoreManagerController** classe :

1. Une méthode d’action affiche un formulaire vide lorsque des responsables de magasin d’abord visiter le **/StoreManager/créer** URL.
2. Une deuxième méthode d’action gère le scénario où le directeur du magasin clique sur le **enregistrer** bouton dans le formulaire et soumet les valeurs de retour à la **/StoreManager/créer** URL sous la forme d’une requête HTTP-POST.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a>Tâche 1 : implémentation de la méthode d’action HTTP-GET créer

Dans cette tâche, vous allez implémenter la version de HTTP-GET de la méthode d’action Create pour récupérer une liste de tous les Genres et artistes, empaqueter ces données dans un **StoreManagerViewModel** objet, qui est ensuite transmis à un modèle de vue.

1. Ouvrez le **commencer** solution situé dans **/Ex4-AddingACreateView/début du fichierSource/** dossier. Sinon, vous pouvez continuer à utiliser le **fin** solution obtenu par le biais de l’exercice précédent.

   1. Si vous avez ouvert le fourni **commencer** solution, vous devrez télécharger certains packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.
   2. Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.

      > [!NOTE]
      > Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
2. Ouvrez **StoreManagerController** classe. Pour ce faire, développez le **contrôleurs** dossier et double-cliquez sur **StoreManagerController.cs**.
3. Remplacez le **créer** code de méthode d’action avec les éléments suivants :

    (Code Snippet - *programmes d’assistance ASP.NET MVC 4 et de formulaires et de Validation - action de création de HTTP-GET Ex4 StoreManagerController*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a>Tâche 2 : ajout de la vue Create

Dans cette tâche, vous allez ajouter le modèle de créer une vue qui affiche un nouveau formulaire d’Album (vide).

1. Avec le bouton droit à l’intérieur de la **créer** méthode d’action, puis **ajouter une vue**. Cela fera apparaître la boîte de dialogue Ajouter une vue.
2. Dans la boîte de dialogue Ajouter une vue, vérifiez que le nom de la vue est **créer**. Sélectionnez le **créer une vue fortement typée** option et sélectionnez **Album (MvcMusicStore.Models)** à partir de la **classe de modèle** liste déroulante et **créer** à partir de la **modèle de structure** liste déroulante. Laissez les autres champs avec leur valeur par défaut, puis **ajouter**.

    ![Ajout d’une vue de créer](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "ajout-a-créer-view.png")

    *Ajout de la vue Create*
3. Mise à jour le **GenreId** et **ArtistId** champs à utiliser une liste déroulante, comme indiqué ci-dessous :

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tâche 3 : exécution de l’Application

Dans cette tâche, vous allez tester que le **StoreManager** **créer** afficher la page affiche un formulaire d’Album vide.

1. Appuyez sur **F5** pour exécuter l’Application.
2. Le projet démarre dans la page d’accueil. Remplacez l’URL par **/StoreManager/créer**. Vérifiez qu’un formulaire vide s’affiche pour remplir les nouvelles propriétés de l’Album.

    ![Créer une vue avec un formulaire vide](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "créer une vue avec un formulaire vide")

    *Créer une vue avec un formulaire vide*

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a>Tâche 4 : implémentation d’une requête HTTP-POST créer la méthode d’Action

Dans cette tâche, vous allez implémenter la version de HTTP-POST de la méthode d’action de création qui sera appelée quand un utilisateur clique sur le **enregistrer** bouton. La méthode doit enregistrer le nouvel album dans la base de données.

1. Fermez le navigateur si nécessaire, pour revenir à la fenêtre Visual Studio. Ouvrez **StoreManagerController** classe. Pour ce faire, développez le **contrôleurs** dossier et double-cliquez sur **StoreManagerController.cs**.
2. Remplacez **HTTP-POST créer** code de méthode d’action avec les éléments suivants :

    (Code Snippet - *Validation - Ex4 StoreManagerController HTTP - POST permet de créer une action et des formulaires et des programmes d’assistance ASP.NET MVC 4*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > L’action de création est assez similaire à la méthode d’action de modification précédente, mais au lieu de définir l’objet comme modifiée, il est ajouté au contexte.

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tâche 5 : exécution de l’Application

Dans cette tâche, vous allez tester que le **StoreManager créer** page de vue vous permet de créer un nouvel Album, il redirige vers la vue Index StoreManager.

1. Appuyez sur **F5** pour exécuter l’Application.
2. Le projet démarre dans la page d’accueil. Remplacez l’URL par **/StoreManager/créer**. Remplissez tous les champs de formulaire avec des données pour un nouvel Album, comme celui de la figure suivante :

    ![Création d’un Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "création d’un Album")

    *Création d’un Album*
3. Vérifiez que vous êtes redirigé vers la vue Index StoreManager qui inclut le nouvel Album venez de créer.

    ![Nouvel Album créé](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "Nouvel Album créé")

    *Nouvel Album créé*

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a>Exercice 5 : Gestion de suppression

La possibilité de supprimer des albums n’est pas encore implémentée. Voici ce que cet exercice sera sur. Comme auparavant, vous allez implémenter le scénario de suppression à l’aide de deux méthodes distinctes au sein de la **StoreManagerController** classe :

1. Une méthode d’action affiche un écran de confirmation
2. Une deuxième méthode d’action gère l’envoi du formulaire

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a>Tâche 1 : implémentation de la méthode d’Action Delete HTTP-GET

Dans cette tâche, vous allez implémenter la version de HTTP-GET de la méthode d’action de suppression pour récupérer les informations de l’album.

1. Ouvrez le **commencer** solution situé dans **/Ex5-HandlingDeletion/début du fichierSource/** dossier. Sinon, vous pouvez continuer à utiliser le **fin** solution obtenu par le biais de l’exercice précédent.

   1. Si vous avez ouvert le fourni **commencer** solution, vous devrez télécharger certains packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.
   2. Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.

      > [!NOTE]
      > Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
2. Ouvrez **StoreManagerController** classe. Pour ce faire, développez le **contrôleurs** dossier et double-cliquez sur **StoreManagerController.cs**.
3. L’action de contrôleur de suppression est exactement le même que l’action de contrôleur Store détails précédente : elle interroge le **album** objet à partir de la base de données à l’aide de la **id** fourni dans l’URL et retourne le appropriée **vue**. Pour ce faire, remplacez la HTTP-GET **supprimer** code de méthode d’action avec les éléments suivants :

    (Code Snippet - *programmes d’assistance ASP.NET MVC 4 et de formulaires et de Validation - action Ex5 gestion suppression HTTP-GET Delete*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. Avec le bouton droit à l’intérieur de la **supprimer** méthode d’action, puis **ajouter une vue**. Cela fera apparaître la boîte de dialogue Ajouter une vue.
5. Dans la boîte de dialogue Ajouter une vue, vérifiez que le nom de la vue est **supprimer**. Sélectionnez le **créer une vue fortement typée** option et sélectionnez **Album (MvcMusicStore.Models)** à partir de la **classe de modèle** liste déroulante. Sélectionnez **supprimer** à partir de la **modèle de structure** liste déroulante. Laissez les autres champs avec leur valeur par défaut, puis **ajouter**.

    ![Ajout d’une vue Delete](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Ajout d’une vue de la suppression")

    *Ajout d’une vue de la suppression*
6. Le modèle de suppression affiche tous les champs à partir du modèle. Vous afficherez uniquement les titre d’album. Pour ce faire, remplacez le contenu de la vue par le code suivant :

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Tâche 2 : exécution de l’Application

Dans cette tâche, vous allez tester que le **StoreManager** **supprimer** afficher la page affiche un formulaire de suppression de confirmation.

1. Appuyez sur **F5** pour exécuter l’Application.
2. Le projet démarre dans la page d’accueil. Remplacez l’URL par **/StoreManager**. Sélectionnez un album à supprimer en cliquant sur **supprimer** et vérifiez que la nouvelle vue est téléchargée.

    ![Suppression d’un Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "suppression d’un Album")

    *Suppression d’un Album*

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a>Tâche 3-implémentation de la méthode d’Action Delete HTTP-POST

Dans cette tâche, vous allez implémenter la version de HTTP-POST de la méthode d’action Delete qui est appelée lorsqu’un utilisateur clique sur le **supprimer** bouton. La méthode doit supprimer l’album dans la base de données.

1. Fermez le navigateur si nécessaire, pour revenir à la fenêtre Visual Studio. Ouvrez **StoreManagerController** classe. Pour ce faire, développez le **contrôleurs** dossier et double-cliquez sur **StoreManagerController.cs**.
2. Remplacez **HTTP-POST et Delete** code de méthode d’action avec les éléments suivants :

    (Code Snippet - *programmes d’assistance ASP.NET MVC 4 et de formulaires et de Validation - action Ex5 gestion suppression HTTP-POST et Delete*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tâche 4 : exécution de l’Application

Dans cette tâche, vous allez tester que le **StoreManager Delete** page de vue vous permet de supprimer un Album, il redirige vers la vue Index StoreManager.

1. Appuyez sur **F5** pour exécuter l’Application.
2. Le projet démarre dans la page d’accueil. Remplacez l’URL par **/StoreManager**. Sélectionnez un album à supprimer en cliquant sur **supprimer.** Confirmer la suppression en cliquant sur **supprimer** bouton :

    ![Suppression d’un Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "suppression d’un Album")

    *Suppression d’un Album*
3. Vérifiez que l’album a été supprimée, car il n’apparaît pas dans le **Index** page.

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a>Exercice 6 : Ajout de la Validation

Actuellement, les formulaires de créer et modifier en place sans effectuer n’importe quel type de validation. Si l’utilisateur quitte un champ obligatoire vide ou les lettres de type dans le champ de prix, la première erreur, que vous obtenez sera effectué depuis la base de données.

Vous pouvez ajouter la validation à l’application en ajoutant des Annotations de données à votre classe de modèle. Annotations de données décrivant les règles appliquées aux propriétés de votre modèle et de ASP.NET MVC s’occupera de l’application et afficher le message approprié pour les utilisateurs.

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a>Tâche 1 : ajouter des Annotations de données

Dans cette tâche, vous allez ajouter des Annotations de données au modèle Album qui rend la page Create et Edit affiche des messages de validation lorsque cela est approprié.

Pour une classe de modèle simple, ajout d’une Annotation de données est uniquement géré en ajoutant un **à l’aide de** instruction pour **System.ComponentModel.DataAnnotation**, puis placer un **[obligatoire]** attribut sur les propriétés appropriées. L’exemple suivant rendrait la **nom** propriété un champ obligatoire dans la vue.

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

Il s’agit un peu plus complexe dans les cas de cette application où l’Entity Data Model est généré. Si vous avez ajouté des Annotations de données directement sur les classes de modèle, ils seront remplacées si vous mettez à jour le modèle à partir de la base de données. Au lieu de cela, vous pouvez rendre l’utilisation des métadonnées des classes partielles qui existera pour contenir les annotations et sont associées avec le modèle de classes à l’aide de la **[MetadataType]** attribut.

1. Ouvrez le **commencer** solution situé dans **/Ex6-AddingValidation/début du fichierSource/** dossier. Sinon, vous pouvez continuer à utiliser le **fin** solution obtenu par le biais de l’exercice précédent.

   1. Si vous avez ouvert le fourni **commencer** solution, vous devrez télécharger certains packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.
   2. Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.

      > [!NOTE]
      > Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
2. Ouvrez le **Album.cs** à partir de la **modèles** dossier.
3. Remplacez **Album.cs** de contenu avec le code en surbrillance, afin qu’il ressemble à ceci :

    > [!NOTE]
    > La ligne **[DisplayFormat(ConvertEmptyStringToNull=false)]** indique que des chaînes vides à partir du modèle ne sont pas être converties en valeurs null lorsque le champ de données est mis à jour dans la source de données. Ce paramètre permet d’éviter une exception lorsqu’Entity Framework attribue des valeurs null pour le modèle avant de l’Annotation de données valide les champs.

    (Code Snippet - *programmes d’assistance ASP.NET MVC 4 et de formulaires et de Validation - classe partielle de métadonnées Ex6 Album*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > Cela **Album** classe partielle a une **MetadataType** attribut qui pointe vers le **AlbumMetaData** classe pour les Annotations de données. Voici certains des attributs d’Annotation de données que vous utilisez pour annoter le modèle d’Album :
    > 
    > - Obligatoire - indique que la propriété est un champ obligatoire
    > - DisplayName : définit le texte à utiliser sur les champs de formulaire et les messages de validation
    > - DisplayFormat - Spécifie le mode d’affichage et de mise en forme des champs de données.
    > - StringLength - définit une longueur maximale pour un champ de chaîne
    > - Plage - donne la valeur minimale et maximale pour un champ numérique
    > - ScaffoldColumn - permet de masquer des champs de formulaires de l’éditeur

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Tâche 2 : exécution de l’Application

Dans cette tâche, vous allez tester les pages Create et Edit valident des champs, en utilisant les noms d’affichage choisis dans la dernière tâche.

1. Appuyez sur **F5** pour exécuter l’Application.
2. Le projet démarre dans la page d’accueil. Remplacez l’URL par **/StoreManager/créer**. Vérifiez que les noms d’affichage correspondent à celles de la classe partielle (tels que **Album Art URL** au lieu de **AlbumArtUrl**)
3. Cliquez sur **créer**, sans le remplir le formulaire. Vérifiez que vous obtenez les messages de validation correspondants.

    ![Validé des champs dans la page Create](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "validé des champs dans la page Create")

    *Champs validés dans la page Create*
4. Vous pouvez vérifier que s’applique également à la **modifier** page. Remplacez l’URL par **/StoreManager/Edit/1** et vérifiez que les noms d’affichage correspondent à celles de la classe partielle (tels que **Album Art URL** au lieu de **AlbumArtUrl**). Vide le **titre** et **prix** champs et cliquez sur **enregistrer**. Vérifiez que vous obtenez les messages de validation correspondants.

    ![Champs validés dans la page de modification](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    *Champs validés dans la page de modification*

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a>Exercice 7 : À l’aide de jQuery non obstructive côté Client

Dans cet exercice, vous allez apprendre à activer MVC 4 Unobtrusive validation jQuery côté client.

> [!NOTE]
> Le jQuery non obstructive utilise le préfixe de données-ajax JavaScript pour appeler des méthodes d’action sur le serveur et non intrusive émettant les scripts client.


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a>Tâche 1 : exécution de l’Application avant l’activation non obstructive jQuery

Dans cette tâche, vous allez exécuter l’application avant d’inclure jQuery pour comparer les deux modèles de validation.

1. Ouvrez le **commencer** solution situé dans **/Ex7-UnobtrusivejQueryValidation/début du fichierSource/** dossier. Sinon, vous pouvez continuer à utiliser le **fin** solution obtenu par le biais de l’exercice précédent.

   1. Si vous avez ouvert le fourni **commencer** solution, vous devrez télécharger certains packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.
   2. Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.

      > [!NOTE]
      > Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
2. Appuyez sur **F5** pour exécuter l’application.
3. Le projet démarre dans la page d’accueil. Parcourir **/StoreManager/créer** et cliquez sur **créer** sans remplir le formulaire pour vérifier que vous obtenez des messages de validation :

    ![Validation côté client désactivée](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "validation côté Client désactivée")

    *Validation côté client désactivée*
4. Dans le navigateur, ouvrez le code source HTML :

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a>Tâche 2 - Validation de Client non obstructive l’activation

Dans cette tâche, vous allez activer jQuery **validation client non obstructive** de **Web.config** fichier, qui est définie sur false dans tous les nouveaux projets ASP.NET MVC 4 par défaut. En outre, vous allez ajouter de que références pour rendre le travail de Validation Client discrète de jQuery les scripts nécessaires.

1. Ouvrez **Web.Config** de fichiers à la racine du projet et vous assurer que le **ClientValidationEnabled** et **UnobtrusiveJavaScriptEnabled** les valeurs de clés sont définies sur **true**.

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > Vous pouvez également activer la validation côté client par le code à Global.asax.cs pour obtenir les mêmes résultats :
    > 
    > **HtmlHelper.ClientValidationEnabled = true ;**
    > 
    > En outre, vous pouvez affecter ClientValidationEnabled attribut dans n’importe quel contrôleur pour avoir un comportement personnalisé.
2. Ouvrez **Create.cshtml** à **Views\StoreManager**.
3. Assurez-vous que les fichiers de script suivant, **jquery.validate** et **jquery.validate.unobtrusive**, sont référencées dans la vue les invites le &quot; **~/bundles/jqueryval** &quot; offre groupée.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > Toutes ces bibliothèques jQuery sont inclus dans les nouveaux projets de MVC 4. Vous trouverez d’autres bibliothèques dans le **/Scripts** dossier de votre projet.
    > 
    > Pour effectuer cette validation bibliothèques fonctionnent, vous devez ajouter une référence à la bibliothèque de framework de jQuery. Étant donné que cette référence est déjà ajoutée dans le  **\_Layout.cshtml** fichier, vous n’avez pas besoin de l’ajouter dans cette vue particulière.

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a>Tâche 3 : l’Application à l’aide de discrète jQuery Validation en cours d’exécution

Dans cette tâche, vous allez tester que le **StoreManager** créer la vue modèle effectue la validation côté client à l’aide des bibliothèques de jQuery lorsque l’utilisateur crée un nouvel album.

1. Appuyez sur **F5** pour exécuter l’application.
2. Le projet démarre dans la page d’accueil. Parcourir **/StoreManager/créer** et cliquez sur **créer** sans remplir le formulaire pour vérifier que vous obtenez des messages de validation :

    ![Validation côté client avec jQuery activé](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "validation côté Client avec jQuery activé")

    *Validation côté client avec jQuery activé*
3. Dans le navigateur, ouvrez le code source pour créer une vue :

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > Pour chaque règle de validation client jQuery non obstructive ajoute un attribut avec des données-val -*rulename*=&quot;*message*&quot;. Voici une liste de balises que discrète jQuery insère dans le champ d’entrée html pour effectuer la validation côté client :
   > 
   > - Données-val
   > - Numéro de données val
   > - Plage de données val
   > - Données-val-range-min / données-val-range-max
   > - Données val requises
   > - Longueur des données-val
   > - Données-val-longueur-max / données val-Longueur-min.
   > 
   > Toutes les valeurs de données sont remplis avec modèle **Annotation de données**. Ensuite, toute la logique qui fonctionne au côté serveur peut être exécutée côté client. Par exemple, attribut prix a l’annotation de données suivante dans le modèle :
   > 
   > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
   > 
   > Après avoir utilisé jQuery non obstructive, le code généré est :
   > 
   > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Récapitulatif

En fin de cet atelier pratique, que vous savez comment permettre aux utilisateurs de modifier les données stockées dans la base de données à l’aide des éléments suivants :

- Actions de contrôleur comme Index, Create, Edit, Delete
- Fonctionnalité de génération de modèles automatique de ASP.NET MVC pour l’affichage des propriétés dans un tableau HTML
- Expérience de programmes d’assistance HTML personnalisées afin d’améliorer l’utilisateur
- Méthodes d’action qui réagissent aux HTTP-GET ou d’appels HTTP-POST
- Un modèle d’éditeur partagé pour les modèles de vue similaires telles que Create et Edit
- Éléments de formulaire tels que des listes déroulantes
- Annotations de données pour la validation de modèle
- Validation côté client à l’aide de la bibliothèque jQuery non obstructive

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Annexe a : installation de Visual Studio Express 2012 pour le Web

Vous pouvez installer **Microsoft Visual Studio Express 2012 pour Web** ou un autre &quot;Express&quot; à l’aide de la version du **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Les instructions suivantes vous guident dans les étapes requises pour installer *Visual studio Express 2012 pour Web* à l’aide de *Microsoft Web Platform Installer*.

1. Accédez à [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Si vous avez déjà installé Web Platform Installer, vous pouvez également ouvrir il et recherchez le produit &quot; <em>Visual Studio Express 2012 pour le Web avec Windows Azure SDK</em>&quot;.
2. Cliquez sur **installer maintenant**. Si vous n’avez pas **Web Platform Installer** vous allez être redirigé pour télécharger et installer en premier.
3. Une fois **Web Platform Installer** est ouvert, cliquez sur **installer** pour démarrer le programme d’installation.

    ![Installer Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "installer Visual Studio Express")

    *Installer Visual Studio Express*
4. Lisez les licences et les termes du contrat de tous les produits et cliquez sur **J’accepte** pour continuer.

    ![Accepter les termes du contrat de licence](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    *Accepter les termes du contrat de licence*
5. Attendez que le processus de téléchargement et l’installation se termine.

    ![Progression de l'installation](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    *Progression de l’installation*
6. Une fois l’installation terminée, cliquez sur **Terminer**.

    ![Installation est terminée](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    *Installation est terminée*
7. Cliquez sur **Exit** pour fermer Web Platform Installer.
8. Pour ouvrir Visual Studio Express pour le Web, accédez à la **Démarrer** écran et démarrer l’écriture &quot; **VS Express**&quot;, puis cliquez sur le **Visual Studio Express pour le Web** vignette.

    ![VS Express pour une vignette de Web](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    *VS Express pour une vignette de Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Annexe b : utilisation d’extraits de Code

Avec des extraits de code, vous avez tout le code que vous avez besoin à portée de main. Le document de laboratoire vous indiquera exactement quand vous pouvez les utiliser, comme indiqué dans l’illustration suivante.

![À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "extraits de code à l’aide de Visual Studio pour insérer du code dans votre projet")

*À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet*

***Pour ajouter un extrait de code à l’aide du clavier (c# uniquement)***

1. Placez le curseur où vous souhaitez insérer le code.
2. Commencez à taper le nom de l’extrait de code (sans espaces ou des traits d’union).
3. Regarder en tant qu’IntelliSense affiche les noms des extraits correspondants.
4. Sélectionnez l’extrait de code correct (ou continuez à taper jusqu'à ce que le nom de l’extrait de code entière est sélectionnée).
5. Appuyez sur la touche Tab à deux reprises pour insérer l’extrait de code à l’emplacement du curseur.

![Commencez à taper le nom de l’extrait de code](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "commencez à taper le nom de l’extrait de code")

*Commencez à taper le nom de l’extrait de code*

![Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "appuyez sur Tab pour sélectionner l’extrait de code en surbrillance")

*Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance*

![Appuyez sur Tab à nouveau et l’extrait de code seront développe](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "appuyez sur Tab à nouveau et l’extrait de code seront développe.")

*Appuyez sur Tab à nouveau et l’extrait de code seront développe.*

***Pour ajouter un extrait de code à l’aide de la souris (c#, Visual Basic et XML)*** 1. Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code.

1. Sélectionnez **insérer un extrait** suivie **mes extraits de Code**.
2. Choisissez l’extrait de code approprié dans la liste, en cliquant dessus.

![Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait")

*Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait*

![Choisissez l’extrait de code approprié dans la liste, en cliquant dessus](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "choisir l’extrait de code approprié dans la liste, en cliquant sur celle-ci")

*Choisissez l’extrait de code approprié dans la liste, en cliquant sur celle-ci*
