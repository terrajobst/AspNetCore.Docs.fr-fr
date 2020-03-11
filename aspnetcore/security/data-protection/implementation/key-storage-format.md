---
title: Format de stockage des clés dans ASP.NET Core
author: rick-anderson
description: Découvrez les détails de l’implémentation du format de stockage de la clé de protection des données ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: 81df124f3dd0cadf8fd895ab55f66eec6415705f
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667754"
---
# <a name="key-storage-format-in-aspnet-core"></a><span data-ttu-id="641dd-103">Format de stockage des clés dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="641dd-103">Key storage format in ASP.NET Core</span></span>

<a name="data-protection-implementation-key-storage-format"></a>

<span data-ttu-id="641dd-104">Les objets sont stockés au repos dans une représentation XML.</span><span class="sxs-lookup"><span data-stu-id="641dd-104">Objects are stored at rest in XML representation.</span></span> <span data-ttu-id="641dd-105">Le répertoire par défaut pour le stockage de clés est%LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.</span><span class="sxs-lookup"><span data-stu-id="641dd-105">The default directory for key storage is %LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.</span></span>

## <a name="the-key-element"></a><span data-ttu-id="641dd-106">Élément key > \<</span><span class="sxs-lookup"><span data-stu-id="641dd-106">The \<key> element</span></span>

<span data-ttu-id="641dd-107">Les clés existent en tant qu’objets de niveau supérieur dans le dépôt de clé.</span><span class="sxs-lookup"><span data-stu-id="641dd-107">Keys exist as top-level objects in the key repository.</span></span> <span data-ttu-id="641dd-108">Les clés de Convention ont la clé de nom de fichier **-{GUID}. xml**, où {GUID} est l’ID de la clé.</span><span class="sxs-lookup"><span data-stu-id="641dd-108">By convention keys have the filename **key-{guid}.xml**, where {guid} is the id of the key.</span></span> <span data-ttu-id="641dd-109">Chaque fichier de ce type contient une clé unique.</span><span class="sxs-lookup"><span data-stu-id="641dd-109">Each such file contains a single key.</span></span> <span data-ttu-id="641dd-110">Le format du fichier est le suivant.</span><span class="sxs-lookup"><span data-stu-id="641dd-110">The format of the file is as follows.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<key id="80732141-ec8f-4b80-af9c-c4d2d1ff8901" version="1">
  <creationDate>2015-03-19T23:32:02.3949887Z</creationDate>
  <activationDate>2015-03-19T23:32:02.3839429Z</activationDate>
  <expirationDate>2015-06-17T23:32:02.3839429Z</expirationDate>
  <descriptor deserializerType="{deserializerType}">
    <descriptor>
      <encryption algorithm="AES_256_CBC" />
      <validation algorithm="HMACSHA256" />
      <enc:encryptedSecret decryptorType="{decryptorType}" xmlns:enc="...">
        <encryptedKey>
          <!-- This key is encrypted with Windows DPAPI. -->
          <value>AQAAANCM...8/zeP8lcwAg==</value>
        </encryptedKey>
      </enc:encryptedSecret>
    </descriptor>
  </descriptor>
</key>
```

<span data-ttu-id="641dd-111">L’élément key > \<contient les attributs et les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="641dd-111">The \<key> element contains the following attributes and child elements:</span></span>

* <span data-ttu-id="641dd-112">ID de la clé. Cette valeur est traitée comme faisant autorité ; le nom de fichier est simplement un nicety pour la lisibilité humaine.</span><span class="sxs-lookup"><span data-stu-id="641dd-112">The key id. This value is treated as authoritative; the filename is simply a nicety for human readability.</span></span>

* <span data-ttu-id="641dd-113">Version de la clé \<> élément, actuellement fixée à 1.</span><span class="sxs-lookup"><span data-stu-id="641dd-113">The version of the \<key> element, currently fixed at 1.</span></span>

* <span data-ttu-id="641dd-114">Dates de création, d’activation et d’expiration de la clé.</span><span class="sxs-lookup"><span data-stu-id="641dd-114">The key's creation, activation, and expiration dates.</span></span>

* <span data-ttu-id="641dd-115">Élément de descripteur de \<>, qui contient des informations sur l’implémentation de chiffrement authentifié contenue dans cette clé.</span><span class="sxs-lookup"><span data-stu-id="641dd-115">A \<descriptor> element, which contains information on the authenticated encryption implementation contained within this key.</span></span>

<span data-ttu-id="641dd-116">Dans l’exemple ci-dessus, l’ID de la clé est {80732141-ec8f-4B80-af9c-c4d2d1ff8901}, il a été créé et activé le 19 mars 2015, et sa durée de vie est de 90 jours.</span><span class="sxs-lookup"><span data-stu-id="641dd-116">In the above example, the key's id is {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, it was created and activated on March 19, 2015, and it has a lifetime of 90 days.</span></span> <span data-ttu-id="641dd-117">(Parfois, la date d’activation peut être légèrement antérieure à la date de création, comme dans cet exemple.</span><span class="sxs-lookup"><span data-stu-id="641dd-117">(Occasionally the activation date might be slightly before the creation date as in this example.</span></span> <span data-ttu-id="641dd-118">Cela est dû à un Nil dans la manière dont les API fonctionnent et est sans conséquence dans la pratique.)</span><span class="sxs-lookup"><span data-stu-id="641dd-118">This is due to a nit in how the APIs work and is harmless in practice.)</span></span>

## <a name="the-descriptor-element"></a><span data-ttu-id="641dd-119">Élément > descripteur de \<</span><span class="sxs-lookup"><span data-stu-id="641dd-119">The \<descriptor> element</span></span>

<span data-ttu-id="641dd-120">L’élément de descripteur de \<externe > contient un attribut deserializerType, qui est le nom qualifié d’assembly d’un type qui implémente IAuthenticatedEncryptorDescriptorDeserializer.</span><span class="sxs-lookup"><span data-stu-id="641dd-120">The outer \<descriptor> element contains an attribute deserializerType, which is the assembly-qualified name of a type which implements IAuthenticatedEncryptorDescriptorDeserializer.</span></span> <span data-ttu-id="641dd-121">Ce type est chargé de lire le descripteur de \<interne > élément et d’analyser les informations contenues dans.</span><span class="sxs-lookup"><span data-stu-id="641dd-121">This type is responsible for reading the inner \<descriptor> element and for parsing the information contained within.</span></span>

<span data-ttu-id="641dd-122">Le format particulier du descripteur de \<> élément dépend de l’implémentation de chiffreur authentifiée encapsulée par la clé, et chaque type de désérialiseur attend un format légèrement différent pour ce.</span><span class="sxs-lookup"><span data-stu-id="641dd-122">The particular format of the \<descriptor> element depends on the authenticated encryptor implementation encapsulated by the key, and each deserializer type expects a slightly different format for this.</span></span> <span data-ttu-id="641dd-123">En général, cependant, cet élément contient des informations algorithmiques (noms, types, OID ou similaires) et un matériel de clé secrète.</span><span class="sxs-lookup"><span data-stu-id="641dd-123">In general, though, this element will contain algorithmic information (names, types, OIDs, or similar) and secret key material.</span></span> <span data-ttu-id="641dd-124">Dans l’exemple ci-dessus, le descripteur spécifie que cette clé encapsule AES-256-CBC Encryption + HMACSHA256 validation.</span><span class="sxs-lookup"><span data-stu-id="641dd-124">In the above example, the descriptor specifies that this key wraps AES-256-CBC encryption + HMACSHA256 validation.</span></span>

## <a name="the-encryptedsecret-element"></a><span data-ttu-id="641dd-125">Élément \<encryptedSecret ></span><span class="sxs-lookup"><span data-stu-id="641dd-125">The \<encryptedSecret> element</span></span>

<span data-ttu-id="641dd-126">Un élément **&lt;encryptedSecret&gt;** contenant la forme chiffrée du matériel de clé secrète peut être présent si le [chiffrement des secrets au repos est activé](xref:security/data-protection/implementation/key-encryption-at-rest).</span><span class="sxs-lookup"><span data-stu-id="641dd-126">An **&lt;encryptedSecret&gt;** element which contains the encrypted form of the secret key material may be present if [encryption of secrets at rest is enabled](xref:security/data-protection/implementation/key-encryption-at-rest).</span></span> <span data-ttu-id="641dd-127">L’attribut `decryptorType` est le nom qualifié d’assembly d’un type qui implémente [IXmlDecryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmldecryptor).</span><span class="sxs-lookup"><span data-stu-id="641dd-127">The attribute `decryptorType` is the assembly-qualified name of a type which implements [IXmlDecryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmldecryptor).</span></span> <span data-ttu-id="641dd-128">Ce type est chargé de lire l’élément **&gt;interne&lt;EncryptedKey** et de le déchiffrer pour récupérer le texte en clair d’origine.</span><span class="sxs-lookup"><span data-stu-id="641dd-128">This type is responsible for reading the inner **&lt;encryptedKey&gt;** element and decrypting it to recover the original plaintext.</span></span>

<span data-ttu-id="641dd-129">Comme avec `<descriptor>`, le format particulier de l’élément `<encryptedSecret>` dépend du mécanisme de chiffrement au repos utilisé.</span><span class="sxs-lookup"><span data-stu-id="641dd-129">As with `<descriptor>`, the particular format of the `<encryptedSecret>` element depends on the at-rest encryption mechanism in use.</span></span> <span data-ttu-id="641dd-130">Dans l’exemple ci-dessus, la clé principale est chiffrée à l’aide de Windows DPAPI pour le commentaire.</span><span class="sxs-lookup"><span data-stu-id="641dd-130">In the above example, the master key is encrypted using Windows DPAPI per the comment.</span></span>

## <a name="the-revocation-element"></a><span data-ttu-id="641dd-131">Élément > de révocation \<</span><span class="sxs-lookup"><span data-stu-id="641dd-131">The \<revocation> element</span></span>

<span data-ttu-id="641dd-132">Les révocations existent en tant qu’objets de niveau supérieur dans le dépôt de clé.</span><span class="sxs-lookup"><span data-stu-id="641dd-132">Revocations exist as top-level objects in the key repository.</span></span> <span data-ttu-id="641dd-133">Par Convention, les révocations de nom de fichier **-{timestamp}. xml** (pour révoquer toutes les clés avant une date spécifique) ou la **révocation-{GUID}. xml** (pour révoquer une clé spécifique).</span><span class="sxs-lookup"><span data-stu-id="641dd-133">By convention revocations have the filename **revocation-{timestamp}.xml** (for revoking all keys before a specific date) or **revocation-{guid}.xml** (for revoking a specific key).</span></span> <span data-ttu-id="641dd-134">Chaque fichier contient un seul élément de révocation \<>.</span><span class="sxs-lookup"><span data-stu-id="641dd-134">Each file contains a single \<revocation> element.</span></span>

<span data-ttu-id="641dd-135">Pour les révocations de clés individuelles, le contenu du fichier se présente comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="641dd-135">For revocations of individual keys, the file contents will be as below.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="641dd-136">Dans ce cas, seule la clé spécifiée est révoquée.</span><span class="sxs-lookup"><span data-stu-id="641dd-136">In this case, only the specified key is revoked.</span></span> <span data-ttu-id="641dd-137">Toutefois, si l’ID de clé est « \* », comme dans l’exemple ci-dessous, toutes les clés dont la date de création est antérieure à la date de révocation spécifiée sont révoquées.</span><span class="sxs-lookup"><span data-stu-id="641dd-137">If the key id is "\*", however, as in the below example, all keys whose creation date is prior to the specified revocation date are revoked.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="641dd-138">La raison \<> élément n’est jamais lu par le système.</span><span class="sxs-lookup"><span data-stu-id="641dd-138">The \<reason> element is never read by the system.</span></span> <span data-ttu-id="641dd-139">Il s’agit simplement d’un emplacement pratique pour stocker une raison explicite de révocation.</span><span class="sxs-lookup"><span data-stu-id="641dd-139">It's simply a convenient place to store a human-readable reason for revocation.</span></span>
