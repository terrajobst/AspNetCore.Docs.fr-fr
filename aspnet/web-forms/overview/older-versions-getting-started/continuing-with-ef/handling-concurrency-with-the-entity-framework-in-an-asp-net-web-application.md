---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
title: Gestion des accès concurrentiels avec Entity Framework 4.0 dans une Application Web 4 ASP.NET | Microsoft Docs
author: tdykstra
description: Cette série de didacticiels s’appuie sur l’application web Contoso University créé par la mise en route avec la série de didacticiels Entity Framework 4.0. J’AI...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: a5aa22a6-fb7f-4f41-9c7f-addda151940b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: c22f06e68b15d3fbf13e9f2af0e19631abe076ec
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37392735"
---
<a name="handling-concurrency-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Gestion des accès concurrentiels avec Entity Framework 4.0 dans une Application Web 4 ASP.NET
====================
par [Tom Dykstra](https://github.com/tdykstra)

> Cette série de didacticiels s’appuie sur l’application web Contoso University créé par le [mise en route avec Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) série de didacticiels. Si vous n’avez pas effectué les didacticiels précédents, comme point de départ pour ce didacticiel vous pouvez [télécharger l’application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que vous l’auriez créée. Vous pouvez également [télécharger l’application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) qui est créé par la série de didacticiels terminée. Si vous avez des questions sur les didacticiels, vous pouvez les publier à le [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


Dans le didacticiel précédent vous avez appris comment trier et données de filtre à l’aide du `ObjectDataSource` contrôle et Entity Framework. Ce didacticiel présente des options de gestion des accès concurrentiels dans une application web ASP.NET qui utilise Entity Framework. Vous allez créer une nouvelle page web qui est dédié à la mise à jour des affectations des formateurs. Vous allez gérer les problèmes d’accès concurrentiel dans cette page et dans la page de services que vous avez créé précédemment.

[![Image06](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

[![Image01](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

## <a name="concurrency-conflicts"></a>Conflits d’accès concurrentiel

Un conflit d’accès concurrentiel se produit lorsqu’un utilisateur modifie un enregistrement et un autre utilisateur modifie le même enregistrement avant la modification du premier utilisateur est écrit dans la base de données. Si vous ne définissez Entity Framework pour détecter ces conflits, toute personne qui met à jour la base de données dernier remplace les modifications des autres utilisateurs. Dans de nombreuses applications, ce risque est acceptable, et vous n’êtes pas obligé de configurer l’application pour gérer les conflits d’accès concurrentiel possible. (S’il existe quelques utilisateurs ou quelques mises à jour, ou si n’est pas réellement critique si des modifications soient remplacées, le coût de la programmation pour l’accès concurrentiel peut annuler les avantages.) Si vous n’avez pas besoin de vous soucier des conflits d’accès concurrentiel, vous pouvez ignorer ce didacticiel. les deux didacticiels restants de la série ne dépendent pas tout ce que vous créez dans celui-ci.

### <a name="pessimistic-concurrency-locking"></a>Accès concurrentiel pessimiste (verrouillage)

Si votre application doit éviter la perte accidentelle de données dans des scénarios d’accès concurrentiel, une manière de le faire consiste à utiliser des verrous de base de données. Il s’agit *d’accès concurrentiel pessimiste*. Par exemple, avant de lire une ligne d’une base de données, vous demandez un verrou pour lecture seule ou pour accès avec mise à jour. Si vous verrouillez une ligne pour accès avec mise à jour, aucun autre utilisateur n’est autorisé à verrouiller la ligne pour lecture seule ou pour accès avec mise à jour, car ils obtiendraient ainsi une copie de données qui sont en cours de modification. Si vous verrouillez une ligne pour accès en lecture seule, d’autres utilisateurs peuvent également la verrouiller pour accès en lecture seule, mais pas pour accès avec mise à jour.

Gestion des verrous présente des inconvénients. Elle peut être complexe à programmer. Elle nécessite des ressources de gestion de base de données importantes, et peut provoquer des problèmes de performances en tant que le nombre d’utilisateurs d’une application augmente (autrement dit, il n’évolue pas correctement). Pour ces raisons, certains systèmes de gestion de base de données ne prennent pas en charge l’accès concurrentiel pessimiste. Entity Framework ne fournit aucune prise en charge intégrée pour celle-ci, et ce didacticiel ne vous montre comment l’implémenter.

### <a name="optimistic-concurrency"></a>Accès concurrentiel optimiste

L’alternative à l’accès concurrentiel pessimiste est *d’accès concurrentiel optimiste*. L’accès concurrentiel optimiste signifie autoriser la survenance des conflits d’accès concurrentiel, puis de réagir correctement quand ils surviennent. Par exemple, John exécute le *Department.aspx* page, clique sur le **modifier** link pour le département de l’historique et réduit le **Budget** montant de $ format 1,000,000.00 à $ 125,000.00. (John administre un département concurrent et souhaite libérer de l’argent pour son propre service).

[![Image07](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Avant de Jean clique sur **mise à jour**, Jane exécute la même page, clique sur le **modifier** lien pour le département de l’historique, puis modifie le **Start Date** champ à 1/1/1/10/2011 1999. (Jane administre le département de l’historique et veut lui donner plus d’ancienneté.)

[![Image08](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

John clique sur **mise à jour** tout d’abord, puis Jane clique sur **mise à jour**. Listes de Jeanne navigateur maintenant le **Budget** montant en tant que format 1,000,000.00 $, mais cela est incorrecte, car la quantité a été modifiée par John à $125,000.00.

Certaines des actions que vous pouvez effectuer dans ce scénario sont les suivantes :

- Vous pouvez effectuer le suivi des propriétés modifiées par un utilisateur et mettre à jour seulement les colonnes correspondantes dans la base de données. Dans l’exemple de scénario, aucune donnée ne serait perdue, car des propriétés différentes ont été mises à jour par chacun des deux utilisateurs. La prochaine fois que quelqu'un consultera le département de l’historique, ils verront le 1/1/1999 et $125,000.00. 

    C’est le comportement par défaut dans Entity Framework, et elle peut réduire considérablement le nombre de conflits qui peuvent entraîner une perte de données. Toutefois, ce comportement n’évite pas une perte de données si des modifications concurrentes sont apportées à la même propriété d’une entité. En outre, ce comportement n’est pas toujours possible ; Lorsque vous mappez des procédures stockées à un type d’entité, toutes les propriétés d’une entité sont mis à jour lorsque des modifications à l’entité sont apportées dans la base de données.
- Vous pouvez laisser les modifications de Jane remplacer les modifications de John. Une fois que Jane clique sur **mise à jour**, le **Budget** quantité revient au format 1,000,000.00 $. Ceci s’appelle un scénario *Priorité au client* ou *Priorité au dernier entré* (Last in Wins). (Les valeurs du client sont prioritaires sur les nouveautés dans le magasin de données).
- Vous pouvez empêcher des modifications de Jane de mise à jour dans la base de données. En règle générale, vous affiche un message d’erreur, démontrer l’état actuel des données et lui permettre d’entrer à nouveau ses modifications si elle souhaite toujours pour les rendre. Vous pourriez davantage automatiser le processus en enregistrant son entrée en lui donnant réappliquer la sans avoir à le retaper. Il s’agit alors d’un scénario *Priorité au magasin*. (Les valeurs du magasin de données sont prioritaires par rapport à celles soumises par le client.)

### <a name="detecting-concurrency-conflicts"></a>Détection des conflits d’accès concurrentiel

Dans Entity Framework, vous pouvez résoudre les conflits en gérant `OptimisticConcurrencyException` exceptions Entity Framework levées par. Pour savoir quand lever ces exceptions, Entity Framework doit être en mesure de détecter les conflits. Par conséquent, vous devez configurer de façon appropriée la base de données et le modèle de données. Voici quelques options pour l’activation de la détection des conflits :

- Dans la base de données, incluez une colonne de table qui peut être utilisée pour déterminer quand une ligne a été modifiée. Vous pouvez ensuite configurer Entity Framework pour inclure cette colonne dans la `Where` clause SQL `Update` ou `Delete` commandes.

    C’est l’objectif de la `Timestamp` colonne dans la `OfficeAssignment` table.

    [![Image09](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

    Le type de données de la `Timestamp` colonne est également appelée `Timestamp`. Toutefois, la colonne ne contient en fait une valeur de date ou d’heure. Au lieu de cela, la valeur est un nombre séquentiel qui est incrémenté chaque fois que la ligne est mise à jour. Dans un `Update` ou `Delete` commande, le `Where` clause inclut la version d’origine `Timestamp` valeur. Si la ligne en cours de mise à jour a été modifiée par un autre utilisateur, la valeur dans `Timestamp` est différente de celle de la valeur d’origine, afin que la `Where` clause ne retourne aucune ligne à mettre à jour. Quand Entity Framework trouve qu’aucune ligne n’ont été mis à jour en cours `Update` ou `Delete` commande (autrement dit, lorsque le nombre de lignes affectées est égal à zéro), il interprète ceci comme un conflit d’accès concurrentiel.
- Configurer Entity Framework pour inclure les valeurs d’origine de chaque colonne dans la table dans le `Where` clause de `Update` et `Delete` commandes.

    Comme dans la première option, si quelque chose dans la ligne a changé depuis la ligne a été lu tout d’abord, le `Where` clause ne retourne pas une ligne à mettre à jour, que l’Entity Framework interprète comme un conflit d’accès concurrentiel. Cette méthode est aussi efficace que d’utiliser un `Timestamp` champ, mais peut être inefficace. Pour les tables de base de données qui ont beaucoup de colonnes, cela peut entraîner de très grandes `Where` clauses, et dans une application web qu’il peut nécessiter la gestion de grandes quantités d’état. Gestion de grandes quantités d’état peut affecter les performances de l’application, car il nécessite des ressources serveur (par exemple, état de session) ou doit être inclus dans la page web elle-même (par exemple, état d’affichage).

Dans ce didacticiel, vous allez ajouter les conflits d’accès concurrentiel optimiste pour une entité qui n’a pas une propriété de suivi de gestion des erreurs (le `Department` entité) et pour une entité qui possède une propriété de suivi (le `OfficeAssignment` entité).

## <a name="handling-optimistic-concurrency-without-a-tracking-property"></a>Gérer l’accès concurrentiel optimiste sans une propriété de suivi

Pour implémenter l’accès concurrentiel optimiste pour la `Department` entité, qui n’a pas de suivi (`Timestamp`) propriété, vous devez accomplir les tâches suivantes :

- Modifier le modèle de données pour activer le suivi pour l’accès concurrentiel `Department` entités.
- Dans le `SchoolRepository` classe, gérer les exceptions d’accès concurrentiel dans le `SaveChanges` (méthode).
- Dans le *Departments.aspx* page, gérer les exceptions d’accès concurrentiel en affichant un message à l’utilisateur d’avertissement que les tentatives de modification ont échoué. L’utilisateur peut ensuite voir les valeurs actuelles et les modifications de nouvelle tentative, s’ils sont toujours nécessaires.

### <a name="enabling-concurrency-tracking-in-the-data-model"></a>L’activation du suivi des modifications dans le modèle de données de concurrence

Dans Visual Studio, ouvrez l’application web Contoso University que vous utilisez dans le didacticiel précédent de cette série.

Ouvrez *SchoolModel.edmx*et dans le Concepteur de modèle de données, cliquez sur le `Name` propriété dans le `Department` entité, puis cliquez sur **propriétés**. Dans le **propriétés** fenêtre, de modifier le `ConcurrencyMode` propriété `Fixed`.

[![Image16](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Faites de même pour les autres propriétés scalaires non de la clé primaire (`Budget`, `StartDate`, et `Administrator`.) (Vous ne pouvez pas faire pour les propriétés de navigation.) Spécifie que chaque fois qu’Entity Framework génère un `Update` ou `Delete` commande SQL pour mettre à jour le `Department` entité dans la base de données, ces colonnes (avec des valeurs d’origine) doivent être inclus dans le `Where` clause. Si aucune ligne n’est trouvée quand la `Update` ou `Delete` commande s’exécute, Entity Framework lève une exception d’accès concurrentiel optimiste.

Enregistrez et fermez le modèle de données.

### <a name="handling-concurrency-exceptions-in-the-dal"></a>Gestion des Exceptions d’accès concurrentiel dans la couche DAL

Ouvrez *SchoolRepository.cs* et ajoutez le code suivant `using` instruction pour la `System.Data` espace de noms :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

Ajoutez le code suivant nouvelle `SaveChanges` (méthode), qui gère les exceptions d’accès concurrentiel optimiste :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Si une erreur d’accès concurrentiel se produit lorsque cette méthode est appelée, les valeurs de propriété de l’entité en mémoire sont remplacées par les valeurs actuellement dans la base de données. L’exception d’accès concurrentiel est à nouveau levée afin que la page web puisse le traiter.

Dans le `DeleteDepartment` et `UpdateDepartment` méthodes, remplacez l’appel existant à `context.SaveChanges()` avec un appel à `SaveChanges()` pour appeler la nouvelle méthode.

### <a name="handling-concurrency-exceptions-in-the-presentation-layer"></a>Gestion des Exceptions d’accès concurrentiel dans la couche de présentation

Ouvrez *Departments.aspx* et ajoutez un `OnDeleted="DepartmentsObjectDataSource_Deleted"` attribut le `DepartmentsObjectDataSource` contrôle. La balise d’ouverture pour le contrôle ressemblera maintenant l’exemple suivant.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

Dans le `DepartmentsGridView` contrôler, spécifier toutes les colonnes de table dans le `DataKeyNames` d’attribut, comme illustré dans l’exemple suivant. Notez que cela crée une vue très volumineuse champs d’état, qui est l’une des raisons pourquoi à l’aide d’un champ de suivi est généralement la meilleure méthode pour effectuer le suivi des conflits d’accès concurrentiel.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Ouvrez *Departments.aspx.cs* et ajoutez le code suivant `using` instruction pour la `System.Data` espace de noms :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

Ajouter la nouvelle méthode suivante, vous appelez à partir du contrôle de source de données `Updated` et `Deleted` gestionnaires d’événements pour la gestion des exceptions d’accès concurrentiel :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

Ce code vérifie le type d’exception, et si elle est une exception d’accès concurrentiel, le code crée dynamiquement un `CustomValidator` contrôle qui à son tour affiche un message dans le `ValidationSummary` contrôle.

Appeler la nouvelle méthode à partir de la `Updated` Gestionnaire d’événements que vous avez ajouté précédemment. En outre, créez un `Deleted` Gestionnaire d’événements qui appelle la même méthode (mais ne fait rien d’autre) :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

### <a name="testing-optimistic-concurrency-in-the-departments-page"></a>Test de l’accès concurrentiel optimiste dans la Page services

Exécutez le *Departments.aspx* page.

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

Cliquez sur **modifier** dans une ligne et remplacez la valeur dans le **Budget** colonne. (N’oubliez pas que vous pouvez modifier uniquement les enregistrements que vous avez créée pour ce didacticiel, étant donné qu’existant `School` des enregistrements de base de données contiennent des données non valides. L’enregistrement pour le département de l’économie est sécurisé à expérimenter.)

[![Image18](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

Ouvrez une nouvelle fenêtre de navigateur et exécutez à nouveau la page (copie l’URL de la zone d’adresse de la première fenêtre navigateur à la deuxième fenêtre de navigateur).

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Cliquez sur **modifier** dans la même ligne que vous avez modifiée précédemment et modifiez le **Budget** valeur à quelque chose de différent.

[![Image19](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

Dans la deuxième fenêtre de navigateur, cliquez sur **mise à jour**. Le **Budget** quantité a été correctement modifiée pour cette nouvelle valeur.

[![Image20](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

Dans la première fenêtre de navigateur, cliquez sur **mise à jour**. La mise à jour échoue. Le **Budget** quantité s’affiche de nouveau à l’aide de la valeur définie dans la deuxième fenêtre de navigateur et vous voyez un message d’erreur.

[![Image21](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

## <a name="handling-optimistic-concurrency-using-a-tracking-property"></a>Gestion des accès concurrentiels optimiste à l’aide d’une propriété de suivi

Pour gérer l’accès concurrentiel optimiste pour une entité qui a une propriété de suivi, vous effectuerez les tâches suivantes :

- Ajouter des procédures stockées au modèle de données pour gérer les `OfficeAssignment` entités. (Le suivi des propriétés et des procédures stockées n’êtes pas obligé d’être utilisés ensemble ; ils sont simplement regroupés ensemble ici à titre d’illustration.)
- Ajouter des méthodes CRUD à la couche DAL et la couche BLL pour `OfficeAssignment` entités, y compris le code pour gérer les exceptions d’accès concurrentiel optimiste dans la couche DAL.
- Créer une page web d’affectations de bureau.
- Tester l’accès concurrentiel optimiste dans la nouvelle page web.

### <a name="adding-officeassignment-stored-procedures-to-the-data-model"></a>Ajout de OfficeAssignment des procédures stockées pour le modèle de données

Ouvrez le *SchoolModel.edmx* de fichiers dans le Générateur de modèles, cliquez sur l’aire de conception et cliquez sur **modèle de mise à jour à partir de la base de données**. Dans le **ajouter** onglet de la **choisir vos objets de base de données** boîte de dialogue, développez **Stored Procedures** et sélectionnez les trois `OfficeAssignment` procédures stockées (consultez le après la capture d’écran), puis cliquez sur **Terminer**. (Ces procédures stockées étaient déjà présents dans la base de données lorsque vous avez téléchargée ou créée à l’aide d’un script.)

[![Image02](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Cliquez sur le `OfficeAssignment` entité, puis sélectionnez **mappage de procédure stockée**.

[![Image03](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Définir le **insérer**, **mise à jour**, et **supprimer** fonctions à utiliser leurs correspondants de procédures stockées. Pour le `OrigTimestamp` paramètre de la `Update` , définissez le **propriété** à `Timestamp` et sélectionnez le **utiliser Original Value** option.

[![Image04](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image30.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image29.png)

Quand Entity Framework appelle la `UpdateOfficeAssignment` une procédure stockée, il passe la valeur d’origine de la `Timestamp` colonne dans la `OrigTimestamp` paramètre. La procédure stockée utilise ce paramètre dans son `Where` clause :

[!code-sql[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.sql)]

La procédure stockée sélectionne également la nouvelle valeur de la `Timestamp` colonne après la mise à jour afin que l’Entity Framework puisse suivre le `OfficeAssignment` entité qui est synchronisée avec la ligne correspondante de la base de données en mémoire.

(Notez que la procédure stockée pour supprimer une affectation de bureau n’a pas un `OrigTimestamp` paramètre. Pour cette raison, Entity Framework ne peut pas vérifier qu’une entité n’est pas modifiée avant de le supprimer.)

Enregistrez et fermez le modèle de données.

### <a name="adding-officeassignment-methods-to-the-dal"></a>Ajout de méthodes de OfficeAssignment à la couche DAL

Ouvrez *ISchoolRepository.cs* et ajoutez les méthodes CRUD suivantes pour le `OfficeAssignment` jeu d’entités :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Ajouter les nouvelles méthodes suivantes pour *SchoolRepository.cs*. Dans le `UpdateOfficeAssignment` (méthode), vous appelez local `SaveChanges` méthode au lieu de `context.SaveChanges`.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

Dans le projet de test, ouvrez *MockSchoolRepository.cs* et ajoutez le code suivant `OfficeAssignment` collection et méthodes CRUD à celui-ci. (Le référentiel factice doit implémenter l’interface de référentiel, ou la solution n’est pas compilé).

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

### <a name="adding-officeassignment-methods-to-the-bll"></a>Ajout de méthodes de OfficeAssignment à la couche BLL

Dans le projet principal, ouvrez *SchoolBL.cs* et ajoutez les méthodes CRUD suivantes pour le `OfficeAssignment` du jeu d’entités à celui-ci :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

## <a name="creating-an-officeassignments-web-page"></a>Création d’une Page Web de OfficeAssignments

Créer une page web qui utilise le *Site.Master* page maître et nommez-le *OfficeAssignments.aspx*. Ajoutez le balisage suivant à la `Content` contrôle nommé `Content2`:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.aspx)]

Notez que dans le `DataKeyNames` d’attribut, le balisage Spécifie le `Timestamp` propriété, ainsi que la clé d’enregistrement (`InstructorID`). Définition des propriétés dans le `DataKeyNames` attribut entraîne le contrôle pour les enregistrer dans l’état du contrôle (qui est similaire à l’état d’affichage) afin que les valeurs d’origine sont disponibles pendant le traitement de publication (postback).

Si vous n’avez pas enregistré la `Timestamp` valeur, Entity Framework aurait pour le `Where` clause de l’instruction SQL `Update` commande. Par conséquent, rien ne se trouve pour mettre à jour. Par conséquent, Entity Framework lève une exception d’accès concurrentiel optimiste chaque fois qu’un `OfficeAssignment` entité est mise à jour.

Ouvrez *OfficeAssignments.aspx.cs* et ajoutez le code suivant `using` instruction pour la couche d’accès aux données :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Ajoutez le code suivant `Page_Init` (méthode), qui offre des fonctionnalités Dynamic Data. Ajoutez également le gestionnaire suivant pour le `ObjectDataSource` du contrôle `Updated` événement afin de vérifier les erreurs d’accès concurrentiel :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

### <a name="testing-optimistic-concurrency-in-the-officeassignments-page"></a>Test de l’accès concurrentiel optimiste dans la Page OfficeAssignments

Exécutez le *OfficeAssignments.aspx* page.

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image32.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image31.png)

Cliquez sur **modifier** dans une ligne et remplacez la valeur dans le **emplacement** colonne.

[![Image11](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image34.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image33.png)

Ouvrez une nouvelle fenêtre de navigateur et exécutez à nouveau la page (copie l’URL de la première fenêtre de navigateur à la deuxième fenêtre de navigateur).

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image36.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image35.png)

Cliquez sur **modifier** dans la même ligne que vous avez modifiée précédemment et modifiez le **emplacement** valeur à quelque chose de différent.

[![Image12](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image38.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image37.png)

Dans la deuxième fenêtre de navigateur, cliquez sur **mise à jour**.

[![Image13](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image40.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image39.png)

Basculez vers la première fenêtre de navigateur et cliquez sur **mise à jour**.

[![Image15](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image42.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image41.png)

Vous voyez un message d’erreur et le **emplacement** valeur a été mis à jour pour afficher la valeur vous l’avez modifiée à dans la deuxième fenêtre de navigateur.

## <a name="handling-concurrency-with-the-entitydatasource-control"></a>Gestion des accès concurrentiels avec le contrôle EntityDataSource

Le `EntityDataSource` contrôle inclut une logique intégrée qui reconnaît les paramètres d’accès concurrentiel dans le modèle de données et gère mettre à jour et des opérations de suppression en conséquence. Toutefois, comme avec toutes les exceptions, vous devez gérer `OptimisticConcurrencyException` exceptions vous-même afin de fournir un message d’erreur convivial.

Ensuite, vous allez configurer le *Courses.aspx* page (qui utilise un `EntityDataSource` contrôle) pour autoriser la mise à jour et supprimer des opérations et pour afficher un message d’erreur si un conflit d’accès concurrentiel se produit. Le `Course` entité n’a pas une concurrence le suivi de colonne, donc vous utiliserez la même méthode que vous l’avez fait le `Department` entité : suivre les valeurs de toutes les propriétés non-clé.

Ouvrez le *SchoolModel.edmx* fichier. Pour les propriétés non-clé de la `Course` entity (`Title`, `Credits`, et `DepartmentID`), définissez la **Mode d’accès concurrentiel** propriété `Fixed`. Ensuite, enregistrez et fermez le modèle de données.

Ouvrez le *Courses.aspx* page et apportez les modifications suivantes :

- Dans le `CoursesEntityDataSource` contrôler, ajoutez `EnableUpdate="true"` et `EnableDelete="true"` attributs. La balise d’ouverture pour ce contrôle ressemble maintenant à l’exemple suivant :

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample16.aspx)]
- Dans le `CoursesGridView` contrôler, modifier le `DataKeyNames` attribut valeur à `"CourseID,Title,Credits,DepartmentID"`. Ajoutez ensuite une `CommandField` élément à la `Columns` élément montre **modifier** et **supprimer** boutons (`<asp:CommandField ShowEditButton="True" ShowDeleteButton="True" />`). Le `GridView` contrôle ressemble maintenant à l’exemple suivant :

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample17.aspx)]

Exécutez la page et créer une situation de conflit que vous avez exécutée auparavant dans la page départements. Exécutez la page dans deux fenêtres de navigateur, cliquez sur **modifier** dans la même ligne dans chaque fenêtre et apportez une modification différente dans chacun d’eux. Cliquez sur **mise à jour** dans une fenêtre, puis cliquez sur **mise à jour** dans l’autre fenêtre. Lorsque vous cliquez sur **mise à jour** la deuxième fois, vous voyez la page d’erreur qui résulte d’une exception non prise en charge d’accès concurrentiel.

[![Image22](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image44.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image43.png)

Vous gérez cette erreur de manière très similaire à la façon dont vous avez géré pour le `ObjectDataSource` contrôle. Ouvrez le *Courses.aspx* page, puis, dans le `CoursesEntityDataSource` contrôle, spécifient des gestionnaires pour le `Deleted` et `Updated` événements. La balise d’ouverture du contrôle ressemble maintenant à l’exemple suivant :

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample18.aspx)]

Avant du `CoursesGridView` de contrôle, ajoutez le code suivant `ValidationSummary` contrôle :

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample19.aspx)]

Dans *Courses.aspx.cs*, ajoutez un `using` instruction pour la `System.Data` espace de noms, ajoutez une méthode qui vérifie pour les exceptions d’accès concurrentiel et ajoutez des gestionnaires pour les `EntityDataSource` du contrôle `Updated` et `Deleted`gestionnaires. Le code doit ressembler à ce qui suit :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample20.cs)]

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample21.cs)]

La seule différence entre ce code et que vous l’avez fait le `ObjectDataSource` contrôle est que dans ce cas l’exception d’accès concurrentiel dans le `Exception` propriété de l’objet arguments d’événement au lieu de cette exception `InnerException` propriété.

Exécutez la page et recréez un conflit d’accès concurrentiel. Cette fois, vous consultez un message d’erreur :

[![Image23](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image46.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image45.png)

Ceci termine l’introduction à la gestion des conflits d’accès concurrentiel. Le didacticiel suivant fournit des conseils sur la façon d’améliorer les performances dans une application web qui utilise Entity Framework.

> [!div class="step-by-step"]
> [Précédent](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
> [Suivant](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
