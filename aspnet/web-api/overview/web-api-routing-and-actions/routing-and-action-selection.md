---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: Routage et sélection d’Action dans l’API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/27/2012
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: 2421d3dd134e9188faf6933b076fb9d22a119617
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832828"
---
<a name="routing-and-action-selection-in-aspnet-web-api"></a>Routage et sélection d’Action dans l’API Web ASP.NET
====================
par [Mike Wasson](https://github.com/MikeWasson)

Cet article décrit comment les API Web ASP.NET achemine une requête HTTP à une action particulière sur un contrôleur.

> [!NOTE]
> Pour obtenir une vue d’ensemble du routage, consultez [routage dans ASP.NET Web API](routing-in-aspnet-web-api.md).


Cet article examine les détails du processus de routage. Si vous créez un projet d’API Web et trouver certaines demandes n’obtenez pas acheminé comme vous le souhaitez, j’espère que cet article vous aide à.

Routage comprend trois phases principales :

1. Correspondance avec l’URI à un modèle d’itinéraire.
2. Sélection d’un contrôleur.
3. En sélectionnant une action.

Vous pouvez remplacer certaines parties du processus avec vos propres comportements personnalisés. Dans cet article, je décris le comportement par défaut. À la fin, j’ai Notez les emplacements où vous pouvez personnaliser le comportement.

## <a name="route-templates"></a>Modèles de routes

Un modèle d’itinéraire est similaire à un chemin d’accès de l’URI, mais il peut avoir des valeurs d’espace réservé, indiquées par des accolades :

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

Lorsque vous créez un itinéraire, vous pouvez fournir des valeurs par défaut pour certains ou tous les espaces réservés :

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

Vous pouvez également fournir des contraintes qui limitent la façon dont un segment d’URI peut correspondre à un espace réservé :

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

Le framework essaie les segments dans le chemin d’accès URI au modèle. Littéraux dans le modèle doivent correspondre exactement. Un espace réservé correspond à n’importe quelle valeur, sauf si vous spécifiez des contraintes. Le framework ne correspond pas à d’autres parties de l’URI, telles que le nom d’hôte ou les paramètres de requête. Le framework sélectionne le premier itinéraire dans la table de routage qui correspond à l’URI.

Il existe deux espaces réservés spéciaux : « {controller} » et « {action} ».

- « {controller} » fournit le nom du contrôleur.
- « {action} » fournit le nom de l’action. Dans l’API Web, la convention habituelle consiste à omettre des « {action} ».

### <a name="defaults"></a>feuille de temps

Si vous fournissez des valeurs par défaut, l’itinéraire correspond un URI qui n’a pas de ces segments. Exemple :

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

L’URI «`http://localhost/api/products`» correspond à cet itinéraire. Le segment « {category} » est affecté à la valeur par défaut « tous ».

### <a name="route-dictionary"></a>Dictionnaire d’itinéraires

Si le framework trouve une correspondance pour un URI, il crée un dictionnaire qui contient la valeur pour chaque espace réservé. Les clés sont les noms d’espace réservé, non compris entre accolades. Les valeurs sont extraites dans le chemin d’accès URI ou dans les valeurs par défaut. Le dictionnaire est stocké dans le **IHttpRouteData** objet.

Pendant cette phase de mise en correspondance de route, les espaces réservés « {action} » spécial « {controller} » sont traitées comme les autres espaces réservés. Ils sont simplement stockées dans le dictionnaire avec les autres valeurs.

Une valeur par défaut peut avoir la valeur spéciale **RouteParameter.Optional**. Si un espace réservé est affecté à cette valeur, la valeur n’est pas ajoutée au dictionnaire d’itinéraire. Exemple :

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

Pour le chemin d’accès URI « api/produits », le dictionnaire d’itinéraires contiendra :

- contrôleur : « produits »
- catégorie : « all »

Pour « api/produits/toys/123 », toutefois, le dictionnaire d’itinéraires contiendra :

- contrôleur : « produits »
- catégorie : « toys »
- ID : « 123 »

Les valeurs par défaut peuvent également inclure une valeur qui n’apparaît pas n’importe où dans le modèle d’itinéraire. Si l’itinéraire correspond à, cette valeur est stockée dans le dictionnaire. Exemple :

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

Si le chemin d’accès de l’URI est « racine/api/8 », le dictionnaire contient deux valeurs :

- contrôleur : « customers »
- ID : « 8 »

## <a name="selecting-a-controller"></a>Sélection d’un contrôleur

Sélection du contrôleur est gérée par le **IHttpControllerSelector.SelectController** (méthode). Cette méthode prend un **HttpRequestMessage** instance et retourne un **HttpControllerDescriptor**. L’implémentation par défaut est fournie par le **DefaultHttpControllerSelector** classe. Cette classe utilise un algorithme simple :

1. Rechercher dans le dictionnaire d’itinéraire pour la clé « controller ».
2. Prendre la valeur pour cette clé et ajoutez la chaîne « Controller » pour obtenir le nom de type de contrôleur.
3. Recherchez un contrôleur d’API Web portant ce nom de type.

Par exemple, si le dictionnaire de l’itinéraire contient la paire clé-valeur « controller » = « produits », puis le type de contrôleur est « ProductsController ». Si aucun type de correspondance, ou existe plusieurs correspondances, le framework retourne une erreur au client.

Dans l’étape 3, **DefaultHttpControllerSelector** utilise le **IHttpControllerTypeResolver** interface permettant d’obtenir la liste des types de contrôleur d’API Web. L’implémentation par défaut de **IHttpControllerTypeResolver** retourne toutes les classes publiques qui implémentent (a) **IHttpController**, (b) ne sont pas abstraites et (c) ont un nom qui se termine par « Controller ».

## <a name="action-selection"></a>Sélection d’action

Après avoir sélectionné le contrôleur, le framework sélectionne l’action en appelant le **IHttpActionSelector.SelectAction** (méthode). Cette méthode prend un **HttpControllerContext** et retourne un **HttpActionDescriptor**.

L’implémentation par défaut est fournie par le **ApiControllerActionSelector** classe. Pour sélectionner une action, il examine les éléments suivants :

- La méthode HTTP de la demande.
- L’espace réservé « {action} » dans le modèle d’itinéraire, le cas échéant.
- Les paramètres des actions sur le contrôleur.

Avant d’examiner l’algorithme de sélection, nous devons comprendre quelques informations sur les actions de contrôleur.

**Les méthodes sur le contrôleur sont considérés comme « actions » ?** Lorsque vous sélectionnez une action, le framework examine uniquement les méthodes d’instance publique sur le contrôleur. En outre, il exclut [« nom spécial »](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) méthodes (constructeurs, événements, les surcharges d’opérateur et ainsi de suite) et les méthodes héritées de la **ApiController** classe.

**Méthodes HTTP.** Le framework choisit uniquement les actions qui correspondent à la méthode HTTP de la demande, déterminée comme suit :

1. Vous pouvez spécifier la méthode HTTP avec un attribut : **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**,  **HttpOptions**, **HttpPatch**, **HttpPost**, ou **HttpPut**.
2. Sinon, si le nom de la méthode de contrôleur commence par « Get », « Post », « Put », « Delete », « Head », « Options » ou « Patch », puis par convention l’action prend en charge cette méthode HTTP.
3. Si aucun des éléments ci-dessus, la méthode prend en charge POST.

**Liaisons de paramètres.** Une liaison de paramètre est comment les API Web crée une valeur pour un paramètre. Voici la règle par défaut pour la liaison de paramètre :

- Types simples sont tirés de l’URI.
- Les types complexes sont tirés de corps de la demande.

Tous les exemples de types simples le [types primitifs .NET Framework](https://msdn.microsoft.com/library/system.type.isprimitive), ainsi que **DateTime**, **décimal**, **Guid**, **chaîne** , et **TimeSpan**. Pour chaque action, au moins un paramètre peut lire le corps de la demande.

> [!NOTE]
> Il est possible de remplacer les règles de liaison par défaut. Consultez [liaison de paramètre de l’API Web sous le capot](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).


En gardant, voici l’algorithme de sélection d’action.

1. Créer une liste de toutes les actions sur le contrôleur qui correspondent à la méthode de demande HTTP.
2. Si le dictionnaire d’itinéraires a une « action », supprimez les actions dont le nom ne correspond pas à cette valeur.
3. Essayez de faire correspondre les paramètres d’action à l’URI, comme suit : 

    1. Pour chaque action, obtenez une liste des paramètres qui sont un type simple, où la liaison Obtient le paramètre de l’URI. Exclure les paramètres facultatifs.
    2. Dans cette liste, essayez de trouver une correspondance pour chaque nom de paramètre, dans le dictionnaire d’itinéraires ou dans la chaîne de requête URI. Les correspondances respectent la casse et ne dépendent pas de l’ordre des paramètres.
    3. Sélectionnez une action dans lequel chaque paramètre dans la liste a une correspondance dans l’URI.
    4. Si une action plus répond à ces critères, choisir celui avec la plupart des correspondances de paramètre.
4. Ignorer les actions avec le **[NonAction]** attribut.

Étape #3 est probablement le plus à confusion. L’idée fondamentale est qu’un paramètre peut obtenir sa valeur à partir de l’URI, à partir du corps de demande ou à partir d’une liaison personnalisée. Pour les paramètres qui proviennent de l’URI, nous voulons vérifier que l’URI contienne en fait une valeur pour ce paramètre, dans le chemin d’accès (via le dictionnaire d’itinéraires) ou dans la chaîne de requête.

Par exemple, considérez l’action suivante :

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

Le *id* le paramètre est lié à l’URI. Par conséquent, cette action peut correspondre uniquement à un URI qui contient une valeur pour « id », dans le dictionnaire d’itinéraires ou dans la chaîne de requête.

Les paramètres facultatifs sont une exception, car ils sont facultatifs. Pour un paramètre facultatif, il est OK si la liaison ne peut pas obtenir la valeur de l’URI.

Les types complexes sont une exception pour une autre raison. Un type complexe ne peut se lier à l’URI via une liaison personnalisée. Mais dans ce cas, le framework ne peut pas savoir à l’avance le si le paramètre établirait une liaison avec un URI particulier. Pour découvrir, elle aurait besoin d’appeler la liaison. L’objectif de l’algorithme de sélection consiste à sélectionner une action à partir de la description statique, avant d’appeler toutes les liaisons. Par conséquent, les types complexes sont exclus de l’algorithme de correspondance.

Une fois que l’action est sélectionnée, toutes les liaisons de paramètre sont appelés.

Résumé :

- L’action doit correspondre à la méthode HTTP de la demande.
- Le nom d’action doit correspondre à l’entrée « action » dans le dictionnaire d’itinéraires, le cas échéant.
- Pour chaque paramètre de l’action, si le paramètre provient de l’URI, puis le nom du paramètre doit être présent dans le dictionnaire d’itinéraires ou dans la chaîne de requête URI. (Les paramètres avec des types complexes et les paramètres facultatifs sont exclus.)
- Essayez de faire correspondre le plus grand nombre de paramètres. La meilleure correspondance peut être une méthode sans paramètres.

## <a name="extended-example"></a>Exemple étendu

Itinéraires :

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

Contrôleur :

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

Requête HTTP :

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a>Acheminer la mise en correspondance

L’URI correspond à l’itinéraire nommé « DefaultApi ». Le dictionnaire de l’itinéraire contient les entrées suivantes :

- contrôleur : « produits »
- ID : « 1 »

Le dictionnaire d’itinéraire ne contient-elle pas les paramètres de chaîne de requête, la « version » et le « détails », mais ils sont toujours considérés lors de la sélection de l’action.

### <a name="controller-selection"></a>Sélection du contrôleur

À partir de l’entrée « controller » dans le dictionnaire d’itinéraires, le type de contrôleur est `ProductsController`.

### <a name="action-selection"></a>Sélection d’action

La requête HTTP est une requête GET. Les actions de contrôleur qui prennent en charge de GET sont `GetAll`, `GetById`, et `FindProductsByName`. Le dictionnaire d’itinéraire ne contient pas une entrée pour « action », nous n’avez pas besoin de correspondre au nom de l’action.

Ensuite, nous essayons de correspondre aux noms de paramètre pour les actions, examinant uniquement les actions GET.

| Action | Paramètres de correspondance |
| --- | --- |
| `GetAll` | none |
| `GetById` | « id » |
| `FindProductsByName` | « nom » |

Notez que le *version* paramètre de `GetById` n’est pas considérée, s’agissant d’un paramètre facultatif.

Le `GetAll` méthode correspond à façon triviale. Le `GetById` méthode correspond également à, car le dictionnaire de l’itinéraire contient « id ». Le `FindProductsByName` méthode ne correspond pas.

Le `GetById` méthode wins, car il correspond à un seul paramètre, et aucun paramètre pour `GetAll`. La méthode est appelée avec les valeurs de paramètre suivantes :

- *ID* = 1
- *version* = 1.5

Notez que même si *version* n’a été utilisé dans l’algorithme de sélection, la valeur du paramètre provient de la chaîne de requête URI.

## <a name="extension-points"></a>Points d’extension

API Web fournit des points d’extension pour certaines parties du processus de routage.

| Interface | Description |
| --- | --- |
| **IHttpControllerSelector** | Sélectionne le contrôleur. |
| **IHttpControllerTypeResolver** | Obtient la liste des types de contrôleur. Le **DefaultHttpControllerSelector** choisit le type de contrôleur à partir de cette liste. |
| **IAssembliesResolver** | Obtient la liste des assemblys de projet. Le **IHttpControllerTypeResolver** interface utilise cette liste pour rechercher les types de contrôleur. |
| **IHttpControllerActivator** | Crée des instances de contrôleur. |
| **IHttpActionSelector** | Sélectionne l’action. |
| **IHttpActionInvoker** | Appelle l’action. |

Pour fournir votre propre implémentation pour une de ces interfaces, utilisez le **Services** collection sur le **HttpConfiguration** objet :

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
