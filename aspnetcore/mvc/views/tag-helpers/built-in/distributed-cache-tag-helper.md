---
title: Tag Helper Cache distribué dans ASP.NET Core
author: pkellner
description: Montre comment utiliser un Tag Helper Cache
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: d33c22802030eb9bc77baa64b83c9bbd7e902195
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275173"
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a>Tag Helper Cache distribué dans ASP.NET Core

Par [Peter Kellner](http://peterkellner.net) 

Le Tag Helper Cache distribué permet d’améliorer considérablement les performances de votre application ASP.NET Core en mettant en cache son contenu dans une source de cache distribué.

Le Tag Helper Cache distribué hérite de la même classe de base que le Tag Helper Cache. Tous les attributs associés au Tag Helper Cache fonctionnent également sur le Tag Helper Cache distribué.

Le Tag Helper Cache distribué suit le **principe des dépendances explicites** appelé **injection de constructeur**. Plus précisément, le conteneur d’interface `IDistributedCache` est passé dans le constructeur du Tag Helper Cache distribué. Si aucune implémentation concrète d’`IDistributedCache` n’a été créée dans `ConfigureServices`, qui se trouve généralement dans startup.cs, le Tag Helper Cache distribué utilise le même fournisseur en mémoire pour le stockage des données mises en cache que le Tag Helper Cache de base.

## <a name="distributed-cache-tag-helper-attributes"></a>Attributs de Tag Helper Cache distribué

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a>enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority

Consultez le Tag Helper Cache pour obtenir les définitions. Comme le Tag Helper Cache distribué hérite de la même classe que le Tag Helper Cache, tous ces attributs sont communs à partir du Tag Helper Cache.

- - -

### <a name="name-required"></a>name (obligatoire)

| Type d’attribut    | Exemple de valeur     |
|----------------   |----------------   |
| chaîne    | "my-distributed-cache-unique-key-101"     |

L’attribut `name` obligatoire est utilisé comme clé de ce cache stockée pour chaque instance d’un Tag Helper Cache distribué. Contrairement au Tag Helper Cache de base qui affecte une clé à chaque instance de Tag Helper Cache selon le nom de la page Razor et l’emplacement du Tag Helper dans la page Razor, le Tag Helper Cache distribué base uniquement sa clé sur l’attribut `name`

Exemple d’utilisation :

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a>Implémentations IDistributedCache de Tag Helper Cache distribué

Il existe deux implémentations d’`IDistributedCache` intégrées à ASP.NET Core. L’une est basée sur SQL Server et l’autre sur Redis. Pour obtenir les détails de ces implémentations, consultez <xref:performance/caching/distributed>. Les deux implémentations impliquent la définition d’une instance de `IDistributedCache` dans le fichier *Startup.cs* d’ASP.NET Core.

Aucun attribut de balise n’est spécifiquement associé à l’utilisation d’une implémentation d’`IDistributedCache`.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
