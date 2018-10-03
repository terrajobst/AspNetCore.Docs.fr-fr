---
title: Fournisseurs de stratégie d’autorisation personnalisée dans ASP.NET Core
author: mjrousos
description: Découvrez comment utiliser un IAuthorizationPolicyProvider personnalisé dans une application ASP.NET Core pour générer dynamiquement des stratégies d’autorisation.
ms.author: riande
ms.custom: mvc
ms.date: 05/02/2018
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: fdd8f9232c4332aa8307b9dbdfba6af48dfafa72
ms.sourcegitcommit: 317f9be24db600499e79d25872d743af74bd86c0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/02/2018
ms.locfileid: "48045495"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a><span data-ttu-id="1e7b2-103">Fournisseurs de stratégie d’autorisation personnalisés à l’aide de IAuthorizationPolicyProvider dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1e7b2-103">Custom Authorization Policy Providers using IAuthorizationPolicyProvider in ASP.NET Core</span></span> 

<span data-ttu-id="1e7b2-104">Par [Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="1e7b2-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="1e7b2-105">En général, lorsque vous utilisez [autorisation basée sur la stratégie](xref:security/authorization/policies), stratégies sont inscrits en appelant `AuthorizationOptions.AddPolicy` dans le cadre de la configuration du service d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-105">Typically when using [policy-based authorization](xref:security/authorization/policies), policies are registered by calling `AuthorizationOptions.AddPolicy` as part of authorization service configuration.</span></span> <span data-ttu-id="1e7b2-106">Dans certains scénarios, il peut être pas possible (ou souhaitable) pour enregistrer toutes les stratégies d’autorisation de cette façon.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-106">In some scenarios, it may not be possible (or desirable) to register all authorization policies in this way.</span></span> <span data-ttu-id="1e7b2-107">Dans ce cas, vous pouvez utiliser un personnalisé `IAuthorizationPolicyProvider` pour contrôler la façon dont les stratégies d’autorisation sont fournis.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-107">In those cases, you can use a custom `IAuthorizationPolicyProvider` to control how authorization policies are supplied.</span></span>

<span data-ttu-id="1e7b2-108">Exemples de scénarios où un personnalisé [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) peuvent être utiles incluent :</span><span class="sxs-lookup"><span data-stu-id="1e7b2-108">Examples of scenarios where a custom [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) may be useful include:</span></span>

* <span data-ttu-id="1e7b2-109">À l’aide d’un service externe pour fournir l’évaluation de la stratégie.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-109">Using an external service to provide policy evaluation.</span></span>
* <span data-ttu-id="1e7b2-110">À l’aide d’une grande plage de stratégies (pour les nombres de pièce ou âges, par exemple), par conséquent, il est inutile pour ajouter chaque stratégie d’autorisation individuels avec une `AuthorizationOptions.AddPolicy` appeler.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-110">Using a large range of policies (for different room numbers or ages, for example), so it doesn’t make sense to add each individual authorization policy with an `AuthorizationOptions.AddPolicy` call.</span></span>
* <span data-ttu-id="1e7b2-111">Création de stratégies lors de l’exécution en fonction des informations dans une source de données externe (par exemple, une base de données) ou la détermination des exigences d’autorisation dynamiquement via un autre mécanisme.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-111">Creating policies at runtime based on information in an external data source (like a database) or determining authorization requirements dynamically through another mechanism.</span></span>

<span data-ttu-id="1e7b2-112">[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/AuthSamples/tree/master/samples/CustomPolicyProvider) à partir de la [référentiel aspnet/AuthSamples](https://github.com/aspnet/AuthSamples).</span><span class="sxs-lookup"><span data-stu-id="1e7b2-112">[View or download sample code](https://github.com/aspnet/AuthSamples/tree/master/samples/CustomPolicyProvider) from the [aspnet/AuthSamples GitHub repository](https://github.com/aspnet/AuthSamples).</span></span> <span data-ttu-id="1e7b2-113">Téléchargez le fichier ZIP d’aspnet/AuthSamples référentiel.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-113">Download the aspnet/AuthSamples repository ZIP file.</span></span>
<span data-ttu-id="1e7b2-114">Décompressez le *AuthSamples-master.zip* fichier.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-114">Unzip the *AuthSamples-master.zip* file.</span></span> <span data-ttu-id="1e7b2-115">Accédez à la *exemples/CustomPolicyProvider* dossier du projet.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-115">Navigate to the *samples/CustomPolicyProvider* project folder.</span></span>

## <a name="customize-policy-retrieval"></a><span data-ttu-id="1e7b2-116">Personnaliser la récupération de stratégie</span><span class="sxs-lookup"><span data-stu-id="1e7b2-116">Customize policy retrieval</span></span>

<span data-ttu-id="1e7b2-117">Les applications ASP.NET Core utilisent une implémentation de la `IAuthorizationPolicyProvider` interface pour récupérer les stratégies d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-117">ASP.NET Core apps use an implementation of the `IAuthorizationPolicyProvider` interface to retrieve authorization policies.</span></span> <span data-ttu-id="1e7b2-118">Par défaut, [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) est enregistré et utilisé.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-118">By default, [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) is registered and used.</span></span> <span data-ttu-id="1e7b2-119">`DefaultAuthorizationPolicyProvider` Retourne les stratégies à partir de la `AuthorizationOptions` fourni dans un `IServiceCollection.AddAuthorization` appeler.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-119">`DefaultAuthorizationPolicyProvider` returns policies from the `AuthorizationOptions` provided in an `IServiceCollection.AddAuthorization` call.</span></span>

<span data-ttu-id="1e7b2-120">Vous pouvez personnaliser ce comportement en enregistrant une autre `IAuthorizationPolicyProvider` implémentation de l’application [l’injection de dépendances](xref:fundamentals/dependency-injection) conteneur.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-120">You can customize this behavior by registering a different `IAuthorizationPolicyProvider` implementation in the app’s [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> 

<span data-ttu-id="1e7b2-121">Le `IAuthorizationPolicyProvider` interface contient deux API :</span><span class="sxs-lookup"><span data-stu-id="1e7b2-121">The `IAuthorizationPolicyProvider` interface contains two APIs:</span></span>

* <span data-ttu-id="1e7b2-122">[GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) renvoie une stratégie d’autorisation pour un nom donné.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-122">[GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) returns an authorization policy for a given name.</span></span>
* <span data-ttu-id="1e7b2-123">[GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) renvoie la stratégie d’autorisation par défaut (la stratégie utilisée pour `[Authorize]` attributs sans une stratégie spécifiée).</span><span class="sxs-lookup"><span data-stu-id="1e7b2-123">[GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) returns the default authorization policy (the policy used for `[Authorize]` attributes without a policy specified).</span></span> 

<span data-ttu-id="1e7b2-124">En implémentant ces deux API, vous pouvez personnaliser la façon dont les stratégies d’autorisation sont fournies.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-124">By implementing these two APIs, you can customize how authorization policies are provided.</span></span>

## <a name="parameterized-authorize-attribute-example"></a><span data-ttu-id="1e7b2-125">Paramétrables autoriser l’exemple d’attribut</span><span class="sxs-lookup"><span data-stu-id="1e7b2-125">Parameterized authorize attribute example</span></span>

<span data-ttu-id="1e7b2-126">Un scénario où `IAuthorizationPolicyProvider` est utile est l’activation personnalisé `[Authorize]` attributs dont les exigences dépendent d’un paramètre.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-126">One scenario where `IAuthorizationPolicyProvider` is useful is enabling custom `[Authorize]` attributes whose requirements depend on a parameter.</span></span> <span data-ttu-id="1e7b2-127">Par exemple, dans [autorisation basée sur la stratégie](xref:security/authorization/policies) documentation, une tranche d’âge (« AtLeast21 ») stratégie a été utilisée en tant qu’exemple.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-127">For example, in [policy-based authorization](xref:security/authorization/policies) documentation, an age-based (“AtLeast21”) policy was used as a sample.</span></span> <span data-ttu-id="1e7b2-128">Si les actions de contrôleur différent dans une application doivent être accessibles aux utilisateurs de *différents* âges, il peut être utile d’avoir de nombreuses stratégies différentes de tranche d’âge.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-128">If different controller actions in an app should be made available to users of *different* ages, it might be useful to have many different age-based policies.</span></span> <span data-ttu-id="1e7b2-129">Au lieu d’inscrire toutes les différentes tranche d’âge stratégies que l’application devra dans `AuthorizationOptions`, vous pouvez générer les stratégies dynamiquement avec un personnalisé `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-129">Instead of registering all the different age-based policies that the application will need in `AuthorizationOptions`, you can generate the policies dynamically with a custom `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="1e7b2-130">Pour rendre à l’aide de stratégies plus facile, vous pouvez annoter des actions avec l’attribut d’autorisation personnalisé comme `[MinimumAgeAuthorize(20)]`.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-130">To make using the policies easier, you can annotate actions with custom authorization attribute like `[MinimumAgeAuthorize(20)]`.</span></span>

## <a name="custom-authorization-attributes"></a><span data-ttu-id="1e7b2-131">Attributs d’autorisation personnalisée</span><span class="sxs-lookup"><span data-stu-id="1e7b2-131">Custom Authorization Attributes</span></span>

<span data-ttu-id="1e7b2-132">Stratégies d’autorisation sont identifiés par leurs noms.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-132">Authorization policies are identified by their names.</span></span> <span data-ttu-id="1e7b2-133">Personnalisé `MinimumAgeAuthorizeAttribute` décrit précédemment doit mettre en correspondance d’arguments dans une chaîne qui peut être utilisée pour récupérer la stratégie d’autorisation correspondant.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-133">The custom `MinimumAgeAuthorizeAttribute` described previously needs to map arguments into a string that can be used to retrieve the corresponding authorization policy.</span></span> <span data-ttu-id="1e7b2-134">Ce faire, vous pouvez dérivant `AuthorizeAttribute` et en rendant le `Age` habillage de la propriété le `AuthorizeAttribute.Policy` propriété.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-134">You can do this by deriving from `AuthorizeAttribute` and making the `Age` property wrap the `AuthorizeAttribute.Policy` property.</span></span>

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

<span data-ttu-id="1e7b2-135">Ce type d’attribut a un `Policy` chaîne basée sur le préfixe codées en dur (`"MinimumAge"`) et un entier transmis via le constructeur.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-135">This attribute type has a `Policy` string based on the hard-coded prefix (`"MinimumAge"`) and an integer passed in via the constructor.</span></span>

<span data-ttu-id="1e7b2-136">Vous pouvez l’appliquer aux actions de la même façon que les autres `Authorize` attributs, à ceci près qu’elle prend un entier en tant que paramètre.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-136">You can apply it to actions in the same way as other `Authorize` attributes except that it takes an integer as a parameter.</span></span>

```csharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a><span data-ttu-id="1e7b2-137">IAuthorizationPolicyProvider personnalisé</span><span class="sxs-lookup"><span data-stu-id="1e7b2-137">Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="1e7b2-138">Personnalisé `MinimumAgeAuthorizeAttribute` facilite pour les stratégies de demande d’autorisation pour n’importe quel âge minimal souhaité.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-138">The custom `MinimumAgeAuthorizeAttribute` makes it easy to request authorization policies for any minimum age desired.</span></span> <span data-ttu-id="1e7b2-139">Le problème suivant à résoudre consiste à s’assurer que les stratégies d’autorisation sont disponibles pour l’ensemble de ces différentes tranches d’âge.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-139">The next problem to solve is making sure that authorization policies are available for all of those different ages.</span></span> <span data-ttu-id="1e7b2-140">C’est là une `IAuthorizationPolicyProvider` est utile.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-140">This is where an `IAuthorizationPolicyProvider` is useful.</span></span>

<span data-ttu-id="1e7b2-141">Lorsque vous utilisez `MinimumAgeAuthorizationAttribute`, les noms de la stratégie d’autorisation suivra le modèle `"MinimumAge" + Age`, de sorte que personnalisé `IAuthorizationPolicyProvider` doit générer des stratégies d’autorisation par :</span><span class="sxs-lookup"><span data-stu-id="1e7b2-141">When using `MinimumAgeAuthorizationAttribute`, the authorization policy names will follow the pattern `"MinimumAge" + Age`, so the custom `IAuthorizationPolicyProvider` should generate authorization policies by:</span></span>

* <span data-ttu-id="1e7b2-142">L’analyse de l’âge à partir du nom de la stratégie.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-142">Parsing the age from the policy name.</span></span>
* <span data-ttu-id="1e7b2-143">À l’aide de `AuthorizationPolicyBuilder` pour créer un nouveau `AuthorizationPolicy`</span><span class="sxs-lookup"><span data-stu-id="1e7b2-143">Using `AuthorizationPolicyBuilder` to create a new `AuthorizationPolicy`</span></span>
* <span data-ttu-id="1e7b2-144">Ajouter des exigences à la stratégie selon l’âge avec `AuthorizationPolicyBuilder.AddRequirements`.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-144">Adding requirements to the policy based on the age with `AuthorizationPolicyBuilder.AddRequirements`.</span></span> <span data-ttu-id="1e7b2-145">Dans d’autres scénarios, vous pouvez utiliser `RequireClaim`, `RequireRole`, ou `RequireUserName` à la place.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-145">In other scenarios, you might use `RequireClaim`, `RequireRole`, or `RequireUserName` instead.</span></span>

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
            var policy = new AuthorizationPolicyBuilder();
            policy.AddRequirements(new MinimumAgeRequirement(age));
            return Task.FromResult(policy.Build());
        }

        return Task.FromResult<AuthorizationPolicy>(null);
    }
}
```

## <a name="multiple-authorization-policy-providers"></a><span data-ttu-id="1e7b2-146">Plusieurs fournisseurs de stratégie d’autorisation</span><span class="sxs-lookup"><span data-stu-id="1e7b2-146">Multiple authorization policy providers</span></span>

<span data-ttu-id="1e7b2-147">Lorsque vous utilisez personnalisé `IAuthorizationPolicyProvider` implémentations, n’oubliez pas que ASP.NET Core utilise uniquement une seule instance de `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-147">When using custom `IAuthorizationPolicyProvider` implementations, keep in mind that ASP.NET Core only uses one instance of `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="1e7b2-148">Si un fournisseur personnalisé n’est pas en mesure de fournir des stratégies d’autorisation pour tous les noms de stratégie, il doit revenir à un fournisseur de sauvegarde.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-148">If a custom provider isn't able to provide authorization policies for all policy names, it should fall back to a backup provider.</span></span> <span data-ttu-id="1e7b2-149">Les noms de stratégie, citons ceux qui proviennent d’une stratégie par défaut pour `[Authorize]` attributs sans nom.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-149">Policy names might include those that come from a default policy for `[Authorize]` attributes without a name.</span></span>

<span data-ttu-id="1e7b2-150">Par exemple, considérez qu'une application nécessaire les stratégies de vieillissement personnalisé et l’extraction de stratégie en fonction du rôle plus traditionnelle.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-150">For example, consider an application needed both custom age policies and more traditional role-based policy retrieval.</span></span> <span data-ttu-id="1e7b2-151">Une telle application peut utiliser un fournisseur de stratégie d’autorisation personnalisée qui :</span><span class="sxs-lookup"><span data-stu-id="1e7b2-151">Such an app could use a custom authorization policy provider that:</span></span>

* <span data-ttu-id="1e7b2-152">Tente d’analyser les noms de stratégie.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-152">Attempts to parse policy names.</span></span> 
* <span data-ttu-id="1e7b2-153">Appelle un fournisseur de stratégie différent (comme `DefaultAuthorizationPolicyProvider`) si le nom de la stratégie ne contient pas d’âge.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-153">Calls into a different policy provider (like `DefaultAuthorizationPolicyProvider`) if the policy name doesn't contain an age.</span></span>

## <a name="default-policy"></a><span data-ttu-id="1e7b2-154">Stratégie par défaut</span><span class="sxs-lookup"><span data-stu-id="1e7b2-154">Default policy</span></span>

<span data-ttu-id="1e7b2-155">En plus de fournir des stratégies d’autorisation nommés, personnalisé `IAuthorizationPolicyProvider` doit implémenter `GetDefaultPolicyAsync` pour fournir une stratégie d’autorisation pour `[Authorize]` attributs sans un nom de la stratégie spécifiée.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-155">In addition to providing named authorization policies, a custom `IAuthorizationPolicyProvider` needs to implement `GetDefaultPolicyAsync` to provide an authorization policy for `[Authorize]` attributes without a policy name specified.</span></span>

<span data-ttu-id="1e7b2-156">Dans de nombreux cas, cet attribut d’autorisation requiert uniquement un utilisateur authentifié, vous pouvez effectuer la stratégie nécessaire avec un appel à `RequireAuthenticatedUser`:</span><span class="sxs-lookup"><span data-stu-id="1e7b2-156">In many cases, this authorization attribute only requires an authenticated user, so you can make the necessary policy with a call to `RequireAuthenticatedUser`:</span></span>

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

<span data-ttu-id="1e7b2-157">Comme avec tous les aspects de personnalisé `IAuthorizationPolicyProvider`, vous pouvez personnaliser ceci, en fonction des besoins.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-157">As with all aspects of a custom `IAuthorizationPolicyProvider`, you can customize this, as needed.</span></span> <span data-ttu-id="1e7b2-158">Dans certains cas :</span><span class="sxs-lookup"><span data-stu-id="1e7b2-158">In some cases:</span></span>

* <span data-ttu-id="1e7b2-159">Les stratégies d’autorisation par défaut ne peuvent pas être utilisés.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-159">Default authorization policies might not be used.</span></span>
* <span data-ttu-id="1e7b2-160">Récupération de la stratégie par défaut peut être déléguée à une procédure de secours `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-160">Retrieving the default policy can be delegated to a fallback `IAuthorizationPolicyProvider`.</span></span>

## <a name="use-a-custom-iauthorizationpolicyprovider"></a><span data-ttu-id="1e7b2-161">Utiliser un IAuthorizationPolicyProvider personnalisé</span><span class="sxs-lookup"><span data-stu-id="1e7b2-161">Use a custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="1e7b2-162">Pour utiliser des stratégies personnalisées à partir d’un `IAuthorizationPolicyProvider`, vous devez :</span><span class="sxs-lookup"><span data-stu-id="1e7b2-162">To use custom policies from an `IAuthorizationPolicyProvider`, you must:</span></span>

* <span data-ttu-id="1e7b2-163">Inscrire approprié `AuthorizationHandler` types avec l’injection de dépendance (décrit dans [autorisation basée sur la stratégie](xref:security/authorization/policies#authorization-handlers)), à l’instar de tous les scénarios d’autorisation basée sur la stratégie.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-163">Register the appropriate `AuthorizationHandler` types with dependency injection (described in [policy-based authorization](xref:security/authorization/policies#authorization-handlers)), as with all policy-based authorization scenarios.</span></span>
* <span data-ttu-id="1e7b2-164">Inscrire personnalisé `IAuthorizationPolicyProvider` type dans la collection de service de l’injection de dépendances de l’application (dans `Startup.ConfigureServices`) pour remplacer le fournisseur de stratégie par défaut.</span><span class="sxs-lookup"><span data-stu-id="1e7b2-164">Register the custom `IAuthorizationPolicyProvider` type in the app's dependency injection service collection (in `Startup.ConfigureServices`) to replace the default policy provider.</span></span>

```csharp
services.AddSingleton<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

<span data-ttu-id="1e7b2-165">Personnalisé complet `IAuthorizationPolicyProvider` exemple est disponible dans le [référentiel aspnet/AuthSamples](https://github.com/aspnet/AuthSamples/tree/master/samples/CustomPolicyProvider).</span><span class="sxs-lookup"><span data-stu-id="1e7b2-165">A complete custom `IAuthorizationPolicyProvider` sample is available in the [aspnet/AuthSamples GitHub repository](https://github.com/aspnet/AuthSamples/tree/master/samples/CustomPolicyProvider).</span></span>
