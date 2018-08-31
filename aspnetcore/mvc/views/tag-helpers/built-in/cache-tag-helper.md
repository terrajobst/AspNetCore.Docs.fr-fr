---
title: Tag Helper Cache dans ASP.NET Core MVC
author: pkellner
description: Montre comment utiliser un Tag Helper Cache
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 425d8c2235f0070665bc0c967d2498f2cff2a4a6
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751443"
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="69ad7-103">Tag Helper Cache dans ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="69ad7-103">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="69ad7-104">Par [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="69ad7-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="69ad7-105">Le Tag Helper Cache permet d’améliorer considérablement les performances de votre application ASP.NET Core en mettant en cache son contenu dans le fournisseur de caches ASP.NET Core interne.</span><span class="sxs-lookup"><span data-stu-id="69ad7-105">The Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="69ad7-106">Le moteur de vue Razor définit la valeur par défaut `expires-after` sur vingt minutes.</span><span class="sxs-lookup"><span data-stu-id="69ad7-106">The Razor View Engine sets the default `expires-after` to twenty minutes.</span></span>

<span data-ttu-id="69ad7-107">Le balisage Razor suivant met en cache la date/l’heure :</span><span class="sxs-lookup"><span data-stu-id="69ad7-107">The following Razor markup caches the date/time:</span></span>

```cshtml
<cache>@DateTime.Now</cache>
```

<span data-ttu-id="69ad7-108">La première demande à la page qui contient `CacheTagHelper` affiche la date/l’heure actuelle.</span><span class="sxs-lookup"><span data-stu-id="69ad7-108">The first request to the page that contains `CacheTagHelper` will display the current date/time.</span></span> <span data-ttu-id="69ad7-109">Les autres demandes affichent la valeur mise en cache jusqu’à ce que le cache expire (par défaut, 20 minutes) ou soit supprimé par sollicitation de la mémoire.</span><span class="sxs-lookup"><span data-stu-id="69ad7-109">Additional requests will show the cached value until the cache expires (default 20 minutes) or is evicted by memory pressure.</span></span>

<span data-ttu-id="69ad7-110">Vous pouvez définir la durée du cache avec les attributs suivants :</span><span class="sxs-lookup"><span data-stu-id="69ad7-110">You can set the cache duration with the following attributes:</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="69ad7-111">Attributs de Tag Helper Cache</span><span class="sxs-lookup"><span data-stu-id="69ad7-111">Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled"></a><span data-ttu-id="69ad7-112">enabled</span><span class="sxs-lookup"><span data-stu-id="69ad7-112">enabled</span></span>    


| <span data-ttu-id="69ad7-113">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="69ad7-113">Attribute Type</span></span>    | <span data-ttu-id="69ad7-114">Valeurs valides</span><span class="sxs-lookup"><span data-stu-id="69ad7-114">Valid Values</span></span>      |
|----------------   |----------------   |
| <span data-ttu-id="69ad7-115">boolean</span><span class="sxs-lookup"><span data-stu-id="69ad7-115">boolean</span></span>           | <span data-ttu-id="69ad7-116">"true" (valeur par défaut)</span><span class="sxs-lookup"><span data-stu-id="69ad7-116">"true" (default)</span></span>  |
|                   | <span data-ttu-id="69ad7-117">"false"</span><span class="sxs-lookup"><span data-stu-id="69ad7-117">"false"</span></span>   |


<span data-ttu-id="69ad7-118">Détermine si le contenu joint par le Tag Helper Cache est mis en cache.</span><span class="sxs-lookup"><span data-stu-id="69ad7-118">Determines whether the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="69ad7-119">La valeur par défaut est `true`.</span><span class="sxs-lookup"><span data-stu-id="69ad7-119">The default is `true`.</span></span>  <span data-ttu-id="69ad7-120">Si la valeur est `false`, ce Tag Helper Cache n’a aucun effet de mise en cache sur la sortie rendue.</span><span class="sxs-lookup"><span data-stu-id="69ad7-120">If set to `false` this Cache Tag Helper will have no caching effect on the rendered output.</span></span>

<span data-ttu-id="69ad7-121">Exemple :</span><span class="sxs-lookup"><span data-stu-id="69ad7-121">Example:</span></span>

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a><span data-ttu-id="69ad7-122">expires-on</span><span class="sxs-lookup"><span data-stu-id="69ad7-122">expires-on</span></span> 

| <span data-ttu-id="69ad7-123">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="69ad7-123">Attribute Type</span></span> |           <span data-ttu-id="69ad7-124">Exemple de valeur</span><span class="sxs-lookup"><span data-stu-id="69ad7-124">Example Value</span></span>            |
|----------------|------------------------------------|
| <span data-ttu-id="69ad7-125">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="69ad7-125">DateTimeOffset</span></span> | <span data-ttu-id="69ad7-126">"@new DateTime(2025,1,29,17,02,0)"</span><span class="sxs-lookup"><span data-stu-id="69ad7-126">"@new DateTime(2025,1,29,17,02,0)"</span></span> |

<span data-ttu-id="69ad7-127">Définit une date d’expiration absolue.</span><span class="sxs-lookup"><span data-stu-id="69ad7-127">Sets an absolute expiration date.</span></span> <span data-ttu-id="69ad7-128">L’exemple suivant met en cache le contenu du Tag Helper Cache jusqu’à 17:02 le 29 janvier 2025.</span><span class="sxs-lookup"><span data-stu-id="69ad7-128">The following example will cache the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025.</span></span>

<span data-ttu-id="69ad7-129">Exemple :</span><span class="sxs-lookup"><span data-stu-id="69ad7-129">Example:</span></span>

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a><span data-ttu-id="69ad7-130">expires-after</span><span class="sxs-lookup"><span data-stu-id="69ad7-130">expires-after</span></span>

| <span data-ttu-id="69ad7-131">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="69ad7-131">Attribute Type</span></span> |        <span data-ttu-id="69ad7-132">Exemple de valeur</span><span class="sxs-lookup"><span data-stu-id="69ad7-132">Example Value</span></span>         |
|----------------|------------------------------|
|    <span data-ttu-id="69ad7-133">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="69ad7-133">TimeSpan</span></span>    | <span data-ttu-id="69ad7-134">"@TimeSpan.FromSeconds(120)"</span><span class="sxs-lookup"><span data-stu-id="69ad7-134">"@TimeSpan.FromSeconds(120)"</span></span> |

<span data-ttu-id="69ad7-135">Définit la durée à partir de l’heure de la première demande pour mettre en cache le contenu.</span><span class="sxs-lookup"><span data-stu-id="69ad7-135">Sets the length of time from the first request time to cache the contents.</span></span> 

<span data-ttu-id="69ad7-136">Exemple :</span><span class="sxs-lookup"><span data-stu-id="69ad7-136">Example:</span></span>

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a><span data-ttu-id="69ad7-137">expires-sliding</span><span class="sxs-lookup"><span data-stu-id="69ad7-137">expires-sliding</span></span>

| <span data-ttu-id="69ad7-138">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="69ad7-138">Attribute Type</span></span> |        <span data-ttu-id="69ad7-139">Exemple de valeur</span><span class="sxs-lookup"><span data-stu-id="69ad7-139">Example Value</span></span>        |
|----------------|-----------------------------|
|    <span data-ttu-id="69ad7-140">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="69ad7-140">TimeSpan</span></span>    | <span data-ttu-id="69ad7-141">"@TimeSpan.FromSeconds(60)"</span><span class="sxs-lookup"><span data-stu-id="69ad7-141">"@TimeSpan.FromSeconds(60)"</span></span> |

<span data-ttu-id="69ad7-142">Définit l’heure à laquelle une entrée de cache doit être supprimée si elle n’a fait l’objet d’aucun accès.</span><span class="sxs-lookup"><span data-stu-id="69ad7-142">Sets the time that a cache entry should be evicted if it has not been accessed.</span></span>

<span data-ttu-id="69ad7-143">Exemple :</span><span class="sxs-lookup"><span data-stu-id="69ad7-143">Example:</span></span>

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a><span data-ttu-id="69ad7-144">vary-by-header</span><span class="sxs-lookup"><span data-stu-id="69ad7-144">vary-by-header</span></span>

| <span data-ttu-id="69ad7-145">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="69ad7-145">Attribute Type</span></span>    | <span data-ttu-id="69ad7-146">Exemples de valeurs</span><span class="sxs-lookup"><span data-stu-id="69ad7-146">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="69ad7-147">Chaîne</span><span class="sxs-lookup"><span data-stu-id="69ad7-147">String</span></span>            | <span data-ttu-id="69ad7-148">"User-Agent"</span><span class="sxs-lookup"><span data-stu-id="69ad7-148">"User-Agent"</span></span>                  |
|                   | <span data-ttu-id="69ad7-149">"User-Agent,content-encoding"</span><span class="sxs-lookup"><span data-stu-id="69ad7-149">"User-Agent,content-encoding"</span></span> |

<span data-ttu-id="69ad7-150">Accepte une seule valeur d’en-tête ou une liste séparée par des virgules de valeurs d’en-tête qui déclenchent une actualisation du cache quand elles changent.</span><span class="sxs-lookup"><span data-stu-id="69ad7-150">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when they change.</span></span> <span data-ttu-id="69ad7-151">L’exemple suivant analyse la valeur d’en-tête `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="69ad7-151">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="69ad7-152">L’exemple met en cache le contenu de chaque valeur `User-Agent` différente présentée au serveur web.</span><span class="sxs-lookup"><span data-stu-id="69ad7-152">The example will cache the content for every different `User-Agent` presented to the web server.</span></span>

<span data-ttu-id="69ad7-153">Exemple :</span><span class="sxs-lookup"><span data-stu-id="69ad7-153">Example:</span></span>

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a><span data-ttu-id="69ad7-154">vary-by-query</span><span class="sxs-lookup"><span data-stu-id="69ad7-154">vary-by-query</span></span>

| <span data-ttu-id="69ad7-155">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="69ad7-155">Attribute Type</span></span>    | <span data-ttu-id="69ad7-156">Exemples de valeurs</span><span class="sxs-lookup"><span data-stu-id="69ad7-156">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="69ad7-157">Chaîne</span><span class="sxs-lookup"><span data-stu-id="69ad7-157">String</span></span>            | <span data-ttu-id="69ad7-158">"Make"</span><span class="sxs-lookup"><span data-stu-id="69ad7-158">"Make"</span></span>                |
|                   | <span data-ttu-id="69ad7-159">"Make,Model"</span><span class="sxs-lookup"><span data-stu-id="69ad7-159">"Make,Model"</span></span> |

<span data-ttu-id="69ad7-160">Accepte une seule valeur d’en-tête ou une liste séparée par des virgules de valeurs d’en-tête qui déclenchent une actualisation du cache quand la valeur d’en-tête change.</span><span class="sxs-lookup"><span data-stu-id="69ad7-160">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header value changes.</span></span> <span data-ttu-id="69ad7-161">L’exemple suivant examine les valeurs de `Make` et `Model`.</span><span class="sxs-lookup"><span data-stu-id="69ad7-161">The following example looks at the values of `Make` and `Model`.</span></span>

<span data-ttu-id="69ad7-162">Exemple :</span><span class="sxs-lookup"><span data-stu-id="69ad7-162">Example:</span></span>

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a><span data-ttu-id="69ad7-163">vary-by-route</span><span class="sxs-lookup"><span data-stu-id="69ad7-163">vary-by-route</span></span>

| <span data-ttu-id="69ad7-164">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="69ad7-164">Attribute Type</span></span>    | <span data-ttu-id="69ad7-165">Exemples de valeurs</span><span class="sxs-lookup"><span data-stu-id="69ad7-165">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="69ad7-166">Chaîne</span><span class="sxs-lookup"><span data-stu-id="69ad7-166">String</span></span>            | <span data-ttu-id="69ad7-167">"Make"</span><span class="sxs-lookup"><span data-stu-id="69ad7-167">"Make"</span></span>                |
|                   | <span data-ttu-id="69ad7-168">"Make,Model"</span><span class="sxs-lookup"><span data-stu-id="69ad7-168">"Make,Model"</span></span> |

<span data-ttu-id="69ad7-169">Accepte une seule valeur d’en-tête ou une liste séparée par des virgules de valeurs d’en-tête qui déclenchent une actualisation du cache quand les valeurs des paramètres de données de routage changent.</span><span class="sxs-lookup"><span data-stu-id="69ad7-169">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the route data parameter value(s) change.</span></span> <span data-ttu-id="69ad7-170">Exemple :</span><span class="sxs-lookup"><span data-stu-id="69ad7-170">Example:</span></span>

<span data-ttu-id="69ad7-171">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="69ad7-171">*Startup.cs*</span></span> 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```

<span data-ttu-id="69ad7-172">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="69ad7-172">*Index.cshtml*</span></span>

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-cookie"></a><span data-ttu-id="69ad7-173">vary-by-cookie</span><span class="sxs-lookup"><span data-stu-id="69ad7-173">vary-by-cookie</span></span>

| <span data-ttu-id="69ad7-174">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="69ad7-174">Attribute Type</span></span>    | <span data-ttu-id="69ad7-175">Exemples de valeurs</span><span class="sxs-lookup"><span data-stu-id="69ad7-175">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="69ad7-176">Chaîne</span><span class="sxs-lookup"><span data-stu-id="69ad7-176">String</span></span>            | <span data-ttu-id="69ad7-177">".AspNetCore.Identity.Application"</span><span class="sxs-lookup"><span data-stu-id="69ad7-177">".AspNetCore.Identity.Application"</span></span>                |
|                   | <span data-ttu-id="69ad7-178">".AspNetCore.Identity.Application,HairColor"</span><span class="sxs-lookup"><span data-stu-id="69ad7-178">".AspNetCore.Identity.Application,HairColor"</span></span> |

<span data-ttu-id="69ad7-179">Accepte une seule valeur d’en-tête ou une liste séparée par des virgules de valeurs d’en-tête qui déclenchent une actualisation du cache quand les valeurs d’en-tête changent.</span><span class="sxs-lookup"><span data-stu-id="69ad7-179">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header values(s) change.</span></span> <span data-ttu-id="69ad7-180">L’exemple suivant examine le cookie associé à ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="69ad7-180">The following example looks at the cookie associated with ASP.NET Core Identity.</span></span> <span data-ttu-id="69ad7-181">Quand un utilisateur est authentifié, cookie de la demande à définir qui déclenche une actualisation du cache.</span><span class="sxs-lookup"><span data-stu-id="69ad7-181">When a user is authenticated the request cookie to be set which triggers a cache refresh.</span></span>

<span data-ttu-id="69ad7-182">Exemple :</span><span class="sxs-lookup"><span data-stu-id="69ad7-182">Example:</span></span>

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a><span data-ttu-id="69ad7-183">vary-by-user</span><span class="sxs-lookup"><span data-stu-id="69ad7-183">vary-by-user</span></span>

| <span data-ttu-id="69ad7-184">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="69ad7-184">Attribute Type</span></span>    | <span data-ttu-id="69ad7-185">Exemples de valeurs</span><span class="sxs-lookup"><span data-stu-id="69ad7-185">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="69ad7-186">Booléen</span><span class="sxs-lookup"><span data-stu-id="69ad7-186">Boolean</span></span>             | <span data-ttu-id="69ad7-187">"true"</span><span class="sxs-lookup"><span data-stu-id="69ad7-187">"true"</span></span>                  |
|                     | <span data-ttu-id="69ad7-188">"false" (valeur par défaut)</span><span class="sxs-lookup"><span data-stu-id="69ad7-188">"false" (default)</span></span> |

<span data-ttu-id="69ad7-189">Spécifie si le cache doit ou non être réinitialisé quand l’utilisateur connecté (ou principal du contexte) change.</span><span class="sxs-lookup"><span data-stu-id="69ad7-189">Specifies whether or not the cache should reset when the logged-in user (or Context Principal) changes.</span></span> <span data-ttu-id="69ad7-190">L’utilisateur actuel est également connu comme principal du contexte de la demande et peut être affiché dans une vue Razor en référençant `@User.Identity.Name`.</span><span class="sxs-lookup"><span data-stu-id="69ad7-190">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="69ad7-191">L’exemple suivant examine l’utilisateur actuellement connecté.</span><span class="sxs-lookup"><span data-stu-id="69ad7-191">The following example looks at the current logged in user.</span></span>  

<span data-ttu-id="69ad7-192">Exemple :</span><span class="sxs-lookup"><span data-stu-id="69ad7-192">Example:</span></span>

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="69ad7-193">L’utilisation de cet attribut permet de conserver le contenu dans le cache via un cycle de connexion et de déconnexion.</span><span class="sxs-lookup"><span data-stu-id="69ad7-193">Using this attribute maintains the contents in cache through a log-in and log-out cycle.</span></span>  <span data-ttu-id="69ad7-194">Quand vous utilisez `vary-by-user="true"`, une action de connexion et de déconnexion invalide le cache pour l’utilisateur authentifié.</span><span class="sxs-lookup"><span data-stu-id="69ad7-194">When using `vary-by-user="true"`, a log-in and log-out action invalidates the cache for the authenticated user.</span></span>  <span data-ttu-id="69ad7-195">Le cache est invalidé, car une nouvelle valeur de cookie unique est générée lors de la connexion.</span><span class="sxs-lookup"><span data-stu-id="69ad7-195">The cache is invalidated because a new unique cookie value is generated on login.</span></span> <span data-ttu-id="69ad7-196">Le cache est conservé pour l’état anonyme quand aucun cookie n’est présent ou n’a expiré.</span><span class="sxs-lookup"><span data-stu-id="69ad7-196">Cache is maintained for the anonymous state when no cookie is present or has expired.</span></span> <span data-ttu-id="69ad7-197">Cela signifie que, si aucun utilisateur n’est connecté, le cache est conservé.</span><span class="sxs-lookup"><span data-stu-id="69ad7-197">This means if no user is logged in, the cache will be maintained.</span></span>

- - -

### <a name="vary-by"></a><span data-ttu-id="69ad7-198">vary-by</span><span class="sxs-lookup"><span data-stu-id="69ad7-198">vary-by</span></span>

| <span data-ttu-id="69ad7-199">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="69ad7-199">Attribute Type</span></span> | <span data-ttu-id="69ad7-200">Exemples de valeurs</span><span class="sxs-lookup"><span data-stu-id="69ad7-200">Example Values</span></span> |
|----------------|----------------|
|     <span data-ttu-id="69ad7-201">Chaîne</span><span class="sxs-lookup"><span data-stu-id="69ad7-201">String</span></span>     |    <span data-ttu-id="69ad7-202">"@Model"</span><span class="sxs-lookup"><span data-stu-id="69ad7-202">"@Model"</span></span>    |

<span data-ttu-id="69ad7-203">Permet la personnalisation des données mises en cache.</span><span class="sxs-lookup"><span data-stu-id="69ad7-203">Allows for customization of what data gets cached.</span></span> <span data-ttu-id="69ad7-204">Quand l’objet référencé par la valeur de la chaîne de l’attribut change, le contenu du Tag Helper Cache est mis à jour.</span><span class="sxs-lookup"><span data-stu-id="69ad7-204">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="69ad7-205">Souvent, une concaténation de chaîne des valeurs de modèle est affectée à cet attribut.</span><span class="sxs-lookup"><span data-stu-id="69ad7-205">Often a string-concatenation of model values are assigned to this attribute.</span></span>  <span data-ttu-id="69ad7-206">En effet, cela signifie qu’une mise à jour de l’une des valeurs concaténées invalide le cache.</span><span class="sxs-lookup"><span data-stu-id="69ad7-206">Effectively, that means an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="69ad7-207">L’exemple suivant suppose que la méthode de contrôleur restituant la vue additionne la valeur entière des deux paramètres de routage, `myParam1` et `myParam2`, et la retourne en tant que propriété de modèle unique.</span><span class="sxs-lookup"><span data-stu-id="69ad7-207">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns that as the single model property.</span></span> <span data-ttu-id="69ad7-208">Quand cette somme est modifiée, le contenu du Tag Helper Cache est rendu et mis en cache à nouveau.</span><span class="sxs-lookup"><span data-stu-id="69ad7-208">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="69ad7-209">Exemple :</span><span class="sxs-lookup"><span data-stu-id="69ad7-209">Example:</span></span>

<span data-ttu-id="69ad7-210">Action :</span><span class="sxs-lookup"><span data-stu-id="69ad7-210">Action:</span></span>

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

<span data-ttu-id="69ad7-211">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="69ad7-211">*Index.cshtml*</span></span>

```cshtml
<cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="priority"></a><span data-ttu-id="69ad7-212">priority</span><span class="sxs-lookup"><span data-stu-id="69ad7-212">priority</span></span>

| <span data-ttu-id="69ad7-213">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="69ad7-213">Attribute Type</span></span>    | <span data-ttu-id="69ad7-214">Exemples de valeurs</span><span class="sxs-lookup"><span data-stu-id="69ad7-214">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="69ad7-215">CacheItemPriority</span><span class="sxs-lookup"><span data-stu-id="69ad7-215">CacheItemPriority</span></span>  | <span data-ttu-id="69ad7-216">"High"</span><span class="sxs-lookup"><span data-stu-id="69ad7-216">"High"</span></span>                   |
|                    | <span data-ttu-id="69ad7-217">"Low"</span><span class="sxs-lookup"><span data-stu-id="69ad7-217">"Low"</span></span> |
|                    | <span data-ttu-id="69ad7-218">"NeverRemove"</span><span class="sxs-lookup"><span data-stu-id="69ad7-218">"NeverRemove"</span></span> |
|                    | <span data-ttu-id="69ad7-219">"Normal"</span><span class="sxs-lookup"><span data-stu-id="69ad7-219">"Normal"</span></span> |

<span data-ttu-id="69ad7-220">Fournit des instructions de suppression de cache au fournisseur de caches intégré.</span><span class="sxs-lookup"><span data-stu-id="69ad7-220">Provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="69ad7-221">Le serveur web supprime d’abord les entrées de cache `Low` en cas de sollicitation de la mémoire.</span><span class="sxs-lookup"><span data-stu-id="69ad7-221">The web server will evict `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="69ad7-222">Exemple :</span><span class="sxs-lookup"><span data-stu-id="69ad7-222">Example:</span></span>

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="69ad7-223">L’attribut `priority` ne garantit pas un niveau spécifique de rétention du cache.</span><span class="sxs-lookup"><span data-stu-id="69ad7-223">The `priority` attribute doesn't guarantee a specific level of cache retention.</span></span> <span data-ttu-id="69ad7-224">`CacheItemPriority` est uniquement une suggestion.</span><span class="sxs-lookup"><span data-stu-id="69ad7-224">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="69ad7-225">Si vous affectez à cet attribut la valeur `NeverRemove`, il n’est pas garanti que le cache sera toujours conservé.</span><span class="sxs-lookup"><span data-stu-id="69ad7-225">Setting this attribute to `NeverRemove` doesn't guarantee that the cache will always be retained.</span></span> <span data-ttu-id="69ad7-226">Pour plus d’informations, consultez [Ressources supplémentaires](#additional-resources).</span><span class="sxs-lookup"><span data-stu-id="69ad7-226">See [Additional Resources](#additional-resources) for more information.</span></span>

<span data-ttu-id="69ad7-227">Le Tag Helper Cache dépend du [service de cache en mémoire](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="69ad7-227">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="69ad7-228">Le Tag Helper Cache ajoute le service s’il ne l’a pas encore été.</span><span class="sxs-lookup"><span data-stu-id="69ad7-228">The Cache Tag Helper adds the service if it has not been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="69ad7-229">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="69ad7-229">Additional resources</span></span>

* [<span data-ttu-id="69ad7-230">Mettre en cache en mémoire</span><span class="sxs-lookup"><span data-stu-id="69ad7-230">Cache in-memory</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="69ad7-231">Présentation d’Identity</span><span class="sxs-lookup"><span data-stu-id="69ad7-231">Introduction to Identity</span></span>](xref:security/authentication/identity)
