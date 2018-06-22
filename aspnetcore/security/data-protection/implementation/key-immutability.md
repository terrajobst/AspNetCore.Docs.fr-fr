---
title: Immuabilité clée et les paramètres de clé dans ASP.NET Core
author: rick-anderson
description: Découvrez les détails d’implémentation de l’immuabilité de clé de Protection des données ASP.NET Core API.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 45460e0bdf6ad0a978f984d55b64f562c13fb575
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279452"
---
# <a name="key-immutability-and-key-settings-in-aspnet-core"></a>Immuabilité clée et les paramètres de clé dans ASP.NET Core

Une fois qu’un objet est persistant dans le magasin de sauvegarde, sa représentation est toujours fixe. Nouvelles données peuvent être ajoutées au magasin de stockage, mais les données existantes ne peuvent jamais être mutées. L’objectif principal de ce comportement est afin d’éviter une altération des données.

Une conséquence de ce comportement est qu’une fois qu’une clé est écrite dans le magasin de stockage, il est immuable. Ses dates de création, d’activation et d’expiration ne sont pas modifiables, si elle peut révoquées à l’aide de `IKeyManager`. En outre, ses informations algorithmiques sous-jacentes, support de gestion et le chiffrement sur les propriétés rest sont également immuables.

Si le développeur modifie un paramètre qui affecte la persistance des clés, ces modifications ne sont pas en vigueur jusqu'à la prochaine génération d’une clé, soit via un appel explicite à `IKeyManager.CreateNewKey` ou via de la protection système de données propre [clé automatique génération](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) comportement. Les paramètres qui affectent la persistance des clés sont les suivantes :

* [La durée de vie de clé par défaut](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)

* [Le chiffrement à clé au mécanisme de rest](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest)

* [Les informations algorithmiques contenues dans la clé](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

Si vous avez besoin de ces paramètres à bannir antérieures à la clé automatique suivante propagée temps, envisagez de faire un appel explicite à `IKeyManager.CreateNewKey` pour forcer la création d’une nouvelle clé. N’oubliez pas de fournir une date de l’activation explicite ({maintenant + 2 jours} est une règle empirique prévoir un délai pour propager les modifications) et la date d’expiration dans l’appel.

>[!TIP]
> Toutes les applications toucher le référentiel doivent spécifier les mêmes paramètres avec le `IDataProtectionBuilder` les méthodes d’extension. Dans le cas contraire, les propriétés de la clé persistante seront dépendantes de l’application spécifique qui a appelé les routines de la génération de clés.
