---
title: Extensibilité de la gestion de clés dans ASP.NET Core
author: rick-anderson
description: En savoir plus sur l’extensibilité de la gestion de clés de Protection des données ASP.NET Core.
ms.author: riande
ms.date: 11/22/2017
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: 965a7ed8ca2f72a66cfe093b5978a54fea5440fd
ms.sourcegitcommit: 8f8924ce4eb9effeaf489f177fb01b66867da16f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39219314"
---
# <a name="key-management-extensibility-in-aspnet-core"></a><span data-ttu-id="af0de-103">Extensibilité de la gestion de clés dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="af0de-103">Key management extensibility in ASP.NET Core</span></span>

> [!TIP]
> <span data-ttu-id="af0de-104">Lire le [gestion des clés](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) section avant de lire cette section, car il explique certains concepts fondamentaux derrière ces API.</span><span class="sxs-lookup"><span data-stu-id="af0de-104">Read the [key management](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) section before reading this section, as it explains some of the fundamental concepts behind these APIs.</span></span>

> [!WARNING]
> <span data-ttu-id="af0de-105">Types qui implémentent les interfaces suivantes doivent être thread-safe pour les appelants plusieurs.</span><span class="sxs-lookup"><span data-stu-id="af0de-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="key"></a><span data-ttu-id="af0de-106">Touche</span><span class="sxs-lookup"><span data-stu-id="af0de-106">Key</span></span>

<span data-ttu-id="af0de-107">Le `IKey` interface est la représentation sous forme de base d’une clé dans les systèmes de chiffrement.</span><span class="sxs-lookup"><span data-stu-id="af0de-107">The `IKey` interface is the basic representation of a key in cryptosystem.</span></span> <span data-ttu-id="af0de-108">L’expression clé est utilisé ici dans le point de vue abstrait, pas dans le sens littéral de « clé de chiffrement ».</span><span class="sxs-lookup"><span data-stu-id="af0de-108">The term key is used here in the abstract sense, not in the literal sense of "cryptographic key material".</span></span> <span data-ttu-id="af0de-109">Une clé a les propriétés suivantes :</span><span class="sxs-lookup"><span data-stu-id="af0de-109">A key has the following properties:</span></span>

* <span data-ttu-id="af0de-110">Dates d’activation, la création et l’expiration</span><span class="sxs-lookup"><span data-stu-id="af0de-110">Activation, creation, and expiration dates</span></span>

* <span data-ttu-id="af0de-111">État de révocation</span><span class="sxs-lookup"><span data-stu-id="af0de-111">Revocation status</span></span>

* <span data-ttu-id="af0de-112">Identificateur de clé (un GUID)</span><span class="sxs-lookup"><span data-stu-id="af0de-112">Key identifier (a GUID)</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="af0de-113">En outre, `IKey` expose un `CreateEncryptor` méthode qui peut être utilisé pour créer un [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance liée à cette clé.</span><span class="sxs-lookup"><span data-stu-id="af0de-113">Additionally, `IKey` exposes a `CreateEncryptor` method which can be used to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="af0de-114">En outre, `IKey` expose un `CreateEncryptorInstance` méthode qui peut être utilisé pour créer un [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance liée à cette clé.</span><span class="sxs-lookup"><span data-stu-id="af0de-114">Additionally, `IKey` exposes a `CreateEncryptorInstance` method which can be used to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

::: moniker-end

> [!NOTE]
> <span data-ttu-id="af0de-115">Il n’existe aucune API pour récupérer le matériel de chiffrement brutes à partir d’un `IKey` instance.</span><span class="sxs-lookup"><span data-stu-id="af0de-115">There's no API to retrieve the raw cryptographic material from an `IKey` instance.</span></span>

## <a name="ikeymanager"></a><span data-ttu-id="af0de-116">IKeyManager</span><span class="sxs-lookup"><span data-stu-id="af0de-116">IKeyManager</span></span>

<span data-ttu-id="af0de-117">Le `IKeyManager` interface représente un objet responsable du stockage de clés généraux, d’extraction et de manipulation.</span><span class="sxs-lookup"><span data-stu-id="af0de-117">The `IKeyManager` interface represents an object responsible for general key storage, retrieval, and manipulation.</span></span> <span data-ttu-id="af0de-118">Elle expose trois opérations de haut niveau :</span><span class="sxs-lookup"><span data-stu-id="af0de-118">It exposes three high-level operations:</span></span>

* <span data-ttu-id="af0de-119">Créer une nouvelle clé et conserver dans le stockage.</span><span class="sxs-lookup"><span data-stu-id="af0de-119">Create a new key and persist it to storage.</span></span>

* <span data-ttu-id="af0de-120">Obtenir toutes les clés de stockage.</span><span class="sxs-lookup"><span data-stu-id="af0de-120">Get all keys from storage.</span></span>

* <span data-ttu-id="af0de-121">Révoquer une ou plusieurs clés et conserver les informations de révocation dans le stockage.</span><span class="sxs-lookup"><span data-stu-id="af0de-121">Revoke one or more keys and persist the revocation information to storage.</span></span>

>[!WARNING]
> <span data-ttu-id="af0de-122">Écriture d’un `IKeyManager` est une tâche très avancée, et il ne doit pas tenter de la majorité des développeurs.</span><span class="sxs-lookup"><span data-stu-id="af0de-122">Writing an `IKeyManager` is a very advanced task, and the majority of developers shouldn't attempt it.</span></span> <span data-ttu-id="af0de-123">Au lieu de cela, la plupart des développeurs doit bénéficier des installations offertes par le [XmlKeyManager](#xmlkeymanager) classe.</span><span class="sxs-lookup"><span data-stu-id="af0de-123">Instead, most developers should take advantage of the facilities offered by the [XmlKeyManager](#xmlkeymanager) class.</span></span>

## <a name="xmlkeymanager"></a><span data-ttu-id="af0de-124">XmlKeyManager</span><span class="sxs-lookup"><span data-stu-id="af0de-124">XmlKeyManager</span></span>

<span data-ttu-id="af0de-125">Le `XmlKeyManager` type est l’implémentation concrète de l’emploi de `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="af0de-125">The `XmlKeyManager` type is the in-box concrete implementation of `IKeyManager`.</span></span> <span data-ttu-id="af0de-126">Il fournit plusieurs installations utiles, y compris le dépôt de clé et le chiffrement des clés au repos.</span><span class="sxs-lookup"><span data-stu-id="af0de-126">It provides several useful facilities, including key escrow and encryption of keys at rest.</span></span> <span data-ttu-id="af0de-127">Dans ce système, les clés sont représentées en tant qu’éléments XML (plus précisément, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span><span class="sxs-lookup"><span data-stu-id="af0de-127">Keys in this system are represented as XML elements (specifically, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span></span>

<span data-ttu-id="af0de-128">`XmlKeyManager` dépend de plusieurs autres composants au cours de l’exécution de ses tâches :</span><span class="sxs-lookup"><span data-stu-id="af0de-128">`XmlKeyManager` depends on several other components in the course of fulfilling its tasks:</span></span>

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="af0de-129">`AlgorithmConfiguration`, qui déterminent les algorithmes utilisés par les nouvelles clés.</span><span class="sxs-lookup"><span data-stu-id="af0de-129">`AlgorithmConfiguration`, which dictates the algorithms used by new keys.</span></span>

* <span data-ttu-id="af0de-130">`IXmlRepository`, qui contrôle où les clés sont conservés dans le stockage.</span><span class="sxs-lookup"><span data-stu-id="af0de-130">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="af0de-131">`IXmlEncryptor` [facultatif], ce qui permet le chiffrement de clés au repos.</span><span class="sxs-lookup"><span data-stu-id="af0de-131">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="af0de-132">`IKeyEscrowSink` [facultatif], qui fournit des services de dépôt de clé.</span><span class="sxs-lookup"><span data-stu-id="af0de-132">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* <span data-ttu-id="af0de-133">`IXmlRepository`, qui contrôle où les clés sont conservés dans le stockage.</span><span class="sxs-lookup"><span data-stu-id="af0de-133">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="af0de-134">`IXmlEncryptor` [facultatif], ce qui permet le chiffrement de clés au repos.</span><span class="sxs-lookup"><span data-stu-id="af0de-134">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="af0de-135">`IKeyEscrowSink` [facultatif], qui fournit des services de dépôt de clé.</span><span class="sxs-lookup"><span data-stu-id="af0de-135">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

::: moniker-end

<span data-ttu-id="af0de-136">Voici les diagrammes de haut niveau qui indiquent la façon dont ces composants sont reliés au sein de `XmlKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="af0de-136">Below are high-level diagrams which indicate how these components are wired together within `XmlKeyManager`.</span></span>

::: moniker range=">= aspnetcore-2.0"

![Création de la clé](key-management/_static/keycreation2.png)

<span data-ttu-id="af0de-138">*La création de clé / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="af0de-138">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="af0de-139">Dans l’implémentation de `CreateNewKey`, le `AlgorithmConfiguration` composant est utilisé pour créer un unique `IAuthenticatedEncryptorDescriptor`, lequel est ensuite sérialisé au format XML.</span><span class="sxs-lookup"><span data-stu-id="af0de-139">In the implementation of `CreateNewKey`, the `AlgorithmConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="af0de-140">Si un récepteur de dépôt de clé est présent, les données XML brutes (non chiffré) est fourni pour le récepteur pour le stockage à long terme.</span><span class="sxs-lookup"><span data-stu-id="af0de-140">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="af0de-141">Le code XML non chiffré est alors exécuté un `IXmlEncryptor` (si nécessaire) pour générer le document XML chiffré.</span><span class="sxs-lookup"><span data-stu-id="af0de-141">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="af0de-142">Ce document chiffré est rendu persistant dans le stockage à long terme via la `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="af0de-142">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="af0de-143">(Si aucun `IXmlEncryptor` est configuré, le document non chiffré est conservé dans le `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="af0de-143">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

![Récupération de clé](key-management/_static/keyretrieval2.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

![Création de la clé](key-management/_static/keycreation1.png)

<span data-ttu-id="af0de-146">*La création de clé / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="af0de-146">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="af0de-147">Dans l’implémentation de `CreateNewKey`, le `IAuthenticatedEncryptorConfiguration` composant est utilisé pour créer un unique `IAuthenticatedEncryptorDescriptor`, lequel est ensuite sérialisé au format XML.</span><span class="sxs-lookup"><span data-stu-id="af0de-147">In the implementation of `CreateNewKey`, the `IAuthenticatedEncryptorConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="af0de-148">Si un récepteur de dépôt de clé est présent, les données XML brutes (non chiffré) est fourni pour le récepteur pour le stockage à long terme.</span><span class="sxs-lookup"><span data-stu-id="af0de-148">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="af0de-149">Le code XML non chiffré est alors exécuté un `IXmlEncryptor` (si nécessaire) pour générer le document XML chiffré.</span><span class="sxs-lookup"><span data-stu-id="af0de-149">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="af0de-150">Ce document chiffré est rendu persistant dans le stockage à long terme via la `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="af0de-150">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="af0de-151">(Si aucun `IXmlEncryptor` est configuré, le document non chiffré est conservé dans le `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="af0de-151">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

![Récupération de clé](key-management/_static/keyretrieval1.png)

::: moniker-end

<span data-ttu-id="af0de-153">*Récupération de clé / GetAllKeys*</span><span class="sxs-lookup"><span data-stu-id="af0de-153">*Key Retrieval / GetAllKeys*</span></span>

<span data-ttu-id="af0de-154">Dans l’implémentation de `GetAllKeys`, clés représentant les documents XML et les révocations sont lus à partir de sous-jacent `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="af0de-154">In the implementation of `GetAllKeys`, the XML documents representing keys and revocations are read from the underlying `IXmlRepository`.</span></span> <span data-ttu-id="af0de-155">Si ces documents sont chiffrées, le système sera automatiquement les déchiffrer.</span><span class="sxs-lookup"><span data-stu-id="af0de-155">If these documents are encrypted, the system will automatically decrypt them.</span></span> <span data-ttu-id="af0de-156">`XmlKeyManager` crée l’approprié `IAuthenticatedEncryptorDescriptorDeserializer` instances pour désérialiser les documents de nouveau `IAuthenticatedEncryptorDescriptor` instances, qui sont ensuite encapsulées dans un individu `IKey` instances.</span><span class="sxs-lookup"><span data-stu-id="af0de-156">`XmlKeyManager` creates the appropriate `IAuthenticatedEncryptorDescriptorDeserializer` instances to deserialize the documents back into `IAuthenticatedEncryptorDescriptor` instances, which are then wrapped in individual `IKey` instances.</span></span> <span data-ttu-id="af0de-157">Cette collection de `IKey` instances est retourné à l’appelant.</span><span class="sxs-lookup"><span data-stu-id="af0de-157">This collection of `IKey` instances is returned to the caller.</span></span>

<span data-ttu-id="af0de-158">Vous trouverez plus d’informations sur les éléments XML particuliers dans le [document de format de stockage de clés](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).</span><span class="sxs-lookup"><span data-stu-id="af0de-158">Further information on the particular XML elements can be found in the [key storage format document](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).</span></span>

## <a name="ixmlrepository"></a><span data-ttu-id="af0de-159">IXmlRepository</span><span class="sxs-lookup"><span data-stu-id="af0de-159">IXmlRepository</span></span>

<span data-ttu-id="af0de-160">Le `IXmlRepository` interface représente un type qui peut faire persister XML à et récupérer des données XML à partir d’un magasin de stockage.</span><span class="sxs-lookup"><span data-stu-id="af0de-160">The `IXmlRepository` interface represents a type that can persist XML to and retrieve XML from a backing store.</span></span> <span data-ttu-id="af0de-161">Elle expose deux API :</span><span class="sxs-lookup"><span data-stu-id="af0de-161">It exposes two APIs:</span></span>

* <span data-ttu-id="af0de-162">`GetAllElements` :`IReadOnlyCollection<XElement>`</span><span class="sxs-lookup"><span data-stu-id="af0de-162">`GetAllElements` :`IReadOnlyCollection<XElement>`</span></span>

* `StoreElement(XElement element, string friendlyName)`

<span data-ttu-id="af0de-163">Les implémentations de `IXmlRepository` n’avez pas besoin analyser le XML en passant par leur intermédiaire.</span><span class="sxs-lookup"><span data-stu-id="af0de-163">Implementations of `IXmlRepository` don't need to parse the XML passing through them.</span></span> <span data-ttu-id="af0de-164">Ils doivent traiter les documents XML comme opaque et laissent les couches supérieures à vous inquiétez pas sur la génération et l’analyse de documents.</span><span class="sxs-lookup"><span data-stu-id="af0de-164">They should treat the XML documents as opaque and let higher layers worry about generating and parsing the documents.</span></span>

<span data-ttu-id="af0de-165">Il existe quatre types concrets intégrés qui implémentent `IXmlRepository`:</span><span class="sxs-lookup"><span data-stu-id="af0de-165">There are four built-in concrete types which implement `IXmlRepository`:</span></span>

* [<span data-ttu-id="af0de-166">FileSystemXmlRepository</span><span class="sxs-lookup"><span data-stu-id="af0de-166">FileSystemXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [<span data-ttu-id="af0de-167">RegistryXmlRepository</span><span class="sxs-lookup"><span data-stu-id="af0de-167">RegistryXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [<span data-ttu-id="af0de-168">AzureStorage.AzureBlobXmlRepository</span><span class="sxs-lookup"><span data-stu-id="af0de-168">AzureStorage.AzureBlobXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [<span data-ttu-id="af0de-169">RedisXmlRepository</span><span class="sxs-lookup"><span data-stu-id="af0de-169">RedisXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.redisxmlrepository)

<span data-ttu-id="af0de-170">Consultez le [document de fournisseurs de stockage de clés](xref:security/data-protection/implementation/key-storage-providers) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="af0de-170">See the [key storage providers document](xref:security/data-protection/implementation/key-storage-providers) for more information.</span></span>

<span data-ttu-id="af0de-171">L’enregistrement personnalisé `IXmlRepository` est appropriée lorsque vous utilisez un magasin de stockage différents (par exemple, le stockage Azure Table).</span><span class="sxs-lookup"><span data-stu-id="af0de-171">Registering a custom `IXmlRepository` is appropriate when using a different backing store (for example, Azure Table Storage).</span></span>

<span data-ttu-id="af0de-172">Pour modifier le référentiel par défaut à l’échelle de l’application, inscrivez un personnalisé `IXmlRepository` instance :</span><span class="sxs-lookup"><span data-stu-id="af0de-172">To change the default repository application-wide, register a custom `IXmlRepository` instance:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
services.Configure<KeyManagementOptions>(options => options.XmlRepository = new MyCustomXmlRepository());
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddSingleton<IXmlRepository>(new MyCustomXmlRepository());
```

::: moniker-end

## <a name="ixmlencryptor"></a><span data-ttu-id="af0de-173">IXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="af0de-173">IXmlEncryptor</span></span>

<span data-ttu-id="af0de-174">Le `IXmlEncryptor` interface représente un type qui permet de chiffrer un élément XML de texte en clair.</span><span class="sxs-lookup"><span data-stu-id="af0de-174">The `IXmlEncryptor` interface represents a type that can encrypt a plaintext XML element.</span></span> <span data-ttu-id="af0de-175">Elle expose une seule API :</span><span class="sxs-lookup"><span data-stu-id="af0de-175">It exposes a single API:</span></span>

* <span data-ttu-id="af0de-176">Chiffrer (plaintextElement XElement) : EncryptedXmlInfo</span><span class="sxs-lookup"><span data-stu-id="af0de-176">Encrypt(XElement plaintextElement) : EncryptedXmlInfo</span></span>

<span data-ttu-id="af0de-177">Si un sérialisée `IAuthenticatedEncryptorDescriptor` contient des éléments de la mention « requiert le chiffrement », puis `XmlKeyManager` exécutera ces éléments via configuré `IXmlEncryptor`de `Encrypt` méthode et qu’elle conserve l’élément des plutôt que la élément de texte en clair à le `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="af0de-177">If a serialized `IAuthenticatedEncryptorDescriptor` contains any elements marked as "requires encryption", then `XmlKeyManager` will run those elements through the configured `IXmlEncryptor`'s `Encrypt` method, and it will persist the enciphered element rather than the plaintext element to the `IXmlRepository`.</span></span> <span data-ttu-id="af0de-178">La sortie de la `Encrypt` méthode est un `EncryptedXmlInfo` objet.</span><span class="sxs-lookup"><span data-stu-id="af0de-178">The output of the `Encrypt` method is an `EncryptedXmlInfo` object.</span></span> <span data-ttu-id="af0de-179">Cet objet est un wrapper qui contient les deux le résultant des `XElement` et le Type qui représente un `IXmlDecryptor` qui peut être utilisé pour déchiffrer l’élément correspondant.</span><span class="sxs-lookup"><span data-stu-id="af0de-179">This object is a wrapper which contains both the resultant enciphered `XElement` and the Type which represents an `IXmlDecryptor` which can be used to decipher the corresponding element.</span></span>

<span data-ttu-id="af0de-180">Il existe quatre types concrets intégrés qui implémentent `IXmlEncryptor`:</span><span class="sxs-lookup"><span data-stu-id="af0de-180">There are four built-in concrete types which implement `IXmlEncryptor`:</span></span>

* [<span data-ttu-id="af0de-181">CertificateXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="af0de-181">CertificateXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.certificatexmlencryptor)
* [<span data-ttu-id="af0de-182">DpapiNGXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="af0de-182">DpapiNGXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapingxmlencryptor)
* [<span data-ttu-id="af0de-183">DpapiXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="af0de-183">DpapiXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapixmlencryptor)
* [<span data-ttu-id="af0de-184">NullXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="af0de-184">NullXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.nullxmlencryptor)

<span data-ttu-id="af0de-185">Consultez le [chiffrement à clé au document de rest](xref:security/data-protection/implementation/key-encryption-at-rest) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="af0de-185">See the [key encryption at rest document](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

<span data-ttu-id="af0de-186">Pour modifier le mécanisme de clé chiffrement au repos par défaut à l’échelle de l’application, inscrivez un personnalisé `IXmlEncryptor` instance :</span><span class="sxs-lookup"><span data-stu-id="af0de-186">To change the default key-encryption-at-rest mechanism application-wide, register a custom `IXmlEncryptor` instance:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
services.Configure<KeyManagementOptions>(options => options.XmlEncryptor = new MyCustomXmlEncryptor());
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddSingleton<IXmlEncryptor>(new MyCustomXmlEncryptor());
```

::: moniker-end

## <a name="ixmldecryptor"></a><span data-ttu-id="af0de-187">IXmlDecryptor</span><span class="sxs-lookup"><span data-stu-id="af0de-187">IXmlDecryptor</span></span>

<span data-ttu-id="af0de-188">Le `IXmlDecryptor` interface représente un type qui sait comment déchiffrer un `XElement` qui a été des un `IXmlEncryptor`.</span><span class="sxs-lookup"><span data-stu-id="af0de-188">The `IXmlDecryptor` interface represents a type that knows how to decrypt an `XElement` that was enciphered via an `IXmlEncryptor`.</span></span> <span data-ttu-id="af0de-189">Elle expose une seule API :</span><span class="sxs-lookup"><span data-stu-id="af0de-189">It exposes a single API:</span></span>

* <span data-ttu-id="af0de-190">Déchiffrer (encryptedElement XElement) : XElement</span><span class="sxs-lookup"><span data-stu-id="af0de-190">Decrypt(XElement encryptedElement) : XElement</span></span>

<span data-ttu-id="af0de-191">Le `Decrypt` méthode annule le chiffrement effectué par `IXmlEncryptor.Encrypt`.</span><span class="sxs-lookup"><span data-stu-id="af0de-191">The `Decrypt` method undoes the encryption performed by `IXmlEncryptor.Encrypt`.</span></span> <span data-ttu-id="af0de-192">En règle générale, chaque concret `IXmlEncryptor` implémentation aura un concret correspondante `IXmlDecryptor` implémentation.</span><span class="sxs-lookup"><span data-stu-id="af0de-192">Generally, each concrete `IXmlEncryptor` implementation will have a corresponding concrete `IXmlDecryptor` implementation.</span></span>

<span data-ttu-id="af0de-193">Types qui implémentent `IXmlDecryptor` doit avoir l’une des deux constructeurs publics :</span><span class="sxs-lookup"><span data-stu-id="af0de-193">Types which implement `IXmlDecryptor` should have one of the following two public constructors:</span></span>

* <span data-ttu-id="af0de-194">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="af0de-194">.ctor(IServiceProvider)</span></span>
* <span data-ttu-id="af0de-195">.ctor()</span><span class="sxs-lookup"><span data-stu-id="af0de-195">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="af0de-196">Le `IServiceProvider` passé au constructeur peut être null.</span><span class="sxs-lookup"><span data-stu-id="af0de-196">The `IServiceProvider` passed to the constructor may be null.</span></span>

## <a name="ikeyescrowsink"></a><span data-ttu-id="af0de-197">IKeyEscrowSink</span><span class="sxs-lookup"><span data-stu-id="af0de-197">IKeyEscrowSink</span></span>

<span data-ttu-id="af0de-198">Le `IKeyEscrowSink` interface représente un type qui peut effectuer le tiers de confiance des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="af0de-198">The `IKeyEscrowSink` interface represents a type that can perform escrow of sensitive information.</span></span> <span data-ttu-id="af0de-199">Rappelez-vous que les descripteurs sérialisées peuvent contenir des informations sensibles (telles que le matériel de chiffrement), et c’est ce qui a conduit à l’introduction de la [IXmlEncryptor](#ixmlencryptor) tapez en premier lieu.</span><span class="sxs-lookup"><span data-stu-id="af0de-199">Recall that serialized descriptors might contain sensitive information (such as cryptographic material), and this is what led to the introduction of the [IXmlEncryptor](#ixmlencryptor) type in the first place.</span></span> <span data-ttu-id="af0de-200">Toutefois, des accidents se produisent, et des anneaux de clé peut être supprimés ou endommagés.</span><span class="sxs-lookup"><span data-stu-id="af0de-200">However, accidents happen, and key rings can be deleted or become corrupted.</span></span>

<span data-ttu-id="af0de-201">L’interface de tiers de confiance offre une urgence, autorisant l’accès au document XML sérialisé brut avant sa transformation par aucun configuré [IXmlEncryptor](#ixmlencryptor).</span><span class="sxs-lookup"><span data-stu-id="af0de-201">The escrow interface provides an emergency escape hatch, allowing access to the raw serialized XML before it's transformed by any configured [IXmlEncryptor](#ixmlencryptor).</span></span> <span data-ttu-id="af0de-202">L’interface expose une API unique :</span><span class="sxs-lookup"><span data-stu-id="af0de-202">The interface exposes a single API:</span></span>

* <span data-ttu-id="af0de-203">Store (Guid keyId, élément de XElement)</span><span class="sxs-lookup"><span data-stu-id="af0de-203">Store(Guid keyId, XElement element)</span></span>

<span data-ttu-id="af0de-204">C’est à le `IKeyEscrowSink` implémentation pour gérer l’élément fourni de manière sécurisée cohérente avec la stratégie d’entreprise.</span><span class="sxs-lookup"><span data-stu-id="af0de-204">It's up to the `IKeyEscrowSink` implementation to handle the provided element in a secure manner consistent with business policy.</span></span> <span data-ttu-id="af0de-205">Une implémentation possible peut être pour le récepteur tiers de confiance chiffrer l’élément XML à l’aide d’un certificat X.509 d’entreprise connu où la clé privée du certificat a été déposée ; le `CertificateXmlEncryptor` type peut vous y aider.</span><span class="sxs-lookup"><span data-stu-id="af0de-205">One possible implementation could be for the escrow sink to encrypt the XML element using a known corporate X.509 certificate where the certificate's private key has been escrowed; the `CertificateXmlEncryptor` type can assist with this.</span></span> <span data-ttu-id="af0de-206">Le `IKeyEscrowSink` implémentation est également chargée de rendre l’élément fourni de manière appropriée.</span><span class="sxs-lookup"><span data-stu-id="af0de-206">The `IKeyEscrowSink` implementation is also responsible for persisting the provided element appropriately.</span></span>

<span data-ttu-id="af0de-207">Par défaut aucun mécanisme de tiers de confiance n’est activé, bien que les administrateurs de serveur peuvent [configurer cela dans le monde entier](xref:security/data-protection/configuration/machine-wide-policy).</span><span class="sxs-lookup"><span data-stu-id="af0de-207">By default no escrow mechanism is enabled, though server administrators can [configure this globally](xref:security/data-protection/configuration/machine-wide-policy).</span></span> <span data-ttu-id="af0de-208">Il peut également être configuré par programmation la `IDataProtectionBuilder.AddKeyEscrowSink` méthode comme illustré dans l’exemple ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="af0de-208">It can also be configured programmatically via the `IDataProtectionBuilder.AddKeyEscrowSink` method as shown in the sample below.</span></span> <span data-ttu-id="af0de-209">Le `AddKeyEscrowSink` mise en miroir des surcharges de méthode le `IServiceCollection.AddSingleton` et `IServiceCollection.AddInstance` surcharges, comme `IKeyEscrowSink` instances sont destinées à être singletons.</span><span class="sxs-lookup"><span data-stu-id="af0de-209">The `AddKeyEscrowSink` method overloads mirror the `IServiceCollection.AddSingleton` and `IServiceCollection.AddInstance` overloads, as `IKeyEscrowSink` instances are intended to be singletons.</span></span> <span data-ttu-id="af0de-210">Si plusieurs `IKeyEscrowSink` instances sont enregistrées, chacun d’eux sera appelée pendant la génération de clés, afin de clés peuvent être déposées simultanément plusieurs mécanismes.</span><span class="sxs-lookup"><span data-stu-id="af0de-210">If multiple `IKeyEscrowSink` instances are registered, each one will be called during key generation, so keys can be escrowed to multiple mechanisms simultaneously.</span></span>

<span data-ttu-id="af0de-211">Il n’existe aucune API pour lire des documents à partir d’un `IKeyEscrowSink` instance.</span><span class="sxs-lookup"><span data-stu-id="af0de-211">There's no API to read material from an `IKeyEscrowSink` instance.</span></span> <span data-ttu-id="af0de-212">Ceci est cohérent avec la théorie de conception du mécanisme de tiers de confiance : il est conçu pour améliorer le matériel de clé pour une autorité approuvée, et étant donné que l’application est lui-même pas une autorité approuvée, il ne doit pas avoir accès à son propre matériel mises en dépôt.</span><span class="sxs-lookup"><span data-stu-id="af0de-212">This is consistent with the design theory of the escrow mechanism: it's intended to make the key material accessible to a trusted authority, and since the application is itself not a trusted authority, it shouldn't have access to its own escrowed material.</span></span>

<span data-ttu-id="af0de-213">L’exemple de code suivant montre comment créer et inscrire un `IKeyEscrowSink` où les clés sont déposées telles que seuls les membres du « CONTOSODomain Admins » pour les récupérer.</span><span class="sxs-lookup"><span data-stu-id="af0de-213">The following sample code demonstrates creating and registering an `IKeyEscrowSink` where keys are escrowed such that only members of "CONTOSODomain Admins" can recover them.</span></span>

> [!NOTE]
> <span data-ttu-id="af0de-214">Pour exécuter cet exemple, vous devez être sur un appareil joint au domaine Windows 8 / machine Windows Server 2012 et le contrôleur de domaine doivent être Windows Server 2012 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="af0de-214">To run this sample, you must be on a domain-joined Windows 8 / Windows Server 2012 machine, and the domain controller must be Windows Server 2012 or later.</span></span>

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
