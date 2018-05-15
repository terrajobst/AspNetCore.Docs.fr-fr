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
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="38741-103">Bien démarrer avec NSwag et ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="38741-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="38741-104">Par [Christoph Nienaber](https://twitter.com/zuckerthoben) et [Rico Suter](https://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="38741-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](https://rsuter.com)</span></span>

<span data-ttu-id="38741-105">L’utilisation de [NSwag](https://github.com/RSuter/NSwag) avec un intergiciel (middleware) ASP.NET Core nécessite le package NuGet [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/).</span><span class="sxs-lookup"><span data-stu-id="38741-105">Using [NSwag](https://github.com/RSuter/NSwag) with ASP.NET Core middleware requires the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="38741-106">Le package se compose d’un générateur Swagger, de l’IU Swagger (v2 et v3) et de [l’IU ReDoc](https://github.com/Rebilly/ReDoc).</span><span class="sxs-lookup"><span data-stu-id="38741-106">The package consists of a Swagger generator, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="38741-107">Nous vous recommandons vivement d’utiliser les fonctionnalités de génération de code de NSwag.</span><span class="sxs-lookup"><span data-stu-id="38741-107">It's highly recommended to make use of NSwag's code generation capabilities.</span></span> <span data-ttu-id="38741-108">Choisissez l’une des options suivantes pour la génération de code :</span><span class="sxs-lookup"><span data-stu-id="38741-108">Choose one of the following options for code generation:</span></span>

* <span data-ttu-id="38741-109">Utiliser [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), une application de bureau Windows pour générer du code client en C# et TypeScript pour votre API</span><span class="sxs-lookup"><span data-stu-id="38741-109">Use [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), a Windows desktop app for generating client code in C# and TypeScript for your API</span></span>
* <span data-ttu-id="38741-110">Utiliser les packages NuGet [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) ou [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) pour générer du code dans votre projet</span><span class="sxs-lookup"><span data-stu-id="38741-110">Use the [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages to do code generation inside your project</span></span>
* <span data-ttu-id="38741-111">Utiliser NSwag à partir de la [ligne de commande](https://github.com/NSwag/NSwag/wiki/CommandLine)</span><span class="sxs-lookup"><span data-stu-id="38741-111">Use NSwag from the [command line](https://github.com/NSwag/NSwag/wiki/CommandLine)</span></span>
* <span data-ttu-id="38741-112">Utiliser le package NuGet [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild)</span><span class="sxs-lookup"><span data-stu-id="38741-112">Use the [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet package</span></span>

## <a name="features"></a><span data-ttu-id="38741-113">Fonctionnalités</span><span class="sxs-lookup"><span data-stu-id="38741-113">Features</span></span>

<span data-ttu-id="38741-114">La principale raison d’utiliser NSwag est non seulement de pouvoir utiliser l’IU Swagger et le générateur Swagger, mais aussi d’utiliser ses fonctionnalités de génération de code flexibles.</span><span class="sxs-lookup"><span data-stu-id="38741-114">The main reason to use NSwag is the ability to not only introduce the Swagger UI and Swagger generator, but to make use of the flexible code generation capabilities.</span></span> <span data-ttu-id="38741-115">Vous n’avez pas besoin d’une API existante, vous pouvez utiliser des API de tiers qui intègrent Swagger et laissent NSwag générer une implémentation du client.</span><span class="sxs-lookup"><span data-stu-id="38741-115">You don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and let NSwag generate a client implementation.</span></span> <span data-ttu-id="38741-116">Dans les deux cas, le cycle de développement est rapide et vous pouvez vous adapter plus facilement aux changements d’API.</span><span class="sxs-lookup"><span data-stu-id="38741-116">Either way, the development cycle is expedited and you can more easily adapt to API changes.</span></span>

## <a name="package-installation"></a><span data-ttu-id="38741-117">Installation de package</span><span class="sxs-lookup"><span data-stu-id="38741-117">Package installation</span></span>

<span data-ttu-id="38741-118">Vous pouvez ajouter le package NuGet NSwag avec l’une des méthodes suivantes :</span><span class="sxs-lookup"><span data-stu-id="38741-118">The NSwag NuGet package can be added with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="38741-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="38741-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="38741-120">À partir de la fenêtre **Console du Gestionnaire de package** :</span><span class="sxs-lookup"><span data-stu-id="38741-120">From the **Package Manager Console** window:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="38741-121">À partir de la boîte de dialogue **Gérer les packages NuGet** :</span><span class="sxs-lookup"><span data-stu-id="38741-121">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="38741-122">Cliquez avec le bouton droit sur votre projet dans **l’Explorateur de solutions** > **Gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="38741-122">Right-click your project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="38741-123">Affectez la valeur « nuget.org » à **Source du package**.</span><span class="sxs-lookup"><span data-stu-id="38741-123">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="38741-124">Entrez « NSwag.AspNetCore » dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="38741-124">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="38741-125">Sélectionnez le package « NSwag.AspNetCore » sous l’onglet **Parcourir** et cliquez sur **Installer**.</span><span class="sxs-lookup"><span data-stu-id="38741-125">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="38741-126">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="38741-126">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="38741-127">Cliquez avec le bouton droit sur le dossier *Packages* dans **Panneau Solutions** > **Ajouter des packages**.</span><span class="sxs-lookup"><span data-stu-id="38741-127">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="38741-128">Dans la fenêtre **Ajouter des packages**, sélectionnez « nuget.org » dans la liste déroulante **Source**.</span><span class="sxs-lookup"><span data-stu-id="38741-128">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="38741-129">Entrez NSwag.AspNetCore dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="38741-129">Enter NSwag.AspNetCore in the search box</span></span>
* <span data-ttu-id="38741-130">Sélectionnez le package NSwag.AspNetCore dans le volet de résultats, puis cliquez sur **Ajouter un package**.</span><span class="sxs-lookup"><span data-stu-id="38741-130">Select the NSwag.AspNetCore package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="38741-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="38741-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="38741-132">Exécutez la commande suivante à partir du **Terminal intégré** :</span><span class="sxs-lookup"><span data-stu-id="38741-132">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="38741-133">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="38741-133">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="38741-134">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="38741-134">Run the following command:</span></span>

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="38741-135">Ajouter et configurer l’intergiciel (middleware) Swagger</span><span class="sxs-lookup"><span data-stu-id="38741-135">Add and configure Swagger middleware</span></span>

<span data-ttu-id="38741-136">Importez les espaces de noms suivants dans la classe `Info` :</span><span class="sxs-lookup"><span data-stu-id="38741-136">Import the following namespaces in the `Info` class:</span></span>

```csharp
using NSwag.AspNetCore;
using System.Reflection;
using NJsonSchema;
```

<span data-ttu-id="38741-137">Dans la méthode `Startup.Configure`, activez l’intergiciel pour traiter la spécification Swagger générée et l’IU Swagger :</span><span class="sxs-lookup"><span data-stu-id="38741-137">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI:</span></span>

[!code-cs[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=4,7-10)]

<span data-ttu-id="38741-138">Lancez l’application.</span><span class="sxs-lookup"><span data-stu-id="38741-138">Launch the app.</span></span> <span data-ttu-id="38741-139">Accédez à `/swagger` pour voir l’IU Swagger.</span><span class="sxs-lookup"><span data-stu-id="38741-139">Navigate to `/swagger` to view the Swagger UI.</span></span> <span data-ttu-id="38741-140">Accédez à `/swagger/v1/swagger.json` pour voir la spécification Swagger.</span><span class="sxs-lookup"><span data-stu-id="38741-140">Navigate to `/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="38741-141">Génération de code</span><span class="sxs-lookup"><span data-stu-id="38741-141">Code generation</span></span>

### <a name="via-nswagstudio"></a><span data-ttu-id="38741-142">Via NSwagStudio</span><span class="sxs-lookup"><span data-stu-id="38741-142">Via NSwagStudio</span></span>

* <span data-ttu-id="38741-143">Installez `NSwagStudio` à partir du [dépôt GitHub](https://github.com/RSuter/NSwag/wiki/NSwagStudio) officiel.</span><span class="sxs-lookup"><span data-stu-id="38741-143">Install `NSwagStudio` from the official [GitHub repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span></span>

* <span data-ttu-id="38741-144">Lancez NSwagStudio.</span><span class="sxs-lookup"><span data-stu-id="38741-144">Launch NSwagStudio.</span></span> <span data-ttu-id="38741-145">Entrez l’emplacement de votre fichier *swagger.json* ou copiez-le directement :</span><span class="sxs-lookup"><span data-stu-id="38741-145">Enter the location of your *swagger.json* or copy it directly:</span></span>

![NSwagStudio](web-api-help-pages-using-swagger/_static/NSwagStudio.png)

* <span data-ttu-id="38741-147">Indiquez le type de client de sortie souhaité.</span><span class="sxs-lookup"><span data-stu-id="38741-147">Indicate the desired client output type.</span></span> <span data-ttu-id="38741-148">Les options sont **TypeScript Client**, **CSharp Client** ou **CSharp Web API Controller**.</span><span class="sxs-lookup"><span data-stu-id="38741-148">Options include **TypeScript Client**, **CSharp Client**, or **CSharp Web API Controller**.</span></span> <span data-ttu-id="38741-149">Un contrôleur d’API web vous permet fondamentalement d’effectuer une génération inverse.</span><span class="sxs-lookup"><span data-stu-id="38741-149">Using a Web API Controller is basically a reverse generation.</span></span> <span data-ttu-id="38741-150">Il utilise la spécification d’un service pour regénérer le service.</span><span class="sxs-lookup"><span data-stu-id="38741-150">It uses a specification of a service to rebuild the service.</span></span>

* <span data-ttu-id="38741-151">Cliquez sur **Generate Outputs** (Générer les sorties).</span><span class="sxs-lookup"><span data-stu-id="38741-151">Click **Generate Outputs**.</span></span>

* <span data-ttu-id="38741-152">Vous pouvez voir une implémentation de client complète de l’exemple *TodoApi.NSwag* en C# :</span><span class="sxs-lookup"><span data-stu-id="38741-152">Here you see a complete client implementation of the *TodoApi.NSwag* sample in C#:</span></span>

![NSwagStudio-Output](web-api-help-pages-using-swagger/_static/NSwagStudio-Output.png)

* <span data-ttu-id="38741-154">Placez le fichier dans un projet client (par exemple, une application [Xamarin.Forms](/xamarin/xamarin-forms/)).</span><span class="sxs-lookup"><span data-stu-id="38741-154">Put the file into a client project (for example, a [Xamarin.Forms](/xamarin/xamarin-forms/) app).</span></span> <span data-ttu-id="38741-155">Démarrez l’utilisation de l’API :</span><span class="sxs-lookup"><span data-stu-id="38741-155">Start consuming the API:</span></span>

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
> <span data-ttu-id="38741-156">Vous pouvez injecter une URL de base et/ou un client HTTP dans le client d’API.</span><span class="sxs-lookup"><span data-stu-id="38741-156">You can inject a base URL and/or a HTTP client into the API client.</span></span> <span data-ttu-id="38741-157">En général, il vaut mieux toujours [réutiliser HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span><span class="sxs-lookup"><span data-stu-id="38741-157">The best practice is to always [reuse the HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span></span>

<span data-ttu-id="38741-158">Vous pouvez maintenant commencer à implémenter votre API facilement dans les projets clients.</span><span class="sxs-lookup"><span data-stu-id="38741-158">You can now start implementing your API into client projects easily.</span></span>

### <a name="other-ways-to-generate-client-code"></a><span data-ttu-id="38741-159">Autres façons de générer du code client</span><span class="sxs-lookup"><span data-stu-id="38741-159">Other ways to generate client code</span></span>

<span data-ttu-id="38741-160">Vous pouvez générer le code d’autres manières, plus adaptées à votre flux de travail :</span><span class="sxs-lookup"><span data-stu-id="38741-160">You can generate the code in other ways, more suited to your workflow:</span></span>

* [<span data-ttu-id="38741-161">MSBuild</span><span class="sxs-lookup"><span data-stu-id="38741-161">MSBuild</span></span>](https://www.nuget.org/packages/NSwag.MSBuild/)

* [<span data-ttu-id="38741-162">Dans le code</span><span class="sxs-lookup"><span data-stu-id="38741-162">In code</span></span>](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [<span data-ttu-id="38741-163">Modèles T4</span><span class="sxs-lookup"><span data-stu-id="38741-163">T4 templates</span></span>](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customization"></a><span data-ttu-id="38741-164">Personnalisation</span><span class="sxs-lookup"><span data-stu-id="38741-164">Customization</span></span>

### <a name="xml-comments"></a><span data-ttu-id="38741-165">commentaires XML</span><span class="sxs-lookup"><span data-stu-id="38741-165">XML comments</span></span>

<span data-ttu-id="38741-166">Vous pouvez activer les commentaires XML de l’une des façons suivantes :</span><span class="sxs-lookup"><span data-stu-id="38741-166">XML comments are enabled with the following approaches:</span></span>

#### <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="38741-167">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="38741-167">Visual Studio</span></span>](#tab/visual-studio-xml/)
* <span data-ttu-id="38741-168">Cliquez avec le bouton droit sur le projet dans **l’Explorateur de solutions**, puis sélectionnez **Propriétés**.</span><span class="sxs-lookup"><span data-stu-id="38741-168">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="38741-169">Cochez la zone **Fichier de documentation XML** dans la section **Sortie** de l’onglet **Générer** :</span><span class="sxs-lookup"><span data-stu-id="38741-169">Check the **XML documentation file** box under the **Output** section of the **Build** tab:</span></span>

![Onglet Générer des propriétés du projet](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

#### <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="38741-171">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="38741-171">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)
* <span data-ttu-id="38741-172">Ouvrez la boîte de dialogue **Options du projet** > **Générer** > **Compilateur**.</span><span class="sxs-lookup"><span data-stu-id="38741-172">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="38741-173">Cochez la zone **Générer la documentation XML** dans la section **Options générales** :</span><span class="sxs-lookup"><span data-stu-id="38741-173">Check the **Generate xml documentation** box under the **General Options** section:</span></span>

![Section Options générales des options du projet](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

#### <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="38741-175">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="38741-175">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)
<span data-ttu-id="38741-176">Ajoutez manuellement l’extrait de code suivant au fichier *.csproj* :</span><span class="sxs-lookup"><span data-stu-id="38741-176">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.NSwag/TodoApiNSwag.csproj?range=7-9)]

* * *
## <a name="data-annotations"></a><span data-ttu-id="38741-177">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="38741-177">Data annotations</span></span>

<span data-ttu-id="38741-178">NSwag utilise la [réflexion](/dotnet/csharp/programming-guide/concepts/reflection) et le mieux pour les actions d’API web est de retourner [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span><span class="sxs-lookup"><span data-stu-id="38741-178">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the best practice for Web API actions is to return [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span></span> <span data-ttu-id="38741-179">Par conséquent, NSwag ne peut pas déduire votre action et ce qu’elle retourne.</span><span class="sxs-lookup"><span data-stu-id="38741-179">Consequently, NSwag can't infer what your action is doing and what it returns.</span></span> <span data-ttu-id="38741-180">Prenons l'exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="38741-180">Consider the following example:</span></span>

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

<span data-ttu-id="38741-181">L’action précédente retourne `IActionResult`, mais à l’intérieur de l’action, [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) ou [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) sont retournés.</span><span class="sxs-lookup"><span data-stu-id="38741-181">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) or [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span></span> <span data-ttu-id="38741-182">Les annotations de données sont utilisées pour indiquer aux clients la réponse HTTP retournée par cette action.</span><span class="sxs-lookup"><span data-stu-id="38741-182">Data annotations are used to tell clients which HTTP response this action is returning.</span></span> <span data-ttu-id="38741-183">Décorez l’action avec les attributs suivants :</span><span class="sxs-lookup"><span data-stu-id="38741-183">Decorate the action with the following attributes:</span></span>

```csharp
[HttpPost]
[ProducesResponseType(typeof(TodoItem), 201)] // Created
[ProducesResponseType(typeof(TodoItem), 400)] // BadRequest
public IActionResult Create([FromBody] TodoItem item)
```

<span data-ttu-id="38741-184">Le générateur Swagger peut maintenant décrire précisément cette action et les clients générés savent ce qu’ils reçoivent quand ils appellent le point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="38741-184">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="38741-185">Nous vous recommandons vivement de décorer toutes les actions avec ces attributs.</span><span class="sxs-lookup"><span data-stu-id="38741-185">Decorating all actions with these attributes is highly recommended.</span></span> <span data-ttu-id="38741-186">Pour obtenir des indications sur les réponses HTTP que doivent retourner vos actions d’API, consultez la [spécification RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).</span><span class="sxs-lookup"><span data-stu-id="38741-186">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
