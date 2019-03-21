---
title: Extensibilité de chiffrement de base dans ASP.NET Core
author: rick-anderson
description: Découvrez IAuthenticatedEncryptor, IAuthenticatedEncryptorDescriptor, IAuthenticatedEncryptorDescriptorDeserializer et la fabrique de niveau supérieur.
ms.author: riande
ms.date: 8/11/2017
uid: security/data-protection/extensibility/core-crypto
ms.openlocfilehash: cf4a142992efe5b00a75285ef9ad9735fe7be411
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209068"
---
# <a name="core-cryptography-extensibility-in-aspnet-core"></a><span data-ttu-id="35740-103">Extensibilité de chiffrement de base dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="35740-103">Core cryptography extensibility in ASP.NET Core</span></span>

<a name="data-protection-extensibility-core-crypto"></a>

>[!WARNING]
> <span data-ttu-id="35740-104">Types qui implémentent les interfaces suivantes doivent être thread-safe pour les appelants plusieurs.</span><span class="sxs-lookup"><span data-stu-id="35740-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptor"></a>

## <a name="iauthenticatedencryptor"></a><span data-ttu-id="35740-105">IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="35740-105">IAuthenticatedEncryptor</span></span>

<span data-ttu-id="35740-106">Le **IAuthenticatedEncryptor** interface est le bloc de construction de base du sous-système de chiffrement.</span><span class="sxs-lookup"><span data-stu-id="35740-106">The **IAuthenticatedEncryptor** interface is the basic building block of the cryptographic subsystem.</span></span> <span data-ttu-id="35740-107">Il est généralement une IAuthenticatedEncryptor par clé, et l’instance IAuthenticatedEncryptor encapsule tous les clé de chiffrement et les informations algorithmiques nécessaires pour effectuer des opérations de chiffrement.</span><span class="sxs-lookup"><span data-stu-id="35740-107">There's generally one IAuthenticatedEncryptor per key, and the IAuthenticatedEncryptor instance wraps all cryptographic key material and algorithmic information necessary to perform cryptographic operations.</span></span>

<span data-ttu-id="35740-108">Comme son nom l’indique, le type est chargé de fournir des services de chiffrement et déchiffrement authentifiés.</span><span class="sxs-lookup"><span data-stu-id="35740-108">As its name suggests, the type is responsible for providing authenticated encryption and decryption services.</span></span> <span data-ttu-id="35740-109">Il expose les API de deux suivantes.</span><span class="sxs-lookup"><span data-stu-id="35740-109">It exposes the following two APIs.</span></span>

* `Decrypt(ArraySegment<byte> ciphertext, ArraySegment<byte> additionalAuthenticatedData) : byte[]`

* `Encrypt(ArraySegment<byte> plaintext, ArraySegment<byte> additionalAuthenticatedData) : byte[]`

<span data-ttu-id="35740-110">La méthode Encrypt retourne un objet blob qui inclut le texte en clair des et une balise d’authentification.</span><span class="sxs-lookup"><span data-stu-id="35740-110">The Encrypt method returns a blob that includes the enciphered plaintext and an authentication tag.</span></span> <span data-ttu-id="35740-111">La balise d’authentification doit englober les données authentifiées supplémentaires (AAD), bien que AAD lui-même ne sont pas nécessairement récupérable à partir de la charge utile finale.</span><span class="sxs-lookup"><span data-stu-id="35740-111">The authentication tag must encompass the additional authenticated data (AAD), though the AAD itself need not be recoverable from the final payload.</span></span> <span data-ttu-id="35740-112">La méthode Decrypt valide de la balise d’authentification et retourne la charge utile à déchiffrer.</span><span class="sxs-lookup"><span data-stu-id="35740-112">The Decrypt method validates the authentication tag and returns the deciphered payload.</span></span> <span data-ttu-id="35740-113">Tous les échecs (à l’exception ArgumentNullException et similaire) doivent être homogénéisés à CryptographicException.</span><span class="sxs-lookup"><span data-stu-id="35740-113">All failures (except ArgumentNullException and similar) should be homogenized to CryptographicException.</span></span>

> [!NOTE]
> <span data-ttu-id="35740-114">L’instance IAuthenticatedEncryptor lui-même n’a pas réellement besoin contenir le matériel de clé.</span><span class="sxs-lookup"><span data-stu-id="35740-114">The IAuthenticatedEncryptor instance itself doesn't actually need to contain the key material.</span></span> <span data-ttu-id="35740-115">Par exemple, l’implémentation pouvait déléguer à un module HSM pour toutes les opérations.</span><span class="sxs-lookup"><span data-stu-id="35740-115">For example, the implementation could delegate to an HSM for all operations.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptorfactory"></a>
<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="how-to-create-an-iauthenticatedencryptor"></a><span data-ttu-id="35740-116">Comment créer un IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="35740-116">How to create an IAuthenticatedEncryptor</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="35740-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="35740-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="35740-118">Le **IAuthenticatedEncryptorFactory** interface représente un type qui sait comment créer un [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span><span class="sxs-lookup"><span data-stu-id="35740-118">The **IAuthenticatedEncryptorFactory** interface represents a type that knows how to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span></span> <span data-ttu-id="35740-119">Voici son API.</span><span class="sxs-lookup"><span data-stu-id="35740-119">Its API is as follows.</span></span>

* <span data-ttu-id="35740-120">CreateEncryptorInstance (clé IKey) : IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="35740-120">CreateEncryptorInstance(IKey key) : IAuthenticatedEncryptor</span></span>

<span data-ttu-id="35740-121">Pour n’importe quelle instance IKey donné, n’importe quel chiffreurs authentifiés créés par sa méthode CreateEncryptorInstance doivent être considérées comme équivalentes, comme dans l’exemple de code ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="35740-121">For any given IKey instance, any authenticated encryptors created by its CreateEncryptorInstance method should be considered equivalent, as in the below code sample.</span></span>

```csharp
// we have an IAuthenticatedEncryptorFactory instance and an IKey instance
IAuthenticatedEncryptorFactory factory = ...;
IKey key = ...;

// get an encryptor instance and perform an authenticated encryption operation
ArraySegment<byte> plaintext = new ArraySegment<byte>(Encoding.UTF8.GetBytes("plaintext"));
ArraySegment<byte> aad = new ArraySegment<byte>(Encoding.UTF8.GetBytes("AAD"));
var encryptor1 = factory.CreateEncryptorInstance(key);
byte[] ciphertext = encryptor1.Encrypt(plaintext, aad);

// get another encryptor instance and perform an authenticated decryption operation
var encryptor2 = factory.CreateEncryptorInstance(key);
byte[] roundTripped = encryptor2.Decrypt(new ArraySegment<byte>(ciphertext), aad);


// the 'roundTripped' and 'plaintext' buffers should be equivalent
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="35740-122">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="35740-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="35740-123">Le **IAuthenticatedEncryptorDescriptor** interface représente un type qui sait comment créer un [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span><span class="sxs-lookup"><span data-stu-id="35740-123">The **IAuthenticatedEncryptorDescriptor** interface represents a type that knows how to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span></span> <span data-ttu-id="35740-124">Voici son API.</span><span class="sxs-lookup"><span data-stu-id="35740-124">Its API is as follows.</span></span>

* <span data-ttu-id="35740-125">CreateEncryptorInstance() : IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="35740-125">CreateEncryptorInstance() : IAuthenticatedEncryptor</span></span>

* <span data-ttu-id="35740-126">ExportToXml() : XmlSerializedDescriptorInfo</span><span class="sxs-lookup"><span data-stu-id="35740-126">ExportToXml() : XmlSerializedDescriptorInfo</span></span>

<span data-ttu-id="35740-127">Comme IAuthenticatedEncryptor, une instance de IAuthenticatedEncryptorDescriptor est supposée pour encapsuler une clé spécifique.</span><span class="sxs-lookup"><span data-stu-id="35740-127">Like IAuthenticatedEncryptor, an instance of IAuthenticatedEncryptorDescriptor is assumed to wrap one specific key.</span></span> <span data-ttu-id="35740-128">Cela signifie que pour n’importe quelle instance IAuthenticatedEncryptorDescriptor donné, n’importe quel chiffreurs authentifiés créés par sa méthode CreateEncryptorInstance doivent être considérés comme équivalents, comme dans l’exemple de code ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="35740-128">This means that for any given IAuthenticatedEncryptorDescriptor instance, any authenticated encryptors created by its CreateEncryptorInstance method should be considered equivalent, as in the below code sample.</span></span>

```csharp
// we have an IAuthenticatedEncryptorDescriptor instance
IAuthenticatedEncryptorDescriptor descriptor = ...;

// get an encryptor instance and perform an authenticated encryption operation
ArraySegment<byte> plaintext = new ArraySegment<byte>(Encoding.UTF8.GetBytes("plaintext"));
ArraySegment<byte> aad = new ArraySegment<byte>(Encoding.UTF8.GetBytes("AAD"));
var encryptor1 = descriptor.CreateEncryptorInstance();
byte[] ciphertext = encryptor1.Encrypt(plaintext, aad);

// get another encryptor instance and perform an authenticated decryption operation
var encryptor2 = descriptor.CreateEncryptorInstance();
byte[] roundTripped = encryptor2.Decrypt(new ArraySegment<byte>(ciphertext), aad);


// the 'roundTripped' and 'plaintext' buffers should be equivalent
```

---

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="iauthenticatedencryptordescriptor-aspnet-core-2x-only"></a><span data-ttu-id="35740-129">IAuthenticatedEncryptorDescriptor (ASP.NET Core 2.x uniquement)</span><span class="sxs-lookup"><span data-stu-id="35740-129">IAuthenticatedEncryptorDescriptor (ASP.NET Core 2.x only)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="35740-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="35740-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="35740-131">Le **IAuthenticatedEncryptorDescriptor** interface représente un type qui sait comment lui-même exporter au format XML.</span><span class="sxs-lookup"><span data-stu-id="35740-131">The **IAuthenticatedEncryptorDescriptor** interface represents a type that knows how to export itself to XML.</span></span> <span data-ttu-id="35740-132">Voici son API.</span><span class="sxs-lookup"><span data-stu-id="35740-132">Its API is as follows.</span></span>

* <span data-ttu-id="35740-133">ExportToXml() : XmlSerializedDescriptorInfo</span><span class="sxs-lookup"><span data-stu-id="35740-133">ExportToXml() : XmlSerializedDescriptorInfo</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="35740-134">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="35740-134">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

---

## <a name="xml-serialization"></a><span data-ttu-id="35740-135">Sérialisation XML</span><span class="sxs-lookup"><span data-stu-id="35740-135">XML Serialization</span></span>

<span data-ttu-id="35740-136">La principale différence entre IAuthenticatedEncryptor et IAuthenticatedEncryptorDescriptor est que le descripteur sait créer le chiffreur et lui fournir les arguments valides.</span><span class="sxs-lookup"><span data-stu-id="35740-136">The primary difference between IAuthenticatedEncryptor and IAuthenticatedEncryptorDescriptor is that the descriptor knows how to create the encryptor and supply it with valid arguments.</span></span> <span data-ttu-id="35740-137">Envisagez un IAuthenticatedEncryptor dont l’implémentation s’appuie sur SymmetricAlgorithm et élément KeyedHashAlgorithm impossible.</span><span class="sxs-lookup"><span data-stu-id="35740-137">Consider an IAuthenticatedEncryptor whose implementation relies on SymmetricAlgorithm and KeyedHashAlgorithm.</span></span> <span data-ttu-id="35740-138">Les travaux du chiffreur sont d’utiliser ces types, mais il ne sache pas nécessairement ces types de provenant, donc il ne peut pas vraiment écrire une description appropriée du comment recréer elle-même si l’application redémarre.</span><span class="sxs-lookup"><span data-stu-id="35740-138">The encryptor's job is to consume these types, but it doesn't necessarily know where these types came from, so it can't really write out a proper description of how to recreate itself if the application restarts.</span></span> <span data-ttu-id="35740-139">Le descripteur agit comme un niveau plus élevé en plus de cela.</span><span class="sxs-lookup"><span data-stu-id="35740-139">The descriptor acts as a higher level on top of this.</span></span> <span data-ttu-id="35740-140">Dans la mesure où le descripteur sait comment créer l’instance de chiffreur (par exemple, il sait comment créer les algorithmes requis), il peut sérialiser ces connaissances au format XML afin que l’instance de chiffreur peut être recréé après la réinitialisation d’une application.</span><span class="sxs-lookup"><span data-stu-id="35740-140">Since the descriptor knows how to create the encryptor instance (e.g., it knows how to create the required algorithms), it can serialize that knowledge in XML form so that the encryptor instance can be recreated after an application reset.</span></span>

<a name="data-protection-extensibility-core-crypto-exporttoxml"></a>

<span data-ttu-id="35740-141">Le descripteur peut être sérialisé par le biais de sa routine ExportToXml.</span><span class="sxs-lookup"><span data-stu-id="35740-141">The descriptor can be serialized via its ExportToXml routine.</span></span> <span data-ttu-id="35740-142">Cette routine retourne un XmlSerializedDescriptorInfo qui contient deux propriétés : la représentation sous forme de XElement de descripteur et le Type qui représente un [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) qui peut être utilisé pour ressusciter ce descripteur étant donné la XElement correspondant.</span><span class="sxs-lookup"><span data-stu-id="35740-142">This routine returns an XmlSerializedDescriptorInfo which contains two properties: the XElement representation of the descriptor and the Type which represents an [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) which can be used to resurrect this descriptor given the corresponding XElement.</span></span>

<span data-ttu-id="35740-143">Le descripteur sérialisé peut contenir des informations sensibles telles que de la clé de chiffrement.</span><span class="sxs-lookup"><span data-stu-id="35740-143">The serialized descriptor may contain sensitive information such as cryptographic key material.</span></span> <span data-ttu-id="35740-144">Le système de protection de données a une prise en charge intégrée pour le chiffrement des informations avant qu’il a rendu persistant dans le stockage.</span><span class="sxs-lookup"><span data-stu-id="35740-144">The data protection system has built-in support for encrypting information before it's persisted to storage.</span></span> <span data-ttu-id="35740-145">Pour tirer parti de cela, le descripteur doit marquer l’élément qui contient des informations sensibles avec l’attribut nom « RequiresEncryption, » (xmlns «<http://schemas.asp.net/2015/03/dataProtection>»), valeur « true ».</span><span class="sxs-lookup"><span data-stu-id="35740-145">To take advantage of this, the descriptor should mark the element which contains sensitive information with the attribute name "requiresEncryption" (xmlns "<http://schemas.asp.net/2015/03/dataProtection>"), value "true".</span></span>

>[!TIP]
> <span data-ttu-id="35740-146">Il est une API d’assistance permettant de définir cet attribut.</span><span class="sxs-lookup"><span data-stu-id="35740-146">There's a helper API for setting this attribute.</span></span> <span data-ttu-id="35740-147">Appelez la méthode d’extension que XElement.markasrequiresencryption() situé dans l’espace de noms Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel.</span><span class="sxs-lookup"><span data-stu-id="35740-147">Call the extension method XElement.MarkAsRequiresEncryption() located in namespace Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel.</span></span>

<span data-ttu-id="35740-148">Il peut également arriver où le descripteur sérialisé ne contient pas d’informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="35740-148">There can also be cases where the serialized descriptor doesn't contain sensitive information.</span></span> <span data-ttu-id="35740-149">Reprenons le cas d’une clé de chiffrement stockée dans un HSM.</span><span class="sxs-lookup"><span data-stu-id="35740-149">Consider again the case of a cryptographic key stored in an HSM.</span></span> <span data-ttu-id="35740-150">Le matériel de clé ne peut pas écrire le descripteur lors de la sérialisation lui-même dans la mesure où le module HSM ne sont pas exposer le matériel sous forme de texte en clair.</span><span class="sxs-lookup"><span data-stu-id="35740-150">The descriptor cannot write out the key material when serializing itself since the HSM won't expose the material in plaintext form.</span></span> <span data-ttu-id="35740-151">Au lieu de cela, le descripteur de peut écrire la version encapsulé à la clé de la clé (si le module HSM autorise l’exportation de cette manière) ou l’identificateur unique du HSM pour la clé.</span><span class="sxs-lookup"><span data-stu-id="35740-151">Instead, the descriptor might write out the key-wrapped version of the key (if the HSM allows export in this fashion) or the HSM's own unique identifier for the key.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer"></a>

## <a name="iauthenticatedencryptordescriptordeserializer"></a><span data-ttu-id="35740-152">IAuthenticatedEncryptorDescriptorDeserializer</span><span class="sxs-lookup"><span data-stu-id="35740-152">IAuthenticatedEncryptorDescriptorDeserializer</span></span>

<span data-ttu-id="35740-153">Le **IAuthenticatedEncryptorDescriptorDeserializer** interface représente un type qui sait comment désérialiser une instance de IAuthenticatedEncryptorDescriptor à partir d’un XElement.</span><span class="sxs-lookup"><span data-stu-id="35740-153">The **IAuthenticatedEncryptorDescriptorDeserializer** interface represents a type that knows how to deserialize an IAuthenticatedEncryptorDescriptor instance from an XElement.</span></span> <span data-ttu-id="35740-154">Elle expose une méthode unique :</span><span class="sxs-lookup"><span data-stu-id="35740-154">It exposes a single method:</span></span>

* <span data-ttu-id="35740-155">ImportFromXml (élément XElement) : IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="35740-155">ImportFromXml(XElement element) : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="35740-156">La méthode ImportFromXml prend XElement qui a été retourné par [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) et crée un équivalent de la IAuthenticatedEncryptorDescriptor d’origine.</span><span class="sxs-lookup"><span data-stu-id="35740-156">The ImportFromXml method takes the XElement that was returned by [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) and creates an equivalent of the original IAuthenticatedEncryptorDescriptor.</span></span>

<span data-ttu-id="35740-157">Les types qui implémentent IAuthenticatedEncryptorDescriptorDeserializer doivent avoir une des deux constructeurs publics :</span><span class="sxs-lookup"><span data-stu-id="35740-157">Types which implement IAuthenticatedEncryptorDescriptorDeserializer should have one of the following two public constructors:</span></span>

* <span data-ttu-id="35740-158">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="35740-158">.ctor(IServiceProvider)</span></span>

* <span data-ttu-id="35740-159">.ctor()</span><span class="sxs-lookup"><span data-stu-id="35740-159">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="35740-160">Le IServiceProvider passé au constructeur peut être null.</span><span class="sxs-lookup"><span data-stu-id="35740-160">The IServiceProvider passed to the constructor may be null.</span></span>

## <a name="the-top-level-factory"></a><span data-ttu-id="35740-161">La fabrique de niveau supérieur</span><span class="sxs-lookup"><span data-stu-id="35740-161">The top-level factory</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="35740-162">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="35740-162">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="35740-163">Le **AlgorithmConfiguration** classe représente un type qui sait comment créer [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instances.</span><span class="sxs-lookup"><span data-stu-id="35740-163">The **AlgorithmConfiguration** class represents a type which knows how to create [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instances.</span></span> <span data-ttu-id="35740-164">Il expose une API unique.</span><span class="sxs-lookup"><span data-stu-id="35740-164">It exposes a single API.</span></span>

* <span data-ttu-id="35740-165">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="35740-165">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="35740-166">Considérez AlgorithmConfiguration comme la fabrique de niveau supérieur.</span><span class="sxs-lookup"><span data-stu-id="35740-166">Think of AlgorithmConfiguration as the top-level factory.</span></span> <span data-ttu-id="35740-167">La configuration sert de modèle.</span><span class="sxs-lookup"><span data-stu-id="35740-167">The configuration serves as a template.</span></span> <span data-ttu-id="35740-168">Elle encapsule les informations algorithmiques (par exemple, cette configuration donne des descripteurs avec une clé principale de AES-128-GCM), mais elle n’est pas encore associée avec une clé spécifique.</span><span class="sxs-lookup"><span data-stu-id="35740-168">It wraps algorithmic information (e.g., this configuration produces descriptors with an AES-128-GCM master key), but it's not yet associated with a specific key.</span></span>

<span data-ttu-id="35740-169">Lorsque CreateNewDescriptor est appelée, nouveau matériel de clé est créée uniquement pour cet appel, et un nouveau IAuthenticatedEncryptorDescriptor est généré qui encapsule ce matériel de clé et les informations algorithmiques nécessaire pour utiliser le matériel.</span><span class="sxs-lookup"><span data-stu-id="35740-169">When CreateNewDescriptor is called, fresh key material is created solely for this call, and a new IAuthenticatedEncryptorDescriptor is produced which wraps this key material and the algorithmic information required to consume the material.</span></span> <span data-ttu-id="35740-170">Le matériel de clé peut être créé dans le logiciel (et conservé en mémoire), il peut être créé et maintenu dans un HSM et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="35740-170">The key material could be created in software (and held in memory), it could be created and held within an HSM, and so on.</span></span> <span data-ttu-id="35740-171">Le point essentiel est que tous les deux appels à CreateNewDescriptor ne devraient jamais créer des instances IAuthenticatedEncryptorDescriptor équivalentes.</span><span class="sxs-lookup"><span data-stu-id="35740-171">The crucial point is that any two calls to CreateNewDescriptor should never create equivalent IAuthenticatedEncryptorDescriptor instances.</span></span>

<span data-ttu-id="35740-172">Le type AlgorithmConfiguration sert tels que le point d’entrée pour les routines de création de la clé [clé automatique propagée](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling).</span><span class="sxs-lookup"><span data-stu-id="35740-172">The AlgorithmConfiguration type serves as the entry point for key creation routines such as [automatic key rolling](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling).</span></span> <span data-ttu-id="35740-173">Pour modifier l’implémentation pour toutes les clés de futures, définissez la propriété de AuthenticatedEncryptorConfiguration dans KeyManagementOptions.</span><span class="sxs-lookup"><span data-stu-id="35740-173">To change the implementation for all future keys, set the AuthenticatedEncryptorConfiguration property in KeyManagementOptions.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="35740-174">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="35740-174">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="35740-175">Le **IAuthenticatedEncryptorConfiguration** interface représente un type qui sait comment créer [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instances.</span><span class="sxs-lookup"><span data-stu-id="35740-175">The **IAuthenticatedEncryptorConfiguration** interface represents a type which knows how to create [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instances.</span></span> <span data-ttu-id="35740-176">Il expose une API unique.</span><span class="sxs-lookup"><span data-stu-id="35740-176">It exposes a single API.</span></span>

* <span data-ttu-id="35740-177">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="35740-177">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="35740-178">Considérez IAuthenticatedEncryptorConfiguration comme la fabrique de niveau supérieur.</span><span class="sxs-lookup"><span data-stu-id="35740-178">Think of IAuthenticatedEncryptorConfiguration as the top-level factory.</span></span> <span data-ttu-id="35740-179">La configuration sert de modèle.</span><span class="sxs-lookup"><span data-stu-id="35740-179">The configuration serves as a template.</span></span> <span data-ttu-id="35740-180">Elle encapsule les informations algorithmiques (par exemple, cette configuration donne des descripteurs avec une clé principale de AES-128-GCM), mais elle n’est pas encore associée avec une clé spécifique.</span><span class="sxs-lookup"><span data-stu-id="35740-180">It wraps algorithmic information (e.g., this configuration produces descriptors with an AES-128-GCM master key), but it's not yet associated with a specific key.</span></span>

<span data-ttu-id="35740-181">Lorsque CreateNewDescriptor est appelée, nouveau matériel de clé est créée uniquement pour cet appel, et un nouveau IAuthenticatedEncryptorDescriptor est généré qui encapsule ce matériel de clé et les informations algorithmiques nécessaire pour utiliser le matériel.</span><span class="sxs-lookup"><span data-stu-id="35740-181">When CreateNewDescriptor is called, fresh key material is created solely for this call, and a new IAuthenticatedEncryptorDescriptor is produced which wraps this key material and the algorithmic information required to consume the material.</span></span> <span data-ttu-id="35740-182">Le matériel de clé peut être créé dans le logiciel (et conservé en mémoire), il peut être créé et maintenu dans un HSM et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="35740-182">The key material could be created in software (and held in memory), it could be created and held within an HSM, and so on.</span></span> <span data-ttu-id="35740-183">Le point essentiel est que tous les deux appels à CreateNewDescriptor ne devraient jamais créer des instances IAuthenticatedEncryptorDescriptor équivalentes.</span><span class="sxs-lookup"><span data-stu-id="35740-183">The crucial point is that any two calls to CreateNewDescriptor should never create equivalent IAuthenticatedEncryptorDescriptor instances.</span></span>

<span data-ttu-id="35740-184">Le type IAuthenticatedEncryptorConfiguration sert tels que le point d’entrée pour les routines de création de la clé [clé automatique propagée](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling).</span><span class="sxs-lookup"><span data-stu-id="35740-184">The IAuthenticatedEncryptorConfiguration type serves as the entry point for key creation routines such as [automatic key rolling](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling).</span></span> <span data-ttu-id="35740-185">Pour modifier l’implémentation pour toutes les clés de futures, inscrire un singleton IAuthenticatedEncryptorConfiguration dans le conteneur de service.</span><span class="sxs-lookup"><span data-stu-id="35740-185">To change the implementation for all future keys, register a singleton IAuthenticatedEncryptorConfiguration in the service container.</span></span>

---
