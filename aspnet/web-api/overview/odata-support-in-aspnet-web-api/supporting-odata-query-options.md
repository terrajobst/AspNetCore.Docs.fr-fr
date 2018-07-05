---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: Prise en charge des Options de requête OData dans ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 02/04/2013
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 029a791074a5ab88f1c6d83311e88fc9f726fe6f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814939"
---
<a name="supporting-odata-query-options-in-aspnet-web-api-2"></a>Prise en charge des Options de requête OData dans ASP.NET Web API 2
====================
par [Mike Wasson](https://github.com/MikeWasson)

OData définit les paramètres qui peuvent être utilisées pour modifier une requête OData. Le client envoie ces paramètres dans la chaîne de requête de l’URI de demande. Par exemple, pour trier les résultats, un client utilise le paramètre $orderby :

`http://localhost/Products?$orderby=Name`

La spécification OData appelle ces paramètres *options de requête*. Vous pouvez activer des options de requête OData pour n’importe quel contrôleur API Web dans votre projet &#8212; des contrôleurs ne doivent pas être un point de terminaison OData. Cela vous donne un moyen pratique d’ajouter des fonctionnalités telles que le filtrage et tri à n’importe quelle application API Web.

Avant d’activer les options de requête, lisez la rubrique [conseils de sécurité](odata-security-guidance.md).

- [Activation des Options de requête OData](#enable)
- [Exemples de requêtes](#examples)
- [Pagination orientée serveur](#server-paging)
- [Limiter les Options de requête](#limiting_query_options)
- [Appeler directement les Options de requête](#ODataQueryOptions)
- [Validation de requête](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a>Activation des Options de requête OData

API Web prend en charge les options de requête OData suivantes :

| Option | Description |
| --- | --- |
| $expand | Développe les entités connexes inline. |
| $filter | Filtre les résultats selon une condition booléenne. |
| $inlinecount | Indique au serveur d’inclure le nombre total d’entités correspondantes dans la réponse. (Utile pour la pagination côté serveur). |
| $orderby | Trie les résultats. |
| $select | Sélectionne les propriétés à inclure dans la réponse. |
| $skip | Ignore les n premiers résultats. |
| $top | Retourne uniquement le n premiers résultats. |

Pour utiliser les options de requête OData, vous devez les activer explicitement. Vous pouvez les activer dans le monde entier pour l’application entière, ou les activer pour des contrôleurs spécifiques ou des actions spécifiques.

Pour activer les options de requête OData dans le monde entier, appelez **EnableQuerySupport** sur le **HttpConfiguration** classe au démarrage :

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

Le **EnableQuerySupport** méthode active les options de requête dans le monde entier pour toute action de contrôleur qui retourne un **IQueryable** type. Si vous ne souhaitez pas les options de requête est activées pour l’application entière, vous pouvez les activer pour les actions de contrôleur spécifique en ajoutant le **[Queryable]** d’attribut à la méthode d’action.

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a>Exemples de requêtes

Cette section montre les types de requêtes qui sont possibles en utilisant les options de requête OData. Pour obtenir des détails spécifiques sur les options de requête, consultez la documentation OData à [www.odata.org](http://www.odata.org/).

Pour plus d’informations à propos de $développez et $select, consultez [à l’aide de $select, $expand et $value dans ASP.NET Web API OData](using-select-expand-and-value.md).

**Pagination pilotée par client**

Pour les jeux d’entités volumineux, le client souhaite peut limiter le nombre de résultats. Par exemple, un client peut afficher des entrées de 10 à la fois, avec des liens « suivant » pour obtenir la page suivante de résultats. Pour ce faire, le client utilise les options $top et $skip.

`http://localhost/Products?$top=10&$skip=20`

L’option $top donne le nombre maximal d’entrées à retourner, et l’option $skip renvoie le nombre d’entrées à ignorer. L’exemple précédent extrait entrées 21 à 30.

**Filtrage**

L’option $filter permet à un client de filtrer les résultats en appliquant une expression booléenne. Les expressions de filtre sont très puissantes ; ils incluent des opérateurs arithmétiques et logiques, les fonctions de chaîne et les fonctions de date.

| Retourne tous les produits de catégorie est égal à « Toys ». | `http://localhost/Products?$filter=Category` EQ « Toys » |
| --- | --- |
| Retourner tous les produits dont le prix est inférieur à 10. | `http://localhost/Products?$filter=Price` lt 10 |
| Opérateurs logiques : renvoyer tous les produits de prix où > = 5 et des prix < = 15. | `http://localhost/Products?$filter=Price` GE 5 et prix le 15 |
| Fonctions de chaîne : renvoie tous les produits avec « zz » dans leur nom. | `http://localhost/Products?$filter=substringof('zz',Name)` |
| Fonctions de date : retourner tous les produits avec ReleaseDate après 2005. | `http://localhost/Products?$filter=year(ReleaseDate)` gt 2005 |

**Tri**

Pour trier les résultats, utilisez le filtre $orderby.

| Trier par prix. | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| Trier par prix décroissant (ordre décroissant). | `http://localhost/Products?$orderby=Price desc` |
| Trier par catégorie, puis trier par prix en ordre décroissant dans les catégories. | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a>Pagination orientée serveur

Si votre base de données contient des millions d’enregistrements, vous ne souhaitez pas les envoyer dans une charge utile. Pour éviter ce problème, le serveur peut limiter le nombre d’entrées qu’il envoie une réponse unique. Pour activer la pagination serveur, définissez la **PageSize** propriété dans le **Queryable** attribut. La valeur est le nombre maximal d’entrées à retourner.

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

Si votre contrôleur retourne le format OData, le corps de réponse contient un lien vers la page suivante de données :

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

Le client peut utiliser ce lien pour extraire la page suivante. Pour connaître le nombre total d’entrées dans le jeu de résultats, le client peut définir l’option de requête $inlinecount avec la valeur « allpages ».

`http://localhost/Products?$inlinecount=allpages`

La valeur « allpages » indique au serveur d’inclure le nombre total dans la réponse :

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> Liens de la page suivante et le nombre inline requièrent le format OData. La raison est qu’OData définit des champs spéciaux dans le corps de réponse pour contenir le nombre et le lien.


Pour des formats non-OData, il est toujours possible prendre en charge le nombre de liens et inline de page suivante, en encapsulant les résultats de requête dans un **PageResult&lt;T&gt;**  objet. Toutefois, elle nécessite un peu plus de code. Voici un exemple :

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

Voici un exemple de réponse JSON :

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a>Limiter les Options de requête

Les options de requête donnent au client un contrôle important sur la requête est exécutée sur le serveur. Dans certains cas, vous souhaiterez limiter les options disponibles pour des raisons de sécurité ou les performances. Le **[Queryable]** attribut a certaines intégrés dans les propriétés pour cela. Voici quelques exemples.

Autoriser uniquement les $skip et $top, pour prendre en charge la pagination et rien d’autre :

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

Autoriser le tri uniquement par certaines propriétés empêcher le tri sur les propriétés qui ne sont pas indexées dans la base de données :

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

Autoriser la fonction logique « eq », mais aucune autre fonction logique :

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

Ne pas autoriser tous les opérateurs arithmétiques :

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

Vous pouvez restreindre les options dans le monde entier en construisant un **QueryableAttribute** instance et en le passant à la **EnableQuerySupport** (fonction) :

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a>Appeler directement les Options de requête

Au lieu d’utiliser le **[Queryable]** attribut, vous pouvez appeler les options de requête directement dans votre contrôleur. Pour ce faire, ajoutez un **ODataQueryOptions** paramètre à la méthode de contrôleur. Dans ce cas, vous n’avez pas besoin du **[Queryable]** attribut.

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

API Web remplit la **ODataQueryOptions** à partir de l’URI de chaîne de requête. Pour appliquer la requête, transmettez un **IQueryable** à la **ApplyTo** (méthode). La méthode retourne un autre **IQueryable**.

Pour les scénarios avancés, si vous n’avez pas un **IQueryable** fournisseur de requête, vous pouvez examiner le **ODataQueryOptions** et traduire les options de requête dans un autre formulaire. (Consultez par exemple, billet de blog de RaghuRam Nadiminti [requêtes OData traduire en HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), qui inclut également un [exemple](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)

<a id="query-validation"></a>
## <a name="query-validation"></a>Validation de requête

Le **[Queryable]** attribut valide la requête avant son exécution. L’étape de validation est effectuée dans le **QueryableAttribute.ValidateQuery** (méthode). Vous pouvez également personnaliser le processus de validation.

Consultez également [conseils de sécurité](odata-security-guidance.md).

Tout d’abord, remplacement une du validateur classes qui est défini dans le **Web.Http.OData.Query.Validators** espace de noms. Par exemple, la classe du programme de validation suivante désactive l’option « desc » pour l’option $orderby.

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

Sous-classe la **[Queryable]** attribut à substituer le **ValidateQuery** (méthode).

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

Puis définissez votre attribut personnalisé soit globalement ou par contrôleur :

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

Si vous utilisez **ODataQueryOptions** directement, définir le programme de validation sur les options :

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
