---
title: Fournisseurs de protection des données éphémères dans ASP.NET Core
author: rick-anderson
description: Découvrez les détails de l’implémentation des fournisseurs de protection des données ASP.NET Core éphémères.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: e4b0014ab3bdbf90b91383e8a33102f94faa8153
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664737"
---
# <a name="ephemeral-data-protection-providers-in-aspnet-core"></a>Fournisseurs de protection des données éphémères dans ASP.NET Core

<a name="data-protection-implementation-key-storage-ephemeral"></a>

Il existe des scénarios dans lesquels une application a besoin d’un `IDataProtectionProvider`inepte. Par exemple, le développeur peut simplement expérimenter une application console unique, ou l’application elle-même est temporaire (il s’agit d’un script ou d’un projet de test unitaire). Pour prendre en charge ces scénarios, le package [Microsoft. AspNetCore. dataprotection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) comprend un type `EphemeralDataProtectionProvider`. Ce type fournit une implémentation de base de `IDataProtectionProvider` dont le dépôt de clé est conservé uniquement en mémoire et n’est pas écrit dans un magasin de stockage.

Chaque instance de `EphemeralDataProtectionProvider` utilise sa propre clé principale unique. Par conséquent, si une `IDataProtector` enracinée dans une `EphemeralDataProtectionProvider` génère une charge utile protégée, cette charge peut uniquement être non protégée par une `IDataProtector` équivalente (à partir de la même chaîne d' [objectif](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-consumer-apis-purposes) ) enracinée dans la même instance de `EphemeralDataProtectionProvider`.

L’exemple suivant illustre l’instanciation d’un `EphemeralDataProtectionProvider` et son utilisation pour protéger et ôter la protection des données.

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
