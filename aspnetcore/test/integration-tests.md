---
title: Tests d’intégration dans ASP.NET Core
author: guardrex
description: Découvrez comment les tests d’intégration garantissent le bon fonctionnement des composants d’une application au niveau de l’infrastructure, notamment la base de données, le système de fichiers et le réseau.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/30/2018
uid: test/integration-tests
ms.openlocfilehash: 15186383497f8dd9a9766f8fe7cf32f70a121c8e
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477590"
---
# <a name="integration-tests-in-aspnet-core"></a>Tests d’intégration dans ASP.NET Core

Par [Luke Latham](https://github.com/guardrex) et [Steve Smith](https://ardalis.com/)

Tests d’intégration vous assurer que les composants d’une application fonctionnent correctement à un niveau qui comprend l’infrastructure de prise en charge de l’application, telles que la base de données, le système de fichiers et le réseau. ASP.NET Core prend en charge les tests d’intégration à l’aide d’une infrastructure de tests unitaires avec un hôte web de test et un serveur de test en mémoire.

Cette rubrique suppose une compréhension élémentaire des tests unitaires. Si êtes pas familiarisé avec les concepts des tests, consultez le [Unit Testing in .NET Core et .NET Standard](/dotnet/core/testing/) rubrique et son contenu lié.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

L’exemple d’application est une application Pages Razor et suppose une compréhension élémentaire des Pages Razor. Si êtes pas familiarisé avec les Pages Razor, consultez les rubriques suivantes :

* [Présentation des pages Razor](xref:razor-pages/index)
* [Bien démarrer avec les pages Razor](xref:tutorials/razor-pages/razor-pages-start)
* [Tests unitaires Pages Razor](xref:test/razor-pages-tests)

> [!NOTE]
> Pour tester les applications SPA, nous recommandons un outil tel que [Selenium](https://www.seleniumhq.org/), ce qui permet d’automatiser un navigateur.

## <a name="introduction-to-integration-tests"></a>Introduction aux tests d’intégration

Composants d’une application sur un niveau supérieur à évaluent les tests d’intégration [tests unitaires](/dotnet/core/testing/). Tests unitaires sont utilisés pour tester les composants logiciels isolé, telles que les méthodes de classe individuels. Tests d’intégration confirmer que deux ou plusieurs composants d’application fonctionnent ensemble pour produire un résultat attendu, éventuellement, y compris tous les composants requis pour traiter complètement une demande.

Ces tests plus larges sont utilisés pour tester l’application infrastructure et framework entier, parmi lesquels figurent souvent les composants suivants :

* Base de données
* Système de fichiers
* Appliances réseau
* Pipeline de requête-réponse

Utilisez fabriqué composants, appelés des tests unitaires *fakes* ou *objets fictifs*, à la place des composants d’infrastructure.

Contrairement aux tests unitaires, tests d’intégration :

* Utiliser les composants réelles utilisée par l’application en production.
* Nécessitent plus de code et de traitement des données.
* Prendre plus de temps.

Par conséquent, limitez l’utilisation de tests d’intégration pour les scénarios d’infrastructure plus importantes. Si un comportement peut être testé à l’aide d’un test unitaire ou un test d’intégration, sélectionnez le test unitaire.

> [!TIP]
> N’écrivez pas les tests d’intégration pour chaque permutation possibles d’accès de fichiers et de données avec les bases de données et les systèmes de fichiers. Quel que soit le nombre de positions entre une application interagir avec les bases de données et les systèmes de fichiers, un ensemble concentré de lecture, écriture, update et delete intégration tests sont généralement capables de base de données de test de manière adéquate et composants du système de fichiers. Utilisez les tests unitaires pour les tests de logique de la méthode de routine qui interagissent avec ces composants. Dans les tests unitaires, l’utilisation de l’infrastructure substituts/simulacres résultat accélère l’exécution de test.

> [!NOTE]
> Dans les discussions des tests d’intégration, le projet testé est fréquemment appelé le *système testé*, ou « ST » pour faire plus court.

## <a name="aspnet-core-integration-tests"></a>Tests d’intégration ASP.NET Core

Tests d’intégration dans ASP.NET Core requièrent les éléments suivants :

* Un projet de test est utilisé pour contenir et exécuter les tests. Le projet de test a une référence au projet ASP.NET Core testé, appelé le *système testé* (ST). _« ST » est utilisé tout au long de cette rubrique pour faire référence à l’application testée._
* Le projet de test crée un hôte web de test pour le st et utilise un client de serveur de test pour gérer les demandes et réponses sur le st.
* Un test runner est utilisé pour exécuter les tests et les rapports les résultats des tests.

Tests d’intégration suivent une séquence d’événements qui incluent habituelles *organiser*, *Act*, et *Assert* étapes de test :

1. Hôte de web du St est configuré.
1. Un client de serveur de test est créé pour envoyer des demandes à l’application.
1. Le *organiser* étape de test est exécutée : l’application de test prépare une demande.
1. Le *Act* étape de test est exécuté : le client envoie la demande et reçoit la réponse.
1. Le *Assert* étape de test est exécuté : le *réel* réponse est validée comme un *passer* ou *échouer* selon un *attendu*  réponse.
1. Le processus se poursuit jusqu'à ce que tous les tests sont exécutés.
1. Les résultats des tests sont signalés.

En règle générale, l’hôte du test web est configuré différemment que l’hôte web normaux de l’application pour le test s’exécute. Par exemple, une autre base de données ou des paramètres d’application différents peuvent être utilisés pour les tests.

Composants d’infrastructure, tels que l’hôte web de test et le serveur de test en mémoire ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)), sont fournies ou gérés par le [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) package. Utilisation de ce package simplifie la création de test et l’exécution.

Le `Microsoft.AspNetCore.Mvc.Testing` package gère les tâches suivantes :

* Copie le fichier de dépendances (*\*.deps*) du St dans le projet de test *bin* dossier.
* Définit la racine de contenu à la racine du projet du St afin que les fichiers statiques et les pages/affichages sont disponibles lorsque les tests sont exécutés.
* Fournit le [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) classe pour rationaliser l’amorçage le ST avec `TestServer`.

Le [tests unitaires](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) documentation explique comment configurer un projet et test testeur, ainsi que des instructions détaillées sur l’exécution des tests et des recommandations sur la manière de tests de nom et classes de test.

> [!NOTE]
> Lorsque vous créez un projet de test pour une application, séparez les tests unitaires dans les tests d’intégration dans des projets différents. Cela permet de garantir que les composants d’infrastructure test ne sont pas accidentellement inclus dans les tests unitaires. Séparation des tests unitaires et d’intégration permet également à contrôler sur quel jeu de tests sont exécutés.

Il n’existe pratiquement aucune différence entre la configuration pour les tests d’applications de Pages Razor et les applications MVC. La seule différence est dans la manière dont les tests sont appelés. Dans une application Pages Razor, les tests de points de terminaison de page sont généralement nommés après la classe de modèle de page (par exemple, `IndexPageTests` pour tester l’intégration du composant pour la page d’Index). Dans une application MVC, les tests sont généralement organisées par les classes de contrôleur et nommées d’après les contrôleurs ils testent (par exemple, `HomeControllerTests` pour tester l’intégration du composant pour le contrôleur Home).

## <a name="test-app-prerequisites"></a>Tester les conditions préalables requises de l’application

Le projet de test doit :

* Référencer les packages suivants :
  - [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)
  - [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/)
* Spécifiez le Kit de développement Web dans le fichier projet (`<Project Sdk="Microsoft.NET.Sdk.Web">`). Le Kit de développement Web est requis lorsque vous référencez le [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app).

Ces conditions préalables sont consultables dans la [exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/). Inspecter le *tests/RazorPagesProject.Tests/RazorPagesProject.Tests.csproj* fichier. L’exemple d’application utilise le [xUnit](https://xunit.github.io/) infrastructure de test et le [AngleSharp](https://anglesharp.github.io/) bibliothèque de l’analyseur, donc fait également référence à l’exemple d’application :

* [xUnit](https://www.nuget.org/packages/xunit/)
* [xUnit.Runner.VisualStudio](https://www.nuget.org/packages/xunit.runner.visualstudio/)
* [AngleSharp](https://www.nuget.org/packages/AngleSharp/)

## <a name="basic-tests-with-the-default-webapplicationfactory"></a>Tests de base avec la valeur par défaut WebApplicationFactory

[WebApplicationFactory&lt;TEntryPoint&gt; ](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) est utilisé pour créer un [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) pour les tests d’intégration. `TEntryPoint` est la classe de point d’entrée du St, généralement la `Startup` classe.

Classes de test implémentent un *fixture de classe* interface (`IClassFixture`) pour indiquer la classe contient des tests et fournir des instances d’objet partagé entre les tests dans la classe.

### <a name="basic-test-of-app-endpoints"></a>Test de base des points de terminaison d’application

Classe, de test de ce qui suit `BasicTests`, utilise le `WebApplicationFactory` pour amorcer le st et fournir un [HttpClient](/dotnet/api/system.net.http.httpclient) à une méthode de test, `Get_EndpointsReturnSuccessAndCorrectContentType`. La méthode vérifie si le code d’état de réponse a réussi (codes d’état dans la plage 200-299) et le `Content-Type` en-tête est `text/html; charset=utf-8` pour plusieurs pages d’application.

[Createclient utile](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) crée une instance de `HttpClient` qui suit les redirections et gère les cookies automatiquement.

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

### <a name="test-a-secure-endpoint"></a>Tester un point de terminaison sécurisé

Un autre test dans le `BasicTests` classe vérifie qu’un point de terminaison sécurisé redirige un utilisateur non authentifié à la page de connexion de l’application.

Dans le st, la `/SecurePage` page utilise un [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention qui permet d’appliquer un [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) à la page. Pour plus d’informations, consultez [conventions d’autorisation des Pages Razor](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

Dans le `Get_SecurePageRequiresAnAuthenticatedUser` tester, un [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) est configuré pour interdire les redirections en définissant [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) à `false`:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet2)]

En interdisant le client pour suivre la redirection, vous peuvent effectuer les vérifications suivantes :

* Le code d’état retourné par le st peut être vérifié par rapport à attendu [HttpStatusCode.Redirect](/dotnet/api/system.net.httpstatuscode) résultat, pas le code d’état final après la redirection vers la page de connexion, ce qui serait [HttpStatusCode.OK](/dotnet/api/system.net.httpstatuscode).
* Le `Location` valeur d’en-tête dans les en-têtes de réponse est activée pour confirmer qu’il démarre avec `http://localhost/Identity/Account/Login`, pas la dernière page réponse de connexion, où le `Location` en-tête n’aurait pas être présent.

Pour plus d’informations sur `WebApplicationFactoryClientOptions`, consultez le [options du Client](#client-options) section.

## <a name="customize-webapplicationfactory"></a>Personnaliser WebApplicationFactory

Configuration de l’hôte Web peut être créée indépendamment les classes de test en héritant de `WebApplicationFactory` pour créer un ou plusieurs des fabrications personnalisées :

1. Hériter `WebApplicationFactory` et remplacer [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost). Le [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) permet la configuration de la collection de service avec [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   Base de données d’amorçage dans le [exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) est effectuée par le `InitializeDbForTests` (méthode). La méthode est décrite dans le [exemple de tests d’intégration : organisation de l’application Test](#test-app-organization) section.

2. Utilisez personnalisé `CustomWebApplicationFactory` dans les classes de test. L’exemple suivant utilise la fabrique dans la `IndexPageTests` classe :

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   Client de l’application exemple est configuré pour empêcher le `HttpClient` à partir de redirections suivantes. Comme expliqué dans la [tester un point de terminaison sécurisé](#test-a-secure-endpoint) section, cela permet des tests pour vérifier le résultat de la première réponse de l’application. La première réponse est une redirection dans la plupart de ces tests avec un `Location` en-tête.

3. Un test classique utilise la `HttpClient` et méthodes d’assistance pour traiter la demande et la réponse :

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

Toute demande POST sur le st doit satisfaire la vérification anti-contrefaçon est effectuée automatiquement par l’application [système anti-contrefaçon de protection de données](xref:security/data-protection/introduction). Afin de disposer de demande POST d’un test, l’application de test doit :

1. Effectuer une demande pour la page.
1. Analyser le cookie anti-contrefaçon et le jeton de validation de demande à partir de la réponse.
1. Effectuer la demande POST avec la validation de demande et le cookie anti-contrefaçon jeton en place.

Le `SendAsync` méthodes d’extension d’assistance (*Helpers/HttpClientExtensions.cs*) et le `GetDocumentAsync` méthode d’assistance (*Helpers/HtmlHelpers.cs*) dans le [exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/) utiliser le [AngleSharp](https://anglesharp.github.io/) analyseur pour gérer le contrôle anti-contrefaçon avec les méthodes suivantes :

* `GetDocumentAsync` &ndash; Reçoit le [HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) et retourne un `IHtmlDocument`. `GetDocumentAsync` utilise une fabrique qui prépare une *réponse virtuel* selon la version d’origine `HttpResponseMessage`. Pour plus d’informations, consultez le [AngleSharp documentation](https://github.com/AngleSharp/AngleSharp#documentation).
* `SendAsync` méthodes d’extension pour le `HttpClient` composer un [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage) et appelez [SendAsync(HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) pour soumettre des demandes sur le st. Surcharges pour `SendAsync` acceptez le formulaire HTML (`IHtmlFormElement`) et les éléments suivants :
  - Bouton du formulaire d’envoi (`IHtmlElement`)
  - Collection des valeurs de formulaire (`IEnumerable<KeyValuePair<string, string>>`)
  - Bouton d’envoi (`IHtmlElement`) et les valeurs de formulaire (`IEnumerable<KeyValuePair<string, string>>`)

> [!NOTE]
> [AngleSharp](https://anglesharp.github.io/) est un tiers, l’analyse bibliothèque utilisé à des fins de démonstration dans cette rubrique et l’exemple d’application. AngleSharp n’est pas pris en charge ou requises pour les tests d’intégration des applications ASP.NET Core. Autres analyseurs peuvent être utilisées, telle que la [Html agilité Pack (HAP)](http://html-agility-pack.net/). Une autre approche consiste à écrire du code pour gérer la demande de jeton de vérification et le cookie anti-contrefaçon le système anti-contrefaçon directement.

## <a name="customize-the-client-with-withwebhostbuilder"></a>Personnalisation du client avec WithWebHostBuilder

Lors de la configuration supplémentaire est nécessaire au sein d’une méthode de test, [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) crée un `WebApplicationFactory` avec un [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) qui est personnalisé en configuration.

Le `Post_DeleteMessageHandler_ReturnsRedirectToRoot` tester la méthode de la [exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) illustre l’utilisation de `WithWebHostBuilder`. Ce test exécute une suppression de l’enregistrement dans la base de données en déclenchant l’envoi d’un formulaire dans le st.

Car un autre test dans le `IndexPageTests` classe effectue une opération qui supprime tous les enregistrements dans la base de données et peut-être s’exécuter avant le `Post_DeleteMessageHandler_ReturnsRedirectToRoot` (méthode), la base de données est amorcée dans cette méthode de test pour vous assurer qu’un enregistrement est présent pour le st à supprimer. En sélectionnant le `deleteBtn1` bouton de la `messages` formulaire dans le st est simulée à la demande pour le st :

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a>Options du client

Le tableau suivant présente la valeur par défaut [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) disponibles lors de la création `HttpClient` instances.

| Option | Description | Par défaut |
| ------ | ----------- | ------- |
| [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | Obtient ou définit si `HttpClient` instances doivent suivre automatiquement les réponses de redirection. | `true` |
| [BaseAddress](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | Obtient ou définit l’adresse de base de `HttpClient` instances. | `http://localhost` |
| [HandleCookies](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | Obtient ou définit si `HttpClient` instances doivent gérer les cookies. | `true` |
| [MaxAutomaticRedirections](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | Obtient ou définit le nombre maximal de réponses de redirection qui `HttpClient` instances doivent suivre. | 7 |

Créer le `WebApplicationFactoryClientOptions` classe et transmettez-le à la [createclient utile](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) (méthode) (par défaut de valeurs sont affichées dans l’exemple de code) :

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="inject-mock-services"></a>Injecter des services fictifs

Services peuvent être substituées dans un test avec un appel à [ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) sur le Générateur de l’hôte. **Pour injecter des services fictifs, le st doit avoir un `Startup` classe avec un `Startup.ConfigureServices` (méthode).**

L’exemple st inclut un service délimité qui renvoie un devis. Le devis est incorporé dans un champ masqué sur la page d’Index lors de la page d’Index est demandée.

*Services/IQuoteService.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/IQuoteService.cs?name=snippet1)]

*Services/QuoteService.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/QuoteService.cs?name=snippet1)]

*Startup.cs* :

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet2)]

*Pages/Index.cshtml.cs* :

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml.cs?name=snippet1&highlight=4,9,20,26)]

*Pages/Index.cs*:

[!code-cshtml[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml?name=snippet_Quote)]

Le balisage suivant est généré lors de l’exécution de l’application st :

```html
<input id="quote" type="hidden" value="Come on, Sarah. We&#x27;ve an appointment in 
    London, and we&#x27;re already 30,000 years late.">
```

Pour tester l’injection de service et le guillemet dans un test d’intégration, un service factice est injecté dans le st par le test. Le service factice remplace l’application `QuoteService` avec un service fourni par l’application de test, appelé `TestQuoteService`:

*IntegrationTests.IndexPageTests.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet4)]

`ConfigureTestServices` est appelée, et le service délimité est enregistré :

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet5&highlight=7-10,17,20-21)]

Le balisage généré pendant l’exécution du test reflète le texte de devis fourni par `TestQuoteService`, par conséquent, l’assertion réussit :

```html
<input id="quote" type="hidden" value="Something&#x27;s interfering with time, 
    Mr. Scarman, and time is my business.">
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a>Comment l’infrastructure de test déduit le chemin d’accès de la racine de contenu application

Le `WebApplicationFactory` constructeur déduit le chemin d’accès de la racine de contenu application en recherchant un [WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) sur l’assembly contenant les tests d’intégration avec une clé égale à la `TEntryPoint` assembly `System.Reflection.Assembly.FullName`. Dans le cas où un attribut avec la clé correcte n’est pas trouvé, `WebApplicationFactory` revient à la recherche d’un fichier solution (*\*.sln*) et ajoute le `TEntryPoint` nom de l’assembly dans le répertoire de solution. Le répertoire racine d’application (le chemin d’accès racine de contenu) permet de détecter des vues et des fichiers de contenu.

Dans la plupart des cas, il n’est pas nécessaire de définir explicitement la racine de contenu de l’application, car la logique de recherche recherche généralement la racine de contenu appropriée lors de l’exécution. Dans des scénarios particuliers où la racine de contenu n’est pas trouvée à l’aide de l’algorithme de recherche intégrées, l’application de contenu racine peut être spécifiée explicitement, ou à l’aide d’une logique personnalisée. Pour définir la racine de contenu d’application dans ces scénarios, appelez le `UseSolutionRelativeContentRoot` méthode d’extension à partir de la [Microsoft.AspNetCore.TestHost](https://www.nuget.org/packages/Microsoft.AspNetCore.TestHost) package. Fournissez le chemin d’accès relatif de la solution et le modèle de nom ou glob du fichier solution facultatif (par défaut = `*.sln`).

Appelez le [UseSolutionRelativeContentRoot](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.usesolutionrelativecontentroot) extension à l’aide de méthode *un* des approches suivantes :

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

* Lors de la configuration des classes de test avec un personnalisé `WebApplicationFactory`, héritent `WebApplicationFactory` et remplacer [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost):

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

## <a name="disable-shadow-copying"></a>Désactiver les clichés instantanés

Clichés instantanés entraîne les tests à exécuter dans un dossier autre que le dossier de sortie. Pour les tests fonctionne correctement, les copies fantômes doivent être désactivée. Le [exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) utilise xUnit et désactive les clichés instantanés pour xUnit en incluant un *xunit.runner.json* fichier avec le paramètre de configuration est correcte. Pour plus d’informations, consultez [configuration xUnit.net avec JSON](https://xunit.github.io/docs/configuring-with-json.html).

Ajouter le *xunit.runner.json* fichier à la racine du projet de test avec le contenu suivant :

```json
{
  "shadowCopy": false
}
```

## <a name="integration-tests-sample"></a>Exemple de tests d’intégration

Le [exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) se compose de deux applications :

| Application | Dossier du projet | Description |
| --- | -------------- | ----------- |
| Application de message (le st) | *src/RazorPagesProject* | Autorise un utilisateur à ajouter, supprimer une, supprimer toutes et analyser les messages. |
| Application de test | *tests/RazorPagesProject.Tests* | Utilisé pour le test d’intégration le st. |

Les tests peuvent être exécutés en utilisant les fonctionnalités de test intégré d’un IDE, comme [Visual Studio](https://www.visualstudio.com/vs/). Si vous utilisez [Visual Studio Code](https://code.visualstudio.com/) ou la ligne de commande, exécutez la commande suivante à une invite de commandes dans le *tests/RazorPagesProject.Tests* dossier :

```console
dotnet test
```

### <a name="message-app-sut-organization"></a>Organisation de l’application (ST) de message

Le st est un système de message de Pages Razor avec les caractéristiques suivantes :

* La page d’Index de l’application (*pages/index.cshtml* et *Pages/Index.cshtml.cs*) fournit une interface utilisateur et la page des méthodes de modèle de contrôle de l’ajout, suppression et analyse des messages (mots moyenne par message) .
* Un message est décrite par le `Message` classe (*Data/Message.cs*) avec deux propriétés : `Id` (clé) et `Text` (message). Le `Text` propriété est requise et limitée à 200 caractères.
* Les messages sont stockés à l’aide de [base de données en mémoire d’Entity Framework](/ef/core/providers/in-memory/)&#8224;.
* L’application contient une couche d’accès aux données (DAL) dans sa classe de contexte de base de données, `AppDbContext` (*Data/AppDbContext.cs*).
* Si la base de données est vide sur le démarrage de l’application, la banque de messages est initialisée avec trois messages.
* L’application comprend un `/SecurePage` qui est accessible uniquement par un utilisateur authentifié.

&#8224;La rubrique EF, [Test avec InMemory](/ef/core/miscellaneous/testing/in-memory), explique comment utiliser une base de données en mémoire pour les tests avec MSTest. Cette rubrique utilise le [xUnit](https://xunit.github.io/) infrastructure de test. Concepts des tests et des implémentations de test entre les frameworks de test différentes sont similaires mais pas identiques.

Bien que l’application n’utilise pas le modèle de référentiel et n’est pas un exemple effectif de la [modèle unité de travail (UoW)](https://martinfowler.com/eaaCatalog/unitOfWork.html), Pages Razor prend en charge ces modèles de développement. Pour plus d’informations, consultez [conception de la couche de persistance d’infrastructure](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) et [logique du contrôleur de Test](/aspnet/core/mvc/controllers/testing) (l’exemple implémente le modèle de référentiel).

### <a name="test-app-organization"></a>Organisation de l’application de test

L’application de test est une application console à l’intérieur de la *tests/RazorPagesProject.Tests* dossier.

| Dossier d’application de test | Description |
| --------------- | ----------- |
| *BasicTests* | *BasicTests.cs* contient des méthodes de test pour le routage, l’accès à une page sécurisée par un utilisateur non authentifié et obtenir un profil d’utilisateur GitHub et vérification de la connexion de l’utilisateur du profil. |
| *IntegrationTests* | *IndexPageTests.cs* contient les tests d’intégration pour la page d’Index à l’aide personnalisée `WebApplicationFactory` classe. |
| *Programmes d’assistance/utilitaires* | <ul><li>*Utilities.cs* contient le `InitializeDbForTests` méthode utilisée pour amorcer la base de données de test.</li><li>*HtmlHelpers.cs* fournit une méthode pour retourner un AngleSharp `IHtmlDocument` pour une utilisation par les méthodes de test.</li><li>*HttpClientExtensions.cs* fournissent des surcharges pour `SendAsync` pour soumettre des demandes sur le st.</li></ul> |

L’infrastructure de test est [xUnit](https://xunit.github.io/). Tests d’intégration sont effectués à l’aide de la [Microsoft.AspNetCore.TestHost](/dotnet/api/microsoft.aspnetcore.testhost), qui inclut le [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver). Étant donné que le [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) package est utilisé pour configurer le serveur hôte et de test de test, le `TestHost` et `TestServer` packages ne nécessitent pas de références de package directement dans le fichier projet de l’application de test ou configuration de développeur dans l’application de test.

**L’amorçage de la base de données de test**

Tests d’intégration nécessitent généralement un petit jeu de données dans la base de données avant l’exécution du test. Par exemple, une suppression tester des appels pour une suppression de l’enregistrement de base de données, la base de données doit avoir au moins un enregistrement pour la demande de suppression réussisse.

L’exemple d’application amorce la base de données avec trois messages dans *Utilities.cs* que les tests peuvent utiliser lorsqu’ils s’exécutent :

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

## <a name="additional-resources"></a>Ressources supplémentaires

* [Tests unitaires](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Tests unitaires Pages Razor](xref:test/razor-pages-tests)
* [Intergiciel (middleware)](xref:fundamentals/middleware/index)
* [Contrôleurs de test](xref:mvc/controllers/testing)
