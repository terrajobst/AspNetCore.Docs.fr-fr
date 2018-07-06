---
uid: mvc/overview/releases/mvc51-release-notes
title: Quelles sont les nouveautés dans ASP.NET MVC 5.1 | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 02/27/2014
ms.assetid: 9a83a058-9b01-48aa-acce-ec041e694567
msc.legacyurl: /mvc/overview/releases/mvc51-release-notes
msc.type: authoredcontent
ms.openlocfilehash: d2e67f64e725e73c3bf9021295da3fe870079a45
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825646"
---
<a name="whats-new-in-aspnet-mvc-51"></a><span data-ttu-id="05514-102">Quelles sont les nouveautés dans ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="05514-102">What's New in ASP.NET MVC 5.1</span></span>
====================
<span data-ttu-id="05514-103">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="05514-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="05514-104">Cette rubrique décrit les nouveautés de ASP.NET Web MVC 5.1.</span><span class="sxs-lookup"><span data-stu-id="05514-104">This topic describes what's new for ASP.NET Web MVC 5.1.</span></span>

- [<span data-ttu-id="05514-105">Configuration logicielle requise</span><span class="sxs-lookup"><span data-stu-id="05514-105">Software Requirements</span></span>](#SoftwareRequirements)
- [<span data-ttu-id="05514-106">Télécharger</span><span class="sxs-lookup"><span data-stu-id="05514-106">Download</span></span>](#download)
- [<span data-ttu-id="05514-107">Documentation</span><span class="sxs-lookup"><span data-stu-id="05514-107">Documentation</span></span>](#documentation)
- [<span data-ttu-id="05514-108">Nouvelles fonctionnalités dans ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="05514-108">New Features in ASP.NET MVC 5.1</span></span>](#new-features)

    - [<span data-ttu-id="05514-109">Améliorations du routage d’attribut</span><span class="sxs-lookup"><span data-stu-id="05514-109">Attribute routing improvements</span></span>](#AttributeRouting)
    - [<span data-ttu-id="05514-110">Prise en charge d’amorçage pour les modèles de l’éditeur</span><span class="sxs-lookup"><span data-stu-id="05514-110">Bootstrap support for editor templates</span></span>](#Bootstrap)
    - [<span data-ttu-id="05514-111">Prise en charge de l’énumération dans les vues</span><span class="sxs-lookup"><span data-stu-id="05514-111">Enum support in views</span></span>](#Enum)
    - [<span data-ttu-id="05514-112">Validation non obstrusive pour les attributs de MinLength/MaxLength</span><span class="sxs-lookup"><span data-stu-id="05514-112">Unobtrusive validation for MinLength/MaxLength Attributes</span></span>](#Unobtrusive)
    - [<span data-ttu-id="05514-113">Prise en charge le contexte 'THI' dans Ajax discrète</span><span class="sxs-lookup"><span data-stu-id="05514-113">Supporting the ‘this' context in Unobtrusive Ajax</span></span>](#thisContext)
- <span data-ttu-id="05514-114">[Problèmes connus et des modifications avec rupture](#KnownBreakingChanges)- [des correctifs de bogues](#bug-fixes)</span><span class="sxs-lookup"><span data-stu-id="05514-114">[Known Issues and Breaking Changes](#KnownBreakingChanges)- [Bug Fixes](#bug-fixes)</span></span>

<a id="SoftwareRequirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="05514-115">Configuration logicielle</span><span class="sxs-lookup"><span data-stu-id="05514-115">Software Requirements</span></span>

- <span data-ttu-id="05514-116">Visual Studio 2012 : Télécharger [ASP.NET et Web Tools 2013.1 pour Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).</span><span class="sxs-lookup"><span data-stu-id="05514-116">Visual Studio 2012: Download [ASP.NET and Web Tools 2013.1 for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).</span></span>
- <span data-ttu-id="05514-117">Visual Studio 2013 : Téléchargement [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064).</span><span class="sxs-lookup"><span data-stu-id="05514-117">Visual Studio 2013: Download [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064).</span></span> <span data-ttu-id="05514-118">Cette mise à jour est nécessaire pour la modification des vues de Razor ASP.NET MVC 5.1.</span><span class="sxs-lookup"><span data-stu-id="05514-118">This update is needed for editing ASP.NET MVC 5.1 Razor Views.</span></span>

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="05514-119">Téléchargement</span><span class="sxs-lookup"><span data-stu-id="05514-119">Download</span></span>

<span data-ttu-id="05514-120">Les fonctionnalités de runtime sont publiées sous forme de packages NuGet dans la galerie NuGet.</span><span class="sxs-lookup"><span data-stu-id="05514-120">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="05514-121">Tous les packages de runtime suivent le [Semver](http://semver.org/) spécification.</span><span class="sxs-lookup"><span data-stu-id="05514-121">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="05514-122">Le dernier package de version RTM d’ASP.NET MVC 5.1 a la version suivante : « 5.1.2 ».</span><span class="sxs-lookup"><span data-stu-id="05514-122">The latest ASP.NET MVC 5.1 RTM package has the following version: "5.1.2".</span></span> <span data-ttu-id="05514-123">Vous pouvez installer ou mettre à jour ces packages via [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/).</span><span class="sxs-lookup"><span data-stu-id="05514-123">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/).</span></span> <span data-ttu-id="05514-124">La version inclut également des packages localisés correspondants sur NuGet.</span><span class="sxs-lookup"><span data-stu-id="05514-124">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="05514-125">Vous pouvez installer ou mettre à jour vers les packages NuGet publiées à l’aide de la Console du Gestionnaire de Package NuGet :</span><span class="sxs-lookup"><span data-stu-id="05514-125">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](mvc51-release-notes/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="05514-126">Documentation</span><span class="sxs-lookup"><span data-stu-id="05514-126">Documentation</span></span>

<span data-ttu-id="05514-127">Didacticiels et autres informations sur la version RTM d’ASP.NET MVC 5.1 sont disponibles à partir du site web ASP.NET ( https://www.asp.net).</span><span class="sxs-lookup"><span data-stu-id="05514-127">Tutorials and other information about ASP.NET MVC 5.1 RTM are available from the ASP.NET web site ( https://www.asp.net).</span></span> 

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-51"></a><span data-ttu-id="05514-128">Nouvelles fonctionnalités dans ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="05514-128">New Features in ASP.NET MVC 5.1</span></span>

<a id="AttributeRouting"></a>

### <a name="attribute-routing-improvements"></a><span data-ttu-id="05514-129">Améliorations du routage d’attribut</span><span class="sxs-lookup"><span data-stu-id="05514-129">Attribute routing improvements</span></span>

 <span data-ttu-id="05514-130">Routage maintenant contraintes prend en charge, l’activation de la gestion des versions et l’en-tête d’attributs en fonction de sélection d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="05514-130">Attribute routing now supports constraints, enabling versioning and header based route selection.</span></span> <span data-ttu-id="05514-131">De nombreux aspects des routes d’attribut sont désormais personnalisables par le biais de la `IDirectRouteFactory` interface et `RouteFactoryAttribute` classe.</span><span class="sxs-lookup"><span data-stu-id="05514-131">Many aspects of attribute routes are now customizable via the `IDirectRouteFactory` interface and `RouteFactoryAttribute` class.</span></span> <span data-ttu-id="05514-132">Le préfixe d’itinéraire est désormais extensible via le `IRoutePrefix` interface et `RoutePrefixAttribute` classe.</span><span class="sxs-lookup"><span data-stu-id="05514-132">The route prefix is now extensible via the `IRoutePrefix` interface and `RoutePrefixAttribute` class.</span></span> 

<a id="Enum"></a>

### <a name="enum-support-in-views"></a><span data-ttu-id="05514-133">Prise en charge de l’énumération dans les vues</span><span class="sxs-lookup"><span data-stu-id="05514-133">Enum support in views</span></span>

1. <span data-ttu-id="05514-134">Nouvelle `@Html.EnumDropDownListFor()` méthodes d’assistance.</span><span class="sxs-lookup"><span data-stu-id="05514-134">New `@Html.EnumDropDownListFor()` helper methods.</span></span> <span data-ttu-id="05514-135">Ceux-ci doivent être utilisés comme la plupart des programmes d’assistance HTML est à noter que l’expression doit correspondre à un [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) type ou un [Nullable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) où `T` est un [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) type.</span><span class="sxs-lookup"><span data-stu-id="05514-135">These should be used like most of the HTML helpers with the caveat that the expression must evaluate to an [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) type or a [Nullable&lt;T&gt;](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) where `T` is an [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) type.</span></span> <span data-ttu-id="05514-136">Utilisez `EnumHelper.IsValidForEnumHelper()` à vérifier ces exigences.</span><span class="sxs-lookup"><span data-stu-id="05514-136">Use `EnumHelper.IsValidForEnumHelper()` to check these requirements.</span></span>
2. <span data-ttu-id="05514-137">Nouvelle `EnumHelper.GetSelectList()` les méthodes qui retournent un `IList<SelectListItem>`.</span><span class="sxs-lookup"><span data-stu-id="05514-137">New `EnumHelper.GetSelectList()` methods which return an `IList<SelectListItem>`.</span></span> <span data-ttu-id="05514-138">Cela est utile lorsque vous devez manipuler une liste de sélection avant d’appeler, par exemple, `@Html.DropDownListFor()`, ou lorsque vous souhaitez afficher les noms qui `@Html.EnumDropDownListFor()` montre.</span><span class="sxs-lookup"><span data-stu-id="05514-138">This is useful when you need to manipulate a select list prior to calling, for example, `@Html.DropDownListFor()`, or when you wish to display the names which `@Html.EnumDropDownListFor()` shows.</span></span>

<span data-ttu-id="05514-139">Le code suivant montre ces API.</span><span class="sxs-lookup"><span data-stu-id="05514-139">The following code shows these APIs.</span></span>

[!code-cshtml[Main](mvc51-release-notes/samples/sample2.cshtml)]

<span data-ttu-id="05514-140">Vous pouvez voir un exemple complet [ici](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).</span><span class="sxs-lookup"><span data-stu-id="05514-140">You can see a complete example [here](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).</span></span>

<a id="Bootstrap"></a>

### <a name="bootstrap-support-for-editor-templates"></a><span data-ttu-id="05514-141">Prise en charge d’amorçage pour les modèles de l’éditeur</span><span class="sxs-lookup"><span data-stu-id="05514-141">Bootstrap support for editor templates</span></span>

<span data-ttu-id="05514-142">Nous avons maintenant autoriser le passage dans les attributs HTML dans [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) comme un [objet anonyme](https://msdn.microsoft.com/en-us/library/bb397696.aspx).</span><span class="sxs-lookup"><span data-stu-id="05514-142">We now allow passing in HTML attributes in [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) as an [anonymous object](https://msdn.microsoft.com/en-us/library/bb397696.aspx).</span></span>

<span data-ttu-id="05514-143">Exemple :</span><span class="sxs-lookup"><span data-stu-id="05514-143">For example:</span></span>

[!code-cshtml[Main](mvc51-release-notes/samples/sample3.cshtml)]

<a id="Unobtrusive"></a>

### <a name="unobtrusive-validation-for-minlengthattribute-and-maxlengthattribute"></a><span data-ttu-id="05514-144">Validation non obstrusive pour MinLengthAttribute et MaxLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="05514-144">Unobtrusive validation for MinLengthAttribute and MaxLengthAttribute</span></span>

<span data-ttu-id="05514-145">Validation côté client pour les types string et array sera désormais être pris en charge pour les propriétés décorée avec le [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) et [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) attributs.</span><span class="sxs-lookup"><span data-stu-id="05514-145">Client-side validation for string and array types will now be supported for properties decorated with the [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) and [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) attributes.</span></span>

<a id="thisContext"></a>

### <a name="supporting-the-this-context-in-unobtrusive-ajax"></a><span data-ttu-id="05514-146">Prise en charge le contexte 'THI' dans Ajax discrète</span><span class="sxs-lookup"><span data-stu-id="05514-146">Supporting the ‘this' context in Unobtrusive Ajax</span></span>

<span data-ttu-id="05514-147">Les fonctions de rappel (`OnBegin, OnComplete, OnFailure, OnSuccess`) pourront désormais rechercher l’élément appelant via le `this` contexte.</span><span class="sxs-lookup"><span data-stu-id="05514-147">The callback functions (`OnBegin, OnComplete, OnFailure, OnSuccess`) will now be able to locate the invoking element via the `this` context.</span></span> <span data-ttu-id="05514-148">Exemple :</span><span class="sxs-lookup"><span data-stu-id="05514-148">For example:</span></span>

[!code-html[Main](mvc51-release-notes/samples/sample4.html)]

<a id="KnownBreakingChanges"></a>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="05514-149">Problèmes connus et les modifications avec rupture</span><span class="sxs-lookup"><span data-stu-id="05514-149">Known Issues and Breaking Changes</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="05514-150">Routage par attributs</span><span class="sxs-lookup"><span data-stu-id="05514-150">Attribute Routing</span></span>

<span data-ttu-id="05514-151">Ambiguïtés dans les correspondances de routage attribut signale désormais une erreur, plutôt que de choisir la première correspondance.</span><span class="sxs-lookup"><span data-stu-id="05514-151">Ambiguities in attribute routing matches will now report an error rather than choosing the first match.</span></span>

<span data-ttu-id="05514-152">Les routes d’attribut sont interdit d’utiliser le `{controller}` paramètre et d’utiliser le `{action}` paramètre sur les itinéraires placés sur les actions.</span><span class="sxs-lookup"><span data-stu-id="05514-152">Attribute routes are prohibited from using the `{controller}` parameter, and from using the `{action}` parameter on routes placed on actions.</span></span> <span data-ttu-id="05514-153">Utilisations de ces paramètres très aboutirait probablement à ambiguïtés.</span><span class="sxs-lookup"><span data-stu-id="05514-153">Uses of these parameters would very likely lead to ambiguities.</span></span> 

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a><span data-ttu-id="05514-154">Génération de modèles automatique MVC/API Web dans un projet avec des résultats packages 5.1 dans des 5.0 packages pour ceux qui n’existent pas déjà dans le projet</span><span class="sxs-lookup"><span data-stu-id="05514-154">Scaffolding MVC/Web API into a project with 5.1 packages results in 5.0 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="05514-155">La mise à jour des packages NuGet pour la version RTM d’ASP.NET MVC 5.1 n’actualise pas les outils de Visual Studio telles que la structure ASP.NET ou le modèle de projet d’Application Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="05514-155">Updating NuGet packages for ASP.NET MVC 5.1 RTM does not update the Visual Studio tools such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="05514-156">Ils utilisent la version précédente des packages de runtime ASP.NET (5.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="05514-156">They use the previous version of the ASP.NET runtime packages (5.0.0.0).</span></span> <span data-ttu-id="05514-157">Par conséquent, la génération de modèles automatique ASP.NET installera la version précédente (5.0.0.0) des packages requis, si elles ne sont pas déjà disponibles dans vos projets.</span><span class="sxs-lookup"><span data-stu-id="05514-157">As a result, the ASP.NET scaffolding will install the previous version (5.0.0.0) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="05514-158">Toutefois, la génération de modèles automatique ASP.NET dans Visual Studio 2013 RTM ou 1 de mise à jour ne remplace pas les derniers packages dans vos projets.</span><span class="sxs-lookup"><span data-stu-id="05514-158">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span> <span data-ttu-id="05514-159">Si vous utilisez la génération de modèles automatique ASP.NET après la mise à jour les packages de vos projets Web API 2.1 ou ASP.NET MVC 5.1, vérifiez que les versions des API Web et ASP.NET MVC sont cohérentes.</span><span class="sxs-lookup"><span data-stu-id="05514-159">If you use ASP.NET scaffolding after updating the packages of your projects to Web API 2.1 or ASP.NET MVC 5.1, make sure the versions of Web API and ASP.NET MVC are consistent.</span></span> 

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a><span data-ttu-id="05514-160">La coloration syntaxique pour les vues Razor dans Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="05514-160">Syntax Highlighting for Razor Views in Visual Studio 2013</span></span>

<span data-ttu-id="05514-161">Si vous mettez à jour vers la version RTM d’ASP.NET MVC 5.1 sans mettre à jour de Visual Studio 2013, vous n’obtiendrez pas prise en charge de l’éditeur Visual Studio pour la coloration syntaxique lorsque vous modifiez les vues Razor.</span><span class="sxs-lookup"><span data-stu-id="05514-161">If you update to ASP.NET MVC 5.1 RTM without updating Visual Studio 2013, you will not get Visual Studio editor support for syntax highlighting while editing the Razor views.</span></span> <span data-ttu-id="05514-162">Vous devez mettre à jour Visual Studio 2013 pour obtenir cette prise en charge.</span><span class="sxs-lookup"><span data-stu-id="05514-162">You will need to update Visual Studio 2013 to get this support.</span></span> 

### <a name="type-renames"></a><span data-ttu-id="05514-163">Changements de noms de type</span><span class="sxs-lookup"><span data-stu-id="05514-163">Type Renames</span></span>

<span data-ttu-id="05514-164">Certains des types utilisés pour l’extensibilité de routage d’attribut sont renommés dans 5.1 RTM.</span><span class="sxs-lookup"><span data-stu-id="05514-164">Some of the types used for attribute routing extensibility are renamed in 5.1 RTM.</span></span>

| <span data-ttu-id="05514-165">**Ancien nom de Type (5.1 RC)**</span><span class="sxs-lookup"><span data-stu-id="05514-165">**Old Type Name (5.1 RC)**</span></span> | <span data-ttu-id="05514-166">**Nouveau Type de nom (5.1 RTM)**</span><span class="sxs-lookup"><span data-stu-id="05514-166">**New Type Name (5.1 RTM)**</span></span> |
| --- | --- |
| <span data-ttu-id="05514-167">IDirectRouteProvider</span><span class="sxs-lookup"><span data-stu-id="05514-167">IDirectRouteProvider</span></span> | <span data-ttu-id="05514-168">IDirectRouteFactory</span><span class="sxs-lookup"><span data-stu-id="05514-168">IDirectRouteFactory</span></span> |
| <span data-ttu-id="05514-169">RouteProviderAttribute</span><span class="sxs-lookup"><span data-stu-id="05514-169">RouteProviderAttribute</span></span> | <span data-ttu-id="05514-170">RouteFactoryAttribute</span><span class="sxs-lookup"><span data-stu-id="05514-170">RouteFactoryAttribute</span></span> |
| <span data-ttu-id="05514-171">DirectRouteProviderContext</span><span class="sxs-lookup"><span data-stu-id="05514-171">DirectRouteProviderContext</span></span> | <span data-ttu-id="05514-172">DirectRouteFactoryContext</span><span class="sxs-lookup"><span data-stu-id="05514-172">DirectRouteFactoryContext</span></span> |

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a><span data-ttu-id="05514-173">Correctifs de bogues</span><span class="sxs-lookup"><span data-stu-id="05514-173">Bug Fixes</span></span>

<span data-ttu-id="05514-174">Cette version inclut également plusieurs résolutions de bogues.</span><span class="sxs-lookup"><span data-stu-id="05514-174">This release also includes several bug fixes.</span></span> <span data-ttu-id="05514-175">Vous trouverez la liste complète ici :</span><span class="sxs-lookup"><span data-stu-id="05514-175">You can find the complete list here:</span></span>

- [<span data-ttu-id="05514-176">5.1.0 les package</span><span class="sxs-lookup"><span data-stu-id="05514-176">5.1.0 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [<span data-ttu-id="05514-177">5.1.1 package de</span><span class="sxs-lookup"><span data-stu-id="05514-177">5.1.1 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.1.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<span data-ttu-id="05514-178">Le 5.1.2 package contient des mises à jour d’IntelliSense, mais aucune des correctifs de bogues.</span><span class="sxs-lookup"><span data-stu-id="05514-178">The 5.1.2 package contains IntelliSense updates but no bug fixes.</span></span>
