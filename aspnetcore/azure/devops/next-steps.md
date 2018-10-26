---
title: DevOps avec ASP.NET Core et Azure | Étapes suivantes
author: CamSoper
description: Un guide qui fournit des conseils de bout en bout sur la création d’un pipeline DevOps pour une application ASP.NET Core hébergée dans Azure.
ms.author: casoper
ms.custom: mvc
ms.date: 10/24/2018
uid: azure/devops/next-steps
ms.openlocfilehash: b82e7251b507f8d141930673d50722cfaa576db5
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50089876"
---
# <a name="next-steps"></a><span data-ttu-id="f4944-103">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="f4944-103">Next steps</span></span>

<span data-ttu-id="f4944-104">Dans ce guide, vous avez créé un pipeline DevOps pour un exemple d’application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f4944-104">In this guide, you created a DevOps pipeline for an ASP.NET Core sample app.</span></span> <span data-ttu-id="f4944-105">Félicitations !</span><span class="sxs-lookup"><span data-stu-id="f4944-105">Congratulations!</span></span> <span data-ttu-id="f4944-106">Nous espérons que vous avez apprécié learning pour publier des applications web ASP.NET Core sur Azure App Service et automatiser l’intégration continue de modifications.</span><span class="sxs-lookup"><span data-stu-id="f4944-106">We hope you enjoyed learning to publish ASP.NET Core web apps to Azure App Service and automate the continuous integration of changes.</span></span>

<span data-ttu-id="f4944-107">Au-delà d’hébergement web et DevOps, Azure propose un large éventail de services Platform-as-a-Service (PaaS) utiles pour les développeurs ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f4944-107">Beyond web hosting and DevOps, Azure has a wide array of Platform-as-a-Service (PaaS) services useful to ASP.NET Core developers.</span></span> <span data-ttu-id="f4944-108">Cette section donne un bref aperçu de certains des services plus couramment utilisés.</span><span class="sxs-lookup"><span data-stu-id="f4944-108">This section gives a brief overview of some of the most commonly used services.</span></span>

## <a name="storage-and-databases"></a><span data-ttu-id="f4944-109">Stockage et les bases de données</span><span class="sxs-lookup"><span data-stu-id="f4944-109">Storage and databases</span></span>

<span data-ttu-id="f4944-110">[Cache redis](/azure/redis-cache/) disponible en tant que service de mise en cache de données à débit élevé et à faible latence.</span><span class="sxs-lookup"><span data-stu-id="f4944-110">[Redis Cache](/azure/redis-cache/) is high-throughput, low-latency data caching available as a service.</span></span> <span data-ttu-id="f4944-111">Il peut être utilisé pour la mise en cache de sortie de page, en réduisant les demandes de base de données et fournissant l’état de session ASP.NET Core sur plusieurs instances d’une application.</span><span class="sxs-lookup"><span data-stu-id="f4944-111">It can be used for caching page output, reducing database requests, and providing ASP.NET Core session state across multiple instances of an app.</span></span>

<span data-ttu-id="f4944-112">[Stockage Azure](/azure/storage/) est un stockage cloud hautement évolutif d’Azure.</span><span class="sxs-lookup"><span data-stu-id="f4944-112">[Azure Storage](/azure/storage/) is Azure's massively scalable cloud storage.</span></span> <span data-ttu-id="f4944-113">Les développeurs peuvent profiter de [stockage file d’attente](/azure/storage/queues/storage-queues-introduction) en file d’attente de messages fiable, et [stockage Table](/azure/storage/tables/table-storage-overview) est un magasin de clé-valeur NoSQL conçu pour un développement rapide à l’aide de jeux de données massifs et semi-structurées.</span><span class="sxs-lookup"><span data-stu-id="f4944-113">Developers can take advantage of [Queue Storage](/azure/storage/queues/storage-queues-introduction) for reliable message queuing, and [Table Storage](/azure/storage/tables/table-storage-overview) is a NoSQL key-value store designed for rapid development using massive, semi-structured data sets.</span></span>

<span data-ttu-id="f4944-114">[Base de données SQL Azure](/azure/sql-database/) fournit des fonctionnalités de base de données relationnelle en tant que service utilisant le moteur Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f4944-114">[Azure SQL Database](/azure/sql-database/) provides familiar relational database functionality as a service using the Microsoft SQL Server Engine.</span></span>

<span data-ttu-id="f4944-115">[COSMOS DB](/azure/cosmos-db/) service de base de données NoSQL multimodèle, distribué dans le monde entier.</span><span class="sxs-lookup"><span data-stu-id="f4944-115">[Cosmos DB](/azure/cosmos-db/) globally distributed, multi-model NoSQL database service.</span></span> <span data-ttu-id="f4944-116">Plusieurs API est disponibles, y compris les API SQL (anciennement DocumentDB), Cassandra et MongoDB.</span><span class="sxs-lookup"><span data-stu-id="f4944-116">Multiple APIs are available, including SQL API (formerly called DocumentDB), Cassandra, and MongoDB.</span></span>

## <a name="identity"></a><span data-ttu-id="f4944-117">Identité</span><span class="sxs-lookup"><span data-stu-id="f4944-117">Identity</span></span>

<span data-ttu-id="f4944-118">[Azure Active Directory](/azure/active-directory/) et [Azure Active Directory B2C](/azure/active-directory-b2c/) sont les deux services d’identité.</span><span class="sxs-lookup"><span data-stu-id="f4944-118">[Azure Active Directory](/azure/active-directory/) and [Azure Active Directory B2C](/azure/active-directory-b2c/) are both identity services.</span></span> <span data-ttu-id="f4944-119">Azure Active Directory est conçu pour les scénarios d’entreprise et permet la collaboration Azure AD B2B (business-to-business), tandis que Azure Active Directory B2C est prévues scénarios business-to-customer, y compris l’authentification de réseau social.</span><span class="sxs-lookup"><span data-stu-id="f4944-119">Azure Active Directory is designed for enterprise scenarios and enables Azure AD B2B (business-to-business) collaboration, while Azure Active Directory B2C is intended business-to-customer scenarios, including social network sign-in.</span></span>

## <a name="mobile"></a><span data-ttu-id="f4944-120">Mobile</span><span class="sxs-lookup"><span data-stu-id="f4944-120">Mobile</span></span>

<span data-ttu-id="f4944-121">[Notification Hubs](/azure/notification-hubs/) est un moteur de notifications push multiplateforme et évolutive pour envoyer rapidement des millions de messages pour les applications qui s’exécutent sur différents types d’appareils.</span><span class="sxs-lookup"><span data-stu-id="f4944-121">[Notification Hubs](/azure/notification-hubs/) is a multi-platform, scalable push-notification engine to quickly send millions of messages to apps running on various types of devices.</span></span>

## <a name="web-infrastructure"></a><span data-ttu-id="f4944-122">Infrastructure Web</span><span class="sxs-lookup"><span data-stu-id="f4944-122">Web infrastructure</span></span>

<span data-ttu-id="f4944-123">[Azure Container Service](/azure/aks/) gère votre environnement Kubernetes hébergé, accélérant et faciles à déployer et gérer des applications conteneurisées sans expertise d’orchestration de conteneur.</span><span class="sxs-lookup"><span data-stu-id="f4944-123">[Azure Container Service](/azure/aks/) manages your hosted Kubernetes environment, making it quick and easy to deploy and manage containerized apps without container orchestration expertise.</span></span>

<span data-ttu-id="f4944-124">[Recherche Azure](/azure/search/) est utilisé pour créer une solution d’entreprise sur le contenu privé et hétérogène.</span><span class="sxs-lookup"><span data-stu-id="f4944-124">[Azure Search](/azure/search/) is used to create an enterprise search solution over private, heterogenous content.</span></span>

<span data-ttu-id="f4944-125">[Service Fabric](/azure/service-fabric/) est une plateforme de systèmes distribués qui facilite à empaqueter, déployer et gérer évolutive et de microservices fiables et de conteneurs.</span><span class="sxs-lookup"><span data-stu-id="f4944-125">[Service Fabric](/azure/service-fabric/) is a distributed systems platform that makes it easy to package, deploy, and manage scalable and reliable microservices and containers.</span></span>
