---
title: "Mettre en cache d’assistance de balise dans le cœur d’ASP.NET MVC"
author: pkellner
description: "Montre comment travailler avec l’application d’assistance de balise de Cache"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: dfd9c3c0c4e50a99e4f8703b01bd9b384930b87a
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2018
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="13b55-103">Mettre en cache d’assistance de balise dans le cœur d’ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="13b55-103">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="13b55-104">Par [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="13b55-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="13b55-105">L’application d’assistance de balise de Cache permet d’améliorer considérablement les performances de votre application ASP.NET Core par son contenu pour le fournisseur de cache ASP.NET Core interne de la mise en cache.</span><span class="sxs-lookup"><span data-stu-id="13b55-105">The Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="13b55-106">Le moteur d’affichage Razor définit la valeur par défaut `expires-after` vingt minutes.</span><span class="sxs-lookup"><span data-stu-id="13b55-106">The Razor View Engine sets the default `expires-after` to twenty minutes.</span></span>

<span data-ttu-id="13b55-107">Le balisage de Razor suivant met en cache la date/heure :</span><span class="sxs-lookup"><span data-stu-id="13b55-107">The following Razor markup caches the date/time:</span></span>

```cshtml
<cache>@DateTime.Now</cache>
```

<span data-ttu-id="13b55-108">La première demande vers la page qui contient `CacheTagHelper` affiche la date/heure actuelle.</span><span class="sxs-lookup"><span data-stu-id="13b55-108">The first request to the page that contains `CacheTagHelper` will display the current date/time.</span></span> <span data-ttu-id="13b55-109">Demandes supplémentaires affichera la valeur mise en cache jusqu'à ce que le cache expire (par défaut, 20 minutes) ou est supprimé par sollicitation de la mémoire.</span><span class="sxs-lookup"><span data-stu-id="13b55-109">Additional requests will show the cached value until the cache expires (default 20 minutes) or is evicted by memory pressure.</span></span>

<span data-ttu-id="13b55-110">Vous pouvez définir la durée du cache avec les attributs suivants :</span><span class="sxs-lookup"><span data-stu-id="13b55-110">You can set the cache duration with the following attributes:</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="13b55-111">Mettre en cache les attributs d’assistance de balise</span><span class="sxs-lookup"><span data-stu-id="13b55-111">Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled"></a><span data-ttu-id="13b55-112">enabled</span><span class="sxs-lookup"><span data-stu-id="13b55-112">enabled</span></span>    


| <span data-ttu-id="13b55-113">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="13b55-113">Attribute Type</span></span>    | <span data-ttu-id="13b55-114">Valeurs valides</span><span class="sxs-lookup"><span data-stu-id="13b55-114">Valid Values</span></span>      |
|----------------   |----------------   |
| <span data-ttu-id="13b55-115">boolean</span><span class="sxs-lookup"><span data-stu-id="13b55-115">boolean</span></span>           | <span data-ttu-id="13b55-116">« true » (par défaut)</span><span class="sxs-lookup"><span data-stu-id="13b55-116">"true" (default)</span></span>  |
|                   | <span data-ttu-id="13b55-117">"false"</span><span class="sxs-lookup"><span data-stu-id="13b55-117">"false"</span></span>   |


<span data-ttu-id="13b55-118">Détermine si le contenu du programme d’assistance de balise de Cache est mis en cache.</span><span class="sxs-lookup"><span data-stu-id="13b55-118">Determines whether the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="13b55-119">La valeur par défaut est `true`.</span><span class="sxs-lookup"><span data-stu-id="13b55-119">The default is `true`.</span></span>  <span data-ttu-id="13b55-120">Si la valeur `false` ce programme d’assistance de balise de Cache n’a aucun effet mise en cache sur la sortie rendue.</span><span class="sxs-lookup"><span data-stu-id="13b55-120">If set to `false` this Cache Tag Helper will have no caching effect on the rendered output.</span></span>

<span data-ttu-id="13b55-121">Exemple :</span><span class="sxs-lookup"><span data-stu-id="13b55-121">Example:</span></span>

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a><span data-ttu-id="13b55-122">expires-on</span><span class="sxs-lookup"><span data-stu-id="13b55-122">expires-on</span></span> 

| <span data-ttu-id="13b55-123">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="13b55-123">Attribute Type</span></span>    | <span data-ttu-id="13b55-124">Exemple de valeur</span><span class="sxs-lookup"><span data-stu-id="13b55-124">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="13b55-125">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="13b55-125">DateTimeOffset</span></span>    | <span data-ttu-id="13b55-126">«@new DateTime(2025,1,29,17,02,0) »</span><span class="sxs-lookup"><span data-stu-id="13b55-126">"@new DateTime(2025,1,29,17,02,0)"</span></span>    |


<span data-ttu-id="13b55-127">Définit une date d’expiration absolue.</span><span class="sxs-lookup"><span data-stu-id="13b55-127">Sets an absolute expiration date.</span></span> <span data-ttu-id="13b55-128">L’exemple suivant met en cache le contenu de l’application d’assistance de balise de Cache jusqu'à 5 h 02 le 29 janvier 2025.</span><span class="sxs-lookup"><span data-stu-id="13b55-128">The following example will cache the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025.</span></span>

<span data-ttu-id="13b55-129">Exemple :</span><span class="sxs-lookup"><span data-stu-id="13b55-129">Example:</span></span>

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a><span data-ttu-id="13b55-130">expire après</span><span class="sxs-lookup"><span data-stu-id="13b55-130">expires-after</span></span>

| <span data-ttu-id="13b55-131">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="13b55-131">Attribute Type</span></span>    | <span data-ttu-id="13b55-132">Exemple de valeur</span><span class="sxs-lookup"><span data-stu-id="13b55-132">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="13b55-133">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="13b55-133">TimeSpan</span></span>    | <span data-ttu-id="13b55-134">"@TimeSpan.FromSeconds(120)"</span><span class="sxs-lookup"><span data-stu-id="13b55-134">"@TimeSpan.FromSeconds(120)"</span></span>    |


<span data-ttu-id="13b55-135">Définit la durée à partir de la première demande pour mettre en cache le contenu.</span><span class="sxs-lookup"><span data-stu-id="13b55-135">Sets the length of time from the first request time to cache the contents.</span></span> 

<span data-ttu-id="13b55-136">Exemple :</span><span class="sxs-lookup"><span data-stu-id="13b55-136">Example:</span></span>

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a><span data-ttu-id="13b55-137">expiration de retrait</span><span class="sxs-lookup"><span data-stu-id="13b55-137">expires-sliding</span></span>

| <span data-ttu-id="13b55-138">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="13b55-138">Attribute Type</span></span>    | <span data-ttu-id="13b55-139">Exemple de valeur</span><span class="sxs-lookup"><span data-stu-id="13b55-139">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="13b55-140">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="13b55-140">TimeSpan</span></span>    | <span data-ttu-id="13b55-141">"@TimeSpan.FromSeconds(60)"</span><span class="sxs-lookup"><span data-stu-id="13b55-141">"@TimeSpan.FromSeconds(60)"</span></span>     |


<span data-ttu-id="13b55-142">Définit l’heure à laquelle une entrée de cache doit être supprimée si elle n’a pas été accédé.</span><span class="sxs-lookup"><span data-stu-id="13b55-142">Sets the time that a cache entry should be evicted if it has not been accessed.</span></span>

<span data-ttu-id="13b55-143">Exemple :</span><span class="sxs-lookup"><span data-stu-id="13b55-143">Example:</span></span>

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a><span data-ttu-id="13b55-144">varient en fonction de l’en-tête</span><span class="sxs-lookup"><span data-stu-id="13b55-144">vary-by-header</span></span>

| <span data-ttu-id="13b55-145">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="13b55-145">Attribute Type</span></span>    | <span data-ttu-id="13b55-146">Exemples de valeurs</span><span class="sxs-lookup"><span data-stu-id="13b55-146">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="13b55-147">Chaîne</span><span class="sxs-lookup"><span data-stu-id="13b55-147">String</span></span>            | <span data-ttu-id="13b55-148">« User-Agent »</span><span class="sxs-lookup"><span data-stu-id="13b55-148">"User-Agent"</span></span>                  |
|                   | <span data-ttu-id="13b55-149">« User-Agent, content-encoding »</span><span class="sxs-lookup"><span data-stu-id="13b55-149">"User-Agent,content-encoding"</span></span> |

<span data-ttu-id="13b55-150">Accepte une valeur d’en-tête unique ou une liste séparée par des virgules de valeurs d’en-tête qui déclenchent une actualisation du cache quand ils changent.</span><span class="sxs-lookup"><span data-stu-id="13b55-150">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when they change.</span></span> <span data-ttu-id="13b55-151">L’exemple suivant analyse la valeur d’en-tête `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="13b55-151">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="13b55-152">L’exemple mettra en cache le contenu de chaque autre `User-Agent` présenté au serveur web.</span><span class="sxs-lookup"><span data-stu-id="13b55-152">The example will cache the content for every different `User-Agent` presented to the web server.</span></span>

<span data-ttu-id="13b55-153">Exemple :</span><span class="sxs-lookup"><span data-stu-id="13b55-153">Example:</span></span>

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a><span data-ttu-id="13b55-154">varient par requête</span><span class="sxs-lookup"><span data-stu-id="13b55-154">vary-by-query</span></span>

| <span data-ttu-id="13b55-155">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="13b55-155">Attribute Type</span></span>    | <span data-ttu-id="13b55-156">Exemples de valeurs</span><span class="sxs-lookup"><span data-stu-id="13b55-156">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="13b55-157">Chaîne</span><span class="sxs-lookup"><span data-stu-id="13b55-157">String</span></span>            | <span data-ttu-id="13b55-158">« Vérifiez »</span><span class="sxs-lookup"><span data-stu-id="13b55-158">"Make"</span></span>                |
|                   | <span data-ttu-id="13b55-159">« Assurez-vous, modèle »</span><span class="sxs-lookup"><span data-stu-id="13b55-159">"Make,Model"</span></span> |

<span data-ttu-id="13b55-160">Accepte une valeur d’en-tête unique ou une liste séparée par des virgules de valeurs d’en-tête qui déclenchent une actualisation du cache lorsque la valeur d’en-tête.</span><span class="sxs-lookup"><span data-stu-id="13b55-160">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header value changes.</span></span> <span data-ttu-id="13b55-161">L’exemple suivant présente les valeurs de `Make` et `Model`.</span><span class="sxs-lookup"><span data-stu-id="13b55-161">The following example looks at the values of `Make` and `Model`.</span></span>

<span data-ttu-id="13b55-162">Exemple :</span><span class="sxs-lookup"><span data-stu-id="13b55-162">Example:</span></span>

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a><span data-ttu-id="13b55-163">varient par itinéraire</span><span class="sxs-lookup"><span data-stu-id="13b55-163">vary-by-route</span></span>

| <span data-ttu-id="13b55-164">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="13b55-164">Attribute Type</span></span>    | <span data-ttu-id="13b55-165">Exemples de valeurs</span><span class="sxs-lookup"><span data-stu-id="13b55-165">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="13b55-166">Chaîne</span><span class="sxs-lookup"><span data-stu-id="13b55-166">String</span></span>            | <span data-ttu-id="13b55-167">« Vérifiez »</span><span class="sxs-lookup"><span data-stu-id="13b55-167">"Make"</span></span>                |
|                   | <span data-ttu-id="13b55-168">« Assurez-vous, modèle »</span><span class="sxs-lookup"><span data-stu-id="13b55-168">"Make,Model"</span></span> |

<span data-ttu-id="13b55-169">Accepte une valeur d’en-tête unique ou une liste séparée par des virgules de valeurs d’en-tête qui déclenchent une actualisation du cache lorsque le paramètre de données d’itinéraire spécifiées à modifier.</span><span class="sxs-lookup"><span data-stu-id="13b55-169">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the route data parameter value(s) change.</span></span> <span data-ttu-id="13b55-170">Exemple :</span><span class="sxs-lookup"><span data-stu-id="13b55-170">Example:</span></span>

<span data-ttu-id="13b55-171">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="13b55-171">*Startup.cs*</span></span> 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```
  
<span data-ttu-id="13b55-172">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="13b55-172">*Index.cshtml*</span></span>

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-cookie"></a><span data-ttu-id="13b55-173">varient par cookie</span><span class="sxs-lookup"><span data-stu-id="13b55-173">vary-by-cookie</span></span>

| <span data-ttu-id="13b55-174">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="13b55-174">Attribute Type</span></span>    | <span data-ttu-id="13b55-175">Exemples de valeurs</span><span class="sxs-lookup"><span data-stu-id="13b55-175">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="13b55-176">Chaîne</span><span class="sxs-lookup"><span data-stu-id="13b55-176">String</span></span>            | <span data-ttu-id="13b55-177">".AspNetCore.Identity.Application"</span><span class="sxs-lookup"><span data-stu-id="13b55-177">".AspNetCore.Identity.Application"</span></span>                |
|                   | <span data-ttu-id="13b55-178">".AspNetCore.Identity.Application,HairColor"</span><span class="sxs-lookup"><span data-stu-id="13b55-178">".AspNetCore.Identity.Application,HairColor"</span></span> |

<span data-ttu-id="13b55-179">Accepte une valeur d’en-tête unique ou une liste séparée par des virgules de valeurs d’en-tête qui déclenchent une actualisation du cache lorsque les valeurs d’en-tête changement de (s).</span><span class="sxs-lookup"><span data-stu-id="13b55-179">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header values(s) change.</span></span> <span data-ttu-id="13b55-180">L’exemple suivant présente le cookie associé d’identité ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="13b55-180">The following example looks at the cookie associated with ASP.NET Identity.</span></span> <span data-ttu-id="13b55-181">Lorsqu’un utilisateur est authentifié le cookie de la demande à définir qui déclenche une actualisation du cache.</span><span class="sxs-lookup"><span data-stu-id="13b55-181">When a user is authenticated the request cookie to be set which triggers a cache refresh.</span></span>

<span data-ttu-id="13b55-182">Exemple :</span><span class="sxs-lookup"><span data-stu-id="13b55-182">Example:</span></span>

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a><span data-ttu-id="13b55-183">varient par utilisateur</span><span class="sxs-lookup"><span data-stu-id="13b55-183">vary-by-user</span></span>

| <span data-ttu-id="13b55-184">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="13b55-184">Attribute Type</span></span>    | <span data-ttu-id="13b55-185">Exemples de valeurs</span><span class="sxs-lookup"><span data-stu-id="13b55-185">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="13b55-186">Booléen</span><span class="sxs-lookup"><span data-stu-id="13b55-186">Boolean</span></span>             | <span data-ttu-id="13b55-187">"true"</span><span class="sxs-lookup"><span data-stu-id="13b55-187">"true"</span></span>                  |
|                     | <span data-ttu-id="13b55-188">« false » (valeur par défaut)</span><span class="sxs-lookup"><span data-stu-id="13b55-188">"false" (default)</span></span> |

<span data-ttu-id="13b55-189">Spécifie si le cache doit réinitialiser lorsque l’utilisateur connecté (ou Principal du contexte) change.</span><span class="sxs-lookup"><span data-stu-id="13b55-189">Specifies whether or not the cache should reset when the logged-in user (or Context Principal) changes.</span></span> <span data-ttu-id="13b55-190">L’utilisateur actuel est également connue sous le Principal demande du contexte et peuvent être affiché dans une vue Razor en référençant `@User.Identity.Name`.</span><span class="sxs-lookup"><span data-stu-id="13b55-190">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="13b55-191">L’exemple suivant ressemble au niveau de l’utilisateur actuellement connecté.</span><span class="sxs-lookup"><span data-stu-id="13b55-191">The following example looks at the current logged in user.</span></span>  

<span data-ttu-id="13b55-192">Exemple :</span><span class="sxs-lookup"><span data-stu-id="13b55-192">Example:</span></span>

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="13b55-193">À l’aide de cet attribut gère le contenu dans le cache via un cycle de connexion et déconnexion.</span><span class="sxs-lookup"><span data-stu-id="13b55-193">Using this attribute maintains the contents in cache through a log-in and log-out cycle.</span></span>  <span data-ttu-id="13b55-194">Lorsque vous utilisez `vary-by-user="true"`, une action de connexion et déconnexion invalide le cache pour l’utilisateur authentifié.</span><span class="sxs-lookup"><span data-stu-id="13b55-194">When using `vary-by-user="true"`, a log-in and log-out action invalidates the cache for the authenticated user.</span></span>  <span data-ttu-id="13b55-195">Le cache est invalidé, car une nouvelle valeur de cookie unique est générée sur la connexion.</span><span class="sxs-lookup"><span data-stu-id="13b55-195">The cache is invalidated because a new unique cookie value is generated on login.</span></span> <span data-ttu-id="13b55-196">Cache est conservé à l’état anonyme lorsque aucun cookie n’est présent ou a expiré.</span><span class="sxs-lookup"><span data-stu-id="13b55-196">Cache is maintained for the anonymous state when no cookie is present or has expired.</span></span> <span data-ttu-id="13b55-197">Cela signifie que si aucun utilisateur n’est connecté, le cache est conservé.</span><span class="sxs-lookup"><span data-stu-id="13b55-197">This means if no user is logged in, the cache will be maintained.</span></span>

- - -

### <a name="vary-by"></a><span data-ttu-id="13b55-198">variation de</span><span class="sxs-lookup"><span data-stu-id="13b55-198">vary-by</span></span>

| <span data-ttu-id="13b55-199">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="13b55-199">Attribute Type</span></span>    | <span data-ttu-id="13b55-200">Exemples de valeurs</span><span class="sxs-lookup"><span data-stu-id="13b55-200">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="13b55-201">Chaîne</span><span class="sxs-lookup"><span data-stu-id="13b55-201">String</span></span>             | <span data-ttu-id="13b55-202">"@Model"</span><span class="sxs-lookup"><span data-stu-id="13b55-202">"@Model"</span></span>                 |


<span data-ttu-id="13b55-203">Permet la personnalisation de données obtient mis en cache.</span><span class="sxs-lookup"><span data-stu-id="13b55-203">Allows for customization of what data gets cached.</span></span> <span data-ttu-id="13b55-204">Lorsque l’objet référencé par les modifications apportées à la valeur de la chaîne de l’attribut, le contenu de l’application d’assistance de balise de Cache est mis à jour.</span><span class="sxs-lookup"><span data-stu-id="13b55-204">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="13b55-205">Fréquence à laquelle une concaténation de chaînes de valeurs de modèle sont affectées à cet attribut.</span><span class="sxs-lookup"><span data-stu-id="13b55-205">Often a string-concatenation of model values are assigned to this attribute.</span></span>  <span data-ttu-id="13b55-206">En effet, cela signifie qu'une mise à jour à une des valeurs concaténées invalide le cache.</span><span class="sxs-lookup"><span data-stu-id="13b55-206">Effectively, that means an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="13b55-207">L’exemple suivant suppose la somme de la vue de rendu de la valeur entière des deux paramètres d’itinéraire, la méthode du contrôleur `myParam1` et `myParam2`et qui retourne en tant que la propriété de modèle unique.</span><span class="sxs-lookup"><span data-stu-id="13b55-207">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns that as the single model property.</span></span> <span data-ttu-id="13b55-208">Lorsque cette somme est modifié, le contenu de l’application d’assistance de balise de Cache est rendu et mis en cache à nouveau.</span><span class="sxs-lookup"><span data-stu-id="13b55-208">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="13b55-209">Exemple :</span><span class="sxs-lookup"><span data-stu-id="13b55-209">Example:</span></span>

<span data-ttu-id="13b55-210">Action :</span><span class="sxs-lookup"><span data-stu-id="13b55-210">Action:</span></span>

```csharp
public IActionResult Index(string myParam1,string myParam2,string myParam3)
{
    int num1;
    int num2;
    int.TryParse(myParam1, out num1);
    int.TryParse(myParam2, out num2);
    return View(viewName, num1 + num2);
}
```

<span data-ttu-id="13b55-211">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="13b55-211">*Index.cshtml*</span></span>

```cshtml
<cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="priority"></a><span data-ttu-id="13b55-212">priority</span><span class="sxs-lookup"><span data-stu-id="13b55-212">priority</span></span>

| <span data-ttu-id="13b55-213">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="13b55-213">Attribute Type</span></span>    | <span data-ttu-id="13b55-214">Exemples de valeurs</span><span class="sxs-lookup"><span data-stu-id="13b55-214">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="13b55-215">CacheItemPriority</span><span class="sxs-lookup"><span data-stu-id="13b55-215">CacheItemPriority</span></span>  | <span data-ttu-id="13b55-216">« High »</span><span class="sxs-lookup"><span data-stu-id="13b55-216">"High"</span></span>                   |
|                    | <span data-ttu-id="13b55-217">« Low »</span><span class="sxs-lookup"><span data-stu-id="13b55-217">"Low"</span></span> |
|                    | <span data-ttu-id="13b55-218">« NeverRemove »</span><span class="sxs-lookup"><span data-stu-id="13b55-218">"NeverRemove"</span></span> |
|                    | <span data-ttu-id="13b55-219">"Normal"</span><span class="sxs-lookup"><span data-stu-id="13b55-219">"Normal"</span></span> |

<span data-ttu-id="13b55-220">Fournit des instructions de suppression de cache au fournisseur de cache intégré.</span><span class="sxs-lookup"><span data-stu-id="13b55-220">Provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="13b55-221">Supprimez le serveur web `Low` mettre en cache les entrées tout d’abord une mémoire insuffisante.</span><span class="sxs-lookup"><span data-stu-id="13b55-221">The web server will evict `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="13b55-222">Exemple :</span><span class="sxs-lookup"><span data-stu-id="13b55-222">Example:</span></span>

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="13b55-223">Le `priority` attribut ne garantit pas un niveau spécifique de la rétention du cache.</span><span class="sxs-lookup"><span data-stu-id="13b55-223">The `priority` attribute does not guarantee a specific level of cache retention.</span></span> <span data-ttu-id="13b55-224">`CacheItemPriority`est uniquement une suggestion.</span><span class="sxs-lookup"><span data-stu-id="13b55-224">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="13b55-225">Cet attribut `NeverRemove` ne garantit pas que le cache est conservé.</span><span class="sxs-lookup"><span data-stu-id="13b55-225">Setting this attribute to `NeverRemove` does not guarantee that the cache will always be retained.</span></span> <span data-ttu-id="13b55-226">Consultez [des ressources supplémentaires](#additional-resources) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="13b55-226">See [Additional Resources](#additional-resources) for more information.</span></span>

<span data-ttu-id="13b55-227">L’application d’assistance de balise de Cache est dépendante de la [du service de cache mémoire](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="13b55-227">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="13b55-228">L’application d’assistance de balise de Cache ajoute le service s’il n’a pas été ajouté.</span><span class="sxs-lookup"><span data-stu-id="13b55-228">The Cache Tag Helper adds the service if it has not been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="13b55-229">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="13b55-229">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
