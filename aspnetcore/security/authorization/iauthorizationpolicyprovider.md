---
title: Fournisseurs de stratégies d’autorisation personnalisés dans ASP.NET Core
author: mjrousos
description: Découvrez comment utiliser un IAuthorizationPolicyProvider personnalisé dans une application ASP.NET Core pour générer dynamiquement des stratégies d’autorisation.
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: b11f7f94e1042e65d5f2ff8ab0fe9ea7838bebeb
ms.sourcegitcommit: b1e480e1736b0fe0e4d8dce4a4cf5c8e47fc2101
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71108053"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a><span data-ttu-id="746e2-103">Fournisseurs de stratégies d’autorisation personnalisés utilisant IAuthorizationPolicyProvider dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="746e2-103">Custom Authorization Policy Providers using IAuthorizationPolicyProvider in ASP.NET Core</span></span> 

<span data-ttu-id="746e2-104">Par [Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="746e2-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="746e2-105">En général, lors de l’utilisation de l' [autorisation basée sur](xref:security/authorization/policies)la `AuthorizationOptions.AddPolicy` stratégie, les stratégies sont inscrites en appelant dans le cadre de la configuration du service d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="746e2-105">Typically when using [policy-based authorization](xref:security/authorization/policies), policies are registered by calling `AuthorizationOptions.AddPolicy` as part of authorization service configuration.</span></span> <span data-ttu-id="746e2-106">Dans certains scénarios, il n’est pas possible (ou souhaitable) d’inscrire toutes les stratégies d’autorisation de cette manière.</span><span class="sxs-lookup"><span data-stu-id="746e2-106">In some scenarios, it may not be possible (or desirable) to register all authorization policies in this way.</span></span> <span data-ttu-id="746e2-107">Dans ce cas, vous pouvez utiliser un personnalisé `IAuthorizationPolicyProvider` pour contrôler la façon dont les stratégies d’autorisation sont fournies.</span><span class="sxs-lookup"><span data-stu-id="746e2-107">In those cases, you can use a custom `IAuthorizationPolicyProvider` to control how authorization policies are supplied.</span></span>

<span data-ttu-id="746e2-108">Voici quelques exemples de scénarios dans lesquels un [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) personnalisé peut être utile :</span><span class="sxs-lookup"><span data-stu-id="746e2-108">Examples of scenarios where a custom [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) may be useful include:</span></span>

* <span data-ttu-id="746e2-109">Utilisation d’un service externe pour fournir une évaluation de la stratégie.</span><span class="sxs-lookup"><span data-stu-id="746e2-109">Using an external service to provide policy evaluation.</span></span>
* <span data-ttu-id="746e2-110">À l’aide d’un large éventail de stratégies (pour différents nombres de pièces ou âges, par exemple), il n’est pas judicieux d’ajouter chaque stratégie `AuthorizationOptions.AddPolicy` d’autorisation avec un appel.</span><span class="sxs-lookup"><span data-stu-id="746e2-110">Using a large range of policies (for different room numbers or ages, for example), so it doesn’t make sense to add each individual authorization policy with an `AuthorizationOptions.AddPolicy` call.</span></span>
* <span data-ttu-id="746e2-111">Création de stratégies au moment de l’exécution en fonction des informations contenues dans une source de données externe (par exemple, une base de données) ou détermination dynamique des exigences d’autorisation par le biais d’un autre mécanisme.</span><span class="sxs-lookup"><span data-stu-id="746e2-111">Creating policies at runtime based on information in an external data source (like a database) or determining authorization requirements dynamically through another mechanism.</span></span>

<span data-ttu-id="746e2-112">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider) à partir du [référentiel GitHub AspNetCore](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="746e2-112">[View or download sample code](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider) from the [AspNetCore GitHub repository](https://github.com/aspnet/AspNetCore).</span></span> <span data-ttu-id="746e2-113">Téléchargez le fichier ZIP du référentiel ASPNET/AspNetCore.</span><span class="sxs-lookup"><span data-stu-id="746e2-113">Download the aspnet/AspNetCore repository ZIP file.</span></span> <span data-ttu-id="746e2-114">Décompressez le fichier.</span><span class="sxs-lookup"><span data-stu-id="746e2-114">Unzip the file.</span></span> <span data-ttu-id="746e2-115">Accédez au dossier de projet *src/Security/Samples/CustomPolicyProvider* .</span><span class="sxs-lookup"><span data-stu-id="746e2-115">Navigate to the *src/Security/samples/CustomPolicyProvider* project folder.</span></span>

## <a name="customize-policy-retrieval"></a><span data-ttu-id="746e2-116">Personnaliser la récupération de stratégie</span><span class="sxs-lookup"><span data-stu-id="746e2-116">Customize policy retrieval</span></span>

<span data-ttu-id="746e2-117">Les applications ASP.net Core utilisent une implémentation de `IAuthorizationPolicyProvider` l’interface pour récupérer des stratégies d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="746e2-117">ASP.NET Core apps use an implementation of the `IAuthorizationPolicyProvider` interface to retrieve authorization policies.</span></span> <span data-ttu-id="746e2-118">Par défaut, [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) est inscrit et utilisé.</span><span class="sxs-lookup"><span data-stu-id="746e2-118">By default, [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) is registered and used.</span></span> <span data-ttu-id="746e2-119">`DefaultAuthorizationPolicyProvider`retourne des stratégies à `AuthorizationOptions` partir du fourni `IServiceCollection.AddAuthorization` dans un appel.</span><span class="sxs-lookup"><span data-stu-id="746e2-119">`DefaultAuthorizationPolicyProvider` returns policies from the `AuthorizationOptions` provided in an `IServiceCollection.AddAuthorization` call.</span></span>

<span data-ttu-id="746e2-120">Vous pouvez personnaliser ce comportement en inscrivant une `IAuthorizationPolicyProvider` implémentation différente dans le conteneur d' [injection de dépendances](xref:fundamentals/dependency-injection) de l’application.</span><span class="sxs-lookup"><span data-stu-id="746e2-120">You can customize this behavior by registering a different `IAuthorizationPolicyProvider` implementation in the app’s [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> 

<span data-ttu-id="746e2-121">L' `IAuthorizationPolicyProvider` interface contient deux API :</span><span class="sxs-lookup"><span data-stu-id="746e2-121">The `IAuthorizationPolicyProvider` interface contains two APIs:</span></span>

* <span data-ttu-id="746e2-122">[GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) retourne une stratégie d’autorisation pour un nom donné.</span><span class="sxs-lookup"><span data-stu-id="746e2-122">[GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) returns an authorization policy for a given name.</span></span>
* <span data-ttu-id="746e2-123">[GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) retourne la stratégie d’autorisation par défaut (la stratégie `[Authorize]` utilisée pour les attributs sans une stratégie spécifiée).</span><span class="sxs-lookup"><span data-stu-id="746e2-123">[GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) returns the default authorization policy (the policy used for `[Authorize]` attributes without a policy specified).</span></span> 

<span data-ttu-id="746e2-124">En implémentant ces deux API, vous pouvez personnaliser la façon dont les stratégies d’autorisation sont fournies.</span><span class="sxs-lookup"><span data-stu-id="746e2-124">By implementing these two APIs, you can customize how authorization policies are provided.</span></span>

## <a name="parameterized-authorize-attribute-example"></a><span data-ttu-id="746e2-125">Exemple d’attribut Authorize paramétrable</span><span class="sxs-lookup"><span data-stu-id="746e2-125">Parameterized authorize attribute example</span></span>

<span data-ttu-id="746e2-126">Un scénario où `IAuthorizationPolicyProvider` est utile est l’activation `[Authorize]` d’attributs personnalisés dont les spécifications dépendent d’un paramètre.</span><span class="sxs-lookup"><span data-stu-id="746e2-126">One scenario where `IAuthorizationPolicyProvider` is useful is enabling custom `[Authorize]` attributes whose requirements depend on a parameter.</span></span> <span data-ttu-id="746e2-127">Par exemple, dans la documentation [d’autorisation basée sur des stratégies](xref:security/authorization/policies) , une stratégie basée sur l’âge (« AtLeast21 ») a été utilisée comme exemple.</span><span class="sxs-lookup"><span data-stu-id="746e2-127">For example, in [policy-based authorization](xref:security/authorization/policies) documentation, an age-based (“AtLeast21”) policy was used as a sample.</span></span> <span data-ttu-id="746e2-128">Si des actions de contrôleur différentes dans une application doivent être mises à la disposition des utilisateurs de *différents* âges, il peut être utile de disposer de nombreuses stratégies d’âge différentes.</span><span class="sxs-lookup"><span data-stu-id="746e2-128">If different controller actions in an app should be made available to users of *different* ages, it might be useful to have many different age-based policies.</span></span> <span data-ttu-id="746e2-129">Au lieu d’inscrire toutes les autres stratégies basées sur l’âge nécessaires à `AuthorizationOptions`l’application, vous pouvez générer les stratégies de manière dynamique avec un personnalisé. `IAuthorizationPolicyProvider`</span><span class="sxs-lookup"><span data-stu-id="746e2-129">Instead of registering all the different age-based policies that the application will need in `AuthorizationOptions`, you can generate the policies dynamically with a custom `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="746e2-130">Pour faciliter l’utilisation des stratégies, vous pouvez annoter des actions avec un attribut `[MinimumAgeAuthorize(20)]`d’autorisation personnalisé comme.</span><span class="sxs-lookup"><span data-stu-id="746e2-130">To make using the policies easier, you can annotate actions with custom authorization attribute like `[MinimumAgeAuthorize(20)]`.</span></span>

## <a name="custom-authorization-attributes"></a><span data-ttu-id="746e2-131">Attributs d’autorisation personnalisés</span><span class="sxs-lookup"><span data-stu-id="746e2-131">Custom Authorization attributes</span></span>

<span data-ttu-id="746e2-132">Les stratégies d’autorisation sont identifiées par leur nom.</span><span class="sxs-lookup"><span data-stu-id="746e2-132">Authorization policies are identified by their names.</span></span> <span data-ttu-id="746e2-133">La procédure `MinimumAgeAuthorizeAttribute` personnalisée décrite précédemment doit mapper des arguments dans une chaîne qui peut être utilisée pour récupérer la stratégie d’autorisation correspondante.</span><span class="sxs-lookup"><span data-stu-id="746e2-133">The custom `MinimumAgeAuthorizeAttribute` described previously needs to map arguments into a string that can be used to retrieve the corresponding authorization policy.</span></span> <span data-ttu-id="746e2-134">Pour ce faire, vous pouvez dériver `AuthorizeAttribute` de et rendre `Age` la propriété encapsulée dans la `AuthorizeAttribute.Policy` propriété.</span><span class="sxs-lookup"><span data-stu-id="746e2-134">You can do this by deriving from `AuthorizeAttribute` and making the `Age` property wrap the `AuthorizeAttribute.Policy` property.</span></span>

```csharp
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

<span data-ttu-id="746e2-135">Ce type d’attribut a `Policy` une chaîne basée sur le préfixe codé en`"MinimumAge"`dur () et un entier passé par le biais du constructeur.</span><span class="sxs-lookup"><span data-stu-id="746e2-135">This attribute type has a `Policy` string based on the hard-coded prefix (`"MinimumAge"`) and an integer passed in via the constructor.</span></span>

<span data-ttu-id="746e2-136">Vous pouvez l’appliquer aux actions de la même façon que d' `Authorize` autres attributs, sauf qu’il prend un entier comme paramètre.</span><span class="sxs-lookup"><span data-stu-id="746e2-136">You can apply it to actions in the same way as other `Authorize` attributes except that it takes an integer as a parameter.</span></span>

```csharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a><span data-ttu-id="746e2-137">IAuthorizationPolicyProvider personnalisé</span><span class="sxs-lookup"><span data-stu-id="746e2-137">Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="746e2-138">Le personnalisé `MinimumAgeAuthorizeAttribute` simplifie la demande de stratégies d’autorisation pour tout âge minimal souhaité.</span><span class="sxs-lookup"><span data-stu-id="746e2-138">The custom `MinimumAgeAuthorizeAttribute` makes it easy to request authorization policies for any minimum age desired.</span></span> <span data-ttu-id="746e2-139">Le prochain problème à résoudre consiste à s’assurer que les stratégies d’autorisation sont disponibles pour tous ces âges.</span><span class="sxs-lookup"><span data-stu-id="746e2-139">The next problem to solve is making sure that authorization policies are available for all of those different ages.</span></span> <span data-ttu-id="746e2-140">C’est là qu' `IAuthorizationPolicyProvider` un est utile.</span><span class="sxs-lookup"><span data-stu-id="746e2-140">This is where an `IAuthorizationPolicyProvider` is useful.</span></span>

<span data-ttu-id="746e2-141">Lorsque vous `MinimumAgeAuthorizationAttribute`utilisez, les noms de stratégie d’autorisation suivent `"MinimumAge" + Age`le modèle, de `IAuthorizationPolicyProvider` sorte que le personnalisé doit générer des stratégies d’autorisation en :</span><span class="sxs-lookup"><span data-stu-id="746e2-141">When using `MinimumAgeAuthorizationAttribute`, the authorization policy names will follow the pattern `"MinimumAge" + Age`, so the custom `IAuthorizationPolicyProvider` should generate authorization policies by:</span></span>

* <span data-ttu-id="746e2-142">Analyse de l’âge à partir du nom de la stratégie.</span><span class="sxs-lookup"><span data-stu-id="746e2-142">Parsing the age from the policy name.</span></span>
* <span data-ttu-id="746e2-143">Utilisation `AuthorizationPolicyBuilder` de pour créer un nouveau`AuthorizationPolicy`</span><span class="sxs-lookup"><span data-stu-id="746e2-143">Using `AuthorizationPolicyBuilder` to create a new `AuthorizationPolicy`</span></span>
* <span data-ttu-id="746e2-144">Dans cet exemple et les exemples suivants, on suppose que l’utilisateur est authentifié via un cookie.</span><span class="sxs-lookup"><span data-stu-id="746e2-144">In this and following examples it will be assumed that the user is authenticated via a cookie.</span></span> <span data-ttu-id="746e2-145">`AuthorizationPolicyBuilder` Doit être construit avec au moins un nom de schéma d’autorisation, ou toujours correctement.</span><span class="sxs-lookup"><span data-stu-id="746e2-145">The `AuthorizationPolicyBuilder` should either be constructed with at least one authorization scheme name or always succeed.</span></span> <span data-ttu-id="746e2-146">Dans le cas contraire, il n’y a pas d’informations sur la façon de fournir une stimulation à l’utilisateur et une exception sera levée.</span><span class="sxs-lookup"><span data-stu-id="746e2-146">Otherwise there is no information on how to provide a challenge to the user and an exception will be thrown.</span></span>
* <span data-ttu-id="746e2-147">Ajout de spécifications à la stratégie en fonction de l' `AuthorizationPolicyBuilder.AddRequirements`âge avec.</span><span class="sxs-lookup"><span data-stu-id="746e2-147">Adding requirements to the policy based on the age with `AuthorizationPolicyBuilder.AddRequirements`.</span></span> <span data-ttu-id="746e2-148">Dans d’autres scénarios, vous pouvez `RequireClaim`utiliser `RequireRole`, ou `RequireUserName` à la place.</span><span class="sxs-lookup"><span data-stu-id="746e2-148">In other scenarios, you might use `RequireClaim`, `RequireRole`, or `RequireUserName` instead.</span></span>

```csharp
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
            var policy = new AuthorizationPolicyBuilder(CookieAuthenticationDefaults.AuthenticationScheme);
            policy.AddRequirements(new MinimumAgeRequirement(age));
            return Task.FromResult(policy.Build());
        }

        return Task.FromResult<AuthorizationPolicy>(null);
    }
}
```

## <a name="multiple-authorization-policy-providers"></a><span data-ttu-id="746e2-149">Plusieurs fournisseurs de stratégies d’autorisation</span><span class="sxs-lookup"><span data-stu-id="746e2-149">Multiple authorization policy providers</span></span>

<span data-ttu-id="746e2-150">Lorsque vous utilisez `IAuthorizationPolicyProvider` des implémentations personnalisées, gardez à l’esprit que ASP.net Core utilise `IAuthorizationPolicyProvider`uniquement une instance de.</span><span class="sxs-lookup"><span data-stu-id="746e2-150">When using custom `IAuthorizationPolicyProvider` implementations, keep in mind that ASP.NET Core only uses one instance of `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="746e2-151">Si un fournisseur personnalisé n’est pas en mesure de fournir des stratégies d’autorisation pour tous les noms de stratégie qui seront utilisés, il doit revenir à un fournisseur de sauvegarde.</span><span class="sxs-lookup"><span data-stu-id="746e2-151">If a custom provider isn't able to provide authorization policies for all policy names that will be used, it should fall back to a backup provider.</span></span> 

<span data-ttu-id="746e2-152">Par exemple, considérez une application qui nécessite à la fois des stratégies d’âge personnalisées et une récupération de stratégie basée sur les rôles plus classique.</span><span class="sxs-lookup"><span data-stu-id="746e2-152">For example, consider an application that needs both custom age policies and more traditional role-based policy retrieval.</span></span> <span data-ttu-id="746e2-153">Une telle application peut utiliser un fournisseur de stratégie d’autorisation personnalisé qui :</span><span class="sxs-lookup"><span data-stu-id="746e2-153">Such an app could use a custom authorization policy provider that:</span></span>

* <span data-ttu-id="746e2-154">Tentatives d’analyse des noms de stratégie.</span><span class="sxs-lookup"><span data-stu-id="746e2-154">Attempts to parse policy names.</span></span> 
* <span data-ttu-id="746e2-155">Appelle un fournisseur de stratégie différent (par `DefaultAuthorizationPolicyProvider`exemple) si le nom de la stratégie ne contient pas d’ancienneté.</span><span class="sxs-lookup"><span data-stu-id="746e2-155">Calls into a different policy provider (like `DefaultAuthorizationPolicyProvider`) if the policy name doesn't contain an age.</span></span>

<span data-ttu-id="746e2-156">L’exemple `IAuthorizationPolicyProvider` d’implémentation illustré ci-dessus peut être mis `DefaultAuthorizationPolicyProvider` à jour pour utiliser le en créant un fournisseur de stratégie de secours dans son constructeur (à utiliser si le nom de la stratégie ne correspond pas au modèle attendu « minimum » + Age).</span><span class="sxs-lookup"><span data-stu-id="746e2-156">The example `IAuthorizationPolicyProvider` implementation shown above can be updated to use the `DefaultAuthorizationPolicyProvider` by creating a fallback policy provider in its constructor (to be used in case the policy name doesn't match its expected pattern of 'MinimumAge' + age).</span></span>

```csharp
private DefaultAuthorizationPolicyProvider FallbackPolicyProvider { get; }

public MinimumAgePolicyProvider(IOptions<AuthorizationOptions> options)
{
    // ASP.NET Core only uses one authorization policy provider, so if the custom implementation
    // doesn't handle all policies it should fall back to an alternate provider.
    FallbackPolicyProvider = new DefaultAuthorizationPolicyProvider(options);
}
```

<span data-ttu-id="746e2-157">La `GetPolicyAsync` méthode peut ensuite être mise à jour pour utiliser `FallbackPolicyProvider` le au lieu de retourner la valeur NULL :</span><span class="sxs-lookup"><span data-stu-id="746e2-157">Then, the `GetPolicyAsync` method can be updated to use the `FallbackPolicyProvider` instead of returning null:</span></span>

```csharp
...
return FallbackPolicyProvider.GetPolicyAsync(policyName);
```

## <a name="default-policy"></a><span data-ttu-id="746e2-158">Stratégie par défaut</span><span class="sxs-lookup"><span data-stu-id="746e2-158">Default policy</span></span>

<span data-ttu-id="746e2-159">En plus de fournir des stratégies d’autorisation nommées `IAuthorizationPolicyProvider` , un personnalisé `GetDefaultPolicyAsync` doit implémenter pour fournir une `[Authorize]` stratégie d’autorisation pour les attributs sans nom de stratégie spécifié.</span><span class="sxs-lookup"><span data-stu-id="746e2-159">In addition to providing named authorization policies, a custom `IAuthorizationPolicyProvider` needs to implement `GetDefaultPolicyAsync` to provide an authorization policy for `[Authorize]` attributes without a policy name specified.</span></span>

<span data-ttu-id="746e2-160">Dans de nombreux cas, cet attribut d’autorisation nécessite uniquement un utilisateur authentifié, ce qui vous permet d’effectuer la stratégie nécessaire avec `RequireAuthenticatedUser`un appel à :</span><span class="sxs-lookup"><span data-stu-id="746e2-160">In many cases, this authorization attribute only requires an authenticated user, so you can make the necessary policy with a call to `RequireAuthenticatedUser`:</span></span>

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder(CookieAuthenticationDefaults.AuthenticationScheme).RequireAuthenticatedUser().Build());
```

<span data-ttu-id="746e2-161">Comme pour tous les aspects d’un `IAuthorizationPolicyProvider`personnalisé, vous pouvez le personnaliser en fonction des besoins.</span><span class="sxs-lookup"><span data-stu-id="746e2-161">As with all aspects of a custom `IAuthorizationPolicyProvider`, you can customize this, as needed.</span></span> <span data-ttu-id="746e2-162">Dans certains cas, il peut être souhaitable de récupérer la stratégie par défaut à partir `IAuthorizationPolicyProvider`d’un secours.</span><span class="sxs-lookup"><span data-stu-id="746e2-162">In some cases, it may be desirable to retrieve the default policy from a fallback `IAuthorizationPolicyProvider`.</span></span>

## <a name="required-policy"></a><span data-ttu-id="746e2-163">Stratégie requise</span><span class="sxs-lookup"><span data-stu-id="746e2-163">Required policy</span></span>

<span data-ttu-id="746e2-164">Un personnalisé `IAuthorizationPolicyProvider` doit implémenter `GetRequiredPolicyAsync` pour, éventuellement, fournir une stratégie qui est toujours requise.</span><span class="sxs-lookup"><span data-stu-id="746e2-164">A custom `IAuthorizationPolicyProvider` needs to implement `GetRequiredPolicyAsync` to, optionally, provide a policy that is always required.</span></span> <span data-ttu-id="746e2-165">Si `GetRequiredPolicyAsync` retourne une stratégie non null, cette stratégie est associée à toute autre stratégie (nommée ou par défaut) qui est demandée.</span><span class="sxs-lookup"><span data-stu-id="746e2-165">If `GetRequiredPolicyAsync` returns a non-null policy, that policy will be combined with any other (named or default) policy that is requested.</span></span>

<span data-ttu-id="746e2-166">Si aucune stratégie n’est requise, le fournisseur peut simplement retourner la valeur null ou se reporter au fournisseur de secours :</span><span class="sxs-lookup"><span data-stu-id="746e2-166">If no required policy is needed, the provider can just return null or defer to the fallback provider:</span></span>

```csharp
public Task<AuthorizationPolicy> GetRequiredPolicyAsync() => 
    Task.FromResult<AuthorizationPolicy>(null);
```

## <a name="use-a-custom-iauthorizationpolicyprovider"></a><span data-ttu-id="746e2-167">Utiliser un IAuthorizationPolicyProvider personnalisé</span><span class="sxs-lookup"><span data-stu-id="746e2-167">Use a custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="746e2-168">Pour utiliser des stratégies personnalisées `IAuthorizationPolicyProvider`à partir d’un, vous devez :</span><span class="sxs-lookup"><span data-stu-id="746e2-168">To use custom policies from an `IAuthorizationPolicyProvider`, you must:</span></span>

* <span data-ttu-id="746e2-169">Inscrire les types `AuthorizationHandler` appropriés avec l’injection de dépendances (décrite dans [autorisation basée sur la stratégie](xref:security/authorization/policies#authorization-handlers)), comme avec tous les scénarios d’autorisation basés sur des stratégies.</span><span class="sxs-lookup"><span data-stu-id="746e2-169">Register the appropriate `AuthorizationHandler` types with dependency injection (described in [policy-based authorization](xref:security/authorization/policies#authorization-handlers)), as with all policy-based authorization scenarios.</span></span>
* <span data-ttu-id="746e2-170">Inscrivez le type `IAuthorizationPolicyProvider` personnalisé dans la collection de services d’injection de dépendances `Startup.ConfigureServices`de l’application (dans) pour remplacer le fournisseur de stratégie par défaut.</span><span class="sxs-lookup"><span data-stu-id="746e2-170">Register the custom `IAuthorizationPolicyProvider` type in the app's dependency injection service collection (in `Startup.ConfigureServices`) to replace the default policy provider.</span></span>

```csharp
services.AddSingleton<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

<span data-ttu-id="746e2-171">Un exemple personnalisé `IAuthorizationPolicyProvider` complet est disponible dans le [référentiel GitHub ASPNET/AuthSamples](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider).</span><span class="sxs-lookup"><span data-stu-id="746e2-171">A complete custom `IAuthorizationPolicyProvider` sample is available in the [aspnet/AuthSamples GitHub repository](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider).</span></span>
