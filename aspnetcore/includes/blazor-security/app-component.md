<span data-ttu-id="85db3-101">Le composant `App` (*app. Razor*) est semblable au composant `App` des applications serveur éblouissantes :</span><span class="sxs-lookup"><span data-stu-id="85db3-101">The `App` component (*App.razor*) is similar to the `App` component found in Blazor Server apps:</span></span>

* <span data-ttu-id="85db3-102">Le composant `CascadingAuthenticationState` gère l’exposition du `AuthenticationState` au reste de l’application.</span><span class="sxs-lookup"><span data-stu-id="85db3-102">The `CascadingAuthenticationState` component manages exposing the `AuthenticationState` to the rest of the app.</span></span>
* <span data-ttu-id="85db3-103">Le composant `AuthorizeRouteView` permet de s’assurer que l’utilisateur actuel est autorisé à accéder à une page donnée, ou à restituer le composant `RedirectToLogin`.</span><span class="sxs-lookup"><span data-stu-id="85db3-103">The `AuthorizeRouteView` component makes sure that the current user is authorized to access a given page or otherwise renders the `RedirectToLogin` component.</span></span>
* <span data-ttu-id="85db3-104">Le composant `RedirectToLogin` gère la redirection des utilisateurs non autorisés vers la page de connexion.</span><span class="sxs-lookup"><span data-stu-id="85db3-104">The `RedirectToLogin` component manages redirecting unauthorized users to the login page.</span></span>

```razor
<CascadingAuthenticationState>
    <Router AppAssembly="@typeof(Program).Assembly">
        <Found Context="routeData">
            <AuthorizeRouteView RouteData="@routeData" 
                DefaultLayout="@typeof(MainLayout)">
                <NotAuthorized>
                    <RedirectToLogin />
                </NotAuthorized>
            </AuthorizeRouteView>
        </Found>
        <NotFound>
            <LayoutView Layout="@typeof(MainLayout)">
                <p>Sorry, there's nothing at this address.</p>
            </LayoutView>
        </NotFound>
    </Router>
</CascadingAuthenticationState>
```
