---
title: Présentation de gRPC sur ASP.NET Core
author: juntaoluo
description: Découvrez les services gRPC avec le serveur Kestrel et la pile ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 02/26/2019
uid: grpc/index
---
# <a name="introduction-to-grpc-on-aspnet-core"></a><span data-ttu-id="1d2e9-103">Présentation de gRPC sur ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1d2e9-103">Introduction to gRPC on ASP.NET Core</span></span>

<span data-ttu-id="1d2e9-104">Par [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="1d2e9-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="1d2e9-105">[gRPC](https://grpc.io/docs/guides/) est un framework d’appel de procédure distante (RPC) indépendant du langage et très performant.</span><span class="sxs-lookup"><span data-stu-id="1d2e9-105">[gRPC](https://grpc.io/docs/guides/) is a language agnostic, high-performance Remote Procedure Call (RPC) framework.</span></span> <span data-ttu-id="1d2e9-106">Pour plus d’informations sur les notions de base de gRPC, consultez la [page de documentation gRPC](https://grpc.io/docs/).</span><span class="sxs-lookup"><span data-stu-id="1d2e9-106">For more on gRPC fundamentals, see the [gRPC documentation page](https://grpc.io/docs/).</span></span>

<span data-ttu-id="1d2e9-107">Les principaux avantages de gRPC sont :</span><span class="sxs-lookup"><span data-stu-id="1d2e9-107">The main benefits of gRPC are:</span></span>
* <span data-ttu-id="1d2e9-108">Framework RPC léger à haut niveau de performance et moderne.</span><span class="sxs-lookup"><span data-stu-id="1d2e9-108">Modern high-performance lightweight RPC framework.</span></span>
* <span data-ttu-id="1d2e9-109">Développement d’API « Contract-first », à l’aide de mémoires tampons de protocole par défaut, permettant des implémentations indépendantes du langage.</span><span class="sxs-lookup"><span data-stu-id="1d2e9-109">Contract-first API development, using Protocol Buffers by default, allowing for language agnostic implementations.</span></span>
* <span data-ttu-id="1d2e9-110">Outils disponibles pour de nombreux langages afin de générer des clients et serveurs fortement typés.</span><span class="sxs-lookup"><span data-stu-id="1d2e9-110">Tooling available for many languages to generate strongly-typed servers and clients.</span></span>
* <span data-ttu-id="1d2e9-111">Prise en charge d’appels de streaming client, serveur et bidirectionnels.</span><span class="sxs-lookup"><span data-stu-id="1d2e9-111">Supports client, server, and bi-directional streaming calls.</span></span>
* <span data-ttu-id="1d2e9-112">Utilisation réduite du réseau avec sérialisation binaire Protobuf.</span><span class="sxs-lookup"><span data-stu-id="1d2e9-112">Reduced network usage with Protobuf binary serialization.</span></span>

<span data-ttu-id="1d2e9-113">Ces avantages font de gRPC la solution idéale pour :</span><span class="sxs-lookup"><span data-stu-id="1d2e9-113">These benefits make gRPC ideal for:</span></span>
* <span data-ttu-id="1d2e9-114">Les microservices légers où l’efficacité est essentielle.</span><span class="sxs-lookup"><span data-stu-id="1d2e9-114">Lightweight microservices where efficiency is critical.</span></span>
* <span data-ttu-id="1d2e9-115">Les systèmes polyglottes où plusieurs langages sont nécessaires au développement.</span><span class="sxs-lookup"><span data-stu-id="1d2e9-115">Polyglot systems where multiple languages are required for development.</span></span>
* <span data-ttu-id="1d2e9-116">Les services en temps réel de point à point qui doivent gérer des demandes ou réponses de streaming.</span><span class="sxs-lookup"><span data-stu-id="1d2e9-116">Point-to-point real-time services that need to handle streaming requests or responses.</span></span>

<span data-ttu-id="1d2e9-117">Alors qu’une implémentation C# est actuellement disponible dans la [page gRPC](https://grpc.io/docs/quickstart/csharp.html) officielle, l’implémentation actuelle s’appuie sur la bibliothèque native écrite en C (gRPC [C-core](https://grpc.io/blog/grpc-stacks)).</span><span class="sxs-lookup"><span data-stu-id="1d2e9-117">While a C# implementation is currently available on the official [gRPC page](https://grpc.io/docs/quickstart/csharp.html), the current implementation relies on the native library written in C (gRPC [C-core](https://grpc.io/blog/grpc-stacks)).</span></span> <span data-ttu-id="1d2e9-118">Des efforts sont en cours pour fournir une nouvelle implémentation basée sur le serveur HTTP Kestrel et la pile ASP.NET Core entièrement managée.</span><span class="sxs-lookup"><span data-stu-id="1d2e9-118">Work is currently in progress to provide a new implementation based on the Kestrel HTTP server and the ASP.NET Core stack that is fully managed.</span></span> <span data-ttu-id="1d2e9-119">Les documents suivants fournissent une présentation de la création de services gRPC avec cette nouvelle implémentation.</span><span class="sxs-lookup"><span data-stu-id="1d2e9-119">The following documents provide an introduction to building gRPC services with this new implementation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1d2e9-120">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="1d2e9-120">Additional resources</span></span>

* <xref:grpc/basics>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/migration>