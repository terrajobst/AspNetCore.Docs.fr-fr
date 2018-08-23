---
uid: mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-cs
title: 'Itération #1 : créer l’Application (c#) | Microsoft Docs'
author: microsoft
description: 'Dans la première itération, nous créons le Gestionnaire de Contact de la façon la plus simple possible. Nous ajoutons la prise en charge pour les opérations de base de données : création, lecture, mise à jour et D....'
ms.author: riande
ms.date: 02/20/2009
ms.assetid: db0f160b-901c-46d3-865e-7ab6cd4ed68d
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-cs
msc.type: authoredcontent
ms.openlocfilehash: 78b488263fbb0c646d9bf6ee8c4ace2ff63ccf9d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832594"
---
<a name="iteration-1--create-the-application-c"></a>Itération #1 : créer l’Application (c#)
====================
par [Microsoft](https://github.com/microsoft)

[Télécharger le Code](iteration-1-create-the-application-cs/_static/contactmanager_1_cs1.zip)

> Dans la première itération, nous créons le Gestionnaire de Contact de la façon la plus simple possible. Nous ajoutons la prise en charge pour les opérations de base de données : création, lecture, mise à jour et suppression (CRUD).


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Création d’une Application ASP.NET MVC de gestion des contacts (VB)

Dans cette série de didacticiels, nous créer une application de gestion des contacts entière à partir du début à la fin. L’application Gestionnaire de Contact permet vous permettent de stocker les informations de contact (noms, numéros de téléphone et adresses de messagerie) pour obtenir la liste de personnes.

Nous générer l’application sur de multiples itérations. Avec chaque itération, nous améliorer progressivement l’application. L’objectif de cette approche à plusieurs itération consiste à vous permettre de comprendre la raison de chaque modification.

- Itération #1 : créer l’application. Dans la première itération, nous créons le Gestionnaire de Contact de la façon la plus simple possible. Nous ajoutons la prise en charge pour les opérations de base de données : création, lecture, mise à jour et suppression (CRUD).

- Itération #2 : donner l’application une apparence intéressante. Dans cette itération, nous améliorer l’apparence de l’application en modifiant la valeur par défaut de page maître de vue ASP.NET MVC et en cascade de feuille de style.

- Itération #3 - Ajouter une validation de formulaire. Dans la troisième itération, nous ajouter la validation de formulaire de base. Nous empêcher des personnes à partir de l’envoi d’un formulaire sans compléter les champs obligatoires. Nous avons également valider que les adresses de messagerie et les numéros de téléphone.

- Itération #4 : rendre l’application faiblement couplée. Dans ce quatrième itération, nous profitons de plusieurs modèles de conception de logiciels pour le rendre plus facile à gérer et modifier l’application Gestionnaire de contacts. Par exemple, nous refactoriser notre application pour utiliser le modèle dépôt et le modèle d’Injection de dépendance.

- Itération #5 - créer des tests unitaires. Dans la cinquième itération, nous faciliter notre application mettre à jour et modifier en ajoutant des tests unitaires. Nous simuler nos classes de modèle de données et générer des tests unitaires pour nos contrôleurs et la logique de validation.

- Itération #6 - utiliser le développement piloté par test. Dans cette itération sixième, nous ajoutons les nouvelles fonctionnalités à notre application en écrivant des tests unitaires tout d’abord et écrire du code pour les tests unitaires. Dans cette itération, nous ajouter des groupes de contacts.

- Itération #7 - ajouter des fonctionnalités Ajax. Dans l’itération septième, nous améliorer la réactivité et les performances de notre application en ajoutant la prise en charge d’Ajax.

## <a name="this-iteration"></a>Cette itération

Dans cette première itération, nous créer l’application de base. L’objectif est de créer le Gestionnaire de Contact de la manière la plus simple et plus rapide possible. Dans les itérations ultérieures, nous améliorons la conception de l’application.

L’application Gestionnaire de contacts est une application pilotée par base de données de base. Vous pouvez utiliser l’application pour créer des contacts, modifier des contacts existants et supprimer des contacts.

Dans cette itération, nous allons terminer les étapes suivantes :

1. Application ASP.NET MVC
2. Créer une base de données pour stocker nos contacts
3. Générer une classe de modèle pour notre base de données avec Microsoft Entity Framework
4. Créer une action de contrôleur et la vue qui nous permet de répertorier tous les contacts dans la base de données
5. Créer des actions de contrôleur et une vue qui nous permet de créer un contact dans la base de données
6. Créer des actions de contrôleur et une vue qui nous permet de modifier un contact existant dans la base de données
7. Créer des actions de contrôleur et une vue qui nous permet de supprimer un contact existant dans la base de données

## <a name="software-prerequisites"></a>Composants logiciels requis

Dans les applications ASP.NET MVC, vous devez disposer de Visual Studio 2008 ou Visual Web Developer 2008 est installé sur votre ordinateur (Visual Web Developer est une version gratuite de Visual Studio qui n’inclut pas toutes les fonctionnalités avancées de Visual Studio). Vous pouvez télécharger la version d’évaluation de Visual Studio 2008 ou Visual Web Developer depuis l’adresse suivante :

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

> [!NOTE] 
> 
> Pour les applications ASP.NET MVC avec Visual Web Developer, vous devez disposer de Visual Web Developer Service Pack 1 installé. Sans Service Pack 1, Impossible de créer des projets d’Application Web.


Infrastructure ASP.NET MVC. Vous pouvez télécharger l’infrastructure ASP.NET MVC à partir de l’adresse suivante :

[https://www.asp.net/mvc](../../../index.md)

Dans ce didacticiel, nous utilisons Microsoft Entity Framework pour accéder à une base de données. Entity Framework est inclus dans .NET Framework 3.5 Service Pack 1. Vous pouvez télécharger ce service pack à partir de l’emplacement suivant :

[https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp; displaylang = en](https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en)

Comme alternative à l’exécution de chacune de ces téléchargements un par un, vous pouvez tirer parti de Web Platform Installer (Web PI). Vous pouvez télécharger Web Platform Installer à partir de l’adresse suivante :

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

## <a name="aspnet-mvc-project"></a>Projet ASP.NET MVC

Projet d’Application Web ASP.NET MVC. Lancez Visual Studio et sélectionnez l’option de menu **fichier, nouveau projet**. Le **nouveau projet** boîte de dialogue s’affiche (voir Figure 1). Sélectionnez le **Web** type de projet et le **Application Web ASP.NET MVC** modèle. Nommez votre nouveau projet *ContactManager* et cliquez sur le bouton OK.


Assurez-vous que vous disposez de .NET Framework 3.5 est sélectionné dans la liste déroulante en haut à droite de la **nouveau projet** boîte de dialogue. Sinon, le modèle d’Application Web ASP.NET MVC a gagné t s’affichent.


[![La boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image1.jpg)](iteration-1-create-the-application-cs/_static/image1.png)

**Figure 01**: boîte de dialogue Nouveau projet ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image2.png))


Application ASP.NET MVC, le **créer un projet de Test unitaire** boîte de dialogue apparaît. Vous pouvez utiliser cette boîte de dialogue pour indiquer que vous souhaitez créer et ajouter un projet de test unitaire à votre solution lorsque vous créez votre application ASP.NET MVC. Bien que nous avons gagné t être création de tests unitaires dans cette itération, vous devez sélectionner l’option **Oui, créer un projet de test unitaire** , car nous prévoyons d’ajouter des tests unitaires dans une itération ultérieure. Ajout d’un projet de Test lorsque vous créez un nouveau projet ASP.NET MVC est beaucoup plus facile que l’ajout d’un projet de Test après avoir créé le projet ASP.NET MVC.

> [!NOTE] 
> 
> Étant donné que Visual Web Developer ne prend pas en charge les projets de Test, vous n’obtenez pas la boîte de dialogue Créer un projet de Test unitaire lors de l’utilisation de Visual Web Developer.


[![La boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image2.jpg)](iteration-1-create-the-application-cs/_static/image3.png)

**Figure 02**: boîte de dialogue le projet de Test unitaire créer ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image4.png))


Application ASP.NET MVC s’affiche dans la fenêtre Explorateur de solutions Visual Studio (voir Figure 3). Si vous ne pas voir la fenêtre de l’Explorateur de solutions, puis vous pouvez ouvrir cette fenêtre en sélectionnant l’option de menu **afficher, Explorateur de solutions**. Notez que la solution contient deux projets : le projet ASP.NET MVC et le projet de Test. Le projet ASP.NET MVC est nommé ContactManager et le projet de Test nommé ContactManager.Tests.


[![La boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image3.jpg)](iteration-1-create-the-application-cs/_static/image5.png)

**Figure 03**: fenêtre de l’Explorateur de solutions ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image6.png))


## <a name="deleting-the-project-sample-files"></a>Supprimer les fichiers d’exemple de projet

Le modèle de projet ASP.NET MVC inclut des exemples de fichiers pour les contrôleurs et les vues. Avant de créer une application ASP.NET MVC, vous devez supprimer ces fichiers. Vous pouvez supprimer des fichiers et dossiers dans la fenêtre Explorateur de solutions en double-cliquant sur un fichier ou dossier, puis en sélectionnant l’option de menu **supprimer**.

Vous devez supprimer les fichiers suivants à partir du projet ASP.NET MVC :

- \Controllers\HomeController.cs

- \Views\Home\About.aspx

- \Views\Home\Index.aspx

Et bien, vous devez supprimer le fichier suivant à partir du projet de Test :

\Controllers\HomeControllerTest.cs

## <a name="creating-the-database"></a>Création de la base de données

L’application Gestionnaire de contacts est une application web pilotée par base de données. Nous utilisons une base de données pour stocker les informations de contact.

L’infrastructure ASP.NET MVC avec une base de données moderne, y compris les bases de données Microsoft SQL Server, Oracle, MySQL et IBM DB2. Dans ce didacticiel, nous utilisons une base de données Microsoft SQL Server. Lorsque vous installez Visual Studio, vous sont fournis avec l’option d’installation de Microsoft SQL Server Express est une version gratuite de la base de données Microsoft SQL Server.

Créer une base de données en double-cliquant sur l’application\_dossier de données dans la fenêtre Explorateur de solutions et en sélectionnant l’option de menu **ajouter, nouvel élément**. Dans le **ajouter un nouvel élément** boîte de dialogue, sélectionnez le **données** catégorie et le **base de données SQL Server** modèle (voir Figure 4). Nommez la nouvelle base de données ContactManagerDB.mdf et cliquez sur le bouton OK.


[![La boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image4.jpg)](iteration-1-create-the-application-cs/_static/image7.png)

**Figure 04**: création d’une nouvelle base de données Microsoft SQL Server Express ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image8.png))


Après avoir créé la nouvelle base de données, la base de données s’affiche dans l’application\_dossier de données dans la fenêtre de l’Explorateur de solutions. Double-cliquez sur le fichier ContactManager.mdf pour ouvrir la fenêtre Explorateur de serveurs et de se connecter à la base de données.

> [!NOTE] 
> 
> La fenêtre Explorateur de serveurs est appelée la fenêtre Explorateur de base de données dans le cas de Microsoft Visual Web Developer.


Vous pouvez utiliser la fenêtre Explorateur de serveurs pour créer des objets de base de données tels que les tables de base de données, des vues, des déclencheurs et des procédures stockées. Cliquez sur le dossier Tables et sélectionnez l’option de menu **ajouter une nouvelle Table**. Le Concepteur de tables de base de données s’affiche (voir Figure 5).


[![La boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image5.jpg)](iteration-1-create-the-application-cs/_static/image9.png)

**Figure 05**: le Concepteur de tables de base de données ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image10.png))


Nous devons créer une table qui contient les colonnes suivantes :

<a id="0.1_table01"></a>


| **Nom de colonne** | **Type de données** | **Autoriser les valeurs null** |
| --- | --- | --- |
| Id | int | False |
| FirstName | nvarchar (50) | False |
| LastName | nvarchar (50) | False |
| Téléphone | nvarchar (50) | False |
| Messagerie | nvarchar (255) | False |


La première colonne, la colonne Id, est spéciale. Vous devez marquer la colonne Id comme une colonne d’identité et une colonne de clé primaire. Vous indiquez qu’une colonne est une colonne d’identité en développant des propriétés de colonne (coup de œil au bas de la Figure 6) et en faisant défiler jusqu'à la propriété de la spécification d’identité. Définir le **(est d’identité)** valeur à la propriété **Oui**.

Vous marquez une colonne comme colonne clé primaire en sélectionnant la colonne et en cliquant sur le bouton avec l’icône d’une clé. Une fois une colonne est marquée comme une colonne de clé primaire, une icône d’une clé s’affiche en regard de la colonne (voir Figure 6).

Une fois que vous avez terminé la création de la table, cliquez sur le bouton Enregistrer (le bouton avec une icône de disquette) pour enregistrer la nouvelle table. Nommez votre nouvelle table *Contacts*.

Après la fin de création de la table de base de données de Contacts, vous devez ajouter des enregistrements à la table. Avec le bouton droit de la table de Contacts dans la fenêtre Explorateur de serveurs, puis sélectionnez l’option de menu **afficher les données de Table**. Entrez un ou plusieurs contacts dans la grille qui s’affiche.

## <a name="creating-the-data-model"></a>Création du modèle de données

L’application ASP.NET MVC se compose des modèles, vues et contrôleurs. Nous commençons par créer une classe de modèle qui représente la table Contacts que nous avons créé dans la section précédente.

Dans ce didacticiel, nous utilisons Microsoft Entity Framework pour générer une classe de modèle à partir de la base de données automatiquement.

> [!NOTE] 
> 
> L’infrastructure ASP.NET MVC n’est pas lié à Microsoft Entity Framework en aucune façon. Vous pouvez utiliser ASP.NET MVC avec les technologies d’accès de base de données alternative notamment NHibernate, LINQ to SQL ou ADO.NET.


Suivez ces étapes pour créer les classes de modèle de données :

1. Cliquez sur le dossier de modèles dans la fenêtre Explorateur de solutions et sélectionnez **ajouter, nouvel élément**. Le **ajouter un nouvel élément** boîte de dialogue s’affiche (voir Figure 6).
2. Sélectionnez le **données** catégorie et le **ADO.NET Entity Data Model** modèle. Nommez votre modèle de données *ContactManagerModel.edmx* et cliquez sur le **ajouter** bouton. L’Assistant Entity Data Model s’affiche (voir la Figure 7).
3. Dans le **choisir le contenu du modèle** étape, sélectionnez **générer à partir de la base de données** (voir la Figure 7).
4. Dans le **choisir votre connexion de données** étape, sélectionnez la base de données ContactManagerDB.mdf et entrez le nom *ContactManagerDBEntities* pour les paramètres de connexion d’entité (voir Figure 8).
5. Dans le **choisir vos objets de base de données** étape, activez la case à cocher Tables (voir la Figure 9). Le modèle de données inclura toutes les tables contenues dans votre base de données (il y a qu’un, la table de Contacts). Entrez l’espace de noms *modèles*. Cliquez sur le bouton Terminer pour terminer l’Assistant.


[![La boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image6.jpg)](iteration-1-create-the-application-cs/_static/image11.png)

**Figure 06**: The ajouter un nouvel élément de boîte de dialogue ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image12.png))


[![La boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image7.jpg)](iteration-1-create-the-application-cs/_static/image13.png)

**Figure 07**: choisir le contenu du modèle ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image14.png))


[![La boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image8.jpg)](iteration-1-create-the-application-cs/_static/image15.png)

**Figure 08**: choisir votre connexion de données ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image16.png))


[![La boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image9.jpg)](iteration-1-create-the-application-cs/_static/image17.png)

**Figure 09**: choisir vos objets de base de données ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image18.png))


Après avoir terminé l’Assistant Entity Data Model, Entity Data Model Designer s’affiche. Le concepteur affiche une classe qui correspond à chaque table en cours de modélisation. Vous devez voir une classe nommée Contacts.

L’Assistant Entity Data Model génère des noms de classe en fonction des noms de table de base de données. Vous devez presque toujours modifier le nom de la classe générée par l’Assistant. Avec le bouton droit de la classe de Contacts dans le concepteur et sélectionnez l’option de menu **renommer**. Modifier le nom de la classe à partir des Contacts (plurielles) contact (singulier). Une fois que vous modifiez le nom de classe, la classe doit apparaître comme la Figure 10.


[![La boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image10.jpg)](iteration-1-create-the-application-cs/_static/image19.png)

**Figure 10**: classe le Contact ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image20.png))


À ce stade, nous avons créé notre modèle de base de données. Nous pouvons utiliser la classe Contact pour représenter un enregistrement de contact particulière dans notre base de données.

## <a name="creating-the-home-controller"></a>Créer le contrôleur Home

L’étape suivante consiste à créer notre contrôleur Home. Le contrôleur Home est le contrôleur par défaut appelé dans une application ASP.NET MVC.

Créer la classe de contrôleur d’accueil en double-cliquant sur le dossier contrôleurs dans la fenêtre Explorateur de solutions et en sélectionnant l’option de menu **ajouter, de contrôleur** (voir Figure 11). Notez que la case à cocher **ajouter des méthodes d’action pour les scénarios Create, Update et Details**. Assurez-vous que cette case à cocher est cochée avant de cliquer sur le **ajouter** bouton.


[![La boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image11.jpg)](iteration-1-create-the-application-cs/_static/image21.png)

**Figure 11**: ajout du contrôleur d’accueil ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image22.png))


Lorsque vous créez le contrôleur Home, vous obtenez la classe dans le Listing 1.

**Liste 1 - Controllers\HomeController.cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample1.cs)]

## <a name="listing-the-contacts"></a>Liste des Contacts

Pour afficher les enregistrements dans la table de base de données de Contacts, nous devons créer une action Index() et une vue Index.

Le contrôleur Home contient déjà une action Index(). Nous devons modifier cette méthode afin qu’elle ressemble Listing 2.

**Listing 2 - Controllers\HomeController.cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample2.cs)]

Notez que la classe de contrôleur d’accueil dans le Listing 2 contient un champ privé nommé \_entités. Le \_champ d’entités représente les entités du modèle de données. Nous utilisons le \_entités champ pour communiquer avec la base de données.

La méthode Index() retourne une vue qui représente tous les contacts de la table de base de données de Contacts. L’expression \_entités. ContactSet.ToList() retourne la liste de contacts sous la forme d’une liste générique.

Maintenant que nous avons ve créé le contrôleur de l’Index, nous devons ensuite créer la vue Index. Avant de créer la vue Index, compilez votre application en sélectionnant l’option de menu **créer, générer la Solution**. Vous devez toujours compiler votre projet avant d’ajouter une vue dans l’ordre de la liste des classes de modèle à afficher dans le **ajouter une vue** boîte de dialogue.

Vous créez la vue Index en double-cliquant sur la méthode Index() et en sélectionnant l’option de menu **ajouter une vue** (voir Figure 12). Cette option de menu ouvre le **ajouter une vue** boîte de dialogue (voir Figure 13).


[![La boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image12.jpg)](iteration-1-create-the-application-cs/_static/image23.png)

**Figure 12**: ajout de la vue Index ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image24.png))


Dans le **ajouter une vue** boîte de dialogue, cochez la case intitulée **créer une vue fortement typée**. Sélectionnez la classe de données d’affichage ContactManager.Models.Contact et la liste de contenu de vue. Sélection de ces options génère une vue qui affiche une liste d’enregistrements de Contact.


[![La boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image13.jpg)](iteration-1-create-the-application-cs/_static/image25.png)

**Figure 13**: boîte de dialogue Ajouter une vue The ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image26.png))


Lorsque vous cliquez sur le **ajouter** bouton, la vue Index dans la liste 3 est généré. Notez que le &lt;% @ Page %&gt; directive qui apparaît en haut du fichier. La vue Index hérite le ViewPage&lt;IEnumerable&lt;ContactManager.Models.Contact&gt; &gt; classe. En d’autres termes, la classe de modèle dans la vue représente une liste d’entités de Contact.

Le corps de la vue Index contient une boucle foreach qui itère au sein de chaque contact représenté par la classe de modèle. La valeur de chaque propriété de la classe de Contact s’affiche dans une table HTML.

**Liste 3 - Views\Home\Index.aspx (non modifié)**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample3.aspx)]

Nous devons apporter une modification à la vue Index. Étant donné que nous n’allons pas créer une vue de détails, nous pouvons supprimer le lien Détails. Recherchez et supprimez le code suivant à partir de la vue Index :

{id = l’élément. % D’ID})&gt;

Après avoir modifié la vue Index, vous pouvez exécuter l’application Gestionnaire de contacts. Sélectionnez l’option de menu Débogage, démarrer le débogage, ou appuyez simplement sur F5. La première fois que vous exécutez l’application, vous obtenez la boîte de dialogue dans la Figure 14. Sélectionnez l’option **modifier le fichier Web.config pour activer le débogage** et cliquez sur le bouton OK.


[![La boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image14.jpg)](iteration-1-create-the-application-cs/_static/image27.png)

**Figure 14**: activation du débogage ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image28.png))


La vue de l’Index est retournée par défaut. Cette vue répertorie toutes les données à partir de la table de base de données de Contacts (voir Figure 15).


[![La boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image15.jpg)](iteration-1-create-the-application-cs/_static/image29.png)

**Figure 15**: affichage de l’Index ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image30.png))


Notez que la vue Index inclut un lien intitulé créer un nouveau en bas de la vue. Dans la section suivante, vous allez apprendre à créer des contacts.

## <a name="creating-new-contacts"></a>Création de nouveaux Contacts

Pour permettre aux utilisateurs de créer de nouveaux contacts, nous devons ajouter deux actions Create() au contrôleur Home. Nous devons créer une action Create() qui retourne un formulaire HTML pour la création d’un contact. Nous devons créer une deuxième action Create() qui effectue l’insertion de base de données réelle du nouveau contact.

Les nouvelles méthodes Create() que nous devons ajouter au contrôleur d’accueil sont contenus dans la liste 4.

**Liste 4 - Controllers\HomeController.cs (avec des méthodes de création)**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample4.cs)]

La première méthode Create() peut être appelée avec un verbe HTTP GET, tandis que la deuxième méthode Create() peut être appelée que par une requête HTTP POST. En d’autres termes, la deuxième méthode Create() peut être appelée uniquement lors de la validation d’un formulaire HTML. La première méthode Create() retourne simplement une vue qui contient le formulaire HTML pour la création d’un contact. La deuxième méthode Create() est beaucoup plus intéressante : il ajoute le nouveau contact à la base de données.

Notez que la deuxième méthode Create() a été modifiée pour accepter une instance de la classe Contact. Les valeurs de formulaire publiées à partir du formulaire HTML sont liés automatiquement à cette classe de Contact par l’infrastructure ASP.NET MVC. Chaque champ de formulaire à partir du formulaire HTML créer est affectée à une propriété du paramètre de Contact.

Notez que le paramètre de Contact est décoré avec un attribut [Bind]. L’attribut [Bind] est utilisé pour exclure la propriété Id de Contact à partir de la liaison. Étant donné que la propriété Id représente une propriété Identity, nous ne souhaitez définir la propriété Id.

Dans le corps de la méthode Create(), Entity Framework est utilisé pour insérer le nouveau Contact dans la base de données. Le nouveau Contact est ajouté à l’ensemble existant de Contacts et la méthode SaveChanges() est appelée pour remettre ces modifications à la base de données sous-jacente.

Vous pouvez générer un formulaire HTML pour la création de nouveaux Contacts en double-cliquant sur une des deux méthodes Create() et en sélectionnant l’option de menu **ajouter une vue** (voir Figure 16).


[![La boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image16.jpg)](iteration-1-create-the-application-cs/_static/image31.png)

**Figure 16**: ajout de la vue Create ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image32.png))


Dans le **ajouter une vue** boîte de dialogue, sélectionnez le **ContactManager.Models.Contact** classe et le **créer** option pour afficher le contenu (voir Figure 17). Lorsque vous cliquez sur le **ajouter** bouton, une création de vue est générée automatiquement.


[![La boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image17.jpg)](iteration-1-create-the-application-cs/_static/image33.png)

**Figure 17**: voyez une page explode ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image34.png))


La vue Create contient des champs de formulaire pour chacune des propriétés de la classe Contact. Le code de la vue Create est inclus dans la liste 5.

**Liste 5 - Views\Home\Create.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample5.aspx)]

Une fois que vous modifiez les méthodes Create() et ajoutez la vue Create, vous pouvez exécuter l’application Gestionnaire de contacts et créer de nouveaux contacts. Cliquez sur le **créer un nouveau** lien qui apparaît dans la vue Index pour accéder à la vue de créer. Vous devez voir la vue dans la Figure 18.


[![La boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image18.jpg)](iteration-1-create-the-application-cs/_static/image35.png)

**Figure 18**: The Create View ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image36.png))


## <a name="editing-contacts"></a>Modification de Contacts

Ajout de la fonctionnalité pour la modification d’un enregistrement de contact est très similaire à l’ajout de la fonctionnalité de création de nouveaux enregistrements de contact. Tout d’abord, nous devons ajouter deux nouvelles méthodes de modification à la classe de contrôleur Home. Ces nouvelles méthodes Edit() sont contenus dans la liste 6.

**Liste 6 - Controllers\HomeController.cs (avec les méthodes de modification)**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample6.cs)]

La première méthode Edit() est appelée par une opération HTTP GET. Un paramètre Id est passé à cette méthode qui représente l’Id de l’enregistrement de contact en cours de modification. Entity Framework permet de récupérer un contact qui correspond à l’Id. Une vue qui contient un formulaire HTML pour modifier un enregistrement est retournée.

La deuxième méthode Edit() effectue la mise à jour réelle à la base de données. Cette méthode accepte une instance de la classe Contact en tant que paramètre. L’infrastructure ASP.NET MVC lie les champs de formulaire à partir du formulaire de modification à cette classe automatiquement. Notez que vous ne pas inclure l’attribut [Bind] lors de la modification d’un Contact (nous avons besoin de la valeur de la propriété d’Id).

Entity Framework est utilisé pour enregistrer le Contact modifié dans la base de données. Le Contact d’origine doit tout d’abord être récupéré à partir de la base de données. Ensuite, la méthode Entity Framework ApplyPropertyChanges() est appelée pour enregistrer les modifications apportées au Contact. Enfin, la méthode Entity Framework SaveChanges() est appelée pour rendre persistantes les modifications apportées à la base de données sous-jacente.

Vous pouvez générer la vue qui contient le formulaire de modification en double-cliquant sur la méthode Edit() et en sélectionnant l’option de menu Vue d’ajouter. Dans la boîte de dialogue Ajouter une vue, sélectionnez le **ContactManager.Models.Contact** classe et le **modifier** afficher le contenu (voir Figure 19).


[![La boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image19.jpg)](iteration-1-create-the-application-cs/_static/image37.png)

**Figure 19**: ajout d’une vue Modifier ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image38.png))


Lorsque vous cliquez sur le bouton Ajouter, une nouvelle vue de modification est générée automatiquement. Le formulaire HTML généré contient des champs qui correspondent à chacune des propriétés de la classe Contact (voir la liste 7).

**Liste 7 - Views\Home\Edit.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample7.aspx)]

## <a name="deleting-contacts"></a>Supprimer des Contacts

Si vous souhaitez supprimer des contacts, vous devez ajouter deux actions Delete() à la classe de contrôleur Home. La première action Delete() affiche un formulaire de confirmation de suppression. La deuxième action Delete() effectue la suppression réelle.

> [!NOTE] 
> 
> Plus tard, dans l’itération #7, nous modifions le Gestionnaire de contacts afin qu’il prend en charge une étape supprimer Ajax.


Les deux nouvelles méthodes Delete() sont contenus dans la liste 8.

**Liste 8 - Controllers\HomeController.cs (méthodes de suppression)**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample8.cs)]

La première méthode Delete() retourne un écran de confirmation de suppression d’un enregistrement de contact à partir de la base de données (voir Figure20). La deuxième méthode Delete() effectue l’opération de suppression réelle par rapport à la base de données. Une fois que le contact d’origine a été récupéré à partir de la base de données, les méthodes Entity Framework DeleteObject() et SaveChanges() sont appelées pour effectuer la suppression de la base de données.


[![La boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image20.jpg)](iteration-1-create-the-application-cs/_static/image39.png)

**Figure 20**: la vue de confirmation de suppression ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image40.png))


Nous devons modifier la vue Index afin qu’il contienne un lien pour la suppression des enregistrements de contact (voir Figure 21). Vous devez ajouter le code suivant à la même cellule du tableau qui contient le lien d’édition :

Html.ActionLink ({id = l’élément. % D’ID})&gt;


[![La boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image21.jpg)](iteration-1-create-the-application-cs/_static/image41.png)

**Figure 21**: vue avec le lien Modifier d’Index ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image42.png))


Ensuite, nous devons créer la vue de confirmation de suppression. Avec le bouton droit de la méthode Delete() dans la classe de contrôleur d’accueil et sélectionnez l’option de menu vue de l’ajouter. La boîte de dialogue Ajouter une vue s’affiche (voir Figure 22).

Contrairement à dans le cas des liste, créer et modifier des vues, la boîte de dialogue Ajouter une vue ne contient pas une option pour créer une vue de la suppression. Au lieu de cela, sélectionnez le **ContactManager.Models.Contact** classe de données et la **vide** afficher le contenu. En sélectionnant la vue vide option contenue va nécessiter la créer la vue nous-mêmes.


[![La boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image22.jpg)](iteration-1-create-the-application-cs/_static/image43.png)

**Figure 22**: ajout de la vue de confirmation de suppression ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image44.png))


Le contenu de la vue Delete est contenu dans la liste 9. Cette vue contient un formulaire qui confirme ou non un contact particulier doit être supprimé (voir Figure 21).

**Liste 9 - Views\Home\Delete.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample9.aspx)]

## <a name="changing-the-name-of-the-default-controller"></a>Modification du nom du contrôleur par défaut

Il peut vous dérange pas que le nom de notre classe de contrôleur pour l’utilisation des contacts est nommé la classe HomeController. Le contrôleur de nommer les t ne doit pas ContactController ?

Ce problème est assez facile à résoudre. Tout d’abord, nous devons refactoriser le nom du contrôleur Home. Ouvrez la classe HomeController dans l’éditeur de Code Visual Studio, cliquez avec le bouton droit sur le nom de la classe et sélectionnez l’option de menu **refactoriser, changement de nom**. Cette option de menu ouvre la boîte de dialogue de changement de nom.


[![La boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image23.jpg)](iteration-1-create-the-application-cs/_static/image45.png)

**Figure 23**: un nom de contrôleur de refactorisation ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image46.png))


[![La boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image24.jpg)](iteration-1-create-the-application-cs/_static/image47.png)

**Figure 24**: à l’aide de la boîte de dialogue Renommer ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image48.png))


Si vous renommez votre classe de contrôleur, Visual Studio met à jour le nom du dossier dans le dossier des vues. Visual Studio renomme le dossier \Views\Home dans le dossier \Views\Contact.

Après avoir apporté cette modification, votre application n’a plus un contrôleur Home. Lorsque vous exécutez votre application, vous obtiendrez la page d’erreur dans la Figure 25.


[![La boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image25.jpg)](iteration-1-create-the-application-cs/_static/image49.png)

**Figure 25**: aucun contrôleur par défaut ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image50.png))


Nous devons mettre à jour de l’itinéraire par défaut dans le fichier Global.asax pour utiliser le contrôleur de Contact au lieu du contrôleur Home. Ouvrez le fichier Global.asax et modifier le contrôleur par défaut utilisé par l’itinéraire par défaut (voir la liste 10).

**Liste 10 - Global.asax.cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample10.cs)]

Après avoir apporté ces modifications, le Gestionnaire de contacts s’exécutent correctement. À présent, il utilise la classe de contrôleur de Contact en tant que le contrôleur par défaut.

## <a name="summary"></a>Récapitulatif

Dans cette première itération, nous avons créé une application de gestionnaire de contacts de base dans le moyen le plus rapide possible. Nous avons tiré parti de Visual Studio pour générer le code initial pour nos contrôleurs et les vues automatiquement. Nous avons également parti d’Entity Framework pour générer automatiquement des nos classes de modèle de base de données.

Actuellement, nous pouvons répertorier, créer, modifier et supprimer des enregistrements de contact avec l’application Gestionnaire de Contact. En d’autres termes, nous pouvons effectuer toutes les opérations de base de données requises par une application web pilotée par base de données.

Malheureusement, notre application pose certains problèmes. Premier et je hésitent admettez que cela, l’application Gestionnaire de contacts n’est pas l’application plus attrayante. Il a besoin d’un travail de conception. Dans l’itération suivante, nous allons examiner comment nous pouvons modifier la page maître de vue par défaut et la feuille de style en cascade pour améliorer l’apparence de l’application.

Deuxièmement, nous n'avons pas implémenté aucune validation de formulaire. Par exemple, il n’est rien ne vous empêche d’envoyer le formulaire de contact de création sans entrer des valeurs pour les champs de formulaire. En outre, vous pouvez entrer des adresses de messagerie et les numéros de téléphone non valide. Nous commençons à attaquer au problème de validation de formulaire dans l’itération #3.

Enfin et surtout, l’itération actuelle de l’application Gestionnaire de contacts ne peut pas être facilement modifiée ou maintenue. Par exemple, la logique d’accès de base de données est intégrée de droite dans les actions de contrôleur. Cela signifie que nous ne pouvons pas modifier notre code d’accès aux données sans modifier nos contrôleurs. Dans les itérations ultérieures, nous explorons les modèles de conception de logiciels que nous pouvons implémenter pour améliorer la résilience pour modifier le Gestionnaire de contacts.

> [!div class="step-by-step"]
> [Next](iteration-2-make-the-application-look-nice-cs.md)
