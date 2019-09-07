---
title: Authentification et autorisation avec ASP.NET Core Blazor
author: guardrex
description: Pour en savoir plus sur les scénarios d’authentification et d’autorisation avec Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2019
uid: security/blazor/index
ms.openlocfilehash: 2ba7b0612c2be50ae0797c50dc3cb0d63c0f0c2d
ms.sourcegitcommit: 43c6335b5859282f64d66a7696c5935a2bcdf966
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/07/2019
ms.locfileid: "70800507"
---
# <a name="aspnet-core-blazor-authentication-and-authorization"></a><span data-ttu-id="0fe4e-103">Authentification et autorisation avec ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="0fe4e-103">ASP.NET Core Blazor authentication and authorization</span></span>

<span data-ttu-id="0fe4e-104">Par [Steve Sanderson](https://github.com/SteveSandersonMS)</span><span class="sxs-lookup"><span data-stu-id="0fe4e-104">By [Steve Sanderson](https://github.com/SteveSandersonMS)</span></span>

<span data-ttu-id="0fe4e-105">ASP.NET Core prend en charge la configuration et la gestion de la sécurité dans les applications Blazor.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-105">ASP.NET Core supports the configuration and management of security in Blazor apps.</span></span>

<span data-ttu-id="0fe4e-106">Les scénarios de sécurité diffèrent entre les applications Blazor côté serveur et côté client.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-106">Security scenarios differ between Blazor server-side and client-side apps.</span></span> <span data-ttu-id="0fe4e-107">Étant donné que les applications Blazor côté serveur s’exécutent sur le serveur, les vérifications d’autorisation sont en mesure de déterminer :</span><span class="sxs-lookup"><span data-stu-id="0fe4e-107">Because Blazor server-side apps run on the server, authorization checks are able to determine:</span></span>

* <span data-ttu-id="0fe4e-108">Les options de l’interface utilisateur présentées à un utilisateur (par exemple, les entrées de menu disponibles pour un utilisateur).</span><span class="sxs-lookup"><span data-stu-id="0fe4e-108">The UI options presented to a user (for example, which menu entries are available to a user).</span></span>
* <span data-ttu-id="0fe4e-109">Les règles d’accès pour les zones de l’application et les composants.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-109">Access rules for areas of the app and components.</span></span>

<span data-ttu-id="0fe4e-110">Les applications Blazor côté client exécutées sur le client.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-110">Blazor client-side apps run on the client.</span></span> <span data-ttu-id="0fe4e-111">L’autorisation est *uniquement* utilisée pour déterminer les options de l’interface utilisateur à afficher.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-111">Authorization is *only* used to determine which UI options to show.</span></span> <span data-ttu-id="0fe4e-112">Dans la mesure où les vérifications côté client peuvent être modifiées ou ignorées par un utilisateur, une application Blazor côté client ne peut pas appliquer les règles d’accès d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-112">Since client-side checks can be modified or bypassed by a user, a Blazor client-side app can't enforce authorization access rules.</span></span>

## <a name="authentication"></a><span data-ttu-id="0fe4e-113">Authentication</span><span class="sxs-lookup"><span data-stu-id="0fe4e-113">Authentication</span></span>

<span data-ttu-id="0fe4e-114">Blazor utilise les mécanismes d’authentification ASP.NET Core existants pour établir l’identité de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-114">Blazor uses the existing ASP.NET Core authentication mechanisms to establish the user's identity.</span></span> <span data-ttu-id="0fe4e-115">Le mécanisme exact dépend de la façon dont l’application Blazor est hébergée, côté serveur ou côté client.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-115">The exact mechanism depends on how the Blazor app is hosted, server-side or client-side.</span></span>

### <a name="blazor-server-side-authentication"></a><span data-ttu-id="0fe4e-116">Authentification Blazor côté serveur</span><span class="sxs-lookup"><span data-stu-id="0fe4e-116">Blazor server-side authentication</span></span>

<span data-ttu-id="0fe4e-117">Les applications Blazor côté serveur fonctionnent via une connexion en temps réel qui est créée à l’aide de SignalR.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-117">Blazor server-side apps operate over a real-time connection that's created using SignalR.</span></span> <span data-ttu-id="0fe4e-118">[L’authentification dans les applications basées sur SignalR](xref:signalr/authn-and-authz) est gérée lorsque la connexion est établie.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-118">[Authentication in SignalR-based apps](xref:signalr/authn-and-authz) is handled when the connection is established.</span></span> <span data-ttu-id="0fe4e-119">L’authentification peut reposer sur un cookie ou un autre jeton du porteur.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-119">Authentication can be based on a cookie or some other bearer token.</span></span>

<span data-ttu-id="0fe4e-120">Le modèle de projet Blazor côté serveur peut configurer l’authentification pour vous lorsque le projet est créé.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-120">The Blazor server-side project template can set up authentication for you when the project is created.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0fe4e-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0fe4e-121">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0fe4e-122">Suivez les instructions de Visual Studio dans l’article <xref:blazor/get-started> pour créer un projet Blazor côté serveur avec un mécanisme d’authentification.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-122">Follow the Visual Studio guidance in the <xref:blazor/get-started> article to create a new Blazor server-side project with an authentication mechanism.</span></span>

<span data-ttu-id="0fe4e-123">Après avoir choisi le modèle **Application serveur Blazor** dans la boîte de dialogue **Créer une application web ASP.NET Core.** , sélectionnez **Modifier** sous **Authentification**.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-123">After choosing the **Blazor Server App** template in the **Create a new ASP.NET Core Web Application** dialog, select **Change** under **Authentication**.</span></span>

<span data-ttu-id="0fe4e-124">Une boîte de dialogue s’ouvre pour offrir le même ensemble de mécanismes d’authentification que ceux disponibles pour les autres projets ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="0fe4e-124">A dialog opens to offer the same set of authentication mechanisms available for other ASP.NET Core projects:</span></span>

* <span data-ttu-id="0fe4e-125">**Aucune authentification**</span><span class="sxs-lookup"><span data-stu-id="0fe4e-125">**No Authentication**</span></span>
* <span data-ttu-id="0fe4e-126">**Comptes d’utilisateur individuels** &ndash; Les comptes d'utilisateur peuvent être stockés :</span><span class="sxs-lookup"><span data-stu-id="0fe4e-126">**Individual User Accounts** &ndash; User accounts can be stored:</span></span>
  * <span data-ttu-id="0fe4e-127">Dans l’application à l’aide du système [d’identité](xref:security/authentication/identity) d’ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-127">Within the app using ASP.NET Core's [Identity](xref:security/authentication/identity) system.</span></span>
  * <span data-ttu-id="0fe4e-128">Avec [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span><span class="sxs-lookup"><span data-stu-id="0fe4e-128">With [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span>
* <span data-ttu-id="0fe4e-129">**Comptes professionnels ou scolaires**</span><span class="sxs-lookup"><span data-stu-id="0fe4e-129">**Work or School Accounts**</span></span>
* <span data-ttu-id="0fe4e-130">**Authentification Windows**</span><span class="sxs-lookup"><span data-stu-id="0fe4e-130">**Windows Authentication**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0fe4e-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0fe4e-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="0fe4e-132">Suivez les instructions de Visual Studio Code dans l’article <xref:blazor/get-started> pour créer un projet Blazor côté serveur avec un mécanisme d’authentification :</span><span class="sxs-lookup"><span data-stu-id="0fe4e-132">Follow the Visual Studio Code guidance in the <xref:blazor/get-started> article to create a new Blazor server-side project with an authentication mechanism:</span></span>

```console
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

<span data-ttu-id="0fe4e-133">Les valeurs autorisées d’authentification (`{AUTHENTICATION}`) sont présentées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-133">Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.</span></span>

| <span data-ttu-id="0fe4e-134">Mécanisme d’authentification</span><span class="sxs-lookup"><span data-stu-id="0fe4e-134">Authentication mechanism</span></span>                                                                 | <span data-ttu-id="0fe4e-135">Valeur`{AUTHENTICATION}`</span><span class="sxs-lookup"><span data-stu-id="0fe4e-135">`{AUTHENTICATION}` value</span></span> |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| <span data-ttu-id="0fe4e-136">Aucune authentification</span><span class="sxs-lookup"><span data-stu-id="0fe4e-136">No Authentication</span></span>                                                                        | `None`                   |
| <span data-ttu-id="0fe4e-137">Individuel</span><span class="sxs-lookup"><span data-stu-id="0fe4e-137">Individual</span></span><br><span data-ttu-id="0fe4e-138">Les utilisateurs stockés dans l’application avec l’identité ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-138">Users stored in the app with ASP.NET Core Identity.</span></span>                        | `Individual`             |
| <span data-ttu-id="0fe4e-139">Individuel</span><span class="sxs-lookup"><span data-stu-id="0fe4e-139">Individual</span></span><br><span data-ttu-id="0fe4e-140">Les utilisateurs stockés dans [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span><span class="sxs-lookup"><span data-stu-id="0fe4e-140">Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span> | `IndividualB2C`          |
| <span data-ttu-id="0fe4e-141">Comptes professionnels ou scolaires</span><span class="sxs-lookup"><span data-stu-id="0fe4e-141">Work or School Accounts</span></span><br><span data-ttu-id="0fe4e-142">Authentification d’organisation pour un seul abonné.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-142">Organizational authentication for a single tenant.</span></span>            | `SingleOrg`              |
| <span data-ttu-id="0fe4e-143">Comptes professionnels ou scolaires</span><span class="sxs-lookup"><span data-stu-id="0fe4e-143">Work or School Accounts</span></span><br><span data-ttu-id="0fe4e-144">Authentification d’organisation pour plusieurs abonnés.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-144">Organizational authentication for multiple tenants.</span></span>           | `MultiOrg`               |
| <span data-ttu-id="0fe4e-145">Authentification Windows</span><span class="sxs-lookup"><span data-stu-id="0fe4e-145">Windows Authentication</span></span>                                                                   | `Windows`                |

<span data-ttu-id="0fe4e-146">La commande crée un dossier nommé avec la valeur fournie pour l’espace réservé `{APP NAME}` et utilise le nom de dossier en tant que nom de l’application.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-146">The command creates a folder named with the value provided for the `{APP NAME}` placeholder and uses the folder name as the app's name.</span></span> <span data-ttu-id="0fe4e-147">Pour plus d’informations, consultez la commande [dotnet new](/dotnet/core/tools/dotnet-new) dans le Guide .NET Core.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-147">For more information, see the [dotnet new](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

<!--

# [Visual Studio for Mac](#tab/visual-studio-mac)

1. Follow the Visual Studio for Mac guidance in the <xref:blazor/get-started> article.

1.

1.

-->

<!--
# [.NET Core CLI](#tab/netcore-cli/)

Follow the .NET Core CLI guidance in the <xref:blazor/get-started> article to create a new Blazor server-side project with an authentication mechanism:

```console
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.

| Authentication mechanism                                                                 | `{AUTHENTICATION}` value |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| No Authentication                                                                        | `None`                   |
| Individual<br>Users stored in the app with ASP.NET Core Identity.                        | `Individual`             |
| Individual<br>Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c). | `IndividualB2C`          |
| Work or School Accounts<br>Organizational authentication for a single tenant.            | `SingleOrg`              |
| Work or School Accounts<br>Organizational authentication for multiple tenants.           | `MultiOrg`               |
| Windows Authentication                                                                   | `Windows`                |

The command creates a folder named with the value provided for the `{APP NAME}` placeholder and uses the folder name as the app's name. For more information, see the [dotnet new](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.

-->

---

### <a name="blazor-client-side-authentication"></a><span data-ttu-id="0fe4e-148">Authentification Blazor côté client</span><span class="sxs-lookup"><span data-stu-id="0fe4e-148">Blazor client-side authentication</span></span>

<span data-ttu-id="0fe4e-149">Dans les applications Blazor côté client, les contrôles d’authentification peuvent être contournés, car tout le code côté client peut être modifié par les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-149">In Blazor client-side apps, authentication checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="0fe4e-150">Cela vaut également pour toutes les technologies d’application côté client, y compris les infrastructures d’application JavaScript SPA ou les applications natives pour n’importe quel système d’exploitation.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-150">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="0fe4e-151">L’implémentation d’un service `AuthenticationStateProvider` personnalisé pour les applications Blazor côté client est couverte dans les sections suivantes.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-151">Implementation of a custom `AuthenticationStateProvider` service for Blazor client-side apps is covered in the following sections.</span></span>

## <a name="authenticationstateprovider-service"></a><span data-ttu-id="0fe4e-152">Service AuthenticationStateProvider</span><span class="sxs-lookup"><span data-stu-id="0fe4e-152">AuthenticationStateProvider service</span></span>

<span data-ttu-id="0fe4e-153">Les applications Blazor côté serveur incluent un service `AuthenticationStateProvider` intégré qui obtient des données d’état d’authentification auprès de `HttpContext.User` de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-153">Blazor server-side apps include a built-in `AuthenticationStateProvider` service that obtains authentication state data from ASP.NET Core's `HttpContext.User`.</span></span> <span data-ttu-id="0fe4e-154">Il s’agit de la façon dont l’état d’authentification s’intègre avec les mécanismes d’authentification ASP.NET Core côté serveur existants.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-154">This is how authentication state integrates with existing ASP.NET Core server-side authentication mechanisms.</span></span>

<span data-ttu-id="0fe4e-155">`AuthenticationStateProvider` est le service sous-jacent utilisé par le composant `AuthorizeView` et le composant `CascadingAuthenticationState` pour obtenir l’état d’authentification.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-155">`AuthenticationStateProvider` is the underlying service used by the `AuthorizeView` component and `CascadingAuthenticationState` component to get the authentication state.</span></span>

<span data-ttu-id="0fe4e-156">Vous n’utilisez généralement pas `AuthenticationStateProvider` directement.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-156">You don't typically use `AuthenticationStateProvider` directly.</span></span> <span data-ttu-id="0fe4e-157">Utilisez les approches [Composant AuthorizeView](#authorizeview-component) ou [Tâche<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) décrites plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-157">Use the [AuthorizeView component](#authorizeview-component) or [Task<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) approaches described later in this article.</span></span> <span data-ttu-id="0fe4e-158">Le principal inconvénient de l’utilisation directe de `AuthenticationStateProvider` est que le composant n’est pas automatiquement averti en cas de modifications de données d’état de l’authentification sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-158">The main drawback to using `AuthenticationStateProvider` directly is that the component isn't notified automatically if the underlying authentication state data changes.</span></span>

<span data-ttu-id="0fe4e-159">Le service `AuthenticationStateProvider` peut fournir les données <xref:System.Security.Claims.ClaimsPrincipal> de l’utilisateur actuel, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="0fe4e-159">The `AuthenticationStateProvider` service can provide the current user's <xref:System.Security.Claims.ClaimsPrincipal> data, as shown in the following example:</span></span>

```cshtml
@page "/"
@inject AuthenticationStateProvider AuthenticationStateProvider

<button @onclick="@LogUsername">Write user info to console</button>

@code {
    private async Task LogUsername()
    {
        var authState = await AuthenticationStateProvider.GetAuthenticationStateAsync();
        var user = authState.User;

        if (user.Identity.IsAuthenticated)
        {
            Console.WriteLine($"{user.Identity.Name} is authenticated.");
        }
        else
        {
            Console.WriteLine("The user is NOT authenticated.");
        }
    }
}
```

<span data-ttu-id="0fe4e-160">Si `user.Identity.IsAuthenticated` est `true` et parce que l’utilisateur est <xref:System.Security.Claims.ClaimsPrincipal>, les revendications peuvent être énumérées et l’appartenance aux rôles évaluée.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-160">If `user.Identity.IsAuthenticated` is `true` and because the user is a <xref:System.Security.Claims.ClaimsPrincipal>, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="0fe4e-161">Pour plus d’informations sur l’injection de dépendances et les services, consultez <xref:blazor/dependency-injection> et <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-161">For more information on dependency injection (DI) and services, see <xref:blazor/dependency-injection> and <xref:fundamentals/dependency-injection>.</span></span>

## <a name="implement-a-custom-authenticationstateprovider"></a><span data-ttu-id="0fe4e-162">Implémenter un AuthenticationStateProvider personnalisé</span><span class="sxs-lookup"><span data-stu-id="0fe4e-162">Implement a custom AuthenticationStateProvider</span></span>

<span data-ttu-id="0fe4e-163">Si vous créez une application Blazor côté client ou si les spécifications de votre application requièrent absolument un fournisseur personnalisé, implémentez un fournisseur et substituez `GetAuthenticationStateAsync` :</span><span class="sxs-lookup"><span data-stu-id="0fe4e-163">If you're building a Blazor client-side app or if your app's specification absolutely requires a custom provider, implement a provider and override `GetAuthenticationStateAsync`:</span></span>

```csharp
class CustomAuthStateProvider : AuthenticationStateProvider
{
    public override Task<AuthenticationState> GetAuthenticationStateAsync()
    {
        var identity = new ClaimsIdentity(new[]
        {
            new Claim(ClaimTypes.Name, "mrfibuli"),
        }, "Fake authentication type");

        var user = new ClaimsPrincipal(identity);

        return Task.FromResult(new AuthenticationState(user));
    }
}
```

<span data-ttu-id="0fe4e-164">Le service `CustomAuthStateProvider` est inscrit dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="0fe4e-164">The `CustomAuthStateProvider` service is registered in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddScoped<AuthenticationStateProvider, CustomAuthStateProvider>();
}
```

<span data-ttu-id="0fe4e-165">Avec `CustomAuthStateProvider`, tous les utilisateurs sont authentifiés avec le nom d’utilisateur `mrfibuli`.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-165">Using the `CustomAuthStateProvider`, all users are authenticated with the username `mrfibuli`.</span></span>

## <a name="expose-the-authentication-state-as-a-cascading-parameter"></a><span data-ttu-id="0fe4e-166">Exposer l’état d’authentification comme un paramètre en cascade</span><span class="sxs-lookup"><span data-stu-id="0fe4e-166">Expose the authentication state as a cascading parameter</span></span>

<span data-ttu-id="0fe4e-167">Si les données d’état d’authentification sont requises pour la logique procédurale, par exemple lors d’une action déclenchée par l’utilisateur, obtenez les données d’état d’authentification en définissant un paramètre en cascade de type `Task<AuthenticationState>` :</span><span class="sxs-lookup"><span data-stu-id="0fe4e-167">If authentication state data is required for procedural logic, such as when performing an action triggered by the user, obtain the authentication state data by defining a cascading parameter of type `Task<AuthenticationState>`:</span></span>

```cshtml
@page "/"

<button @onclick="@LogUsername">Log username</button>

@code {
    [CascadingParameter]
    private Task<AuthenticationState> authenticationStateTask { get; set; }

    private async Task LogUsername()
    {
        var authState = await authenticationStateTask;
        var user = authState.User;

        if (user.Identity.IsAuthenticated)
        {
            Console.WriteLine($"{user.Identity.Name} is authenticated.");
        }
        else
        {
            Console.WriteLine("The user is NOT authenticated.");
        }
    }
}
```

<span data-ttu-id="0fe4e-168">Si `user.Identity.IsAuthenticated` est `true`, les revendications peuvent être énumérées et l’appartenance aux rôles évaluée.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-168">If `user.Identity.IsAuthenticated` is `true`, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="0fe4e-169">Configurez `Task<AuthenticationState>` le paramètre en cascade à `AuthorizeRouteView` l' `CascadingAuthenticationState` aide des composants et :</span><span class="sxs-lookup"><span data-stu-id="0fe4e-169">Set up the `Task<AuthenticationState>` cascading parameter using the `AuthorizeRouteView` and `CascadingAuthenticationState` components:</span></span>

```cshtml
<Router AppAssembly="@typeof(Program).Assembly">
    <Found Context="routeData">
        <AuthorizeRouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <CascadingAuthenticationState>
            <LayoutView Layout="@typeof(MainLayout)">
                <p>Sorry, there's nothing at this address.</p>
            </LayoutView>
        </CascadingAuthenticationState>
    </NotFound>
</Router>
```

## <a name="authorization"></a><span data-ttu-id="0fe4e-170">Authorization</span><span class="sxs-lookup"><span data-stu-id="0fe4e-170">Authorization</span></span>

<span data-ttu-id="0fe4e-171">Une fois qu’un utilisateur est authentifié, les règles *d’autorisation* sont appliquées pour contrôler ce que l’utilisateur peut faire.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-171">After a user is authenticated, *authorization* rules are applied to control what the user can do.</span></span>

<span data-ttu-id="0fe4e-172">L’accès est généralement accordé ou refusé selon les conditions suivantes :</span><span class="sxs-lookup"><span data-stu-id="0fe4e-172">Access is typically granted or denied based on whether:</span></span>

* <span data-ttu-id="0fe4e-173">Un utilisateur est authentifié (connecté).</span><span class="sxs-lookup"><span data-stu-id="0fe4e-173">A user is authenticated (signed in).</span></span>
* <span data-ttu-id="0fe4e-174">Un utilisateur appartient à un *rôle*.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-174">A user is in a *role*.</span></span>
* <span data-ttu-id="0fe4e-175">Un utilisateur a une *revendication*.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-175">A user has a *claim*.</span></span>
* <span data-ttu-id="0fe4e-176">Une *stratégie* est satisfaite.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-176">A *policy* is satisfied.</span></span>

<span data-ttu-id="0fe4e-177">Tous ces concepts sont les mêmes que dans une application ASP.NET Core MVC ou Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-177">Each of these concepts is the same as in an ASP.NET Core MVC or Razor Pages app.</span></span> <span data-ttu-id="0fe4e-178">Pour plus d’informations sur la sécurité d’ASP.NET Core, consultez les articles sous [Sécurité et identité dans ASP.NET Core](xref:security/index).</span><span class="sxs-lookup"><span data-stu-id="0fe4e-178">For more information on ASP.NET Core security, see the articles under [ASP.NET Core Security and Identity](xref:security/index).</span></span>

## <a name="authorizeview-component"></a><span data-ttu-id="0fe4e-179">Composant AuthorizeView</span><span class="sxs-lookup"><span data-stu-id="0fe4e-179">AuthorizeView component</span></span>

<span data-ttu-id="0fe4e-180">Le composant `AuthorizeView` affiche sélectivement l’interface utilisateur en fonction de l’autorisation que l’utilisateur a pour l’afficher.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-180">The `AuthorizeView` component selectively displays UI depending on whether the user is authorized to see it.</span></span> <span data-ttu-id="0fe4e-181">Cette approche est utile lorsque vous devez uniquement *afficher* les données de l’utilisateur et que vous n’avez pas besoin d’utiliser l’identité de l’utilisateur dans la logique procédurale.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-181">This approach is useful when you only need to *display* data for the user and don't need to use the user's identity in procedural logic.</span></span>

<span data-ttu-id="0fe4e-182">Le composant expose une variable `context` de type `AuthenticationState`, que vous pouvez utiliser pour accéder aux informations relatives à l’utilisateur connecté :</span><span class="sxs-lookup"><span data-stu-id="0fe4e-182">The component exposes a `context` variable of type `AuthenticationState`, which you can use to access information about the signed-in user:</span></span>

```cshtml
<AuthorizeView>
    <h1>Hello, @context.User.Identity.Name!</h1>
    <p>You can only see this content if you're authenticated.</p>
</AuthorizeView>
```

<span data-ttu-id="0fe4e-183">Vous pouvez également fournir un contenu différent à afficher si l’utilisateur n’est pas authentifié :</span><span class="sxs-lookup"><span data-stu-id="0fe4e-183">You can also supply different content for display if the user isn't authenticated:</span></span>

```cshtml
<AuthorizeView>
    <Authorized>
        <h1>Hello, @context.User.Identity.Name!</h1>
        <p>You can only see this content if you're authenticated.</p>
    </Authorized>
    <NotAuthorized>
        <h1>Authentication Failure!</h1>
        <p>You're not signed in.</p>
    </NotAuthorized>
</AuthorizeView>
```

<span data-ttu-id="0fe4e-184">Le contenu des `<Authorized>` balises et `<NotAuthorized>` peut inclure des éléments arbitraires, tels que d’autres composants interactifs.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-184">The content of `<Authorized>` and `<NotAuthorized>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="0fe4e-185">Les conditions d’autorisation, comme les rôles ou les stratégies qui contrôlent les options d’interface utilisateur ou d’accès, sont traitées dans la section [Autorisation](#authorization).</span><span class="sxs-lookup"><span data-stu-id="0fe4e-185">Authorization conditions, such as roles or policies that control UI options or access, are covered in the [Authorization](#authorization) section.</span></span>

<span data-ttu-id="0fe4e-186">Si les conditions d’autorisation ne sont pas spécifiées, `AuthorizeView` utilise une stratégie par défaut et traite :</span><span class="sxs-lookup"><span data-stu-id="0fe4e-186">If authorization conditions aren't specified, `AuthorizeView` uses a default policy and treats:</span></span>

* <span data-ttu-id="0fe4e-187">Les utilisateurs authentifiés (connectés) comme étant autorisés.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-187">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="0fe4e-188">Les utilisateurs non authentifiés (déconnectés) comme étant non autorisés.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-188">Unauthenticated (signed-out) users as unauthorized.</span></span>

### <a name="role-based-and-policy-based-authorization"></a><span data-ttu-id="0fe4e-189">Autorisation en fonction du rôle et de la stratégie</span><span class="sxs-lookup"><span data-stu-id="0fe4e-189">Role-based and policy-based authorization</span></span>

<span data-ttu-id="0fe4e-190">Le composant `AuthorizeView` prend en charge l’autorisation *basée sur le rôle* et *basée sur les stratégies*.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-190">The `AuthorizeView` component supports *role-based* or *policy-based* authorization.</span></span>

<span data-ttu-id="0fe4e-191">Pour l’autorisation en fonction du rôle, utilisez le paramètre `Roles` :</span><span class="sxs-lookup"><span data-stu-id="0fe4e-191">For role-based authorization, use the `Roles` parameter:</span></span>

```cshtml
<AuthorizeView Roles="admin, superuser">
    <p>You can only see this if you're an admin or superuser.</p>
</AuthorizeView>
```

<span data-ttu-id="0fe4e-192">Pour plus d'informations, consultez <xref:security/authorization/roles>.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-192">For more information, see <xref:security/authorization/roles>.</span></span>

<span data-ttu-id="0fe4e-193">Pour l’autorisation en fonction des stratégies, utilisez le paramètre `Policy` :</span><span class="sxs-lookup"><span data-stu-id="0fe4e-193">For policy-based authorization, use the `Policy` parameter:</span></span>

```cshtml
<AuthorizeView Policy="content-editor">
    <p>You can only see this if you satisfy the "content-editor" policy.</p>
</AuthorizeView>
```

<span data-ttu-id="0fe4e-194">L’autorisation basée sur les revendications est un cas spécial d’autorisation basée sur les stratégies.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-194">Claims-based authorization is a special case of policy-based authorization.</span></span> <span data-ttu-id="0fe4e-195">Par exemple, vous pouvez définir une stratégie qui impose aux utilisateurs d’avoir une certaine revendication.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-195">For example, you can define a policy that requires users to have a certain claim.</span></span> <span data-ttu-id="0fe4e-196">Pour plus d'informations, consultez <xref:security/authorization/policies>.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-196">For more information, see <xref:security/authorization/policies>.</span></span>

<span data-ttu-id="0fe4e-197">Ces API peuvent être utilisées dans les applications Blazor côté serveur ou client.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-197">These APIs can be used in either Blazor server-side or Blazor client-side apps.</span></span>

<span data-ttu-id="0fe4e-198">Si ni `Roles` ni `Policy` n’est spécifié, `AuthorizeView` utilise la stratégie par défaut.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-198">If neither `Roles` nor `Policy` is specified, `AuthorizeView` uses the default policy.</span></span>

### <a name="content-displayed-during-asynchronous-authentication"></a><span data-ttu-id="0fe4e-199">Contenu affiché lors de l’authentification asynchrone</span><span class="sxs-lookup"><span data-stu-id="0fe4e-199">Content displayed during asynchronous authentication</span></span>

<span data-ttu-id="0fe4e-200">Blazor permet de déterminer l’état d’authentification *de façon asynchrone*.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-200">Blazor allows for authentication state to be determined *asynchronously*.</span></span> <span data-ttu-id="0fe4e-201">Le scénario principal de cette approche est dans les applications Blazor côté client qui créent une requête d’authentification sur un point de terminaison externe.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-201">The primary scenario for this approach is in Blazor client-side apps that make a request to an external endpoint for authentication.</span></span>

<span data-ttu-id="0fe4e-202">Lorsque l’authentification est en cours, `AuthorizeView` n’affiche aucun contenu par défaut.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-202">While authentication is in progress, `AuthorizeView` displays no content by default.</span></span> <span data-ttu-id="0fe4e-203">Pour afficher le contenu pendant que l’authentification se produit, utilisez l’élément `<Authorizing>` :</span><span class="sxs-lookup"><span data-stu-id="0fe4e-203">To display content while authentication occurs, use the `<Authorizing>` element:</span></span>

```cshtml
<AuthorizeView>
    <Authorized>
        <h1>Hello, @context.User.Identity.Name!</h1>
        <p>You can only see this content if you're authenticated.</p>
    </Authorized>
    <Authorizing>
        <h1>Authentication in progress</h1>
        <p>You can only see this content while authentication is in progress.</p>
    </Authorizing>
</AuthorizeView>
```

<span data-ttu-id="0fe4e-204">Cette approche n’est pas normalement applicable aux applications Blazor côté serveur.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-204">This approach isn't normally applicable to Blazor server-side apps.</span></span> <span data-ttu-id="0fe4e-205">Les applications Blazor côté serveur connaissent l’état de l’authentification dès que l’état est établi.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-205">Blazor server-side apps know the authentication state as soon as the state is established.</span></span> <span data-ttu-id="0fe4e-206">Le contenu de `Authorizing` peut être fourni dans le compsosant `AuthorizeView` d’une application Blazor côté serveur, mais le contenu n’est jamais affiché.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-206">`Authorizing` content can be provided in a Blazor server-side app's `AuthorizeView` component, but the content is never displayed.</span></span>

## <a name="authorize-attribute"></a><span data-ttu-id="0fe4e-207">Attribut [Authorize]</span><span class="sxs-lookup"><span data-stu-id="0fe4e-207">[Authorize] attribute</span></span>

<span data-ttu-id="0fe4e-208">Tout comme une application peut utiliser `[Authorize]` avec un contrôleur MVC ou une page Razor, `[Authorize]` peut également servir avec les Razor Components :</span><span class="sxs-lookup"><span data-stu-id="0fe4e-208">Just like an app can use `[Authorize]` with an MVC controller or Razor page, `[Authorize]` can also be used with Razor Components:</span></span>

```cshtml
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!IMPORTANT]
> <span data-ttu-id="0fe4e-209">Utilisez uniquement `[Authorize]` sur les composants `@page` atteints via le routeur Blazor.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-209">Only use `[Authorize]` on `@page` components reached via the Blazor Router.</span></span> <span data-ttu-id="0fe4e-210">L’autorisation est effectuée uniquement en tant qu’aspect du routage et *pas* pour les composants enfants rendus dans une page.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-210">Authorization is only performed as an aspect of routing and *not* for child components rendered within a page.</span></span> <span data-ttu-id="0fe4e-211">Pour autoriser l’affichage d’éléments spécifiques dans une page, utilisez `AuthorizeView` à la place.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-211">To authorize the display of specific parts within a page, use `AuthorizeView` instead.</span></span>

<span data-ttu-id="0fe4e-212">Vous devrez peut-être ajouter `@using Microsoft.AspNetCore.Authorization` au composant ou au fichier *_Imports.razor* pour que le composant compile.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-212">You may need to add `@using Microsoft.AspNetCore.Authorization` either to the component or to the *_Imports.razor* file in order for the component to compile.</span></span>

<span data-ttu-id="0fe4e-213">L’attribut `[Authorize]` prend également en charge l’autorisation en fonction du rôle ou des stratégies.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-213">The `[Authorize]` attribute also supports role-based or policy-based authorization.</span></span> <span data-ttu-id="0fe4e-214">Pour l’autorisation en fonction du rôle, utilisez le paramètre `Roles` :</span><span class="sxs-lookup"><span data-stu-id="0fe4e-214">For role-based authorization, use the `Roles` parameter:</span></span>

```cshtml
@page "/"
@attribute [Authorize(Roles = "admin, superuser")]

<p>You can only see this if you're in the 'admin' or 'superuser' role.</p>
```

<span data-ttu-id="0fe4e-215">Pour l’autorisation en fonction des stratégies, utilisez le paramètre `Policy` :</span><span class="sxs-lookup"><span data-stu-id="0fe4e-215">For policy-based authorization, use the `Policy` parameter:</span></span>

```cshtml
@page "/"
@attribute [Authorize(Policy = "content-editor")]

<p>You can only see this if you satisfy the 'content-editor' policy.</p>
```

<span data-ttu-id="0fe4e-216">Si ni `Roles` ni `Policy` n’est spécifié, `[Authorize]` utilise la stratégie par défaut, qui consiste par défaut à traiter :</span><span class="sxs-lookup"><span data-stu-id="0fe4e-216">If neither `Roles` nor `Policy` is specified, `[Authorize]` uses the default policy, which by default is to treat:</span></span>

* <span data-ttu-id="0fe4e-217">Les utilisateurs authentifiés (connectés) comme étant autorisés.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-217">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="0fe4e-218">Les utilisateurs non authentifiés (déconnectés) comme étant non autorisés.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-218">Unauthenticated (signed-out) users as unauthorized.</span></span>

## <a name="customize-unauthorized-content-with-the-router-component"></a><span data-ttu-id="0fe4e-219">Personnaliser le contenu non autorisé avec le composant Router</span><span class="sxs-lookup"><span data-stu-id="0fe4e-219">Customize unauthorized content with the Router component</span></span>

<span data-ttu-id="0fe4e-220">Le `Router` composant, conjointement avec le `AuthorizeRouteView` composant, permet à l’application de spécifier du contenu personnalisé dans les cas suivants :</span><span class="sxs-lookup"><span data-stu-id="0fe4e-220">The `Router` component, in conjunction with the `AuthorizeRouteView` component, allows the app to specify custom content if:</span></span>

* <span data-ttu-id="0fe4e-221">Le contenu est introuvable.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-221">Content isn't found.</span></span>
* <span data-ttu-id="0fe4e-222">L’utilisateur ne répond pas à une condition `[Authorize]` appliquée au composant.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-222">The user fails an `[Authorize]` condition applied to the component.</span></span> <span data-ttu-id="0fe4e-223">L’attribut `[Authorize]` est couvert dans la section [Attribut [Authorize]](#authorize-attribute).</span><span class="sxs-lookup"><span data-stu-id="0fe4e-223">The `[Authorize]` attribute is covered in the [[Authorize] attribute](#authorize-attribute) section.</span></span>
* <span data-ttu-id="0fe4e-224">Une authentification asynchrone est en cours.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-224">Asynchronous authentication is in progress.</span></span>

<span data-ttu-id="0fe4e-225">Dans le modèle de projet Blazor côté serveur par défaut, le fichier *App.razor* montre comment définir le contenu personnalisé :</span><span class="sxs-lookup"><span data-stu-id="0fe4e-225">In the default Blazor server-side project template, the *App.razor* file demonstrates how to set custom content:</span></span>

```cshtml
<Router AppAssembly="@typeof(Program).Assembly">
    <Found Context="routeData">
        <AuthorizeRouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)">
            <NotAuthorized>
                <h1>Sorry</h1>
                <p>You're not authorized to reach this page.</p>
                <p>You may need to log in as a different user.</p>
            </NotAuthorized>
            <Authorizing>
                <h1>Authentication in progress</h1>
                <p>Only visible while authentication is in progress.</p>
            </Authorizing>
        </AuthorizeRouteView>
    </Found>
    <NotFound>
        <CascadingAuthenticationState>
            <LayoutView Layout="@typeof(MainLayout)">
                <h1>Sorry</h1>
                <p>Sorry, there's nothing at this address.</p>
            </LayoutView>
        </CascadingAuthenticationState>
    </NotFound>
</Router>
```

<span data-ttu-id="0fe4e-226">Le contenu des `<NotFound>`balises, `<Authorizing>` `<NotAuthorized>`et peut inclure des éléments arbitraires, tels que d’autres composants interactifs.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-226">The content of `<NotFound>`, `<NotAuthorized>`, and `<Authorizing>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="0fe4e-227">Si l' `<NotAuthorized>` élément n’est pas spécifié `AuthorizeRouteView` , le utilise le message de secours suivant :</span><span class="sxs-lookup"><span data-stu-id="0fe4e-227">If the `<NotAuthorized>` element isn't specified, the `AuthorizeRouteView` uses the following fallback message:</span></span>

```html
Not authorized.
```

## <a name="notification-about-authentication-state-changes"></a><span data-ttu-id="0fe4e-228">Notification sur les changements d’état de l’authentification</span><span class="sxs-lookup"><span data-stu-id="0fe4e-228">Notification about authentication state changes</span></span>

<span data-ttu-id="0fe4e-229">Si l’application détermine que les données d’état d’authentification sous-jacentes ont changé (par exemple, parce que l’utilisateur s’est déconnecté ou qu’un autre utilisateur a modifié ses rôles), un `AuthenticationStateProvider` personnalisé peut éventuellement appeler la méthode `NotifyAuthenticationStateChanged` sur la classe de base `AuthenticationStateProvider`.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-229">If the app determines that the underlying authentication state data has changed (for example, because the user signed out or another user has changed their roles), a custom `AuthenticationStateProvider` can optionally invoke the method `NotifyAuthenticationStateChanged` on the `AuthenticationStateProvider` base class.</span></span> <span data-ttu-id="0fe4e-230">Cela prévient les consommateurs des données d’état d’authentification (par exemple, `AuthorizeView`) qu’elles doivent appliquer un nouveau rendu à l’aide des nouvelles données.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-230">This notifies consumers of the authentication state data (for example, `AuthorizeView`) to rerender using the new data.</span></span>

## <a name="procedural-logic"></a><span data-ttu-id="0fe4e-231">Logique procédurale</span><span class="sxs-lookup"><span data-stu-id="0fe4e-231">Procedural logic</span></span>

<span data-ttu-id="0fe4e-232">Si l’application est nécessaire pour vérifier les règles d’autorisation dans le cadre de la logique procédurale, utilisez un paramètre en cascade de type `Task<AuthenticationState>` pour obtenir le <xref:System.Security.Claims.ClaimsPrincipal> de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-232">If the app is required to check authorization rules as part of procedural logic, use a cascaded parameter of type `Task<AuthenticationState>` to obtain the user's <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="0fe4e-233">`Task<AuthenticationState>` peut être combiné avec d’autres services, comme `IAuthorizationService`, pour évaluer les stratégies.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-233">`Task<AuthenticationState>` can be combined with other services, such as `IAuthorizationService`, to evaluate policies.</span></span>

```cshtml
@inject IAuthorizationService AuthorizationService

<button @onclick="@DoSomething">Do something important</button>

@code {
    [CascadingParameter]
    private Task<AuthenticationState> authenticationStateTask { get; set; }

    private async Task DoSomething()
    {
        var user = (await authenticationStateTask).User;

        if (user.Identity.IsAuthenticated)
        {
            // Perform an action only available to authenticated (signed-in) users.
        }

        if (user.IsInRole("admin"))
        {
            // Perform an action only available to users in the 'admin' role.
        }

        if ((await AuthorizationService.AuthorizeAsync(user, "content-editor"))
            .Succeeded)
        {
            // Perform an action only available to users satisfying the 
            // 'content-editor' policy.
        }
    }
}
```

## <a name="authorization-in-blazor-client-side-apps"></a><span data-ttu-id="0fe4e-234">Autorisation dans les applications Blazor côté client</span><span class="sxs-lookup"><span data-stu-id="0fe4e-234">Authorization in Blazor client-side apps</span></span>

<span data-ttu-id="0fe4e-235">Dans les applications Blazor côté client, les contrôles d’autorisation peuvent être contournés, car tout le code côté client peut être modifié par les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-235">In Blazor client-side apps, authorization checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="0fe4e-236">Cela vaut également pour toutes les technologies d’application côté client, y compris les infrastructures d’application JavaScript SPA ou les applications natives pour n’importe quel système d’exploitation.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-236">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="0fe4e-237">**Effectuez toujours les vérifications d’autorisation sur le serveur au sein des points de terminaison de l’API auxquels votre application côté client accède.**</span><span class="sxs-lookup"><span data-stu-id="0fe4e-237">**Always perform authorization checks on the server within any API endpoints accessed by your client-side app.**</span></span>

## <a name="troubleshoot-errors"></a><span data-ttu-id="0fe4e-238">Résoudre les erreurs</span><span class="sxs-lookup"><span data-stu-id="0fe4e-238">Troubleshoot errors</span></span>

<span data-ttu-id="0fe4e-239">Erreurs courantes :</span><span class="sxs-lookup"><span data-stu-id="0fe4e-239">Common errors:</span></span>

* <span data-ttu-id="0fe4e-240">**L’autorisation nécessite un paramètre en cascade de type Task\<AuthenticationState>. Envisagez d’utiliser CascadingAuthenticationState pour fournir cela.**</span><span class="sxs-lookup"><span data-stu-id="0fe4e-240">**Authorization requires a cascading parameter of type Task\<AuthenticationState>. Consider using CascadingAuthenticationState to supply this.**</span></span>

* <span data-ttu-id="0fe4e-241">**La valeur `null` est reçue pour `authenticationStateTask`**</span><span class="sxs-lookup"><span data-stu-id="0fe4e-241">**`null` value is received for `authenticationStateTask`**</span></span>

<span data-ttu-id="0fe4e-242">Il est probable que le projet n’a pas été créé à l’aide d’un modèle Blazor côté serveur avec l’authentification activée.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-242">It's likely that the project wasn't created using a Blazor server-side template with authentication enabled.</span></span> <span data-ttu-id="0fe4e-243">Encapsulez un `<CascadingAuthenticationState>` dans certaines parties de l’arborescence de l’interface utilisateur, par exemple dans *App.razor*, comme suit :</span><span class="sxs-lookup"><span data-stu-id="0fe4e-243">Wrap a `<CascadingAuthenticationState>` around some part of the UI tree, for example in *App.razor* as follows:</span></span>

```cshtml
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CascadingAuthenticationState>
```

<span data-ttu-id="0fe4e-244">`CascadingAuthenticationState` fournit le paramètre en cascade `Task<AuthenticationState>`, qu’il reçoit à son tour du service d’injection de dépendances `AuthenticationStateProvider` sous-jacent.</span><span class="sxs-lookup"><span data-stu-id="0fe4e-244">The `CascadingAuthenticationState` supplies the `Task<AuthenticationState>` cascading parameter, which in turn it receives from the underlying `AuthenticationStateProvider` DI service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0fe4e-245">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="0fe4e-245">Additional resources</span></span>

* <xref:security/index>
* <xref:security/blazor/server-side>
* <xref:security/authentication/windowsauth>
