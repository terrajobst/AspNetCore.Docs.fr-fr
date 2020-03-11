---
title: Étapes suivantes - DevOps avec ASP.NET Core et Azure
author: CamSoper
description: Autres voies de formation pour les opérations de développement avec ASP.NET Core et Azure.
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/next-steps
ms.openlocfilehash: a775dc42551a43bcce72b5f9ca364ed00b1dc4e6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659473"
---
# <a name="next-steps"></a>Étapes suivantes

Dans ce guide, vous avez créé un pipeline DevOps pour un exemple d’application ASP.NET Core. Félicitations ! Nous espérons que vous avez apprécié learning pour publier des applications web ASP.NET Core sur Azure App Service et automatiser l’intégration continue de modifications.

Au-delà d’hébergement web et DevOps, Azure propose un large éventail de services Platform-as-a-Service (PaaS) utiles pour les développeurs ASP.NET Core. Cette section donne un bref aperçu de certains des services plus couramment utilisés.

## <a name="storage-and-databases"></a>Stockage et les bases de données

Le [cache redims](/azure/redis-cache/) est une mise en cache de données à débit élevé et à faible latence disponible en tant que service. Il peut être utilisé pour la mise en cache de sortie de page, en réduisant les demandes de base de données et fournissant l’état de session ASP.NET Core sur plusieurs instances d’une application.

Le [stockage Azure](/azure/storage/) est le stockage cloud à grande échelle d’Azure. Les développeurs peuvent tirer parti du [stockage file d’attente](/azure/storage/queues/storage-queues-introduction) pour la mise en file d’attente de messages fiable et le [stockage table](/azure/storage/tables/table-storage-overview) est un magasin de valeurs clés NoSQL conçu pour un développement rapide à l’aide de jeux de données volumineux et semi-structurés.

[Azure SQL Database](/azure/sql-database/) fournit des fonctionnalités de base de données relationnelles familières en tant que service à l’aide du moteur de Microsoft SQL Server.

[Cosmos DB](/azure/cosmos-db/) service de base de données NoSQL multimodèle distribué à l’échelle mondiale. Plusieurs API est disponibles, y compris les API SQL (anciennement DocumentDB), Cassandra et MongoDB.

## <a name="identity"></a>Identité

[Azure Active Directory](/azure/active-directory/) et [Azure Active Directory B2C](/azure/active-directory-b2c/) sont tous deux des services d’identité. Azure Active Directory est conçu pour les scénarios d’entreprise et permet la collaboration Azure AD B2B (business-to-business), tandis que Azure Active Directory B2C est prévues scénarios business-to-customer, y compris l’authentification de réseau social.

## <a name="mobile"></a>Mobile

[Notification hubs](/azure/notification-hubs/) est un moteur de notifications push multi-plateforme et évolutif pour envoyer rapidement des millions de messages à des applications exécutées sur différents types d’appareils.

## <a name="web-infrastructure"></a>Infrastructure Web

[Azure Container Service](/azure/aks/) gère votre environnement Kubernetes hébergé, ce qui vous permet de déployer et de gérer rapidement et facilement des applications en conteneur sans l’expertise de l’orchestration de conteneur.

[Recherche Azure](/azure/search/) est utilisé pour créer une solution de recherche d’entreprise sur du contenu privé et hétérogène.

[Service Fabric](/azure/service-fabric/) est une plateforme de systèmes distribués qui facilite l’empaquetage, le déploiement et la gestion de microservices et de conteneurs évolutifs et fiables.
