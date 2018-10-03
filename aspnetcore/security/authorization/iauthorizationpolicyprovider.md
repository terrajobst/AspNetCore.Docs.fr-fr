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
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a>Fournisseurs de stratégie d’autorisation personnalisés à l’aide de IAuthorizationPolicyProvider dans ASP.NET Core 

Par [Mike Rousos](https://github.com/mjrousos)

En général, lorsque vous utilisez [autorisation basée sur la stratégie](xref:security/authorization/policies), stratégies sont inscrits en appelant `AuthorizationOptions.AddPolicy` dans le cadre de la configuration du service d’autorisation. Dans certains scénarios, il peut être pas possible (ou souhaitable) pour enregistrer toutes les stratégies d’autorisation de cette façon. Dans ce cas, vous pouvez utiliser un personnalisé `IAuthorizationPolicyProvider` pour contrôler la façon dont les stratégies d’autorisation sont fournis.

Exemples de scénarios où un personnalisé [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) peuvent être utiles incluent :

* À l’aide d’un service externe pour fournir l’évaluation de la stratégie.
* À l’aide d’une grande plage de stratégies (pour les nombres de pièce ou âges, par exemple), par conséquent, il est inutile pour ajouter chaque stratégie d’autorisation individuels avec une `AuthorizationOptions.AddPolicy` appeler.
* Création de stratégies lors de l’exécution en fonction des informations dans une source de données externe (par exemple, une base de données) ou la détermination des exigences d’autorisation dynamiquement via un autre mécanisme.

[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/AuthSamples/tree/master/samples/CustomPolicyProvider) à partir de la [référentiel aspnet/AuthSamples](https://github.com/aspnet/AuthSamples). Téléchargez le fichier ZIP d’aspnet/AuthSamples référentiel.
Décompressez le *AuthSamples-master.zip* fichier. Accédez à la *exemples/CustomPolicyProvider* dossier du projet.

## <a name="customize-policy-retrieval"></a>Personnaliser la récupération de stratégie

Les applications ASP.NET Core utilisent une implémentation de la `IAuthorizationPolicyProvider` interface pour récupérer les stratégies d’autorisation. Par défaut, [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) est enregistré et utilisé. `DefaultAuthorizationPolicyProvider` Retourne les stratégies à partir de la `AuthorizationOptions` fourni dans un `IServiceCollection.AddAuthorization` appeler.

Vous pouvez personnaliser ce comportement en enregistrant une autre `IAuthorizationPolicyProvider` implémentation de l’application [l’injection de dépendances](xref:fundamentals/dependency-injection) conteneur. 

Le `IAuthorizationPolicyProvider` interface contient deux API :

* [GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) renvoie une stratégie d’autorisation pour un nom donné.
* [GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) renvoie la stratégie d’autorisation par défaut (la stratégie utilisée pour `[Authorize]` attributs sans une stratégie spécifiée). 

En implémentant ces deux API, vous pouvez personnaliser la façon dont les stratégies d’autorisation sont fournies.

## <a name="parameterized-authorize-attribute-example"></a>Paramétrables autoriser l’exemple d’attribut

Un scénario où `IAuthorizationPolicyProvider` est utile est l’activation personnalisé `[Authorize]` attributs dont les exigences dépendent d’un paramètre. Par exemple, dans [autorisation basée sur la stratégie](xref:security/authorization/policies) documentation, une tranche d’âge (« AtLeast21 ») stratégie a été utilisée en tant qu’exemple. Si les actions de contrôleur différent dans une application doivent être accessibles aux utilisateurs de *différents* âges, il peut être utile d’avoir de nombreuses stratégies différentes de tranche d’âge. Au lieu d’inscrire toutes les différentes tranche d’âge stratégies que l’application devra dans `AuthorizationOptions`, vous pouvez générer les stratégies dynamiquement avec un personnalisé `IAuthorizationPolicyProvider`. Pour rendre à l’aide de stratégies plus facile, vous pouvez annoter des actions avec l’attribut d’autorisation personnalisé comme `[MinimumAgeAuthorize(20)]`.

## <a name="custom-authorization-attributes"></a>Attributs d’autorisation personnalisée

Stratégies d’autorisation sont identifiés par leurs noms. Personnalisé `MinimumAgeAuthorizeAttribute` décrit précédemment doit mettre en correspondance d’arguments dans une chaîne qui peut être utilisée pour récupérer la stratégie d’autorisation correspondant. Ce faire, vous pouvez dérivant `AuthorizeAttribute` et en rendant le `Age` habillage de la propriété le `AuthorizeAttribute.Policy` propriété.

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

Ce type d’attribut a un `Policy` chaîne basée sur le préfixe codées en dur (`"MinimumAge"`) et un entier transmis via le constructeur.

Vous pouvez l’appliquer aux actions de la même façon que les autres `Authorize` attributs, à ceci près qu’elle prend un entier en tant que paramètre.

```csharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a>IAuthorizationPolicyProvider personnalisé

Personnalisé `MinimumAgeAuthorizeAttribute` facilite pour les stratégies de demande d’autorisation pour n’importe quel âge minimal souhaité. Le problème suivant à résoudre consiste à s’assurer que les stratégies d’autorisation sont disponibles pour l’ensemble de ces différentes tranches d’âge. C’est là une `IAuthorizationPolicyProvider` est utile.

Lorsque vous utilisez `MinimumAgeAuthorizationAttribute`, les noms de la stratégie d’autorisation suivra le modèle `"MinimumAge" + Age`, de sorte que personnalisé `IAuthorizationPolicyProvider` doit générer des stratégies d’autorisation par :

* L’analyse de l’âge à partir du nom de la stratégie.
* À l’aide de `AuthorizationPolicyBuilder` pour créer un nouveau `AuthorizationPolicy`
* Ajouter des exigences à la stratégie selon l’âge avec `AuthorizationPolicyBuilder.AddRequirements`. Dans d’autres scénarios, vous pouvez utiliser `RequireClaim`, `RequireRole`, ou `RequireUserName` à la place.

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

## <a name="multiple-authorization-policy-providers"></a>Plusieurs fournisseurs de stratégie d’autorisation

Lorsque vous utilisez personnalisé `IAuthorizationPolicyProvider` implémentations, n’oubliez pas que ASP.NET Core utilise uniquement une seule instance de `IAuthorizationPolicyProvider`. Si un fournisseur personnalisé n’est pas en mesure de fournir des stratégies d’autorisation pour tous les noms de stratégie, il doit revenir à un fournisseur de sauvegarde. Les noms de stratégie, citons ceux qui proviennent d’une stratégie par défaut pour `[Authorize]` attributs sans nom.

Par exemple, considérez qu'une application nécessaire les stratégies de vieillissement personnalisé et l’extraction de stratégie en fonction du rôle plus traditionnelle. Une telle application peut utiliser un fournisseur de stratégie d’autorisation personnalisée qui :

* Tente d’analyser les noms de stratégie. 
* Appelle un fournisseur de stratégie différent (comme `DefaultAuthorizationPolicyProvider`) si le nom de la stratégie ne contient pas d’âge.

## <a name="default-policy"></a>Stratégie par défaut

En plus de fournir des stratégies d’autorisation nommés, personnalisé `IAuthorizationPolicyProvider` doit implémenter `GetDefaultPolicyAsync` pour fournir une stratégie d’autorisation pour `[Authorize]` attributs sans un nom de la stratégie spécifiée.

Dans de nombreux cas, cet attribut d’autorisation requiert uniquement un utilisateur authentifié, vous pouvez effectuer la stratégie nécessaire avec un appel à `RequireAuthenticatedUser`:

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

Comme avec tous les aspects de personnalisé `IAuthorizationPolicyProvider`, vous pouvez personnaliser ceci, en fonction des besoins. Dans certains cas :

* Les stratégies d’autorisation par défaut ne peuvent pas être utilisés.
* Récupération de la stratégie par défaut peut être déléguée à une procédure de secours `IAuthorizationPolicyProvider`.

## <a name="use-a-custom-iauthorizationpolicyprovider"></a>Utiliser un IAuthorizationPolicyProvider personnalisé

Pour utiliser des stratégies personnalisées à partir d’un `IAuthorizationPolicyProvider`, vous devez :

* Inscrire approprié `AuthorizationHandler` types avec l’injection de dépendance (décrit dans [autorisation basée sur la stratégie](xref:security/authorization/policies#authorization-handlers)), à l’instar de tous les scénarios d’autorisation basée sur la stratégie.
* Inscrire personnalisé `IAuthorizationPolicyProvider` type dans la collection de service de l’injection de dépendances de l’application (dans `Startup.ConfigureServices`) pour remplacer le fournisseur de stratégie par défaut.

```csharp
services.AddSingleton<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

Personnalisé complet `IAuthorizationPolicyProvider` exemple est disponible dans le [référentiel aspnet/AuthSamples](https://github.com/aspnet/AuthSamples/tree/master/samples/CustomPolicyProvider).
