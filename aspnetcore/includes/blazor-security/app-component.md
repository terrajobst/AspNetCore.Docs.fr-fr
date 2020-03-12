Le composant `App` (*app. Razor*) est semblable au composant `App` des applications serveur éblouissantes :

* Le composant `CascadingAuthenticationState` gère l’exposition du `AuthenticationState` au reste de l’application.
* Le composant `AuthorizeRouteView` permet de s’assurer que l’utilisateur actuel est autorisé à accéder à une page donnée, ou à restituer le composant `RedirectToLogin`.
* Le composant `RedirectToLogin` gère la redirection des utilisateurs non autorisés vers la page de connexion.

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
