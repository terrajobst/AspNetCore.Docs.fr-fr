---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
title: Quelles sont les nouveautés dans Entity Framework 4.0 | Microsoft Docs
author: tdykstra
description: Cette série de didacticiels s’appuie sur l’application web Contoso University créé par la mise en route avec la série de didacticiels Entity Framework 4.0. J’AI...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 393df4a8-b1db-44c4-9db7-2b533ca887d0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
msc.type: authoredcontent
ms.openlocfilehash: 402e7ace1abad899d32ed179d6b68de4e5a129f5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826173"
---
<a name="whats-new-in-the-entity-framework-40"></a>Quelles sont les nouveautés dans Entity Framework 4.0
====================
par [Tom Dykstra](https://github.com/tdykstra)

> Cette série de didacticiels s’appuie sur l’application web Contoso University créé par le [mise en route avec Entity Framework](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) série de didacticiels. Si vous n’avez pas effectué les didacticiels précédents, comme point de départ pour ce didacticiel vous pouvez [télécharger l’application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que vous l’auriez créée. Vous pouvez également [télécharger l’application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) qui est créé par la série de didacticiels terminée. Si vous avez des questions sur les didacticiels, vous pouvez les publier à le [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


Dans le didacticiel précédent, vous avez vu des méthodes pour optimiser les performances d’une application web qui utilise Entity Framework. Ce didacticiel passe en revue certaines des plus importantes nouvelles fonctionnalités dans la version 4 d’Entity Framework, et il est lié à des ressources qui fournissent une présentation plus complète pour toutes les nouvelles fonctionnalités. Les fonctionnalités de mise en surbrillance dans ce didacticiel sont les suivantes :

- Associations de clé étrangère.
- L’exécution des commandes SQL défini par l’utilisateur.
- Développement de modèle en premier.
- Prise en charge POCO.

En outre, le didacticiel vous présente brièvement *développement code first*, une fonctionnalité qui est prévue dans la prochaine version d’Entity Framework.

Pour démarrer le didacticiel, démarrez Visual Studio et ouvrez l’application web Contoso University que vous utilisez dans le didacticiel précédent.

## <a name="foreign-key-associations"></a>Associations de clé étrangère

La version 3.5 d’Entity Framework inclus les propriétés de navigation, mais n’a pas les propriétés de clé étrangère dans le modèle de données. Par exemple, le `CourseID` et `StudentID` colonnes de la `StudentGrade` table sont omise de la `StudentGrade` entité.

[![Image01](what-s-new-in-the-entity-framework-4/_static/image2.png)](what-s-new-in-the-entity-framework-4/_static/image1.png)

La raison de cette approche est que, strictement parlant, les clés étrangères sont un détail d’implémentation physique et n’appartenant pas à un modèle conceptuel de données. Toutefois, dans la pratique, il est souvent plus facile de travailler avec les entités de code lorsque vous avez un accès direct aux clés étrangères.

Pour un exemple de comment étrangères clés dans le modèle de données peut simplifier votre code, envisagez la façon dont vous auriez dû au code la *DepartmentsAdd.aspx* page sans eux. Dans le `Department` entité, le `Administrator` propriété est une clé étrangère qui correspond à `PersonID` dans le `Person` entité. Afin d’établir l’association entre un service et son administrateur, il vous suffisait à faire a été défini à la valeur de la `Administrator` propriété dans le `ItemInserting` Gestionnaire d’événements du contrôle lié aux données :

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample1.cs)]

Sans les clés étrangères dans le modèle de données, vous pouvez gérer le `Inserting` événement du contrôle de source de données au lieu du `ItemInserting` événement du contrôle lié aux données, afin d’obtenir une référence à l’entité elle-même avant de l’entité est ajoutée au jeu d’entités. Lorsque vous avez cette référence, vous établissez l’association à l’aide de code similaire dans les exemples suivants :

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample2.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample3.cs)]

Comme vous pouvez le voir dans l’équipe Entity Framework [billet de blog sur les associations de clé étrangère](https://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx), il existe des autres cas où la différence en termes de complexité de code est beaucoup plus important. Pour répondre aux besoins de ceux qui préfèrent en direct avec les détails d’implémentation dans le modèle conceptuel de données pour des raisons de code plus simple, Entity Framework permet à présent la possibilité d’inclure des clés étrangères dans le modèle de données.

Dans la terminologie Entity Framework, si vous incluez des clés étrangères dans le modèle de données que vous utilisez *associations de clé étrangère*, et si vous excluez des clés étrangères, vous utilisez *associations indépendantes*.

## <a name="executing-user-defined-sql-commands"></a>L’exécution des commandes SQL défini par l’utilisateur

Dans les versions antérieures d’Entity Framework, il n’a aucun moyen facile de créer vos propres commandes SQL à la volée et de les exécuter. Entity Framework généré dynamiquement des commandes SQL pour vous, ou vous deviez créer une procédure stockée et l’importer en tant que fonction. Version 4 ajoute `ExecuteStoreQuery` et `ExecuteStoreCommand` méthodes la `ObjectContext` classe qui le rendent plus facile pour vous permettre de passer n’importe quelle requête directement à la base de données.

Supposons que les administrateurs de Contoso University veulent être en mesure d’effectuer des modifications en bloc dans la base de données sans avoir à passer par le processus de création d’une procédure stockée et l’importer dans le modèle de données. Leur première demande est pour une page qui leur permet de modifier le nombre de crédits pour tous les cours dans la base de données. Dans la page web, ils veulent être en mesure d’entrer un nombre à utiliser pour multiplier la valeur de chaque `Course` de ligne `Credits` colonne.

Créer une nouvelle page qui utilise le *Site.Master* page maître et nommez-le *UpdateCredits.aspx*. Puis ajoutez le balisage suivant à la `Content` contrôle nommé `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample4.aspx)]

Ce balisage crée un `TextBox` contrôle dans lequel l’utilisateur peut entrer la valeur de multiplicateur, un `Button` contrôle cliquer afin d’exécuter la commande et un `Label` contrôle permettant d’indiquer le nombre de lignes affectées.

Ouvrez *UpdateCredits.aspx.cs*et ajoutez le code suivant `using` instruction et un gestionnaire pour le bouton `Click` événement :

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample5.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample6.cs)]

Ce code exécute l’instruction SQL `Update` commande à l’aide de la valeur dans la zone de texte et utilise l’étiquette pour afficher le nombre de lignes affectées. Avant d’exécuter la page, exécutez le *Courses.aspx* page pour obtenir une image « avant » de certaines données.

[![Image02](what-s-new-in-the-entity-framework-4/_static/image4.png)](what-s-new-in-the-entity-framework-4/_static/image3.png)

Exécutez *UpdateCredits.aspx*, entrez « 10 » en tant que le multiplicateur, puis cliquez sur **Execute**.

[![Image03](what-s-new-in-the-entity-framework-4/_static/image6.png)](what-s-new-in-the-entity-framework-4/_static/image5.png)

Exécutez le *Courses.aspx* page à nouveau pour voir les données modifiées.

[![Image04](what-s-new-in-the-entity-framework-4/_static/image8.png)](what-s-new-in-the-entity-framework-4/_static/image7.png)

(Si vous souhaitez définir le nombre de crédits à leurs valeurs d’origine, en *UpdateCredits.aspx.cs* modifier `Credits * {0}` à `Credits / {0}` et exécutez de nouveau la page, entrez 10 comme diviseur.)

Pour plus d’informations sur l’exécution de requêtes que vous définissez dans le code, consultez [Comment : directement exécuter des commandes par rapport à la Source de données](https://msdn.microsoft.com/library/ee358769.aspx).

## <a name="model-first-development"></a>Développement basé d’abord modèle

Dans ces procédures pas à pas, vous allez créé en premier la base de données et ensuite généré le modèle de données basé sur la structure de base de données. Dans Entity Framework 4, vous pouvez commencer avec le modèle de données à la place et générer la base de données basé sur la structure de modèle de données. Si vous créez une application pour laquelle la base de données n’existe pas déjà, l’approche model first vous permet de créer des entités et relations pertinentes sur le plan conceptuel pour l’application, tout ne pas soucier des détails d’implémentation physique . (Cela reste vrai uniquement via les étapes initiales de développement, toutefois. Finit par la base de données est créé et a des données de production qu’il contient et la recréer à partir du modèle ne sera plus pratique ; à ce stade vous reviendrez l’approche de la base de données en premier.)

Dans cette section du didacticiel, vous allez créer un modèle de données simple et générer la base de données à partir de celui-ci.

Dans **l’Explorateur de solutions**, avec le bouton droit le *DAL* dossier et sélectionnez **ajouter un nouvel élément**. Dans le **ajouter un nouvel élément** boîte de dialogue **modèles installés** sélectionnez **données** , puis sélectionnez le **ADO.NET Entity Data Model** modèle . Nommez le nouveau fichier *AlumniAssociationModel.edmx* et cliquez sur **ajouter**.

[![Image06](what-s-new-in-the-entity-framework-4/_static/image10.png)](what-s-new-in-the-entity-framework-4/_static/image9.png)

Cette opération lance l’Assistant Entity Data Model. Dans le **choisir le contenu du modèle** étape, sélectionnez **modèle vide** puis cliquez sur **Terminer**.

[![Image07](what-s-new-in-the-entity-framework-4/_static/image12.png)](what-s-new-in-the-entity-framework-4/_static/image11.png)

Le **Entity Data Model Designer** s’ouvre avec une aire de conception vide. Faites glisser un **entité** d’élément à partir de la **boîte à outils** sur l’aire de conception.

[![Image08](what-s-new-in-the-entity-framework-4/_static/image14.png)](what-s-new-in-the-entity-framework-4/_static/image13.png)

Modifier le nom de l’entité à partir de `Entity1` à `Alumnus`, modifiez le `Id` nom de propriété à `AlumnusId`et ajouter une nouvelle propriété scalaire nommée `Name`. Pour ajouter de nouvelles propriétés vous pouvez appuyer sur ENTRÉE après avoir modifié le nom de la `Id` colonne, ou cliquez sur l’entité, puis sélectionnez **ajouter une propriété scalaire**. Est le type par défaut pour les nouvelles propriétés `String`, ce qui convient pour cette démonstration simple, mais bien sûr, vous pouvez modifier les choses comme type de données dans le **propriétés** fenêtre.

Créez une autre entité de la même façon et nommez-le `Donation`. Modifier le `Id` propriété `DonationId` et ajouter une propriété scalaire nommée `DateAndAmount`.

[![Image09](what-s-new-in-the-entity-framework-4/_static/image16.png)](what-s-new-in-the-entity-framework-4/_static/image15.png)

Pour ajouter une association entre ces deux entités, cliquez sur le `Alumnus` entité, sélectionnez **ajouter**, puis sélectionnez **Association**.

[![Image10](what-s-new-in-the-entity-framework-4/_static/image18.png)](what-s-new-in-the-entity-framework-4/_static/image17.png)

Les valeurs par défaut dans le **ajouter une Association** boîte de dialogue sont ce que vous voulez (un-à-plusieurs, inclure des propriétés de navigation, inclure des clés étrangères), par conséquent, cliquez sur **OK**.

[![Image11](what-s-new-in-the-entity-framework-4/_static/image20.png)](what-s-new-in-the-entity-framework-4/_static/image19.png)

Le concepteur ajoute une ligne d’association et une propriété de clé étrangère.

[![Image12](what-s-new-in-the-entity-framework-4/_static/image22.png)](what-s-new-in-the-entity-framework-4/_static/image21.png)

Vous êtes maintenant prêt à créer la base de données. Cliquez sur l’aire de conception et sélectionnez **générer la base de données à partir du modèle**.

[![Image13](what-s-new-in-the-entity-framework-4/_static/image24.png)](what-s-new-in-the-entity-framework-4/_static/image23.png)

Cette opération lance l’Assistant génération de base de données. (Si vous voyez des avertissements qui indiquent que les entités ne sont pas mappées, vous pouvez ignorer celles pour le moment.)

Dans le **choisir votre connexion de données** étape, cliquez sur **nouvelle connexion**.

[![Image14](what-s-new-in-the-entity-framework-4/_static/image26.png)](what-s-new-in-the-entity-framework-4/_static/image25.png)

Dans le **propriétés de connexion** boîte de dialogue zone, sélectionnez l’instance locale de SQL Server Express et le nom de la base de données `AlumniAsssociation`.

[![Image15](what-s-new-in-the-entity-framework-4/_static/image28.png)](what-s-new-in-the-entity-framework-4/_static/image27.png)

Cliquez sur **Oui** lorsque vous êtes invité si vous souhaitez créer la base de données. Lorsque le **choisir votre connexion de données** étape s’affiche de nouveau, cliquez sur **suivant**.

Dans le **résumé et paramètres** étape, cliquez sur **Terminer**.

[![Image18](what-s-new-in-the-entity-framework-4/_static/image30.png)](what-s-new-in-the-entity-framework-4/_static/image29.png)

Un *.sql* fichier avec les commandes data definition language (DDL) est créé, mais les commandes n’ont pas encore été exécutés.

[![Image20](what-s-new-in-the-entity-framework-4/_static/image32.png)](what-s-new-in-the-entity-framework-4/_static/image31.png)

Utiliser un outil tel que **SQL Server Management Studio** pour exécuter le script et créer les tables, comme vous avez effectué lorsque vous avez créé le `School` pour la base de données [le premier didacticiel de la série de didacticiels mise en route ](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). (Sauf si vous avez téléchargé la base de données.)

Vous pouvez maintenant utiliser le `AlumniAssociation` modèle de données dans votre site web pages de la même façon que vous utilisiez le `School` modèle. Pour l’essayer, ajoutez des données aux tables et créer une page web qui affiche les données.

À l’aide de **Explorateur de serveurs**, ajoutez les lignes suivantes à la `Alumnus` et `Donation` tables.

[![Image21](what-s-new-in-the-entity-framework-4/_static/image34.png)](what-s-new-in-the-entity-framework-4/_static/image33.png)

Créer une nouvelle page web nommée *Alumni.aspx* qui utilise le *Site.Master* page maître. Ajoutez le balisage suivant à la `Content` contrôle nommé `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample7.aspx)]

Ce balisage crée imbriqué `GridView` des contrôles, celui externe pour afficher les noms des étudiants et celle interne pour afficher les dates de dons et de quantités.

Ouvrez *Alumni.aspx.cs*. Ajouter un `using` instruction pour les données d’accéder à la couche et un gestionnaire d’expiration `GridView` du contrôle `RowDataBound` événement :

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample8.cs)]

Cette établit de code interne `GridView` contrôler à l’aide de la `Donations` propriété de navigation de la ligne actuelle `Alumnus` entité.

Exécutez la page.

[![Image22](what-s-new-in-the-entity-framework-4/_static/image36.png)](what-s-new-in-the-entity-framework-4/_static/image35.png)

(Remarque : cette page est incluse dans le projet téléchargeable, mais pour le rendre fonctionne, vous devez créer la base de données dans votre instance locale de SQL Server Express ; la base de données n’est pas inclus en tant qu’un *.mdf* de fichiers dans le *application\_ Données* dossier.)

Pour plus d’informations sur l’utilisation de la fonctionnalité de model first d’Entity Framework, consultez [Model First dans Entity Framework 4](https://msdn.microsoft.com/data/ff830362.aspx).

## <a name="poco-support"></a>Prise en charge POCO

Lorsque vous utilisez la méthodologie de conception pilotée par domaine, vous concevez des classes de données qui représentent les données et le comportement qui est pertinente pour le domaine d’entreprise. Ces classes doivent être indépendantes de toute technologie spécifique utilisée pour stocker (conserver) les données ; en d’autres termes, ils doivent être *ignorant la persistance*. Ignorance de persistance peut également rendre une classe plus facile de test unitaire, car le projet de test unitaire peut utiliser toute technologie de persistance est la plus commode pour le test. Les versions antérieures d’Entity Framework proposé prise en charge limitée pour l’ignorance de la persistance, car les classes d’entité devaient hériter la `EntityObject` classe et par conséquent inclus un grand nombre de fonctionnalités spécifiques à Entity Framework.

Entity Framework 4 introduit la possibilité d’utiliser des classes d’entité qui n’héritent la `EntityObject` classe et ne sont donc persistance ignorant la. Dans le contexte d’Entity Framework, les classes comme ceci sont généralement appelées *objet CLR traditionnel* (POCO ou OCT). Vous pouvez écrire des classes POCO manuellement, ou vous pouvez les générer automatiquement selon un modèle de données existant à l’aide de modèles Text Template Transformation Toolkit (T4) fournis par Entity Framework.

Pour plus d’informations sur l’utilisation d’oct dans Entity Framework, consultez les ressources suivantes :

- [Utilisation des entités POCO](https://msdn.microsoft.com/library/dd456853.aspx). Il s’agit d’un document MSDN qui est une vue d’ensemble d’Oct, avec des liens vers d’autres documents qui ont des informations plus détaillées.
- [Procédure pas à pas : POCO modèle pour Entity Framework](https://blogs.msdn.com/b/adonet/archive/2010/01/25/walkthrough-poco-template-for-the-entity-framework.aspx) il s’agit d’un billet de blog de l’équipe de développement Entity Framework, avec des liens vers d’autres billets de blog sur oct.

## <a name="code-first-development"></a>Développement de code First

Prise en charge POCO dans Entity Framework 4 nécessite toujours que vous créez un modèle de données et la liaison de vos classes d’entité au modèle de données. La prochaine version d’Entity Framework inclut une fonctionnalité appelée *développement code first*. Cette fonctionnalité vous permet d’utiliser Entity Framework avec vos propres classes POCO sans avoir besoin d’utiliser le Générateur de modèles de données ou un fichier XML de modèle de données. (Par conséquent, cette option a également été appelée *code uniquement*; *code first* et *code uniquement* se rapportent à la même fonctionnalité Entity Framework.)

Pour plus d’informations sur l’utilisation de l’approche code first pour le développement, consultez les ressources suivantes :

- [Développement avec Entity Framework 4 orienté code](https://weblogs.asp.net/scottgu/archive/2010/07/16/code-first-development-with-entity-framework-4.aspx). Il s’agit d’un billet de blog de développement code first présentation de Scott Guthrie.
- [Entity Framework développement Team - billets CodeOnly avec balises](https://blogs.msdn.com/b/efdesign/archive/tags/codeonly/)
- [Entity Framework développement Team - billets Code First avec balises](https://blogs.msdn.com/b/efdesign/archive/tags/code+first/)
- [Didacticiel de Store de musique MVC - partie 4 : accès aux données et modèles](../../../../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4.md)
- [Bien démarrer avec MVC 3 - partie 4 : développement de Code First de Entity Framework](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model.md)

En outre, un nouveau didacticiel MVC Code First qui génère une application similaire à l’application Contoso University devrait être publié au printemps 2011 à [https://asp.net/entity-framework/tutorials](../../../../entity-framework.md)

## <a name="more-information"></a>Informations complémentaires

Cette étape termine la vue d’ensemble à ce qui est nouveau dans Entity Framework et la poursuite de l’opération avec la série de didacticiels Entity Framework. Pour plus d’informations sur les nouvelles fonctionnalités dans Entity Framework 4 qui ne sont pas abordées ici, consultez les ressources suivantes :

- [Quelles sont les nouveautés dans ADO.NET](https://msdn.microsoft.com/library/ex6y04yf.aspx) rubrique MSDN sur les nouvelles fonctionnalités dans la version 4 d’Entity Framework.
- [Annonce de la version d’Entity Framework 4](https://blogs.msdn.com/b/efdesign/archive/2010/04/12/announcing-the-release-of-entity-framework-4.aspx) billet de blog de l’équipe de développement d’Entity Framework sur les nouvelles fonctionnalités dans la version 4.

> [!div class="step-by-step"]
> [Précédent](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
