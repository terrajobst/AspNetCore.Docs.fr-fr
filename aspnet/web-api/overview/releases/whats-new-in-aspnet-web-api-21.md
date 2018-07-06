---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-21
title: Quelles sont les nouveautés dans ASP.NET Web API 2.1 | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 01/20/2014
ms.assetid: b6721bba-38c8-48c4-acbf-274c1a34e817
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 7952614456b1de24e4c618b9e7ba8448b2a01741
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838401"
---
<a name="whats-new-in-aspnet-web-api-21"></a>Quelles sont les nouveautés dans ASP.NET Web API 2.1
====================
par [Microsoft](https://github.com/microsoft)

Cette rubrique décrit les nouveautés de ASP.NET Web API 2.1.

- [Télécharger](#download)
- [Documentation](#documentation)
- [Nouvelles fonctionnalités dans ASP.NET Web API 2.1](#new-features)

    - [Gestion des erreurs globales](#global-error)
    - [Améliorations du routage d’attribut](#attribute-routing)
    - [Améliorations des pages d’aide](#help-page)
    - [Prise en charge IgnoreRoute](#ignoreroute)
    - [Formateur de Type de média BSON](#bson)
    - [Meilleure prise en charge pour les filtres d’Async](#async-filters)
    - [Analyse du client de mise en forme de la bibliothèque de la requête](#query-parsing)
- [Problèmes connus et les modifications avec rupture](#known-issues)
- [Correctifs de bogues](#bug-fixes)

<a id="download"></a>
## <a name="download"></a>Téléchargement

Les fonctionnalités de runtime sont publiées sous forme de packages NuGet dans la galerie NuGet. Tous les packages de runtime suivent le [Semver](http://semver.org/) spécification. Le dernier package ASP.NET Web API 2.1 RTM a la version suivante : « 5.1.2 ». Vous pouvez installer ou mettre à jour ces packages via [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/). La version inclut également des packages localisés correspondants sur NuGet.

Vous pouvez installer ou mettre à jour vers les packages NuGet publiées à l’aide de la Console du Gestionnaire de Package NuGet :

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Documentation

Didacticiels et autres informations sur ASP.NET Web API 2.1 RTM sont disponibles à partir du site web ASP.NET ([https://www.asp.net/web-api](../../index.md)).

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a>Nouvelles fonctionnalités dans ASP.NET Web API 2.1

<a id="global-error"></a>
### <a name="global-error-handling"></a>Gestion des erreurs globales

Toutes les exceptions non gérées peuvent maintenant être consignées via un mécanisme central et le comportement des exceptions non gérées peut être personnalisé.

Le framework prend en charge plusieurs enregistreurs des exceptions, qui tous voir l’exception non gérée et des informations sur le contexte dans lequel elle s’est produite, telles que la demande en cours de traitement en temps.

Par exemple, le code suivant utilise System.Diagnostics.TraceSource pour enregistrer toutes les exceptions non prises en charge :

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

Vous pouvez également remplacer le Gestionnaire d’exceptions par défaut, afin que vous puissiez personnaliser entièrement le message de réponse HTTP qui est envoyé lorsqu’une exception non gérée se produit.

Nous avons fourni un [exemple](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) qui consigne des exceptions non gérées par le biais de l’infrastructure ELMAH populaire.

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a>Améliorations du routage d’attribut

Routage par attributs maintenant prend en charge les contraintes, l’activation de la gestion des versions et la sélection de l’itinéraire basé sur l’en-tête. En outre, de nombreux aspects des routes d’attribut sont désormais personnalisables par le biais de la **IDirectRouteFactory** interface et **RouteFactoryAttribute** classe. Le préfixe d’itinéraire est désormais extensible via le **IRoutePrefix** interface et **RoutePrefixAttribute** classe.

Nous avons fourni un [exemple](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) qui utilise des contraintes pour filtrer dynamiquement des contrôleurs par un en-tête HTTP de « api-version ».

<a id="help-page"></a>
### <a name="help-page-improvements"></a>Améliorations des pages d’aide

Web API 2.1 inclut les améliorations suivantes à [Pages d’aide API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):

- Documentation des propriétés individuelles de paramètres ou types de retour des actions.
- Documentation du modèle des annotations de données.

La conception de l’interface utilisateur de pages d’aide a été également mis à jour, pour gérer ces modifications.

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a>Prise en charge IgnoreRoute

Web prend en charge de l’API 2.1 en ignorant les modèles d’URL de routage d’API Web, via un ensemble de **IgnoreRoute** méthodes d’extension sur **HttpRouteCollection**. Ces méthodes provoquent des API Web ignorer les URL qui correspond à un modèle spécifié et permettent à l’hôte appliquer un traitement supplémentaire si nécessaire.

L’exemple suivant ignore les URI qui commencent par un &quot;contenu&quot; segment :

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a>Formateur de Type de média BSON

Web API prend désormais en charge la [BSON](http://bsonspec.org/) format câble, à la fois sur le client et sur le serveur.

Pour activer BSON côté serveur, ajoutez le **BsonMediaTypeFormatter** à la collection de formateurs :

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

Voici comment un client .NET peut utiliser le format de BSON :

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

Nous avons fourni un [exemple](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) qui affiche à la fois le client et côté serveur.

Pour plus d’informations, consultez [prise en charge de BSON dans Web API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a>Meilleure prise en charge pour les filtres d’Async

API Web prend désormais en charge un moyen facile de créer des filtres qui s’exécutent de façon asynchrone. Cette fonctionnalité est utile est votre filtre a besoin pour effectuer une action asynchrone, telles que l’accès une base de données. Auparavant, pour créer un filtre asynchrone, vous deviez implémenter vous-même, l’interface de filtre, car les classes de base du filtre exposée uniquement les méthodes synchrones. Maintenant vous pouvez remplacer le virtuel `On*Async` méthodes du filtre de classe de base.

Exemple :

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

Le **AuthorizationFilterAttribute**, **ActionFilterAttribute**, et **ExceptionFilterAttribute** toutes les classes de prise en charge async dans Web API 2.1.

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a>Analyse du client de mise en forme de la bibliothèque de la requête

Auparavant, **System.Net.Http.Formatting** pris en charge l’analyse et la mise à jour les requêtes d’URI pour le code côté serveur, mais la bibliothèque portable équivalente n’a pas cette fonctionnalité. Dans Web API 2.1, une application cliente peut maintenant facilement analyser et mettre à jour d’une chaîne de requête.

Les exemples suivants montrent comment analyser, modifier et générer des requêtes d’URI. (Les exemples montrent une application de console par souci de simplicité.)

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Problèmes connus et les modifications avec rupture

Cette section décrit les problèmes connus et les modifications avec rupture dans ASP.NET Web API 2.1 RTM.

### <a name="attribute-routing"></a>Routage par attributs

Ambiguïtés dans les correspondances de routage attribut signalent désormais une erreur, plutôt que de choisir la première correspondance.

Les routes d’attribut sont interdit d’utiliser le *{controller}* paramètre et d’utiliser le *{action}* paramètre sur les itinéraires placés sur les actions. Ces paramètres seraient très probablement provoquer des ambiguïtés.

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>Génération de modèles automatique MVC/API Web dans un projet avec des résultats packages 5.1 dans des 5.0 packages pour ceux qui n’existent pas déjà dans le projet

La mise à jour des packages NuGet pour ASP.NET Web API 2.1 RTM ne met pas à jour les outils de Visual Studio, tels que de la génération de modèles automatique ASP.NET ou le modèle de projet d’Application Web ASP.NET. Ils utilisent la version précédente des packages de runtime ASP.NET (5.0.0.0). Par conséquent, la génération de modèles automatique ASP.NET installera la version précédente (5.0.0.0) des packages requis, si elles ne sont pas déjà disponibles dans vos projets. Toutefois, la génération de modèles automatique ASP.NET dans Visual Studio 2013 RTM ou 1 de mise à jour ne remplace pas les derniers packages dans vos projets.

Si vous utilisez la génération de modèles automatique ASP.NET après la mise à jour les packages Web API 2.1 ou ASP.NET MVC 5.1, vérifiez que les versions des API Web et MVC sont cohérentes.

### <a name="type-renames"></a>Changements de noms de type

Certains des types utilisés pour l’extensibilité de routage d’attribut ont été renommés à partir de la version RC à RTM 2.1.

| Ancien nom de type (2.1 RC) | Nouveau Type de nom (RTM 2.1) |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a>Filtres d’exception unwrap pas agréger des exceptions levées dans les actions asynchrones

Auparavant, si une action asynchrone a levé une **AggregateException**, un filtre d’exception serait désencapsuler l’exception, et **OnException** obtiendriez l’exception de base. 2.1, le filtre d’exception ne pas unwrap, et **OnException** Obtient la version d’origine **AggregateException**.

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Correctifs de bogues

Cette version inclut également plusieurs résolutions de bogues. Vous trouverez la liste complète ici :

- [5.1.0 les package](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 package de](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

Le 5.1.2 package contient des mises à jour d’IntelliSense, mais aucune des correctifs de bogues.
