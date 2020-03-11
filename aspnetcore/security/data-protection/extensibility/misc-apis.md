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
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a><span data-ttu-id="df4ca-103">API de protection des données diverses ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="df4ca-103">Miscellaneous ASP.NET Core Data Protection APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="df4ca-104">Types qui implémentent les interfaces suivantes doivent être thread-safe pour les appelants plusieurs.</span><span class="sxs-lookup"><span data-stu-id="df4ca-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="df4ca-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="df4ca-105">ISecret</span></span>

<span data-ttu-id="df4ca-106">L’interface `ISecret` représente une valeur secrète, telle que le matériel de clé de chiffrement.</span><span class="sxs-lookup"><span data-stu-id="df4ca-106">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="df4ca-107">Il contient la surface de l’API suivante :</span><span class="sxs-lookup"><span data-stu-id="df4ca-107">It contains the following API surface:</span></span>

* <span data-ttu-id="df4ca-108">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="df4ca-108">`Length`: `int`</span></span>

* <span data-ttu-id="df4ca-109">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="df4ca-109">`Dispose()`: `void`</span></span>

* <span data-ttu-id="df4ca-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="df4ca-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="df4ca-111">La méthode `WriteSecretIntoBuffer` remplit la mémoire tampon fournie avec la valeur de secret brut.</span><span class="sxs-lookup"><span data-stu-id="df4ca-111">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="df4ca-112">La raison pour laquelle cette API prend la mémoire tampon comme paramètre plutôt que de retourner directement une `byte[]` est que cela donne à l’appelant la possibilité d’épingler l’objet de mémoire tampon, ce qui limite l’exposition secrète au garbage collector géré.</span><span class="sxs-lookup"><span data-stu-id="df4ca-112">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="df4ca-113">Le type de `Secret` est une implémentation concrète de `ISecret` où la valeur secrète est stockée dans la mémoire in-process.</span><span class="sxs-lookup"><span data-stu-id="df4ca-113">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="df4ca-114">Sur les plateformes Windows, la valeur de la clé secrète est chiffrée via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="df4ca-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
