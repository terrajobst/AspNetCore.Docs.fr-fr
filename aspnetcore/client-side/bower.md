---
title: Gérer les dépendances côté client avec Bower dans ASP.NET Core
author: rick-anderson
description: Gestion des packages Bower côté client.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bower
ms.openlocfilehash: 2d6cc526b5a0890103e2856a0ca4b58c5f162c79
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a><span data-ttu-id="ac26e-103">Gérer les dépendances côté client avec Bower dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ac26e-103">Manage client-side packages with Bower in ASP.NET Core</span></span>

<span data-ttu-id="ac26e-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel riz](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), et [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="ac26e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel Rice](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), and [Scott Addie](https://scottaddie.com)</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="ac26e-105">Tandis que Bower est conservé, son chargés de maintenance est recommandé d’utiliser une autre solution.</span><span class="sxs-lookup"><span data-stu-id="ac26e-105">While Bower is maintained, its maintainers recommend using a different solution.</span></span> <span data-ttu-id="ac26e-106">[Le Gestionnaire de bibliothèque](https://blogs.msdn.microsoft.com/webdev/2018/04/18/what-happened-to-bower/) (LibMan en abrégé) est le nouveau système de gestion de contenu statique côté client de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ac26e-106">[Library Manager](https://blogs.msdn.microsoft.com/webdev/2018/04/18/what-happened-to-bower/) (LibMan for short) is Visual Studio's new client-side static content management system.</span></span> <span data-ttu-id="ac26e-107">L'utilisation de Yarn avec Webpack constitue une alternative courante pour laquelle des [instructions de migration](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) sont disponibles.</span><span class="sxs-lookup"><span data-stu-id="ac26e-107">Yarn with Webpack is one popular alternative for which [migration instructions](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) are available.</span></span>

<span data-ttu-id="ac26e-108">[Bower](https://bower.io/) se définit lui-même comme "Un gestionnaire de paquets pour le web".</span><span class="sxs-lookup"><span data-stu-id="ac26e-108">[Bower](https://bower.io/) calls itself "A package manager for the web".</span></span> <span data-ttu-id="ac26e-109">Dans l’écosystème .NET, il remplit le vide laissé par l’impossibilité d'inclure des fichiers de contenu statique avec NuGet.</span><span class="sxs-lookup"><span data-stu-id="ac26e-109">Within the .NET ecosystem, it fills the void left by NuGet's inability to deliver static content files.</span></span> <span data-ttu-id="ac26e-110">Pour les projets ASP.NET Core, ces fichiers statiques sont inhérents aux bibliothèques côté client comme[jQuery](http://jquery.com/) et [Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="ac26e-110">For ASP.NET Core projects, these static files are inherent to client-side libraries like [jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/).</span></span> <span data-ttu-id="ac26e-111">Pour les bibliothèques .NET, vous utilisez toujours le gestionnaire de package [NuGet](https://www.nuget.org/)</span><span class="sxs-lookup"><span data-stu-id="ac26e-111">For .NET libraries, you still use [NuGet](https://www.nuget.org/) package manager.</span></span>

<span data-ttu-id="ac26e-112">Les nouveaux projets créés avec les modèles de projet ASP.NET Core sont configurés avec la génération côté client.</span><span class="sxs-lookup"><span data-stu-id="ac26e-112">New projects created with the ASP.NET Core project templates set up the client-side build process.</span></span> <span data-ttu-id="ac26e-113">[jQuery](http://jquery.com/) et [Bootstrap](http://getbootstrap.com/) sont installés, et Bower est pris en charge.</span><span class="sxs-lookup"><span data-stu-id="ac26e-113">[jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/) are installed, and Bower is supported.</span></span>

<span data-ttu-id="ac26e-114">Les packages côté client sont répertoriés dans le fichier *bower.json*.</span><span class="sxs-lookup"><span data-stu-id="ac26e-114">Client-side packages are listed in the *bower.json* file.</span></span> <span data-ttu-id="ac26e-115">Les modèles de projet ASP.NET Core configurent *bower.json* avec jQuery, la validation jQuery et Bootstrap.</span><span class="sxs-lookup"><span data-stu-id="ac26e-115">The ASP.NET Core project templates configures *bower.json* with jQuery, jQuery validation, and Bootstrap.</span></span>

<span data-ttu-id="ac26e-116">Dans ce didacticiel, nous allons ajouter la prise en charge de [Font Awesome](http://fontawesome.io).</span><span class="sxs-lookup"><span data-stu-id="ac26e-116">In this tutorial, we'll add support for [Font Awesome](http://fontawesome.io).</span></span> <span data-ttu-id="ac26e-117">Les packages Bower peuvent être installés avec l'interface graphique **Gérer les packages Bower** ou manuellement dans le fichier *bower.json*.</span><span class="sxs-lookup"><span data-stu-id="ac26e-117">Bower packages can be installed with the **Manage Bower Packages** UI or manually in the *bower.json* file.</span></span>

### <a name="installation-via-manage-bower-packages-ui"></a><span data-ttu-id="ac26e-118">Installation via gérer les Packages Bower l’interface utilisateur</span><span class="sxs-lookup"><span data-stu-id="ac26e-118">Installation via Manage Bower Packages UI</span></span>

* <span data-ttu-id="ac26e-119">Créez une application web ASP.NET Core avec le modèle **Application web ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="ac26e-119">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="ac26e-120">Sélectionnez **Application Web** et **Aucune authentification**.</span><span class="sxs-lookup"><span data-stu-id="ac26e-120">Select **Web Application** and **No Authentication**.</span></span>

* <span data-ttu-id="ac26e-121">Cliquez avec le bouton droit sur le projet dans l’Explorateur de solutions et sélectionnez **Gérer les packages Bower** (ou dans le menu principal, **Projet** > **Gérer les packages Bower**).</span><span class="sxs-lookup"><span data-stu-id="ac26e-121">Right-click the project in Solution Explorer and select **Manage Bower Packages** (alternatively from the main menu, **Project** > **Manage Bower Packages**).</span></span>

* <span data-ttu-id="ac26e-122">Dans la fenêtre **Bower : \<nom du projet\>** cliquez sur l’onglet "Parcourir", puis filtrer la liste des packages en entrant `font-awesome` dans la zone de recherche :</span><span class="sxs-lookup"><span data-stu-id="ac26e-122">In the **Bower: \<project name\>** window, click the "Browse" tab, and then filter the packages list by entering `font-awesome` in the search box:</span></span>

  ![gérer les packages bower](bower/_static/manage-bower-packages.png)

* <span data-ttu-id="ac26e-124">Vérifiez que la case à cocher "Enregistrer les modifications dans *bower.json* est activée.</span><span class="sxs-lookup"><span data-stu-id="ac26e-124">Confirm that the "Save changes to *bower.json*" checkbox is checked.</span></span> <span data-ttu-id="ac26e-125">Sélectionnez une version dans la liste déroulante et cliquez sur le bouton **Installer**.</span><span class="sxs-lookup"><span data-stu-id="ac26e-125">Select a version from the drop-down list and click the **Install** button.</span></span> <span data-ttu-id="ac26e-126">Le fenêtre de **sortie** affiche les détails d’installation.</span><span class="sxs-lookup"><span data-stu-id="ac26e-126">The **Output** window shows the installation details.</span></span>

### <a name="manual-installation-in-bowerjson"></a><span data-ttu-id="ac26e-127">Installation manuelle de bower.json</span><span class="sxs-lookup"><span data-stu-id="ac26e-127">Manual installation in bower.json</span></span>

<span data-ttu-id="ac26e-128">Ouvrez le fichier *bower.json* et ajoutez "Font Awesome" aux dépendances.</span><span class="sxs-lookup"><span data-stu-id="ac26e-128">Open the *bower.json* file and add "font-awesome" to the dependencies.</span></span> <span data-ttu-id="ac26e-129">IntelliSense affiche les packages disponibles. </span><span class="sxs-lookup"><span data-stu-id="ac26e-129">IntelliSense shows the available packages.</span></span> <span data-ttu-id="ac26e-130">Lorsqu’un package est sélectionné, les versions disponibles sont affichées. </span><span class="sxs-lookup"><span data-stu-id="ac26e-130">When a package is selected, the available versions are displayed.</span></span> <span data-ttu-id="ac26e-131">Les images ci-dessous sont plus anciennes et ne correspondent pas à ce que vous voyez.</span><span class="sxs-lookup"><span data-stu-id="ac26e-131">The images below are older and won't match what you see.</span></span>

![IntelliSense de l’Explorateur de package bower](bower/_static/add-package.png)

![version de bower IntelliSense](bower/_static/version-intelliSense.png)

<span data-ttu-id="ac26e-134">Bower utilise le [contrôle de version sémantique](http://semver.org/) pour organiser les dépendances.  les dépendances.</span><span class="sxs-lookup"><span data-stu-id="ac26e-134">Bower uses [semantic versioning](http://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="ac26e-135">Le contrôle de version sémantique, également appelé SemVer, identifie les packages avec le modèle de numérotation \<majeure >.\< mineure >. \<correctif >.</span><span class="sxs-lookup"><span data-stu-id="ac26e-135">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme \<major>.\<minor>.\<patch>.</span></span> <span data-ttu-id="ac26e-136">IntelliSense simplifie le contrôle de version sémantique en affichant uniquement quelques choix courants.</span><span class="sxs-lookup"><span data-stu-id="ac26e-136">IntelliSense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="ac26e-137">Le premier élément dans la liste IntelliSense (4.6.3 dans l’exemple ci-dessus) est considéré comme la dernière version stable du package.</span><span class="sxs-lookup"><span data-stu-id="ac26e-137">The top item in the IntelliSense list (4.6.3 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="ac26e-138">Le symbole de signe insertion (^) correspond à la version majeure la plus récente et le tilde (~) correspond à la version mineure la plus récente.</span><span class="sxs-lookup"><span data-stu-id="ac26e-138">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span>

<span data-ttu-id="ac26e-139">Enregistrez le fichier *bower.json*.</span><span class="sxs-lookup"><span data-stu-id="ac26e-139">Save the *bower.json* file.</span></span> <span data-ttu-id="ac26e-140">Visual Studio surveille les modifications du fichier *bower.json*.</span><span class="sxs-lookup"><span data-stu-id="ac26e-140">Visual Studio watches the *bower.json* file for changes.</span></span> <span data-ttu-id="ac26e-141">Après l’enregistrement, la commande *bower install* est exécutée.</span><span class="sxs-lookup"><span data-stu-id="ac26e-141">Upon saving, the *bower install* command is executed.</span></span> <span data-ttu-id="ac26e-142">Consultez la fenêtre de sortie et la vue**Bower/npm** pour la commande exacte exécutée.</span><span class="sxs-lookup"><span data-stu-id="ac26e-142">See the Output window's **Bower/npm** view for the exact command executed.</span></span>

<span data-ttu-id="ac26e-143">Ouvrez le fichier *.bowerrc*sous*bower.json*.</span><span class="sxs-lookup"><span data-stu-id="ac26e-143">Open the *.bowerrc* file under *bower.json*.</span></span> <span data-ttu-id="ac26e-144">La propriété `directory` est configurée sur *wwwroot/lib*  qui indique l’emplacement où Bower installera les composants de package.</span><span class="sxs-lookup"><span data-stu-id="ac26e-144">The `directory` property is set to *wwwroot/lib* which indicates the location Bower will install the package assets.</span></span>

```json
{
 "directory": "wwwroot/lib"
}
```

<span data-ttu-id="ac26e-145">Vous pouvez utiliser la zone de recherche dans l’Explorateur de solutions pour rechercher et afficher le package font-awesome.</span><span class="sxs-lookup"><span data-stu-id="ac26e-145">You can use the search box in Solution Explorer to find and display the font-awesome package.</span></span>

<span data-ttu-id="ac26e-146">Ouvrez le fichier *Views\Shared\_Layout.cshtml* et ajoutez le fichier CSS font-awesome au[TagHelper](xref:mvc/views/tag-helpers/intro) de l’environnement pour `Development`.</span><span class="sxs-lookup"><span data-stu-id="ac26e-146">Open the *Views\Shared\_Layout.cshtml* file and add the font-awesome CSS file to the environment [Tag Helper](xref:mvc/views/tag-helpers/intro) for `Development`.</span></span> <span data-ttu-id="ac26e-147">À partir de l’Explorateur de solutions, faites glisser et déposez*font-awesome.css* à l’intérieur de l’élément `<environment names="Development">`.</span><span class="sxs-lookup"><span data-stu-id="ac26e-147">From Solution Explorer, drag and drop *font-awesome.css* inside the `<environment names="Development">` element.</span></span>

[!code-html[](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]

<span data-ttu-id="ac26e-148">Dans une application de production, vous ajouteriez *font-awesome.min.css* au tag helper de l'environnement pour `Staging,Production`.</span><span class="sxs-lookup"><span data-stu-id="ac26e-148">In a production app you would add *font-awesome.min.css* to the environment tag helper for `Staging,Production`.</span></span>

<span data-ttu-id="ac26e-149">Remplacez le contenu du fichier Razor *Views\Home\About.cshtml* par le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="ac26e-149">Replace the contents of the *Views\Home\About.cshtml* Razor file with the following markup:</span></span>

[!code-html[](bower/sample/About.cshtml)]

<span data-ttu-id="ac26e-150">Exécutez l’application et accédez à la vue About pour vérifier le fonctionnement du package font-awesome.</span><span class="sxs-lookup"><span data-stu-id="ac26e-150">Run the app and navigate to the About view to verify the font-awesome package works.</span></span>

## <a name="exploring-the-client-side-build-process"></a><span data-ttu-id="ac26e-151">Explorer le processus de génération du côté client</span><span class="sxs-lookup"><span data-stu-id="ac26e-151">Exploring the client-side build process</span></span>

<span data-ttu-id="ac26e-152">La plupart des modèles de projet ASP.NET Core sont déjà configurés pour utiliser Bower.</span><span class="sxs-lookup"><span data-stu-id="ac26e-152">Most ASP.NET Core project templates are already configured to use Bower.</span></span> <span data-ttu-id="ac26e-153">Cette procédure pas à pas commence par un projet ASP.NET Core vide et ajoute chaque élément manuellement pour vous donner une idée de l’utilisation de Bower dans un projet.</span><span class="sxs-lookup"><span data-stu-id="ac26e-153">This next walkthrough starts with an empty ASP.NET Core project and adds each piece manually, so you can get a feel for how Bower is used in a project.</span></span> <span data-ttu-id="ac26e-154">Vous pouvez voir ainsi ce qui arrive à la structure du projet et le résultat de l’exécution chaque fois que la configuration est modifiée.</span><span class="sxs-lookup"><span data-stu-id="ac26e-154">You can see what happens to the project structure and the runtime output as each configuration change is made.</span></span>

<span data-ttu-id="ac26e-155">Les étapes globales à utiliser pour le processus de génération côté client avec Bower sont :</span><span class="sxs-lookup"><span data-stu-id="ac26e-155">The general steps to use the client-side build process with Bower are:</span></span>

* <span data-ttu-id="ac26e-156">Définir les paquets utilisés dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="ac26e-156">Define packages used in your project.</span></span> <!-- once defined, you don't need to download them, VS does -->
* <span data-ttu-id="ac26e-157">Référencer des paquets à partir de vos pages web.</span><span class="sxs-lookup"><span data-stu-id="ac26e-157">Reference packages from your web pages.</span></span>

### <a name="define-packages"></a><span data-ttu-id="ac26e-158">Définir des packages</span><span class="sxs-lookup"><span data-stu-id="ac26e-158">Define packages</span></span>

<span data-ttu-id="ac26e-159">Une fois la liste de paquets définie dans le fichier *bower.json*, Visual Studio télécharge les paquets.</span><span class="sxs-lookup"><span data-stu-id="ac26e-159">Once you list packages in the *bower.json* file, Visual Studio will download them.</span></span> <span data-ttu-id="ac26e-160">L’exemple suivant utilise Bower pour charger jQuery et Bootstrap dans le dossier *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="ac26e-160">The following example uses Bower to load jQuery and Bootstrap to the *wwwroot* folder.</span></span>

* <span data-ttu-id="ac26e-161">Créez une application web ASP.NET Core avec le modèle **Application web ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="ac26e-161">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="ac26e-162">Sélectionnez le modèle de projet **vide** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="ac26e-162">Select the **Empty** project template and click **OK**.</span></span>

* <span data-ttu-id="ac26e-163">Dans l’Explorateur de solutions, cliquez sur le projet > **Ajouter un nouvel élément** et sélectionnez **Fichier de configuration Bower**.</span><span class="sxs-lookup"><span data-stu-id="ac26e-163">In Solution Explorer, right-click the project > **Add New Item** and select **Bower Configuration File**.</span></span> <span data-ttu-id="ac26e-164">Remarque : un fichier *.bowerrc* est également ajouté.</span><span class="sxs-lookup"><span data-stu-id="ac26e-164">Note: A *.bowerrc* file is also added.</span></span>

* <span data-ttu-id="ac26e-165">Ouvrez le fichier *bower.json*et ajoutez jquery et bootstrap dans la section `dependencies`.</span><span class="sxs-lookup"><span data-stu-id="ac26e-165">Open *bower.json*, and add jquery and bootstrap to the `dependencies` section.</span></span> <span data-ttu-id="ac26e-166">Le fichier *bower.json* résultant ressemblera à l’exemple suivant.</span><span class="sxs-lookup"><span data-stu-id="ac26e-166">The resulting *bower.json* file will look like the following example.</span></span> <span data-ttu-id="ac26e-167">Les versions changent au fil du temps et peuvent ne pas correspondre à l’image ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="ac26e-167">The versions will change over time and may not match the image below.</span></span>

[!code-json[](bower/sample/bower.json?highlight=5,6)]

* <span data-ttu-id="ac26e-168">Enregistrez le fichier *bower.json*.</span><span class="sxs-lookup"><span data-stu-id="ac26e-168">Save the *bower.json* file.</span></span>

  <span data-ttu-id="ac26e-169">Vérifiez que le projet inclut les répertoires *Bootstrap* et *jQuery* dans *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="ac26e-169">Verify the project includes the *bootstrap* and *jQuery* directories in *wwwroot/lib*.</span></span> <span data-ttu-id="ac26e-170">Bower utilise le fichier *.bowerrc* pour installer les composants dans *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="ac26e-170">Bower uses the *.bowerrc* file to install the assets in *wwwroot/lib*.</span></span>

  <span data-ttu-id="ac26e-171">Remarque : L’interface utilisateur "Gérer les paquets Bower" offre une alternative à la modification manuelle du fichier.</span><span class="sxs-lookup"><span data-stu-id="ac26e-171">Note: The "Manage Bower Packages" UI provides an alternative to manual file editing.</span></span>

### <a name="enable-static-files"></a><span data-ttu-id="ac26e-172">Activer les fichiers statiques</span><span class="sxs-lookup"><span data-stu-id="ac26e-172">Enable static files</span></span>

* <span data-ttu-id="ac26e-173">Ajoutez le paquet NuGet `Microsoft.AspNetCore.StaticFiles` au projet.</span><span class="sxs-lookup"><span data-stu-id="ac26e-173">Add the `Microsoft.AspNetCore.StaticFiles` NuGet package to the project.</span></span>
* <span data-ttu-id="ac26e-174">Activez le support des fichiers statiques grâce à [l’intergiciel (middleware) de service des fichiers statiques](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions).</span><span class="sxs-lookup"><span data-stu-id="ac26e-174">Enable static files to be served with the [Static file middleware](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions).</span></span> <span data-ttu-id="ac26e-175">Ajoutez un appel à [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions) à la méthode `Configure` de la classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="ac26e-175">Add a call to [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions) to the `Configure` method of `Startup`.</span></span>

[!code-csharp[](bower/sample/Startup.cs?highlight=9)]

### <a name="reference-packages"></a><span data-ttu-id="ac26e-176">Packages de référence</span><span class="sxs-lookup"><span data-stu-id="ac26e-176">Reference packages</span></span>

<span data-ttu-id="ac26e-177">Dans cette section, vous allez créer une page HTML pour vérifier qu’il peut accéder aux packages déployés.</span><span class="sxs-lookup"><span data-stu-id="ac26e-177">In this section, you will create an HTML page to verify it can access the deployed packages.</span></span>

* <span data-ttu-id="ac26e-178">Ajoutez une nouvelle page HTML nommée *Index.html* dans le dossier *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="ac26e-178">Add a new HTML page named *Index.html* to the *wwwroot* folder.</span></span> <span data-ttu-id="ac26e-179">Remarque : Vous devez ajouter le fichier HTML dans le dossier *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="ac26e-179">Note: You must add the HTML file to the *wwwroot* folder.</span></span> <span data-ttu-id="ac26e-180">Remarque : Vous devez ajouter le fichier HTML dans le dossier *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="ac26e-180">By default, static content cannot be served outside *wwwroot*.</span></span> <span data-ttu-id="ac26e-181">Consultez [travailler avec des fichiers statiques](xref:fundamentals/static-files) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="ac26e-181">See [Work with static files](xref:fundamentals/static-files) for more information.</span></span>

  <span data-ttu-id="ac26e-182">Remplacez le contenu du fichier *Index.html* par le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="ac26e-182">Replace the contents of *Index.html* with the following markup:</span></span>

[!code-html[](bower/sample/Index.html)]

* <span data-ttu-id="ac26e-183">Exécutez l’application et accédez à `http://localhost:<port>/Index.html`.</span><span class="sxs-lookup"><span data-stu-id="ac26e-183">Run the app and navigate to `http://localhost:<port>/Index.html`.</span></span> <span data-ttu-id="ac26e-184">Vous pouvez également, à partir du fichier *Index.html* ouvert, appuyer sur `Ctrl+Shift+W`.</span><span class="sxs-lookup"><span data-stu-id="ac26e-184">Alternatively, with *Index.html* opened, press `Ctrl+Shift+W`.</span></span> <span data-ttu-id="ac26e-185">Vérifiez que le style jumbotron est appliqué, que le code jQuery répond quand le bouton fait l'objet d'un clic et que le bouton Bootstrap change d’état.</span><span class="sxs-lookup"><span data-stu-id="ac26e-185">Verify that the jumbotron styling is applied, the jQuery code responds when the button is clicked, and that the Bootstrap button changes state.</span></span>

  ![style de JumboTron appliqué](bower/_static/jumbotron.png)
