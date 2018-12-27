> [!WARNING]
> Pour des raisons de sécurité, vous devez choisir de lier les données de requête `GET` aux propriétés du modèle de page. Vérifiez l’entrée utilisateur avant de la mapper à des propriétés. Le choix de la liaison `GET` est utile pour les scénarios qui reposent sur des valeurs de routage ou de chaîne de requête.
>
> Pour lier une propriété sur des requêtes `GET`, affectez à la propriété `SupportsGet` de l’attribut [[BindProperty]](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) la valeur `true` : `[BindProperty(SupportsGet = true)]`
