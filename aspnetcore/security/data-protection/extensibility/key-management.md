---
title: Extensibilité de la gestion de clés dans ASP.NET Core
author: rick-anderson
description: En savoir plus sur l’extensibilité de la gestion de clés de Protection des données ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: 1cf3fc30f72fb872ff9d7f33fc5ffb12a11a982f
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090613"
---
# <a name="key-management-extensibility-in-aspnet-core"></a>Extensibilité de la gestion de clés dans ASP.NET Core

> [!TIP]
> Lire le [gestion des clés](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) section avant de lire cette section, car il explique certains concepts fondamentaux derrière ces API.

> [!WARNING]
> Types qui implémentent les interfaces suivantes doivent être thread-safe pour les appelants plusieurs.

## <a name="key"></a>Touche

Le `IKey` interface est la représentation sous forme de base d’une clé dans les systèmes de chiffrement. L’expression clé est utilisé ici dans le point de vue abstrait, pas dans le sens littéral de « clé de chiffrement ». Une clé a les propriétés suivantes :

* Dates d’activation, la création et l’expiration

* État de révocation

* Identificateur de clé (un GUID)

::: moniker range=">= aspnetcore-2.0"

En outre, `IKey` expose un `CreateEncryptor` méthode qui peut être utilisé pour créer un [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance liée à cette clé.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

En outre, `IKey` expose un `CreateEncryptorInstance` méthode qui peut être utilisé pour créer un [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance liée à cette clé.

::: moniker-end

> [!NOTE]
> Il n’existe aucune API pour récupérer le matériel de chiffrement brutes à partir d’un `IKey` instance.

## <a name="ikeymanager"></a>IKeyManager

Le `IKeyManager` interface représente un objet responsable du stockage de clés généraux, d’extraction et de manipulation. Elle expose trois opérations de haut niveau :

* Créer une nouvelle clé et conserver dans le stockage.

* Obtenir toutes les clés de stockage.

* Révoquer une ou plusieurs clés et conserver les informations de révocation dans le stockage.

>[!WARNING]
> Écriture d’un `IKeyManager` est une tâche très avancée, et il ne doit pas tenter de la majorité des développeurs. Au lieu de cela, la plupart des développeurs doit bénéficier des installations offertes par le [XmlKeyManager](#xmlkeymanager) classe.

## <a name="xmlkeymanager"></a>XmlKeyManager

Le `XmlKeyManager` type est l’implémentation concrète de l’emploi de `IKeyManager`. Il fournit plusieurs installations utiles, y compris le dépôt de clé et le chiffrement des clés au repos. Dans ce système, les clés sont représentées en tant qu’éléments XML (plus précisément, [XElement](/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).

`XmlKeyManager` dépend de plusieurs autres composants au cours de l’exécution de ses tâches :

::: moniker range=">= aspnetcore-2.0"

* `AlgorithmConfiguration`, qui déterminent les algorithmes utilisés par les nouvelles clés.

* `IXmlRepository`, qui contrôle où les clés sont conservés dans le stockage.

* `IXmlEncryptor` [facultatif], ce qui permet le chiffrement de clés au repos.

* `IKeyEscrowSink` [facultatif], qui fournit des services de dépôt de clé.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* `IXmlRepository`, qui contrôle où les clés sont conservés dans le stockage.

* `IXmlEncryptor` [facultatif], ce qui permet le chiffrement de clés au repos.

* `IKeyEscrowSink` [facultatif], qui fournit des services de dépôt de clé.

::: moniker-end

Voici les diagrammes de haut niveau qui indiquent la façon dont ces composants sont reliés au sein de `XmlKeyManager`.

::: moniker range=">= aspnetcore-2.0"

![Création de la clé](key-management/_static/keycreation2.png)

*La création de clé / CreateNewKey*

Dans l’implémentation de `CreateNewKey`, le `AlgorithmConfiguration` composant est utilisé pour créer un unique `IAuthenticatedEncryptorDescriptor`, lequel est ensuite sérialisé au format XML. Si un récepteur de dépôt de clé est présent, les données XML brutes (non chiffré) est fourni pour le récepteur pour le stockage à long terme. Le code XML non chiffré est alors exécuté un `IXmlEncryptor` (si nécessaire) pour générer le document XML chiffré. Ce document chiffré est rendu persistant dans le stockage à long terme via la `IXmlRepository`. (Si aucun `IXmlEncryptor` est configuré, le document non chiffré est conservé dans le `IXmlRepository`.)

![Récupération de clé](key-management/_static/keyretrieval2.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

![Création de la clé](key-management/_static/keycreation1.png)

*La création de clé / CreateNewKey*

Dans l’implémentation de `CreateNewKey`, le `IAuthenticatedEncryptorConfiguration` composant est utilisé pour créer un unique `IAuthenticatedEncryptorDescriptor`, lequel est ensuite sérialisé au format XML. Si un récepteur de dépôt de clé est présent, les données XML brutes (non chiffré) est fourni pour le récepteur pour le stockage à long terme. Le code XML non chiffré est alors exécuté un `IXmlEncryptor` (si nécessaire) pour générer le document XML chiffré. Ce document chiffré est rendu persistant dans le stockage à long terme via la `IXmlRepository`. (Si aucun `IXmlEncryptor` est configuré, le document non chiffré est conservé dans le `IXmlRepository`.)

![Récupération de clé](key-management/_static/keyretrieval1.png)

::: moniker-end

*Récupération de clé / GetAllKeys*

Dans l’implémentation de `GetAllKeys`, clés représentant les documents XML et les révocations sont lus à partir de sous-jacent `IXmlRepository`. Si ces documents sont chiffrées, le système sera automatiquement les déchiffrer. `XmlKeyManager` crée l’approprié `IAuthenticatedEncryptorDescriptorDeserializer` instances pour désérialiser les documents de nouveau `IAuthenticatedEncryptorDescriptor` instances, qui sont ensuite encapsulées dans un individu `IKey` instances. Cette collection de `IKey` instances est retourné à l’appelant.

Vous trouverez plus d’informations sur les éléments XML particuliers dans le [document de format de stockage de clés](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).

## <a name="ixmlrepository"></a>IXmlRepository

Le `IXmlRepository` interface représente un type qui peut faire persister XML à et récupérer des données XML à partir d’un magasin de stockage. Elle expose deux API :

* `GetAllElements` :`IReadOnlyCollection<XElement>`

* `StoreElement(XElement element, string friendlyName)`

Les implémentations de `IXmlRepository` n’avez pas besoin analyser le XML en passant par leur intermédiaire. Ils doivent traiter les documents XML comme opaque et laissent les couches supérieures à vous inquiétez pas sur la génération et l’analyse de documents.

Il existe quatre types concrets intégrés qui implémentent `IXmlRepository`:

::: moniker range=">= aspnetcore-2.2"

* [FileSystemXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [RegistryXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [AzureStorage.AzureBlobXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [RedisXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredis.redisxmlrepository)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [FileSystemXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [RegistryXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [AzureStorage.AzureBlobXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [RedisXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.redisxmlrepository)

::: moniker-end

Consultez le [document de fournisseurs de stockage de clés](xref:security/data-protection/implementation/key-storage-providers) pour plus d’informations.

L’enregistrement personnalisé `IXmlRepository` est appropriée lorsque vous utilisez un magasin de stockage différents (par exemple, le stockage Azure Table).

Pour modifier le référentiel par défaut à l’échelle de l’application, inscrivez un personnalisé `IXmlRepository` instance :

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

Le `IXmlEncryptor` interface représente un type qui permet de chiffrer un élément XML de texte en clair. Elle expose une seule API :

* Chiffrer (plaintextElement XElement) : EncryptedXmlInfo

Si un sérialisée `IAuthenticatedEncryptorDescriptor` contient des éléments de la mention « requiert le chiffrement », puis `XmlKeyManager` exécutera ces éléments via configuré `IXmlEncryptor`de `Encrypt` méthode et qu’elle conserve l’élément des plutôt que la élément de texte en clair à le `IXmlRepository`. La sortie de la `Encrypt` méthode est un `EncryptedXmlInfo` objet. Cet objet est un wrapper qui contient les deux le résultant des `XElement` et le Type qui représente un `IXmlDecryptor` qui peut être utilisé pour déchiffrer l’élément correspondant.

Il existe quatre types concrets intégrés qui implémentent `IXmlEncryptor`:

* [CertificateXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.certificatexmlencryptor)
* [DpapiNGXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapingxmlencryptor)
* [DpapiXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapixmlencryptor)
* [NullXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.nullxmlencryptor)

Consultez le [chiffrement à clé au document de rest](xref:security/data-protection/implementation/key-encryption-at-rest) pour plus d’informations.

Pour modifier le mécanisme de clé chiffrement au repos par défaut à l’échelle de l’application, inscrivez un personnalisé `IXmlEncryptor` instance :

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

Le `IXmlDecryptor` interface représente un type qui sait comment déchiffrer un `XElement` qui a été des un `IXmlEncryptor`. Elle expose une seule API :

* Déchiffrer (encryptedElement XElement) : XElement

Le `Decrypt` méthode annule le chiffrement effectué par `IXmlEncryptor.Encrypt`. En règle générale, chaque concret `IXmlEncryptor` implémentation aura un concret correspondante `IXmlDecryptor` implémentation.

Types qui implémentent `IXmlDecryptor` doit avoir l’une des deux constructeurs publics :

* .ctor(IServiceProvider)
* .ctor()

> [!NOTE]
> Le `IServiceProvider` passé au constructeur peut être null.

## <a name="ikeyescrowsink"></a>IKeyEscrowSink

Le `IKeyEscrowSink` interface représente un type qui peut effectuer le tiers de confiance des informations sensibles. Rappelez-vous que les descripteurs sérialisées peuvent contenir des informations sensibles (telles que le matériel de chiffrement), et c’est ce qui a conduit à l’introduction de la [IXmlEncryptor](#ixmlencryptor) tapez en premier lieu. Toutefois, des accidents se produisent, et des anneaux de clé peut être supprimés ou endommagés.

L’interface de tiers de confiance offre une urgence, autorisant l’accès au document XML sérialisé brut avant sa transformation par aucun configuré [IXmlEncryptor](#ixmlencryptor). L’interface expose une API unique :

* Store (Guid keyId, élément de XElement)

C’est à le `IKeyEscrowSink` implémentation pour gérer l’élément fourni de manière sécurisée cohérente avec la stratégie d’entreprise. Une implémentation possible peut être pour le récepteur tiers de confiance chiffrer l’élément XML à l’aide d’un certificat X.509 d’entreprise connu où la clé privée du certificat a été déposée ; le `CertificateXmlEncryptor` type peut vous y aider. Le `IKeyEscrowSink` implémentation est également chargée de rendre l’élément fourni de manière appropriée.

Par défaut aucun mécanisme de tiers de confiance n’est activé, bien que les administrateurs de serveur peuvent [configurer cela dans le monde entier](xref:security/data-protection/configuration/machine-wide-policy). Il peut également être configuré par programmation la `IDataProtectionBuilder.AddKeyEscrowSink` méthode comme illustré dans l’exemple ci-dessous. Le `AddKeyEscrowSink` mise en miroir des surcharges de méthode le `IServiceCollection.AddSingleton` et `IServiceCollection.AddInstance` surcharges, comme `IKeyEscrowSink` instances sont destinées à être singletons. Si plusieurs `IKeyEscrowSink` instances sont enregistrées, chacun d’eux sera appelée pendant la génération de clés, afin de clés peuvent être déposées simultanément plusieurs mécanismes.

Il n’existe aucune API pour lire des documents à partir d’un `IKeyEscrowSink` instance. Ceci est cohérent avec la théorie de conception du mécanisme de tiers de confiance : il est conçu pour améliorer le matériel de clé pour une autorité approuvée, et étant donné que l’application est lui-même pas une autorité approuvée, il ne doit pas avoir accès à son propre matériel mises en dépôt.

L’exemple de code suivant montre comment créer et inscrire un `IKeyEscrowSink` où les clés sont déposées telles que seuls les membres du « CONTOSODomain Admins » pour les récupérer.

> [!NOTE]
> Pour exécuter cet exemple, vous devez être sur un appareil joint au domaine Windows 8 / machine Windows Server 2012 et le contrôleur de domaine doivent être Windows Server 2012 ou version ultérieure.

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
