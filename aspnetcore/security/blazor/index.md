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
# <a name="aspnet-core-blazor-authentication-and-authorization"></a>Authentification et autorisation avec ASP.NET Core Blazor

Par [Steve Sanderson](https://github.com/SteveSandersonMS)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

ASP.NET Core prend en charge la configuration et la gestion de la sécurité dans les applications Blazor.

Les scénarios de sécurité diffèrent entre le serveur éblouissant et les applications webassembly éblouissantes. Étant donné que les applications serveur éblouissantes s’exécutent sur le serveur, les contrôles d’autorisation peuvent déterminer les éléments suivants :

* Les options de l’interface utilisateur présentées à un utilisateur (par exemple, les entrées de menu disponibles pour un utilisateur).
* Les règles d’accès pour les zones de l’application et les composants.

Les applications webassembly éblouissant s’exécutent sur le client. L’autorisation est *uniquement* utilisée pour déterminer les options de l’interface utilisateur à afficher. Étant donné que les contrôles côté client peuvent être modifiés ou ignorés par un utilisateur, une application de webassembly éblouissante ne peut pas appliquer les règles d’accès aux autorisations.

[Razor pages conventions d’autorisation](xref:security/authorization/razor-pages-authorization) ne s’appliquent pas aux composants Razor routables. Si un composant Razor non routable est [incorporé dans une page](xref:blazor/integrate-components#render-components-from-a-page-or-view), les conventions d’autorisation de la page affectent indirectement le composant Razor avec le reste du contenu de la page.

## <a name="authentication"></a>Authentication

Blazor utilise les mécanismes d’authentification ASP.NET Core existants pour établir l’identité de l’utilisateur. Le mécanisme exact dépend de la façon dont l’application éblouissant est hébergée, éblouissante Server ou éblouissant webassembly.

### <a name="blazor-server-authentication"></a>Authentification du serveur éblouissant

Les applications serveur éblouissantes fonctionnent sur une connexion en temps réel créée à l’aide de Signalr. [L’authentification dans les applications basées sur SignalR](xref:signalr/authn-and-authz) est gérée lorsque la connexion est établie. L’authentification peut reposer sur un cookie ou un autre jeton du porteur.

Le modèle de projet de serveur éblouissant peut configurer l’authentification à votre place lors de la création du projet.

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

Suivez les instructions de Visual Studio dans l’article <xref:blazor/get-started> pour créer un projet de serveur éblouissant avec un mécanisme d’authentification.

Après avoir choisi le modèle **Application serveur Blazor** dans la boîte de dialogue **Créer une application web ASP.NET Core.** , sélectionnez **Modifier** sous **Authentification**.

Une boîte de dialogue s’ouvre pour offrir le même ensemble de mécanismes d’authentification que ceux disponibles pour les autres projets ASP.NET Core :

* **Aucune authentification**
* **Les comptes d’utilisateurs individuels &ndash; les** comptes d’utilisateur peuvent être stockés :
  * Dans l’application à l’aide du système [d’identité](xref:security/authentication/identity) d’ASP.NET Core.
  * Avec [Azure AD B2C](xref:security/authentication/azure-ad-b2c).
* **Comptes professionnels ou scolaires**
* **Authentification Windows**

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Suivez les instructions de Visual Studio Code dans l’article <xref:blazor/get-started> pour créer un projet de serveur éblouissant avec un mécanisme d’authentification :

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

Les valeurs autorisées d’authentification (`{AUTHENTICATION}`) sont présentées dans le tableau suivant.

| Mécanisme d’authentification                                                                 | Valeur `{AUTHENTICATION}` |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| Aucune authentification                                                                        | `None`                   |
| Individuel<br>Les utilisateurs stockés dans l’application avec l’identité ASP.NET Core.                        | `Individual`             |
| Individuel<br>Les utilisateurs stockés dans [Azure AD B2C](xref:security/authentication/azure-ad-b2c). | `IndividualB2C`          |
| Comptes professionnels ou scolaires<br>Authentification d’organisation pour un seul abonné.            | `SingleOrg`              |
| Comptes professionnels ou scolaires<br>Authentification d’organisation pour plusieurs abonnés.           | `MultiOrg`               |
| Authentification Windows                                                                   | `Windows`                |

La commande crée un dossier nommé avec la valeur fournie pour l’espace réservé `{APP NAME}` et utilise le nom de dossier en tant que nom de l’application. Pour plus d’informations, consultez la commande [dotnet new](/dotnet/core/tools/dotnet-new) dans le Guide .NET Core.

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

### <a name="opno-locblazor-webassembly-authentication"></a>Blazor l’authentification webassembly

Dans Blazor applications webassembly, les vérifications d’authentification peuvent être contournées, car le code côté client peut être modifié par les utilisateurs. Cela vaut également pour toutes les technologies d’application côté client, y compris les infrastructures d’application JavaScript SPA ou les applications natives pour n’importe quel système d’exploitation.

Ajoutez une référence de package pour [Microsoft. AspNetCore. Components. Authorization](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/) au fichier projet de l’application.

L’implémentation d’un service de `AuthenticationStateProvider` personnalisé pour Blazor applications webassembly est traitée dans les sections suivantes.

## <a name="authenticationstateprovider-service"></a>Service AuthenticationStateProvider

Blazor applications serveur incluent un service `AuthenticationStateProvider` intégré qui obtient des données d’état d’authentification du `HttpContext.User`de ASP.NET Core. Il s’agit de la façon dont l’état d’authentification s’intègre avec les mécanismes d’authentification ASP.NET Core côté serveur existants.

`AuthenticationStateProvider` est le service sous-jacent utilisé par le composant `AuthorizeView` et le composant `CascadingAuthenticationState` pour obtenir l’état d’authentification.

Vous n’utilisez généralement pas `AuthenticationStateProvider` directement. Utilisez les approches [Composant AuthorizeView](#authorizeview-component) ou [Tâche<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) décrites plus loin dans cet article. Le principal inconvénient de l’utilisation directe de `AuthenticationStateProvider` est que le composant n’est pas automatiquement averti en cas de modifications de données d’état de l’authentification sous-jacente.

Le service `AuthenticationStateProvider` peut fournir les données <xref:System.Security.Claims.ClaimsPrincipal> de l’utilisateur actuel, comme indiqué dans l’exemple suivant :

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

Si `user.Identity.IsAuthenticated` est `true` et parce que l’utilisateur est <xref:System.Security.Claims.ClaimsPrincipal>, les revendications peuvent être énumérées et l’appartenance aux rôles évaluée.

Pour plus d’informations sur l’injection de dépendances et les services, consultez <xref:blazor/dependency-injection> et <xref:fundamentals/dependency-injection>.

## <a name="implement-a-custom-authenticationstateprovider"></a>Implémenter un AuthenticationStateProvider personnalisé

Si vous générez une application Blazor webassembly ou si la spécification de votre application requiert absolument un fournisseur personnalisé, implémentez un fournisseur et remplacez `GetAuthenticationStateAsync`:

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

Dans une application Blazor webassembly, le service `CustomAuthStateProvider` est inscrit dans `Main` de *Program.cs*:

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

Avec `CustomAuthStateProvider`, tous les utilisateurs sont authentifiés avec le nom d’utilisateur `mrfibuli`.

## <a name="expose-the-authentication-state-as-a-cascading-parameter"></a>Exposer l’état d’authentification comme un paramètre en cascade

Si les données d’état d’authentification sont requises pour la logique procédurale, par exemple lors d’une action déclenchée par l’utilisateur, obtenez les données d’état d’authentification en définissant un paramètre en cascade de type `Task<AuthenticationState>` :

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
> Dans un composant d’application Blazor webassembly, ajoutez l’espace de noms `Microsoft.AspNetCore.Components.Authorization` (`@using Microsoft.AspNetCore.Components.Authorization`).

Si `user.Identity.IsAuthenticated` est `true`, les revendications peuvent être énumérées et l’appartenance aux rôles évaluée.

Configurez le `Task<AuthenticationState>` paramètre en cascade à l’aide des composants `AuthorizeRouteView` et `CascadingAuthenticationState` dans le fichier *app. Razor* :

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

Ajouter des services pour les options et l’autorisation d' `Program.Main`:

```csharp
builder.Services.AddOptions();
builder.Services.AddAuthorizationCore();
```

## <a name="authorization"></a>Autorisation

Une fois qu’un utilisateur est authentifié, les règles *d’autorisation* sont appliquées pour contrôler ce que l’utilisateur peut faire.

L’accès est généralement accordé ou refusé selon les conditions suivantes :

* Un utilisateur est authentifié (connecté).
* Un utilisateur appartient à un *rôle*.
* Un utilisateur a une *revendication*.
* Une *stratégie* est satisfaite.

Tous ces concepts sont les mêmes que dans une application ASP.NET Core MVC ou Razor Pages. Pour plus d’informations sur la sécurité d’ASP.NET Core, consultez les articles sous [Sécurité et identité dans ASP.NET Core](xref:security/index).

## <a name="authorizeview-component"></a>Composant AuthorizeView

Le composant `AuthorizeView` affiche sélectivement l’interface utilisateur en fonction de l’autorisation que l’utilisateur a pour l’afficher. Cette approche est utile lorsque vous devez uniquement *afficher* les données de l’utilisateur et que vous n’avez pas besoin d’utiliser l’identité de l’utilisateur dans la logique procédurale.

Le composant expose une variable `context` de type `AuthenticationState`, que vous pouvez utiliser pour accéder aux informations relatives à l’utilisateur connecté :

```razor
<AuthorizeView>
    <h1>Hello, @context.User.Identity.Name!</h1>
    <p>You can only see this content if you're authenticated.</p>
</AuthorizeView>
```

Vous pouvez également fournir un contenu différent à afficher si l’utilisateur n’est pas authentifié :

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

Le contenu des balises `<Authorized>` et `<NotAuthorized>` peut inclure des éléments arbitraires, tels que d’autres composants interactifs.

Les conditions d’autorisation, comme les rôles ou les stratégies qui contrôlent les options d’interface utilisateur ou d’accès, sont traitées dans la section [Autorisation](#authorization).

Si les conditions d’autorisation ne sont pas spécifiées, `AuthorizeView` utilise une stratégie par défaut et traite :

* Les utilisateurs authentifiés (connectés) comme étant autorisés.
* Les utilisateurs non authentifiés (déconnectés) comme étant non autorisés.

### <a name="role-based-and-policy-based-authorization"></a>Autorisation en fonction du rôle et de la stratégie

Le composant `AuthorizeView` prend en charge l’autorisation *basée sur le rôle* et *basée sur les stratégies*.

Pour l’autorisation en fonction du rôle, utilisez le paramètre `Roles` :

```razor
<AuthorizeView Roles="admin, superuser">
    <p>You can only see this if you're an admin or superuser.</p>
</AuthorizeView>
```

Pour plus d’informations, consultez <xref:security/authorization/roles>.

Pour l’autorisation en fonction des stratégies, utilisez le paramètre `Policy` :

```razor
<AuthorizeView Policy="content-editor">
    <p>You can only see this if you satisfy the "content-editor" policy.</p>
</AuthorizeView>
```

L’autorisation basée sur les revendications est un cas spécial d’autorisation basée sur les stratégies. Par exemple, vous pouvez définir une stratégie qui impose aux utilisateurs d’avoir une certaine revendication. Pour plus d’informations, consultez <xref:security/authorization/policies>.

Ces API peuvent être utilisées dans les applications Blazor Server ou Blazor webassembly.

Si ni `Roles` ni `Policy` n’est spécifié, `AuthorizeView` utilise la stratégie par défaut.

### <a name="content-displayed-during-asynchronous-authentication"></a>Contenu affiché lors de l’authentification asynchrone

Blazor permet de déterminer l’état d’authentification de *manière asynchrone*. Le scénario principal de cette approche se trouve dans Blazor applications webassembly qui effectuent une demande à un point de terminaison externe pour l’authentification.

Lorsque l’authentification est en cours, `AuthorizeView` n’affiche aucun contenu par défaut. Pour afficher le contenu pendant que l’authentification se produit, utilisez l’élément `<Authorizing>` :

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

Cette approche n’est normalement pas applicable aux applications Blazor Server. Blazor applications serveur connaissent l’état d’authentification dès que l’État est établi. `Authorizing` contenu peut être fourni dans le composant `AuthorizeView` d’une application Blazor Server, mais le contenu n’est jamais affiché.

## <a name="authorize-attribute"></a>Attribut [Authorize]

L’attribut `[Authorize]` peut être utilisé dans les composants Razor :

```razor
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!NOTE]
> Dans un composant d’application Blazor webassembly, ajoutez l’espace de noms `Microsoft.AspNetCore.Authorization` (`@using Microsoft.AspNetCore.Authorization`) aux exemples de cette section.

> [!IMPORTANT]
> N’utilisez `[Authorize]` sur `@page` composants atteints via le routeur Blazor. L’autorisation est effectuée uniquement en tant qu’aspect du routage et *pas* pour les composants enfants rendus dans une page. Pour autoriser l’affichage d’éléments spécifiques dans une page, utilisez `AuthorizeView` à la place.

L’attribut `[Authorize]` prend également en charge l’autorisation en fonction du rôle ou des stratégies. Pour l’autorisation en fonction du rôle, utilisez le paramètre `Roles` :

```razor
@page "/"
@attribute [Authorize(Roles = "admin, superuser")]

<p>You can only see this if you're in the 'admin' or 'superuser' role.</p>
```

Pour l’autorisation en fonction des stratégies, utilisez le paramètre `Policy` :

```razor
@page "/"
@attribute [Authorize(Policy = "content-editor")]

<p>You can only see this if you satisfy the 'content-editor' policy.</p>
```

Si ni `Roles` ni `Policy` n’est spécifié, `[Authorize]` utilise la stratégie par défaut, qui consiste par défaut à traiter :

* Les utilisateurs authentifiés (connectés) comme étant autorisés.
* Les utilisateurs non authentifiés (déconnectés) comme étant non autorisés.

## <a name="customize-unauthorized-content-with-the-router-component"></a>Personnaliser le contenu non autorisé avec le composant Router

Le composant `Router`, conjointement au composant `AuthorizeRouteView`, permet à l’application de spécifier du contenu personnalisé dans les cas suivants :

* Le contenu est introuvable.
* L’utilisateur ne répond pas à une condition `[Authorize]` appliquée au composant. L’attribut `[Authorize]` est abordé dans la section [`[Authorize]` attribut](#authorize-attribute) .
* Une authentification asynchrone est en cours.

Dans le modèle de projet Blazor Server par défaut, le fichier *app. Razor* montre comment définir un contenu personnalisé :

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

Le contenu des balises `<NotFound>`, `<NotAuthorized>`et `<Authorizing>` peut inclure des éléments arbitraires, tels que d’autres composants interactifs.

Si l’élément `<NotAuthorized>` n’est pas spécifié, le `AuthorizeRouteView` utilise le message de secours suivant :

```html
Not authorized.
```

## <a name="notification-about-authentication-state-changes"></a>Notification sur les changements d’état de l’authentification

Si l’application détermine que les données d’état d’authentification sous-jacentes ont changé (par exemple, parce que l’utilisateur s’est déconnecté ou qu’un autre utilisateur a modifié ses rôles), un `AuthenticationStateProvider` personnalisé peut éventuellement appeler la méthode `NotifyAuthenticationStateChanged` sur la classe de base `AuthenticationStateProvider`. Cela prévient les consommateurs des données d’état d’authentification (par exemple, `AuthorizeView`) qu’elles doivent appliquer un nouveau rendu à l’aide des nouvelles données.

## <a name="procedural-logic"></a>Logique procédurale

Si l’application est nécessaire pour vérifier les règles d’autorisation dans le cadre de la logique procédurale, utilisez un paramètre en cascade de type `Task<AuthenticationState>` pour obtenir le <xref:System.Security.Claims.ClaimsPrincipal> de l’utilisateur. `Task<AuthenticationState>` peut être combiné avec d’autres services, comme `IAuthorizationService`, pour évaluer les stratégies.

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
> Dans un composant d’application Blazor webassembly, ajoutez les espaces de noms `Microsoft.AspNetCore.Authorization` et `Microsoft.AspNetCore.Components.Authorization` :
>
> ```razor
> @using Microsoft.AspNetCore.Authorization
> @using Microsoft.AspNetCore.Components.Authorization
> ```

## <a name="authorization-in-opno-locblazor-webassembly-apps"></a>Autorisation dans Blazor applications webassembly

Dans Blazor applications webassembly, les vérifications d’autorisation peuvent être ignorées, car le code côté client peut être modifié par les utilisateurs. Cela vaut également pour toutes les technologies d’application côté client, y compris les infrastructures d’application JavaScript SPA ou les applications natives pour n’importe quel système d’exploitation.

**Effectuez toujours les vérifications d’autorisation sur le serveur au sein des points de terminaison de l’API auxquels votre application côté client accède.**

Pour plus d’informations, consultez les articles sous <xref:security/blazor/webassembly/index>.

## <a name="troubleshoot-errors"></a>Résoudre les erreurs

Erreurs courantes :

* **L’autorisation nécessite un paramètre en cascade de type Task\<AuthenticationState >. Envisagez d’utiliser CascadingAuthenticationState pour fournir ce.**

* **La valeur `null` est reçue pour `authenticationStateTask`**

Il est probable que le projet n’a pas été créé à l’aide d’un modèle de serveur Blazor avec l’authentification activée. Encapsulez un `<CascadingAuthenticationState>` dans certaines parties de l’arborescence de l’interface utilisateur, par exemple dans *App.razor*, comme suit :

```razor
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CascadingAuthenticationState>
```

`CascadingAuthenticationState` fournit le paramètre en cascade `Task<AuthenticationState>`, qu’il reçoit à son tour du service d’injection de dépendances `AuthenticationStateProvider` sous-jacent.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:security/index>
* <xref:security/blazor/server>
* <xref:security/authentication/windowsauth>
* [Isard Blazor:](https://github.com/AdrienTorris/awesome-blazor#authentication) liens vers des exemples de communauté d’authentification
