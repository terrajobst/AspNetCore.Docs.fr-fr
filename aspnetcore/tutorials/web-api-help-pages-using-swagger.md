---
title: "Pages d’aide d’API web ASP.NET Core à l’aide de Swagger"
author: spboyer
description: "Ce didacticiel montre comment ajouter Swagger pour générer des pages d’aide et de documentation à une application d’API web."
ms.author: spboyer
manager: wpickett
ms.date: 09/01/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: 302199bb0b32d4f6610e04455bb28372095e9873
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
# <a name="aspnet-core-web-api-help-pages-using-swagger"></a><span data-ttu-id="76307-103">Pages d’aide d’API web ASP.NET Core à l’aide de Swagger</span><span class="sxs-lookup"><span data-stu-id="76307-103">ASP.NET Core Web API Help Pages using Swagger</span></span>

<a name="web-api-help-pages-using-swagger"></a>

<span data-ttu-id="76307-104">De [Shayne Boyer](https://twitter.com/spboyer) et [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="76307-104">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="76307-105">Comprendre les différentes méthodes d’une API peut être un défi pour un développeur lors de la création d’une application consommatrice.</span><span class="sxs-lookup"><span data-stu-id="76307-105">Understanding the various methods of an API can be a challenge for a developer when building a consuming application.</span></span>

<span data-ttu-id="76307-106">Générer des pages d’aide et de documentation utiles pour votre API web à l’aide de [Swagger](https://swagger.io/) avec l’implémentation .NET Core de [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) est un jeu d’enfant : il vous suffit d’ajouter deux packages NuGet et de modifier le fichier *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="76307-106">Generating good documentation and help pages for your Web API, using [Swagger](https://swagger.io/) with the .NET Core implementation [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore), is as easy as adding a couple of NuGet packages and modifying the *Startup.cs*.</span></span>

* <span data-ttu-id="76307-107">[Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) est un projet open source pour la génération de documents Swagger pour des API web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="76307-107">[Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) is an open source project for generating Swagger documents for ASP.NET Core Web APIs.</span></span>

* <span data-ttu-id="76307-108">[Swagger](https://swagger.io/) est une représentation lisible par la machine d’une API RESTful qui permet de bénéficier de la prise en charge de documentation interactive, de la génération de SDK client et de la fonctionnalité de découverte.</span><span class="sxs-lookup"><span data-stu-id="76307-108">[Swagger](https://swagger.io/) is a machine-readable representation of a RESTful API that enables support for interactive documentation, client SDK generation, and discoverability.</span></span>

<span data-ttu-id="76307-109">Ce didacticiel s’appuie sur l’exemple [Création de votre première API web avec ASP.NET Core MVC et Visual Studio](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="76307-109">This tutorial builds on the sample on [Building Your First Web API with ASP.NET Core MVC and Visual Studio](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="76307-110">Si vous souhaitez suivre ce didacticiel, téléchargez l’exemple à l’adresse [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample).</span><span class="sxs-lookup"><span data-stu-id="76307-110">If you'd like to follow along, download the sample at [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample).</span></span>

## <a name="getting-started"></a><span data-ttu-id="76307-111">Prise en main</span><span class="sxs-lookup"><span data-stu-id="76307-111">Getting Started</span></span>

<span data-ttu-id="76307-112">Swashbuckle compte trois composants principaux :</span><span class="sxs-lookup"><span data-stu-id="76307-112">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="76307-113">`Swashbuckle.AspNetCore.Swagger` : un modèle objet Swagger et un intergiciel (middleware) pour exposer des objets `SwaggerDocument` en tant que points de terminaison JSON.</span><span class="sxs-lookup"><span data-stu-id="76307-113">`Swashbuckle.AspNetCore.Swagger`: a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="76307-114">`Swashbuckle.AspNetCore.SwaggerGen` : un générateur Swagger qui crée des objets `SwaggerDocument` directement à partir de vos itinéraires, contrôleurs et modèles.</span><span class="sxs-lookup"><span data-stu-id="76307-114">`Swashbuckle.AspNetCore.SwaggerGen`: a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="76307-115">Il est généralement associé à l’intergiciel de point de terminaison Swagger pour exposer automatiquement Swagger JSON.</span><span class="sxs-lookup"><span data-stu-id="76307-115">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="76307-116">`Swashbuckle.AspNetCore.SwaggerUI` : une version incorporée de l’outil d’interface utilisateur Swagger qui interprète Swagger JSON afin de générer une expérience enrichie et personnalisable pour la description de la fonctionnalité de l’API web.</span><span class="sxs-lookup"><span data-stu-id="76307-116">`Swashbuckle.AspNetCore.SwaggerUI`: an embedded version of the Swagger UI tool which interprets Swagger JSON to build a rich, customizable experience for describing the Web API functionality.</span></span> <span data-ttu-id="76307-117">Il inclut des ateliers de test intégrés pour les méthodes publiques.</span><span class="sxs-lookup"><span data-stu-id="76307-117">It includes built-in test harnesses for the public methods.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="76307-118">Packages NuGet</span><span class="sxs-lookup"><span data-stu-id="76307-118">NuGet Packages</span></span>

<span data-ttu-id="76307-119">Vous pouvez ajouter Swashbuckle en adoptant l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="76307-119">Swashbuckle can be added with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="76307-120">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="76307-120">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="76307-121">À partir de la fenêtre **Console du Gestionnaire de package** :</span><span class="sxs-lookup"><span data-stu-id="76307-121">From the **Package Manager Console** window:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* <span data-ttu-id="76307-122">À partir de la boîte de dialogue **Gérer les packages NuGet** :</span><span class="sxs-lookup"><span data-stu-id="76307-122">From the **Manage NuGet Packages** dialog:</span></span>

     * <span data-ttu-id="76307-123">Cliquez avec le bouton droit sur votre projet dans **l’Explorateur de solutions** > **Gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="76307-123">Right-click your project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
     * <span data-ttu-id="76307-124">Affectez la valeur « nuget.org » à **Source du package**.</span><span class="sxs-lookup"><span data-stu-id="76307-124">Set the **Package source** to "nuget.org"</span></span>
     * <span data-ttu-id="76307-125">Entrez « Swashbuckle.AspNetCore » dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="76307-125">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
     * <span data-ttu-id="76307-126">Sélectionnez le package « Swashbuckle.AspNetCore » sous l’onglet **Parcourir** et cliquez sur **Installer**.</span><span class="sxs-lookup"><span data-stu-id="76307-126">Select the "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="76307-127">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="76307-127">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="76307-128">Cliquez avec le bouton droit sur le dossier *Packages* dans **Panneau Solutions** > **Ajouter des packages**.</span><span class="sxs-lookup"><span data-stu-id="76307-128">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="76307-129">Dans la fenêtre **Ajouter des packages**, sélectionnez « nuget.org » dans la liste déroulante **Source**.</span><span class="sxs-lookup"><span data-stu-id="76307-129">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="76307-130">Entrez Swashbuckle.AspNetCore dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="76307-130">Enter Swashbuckle.AspNetCore in the search box</span></span>
* <span data-ttu-id="76307-131">Sélectionnez le package Swashbuckle.AspNetCore dans le volet de résultats, puis cliquez sur **Ajouter un package**.</span><span class="sxs-lookup"><span data-stu-id="76307-131">Select the Swashbuckle.AspNetCore package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="76307-132">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="76307-132">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="76307-133">Exécutez la commande suivante à partir du **Terminal intégré** :</span><span class="sxs-lookup"><span data-stu-id="76307-133">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="76307-134">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="76307-134">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="76307-135">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="76307-135">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-to-the-middleware"></a><span data-ttu-id="76307-136">Ajouter et configurer Swagger à l’intergiciel</span><span class="sxs-lookup"><span data-stu-id="76307-136">Add and configure Swagger to the middleware</span></span>

<span data-ttu-id="76307-137">Ajoutez le générateur Swagger à la collection de services dans la méthode `ConfigureServices` de *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="76307-137">Add the Swagger generator to the services collection in the `ConfigureServices` method of *Startup.cs*:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]

<span data-ttu-id="76307-138">Ajoutez l’instruction using suivante pour la classe `Info` :</span><span class="sxs-lookup"><span data-stu-id="76307-138">Add the following using statement for the `Info` class:</span></span>

```csharp
using Swashbuckle.AspNetCore.Swagger;
```

<span data-ttu-id="76307-139">Dans la méthode `Configure` de *Startup.cs*, activez l’intergiciel pour traiter le document JSON généré et SwaggerUI :</span><span class="sxs-lookup"><span data-stu-id="76307-139">In the `Configure` method of *Startup.cs*, enable the middleware for serving the generated JSON document and the SwaggerUI:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]

<span data-ttu-id="76307-140">Lancez l’application et accédez à `http://localhost:<random_port>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="76307-140">Launch the app, and navigate to `http://localhost:<random_port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="76307-141">Le document généré décrivant les points de terminaison s’affiche.</span><span class="sxs-lookup"><span data-stu-id="76307-141">The generated document describing the endpoints appears.</span></span>

<span data-ttu-id="76307-142">**Remarque :** Microsoft Edge, Google Chrome et Firefox affichent les documents JSON en mode natif.</span><span class="sxs-lookup"><span data-stu-id="76307-142">**Note:** Microsoft Edge, Google Chrome, and Firefox display JSON documents natively.</span></span> <span data-ttu-id="76307-143">Il existe des extensions de Chrome qui mettent en forme le document afin d’en faciliter la lecture.</span><span class="sxs-lookup"><span data-stu-id="76307-143">There are extensions for Chrome that format the document for easier reading.</span></span> <span data-ttu-id="76307-144">*L’exemple suivant est réduit par souci de concision.*</span><span class="sxs-lookup"><span data-stu-id="76307-144">*The following example is reduced for brevity.*</span></span>

```json
{
   "swagger": "2.0",
   "info": {
       "version": "v1",
       "title": "API V1"
   },
   "basePath": "/",
   "paths": {
       "/api/Todo": {
           "get": {
               "tags": [
                   "Todo"
               ],
               "operationId": "ApiTodoGet",
               "consumes": [],
               "produces": [
                   "text/plain",
                   "application/json",
                   "text/json"
               ],
               "responses": {
                   "200": {
                       "description": "Success",
                       "schema": {
                           "type": "array",
                           "items": {
                               "$ref": "#/definitions/TodoItem"
                           }
                       }
                   }
                }
           },
           "post": {
               ...
           }
       },
       "/api/Todo/{id}": {
           "get": {
               ...
           },
           "put": {
               ...
           },
           "delete": {
               ...
   },
   "definitions": {
       "TodoItem": {
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
   "securityDefinitions": {}
}
```

<span data-ttu-id="76307-145">Ce document pilote l’interface utilisateur de Swagger, qui peut être affichée en accédant à `http://localhost:<random_port>/swagger` :</span><span class="sxs-lookup"><span data-stu-id="76307-145">This document drives the Swagger UI, which can be viewed by navigating to `http://localhost:<random_port>/swagger`:</span></span>

![Interface utilisateur de Swagger](web-api-help-pages-using-swagger/_static/swagger-ui.png)

<span data-ttu-id="76307-147">Chaque méthode d’action publique dans `TodoController` peut être testée à partir de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="76307-147">Each public action method in `TodoController` can be tested from the UI.</span></span> <span data-ttu-id="76307-148">Cliquez sur un nom de méthode pour développer la section.</span><span class="sxs-lookup"><span data-stu-id="76307-148">Click a method name to expand the section.</span></span> <span data-ttu-id="76307-149">Ajoutez tous les paramètres nécessaires, puis cliquez sur « Try it out! ».</span><span class="sxs-lookup"><span data-stu-id="76307-149">Add any necessary parameters, and click "Try it out!".</span></span>

![Exemple de test GET Swagger](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

## <a name="customization--extensibility"></a><span data-ttu-id="76307-151">Personnalisation et extensibilité</span><span class="sxs-lookup"><span data-stu-id="76307-151">Customization & Extensibility</span></span>

<span data-ttu-id="76307-152">Swagger fournit des options pour documenter le modèle objet et personnaliser l’interface utilisateur en fonction de votre thème.</span><span class="sxs-lookup"><span data-stu-id="76307-152">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="76307-153">Informations sur l’API et description</span><span class="sxs-lookup"><span data-stu-id="76307-153">API Info and Description</span></span>

<span data-ttu-id="76307-154">Vous pouvez utiliser l’action de configuration transmise à la méthode `AddSwaggerGen` pour ajouter des informations telles que l’auteur, la licence et la description :</span><span class="sxs-lookup"><span data-stu-id="76307-154">The configuration action passed to the `AddSwaggerGen` method can be used to add information such as the author, license, and description:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?range=20-30,36)]

<span data-ttu-id="76307-155">L’image suivante représente l’interface utilisateur de Swagger montrant les informations de version :</span><span class="sxs-lookup"><span data-stu-id="76307-155">The following image depicts the Swagger UI displaying the version information:</span></span>

![Interface utilisateur de Swagger avec les informations de version : description, auteur et lien pour en savoir plus](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="76307-157">Commentaires XML</span><span class="sxs-lookup"><span data-stu-id="76307-157">XML Comments</span></span>

<span data-ttu-id="76307-158">Vous pouvez activer les commentaires XML en adoptant l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="76307-158">XML comments can be enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="76307-159">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="76307-159">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="76307-160">Cliquez avec le bouton droit sur le projet dans **l’Explorateur de solutions**, puis sélectionnez **Propriétés**.</span><span class="sxs-lookup"><span data-stu-id="76307-160">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="76307-161">Cochez la zone **Fichier de documentation XML** dans la section **Sortie** de l’onglet **Générer** :</span><span class="sxs-lookup"><span data-stu-id="76307-161">Check the **XML documentation file** box under the **Output** section of the **Build** tab:</span></span>

![Onglet Générer des propriétés du projet](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="76307-163">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="76307-163">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="76307-164">Ouvrez la boîte de dialogue **Options du projet** > **Générer** > **Compilateur**.</span><span class="sxs-lookup"><span data-stu-id="76307-164">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="76307-165">Cochez la zone **Générer la documentation XML** dans la section **Options générales** :</span><span class="sxs-lookup"><span data-stu-id="76307-165">Check the **Generate xml documentation** box under the **General Options** section:</span></span>

![Section Options générales des options du projet](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="76307-167">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="76307-167">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="76307-168">Ajoutez manuellement l’extrait de code suivant au fichier *.csproj* :</span><span class="sxs-lookup"><span data-stu-id="76307-168">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/TodoApi.csproj?range=7-9)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="76307-169">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="76307-169">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="76307-170">Consultez Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="76307-170">See Visual Studio Code.</span></span>

---

<span data-ttu-id="76307-171">L’activation de commentaires XML fournit des informations de débogage sur les membres et types publics non documentés.</span><span class="sxs-lookup"><span data-stu-id="76307-171">Enabling XML comments provides debug information for undocumented public types and members.</span></span> <span data-ttu-id="76307-172">Les types et les membres non documentés sont signalés par le message d’avertissement : *Commentaire XML manquant pour le type ou le membre visible publiquement* .</span><span class="sxs-lookup"><span data-stu-id="76307-172">Undocumented types and members are indicated by the warning message: *Missing XML comment for publicly visible type or member*.</span></span>

<span data-ttu-id="76307-173">Configurez Swagger pour utiliser le fichier XML généré.</span><span class="sxs-lookup"><span data-stu-id="76307-173">Configure Swagger to use the generated XML file.</span></span> <span data-ttu-id="76307-174">Pour les systèmes d’exploitation Linux ou autres que Windows, les chemins et les noms de fichiers peuvent respecter la casse.</span><span class="sxs-lookup"><span data-stu-id="76307-174">For Linux or non-Windows operating systems, file names and paths can be case sensitive.</span></span> <span data-ttu-id="76307-175">Par exemple, un fichier *ToDoApi.XML* peut exister sur Windows, mais pas sur CentOS.</span><span class="sxs-lookup"><span data-stu-id="76307-175">For example, a *ToDoApi.XML* file would be found on Windows but not CentOS.</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_ConfigureServices&highlight=20-22)]

<span data-ttu-id="76307-176">Dans le code précédent, `ApplicationBasePath` obtient le chemin de base de l’application.</span><span class="sxs-lookup"><span data-stu-id="76307-176">In the preceding code, `ApplicationBasePath` gets the base path of the app.</span></span> <span data-ttu-id="76307-177">Le chemin de base sert à trouver le fichier de commentaires XML.</span><span class="sxs-lookup"><span data-stu-id="76307-177">The base path is used to locate the XML comments file.</span></span> <span data-ttu-id="76307-178">*TodoApi.xml* fonctionne uniquement pour cet exemple, étant donné que le nom du fichier de commentaires XML généré est basé sur le nom de l’application.</span><span class="sxs-lookup"><span data-stu-id="76307-178">*TodoApi.xml* only works for this example, since the name of the generated XML comments file is based on the application name.</span></span>

<span data-ttu-id="76307-179">Le fait d’ajouter les commentaires à trois barres obliques à la méthode améliore l’interface utilisateur de Swagger en ajoutant la description à l’en-tête de section :</span><span class="sxs-lookup"><span data-stu-id="76307-179">Adding the triple-slash comments to the method enhances the Swagger UI by adding the description to the section header:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete&highlight=2)]

![Interface utilisateur de Swagger affichant le commentaire XML « Deletes a specific TodoItem. »](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="76307-182">L’interface utilisateur est pilotée par le fichier JSON généré, qui contient également ces commentaires :</span><span class="sxs-lookup"><span data-stu-id="76307-182">The UI is driven by the generated JSON file, which also contains these comments:</span></span>

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

<span data-ttu-id="76307-183">Ajoutez une balise [<remarks>](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) à la documentation de méthode d’action `Create`.</span><span class="sxs-lookup"><span data-stu-id="76307-183">Add a [<remarks>](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) tag to the `Create` action method documentation.</span></span> <span data-ttu-id="76307-184">Elle complète les informations spécifiées dans la balise `<summary>` et fournit une interface utilisateur de Swagger plus robuste.</span><span class="sxs-lookup"><span data-stu-id="76307-184">It supplements information specified in the `<summary>` tag and provides a more robust Swagger UI.</span></span> <span data-ttu-id="76307-185">Le contenu de la balise `<remarks>` peut être constitué de texte, JSON ou XML.</span><span class="sxs-lookup"><span data-stu-id="76307-185">The `<remarks>` tag content can consist of text, JSON, or XML.</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

<span data-ttu-id="76307-186">Notez les améliorations de l’interface utilisateur avec ces commentaires supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="76307-186">Notice the UI enhancements with these additional comments.</span></span>

![Interface utilisateur de Swagger avec des commentaires supplémentaires affichés](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="76307-188">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="76307-188">Data Annotations</span></span>

<span data-ttu-id="76307-189">Décorez le modèle avec des attributs, disponibles dans `System.ComponentModel.DataAnnotations`, afin d’optimiser les composants de l’interface utilisateur de Swagger.</span><span class="sxs-lookup"><span data-stu-id="76307-189">Decorate the model with attributes, found in `System.ComponentModel.DataAnnotations`, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="76307-190">Ajoutez l’attribut `[Required]` à la propriété `Name` de la classe `TodoItem` :</span><span class="sxs-lookup"><span data-stu-id="76307-190">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="76307-191">La présence de cet attribut change le comportement de l’interface utilisateur et modifie le schéma JSON sous-jacent :</span><span class="sxs-lookup"><span data-stu-id="76307-191">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

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

<span data-ttu-id="76307-192">Ajoutez l’attribut `[Produces("application/json")]` au contrôleur d’API.</span><span class="sxs-lookup"><span data-stu-id="76307-192">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="76307-193">Son objectif est de déclarer que les actions du contrôleur prennent en charge le retour d’un type de contenu *application/json* :</span><span class="sxs-lookup"><span data-stu-id="76307-193">Its purpose is to declare that the controller's actions support a return a content type of *application/json*:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]

<span data-ttu-id="76307-194">La zone de liste déroulante **Response Content Type** permet de sélectionner ce type de contenu comme valeur par défaut pour les actions GET du contrôleur :</span><span class="sxs-lookup"><span data-stu-id="76307-194">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Interface utilisateur de Swagger avec le type de contenu de réponse par défaut](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="76307-196">À mesure que l’utilisation des annotations de données dans l’API web augmente, l’interface utilisateur et les pages d’aide de l’API deviennent de plus en plus descriptives et utiles.</span><span class="sxs-lookup"><span data-stu-id="76307-196">As the usage of data annotations in the Web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describing-response-types"></a><span data-ttu-id="76307-197">Description des types de réponse</span><span class="sxs-lookup"><span data-stu-id="76307-197">Describing Response Types</span></span>

<span data-ttu-id="76307-198">Les développeurs consommateurs se soucient davantage de ce qui est retourné, plus spécifiquement par les types de réponse et les codes d’erreur (s’ils ne sont pas standard).</span><span class="sxs-lookup"><span data-stu-id="76307-198">Consuming developers are most concerned with what is returned &mdash; specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="76307-199">Ceux-ci sont gérés dans les annotations de données et les commentaires XML.</span><span class="sxs-lookup"><span data-stu-id="76307-199">These are handled in the XML comments and data annotations.</span></span>

<span data-ttu-id="76307-200">L’action `Create` retourne `201 Created` en cas de réussite ou `400 Bad Request` quand le corps de la requête envoyée est null.</span><span class="sxs-lookup"><span data-stu-id="76307-200">The `Create` action returns `201 Created` on success or `400 Bad Request` when the posted request body is null.</span></span> <span data-ttu-id="76307-201">Sans documentation appropriée dans l’interface utilisateur de Swagger, le consommateur n’a pas connaissance de ces résultats attendus.</span><span class="sxs-lookup"><span data-stu-id="76307-201">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="76307-202">Pour résoudre ce problème, il faut ajouter les lignes en surbrillance dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="76307-202">That problem is fixed by adding the highlighted lines in the following example:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

<span data-ttu-id="76307-203">L’interface utilisateur de Swagger documente maintenant clairement les codes de réponse HTTP attendus :</span><span class="sxs-lookup"><span data-stu-id="76307-203">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![Interface utilisateur de Swagger affichant la description de la classe de réponse POST « Returns the newly created Todo item » et « 400 - If the item is null » pour le code d’état et la raison sous Response Messages](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customizing-the-ui"></a><span data-ttu-id="76307-205">Personnalisation de l’interface utilisateur</span><span class="sxs-lookup"><span data-stu-id="76307-205">Customizing the UI</span></span>

<span data-ttu-id="76307-206">L’interface utilisateur par défaut est fonctionnelle et présentable, mais lors de la création des pages de documentation de votre API, vous souhaiterez probablement qu’elle représente votre marque ou votre thème.</span><span class="sxs-lookup"><span data-stu-id="76307-206">The stock UI is both functional and presentable; however, when building documentation pages for your API, you want it to represent your brand or theme.</span></span> <span data-ttu-id="76307-207">Pour accomplir cette tâche avec les composants Swashbuckle, vous devez ajouter les ressources de prise en charge des fichiers statiques, puis générer la structure de dossiers pour héberger ces fichiers.</span><span class="sxs-lookup"><span data-stu-id="76307-207">Accomplishing that task with the Swashbuckle components requires adding the resources to serve static files and then building the folder structure to host those files.</span></span>

<span data-ttu-id="76307-208">Si vous ciblez le .NET Framework, ajoutez le package NuGet `Microsoft.AspNetCore.StaticFiles` au projet :</span><span class="sxs-lookup"><span data-stu-id="76307-208">If targeting .NET Framework, add the `Microsoft.AspNetCore.StaticFiles` NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="76307-209">Activez l’intergiciel des fichiers statiques :</span><span class="sxs-lookup"><span data-stu-id="76307-209">Enable the static files middleware:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_Configure&highlight=3)]

<span data-ttu-id="76307-210">Faites l’acquisition du contenu du dossier *dist* à partir du [dépôt GitHub de l’interface utilisateur de Swagger](https://github.com/swagger-api/swagger-ui/tree/2.x/dist).</span><span class="sxs-lookup"><span data-stu-id="76307-210">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/2.x/dist).</span></span> <span data-ttu-id="76307-211">Ce dossier contient les ressources nécessaires pour la page de l’interface utilisateur de Swagger.</span><span class="sxs-lookup"><span data-stu-id="76307-211">This folder contains the necessary assets for the Swagger UI page.</span></span>

<span data-ttu-id="76307-212">Créez un dossier *wwwroot/swagger/ui* et copiez dedans le contenu du dossier *dist*.</span><span class="sxs-lookup"><span data-stu-id="76307-212">Create a *wwwroot/swagger/ui* folder, and copy into it the contents of the *dist* folder.</span></span>

<span data-ttu-id="76307-213">Créez un fichier *wwwroot/swagger/ui/css/custom.css* avec le code CSS suivant pour personnaliser l’en-tête de page :</span><span class="sxs-lookup"><span data-stu-id="76307-213">Create a *wwwroot/swagger/ui/css/custom.css* file with the following CSS to customize the page header:</span></span>

[!code-css[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/css/custom.css)]

<span data-ttu-id="76307-214">Référencez *custom.css* dans le fichier *index.html* :</span><span class="sxs-lookup"><span data-stu-id="76307-214">Reference *custom.css* in the *index.html* file:</span></span>

[!code-html[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/index.html?range=14)]

<span data-ttu-id="76307-215">Accédez à la page *index.html* à l’adresse `http://localhost:<random_port>/swagger/ui/index.html`.</span><span class="sxs-lookup"><span data-stu-id="76307-215">Browse to the *index.html* page at `http://localhost:<random_port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="76307-216">Entrez `http://localhost:<random_port>/swagger/v1/swagger.json` dans la zone de texte de l’en-tête, puis cliquez sur le bouton **Explorer**.</span><span class="sxs-lookup"><span data-stu-id="76307-216">Enter `http://localhost:<random_port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="76307-217">La page résultante ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="76307-217">The resulting page looks as follows:</span></span>

![Interface utilisateur de Swagger avec titre d’en-tête personnalisé](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="76307-219">Vous pouvez faire encore beaucoup avec cette page.</span><span class="sxs-lookup"><span data-stu-id="76307-219">There's much more you can do with the page.</span></span> <span data-ttu-id="76307-220">Pour découvrir toutes les fonctionnalités pour les ressources d’interface utilisateur, accédez au [dépôt GitHub de l’interface utilisateur de Swagger](https://github.com/swagger-api/swagger-ui).</span><span class="sxs-lookup"><span data-stu-id="76307-220">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>
