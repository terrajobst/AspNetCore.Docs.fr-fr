---
title: Présentation de gRPC sur ASP.NET Core
author: juntaoluo
description: Découvrez les services gRPC avec le serveur Kestrel et la pile ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 02/26/2019
uid: grpc/index
---
# <a name="introduction-to-grpc-on-aspnet-core"></a>Présentation de gRPC sur ASP.NET Core

Par [John Luo](https://github.com/juntaoluo)

[gRPC](https://grpc.io/docs/guides/) est un framework d’appel de procédure distante (RPC) indépendant du langage et très performant. Pour plus d’informations sur les notions de base de gRPC, consultez la [page de documentation gRPC](https://grpc.io/docs/).

Les principaux avantages de gRPC sont :
* Framework RPC léger à haut niveau de performance et moderne.
* Développement d’API « Contract-first », à l’aide de mémoires tampons de protocole par défaut, permettant des implémentations indépendantes du langage.
* Outils disponibles pour de nombreux langages afin de générer des clients et serveurs fortement typés.
* Prise en charge d’appels de streaming client, serveur et bidirectionnels.
* Utilisation réduite du réseau avec sérialisation binaire Protobuf.

Ces avantages font de gRPC la solution idéale pour :
* Les microservices légers où l’efficacité est essentielle.
* Les systèmes polyglottes où plusieurs langages sont nécessaires au développement.
* Les services en temps réel de point à point qui doivent gérer des demandes ou réponses de streaming.

Alors qu’une implémentation C# est actuellement disponible dans la [page gRPC](https://grpc.io/docs/quickstart/csharp.html) officielle, l’implémentation actuelle s’appuie sur la bibliothèque native écrite en C (gRPC [C-core](https://grpc.io/blog/grpc-stacks)). Des efforts sont en cours pour fournir une nouvelle implémentation basée sur le serveur HTTP Kestrel et la pile ASP.NET Core entièrement managée. Les documents suivants fournissent une présentation de la création de services gRPC avec cette nouvelle implémentation.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:grpc/basics>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/migration>