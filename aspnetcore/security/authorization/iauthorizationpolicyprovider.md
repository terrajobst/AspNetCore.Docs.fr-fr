---
title: Fournisseurs de stratégie d’autorisation personnalisée dans ASP.NET Core
author: mjrousos
description: Découvrez comment utiliser un IAuthorizationPolicyProvider personnalisé dans une application ASP.NET Core pour générer dynamiquement des stratégies d’autorisation.
ms.author: riande
ms.custom: mvc
ms.date: 05/02/2018
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: 524928a5b291e02556d11a762d86430a6dc94660
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277255"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a><span data-ttu-id="fb7f4-103">Fournisseurs de stratégie d’autorisation personnalisés à l’aide de IAuthorizationPolicyProvider dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fb7f4-103">Custom Authorization Policy Providers using IAuthorizationPolicyProvider in ASP.NET Core</span></span> 

<span data-ttu-id="fb7f4-104">Par [Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="fb7f4-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="fb7f4-105">En général, lors de l’utilisation [d’autorisation basée sur la stratégie](xref:security/authorization/policies), les stratégies sont enregistrés en appelant `AuthorizationOptions.AddPolicy` dans le cadre de la configuration du service d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-105">Typically when using [policy-based authorization](xref:security/authorization/policies), policies are registered by calling `AuthorizationOptions.AddPolicy` as part of authorization service configuration.</span></span> <span data-ttu-id="fb7f4-106">Dans certains scénarios, il ne peut pas être possible (ou souhaitable) pour enregistrer toutes les stratégies d’autorisation de cette façon.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-106">In some scenarios, it may not be possible (or desirable) to register all authorization policies in this way.</span></span> <span data-ttu-id="fb7f4-107">Dans ce cas, vous pouvez utiliser une personnalisée `IAuthorizationPolicyProvider` pour contrôler la façon dont les stratégies d’autorisation sont fournies.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-107">In those cases, you can use a custom `IAuthorizationPolicyProvider` to control how authorization policies are supplied.</span></span>

<span data-ttu-id="fb7f4-108">Exemples de scénarios où une personnalisée [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) peuvent être utiles incluent :</span><span class="sxs-lookup"><span data-stu-id="fb7f4-108">Examples of scenarios where a custom [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) may be useful include:</span></span>

* <span data-ttu-id="fb7f4-109">À l’aide d’un service externe pour fournir l’évaluation de la stratégie.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-109">Using an external service to provide policy evaluation.</span></span>
* <span data-ttu-id="fb7f4-110">À l’aide d’une grande plage de stratégies (pour les numéros de pièce ou ans, par exemple), par conséquent, il n’a aucune signification pour ajouter chaque stratégie d’autorisation individuels avec un `AuthorizationOptions.AddPolicy` appeler.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-110">Using a large range of policies (for different room numbers or ages, for example), so it doesn’t make sense to add each individual authorization policy with an `AuthorizationOptions.AddPolicy` call.</span></span>
* <span data-ttu-id="fb7f4-111">Création de stratégies lors de l’exécution en fonction des informations dans une source de données externe (par exemple, une base de données) ou déterminer dynamiquement les exigences d’autorisation via un autre mécanisme.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-111">Creating policies at runtime based on information in an external data source (like a database) or determining authorization requirements dynamically through another mechanism.</span></span>

## <a name="customizing-policy-retrieval"></a><span data-ttu-id="fb7f4-112">Récupération de la stratégie de personnalisation</span><span class="sxs-lookup"><span data-stu-id="fb7f4-112">Customizing policy retrieval</span></span>

<span data-ttu-id="fb7f4-113">Applications ASP.NET Core utilisent une implémentation de la `IAuthorizationPolicyProvider` interface pour récupérer les stratégies d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-113">ASP.NET Core apps use an implementation of the `IAuthorizationPolicyProvider` interface to retrieve authorization policies.</span></span> <span data-ttu-id="fb7f4-114">Par défaut, [DefaultAuthorizationPolicyProvider](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) est enregistré et utilisé.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-114">By default, [DefaultAuthorizationPolicyProvider](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) is registered and used.</span></span> <span data-ttu-id="fb7f4-115">`DefaultAuthorizationPolicyProvider` Retourne les stratégies à partir de la `AuthorizationOptions` fourni dans une `IServiceCollection.AddAuthorization` appeler.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-115">`DefaultAuthorizationPolicyProvider` returns policies from the `AuthorizationOptions` provided in an `IServiceCollection.AddAuthorization` call.</span></span>

<span data-ttu-id="fb7f4-116">Vous pouvez personnaliser ce comportement en enregistrant une autre `IAuthorizationPolicyProvider` mise en œuvre dans l’application [injection de dépendance](xref:fundamentals/dependency-injection) conteneur.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-116">You can customize this behavior by registering a different `IAuthorizationPolicyProvider` implementation in the app’s [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> 

<span data-ttu-id="fb7f4-117">Le `IAuthorizationPolicyProvider` interface contient deux API :</span><span class="sxs-lookup"><span data-stu-id="fb7f4-117">The `IAuthorizationPolicyProvider` interface contains two APIs:</span></span>

* <span data-ttu-id="fb7f4-118">[GetPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) renvoie une stratégie d’autorisation pour un nom donné.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-118">[GetPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) returns an authorization policy for a given name.</span></span>
* <span data-ttu-id="fb7f4-119">[GetDefaultPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync?view=aspnetcore-2.0) renvoie la stratégie d’autorisation par défaut (la stratégie utilisée pour `[Authorize]` attributs sans une stratégie spécifiée).</span><span class="sxs-lookup"><span data-stu-id="fb7f4-119">[GetDefaultPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync?view=aspnetcore-2.0) returns the default authorization policy (the policy used for `[Authorize]` attributes without a policy specified).</span></span> 

<span data-ttu-id="fb7f4-120">En implémentant ces deux API, vous pouvez personnaliser la manière dont les stratégies d’autorisation sont fournis.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-120">By implementing these two APIs, you can customize how authorization policies are provided.</span></span>

## <a name="parameterized-authorize-attribute-example"></a><span data-ttu-id="fb7f4-121">Paramétrable autoriser l’exemple d’attribut</span><span class="sxs-lookup"><span data-stu-id="fb7f4-121">Parameterized authorize attribute example</span></span>

<span data-ttu-id="fb7f4-122">Dans un scénario où `IAuthorizationPolicyProvider` est utile est de l’activation personnalisée `[Authorize]` attributs dont les spécifications dépendent d’un paramètre.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-122">One scenario where `IAuthorizationPolicyProvider` is useful is enabling custom `[Authorize]` attributes whose requirements depend on a parameter.</span></span> <span data-ttu-id="fb7f4-123">Par exemple, dans [d’autorisation basée sur la stratégie](xref:security/authorization/policies) documentation, une tranche d’âge (« AtLeast21 ») stratégie a été utilisée en tant qu’exemple.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-123">For example, in [policy-based authorization](xref:security/authorization/policies) documentation, an age-based (“AtLeast21”) policy was used as a sample.</span></span> <span data-ttu-id="fb7f4-124">Si les actions de contrôleur différent dans une application doivent être accessibles aux utilisateurs de *différents* âges, il peut être utile d’avoir de nombreuses stratégies autres tranche d’âge.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-124">If different controller actions in an app should be made available to users of *different* ages, it might be useful to have many different age-based policies.</span></span> <span data-ttu-id="fb7f4-125">Au lieu d’inscrire toutes les différentes tranche d’âge stratégies que l’application devra dans `AuthorizationOptions`, vous pouvez générer les stratégies dynamiquement avec un personnalisé `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-125">Instead of registering all the different age-based policies that the application will need in `AuthorizationOptions`, you can generate the policies dynamically with a custom `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="fb7f4-126">Pour rendre à l’aide des stratégies plus facile, vous pouvez annoter les actions avec l’attribut d’autorisation personnalisée comme `[MinimumAgeAuthorize(20)]`.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-126">To make using the policies easier, you can annotate actions with custom authorization attribute like `[MinimumAgeAuthorize(20)]`.</span></span>

## <a name="custom-authorization-attributes"></a><span data-ttu-id="fb7f4-127">Attributs d’autorisation personnalisée</span><span class="sxs-lookup"><span data-stu-id="fb7f4-127">Custom Authorization Attributes</span></span>

<span data-ttu-id="fb7f4-128">Stratégies d’autorisation sont identifiés par leurs noms.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-128">Authorization policies are identified by their names.</span></span> <span data-ttu-id="fb7f4-129">Personnalisée `MinimumAgeAuthorizeAttribute` décrite précédemment doit mettre en correspondance des arguments dans une chaîne qui peut être utilisée pour récupérer la stratégie d’autorisation correspondant.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-129">The custom `MinimumAgeAuthorizeAttribute` described previously needs to map arguments into a string that can be used to retrieve the corresponding authorization policy.</span></span> <span data-ttu-id="fb7f4-130">Ce faire, vous pouvez dérivant de `AuthorizeAttribute` et le `Age` retour à la propriété le `AuthorizeAttribute.Policy` propriété.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-130">You can do this by deriving from `AuthorizeAttribute` and making the `Age` property wrap the `AuthorizeAttribute.Policy` property.</span></span>

```CSharp
internal class MinimumAgeAuthorizeAttribute : AuthorizeAttribute
{
    const string POLICY_PREFIX = "MinimumAge";

    public MinimumAgeAuthorizeAttribute(int age) => Age = age;

    // Get or set the Age property by manipulating the underlying Policy property
    public int Age
    {
        get
        {
            if (int.TryParse(Policy.Substring(POLICY_PREFIX.Length), out var age))
            {
                return age;
            }
            return default(int);
        }
        set
        {
            Policy = $"{POLICY_PREFIX}{value.ToString()}";
        }
    }
}
```

<span data-ttu-id="fb7f4-131">Ce type d’attribut a un `Policy` chaîne basée sur le préfixe codée en dur (`"MinimumAge"`) et un entier transmis via le constructeur.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-131">This attribute type has a `Policy` string based on the hard-coded prefix (`"MinimumAge"`) and an integer passed in via the constructor.</span></span>

<span data-ttu-id="fb7f4-132">Vous pouvez l’appliquer aux actions de la même façon que d’autres `Authorize` des attributs, sauf qu’il accepte un entier en tant que paramètre.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-132">You can apply it to actions in the same way as other `Authorize` attributes except that it takes an integer as a parameter.</span></span>

```CSharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a><span data-ttu-id="fb7f4-133">IAuthorizationPolicyProvider personnalisé</span><span class="sxs-lookup"><span data-stu-id="fb7f4-133">Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="fb7f4-134">Personnalisé `MinimumAgeAuthorizeAttribute` facilite les stratégies de demande d’autorisation pour n’importe quel âge minimal que vous le souhaitez.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-134">The custom `MinimumAgeAuthorizeAttribute` makes it easy to request authorization policies for any minimum age desired.</span></span> <span data-ttu-id="fb7f4-135">Résoudre le problème suivant consiste à s’assurer que les stratégies d’autorisation sont disponibles pour toutes les différentes tranches d’âge.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-135">The next problem to solve is making sure that authorization policies are available for all of those different ages.</span></span> <span data-ttu-id="fb7f4-136">C’est là un `IAuthorizationPolicyProvider` est utile.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-136">This is where an `IAuthorizationPolicyProvider` is useful.</span></span>

<span data-ttu-id="fb7f4-137">Lorsque vous utilisez `MinimumAgeAuthorizationAttribute`, les noms de la stratégie d’autorisation suivront le modèle `"MinimumAge" + Age`, de sorte que personnalisé `IAuthorizationPolicyProvider` doit générer des stratégies d’autorisation par :</span><span class="sxs-lookup"><span data-stu-id="fb7f4-137">When using `MinimumAgeAuthorizationAttribute`, the authorization policy names will follow the pattern `"MinimumAge" + Age`, so the custom `IAuthorizationPolicyProvider` should generate authorization policies by:</span></span>

* <span data-ttu-id="fb7f4-138">L’analyse de la durée de vie à partir du nom de la stratégie.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-138">Parsing the age from the policy name.</span></span>
* <span data-ttu-id="fb7f4-139">À l’aide de `AuthorizationPolicyBuiler` pour créer un nouveau `AuthorizationPolicy`</span><span class="sxs-lookup"><span data-stu-id="fb7f4-139">Using `AuthorizationPolicyBuiler` to create a new `AuthorizationPolicy`</span></span>
* <span data-ttu-id="fb7f4-140">Ajout de conditions à la stratégie basée sur la durée de vie avec `AuthorizationPolicyBuilder.AddRequirements`.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-140">Adding requirements to the policy based on the age with `AuthorizationPolicyBuilder.AddRequirements`.</span></span> <span data-ttu-id="fb7f4-141">Dans d’autres scénarios, vous pouvez utiliser `RequireClaim`, `RequireRole`, ou `RequireUserName` à la place.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-141">In other scenarios, you might use `RequireClaim`, `RequireRole`, or `RequireUserName` instead.</span></span>

```CSharp
internal class MinimumAgePolicyProvider : IAuthorizationPolicyProvider
{
    const string POLICY_PREFIX = "MinimumAge";

    // Policies are looked up by string name, so expect 'parameters' (like age)
    // to be embedded in the policy names. This is abstracted away from developers
    // by the more strongly-typed attributes derived from AuthorizeAttribute
    // (like [MinimumAgeAuthorize()] in this sample)
    public Task<AuthorizationPolicy> GetPolicyAsync(string policyName)
    {
        if (policyName.StartsWith(POLICY_PREFIX, StringComparison.OrdinalIgnoreCase) &&
            int.TryParse(policyName.Substring(POLICY_PREFIX.Length), out var age))
        {
            var policy = new AuthorizationPolicyBuilder();
            policy.AddRequirements(new MinimumAgeRequirement(age));
            return Task.FromResult(policy.Build());
        }

        return Task.FromResult<AuthorizationPolicy>(null);
    }
}
```

## <a name="multiple-authorization-policy-providers"></a><span data-ttu-id="fb7f4-142">Plusieurs fournisseurs de stratégie d’autorisation</span><span class="sxs-lookup"><span data-stu-id="fb7f4-142">Multiple authorization policy providers</span></span>

<span data-ttu-id="fb7f4-143">Lorsque vous utilisez personnalisé `IAuthorizationPolicyProvider` implémentations, gardez à l’esprit que ASP.NET Core utilise uniquement une seule instance de `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-143">When using custom `IAuthorizationPolicyProvider` implementations, keep in mind that ASP.NET Core only uses one instance of `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="fb7f4-144">Si un fournisseur personnalisé n’est pas en mesure de fournir des stratégies d’autorisation pour tous les noms de stratégie, il doit revenir à un fournisseur de sauvegarde.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-144">If a custom provider isn't able to provide authorization policies for all policy names, it should fall back to a backup provider.</span></span> <span data-ttu-id="fb7f4-145">Les noms de stratégie peuvent inclure ceux qui proviennent d’une stratégie par défaut pour `[Authorize]` attributs sans nom.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-145">Policy names might include those that come from a default policy for `[Authorize]` attributes without a name.</span></span>

<span data-ttu-id="fb7f4-146">Par exemple, considérez qu'une application nécessaire à la fois les stratégies de vieillissement personnalisé et plus traditionnelle de récupération de stratégie basée sur le rôle.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-146">For example, consider an application needed both custom age policies and more traditional role-based policy retrieval.</span></span> <span data-ttu-id="fb7f4-147">Une telle application peut utiliser un fournisseur de stratégie d’autorisation personnalisée qui :</span><span class="sxs-lookup"><span data-stu-id="fb7f4-147">Such an app could use a custom authorization policy provider that:</span></span>

* <span data-ttu-id="fb7f4-148">Tentatives d’analyse des noms de la stratégie.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-148">Attempts to parse policy names.</span></span> 
* <span data-ttu-id="fb7f4-149">Appelle un fournisseur de stratégie différent (comme `DefaultAuthorizationPolicyProvider`) si le nom de la stratégie ne contient pas d’âge.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-149">Calls into a different policy provider (like `DefaultAuthorizationPolicyProvider`) if the policy name doesn't contain an age.</span></span>

## <a name="default-policy"></a><span data-ttu-id="fb7f4-150">Stratégie par défaut</span><span class="sxs-lookup"><span data-stu-id="fb7f4-150">Default policy</span></span>

<span data-ttu-id="fb7f4-151">En plus de fournir des stratégies d’autorisation nommée, personnalisé `IAuthorizationPolicyProvider` doit implémenter `GetDefaultPolicyAsync` pour fournir une stratégie d’autorisation pour `[Authorize]` attributs sans un nom de la stratégie spécifiée.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-151">In addition to providing named authorization policies, a custom `IAuthorizationPolicyProvider` needs to implement `GetDefaultPolicyAsync` to provide an authorization policy for `[Authorize]` attributes without a policy name specified.</span></span>

<span data-ttu-id="fb7f4-152">Dans de nombreux cas, cet attribut d’autorisation ne requiert qu’un utilisateur authentifié, afin de vérifier la stratégie requise par un appel à `RequireAuthenticatedUser`:</span><span class="sxs-lookup"><span data-stu-id="fb7f4-152">In many cases, this authorization attribute only requires an authenticated user, so you can make the necessary policy with a call to `RequireAuthenticatedUser`:</span></span>

```CSharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

<span data-ttu-id="fb7f4-153">Comme avec tous les aspects de personnalisé `IAuthorizationPolicyProvider`, vous pouvez personnaliser, si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-153">As with all aspects of a custom `IAuthorizationPolicyProvider`, you can customize this, as needed.</span></span> <span data-ttu-id="fb7f4-154">Dans certains cas :</span><span class="sxs-lookup"><span data-stu-id="fb7f4-154">In some cases:</span></span>

* <span data-ttu-id="fb7f4-155">Les stratégies d’autorisation par défaut ne peuvent pas être utilisés.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-155">Default authorization policies might not be used.</span></span>
* <span data-ttu-id="fb7f4-156">La récupération de la stratégie par défaut peut être déléguée à une stratégie de secours `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-156">Retrieving the default policy can be delegated to a fallback `IAuthorizationPolicyProvider`.</span></span>

## <a name="using-a-custom-iauthorizationpolicyprovider"></a><span data-ttu-id="fb7f4-157">À l’aide d’un IAuthorizationPolicyProvider personnalisé</span><span class="sxs-lookup"><span data-stu-id="fb7f4-157">Using a Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="fb7f4-158">Pour utiliser les stratégies personnalisées à partir d’un `IAuthorizationPolicyProvider`, vous devez :</span><span class="sxs-lookup"><span data-stu-id="fb7f4-158">To use custom policies from an `IAuthorizationPolicyProvider`, you must:</span></span>

* <span data-ttu-id="fb7f4-159">Inscrire les `AuthorizationHandler` types avec l’injection de dépendances (décrit dans [d’autorisation basée sur la stratégie](xref:security/authorization/policies#authorization-handlers)), comme avec tous les scénarios d’autorisation basée sur la stratégie.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-159">Register the appropriate `AuthorizationHandler` types with dependency injection (described in [policy-based authorization](xref:security/authorization/policies#authorization-handlers)), as with all policy-based authorization scenarios.</span></span>
* <span data-ttu-id="fb7f4-160">Inscrire personnalisé `IAuthorizationPolicyProvider` type dans la collection de service de d’injection de dépendances de l’application (dans `Startup.ConfigureServices`) pour remplacer le fournisseur de stratégie par défaut.</span><span class="sxs-lookup"><span data-stu-id="fb7f4-160">Register the custom `IAuthorizationPolicyProvider` type in the app's dependency injection service collection (in `Startup.ConfigureServices`) to replace the default policy provider.</span></span>

```CSharp
services.AddTransient<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

<span data-ttu-id="fb7f4-161">Personnalisé complet `IAuthorizationPolicyProvider` exemple est disponible dans le [référentiel GitHub d’aspnet/AuthSamples](https://github.com/aspnet/AuthSamples/tree/dev/samples/CustomPolicyProvider).</span><span class="sxs-lookup"><span data-stu-id="fb7f4-161">A complete custom `IAuthorizationPolicyProvider` sample is available in the [aspnet/AuthSamples GitHub repository](https://github.com/aspnet/AuthSamples/tree/dev/samples/CustomPolicyProvider).</span></span>
