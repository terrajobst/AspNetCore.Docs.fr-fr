---
title: Tester la logique des contrôleurs dans ASP.NET Core
author: ardalis
description: Découvrez plus d’informations sur le test de la logique des contrôleurs dans ASP.NET Core avec Moq et xUnit.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/controllers/testing
ms.openlocfilehash: d0b2a25d00187c088671be147844aa892f824c6e
ms.sourcegitcommit: 64c2ca86fff445944b155635918126165ee0f8aa
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/18/2018
ms.locfileid: "41751752"
---
# <a name="test-controller-logic-in-aspnet-core"></a>Tester la logique des contrôleurs dans ASP.NET Core

Par [Steve Smith](https://ardalis.com/)

Les contrôleurs sont une partie centrale des applications ASP.NET Core MVC. Par conséquent, vous devez avoir entièrement confiance dans le fait qu’ils se comportent comme prévu pour votre application. Les tests automatisés peuvent vous donner cette confiance et détecter des erreurs avant la mise en production. Il est important d’éviter de donner des responsabilités inutiles à vos contrôleurs et de faire en sorte que vos tests portent seulement sur les responsabilités du contrôleur.

La logique d’un contrôleur doit être minimale et ne pas porter sur la logique métier ni sur les problèmes d’infrastructure (par exemple l’accès aux données). Testez la logique du contrôleur et non pas le framework. Testez la façon dont le contrôleur *se comporte* en fonction d’entrées valides et non valides. Testez les réponses du contrôleur sur la base du résultat de l’opération métier qu’il effectue.

Voici les responsabilités classiques d’un contrôleur :

* Vérifier `ModelState.IsValid`
* Retourner une réponse d’erreur si `ModelState` n’est pas valide
* Extraire une entité métier de son stockage.
* Effectuer une action sur l’entité métier.
* Enregistrer l’entité métier dans un stockage.
* Retourner un `IActionResult` approprié.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="unit-tests-of-controller-logic"></a>Tests unitaires de la logique de contrôleur

Les [tests unitaires](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) impliquent le test d’une partie d’une application de façon isolée par rapport à son infrastructure et à ses dépendances. Lors du test de la logique d’un contrôleur, seul le contenu d’une action est testé, et non pas le comportement de ses dépendances ou du framework lui-même. Quand vous procédez à des tests unitaires des actions de votre contrôleur, pensez à vous concentrer seulement sur son comportement. Les tests unitaires d’un contrôleur évitent les éléments tels que les [filtres](xref:mvc/controllers/filters), le [routage](xref:fundamentals/routing) ou la [liaison de modèle](xref:mvc/models/model-binding). Étant consacrés au test d’une seule chose, les tests unitaires sont généralement simples à écrire et rapides à exécuter. Un ensemble de tests unitaires bien écrits peut être exécuté fréquemment sans engendrer une surcharge importante. Cependant, les tests unitaires ne détectent pas les problèmes d’interaction entre les composants, qui sont l’objet des [tests d’intégration](xref:test/integration-tests).

Si vous écrivez des routes et des filtres personnalisés, vous devez exécuter sur eux des tests unitaires de manière isolée, et non dans le cadre de vos tests sur une action spécifique d’un contrôleur.

> [!TIP]
> [Créer et exécuter des tests unitaires avec Visual Studio](/visualstudio/test/unit-test-your-code)

Pour voir ce que sont les tests unitaires, commencez par examiner le contrôleur suivant. Il affiche une liste de sessions de brainstorming et permet la création de nouvelles sessions avec une requête POST :

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/HomeController.cs?highlight=12,16,21,42,43)]

Le contrôleur suit le [principe des dépendances explicites](http://deviq.com/explicit-dependencies-principle/), attendant qu’une injection de dépendances lui fournisse une instance de `IBrainstormSessionRepository`. Ceci le rend relativement facile à tester avec un framework d’objets fictifs, comme [Moq](https://www.nuget.org/packages/Moq/). La méthode `HTTP GET Index` n’a pas de boucle ni de branchement, et elle appelle seulement une méthode. Pour tester cette méthode `Index`, nous devons vérifier qu’un `ViewResult` est retourné, avec un `ViewModel` provenant de la méthode `List` du référentiel.

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=17-18&range=1-33,76-95)]

La méthode `HomeController` `HTTP POST Index` (montrée ci-dessus) doit vérifier :

* La méthode d’action retourne une erreur Demande incorrecte : `ViewResult`, avec les données appropriées quand `ModelState.IsValid` est a la valeur `false`.

* La méthode `Add` sur le référentiel est appelée et un `RedirectToActionResult` est retourné avec les arguments corrects quand `ModelState.IsValid` est défini sur true.

Un état de modèle non valide peut être testé en ajoutant des erreurs avec `AddModelError`, comme le montre le premier test ci-dessous.

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=8,15-16,37-39&range=35-75)]

Le premier test vérifie que quand `ModelState` n’est pas valide, le même `ViewResult` est retourné, comme pour une requête `GET`. Notez que le test ne tente pas de passer un modèle non valide. Ceci ne fonctionnerait de toute façon pas, car la liaison de modèle n’est pas en cours d’exécution (même si un [test d’intégration](xref:test/integration-tests) utiliserait une liaison de modèle de test). Dans ce cas, la liaison de modèle n’est pas testée. Ces tests unitaires testent seulement ce que fait le code dans la méthode d’action.

Le second test vérifie que quand `ModelState` est valide, une nouvelle `BrainstormSession` est ajoutée (via le référentiel), et que la méthode retourne un `RedirectToActionResult` avec les propriétés attendues. Les appels fictifs qui ne sont pas appelés sont normalement ignorés, mais l’appel de `Verifiable` à la fin de l’appel de configuration lui permet d’être vérifié dans le test. Ceci se fait avec l’appel à `mockRepo.Verify`, qui échoue au test si la méthode attendue n’a pas été appelée.

> [!NOTE]
> La bibliothèque Moq utilisée dans cet exemple facilite la combinaison d’éléments fictifs vérifiables (ou « stricts ») avec des éléments fictifs non vérifiables (également appelés éléments ou stubs « lâches»). Découvrez plus d’informations sur la [personnalisation des éléments fictifs avec Moq](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior).

Un autre contrôleur de l’application affiche les informations relatives à une session de brainstorming particulière. Il inclut une logique pour traiter les valeurs d’ID non valides :

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs?highlight=19,20,21,22,25,26,27,28)]

L’action du contrôleur a trois cas à tester, une pour chaque instruction `return` :

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/SessionControllerTests.cs?highlight=27,28,29,46,47,64,65,66,67,68)]

L’application expose des fonctionnalités comme une API web (une liste d’idées associées à une session de brainstorming et une méthode pour ajouter de nouvelles idées à une session) :

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?highlight=21,22,27,30,31,32,33,34,35,36,41,42,46,52,65)]

La méthode `ForSession` retourne une liste de types `IdeaDTO`. Évitez de retourner vos entités du domaine métier directement via des appels d’API, car ils incluent souvent plus de données que ce dont le client de l’API a besoin, et ils couplent inutilement le modèle du domaine interne de votre application avec l’API que vous exposez en externe. Le mappage entre les entités de domaine et les types que vous retournez sur le fil peut être effectué manuellement (avec une requête LINQ `Select` comme indiqué ici) ou en utilisant une bibliothèque comme [AutoMapper](https://github.com/AutoMapper/AutoMapper).

Les tests unitaires pour les méthodes d’API `Create` et `ForSession` :

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?highlight=18,23,29,33,38-39,43,50,58-59,68-70,76-78&range=1-83,121-135)]

Comme indiqué précédemment, pour tester le comportement de la méthode quand `ModelState` n’est pas valide, ajoutez une erreur de modèle au contrôleur dans le cadre du test. N’essayez pas de tester la validation de modèle ou la liaison de modèle dans vos tests unitaires : testez simplement le comportement de votre méthode d’action quand elle est confrontée à une valeur de `ModelState` particulière.

Le deuxième test dépend du retour d’une valeur null par le référentiel : le référentiel fictif est donc configuré pour retourner une valeur null. Il est inutile de créer une base de données de test (dans la mémoire ou ailleurs) et de construire une requête qui retourne ce résultat : ceci peut se faire en une seule instruction, comme vous pouvez le voir.

Le dernier test vérifie que la méthode `Update` du référentiel est appelée. Comme nous l’avons fait précédemment, l’élément fictif est appelé avec `Verifiable`, puis la méthode `Verify` du référentiel fictif est appelée pour vérifier que la méthode vérifiable a été exécutée. Il n’est pas de la responsabilité d’un test unitaire de garantir que la méthode `Update` a enregistré les données ; ceci peut être vérifié avec un test d’intégration.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:test/integration-tests>
