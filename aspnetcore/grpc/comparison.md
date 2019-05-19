---
title: Comparaison des services gRPC avec les API HTTP
author: jamesnk
description: Découvrez comment compare gRPC avec HTTP APIs et il a recommandons sont des scénarios.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 03/31/2019
uid: grpc/comparison
ms.openlocfilehash: 655c921788deb30f3c0f3b47f4440dc8701c0f59
ms.sourcegitcommit: ccbb84ae307a5bc527441d3d509c20b5c1edde05
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2019
ms.locfileid: "65874940"
---
# <a name="comparing-grpc-services-with-http-apis"></a>Comparaison des services gRPC avec les API HTTP

Par [James Newton-King](https://twitter.com/jamesnk)

Cet article explique comment [gRPC services](https://grpc.io/docs/guides/) comparer à APIs HTTP (y compris ASP.NET Core [API Web](xref:web-api/index)). La technologie utilisée pour fournir une API pour votre application est un choix important et gRPC offre des avantages uniques par rapport à APIs HTTP. Cet article étudie les forces et faiblesses de gRPC et recommande des scénarios d’utilisation gRPC par rapport aux autres technologies.

#### <a name="overview"></a>Vue d'ensemble

|    Fonctionnalité             |    gRPC                                                 |    API HTTP avec JSON                       |
|------------------------|---------------------------------------------------------|----------------------------------------------|
|    Contrat            |    Requis (`*.proto`)                                 |    Facultatif (OpenAPI)                        |
|    Transport           |    HTTP/2                                               |    HTTP                                      |
|    Charge utile             |    [Protobuf (petite, binaire)](#performance)             |    JSON (lisibles par l’homme volumineux)              |
|    Prescriptiveness    |    [Spécification stricte](#strict-specification)        |    Libre. N’importe quel HTTP est valide                  |
|    Diffusion en continu           |    [Client, serveur, bidirectionnel](#streaming)         |    Client, serveur                            |
|    Prise en charge du navigateur     |    [Non (requiert grpc-web)](#limited-browser-support)   |    Oui                                       |
|    Sécurité            |    Transport (HTTPS)                                    |    Transport (HTTPS)                         |
|    Génération de code client     |    [Oui](#code-generation)                              |    OpenAPI et les outils de fournisseurs tiers             |

## <a name="grpc-strengths"></a>points forts de gRPC

### <a name="performance"></a>Performances

messages de gRPC sont sérialisés à l’aide de [Protobuf](https://developers.google.com/protocol-buffers/docs/overview), un format de message binaire efficace. Protobuf sérialise très rapidement sur le serveur et le client. Résultats de sérialisation Protobuf dans les charges utiles de message de petite taille, importants dans les scénarios de bande passante limitée, comme les applications mobiles.

gRPC est conçu pour HTTP/2, une révision majeure de HTTP qui offre les avantages de performances significatifs sur HTTP 1.x :

* Tramage binaire et compression. Protocole HTTP/2 est compact et efficace à la fois dans l’envoi et la réception.
* Multiplexage de plusieurs appels HTTP/2 via une connexion TCP unique. MULTIPLEXAGE élimine [blocage de l’en-tête de ligne](https://en.wikipedia.org/wiki/Head-of-line_blocking).

### <a name="code-generation"></a>Génération de code

Toutes les infrastructures gRPC fournissent une excellente prise en charge pour la génération de code. Est un fichier de base au développement de gRPC la [ `*.proto` fichier](https://developers.google.com/protocol-buffers/docs/proto3), qui définit le contrat de services de gRPC et les messages. À partir de cette infrastructures gRPC de fichier code génère une classe de base du service, les messages et un client complet.

En partageant le `*.proto` fichier entre le serveur et le client, le messages et le code client peut être généré à partir de la fin à la fin. Génération de code du client élimine la duplication des messages sur le serveur et client et crée un client fortement typés pour vous. Ne pas avoir à écrire un client gagner du temps de développement significatifs dans les applications avec de nombreux services.

### <a name="strict-specification"></a>Spécification stricte

Une spécification formelle pour l’API HTTP avec JSON n’existe pas. Les développeurs débattre le meilleur format d’URL, les verbes HTTP et les codes de réponse.

Le [gRPC spécification](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-HTTP2.md) est normatif concernant le format d’un service gRPC doit suivre. gRPC élimine le débat et fait gagner du temps de développement étant GPRC pour cohérent entre les plates-formes et les implémentations.

### <a name="streaming"></a>Diffusion en continu

HTTP/2 fournit une base pour les flux de communication de longue durée, en temps réel. gRPC fournit une excellente prise en charge pour la diffusion en continu via HTTP/2.

Un service gRPC prend en charge toutes les combinaisons de diffusion en continu :

* Unaire (aucune diffusion en continu)
* Serveur au client de diffusion en continu
* Client à serveur de diffusion en continu
* Bidirectionnelle de diffusion en continu

### <a name="deadlinetimeouts-and-cancellation"></a>Échéance/des délais d’attente et l’annulation

gRPC permet aux clients de spécifier la durée pendant laquelle ils sont disposés à attendre pour un RPC terminer. Le [échéance](https://grpc.io/blog/deadlines) est envoyé au serveur, et le serveur peut décider de l’action à entreprendre si elle dépasse le délai. Par exemple, le serveur pourrait annuler des demandes de gRPC/HTTP/base de données en cours d’exécution sur le délai d’attente.

Propagation de l’échéance et l’annulation via enfant gRPC appels permet d’appliquer des limites de l’utilisation des ressources.

## <a name="grpc-recommended-scenarios"></a>gRPC recommandé de scénarios

gRPC convient bien pour les scénarios suivants :

* **Microservices** &ndash; gRPC est conçu pour la communication à haut débit et une faible latence. gRPC est très utile pour les microservices léger où l’efficacité est critique.
* **Communication en temps réel de point à point** &ndash; gRPC a excellente prise en charge pour la diffusion en continu bidirectionnel. gRPC services peuvent transmettre des messages en temps réel sans interrogation.
* **Les environnements Polygot** &ndash; gRPC outils prend en charge tous les langages de développement courants, rendre gRPC un bon choix pour les environnements multilingues.
* **Contrainte d’environnements réseau** &ndash; gRPC messages sont sérialisés avec Protobuf, un format de message léger. Un message gRPC est toujours inférieur à un message JSON équivalent.

## <a name="grpc-weaknesses"></a>faiblesses de gRPC

### <a name="limited-browser-support"></a>Prise en charge de navigateur limitée

Il est impossible d’appeler directement un service gRPC à partir d’un navigateur dès aujourd'hui. gRPC utilise massivement les fonctionnalités HTTP/2 et aucun navigateur n’offre le niveau de contrôle requis sur les requêtes web pour prendre en charge d’un client de gRPC. Par exemple, navigateurs ne pas autoriser un appelant à demander l’utilisation de HTTP/2, ou fournir l’accès aux cadres HTTP/2 sous-jacent.

[Web-gRPC](https://grpc.io/docs/tutorials/basic/web.html) est une technologie supplémentaire à partir de l’équipe gRPC qui fournit la prise en charge de gRPC limitée dans le navigateur. Web-gRPC se compose de deux parties : un client JavaScript qui prend en charge tous les navigateurs modernes et un proxy Web de gRPC sur le serveur. Le client Web de gRPC appelle le proxy et le proxy transfère sur les demandes de gRPC sur le serveur de gRPC.

Pas toutes les fonctionnalités de gRPC sont pris en charge par le Web de gRPC. Client et bidirectionnelle de diffusion en continu n’est pas pris en charge, et prise en charge limitée pour le serveur de diffusion en continu.

### <a name="not-human-readable"></a>Pas lisibles par l’homme

Demandes d’API HTTP sont envoyées sous forme de texte et peuvent lire et créés par l’homme.

par défaut, les messages de gRPC sont encodés avec Protobuf. Bien que Protobuf soit efficace pour envoyer et recevoir, son format binaire n’est pas humaine lisible. Protobuf nécessite la description de l’interface du message spécifiée dans le `*.proto` fichier à désérialiser correctement. Outils supplémentaires sont requises pour analyser les charges utiles Protobuf sur le câble et composer des requêtes à la main.

Fonctionnalités telles que [réflexion de serveur](https://github.com/grpc/grpc/blob/master/doc/server-reflection.md) et [outil de ligne de commande gRPC](https://github.com/grpc/grpc/blob/master/doc/command_line_tool.md) existent pour aider aux messages Protobuf binaires. En outre, de prise en charge des messages Protobuf [conversion vers et depuis JSON](https://developers.google.com/protocol-buffers/docs/proto3#json). La conversion de JSON intégrée fournit un moyen efficace de convertir les messages Protobuf vers et depuis un format lisible lors du débogage.

## <a name="alternative-framework-scenarios"></a>Scénarios d’infrastructure autre

Autres infrastructures sont préférables gRPC dans les scénarios suivants :

* **API accessible navigateur** &ndash; gRPC n’est pas entièrement pris en charge dans le navigateur. gRPC-Web peut offrir la prise en charge du navigateur, mais il a des limitations et introduit un serveur proxy.
* **Communication en temps réel de diffusion** &ndash; gRPC prend en charge la communication en temps réel par le biais de diffusion en continu, mais le concept de diffuser un message de sortie pour les connexions enregistrées n’existe pas. Par exemple dans un scénario de salle de conversation où les nouveaux messages de conversation doivent être envoyés à tous les clients dans la salle de conversation, chaque appel gRPC est requis pour diffuser individuellement les nouveaux messages de conversation au client. [SignalR](xref:signalr/introduction) est un cadre utile pour ce scénario. SignalR propose le concept de connexions persistantes et prise en charge intégrée pour diffuser des messages.
* **Communication entre processus** &ndash; un processus doit héberger un serveur HTTP/2 pour accepter les appels entrants gRPC. Pour Windows, communication interprocessus [canaux](/dotnet/standard/io/pipe-operations) est une méthode rapide et léger de communication.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
