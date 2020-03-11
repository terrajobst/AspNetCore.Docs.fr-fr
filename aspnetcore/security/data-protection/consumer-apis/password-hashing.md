---
title: Hacher les mots de passe dans ASP.NET Core
author: rick-anderson
description: Découvrez comment hacher les mots de passe à l’aide des API de protection des données ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: 52532f9f4d7f9037609c8273deb8b6964d81c714
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78656050"
---
# <a name="hash-passwords-in-aspnet-core"></a><span data-ttu-id="610bb-103">Hacher les mots de passe dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="610bb-103">Hash passwords in ASP.NET Core</span></span>

<span data-ttu-id="610bb-104">La base de code de protection des données inclut un package *Microsoft. AspNetCore. Cryptography. keydérivation* qui contient des fonctions de dérivation de clé de chiffrement.</span><span class="sxs-lookup"><span data-stu-id="610bb-104">The data protection code base includes a package *Microsoft.AspNetCore.Cryptography.KeyDerivation* which contains cryptographic key derivation functions.</span></span> <span data-ttu-id="610bb-105">Ce package est un composant autonome et n’a aucune dépendance vis-à-vis du reste du système de protection des données.</span><span class="sxs-lookup"><span data-stu-id="610bb-105">This package is a standalone component and has no dependencies on the rest of the data protection system.</span></span> <span data-ttu-id="610bb-106">Il peut être utilisé entièrement indépendamment.</span><span class="sxs-lookup"><span data-stu-id="610bb-106">It can be used completely independently.</span></span> <span data-ttu-id="610bb-107">La source existe avec la base de code de protection des données pour des raisons pratiques.</span><span class="sxs-lookup"><span data-stu-id="610bb-107">The source exists alongside the data protection code base as a convenience.</span></span>

<span data-ttu-id="610bb-108">Le package offre actuellement une méthode `KeyDerivation.Pbkdf2` qui autorise le hachage d’un mot de passe à l’aide de l' [algorithme PBKDF2](https://tools.ietf.org/html/rfc2898#section-5.2).</span><span class="sxs-lookup"><span data-stu-id="610bb-108">The package currently offers a method `KeyDerivation.Pbkdf2` which allows hashing a password using the [PBKDF2 algorithm](https://tools.ietf.org/html/rfc2898#section-5.2).</span></span> <span data-ttu-id="610bb-109">Cette API est très similaire au [type Rfc2898DeriveBytes](/dotnet/api/system.security.cryptography.rfc2898derivebytes)existant de .NET Framework, mais il existe trois différences importantes :</span><span class="sxs-lookup"><span data-stu-id="610bb-109">This API is very similar to the .NET Framework's existing [Rfc2898DeriveBytes type](/dotnet/api/system.security.cryptography.rfc2898derivebytes), but there are three important distinctions:</span></span>

1. <span data-ttu-id="610bb-110">La méthode `KeyDerivation.Pbkdf2` prend en charge la consommation de plusieurs PRFs (actuellement `HMACSHA1`, `HMACSHA256`et `HMACSHA512`), tandis que le type `Rfc2898DeriveBytes` prend uniquement en charge `HMACSHA1`.</span><span class="sxs-lookup"><span data-stu-id="610bb-110">The `KeyDerivation.Pbkdf2` method supports consuming multiple PRFs (currently `HMACSHA1`, `HMACSHA256`, and `HMACSHA512`), whereas the `Rfc2898DeriveBytes` type only supports `HMACSHA1`.</span></span>

2. <span data-ttu-id="610bb-111">La méthode `KeyDerivation.Pbkdf2` détecte le système d’exploitation actuel et tente de choisir l’implémentation la plus optimisée de la routine, offrant ainsi de meilleures performances dans certains cas.</span><span class="sxs-lookup"><span data-stu-id="610bb-111">The `KeyDerivation.Pbkdf2` method detects the current operating system and attempts to choose the most optimized implementation of the routine, providing much better performance in certain cases.</span></span> <span data-ttu-id="610bb-112">(Sur Windows 8, il offre environ 10 fois plus de débit de `Rfc2898DeriveBytes`.)</span><span class="sxs-lookup"><span data-stu-id="610bb-112">(On Windows 8, it offers around 10x the throughput of `Rfc2898DeriveBytes`.)</span></span>

3. <span data-ttu-id="610bb-113">La méthode `KeyDerivation.Pbkdf2` oblige l’appelant à spécifier tous les paramètres (Salt, PRF et nombre d’itérations).</span><span class="sxs-lookup"><span data-stu-id="610bb-113">The `KeyDerivation.Pbkdf2` method requires the caller to specify all parameters (salt, PRF, and iteration count).</span></span> <span data-ttu-id="610bb-114">Le type de `Rfc2898DeriveBytes` fournit des valeurs par défaut pour ces.</span><span class="sxs-lookup"><span data-stu-id="610bb-114">The `Rfc2898DeriveBytes` type provides default values for these.</span></span>

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

<span data-ttu-id="610bb-115">Consultez le [code source](https://github.com/dotnet/AspNetCore/blob/master/src/Identity/Extensions.Core/src/PasswordHasher.cs) pour ASP.net Core type de `PasswordHasher` d’identité pour un cas d’usage réel.</span><span class="sxs-lookup"><span data-stu-id="610bb-115">See the [source code](https://github.com/dotnet/AspNetCore/blob/master/src/Identity/Extensions.Core/src/PasswordHasher.cs) for ASP.NET Core Identity's `PasswordHasher` type for a real-world use case.</span></span>
