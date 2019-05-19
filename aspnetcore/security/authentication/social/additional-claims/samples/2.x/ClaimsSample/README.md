# <a name="additional-claims-sample-app"></a>Exemple d’application des revendications supplémentaires

L’exemple d’application montre comment :

* Obtenir le nom et prénom de l’utilisateur à partir de Google et stocker les revendications de nom avec les valeurs fournies par Google.
* Store le jeton d’accès Google de l’utilisateur `AuthenticationProperties`.

Pour utiliser l’exemple d’application :

1. Inscrire l’application et obtenir un ID client valide et une clé secrète client pour l’authentification Google. Pour plus d’informations, consultez [le programme d’installation de Google connexion externe](https://docs.microsoft.com/aspnet/core/security/authentication/social/google-logins).
1. Fournir l’ID client et le secret du client à l’application dans le [GoogleOptions](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) de `Startup.ConfigureServices`.
1. Exécutez l’application et demandez la page Mes revendications. Lorsque l’utilisateur n’est pas connecté, l’application redirige vers Google. Se connecter avec Google. Google redirige l’utilisateur vers l’application (`/MyClaims`). L’utilisateur est authentifié, et la page Mes revendications est chargée. Le nom donné et les revendications de nom de famille sont présentes sous **revendications utilisateur** avec les valeurs fournies par Google. Le jeton d’accès est affiché sous **propriétés d’authentification**.
