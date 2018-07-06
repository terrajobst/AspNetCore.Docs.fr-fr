---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
title: Bien démarrer avec Entity Framework 4.0 Database First et 4 d’ASP.NET Web Forms - partie 6 | Microsoft Docs
author: tdykstra
description: L’exemple d’application web Contoso University montre comment créer des applications Web Forms ASP.NET à l’aide d’Entity Framework. L’exemple d’application est en cours...
ms.author: aspnetcontent
ms.date: 12/03/2010
ms.assetid: 994a5496-c648-4830-b03c-55bb43f325d2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
msc.type: authoredcontent
ms.openlocfilehash: 1f0a0f050f74b41d6e33a406dfcf7a760c81321b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817432"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-6"></a>Bien démarrer avec Entity Framework 4.0 Database First et 4 les Web Forms ASP.NET - partie 6
====================
par [Tom Dykstra](https://github.com/tdykstra)

> L’exemple d’application web Contoso University montre comment créer des applications Web Forms ASP.NET à l’aide de l’Entity Framework 4.0 et Visual Studio 2010. Pour plus d’informations sur la série de didacticiels, consultez [le premier didacticiel de la série](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="implementing-table-per-hierarchy-inheritance"></a>Implémentation de l'héritage TPH (table par hiérarchie)

Dans le didacticiel précédent vous a travaillé avec les données associées en ajoutant et supprimant des relations et en ajoutant une nouvelle entité ayant une relation à une entité existante. Ce didacticiel vous indiquera comment implémenter l’héritage dans le modèle de données.

Dans la programmation orientée objet, vous pouvez utiliser l’héritage pour le rendre plus facile de travailler avec les classes connexes. Par exemple, vous pouvez créer `Instructor` et `Student` classes qui dérivent d’un `Person` classe de base. Vous pouvez créer les mêmes types de structures d’héritage entre les entités dans Entity Framework.

Dans cette partie du didacticiel, vous ne créez toutes les nouvelles pages web. Au lieu de cela, vous ajoutez des entités dérivées au modèle de données et modifier des pages existantes pour utiliser les nouvelles entités.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Table par hiérarchie par rapport à l’héritage Table par Type

Une base de données peut stocker des informations sur les objets connexes dans une table ou dans plusieurs tables. Par exemple, dans le `School` base de données, le `Person` table inclut des informations sur les étudiants et formateurs dans une table unique. Certaines colonnes s’appliquent uniquement aux formateurs (`HireDate`), certaines uniquement aux étudiants (`EnrollmentDate`) et autres aux deux (`LastName`, `FirstName`).

[![image11](the-entity-framework-and-aspnet-getting-started-part-6/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image1.png)

Vous pouvez configurer Entity Framework pour créer `Instructor` et `Student` entités qui héritent de la `Person` entité. Ce modèle de génération d’une structure de l’héritage d’entité à partir d’une table de base de données unique est appelé *table par hiérarchie* l’héritage (TPH).

Pour les cours, le `School` base de données utilise un modèle différent. Cours en ligne et des cours sur site sont stockés dans des tables distinctes, chacune d’elles a une clé étrangère qui pointe vers le `Course` table. Informations communes aux deux types de cours sont stockées uniquement dans le `Course` table.

[![image12](the-entity-framework-and-aspnet-getting-started-part-6/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image3.png)

Vous pouvez configurer le modèle de données Entity Framework afin que `OnlineCourse` et `OnsiteCourse` entités héritent le `Course` entité. Ce modèle de génération d’une structure de l’héritage d’entité à partir des tables distinctes pour chaque type, avec chaque table distincte référant à une table qui stocke les données communes à tous les types, est appelé *table par type* l’héritage (TPT).

Modèles d’héritage TPH fournissent généralement de meilleures performances dans Entity Framework que les modèles d’héritage TPT, étant donné que les modèles TPT peuvent entraîner dans les requêtes de jointure complexe. Cette procédure pas à pas montre comment implémenter l’héritage TPH. Vous pouvez utiliser en effectuant les étapes suivantes :

- Créer `Instructor` et `Student` types d’entités qui dérivent de `Person`.
- Propriétés de déplacement qui se rapportent aux entités dérivées de la `Person` entité pour les entités dérivées.
- Définir des contraintes sur les propriétés dans les types dérivés.
- Rendre le `Person` entité une entité abstraite.
- Carte chaque dérivée d’entité à la `Person` table avec une condition qui spécifie comment déterminer si un `Person` représente ce type dérivé de la ligne.

## <a name="adding-instructor-and-student-entities"></a>Ajout d’entités Instructor et Student

Ouvrez le <em>SchoolModel.edmx</em> de fichier, avec le bouton droit dans le concepteur, sélectionnez une zone inoccupée <strong>ajouter</strong>, puis sélectionnez <strong>entité</strong><em>.</em>

[![image01](the-entity-framework-and-aspnet-getting-started-part-6/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image5.png)

Dans le **ajouter une entité** boîte de dialogue, nom de l’entité `Instructor` et définissez son **type de Base** option `Person`.

[![image02](the-entity-framework-and-aspnet-getting-started-part-6/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image7.png)

Cliquez sur **OK**. Le concepteur crée un `Instructor` entité dérive le `Person` entité. La nouvelle entité n’a pas encore de toutes les propriétés.

[![image03](the-entity-framework-and-aspnet-getting-started-part-6/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image9.png)

Répétez la procédure pour créer un `Student` entité dérive également de `Person`.

Uniquement les formateurs ont des dates d’embauche, donc vous devez déplacer cette propriété à partir de la `Person` entité à la `Instructor` entité. Dans le `Person` entité, avec le bouton droit le `HireDate` propriété et cliquez sur **couper**. Avec le bouton droit puis **propriétés** dans le `Instructor` entité et cliquez sur **coller**.

[![image04](the-entity-framework-and-aspnet-getting-started-part-6/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image11.png)

La date d’embauche un `Instructor` entité ne peut pas être null. Avec le bouton droit le `HireDate` propriété, cliquez sur **propriétés**, puis, dans le **propriétés** modification de la fenêtre `Nullable` à `False`.

[![image05](the-entity-framework-and-aspnet-getting-started-part-6/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image13.png)

Répétez la procédure pour déplacer le `EnrollmentDate` propriété à partir de la `Person` entité à la `Student` entité. Veillez à définir également `Nullable` à `False` pour le `EnrollmentDate` propriété.

Maintenant qu’un `Person` entité dispose uniquement des propriétés qui sont communes aux `Instructor` et `Student` entités (à part les propriétés de navigation, ce qui vous ne déplacez pas), l’entité peut uniquement servir comme entité de base dans la structure d’héritage. Par conséquent, vous devez vous assurer qu’elle n’est jamais traitée comme une entité indépendante. Avec le bouton droit le `Person` entité, sélectionnez **propriétés**, puis, dans le **propriétés** fenêtre Modifier la valeur de la **abstraite** propriété  **True**.

[![image06](the-entity-framework-and-aspnet-getting-started-part-6/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image15.png)

## <a name="mapping-instructor-and-student-entities-to-the-person-table"></a>Mappage des entités Instructor et Student pour la Table Person

Maintenant, vous devez indiquer à Entity Framework comment faire la distinction entre `Instructor` et `Student` entités dans la base de données.

Cliquez sur le `Instructor` entité, puis sélectionnez **mappage de Table**. Dans le **détails de Mapping** fenêtre, cliquez sur **ajouter une Table ou vue** et sélectionnez **personne**.

[![image07](the-entity-framework-and-aspnet-getting-started-part-6/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image17.png)

Cliquez sur **ajouter une Condition**, puis sélectionnez **HireDate**.

[![image09](the-entity-framework-and-aspnet-getting-started-part-6/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image19.png)

Modification **opérateur** à **est** et **valeur / propriété** à **pas Null**.

[![image10](the-entity-framework-and-aspnet-getting-started-part-6/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image21.png)

Répétez la procédure pour le `Students` entité, en spécifiant que cette entité est mappée à la `Person` table lorsque la `EnrollmentDate` colonne n’est pas null. Ensuite, enregistrez et fermez le modèle de données.

Générez le projet pour créer les nouvelles entités en tant que classes et de les rendre disponibles dans le concepteur.

## <a name="using-the-instructor-and-student-entities"></a>À l’aide des entités Instructor et Student

Lorsque vous avez créé les pages web qui fonctionnent avec des données student et instructor, liés aux données vous leur le `Person` du jeu d’entités, et que vous avez filtré sur la `HireDate` ou `EnrollmentDate` propriété pour restreindre les données retournées pour les étudiants ou enseignants. Toutefois, maintenant lorsque vous liez chaque contrôle de source de données à la `Person` du jeu d’entités, vous pouvez spécifier que seules `Student` ou `Instructor` types d’entité doivent être sélectionnés. Étant donné que l’Entity Framework sait comment différencier les étudiants et formateurs dans le `Person` jeu d’entités, vous pouvez supprimer le `Where` paramètres de propriété que vous avez entré manuellement pour ce faire.

Dans le concepteur Visual Studio, vous pouvez spécifier le type de l’entité un `EntityDataSource` contrôle doit sélectionner dans la **EntityTypeFilter** zone de liste déroulante de la `Configure Data Source` Assistant, comme indiqué dans l’exemple suivant.

[![image13](the-entity-framework-and-aspnet-getting-started-part-6/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image23.png)

Puis, dans le **propriétés** fenêtre que vous pouvez supprimer `Where` valeurs de la clause qui ne sont plus nécessaires, comme indiqué dans l’exemple suivant.

[![image14](the-entity-framework-and-aspnet-getting-started-part-6/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image25.png)

Toutefois, étant donné que vous avez modifié le balisage de `EntityDataSource` contrôles à utiliser le `ContextTypeName` attribut, vous ne pouvez pas exécuter le **configurer la Source de données** Assistant sur `EntityDataSource` contrôles que vous avez déjà créé. Par conséquent, vous apporterez des modifications requises en modifiant le balisage à la place.

Ouvrez le *Students.aspx* page. Dans le `StudentsEntityDataSource` contrôler, supprimez le `Where` d’attribut et d’ajouter un `EntityTypeFilter="Student"` attribut. Les marques ressembleront maintenant l’exemple suivant :

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample1.aspx)]

Définition de la `EntityTypeFilter` attribut garantit que le `EntityDataSource` contrôle sélectionne uniquement le type d’entité spécifié. Si vous souhaitez récupérer les deux `Student` et `Instructor` types d’entité, vous devez définir pas cet attribut. (Vous avez la possibilité de récupération de plusieurs types d’entités avec une `EntityDataSource` contrôler uniquement si vous utilisez le contrôle pour accéder aux données en lecture seule. Si vous utilisez un `EntityDataSource` contrôler pour insérer, mettre à jour ou supprimer des entités et si elle est liée à l’ensemble d’entités peut contenir plusieurs types, vous pouvez uniquement travailler avec un type d’entité, et vous devez définir cet attribut.)

Répétez la procédure pour le `SearchEntityDataSource` contrôler, sauf la suppression uniquement la partie de la `Where` attribut sélectionne `Student` entités au lieu de supprimer complètement de la propriété. La balise d’ouverture du contrôle ressemblera maintenant à l’exemple suivant :

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample2.aspx)]

Exécutez la page pour vérifier qu’elle fonctionne toujours comme avant.

[![image15](the-entity-framework-and-aspnet-getting-started-part-6/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image27.png)

Mettre à jour les pages suivantes que vous avez créé dans les didacticiels précédents afin qu’ils utilisent le nouveau `Student` et `Instructor` entités au lieu de `Person` entités, puis les exécuter pour vérifier qu’elles fonctionnent comme auparavant :

- Dans *StudentsAdd.aspx*, ajouter `EntityTypeFilter="Student"` à la `StudentsEntityDataSource` contrôle. Les marques ressembleront maintenant l’exemple suivant : 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample3.aspx)]

    [![image16](the-entity-framework-and-aspnet-getting-started-part-6/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image29.png)
- Dans *About.aspx*, ajouter `EntityTypeFilter="Student"` à la `StudentStatisticsEntityDataSource` contrôler et supprimer `Where="it.EnrollmentDate is not null"`. Les marques ressembleront maintenant l’exemple suivant : 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample4.aspx)]

    [![image17](the-entity-framework-and-aspnet-getting-started-part-6/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image31.png)
- Dans *Instructors.aspx* et *InstructorsCourses.aspx*, ajouter `EntityTypeFilter="Instructor"` à la `InstructorsEntityDataSource` contrôler et supprimer `Where="it.HireDate is not null"`. Le balisage dans *Instructors.aspx* ressemble maintenant à l’exemple suivant : 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample5.aspx)]

    [![image18](the-entity-framework-and-aspnet-getting-started-part-6/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image33.png)

    Le balisage dans *InstructorsCourses.aspx* seront maintenant ressembler à l’exemple suivant :

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample6.aspx)]

    [![image19](the-entity-framework-and-aspnet-getting-started-part-6/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image35.png)

À la suite de ces modifications, vous avez amélioré la facilité de gestion de l’application Contoso University de plusieurs façons. Vous avez déplacé la logique de sélection et validation en dehors de la couche d’interface utilisateur (*.aspx* markup) et le rendre une partie intégrante de la couche d’accès aux données. Cela permet d’isoler le code de votre application contre les modifications que vous pouvez apporter à l’avenir pour le schéma de base de données ou le modèle de données. Par exemple, vous pouvez décider que les étudiants peuvent être engagés comme aides de professeurs et par conséquent obtiendriez une date d’embauche. Vous pouvez ensuite ajouter une nouvelle propriété pour différencier les étudiants des formateurs et mettre à jour le modèle de données. Aucun code dans l’application web ne doit remplacer, sauf lorsque vous souhaitez afficher une date d’embauche pour les étudiants. Un autre avantage de l’ajout `Instructor` et `Student` entités est que votre code est plus facilement compréhensible que lorsqu’il agit de `Person` les objets qui ont été réellement les étudiants ou enseignants.

Vous avez maintenant vu une manière d’implémenter un modèle d’héritage dans Entity Framework. Dans ce didacticiel, vous allez apprendre à utiliser des procédures stockées afin de mieux contrôler la façon dont Entity Framework accède à la base de données.

> [!div class="step-by-step"]
> [Précédent](the-entity-framework-and-aspnet-getting-started-part-5.md)
> [Suivant](the-entity-framework-and-aspnet-getting-started-part-7.md)
