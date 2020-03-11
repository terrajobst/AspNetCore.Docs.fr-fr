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
# <a name="hash-passwords-in-aspnet-core"></a>Hacher les mots de passe dans ASP.NET Core

La base de code de protection des données inclut un package *Microsoft. AspNetCore. Cryptography. keydérivation* qui contient des fonctions de dérivation de clé de chiffrement. Ce package est un composant autonome et n’a aucune dépendance vis-à-vis du reste du système de protection des données. Il peut être utilisé entièrement indépendamment. La source existe avec la base de code de protection des données pour des raisons pratiques.

Le package offre actuellement une méthode `KeyDerivation.Pbkdf2` qui autorise le hachage d’un mot de passe à l’aide de l' [algorithme PBKDF2](https://tools.ietf.org/html/rfc2898#section-5.2). Cette API est très similaire au [type Rfc2898DeriveBytes](/dotnet/api/system.security.cryptography.rfc2898derivebytes)existant de .NET Framework, mais il existe trois différences importantes :

1. La méthode `KeyDerivation.Pbkdf2` prend en charge la consommation de plusieurs PRFs (actuellement `HMACSHA1`, `HMACSHA256`et `HMACSHA512`), tandis que le type `Rfc2898DeriveBytes` prend uniquement en charge `HMACSHA1`.

2. La méthode `KeyDerivation.Pbkdf2` détecte le système d’exploitation actuel et tente de choisir l’implémentation la plus optimisée de la routine, offrant ainsi de meilleures performances dans certains cas. (Sur Windows 8, il offre environ 10 fois plus de débit de `Rfc2898DeriveBytes`.)

3. La méthode `KeyDerivation.Pbkdf2` oblige l’appelant à spécifier tous les paramètres (Salt, PRF et nombre d’itérations). Le type de `Rfc2898DeriveBytes` fournit des valeurs par défaut pour ces.

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

Consultez le [code source](https://github.com/dotnet/AspNetCore/blob/master/src/Identity/Extensions.Core/src/PasswordHasher.cs) pour ASP.net Core type de `PasswordHasher` d’identité pour un cas d’usage réel.
