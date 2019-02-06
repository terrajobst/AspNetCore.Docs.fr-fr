---
title: Utiliser les conventions d’API web
author: pranavkm
description: Découvrez plus d’informations sur les conventions d’API web dans ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: scaddie
ms.custom: mvc
ms.date: 12/13/2018
uid: web-api/advanced/conventions
ms.openlocfilehash: 5ae96b213a19464045e1d0b1a76f8eb81089dc5b
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236339"
---
# <a name="use-web-api-conventions"></a>Utiliser les conventions d’API web

Par [Pranav Krishnamoorthy](https://github.com/pranavkm) et [Scott Addie](https://github.com/scottaddie)

ASP.NET Core 2.2 et ses versions ultérieures incluent un moyen d’extraire la [documentation des API](xref:tutorials/web-api-help-pages-using-swagger) courantes et de l’appliquer à plusieurs actions, à plusieurs contrôleurs ou à tous les contrôleurs au sein d’un assembly. Les conventions des API web sont un substitut permettant de décorer des actions individuelles avec [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute).

Une convention vous permet de :

* Définir les types de retours les plus courants et les codes d’état retournés à partir d’un type d’action spécifique.
* Identifier les actions qui s’écartent de la norme définie.

ASP.NET Core MVC 2.2 et ses versions ultérieures incluent un ensemble de conventions par défaut dans `Microsoft.AspNetCore.Mvc.DefaultApiConventions`. Les conventions sont basées sur le contrôleur (*ValuesController.cs*) fourni dans le modèle de projet de l’**API** ASP.NET Core. Si vos actions suivent les profils dans le modèle, vous devriez normalement réussir à utiliser les conventions par défaut. Si les conventions par défaut ne répondent pas à vos besoins, consultez [Créer des conventions d’API web](#create-web-api-conventions).

À l’exécution, <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> comprend les conventions. `ApiExplorer` est l’abstraction de MVC pour communiquer avec des générateurs de documents [OpenAPI](https://www.openapis.org/) (également nommé Swagger). Les attributs provenant de la convention appliquée sont associés à une action et sont inclus dans la documentation OpenAPI de l’action. Les [analyseurs d’API](xref:web-api/advanced/analyzers) comprennent aussi les conventions. Si votre action n’est pas conventionnelle (par exemple, elle retourne un code d’état qui n’est pas documenté par la convention appliquée), un avertissement vous encourage à documenter le code d’état.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="apply-web-api-conventions"></a>Appliquer des conventions d’API web

Les conventions ne se combinent pas : chaque action peut être associée à une seule convention. Les conventions plus spécifiques sont prioritaires par rapport à celles qui sont moins spécifiques. La sélection est non déterministe quand plusieurs conventions de même priorité s’appliquent à une action. Les options suivantes sont disponibles pour appliquer une convention à une action, de la plus spécifique à la moins spécifique :

1. `Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; s’applique à des actions individuelles, et spécifie le type de convention et la méthode de convention qui s’applique.

    Dans l’exemple suivant, la méthode de convention `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` du type de convention par défaut est appliquée à l’action `Update` :

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=snippet_ApiConventionMethod&highlight=3)]

    La méthode de convention `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` applique les attributs suivants à l’action :

    ```csharp
    [ProducesDefaultResponseType]
    [ProducesResponseType(204)]
    [ProducesResponseType(404)]
    [ProducesResponseType(400)]
    ```

Pour plus d’informations sur `[ProducesDefaultResponseType]`, consultez [Réponse par défaut](https://swagger.io/docs/specification/describing-responses/#default).

1. `Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` appliqué à un contrôleur &mdash; applique le type de convention spécifié à toutes les actions sur le contrôleur. Une méthode de convention est décorée avec des indicateurs qui déterminent les actions auxquelles la méthode de convention s’applique. Pour plus d’informations sur les indicateurs, consultez [Créer des conventions d’API web](#create-web-api-conventions)).

    Dans l’exemple suivant, l’ensemble de conventions par défaut est appliqué à toutes les actions dans *ContactsConventionController* :

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=snippet_ApiConventionTypeAttribute&highlight=2)]

1. `Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` appliqué à un assembly &mdash; applique le type de convention spécifié à tous les contrôleurs dans l’assembly actif. Il est recommandé d’appliquer des attributs de niveau assembly au fichier *Startup.cs*.

    Dans l’exemple suivant, l’ensemble de conventions par défaut est appliqué à tous les contrôleurs dans l’assembly :

    [!code-csharp[](conventions/sample/Startup.cs?name=snippet_ApiConventionTypeAttribute&highlight=1)]

## <a name="create-web-api-conventions"></a>Créer des conventions d’API web

Si les conventions d’API par défaut ne répondent pas à vos besoins, créez vos propres conventions. Une convention est :

* Un type statique avec des méthodes.
* Capable de définir des [types de réponse](#response-types) et des [exigences de noms](#naming-requirements) sur les actions.

### <a name="response-types"></a>Types de réponse

Ces méthodes sont annotées avec des attributs `[ProducesResponseType]` ou `[ProducesDefaultResponseType]`. Par exemple :

```csharp
public static class MyAppConventions
{
    [ProducesResponseType(200)]
    [ProducesResponseType(404)]
    public static void Find(int id)
    {
    }
}
```

Si des attributs de métadonnées plus spécifiques sont absents, l’application de cette convention à un assembly considère que :

* La méthode de convention s’applique à toute action nommée `Find`.
* Un paramètre nommé `id` est présent sur l’action `Find`.

### <a name="naming-requirements"></a>Exigences de dénomination

Les attributs `[ApiConventionNameMatch]` et `[ApiConventionTypeMatch]` peuvent être appliqués à la méthode de convention qui détermine les actions auxquelles ils s’appliquent. Par exemple :

```csharp
[ProducesResponseType(200)]
[ProducesResponseType(404)]
[ApiConventionNameMatch(ApiConventionNameMatchBehavior.Prefix)]
public static void Find(
    [ApiConventionNameMatch(ApiConventionNameMatchBehavior.Suffix)]
    int id)
{ }
```

Dans l'exemple précédent :

* L’option `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` appliquée à la méthode indique que la convention correspond à n’importe action préfixée par « Find ». Les exemples d’actions correspondantes incluent `Find`, `FindPet`, et `FindById`.
* `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` appliqué au paramètre indique que la convention correspond aux méthodes avec un seul paramètre se terminant par l’identificateur du suffixe. Les exemples incluent des paramètres comme `id` ou `petId`. `ApiConventionTypeMatch` peut être appliqué de façon similaire à des types pour contraindre le type du paramètre. Un argument `params[]` indique les paramètres restants pour lesquels une correspondance explicite n’est pas nécessaire.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:web-api/advanced/analyzers>
* <xref:tutorials/web-api-help-pages-using-swagger>
