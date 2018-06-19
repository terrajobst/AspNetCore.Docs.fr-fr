---
title: Protection des données dans ASP.NET Core
author: rick-anderson
description: Ce document constitue la table des matières des différentes rubriques relatives à la protection des données ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/index
ms.openlocfilehash: 83b5bb1e6a4942a4d3e5ec0d445fa6e5a21fb533
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
ms.locfileid: "30071693"
---
# <a name="data-protection-in-aspnet-core"></a>Protection des données dans ASP.NET Core

* [Présentation de la protection des données](xref:security/data-protection/introduction)

* [Bien démarrer avec les API de protection des données](xref:security/data-protection/using-data-protection)

* [API de contrôle serveur consommateur](xref:security/data-protection/consumer-apis/index)

  * [Vue d’ensemble des API de contrôle serveur consommateur](xref:security/data-protection/consumer-apis/overview)

  * [Chaînes d’objectifs](xref:security/data-protection/consumer-apis/purpose-strings)

  * [Hiérarchie d’objectifs et architecture mutualisée](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)

  * [Hacher les mots de passe](xref:security/data-protection/consumer-apis/password-hashing)

  * [Limiter la durée de vie des charges utiles protégées](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)

  * [Retirer la protection des charges utiles dont les clés ont été révoquées](xref:security/data-protection/consumer-apis/dangerous-unprotect)

* [Configuration](xref:security/data-protection/configuration/index)

  * [Configurer la protection des données ASP.NET Core](xref:security/data-protection/configuration/overview)

  * [Paramètres par défaut](xref:security/data-protection/configuration/default-settings)

  * [Stratégie à l’échelle de l’ordinateur](xref:security/data-protection/configuration/machine-wide-policy)

  * [Scénarios non compatibles avec l’injection de dépendances](xref:security/data-protection/configuration/non-di-scenarios)

* [API d’extensibilité](xref:security/data-protection/extensibility/index)

  * [Extensibilité du chiffrement de base](xref:security/data-protection/extensibility/core-crypto)

  * [Extensibilité de la gestion de clés](xref:security/data-protection/extensibility/key-management)

  * [API diverses](xref:security/data-protection/extensibility/misc-apis)

* [Implémentation](xref:security/data-protection/implementation/index)

  * [Détails du chiffrement authentifié](xref:security/data-protection/implementation/authenticated-encryption-details)

  * [Dérivation de sous-clé et chiffrement authentifié](xref:security/data-protection/implementation/subkeyderivation)

  * [En-têtes de contexte](xref:security/data-protection/implementation/context-headers)

  * [Gestion des clés](xref:security/data-protection/implementation/key-management)

  * [Fournisseurs de stockage de clés](xref:security/data-protection/implementation/key-storage-providers)

  * [Chiffrement à clé au repos](xref:security/data-protection/implementation/key-encryption-at-rest)

  * [Immuabilité et paramètres des clés](xref:security/data-protection/implementation/key-immutability)

  * [Format de stockage des clés](xref:security/data-protection/implementation/key-storage-format)

  * [Fournisseurs de protection des données éphémères](xref:security/data-protection/implementation/key-storage-ephemeral)

* [Compatibilité](xref:security/data-protection/compatibility/index)

  * [Remplacement de <machineKey> d’ASP.NET dans ASP.NET Core](xref:security/data-protection/compatibility/replacing-machinekey)
