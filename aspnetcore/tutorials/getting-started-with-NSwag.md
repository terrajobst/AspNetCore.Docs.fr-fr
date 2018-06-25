---
title: Bien démarrer avec NSwag et ASP.NET Core
author: zuckerthoben
description: Découvrez comment utiliser NSwag pour générer des pages d’aide et de documentation pour une API web ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 05/08/2018
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: f4cc9ec1f32ef2bd0056ba8d0cbbbe9228834d85
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279195"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a>Bien démarrer avec NSwag et ASP.NET Core

Par [Christoph Nienaber](https://twitter.com/zuckerthoben) et [Rico Suter](https://rsuter.com)

::: moniker range="<= aspnetcore-2.0"
[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))
::: moniker-end

L’utilisation de [NSwag](https://github.com/RSuter/NSwag) avec un intergiciel (middleware) ASP.NET Core nécessite le package NuGet [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/). Le package se compose d’un générateur Swagger, de l’IU Swagger (v2 et v3) et de [l’IU ReDoc](https://github.com/Rebilly/ReDoc).

Nous vous recommandons vivement d’utiliser les fonctionnalités de génération de code de NSwag. Choisissez l’une des options suivantes pour la génération de code :

* Utiliser [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), une application de bureau Windows pour générer du code client en C# et TypeScript pour votre API.
* Utiliser les packages NuGet [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) ou [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) pour générer du code dans votre projet.
* Utiliser NSwag à partir de la [ligne de commande](https://github.com/NSwag/NSwag/wiki/CommandLine).
* Utiliser le package NuGet [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild).

## <a name="features"></a>Fonctionnalités

La principale raison d’utiliser NSwag est non seulement de pouvoir utiliser l’IU Swagger et le générateur Swagger, mais aussi d’utiliser ses fonctionnalités de génération de code flexibles. Vous n’avez pas besoin d’une API existante, vous pouvez utiliser des API de tiers qui intègrent Swagger et laissent NSwag générer une implémentation du client. Dans les deux cas, le cycle de développement est rapide et vous pouvez vous adapter plus facilement aux changements d’API.

## <a name="package-installation"></a>Installation de package

Vous pouvez ajouter le package NuGet NSwag avec l’une des méthodes suivantes :

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* À partir de la fenêtre **Console du Gestionnaire de package** :
  * Accédez à **Affichage** > **Autres fenêtres** > **Console du Gestionnaire de package**.
  * Accédez au répertoire où se trouve le fichier *TodoApi.csproj*.
  * Exécutez la commande suivante :

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* À partir de la boîte de dialogue **Gérer les packages NuGet** :
  * Cliquez avec le bouton droit sur le projet dans **l’Explorateur de solutions** > **Gérer les packages NuGet**.
  * Affectez la valeur « nuget.org » à **Source du package**.
  * Entrez « NSwag.AspNetCore » dans la zone de recherche.
  * Sélectionnez le package « NSwag.AspNetCore » sous l’onglet **Parcourir** et cliquez sur **Installer**.

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

* Cliquez avec le bouton droit sur le dossier *Packages* dans **Panneau Solutions** > **Ajouter des packages**.
* Dans la fenêtre **Ajouter des packages**, sélectionnez « nuget.org » dans la liste déroulante **Source**.
* Entrez « NSwag.AspNetCore » dans la zone de recherche.
* Sélectionnez le package « NSwag.AspNetCore » dans le volet de résultats, puis cliquez sur **Ajouter un package**.

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Exécutez la commande suivante à partir du **Terminal intégré** :

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

Exécutez la commande suivante :

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a>Ajouter et configurer l’intergiciel (middleware) Swagger

Importez les espaces de noms suivants dans la classe `Startup` :

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_StartupConfigureImports)]

Dans la méthode `Startup.Configure`, activez l’intergiciel pour traiter la spécification Swagger générée et l’IU Swagger :

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-10)]

Lancez l’application. Accédez à `http://localhost:<port>/swagger` pour voir l’IU Swagger. Accédez à `http://localhost:<port>/swagger/v1/swagger.json` pour voir la spécification Swagger.

## <a name="code-generation"></a>Génération de code

### <a name="via-nswagstudio"></a>Via NSwagStudio

* Installez NSwagStudio à partir du [dépôt GitHub](https://github.com/RSuter/NSwag/wiki/NSwagStudio) officiel.
* Lancez NSwagStudio. Entrez l’URL du fichier *swagger.json* dans la zone de texte **Swagger Specification URL** (URL de spécification Swagger), puis cliquez sur le bouton **Create local Copy** (Créer une copie locale).
* Sélectionnez le type de sortie client **CSharp Client** (Client CSharp). Parmi les autres options, citons **TypeScript Client** (Client TypeScript) et **CSharp Web API Controller** (Contrôleur d’API web CSharp). Un contrôleur d’API web vous permet fondamentalement d’effectuer une génération inverse. Il utilise la spécification d’un service pour regénérer le service.
* Cliquez sur le bouton **Generate Outputs** (Générer des sorties). Une implémentation client C# complète du projet *TodoApi.NSwag* est produite. Cliquez sur l’onglet **CSharp Client** (Client CSharp) de la section **Outputs** (Sorties) pour afficher le code client généré :

```csharp
//----------------------
// <auto-generated>
//     Generated using the NSwag toolchain v11.17.3.0 (NJsonSchema v9.10.46.0 (Newtonsoft.Json v9.0.0.0)) (http://NSwag.org)
// </auto-generated>
//----------------------

namespace MyNamespace
{
    #pragma warning disable // Disable all warnings

    [System.CodeDom.Compiler.GeneratedCode("NSwag",
        "11.17.3.0 (NJsonSchema v9.10.46.0 (Newtonsoft.Json v9.0.0.0))")]
    public partial class TodoClient
    {
        private string _baseUrl = "http://localhost:50499";
        private System.Lazy<Newtonsoft.Json.JsonSerializerSettings> _settings;

        public TodoClient()
        {
            _settings = new System.Lazy
                <Newtonsoft.Json.JsonSerializerSettings>(() =>
            {
                var settings = new Newtonsoft.Json.JsonSerializerSettings();
                UpdateJsonSerializerSettings(settings);
                return settings;
            });
        }

        public string BaseUrl
        {
            get { return _baseUrl; }
            set { _baseUrl = value; }
        }

        // code omitted for brevity
```

> [!TIP]
> Le code client C# est généré en fonction des paramètres définis sous l’onglet **Settings** (Paramètres) de l’onglet **CSharp Client** (Client CSharp). Modifiez les paramètres pour effectuer des tâches telles que le renommage de l’espace de noms par défaut et la génération de méthodes synchrones.

* Copiez le code C# généré dans un fichier dans un projet client (par exemple, une application [Xamarin.Forms](/xamarin/xamarin-forms/)).
* Démarrez l’utilisation de l’API web :

```csharp
var todoClient = new TodoClient();

// Gets all to-dos from the API
var allTodos = await todoClient.GetAllAsync();

// Create a new TodoItem, and save it in the API
var createdTodo = await todoClient.CreateAsync(new TodoItem());

// Get a single to-do by ID
var foundTodo = await todoClient.GetByIdAsync(1);
```

> [!NOTE]
> Vous pouvez injecter une URL de base et/ou un client HTTP dans le client d’API. En général, il vaut mieux toujours [réutiliser HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).

### <a name="other-ways-to-generate-client-code"></a>Autres façons de générer du code client

Vous pouvez générer le code client d’autres manières, plus adaptées à votre flux de travail :

* [MSBuild](https://www.nuget.org/packages/NSwag.MSBuild/)

* [Dans le code](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [Modèles T4](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customize"></a>Personnaliser

Swagger fournit des options pour documenter le modèle objet pour faciliter l’utilisation de l’API web.

### <a name="api-info-and-description"></a>Informations sur l’API et description

Dans la méthode `Startup.Configure`, une action de configuration passée à la méthode `UseSwagger` ajoute des informations comme l’auteur, la licence et la description :

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_UseSwagger)]

L’IU Swagger affiche les informations de la version :

![Interface utilisateur de Swagger avec des informations de version](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a>commentaires XML

Vous pouvez activer les commentaires XML de l’une des façons suivantes :

# <a name="visual-studiotabvisual-studio-xml"></a>[Visual Studio](#tab/visual-studio-xml/)

* Cliquez avec le bouton droit sur le projet dans **l’Explorateur de solutions**, puis sélectionnez **Propriétés**.
* Cochez la case **Fichier de documentation XML** dans la section **Sortie** sous l’onglet **Générer**.

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[Visual Studio pour Mac](#tab/visual-studio-mac-xml/)

* Ouvrez la boîte de dialogue **Options du projet** > **Générer** > **Compilateur**.
* Cochez la case **Générer la documentation XML** dans la section **Options générales**.

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[Visual Studio Code](#tab/visual-studio-code-xml/)

Ajoutez manuellement l’extrait de code suivant au fichier *.csproj* :

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement)]

---

### <a name="data-annotations"></a>Annotations de données

::: moniker range="<= aspnetcore-2.0"
NSwag utilise la [réflexion](/dotnet/csharp/programming-guide/concepts/reflection), et le type de retour recommandé pour les actions d’API web est [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult). Par conséquent, NSwag ne peut pas déduire votre action et ce qu’elle retourne. Prenons l'exemple suivant :

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

L’action précédente retourne `IActionResult`, mais à l’intérieur de l’action, [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) ou [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) sont retournés. Les annotations de données sont utilisées pour indiquer aux clients les codes d’état HTTP que cette action est susceptible de retourner. Décorez l’action avec les attributs suivants :

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
NSwag utilise la [réflexion](/dotnet/csharp/programming-guide/concepts/reflection), et le type de retour recommandé pour les actions d’API web est [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1). NSwag peut donc uniquement déduire le type de retour défini par `T`. Il n’est pas possible de déduire d’autres types de retour possibles. Prenons l'exemple suivant :

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

L’action précédente retourne `ActionResult<T>`, mais à l’intérieur de l’action, [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) est retourné. Étant donné que le contrôleur est décoré avec l’attribut [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute), une réponse [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) est également possible. Pour plus d’informations, consultez [Réponses HTTP 400 automatiques](xref:web-api/index#automatic-http-400-responses). Les annotations de données sont utilisées pour indiquer aux clients les codes d’état HTTP que cette action est susceptible de retourner. Décorez l’action avec les attributs suivants :

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]
::: moniker-end

Le générateur Swagger peut maintenant décrire précisément cette action et les clients générés savent ce qu’ils reçoivent quand ils appellent le point de terminaison. Nous vous recommandons vivement de décorer toutes les actions avec ces attributs. Pour obtenir des indications sur les réponses HTTP que doivent retourner vos actions d’API, consultez la [spécification RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).
