---
title: Héberger et déployer des ASP.NET Core Blazor
author: guardrex
description: Découvrez comment héberger et déployer des applications Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/index
ms.openlocfilehash: 238e7fc8f8d64c7847dc8847fb66e22442a3c8e0
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76160260"
---
# <a name="host-and-deploy-aspnet-core-opno-locblazor"></a><span data-ttu-id="a3575-103">Héberger et déployer des ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="a3575-103">Host and deploy ASP.NET Core Blazor</span></span>

<span data-ttu-id="a3575-104">Par [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) et [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="a3575-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="publish-the-app"></a><span data-ttu-id="a3575-105">Publier l'application</span><span class="sxs-lookup"><span data-stu-id="a3575-105">Publish the app</span></span>

<span data-ttu-id="a3575-106">Les applications sont publiées pour le déploiement dans la configuration Release.</span><span class="sxs-lookup"><span data-stu-id="a3575-106">Apps are published for deployment in Release configuration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a3575-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a3575-107">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="a3575-108">Sélectionnez **Build** > **Publier {APPLICATION}** dans la barre de navigation.</span><span class="sxs-lookup"><span data-stu-id="a3575-108">Select **Build** > **Publish {APPLICATION}** from the navigation bar.</span></span>
1. <span data-ttu-id="a3575-109">Sélectionnez l’onglet *Cible de publication*.</span><span class="sxs-lookup"><span data-stu-id="a3575-109">Select the *publish target*.</span></span> <span data-ttu-id="a3575-110">Pour publier localement, sélectionnez **Dossier**.</span><span class="sxs-lookup"><span data-stu-id="a3575-110">To publish locally, select **Folder**.</span></span>
1. <span data-ttu-id="a3575-111">Acceptez l’emplacement par défaut dans le champ **Choisir un dossier** ou spécifiez un autre emplacement.</span><span class="sxs-lookup"><span data-stu-id="a3575-111">Accept the default location in the **Choose a folder** field or specify a different location.</span></span> <span data-ttu-id="a3575-112">Sélectionnez le bouton **Publier**.</span><span class="sxs-lookup"><span data-stu-id="a3575-112">Select the **Publish** button.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a3575-113">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="a3575-113">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="a3575-114">Utilisez la commande [dotnet publish](/dotnet/core/tools/dotnet-publish) pour publier l’application avec une configuration Release :</span><span class="sxs-lookup"><span data-stu-id="a3575-114">Use the [dotnet publish](/dotnet/core/tools/dotnet-publish) command to publish the app with a Release configuration:</span></span>

```dotnetcli
dotnet publish -c Release
```

---

<span data-ttu-id="a3575-115">La publication de l’application déclenche une [restauration](/dotnet/core/tools/dotnet-restore) des dépendances du projet et [crée](/dotnet/core/tools/dotnet-build) le projet avant de créer les ressources pour le déploiement.</span><span class="sxs-lookup"><span data-stu-id="a3575-115">Publishing the app triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="a3575-116">Dans le cadre du processus de génération, les assemblys et méthodes inutilisés sont supprimés pour réduire la durée du chargement et la taille du téléchargement de l’application.</span><span class="sxs-lookup"><span data-stu-id="a3575-116">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span>

<span data-ttu-id="a3575-117">Une application Blazor webassembly est publiée dans le dossier */bin/Release/{Target Framework}/Publish/{assembly/dist* .</span><span class="sxs-lookup"><span data-stu-id="a3575-117">A Blazor WebAssembly app is published to the */bin/Release/{TARGET FRAMEWORK}/publish/{ASSEMBLY NAME}/dist* folder.</span></span> <span data-ttu-id="a3575-118">Une application Blazor Server est publiée dans le dossier */bin/Release/{Target Framework}/Publish* .</span><span class="sxs-lookup"><span data-stu-id="a3575-118">A Blazor Server app is published to the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span>

<span data-ttu-id="a3575-119">Les ressources du dossier sont déployées sur le serveur web.</span><span class="sxs-lookup"><span data-stu-id="a3575-119">The assets in the folder are deployed to the web server.</span></span> <span data-ttu-id="a3575-120">Le déploiement peut être un processus manuel ou automatisé, en fonction des outils de développement utilisés.</span><span class="sxs-lookup"><span data-stu-id="a3575-120">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="app-base-path"></a><span data-ttu-id="a3575-121">Chemin de base de l’application</span><span class="sxs-lookup"><span data-stu-id="a3575-121">App base path</span></span>

<span data-ttu-id="a3575-122">Le *chemin d’accès de base* de l’application est le chemin de l’URL racine de l’application.</span><span class="sxs-lookup"><span data-stu-id="a3575-122">The *app base path* is the app's root URL path.</span></span> <span data-ttu-id="a3575-123">Tenez compte des ASP.NET Core application et Blazor sous-application suivantes :</span><span class="sxs-lookup"><span data-stu-id="a3575-123">Consider the following ASP.NET Core app and Blazor sub-app:</span></span>

* <span data-ttu-id="a3575-124">L’application ASP.NET Core est nommée `MyApp`:</span><span class="sxs-lookup"><span data-stu-id="a3575-124">The ASP.NET Core app is named `MyApp`:</span></span>
  * <span data-ttu-id="a3575-125">L’application réside physiquement dans *d:/MyApp*.</span><span class="sxs-lookup"><span data-stu-id="a3575-125">The app physically resides at *d:/MyApp*.</span></span>
  * <span data-ttu-id="a3575-126">Les demandes sont reçues au `https://www.contoso.com/{MYAPP RESOURCE}`.</span><span class="sxs-lookup"><span data-stu-id="a3575-126">Requests are received at `https://www.contoso.com/{MYAPP RESOURCE}`.</span></span>
* <span data-ttu-id="a3575-127">Une Blazor application nommée `CoolApp` est une sous-application de `MyApp`:</span><span class="sxs-lookup"><span data-stu-id="a3575-127">A Blazor app named `CoolApp` is a sub-app of `MyApp`:</span></span>
  * <span data-ttu-id="a3575-128">La sous-application réside physiquement dans *d:/MyApp/coolapp*.</span><span class="sxs-lookup"><span data-stu-id="a3575-128">The sub-app physically resides at *d:/MyApp/CoolApp*.</span></span>
  * <span data-ttu-id="a3575-129">Les demandes sont reçues au `https://www.contoso.com/CoolApp/{COOLAPP RESOURCE}`.</span><span class="sxs-lookup"><span data-stu-id="a3575-129">Requests are received at `https://www.contoso.com/CoolApp/{COOLAPP RESOURCE}`.</span></span>

<span data-ttu-id="a3575-130">Si vous ne spécifiez pas de configuration supplémentaire pour `CoolApp`, la sous-application dans ce scénario n’a aucune connaissance de son emplacement sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="a3575-130">Without specifying additional configuration for `CoolApp`, the sub-app in this scenario has no knowledge of where it resides on the server.</span></span> <span data-ttu-id="a3575-131">Par exemple, l’application ne peut pas construire des URL relatives correctes pour ses ressources sans savoir qu’elle réside au chemin d’URL relatif `/CoolApp/`.</span><span class="sxs-lookup"><span data-stu-id="a3575-131">For example, the app can't construct correct relative URLs to its resources without knowing that it resides at the relative URL path `/CoolApp/`.</span></span>

<span data-ttu-id="a3575-132">Pour fournir la configuration du chemin d’accès de base de l’application Blazor de `https://www.contoso.com/CoolApp/`, l’attribut `href` de la balise `<base>` est défini sur le chemin d’accès racine relatif dans le fichier *pages/_Host. cshtml* (Blazor Server) ou le fichier *wwwroot/index.html* (Blazor webassembly) :</span><span class="sxs-lookup"><span data-stu-id="a3575-132">To provide configuration for the Blazor app's base path of `https://www.contoso.com/CoolApp/`, the `<base>` tag's `href` attribute is set to the relative root path in the *Pages/_Host.cshtml* file (Blazor Server) or *wwwroot/index.html* file (Blazor WebAssembly):</span></span>

```html
<base href="/CoolApp/">
```

Blazor<span data-ttu-id="a3575-133"> les applications serveur définissent également le chemin d’accès de base côté serveur en appelant <xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*> dans le pipeline de requête de l’application de `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="a3575-133"> Server apps additionally set the server-side base path by calling <xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*> in the app's request pipeline of `Startup.Configure`:</span></span>

```csharp
app.UsePathBase("/CoolApp");
```

<span data-ttu-id="a3575-134">En fournissant le chemin d’URL relatif, un composant qui ne se trouve pas dans le répertoire racine peut construire des URL relatives au chemin d’accès racine de l’application.</span><span class="sxs-lookup"><span data-stu-id="a3575-134">By providing the relative URL path, a component that isn't in the root directory can construct URLs relative to the app's root path.</span></span> <span data-ttu-id="a3575-135">Les composants à différents niveaux de la structure de répertoires peuvent créer des liens vers d’autres ressources à des emplacements de l’application.</span><span class="sxs-lookup"><span data-stu-id="a3575-135">Components at different levels of the directory structure can build links to other resources at locations throughout the app.</span></span> <span data-ttu-id="a3575-136">Le chemin d’accès de base de l’application est également utilisé pour intercepter les liens hypertexte sélectionnés où la `href` cible du lien se trouve dans l’espace URI du chemin d’accès de base de l’application.</span><span class="sxs-lookup"><span data-stu-id="a3575-136">The app base path is also used to intercept selected hyperlinks where the `href` target of the link is within the app base path URI space.</span></span> <span data-ttu-id="a3575-137">Le routeur Blazor gère la navigation interne.</span><span class="sxs-lookup"><span data-stu-id="a3575-137">The Blazor router handles the internal navigation.</span></span>

<span data-ttu-id="a3575-138">Dans de nombreux scénarios d’hébergement, le chemin d’URL relatif à l’application est la racine de l’application.</span><span class="sxs-lookup"><span data-stu-id="a3575-138">In many hosting scenarios, the relative URL path to the app is the root of the app.</span></span> <span data-ttu-id="a3575-139">Dans ce cas, le chemin d’accès de base de l’URL relative de l’application est une barre oblique (`<base href="/" />`), qui est la configuration par défaut pour une application Blazor.</span><span class="sxs-lookup"><span data-stu-id="a3575-139">In these cases, the app's relative URL base path is a forward slash (`<base href="/" />`), which is the default configuration for a Blazor app.</span></span> <span data-ttu-id="a3575-140">Dans d’autres scénarios d’hébergement, tels que les pages GitHub et les sous-applications IIS, le chemin d’accès de base de l’application doit être défini sur le chemin d’URL relatif du serveur de l’application.</span><span class="sxs-lookup"><span data-stu-id="a3575-140">In other hosting scenarios, such as GitHub Pages and IIS sub-apps, the app base path must be set to the server's relative URL path of the app.</span></span>

<span data-ttu-id="a3575-141">Pour définir le chemin d’accès de base de l’application, mettez à jour la balise `<base>` dans les éléments `<head>` balise du fichier *pages/_Host. cshtml* (serveurBlazor) ou du fichier *wwwroot/index.html* (Blazor webassembly).</span><span class="sxs-lookup"><span data-stu-id="a3575-141">To set the app's base path, update the `<base>` tag within the `<head>` tag elements of the *Pages/_Host.cshtml* file (Blazor Server) or *wwwroot/index.html* file (Blazor WebAssembly).</span></span> <span data-ttu-id="a3575-142">Définissez la valeur de l’attribut `href` sur `/{RELATIVE URL PATH}/` (la barre oblique de fin est requise), où `{RELATIVE URL PATH}` est le chemin d’URL relatif complet de l’application.</span><span class="sxs-lookup"><span data-stu-id="a3575-142">Set the `href` attribute value to `/{RELATIVE URL PATH}/` (the trailing slash is required), where `{RELATIVE URL PATH}` is the app's full relative URL path.</span></span>

<span data-ttu-id="a3575-143">Pour une application Blazor webassembly avec un chemin d’URL relatif non racine (par exemple, `<base href="/CoolApp/">`), l’application ne parvient pas à trouver ses ressources lorsqu’elle est *exécutée localement*.</span><span class="sxs-lookup"><span data-stu-id="a3575-143">For an Blazor WebAssembly app with a non-root relative URL path (for example, `<base href="/CoolApp/">`), the app fails to find its resources *when run locally*.</span></span> <span data-ttu-id="a3575-144">Pour surmonter ce problème pendant le développement local et les tests, vous pouvez fournir un argument de *base de chemin* qui correspond à la valeur `href` de la balise `<base>` au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="a3575-144">To overcome this problem during local development and testing, you can supply a *path base* argument that matches the `href` value of the `<base>` tag at runtime.</span></span> <span data-ttu-id="a3575-145">N’incluez pas de barre oblique finale.</span><span class="sxs-lookup"><span data-stu-id="a3575-145">Don't include a trailing slash.</span></span> <span data-ttu-id="a3575-146">Pour passer l’argument de base Path lors de l’exécution locale de l’application, exécutez la commande `dotnet run` à partir du répertoire de l’application avec l’option `--pathbase` :</span><span class="sxs-lookup"><span data-stu-id="a3575-146">To pass the path base argument when running the app locally, execute the `dotnet run` command from the app's directory with the `--pathbase` option:</span></span>

```dotnetcli
dotnet run --pathbase=/{RELATIVE URL PATH (no trailing slash)}
```

<span data-ttu-id="a3575-147">Pour une application Blazor webassembly avec un chemin d’URL relatif de `/CoolApp/` (`<base href="/CoolApp/">`), la commande est la suivante :</span><span class="sxs-lookup"><span data-stu-id="a3575-147">For a Blazor WebAssembly app with a relative URL path of `/CoolApp/` (`<base href="/CoolApp/">`), the command is:</span></span>

```dotnetcli
dotnet run --pathbase=/CoolApp
```

<span data-ttu-id="a3575-148">L’application Blazor webassembly répond localement à `http://localhost:port/CoolApp`.</span><span class="sxs-lookup"><span data-stu-id="a3575-148">The Blazor WebAssembly app responds locally at `http://localhost:port/CoolApp`.</span></span>

## <a name="deployment"></a><span data-ttu-id="a3575-149">Déploiement</span><span class="sxs-lookup"><span data-stu-id="a3575-149">Deployment</span></span>

<span data-ttu-id="a3575-150">Pour obtenir des conseils de déploiement, consultez les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="a3575-150">For deployment guidance, see the following topics:</span></span>

* <xref:host-and-deploy/blazor/webassembly>
* <xref:host-and-deploy/blazor/server>
