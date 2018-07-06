---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
title: 'À l’aide d’Entity Framework 4.0 et le contrôle ObjectDataSource, partie 2 : ajout d’une couche de logique métier et les Tests unitaires | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels s’appuie sur l’application web Contoso University créé par la mise en route avec la série de didacticiels Entity Framework 4.0. J’AI...
ms.author: aspnetcontent
ms.date: 01/26/2011
ms.assetid: efb0e677-10b8-48dc-93d3-9ba3902dd807
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
msc.type: authoredcontent
ms.openlocfilehash: 6adeb0e49fa754c42bdff6e0bb058f7b73a3f68f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832556"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests"></a>À l’aide d’Entity Framework 4.0 et le contrôle ObjectDataSource, partie 2 : ajout d’une couche de logique métier et les Tests unitaires
====================
par [Tom Dykstra](https://github.com/tdykstra)

> Cette série de didacticiels s’appuie sur l’application web Contoso University créé par le [mise en route avec Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) série de didacticiels. Si vous n’avez pas effectué les didacticiels précédents, comme point de départ pour ce didacticiel vous pouvez [télécharger l’application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que vous l’auriez créée. Vous pouvez également [télécharger l’application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) qui est créé par la série de didacticiels terminée. Si vous avez des questions sur les didacticiels, vous pouvez les publier à le [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


Dans le didacticiel précédent, vous avez créé une application à n niveaux web à l’aide d’Entity Framework et le `ObjectDataSource` contrôle. Ce didacticiel montre comment ajouter une logique métier tout en conservant la couche de logique métier (BLL) et la couche d’accès aux données (DAL) distinct, et il montre comment créer des tests unitaires automatisés pour la couche BLL.

Dans ce didacticiel, vous effectuerez les tâches suivantes :

- Créer une interface de référentiel qui déclare les méthodes d’accès aux données que vous avez besoin.
- Implémentez l’interface de référentiel dans la classe de dépôt.
- Créez une classe de logique métier qui appelle la classe de dépôt pour effectuer des fonctions d’accès aux données.
- Connecter le `ObjectDataSource` contrôle à la classe de logique métier au lieu de la classe de référentiel.
- Créez un projet de test unitaire et une classe de référentiel qui utilise des collections en mémoire pour stocker ses données.
- Créer un test unitaire pour la logique métier que vous souhaitez ajouter à la classe de la logique métier, puis exécuter le test et voir échouent.
- Implémenter la logique métier dans la classe de logique métier, puis ré-exécutez l’unité de test et voir passer.

Vous allez utiliser le *Departments.aspx* et *DepartmentsAdd.aspx* pages que vous avez créé dans le didacticiel précédent.

## <a name="creating-a-repository-interface"></a>Création d’une Interface de référentiel

Vous commencerez par créer l’interface de référentiel.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image1.png)

Dans le *DAL* dossier, créez un fichier de classe, nommez-le *ISchoolRepository.cs*et remplacez le code existant par le code suivant :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample1.cs)]

L’interface définit une méthode pour chacun du CRUD (créer, lire, mettre à jour, supprimer) les méthodes que vous avez créé dans la classe de dépôt.

Dans le `SchoolRepository` classe *SchoolRepository.cs*, indiquer que cette classe implémente le `ISchoolRepository` interface :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample2.cs)]

## <a name="creating-a-business-logic-class"></a>Création d’une classe de logique métier

Ensuite, vous allez créer la classe de logique métier. Cela afin que vous pouvez ajouter une logique métier qui est exécutée par le `ObjectDataSource` contrôler, bien que vous ne fera qu’encore. Pour l’instant, la nouvelle classe de logique métier effectue uniquement les mêmes opérations CRUD qui effectue le référentiel.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image3.png)

Créez un nouveau dossier et nommez-le *BLL*. (Dans une application réelle, la couche de logique métier doit généralement être implémentée comme une bibliothèque de classes, un projet distinct, mais pour simplifier ce didacticiel, les classes de la couche BLL seront conservés dans un dossier de projet.)

Dans le *BLL* dossier, créez un fichier de classe, nommez-le *SchoolBL.cs*et remplacez le code existant par le code suivant :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample3.cs)]

Ce code crée les mêmes méthodes CRUD, que vous avez vu plus haut dans la classe de dépôt, mais au lieu d’accéder directement les méthodes Entity Framework, il appelle le référentiel des méthodes de la classe.

La variable de classe qui contient une référence à la classe de dépôt est définie comme un type d’interface, et le code qui instancie la classe de dépôt est contenu dans les deux constructeurs. Le constructeur sans paramètre sera utilisé par le `ObjectDataSource` contrôle. Il crée une instance de la `SchoolRepository` classe que vous avez créé précédemment. L’autre constructeur permet de tout code qui instancie la classe de logique métier à passer dans n’importe quel objet qui implémente l’interface de référentiel.

Les méthodes CRUD qui appellent la classe de dépôt et les deux constructeurs permettent d’utiliser la classe de logique métier avec le magasin de données back-end vous choisissez. La classe de logique métier n’a pas besoin être conscient de conservation des données dans la classe qu’il appelle. (On parle souvent *ignorance de persistance*.) Cela facilite les tests unitaires, car vous pouvez vous connecter à la classe de logique métier pour une implémentation de référentiel qui utilise un élément simple en tant qu’en mémoire `List` collections pour stocker les données.

> [!NOTE]
> Techniquement, les objets d’entité sont toujours pas ignorant la persistance, car elles sont instanciées à partir de classes qui héritent d’Entity Framework `EntityObject` classe. Pour l’ignorance de la persistance terminée, vous pouvez utiliser *bons vieux objets CLR*, ou *oct*, à la place des objets qui héritent de la `EntityObject` classe. À l’aide d’oct est dépasse le cadre de ce didacticiel. Pour plus d’informations, consultez [testabilité et Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx) sur le site Web MSDN.)


Vous pouvez désormais connecter le `ObjectDataSource` contrôles à la logique métier classe au lieu du référentiel et vérifier que tout fonctionne comme avant.

Dans *Departments.aspx* et *DepartmentsAdd.aspx*, remplacez chaque occurrence de `TypeName="ContosoUniversity.DAL.SchoolRepository"` à `TypeName="ContosoUniversity.BLL.SchoolBL`». (Il existe quatre instances en tout).

Exécutez le *Departments.aspx* et *DepartmentsAdd.aspx* pages pour vérifier qu’elles fonctionnent toujours comme avant.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image5.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image7.png)

## <a name="creating-a-unit-test-project-and-repository-implementation"></a>Création d’un projet de Test unitaire et l’implémentation du dépôt

Ajouter un nouveau projet à la solution en utilisant le **projet de Test** modèle et nommez-le `ContosoUniversity.Tests`.

Dans le projet de test, ajoutez une référence à `System.Data.Entity` et ajouter une référence de projet à la `ContosoUniversity` projet.

Vous pouvez maintenant créer la classe de dépôt que vous utiliserez avec des tests unitaires. Le magasin de données pour ce dépôt sera au sein de la classe.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image9.png)

Dans le projet de test, créez un nouveau fichier de classe, nommez-le *MockSchoolRepository.cs*et remplacez le code existant par le code suivant :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample4.cs)]

Cette classe de référentiel possède les mêmes méthodes CRUD que celui qui accède directement à Entity Framework, mais elles fonctionnent avec `List` collections en mémoire au lieu d’une base de données. Cela rend plus facile pour une classe de test configurer et valider les tests unitaires pour la classe de logique métier.

## <a name="creating-unit-tests"></a>Création de Tests unitaires

Le **tester** modèle de projet créé une classe de test unitaire stub pour vous et votre tâche suivante consiste à modifier cette classe en y ajoutant des méthodes de test unitaire pour la logique métier que vous souhaitez ajouter à la classe de logique métier.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image11.png)

À l’Université de Contoso, tout formateur individuel peut faire que l’administrateur d’un seul service, et vous devez ajouter une logique métier pour appliquer cette règle. Vous allez commencer par ajouter des tests et les tests pour les voir échouent en cours d’exécution. Puis vous ajoutez le code et réexécuter les tests, attendez les transmettre.

Ouvrez le *UnitTest1.cs* fichier, puis ajoutez `using` instructions pour les couches de logique et l’accès aux données d’entreprise que vous avez créé dans le projet ContosoUniversity :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample5.cs)]

Remplacez le `TestMethod1` méthode avec les méthodes suivantes :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample6.cs)]

Le `CreateSchoolBL` méthode crée une instance de la classe de référentiel que vous avez créé pour le test unitaire projet, qu’il passe ensuite à une nouvelle instance de la classe de logique métier. La méthode utilise ensuite la classe de logique métier pour insérer trois services que vous pouvez utiliser dans les méthodes de test.

Les méthodes de test vérifier que la classe de logique métier lève une exception si quelqu'un tente d’insérer un nouveau service avec le même administrateur comme un service existant, ou si quelqu'un tente de mettre à jour d’administrateur d’un service d’entreprise en lui attribuant l’ID d’une personne qui est déjà l’administrateur d’un autre département.

Vous n’avez pas créé la classe d’exception encore, donc ce code n’est pas compilé. Pour laisser se compiler, avec le bouton droit `DuplicateAdministratorException` et sélectionnez **générer**, puis **classe**.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image13.png)

Cela crée une classe dans le projet de test que vous pouvez supprimer une fois que vous avez créé la classe d’exception dans le projet principal. et mis en œuvre la logique métier.

Exécutez le projet de test. Comme prévu, les tests échouent.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image15.png)

## <a name="adding-business-logic-to-make-a-test-pass"></a>Ajouter une logique métier pour effectuer un test

Ensuite, vous allez implémenter la logique métier qui rend impossible de définir en tant que l’administrateur d’un service, une personne qui est déjà administrateur d’un autre département. Vous effectuerez les lever une exception à partir de la couche de logique métier et intercepter dans la couche de présentation si un utilisateur modifie un département et clique sur **mise à jour** après avoir sélectionné une personne qui est déjà un administrateur. (Vous pouvez aussi supprimer des formateurs dans la liste déroulante qui sont déjà administrateurs avant de restituer la page, mais l’objectif ici est de travailler avec la couche de logique métier).

Commencez par créer la classe d’exception que vous allez lever lorsqu’un utilisateur tente d’établir un formateur l’administrateur de plusieurs départements. Dans le projet principal, créez un nouveau fichier de classe dans le *BLL* dossier, nommez-le *DuplicateAdministratorException.cs*et remplacez le code existant par le code suivant :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample7.cs)]

À présent supprimer temporaire *DuplicateAdministratorException.cs* fichier que vous avez créé dans le projet de test afin d’être en mesure de compiler.

Dans le projet principal, ouvrez le *SchoolBL.cs* fichier, puis ajoutez la méthode suivante qui contient la logique de validation. (Le code fait référence à une méthode que vous créerez ultérieurement.)

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample8.cs)]

Vous devez appeler cette méthode lorsque que vous insérez ou la mise à jour `Department` entités afin de vérifier si un autre service a déjà le même administrateur.

Le code appelle une méthode pour rechercher la base de données pour un `Department` entité ayant le même `Administrator` valeur de propriété en tant que l’entité qui est inséré ou mis à jour. S’il existe, le code lève une exception. Aucune vérification de validation est nécessaire si l’entité est insérée ou mise à jour n’a pas `Administrator` valeur et aucune exception n’est levée si la méthode est appelée pendant une mise à jour et la `Department` entité trouvée correspond à la `Department` entité en cours de mise à jour.

Appeler la nouvelle méthode à partir de la `Insert` et `Update` méthodes :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample9.cs)]

Dans *ISchoolRepository.cs*, ajoutez la déclaration suivante pour la nouvelle méthode d’accès aux données :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample10.cs)]

Dans *SchoolRepository.cs*, ajoutez le code suivant `using` instruction :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample11.cs)]

Dans *SchoolRepository.cs*, ajoutez la nouvelle méthode d’accès aux données suivantes :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample12.cs)]

Ce code récupère `Department` entités qui présentent un administrateur spécifié. Un département doit se trouver (le cas échéant). Toutefois, comme aucune contrainte n’est créée dans la base de données, le type de retour est une collection dans le cas où plusieurs services sont trouvent.

Par défaut, lorsque le contexte d’objet récupère des entités à partir de la base de données, il effectue le suivi de leur dans son gestionnaire d’état objet. Le `MergeOption.NoTracking` paramètre spécifie que ce suivi n’est pas effectué pour cette requête. Cela est nécessaire car la requête peut retourner l’entité exacte que vous tentez de mettre à jour, et ensuite vous serait pas en mesure de joindre cette entité. Par exemple, si vous modifiez le département de l’historique dans le *Departments.aspx* page et ne pas modifier l’administrateur, cette requête renvoie le département de l’historique. Si `NoTracking` n’est pas défini, le contexte d’objet déjà aurait l’entité department de l’historique de son gestionnaire d’état objet. Puis lorsque vous attachez de l’entité department historique qui est recréée à partir de l’état d’affichage, le contexte d’objet lèverait une exception indiquant `"An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key"`.

(Comme alternative à la spécification `MergeOption.NoTracking`, vous pouvez créer un nouveau contexte d’objet uniquement pour cette requête. Étant donné que le nouveau contexte d’objet aurait sa propre gestionnaire d’état objet, il n’y aurait aucun conflit lorsque vous appelez le `Attach` (méthode). Le nouveau contexte d’objet doivent partager la connexion de base de données et métadonnées avec le contexte de l’objet d’origine, donc dégrader les performances de cette approche est minime. L’approche illustrée ici, toutefois, introduit la `NoTracking` option, qui vous seront utiles dans d’autres contextes. Le `NoTracking` option est décrite plus en détail dans un didacticiel plus loin dans cette série.)

Dans le projet de test, ajoutez la nouvelle méthode d’accès aux données à *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample13.cs)]

Ce code utilise LINQ pour effectuer la même sélection de données qui le `ContosoUniversity` dépôt de projet utilise LINQ to Entities pour.

Exécutez à nouveau le projet de test. Cette fois-ci, les tests réussissent.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image17.png)

## <a name="handling-objectdatasource-exceptions"></a>Gestion des Exceptions de l’ObjectDataSource

Dans le `ContosoUniversity` projet, exécutez le *Departments.aspx* page et essayez de modifier l’administrateur d’un service à une personne est déjà administrateur pour un autre service. (N’oubliez pas que vous pouvez modifier uniquement les services que vous avez ajouté au cours de ce didacticiel, étant donné que la base de données est préinstallé avec des données non valides). Vous obtenez la page erreur de serveur à l’adresse suivante :

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image19.png)

Vous ne voulez pas aux utilisateurs de voir ce type de page d’erreur, vous devez donc ajouter code de gestion des erreurs. Ouvrez *Departments.aspx* et spécifiez un gestionnaire pour la `OnUpdated` événements de la `DepartmentsObjectDataSource`. Le `ObjectDataSource` balise d’ouverture maintenant ressemble à l’exemple suivant.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample14.aspx)]

Dans *Departments.aspx.cs*, ajoutez le code suivant `using` instruction :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample15.cs)]

Ajoutez le gestionnaire suivant pour le `Updated` événement :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample16.cs)]

Si le `ObjectDataSource` contrôle intercepte une exception lorsqu’elle tente d’effectuer la mise à jour, l’exception est passée dans l’argument d’événement (`e`) à ce gestionnaire. Le code dans le gestionnaire vérifie pour voir si l’exception est l’exception de l’administrateur en double. Si c’est le cas, le code crée un contrôle de validateur qui contient un message d’erreur pour le `ValidationSummary` contrôle à afficher.

Exécutez la page et tentez de réeffectuer une personne l’administrateur des deux services. Cette fois le `ValidationSummary` contrôle affiche un message d’erreur.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image21.png)

Apporter des modifications similaires à la *DepartmentsAdd.aspx* page. Dans *DepartmentsAdd.aspx*, spécifiez un gestionnaire pour le `OnInserted` événements de la `DepartmentsObjectDataSource`. Le balisage résultant doit ressembler à l’exemple suivant.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample17.aspx)]

Dans *DepartmentsAdd.aspx.cs*, ajoutez le même `using` instruction :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample18.cs)]

Ajoutez le Gestionnaire d’événements suivantes :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample19.cs)]

Vous pouvez maintenant tester la *DepartmentsAdd.aspx.cs* page pour vérifier qu’elle gère également correctement tente d’établir une personne de l’administrateur de plusieurs départements.

Ceci termine l’introduction à l’implémentation du modèle de référentiel pour l’utilisation de la `ObjectDataSource` contrôle avec Entity Framework. Pour plus d’informations sur le modèle de référentiel et la testabilité, consultez le livre blanc MSDN [testabilité et Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx).

Dans ce didacticiel, vous verrez comment ajouter le tri et filtrage des fonctionnalités à l’application.

> [!div class="step-by-step"]
> [Précédent](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)
> [Suivant](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
