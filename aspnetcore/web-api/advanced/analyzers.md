---
title: Utiliser les analyseurs d’API web
author: pranavkm
description: Découvrez plus d’informations sur les analyseurs d’API web dans Microsoft.AspNetCore.Mvc.Api.Analyzers.
monikerRange: '>= aspnetcore-2.2'
ms.author: pranavkm
ms.custom: mvc
ms.date: 12/14/2018
uid: web-api/advanced/analyzers
ms.openlocfilehash: 7558552586d3056c43d8bfd9ef74cbcb3396726f
ms.sourcegitcommit: 6548c19f345850ee22b50f7ef9fca732895d9e08
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53425092"
---
# <a name="use-web-api-analyzers"></a><span data-ttu-id="20d2c-103">Utiliser les analyseurs d’API web</span><span class="sxs-lookup"><span data-stu-id="20d2c-103">Use web API analyzers</span></span>

<span data-ttu-id="20d2c-104">ASP.NET Core 2.2 et les versions ultérieures introduisent le package NuGet [Microsoft.AspNetCore.Mvc.Api.Analyzers](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Api.Analyzers) contenant des analyseurs pour les API web.</span><span class="sxs-lookup"><span data-stu-id="20d2c-104">ASP.NET Core 2.2 and later includes the [Microsoft.AspNetCore.Mvc.Api.Analyzers](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Api.Analyzers) NuGet package containing analyzers for web APIs.</span></span> <span data-ttu-id="20d2c-105">Les analyseurs travaillent avec les contrôleurs annotés avec <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute>, tout en s’appuyant sur les [conventions des API](xref:web-api/advanced/conventions).</span><span class="sxs-lookup"><span data-stu-id="20d2c-105">The analyzers work with controllers annotated with <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute>, while building on [API conventions](xref:web-api/advanced/conventions).</span></span>

## <a name="package-installation"></a><span data-ttu-id="20d2c-106">Installation de package</span><span class="sxs-lookup"><span data-stu-id="20d2c-106">Package installation</span></span>

<span data-ttu-id="20d2c-107">Vous pouvez ajouter `Microsoft.AspNetCore.Mvc.Api.Analyzers` en adoptant une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="20d2c-107">`Microsoft.AspNetCore.Mvc.Api.Analyzers` can be added with one of the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="20d2c-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="20d2c-108">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="20d2c-109">À partir de la fenêtre **Console du Gestionnaire de package** :</span><span class="sxs-lookup"><span data-stu-id="20d2c-109">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="20d2c-110">Accédez à **Affichage** > **Autres fenêtres** > **Console du Gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="20d2c-110">Go to **View** > **Other Windows** > **Package Manager Console**.</span></span>
  * <span data-ttu-id="20d2c-111">Accédez au répertoire où se trouve le fichier *ApiConventions.csproj*.</span><span class="sxs-lookup"><span data-stu-id="20d2c-111">Navigate to the directory in which the *ApiConventions.csproj* file exists.</span></span>
  * <span data-ttu-id="20d2c-112">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="20d2c-112">Execute the following command:</span></span>

    ```powershell
    Install-Package Microsoft.AspNetCore.Mvc.Api.Analyzers
    ```

* <span data-ttu-id="20d2c-113">À partir de la boîte de dialogue **Gérer les packages NuGet** :</span><span class="sxs-lookup"><span data-stu-id="20d2c-113">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="20d2c-114">Cliquez avec le bouton droit sur le projet dans **Explorateur de solutions** > **Gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="20d2c-114">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**.</span></span>
  * <span data-ttu-id="20d2c-115">Définissez **Source du package** sur « nuget.org ».</span><span class="sxs-lookup"><span data-stu-id="20d2c-115">Set the **Package source** to "nuget.org".</span></span>
  * <span data-ttu-id="20d2c-116">Entrez « Microsoft.AspNetCore.Mvc.Api.Analyzers » dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="20d2c-116">Enter "Microsoft.AspNetCore.Mvc.Api.Analyzers" in the search box.</span></span>
  * <span data-ttu-id="20d2c-117">Sélectionnez le package « Microsoft.AspNetCore.Mvc.Api.Analyzers » sous l’onglet **Parcourir**, puis cliquez sur **Installer**.</span><span class="sxs-lookup"><span data-stu-id="20d2c-117">Select the "Microsoft.AspNetCore.Mvc.Api.Analyzers" package from the **Browse** tab and click **Install**.</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="20d2c-118">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="20d2c-118">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="20d2c-119">Cliquez avec le bouton droit sur le dossier *Packages* dans **Panneau Solutions** > **Ajouter des packages...**.</span><span class="sxs-lookup"><span data-stu-id="20d2c-119">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**.</span></span>
* <span data-ttu-id="20d2c-120">Dans la fenêtre **Ajouter des packages**, sélectionnez « nuget.org » dans la liste déroulante **Source**.</span><span class="sxs-lookup"><span data-stu-id="20d2c-120">Set the **Add Packages** window's **Source** drop-down to "nuget.org".</span></span>
* <span data-ttu-id="20d2c-121">Entrez « Microsoft.AspNetCore.Mvc.Api.Analyzers » dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="20d2c-121">Enter "Microsoft.AspNetCore.Mvc.Api.Analyzers" in the search box.</span></span>
* <span data-ttu-id="20d2c-122">Sélectionnez le package « Microsoft.AspNetCore.Mvc.Api.Analyzers » dans le volet de résultats, puis cliquez sur **Ajouter un package**.</span><span class="sxs-lookup"><span data-stu-id="20d2c-122">Select the "Microsoft.AspNetCore.Mvc.Api.Analyzers" package from the results pane and click **Add Package**.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="20d2c-123">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="20d2c-123">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="20d2c-124">Exécutez la commande suivante à partir du **Terminal intégré** :</span><span class="sxs-lookup"><span data-stu-id="20d2c-124">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="20d2c-125">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="20d2c-125">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="20d2c-126">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="20d2c-126">Run the following command:</span></span>

```console
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

---

## <a name="analyzers-for-api-conventions"></a><span data-ttu-id="20d2c-127">Analyseurs pour les conventions d’API</span><span class="sxs-lookup"><span data-stu-id="20d2c-127">Analyzers for API conventions</span></span>

<span data-ttu-id="20d2c-128">Les documents sur les API ouvertes contiennent les codes d’état et les types de réponse qu’une action peut retourner.</span><span class="sxs-lookup"><span data-stu-id="20d2c-128">OpenAPI documents contain status codes and response types that an action may return.</span></span> <span data-ttu-id="20d2c-129">Dans ASP.NET Core MVC, des attributs comme <xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute> et <xref:Microsoft.AspNetCore.Mvc.ProducesAttribute> sont utilisés pour documenter une action.</span><span class="sxs-lookup"><span data-stu-id="20d2c-129">In ASP.NET Core MVC, attributes such as <xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute> and <xref:Microsoft.AspNetCore.Mvc.ProducesAttribute> are used to document an action.</span></span> <span data-ttu-id="20d2c-130"><xref:tutorials/web-api-help-pages-using-swagger> constitue une documentation plus détaillée de votre API.</span><span class="sxs-lookup"><span data-stu-id="20d2c-130"><xref:tutorials/web-api-help-pages-using-swagger> goes into further detail on documenting your API.</span></span>

<span data-ttu-id="20d2c-131">Un des analyseurs du package inspecte les contrôleurs annotés avec <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute> et identifie les actions qui ne documentent pas entièrement leurs réponses.</span><span class="sxs-lookup"><span data-stu-id="20d2c-131">One of the analyzers in the package inspects controllers annotated with <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute> and identifies actions that don't entirely document their responses.</span></span> <span data-ttu-id="20d2c-132">Prenons l'exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="20d2c-132">Consider the following example:</span></span>

[!code-csharp[](conventions/sample/Controllers/ContactsController.cs?name=missing404docs&highlight=9)]

<span data-ttu-id="20d2c-133">L’action précédente documente le type de retour avec réussite HTTP 200, mais ne documente pas le code d’état d’échec HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="20d2c-133">The preceding action documents the HTTP 200 success return type but doesn't document the HTTP 404 failure status code.</span></span> <span data-ttu-id="20d2c-134">L’analyseur signale la documentation manquante pour le code d’état HTTP 404 sous la forme d’un avertissement.</span><span class="sxs-lookup"><span data-stu-id="20d2c-134">The analyzer reports the missing documentation for the HTTP 404 status code as a warning.</span></span> <span data-ttu-id="20d2c-135">Une option pour résoudre le problème est fournie.</span><span class="sxs-lookup"><span data-stu-id="20d2c-135">An option to fix the problem is provided.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="20d2c-136">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="20d2c-136">Additional resources</span></span>

* <xref:web-api/advanced/conventions>
* <xref:tutorials/web-api-help-pages-using-swagger>
* [<span data-ttu-id="20d2c-137">Annotation avec attribut ApiController</span><span class="sxs-lookup"><span data-stu-id="20d2c-137">Annotation with ApiController attribute</span></span>](xref:web-api/index#annotation-with-apicontroller-attribute)
