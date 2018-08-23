---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
title: Bien démarrer avec Entity Framework 4.0 Database First et 4 d’ASP.NET Web Forms - partie 2 | Microsoft Docs
author: tdykstra
description: L’exemple d’application web Contoso University montre comment créer des applications Web Forms ASP.NET à l’aide d’Entity Framework. L’exemple d’application est en cours...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: fb63a326-a4ae-4b0c-a4f5-412327197216
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
msc.type: authoredcontent
ms.openlocfilehash: a0b4acca93deee4fb514fa1bc3e2bd13490cf10e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838696"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-2"></a>Bien démarrer avec Entity Framework 4.0 Database First et 4 les Web Forms ASP.NET - partie 2
====================
par [Tom Dykstra](https://github.com/tdykstra)

> L’exemple d’application web Contoso University montre comment créer des applications Web Forms ASP.NET à l’aide de l’Entity Framework 4.0 et Visual Studio 2010. Pour plus d’informations sur la série de didacticiels, consultez [le premier didacticiel de la série](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="the-entitydatasource-control"></a>Le contrôle EntityDataSource

Dans le didacticiel précédent, vous avez créé un site web, une base de données et un modèle de données. Dans ce didacticiel, vous travaillez avec le `EntityDataSource` contrôle ASP.NET fournit pour le rendre facile à utiliser avec un modèle de données Entity Framework. Vous allez créer un `GridView` contrôle pour afficher et modifier les données des étudiants, un `DetailsView` contrôle pour l’ajout de nouveaux étudiants et un `DropDownList` contrôle pour la sélection d’un service (que vous utiliserez ultérieurement pour l’affichage des cours associés).

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image1.png)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image3.png)

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image5.png)

Notez que dans cette application vous ne sont pas ajouter la validation des entrées aux pages qui mettent à jour de la base de données, et certains de la gestion des erreurs ne seront pas aussi fiables que seraient nécessaires dans une application de production. Qui conserve le didacticiel consacré à Entity Framework et l’empêche de devient trop longue. Pour plus d’informations sur l’ajout de ces fonctionnalités à votre application, consultez [validation des entrées d’utilisateur dans ASP.NET Web Pages](https://msdn.microsoft.com/library/7kh55542.aspx) et [gestion des erreurs dans les Pages ASP.NET et les Applications](https://msdn.microsoft.com/library/w16865z6.aspx).

## <a name="adding-and-configuring-the-entitydatasource-control"></a>Ajout et configuration du contrôle EntityDataSource

Vous commencerez en configurant un `EntityDataSource` contrôle lire `Person` entités à partir de la `People` jeu d’entités.

Vérifiez que vous avez Visual Studio ouvert et que vous travaillez avec le projet que vous avez créé dans la partie 1. Si vous n’avez pas créé le projet dans la mesure où vous avez créé le modèle de données ou depuis la dernière modification que vous avez apportées à elle, générez le projet maintenant. Modifications du modèle de données ne sont pas accessibles au concepteur jusqu'à ce que le projet est généré.

Créer une page web en utilisant le **Web Form avec Page maître** modèle et nommez-le *Students.aspx*.

[![Image23](the-entity-framework-and-aspnet-getting-started-part-2/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image7.png)

Spécifiez *Site.Master* en tant que la page maître. Toutes les pages que vous créez pour ces didacticiels utilisera cette page maître.

[![Image24](the-entity-framework-and-aspnet-getting-started-part-2/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image9.png)

Dans **Source** afficher, ajouter un `h2` titre pour le `Content` contrôle nommé `Content2`, comme illustré dans l’exemple suivant :

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample1.aspx)]

À partir de la **données** onglet de la **boîte à outils**, faites glisser un `EntityDataSource` le contrôle à la page, sous l’en-tête et modifiez le code pour `StudentsEntityDataSource`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample2.aspx)]

Basculez vers **conception** afficher, cliquez sur la balise active du contrôle de source de données, puis cliquez sur **configurer la Source de données** pour lancer le **configurer la Source de données** Assistant.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-2/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image11.png)

Dans le **configurer ObjectContext** étape de l’Assistant, sélectionnez **SchoolEntities** comme valeur pour **connexion nommée**, puis sélectionnez **SchoolEntities**en tant que le **DefaultContainerName** valeur. Cliquez ensuite sur **Suivant**.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-2/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image13.png)

Remarque : Si vous obtenez la boîte de dialogue suivantes à ce stade, vous devez générer le projet avant de continuer.

[![Image25](the-entity-framework-and-aspnet-getting-started-part-2/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image15.png)

Dans le **configurer la sélection des données** étape, sélectionnez **personnes** comme valeur pour **EntitySetName**. Sous **sélectionnez**, assurez-vous que le **sélectionner A** ll case à cocher est activée. Puis sélectionnez les options pour activer la mise à jour et supprimer. Lorsque vous avez terminé, cliquez sur **Terminer**.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-2/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image17.png)

## <a name="configuring-database-rules-to-allow-deletion"></a>Configuration des règles de base de données pour autoriser la suppression

Vous créerez une page qui permet aux utilisateurs de supprimer les étudiants à partir de la `Person` table, qui possède trois relations avec d’autres tables (`Course`, `StudentGrade`, et `OfficeAssignment`). Par défaut, la base de données vous empêche de supprimer une ligne dans `Person` s’il existe des lignes connexes d’une des autres tables. Vous pouvez supprimer manuellement tout d’abord les lignes associées, ou vous pouvez configurer la base de données pour supprimer automatiquement lorsque vous supprimez un `Person` ligne. Pour les enregistrements d’étudiants dans ce didacticiel, vous allez configurer la base de données pour supprimer les données associées automatiquement. Étant donné que les étudiants peuvent avoir des lignes associées uniquement dans le `StudentGrade` table, vous devez configurer uniquement une des trois relations.

Si vous utilisez le *School.mdf* fichier que vous avez téléchargé à partir du projet qui accompagne ce didacticiel, vous pouvez ignorer cette section, car ces modifications de configuration ont déjà été effectuées. Si vous avez créé la base de données en exécutant un script, configurez la base de données en effectuant les procédures suivantes.

Dans **Explorateur de serveurs**, ouvrez le schéma de base de données que vous avez créé dans la partie 1. Avec le bouton droit de la relation entre `Person` et `StudentGrade` (la ligne entre les tables), puis sélectionnez **propriétés**.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-2/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image19.png)

Dans le **propriétés** fenêtre, développez **Spécification INSERT et UPDATE** et définir le **DeleteRule** propriété **Cascade**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-2/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image21.png)

Enregistrez et fermez le diagramme. Si vous êtes invité que vous souhaitiez mettre à jour de la base de données, cliquez sur **Oui**.

Pour vous assurer que le modèle conserve les entités qui se trouvent dans la mémoire synchronisée avec ce que fait la base de données, vous devez définir des règles correspondantes dans le modèle de données. Ouvrez *SchoolModel.edmx*, avec le bouton droit de la ligne d’association entre `Person` et `StudentGrade`, puis sélectionnez **propriétés**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-2/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image23.png)

Dans le **propriétés** fenêtre, définissez **terminaison1 OnDelete** à **Cascade**.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-2/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image25.png)

Enregistrez et fermez le *SchoolModel.edmx* de fichiers, puis régénérez le projet.

En règle générale, lorsque la base de données change, vous avez plusieurs possibilités pour savoir comment synchroniser le modèle :

- Pour certains types de modifications (comme l’ajout ou l’actualisation des tables, vues ou procédures stockées), avec le bouton droit dans le concepteur et sélectionnez **modèle de mise à jour à partir de la base de données** pour le concepteur crée les modifications automatiquement.
- Régénérer le modèle de données.
- Vérifiez les mises à jour manuelles similaire à celle-ci.

Dans ce cas, vous pouvez avez régénéré le modèle ou actualisé les tables affectées par la modification de la relation, mais puis vous seriez obligé de réeffectuer la modification du nom de champ (à partir de `FirstName` à `FirstMidName`).

## <a name="using-a-gridview-control-to-read-and-update-entities"></a>À l’aide d’un contrôle GridView pour lire et mettre à jour des entités

Dans cette section, vous allez utiliser un `GridView` contrôle à afficher, mettre à jour ou supprimer des étudiants.

Ouvrez ou basculez vers *Students.aspx* et basculez vers **conception** vue. À partir de la **données** onglet de la **boîte à outils**, faites glisser un `GridView` contrôle situé à droite de la `EntityDataSource` contrôler, nommez-le `StudentsGridView`et cliquez sur la balise active, puis sélectionnez  **StudentsEntityDataSource** comme source de données.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-2/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image27.png)

Cliquez sur **actualiser le schéma** (cliquez sur **Oui** si vous êtes invité à confirmer), puis cliquez sur **activer la pagination**, **activer le tri**, **Activer la modification**, et **activer la suppression**.

Cliquez sur **modifier les colonnes**.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-2/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image29.png)

Dans le **champs sélectionnés** box, supprimez **PersonID**, **LastName**, et **HireDate**. Vous n’affichez généralement une clé d’enregistrement pour les utilisateurs, la date d’embauche n’est pas pertinente pour les étudiants et vous allez placer les deux parties du nom d’un champ, vous devez uniquement un des champs de nom.)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-2/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image31.png)

Sélectionnez le **FirstMidName** champ, puis cliquez sur **convertir ce champ en TemplateField**.

Faites de même pour **EnrollmentDate**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-2/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image33.png)

Cliquez sur **OK** puis basculez vers **Source** vue. Les modifications restantes seront plus faciles à effectuer directement dans le balisage. Le `GridView` contrôler le balisage se présente comme dans l’exemple suivant.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample3.aspx)]

La première colonne, une fois que le champ de commande est un champ de modèle qui est actuellement affiche le prénom. Modifiez le balisage pour ce champ de modèle ressemble à l’exemple suivant :

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample4.aspx)]

En mode d’affichage, deux `Label` contrôles affichent le nom et prénom. En mode édition, deux zones de texte sont fournies afin de pouvoir le prénom et le nom. Comme avec la `Label` contrôles en mode d’affichage, vous utilisez `Bind` et `Eval` expressions exactement comme vous le feriez avec les contrôles de source de données ASP.NET qui se connectent directement aux bases de données. La seule différence est que vous spécifiez les propriétés d’entité au lieu de colonnes de la base de données.

La dernière colonne est un champ de modèle qui affiche la date d’inscription. Modifiez le balisage de ce champ pour ressembler à l’exemple suivant :

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample5.aspx)]

Dans les afficher et modifier le mode, la chaîne de format « {0, d} » entraîne la date à afficher dans le format « date courte ». (Votre ordinateur peut être configuré pour afficher ce format différemment à partir d’images d’écran présentées dans ce didacticiel.)

Notez que, dans chacun de ces champs de modèle, le concepteur utilisé un `Bind` expression par défaut, mais vous avez modifié à un `Eval` expression dans le `ItemTemplate` éléments. Le `Bind` expression rend les données disponibles dans `GridView` propriétés du contrôle au cas où vous deviez accéder aux données dans le code. Dans cette page vous n’avez pas besoin accéder à ces données dans le code, vous pouvez donc utiliser `Eval`, qui est plus efficace. Pour plus d’informations, consultez [obtenir vos données en dehors des contrôles de données](https://weblogs.asp.net/davidfowler/archive/2008/12/12/getting-your-data-out-of-the-data-controls.aspx).

## <a name="revising-entitydatasource-control-markup-to-improve-performance"></a>Réviser le balisage de contrôle EntityDataSource pour améliorer les performances

Dans le balisage pour le `EntityDataSource` contrôler, supprimez le `ConnectionString` et `DefaultContainerName` attributs et les remplacer par un `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` attribut. Il s’agit d’une modification, vous devez effectuer chaque fois que vous créez un `EntityDataSource` contrôler, à moins que vous devez utiliser une connexion qui est différente de celui qui est codé en dur dans la classe de contexte d’objet. À l’aide de la `ContextTypeName` attribut offre les avantages suivants :

- Performances améliorées. Lorsque le `EntityDataSource` contrôle initialise le modèle de données à l’aide de la `ConnectionString` et `DefaultContainerName` attributs, il effectue un travail supplémentaire pour charger les métadonnées sur chaque demande. Cela n’est pas nécessaire si vous spécifiez le `ContextTypeName` attribut.
- Le chargement différé est activé par défaut dans les classes de contexte d’objet généré (tel que `SchoolEntities` dans ce didacticiel) dans Entity Framework 4.0. Cela signifie que les propriétés de navigation sont chargées avec les données associées automatiquement dès que vous en avez besoin. Le chargement différé est expliqué plus en détail plus loin dans ce didacticiel.
- Toutes les personnalisations que vous avez appliquées à la classe de contexte d’objet (dans ce cas, le `SchoolEntities` classe) sera disponible pour les contrôles qui utilisent la `EntityDataSource` contrôle. Personnalisation de la classe de contexte d’objet est une rubrique avancée qui n’est pas couverte dans cette série de didacticiels. Pour plus d’informations, consultez [Types de généré Entity Framework extension](https://msdn.microsoft.com/library/dd456844.aspx).

Les marques ressembleront maintenant l’exemple suivant (l’ordre des propriétés peut être différent) :

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample6.aspx)]

Le `EnableFlattening` attribut fait référence à une fonctionnalité qui était nécessaire dans les versions précédentes d’Entity Framework, car les colonnes clés étrangères n’étaient pas exposés en tant que propriétés de l’entité. La version actuelle vous permet d’utiliser *associations de clé étrangère*, ce qui signifie que les propriétés de clé étrangère sont exposées pour tout sauf plusieurs-à-plusieurs associations. Si vos entités ont des propriétés de clé étrangère et aucune [des types complexes](https://msdn.microsoft.com/library/bb738472.aspx), vous pouvez laisser cet attribut défini sur `False`. Ne supprimez pas de l’attribut à partir du balisage, car la valeur par défaut est `True`. Pour plus d’informations, consultez [APLANISSEMENT d’objets (EntityDataSource)](https://msdn.microsoft.com/library/ee404746.aspx).

Exécutez la page, et vous voyez une liste des étudiants et des employés (vous allez filtrer pour les étudiants simples dans le didacticiel suivant). Le prénom et le nom sont affichés ensemble.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-2/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image35.png)

Pour trier l’affichage, cliquez sur un nom de colonne.

Cliquez sur **modifier** dans n’importe quelle ligne. Zones de texte sont affichés dans lequel vous pouvez modifier le nom et prénom.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-2/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image37.png)

Le **supprimer** fonctionnement du bouton également. Cliquez sur Supprimer pour une ligne qui a une date d’inscription et la ligne disparaît. (Sans date d’inscription, les lignes représentent des formateurs et vous pouvez obtenir une erreur d’intégrité référentielle. Dans le didacticiel suivant vous allez filtrer cette liste pour inclure les étudiants uniquement.)

## <a name="displaying-data-from-a-navigation-property"></a>Affichage des données à partir d’une propriété de Navigation

Maintenant Supposons que vous souhaitez savoir combien de cours chaque étudiant est inscrit dans. Entity Framework fournit ces informations dans le `StudentGrades` propriété de navigation de la `Person` entité. Étant donné que la conception de base de données n’autorise pas un étudiant à être inscrits dans un cours sans avoir un niveau attribué, pour ce didacticiel vous pouvez supposer que celui dont l’une ligne le `StudentGrade` ligne de table qui est associé à un cours est identique à l’inscrit dans le cours. (Le `Courses` propriété de navigation est uniquement pour les formateurs.)

Lorsque vous utilisez le `ContextTypeName` attribut de la `EntityDataSource` contrôle, Entity Framework récupère les informations pour une propriété de navigation d’automatiquement lorsque vous accédez à cette propriété. Il s’agit *le chargement différé*. Toutefois, cela peut être inefficace, car il en résulte un appel séparé à la base de données que des informations supplémentaires chaque fois sont nécessaire. Si vous avez besoin de données à partir de la propriété de navigation pour chaque entité retournée par la `EntityDataSource` contrôle, il est plus efficace pour récupérer les données associées, ainsi que l’entité elle-même dans un seul appel à la base de données. Il s’agit *chargement hâtif*, et que vous spécifiez un chargement hâtif pour une propriété de navigation en définissant le `Include` propriété de la `EntityDataSource` contrôle.

Dans *Students.aspx*, que vous souhaitez afficher le nombre de cours pour chaque étudiant, le chargement hâtif donc est le meilleur choix. Si vous avez affichant tous les étudiants mais montrant le nombre de cours uniquement pour certaines d'entre elles (ce qui nécessiterait à écrire du code en plus du balisage), le chargement différé peut être un meilleur choix.

Ouvre ou bascule vers *Students.aspx*, basculez vers **conception** afficher, sélectionnez `StudentsEntityDataSource`, puis, dans le **propriétés** fenêtre, définissez la **Include**propriété **StudentGrades**. (Si vous souhaitez obtenir plusieurs propriétés de navigation, vous pouvez spécifier leurs noms séparés par des virgules, par exemple, **StudentGrades, cours**.)

[![Image19](the-entity-framework-and-aspnet-getting-started-part-2/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image39.png)

Basculez vers **Source** vue. Dans le `StudentsGridView` contrôle, après le dernier `asp:TemplateField` élément, ajouter le nouveau champ de modèle suivant :

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample7.aspx)]

Dans le `Eval` expression, vous pouvez référencer la propriété de navigation `StudentGrades`. Étant donné que cette propriété contient une collection, il a un `Count` propriété que vous pouvez utiliser pour afficher le nombre de cours dans lequel l’étudiant est inscrit. Dans un prochain didacticiel, vous verrez comment afficher des données à partir des propriétés de navigation qui contiennent des entités uniques au lieu de collections. (Notez que vous ne pouvez pas utiliser `BoundField` éléments pour afficher des données à partir des propriétés de navigation.)

Exécutez la page, et vous voyez maintenant comment de nombreux cours chaque étudiant est inscrit dans.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image41.png)

## <a name="using-a-detailsview-control-to-insert-entities"></a>À l’aide d’un contrôle DetailsView d’insérer des entités

L’étape suivante consiste à créer une page qui a un `DetailsView` contrôle qui vous permettent d’ajouter de nouveaux étudiants. Fermez le navigateur et ensuite créer une page web en utilisant le *Site.Master* page maître. Nommez la page *StudentsAdd.aspx*, puis basculez vers **Source** vue.

Ajoutez le balisage suivant pour remplacer le balisage existant pour le `Content` contrôle nommé `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample8.aspx)]

Ce balisage crée un `EntityDataSource` contrôle qui est semblable à celle que vous avez créé dans *Students.aspx*, sauf qu’elle permet l’insertion. Comme avec la `GridView` contrôler, les champs liés de la `DetailsView` contrôle sont codées exactement tels qu’ils seraient pour un contrôle de données qui se connecte directement à une base de données, sauf qu’elles font référence les propriétés de l’entité. Dans ce cas, le `DetailsView` contrôle est utilisé uniquement pour l’insertion de lignes, donc vous avez défini le mode par défaut sur `Insert`.

Exécutez la page et ajouter un nouvel étudiant.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image43.png)

Rien ne se passe après avoir inséré un nouvel étudiant, mais si vous exécutez maintenant *Students.aspx*, vous verrez les nouvelles informations de l’étudiant.

## <a name="displaying-data-in-a-drop-down-list"></a>Affichage des données dans une liste déroulante

Dans les étapes suivantes, vous allez databind un `DropDownList` contrôle à une entité définie à l’aide un `EntityDataSource` contrôle. Dans cette partie du didacticiel, vous ne suffit pas avec cette liste. Dans les parties suivantes, cependant, vous utiliserez la liste pour permettre aux utilisateurs de sélectionner un département pour afficher les cours associés au département.

Créer une nouvelle page web nommée *Courses.aspx*. Dans **Source** afficher, ajouter un titre pour le `Content` contrôle nommé `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample9.aspx)]

Dans **conception** afficher, ajouter un `EntityDataSource` contrôle à la page comme vous l’avez fait précédemment, mais cette fois nommez-le `DepartmentsEntityDataSource`. Sélectionnez **départements** en tant que le **EntitySetName** valeur, puis sélectionnez uniquement le **DepartmentID** et **nom** propriétés.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-2/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image45.png)

À partir de la **Standard** onglet de la **boîte à outils**, faites glisser un `DropDownList` le contrôle à la page, nommez-le `DepartmentsDropDownList`, cliquez sur la balise active, puis sélectionnez **choisir la Source de données** à Démarrer le **Assistant de Configuration de source de données**.

[![Image16](the-entity-framework-and-aspnet-getting-started-part-2/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image47.png)

Dans le **choisir une Source de données** étape, sélectionnez **DepartmentsEntityDataSource** comme source de données, cliquez sur **actualiser le schéma**, puis sélectionnez **nom** en tant que champ de données à afficher et **DepartmentID** en tant que le champ de données de valeur. Cliquez sur **OK**.

[![Image17](the-entity-framework-and-aspnet-getting-started-part-2/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image49.png)

La méthode vous permet de lier le contrôle à l’aide d’Entity Framework est le même qu’avec d’autres données ASP.NET des contrôles de code source sauf que vous spécifiez les entités et les propriétés de l’entité.

Basculer vers **Source** afficher et ajouter « sélectionner un service : « immédiatement avant le `DropDownList` contrôle.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample10.aspx)]

En guise de rappel, modifiez le balisage pour le `EntityDataSource` contrôle à ce stade, en remplaçant le `ConnectionString` et `DefaultContainerName` attributs avec un `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` attribut. Il est souvent préférable d’attendre jusqu'à une fois que vous avez créé le contrôle lié aux données qui est lié au contrôle de source de données avant de modifier le `EntityDataSource` contrôler le balisage, car après avoir apporté la modification, le concepteur pas vous fourniront un **actualiser Schéma** option dans le contrôle lié aux données.

Exécutez la page, et vous pouvez sélectionner un département dans la liste déroulante.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image51.png)

Ceci termine l’introduction à l’utilisation de la `EntityDataSource` contrôle. Utilisation de ce contrôle diffère généralement pas travailler avec d’autres données d’ASP.NET, les contrôles de source, à ceci près que vous référencez des entités et propriétés au lieu de tables et colonnes. La seule exception est lorsque vous souhaitez accéder aux propriétés de navigation. Dans le didacticiel suivant, vous verrez que la syntaxe que vous utilisez avec `EntityDataSource` contrôle peut-être également différer à partir d’autres contrôles de source de données lorsque vous filtrez, regrouper et trier les données.

> [!div class="step-by-step"]
> [Précédent](the-entity-framework-and-aspnet-getting-started-part-1.md)
> [Suivant](the-entity-framework-and-aspnet-getting-started-part-3.md)
