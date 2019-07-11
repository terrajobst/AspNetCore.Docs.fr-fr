---
title: Réutilisation d’un objet avec le pool de threads dans ASP.NET Core
author: rick-anderson
description: Conseils pour augmenter les performances dans les applications ASP.NET Core à l’aide du pool de threads.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 04/11/2019
uid: performance/ObjectPool
ms.openlocfilehash: 771f19e54a908b8b2cd85ff72f368f16e94a2310
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815521"
---
# <a name="object-reuse-with-objectpool-in-aspnet-core"></a>Réutilisation d’un objet avec le pool de threads dans ASP.NET Core

Par [Steve Gordon](https://twitter.com/stevejgordon), [Ryan Nowak](https://github.com/rynowak), et [Rick Anderson](https://twitter.com/RickAndMSFT)

<xref:Microsoft.Extensions.ObjectPool> fait partie de l’infrastructure ASP.NET Core qui prend en charge en conservant un groupe d’objets en mémoire pour une réutilisation au lieu d’objets garbage collectées.

Vous souhaiterez peut-être utiliser le pool d’objets si les objets qui sont gérés sont :

- Cher / d’initialiser allouer.
- Représentent une ressource limitée.
- Utilisé de manière prévisible et fréquemment.

Par exemple, le framework ASP.NET Core utilise le pool d’objets à certains endroits à réutiliser <xref:System.Text.StringBuilder> instances. `StringBuilder` alloue et gère son propre mémoires tampons pour conserver les données de caractère. ASP.NET Core utilise régulièrement `StringBuilder` pour implémenter les fonctionnalités, et réutilisez permet un gain de performances.

Le pool d’objets ne toujours améliore les performances :

- À moins que le coût de l’initialisation d’un objet est élevé, il est généralement plus lent obtenir l’objet à partir du pool.
- Objets gérés par le pool ne sont pas désalloués jusqu'à ce que le pool est désallouée.

Utiliser le pool d’objets uniquement après la collecte de données de performances à l’aide de scénarios réalistes pour votre application ou une bibliothèque.

**AVERTISSEMENT : Le `ObjectPool` n’implémente pas `IDisposable`. Nous ne recommandons pas l’utiliser avec les types qui ont besoin de disposition.**

**REMARQUE : Le pool de threads ne place pas une limite sur le nombre d’objets il alloue, elle place une limite sur le nombre d’objets, qu'il conserve.**

## <a name="concepts"></a>Concepts

<xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> -l’abstraction de pool d’objet de base. Utilisé pour obtenir et retourner des objets.

<xref:Microsoft.Extensions.ObjectPool.PooledObjectPolicy%601> -implémenter cela pour personnaliser la façon dont un objet est créé et comment il est *réinitialiser* lorsque retournée au pool. Cela peut être passé dans un pool d’objets que vous construisez directement... Ou

<xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider.Create*> sert de fabrique pour la création de pools d’objet.
<!-- REview, there is no ObjectPoolProvider<T> -->

Le pool de threads peut être utilisé dans une application de plusieurs façons :

* Instanciation d’un pool.
* L’inscription d’un pool dans [l’injection de dépendances](xref:fundamentals/dependency-injection) (DI) en tant qu’instance.
* L’inscription du `ObjectPoolProvider<>` dans l’injection de dépendances et l’utiliser comme une fabrique.

## <a name="how-to-use-objectpool"></a>Comment utiliser le pool de threads

Appelez <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> pour obtenir un objet et <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1.Return*> pour retourner l’objet.  N’est pas obligatoire que renvoie chaque objet. Si vous ne retournez un objet, il sera par le garbage collecté.

## <a name="objectpool-sample"></a>Exemple de pool de threads

L'exemple de code suivant :

* Ajoute `ObjectPoolProvider` à la [l’injection de dépendances](xref:fundamentals/dependency-injection) (DI) conteneur.
* Ajoute et configure `ObjectPool<StringBuilder>` vers le conteneur d’injection de dépendances.
* Ajoute le `BirthdayMiddleware`.

[!code-csharp[](ObjectPool/ObjectPoolSample/Startup.cs?name=snippet)]

Le code suivant implémente `BirthdayMiddleware`

[!code-csharp[](ObjectPool/ObjectPoolSample/BirthdayMiddleware.cs?name=snippet)]
