---
title: Stratégie de Protection des données à l’échelle de l’ordinateur prend en charge dans ASP.NET Core
author: rick-anderson
description: Découvrez la prise en charge pour la définition d’une stratégie de l’ordinateur par défaut pour toutes les applications qui utilisent la Protection des données ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/configuration/machine-wide-policy
ms.openlocfilehash: 70aaca7afcd3df22cebb4466fbd9845a2277688c
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897266"
---
# <a name="data-protection-machine-wide-policy-support-in-aspnet-core"></a>Stratégie de Protection des données à l’échelle de l’ordinateur prend en charge dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Lors de l’exécution sur Windows, le système de Protection des données prend en charge limitée pour la définition d’une stratégie de l’ordinateur par défaut pour toutes les applications qui utilisent la Protection des données ASP.NET Core. L’idée générale est qu’un administrateur peut souhaiter modifier un paramètre par défaut, tels que les algorithmes utilisés ou la durée de vie des clés, sans avoir à mettre à jour manuellement chaque application sur l’ordinateur.

> [!WARNING]
> L’administrateur système peut définir la stratégie par défaut, mais ils ne peuvent pas l’appliquer. Le développeur d’application peut toujours remplacer n’importe quelle valeur avec l’un de leurs propres choix. La stratégie par défaut affecte uniquement les applications où le développeur n’a pas spécifié une valeur explicite pour un paramètre.

## <a name="setting-default-policy"></a>Définition de la stratégie par défaut

Pour définir la stratégie par défaut, un administrateur peut définir des valeurs connues dans le Registre système sous la clé de Registre suivante :

**HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection**

Si vous êtes sur un système d’exploitation 64 bits et que vous souhaitez affecter le comportement des applications 32 bits, pensez à configurer l’équivalent Wow6432Node de la clé ci-dessus.

Les valeurs prises en charge sont indiquées ci-dessous.

| Value              | Type   | Description |
| ------------------ | :----: | ----------- |
| EncryptionType     | string | Spécifie les algorithmes qui doivent être utilisés pour la protection des données. La valeur doit être CNG-CBC, CNG-GCM ou géré et est décrite plus en détail ci-dessous. |
| DefaultKeyLifetime | DWORD  | Spécifie la durée de vie des clés qui vient d’être généré. La valeur est spécifiée en jours et doit être > = 7. |
| KeyEscrowSinks     | string | Spécifie les types qui sont utilisés pour le dépôt de clé. La valeur est une liste délimitée par des points-virgules des récepteurs de dépôt de clé, où chaque élément dans la liste est le nom qualifié d’assembly d’un type qui implémente [IKeyEscrowSink](/dotnet/api/microsoft.aspnetcore.dataprotection.keymanagement.ikeyescrowsink). |

## <a name="encryption-types"></a>Types de chiffrement

Si EncryptionType est CNG-CBC, le système est configuré pour utiliser un chiffrement par bloc symétriques d’en mode CBC pour la confidentialité et HMAC authenticité avec les services fournis par Windows CNG (consultez [spécifiant les algorithmes CNG de Windows personnalisés](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms) pour plus de détails). Les valeurs supplémentaires suivantes sont prises en charge, chacun d’eux correspondant à une propriété sur le type CngCbcAuthenticatedEncryptionSettings.

| Value                       | Type   | Description |
| --------------------------- | :----: | ----------- |
| EncryptionAlgorithm         | string | Le nom d’un algorithme de chiffrement symétrique compris par CNG. Cet algorithme est ouvert en mode CBC. |
| EncryptionAlgorithmProvider | string | Le nom de l’implémentation de fournisseur CNG qui peut produire l’algorithme EncryptionAlgorithm. |
| EncryptionAlgorithmKeySize  | DWORD  | La longueur (en bits) de la clé à dériver pour l’algorithme de chiffrement par bloc symétriques. |
| Élément HashAlgorithm Impossible               | string | Le nom d’un algorithme de hachage compris par CNG. Cet algorithme est ouvert en mode HMAC. |
| HashAlgorithmProvider       | string | Le nom de l’implémentation de fournisseur CNG qui peut produire l’algorithme HashAlgorithm. |

Si EncryptionType est CNG-GCM, le système est configuré pour utiliser un chiffrement par bloc symétriques de Mode Galois/compteur pour la confidentialité et l’authenticité avec les services fournis par Windows CNG (consultez [spécifiant les algorithmes CNG de Windows personnalisés](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms) Pour plus d’informations.) Les valeurs supplémentaires suivantes sont prises en charge, chacun d’eux correspondant à une propriété sur le type CngGcmAuthenticatedEncryptionSettings.

| Value                       | Type   | Description |
| --------------------------- | :----: | ----------- |
| EncryptionAlgorithm         | string | Le nom d’un algorithme de chiffrement symétrique compris par CNG. Cet algorithme est ouvert en Mode de compteur/Galois. |
| EncryptionAlgorithmProvider | string | Le nom de l’implémentation de fournisseur CNG qui peut produire l’algorithme EncryptionAlgorithm. |
| EncryptionAlgorithmKeySize  | DWORD  | La longueur (en bits) de la clé à dériver pour l’algorithme de chiffrement par bloc symétriques. |

Si EncryptionType est géré, le système est configuré pour utiliser un SymmetricAlgorithm géré pour la confidentialité et l’élément KeyedHashAlgorithm impossible pour l’authenticité (consultez [spécification personnalisé géré algorithmes](xref:security/data-protection/configuration/overview#specifying-custom-managed-algorithms) pour plus d’informations). Les valeurs supplémentaires suivantes sont prises en charge, chacun d’eux correspondant à une propriété sur le type ManagedAuthenticatedEncryptionSettings.

| Value                      | Type   | Description |
| -------------------------- | :----: | ----------- |
| EncryptionAlgorithmType    | string | Le nom qualifié d’assembly d’un type qui implémente SymmetricAlgorithm. |
| EncryptionAlgorithmKeySize | DWORD  | La longueur (en bits) de la clé à dériver pour l’algorithme de chiffrement symétrique. |
| ValidationAlgorithmType    | string | Le nom qualifié d’assembly d’un type qui implémente l’élément KeyedHashAlgorithm impossible. |

Si EncryptionType a toute autre valeur différente de null ou vide, le système de Protection de données lève une exception au démarrage.

> [!WARNING]
> Lorsque vous configurez un paramètre de stratégie par défaut qui implique des noms de types (EncryptionAlgorithmType, ValidationAlgorithmType, KeyEscrowSinks), les types doivent être disponibles pour l’application. Cela signifie que pour les applications qui s’exécutent sur le CLR de bureau, les assemblys qui contiennent ces types doivent figurer dans le Global Assembly Cache (GAC). Pour les applications ASP.NET Core s’exécutant sur .NET Core, les packages qui contiennent ces types doivent être installés.
