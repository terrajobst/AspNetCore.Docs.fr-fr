---
title: Razor Pages les tests unitaires dans ASP.NET Core
author: guardrex
description: Découvrez comment créer des tests unitaires pour les applications Razor Pages.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/14/2019
uid: test/razor-pages-tests
ms.openlocfilehash: afac97d686ef190ebb92d20a55a15dd774b0d1de
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081424"
---
# <a name="razor-pages-unit-tests-in-aspnet-core"></a>Razor Pages les tests unitaires dans ASP.NET Core

Par [Luke Latham](https://github.com/guardrex)

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core prend en charge les tests unitaires d’applications Razor Pages. Les tests de la couche d’accès aux données (DAL) et des modèles de page permettent de garantir :

* Les parties d’une application Razor Pages fonctionnent indépendamment et ensemble en tant qu’unité lors de la construction de l’application.
* Les classes et les méthodes ont des étendues de responsabilité limitées.
* Il existe une documentation supplémentaire sur le comportement de l’application.
* Les régressions, qui sont des erreurs résultant des mises à jour du code, sont détectées lors de la génération et du déploiement automatisés.

Cette rubrique suppose que vous avez une compréhension de base des applications Razor Pages et des tests unitaires. Si vous n’êtes pas familiarisé avec les applications de Razor Pages ou les concepts de test, consultez les rubriques suivantes :

* <xref:razor-pages/index>
* <xref:tutorials/razor-pages/razor-pages-start>
* [Tests C# unitaires dans .net core à l’aide de dotnet test et xUnit](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

L’exemple de projet est composé de deux applications :

| Application         | Dossier du projet                     | Description |
| ----------- | ---------------------------------- | ----------- |
| Application de message | *src/RazorPagesTestSample*         | Permet à un utilisateur d’ajouter un message, de supprimer un message, de supprimer tous les messages et d’analyser les messages (recherche le nombre moyen de mots par message). |
| Tester l’application    | *tests/RazorPagesTestSample.Tests* | Utilisé pour effectuer un test unitaire de la couche DAL et du modèle de page d’index de l’application message. |

Les tests peuvent être exécutés à l’aide des fonctionnalités de test intégrées d’un environnement de développement intégré (IDE), telles que [Visual Studio](/visualstudio/test/unit-test-your-code) ou [Visual Studio pour Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution). Si vous utilisez [Visual Studio code](https://code.visualstudio.com/) ou la ligne de commande, exécutez la commande suivante à partir d’une invite de commandes dans le dossier *tests/RazorPagesTestSample. tests* :

```dotnetcli
dotnet test
```

## <a name="message-app-organization"></a>Organisation des applications de messages

L’application de message est un système de messagerie Razor Pages avec les caractéristiques suivantes :

* La page d’index de l’application (*pages/index. cshtml* et *pages/index. cshtml. cs*) fournit une interface utilisateur et des méthodes de modèle de page pour contrôler l’ajout, la suppression et l’analyse des messages (recherchez le nombre moyen de mots par message).
* Un message est décrit par la `Message` classe (*Data/message. cs*) avec deux propriétés : `Id` (Key) et `Text` (message). La `Text` propriété est obligatoire et limitée à 200 caractères.
* Les messages sont stockés à l’aide&#8224; [de la base de données en mémoire de Entity Framework](/ef/core/providers/in-memory/).
* L’application contient une couche DAL dans sa classe de contexte `AppDbContext` de base de données, (*Data/AppDbContext. cs*). Les méthodes Dal sont marquées `virtual`, ce qui permet de simuler les méthodes à utiliser dans les tests.
* Si la base de données est vide au démarrage de l’application, la Banque de messages est initialisée avec trois messages. Ces *messages amorcés* sont également utilisés dans les tests.

&#8224;La rubrique EF, [test with InMemory](/ef/core/miscellaneous/testing/in-memory), explique comment utiliser une base de données en mémoire pour les tests avec MSTest. Cette rubrique utilise l’infrastructure de test [xUnit](https://xunit.github.io/) . Les concepts de test et les implémentations de test dans différents frameworks de test sont similaires, mais pas identiques.

Bien que l’exemple d’application n’utilise pas le modèle de référentiel et ne soit pas un exemple efficace du [modèle UOW (Unit of Work)](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor pages prend en charge ces modèles de développement. Pour plus d’informations, consultez [conception de la couche de persistance de l’infrastructure](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) et <xref:mvc/controllers/testing> (l’exemple implémente le modèle de référentiel).

## <a name="test-app-organization"></a>Organisation des applications de test

L’application de test est une application console dans le dossier *tests/RazorPagesTestSample. tests* .

| Dossier d’application de test | Description |
| --------------- | ----------- |
| *UnitTests*     | <ul><li>*DataAccessLayerTest.cs* contient les tests unitaires pour la couche DAL.</li><li>*IndexPageTests.cs* contient les tests unitaires pour le modèle de page d’index.</li></ul> |
| *Utilitaires*     | Contient la `TestDbContextOptions` méthode utilisée pour créer de nouvelles options de contexte de base de données pour chaque test unitaire dal afin que la base de données soit réinitialisée à sa condition de base pour chaque test. |

L’infrastructure de test est [xUnit](https://xunit.github.io/). L’infrastructure de simulacre d’objets est [MOQ](https://github.com/moq/moq4).

## <a name="unit-tests-of-the-data-access-layer-dal"></a>Tests unitaires de la couche d’accès aux données (DAL)

L’application de message a une couche DAL avec quatre méthodes contenues dans la `AppDbContext` classe (*src/RazorPagesTestSample/Data/AppDbContext. cs*). Chaque méthode a un ou deux tests unitaires dans l’application de test.

| Méthode DAL               | Fonction                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | Obtient un `List<Message>` à partir de la base de données `Text` triée par la propriété. |
| `AddMessageAsync`        | Ajoute un `Message` à la base de données.                                          |
| `DeleteAllMessagesAsync` | Supprime toutes les `Message` entrées de la base de données.                           |
| `DeleteMessageAsync`     | Supprime un unique `Message` de la base de données `Id`par.                      |

Les tests unitaires de la <xref:Microsoft.EntityFrameworkCore.DbContextOptions> couche DAL nécessitent lors `AppDbContext` de la création d’un nouveau pour chaque test. Une approche de la `DbContextOptions` création de pour chaque test consiste à utiliser un : <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder>

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

Le problème de cette approche est que chaque test reçoit la base de données dans l’État où le test précédent l’a quitté. Cela peut être problématique quand vous tentez d’écrire des tests unitaires atomiques qui n’interfèrent pas entre eux. Pour forcer le `AppDbContext` à utiliser un nouveau contexte de base de données pour chaque test `DbContextOptions` , fournissez une instance basée sur un nouveau fournisseur de services. L’application de test montre comment effectuer cette opération à `Utilities` l’aide `TestDbContextOptions` de sa méthode de classe (*tests/RazorPagesTestSample. tests/Utilities/Utilities. cs*) :

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

L’utilisation `DbContextOptions` de dans les tests unitaires dal permet à chaque test de s’exécuter atomiquement avec une nouvelle instance de base de données :

```csharp
using (var db = new AppDbContext(Utilities.TestDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

Chaque méthode de test de `DataAccessLayerTest` la classe (*UnitTests/DataAccessLayerTest. cs*) suit un modèle arrange-Act-Assert similaire :

1. Positionner La base de données est configurée pour le test et/ou le résultat attendu est défini.
1. Répondre Le test est exécuté.
1. Assert Des assertions sont effectuées pour déterminer si le résultat du test est un succès.

Par exemple, la `DeleteMessageAsync` méthode est chargée de supprimer un seul message identifié par son `Id` (*src/RazorPagesTestSample/Data/AppDbContext. cs*) :

[!code-csharp[](razor-pages-tests/samples/3.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

Il existe deux tests pour cette méthode. Un test vérifie que la méthode supprime un message lorsque le message est présent dans la base de données. L’autre méthode teste que la base de données ne change `Id` pas si le message à supprimer n’existe pas. La `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` méthode est illustrée ci-dessous :

[!code-csharp[](razor-pages-tests/samples_snapshot/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

Tout d’abord, la méthode exécute l’étape de réorganisation, où la préparation de l’étape Act a lieu. Les messages d’amorçage sont obtenus et conservés dans `seedMessages`. Les messages d’amorçage sont enregistrés dans la base de données. Le message avec un `Id` de `1` est défini pour suppression. Lorsque la `DeleteMessageAsync` méthode est exécutée, les messages attendus doivent avoir tous les messages à l’exception de celui `Id` avec `1`un de. La `expectedMessages` variable représente ce résultat attendu.

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

La méthode agit : La `DeleteMessageAsync` méthode est exécutée en passant `recId` dans `1`le de :

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

Enfin, la méthode obtient le `Messages` à partir du contexte et le compare à la `expectedMessages` déclaration que les deux sont égales :

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

Pour comparer les deux `List<Message>` , procédez comme suit :

* Les messages sont triés `Id`par.
* Les paires de messages sont comparées sur la `Text` propriété.

Une méthode `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` de test similaire vérifie le résultat de la tentative de suppression d’un message qui n’existe pas. Dans ce cas, les messages attendus dans la base de données doivent être identiques aux messages réels `DeleteMessageAsync` après l’exécution de la méthode. Aucune modification n’est apportée au contenu de la base de données :

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a>Tests unitaires des méthodes de modèle de page

Un autre ensemble de tests unitaires est responsable des tests des méthodes de modèle de page. Dans l’application message, les modèles de page d’index se trouvent `IndexModel` dans la classe de *src/RazorPagesTestSample/pages/index. cshtml. cs*.

| Méthode du modèle de page | Fonction |
| ----------------- | -------- |
| `OnGetAsync` | Obtient les messages de la couche DAL de l’interface utilisateur à `GetMessagesAsync` l’aide de la méthode. |
| `OnPostAddMessageAsync` | Si le [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) est valide, appelle `AddMessageAsync` pour ajouter un message à la base de données. |
| `OnPostDeleteAllMessagesAsync` | Appelle `DeleteAllMessagesAsync` pour supprimer tous les messages de la base de données. |
| `OnPostDeleteMessageAsync` | Exécute pour supprimer un message avec le `Id` spécifié. `DeleteMessageAsync` |
| `OnPostAnalyzeMessagesAsync` | Si un ou plusieurs messages se trouvent dans la base de données, calcule le nombre moyen de mots par message. |

Les méthodes de modèle de page sont testées à l' `IndexPageTests` aide de sept tests de la classe (*tests/RazorPagesTestSample. tests/UnitTests/IndexPageTests. cs*). Les tests utilisent le modèle « arrange-Assert-Act » familier. Ces tests portent sur les éléments suivants :

* Déterminer si les méthodes suivent le comportement correct lorsque le [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) n’est pas valide.
* La confirmation des méthodes génère la bonne <xref:Microsoft.AspNetCore.Mvc.IActionResult>.
* Vérification que les assignations de valeurs de propriété sont effectuées correctement.

Ce groupe de tests simule souvent les méthodes de la couche DAL pour produire les données attendues pour l’étape Act dans laquelle une méthode de modèle de page est exécutée. Par exemple, la `GetMessagesAsync` méthode `AppDbContext` de est factice pour produire une sortie. Lorsqu’une méthode de modèle de page exécute cette méthode, le simulacre retourne le résultat. Les données ne proviennent pas de la base de données. Cela crée des conditions de test prévisibles et fiables pour l’utilisation de la couche DAL dans les tests de modèle de page.

Le `OnGetAsync_PopulatesThePageModel_WithAListOfMessages` test montre comment la `GetMessagesAsync` méthode est factice pour le modèle de page :

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

Quand la `OnGetAsync` méthode est exécutée dans l’étape Act, elle appelle la méthode du `GetMessagesAsync` modèle de page.

Étape Act de test unitaire (*tests/RazorPagesTestSample. tests/UnitTests/IndexPageTests. cs*) :

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

`IndexPage`méthode du modèle `OnGetAsync` de page (*src/RazorPagesTestSample/pages/index. cshtml. cs*) :

[!code-csharp[](razor-pages-tests/samples/3.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

La `GetMessagesAsync` méthode dans la couche DAL ne retourne pas le résultat de cet appel de méthode. La version factice de la méthode retourne le résultat.

À l' `Assert` étape, les messages réels (`actualMessages`) sont affectés à partir `Messages` de la propriété du modèle de page. Une vérification de type est également effectuée quand les messages sont assignés. Les messages attendus et réels sont comparés `Text` par leurs propriétés. Le test déclare que les deux `List<Message>` instances contiennent les mêmes messages.

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

D’autres tests de ce groupe créent des objets de modèle de <xref:Microsoft.AspNetCore.Http.DefaultHttpContext>page qui <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary>incluent le <xref:Microsoft.AspNetCore.Mvc.ActionContext> , le, `PageContext`un pour `ViewDataDictionary`établir le, `PageContext`un et un. Ils sont utiles pour effectuer des tests. Par exemple, l’application de message établit `ModelState` une erreur <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*> avec pour vérifier qu’une <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult> valeur valide est `OnPostAddMessageAsync` retournée lorsque est exécuté :

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a>Ressources supplémentaires

* [Tests C# unitaires dans .net core à l’aide de dotnet test et xUnit](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* <xref:mvc/controllers/testing>
* [Test unitaire de votre code](/visualstudio/test/unit-test-your-code) (Visual Studio)
* <xref:test/integration-tests>
* [xUnit.net](https://xunit.github.io/)
* [Génération d’une solution .NET Core complète sur macOS à l’aide de Visual Studio pour Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution)
* [Prise en main de xUnit.net : Utilisation de .NET Core avec la ligne de commande du kit de développement logiciel (SDK) .NET](https://xunit.github.io/docs/getting-started-dotnet-core)
* [Moq](https://github.com/moq/moq4)
* [Démarrage rapide de MOQ](https://github.com/Moq/moq4/wiki/Quickstart)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core prend en charge les tests unitaires d’applications Razor Pages. Les tests de la couche d’accès aux données (DAL) et des modèles de page permettent de garantir :

* Les parties d’une application Razor Pages fonctionnent indépendamment et ensemble en tant qu’unité lors de la construction de l’application.
* Les classes et les méthodes ont des étendues de responsabilité limitées.
* Il existe une documentation supplémentaire sur le comportement de l’application.
* Les régressions, qui sont des erreurs résultant des mises à jour du code, sont détectées lors de la génération et du déploiement automatisés.

Cette rubrique suppose que vous avez une compréhension de base des applications Razor Pages et des tests unitaires. Si vous n’êtes pas familiarisé avec les applications de Razor Pages ou les concepts de test, consultez les rubriques suivantes :

* <xref:razor-pages/index>
* <xref:tutorials/razor-pages/razor-pages-start>
* [Tests C# unitaires dans .net core à l’aide de dotnet test et xUnit](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

L’exemple de projet est composé de deux applications :

| Application         | Dossier du projet                     | Description |
| ----------- | ---------------------------------- | ----------- |
| Application de message | *src/RazorPagesTestSample*         | Permet à un utilisateur d’ajouter un message, de supprimer un message, de supprimer tous les messages et d’analyser les messages (recherche le nombre moyen de mots par message). |
| Tester l’application    | *tests/RazorPagesTestSample.Tests* | Utilisé pour effectuer un test unitaire de la couche DAL et du modèle de page d’index de l’application message. |

Les tests peuvent être exécutés à l’aide des fonctionnalités de test intégrées d’un environnement de développement intégré (IDE), telles que [Visual Studio](/visualstudio/test/unit-test-your-code) ou [Visual Studio pour Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution). Si vous utilisez [Visual Studio code](https://code.visualstudio.com/) ou la ligne de commande, exécutez la commande suivante à partir d’une invite de commandes dans le dossier *tests/RazorPagesTestSample. tests* :

```dotnetcli
dotnet test
```

## <a name="message-app-organization"></a>Organisation des applications de messages

L’application de message est un système de messagerie Razor Pages avec les caractéristiques suivantes :

* La page d’index de l’application (*pages/index. cshtml* et *pages/index. cshtml. cs*) fournit une interface utilisateur et des méthodes de modèle de page pour contrôler l’ajout, la suppression et l’analyse des messages (recherchez le nombre moyen de mots par message).
* Un message est décrit par la `Message` classe (*Data/message. cs*) avec deux propriétés : `Id` (Key) et `Text` (message). La `Text` propriété est obligatoire et limitée à 200 caractères.
* Les messages sont stockés à l’aide&#8224; [de la base de données en mémoire de Entity Framework](/ef/core/providers/in-memory/).
* L’application contient une couche DAL dans sa classe de contexte `AppDbContext` de base de données, (*Data/AppDbContext. cs*). Les méthodes Dal sont marquées `virtual`, ce qui permet de simuler les méthodes à utiliser dans les tests.
* Si la base de données est vide au démarrage de l’application, la Banque de messages est initialisée avec trois messages. Ces *messages amorcés* sont également utilisés dans les tests.

&#8224;La rubrique EF, [test with InMemory](/ef/core/miscellaneous/testing/in-memory), explique comment utiliser une base de données en mémoire pour les tests avec MSTest. Cette rubrique utilise l’infrastructure de test [xUnit](https://xunit.github.io/) . Les concepts de test et les implémentations de test dans différents frameworks de test sont similaires, mais pas identiques.

Bien que l’exemple d’application n’utilise pas le modèle de référentiel et ne soit pas un exemple efficace du [modèle UOW (Unit of Work)](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor pages prend en charge ces modèles de développement. Pour plus d’informations, consultez [conception de la couche de persistance de l’infrastructure](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) et <xref:mvc/controllers/testing> (l’exemple implémente le modèle de référentiel).

## <a name="test-app-organization"></a>Organisation des applications de test

L’application de test est une application console dans le dossier *tests/RazorPagesTestSample. tests* .

| Dossier d’application de test | Description |
| --------------- | ----------- |
| *UnitTests*     | <ul><li>*DataAccessLayerTest.cs* contient les tests unitaires pour la couche DAL.</li><li>*IndexPageTests.cs* contient les tests unitaires pour le modèle de page d’index.</li></ul> |
| *Utilitaires*     | Contient la `TestDbContextOptions` méthode utilisée pour créer de nouvelles options de contexte de base de données pour chaque test unitaire dal afin que la base de données soit réinitialisée à sa condition de base pour chaque test. |

L’infrastructure de test est [xUnit](https://xunit.github.io/). L’infrastructure de simulacre d’objets est [MOQ](https://github.com/moq/moq4).

## <a name="unit-tests-of-the-data-access-layer-dal"></a>Tests unitaires de la couche d’accès aux données (DAL)

L’application de message a une couche DAL avec quatre méthodes contenues dans la `AppDbContext` classe (*src/RazorPagesTestSample/Data/AppDbContext. cs*). Chaque méthode a un ou deux tests unitaires dans l’application de test.

| Méthode DAL               | Fonction                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | Obtient un `List<Message>` à partir de la base de données `Text` triée par la propriété. |
| `AddMessageAsync`        | Ajoute un `Message` à la base de données.                                          |
| `DeleteAllMessagesAsync` | Supprime toutes les `Message` entrées de la base de données.                           |
| `DeleteMessageAsync`     | Supprime un unique `Message` de la base de données `Id`par.                      |

Les tests unitaires de la <xref:Microsoft.EntityFrameworkCore.DbContextOptions> couche DAL nécessitent lors `AppDbContext` de la création d’un nouveau pour chaque test. Une approche de la `DbContextOptions` création de pour chaque test consiste à utiliser un : <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder>

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

Le problème de cette approche est que chaque test reçoit la base de données dans l’État où le test précédent l’a quitté. Cela peut être problématique quand vous tentez d’écrire des tests unitaires atomiques qui n’interfèrent pas entre eux. Pour forcer le `AppDbContext` à utiliser un nouveau contexte de base de données pour chaque test `DbContextOptions` , fournissez une instance basée sur un nouveau fournisseur de services. L’application de test montre comment effectuer cette opération à `Utilities` l’aide `TestDbContextOptions` de sa méthode de classe (*tests/RazorPagesTestSample. tests/Utilities/Utilities. cs*) :

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

L’utilisation `DbContextOptions` de dans les tests unitaires dal permet à chaque test de s’exécuter atomiquement avec une nouvelle instance de base de données :

```csharp
using (var db = new AppDbContext(Utilities.TestDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

Chaque méthode de test de `DataAccessLayerTest` la classe (*UnitTests/DataAccessLayerTest. cs*) suit un modèle arrange-Act-Assert similaire :

1. Positionner La base de données est configurée pour le test et/ou le résultat attendu est défini.
1. Répondre Le test est exécuté.
1. Assert Des assertions sont effectuées pour déterminer si le résultat du test est un succès.

Par exemple, la `DeleteMessageAsync` méthode est chargée de supprimer un seul message identifié par son `Id` (*src/RazorPagesTestSample/Data/AppDbContext. cs*) :

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

Il existe deux tests pour cette méthode. Un test vérifie que la méthode supprime un message lorsque le message est présent dans la base de données. L’autre méthode teste que la base de données ne change `Id` pas si le message à supprimer n’existe pas. La `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` méthode est illustrée ci-dessous :

[!code-csharp[](razor-pages-tests/samples_snapshot/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

Tout d’abord, la méthode exécute l’étape de réorganisation, où la préparation de l’étape Act a lieu. Les messages d’amorçage sont obtenus et conservés dans `seedMessages`. Les messages d’amorçage sont enregistrés dans la base de données. Le message avec un `Id` de `1` est défini pour suppression. Lorsque la `DeleteMessageAsync` méthode est exécutée, les messages attendus doivent avoir tous les messages à l’exception de celui `Id` avec `1`un de. La `expectedMessages` variable représente ce résultat attendu.

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

La méthode agit : La `DeleteMessageAsync` méthode est exécutée en passant `recId` dans `1`le de :

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

Enfin, la méthode obtient le `Messages` à partir du contexte et le compare à la `expectedMessages` déclaration que les deux sont égales :

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

Pour comparer les deux `List<Message>` , procédez comme suit :

* Les messages sont triés `Id`par.
* Les paires de messages sont comparées sur la `Text` propriété.

Une méthode `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` de test similaire vérifie le résultat de la tentative de suppression d’un message qui n’existe pas. Dans ce cas, les messages attendus dans la base de données doivent être identiques aux messages réels `DeleteMessageAsync` après l’exécution de la méthode. Aucune modification n’est apportée au contenu de la base de données :

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a>Tests unitaires des méthodes de modèle de page

Un autre ensemble de tests unitaires est responsable des tests des méthodes de modèle de page. Dans l’application message, les modèles de page d’index se trouvent `IndexModel` dans la classe de *src/RazorPagesTestSample/pages/index. cshtml. cs*.

| Méthode du modèle de page | Fonction |
| ----------------- | -------- |
| `OnGetAsync` | Obtient les messages de la couche DAL de l’interface utilisateur à `GetMessagesAsync` l’aide de la méthode. |
| `OnPostAddMessageAsync` | Si le [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) est valide, appelle `AddMessageAsync` pour ajouter un message à la base de données. |
| `OnPostDeleteAllMessagesAsync` | Appelle `DeleteAllMessagesAsync` pour supprimer tous les messages de la base de données. |
| `OnPostDeleteMessageAsync` | Exécute pour supprimer un message avec le `Id` spécifié. `DeleteMessageAsync` |
| `OnPostAnalyzeMessagesAsync` | Si un ou plusieurs messages se trouvent dans la base de données, calcule le nombre moyen de mots par message. |

Les méthodes de modèle de page sont testées à l' `IndexPageTests` aide de sept tests de la classe (*tests/RazorPagesTestSample. tests/UnitTests/IndexPageTests. cs*). Les tests utilisent le modèle « arrange-Assert-Act » familier. Ces tests portent sur les éléments suivants :

* Déterminer si les méthodes suivent le comportement correct lorsque le [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) n’est pas valide.
* La confirmation des méthodes génère la bonne <xref:Microsoft.AspNetCore.Mvc.IActionResult>.
* Vérification que les assignations de valeurs de propriété sont effectuées correctement.

Ce groupe de tests simule souvent les méthodes de la couche DAL pour produire les données attendues pour l’étape Act dans laquelle une méthode de modèle de page est exécutée. Par exemple, la `GetMessagesAsync` méthode `AppDbContext` de est factice pour produire une sortie. Lorsqu’une méthode de modèle de page exécute cette méthode, le simulacre retourne le résultat. Les données ne proviennent pas de la base de données. Cela crée des conditions de test prévisibles et fiables pour l’utilisation de la couche DAL dans les tests de modèle de page.

Le `OnGetAsync_PopulatesThePageModel_WithAListOfMessages` test montre comment la `GetMessagesAsync` méthode est factice pour le modèle de page :

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

Quand la `OnGetAsync` méthode est exécutée dans l’étape Act, elle appelle la méthode du `GetMessagesAsync` modèle de page.

Étape Act de test unitaire (*tests/RazorPagesTestSample. tests/UnitTests/IndexPageTests. cs*) :

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

`IndexPage`méthode du modèle `OnGetAsync` de page (*src/RazorPagesTestSample/pages/index. cshtml. cs*) :

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

La `GetMessagesAsync` méthode dans la couche DAL ne retourne pas le résultat de cet appel de méthode. La version factice de la méthode retourne le résultat.

À l' `Assert` étape, les messages réels (`actualMessages`) sont affectés à partir `Messages` de la propriété du modèle de page. Une vérification de type est également effectuée quand les messages sont assignés. Les messages attendus et réels sont comparés `Text` par leurs propriétés. Le test déclare que les deux `List<Message>` instances contiennent les mêmes messages.

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

D’autres tests de ce groupe créent des objets de modèle de <xref:Microsoft.AspNetCore.Http.DefaultHttpContext>page qui <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary>incluent le <xref:Microsoft.AspNetCore.Mvc.ActionContext> , le, `PageContext`un pour `ViewDataDictionary`établir le, `PageContext`un et un. Ils sont utiles pour effectuer des tests. Par exemple, l’application de message établit `ModelState` une erreur <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*> avec pour vérifier qu’une <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult> valeur valide est `OnPostAddMessageAsync` retournée lorsque est exécuté :

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a>Ressources supplémentaires

* [Tests C# unitaires dans .net core à l’aide de dotnet test et xUnit](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* <xref:mvc/controllers/testing>
* [Test unitaire de votre code](/visualstudio/test/unit-test-your-code) (Visual Studio)
* <xref:test/integration-tests>
* [xUnit.net](https://xunit.github.io/)
* [Génération d’une solution .NET Core complète sur macOS à l’aide de Visual Studio pour Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution)
* [Prise en main de xUnit.net : Utilisation de .NET Core avec la ligne de commande du kit de développement logiciel (SDK) .NET](https://xunit.github.io/docs/getting-started-dotnet-core)
* [Moq](https://github.com/moq/moq4)
* [Démarrage rapide de MOQ](https://github.com/Moq/moq4/wiki/Quickstart)

::: moniker-end
