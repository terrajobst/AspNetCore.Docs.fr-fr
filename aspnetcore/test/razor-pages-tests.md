---
title: Tests d’unités de Pages Razor dans ASP.NET Core
author: guardrex
description: Découvrez comment créer des tests unitaires pour les applications de Pages Razor.
ms.author: riande
ms.custom: mvc
ms.date: 11/27/2017
uid: test/razor-pages-tests
ms.openlocfilehash: 2bac84d6b6e5f30ff0cbbbc0b841833b5c9f48ab
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342404"
---
# <a name="razor-pages-unit-tests-in-aspnet-core"></a>Tests d’unités de Pages Razor dans ASP.NET Core

Par [Luke Latham](https://github.com/guardrex)

ASP.NET Core prend en charge les tests unitaires d’applications de Pages Razor. Tests des données accèdent à la couche (DAL) et garantir de modèles de page :

* Parties d’une application Pages Razor fonctionnent indépendamment et ensemble en tant qu’unité pendant la construction de l’application.
* Classes et méthodes ont limité les étendues de responsabilité.
* Il existe une documentation supplémentaire sur le comportement de l’application.
* Les régressions sont des erreurs provoquées par des mises à jour pour le code, sont trouvent pendant le déploiement et de génération automatisée.

Cette rubrique suppose que vous avez une compréhension élémentaire des applications de Pages Razor et les tests unitaires. Si vous n’êtes pas familiarisé avec les applications de Pages Razor ou les concepts des tests, consultez les rubriques suivantes :

* [Présentation des pages Razor](xref:razor-pages/index)
* [Bien démarrer avec les pages Razor](xref:tutorials/razor-pages/razor-pages-start)
* [Test unitaire c# dans .NET Core à l’aide de dotnet test et xUnit](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

L’exemple de projet se compose de deux applications :

| Application         | Dossier du projet                        | Description |
| ----------- | ------------------------------------- | ----------- |
| Application de message | *src/RazorPagesTestSample*            | Autorise un utilisateur à ajouter, supprimer une, supprimer toutes et analyser les messages. |
| Application de test    | *tests/RazorPagesTestSample.Tests*    | Utilisé pour l’application de message de test unitaire : accès aux données (DAL) de couche et de modèle de page d’Index. |

Les tests peuvent être exécutés en utilisant les fonctionnalités de test intégré d’un IDE, comme [Visual Studio](https://www.visualstudio.com/vs/). Si vous utilisez [Visual Studio Code](https://code.visualstudio.com/) ou la ligne de commande, exécutez la commande suivante à une invite de commandes dans le *tests/RazorPagesTestSample.Tests* dossier :

```console
dotnet test
```

## <a name="message-app-organization"></a>Organisation d’application de message

L’application de message est un simple système de message de Pages Razor avec les caractéristiques suivantes :

* La page d’Index de l’application (*pages/index.cshtml* et *Pages/Index.cshtml.cs*) fournit une interface utilisateur et la page des méthodes de modèle de contrôle de l’ajout, suppression et analyse des messages (mots moyenne par message) .
* Un message est décrite par le `Message` classe (*Data/Message.cs*) avec deux propriétés : `Id` (clé) et `Text` (message). Le `Text` propriété est requise et limitée à 200 caractères.
* Les messages sont stockés à l’aide de [base de données en mémoire d’Entity Framework](/ef/core/providers/in-memory/)&#8224;.
* L’application contient une couche d’accès aux données (DAL) dans sa classe de contexte de base de données, `AppDbContext` (*Data/AppDbContext.cs*). Les méthodes de la couche DAL sont marquées `virtual`, ce qui permet de simuler les méthodes à utiliser dans les tests.
* Si la base de données est vide sur le démarrage de l’application, la banque de messages est initialisée avec trois messages. Ces *amorcée messages* sont également utilisés dans les tests.

&#8224;La rubrique EF, [Test avec InMemory](/ef/core/miscellaneous/testing/in-memory), explique comment utiliser une base de données en mémoire pour les tests avec MSTest. Cette rubrique utilise le [xUnit](https://xunit.github.io/) infrastructure de test. Concepts des tests et des implémentations de test entre les frameworks de test différentes sont similaires mais pas identiques.

Bien que l’application n’utilise pas le [modèle dépôt](xref:fundamentals/repository-pattern) et n’est pas un exemple effectif de la [modèle unité de travail (UoW)](https://martinfowler.com/eaaCatalog/unitOfWork.html), Pages Razor prend en charge ces modèles de développement. Pour plus d’informations, consultez [conception de la couche de persistance d’infrastructure](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design), [implémentant le référentiel et les modèles d’unité de travail dans une Application ASP.NET MVC](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application), et [contrôleur de Test logique](/aspnet/core/mvc/controllers/testing) (l’exemple implémente le modèle de référentiel).

## <a name="test-app-organization"></a>Organisation de l’application de test

L’application de test est une application console à l’intérieur de la *tests/RazorPagesTestSample.Tests* dossier.

| Dossier d’application de test | Description |
| --------------- | ----------- |
| *UnitTests*     | <ul><li>*DataAccessLayerTest.cs* contient les tests unitaires pour la couche DAL.</li><li>*IndexPageTests.cs* contient les tests unitaires pour le modèle de page d’Index.</li></ul> |
| *Utilitaires*     | Contient le `TestingDbContextOptions` méthode utilisée pour créer des options de contexte pour chaque test unitaire de la couche DAL de base de données afin que la base de données est réinitialisé à son état initial pour chaque test. |

L’infrastructure de test est [xUnit](https://xunit.github.io/). L’objet framework de simulation est [Moq](https://github.com/moq/moq4).

## <a name="unit-tests-of-the-data-access-layer-dal"></a>Tests unitaires des données accéder à la couche DAL)

L’application de message dispose d’une DAL avec quatre méthodes contenues dans le `AppDbContext` classe (*src/RazorPagesTestSample/Data/AppDbContext.cs*). Chaque méthode possède un ou deux tests unitaires dans l’application de test.

| Méthode de la couche DAL               | Fonction                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | Obtient un `List<Message>` à partir de la base de données trié par le `Text` propriété. |
| `AddMessageAsync`        | Ajoute un `Message` à la base de données.                                          |
| `DeleteAllMessagesAsync` | Supprime tous les `Message` entrées à partir de la base de données.                           |
| `DeleteMessageAsync`     | Supprime un seul `Message` à partir de la base de données par `Id`.                      |

Nécessitent des tests unitaires de la couche DAL [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) lorsque vous créez un nouveau `AppDbContext` pour chaque test. Une approche de création la `DbContextOptions` pour chaque test est d’utiliser un [DbContextOptionsBuilder](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptionsbuilder):

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

Le problème avec cette approche est que chaque test reçoit la base de données dans l’état dans lequel le test précédent laissé. Cela peut être problématique lorsque vous tentez d’écrire des tests d’unité atomique qui n’interfèrent pas entre eux. Pour forcer le `AppDbContext` pour utiliser un nouveau contexte de base de données pour chaque test, fournissez un `DbContextOptions` instance qui est basé sur un nouveau fournisseur de services. L’application de test montre comment effectuer cette opération à l’aide de son `Utilities` méthode de classe `TestingDbContextOptions` (*tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs*) :

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

À l’aide de la `DbContextOptions` dans l’unité de la couche DAL tests permet à chaque test exécuter de manière atomique avec une instance de la nouvelle base de données :

```csharp
using (var db = new AppDbContext(Utilities.TestingDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

Chaque méthode de test dans le `DataAccessLayerTest` classe (*UnitTests/DataAccessLayerTest.cs*) suit un modèle similaire organiser Act-Assert :

1. Organiser : La base de données est configuré pour le test et/ou le résultat attendu est défini.
1. ACT : Le test est exécuté.
1. Assert : Assertions sont effectuées pour déterminer si le résultat de test est une réussite.

Par exemple, le `DeleteMessageAsync` méthode est responsable de la suppression d’un message unique identifié par son `Id` (*src/RazorPagesTestSample/Data/AppDbContext.cs*) :

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

Il existe deux tests pour cette méthode. Un test vérifie que la méthode supprime un message lorsque le message est présent dans la base de données. Les autres tests de méthode que la base de données ne change pas si le message `Id` pour suppression n’existe pas. Le `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` méthode est indiquée ci-dessous :

[!code-csharp[](razor-pages-tests/samples_snapshot/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

Tout d’abord, la méthode exécute l’étape d’organisation, où la préparation pour l’étape d’action a lieu. Les messages d’amorçage sont obtenues et maintenus dans `seedMessages`. Les messages d’amorçage sont enregistrés dans la base de données. Le message avec un `Id` de `1` est définie pour la suppression. Lorsque le `DeleteMessageAsync` méthode est exécutée, les messages attendus doivent comporter tous les messages à l’exception de celui avec une `Id` de `1`. Le `expectedMessages` variable représente ce résultat attendu.

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

La méthode agit : le `DeleteMessageAsync` méthode est exécutée en passant le `recId` de `1`:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

Enfin, la méthode obtient le `Messages` à partir du contexte et le compare à la `expectedMessages` déclarant que s’ils sont égaux :

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

Pour comparer les deux `List<Message>` sont les mêmes :

* Les messages sont classés par `Id`.
* Paires de messages sont comparées sur la `Text` propriété.

Une méthode de test similaire, `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` vérifie le résultat de la tentative de suppression d’un message qui n’existe pas. Dans ce cas, les messages attendus dans la base de données doivent être égales aux messages réels après le `DeleteMessageAsync` méthode est exécutée. Il ne doit y avoir aucune modification au contenu de la base de données :

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a>Méthodes de tests unitaires de la page Modèles

Il incombe à un autre ensemble de tests unitaires pour les tests de méthodes de modèle de page. Dans l’application de message, les modèles de page d’Index sont trouvent dans le `IndexModel` classe *src/RazorPagesTestSample/Pages/Index.cshtml.cs*.

| Méthode de modèle de page | Fonction |
| ----------------- | -------- |
| `OnGetAsync` | Obtient les messages à partir de la couche DAL pour l’interface utilisateur en utilisant le `GetMessagesAsync` (méthode). |
| `OnPostAddMessageAsync` | Si le `ModelState` est valide, les appels `AddMessageAsync` pour ajouter un message à la base de données. |
| `OnPostDeleteAllMessagesAsync` | Appels `DeleteAllMessagesAsync` pour supprimer tous les messages dans la base de données. |
| `OnPostDeleteMessageAsync` | Exécute `DeleteMessageAsync` pour supprimer un message avec le `Id` spécifié. |
| `OnPostAnalyzeMessagesAsync` | Si un ou plusieurs messages se trouvent dans la base de données, calcule le nombre moyen de mots par message. |

Les méthodes de modèle de page sont testées à l’aide de sept tests dans le `IndexPageTests` classe (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*). Les tests utilisent le modèle de disposition Assert Act familier. Ces tests se consacrent à :

* Déterminer si les méthodes suivent le comportement correct lorsque la `ModelState` n’est pas valide.
* Confirmer les méthodes produisent la bonne `IActionResult`.
* Vérification que les assignations de valeur de propriété sont correctement effectuées.

Ce groupe de tests simuler souvent les méthodes de la couche DAL pour produire des données attendues pour l’étape d’action où une méthode de modèle de page est exécutée. Par exemple, le `GetMessagesAsync` méthode de la `AppDbContext` est simulée pour produire une sortie. Lorsqu’une méthode de modèle de page exécute cette méthode, le simulacre retourne le résultat. Les données ne réside pas dans la base de données. Cette opération crée les conditions de test prévisible et fiable pour l’utilisation de la couche DAL dans les tests de modèle de page.

Le `OnGetAsync_PopulatesThePageModel_WithAListOfMessages` test montre comment la `GetMessagesAsync` méthode est simulée pour le modèle de page :

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

Lorsque le `OnGetAsync` méthode est exécutée dans l’étape d’action, elle appelle le modèle de page `GetMessagesAsync` (méthode).

Étape d’action de test unitaire (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*) :

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

`IndexPage` du modèle de page `OnGetAsync` (méthode) (*src/RazorPagesTestSample/Pages/Index.cshtml.cs*) :

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

Le `GetMessagesAsync` méthode dans la couche DAL ne retourne pas le résultat de cet appel de méthode. La version simulée de la méthode retourne le résultat.

Dans le `Assert` étape, les messages réels (`actualMessages`) sont affectées à partir du `Messages` propriété du modèle de page. Une vérification de type est également effectuée lorsque les messages sont affectés. Les messages attendues et réelles sont comparées à leurs `Text` propriétés. Le test de la méthode déclare que les deux `List<Message>` instances contiennent les mêmes messages.

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

Autres tests de ce groupe Créer page objets de modèle qui incluent le `DefaultHttpContext`, le `ModelStateDictionary`, un `ActionContext` pour établir le `PageContext`, un `ViewDataDictionary`et un `PageContext`. Ils sont utiles dans la réalisation des tests. Par exemple, l’application message établit un `ModelState` erreur avec `AddModelError` pour vérifier que valide `PageResult` est retourné lorsque `OnPostAddMessageAsync` est exécutée :

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a>Ressources supplémentaires

* [Test unitaire c# dans .NET Core à l’aide de dotnet test et xUnit](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Contrôleurs de test](xref:mvc/controllers/testing)
* [Votre Code de Test unitaire](/visualstudio/test/unit-test-your-code) (Visual Studio)
* [Tests d’intégration](xref:test/integration-tests)
* [xUnit.net](https://xunit.github.io/)
* [Prise en main xUnit.net (.NET Core/ASP.NET Core)](https://xunit.github.io/docs/getting-started-dotnet-core)
* [Moq](https://github.com/moq/moq4)
* [Guide de démarrage rapide Moq](https://github.com/Moq/moq4/wiki/Quickstart)
