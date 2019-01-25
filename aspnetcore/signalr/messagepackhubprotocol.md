---
title: Utiliser le protocole MessagePack Hub dans SignalR pour ASP.NET Core
author: bradygaster
description: Ajouter le protocole MessagePack Hub à ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 2852ca93c62e706e9a5203625822c2fb954fd2b8
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54835607"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a><span data-ttu-id="4598b-103">Utiliser le protocole MessagePack Hub dans SignalR pour ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4598b-103">Use MessagePack Hub Protocol in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="4598b-104">Par [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="4598b-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="4598b-105">Cet article suppose que le lecteur est familiarisé avec les sujets abordés dans la [prise en main](xref:tutorials/signalr).</span><span class="sxs-lookup"><span data-stu-id="4598b-105">This article assumes the reader is familiar with the topics covered in [Get Started](xref:tutorials/signalr).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="4598b-106">Qu’est MessagePack ?</span><span class="sxs-lookup"><span data-stu-id="4598b-106">What is MessagePack?</span></span>

<span data-ttu-id="4598b-107">[MessagePack](https://msgpack.org/index.html) est un format de sérialisation binaire qui est rapide et compact.</span><span class="sxs-lookup"><span data-stu-id="4598b-107">[MessagePack](https://msgpack.org/index.html) is a binary serialization format that is fast and compact.</span></span> <span data-ttu-id="4598b-108">Il est utile lorsque la bande passante et les performances sont un critère important, car il crée des messages plus petits comparé au [JSON](https://www.json.org/).</span><span class="sxs-lookup"><span data-stu-id="4598b-108">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="4598b-109">S’agissant d’un format binaire, les messages sont illisibles lorsque vous examinez les journaux et traces réseau, sauf si les octets sont transmis via un analyseur MessagePack.</span><span class="sxs-lookup"><span data-stu-id="4598b-109">Because it's a binary format, messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> <span data-ttu-id="4598b-110">SignalR prend en charge le format MessagePack et fournit des API  à utiliser par le client et le serveur.</span><span class="sxs-lookup"><span data-stu-id="4598b-110">SignalR has built-in support for the MessagePack format, and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="4598b-111">Configurer MessagePack sur le serveur</span><span class="sxs-lookup"><span data-stu-id="4598b-111">Configure MessagePack on the server</span></span>

<span data-ttu-id="4598b-112">Pour activer le protocole de Hub MessagePack sur le serveur, installez le package `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` dans votre application.</span><span class="sxs-lookup"><span data-stu-id="4598b-112">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="4598b-113">Dans le fichier Startup.cs ajoutez `AddMessagePackProtocol` à l'appel à `AddSignalR` pour activer la prise en charge MessagePack sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="4598b-113">In the Startup.cs file add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="4598b-114">JSON est activé par défaut.</span><span class="sxs-lookup"><span data-stu-id="4598b-114">JSON is enabled by default.</span></span> <span data-ttu-id="4598b-115">Ajouter de MessagePack permet la prise en charge pour les clients JSON et MessagePack.</span><span class="sxs-lookup"><span data-stu-id="4598b-115">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="4598b-116">Pour personnaliser la façon dont MessagePack met en forme vos données, `AddMessagePackProtocol` prend un délégué pour la configuration des options.</span><span class="sxs-lookup"><span data-stu-id="4598b-116">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="4598b-117">Dans ce délégué, la propriété `FormatterResolvers` peut être utilisée pour configurer les options de sérialisation MessagePack.</span><span class="sxs-lookup"><span data-stu-id="4598b-117">In that delegate, the `FormatterResolvers` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="4598b-118">Pour plus d’informations sur la façon dont les programmes de résolution fonctionne, consultez la bibliothèque de MessagePack à [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span><span class="sxs-lookup"><span data-stu-id="4598b-118">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="4598b-119">Les attributs peuvent être utilisés sur les objets que vous souhaitez sérialiser pour définir comment ils doivent être traités.</span><span class="sxs-lookup"><span data-stu-id="4598b-119">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

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

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="4598b-120">Configurer MessagePack sur le client</span><span class="sxs-lookup"><span data-stu-id="4598b-120">Configure MessagePack on the client</span></span>

> [!NOTE]
> <span data-ttu-id="4598b-121">JSON est activé par défaut pour les clients pris en charge.</span><span class="sxs-lookup"><span data-stu-id="4598b-121">JSON is enabled by default for the supported clients.</span></span> <span data-ttu-id="4598b-122">Les clients peuvent prendre uniquement en charge un seul protocole.</span><span class="sxs-lookup"><span data-stu-id="4598b-122">Clients can only support a single protocol.</span></span> <span data-ttu-id="4598b-123">L'ajout de prise en charge MessagePack remplacera les protocoles précédemment configurés.</span><span class="sxs-lookup"><span data-stu-id="4598b-123">Adding MessagePack support will replace any previously configured protocols.</span></span>

### <a name="net-client"></a><span data-ttu-id="4598b-124">Client .NET</span><span class="sxs-lookup"><span data-stu-id="4598b-124">.NET client</span></span>

<span data-ttu-id="4598b-125">Pour activer MessagePack dans le Client .NET, installez le package `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` et appelez `AddMessagePackProtocol` sur `HubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="4598b-125">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="4598b-126">Cet appel `AddMessagePackProtocol` prend un délégué pour la configuration des options comme le serveur.</span><span class="sxs-lookup"><span data-stu-id="4598b-126">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="4598b-127">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="4598b-127">JavaScript client</span></span>

<span data-ttu-id="4598b-128">La prise en charge de MessagePack pour le client Javascript est fournie par le package NPM `@aspnet/signalr-protocol-msgpack`.</span><span class="sxs-lookup"><span data-stu-id="4598b-128">MessagePack support for the Javascript client is provided by the `@aspnet/signalr-protocol-msgpack` NPM package.</span></span>

```console
npm install @aspnet/signalr-protocol-msgpack
```

<span data-ttu-id="4598b-129">Après avoir installé le package npm, le module peut être utilisé directement par le biais d’un chargeur de module JavaScript ou importé dans le navigateur en référençant le fichier *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*.</span><span class="sxs-lookup"><span data-stu-id="4598b-129">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* file.</span></span> <span data-ttu-id="4598b-130">Dans un navigateur, la bibliothèque `msgpack5` doit également être référencée.</span><span class="sxs-lookup"><span data-stu-id="4598b-130">In a browser, the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="4598b-131">Utilisez un `<script>` balise pour créer une référence.</span><span class="sxs-lookup"><span data-stu-id="4598b-131">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="4598b-132">La bibliothèque, consultez *node_modules\msgpack5\dist\msgpack5.js*.</span><span class="sxs-lookup"><span data-stu-id="4598b-132">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="4598b-133">Lorsque vous utilisez l'élément `<script>`, l’ordre est important.</span><span class="sxs-lookup"><span data-stu-id="4598b-133">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="4598b-134">Si *signalr-protocol-msgpack.js* est référencé avant *msgpack5.js*, une erreur se produit lorsque vous tentez de vous connecter avec MessagePack.</span><span class="sxs-lookup"><span data-stu-id="4598b-134">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="4598b-135">*SignalR.js* est également requis avant *signalr-protocol-msgpack.js*.</span><span class="sxs-lookup"><span data-stu-id="4598b-135">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="4598b-136">L'jout de `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` à `HubConnectionBuilder` configurera le client pour utiliser le protocole MessagePack lors de la connexion à un serveur.</span><span class="sxs-lookup"><span data-stu-id="4598b-136">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="4598b-137">À ce stade, il n’existe aucune option de configuration pour le protocole MessagePack sur le client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4598b-137">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="related-resources"></a><span data-ttu-id="4598b-138">Ressources connexes</span><span class="sxs-lookup"><span data-stu-id="4598b-138">Related resources</span></span>

* [<span data-ttu-id="4598b-139">Bien démarrer</span><span class="sxs-lookup"><span data-stu-id="4598b-139">Get Started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="4598b-140">Client .NET</span><span class="sxs-lookup"><span data-stu-id="4598b-140">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="4598b-141">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="4598b-141">JavaScript client</span></span>](xref:signalr/javascript-client)
