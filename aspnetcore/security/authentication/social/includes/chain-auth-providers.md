## <a name="multiple-authentication-providers"></a>Plusieurs fournisseurs d’authentification

Lorsque l’application nécessite plusieurs fournisseurs, chaînez les méthodes d’extension de fournisseur derrière [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication) :

```csharp
services.AddAuthentication()
    .AddMicrosoftAccount(microsoftOptions => { ... })
    .AddGoogle(googleOptions => { ... })
    .AddTwitter(twitterOptions => { ... })
    .AddFacebook(facebookOptions => { ... });
```
