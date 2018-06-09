---
title: Tests d’intégration dans ASP.NET Core
author: guardrex
description: Découvrez comment les tests d’intégration garantissent que les composants d’une application fonctionnent correctement au niveau de l’infrastructure, y compris la base de données, le système de fichiers et le réseau.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/30/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: test/integration-tests
ms.openlocfilehash: a402c3f5f6a75917eba4058e6cc6926f25b214d4
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "35217587"
---
# <a name="integration-tests-in-aspnet-core"></a>Tests d’intégration dans ASP.NET Core

Par [Luke Latham](https://github.com/guardrex) et [Steve Smith](https://ardalis.com/)

Tests d’intégration vous assurer que les composants d’une application fonctionnent correctement à un niveau qui inclut l’infrastructure de prise en charge de l’application, telles que la base de données, le système de fichiers et le réseau. ASP.NET Core prend en charge les tests d’intégration à l’aide d’une infrastructure de tests unitaires avec un hôte du test web et un serveur de test en mémoire.

Cette rubrique suppose une connaissance élémentaire de tests unitaires. Si êtes pas familiarisé avec les concepts de test, consultez le [exécuter des tests unitaires dans .NET Core et .NET Standard](/dotnet/core/testing/) rubrique et son contenu lié.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

L’exemple d’application est une application de Pages Razor et suppose une connaissance élémentaire de Pages Razor. Si êtes pas familiarisé avec les Pages Razor, consultez les rubriques suivantes :

* [Présentation des pages Razor](xref:mvc/razor-pages/index)
* [Bien démarrer avec les pages Razor](xref:tutorials/razor-pages/razor-pages-start)
* [Tests unitaires de Pages Razor](xref:test/razor-pages-tests)

## <a name="introduction-to-integration-tests"></a>Introduction aux tests d’intégration

Tests d’intégration évaluent les composants d’une application sur un niveau plus large que [tests unitaires](/dotnet/core/testing/). Tests unitaires sont utilisés pour tester les composants du logiciel isolé, telles que les méthodes de classe individuels. Tests d’intégration confirmer que deux ou plusieurs composants d’application fonctionnent ensemble pour produire un résultat attendu, éventuellement, y compris tous les composants requis pour traiter complètement une demande.

Ces tests plus larges sont utilisés pour tester l’application d’infrastructure et de framework entière, souvent, y compris les composants suivants :

* Base de données
* Système de fichiers
* Dispositifs réseau
* Pipeline de requête-réponse

Composants d’utilisation fabriquée, appelés des tests unitaires *fakes* ou *simuler des objets*, à la place des composants d’infrastructure.

Contrairement aux tests unitaires, tests d’intégration :

* Utiliser les composants réels utilisé par l’application en production.
* Nécessite plus de code et de traitement de données.
* Prendre plus de temps.

Par conséquent, limitez l’utilisation de tests d’intégration pour les scénarios d’infrastructure plus importantes. Si un comportement peut être testé à l’aide d’un test unitaire ou un test d’intégration, sélectionnez le test unitaire.

> [!TIP]
> N’écrivez des tests d’intégration pour chaque combinaison possible d’accès de fichiers et de données avec les systèmes de fichiers et de bases de données. Indépendamment de la place dans une application interagir avec les bases de données et les systèmes de fichiers, un ensemble ayant le focus de la lecture, écriture, mise à jour et supprimer l’intégration tests généralement compatibles avec suffisamment de test de la base de données et les composants du système de fichiers. Utilisez les tests unitaires pour les tests de la logique de la méthode routines qui interagissent avec ces composants. Dans les tests unitaires, l’utilisation de l’infrastructure fakes/mocks entraîne l’exécution du test plus rapide.

> [!NOTE]
> Dans les discussions de tests d’intégration, le projet testé est souvent appelé la *système sous test*, ou « SUT » pour la forme courte.

## <a name="aspnet-core-integration-tests"></a>Tests d’intégration ASP.NET Core

Tests d’intégration dans ASP.NET Core besoin des éléments suivants :

* Un projet de test est utilisé pour contenir et exécuter les tests. Le projet de test a une référence au projet testé ASP.NET Core, appelée la *système sous test* (SUT). _« SUT » est utilisé dans cette rubrique pour faire référence à l’application testée._
* Le projet de test crée un hôte de test web pour le SUT et utilise un client de serveur de test pour gérer les demandes et réponses pour le SUT.
* Un test runner est utilisé pour exécuter les tests et les rapports les résultats des tests.

Tests d’intégration suivent une séquence d’événements qui incluent les commandes *disposition*, *Act*, et *Assert* étapes de test :

1. Hôte de le SUT web est configuré.
1. Un client de serveur de test est créé pour soumettre des demandes à l’application.
1. Le *disposition* étape de test est exécuté : l’application test prépare une demande.
1. Le *Act* étape de test est exécuté : le client envoie la demande et reçoit la réponse.
1. Le *Assert* étape de test est exécuté : le *réel* réponse est validée comme une *passer* ou *échouer* selon une *attendu*  réponse.
1. Le processus continue jusqu'à ce que tous les tests sont exécutés.
1. Les résultats des tests sont signalés.

En règle générale, l’hôte du test web est configuré différemment que l’hôte web normale de l’application pour le test s’exécute. Par exemple, une base de données ou des paramètres de l’autre application peuvent servir pour les tests.

Composants d’infrastructure, tels que l’hôte du test web et le serveur de test en mémoire ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)), sont fournis ou gérés par le [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) package. Utilisation de ce package simplifie la création de test et de l’exécution.

Le `Microsoft.AspNetCore.Mvc.Testing` package gère les tâches suivantes :

* Copie le fichier de dépendances (*\*.deps*) à partir de la SUT dans le projet de test *bin* dossier.
* Définit la racine du contenu à la racine du projet de la SUT afin que les fichiers statiques et pages/vues sont trouvent lorsque les tests sont exécutés.
* Fournit la [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) classe afin de rationaliser l’amorçage le SUT avec `TestServer`.

Le [tests unitaires](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) documentation décrit comment configurer un test projet et test runner, ainsi que des instructions détaillées sur l’exécution des tests et des recommandations sur la manière de tests de nom et classes de test.

> [!NOTE]
> Lorsque vous créez un projet de test pour une application, séparez les tests unitaires à partir de tests d’intégration dans des projets différents. Cela permet de garantir que les composants d’infrastructure test ne sont pas accidentellement inclus dans les tests unitaires. Séparation des tests unitaires et intégration permet également à contrôler sur quel jeu de tests sont exécutés.

Il n’existe pratiquement aucune différence entre la configuration pour les tests d’applications de Pages Razor et les applications MVC. La seule différence est dans la manière dont les tests sont appelés. Dans une application de Pages Razor, des tests de points de terminaison de la page sont généralement nommés d’après la classe de modèle de page (par exemple, `IndexPageTests` pour tester l’intégration du composant de la page d’Index). Dans une application MVC, les tests sont généralement organisés par les classes de contrôleur et nommées d’après les contrôleurs de test qu’ils (par exemple, `HomeControllerTests` pour tester l’intégration du composant pour le contrôleur Home).

## <a name="test-app-prerequisites"></a>Conditions préalables requises de l’application de test

Le projet de test doit :

* Référence de package pour [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/).
* Utilisez le Kit de développement Web dans le fichier projet (`<Project Sdk="Microsoft.NET.Sdk.Web">`).

Ces prerequesities peuvent être consultés dans le [exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/). Inspecter le *tests/RazorPagesProject.Tests/RazorPagesProject.Tests.csproj* fichier.

## <a name="basic-tests-with-the-default-webapplicationfactory"></a>Tests de base avec la valeur par défaut WebApplicationFactory

[WebApplicationFactory&lt;TEntryPoint&gt; ](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) est utilisé pour créer un [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) pour les tests d’intégration. `TEntryPoint` est la classe de point d’entrée de la SUT, généralement la `Startup` classe.

Implémenter des classes de test un *fixture de classe* interface (`IClassFixture`) pour indiquer que la classe contient des tests et fournit des instances de l’objet partagé entre les tests dans la classe.

### <a name="basic-test-of-app-endpoints"></a>Test de base de points de terminaison

Le test suivant (classe), `BasicTests`, utilise le `WebApplicationFactory` pour amorcer le SUT et fournir une [HttpClient](/dotnet/api/system.net.http.httpclient) à une méthode de test, `Get_EndpointsReturnSuccessAndCorrectContentType`. La méthode vérifie si le code d’état de réponse a réussi (codes d’état dans la plage 200-299) et le `Content-Type` en-tête est `text/html; charset=utf-8` pour plusieurs pages d’application.

[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) crée une instance de `HttpClient` qui suit les redirections et gère les cookies automatiquement.

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

### <a name="test-a-secure-endpoint"></a>Test d’un point de terminaison sécurisé

Un autre test dans la `BasicTests` classe vérifie qu’un point de terminaison sécurisé redirige un utilisateur non authentifié vers la page de connexion de l’application.

Dans le SUT, le `/SecurePage` page utilise un [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention pour appliquer une [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) à la page. Pour plus d’informations, consultez [conventions d’autorisation de Pages Razor](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

Dans le `Get_SecurePageRequiresAnAuthenticatedUser` de test, un [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) est configuré pour interdire les redirections en définissant [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) à `false`:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet2)]

En interdisant le client pour suivre la redirection, les vérifications suivantes peuvent être effectuées :

* Le code d’état retourné par le SUT peut être vérifié par rapport à attendu [HttpStatusCode.Redirect](/dotnet/api/system.net.httpstatuscode) résultat, pas le code d’état final après la redirection vers la page de connexion, qu’il serait [HttpStatusCode.OK](/dotnet/api/system.net.httpstatuscode).
* Le `Location` valeur d’en-tête dans les en-têtes de réponse est vérifiée pour confirmer qu’elle démarre avec `http://localhost/Identity/Account/Login`, pas la dernière page réponse de connexion, où le `Location` en-tête ne pourraient être présent.

Pour plus d’informations sur `WebApplicationFactoryClientOptions`, consultez la [options du Client](#client-options) section.

## <a name="customize-webapplicationfactory"></a>Personnaliser WebApplicationFactory

Configuration de l’hôte Web permettre être créée indépendamment les classes de test en héritant de `WebApplicationFactory` pour créer un ou plusieurs des fabrications personnalisées :

1. Hériter de `WebApplicationFactory` et remplacez [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost). Le [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) permet la configuration de la collection de service avec [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   Base de données dans l’amorçage le [exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) est effectuée par le `InitializeDbForTests` (méthode). La méthode est décrite dans le [exemple tests d’intégration : application organisation Test](#test-app-organization) section.

2. Utilisez personnalisé `CustomWebApplicationFactory` dans les classes de test. L’exemple suivant utilise la fabrique dans la `IndexPageTests` classe :

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   Client de l’application exemple est configuré pour empêcher le `HttpClient` de redirections suivantes. Comme expliqué dans la [tester un point de terminaison sécurisé](#test-a-secure-endpoint) section, cela permet à des tests pour vérifier le résultat de la première réponse de l’application. La première réponse est une redirection dans la plupart de ces tests avec un `Location` en-tête.

3. Un test classique utilise le `HttpClient` et les méthodes d’assistance pour traiter la demande et la réponse :

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

Toute demande POST à le SUT doit satisfaire la vérification est effectuée automatiquement par l’application de côté [côté système de protection des données](xref:security/data-protection/introduction). Afin de disposer de la demande de publication d’un test, l’application de test doit :

1. Effectuez une demande pour la page.
1. Analyser le cookie côté et un jeton de validation de demande à partir de la réponse.
1. Vérifiez la requête POST avec la validation de cookie et une demande côtée jeton en place.

Le `SendAsync` les méthodes d’extension d’assistance (*Helpers/HttpClientExtensions.cs*) et le `GetDocumentAsync` méthode d’assistance (*Helpers/HtmlHelpers.cs*) dans le [exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/) utiliser le [AngleSharp](https://anglesharp.github.io/) analyseur pour gérer le contrôle côté avec les méthodes suivantes :

* `GetDocumentAsync` &ndash; Reçoit le [HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) et retourne un `IHtmlDocument`. `GetDocumentAsync` utilise une fabrique qui prépare un *réponse virtuel* basé sur l’original `HttpResponseMessage`. Pour plus d’informations, consultez la [AngleSharp documentation](https://github.com/AngleSharp/AngleSharp#documentation).
* `SendAsync` méthodes d’extension pour le `HttpClient` composer une [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage) et appelez [SendAsync(HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) pour soumettre des demandes pour le SUT. Pour les surcharges `SendAsync` acceptez le formulaire HTML (`IHtmlFormElement`) et les éléments suivants :
  - Bouton de la forme envoyer (`IHtmlElement`)
  - Collection de valeurs de formulaire (`IEnumerable<KeyValuePair<string, string>>`)
  - Bouton Envoyer (`IHtmlElement`) et les valeurs de formulaire (`IEnumerable<KeyValuePair<string, string>>`)

> [!NOTE]
> [AngleSharp](https://anglesharp.github.io/) est une application tierce, l’analyse bibliothèque utilisé à des fins de démonstration dans cette rubrique et l’exemple d’application. AngleSharp n’est pas pris en charge ou requises pour les tests d’intégration d’applications ASP.NET Core. Autres analyseurs peuvent être utilisées, telle que la [Html agilité Pack (HAP)](http://html-agility-pack.net/). Une autre approche consiste à écrire du code pour gérer le jeton de vérification de demande et le cookie côté du système côté directement.

## <a name="customize-the-client-with-withwebhostbuilder"></a>Personnaliser le client avec WithWebHostBuilder

Lors de la configuration supplémentaire est nécessaire au sein d’une méthode de test, [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) crée un nouveau `WebApplicationFactory` avec un [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) qui est personnalisé en configuration.

Le `Post_DeleteMessageHandler_ReturnsRedirectToRoot` tester la méthode de la [exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) illustre l’utilisation de `WithWebHostBuilder`. Ce test exécute une suppression de l’enregistrement dans la base de données en déclenchant l’envoi d’un formulaire dans le SUT.

Car un autre test dans le `IndexPageTests` classe effectue une opération qui supprime tous les enregistrements dans la base de données et peut-être s’exécuter avant le `Post_DeleteMessageHandler_ReturnsRedirectToRoot` (méthode), la base de données est amorcée dans cette méthode de test pour vous assurer qu’un enregistrement est présent pour la SUT à supprimer. En sélectionnant le `deleteBtn1` bouton de la `messages` formulaire dans le SUT est simulé dans la demande pour le SUT :

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a>Options du client

Le tableau suivant indique la valeur par défaut [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) disponibles lors de la création `HttpClient` instances.

| Option | Description | Par défaut |
| ------ | ----------- | ------- |
| [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | Obtient ou définit si `HttpClient` instances doivent suivre automatiquement les réponses de redirection. | `true` |
| [BaseAddress](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | Obtient ou définit l’adresse de base de `HttpClient` instances. | `http://localhost` |
| [HandleCookies](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | Obtient ou définit si `HttpClient` instances doivent gérer les cookies. | `true` |
| [MaxAutomaticRedirections](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | Obtient ou définit le nombre maximal de réponses de redirection qui `HttpClient` les instances doivent respecter. | 7 |

Créer le `WebApplicationFactoryClientOptions` de classe et de passer à la [CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) (méthode) (valeur par défaut les valeurs sont affichés dans l’exemple de code) :

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a>Comment l’infrastructure de test déduit le chemin d’accès de la racine du contenu app

Le `WebApplicationFactory` constructeur déduit le chemin d’accès de la racine du contenu app en recherchant un [WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) sur l’assembly contenant les tests d’intégration avec une clé égale à la `TEntryPoint` assembly `System.Reflection.Assembly.FullName`. Si un attribut avec la clé correcte n’est pas trouvé, `WebApplicationFactory` revient à la recherche d’un fichier de solution (*\*.sln*) et ajoute le `TEntryPoint` nom de l’assembly dans le répertoire de solution. Le répertoire racine des applications (le chemin d’accès racine du contenu) permet de détecter les vues et les fichiers de contenu.

Dans la plupart des cas, il n’est pas nécessaire de définir explicitement la racine de contenu de l’application, car la logique de recherche recherche généralement la racine de contenu appropriée lors de l’exécution. Dans les scénarios spécifiques où la racine de contenu n’est pas trouvée à l’aide de l’algorithme de recherche intégrées, l’application de contenu racine peut être spécifiée explicitement, ou à l’aide d’une logique personnalisée. Pour définir la racine de l’application contenu dans ces scénarios, appelez le `UseSolutionRelativeContentRoot` méthode d’extension à partir de la [Microsoft.AspNetCore.TestHost](https://www.nuget.org/packages/Microsoft.AspNetCore.TestHost) package. Fournissez le chemin d’accès relatif de la solution et le modèle de nom ou glob de fichier solution facultatif (par défaut = `*.sln`).

Appelez le [UseSolutionRelativeContentRoot](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.usesolutionrelativecontentroot) méthode d’extension à l’aide *un* des approches suivantes :

* Lors de la configuration des classes de test avec `WebApplicationFactory`, fournir une configuration personnalisée avec le [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder):

   ```csharp
   public IndexPageTests(
       WebApplicationFactory<RazorPagesProject.Startup> factory)
   {
       var _factory = factory.WithWebHostBuilder(builder =>
       {
           builder.UseSolutionRelativeContentRoot("<SOLUTION-RELATIVE-PATH>");

           ...
       });
   }
   ```

* Lorsque vous configurez des classes de test personnalisé `WebApplicationFactory`, héritent de `WebApplicationFactory` et remplacez [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost):

   ```csharp
   public class CustomWebApplicationFactory<TStartup>
       : WebApplicationFactory<RazorPagesProject.Startup>
   {
       protected override void ConfigureWebHost(IWebHostBuilder builder)
       {
           builder.ConfigureServices(services =>
           {
               builder.UseSolutionRelativeContentRoot("<SOLUTION-RELATIVE-PATH>");

               ...
           });
       }
   }
   ```

## <a name="disable-shadow-copying"></a>Désactivez les clichés instantanés

Clichés instantanés entraîne les tests à exécuter dans un dossier autre que le dossier de sortie. Pour les tests fonctionne correctement, les clichés doivent être désactivée. Le [exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) utilise xUnit et désactive les clichés instantanés pour xUnit en incluant un *xunit.runner.json* fichier avec le paramètre de configuration approprié. Pour plus d’informations, consultez [configuration xUnit.net avec JSON](https://xunit.github.io/docs/configuring-with-json.html).

Ajouter le *xunit.runner.json* fichier à la racine du projet de test avec le contenu suivant :

```json
{
  "shadowCopy": false
}
```

## <a name="integration-tests-sample"></a>Exemple de tests d’intégration

Le [exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) est composé de deux applications :

| Application | Dossier du projet | Description |
| --- | -------------- | ----------- |
| Application de message (le SUT) | *src/RazorPagesProject* | Permet à un utilisateur à ajouter, supprimer une, supprimez tous les et analyser les messages. |
| Application de test | *tests/RazorPagesProject.Tests* | Utilisé pour le test d’intégration la SUT. |

Les tests peuvent être exécutés à l’aide des fonctionnalités intégrées de test d’un IDE, tel que [Visual Studio](https://www.visualstudio.com/vs/). Si vous utilisez [Visual Studio Code](https://code.visualstudio.com/) ou la ligne de commande, exécutez la commande suivante à une invite de commandes dans le *tests/RazorPagesProject.Tests* dossier :

```console
dotnet test
```

### <a name="message-app-sut-organization"></a>Organisation de l’application (SUT) de message

Le SUT est un système de messages de Pages Razor avec les caractéristiques suivantes :

* La page d’Index de l’application (*Pages/Index.cshtml* et *Pages/Index.cshtml.cs*) fournit une interface utilisateur et la page des méthodes de modèle de contrôle de l’ajout, suppression et analyse des messages (mots moyenne par message) .
* Un message est décrite par la `Message` classe (*Data/Message.cs*) avec deux propriétés : `Id` (clé) et `Text` (message). Le `Text` propriété est obligatoire et est limitée à 200 caractères.
* Les messages sont stockés à l’aide de [base de données en mémoire d’Entity Framework](/ef/core/providers/in-memory/)&#8224;.
* L’application contient une couche d’accès aux données (DAL) dans sa classe de contexte de base de données, `AppDbContext` (*Data/AppDbContext.cs*).
* Si la base de données est vide au démarrage de l’application, la banque de messages est initialisée avec trois messages.
* L’application inclut un `/SecurePage` qui n’est accessible par un utilisateur authentifié.

&#8224;La rubrique EF [Test avec InMemory](/ef/core/miscellaneous/testing/in-memory), explique comment utiliser une base de données en mémoire pour les tests avec MSTest. Cette rubrique utilise le [xUnit](https://xunit.github.io/) infrastructure de test. Concepts de test et les implémentations de test sur des infrastructures de tests différents sont similaires mais non identiques.

Bien que l’application n’utilise pas le [modèle de référentiel](http://martinfowler.com/eaaCatalog/repository.html) et n’est pas un exemple effectif de la [modèle d’unité de travail (UoW)](https://martinfowler.com/eaaCatalog/unitOfWork.html), Pages Razor prend en charge de ces modèles de développement. Pour plus d’informations, consultez [conception de la couche de persistance infrastructure](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design), [mise en œuvre du référentiel et les modèles d’unité de travail dans une Application ASP.NET MVC](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application), et [contrôleur de Test logique](/aspnet/core/mvc/controllers/testing) (l’exemple implémente le modèle de référentiel).

### <a name="test-app-organization"></a>Organisation de l’application de test

L’application de test est une application console à l’intérieur de la *tests/RazorPagesProject.Tests* dossier.

| Dossier d’application de test | Description |
| --------------- | ----------- |
| *BasicTests* | *BasicTests.cs* contient des méthodes de test pour le routage, l’accès à une page sécurisée par un utilisateur authentifié et obtenir un profil d’utilisateur GitHub et la vérification de la connexion du profil de l’utilisateur. |
| *IntegrationTests* | *IndexPageTests.cs* contient les tests d’intégration pour la page d’Index à l’aide personnalisée `WebApplicationFactory` classe. |
| *Programmes d’assistance et utilitaires* | <ul><li>*Utilities.cs* contient la `InitializeDbForTests` méthode utilisée pour amorcer la base de données de test.</li><li>*HtmlHelpers.cs* fournit une méthode pour retourner un AngleSharp `IHtmlDocument` pour une utilisation par les méthodes de test.</li><li>*HttpClientExtensions.cs* fournissent des surcharges pour `SendAsync` pour soumettre des demandes pour le SUT.</li></ul> |

L’infrastructure de test est [xUnit](https://xunit.github.io/). Tests d’intégration sont effectuées à l’aide de la [Microsoft.AspNetCore.TestHost](/dotnet/api/microsoft.aspnetcore.testhost), qui inclut le [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver). Étant donné que la [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) package est utilisé pour configurer le serveur hôte et de test de test, le `TestHost` et `TestServer` packages ne nécessitent pas les références de package directe dans le fichier projet de l’application de test ou configuration de développeur dans l’application de test.

**L’amorçage de la base de données de test**

Tests d’intégration nécessitent généralement un petit jeu de données dans la base de données avant l’exécution du test. Par exemple, une suppression tester les appels pour une suppression de l’enregistrement de base de données, la base de données doit avoir au moins un enregistrement pour la demande de suppression réussisse.

L’exemple d’application basée sur la base de données avec trois messages dans *Utilities.cs* que les tests peuvent utiliser lorsqu’ils exécutent :

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

## <a name="additional-resources"></a>Ressources supplémentaires

* [Tests unitaires](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Tests unitaires de Pages Razor](xref:test/razor-pages-tests)
* [Intergiciel (middleware)](xref:fundamentals/middleware/index)
* [Contrôleurs de test](xref:mvc/controllers/testing)
