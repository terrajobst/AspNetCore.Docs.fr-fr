---
title: Scénarios de sécurité supplémentaires de ASP.NET Core Blazor webassembly
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/additional-scenarios
ms.openlocfilehash: ccb512392341e3eea33f4ab45740b7900f7b63f9
ms.sourcegitcommit: 9b6e7f421c243963d5e419bdcfc5c4bde71499aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2020
ms.locfileid: "79989454"
---
# <a name="aspnet-core-blazor-webassembly-additional-security-scenarios"></a><span data-ttu-id="1520a-102">Scénarios de sécurité supplémentaires ASP.NET Core éblouissant webassembly</span><span class="sxs-lookup"><span data-stu-id="1520a-102">ASP.NET Core Blazor WebAssembly additional security scenarios</span></span>

<span data-ttu-id="1520a-103">Par [Javier Calvarro Nelson](https://github.com/javiercn)</span><span class="sxs-lookup"><span data-stu-id="1520a-103">By [Javier Calvarro Nelson](https://github.com/javiercn)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

## <a name="handle-token-request-errors"></a><span data-ttu-id="1520a-104">Gérer les erreurs de demande de jeton</span><span class="sxs-lookup"><span data-stu-id="1520a-104">Handle token request errors</span></span>

<span data-ttu-id="1520a-105">Lorsqu’une application à page unique (SPA) authentifie un utilisateur à l’aide d’Open ID Connect (OIDC), l’état d’authentification est conservé localement au sein du SPA et dans le fournisseur d’identité (IP) sous la forme d’un cookie de session défini à la suite de l’utilisateur qui fournit son informations d’identification.</span><span class="sxs-lookup"><span data-stu-id="1520a-105">When a Single Page Application (SPA) authenticates a user using Open ID Connect (OIDC), the authentication state is maintained locally within the SPA and in the Identity Provider (IP) in the form of a session cookie that's set as a result of the user providing their credentials.</span></span>

<span data-ttu-id="1520a-106">Les jetons que l’adresse IP émet pour l’utilisateur sont généralement valides pendant de courtes périodes, environ une heure normalement, de sorte que l’application cliente doit régulièrement extraire les nouveaux jetons.</span><span class="sxs-lookup"><span data-stu-id="1520a-106">The tokens that the IP emits for the user typically are valid for short periods of time, about one hour normally, so the client app must regularly fetch new tokens.</span></span> <span data-ttu-id="1520a-107">Dans le cas contraire, l’utilisateur est déconnecté après l’expiration des jetons accordés.</span><span class="sxs-lookup"><span data-stu-id="1520a-107">Otherwise, the user would be logged-out after the granted tokens expire.</span></span> <span data-ttu-id="1520a-108">Dans la plupart des cas, les clients OIDC sont en mesure de configurer de nouveaux jetons sans que l’utilisateur soit obligé de s’authentifier à nouveau grâce à l’état d’authentification ou à la « session » qui est conservée au sein de l’adresse IP.</span><span class="sxs-lookup"><span data-stu-id="1520a-108">In most cases, OIDC clients are able to provision new tokens without requiring the user to authenticate again thanks to the authentication state or "session" that is kept within the IP.</span></span>

<span data-ttu-id="1520a-109">Dans certains cas, le client ne peut pas obtenir un jeton sans intervention de l’utilisateur, par exemple, lorsque, pour une raison quelconque, l’utilisateur se déconnecte explicitement de l’adresse IP.</span><span class="sxs-lookup"><span data-stu-id="1520a-109">There are some cases in which the client can't get a token without user interaction, for example, when for some reason the user explicitly logs out from the IP.</span></span> <span data-ttu-id="1520a-110">Ce scénario se produit si un utilisateur visite `https://login.microsoftonline.com` et se déconnecte. Dans ces scénarios, l’application ne sait pas immédiatement que l’utilisateur s’est déconnecté. Tout jeton que le client contient peut ne plus être valide.</span><span class="sxs-lookup"><span data-stu-id="1520a-110">This scenario occurs if a user visits `https://login.microsoftonline.com` and logs out. In these scenarios, the app doesn't know immediately that the user has logged out. Any token that the client holds might no longer be valid.</span></span> <span data-ttu-id="1520a-111">En outre, le client n’est pas en mesure d’approvisionner un nouveau jeton sans interaction de l’utilisateur après l’expiration du jeton actuel.</span><span class="sxs-lookup"><span data-stu-id="1520a-111">Also, the client isn't able to provision a new token without user interaction after the current token expires.</span></span>

<span data-ttu-id="1520a-112">Ces scénarios ne sont pas spécifiques à l’authentification basée sur les jetons.</span><span class="sxs-lookup"><span data-stu-id="1520a-112">These scenarios aren't specific to token-based authentication.</span></span> <span data-ttu-id="1520a-113">Elles font partie de la nature des SPAs.</span><span class="sxs-lookup"><span data-stu-id="1520a-113">They are part of the nature of SPAs.</span></span> <span data-ttu-id="1520a-114">Un SPA utilisant des cookies ne peut pas non plus appeler une API serveur si le cookie d’authentification est supprimé.</span><span class="sxs-lookup"><span data-stu-id="1520a-114">An SPA using cookies also fails to call a server API if the authentication cookie is removed.</span></span>

<span data-ttu-id="1520a-115">Quand une application effectue des appels d’API vers des ressources protégées, vous devez tenir compte des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="1520a-115">When an app performs API calls to protected resources, you must be aware of the following:</span></span>

* <span data-ttu-id="1520a-116">Pour approvisionner un nouveau jeton d’accès pour appeler l’API, l’utilisateur peut être amené à s’authentifier à nouveau.</span><span class="sxs-lookup"><span data-stu-id="1520a-116">To provision a new access token to call the API, the user might be required to authenticate again.</span></span>
* <span data-ttu-id="1520a-117">Même si le client possède un jeton qui semble être valide, l’appel au serveur peut échouer parce que le jeton a été révoqué par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="1520a-117">Even if the client has a token that seems to be valid, the call to the server might fail because the token was revoked by the user.</span></span>

<span data-ttu-id="1520a-118">Lorsque l’application demande un jeton, deux résultats sont possibles :</span><span class="sxs-lookup"><span data-stu-id="1520a-118">When the app requests a token, there are two possible outcomes:</span></span>

* <span data-ttu-id="1520a-119">La demande a échoué et l’application a un jeton valide.</span><span class="sxs-lookup"><span data-stu-id="1520a-119">The request succeeds, and the app has a valid token.</span></span>
* <span data-ttu-id="1520a-120">La demande échoue et l’application doit authentifier à nouveau l’utilisateur pour obtenir un nouveau jeton.</span><span class="sxs-lookup"><span data-stu-id="1520a-120">The request fails, and the app must authenticate the user again to obtain a new token.</span></span>

<span data-ttu-id="1520a-121">En cas d’échec d’une demande de jeton, vous devez décider si vous souhaitez enregistrer un état actuel avant d’effectuer une redirection.</span><span class="sxs-lookup"><span data-stu-id="1520a-121">When a token request fails, you need to decide whether you want to save any current state before you perform a redirection.</span></span> <span data-ttu-id="1520a-122">Il existe plusieurs approches avec des niveaux de complexité de plus en plus complexes :</span><span class="sxs-lookup"><span data-stu-id="1520a-122">Several approaches exist with increasing levels of complexity:</span></span>

* <span data-ttu-id="1520a-123">Stocke l’état actuel de la page dans le stockage de session.</span><span class="sxs-lookup"><span data-stu-id="1520a-123">Store the current page state in session storage.</span></span> <span data-ttu-id="1520a-124">Pendant l' `OnInitializeAsync`, vérifiez si l’État peut être restauré avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="1520a-124">During `OnInitializeAsync`, check if state can be restored before continuing.</span></span>
* <span data-ttu-id="1520a-125">Ajoutez un paramètre de chaîne de requête et utilisez-le comme méthode pour signaler à l’application qu’elle doit réalimenter l’état enregistré précédemment.</span><span class="sxs-lookup"><span data-stu-id="1520a-125">Add a query string parameter and use that as a way to signal the app that it needs to re-hydrate the previously saved state.</span></span>
* <span data-ttu-id="1520a-126">Ajoutez un paramètre de chaîne de requête avec un identificateur unique pour stocker les données dans le stockage de session sans risquer des collisions avec d’autres éléments.</span><span class="sxs-lookup"><span data-stu-id="1520a-126">Add a query string parameter with a unique identifier to store data in session storage without risking collisions with other items.</span></span>

<span data-ttu-id="1520a-127">L’exemple suivant montre comment :</span><span class="sxs-lookup"><span data-stu-id="1520a-127">The following example shows how to:</span></span>

* <span data-ttu-id="1520a-128">Conserver l’état avant la redirection vers la page de connexion.</span><span class="sxs-lookup"><span data-stu-id="1520a-128">Preserve state before redirecting to the login page.</span></span>
* <span data-ttu-id="1520a-129">Récupérez l’état précédent après l’authentification à l’aide du paramètre de chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="1520a-129">Recover the previous state afterward authentication using the query string parameter.</span></span>

```razor
<EditForm Model="User" @onsubmit="OnSaveAsync">
    <label>User
        <InputText @bind-Value="User.Name" />
    </label>
    <label>Last name
        <InputText @bind-Value="User.LastName" />
    </label>
</EditForm>

@code {
    public class Profile
    {
        public string Name { get; set; }
        public string LastName { get; set; }
    }

    public Profile User { get; set; } = new Profile();

    protected async override Task OnInitializedAsync()
    {
        var currentQuery = new Uri(Navigation.Uri).Query;

        if (currentQuery.Contains("state=resumeSavingProfile"))
        {
            User = await JS.InvokeAsync<Profile>("sessionStorage.getState", 
                "resumeSavingProfile");
        }
    }

    public async Task OnSaveAsync()
    {
        var httpClient = new HttpClient();
        httpClient.BaseAddress = new Uri(Navigation.BaseUri);

        var resumeUri = Navigation.Uri + $"?state=resumeSavingProfile";

        var tokenResult = await AuthenticationService.RequestAccessToken(
            new AccessTokenRequestOptions
            {
                ReturnUrl = resumeUri
            });

        if (tokenResult.TryGetToken(out var token))
        {
            httpClient.DefaultRequestHeaders.Add("Authorization", 
                $"Bearer {token.Value}");
            await httpClient.PostJsonAsync("Save", User);
        }
        else
        {
            await JS.InvokeVoidAsync("sessionStorage.setState", 
                "resumeSavingProfile", User);
            Navigation.NavigateTo(tokenResult.RedirectUrl);
        }
    }
}
```

## <a name="save-app-state-before-an-authentication-operation"></a><span data-ttu-id="1520a-130">Enregistrer l’état de l’application avant une opération d’authentification</span><span class="sxs-lookup"><span data-stu-id="1520a-130">Save app state before an authentication operation</span></span>

<span data-ttu-id="1520a-131">Au cours d’une opération d’authentification, il existe des cas où vous souhaitez enregistrer l’état de l’application avant que le navigateur soit redirigé vers l’adresse IP.</span><span class="sxs-lookup"><span data-stu-id="1520a-131">During an authentication operation, there are cases where you want to save the app state before the browser is redirected to the IP.</span></span> <span data-ttu-id="1520a-132">Cela peut être le cas lorsque vous utilisez un conteneur d’État et que vous souhaitez restaurer l’État une fois l’authentification réussie.</span><span class="sxs-lookup"><span data-stu-id="1520a-132">This can be the case when you are using something like a state container and you want to restore the state after the authentication succeeds.</span></span> <span data-ttu-id="1520a-133">Vous pouvez utiliser un objet d’état d’authentification personnalisé pour conserver l’état spécifique à l’application ou une référence à celui-ci, et restaurer cet État une fois l’opération d’authentification terminée.</span><span class="sxs-lookup"><span data-stu-id="1520a-133">You can use a custom authentication state object to preserve app-specific state or a reference to it and restore that state once the authentication operation successfully completes.</span></span>

<span data-ttu-id="1520a-134">composant `Authentication` (*pages/Authentication. Razor*) :</span><span class="sxs-lookup"><span data-stu-id="1520a-134">`Authentication` component (*Pages/Authentication.razor*):</span></span>

```razor
@page "/authentication/{action}"
@inject JSRuntime JS
@inject StateContainer State
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorViewCore Action="@Action" 
    AuthenticationState="AuthenticationState" OnLoginSucceded="RestoreState" 
    OnLogoutSucceded="RestoreState" />

@code {
    [Parameter]
    public string Action { get; set; }

    public class ApplicationAuthenticationState : RemoteAuthenticationState
    {
        public string Id { get; set; }
    }

    protected async override Task OnInitializedAsync()
    {
        if (RemoteAuthenticationActions.IsAction(RemoteAuthenticationActions.LogIn, 
            Action))
        {
            AuthenticationState.Id = Guid.NewGuid().ToString();
            await JS.InvokeVoidAsync("sessionStorage.setKey", 
                AuthenticationState.Id, State.Store());
        }
    }

    public async Task RestoreState(ApplicationAuthenticationState state)
    {
        var stored = await JS.InvokeAsync<string>("sessionStorage.getKey", 
            state.Id);
        State.FromStore(stored);
    }

    public ApplicationAuthenticationState AuthenticationState { get; set; } = 
        new ApplicationAuthenticationState();
}
```

## <a name="request-additional-access-tokens"></a><span data-ttu-id="1520a-135">Demander des jetons d’accès supplémentaires</span><span class="sxs-lookup"><span data-stu-id="1520a-135">Request additional access tokens</span></span>

<span data-ttu-id="1520a-136">La plupart des applications ont uniquement besoin d’un jeton d’accès pour interagir avec les ressources protégées qu’elles utilisent.</span><span class="sxs-lookup"><span data-stu-id="1520a-136">Most apps only require an access token to interact with the protected resources that they use.</span></span> <span data-ttu-id="1520a-137">Dans certains scénarios, une application peut nécessiter plusieurs jetons pour interagir avec au moins deux ressources.</span><span class="sxs-lookup"><span data-stu-id="1520a-137">In some scenarios, an app might require more than one token in order to interact with two or more resources.</span></span> <span data-ttu-id="1520a-138">La méthode `IAccessTokenProvider.RequestToken` fournit une surcharge qui permet à une application de configurer un jeton avec un ensemble donné d’étendues, comme illustré dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="1520a-138">The `IAccessTokenProvider.RequestToken` method provides an overload that allows an app to provision a token with a given set of scopes, as seen in the following example:</span></span>

```csharp
var tokenResult = await AuthenticationService.RequestAccessToken(
    new AccessTokenRequestOptions
    {
        Scopes = new[] { "https://graph.microsoft.com/Mail.Send", 
            "https://graph.microsoft.com/User.Read" }
    });
```

## <a name="customize-app-routes"></a><span data-ttu-id="1520a-139">Personnaliser les itinéraires de l’application</span><span class="sxs-lookup"><span data-stu-id="1520a-139">Customize app routes</span></span>

<span data-ttu-id="1520a-140">Par défaut, la bibliothèque de `Microsoft.AspNetCore.Components.WebAssembly.Authentication` utilise les itinéraires indiqués dans le tableau suivant pour représenter des États d’authentification différents.</span><span class="sxs-lookup"><span data-stu-id="1520a-140">By default, the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` library uses the routes shown in the following table for representing different authentication states.</span></span>

| <span data-ttu-id="1520a-141">Routage</span><span class="sxs-lookup"><span data-stu-id="1520a-141">Route</span></span>                            | <span data-ttu-id="1520a-142">Objectif</span><span class="sxs-lookup"><span data-stu-id="1520a-142">Purpose</span></span> |
| -------------------------------- | ------- |
| `authentication/login`           | <span data-ttu-id="1520a-143">Déclenche une opération de connexion.</span><span class="sxs-lookup"><span data-stu-id="1520a-143">Triggers a sign-in operation.</span></span> |
| `authentication/login-callback`  | <span data-ttu-id="1520a-144">Gère le résultat de toute opération de connexion.</span><span class="sxs-lookup"><span data-stu-id="1520a-144">Handles the result of any sign-in operation.</span></span> |
| `authentication/login-failed`    | <span data-ttu-id="1520a-145">Affiche des messages d’erreur lorsque l’opération de connexion échoue pour une raison quelconque.</span><span class="sxs-lookup"><span data-stu-id="1520a-145">Displays error messages when the sign-in operation fails for some reason.</span></span> |
| `authentication/logout`          | <span data-ttu-id="1520a-146">Déclenche une opération de déconnexion.</span><span class="sxs-lookup"><span data-stu-id="1520a-146">Triggers a sign-out operation.</span></span> |
| `authentication/logout-callback` | <span data-ttu-id="1520a-147">Gère le résultat d’une opération de déconnexion.</span><span class="sxs-lookup"><span data-stu-id="1520a-147">Handles the result of a sign-out operation.</span></span> |
| `authentication/logout-failed`   | <span data-ttu-id="1520a-148">Affiche des messages d’erreur lorsque l’opération de déconnexion échoue pour une raison quelconque.</span><span class="sxs-lookup"><span data-stu-id="1520a-148">Displays error messages when the sign-out operation fails for some reason.</span></span> |
| `authentication/logged-out`      | <span data-ttu-id="1520a-149">Indique que l’utilisateur a réussi à se déconnecter.</span><span class="sxs-lookup"><span data-stu-id="1520a-149">Indicates that the user has successfully logout.</span></span> |
| `authentication/profile`         | <span data-ttu-id="1520a-150">Déclenche une opération de modification du profil utilisateur.</span><span class="sxs-lookup"><span data-stu-id="1520a-150">Triggers an operation to edit the user profile.</span></span> |
| `authentication/register`        | <span data-ttu-id="1520a-151">Déclenche une opération pour inscrire un nouvel utilisateur.</span><span class="sxs-lookup"><span data-stu-id="1520a-151">Triggers an operation to register a new user.</span></span> |

<span data-ttu-id="1520a-152">Les itinéraires indiqués dans le tableau précédent sont configurables via `RemoteAuthenticationOptions<TProviderOptions>.AuthenticationPaths`.</span><span class="sxs-lookup"><span data-stu-id="1520a-152">The routes shown in the preceding table are configurable via `RemoteAuthenticationOptions<TProviderOptions>.AuthenticationPaths`.</span></span> <span data-ttu-id="1520a-153">Quand vous définissez des options pour fournir des itinéraires personnalisés, vérifiez que l’application a un itinéraire qui gère chaque chemin d’accès.</span><span class="sxs-lookup"><span data-stu-id="1520a-153">When setting options to provide custom routes, confirm that the app has a route that handles each path.</span></span>

<span data-ttu-id="1520a-154">Dans l’exemple suivant, tous les chemins d’accès ont le préfixe `/security`.</span><span class="sxs-lookup"><span data-stu-id="1520a-154">In the following example, all the paths are prefixed with `/security`.</span></span>

<span data-ttu-id="1520a-155">composant `Authentication` (*pages/Authentication. Razor*) :</span><span class="sxs-lookup"><span data-stu-id="1520a-155">`Authentication` component (*Pages/Authentication.razor*):</span></span>

```razor
@page "/security/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action" />

@code{
    [Parameter]
    public string Action { get; set; }
}
```

<span data-ttu-id="1520a-156">`Program.Main` (*Program.cs*) :</span><span class="sxs-lookup"><span data-stu-id="1520a-156">`Program.Main` (*Program.cs*):</span></span>

```csharp
builder.Services.AddApiAuthorization(options => { 
    options.AuthenticationPaths.LogInPath = "security/login";
    options.AuthenticationPaths.LogInCallbackPath = "security/login-callback";
    options.AuthenticationPaths.LogInFailedPath = "security/login-failed";
    options.AuthenticationPaths.LogOutPath = "security/logout";
    options.AuthenticationPaths.LogOutCallbackPath = "security/logout-callback";
    options.AuthenticationPaths.LogOutFailedPath = "security/logout-failed";
    options.AuthenticationPaths.LogOutSucceededPath = "security/logged-out";
    options.AuthenticationPaths.ProfilePath = "security/profile";
    options.AuthenticationPaths.RegisterPath = "security/register";
});
```

<span data-ttu-id="1520a-157">Si la spécification exige des chemins d’accès complètement différents, définissez les itinéraires comme décrit précédemment et affichez le `RemoteAuthenticatorView` avec un paramètre d’action explicite :</span><span class="sxs-lookup"><span data-stu-id="1520a-157">If the requirement calls for completely different paths, set the routes as described previously and render the `RemoteAuthenticatorView` with an explicit action parameter:</span></span>

```razor
@page "/register"

<RemoteAuthenticatorView Action="@RemoteAuthenticationActions.Register" />
```

<span data-ttu-id="1520a-158">Si vous le souhaitez, vous avez la possibilité de scinder l’interface utilisateur en différentes pages.</span><span class="sxs-lookup"><span data-stu-id="1520a-158">You're allowed to break the UI into different pages if you choose to do so.</span></span>

## <a name="customize-the-authentication-user-interface"></a><span data-ttu-id="1520a-159">Personnaliser l’interface utilisateur de l’authentification</span><span class="sxs-lookup"><span data-stu-id="1520a-159">Customize the authentication user interface</span></span>

<span data-ttu-id="1520a-160">`RemoteAuthenticatorView` comprend un ensemble par défaut d’éléments d’interface utilisateur pour chaque État d’authentification.</span><span class="sxs-lookup"><span data-stu-id="1520a-160">`RemoteAuthenticatorView` includes a default set of UI pieces for each authentication state.</span></span> <span data-ttu-id="1520a-161">Chaque État peut être personnalisé en passant une `RenderFragment`personnalisée.</span><span class="sxs-lookup"><span data-stu-id="1520a-161">Each state can be customized by passing in a custom `RenderFragment`.</span></span> <span data-ttu-id="1520a-162">Pour personnaliser le texte affiché pendant le processus de connexion initial, vous pouvez modifier les `RemoteAuthenticatorView` comme suit.</span><span class="sxs-lookup"><span data-stu-id="1520a-162">To customize the displayed text during the initial login process, can change the `RemoteAuthenticatorView` as follows.</span></span>

<span data-ttu-id="1520a-163">composant `Authentication` (*pages/Authentication. Razor*) :</span><span class="sxs-lookup"><span data-stu-id="1520a-163">`Authentication` component (*Pages/Authentication.razor*):</span></span>

```razor
@page "/security/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action">
    <LoggingIn>
        You are about to be redirected to https://login.microsoftonline.com.
    </LoggingIn>
</RemoteAuthenticatorView>

@code{
    [Parameter]
    public string Action { get; set; }
}
```

<span data-ttu-id="1520a-164">Le `RemoteAuthenticatorView` possède un fragment qui peut être utilisé par itinéraire d’authentification, comme indiqué dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="1520a-164">The `RemoteAuthenticatorView` has one fragment that can be used per authentication route shown in the following table.</span></span>

| <span data-ttu-id="1520a-165">Routage</span><span class="sxs-lookup"><span data-stu-id="1520a-165">Route</span></span>                            | <span data-ttu-id="1520a-166">Fragment</span><span class="sxs-lookup"><span data-stu-id="1520a-166">Fragment</span></span>                |
| -------------------------------- | ----------------------- |
| `authentication/login`           | `<LoggingIn>`           |
| `authentication/login-callback`  | `<CompletingLoggingIn>` |
| `authentication/login-failed`    | `<LogInFailed>`         |
| `authentication/logout`          | `<LogOut>`              |
| `authentication/logout-callback` | `<CompletingLogOut>`    |
| `authentication/logout-failed`   | `<LogOutFailed>`        |
| `authentication/logged-out`      | `<LogOutSucceeded>`     |
| `authentication/profile`         | `<UserProfile>`         |
| `authentication/register`        | `<Registering>`         |
