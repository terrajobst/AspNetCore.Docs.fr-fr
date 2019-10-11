---
title: Utiliser le protocole MessagePack Hub dans Signalr pour ASP.NET Core
author: bradygaster
description: Ajoutez le protocole MessagePack Hub à ASP.NET Core Signalr.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 10/08/2019
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: fe09b646eba5ae15cbd9e568b276aaf7763e4b1b
ms.sourcegitcommit: fcdf9aaa6c45c1a926bd870ed8f893bdb4935152
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72165403"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a>Utiliser le protocole MessagePack Hub dans Signalr pour ASP.NET Core

Par [Brennan Conroy](https://github.com/BrennanConroy)

Cet article suppose que le lecteur est familiarisé avec les sujets abordés dans la [prise en main](xref:tutorials/signalr).

## <a name="what-is-messagepack"></a>Qu’est-ce que MessagePack ?

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
> JSON est activé par défaut pour les clients pris en charge. Les clients ne peuvent prendre en charge qu’un seul protocole. L'ajout de prise en charge MessagePack remplacera les protocoles précédemment configurés.

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

::: moniker range=">= aspnetcore-3.0"

La prise en charge de MessagePack pour le client JavaScript est fournie par le package NPM `@microsoft/signalr-protocol-msgpack`. Installez le package en exécutant la commande suivante dans une interface de commande :

```console
npm install @microsoft/signalr-protocol-msgpack
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

La prise en charge de MessagePack pour le client JavaScript est fournie par le package NPM `@aspnet/signalr-protocol-msgpack`. Installez le package en exécutant la commande suivante dans une interface de commande :

```console
npm install @aspnet/signalr-protocol-msgpack
```

::: moniker-end

Après l’installation du package NPM, le module peut être utilisé directement via un chargeur de module JavaScript ou importé dans le navigateur en référençant le fichier suivant :

::: moniker range=">= aspnetcore-3.0"

*node_modules @ no__t-1 @ no__t-2* 

::: moniker-end

::: moniker range="< aspnetcore-3.0"

*node_modules @ no__t-1 @ no__t-2* 

::: moniker-end

Dans un navigateur, la bibliothèque `msgpack5` doit également être référencée. Utilisez un `<script>` balise pour créer une référence. La bibliothèque, consultez *node_modules\msgpack5\dist\msgpack5.js*.

> [!NOTE]
> Lorsque vous utilisez l'élément `<script>`, l’ordre est important. Si *signalr-Protocol-msgpack. js* est référencé avant *msgpack5. js*, une erreur se produit lors de la tentative de connexion à MessagePack. *SignalR.js* est également requis avant *signalr-protocol-msgpack.js*.

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

## <a name="messagepack-quirks"></a>MessagePack excentriques

Il existe quelques problèmes à connaître lors de l’utilisation du protocole MessagePack Hub.

### <a name="messagepack-is-case-sensitive"></a>MessagePack respecte la casse

Le protocole MessagePack respecte la casse. Par exemple, considérez la C# classe suivante :

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

Lors de l’envoi à partir du client JavaScript, vous devez utiliser des noms de propriété `PascalCased`, car C# la casse doit correspondre exactement à la classe. Exemple :

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

L’utilisation de noms `camelCased` ne se liera pas correctement à la C# classe. Vous pouvez contourner ce contournement en utilisant l’attribut `Key` pour spécifier un nom différent pour la propriété MessagePack. Pour plus d’informations, consultez [la documentation MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp#object-serialization).

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a>DateTime. Kind n’est pas conservé lors de la sérialisation/désérialisation

Le protocole MessagePack ne fournit pas de méthode pour encoder la valeur `Kind` d’un `DateTime`. Par conséquent, lors de la désérialisation d’une date, le protocole MessagePack Hub part du principe que la date d’entrée est au format UTC. Si vous utilisez des valeurs `DateTime` en heure locale, nous vous recommandons de procéder à une conversion en heure UTC avant de les envoyer. Convertissez-les de l’heure UTC en heure locale lorsque vous les recevez.

Pour plus d’informations sur cette limitation, consultez GitHub problème [ASPNET/signalr # 2632](https://github.com/aspnet/SignalR/issues/2632).

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a>DateTime. MinValue n’est pas pris en charge par MessagePack dans JavaScript

La bibliothèque [msgpack5](https://github.com/mcollina/msgpack5) utilisée par le client JavaScript signalr ne prend pas en charge le type `timestamp96` dans MessagePack. Ce type est utilisé pour encoder les valeurs de date très volumineuses (soit très tôt dans le passé, soit très loin dans le futur). La valeur de `DateTime.MinValue` est `January 1, 0001` qui doit être encodée dans une valeur `timestamp96`. Pour cette raison, l’envoi de `DateTime.MinValue` à un client JavaScript n’est pas pris en charge. Lorsque `DateTime.MinValue` est reçu par le client JavaScript, l’erreur suivante est levée :

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

En règle générale, `DateTime.MinValue` est utilisé pour encoder une valeur « manquante » ou `null`. Si vous devez encoder cette valeur dans MessagePack, utilisez une valeur Nullable `DateTime` (`DateTime?`) ou encodez une valeur `bool` distincte indiquant si la date est présente.

Pour plus d’informations sur cette limitation, consultez GitHub problème [ASPNET/signalr # 2228](https://github.com/aspnet/SignalR/issues/2228).

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a>Prise en charge de MessagePack dans l’environnement de compilation « à l’avance »

La bibliothèque [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp) utilisée par le client et le serveur .NET utilise la génération de code pour optimiser la sérialisation. Par conséquent, il n’est pas pris en charge par défaut dans les environnements qui utilisent la compilation « à l’avance » (par exemple, Xamarin iOS ou Unity). Il est possible d’utiliser MessagePack dans ces environnements en prégénérant le code de sérialiseur/désérialiseur. Pour plus d’informations, consultez [la documentation MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp#pre-code-generationunityxamarin-supports). Une fois que vous avez généré le prétraitement des sérialiseurs, vous pouvez les inscrire à l’aide du délégué de configuration passé à `AddMessagePackProtocol` :

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

### <a name="type-checks-are-more-strict-in-messagepack"></a>Les vérifications de type sont plus strictes dans MessagePack

Le protocole JSON Hub effectue des conversions de type pendant la désérialisation. Par exemple, si l’objet entrant a une valeur de propriété qui est un nombre (`{ foo: 42 }`), mais que la propriété de la classe .NET est de type `string`, la valeur est convertie. Toutefois, MessagePack n’effectue pas cette conversion et lèvera une exception qui est visible dans les journaux côté serveur (et dans la console) :

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

Pour plus d’informations sur cette limitation, consultez GitHub problème [ASPNET/signalr # 2937](https://github.com/aspnet/SignalR/issues/2937).

## <a name="related-resources"></a>Ressources connexes

* [Bien démarrer](xref:tutorials/signalr)
* [Client .NET](xref:signalr/dotnet-client)
* [Client JavaScript](xref:signalr/javascript-client)
