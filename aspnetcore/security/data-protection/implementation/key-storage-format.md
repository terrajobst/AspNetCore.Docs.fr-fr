---
title: Format de stockage de clés dans ASP.NET Core
author: rick-anderson
description: Découvrez les détails d’implémentation du format de stockage de clés de Protection des données ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: bca19ad001dd20b5d02ae5470f7d928082496037
ms.sourcegitcommit: 8f8924ce4eb9effeaf489f177fb01b66867da16f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39219275"
---
# <a name="key-storage-format-in-aspnet-core"></a>Format de stockage de clés dans ASP.NET Core

<a name="data-protection-implementation-key-storage-format"></a>

Les objets sont stockées au repos dans la représentation XML. Le répertoire par défaut pour le stockage de clés est % LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.

## <a name="the-key-element"></a>Le \<clé > élément

Clés existent en tant qu’objets de niveau supérieur dans le référentiel de clé. Par convention, les clés ont le nom de fichier **key-{guid} .xml**, où {guid} est l’id de la clé. Chacun de ces fichiers contient une clé unique. Voici le format du fichier.

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

Le \<clé > élément contient les attributs suivants et les éléments enfants :

* L’id de clé. Cette valeur est traitée comme faisant autorité ; le nom de fichier est simplement une subtilité pour une meilleure lisibilité humaine.

* La version de la \<clé > élément, actuellement fixée à 1.

* Dates de création, l’activation et l’expiration de la clé.

* Un \<descripteur > élément, qui contient des informations sur l’implémentation du chiffrement authentifié contenue dans cette clé.

Dans l’exemple ci-dessus, l’id de la clé est {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, il a été créé et activé sur le 19 mars 2015, et il a une durée de vie de 90 jours. (Il peut arriver que la date d’activation peut être légèrement avant la date de création, comme dans cet exemple. Cela est dû à un CD/m² dans le fonctionnement des API et ne présente aucun danger dans la pratique.)

## <a name="the-descriptor-element"></a>Le \<descripteur > élément

Externe \<descripteur > élément contient un attribut deserializerType, qui est le nom qualifié d’assembly d’un type qui implémente IAuthenticatedEncryptorDescriptorDeserializer. Ce type est responsable de lire les interne \<descripteur > élément et pour analyser les informations contenues dans.

Le format particulier de la \<descripteur > élément dépend de l’implémentation de chiffreur authentifié encapsulée par la clé, et chaque type de désérialiseur attend un format légèrement différent pour cela. En général, cependant, cet élément contiendra des informations algorithmiques (les noms, types, OID, ou similaire) et le matériel de clé secrète. Dans l’exemple ci-dessus, le descripteur spécifie que cette clé encapsule le chiffrement AES-256-CBC + HMACSHA256 validation.

## <a name="the-encryptedsecret-element"></a>Le \<encryptedSecret > élément

Un **&lt;encryptedSecret&gt;** élément qui contient la forme chiffrée de la clé secrète peut être présent si [le chiffrement des secrets au repos est activé](xref:security/data-protection/implementation/key-encryption-at-rest). L’attribut `decryptorType` est le nom qualifié d’assembly d’un type qui implémente [IXmlDecryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmldecryptor). Ce type est responsable de lire les interne **&lt;encryptedKey&gt;** élément et le déchiffrement pour récupérer le texte brut d’origine.

Comme avec \<descripteur >, le format particulier de la <encryptedSecret> élément varie selon le mécanisme de chiffrement au repos en cours d’utilisation. Dans l’exemple ci-dessus, la clé principale est chiffrée à l’aide de DPAPI de Windows par le commentaire.

## <a name="the-revocation-element"></a>Le \<révocation > élément

Révocations existent en tant qu’objets de niveau supérieur dans le référentiel de clé. Par convention révocations ont le nom de fichier **révocation-{horodatage} .xml** (pour la révocation de toutes les clés avant une date spécifique) ou **révocation-{guid} .xml** (pour la révocation d’une clé spécifique). Chaque fichier contienne un seul \<révocation > élément.

Pour les révocations de clés individuelles, le contenu du fichier sera comme suit.

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

Dans ce cas, uniquement la clé spécifiée est révoquée. Si l’id de clé est « * », toutefois, comme dans l’exemple ci-dessous, dont la date de création est antérieure à la date spécifiée de la révocation de toutes les clés sont révoquées.

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

Le \<raison > élément n’est jamais lue par le système. Il est simplement un emplacement pratique pour stocker une raison explicite de la révocation.
