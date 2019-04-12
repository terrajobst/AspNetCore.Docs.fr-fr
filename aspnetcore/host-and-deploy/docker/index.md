---
title: Héberger ASP.NET Core dans des conteneurs Docker
author: rick-anderson
description: Découvrez des liens vers des ressources pour savoir comment héberger des applications ASP.NET Core dans des conteneurs Docker.
ms.author: riande
ms.custom: mvc
ms.date: 01/08/2018
uid: host-and-deploy/docker/index
ms.openlocfilehash: e56f90ec7272ce0411651ee6f8e7c754ae44b78d
ms.sourcegitcommit: 9b7fcb4ce00a3a32e153a080ebfaae4ef417aafa
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59516259"
---
# <a name="host-aspnet-core-in-docker-containers"></a><span data-ttu-id="62884-103">Héberger ASP.NET Core dans des conteneurs Docker</span><span class="sxs-lookup"><span data-stu-id="62884-103">Host ASP.NET Core in Docker containers</span></span>

<span data-ttu-id="62884-104">Les articles suivants sont disponibles pour en savoir plus sur l’hébergement d’applications ASP.NET Core dans Docker :</span><span class="sxs-lookup"><span data-stu-id="62884-104">The following articles are available for learning about hosting ASP.NET Core apps in Docker:</span></span>

[<span data-ttu-id="62884-105">Introduction aux conteneurs et à Docker</span><span class="sxs-lookup"><span data-stu-id="62884-105">Introduction to Containers and Docker</span></span>](/dotnet/standard/microservices-architecture/container-docker-introduction/index)  
<span data-ttu-id="62884-106">Découvrez dans quelle mesure la mise en conteneur est une approche de développement de logiciels qui consiste à empaqueter une application ou un service, ses dépendances et sa configuration sous forme d’image de conteneur.</span><span class="sxs-lookup"><span data-stu-id="62884-106">See how containerization is an approach to software development in which an application or service, its dependencies, and its configuration are packaged together as a container image.</span></span> <span data-ttu-id="62884-107">L’image peut être testée, puis déployée sur un hôte.</span><span class="sxs-lookup"><span data-stu-id="62884-107">The image can be tested and then deployed to a host.</span></span>

[<span data-ttu-id="62884-108">Présentation de Docker</span><span class="sxs-lookup"><span data-stu-id="62884-108">What is Docker</span></span>](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-defined)  
<span data-ttu-id="62884-109">Découvrez Docker, projet open source permettant d’automatiser le déploiement d’applications en tant que conteneurs portables et autonomes exécutables sur le cloud ou localement.</span><span class="sxs-lookup"><span data-stu-id="62884-109">Discover how Docker is an open-source project for automating the deployment of apps as portable, self-sufficient containers that can run on the cloud or on-premises.</span></span>

[<span data-ttu-id="62884-110">Terminologie Docker</span><span class="sxs-lookup"><span data-stu-id="62884-110">Docker Terminology</span></span>](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-terminology)  
<span data-ttu-id="62884-111">Découvrez les termes et définitions de la technologie Docker.</span><span class="sxs-lookup"><span data-stu-id="62884-111">Learn terms and definitions for Docker technology.</span></span>

[<span data-ttu-id="62884-112">Conteneurs, images et registres Docker</span><span class="sxs-lookup"><span data-stu-id="62884-112">Docker containers, images, and registries</span></span>](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-containers-images-registries)  
<span data-ttu-id="62884-113">Découvrez comment les images de conteneur Docker sont stockées dans un registre d’image pour un déploiement cohérent entre les environnements.</span><span class="sxs-lookup"><span data-stu-id="62884-113">Find out how Docker container images are stored in an image registry for consistent deployment across environments.</span></span>

[<span data-ttu-id="62884-114">Visual Studio Tools pour Docker</span><span class="sxs-lookup"><span data-stu-id="62884-114">Visual Studio Tools for Docker</span></span>](xref:host-and-deploy/docker/visual-studio-tools-for-docker)  
<span data-ttu-id="62884-115">Découvrez dans quelle mesure Visual Studio 2017 prend en charge la création, le débogage et l’exécution d’applications ASP.NET Core ciblant le .NET Framework ou .NET Core sur Docker pour Windows.</span><span class="sxs-lookup"><span data-stu-id="62884-115">Discover how Visual Studio 2017 supports building, debugging, and running ASP.NET Core apps targeting either .NET Framework or .NET Core on Docker for Windows.</span></span> <span data-ttu-id="62884-116">Les conteneurs Windows et Linux sont pris en charge.</span><span class="sxs-lookup"><span data-stu-id="62884-116">Both Windows and Linux containers are supported.</span></span>

[<span data-ttu-id="62884-117">Publication sur une image Docker</span><span class="sxs-lookup"><span data-stu-id="62884-117">Publish to a Docker Image</span></span>](/azure/vs-azure-tools-docker-hosting-web-apps-in-docker)  
<span data-ttu-id="62884-118">Découvrez comment utiliser l’extension Visual Studio Tools pour Docker afin de déployer une application ASP.NET Core sur un hôte Docker sur Azure à l’aide de PowerShell.</span><span class="sxs-lookup"><span data-stu-id="62884-118">Find out how to use the Visual Studio Tools for Docker extension to deploy an ASP.NET Core app to a Docker host on Azure using PowerShell.</span></span>

[<span data-ttu-id="62884-119">Configurer ASP.NET Core pour l’utilisation de serveurs proxy et d’équilibreurs de charge</span><span class="sxs-lookup"><span data-stu-id="62884-119">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)  
<span data-ttu-id="62884-120">Une configuration supplémentaire peut être nécessaire pour les applications hébergées derrière des serveurs proxy et des équilibreurs de charge.</span><span class="sxs-lookup"><span data-stu-id="62884-120">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="62884-121">Le passage des requêtes à travers un proxy masque souvent des informations sur la requête d’origine, comme le schéma et l’adresse IP du client.</span><span class="sxs-lookup"><span data-stu-id="62884-121">Passing requests through a proxy often obscures information about the original request, such as the scheme and client IP.</span></span> <span data-ttu-id="62884-122">Il peut être nécessaire de transférer manuellement à l’application certaines informations sur la requête.</span><span class="sxs-lookup"><span data-stu-id="62884-122">It might be necessary to forwarded some information about the request manually to the app.</span></span>
