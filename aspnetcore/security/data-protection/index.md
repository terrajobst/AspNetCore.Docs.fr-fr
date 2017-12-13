---
title: "Protection des données dans ASP.NET Core"
author: rick-anderson
description: "Ce document constitue la table des matières des différentes rubriques relatives à la protection des données ASP.NET Core."
keywords: "ASP.NET Core,protection des données"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 1f402da8-1052-4970-9835-9f9f16a02dbc
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: 8b42e65bb6121355120a6f4fbe8cd4d1fea153de
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="03703-104">Protection des données dans ASP.NET Core : API de contrôle serveur consommateur, configuration, API d’extensibilité et implémentation</span><span class="sxs-lookup"><span data-stu-id="03703-104">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="03703-105">Présentation de la protection des données</span><span class="sxs-lookup"><span data-stu-id="03703-105">Introduction to Data Protection</span></span>](introduction.md)

* [<span data-ttu-id="03703-106">Bien démarrer avec les API de protection des données</span><span class="sxs-lookup"><span data-stu-id="03703-106">Getting Started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="03703-107">API de contrôle serveur consommateur</span><span class="sxs-lookup"><span data-stu-id="03703-107">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="03703-108">Vue d’ensemble des API de contrôle serveur consommateur</span><span class="sxs-lookup"><span data-stu-id="03703-108">Consumer APIs Overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="03703-109">Chaînes d’objectifs</span><span class="sxs-lookup"><span data-stu-id="03703-109">Purpose Strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="03703-110">Hiérarchie d’objectifs et architecture mutualisée</span><span class="sxs-lookup"><span data-stu-id="03703-110">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="03703-111">Hachage de mot de passe</span><span class="sxs-lookup"><span data-stu-id="03703-111">Password Hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="03703-112">Limitation de la durée de vie des charges utiles protégées</span><span class="sxs-lookup"><span data-stu-id="03703-112">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="03703-113">Retrait de la protection des charges utiles dont les clés ont été révoquées</span><span class="sxs-lookup"><span data-stu-id="03703-113">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="03703-114">Configuration</span><span class="sxs-lookup"><span data-stu-id="03703-114">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="03703-115">Configuration de la protection des données</span><span class="sxs-lookup"><span data-stu-id="03703-115">Configuring Data Protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="03703-116">Paramètres par défaut</span><span class="sxs-lookup"><span data-stu-id="03703-116">Default Settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="03703-117">Stratégie à l’échelle de la machine</span><span class="sxs-lookup"><span data-stu-id="03703-117">Machine Wide Policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="03703-118">Scénarios non compatibles avec l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="03703-118">Non DI Aware Scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="03703-119">API d’extensibilité</span><span class="sxs-lookup"><span data-stu-id="03703-119">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="03703-120">Extensibilité du chiffrement de base</span><span class="sxs-lookup"><span data-stu-id="03703-120">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="03703-121">Extensibilité de la gestion de clés</span><span class="sxs-lookup"><span data-stu-id="03703-121">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="03703-122">API diverses</span><span class="sxs-lookup"><span data-stu-id="03703-122">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="03703-123">Implémentation</span><span class="sxs-lookup"><span data-stu-id="03703-123">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="03703-124">Détails du chiffrement authentifié</span><span class="sxs-lookup"><span data-stu-id="03703-124">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="03703-125">Dérivation de sous-clé et chiffrement authentifié</span><span class="sxs-lookup"><span data-stu-id="03703-125">Subkey Derivation and Authenticated Encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="03703-126">En-têtes de contexte</span><span class="sxs-lookup"><span data-stu-id="03703-126">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="03703-127">Gestion des clés</span><span class="sxs-lookup"><span data-stu-id="03703-127">Key Management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="03703-128">Fournisseurs de stockage de clés</span><span class="sxs-lookup"><span data-stu-id="03703-128">Key Storage Providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="03703-129">Chiffrement à clé au repos</span><span class="sxs-lookup"><span data-stu-id="03703-129">Key Encryption At Rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="03703-130">Immuabilité des clés et modification des paramètres</span><span class="sxs-lookup"><span data-stu-id="03703-130">Key Immutability and Changing Settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="03703-131">Format de stockage des clés</span><span class="sxs-lookup"><span data-stu-id="03703-131">Key Storage Format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="03703-132">Fournisseurs de protection des données éphémères</span><span class="sxs-lookup"><span data-stu-id="03703-132">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="03703-133">Compatibilité</span><span class="sxs-lookup"><span data-stu-id="03703-133">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="03703-134">Partage des cookies entre les applications</span><span class="sxs-lookup"><span data-stu-id="03703-134">Sharing cookies between applications</span></span>](compatibility/cookie-sharing.md)

  * [<span data-ttu-id="03703-135">Remplacement de <machineKey> dans ASP.NET</span><span class="sxs-lookup"><span data-stu-id="03703-135">Replacing <machineKey> in ASP.NET</span></span>](compatibility/replacing-machinekey.md)
