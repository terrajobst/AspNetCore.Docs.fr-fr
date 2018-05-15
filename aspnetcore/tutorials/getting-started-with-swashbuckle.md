---
title: Bien démarrer avec Swashbuckle et ASP.NET Core
author: zuckerthoben
description: Découvrez comment ajouter Swashbuckle à votre projet ASP.NET Core pour intégrer l’IU Swagger.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 03/26/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-swashbuckle
ms.openlocfilehash: e90339f2884dd9b20cf135f879c9cab6110efecf
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-swashbuckle-and-aspnet-core"></a><span data-ttu-id="e793f-103">Bien démarrer avec Swashbuckle et ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e793f-103">Get started with Swashbuckle and ASP.NET Core</span></span>

<span data-ttu-id="e793f-104">De [Shayne Boyer](https://twitter.com/spboyer) et [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="e793f-104">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="e793f-105">Swashbuckle compte trois composants principaux :</span><span class="sxs-lookup"><span data-stu-id="e793f-105">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="e793f-106">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/) : modèle objet Swagger et intergiciel (middleware) pour exposer des objets `SwaggerDocument` sous forme de points de terminaison JSON.</span><span class="sxs-lookup"><span data-stu-id="e793f-106">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="e793f-107">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/) : générateur Swagger qui crée des objets `SwaggerDocument` directement à partir de vos routes, contrôleurs et modèles.</span><span class="sxs-lookup"><span data-stu-id="e793f-107">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="e793f-108">Il est généralement associé à l’intergiciel de point de terminaison Swagger pour exposer automatiquement Swagger JSON.</span><span class="sxs-lookup"><span data-stu-id="e793f-108">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="e793f-109">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): version incorporée de l’outil IU Swagger.</span><span class="sxs-lookup"><span data-stu-id="e793f-109">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): an embedded version of the Swagger UI tool.</span></span> <span data-ttu-id="e793f-110">Elle interprète Swagger JSON afin de générer une expérience complète et personnalisable pour décrire la fonctionnalité de l’API web.</span><span class="sxs-lookup"><span data-stu-id="e793f-110">It interprets Swagger JSON to build a rich, customizable experience for describing the Web API functionality.</span></span> <span data-ttu-id="e793f-111">Il inclut des ateliers de test intégrés pour les méthodes publiques.</span><span class="sxs-lookup"><span data-stu-id="e793f-111">It includes built-in test harnesses for the public methods.</span></span>

## <a name="package-installation"></a><span data-ttu-id="e793f-112">Installation de package</span><span class="sxs-lookup"><span data-stu-id="e793f-112">Package installation</span></span>

<span data-ttu-id="e793f-113">Vous pouvez ajouter Swashbuckle en adoptant l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="e793f-113">Swashbuckle can be added with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e793f-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e793f-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e793f-115">À partir de la fenêtre **Console du Gestionnaire de package** :</span><span class="sxs-lookup"><span data-stu-id="e793f-115">From the **Package Manager Console** window:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* <span data-ttu-id="e793f-116">À partir de la boîte de dialogue **Gérer les packages NuGet** :</span><span class="sxs-lookup"><span data-stu-id="e793f-116">From the **Manage NuGet Packages** dialog:</span></span>

  * <span data-ttu-id="e793f-117">Cliquez avec le bouton droit sur votre projet dans **l’Explorateur de solutions** > **Gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="e793f-117">Right-click your project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="e793f-118">Affectez la valeur « nuget.org » à **Source du package**.</span><span class="sxs-lookup"><span data-stu-id="e793f-118">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="e793f-119">Entrez « Swashbuckle.AspNetCore » dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="e793f-119">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="e793f-120">Sélectionnez le package « Swashbuckle.AspNetCore » sous l’onglet **Parcourir** et cliquez sur **Installer**.</span><span class="sxs-lookup"><span data-stu-id="e793f-120">Select the "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e793f-121">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e793f-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e793f-122">Cliquez avec le bouton droit sur le dossier *Packages* dans **Panneau Solutions** > **Ajouter des packages**.</span><span class="sxs-lookup"><span data-stu-id="e793f-122">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="e793f-123">Dans la fenêtre **Ajouter des packages**, sélectionnez « nuget.org » dans la liste déroulante **Source**.</span><span class="sxs-lookup"><span data-stu-id="e793f-123">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="e793f-124">Entrez Swashbuckle.AspNetCore dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="e793f-124">Enter Swashbuckle.AspNetCore in the search box</span></span>
* <span data-ttu-id="e793f-125">Sélectionnez le package Swashbuckle.AspNetCore dans le volet de résultats, puis cliquez sur **Ajouter un package**.</span><span class="sxs-lookup"><span data-stu-id="e793f-125">Select the Swashbuckle.AspNetCore package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e793f-126">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e793f-126">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e793f-127">Exécutez la commande suivante à partir du **Terminal intégré** :</span><span class="sxs-lookup"><span data-stu-id="e793f-127">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e793f-128">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="e793f-128">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="e793f-129">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="e793f-129">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="e793f-130">Ajouter et configurer l’intergiciel (middleware) Swagger</span><span class="sxs-lookup"><span data-stu-id="e793f-130">Add and configure Swagger middleware</span></span>

<span data-ttu-id="e793f-131">Ajoutez le générateur Swagger à la collection de services dans la méthode `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="e793f-131">Add the Swagger generator to the services collection in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]

<span data-ttu-id="e793f-132">Importez les espaces de noms suivants à utiliser dans la classe `Info` :</span><span class="sxs-lookup"><span data-stu-id="e793f-132">Import the following namespace to use the `Info` class:</span></span>

```csharp
using Swashbuckle.AspNetCore.Swagger;
```

<span data-ttu-id="e793f-133">Dans la méthode `Startup.Configure`, activez l’intergiciel pour traiter le document JSON généré et l’IU Swagger :</span><span class="sxs-lookup"><span data-stu-id="e793f-133">In the `Startup.Configure` method, enable the middleware for serving the generated JSON document and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]

<span data-ttu-id="e793f-134">Lancez l’application et accédez à `http://localhost:<random_port>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="e793f-134">Launch the app, and navigate to `http://localhost:<random_port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="e793f-135">Le document généré décrivant les points de terminaison s’affiche comme illustré dans la [spécification Swagger (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span><span class="sxs-lookup"><span data-stu-id="e793f-135">The generated document describing the endpoints appears as shown in [Swagger specification (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span></span>

<span data-ttu-id="e793f-136">L’IU Swagger se trouve à l’adresse `http://localhost:<random_port>/swagger`.</span><span class="sxs-lookup"><span data-stu-id="e793f-136">The Swagger UI can be found at `http://localhost:<random_port>/swagger`.</span></span> <span data-ttu-id="e793f-137">Explorez l’API via l’IU Swagger et incorporez-la dans d’autres programmes.</span><span class="sxs-lookup"><span data-stu-id="e793f-137">Explore the API via Swagger UI and incorporate it in other programs.</span></span>

> [!TIP]
> <span data-ttu-id="e793f-138">Pour utiliser l’IU Swagger à la racine de l’application (`http://localhost:<random_port>/`), définissez la propriété `RoutePrefix` sur une chaîne vide :</span><span class="sxs-lookup"><span data-stu-id="e793f-138">To serve the Swagger UI at the app's root (`http://localhost:<random_port>/`), set the `RoutePrefix` property to an empty string:</span></span>
> 
> [!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup3.cs?name=snippet_UseSwaggerUI&highlight=4)]

## <a name="customize--extend"></a><span data-ttu-id="e793f-139">Personnaliser et étendre</span><span class="sxs-lookup"><span data-stu-id="e793f-139">Customize & extend</span></span>

<span data-ttu-id="e793f-140">Swagger fournit des options pour documenter le modèle objet et personnaliser l’interface utilisateur en fonction de votre thème.</span><span class="sxs-lookup"><span data-stu-id="e793f-140">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="e793f-141">Informations sur l’API et description</span><span class="sxs-lookup"><span data-stu-id="e793f-141">API info and description</span></span>

<span data-ttu-id="e793f-142">L’action de configuration transmise à la méthode `AddSwaggerGen` ajoute des informations comme l’auteur, la licence et la description :</span><span class="sxs-lookup"><span data-stu-id="e793f-142">The configuration action passed to the `AddSwaggerGen` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup.cs?range=21-40,46)]

<span data-ttu-id="e793f-143">L’IU Swagger affiche les informations de la version :</span><span class="sxs-lookup"><span data-stu-id="e793f-143">The Swagger UI displays the version's information:</span></span>

![Interface utilisateur de Swagger avec les informations de version : description, auteur et lien pour en savoir plus](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="e793f-145">commentaires XML</span><span class="sxs-lookup"><span data-stu-id="e793f-145">XML comments</span></span>

<span data-ttu-id="e793f-146">Vous pouvez activer les commentaires XML en adoptant l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="e793f-146">XML comments can be enabled with the following approaches:</span></span>

#### <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="e793f-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e793f-147">Visual Studio</span></span>](#tab/visual-studio-xml/)
* <span data-ttu-id="e793f-148">Cliquez avec le bouton droit sur le projet dans **l’Explorateur de solutions**, puis sélectionnez **Propriétés**.</span><span class="sxs-lookup"><span data-stu-id="e793f-148">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="e793f-149">Cochez la zone **Fichier de documentation XML** dans la section **Sortie** de l’onglet **Générer** :</span><span class="sxs-lookup"><span data-stu-id="e793f-149">Check the **XML documentation file** box under the **Output** section of the **Build** tab:</span></span>

![Onglet Générer des propriétés du projet](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

#### <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="e793f-151">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e793f-151">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)
* <span data-ttu-id="e793f-152">Ouvrez la boîte de dialogue **Options du projet** > **Générer** > **Compilateur**.</span><span class="sxs-lookup"><span data-stu-id="e793f-152">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="e793f-153">Cochez la zone **Générer la documentation XML** dans la section **Options générales** :</span><span class="sxs-lookup"><span data-stu-id="e793f-153">Check the **Generate xml documentation** box under the **General Options** section:</span></span>

![Section Options générales des options du projet](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

#### <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="e793f-155">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e793f-155">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)
<span data-ttu-id="e793f-156">Ajoutez manuellement l’extrait de code suivant au fichier *.csproj* :</span><span class="sxs-lookup"><span data-stu-id="e793f-156">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=2)]

* * *
<span data-ttu-id="e793f-157">L’activation de commentaires XML fournit des informations de débogage sur les membres et types publics non documentés.</span><span class="sxs-lookup"><span data-stu-id="e793f-157">Enabling XML comments provides debug information for undocumented public types and members.</span></span> <span data-ttu-id="e793f-158">Les membres et types non documentés sont indiqués par le message d’avertissement.</span><span class="sxs-lookup"><span data-stu-id="e793f-158">Undocumented types and members are indicated by the warning message.</span></span> <span data-ttu-id="e793f-159">Par exemple, le message suivant indique une violation du code d’avertissement 1591 :</span><span class="sxs-lookup"><span data-stu-id="e793f-159">For example, the following message indicates a violation of warning code 1591:</span></span>

```text
warning CS1591: Missing XML comment for publicly visible type or member 'TodoController.GetAll()'
```

<span data-ttu-id="e793f-160">Supprimez les avertissements en définissant une liste de codes d’avertissement séparée par des points-virgules à ignorer dans le fichier *.csproj* :</span><span class="sxs-lookup"><span data-stu-id="e793f-160">Suppress warnings by defining a semicolon-delimited list of warning codes to ignore in the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

<span data-ttu-id="e793f-161">Configurez Swagger pour utiliser le fichier XML généré.</span><span class="sxs-lookup"><span data-stu-id="e793f-161">Configure Swagger to use the generated XML file.</span></span> <span data-ttu-id="e793f-162">Pour les systèmes d’exploitation Linux ou non-Windows, les chemins et les noms de fichiers peuvent respecter la casse.</span><span class="sxs-lookup"><span data-stu-id="e793f-162">For Linux or non-Windows operating systems, file names and paths can be case-sensitive.</span></span> <span data-ttu-id="e793f-163">Par exemple, un fichier *ToDoApi.XML* est valide sur Windows, mais pas sur CentOS.</span><span class="sxs-lookup"><span data-stu-id="e793f-163">For example, a *TodoApi.XML* file is valid on Windows but not CentOS.</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=29-31)]

<span data-ttu-id="e793f-164">Dans le code précédent, la [réflexion](/dotnet/csharp/programming-guide/concepts/reflection) est utilisée pour générer un nom de fichier XML correspondant à celui du projet d’API web.</span><span class="sxs-lookup"><span data-stu-id="e793f-164">In the preceding code, [Reflection](/dotnet/csharp/programming-guide/concepts/reflection) is used to build an XML file name matching that of the Web API project.</span></span> <span data-ttu-id="e793f-165">Cette approche garantit que le nom de fichier XML généré correspond au nom de projet.</span><span class="sxs-lookup"><span data-stu-id="e793f-165">This approach ensures that the generated XML file name matches the project name.</span></span> <span data-ttu-id="e793f-166">La propriété [AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory#System_AppContext_BaseDirectory) est utilisée pour construire le chemin du fichier XML.</span><span class="sxs-lookup"><span data-stu-id="e793f-166">The [AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory#System_AppContext_BaseDirectory) property is used to construct a path to the XML file.</span></span>

<span data-ttu-id="e793f-167">Quand vous ajoutez des commentaires avec trois barres obliques à une action, la description est ajoutée à l’en-tête de section dans l’IU Swagger.</span><span class="sxs-lookup"><span data-stu-id="e793f-167">Adding triple-slash comments to an action enhances the Swagger UI by adding the description to the section header.</span></span> <span data-ttu-id="e793f-168">Ajoutez un élément [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) au dessus de l’action `Delete` :</span><span class="sxs-lookup"><span data-stu-id="e793f-168">Add a [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) element above the `Delete` action:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Delete&highlight=1-3)]

<span data-ttu-id="e793f-169">L’IU Swagger affiche le texte interne de l’élément `<summary>` du code précédent :</span><span class="sxs-lookup"><span data-stu-id="e793f-169">The Swagger UI displays the inner text of the preceding code's `<summary>` element:</span></span>

![Interface utilisateur de Swagger affichant le commentaire XML « Deletes a specific TodoItem. »](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="e793f-172">L’IU est définie par le schéma JSON généré :</span><span class="sxs-lookup"><span data-stu-id="e793f-172">The UI is driven by the generated JSON schema:</span></span>

```json
"delete": {
    "tags": [
        "Todo"
    ],
    "summary": "Deletes a specific TodoItem.",
    "operationId": "ApiTodoByIdDelete",
    "consumes": [],
    "produces": [],
    "parameters": [
        {
            "name": "id",
            "in": "path",
            "description": "",
            "required": true,
            "type": "integer",
            "format": "int64"
        }
    ],
    "responses": {
        "200": {
            "description": "Success"
        }
    }
}
```

<span data-ttu-id="e793f-173">Ajoutez un élément [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) à la documentation de la méthode de l’action `Create`.</span><span class="sxs-lookup"><span data-stu-id="e793f-173">Add a [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) element to the `Create` action method documentation.</span></span> <span data-ttu-id="e793f-174">Il complète les informations spécifiées dans l’élément `<summary>` et fournit une IU Swagger plus robuste.</span><span class="sxs-lookup"><span data-stu-id="e793f-174">It supplements information specified in the `<summary>` element and provides a more robust Swagger UI.</span></span> <span data-ttu-id="e793f-175">Le contenu de l’élément `<remarks>` peut être du texte, du code JSON ou du code XML.</span><span class="sxs-lookup"><span data-stu-id="e793f-175">The `<remarks>` element content can consist of text, JSON, or XML.</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

<span data-ttu-id="e793f-176">Notez les améliorations de l’IU avec ces commentaires supplémentaires :</span><span class="sxs-lookup"><span data-stu-id="e793f-176">Notice the UI enhancements with these additional comments:</span></span>

![Interface utilisateur de Swagger avec des commentaires supplémentaires affichés](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="e793f-178">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="e793f-178">Data annotations</span></span>

<span data-ttu-id="e793f-179">Décorez le modèle avec des attributs, présents dans l’espace de noms [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations), pour gérer les composants de l’IU Swagger.</span><span class="sxs-lookup"><span data-stu-id="e793f-179">Decorate the model with attributes, found in the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="e793f-180">Ajoutez l’attribut `[Required]` à la propriété `Name` de la classe `TodoItem` :</span><span class="sxs-lookup"><span data-stu-id="e793f-180">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="e793f-181">La présence de cet attribut change le comportement de l’interface utilisateur et modifie le schéma JSON sous-jacent :</span><span class="sxs-lookup"><span data-stu-id="e793f-181">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

```json
"definitions": {
    "TodoItem": {
        "required": [
            "name"
        ],
        "type": "object",
        "properties": {
            "id": {
                "format": "int64",
                "type": "integer"
            },
            "name": {
                "type": "string"
            },
            "isComplete": {
                "default": false,
                "type": "boolean"
            }
        }
    }
},
```

<span data-ttu-id="e793f-182">Ajoutez l’attribut `[Produces("application/json")]` au contrôleur d’API.</span><span class="sxs-lookup"><span data-stu-id="e793f-182">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="e793f-183">Son objectif est de déclarer que les actions du contrôleur prennent en charge une réponse dont le type de contenu est *application/json* :</span><span class="sxs-lookup"><span data-stu-id="e793f-183">Its purpose is to declare that the controller's actions support a response content type of *application/json*:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]

<span data-ttu-id="e793f-184">La zone de liste déroulante **Response Content Type** permet de sélectionner ce type de contenu comme valeur par défaut pour les actions GET du contrôleur :</span><span class="sxs-lookup"><span data-stu-id="e793f-184">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Interface utilisateur de Swagger avec le type de contenu de réponse par défaut](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="e793f-186">À mesure que l’utilisation des annotations de données dans l’API web augmente, l’interface utilisateur et les pages d’aide de l’API deviennent de plus en plus descriptives et utiles.</span><span class="sxs-lookup"><span data-stu-id="e793f-186">As the usage of data annotations in the Web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describe-response-types"></a><span data-ttu-id="e793f-187">Décrire des types de réponse</span><span class="sxs-lookup"><span data-stu-id="e793f-187">Describe response types</span></span>

<span data-ttu-id="e793f-188">Les développeurs consommateurs s’intéressent surtout à ce qui est retourné, en particulier les types de réponse et les codes d’erreur (s’ils ne sont pas standard).</span><span class="sxs-lookup"><span data-stu-id="e793f-188">Consuming developers are most concerned with what's returned&mdash;specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="e793f-189">Les types de réponse et les codes d’erreur sont décrits dans les commentaires XML et les annotations de données.</span><span class="sxs-lookup"><span data-stu-id="e793f-189">The response types and error codes are denoted in the XML comments and data annotations.</span></span>

<span data-ttu-id="e793f-190">L’action `Create` retourne un code d’état HTTP 201 en cas de réussite.</span><span class="sxs-lookup"><span data-stu-id="e793f-190">The `Create` action returns an HTTP 201 status code on success.</span></span> <span data-ttu-id="e793f-191">Un code d’état HTTP 400 est retourné quand le corps de la demande postée est null.</span><span class="sxs-lookup"><span data-stu-id="e793f-191">An HTTP 400 status code is returned when the posted request body is null.</span></span> <span data-ttu-id="e793f-192">Sans documentation appropriée dans l’interface utilisateur de Swagger, le consommateur n’a pas connaissance de ces résultats attendus.</span><span class="sxs-lookup"><span data-stu-id="e793f-192">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="e793f-193">Pour résoudre ce problème, ajoutez les lignes en surbrillance de l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="e793f-193">Fix that problem by adding the highlighted lines in the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

<span data-ttu-id="e793f-194">L’interface utilisateur de Swagger documente maintenant clairement les codes de réponse HTTP attendus :</span><span class="sxs-lookup"><span data-stu-id="e793f-194">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![Interface utilisateur de Swagger affichant la description de la classe de réponse POST « Returns the newly created Todo item » et « 400 - If the item is null » pour le code d’état et la raison sous Response Messages](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customize-the-ui"></a><span data-ttu-id="e793f-196">Personnaliser l’IU</span><span class="sxs-lookup"><span data-stu-id="e793f-196">Customize the UI</span></span>

<span data-ttu-id="e793f-197">L’IU de stock est fonctionnelle et conviviale.</span><span class="sxs-lookup"><span data-stu-id="e793f-197">The stock UI is both functional and presentable.</span></span> <span data-ttu-id="e793f-198">Toutefois, les pages de documentation d’API doivent représenter votre marque ou thème.</span><span class="sxs-lookup"><span data-stu-id="e793f-198">However, API documentation pages should represent your brand or theme.</span></span> <span data-ttu-id="e793f-199">La personnalisation des composants Swashbuckle nécessite d’ajouter les ressources qui traitent les fichiers statiques et de générer la structure de dossiers pour héberger ces fichiers.</span><span class="sxs-lookup"><span data-stu-id="e793f-199">Branding the Swashbuckle components requires adding the resources to serve static files and building the folder structure to host those files.</span></span>

<span data-ttu-id="e793f-200">Si vous ciblez le .NET Framework ou .NET Core 1.x, ajoutez le package NuGet [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) au projet :</span><span class="sxs-lookup"><span data-stu-id="e793f-200">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="e793f-201">Le package NuGet précédent est déjà installé si vous ciblez .NET Core 2.x et que vous utilisez le [métapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="e793f-201">The preceding NuGet package is already installed if targeting .NET Core 2.x and using the [metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="e793f-202">Activez l’intergiciel des fichiers statiques :</span><span class="sxs-lookup"><span data-stu-id="e793f-202">Enable the static files middleware:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup.cs?name=snippet_Configure&highlight=3)]

<span data-ttu-id="e793f-203">Faites l’acquisition du contenu du dossier *dist* à partir du [dépôt GitHub de l’interface utilisateur de Swagger](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span><span class="sxs-lookup"><span data-stu-id="e793f-203">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span></span> <span data-ttu-id="e793f-204">Ce dossier contient les ressources nécessaires pour la page de l’interface utilisateur de Swagger.</span><span class="sxs-lookup"><span data-stu-id="e793f-204">This folder contains the necessary assets for the Swagger UI page.</span></span>

<span data-ttu-id="e793f-205">Créez un dossier *wwwroot/swagger/ui* et copiez dedans le contenu du dossier *dist*.</span><span class="sxs-lookup"><span data-stu-id="e793f-205">Create a *wwwroot/swagger/ui* folder, and copy into it the contents of the *dist* folder.</span></span>

<span data-ttu-id="e793f-206">Créez un fichier *custom.css* dans *wwwroot/swagger/ui* avec le code CSS suivant pour personnaliser l’en-tête de page :</span><span class="sxs-lookup"><span data-stu-id="e793f-206">Create a *custom.css* file, in *wwwroot/swagger/ui*, with the following CSS to customize the page header:</span></span>

[!code-css[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/wwwroot/swagger/ui/custom.css)]

<span data-ttu-id="e793f-207">Référencez *custom.css* dans le fichier *index.html*, à la suite de n’importe quel autre fichier CSS :</span><span class="sxs-lookup"><span data-stu-id="e793f-207">Reference *custom.css* in the *index.html* file, after any other CSS files:</span></span>

[!code-html[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/wwwroot/swagger/ui/index.html?name=snippet_SwaggerUiCss&highlight=3)]

<span data-ttu-id="e793f-208">Accédez à la page *index.html* à l’adresse `http://localhost:<random_port>/swagger/ui/index.html`.</span><span class="sxs-lookup"><span data-stu-id="e793f-208">Browse to the *index.html* page at `http://localhost:<random_port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="e793f-209">Entrez `http://localhost:<random_port>/swagger/v1/swagger.json` dans la zone de texte de l’en-tête, puis cliquez sur le bouton **Explorer**.</span><span class="sxs-lookup"><span data-stu-id="e793f-209">Enter `http://localhost:<random_port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="e793f-210">La page résultante ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="e793f-210">The resulting page looks as follows:</span></span>

![Interface utilisateur de Swagger avec titre d’en-tête personnalisé](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="e793f-212">Vous pouvez faire encore beaucoup avec cette page.</span><span class="sxs-lookup"><span data-stu-id="e793f-212">There's much more you can do with the page.</span></span> <span data-ttu-id="e793f-213">Pour découvrir toutes les fonctionnalités pour les ressources d’interface utilisateur, accédez au [dépôt GitHub de l’interface utilisateur de Swagger](https://github.com/swagger-api/swagger-ui).</span><span class="sxs-lookup"><span data-stu-id="e793f-213">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>
