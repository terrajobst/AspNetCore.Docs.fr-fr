---
title: Hachage des mots de passe dans ASP.NET Core
author: rick-anderson
description: En savoir plus sur le hachage des mots de passe à l’aide de l’API de Protection de données ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: 63348da144e84d614f274b5d816cbecb020dcab4
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="hash-passwords-in-aspnet-core"></a><span data-ttu-id="a0cba-103">Hachage des mots de passe dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a0cba-103">Hash passwords in ASP.NET Core</span></span>

<span data-ttu-id="a0cba-104">La base de code de protection de données inclut un package *Microsoft.AspNetCore.Cryptography.KeyDerivation* qui contient les fonctions de dérivation de clé de chiffrement.</span><span class="sxs-lookup"><span data-stu-id="a0cba-104">The data protection code base includes a package *Microsoft.AspNetCore.Cryptography.KeyDerivation* which contains cryptographic key derivation functions.</span></span> <span data-ttu-id="a0cba-105">Ce package est un composant autonome et n’a aucune dépendance sur le reste du système de protection des données.</span><span class="sxs-lookup"><span data-stu-id="a0cba-105">This package is a standalone component and has no dependencies on the rest of the data protection system.</span></span> <span data-ttu-id="a0cba-106">Il peut être utilisé totalement indépendants.</span><span class="sxs-lookup"><span data-stu-id="a0cba-106">It can be used completely independently.</span></span> <span data-ttu-id="a0cba-107">La source existe en même temps que le code de protection des données base pour des raisons pratiques.</span><span class="sxs-lookup"><span data-stu-id="a0cba-107">The source exists alongside the data protection code base as a convenience.</span></span>

<span data-ttu-id="a0cba-108">Le package actuellement propose une méthode `KeyDerivation.Pbkdf2` qui permet de hachage d’un mot de passe à l’aide de la [PBKDF2 algorithme](https://tools.ietf.org/html/rfc2898#section-5.2).</span><span class="sxs-lookup"><span data-stu-id="a0cba-108">The package currently offers a method `KeyDerivation.Pbkdf2` which allows hashing a password using the [PBKDF2 algorithm](https://tools.ietf.org/html/rfc2898#section-5.2).</span></span> <span data-ttu-id="a0cba-109">Cette API méthode est très similaire à .NET Framework existant [Rfc2898DeriveBytes type](https://docs.microsoft.com/dotnet/api/system.security.cryptography.rfc2898derivebytes), mais il existe trois des différences importantes :</span><span class="sxs-lookup"><span data-stu-id="a0cba-109">This API is very similar to the .NET Framework's existing [Rfc2898DeriveBytes type](https://docs.microsoft.com/dotnet/api/system.security.cryptography.rfc2898derivebytes), but there are three important distinctions:</span></span>

1. <span data-ttu-id="a0cba-110">Le `KeyDerivation.Pbkdf2` méthode prend en charge la consommation de plusieurs PRF (actuellement `HMACSHA1`, `HMACSHA256`, et `HMACSHA512`), alors que le `Rfc2898DeriveBytes` prend uniquement en charge de type `HMACSHA1`.</span><span class="sxs-lookup"><span data-stu-id="a0cba-110">The `KeyDerivation.Pbkdf2` method supports consuming multiple PRFs (currently `HMACSHA1`, `HMACSHA256`, and `HMACSHA512`), whereas the `Rfc2898DeriveBytes` type only supports `HMACSHA1`.</span></span>

2. <span data-ttu-id="a0cba-111">Le `KeyDerivation.Pbkdf2` méthode détecte le système d’exploitation actuel et tente de choisir l’implémentation plus optimisée de la routine, en fournissant de bien meilleures performances dans certains cas.</span><span class="sxs-lookup"><span data-stu-id="a0cba-111">The `KeyDerivation.Pbkdf2` method detects the current operating system and attempts to choose the most optimized implementation of the routine, providing much better performance in certain cases.</span></span> <span data-ttu-id="a0cba-112">(Sous Windows 8, il offre environ 10 x le débit de `Rfc2898DeriveBytes`.)</span><span class="sxs-lookup"><span data-stu-id="a0cba-112">(On Windows 8, it offers around 10x the throughput of `Rfc2898DeriveBytes`.)</span></span>

3. <span data-ttu-id="a0cba-113">Le `KeyDerivation.Pbkdf2` méthode exige de l’appelant spécifier tous les paramètres (SEL, PRF et nombre d’itérations).</span><span class="sxs-lookup"><span data-stu-id="a0cba-113">The `KeyDerivation.Pbkdf2` method requires the caller to specify all parameters (salt, PRF, and iteration count).</span></span> <span data-ttu-id="a0cba-114">Le `Rfc2898DeriveBytes` type fournit les valeurs par défaut pour ces.</span><span class="sxs-lookup"><span data-stu-id="a0cba-114">The `Rfc2898DeriveBytes` type provides default values for these.</span></span>

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

<span data-ttu-id="a0cba-115">Consultez le code source pour ASP.NET Core Identity `PasswordHasher` cas d’utilisation de type pour un monde réel.</span><span class="sxs-lookup"><span data-stu-id="a0cba-115">See the source code for ASP.NET Core Identity's `PasswordHasher` type for a real-world use case.</span></span>
