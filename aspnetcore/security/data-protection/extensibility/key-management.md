---
title: Extensibilité de la gestion de clés dans ASP.NET Core
author: rick-anderson
description: En savoir plus sur l’extensibilité de la gestion de clés de Protection des données ASP.NET Core.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: 28932cbef1cc797338980f3e0de8b09caee324c0
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78665878"
---
# <a name="key-management-extensibility-in-aspnet-core"></a>Extensibilité de la gestion de clés dans ASP.NET Core

> [!TIP]
> Lisez la section [gestion des clés](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) avant de lire cette section, car elle explique certains concepts fondamentaux sous-jacents à ces API.

> [!WARNING]
> Types qui implémentent les interfaces suivantes doivent être thread-safe pour les appelants plusieurs.

## <a name="key"></a>Clé

L’interface `IKey` est la représentation de base d’une clé dans chiffrement. L’expression clé est utilisé ici dans le point de vue abstrait, pas dans le sens littéral de « clé de chiffrement ». Une clé a les propriétés suivantes :

* Dates d’activation, la création et l’expiration

* État de révocation.

* Identificateur de clé (un GUID)

::: moniker range=">= aspnetcore-2.0"

En outre, `IKey` expose une méthode `CreateEncryptor` qui peut être utilisée pour créer une instance [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) liée à cette clé.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

En outre, `IKey` expose une méthode `CreateEncryptorInstance` qui peut être utilisée pour créer une instance [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) liée à cette clé.

::: moniker-end

> [!NOTE]
> Il n’existe pas d’API permettant de récupérer le matériel de chiffrement brut à partir d’une instance de `IKey`.

## <a name="ikeymanager"></a>IKeyManager

L’interface `IKeyManager` représente un objet responsable du stockage, de la récupération et de la manipulation des clés générales. Elle expose trois opérations de haut niveau :

* Créer une nouvelle clé et conserver dans le stockage.

* Obtenir toutes les clés de stockage.

* Révoquer une ou plusieurs clés et conserver les informations de révocation dans le stockage.

>[!WARNING]
> L’écriture d’une `IKeyManager` est une tâche très avancée et la majorité des développeurs ne doivent pas la tenter. Au lieu de cela, la plupart des développeurs doivent tirer parti des fonctionnalités offertes par la classe [XmlKeyManager](#xmlkeymanager) .

## <a name="xmlkeymanager"></a>XmlKeyManager

Le type de `XmlKeyManager` est l’implémentation concrète de `IKeyManager`. Il fournit plusieurs installations utiles, y compris le dépôt de clé et le chiffrement des clés au repos. Les clés de ce système sont représentées en tant qu’éléments XML (en particulier, [XElement](/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).

`XmlKeyManager` dépend de plusieurs autres composants dans le cadre de la réalisation de ses tâches :

::: moniker range=">= aspnetcore-2.0"

* `AlgorithmConfiguration`, qui dicte les algorithmes utilisés par les nouvelles clés.

* `IXmlRepository`, qui contrôle où les clés sont conservées dans le stockage.

* `IXmlEncryptor` [facultatif], qui autorise le chiffrement des clés au repos.

* `IKeyEscrowSink` [facultatif], qui fournit des services de dépôt de clés.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* `IXmlRepository`, qui contrôle où les clés sont conservées dans le stockage.

* `IXmlEncryptor` [facultatif], qui autorise le chiffrement des clés au repos.

* `IKeyEscrowSink` [facultatif], qui fournit des services de dépôt de clés.

::: moniker-end

Vous trouverez ci-dessous des diagrammes de haut niveau qui indiquent comment ces composants sont reliés entre eux dans `XmlKeyManager`.

::: moniker range=">= aspnetcore-2.0"

![Création de la clé](key-management/_static/keycreation2.png)

*Création de clé/CreateNewKey*

Dans l’implémentation de `CreateNewKey`, le composant `AlgorithmConfiguration` est utilisé pour créer un `IAuthenticatedEncryptorDescriptor`unique, qui est ensuite sérialisé au format XML. Si un récepteur de dépôt de clé est présent, les données XML brutes (non chiffré) est fourni pour le récepteur pour le stockage à long terme. Le XML non chiffré est ensuite exécuté via un `IXmlEncryptor` (si nécessaire) pour générer le document XML chiffré. Ce document chiffré est conservé dans un stockage à long terme via le `IXmlRepository`. (Si aucune `IXmlEncryptor` n’est configurée, le document non chiffré est conservé dans le `IXmlRepository`.)

![Récupération de clé](key-management/_static/keyretrieval2.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

![Création de la clé](key-management/_static/keycreation1.png)

*Création de clé/CreateNewKey*

Dans l’implémentation de `CreateNewKey`, le composant `IAuthenticatedEncryptorConfiguration` est utilisé pour créer un `IAuthenticatedEncryptorDescriptor`unique, qui est ensuite sérialisé au format XML. Si un récepteur de dépôt de clé est présent, les données XML brutes (non chiffré) est fourni pour le récepteur pour le stockage à long terme. Le XML non chiffré est ensuite exécuté via un `IXmlEncryptor` (si nécessaire) pour générer le document XML chiffré. Ce document chiffré est conservé dans un stockage à long terme via le `IXmlRepository`. (Si aucune `IXmlEncryptor` n’est configurée, le document non chiffré est conservé dans le `IXmlRepository`.)

![Récupération de clé](key-management/_static/keyretrieval1.png)

::: moniker-end

*Récupération de clé/GetAllKeys*

Dans l’implémentation de `GetAllKeys`, les documents XML représentant les clés et les révocations sont lus à partir des `IXmlRepository`sous-jacentes. Si ces documents sont chiffrées, le système sera automatiquement les déchiffrer. `XmlKeyManager` crée les instances de `IAuthenticatedEncryptorDescriptorDeserializer` appropriées pour désérialiser les documents dans des instances de `IAuthenticatedEncryptorDescriptor`, qui sont ensuite encapsulées dans des instances de `IKey` individuelles. Cette collection d’instances de `IKey` est retournée à l’appelant.

Vous trouverez plus d’informations sur les éléments XML particuliers dans le [document de format de stockage de clés](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).

## <a name="ixmlrepository"></a>IXmlRepository

L’interface `IXmlRepository` représente un type qui peut conserver XML dans un magasin de stockage et récupérer du code XML. Elle expose deux API :

* `GetAllElements` :`IReadOnlyCollection<XElement>`

* `StoreElement(XElement element, string friendlyName)`

Les implémentations de `IXmlRepository` n’ont pas besoin d’analyser le XML qui les traverse. Ils doivent traiter les documents XML comme opaque et laissent les couches supérieures à vous inquiétez pas sur la génération et l’analyse de documents.

Il existe quatre types concrets intégrés qui implémentent `IXmlRepository`:

::: moniker range=">= aspnetcore-2.2"

* [FileSystemXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [RegistryXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [AzureStorage. AzureBlobXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [RedisXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredis.redisxmlrepository)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [FileSystemXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [RegistryXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [AzureStorage. AzureBlobXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [RedisXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.redisxmlrepository)

::: moniker-end

Pour plus d’informations, consultez le document sur les [fournisseurs de stockage de clés](xref:security/data-protection/implementation/key-storage-providers) .

L’inscription d’un `IXmlRepository` personnalisé est appropriée lors de l’utilisation d’un autre magasin de stockage (par exemple, stockage table Azure).

Pour modifier le référentiel par défaut au niveau de l’application, inscrivez une instance de `IXmlRepository` personnalisée :

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

## <a name="ixmlencryptor"></a>IXmlEncryptor

L’interface `IXmlEncryptor` représente un type qui peut chiffrer un élément XML en texte brut. Elle expose une seule API :

* Chiffrer (plaintextElement XElement) : EncryptedXmlInfo

Si un `IAuthenticatedEncryptorDescriptor` sérialisé contient des éléments marqués comme « nécessite un chiffrement », `XmlKeyManager` exécutera ces éléments via la méthode de `Encrypt` de `IXmlEncryptor`configurée, et il conserve l’élément chiffré plutôt que l’élément de texte en clair dans le `IXmlRepository`. La sortie de la méthode `Encrypt` est un objet `EncryptedXmlInfo`. Cet objet est un wrapper qui contient à la fois le `XElement` chiffré résultant et le type qui représente un `IXmlDecryptor` qui peut être utilisé pour déchiffrer l’élément correspondant.

Il existe quatre types concrets intégrés qui implémentent `IXmlEncryptor`:

* [CertificateXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.certificatexmlencryptor)
* [DpapiNGXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapingxmlencryptor)
* [DpapiXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapixmlencryptor)
* [NullXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.nullxmlencryptor)

Pour plus d’informations, consultez le [document chiffrement à clé au repos](xref:security/data-protection/implementation/key-encryption-at-rest) .

Pour modifier le mécanisme par défaut de chiffrement par clé au repos à l’ensemble de l’application, inscrivez une instance de `IXmlEncryptor` personnalisée :

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

## <a name="ixmldecryptor"></a>IXmlDecryptor

L’interface `IXmlDecryptor` représente un type qui sait comment déchiffrer un `XElement` qui a été chiffré par le biais d’un `IXmlEncryptor`. Elle expose une seule API :

* Déchiffrer (encryptedElement XElement) : XElement

La méthode `Decrypt` annule le chiffrement effectué par `IXmlEncryptor.Encrypt`. En règle générale, chaque implémentation de `IXmlEncryptor` concrète aura une implémentation de `IXmlDecryptor` concrète correspondante.

Les types qui implémentent `IXmlDecryptor` doivent avoir l’un des deux constructeurs publics suivants :

* .ctor(IServiceProvider)
* .ctor()

> [!NOTE]
> La `IServiceProvider` passée au constructeur peut être null.

## <a name="ikeyescrowsink"></a>IKeyEscrowSink

L’interface `IKeyEscrowSink` représente un type qui peut effectuer un tiers de confiance d’informations sensibles. Rappelez-vous que les descripteurs sérialisés peuvent contenir des informations sensibles (telles que le matériel de chiffrement) et c’est ce qui a conduit à l’introduction du type [IXmlEncryptor](#ixmlencryptor) en premier lieu. Toutefois, des accidents se produisent, et des anneaux de clé peut être supprimés ou endommagés.

L’interface du tiers de confiance fournit un hachage d’échappement d’urgence, permettant d’accéder au code XML sérialisé brut avant qu’il ne soit transformé par un [IXmlEncryptor](#ixmlencryptor)configuré. L’interface expose une API unique :

* Store (Guid keyId, élément de XElement)

Il s’agit de l’implémentation `IKeyEscrowSink` pour gérer l’élément fourni de manière sécurisée et cohérente avec la stratégie d’entreprise. Une implémentation possible peut être que le récepteur de tiers de confiance chiffre l’élément XML à l’aide d’un certificat X. 509 d’entreprise connu dans lequel la clé privée du certificat a été déposée. le type de `CertificateXmlEncryptor` peut vous aider. L’implémentation de `IKeyEscrowSink` est également chargée de rendre l’élément fourni de manière appropriée.

Par défaut, aucun mécanisme tiers de confiance n’est activé, bien que les administrateurs de serveur puissent [le configurer globalement](xref:security/data-protection/configuration/machine-wide-policy). Elle peut également être configurée par programme à l’aide de la méthode `IDataProtectionBuilder.AddKeyEscrowSink`, comme indiqué dans l’exemple ci-dessous. Les surcharges de méthode `AddKeyEscrowSink` reflètent les surcharges de `IServiceCollection.AddSingleton` et de `IServiceCollection.AddInstance`, car les instances `IKeyEscrowSink` sont destinées à être des singletons. Si plusieurs instances de `IKeyEscrowSink` sont inscrites, chacune sera appelée pendant la génération de la clé, de sorte que les clés peuvent être déposées simultanément sur plusieurs mécanismes.

Il n’existe aucune API pour lire des documents à partir d’une instance de `IKeyEscrowSink`. Ceci est cohérent avec la théorie de conception du mécanisme de tiers de confiance : il est conçu pour améliorer le matériel de clé pour une autorité approuvée, et étant donné que l’application est lui-même pas une autorité approuvée, il ne doit pas avoir accès à son propre matériel mises en dépôt.

L’exemple de code suivant illustre la création et l’inscription d’un `IKeyEscrowSink` dans lequel les clés sont déposées de sorte que seuls les membres de « CONTOSODomain admins » puissent les récupérer.

> [!NOTE]
> Pour exécuter cet exemple, vous devez être sur un appareil joint au domaine Windows 8 / machine Windows Server 2012 et le contrôleur de domaine doivent être Windows Server 2012 ou version ultérieure.

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
