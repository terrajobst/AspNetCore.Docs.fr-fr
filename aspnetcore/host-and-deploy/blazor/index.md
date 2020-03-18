---
title: Héberger et déployer des ASP.NET Core Blazor
author: guardrex
description: Découvrez comment héberger et déployer des applications Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/11/2020
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/index
ms.openlocfilehash: ddf70da29a82d462422c1bdf74ff45b92bb10b56
ms.sourcegitcommit: 5bdc54162d7dea8d9fa54ac3055678db23586af1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/17/2020
ms.locfileid: "79434263"
---
# <a name="host-and-deploy-aspnet-core-blazor"></a><span data-ttu-id="c2fcb-103">Héberger et déployer ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="c2fcb-103">Host and deploy ASP.NET Core Blazor</span></span>

<span data-ttu-id="c2fcb-104">Par [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) et [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="c2fcb-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="publish-the-app"></a><span data-ttu-id="c2fcb-105">Publier l’application</span><span class="sxs-lookup"><span data-stu-id="c2fcb-105">Publish the app</span></span>

<span data-ttu-id="c2fcb-106">Les applications sont publiées pour le déploiement dans la configuration Release.</span><span class="sxs-lookup"><span data-stu-id="c2fcb-106">Apps are published for deployment in Release configuration.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="c2fcb-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c2fcb-107">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="c2fcb-108">Sélectionnez **Build** > **Publier {APPLICATION}** dans la barre de navigation.</span><span class="sxs-lookup"><span data-stu-id="c2fcb-108">Select **Build** > **Publish {APPLICATION}** from the navigation bar.</span></span>
1. <span data-ttu-id="c2fcb-109">Sélectionnez l’onglet *Cible de publication*.</span><span class="sxs-lookup"><span data-stu-id="c2fcb-109">Select the *publish target*.</span></span> <span data-ttu-id="c2fcb-110">Pour publier localement, sélectionnez **Dossier**.</span><span class="sxs-lookup"><span data-stu-id="c2fcb-110">To publish locally, select **Folder**.</span></span>
1. <span data-ttu-id="c2fcb-111">Acceptez l’emplacement par défaut dans le champ **Choisir un dossier** ou spécifiez un autre emplacement.</span><span class="sxs-lookup"><span data-stu-id="c2fcb-111">Accept the default location in the **Choose a folder** field or specify a different location.</span></span> <span data-ttu-id="c2fcb-112">Cliquez sur le bouton **Publier**.</span><span class="sxs-lookup"><span data-stu-id="c2fcb-112">Select the **Publish** button.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="c2fcb-113">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="c2fcb-113">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="c2fcb-114">Utilisez la commande [dotnet publish](/dotnet/core/tools/dotnet-publish) pour publier l’application avec une configuration Release :</span><span class="sxs-lookup"><span data-stu-id="c2fcb-114">Use the [dotnet publish](/dotnet/core/tools/dotnet-publish) command to publish the app with a Release configuration:</span></span>

```dotnetcli
dotnet publish -c Release
```

---

<span data-ttu-id="c2fcb-115">La publication de l’application déclenche une [restauration](/dotnet/core/tools/dotnet-restore) des dépendances du projet et [crée](/dotnet/core/tools/dotnet-build) le projet avant de créer les ressources pour le déploiement.</span><span class="sxs-lookup"><span data-stu-id="c2fcb-115">Publishing the app triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="c2fcb-116">Dans le cadre du processus de génération, les assemblys et méthodes inutilisés sont supprimés pour réduire la durée du chargement et la taille du téléchargement de l’application.</span><span class="sxs-lookup"><span data-stu-id="c2fcb-116">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span>

<span data-ttu-id="c2fcb-117">Emplacements de publication :</span><span class="sxs-lookup"><span data-stu-id="c2fcb-117">Publish locations:</span></span>

* Blazor<span data-ttu-id="c2fcb-118"> webassembly</span><span class="sxs-lookup"><span data-stu-id="c2fcb-118"> WebAssembly</span></span>
  * <span data-ttu-id="c2fcb-119">Autonome &ndash; l’application est publiée dans le dossier */bin/Release/{Target Framework}/Publish/wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="c2fcb-119">Standalone &ndash; The app is published into the */bin/Release/{TARGET FRAMEWORK}/publish/wwwroot* folder.</span></span> <span data-ttu-id="c2fcb-120">Pour déployer l’application en tant que site statique, copiez le contenu du dossier *wwwroot* sur l’hôte de site statique.</span><span class="sxs-lookup"><span data-stu-id="c2fcb-120">To deploy the app as a static site, copy the contents of the *wwwroot* folder to the static site host.</span></span>
  * <span data-ttu-id="c2fcb-121">Hébergé &ndash; l’application webassembly Blazor client est publiée dans le dossier */bin/Release/{Target Framework}/Publish/wwwroot* de l’application serveur, ainsi que les autres ressources Web statiques de l’application serveur.</span><span class="sxs-lookup"><span data-stu-id="c2fcb-121">Hosted &ndash; The client Blazor WebAssembly app is published into the */bin/Release/{TARGET FRAMEWORK}/publish/wwwroot* folder of the server app, along with any other static web assets of the server app.</span></span> <span data-ttu-id="c2fcb-122">Déployez le contenu du dossier de *publication* sur l’hôte.</span><span class="sxs-lookup"><span data-stu-id="c2fcb-122">Deploy the contents of the *publish* folder to the host.</span></span>
* Blazor<span data-ttu-id="c2fcb-123"> Server &ndash; l’application est publiée dans le dossier */bin/Release/{Target Framework}/Publish* .</span><span class="sxs-lookup"><span data-stu-id="c2fcb-123"> Server &ndash; The app is published into the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span> <span data-ttu-id="c2fcb-124">Déployez le contenu du dossier de *publication* sur l’hôte.</span><span class="sxs-lookup"><span data-stu-id="c2fcb-124">Deploy the contents of the *publish* folder to the host.</span></span>

<span data-ttu-id="c2fcb-125">Les ressources du dossier sont déployées sur le serveur web.</span><span class="sxs-lookup"><span data-stu-id="c2fcb-125">The assets in the folder are deployed to the web server.</span></span> <span data-ttu-id="c2fcb-126">Le déploiement peut être un processus manuel ou automatisé, en fonction des outils de développement utilisés.</span><span class="sxs-lookup"><span data-stu-id="c2fcb-126">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="app-base-path"></a><span data-ttu-id="c2fcb-127">Chemin de base de l’application</span><span class="sxs-lookup"><span data-stu-id="c2fcb-127">App base path</span></span>

<span data-ttu-id="c2fcb-128">Le *chemin d’accès de base* de l’application est le chemin de l’URL racine de l’application.</span><span class="sxs-lookup"><span data-stu-id="c2fcb-128">The *app base path* is the app's root URL path.</span></span> <span data-ttu-id="c2fcb-129">Tenez compte des ASP.NET Core application et Blazor sous-application suivantes :</span><span class="sxs-lookup"><span data-stu-id="c2fcb-129">Consider the following ASP.NET Core app and Blazor sub-app:</span></span>

* <span data-ttu-id="c2fcb-130">L’application ASP.NET Core est nommée `MyApp`:</span><span class="sxs-lookup"><span data-stu-id="c2fcb-130">The ASP.NET Core app is named `MyApp`:</span></span>
  * <span data-ttu-id="c2fcb-131">L’application réside physiquement dans *d:/MyApp*.</span><span class="sxs-lookup"><span data-stu-id="c2fcb-131">The app physically resides at *d:/MyApp*.</span></span>
  * <span data-ttu-id="c2fcb-132">Les demandes sont reçues au `https://www.contoso.com/{MYAPP RESOURCE}`.</span><span class="sxs-lookup"><span data-stu-id="c2fcb-132">Requests are received at `https://www.contoso.com/{MYAPP RESOURCE}`.</span></span>
* <span data-ttu-id="c2fcb-133">Une Blazor application nommée `CoolApp` est une sous-application de `MyApp`:</span><span class="sxs-lookup"><span data-stu-id="c2fcb-133">A Blazor app named `CoolApp` is a sub-app of `MyApp`:</span></span>
  * <span data-ttu-id="c2fcb-134">La sous-application réside physiquement dans *d:/MyApp/coolapp*.</span><span class="sxs-lookup"><span data-stu-id="c2fcb-134">The sub-app physically resides at *d:/MyApp/CoolApp*.</span></span>
  * <span data-ttu-id="c2fcb-135">Les demandes sont reçues au `https://www.contoso.com/CoolApp/{COOLAPP RESOURCE}`.</span><span class="sxs-lookup"><span data-stu-id="c2fcb-135">Requests are received at `https://www.contoso.com/CoolApp/{COOLAPP RESOURCE}`.</span></span>

<span data-ttu-id="c2fcb-136">Si vous ne spécifiez pas de configuration supplémentaire pour `CoolApp`, la sous-application dans ce scénario n’a aucune connaissance de son emplacement sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="c2fcb-136">Without specifying additional configuration for `CoolApp`, the sub-app in this scenario has no knowledge of where it resides on the server.</span></span> <span data-ttu-id="c2fcb-137">Par exemple, l’application ne peut pas construire des URL relatives correctes pour ses ressources sans savoir qu’elle réside au chemin d’URL relatif `/CoolApp/`.</span><span class="sxs-lookup"><span data-stu-id="c2fcb-137">For example, the app can't construct correct relative URLs to its resources without knowing that it resides at the relative URL path `/CoolApp/`.</span></span>

<span data-ttu-id="c2fcb-138">Pour fournir la configuration du chemin d’accès de base de l’application Blazor de `https://www.contoso.com/CoolApp/`, l’attribut `href` de la balise `<base>` est défini sur le chemin d’accès racine relatif dans le fichier *pages/_Host. cshtml* (Blazor Server) ou le fichier *wwwroot/index.html* (Blazor webassembly) :</span><span class="sxs-lookup"><span data-stu-id="c2fcb-138">To provide configuration for the Blazor app's base path of `https://www.contoso.com/CoolApp/`, the `<base>` tag's `href` attribute is set to the relative root path in the *Pages/_Host.cshtml* file (Blazor Server) or *wwwroot/index.html* file (Blazor WebAssembly):</span></span>

```html
<base href="/CoolApp/">
```

Blazor<span data-ttu-id="c2fcb-139"> les applications serveur définissent également le chemin d’accès de base côté serveur en appelant <xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*> dans le pipeline de requête de l’application de `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="c2fcb-139"> Server apps additionally set the server-side base path by calling <xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*> in the app's request pipeline of `Startup.Configure`:</span></span>

```csharp
app.UsePathBase("/CoolApp");
```

<span data-ttu-id="c2fcb-140">En fournissant le chemin d’URL relatif, un composant qui ne se trouve pas dans le répertoire racine peut construire des URL relatives au chemin d’accès racine de l’application.</span><span class="sxs-lookup"><span data-stu-id="c2fcb-140">By providing the relative URL path, a component that isn't in the root directory can construct URLs relative to the app's root path.</span></span> <span data-ttu-id="c2fcb-141">Les composants à différents niveaux de la structure de répertoires peuvent créer des liens vers d’autres ressources à des emplacements de l’application.</span><span class="sxs-lookup"><span data-stu-id="c2fcb-141">Components at different levels of the directory structure can build links to other resources at locations throughout the app.</span></span> <span data-ttu-id="c2fcb-142">Le chemin d’accès de base de l’application est également utilisé pour intercepter les liens hypertexte sélectionnés où la `href` cible du lien se trouve dans l’espace URI du chemin d’accès de base de l’application.</span><span class="sxs-lookup"><span data-stu-id="c2fcb-142">The app base path is also used to intercept selected hyperlinks where the `href` target of the link is within the app base path URI space.</span></span> <span data-ttu-id="c2fcb-143">Le routeur Blazor gère la navigation interne.</span><span class="sxs-lookup"><span data-stu-id="c2fcb-143">The Blazor router handles the internal navigation.</span></span>

<span data-ttu-id="c2fcb-144">Dans de nombreux scénarios d’hébergement, le chemin d’URL relatif à l’application est la racine de l’application.</span><span class="sxs-lookup"><span data-stu-id="c2fcb-144">In many hosting scenarios, the relative URL path to the app is the root of the app.</span></span> <span data-ttu-id="c2fcb-145">Dans ce cas, le chemin d’accès de base de l’URL relative de l’application est une barre oblique (`<base href="/" />`), qui est la configuration par défaut pour une application Blazor.</span><span class="sxs-lookup"><span data-stu-id="c2fcb-145">In these cases, the app's relative URL base path is a forward slash (`<base href="/" />`), which is the default configuration for a Blazor app.</span></span> <span data-ttu-id="c2fcb-146">Dans d’autres scénarios d’hébergement, tels que les pages GitHub et les sous-applications IIS, le chemin d’accès de base de l’application doit être défini sur le chemin d’URL relatif du serveur de l’application.</span><span class="sxs-lookup"><span data-stu-id="c2fcb-146">In other hosting scenarios, such as GitHub Pages and IIS sub-apps, the app base path must be set to the server's relative URL path of the app.</span></span>

<span data-ttu-id="c2fcb-147">Pour définir le chemin d’accès de base de l’application, mettez à jour la balise `<base>` dans les éléments `<head>` balise du fichier *pages/_Host. cshtml* (serveurBlazor) ou du fichier *wwwroot/index.html* (Blazor webassembly).</span><span class="sxs-lookup"><span data-stu-id="c2fcb-147">To set the app's base path, update the `<base>` tag within the `<head>` tag elements of the *Pages/_Host.cshtml* file (Blazor Server) or *wwwroot/index.html* file (Blazor WebAssembly).</span></span> <span data-ttu-id="c2fcb-148">Définissez la valeur de l’attribut `href` sur `/{RELATIVE URL PATH}/` (la barre oblique de fin est requise), où `{RELATIVE URL PATH}` est le chemin d’URL relatif complet de l’application.</span><span class="sxs-lookup"><span data-stu-id="c2fcb-148">Set the `href` attribute value to `/{RELATIVE URL PATH}/` (the trailing slash is required), where `{RELATIVE URL PATH}` is the app's full relative URL path.</span></span>

<span data-ttu-id="c2fcb-149">Pour une application Blazor webassembly avec un chemin d’URL relatif non racine (par exemple, `<base href="/CoolApp/">`), l’application ne parvient pas à trouver ses ressources lorsqu’elle est *exécutée localement*.</span><span class="sxs-lookup"><span data-stu-id="c2fcb-149">For an Blazor WebAssembly app with a non-root relative URL path (for example, `<base href="/CoolApp/">`), the app fails to find its resources *when run locally*.</span></span> <span data-ttu-id="c2fcb-150">Pour surmonter ce problème pendant le développement local et les tests, vous pouvez fournir un argument de *base de chemin* qui correspond à la valeur `href` de la balise `<base>` au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="c2fcb-150">To overcome this problem during local development and testing, you can supply a *path base* argument that matches the `href` value of the `<base>` tag at runtime.</span></span> <span data-ttu-id="c2fcb-151">N’incluez pas de barre oblique finale.</span><span class="sxs-lookup"><span data-stu-id="c2fcb-151">Don't include a trailing slash.</span></span> <span data-ttu-id="c2fcb-152">Pour passer l’argument de base Path lors de l’exécution locale de l’application, exécutez la commande `dotnet run` à partir du répertoire de l’application avec l’option `--pathbase` :</span><span class="sxs-lookup"><span data-stu-id="c2fcb-152">To pass the path base argument when running the app locally, execute the `dotnet run` command from the app's directory with the `--pathbase` option:</span></span>

```dotnetcli
dotnet run --pathbase=/{RELATIVE URL PATH (no trailing slash)}
```

<span data-ttu-id="c2fcb-153">Pour une application Blazor webassembly avec un chemin d’URL relatif de `/CoolApp/` (`<base href="/CoolApp/">`), la commande est la suivante :</span><span class="sxs-lookup"><span data-stu-id="c2fcb-153">For a Blazor WebAssembly app with a relative URL path of `/CoolApp/` (`<base href="/CoolApp/">`), the command is:</span></span>

```dotnetcli
dotnet run --pathbase=/CoolApp
```

<span data-ttu-id="c2fcb-154">L’application Blazor webassembly répond localement à `http://localhost:port/CoolApp`.</span><span class="sxs-lookup"><span data-stu-id="c2fcb-154">The Blazor WebAssembly app responds locally at `http://localhost:port/CoolApp`.</span></span>

## <a name="deployment"></a><span data-ttu-id="c2fcb-155">Déploiement</span><span class="sxs-lookup"><span data-stu-id="c2fcb-155">Deployment</span></span>

<span data-ttu-id="c2fcb-156">Pour obtenir des conseils de déploiement, consultez les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="c2fcb-156">For deployment guidance, see the following topics:</span></span>

* <xref:host-and-deploy/blazor/webassembly>
* <xref:host-and-deploy/blazor/server>
