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
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a><span data-ttu-id="ac1d2-103">API de Protection des données divers ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ac1d2-103">Miscellaneous ASP.NET Core Data Protection APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="ac1d2-104">Types qui implémentent les interfaces suivantes doivent être thread-safe pour les appelants plusieurs.</span><span class="sxs-lookup"><span data-stu-id="ac1d2-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="ac1d2-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="ac1d2-105">ISecret</span></span>

<span data-ttu-id="ac1d2-106">Le `ISecret` interface représente une valeur secrète, comme clé de chiffrement.</span><span class="sxs-lookup"><span data-stu-id="ac1d2-106">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="ac1d2-107">Il contient la surface d’API suivante :</span><span class="sxs-lookup"><span data-stu-id="ac1d2-107">It contains the following API surface:</span></span>

* <span data-ttu-id="ac1d2-108">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="ac1d2-108">`Length`: `int`</span></span>

* <span data-ttu-id="ac1d2-109">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="ac1d2-109">`Dispose()`: `void`</span></span>

* <span data-ttu-id="ac1d2-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="ac1d2-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="ac1d2-111">Le `WriteSecretIntoBuffer` méthode remplit la mémoire tampon fournie avec la valeur du secret brutes.</span><span class="sxs-lookup"><span data-stu-id="ac1d2-111">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="ac1d2-112">La raison pour laquelle cette API prend la mémoire tampon en tant que paramètre au lieu de retourner un `byte[]` directement est que cela donne à l’appelant la possibilité d’épingler l’objet de mémoire tampon, en limitant l’exposition de secret pour le RÉCUPÉRATEUR de mémoire géré.</span><span class="sxs-lookup"><span data-stu-id="ac1d2-112">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="ac1d2-113">Le `Secret` type est une implémentation concrète de `ISecret` où la valeur du secret est stockée dans la mémoire de dans le processus.</span><span class="sxs-lookup"><span data-stu-id="ac1d2-113">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="ac1d2-114">Sur les plateformes Windows, la valeur du secret est chiffrée [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="ac1d2-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
