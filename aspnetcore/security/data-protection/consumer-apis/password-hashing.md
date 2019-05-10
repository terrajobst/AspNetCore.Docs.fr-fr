---
title: Hachage des mots de passe dans ASP.NET Core
author: rick-anderson
description: Découvrez comment hacher des mots de passe à l’aide de l’API de Protection des données ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: 70301ffffbaaf3c5ff0642b19b80e40be83aa438
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896646"
---
# <a name="hash-passwords-in-aspnet-core"></a>Hachage des mots de passe dans ASP.NET Core

La base de code de protection de données inclut un package *Microsoft.AspNetCore.Cryptography.KeyDerivation* qui contient les fonctions de dérivation de clé de chiffrement. Ce package est un composant autonome et n’a aucune dépendance sur le reste du système de protection des données. Il peut être utilisé complètement indépendamment. La source existe en même temps que le code de protection de données base pour des raisons pratiques.

Le package offre actuellement une méthode `KeyDerivation.Pbkdf2` qui permet à un mot de passe à l’aide de hachage le [PBKDF2 algorithme](https://tools.ietf.org/html/rfc2898#section-5.2). Cette API est très similaire à un élément existant du .NET Framework [Rfc2898DeriveBytes type](/dotnet/api/system.security.cryptography.rfc2898derivebytes), mais il existe trois des différences importantes :

1. Le `KeyDerivation.Pbkdf2` méthode prend en charge la consommation plusieurs PRF (actuellement `HMACSHA1`, `HMACSHA256`, et `HMACSHA512`), tandis que le `Rfc2898DeriveBytes` type uniquement prend en charge `HMACSHA1`.

2. Le `KeyDerivation.Pbkdf2` méthode détecte le système d’exploitation actuel et essaie d’en choisir l’implémentation plus optimisée de la routine, offrant de bien meilleures performances dans certains cas. (Sur Windows 8, il offre environ 10 x le débit de `Rfc2898DeriveBytes`.)

3. Le `KeyDerivation.Pbkdf2` méthode nécessite l’appelant de spécifier tous les paramètres (SEL, PRF et nombre d’itérations). Le `Rfc2898DeriveBytes` type fournit les valeurs par défaut pour ces.

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

Consultez le [code source](https://github.com/aspnet/Identity/blob/master/src/Core/PasswordHasher.cs) pour ASP.NET Core Identity `PasswordHasher` cas d’usage de type pour un monde réel.
