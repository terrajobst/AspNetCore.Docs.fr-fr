> [!WARNING]
> <span data-ttu-id="550be-101">Pour des raisons de sécurité, vous devez choisir de lier les données de requête `GET` aux propriétés du modèle de page.</span><span class="sxs-lookup"><span data-stu-id="550be-101">For security reasons, you must opt in to binding `GET` request data to page model properties.</span></span> <span data-ttu-id="550be-102">Vérifiez l’entrée utilisateur avant de la mapper à des propriétés.</span><span class="sxs-lookup"><span data-stu-id="550be-102">Verify user input before mapping it to properties.</span></span> <span data-ttu-id="550be-103">L’utilisation de la liaisonestutilelorsdel’adressagedescénariosquireposentsurunechaînederequêteoudesvaleursderoute.`GET`</span><span class="sxs-lookup"><span data-stu-id="550be-103">Opting into `GET` binding is useful when addressing scenarios that rely on query string or route values.</span></span>
>
> <span data-ttu-id="550be-104">Pour lier une propriété à `GET` des demandes, affectez à `true`la propriété de `SupportsGet` l’attribut [[BindProperty]](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) la valeur :</span><span class="sxs-lookup"><span data-stu-id="550be-104">To bind a property on `GET` requests, set the [[BindProperty]](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) attribute's `SupportsGet` property to `true`:</span></span>
>
> ```csharp
> [BindProperty(SupportsGet = true)]
> ```
>
> <span data-ttu-id="550be-105">Pour plus d’informations, [consultez ASP.net Core Community réunions : Liez sur obtenir une discussion (YouTube](https://www.youtube.com/watch?v=p7iHB9V-KVU&feature=youtu.be&t=54m27s)).</span><span class="sxs-lookup"><span data-stu-id="550be-105">For more information, see [ASP.NET Core Community Standup: Bind on GET discussion (YouTube)](https://www.youtube.com/watch?v=p7iHB9V-KVU&feature=youtu.be&t=54m27s).</span></span>
