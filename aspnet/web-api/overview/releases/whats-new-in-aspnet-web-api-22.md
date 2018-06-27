---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-22
title: Quelles sont les nouveautés dans ASP.NET Web API 2.2 | Documents Microsoft
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/25/2014
ms.topic: article
ms.assetid: 99c59ae4-167e-4f66-a6cd-d3f1098c4e4a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 89b065fccd0e4864f4a24c37b4caa29a1e127840
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961297"
---
<a name="whats-new-in-aspnet-web-api-22"></a><span data-ttu-id="ee81c-102">Quelles sont les nouveautés dans ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="ee81c-102">What's New in ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="ee81c-103">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="ee81c-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="ee81c-104">Cette rubrique décrit les nouveautés de ASP.NET Web API 2.2.</span><span class="sxs-lookup"><span data-stu-id="ee81c-104">This topic describes what's new for ASP.NET Web API 2.2.</span></span>

- [<span data-ttu-id="ee81c-105">Télécharger</span><span class="sxs-lookup"><span data-stu-id="ee81c-105">Download</span></span>](#download)
- [<span data-ttu-id="ee81c-106">Documentation</span><span class="sxs-lookup"><span data-stu-id="ee81c-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="ee81c-107">Nouvelles fonctionnalités dans ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="ee81c-107">New Features in ASP.NET Web API 2.2</span></span>](#newf)

    - [<span data-ttu-id="ee81c-108">OData v4</span><span class="sxs-lookup"><span data-stu-id="ee81c-108">OData v4</span></span>](#OData)
    - [<span data-ttu-id="ee81c-109">Améliorations du routage d’attribut</span><span class="sxs-lookup"><span data-stu-id="ee81c-109">Attribute Routing Improvements</span></span>](#ARI)
    - [<span data-ttu-id="ee81c-110">Prise en charge de Client de l’API Web pour Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="ee81c-110">Web API Client support for Windows Phone 8.1</span></span>](#phone)
- [<span data-ttu-id="ee81c-111">Problèmes connus et les modifications avec rupture</span><span class="sxs-lookup"><span data-stu-id="ee81c-111">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="ee81c-112">Correctifs de bogues</span><span class="sxs-lookup"><span data-stu-id="ee81c-112">Bug Fixes</span></span>](#bug-fixes)
- [<span data-ttu-id="ee81c-113">Microsoft.AspNet.OData 5.2.1</span><span class="sxs-lookup"><span data-stu-id="ee81c-113">Microsoft.AspNet.OData 5.2.1</span></span>](#odata521)
- [<span data-ttu-id="ee81c-114">Microsoft.AspNet.WebAPI 5.2.2</span><span class="sxs-lookup"><span data-stu-id="ee81c-114">Microsoft.AspNet.WebAPI 5.2.2</span></span>](#522RC)
- [<span data-ttu-id="ee81c-115">Microsoft.AspNet.WebAPI 5.2.3 bêta</span><span class="sxs-lookup"><span data-stu-id="ee81c-115">Microsoft.AspNet.WebAPI 5.2.3 Beta</span></span>](#523)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="ee81c-116">Téléchargement</span><span class="sxs-lookup"><span data-stu-id="ee81c-116">Download</span></span>

<span data-ttu-id="ee81c-117">Les fonctions d’exécution sont publiées en tant que packages NuGet lors de la galerie NuGet.</span><span class="sxs-lookup"><span data-stu-id="ee81c-117">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="ee81c-118">Tous les packages de runtime suivent le [contrôle de version sémantique](http://semver.org/) spécification.</span><span class="sxs-lookup"><span data-stu-id="ee81c-118">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="ee81c-119">Le dernier package de ASP.NET Web API 2.2 a la version suivante : « 5.2.0 ».</span><span class="sxs-lookup"><span data-stu-id="ee81c-119">The latest ASP.NET Web API 2.2 package has the following version: "5.2.0".</span></span> <span data-ttu-id="ee81c-120">Vous pouvez installer ou mettre à jour ces packages via [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span><span class="sxs-lookup"><span data-stu-id="ee81c-120">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span></span> <span data-ttu-id="ee81c-121">Cette version inclut également des packages localisés correspondants sur NuGet.</span><span class="sxs-lookup"><span data-stu-id="ee81c-121">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="ee81c-122">Vous pouvez installer ou mettre à jour pour les packages NuGet publiés à l’aide de la Console du Gestionnaire de Package NuGet :</span><span class="sxs-lookup"><span data-stu-id="ee81c-122">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="ee81c-123">Documentation</span><span class="sxs-lookup"><span data-stu-id="ee81c-123">Documentation</span></span>

<span data-ttu-id="ee81c-124">Didacticiels et autres informations sur ASP.NET Web API 2.2 sont disponibles à partir du site web ASP.NET ([https://www.asp.net/web-api](../../index.md)).</span><span class="sxs-lookup"><span data-stu-id="ee81c-124">Tutorials and other information about ASP.NET Web API 2.2 are available from the ASP.NET web site ([https://www.asp.net/web-api](../../index.md)).</span></span>

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a><span data-ttu-id="ee81c-125">Nouvelles fonctionnalités dans ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="ee81c-125">New Features in ASP.NET Web API 2.2</span></span>

<a id="OData"></a>
### <a name="odata-v4"></a><span data-ttu-id="ee81c-126">OData v4</span><span class="sxs-lookup"><span data-stu-id="ee81c-126">OData v4</span></span>

<span data-ttu-id="ee81c-127">Cette version ajoute la prise en charge de protocole OData v4.</span><span class="sxs-lookup"><span data-stu-id="ee81c-127">This release adds support for the OData v4 protocol.</span></span> <span data-ttu-id="ee81c-128">Pour plus d’informations, consultez le [Web API OData v4 documentation.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="ee81c-128">For more information, see the [Web API OData v4 documentation.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)</span></span>

<span data-ttu-id="ee81c-129">Voici quelques-unes des principales fonctionnalités et des modifications pour OData v4 :</span><span class="sxs-lookup"><span data-stu-id="ee81c-129">Here are some of the key features and changes for OData v4:</span></span>

- [<span data-ttu-id="ee81c-130">Prise en charge pour les propriétés d’alias dans le modèle d’OData</span><span class="sxs-lookup"><span data-stu-id="ee81c-130">Support for aliasing properties in OData model</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [<span data-ttu-id="ee81c-131">Prise en charge de ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute et ConcurrencyCheckAttribute dans ODataConventionModelBuilder</span><span class="sxs-lookup"><span data-stu-id="ee81c-131">Support for ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute and ConcurrencyCheckAttribute in ODataConventionModelBuilder</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [<span data-ttu-id="ee81c-132">Permettent de fournir un titre convivial pour les actions</span><span class="sxs-lookup"><span data-stu-id="ee81c-132">Provide ability to supply friendly Title for actions</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- <span data-ttu-id="ee81c-133">Intégrer ODL UriParser</span><span class="sxs-lookup"><span data-stu-id="ee81c-133">Integrate with ODL UriParser</span></span>
- <span data-ttu-id="ee81c-134">Prise en charge de [enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [relation contenant-contenu](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) et [singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)</span><span class="sxs-lookup"><span data-stu-id="ee81c-134">Support for [enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [containment](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) and [singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)</span></span>
- <span data-ttu-id="ee81c-135">Prise en charge par les types primitifs</span><span class="sxs-lookup"><span data-stu-id="ee81c-135">Support cast for primitive types</span></span>
- [<span data-ttu-id="ee81c-136">Prise en charge OData (fonction)</span><span class="sxs-lookup"><span data-stu-id="ee81c-136">Added OData function support</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [<span data-ttu-id="ee81c-137">Alias de paramètre de prise en charge pour les appels de fonction</span><span class="sxs-lookup"><span data-stu-id="ee81c-137">Support parameter aliases for function calls</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [<span data-ttu-id="ee81c-138">Prend en charge la convention d’affectation de noms de casse mixte dans le modèle</span><span class="sxs-lookup"><span data-stu-id="ee81c-138">Support camel case naming convention in model</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- <span data-ttu-id="ee81c-139">Prise en charge pour cast() dans $filter</span><span class="sxs-lookup"><span data-stu-id="ee81c-139">Support for cast() in $filter</span></span>
- <span data-ttu-id="ee81c-140">Prise en charge pour le type complexe ouvert</span><span class="sxs-lookup"><span data-stu-id="ee81c-140">Support for open complex type</span></span>
- <span data-ttu-id="ee81c-141">Supprimé EntitySetController et AsyncEntitySetController</span><span class="sxs-lookup"><span data-stu-id="ee81c-141">Removed EntitySetController and AsyncEntitySetController</span></span>
- [<span data-ttu-id="ee81c-142">Modifié $link pour $ref</span><span class="sxs-lookup"><span data-stu-id="ee81c-142">Changed $link to $ref</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [<span data-ttu-id="ee81c-143">Attribut routage prise en charge</span><span class="sxs-lookup"><span data-stu-id="ee81c-143">Added Attribute routing support</span></span>](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- <span data-ttu-id="ee81c-144">Utilise les bibliothèques d’OData Core 6.4.0</span><span class="sxs-lookup"><span data-stu-id="ee81c-144">Uses OData Core Libraries 6.4.0</span></span>

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a><span data-ttu-id="ee81c-145">Améliorations du routage d’attribut</span><span class="sxs-lookup"><span data-stu-id="ee81c-145">Attribute Routing Improvements</span></span>

<span data-ttu-id="ee81c-146">Attribut routage maintenant fournit un point d’extensibilité appelé IDirectRouteProvider, ce qui permet un contrôle total sur la façon dont les itinéraires d’attribut sont détectées et configurés.</span><span class="sxs-lookup"><span data-stu-id="ee81c-146">Attribute Routing now provides an extensibility point called IDirectRouteProvider, which allows full control over how attribute routes are discovered and configured.</span></span> <span data-ttu-id="ee81c-147">Un IDirectRouteProvider est chargé de fournir une liste des actions et les contrôleurs, ainsi que les informations d’itinéraire associés pour spécifier exactement quelle configuration de routage est souhaitée pour ces actions.</span><span class="sxs-lookup"><span data-stu-id="ee81c-147">An IDirectRouteProvider is responsible for providing a list of actions and controllers along with associated route information to specify exactly what routing configuration is desired for those actions.</span></span> <span data-ttu-id="ee81c-148">Une implémentation d’un IDirectRouteProvider peut être spécifiée lors de l’appel MapAttributes/MapHttpAttributeRoutes.</span><span class="sxs-lookup"><span data-stu-id="ee81c-148">An IDirectRouteProvider implementation may be specified when calling MapAttributes/MapHttpAttributeRoutes.</span></span>

<span data-ttu-id="ee81c-149">Personnalisation de IDirectRouteProvider sera plus simple, en étendant notre implémentation par défaut, DefaultDirectRouteProvider.</span><span class="sxs-lookup"><span data-stu-id="ee81c-149">Customizing IDirectRouteProvider will be easiest by extending our default implementation, DefaultDirectRouteProvider.</span></span> <span data-ttu-id="ee81c-150">Cette classe fournit des méthodes virtuelles substituables distincts pour modifier la logique de détection des attributs, création d’entrées d’itinéraire et découvrir le préfixe d’itinéraire et au préfixe de zone.</span><span class="sxs-lookup"><span data-stu-id="ee81c-150">This class provides separate overridable virtual methods to change the logic for discovering attributes, creating route entries, and discovering route prefix and area prefix.</span></span>

<span data-ttu-id="ee81c-151">Voici quelques exemples qui illustrent ce que vous pouvez faire avec ce nouveau point d’extensibilité :</span><span class="sxs-lookup"><span data-stu-id="ee81c-151">Following are some examples on what you could do with this new extensibility point:</span></span>

1. <span data-ttu-id="ee81c-152">Prend en charge l’héritage des attributs d’itinéraire</span><span class="sxs-lookup"><span data-stu-id="ee81c-152">Support inheritance of Route attributes</span></span>

    <span data-ttu-id="ee81c-153">Exemple :</span><span class="sxs-lookup"><span data-stu-id="ee81c-153">Example:</span></span>

    <span data-ttu-id="ee81c-154">Ici une requête like « / api/valeurs/10 » retournerait correctement « Réussite : 10 »</span><span class="sxs-lookup"><span data-stu-id="ee81c-154">Here a request like "/api/values/10" would successfully return "Success:10"</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. <span data-ttu-id="ee81c-155">Fournir un nom d’itinéraire par défaut pour votre itinéraires d’attribut en suivant certaines convention que vous le souhaitez.</span><span class="sxs-lookup"><span data-stu-id="ee81c-155">Provide a default route name for your attribute routes by following some convention you like.</span></span> <span data-ttu-id="ee81c-156">Par défaut, routage d’attributs ne crée pas automatiquement les noms d’itinéraires d’attribut.</span><span class="sxs-lookup"><span data-stu-id="ee81c-156">By default, attribute routing doesn't automatically create names for attribute routes.</span></span>
3. <span data-ttu-id="ee81c-157">Modifier le modèle d’itinéraire itinéraires d’attribut à un emplacement centralisé avant qu’ils se retrouver dans la table d’itinéraires.</span><span class="sxs-lookup"><span data-stu-id="ee81c-157">Modify attribute routes' route template at one central place before they end up in the route table.</span></span>

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a><span data-ttu-id="ee81c-158">Prise en charge de Client de l’API Web pour Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="ee81c-158">Web API Client Support for Windows Phone 8.1</span></span>

<span data-ttu-id="ee81c-159">Vous pouvez maintenant utiliser le package NuGet Client de l’API Web pour implémenter votre logique de client d’API Web lorsque vous ciblez Windows Phone 8.1 ou à partir d’une application universelle.</span><span class="sxs-lookup"><span data-stu-id="ee81c-159">You can now use the Web API Client NuGet package to implement your Web API client logic when targeting Windows Phone 8.1 or from within a Universal App.</span></span>

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="ee81c-160">Problèmes connus et les modifications avec rupture</span><span class="sxs-lookup"><span data-stu-id="ee81c-160">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="ee81c-161">Cette section décrit les problèmes connus et les modifications avec rupture dans ASP.NET Web API 2.2.</span><span class="sxs-lookup"><span data-stu-id="ee81c-161">This section describes known issues and breaking changes in the ASP.NET Web API 2.2.</span></span>

### <a name="odata-v4"></a><span data-ttu-id="ee81c-162">OData v4</span><span class="sxs-lookup"><span data-stu-id="ee81c-162">OData v4</span></span>

#### <a name="model-builder"></a><span data-ttu-id="ee81c-163">Générateur de modèles</span><span class="sxs-lookup"><span data-stu-id="ee81c-163">Model builder</span></span>

<span data-ttu-id="ee81c-164">Problème : Les fonctions surchargées ne pouvaient pas être exposées en tant que FunctionImport</span><span class="sxs-lookup"><span data-stu-id="ee81c-164">Issue: Overloaded Functions could not be exposed as FunctionImport</span></span>

<span data-ttu-id="ee81c-165">S’il existe 2 des fonctions surchargées et sont également FunctionImport comme indiqué ci-dessous puis demande ~/GetAllConventionCustomers(CustomerName={customerName}) entraîne System.InvalidOperationException.</span><span class="sxs-lookup"><span data-stu-id="ee81c-165">If there are 2 overloaded functions and they are also FunctionImport as shown below then requesting ~/GetAllConventionCustomers(CustomerName={customerName}) results in System.InvalidOperationException.</span></span>

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

<span data-ttu-id="ee81c-166">Solution de contournement : La solution de contournement pour résoudre ce problème consiste à ajouter à la fois les surcharges de fonction en tant que FunctionImports.</span><span class="sxs-lookup"><span data-stu-id="ee81c-166">Workaround: The workaround for this issue is to add both the function overloads as FunctionImports.</span></span>

#### <a name="odata-routing"></a><span data-ttu-id="ee81c-167">Routage d’OData</span><span class="sxs-lookup"><span data-stu-id="ee81c-167">OData Routing</span></span>

<span data-ttu-id="ee81c-168">Littéraux de chaîne qui incluant l’URL encodée barre oblique (% 2F), et backslash(%5C) provoquent une erreur 404 lorsqu’ils sont utilisés dans les chemins d’accès de la ressource OData.</span><span class="sxs-lookup"><span data-stu-id="ee81c-168">String literals that include the URL encoded slash (%2F), and backslash(%5C) cause a 404 error when they are used in the OData resource paths.</span></span>

<span data-ttu-id="ee81c-169">Par exemple, les littéraux de chaîne peuvent être utilisés dans les chemins d’accès de la ressource OData en tant que paramètres de fonctions ou les valeurs de clé des jeux d’entités.</span><span class="sxs-lookup"><span data-stu-id="ee81c-169">For example, string literals can be used in the OData resource paths as parameters of functions or key values of entity sets.</span></span>

<span data-ttu-id="ee81c-170">/Employees/total.GetCount(Name='Name%2F')</span><span class="sxs-lookup"><span data-stu-id="ee81c-170">/Employees/Total.GetCount(Name='Name%2F')</span></span>

<span data-ttu-id="ee81c-171">/Employees('Name%5C')</span><span class="sxs-lookup"><span data-stu-id="ee81c-171">/Employees('Name%5C')</span></span>

<span data-ttu-id="ee81c-172">Lorsque les services reçoivent ces demandes hôtes va annuler-d’échappement les séquences d’échappement avant leur transmission à l’exécution de l’API Web.</span><span class="sxs-lookup"><span data-stu-id="ee81c-172">When services receive such requests the hosts will un-escape those escape sequences before passing them to the Web API runtime.</span></span> <span data-ttu-id="ee81c-173">Cela protège contre les attaques comme suit :</span><span class="sxs-lookup"><span data-stu-id="ee81c-173">This protects against attacks like the following:</span></span>  
  
`http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:`

<span data-ttu-id="ee81c-174">Cela provoque la pile Web API OData retourner une erreur 404 (introuvable).</span><span class="sxs-lookup"><span data-stu-id="ee81c-174">This causes the Web API OData stack to return a 404 error (Not Found).</span></span> <span data-ttu-id="ee81c-175">Pour éviter cette erreur, votre client doit utiliser les séquences de double échappement pour une barre oblique (% 252F) et une barre oblique inverse (% 255C).</span><span class="sxs-lookup"><span data-stu-id="ee81c-175">To prevent this error, your client should use the double escape sequences for slash (%252F) and backslash (%255C).</span></span> <span data-ttu-id="ee81c-176">Cela ne se produit pas pour les chaînes de requête tels que /Employees ? $filter = Name eq 'Nom % 2F'</span><span class="sxs-lookup"><span data-stu-id="ee81c-176">This does not happen for query strings such as /Employees?$filter=Name eq 'Name%2F'</span></span>

<span data-ttu-id="ee81c-177">**Notez les barres obliques sans séquence d’échappement (« / ») et les barres obliques inverses (") ne sont pas autorisés dans les littéraux de chaîne de chemin d’accès de ressource OData. Les barres obliques doivent apparaître uniquement comme séparateurs de chemin d’accès et les barres obliques inverses ne doivent pas apparaître dans le chemin d’accès de la ressource OData du tout. (Les deux sont utilisables dans certaines parties d’une chaîne de requête OData).**</span><span class="sxs-lookup"><span data-stu-id="ee81c-177">**Note un-escaped slashes ('/') and backslashes ('') are not legal in OData resource path string literals. Slashes should appear only as path separators and backslashes should not appear in the OData resource path at all. (Both are usable in some portions of an OData query string.)**</span></span>

<span data-ttu-id="ee81c-178">Solution de contournement : Vous pouvez substituer la méthode Parse de DefaultODataPathHandler d’échappement de la barre oblique et une barre oblique inverse dans les littéraux de chaîne avant analyse réellement.</span><span class="sxs-lookup"><span data-stu-id="ee81c-178">Workaround: You could override the Parse method of DefaultODataPathHandler to escape the slash and backslash in string literals before actually parsing them.</span></span> <span data-ttu-id="ee81c-179">Vous trouverez un exemple de cette approche ici.</span><span class="sxs-lookup"><span data-stu-id="ee81c-179">You can find a sample of this approach here.</span></span>

### <a name="odata-v3"></a><span data-ttu-id="ee81c-180">OData v3</span><span class="sxs-lookup"><span data-stu-id="ee81c-180">OData v3</span></span>

#### <a name="queryable"></a><span data-ttu-id="ee81c-181">[Requêtable]</span><span class="sxs-lookup"><span data-stu-id="ee81c-181">[Queryable]</span></span>

<span data-ttu-id="ee81c-182">L’attribut [Queryable] est déconseillée.</span><span class="sxs-lookup"><span data-stu-id="ee81c-182">The [Queryable] attribute is deprecated.</span></span> <span data-ttu-id="ee81c-183">Nouvelles OData v3 applications doivent utiliser **System.Web.Http.OData.EnableQueryAttribute**.</span><span class="sxs-lookup"><span data-stu-id="ee81c-183">New OData v3 applications should use **System.Web.Http.OData.EnableQueryAttribute**.</span></span>

<span data-ttu-id="ee81c-184">Le **ODataHttpConfigurationExtensions.EnableQuerySupport** méthode d’extension ajoute à présent un **EnableQueryAttribute** à la collection de filtres globaux.</span><span class="sxs-lookup"><span data-stu-id="ee81c-184">The **ODataHttpConfigurationExtensions.EnableQuerySupport** extension method now adds an **EnableQueryAttribute** to the global filter collection.</span></span> <span data-ttu-id="ee81c-185">Si vous disposez de tous les contrôleurs de le **[Queryable]** d’attribut, l’appel `config.EnableQuerySupport()` entraîne la **[Queryable]** attribut Échec</span><span class="sxs-lookup"><span data-stu-id="ee81c-185">If any controllers have the **[Queryable]** attribute, calling `config.EnableQuerySupport()` will cause the **[Queryable]** attribute to fail</span></span>

<span data-ttu-id="ee81c-186">La méthode recommandée pour résoudre ce problème consiste à remplacer toutes les instances de **QueryableAttribute** avec **System.Web.Http.OData.EnableQueryAttribute**.</span><span class="sxs-lookup"><span data-stu-id="ee81c-186">The recommended way to resolve this issue is to replace all instances of **QueryableAttribute** with **System.Web.Http.OData.EnableQueryAttribute**.</span></span>

<span data-ttu-id="ee81c-187">Une autre solution consiste à utiliser le code suivant dans votre configuration de l’API Web :</span><span class="sxs-lookup"><span data-stu-id="ee81c-187">An alternative workaround is to use the following code in your Web API configuration:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a><span data-ttu-id="ee81c-188">Routage d’attributs</span><span class="sxs-lookup"><span data-stu-id="ee81c-188">Attribute Routing</span></span>

<span data-ttu-id="ee81c-189">Problème : Liaison de modèle de type complexe qui est décorée avec l’attribut de FromUri se comporte différemment lorsque vous utilisez le routage d’attributs.</span><span class="sxs-lookup"><span data-stu-id="ee81c-189">Issue: Model binding of complex type which is decorated with FromUri attribute behaves differently when using Attribute Routing.</span></span>

<span data-ttu-id="ee81c-190">Lien suivant consiste à suivre le problème et aussi des informations sur une solution de contournement.</span><span class="sxs-lookup"><span data-stu-id="ee81c-190">Following link is tracking the issue and also has details about a workaround.</span></span>  
[http://aspnetwebstack.codeplex.com/workitem/1944](http://aspnetwebstack.codeplex.com/workitem/1944)

<span data-ttu-id="ee81c-191">Problème : Génération de modèles automatique/Web MVC API dans un projet avec 5.2.0 résultats packages dans 5.1.2 packages pour ceux qui n’existent pas déjà dans le projet</span><span class="sxs-lookup"><span data-stu-id="ee81c-191">Issue: Scaffolding MVC/Web API into a project with 5.2.0 packages results in 5.1.2 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="ee81c-192">Mise à jour des packages NuGet pour ASP.NET MVC 5.2 ne met pas à jour les outils de Visual Studio, tels que de la génération de modèles automatique ASP.NET ou le modèle de projet d’Application Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ee81c-192">Updating NuGet packages for ASP.NET MVC 5.2 does not update the Visual Studio tools such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="ee81c-193">Ils utilisent la version précédente des packages de runtime ASP.NET (par exemple, 5.1.2 dans Update 2).</span><span class="sxs-lookup"><span data-stu-id="ee81c-193">They use the previous version of the ASP.NET runtime packages (e.g. 5.1.2 in Update 2).</span></span> <span data-ttu-id="ee81c-194">Par conséquent, la génération de modèles automatique ASP.NET installera la version précédente (par exemple, 5.1.2 dans Update 2) des packages requis, si elles ne sont pas déjà disponibles dans vos projets.</span><span class="sxs-lookup"><span data-stu-id="ee81c-194">As a result, the ASP.NET scaffolding will install the previous version (e.g. 5.1.2 in Update 2) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="ee81c-195">Toutefois, la génération de modèles automatique ASP.NET dans Visual Studio 2013 RTM ou une mise à jour 1 ne remplace pas les packages plus récentes dans vos projets.</span><span class="sxs-lookup"><span data-stu-id="ee81c-195">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span> <span data-ttu-id="ee81c-196">Si vous utilisez la génération de modèles automatique ASP.NET après la mise à jour les packages de vos projets Web API 2.2 ou ASP.NET MVC 5.2, assurez-vous que les versions des API Web ASP.NET MVC sont cohérentes.</span><span class="sxs-lookup"><span data-stu-id="ee81c-196">If you use ASP.NET scaffolding after updating the packages of your projects to Web API 2.2 or ASP.NET MVC 5.2, make sure the versions of Web API and ASP.NET MVC are consistent.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a><span data-ttu-id="ee81c-197">Correctifs de bogues et mises à jour mineures de fonctionnalité</span><span class="sxs-lookup"><span data-stu-id="ee81c-197">Bug Fixes and Minor Feature Updates</span></span>

<span data-ttu-id="ee81c-198">Cette version inclut également plusieurs correctifs de bogues et les fonctionnalités mineures mises à jour.</span><span class="sxs-lookup"><span data-stu-id="ee81c-198">This release also includes several bug fixes and minor feature updates.</span></span> <span data-ttu-id="ee81c-199">Vous trouverez la liste complète ici :</span><span class="sxs-lookup"><span data-stu-id="ee81c-199">You can find the complete list here:</span></span>

- [<span data-ttu-id="ee81c-200">package 5.2</span><span class="sxs-lookup"><span data-stu-id="ee81c-200">5.2 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a><span data-ttu-id="ee81c-201">Microsoft.AspNet.OData 5.2.1</span><span class="sxs-lookup"><span data-stu-id="ee81c-201">Microsoft.AspNet.OData 5.2.1</span></span>

<span data-ttu-id="ee81c-202">Le package Microsoft.AspNet.OData 5.2.1 contient des mises à jour des dépendances de NuGet, mais aucune des correctifs de bogues.</span><span class="sxs-lookup"><span data-stu-id="ee81c-202">The Microsoft.AspNet.OData 5.2.1 package contains NuGet dependency updates but no bug fixes.</span></span> <span data-ttu-id="ee81c-203">Avec cette mise à jour, il n’est plus une dépendance stricte Microsoft.OData.Core 6.4.0, mais un peut mettre à niveau vers toute version entre 6.4.0 et 7.0.0.</span><span class="sxs-lookup"><span data-stu-id="ee81c-203">With this update, there is no longer a strict dependency on Microsoft.OData.Core 6.4.0, but one can upgrade to any version between 6.4.0 and 7.0.0.</span></span>

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a><span data-ttu-id="ee81c-204">Microsoft.AspNet.WebAPI 5.2.2</span><span class="sxs-lookup"><span data-stu-id="ee81c-204">Microsoft.AspNet.WebAPI 5.2.2</span></span>

<span data-ttu-id="ee81c-205">Dans cette version nous avons modifié une dépendance pour `Json.Net 6.0.4`.</span><span class="sxs-lookup"><span data-stu-id="ee81c-205">In this release we have made a dependency change for `Json.Net 6.0.4`.</span></span> <span data-ttu-id="ee81c-206">Pour plus d’informations sur les nouveautés de cette version de `Json.NET`, consultez [Json.NET 6.0 version 4 - JSON de fusion, l’Injection de dépendances](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="ee81c-206">For more information on what is new in this release of `Json.NET`, see [Json.NET 6.0 Release 4 - JSON Merge, Dependency Injection](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection).</span></span> <span data-ttu-id="ee81c-207">Cette version ne dispose de toutes les autres nouvelles fonctionnalités ou des correctifs de bogues dans l’API Web.</span><span class="sxs-lookup"><span data-stu-id="ee81c-207">This release doesn't have any other new features or bug fixes in Web API.</span></span> <span data-ttu-id="ee81c-208">Nous avons ensuite mis à jour tous les autres packages dépendants que nous avons pour dépendent de cette nouvelle version de l’API Web.</span><span class="sxs-lookup"><span data-stu-id="ee81c-208">We have subsequently updated all other dependent packages we own to depend on this new version of Web API.</span></span>

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a><span data-ttu-id="ee81c-209">Microsoft.AspNet.WebAPI 5.2.3 bêta</span><span class="sxs-lookup"><span data-stu-id="ee81c-209">Microsoft.AspNet.WebAPI 5.2.3 Beta</span></span>

<span data-ttu-id="ee81c-210">Vous pouvez lire sur la version [ici](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx).</span><span class="sxs-lookup"><span data-stu-id="ee81c-210">You can read about the release [here](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx).</span></span> <span data-ttu-id="ee81c-211">Cette version contient uniquement les correctifs de bogues.</span><span class="sxs-lookup"><span data-stu-id="ee81c-211">This release contains only bug fixes.</span></span> <span data-ttu-id="ee81c-212">Vous pouvez utiliser [cette requête](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) pour afficher la liste des problèmes résolus dans cette version.</span><span class="sxs-lookup"><span data-stu-id="ee81c-212">You can use [this query](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) to see the list of issues fixed in this release.</span></span>
