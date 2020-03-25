---
title: ASP.NET Core Blazor l’authentification et l’autorisation
author: guardrex
description: En savoir plus sur les scénarios d’authentification et d’autorisation Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/21/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/index
ms.openlocfilehash: f7ffb4c3d5a05cb916b4f00cdfaf5898634a1a6d
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219023"
---
# <a name="aspnet-core-blazor-authentication-and-authorization"></a><span data-ttu-id="f3960-103">Authentification et autorisation avec ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="f3960-103">ASP.NET Core Blazor authentication and authorization</span></span>

<span data-ttu-id="f3960-104">Par [Steve Sanderson](https://github.com/SteveSandersonMS)</span><span class="sxs-lookup"><span data-stu-id="f3960-104">By [Steve Sanderson](https://github.com/SteveSandersonMS)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="f3960-105">ASP.NET Core prend en charge la configuration et la gestion de la sécurité dans les applications Blazor.</span><span class="sxs-lookup"><span data-stu-id="f3960-105">ASP.NET Core supports the configuration and management of security in Blazor apps.</span></span>

<span data-ttu-id="f3960-106">Les scénarios de sécurité diffèrent entre le serveur éblouissant et les applications webassembly éblouissantes.</span><span class="sxs-lookup"><span data-stu-id="f3960-106">Security scenarios differ between Blazor Server and Blazor WebAssembly apps.</span></span> <span data-ttu-id="f3960-107">Étant donné que les applications serveur éblouissantes s’exécutent sur le serveur, les contrôles d’autorisation peuvent déterminer les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="f3960-107">Because Blazor Server apps run on the server, authorization checks are able to determine:</span></span>

* <span data-ttu-id="f3960-108">Les options de l’interface utilisateur présentées à un utilisateur (par exemple, les entrées de menu disponibles pour un utilisateur).</span><span class="sxs-lookup"><span data-stu-id="f3960-108">The UI options presented to a user (for example, which menu entries are available to a user).</span></span>
* <span data-ttu-id="f3960-109">Les règles d’accès pour les zones de l’application et les composants.</span><span class="sxs-lookup"><span data-stu-id="f3960-109">Access rules for areas of the app and components.</span></span>

<span data-ttu-id="f3960-110">Les applications webassembly éblouissant s’exécutent sur le client.</span><span class="sxs-lookup"><span data-stu-id="f3960-110">Blazor WebAssembly apps run on the client.</span></span> <span data-ttu-id="f3960-111">L’autorisation est *uniquement* utilisée pour déterminer les options de l’interface utilisateur à afficher.</span><span class="sxs-lookup"><span data-stu-id="f3960-111">Authorization is *only* used to determine which UI options to show.</span></span> <span data-ttu-id="f3960-112">Étant donné que les contrôles côté client peuvent être modifiés ou ignorés par un utilisateur, une application de webassembly éblouissante ne peut pas appliquer les règles d’accès aux autorisations.</span><span class="sxs-lookup"><span data-stu-id="f3960-112">Since client-side checks can be modified or bypassed by a user, a Blazor WebAssembly app can't enforce authorization access rules.</span></span>

<span data-ttu-id="f3960-113">[Razor pages conventions d’autorisation](xref:security/authorization/razor-pages-authorization) ne s’appliquent pas aux composants Razor routables.</span><span class="sxs-lookup"><span data-stu-id="f3960-113">[Razor Pages authorization conventions](xref:security/authorization/razor-pages-authorization) don't apply to routable Razor components.</span></span> <span data-ttu-id="f3960-114">Si un composant Razor non routable est [incorporé dans une page](xref:blazor/integrate-components#render-components-from-a-page-or-view), les conventions d’autorisation de la page affectent indirectement le composant Razor avec le reste du contenu de la page.</span><span class="sxs-lookup"><span data-stu-id="f3960-114">If a non-routable Razor component is [embedded in a page](xref:blazor/integrate-components#render-components-from-a-page-or-view), the page's authorization conventions indirectly affect the Razor component along with the rest of the page's content.</span></span>

## <a name="authentication"></a><span data-ttu-id="f3960-115">Authentication</span><span class="sxs-lookup"><span data-stu-id="f3960-115">Authentication</span></span>

<span data-ttu-id="f3960-116">Blazor utilise les mécanismes d’authentification ASP.NET Core existants pour établir l’identité de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f3960-116">Blazor uses the existing ASP.NET Core authentication mechanisms to establish the user's identity.</span></span> <span data-ttu-id="f3960-117">Le mécanisme exact dépend de la façon dont l’application éblouissant est hébergée, éblouissante Server ou éblouissant webassembly.</span><span class="sxs-lookup"><span data-stu-id="f3960-117">The exact mechanism depends on how the Blazor app is hosted, Blazor Server or Blazor WebAssembly.</span></span>

### <a name="blazor-server-authentication"></a><span data-ttu-id="f3960-118">Authentification du serveur éblouissant</span><span class="sxs-lookup"><span data-stu-id="f3960-118">Blazor Server authentication</span></span>

<span data-ttu-id="f3960-119">Les applications serveur éblouissantes fonctionnent sur une connexion en temps réel créée à l’aide de Signalr.</span><span class="sxs-lookup"><span data-stu-id="f3960-119">Blazor Server apps operate over a real-time connection that's created using SignalR.</span></span> <span data-ttu-id="f3960-120">[L’authentification dans les applications basées sur SignalR](xref:signalr/authn-and-authz) est gérée lorsque la connexion est établie.</span><span class="sxs-lookup"><span data-stu-id="f3960-120">[Authentication in SignalR-based apps](xref:signalr/authn-and-authz) is handled when the connection is established.</span></span> <span data-ttu-id="f3960-121">L’authentification peut reposer sur un cookie ou un autre jeton du porteur.</span><span class="sxs-lookup"><span data-stu-id="f3960-121">Authentication can be based on a cookie or some other bearer token.</span></span>

<span data-ttu-id="f3960-122">Le modèle de projet de serveur éblouissant peut configurer l’authentification à votre place lors de la création du projet.</span><span class="sxs-lookup"><span data-stu-id="f3960-122">The Blazor Server project template can set up authentication for you when the project is created.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="f3960-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f3960-123">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f3960-124">Suivez les instructions de Visual Studio dans l’article <xref:blazor/get-started> pour créer un projet de serveur éblouissant avec un mécanisme d’authentification.</span><span class="sxs-lookup"><span data-stu-id="f3960-124">Follow the Visual Studio guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism.</span></span>

<span data-ttu-id="f3960-125">Après avoir choisi le modèle **Application serveur Blazor** dans la boîte de dialogue **Créer une application web ASP.NET Core.** , sélectionnez **Modifier** sous **Authentification**.</span><span class="sxs-lookup"><span data-stu-id="f3960-125">After choosing the **Blazor Server App** template in the **Create a new ASP.NET Core Web Application** dialog, select **Change** under **Authentication**.</span></span>

<span data-ttu-id="f3960-126">Une boîte de dialogue s’ouvre pour offrir le même ensemble de mécanismes d’authentification que ceux disponibles pour les autres projets ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="f3960-126">A dialog opens to offer the same set of authentication mechanisms available for other ASP.NET Core projects:</span></span>

* <span data-ttu-id="f3960-127">**Aucune authentification**</span><span class="sxs-lookup"><span data-stu-id="f3960-127">**No Authentication**</span></span>
* <span data-ttu-id="f3960-128">**Les comptes d’utilisateurs individuels &ndash; les** comptes d’utilisateur peuvent être stockés :</span><span class="sxs-lookup"><span data-stu-id="f3960-128">**Individual User Accounts** &ndash; User accounts can be stored:</span></span>
  * <span data-ttu-id="f3960-129">Dans l’application à l’aide du système [d’identité](xref:security/authentication/identity) d’ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f3960-129">Within the app using ASP.NET Core's [Identity](xref:security/authentication/identity) system.</span></span>
  * <span data-ttu-id="f3960-130">Avec [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span><span class="sxs-lookup"><span data-stu-id="f3960-130">With [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span>
* <span data-ttu-id="f3960-131">**Comptes professionnels ou scolaires**</span><span class="sxs-lookup"><span data-stu-id="f3960-131">**Work or School Accounts**</span></span>
* <span data-ttu-id="f3960-132">**Authentification Windows**</span><span class="sxs-lookup"><span data-stu-id="f3960-132">**Windows Authentication**</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="f3960-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f3960-133">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="f3960-134">Suivez les instructions de Visual Studio Code dans l’article <xref:blazor/get-started> pour créer un projet de serveur éblouissant avec un mécanisme d’authentification :</span><span class="sxs-lookup"><span data-stu-id="f3960-134">Follow the Visual Studio Code guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism:</span></span>

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

<span data-ttu-id="f3960-135">Les valeurs autorisées d’authentification (`{AUTHENTICATION}`) sont présentées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="f3960-135">Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.</span></span>

| <span data-ttu-id="f3960-136">Mécanisme d’authentification</span><span class="sxs-lookup"><span data-stu-id="f3960-136">Authentication mechanism</span></span>                                                                 | <span data-ttu-id="f3960-137">Valeur `{AUTHENTICATION}`</span><span class="sxs-lookup"><span data-stu-id="f3960-137">`{AUTHENTICATION}` value</span></span> |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| <span data-ttu-id="f3960-138">Aucune authentification</span><span class="sxs-lookup"><span data-stu-id="f3960-138">No Authentication</span></span>                                                                        | `None`                   |
| <span data-ttu-id="f3960-139">Individuel</span><span class="sxs-lookup"><span data-stu-id="f3960-139">Individual</span></span><br><span data-ttu-id="f3960-140">Les utilisateurs stockés dans l’application avec l’identité ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f3960-140">Users stored in the app with ASP.NET Core Identity.</span></span>                        | `Individual`             |
| <span data-ttu-id="f3960-141">Individuel</span><span class="sxs-lookup"><span data-stu-id="f3960-141">Individual</span></span><br><span data-ttu-id="f3960-142">Les utilisateurs stockés dans [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span><span class="sxs-lookup"><span data-stu-id="f3960-142">Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span> | `IndividualB2C`          |
| <span data-ttu-id="f3960-143">Comptes professionnels ou scolaires</span><span class="sxs-lookup"><span data-stu-id="f3960-143">Work or School Accounts</span></span><br><span data-ttu-id="f3960-144">Authentification d’organisation pour un seul abonné.</span><span class="sxs-lookup"><span data-stu-id="f3960-144">Organizational authentication for a single tenant.</span></span>            | `SingleOrg`              |
| <span data-ttu-id="f3960-145">Comptes professionnels ou scolaires</span><span class="sxs-lookup"><span data-stu-id="f3960-145">Work or School Accounts</span></span><br><span data-ttu-id="f3960-146">Authentification d’organisation pour plusieurs abonnés.</span><span class="sxs-lookup"><span data-stu-id="f3960-146">Organizational authentication for multiple tenants.</span></span>           | `MultiOrg`               |
| <span data-ttu-id="f3960-147">Authentification Windows</span><span class="sxs-lookup"><span data-stu-id="f3960-147">Windows Authentication</span></span>                                                                   | `Windows`                |

<span data-ttu-id="f3960-148">La commande crée un dossier nommé avec la valeur fournie pour l’espace réservé `{APP NAME}` et utilise le nom de dossier en tant que nom de l’application.</span><span class="sxs-lookup"><span data-stu-id="f3960-148">The command creates a folder named with the value provided for the `{APP NAME}` placeholder and uses the folder name as the app's name.</span></span> <span data-ttu-id="f3960-149">Pour plus d’informations, consultez la commande [dotnet new](/dotnet/core/tools/dotnet-new) dans le Guide .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f3960-149">For more information, see the [dotnet new](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

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

### <a name="opno-locblazor-webassembly-authentication"></a>Blazor<span data-ttu-id="f3960-150"> l’authentification webassembly</span><span class="sxs-lookup"><span data-stu-id="f3960-150"> WebAssembly authentication</span></span>

<span data-ttu-id="f3960-151">Dans Blazor applications webassembly, les vérifications d’authentification peuvent être contournées, car le code côté client peut être modifié par les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="f3960-151">In Blazor WebAssembly apps, authentication checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="f3960-152">Cela vaut également pour toutes les technologies d’application côté client, y compris les infrastructures d’application JavaScript SPA ou les applications natives pour n’importe quel système d’exploitation.</span><span class="sxs-lookup"><span data-stu-id="f3960-152">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="f3960-153">Ajoutez une référence de package pour [Microsoft. AspNetCore. Components. Authorization](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/) au fichier projet de l’application.</span><span class="sxs-lookup"><span data-stu-id="f3960-153">Add a package reference for [Microsoft.AspNetCore.Components.Authorization](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/) to the app's project file.</span></span>

<span data-ttu-id="f3960-154">L’implémentation d’un service de `AuthenticationStateProvider` personnalisé pour Blazor applications webassembly est traitée dans les sections suivantes.</span><span class="sxs-lookup"><span data-stu-id="f3960-154">Implementation of a custom `AuthenticationStateProvider` service for Blazor WebAssembly apps is covered in the following sections.</span></span>

## <a name="authenticationstateprovider-service"></a><span data-ttu-id="f3960-155">Service AuthenticationStateProvider</span><span class="sxs-lookup"><span data-stu-id="f3960-155">AuthenticationStateProvider service</span></span>

Blazor<span data-ttu-id="f3960-156"> applications serveur incluent un service `AuthenticationStateProvider` intégré qui obtient des données d’état d’authentification du `HttpContext.User`de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f3960-156"> Server apps include a built-in `AuthenticationStateProvider` service that obtains authentication state data from ASP.NET Core's `HttpContext.User`.</span></span> <span data-ttu-id="f3960-157">Il s’agit de la façon dont l’état d’authentification s’intègre avec les mécanismes d’authentification ASP.NET Core côté serveur existants.</span><span class="sxs-lookup"><span data-stu-id="f3960-157">This is how authentication state integrates with existing ASP.NET Core server-side authentication mechanisms.</span></span>

<span data-ttu-id="f3960-158">`AuthenticationStateProvider` est le service sous-jacent utilisé par le composant `AuthorizeView` et le composant `CascadingAuthenticationState` pour obtenir l’état d’authentification.</span><span class="sxs-lookup"><span data-stu-id="f3960-158">`AuthenticationStateProvider` is the underlying service used by the `AuthorizeView` component and `CascadingAuthenticationState` component to get the authentication state.</span></span>

<span data-ttu-id="f3960-159">Vous n’utilisez généralement pas `AuthenticationStateProvider` directement.</span><span class="sxs-lookup"><span data-stu-id="f3960-159">You don't typically use `AuthenticationStateProvider` directly.</span></span> <span data-ttu-id="f3960-160">Utilisez les approches [Composant AuthorizeView](#authorizeview-component) ou [Tâche<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) décrites plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="f3960-160">Use the [AuthorizeView component](#authorizeview-component) or [Task<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) approaches described later in this article.</span></span> <span data-ttu-id="f3960-161">Le principal inconvénient de l’utilisation directe de `AuthenticationStateProvider` est que le composant n’est pas automatiquement averti en cas de modifications de données d’état de l’authentification sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="f3960-161">The main drawback to using `AuthenticationStateProvider` directly is that the component isn't notified automatically if the underlying authentication state data changes.</span></span>

<span data-ttu-id="f3960-162">Le service `AuthenticationStateProvider` peut fournir les données <xref:System.Security.Claims.ClaimsPrincipal> de l’utilisateur actuel, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="f3960-162">The `AuthenticationStateProvider` service can provide the current user's <xref:System.Security.Claims.ClaimsPrincipal> data, as shown in the following example:</span></span>

```razor
@page "/"
@using Microsoft.AspNetCore.Components.Authorization
@inject AuthenticationStateProvider AuthenticationStateProvider

<button @onclick="LogUsername">Write user info to console</button>

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

<span data-ttu-id="f3960-163">Si `user.Identity.IsAuthenticated` est `true` et parce que l’utilisateur est <xref:System.Security.Claims.ClaimsPrincipal>, les revendications peuvent être énumérées et l’appartenance aux rôles évaluée.</span><span class="sxs-lookup"><span data-stu-id="f3960-163">If `user.Identity.IsAuthenticated` is `true` and because the user is a <xref:System.Security.Claims.ClaimsPrincipal>, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="f3960-164">Pour plus d’informations sur l’injection de dépendances et les services, consultez <xref:blazor/dependency-injection> et <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="f3960-164">For more information on dependency injection (DI) and services, see <xref:blazor/dependency-injection> and <xref:fundamentals/dependency-injection>.</span></span>

## <a name="implement-a-custom-authenticationstateprovider"></a><span data-ttu-id="f3960-165">Implémenter un AuthenticationStateProvider personnalisé</span><span class="sxs-lookup"><span data-stu-id="f3960-165">Implement a custom AuthenticationStateProvider</span></span>

<span data-ttu-id="f3960-166">Si vous générez une application Blazor webassembly ou si la spécification de votre application requiert absolument un fournisseur personnalisé, implémentez un fournisseur et remplacez `GetAuthenticationStateAsync`:</span><span class="sxs-lookup"><span data-stu-id="f3960-166">If you're building a Blazor WebAssembly app or if your app's specification absolutely requires a custom provider, implement a provider and override `GetAuthenticationStateAsync`:</span></span>

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

<span data-ttu-id="f3960-167">Dans une application Blazor webassembly, le service `CustomAuthStateProvider` est inscrit dans `Main` de *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="f3960-167">In a Blazor WebAssembly app, the `CustomAuthStateProvider` service is registered in `Main` of *Program.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.Extensions.DependencyInjection;
using BlazorSample.Services;

public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddScoped<AuthenticationStateProvider, 
            CustomAuthStateProvider>();
        builder.RootComponents.Add<App>("app");

        await builder.Build().RunAsync();
    }
}
```

<span data-ttu-id="f3960-168">Avec `CustomAuthStateProvider`, tous les utilisateurs sont authentifiés avec le nom d’utilisateur `mrfibuli`.</span><span class="sxs-lookup"><span data-stu-id="f3960-168">Using the `CustomAuthStateProvider`, all users are authenticated with the username `mrfibuli`.</span></span>

## <a name="expose-the-authentication-state-as-a-cascading-parameter"></a><span data-ttu-id="f3960-169">Exposer l’état d’authentification comme un paramètre en cascade</span><span class="sxs-lookup"><span data-stu-id="f3960-169">Expose the authentication state as a cascading parameter</span></span>

<span data-ttu-id="f3960-170">Si les données d’état d’authentification sont requises pour la logique procédurale, par exemple lors d’une action déclenchée par l’utilisateur, obtenez les données d’état d’authentification en définissant un paramètre en cascade de type `Task<AuthenticationState>` :</span><span class="sxs-lookup"><span data-stu-id="f3960-170">If authentication state data is required for procedural logic, such as when performing an action triggered by the user, obtain the authentication state data by defining a cascading parameter of type `Task<AuthenticationState>`:</span></span>

```razor
@page "/"

<button @onclick="LogUsername">Log username</button>

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
> <span data-ttu-id="f3960-171">Dans un composant d’application Blazor webassembly, ajoutez l’espace de noms `Microsoft.AspNetCore.Components.Authorization` (`@using Microsoft.AspNetCore.Components.Authorization`).</span><span class="sxs-lookup"><span data-stu-id="f3960-171">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Components.Authorization` namespace (`@using Microsoft.AspNetCore.Components.Authorization`).</span></span>

<span data-ttu-id="f3960-172">Si `user.Identity.IsAuthenticated` est `true`, les revendications peuvent être énumérées et l’appartenance aux rôles évaluée.</span><span class="sxs-lookup"><span data-stu-id="f3960-172">If `user.Identity.IsAuthenticated` is `true`, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="f3960-173">Configurez le `Task<AuthenticationState>` paramètre en cascade à l’aide des composants `AuthorizeRouteView` et `CascadingAuthenticationState` dans le fichier *app. Razor* :</span><span class="sxs-lookup"><span data-stu-id="f3960-173">Set up the `Task<AuthenticationState>` cascading parameter using the `AuthorizeRouteView` and `CascadingAuthenticationState` components in the *App.razor* file:</span></span>

```razor
@using Microsoft.AspNetCore.Components.Authorization

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

<span data-ttu-id="f3960-174">Ajouter des services pour les options et l’autorisation d' `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="f3960-174">Add services for options and authorization to `Program.Main`:</span></span>

```csharp
builder.Services.AddOptions();
builder.Services.AddAuthorizationCore();
```

## <a name="authorization"></a><span data-ttu-id="f3960-175">Autorisation</span><span class="sxs-lookup"><span data-stu-id="f3960-175">Authorization</span></span>

<span data-ttu-id="f3960-176">Une fois qu’un utilisateur est authentifié, les règles *d’autorisation* sont appliquées pour contrôler ce que l’utilisateur peut faire.</span><span class="sxs-lookup"><span data-stu-id="f3960-176">After a user is authenticated, *authorization* rules are applied to control what the user can do.</span></span>

<span data-ttu-id="f3960-177">L’accès est généralement accordé ou refusé selon les conditions suivantes :</span><span class="sxs-lookup"><span data-stu-id="f3960-177">Access is typically granted or denied based on whether:</span></span>

* <span data-ttu-id="f3960-178">Un utilisateur est authentifié (connecté).</span><span class="sxs-lookup"><span data-stu-id="f3960-178">A user is authenticated (signed in).</span></span>
* <span data-ttu-id="f3960-179">Un utilisateur appartient à un *rôle*.</span><span class="sxs-lookup"><span data-stu-id="f3960-179">A user is in a *role*.</span></span>
* <span data-ttu-id="f3960-180">Un utilisateur a une *revendication*.</span><span class="sxs-lookup"><span data-stu-id="f3960-180">A user has a *claim*.</span></span>
* <span data-ttu-id="f3960-181">Une *stratégie* est satisfaite.</span><span class="sxs-lookup"><span data-stu-id="f3960-181">A *policy* is satisfied.</span></span>

<span data-ttu-id="f3960-182">Tous ces concepts sont les mêmes que dans une application ASP.NET Core MVC ou Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="f3960-182">Each of these concepts is the same as in an ASP.NET Core MVC or Razor Pages app.</span></span> <span data-ttu-id="f3960-183">Pour plus d’informations sur la sécurité d’ASP.NET Core, consultez les articles sous [Sécurité et identité dans ASP.NET Core](xref:security/index).</span><span class="sxs-lookup"><span data-stu-id="f3960-183">For more information on ASP.NET Core security, see the articles under [ASP.NET Core Security and Identity](xref:security/index).</span></span>

## <a name="authorizeview-component"></a><span data-ttu-id="f3960-184">Composant AuthorizeView</span><span class="sxs-lookup"><span data-stu-id="f3960-184">AuthorizeView component</span></span>

<span data-ttu-id="f3960-185">Le composant `AuthorizeView` affiche sélectivement l’interface utilisateur en fonction de l’autorisation que l’utilisateur a pour l’afficher.</span><span class="sxs-lookup"><span data-stu-id="f3960-185">The `AuthorizeView` component selectively displays UI depending on whether the user is authorized to see it.</span></span> <span data-ttu-id="f3960-186">Cette approche est utile lorsque vous devez uniquement *afficher* les données de l’utilisateur et que vous n’avez pas besoin d’utiliser l’identité de l’utilisateur dans la logique procédurale.</span><span class="sxs-lookup"><span data-stu-id="f3960-186">This approach is useful when you only need to *display* data for the user and don't need to use the user's identity in procedural logic.</span></span>

<span data-ttu-id="f3960-187">Le composant expose une variable `context` de type `AuthenticationState`, que vous pouvez utiliser pour accéder aux informations relatives à l’utilisateur connecté :</span><span class="sxs-lookup"><span data-stu-id="f3960-187">The component exposes a `context` variable of type `AuthenticationState`, which you can use to access information about the signed-in user:</span></span>

```razor
<AuthorizeView>
    <h1>Hello, @context.User.Identity.Name!</h1>
    <p>You can only see this content if you're authenticated.</p>
</AuthorizeView>
```

<span data-ttu-id="f3960-188">Vous pouvez également fournir un contenu différent à afficher si l’utilisateur n’est pas authentifié :</span><span class="sxs-lookup"><span data-stu-id="f3960-188">You can also supply different content for display if the user isn't authenticated:</span></span>

```razor
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

<span data-ttu-id="f3960-189">Le contenu des balises `<Authorized>` et `<NotAuthorized>` peut inclure des éléments arbitraires, tels que d’autres composants interactifs.</span><span class="sxs-lookup"><span data-stu-id="f3960-189">The content of `<Authorized>` and `<NotAuthorized>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="f3960-190">Les conditions d’autorisation, comme les rôles ou les stratégies qui contrôlent les options d’interface utilisateur ou d’accès, sont traitées dans la section [Autorisation](#authorization).</span><span class="sxs-lookup"><span data-stu-id="f3960-190">Authorization conditions, such as roles or policies that control UI options or access, are covered in the [Authorization](#authorization) section.</span></span>

<span data-ttu-id="f3960-191">Si les conditions d’autorisation ne sont pas spécifiées, `AuthorizeView` utilise une stratégie par défaut et traite :</span><span class="sxs-lookup"><span data-stu-id="f3960-191">If authorization conditions aren't specified, `AuthorizeView` uses a default policy and treats:</span></span>

* <span data-ttu-id="f3960-192">Les utilisateurs authentifiés (connectés) comme étant autorisés.</span><span class="sxs-lookup"><span data-stu-id="f3960-192">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="f3960-193">Les utilisateurs non authentifiés (déconnectés) comme étant non autorisés.</span><span class="sxs-lookup"><span data-stu-id="f3960-193">Unauthenticated (signed-out) users as unauthorized.</span></span>

### <a name="role-based-and-policy-based-authorization"></a><span data-ttu-id="f3960-194">Autorisation en fonction du rôle et de la stratégie</span><span class="sxs-lookup"><span data-stu-id="f3960-194">Role-based and policy-based authorization</span></span>

<span data-ttu-id="f3960-195">Le composant `AuthorizeView` prend en charge l’autorisation *basée sur le rôle* et *basée sur les stratégies*.</span><span class="sxs-lookup"><span data-stu-id="f3960-195">The `AuthorizeView` component supports *role-based* or *policy-based* authorization.</span></span>

<span data-ttu-id="f3960-196">Pour l’autorisation en fonction du rôle, utilisez le paramètre `Roles` :</span><span class="sxs-lookup"><span data-stu-id="f3960-196">For role-based authorization, use the `Roles` parameter:</span></span>

```razor
<AuthorizeView Roles="admin, superuser">
    <p>You can only see this if you're an admin or superuser.</p>
</AuthorizeView>
```

<span data-ttu-id="f3960-197">Pour plus d’informations, consultez <xref:security/authorization/roles>.</span><span class="sxs-lookup"><span data-stu-id="f3960-197">For more information, see <xref:security/authorization/roles>.</span></span>

<span data-ttu-id="f3960-198">Pour l’autorisation en fonction des stratégies, utilisez le paramètre `Policy` :</span><span class="sxs-lookup"><span data-stu-id="f3960-198">For policy-based authorization, use the `Policy` parameter:</span></span>

```razor
<AuthorizeView Policy="content-editor">
    <p>You can only see this if you satisfy the "content-editor" policy.</p>
</AuthorizeView>
```

<span data-ttu-id="f3960-199">L’autorisation basée sur les revendications est un cas spécial d’autorisation basée sur les stratégies.</span><span class="sxs-lookup"><span data-stu-id="f3960-199">Claims-based authorization is a special case of policy-based authorization.</span></span> <span data-ttu-id="f3960-200">Par exemple, vous pouvez définir une stratégie qui impose aux utilisateurs d’avoir une certaine revendication.</span><span class="sxs-lookup"><span data-stu-id="f3960-200">For example, you can define a policy that requires users to have a certain claim.</span></span> <span data-ttu-id="f3960-201">Pour plus d’informations, consultez <xref:security/authorization/policies>.</span><span class="sxs-lookup"><span data-stu-id="f3960-201">For more information, see <xref:security/authorization/policies>.</span></span>

<span data-ttu-id="f3960-202">Ces API peuvent être utilisées dans les applications Blazor Server ou Blazor webassembly.</span><span class="sxs-lookup"><span data-stu-id="f3960-202">These APIs can be used in either Blazor Server or Blazor WebAssembly apps.</span></span>

<span data-ttu-id="f3960-203">Si ni `Roles` ni `Policy` n’est spécifié, `AuthorizeView` utilise la stratégie par défaut.</span><span class="sxs-lookup"><span data-stu-id="f3960-203">If neither `Roles` nor `Policy` is specified, `AuthorizeView` uses the default policy.</span></span>

### <a name="content-displayed-during-asynchronous-authentication"></a><span data-ttu-id="f3960-204">Contenu affiché lors de l’authentification asynchrone</span><span class="sxs-lookup"><span data-stu-id="f3960-204">Content displayed during asynchronous authentication</span></span>

Blazor<span data-ttu-id="f3960-205"> permet de déterminer l’état d’authentification de *manière asynchrone*.</span><span class="sxs-lookup"><span data-stu-id="f3960-205"> allows for authentication state to be determined *asynchronously*.</span></span> <span data-ttu-id="f3960-206">Le scénario principal de cette approche se trouve dans Blazor applications webassembly qui effectuent une demande à un point de terminaison externe pour l’authentification.</span><span class="sxs-lookup"><span data-stu-id="f3960-206">The primary scenario for this approach is in Blazor WebAssembly apps that make a request to an external endpoint for authentication.</span></span>

<span data-ttu-id="f3960-207">Lorsque l’authentification est en cours, `AuthorizeView` n’affiche aucun contenu par défaut.</span><span class="sxs-lookup"><span data-stu-id="f3960-207">While authentication is in progress, `AuthorizeView` displays no content by default.</span></span> <span data-ttu-id="f3960-208">Pour afficher le contenu pendant que l’authentification se produit, utilisez l’élément `<Authorizing>` :</span><span class="sxs-lookup"><span data-stu-id="f3960-208">To display content while authentication occurs, use the `<Authorizing>` element:</span></span>

```razor
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

<span data-ttu-id="f3960-209">Cette approche n’est normalement pas applicable aux applications Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="f3960-209">This approach isn't normally applicable to Blazor Server apps.</span></span> Blazor<span data-ttu-id="f3960-210"> applications serveur connaissent l’état d’authentification dès que l’État est établi.</span><span class="sxs-lookup"><span data-stu-id="f3960-210"> Server apps know the authentication state as soon as the state is established.</span></span> <span data-ttu-id="f3960-211">`Authorizing` contenu peut être fourni dans le composant `AuthorizeView` d’une application Blazor Server, mais le contenu n’est jamais affiché.</span><span class="sxs-lookup"><span data-stu-id="f3960-211">`Authorizing` content can be provided in a Blazor Server app's `AuthorizeView` component, but the content is never displayed.</span></span>

## <a name="authorize-attribute"></a><span data-ttu-id="f3960-212">Attribut [Authorize]</span><span class="sxs-lookup"><span data-stu-id="f3960-212">[Authorize] attribute</span></span>

<span data-ttu-id="f3960-213">L’attribut `[Authorize]` peut être utilisé dans les composants Razor :</span><span class="sxs-lookup"><span data-stu-id="f3960-213">The `[Authorize]` attribute can be used in Razor components:</span></span>

```razor
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!NOTE]
> <span data-ttu-id="f3960-214">Dans un composant d’application Blazor webassembly, ajoutez l’espace de noms `Microsoft.AspNetCore.Authorization` (`@using Microsoft.AspNetCore.Authorization`) aux exemples de cette section.</span><span class="sxs-lookup"><span data-stu-id="f3960-214">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Authorization` namespace (`@using Microsoft.AspNetCore.Authorization`) to the examples in this section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f3960-215">N’utilisez `[Authorize]` sur `@page` composants atteints via le routeur Blazor.</span><span class="sxs-lookup"><span data-stu-id="f3960-215">Only use `[Authorize]` on `@page` components reached via the Blazor Router.</span></span> <span data-ttu-id="f3960-216">L’autorisation est effectuée uniquement en tant qu’aspect du routage et *pas* pour les composants enfants rendus dans une page.</span><span class="sxs-lookup"><span data-stu-id="f3960-216">Authorization is only performed as an aspect of routing and *not* for child components rendered within a page.</span></span> <span data-ttu-id="f3960-217">Pour autoriser l’affichage d’éléments spécifiques dans une page, utilisez `AuthorizeView` à la place.</span><span class="sxs-lookup"><span data-stu-id="f3960-217">To authorize the display of specific parts within a page, use `AuthorizeView` instead.</span></span>

<span data-ttu-id="f3960-218">L’attribut `[Authorize]` prend également en charge l’autorisation en fonction du rôle ou des stratégies.</span><span class="sxs-lookup"><span data-stu-id="f3960-218">The `[Authorize]` attribute also supports role-based or policy-based authorization.</span></span> <span data-ttu-id="f3960-219">Pour l’autorisation en fonction du rôle, utilisez le paramètre `Roles` :</span><span class="sxs-lookup"><span data-stu-id="f3960-219">For role-based authorization, use the `Roles` parameter:</span></span>

```razor
@page "/"
@attribute [Authorize(Roles = "admin, superuser")]

<p>You can only see this if you're in the 'admin' or 'superuser' role.</p>
```

<span data-ttu-id="f3960-220">Pour l’autorisation en fonction des stratégies, utilisez le paramètre `Policy` :</span><span class="sxs-lookup"><span data-stu-id="f3960-220">For policy-based authorization, use the `Policy` parameter:</span></span>

```razor
@page "/"
@attribute [Authorize(Policy = "content-editor")]

<p>You can only see this if you satisfy the 'content-editor' policy.</p>
```

<span data-ttu-id="f3960-221">Si ni `Roles` ni `Policy` n’est spécifié, `[Authorize]` utilise la stratégie par défaut, qui consiste par défaut à traiter :</span><span class="sxs-lookup"><span data-stu-id="f3960-221">If neither `Roles` nor `Policy` is specified, `[Authorize]` uses the default policy, which by default is to treat:</span></span>

* <span data-ttu-id="f3960-222">Les utilisateurs authentifiés (connectés) comme étant autorisés.</span><span class="sxs-lookup"><span data-stu-id="f3960-222">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="f3960-223">Les utilisateurs non authentifiés (déconnectés) comme étant non autorisés.</span><span class="sxs-lookup"><span data-stu-id="f3960-223">Unauthenticated (signed-out) users as unauthorized.</span></span>

## <a name="customize-unauthorized-content-with-the-router-component"></a><span data-ttu-id="f3960-224">Personnaliser le contenu non autorisé avec le composant Router</span><span class="sxs-lookup"><span data-stu-id="f3960-224">Customize unauthorized content with the Router component</span></span>

<span data-ttu-id="f3960-225">Le composant `Router`, conjointement au composant `AuthorizeRouteView`, permet à l’application de spécifier du contenu personnalisé dans les cas suivants :</span><span class="sxs-lookup"><span data-stu-id="f3960-225">The `Router` component, in conjunction with the `AuthorizeRouteView` component, allows the app to specify custom content if:</span></span>

* <span data-ttu-id="f3960-226">Le contenu est introuvable.</span><span class="sxs-lookup"><span data-stu-id="f3960-226">Content isn't found.</span></span>
* <span data-ttu-id="f3960-227">L’utilisateur ne répond pas à une condition `[Authorize]` appliquée au composant.</span><span class="sxs-lookup"><span data-stu-id="f3960-227">The user fails an `[Authorize]` condition applied to the component.</span></span> <span data-ttu-id="f3960-228">L’attribut `[Authorize]` est abordé dans la section [`[Authorize]` attribut](#authorize-attribute) .</span><span class="sxs-lookup"><span data-stu-id="f3960-228">The `[Authorize]` attribute is covered in the [`[Authorize]` attribute](#authorize-attribute) section.</span></span>
* <span data-ttu-id="f3960-229">Une authentification asynchrone est en cours.</span><span class="sxs-lookup"><span data-stu-id="f3960-229">Asynchronous authentication is in progress.</span></span>

<span data-ttu-id="f3960-230">Dans le modèle de projet Blazor Server par défaut, le fichier *app. Razor* montre comment définir un contenu personnalisé :</span><span class="sxs-lookup"><span data-stu-id="f3960-230">In the default Blazor Server project template, the *App.razor* file demonstrates how to set custom content:</span></span>

```razor
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

<span data-ttu-id="f3960-231">Le contenu des balises `<NotFound>`, `<NotAuthorized>`et `<Authorizing>` peut inclure des éléments arbitraires, tels que d’autres composants interactifs.</span><span class="sxs-lookup"><span data-stu-id="f3960-231">The content of `<NotFound>`, `<NotAuthorized>`, and `<Authorizing>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="f3960-232">Si l’élément `<NotAuthorized>` n’est pas spécifié, le `AuthorizeRouteView` utilise le message de secours suivant :</span><span class="sxs-lookup"><span data-stu-id="f3960-232">If the `<NotAuthorized>` element isn't specified, the `AuthorizeRouteView` uses the following fallback message:</span></span>

```html
Not authorized.
```

## <a name="notification-about-authentication-state-changes"></a><span data-ttu-id="f3960-233">Notification sur les changements d’état de l’authentification</span><span class="sxs-lookup"><span data-stu-id="f3960-233">Notification about authentication state changes</span></span>

<span data-ttu-id="f3960-234">Si l’application détermine que les données d’état d’authentification sous-jacentes ont changé (par exemple, parce que l’utilisateur s’est déconnecté ou qu’un autre utilisateur a modifié ses rôles), un `AuthenticationStateProvider` personnalisé peut éventuellement appeler la méthode `NotifyAuthenticationStateChanged` sur la classe de base `AuthenticationStateProvider`.</span><span class="sxs-lookup"><span data-stu-id="f3960-234">If the app determines that the underlying authentication state data has changed (for example, because the user signed out or another user has changed their roles), a custom `AuthenticationStateProvider` can optionally invoke the method `NotifyAuthenticationStateChanged` on the `AuthenticationStateProvider` base class.</span></span> <span data-ttu-id="f3960-235">Cela prévient les consommateurs des données d’état d’authentification (par exemple, `AuthorizeView`) qu’elles doivent appliquer un nouveau rendu à l’aide des nouvelles données.</span><span class="sxs-lookup"><span data-stu-id="f3960-235">This notifies consumers of the authentication state data (for example, `AuthorizeView`) to rerender using the new data.</span></span>

## <a name="procedural-logic"></a><span data-ttu-id="f3960-236">Logique procédurale</span><span class="sxs-lookup"><span data-stu-id="f3960-236">Procedural logic</span></span>

<span data-ttu-id="f3960-237">Si l’application est nécessaire pour vérifier les règles d’autorisation dans le cadre de la logique procédurale, utilisez un paramètre en cascade de type `Task<AuthenticationState>` pour obtenir le <xref:System.Security.Claims.ClaimsPrincipal> de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f3960-237">If the app is required to check authorization rules as part of procedural logic, use a cascaded parameter of type `Task<AuthenticationState>` to obtain the user's <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="f3960-238">`Task<AuthenticationState>` peut être combiné avec d’autres services, comme `IAuthorizationService`, pour évaluer les stratégies.</span><span class="sxs-lookup"><span data-stu-id="f3960-238">`Task<AuthenticationState>` can be combined with other services, such as `IAuthorizationService`, to evaluate policies.</span></span>

```razor
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
> <span data-ttu-id="f3960-239">Dans un composant d’application Blazor webassembly, ajoutez les espaces de noms `Microsoft.AspNetCore.Authorization` et `Microsoft.AspNetCore.Components.Authorization` :</span><span class="sxs-lookup"><span data-stu-id="f3960-239">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Authorization` and `Microsoft.AspNetCore.Components.Authorization` namespaces:</span></span>
>
> ```razor
> @using Microsoft.AspNetCore.Authorization
> @using Microsoft.AspNetCore.Components.Authorization
> ```

## <a name="authorization-in-opno-locblazor-webassembly-apps"></a><span data-ttu-id="f3960-240">Autorisation dans Blazor applications webassembly</span><span class="sxs-lookup"><span data-stu-id="f3960-240">Authorization in Blazor WebAssembly apps</span></span>

<span data-ttu-id="f3960-241">Dans Blazor applications webassembly, les vérifications d’autorisation peuvent être ignorées, car le code côté client peut être modifié par les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="f3960-241">In Blazor WebAssembly apps, authorization checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="f3960-242">Cela vaut également pour toutes les technologies d’application côté client, y compris les infrastructures d’application JavaScript SPA ou les applications natives pour n’importe quel système d’exploitation.</span><span class="sxs-lookup"><span data-stu-id="f3960-242">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="f3960-243">**Effectuez toujours les vérifications d’autorisation sur le serveur au sein des points de terminaison de l’API auxquels votre application côté client accède.**</span><span class="sxs-lookup"><span data-stu-id="f3960-243">**Always perform authorization checks on the server within any API endpoints accessed by your client-side app.**</span></span>

<span data-ttu-id="f3960-244">Pour plus d’informations, consultez les articles sous <xref:security/blazor/webassembly/index>.</span><span class="sxs-lookup"><span data-stu-id="f3960-244">For more information, see the articles under <xref:security/blazor/webassembly/index>.</span></span>

## <a name="troubleshoot-errors"></a><span data-ttu-id="f3960-245">Résoudre les erreurs</span><span class="sxs-lookup"><span data-stu-id="f3960-245">Troubleshoot errors</span></span>

<span data-ttu-id="f3960-246">Erreurs courantes :</span><span class="sxs-lookup"><span data-stu-id="f3960-246">Common errors:</span></span>

* <span data-ttu-id="f3960-247">**L’autorisation nécessite un paramètre en cascade de type Task\<AuthenticationState >. Envisagez d’utiliser CascadingAuthenticationState pour fournir ce.**</span><span class="sxs-lookup"><span data-stu-id="f3960-247">**Authorization requires a cascading parameter of type Task\<AuthenticationState>. Consider using CascadingAuthenticationState to supply this.**</span></span>

* <span data-ttu-id="f3960-248">**La valeur `null` est reçue pour `authenticationStateTask`**</span><span class="sxs-lookup"><span data-stu-id="f3960-248">**`null` value is received for `authenticationStateTask`**</span></span>

<span data-ttu-id="f3960-249">Il est probable que le projet n’a pas été créé à l’aide d’un modèle de serveur Blazor avec l’authentification activée.</span><span class="sxs-lookup"><span data-stu-id="f3960-249">It's likely that the project wasn't created using a Blazor Server template with authentication enabled.</span></span> <span data-ttu-id="f3960-250">Encapsulez un `<CascadingAuthenticationState>` dans certaines parties de l’arborescence de l’interface utilisateur, par exemple dans *App.razor*, comme suit :</span><span class="sxs-lookup"><span data-stu-id="f3960-250">Wrap a `<CascadingAuthenticationState>` around some part of the UI tree, for example in *App.razor* as follows:</span></span>

```razor
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CascadingAuthenticationState>
```

<span data-ttu-id="f3960-251">`CascadingAuthenticationState` fournit le paramètre en cascade `Task<AuthenticationState>`, qu’il reçoit à son tour du service d’injection de dépendances `AuthenticationStateProvider` sous-jacent.</span><span class="sxs-lookup"><span data-stu-id="f3960-251">The `CascadingAuthenticationState` supplies the `Task<AuthenticationState>` cascading parameter, which in turn it receives from the underlying `AuthenticationStateProvider` DI service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f3960-252">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="f3960-252">Additional resources</span></span>

* <xref:security/index>
* <xref:security/blazor/server>
* <xref:security/authentication/windowsauth>
* <span data-ttu-id="f3960-253">[Isard Blazor:](https://github.com/AdrienTorris/awesome-blazor#authentication) liens vers des exemples de communauté d’authentification</span><span class="sxs-lookup"><span data-stu-id="f3960-253">[Awesome Blazor: Authentication](https://github.com/AdrienTorris/awesome-blazor#authentication) community sample links</span></span>
