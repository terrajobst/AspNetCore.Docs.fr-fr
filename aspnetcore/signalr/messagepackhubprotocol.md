---
title: Utilisez le protocole de MessagePack Hub dans SignalR pour ASP.NET Core
author: rachelappel
description: Ajouter MessagePack Hub protocole à ASP.NET Core SignalR.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: b6c33c4da47a19d67bffbaf84f54d59013edadbe
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252477"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a>Utilisez le protocole de MessagePack Hub dans SignalR pour ASP.NET Core

Par [Brennan Conroy](https://github.com/BrennanConroy)

Cet article suppose que le lecteur est familiarisé avec les rubriques de [prise en main](xref:signalr/get-started).

## <a name="what-is-messagepack"></a>Qu’est MessagePack ?

[MessagePack](https://msgpack.org/index.html) est un format de sérialisation binaire est rapide et compact. Il est utile lorsque les performances et la bande passante sont un critère important, car elle crée des messages plus petits par rapport à [JSON](https://www.json.org/). S’agissant d’un format binaire, les messages sont illisibles en examinant les journaux et traces réseau, sauf si les octets sont transmises via un analyseur MessagePack. SignalR prend en charge le format MessagePack et fournit des API pour le client et le serveur à utiliser.

## <a name="configure-messagepack-on-the-server"></a>Configurer MessagePack sur le serveur

Pour activer le protocole de concentrateur MessagePack sur le serveur, installez le `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package dans votre application. Dans le fichier Startup.cs ajoutez `AddMessagePackProtocol` à la `AddSignalR` appel pour activer la prise en charge MessagePack sur le serveur.

> [!NOTE]
> JSON est activé par défaut. Ajout de MessagePack permet la prise en charge pour les clients JSON et MessagePack.

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

Pour personnaliser la MessagePack met en forme vos données, `AddMessagePackProtocol` prend un délégué pour configurer les options. Dans ce délégué, la `FormatterResolvers` propriété peut être utilisée pour configurer les options de sérialisation MessagePack. Pour plus d’informations sur la façon dont les programmes de résolution, visitez la bibliothèque MessagePack à [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp). Attributs peuvent être utilisés sur les objets que vous souhaitez sérialiser pour définir la façon dont ils doivent être traités.

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

### <a name="net-client"></a>Client .NET

Pour activer MessagePack dans le Client .NET, installez le `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package et appelez `AddMessagePackProtocol` sur `HubConnectionBuilder`.

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> Cela `AddMessagePackProtocol` appel prend un délégué pour la configuration des options comme le serveur.

### <a name="javascript-client"></a>Client JavaScript

MessagePack pour le client Javascript est prise en charge par le `@aspnet/signalr-protocol-msgpack` package NPM.

```console
npm install @aspnet/signalr-protocol-msgpack
```

Après avoir installé le package npm, le module peut être utilisé directement par le biais d’un chargeur de module JavaScript ou importé dans le navigateur en référençant la *node_modules\\ @aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*  fichier. Dans un navigateur le `msgpack5` bibliothèque doit également être référencée. Utilisez un `<script>` balise pour créer une référence. La bibliothèque, consultez *node_modules\msgpack5\dist\msgpack5.js*.

> [!NOTE]
> Lorsque vous utilisez la `<script>` élément, l’ordre est important. Si *signalr-protocole-msgpack.js* est référencé avant *msgpack5.js*, une erreur se produit lorsque vous tentez de vous connecter avec MessagePack. *SignalR.js* est également requis avant *signalr-protocole-msgpack.js*.

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

Ajout de `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` pour le `HubConnectionBuilder` configure le client pour utiliser le protocole MessagePack lors de la connexion à un serveur.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> À ce stade, il n’existe aucune option de configuration pour le protocole MessagePack sur le client JavaScript.

## <a name="related-resources"></a>Ressources connexes

* [Bien démarrer](xref:signalr/get-started)
* [Client .NET](xref:signalr/dotnet-client)
* [Client JavaScript](xref:signalr/javascript-client)
