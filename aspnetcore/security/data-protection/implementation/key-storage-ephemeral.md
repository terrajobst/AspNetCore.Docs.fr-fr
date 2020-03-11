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
# <a name="ephemeral-data-protection-providers-in-aspnet-core"></a><span data-ttu-id="e01f4-103">Fournisseurs de protection des données éphémères dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e01f4-103">Ephemeral data protection providers in ASP.NET Core</span></span>

<a name="data-protection-implementation-key-storage-ephemeral"></a>

<span data-ttu-id="e01f4-104">Il existe des scénarios dans lesquels une application a besoin d’un `IDataProtectionProvider`inepte.</span><span class="sxs-lookup"><span data-stu-id="e01f4-104">There are scenarios where an application needs a throwaway `IDataProtectionProvider`.</span></span> <span data-ttu-id="e01f4-105">Par exemple, le développeur peut simplement expérimenter une application console unique, ou l’application elle-même est temporaire (il s’agit d’un script ou d’un projet de test unitaire).</span><span class="sxs-lookup"><span data-stu-id="e01f4-105">For example, the developer might just be experimenting in a one-off console application, or the application itself is transient (it's scripted or a unit test project).</span></span> <span data-ttu-id="e01f4-106">Pour prendre en charge ces scénarios, le package [Microsoft. AspNetCore. dataprotection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) comprend un type `EphemeralDataProtectionProvider`.</span><span class="sxs-lookup"><span data-stu-id="e01f4-106">To support these scenarios the [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) package includes a type `EphemeralDataProtectionProvider`.</span></span> <span data-ttu-id="e01f4-107">Ce type fournit une implémentation de base de `IDataProtectionProvider` dont le dépôt de clé est conservé uniquement en mémoire et n’est pas écrit dans un magasin de stockage.</span><span class="sxs-lookup"><span data-stu-id="e01f4-107">This type provides a basic implementation of `IDataProtectionProvider` whose key repository is held solely in-memory and isn't written out to any backing store.</span></span>

<span data-ttu-id="e01f4-108">Chaque instance de `EphemeralDataProtectionProvider` utilise sa propre clé principale unique.</span><span class="sxs-lookup"><span data-stu-id="e01f4-108">Each instance of `EphemeralDataProtectionProvider` uses its own unique master key.</span></span> <span data-ttu-id="e01f4-109">Par conséquent, si une `IDataProtector` enracinée dans une `EphemeralDataProtectionProvider` génère une charge utile protégée, cette charge peut uniquement être non protégée par une `IDataProtector` équivalente (à partir de la même chaîne d' [objectif](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-consumer-apis-purposes) ) enracinée dans la même instance de `EphemeralDataProtectionProvider`.</span><span class="sxs-lookup"><span data-stu-id="e01f4-109">Therefore, if an `IDataProtector` rooted at an `EphemeralDataProtectionProvider` generates a protected payload, that payload can only be unprotected by an equivalent `IDataProtector` (given the same [purpose](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-consumer-apis-purposes) chain) rooted at the same `EphemeralDataProtectionProvider` instance.</span></span>

<span data-ttu-id="e01f4-110">L’exemple suivant illustre l’instanciation d’un `EphemeralDataProtectionProvider` et son utilisation pour protéger et ôter la protection des données.</span><span class="sxs-lookup"><span data-stu-id="e01f4-110">The following sample demonstrates instantiating an `EphemeralDataProtectionProvider` and using it to protect and unprotect data.</span></span>

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
