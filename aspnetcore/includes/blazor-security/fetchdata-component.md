<span data-ttu-id="bb474-101">Le composant `FetchData` montre comment :</span><span class="sxs-lookup"><span data-stu-id="bb474-101">The `FetchData` component shows how to:</span></span>

* <span data-ttu-id="bb474-102">Approvisionner un jeton d’accès.</span><span class="sxs-lookup"><span data-stu-id="bb474-102">Provision an access token.</span></span>
* <span data-ttu-id="bb474-103">Utilisez le jeton d’accès pour appeler une API de ressource protégée dans l’application *serveur* .</span><span class="sxs-lookup"><span data-stu-id="bb474-103">Use the access token to call a protected resource API in the *Server* app.</span></span>

<span data-ttu-id="bb474-104">La directive `@attribute [Authorize]` indique au système d’autorisation de webassembly éblouissant que l’utilisateur doit être autorisé à accéder à ce composant.</span><span class="sxs-lookup"><span data-stu-id="bb474-104">The `@attribute [Authorize]` directive indicates to the Blazor WebAssembly authorization system that the user must be authorized in order to visit this component.</span></span> <span data-ttu-id="bb474-105">La présence de l’attribut dans l’application *cliente* n’empêche pas l’appel de l’API sur le serveur sans les informations d’identification appropriées.</span><span class="sxs-lookup"><span data-stu-id="bb474-105">The presence of the attribute in the *Client* app doesn't prevent the API on the server from being called without proper credentials.</span></span> <span data-ttu-id="bb474-106">L’application *serveur* doit également utiliser `[Authorize]` sur les points de terminaison appropriés pour les protéger correctement.</span><span class="sxs-lookup"><span data-stu-id="bb474-106">The *Server* app also must use `[Authorize]` on the appropriate endpoints to correctly protect them.</span></span>

<span data-ttu-id="bb474-107">`AuthenticationService.RequestAccessToken();` s’occupe de demander un jeton d’accès qui peut être ajouté à la demande d’appel de l’API.</span><span class="sxs-lookup"><span data-stu-id="bb474-107">`AuthenticationService.RequestAccessToken();` takes care of requesting an access token that can be added to the request to call the API.</span></span> <span data-ttu-id="bb474-108">Si le jeton est mis en cache ou si le service est en mesure d’approvisionner un nouveau jeton d’accès sans intervention de l’utilisateur, la demande de jeton réussit.</span><span class="sxs-lookup"><span data-stu-id="bb474-108">If the token is cached or the service is able to provision a new access token without user interaction, the token request succeeds.</span></span> <span data-ttu-id="bb474-109">Dans le cas contraire, la demande de jeton échoue.</span><span class="sxs-lookup"><span data-stu-id="bb474-109">Otherwise, the token request fails.</span></span>

<span data-ttu-id="bb474-110">Afin d’obtenir le jeton à inclure dans la demande, l’application doit vérifier que la demande a réussi en appelant `tokenResult.TryGetToken(out var token)`.</span><span class="sxs-lookup"><span data-stu-id="bb474-110">In order to obtain the actual token to include in the request, the app must check that the request succeeded by calling `tokenResult.TryGetToken(out var token)`.</span></span> 

<span data-ttu-id="bb474-111">Si la demande a réussi, la variable de jeton est remplie avec le jeton d’accès.</span><span class="sxs-lookup"><span data-stu-id="bb474-111">If the request was successful, the token variable is populated with the access token.</span></span> <span data-ttu-id="bb474-112">La propriété `Value` du jeton expose la chaîne littérale à inclure dans l’en-tête de demande `Authorization`.</span><span class="sxs-lookup"><span data-stu-id="bb474-112">The `Value` property of the token exposes the literal string to include in the `Authorization` request header.</span></span>

<span data-ttu-id="bb474-113">Si la requête a échoué parce que le jeton n’a pas pu être approvisionné sans intervention de l’utilisateur, le résultat du jeton contient une URL de redirection.</span><span class="sxs-lookup"><span data-stu-id="bb474-113">If the request failed because the token couldn't be provisioned without user interaction, the token result contains a redirect URL.</span></span> <span data-ttu-id="bb474-114">Si vous accédez à cette URL, l’utilisateur accède à la page de connexion et revient à la page active après une authentification réussie.</span><span class="sxs-lookup"><span data-stu-id="bb474-114">Navigating to this URL takes the user to the login page and back to the current page after a successful authentication.</span></span>

```razor
@page "/fetchdata"
...
@attribute [Authorize]

...

@code {
    private WeatherForecast[] forecasts;

    protected override async Task OnInitializedAsync()
    {
        var httpClient = new HttpClient();
        httpClient.BaseAddress = new Uri(Navigation.BaseUri);

        var tokenResult = await AuthenticationService.RequestAccessToken();

        if (tokenResult.TryGetToken(out var token))
        {
            httpClient.DefaultRequestHeaders.Add("Authorization", 
                $"Bearer {token.Value}");
            forecasts = await httpClient.GetJsonAsync<WeatherForecast[]>(
                "WeatherForecast");
        }
        else
        {
            Navigation.NavigateTo(tokenResult.RedirectUrl);
        }

    }
}
```

<span data-ttu-id="bb474-115">Pour plus d’informations, consultez [enregistrer l’état de l’application avant une opération d’authentification](xref:security/blazor/webassembly/additional-scenarios#save-app-state-before-an-authentication-operation).</span><span class="sxs-lookup"><span data-stu-id="bb474-115">For more information, see [Save app state before an authentication operation](xref:security/blazor/webassembly/additional-scenarios#save-app-state-before-an-authentication-operation).</span></span>
