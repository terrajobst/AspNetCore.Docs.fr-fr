---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: Prise en charge des Options de requête OData dans ASP.NET Web API 2 | Documents Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/04/2013
ms.topic: article
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 004c029db6f01627f7cadff26aaf5554ce2b93a5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508018"
---
<a name="supporting-odata-query-options-in-aspnet-web-api-2"></a><span data-ttu-id="90ec7-102">Prise en charge des Options de requête OData dans ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="90ec7-102">Supporting OData Query Options in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="90ec7-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="90ec7-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="90ec7-104">OData définit les paramètres qui peuvent être utilisés pour modifier une requête OData.</span><span class="sxs-lookup"><span data-stu-id="90ec7-104">OData defines parameters that can be used to modify an OData query.</span></span> <span data-ttu-id="90ec7-105">Le client envoie ces paramètres dans la chaîne de requête de l’URI de requête.</span><span class="sxs-lookup"><span data-stu-id="90ec7-105">The client sends these parameters in the query string of the request URI.</span></span> <span data-ttu-id="90ec7-106">Par exemple, pour trier les résultats, un client utilise le paramètre $orderby :</span><span class="sxs-lookup"><span data-stu-id="90ec7-106">For example, to sort the results, a client uses the $orderby parameter:</span></span>

`http://localhost/Products?$orderby=Name`

<span data-ttu-id="90ec7-107">La spécification OData appelle ces paramètres *options de requête*.</span><span class="sxs-lookup"><span data-stu-id="90ec7-107">The OData specification calls these parameters *query options*.</span></span> <span data-ttu-id="90ec7-108">Vous pouvez activer des options de requête OData pour n’importe quel contrôleur d’API Web dans votre projet & #8212 ; le contrôleur n’a pas besoin d’un point de terminaison OData.</span><span class="sxs-lookup"><span data-stu-id="90ec7-108">You can enable OData query options for any Web API controller in your project &#8212; the controller does not need to be an OData endpoint.</span></span> <span data-ttu-id="90ec7-109">Cela vous donne un moyen pratique pour ajouter des fonctionnalités telles que le filtrage et tri à n’importe quelle application de l’API Web.</span><span class="sxs-lookup"><span data-stu-id="90ec7-109">This gives you a convenient way to add features such as filtering and sorting to any Web API application.</span></span>

<span data-ttu-id="90ec7-110">Avant d’activer les options de requête, consultez la rubrique [Guide de sécurité OData](odata-security-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="90ec7-110">Before enabling query options, please read the topic [OData Security Guidance](odata-security-guidance.md).</span></span>

- [<span data-ttu-id="90ec7-111">Activation des Options de requête OData</span><span class="sxs-lookup"><span data-stu-id="90ec7-111">Enabling OData Query Options</span></span>](#enable)
- [<span data-ttu-id="90ec7-112">Exemples de requêtes</span><span class="sxs-lookup"><span data-stu-id="90ec7-112">Example Queries</span></span>](#examples)
- [<span data-ttu-id="90ec7-113">Pagination orientée serveur</span><span class="sxs-lookup"><span data-stu-id="90ec7-113">Server-Driven Paging</span></span>](#server-paging)
- [<span data-ttu-id="90ec7-114">Limiter les Options de requête</span><span class="sxs-lookup"><span data-stu-id="90ec7-114">Limiting the Query Options</span></span>](#limiting_query_options)
- [<span data-ttu-id="90ec7-115">Appeler directement les Options de requête</span><span class="sxs-lookup"><span data-stu-id="90ec7-115">Invoking Query Options Directly</span></span>](#ODataQueryOptions)
- [<span data-ttu-id="90ec7-116">Validation de requête</span><span class="sxs-lookup"><span data-stu-id="90ec7-116">Query Validation</span></span>](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a><span data-ttu-id="90ec7-117">Activation des Options de requête OData</span><span class="sxs-lookup"><span data-stu-id="90ec7-117">Enabling OData Query Options</span></span>

<span data-ttu-id="90ec7-118">API Web prend en charge les options de requête OData suivantes :</span><span class="sxs-lookup"><span data-stu-id="90ec7-118">Web API supports the following OData query options:</span></span>

| <span data-ttu-id="90ec7-119">Option</span><span class="sxs-lookup"><span data-stu-id="90ec7-119">Option</span></span> | <span data-ttu-id="90ec7-120">Description</span><span class="sxs-lookup"><span data-stu-id="90ec7-120">Description</span></span> |
| --- | --- |
| <span data-ttu-id="90ec7-121">$expand</span><span class="sxs-lookup"><span data-stu-id="90ec7-121">$expand</span></span> | <span data-ttu-id="90ec7-122">Développe les entités connexes inline.</span><span class="sxs-lookup"><span data-stu-id="90ec7-122">Expands related entities inline.</span></span> |
| <span data-ttu-id="90ec7-123">$filter</span><span class="sxs-lookup"><span data-stu-id="90ec7-123">$filter</span></span> | <span data-ttu-id="90ec7-124">Filtre les résultats selon une condition booléenne.</span><span class="sxs-lookup"><span data-stu-id="90ec7-124">Filters the results, based on a Boolean condition.</span></span> |
| <span data-ttu-id="90ec7-125">$inlinecount</span><span class="sxs-lookup"><span data-stu-id="90ec7-125">$inlinecount</span></span> | <span data-ttu-id="90ec7-126">Indique au serveur pour inclure le nombre total d’entités correspondantes dans la réponse.</span><span class="sxs-lookup"><span data-stu-id="90ec7-126">Tells the server to include the total count of matching entities in the response.</span></span> <span data-ttu-id="90ec7-127">(Utile pour la pagination côté serveur).</span><span class="sxs-lookup"><span data-stu-id="90ec7-127">(Useful for server-side paging.)</span></span> |
| <span data-ttu-id="90ec7-128">$orderby</span><span class="sxs-lookup"><span data-stu-id="90ec7-128">$orderby</span></span> | <span data-ttu-id="90ec7-129">Trie les résultats.</span><span class="sxs-lookup"><span data-stu-id="90ec7-129">Sorts the results.</span></span> |
| <span data-ttu-id="90ec7-130">$select</span><span class="sxs-lookup"><span data-stu-id="90ec7-130">$select</span></span> | <span data-ttu-id="90ec7-131">Sélectionne les propriétés à inclure dans la réponse.</span><span class="sxs-lookup"><span data-stu-id="90ec7-131">Selects which properties to include in the response.</span></span> |
| <span data-ttu-id="90ec7-132">$skip</span><span class="sxs-lookup"><span data-stu-id="90ec7-132">$skip</span></span> | <span data-ttu-id="90ec7-133">Ignore les n premiers résultats.</span><span class="sxs-lookup"><span data-stu-id="90ec7-133">Skips the first n results.</span></span> |
| <span data-ttu-id="90ec7-134">$top</span><span class="sxs-lookup"><span data-stu-id="90ec7-134">$top</span></span> | <span data-ttu-id="90ec7-135">Retourne uniquement la première n les résultats.</span><span class="sxs-lookup"><span data-stu-id="90ec7-135">Returns only the first n the results.</span></span> |

<span data-ttu-id="90ec7-136">Pour utiliser les options de requête OData, vous devez les activer explicitement.</span><span class="sxs-lookup"><span data-stu-id="90ec7-136">To use OData query options, you must enable them explicitly.</span></span> <span data-ttu-id="90ec7-137">Vous pouvez les activer globalement pour l’application entière, ou les activer pour les contrôleurs spécifiques ou des actions spécifiques.</span><span class="sxs-lookup"><span data-stu-id="90ec7-137">You can enable them globally for the entire application, or enable them for specific controllers or specific actions.</span></span>

<span data-ttu-id="90ec7-138">Pour activer les options de requête OData globalement, appelez **EnableQuerySupport** sur la **HttpConfiguration** classe au démarrage :</span><span class="sxs-lookup"><span data-stu-id="90ec7-138">To enable OData query options globally, call **EnableQuerySupport** on the **HttpConfiguration** class at startup:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

<span data-ttu-id="90ec7-139">Le **EnableQuerySupport** méthode active les options de requête globalement pour toute action de contrôleur qui retourne un **IQueryable** type.</span><span class="sxs-lookup"><span data-stu-id="90ec7-139">The **EnableQuerySupport** method enables query options globally for any controller action that returns an **IQueryable** type.</span></span> <span data-ttu-id="90ec7-140">Si vous ne souhaitez pas les options de requête activées pour l’application entière, vous pouvez les activer pour les actions de contrôleur spécifique en ajoutant le **[Queryable]** d’attribut à la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="90ec7-140">If you don't want query options enabled for the entire application, you can enable them for specific controller actions by adding the **[Queryable]** attribute to the action method.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a><span data-ttu-id="90ec7-141">Exemples de requêtes</span><span class="sxs-lookup"><span data-stu-id="90ec7-141">Example Queries</span></span>

<span data-ttu-id="90ec7-142">Cette section présente les types de requêtes sont possibles à l’aide des options de requête OData.</span><span class="sxs-lookup"><span data-stu-id="90ec7-142">This section shows the types of queries that are possible using the OData query options.</span></span> <span data-ttu-id="90ec7-143">Pour obtenir des détails spécifiques sur les options de requête, consultez la documentation OData à [www.odata.org](http://www.odata.org/).</span><span class="sxs-lookup"><span data-stu-id="90ec7-143">For specific details about the query options, refer to the OData documentation at [www.odata.org](http://www.odata.org/).</span></span>

<span data-ttu-id="90ec7-144">Pour plus d’informations à propos de $développez et $select, consultez [à l’aide de $select, $expand et $value dans ASP.NET Web API OData](using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="90ec7-144">For information about $expand and $select, see [Using $select, $expand, and $value in ASP.NET Web API OData](using-select-expand-and-value.md).</span></span>

<span data-ttu-id="90ec7-145">**Pagination orientée sur le client**</span><span class="sxs-lookup"><span data-stu-id="90ec7-145">**Client-Driven Paging**</span></span>

<span data-ttu-id="90ec7-146">Pour les jeux d’entités volumineux, le client souhaite peut limiter le nombre de résultats.</span><span class="sxs-lookup"><span data-stu-id="90ec7-146">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="90ec7-147">Par exemple, un client peut afficher des entrées de 10 à la fois, avec des liens « suivant » pour obtenir la page suivante de résultats.</span><span class="sxs-lookup"><span data-stu-id="90ec7-147">For example, a client might show 10 entries at a time, with "next" links to get the next page of results.</span></span> <span data-ttu-id="90ec7-148">Pour ce faire, le client utilise les options $top et $skip.</span><span class="sxs-lookup"><span data-stu-id="90ec7-148">To do this, the client uses the $top and $skip options.</span></span>

`http://localhost/Products?$top=10&$skip=20`

<span data-ttu-id="90ec7-149">L’option $top indique le nombre maximal d’entrées à retourner, et l’option $skip donne le nombre d’entrées à ignorer.</span><span class="sxs-lookup"><span data-stu-id="90ec7-149">The $top option gives the maximum number of entries to return, and the $skip option gives the number of entries to skip.</span></span> <span data-ttu-id="90ec7-150">L’exemple précédent récupère les entrées 21 à 30.</span><span class="sxs-lookup"><span data-stu-id="90ec7-150">The previous example fetches entries 21 through 30.</span></span>

<span data-ttu-id="90ec7-151">**Le filtrage**</span><span class="sxs-lookup"><span data-stu-id="90ec7-151">**Filtering**</span></span>

<span data-ttu-id="90ec7-152">L’option $filter permet à un client de filtrer les résultats en appliquant une expression booléenne.</span><span class="sxs-lookup"><span data-stu-id="90ec7-152">The $filter option lets a client filter the results by applying a Boolean expression.</span></span> <span data-ttu-id="90ec7-153">Les expressions de filtre sont très puissantes ; ils comprennent les opérateurs arithmétiques et logiques, les fonctions de chaîne et les fonctions de date.</span><span class="sxs-lookup"><span data-stu-id="90ec7-153">The filter expressions are quite powerful; they include logical and arithmetic operators, string functions, and date functions.</span></span>

| <span data-ttu-id="90ec7-154">Retourner tous les produits à catégorie égal à « Toys ».</span><span class="sxs-lookup"><span data-stu-id="90ec7-154">Return all products with category equal to "Toys".</span></span> | <span data-ttu-id="90ec7-155">`http://localhost/Products?$filter=Category`EQ « Toys »</span><span class="sxs-lookup"><span data-stu-id="90ec7-155">`http://localhost/Products?$filter=Category` eq 'Toys'</span></span> |
| --- | --- |
| <span data-ttu-id="90ec7-156">Retourner tous les produits dont le prix est inférieur à 10.</span><span class="sxs-lookup"><span data-stu-id="90ec7-156">Return all products with price less than 10.</span></span> | <span data-ttu-id="90ec7-157">`http://localhost/Products?$filter=Price`lt 10</span><span class="sxs-lookup"><span data-stu-id="90ec7-157">`http://localhost/Products?$filter=Price` lt 10</span></span> |
| <span data-ttu-id="90ec7-158">Opérateurs logiques : renvoyer tous les produits de prix où > = 5 et prix < = 15.</span><span class="sxs-lookup"><span data-stu-id="90ec7-158">Logical operators: Return all products where price >= 5 and price <= 15.</span></span> | <span data-ttu-id="90ec7-159">`http://localhost/Products?$filter=Price`GE 5 et le de prix 15</span><span class="sxs-lookup"><span data-stu-id="90ec7-159">`http://localhost/Products?$filter=Price` ge 5 and Price le 15</span></span> |
| <span data-ttu-id="90ec7-160">Fonctions de chaîne : retourner tous les produits avec « zz » dans le nom.</span><span class="sxs-lookup"><span data-stu-id="90ec7-160">String functions: Return all products with "zz" in the name.</span></span> | `http://localhost/Products?$filter=substringof('zz',Name)` |
| <span data-ttu-id="90ec7-161">Fonctions de date : retourner tous les produits avec ReleaseDate après 2005.</span><span class="sxs-lookup"><span data-stu-id="90ec7-161">Date functions: Return all products with ReleaseDate after 2005.</span></span> | <span data-ttu-id="90ec7-162">`http://localhost/Products?$filter=year(ReleaseDate)`gt 2005</span><span class="sxs-lookup"><span data-stu-id="90ec7-162">`http://localhost/Products?$filter=year(ReleaseDate)` gt 2005</span></span> |

<span data-ttu-id="90ec7-163">**Le tri**</span><span class="sxs-lookup"><span data-stu-id="90ec7-163">**Sorting**</span></span>

<span data-ttu-id="90ec7-164">Pour trier les résultats, utilisez le filtre $orderby.</span><span class="sxs-lookup"><span data-stu-id="90ec7-164">To sort the results, use the $orderby filter.</span></span>

| <span data-ttu-id="90ec7-165">Trier par prix.</span><span class="sxs-lookup"><span data-stu-id="90ec7-165">Sort by price.</span></span> | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| <span data-ttu-id="90ec7-166">Trier par prix décroissant (ordre décroissant).</span><span class="sxs-lookup"><span data-stu-id="90ec7-166">Sort by price in descending order (highest to lowest).</span></span> | `http://localhost/Products?$orderby=Price desc` |
| <span data-ttu-id="90ec7-167">Trier par catégorie, puis trier par ordre décroissant dans les catégories de prix.</span><span class="sxs-lookup"><span data-stu-id="90ec7-167">Sort by category, then sort by price in descending order within categories.</span></span> | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a><span data-ttu-id="90ec7-168">Pagination orientée serveur</span><span class="sxs-lookup"><span data-stu-id="90ec7-168">Server-Driven Paging</span></span>

<span data-ttu-id="90ec7-169">Si votre base de données contient des millions d’enregistrements, vous ne souhaitez pas les envoyer dans une charge utile.</span><span class="sxs-lookup"><span data-stu-id="90ec7-169">If your database contains millions of records, you don't want to send them all in one payload.</span></span> <span data-ttu-id="90ec7-170">Pour éviter ce problème, le serveur peut limiter le nombre d’entrées qu’il envoie une réponse unique.</span><span class="sxs-lookup"><span data-stu-id="90ec7-170">To prevent this, the server can limit the number of entries that it sends in a single response.</span></span> <span data-ttu-id="90ec7-171">Pour activer la pagination de serveur, définissez la **PageSize** propriété dans le **Queryable** attribut.</span><span class="sxs-lookup"><span data-stu-id="90ec7-171">To enable server paging, set the **PageSize** property in the **Queryable** attribute.</span></span> <span data-ttu-id="90ec7-172">La valeur est le nombre maximal d’entrées à retourner.</span><span class="sxs-lookup"><span data-stu-id="90ec7-172">The value is the maximum number of entries to return.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

<span data-ttu-id="90ec7-173">Si votre contrôleur retourne le format OData, le corps de réponse contient un lien vers la page de données suivante :</span><span class="sxs-lookup"><span data-stu-id="90ec7-173">If your controller returns OData format, the response body will contain a link to the next page of data:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

<span data-ttu-id="90ec7-174">Le client peut utiliser ce lien pour récupérer la page suivante.</span><span class="sxs-lookup"><span data-stu-id="90ec7-174">The client can use this link to fetch the next page.</span></span> <span data-ttu-id="90ec7-175">Pour connaître le nombre total d’entrées dans le jeu de résultats, le client peut définir l’option de requête $inlinecount avec la valeur « allpages ».</span><span class="sxs-lookup"><span data-stu-id="90ec7-175">To learn the total number of entries in the result set, the client can set the $inlinecount query option with the value "allpages".</span></span>

`http://localhost/Products?$inlinecount=allpages`

<span data-ttu-id="90ec7-176">La valeur « allpages » indique au serveur pour inclure le nombre total de la réponse :</span><span class="sxs-lookup"><span data-stu-id="90ec7-176">The value "allpages" tells the server to include the total count in the response:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> <span data-ttu-id="90ec7-177">Liens de la page suivante et le nombre inline requièrent le format OData.</span><span class="sxs-lookup"><span data-stu-id="90ec7-177">Next-page links and inline count both require OData format.</span></span> <span data-ttu-id="90ec7-178">La raison est que OData définit des champs spéciaux dans le corps de réponse pour conserver le lien et le nombre.</span><span class="sxs-lookup"><span data-stu-id="90ec7-178">The reason is that OData defines special fields in the response body to hold the link and count.</span></span>


<span data-ttu-id="90ec7-179">Pour des formats non-OData, il est toujours possible prendre en charge le nombre de liens et inline de page suivante, en encapsulant les résultats de requête dans un **PageResult&lt;T&gt;**  objet.</span><span class="sxs-lookup"><span data-stu-id="90ec7-179">For non-OData formats, it is still possible to support next-page links and inline count, by wrapping the query results in a **PageResult&lt;T&gt;** object.</span></span> <span data-ttu-id="90ec7-180">Toutefois, il requiert un peu plus de code.</span><span class="sxs-lookup"><span data-stu-id="90ec7-180">However, it requires a bit more code.</span></span> <span data-ttu-id="90ec7-181">Voici un exemple :</span><span class="sxs-lookup"><span data-stu-id="90ec7-181">Here is an example:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

<span data-ttu-id="90ec7-182">Voici un exemple de réponse JSON :</span><span class="sxs-lookup"><span data-stu-id="90ec7-182">Here is an example JSON response:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a><span data-ttu-id="90ec7-183">Limiter les Options de requête</span><span class="sxs-lookup"><span data-stu-id="90ec7-183">Limiting the Query Options</span></span>

<span data-ttu-id="90ec7-184">Les options de requête donnent au client un contrôle important sur la requête est exécutée sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="90ec7-184">The query options give the client a lot of control over the query that is run on the server.</span></span> <span data-ttu-id="90ec7-185">Dans certains cas, vous souhaiterez limiter les options disponibles pour des raisons de sécurité ou de performances.</span><span class="sxs-lookup"><span data-stu-id="90ec7-185">In some cases, you might want to limit the available options for security or performance reasons.</span></span> <span data-ttu-id="90ec7-186">Le **[Queryable]** attribut certaines intègre des propriétés pour ce.</span><span class="sxs-lookup"><span data-stu-id="90ec7-186">The **[Queryable]** attribute has some built in properties for this.</span></span> <span data-ttu-id="90ec7-187">Voici quelques exemples.</span><span class="sxs-lookup"><span data-stu-id="90ec7-187">Here are some examples.</span></span>

<span data-ttu-id="90ec7-188">Autoriser uniquement les $skip et $top, pour prendre en charge la pagination et rien d’autre :</span><span class="sxs-lookup"><span data-stu-id="90ec7-188">Allow only $skip and $top, to support paging and nothing else:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

<span data-ttu-id="90ec7-189">Autoriser le tri uniquement par certaines propriétés empêcher le tri sur les propriétés qui ne sont pas indexées dans la base de données :</span><span class="sxs-lookup"><span data-stu-id="90ec7-189">Allow ordering only by certain properties, to prevent sorting on properties that are not indexed in the database:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

<span data-ttu-id="90ec7-190">Autoriser la fonction logique « eq », mais aucune autre fonction logique :</span><span class="sxs-lookup"><span data-stu-id="90ec7-190">Allow the "eq" logical function but no other logical functions:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

<span data-ttu-id="90ec7-191">Ne pas autoriser tous les opérateurs arithmétiques :</span><span class="sxs-lookup"><span data-stu-id="90ec7-191">Do not allow any arithmetic operators:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

<span data-ttu-id="90ec7-192">Vous pouvez restreindre les options globalement en construisant un **QueryableAttribute** instance et en le passant à la **EnableQuerySupport** (fonction) :</span><span class="sxs-lookup"><span data-stu-id="90ec7-192">You can restrict options globally by constructing a **QueryableAttribute** instance and passing it to the **EnableQuerySupport** function:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a><span data-ttu-id="90ec7-193">Appeler directement les Options de requête</span><span class="sxs-lookup"><span data-stu-id="90ec7-193">Invoking Query Options Directly</span></span>

<span data-ttu-id="90ec7-194">Au lieu d’utiliser le **[Queryable]** attribut, vous pouvez appeler les options de requête directement dans votre contrôleur.</span><span class="sxs-lookup"><span data-stu-id="90ec7-194">Instead of using the **[Queryable]** attribute, you can invoke the query options directly in your controller.</span></span> <span data-ttu-id="90ec7-195">Pour ce faire, ajoutez un **ODataQueryOptions** paramètre à la méthode de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="90ec7-195">To do so, add an **ODataQueryOptions** parameter to the controller method.</span></span> <span data-ttu-id="90ec7-196">Dans ce cas, vous n’avez pas besoin du **[Queryable]** attribut.</span><span class="sxs-lookup"><span data-stu-id="90ec7-196">In this case, you don't need the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

<span data-ttu-id="90ec7-197">API Web remplit la **ODataQueryOptions** à partir de l’URI de chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="90ec7-197">Web API populates the **ODataQueryOptions** from the URI query string.</span></span> <span data-ttu-id="90ec7-198">Pour appliquer la requête, vous devez passer un **IQueryable** à la **ApplyTo** (méthode).</span><span class="sxs-lookup"><span data-stu-id="90ec7-198">To apply the query, pass an **IQueryable** to the **ApplyTo** method.</span></span> <span data-ttu-id="90ec7-199">La méthode retourne un autre **IQueryable**.</span><span class="sxs-lookup"><span data-stu-id="90ec7-199">The method returns another **IQueryable**.</span></span>

<span data-ttu-id="90ec7-200">Pour les scénarios avancés, si vous n’avez pas une **IQueryable** fournisseur de requêtes, vous pouvez examiner le **ODataQueryOptions** et traduire les options de requête dans une autre forme.</span><span class="sxs-lookup"><span data-stu-id="90ec7-200">For advanced scenarios, if you do not have an **IQueryable** query provider, you can examine the **ODataQueryOptions** and translate the query options into another form.</span></span> <span data-ttu-id="90ec7-201">(Par exemple, consultez blog de RaghuRam Nadiminti [requêtes traduire les OData en HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), qui inclut également un [exemple](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)</span><span class="sxs-lookup"><span data-stu-id="90ec7-201">(For example, see RaghuRam Nadiminti's blog post [Translating OData queries to HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), which also includes a [sample](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)</span></span>

<a id="query-validation"></a>
## <a name="query-validation"></a><span data-ttu-id="90ec7-202">Validation de requête</span><span class="sxs-lookup"><span data-stu-id="90ec7-202">Query Validation</span></span>

<span data-ttu-id="90ec7-203">Le **[Queryable]** attribut valide de la requête avant son exécution.</span><span class="sxs-lookup"><span data-stu-id="90ec7-203">The **[Queryable]** attribute validates the query before executing it.</span></span> <span data-ttu-id="90ec7-204">L’étape de validation est effectuée dans le **QueryableAttribute.ValidateQuery** (méthode).</span><span class="sxs-lookup"><span data-stu-id="90ec7-204">The validation step is performed in the **QueryableAttribute.ValidateQuery** method.</span></span> <span data-ttu-id="90ec7-205">Vous pouvez également personnaliser le processus de validation.</span><span class="sxs-lookup"><span data-stu-id="90ec7-205">You can also customize the validation process.</span></span>

<span data-ttu-id="90ec7-206">Consultez également [Guide de sécurité OData](odata-security-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="90ec7-206">Also see [OData Security Guidance](odata-security-guidance.md).</span></span>

<span data-ttu-id="90ec7-207">Tout d’abord, le remplacement parmi le validateur de classes qui est défini dans le **Web.Http.OData.Query.Validators** espace de noms.</span><span class="sxs-lookup"><span data-stu-id="90ec7-207">First, override one of the validator classes that is defined in the **Web.Http.OData.Query.Validators** namespace.</span></span> <span data-ttu-id="90ec7-208">Par exemple, la classe du programme de validation suivante désactive l’option « desc » pour l’option $orderby.</span><span class="sxs-lookup"><span data-stu-id="90ec7-208">For example, the following validator class disables the 'desc' option for the $orderby option.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

<span data-ttu-id="90ec7-209">Sous-classe le **[Queryable]** attribut pour remplacer le **ValidateQuery** (méthode).</span><span class="sxs-lookup"><span data-stu-id="90ec7-209">Subclass the **[Queryable]** attribute to override the **ValidateQuery** method.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

<span data-ttu-id="90ec7-210">Puis définissez votre attribut personnalisé soit globalement ou par contrôleur :</span><span class="sxs-lookup"><span data-stu-id="90ec7-210">Then set your custom attribute either globally or per-controller:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

<span data-ttu-id="90ec7-211">Si vous utilisez **ODataQueryOptions** directement, définir le programme de validation sur les options :</span><span class="sxs-lookup"><span data-stu-id="90ec7-211">If you are using **ODataQueryOptions** directly, set the validator on the options:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
