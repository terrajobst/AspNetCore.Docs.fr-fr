---
title: API de Protection des données divers ASP.NET Core
author: rick-anderson
description: En savoir plus sur l’interface ISecret de Protection des données ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 114cdd6209970e46b827e403fbe79b95692d0242
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896616"
---
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a>API de Protection des données divers ASP.NET Core

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> Types qui implémentent les interfaces suivantes doivent être thread-safe pour les appelants plusieurs.

## <a name="isecret"></a>ISecret

Le `ISecret` interface représente une valeur secrète, comme clé de chiffrement. Il contient la surface d’API suivante :

* `Length`: `int`

* `Dispose()`: `void`

* `WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`

Le `WriteSecretIntoBuffer` méthode remplit la mémoire tampon fournie avec la valeur du secret brutes. La raison pour laquelle cette API prend la mémoire tampon en tant que paramètre au lieu de retourner un `byte[]` directement est que cela donne à l’appelant la possibilité d’épingler l’objet de mémoire tampon, en limitant l’exposition de secret pour le RÉCUPÉRATEUR de mémoire géré.

Le `Secret` type est une implémentation concrète de `ISecret` où la valeur du secret est stockée dans la mémoire de dans le processus. Sur les plateformes Windows, la valeur du secret est chiffrée [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).
