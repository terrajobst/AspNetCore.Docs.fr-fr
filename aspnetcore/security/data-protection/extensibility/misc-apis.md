---
title: API diverses
author: rick-anderson
description: "Ce document décrit l’interface de ISecret de protection de données ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 26474de3a1c681a8c7f8b7812e9ef859f5d448bb
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="miscellaneous-apis"></a><span data-ttu-id="2ebaa-103">API diverses</span><span class="sxs-lookup"><span data-stu-id="2ebaa-103">Miscellaneous APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="2ebaa-104">Les types qui implémentent des interfaces suivantes doivent être thread-safe pour les appelants plusieurs.</span><span class="sxs-lookup"><span data-stu-id="2ebaa-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="2ebaa-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="2ebaa-105">ISecret</span></span>

<span data-ttu-id="2ebaa-106">Le `ISecret` interface représente une valeur secrète, tels que du matériel de clé de chiffrement.</span><span class="sxs-lookup"><span data-stu-id="2ebaa-106">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="2ebaa-107">Elle contient la surface API suivante :</span><span class="sxs-lookup"><span data-stu-id="2ebaa-107">It contains the following API surface:</span></span>

* <span data-ttu-id="2ebaa-108">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="2ebaa-108">`Length`: `int`</span></span>

* <span data-ttu-id="2ebaa-109">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="2ebaa-109">`Dispose()`: `void`</span></span>

* <span data-ttu-id="2ebaa-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="2ebaa-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="2ebaa-111">Le `WriteSecretIntoBuffer` méthode remplit la mémoire tampon fournie avec la valeur brute de secret principal.</span><span class="sxs-lookup"><span data-stu-id="2ebaa-111">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="2ebaa-112">La raison pour laquelle cette API prend la mémoire tampon en tant que paramètre plutôt que de retourner un `byte[]` directement est que cela permet l’appelant pour épingler l’objet de la mémoire tampon, limiter l’exposition de secret principal pour le RÉCUPÉRATEUR de mémoire managée.</span><span class="sxs-lookup"><span data-stu-id="2ebaa-112">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="2ebaa-113">Le `Secret` type est une implémentation concrète de `ISecret` où la valeur secrète est stockée dans la mémoire d’en cours.</span><span class="sxs-lookup"><span data-stu-id="2ebaa-113">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="2ebaa-114">Sur les plateformes Windows, la valeur de clé secrète est chiffrée [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="2ebaa-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
