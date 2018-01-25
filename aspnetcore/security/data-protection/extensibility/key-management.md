---
title: "Extensibilité de la gestion de clés"
author: rick-anderson
description: "Ce document décrit l’extensibilité de la gestion de clés de protection de données ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 11/22/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: d748ee9d3edf9eed4285fab447d5b379dfcd937c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
# <a name="key-management-extensibility"></a><span data-ttu-id="34f2a-103">Extensibilité de la gestion de clés</span><span class="sxs-lookup"><span data-stu-id="34f2a-103">Key management extensibility</span></span>

<a name="data-protection-extensibility-key-management"></a>

>[!TIP]
> <span data-ttu-id="34f2a-104">Lecture la [gestion de clés](../implementation/key-management.md#data-protection-implementation-key-management) section avant de lire cette section, car il décrit certains des concepts fondamentaux de ces API.</span><span class="sxs-lookup"><span data-stu-id="34f2a-104">Read the [key management](../implementation/key-management.md#data-protection-implementation-key-management) section before reading this section, as it explains some of the fundamental concepts behind these APIs.</span></span>

>[!WARNING]
> <span data-ttu-id="34f2a-105">Les types qui implémentent des interfaces suivantes doivent être thread-safe pour les appelants plusieurs.</span><span class="sxs-lookup"><span data-stu-id="34f2a-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="key"></a><span data-ttu-id="34f2a-106">Touche</span><span class="sxs-lookup"><span data-stu-id="34f2a-106">Key</span></span>

<span data-ttu-id="34f2a-107">Le `IKey` interface est la représentation sous forme de base d’une clé dans le système de cryptage par.</span><span class="sxs-lookup"><span data-stu-id="34f2a-107">The `IKey` interface is the basic representation of a key in cryptosystem.</span></span> <span data-ttu-id="34f2a-108">L’expression clé est utilisé ici dans le sens abstrait, pas dans le sens littéral « chiffrement de matériel de clé ».</span><span class="sxs-lookup"><span data-stu-id="34f2a-108">The term key is used here in the abstract sense, not in the literal sense of "cryptographic key material".</span></span> <span data-ttu-id="34f2a-109">Une clé a les propriétés suivantes :</span><span class="sxs-lookup"><span data-stu-id="34f2a-109">A key has the following properties:</span></span>

* <span data-ttu-id="34f2a-110">Dates d’expiration, la création et l’activation</span><span class="sxs-lookup"><span data-stu-id="34f2a-110">Activation, creation, and expiration dates</span></span>

* <span data-ttu-id="34f2a-111">État de révocation</span><span class="sxs-lookup"><span data-stu-id="34f2a-111">Revocation status</span></span>

* <span data-ttu-id="34f2a-112">Identificateur de clé (GUID)</span><span class="sxs-lookup"><span data-stu-id="34f2a-112">Key identifier (a GUID)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="34f2a-113">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="34f2a-113">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="34f2a-114">En outre, `IKey` expose un `CreateEncryptor` méthode qui peut être utilisé pour créer un [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance liée à cette clé.</span><span class="sxs-lookup"><span data-stu-id="34f2a-114">Additionally, `IKey` exposes a `CreateEncryptor` method which can be used to create an [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="34f2a-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="34f2a-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="34f2a-116">En outre, `IKey` expose un `CreateEncryptorInstance` méthode qui peut être utilisé pour créer un [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance liée à cette clé.</span><span class="sxs-lookup"><span data-stu-id="34f2a-116">Additionally, `IKey` exposes a `CreateEncryptorInstance` method which can be used to create an [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

---

> [!NOTE]
> <span data-ttu-id="34f2a-117">Il n’existe aucune API pour extraire le matériel de chiffrement brut à partir d’un `IKey` instance.</span><span class="sxs-lookup"><span data-stu-id="34f2a-117">There's no API to retrieve the raw cryptographic material from an `IKey` instance.</span></span>

## <a name="ikeymanager"></a><span data-ttu-id="34f2a-118">IKeyManager</span><span class="sxs-lookup"><span data-stu-id="34f2a-118">IKeyManager</span></span>

<span data-ttu-id="34f2a-119">Le `IKeyManager` interface représente un objet chargé de stockage général de la clé, la récupération et la manipulation.</span><span class="sxs-lookup"><span data-stu-id="34f2a-119">The `IKeyManager` interface represents an object responsible for general key storage, retrieval, and manipulation.</span></span> <span data-ttu-id="34f2a-120">Il expose trois opérations de niveau supérieur :</span><span class="sxs-lookup"><span data-stu-id="34f2a-120">It exposes three high-level operations:</span></span>

* <span data-ttu-id="34f2a-121">Créer une nouvelle clé et la rendre persistante dans le stockage.</span><span class="sxs-lookup"><span data-stu-id="34f2a-121">Create a new key and persist it to storage.</span></span>

* <span data-ttu-id="34f2a-122">Obtenir toutes les clés de stockage.</span><span class="sxs-lookup"><span data-stu-id="34f2a-122">Get all keys from storage.</span></span>

* <span data-ttu-id="34f2a-123">Révoquer une ou plusieurs clés et rendre persistantes les informations de révocation pour le stockage.</span><span class="sxs-lookup"><span data-stu-id="34f2a-123">Revoke one or more keys and persist the revocation information to storage.</span></span>

>[!WARNING]
> <span data-ttu-id="34f2a-124">L’écriture d’un `IKeyManager` est une tâche très avancée, et il ne doit pas tenter de la majorité des développeurs.</span><span class="sxs-lookup"><span data-stu-id="34f2a-124">Writing an `IKeyManager` is a very advanced task, and the majority of developers shouldn't attempt it.</span></span> <span data-ttu-id="34f2a-125">Au lieu de cela, la plupart des développeurs doit bénéficier des installations offertes par le [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) classe.</span><span class="sxs-lookup"><span data-stu-id="34f2a-125">Instead, most developers should take advantage of the facilities offered by the [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) class.</span></span>

<a name="data-protection-extensibility-key-management-xmlkeymanager"></a>

## <a name="xmlkeymanager"></a><span data-ttu-id="34f2a-126">XmlKeyManager</span><span class="sxs-lookup"><span data-stu-id="34f2a-126">XmlKeyManager</span></span>

<span data-ttu-id="34f2a-127">Le `XmlKeyManager` type est l’implémentation concrète de la boîte de réception de `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="34f2a-127">The `XmlKeyManager` type is the in-box concrete implementation of `IKeyManager`.</span></span> <span data-ttu-id="34f2a-128">Il fournit plusieurs installations utiles, y compris le dépôt de clé et le chiffrement des clés au repos.</span><span class="sxs-lookup"><span data-stu-id="34f2a-128">It provides several useful facilities, including key escrow and encryption of keys at rest.</span></span> <span data-ttu-id="34f2a-129">Dans ce système, les clés sont représentées comme des éléments XML (en particulier, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span><span class="sxs-lookup"><span data-stu-id="34f2a-129">Keys in this system are represented as XML elements (specifically, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span></span>

<span data-ttu-id="34f2a-130">`XmlKeyManager`dépend de plusieurs autres composants au cours de l’exécution de ses tâches :</span><span class="sxs-lookup"><span data-stu-id="34f2a-130">`XmlKeyManager` depends on several other components in the course of fulfilling its tasks:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="34f2a-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="34f2a-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="34f2a-132">`AlgorithmConfiguration`, qui déterminent les algorithmes utilisés par les nouvelles clés.</span><span class="sxs-lookup"><span data-stu-id="34f2a-132">`AlgorithmConfiguration`, which dictates the algorithms used by new keys.</span></span>

* <span data-ttu-id="34f2a-133">`IXmlRepository`, qui contrôle où les clés sont rendues persistantes dans le stockage.</span><span class="sxs-lookup"><span data-stu-id="34f2a-133">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="34f2a-134">`IXmlEncryptor`[facultatif], ce qui permet de chiffrer les clés au repos.</span><span class="sxs-lookup"><span data-stu-id="34f2a-134">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="34f2a-135">`IKeyEscrowSink`[facultatif], qui fournit des services de clé (Key escrow).</span><span class="sxs-lookup"><span data-stu-id="34f2a-135">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="34f2a-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="34f2a-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="34f2a-137">`IXmlRepository`, qui contrôle où les clés sont rendues persistantes dans le stockage.</span><span class="sxs-lookup"><span data-stu-id="34f2a-137">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="34f2a-138">`IXmlEncryptor`[facultatif], ce qui permet de chiffrer les clés au repos.</span><span class="sxs-lookup"><span data-stu-id="34f2a-138">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="34f2a-139">`IKeyEscrowSink`[facultatif], qui fournit des services de clé (Key escrow).</span><span class="sxs-lookup"><span data-stu-id="34f2a-139">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

---

<span data-ttu-id="34f2a-140">Vous trouverez ci-dessous des diagrammes de haut niveau qui indiquent la façon dont ces composants sont reliées entre elles au sein de `XmlKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="34f2a-140">Below are high-level diagrams which indicate how these components are wired together within `XmlKeyManager`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="34f2a-141">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="34f2a-141">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![Création de la clé](key-management/_static/keycreation2.png)

   <span data-ttu-id="34f2a-143">*La création de clé / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="34f2a-143">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="34f2a-144">Dans l’implémentation de `CreateNewKey`, le `AlgorithmConfiguration` composant est utilisé pour créer un unique `IAuthenticatedEncryptorDescriptor`, lequel est ensuite sérialisé au format XML.</span><span class="sxs-lookup"><span data-stu-id="34f2a-144">In the implementation of `CreateNewKey`, the `AlgorithmConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="34f2a-145">Si un récepteur de clé (Key escrow) est présent, les données XML brutes (non chiffré) est fourni pour le récepteur pour le stockage à long terme.</span><span class="sxs-lookup"><span data-stu-id="34f2a-145">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="34f2a-146">Le code XML non chiffré est ensuite exécuter via un `IXmlEncryptor` (si nécessaire) pour générer le document XML chiffré.</span><span class="sxs-lookup"><span data-stu-id="34f2a-146">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="34f2a-147">Ce document chiffré est rendue persistante dans un stockage à long terme via la `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="34f2a-147">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="34f2a-148">(Si aucun `IXmlEncryptor` est configuré, le document non chiffré est conservé dans le `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="34f2a-148">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="34f2a-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="34f2a-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![Création de la clé](key-management/_static/keycreation1.png)

   <span data-ttu-id="34f2a-151">*La création de clé / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="34f2a-151">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="34f2a-152">Dans l’implémentation de `CreateNewKey`, le `IAuthenticatedEncryptorConfiguration` composant est utilisé pour créer un unique `IAuthenticatedEncryptorDescriptor`, lequel est ensuite sérialisé au format XML.</span><span class="sxs-lookup"><span data-stu-id="34f2a-152">In the implementation of `CreateNewKey`, the `IAuthenticatedEncryptorConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="34f2a-153">Si un récepteur de clé (Key escrow) est présent, les données XML brutes (non chiffré) est fourni pour le récepteur pour le stockage à long terme.</span><span class="sxs-lookup"><span data-stu-id="34f2a-153">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="34f2a-154">Le code XML non chiffré est ensuite exécuter via un `IXmlEncryptor` (si nécessaire) pour générer le document XML chiffré.</span><span class="sxs-lookup"><span data-stu-id="34f2a-154">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="34f2a-155">Ce document chiffré est rendue persistante dans un stockage à long terme via la `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="34f2a-155">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="34f2a-156">(Si aucun `IXmlEncryptor` est configuré, le document non chiffré est conservé dans le `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="34f2a-156">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

---

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="34f2a-157">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="34f2a-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![Récupération de clés](key-management/_static/keyretrieval2.png)
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="34f2a-159">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="34f2a-159">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![Récupération de clés](key-management/_static/keyretrieval1.png)

---

   <span data-ttu-id="34f2a-161">*Récupération de clé / GetAllKeys*</span><span class="sxs-lookup"><span data-stu-id="34f2a-161">*Key Retrieval / GetAllKeys*</span></span>

<span data-ttu-id="34f2a-162">Dans l’implémentation de `GetAllKeys`, représentant les clés des documents XML et révocations sont lus à partir de l’objet sous-jacent `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="34f2a-162">In the implementation of `GetAllKeys`, the XML documents representing keys and revocations are read from the underlying `IXmlRepository`.</span></span> <span data-ttu-id="34f2a-163">Si ces documents sont chiffrées, le système sera automatiquement les déchiffrer.</span><span class="sxs-lookup"><span data-stu-id="34f2a-163">If these documents are encrypted, the system will automatically decrypt them.</span></span> <span data-ttu-id="34f2a-164">`XmlKeyManager`crée le `IAuthenticatedEncryptorDescriptorDeserializer` instances pour désérialiser les documents de nouveau `IAuthenticatedEncryptorDescriptor` instances, qui sont ensuite encapsulées dans individuels `IKey` instances.</span><span class="sxs-lookup"><span data-stu-id="34f2a-164">`XmlKeyManager` creates the appropriate `IAuthenticatedEncryptorDescriptorDeserializer` instances to deserialize the documents back into `IAuthenticatedEncryptorDescriptor` instances, which are then wrapped in individual `IKey` instances.</span></span> <span data-ttu-id="34f2a-165">Cette collection de `IKey` instances est retourné à l’appelant.</span><span class="sxs-lookup"><span data-stu-id="34f2a-165">This collection of `IKey` instances is returned to the caller.</span></span>

<span data-ttu-id="34f2a-166">Vous trouverez plus d’informations sur les éléments XML particuliers dans le [document au format de stockage de clés](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).</span><span class="sxs-lookup"><span data-stu-id="34f2a-166">Further information on the particular XML elements can be found in the [key storage format document](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).</span></span>

## <a name="ixmlrepository"></a><span data-ttu-id="34f2a-167">IXmlRepository</span><span class="sxs-lookup"><span data-stu-id="34f2a-167">IXmlRepository</span></span>

<span data-ttu-id="34f2a-168">Le `IXmlRepository` interface représente un type qui peut faire persister XML à et récupérer des données XML à partir d’un magasin de stockage.</span><span class="sxs-lookup"><span data-stu-id="34f2a-168">The `IXmlRepository` interface represents a type that can persist XML to and retrieve XML from a backing store.</span></span> <span data-ttu-id="34f2a-169">Il expose deux API :</span><span class="sxs-lookup"><span data-stu-id="34f2a-169">It exposes two APIs:</span></span>

* <span data-ttu-id="34f2a-170">GetAllElements() : IReadOnlyCollection<XElement></span><span class="sxs-lookup"><span data-stu-id="34f2a-170">GetAllElements() : IReadOnlyCollection<XElement></span></span>

* <span data-ttu-id="34f2a-171">StoreElement (élément de XElement, chaîne friendlyName)</span><span class="sxs-lookup"><span data-stu-id="34f2a-171">StoreElement(XElement element, string friendlyName)</span></span>

<span data-ttu-id="34f2a-172">Les implémentations de `IXmlRepository` n’avez pas besoin analyser le XML en passant par leur intermédiaire.</span><span class="sxs-lookup"><span data-stu-id="34f2a-172">Implementations of `IXmlRepository` don't need to parse the XML passing through them.</span></span> <span data-ttu-id="34f2a-173">Qu’ils doivent traiter les documents XML comme opaque et laisser les couches supérieures vous inquiétez pas sur la génération et l’analyse de documents.</span><span class="sxs-lookup"><span data-stu-id="34f2a-173">They should treat the XML documents as opaque and let higher layers worry about generating and parsing the documents.</span></span>

<span data-ttu-id="34f2a-174">Il existe deux types intégrés concrètes qui implémentent `IXmlRepository`: `FileSystemXmlRepository` et `RegistryXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="34f2a-174">There are two built-in concrete types which implement `IXmlRepository`: `FileSystemXmlRepository` and `RegistryXmlRepository`.</span></span> <span data-ttu-id="34f2a-175">Consultez le [document de fournisseurs de stockage de clés](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="34f2a-175">See the [key storage providers document](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) for more information.</span></span> <span data-ttu-id="34f2a-176">Enregistrement personnalisé `IXmlRepository` serait de manière appropriée à utiliser un magasin de stockage, par exemple, stockage d’objets Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="34f2a-176">Registering a custom `IXmlRepository` would be the appropriate manner to use a different backing store, e.g., Azure Blob Storage.</span></span>

<span data-ttu-id="34f2a-177">Pour modifier le référentiel par défaut à l’échelle de l’application, enregistrer personnalisé `IXmlRepository` instance :</span><span class="sxs-lookup"><span data-stu-id="34f2a-177">To change the default repository application-wide, register a custom `IXmlRepository` instance:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="34f2a-178">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="34f2a-178">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlRepository = new MyCustomXmlRepository());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="34f2a-179">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="34f2a-179">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlRepository>(new MyCustomXmlRepository());
   ```

---

<a name="data-protection-extensibility-key-management-ixmlencryptor"></a>

## <a name="ixmlencryptor"></a><span data-ttu-id="34f2a-180">IXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="34f2a-180">IXmlEncryptor</span></span>

<span data-ttu-id="34f2a-181">Le `IXmlEncryptor` interface représente un type qui peut chiffrer un élément XML de texte en clair.</span><span class="sxs-lookup"><span data-stu-id="34f2a-181">The `IXmlEncryptor` interface represents a type that can encrypt a plaintext XML element.</span></span> <span data-ttu-id="34f2a-182">Il expose une API unique :</span><span class="sxs-lookup"><span data-stu-id="34f2a-182">It exposes a single API:</span></span>

* <span data-ttu-id="34f2a-183">Chiffrer (plaintextElement XElement) : EncryptedXmlInfo</span><span class="sxs-lookup"><span data-stu-id="34f2a-183">Encrypt(XElement plaintextElement) : EncryptedXmlInfo</span></span>

<span data-ttu-id="34f2a-184">Si elle est sérialisée `IAuthenticatedEncryptorDescriptor` contient des éléments de la mention « exige un chiffrement », puis `XmlKeyManager` exécute ces éléments configuré `IXmlEncryptor`de `Encrypt` (méthode), qui rendront l’élément enciphered plutôt que élément de texte en clair à le `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="34f2a-184">If a serialized `IAuthenticatedEncryptorDescriptor` contains any elements marked as "requires encryption", then `XmlKeyManager` will run those elements through the configured `IXmlEncryptor`'s `Encrypt` method, and it will persist the enciphered element rather than the plaintext element to the `IXmlRepository`.</span></span> <span data-ttu-id="34f2a-185">La sortie de la `Encrypt` méthode est un `EncryptedXmlInfo` objet.</span><span class="sxs-lookup"><span data-stu-id="34f2a-185">The output of the `Encrypt` method is an `EncryptedXmlInfo` object.</span></span> <span data-ttu-id="34f2a-186">Cet objet est un wrapper qui contient les deux résultant enciphered `XElement` et le Type qui représente un `IXmlDecryptor` qui peut être utilisé pour déchiffrer l’élément correspondant.</span><span class="sxs-lookup"><span data-stu-id="34f2a-186">This object is a wrapper which contains both the resultant enciphered `XElement` and the Type which represents an `IXmlDecryptor` which can be used to decipher the corresponding element.</span></span>

<span data-ttu-id="34f2a-187">Il existe quatre types intégrés concrètes qui implémentent `IXmlEncryptor`:</span><span class="sxs-lookup"><span data-stu-id="34f2a-187">There are four built-in concrete types which implement `IXmlEncryptor`:</span></span>
* `CertificateXmlEncryptor`
* `DpapiNGXmlEncryptor`
* `DpapiXmlEncryptor`
* `NullXmlEncryptor`

<span data-ttu-id="34f2a-188">Consultez le [chiffrement à clé au niveau du document de rest](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="34f2a-188">See the [key encryption at rest document](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) for more information.</span></span>

<span data-ttu-id="34f2a-189">Pour modifier le mécanisme de clé chiffrement au repos à l’échelle de l’application par défaut, enregistrer personnalisé `IXmlEncryptor` instance :</span><span class="sxs-lookup"><span data-stu-id="34f2a-189">To change the default key-encryption-at-rest mechanism application-wide, register a custom `IXmlEncryptor` instance:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="34f2a-190">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="34f2a-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlEncryptor = new MyCustomXmlEncryptor());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="34f2a-191">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="34f2a-191">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlEncryptor>(new MyCustomXmlEncryptor());
   ```

---

## <a name="ixmldecryptor"></a><span data-ttu-id="34f2a-192">IXmlDecryptor</span><span class="sxs-lookup"><span data-stu-id="34f2a-192">IXmlDecryptor</span></span>

<span data-ttu-id="34f2a-193">Le `IXmlDecryptor` interface représente un type qui sait comment déchiffrer un `XElement` qui a été enciphered via un `IXmlEncryptor`.</span><span class="sxs-lookup"><span data-stu-id="34f2a-193">The `IXmlDecryptor` interface represents a type that knows how to decrypt an `XElement` that was enciphered via an `IXmlEncryptor`.</span></span> <span data-ttu-id="34f2a-194">Il expose une API unique :</span><span class="sxs-lookup"><span data-stu-id="34f2a-194">It exposes a single API:</span></span>

* <span data-ttu-id="34f2a-195">Déchiffrer (encryptedElement XElement) : XElement</span><span class="sxs-lookup"><span data-stu-id="34f2a-195">Decrypt(XElement encryptedElement) : XElement</span></span>

<span data-ttu-id="34f2a-196">Le `Decrypt` méthode annule le chiffrement effectué par `IXmlEncryptor.Encrypt`.</span><span class="sxs-lookup"><span data-stu-id="34f2a-196">The `Decrypt` method undoes the encryption performed by `IXmlEncryptor.Encrypt`.</span></span> <span data-ttu-id="34f2a-197">En règle générale, chaque concrète `IXmlEncryptor` implémentation aura un concrète correspondante `IXmlDecryptor` implémentation.</span><span class="sxs-lookup"><span data-stu-id="34f2a-197">Generally, each concrete `IXmlEncryptor` implementation will have a corresponding concrete `IXmlDecryptor` implementation.</span></span>

<span data-ttu-id="34f2a-198">Les types qui implémentent `IXmlDecryptor` doivent avoir l’une des deux constructeurs publics suivants :</span><span class="sxs-lookup"><span data-stu-id="34f2a-198">Types which implement `IXmlDecryptor` should have one of the following two public constructors:</span></span>

* <span data-ttu-id="34f2a-199">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="34f2a-199">.ctor(IServiceProvider)</span></span>
* <span data-ttu-id="34f2a-200">.ctor()</span><span class="sxs-lookup"><span data-stu-id="34f2a-200">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="34f2a-201">Le `IServiceProvider` passé au constructeur peut être null.</span><span class="sxs-lookup"><span data-stu-id="34f2a-201">The `IServiceProvider` passed to the constructor may be null.</span></span>

## <a name="ikeyescrowsink"></a><span data-ttu-id="34f2a-202">IKeyEscrowSink</span><span class="sxs-lookup"><span data-stu-id="34f2a-202">IKeyEscrowSink</span></span>

<span data-ttu-id="34f2a-203">Le `IKeyEscrowSink` interface représente un type qui peut effectuer le dépôt d’informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="34f2a-203">The `IKeyEscrowSink` interface represents a type that can perform escrow of sensitive information.</span></span> <span data-ttu-id="34f2a-204">N’oubliez pas que les descripteurs sérialisés peuvent contenir des informations sensibles (telles que le matériel de chiffrement), c’est ce qui a conduit à l’introduction de la [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) tapez en premier lieu.</span><span class="sxs-lookup"><span data-stu-id="34f2a-204">Recall that serialized descriptors might contain sensitive information (such as cryptographic material), and this is what led to the introduction of the [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) type in the first place.</span></span> <span data-ttu-id="34f2a-205">Toutefois, les accidents se produisent et anneaux de clé peut être supprimés ou endommagés.</span><span class="sxs-lookup"><span data-stu-id="34f2a-205">However, accidents happen, and key rings can be deleted or become corrupted.</span></span>

<span data-ttu-id="34f2a-206">L’interface (Key escrow) offre une urgence, autoriser l’accès pour le XML sérialisé brut avant sa transformation par aucune configuré [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span><span class="sxs-lookup"><span data-stu-id="34f2a-206">The escrow interface provides an emergency escape hatch, allowing access to the raw serialized XML before it's transformed by any configured [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span></span> <span data-ttu-id="34f2a-207">L’interface expose une API unique :</span><span class="sxs-lookup"><span data-stu-id="34f2a-207">The interface exposes a single API:</span></span>

* <span data-ttu-id="34f2a-208">Magasin (Guid keyId, élément de XElement)</span><span class="sxs-lookup"><span data-stu-id="34f2a-208">Store(Guid keyId, XElement element)</span></span>

<span data-ttu-id="34f2a-209">C’est à le `IKeyEscrowSink` implémentation pour gérer l’élément fourni de manière sécurisée cohérente avec la stratégie d’entreprise.</span><span class="sxs-lookup"><span data-stu-id="34f2a-209">It's up to the `IKeyEscrowSink` implementation to handle the provided element in a secure manner consistent with business policy.</span></span> <span data-ttu-id="34f2a-210">Une implémentation possible peut être pour le récepteur tiers de confiance chiffrer l’élément XML à l’aide d’un certificat X.509 d’entreprise connu où la clé du certificat privé a été déposée ; le `CertificateXmlEncryptor` type peut vous aider à cela.</span><span class="sxs-lookup"><span data-stu-id="34f2a-210">One possible implementation could be for the escrow sink to encrypt the XML element using a known corporate X.509 certificate where the certificate's private key has been escrowed; the `CertificateXmlEncryptor` type can assist with this.</span></span> <span data-ttu-id="34f2a-211">Le `IKeyEscrowSink` implémentation est également chargée de rendre l’élément fourni de manière appropriée.</span><span class="sxs-lookup"><span data-stu-id="34f2a-211">The `IKeyEscrowSink` implementation is also responsible for persisting the provided element appropriately.</span></span>

<span data-ttu-id="34f2a-212">Par défaut aucun mécanisme de tiers de confiance n’est activé, bien que les administrateurs de serveur peuvent [cette configuration globalement](xref:security/data-protection/configuration/machine-wide-policy).</span><span class="sxs-lookup"><span data-stu-id="34f2a-212">By default no escrow mechanism is enabled, though server administrators can [configure this globally](xref:security/data-protection/configuration/machine-wide-policy).</span></span> <span data-ttu-id="34f2a-213">Il peut également être configuré par programmation la `IDataProtectionBuilder.AddKeyEscrowSink` méthode comme indiqué dans l’exemple ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="34f2a-213">It can also be configured programmatically via the `IDataProtectionBuilder.AddKeyEscrowSink` method as shown in the sample below.</span></span> <span data-ttu-id="34f2a-214">Le `AddKeyEscrowSink` mise en miroir des surcharges de méthode le `IServiceCollection.AddSingleton` et `IServiceCollection.AddInstance` surcharges, comme `IKeyEscrowSink` instances sont destinées à être singletons.</span><span class="sxs-lookup"><span data-stu-id="34f2a-214">The `AddKeyEscrowSink` method overloads mirror the `IServiceCollection.AddSingleton` and `IServiceCollection.AddInstance` overloads, as `IKeyEscrowSink` instances are intended to be singletons.</span></span> <span data-ttu-id="34f2a-215">Si plusieurs `IKeyEscrowSink` inscrit des instances, chacun d’eux sera appelée lors de la génération de clés, donc les clés peuvent être déposées simultanément plusieurs mécanismes.</span><span class="sxs-lookup"><span data-stu-id="34f2a-215">If multiple `IKeyEscrowSink` instances are registered, each one will be called during key generation, so keys can be escrowed to multiple mechanisms simultaneously.</span></span>

<span data-ttu-id="34f2a-216">Il n’existe aucune API pour la lecture de documents à partir d’un `IKeyEscrowSink` instance.</span><span class="sxs-lookup"><span data-stu-id="34f2a-216">There's no API to read material from an `IKeyEscrowSink` instance.</span></span> <span data-ttu-id="34f2a-217">Cela est cohérent avec la théorie de conception du mécanisme de tiers de confiance : il a conçu améliorer le matériel de clé pour une autorité approuvée, et étant donné que l’application n’est lui-même pas une autorité approuvée, il ne doit pas avoir accès à son propre matériel de clés.</span><span class="sxs-lookup"><span data-stu-id="34f2a-217">This is consistent with the design theory of the escrow mechanism: it's intended to make the key material accessible to a trusted authority, and since the application is itself not a trusted authority, it shouldn't have access to its own escrowed material.</span></span>

<span data-ttu-id="34f2a-218">L’exemple de code suivant montre comment créer et inscrire un `IKeyEscrowSink` où les clés sont déposées telles que seuls les membres de « Administrateurs CONTOSODomain » pour les récupérer.</span><span class="sxs-lookup"><span data-stu-id="34f2a-218">The following sample code demonstrates creating and registering an `IKeyEscrowSink` where keys are escrowed such that only members of "CONTOSODomain Admins" can recover them.</span></span>

> [!NOTE]
> <span data-ttu-id="34f2a-219">Pour exécuter cet exemple, vous devez être sur un domaine Windows 8 / machine Windows Server 2012 et le contrôleur de domaine doivent être Windows Server 2012 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="34f2a-219">To run this sample, you must be on a domain-joined Windows 8 / Windows Server 2012 machine, and the domain controller must be Windows Server 2012 or later.</span></span>

[!code-csharp[Main](key-management/samples/key-management-extensibility.cs)]
