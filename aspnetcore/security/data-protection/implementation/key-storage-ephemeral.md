---
title: Fournisseurs de protection des données éphémères dans ASP.NET Core
author: rick-anderson
description: Découvrez les détails d’implémentation des fournisseurs de protection des données éphémères ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: e4b0014ab3bdbf90b91383e8a33102f94faa8153
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64895176"
---
# <a name="ephemeral-data-protection-providers-in-aspnet-core"></a>Fournisseurs de protection des données éphémères dans ASP.NET Core

<a name="data-protection-implementation-key-storage-ephemeral"></a>

Il existe des scénarios où une application a besoin d’un throwaway `IDataProtectionProvider`. Par exemple, le développeur peut simplement vos procédures dans une application console One-Off, ou l’application elle-même est transitoire (il est écrit ou un test unitaire projet). Pour prendre en charge ces scénarios le [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) package inclut un type `EphemeralDataProtectionProvider`. Ce type fournit une implémentation de base de `IDataProtectionProvider` dont dépôt de clé est conservé en mémoire uniquement et n’est pas écrit dans n’importe quel magasin de stockage.

Chaque instance de `EphemeralDataProtectionProvider` utilise sa propre clé principale unique. Par conséquent, si un `IDataProtector` dont la racine est un `EphemeralDataProtectionProvider` génère une charge utile protégée, cette charge utile ne peut uniquement être protégée par un équivalent `IDataProtector` (étant donné le même [usage](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-consumer-apis-purposes) chaîne) dont la racine est le même `EphemeralDataProtectionProvider` instance.

L’exemple suivant montre comment instancier un `EphemeralDataProtectionProvider` et son utilisation pour protéger et déprotéger les données.

```csharp
using System;
using Microsoft.AspNetCore.DataProtection;

public class Program
{
    public static void Main(string[] args)
    {
        const string purpose = "Ephemeral.App.v1";

        // create an ephemeral provider and demonstrate that it can round-trip a payload
        var provider = new EphemeralDataProtectionProvider();
        var protector = provider.CreateProtector(purpose);
        Console.Write("Enter input: ");
        string input = Console.ReadLine();

        // protect the payload
        string protectedPayload = protector.Protect(input);
        Console.WriteLine($"Protect returned: {protectedPayload}");

        // unprotect the payload
        string unprotectedPayload = protector.Unprotect(protectedPayload);
        Console.WriteLine($"Unprotect returned: {unprotectedPayload}");

        // if I create a new ephemeral provider, it won't be able to unprotect existing
        // payloads, even if I specify the same purpose
        provider = new EphemeralDataProtectionProvider();
        protector = provider.CreateProtector(purpose);
        unprotectedPayload = protector.Unprotect(protectedPayload); // THROWS
    }
}

/*
* SAMPLE OUTPUT
*
* Enter input: Hello!
* Protect returned: CfDJ8AAAAAAAAAAAAAAAAAAAAA...uGoxWLjGKtm1SkNACQ
* Unprotect returned: Hello!
* << throws CryptographicException >>
*/
```
