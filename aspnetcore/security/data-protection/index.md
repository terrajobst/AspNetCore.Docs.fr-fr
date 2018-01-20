---
title: "Protection des données dans ASP.NET Core"
author: rick-anderson
description: "Ce document constitue la table des matières des différentes rubriques relatives à la protection des données ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: 7bbd203a67b32032ba2ab82448a5fc9a495b52aa
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="07b04-103">Protection des données dans ASP.NET Core : API de contrôle serveur consommateur, configuration, API d’extensibilité et implémentation</span><span class="sxs-lookup"><span data-stu-id="07b04-103">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="07b04-104">Présentation de la protection des données</span><span class="sxs-lookup"><span data-stu-id="07b04-104">Introduction to data protection</span></span>](introduction.md)

* [<span data-ttu-id="07b04-105">Bien démarrer avec les API de protection des données</span><span class="sxs-lookup"><span data-stu-id="07b04-105">Get started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="07b04-106">API de contrôle serveur consommateur</span><span class="sxs-lookup"><span data-stu-id="07b04-106">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="07b04-107">Vue d’ensemble des API de contrôle serveur consommateur</span><span class="sxs-lookup"><span data-stu-id="07b04-107">Consumer APIs overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="07b04-108">Chaînes d’objectifs</span><span class="sxs-lookup"><span data-stu-id="07b04-108">Purpose strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="07b04-109">Hiérarchie d’objectifs et architecture mutualisée</span><span class="sxs-lookup"><span data-stu-id="07b04-109">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="07b04-110">Hachage de mot de passe</span><span class="sxs-lookup"><span data-stu-id="07b04-110">Password hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="07b04-111">Limitation de la durée de vie des charges utiles protégées</span><span class="sxs-lookup"><span data-stu-id="07b04-111">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="07b04-112">Retrait de la protection des charges utiles dont les clés ont été révoquées</span><span class="sxs-lookup"><span data-stu-id="07b04-112">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="07b04-113">Configuration</span><span class="sxs-lookup"><span data-stu-id="07b04-113">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="07b04-114">Configuration de la protection des données</span><span class="sxs-lookup"><span data-stu-id="07b04-114">Configuring data protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="07b04-115">Paramètres par défaut</span><span class="sxs-lookup"><span data-stu-id="07b04-115">Default settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="07b04-116">Stratégie à l’échelle de l’ordinateur</span><span class="sxs-lookup"><span data-stu-id="07b04-116">Machine-wide policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="07b04-117">Scénarios non compatibles avec l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="07b04-117">Non DI-aware scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="07b04-118">API d’extensibilité</span><span class="sxs-lookup"><span data-stu-id="07b04-118">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="07b04-119">Extensibilité du chiffrement de base</span><span class="sxs-lookup"><span data-stu-id="07b04-119">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="07b04-120">Extensibilité de la gestion de clés</span><span class="sxs-lookup"><span data-stu-id="07b04-120">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="07b04-121">API diverses</span><span class="sxs-lookup"><span data-stu-id="07b04-121">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="07b04-122">Implémentation</span><span class="sxs-lookup"><span data-stu-id="07b04-122">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="07b04-123">Détails du chiffrement authentifié</span><span class="sxs-lookup"><span data-stu-id="07b04-123">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="07b04-124">Dérivation de sous-clé et chiffrement authentifié</span><span class="sxs-lookup"><span data-stu-id="07b04-124">Subkey derivation and authenticated encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="07b04-125">En-têtes de contexte</span><span class="sxs-lookup"><span data-stu-id="07b04-125">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="07b04-126">Gestion des clés</span><span class="sxs-lookup"><span data-stu-id="07b04-126">Key management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="07b04-127">Fournisseurs de stockage de clés</span><span class="sxs-lookup"><span data-stu-id="07b04-127">Key storage providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="07b04-128">Chiffrement à clé au repos</span><span class="sxs-lookup"><span data-stu-id="07b04-128">Key encryption at rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="07b04-129">Immuabilité des clés et modification des paramètres</span><span class="sxs-lookup"><span data-stu-id="07b04-129">Key immutability and changing settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="07b04-130">Format de stockage des clés</span><span class="sxs-lookup"><span data-stu-id="07b04-130">Key storage format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="07b04-131">Fournisseurs de protection des données éphémères</span><span class="sxs-lookup"><span data-stu-id="07b04-131">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="07b04-132">Compatibilité</span><span class="sxs-lookup"><span data-stu-id="07b04-132">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="07b04-133">Partage des cookies entre les applications</span><span class="sxs-lookup"><span data-stu-id="07b04-133">Sharing cookies between apps</span></span>](compatibility/cookie-sharing.md)

  * [<span data-ttu-id="07b04-134">Remplacement de <machineKey> dans ASP.NET</span><span class="sxs-lookup"><span data-stu-id="07b04-134">Replacing <machineKey> in ASP.NET</span></span>](compatibility/replacing-machinekey.md)
