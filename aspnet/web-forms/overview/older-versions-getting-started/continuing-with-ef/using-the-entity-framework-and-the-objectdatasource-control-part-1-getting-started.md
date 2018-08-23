---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
title: 'À l’aide d’Entity Framework 4.0 et le contrôle ObjectDataSource, partie 1 : mise en route | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels s’appuie sur l’application web Contoso University créé par la mise en route avec la série de didacticiels Entity Framework. Si yo...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 244278c1-fec8-4255-8a8a-13bde491c4f5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
msc.type: authoredcontent
ms.openlocfilehash: 5eaeaa0aa474e1aed86954e6c10dd1703b938944
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831432"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-1-getting-started"></a>À l’aide d’Entity Framework 4.0 et le contrôle ObjectDataSource, partie 1 : mise en route
====================
par [Tom Dykstra](https://github.com/tdykstra)

> Cette série de didacticiels s’appuie sur l’application web Contoso University créé par le [mise en route avec Entity Framework 4.0](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) série de didacticiels. Si vous n’avez pas effectué les didacticiels précédents, comme point de départ pour ce didacticiel vous pouvez [télécharger l’application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que vous l’auriez créée. Vous pouvez également [télécharger l’application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) qui est créé par la série de didacticiels terminée.
> 
> L’exemple d’application web Contoso University montre comment créer des applications Web Forms ASP.NET à l’aide de l’Entity Framework 4.0 et Visual Studio 2010. L’exemple d’application est un site Web pour une université Contoso fictive. Il inclut des fonctionnalités telles que l'admission d’étudiant, la création de cours et les affectations de formateur.
> 
> Le didacticiel présente des exemples en c#. Le [exemple téléchargeable](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) contient le code en c# et Visual Basic.
> 
> ## <a name="database-first"></a>Tout d’abord la base de données
> 
> Il existe trois façons, vous pouvez travailler avec des données dans Entity Framework : *Database First*, *Model First*, et *Code First*. Ce didacticiel s’adresse la première base de données. Pour plus d’informations sur les différences entre ces flux de travail et des conseils sur la façon de choisir le mieux adapté pour votre scénario, consultez [Workflows de développement Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>Web Forms
> 
> Comme avec la série mise en route, cette série de didacticiels utilise le modèle Web Forms ASP.NET et suppose que vous savez comment travailler avec ASP.NET Web Forms dans Visual Studio. Si vous ne faites pas, consultez [bien démarrer avec Web Forms ASP.NET 4.5](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Si vous préférez travailler avec l’infrastructure ASP.NET MVC, consultez [mise en route avec Entity Framework à l’aide d’ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Versions des logiciels
> 
> | **Indiqué dans le didacticiel** | **Fonctionne également avec** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express pour le Web. Le didacticiel n’a pas été testé avec les versions ultérieures de Visual Studio. Il existe de nombreuses différences dans les sélections de menu, boîtes de dialogue et des modèles. |
> | .NET 4 | .NET 4.5 est à compatibilité descendante avec le .NET 4, mais le didacticiel n’a pas été testé avec le .NET 4.5. |
> | Entity Framework 4 | Le didacticiel n’a pas été testé avec les versions ultérieures d’Entity Framework. À compter d’Entity Framework 5, EF utilise par défaut le `DbContext API` qui a été introduite avec Entity Framework 4.1. Le contrôle EntityDataSource a été conçu pour utiliser le `ObjectContext` API. Pour plus d’informations sur l’utilisation du contrôle EntityDataSource contrôler avec le `DbContext` API, consultez [ce billet de blog](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Questions
> 
> Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les publier à le [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), le [Entity Framework et LINQ au forum d’entités](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), ou [ StackOverflow.com](http://stackoverflow.com/).


Le `EntityDataSource` contrôle vous permet de créer une application très rapidement, mais il nécessite en général vous permet de conserver une quantité significative de logique métier et logique d’accès aux données dans votre *.aspx* pages. Si vous pensez que votre application à croître en complexité et requièrent une maintenance, vous pouvez investir plus de temps de développement afin de créer un *multiniveau* ou *en couches* structure de l’application C’est plus facile à gérer. Pour implémenter cette architecture, vous séparez la couche de présentation à partir de la couche de logique métier (BLL) et la couche d’accès aux données (DAL). Une manière d’implémenter cette structure consiste à utiliser le `ObjectDataSource` contrôler au lieu du `EntityDataSource` contrôle. Lorsque vous utilisez le `ObjectDataSource` contrôle, vous implémentez votre propre code d’accès aux données, puis l’appeler dans *.aspx* pages à l’aide d’un contrôle comportant de nombreux du même fonctionnalités que les autres contrôles de source de données. Cela vous permet de combiner les avantages d’une approche à n niveaux avec les avantages de l’utilisation d’un contrôle Web Forms pour accéder aux données.

Le `ObjectDataSource` contrôle offre plus de souplesse dans d’autres façons. Vous écrivez votre propre code d’accès aux données, il est plus facile de plus que lire, insérer, mettre à jour ou supprimer un type d’entité spécifique, qui sont les tâches qui le `EntityDataSource` contrôle vise à effectuer. Par exemple, vous pouvez effectuer la journalisation de chaque mise à jour d’une entité, archiver les données chaque fois qu’une entité est supprimée, ou automatiquement vérification et mise à jour des données liées en fonction des besoins lors de l’insertion d’une ligne avec une valeur de clé étrangère.

## <a name="business-logic-and-repository-classes"></a>Logique métier et les Classes du référentiel

Un `ObjectDataSource` fonctionnement du contrôle en appelant une classe que vous créez. La classe inclut des méthodes qui récupèrent et mettre à jour des données, et vous fournir les noms de ces méthodes pour la `ObjectDataSource` contrôle dans le balisage. Au cours de rendu ou de traitement de publication (postback), le `ObjectDataSource` appelle les méthodes que vous avez spécifié.

Outre les opérations CRUD de base, la classe que vous créez pour l’utiliser avec le `ObjectDataSource` contrôle devrez peut-être exécuter la logique métier lors de la `ObjectDataSource` lit ou met à jour des données. Par exemple, lorsque vous mettez à jour un service, vous devrez peut-être valider que sans autres départements ont le même administrateur, car une personne ne peut pas être administrateur de plus d’un service.

Dans certains `ObjectDataSource` documentation, tel que le [vue d’ensemble de la classe de l’ObjectDataSource](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.aspx), le contrôle appelle une classe appelée un *objet métier* qui inclut une logique métier et logique d’accès aux données . Dans ce didacticiel, vous allez créer des classes distinctes pour la logique métier et logique d’accès aux données. La classe qui encapsule la logique d’accès aux données est appelée un *référentiel*. La classe de logique métier inclut à la fois des méthodes de logique métier et des méthodes d’accès aux données, mais les méthodes d’accès aux données appellent le référentiel pour effectuer des tâches d’accès aux données.

Vous allez également créer une couche d’abstraction entre votre couche de logique métier et la couche DAL qui facilite d’unités automatisés de test de la couche BLL. Cette couche d’abstraction est implémentée par la création d’une interface et à l’aide de l’interface lorsque vous instanciez le référentiel dans la classe de logique métier. Cela rend possible pour fournir la logique métier de classe avec une référence à n’importe quel objet qui implémente l’interface de référentiel. Pour un fonctionnement normal, vous fournissez un objet de référentiel qui fonctionne avec Entity Framework. Pour le test, vous fournissez un objet de référentiel qui fonctionne avec les données stockées d’une manière qui vous pouvez de manipuler facilement, tels que des variables de classe définis en tant que collections.

L’illustration suivante montre la différence entre une classe de logique métier qui inclut la logique d’accès aux données sans un référentiel et une qui utilise un référentiel.

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image1.png)

Vous allez commencer en créant des pages web dans lequel le `ObjectDataSource` contrôle est lié directement à un référentiel, car elle effectue uniquement des tâches de base d’accès aux données. Dans le didacticiel suivant vous créerez une classe de logique métier avec la logique de validation et lier le `ObjectDataSource` contrôle à cette classe à la place de la classe de référentiel. Vous allez également créer des tests unitaires pour la logique de validation. Dans le troisième didacticiel de cette série vous allez ajouter le tri et filtrage des fonctionnalités à l’application.

Les pages que vous créez dans ce didacticiel fonctionnent avec le `Departments` jeu d’entités du modèle de données que vous avez créé dans le [série de didacticiels mise en route](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md).

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image3.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image5.png)

## <a name="updating-the-database-and-the-data-model"></a>La mise à jour de la base de données et le modèle de données

Vous allez commencer ce didacticiel en apportant des modifications de deux à la base de données, qui nécessitent des modifications correspondantes au modèle de données que vous avez créé dans le [mise en route avec Entity Framework et Web Forms](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) didacticiels. Dans un de ces didacticiels, vous avez modifié dans le concepteur manuellement pour synchroniser le modèle de données avec la base de données après une modification de la base de données. Dans ce didacticiel, vous allez utiliser le concepteur **mettre à jour de base de données Model** outil pour mettre à jour le modèle de données automatiquement.

### <a name="adding-a-relationship-to-the-database"></a>Ajout d’une relation à la base de données

Dans Visual Studio, ouvrez l’application web Contoso University vous avez créé dans le [mise en route avec Entity Framework et Web Forms](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) série de didacticiels, puis ouvrez le `SchoolDiagram` diagramme de base de données.

Si vous examinez le `Department` table dans le diagramme de base de données, vous verrez qu’il a un `Administrator` colonne. Cette colonne est une clé étrangère pour la `Person` table, mais aucune relation de clé étrangère est définie dans la base de données. Vous devez créer la relation et mettre à jour le modèle de données afin qu’Entity Framework puisse gérer automatiquement cette relation.

Dans le diagramme de base de données, cliquez sur le `Department` table, puis sélectionnez **relations**.

[![Image80](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image7.png)

Dans le **relations de clé étrangère** boîte, cliquez sur **ajouter**, puis cliquez sur les points de suspension pour **spécification de Tables et colonnes**.

[![Image81](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image9.png)

Dans le **Tables et colonnes** boîte de dialogue zone, définissez la table de clé primaire et un champ pour `Person` et `PersonID`et définir la table de clé étrangère et un champ pour `Department` et `Administrator`. (Lorsque vous effectuez cette opération, le nom de relation pourra `FK_Department_Department` à `FK_Department_Person`.)

[![Image82](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image11.png)

Cliquez sur **OK** dans le **Tables et colonnes** , cliquez sur **fermer** dans le **relations de clé étrangère** zone, puis enregistrez les modifications. Si vous êtes invité si vous souhaitez enregistrer le `Person` et `Department` tables, cliquez sur **Oui**.

> [!NOTE]
> Si vous avez supprimé `Person` les lignes qui correspondent aux données figurant déjà dans la `Administrator` colonne, vous ne pourrez enregistrer cette modification. Dans ce cas, utilisez l’éditeur de table dans **Explorateur de serveurs** pour vous assurer que le `Administrator` valeur dans chaque `Department` ligne contient l’ID d’un enregistrement existe réellement dans le `Person` table.
> 
> Une fois que vous enregistrez la modification, vous ne pourrez pas supprimer une ligne à partir de la `Person` si cette personne est un administrateur de service de table. Dans une application de production, vous fourniriez un message d’erreur spécifique lorsqu’une contrainte de base de données empêche une suppression, ou vous devez spécifier une suppression en cascade. Pour obtenir un exemple montrant comment spécifier une suppression en cascade, consultez [Entity Framework et ASP.NET – mise en route partie 2](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2.md).


### <a name="adding-a-view-to-the-database"></a>Ajout d’une vue à la base de données

Dans le nouveau *Departments.aspx* page que vous allez créer, que vous souhaitez fournir une liste déroulante des formateurs, avec des noms au format « nom, prénom » afin que les utilisateurs peuvent sélectionner des administrateurs de service. Pour simplifier les opérations pour ce faire, vous allez créer une vue dans la base de données. La vue se compose de simplement les données nécessaires à la liste déroulante : le nom complet (correct) et la clé d’enregistrement.

Dans **Explorateur de serveurs**, développez *School.mdf*, avec le bouton droit le **vues** dossier, puis sélectionnez **ajouter une nouvelle vue**.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image13.png)

Cliquez sur **fermer** lorsque le **ajouter une Table** boîte de dialogue s’affiche et collez l’instruction SQL suivante dans le volet SQL :

[!code-sql[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample1.sql)]

Enregistrez la vue en tant que `vInstructorName`.

### <a name="updating-the-data-model"></a>La mise à jour le modèle de données

Dans le *DAL* dossier, ouvrez le *SchoolModel.edmx* de fichiers, cliquez sur l’aire de conception et sélectionnez **modèle de mise à jour à partir de la base de données**.

[![Image07](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image15.png)

Dans le **choisir vos objets de base de données** boîte de dialogue, sélectionnez le **ajouter** onglet et sélectionnez la vue que vous venez de créer.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image17.png)

Cliquez sur **Terminer**.

Dans le concepteur, vous voyez que l’outil créé une `vInstructorName` entité et une nouvelle association entre le `Department` et `Person` entités.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image19.png)

> [!NOTE]
> Dans le **sortie** et **liste d’erreurs** windows, vous pouvez voir un message d’avertissement vous informant que l’outil créé automatiquement un principal de clé pour le nouveau `vInstructorName` vue. Ce comportement est normal.


Lorsque vous faites référence au nouveau `vInstructorName` entité dans le code, que vous souhaitez utiliser la convention de base de données d’en ajoutant le préfixe « v » en minuscules à celui-ci. Par conséquent, vous allez renommer l’entité et un jeu d’entités dans le modèle.

Ouvrez le **navigateur de modèle**. Vous voyez `vInstructorName` répertorié comme un type d’entité et une vue.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image21.png)

Sous **SchoolModel** (pas **SchoolModel.Store**), avec le bouton droit **vInstructorName** et sélectionnez **propriétés**. Dans le **propriétés** fenêtre, modification le **nom** à la propriété « InstructorName » et de modifier le **Nom_jeu_entités** propriété à « InstructorNames ».

[![Image15](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image24.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image23.png)

Enregistrez et fermez le modèle de données et puis régénérez le projet.

## <a name="using-a-repository-class-and-an-objectdatasource-control"></a>À l’aide d’une classe de référentiel et un contrôle ObjectDataSource

Créer un nouveau fichier de classe dans le *DAL* dossier, nommez-le *SchoolRepository.cs*et remplacez le code existant par le code suivant :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample2.cs)]

Ce code fournit un seul `GetDepartments` méthode qui retourne toutes les entités dans le `Departments` jeu d’entités. Étant donné que vous savez que vous voulez accéder le `Person` propriété de navigation pour chaque ligne retournée, vous spécifiez le chargement hâtif pour cette propriété à l’aide de la `Include` (méthode). La classe implémente également le `IDisposable` interface pour vous assurer que la connexion de base de données est libérée lorsque l’objet est supprimé.

> [!NOTE]
> Une pratique courante consiste à créer une classe de référentiel pour chaque type d’entité. Dans ce didacticiel, une classe de référentiel pour plusieurs types d’entité est utilisée. Pour plus d’informations sur le modèle de référentiel, consultez les billets dans [blog de l’équipe Entity Framework](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) et [blog de Julie](http://thedatafarm.com/blog/data-access/agile-ef4-repository-part-3-fine-tuning-the-repository/).


Le `GetDepartments` méthode retourne un `IEnumerable` objet plutôt qu’un `IQueryable` objet afin de garantir que la collection retournée est utilisable, même après la suppression de l’objet de référentiel lui-même. Un `IQueryable` objet peut entraîner la base de données chaque fois qu’il est accessible, mais l’objet de référentiel peut être supprimé au moment où un contrôle lié aux données tente d’afficher les données. Vous pouvez retourner un autre type de collection, comme un `IList` au lieu de l’objet une `IEnumerable` objet. Toutefois, en retournant un `IEnumerable` objet permet de s’assurer que vous pouvez effectuer les tâches de traitement typique liste en lecture seule comme `foreach` boucles et les requêtes LINQ, mais vous ne pouvez pas ajouter ou supprimer des éléments dans la collection, ce qui implique que ces modifications serait rendues persistantes dans la base de données.

Créer un *Departments.aspx* page qui utilise le *Site.Master* page maître, puis ajoutez le balisage suivant dans le `Content` contrôle nommé `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample3.aspx)]

Ce balisage crée un `ObjectDataSource` contrôle qui utilise la classe de dépôt que vous venez de créer, et un `GridView` contrôle pour afficher les données. Le `GridView` contrôle spécifie **modifier** et **supprimer** commandes, mais vous n’avez pas ajouté du code pour prendre en charge encore.

Utilisent plusieurs colonnes `DynamicField` contrôle afin que vous pouvez tirer parti des fonctionnalités de mise en forme et de validation automatique des données. Dans ce cas fonctionne, vous devez appeler la `EnableDynamicData` méthode dans le `Page_Init` Gestionnaire d’événements. (`DynamicControl` contrôles ne sont pas utilisés dans le `Administrator` champ, car ils ne fonctionnent pas avec les propriétés de navigation.)

Le `Vertical-Align="Top"` attributs sera importants plus tard lorsque vous ajoutez une colonne qui comprend un bloc imbriqué `GridView` contrôle à la grille.

Ouvrez le *Departments.aspx.cs* fichier et ajoutez le code suivant `using` instruction :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample4.cs)]

Puis ajoutez le gestionnaire suivant de la page `Init` événement :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample5.cs)]

Dans le *DAL* dossier, créez un fichier de classe nommé *Department.cs* et remplacez le code existant par le code suivant :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample6.cs)]

Ce code ajoute des métadonnées au modèle de données. Il spécifie que le `Budget` propriété de la `Department` entité représente réellement la devise bien que son type de données est `Decimal`, et il indique que la valeur doit être comprise entre 0 et format 1,000,000.00 $. Il spécifie également que le `StartDate` propriété doit être au format de date dans le format mm/jj/aaaa.

Exécutez le *Departments.aspx* page.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image26.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image25.png)

Notez que bien que vous n’avez pas spécifié une chaîne de format dans le *Departments.aspx* balisage de page pour le **Budget** ou **Start Date** devise et date par défaut des colonnes, mise en forme a été appliquée à eux à la `DynamicField` contrôle, à l’aide de métadonnées que vous avez fournies dans le *Department.cs* fichier.

## <a name="adding-insert-and-delete-functionality"></a>Ajout d’insertion et de fonctionnalités de suppression

Ouvrez *SchoolRepository.cs*, ajoutez le code suivant pour créer un `Insert` (méthode) et un `Delete` (méthode). Le code inclut également une méthode nommée `GenerateDepartmentID` qui calcule la valeur de clé suivante enregistrement disponible pour une utilisation par le `Insert` (méthode). Cela est nécessaire car la base de données n’est pas configuré pour calculer automatiquement pour le `Department` table.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample7.cs)]

### <a name="the-attach-method"></a>La méthode d’attachement

Le `DeleteDepartment` les appels de méthode le `Attach` méthode afin de rétablir le lien est conservé dans le Gestionnaire d’état objet du contexte de l’objet entre l’entité dans la mémoire et de la base de données de ligne qu’il représente. Cela doit se produire avant les appels de méthode le `SaveChanges` (méthode).

Le terme *contexte de l’objet* fait référence à la classe Entity Framework qui dérive de la `ObjectContext` classe que vous utilisez pour accéder à vos jeux d’entités et les entités. Dans le code pour ce projet, la classe est nommée `SchoolEntities`, et une instance de celle-ci est toujours nommée `context`. Le contexte d’objet *Gestionnaire d’état objet* est une classe qui dérive de la `ObjectStateManager` classe. Le contact de l’objet utilise le Gestionnaire d’état objet pour stocker les objets d’entité et de savoir si chacun d’eux est synchronisée avec sa ligne correspondante de la table ou les lignes dans la base de données.

Lorsque vous lisez une entité, le contexte de l’objet stocke dans le Gestionnaire d’état objet et effectue le suivi de cette représentation de l’objet soit synchronisée avec la base de données. Par exemple, si vous modifiez une valeur de propriété, un indicateur est défini pour indiquer que la propriété que vous avez modifié n’est plus synchronisée avec la base de données. Ensuite, quand vous appelez le `SaveChanges` (méthode), le contexte d’objet sait quoi faire dans la base de données, car le Gestionnaire d’état objet sait exactement quelle est la différence entre l’état actuel de l’entité et l’état de la base de données.

Toutefois, ce processus en général, ne fonctionne pas dans une application web, car l’instance de contexte d’objet qui lit une entité, ainsi que tous les éléments de son gestionnaire d’état objet, est supprimé après le rendu d’une page. L’instance de contexte d’objet qui doit appliquer les modifications est une autre qui est instanciée pour le traitement de publication (postback). Dans le cas de la `DeleteDepartment` (méthode), le `ObjectDataSource` contrôle ré-crée la version d’origine de l’entité à partir de valeurs de l’état dans la vue, mais ceci est recréé `Department` entité n’existe pas dans le Gestionnaire d’état objet. Si vous avez appelé la `DeleteObject` méthode sur cette entité recréée, l’appel échoue car le contexte d’objet ne sait pas si l’entité est synchronisée avec la base de données. Toutefois, l’appel la `Attach` méthode établit à nouveau le même suivi entre l’entité recréée et les valeurs dans la base de données qui a été initialement généré automatiquement quand l’entité a été lue dans une instance antérieure de contexte de l’objet.

Il est parfois lorsque vous ne souhaitez pas le contexte de l’objet pour effectuer le suivi des entités dans le Gestionnaire d’état objet, et vous pouvez définir des indicateurs pour l’empêcher d’effectuer cette opération. Exemples de ce sont présentés dans les didacticiels plus loin dans cette série.

### <a name="the-savechanges-method"></a>La méthode SaveChanges

Cette classe de référentiel simple illustre les principes fondamentaux de la façon d’effectuer des opérations CRUD. Dans cet exemple, le `SaveChanges` méthode est appelée immédiatement après chaque mise à jour. Dans une application de production, vous souhaiterez appeler le `SaveChanges` méthode à partir d’une méthode distincte pour vous donner plus de contrôle sur quand la base de données est mis à jour. (À la fin du didacticiel suivant vous trouverez un lien vers un livre blanc qui présente la modèle unité de travail qui est une approche à la coordination des mises à jour.) Notez également que, dans l’exemple, le `DeleteDepartment` méthode n’inclut pas de code permettant de gérer les conflits d’accès concurrentiel ; code d’implémentation qui sera ajoutée dans un didacticiel plus loin dans cette série.

### <a name="retrieving-instructor-names-to-select-when-inserting"></a>Récupération des noms de l’instructeur pour sélectionner lors de l’insertion

Les utilisateurs doivent pouvoir sélectionner un administrateur à partir d’une liste des formateurs dans une liste déroulante lors de la création de nouveaux services. Par conséquent, ajoutez le code suivant à *SchoolRepository.cs* pour créer une méthode pour récupérer la liste des formateurs à l’aide de la vue que vous avez créé précédemment :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample8.cs)]

### <a name="creating-a-page-for-inserting-departments"></a>Création d’une Page pour l’insertion des départements

Créer un *DepartmentsAdd.aspx* page qui utilise le *Site.Master* page et ajoutez le balisage suivant dans le `Content` contrôle nommé `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample9.aspx)]

Ce balisage crée deux `ObjectDataSource` des contrôles, une pour insérer de nouveaux `Department` entités et l’autre pour récupérer le nom des enseignants pour le `DropDownList` contrôle qui est utilisé pour la sélection des administrateurs de service. Le balisage crée un `DetailsView` contrôle de saisie de nouveaux services et il spécifie un gestionnaire pour le contrôle `ItemInserting` événement afin que vous pouvez définir le `Administrator` valeur de clé étrangère. À la fin est un `ValidationSummary` contrôle pour afficher les messages d’erreur.

Ouvrez *DepartmentsAdd.aspx.cs* et ajoutez le code suivant `using` instruction :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample10.cs)]

Ajoutez les méthodes et la variable de classe suivante :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample11.cs)]

Le `Page_Init` méthode active la fonctionnalité de données dynamiques. Le gestionnaire pour le `DropDownList` du contrôle `Init` événement enregistre une référence au contrôle et le gestionnaire pour le `DetailsView` du contrôle `Inserting` événement utilise cette référence pour obtenir la `PersonID` valeur du formateur sélectionné et de la mise à jour le `Administrator` une propriété de clé étrangère de la `Department` entité.

Exécutez la page, ajouter des informations pour un nouveau service, puis cliquez sur le **insérer** lien.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image28.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image27.png)

Entrez des valeurs pour un autre nouveau service. Entrez un nombre supérieur à format 1,000,000.00 dans le **Budget** champ et onglet dans le champ suivant. Un astérisque apparaît dans le champ, et si vous maintenez le pointeur de la souris dessus, vous pouvez voir le message d’erreur que vous avez entré dans les métadonnées pour ce champ.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image30.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image29.png)

Cliquez sur **insérer**, et vous voyez le message d’erreur affiché par le `ValidationSummary` contrôle en bas de la page.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image32.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image31.png)

Ensuite, fermez le navigateur et ouvrez le *Departments.aspx* page. Ajouter la fonction de suppression pour le *Departments.aspx* page en ajoutant un `DeleteMethod` attribut le `ObjectDataSource` contrôle et un `DataKeyNames` attribut le `GridView` contrôle. Les balises d’ouverture de ces contrôles ressemblera maintenant à l’exemple suivant :

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample12.aspx)]

Exécutez la page.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image34.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image33.png)

Supprimer le service que vous avez ajouté lorsque vous avez exécuté le *DepartmentsAdd.aspx* page.

## <a name="adding-update-functionality"></a>Ajout d’une fonctionnalité de mise à jour

Ouvrez *SchoolRepository.cs* et ajoutez le code suivant `Update` méthode :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample13.cs)]

Lorsque vous cliquez sur **mise à jour** dans le *Departments.aspx* page, le `ObjectDataSource` contrôle crée deux `Department` entités à passer à la `UpdateDepartment` (méthode). Un contient les valeurs d’origine qui ont été stockés dans un état d’affichage, et l’autre les nouvelles valeurs ont été entrées dans le `GridView` contrôle. Le code dans le `UpdateDepartment` méthode passe le `Department` entité ayant les valeurs d’origine à la `Attach` méthode afin d’établir le suivi entre l’entité et les nouveautés de la base de données. Ensuite le code passe le `Department` entité ayant les nouvelles valeurs à la `ApplyCurrentValues` (méthode). Contexte de l’objet compare les valeurs anciennes et nouvelles. Si une nouvelle valeur est différente d’une ancienne valeur, le contexte de l’objet change la valeur de propriété. Le `SaveChanges` méthode met à jour uniquement les colonnes modifiées dans la base de données. (Toutefois, si la fonction de mise à jour pour cette entité ont été mappée à une procédure stockée, la ligne entière est actualisée quels que soient les colonnes ont été modifiées.)

Ouvrez le *Departments.aspx* fichier, puis ajoutez les attributs suivants à la `DepartmentsObjectDataSource` contrôle :

- `UpdateMethod="UpdateDepartment"`
- `ConflictDetection="CompareAllValues"`   
 Cette causes anciennes valeurs à être stockées dans la vue état afin qu’ils peuvent être comparés avec les nouvelles valeurs dans le `Update` (méthode).
- `OldValuesParameterFormatString="orig{0}"`   
 Cela indique au contrôle que le nom du paramètre de valeurs d’origine est `origDepartment` .

Le balisage de la balise d’ouverture de la `ObjectDataSource` contrôle ressemble maintenant à l’exemple suivant :

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample14.aspx)]

Ajouter un `OnRowUpdating="DepartmentsGridView_RowUpdating"` attribut le `GridView` contrôle. Vous allez utiliser pour définir le `Administrator` valeur de propriété basée sur la ligne que l’utilisateur sélectionne dans une liste déroulante. Le `GridView` balise d’ouverture maintenant ressemble à l’exemple suivant :

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample15.aspx)]

Ajouter un `EditItemTemplate` de contrôle pour le `Administrator` colonne à la `GridView` contrôler, immédiatement après le `ItemTemplate` contrôle pour cette colonne :

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample16.aspx)]

Cela `EditItemTemplate` contrôle est similaire à la `InsertItemTemplate` dans contrôler le *DepartmentsAdd.aspx* page. La différence est que la valeur initiale du contrôle est définie à l’aide de la `SelectedValue` attribut.

Avant du `GridView` contrôler, ajoutez un `ValidationSummary` contrôle comme vous le faisiez le *DepartmentsAdd.aspx* page.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample17.aspx)]

Ouvrez *Departments.aspx.cs* et immédiatement après la déclaration de classe partielle, ajoutez le code suivant pour créer un champ privé pour faire référence à la `DropDownList` contrôle :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample18.cs)]

Ajouter des gestionnaires pour les `DropDownList` du contrôle `Init` événement et le `GridView` du contrôle `RowUpdating` événement :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample19.cs)]

Le gestionnaire pour le `Init` événement enregistre une référence à la `DropDownList` contrôle dans le champ de classe. Le gestionnaire pour le `RowUpdating` événement utilise la référence pour obtenir la valeur de l’utilisateur a entré et l’appliquer à la `Administrator` propriété de la `Department` entité.

Utilisez le *DepartmentsAdd.aspx* page pour ajouter un nouveau service, puis exécutez le *Departments.aspx* page et cliquez sur **modifier** sur la ligne que vous avez ajouté.

> [!NOTE]
> Vous ne pourrez pas modifier les lignes que vous n’avez pas ajouté (autrement dit, qui étaient déjà présents dans la base de données), en raison de données non valides dans la base de données ; les administrateurs pour les lignes qui ont été créés avec la base de données sont les étudiants. Si vous essayez de modifier un d’eux, vous obtiendrez une page d’erreur qui signale une erreur telle que `'InstructorsDropDownList' has a SelectedValue which is invalid because it does not exist in the list of items.`


[![Image10](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image36.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image35.png)

Si vous entrez non valide **Budget** montant, puis cliquez sur **mise à jour**, vous voyez le même astérisque et un message d’erreur que vous avez vu dans la *Departments.aspx* page.

Modifier une valeur de champ ou sélectionnez un autre administrateur, cliquez sur **mise à jour**. La modification s’affiche.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image38.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image37.png)

Ceci termine l’introduction à l’utilisation de la `ObjectDataSource` contrôle pour CRUD de base (créer, lire, mettre à jour, supprimer) des opérations avec Entity Framework. Vous avez créé une application à n niveaux simple, mais la couche de logique métier est toujours étroitement liée à la couche d’accès aux données, ce qui complique les tests d’unités automatisés. Dans ce didacticiel, vous verrez comment implémenter le modèle de référentiel pour faciliter les tests unitaires.

> [!div class="step-by-step"]
> [Next](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
