---
title: Utiliser les conventions d’API web
author: pranavkm
description: Découvrez plus d’informations sur les conventions d’API web dans ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: pranavkm
ms.custom: mvc
ms.date: 11/13/2018
uid: web-api/advanced/conventions
ms.openlocfilehash: ede9a46c160cf6a49aa93da710af0bf0b8f59acc
ms.sourcegitcommit: c4572be5ebb301013a5698caf9b5572b76cb2e34
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52710073"
---
# <a name="use-web-api-conventions"></a>Utiliser les conventions d’API web

ASP.NET Core 2.2 introduit un moyen d’extraire la [documentation des API](xref:tutorials/web-api-help-pages-using-swagger) courantes et de l’appliquer à plusieurs actions, à plusieurs contrôleurs ou à tous les contrôleurs au sein d’un assembly. Les conventions des API web sont un substitut permettant de décorer des actions individuelles avec [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute). Elles vous permettent de définir les types de retour « conventionnels » les plus courants et les codes d’état retournés depuis votre action avec un moyen de sélectionner la méthode de convention qui s’applique à une action.

Par défaut, ASP.NET Core MVC 2.2 est livré avec un ensemble de conventions par défaut, `Microsoft.AspNetCore.Mvc.DefaultApiConventions`. Les conventions sont basées sur le contrôleur généré automatiquement comme modèle par ASP.NET Core. Si vos actions suivent le modèle produit par cette génération de modèles automatique, vous devez normalement réussir à utiliser les conventions par défaut.

À l’exécution, <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> comprend les conventions. `ApiExplorer` est l’abstraction de MVC pour communiquer avec des générateurs de documents d’API ouvertes. Les attributs provenant de la convention appliquée sont associés à une action et sont inclus dans la documentation Swagger de l’action. Les analyseurs d’API comprennent aussi les conventions. Si votre action n’est pas conventionnelle (par exemple, elle retourne un code d’état qui n’est pas documenté par la convention appliquée), elle produit un avertissement, qui vous encourage à le documenter.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="apply-web-api-conventions"></a>Appliquer des conventions d’API web

Il existe trois façons d’appliquer une convention. Les conventions ne se combinent pas : chaque action peut être associée à une seule convention. Les conventions plus spécifiques (détaillées ci-dessous) sont prioritaires par rapport à celles qui sont moins spécifiques. La sélection est non déterministe quand plusieurs conventions de même priorité s’appliquent à une action. Les options suivantes sont disponibles pour appliquer une convention à une action, de la plus spécifique à la moins spécifique :

1. `Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; s’applique à des actions individuelles, et spécifie le type de convention et la méthode de convention qui s’applique. Dans l’exemple suivant, la méthode de convention `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` est appliquée à l’action `Update` :

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=apiconventionmethod&highlight=2-3)]

1. `Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` appliqué à un contrôleur &mdash; applique le type de convention à toutes les actions sur le contrôleur. Les méthodes de convention sont décorées avec des indicateurs qui déterminent les actions auxquelles elles s’appliquent (détails dans le cadre des conventions de création). Exemple :

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=apiconventiontypeattribute)]

1. `Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` appliqué à un assembly &mdash; applique le type de convention à tous les contrôleurs dans l’assembly actif. Exemple :

    [!code-csharp[](conventions/sample/Startup.cs?name=apiconventiontypeattribute)]

## <a name="create-web-api-conventions"></a>Créer des conventions d’API web

Une convention est un type statique avec des méthodes. Ces méthodes sont annotées avec des attributs `[ProducesResponseType]` ou `[ProducesDefaultResponseType]`.

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

L’application de cette convention à un assembly fait que la méthode de convention est appliquée aux actions ayant le nom `Find` et avec un seul paramètre nommé `id`, dès lors qu’elles n’ont pas d’autres attributs de métadonnées plus spécifiques.

En plus de `[ProducesResponseType]` et de `[ProducesDefaultResponseType]`, `[ApiConventionNameMatch]` et `[ApiConventionTypeMatch]` peuvent être appliquées à la méthode de convention qui détermine les méthodes auxquelles elles s’appliquent. Exemple :

```csharp
[ProducesResponseType(200)]
[ProducesResponseType(404)]
[ApiConventionNameMatch(ApiConventionNameMatchBehavior.Prefix)]
public static void Find(
    [ApiConventionNameMatch(ApiConventionNameMatchBehavior.Suffix)]
    int id)
{ }
```

* L’option `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` appliquée à la méthode indique que la convention peut correspondre à n’importe action dès lors qu’elle est préfixée par « Find ». Ceci inclut des méthodes comme `Find`, `FindPet` ou `FindById`.
* `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` appliqué au paramètre indique que la convention peut correspondre aux méthodes avec un seul paramètre se terminant par l’identificateur du suffixe. Ceci inclut des paramètres comme `id` ou `petId`. `ApiConventionTypeMatch` peut être appliqué de façon similaire à des types pour contraindre le type du paramètre. Un argument `params[]` peut être utilisé afin d’indiquer les paramètres restants pour lesquels une correspondance explicite n’est pas nécessaire.
