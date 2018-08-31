---
title: Tag Helper Cache dans ASP.NET Core MVC
author: pkellner
description: Montre comment utiliser un Tag Helper Cache
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 425d8c2235f0070665bc0c967d2498f2cff2a4a6
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751443"
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a>Tag Helper Cache dans ASP.NET Core MVC

Par [Peter Kellner](http://peterkellner.net) 

Le Tag Helper Cache permet d’améliorer considérablement les performances de votre application ASP.NET Core en mettant en cache son contenu dans le fournisseur de caches ASP.NET Core interne.

Le moteur de vue Razor définit la valeur par défaut `expires-after` sur vingt minutes.

Le balisage Razor suivant met en cache la date/l’heure :

```cshtml
<cache>@DateTime.Now</cache>
```

La première demande à la page qui contient `CacheTagHelper` affiche la date/l’heure actuelle. Les autres demandes affichent la valeur mise en cache jusqu’à ce que le cache expire (par défaut, 20 minutes) ou soit supprimé par sollicitation de la mémoire.

Vous pouvez définir la durée du cache avec les attributs suivants :

## <a name="cache-tag-helper-attributes"></a>Attributs de Tag Helper Cache

- - -

### <a name="enabled"></a>enabled    


| Type d’attribut    | Valeurs valides      |
|----------------   |----------------   |
| boolean           | "true" (valeur par défaut)  |
|                   | "false"   |


Détermine si le contenu joint par le Tag Helper Cache est mis en cache. La valeur par défaut est `true`.  Si la valeur est `false`, ce Tag Helper Cache n’a aucun effet de mise en cache sur la sortie rendue.

Exemple :

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a>expires-on 

| Type d’attribut |           Exemple de valeur            |
|----------------|------------------------------------|
| DateTimeOffset | "@new DateTime(2025,1,29,17,02,0)" |

Définit une date d’expiration absolue. L’exemple suivant met en cache le contenu du Tag Helper Cache jusqu’à 17:02 le 29 janvier 2025.

Exemple :

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a>expires-after

| Type d’attribut |        Exemple de valeur         |
|----------------|------------------------------|
|    TimeSpan    | "@TimeSpan.FromSeconds(120)" |

Définit la durée à partir de l’heure de la première demande pour mettre en cache le contenu. 

Exemple :

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a>expires-sliding

| Type d’attribut |        Exemple de valeur        |
|----------------|-----------------------------|
|    TimeSpan    | "@TimeSpan.FromSeconds(60)" |

Définit l’heure à laquelle une entrée de cache doit être supprimée si elle n’a fait l’objet d’aucun accès.

Exemple :

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a>vary-by-header

| Type d’attribut    | Exemples de valeurs                |
|----------------   |----------------               |
| Chaîne            | "User-Agent"                  |
|                   | "User-Agent,content-encoding" |

Accepte une seule valeur d’en-tête ou une liste séparée par des virgules de valeurs d’en-tête qui déclenchent une actualisation du cache quand elles changent. L’exemple suivant analyse la valeur d’en-tête `User-Agent`. L’exemple met en cache le contenu de chaque valeur `User-Agent` différente présentée au serveur web.

Exemple :

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a>vary-by-query

| Type d’attribut    | Exemples de valeurs                |
|----------------   |----------------               |
| Chaîne            | "Make"                |
|                   | "Make,Model" |

Accepte une seule valeur d’en-tête ou une liste séparée par des virgules de valeurs d’en-tête qui déclenchent une actualisation du cache quand la valeur d’en-tête change. L’exemple suivant examine les valeurs de `Make` et `Model`.

Exemple :

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a>vary-by-route

| Type d’attribut    | Exemples de valeurs                |
|----------------   |----------------               |
| Chaîne            | "Make"                |
|                   | "Make,Model" |

Accepte une seule valeur d’en-tête ou une liste séparée par des virgules de valeurs d’en-tête qui déclenchent une actualisation du cache quand les valeurs des paramètres de données de routage changent. Exemple :

*Startup.cs* 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```

*Index.cshtml*

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-cookie"></a>vary-by-cookie

| Type d’attribut    | Exemples de valeurs                |
|----------------   |----------------               |
| Chaîne            | ".AspNetCore.Identity.Application"                |
|                   | ".AspNetCore.Identity.Application,HairColor" |

Accepte une seule valeur d’en-tête ou une liste séparée par des virgules de valeurs d’en-tête qui déclenchent une actualisation du cache quand les valeurs d’en-tête changent. L’exemple suivant examine le cookie associé à ASP.NET Core Identity. Quand un utilisateur est authentifié, cookie de la demande à définir qui déclenche une actualisation du cache.

Exemple :

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a>vary-by-user

| Type d’attribut    | Exemples de valeurs                |
|----------------   |----------------               |
| Booléen             | "true"                  |
|                     | "false" (valeur par défaut) |

Spécifie si le cache doit ou non être réinitialisé quand l’utilisateur connecté (ou principal du contexte) change. L’utilisateur actuel est également connu comme principal du contexte de la demande et peut être affiché dans une vue Razor en référençant `@User.Identity.Name`.

L’exemple suivant examine l’utilisateur actuellement connecté.  

Exemple :

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

L’utilisation de cet attribut permet de conserver le contenu dans le cache via un cycle de connexion et de déconnexion.  Quand vous utilisez `vary-by-user="true"`, une action de connexion et de déconnexion invalide le cache pour l’utilisateur authentifié.  Le cache est invalidé, car une nouvelle valeur de cookie unique est générée lors de la connexion. Le cache est conservé pour l’état anonyme quand aucun cookie n’est présent ou n’a expiré. Cela signifie que, si aucun utilisateur n’est connecté, le cache est conservé.

- - -

### <a name="vary-by"></a>vary-by

| Type d’attribut | Exemples de valeurs |
|----------------|----------------|
|     Chaîne     |    "@Model"    |

Permet la personnalisation des données mises en cache. Quand l’objet référencé par la valeur de la chaîne de l’attribut change, le contenu du Tag Helper Cache est mis à jour. Souvent, une concaténation de chaîne des valeurs de modèle est affectée à cet attribut.  En effet, cela signifie qu’une mise à jour de l’une des valeurs concaténées invalide le cache.

L’exemple suivant suppose que la méthode de contrôleur restituant la vue additionne la valeur entière des deux paramètres de routage, `myParam1` et `myParam2`, et la retourne en tant que propriété de modèle unique. Quand cette somme est modifiée, le contenu du Tag Helper Cache est rendu et mis en cache à nouveau.  

Exemple :

Action :

```csharp
public IActionResult Index(string myParam1,string myParam2,string myParam3)
{
    int num1;
    int num2;
    int.TryParse(myParam1, out num1);
    int.TryParse(myParam2, out num2);
    return View(viewName, num1 + num2);
}
```

*Index.cshtml*

```cshtml
<cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="priority"></a>priority

| Type d’attribut    | Exemples de valeurs                |
|----------------   |----------------               |
| CacheItemPriority  | "High"                   |
|                    | "Low" |
|                    | "NeverRemove" |
|                    | "Normal" |

Fournit des instructions de suppression de cache au fournisseur de caches intégré. Le serveur web supprime d’abord les entrées de cache `Low` en cas de sollicitation de la mémoire.

Exemple :

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

L’attribut `priority` ne garantit pas un niveau spécifique de rétention du cache. `CacheItemPriority` est uniquement une suggestion. Si vous affectez à cet attribut la valeur `NeverRemove`, il n’est pas garanti que le cache sera toujours conservé. Pour plus d’informations, consultez [Ressources supplémentaires](#additional-resources).

Le Tag Helper Cache dépend du [service de cache en mémoire](xref:performance/caching/memory). Le Tag Helper Cache ajoute le service s’il ne l’a pas encore été.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Mettre en cache en mémoire](xref:performance/caching/memory)
* [Présentation d’Identity](xref:security/authentication/identity)
