---
title: Scénarios de prenant en charge l’injection de dépendances non pour la Protection des données dans ASP.NET Core
author: rick-anderson
description: Découvrez comment prendre en charge les scénarios de protection de données où vous ne pouvez pas ou ne souhaitez pas utiliser un service fourni par l’injection de dépendances.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/configuration/non-di-scenarios
ms.openlocfilehash: 34354c8443f6ae00bcce6ad9bdb6c11aaaa25bf8
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897306"
---
# <a name="non-di-aware-scenarios-for-data-protection-in-aspnet-core"></a>Scénarios de prenant en charge l’injection de dépendances non pour la Protection des données dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Le système de Protection des données ASP.NET Core est normalement [ajouté à un conteneur de service](xref:security/data-protection/consumer-apis/overview) et utilisées par les composants dépendants via l’injection de dépendance (DI). Toutefois, il existe des cas où cela n’est pas faisable ou souhaitée, en particulier lorsque le système une importation dans une application existante.

Pour prendre en charge ces scénarios, le [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) package fournit un type concret, [DataProtectionProvider](/dotnet/api/Microsoft.AspNetCore.DataProtection.DataProtectionProvider), qui offre un moyen simple d’utiliser la Protection des données sans se baser sur l’injection de dépendances. Le `DataProtectionProvider` type implémente [IDataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionprovider). Construction `DataProtectionProvider` , il suffit en fournissant un [DirectoryInfo](/dotnet/api/system.io.directoryinfo) instance pour indiquer où les clés de chiffrement du fournisseur doivent être stockées, comme illustré dans l’exemple de code suivant :

[!code-none[](non-di-scenarios/_static/nodisample1.cs)]

Par défaut, le `DataProtectionProvider` type concret ne chiffre le matériel de clé brutes avant du rendre persistantes dans le système de fichiers. Il s’agit pour prendre en charge les scénarios où les points de développeur à un partage réseau et le système de Protection des données ne peut pas déduire automatiquement un mécanisme de chiffrement à clé approprié au repos.

En outre, le `DataProtectionProvider` type concret ne [isoler les applications](xref:security/data-protection/configuration/overview#per-application-isolation) par défaut. Toutes les applications utilisant le même répertoire de clés peuvent partager des charges utiles tant que leur [usage paramètres](xref:security/data-protection/consumer-apis/purpose-strings) correspondent.

Le [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) constructeur accepte un rappel de configuration facultative qui peut être utilisé pour ajuster les comportements du système. L’exemple ci-dessous illustre la restauration d’isolation avec un appel explicite à [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname). L’exemple illustre également la configuration du système pour chiffrer automatiquement des clés persistantes à l’aide de Windows DPAPI. Si le répertoire pointe vers un partage UNC, vous avez la possibilité de distribuer un certificat partagé sur toutes les machines pertinentes et pour configurer le système pour utiliser le chiffrement de base de certificat avec un appel à [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate).

[!code-none[](non-di-scenarios/_static/nodisample2.cs)]

> [!TIP]
> Instances de la `DataProtectionProvider` type concret sont coûteux à créer. Si une application gère plusieurs instances de ce type, et s’ils sont tous utilisent le même répertoire de stockage de clés, peuvent dégrader les performances de l’application. Si vous utilisez le `DataProtectionProvider` type, nous vous recommandons de créez ce type une seule fois et le réutiliser autant que possible. Le `DataProtectionProvider` type et tous les [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) instances créées à partir de celui-ci sont thread-safe pour les appelants plusieurs.
