---
title: Réutilisation d’objets avec ObjectPool dans ASP.NET Core
author: rick-anderson
description: Conseils pour améliorer les performances dans les applications de ASP.NET Core à l’aide de ObjectPool.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 04/11/2019
uid: performance/ObjectPool
ms.openlocfilehash: 771f19e54a908b8b2cd85ff72f368f16e94a2310
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666109"
---
# <a name="object-reuse-with-objectpool-in-aspnet-core"></a>Réutilisation d’objets avec ObjectPool dans ASP.NET Core

Par [Steve Gordon](https://twitter.com/stevejgordon), [Ryan Nowak](https://github.com/rynowak)et [Rick Anderson](https://twitter.com/RickAndMSFT)

<xref:Microsoft.Extensions.ObjectPool> fait partie de l’infrastructure ASP.NET Core qui prend en charge la conservation d’un groupe d’objets en mémoire à des fins de réutilisation, au lieu d’autoriser les objets à être récupérés par le garbage collector.

Vous souhaiterez peut-être utiliser le pool d’objets si les objets gérés sont :

- Coûteuse à allouer/initialiser.
- Représentent une ressource limitée.
- Utilisé de manière prévisible et fréquente.

Par exemple, l’infrastructure ASP.NET Core utilise le pool d’objets à certains endroits pour réutiliser des instances <xref:System.Text.StringBuilder>. `StringBuilder` alloue et gère ses propres mémoires tampons pour contenir les données de caractères. ASP.NET Core utilise régulièrement `StringBuilder` pour implémenter des fonctionnalités, et les réutiliser améliorent les performances.

La mise en pool d’objets n’améliore pas toujours les performances :

- À moins que le coût d’initialisation d’un objet ne soit élevé, il est généralement plus lent de récupérer l’objet à partir du pool.
- Les objets gérés par le pool ne sont pas désalloués tant que le pool n’est pas libéré.

Utilisez le mise en pool d’objets uniquement après avoir collecté les données de performances à l’aide de scénarios réalistes pour votre application ou bibliothèque.

**AVERTISSEMENT : le `ObjectPool` n’implémente pas `IDisposable`. Nous vous déconseillons de l’utiliser avec des types nécessitant une suppression.**

**Remarque : le ObjectPool n’impose aucune limite quant au nombre d’objets qu’il va allouer, il limite le nombre d’objets qu’il va conserver.**

## <a name="concepts"></a>Concepts

<xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> : abstraction du pool d’objets de base. Permet d’obtenir et de retourner des objets.

<xref:Microsoft.Extensions.ObjectPool.PooledObjectPolicy%601> : implémentez cette méthode pour personnaliser la façon dont un objet est créé et la façon dont il est *réinitialisé* lorsqu’il est retourné au pool. Cela peut être passé dans un pool d’objets que vous créez directement... NI

<xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider.Create*> agit comme une fabrique pour la création de pools d’objets.
<!-- REview, there is no ObjectPoolProvider<T> -->

Le ObjectPool peut être utilisé dans une application de plusieurs façons :

* Instanciation d’un pool.
* Inscription d’un pool dans une [injection de dépendance](xref:fundamentals/dependency-injection) (di) en tant qu’instance.
* L’inscription de la `ObjectPoolProvider<>` dans DI et son utilisation comme fabrique.

## <a name="how-to-use-objectpool"></a>Utilisation de ObjectPool

Appelez <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> pour obtenir un objet et <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1.Return*> pour retourner l’objet.  Vous n’avez pas besoin de retourner chaque objet. Si vous ne renvoyez pas d’objet, il sera récupéré par le garbage collector.

## <a name="objectpool-sample"></a>Exemple ObjectPool

Le code suivant :

* Ajoute `ObjectPoolProvider` au conteneur d' [injection de dépendances](xref:fundamentals/dependency-injection) (di).
* Ajoute et configure `ObjectPool<StringBuilder>` vers le conteneur DI.
* Ajoute le `BirthdayMiddleware`.

[!code-csharp[](ObjectPool/ObjectPoolSample/Startup.cs?name=snippet)]

Le code suivant implémente `BirthdayMiddleware`

[!code-csharp[](ObjectPool/ObjectPoolSample/BirthdayMiddleware.cs?name=snippet)]
