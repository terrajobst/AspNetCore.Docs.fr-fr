---
title: Tag Helper Cache dans ASP.NET Core MVC
author: pkellner
description: Découvrez comment utiliser le Tag Helper Cache.
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: fb69584f6e9d4756e175bbd6f3deb1f413b80fc5
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244812"
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="6c48c-103">Tag Helper Cache dans ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="6c48c-103">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="6c48c-104">Par [Peter Kellner](http://peterkellner.net) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6c48c-104">By [Peter Kellner](http://peterkellner.net) and [Luke Latham](https://github.com/guardrex)</span></span> 

<span data-ttu-id="6c48c-105">Le Tag Helper Cache permet d’améliorer les performances de votre application ASP.NET Core en mettant en cache son contenu dans le fournisseur de caches ASP.NET Core interne.</span><span class="sxs-lookup"><span data-stu-id="6c48c-105">The Cache Tag Helper provides the ability to improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="6c48c-106">Pour avoir une vue d’ensemble des Tag Helpers, consultez <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="6c48c-106">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

<span data-ttu-id="6c48c-107">Le balisage Razor suivant met en cache la date actuelle :</span><span class="sxs-lookup"><span data-stu-id="6c48c-107">The following Razor markup caches the current date:</span></span>

```cshtml
<cache>@DateTime.Now</cache>
```

<span data-ttu-id="6c48c-108">La première requête à la page qui contient le Tag Helper affiche la date actuelle.</span><span class="sxs-lookup"><span data-stu-id="6c48c-108">The first request to the page that contains the Tag Helper displays the current date.</span></span> <span data-ttu-id="6c48c-109">Les autres requêtes affichent la valeur mise en cache jusqu’à ce que le cache expire (par défaut, 20 minutes) ou jusqu’à ce que la date du cache soit supprimée du cache.</span><span class="sxs-lookup"><span data-stu-id="6c48c-109">Additional requests show the cached value until the cache expires (default 20 minutes) or until the cached date is evicted from the cache.</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="6c48c-110">Attributs de Tag Helper Cache</span><span class="sxs-lookup"><span data-stu-id="6c48c-110">Cache Tag Helper Attributes</span></span>

### <a name="enabled"></a><span data-ttu-id="6c48c-111">enabled</span><span class="sxs-lookup"><span data-stu-id="6c48c-111">enabled</span></span>

| <span data-ttu-id="6c48c-112">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="6c48c-112">Attribute Type</span></span>  | <span data-ttu-id="6c48c-113">Exemples</span><span class="sxs-lookup"><span data-stu-id="6c48c-113">Examples</span></span>        | <span data-ttu-id="6c48c-114">Par défaut</span><span class="sxs-lookup"><span data-stu-id="6c48c-114">Default</span></span> |
| --------------- | --------------- | ------- |
| <span data-ttu-id="6c48c-115">Booléen</span><span class="sxs-lookup"><span data-stu-id="6c48c-115">Boolean</span></span>         | <span data-ttu-id="6c48c-116">`true`, `false`</span><span class="sxs-lookup"><span data-stu-id="6c48c-116">`true`, `false`</span></span> | `true`  |

<span data-ttu-id="6c48c-117">`enabled` détermine si le contenu joint par le Tag Helper Cache est mis en cache.</span><span class="sxs-lookup"><span data-stu-id="6c48c-117">`enabled` determines if the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="6c48c-118">La valeur par défaut est `true`.</span><span class="sxs-lookup"><span data-stu-id="6c48c-118">The default is `true`.</span></span> <span data-ttu-id="6c48c-119">Si la valeur est `false`, la sortie rendue n’est **pas** mise en cache.</span><span class="sxs-lookup"><span data-stu-id="6c48c-119">If set to `false`, the rendered output is **not** cached.</span></span>

<span data-ttu-id="6c48c-120">Exemple :</span><span class="sxs-lookup"><span data-stu-id="6c48c-120">Example:</span></span>

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="expires-on"></a><span data-ttu-id="6c48c-121">expires-on</span><span class="sxs-lookup"><span data-stu-id="6c48c-121">expires-on</span></span>

| <span data-ttu-id="6c48c-122">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="6c48c-122">Attribute Type</span></span>   | <span data-ttu-id="6c48c-123">Exemple</span><span class="sxs-lookup"><span data-stu-id="6c48c-123">Example</span></span>                            |
| ---------------- | ---------------------------------- |
| `DateTimeOffset` | `@new DateTime(2025,1,29,17,02,0)` |

<span data-ttu-id="6c48c-124">`expires-on` définit une date d’expiration absolue pour l’élément mis en cache.</span><span class="sxs-lookup"><span data-stu-id="6c48c-124">`expires-on` sets an absolute expiration date for the cached item.</span></span>

<span data-ttu-id="6c48c-125">L’exemple suivant met en cache le contenu du Tag Helper Cache jusqu’à 17:02 le 29 janvier 2025 :</span><span class="sxs-lookup"><span data-stu-id="6c48c-125">The following example caches the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025:</span></span>

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="expires-after"></a><span data-ttu-id="6c48c-126">expires-after</span><span class="sxs-lookup"><span data-stu-id="6c48c-126">expires-after</span></span>

| <span data-ttu-id="6c48c-127">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="6c48c-127">Attribute Type</span></span> | <span data-ttu-id="6c48c-128">Exemple</span><span class="sxs-lookup"><span data-stu-id="6c48c-128">Example</span></span>                      | <span data-ttu-id="6c48c-129">Par défaut</span><span class="sxs-lookup"><span data-stu-id="6c48c-129">Default</span></span>    |
| -------------- | ---------------------------- | ---------- |
| `TimeSpan`     | `@TimeSpan.FromSeconds(120)` | <span data-ttu-id="6c48c-130">20 minutes</span><span class="sxs-lookup"><span data-stu-id="6c48c-130">20 minutes</span></span> |

<span data-ttu-id="6c48c-131">`expires-after` définit la durée à partir de l’heure de la première demande pour mettre en cache le contenu.</span><span class="sxs-lookup"><span data-stu-id="6c48c-131">`expires-after` sets the length of time from the first request time to cache the contents.</span></span>

<span data-ttu-id="6c48c-132">Exemple :</span><span class="sxs-lookup"><span data-stu-id="6c48c-132">Example:</span></span>

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="6c48c-133">Le moteur de vue Razor définit la valeur par défaut `expires-after` sur vingt minutes.</span><span class="sxs-lookup"><span data-stu-id="6c48c-133">The Razor View Engine sets the default `expires-after` value to twenty minutes.</span></span>

### <a name="expires-sliding"></a><span data-ttu-id="6c48c-134">expires-sliding</span><span class="sxs-lookup"><span data-stu-id="6c48c-134">expires-sliding</span></span>

| <span data-ttu-id="6c48c-135">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="6c48c-135">Attribute Type</span></span> | <span data-ttu-id="6c48c-136">Exemple</span><span class="sxs-lookup"><span data-stu-id="6c48c-136">Example</span></span>                     |
| -------------- | --------------------------- |
| `TimeSpan`     | `@TimeSpan.FromSeconds(60)` |

<span data-ttu-id="6c48c-137">Définit l’heure à laquelle une entrée de cache doit être supprimée si sa valeur n’a fait l’objet d’aucun accès.</span><span class="sxs-lookup"><span data-stu-id="6c48c-137">Sets the time that a cache entry should be evicted if its value hasn't been accessed.</span></span>

<span data-ttu-id="6c48c-138">Exemple :</span><span class="sxs-lookup"><span data-stu-id="6c48c-138">Example:</span></span>

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-header"></a><span data-ttu-id="6c48c-139">vary-by-header</span><span class="sxs-lookup"><span data-stu-id="6c48c-139">vary-by-header</span></span>

| <span data-ttu-id="6c48c-140">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="6c48c-140">Attribute Type</span></span> | <span data-ttu-id="6c48c-141">Exemples</span><span class="sxs-lookup"><span data-stu-id="6c48c-141">Examples</span></span>                                    |
| -------------- | ------------------------------------------- |
| <span data-ttu-id="6c48c-142">Chaîne</span><span class="sxs-lookup"><span data-stu-id="6c48c-142">String</span></span>         | <span data-ttu-id="6c48c-143">`User-Agent`, `User-Agent,content-encoding`</span><span class="sxs-lookup"><span data-stu-id="6c48c-143">`User-Agent`, `User-Agent,content-encoding`</span></span> |

<span data-ttu-id="6c48c-144">`vary-by-header` accepte une liste séparée par des virgules de valeurs d’en-tête qui déclenchent une actualisation du cache quand elles changent.</span><span class="sxs-lookup"><span data-stu-id="6c48c-144">`vary-by-header` accepts a comma-delimited list of header values that trigger a cache refresh when they change.</span></span>

<span data-ttu-id="6c48c-145">L’exemple suivant analyse la valeur d’en-tête `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="6c48c-145">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="6c48c-146">L’exemple met en cache le contenu de chaque valeur `User-Agent` différente présentée au serveur web :</span><span class="sxs-lookup"><span data-stu-id="6c48c-146">The example caches the content for every different `User-Agent` presented to the web server:</span></span>

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-query"></a><span data-ttu-id="6c48c-147">vary-by-query</span><span class="sxs-lookup"><span data-stu-id="6c48c-147">vary-by-query</span></span>

| <span data-ttu-id="6c48c-148">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="6c48c-148">Attribute Type</span></span> | <span data-ttu-id="6c48c-149">Exemples</span><span class="sxs-lookup"><span data-stu-id="6c48c-149">Examples</span></span>             |
| -------------- | -------------------- |
| <span data-ttu-id="6c48c-150">Chaîne</span><span class="sxs-lookup"><span data-stu-id="6c48c-150">String</span></span>         | <span data-ttu-id="6c48c-151">`Make`, `Make,Model`</span><span class="sxs-lookup"><span data-stu-id="6c48c-151">`Make`, `Make,Model`</span></span> |

<span data-ttu-id="6c48c-152">`vary-by-query` accepte une liste de <xref:Microsoft.AspNetCore.Http.IQueryCollection.Keys*> séparées par des virgules dans une chaîne de requête (<xref:Microsoft.AspNetCore.Http.HttpRequest.Query*>) déclenchant une actualisation du cache quand la valeur de l’une des clés répertoriées est modifiée.</span><span class="sxs-lookup"><span data-stu-id="6c48c-152">`vary-by-query` accepts a comma-delimited list of <xref:Microsoft.AspNetCore.Http.IQueryCollection.Keys*> in a query string (<xref:Microsoft.AspNetCore.Http.HttpRequest.Query*>) that trigger a cache refresh when the value of any listed key changes.</span></span>

<span data-ttu-id="6c48c-153">L’exemple suivant analyse les valeurs de `Make` et `Model`.</span><span class="sxs-lookup"><span data-stu-id="6c48c-153">The following example monitors the values of `Make` and `Model`.</span></span> <span data-ttu-id="6c48c-154">L’exemple met en cache le contenu de chaque valeur `Make` et `Model` différente présentée au serveur web :</span><span class="sxs-lookup"><span data-stu-id="6c48c-154">The example caches the content for every different `Make` and `Model` presented to the web server:</span></span>

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-route"></a><span data-ttu-id="6c48c-155">vary-by-route</span><span class="sxs-lookup"><span data-stu-id="6c48c-155">vary-by-route</span></span>

| <span data-ttu-id="6c48c-156">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="6c48c-156">Attribute Type</span></span> | <span data-ttu-id="6c48c-157">Exemples</span><span class="sxs-lookup"><span data-stu-id="6c48c-157">Examples</span></span>             |
| -------------- | -------------------- |
| <span data-ttu-id="6c48c-158">Chaîne</span><span class="sxs-lookup"><span data-stu-id="6c48c-158">String</span></span>         | <span data-ttu-id="6c48c-159">`Make`, `Make,Model`</span><span class="sxs-lookup"><span data-stu-id="6c48c-159">`Make`, `Make,Model`</span></span> |

<span data-ttu-id="6c48c-160">`vary-by-route` accepte une liste séparée par des virgules de noms de paramètre de route qui déclenchent une actualisation du cache quand la valeur du paramètre des données de route change.</span><span class="sxs-lookup"><span data-stu-id="6c48c-160">`vary-by-route` accepts a comma-delimited list of route parameter names that trigger a cache refresh when the route data parameter value changes.</span></span>

<span data-ttu-id="6c48c-161">Exemple :</span><span class="sxs-lookup"><span data-stu-id="6c48c-161">Example:</span></span>

<span data-ttu-id="6c48c-162">*Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="6c48c-162">*Startup.cs*:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```

<span data-ttu-id="6c48c-163">*Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6c48c-163">*Index.cshtml*:</span></span>

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-cookie"></a><span data-ttu-id="6c48c-164">vary-by-cookie</span><span class="sxs-lookup"><span data-stu-id="6c48c-164">vary-by-cookie</span></span>

| <span data-ttu-id="6c48c-165">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="6c48c-165">Attribute Type</span></span> | <span data-ttu-id="6c48c-166">Exemples</span><span class="sxs-lookup"><span data-stu-id="6c48c-166">Examples</span></span>                                                                         |
| -------------- | -------------------------------------------------------------------------------- |
| <span data-ttu-id="6c48c-167">Chaîne</span><span class="sxs-lookup"><span data-stu-id="6c48c-167">String</span></span>         | <span data-ttu-id="6c48c-168">`.AspNetCore.Identity.Application`, `.AspNetCore.Identity.Application,HairColor`</span><span class="sxs-lookup"><span data-stu-id="6c48c-168">`.AspNetCore.Identity.Application`, `.AspNetCore.Identity.Application,HairColor`</span></span> |

<span data-ttu-id="6c48c-169">`vary-by-cookie` accepte une liste séparée par des virgules de noms de cookie qui déclenchent une actualisation du cache quand la valeur du cookie change.</span><span class="sxs-lookup"><span data-stu-id="6c48c-169">`vary-by-cookie` accepts a comma-delimited list of cookie names that trigger a cache refresh when the cookie values change.</span></span>

<span data-ttu-id="6c48c-170">L’exemple suivant analyse le cookie associé à ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="6c48c-170">The following example monitors the cookie associated with ASP.NET Core Identity.</span></span> <span data-ttu-id="6c48c-171">Lorsqu’un utilisateur est authentifié, une modification dans le cookie Identity déclenche une actualisation du cache :</span><span class="sxs-lookup"><span data-stu-id="6c48c-171">When a user is authenticated, a change in the Identity cookie triggers a cache refresh:</span></span>

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-user"></a><span data-ttu-id="6c48c-172">vary-by-user</span><span class="sxs-lookup"><span data-stu-id="6c48c-172">vary-by-user</span></span>

| <span data-ttu-id="6c48c-173">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="6c48c-173">Attribute Type</span></span>  | <span data-ttu-id="6c48c-174">Exemples</span><span class="sxs-lookup"><span data-stu-id="6c48c-174">Examples</span></span>        | <span data-ttu-id="6c48c-175">Par défaut</span><span class="sxs-lookup"><span data-stu-id="6c48c-175">Default</span></span> |
| --------------- | --------------- | ------- |
| <span data-ttu-id="6c48c-176">Booléen</span><span class="sxs-lookup"><span data-stu-id="6c48c-176">Boolean</span></span>         | <span data-ttu-id="6c48c-177">`true`, `false`</span><span class="sxs-lookup"><span data-stu-id="6c48c-177">`true`, `false`</span></span> | `true`  |

<span data-ttu-id="6c48c-178">`vary-by-user` spécifie si le cache se réinitialise ou pas quand l’utilisateur connecté (ou principal du contexte) change.</span><span class="sxs-lookup"><span data-stu-id="6c48c-178">`vary-by-user` specifies whether or not the cache resets when the signed-in user (or Context Principal) changes.</span></span> <span data-ttu-id="6c48c-179">L’utilisateur actuel est également connu comme principal du contexte de la demande et peut être affiché dans une vue Razor en référençant `@User.Identity.Name`.</span><span class="sxs-lookup"><span data-stu-id="6c48c-179">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="6c48c-180">L’exemple suivant analyse l’utilisateur actuellement connecté pour déclencher une actualisation du cache :</span><span class="sxs-lookup"><span data-stu-id="6c48c-180">The following example monitors the current logged in user to trigger a cache refresh:</span></span>

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="6c48c-181">L’utilisation de cet attribut permet de conserver le contenu dans le cache lors d’un cycle de connexion et de déconnexion.</span><span class="sxs-lookup"><span data-stu-id="6c48c-181">Using this attribute maintains the contents in cache through a sign-in and sign-out cycle.</span></span> <span data-ttu-id="6c48c-182">Lorsque la valeur est définie sur `true`, un cycle d’authentification invalide le cache pour l’utilisateur authentifié.</span><span class="sxs-lookup"><span data-stu-id="6c48c-182">When the value is set to `true`, an authentication cycle invalidates the cache for the authenticated user.</span></span> <span data-ttu-id="6c48c-183">Le cache est invalidé, car une nouvelle valeur de cookie unique est générée quand un utilisateur est authentifié.</span><span class="sxs-lookup"><span data-stu-id="6c48c-183">The cache is invalidated because a new unique cookie value is generated when a user is authenticated.</span></span> <span data-ttu-id="6c48c-184">Le cache est conservé pour l’état anonyme quand aucun cookie n’est présent ou quand le cookie a expiré.</span><span class="sxs-lookup"><span data-stu-id="6c48c-184">Cache is maintained for the anonymous state when no cookie is present or the cookie has expired.</span></span> <span data-ttu-id="6c48c-185">Si l’utilisateur n’est **pas** authentifié, le cache est conservé.</span><span class="sxs-lookup"><span data-stu-id="6c48c-185">If the user is **not** authenticated, the cache is maintained.</span></span>

### <a name="vary-by"></a><span data-ttu-id="6c48c-186">vary-by</span><span class="sxs-lookup"><span data-stu-id="6c48c-186">vary-by</span></span>

| <span data-ttu-id="6c48c-187">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="6c48c-187">Attribute Type</span></span> | <span data-ttu-id="6c48c-188">Exemple</span><span class="sxs-lookup"><span data-stu-id="6c48c-188">Example</span></span>  |
| -------------- | -------- |
| <span data-ttu-id="6c48c-189">Chaîne</span><span class="sxs-lookup"><span data-stu-id="6c48c-189">String</span></span>         | `@Model` |

<span data-ttu-id="6c48c-190">`vary-by` permet la personnalisation des données mises en cache.</span><span class="sxs-lookup"><span data-stu-id="6c48c-190">`vary-by` allows for customization of what data is cached.</span></span> <span data-ttu-id="6c48c-191">Quand l’objet référencé par la valeur de la chaîne de l’attribut change, le contenu du Tag Helper Cache est mis à jour.</span><span class="sxs-lookup"><span data-stu-id="6c48c-191">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="6c48c-192">Souvent, une concaténation de chaîne des valeurs de modèle est affectée à cet attribut.</span><span class="sxs-lookup"><span data-stu-id="6c48c-192">Often, a string-concatenation of model values are assigned to this attribute.</span></span> <span data-ttu-id="6c48c-193">En effet, cela entraîne un scénario où une mise à jour de l’une des valeurs concaténées invalide le cache.</span><span class="sxs-lookup"><span data-stu-id="6c48c-193">Effectively, this results in a scenario where an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="6c48c-194">L’exemple suivant suppose que la méthode de contrôleur restituant la vue additionne la valeur entière des deux paramètres de routage, `myParam1` et `myParam2`, et retourne la somme en tant que propriété de modèle unique.</span><span class="sxs-lookup"><span data-stu-id="6c48c-194">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns the sum as the single model property.</span></span> <span data-ttu-id="6c48c-195">Quand cette somme est modifiée, le contenu du Tag Helper Cache est rendu et mis en cache à nouveau.</span><span class="sxs-lookup"><span data-stu-id="6c48c-195">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="6c48c-196">Action :</span><span class="sxs-lookup"><span data-stu-id="6c48c-196">Action:</span></span>

```csharp
public IActionResult Index(string myParam1, string myParam2, string myParam3)
{
    int num1;
    int num2;
    int.TryParse(myParam1, out num1);
    int.TryParse(myParam2, out num2);
    return View(viewName, num1 + num2);
}
```

<span data-ttu-id="6c48c-197">*Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6c48c-197">*Index.cshtml*:</span></span>

```cshtml
<cache vary-by="@Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="priority"></a><span data-ttu-id="6c48c-198">priority</span><span class="sxs-lookup"><span data-stu-id="6c48c-198">priority</span></span>

| <span data-ttu-id="6c48c-199">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="6c48c-199">Attribute Type</span></span>      | <span data-ttu-id="6c48c-200">Exemples</span><span class="sxs-lookup"><span data-stu-id="6c48c-200">Examples</span></span>                               | <span data-ttu-id="6c48c-201">Par défaut</span><span class="sxs-lookup"><span data-stu-id="6c48c-201">Default</span></span>  |
| ------------------- | -------------------------------------- | -------- |
| `CacheItemPriority` | <span data-ttu-id="6c48c-202">`High`, `Low`, `NeverRemove`, `Normal`</span><span class="sxs-lookup"><span data-stu-id="6c48c-202">`High`, `Low`, `NeverRemove`, `Normal`</span></span> | `Normal` |

<span data-ttu-id="6c48c-203">`priority` fournit des instructions de suppression de cache au fournisseur de caches intégré.</span><span class="sxs-lookup"><span data-stu-id="6c48c-203">`priority` provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="6c48c-204">Le serveur web supprime d’abord les entrées de cache `Low` en cas de sollicitation de la mémoire.</span><span class="sxs-lookup"><span data-stu-id="6c48c-204">The web server evicts `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="6c48c-205">Exemple :</span><span class="sxs-lookup"><span data-stu-id="6c48c-205">Example:</span></span>

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="6c48c-206">L’attribut `priority` ne garantit pas un niveau spécifique de rétention du cache.</span><span class="sxs-lookup"><span data-stu-id="6c48c-206">The `priority` attribute doesn't guarantee a specific level of cache retention.</span></span> <span data-ttu-id="6c48c-207">`CacheItemPriority` est uniquement une suggestion.</span><span class="sxs-lookup"><span data-stu-id="6c48c-207">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="6c48c-208">Si vous affectez à cet attribut la valeur `NeverRemove`, il n’est pas garanti que les éléments du cache soient toujours conservés.</span><span class="sxs-lookup"><span data-stu-id="6c48c-208">Setting this attribute to `NeverRemove` doesn't guarantee that cached items are always retained.</span></span> <span data-ttu-id="6c48c-209">Pour plus d’informations, consultez les rubriques de la section [Ressources supplémentaires](#additional-resources).</span><span class="sxs-lookup"><span data-stu-id="6c48c-209">See the topics in the [Additional Resources](#additional-resources) section for more information.</span></span>

<span data-ttu-id="6c48c-210">Le Tag Helper Cache dépend du [service de cache en mémoire](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="6c48c-210">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="6c48c-211">Le Tag Helper Cache ajoute le service s’il ne l’a pas encore été.</span><span class="sxs-lookup"><span data-stu-id="6c48c-211">The Cache Tag Helper adds the service if it hasn't been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6c48c-212">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="6c48c-212">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
