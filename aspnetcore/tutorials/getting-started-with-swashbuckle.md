---
title: Bien démarrer avec Swashbuckle et ASP.NET Core
author: zuckerthoben
description: Découvrez comment ajouter Swashbuckle à votre projet d’API web ASP.NET Core pour intégrer l’IU Swagger.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/08/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-swashbuckle
ms.openlocfilehash: 0eb9aa12419cc09899af6bc85dd32a85687dab62
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/12/2018
---
# <a name="get-started-with-swashbuckle-and-aspnet-core"></a><span data-ttu-id="64644-103">Bien démarrer avec Swashbuckle et ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="64644-103">Get started with Swashbuckle and ASP.NET Core</span></span>

<span data-ttu-id="64644-104">De [Shayne Boyer](https://twitter.com/spboyer) et [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="64644-104">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="64644-105">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="64644-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="64644-106">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="64644-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end

<span data-ttu-id="64644-107">Swashbuckle compte trois composants principaux :</span><span class="sxs-lookup"><span data-stu-id="64644-107">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="64644-108">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/) : modèle objet Swagger et intergiciel (middleware) pour exposer des objets `SwaggerDocument` sous forme de points de terminaison JSON.</span><span class="sxs-lookup"><span data-stu-id="64644-108">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="64644-109">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/) : générateur Swagger qui crée des objets `SwaggerDocument` directement à partir de vos routes, contrôleurs et modèles.</span><span class="sxs-lookup"><span data-stu-id="64644-109">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="64644-110">Il est généralement associé à l’intergiciel de point de terminaison Swagger pour exposer automatiquement Swagger JSON.</span><span class="sxs-lookup"><span data-stu-id="64644-110">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="64644-111">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): version incorporée de l’outil IU Swagger.</span><span class="sxs-lookup"><span data-stu-id="64644-111">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): an embedded version of the Swagger UI tool.</span></span> <span data-ttu-id="64644-112">Elle interprète Swagger JSON afin de générer une expérience complète et personnalisable pour décrire la fonctionnalité de l’API web.</span><span class="sxs-lookup"><span data-stu-id="64644-112">It interprets Swagger JSON to build a rich, customizable experience for describing the Web API functionality.</span></span> <span data-ttu-id="64644-113">Il inclut des ateliers de test intégrés pour les méthodes publiques.</span><span class="sxs-lookup"><span data-stu-id="64644-113">It includes built-in test harnesses for the public methods.</span></span>

## <a name="package-installation"></a><span data-ttu-id="64644-114">Installation de package</span><span class="sxs-lookup"><span data-stu-id="64644-114">Package installation</span></span>

<span data-ttu-id="64644-115">Vous pouvez ajouter Swashbuckle en adoptant l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="64644-115">Swashbuckle can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="64644-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="64644-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="64644-117">À partir de la fenêtre **Console du Gestionnaire de package** :</span><span class="sxs-lookup"><span data-stu-id="64644-117">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="64644-118">Accédez à **Affichage** > **Autres fenêtres** > **Console du Gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="64644-118">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="64644-119">Accédez au répertoire où se trouve le fichier *TodoApi.csproj*.</span><span class="sxs-lookup"><span data-stu-id="64644-119">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="64644-120">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="64644-120">Execute the following command:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* <span data-ttu-id="64644-121">À partir de la boîte de dialogue **Gérer les packages NuGet** :</span><span class="sxs-lookup"><span data-stu-id="64644-121">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="64644-122">Cliquez avec le bouton droit sur le projet dans **l’Explorateur de solutions** > **Gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="64644-122">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="64644-123">Affectez la valeur « nuget.org » à **Source du package**.</span><span class="sxs-lookup"><span data-stu-id="64644-123">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="64644-124">Entrez « Swashbuckle.AspNetCore » dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="64644-124">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="64644-125">Sélectionnez le package « Swashbuckle.AspNetCore » sous l’onglet **Parcourir** et cliquez sur **Installer**.</span><span class="sxs-lookup"><span data-stu-id="64644-125">Select the "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="64644-126">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="64644-126">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="64644-127">Cliquez avec le bouton droit sur le dossier *Packages* dans **Panneau Solutions** > **Ajouter des packages**.</span><span class="sxs-lookup"><span data-stu-id="64644-127">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="64644-128">Dans la fenêtre **Ajouter des packages**, sélectionnez « nuget.org » dans la liste déroulante **Source**.</span><span class="sxs-lookup"><span data-stu-id="64644-128">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="64644-129">Entrez « Swashbuckle.AspNetCore » dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="64644-129">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
* <span data-ttu-id="64644-130">Sélectionnez le package « Swashbuckle.AspNetCore » dans le volet de résultats, puis cliquez sur **Ajouter un package**.</span><span class="sxs-lookup"><span data-stu-id="64644-130">Select the "Swashbuckle.AspNetCore" package from the results pane and click **Add Package**</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="64644-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="64644-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="64644-132">Exécutez la commande suivante à partir du **Terminal intégré** :</span><span class="sxs-lookup"><span data-stu-id="64644-132">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="64644-133">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="64644-133">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="64644-134">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="64644-134">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="64644-135">Ajouter et configurer l’intergiciel (middleware) Swagger</span><span class="sxs-lookup"><span data-stu-id="64644-135">Add and configure Swagger middleware</span></span>

<span data-ttu-id="64644-136">Ajoutez le générateur Swagger à la collection de services dans la méthode `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="64644-136">Add the Swagger generator to the services collection in the `Startup.ConfigureServices` method:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="64644-137">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=8-11)]</span><span class="sxs-lookup"><span data-stu-id="64644-137">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=8-11)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="64644-138">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=9-12)]</span><span class="sxs-lookup"><span data-stu-id="64644-138">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=9-12)]</span></span>
::: moniker-end

<span data-ttu-id="64644-139">Importez les espaces de noms suivants à utiliser dans la classe `Info` :</span><span class="sxs-lookup"><span data-stu-id="64644-139">Import the following namespace to use the `Info` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_InfoClassNamespace)]

<span data-ttu-id="64644-140">Dans la méthode `Startup.Configure`, activez l’intergiciel pour traiter le document JSON généré et l’IU Swagger :</span><span class="sxs-lookup"><span data-stu-id="64644-140">In the `Startup.Configure` method, enable the middleware for serving the generated JSON document and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_Configure&highlight=4,8-11)]

<span data-ttu-id="64644-141">Lancez l’application et accédez à `http://localhost:<port>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="64644-141">Launch the app, and navigate to `http://localhost:<port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="64644-142">Le document généré décrivant les points de terminaison s’affiche comme illustré dans la [spécification Swagger (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span><span class="sxs-lookup"><span data-stu-id="64644-142">The generated document describing the endpoints appears as shown in [Swagger specification (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span></span>

<span data-ttu-id="64644-143">L’IU Swagger se trouve à l’adresse `http://localhost:<port>/swagger`.</span><span class="sxs-lookup"><span data-stu-id="64644-143">The Swagger UI can be found at `http://localhost:<port>/swagger`.</span></span> <span data-ttu-id="64644-144">Explorez l’API via l’IU Swagger et incorporez-la dans d’autres programmes.</span><span class="sxs-lookup"><span data-stu-id="64644-144">Explore the API via Swagger UI and incorporate it in other programs.</span></span>

> [!TIP]
> <span data-ttu-id="64644-145">Pour utiliser l’IU Swagger à la racine de l’application (`http://localhost:<port>/`), définissez la propriété `RoutePrefix` sur une chaîne vide :</span><span class="sxs-lookup"><span data-stu-id="64644-145">To serve the Swagger UI at the app's root (`http://localhost:<port>/`), set the `RoutePrefix` property to an empty string:</span></span>
>
> [!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup3.cs?name=snippet_UseSwaggerUI&highlight=4)]

## <a name="customize--extend"></a><span data-ttu-id="64644-146">Personnaliser et étendre</span><span class="sxs-lookup"><span data-stu-id="64644-146">Customize & extend</span></span>

<span data-ttu-id="64644-147">Swagger fournit des options pour documenter le modèle objet et personnaliser l’interface utilisateur en fonction de votre thème.</span><span class="sxs-lookup"><span data-stu-id="64644-147">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="64644-148">Informations sur l’API et description</span><span class="sxs-lookup"><span data-stu-id="64644-148">API info and description</span></span>

<span data-ttu-id="64644-149">L’action de configuration transmise à la méthode `AddSwaggerGen` ajoute des informations comme l’auteur, la licence et la description :</span><span class="sxs-lookup"><span data-stu-id="64644-149">The configuration action passed to the `AddSwaggerGen` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup4.cs?name=snippet_AddSwaggerGen)]

<span data-ttu-id="64644-150">L’IU Swagger affiche les informations de la version :</span><span class="sxs-lookup"><span data-stu-id="64644-150">The Swagger UI displays the version's information:</span></span>

![Interface utilisateur de Swagger avec les informations de version : description, auteur et lien pour en savoir plus](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="64644-152">commentaires XML</span><span class="sxs-lookup"><span data-stu-id="64644-152">XML comments</span></span>

<span data-ttu-id="64644-153">Vous pouvez activer les commentaires XML en adoptant l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="64644-153">XML comments can be enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="64644-154">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="64644-154">Visual Studio</span></span>](#tab/visual-studio-xml/)

* <span data-ttu-id="64644-155">Cliquez avec le bouton droit sur le projet dans **l’Explorateur de solutions**, puis sélectionnez **Propriétés**.</span><span class="sxs-lookup"><span data-stu-id="64644-155">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="64644-156">Cochez la case **Fichier de documentation XML** dans la section **Sortie** sous l’onglet **Générer**.</span><span class="sxs-lookup"><span data-stu-id="64644-156">Check the **XML documentation file** box under the **Output** section of the **Build** tab</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="64644-157">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="64644-157">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)

* <span data-ttu-id="64644-158">Ouvrez la boîte de dialogue **Options du projet** > **Générer** > **Compilateur**.</span><span class="sxs-lookup"><span data-stu-id="64644-158">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="64644-159">Cochez la case **Générer la documentation XML** dans la section **Options générales**.</span><span class="sxs-lookup"><span data-stu-id="64644-159">Check the **Generate xml documentation** box under the **General Options** section</span></span>

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="64644-160">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="64644-160">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)

<span data-ttu-id="64644-161">Ajoutez manuellement l’extrait de code suivant au fichier *.csproj* :</span><span class="sxs-lookup"><span data-stu-id="64644-161">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=2)]

---

<span data-ttu-id="64644-162">L’activation de commentaires XML fournit des informations de débogage sur les membres et types publics non documentés.</span><span class="sxs-lookup"><span data-stu-id="64644-162">Enabling XML comments provides debug information for undocumented public types and members.</span></span> <span data-ttu-id="64644-163">Les membres et types non documentés sont indiqués par le message d’avertissement.</span><span class="sxs-lookup"><span data-stu-id="64644-163">Undocumented types and members are indicated by the warning message.</span></span> <span data-ttu-id="64644-164">Par exemple, le message suivant indique une violation du code d’avertissement 1591 :</span><span class="sxs-lookup"><span data-stu-id="64644-164">For example, the following message indicates a violation of warning code 1591:</span></span>

```text
warning CS1591: Missing XML comment for publicly visible type or member 'TodoController.GetAll()'
```

<span data-ttu-id="64644-165">Supprimez les avertissements en définissant une liste de codes d’avertissement séparée par des points-virgules à ignorer dans le fichier *.csproj* :</span><span class="sxs-lookup"><span data-stu-id="64644-165">Suppress warnings by defining a semicolon-delimited list of warning codes to ignore in the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

<span data-ttu-id="64644-166">Configurez Swagger pour utiliser le fichier XML généré.</span><span class="sxs-lookup"><span data-stu-id="64644-166">Configure Swagger to use the generated XML file.</span></span> <span data-ttu-id="64644-167">Pour les systèmes d’exploitation Linux ou non-Windows, les chemins et les noms de fichiers peuvent respecter la casse.</span><span class="sxs-lookup"><span data-stu-id="64644-167">For Linux or non-Windows operating systems, file names and paths can be case-sensitive.</span></span> <span data-ttu-id="64644-168">Par exemple, un fichier *ToDoApi.XML* est valide sur Windows, mais pas sur CentOS.</span><span class="sxs-lookup"><span data-stu-id="64644-168">For example, a *TodoApi.XML* file is valid on Windows but not CentOS.</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="64644-169">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=30-32)]</span><span class="sxs-lookup"><span data-stu-id="64644-169">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=30-32)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="64644-170">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=31-33)]</span><span class="sxs-lookup"><span data-stu-id="64644-170">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=31-33)]</span></span>
::: moniker-end

<span data-ttu-id="64644-171">Dans le code précédent, la [réflexion](/dotnet/csharp/programming-guide/concepts/reflection) est utilisée pour générer un nom de fichier XML correspondant à celui du projet d’API web.</span><span class="sxs-lookup"><span data-stu-id="64644-171">In the preceding code, [Reflection](/dotnet/csharp/programming-guide/concepts/reflection) is used to build an XML file name matching that of the Web API project.</span></span> <span data-ttu-id="64644-172">Cette approche garantit que le nom de fichier XML généré correspond au nom de projet.</span><span class="sxs-lookup"><span data-stu-id="64644-172">This approach ensures that the generated XML file name matches the project name.</span></span> <span data-ttu-id="64644-173">La propriété [AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory#System_AppContext_BaseDirectory) est utilisée pour construire le chemin du fichier XML.</span><span class="sxs-lookup"><span data-stu-id="64644-173">The [AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory#System_AppContext_BaseDirectory) property is used to construct a path to the XML file.</span></span>

<span data-ttu-id="64644-174">Quand vous ajoutez des commentaires avec trois barres obliques à une action, la description est ajoutée à l’en-tête de section dans l’IU Swagger.</span><span class="sxs-lookup"><span data-stu-id="64644-174">Adding triple-slash comments to an action enhances the Swagger UI by adding the description to the section header.</span></span> <span data-ttu-id="64644-175">Ajoutez un élément [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) au dessus de l’action `Delete` :</span><span class="sxs-lookup"><span data-stu-id="64644-175">Add a [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) element above the `Delete` action:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Delete&highlight=1-3)]

<span data-ttu-id="64644-176">L’IU Swagger affiche le texte interne de l’élément `<summary>` du code précédent :</span><span class="sxs-lookup"><span data-stu-id="64644-176">The Swagger UI displays the inner text of the preceding code's `<summary>` element:</span></span>

![Interface utilisateur de Swagger affichant le commentaire XML « Deletes a specific TodoItem. »](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="64644-179">L’IU est définie par le schéma JSON généré :</span><span class="sxs-lookup"><span data-stu-id="64644-179">The UI is driven by the generated JSON schema:</span></span>

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

<span data-ttu-id="64644-180">Ajoutez un élément [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) à la documentation de la méthode de l’action `Create`.</span><span class="sxs-lookup"><span data-stu-id="64644-180">Add a [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) element to the `Create` action method documentation.</span></span> <span data-ttu-id="64644-181">Il complète les informations spécifiées dans l’élément `<summary>` et fournit une IU Swagger plus robuste.</span><span class="sxs-lookup"><span data-stu-id="64644-181">It supplements information specified in the `<summary>` element and provides a more robust Swagger UI.</span></span> <span data-ttu-id="64644-182">Le contenu de l’élément `<remarks>` peut être du texte, du code JSON ou du code XML.</span><span class="sxs-lookup"><span data-stu-id="64644-182">The `<remarks>` element content can consist of text, JSON, or XML.</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="64644-183">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]</span><span class="sxs-lookup"><span data-stu-id="64644-183">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="64644-184">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]</span><span class="sxs-lookup"><span data-stu-id="64644-184">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]</span></span>
::: moniker-end

<span data-ttu-id="64644-185">Notez les améliorations de l’IU avec ces commentaires supplémentaires :</span><span class="sxs-lookup"><span data-stu-id="64644-185">Notice the UI enhancements with these additional comments:</span></span>

![Interface utilisateur de Swagger avec des commentaires supplémentaires affichés](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="64644-187">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="64644-187">Data annotations</span></span>

<span data-ttu-id="64644-188">Décorez le modèle avec des attributs, présents dans l’espace de noms [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations), pour gérer les composants de l’IU Swagger.</span><span class="sxs-lookup"><span data-stu-id="64644-188">Decorate the model with attributes, found in the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="64644-189">Ajoutez l’attribut `[Required]` à la propriété `Name` de la classe `TodoItem` :</span><span class="sxs-lookup"><span data-stu-id="64644-189">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="64644-190">La présence de cet attribut change le comportement de l’interface utilisateur et modifie le schéma JSON sous-jacent :</span><span class="sxs-lookup"><span data-stu-id="64644-190">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

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

<span data-ttu-id="64644-191">Ajoutez l’attribut `[Produces("application/json")]` au contrôleur d’API.</span><span class="sxs-lookup"><span data-stu-id="64644-191">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="64644-192">Son objectif est de déclarer que les actions du contrôleur prennent en charge une réponse dont le type de contenu est *application/json* :</span><span class="sxs-lookup"><span data-stu-id="64644-192">Its purpose is to declare that the controller's actions support a response content type of *application/json*:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="64644-193">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="64644-193">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="64644-194">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="64644-194">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]</span></span>
::: moniker-end

<span data-ttu-id="64644-195">La zone de liste déroulante **Response Content Type** permet de sélectionner ce type de contenu comme valeur par défaut pour les actions GET du contrôleur :</span><span class="sxs-lookup"><span data-stu-id="64644-195">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Interface utilisateur de Swagger avec le type de contenu de réponse par défaut](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="64644-197">À mesure que l’utilisation des annotations de données dans l’API web augmente, l’interface utilisateur et les pages d’aide de l’API deviennent de plus en plus descriptives et utiles.</span><span class="sxs-lookup"><span data-stu-id="64644-197">As the usage of data annotations in the Web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describe-response-types"></a><span data-ttu-id="64644-198">Décrire des types de réponse</span><span class="sxs-lookup"><span data-stu-id="64644-198">Describe response types</span></span>

<span data-ttu-id="64644-199">Les développeurs consommateurs s’intéressent surtout à ce qui est retourné, en particulier les types de réponse et les codes d’erreur (s’ils ne sont pas standard).</span><span class="sxs-lookup"><span data-stu-id="64644-199">Consuming developers are most concerned with what's returned&mdash;specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="64644-200">Les types de réponse et les codes d’erreur sont décrits dans les commentaires XML et les annotations de données.</span><span class="sxs-lookup"><span data-stu-id="64644-200">The response types and error codes are denoted in the XML comments and data annotations.</span></span>

<span data-ttu-id="64644-201">L’action `Create` retourne un code d’état HTTP 201 en cas de réussite.</span><span class="sxs-lookup"><span data-stu-id="64644-201">The `Create` action returns an HTTP 201 status code on success.</span></span> <span data-ttu-id="64644-202">Un code d’état HTTP 400 est retourné quand le corps de la demande postée est null.</span><span class="sxs-lookup"><span data-stu-id="64644-202">An HTTP 400 status code is returned when the posted request body is null.</span></span> <span data-ttu-id="64644-203">Sans documentation appropriée dans l’interface utilisateur de Swagger, le consommateur n’a pas connaissance de ces résultats attendus.</span><span class="sxs-lookup"><span data-stu-id="64644-203">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="64644-204">Pour résoudre ce problème, ajoutez les lignes en surbrillance de l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="64644-204">Fix that problem by adding the highlighted lines in the following example:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="64644-205">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]</span><span class="sxs-lookup"><span data-stu-id="64644-205">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="64644-206">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]</span><span class="sxs-lookup"><span data-stu-id="64644-206">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]</span></span>
::: moniker-end

<span data-ttu-id="64644-207">L’interface utilisateur de Swagger documente maintenant clairement les codes de réponse HTTP attendus :</span><span class="sxs-lookup"><span data-stu-id="64644-207">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![Interface utilisateur de Swagger affichant la description de la classe de réponse POST « Returns the newly created Todo item » et « 400 - If the item is null » pour le code d’état et la raison sous Response Messages](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customize-the-ui"></a><span data-ttu-id="64644-209">Personnaliser l’IU</span><span class="sxs-lookup"><span data-stu-id="64644-209">Customize the UI</span></span>

<span data-ttu-id="64644-210">L’IU de stock est fonctionnelle et conviviale.</span><span class="sxs-lookup"><span data-stu-id="64644-210">The stock UI is both functional and presentable.</span></span> <span data-ttu-id="64644-211">Toutefois, les pages de documentation d’API doivent représenter votre marque ou thème.</span><span class="sxs-lookup"><span data-stu-id="64644-211">However, API documentation pages should represent your brand or theme.</span></span> <span data-ttu-id="64644-212">La personnalisation des composants Swashbuckle nécessite d’ajouter les ressources qui traitent les fichiers statiques et de générer la structure de dossiers pour héberger ces fichiers.</span><span class="sxs-lookup"><span data-stu-id="64644-212">Branding the Swashbuckle components requires adding the resources to serve static files and building the folder structure to host those files.</span></span>

<span data-ttu-id="64644-213">Si vous ciblez le .NET Framework ou .NET Core 1.x, ajoutez le package NuGet [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) au projet :</span><span class="sxs-lookup"><span data-stu-id="64644-213">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="64644-214">Le package NuGet précédent est déjà installé si vous ciblez .NET Core 2.x et que vous utilisez le [métapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="64644-214">The preceding NuGet package is already installed if targeting .NET Core 2.x and using the [metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="64644-215">Activez l’intergiciel des fichiers statiques :</span><span class="sxs-lookup"><span data-stu-id="64644-215">Enable the static files middleware:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_Configure&highlight=3)]

<span data-ttu-id="64644-216">Faites l’acquisition du contenu du dossier *dist* à partir du [dépôt GitHub de l’interface utilisateur de Swagger](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span><span class="sxs-lookup"><span data-stu-id="64644-216">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span></span> <span data-ttu-id="64644-217">Ce dossier contient les ressources nécessaires pour la page de l’interface utilisateur de Swagger.</span><span class="sxs-lookup"><span data-stu-id="64644-217">This folder contains the necessary assets for the Swagger UI page.</span></span>

<span data-ttu-id="64644-218">Créez un dossier *wwwroot/swagger/ui* et copiez dedans le contenu du dossier *dist*.</span><span class="sxs-lookup"><span data-stu-id="64644-218">Create a *wwwroot/swagger/ui* folder, and copy into it the contents of the *dist* folder.</span></span>

<span data-ttu-id="64644-219">Créez un fichier *custom.css* dans *wwwroot/swagger/ui* avec le code CSS suivant pour personnaliser l’en-tête de page :</span><span class="sxs-lookup"><span data-stu-id="64644-219">Create a *custom.css* file, in *wwwroot/swagger/ui*, with the following CSS to customize the page header:</span></span>

[!code-css[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/wwwroot/swagger/ui/custom.css)]

<span data-ttu-id="64644-220">Référencez *custom.css* dans le fichier *index.html*, à la suite de n’importe quel autre fichier CSS :</span><span class="sxs-lookup"><span data-stu-id="64644-220">Reference *custom.css* in the *index.html* file, after any other CSS files:</span></span>

[!code-html[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/wwwroot/swagger/ui/index.html?name=snippet_SwaggerUiCss&highlight=3)]

<span data-ttu-id="64644-221">Accédez à la page *index.html* à l’adresse `http://localhost:<port>/swagger/ui/index.html`.</span><span class="sxs-lookup"><span data-stu-id="64644-221">Browse to the *index.html* page at `http://localhost:<port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="64644-222">Entrez `http://localhost:<port>/swagger/v1/swagger.json` dans la zone de texte de l’en-tête, puis cliquez sur le bouton **Explorer**.</span><span class="sxs-lookup"><span data-stu-id="64644-222">Enter `http://localhost:<port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="64644-223">La page résultante ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="64644-223">The resulting page looks as follows:</span></span>

![Interface utilisateur de Swagger avec titre d’en-tête personnalisé](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="64644-225">Vous pouvez faire encore beaucoup avec cette page.</span><span class="sxs-lookup"><span data-stu-id="64644-225">There's much more you can do with the page.</span></span> <span data-ttu-id="64644-226">Pour découvrir toutes les fonctionnalités pour les ressources d’interface utilisateur, accédez au [dépôt GitHub de l’interface utilisateur de Swagger](https://github.com/swagger-api/swagger-ui).</span><span class="sxs-lookup"><span data-stu-id="64644-226">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>
