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
ms.openlocfilehash: 10aa1b493dbd0672cac789f6e48ddf2f14ba35dc
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="041b7-103">Mettre en cache d’assistance de balise dans le cœur d’ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="041b7-103">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="041b7-104">Par [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="041b7-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="041b7-105">L’application d’assistance de balise de Cache permet d’améliorer considérablement les performances de votre application ASP.NET Core par son contenu pour le fournisseur de cache ASP.NET Core interne de la mise en cache.</span><span class="sxs-lookup"><span data-stu-id="041b7-105">The Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="041b7-106">Le moteur d’affichage Razor définit la valeur par défaut `expires-after` vingt minutes.</span><span class="sxs-lookup"><span data-stu-id="041b7-106">The Razor View Engine sets the default `expires-after` to twenty minutes.</span></span>

<span data-ttu-id="041b7-107">Le balisage de Razor suivant met en cache la date/heure :</span><span class="sxs-lookup"><span data-stu-id="041b7-107">The following Razor markup caches the date/time:</span></span>

```cshtml
<cache>@DateTime.Now</cache>
```

<span data-ttu-id="041b7-108">La première demande vers la page qui contient `CacheTagHelper` affiche la date/heure actuelle.</span><span class="sxs-lookup"><span data-stu-id="041b7-108">The first request to the page that contains `CacheTagHelper` will display the current date/time.</span></span> <span data-ttu-id="041b7-109">Demandes supplémentaires affichera la valeur mise en cache jusqu'à ce que le cache expire (par défaut, 20 minutes) ou est supprimé par sollicitation de la mémoire.</span><span class="sxs-lookup"><span data-stu-id="041b7-109">Additional requests will show the cached value until the cache expires (default 20 minutes) or is evicted by memory pressure.</span></span>

<span data-ttu-id="041b7-110">Vous pouvez définir la durée du cache avec les attributs suivants :</span><span class="sxs-lookup"><span data-stu-id="041b7-110">You can set the cache duration with the following attributes:</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="041b7-111">Mettre en cache les attributs d’assistance de balise</span><span class="sxs-lookup"><span data-stu-id="041b7-111">Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled"></a><span data-ttu-id="041b7-112">enabled</span><span class="sxs-lookup"><span data-stu-id="041b7-112">enabled</span></span>    


| <span data-ttu-id="041b7-113">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="041b7-113">Attribute Type</span></span>    | <span data-ttu-id="041b7-114">Valeurs valides</span><span class="sxs-lookup"><span data-stu-id="041b7-114">Valid Values</span></span>      |
|----------------   |----------------   |
| <span data-ttu-id="041b7-115">boolean</span><span class="sxs-lookup"><span data-stu-id="041b7-115">boolean</span></span>           | <span data-ttu-id="041b7-116">« true » (par défaut)</span><span class="sxs-lookup"><span data-stu-id="041b7-116">"true" (default)</span></span>  |
|                   | <span data-ttu-id="041b7-117">"false"</span><span class="sxs-lookup"><span data-stu-id="041b7-117">"false"</span></span>   |


<span data-ttu-id="041b7-118">Détermine si le contenu du programme d’assistance de balise de Cache est mis en cache.</span><span class="sxs-lookup"><span data-stu-id="041b7-118">Determines whether the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="041b7-119">La valeur par défaut est `true`.</span><span class="sxs-lookup"><span data-stu-id="041b7-119">The default is `true`.</span></span>  <span data-ttu-id="041b7-120">Si la valeur `false` ce programme d’assistance de balise de Cache n’a aucun effet mise en cache sur la sortie rendue.</span><span class="sxs-lookup"><span data-stu-id="041b7-120">If set to `false` this Cache Tag Helper will have no caching effect on the rendered output.</span></span>

<span data-ttu-id="041b7-121">Exemple :</span><span class="sxs-lookup"><span data-stu-id="041b7-121">Example:</span></span>

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a><span data-ttu-id="041b7-122">expires-on</span><span class="sxs-lookup"><span data-stu-id="041b7-122">expires-on</span></span> 

| <span data-ttu-id="041b7-123">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="041b7-123">Attribute Type</span></span>    | <span data-ttu-id="041b7-124">Exemple de valeur</span><span class="sxs-lookup"><span data-stu-id="041b7-124">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="041b7-125">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="041b7-125">DateTimeOffset</span></span>    | <span data-ttu-id="041b7-126">«@new DateTime(2025,1,29,17,02,0) »</span><span class="sxs-lookup"><span data-stu-id="041b7-126">"@new DateTime(2025,1,29,17,02,0)"</span></span>    |


<span data-ttu-id="041b7-127">Définit une date d’expiration absolue.</span><span class="sxs-lookup"><span data-stu-id="041b7-127">Sets an absolute expiration date.</span></span> <span data-ttu-id="041b7-128">L’exemple suivant met en cache le contenu de l’application d’assistance de balise de Cache jusqu'à 5 h 02 le 29 janvier 2025.</span><span class="sxs-lookup"><span data-stu-id="041b7-128">The following example will cache the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025.</span></span>

<span data-ttu-id="041b7-129">Exemple :</span><span class="sxs-lookup"><span data-stu-id="041b7-129">Example:</span></span>

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a><span data-ttu-id="041b7-130">expire après</span><span class="sxs-lookup"><span data-stu-id="041b7-130">expires-after</span></span>

| <span data-ttu-id="041b7-131">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="041b7-131">Attribute Type</span></span>    | <span data-ttu-id="041b7-132">Exemple de valeur</span><span class="sxs-lookup"><span data-stu-id="041b7-132">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="041b7-133">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="041b7-133">TimeSpan</span></span>    | <span data-ttu-id="041b7-134">"@TimeSpan.FromSeconds(120)"</span><span class="sxs-lookup"><span data-stu-id="041b7-134">"@TimeSpan.FromSeconds(120)"</span></span>    |


<span data-ttu-id="041b7-135">Définit la durée à partir de la première demande pour mettre en cache le contenu.</span><span class="sxs-lookup"><span data-stu-id="041b7-135">Sets the length of time from the first request time to cache the contents.</span></span> 

<span data-ttu-id="041b7-136">Exemple :</span><span class="sxs-lookup"><span data-stu-id="041b7-136">Example:</span></span>

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a><span data-ttu-id="041b7-137">expiration de retrait</span><span class="sxs-lookup"><span data-stu-id="041b7-137">expires-sliding</span></span>

| <span data-ttu-id="041b7-138">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="041b7-138">Attribute Type</span></span>    | <span data-ttu-id="041b7-139">Exemple de valeur</span><span class="sxs-lookup"><span data-stu-id="041b7-139">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="041b7-140">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="041b7-140">TimeSpan</span></span>    | <span data-ttu-id="041b7-141">"@TimeSpan.FromSeconds(60)"</span><span class="sxs-lookup"><span data-stu-id="041b7-141">"@TimeSpan.FromSeconds(60)"</span></span>     |


<span data-ttu-id="041b7-142">Définit l’heure à laquelle une entrée de cache doit être supprimée si elle n’a pas été accédé.</span><span class="sxs-lookup"><span data-stu-id="041b7-142">Sets the time that a cache entry should be evicted if it has not been accessed.</span></span>

<span data-ttu-id="041b7-143">Exemple :</span><span class="sxs-lookup"><span data-stu-id="041b7-143">Example:</span></span>

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a><span data-ttu-id="041b7-144">varient en fonction de l’en-tête</span><span class="sxs-lookup"><span data-stu-id="041b7-144">vary-by-header</span></span>

| <span data-ttu-id="041b7-145">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="041b7-145">Attribute Type</span></span>    | <span data-ttu-id="041b7-146">Exemples de valeurs</span><span class="sxs-lookup"><span data-stu-id="041b7-146">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="041b7-147">Chaîne</span><span class="sxs-lookup"><span data-stu-id="041b7-147">String</span></span>            | <span data-ttu-id="041b7-148">« User-Agent »</span><span class="sxs-lookup"><span data-stu-id="041b7-148">"User-Agent"</span></span>                  |
|                   | <span data-ttu-id="041b7-149">« User-Agent, content-encoding »</span><span class="sxs-lookup"><span data-stu-id="041b7-149">"User-Agent,content-encoding"</span></span> |

<span data-ttu-id="041b7-150">Accepte une valeur d’en-tête unique ou une liste séparée par des virgules de valeurs d’en-tête qui déclenchent une actualisation du cache quand ils changent.</span><span class="sxs-lookup"><span data-stu-id="041b7-150">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when they change.</span></span> <span data-ttu-id="041b7-151">L’exemple suivant analyse la valeur d’en-tête `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="041b7-151">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="041b7-152">L’exemple mettra en cache le contenu de chaque autre `User-Agent` présenté au serveur web.</span><span class="sxs-lookup"><span data-stu-id="041b7-152">The example will cache the content for every different `User-Agent` presented to the web server.</span></span>

<span data-ttu-id="041b7-153">Exemple :</span><span class="sxs-lookup"><span data-stu-id="041b7-153">Example:</span></span>

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a><span data-ttu-id="041b7-154">varient par requête</span><span class="sxs-lookup"><span data-stu-id="041b7-154">vary-by-query</span></span>

| <span data-ttu-id="041b7-155">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="041b7-155">Attribute Type</span></span>    | <span data-ttu-id="041b7-156">Exemples de valeurs</span><span class="sxs-lookup"><span data-stu-id="041b7-156">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="041b7-157">Chaîne</span><span class="sxs-lookup"><span data-stu-id="041b7-157">String</span></span>            | <span data-ttu-id="041b7-158">« Vérifiez »</span><span class="sxs-lookup"><span data-stu-id="041b7-158">"Make"</span></span>                |
|                   | <span data-ttu-id="041b7-159">« Assurez-vous, modèle »</span><span class="sxs-lookup"><span data-stu-id="041b7-159">"Make,Model"</span></span> |

<span data-ttu-id="041b7-160">Accepte une valeur d’en-tête unique ou une liste séparée par des virgules de valeurs d’en-tête qui déclenchent une actualisation du cache lorsque la valeur d’en-tête.</span><span class="sxs-lookup"><span data-stu-id="041b7-160">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header value changes.</span></span> <span data-ttu-id="041b7-161">L’exemple suivant présente les valeurs de `Make` et `Model`.</span><span class="sxs-lookup"><span data-stu-id="041b7-161">The following example looks at the values of `Make` and `Model`.</span></span>

<span data-ttu-id="041b7-162">Exemple :</span><span class="sxs-lookup"><span data-stu-id="041b7-162">Example:</span></span>

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a><span data-ttu-id="041b7-163">varient par itinéraire</span><span class="sxs-lookup"><span data-stu-id="041b7-163">vary-by-route</span></span>

| <span data-ttu-id="041b7-164">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="041b7-164">Attribute Type</span></span>    | <span data-ttu-id="041b7-165">Exemples de valeurs</span><span class="sxs-lookup"><span data-stu-id="041b7-165">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="041b7-166">Chaîne</span><span class="sxs-lookup"><span data-stu-id="041b7-166">String</span></span>            | <span data-ttu-id="041b7-167">« Vérifiez »</span><span class="sxs-lookup"><span data-stu-id="041b7-167">"Make"</span></span>                |
|                   | <span data-ttu-id="041b7-168">« Assurez-vous, modèle »</span><span class="sxs-lookup"><span data-stu-id="041b7-168">"Make,Model"</span></span> |

<span data-ttu-id="041b7-169">Accepte une valeur d’en-tête unique ou une liste séparée par des virgules de valeurs d’en-tête qui déclenchent une actualisation du cache lorsque le paramètre de données d’itinéraire spécifiées à modifier.</span><span class="sxs-lookup"><span data-stu-id="041b7-169">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the route data parameter value(s) change.</span></span> <span data-ttu-id="041b7-170">Exemple :</span><span class="sxs-lookup"><span data-stu-id="041b7-170">Example:</span></span>

<span data-ttu-id="041b7-171">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="041b7-171">*Startup.cs*</span></span> 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```
  
<span data-ttu-id="041b7-172">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="041b7-172">*Index.cshtml*</span></span>

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-cookie"></a><span data-ttu-id="041b7-173">varient par cookie</span><span class="sxs-lookup"><span data-stu-id="041b7-173">vary-by-cookie</span></span>

| <span data-ttu-id="041b7-174">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="041b7-174">Attribute Type</span></span>    | <span data-ttu-id="041b7-175">Exemples de valeurs</span><span class="sxs-lookup"><span data-stu-id="041b7-175">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="041b7-176">Chaîne</span><span class="sxs-lookup"><span data-stu-id="041b7-176">String</span></span>            | <span data-ttu-id="041b7-177">".AspNetCore.Identity.Application"</span><span class="sxs-lookup"><span data-stu-id="041b7-177">".AspNetCore.Identity.Application"</span></span>                |
|                   | <span data-ttu-id="041b7-178">".AspNetCore.Identity.Application,HairColor"</span><span class="sxs-lookup"><span data-stu-id="041b7-178">".AspNetCore.Identity.Application,HairColor"</span></span> |

<span data-ttu-id="041b7-179">Accepte une valeur d’en-tête unique ou une liste séparée par des virgules de valeurs d’en-tête qui déclenchent une actualisation du cache lorsque les valeurs d’en-tête changement de (s).</span><span class="sxs-lookup"><span data-stu-id="041b7-179">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header values(s) change.</span></span> <span data-ttu-id="041b7-180">L’exemple suivant présente le cookie associé d’identité ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="041b7-180">The following example looks at the cookie associated with ASP.NET Identity.</span></span> <span data-ttu-id="041b7-181">Lorsqu’un utilisateur est authentifié le cookie de la demande à définir qui déclenche une actualisation du cache.</span><span class="sxs-lookup"><span data-stu-id="041b7-181">When a user is authenticated the request cookie to be set which triggers a cache refresh.</span></span>

<span data-ttu-id="041b7-182">Exemple :</span><span class="sxs-lookup"><span data-stu-id="041b7-182">Example:</span></span>

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a><span data-ttu-id="041b7-183">varient par utilisateur</span><span class="sxs-lookup"><span data-stu-id="041b7-183">vary-by-user</span></span>

| <span data-ttu-id="041b7-184">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="041b7-184">Attribute Type</span></span>    | <span data-ttu-id="041b7-185">Exemples de valeurs</span><span class="sxs-lookup"><span data-stu-id="041b7-185">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="041b7-186">Booléen</span><span class="sxs-lookup"><span data-stu-id="041b7-186">Boolean</span></span>             | <span data-ttu-id="041b7-187">"true"</span><span class="sxs-lookup"><span data-stu-id="041b7-187">"true"</span></span>                  |
|                     | <span data-ttu-id="041b7-188">« false » (valeur par défaut)</span><span class="sxs-lookup"><span data-stu-id="041b7-188">"false" (default)</span></span> |

<span data-ttu-id="041b7-189">Spécifie si le cache doit réinitialiser lorsque l’utilisateur connecté (ou Principal du contexte) change.</span><span class="sxs-lookup"><span data-stu-id="041b7-189">Specifies whether or not the cache should reset when the logged-in user (or Context Principal) changes.</span></span> <span data-ttu-id="041b7-190">L’utilisateur actuel est également connue sous le Principal demande du contexte et peuvent être affiché dans une vue Razor en référençant `@User.Identity.Name`.</span><span class="sxs-lookup"><span data-stu-id="041b7-190">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="041b7-191">L’exemple suivant ressemble au niveau de l’utilisateur actuellement connecté.</span><span class="sxs-lookup"><span data-stu-id="041b7-191">The following example looks at the current logged in user.</span></span>  

<span data-ttu-id="041b7-192">Exemple :</span><span class="sxs-lookup"><span data-stu-id="041b7-192">Example:</span></span>

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="041b7-193">À l’aide de cet attribut gère le contenu dans le cache via un cycle de connexion et déconnexion.</span><span class="sxs-lookup"><span data-stu-id="041b7-193">Using this attribute maintains the contents in cache through a log-in and log-out cycle.</span></span>  <span data-ttu-id="041b7-194">Lorsque vous utilisez `vary-by-user="true"`, une action de connexion et déconnexion invalide le cache pour l’utilisateur authentifié.</span><span class="sxs-lookup"><span data-stu-id="041b7-194">When using `vary-by-user="true"`, a log-in and log-out action invalidates the cache for the authenticated user.</span></span>  <span data-ttu-id="041b7-195">Le cache est invalidé, car une nouvelle valeur de cookie unique est générée sur la connexion.</span><span class="sxs-lookup"><span data-stu-id="041b7-195">The cache is invalidated because a new unique cookie value is generated on login.</span></span> <span data-ttu-id="041b7-196">Cache est conservé à l’état anonyme lorsque aucun cookie n’est présent ou a expiré.</span><span class="sxs-lookup"><span data-stu-id="041b7-196">Cache is maintained for the anonymous state when no cookie is present or has expired.</span></span> <span data-ttu-id="041b7-197">Cela signifie que si aucun utilisateur n’est connecté, le cache est conservé.</span><span class="sxs-lookup"><span data-stu-id="041b7-197">This means if no user is logged in, the cache will be maintained.</span></span>

- - -

### <a name="vary-by"></a><span data-ttu-id="041b7-198">variation de</span><span class="sxs-lookup"><span data-stu-id="041b7-198">vary-by</span></span>

| <span data-ttu-id="041b7-199">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="041b7-199">Attribute Type</span></span>    | <span data-ttu-id="041b7-200">Exemples de valeurs</span><span class="sxs-lookup"><span data-stu-id="041b7-200">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="041b7-201">Chaîne</span><span class="sxs-lookup"><span data-stu-id="041b7-201">String</span></span>             | <span data-ttu-id="041b7-202">"@Model"</span><span class="sxs-lookup"><span data-stu-id="041b7-202">"@Model"</span></span>                 |


<span data-ttu-id="041b7-203">Permet la personnalisation de données obtient mis en cache.</span><span class="sxs-lookup"><span data-stu-id="041b7-203">Allows for customization of what data gets cached.</span></span> <span data-ttu-id="041b7-204">Lorsque l’objet référencé par les modifications apportées à la valeur de la chaîne de l’attribut, le contenu de l’application d’assistance de balise de Cache est mis à jour.</span><span class="sxs-lookup"><span data-stu-id="041b7-204">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="041b7-205">Fréquence à laquelle une concaténation de chaînes de valeurs de modèle sont affectées à cet attribut.</span><span class="sxs-lookup"><span data-stu-id="041b7-205">Often a string-concatenation of model values are assigned to this attribute.</span></span>  <span data-ttu-id="041b7-206">En effet, cela signifie qu'une mise à jour à une des valeurs concaténées invalide le cache.</span><span class="sxs-lookup"><span data-stu-id="041b7-206">Effectively, that means an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="041b7-207">L’exemple suivant suppose la somme de la vue de rendu de la valeur entière des deux paramètres d’itinéraire, la méthode du contrôleur `myParam1` et `myParam2`et qui retourne en tant que la propriété de modèle unique.</span><span class="sxs-lookup"><span data-stu-id="041b7-207">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns that as the single model property.</span></span> <span data-ttu-id="041b7-208">Lorsque cette somme est modifié, le contenu de l’application d’assistance de balise de Cache est rendu et mis en cache à nouveau.</span><span class="sxs-lookup"><span data-stu-id="041b7-208">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="041b7-209">Exemple :</span><span class="sxs-lookup"><span data-stu-id="041b7-209">Example:</span></span>

<span data-ttu-id="041b7-210">Action :</span><span class="sxs-lookup"><span data-stu-id="041b7-210">Action:</span></span>

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

<span data-ttu-id="041b7-211">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="041b7-211">*Index.cshtml*</span></span>

```cshtml
<cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="priority"></a><span data-ttu-id="041b7-212">priority</span><span class="sxs-lookup"><span data-stu-id="041b7-212">priority</span></span>

| <span data-ttu-id="041b7-213">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="041b7-213">Attribute Type</span></span>    | <span data-ttu-id="041b7-214">Exemples de valeurs</span><span class="sxs-lookup"><span data-stu-id="041b7-214">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="041b7-215">CacheItemPriority</span><span class="sxs-lookup"><span data-stu-id="041b7-215">CacheItemPriority</span></span>  | <span data-ttu-id="041b7-216">« High »</span><span class="sxs-lookup"><span data-stu-id="041b7-216">"High"</span></span>                   |
|                    | <span data-ttu-id="041b7-217">« Low »</span><span class="sxs-lookup"><span data-stu-id="041b7-217">"Low"</span></span> |
|                    | <span data-ttu-id="041b7-218">« NeverRemove »</span><span class="sxs-lookup"><span data-stu-id="041b7-218">"NeverRemove"</span></span> |
|                    | <span data-ttu-id="041b7-219">"Normal"</span><span class="sxs-lookup"><span data-stu-id="041b7-219">"Normal"</span></span> |

<span data-ttu-id="041b7-220">Fournit des instructions de suppression de cache au fournisseur de cache intégré.</span><span class="sxs-lookup"><span data-stu-id="041b7-220">Provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="041b7-221">Supprimez le serveur web `Low` mettre en cache les entrées tout d’abord une mémoire insuffisante.</span><span class="sxs-lookup"><span data-stu-id="041b7-221">The web server will evict `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="041b7-222">Exemple :</span><span class="sxs-lookup"><span data-stu-id="041b7-222">Example:</span></span>

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="041b7-223">Le `priority` attribut ne garantit pas un niveau spécifique de la rétention du cache.</span><span class="sxs-lookup"><span data-stu-id="041b7-223">The `priority` attribute doesn't guarantee a specific level of cache retention.</span></span> <span data-ttu-id="041b7-224">`CacheItemPriority`est uniquement une suggestion.</span><span class="sxs-lookup"><span data-stu-id="041b7-224">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="041b7-225">Cet attribut `NeverRemove` ne garantit pas que le cache est conservé.</span><span class="sxs-lookup"><span data-stu-id="041b7-225">Setting this attribute to `NeverRemove` doesn't guarantee that the cache will always be retained.</span></span> <span data-ttu-id="041b7-226">Consultez [des ressources supplémentaires](#additional-resources) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="041b7-226">See [Additional Resources](#additional-resources) for more information.</span></span>

<span data-ttu-id="041b7-227">L’application d’assistance de balise de Cache est dépendante de la [du service de cache mémoire](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="041b7-227">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="041b7-228">L’application d’assistance de balise de Cache ajoute le service s’il n’a pas été ajouté.</span><span class="sxs-lookup"><span data-stu-id="041b7-228">The Cache Tag Helper adds the service if it has not been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="041b7-229">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="041b7-229">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
