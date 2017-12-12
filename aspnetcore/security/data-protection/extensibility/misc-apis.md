---
title: API diverses
author: rick-anderson
description: "Ce document décrit l’interface de ISecret de protection de données ASP.NET Core."
keywords: "ASP.NET Core, protection des données, ISecret"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 512c6ba7-88ec-47e4-a656-6b30350b34e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: f5d6920f9f229bd480a76c952dab30efb7d9eff5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
# <a name="miscellaneous-apis"></a><span data-ttu-id="02cf3-104">API diverses</span><span class="sxs-lookup"><span data-stu-id="02cf3-104">Miscellaneous APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="02cf3-105">Les types qui implémentent des interfaces suivantes doivent être thread-safe pour les appelants plusieurs.</span><span class="sxs-lookup"><span data-stu-id="02cf3-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="02cf3-106">ISecret</span><span class="sxs-lookup"><span data-stu-id="02cf3-106">ISecret</span></span>

<span data-ttu-id="02cf3-107">Le `ISecret` interface représente une valeur secrète, tels que du matériel de clé de chiffrement.</span><span class="sxs-lookup"><span data-stu-id="02cf3-107">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="02cf3-108">Elle contient la surface API suivante :</span><span class="sxs-lookup"><span data-stu-id="02cf3-108">It contains the following API surface:</span></span>

* <span data-ttu-id="02cf3-109">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="02cf3-109">`Length`: `int`</span></span>

* <span data-ttu-id="02cf3-110">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="02cf3-110">`Dispose()`: `void`</span></span>

* <span data-ttu-id="02cf3-111">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="02cf3-111">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="02cf3-112">Le `WriteSecretIntoBuffer` méthode remplit la mémoire tampon fournie avec la valeur brute de secret principal.</span><span class="sxs-lookup"><span data-stu-id="02cf3-112">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="02cf3-113">La raison pour laquelle cette API prend la mémoire tampon en tant que paramètre plutôt que de retourner un `byte[]` directement est que cela permet l’appelant pour épingler l’objet de la mémoire tampon, limiter l’exposition de secret principal pour le RÉCUPÉRATEUR de mémoire managée.</span><span class="sxs-lookup"><span data-stu-id="02cf3-113">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="02cf3-114">Le `Secret` type est une implémentation concrète de `ISecret` où la valeur secrète est stockée dans la mémoire d’en cours.</span><span class="sxs-lookup"><span data-stu-id="02cf3-114">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="02cf3-115">Sur les plateformes Windows, la valeur de clé secrète est chiffrée [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="02cf3-115">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
