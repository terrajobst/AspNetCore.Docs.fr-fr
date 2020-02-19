---
title: Gérer les erreurs dans les applications de Blazor ASP.NET Core
author: guardrex
description: Découvrez comment ASP.NET Core Blazor comment Blazor gère les exceptions non gérées et comment développer des applications qui détectent et gèrent les erreurs.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/handle-errors
ms.openlocfilehash: 7191ae50d64ebd6a9b23b391116aedf3a6d01de2
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77447020"
---
# <a name="handle-errors-in-aspnet-core-opno-locblazor-apps"></a><span data-ttu-id="09167-103">Gérer les erreurs dans les applications de Blazor ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="09167-103">Handle errors in ASP.NET Core Blazor apps</span></span>

<span data-ttu-id="09167-104">Par [Steve Sanderson](https://github.com/SteveSandersonMS)</span><span class="sxs-lookup"><span data-stu-id="09167-104">By [Steve Sanderson](https://github.com/SteveSandersonMS)</span></span>

<span data-ttu-id="09167-105">Cet article explique comment Blazor gère les exceptions non gérées et comment développer des applications qui détectent et gèrent les erreurs.</span><span class="sxs-lookup"><span data-stu-id="09167-105">This article describes how Blazor manages unhandled exceptions and how to develop apps that detect and handle errors.</span></span>

## <a name="detailed-errors-during-development"></a><span data-ttu-id="09167-106">Erreurs détaillées pendant le développement</span><span class="sxs-lookup"><span data-stu-id="09167-106">Detailed errors during development</span></span>

<span data-ttu-id="09167-107">Quand une application Blazor ne fonctionne pas correctement pendant le développement, la réception d’informations d’erreur détaillées de l’application vous aide à résoudre les problèmes et à résoudre le problème.</span><span class="sxs-lookup"><span data-stu-id="09167-107">When a Blazor app isn't functioning properly during development, receiving detailed error information from the app assists in troubleshooting and fixing the issue.</span></span> <span data-ttu-id="09167-108">Lorsqu’une erreur se produit, Blazor applications affichent une barre dorée en bas de l’écran :</span><span class="sxs-lookup"><span data-stu-id="09167-108">When an error occurs, Blazor apps display a gold bar at the bottom of the screen:</span></span>

* <span data-ttu-id="09167-109">Pendant le développement, la barre dorée vous dirige vers la console du navigateur, où vous pouvez voir l’exception.</span><span class="sxs-lookup"><span data-stu-id="09167-109">During development, the gold bar directs you to the browser console, where you can see the exception.</span></span>
* <span data-ttu-id="09167-110">En production, la barre dorée avertit l’utilisateur qu’une erreur s’est produite et recommande l’actualisation du navigateur.</span><span class="sxs-lookup"><span data-stu-id="09167-110">In production, the gold bar notifies the user that an error has occurred and recommends refreshing the browser.</span></span>

<span data-ttu-id="09167-111">L’interface utilisateur de cette expérience de gestion des erreurs fait partie des modèles de projet Blazor.</span><span class="sxs-lookup"><span data-stu-id="09167-111">The UI for this error handling experience is part of the Blazor project templates.</span></span>

<span data-ttu-id="09167-112">Dans une application Blazor webassembly, personnalisez l’expérience dans le fichier *wwwroot/index.html* :</span><span class="sxs-lookup"><span data-stu-id="09167-112">In a Blazor WebAssembly app, customize the experience in the *wwwroot/index.html* file:</span></span>

```html
<div id="blazor-error-ui">
    An unhandled error has occurred.
    <a href="" class="reload">Reload</a>
    <a class="dismiss">🗙</a>
</div>
```

<span data-ttu-id="09167-113">Dans une application Blazor Server, personnalisez l’expérience dans le fichier *pages/_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="09167-113">In a Blazor Server app, customize the experience in the *Pages/_Host.cshtml* file:</span></span>

```cshtml
<div id="blazor-error-ui">
    <environment include="Staging,Production">
        An error has occurred. This application may no longer respond until reloaded.
    </environment>
    <environment include="Development">
        An unhandled exception has occurred. See browser dev tools for details.
    </environment>
    <a href="" class="reload">Reload</a>
    <a class="dismiss">🗙</a>
</div>
```

<span data-ttu-id="09167-114">L’élément `blazor-error-ui` est masqué par les styles inclus avec les modèles Blazor, puis indiqué lorsqu’une erreur se produit.</span><span class="sxs-lookup"><span data-stu-id="09167-114">The `blazor-error-ui` element is hidden by the styles included with the Blazor templates and then shown when an error occurs.</span></span>

## <a name="how-a-opno-locblazor-server-app-reacts-to-unhandled-exceptions"></a><span data-ttu-id="09167-115">Comment une application Blazor Server réagit aux exceptions non gérées</span><span class="sxs-lookup"><span data-stu-id="09167-115">How a Blazor Server app reacts to unhandled exceptions</span></span>

Blazor<span data-ttu-id="09167-116"> Server est un Framework avec état.</span><span class="sxs-lookup"><span data-stu-id="09167-116"> Server is a stateful framework.</span></span> <span data-ttu-id="09167-117">Tandis que les utilisateurs interagissent avec une application, ils maintiennent une connexion au serveur appelé « *circuit*».</span><span class="sxs-lookup"><span data-stu-id="09167-117">While users interact with an app, they maintain a connection to the server known as a *circuit*.</span></span> <span data-ttu-id="09167-118">Le circuit contient des instances de composant actives, ainsi que de nombreux autres aspects de l’État, tels que :</span><span class="sxs-lookup"><span data-stu-id="09167-118">The circuit holds active component instances, plus many other aspects of state, such as:</span></span>

* <span data-ttu-id="09167-119">Sortie du rendu le plus récent des composants.</span><span class="sxs-lookup"><span data-stu-id="09167-119">The most recent rendered output of components.</span></span>
* <span data-ttu-id="09167-120">Ensemble actuel de délégués de gestion d’événements qui peuvent être déclenchés par les événements côté client.</span><span class="sxs-lookup"><span data-stu-id="09167-120">The current set of event-handling delegates that could be triggered by client-side events.</span></span>

<span data-ttu-id="09167-121">Si un utilisateur ouvre l’application dans plusieurs onglets de navigateur, il dispose de plusieurs circuits indépendants.</span><span class="sxs-lookup"><span data-stu-id="09167-121">If a user opens the app in multiple browser tabs, they have multiple independent circuits.</span></span>

Blazor<span data-ttu-id="09167-122"> traite la plupart des exceptions non gérées comme étant irrécupérables par le circuit dans lequel elles se produisent.</span><span class="sxs-lookup"><span data-stu-id="09167-122"> treats most unhandled exceptions as fatal to the circuit where they occur.</span></span> <span data-ttu-id="09167-123">Si un circuit est arrêté en raison d’une exception non gérée, l’utilisateur ne peut continuer à interagir avec l’application qu’en rechargeant la page pour créer un nouveau circuit.</span><span class="sxs-lookup"><span data-stu-id="09167-123">If a circuit is terminated due to an unhandled exception, the user can only continue to interact with the app by reloading the page to create a new circuit.</span></span> <span data-ttu-id="09167-124">Les circuits en dehors de celui qui est terminé, qui sont des circuits pour d’autres utilisateurs ou d’autres onglets de navigateur, ne sont pas affectés.</span><span class="sxs-lookup"><span data-stu-id="09167-124">Circuits outside of the one that's terminated, which are circuits for other users or other browser tabs, aren't affected.</span></span> <span data-ttu-id="09167-125">Ce scénario est similaire à une application de bureau qui se bloque&mdash;l’application bloquée doit être redémarrée, mais les autres applications ne sont pas affectées.</span><span class="sxs-lookup"><span data-stu-id="09167-125">This scenario is similar to a desktop app that crashes&mdash;the crashed app must be restarted, but other apps aren't affected.</span></span>

<span data-ttu-id="09167-126">Un circuit se termine lorsqu’une exception non gérée se produit pour les raisons suivantes :</span><span class="sxs-lookup"><span data-stu-id="09167-126">A circuit is terminated when an unhandled exception occurs for the following reasons:</span></span>

* <span data-ttu-id="09167-127">Une exception non gérée rend souvent le circuit dans un État indéfini.</span><span class="sxs-lookup"><span data-stu-id="09167-127">An unhandled exception often leaves the circuit in an undefined state.</span></span>
* <span data-ttu-id="09167-128">L’opération normale de l’application ne peut pas être garantie après une exception non gérée.</span><span class="sxs-lookup"><span data-stu-id="09167-128">The app's normal operation can't be guaranteed after an unhandled exception.</span></span>
* <span data-ttu-id="09167-129">Des failles de sécurité peuvent apparaître dans l’application si le circuit continue.</span><span class="sxs-lookup"><span data-stu-id="09167-129">Security vulnerabilities may appear in the app if the circuit continues.</span></span>

## <a name="manage-unhandled-exceptions-in-developer-code"></a><span data-ttu-id="09167-130">Gérer les exceptions non gérées dans le code du développeur</span><span class="sxs-lookup"><span data-stu-id="09167-130">Manage unhandled exceptions in developer code</span></span>

<span data-ttu-id="09167-131">Pour qu’une application continue après une erreur, l’application doit avoir une logique de gestion des erreurs.</span><span class="sxs-lookup"><span data-stu-id="09167-131">For an app to continue after an error, the app must have error handling logic.</span></span> <span data-ttu-id="09167-132">Les sections suivantes de cet article décrivent les sources potentielles d’exceptions non gérées.</span><span class="sxs-lookup"><span data-stu-id="09167-132">Later sections of this article describe potential sources of unhandled exceptions.</span></span>

<span data-ttu-id="09167-133">En production, ne rendez pas les messages d’exception d’infrastructure ou les traces de pile dans l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="09167-133">In production, don't render framework exception messages or stack traces in the UI.</span></span> <span data-ttu-id="09167-134">Le rendu des messages d’exception ou des traces de pile peut :</span><span class="sxs-lookup"><span data-stu-id="09167-134">Rendering exception messages or stack traces could:</span></span>

* <span data-ttu-id="09167-135">Divulguer des informations sensibles aux utilisateurs finaux.</span><span class="sxs-lookup"><span data-stu-id="09167-135">Disclose sensitive information to end users.</span></span>
* <span data-ttu-id="09167-136">Aidez un utilisateur malveillant à découvrir des faiblesses dans une application qui peuvent compromettre la sécurité de l’application, du serveur ou du réseau.</span><span class="sxs-lookup"><span data-stu-id="09167-136">Help a malicious user discover weaknesses in an app that can compromise the security of the app, server, or network.</span></span>

## <a name="log-errors-with-a-persistent-provider"></a><span data-ttu-id="09167-137">Consigner les erreurs avec un fournisseur persistant</span><span class="sxs-lookup"><span data-stu-id="09167-137">Log errors with a persistent provider</span></span>

<span data-ttu-id="09167-138">Si une exception non gérée se produit, l’exception est consignée dans <xref:Microsoft.Extensions.Logging.ILogger> instances configurées dans le conteneur de service.</span><span class="sxs-lookup"><span data-stu-id="09167-138">If an unhandled exception occurs, the exception is logged to <xref:Microsoft.Extensions.Logging.ILogger> instances configured in the service container.</span></span> <span data-ttu-id="09167-139">Par défaut, les applications Blazor sont consignées dans la sortie de la console avec le fournisseur de journalisation de la console.</span><span class="sxs-lookup"><span data-stu-id="09167-139">By default, Blazor apps log to console output with the Console Logging Provider.</span></span> <span data-ttu-id="09167-140">Envisagez de vous connecter à un emplacement plus permanent avec un fournisseur qui gère la taille du journal et la rotation des journaux.</span><span class="sxs-lookup"><span data-stu-id="09167-140">Consider logging to a more permanent location with a provider that manages log size and log rotation.</span></span> <span data-ttu-id="09167-141">Pour plus d’informations, consultez <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="09167-141">For more information, see <xref:fundamentals/logging/index>.</span></span>

<span data-ttu-id="09167-142">Pendant le développement, Blazor envoie généralement les détails complets des exceptions à la console du navigateur pour faciliter le débogage.</span><span class="sxs-lookup"><span data-stu-id="09167-142">During development, Blazor usually sends the full details of exceptions to the browser's console to aid in debugging.</span></span> <span data-ttu-id="09167-143">En production, les erreurs détaillées dans la console du navigateur sont désactivées par défaut, ce qui signifie que les erreurs ne sont pas envoyées aux clients, mais que les détails complets de l’exception sont toujours consignés côté serveur.</span><span class="sxs-lookup"><span data-stu-id="09167-143">In production, detailed errors in the browser's console are disabled by default, which means that errors aren't sent to clients but the exception's full details are still logged server-side.</span></span> <span data-ttu-id="09167-144">Pour plus d’informations, consultez <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="09167-144">For more information, see <xref:fundamentals/error-handling>.</span></span>

<span data-ttu-id="09167-145">Vous devez choisir les incidents à enregistrer et le niveau de gravité des incidents journalisés.</span><span class="sxs-lookup"><span data-stu-id="09167-145">You must decide which incidents to log and the level of severity of logged incidents.</span></span> <span data-ttu-id="09167-146">Les utilisateurs hostiles peuvent être en mesure de déclencher délibérément des erreurs.</span><span class="sxs-lookup"><span data-stu-id="09167-146">Hostile users might be able to trigger errors deliberately.</span></span> <span data-ttu-id="09167-147">Par exemple, ne consignez pas un incident à partir d’une erreur où un `ProductId` inconnu est fourni dans l’URL d’un composant qui affiche les détails du produit.</span><span class="sxs-lookup"><span data-stu-id="09167-147">For example, don't log an incident from an error where an unknown `ProductId` is supplied in the URL of a component that displays product details.</span></span> <span data-ttu-id="09167-148">Toutes les erreurs ne doivent pas être traitées comme des incidents de gravité élevée pour la journalisation.</span><span class="sxs-lookup"><span data-stu-id="09167-148">Not all errors should be treated as high-severity incidents for logging.</span></span>

## <a name="places-where-errors-may-occur"></a><span data-ttu-id="09167-149">Emplacements où des erreurs peuvent se produire</span><span class="sxs-lookup"><span data-stu-id="09167-149">Places where errors may occur</span></span>

<span data-ttu-id="09167-150">Le code d’infrastructure et d’application peut déclencher des exceptions non prises en charge dans l’un des emplacements suivants :</span><span class="sxs-lookup"><span data-stu-id="09167-150">Framework and app code may trigger unhandled exceptions in any of the following locations:</span></span>

* [<span data-ttu-id="09167-151">Instanciation du composant</span><span class="sxs-lookup"><span data-stu-id="09167-151">Component instantiation</span></span>](#component-instantiation)
* [<span data-ttu-id="09167-152">Méthodes de cycle de vie</span><span class="sxs-lookup"><span data-stu-id="09167-152">Lifecycle methods</span></span>](#lifecycle-methods)
* [<span data-ttu-id="09167-153">Logique de rendu</span><span class="sxs-lookup"><span data-stu-id="09167-153">Rendering logic</span></span>](#rendering-logic)
* [<span data-ttu-id="09167-154">Gestionnaires d’événements</span><span class="sxs-lookup"><span data-stu-id="09167-154">Event handlers</span></span>](#event-handlers)
* [<span data-ttu-id="09167-155">Suppression de composants</span><span class="sxs-lookup"><span data-stu-id="09167-155">Component disposal</span></span>](#component-disposal)
* [<span data-ttu-id="09167-156">Interopérabilité JavaScript</span><span class="sxs-lookup"><span data-stu-id="09167-156">JavaScript interop</span></span>](#javascript-interop)
* <span data-ttu-id="09167-157">[gestionnaires de circuits Blazor Server](#blazor-server-circuit-handlers)</span><span class="sxs-lookup"><span data-stu-id="09167-157">[Blazor Server circuit handlers](#blazor-server-circuit-handlers)</span></span>
* <span data-ttu-id="09167-158">[élimination du circuit Blazor Server](#blazor-server-circuit-disposal)</span><span class="sxs-lookup"><span data-stu-id="09167-158">[Blazor Server circuit disposal](#blazor-server-circuit-disposal)</span></span>
* <span data-ttu-id="09167-159">[réaffichage du serveur de Blazor](#blazor-server-prerendering)</span><span class="sxs-lookup"><span data-stu-id="09167-159">[Blazor Server rerendering](#blazor-server-prerendering)</span></span>

<span data-ttu-id="09167-160">Les exceptions non gérées précédentes sont décrites dans les sections suivantes de cet article.</span><span class="sxs-lookup"><span data-stu-id="09167-160">The preceding unhandled exceptions are described in the following sections of this article.</span></span>

### <a name="component-instantiation"></a><span data-ttu-id="09167-161">Instanciation du composant</span><span class="sxs-lookup"><span data-stu-id="09167-161">Component instantiation</span></span>

<span data-ttu-id="09167-162">Lorsque Blazor crée une instance d’un composant :</span><span class="sxs-lookup"><span data-stu-id="09167-162">When Blazor creates an instance of a component:</span></span>

* <span data-ttu-id="09167-163">Le constructeur du composant est appelé.</span><span class="sxs-lookup"><span data-stu-id="09167-163">The component's constructor is invoked.</span></span>
* <span data-ttu-id="09167-164">Les constructeurs de tout service DI non-Singleton fourni au constructeur du composant via la directive [`@inject`](xref:blazor/dependency-injection#request-a-service-in-a-component) ou l’attribut [`[Inject]`](xref:blazor/dependency-injection#request-a-service-in-a-component) sont appelés.</span><span class="sxs-lookup"><span data-stu-id="09167-164">The constructors of any non-singleton DI services supplied to the component's constructor via the [`@inject`](xref:blazor/dependency-injection#request-a-service-in-a-component) directive or the [`[Inject]`](xref:blazor/dependency-injection#request-a-service-in-a-component) attribute are invoked.</span></span>

<span data-ttu-id="09167-165">Un circuit Blazor Server échoue quand un constructeur exécuté ou une méthode setter pour une propriété `[Inject]` lève une exception non gérée.</span><span class="sxs-lookup"><span data-stu-id="09167-165">A Blazor Server circuit fails when any executed constructor or a setter for any `[Inject]` property throws an unhandled exception.</span></span> <span data-ttu-id="09167-166">L’exception est irrécupérable, car l’infrastructure ne peut pas instancier le composant.</span><span class="sxs-lookup"><span data-stu-id="09167-166">The exception is fatal because the framework can't instantiate the component.</span></span> <span data-ttu-id="09167-167">Si la logique du constructeur peut lever des exceptions, l’application doit intercepter les exceptions à l’aide d’une instruction [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) avec la gestion des erreurs et la journalisation.</span><span class="sxs-lookup"><span data-stu-id="09167-167">If constructor logic may throw exceptions, the app should trap the exceptions using a [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statement with error handling and logging.</span></span>

### <a name="lifecycle-methods"></a><span data-ttu-id="09167-168">Méthodes de cycle de vie</span><span class="sxs-lookup"><span data-stu-id="09167-168">Lifecycle methods</span></span>

<span data-ttu-id="09167-169">Pendant la durée de vie d’un composant, Blazor appelle les [méthodes de cycle de vie](xref:blazor/lifecycle)suivantes :</span><span class="sxs-lookup"><span data-stu-id="09167-169">During the lifetime of a component, Blazor invokes the following [lifecycle methods](xref:blazor/lifecycle):</span></span>

* `OnInitialized` / `OnInitializedAsync`
* `OnParametersSet` / `OnParametersSetAsync`
* `ShouldRender` / `ShouldRenderAsync`
* `OnAfterRender` / `OnAfterRenderAsync`

<span data-ttu-id="09167-170">Si une méthode de cycle de vie lève une exception, de manière synchrone ou asynchrone, l’exception est irrécupérable pour un circuit Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="09167-170">If any lifecycle method throws an exception, synchronously or asynchronously, the exception is fatal to a Blazor Server circuit.</span></span> <span data-ttu-id="09167-171">Pour les composants qui gèrent les erreurs dans les méthodes de cycle de vie, ajoutez une logique de gestion des erreurs.</span><span class="sxs-lookup"><span data-stu-id="09167-171">For components to deal with errors in lifecycle methods, add error handling logic.</span></span>

<span data-ttu-id="09167-172">Dans l’exemple suivant, où `OnParametersSetAsync` appelle une méthode pour obtenir un produit :</span><span class="sxs-lookup"><span data-stu-id="09167-172">In the following example where `OnParametersSetAsync` calls a method to obtain a product:</span></span>

* <span data-ttu-id="09167-173">Une exception levée dans la méthode `ProductRepository.GetProductByIdAsync` est gérée par une instruction `try-catch`.</span><span class="sxs-lookup"><span data-stu-id="09167-173">An exception thrown in the `ProductRepository.GetProductByIdAsync` method is handled by a `try-catch` statement.</span></span>
* <span data-ttu-id="09167-174">Lorsque le bloc `catch` est exécuté :</span><span class="sxs-lookup"><span data-stu-id="09167-174">When the `catch` block is executed:</span></span>
  * <span data-ttu-id="09167-175">`loadFailed` est défini sur `true`, qui est utilisé pour afficher un message d’erreur à l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="09167-175">`loadFailed` is set to `true`, which is used to display an error message to the user.</span></span>
  * <span data-ttu-id="09167-176">L’erreur est consignée.</span><span class="sxs-lookup"><span data-stu-id="09167-176">The error is logged.</span></span>

[!code-razor[](handle-errors/samples_snapshot/3.x/product-details.razor?highlight=11,27-39)]

### <a name="rendering-logic"></a><span data-ttu-id="09167-177">Logique de rendu</span><span class="sxs-lookup"><span data-stu-id="09167-177">Rendering logic</span></span>

<span data-ttu-id="09167-178">Le balisage déclaratif dans un fichier de composant `.razor` est compilé dans C# une méthode appelée `BuildRenderTree`.</span><span class="sxs-lookup"><span data-stu-id="09167-178">The declarative markup in a `.razor` component file is compiled into a C# method called `BuildRenderTree`.</span></span> <span data-ttu-id="09167-179">Quand un composant s’affiche, `BuildRenderTree` exécute et génère une structure de données décrivant les éléments, le texte et les composants enfants du composant rendu.</span><span class="sxs-lookup"><span data-stu-id="09167-179">When a component renders, `BuildRenderTree` executes and builds up a data structure describing the elements, text, and child components of the rendered component.</span></span>

<span data-ttu-id="09167-180">La logique de rendu peut lever une exception.</span><span class="sxs-lookup"><span data-stu-id="09167-180">Rendering logic can throw an exception.</span></span> <span data-ttu-id="09167-181">Un exemple de ce scénario se produit lorsque `@someObject.PropertyName` est évaluée, mais que `@someObject` est `null`.</span><span class="sxs-lookup"><span data-stu-id="09167-181">An example of this scenario occurs when `@someObject.PropertyName` is evaluated but `@someObject` is `null`.</span></span> <span data-ttu-id="09167-182">Une exception non gérée levée par la logique de rendu est irrécupérable pour un circuit Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="09167-182">An unhandled exception thrown by rendering logic is fatal to a Blazor Server circuit.</span></span>

<span data-ttu-id="09167-183">Pour éviter une exception de référence null dans la logique de rendu, recherchez un objet `null` avant d’accéder à ses membres.</span><span class="sxs-lookup"><span data-stu-id="09167-183">To prevent a null reference exception in rendering logic, check for a `null` object before accessing its members.</span></span> <span data-ttu-id="09167-184">Dans l’exemple suivant, `person.Address` propriétés ne sont pas accessibles si `person.Address` est `null`:</span><span class="sxs-lookup"><span data-stu-id="09167-184">In the following example, `person.Address` properties aren't accessed if `person.Address` is `null`:</span></span>

[!code-razor[](handle-errors/samples_snapshot/3.x/person-example.razor?highlight=1)]

<span data-ttu-id="09167-185">Le code précédent suppose que `person` n’est pas `null`.</span><span class="sxs-lookup"><span data-stu-id="09167-185">The preceding code assumes that `person` isn't `null`.</span></span> <span data-ttu-id="09167-186">Souvent, la structure du code garantit l’existence d’un objet au moment du rendu du composant.</span><span class="sxs-lookup"><span data-stu-id="09167-186">Often, the structure of the code guarantees that an object exists at the time the component is rendered.</span></span> <span data-ttu-id="09167-187">Dans ce cas, il n’est pas nécessaire de vérifier la `null` dans la logique de rendu.</span><span class="sxs-lookup"><span data-stu-id="09167-187">In those cases, it isn't necessary to check for `null` in rendering logic.</span></span> <span data-ttu-id="09167-188">Dans l’exemple précédent, il est possible que `person` existe, car `person` est créé lors de l’instanciation du composant.</span><span class="sxs-lookup"><span data-stu-id="09167-188">In the prior example, `person` might be guaranteed to exist because `person` is created when the component is instantiated.</span></span>

### <a name="event-handlers"></a><span data-ttu-id="09167-189">Gestionnaires d’événements</span><span class="sxs-lookup"><span data-stu-id="09167-189">Event handlers</span></span>

<span data-ttu-id="09167-190">Le code côté client déclenche des appels de code lors C# de la création de gestionnaires d’événements à l’aide de :</span><span class="sxs-lookup"><span data-stu-id="09167-190">Client-side code triggers invocations of C# code when event handlers are created using:</span></span>

* `@onclick`
* `@onchange`
* <span data-ttu-id="09167-191">Autres attributs `@on...`</span><span class="sxs-lookup"><span data-stu-id="09167-191">Other `@on...` attributes</span></span>
* `@bind`

<span data-ttu-id="09167-192">Le code du gestionnaire d’événements peut lever une exception non gérée dans ces scénarios.</span><span class="sxs-lookup"><span data-stu-id="09167-192">Event handler code might throw an unhandled exception in these scenarios.</span></span>

<span data-ttu-id="09167-193">Si un gestionnaire d’événements lève une exception non gérée (par exemple, une requête de base de données échoue), l’exception est irrécupérable pour un circuit Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="09167-193">If an event handler throws an unhandled exception (for example, a database query fails), the exception is fatal to a Blazor Server circuit.</span></span> <span data-ttu-id="09167-194">Si l’application appelle du code qui pourrait échouer pour des raisons externes, interceptez les exceptions à l’aide d’une instruction [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) avec gestion des erreurs et journalisation.</span><span class="sxs-lookup"><span data-stu-id="09167-194">If the app calls code that could fail for external reasons, trap exceptions using a [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statement with error handling and logging.</span></span>

<span data-ttu-id="09167-195">Si le code utilisateur n’intercepte pas et ne gère pas l’exception, le Framework journalise l’exception et met fin au circuit.</span><span class="sxs-lookup"><span data-stu-id="09167-195">If user code doesn't trap and handle the exception, the framework logs the exception and terminates the circuit.</span></span>

### <a name="component-disposal"></a><span data-ttu-id="09167-196">Suppression de composants</span><span class="sxs-lookup"><span data-stu-id="09167-196">Component disposal</span></span>

<span data-ttu-id="09167-197">Un composant peut être supprimé de l’interface utilisateur, par exemple, parce que l’utilisateur a accédé à une autre page.</span><span class="sxs-lookup"><span data-stu-id="09167-197">A component may be removed from the UI, for example, because the user has navigated to another page.</span></span> <span data-ttu-id="09167-198">Quand un composant qui implémente <xref:System.IDisposable?displayProperty=fullName> est supprimé de l’interface utilisateur, le Framework appelle la méthode <xref:System.IDisposable.Dispose*> du composant.</span><span class="sxs-lookup"><span data-stu-id="09167-198">When a component that implements <xref:System.IDisposable?displayProperty=fullName> is removed from the UI, the framework calls the component's <xref:System.IDisposable.Dispose*> method.</span></span>

<span data-ttu-id="09167-199">Si la méthode `Dispose` du composant lève une exception non gérée, l’exception est irrécupérable pour un circuit Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="09167-199">If the component's `Dispose` method throws an unhandled exception, the exception is fatal to a Blazor Server circuit.</span></span> <span data-ttu-id="09167-200">Si la logique de suppression peut lever des exceptions, l’application doit intercepter les exceptions à l’aide d’une instruction [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) avec la gestion des erreurs et la journalisation.</span><span class="sxs-lookup"><span data-stu-id="09167-200">If disposal logic may throw exceptions, the app should trap the exceptions using a [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statement with error handling and logging.</span></span>

<span data-ttu-id="09167-201">Pour plus d’informations sur la suppression de composants, consultez <xref:blazor/lifecycle#component-disposal-with-idisposable>.</span><span class="sxs-lookup"><span data-stu-id="09167-201">For more information on component disposal, see <xref:blazor/lifecycle#component-disposal-with-idisposable>.</span></span>

### <a name="javascript-interop"></a><span data-ttu-id="09167-202">Interopérabilité de JavaScript</span><span class="sxs-lookup"><span data-stu-id="09167-202">JavaScript interop</span></span>

<span data-ttu-id="09167-203">`IJSRuntime.InvokeAsync<T>` permet au code .NET d’effectuer des appels asynchrones au runtime JavaScript dans le navigateur de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="09167-203">`IJSRuntime.InvokeAsync<T>` allows .NET code to make asynchronous calls to the JavaScript runtime in the user's browser.</span></span>

<span data-ttu-id="09167-204">Les conditions suivantes s’appliquent à la gestion des erreurs avec `InvokeAsync<T>`:</span><span class="sxs-lookup"><span data-stu-id="09167-204">The following conditions apply to error handling with `InvokeAsync<T>`:</span></span>

* <span data-ttu-id="09167-205">Si un appel à `InvokeAsync<T>` échoue de façon synchrone, une exception .NET se produit.</span><span class="sxs-lookup"><span data-stu-id="09167-205">If a call to `InvokeAsync<T>` fails synchronously, a .NET exception occurs.</span></span> <span data-ttu-id="09167-206">Un appel à `InvokeAsync<T>` peut échouer, par exemple, car les arguments fournis ne peuvent pas être sérialisés.</span><span class="sxs-lookup"><span data-stu-id="09167-206">A call to `InvokeAsync<T>` may fail, for example, because the supplied arguments can't be serialized.</span></span> <span data-ttu-id="09167-207">Le code du développeur doit intercepter l’exception.</span><span class="sxs-lookup"><span data-stu-id="09167-207">Developer code must catch the exception.</span></span> <span data-ttu-id="09167-208">Si le code d’application dans une méthode de gestionnaire d’événements ou de cycle de vie de composant ne gère pas une exception, l’exception résultante est irrécupérable pour un circuit Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="09167-208">If app code in an event handler or component lifecycle method doesn't handle an exception, the resulting exception is fatal to a Blazor Server circuit.</span></span>
* <span data-ttu-id="09167-209">Si un appel à `InvokeAsync<T>` échoue de manière asynchrone, le <xref:System.Threading.Tasks.Task> .NET échoue.</span><span class="sxs-lookup"><span data-stu-id="09167-209">If a call to `InvokeAsync<T>` fails asynchronously, the .NET <xref:System.Threading.Tasks.Task> fails.</span></span> <span data-ttu-id="09167-210">Un appel à `InvokeAsync<T>` peut échouer, par exemple, car le code côté JavaScript lève une exception ou retourne un `Promise` qui s’est terminé comme `rejected`.</span><span class="sxs-lookup"><span data-stu-id="09167-210">A call to `InvokeAsync<T>` may fail, for example, because the JavaScript-side code throws an exception or returns a `Promise` that completed as `rejected`.</span></span> <span data-ttu-id="09167-211">Le code du développeur doit intercepter l’exception.</span><span class="sxs-lookup"><span data-stu-id="09167-211">Developer code must catch the exception.</span></span> <span data-ttu-id="09167-212">Si vous utilisez l’opérateur [await](/dotnet/csharp/language-reference/keywords/await) , encapsulez l’appel de méthode dans une instruction [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) avec la gestion des erreurs et la journalisation.</span><span class="sxs-lookup"><span data-stu-id="09167-212">If using the [await](/dotnet/csharp/language-reference/keywords/await) operator, consider wrapping the method call in a [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statement with error handling and logging.</span></span> <span data-ttu-id="09167-213">Dans le cas contraire, le code défaillant entraîne une exception non gérée qui est irrécupérable pour un circuit Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="09167-213">Otherwise, the failing code results in an unhandled exception that's fatal to a Blazor Server circuit.</span></span>
* <span data-ttu-id="09167-214">Par défaut, les appels à `InvokeAsync<T>` doivent se terminer dans un laps de temps donné, sinon l’appel expire. Le délai d’expiration par défaut est d’une minute.</span><span class="sxs-lookup"><span data-stu-id="09167-214">By default, calls to `InvokeAsync<T>` must complete within a certain period or else the call times out. The default timeout period is one minute.</span></span> <span data-ttu-id="09167-215">Le délai d’attente protège le code contre toute perte de connectivité réseau ou de code JavaScript qui ne renvoie jamais de message d’achèvement.</span><span class="sxs-lookup"><span data-stu-id="09167-215">The timeout protects the code against a loss in network connectivity or JavaScript code that never sends back a completion message.</span></span> <span data-ttu-id="09167-216">Si l’appel expire, le `Task` résultant échoue avec une <xref:System.OperationCanceledException>.</span><span class="sxs-lookup"><span data-stu-id="09167-216">If the call times out, the resulting `Task` fails with an <xref:System.OperationCanceledException>.</span></span> <span data-ttu-id="09167-217">Interceptez et traitez l’exception avec la journalisation.</span><span class="sxs-lookup"><span data-stu-id="09167-217">Trap and process the exception with logging.</span></span>

<span data-ttu-id="09167-218">De même, le code JavaScript peut initier des appels à des méthodes .NET indiquées par l’attribut [`[JSInvokable]`](xref:blazor/javascript-interop#invoke-net-methods-from-javascript-functions) .</span><span class="sxs-lookup"><span data-stu-id="09167-218">Similarly, JavaScript code may initiate calls to .NET methods indicated by the [`[JSInvokable]`](xref:blazor/javascript-interop#invoke-net-methods-from-javascript-functions) attribute.</span></span> <span data-ttu-id="09167-219">Si ces méthodes .NET lèvent une exception non gérée :</span><span class="sxs-lookup"><span data-stu-id="09167-219">If these .NET methods throw an unhandled exception:</span></span>

* <span data-ttu-id="09167-220">L’exception n’est pas considérée comme irrécupérable pour un circuit Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="09167-220">The exception isn't treated as fatal to a Blazor Server circuit.</span></span>
* <span data-ttu-id="09167-221">Le `Promise` côté JavaScript est rejeté.</span><span class="sxs-lookup"><span data-stu-id="09167-221">The JavaScript-side `Promise` is rejected.</span></span>

<span data-ttu-id="09167-222">Vous avez la possibilité d’utiliser le code de gestion des erreurs côté .NET ou JavaScript de l’appel de méthode.</span><span class="sxs-lookup"><span data-stu-id="09167-222">You have the option of using error handling code on either the .NET side or the JavaScript side of the method call.</span></span>

<span data-ttu-id="09167-223">Pour plus d’informations, consultez <xref:blazor/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="09167-223">For more information, see <xref:blazor/javascript-interop>.</span></span>

### <a name="opno-locblazor-server-circuit-handlers"></a><span data-ttu-id="09167-224">gestionnaires de circuits Blazor Server</span><span class="sxs-lookup"><span data-stu-id="09167-224">Blazor Server circuit handlers</span></span>

Blazor<span data-ttu-id="09167-225"> Server permet au code de définir un *Gestionnaire de circuit*qui permet d’exécuter du code sur les modifications de l’état du circuit d’un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="09167-225"> Server allows code to define a *circuit handler*, which allows running code on changes to the state of a user's circuit.</span></span> <span data-ttu-id="09167-226">Un gestionnaire de circuit est implémenté en dérivant de `CircuitHandler` et en inscrivant la classe dans le conteneur de services de l’application.</span><span class="sxs-lookup"><span data-stu-id="09167-226">A circuit handler is implemented by deriving from `CircuitHandler` and registering the class in the app's service container.</span></span> <span data-ttu-id="09167-227">L’exemple suivant d’un gestionnaire de circuit effectue le suivi des connexions SignalR ouvertes :</span><span class="sxs-lookup"><span data-stu-id="09167-227">The following example of a circuit handler tracks open SignalR connections:</span></span>

```csharp
using System.Collections.Generic;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Components.Server.Circuits;

public class TrackingCircuitHandler : CircuitHandler
{
    private HashSet<Circuit> _circuits = new HashSet<Circuit>();

    public override Task OnConnectionUpAsync(Circuit circuit, 
        CancellationToken cancellationToken)
    {
        _circuits.Add(circuit);

        return Task.CompletedTask;
    }

    public override Task OnConnectionDownAsync(Circuit circuit, 
        CancellationToken cancellationToken)
    {
        _circuits.Remove(circuit);

        return Task.CompletedTask;
    }

    public int ConnectedCircuits => _circuits.Count;
}
```

<span data-ttu-id="09167-228">Les gestionnaires de circuits sont inscrits à l’aide de DI.</span><span class="sxs-lookup"><span data-stu-id="09167-228">Circuit handlers are registered using DI.</span></span> <span data-ttu-id="09167-229">Les instances délimitées sont créées par instance d’un circuit.</span><span class="sxs-lookup"><span data-stu-id="09167-229">Scoped instances are created per instance of a circuit.</span></span> <span data-ttu-id="09167-230">À l’aide de la `TrackingCircuitHandler` dans l’exemple précédent, un service Singleton est créé car l’état de tous les circuits doit être suivi :</span><span class="sxs-lookup"><span data-stu-id="09167-230">Using the `TrackingCircuitHandler` in the preceding example, a singleton service is created because the state of all circuits must be tracked:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    ...
    services.AddSingleton<CircuitHandler, TrackingCircuitHandler>();
}
```

<span data-ttu-id="09167-231">Si les méthodes d’un gestionnaire de circuit personnalisé lèvent une exception non gérée, l’exception est irrémédiable au circuit du serveur Blazor.</span><span class="sxs-lookup"><span data-stu-id="09167-231">If a custom circuit handler's methods throw an unhandled exception, the exception is fatal to the Blazor Server circuit.</span></span> <span data-ttu-id="09167-232">Pour tolérer des exceptions dans le code d’un gestionnaire ou les méthodes appelées, encapsulez le code dans une ou plusieurs instructions [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) avec la gestion des erreurs et la journalisation.</span><span class="sxs-lookup"><span data-stu-id="09167-232">To tolerate exceptions in a handler's code or called methods, wrap the code in one or more [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statements with error handling and logging.</span></span>

### <a name="opno-locblazor-server-circuit-disposal"></a><span data-ttu-id="09167-233">élimination du circuit Blazor Server</span><span class="sxs-lookup"><span data-stu-id="09167-233">Blazor Server circuit disposal</span></span>

<span data-ttu-id="09167-234">Lorsqu’un circuit se termine parce qu’un utilisateur s’est déconnecté et que l’infrastructure nettoie l’état du circuit, le Framework supprime l’étendue de l’injection de port.</span><span class="sxs-lookup"><span data-stu-id="09167-234">When a circuit ends because a user has disconnected and the framework is cleaning up the circuit state, the framework disposes of the circuit's DI scope.</span></span> <span data-ttu-id="09167-235">La suppression de l’étendue supprime tous les services d’étendue de circuit qui implémentent <xref:System.IDisposable?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="09167-235">Disposing the scope disposes any circuit-scoped DI services that implement <xref:System.IDisposable?displayProperty=fullName>.</span></span> <span data-ttu-id="09167-236">Si un service d’injection de services lève une exception non gérée pendant la suppression, le Framework journalise l’exception.</span><span class="sxs-lookup"><span data-stu-id="09167-236">If any DI service throws an unhandled exception during disposal, the framework logs the exception.</span></span>

### <a name="opno-locblazor-server-prerendering"></a><span data-ttu-id="09167-237">prérendu du serveur Blazor</span><span class="sxs-lookup"><span data-stu-id="09167-237">Blazor Server prerendering</span></span>

Blazor<span data-ttu-id="09167-238"> composants peuvent être prérendus à l’aide du tag Helper `Component` afin que le balisage HTML rendu soit renvoyé dans le cadre de la requête HTTP initiale de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="09167-238"> components can be prerendered using the `Component` Tag Helper so that their rendered HTML markup is returned as part of the user's initial HTTP request.</span></span> <span data-ttu-id="09167-239">Cela fonctionne de la façon suivante :</span><span class="sxs-lookup"><span data-stu-id="09167-239">This works by:</span></span>

* <span data-ttu-id="09167-240">Création d’un nouveau circuit pour tous les composants prérendus qui font partie de la même page.</span><span class="sxs-lookup"><span data-stu-id="09167-240">Creating a new circuit for all of the prerendered components that are part of the same page.</span></span>
* <span data-ttu-id="09167-241">Génération du code HTML initial.</span><span class="sxs-lookup"><span data-stu-id="09167-241">Generating the initial HTML.</span></span>
* <span data-ttu-id="09167-242">Le traitement du circuit comme `disconnected` jusqu’à ce que le navigateur de l’utilisateur établisse une connexion SignalR au même serveur.</span><span class="sxs-lookup"><span data-stu-id="09167-242">Treating the circuit as `disconnected` until the user's browser establishes a SignalR connection back to the same server.</span></span> <span data-ttu-id="09167-243">Lorsque la connexion est établie, l’interactivité sur le circuit est reprise et le balisage HTML des composants est mis à jour.</span><span class="sxs-lookup"><span data-stu-id="09167-243">When the connection is established, interactivity on the circuit is resumed and the components' HTML markup is updated.</span></span>

<span data-ttu-id="09167-244">Si un composant lève une exception non gérée pendant le prérendu, par exemple, pendant une méthode de cycle de vie ou dans une logique de rendu :</span><span class="sxs-lookup"><span data-stu-id="09167-244">If any component throws an unhandled exception during prerendering, for example, during a lifecycle method or in rendering logic:</span></span>

* <span data-ttu-id="09167-245">L’exception est irrécupérable pour le circuit.</span><span class="sxs-lookup"><span data-stu-id="09167-245">The exception is fatal to the circuit.</span></span>
* <span data-ttu-id="09167-246">L’exception est levée dans la pile des appels à partir du tag Helper `Component`.</span><span class="sxs-lookup"><span data-stu-id="09167-246">The exception is thrown up the call stack from the `Component` Tag Helper.</span></span> <span data-ttu-id="09167-247">Par conséquent, la requête HTTP entière échoue, sauf si l’exception est explicitement interceptée par le code du développeur.</span><span class="sxs-lookup"><span data-stu-id="09167-247">Therefore, the entire HTTP request fails unless the exception is explicitly caught by developer code.</span></span>

<span data-ttu-id="09167-248">Dans des circonstances normales, lorsque le prérendu échoue, la création et le rendu du composant n’ont pas de sens, car un composant de travail ne peut pas être rendu.</span><span class="sxs-lookup"><span data-stu-id="09167-248">Under normal circumstances when prerendering fails, continuing to build and render the component doesn't make sense because a working component can't be rendered.</span></span>

<span data-ttu-id="09167-249">Pour tolérer les erreurs qui peuvent se produire pendant le prérendu, la logique de gestion des erreurs doit être placée à l’intérieur d’un composant qui peut lever des exceptions.</span><span class="sxs-lookup"><span data-stu-id="09167-249">To tolerate errors that may occur during prerendering, error handling logic must be placed inside a component that may throw exceptions.</span></span> <span data-ttu-id="09167-250">Utilisez les instructions [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) avec la gestion des erreurs et la journalisation.</span><span class="sxs-lookup"><span data-stu-id="09167-250">Use [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statements with error handling and logging.</span></span> <span data-ttu-id="09167-251">Au lieu d’encapsuler le tag Helper `Component` dans une instruction `try-catch`, placez la logique de gestion des erreurs dans le composant rendu par le tag Helper `Component`.</span><span class="sxs-lookup"><span data-stu-id="09167-251">Instead of wrapping the `Component` Tag Helper in a `try-catch` statement, place error handling logic in the component rendered by the `Component` Tag Helper.</span></span>

## <a name="advanced-scenarios"></a><span data-ttu-id="09167-252">Scénarios avancés</span><span class="sxs-lookup"><span data-stu-id="09167-252">Advanced scenarios</span></span>

### <a name="recursive-rendering"></a><span data-ttu-id="09167-253">Rendu récursif</span><span class="sxs-lookup"><span data-stu-id="09167-253">Recursive rendering</span></span>

<span data-ttu-id="09167-254">Les composants peuvent être imbriqués de manière récursive.</span><span class="sxs-lookup"><span data-stu-id="09167-254">Components can be nested recursively.</span></span> <span data-ttu-id="09167-255">Cela est utile pour représenter des structures de données récursives.</span><span class="sxs-lookup"><span data-stu-id="09167-255">This is useful for representing recursive data structures.</span></span> <span data-ttu-id="09167-256">Par exemple, un composant `TreeNode` peut restituer plus de composants `TreeNode` pour chacun des enfants du nœud.</span><span class="sxs-lookup"><span data-stu-id="09167-256">For example, a `TreeNode` component can render more `TreeNode` components for each of the node's children.</span></span>

<span data-ttu-id="09167-257">Lors du rendu de manière récursive, évitez les modèles de codage qui se traduisent par une récurrence infinie :</span><span class="sxs-lookup"><span data-stu-id="09167-257">When rendering recursively, avoid coding patterns that result in infinite recursion:</span></span>

* <span data-ttu-id="09167-258">Ne rendez pas de manière récursive une structure de données qui contient un cycle.</span><span class="sxs-lookup"><span data-stu-id="09167-258">Don't recursively render a data structure that contains a cycle.</span></span> <span data-ttu-id="09167-259">Par exemple, n’affichez pas un nœud d’arbre dont les enfants s’y trouvent.</span><span class="sxs-lookup"><span data-stu-id="09167-259">For example, don't render a tree node whose children includes itself.</span></span>
* <span data-ttu-id="09167-260">Ne créez pas une chaîne de dispositions qui contiennent un cycle.</span><span class="sxs-lookup"><span data-stu-id="09167-260">Don't create a chain of layouts that contain a cycle.</span></span> <span data-ttu-id="09167-261">Par exemple, ne créez pas une disposition dont la disposition est elle-même.</span><span class="sxs-lookup"><span data-stu-id="09167-261">For example, don't create a layout whose layout is itself.</span></span>
* <span data-ttu-id="09167-262">N’autorisez pas un utilisateur final à enfreindre les invariants de récurrence (règles) par le biais d’une entrée de données malveillante ou d’appels Interop JavaScript.</span><span class="sxs-lookup"><span data-stu-id="09167-262">Don't allow an end user to violate recursion invariants (rules) through malicious data entry or JavaScript interop calls.</span></span>

<span data-ttu-id="09167-263">Boucles infinies pendant le rendu :</span><span class="sxs-lookup"><span data-stu-id="09167-263">Infinite loops during rendering:</span></span>

* <span data-ttu-id="09167-264">Fait en sorte que le processus de rendu continue de façon permanente.</span><span class="sxs-lookup"><span data-stu-id="09167-264">Causes the rendering process to continue forever.</span></span>
* <span data-ttu-id="09167-265">Équivaut à créer une boucle non terminée.</span><span class="sxs-lookup"><span data-stu-id="09167-265">Is equivalent to creating an unterminated loop.</span></span>

<span data-ttu-id="09167-266">Dans ces scénarios, un circuit Blazor Server affecté échoue et le thread tente généralement d’effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="09167-266">In these scenarios, an affected Blazor Server circuit fails, and the thread usually attempts to:</span></span>

* <span data-ttu-id="09167-267">Consommez le plus de temps processeur autorisé par le système d’exploitation, indéfiniment.</span><span class="sxs-lookup"><span data-stu-id="09167-267">Consume as much CPU time as permitted by the operating system, indefinitely.</span></span>
* <span data-ttu-id="09167-268">Consommez une quantité illimitée de mémoire serveur.</span><span class="sxs-lookup"><span data-stu-id="09167-268">Consume an unlimited amount of server memory.</span></span> <span data-ttu-id="09167-269">La consommation de mémoire illimitée est équivalente au scénario dans lequel une boucle non terminée ajoute des entrées à une collection à chaque itération.</span><span class="sxs-lookup"><span data-stu-id="09167-269">Consuming unlimited memory is equivalent to the scenario where an unterminated loop adds entries to a collection on every iteration.</span></span>

<span data-ttu-id="09167-270">Pour éviter les modèles de récurrence infinis, assurez-vous que le code de rendu récursif contient des conditions d’arrêt appropriées.</span><span class="sxs-lookup"><span data-stu-id="09167-270">To avoid infinite recursion patterns, ensure that recursive rendering code contains suitable stopping conditions.</span></span>

### <a name="custom-render-tree-logic"></a><span data-ttu-id="09167-271">Logique d’arborescence de rendu personnalisé</span><span class="sxs-lookup"><span data-stu-id="09167-271">Custom render tree logic</span></span>

<span data-ttu-id="09167-272">La plupart des composants Blazor sont implémentés en tant que fichiers *. Razor* et sont compilés pour produire une logique qui fonctionne sur un `RenderTreeBuilder` pour restituer leur sortie.</span><span class="sxs-lookup"><span data-stu-id="09167-272">Most Blazor components are implemented as *.razor* files and are compiled to produce logic that operates on a `RenderTreeBuilder` to render their output.</span></span> <span data-ttu-id="09167-273">Un développeur peut implémenter manuellement `RenderTreeBuilder` logique à C# l’aide du code procédural.</span><span class="sxs-lookup"><span data-stu-id="09167-273">A developer may manually implement `RenderTreeBuilder` logic using procedural C# code.</span></span> <span data-ttu-id="09167-274">Pour plus d’informations, consultez <xref:blazor/advanced-scenarios#manual-rendertreebuilder-logic>.</span><span class="sxs-lookup"><span data-stu-id="09167-274">For more information, see <xref:blazor/advanced-scenarios#manual-rendertreebuilder-logic>.</span></span>

> [!WARNING]
> <span data-ttu-id="09167-275">L’utilisation de la logique du générateur d’arborescence de rendu manuel est considérée comme un scénario avancé et risqué, non recommandé pour le développement de composants généraux.</span><span class="sxs-lookup"><span data-stu-id="09167-275">Use of manual render tree builder logic is considered an advanced and unsafe scenario, not recommended for general component development.</span></span>

<span data-ttu-id="09167-276">Si `RenderTreeBuilder` code est écrit, le développeur doit garantir l’exactitude du code.</span><span class="sxs-lookup"><span data-stu-id="09167-276">If `RenderTreeBuilder` code is written, the developer must guarantee the correctness of the code.</span></span> <span data-ttu-id="09167-277">Par exemple, le développeur doit s’assurer que :</span><span class="sxs-lookup"><span data-stu-id="09167-277">For example, the developer must ensure that:</span></span>

* <span data-ttu-id="09167-278">Les appels à `OpenElement` et `CloseElement` sont correctement équilibrés.</span><span class="sxs-lookup"><span data-stu-id="09167-278">Calls to `OpenElement` and `CloseElement` are correctly balanced.</span></span>
* <span data-ttu-id="09167-279">Les attributs sont ajoutés uniquement aux emplacements appropriés.</span><span class="sxs-lookup"><span data-stu-id="09167-279">Attributes are only added in the correct places.</span></span>

<span data-ttu-id="09167-280">Une logique incorrecte du générateur d’arborescence de rendu manuel peut entraîner un comportement arbitraire non défini, y compris des pannes, des blocages du serveur et des failles de sécurité.</span><span class="sxs-lookup"><span data-stu-id="09167-280">Incorrect manual render tree builder logic can cause arbitrary undefined behavior, including crashes, server hangs, and security vulnerabilities.</span></span>

<span data-ttu-id="09167-281">Envisagez une logique de générateur d’arborescence de rendu manuel sur le même niveau de complexité et avec le même niveau de *danger* que l’écriture de code assembleur ou d’instructions MSIL à la main.</span><span class="sxs-lookup"><span data-stu-id="09167-281">Consider manual render tree builder logic on the same level of complexity and with the same level of *danger* as writing assembly code or MSIL instructions by hand.</span></span>
