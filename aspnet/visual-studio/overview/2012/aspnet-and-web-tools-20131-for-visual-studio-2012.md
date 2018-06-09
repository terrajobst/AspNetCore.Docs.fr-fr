---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: Notes de publication pour ASP.NET et outils Web 2013.1 pour Visual Studio 2012 | Documents Microsoft
author: microsoft
description: Ce document décrit la version de ASP.NET et Web Tools 2013.1 pour Visual Studio 2012.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2013
ms.topic: article
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: c11e2ef9c33b0cae1f196690533094ce1c342da5
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "28036425"
---
<a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="6b679-103">Notes de version de ASP.NET et outils Web 2013.1 pour Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="6b679-103">Release Notes for ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>
====================
<span data-ttu-id="6b679-104">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="6b679-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="6b679-105">Ce document décrit la version de ASP.NET et Web Tools 2013.1 pour Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="6b679-105">This document describes the release of ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>


## <a name="contents"></a><span data-ttu-id="6b679-106">Sommaire</span><span class="sxs-lookup"><span data-stu-id="6b679-106">Contents</span></span>

- [<span data-ttu-id="6b679-107">Notes d’installation</span><span class="sxs-lookup"><span data-stu-id="6b679-107">Installation Notes</span></span>](#install)
- [<span data-ttu-id="6b679-108">Configuration logicielle requise</span><span class="sxs-lookup"><span data-stu-id="6b679-108">Software Requirements</span></span>](#requirements)
- <span data-ttu-id="6b679-109">Nouvelles fonctionnalités dans ASP.NET et outils Web 2013.1 pour Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="6b679-109">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

    - [<span data-ttu-id="6b679-110">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="6b679-110">Bootstrap</span></span>](#bootstrap)
    - [<span data-ttu-id="6b679-111">Modèles</span><span class="sxs-lookup"><span data-stu-id="6b679-111">Templates</span></span>](#templates)

        - [<span data-ttu-id="6b679-112">Modèle ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="6b679-112">ASP.NET MVC 5 template</span></span>](#mvc5template)
        - [<span data-ttu-id="6b679-113">ASP.NET Web API 2 modèle</span><span class="sxs-lookup"><span data-stu-id="6b679-113">ASP.NET Web API 2 template</span></span>](#apitemplate)
        - [<span data-ttu-id="6b679-114">Modèles d’élément</span><span class="sxs-lookup"><span data-stu-id="6b679-114">Item Templates</span></span>](#itemtemplate)
    - [<span data-ttu-id="6b679-115">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="6b679-115">Entity Framework 6</span></span>](#ef6)
    - [<span data-ttu-id="6b679-116">Génération de modèles automatique ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6b679-116">ASP.NET Scaffolding</span></span>](#scaffold)
    - [<span data-ttu-id="6b679-117">Razor-éditeur</span><span class="sxs-lookup"><span data-stu-id="6b679-117">Razor Editor</span></span>](#razor)
    - [<span data-ttu-id="6b679-118">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="6b679-118">NuGet 2.7</span></span>](#nuget)
- <span data-ttu-id="6b679-119">Problèmes connus et les modifications avec rupture</span><span class="sxs-lookup"><span data-stu-id="6b679-119">Known Issues and Breaking Changes</span></span>

    - [<span data-ttu-id="6b679-120">Génération de modèles automatique ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6b679-120">ASP.NET Scaffolding</span></span>](#issuescaffolding)

        - [<span data-ttu-id="6b679-121">MVC et structure d’API Web - HTTP 404, erreur introuvable</span><span class="sxs-lookup"><span data-stu-id="6b679-121">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>](#404issue)
        - [<span data-ttu-id="6b679-122">Visual Studio Express 2012 pour Web cesse de fonctionner après l’ajout d’un élément généré automatiquement</span><span class="sxs-lookup"><span data-stu-id="6b679-122">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>](#expressissue)
    - [<span data-ttu-id="6b679-123">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="6b679-123">ASP.NET Razor 3</span></span>](#issuerazor)

        - [<span data-ttu-id="6b679-124">Affichage fichier cshtml et naviguer avec F5 provoque une erreur de serveur</span><span class="sxs-lookup"><span data-stu-id="6b679-124">Viewing cshtml file with Browse With or F5 causes a server error</span></span>](#browseissue)
        - [<span data-ttu-id="6b679-125">Tilde(~) et la réécriture d’Url</span><span class="sxs-lookup"><span data-stu-id="6b679-125">Url Rewrite and Tilde(~)</span></span>](#rewriteissue)
    - [<span data-ttu-id="6b679-126">Modèles</span><span class="sxs-lookup"><span data-stu-id="6b679-126">Templates</span></span>](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a><span data-ttu-id="6b679-127">Notes d’installation</span><span class="sxs-lookup"><span data-stu-id="6b679-127">Installation Notes</span></span>

<span data-ttu-id="6b679-128">[Installer](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) Web ASP.NET et outils 2013.1 pour Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="6b679-128">[Install](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="requirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="6b679-129">Configuration logicielle</span><span class="sxs-lookup"><span data-stu-id="6b679-129">Software Requirements</span></span>

<span data-ttu-id="6b679-130">Vous devez disposer de Visual Studio 2012 ou Visual Studio Express 2012 pour le Web.</span><span class="sxs-lookup"><span data-stu-id="6b679-130">You must have either Visual Studio 2012 or Visual Studio Express 2012 for Web.</span></span>

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="6b679-131">Nouvelles fonctionnalités dans ASP.NET et outils Web 2013.1 pour Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="6b679-131">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

<a id="bootstrap"></a>
### <a name="bootstrap"></a><span data-ttu-id="6b679-132">programme d’amorçage</span><span class="sxs-lookup"><span data-stu-id="6b679-132">Bootstrap</span></span>

<span data-ttu-id="6b679-133">Lorsque vous structurez les contrôleurs MVC 5 et les vues, le balisage pour les vues utilise [Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="6b679-133">When you scaffold MVC 5 controllers and views, the markup for the views uses [Bootstrap](http://getbootstrap.com/).</span></span>

<a id="templates"></a>
### <a name="templates"></a><span data-ttu-id="6b679-134">Modèles</span><span class="sxs-lookup"><span data-stu-id="6b679-134">Templates</span></span>

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a><span data-ttu-id="6b679-135">Modèle ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="6b679-135">ASP.NET MVC 5 template</span></span>

<span data-ttu-id="6b679-136">Nous avons ajouté un nouveau modèle MVC 5.</span><span class="sxs-lookup"><span data-stu-id="6b679-136">We added a new MVC 5 template.</span></span> <span data-ttu-id="6b679-137">Il fait référence à des derniers MVC 5 NuGet packages, et vous pouvez utiliser la structure pour ajouter des contrôleurs et vues.</span><span class="sxs-lookup"><span data-stu-id="6b679-137">It references the latest MVC 5 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a><span data-ttu-id="6b679-138">ASP.NET Web API 2 modèle</span><span class="sxs-lookup"><span data-stu-id="6b679-138">ASP.NET Web API 2 template</span></span>

<span data-ttu-id="6b679-139">Nous avons ajouté un nouveau modèle d’API Web 2.</span><span class="sxs-lookup"><span data-stu-id="6b679-139">We added a new Web API 2 template.</span></span> <span data-ttu-id="6b679-140">Il fait référence à des derniers packages NuGet 2 de l’API Web, et vous pouvez utiliser la structure pour ajouter des contrôleurs et vues.</span><span class="sxs-lookup"><span data-stu-id="6b679-140">It references the latest Web API 2 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="itemtemplate"></a>
#### <a name="item-templates"></a><span data-ttu-id="6b679-141">Modèles d’élément</span><span class="sxs-lookup"><span data-stu-id="6b679-141">Item Templates</span></span>

<span data-ttu-id="6b679-142">Nous avons ajouté de nouveaux modèles d’élément pour les vues MVC 5, Web Pages (Razor 3) et les contrôleurs de l’API Web 2.</span><span class="sxs-lookup"><span data-stu-id="6b679-142">We added new item templates for MVC 5 views, Web Pages (Razor 3), and Web API 2 controllers.</span></span> <span data-ttu-id="6b679-143">Les fichiers installent les packages NuGet associées au projet lors de l’ajout de nouveaux éléments.</span><span class="sxs-lookup"><span data-stu-id="6b679-143">They install the related NuGet packages to the project while adding new items.</span></span>

<a id="ef6"></a>
### <a name="entity-framework-6"></a><span data-ttu-id="6b679-144">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="6b679-144">Entity Framework 6</span></span>

<span data-ttu-id="6b679-145">Lorsque vous structurez un contrôleur MVC ou l’API Web à l’aide d’Entity Framework, nous utilisons Framework 6.</span><span class="sxs-lookup"><span data-stu-id="6b679-145">When you scaffold an MVC or Web API controller using Entity Framework, we use Framework 6.</span></span> <span data-ttu-id="6b679-146">Pour plus d’informations sur Entity Framework, consultez le [Entity Framework Version historique](https://msdn.com/data/jj574253).</span><span class="sxs-lookup"><span data-stu-id="6b679-146">For more information about Entity Framework, see the [Entity Framework Version History](https://msdn.com/data/jj574253).</span></span>

<span data-ttu-id="6b679-147">Vous pouvez également télécharger et installer les outils Entity Framework 6 pour Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="6b679-147">You can also download and install the Entity Framework 6 Tools for Visual Studio 2012.</span></span> <span data-ttu-id="6b679-148">Consultez le [obtenir Entity Framework](https://msdn.com/data/ee712906#tooling).</span><span class="sxs-lookup"><span data-stu-id="6b679-148">See the [Get Entity Framework](https://msdn.com/data/ee712906#tooling).</span></span>

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="6b679-149">Génération de modèles automatique ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6b679-149">ASP.NET Scaffolding</span></span>

<span data-ttu-id="6b679-150">Génération de modèles automatique ASP.NET est une infrastructure de génération de code pour les applications Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6b679-150">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="6b679-151">Elle rend faciles à ajouter du code réutilisable à votre projet interagit avec un modèle de données.</span><span class="sxs-lookup"><span data-stu-id="6b679-151">It makes it easy to add boilerplate code to your project that interacts with a data model.</span></span>

<span data-ttu-id="6b679-152">Dans les versions précédentes de Visual Studio, la structure a été limitée aux projets ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="6b679-152">In previous versions of Visual Studio, scaffolding was limited to ASP.NET MVC projects.</span></span> <span data-ttu-id="6b679-153">Cette mise à jour, vous pouvez maintenant utiliser la structure d’un projet ASP.NET, y compris les Web Forms.</span><span class="sxs-lookup"><span data-stu-id="6b679-153">With this update, you can now use scaffolding for any ASP.NET project, including Web Forms.</span></span> <span data-ttu-id="6b679-154">Cette mise à jour ne prend pas en charge la génération de pages pour un projet Web Forms, mais vous pouvez toujours utiliser la structure des Web Forms en ajoutant des dépendances MVC au projet.</span><span class="sxs-lookup"><span data-stu-id="6b679-154">This update does not support generating pages for a Web Forms project, but you can still use scaffolding with Web Forms by adding MVC dependencies to the project.</span></span> <span data-ttu-id="6b679-155">Prise en charge pour la génération de pages pour Web Forms sera ajoutée dans une prochaine mise à jour.</span><span class="sxs-lookup"><span data-stu-id="6b679-155">Support for generating pages for Web Forms will be added in a future update.</span></span>

<span data-ttu-id="6b679-156">Lorsque vous utilisez la structure, nous Assurez-vous que tous les requises dépendances sont installées dans le projet.</span><span class="sxs-lookup"><span data-stu-id="6b679-156">When using scaffolding, we ensure that all required dependencies are installed in the project.</span></span> <span data-ttu-id="6b679-157">Par exemple, si vous démarrez avec un projet Web Forms ASP.NET et puis utilisez la structure pour ajouter un contrôleur d’API Web, les packages NuGet requis et les références sont ajoutés à votre projet automatiquement.</span><span class="sxs-lookup"><span data-stu-id="6b679-157">For example, if you start with an ASP.NET Web Forms project and then use scaffolding to add a Web API Controller, the required NuGet packages and references are added to your project automatically.</span></span>

<span data-ttu-id="6b679-158">Pour ajouter la structure de MVC à un projet Web Forms, ajoutez un **un nouvel élément structuré** et sélectionnez **MVC 5 dépendances** dans la fenêtre de la boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="6b679-158">To add MVC scaffolding to a Web Forms project, add a **New Scaffolded Item** and select **MVC 5 Dependencies** in the dialog window.</span></span> <span data-ttu-id="6b679-159">Il existe deux options pour la génération de modèles automatique MVC ; Minimale et complète.</span><span class="sxs-lookup"><span data-stu-id="6b679-159">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="6b679-160">Si vous sélectionnez Minimal, seuls les packages NuGet et les références pour ASP.NET MVC sont ajoutés à votre projet.</span><span class="sxs-lookup"><span data-stu-id="6b679-160">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="6b679-161">Si vous sélectionnez l’option complet, les dépendances minimale sont ajoutés, ainsi que les fichiers de contenu requis pour un projet MVC.</span><span class="sxs-lookup"><span data-stu-id="6b679-161">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span>

<span data-ttu-id="6b679-162">Prise en charge de la génération de modèles automatique async contrôleurs utilise les nouvelles fonctionnalités asynchrones à partir de l’Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="6b679-162">Support for scaffolding async controllers uses the new async features from Entity Framework 6.</span></span>

<span data-ttu-id="6b679-163">Pour plus d’informations et des didacticiels, consultez [vue d’ensemble de la génération de modèles automatique ASP.NET](../2013/aspnet-scaffolding-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6b679-163">For more information and tutorials, see [ASP.NET Scaffolding Overview](../2013/aspnet-scaffolding-overview.md).</span></span> <span data-ttu-id="6b679-164">Ces didacticiels montrent la structure avec Visual Studio 2013, mais elles sont également applicables à ASP.NET et 2013.1 des outils Web pour Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="6b679-164">These tutorials show scaffolding with Visual Studio 2013, but they are also applicable to ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="razor"></a>
### <a name="razor-editor"></a><span data-ttu-id="6b679-165">Razor-éditeur</span><span class="sxs-lookup"><span data-stu-id="6b679-165">Razor Editor</span></span>

<span data-ttu-id="6b679-166">Cette mise à jour, Visual Studio 2012 prend désormais en charge l’édition/outils Razor 3.</span><span class="sxs-lookup"><span data-stu-id="6b679-166">With this update, Visual Studio 2012 now supports Razor 3 tooling/editing.</span></span>

<a id="nuget"></a>
### <a name="nuget-27"></a><span data-ttu-id="6b679-167">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="6b679-167">NuGet 2.7</span></span>

<span data-ttu-id="6b679-168">NuGet 2.7 comprend un ensemble complet de nouvelles fonctionnalités qui sont décrites en détail à [Notes de version 2.7 NuGet](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span><span class="sxs-lookup"><span data-stu-id="6b679-168">NuGet 2.7 includes a rich set of new features which are described in detail at [NuGet 2.7 Release Notes](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span></span>

<span data-ttu-id="6b679-169">Cette version de NuGet supprime la nécessité pour les utilisateurs à autoriser explicitement NuGet pour restaurer les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="6b679-169">This version of NuGet removes the need for users to explicitly allow NuGet to restore missing packages.</span></span> <span data-ttu-id="6b679-170">Lors de l’installation de NuGet 2.7, les utilisateurs implicitement le consentement pour restaurer automatiquement les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="6b679-170">When installing NuGet 2.7, users implicitly consent to automatically restoring missing packages.</span></span> <span data-ttu-id="6b679-171">Les utilisateurs peuvent choisir explicitement en dehors de la restauration de package via les paramètres de NuGet dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6b679-171">Users can explicitly opt out of package restoration through the NuGet settings in Visual Studio.</span></span> <span data-ttu-id="6b679-172">Cette modification simplifie le fonctionne de la restauration du package.</span><span class="sxs-lookup"><span data-stu-id="6b679-172">This change simplifies how package restoration works.</span></span>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="6b679-173">Problèmes connus et les modifications avec rupture</span><span class="sxs-lookup"><span data-stu-id="6b679-173">Known Issues and Breaking Changes</span></span>

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="6b679-174">Génération de modèles automatique ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6b679-174">ASP.NET Scaffolding</span></span>

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a><span data-ttu-id="6b679-175">MVC et structure d’API Web - HTTP 404, erreur introuvable</span><span class="sxs-lookup"><span data-stu-id="6b679-175">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>

<span data-ttu-id="6b679-176">Si vous rencontrez une erreur lors de l’ajout d’un élément généré automatiquement à un projet, il est possible que votre projet reste dans un état incohérent.</span><span class="sxs-lookup"><span data-stu-id="6b679-176">If you encounter an error when adding a scaffolded item to a project, it is possible your project will be left in an inconsistent state.</span></span> <span data-ttu-id="6b679-177">Certaines des modifications apportées à la structure d’être seront restaurée, mais d’autres modifications, telles que les packages NuGet installés, ne seront pas restaurées.</span><span class="sxs-lookup"><span data-stu-id="6b679-177">Some of the changes made be scaffolding will be rolled back but other changes, such as the installed NuGet packages, will not be rolled back.</span></span> <span data-ttu-id="6b679-178">Si les modifications de configuration de routage sont annulées, les utilisateurs recevront une erreur HTTP 404 lors de la navigation vers structuré des éléments.</span><span class="sxs-lookup"><span data-stu-id="6b679-178">If the routing configuration changes are rolled back, users will receive an HTTP 404 error when navigating to scaffolded items.</span></span>

<span data-ttu-id="6b679-179">Pour corriger cette erreur pour MVC, ajouter un nouvel élément de modèle généré automatiquement, puis sélectionnez les dépendances de MVC 5 (complète ou minimale).</span><span class="sxs-lookup"><span data-stu-id="6b679-179">To fix this error for MVC, add a new scaffolded item and select MVC 5 Dependencies (either Minimal or Full).</span></span> <span data-ttu-id="6b679-180">Ce processus sera ajouter toutes les modifications nécessaires à votre projet.</span><span class="sxs-lookup"><span data-stu-id="6b679-180">This process will add all of the required changes to your project.</span></span>

<span data-ttu-id="6b679-181">Pour corriger cette erreur pour l’API Web :</span><span class="sxs-lookup"><span data-stu-id="6b679-181">To fix this error for Web API:</span></span>

1. <span data-ttu-id="6b679-182">Ajoutez la classe WebApiConfig suivante à votre projet.</span><span class="sxs-lookup"><span data-stu-id="6b679-182">Add the following WebApiConfig class to your project.</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. <span data-ttu-id="6b679-183">Configurer WebApiConfig.Register dans l’Application\_Start (méthode) dans Global.asax, comme suit :</span><span class="sxs-lookup"><span data-stu-id="6b679-183">Configure WebApiConfig.Register in the Application\_Start method in Global.asax as follows:</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a><span data-ttu-id="6b679-184">Visual Studio Express 2012 pour Web cesse de fonctionner après l’ajout d’un élément généré automatiquement</span><span class="sxs-lookup"><span data-stu-id="6b679-184">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>

<span data-ttu-id="6b679-185">Si Visual Studio Express 2012 pour Web cesse de fonctionner après l’ajout d’un élément structuré avec Entity Framework (par exemple, Web API 2 contrôleur avec actions, utilisant Entity Framework), il est possible que Visual Studio Express Impossible de charger l’image native d’un assembly en fonction de System.Web.Extensions.</span><span class="sxs-lookup"><span data-stu-id="6b679-185">If Visual Studio Express 2012 for Web stops working after adding scaffolded item with Entity Framework (such as Web API 2 Controller with actions, using Entity Framework), it is possible that Visual Studio Express failed to load the native image of an assembly dependent on System.Web.Extensions.</span></span>

<span data-ttu-id="6b679-186">Pour corriger ce problème, configurez Visual Studio Express pour travailler avec l’image MSIL de System.Web.Extensions :</span><span class="sxs-lookup"><span data-stu-id="6b679-186">To correct this problem, configure Visual Studio Express to work with the MSIL image of System.Web.Extensions:</span></span>

1. <span data-ttu-id="6b679-187">Ouvrez l’invite de commandes en mode administrateur.</span><span class="sxs-lookup"><span data-stu-id="6b679-187">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="6b679-188">Accédez à %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE ou % ProgramFiles(x86) %\Microsoft Visual Studio 11.0\Common7\IDE (pour Windows 64 bits).</span><span class="sxs-lookup"><span data-stu-id="6b679-188">Go to %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE or %ProgramFiles(x86)%\Microsoft Visual Studio 11.0\Common7\IDE (for 64 bit Windows).</span></span>
3. <span data-ttu-id="6b679-189">Ouvrez VWDExpress.exe.config dans un éditeur de texte.</span><span class="sxs-lookup"><span data-stu-id="6b679-189">Open VWDExpress.exe.config in a text editor.</span></span>
4. <span data-ttu-id="6b679-190">Ajoutez la ligne suivante sous la &lt;configuration&gt;/&lt;runtime&gt; élément :</span><span class="sxs-lookup"><span data-stu-id="6b679-190">Add the following line under the &lt;configuration&gt;/&lt;runtime&gt; element:</span></span>  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. <span data-ttu-id="6b679-191">Redémarrez Visual Studio Express 2012 pour le Web.</span><span class="sxs-lookup"><span data-stu-id="6b679-191">Restart Visual Studio Express 2012 for Web.</span></span>

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a><span data-ttu-id="6b679-192">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="6b679-192">ASP.NET Razor 3</span></span>

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-withbrowse-withorf5causes-a-server-error"></a><span data-ttu-id="6b679-193">Affichage cshtml fichier withBrowse WithorF5causes une erreur de serveur</span><span class="sxs-lookup"><span data-stu-id="6b679-193">Viewing cshtml file withBrowse WithorF5causes a server error</span></span>

<span data-ttu-id="6b679-194">Lorsque vous créez un projet MVC 5 dans Visual Studio 2012 (ou ouvrir de projet Visual Studio 2012 un MVC 5 qui a été créé dans Visual Studio 2013) et que vous essayez d’afficher un fichier cshtml à l’aide de naviguer avec ou sur F5, vous recevrez une erreur indiquant - **erreur serveur dans Application '/'**.</span><span class="sxs-lookup"><span data-stu-id="6b679-194">When you create an MVC 5 project in Visual Studio 2012 (or open in Visual Studio 2012 an MVC 5 project that was created in Visual Studio 2013) and attempt to view a cshtml file by using Browse With or F5, you will receive an error that states - **Server Error in '/' Application**.</span></span> <span data-ttu-id="6b679-195">Le serveur tente d’accéder à `http://localhost:XXXX/Views/../XXXX.cshtml`</span><span class="sxs-lookup"><span data-stu-id="6b679-195">The server attempts to navigate to `http://localhost:XXXX/Views/../XXXX.cshtml`</span></span>

<span data-ttu-id="6b679-196">Pour résoudre ce problème, modifiez le **Action de démarrage** définition dans votre projet pour **Page spécifique**.</span><span class="sxs-lookup"><span data-stu-id="6b679-196">To resolve this issue, change the **Start Action** setting in your project to **Specific Page**.</span></span> <span data-ttu-id="6b679-197">Vous n’avez pas besoin fournir une valeur pour la page.</span><span class="sxs-lookup"><span data-stu-id="6b679-197">You do not need to provide a value for the page.</span></span>

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

<span data-ttu-id="6b679-198">Après avoir apporté cette modification, en sélectionnant F5 accède à la racine de votre application (`http://localhost:XXXX`).</span><span class="sxs-lookup"><span data-stu-id="6b679-198">After making this change, selecting F5 navigates to the root of your application (`http://localhost:XXXX`).</span></span> <span data-ttu-id="6b679-199">Ce comportement n’est pas le même que le comportement pour les projets MVC 5 dans Visual Studio 2013, où le **Page actuelle** paramètre lance la page ouverte.</span><span class="sxs-lookup"><span data-stu-id="6b679-199">This behavior is not the same as the behavior for MVC 5 projects in Visual Studio 2013, where the **Current Page** setting launches the open page.</span></span>

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a><span data-ttu-id="6b679-200">Tilde(~) et la réécriture d’Url</span><span class="sxs-lookup"><span data-stu-id="6b679-200">Url Rewrite and Tilde(~)</span></span>

<span data-ttu-id="6b679-201">Après la mise à niveau à 3 de Razor ASP.NET ou ASP.NET MVC 5, la notation tilde(~) peut ne plus fonctionner correctement si vous utilisez des URL réécritures.</span><span class="sxs-lookup"><span data-stu-id="6b679-201">After upgrading to ASP.NET Razor 3 or ASP.NET MVC 5, the tilde(~) notation may no longer work correctly if you are using URL rewrites.</span></span> <span data-ttu-id="6b679-202">La réécriture d’URL affecte la notation tilde(~) dans les éléments HTML tels que &lt;A /&gt;, &lt;SCRIPT /&gt;, &lt;lien /&gt;, et par conséquent le tilde mappe n’est plus dans le répertoire racine.</span><span class="sxs-lookup"><span data-stu-id="6b679-202">The URL rewrite affects the tilde(~) notation in HTML elements such as &lt;A/&gt;, &lt;SCRIPT/&gt;, &lt;LINK/&gt;, and as a result the tilde no longer maps to the root directory.</span></span>

<span data-ttu-id="6b679-203">Par exemple, si vous réécrivez les requêtes pour **asp.net/content** à **asp.net**, l’attribut href de &lt;A href = « ~/content/ » /&gt; se résout en **/content/ contenu /** au lieu de **/**.</span><span class="sxs-lookup"><span data-stu-id="6b679-203">For example, if you rewrite requests for **asp.net/content** to **asp.net**, the href attribute in &lt;A href="~/content/"/&gt; resolves to **/content/content/** instead of **/**.</span></span> <span data-ttu-id="6b679-204">Pour supprimer cette modification, vous pouvez définir le **IIS\_WasUrlRewritten** contexte false dans chaque Page Web ou dans **Application\_BeginRequest** dans Global.asax.</span><span class="sxs-lookup"><span data-stu-id="6b679-204">To suppress this change, you can set the **IIS\_WasUrlRewritten** context to false in each Web Page or in **Application\_BeginRequest** in Global.asax.</span></span>

<a id="templateissue"></a>
### <a name="templates"></a><span data-ttu-id="6b679-205">Modèles</span><span class="sxs-lookup"><span data-stu-id="6b679-205">Templates</span></span>

<span data-ttu-id="6b679-206">Lorsque vous créez ASP.NET MVC projets avec Visual Studio 2012 sur Windows 8.1 ou Windows Server 2012 R2, Visual Studio affiche un message d’erreur indiquant « Configuration Web [url] pour ASP.NET 4.5 échoué ».</span><span class="sxs-lookup"><span data-stu-id="6b679-206">When you create ASP.NET MVC projects with Visual Studio 2012 on Windows 8.1 or Windows Server 2012 R2, Visual Studio displays an error message that states "Configuring Web [url] for ASP.NET 4.5 failed."</span></span>

![Erreur de configuration](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

<span data-ttu-id="6b679-208">Vous voyez cette erreur, car Visual Studio 2012 n’active pas la fonctionnalité ASP.NET 4.5 lorsqu’il est installé sur ces versions de Windows.</span><span class="sxs-lookup"><span data-stu-id="6b679-208">You see this error because Visual Studio 2012 does not enable the ASP.NET 4.5 feature when it is installed on those releases of Windows.</span></span> <span data-ttu-id="6b679-209">Pour activer ASP.NET 4.5, effectuez les étapes décrites dans [ou désactiver des fonctionnalités Windows d’activer](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).</span><span class="sxs-lookup"><span data-stu-id="6b679-209">To enable ASP.NET 4.5, perform the steps described in [Turn Windows features on or off](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).</span></span>

![activer ou désactiver des fonctionnalités Windows](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

<span data-ttu-id="6b679-211">Vous pouvez également activer ASP.NET 4.5 via la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="6b679-211">Alternatively, you can enable ASP.NET 4.5 through the command line.</span></span>

1. <span data-ttu-id="6b679-212">Ouvrez l’invite de commandes en mode administrateur.</span><span class="sxs-lookup"><span data-stu-id="6b679-212">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="6b679-213">Exécutez la commande suivante pour activer ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="6b679-213">Run the following command to enable ASP.NET 4.5.</span></span>  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
