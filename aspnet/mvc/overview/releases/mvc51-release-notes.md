---
uid: mvc/overview/releases/mvc51-release-notes
title: Quelles sont les nouveautés dans ASP.NET MVC 5.1 | Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9a83a058-9b01-48aa-acce-ec041e694567
msc.legacyurl: /mvc/overview/releases/mvc51-release-notes
msc.type: authoredcontent
ms.openlocfilehash: 6cdf5ae42457ab10e30693a1b77b4aee65570be5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834447"
---
<a name="whats-new-in-aspnet-mvc-51"></a>Quelles sont les nouveautés dans ASP.NET MVC 5.1
====================
par [Microsoft](https://github.com/microsoft)

Cette rubrique décrit les nouveautés de ASP.NET Web MVC 5.1.

- [Configuration logicielle requise](#SoftwareRequirements)
- [Télécharger](#download)
- [Documentation](#documentation)
- [Nouvelles fonctionnalités dans ASP.NET MVC 5.1](#new-features)

    - [Améliorations du routage d’attribut](#AttributeRouting)
    - [Prise en charge d’amorçage pour les modèles de l’éditeur](#Bootstrap)
    - [Prise en charge de l’énumération dans les vues](#Enum)
    - [Validation non obstrusive pour les attributs de MinLength/MaxLength](#Unobtrusive)
    - [Prise en charge le contexte 'THI' dans Ajax discrète](#thisContext)
- [Problèmes connus et des modifications avec rupture](#KnownBreakingChanges)- [des correctifs de bogues](#bug-fixes)

<a id="SoftwareRequirements"></a>
## <a name="software-requirements"></a>Configuration logicielle

- Visual Studio 2012 : Télécharger [ASP.NET et Web Tools 2013.1 pour Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).
- Visual Studio 2013 : Téléchargement [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064). Cette mise à jour est nécessaire pour la modification des vues de Razor ASP.NET MVC 5.1.

<a id="download"></a>
## <a name="download"></a>Téléchargement

Les fonctionnalités de runtime sont publiées sous forme de packages NuGet dans la galerie NuGet. Tous les packages de runtime suivent le [Semver](http://semver.org/) spécification. Le dernier package de version RTM d’ASP.NET MVC 5.1 a la version suivante : « 5.1.2 ». Vous pouvez installer ou mettre à jour ces packages via [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/). La version inclut également des packages localisés correspondants sur NuGet.

Vous pouvez installer ou mettre à jour vers les packages NuGet publiées à l’aide de la Console du Gestionnaire de Package NuGet :

[!code-console[Main](mvc51-release-notes/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Documentation

Didacticiels et autres informations sur la version RTM d’ASP.NET MVC 5.1 sont disponibles à partir du site web ASP.NET ( https://www.asp.net). 

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-51"></a>Nouvelles fonctionnalités dans ASP.NET MVC 5.1

<a id="AttributeRouting"></a>

### <a name="attribute-routing-improvements"></a>Améliorations du routage d’attribut

 Routage maintenant contraintes prend en charge, l’activation de la gestion des versions et l’en-tête d’attributs en fonction de sélection d’itinéraire. De nombreux aspects des routes d’attribut sont désormais personnalisables par le biais de la `IDirectRouteFactory` interface et `RouteFactoryAttribute` classe. Le préfixe d’itinéraire est désormais extensible via le `IRoutePrefix` interface et `RoutePrefixAttribute` classe. 

<a id="Enum"></a>

### <a name="enum-support-in-views"></a>Prise en charge de l’énumération dans les vues

1. Nouvelle `@Html.EnumDropDownListFor()` méthodes d’assistance. Ceux-ci doivent être utilisés comme la plupart des programmes d’assistance HTML est à noter que l’expression doit correspondre à un [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) type ou un [Nullable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) où `T` est un [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) type. Utilisez `EnumHelper.IsValidForEnumHelper()` à vérifier ces exigences.
2. Nouvelle `EnumHelper.GetSelectList()` les méthodes qui retournent un `IList<SelectListItem>`. Cela est utile lorsque vous devez manipuler une liste de sélection avant d’appeler, par exemple, `@Html.DropDownListFor()`, ou lorsque vous souhaitez afficher les noms qui `@Html.EnumDropDownListFor()` montre.

Le code suivant montre ces API.

[!code-cshtml[Main](mvc51-release-notes/samples/sample2.cshtml)]

Vous pouvez voir un exemple complet [ici](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).

<a id="Bootstrap"></a>

### <a name="bootstrap-support-for-editor-templates"></a>Prise en charge d’amorçage pour les modèles de l’éditeur

Nous avons maintenant autoriser le passage dans les attributs HTML dans [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) comme un [objet anonyme](https://msdn.microsoft.com/en-us/library/bb397696.aspx).

Exemple :

[!code-cshtml[Main](mvc51-release-notes/samples/sample3.cshtml)]

<a id="Unobtrusive"></a>

### <a name="unobtrusive-validation-for-minlengthattribute-and-maxlengthattribute"></a>Validation non obstrusive pour MinLengthAttribute et MaxLengthAttribute

Validation côté client pour les types string et array sera désormais être pris en charge pour les propriétés décorée avec le [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) et [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) attributs.

<a id="thisContext"></a>

### <a name="supporting-the-this-context-in-unobtrusive-ajax"></a>Prise en charge le contexte 'THI' dans Ajax discrète

Les fonctions de rappel (`OnBegin, OnComplete, OnFailure, OnSuccess`) pourront désormais rechercher l’élément appelant via le `this` contexte. Exemple :

[!code-html[Main](mvc51-release-notes/samples/sample4.html)]

<a id="KnownBreakingChanges"></a>

## <a name="known-issues-and-breaking-changes"></a>Problèmes connus et les modifications avec rupture

### <a name="attribute-routing"></a>Routage par attributs

Ambiguïtés dans les correspondances de routage attribut signale désormais une erreur, plutôt que de choisir la première correspondance.

Les routes d’attribut sont interdit d’utiliser le `{controller}` paramètre et d’utiliser le `{action}` paramètre sur les itinéraires placés sur les actions. Utilisations de ces paramètres très aboutirait probablement à ambiguïtés. 

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>Génération de modèles automatique MVC/API Web dans un projet avec des résultats packages 5.1 dans des 5.0 packages pour ceux qui n’existent pas déjà dans le projet

La mise à jour des packages NuGet pour la version RTM d’ASP.NET MVC 5.1 n’actualise pas les outils de Visual Studio telles que la structure ASP.NET ou le modèle de projet d’Application Web ASP.NET. Ils utilisent la version précédente des packages de runtime ASP.NET (5.0.0.0). Par conséquent, la génération de modèles automatique ASP.NET installera la version précédente (5.0.0.0) des packages requis, si elles ne sont pas déjà disponibles dans vos projets. Toutefois, la génération de modèles automatique ASP.NET dans Visual Studio 2013 RTM ou 1 de mise à jour ne remplace pas les derniers packages dans vos projets. Si vous utilisez la génération de modèles automatique ASP.NET après la mise à jour les packages de vos projets Web API 2.1 ou ASP.NET MVC 5.1, vérifiez que les versions des API Web et ASP.NET MVC sont cohérentes. 

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>La coloration syntaxique pour les vues Razor dans Visual Studio 2013

Si vous mettez à jour vers la version RTM d’ASP.NET MVC 5.1 sans mettre à jour de Visual Studio 2013, vous n’obtiendrez pas prise en charge de l’éditeur Visual Studio pour la coloration syntaxique lorsque vous modifiez les vues Razor. Vous devez mettre à jour Visual Studio 2013 pour obtenir cette prise en charge. 

### <a name="type-renames"></a>Changements de noms de type

Certains des types utilisés pour l’extensibilité de routage d’attribut sont renommés dans 5.1 RTM.

| **Ancien nom de Type (5.1 RC)** | **Nouveau Type de nom (5.1 RTM)** |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Correctifs de bogues

Cette version inclut également plusieurs résolutions de bogues. Vous trouverez la liste complète ici :

- [5.1.0 les package](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 package de](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.1.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

Le 5.1.2 package contient des mises à jour d’IntelliSense, mais aucune des correctifs de bogues.
