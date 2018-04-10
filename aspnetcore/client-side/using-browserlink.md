---
title: Lien du navigateur dans ASP.NET Core
author: ncarandini
description: Explique comment le lien du navigateur est une fonctionnalité de Visual Studio qui lie l’environnement de développement avec un ou plusieurs navigateurs web.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/22/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/using-browserlink
ms.openlocfilehash: a75a896dd7ebc488e3e9344ec705c24201924375
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="browser-link-in-aspnet-core"></a><span data-ttu-id="8c935-103">Lien du navigateur dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8c935-103">Browser Link in ASP.NET Core</span></span>

<span data-ttu-id="8c935-104">Par [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), et [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="8c935-104">By [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="8c935-105">Le lien du navigateur est une fonctionnalité de Visual Studio qui crée un canal de communication entre l’environnement de développement et un ou plusieurs navigateurs web.</span><span class="sxs-lookup"><span data-stu-id="8c935-105">Browser Link is a feature in Visual Studio that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="8c935-106">Vous pouvez utiliser le lien du navigateur pour actualiser votre application web dans plusieurs navigateurs à la fois, ce qui est utile pour les tests dans plusieurs navigateurs.</span><span class="sxs-lookup"><span data-stu-id="8c935-106">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

## <a name="browser-link-setup"></a><span data-ttu-id="8c935-107">Installation du lien du navigateur</span><span class="sxs-lookup"><span data-stu-id="8c935-107">Browser Link setup</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8c935-108">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8c935-108">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8c935-109">Le modèles de projet d’ASP.NET Core 2.x **Application Web**, **Vide** et **API Web** utilisent le métapackage [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) qui contient une référence de package pour [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/)</span><span class="sxs-lookup"><span data-stu-id="8c935-109">The ASP.NET Core 2.x **Web Application**, **Empty**, and **Web API** template projects use the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) meta-package, which contains a package reference for [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span></span> <span data-ttu-id="8c935-110">Grâce au métapackage `Microsoft.AspNetCore.All`, aucune action supplémentaire n'est nécessaire pour rendre le lien du navigateur disponible.</span><span class="sxs-lookup"><span data-stu-id="8c935-110">Therefore, using the `Microsoft.AspNetCore.All` meta-package requires no further action to make Browser Link available for use.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8c935-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8c935-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8c935-112">Le modèle de projet d’ASP.NET Core 1.x **Application Web** a une référence de package pour le package [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span><span class="sxs-lookup"><span data-stu-id="8c935-112">The ASP.NET Core 1.x **Web Application** project template has a package reference for the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package.</span></span> <span data-ttu-id="8c935-113">Pour les modèles de projet **Vide** ou **API Web**, vous devez ajouter une référence de package à `Microsoft.VisualStudio.Web.BrowserLink`.</span><span class="sxs-lookup"><span data-stu-id="8c935-113">The **Empty** or **Web API** template projects require you to add a package reference to `Microsoft.VisualStudio.Web.BrowserLink`.</span></span>

<span data-ttu-id="8c935-114">Dans la mesure où il s'agit d'une fonctionnalité Visual Studio, la meilleure façon d'ajouter le package à un modèle de projet **Vide** ou **API Web** consiste à ouvrir la **Console du Gestionnaire de package** (**Affichage**  >   **Autres fenêtres**  >  **Console du Gestionnaire de package**) et à exécuter la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="8c935-114">Since this is a Visual Studio feature, the easiest way to add the package to an **Empty** or **Web API** template project is to open the **Package Manager Console** (**View** > **Other Windows** > **Package Manager Console**) and run the following command:</span></span>

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

<span data-ttu-id="8c935-115">Vous pouvez également utiliser le **Gestionnaire de package NuGet**. </span><span class="sxs-lookup"><span data-stu-id="8c935-115">Alternatively, you can use **NuGet Package Manager**.</span></span> <span data-ttu-id="8c935-116">Cliquez sur le nom du projet dans **l’Explorateur de solutions** et choisissez **Gérer les packages NuGet** :</span><span class="sxs-lookup"><span data-stu-id="8c935-116">Right-click the project name in **Solution Explorer** and choose **Manage NuGet Packages**:</span></span>

![Gestionnaire de Package NuGet ouvert](using-browserlink/_static/open-nuget-package-manager.png)

<span data-ttu-id="8c935-118">Recherchez et installez le package :</span><span class="sxs-lookup"><span data-stu-id="8c935-118">Find and install the package:</span></span>

![Ajouter un package avec le Gestionnaire de Package NuGet](using-browserlink/_static/add-package-with-nuget-package-manager.png)

---

### <a name="configuration"></a><span data-ttu-id="8c935-120">Configuration</span><span class="sxs-lookup"><span data-stu-id="8c935-120">Configuration</span></span>

<span data-ttu-id="8c935-121">Dans la méthode `Configure` du fichier *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="8c935-121">In the `Configure` method of the *Startup.cs* file:</span></span>

```csharp
app.UseBrowserLink();
```

<span data-ttu-id="8c935-122">Le code se trouve généralement à l’intérieur d’un bloc `if` qui permet uniquement d'utiliser le lien du navigateur dans l’environnement de développement, comme indiqué ici :</span><span class="sxs-lookup"><span data-stu-id="8c935-122">Usually the code is inside an `if` block that only enables Browser Link in the Development environment, as shown here:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

<span data-ttu-id="8c935-123">Pour plus d’informations, consultez [travailler avec plusieurs environnements](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="8c935-123">For more information, see [Work with Multiple Environments](xref:fundamentals/environments).</span></span>

## <a name="how-to-use-browser-link"></a><span data-ttu-id="8c935-124">Comment utiliser le lien du navigateur</span><span class="sxs-lookup"><span data-stu-id="8c935-124">How to use Browser Link</span></span>

<span data-ttu-id="8c935-125">Quand vous avez un projet ASP.NET Core ouvert, Visual Studio affiche le contrôle de barre d’outils Lien du navigateur à côté du contrôle de barre d’outils **Cible de débogage** :</span><span class="sxs-lookup"><span data-stu-id="8c935-125">When you have an ASP.NET Core project open, Visual Studio shows the Browser Link toolbar control next to the **Debug Target** toolbar control:</span></span>

![Menu de liste déroulante de lien de navigateur](using-browserlink/_static/browserLink-dropdown-menu.png)

<span data-ttu-id="8c935-127">À partir du contrôle de barre d’outils Lien du navigateur, vous pouvez :</span><span class="sxs-lookup"><span data-stu-id="8c935-127">From the Browser Link toolbar control, you can:</span></span>

* <span data-ttu-id="8c935-128">Actualiser l’application web dans plusieurs navigateurs à la fois.</span><span class="sxs-lookup"><span data-stu-id="8c935-128">Refresh the web application in several browsers at once.</span></span>
* <span data-ttu-id="8c935-129">Ouvrir le **tableau de bord Lien du navigateur**.</span><span class="sxs-lookup"><span data-stu-id="8c935-129">Open the **Browser Link Dashboard**.</span></span>
* <span data-ttu-id="8c935-130">Activer ou désactiver le **lien du navigateur**.</span><span class="sxs-lookup"><span data-stu-id="8c935-130">Enable or disable **Browser Link**.</span></span> <span data-ttu-id="8c935-131">Remarque : Le lien du navigateur est désactivé par défaut dans Visual Studio 2017 (15.3).</span><span class="sxs-lookup"><span data-stu-id="8c935-131">Note: Browser Link is disabled by default in Visual Studio 2017 (15.3).</span></span>
* <span data-ttu-id="8c935-132">Activer ou désactiver [la synchronisation automatique CSS](#enable-or-disable-css-auto-sync).</span><span class="sxs-lookup"><span data-stu-id="8c935-132">Enable or disable [CSS Auto-Sync](#enable-or-disable-css-auto-sync).</span></span>

> [!NOTE]
> <span data-ttu-id="8c935-133">Certains plug-ins Visual Studio, notamment *Web Extension Pack 2015* et *Web Extension Pack 2017*, proposent des fonctionnalités étendues pour le lien du navigateur, mais certaines de ces fonctionnalités ne fonctionnent pas avec ASP. Projets NET Core.</span><span class="sxs-lookup"><span data-stu-id="8c935-133">Some Visual Studio plug-ins, most notably *Web Extension Pack 2015* and *Web Extension Pack 2017*, offer extended functionality for Browser Link, but some of the additional features don't work with ASP.NET Core projects.</span></span>

## <a name="refresh-the-web-application-in-several-browsers-at-once"></a><span data-ttu-id="8c935-134">Actualiser à la fois l’application web dans plusieurs navigateurs</span><span class="sxs-lookup"><span data-stu-id="8c935-134">Refresh the web application in several browsers at once</span></span>

<span data-ttu-id="8c935-135">Pour choisir un navigateur web unique à lancer au démarrage du projet, utilisez le menu déroulant du contrôle de barre d'outils **Cible de débogage** :</span><span class="sxs-lookup"><span data-stu-id="8c935-135">To choose a single web browser to launch when starting the project, use the drop-down menu in the **Debug Target** toolbar control:</span></span>

![Menu déroulant de F5](using-browserlink/_static/debug-target-dropdown-menu.png)

<span data-ttu-id="8c935-137">Pour ouvrir plusieurs navigateurs à la fois, choisissez **Naviguer avec...** dans la même liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="8c935-137">To open multiple browsers at once, choose **Browse with...** from the same drop-down.</span></span> <span data-ttu-id="8c935-138">Maintenez la touche Ctrl enfoncée pour sélectionner les navigateurs de votre choix, puis cliquez sur **Parcourir** :</span><span class="sxs-lookup"><span data-stu-id="8c935-138">Hold down the CTRL key to select the browsers you want, and then click **Browse**:</span></span>

![Ouvrir à la fois des nombreux navigateurs](using-browserlink/_static/open-many-browsers-at-once.png)

<span data-ttu-id="8c935-140">Voici une capture d’écran montrant Visual Studio avec la vue Index ouverte et deux navigateurs ouverts :</span><span class="sxs-lookup"><span data-stu-id="8c935-140">Here's a screenshot showing Visual Studio with the Index view open and two open browsers:</span></span>

![Synchroniser avec l’exemple de deux navigateurs](using-browserlink/_static/sync-with-two-browsers-example.png)

<span data-ttu-id="8c935-142">Placez le curseur sur la barre d’outils du lien du navigateur pour voir les navigateurs connectés au projet :</span><span class="sxs-lookup"><span data-stu-id="8c935-142">Hover over the Browser Link toolbar control to see the browsers that are connected to the project:</span></span>

![Placez le curseur pointe](using-browserlink/_static/hoover-tip.png)

<span data-ttu-id="8c935-144">Changez l’affichage de l’index. Tous les navigateurs connectés sont mis à jour quand vous cliquez sur le bouton d’actualisation du lien du navigateur :</span><span class="sxs-lookup"><span data-stu-id="8c935-144">Change the Index view, and all connected browsers are updated when you click the Browser Link refresh button:</span></span>

![navigateurs-sync-changes](using-browserlink/_static/browsers-sync-to-changes.png)

<span data-ttu-id="8c935-146">Le lien du navigateur fonctionne également avec les navigateurs lancés en dehors de Visual Studio et accessibles au moyen de l’URL de l’application.</span><span class="sxs-lookup"><span data-stu-id="8c935-146">Browser Link also works with browsers that you launch from outside Visual Studio and navigate to the application URL.</span></span>

### <a name="the-browser-link-dashboard"></a><span data-ttu-id="8c935-147">Le tableau de bord de lien de navigateur</span><span class="sxs-lookup"><span data-stu-id="8c935-147">The Browser Link Dashboard</span></span>

<span data-ttu-id="8c935-148">Dans la liste déroulante de lien du navigateur vers le bas du menu pour gérer les connexions avec les navigateurs ouverts, ouvrez le tableau de bord de lien de navigateur :</span><span class="sxs-lookup"><span data-stu-id="8c935-148">Open the Browser Link Dashboard from the Browser Link drop down menu to manage the connection with open browsers:</span></span>

![Ouvrir-browserslink-tableau de bord](using-browserlink/_static/open-browserlink-dashboard.png)

<span data-ttu-id="8c935-150">Si aucun navigateur n’est connecté, vous pouvez démarrer une session autre qu'une session de débogage en sélectionnant le lien *Afficher dans le navigateur* :</span><span class="sxs-lookup"><span data-stu-id="8c935-150">If no browser is connected, you can start a non-debugging session by selecting the *View in Browser* link:</span></span>

![browserlink-dashboard-no-connections](using-browserlink/_static/browserlink-dashboard-no-connections.png)

<span data-ttu-id="8c935-152">Dans le cas contraire, les navigateurs connectés sont affichés avec le chemin d’accès à la page qui affiche chaque navigateur :</span><span class="sxs-lookup"><span data-stu-id="8c935-152">Otherwise, the connected browsers are shown with the path to the page that each browser is showing:</span></span>

![browserlink-dashboard-two-connections](using-browserlink/_static/browserlink-dashboard-two-connections.png)

<span data-ttu-id="8c935-154">Si vous le souhaitez, vous pouvez cliquer sur un nom de navigateur répertorié pour actualiser seulement ce navigateur.</span><span class="sxs-lookup"><span data-stu-id="8c935-154">If you like, you can click on a listed browser name to refresh that single browser.</span></span>

### <a name="enable-or-disable-browser-link"></a><span data-ttu-id="8c935-155">Activer ou désactiver le lien du navigateur</span><span class="sxs-lookup"><span data-stu-id="8c935-155">Enable or disable Browser Link</span></span>

<span data-ttu-id="8c935-156">Quand vous réactivez le lien du navigateur après l’avoir désactivé, vous devez actualiser les navigateurs pour les reconnecter.</span><span class="sxs-lookup"><span data-stu-id="8c935-156">When you re-enable Browser Link after disabling it, you must refresh the browsers to reconnect them.</span></span>

### <a name="enable-or-disable-css-auto-sync"></a><span data-ttu-id="8c935-157">Activer ou désactiver la synchronisation automatique CSS</span><span class="sxs-lookup"><span data-stu-id="8c935-157">Enable or disable CSS Auto-Sync</span></span>

<span data-ttu-id="8c935-158">Lorsque la synchronisation automatique CSS est activée, les navigateurs connectés sont automatiquement actualisées lorsque vous modifiez des fichiers CSS.</span><span class="sxs-lookup"><span data-stu-id="8c935-158">When CSS Auto-Sync is enabled, connected browsers are automatically refreshed when you make any change to CSS files.</span></span>

## <a name="how-does-it-work"></a><span data-ttu-id="8c935-159">Comment cela fonctionne-t-il ?</span><span class="sxs-lookup"><span data-stu-id="8c935-159">How does it work?</span></span>

<span data-ttu-id="8c935-160">Browser Link utilise SignalR pour créer un canal de communication entre Visual Studio et le navigateur.</span><span class="sxs-lookup"><span data-stu-id="8c935-160">Browser Link uses SignalR to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="8c935-161">Quand le lien du navigateur est activé, Visual Studio fait office de serveur SignalR auquel plusieurs clients (navigateurs) peuvent se connecter. </span><span class="sxs-lookup"><span data-stu-id="8c935-161">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="8c935-162">Browser Link enregistre également un composant d’intergiciel (middleware) dans le pipeline de demande ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8c935-162">Browser Link also registers a middleware component in the ASP.NET request pipeline.</span></span> <span data-ttu-id="8c935-163">Ce composant injecte des références `<script>` dans chaque demande de page à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="8c935-163">This component injects special `<script>` references into every page request from the server.</span></span> <span data-ttu-id="8c935-164">Pour voir les références de script, sélectionnez **Afficher la source** dans le navigateur et faites défiler jusqu’à la fin du contenu de la balise `<body>` :</span><span class="sxs-lookup"><span data-stu-id="8c935-164">You can see the script references by selecting **View source** in the browser and scrolling to the end of the `<body>` tag content:</span></span>

```javascript
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

<span data-ttu-id="8c935-165">Vos fichiers sources ne sont pas modifiés.</span><span class="sxs-lookup"><span data-stu-id="8c935-165">Your source files aren't modified.</span></span> <span data-ttu-id="8c935-166">Le composant d’intergiciel (middleware) injecte les références de script dynamiquement.</span><span class="sxs-lookup"><span data-stu-id="8c935-166">The middleware component injects the script references dynamically.</span></span> 

<span data-ttu-id="8c935-167">Le code côté navigateur étant uniquement du code JavaScript, il fonctionne sur tous les navigateurs prenant en charge SignalR sans nécessiter un plug-in de navigateur.</span><span class="sxs-lookup"><span data-stu-id="8c935-167">Because the browser-side code is all JavaScript, it works on all browsers that SignalR supports without requiring a browser plug-in.</span></span>
