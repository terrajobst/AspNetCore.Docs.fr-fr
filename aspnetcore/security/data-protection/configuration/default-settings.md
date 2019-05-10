---
title: Gestion de clés de Protection des données et la durée de vie dans ASP.NET Core
author: rick-anderson
description: En savoir plus sur la gestion de clés de Protection des données et la durée de vie dans ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/configuration/default-settings
ms.openlocfilehash: 2f022a4c7519485fe629ce47c27d214c8c27d5bc
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897276"
---
# <a name="data-protection-key-management-and-lifetime-in-aspnet-core"></a>Gestion de clés de Protection des données et la durée de vie dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="key-management"></a>Gestion des clés

L’application tente de détecter de son environnement d’exploitation et de gérer la configuration de la clé sur son propre.

1. Si l’application est hébergée dans [Azure Apps](https://azure.microsoft.com/services/app-service/), les clés sont conservés dans le *%HOME%\ASP.NET\DataProtection-Keys* dossier. Ce dossier est alimenté par le stockage réseau et synchronisé sur tous les ordinateurs hébergeant l’application.
   * Les clés ne sont pas protégées au repos.
   * Le *DataProtection-clés* dossier fournit le porte-clés à toutes les instances d’une application dans un emplacement de déploiement.
   * Les emplacements de déploiement distincts, tels que Préproduction et Production, ne partagent pas de porte-clés. Lorsque vous échangez entre les emplacements de déploiement, par exemple le remplacement en Production ou à l’aide d’un des tests / B, n’importe quelle application à l’aide de la Protection des données sera en mesure de déchiffrer les données stockées à l’aide de l’anneau de clé à l’intérieur du slot précédent. Cela conduit à des utilisateurs consignés en dehors d’une application qui utilise l’authentification de cookie standard ASP.NET Core, car elle utilise la Protection des données pour protéger ses cookies. Si vous le souhaitez anneaux de clé indépendante de l’emplacement, utiliser un fournisseur de porte-clés externe, telles que le stockage Blob Azure, Azure Key Vault, un magasin SQL, ou du cache Redis.

1. Si le profil utilisateur est disponible, les clés sont conservés dans le *%LOCALAPPDATA%\ASP.NET\DataProtection-Keys* dossier. Si le système d’exploitation est Windows, les clés sont chiffrées au repos à l’aide de DPAPI.

   L’[attribut setProfileEnvironment](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration) du pool d’applications doit également être activé. La valeur par défaut de `setProfileEnvironment` est `true`. Dans certains scénarios (par exemple pour le système d’exploitation Windows), `setProfileEnvironment` est défini sur `false`. Si les clés ne sont pas stockées dans le répertoire de profil utilisateur comme prévu :

   1. Accédez au dossier *%windir%/system32/inetsrv/config*.
   1. Ouvrez le fichier *applicationHost.config*.
   1. Recherchez l’élément `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` .
   1. Confirmez que l’attribut `setProfileEnvironment` n’est pas présent, ce qui implique que la valeur par défaut est `true`, ou définissez de manière explicite la valeur de l’attribut sur `true`.

1. Si l’application est hébergée dans IIS, les clés sont conservées dans le Registre HKLM dans une clé de Registre spéciale est tiennent uniquement pour le compte de processus de travail. Les clés sont chiffrées au repos à l’aide de DPAPI.

1. Si aucune de ces conditions, les clés ne sont pas conservées en dehors du processus en cours. Lorsque le processus s’arrête, tous générés clés sont perdues.

Le développeur est toujours dans un contrôle total et peut substituer comment et où les clés sont stockées. Les trois premières options ci-dessus doivent fournir des valeurs par défaut appropriées pour la plupart des applications de façon similaire à la ASP.NET  **\<machineKey >** les routines de la génération automatique a travaillé dans le passé. L’option de secours, finale est le seul scénario qui exige que le développeur à spécifier [configuration](xref:security/data-protection/configuration/overview) initial s’ils veulent persistance des clés, mais cette action de secours se produit uniquement dans de rares cas.

Lorsque vous hébergez dans un conteneur Docker, clés doivent être persistante dans un dossier qui est un volume Docker (un volume partagé ou un volume monté hôte qui persiste au-delà de durée de vie du conteneur) ou dans un fournisseur externe, tel que [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) ou [Redis](https://redis.io/). Un fournisseur externe est également utile dans les scénarios de batterie de serveurs web si les applications ne peut pas accéder à un volume partagé de réseau (voir [PersistKeysToFileSystem](xref:security/data-protection/configuration/overview#persistkeystofilesystem) pour plus d’informations).

> [!WARNING]
> Si le développeur remplace les règles décrites ci-dessus et pointe le système de Protection des données à un dépôt de clé spécifique, le chiffrement automatique de clés au repos est désactivé. Protection d’au repos peut être réactivée via [configuration](xref:security/data-protection/configuration/overview).

## <a name="key-lifetime"></a>Durée de vie des clés

Par défaut, les clés ont une durée de vie de 90 jours. Lorsqu’une clé expire, l’application génère une nouvelle clé et définit la nouvelle clé comme clé active automatiquement. Tant que les clés supprimées restent sur le système, votre application peut déchiffrer toutes les données protégées avec eux. Consultez [gestion des clés](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling) pour plus d’informations.

## <a name="default-algorithms"></a>Algorithmes par défaut

L’algorithme de protection de charge utile par défaut utilisé est AES-256-CBC pour la confidentialité et HMACSHA256 authenticité. Une clé principale de 512 bits, modifiée tous les 90 jours, est utilisée pour dériver les deux sous-clés utilisés pour ces algorithmes sur une base par charge utile. Consultez [dérivation de sous-clé](xref:security/data-protection/implementation/subkeyderivation#additional-authenticated-data-and-subkey-derivation) pour plus d’informations.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:security/data-protection/extensibility/key-management>
* <xref:host-and-deploy/web-farm>
