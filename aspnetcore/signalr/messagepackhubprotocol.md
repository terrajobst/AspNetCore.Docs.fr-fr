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
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a><span data-ttu-id="1075e-103">Utiliser le protocole MessagePack Hub dans SignalR pour ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1075e-103">Use MessagePack Hub Protocol in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="1075e-104">Par [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="1075e-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="1075e-105">Cet article suppose que le lecteur est familiarisé avec les sujets abordés dans la [prise en main](xref:tutorials/signalr).</span><span class="sxs-lookup"><span data-stu-id="1075e-105">This article assumes the reader is familiar with the topics covered in [Get Started](xref:tutorials/signalr).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="1075e-106">Qu’est MessagePack ?</span><span class="sxs-lookup"><span data-stu-id="1075e-106">What is MessagePack?</span></span>

<span data-ttu-id="1075e-107">[MessagePack](https://msgpack.org/index.html) est un format de sérialisation binaire qui est rapide et compact.</span><span class="sxs-lookup"><span data-stu-id="1075e-107">[MessagePack](https://msgpack.org/index.html) is a binary serialization format that is fast and compact.</span></span> <span data-ttu-id="1075e-108">Il est utile lorsque la bande passante et les performances sont un critère important, car il crée des messages plus petits comparé au [JSON](https://www.json.org/).</span><span class="sxs-lookup"><span data-stu-id="1075e-108">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="1075e-109">S’agissant d’un format binaire, les messages sont illisibles lorsque vous examinez les journaux et traces réseau, sauf si les octets sont transmis via un analyseur MessagePack.</span><span class="sxs-lookup"><span data-stu-id="1075e-109">Because it's a binary format, messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> <span data-ttu-id="1075e-110">SignalR prend en charge le format MessagePack et fournit des API  à utiliser par le client et le serveur.</span><span class="sxs-lookup"><span data-stu-id="1075e-110">SignalR has built-in support for the MessagePack format, and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="1075e-111">Configurer MessagePack sur le serveur</span><span class="sxs-lookup"><span data-stu-id="1075e-111">Configure MessagePack on the server</span></span>

<span data-ttu-id="1075e-112">Pour activer le protocole de Hub MessagePack sur le serveur, installez le package `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` dans votre application.</span><span class="sxs-lookup"><span data-stu-id="1075e-112">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="1075e-113">Dans le fichier Startup.cs ajoutez `AddMessagePackProtocol` à l'appel à `AddSignalR` pour activer la prise en charge MessagePack sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="1075e-113">In the Startup.cs file add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="1075e-114">JSON est activé par défaut.</span><span class="sxs-lookup"><span data-stu-id="1075e-114">JSON is enabled by default.</span></span> <span data-ttu-id="1075e-115">Ajouter de MessagePack permet la prise en charge pour les clients JSON et MessagePack.</span><span class="sxs-lookup"><span data-stu-id="1075e-115">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="1075e-116">Pour personnaliser la façon dont MessagePack met en forme vos données, `AddMessagePackProtocol` prend un délégué pour la configuration des options.</span><span class="sxs-lookup"><span data-stu-id="1075e-116">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="1075e-117">Dans ce délégué, la propriété `FormatterResolvers` peut être utilisée pour configurer les options de sérialisation MessagePack.</span><span class="sxs-lookup"><span data-stu-id="1075e-117">In that delegate, the `FormatterResolvers` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="1075e-118">Pour plus d’informations sur la façon dont les programmes de résolution fonctionne, consultez la bibliothèque de MessagePack à [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span><span class="sxs-lookup"><span data-stu-id="1075e-118">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="1075e-119">Les attributs peuvent être utilisés sur les objets que vous souhaitez sérialiser pour définir comment ils doivent être traités.</span><span class="sxs-lookup"><span data-stu-id="1075e-119">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

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

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="1075e-120">Configurer MessagePack sur le client</span><span class="sxs-lookup"><span data-stu-id="1075e-120">Configure MessagePack on the client</span></span>

> [!NOTE]
> <span data-ttu-id="1075e-121">JSON est activé par défaut pour les clients pris en charge.</span><span class="sxs-lookup"><span data-stu-id="1075e-121">JSON is enabled by default for the supported clients.</span></span> <span data-ttu-id="1075e-122">Les clients peuvent prendre uniquement en charge un seul protocole.</span><span class="sxs-lookup"><span data-stu-id="1075e-122">Clients can only support a single protocol.</span></span> <span data-ttu-id="1075e-123">L'ajout de prise en charge MessagePack remplacera les protocoles précédemment configurés.</span><span class="sxs-lookup"><span data-stu-id="1075e-123">Adding MessagePack support will replace any previously configured protocols.</span></span>

### <a name="net-client"></a><span data-ttu-id="1075e-124">Client .NET</span><span class="sxs-lookup"><span data-stu-id="1075e-124">.NET client</span></span>

<span data-ttu-id="1075e-125">Pour activer MessagePack dans le Client .NET, installez le package `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` et appelez `AddMessagePackProtocol` sur `HubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1075e-125">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="1075e-126">Cet appel `AddMessagePackProtocol` prend un délégué pour la configuration des options comme le serveur.</span><span class="sxs-lookup"><span data-stu-id="1075e-126">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="1075e-127">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="1075e-127">JavaScript client</span></span>

<span data-ttu-id="1075e-128">MessagePack pour le client JavaScript est prise en charge par le `@aspnet/signalr-protocol-msgpack` package npm.</span><span class="sxs-lookup"><span data-stu-id="1075e-128">MessagePack support for the JavaScript client is provided by the `@aspnet/signalr-protocol-msgpack` npm package.</span></span>

```console
npm install @aspnet/signalr-protocol-msgpack
```

<span data-ttu-id="1075e-129">Après avoir installé le package npm, le module peut être utilisé directement par le biais d’un chargeur de module JavaScript ou importé dans le navigateur en référençant le fichier *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*.</span><span class="sxs-lookup"><span data-stu-id="1075e-129">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* file.</span></span> <span data-ttu-id="1075e-130">Dans un navigateur, la bibliothèque `msgpack5` doit également être référencée.</span><span class="sxs-lookup"><span data-stu-id="1075e-130">In a browser, the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="1075e-131">Utilisez un `<script>` balise pour créer une référence.</span><span class="sxs-lookup"><span data-stu-id="1075e-131">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="1075e-132">La bibliothèque, consultez *node_modules\msgpack5\dist\msgpack5.js*.</span><span class="sxs-lookup"><span data-stu-id="1075e-132">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="1075e-133">Lorsque vous utilisez l'élément `<script>`, l’ordre est important.</span><span class="sxs-lookup"><span data-stu-id="1075e-133">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="1075e-134">Si *signalr-protocol-msgpack.js* est référencé avant *msgpack5.js*, une erreur se produit lorsque vous tentez de vous connecter avec MessagePack.</span><span class="sxs-lookup"><span data-stu-id="1075e-134">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="1075e-135">*SignalR.js* est également requis avant *signalr-protocol-msgpack.js*.</span><span class="sxs-lookup"><span data-stu-id="1075e-135">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="1075e-136">L'jout de `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` à `HubConnectionBuilder` configurera le client pour utiliser le protocole MessagePack lors de la connexion à un serveur.</span><span class="sxs-lookup"><span data-stu-id="1075e-136">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="1075e-137">À ce stade, il n’existe aucune option de configuration pour le protocole MessagePack sur le client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1075e-137">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="messagepack-quirks"></a><span data-ttu-id="1075e-138">MessagePack quirks</span><span class="sxs-lookup"><span data-stu-id="1075e-138">MessagePack quirks</span></span>

<span data-ttu-id="1075e-139">Il existe quelques problèmes à connaître lorsque vous utilisez le protocole de Hub MessagePack.</span><span class="sxs-lookup"><span data-stu-id="1075e-139">There are a few issues to be aware of when using the MessagePack Hub Protocol.</span></span>

### <a name="messagepack-is-case-sensitive"></a><span data-ttu-id="1075e-140">MessagePack respecte la casse</span><span class="sxs-lookup"><span data-stu-id="1075e-140">MessagePack is case-sensitive</span></span>

<span data-ttu-id="1075e-141">Le protocole MessagePack respecte la casse.</span><span class="sxs-lookup"><span data-stu-id="1075e-141">The MessagePack protocol is case-sensitive.</span></span> <span data-ttu-id="1075e-142">Par exemple, considérez les éléments suivants C# classe :</span><span class="sxs-lookup"><span data-stu-id="1075e-142">For example, consider the following C# class:</span></span>

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

<span data-ttu-id="1075e-143">Lors de l’envoi à partir du client JavaScript, vous devez utiliser `PascalCased` des noms de propriété, étant donné que la casse doit correspondre à la C# classe exactement.</span><span class="sxs-lookup"><span data-stu-id="1075e-143">When sending from the JavaScript client, you must use `PascalCased` property names, since the casing must match the C# class exactly.</span></span> <span data-ttu-id="1075e-144">Exemple :</span><span class="sxs-lookup"><span data-stu-id="1075e-144">For example:</span></span>

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

<span data-ttu-id="1075e-145">À l’aide de `camelCased` noms ne se lier correctement à la C# classe.</span><span class="sxs-lookup"><span data-stu-id="1075e-145">Using `camelCased` names won't properly bind to the C# class.</span></span> <span data-ttu-id="1075e-146">Vous pouvez contourner ce à l’aide de la `Key` attribut pour spécifier un autre nom pour la propriété MessagePack.</span><span class="sxs-lookup"><span data-stu-id="1075e-146">You can work around this by using the `Key` attribute to specify a different name for the MessagePack property.</span></span> <span data-ttu-id="1075e-147">Pour plus d’informations, consultez [la documentation MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp#object-serialization).</span><span class="sxs-lookup"><span data-stu-id="1075e-147">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp#object-serialization).</span></span>

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a><span data-ttu-id="1075e-148">DateTime.Kind n’est pas conservé lors de la sérialisation/désérialisation</span><span class="sxs-lookup"><span data-stu-id="1075e-148">DateTime.Kind is not preserved when serializing/deserializing</span></span>

<span data-ttu-id="1075e-149">Le protocole MessagePack ne fournit pas un moyen pour encoder le `Kind` valeur d’un `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="1075e-149">The MessagePack protocol doesn't provide a way to encode the `Kind` value of a `DateTime`.</span></span> <span data-ttu-id="1075e-150">Par conséquent, lors de la désérialisation d’une date, le protocole de Hub MessagePack part du principe que la date entrante est au format UTC.</span><span class="sxs-lookup"><span data-stu-id="1075e-150">As a result, when deserializing a date, the MessagePack Hub Protocol assumes the incoming date is in UTC format.</span></span> <span data-ttu-id="1075e-151">Si vous travaillez avec `DateTime` valeurs en heure locale, nous vous recommandons de conversion en heure UTC avant de les envoyer.</span><span class="sxs-lookup"><span data-stu-id="1075e-151">If you're working with `DateTime` values in local time, we recommend converting to UTC before sending them.</span></span> <span data-ttu-id="1075e-152">Convertir les à l’heure UTC en heure locale lorsque vous les recevez.</span><span class="sxs-lookup"><span data-stu-id="1075e-152">Convert them from UTC to local time when you receive them.</span></span>

<span data-ttu-id="1075e-153">Pour plus d’informations sur cette limitation, consultez le GitHub problème [aspnet/SignalR #2632](https://github.com/aspnet/SignalR/issues/2632).</span><span class="sxs-lookup"><span data-stu-id="1075e-153">For more information on this limitation, see GitHub issue [aspnet/SignalR#2632](https://github.com/aspnet/SignalR/issues/2632).</span></span>

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a><span data-ttu-id="1075e-154">DateTime.MinValue n’est pas pris en charge par MessagePack dans JavaScript</span><span class="sxs-lookup"><span data-stu-id="1075e-154">DateTime.MinValue is not supported by MessagePack in JavaScript</span></span>

<span data-ttu-id="1075e-155">Le [msgpack5](https://github.com/mcollina/msgpack5) bibliothèque utilisée par le client SignalR JavaScript ne prend pas en charge la `timestamp96` type dans MessagePack.</span><span class="sxs-lookup"><span data-stu-id="1075e-155">The [msgpack5](https://github.com/mcollina/msgpack5) library used by the SignalR JavaScript client doesn't support the `timestamp96` type in MessagePack.</span></span> <span data-ttu-id="1075e-156">Ce type est utilisé pour encoder les valeurs de date très volumineux (soit très tôt dans le passé, soit très éloignée dans le futur).</span><span class="sxs-lookup"><span data-stu-id="1075e-156">This type is used to encode very large date values (either very early in the past or very far in the future).</span></span> <span data-ttu-id="1075e-157">La valeur de `DateTime.MinValue` est `January 1, 0001` qui doit être codé dans un `timestamp96` valeur.</span><span class="sxs-lookup"><span data-stu-id="1075e-157">The value of `DateTime.MinValue` is `January 1, 0001` which must be encoded in a `timestamp96` value.</span></span> <span data-ttu-id="1075e-158">En raison de cette option, envoi `DateTime.MinValue` à un code JavaScript client n’est pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="1075e-158">Because of this, sending `DateTime.MinValue` to a JavaScript client isn't supported.</span></span> <span data-ttu-id="1075e-159">Lorsque `DateTime.MinValue` est reçu par le client JavaScript, l’erreur suivante est levée :</span><span class="sxs-lookup"><span data-stu-id="1075e-159">When `DateTime.MinValue` is received by the JavaScript client, the following error is thrown:</span></span>

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

<span data-ttu-id="1075e-160">En règle générale, `DateTime.MinValue` est utilisé pour encoder un « manquant » ou `null` valeur.</span><span class="sxs-lookup"><span data-stu-id="1075e-160">Usually, `DateTime.MinValue` is used to encode a "missing" or `null` value.</span></span> <span data-ttu-id="1075e-161">Si vous devez coder cette valeur dans MessagePack, utilisez nullable `DateTime` valeur (`DateTime?`) ou encoder un distinct `bool` valeur indiquant si la date est présente.</span><span class="sxs-lookup"><span data-stu-id="1075e-161">If you need to encode that value in MessagePack, use a nullable `DateTime` value (`DateTime?`) or encode a separate `bool` value indicating if the date is present.</span></span>

<span data-ttu-id="1075e-162">Pour plus d’informations sur cette limitation, consultez le GitHub problème [aspnet/SignalR #2228](https://github.com/aspnet/SignalR/issues/2228).</span><span class="sxs-lookup"><span data-stu-id="1075e-162">For more information on this limitation, see GitHub issue [aspnet/SignalR#2228](https://github.com/aspnet/SignalR/issues/2228).</span></span>

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a><span data-ttu-id="1075e-163">Prise en charge de MessagePack dans l’environnement de compilation de « ahead-of-time »</span><span class="sxs-lookup"><span data-stu-id="1075e-163">MessagePack support in "ahead-of-time" compilation environment</span></span>

<span data-ttu-id="1075e-164">Le [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp) bibliothèque utilisée par le client .NET et le serveur utilise la génération de code pour optimiser la sérialisation.</span><span class="sxs-lookup"><span data-stu-id="1075e-164">The [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp) library used by the .NET client and server uses code generation to optimize serialization.</span></span> <span data-ttu-id="1075e-165">Par conséquent, il n’est pas pris en charge par défaut sur les environnements qui utilisent la compilation « ahead-of-time » (par exemple, Xamarin iOS ou Unity).</span><span class="sxs-lookup"><span data-stu-id="1075e-165">As a result, it isn't supported by default on environments that use "ahead-of-time" compilation (such as Xamarin iOS or Unity).</span></span> <span data-ttu-id="1075e-166">Il est possible d’utiliser MessagePack dans ces environnements par « prégénération de « le code de sérialiseur/désérialiseur.</span><span class="sxs-lookup"><span data-stu-id="1075e-166">It's possible to use MessagePack in these environments by "pre-generating" the serializer/deserializer code.</span></span> <span data-ttu-id="1075e-167">Pour plus d’informations, consultez [la documentation MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp#pre-code-generationunityxamarin-supports).</span><span class="sxs-lookup"><span data-stu-id="1075e-167">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp#pre-code-generationunityxamarin-supports).</span></span> <span data-ttu-id="1075e-168">Une fois que vous avez préalablement généré les sérialiseurs, vous pouvez les enregistrer à l’aide du délégué de configuration transmis à `AddMessagePackProtocol`:</span><span class="sxs-lookup"><span data-stu-id="1075e-168">Once you have pre-generated the serializers, you can register them using the configuration delegate passed to `AddMessagePackProtocol`:</span></span>

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

### <a name="type-checks-are-more-strict-in-messagepack"></a><span data-ttu-id="1075e-169">Vérifications de type sont plus strictes dans MessagePack</span><span class="sxs-lookup"><span data-stu-id="1075e-169">Type checks are more strict in MessagePack</span></span>

<span data-ttu-id="1075e-170">Le protocole de Hub JSON effectue les conversions de type pendant la désérialisation.</span><span class="sxs-lookup"><span data-stu-id="1075e-170">The JSON Hub Protocol will perform type conversions during deserialization.</span></span> <span data-ttu-id="1075e-171">Par exemple, si l’objet entrant a une valeur de propriété qui est un nombre (`{ foo: 42 }`), mais la propriété sur la classe .NET est de type `string`, la valeur sera convertie.</span><span class="sxs-lookup"><span data-stu-id="1075e-171">For example, if the incoming object has a property value that is a number (`{ foo: 42 }`) but the property on the .NET class is of type `string`, the value will be converted.</span></span> <span data-ttu-id="1075e-172">Toutefois, MessagePack n’effectue pas cette conversion et lève une exception qui peut être consultée dans les journaux côté serveur (et dans la console) :</span><span class="sxs-lookup"><span data-stu-id="1075e-172">However, MessagePack doesn't perform this conversion and will throw an exception that can be seen in server-side logs (and in the console):</span></span>

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

<span data-ttu-id="1075e-173">Pour plus d’informations sur cette limitation, consultez le GitHub problème [aspnet/SignalR #2937](https://github.com/aspnet/SignalR/issues/2937).</span><span class="sxs-lookup"><span data-stu-id="1075e-173">For more information on this limitation, see GitHub issue [aspnet/SignalR#2937](https://github.com/aspnet/SignalR/issues/2937).</span></span>

## <a name="related-resources"></a><span data-ttu-id="1075e-174">Ressources connexes</span><span class="sxs-lookup"><span data-stu-id="1075e-174">Related resources</span></span>

* [<span data-ttu-id="1075e-175">Bien démarrer</span><span class="sxs-lookup"><span data-stu-id="1075e-175">Get Started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="1075e-176">Client .NET</span><span class="sxs-lookup"><span data-stu-id="1075e-176">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="1075e-177">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="1075e-177">JavaScript client</span></span>](xref:signalr/javascript-client)
