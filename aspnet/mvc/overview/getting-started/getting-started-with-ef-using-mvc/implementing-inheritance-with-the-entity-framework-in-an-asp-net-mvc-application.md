---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Modèle : Implémenter l’héritage avec Entity Framework dans des applications ASP.NET MVC 5'
description: Ce didacticiel vous indiquera comment implémenter l’héritage dans le modèle de données.
author: tdykstra
ms.author: riande
ms.date: 01/21/2019
ms.topic: tutorial
ms.assetid: 08834147-77ec-454a-bb7a-d931d2a40dab
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 79513edce7ac3044f6f547149400cba7d307edfa
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889893"
---
# <a name="template-implement-inheritance-with-ef-in-an-aspnet-mvc-5-app"></a>Modèle : Implémenter l’héritage avec Entity Framework dans une application ASP.NET MVC 5

Dans le didacticiel précédent, vous avez géré les exceptions d’accès concurrentiel. Ce didacticiel vous indiquera comment implémenter l’héritage dans le modèle de données.

Dans la programmation orientée objet, vous pouvez utiliser [héritage](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) pour faciliter la [réutilisation du code](http://en.wikipedia.org/wiki/Code_reuse). Dans ce didacticiel, vous allez modifier les classes `Instructor` et `Student` afin qu’elles dérivent d’une classe de base `Person` qui contient des propriétés telles que `LastName`, communes aux formateurs et aux étudiants. Vous n’ajouterez ni ne modifierez aucune page web, mais vous modifierez une partie du code et ces modifications seront automatiquement répercutées dans la base de données.

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Apprenez à mapper l’héritage à base de données
> * Créer la classe Person
> * Student et Instructor de mise à jour
> * Ajouter une personne au modèle
> * Créer et mettre à jour les Migrations
> * Tester l’implémentation
> * Déployer sur Azure

## <a name="prerequisites"></a>Prérequis

* [Implémentation de l’héritage](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="map-inheritance-to-database"></a>Mapper l’héritage à base de données

Le `Instructor` et `Student` classes dans le `School` modèle de données ont plusieurs propriétés qui sont identiques :

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

Supposons que vous souhaitez éliminer le code redondant pour les propriétés partagées par les entités `Instructor` et `Student`. Ou vous souhaitez écrire un service capable de mettre en forme les noms sans se soucier du fait que le nom provienne d’un formateur ou d’un étudiant. Vous pouvez créer un `Person` classe de base qui contient uniquement les propriétés partagées, puis apportez les `Instructor` et `Student` entités héritent de cette classe de base, comme indiqué dans l’illustration suivante :

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Il existe plusieurs façons de représenter cette structure d’héritage dans la base de données. Vous pourriez avoir un `Person` table contenant des informations sur les étudiants et formateurs dans une table unique. Certaines colonnes pourraient s’appliquent uniquement aux formateurs (`HireDate`), certaines uniquement aux étudiants (`EnrollmentDate`), certaines aux deux (`LastName`, `FirstName`). En règle générale, vous devriez un *discriminateur* colonne afin d’indiquer les types sur lesquels chaque ligne représente. Par exemple, la colonne de discriminateur peut avoir « Instructor » pour les formateurs et « Student » pour les étudiants.

![Table-par-hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Ce modèle de génération d’une structure de l’héritage d’entité à partir d’une table de base de données unique est appelé *table par hiérarchie* l’héritage (TPH).

Une alternative consiste à faire en sorte que la base de données ressemble plus à la structure d’héritage. Par exemple, vous pouvez avoir uniquement les champs de nom dans la `Person` table et avez distinct `Instructor` et `Student` tables avec les champs de date.

![Table-par-type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Ce modèle consistant à créer une table de base de données pour chaque classe d’entité est appelé *table par type* l’héritage (TPT).

Une autre option encore consiste à mapper tous les types non abstraits à des tables individuelles. Toutes les propriétés d’une classe, y compris les propriétés héritées, sont mappées aux colonnes de la table correspondante. Ce modèle porte le nom d’héritage TPC (table par classe concrète). Si vous avez implémenté l’héritage TPC pour le `Person`, `Student`, et `Instructor` classes comme indiqué précédemment, le `Student` et `Instructor` tables seraient pas différentes après l’implémentation de l’héritage auparavant.

TPC et modèles d’héritage TPH fournissent généralement de meilleures performances dans Entity Framework que les modèles d’héritage TPT, étant donné que les modèles TPT peuvent entraîner dans les requêtes de jointure complexe.

Ce didacticiel montre comment implémenter l’héritage TPH. TPH est le modèle d’héritage dans Entity Framework, par conséquent, il vous suffit est de créer un `Person` de classe, de modifier le `Instructor` et `Student` classes de dériver de `Person`, ajouter la nouvelle classe à la `DbContext`et créer un migration. (Pour plus d’informations sur la façon d’implémenter les autres modèles d’héritage, consultez [mappage de l’héritage Table par Type (TPT)](https://msdn.microsoft.com/data/jj591617#2.5) et [mappage de l’héritage de classe de Table par concret (TPC)](https://msdn.microsoft.com/data/jj591617#2.6) dans MSDN Documentation d’Entity Framework.)

## <a name="create-the-person-class"></a>Créer la classe Person

Dans le *modèles* dossier, créez *Person.cs* et remplacez le code du modèle par le code suivant :

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="update-instructor-and-student"></a>Student et Instructor de mise à jour

Maintenant mettre à jour le *Instructor.cs* et *Sudent.cs* à hériter des valeurs à partir de la *Person.sc*.

Dans *Instructor.cs*, dériver le `Instructor` classe à partir de la `Person` classe et supprimez les champs de clé et de nom. Le code ressemblera à l’exemple suivant :

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Apporter des modifications similaires à *Student.cs*. Le `Student` classe ressemblera à l’exemple suivant :

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-person-to-the-model"></a>Ajouter une personne au modèle

Dans *SchoolContext.cs*, ajoutez un `DbSet` propriété pour le `Person` type d’entité :

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

C’est là tout ce dont Entity Framework a besoin pour configurer l’héritage TPH (table par hiérarchie). Comme vous le verrez, lorsque la base de données est mis à jour, elle aura une `Person` table à la place de la `Student` et `Instructor` tables.

## <a name="create-and-update-migrations"></a>Créer et mettre à jour les Migrations

Dans le Package Manager de la console, entrez la commande suivante :

`Add-Migration Inheritance`

Exécuter le `Update-Database` dans PMC. La commande échoue à ce stade, car nous avons migrations ne sait pas comment gérer les données existantes. Vous obtenez un message d’erreur semblable à celui-ci :

> *Impossible de supprimer l’objet ' dbo. Formateur ', car il est référencé par une contrainte FOREIGN KEY.*


Ouvrez *Migrations\&lt ; horodatage&gt;\_Inheritance.cs* et remplacez le `Up` méthode avec le code suivant :

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

Ce code prend en charge les tâches de mise à jour de base de données suivantes :

- Supprime les contraintes de clé étrangère et les index qui pointent vers la table Student.
- Renomme la table Instructor en Person et apporte les modifications nécessaires pour qu’elle stocke les données des étudiants :

    - Ajoute une EnrollmentDate nullable pour les étudiants.
    - Ajoute la colonne Discriminator pour indiquer si une ligne est pour un étudiant ou un formateur.
    - Rend HireDate nullable étant donné que les lignes d’étudiant n’ont pas de dates d’embauche.
    - Ajoute un champ temporaire qui sera utilisé pour mettre à jour les clés étrangères qui pointent vers les étudiants. Lorsque vous copiez des étudiants dans la table Person, ils obtiendront nouvelles valeurs de clé primaire.
- Copie des données à partir de la table Student dans la table Person. Cela entraîne l’affectation de nouvelles valeurs de clés primaires aux étudiants.
- Corrige les valeurs de clés étrangères qui pointent vers les étudiants.
- Crée de nouveau les index et les contraintes de clé étrangère, désormais pointées vers la table Person.

(Si vous aviez utilisé un GUID à la place d’un entier comme type de clé primaire, les valeurs des clés primaires des étudiants n’auraient pas changé, et plusieurs de ces étapes auraient pu être omises.)

Exécutez le `update-database` réexécutez la commande.

(Dans un système de production vous fera modifications correspondantes à la méthode vers le bas dans le cas où vous auriez à utiliser pour revenir à la version précédente de la base de données. Pour ce didacticiel vous n’utiliserez pas la méthode vers le bas.)

> [!NOTE]
> Il est possible d’obtenir d’autres erreurs lors de la migration des données et apporter des modifications de schéma. Si vous obtenez des erreurs de migration vous ne pouvez pas résoudre, vous pouvez poursuivre le didacticiel en modifiant la chaîne de connexion dans le *Web.config* de fichiers ou en supprimant la base de données. L’approche la plus simple consiste à renommer la base de données dans le *Web.config* fichier. Par exemple, modifier le nom de la base de données par contosouniversity2 comme indiqué dans l’exemple suivant :
>
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
>
> Avec une base de données, il n’existe pas de données à migrer et le `update-database` commande est beaucoup plus susceptible de s’exécuter sans erreur. Pour obtenir des instructions sur la suppression de la base de données, consultez [comment supprimer une base de données à partir de Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). Si vous adoptez cette approche pour poursuivre le didacticiel, ignorez l’étape de déploiement à la fin de ce didacticiel, ou déployer un nouveau site et de la base de données. Si vous déployez une mise à jour sur le même site que vous avez été le déploiement déjà, EF obtiendra la même erreur il lorsqu’elle s’exécute automatiquement migrations. Si vous souhaitez corriger une erreur de migrations, la meilleure ressource est un des forums d’Entity Framework ou StackOverflow.com.

## <a name="test-the-implementation"></a>Tester l’implémentation

Exécutez le site et essayez différentes pages. Tout fonctionne comme avant.

Dans **Explorateur de serveurs,** développez **données Connections\SchoolContext** , puis **Tables**, et vous pouvez constater que le **étudiant** et **Instructor** tables ont été remplacés par un **personne** table. Développez le **personne** table et que vous consultez qu’il dispose de toutes les colonnes utilisées pour se trouver dans le **étudiant** et **Instructor** tables.

Cliquez avec le bouton droit sur la table Person, puis cliquez sur **Afficher les données de la table** pour voir la colonne de discriminateur.

Le diagramme suivant illustre la structure de la nouvelle base de données School :

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a>Déployer sur Azure

Cette section vous oblige à terminées facultatif **déploiement de l’application vers Azure** section [partie 3, tri, filtrage et pagination](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) de cette série de didacticiels. Si vous aviez des erreurs de migrations que vous avez résolu en supprimant la base de données dans votre projet local, ignorez cette étape ; ou créer un nouveau site et la base de données et les déployer vers le nouvel environnement.

1. Dans Visual Studio, cliquez sur le projet dans **l’Explorateur de solutions** et sélectionnez **publier** dans le menu contextuel.

2. Cliquez sur **Publier**.

    L’application Web s’ouvre dans votre navigateur par défaut.

3. Tester l’application afin de vérifier qu’il fonctionne.

    La première fois que vous exécutez une page qui accède à la base de données, Entity Framework s’exécute toutes les migrations `Up` méthodes requis pour mettre à jour avec le modèle de données actuel de la base de données.

## <a name="get-the-code"></a>Obtenir le code

[Télécharger le projet terminé](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Ressources supplémentaires

Vous trouverez des liens vers d’autres ressources Entity Framework dans le [accès aux données ASP.NET - ressources recommandées](../../../../whitepapers/aspnet-data-access-content-map.md).

Pour plus d’informations sur cela et d’autres structures de l’héritage, consultez [modèle d’héritage TPT](https://msdn.microsoft.com/data/jj618293) et [modèle d’héritage TPH](https://msdn.microsoft.com/data/jj618292) sur MSDN. Dans le prochain didacticiel, vous allez apprendre à gérer divers scénarios Entity Framework relativement avancés.

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Appris à mapper l’héritage à base de données
> * Création de la classe Person
> * Student et Instructor mis à jour
> * Personne ajoutée au modèle
> * Créé et mettre à jour les Migrations
> * Testé l’implémentation
> * Déployé sur Azure

Passez à l’article suivant pour en savoir plus sur les sujets qui sont utiles à connaître lorsque vous allez au-delà les principes fondamentaux du développement d’applications web ASP.NET qui utilisent Entity Framework Code First.
> [!div class="nextstepaction"]
> [Scénarios Entity Framework avancés](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)