---
title: ASP.NET Core Blazor l’authentification et l’autorisation
author: guardrex
description: En savoir plus sur les scénarios d’authentification et d’autorisation Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- Blazor
- SignalR
uid: security/blazor/index
ms.openlocfilehash: 693ac1a5b5bcaf8a9bbf0ff9ab63fb41764e3888
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880455"
---
# <a name="aspnet-core-opno-locblazor-authentication-and-authorization"></a><span data-ttu-id="c773b-103">ASP.NET Core Blazor l’authentification et l’autorisation</span><span class="sxs-lookup"><span data-stu-id="c773b-103">ASP.NET Core Blazor authentication and authorization</span></span>

<span data-ttu-id="c773b-104">Par [Steve Sanderson](https://github.com/SteveSandersonMS)</span><span class="sxs-lookup"><span data-stu-id="c773b-104">By [Steve Sanderson](https://github.com/SteveSandersonMS)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="c773b-105">ASP.NET Core prend en charge la configuration et la gestion de la sécurité dans les applications Blazor.</span><span class="sxs-lookup"><span data-stu-id="c773b-105">ASP.NET Core supports the configuration and management of security in Blazor apps.</span></span>

<span data-ttu-id="c773b-106">Les scénarios de sécurité diffèrent entre les applications Blazor Server et Blazor webassembly.</span><span class="sxs-lookup"><span data-stu-id="c773b-106">Security scenarios differ between Blazor Server and Blazor WebAssembly apps.</span></span> <span data-ttu-id="c773b-107">Étant donné que les applications Blazor Server s’exécutent sur le serveur, les contrôles d’autorisation peuvent déterminer les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="c773b-107">Because Blazor Server apps run on the server, authorization checks are able to determine:</span></span>

* <span data-ttu-id="c773b-108">Les options de l’interface utilisateur présentées à un utilisateur (par exemple, les entrées de menu disponibles pour un utilisateur).</span><span class="sxs-lookup"><span data-stu-id="c773b-108">The UI options presented to a user (for example, which menu entries are available to a user).</span></span>
* <span data-ttu-id="c773b-109">Les règles d’accès pour les zones de l’application et les composants.</span><span class="sxs-lookup"><span data-stu-id="c773b-109">Access rules for areas of the app and components.</span></span>

Blazor<span data-ttu-id="c773b-110"> applications webassembly s’exécutent sur le client.</span><span class="sxs-lookup"><span data-stu-id="c773b-110"> WebAssembly apps run on the client.</span></span> <span data-ttu-id="c773b-111">L’autorisation est *uniquement* utilisée pour déterminer les options de l’interface utilisateur à afficher.</span><span class="sxs-lookup"><span data-stu-id="c773b-111">Authorization is *only* used to determine which UI options to show.</span></span> <span data-ttu-id="c773b-112">Étant donné que les contrôles côté client peuvent être modifiés ou ignorés par un utilisateur, une application Blazor webassembly ne peut pas appliquer les règles d’accès d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="c773b-112">Since client-side checks can be modified or bypassed by a user, a Blazor WebAssembly app can't enforce authorization access rules.</span></span>

## <a name="authentication"></a><span data-ttu-id="c773b-113">Authentification</span><span class="sxs-lookup"><span data-stu-id="c773b-113">Authentication</span></span>

Blazor<span data-ttu-id="c773b-114"> utilise les mécanismes d’authentification ASP.NET Core existants pour établir l’identité de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="c773b-114"> uses the existing ASP.NET Core authentication mechanisms to establish the user's identity.</span></span> <span data-ttu-id="c773b-115">Le mécanisme exact dépend de la façon dont l’application Blazor est hébergée, Blazor Server ou Blazor webassembly.</span><span class="sxs-lookup"><span data-stu-id="c773b-115">The exact mechanism depends on how the Blazor app is hosted, Blazor Server or Blazor WebAssembly.</span></span>

### <a name="opno-locblazor-server-authentication"></a><span data-ttu-id="c773b-116">authentification du serveur de Blazor</span><span class="sxs-lookup"><span data-stu-id="c773b-116">Blazor Server authentication</span></span>

Blazor<span data-ttu-id="c773b-117"> applications serveur fonctionnent sur une connexion en temps réel créée à l’aide de SignalR.</span><span class="sxs-lookup"><span data-stu-id="c773b-117"> Server apps operate over a real-time connection that's created using SignalR.</span></span> <span data-ttu-id="c773b-118">L' [authentification dans les applications basées sur des SignalR](xref:signalr/authn-and-authz) est gérée lorsque la connexion est établie.</span><span class="sxs-lookup"><span data-stu-id="c773b-118">[Authentication in SignalR-based apps](xref:signalr/authn-and-authz) is handled when the connection is established.</span></span> <span data-ttu-id="c773b-119">L’authentification peut reposer sur un cookie ou un autre jeton du porteur.</span><span class="sxs-lookup"><span data-stu-id="c773b-119">Authentication can be based on a cookie or some other bearer token.</span></span>

<span data-ttu-id="c773b-120">Le modèle de projet Blazor Server peut configurer l’authentification à votre place lors de la création du projet.</span><span class="sxs-lookup"><span data-stu-id="c773b-120">The Blazor Server project template can set up authentication for you when the project is created.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c773b-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c773b-121">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c773b-122">Suivez les instructions de Visual Studio dans l’article <xref:blazor/get-started> pour créer un nouveau projet Blazor Server avec un mécanisme d’authentification.</span><span class="sxs-lookup"><span data-stu-id="c773b-122">Follow the Visual Studio guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism.</span></span>

<span data-ttu-id="c773b-123">Après avoir choisi le modèle d’application **Blazor Server** dans la boîte de dialogue **créer une nouvelle application Web ASP.net Core** , sélectionnez **modifier** sous **authentification**.</span><span class="sxs-lookup"><span data-stu-id="c773b-123">After choosing the **Blazor Server App** template in the **Create a new ASP.NET Core Web Application** dialog, select **Change** under **Authentication**.</span></span>

<span data-ttu-id="c773b-124">Une boîte de dialogue s’ouvre pour offrir le même ensemble de mécanismes d’authentification que ceux disponibles pour les autres projets ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="c773b-124">A dialog opens to offer the same set of authentication mechanisms available for other ASP.NET Core projects:</span></span>

* <span data-ttu-id="c773b-125">**Aucune authentification**</span><span class="sxs-lookup"><span data-stu-id="c773b-125">**No Authentication**</span></span>
* <span data-ttu-id="c773b-126">**Comptes d’utilisateur individuels** &ndash; Les comptes d'utilisateur peuvent être stockés :</span><span class="sxs-lookup"><span data-stu-id="c773b-126">**Individual User Accounts** &ndash; User accounts can be stored:</span></span>
  * <span data-ttu-id="c773b-127">Dans l’application à l’aide du système [d’identité](xref:security/authentication/identity) d’ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c773b-127">Within the app using ASP.NET Core's [Identity](xref:security/authentication/identity) system.</span></span>
  * <span data-ttu-id="c773b-128">Avec [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span><span class="sxs-lookup"><span data-stu-id="c773b-128">With [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span>
* <span data-ttu-id="c773b-129">**Comptes professionnels ou scolaires**</span><span class="sxs-lookup"><span data-stu-id="c773b-129">**Work or School Accounts**</span></span>
* <span data-ttu-id="c773b-130">**Authentification Windows**</span><span class="sxs-lookup"><span data-stu-id="c773b-130">**Windows Authentication**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c773b-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c773b-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="c773b-132">Pour créer un projet de serveur Blazor avec un mécanisme d’authentification, suivez les instructions de Visual Studio Code dans l’article <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="c773b-132">Follow the Visual Studio Code guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism:</span></span>

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

<span data-ttu-id="c773b-133">Les valeurs autorisées d’authentification (`{AUTHENTICATION}`) sont présentées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="c773b-133">Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.</span></span>

| <span data-ttu-id="c773b-134">Mécanisme d’authentification</span><span class="sxs-lookup"><span data-stu-id="c773b-134">Authentication mechanism</span></span>                                                                 | <span data-ttu-id="c773b-135">Valeur de `{AUTHENTICATION}`</span><span class="sxs-lookup"><span data-stu-id="c773b-135">`{AUTHENTICATION}` value</span></span> |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| <span data-ttu-id="c773b-136">Aucune authentification</span><span class="sxs-lookup"><span data-stu-id="c773b-136">No Authentication</span></span>                                                                        | `None`                   |
| <span data-ttu-id="c773b-137">Individuel</span><span class="sxs-lookup"><span data-stu-id="c773b-137">Individual</span></span><br><span data-ttu-id="c773b-138">Les utilisateurs stockés dans l’application avec l’identité ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c773b-138">Users stored in the app with ASP.NET Core Identity.</span></span>                        | `Individual`             |
| <span data-ttu-id="c773b-139">Individuel</span><span class="sxs-lookup"><span data-stu-id="c773b-139">Individual</span></span><br><span data-ttu-id="c773b-140">Les utilisateurs stockés dans [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span><span class="sxs-lookup"><span data-stu-id="c773b-140">Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span> | `IndividualB2C`          |
| <span data-ttu-id="c773b-141">Comptes professionnels ou scolaires</span><span class="sxs-lookup"><span data-stu-id="c773b-141">Work or School Accounts</span></span><br><span data-ttu-id="c773b-142">Authentification d’organisation pour un seul abonné.</span><span class="sxs-lookup"><span data-stu-id="c773b-142">Organizational authentication for a single tenant.</span></span>            | `SingleOrg`              |
| <span data-ttu-id="c773b-143">Comptes professionnels ou scolaires</span><span class="sxs-lookup"><span data-stu-id="c773b-143">Work or School Accounts</span></span><br><span data-ttu-id="c773b-144">Authentification d’organisation pour plusieurs abonnés.</span><span class="sxs-lookup"><span data-stu-id="c773b-144">Organizational authentication for multiple tenants.</span></span>           | `MultiOrg`               |
| <span data-ttu-id="c773b-145">Authentification Windows</span><span class="sxs-lookup"><span data-stu-id="c773b-145">Windows Authentication</span></span>                                                                   | `Windows`                |

<span data-ttu-id="c773b-146">La commande crée un dossier nommé avec la valeur fournie pour l’espace réservé `{APP NAME}` et utilise le nom de dossier en tant que nom de l’application.</span><span class="sxs-lookup"><span data-stu-id="c773b-146">The command creates a folder named with the value provided for the `{APP NAME}` placeholder and uses the folder name as the app's name.</span></span> <span data-ttu-id="c773b-147">Pour plus d’informations, consultez la commande [dotnet new](/dotnet/core/tools/dotnet-new) dans le Guide .NET Core.</span><span class="sxs-lookup"><span data-stu-id="c773b-147">For more information, see the [dotnet new](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

<!--

# [Visual Studio for Mac](#tab/visual-studio-mac)

1. Follow the Visual Studio for Mac guidance in the <xref:blazor/get-started> article.

1.

1.

-->

<!--
# [.NET Core CLI](#tab/netcore-cli/)

Follow the .NET Core CLI guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism:

```dotnetcli
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

### <a name="opno-locblazor-webassembly-authentication"></a>Blazor<span data-ttu-id="c773b-148"> l’authentification webassembly</span><span class="sxs-lookup"><span data-stu-id="c773b-148"> WebAssembly authentication</span></span>

<span data-ttu-id="c773b-149">Dans Blazor applications webassembly, les vérifications d’authentification peuvent être contournées, car le code côté client peut être modifié par les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="c773b-149">In Blazor WebAssembly apps, authentication checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="c773b-150">Cela vaut également pour toutes les technologies d’application côté client, y compris les infrastructures d’application JavaScript SPA ou les applications natives pour n’importe quel système d’exploitation.</span><span class="sxs-lookup"><span data-stu-id="c773b-150">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="c773b-151">Ajoutez une référence de package pour [Microsoft. AspNetCore. Components. Authorization](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/) au fichier projet de l’application.</span><span class="sxs-lookup"><span data-stu-id="c773b-151">Add a package reference for [Microsoft.AspNetCore.Components.Authorization](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/) to the app's project file.</span></span>

<span data-ttu-id="c773b-152">L’implémentation d’un service de `AuthenticationStateProvider` personnalisé pour Blazor applications webassembly est traitée dans les sections suivantes.</span><span class="sxs-lookup"><span data-stu-id="c773b-152">Implementation of a custom `AuthenticationStateProvider` service for Blazor WebAssembly apps is covered in the following sections.</span></span>

## <a name="authenticationstateprovider-service"></a><span data-ttu-id="c773b-153">Service AuthenticationStateProvider</span><span class="sxs-lookup"><span data-stu-id="c773b-153">AuthenticationStateProvider service</span></span>

Blazor<span data-ttu-id="c773b-154"> applications serveur incluent un service `AuthenticationStateProvider` intégré qui obtient des données d’état d’authentification du `HttpContext.User`de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c773b-154"> Server apps include a built-in `AuthenticationStateProvider` service that obtains authentication state data from ASP.NET Core's `HttpContext.User`.</span></span> <span data-ttu-id="c773b-155">Il s’agit de la façon dont l’état d’authentification s’intègre avec les mécanismes d’authentification ASP.NET Core côté serveur existants.</span><span class="sxs-lookup"><span data-stu-id="c773b-155">This is how authentication state integrates with existing ASP.NET Core server-side authentication mechanisms.</span></span>

<span data-ttu-id="c773b-156">`AuthenticationStateProvider` est le service sous-jacent utilisé par le composant `AuthorizeView` et le composant `CascadingAuthenticationState` pour obtenir l’état d’authentification.</span><span class="sxs-lookup"><span data-stu-id="c773b-156">`AuthenticationStateProvider` is the underlying service used by the `AuthorizeView` component and `CascadingAuthenticationState` component to get the authentication state.</span></span>

<span data-ttu-id="c773b-157">Vous n’utilisez généralement pas `AuthenticationStateProvider` directement.</span><span class="sxs-lookup"><span data-stu-id="c773b-157">You don't typically use `AuthenticationStateProvider` directly.</span></span> <span data-ttu-id="c773b-158">Utilisez les approches [Composant AuthorizeView](#authorizeview-component) ou [Tâche<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) décrites plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="c773b-158">Use the [AuthorizeView component](#authorizeview-component) or [Task<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) approaches described later in this article.</span></span> <span data-ttu-id="c773b-159">Le principal inconvénient de l’utilisation directe de `AuthenticationStateProvider` est que le composant n’est pas automatiquement averti en cas de modifications de données d’état de l’authentification sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="c773b-159">The main drawback to using `AuthenticationStateProvider` directly is that the component isn't notified automatically if the underlying authentication state data changes.</span></span>

<span data-ttu-id="c773b-160">Le service `AuthenticationStateProvider` peut fournir les données <xref:System.Security.Claims.ClaimsPrincipal> de l’utilisateur actuel, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="c773b-160">The `AuthenticationStateProvider` service can provide the current user's <xref:System.Security.Claims.ClaimsPrincipal> data, as shown in the following example:</span></span>

```cshtml
@page "/"
@using Microsoft.AspNetCore.Components.Authorization
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

<span data-ttu-id="c773b-161">Si `user.Identity.IsAuthenticated` est `true` et parce que l’utilisateur est <xref:System.Security.Claims.ClaimsPrincipal>, les revendications peuvent être énumérées et l’appartenance aux rôles évaluée.</span><span class="sxs-lookup"><span data-stu-id="c773b-161">If `user.Identity.IsAuthenticated` is `true` and because the user is a <xref:System.Security.Claims.ClaimsPrincipal>, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="c773b-162">Pour plus d’informations sur l’injection de dépendances et les services, consultez <xref:blazor/dependency-injection> et <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="c773b-162">For more information on dependency injection (DI) and services, see <xref:blazor/dependency-injection> and <xref:fundamentals/dependency-injection>.</span></span>

## <a name="implement-a-custom-authenticationstateprovider"></a><span data-ttu-id="c773b-163">Implémenter un AuthenticationStateProvider personnalisé</span><span class="sxs-lookup"><span data-stu-id="c773b-163">Implement a custom AuthenticationStateProvider</span></span>

<span data-ttu-id="c773b-164">Si vous générez une application Blazor webassembly ou si la spécification de votre application requiert absolument un fournisseur personnalisé, implémentez un fournisseur et remplacez `GetAuthenticationStateAsync`:</span><span class="sxs-lookup"><span data-stu-id="c773b-164">If you're building a Blazor WebAssembly app or if your app's specification absolutely requires a custom provider, implement a provider and override `GetAuthenticationStateAsync`:</span></span>

```csharp
using System.Security.Claims;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Components.Authorization;

namespace BlazorSample.Services
{
    public class CustomAuthStateProvider : AuthenticationStateProvider
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
}
```

<span data-ttu-id="c773b-165">Le service `CustomAuthStateProvider` est inscrit dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="c773b-165">The `CustomAuthStateProvider` service is registered in `Startup.ConfigureServices`:</span></span>

```csharp
// using Microsoft.AspNetCore.Components.Authorization;
// using BlazorSample.Services;

services.AddScoped<AuthenticationStateProvider, CustomAuthStateProvider>();
```

<span data-ttu-id="c773b-166">Avec `CustomAuthStateProvider`, tous les utilisateurs sont authentifiés avec le nom d’utilisateur `mrfibuli`.</span><span class="sxs-lookup"><span data-stu-id="c773b-166">Using the `CustomAuthStateProvider`, all users are authenticated with the username `mrfibuli`.</span></span>

## <a name="expose-the-authentication-state-as-a-cascading-parameter"></a><span data-ttu-id="c773b-167">Exposer l’état d’authentification comme un paramètre en cascade</span><span class="sxs-lookup"><span data-stu-id="c773b-167">Expose the authentication state as a cascading parameter</span></span>

<span data-ttu-id="c773b-168">Si les données d’état d’authentification sont requises pour la logique procédurale, par exemple lors d’une action déclenchée par l’utilisateur, obtenez les données d’état d’authentification en définissant un paramètre en cascade de type `Task<AuthenticationState>` :</span><span class="sxs-lookup"><span data-stu-id="c773b-168">If authentication state data is required for procedural logic, such as when performing an action triggered by the user, obtain the authentication state data by defining a cascading parameter of type `Task<AuthenticationState>`:</span></span>

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

> [!NOTE]
> <span data-ttu-id="c773b-169">Dans un composant d’application Blazor webassembly, ajoutez l’espace de noms `Microsoft.AspNetCore.Components.Authorization` (`@using Microsoft.AspNetCore.Components.Authorization`).</span><span class="sxs-lookup"><span data-stu-id="c773b-169">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Components.Authorization` namespace (`@using Microsoft.AspNetCore.Components.Authorization`).</span></span>

<span data-ttu-id="c773b-170">Si `user.Identity.IsAuthenticated` est `true`, les revendications peuvent être énumérées et l’appartenance aux rôles évaluée.</span><span class="sxs-lookup"><span data-stu-id="c773b-170">If `user.Identity.IsAuthenticated` is `true`, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="c773b-171">Configurez le `Task<AuthenticationState>` paramètre en cascade à l’aide des composants `AuthorizeRouteView` et `CascadingAuthenticationState` :</span><span class="sxs-lookup"><span data-stu-id="c773b-171">Set up the `Task<AuthenticationState>` cascading parameter using the `AuthorizeRouteView` and `CascadingAuthenticationState` components:</span></span>

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

## <a name="authorization"></a><span data-ttu-id="c773b-172">Autorisation</span><span class="sxs-lookup"><span data-stu-id="c773b-172">Authorization</span></span>

<span data-ttu-id="c773b-173">Une fois qu’un utilisateur est authentifié, les règles *d’autorisation* sont appliquées pour contrôler ce que l’utilisateur peut faire.</span><span class="sxs-lookup"><span data-stu-id="c773b-173">After a user is authenticated, *authorization* rules are applied to control what the user can do.</span></span>

<span data-ttu-id="c773b-174">L’accès est généralement accordé ou refusé selon les conditions suivantes :</span><span class="sxs-lookup"><span data-stu-id="c773b-174">Access is typically granted or denied based on whether:</span></span>

* <span data-ttu-id="c773b-175">Un utilisateur est authentifié (connecté).</span><span class="sxs-lookup"><span data-stu-id="c773b-175">A user is authenticated (signed in).</span></span>
* <span data-ttu-id="c773b-176">Un utilisateur appartient à un *rôle*.</span><span class="sxs-lookup"><span data-stu-id="c773b-176">A user is in a *role*.</span></span>
* <span data-ttu-id="c773b-177">Un utilisateur a une *revendication*.</span><span class="sxs-lookup"><span data-stu-id="c773b-177">A user has a *claim*.</span></span>
* <span data-ttu-id="c773b-178">Une *stratégie* est satisfaite.</span><span class="sxs-lookup"><span data-stu-id="c773b-178">A *policy* is satisfied.</span></span>

<span data-ttu-id="c773b-179">Tous ces concepts sont les mêmes que dans une application ASP.NET Core MVC ou Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="c773b-179">Each of these concepts is the same as in an ASP.NET Core MVC or Razor Pages app.</span></span> <span data-ttu-id="c773b-180">Pour plus d’informations sur la sécurité d’ASP.NET Core, consultez les articles sous [Sécurité et identité dans ASP.NET Core](xref:security/index).</span><span class="sxs-lookup"><span data-stu-id="c773b-180">For more information on ASP.NET Core security, see the articles under [ASP.NET Core Security and Identity](xref:security/index).</span></span>

## <a name="authorizeview-component"></a><span data-ttu-id="c773b-181">Composant AuthorizeView</span><span class="sxs-lookup"><span data-stu-id="c773b-181">AuthorizeView component</span></span>

<span data-ttu-id="c773b-182">Le composant `AuthorizeView` affiche sélectivement l’interface utilisateur en fonction de l’autorisation que l’utilisateur a pour l’afficher.</span><span class="sxs-lookup"><span data-stu-id="c773b-182">The `AuthorizeView` component selectively displays UI depending on whether the user is authorized to see it.</span></span> <span data-ttu-id="c773b-183">Cette approche est utile lorsque vous devez uniquement *afficher* les données de l’utilisateur et que vous n’avez pas besoin d’utiliser l’identité de l’utilisateur dans la logique procédurale.</span><span class="sxs-lookup"><span data-stu-id="c773b-183">This approach is useful when you only need to *display* data for the user and don't need to use the user's identity in procedural logic.</span></span>

<span data-ttu-id="c773b-184">Le composant expose une variable `context` de type `AuthenticationState`, que vous pouvez utiliser pour accéder aux informations relatives à l’utilisateur connecté :</span><span class="sxs-lookup"><span data-stu-id="c773b-184">The component exposes a `context` variable of type `AuthenticationState`, which you can use to access information about the signed-in user:</span></span>

```cshtml
<AuthorizeView>
    <h1>Hello, @context.User.Identity.Name!</h1>
    <p>You can only see this content if you're authenticated.</p>
</AuthorizeView>
```

<span data-ttu-id="c773b-185">Vous pouvez également fournir un contenu différent à afficher si l’utilisateur n’est pas authentifié :</span><span class="sxs-lookup"><span data-stu-id="c773b-185">You can also supply different content for display if the user isn't authenticated:</span></span>

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

<span data-ttu-id="c773b-186">Le contenu des balises `<Authorized>` et `<NotAuthorized>` peut inclure des éléments arbitraires, tels que d’autres composants interactifs.</span><span class="sxs-lookup"><span data-stu-id="c773b-186">The content of `<Authorized>` and `<NotAuthorized>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="c773b-187">Les conditions d’autorisation, comme les rôles ou les stratégies qui contrôlent les options d’interface utilisateur ou d’accès, sont traitées dans la section [Autorisation](#authorization).</span><span class="sxs-lookup"><span data-stu-id="c773b-187">Authorization conditions, such as roles or policies that control UI options or access, are covered in the [Authorization](#authorization) section.</span></span>

<span data-ttu-id="c773b-188">Si les conditions d’autorisation ne sont pas spécifiées, `AuthorizeView` utilise une stratégie par défaut et traite :</span><span class="sxs-lookup"><span data-stu-id="c773b-188">If authorization conditions aren't specified, `AuthorizeView` uses a default policy and treats:</span></span>

* <span data-ttu-id="c773b-189">Les utilisateurs authentifiés (connectés) comme étant autorisés.</span><span class="sxs-lookup"><span data-stu-id="c773b-189">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="c773b-190">Les utilisateurs non authentifiés (déconnectés) comme étant non autorisés.</span><span class="sxs-lookup"><span data-stu-id="c773b-190">Unauthenticated (signed-out) users as unauthorized.</span></span>

### <a name="role-based-and-policy-based-authorization"></a><span data-ttu-id="c773b-191">Autorisation en fonction du rôle et de la stratégie</span><span class="sxs-lookup"><span data-stu-id="c773b-191">Role-based and policy-based authorization</span></span>

<span data-ttu-id="c773b-192">Le composant `AuthorizeView` prend en charge l’autorisation *basée sur le rôle* et *basée sur les stratégies*.</span><span class="sxs-lookup"><span data-stu-id="c773b-192">The `AuthorizeView` component supports *role-based* or *policy-based* authorization.</span></span>

<span data-ttu-id="c773b-193">Pour l’autorisation en fonction du rôle, utilisez le paramètre `Roles` :</span><span class="sxs-lookup"><span data-stu-id="c773b-193">For role-based authorization, use the `Roles` parameter:</span></span>

```cshtml
<AuthorizeView Roles="admin, superuser">
    <p>You can only see this if you're an admin or superuser.</p>
</AuthorizeView>
```

<span data-ttu-id="c773b-194">Pour plus d'informations, consultez <xref:security/authorization/roles>.</span><span class="sxs-lookup"><span data-stu-id="c773b-194">For more information, see <xref:security/authorization/roles>.</span></span>

<span data-ttu-id="c773b-195">Pour l’autorisation en fonction des stratégies, utilisez le paramètre `Policy` :</span><span class="sxs-lookup"><span data-stu-id="c773b-195">For policy-based authorization, use the `Policy` parameter:</span></span>

```cshtml
<AuthorizeView Policy="content-editor">
    <p>You can only see this if you satisfy the "content-editor" policy.</p>
</AuthorizeView>
```

<span data-ttu-id="c773b-196">L’autorisation basée sur les revendications est un cas spécial d’autorisation basée sur les stratégies.</span><span class="sxs-lookup"><span data-stu-id="c773b-196">Claims-based authorization is a special case of policy-based authorization.</span></span> <span data-ttu-id="c773b-197">Par exemple, vous pouvez définir une stratégie qui impose aux utilisateurs d’avoir une certaine revendication.</span><span class="sxs-lookup"><span data-stu-id="c773b-197">For example, you can define a policy that requires users to have a certain claim.</span></span> <span data-ttu-id="c773b-198">Pour plus d'informations, consultez <xref:security/authorization/policies>.</span><span class="sxs-lookup"><span data-stu-id="c773b-198">For more information, see <xref:security/authorization/policies>.</span></span>

<span data-ttu-id="c773b-199">Ces API peuvent être utilisées dans les applications Blazor Server ou Blazor webassembly.</span><span class="sxs-lookup"><span data-stu-id="c773b-199">These APIs can be used in either Blazor Server or Blazor WebAssembly apps.</span></span>

<span data-ttu-id="c773b-200">Si ni `Roles` ni `Policy` n’est spécifié, `AuthorizeView` utilise la stratégie par défaut.</span><span class="sxs-lookup"><span data-stu-id="c773b-200">If neither `Roles` nor `Policy` is specified, `AuthorizeView` uses the default policy.</span></span>

### <a name="content-displayed-during-asynchronous-authentication"></a><span data-ttu-id="c773b-201">Contenu affiché lors de l’authentification asynchrone</span><span class="sxs-lookup"><span data-stu-id="c773b-201">Content displayed during asynchronous authentication</span></span>

Blazor<span data-ttu-id="c773b-202"> permet de déterminer l’état d’authentification de *manière asynchrone*.</span><span class="sxs-lookup"><span data-stu-id="c773b-202"> allows for authentication state to be determined *asynchronously*.</span></span> <span data-ttu-id="c773b-203">Le scénario principal de cette approche se trouve dans Blazor applications webassembly qui effectuent une demande à un point de terminaison externe pour l’authentification.</span><span class="sxs-lookup"><span data-stu-id="c773b-203">The primary scenario for this approach is in Blazor WebAssembly apps that make a request to an external endpoint for authentication.</span></span>

<span data-ttu-id="c773b-204">Lorsque l’authentification est en cours, `AuthorizeView` n’affiche aucun contenu par défaut.</span><span class="sxs-lookup"><span data-stu-id="c773b-204">While authentication is in progress, `AuthorizeView` displays no content by default.</span></span> <span data-ttu-id="c773b-205">Pour afficher le contenu pendant que l’authentification se produit, utilisez l’élément `<Authorizing>` :</span><span class="sxs-lookup"><span data-stu-id="c773b-205">To display content while authentication occurs, use the `<Authorizing>` element:</span></span>

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

<span data-ttu-id="c773b-206">Cette approche n’est normalement pas applicable aux applications Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="c773b-206">This approach isn't normally applicable to Blazor Server apps.</span></span> Blazor<span data-ttu-id="c773b-207"> applications serveur connaissent l’état d’authentification dès que l’État est établi.</span><span class="sxs-lookup"><span data-stu-id="c773b-207"> Server apps know the authentication state as soon as the state is established.</span></span> <span data-ttu-id="c773b-208">`Authorizing` contenu peut être fourni dans le composant `AuthorizeView` d’une application Blazor Server, mais le contenu n’est jamais affiché.</span><span class="sxs-lookup"><span data-stu-id="c773b-208">`Authorizing` content can be provided in a Blazor Server app's `AuthorizeView` component, but the content is never displayed.</span></span>

## <a name="authorize-attribute"></a><span data-ttu-id="c773b-209">Attribut [Authorize]</span><span class="sxs-lookup"><span data-stu-id="c773b-209">[Authorize] attribute</span></span>

<span data-ttu-id="c773b-210">L’attribut `[Authorize]` peut être utilisé dans les composants Razor :</span><span class="sxs-lookup"><span data-stu-id="c773b-210">The `[Authorize]` attribute can be used in Razor components:</span></span>

```cshtml
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!NOTE]
> <span data-ttu-id="c773b-211">Dans un composant d’application Blazor webassembly, ajoutez l’espace de noms `Microsoft.AspNetCore.Authorization` (`@using Microsoft.AspNetCore.Authorization`) aux exemples de cette section.</span><span class="sxs-lookup"><span data-stu-id="c773b-211">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Authorization` namespace (`@using Microsoft.AspNetCore.Authorization`) to the examples in this section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c773b-212">N’utilisez `[Authorize]` sur `@page` composants atteints via le routeur Blazor.</span><span class="sxs-lookup"><span data-stu-id="c773b-212">Only use `[Authorize]` on `@page` components reached via the Blazor Router.</span></span> <span data-ttu-id="c773b-213">L’autorisation est effectuée uniquement en tant qu’aspect du routage et *pas* pour les composants enfants rendus dans une page.</span><span class="sxs-lookup"><span data-stu-id="c773b-213">Authorization is only performed as an aspect of routing and *not* for child components rendered within a page.</span></span> <span data-ttu-id="c773b-214">Pour autoriser l’affichage d’éléments spécifiques dans une page, utilisez `AuthorizeView` à la place.</span><span class="sxs-lookup"><span data-stu-id="c773b-214">To authorize the display of specific parts within a page, use `AuthorizeView` instead.</span></span>

<span data-ttu-id="c773b-215">L’attribut `[Authorize]` prend également en charge l’autorisation en fonction du rôle ou des stratégies.</span><span class="sxs-lookup"><span data-stu-id="c773b-215">The `[Authorize]` attribute also supports role-based or policy-based authorization.</span></span> <span data-ttu-id="c773b-216">Pour l’autorisation en fonction du rôle, utilisez le paramètre `Roles` :</span><span class="sxs-lookup"><span data-stu-id="c773b-216">For role-based authorization, use the `Roles` parameter:</span></span>

```cshtml
@page "/"
@attribute [Authorize(Roles = "admin, superuser")]

<p>You can only see this if you're in the 'admin' or 'superuser' role.</p>
```

<span data-ttu-id="c773b-217">Pour l’autorisation en fonction des stratégies, utilisez le paramètre `Policy` :</span><span class="sxs-lookup"><span data-stu-id="c773b-217">For policy-based authorization, use the `Policy` parameter:</span></span>

```cshtml
@page "/"
@attribute [Authorize(Policy = "content-editor")]

<p>You can only see this if you satisfy the 'content-editor' policy.</p>
```

<span data-ttu-id="c773b-218">Si ni `Roles` ni `Policy` n’est spécifié, `[Authorize]` utilise la stratégie par défaut, qui consiste par défaut à traiter :</span><span class="sxs-lookup"><span data-stu-id="c773b-218">If neither `Roles` nor `Policy` is specified, `[Authorize]` uses the default policy, which by default is to treat:</span></span>

* <span data-ttu-id="c773b-219">Les utilisateurs authentifiés (connectés) comme étant autorisés.</span><span class="sxs-lookup"><span data-stu-id="c773b-219">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="c773b-220">Les utilisateurs non authentifiés (déconnectés) comme étant non autorisés.</span><span class="sxs-lookup"><span data-stu-id="c773b-220">Unauthenticated (signed-out) users as unauthorized.</span></span>

## <a name="customize-unauthorized-content-with-the-router-component"></a><span data-ttu-id="c773b-221">Personnaliser le contenu non autorisé avec le composant Router</span><span class="sxs-lookup"><span data-stu-id="c773b-221">Customize unauthorized content with the Router component</span></span>

<span data-ttu-id="c773b-222">Le composant `Router`, conjointement au composant `AuthorizeRouteView`, permet à l’application de spécifier du contenu personnalisé dans les cas suivants :</span><span class="sxs-lookup"><span data-stu-id="c773b-222">The `Router` component, in conjunction with the `AuthorizeRouteView` component, allows the app to specify custom content if:</span></span>

* <span data-ttu-id="c773b-223">Le contenu est introuvable.</span><span class="sxs-lookup"><span data-stu-id="c773b-223">Content isn't found.</span></span>
* <span data-ttu-id="c773b-224">L’utilisateur ne répond pas à une condition `[Authorize]` appliquée au composant.</span><span class="sxs-lookup"><span data-stu-id="c773b-224">The user fails an `[Authorize]` condition applied to the component.</span></span> <span data-ttu-id="c773b-225">L’attribut `[Authorize]` est abordé dans la section [`[Authorize]` attribut](#authorize-attribute) .</span><span class="sxs-lookup"><span data-stu-id="c773b-225">The `[Authorize]` attribute is covered in the [`[Authorize]` attribute](#authorize-attribute) section.</span></span>
* <span data-ttu-id="c773b-226">Une authentification asynchrone est en cours.</span><span class="sxs-lookup"><span data-stu-id="c773b-226">Asynchronous authentication is in progress.</span></span>

<span data-ttu-id="c773b-227">Dans le modèle de projet Blazor Server par défaut, le fichier *app. Razor* montre comment définir un contenu personnalisé :</span><span class="sxs-lookup"><span data-stu-id="c773b-227">In the default Blazor Server project template, the *App.razor* file demonstrates how to set custom content:</span></span>

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

<span data-ttu-id="c773b-228">Le contenu des balises `<NotFound>`, `<NotAuthorized>`et `<Authorizing>` peut inclure des éléments arbitraires, tels que d’autres composants interactifs.</span><span class="sxs-lookup"><span data-stu-id="c773b-228">The content of `<NotFound>`, `<NotAuthorized>`, and `<Authorizing>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="c773b-229">Si l’élément `<NotAuthorized>` n’est pas spécifié, le `AuthorizeRouteView` utilise le message de secours suivant :</span><span class="sxs-lookup"><span data-stu-id="c773b-229">If the `<NotAuthorized>` element isn't specified, the `AuthorizeRouteView` uses the following fallback message:</span></span>

```html
Not authorized.
```

## <a name="notification-about-authentication-state-changes"></a><span data-ttu-id="c773b-230">Notification sur les changements d’état de l’authentification</span><span class="sxs-lookup"><span data-stu-id="c773b-230">Notification about authentication state changes</span></span>

<span data-ttu-id="c773b-231">Si l’application détermine que les données d’état d’authentification sous-jacentes ont changé (par exemple, parce que l’utilisateur s’est déconnecté ou qu’un autre utilisateur a modifié ses rôles), un `AuthenticationStateProvider` personnalisé peut éventuellement appeler la méthode `NotifyAuthenticationStateChanged` sur la classe de base `AuthenticationStateProvider`.</span><span class="sxs-lookup"><span data-stu-id="c773b-231">If the app determines that the underlying authentication state data has changed (for example, because the user signed out or another user has changed their roles), a custom `AuthenticationStateProvider` can optionally invoke the method `NotifyAuthenticationStateChanged` on the `AuthenticationStateProvider` base class.</span></span> <span data-ttu-id="c773b-232">Cela prévient les consommateurs des données d’état d’authentification (par exemple, `AuthorizeView`) qu’elles doivent appliquer un nouveau rendu à l’aide des nouvelles données.</span><span class="sxs-lookup"><span data-stu-id="c773b-232">This notifies consumers of the authentication state data (for example, `AuthorizeView`) to rerender using the new data.</span></span>

## <a name="procedural-logic"></a><span data-ttu-id="c773b-233">Logique procédurale</span><span class="sxs-lookup"><span data-stu-id="c773b-233">Procedural logic</span></span>

<span data-ttu-id="c773b-234">Si l’application est nécessaire pour vérifier les règles d’autorisation dans le cadre de la logique procédurale, utilisez un paramètre en cascade de type `Task<AuthenticationState>` pour obtenir le <xref:System.Security.Claims.ClaimsPrincipal> de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="c773b-234">If the app is required to check authorization rules as part of procedural logic, use a cascaded parameter of type `Task<AuthenticationState>` to obtain the user's <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="c773b-235">`Task<AuthenticationState>` peut être combiné avec d’autres services, comme `IAuthorizationService`, pour évaluer les stratégies.</span><span class="sxs-lookup"><span data-stu-id="c773b-235">`Task<AuthenticationState>` can be combined with other services, such as `IAuthorizationService`, to evaluate policies.</span></span>

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

> [!NOTE]
> <span data-ttu-id="c773b-236">Dans un composant d’application Blazor webassembly, ajoutez les espaces de noms `Microsoft.AspNetCore.Authorization` et `Microsoft.AspNetCore.Components.Authorization` :</span><span class="sxs-lookup"><span data-stu-id="c773b-236">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Authorization` and `Microsoft.AspNetCore.Components.Authorization` namespaces:</span></span>
>
> ```cshtml
> @using Microsoft.AspNetCore.Authorization
> @using Microsoft.AspNetCore.Components.Authorization
> ```

## <a name="authorization-in-opno-locblazor-webassembly-apps"></a><span data-ttu-id="c773b-237">Autorisation dans Blazor applications webassembly</span><span class="sxs-lookup"><span data-stu-id="c773b-237">Authorization in Blazor WebAssembly apps</span></span>

<span data-ttu-id="c773b-238">Dans Blazor applications webassembly, les vérifications d’autorisation peuvent être ignorées, car le code côté client peut être modifié par les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="c773b-238">In Blazor WebAssembly apps, authorization checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="c773b-239">Cela vaut également pour toutes les technologies d’application côté client, y compris les infrastructures d’application JavaScript SPA ou les applications natives pour n’importe quel système d’exploitation.</span><span class="sxs-lookup"><span data-stu-id="c773b-239">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="c773b-240">**Effectuez toujours les vérifications d’autorisation sur le serveur au sein des points de terminaison de l’API auxquels votre application côté client accède.**</span><span class="sxs-lookup"><span data-stu-id="c773b-240">**Always perform authorization checks on the server within any API endpoints accessed by your client-side app.**</span></span>

## <a name="troubleshoot-errors"></a><span data-ttu-id="c773b-241">Résoudre les erreurs</span><span class="sxs-lookup"><span data-stu-id="c773b-241">Troubleshoot errors</span></span>

<span data-ttu-id="c773b-242">Erreurs courantes :</span><span class="sxs-lookup"><span data-stu-id="c773b-242">Common errors:</span></span>

* <span data-ttu-id="c773b-243">**L’autorisation nécessite un paramètre en cascade de type Task\<AuthenticationState >. Envisagez d’utiliser CascadingAuthenticationState pour fournir ce.**</span><span class="sxs-lookup"><span data-stu-id="c773b-243">**Authorization requires a cascading parameter of type Task\<AuthenticationState>. Consider using CascadingAuthenticationState to supply this.**</span></span>

* <span data-ttu-id="c773b-244">**La valeur `null` est reçue pour `authenticationStateTask`**</span><span class="sxs-lookup"><span data-stu-id="c773b-244">**`null` value is received for `authenticationStateTask`**</span></span>

<span data-ttu-id="c773b-245">Il est probable que le projet n’a pas été créé à l’aide d’un modèle de serveur Blazor avec l’authentification activée.</span><span class="sxs-lookup"><span data-stu-id="c773b-245">It's likely that the project wasn't created using a Blazor Server template with authentication enabled.</span></span> <span data-ttu-id="c773b-246">Encapsulez un `<CascadingAuthenticationState>` dans certaines parties de l’arborescence de l’interface utilisateur, par exemple dans *App.razor*, comme suit :</span><span class="sxs-lookup"><span data-stu-id="c773b-246">Wrap a `<CascadingAuthenticationState>` around some part of the UI tree, for example in *App.razor* as follows:</span></span>

```cshtml
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CascadingAuthenticationState>
```

<span data-ttu-id="c773b-247">`CascadingAuthenticationState` fournit le paramètre en cascade `Task<AuthenticationState>`, qu’il reçoit à son tour du service d’injection de dépendances `AuthenticationStateProvider` sous-jacent.</span><span class="sxs-lookup"><span data-stu-id="c773b-247">The `CascadingAuthenticationState` supplies the `Task<AuthenticationState>` cascading parameter, which in turn it receives from the underlying `AuthenticationStateProvider` DI service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c773b-248">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="c773b-248">Additional resources</span></span>

* <xref:security/index>
* <xref:security/blazor/server>
* <xref:security/authentication/windowsauth>
