---
title: "Tests d’intégration dans ASP.NET Core"
author: ardalis
description: "Comment utiliser des tests d’intégration ASP.NET Core pour vous assurer que les composants d’une application fonctionnent correctement."
manager: wpickett
ms.author: riande
ms.date: 09/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: testing/integration-testing
ms.openlocfilehash: 4a5f14e11de6ed91f67808c3ea8c78a7b1d43b03
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2018
---
# <a name="integration-testing-in-aspnet-core"></a>Tests d’intégration dans ASP.NET Core

Par [Steve Smith](https://ardalis.com/)

Les tests d’intégration garantissent que les composants d’une application fonctionnent correctement lors de l’assemblage. ASP.NET Core prend en charge les tests d’intégration à l’aide des infrastructures de tests unitaires et un hôte web de test intégré qui peut être utilisé pour gérer les requêtes sans surcharger le réseau.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/integration-testing/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="introduction-to-integration-testing"></a>Introduction aux tests d’intégration

Les tests d’intégration vérifient que les différentes parties d’une application fonctionnent correctement ensemble. Contrairement aux [tests unitaires](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test), les tests d’intégration impliquent souvent des considérations d’infrastructure d'application, par exemple une base de données, un système de fichiers, des ressources réseau, ou des requêtes et réponses web. Les tests unitaires utilisent des objets fakes ou mocks à la place de ces considérations, mais l’objectif deq tests d’intégration consiste à vérifier que le système fonctionne comme prévu avec ces systèmes.

Les tests d’intégration, car ils exercent des segments de code plus grande et car ils reposent sur les éléments d’infrastructure, ont tendance à être beaucoup plus lents que les tests unitaires. Par conséquent, il est judicieux de limiter le nombre de tests intégration que vous écrivez, surtout si vous pouvez tester le même comportement avec un test unitaire.

> [!NOTE]
> Si un comportement peut être testé à l’aide d’un test unitaire ou un test d’intégration, préférez le test unitaire, puisqu’il est presque toujours être plus rapide. Vous pouvez avoir des dizaines ou des centaines de tests unitaires avec de nombreuses entrées différentes mais quelques tests d’intégration qui couvrent les scénarios plus importants.

Tester la logique dans vos propres méthodes est généralement le domaine des tests unitaires. Tester le fonctionnement de votre application au sein de son infrastructure, par exemple ASP.NET Core ou avec une base de données est là où des tests d’intégration entrent en jeu. Il ne faut pas trop de tests d’intégration pour confirmer que vous êtes en mesure d’écrire une ligne en base de données et la lire. Vous n’avez pas besoin de tester chaque combinaison possible de votre code d’accès aux données, vous devez seulement tester suffisamment pour garantir que votre application fonctionne correctement.

## <a name="integration-testing-aspnet-core"></a>Intégration de test ASP.NET Core

Pour être prêt pour effectuer des tests d’intégration d’exécution, vous devez créer un projet de test, ajoutez une référence à votre projet de web ASP.NET Core et installer un test runner. Ce processus est décrit dans la documentation [test unitaire](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test), ainsi que des instructions plus détaillées sur l’exécution de tests et des recommandations pour nommer vos tests et les classes de test.

> [!NOTE]
> Séparez vos tests unitaires et vos tests d’intégration à l’aide de différents projets. Cela permet de s’assurer que vous ne'introduisez pas par accident des problèmes d’infrastructure dans vos tests unitaires et vous permet de facilement choisir l’ensemble des tests à exécuter.

### <a name="the-test-host"></a>L’hôte de Test

ASP.NET Core inclut un hôte de test qui peut être ajouté aux projets de test d’intégration et utilisé pour héberger les applications ASP.NET Core, les demandes de service de test sans la nécessité d’un hôte web réel. L’exemple fourni inclut un projet de test d’intégration qui a été configuré pour utiliser [xUnit](https://xunit.github.io) et l’hôte de Test. Il utilise le package NuGet `Microsoft.AspNetCore.TestHost`.

Une fois que le package `Microsoft.AspNetCore.TestHost` est inclus dans le projet, vous serez en mesure de créer et configurer un `TestServer` dans vos tests. Le test suivant montre comment vérifier qu’une demande adressée à la racine d’un site renvoie « Hello World ! » et doit s’exécuter avec succès par rapport à la valeur par défaut modèle ASP.NET Core Web vide créé par Visual Studio.

[!code-csharp[Main](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebDefaultRequestShould.cs?name=snippet_WebDefault&highlight=7,16,22)]

Ce test utilise le pattern Act-Assert. L’étape d’organisation est effectuée dans le constructeur, ce qui crée une instance de `TestServer`. Configurer `WebHostBuilder` permet de créer un `TestHost`; dans cet exemple, la méthode `Configure` à partir de la classe `Startup` du système sous test (SUT) est passée à la `WebHostBuilder`. Cette méthode permet de configurer le pipeline de demande du `TestServer` de manière identique à la façon dont le serveur SUT serait configuré.

Dans la partie Act du test, une demande est faite pour l'instance `TestServer` pour le chemin d’accès « / » et la réponse est lue dans une chaîne. Cette chaîne est comparée à la chaîne attendue « Hello World ! ». S’ils correspondent, le test réussit ; Sinon, elle échoue.

Vous pouvez maintenant ajouter quelques tests d’intégration pour confirmer que la vérification de la fonctionnalité fonctionne via l’application web :

[!code-csharp[Main](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebCheckPrimeShould.cs?name=snippet_CheckPrime)]

Notez que vous n’essayez pas réellement tester l’exactitude de l’outil d’analyse de nombres premiers avec ces tests mais plutôt que l’application web fait ce que vous attendez. Vous disposez déjà de la couverture de test unitaire qui vous donne confiance dans `PrimeService`, comme vous pouvez voir ici :

![Explorateur de tests](integration-testing/_static/test-explorer.png)

Plus d’informations sur les tests unitaires dans l’article [test unitaire](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test).


### <a name="integration-testing-mvcrazor"></a>Intégration de test Mvc/Razor

Les projets de test qui contiennent des vues Razor requièrent que `<PreserveCompilationContext>` soit défini à true dans le fichier *.csproj* :


```xml
    <PreserveCompilationContext>true</PreserveCompilationContext>
```
Les projets de test qui n'ont pas cet élément génère une erreur semblable à la suivante :
```
Microsoft.AspNetCore.Mvc.Razor.Compilation.CompilationFailedException: 'One or more compilation failures occurred:
ooebhccx.1bd(4,62): error CS0012: The type 'Attribute' is defined in an assembly that is not referenced. You must add a reference to assembly 'netstandard, Version=2.0.0.0, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51'.
```


## <a name="refactoring-to-use-middleware"></a>Refactorisation pour utiliser l’intergiciel (middleware)

La refactorisation est le processus de modifier le code d’application pour améliorer sa conception sans modifier son comportement. Elle doit être effectuée dans l’idéal, lorsqu’il existe une suite de tests qui passent, étant donné qu'ils garantissent que le comportement du système reste le même avant et après les modifications. En examinant la manière avec laquelle la logique de contrôle est implémentée dans la méthode `Configure` de l’application web, vous voyez :

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }

    app.Run(async (context) =>
    {
        if (context.Request.Path.Value.Contains("checkprime"))
        {
            int numberToCheck;
            try
            {
                numberToCheck = int.Parse(context.Request.QueryString.Value.Replace("?", ""));
                var primeService = new PrimeService();
                if (primeService.IsPrime(numberToCheck))
                {
                    await context.Response.WriteAsync($"{numberToCheck} is prime!");
                }
                else
                {
                    await context.Response.WriteAsync($"{numberToCheck} is NOT prime!");
                }
            }
            catch
            {
                await context.Response.WriteAsync("Pass in a number to check in the form /checkprime?5");
            }
        }
        else
        {
            await context.Response.WriteAsync("Hello World!");
        }
    });
}
```

Ce code fonctionne, mais il est loin d’être la mainère dont vous voulez implémenter ce genre de fonctionnalités dans une application ASP.NET Core, même si ça a l'air simple. Imaginez à quoi la méthode `Configure` ressemblerait si vous aviez besoin de lui ajouter cette quantité de code chaque fois que vous ajoutez un autre point de terminaison d'URL !

Une option à envisager est d'ajouter [MVC](xref:mvc/overview) à l’application et créer un contrôleur pour gérer la vérification de la méthode Prime. Toutefois, si que vous n’avez pas actuellement besoin de toutes les autres fonctionnalités MVC, c'est un peu excessif.

Vous pouvez, toutefois, tirer parti de l'[intergiciel (middleware)](xref:fundamentals/middleware/index) d’ASP.NET Core, ce qui nous aidera à encapsuler la logique de la méthode prime dans sa propre classe de contrôle et d’obtenir une meilleure [séparation des préoccupations](http://deviq.com/separation-of-concerns/) dans la méthode `Configure`.

Vous souhaitez autoriser le chemin d’accès que l’intergiciel (middleware) utilise pour être spécifié en tant que paramètre, la classe de l’intergiciel (middleware) attend une instance `RequestDelegate` et un `PrimeCheckerOptions` dans son constructeur. Si le chemin d’accès de la demande ne correspond pas à ce que cet intergiciel (middleware) est configuré pour attendre, vous appelez simplement l’intergiciel (middleware) suivant dans la chaîne et ne faites rien de plus. Le reste du code de l'implémentation dans `Configure` figure désormais dans la méthode `Invoke`.

> [!NOTE]
> Étant donné que l’intergiciel (middleware) varie selon le service `PrimeService`, vous demandez également une instance de ce service avec le constructeur. L’infrastructure fournit ce service via l'[Injection de dépendance](xref:fundamentals/dependency-injection), en supposant qu’il a été configuré, par exemple dans `ConfigureServices`.

[!code-csharp[Main](../testing/integration-testing/sample/src/PrimeWeb/Middleware/PrimeCheckerMiddleware.cs?highlight=39-63)]

Cet intergiciel (middleware) agit comme un point de terminaison dans le délégué de chaîne de demande lorsque son chemin d’accès correspond, il n’existe aucun appel à `_next.Invoke` lorsque cet intergiciel (middleware) traite la demande.

Avec cet intergiciel (middleware) en place et certaines méthodes d’extension utiles créées pour faciliter sa configuration, la méthode `Configure` refactorisée ressemble à ceci :

[!code-csharp[Main](../testing/integration-testing/sample/src/PrimeWeb/Startup.cs?highlight=9&range=19-33)]

Après cette refactorisation, vous êtes certain que l’application web fonctionne toujours comme avant, étant donné que vos tests d’intégration passent.

> [!NOTE]
> Il est conseillé de valider les modifications apportées au contrôle de code source une fois que vous effectuez une opération de refactorisation et vos tests. Si vous faites du Test Driven Development [envisagez d’ajouter la validation à votre cycle Red-Green-Refactor](https://ardalis.com/rgrc-is-the-new-red-green-refactor-for-test-first-development).

## <a name="resources"></a>Ressources

* [Tests unitaires](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Intergiciel (middleware)](xref:fundamentals/middleware/index)
* [Test des contrôleurs](xref:mvc/controllers/testing)
