---
title: Utiliser le protocole MessagePack Hub dans SignalR pour ASP.NET Core
author: bradygaster
description: Ajoutez le protocole MessagePack Hub à ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: cd052a97db1e20d6c7aa00f47cf6a7d01a9bc305
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963766"
---
# <a name="use-messagepack-hub-protocol-in-opno-locsignalr-for-aspnet-core"></a><span data-ttu-id="8d79e-103">Utiliser le protocole MessagePack Hub dans SignalR pour ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8d79e-103">Use MessagePack Hub Protocol in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="8d79e-104">Par [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="8d79e-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="8d79e-105">Cet article suppose que le lecteur est familiarisé avec les sujets abordés dans la rubrique [prise en main](xref:tutorials/signalr).</span><span class="sxs-lookup"><span data-stu-id="8d79e-105">This article assumes the reader is familiar with the topics covered in [Get Started](xref:tutorials/signalr).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="8d79e-106">Qu’est-ce que MessagePack ?</span><span class="sxs-lookup"><span data-stu-id="8d79e-106">What is MessagePack?</span></span>

<span data-ttu-id="8d79e-107">[MessagePack](https://msgpack.org/index.html) est un format de sérialisation binaire rapide et compact.</span><span class="sxs-lookup"><span data-stu-id="8d79e-107">[MessagePack](https://msgpack.org/index.html) is a binary serialization format that is fast and compact.</span></span> <span data-ttu-id="8d79e-108">Elle est utile lorsque les performances et la bande passante sont un problème, car elle crée des messages plus petits par rapport à [JSON](https://www.json.org/).</span><span class="sxs-lookup"><span data-stu-id="8d79e-108">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="8d79e-109">Étant donné qu’il s’agit d’un format binaire, les messages ne sont pas lisibles lors de la consultation des journaux et des suivis réseau, sauf si les octets sont transmis via un analyseur MessagePack.</span><span class="sxs-lookup"><span data-stu-id="8d79e-109">Because it's a binary format, messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> SignalR<span data-ttu-id="8d79e-110"> offre une prise en charge intégrée du format MessagePack et fournit des API que le client et le serveur doivent utiliser.</span><span class="sxs-lookup"><span data-stu-id="8d79e-110"> has built-in support for the MessagePack format, and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="8d79e-111">Configurer MessagePack sur le serveur</span><span class="sxs-lookup"><span data-stu-id="8d79e-111">Configure MessagePack on the server</span></span>

<span data-ttu-id="8d79e-112">Pour activer le protocole MessagePack Hub sur le serveur, installez le package `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` dans votre application.</span><span class="sxs-lookup"><span data-stu-id="8d79e-112">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="8d79e-113">Dans le fichier Startup.cs, ajoutez `AddMessagePackProtocol` à l’appel `AddSignalR` pour activer la prise en charge de MessagePack sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="8d79e-113">In the Startup.cs file add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="8d79e-114">JSON est activé par défaut.</span><span class="sxs-lookup"><span data-stu-id="8d79e-114">JSON is enabled by default.</span></span> <span data-ttu-id="8d79e-115">L’ajout de MessagePack permet la prise en charge des clients JSON et MessagePack.</span><span class="sxs-lookup"><span data-stu-id="8d79e-115">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="8d79e-116">Pour personnaliser la façon dont MessagePack met en forme vos données, `AddMessagePackProtocol` prend un délégué pour configurer les options.</span><span class="sxs-lookup"><span data-stu-id="8d79e-116">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="8d79e-117">Dans ce délégué, la propriété `FormatterResolvers` peut être utilisée pour configurer les options de sérialisation MessagePack.</span><span class="sxs-lookup"><span data-stu-id="8d79e-117">In that delegate, the `FormatterResolvers` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="8d79e-118">Pour plus d’informations sur le fonctionnement des programmes de résolution, consultez la bibliothèque MessagePack sur [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span><span class="sxs-lookup"><span data-stu-id="8d79e-118">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="8d79e-119">Les attributs peuvent être utilisés sur les objets que vous souhaitez sérialiser pour définir la manière dont ils doivent être gérés.</span><span class="sxs-lookup"><span data-stu-id="8d79e-119">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

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

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="8d79e-120">Configurer MessagePack sur le client</span><span class="sxs-lookup"><span data-stu-id="8d79e-120">Configure MessagePack on the client</span></span>

> [!NOTE]
> <span data-ttu-id="8d79e-121">JSON est activé par défaut pour les clients pris en charge.</span><span class="sxs-lookup"><span data-stu-id="8d79e-121">JSON is enabled by default for the supported clients.</span></span> <span data-ttu-id="8d79e-122">Les clients ne peuvent prendre en charge qu’un seul protocole.</span><span class="sxs-lookup"><span data-stu-id="8d79e-122">Clients can only support a single protocol.</span></span> <span data-ttu-id="8d79e-123">L’ajout de la prise en charge de MessagePack remplacera tous les protocoles précédemment configurés.</span><span class="sxs-lookup"><span data-stu-id="8d79e-123">Adding MessagePack support will replace any previously configured protocols.</span></span>

### <a name="net-client"></a><span data-ttu-id="8d79e-124">Client .NET</span><span class="sxs-lookup"><span data-stu-id="8d79e-124">.NET client</span></span>

<span data-ttu-id="8d79e-125">Pour activer MessagePack dans le client .NET, installez le package `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` et appelez `AddMessagePackProtocol` sur `HubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="8d79e-125">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="8d79e-126">Cet appel de `AddMessagePackProtocol` prend un délégué pour configurer les options de la même façon que le serveur.</span><span class="sxs-lookup"><span data-stu-id="8d79e-126">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="8d79e-127">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="8d79e-127">JavaScript client</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="8d79e-128">La prise en charge de MessagePack pour le client JavaScript est fournie par le package NPM `@microsoft/signalr-protocol-msgpack`.</span><span class="sxs-lookup"><span data-stu-id="8d79e-128">MessagePack support for the JavaScript client is provided by the `@microsoft/signalr-protocol-msgpack` npm package.</span></span> <span data-ttu-id="8d79e-129">Installez le package en exécutant la commande suivante dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="8d79e-129">Install the package by executing the following command in a command shell:</span></span>

```console
npm install @microsoft/signalr-protocol-msgpack
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="8d79e-130">La prise en charge de MessagePack pour le client JavaScript est fournie par le package NPM `@aspnet/signalr-protocol-msgpack`.</span><span class="sxs-lookup"><span data-stu-id="8d79e-130">MessagePack support for the JavaScript client is provided by the `@aspnet/signalr-protocol-msgpack` npm package.</span></span> <span data-ttu-id="8d79e-131">Installez le package en exécutant la commande suivante dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="8d79e-131">Install the package by executing the following command in a command shell:</span></span>

```console
npm install @aspnet/signalr-protocol-msgpack
```

::: moniker-end

<span data-ttu-id="8d79e-132">Après l’installation du package NPM, le module peut être utilisé directement via un chargeur de module JavaScript ou importé dans le navigateur en référençant le fichier suivant :</span><span class="sxs-lookup"><span data-stu-id="8d79e-132">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the following file:</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="8d79e-133">*node_modules\\@microsoft\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span><span class="sxs-lookup"><span data-stu-id="8d79e-133">*node_modules\\@microsoft\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span></span> 

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="8d79e-134">*node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span><span class="sxs-lookup"><span data-stu-id="8d79e-134">*node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span></span> 

::: moniker-end

<span data-ttu-id="8d79e-135">Dans un navigateur, la bibliothèque de `msgpack5` doit également être référencée.</span><span class="sxs-lookup"><span data-stu-id="8d79e-135">In a browser, the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="8d79e-136">Utilisez une balise `<script>` pour créer une référence.</span><span class="sxs-lookup"><span data-stu-id="8d79e-136">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="8d79e-137">La bibliothèque se trouve sur *node_modules \msgpack5\dist\msgpack5.js*.</span><span class="sxs-lookup"><span data-stu-id="8d79e-137">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="8d79e-138">Lorsque vous utilisez l’élément `<script>`, l’ordre est important.</span><span class="sxs-lookup"><span data-stu-id="8d79e-138">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="8d79e-139">Si *signalr-Protocol-msgpack. js* est référencé avant *msgpack5. js*, une erreur se produit lors de la tentative de connexion à MessagePack.</span><span class="sxs-lookup"><span data-stu-id="8d79e-139">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="8d79e-140">*signalr. js* est également requis avant *signalr-Protocol-msgpack. js*.</span><span class="sxs-lookup"><span data-stu-id="8d79e-140">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="8d79e-141">L’ajout de `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` à la `HubConnectionBuilder` configure le client pour qu’il utilise le protocole MessagePack lors de la connexion à un serveur.</span><span class="sxs-lookup"><span data-stu-id="8d79e-141">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="8d79e-142">À ce stade, il n’existe aucune option de configuration pour le protocole MessagePack sur le client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8d79e-142">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="messagepack-quirks"></a><span data-ttu-id="8d79e-143">MessagePack excentriques</span><span class="sxs-lookup"><span data-stu-id="8d79e-143">MessagePack quirks</span></span>

<span data-ttu-id="8d79e-144">Il existe quelques problèmes à connaître lors de l’utilisation du protocole MessagePack Hub.</span><span class="sxs-lookup"><span data-stu-id="8d79e-144">There are a few issues to be aware of when using the MessagePack Hub Protocol.</span></span>

### <a name="messagepack-is-case-sensitive"></a><span data-ttu-id="8d79e-145">MessagePack respecte la casse</span><span class="sxs-lookup"><span data-stu-id="8d79e-145">MessagePack is case-sensitive</span></span>

<span data-ttu-id="8d79e-146">Le protocole MessagePack respecte la casse.</span><span class="sxs-lookup"><span data-stu-id="8d79e-146">The MessagePack protocol is case-sensitive.</span></span> <span data-ttu-id="8d79e-147">Par exemple, considérez la C# classe suivante :</span><span class="sxs-lookup"><span data-stu-id="8d79e-147">For example, consider the following C# class:</span></span>

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

<span data-ttu-id="8d79e-148">Lors de l’envoi à partir du client JavaScript, vous devez utiliser `PascalCased` noms de propriété, car la C# casse doit correspondre exactement à la classe.</span><span class="sxs-lookup"><span data-stu-id="8d79e-148">When sending from the JavaScript client, you must use `PascalCased` property names, since the casing must match the C# class exactly.</span></span> <span data-ttu-id="8d79e-149">Exemple :</span><span class="sxs-lookup"><span data-stu-id="8d79e-149">For example:</span></span>

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

<span data-ttu-id="8d79e-150">L’utilisation de noms de `camelCased` n’est C# pas correctement liée à la classe.</span><span class="sxs-lookup"><span data-stu-id="8d79e-150">Using `camelCased` names won't properly bind to the C# class.</span></span> <span data-ttu-id="8d79e-151">Vous pouvez contourner ce cas à l’aide de l’attribut `Key` pour spécifier un nom différent pour la propriété MessagePack.</span><span class="sxs-lookup"><span data-stu-id="8d79e-151">You can work around this by using the `Key` attribute to specify a different name for the MessagePack property.</span></span> <span data-ttu-id="8d79e-152">Pour plus d’informations, consultez [la documentation MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp#object-serialization).</span><span class="sxs-lookup"><span data-stu-id="8d79e-152">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp#object-serialization).</span></span>

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a><span data-ttu-id="8d79e-153">DateTime. Kind n’est pas conservé lors de la sérialisation/désérialisation</span><span class="sxs-lookup"><span data-stu-id="8d79e-153">DateTime.Kind is not preserved when serializing/deserializing</span></span>

<span data-ttu-id="8d79e-154">Le protocole MessagePack ne permet pas d’encoder la valeur `Kind` d’un `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="8d79e-154">The MessagePack protocol doesn't provide a way to encode the `Kind` value of a `DateTime`.</span></span> <span data-ttu-id="8d79e-155">Par conséquent, lors de la désérialisation d’une date, le protocole MessagePack Hub part du principe que la date d’entrée est au format UTC.</span><span class="sxs-lookup"><span data-stu-id="8d79e-155">As a result, when deserializing a date, the MessagePack Hub Protocol assumes the incoming date is in UTC format.</span></span> <span data-ttu-id="8d79e-156">Si vous utilisez des valeurs de `DateTime` en heure locale, nous vous recommandons de convertir au format UTC avant de les envoyer.</span><span class="sxs-lookup"><span data-stu-id="8d79e-156">If you're working with `DateTime` values in local time, we recommend converting to UTC before sending them.</span></span> <span data-ttu-id="8d79e-157">Convertissez-les de l’heure UTC en heure locale lorsque vous les recevez.</span><span class="sxs-lookup"><span data-stu-id="8d79e-157">Convert them from UTC to local time when you receive them.</span></span>

<span data-ttu-id="8d79e-158">Pour plus d’informations sur cette limitation, consultez GitHub issue [ASPNET/SignalR#2632](https://github.com/aspnet/SignalR/issues/2632).</span><span class="sxs-lookup"><span data-stu-id="8d79e-158">For more information on this limitation, see GitHub issue [aspnet/SignalR#2632](https://github.com/aspnet/SignalR/issues/2632).</span></span>

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a><span data-ttu-id="8d79e-159">DateTime. MinValue n’est pas pris en charge par MessagePack dans JavaScript</span><span class="sxs-lookup"><span data-stu-id="8d79e-159">DateTime.MinValue is not supported by MessagePack in JavaScript</span></span>

<span data-ttu-id="8d79e-160">La bibliothèque [msgpack5](https://github.com/mcollina/msgpack5) utilisée par le client JavaScript SignalR ne prend pas en charge le type de `timestamp96` dans MessagePack.</span><span class="sxs-lookup"><span data-stu-id="8d79e-160">The [msgpack5](https://github.com/mcollina/msgpack5) library used by the SignalR JavaScript client doesn't support the `timestamp96` type in MessagePack.</span></span> <span data-ttu-id="8d79e-161">Ce type est utilisé pour encoder les valeurs de date très volumineuses (soit très tôt dans le passé, soit très loin dans le futur).</span><span class="sxs-lookup"><span data-stu-id="8d79e-161">This type is used to encode very large date values (either very early in the past or very far in the future).</span></span> <span data-ttu-id="8d79e-162">La valeur de `DateTime.MinValue` est `January 1, 0001` qui doit être encodée dans une valeur de `timestamp96`.</span><span class="sxs-lookup"><span data-stu-id="8d79e-162">The value of `DateTime.MinValue` is `January 1, 0001` which must be encoded in a `timestamp96` value.</span></span> <span data-ttu-id="8d79e-163">Pour cette raison, l’envoi de `DateTime.MinValue` à un client JavaScript n’est pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="8d79e-163">Because of this, sending `DateTime.MinValue` to a JavaScript client isn't supported.</span></span> <span data-ttu-id="8d79e-164">Lorsque `DateTime.MinValue` est reçu par le client JavaScript, l’erreur suivante est levée :</span><span class="sxs-lookup"><span data-stu-id="8d79e-164">When `DateTime.MinValue` is received by the JavaScript client, the following error is thrown:</span></span>

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

<span data-ttu-id="8d79e-165">En règle générale, `DateTime.MinValue` est utilisé pour encoder une valeur « manquante » ou `null`.</span><span class="sxs-lookup"><span data-stu-id="8d79e-165">Usually, `DateTime.MinValue` is used to encode a "missing" or `null` value.</span></span> <span data-ttu-id="8d79e-166">Si vous devez encoder cette valeur dans MessagePack, utilisez une valeur de `DateTime` Nullable (`DateTime?`) ou encodez une valeur `bool` distincte indiquant si la date est présente.</span><span class="sxs-lookup"><span data-stu-id="8d79e-166">If you need to encode that value in MessagePack, use a nullable `DateTime` value (`DateTime?`) or encode a separate `bool` value indicating if the date is present.</span></span>

<span data-ttu-id="8d79e-167">Pour plus d’informations sur cette limitation, consultez GitHub issue [ASPNET/SignalR#2228](https://github.com/aspnet/SignalR/issues/2228).</span><span class="sxs-lookup"><span data-stu-id="8d79e-167">For more information on this limitation, see GitHub issue [aspnet/SignalR#2228](https://github.com/aspnet/SignalR/issues/2228).</span></span>

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a><span data-ttu-id="8d79e-168">Prise en charge de MessagePack dans l’environnement de compilation « à l’avance »</span><span class="sxs-lookup"><span data-stu-id="8d79e-168">MessagePack support in "ahead-of-time" compilation environment</span></span>

<span data-ttu-id="8d79e-169">La bibliothèque [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp) utilisée par le client et le serveur .NET utilise la génération de code pour optimiser la sérialisation.</span><span class="sxs-lookup"><span data-stu-id="8d79e-169">The [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp) library used by the .NET client and server uses code generation to optimize serialization.</span></span> <span data-ttu-id="8d79e-170">Par conséquent, il n’est pas pris en charge par défaut dans les environnements qui utilisent la compilation « à l’avance » (par exemple, Xamarin iOS ou Unity).</span><span class="sxs-lookup"><span data-stu-id="8d79e-170">As a result, it isn't supported by default on environments that use "ahead-of-time" compilation (such as Xamarin iOS or Unity).</span></span> <span data-ttu-id="8d79e-171">Il est possible d’utiliser MessagePack dans ces environnements en prégénérant le code de sérialiseur/désérialiseur.</span><span class="sxs-lookup"><span data-stu-id="8d79e-171">It's possible to use MessagePack in these environments by "pre-generating" the serializer/deserializer code.</span></span> <span data-ttu-id="8d79e-172">Pour plus d’informations, consultez [la documentation MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp#pre-code-generationunityxamarin-supports).</span><span class="sxs-lookup"><span data-stu-id="8d79e-172">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp#pre-code-generationunityxamarin-supports).</span></span> <span data-ttu-id="8d79e-173">Une fois que vous avez généré le prétraitement des sérialiseurs, vous pouvez les inscrire à l’aide du délégué de configuration passé à `AddMessagePackProtocol`:</span><span class="sxs-lookup"><span data-stu-id="8d79e-173">Once you have pre-generated the serializers, you can register them using the configuration delegate passed to `AddMessagePackProtocol`:</span></span>

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

### <a name="type-checks-are-more-strict-in-messagepack"></a><span data-ttu-id="8d79e-174">Les vérifications de type sont plus strictes dans MessagePack</span><span class="sxs-lookup"><span data-stu-id="8d79e-174">Type checks are more strict in MessagePack</span></span>

<span data-ttu-id="8d79e-175">Le protocole JSON Hub effectue des conversions de type pendant la désérialisation.</span><span class="sxs-lookup"><span data-stu-id="8d79e-175">The JSON Hub Protocol will perform type conversions during deserialization.</span></span> <span data-ttu-id="8d79e-176">Par exemple, si l’objet entrant a une valeur de propriété qui est un nombre (`{ foo: 42 }`), mais que la propriété de la classe .NET est de type `string`, la valeur est convertie.</span><span class="sxs-lookup"><span data-stu-id="8d79e-176">For example, if the incoming object has a property value that is a number (`{ foo: 42 }`) but the property on the .NET class is of type `string`, the value will be converted.</span></span> <span data-ttu-id="8d79e-177">Toutefois, MessagePack n’effectue pas cette conversion et lèvera une exception qui est visible dans les journaux côté serveur (et dans la console) :</span><span class="sxs-lookup"><span data-stu-id="8d79e-177">However, MessagePack doesn't perform this conversion and will throw an exception that can be seen in server-side logs (and in the console):</span></span>

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

<span data-ttu-id="8d79e-178">Pour plus d’informations sur cette limitation, consultez GitHub issue [ASPNET/SignalR#2937](https://github.com/aspnet/SignalR/issues/2937).</span><span class="sxs-lookup"><span data-stu-id="8d79e-178">For more information on this limitation, see GitHub issue [aspnet/SignalR#2937](https://github.com/aspnet/SignalR/issues/2937).</span></span>

## <a name="related-resources"></a><span data-ttu-id="8d79e-179">Ressources connexes</span><span class="sxs-lookup"><span data-stu-id="8d79e-179">Related resources</span></span>

* [<span data-ttu-id="8d79e-180">Bien démarrer</span><span class="sxs-lookup"><span data-stu-id="8d79e-180">Get Started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="8d79e-181">Client .NET</span><span class="sxs-lookup"><span data-stu-id="8d79e-181">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="8d79e-182">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="8d79e-182">JavaScript client</span></span>](xref:signalr/javascript-client)
