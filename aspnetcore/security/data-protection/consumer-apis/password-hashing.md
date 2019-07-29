---
title: Hacher les mots de passe dans ASP.NET Core
author: rick-anderson
description: Découvrez comment hacher les mots de passe à l’aide des API de protection des données ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: bd4b8fcf6a5a16a86ada97bbd3519f872d1417b7
ms.sourcegitcommit: 0efb9e219fef481dee35f7b763165e488aa6cf9c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68602457"
---
# <a name="hash-passwords-in-aspnet-core"></a><span data-ttu-id="90179-103">Hacher les mots de passe dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="90179-103">Hash passwords in ASP.NET Core</span></span>

<span data-ttu-id="90179-104">La base de code de protection des données inclut un package *Microsoft. AspNetCore. Cryptography. keydérivation* qui contient des fonctions de dérivation de clé de chiffrement.</span><span class="sxs-lookup"><span data-stu-id="90179-104">The data protection code base includes a package *Microsoft.AspNetCore.Cryptography.KeyDerivation* which contains cryptographic key derivation functions.</span></span> <span data-ttu-id="90179-105">Ce package est un composant autonome et n’a aucune dépendance vis-à-vis du reste du système de protection des données.</span><span class="sxs-lookup"><span data-stu-id="90179-105">This package is a standalone component and has no dependencies on the rest of the data protection system.</span></span> <span data-ttu-id="90179-106">Il peut être utilisé entièrement indépendamment.</span><span class="sxs-lookup"><span data-stu-id="90179-106">It can be used completely independently.</span></span> <span data-ttu-id="90179-107">La source existe avec la base de code de protection des données pour des raisons pratiques.</span><span class="sxs-lookup"><span data-stu-id="90179-107">The source exists alongside the data protection code base as a convenience.</span></span>

<span data-ttu-id="90179-108">Le package offre actuellement une méthode `KeyDerivation.Pbkdf2` qui autorise le hachage d’un mot de passe à l’aide de l' [algorithme PBKDF2](https://tools.ietf.org/html/rfc2898#section-5.2).</span><span class="sxs-lookup"><span data-stu-id="90179-108">The package currently offers a method `KeyDerivation.Pbkdf2` which allows hashing a password using the [PBKDF2 algorithm](https://tools.ietf.org/html/rfc2898#section-5.2).</span></span> <span data-ttu-id="90179-109">Cette API est très similaire au [type Rfc2898DeriveBytes](/dotnet/api/system.security.cryptography.rfc2898derivebytes)existant de .NET Framework, mais il existe trois différences importantes:</span><span class="sxs-lookup"><span data-stu-id="90179-109">This API is very similar to the .NET Framework's existing [Rfc2898DeriveBytes type](/dotnet/api/system.security.cryptography.rfc2898derivebytes), but there are three important distinctions:</span></span>

1. <span data-ttu-id="90179-110">La `KeyDerivation.Pbkdf2` méthode prend en charge l’utilisation de plusieurs `HMACSHA1`PRFs `HMACSHA256`(actuellement `HMACSHA512`, et), `Rfc2898DeriveBytes` tandis que `HMACSHA1`le type prend en charge uniquement.</span><span class="sxs-lookup"><span data-stu-id="90179-110">The `KeyDerivation.Pbkdf2` method supports consuming multiple PRFs (currently `HMACSHA1`, `HMACSHA256`, and `HMACSHA512`), whereas the `Rfc2898DeriveBytes` type only supports `HMACSHA1`.</span></span>

2. <span data-ttu-id="90179-111">La `KeyDerivation.Pbkdf2` méthode détecte le système d’exploitation actuel et tente de choisir l’implémentation la plus optimisée de la routine, offrant ainsi de meilleures performances dans certains cas.</span><span class="sxs-lookup"><span data-stu-id="90179-111">The `KeyDerivation.Pbkdf2` method detects the current operating system and attempts to choose the most optimized implementation of the routine, providing much better performance in certain cases.</span></span> <span data-ttu-id="90179-112">(Sur Windows 8, il offre environ 10 fois plus de `Rfc2898DeriveBytes`débit.)</span><span class="sxs-lookup"><span data-stu-id="90179-112">(On Windows 8, it offers around 10x the throughput of `Rfc2898DeriveBytes`.)</span></span>

3. <span data-ttu-id="90179-113">La `KeyDerivation.Pbkdf2` méthode impose à l’appelant de spécifier tous les paramètres (Salt, PRF et nombre d’itérations).</span><span class="sxs-lookup"><span data-stu-id="90179-113">The `KeyDerivation.Pbkdf2` method requires the caller to specify all parameters (salt, PRF, and iteration count).</span></span> <span data-ttu-id="90179-114">Le `Rfc2898DeriveBytes` type fournit des valeurs par défaut pour ces.</span><span class="sxs-lookup"><span data-stu-id="90179-114">The `Rfc2898DeriveBytes` type provides default values for these.</span></span>

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

<span data-ttu-id="90179-115">Consultez le [code source](https://github.com/aspnet/AspNetCore/blob/master/src/Identity/Extensions.Core/src/PasswordHasher.cs) du `PasswordHasher` type de ASP.net Core identité pour un cas d’usage réel.</span><span class="sxs-lookup"><span data-stu-id="90179-115">See the [source code](https://github.com/aspnet/AspNetCore/blob/master/src/Identity/Extensions.Core/src/PasswordHasher.cs) for ASP.NET Core Identity's `PasswordHasher` type for a real-world use case.</span></span>
