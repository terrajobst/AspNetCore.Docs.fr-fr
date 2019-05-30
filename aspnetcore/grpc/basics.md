---
title: Services gRPC avec C#
author: juntaoluo
description: Découvrez les concepts de base lors de l’écriture de services gRPC avec C#.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/31/2019
uid: grpc/basics
ms.openlocfilehash: 5a88bd0e9f789058b3606691c5ebd9a74325ac9b
ms.sourcegitcommit: 4d05e30567279072f1b070618afe58ae1bcefd5a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376342"
---
# <a name="grpc-services-with-c"></a>services de gRPC avec C\#

Ce document décrit les concepts nécessaires à l’écriture [gRPC](https://grpc.io/docs/guides/) applications dans C#. Les sujets abordés ici s’appliquent aux deux [C-core](https://grpc.io/blog/grpc-stacks)-gRPC et ASP.NET Core-applications.

## <a name="proto-file"></a>proto fichier

gRPC utilise une approche contrat d’abord au développement d’API. Mémoires tampons de protocole (protobuf) sont utilisés en tant que la conception de langage IDL (Interface) par défaut. Le *.proto* fichier contient :

* La définition du service gRPC.
* Les messages envoyés entre les clients et serveurs.

Pour plus d’informations sur la syntaxe des fichiers de protobuf, consultez le [documentation officielle (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).

Par exemple, considérez le *greet.proto* fichier utilisé dans [prise en main gRPC service](xref:tutorials/grpc/grpc-start):

* Définit un `Greeter` service.
* Le `Greeter` service définit un `SayHello` appeler.
* `SayHello` envoie un `HelloRequest` du message et reçoit un `HelloResponse` message :

[!code-proto[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/Protos/greet.proto)]

## <a name="add-a-proto-file-to-a-c-app"></a>Ajouter un fichier .proto à un C\# application

Le *.proto* fichier est inclus dans un projet en l’ajoutant à la `<Protobuf>` groupe d’éléments :

[!code-xml[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-11)]

## <a name="c-tooling-support-for-proto-files"></a>C#Prise en charge pour les fichiers de .proto outils

Le package d’outils [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/) est requis pour générer le C# actifs à partir de *.proto* fichiers. Ressources générées (fichiers) :

* Sont générés chaque fois que le projet est généré sur une en fonction des besoins.
* Ne sont pas ajoutés au projet ou archivé dans le contrôle de code source.
* Sont un artefact de build contenu dans le *obj* directory.

Ce package est requis par les projets client et serveur. `Grpc.Tools` peut être ajouté en utilisant le Gestionnaire de Package dans Visual Studio ou en ajoutant un `<PackageReference>` au fichier projet :

[!code-xml[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=1&range=17)]

Le package d’outils n’est pas nécessaire lors de l’exécution. La dépendance est donc marquée avec `PrivateAssets="All"`.

## <a name="generated-c-assets"></a>Généré C# actifs

Le package d’outils génère le C# types représentant les messages définis dans les éléments inclus *.proto* fichiers.

Pour les ressources côté serveur, un type de base abstrait de service est généré. Le type de base contient les définitions de tous les appels de gRPC contenus dans le *.proto* fichier. Créer une implémentation concrète de service qui dérive de ce type de base et implémente la logique pour les appels de gRPC. Pour le `greet.proto`, l’exemple décrit précédemment, abstraite `GreeterBase` type qui contient une machine virtuelle `SayHello` méthode est générée. Une implémentation concrète `GreeterService` substitue la méthode et implémente la logique de gestion de l’appel de gRPC.

[!code-csharp[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?name=snippet)]

Pour les ressources côté client, un type concret client est généré. Le gRPC appelle le *.proto* fichier sont converties en méthodes sur le type concret, ce qui peut être appelée. Pour le `greet.proto`, l’exemple décrit précédemment, un concret `GreeterClient` type est généré. Appelez `GreeterClient.SayHello` pour lancer un appel de gRPC sur le serveur.

[!code-csharp[](~/tutorials//grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?highlight=5-8&name=snippet)]

Par défaut, les ressources de serveur et client sont générés pour chacun *.proto* fichier inclus dans le `<Protobuf>` groupe d’éléments. Pour ne garantir que les composants de serveur sont générées dans un projet de serveur, le `GrpcServices` attribut a la valeur `Server`.

[!code-xml[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-11)]

De même, l’attribut est défini sur `Client` dans les projets de client.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:grpc/index>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/migration>
