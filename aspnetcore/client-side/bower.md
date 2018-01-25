---
title: "À l’aide de Bower dans ASP.NET Core"
author: rick-anderson
description: "Gestion des packages Bower côté client."
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/bower
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7e3e936c81126b7ed01332565f997910a2886993
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a><span data-ttu-id="1cdec-103">Gérer les packages côté client avec Bower dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1cdec-103">Manage client-side packages with Bower in ASP.NET Core</span></span>

<span data-ttu-id="1cdec-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel riz](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), et [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="1cdec-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel Rice](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), and [Scott Addie](https://scottaddie.com)</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="1cdec-105">Bower est conservée, ils vous recommandons à l’aide d’une autre solution.</span><span class="sxs-lookup"><span data-stu-id="1cdec-105">While Bower is maintained, they recommend using a different solution.</span></span> <span data-ttu-id="1cdec-106">Fil avec Webpack est une solution courante pour laquelle [instructions de migration](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) sont disponibles.</span><span class="sxs-lookup"><span data-stu-id="1cdec-106">Yarn with Webpack is one popular alternative for which [migration instructions](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) are available.</span></span>

<span data-ttu-id="1cdec-107">[Bower](https://bower.io/) s’appelle elle-même « Un gestionnaire de package pour le web ».</span><span class="sxs-lookup"><span data-stu-id="1cdec-107">[Bower](https://bower.io/) calls itself "A package manager for the web".</span></span> <span data-ttu-id="1cdec-108">Dans l’écosystème .NET, il remplit le vide laissé par l’impossibilité de NuGet pour remettre les fichiers de contenu statique.</span><span class="sxs-lookup"><span data-stu-id="1cdec-108">Within the .NET ecosystem, it fills the void left by NuGet’s inability to deliver static content files.</span></span> <span data-ttu-id="1cdec-109">Pour les projets ASP.NET Core, ces fichiers statiques sont inhérents aux bibliothèques côté client comme [jQuery](http://jquery.com/) et [Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="1cdec-109">For ASP.NET Core projects, these static files are inherent to client-side libraries like [jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/).</span></span> <span data-ttu-id="1cdec-110">Pour les bibliothèques .NET, vous utilisez toujours [NuGet](https://www.nuget.org/) Gestionnaire de package.</span><span class="sxs-lookup"><span data-stu-id="1cdec-110">For .NET libraries, you still use [NuGet](https://www.nuget.org/) package manager.</span></span>

<span data-ttu-id="1cdec-111">Processus de génération de nouveaux projets créés avec les modèles de projet ASP.NET Core configuration côté client.</span><span class="sxs-lookup"><span data-stu-id="1cdec-111">New projects created with the ASP.NET Core project templates set up the client-side build process.</span></span> <span data-ttu-id="1cdec-112">[jQuery](http://jquery.com/) et [Bootstrap](http://getbootstrap.com/) sont installés, et Bower est pris en charge.</span><span class="sxs-lookup"><span data-stu-id="1cdec-112">[jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/) are installed, and Bower is supported.</span></span>

<span data-ttu-id="1cdec-113">Les packages côté client sont répertoriés dans le *bower.json* fichier.</span><span class="sxs-lookup"><span data-stu-id="1cdec-113">Client-side packages are listed in the *bower.json* file.</span></span> <span data-ttu-id="1cdec-114">Les modèles de projet ASP.NET Core configure *bower.json* avec jQuery, jQuery validation et d’amorçage.</span><span class="sxs-lookup"><span data-stu-id="1cdec-114">The ASP.NET Core project templates configures *bower.json* with jQuery, jQuery validation, and Bootstrap.</span></span>

<span data-ttu-id="1cdec-115">Dans ce didacticiel, nous allons ajouter la prise en charge de [police impressionnant](http://fontawesome.io).</span><span class="sxs-lookup"><span data-stu-id="1cdec-115">In this tutorial, we'll add support for [Font Awesome](http://fontawesome.io).</span></span> <span data-ttu-id="1cdec-116">Bower packages peuvent être installés avec le **gérer les Packages Bower** UI ou manuellement dans le *bower.json* fichier.</span><span class="sxs-lookup"><span data-stu-id="1cdec-116">Bower packages can be installed with the **Manage Bower Packages** UI or manually in the *bower.json* file.</span></span>

### <a name="installation-via-manage-bower-packages-ui"></a><span data-ttu-id="1cdec-117">Installation via gérer les Packages Bower l’interface utilisateur</span><span class="sxs-lookup"><span data-stu-id="1cdec-117">Installation via Manage Bower Packages UI</span></span>

* <span data-ttu-id="1cdec-118">Créer une application ASP.NET Core Web avec le **Application ASP.NET Core Web (.NET Core)** modèle.</span><span class="sxs-lookup"><span data-stu-id="1cdec-118">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="1cdec-119">Sélectionnez **Application Web** et **aucune authentification**.</span><span class="sxs-lookup"><span data-stu-id="1cdec-119">Select **Web Application** and **No Authentication**.</span></span>

* <span data-ttu-id="1cdec-120">Cliquez sur le projet dans l’Explorateur de solutions et sélectionnez **gérer les Packages Bower** (également dans le menu principal, **projet** > **gérer les Packages Bower**).</span><span class="sxs-lookup"><span data-stu-id="1cdec-120">Right-click the project in Solution Explorer and select **Manage Bower Packages** (alternatively from the main menu, **Project** > **Manage Bower Packages**).</span></span>

* <span data-ttu-id="1cdec-121">Dans le **Bower : \<nom du projet\>**  fenêtre, cliquez sur l’onglet « Parcourir », puis filtrer la liste de packages en entrant `font-awesome` dans la zone de recherche :</span><span class="sxs-lookup"><span data-stu-id="1cdec-121">In the **Bower: \<project name\>** window, click the "Browse" tab, and then filter the packages list by entering `font-awesome` in the search box:</span></span>

 ![gérer les packages bower](bower/_static/manage-bower-packages.png)

* <span data-ttu-id="1cdec-123">Vérifiez que le « enregistrer les modifications *bower.json*« case à cocher est activée.</span><span class="sxs-lookup"><span data-stu-id="1cdec-123">Confirm that the "Save changes to *bower.json*" checkbox is checked.</span></span> <span data-ttu-id="1cdec-124">Sélectionnez une version dans la liste déroulante et cliquez sur le **installer** bouton.</span><span class="sxs-lookup"><span data-stu-id="1cdec-124">Select a version from the drop-down list and click the **Install** button.</span></span> <span data-ttu-id="1cdec-125">Le **sortie** fenêtre affiche les détails d’installation.</span><span class="sxs-lookup"><span data-stu-id="1cdec-125">The **Output** window shows the installation details.</span></span>

### <a name="manual-installation-in-bowerjson"></a><span data-ttu-id="1cdec-126">Installation manuelle de bower.json</span><span class="sxs-lookup"><span data-stu-id="1cdec-126">Manual installation in bower.json</span></span>

<span data-ttu-id="1cdec-127">Ouvrez le *bower.json* et ajoutez « police impressionnant » aux dépendances.</span><span class="sxs-lookup"><span data-stu-id="1cdec-127">Open the *bower.json* file and add "font-awesome" to the dependencies.</span></span> <span data-ttu-id="1cdec-128">IntelliSense affiche les packages disponibles.</span><span class="sxs-lookup"><span data-stu-id="1cdec-128">IntelliSense shows the available packages.</span></span> <span data-ttu-id="1cdec-129">Lorsqu’un package est sélectionné, les versions disponibles sont affichées.</span><span class="sxs-lookup"><span data-stu-id="1cdec-129">When a package is selected, the available versions are displayed.</span></span> <span data-ttu-id="1cdec-130">Les images ci-dessous sont plus anciennes et ne correspond pas à ce que vous voyez.</span><span class="sxs-lookup"><span data-stu-id="1cdec-130">The images below are older and won't match what you see.</span></span>

![IntelliSense de l’Explorateur de package bower](bower/_static/add-package.png)

![version de bower IntelliSense](bower/_static/version-intelliSense.png)

<span data-ttu-id="1cdec-133">Bower utilise [contrôle de version sémantique](http://semver.org/) pour organiser les dépendances.</span><span class="sxs-lookup"><span data-stu-id="1cdec-133">Bower uses [semantic versioning](http://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="1cdec-134">Contrôle de version sémantique, également appelé SemVer, identifie les packages avec le modèle de numérotation \<majeure >.\< mineure >. \<correctif >.</span><span class="sxs-lookup"><span data-stu-id="1cdec-134">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme \<major>.\<minor>.\<patch>.</span></span> <span data-ttu-id="1cdec-135">IntelliSense simplifie le contrôle de version sémantique en affichant uniquement quelques choix courants.</span><span class="sxs-lookup"><span data-stu-id="1cdec-135">IntelliSense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="1cdec-136">Le premier élément dans la liste IntelliSense (4.6.3 dans l’exemple ci-dessus) est considéré comme la dernière version stable du package.</span><span class="sxs-lookup"><span data-stu-id="1cdec-136">The top item in the IntelliSense list (4.6.3 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="1cdec-137">Le symbole du signe insertion (^) correspond à la version majeure la plus récente et le tilde (~) correspond à la version mineure la plus récente.</span><span class="sxs-lookup"><span data-stu-id="1cdec-137">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span>

<span data-ttu-id="1cdec-138">Enregistrer le *bower.json* fichier.</span><span class="sxs-lookup"><span data-stu-id="1cdec-138">Save the *bower.json* file.</span></span> <span data-ttu-id="1cdec-139">Visual Studio surveille le *bower.json* des modifications.</span><span class="sxs-lookup"><span data-stu-id="1cdec-139">Visual Studio watches the *bower.json* file for changes.</span></span> <span data-ttu-id="1cdec-140">Lors de l’enregistrement, la *bower installation* commande est exécutée.</span><span class="sxs-lookup"><span data-stu-id="1cdec-140">Upon saving, the *bower install* command is executed.</span></span> <span data-ttu-id="1cdec-141">Consultez la fenêtre de sortie **Bower/npm** vue pour la commande exacte exécutée.</span><span class="sxs-lookup"><span data-stu-id="1cdec-141">See the Output window's **Bower/npm** view for the exact command executed.</span></span>

<span data-ttu-id="1cdec-142">Ouvrez le *.bowerrc* de fichiers sous *bower.json*.</span><span class="sxs-lookup"><span data-stu-id="1cdec-142">Open the *.bowerrc* file under *bower.json*.</span></span> <span data-ttu-id="1cdec-143">Le `directory` est définie sur *wwwroot/lib* qui indique l’emplacement Bower installera les composants de package.</span><span class="sxs-lookup"><span data-stu-id="1cdec-143">The `directory` property is set to *wwwroot/lib* which indicates the location Bower will install the package assets.</span></span>

```json
{
 "directory": "wwwroot/lib"
}
```

<span data-ttu-id="1cdec-144">Vous pouvez utiliser la zone de recherche dans l’Explorateur de solutions pour rechercher et afficher le package impressionnant de police.</span><span class="sxs-lookup"><span data-stu-id="1cdec-144">You can use the search box in Solution Explorer to find and display the font-awesome package.</span></span>

<span data-ttu-id="1cdec-145">Ouvrez le *Views\Shared\_Layout.cshtml* et ajoutez le fichier CSS impressionnant de police à l’environnement [application d’assistance de balise](xref:mvc/views/tag-helpers/intro) pour `Development`.</span><span class="sxs-lookup"><span data-stu-id="1cdec-145">Open the *Views\Shared\_Layout.cshtml* file and add the font-awesome CSS file to the environment [Tag Helper](xref:mvc/views/tag-helpers/intro) for `Development`.</span></span> <span data-ttu-id="1cdec-146">À partir de l’Explorateur de solutions, faites glisser et déposez *awesome.css de police* à l’intérieur du `<environment names="Development">` élément.</span><span class="sxs-lookup"><span data-stu-id="1cdec-146">From Solution Explorer, drag and drop *font-awesome.css* inside the `<environment names="Development">` element.</span></span>

[!code-html[Main](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]

<span data-ttu-id="1cdec-147">Dans une application de production, vous ajouteriez *awesome.min.css de police* dans l’application d’assistance de balise environnement pour `Staging,Production`.</span><span class="sxs-lookup"><span data-stu-id="1cdec-147">In a production app you would add *font-awesome.min.css* to the environment tag helper for `Staging,Production`.</span></span>

<span data-ttu-id="1cdec-148">Remplacez le contenu de la *Views\Home\About.cshtml* fichier Razor avec le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="1cdec-148">Replace the contents of the *Views\Home\About.cshtml* Razor file with the following markup:</span></span>

[!code-html[Main](bower/sample/About.cshtml)]

<span data-ttu-id="1cdec-149">Exécutez l’application et accédez à la vue à propos de pour vérifier le fonctionnement du package impressionnant de police.</span><span class="sxs-lookup"><span data-stu-id="1cdec-149">Run the app and navigate to the About view to verify the font-awesome package works.</span></span>

## <a name="exploring-the-client-side-build-process"></a><span data-ttu-id="1cdec-150">Explorer le processus de génération du côté client</span><span class="sxs-lookup"><span data-stu-id="1cdec-150">Exploring the client-side build process</span></span>

<span data-ttu-id="1cdec-151">La plupart des modèles de projet ASP.NET Core sont déjà configurés pour utiliser Bower.</span><span class="sxs-lookup"><span data-stu-id="1cdec-151">Most ASP.NET Core project templates are already configured to use Bower.</span></span> <span data-ttu-id="1cdec-152">Cette procédure pas à pas suivante commence par un projet ASP.NET Core vide et ajoute chaque élément manuellement, vous pouvez obtenir une idée pour l’utilisation de Bower dans un projet.</span><span class="sxs-lookup"><span data-stu-id="1cdec-152">This next walkthrough starts with an empty ASP.NET Core project and adds each piece manually, so you can get a feel for how Bower is used in a project.</span></span> <span data-ttu-id="1cdec-153">Vous pouvez voir ce qui arrive à la structure de projet et l’exécution de chaque configuration est modifié de sortie.</span><span class="sxs-lookup"><span data-stu-id="1cdec-153">You can see what happens to the project structure and the runtime output as each configuration change is made.</span></span>

<span data-ttu-id="1cdec-154">Les étapes générales à utiliser le processus de génération de côté client avec Bower sont :</span><span class="sxs-lookup"><span data-stu-id="1cdec-154">The general steps to use the client-side build process with Bower are:</span></span>

* <span data-ttu-id="1cdec-155">Définir les packages utilisés dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="1cdec-155">Define packages used in your project.</span></span> <!-- once defined, you don't need to download them, VS does -->
* <span data-ttu-id="1cdec-156">Référence à des packages à partir de vos pages web.</span><span class="sxs-lookup"><span data-stu-id="1cdec-156">Reference packages from your web pages.</span></span>

### <a name="define-packages"></a><span data-ttu-id="1cdec-157">Définir des packages</span><span class="sxs-lookup"><span data-stu-id="1cdec-157">Define packages</span></span>

<span data-ttu-id="1cdec-158">Une fois que la liste de packages dans le *bower.json* fichier, Visual Studio permet de le télécharger.</span><span class="sxs-lookup"><span data-stu-id="1cdec-158">Once you list packages in the *bower.json* file, Visual Studio will download them.</span></span> <span data-ttu-id="1cdec-159">L’exemple suivant utilise Bower pour charger jQuery et amorçage pour le *wwwroot* dossier.</span><span class="sxs-lookup"><span data-stu-id="1cdec-159">The following example uses Bower to load jQuery and Bootstrap to the *wwwroot* folder.</span></span>

* <span data-ttu-id="1cdec-160">Créer une application ASP.NET Core Web avec le **Application ASP.NET Core Web (.NET Core)** modèle.</span><span class="sxs-lookup"><span data-stu-id="1cdec-160">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="1cdec-161">Sélectionnez le **vide** modèle de projet et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="1cdec-161">Select the **Empty** project template and click **OK**.</span></span>

* <span data-ttu-id="1cdec-162">Dans l’Explorateur de solutions, cliquez sur le projet > **ajouter un nouvel élément** et sélectionnez **fichier de Configuration Bower**.</span><span class="sxs-lookup"><span data-stu-id="1cdec-162">In Solution Explorer, right-click the project > **Add New Item** and select **Bower Configuration File**.</span></span> <span data-ttu-id="1cdec-163">Remarque : Un *.bowerrc* fichier est également ajouté.</span><span class="sxs-lookup"><span data-stu-id="1cdec-163">Note: A *.bowerrc* file is also added.</span></span>

* <span data-ttu-id="1cdec-164">Ouvrez *bower.json*et ajouter de jquery et données d’amorçage pour le `dependencies` section.</span><span class="sxs-lookup"><span data-stu-id="1cdec-164">Open *bower.json*, and add jquery and bootstrap to the `dependencies` section.</span></span> <span data-ttu-id="1cdec-165">Résultant *bower.json* fichier ressemblera à l’exemple suivant.</span><span class="sxs-lookup"><span data-stu-id="1cdec-165">The resulting *bower.json* file will look like the following example.</span></span> <span data-ttu-id="1cdec-166">Les versions changent au fil du temps et ne peuvent pas correspondre à l’image ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="1cdec-166">The versions will change over time and may not match the image below.</span></span>

[!code-json[Main](bower/sample/bower.json?highlight=5,6)]

* <span data-ttu-id="1cdec-167">Enregistrer le *bower.json* fichier.</span><span class="sxs-lookup"><span data-stu-id="1cdec-167">Save the *bower.json* file.</span></span>

 <span data-ttu-id="1cdec-168">Vérifiez que le projet inclut le *amorçage* et *jQuery* répertoires dans *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="1cdec-168">Verify the project includes the *bootstrap* and *jQuery* directories in *wwwroot/lib*.</span></span> <span data-ttu-id="1cdec-169">Bower utilise le *.bowerrc* fichier pour installer les composants dans *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="1cdec-169">Bower uses the *.bowerrc* file to install the assets in *wwwroot/lib*.</span></span>

 <span data-ttu-id="1cdec-170">Remarque : L’interface utilisateur « Gérer les Packages Bower » offre une alternative à la modification du fichier manuelle.</span><span class="sxs-lookup"><span data-stu-id="1cdec-170">Note: The "Manage Bower Packages" UI provides an alternative to manual file editing.</span></span>

### <a name="enable-static-files"></a><span data-ttu-id="1cdec-171">Activer les fichiers statiques</span><span class="sxs-lookup"><span data-stu-id="1cdec-171">Enable static files</span></span>

* <span data-ttu-id="1cdec-172">Ajouter le `Microsoft.AspNetCore.StaticFiles` package NuGet pour le projet.</span><span class="sxs-lookup"><span data-stu-id="1cdec-172">Add the `Microsoft.AspNetCore.StaticFiles` NuGet package to the project.</span></span>
* <span data-ttu-id="1cdec-173">Activer des fichiers statiques avec la [intergiciel (middleware) du fichier statique](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions).</span><span class="sxs-lookup"><span data-stu-id="1cdec-173">Enable static files to be served with the [Static file middleware](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions).</span></span> <span data-ttu-id="1cdec-174">Ajoutez un appel à [UseStaticFiles](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions) à la `Configure` méthode `Startup`.</span><span class="sxs-lookup"><span data-stu-id="1cdec-174">Add a call to [UseStaticFiles](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions) to the `Configure` method of `Startup`.</span></span>

[!code-csharp[Main](bower/sample/Startup.cs?highlight=9)]

### <a name="reference-packages"></a><span data-ttu-id="1cdec-175">Packages de référence</span><span class="sxs-lookup"><span data-stu-id="1cdec-175">Reference packages</span></span>

<span data-ttu-id="1cdec-176">Dans cette section, vous allez créer une page HTML pour vérifier qu’il peut accéder aux packages déployés.</span><span class="sxs-lookup"><span data-stu-id="1cdec-176">In this section, you will create an HTML page to verify it can access the deployed packages.</span></span>

* <span data-ttu-id="1cdec-177">Ajouter une nouvelle page HTML nommée *Index.html* à la *wwwroot* dossier.</span><span class="sxs-lookup"><span data-stu-id="1cdec-177">Add a new HTML page named *Index.html* to the *wwwroot* folder.</span></span> <span data-ttu-id="1cdec-178">Remarque : Vous devez ajouter le fichier HTML pour le *wwwroot* dossier.</span><span class="sxs-lookup"><span data-stu-id="1cdec-178">Note: You must add the HTML file to the *wwwroot* folder.</span></span> <span data-ttu-id="1cdec-179">Par défaut, le contenu statique ne peut pas être traitée en dehors de *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="1cdec-179">By default, static content cannot be served outside *wwwroot*.</span></span> <span data-ttu-id="1cdec-180">Consultez [utilisation des fichiers statiques](xref:fundamentals/static-files) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="1cdec-180">See [Working with static files](xref:fundamentals/static-files) for more information.</span></span>

 <span data-ttu-id="1cdec-181">Remplacez le contenu de *Index.html* par le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="1cdec-181">Replace the contents of *Index.html* with the following markup:</span></span>

[!code-html[Main](bower/sample/Index.html)]

* <span data-ttu-id="1cdec-182">Exécutez l’application et accédez à `http://localhost:<port>/Index.html`.</span><span class="sxs-lookup"><span data-stu-id="1cdec-182">Run the app and navigate to `http://localhost:<port>/Index.html`.</span></span> <span data-ttu-id="1cdec-183">Vous pouvez également avec *Index.html* ouvert, appuyez sur `Ctrl+Shift+W`.</span><span class="sxs-lookup"><span data-stu-id="1cdec-183">Alternatively, with *Index.html* opened, press `Ctrl+Shift+W`.</span></span> <span data-ttu-id="1cdec-184">Vérifiez que le style jumbotron est appliqué, le code jQuery répond quand le bouton est activé, et que le bouton d’amorçage modifie l’état.</span><span class="sxs-lookup"><span data-stu-id="1cdec-184">Verify that the jumbotron styling is applied, the jQuery code responds when the button is clicked, and that the Bootstrap button changes state.</span></span>

 ![style de JumboTron appliqué](bower/_static/jumbotron.png)
