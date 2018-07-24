---
title: Immuabilité des clés et les paramètres de clé dans ASP.NET Core
author: rick-anderson
description: Découvrez les détails d’implémentation de l’immuabilité de clés de Protection des données ASP.NET Core API.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 7796cb102c0f6f03809704016fd36b28c7a82438
ms.sourcegitcommit: 8f8924ce4eb9effeaf489f177fb01b66867da16f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39219301"
---
# <a name="key-immutability-and-key-settings-in-aspnet-core"></a>Immuabilité des clés et les paramètres de clé dans ASP.NET Core

Une fois qu’un objet est conservé dans le magasin de stockage, sa représentation sous forme a été résolu pour toujours. Nouvelles données peuvent être ajoutées au magasin de stockage, mais les données existantes ne peuvent jamais être mutées. L’objectif principal de ce comportement consiste à empêcher la corruption de données.

Une des conséquences de ce comportement est qu’une fois une clé est écrite dans le magasin de stockage, il est immuable. Ses dates de création, l’activation et d’expiration ne sont pas modifiables, même si elle peut révoquées à l’aide de `IKeyManager`. En outre, ses informations algorithmiques sous-jacentes, le support de gestion et le chiffrement des propriétés de rest sont également immuables.

Si le développeur modifie un paramètre qui affecte la persistance des clés, ces modifications n’entre en vigueur jusqu'à la prochaine génération d’une clé, soit via un appel explicite à `IKeyManager.CreateNewKey` ou par le biais de data protection du système propre [clé automatique génération](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) comportement. Les paramètres qui affectent la persistance des clés sont les suivantes :

* [La durée de vie de clé par défaut](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)

* [Le chiffrement à clé au mécanisme de rest](xref:security/data-protection/implementation/key-encryption-at-rest)

* [Les informations algorithmiques contenues dans la clé](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

Si vous avez besoin de ces paramètres s’antérieures à la clé automatique suivante propagée temps, envisagez de faire un appel explicite à `IKeyManager.CreateNewKey` pour forcer la création d’une nouvelle clé. N’oubliez pas de fournir une date de l’activation explicite ({maintenant + 2 jours} est une règle empirique de prévoir un délai pour le changement de propagation) et la date d’expiration dans l’appel.

>[!TIP]
> Toutes les applications toucher le référentiel doivent spécifier les mêmes paramètres avec le `IDataProtectionBuilder` méthodes d’extension. Sinon, les propriétés de la clé persistante seront dépendantes de l’application particulière qui a appelé les routines de la génération de clé.
