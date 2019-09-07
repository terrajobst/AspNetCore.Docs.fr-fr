> [!WARNING]
> Pour des raisons de sécurité, vous devez choisir de lier les données de requête `GET` aux propriétés du modèle de page. Vérifiez l’entrée utilisateur avant de la mapper à des propriétés. L’utilisation de la liaisonestutilelorsdel’adressagedescénariosquireposentsurunechaînederequêteoudesvaleursderoute.`GET`
>
> Pour lier une propriété à `GET` des demandes, affectez à `true`la propriété de `SupportsGet` l’attribut [[BindProperty]](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) la valeur :
>
> ```csharp
> [BindProperty(SupportsGet = true)]
> ```
>
> Pour plus d’informations, [consultez ASP.net Core Community réunions : Liez sur obtenir une discussion (YouTube](https://www.youtube.com/watch?v=p7iHB9V-KVU&feature=youtu.be&t=54m27s)).
