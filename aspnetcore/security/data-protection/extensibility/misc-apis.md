---
title: API de Protection de données divers ASP.NET Core
author: rick-anderson
description: En savoir plus sur l’interface ISecret de Protection de données de base ASP.NET.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 484c6a0979a10e7cf2b801873655caa99a42532c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
ms.locfileid: "30072762"
---
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a><span data-ttu-id="bedc5-103">API de Protection de données divers ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bedc5-103">Miscellaneous ASP.NET Core Data Protection APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="bedc5-104">Les types qui implémentent des interfaces suivantes doivent être thread-safe pour les appelants plusieurs.</span><span class="sxs-lookup"><span data-stu-id="bedc5-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="bedc5-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="bedc5-105">ISecret</span></span>

<span data-ttu-id="bedc5-106">Le `ISecret` interface représente une valeur secrète, tels que du matériel de clé de chiffrement.</span><span class="sxs-lookup"><span data-stu-id="bedc5-106">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="bedc5-107">Elle contient la surface API suivante :</span><span class="sxs-lookup"><span data-stu-id="bedc5-107">It contains the following API surface:</span></span>

* <span data-ttu-id="bedc5-108">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="bedc5-108">`Length`: `int`</span></span>

* <span data-ttu-id="bedc5-109">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="bedc5-109">`Dispose()`: `void`</span></span>

* <span data-ttu-id="bedc5-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="bedc5-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="bedc5-111">Le `WriteSecretIntoBuffer` méthode remplit la mémoire tampon fournie avec la valeur brute de secret principal.</span><span class="sxs-lookup"><span data-stu-id="bedc5-111">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="bedc5-112">La raison pour laquelle cette API prend la mémoire tampon en tant que paramètre plutôt que de retourner un `byte[]` directement est que cela permet l’appelant pour épingler l’objet de la mémoire tampon, limiter l’exposition de secret principal pour le RÉCUPÉRATEUR de mémoire managée.</span><span class="sxs-lookup"><span data-stu-id="bedc5-112">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="bedc5-113">Le `Secret` type est une implémentation concrète de `ISecret` où la valeur secrète est stockée dans la mémoire d’en cours.</span><span class="sxs-lookup"><span data-stu-id="bedc5-113">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="bedc5-114">Sur les plateformes Windows, la valeur de clé secrète est chiffrée [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="bedc5-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
