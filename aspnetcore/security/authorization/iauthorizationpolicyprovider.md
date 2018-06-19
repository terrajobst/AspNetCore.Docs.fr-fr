---
title: Fournisseurs de stratégie d’autorisation personnalisée dans ASP.NET Core
author: mjrousos
description: Découvrez comment utiliser un IAuthorizationPolicyProvider personnalisé dans une application ASP.NET Core pour générer dynamiquement des stratégies d’autorisation.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/02/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: a5bad88b37d38b819b960b1eb27808d891268c01
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2018
ms.locfileid: "34233441"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a>Fournisseurs de stratégie d’autorisation personnalisés à l’aide de IAuthorizationPolicyProvider dans ASP.NET Core 

Par [Mike Rousos](https://github.com/mjrousos)

En général, lors de l’utilisation [d’autorisation basée sur la stratégie](xref:security/authorization/policies), les stratégies sont enregistrés en appelant `AuthorizationOptions.AddPolicy` dans le cadre de la configuration du service d’autorisation. Dans certains scénarios, il ne peut pas être possible (ou souhaitable) pour enregistrer toutes les stratégies d’autorisation de cette façon. Dans ce cas, vous pouvez utiliser une personnalisée `IAuthorizationPolicyProvider` pour contrôler la façon dont les stratégies d’autorisation sont fournies.

Exemples de scénarios où une personnalisée [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) peuvent être utiles incluent :

* À l’aide d’un service externe pour fournir l’évaluation de la stratégie.
* À l’aide d’une grande plage de stratégies (pour les numéros de pièce ou ans, par exemple), par conséquent, il n’a aucune signification pour ajouter chaque stratégie d’autorisation individuels avec un `AuthorizationOptions.AddPolicy` appeler.
* Création de stratégies lors de l’exécution en fonction des informations dans une source de données externe (par exemple, une base de données) ou déterminer dynamiquement les exigences d’autorisation via un autre mécanisme.

## <a name="customizing-policy-retrieval"></a>Récupération de la stratégie de personnalisation

Applications ASP.NET Core utilisent une implémentation de la `IAuthorizationPolicyProvider` interface pour récupérer les stratégies d’autorisation. Par défaut, [DefaultAuthorizationPolicyProvider](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) est enregistré et utilisé. `DefaultAuthorizationPolicyProvider` Retourne les stratégies à partir de la `AuthorizationOptions` fourni dans une `IServiceCollection.AddAuthorization` appeler.

Vous pouvez personnaliser ce comportement en enregistrant une autre `IAuthorizationPolicyProvider` mise en œuvre dans l’application [injection de dépendance](xref:fundamentals/dependency-injection) conteneur. 

Le `IAuthorizationPolicyProvider` interface contient deux API :

* [GetPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) renvoie une stratégie d’autorisation pour un nom donné.
* [GetDefaultPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync?view=aspnetcore-2.0) renvoie la stratégie d’autorisation par défaut (la stratégie utilisée pour `[Authorize]` attributs sans une stratégie spécifiée). 

En implémentant ces deux API, vous pouvez personnaliser la manière dont les stratégies d’autorisation sont fournis.

## <a name="parameterized-authorize-attribute-example"></a>Paramétrable autoriser l’exemple d’attribut

Dans un scénario où `IAuthorizationPolicyProvider` est utile est de l’activation personnalisée `[Authorize]` attributs dont les spécifications dépendent d’un paramètre. Par exemple, dans [d’autorisation basée sur la stratégie](xref:security/authorization/policies) documentation, une tranche d’âge (« AtLeast21 ») stratégie a été utilisée en tant qu’exemple. Si les actions de contrôleur différent dans une application doivent être accessibles aux utilisateurs de *différents* âges, il peut être utile d’avoir de nombreuses stratégies autres tranche d’âge. Au lieu d’inscrire toutes les différentes tranche d’âge stratégies que l’application devra dans `AuthorizationOptions`, vous pouvez générer les stratégies dynamiquement avec un personnalisé `IAuthorizationPolicyProvider`. Pour rendre à l’aide des stratégies plus facile, vous pouvez annoter les actions avec l’attribut d’autorisation personnalisée comme `[MinimumAgeAuthorize(20)]`.

## <a name="custom-authorization-attributes"></a>Attributs d’autorisation personnalisée

Stratégies d’autorisation sont identifiés par leurs noms. Personnalisée `MinimumAgeAuthorizeAttribute` décrite précédemment doit mettre en correspondance des arguments dans une chaîne qui peut être utilisée pour récupérer la stratégie d’autorisation correspondant. Ce faire, vous pouvez dérivant de `AuthorizeAttribute` et le `Age` retour à la propriété le `AuthorizeAttribute.Policy` propriété.

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

Ce type d’attribut a un `Policy` chaîne basée sur le préfixe codée en dur (`"MinimumAge"`) et un entier transmis via le constructeur.

Vous pouvez l’appliquer aux actions de la même façon que d’autres `Authorize` des attributs, sauf qu’il accepte un entier en tant que paramètre.

```CSharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a>IAuthorizationPolicyProvider personnalisé

Personnalisé `MinimumAgeAuthorizeAttribute` facilite les stratégies de demande d’autorisation pour n’importe quel âge minimal que vous le souhaitez. Résoudre le problème suivant consiste à s’assurer que les stratégies d’autorisation sont disponibles pour toutes les différentes tranches d’âge. C’est là un `IAuthorizationPolicyProvider` est utile.

Lorsque vous utilisez `MinimumAgeAuthorizationAttribute`, les noms de la stratégie d’autorisation suivront le modèle `"MinimumAge" + Age`, de sorte que personnalisé `IAuthorizationPolicyProvider` doit générer des stratégies d’autorisation par :

* L’analyse de la durée de vie à partir du nom de la stratégie.
* À l’aide de `AuthorizationPolicyBuiler` pour créer un nouveau `AuthorizationPolicy`
* Ajout de conditions à la stratégie basée sur la durée de vie avec `AuthorizationPolicyBuilder.AddRequirements`. Dans d’autres scénarios, vous pouvez utiliser `RequireClaim`, `RequireRole`, ou `RequireUserName` à la place.

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

## <a name="multiple-authorization-policy-providers"></a>Plusieurs fournisseurs de stratégie d’autorisation

Lorsque vous utilisez personnalisé `IAuthorizationPolicyProvider` implémentations, gardez à l’esprit que ASP.NET Core utilise uniquement une seule instance de `IAuthorizationPolicyProvider`. Si un fournisseur personnalisé n’est pas en mesure de fournir des stratégies d’autorisation pour tous les noms de stratégie, il doit revenir à un fournisseur de sauvegarde. Les noms de stratégie peuvent inclure ceux qui proviennent d’une stratégie par défaut pour `[Authorize]` attributs sans nom.

Par exemple, considérez qu'une application nécessaire à la fois les stratégies de vieillissement personnalisé et plus traditionnelle de récupération de stratégie basée sur le rôle. Une telle application peut utiliser un fournisseur de stratégie d’autorisation personnalisée qui :

* Tentatives d’analyse des noms de la stratégie. 
* Appelle un fournisseur de stratégie différent (comme `DefaultAuthorizationPolicyProvider`) si le nom de la stratégie ne contient pas d’âge.

## <a name="default-policy"></a>Stratégie par défaut

En plus de fournir des stratégies d’autorisation nommée, personnalisé `IAuthorizationPolicyProvider` doit implémenter `GetDefaultPolicyAsync` pour fournir une stratégie d’autorisation pour `[Authorize]` attributs sans un nom de la stratégie spécifiée.

Dans de nombreux cas, cet attribut d’autorisation ne requiert qu’un utilisateur authentifié, afin de vérifier la stratégie requise par un appel à `RequireAuthenticatedUser`:

```CSharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

Comme avec tous les aspects de personnalisé `IAuthorizationPolicyProvider`, vous pouvez personnaliser, si nécessaire. Dans certains cas :

* Les stratégies d’autorisation par défaut ne peuvent pas être utilisés.
* La récupération de la stratégie par défaut peut être déléguée à une stratégie de secours `IAuthorizationPolicyProvider`.

## <a name="using-a-custom-iauthorizationpolicyprovider"></a>À l’aide d’un IAuthorizationPolicyProvider personnalisé

Pour utiliser les stratégies personnalisées à partir d’un `IAuthorizationPolicyProvider`, vous devez :

* Inscrire les `AuthorizationHandler` types avec l’injection de dépendances (décrit dans [d’autorisation basée sur la stratégie](xref:security/authorization/policies#authorization-handlers)), comme avec tous les scénarios d’autorisation basée sur la stratégie.
* Inscrire personnalisé `IAuthorizationPolicyProvider` type dans la collection de service de d’injection de dépendances de l’application (dans `Startup.ConfigureServices`) pour remplacer le fournisseur de stratégie par défaut.

```CSharp
services.AddTransient<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

Personnalisé complet `IAuthorizationPolicyProvider` exemple est disponible dans le [référentiel GitHub d’aspnet/AuthSamples](https://github.com/aspnet/AuthSamples/tree/dev/samples/CustomPolicyProvider).
