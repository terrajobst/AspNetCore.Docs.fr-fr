---
title: Hachage des mots de passe dans ASP.NET Core
author: rick-anderson
description: Découvrez comment hacher des mots de passe à l’aide de l’API de Protection des données ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: 882ac9b256b0cdf5fd19dc4bd2757cac7e8ecad3
ms.sourcegitcommit: a742b55e4b8276a48b8b4394784554fecd883c84
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45538373"
---
# <a name="hash-passwords-in-aspnet-core"></a><span data-ttu-id="34e68-103">Hachage des mots de passe dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="34e68-103">Hash passwords in ASP.NET Core</span></span>

<span data-ttu-id="34e68-104">La base de code de protection de données inclut un package *Microsoft.AspNetCore.Cryptography.KeyDerivation* qui contient les fonctions de dérivation de clé de chiffrement.</span><span class="sxs-lookup"><span data-stu-id="34e68-104">The data protection code base includes a package *Microsoft.AspNetCore.Cryptography.KeyDerivation* which contains cryptographic key derivation functions.</span></span> <span data-ttu-id="34e68-105">Ce package est un composant autonome et n’a aucune dépendance sur le reste du système de protection des données.</span><span class="sxs-lookup"><span data-stu-id="34e68-105">This package is a standalone component and has no dependencies on the rest of the data protection system.</span></span> <span data-ttu-id="34e68-106">Il peut être utilisé complètement indépendamment.</span><span class="sxs-lookup"><span data-stu-id="34e68-106">It can be used completely independently.</span></span> <span data-ttu-id="34e68-107">La source existe en même temps que le code de protection de données base pour des raisons pratiques.</span><span class="sxs-lookup"><span data-stu-id="34e68-107">The source exists alongside the data protection code base as a convenience.</span></span>

<span data-ttu-id="34e68-108">Le package offre actuellement une méthode `KeyDerivation.Pbkdf2` qui permet à un mot de passe à l’aide de hachage le [PBKDF2 algorithme](https://tools.ietf.org/html/rfc2898#section-5.2).</span><span class="sxs-lookup"><span data-stu-id="34e68-108">The package currently offers a method `KeyDerivation.Pbkdf2` which allows hashing a password using the [PBKDF2 algorithm](https://tools.ietf.org/html/rfc2898#section-5.2).</span></span> <span data-ttu-id="34e68-109">Cette API est très similaire à un élément existant du .NET Framework [Rfc2898DeriveBytes type](/dotnet/api/system.security.cryptography.rfc2898derivebytes), mais il existe trois des différences importantes :</span><span class="sxs-lookup"><span data-stu-id="34e68-109">This API is very similar to the .NET Framework's existing [Rfc2898DeriveBytes type](/dotnet/api/system.security.cryptography.rfc2898derivebytes), but there are three important distinctions:</span></span>

1. <span data-ttu-id="34e68-110">Le `KeyDerivation.Pbkdf2` méthode prend en charge la consommation plusieurs PRF (actuellement `HMACSHA1`, `HMACSHA256`, et `HMACSHA512`), tandis que le `Rfc2898DeriveBytes` type uniquement prend en charge `HMACSHA1`.</span><span class="sxs-lookup"><span data-stu-id="34e68-110">The `KeyDerivation.Pbkdf2` method supports consuming multiple PRFs (currently `HMACSHA1`, `HMACSHA256`, and `HMACSHA512`), whereas the `Rfc2898DeriveBytes` type only supports `HMACSHA1`.</span></span>

2. <span data-ttu-id="34e68-111">Le `KeyDerivation.Pbkdf2` méthode détecte le système d’exploitation actuel et essaie d’en choisir l’implémentation plus optimisée de la routine, offrant de bien meilleures performances dans certains cas.</span><span class="sxs-lookup"><span data-stu-id="34e68-111">The `KeyDerivation.Pbkdf2` method detects the current operating system and attempts to choose the most optimized implementation of the routine, providing much better performance in certain cases.</span></span> <span data-ttu-id="34e68-112">(Sur Windows 8, il offre environ 10 x le débit de `Rfc2898DeriveBytes`.)</span><span class="sxs-lookup"><span data-stu-id="34e68-112">(On Windows 8, it offers around 10x the throughput of `Rfc2898DeriveBytes`.)</span></span>

3. <span data-ttu-id="34e68-113">Le `KeyDerivation.Pbkdf2` méthode nécessite l’appelant de spécifier tous les paramètres (SEL, PRF et nombre d’itérations).</span><span class="sxs-lookup"><span data-stu-id="34e68-113">The `KeyDerivation.Pbkdf2` method requires the caller to specify all parameters (salt, PRF, and iteration count).</span></span> <span data-ttu-id="34e68-114">Le `Rfc2898DeriveBytes` type fournit les valeurs par défaut pour ces.</span><span class="sxs-lookup"><span data-stu-id="34e68-114">The `Rfc2898DeriveBytes` type provides default values for these.</span></span>

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

<span data-ttu-id="34e68-115">Consultez le [code source] (https://github.com/aspnet/Identity/blob/master/src/Core/PasswordHasher.cs) pour ASP.NET Core Identity `PasswordHasher` cas d’usage de type pour un monde réel.</span><span class="sxs-lookup"><span data-stu-id="34e68-115">See the [source code]（https://github.com/aspnet/Identity/blob/master/src/Core/PasswordHasher.cs) for ASP.NET Core Identity's `PasswordHasher` type for a real-world use case.</span></span>
