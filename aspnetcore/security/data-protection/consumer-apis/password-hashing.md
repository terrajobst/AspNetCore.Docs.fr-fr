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
ms.openlocfilehash: f44e66789bf348ef6d99f6d862fb34c2d943a0b2
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="hash-passwords-in-aspnet-core"></a>Hachage des mots de passe dans ASP.NET Core

La base de code de protection de données inclut un package *Microsoft.AspNetCore.Cryptography.KeyDerivation* qui contient les fonctions de dérivation de clé de chiffrement. Ce package est un composant autonome et n’a aucune dépendance sur le reste du système de protection des données. Il peut être utilisé totalement indépendants. La source existe en même temps que le code de protection des données base pour des raisons pratiques.

Le package actuellement propose une méthode `KeyDerivation.Pbkdf2` qui permet de hachage d’un mot de passe à l’aide de la [PBKDF2 algorithme](https://tools.ietf.org/html/rfc2898#section-5.2). Cette API méthode est très similaire à .NET Framework existant [Rfc2898DeriveBytes type](/dotnet/api/system.security.cryptography.rfc2898derivebytes), mais il existe trois des différences importantes :

1. Le `KeyDerivation.Pbkdf2` méthode prend en charge la consommation de plusieurs PRF (actuellement `HMACSHA1`, `HMACSHA256`, et `HMACSHA512`), alors que le `Rfc2898DeriveBytes` prend uniquement en charge de type `HMACSHA1`.

2. Le `KeyDerivation.Pbkdf2` méthode détecte le système d’exploitation actuel et tente de choisir l’implémentation plus optimisée de la routine, en fournissant de bien meilleures performances dans certains cas. (Sous Windows 8, il offre environ 10 x le débit de `Rfc2898DeriveBytes`.)

3. Le `KeyDerivation.Pbkdf2` méthode exige de l’appelant spécifier tous les paramètres (SEL, PRF et nombre d’itérations). Le `Rfc2898DeriveBytes` type fournit les valeurs par défaut pour ces.

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

Consultez le code source pour ASP.NET Core Identity `PasswordHasher` cas d’utilisation de type pour un monde réel.
