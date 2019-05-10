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
# <a name="claims-based-authorization-in-aspnet-core"></a><span data-ttu-id="24e8e-103">Autorisation basée sur les revendications dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="24e8e-103">Claims-based authorization in ASP.NET Core</span></span>

<a name="security-authorization-claims-based"></a>

<span data-ttu-id="24e8e-104">Lors de la création d’une identité d’une ou plusieurs revendications émises par une partie de confiance peut lui être attribuée.</span><span class="sxs-lookup"><span data-stu-id="24e8e-104">When an identity is created it may be assigned one or more claims issued by a trusted party.</span></span> <span data-ttu-id="24e8e-105">Une revendication est une paire nom-valeur qui représente le sujet est, pas quel sujet peut faire.</span><span class="sxs-lookup"><span data-stu-id="24e8e-105">A claim is a name value pair that represents what the subject is, not what the subject can do.</span></span> <span data-ttu-id="24e8e-106">Par exemple, vous pouvez avoir conduire un permis de, émis par une autorité de licence de conduite local.</span><span class="sxs-lookup"><span data-stu-id="24e8e-106">For example, you may have a driver's license, issued by a local driving license authority.</span></span> <span data-ttu-id="24e8e-107">Conduire votre permis de comporte votre date de naissance.</span><span class="sxs-lookup"><span data-stu-id="24e8e-107">Your driver's license has your date of birth on it.</span></span> <span data-ttu-id="24e8e-108">Dans ce cas le nom de la revendication serait `DateOfBirth`, la valeur de revendication serait votre date de naissance, par exemple `8th June 1970` et l’émetteur serait l’autorité de licence de conduite.</span><span class="sxs-lookup"><span data-stu-id="24e8e-108">In this case the claim name would be `DateOfBirth`, the claim value would be your date of birth, for example `8th June 1970` and the issuer would be the driving license authority.</span></span> <span data-ttu-id="24e8e-109">Autorisation basée sur les revendications, la plus simple, vérifie la valeur d’une revendication et autorise l’accès à une ressource en fonction de cette valeur.</span><span class="sxs-lookup"><span data-stu-id="24e8e-109">Claims based authorization, at its simplest, checks the value of a claim and allows access to a resource based upon that value.</span></span> <span data-ttu-id="24e8e-110">Pour exemple, si vous souhaitez accéder à un club de nuit, le processus d’autorisation peut être :</span><span class="sxs-lookup"><span data-stu-id="24e8e-110">For example if you want access to a night club the authorization process might be:</span></span>

<span data-ttu-id="24e8e-111">Donne la valeur la valeur de la date de naissance revendication et qu’elles s’approuvent l’émetteur (l’autorité de licence conduite) avant d’accorder à vous accéder à votre responsable de la sécurité de la porte.</span><span class="sxs-lookup"><span data-stu-id="24e8e-111">The door security officer would evaluate the value of your date of birth claim and whether they trust the issuer (the driving license authority) before granting you access.</span></span>

<span data-ttu-id="24e8e-112">Une identité peut contenir plusieurs revendications avec plusieurs valeurs et peut contenir plusieurs revendications du même type.</span><span class="sxs-lookup"><span data-stu-id="24e8e-112">An identity can contain multiple claims with multiple values and can contain multiple claims of the same type.</span></span>

## <a name="adding-claims-checks"></a><span data-ttu-id="24e8e-113">Ajout de vérifications de revendications</span><span class="sxs-lookup"><span data-stu-id="24e8e-113">Adding claims checks</span></span>

<span data-ttu-id="24e8e-114">Revendication les vérifications d’autorisations déclaratives - le développeur les incorpore dans leur code, par rapport à un contrôleur ou une action dans un contrôleur, en spécifiant les revendications qui l’utilisateur actuel doit posséder et éventuellement la valeur de la revendication doit contenir pour accéder à la ressource demandée.</span><span class="sxs-lookup"><span data-stu-id="24e8e-114">Claim based authorization checks are declarative - the developer embeds them within their code, against a controller or an action within a controller, specifying claims which the current user must possess, and optionally the value the claim must hold to access the requested resource.</span></span> <span data-ttu-id="24e8e-115">Configuration requise est basée sur la stratégie de revendications, le développeur doit créer et enregistrer une stratégie d’exprimer les exigences de revendications.</span><span class="sxs-lookup"><span data-stu-id="24e8e-115">Claims requirements are policy based, the developer must build and register a policy expressing the claims requirements.</span></span>

<span data-ttu-id="24e8e-116">Le type le plus simple de stratégie recherche la présence d’une revendication de revendication et ne vérifie pas la valeur.</span><span class="sxs-lookup"><span data-stu-id="24e8e-116">The simplest type of claim policy looks for the presence of a claim and doesn't check the value.</span></span>

<span data-ttu-id="24e8e-117">Vous devez d’abord générer et enregistrez la stratégie.</span><span class="sxs-lookup"><span data-stu-id="24e8e-117">First you need to build and register the policy.</span></span> <span data-ttu-id="24e8e-118">Cette opération a lieu dans le cadre de la configuration du service d’autorisation, qui fait normalement partie intégrante de `ConfigureServices()` dans votre *Startup.cs* fichier.</span><span class="sxs-lookup"><span data-stu-id="24e8e-118">This takes place as part of the Authorization service configuration, which normally takes part in `ConfigureServices()` in your *Startup.cs* file.</span></span>

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

<span data-ttu-id="24e8e-119">Dans ce cas le `EmployeeOnly` stratégie vérifie la présence d’un `EmployeeNumber` revendication sur l’identité actuelle.</span><span class="sxs-lookup"><span data-stu-id="24e8e-119">In this case the `EmployeeOnly` policy checks for the presence of an `EmployeeNumber` claim on the current identity.</span></span>

<span data-ttu-id="24e8e-120">Vous appliquez ensuite la stratégie à l’aide de la `Policy` propriété sur le `AuthorizeAttribute` attribut pour spécifier le nom de la stratégie ;</span><span class="sxs-lookup"><span data-stu-id="24e8e-120">You then apply the policy using the `Policy` property on the `AuthorizeAttribute` attribute to specify the policy name;</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

<span data-ttu-id="24e8e-121">Le `AuthorizeAttribute` attribut peut être appliqué à un contrôleur entier, dans cette instance uniquement la stratégie de correspondance des identités peut accéder à une Action sur le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="24e8e-121">The `AuthorizeAttribute` attribute can be applied to an entire controller, in this instance only identities matching the policy will be allowed access to any Action on the controller.</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

<span data-ttu-id="24e8e-122">Si vous disposez d’un contrôleur qui est protégé par le `AuthorizeAttribute` d’attribut, mais souhaitez autoriser l’accès anonyme aux actions particulières que vous appliquez le `AllowAnonymousAttribute` attribut.</span><span class="sxs-lookup"><span data-stu-id="24e8e-122">If you have a controller that's protected by the `AuthorizeAttribute` attribute, but want to allow anonymous access to particular actions you apply the `AllowAnonymousAttribute` attribute.</span></span>

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

<span data-ttu-id="24e8e-123">La plupart des affirmations sont fournis avec une valeur.</span><span class="sxs-lookup"><span data-stu-id="24e8e-123">Most claims come with a value.</span></span> <span data-ttu-id="24e8e-124">Vous pouvez spécifier une liste de valeurs autorisées lors de la création de la stratégie.</span><span class="sxs-lookup"><span data-stu-id="24e8e-124">You can specify a list of allowed values when creating the policy.</span></span> <span data-ttu-id="24e8e-125">L’exemple suivant aboutirait uniquement pour les employés dont le numéro employé a été 1, 2, 3, 4 ou 5.</span><span class="sxs-lookup"><span data-stu-id="24e8e-125">The following example would only succeed for employees whose employee number was 1, 2, 3, 4 or 5.</span></span>

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

### <a name="add-a-generic-claim-check"></a><span data-ttu-id="24e8e-126">Ajouter une vérification de la revendication générique</span><span class="sxs-lookup"><span data-stu-id="24e8e-126">Add a generic claim check</span></span>

<span data-ttu-id="24e8e-127">Si la valeur de revendication n’est pas une valeur unique ou une transformation est requise, utilisez [RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion).</span><span class="sxs-lookup"><span data-stu-id="24e8e-127">If the claim value isn't a single value or a transformation is required, use [RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion).</span></span> <span data-ttu-id="24e8e-128">Pour plus d’informations, consultez [à l’aide d’une variable func pour répondre à une stratégie](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy).</span><span class="sxs-lookup"><span data-stu-id="24e8e-128">For more information, see [Using a func to fulfill a policy](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy).</span></span>

## <a name="multiple-policy-evaluation"></a><span data-ttu-id="24e8e-129">Évaluation des stratégies</span><span class="sxs-lookup"><span data-stu-id="24e8e-129">Multiple Policy Evaluation</span></span>

<span data-ttu-id="24e8e-130">Si vous appliquez plusieurs stratégies à un contrôleur ou d’action, puis toutes les stratégies doivent s’écouler avant l’accès est accordé.</span><span class="sxs-lookup"><span data-stu-id="24e8e-130">If you apply multiple policies to a controller or action, then all policies must pass before access is granted.</span></span> <span data-ttu-id="24e8e-131">Exemple :</span><span class="sxs-lookup"><span data-stu-id="24e8e-131">For example:</span></span>

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

<span data-ttu-id="24e8e-132">Dans l’exemple ci-dessus n’importe quelle identité, ce qui répond à la `EmployeeOnly` stratégie peut accéder à la `Payslip` action en tant que cette stratégie est appliquée sur le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="24e8e-132">In the above example any identity which fulfills the `EmployeeOnly` policy can access the `Payslip` action as that policy is enforced on the controller.</span></span> <span data-ttu-id="24e8e-133">Toutefois pour pouvoir appeler le `UpdateSalary` action doit répondre à l’identité *à la fois* le `EmployeeOnly` stratégie et le `HumanResources` stratégie.</span><span class="sxs-lookup"><span data-stu-id="24e8e-133">However in order to call the `UpdateSalary` action the identity must fulfill *both* the `EmployeeOnly` policy and the `HumanResources` policy.</span></span>

<span data-ttu-id="24e8e-134">Si vous souhaitez que des stratégies plus complexes, telles que la réalisation d’une date de naissance revendication, calculer un âge à partir de celui-ci, puis la vérification de l’âge est 21 ou version antérieure, vous devez écrire [gestionnaires de stratégie personnalisée](xref:security/authorization/policies).</span><span class="sxs-lookup"><span data-stu-id="24e8e-134">If you want more complicated policies, such as taking a date of birth claim, calculating an age from it then checking the age is 21 or older then you need to write [custom policy handlers](xref:security/authorization/policies).</span></span>
