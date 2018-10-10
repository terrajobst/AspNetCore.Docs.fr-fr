---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: Création de Pages d’aide pour l’API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 04/01/2013
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: c081064a32151a71fc4f3ea407e0c48a1539432a
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48913123"
---
<a name="creating-help-pages-for-aspnet-web-api"></a><span data-ttu-id="dc78e-102">Création de Pages d’aide pour l’API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="dc78e-102">Creating Help Pages for ASP.NET Web API</span></span>
====================
<span data-ttu-id="dc78e-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="dc78e-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="dc78e-104">Lorsque vous créez une API web, il est souvent utile de créer une page d’aide, afin que les autres développeurs sache comment appeler votre API.</span><span class="sxs-lookup"><span data-stu-id="dc78e-104">When you create a web API, it is often useful to create a help page, so that other developers will know how to call your API.</span></span> <span data-ttu-id="dc78e-105">Vous pouvez créer manuellement toute la documentation, mais il est préférable de créer automatiquement autant que possible.</span><span class="sxs-lookup"><span data-stu-id="dc78e-105">You could create all of the documentation manually, but it is better to autogenerate as much as possible.</span></span>

<span data-ttu-id="dc78e-106">Pour faciliter cette tâche, l’API Web ASP.NET fournit une bibliothèque pour générer automatiquement des pages d’aide en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="dc78e-106">To make this task easier, ASP.NET Web API provides a library for auto-generating help pages at run time.</span></span>

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a><span data-ttu-id="dc78e-107">Création de Pages d’aide de l’API</span><span class="sxs-lookup"><span data-stu-id="dc78e-107">Creating API Help Pages</span></span>

<span data-ttu-id="dc78e-108">Installer [ASP.NET et Web Tools 2012.2 mise à jour](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="dc78e-108">Install [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="dc78e-109">Cette mise à jour intègre des pages d’aide dans le modèle de projet API Web.</span><span class="sxs-lookup"><span data-stu-id="dc78e-109">This update integrates help pages into the Web API project template.</span></span>

<span data-ttu-id="dc78e-110">Ensuite, créez un projet ASP.NET MVC 4 et sélectionnez le modèle de projet API Web.</span><span class="sxs-lookup"><span data-stu-id="dc78e-110">Next, create a new ASP.NET MVC 4 project and select the Web API project template.</span></span> <span data-ttu-id="dc78e-111">Le modèle de projet crée un contrôleur de l’exemple d’API nommé `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="dc78e-111">The project template creates an example API controller named `ValuesController`.</span></span> <span data-ttu-id="dc78e-112">Le modèle crée également les pages d’aide API.</span><span class="sxs-lookup"><span data-stu-id="dc78e-112">The template also creates the API help pages.</span></span> <span data-ttu-id="dc78e-113">Tous les fichiers de code pour la page d’aide sont placés dans le dossier des zones du projet.</span><span class="sxs-lookup"><span data-stu-id="dc78e-113">All of the code files for the help page are placed in the Areas folder of the project.</span></span>

![](creating-api-help-pages/_static/image2.png)

<span data-ttu-id="dc78e-114">Lorsque vous exécutez l’application, la page d’accueil contient un lien vers la page d’aide API.</span><span class="sxs-lookup"><span data-stu-id="dc78e-114">When you run the application, the home page contains a link to the API help page.</span></span> <span data-ttu-id="dc78e-115">À partir de la page d’accueil, le chemin d’accès relatif est /Help.</span><span class="sxs-lookup"><span data-stu-id="dc78e-115">From the home page, the relative path is /Help.</span></span>

![](creating-api-help-pages/_static/image3.png)

<span data-ttu-id="dc78e-116">Ce lien vous dirige vers une page de résumé des API.</span><span class="sxs-lookup"><span data-stu-id="dc78e-116">This link brings you to an API summary page.</span></span>

![](creating-api-help-pages/_static/image4.png)

<span data-ttu-id="dc78e-117">La vue MVC pour cette page est définie dans Areas/HelpPage/Views/Help/Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="dc78e-117">The MVC view for this page is defined in Areas/HelpPage/Views/Help/Index.cshtml.</span></span> <span data-ttu-id="dc78e-118">Vous pouvez modifier cette page pour modifier la mise en page introduction, titre, styles et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="dc78e-118">You can edit this page to modify the layout, introduction, title, styles, and so forth.</span></span>

<span data-ttu-id="dc78e-119">La partie principale de la page est une table d’API, regroupées par contrôleur.</span><span class="sxs-lookup"><span data-stu-id="dc78e-119">The main part of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="dc78e-120">Les entrées de table sont générées dynamiquement, à l’aide de la **IApiExplorer** interface.</span><span class="sxs-lookup"><span data-stu-id="dc78e-120">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="dc78e-121">(Je reviendrai sur cette interface ultérieurement.) Si vous ajoutez un nouveau contrôleur d’API, la table est automatiquement mis à jour en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="dc78e-121">(I'll talk more about this interface later.) If you add a new API controller, the table is automatically updated at run time.</span></span>

<span data-ttu-id="dc78e-122">La colonne « API » répertorie la méthode HTTP et un URI relatif.</span><span class="sxs-lookup"><span data-stu-id="dc78e-122">The "API" column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="dc78e-123">La colonne « Description » contient la documentation pour chaque API.</span><span class="sxs-lookup"><span data-stu-id="dc78e-123">The "Description" column contains documentation for each API.</span></span> <span data-ttu-id="dc78e-124">Initialement, la documentation est simplement texte d’espace réservé.</span><span class="sxs-lookup"><span data-stu-id="dc78e-124">Initially, the documentation is just placeholder text.</span></span> <span data-ttu-id="dc78e-125">Dans la section suivante, je vous montrerai comment ajouter la documentation à partir de commentaires XML.</span><span class="sxs-lookup"><span data-stu-id="dc78e-125">In the next section, I'll show you how to add documentation from XML comments.</span></span>

<span data-ttu-id="dc78e-126">Chaque API comporte un lien vers une page avec des informations plus détaillées, y compris le corps de demande et de réponse d’exemple.</span><span class="sxs-lookup"><span data-stu-id="dc78e-126">Each API has a link to a page with more detailed information, including example request and response bodies.</span></span>

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a><span data-ttu-id="dc78e-127">Ajout de Pages d’aide à un projet existant</span><span class="sxs-lookup"><span data-stu-id="dc78e-127">Adding Help Pages to an Existing Project</span></span>

<span data-ttu-id="dc78e-128">Vous pouvez ajouter des pages d’aide à un projet d’API Web existant à l’aide du Gestionnaire de Package NuGet.</span><span class="sxs-lookup"><span data-stu-id="dc78e-128">You can add help pages to an existing Web API project by using NuGet Package Manager.</span></span> <span data-ttu-id="dc78e-129">Cette option est utile de que commencer à partir d’un autre modèle de projet que le modèle « API Web ».</span><span class="sxs-lookup"><span data-stu-id="dc78e-129">This option is useful you start from a different project template than the "Web API" template.</span></span>

<span data-ttu-id="dc78e-130">À partir de la **outils** menu, sélectionnez **Gestionnaire de Package NuGet**, puis sélectionnez **Console du Gestionnaire de Package**.</span><span class="sxs-lookup"><span data-stu-id="dc78e-130">From the **Tools** menu, select **NuGet Package Manager**, and then select **Package Manager Console**.</span></span> <span data-ttu-id="dc78e-131">Dans le [Console du Gestionnaire de Package](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) fenêtre, tapez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="dc78e-131">In the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) window, type one of the following commands:</span></span>

<span data-ttu-id="dc78e-132">Pour un **c#** application : `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span><span class="sxs-lookup"><span data-stu-id="dc78e-132">For a **C#** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span></span>

<span data-ttu-id="dc78e-133">Pour un **Visual Basic** application : `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span><span class="sxs-lookup"><span data-stu-id="dc78e-133">For a **Visual Basic** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span></span>

<span data-ttu-id="dc78e-134">Il existe deux packages, pour c# et l’autre pour Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="dc78e-134">There are two packages, one for C# and one for Visual Basic.</span></span> <span data-ttu-id="dc78e-135">Veillez à utiliser celui qui correspond à votre projet.</span><span class="sxs-lookup"><span data-stu-id="dc78e-135">Make sure to use the one that matches your project.</span></span>

<span data-ttu-id="dc78e-136">Cette commande installe les assemblys nécessaires et ajoute les vues MVC pour les pages d’aide (situés dans le dossier zones/HelpPage).</span><span class="sxs-lookup"><span data-stu-id="dc78e-136">This command installs the necessary assemblies and adds the MVC views for the help pages (located in the Areas/HelpPage folder).</span></span> <span data-ttu-id="dc78e-137">Vous devez manuellement ajouter un lien vers la page d’aide.</span><span class="sxs-lookup"><span data-stu-id="dc78e-137">You'll need to manually add a link to the Help page.</span></span> <span data-ttu-id="dc78e-138">L’URI est /Help.</span><span class="sxs-lookup"><span data-stu-id="dc78e-138">The URI is /Help.</span></span> <span data-ttu-id="dc78e-139">Pour créer un lien dans une vue razor, ajoutez ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="dc78e-139">To create a link in a razor view, add the following:</span></span>

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

<span data-ttu-id="dc78e-140">En outre, assurez-vous qu’inscrire les zones.</span><span class="sxs-lookup"><span data-stu-id="dc78e-140">Also, make sure to register areas.</span></span> <span data-ttu-id="dc78e-141">Dans le fichier Global.asax, ajoutez le code suivant à la **Application\_Démarrer** méthode, s’il y ne figure pas déjà :</span><span class="sxs-lookup"><span data-stu-id="dc78e-141">In the Global.asax file, add the following code to the **Application\_Start** method, if it is not there already:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a><span data-ttu-id="dc78e-142">Ajout de Documentation de l’API</span><span class="sxs-lookup"><span data-stu-id="dc78e-142">Adding API Documentation</span></span>

<span data-ttu-id="dc78e-143">Par défaut, l’aide de pages contiennent des chaînes d’espace réservé pour la documentation.</span><span class="sxs-lookup"><span data-stu-id="dc78e-143">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="dc78e-144">Vous pouvez utiliser [commentaires de documentation XML](https://msdn.microsoft.com/library/b2s063f7.aspx) pour créer la documentation.</span><span class="sxs-lookup"><span data-stu-id="dc78e-144">You can use [XML documentation comments](https://msdn.microsoft.com/library/b2s063f7.aspx) to create the documentation.</span></span> <span data-ttu-id="dc78e-145">Pour activer cette fonctionnalité, ouvrez le fichier de zones/HelpPage/application\_Start/HelpPageConfig.cs et supprimez les commentaires de la ligne suivante :</span><span class="sxs-lookup"><span data-stu-id="dc78e-145">To enable this feature, open the file Areas/HelpPage/App\_Start/HelpPageConfig.cs and uncomment the following line:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

<span data-ttu-id="dc78e-146">Activez maintenant la documentation XML.</span><span class="sxs-lookup"><span data-stu-id="dc78e-146">Now enable XML documentation.</span></span> <span data-ttu-id="dc78e-147">Dans l’Explorateur de solutions, cliquez sur le projet et sélectionnez **propriétés**.</span><span class="sxs-lookup"><span data-stu-id="dc78e-147">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="dc78e-148">Sélectionnez le **Build** page.</span><span class="sxs-lookup"><span data-stu-id="dc78e-148">Select the **Build** page.</span></span>

![](creating-api-help-pages/_static/image6.png)

<span data-ttu-id="dc78e-149">Sous **sortie**, vérifiez **fichier de documentation XML**.</span><span class="sxs-lookup"><span data-stu-id="dc78e-149">Under **Output**, check **XML documentation file**.</span></span> <span data-ttu-id="dc78e-150">Dans la zone d’édition, tapez « application\_Data/XmlDocument.xml ».</span><span class="sxs-lookup"><span data-stu-id="dc78e-150">In the edit box, type "App\_Data/XmlDocument.xml".</span></span>

![](creating-api-help-pages/_static/image7.png)

<span data-ttu-id="dc78e-151">Ensuite, ouvrez le code pour le `ValuesController` contrôleur d’API, qui est défini dans /Controllers/ValuesControler.cs.</span><span class="sxs-lookup"><span data-stu-id="dc78e-151">Next, open the code for the `ValuesController` API controller, which is defined in /Controllers/ValuesControler.cs.</span></span> <span data-ttu-id="dc78e-152">Ajouter des commentaires de documentation pour les méthodes de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="dc78e-152">Add some documentation comments to the controller methods.</span></span> <span data-ttu-id="dc78e-153">Exemple :</span><span class="sxs-lookup"><span data-stu-id="dc78e-153">For example:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="dc78e-154">Conseil : Si vous positionnez le signe insertion sur la ligne au-dessus de la méthode et que vous tapez les trois barres obliques, Visual Studio insère automatiquement les éléments XML.</span><span class="sxs-lookup"><span data-stu-id="dc78e-154">Tip: If you position the caret on the line above the method and type three forward slashes, Visual Studio automatically inserts the XML elements.</span></span> <span data-ttu-id="dc78e-155">Vous pouvez ensuite renseigner les champs vides.</span><span class="sxs-lookup"><span data-stu-id="dc78e-155">Then you can fill in the blanks.</span></span>


<span data-ttu-id="dc78e-156">Maintenant générer et exécuter à nouveau l’application et naviguer vers les pages d’aide.</span><span class="sxs-lookup"><span data-stu-id="dc78e-156">Now build and run the application again, and navigate to the help pages.</span></span> <span data-ttu-id="dc78e-157">Les chaînes de documentation doivent apparaître dans la table API.</span><span class="sxs-lookup"><span data-stu-id="dc78e-157">The documentation strings should appear in the API table.</span></span>

![](creating-api-help-pages/_static/image8.png)

<span data-ttu-id="dc78e-158">La page d’aide lit les chaînes à partir du fichier XML en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="dc78e-158">The help page reads the strings from the XML file at run time.</span></span> <span data-ttu-id="dc78e-159">(Lorsque vous déployez l’application, veillez à déployer le fichier XML.)</span><span class="sxs-lookup"><span data-stu-id="dc78e-159">(When you deploy the application, make sure to deploy the XML file.)</span></span>

## <a name="under-the-hood"></a><span data-ttu-id="dc78e-160">Sous le capot</span><span class="sxs-lookup"><span data-stu-id="dc78e-160">Under the Hood</span></span>

<span data-ttu-id="dc78e-161">Les pages d’aide sont appuient sur le **ApiExplorer** (classe), qui fait partie de l’infrastructure API Web.</span><span class="sxs-lookup"><span data-stu-id="dc78e-161">The help pages are built on top of the **ApiExplorer** class, which is part of the Web API framework.</span></span> <span data-ttu-id="dc78e-162">Le **ApiExplorer** classe fournit la matière première pour la création d’une page d’aide.</span><span class="sxs-lookup"><span data-stu-id="dc78e-162">The **ApiExplorer** class provides the raw material for creating a help page.</span></span> <span data-ttu-id="dc78e-163">Pour chaque API, **ApiExplorer** contient un **ApiDescription** qui décrit l’API.</span><span class="sxs-lookup"><span data-stu-id="dc78e-163">For each API, **ApiExplorer** contains an **ApiDescription** that describes the API.</span></span> <span data-ttu-id="dc78e-164">Pour cela, une « API » est définie comme la combinaison de méthodes HTTP et l’URI relatif.</span><span class="sxs-lookup"><span data-stu-id="dc78e-164">For this purpose, an "API" is defined as the combination of HTTP method and relative URI.</span></span> <span data-ttu-id="dc78e-165">Par exemple, Voici certaines API distinctes :</span><span class="sxs-lookup"><span data-stu-id="dc78e-165">For example, here are some distinct APIs:</span></span>

- <span data-ttu-id="dc78e-166">OBTENIR /api/Products</span><span class="sxs-lookup"><span data-stu-id="dc78e-166">GET /api/Products</span></span>
- <span data-ttu-id="dc78e-167">OBTENIR/API/produits / {id}</span><span class="sxs-lookup"><span data-stu-id="dc78e-167">GET /api/Products/{id}</span></span>
- <span data-ttu-id="dc78e-168">VALIDER les produits/api /</span><span class="sxs-lookup"><span data-stu-id="dc78e-168">POST /api/Products</span></span>

<span data-ttu-id="dc78e-169">Si une action de contrôleur prend en charge plusieurs méthodes HTTP, le **ApiExplorer** traite chaque méthode comme une API distincte.</span><span class="sxs-lookup"><span data-stu-id="dc78e-169">If a controller action supports multiple HTTP methods, the **ApiExplorer** treats each method as a distinct API.</span></span>

<span data-ttu-id="dc78e-170">Pour masquer une API à partir de la **ApiExplorer**, ajouter le **ApiExplorerSettings** à l’action et un ensemble d’attributs *IgnoreApi* sur true.</span><span class="sxs-lookup"><span data-stu-id="dc78e-170">To hide an API from the **ApiExplorer**, add the **ApiExplorerSettings** attribute to the action and set *IgnoreApi* to true.</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

<span data-ttu-id="dc78e-171">Vous pouvez également ajouter cet attribut au contrôleur, pour exclure l’ensemble du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="dc78e-171">You can also add this attribute to the controller, to exclude the entire controller.</span></span>

<span data-ttu-id="dc78e-172">La classe ApiExplorer Obtient les chaînes de documentation à partir de la **IDocumentationProvider** interface.</span><span class="sxs-lookup"><span data-stu-id="dc78e-172">The ApiExplorer class gets documentation strings from the **IDocumentationProvider** interface.</span></span> <span data-ttu-id="dc78e-173">Comme vous l’avez vu précédemment, la bibliothèque de Pages d’aide fournit un **IDocumentationProvider** qui obtient la documentation à partir de chaînes de documentation XML.</span><span class="sxs-lookup"><span data-stu-id="dc78e-173">As you saw earlier, the Help Pages library provides an **IDocumentationProvider** that gets documentation from XML documentation strings.</span></span> <span data-ttu-id="dc78e-174">Le code se trouve dans /Areas/HelpPage/XmlDocumentationProvider.cs.</span><span class="sxs-lookup"><span data-stu-id="dc78e-174">The code is located in /Areas/HelpPage/XmlDocumentationProvider.cs.</span></span> <span data-ttu-id="dc78e-175">Vous pouvez obtenir documentation à partir d’une autre source en écrivant votre propre **IDocumentationProvider**.</span><span class="sxs-lookup"><span data-stu-id="dc78e-175">You can get documentation from another source by writing your own **IDocumentationProvider**.</span></span> <span data-ttu-id="dc78e-176">Pour rattacher, appelez le **SetDocumentationProvider** méthode d’extension, définie dans **HelpPageConfigurationExtensions**</span><span class="sxs-lookup"><span data-stu-id="dc78e-176">To wire it up, call the **SetDocumentationProvider** extension method, defined in **HelpPageConfigurationExtensions**</span></span>

<span data-ttu-id="dc78e-177">**ApiExplorer** appelle automatiquement la **IDocumentationProvider** interface permettant d’obtenir des chaînes de documentation pour chaque API.</span><span class="sxs-lookup"><span data-stu-id="dc78e-177">**ApiExplorer** automatically calls into the **IDocumentationProvider** interface to get documentation strings for each API.</span></span> <span data-ttu-id="dc78e-178">Elle les stocke dans le **Documentation** propriété de la **ApiDescription** et **ApiParameterDescription** objets.</span><span class="sxs-lookup"><span data-stu-id="dc78e-178">It stores them in the **Documentation** property of the **ApiDescription** and **ApiParameterDescription** objects.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dc78e-179">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="dc78e-179">Next Steps</span></span>

<span data-ttu-id="dc78e-180">Vous n’êtes pas limité aux pages d’aide indiqués ici.</span><span class="sxs-lookup"><span data-stu-id="dc78e-180">You aren't limited to the help pages shown here.</span></span> <span data-ttu-id="dc78e-181">En fait, **ApiExplorer** n’est pas limité à la création de pages d’aide.</span><span class="sxs-lookup"><span data-stu-id="dc78e-181">In fact, **ApiExplorer** is not limited to creating help pages.</span></span> <span data-ttu-id="dc78e-182">Yao Huang Lin a écrit qu'un excellent blog publie pour susciter votre réflexion prêts à l’emploi :</span><span class="sxs-lookup"><span data-stu-id="dc78e-182">Yao Huang Lin has written some great blog posts to get you thinking out of the box:</span></span>

- [<span data-ttu-id="dc78e-183">Ajout d’un Client de Test simple à la Page d’aide ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="dc78e-183">Adding a simple Test Client to ASP.NET Web API Help Page</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [<span data-ttu-id="dc78e-184">Faire les API vous aider à la Page Web ASP.NET à fonctionner sur les services auto-hébergés</span><span class="sxs-lookup"><span data-stu-id="dc78e-184">Making ASP.NET Web API Help Page work on self-hosted services</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [<span data-ttu-id="dc78e-185">Génération au moment du design de help page (ou client) pour l’API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="dc78e-185">Design-time generation of help page (or client) for ASP.NET Web API</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [<span data-ttu-id="dc78e-186">Personnalisations de Page d’aide avancées</span><span class="sxs-lookup"><span data-stu-id="dc78e-186">Advanced Help Page customizations</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
