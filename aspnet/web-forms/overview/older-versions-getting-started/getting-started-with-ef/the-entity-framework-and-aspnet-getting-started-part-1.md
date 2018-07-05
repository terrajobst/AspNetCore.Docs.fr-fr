---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
title: Bien démarrer avec Entity Framework 4.0 Database First et 4 d’ASP.NET Web Forms | Microsoft Docs
author: tdykstra
description: L’exemple d’application web Contoso University montre comment créer des applications Web Forms ASP.NET à l’aide de l’Entity Framework 4.0 et Visual Studio 2010...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 5cb00916-8f46-491f-be25-4739a615d619
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
msc.type: authoredcontent
ms.openlocfilehash: fbfbc667b4031c67827dc158ffca4d1b4dbfb23e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394292"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms"></a>Bien démarrer avec Entity Framework 4.0 Database First et 4 d’ASP.NET Web Forms
====================
par [Tom Dykstra](https://github.com/tdykstra)

> L’exemple d’application web Contoso University montre comment créer des applications Web Forms ASP.NET à l’aide de l’Entity Framework 4.0 et Visual Studio 2010. L’exemple d’application est un site Web pour une université Contoso fictive. Il inclut des fonctionnalités telles que l'admission d’étudiant, la création de cours et les affectations de formateur.
> 
> Le didacticiel présente des exemples en c#. Le [exemple téléchargeable](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) contient le code en c# et Visual Basic.
> 
> ## <a name="database-first"></a>Tout d’abord la base de données
> 
> Il existe trois façons, vous pouvez travailler avec des données dans Entity Framework : *Database First*, *Model First*, et *Code First*. Ce didacticiel s’adresse la première base de données. Pour plus d’informations sur les différences entre ces flux de travail et des conseils sur la façon de choisir le mieux adapté pour votre scénario, consultez [Workflows de développement Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>Web Forms
> 
> Cette série de didacticiels utilise le modèle Web Forms ASP.NET et suppose que vous savez comment travailler avec ASP.NET Web Forms dans Visual Studio. Si vous ne faites pas, consultez [bien démarrer avec Web Forms ASP.NET 4.5](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Si vous préférez travailler avec l’infrastructure ASP.NET MVC, consultez [mise en route avec Entity Framework à l’aide d’ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
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


## <a name="overview"></a>Vue d'ensemble

L’application que vous créez dans ces didacticiels est un site Web simple d’université.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image1.png)

Les utilisateurs peuvent afficher et mettre à jour les informations relatives aux étudiants, aux cours et aux formateurs. Voici quelques-uns des écrans que vous allez créer.

[![Image30](the-entity-framework-and-aspnet-getting-started-part-1/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image3.png)

[![Image37](the-entity-framework-and-aspnet-getting-started-part-1/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image5.png)

[![Image31](the-entity-framework-and-aspnet-getting-started-part-1/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image7.png)

[![Image32](the-entity-framework-and-aspnet-getting-started-part-1/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image9.png)

## <a name="creating-the-web-application"></a>Création de l’Application Web

Pour démarrer le didacticiel, ouvrez Visual Studio et créez un nouveau projet d’Application Web ASP.NET à l’aide du **Application Web ASP.NET** modèle :

[![Image01](the-entity-framework-and-aspnet-getting-started-part-1/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image11.png)

Ce modèle crée un projet d’application web qui inclut déjà une feuille de style et pages maîtres :

[![Image02](the-entity-framework-and-aspnet-getting-started-part-1/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image13.png)

Ouvrez le *Site.Master* du fichier et changer de « My ASP.NET Application » à « Contoso University ».

[!code-html[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample1.html)]

Rechercher la *Menu* contrôle nommé `NavigationMenu` et remplacez-le par le balisage suivant, qui ajoute des éléments de menu pour les pages que vous créez.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample2.aspx)]

Ouvrez le *Default.aspx* page et modifier le `Content` contrôle nommé `BodyContent` à ce :

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample3.aspx)]

Vous disposez maintenant d’une simple page d’accueil avec des liens vers les différentes pages que vous créez :

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image15.png)

## <a name="creating-the-database"></a>Création de la base de données

Pour ces didacticiels, vous allez utiliser le Concepteur de modèle de données Entity Framework pour créer automatiquement le modèle de données basé sur une base de données existante (souvent appelé le *database first* approche). Une alternative qui n’est pas couverte dans cette série de didacticiels consiste à créer le modèle de données manuellement et ensuite les Concepteur de générer des scripts qui créent la base de données (le *model first* approche).

Pour la méthode de base de données en premier utilisée dans ce didacticiel, l’étape suivante consiste à ajouter une base de données du site. Le moyen le plus simple consiste à tout d’abord télécharger le projet qui accompagne ce didacticiel. Cliquez sur le *application\_données* dossier, sélectionnez **ajouter un élément existant**, puis sélectionnez le *School.mdf* fichier de base de données du projet téléchargé.

Une alternative consiste à suivre les instructions fournies dans [création de la base de données School](https://msdn.microsoft.com/library/bb399731.aspx). Si vous téléchargez la base de données ou le créer, copier le *School.mdf* fichier dans le dossier suivant à votre application *application\_données* dossier :

`%PROGRAMFILES%\Microsoft SQL Server\MSSQL10.SQLEXPRESS\MSSQL\DATA`

(Cet emplacement de la *.mdf* fichier suppose que vous utilisez SQL Server 2008 Express.)

Si vous créez la base de données à partir d’un script, procédez comme suit pour créer un diagramme de base de données :

1. Dans **Explorateur de serveurs**, développez **des connexions de données**, développez *School.mdf*, avec le bouton droit **les schémas de base de données**, puis sélectionnez **Ajouter nouveau diagramme**.

    [![Image35](the-entity-framework-and-aspnet-getting-started-part-1/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image17.png)
2. Sélectionnez toutes les tables, puis cliquez sur **ajouter**.

    [![Image36](the-entity-framework-and-aspnet-getting-started-part-1/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image19.png)

    SQL Server crée un schéma de base de données illustrant les tables, les colonnes des tables et des relations entre les tables. Vous pouvez déplacer les tables pour les organiser comme vous le souhaitez.
3. Enregistrez le diagramme en tant que « SchoolDiagram », puis fermez-le.

Si vous téléchargez le *School.mdf* fichier qui accompagne ce didacticiel, vous pouvez afficher le diagramme de base de données en double-cliquant sur **SchoolDiagram** sous **les schémas de base de données** dans **Explorateur de serveurs**.

[![Image38](the-entity-framework-and-aspnet-getting-started-part-1/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image21.png)

Le diagramme ressemble à ceci (les tables peuvent être dans différents emplacements de celles indiquées ici) :

[![Image04](the-entity-framework-and-aspnet-getting-started-part-1/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image23.png)

## <a name="creating-the-entity-framework-data-model"></a>Création de l’infrastructure Entity Data Model

Vous pouvez maintenant créer un modèle de données Entity Framework à partir de cette base de données. Vous pouvez créer le modèle de données dans le dossier racine de l’application, mais pour ce didacticiel vous allez le placer dans un dossier nommé *DAL* (pour la couche d’accès aux données).

Dans **l’Explorateur de solutions**, ajoutez un dossier de projet nommé *DAL* (Vérifiez qu’il est sous le projet, pas sous la solution).

Cliquez sur le *DAL* dossier, puis sélectionnez **ajouter** et **un nouvel élément**. Sous **modèles installés**, sélectionnez **données**, sélectionnez le **ADO.NET Entity Data Model** modèle, nommez-le *SchoolModel.edmx*, et puis cliquez sur **ajouter**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-1/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image25.png)

Cela démarre l’Assistant Entity Data Model. Dans la première étape de l’Assistant, le **générer à partir de la base de données** option est sélectionnée par défaut. Cliquez sur **Suivant**.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-1/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image27.png)

Dans le **choisir votre connexion de données** étape, laissez les valeurs par défaut et cliquez sur **suivant**. La base de données School est sélectionné par défaut et le paramètre de connexion est enregistré dans le *Web.config* de fichiers **SchoolEntities**.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-1/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image29.png)

Dans le **choisir vos objets de base de données** étape de l’Assistant, sélectionnez toutes les tables à l’exception `sysdiagrams` (qui a été créé pour le diagramme que vous avez généré précédemment) puis cliquez sur **Terminer**.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-1/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image31.png)

Une fois qu’il a terminé la création du modèle, Visual Studio affiche une représentation graphique des objets Entity Framework (entités) qui correspondent à vos tables de base de données. (Comme avec le schéma de base de données, l’emplacement des éléments individuels peut être différent de ce que vous voyez dans cette illustration. Vous pouvez faire glisser les éléments environ pour correspondre à l’illustration, si vous le souhaitez.)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-1/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image33.png)

## <a name="exploring-the-entity-framework-data-model"></a>Exploration du modèle de données Entity Framework

Vous pouvez voir que le diagramme de l’entité est très similaire au diagramme de base de données, avec quelques différences. Une différence est l’ajout de symboles à la fin de chaque association qui indiquent le type d’association (relations entre les tables sont appelées des associations d’entités dans le modèle de données) :

- Une association un-à-zéro-ou-un est représentée par « 1 » et « 0.. 1 ».

    [![Image39](the-entity-framework-and-aspnet-getting-started-part-1/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image35.png)

    Dans ce cas, un `Person` entité peut ou ne peut pas être associée à un `OfficeAssignment` entité. Un `OfficeAssignment` entité doit être associée à un `Person` entité. En d’autres termes, un formateur peut ou ne peut pas être assigné à un bureau, et n’importe quel office peut être affecté à uniquement un formateur.
- Une association un-à-plusieurs est représentée par « 1 » et «\*».

    [![Image40](the-entity-framework-and-aspnet-getting-started-part-1/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image37.png)

    Dans ce cas, un `Person` entité peut ou ne peut pas avoir associé `StudentGrade` entités. Un `StudentGrade` entité doit être associée à un `Person` entité. `StudentGrade` en fait, les entités représentent des cours inscrits dans cette base de données ; Si un étudiant est inscrit dans un cours et il n’existe encore, aucun niveau de la `Grade` propriété a la valeur null. En d’autres termes, un étudiant ne peut pas être inscrits dans n’importe quel cours a encore été, peut-être être inscrits à un cours ou peut-être être inscrits dans plusieurs cours. Chaque niveau scolaire dans un cours inscrit s’applique à uniquement un seul étudiant.
- Une association plusieurs-à-plusieurs est représentée par «\*« et »\*».

    [![Image41](the-entity-framework-and-aspnet-getting-started-part-1/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image39.png)

    Dans ce cas, un `Person` entité peut ou ne peut pas avoir associé `Course` entités et l’inverse est également vrai : une `Course` entité peut ou ne peut pas avoir associé `Person` entités. En d’autres termes, un formateur peut-être enseigner plusieurs cours, et un cours peut être animé par plusieurs formateurs. (Dans cette base de données, cette relation s’applique uniquement aux formateurs ; il ne lie pas les étudiants aux cours. Les étudiants sont liés à des cours par la table StudentGrades.)

Une autre différence entre le schéma de base de données et le modèle de données est supplémentaires **propriétés de Navigation** section pour chaque entité. Une propriété de navigation d’une entité fait référence à des entités associées. Par exemple, le `Courses` propriété dans un `Person` entité contient une collection de tous les `Course` entités associées à ce `Person` entité.

[![Image12](the-entity-framework-and-aspnet-getting-started-part-1/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image41.png)

Encore une autre différence entre le modèle de base de données et des données est l’absence de la `CourseInstructor` tableau d’association qui est utilisé dans la base de données pour lier le `Person` et `Course` dans une relation plusieurs-à-plusieurs. Les propriétés de navigation vous permettent d’obtenir liées `Course` entités à partir de la `Person` entité et connexes `Person` entités à partir de la `Course` entité, il est donc pas nécessaire pour représenter la table d’association dans le modèle de données.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-1/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image43.png)

Pour les besoins de ce didacticiel, supposons que le `FirstName` colonne de la `Person` table contienne en fait d’une personne prénom et le deuxième prénom. Vous souhaitez modifier le nom du champ afin de refléter ceci, mais l’administrateur de base de données (DBA) pouvez pas modifier la base de données. Vous pouvez modifier le nom de la `FirstName` inchangé de propriété dans le modèle de données, tout en conservant sa base de données équivalent.

Dans le concepteur, cliquez sur **FirstName** dans le `Person` entité, puis sélectionnez **renommer**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-1/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image45.png)

Tapez le nouveau nom de « FirstMidName ». Cela modifie la façon de que vous faire référence à la colonne dans le code sans modifier la base de données.

[![Image29](the-entity-framework-and-aspnet-getting-started-part-1/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image47.png)

L’Explorateur de modèles est un autre moyen d’afficher la structure de base de données, la structure de modèle de données et le mappage entre eux. Pour voir comment cela, cliquez sur une zone vide dans le Concepteur d’entités, puis **Explorateur de modèles**.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-1/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image49.png)

Le **Explorateur de modèles** volet affiche une arborescence. (Le **Explorateur de modèles** volet peut être ancré avec le **l’Explorateur de solutions** volet.) Le **SchoolModel** nœud représente la structure de modèle de données et le **SchoolModel.Store** nœud représente la structure de base de données.

[![Image26](the-entity-framework-and-aspnet-getting-started-part-1/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image51.png)

Développez **SchoolModel.Store** pour afficher les tables, développez **Tables / vues** pour voir les tables, puis développez **cours** pour afficher les colonnes dans une table.

[![Image19](the-entity-framework-and-aspnet-getting-started-part-1/_static/image54.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image53.png)

Développez **SchoolModel**, développez **Types d’entité**, puis développez le **cours** nœud pour afficher les entités et les propriétés au sein des entités.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-1/_static/image56.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image55.png)

Dans le des concepteurs ou **Explorateur de modèles** volet, vous pouvez voir comment Entity Framework lie les objets des deux modèles. Cliquez sur le `Person` entité, puis sélectionnez **mappage de Table**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-1/_static/image58.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image57.png)

Cette opération ouvre le **détails de Mapping** fenêtre. Notez que cette fenêtre vous permet de voir que la colonne de base de données `FirstName` est mappé à `FirstMidName`, qui est ce que vous avez renommé à dans le modèle de données.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-1/_static/image60.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image59.png)

Entity Framework utilise XML pour stocker des informations sur la base de données, le modèle de données et les mappages entre eux. Le *SchoolModel.edmx* fichier est en fait un fichier XML qui contient ces informations. Le concepteur affiche les informations contenues dans un format graphique, mais vous pouvez également afficher le fichier au format XML en cliquant sur le *.edmx* fichier **l’Explorateur de solutions**, puis sur **ouvrir avec**et en sélectionnant **éditeur XML (texte)**. (Le Générateur de modèles de données et un éditeur XML sont simplement deux façons différentes d’ouvrir et travailler avec le même fichier, donc vous ne pouvez pas le concepteur ouvrir et ouvrez le fichier dans un éditeur XML en même temps.)

Vous venez de créer un site Web, une base de données et un modèle de données. La procédure suivante, vous commencerez utilisation des données à l’aide du modèle de données et ASP.NET `EntityDataSource` contrôle.

> [!div class="step-by-step"]
> [Next](the-entity-framework-and-aspnet-getting-started-part-2.md)
