---
title: Étapes suivantes - DevOps avec ASP.NET Core et Azure
author: CamSoper
description: Autres voies de formation pour les opérations de développement avec ASP.NET Core et Azure.
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/next-steps
ms.openlocfilehash: 7c3b1c701b13b2a2052c72f5f84bba33d4995ad7
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121322"
---
# <a name="next-steps"></a>Étapes suivantes

Dans ce guide, vous avez créé un pipeline DevOps pour un exemple d’application ASP.NET Core. Félicitations ! Nous espérons que vous avez apprécié learning pour publier des applications web ASP.NET Core sur Azure App Service et automatiser l’intégration continue de modifications.

Au-delà d’hébergement web et DevOps, Azure propose un large éventail de services Platform-as-a-Service (PaaS) utiles pour les développeurs ASP.NET Core. Cette section donne un bref aperçu de certains des services plus couramment utilisés.

## <a name="storage-and-databases"></a>Stockage et les bases de données

[Cache redis](/azure/redis-cache/) disponible en tant que service de mise en cache de données à débit élevé et à faible latence. Il peut être utilisé pour la mise en cache de sortie de page, en réduisant les demandes de base de données et fournissant l’état de session ASP.NET Core sur plusieurs instances d’une application.

[Stockage Azure](/azure/storage/) est un stockage cloud hautement évolutif d’Azure. Les développeurs peuvent profiter de [stockage file d’attente](/azure/storage/queues/storage-queues-introduction) en file d’attente de messages fiable, et [stockage Table](/azure/storage/tables/table-storage-overview) est un magasin de clé-valeur NoSQL conçu pour un développement rapide à l’aide de jeux de données massifs et semi-structurées.

[Base de données SQL Azure](/azure/sql-database/) fournit des fonctionnalités de base de données relationnelle en tant que service utilisant le moteur Microsoft SQL Server.

[COSMOS DB](/azure/cosmos-db/) service de base de données NoSQL multimodèle, distribué dans le monde entier. Plusieurs API est disponibles, y compris les API SQL (anciennement DocumentDB), Cassandra et MongoDB.

## <a name="identity"></a>Identité

[Azure Active Directory](/azure/active-directory/) et [Azure Active Directory B2C](/azure/active-directory-b2c/) sont les deux services d’identité. Azure Active Directory est conçu pour les scénarios d’entreprise et permet la collaboration Azure AD B2B (business-to-business), tandis que Azure Active Directory B2C est prévues scénarios business-to-customer, y compris l’authentification de réseau social.

## <a name="mobile"></a>Mobile

[Notification Hubs](/azure/notification-hubs/) est un moteur de notifications push multiplateforme et évolutive pour envoyer rapidement des millions de messages pour les applications qui s’exécutent sur différents types d’appareils.

## <a name="web-infrastructure"></a>Infrastructure Web

[Azure Container Service](/azure/aks/) gère votre environnement Kubernetes hébergé, accélérant et faciles à déployer et gérer des applications conteneurisées sans expertise d’orchestration de conteneur.

[Recherche Azure](/azure/search/) est utilisé pour créer une solution d’entreprise sur le contenu privé et hétérogène.

[Service Fabric](/azure/service-fabric/) est une plateforme de systèmes distribués qui facilite à empaqueter, déployer et gérer évolutive et de microservices fiables et de conteneurs.
