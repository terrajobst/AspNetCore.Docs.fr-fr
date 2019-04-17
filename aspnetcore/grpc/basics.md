---
title: Services gRPC avec C#
author: juntaoluo
description: Découvrez les concepts de base lors de l’écriture de services gRPC avec C#.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/31/2019
uid: grpc/basics
ms.openlocfilehash: 7c5ecf21124414b21f5c36b76e90bde67ac1f958
ms.sourcegitcommit: 57a974556acd09363a58f38c26f74dc21e0d4339
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59672669"
---
# <a name="grpc-services-with-c"></a><span data-ttu-id="2e130-103">services de gRPC avec C\#</span><span class="sxs-lookup"><span data-stu-id="2e130-103">gRPC services with C\#</span></span>

<span data-ttu-id="2e130-104">Ce document décrit les concepts nécessaires à l’écriture [gRPC](https://grpc.io/docs/guides/) applications dans C#.</span><span class="sxs-lookup"><span data-stu-id="2e130-104">This document outlines the concepts needed to write [gRPC](https://grpc.io/docs/guides/) apps in C#.</span></span> <span data-ttu-id="2e130-105">Les sujets abordés ici s’appliquent aux deux [C-core](https://grpc.io/blog/grpc-stacks)-gRPC et ASP.NET Core-applications.</span><span class="sxs-lookup"><span data-stu-id="2e130-105">The topics covered here apply to both [C-core](https://grpc.io/blog/grpc-stacks)-based and ASP.NET Core-based gRPC apps.</span></span>

## <a name="proto-file"></a><span data-ttu-id="2e130-106">proto fichier</span><span class="sxs-lookup"><span data-stu-id="2e130-106">proto file</span></span>

<span data-ttu-id="2e130-107">gRPC utilise une approche contrat d’abord au développement d’API.</span><span class="sxs-lookup"><span data-stu-id="2e130-107">gRPC uses a contract-first approach to API development.</span></span> <span data-ttu-id="2e130-108">Mémoires tampons de protocole (protobuf) sont utilisés en tant que la conception de langage IDL (Interface) par défaut.</span><span class="sxs-lookup"><span data-stu-id="2e130-108">Protocol buffers (protobuf) are used as the Interface Design Language (IDL) by default.</span></span> <span data-ttu-id="2e130-109">Le *.proto* fichier contient :</span><span class="sxs-lookup"><span data-stu-id="2e130-109">The *.proto* file contains:</span></span>

* <span data-ttu-id="2e130-110">La définition du service gRPC.</span><span class="sxs-lookup"><span data-stu-id="2e130-110">The definition of the gRPC service.</span></span>
* <span data-ttu-id="2e130-111">Les messages envoyés entre les clients et serveurs.</span><span class="sxs-lookup"><span data-stu-id="2e130-111">The messages sent between clients and servers.</span></span>

<span data-ttu-id="2e130-112">Pour plus d’informations sur la syntaxe des fichiers de protobuf, consultez le [documentation officielle (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).</span><span class="sxs-lookup"><span data-stu-id="2e130-112">For more information on the syntax of protobuf files, see the [official documentation (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).</span></span>

<span data-ttu-id="2e130-113">Par exemple, considérez le *greet.proto* fichier utilisé dans [prise en main gRPC service](xref:tutorials/grpc/grpc-start):</span><span class="sxs-lookup"><span data-stu-id="2e130-113">For example, consider the *greet.proto* file used in [Get started with gRPC service](xref:tutorials/grpc/grpc-start):</span></span>

* <span data-ttu-id="2e130-114">Définit un `Greeter` service.</span><span class="sxs-lookup"><span data-stu-id="2e130-114">Defines a `Greeter` service.</span></span>
* <span data-ttu-id="2e130-115">Le `Greeter` service définit un `SayHello` appeler.</span><span class="sxs-lookup"><span data-stu-id="2e130-115">The `Greeter` service defines a `SayHello` call.</span></span>
* <span data-ttu-id="2e130-116">`SayHello` envoie un `HelloRequest` du message et reçoit un `HelloResponse` message :</span><span class="sxs-lookup"><span data-stu-id="2e130-116">`SayHello` sends a `HelloRequest` message and receives a `HelloResponse` message:</span></span>

[!code-proto[](~/tutorials/grpc/grpc-start/samples/GrpcGreeter/Protos/greet.proto)]

## <a name="add-a-proto-file-to-a-c-app"></a><span data-ttu-id="2e130-117">Ajouter un fichier .proto à un C\# application</span><span class="sxs-lookup"><span data-stu-id="2e130-117">Add a .proto file to a C\# app</span></span>

<span data-ttu-id="2e130-118">Le *.proto* fichier est inclus dans un projet en l’ajoutant à la `<Protobuf>` groupe d’éléments :</span><span class="sxs-lookup"><span data-stu-id="2e130-118">The *.proto* file is included in a project by adding it to the `<Protobuf>` item group:</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/samples/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-11)]

## <a name="c-tooling-support-for-proto-files"></a><span data-ttu-id="2e130-119">C#Prise en charge pour les fichiers de .proto outils</span><span class="sxs-lookup"><span data-stu-id="2e130-119">C# Tooling support for .proto files</span></span>

<span data-ttu-id="2e130-120">Le package d’outils [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/) est requis pour générer le C# actifs à partir de *.proto* fichiers.</span><span class="sxs-lookup"><span data-stu-id="2e130-120">The tooling package [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/) is required to generate the C# assets from *.proto* files.</span></span> <span data-ttu-id="2e130-121">Ressources générées (fichiers) :</span><span class="sxs-lookup"><span data-stu-id="2e130-121">The generated assets (files):</span></span>

* <span data-ttu-id="2e130-122">Sont générés chaque fois que le projet est généré sur une en fonction des besoins.</span><span class="sxs-lookup"><span data-stu-id="2e130-122">Are generated on an as-needed basis each time the project is built.</span></span>
* <span data-ttu-id="2e130-123">Ne sont pas ajoutés au projet ou archivé dans le contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="2e130-123">Aren't added to the project or checked into source control.</span></span>
* <span data-ttu-id="2e130-124">Sont un artefact de build contenu dans le *obj* directory.</span><span class="sxs-lookup"><span data-stu-id="2e130-124">Are a build artifact contained in the *obj* directory.</span></span>

<span data-ttu-id="2e130-125">Ce package est requis par les projets client et serveur.</span><span class="sxs-lookup"><span data-stu-id="2e130-125">This package is required by both the server and client projects.</span></span> <span data-ttu-id="2e130-126">`Grpc.Tools` peut être ajouté en utilisant le Gestionnaire de Package dans Visual Studio ou en ajoutant un `<PackageReference>` au fichier projet :</span><span class="sxs-lookup"><span data-stu-id="2e130-126">`Grpc.Tools` can be added by using the Package Manager in Visual Studio or adding a `<PackageReference>` to the project file:</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/samples/GrpcGreeter/GrpcGreeter.csproj?highlight=1&range=17)]

<span data-ttu-id="2e130-127">Le package d’outils n’est pas nécessaire lors de l’exécution, donc la dépendance est marquée avec `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="2e130-127">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

## <a name="generated-c-assets"></a><span data-ttu-id="2e130-128">Généré C# actifs</span><span class="sxs-lookup"><span data-stu-id="2e130-128">Generated C# assets</span></span>

<span data-ttu-id="2e130-129">Le package d’outils génère le C# types représentant les messages définis dans les éléments inclus *.proto* fichiers.</span><span class="sxs-lookup"><span data-stu-id="2e130-129">The tooling package generates the C# types representing the messages defined in the included *.proto* files.</span></span>

<span data-ttu-id="2e130-130">Pour les ressources côté serveur, un type de base abstrait de service est généré.</span><span class="sxs-lookup"><span data-stu-id="2e130-130">For server-side assets, an abstract service base type is generated.</span></span> <span data-ttu-id="2e130-131">Le type de base contient les définitions de tous les appels de gRPC contenus dans le *.proto* fichier.</span><span class="sxs-lookup"><span data-stu-id="2e130-131">The base type contains the definitions of all the gRPC calls contained in the *.proto* file.</span></span> <span data-ttu-id="2e130-132">Créer une implémentation concrète de service qui dérive de ce type de base et implémente la logique pour les appels de gRPC.</span><span class="sxs-lookup"><span data-stu-id="2e130-132">Create a concrete service implementation that derives from this base type and implements the logic for the gRPC calls.</span></span> <span data-ttu-id="2e130-133">Pour le `greet.proto`, l’exemple décrit précédemment, abstraite `GreeterBase` type qui contient une machine virtuelle `SayHello` méthode est générée.</span><span class="sxs-lookup"><span data-stu-id="2e130-133">For the `greet.proto`, the example described previously, an abstract `GreeterBase` type that contains a virtual `SayHello` method is generated.</span></span> <span data-ttu-id="2e130-134">Une implémentation concrète `GreeterService` substitue la méthode et implémente la logique de gestion de l’appel de gRPC.</span><span class="sxs-lookup"><span data-stu-id="2e130-134">A concrete implementation `GreeterService` overrides the method and implements the logic handling the gRPC call.</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/samples/GrpcGreeter/Services/GreeterService.cs?name=snippet)]

<span data-ttu-id="2e130-135">Pour les ressources côté client, un type concret client est généré.</span><span class="sxs-lookup"><span data-stu-id="2e130-135">For client-side assets, a concrete client type is generated.</span></span> <span data-ttu-id="2e130-136">Le gRPC appelle le *.proto* fichier sont converties en méthodes sur le type concret, ce qui peut être appelée.</span><span class="sxs-lookup"><span data-stu-id="2e130-136">The gRPC calls in the *.proto* file are translated into methods on the concrete type, which can be called.</span></span> <span data-ttu-id="2e130-137">Pour le `greet.proto`, l’exemple décrit précédemment, un concret `GreeterClient` type est généré.</span><span class="sxs-lookup"><span data-stu-id="2e130-137">For the `greet.proto`, the example described previously, a concrete `GreeterClient` type is generated.</span></span> <span data-ttu-id="2e130-138">Appelez `GreeterClient.SayHello` pour lancer un appel de gRPC sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="2e130-138">Call `GreeterClient.SayHello` to initiate a gRPC call to the server.</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?highlight=5-8&name=snippet)]

<span data-ttu-id="2e130-139">Par défaut, les ressources de serveur et client sont générés pour chacun *.proto* fichier inclus dans le `<Protobuf>` groupe d’éléments.</span><span class="sxs-lookup"><span data-stu-id="2e130-139">By default, server and client assets are generated for each *.proto* file included in the `<Protobuf>` item group.</span></span> <span data-ttu-id="2e130-140">Pour ne garantir que les composants de serveur sont générées dans un projet de serveur, le `GrpcServices` attribut a la valeur `Server`.</span><span class="sxs-lookup"><span data-stu-id="2e130-140">To ensure only the server assets are generated in a server project, the `GrpcServices` attribute is set to `Server`.</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/samples/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-11)]

<span data-ttu-id="2e130-141">De même, l’attribut est défini sur `Client` dans les projets de client.</span><span class="sxs-lookup"><span data-stu-id="2e130-141">Similarly, the attribute is set to `Client` in client projects.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2e130-142">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="2e130-142">Additional resources</span></span>

* <xref:grpc/index>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/migration>
