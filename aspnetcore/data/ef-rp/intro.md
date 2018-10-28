---
title: Pages Razor avec Entity Framework Core dans ASP.NET Core - Tutoriel 1 sur 8
author: rick-anderson
description: Montre comment créer une application Pages Razor à l’aide d’Entity Framework Core.
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/intro
ms.openlocfilehash: f714934a2ef804813290ae0c04432470a2ba5816
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477564"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a>Pages Razor avec Entity Framework Core dans ASP.NET Core - Tutoriel 1 sur 8

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

Par [Tom Dykstra](https://github.com/tdykstra) et [Rick Anderson](https://twitter.com/RickAndMSFT)

L’exemple d’application web Contoso University montre comment créer une application web Razor Pages ASP.NET Core à l’aide d’Entity Framework (EF) Core.

L’exemple d’application est un site web pour une université Contoso fictive. Il comprend des fonctionnalités telles que l’admission des étudiants, la création des cours et les affectations des formateurs. Cette page est la première d’une série de didacticiels qui expliquent comment générer l’exemple d’application Contoso University.

[Télécharger ou afficher l’application complète.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Télécharger les instructions](xref:tutorials/index#how-to-download-a-sample).

## <a name="prerequisites"></a>Prérequis

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

[!INCLUDE [](~/includes/2.1-SDK.md)]

------

Connaissance des [Pages Razor](xref:razor-pages/index). Les programmeurs débutants doivent lire [Bien démarrer avec les pages Razor](xref:tutorials/razor-pages/razor-pages-start) avant de démarrer cette série.

## <a name="troubleshooting"></a>Résolution des problèmes

Si vous rencontrez un problème que vous ne pouvez pas résoudre, vous pouvez généralement trouver la solution en comparant votre code au [projet terminé](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples). Un bon moyen d’obtenir de l’aide est de publier une question sur [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) pour [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) ou [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).

## <a name="the-contoso-university-web-app"></a>L’application web Contoso University

L’application générée dans ces didacticiels est le site web de base d’une université.

Les utilisateurs peuvent afficher et mettre à jour les informations relatives aux étudiants, aux cours et aux formateurs. Voici quelques-uns des écrans créés dans le didacticiel.

![Page d’index des étudiants](intro/_static/students-index.png)

![Page de modification des étudiants](intro/_static/student-edit.png)

Le style de l’interface utilisateur de ce site est proche de ce qui est généré par les modèles prédéfinis. Le didacticiel est axé sur Core EF avec des pages Razor, et non sur l’interface utilisateur.

## <a name="create-the-contosouniversity-razor-pages-web-app"></a>Créer l’application web Razor Pages ContosoUniversity

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.
* Créez une application web ASP.NET Core. Nommez le projet **ContosoUniversity**. Il est important de nommer le projet *ContosoUniversity* afin que les espaces de noms correspondent quand le code est copié/collé.
* Sélectionnez **ASP.NET Core 2.1** dans la liste déroulante, puis sélectionnez **Application web**.

Pour les images des étapes précédentes, consultez [Créer une application web Razor](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-web-app).
Exécutez l’application.

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

```CLI
dotnet new webapp -o ContosoUniversity
cd ContosoUniversity
dotnet run
```

------

## <a name="set-up-the-site-style"></a>Configurer le style du site

Quelques changements permettent de définir le menu, la disposition et la page d’accueil du site. Mettez à jour *Pages/Shared/_Layout.cshtml* avec les changements suivants :

* Remplacez chaque occurrence de « ContosoUniversity » par « Contoso University ». Il y a trois occurrences.

* Ajoutez des entrées de menu pour **Students**, **Courses**, **Instructors** et **Departments**, et supprimez l’entrée de menu **Contact**.

Les modifications sont mises en surbrillance. (Tout le balisage n’est *pas* affiché.)

[!code-html[](intro/samples/cu21/Pages/Shared/_Layout.cshtml?highlight=6,29,35-38,50&name=snippet)]

Dans *Pages/Index.cshtml*, remplacez le contenu du fichier par le code suivant afin de remplacer le texte sur ASP.NET et MVC par le texte concernant cette application :

[!code-html[](intro/samples/cu21/Pages/Index.cshtml)]

## <a name="create-the-data-model"></a>Créer le modèle de données

Créez des classes d’entités pour l’application Contoso University. Commencez par les trois entités suivantes :

![Diagramme de modèle de données Cours-Inscription-Étudiant](intro/_static/data-model-diagram.png)

Il existe une relation un-à-plusieurs entre les entités `Student` et `Enrollment`. Il existe une relation un-à-plusieurs entre les entités `Course` et `Enrollment`. Un étudiant peut s’inscrire à autant de cours qu’il le souhaite. Un cours peut avoir une quantité illimitée d’élèves inscrits.

Dans les sections suivantes, une classe pour chacune de ces entités est créée.

### <a name="the-student-entity"></a>L’entité Student

![Diagramme de l’entité Student](intro/_static/student-entity.png)

Créez un dossier *Models*. Dans le dossier *Models*, créez un fichier de classe nommé *Student.cs* avec le code suivant :

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Intro)]

La propriété `ID` devient la colonne de clé primaire de la table de base de données qui correspond à cette classe. Par défaut, EF Core interprète une propriété nommée `ID` ou `classnameID` comme clé primaire. Dans `classnameID`, `classname` est le nom de la classe. L’autre clé primaire reconnue automatiquement est `StudentID` dans l’exemple précédent.

La propriété `Enrollments` est une [propriété de navigation](/ef/core/modeling/relationship). Les propriétés de navigation établissent des liaisons à d’autres entités qui sont associées à cette entité. Ici, la propriété `Enrollments` d’un `Student entity` contient toutes les entités `Enrollment` associées à ce `Student`. Par exemple, si une ligne Student dans la base de données a deux lignes Enrollment associées, la propriété de navigation `Enrollments` contient ces deux entités `Enrollment`. Une ligne `Enrollment` associée est une ligne qui contient la valeur de clé primaire de cette étudiant dans la colonne `StudentID`. Par exemple, supposez que l’étudiant avec ID=1 a deux lignes dans la table `Enrollment`. La table `Enrollment` a deux lignes avec `StudentID` = 1. `StudentID` est une clé étrangère dans la table `Enrollment` qui spécifie l’étudiant dans la table `Student`.

Si une propriété de navigation peut contenir plusieurs entités, la propriété de navigation doit être un type de liste, tel que `ICollection<T>`. Vous pouvez spécifier `ICollection<T>`, ou un type tel que `List<T>` ou `HashSet<T>`. Quand vous utilisez `ICollection<T>`, EF Core crée une collection `HashSet<T>` par défaut. Les propriétés de navigation qui contiennent plusieurs entités proviennent de relations plusieurs-à-plusieurs et un-à-plusieurs.

### <a name="the-enrollment-entity"></a>L’entité Enrollment

![Diagramme de l’entité Enrollment](intro/_static/enrollment-entity.png)

Dans le dossier *Models*, créez un fichier *Enrollment.cs* avec le code suivant :

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Intro)]

La propriété `EnrollmentID` est la clé primaire. Cette entité utilise le modèle `classnameID` au lieu de `ID` comme l’entité `Student`. En général, les développeurs choisissent un modèle et l’utilisent dans tout le modèle de données. Dans un prochain didacticiel, nous verrons qu’utiliser ID sans classname simplifie l’implémentation de l’héritage dans le modèle de données.

La propriété `Grade` est un `enum`. Le point d’interrogation après la déclaration de type `Grade` indique que la propriété `Grade` est nullable. Une note (Grade) qui a la valeur Null est différente d’une note égale à zéro : la valeur Null signifie qu’une note n’est pas connue ou n’a pas encore été affectée.

La propriété `StudentID` est une clé étrangère, et la propriété de navigation correspondante est `Student`. Une entité `Enrollment` est associée à une entité `Student`. Par conséquent, la propriété contient une seule entité `Student`. L’entité `Student` diffère de la propriété de navigation `Student.Enrollments`, qui contient plusieurs entités `Enrollment`.

La propriété `CourseID` est une clé étrangère, et la propriété de navigation correspondante est `Course`. Une entité `Enrollment` est associée à une entité `Course`.

EF Core interprète une propriété en tant que clé étrangère si elle se nomme `<navigation property name><primary key property name>`. Par exemple, `StudentID` pour la propriété de navigation `Student`, puisque la clé primaire de l’entité `Student` est `ID`. Les propriétés de clé étrangère peuvent également se nommer `<primary key property name>`. Par exemple, `CourseID` puisque la clé primaire de l’entité `Course` est `CourseID`.

### <a name="the-course-entity"></a>L’entité Course

![Diagramme de l’entité Course](intro/_static/course-entity.png)

Dans le dossier *Models*, créez un fichier *Course.cs* avec le code suivant :

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Intro)]

La propriété `Enrollments` est une propriété de navigation. Une entité `Course` peut être associée à un nombre quelconque d’entités `Enrollment`.

L’attribut `DatabaseGenerated` permet à l’application de spécifier la clé primaire, plutôt que de la faire générer par la base de données.

## <a name="scaffold-the-student-model"></a>Générer automatiquement le modèle d’étudiant

Dans cette section, le modèle d’étudiant est généré automatiquement. Autrement dit, l’outil de génération de modèles automatique génère des pages pour les opérations de création (Create), de lecture (Read), de mise à jour (Update) et de suppression (Delete) (CRUD) pour le modèle d’étudiant.

* Générez le projet.
* Créez le dossier *Pages/Students*.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *Pages/Students* > **Ajouter** > **Nouvel élément généré automatiquement**.
* Dans la boîte de dialogue **Ajouter un modèle automatique**, sélectionnez **Pages Razor avec Entity Framework (CRUD)** > **AJOUTER**.

Renseignez la boîte de dialogue **Pages Razor avec Entity Framework (CRUD)** :

* Dans la liste déroulante **Classe de modèle**, sélectionnez **Student (ContosoUniversity.Models)**.
* Dans la ligne **Classe de contexte de données**, sélectionnez le signe **+** (plus) et remplacez le nom généré par **ContosoUniversity.Models.SchoolContext**.
* Dans la liste déroulante **Classe de contexte de données**, sélectionnez **ContosoUniversity.Models.SchoolContext**
* Sélectionnez **Ajouter**.

![Boîte de dialogue CRUD](intro/_static/s1.png)

Consultez [Générer automatiquement le modèle de film](xref:tutorials/razor-pages/model#scaffold-the-movie-model) si vous rencontrez un problème à l’étape précédente.

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

Exécutez les commandes suivantes pour générer automatiquement le modèle d’étudiant.

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Models.SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
```
------

Le processus de génération de modèles automatique a créé et changé les fichiers suivants :

### <a name="files-created"></a>Fichiers créés

* *Pages/Students* Create, Delete, Details, Edit, Index.
* *Data/SchoolContext.cs*

### <a name="file-updates"></a>Mises à jour du fichier

* *Startup.cs* : Les changements apportés à ce fichier sont détaillés dans la section suivante.
* *appSettings.JSON* : La chaîne de connexion utilisée pour se connecter à une base de données locale est ajoutée.

## <a name="examine-the-context-registered-with-dependency-injection"></a>Examiner le contexte inscrit avec l’injection de dépendances

ASP.NET Core comprend [l’injection de dépendances](xref:fundamentals/dependency-injection). Des services (tels que le contexte de base de données EF Core) sont inscrits avec l’injection de dépendances au démarrage de l’application. Ces services sont affectés aux composants qui les nécessitent (par exemple les Pages Razor) par le biais de paramètres de constructeur. Le code du constructeur qui obtient une instance de contexte de base de données est indiqué plus loin dans le didacticiel.

L’outil de génération de modèles automatique a créé automatiquement un contexte de base de données et l’a inscrit dans le conteneur d’injection de dépendances.

Examinez la méthode `ConfigureServices` dans *Startup.cs*. La ligne en surbrillance a été ajoutée par l’outil de génération de modèles automatique :

[!code-csharp[](intro/samples/cu21/Startup.cs?name=snippet_SchoolContext&highlight=13-14)]

Le nom de la chaîne de connexion est transmis au contexte en appelant une méthode sur un objet [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions). Pour le développement local, le [système de configuration ASP.NET Core](xref:fundamentals/configuration/index) lit la chaîne de connexion à partir du fichier *appsettings.json*.

## <a name="update-main"></a>Mettre à jour la méthode Main

Dans *Program.cs*, modifiez la méthode `Main` pour effectuer les opérations suivantes :

* Obtenir une instance de contexte de base de données à partir du conteneur d’injection de dépendances.
* Appelez [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).
* Supprimez le contexte une fois la méthode `EnsureCreated` exécutée.

Le code suivant montre le fichier *Program.cs* mis à jour.

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet)]

`EnsureCreated` garantit que la base de données existe pour le contexte. Si elle existe, aucune action n’est effectuée. Si elle n’existe pas, la base de données et tous ses schéma sont créés. `EnsureCreated` n’utilise pas de migrations pour créer la base de données. Une base de données créée avec `EnsureCreated` ne peut pas être mise à jour à l’aide de migrations par la suite.

`EnsureCreated` est appelée au démarrage de l’application, ce qui active le flux de travail suivant :

* Supprimez la base de données.
* Modification du schéma de base de données (par exemple, ajout d’un champ `EmailAddress`).
* Exécutez l’application.
* `EnsureCreated` crée une base de données avec la colonne `EmailAddress`.

`EnsureCreated` est pratique au début du développement quand le schéma évolue rapidement. Plus loin dans le tutoriel, la base de données est supprimée et les migrations sont utilisées.

### <a name="test-the-app"></a>Tester l’application

Exécutez l’application et acceptez la politique de cookies. Cette application ne conserve pas les informations personnelles. Vous pouvez en savoir plus sur la politique de cookies à la section [Prise en charge du règlement général sur la protection des données (RGPD)](xref:security/gdpr).

* Sélectionnez le lien **Students**, puis **Créer nouveau**.
* Testez les liens Edit, Details et Delete.

## <a name="examine-the-schoolcontext-db-context"></a>Examiner le contexte de base de données SchoolContext

La classe principale qui coordonne les fonctionnalités d’EF Core pour un modèle de données spécifié est la classe de contexte de base de données. Le contexte de données est dérivé de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext). Il spécifie les entités qui sont incluses dans le modèle de données. Dans ce projet, la classe est nommée `SchoolContext`.

Mettez à jour *SchoolContext.cs* avec le code suivant :

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_Intro&highlight=12-14)]

Le code en surbrillance crée une propriété [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) pour chaque jeu d’entités. Dans la terminologie d’EF Core :

* Un jeu d’entités correspond généralement à une table de base de données.
* Une entité correspond à une ligne dans la table.

`DbSet<Enrollment>` et `DbSet<Course>` peuvent être omis. EF Core les inclut implicitement, car l’entité `Student` référence l’entité `Enrollment`, et l’entité `Enrollment` référence l’entité `Course`. Pour ce didacticiel, conservez `DbSet<Enrollment>` et `DbSet<Course>` dans le `SchoolContext`.

### <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

La chaîne de connexion spécifie [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb). LocalDB est une version allégée du moteur de base de données SQL Server Express. Elle est destinée au développement d’applications, et non à une utilisation en production. LocalDB démarre à la demande et s’exécute en mode utilisateur, ce qui n’implique aucune configuration complexe. Par défaut, LocalDB crée des fichiers de base de données *.mdf* dans le répertoire `C:/Users/<user>`.

## <a name="add-code-to-initialize-the-db-with-test-data"></a>Ajouter du code pour initialiser la base de données avec des données de test

EF Core crée une base de données vide. Dans cette section, une méthode `Initialize` est écrite pour la remplir avec des données de test.

Dans le dossier *Data*, créez un fichier de classe nommé *DbInitializer.cs* et ajoutez le code suivant :

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Intro)]

Le code vérifie s’il existe des étudiants dans la base de données. S’il n’y en a pas, la base de données est initialisée avec des données de test. Il charge les données de test dans les tableaux plutôt que dans les collections `List<T>` afin d’optimiser les performances.

La méthode `EnsureCreated` crée automatiquement la base de données pour le contexte de base de données. Si la base de données existe, `EnsureCreated` retourne sans modifier la base de données.

Dans *Program.cs*, modifiez la méthode `Main` pour appeler `Initialize` :

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet2&highlight=14-15)]

Supprimez les enregistrements d’étudiants et redémarrez l’application. Si la base de données n’est pas initialisée, définissez un point d’arrêt dans `Initialize` pour diagnostiquer le problème.

## <a name="view-the-db"></a>Afficher la base de données

Ouvrez **l’Explorateur d’objets SQL Server** (SSOX) à partir du menu **Affichage** de Visual Studio.
Dans SSOX, cliquez sur **(localdb)\MSSQLLocalDB > Bases de données > ContosoUniversity1**.

Développez le nœud **Tables**.

Cliquez avec le bouton droit sur la table **Student** et cliquez sur **Afficher les données** pour voir les colonnes créées et les lignes insérées dans la table.

## <a name="asynchronous-code"></a>Code asynchrone

La programmation asynchrone est le mode par défaut pour ASP.NET Core et EF Core.

Un serveur web a un nombre limité de threads disponibles et, dans les situations de forte charge, tous les threads disponibles peuvent être utilisés. Quand cela se produit, le serveur ne peut pas traiter de nouvelle requête tant que les threads ne sont pas libérés. Avec le code synchrone, plusieurs threads peuvent être bloqués alors qu’ils n’effectuent en fait aucun travail, car ils attendent que des E/S se terminent. Avec le code asynchrone, quand un processus attend que des E/S se terminent, son thread est libéré afin d’être utilisé par le serveur pour traiter d’autres demandes. Il permet ainsi d’utiliser les ressources serveur plus efficacement, et le serveur peut gérer plus de trafic sans retard.

Le code asynchrone introduit néanmoins une petite surcharge au moment de l’exécution. Dans les situations de faible trafic, le gain de performances est négligeable, tandis qu’en cas de trafic élevé l’amélioration potentielle des performances est importante.

Dans le code suivant, le mot clé [async](/dotnet/csharp/language-reference/keywords/async), la valeur renvoyée `Task<T>`, le mot clé `await` et la méthode `ToListAsync` déclenchent l’exécution asynchrone du code.

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* Le mot clé `async` fait en sorte que le compilateur :
  * Génère des rappels pour les parties du corps de méthode.
  * Crée automatiquement l’objet [Task](/dotnet/api/system.threading.tasks.task) qui est retourné. Pour plus d’informations, consultez [Type de retour Task](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).

* Le type de retour implicite `Task` représente le travail en cours.
* Le mot clé `await` fait en sorte que le compilateur fractionne la méthode en deux parties. La première partie se termine par l’opération qui est démarrée de façon asynchrone. La seconde partie est placée dans une méthode de rappel qui est appelée quand l’opération se termine.
* `ToListAsync` est la version asynchrone de la méthode d’extension `ToList`.

Voici quelques éléments à connaître lors de l’écriture de code asynchrone qui utilise EF Core :

* Seules les instructions qui provoquent l’envoi de requêtes ou de commandes vers la base de données sont exécutées de façon asynchrone. Cela comprend `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync` et `SaveChangesAsync`, mais pas les instructions qui ne font que changer un `IQueryable`, telles que `var students = context.Students.Where(s => s.LastName == "Davolio")`.
* Un contexte EF Core n’est pas thread-safe : n’essayez pas d’effectuer plusieurs opérations en parallèle.
* Pour tirer parti des avantages de performances du code asynchrone, vérifiez que les packages de bibliothèque (par exemple pour la pagination) utilisent le mode asynchrone s’ils appellent des méthodes EF Core qui envoient des requêtes à la base de données.

Pour plus d’informations sur la programmation asynchrone dans .NET, consultez [Vue d’ensemble d’Async](/dotnet/articles/standard/async) et [Programmation asynchrone avec async et await](/dotnet/csharp/programming-guide/concepts/async/).

Dans le didacticiel suivant, nous allons examiner les opérations CRUD de base (créer, lire, mettre à jour, supprimer).

::: moniker-end

> [!div class="step-by-step"]
> [Next](xref:data/ef-rp/crud)
