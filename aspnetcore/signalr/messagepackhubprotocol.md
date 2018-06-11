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
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a><span data-ttu-id="1bc6f-103">Utilisez le protocole de MessagePack Hub dans SignalR pour ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1bc6f-103">Use MessagePack Hub Protocol in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="1bc6f-104">Par [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="1bc6f-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="1bc6f-105">Cet article suppose que le lecteur est familiarisé avec les rubriques de [prise en main](xref:signalr/get-started).</span><span class="sxs-lookup"><span data-stu-id="1bc6f-105">This article assumes the reader is familiar with the topics covered in [Get Started](xref:signalr/get-started).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="1bc6f-106">Qu’est MessagePack ?</span><span class="sxs-lookup"><span data-stu-id="1bc6f-106">What is MessagePack?</span></span>

<span data-ttu-id="1bc6f-107">[MessagePack](https://msgpack.org/index.html) est un format de sérialisation binaire est rapide et compact.</span><span class="sxs-lookup"><span data-stu-id="1bc6f-107">[MessagePack](https://msgpack.org/index.html) is a binary serialization format that is fast and compact.</span></span> <span data-ttu-id="1bc6f-108">Il est utile lorsque les performances et la bande passante sont un critère important, car elle crée des messages plus petits par rapport à [JSON](https://www.json.org/).</span><span class="sxs-lookup"><span data-stu-id="1bc6f-108">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="1bc6f-109">S’agissant d’un format binaire, les messages sont illisibles en examinant les journaux et traces réseau, sauf si les octets sont transmises via un analyseur MessagePack.</span><span class="sxs-lookup"><span data-stu-id="1bc6f-109">Because it's a binary format, messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> <span data-ttu-id="1bc6f-110">SignalR prend en charge le format MessagePack et fournit des API pour le client et le serveur à utiliser.</span><span class="sxs-lookup"><span data-stu-id="1bc6f-110">SignalR has built-in support for the MessagePack format, and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="1bc6f-111">Configurer MessagePack sur le serveur</span><span class="sxs-lookup"><span data-stu-id="1bc6f-111">Configure MessagePack on the server</span></span>

<span data-ttu-id="1bc6f-112">Pour activer le protocole de concentrateur MessagePack sur le serveur, installez le `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package dans votre application.</span><span class="sxs-lookup"><span data-stu-id="1bc6f-112">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="1bc6f-113">Dans le fichier Startup.cs ajoutez `AddMessagePackProtocol` à la `AddSignalR` appel pour activer la prise en charge MessagePack sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="1bc6f-113">In the Startup.cs file add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="1bc6f-114">JSON est activé par défaut.</span><span class="sxs-lookup"><span data-stu-id="1bc6f-114">JSON is enabled by default.</span></span> <span data-ttu-id="1bc6f-115">Ajout de MessagePack permet la prise en charge pour les clients JSON et MessagePack.</span><span class="sxs-lookup"><span data-stu-id="1bc6f-115">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="1bc6f-116">Pour personnaliser la MessagePack met en forme vos données, `AddMessagePackProtocol` prend un délégué pour configurer les options.</span><span class="sxs-lookup"><span data-stu-id="1bc6f-116">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="1bc6f-117">Dans ce délégué, la `FormatterResolvers` propriété peut être utilisée pour configurer les options de sérialisation MessagePack.</span><span class="sxs-lookup"><span data-stu-id="1bc6f-117">In that delegate, the `FormatterResolvers` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="1bc6f-118">Pour plus d’informations sur la façon dont les programmes de résolution, visitez la bibliothèque MessagePack à [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span><span class="sxs-lookup"><span data-stu-id="1bc6f-118">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="1bc6f-119">Attributs peuvent être utilisés sur les objets que vous souhaitez sérialiser pour définir la façon dont ils doivent être traités.</span><span class="sxs-lookup"><span data-stu-id="1bc6f-119">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

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

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="1bc6f-120">Configurer MessagePack sur le client</span><span class="sxs-lookup"><span data-stu-id="1bc6f-120">Configure MessagePack on the client</span></span>

### <a name="net-client"></a><span data-ttu-id="1bc6f-121">Client .NET</span><span class="sxs-lookup"><span data-stu-id="1bc6f-121">.NET client</span></span>

<span data-ttu-id="1bc6f-122">Pour activer MessagePack dans le Client .NET, installez le `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package et appelez `AddMessagePackProtocol` sur `HubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1bc6f-122">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="1bc6f-123">Cela `AddMessagePackProtocol` appel prend un délégué pour la configuration des options comme le serveur.</span><span class="sxs-lookup"><span data-stu-id="1bc6f-123">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="1bc6f-124">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="1bc6f-124">JavaScript client</span></span>

<span data-ttu-id="1bc6f-125">MessagePack pour le client Javascript est prise en charge par le `@aspnet/signalr-protocol-msgpack` package NPM.</span><span class="sxs-lookup"><span data-stu-id="1bc6f-125">MessagePack support for the Javascript client is provided by the `@aspnet/signalr-protocol-msgpack` NPM package.</span></span>

```console
npm install @aspnet/signalr-protocol-msgpack
```

<span data-ttu-id="1bc6f-126">Après avoir installé le package npm, le module peut être utilisé directement par le biais d’un chargeur de module JavaScript ou importé dans le navigateur en référençant la *node_modules\\ @aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*  fichier.</span><span class="sxs-lookup"><span data-stu-id="1bc6f-126">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* file.</span></span> <span data-ttu-id="1bc6f-127">Dans un navigateur le `msgpack5` bibliothèque doit également être référencée.</span><span class="sxs-lookup"><span data-stu-id="1bc6f-127">In a browser the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="1bc6f-128">Utilisez un `<script>` balise pour créer une référence.</span><span class="sxs-lookup"><span data-stu-id="1bc6f-128">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="1bc6f-129">La bibliothèque, consultez *node_modules\msgpack5\dist\msgpack5.js*.</span><span class="sxs-lookup"><span data-stu-id="1bc6f-129">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="1bc6f-130">Lorsque vous utilisez la `<script>` élément, l’ordre est important.</span><span class="sxs-lookup"><span data-stu-id="1bc6f-130">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="1bc6f-131">Si *signalr-protocole-msgpack.js* est référencé avant *msgpack5.js*, une erreur se produit lorsque vous tentez de vous connecter avec MessagePack.</span><span class="sxs-lookup"><span data-stu-id="1bc6f-131">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="1bc6f-132">*SignalR.js* est également requis avant *signalr-protocole-msgpack.js*.</span><span class="sxs-lookup"><span data-stu-id="1bc6f-132">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="1bc6f-133">Ajout de `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` pour le `HubConnectionBuilder` configure le client pour utiliser le protocole MessagePack lors de la connexion à un serveur.</span><span class="sxs-lookup"><span data-stu-id="1bc6f-133">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="1bc6f-134">À ce stade, il n’existe aucune option de configuration pour le protocole MessagePack sur le client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1bc6f-134">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="related-resources"></a><span data-ttu-id="1bc6f-135">Ressources connexes</span><span class="sxs-lookup"><span data-stu-id="1bc6f-135">Related resources</span></span>

* [<span data-ttu-id="1bc6f-136">Bien démarrer</span><span class="sxs-lookup"><span data-stu-id="1bc6f-136">Get Started</span></span>](xref:signalr/get-started)
* [<span data-ttu-id="1bc6f-137">Client .NET</span><span class="sxs-lookup"><span data-stu-id="1bc6f-137">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="1bc6f-138">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="1bc6f-138">JavaScript client</span></span>](xref:signalr/javascript-client)
