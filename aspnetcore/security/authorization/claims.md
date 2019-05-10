---
title: Autorisation basée sur les revendications dans ASP.NET Core
author: rick-anderson
description: Découvrez comment ajouter des contrôles de revendications pour l’autorisation dans une application ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/claims
ms.openlocfilehash: 6b60ae5515819b017ab577f655ed91ee4d8ed0dd
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65086152"
---
# <a name="claims-based-authorization-in-aspnet-core"></a>Autorisation basée sur les revendications dans ASP.NET Core

<a name="security-authorization-claims-based"></a>

Lors de la création d’une identité d’une ou plusieurs revendications émises par une partie de confiance peut lui être attribuée. Une revendication est une paire nom-valeur qui représente le sujet est, pas quel sujet peut faire. Par exemple, vous pouvez avoir conduire un permis de, émis par une autorité de licence de conduite local. Conduire votre permis de comporte votre date de naissance. Dans ce cas le nom de la revendication serait `DateOfBirth`, la valeur de revendication serait votre date de naissance, par exemple `8th June 1970` et l’émetteur serait l’autorité de licence de conduite. Autorisation basée sur les revendications, la plus simple, vérifie la valeur d’une revendication et autorise l’accès à une ressource en fonction de cette valeur. Pour exemple, si vous souhaitez accéder à un club de nuit, le processus d’autorisation peut être :

Donne la valeur la valeur de la date de naissance revendication et qu’elles s’approuvent l’émetteur (l’autorité de licence conduite) avant d’accorder à vous accéder à votre responsable de la sécurité de la porte.

Une identité peut contenir plusieurs revendications avec plusieurs valeurs et peut contenir plusieurs revendications du même type.

## <a name="adding-claims-checks"></a>Ajout de vérifications de revendications

Revendication les vérifications d’autorisations déclaratives - le développeur les incorpore dans leur code, par rapport à un contrôleur ou une action dans un contrôleur, en spécifiant les revendications qui l’utilisateur actuel doit posséder et éventuellement la valeur de la revendication doit contenir pour accéder à la ressource demandée. Configuration requise est basée sur la stratégie de revendications, le développeur doit créer et enregistrer une stratégie d’exprimer les exigences de revendications.

Le type le plus simple de stratégie recherche la présence d’une revendication de revendication et ne vérifie pas la valeur.

Vous devez d’abord générer et enregistrez la stratégie. Cette opération a lieu dans le cadre de la configuration du service d’autorisation, qui fait normalement partie intégrante de `ConfigureServices()` dans votre *Startup.cs* fichier.

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

Dans ce cas le `EmployeeOnly` stratégie vérifie la présence d’un `EmployeeNumber` revendication sur l’identité actuelle.

Vous appliquez ensuite la stratégie à l’aide de la `Policy` propriété sur le `AuthorizeAttribute` attribut pour spécifier le nom de la stratégie ;

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

Le `AuthorizeAttribute` attribut peut être appliqué à un contrôleur entier, dans cette instance uniquement la stratégie de correspondance des identités peut accéder à une Action sur le contrôleur.

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

Si vous disposez d’un contrôleur qui est protégé par le `AuthorizeAttribute` d’attribut, mais souhaitez autoriser l’accès anonyme aux actions particulières que vous appliquez le `AllowAnonymousAttribute` attribut.

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

La plupart des affirmations sont fournis avec une valeur. Vous pouvez spécifier une liste de valeurs autorisées lors de la création de la stratégie. L’exemple suivant aboutirait uniquement pour les employés dont le numéro employé a été 1, 2, 3, 4 ou 5.

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

### <a name="add-a-generic-claim-check"></a>Ajouter une vérification de la revendication générique

Si la valeur de revendication n’est pas une valeur unique ou une transformation est requise, utilisez [RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion). Pour plus d’informations, consultez [à l’aide d’une variable func pour répondre à une stratégie](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy).

## <a name="multiple-policy-evaluation"></a>Évaluation des stratégies

Si vous appliquez plusieurs stratégies à un contrôleur ou d’action, puis toutes les stratégies doivent s’écouler avant l’accès est accordé. Exemple :

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

Dans l’exemple ci-dessus n’importe quelle identité, ce qui répond à la `EmployeeOnly` stratégie peut accéder à la `Payslip` action en tant que cette stratégie est appliquée sur le contrôleur. Toutefois pour pouvoir appeler le `UpdateSalary` action doit répondre à l’identité *à la fois* le `EmployeeOnly` stratégie et le `HumanResources` stratégie.

Si vous souhaitez que des stratégies plus complexes, telles que la réalisation d’une date de naissance revendication, calculer un âge à partir de celui-ci, puis la vérification de l’âge est 21 ou version antérieure, vous devez écrire [gestionnaires de stratégie personnalisée](xref:security/authorization/policies).
