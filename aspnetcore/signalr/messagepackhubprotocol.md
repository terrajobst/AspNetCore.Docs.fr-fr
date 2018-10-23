---
title: Utiliser le protocole MessagePack Hub dans SignalR pour ASP.NET Core
author: tdykstra
description: Ajouter le protocole MessagePack Hub à ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 0874afc5493eca5d43dfde30bb28aedc1f193744
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325575"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a>Utiliser le protocole MessagePack Hub dans SignalR pour ASP.NET Core

Par [Brennan Conroy](https://github.com/BrennanConroy)

Cet article suppose que le lecteur est familiarisé avec les sujets abordés dans la [prise en main](xref:tutorials/signalr).

## <a name="what-is-messagepack"></a>Qu’est MessagePack ?

[MessagePack](https://msgpack.org/index.html) est un format de sérialisation binaire qui est rapide et compact. Il est utile lorsque la bande passante et les performances sont un critère important, car il crée des messages plus petits comparé au [JSON](https://www.json.org/). S’agissant d’un format binaire, les messages sont illisibles lorsque vous examinez les journaux et traces réseau, sauf si les octets sont transmis via un analyseur MessagePack. SignalR prend en charge le format MessagePack et fournit des API  à utiliser par le client et le serveur.

## <a name="configure-messagepack-on-the-server"></a>Configurer MessagePack sur le serveur

Pour activer le protocole de Hub MessagePack sur le serveur, installez le package `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` dans votre application. Dans le fichier Startup.cs ajoutez `AddMessagePackProtocol` à l'appel à `AddSignalR` pour activer la prise en charge MessagePack sur le serveur.

> [!NOTE]
> JSON est activé par défaut. Ajouter de MessagePack permet la prise en charge pour les clients JSON et MessagePack.

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

Pour personnaliser la façon dont MessagePack met en forme vos données, `AddMessagePackProtocol` prend un délégué pour la configuration des options. Dans ce délégué, la propriété `FormatterResolvers` peut être utilisée pour configurer les options de sérialisation MessagePack. Pour plus d’informations sur la façon dont les programmes de résolution fonctionne, consultez la bibliothèque de MessagePack à [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp). Les attributs peuvent être utilisés sur les objets que vous souhaitez sérialiser pour définir comment ils doivent être traités.

```csharp
services.AddSignalR()
    .AddMessagePackProtocol(options =>
    {
        options.FormatterResolvers = new List<MessagePack.IFormatterResolver>()
        {
            MessagePack.Resolvers.StandardResolver.Instance
        };
    });
```

## <a name="configure-messagepack-on-the-client"></a>Configurer MessagePack sur le client

> [!NOTE]
> JSON est activé par défaut pour les clients pris en charge. Les clients peuvent prendre uniquement en charge un seul protocole. L'ajout de prise en charge MessagePack remplacera les protocoles précédemment configurés.

### <a name="net-client"></a>Client .NET

Pour activer MessagePack dans le Client .NET, installez le package `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` et appelez `AddMessagePackProtocol` sur `HubConnectionBuilder`.

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> Cet appel `AddMessagePackProtocol` prend un délégué pour la configuration des options comme le serveur.

### <a name="javascript-client"></a>Client JavaScript

La prise en charge de MessagePack pour le client Javascript est fournie par le package NPM `@aspnet/signalr-protocol-msgpack`.

```console
npm install @aspnet/signalr-protocol-msgpack
```

Après avoir installé le package npm, le module peut être utilisé directement par le biais d’un chargeur de module JavaScript ou importé dans le navigateur en référençant le fichier *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*. Dans un navigateur, la bibliothèque `msgpack5` doit également être référencée. Utilisez un `<script>` balise pour créer une référence. La bibliothèque, consultez *node_modules\msgpack5\dist\msgpack5.js*.

> [!NOTE]
> Lorsque vous utilisez l'élément `<script>`, l’ordre est important. Si *signalr-protocol-msgpack.js* est référencé avant *msgpack5.js*, une erreur se produit lorsque vous tentez de vous connecter avec MessagePack. *SignalR.js* est également requis avant *signalr-protocol-msgpack.js*.

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

L'jout de `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` à `HubConnectionBuilder` configurera le client pour utiliser le protocole MessagePack lors de la connexion à un serveur.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> À ce stade, il n’existe aucune option de configuration pour le protocole MessagePack sur le client JavaScript.

## <a name="related-resources"></a>Ressources connexes

* [Bien démarrer](xref:tutorials/signalr)
* [Client .NET](xref:signalr/dotnet-client)
* [Client JavaScript](xref:signalr/javascript-client)
