La page générée par le composant `Authentication` (*pages/Authentication. Razor*) définit les itinéraires requis pour la gestion des différentes étapes d’authentification.

Le composant `RemoteAuthenticatorView` :

* Est fourni par le package de `Microsoft.AspNetCore.Components.WebAssembly.Authentication`.
* Gère l’exécution des actions appropriées à chaque étape de l’authentification.

```razor
@page "/authentication/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action" />

@code {
    [Parameter]
    public string Action { get; set; }
}
```
