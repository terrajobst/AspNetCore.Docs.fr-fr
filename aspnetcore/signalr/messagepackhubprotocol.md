---
title: Utiliser le protocole MessagePack Hub dans SignalR pour ASP.NET Core
author: bradygaster
description: Ajouter le protocole MessagePack Hub à ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/27/2019
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 7742f6f8bb53fb3c299ff98ae52a0da519ff396c
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896516"
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

MessagePack pour le client JavaScript est prise en charge par le `@aspnet/signalr-protocol-msgpack` package npm.

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

## <a name="messagepack-quirks"></a>MessagePack quirks

Il existe quelques problèmes à connaître lorsque vous utilisez le protocole de Hub MessagePack.

### <a name="messagepack-is-case-sensitive"></a>MessagePack respecte la casse

Le protocole MessagePack respecte la casse. Par exemple, considérez les éléments suivants C# classe :

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

Lors de l’envoi à partir du client JavaScript, vous devez utiliser `PascalCased` des noms de propriété, étant donné que la casse doit correspondre à la C# classe exactement. Exemple :

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

À l’aide de `camelCased` noms ne se lier correctement à la C# classe. Vous pouvez contourner ce à l’aide de la `Key` attribut pour spécifier un autre nom pour la propriété MessagePack. Pour plus d’informations, consultez [la documentation MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp#object-serialization).

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a>DateTime.Kind n’est pas conservé lors de la sérialisation/désérialisation

Le protocole MessagePack ne fournit pas un moyen pour encoder le `Kind` valeur d’un `DateTime`. Par conséquent, lors de la désérialisation d’une date, le protocole de Hub MessagePack part du principe que la date entrante est au format UTC. Si vous travaillez avec `DateTime` valeurs en heure locale, nous vous recommandons de conversion en heure UTC avant de les envoyer. Convertir les à l’heure UTC en heure locale lorsque vous les recevez.

Pour plus d’informations sur cette limitation, consultez le GitHub problème [aspnet/SignalR #2632](https://github.com/aspnet/SignalR/issues/2632).

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a>DateTime.MinValue n’est pas pris en charge par MessagePack dans JavaScript

Le [msgpack5](https://github.com/mcollina/msgpack5) bibliothèque utilisée par le client SignalR JavaScript ne prend pas en charge la `timestamp96` type dans MessagePack. Ce type est utilisé pour encoder les valeurs de date très volumineux (soit très tôt dans le passé, soit très éloignée dans le futur). La valeur de `DateTime.MinValue` est `January 1, 0001` qui doit être codé dans un `timestamp96` valeur. En raison de cette option, envoi `DateTime.MinValue` à un code JavaScript client n’est pas pris en charge. Lorsque `DateTime.MinValue` est reçu par le client JavaScript, l’erreur suivante est levée :

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

En règle générale, `DateTime.MinValue` est utilisé pour encoder un « manquant » ou `null` valeur. Si vous devez coder cette valeur dans MessagePack, utilisez nullable `DateTime` valeur (`DateTime?`) ou encoder un distinct `bool` valeur indiquant si la date est présente.

Pour plus d’informations sur cette limitation, consultez le GitHub problème [aspnet/SignalR #2228](https://github.com/aspnet/SignalR/issues/2228).

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a>Prise en charge de MessagePack dans l’environnement de compilation de « ahead-of-time »

Le [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp) bibliothèque utilisée par le client .NET et le serveur utilise la génération de code pour optimiser la sérialisation. Par conséquent, il n’est pas pris en charge par défaut sur les environnements qui utilisent la compilation « ahead-of-time » (par exemple, Xamarin iOS ou Unity). Il est possible d’utiliser MessagePack dans ces environnements par « prégénération de « le code de sérialiseur/désérialiseur. Pour plus d’informations, consultez [la documentation MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp#pre-code-generationunityxamarin-supports). Une fois que vous avez préalablement généré les sérialiseurs, vous pouvez les enregistrer à l’aide du délégué de configuration transmis à `AddMessagePackProtocol`:

```csharp
services.AddSignalR()
    .AddMessagePackProtocol(options =>
    {
        options.FormatterResolvers = new List<MessagePack.IFormatterResolver>()
        {
            MessagePack.Resolvers.GeneratedResolver.Instance,
            MessagePack.Resolvers.StandardResolver.Instance
        };
    });
```

### <a name="type-checks-are-more-strict-in-messagepack"></a>Vérifications de type sont plus strictes dans MessagePack

Le protocole de Hub JSON effectue les conversions de type pendant la désérialisation. Par exemple, si l’objet entrant a une valeur de propriété qui est un nombre (`{ foo: 42 }`), mais la propriété sur la classe .NET est de type `string`, la valeur sera convertie. Toutefois, MessagePack n’effectue pas cette conversion et lève une exception qui peut être consultée dans les journaux côté serveur (et dans la console) :

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

Pour plus d’informations sur cette limitation, consultez le GitHub problème [aspnet/SignalR #2937](https://github.com/aspnet/SignalR/issues/2937).

## <a name="related-resources"></a>Ressources connexes

* [Bien démarrer](xref:tutorials/signalr)
* [Client .NET](xref:signalr/dotnet-client)
* [Client JavaScript](xref:signalr/javascript-client)
