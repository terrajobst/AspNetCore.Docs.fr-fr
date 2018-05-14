---
title: Bien démarrer avec NSwag et ASP.NET Core
author: zuckerthoben
description: Découvrez comment utiliser NSwag pour générer des pages d’aide et de documentation pour une application API web ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 03/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: 80e6a9e1702d8f68d139d2ff9c3a01a27c40cecb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-nswag-and-aspnet-core"></a>Bien démarrer avec NSwag et ASP.NET Core

Par [Christoph Nienaber](https://twitter.com/zuckerthoben) et [Rico Suter](https://rsuter.com)

L’utilisation de [NSwag](https://github.com/RSuter/NSwag) avec un intergiciel (middleware) ASP.NET Core nécessite le package NuGet [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/). Le package se compose d’un générateur Swagger, de l’IU Swagger (v2 et v3) et de [l’IU ReDoc](https://github.com/Rebilly/ReDoc).

Nous vous recommandons vivement d’utiliser les fonctionnalités de génération de code de NSwag. Choisissez l’une des options suivantes pour la génération de code :

* Utiliser [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), une application de bureau Windows pour générer du code client en C# et TypeScript pour votre API
* Utiliser les packages NuGet [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) ou [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) pour générer du code dans votre projet
* Utiliser NSwag à partir de la [ligne de commande](https://github.com/NSwag/NSwag/wiki/CommandLine)
* Utiliser le package NuGet [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild)

## <a name="features"></a>Fonctionnalités

La principale raison d’utiliser NSwag est non seulement de pouvoir utiliser l’IU Swagger et le générateur Swagger, mais aussi d’utiliser ses fonctionnalités de génération de code flexibles. Vous n’avez pas besoin d’une API existante, vous pouvez utiliser des API de tiers qui intègrent Swagger et laissent NSwag générer une implémentation du client. Dans les deux cas, le cycle de développement est rapide et vous pouvez vous adapter plus facilement aux changements d’API.

## <a name="package-installation"></a>Installation de package

Vous pouvez ajouter le package NuGet NSwag avec l’une des méthodes suivantes :

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* À partir de la fenêtre **Console du Gestionnaire de package** :

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* À partir de la boîte de dialogue **Gérer les packages NuGet** :
  * Cliquez avec le bouton droit sur votre projet dans **l’Explorateur de solutions** > **Gérer les packages NuGet**.
  * Affectez la valeur « nuget.org » à **Source du package**.
  * Entrez « NSwag.AspNetCore » dans la zone de recherche.
  * Sélectionnez le package « NSwag.AspNetCore » sous l’onglet **Parcourir** et cliquez sur **Installer**.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

* Cliquez avec le bouton droit sur le dossier *Packages* dans **Panneau Solutions** > **Ajouter des packages**.
* Dans la fenêtre **Ajouter des packages**, sélectionnez « nuget.org » dans la liste déroulante **Source**.
* Entrez NSwag.AspNetCore dans la zone de recherche.
* Sélectionnez le package NSwag.AspNetCore dans le volet de résultats, puis cliquez sur **Ajouter un package**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Exécutez la commande suivante à partir du **Terminal intégré** :

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

Exécutez la commande suivante :

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a>Ajouter et configurer l’intergiciel (middleware) Swagger

Importez les espaces de noms suivants dans la classe `Info` :

```csharp
using NSwag.AspNetCore;
using System.Reflection;
using NJsonSchema;
```

Dans la méthode `Startup.Configure`, activez l’intergiciel pour traiter la spécification Swagger générée et l’IU Swagger :

[!code-cs[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=4,7-10)]

Lancez l’application. Accédez à `/swagger` pour voir l’IU Swagger. Accédez à `/swagger/v1/swagger.json` pour voir la spécification Swagger.

## <a name="code-generation"></a>Génération de code

### <a name="via-nswagstudio"></a>Via NSwagStudio

* Installez `NSwagStudio` à partir du [dépôt GitHub](https://github.com/RSuter/NSwag/wiki/NSwagStudio) officiel.

* Lancez NSwagStudio. Entrez l’emplacement de votre fichier *swagger.json* ou copiez-le directement :

![NSwagStudio](web-api-help-pages-using-swagger/_static/NSwagStudio.png)

* Indiquez le type de client de sortie souhaité. Les options sont **TypeScript Client**, **CSharp Client** ou **CSharp Web API Controller**. Un contrôleur d’API web vous permet fondamentalement d’effectuer une génération inverse. Il utilise la spécification d’un service pour regénérer le service.

* Cliquez sur **Generate Outputs** (Générer les sorties).

* Vous pouvez voir une implémentation de client complète de l’exemple *TodoApi.NSwag* en C# :

![NSwagStudio-Output](web-api-help-pages-using-swagger/_static/NSwagStudio-Output.png)

* Placez le fichier dans un projet client (par exemple, une application [Xamarin.Forms](/xamarin/xamarin-forms/)). Démarrez l’utilisation de l’API :

```csharp
var todoClient = new TodoClient();

// Gets all Todos from the Api
var allTodos = await todoClient.GetAllAsync();

// Create a new TodoItem and save it in the Api
var createdTodo = await todoClient.CreateAsync(new TodoItem);

// Get a single Todo by Id
var foundTodo = await todoClient.GetByIdAsync(1);
```

> [!NOTE]
> Vous pouvez injecter une URL de base et/ou un client HTTP dans le client d’API. En général, il vaut mieux toujours [réutiliser HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).

Vous pouvez maintenant commencer à implémenter votre API facilement dans les projets clients.

### <a name="other-ways-to-generate-client-code"></a>Autres façons de générer du code client

Vous pouvez générer le code d’autres manières, plus adaptées à votre flux de travail :

* [MSBuild](https://www.nuget.org/packages/NSwag.MSBuild/)

* [Dans le code](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [Modèles T4](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customization"></a>Personnalisation

### <a name="xml-comments"></a>commentaires XML

Vous pouvez activer les commentaires XML de l’une des façons suivantes :

#### <a name="visual-studiotabvisual-studio-xml"></a>[Visual Studio](#tab/visual-studio-xml/)
* Cliquez avec le bouton droit sur le projet dans **l’Explorateur de solutions**, puis sélectionnez **Propriétés**.
* Cochez la zone **Fichier de documentation XML** dans la section **Sortie** de l’onglet **Générer** :

![Onglet Générer des propriétés du projet](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

#### <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[Visual Studio pour Mac](#tab/visual-studio-mac-xml/)
* Ouvrez la boîte de dialogue **Options du projet** > **Générer** > **Compilateur**.
* Cochez la zone **Générer la documentation XML** dans la section **Options générales** :

![Section Options générales des options du projet](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

#### <a name="visual-studio-codetabvisual-studio-code-xml"></a>[Visual Studio Code](#tab/visual-studio-code-xml/)
Ajoutez manuellement l’extrait de code suivant au fichier *.csproj* :

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.NSwag/TodoApiNSwag.csproj?range=7-9)]

* * *
## <a name="data-annotations"></a>Annotations de données

NSwag utilise la [réflexion](/dotnet/csharp/programming-guide/concepts/reflection) et le mieux pour les actions d’API web est de retourner [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult). Par conséquent, NSwag ne peut pas déduire votre action et ce qu’elle retourne. Prenons l'exemple suivant :

```csharp
public IActionResult Create([FromBody] TodoItem item)
{
    if (item == null)
    {
        return BadRequest();
    }

    _context.TodoItems.Add(item);
    _context.SaveChanges();

     return CreatedAtRoute("GetTodo", new { id = item.Id }, item);
}
```

L’action précédente retourne `IActionResult`, mais à l’intérieur de l’action, [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) ou [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) sont retournés. Les annotations de données sont utilisées pour indiquer aux clients la réponse HTTP retournée par cette action. Décorez l’action avec les attributs suivants :

```csharp
[HttpPost]
[ProducesResponseType(typeof(TodoItem), 201)] // Created
[ProducesResponseType(typeof(TodoItem), 400)] // BadRequest
public IActionResult Create([FromBody] TodoItem item)
```

Le générateur Swagger peut maintenant décrire précisément cette action et les clients générés savent ce qu’ils reçoivent quand ils appellent le point de terminaison. Nous vous recommandons vivement de décorer toutes les actions avec ces attributs. Pour obtenir des indications sur les réponses HTTP que doivent retourner vos actions d’API, consultez la [spécification RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).
