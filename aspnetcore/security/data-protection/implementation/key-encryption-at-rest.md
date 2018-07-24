---
title: Clé de chiffrement au repos dans ASP.NET Core
author: rick-anderson
description: Découvrez les détails d’implémentation du chiffrement de clé de Protection des données ASP.NET Core au repos.
ms.author: riande
ms.date: 07/16/2018
uid: security/data-protection/implementation/key-encryption-at-rest
ms.openlocfilehash: 52c3137dbe467096364b42430c92aecc7c15e313
ms.sourcegitcommit: 8f8924ce4eb9effeaf489f177fb01b66867da16f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39219288"
---
# <a name="key-encryption-at-rest-in-aspnet-core"></a><span data-ttu-id="37326-103">Clé de chiffrement au repos dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="37326-103">Key encryption at rest in ASP.NET Core</span></span>

<span data-ttu-id="37326-104">Le système de protection des données [utilise un mécanisme de découverte par défaut](xref:security/data-protection/configuration/default-settings) pour déterminer les clés de chiffrement comment doivent être chiffrées au repos.</span><span class="sxs-lookup"><span data-stu-id="37326-104">The data protection system [employs a discovery mechanism by default](xref:security/data-protection/configuration/default-settings) to determine how cryptographic keys should be encrypted at rest.</span></span> <span data-ttu-id="37326-105">Le développeur peut remplacer le mécanisme de découverte et spécifier manuellement la façon dont les clés doivent être chiffrées au repos.</span><span class="sxs-lookup"><span data-stu-id="37326-105">The developer can override the discovery mechanism and manually specify how keys should be encrypted at rest.</span></span>

> [!WARNING]
> <span data-ttu-id="37326-106">Si vous spécifiez un texte explicite [emplacement de persistance de la clé](xref:security/data-protection/implementation/key-storage-providers), le système de protection des données annule l’inscription du chiffrement à clé par défaut au mécanisme de rest.</span><span class="sxs-lookup"><span data-stu-id="37326-106">If you specify an explicit [key persistence location](xref:security/data-protection/implementation/key-storage-providers), the data protection system deregisters the default key encryption at rest mechanism.</span></span> <span data-ttu-id="37326-107">Par conséquent, les clés ne sont plus chiffrés au repos.</span><span class="sxs-lookup"><span data-stu-id="37326-107">Consequently, keys are no longer encrypted at rest.</span></span> <span data-ttu-id="37326-108">Il est recommandé que vous [spécifier un mécanisme de chiffrement à clé explicite](xref:security/data-protection/implementation/key-encryption-at-rest) pour les déploiements de production.</span><span class="sxs-lookup"><span data-stu-id="37326-108">We recommend that you [specify an explicit key encryption mechanism](xref:security/data-protection/implementation/key-encryption-at-rest) for production deployments.</span></span> <span data-ttu-id="37326-109">Les options de mécanisme de chiffrement au repos sont décrits dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="37326-109">The encryption-at-rest mechanism options are described in this topic.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="azure-key-vault"></a><span data-ttu-id="37326-110">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="37326-110">Azure Key Vault</span></span>

<span data-ttu-id="37326-111">Pour stocker les clés dans [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configurer le système avec [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) dans la `Startup` classe :</span><span class="sxs-lookup"><span data-stu-id="37326-111">To store keys in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configure the system with [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) in the `Startup` class:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

<span data-ttu-id="37326-112">Pour plus d’informations, consultez [Protection des données configurer ASP.NET Core : ProtectKeysWithAzureKeyVault](xref:security/data-protection/configuration/overview#protectkeyswithazurekeyvault).</span><span class="sxs-lookup"><span data-stu-id="37326-112">For more information, see [Configure ASP.NET Core Data Protection: ProtectKeysWithAzureKeyVault](xref:security/data-protection/configuration/overview#protectkeyswithazurekeyvault).</span></span>

::: moniker-end

## <a name="windows-dpapi"></a><span data-ttu-id="37326-113">Windows DPAPI</span><span class="sxs-lookup"><span data-stu-id="37326-113">Windows DPAPI</span></span>

<span data-ttu-id="37326-114">**S’applique uniquement aux déploiements de Windows.**</span><span class="sxs-lookup"><span data-stu-id="37326-114">**Only applies to Windows deployments.**</span></span>

<span data-ttu-id="37326-115">Lorsque Windows DPAPI est utilisé, le matériel de clé est chiffré avec [CryptProtectData](/windows/desktop/api/dpapi/nf-dpapi-cryptprotectdata) avant d’être rendues persistantes dans le stockage.</span><span class="sxs-lookup"><span data-stu-id="37326-115">When Windows DPAPI is used, key material is encrypted with [CryptProtectData](/windows/desktop/api/dpapi/nf-dpapi-cryptprotectdata) before being persisted to storage.</span></span> <span data-ttu-id="37326-116">DPAPI est un mécanisme de chiffrement approprié pour les données qui ne sont jamais lue en dehors de l’ordinateur actuel (Cependant, il est possible de sauvegarder ces clés à Active Directory ; voir [DPAPI et les profils itinérants](https://support.microsoft.com/kb/309408/#6)).</span><span class="sxs-lookup"><span data-stu-id="37326-116">DPAPI is an appropriate encryption mechanism for data that's never read outside of the current machine (though it's possible to back these keys up to Active Directory; see [DPAPI and Roaming Profiles](https://support.microsoft.com/kb/309408/#6)).</span></span> <span data-ttu-id="37326-117">Pour configurer le chiffrement de clé au repos DPAPI, appelez une de la [ProtectKeysWithDpapi](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpapi) méthodes d’extension :</span><span class="sxs-lookup"><span data-stu-id="37326-117">To configure DPAPI key-at-rest encryption, call one of the [ProtectKeysWithDpapi](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpapi) extension methods:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Only the local user account can decrypt the keys
    services.AddDataProtection()
        .ProtectKeysWithDpapi();
}
```

<span data-ttu-id="37326-118">Si `ProtectKeysWithDpapi` est appelée sans paramètres, seul le compte d’utilisateur Windows actuel peut déchiffrer le key ring persistant.</span><span class="sxs-lookup"><span data-stu-id="37326-118">If `ProtectKeysWithDpapi` is called with no parameters, only the current Windows user account can decipher the persisted key ring.</span></span> <span data-ttu-id="37326-119">Vous pouvez éventuellement spécifier que n’importe quel compte d’utilisateur sur l’ordinateur (pas seulement le compte d’utilisateur actuel) être en mesure de déchiffrer le key ring :</span><span class="sxs-lookup"><span data-stu-id="37326-119">You can optionally specify that any user account on the machine (not just the current user account) be able to decipher the key ring:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // All user accounts on the machine can decrypt the keys
    services.AddDataProtection()
        .ProtectKeysWithDpapi(protectToLocalMachine: true);
}
```

::: moniker range=">= aspnetcore-2.0"

## <a name="x509-certificate"></a><span data-ttu-id="37326-120">Certificat X.509</span><span class="sxs-lookup"><span data-stu-id="37326-120">X.509 certificate</span></span>

<span data-ttu-id="37326-121">Si l’application est répartie sur plusieurs ordinateurs, il peut être pratique de distribuer un certificat X.509 partagé entre les machines et de configurer les applications hébergées pour utiliser le certificat pour le chiffrement des clés au repos :</span><span class="sxs-lookup"><span data-stu-id="37326-121">If the app is spread across multiple machines, it may be convenient to distribute a shared X.509 certificate across the machines and configure the hosted apps to use the certificate for encryption of keys at rest:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
}
```

<span data-ttu-id="37326-122">En raison des limitations de .NET Framework, seuls les certificats avec clés privées CAPI sont pris en charge.</span><span class="sxs-lookup"><span data-stu-id="37326-122">Due to .NET Framework limitations, only certificates with CAPI private keys are supported.</span></span> <span data-ttu-id="37326-123">Consultez le contenu ci-dessous pour les solutions possibles à ces limitations.</span><span class="sxs-lookup"><span data-stu-id="37326-123">See the content below for possible workarounds to these limitations.</span></span>

::: moniker-end

## <a name="windows-dpapi-ng"></a><span data-ttu-id="37326-124">Windows DPAPI-NG</span><span class="sxs-lookup"><span data-stu-id="37326-124">Windows DPAPI-NG</span></span>

<span data-ttu-id="37326-125">**Ce mécanisme est disponible uniquement sur Windows 8/Windows Server 2012 ou version ultérieure.**</span><span class="sxs-lookup"><span data-stu-id="37326-125">**This mechanism is available only on Windows 8/Windows Server 2012 or later.**</span></span>

<span data-ttu-id="37326-126">À compter de Windows 8, le système d’exploitation Windows prend en charge DPAPI-NG (également appelé CNG DPAPI).</span><span class="sxs-lookup"><span data-stu-id="37326-126">Beginning with Windows 8, Windows OS supports DPAPI-NG (also called CNG DPAPI).</span></span> <span data-ttu-id="37326-127">Pour plus d’informations, consultez [sur CNG DPAPI](/windows/desktop/SecCNG/cng-dpapi).</span><span class="sxs-lookup"><span data-stu-id="37326-127">For more information, see [About CNG DPAPI](/windows/desktop/SecCNG/cng-dpapi).</span></span>

<span data-ttu-id="37326-128">Le principal est encodé comme une règle de descripteur de protection.</span><span class="sxs-lookup"><span data-stu-id="37326-128">The principal is encoded as a protection descriptor rule.</span></span> <span data-ttu-id="37326-129">Dans l’exemple suivant appelle [ProtectKeysWithDpapiNG](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpaping), seul l’utilisateur de domaine avec le SID spécifié peut déchiffrer le key ring :</span><span class="sxs-lookup"><span data-stu-id="37326-129">In the following example that calls [ProtectKeysWithDpapiNG](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpaping), only the domain-joined user with the specified SID can decrypt the key ring:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Uses the descriptor rule "SID=S-1-5-21-..."
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
        flags: DpapiNGProtectionDescriptorFlags.None);
}
```

<span data-ttu-id="37326-130">Il existe également une surcharge sans paramètre de `ProtectKeysWithDpapiNG`.</span><span class="sxs-lookup"><span data-stu-id="37326-130">There's also a parameterless overload of `ProtectKeysWithDpapiNG`.</span></span> <span data-ttu-id="37326-131">Utilisez cette méthode pratique pour spécifier la règle « SID = {CURRENT_ACCOUNT_SID} », où *CURRENT_ACCOUNT_SID* est le SID du compte d’utilisateur Windows actuel :</span><span class="sxs-lookup"><span data-stu-id="37326-131">Use this convenience method to specify the rule "SID={CURRENT_ACCOUNT_SID}", where *CURRENT_ACCOUNT_SID* is the SID of the current Windows user account:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Use the descriptor rule "SID={current account SID}"
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG();
}
```

<span data-ttu-id="37326-132">Dans ce scénario, le contrôleur de domaine Active Directory est responsable de la distribution de clés de chiffrement utilisées par les opérations de DPAPI-NG.</span><span class="sxs-lookup"><span data-stu-id="37326-132">In this scenario, the AD domain controller is responsible for distributing the encryption keys used by the DPAPI-NG operations.</span></span> <span data-ttu-id="37326-133">L’utilisateur cible peut déchiffrer la charge utile chiffrée à partir de n’importe quel ordinateur joint au domaine (à condition que le processus s’exécute sous leur identité).</span><span class="sxs-lookup"><span data-stu-id="37326-133">The target user can decipher the encrypted payload from any domain-joined machine (provided that the process is running under their identity).</span></span>

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a><span data-ttu-id="37326-134">Chiffrement par certificat avec Windows DPAPI-NG</span><span class="sxs-lookup"><span data-stu-id="37326-134">Certificate-based encryption with Windows DPAPI-NG</span></span>

<span data-ttu-id="37326-135">Si l’application s’exécute sur Windows 8.1 / Windows Server 2012 R2 ou version ultérieure, vous pouvez utiliser Windows DPAPI-NG pour effectuer un chiffrement par certificat.</span><span class="sxs-lookup"><span data-stu-id="37326-135">If the app is running on Windows 8.1/Windows Server 2012 R2 or later, you can use Windows DPAPI-NG to perform certificate-based encryption.</span></span> <span data-ttu-id="37326-136">Utiliser la chaîne de descripteur de règle « certificat = HashId:THUMBPRINT », où *empreinte* est l’empreinte numérique SHA1 codé en hexadécimal du certificat :</span><span class="sxs-lookup"><span data-stu-id="37326-136">Use the rule descriptor string "CERTIFICATE=HashId:THUMBPRINT", where *THUMBPRINT* is the hex-encoded SHA1 thumbprint of the certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2...B5AEA2A9BD2575A0",
            flags: DpapiNGProtectionDescriptorFlags.None);
}
```

<span data-ttu-id="37326-137">N’importe quelle application pointée vers ce dépôt doit être en cours d’exécution sur Windows 8.1 / Windows Server 2012 R2 ou version ultérieure pour déchiffrer les clés.</span><span class="sxs-lookup"><span data-stu-id="37326-137">Any app pointed at this repository must be running on Windows 8.1/Windows Server 2012 R2 or later to decipher the keys.</span></span>

## <a name="custom-key-encryption"></a><span data-ttu-id="37326-138">Clé de chiffrement personnalisé</span><span class="sxs-lookup"><span data-stu-id="37326-138">Custom key encryption</span></span>

<span data-ttu-id="37326-139">Si les mécanismes d’origine ne sont pas appropriées, le développeur peut spécifier leur propre mécanisme de chiffrement à clé en fournissant un personnalisé [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor).</span><span class="sxs-lookup"><span data-stu-id="37326-139">If the in-box mechanisms aren't appropriate, the developer can specify their own key encryption mechanism by providing a custom [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor).</span></span>
