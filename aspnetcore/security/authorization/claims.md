---
title: Autorisation basée sur les revendications dans ASP.NET Core
author: rick-anderson
description: Découvrez comment ajouter des vérifications de revendications pour l’autorisation dans une application ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/claims
ms.openlocfilehash: e289851aafcbc7e3b3f60ab9fbe4b182a78bdf8a
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661804"
---
# <a name="claims-based-authorization-in-aspnet-core"></a>Autorisation basée sur les revendications dans ASP.NET Core

<a name="security-authorization-claims-based"></a>

Lorsqu’une identité est créée, elle peut se voir attribuer une ou plusieurs revendications émises par un tiers de confiance. Une revendication est une paire nom/valeur qui représente l’objet, pas ce que le sujet peut faire. Par exemple, vous pouvez avoir une licence de pilote, publiée par une autorité de certification de conduite locale. La date de la licence de votre pilote est la date de naissance. Dans ce cas, le nom de la revendication est `DateOfBirth`, la valeur de la revendication est la date de naissance, par exemple `8th June 1970` et l’émetteur est l’autorité de la licence de conduite. L’autorisation basée sur les revendications, à son plus simple, vérifie la valeur d’une revendication et autorise l’accès à une ressource en fonction de cette valeur. Par exemple, si vous souhaitez accéder à un club nocturne, le processus d’autorisation peut être :

Le responsable de la sécurité de la porte évalue la valeur de votre revendication de date de naissance et s’il fait confiance à l’émetteur (l’autorité de licence de conduite) avant de vous accorder l’accès.

Une identité peut contenir plusieurs revendications avec plusieurs valeurs et peut contenir plusieurs revendications du même type.

## <a name="adding-claims-checks"></a>Ajout de vérifications de revendications

Les vérifications d’autorisation basées sur les revendications sont déclaratives : le développeur les incorpore dans leur code, sur un contrôleur ou une action au sein d’un contrôleur, en spécifiant les revendications que l’utilisateur actuel doit posséder, et éventuellement la valeur que la revendication doit conserver pour accéder au ressource demandée. Les demandes de revendications sont basées sur des stratégies, le développeur doit créer et inscrire une stratégie exprimant les exigences en matière de revendications.

Le type de stratégie de revendication le plus simple recherche la présence d’une revendication et ne vérifie pas la valeur.

Tout d’abord, vous devez créer et inscrire la stratégie. Cela a lieu dans le cadre de la configuration du service d’autorisation, qui participe normalement à `ConfigureServices()` dans votre fichier *Startup.cs* .

::: moniker range=">= aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllersWithViews();
    services.AddRazorPages();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("EmployeeOnly", policy => policy.RequireClaim("EmployeeNumber"));
    });
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("EmployeeOnly", policy => policy.RequireClaim("EmployeeNumber"));
    });
}
```

::: moniker-end

Dans ce cas, la stratégie de `EmployeeOnly` vérifie la présence d’une revendication `EmployeeNumber` sur l’identité actuelle.

Vous appliquez ensuite la stratégie à l’aide de la propriété `Policy` de l’attribut `AuthorizeAttribute` pour spécifier le nom de la stratégie ;

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

L’attribut `AuthorizeAttribute` peut être appliqué à un contrôleur entier. dans ce cas, seules les identités correspondant à la stratégie seront autorisées à accéder à une action sur le contrôleur.

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

Si vous avez un contrôleur qui est protégé par l’attribut `AuthorizeAttribute`, mais que vous souhaitez autoriser un accès anonyme à des actions spécifiques, vous appliquez l’attribut `AllowAnonymousAttribute`.

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }

    [AllowAnonymous]
    public ActionResult VacationPolicy()
    {
    }
}
```

La plupart des revendications sont accompagnées d’une valeur. Vous pouvez spécifier une liste de valeurs autorisées lors de la création de la stratégie. L’exemple suivant ne fonctionne que pour les employés dont le matricule d’employé était 1, 2, 3, 4 ou 5.

::: moniker range=">= aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllersWithViews();
    services.AddRazorPages();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Founders", policy =>
                          policy.RequireClaim("EmployeeNumber", "1", "2", "3", "4", "5"));
    });
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Founders", policy =>
                          policy.RequireClaim("EmployeeNumber", "1", "2", "3", "4", "5"));
    });
}
```

::: moniker-end
### <a name="add-a-generic-claim-check"></a>Ajouter une vérification de revendication générique

Si la valeur de revendication n’est pas une valeur unique ou si une transformation est requise, utilisez [RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion). Pour plus d’informations, consultez [utilisation d’une fonction Func pour accomplir une stratégie](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy).

## <a name="multiple-policy-evaluation"></a>Évaluation de plusieurs stratégies

Si vous appliquez plusieurs stratégies à un contrôleur ou à une action, toutes les stratégies doivent réussir avant l’octroi de l’accès. Par exemple :

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class SalaryController : Controller
{
    public ActionResult Payslip()
    {
    }

    [Authorize(Policy = "HumanResources")]
    public ActionResult UpdateSalary()
    {
    }
}
```

Dans l’exemple ci-dessus, toute identité qui répond à la stratégie de `EmployeeOnly` peut accéder à l’action `Payslip` lorsque cette stratégie est appliquée sur le contrôleur. Toutefois, pour appeler l’action `UpdateSalary`, l’identité *doit respecter la stratégie de `EmployeeOnly`* et la stratégie de `HumanResources`.

Si vous souhaitez des stratégies plus compliquées, telles que la prise de la revendication date de naissance, le calcul d’une ancienneté, le contrôle de l’âge est 21 ou plus, vous devez écrire des [gestionnaires de stratégie personnalisés](xref:security/authorization/policies).
