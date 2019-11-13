---
title: Lien du navigateur dans ASP.NET Core
author: ncarandini
description: Explique comment le lien du navigateur est une fonctionnalité de Visual Studio qui lie l’environnement de développement à un ou plusieurs navigateurs Web.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 11/12/2019
no-loc:
- SignalR
uid: client-side/using-browserlink
ms.openlocfilehash: b21b698d49e72b559cd9cd3753c48a38c99db24d
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73962783"
---
# <a name="browser-link-in-aspnet-core"></a><span data-ttu-id="3f8b7-103">Lien du navigateur dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3f8b7-103">Browser Link in ASP.NET Core</span></span>

<span data-ttu-id="3f8b7-104">Par [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson)et [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="3f8b7-104">By [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="3f8b7-105">Le lien du navigateur est une fonctionnalité de Visual Studio qui crée un canal de communication entre l’environnement de développement et un ou plusieurs navigateurs Web.</span><span class="sxs-lookup"><span data-stu-id="3f8b7-105">Browser Link is a feature in Visual Studio that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="3f8b7-106">Vous pouvez utiliser le lien du navigateur pour actualiser votre application Web dans plusieurs navigateurs à la fois, ce qui est utile pour les tests entre navigateurs.</span><span class="sxs-lookup"><span data-stu-id="3f8b7-106">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

## <a name="browser-link-setup"></a><span data-ttu-id="3f8b7-107">Configuration des liens du navigateur</span><span class="sxs-lookup"><span data-stu-id="3f8b7-107">Browser Link setup</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="3f8b7-108">Lors de la conversion d’un projet ASP.NET Core 2,0 en ASP.NET Core 2,1 et de la transition vers le [AspNetCore Microsoft. app](xref:fundamentals/metapackage-app), installez le package [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) pour la fonctionnalité BrowserLink.</span><span class="sxs-lookup"><span data-stu-id="3f8b7-108">When converting an ASP.NET Core 2.0 project to ASP.NET Core 2.1 and transitioning to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), install the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package for BrowserLink functionality.</span></span> <span data-ttu-id="3f8b7-109">Les modèles de projet ASP.NET Core 2,1 utilisent le sous-package `Microsoft.AspNetCore.App` par défaut.</span><span class="sxs-lookup"><span data-stu-id="3f8b7-109">The ASP.NET Core 2.1 project templates use the `Microsoft.AspNetCore.App` metapackage by default.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="3f8b7-110">Les modèles de projet **application web**ASP.net Core 2,0, **vide**et **API Web** utilisent le sous- [package Microsoft. AspNetCore. All](xref:fundamentals/metapackage), qui contient une référence de package pour [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span><span class="sxs-lookup"><span data-stu-id="3f8b7-110">The ASP.NET Core 2.0 **Web Application**, **Empty**, and **Web API** project templates use the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), which contains a package reference for [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span></span> <span data-ttu-id="3f8b7-111">Par conséquent, l’utilisation du `Microsoft.AspNetCore.All` repackage ne nécessite aucune action supplémentaire pour rendre le lien de navigateur disponible.</span><span class="sxs-lookup"><span data-stu-id="3f8b7-111">Therefore, using the `Microsoft.AspNetCore.All` metapackage requires no further action to make Browser Link available for use.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="3f8b7-112">Le modèle de projet d' **application Web** ASP.net Core 1. x a une référence de package pour le package [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) .</span><span class="sxs-lookup"><span data-stu-id="3f8b7-112">The ASP.NET Core 1.x **Web Application** project template has a package reference for the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package.</span></span> <span data-ttu-id="3f8b7-113">Les projets de modèle d' **API Web** ou **vides** requièrent que vous ajoutiez une référence de package à `Microsoft.VisualStudio.Web.BrowserLink`.</span><span class="sxs-lookup"><span data-stu-id="3f8b7-113">The **Empty** or **Web API** template projects require you to add a package reference to `Microsoft.VisualStudio.Web.BrowserLink`.</span></span>

<span data-ttu-id="3f8b7-114">Dans la mesure où il s’agit d’une fonctionnalité de Visual Studio, le moyen le plus simple d’ajouter le package à un projet de modèle d' **API Web** ou **vide** consiste à ouvrir la **console du gestionnaire de package** (**Afficher** > autre console du **Gestionnaire de package** **Windows** >) et à exécuter la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="3f8b7-114">Since this is a Visual Studio feature, the easiest way to add the package to an **Empty** or **Web API** template project is to open the **Package Manager Console** (**View** > **Other Windows** > **Package Manager Console**) and run the following command:</span></span>

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

<span data-ttu-id="3f8b7-115">Vous pouvez également utiliser le **Gestionnaire de package NuGet**.</span><span class="sxs-lookup"><span data-stu-id="3f8b7-115">Alternatively, you can use **NuGet Package Manager**.</span></span> <span data-ttu-id="3f8b7-116">Dans **Explorateur de solutions** , cliquez avec le bouton droit sur le nom du projet et choisissez **gérer les packages NuGet**:</span><span class="sxs-lookup"><span data-stu-id="3f8b7-116">Right-click the project name in **Solution Explorer** and choose **Manage NuGet Packages**:</span></span>

![Ouvrir le gestionnaire de package NuGet](using-browserlink/_static/open-nuget-package-manager.png)

<span data-ttu-id="3f8b7-118">Recherchez et installez le package :</span><span class="sxs-lookup"><span data-stu-id="3f8b7-118">Find and install the package:</span></span>

![Ajouter un package avec le gestionnaire de package NuGet](using-browserlink/_static/add-package-with-nuget-package-manager.png)

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="3f8b7-120">Configuration</span><span class="sxs-lookup"><span data-stu-id="3f8b7-120">Configuration</span></span>

<span data-ttu-id="3f8b7-121">Dans la méthode `Startup.Configure` :</span><span class="sxs-lookup"><span data-stu-id="3f8b7-121">In the `Startup.Configure` method:</span></span>

```csharp
app.UseBrowserLink();
```

<span data-ttu-id="3f8b7-122">En général, le code se trouve à l’intérieur d’un bloc de `if` qui active uniquement le lien du navigateur dans l’environnement de développement, comme illustré ici :</span><span class="sxs-lookup"><span data-stu-id="3f8b7-122">Usually the code is inside an `if` block that only enables Browser Link in the Development environment, as shown here:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

<span data-ttu-id="3f8b7-123">Pour plus d’informations, consultez [Utiliser plusieurs environnements](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="3f8b7-123">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="how-to-use-browser-link"></a><span data-ttu-id="3f8b7-124">Procédure d’utilisation d’un lien de navigateur</span><span class="sxs-lookup"><span data-stu-id="3f8b7-124">How to use Browser Link</span></span>

<span data-ttu-id="3f8b7-125">Quand un projet de ASP.NET Core est ouvert, Visual Studio affiche le contrôle de barre d’outils lien de navigateur en regard du contrôle de barre d’outils **cible de débogage** :</span><span class="sxs-lookup"><span data-stu-id="3f8b7-125">When you have an ASP.NET Core project open, Visual Studio shows the Browser Link toolbar control next to the **Debug Target** toolbar control:</span></span>

![Menu déroulant lien du navigateur](using-browserlink/_static/browserLink-dropdown-menu.png)

<span data-ttu-id="3f8b7-127">Dans le contrôle de barre d’outils lien de navigateur, vous pouvez :</span><span class="sxs-lookup"><span data-stu-id="3f8b7-127">From the Browser Link toolbar control, you can:</span></span>

* <span data-ttu-id="3f8b7-128">Actualisez l’application Web dans plusieurs navigateurs à la fois.</span><span class="sxs-lookup"><span data-stu-id="3f8b7-128">Refresh the web application in several browsers at once.</span></span>
* <span data-ttu-id="3f8b7-129">Ouvrez le **tableau de bord du lien de navigateur**.</span><span class="sxs-lookup"><span data-stu-id="3f8b7-129">Open the **Browser Link Dashboard**.</span></span>
* <span data-ttu-id="3f8b7-130">Activez ou désactivez le **lien du navigateur**.</span><span class="sxs-lookup"><span data-stu-id="3f8b7-130">Enable or disable **Browser Link**.</span></span> <span data-ttu-id="3f8b7-131">Remarque : le lien du navigateur est désactivé par défaut dans Visual Studio 2017 (15,3).</span><span class="sxs-lookup"><span data-stu-id="3f8b7-131">Note: Browser Link is disabled by default in Visual Studio 2017 (15.3).</span></span>
* <span data-ttu-id="3f8b7-132">Activez ou désactivez la [synchronisation automatique CSS](#enable-or-disable-css-auto-sync).</span><span class="sxs-lookup"><span data-stu-id="3f8b7-132">Enable or disable [CSS Auto-Sync](#enable-or-disable-css-auto-sync).</span></span>

> [!NOTE]
> <span data-ttu-id="3f8b7-133">Certains plug-ins Visual Studio, notamment le *Pack d’extension web 2015* et le *Pack d’extension Web 2017*, offrent des fonctionnalités étendues pour le lien du navigateur, mais certaines fonctionnalités supplémentaires ne fonctionnent pas avec les projets de ASP.net core.</span><span class="sxs-lookup"><span data-stu-id="3f8b7-133">Some Visual Studio plug-ins, most notably *Web Extension Pack 2015* and *Web Extension Pack 2017*, offer extended functionality for Browser Link, but some of the additional features don't work with ASP.NET Core projects.</span></span>

## <a name="refresh-the-web-app-in-several-browsers-at-once"></a><span data-ttu-id="3f8b7-134">Actualiser l’application Web dans plusieurs navigateurs à la fois</span><span class="sxs-lookup"><span data-stu-id="3f8b7-134">Refresh the web app in several browsers at once</span></span>

<span data-ttu-id="3f8b7-135">Pour choisir un seul navigateur Web à lancer au démarrage du projet, utilisez le menu déroulant du contrôle de barre d’outils de la **cible de débogage** :</span><span class="sxs-lookup"><span data-stu-id="3f8b7-135">To choose a single web browser to launch when starting the project, use the drop-down menu in the **Debug Target** toolbar control:</span></span>

![Menu déroulant F5](using-browserlink/_static/debug-target-dropdown-menu.png)

<span data-ttu-id="3f8b7-137">Pour ouvrir plusieurs navigateurs à la fois, choisissez **Parcourir avec...** dans la même liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="3f8b7-137">To open multiple browsers at once, choose **Browse with...** from the same drop-down.</span></span> <span data-ttu-id="3f8b7-138">Maintenez la touche CTRL enfoncée pour sélectionner les navigateurs de votre choix, puis cliquez sur **Parcourir**:</span><span class="sxs-lookup"><span data-stu-id="3f8b7-138">Hold down the CTRL key to select the browsers you want, and then click **Browse**:</span></span>

![Ouvrir plusieurs navigateurs à la fois](using-browserlink/_static/open-many-browsers-at-once.png)

<span data-ttu-id="3f8b7-140">Voici une capture d’écran montrant Visual Studio avec la vue d’index ouverte et deux navigateurs ouverts :</span><span class="sxs-lookup"><span data-stu-id="3f8b7-140">Here's a screenshot showing Visual Studio with the Index view open and two open browsers:</span></span>

![Exemple de synchronisation avec deux navigateurs](using-browserlink/_static/sync-with-two-browsers-example.png)

<span data-ttu-id="3f8b7-142">Pointez sur le contrôle de barre d’outils lien du navigateur pour afficher les navigateurs qui sont connectés au projet :</span><span class="sxs-lookup"><span data-stu-id="3f8b7-142">Hover over the Browser Link toolbar control to see the browsers that are connected to the project:</span></span>

![Pointe pointage](using-browserlink/_static/hoover-tip.png)

<span data-ttu-id="3f8b7-144">Modifiez la vue index et tous les navigateurs connectés sont mis à jour lorsque vous cliquez sur le bouton actualiser le lien du navigateur :</span><span class="sxs-lookup"><span data-stu-id="3f8b7-144">Change the Index view, and all connected browsers are updated when you click the Browser Link refresh button:</span></span>

![navigateurs-synchronisation-à-modification](using-browserlink/_static/browsers-sync-to-changes.png)

<span data-ttu-id="3f8b7-146">Le lien du navigateur fonctionne également avec les navigateurs que vous lancez en dehors de Visual Studio et accédez à l’URL de l’application.</span><span class="sxs-lookup"><span data-stu-id="3f8b7-146">Browser Link also works with browsers that you launch from outside Visual Studio and navigate to the application URL.</span></span>

### <a name="the-browser-link-dashboard"></a><span data-ttu-id="3f8b7-147">Tableau de bord du lien de navigateur</span><span class="sxs-lookup"><span data-stu-id="3f8b7-147">The Browser Link Dashboard</span></span>

<span data-ttu-id="3f8b7-148">Ouvrez le tableau de bord du lien de navigateur dans le menu déroulant lien du navigateur pour gérer la connexion avec les navigateurs ouverts :</span><span class="sxs-lookup"><span data-stu-id="3f8b7-148">Open the Browser Link Dashboard from the Browser Link drop down menu to manage the connection with open browsers:</span></span>

![ouvrir-browserslink-tableau de bord](using-browserlink/_static/open-browserlink-dashboard.png)

<span data-ttu-id="3f8b7-150">Si aucun navigateur n’est connecté, vous pouvez démarrer une session de non-débogage en sélectionnant le lien *afficher dans le navigateur* :</span><span class="sxs-lookup"><span data-stu-id="3f8b7-150">If no browser is connected, you can start a non-debugging session by selecting the *View in Browser* link:</span></span>

![browserlink-tableau de bord-sans-connexions](using-browserlink/_static/browserlink-dashboard-no-connections.png)

<span data-ttu-id="3f8b7-152">Dans le cas contraire, les navigateurs connectés affichent le chemin d’accès à la page affichée par chaque navigateur :</span><span class="sxs-lookup"><span data-stu-id="3f8b7-152">Otherwise, the connected browsers are shown with the path to the page that each browser is showing:</span></span>

![browserlink-tableau de bord-deux-connexions](using-browserlink/_static/browserlink-dashboard-two-connections.png)

<span data-ttu-id="3f8b7-154">Si vous le souhaitez, vous pouvez cliquer sur le nom d’un navigateur pour actualiser ce navigateur.</span><span class="sxs-lookup"><span data-stu-id="3f8b7-154">If you like, you can click on a listed browser name to refresh that single browser.</span></span>

### <a name="enable-or-disable-browser-link"></a><span data-ttu-id="3f8b7-155">Activer ou désactiver le lien du navigateur</span><span class="sxs-lookup"><span data-stu-id="3f8b7-155">Enable or disable Browser Link</span></span>

<span data-ttu-id="3f8b7-156">Lorsque vous réactivez le lien de navigateur après l’avoir désactivé, vous devez actualiser les navigateurs pour les reconnecter.</span><span class="sxs-lookup"><span data-stu-id="3f8b7-156">When you re-enable Browser Link after disabling it, you must refresh the browsers to reconnect them.</span></span>

### <a name="enable-or-disable-css-auto-sync"></a><span data-ttu-id="3f8b7-157">Activer ou désactiver la synchronisation automatique CSS</span><span class="sxs-lookup"><span data-stu-id="3f8b7-157">Enable or disable CSS Auto-Sync</span></span>

<span data-ttu-id="3f8b7-158">Lorsque la synchronisation automatique CSS est activée, les navigateurs connectés sont actualisés automatiquement lorsque vous apportez des modifications aux fichiers CSS.</span><span class="sxs-lookup"><span data-stu-id="3f8b7-158">When CSS Auto-Sync is enabled, connected browsers are automatically refreshed when you make any change to CSS files.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="3f8b7-159">Fonctionnement</span><span class="sxs-lookup"><span data-stu-id="3f8b7-159">How it works</span></span>

<span data-ttu-id="3f8b7-160">Le lien du navigateur utilise SignalR pour créer un canal de communication entre Visual Studio et le navigateur.</span><span class="sxs-lookup"><span data-stu-id="3f8b7-160">Browser Link uses SignalR to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="3f8b7-161">Lorsque le lien du navigateur est activé, Visual Studio agit comme un serveur de SignalR auquel plusieurs clients (navigateurs) peuvent se connecter.</span><span class="sxs-lookup"><span data-stu-id="3f8b7-161">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="3f8b7-162">Le lien du navigateur inscrit également un composant d’intergiciel dans le pipeline de demande ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3f8b7-162">Browser Link also registers a middleware component in the ASP.NET Core request pipeline.</span></span> <span data-ttu-id="3f8b7-163">Ce composant injecte des références `<script>` spéciales dans chaque demande de page à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="3f8b7-163">This component injects special `<script>` references into every page request from the server.</span></span> <span data-ttu-id="3f8b7-164">Vous pouvez voir les références de script en sélectionnant **afficher la source** dans le navigateur et en faisant défiler jusqu’à la fin du contenu de la balise `<body>` :</span><span class="sxs-lookup"><span data-stu-id="3f8b7-164">You can see the script references by selecting **View source** in the browser and scrolling to the end of the `<body>` tag content:</span></span>

```html
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

<span data-ttu-id="3f8b7-165">Vos fichiers sources ne sont pas modifiés.</span><span class="sxs-lookup"><span data-stu-id="3f8b7-165">Your source files aren't modified.</span></span> <span data-ttu-id="3f8b7-166">Le composant intergiciel (middleware) injecte les références de script de manière dynamique.</span><span class="sxs-lookup"><span data-stu-id="3f8b7-166">The middleware component injects the script references dynamically.</span></span>

<span data-ttu-id="3f8b7-167">Étant donné que le code côté navigateur est tout JavaScript, il fonctionne sur tous les navigateurs que SignalR prend en charge sans nécessiter de plug-in de navigateur.</span><span class="sxs-lookup"><span data-stu-id="3f8b7-167">Because the browser-side code is all JavaScript, it works on all browsers that SignalR supports without requiring a browser plug-in.</span></span>
