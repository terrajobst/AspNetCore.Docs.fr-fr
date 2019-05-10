---
title: Hiérarchie d’objectifs et architecture mutualisée dans ASP.NET Core
author: rick-anderson
description: En savoir plus sur la hiérarchie de chaîne d’usage et une architecture mutualisée par rapport à l’API de Protection des données ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/purpose-strings-multitenancy
ms.openlocfilehash: 1133d40e7b325d58b3f70e7387494dae36ff8ac9
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896796"
---
# <a name="purpose-hierarchy-and-multi-tenancy-in-aspnet-core"></a>Hiérarchie d’objectifs et architecture mutualisée dans ASP.NET Core

Dans la mesure où un `IDataProtector` est également implicitement un `IDataProtectionProvider`, à des fins peuvent être chaînés ensemble. Dans ce sens, `provider.CreateProtector([ "purpose1", "purpose2" ])` équivaut à `provider.CreateProtector("purpose1").CreateProtector("purpose2")`.

Ainsi, pour certaines relations hiérarchiques intéressantes via le système de protection des données. Dans l’exemple précédent de [Contoso.Messaging.SecureMessage](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-contoso-purpose), le composant SecureMessage peut appeler `provider.CreateProtector("Contoso.Messaging.SecureMessage")` initiaux qu’une seule fois et met en cache le résultat dans un privé `_myProvider` champ. Protecteurs de futures peuvent ensuite être créées via des appels à `_myProvider.CreateProtector("User: username")`, et ces protections sont utilisées pour sécuriser les messages individuels.

Cela peut également être retournée. Imaginez une application de logique unique qui héberge plusieurs locataires (un CMS semble raisonnable) et chaque client peuvent être configuré avec son propre système de gestion de l’authentification et l’état. L’application PARAPLUIE possède un seul fournisseur de master, et il appelle `provider.CreateProtector("Tenant 1")` et `provider.CreateProtector("Tenant 2")` afin de donner à chaque client sa propre tranche isolé du système de protection des données. Les locataires peuvent ensuite dériver leurs propres protecteurs individuels en fonction de leurs propres besoins, mais quel que soit le hard ils tentent ne peut pas créer protecteurs qui sont en conflit avec un autre client dans le système. Sous forme de graphique, cela est représenté comme suit.

![À des fins de location de multiples](purpose-strings-multitenancy/_static/purposes-multi-tenancy.png)

>[!WARNING]
> Cela suppose l’enseigne de contrôles d’application les API sont disponibles pour les locataires individuels et que les clients ne peuvent pas exécuter du code arbitraire sur le serveur. Si un client peut exécuter du code arbitraire, qu’ils puissent exécuter réflexion privée pour interrompre les garanties d’isolation, ou ils simplement de lire le jeu de clés principal directement et dériver les sous-clés ils le souhaitent.

Le système de protection des données utilise en fait un tri d’une architecture mutualisée dans sa configuration out-of-the-box de valeur par défaut. Par défaut le support de gestion est stockée dans le dossier du profil utilisateur du compte de processus de travail (ou le Registre pour les identités du pool d’applications IIS). Mais il est en fait assez courant d’utiliser un seul compte pour exécuter plusieurs applications, et donc toutes ces applications seraient finissent par partager le maître de support de gestion. Pour résoudre ce problème, le système de protection des données insère automatiquement un identificateur unique par application comme premier élément dans la chaîne d’objectif global. Cet effet implicit sert à [isoler les applications individuelles](xref:security/data-protection/configuration/overview#per-application-isolation) uns des autres par traiter efficacement chaque application comme un client unique au sein du système et le processus de création de protecteur est identique à l’image ci-dessus.
