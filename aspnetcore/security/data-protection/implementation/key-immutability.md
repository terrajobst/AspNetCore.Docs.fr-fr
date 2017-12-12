---
title: "Immuabilité clée et la modification des paramètres"
author: rick-anderson
description: "Ce document décrit les détails d’implémentation de l’ASP.NET Core protection clée immuabilité des données API."
keywords: "ASP.NET Core, protection des données, la clé que l’immuabilité"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: fc911ae3-feca-4ffe-8b43-362bc632fe04
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 96860b44b64f241a1bbff2ac8366e0863b1ac10c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
# <a name="key-immutability-and-changing-settings"></a><span data-ttu-id="1d440-104">Immuabilité et la modification des paramètres de clé</span><span class="sxs-lookup"><span data-stu-id="1d440-104">Key Immutability and changing settings</span></span>

<span data-ttu-id="1d440-105">Une fois qu’un objet est persistant dans le magasin de sauvegarde, sa représentation est toujours fixe.</span><span class="sxs-lookup"><span data-stu-id="1d440-105">Once an object is persisted to the backing store, its representation is forever fixed.</span></span> <span data-ttu-id="1d440-106">Nouvelles données peuvent être ajoutées au magasin de stockage, mais les données existantes ne peuvent jamais être mutées.</span><span class="sxs-lookup"><span data-stu-id="1d440-106">New data can be added to the backing store, but existing data can never be mutated.</span></span> <span data-ttu-id="1d440-107">L’objectif principal de ce comportement est afin d’éviter une altération des données.</span><span class="sxs-lookup"><span data-stu-id="1d440-107">The primary purpose of this behavior is to prevent data corruption.</span></span>

<span data-ttu-id="1d440-108">Une conséquence de ce comportement est qu’une fois qu’une clé est écrite dans le magasin de stockage, il est immuable.</span><span class="sxs-lookup"><span data-stu-id="1d440-108">One consequence of this behavior is that once a key is written to the backing store, it is immutable.</span></span> <span data-ttu-id="1d440-109">Ses dates de création, d’activation et d’expiration ne sont pas modifiables, si elle peut révoquées à l’aide de `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="1d440-109">Its creation, activation, and expiration dates can never be changed, though it can revoked by using `IKeyManager`.</span></span> <span data-ttu-id="1d440-110">En outre, ses informations algorithmiques sous-jacentes, support de gestion et le chiffrement sur les propriétés rest sont également immuables.</span><span class="sxs-lookup"><span data-stu-id="1d440-110">Additionally, its underlying algorithmic information, master keying material, and encryption at rest properties are also immutable.</span></span>

<span data-ttu-id="1d440-111">Si le développeur modifie un paramètre qui affecte la persistance des clés, ces modifications ne prendront effet jusqu'à la prochaine génération d’une clé, soit via un appel explicite à `IKeyManager.CreateNewKey` ou via de la protection système de données propre [clé automatique génération](key-management.md#data-protection-implementation-key-management) comportement.</span><span class="sxs-lookup"><span data-stu-id="1d440-111">If the developer changes any setting that affects key persistence, those changes will not go into effect until the next time a key is generated, either via an explicit call to `IKeyManager.CreateNewKey` or via the data protection system's own [automatic key generation](key-management.md#data-protection-implementation-key-management) behavior.</span></span> <span data-ttu-id="1d440-112">Les paramètres qui affectent la persistance des clés sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="1d440-112">The settings that affect key persistence are as follows:</span></span>

* [<span data-ttu-id="1d440-113">La durée de vie de clé par défaut</span><span class="sxs-lookup"><span data-stu-id="1d440-113">The default key lifetime</span></span>](key-management.md#data-protection-implementation-key-management)

* [<span data-ttu-id="1d440-114">Le chiffrement à clé au mécanisme de rest</span><span class="sxs-lookup"><span data-stu-id="1d440-114">The key encryption at rest mechanism</span></span>](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)

* [<span data-ttu-id="1d440-115">Les informations algorithmiques contenues dans la clé</span><span class="sxs-lookup"><span data-stu-id="1d440-115">The algorithmic information contained within the key</span></span>](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

<span data-ttu-id="1d440-116">Si vous avez besoin de ces paramètres à bannir antérieures à la clé automatique suivante propagée temps, envisagez de faire un appel explicite à `IKeyManager.CreateNewKey` pour forcer la création d’une nouvelle clé.</span><span class="sxs-lookup"><span data-stu-id="1d440-116">If you need these settings to kick in earlier than the next automatic key rolling time, consider making an explicit call to `IKeyManager.CreateNewKey` to force the creation of a new key.</span></span> <span data-ttu-id="1d440-117">N’oubliez pas de fournir une date de l’activation explicite ({maintenant + 2 jours} est une règle empirique prévoir un délai pour propager les modifications) et la date d’expiration dans l’appel.</span><span class="sxs-lookup"><span data-stu-id="1d440-117">Remember to provide an explicit activation date ({ now + 2 days } is a good rule of thumb to allow time for the change to propagate) and expiration date in the call.</span></span>

>[!TIP]
> <span data-ttu-id="1d440-118">Toutes les applications toucher le référentiel doivent spécifier les mêmes paramètres avec le `IDataProtectionBuilder` les méthodes d’extension.</span><span class="sxs-lookup"><span data-stu-id="1d440-118">All applications touching the repository should specify the same settings with the `IDataProtectionBuilder` extension methods.</span></span> <span data-ttu-id="1d440-119">Dans le cas contraire, les propriétés de la clé persistante seront dépendantes de l’application spécifique qui a appelé les routines de la génération de clés.</span><span class="sxs-lookup"><span data-stu-id="1d440-119">Otherwise, the properties of the persisted key will be dependent on the particular application that invoked the key generation routines.</span></span>
