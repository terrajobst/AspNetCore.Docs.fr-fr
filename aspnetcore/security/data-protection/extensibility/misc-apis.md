---
title: API de protection des données diverses ASP.NET Core
author: rick-anderson
description: En savoir plus sur l’interface ISecret de protection des données ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 114cdd6209970e46b827e403fbe79b95692d0242
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666081"
---
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a>API de protection des données diverses ASP.NET Core

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> Types qui implémentent les interfaces suivantes doivent être thread-safe pour les appelants plusieurs.

## <a name="isecret"></a>ISecret

L’interface `ISecret` représente une valeur secrète, telle que le matériel de clé de chiffrement. Il contient la surface de l’API suivante :

* `Length`: `int`

* `Dispose()`: `void`

* `WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`

La méthode `WriteSecretIntoBuffer` remplit la mémoire tampon fournie avec la valeur de secret brut. La raison pour laquelle cette API prend la mémoire tampon comme paramètre plutôt que de retourner directement une `byte[]` est que cela donne à l’appelant la possibilité d’épingler l’objet de mémoire tampon, ce qui limite l’exposition secrète au garbage collector géré.

Le type de `Secret` est une implémentation concrète de `ISecret` où la valeur secrète est stockée dans la mémoire in-process. Sur les plateformes Windows, la valeur de la clé secrète est chiffrée via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).
