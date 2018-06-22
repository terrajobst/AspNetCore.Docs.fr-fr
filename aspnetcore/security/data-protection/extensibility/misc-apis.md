---
title: API de Protection de données divers ASP.NET Core
author: rick-anderson
description: En savoir plus sur l’interface ISecret de Protection de données de base ASP.NET.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 114cdd6209970e46b827e403fbe79b95692d0242
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279153"
---
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a>API de Protection de données divers ASP.NET Core

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> Les types qui implémentent des interfaces suivantes doivent être thread-safe pour les appelants plusieurs.

## <a name="isecret"></a>ISecret

Le `ISecret` interface représente une valeur secrète, tels que du matériel de clé de chiffrement. Elle contient la surface API suivante :

* `Length`: `int`

* `Dispose()`: `void`

* `WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`

Le `WriteSecretIntoBuffer` méthode remplit la mémoire tampon fournie avec la valeur brute de secret principal. La raison pour laquelle cette API prend la mémoire tampon en tant que paramètre plutôt que de retourner un `byte[]` directement est que cela permet l’appelant pour épingler l’objet de la mémoire tampon, limiter l’exposition de secret principal pour le RÉCUPÉRATEUR de mémoire managée.

Le `Secret` type est une implémentation concrète de `ISecret` où la valeur secrète est stockée dans la mémoire d’en cours. Sur les plateformes Windows, la valeur de clé secrète est chiffrée [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).
