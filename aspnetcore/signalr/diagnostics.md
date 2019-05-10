---
title: Journalisation et diagnostics dans ASP.NET Core SignalR
author: anurse
description: Découvrez comment recueillir des diagnostics à partir de votre application ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: signalr
ms.date: 02/27/2019
uid: signalr/diagnostics
ms.openlocfilehash: b6bd21314ed183488999bcff3553e53493537a11
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896886"
---
# <a name="logging-and-diagnostics-in-aspnet-core-signalr"></a><span data-ttu-id="024cf-103">Journalisation et diagnostics dans ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="024cf-103">Logging and diagnostics in ASP.NET Core SignalR</span></span>

<span data-ttu-id="024cf-104">Par [Andrew Stanton-Nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="024cf-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="024cf-105">Cet article fournit des conseils pour la collecte des diagnostics à partir de votre application ASP.NET Core SignalR pour aider à résoudre les problèmes.</span><span class="sxs-lookup"><span data-stu-id="024cf-105">This article provides guidance for gathering diagnostics from your ASP.NET Core SignalR app to help troubleshoot issues.</span></span>

## <a name="server-side-logging"></a><span data-ttu-id="024cf-106">Journalisation côté serveur</span><span class="sxs-lookup"><span data-stu-id="024cf-106">Server-side logging</span></span>

> [!WARNING]
> <span data-ttu-id="024cf-107">Journaux côté serveur peuvent contenir des informations sensibles de votre application.</span><span class="sxs-lookup"><span data-stu-id="024cf-107">Server-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="024cf-108">**Jamais** publier des journaux bruts à partir d’applications de production dans les forums publics comme GitHub.</span><span class="sxs-lookup"><span data-stu-id="024cf-108">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="024cf-109">Étant donné que SignalR fait partie d’ASP.NET Core, il utilise le système de journalisation ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="024cf-109">Since SignalR is part of ASP.NET Core, it uses the ASP.NET Core logging system.</span></span> <span data-ttu-id="024cf-110">Dans la configuration par défaut, les journaux de SignalR très peu d’informations, mais cela peut configurés.</span><span class="sxs-lookup"><span data-stu-id="024cf-110">In the default configuration, SignalR logs very little information, but this can configured.</span></span> <span data-ttu-id="024cf-111">Consultez la documentation sur [journalisation ASP.NET Core](xref:fundamentals/logging/index#configuration) pour plus d’informations sur la configuration de journalisation ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="024cf-111">See the documentation on [ASP.NET Core logging](xref:fundamentals/logging/index#configuration) for details on configuring ASP.NET Core logging.</span></span>

<span data-ttu-id="024cf-112">SignalR utilise deux catégories d’enregistreur d’événements :</span><span class="sxs-lookup"><span data-stu-id="024cf-112">SignalR uses two logger categories:</span></span>

* <span data-ttu-id="024cf-113">`Microsoft.AspNetCore.SignalR` -pour les journaux liés aux protocoles de concentrateur, activation des Hubs, l’appel de méthodes et autres activités du Hub.</span><span class="sxs-lookup"><span data-stu-id="024cf-113">`Microsoft.AspNetCore.SignalR` - for logs related to Hub Protocols, activating Hubs, invoking methods, and other Hub-related activities.</span></span>
* <span data-ttu-id="024cf-114">`Microsoft.AspNetCore.Http.Connections` -pour les journaux liés aux transports, tels que les WebSockets, d’interrogation longue et les événements et de bas niveau infrastructure SignalR.</span><span class="sxs-lookup"><span data-stu-id="024cf-114">`Microsoft.AspNetCore.Http.Connections` - for logs related to transports such as WebSockets, Long Polling and Server-Sent Events and low-level SignalR infrastructure.</span></span>

<span data-ttu-id="024cf-115">Pour activer les journaux détaillés à partir de SignalR, configurez les deux préfixes précédents à la `Debug` niveau dans votre `appsettings.json` fichier en ajoutant les éléments suivants dans le `LogLevel` sous-section dans `Logging`:</span><span class="sxs-lookup"><span data-stu-id="024cf-115">To enable detailed logs from SignalR, configure both of the preceding prefixes to the `Debug` level in your `appsettings.json` file by adding the following items to the `LogLevel` sub-section in `Logging`:</span></span>

[!code-json[Configuring logging](diagnostics/logging-config.json?highlight=7-8)]

<span data-ttu-id="024cf-116">Vous pouvez aussi configurer cela dans le code dans votre `CreateWebHostBuilder` méthode :</span><span class="sxs-lookup"><span data-stu-id="024cf-116">You can also configure this in code in your `CreateWebHostBuilder` method:</span></span>

[!code-csharp[Configuring logging in code](diagnostics/logging-config-code.cs?highlight=5-6)]

<span data-ttu-id="024cf-117">Si vous n’utilisez pas configuration basée sur JSON, définissez les valeurs de configuration suivantes dans votre système de configuration :</span><span class="sxs-lookup"><span data-stu-id="024cf-117">If you aren't using JSON-based configuration, set the following configuration values in your configuration system:</span></span>

* `Logging:LogLevel:Microsoft.AspNetCore.SignalR` = `Debug`
* `Logging:LogLevel:Microsoft.AspNetCore.Http.Connections` = `Debug`

<span data-ttu-id="024cf-118">Consultez la documentation de votre système de configuration déterminer comment spécifier des valeurs de configuration imbriquée.</span><span class="sxs-lookup"><span data-stu-id="024cf-118">Check the documentation for your configuration system to determine how to specify nested configuration values.</span></span> <span data-ttu-id="024cf-119">Par exemple, lors de l’utilisation de variables d’environnement, deux `_` caractères sont utilisés à la place de la `:` (par exemple : `Logging__LogLevel__Microsoft.AspNetCore.SignalR`).</span><span class="sxs-lookup"><span data-stu-id="024cf-119">For example, when using environment variables, two `_` characters are used instead of the `:` (such as: `Logging__LogLevel__Microsoft.AspNetCore.SignalR`).</span></span>

<span data-ttu-id="024cf-120">Nous vous recommandons d’utiliser le `Debug` niveau lors de la collecte plus de diagnostics pour votre application.</span><span class="sxs-lookup"><span data-stu-id="024cf-120">We recommend using the `Debug` level when gathering more detailed diagnostics for your app.</span></span> <span data-ttu-id="024cf-121">Le `Trace` au niveau du produit les diagnostics de très bas niveau et est rarement nécessaire pour diagnostiquer les problèmes dans votre application.</span><span class="sxs-lookup"><span data-stu-id="024cf-121">The `Trace` level produces very low-level diagnostics and is rarely needed to diagnose issues in your app.</span></span>

## <a name="access-server-side-logs"></a><span data-ttu-id="024cf-122">Accéder aux journaux du côté serveur</span><span class="sxs-lookup"><span data-stu-id="024cf-122">Access server-side logs</span></span>

<span data-ttu-id="024cf-123">Mode d’accès aux journaux côté serveur dépend de l’environnement dans lequel vous êtes en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="024cf-123">How you access server-side logs depends on the environment in which you're running.</span></span>

### <a name="as-a-console-app-outside-iis"></a><span data-ttu-id="024cf-124">Une application console en dehors d’IIS</span><span class="sxs-lookup"><span data-stu-id="024cf-124">As a console app outside IIS</span></span>

<span data-ttu-id="024cf-125">Si vous utilisez dans une application console, le [journal de Console](xref:fundamentals/logging/index#console-provider) doit être activée par défaut.</span><span class="sxs-lookup"><span data-stu-id="024cf-125">If you're running in a console app, the [Console logger](xref:fundamentals/logging/index#console-provider) should be enabled by default.</span></span> <span data-ttu-id="024cf-126">Journaux de SignalR seront affiche dans la console.</span><span class="sxs-lookup"><span data-stu-id="024cf-126">SignalR logs will appear in the console.</span></span>

### <a name="within-iis-express-from-visual-studio"></a><span data-ttu-id="024cf-127">Dans IIS Express à partir de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="024cf-127">Within IIS Express from Visual Studio</span></span>

<span data-ttu-id="024cf-128">Visual Studio affiche la sortie du journal dans le **sortie** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="024cf-128">Visual Studio displays the log output in the **Output** window.</span></span> <span data-ttu-id="024cf-129">Sélectionnez le **ASP.NET Core Web Server** option de liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="024cf-129">Select the **ASP.NET Core Web Server** drop down option.</span></span>

### <a name="azure-app-service"></a><span data-ttu-id="024cf-130">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="024cf-130">Azure App Service</span></span>

<span data-ttu-id="024cf-131">Activez l’option « Journal des applications (Filesystem) » dans la section « Journaux de Diagnostics » du portail Azure App Service et configurer le niveau de `Verbose`.</span><span class="sxs-lookup"><span data-stu-id="024cf-131">Enable the "Application Logging (Filesystem)" option in the "Diagnostics logs" section of the Azure App Service portal and configure the Level to `Verbose`.</span></span> <span data-ttu-id="024cf-132">Journaux doivent être disponibles à partir du service « Diffusion », ainsi que dans les journaux sur le système de fichiers de votre application de Service.</span><span class="sxs-lookup"><span data-stu-id="024cf-132">Logs should be available from the "Log streaming" service, as well as in logs on the file system of your App Service.</span></span> <span data-ttu-id="024cf-133">Pour plus d’informations, consultez la documentation sur [streaming des journaux Azure](xref:fundamentals/logging/index#azure-log-streaming).</span><span class="sxs-lookup"><span data-stu-id="024cf-133">For more information, see the documentation on [Azure log streaming](xref:fundamentals/logging/index#azure-log-streaming).</span></span>

### <a name="other-environments"></a><span data-ttu-id="024cf-134">Autres environnements</span><span class="sxs-lookup"><span data-stu-id="024cf-134">Other environments</span></span>

<span data-ttu-id="024cf-135">Si vous exécutez dans un autre environnement (Docker, Kubernetes, Service de Windows, etc.), consultez la documentation complète sur [journalisation ASP.NET Core](xref:fundamentals/logging/index) pour plus d’informations sur la configuration des fournisseurs de journalisation adaptés à votre environnement.</span><span class="sxs-lookup"><span data-stu-id="024cf-135">If you're running in another environment (Docker, Kubernetes, Windows Service, etc.), see the full documentation on [ASP.NET Core Logging](xref:fundamentals/logging/index) for more information on how to configure logging providers suitable to your environment.</span></span>

## <a name="javascript-client-logging"></a><span data-ttu-id="024cf-136">Enregistrement du client JavaScript</span><span class="sxs-lookup"><span data-stu-id="024cf-136">JavaScript client logging</span></span>

> [!WARNING]
> <span data-ttu-id="024cf-137">Journaux côté client peuvent contenir des informations sensibles de votre application.</span><span class="sxs-lookup"><span data-stu-id="024cf-137">Client-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="024cf-138">**Jamais** publier des journaux bruts à partir d’applications de production dans les forums publics comme GitHub.</span><span class="sxs-lookup"><span data-stu-id="024cf-138">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="024cf-139">Lorsque vous utilisez le client JavaScript, vous pouvez configurer les options de journalisation à l’aide de la `configureLogging` méthode sur `HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="024cf-139">When using the JavaScript client, you can configure logging options using the `configureLogging` method on `HubConnectionBuilder`:</span></span>

[!code-javascript[Configuring logging in the JavaScript client](diagnostics/logging-config-js.js?highlight=3)]

<span data-ttu-id="024cf-140">Pour désactiver entièrement la journalisation, spécifiez `signalR.LogLevel.None` dans la méthode `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="024cf-140">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="024cf-141">Le tableau suivant présente les niveaux de journalisation disponibles pour le client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="024cf-141">The following table shows log levels available to the JavaScript client.</span></span> <span data-ttu-id="024cf-142">Une des valeurs suivantes affectant le niveau de journal Active la journalisation à ce niveau et tous les niveaux au-dessus de lui dans la table.</span><span class="sxs-lookup"><span data-stu-id="024cf-142">Setting the log level to one of these values enables logging at that level and all levels above it in the table.</span></span>

| <span data-ttu-id="024cf-143">Niveau</span><span class="sxs-lookup"><span data-stu-id="024cf-143">Level</span></span> | <span data-ttu-id="024cf-144">Description</span><span class="sxs-lookup"><span data-stu-id="024cf-144">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="024cf-145">Aucun message n’est enregistré.</span><span class="sxs-lookup"><span data-stu-id="024cf-145">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="024cf-146">Messages qui indiquent un échec dans l’application entière.</span><span class="sxs-lookup"><span data-stu-id="024cf-146">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="024cf-147">Messages qui indiquent un échec dans l’opération en cours.</span><span class="sxs-lookup"><span data-stu-id="024cf-147">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="024cf-148">Messages qui indiquent un problème non irrécupérable.</span><span class="sxs-lookup"><span data-stu-id="024cf-148">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="024cf-149">Messages d’information.</span><span class="sxs-lookup"><span data-stu-id="024cf-149">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="024cf-150">Messages de diagnostic utiles pour le débogage.</span><span class="sxs-lookup"><span data-stu-id="024cf-150">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="024cf-151">Messages de diagnostic très détaillés conçus pour diagnostiquer les problèmes spécifiques.</span><span class="sxs-lookup"><span data-stu-id="024cf-151">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

<span data-ttu-id="024cf-152">Une fois que vous avez configuré le niveau de détail, les journaux sont écrits à la Console du navigateur (ou de la sortie Standard dans une application NodeJS).</span><span class="sxs-lookup"><span data-stu-id="024cf-152">Once you've configured the verbosity, the logs will be written to the Browser Console (or Standard Output in a NodeJS app).</span></span>

<span data-ttu-id="024cf-153">Si vous souhaitez envoyer des journaux à un système de journalisation personnalisée, vous pouvez fournir un objet JavaScript qui implémente le `ILogger` interface.</span><span class="sxs-lookup"><span data-stu-id="024cf-153">If you want to send logs to a custom logging system, you can provide a JavaScript object implementing the `ILogger` interface.</span></span> <span data-ttu-id="024cf-154">La seule méthode qui doit être implémentée est `log`, ce qui amène le niveau de l’événement et le message associé à l’événement.</span><span class="sxs-lookup"><span data-stu-id="024cf-154">The only method that needs to be implemented is `log`, which takes the level of the event and the message associated with the event.</span></span> <span data-ttu-id="024cf-155">Exemple :</span><span class="sxs-lookup"><span data-stu-id="024cf-155">For example:</span></span>

[!code-typescript[Creating a custom logger](diagnostics/custom-logger.ts?highlight=3-7,13)]

## <a name="net-client-logging"></a><span data-ttu-id="024cf-156">Enregistrement du client .NET</span><span class="sxs-lookup"><span data-stu-id="024cf-156">.NET client logging</span></span>

> [!WARNING]
> <span data-ttu-id="024cf-157">Journaux côté client peuvent contenir des informations sensibles de votre application.</span><span class="sxs-lookup"><span data-stu-id="024cf-157">Client-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="024cf-158">**Jamais** publier des journaux bruts à partir d’applications de production dans les forums publics comme GitHub.</span><span class="sxs-lookup"><span data-stu-id="024cf-158">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="024cf-159">Pour obtenir des journaux à partir du client .NET, vous pouvez utiliser la `ConfigureLogging` méthode sur `HubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="024cf-159">To get logs from the .NET client, you can use the `ConfigureLogging` method on `HubConnectionBuilder`.</span></span> <span data-ttu-id="024cf-160">Cela fonctionne de la même façon que le `ConfigureLogging` méthode sur `WebHostBuilder` et `HostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="024cf-160">This works the same way as the `ConfigureLogging` method on `WebHostBuilder` and `HostBuilder`.</span></span> <span data-ttu-id="024cf-161">Vous pouvez configurer les mêmes fournisseurs de journalisation que vous utilisez dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="024cf-161">You can configure the same logging providers you use in ASP.NET Core.</span></span> <span data-ttu-id="024cf-162">Toutefois, vous devez installer et activer les packages NuGet pour les fournisseurs de journalisation individuelles manuellement.</span><span class="sxs-lookup"><span data-stu-id="024cf-162">However, you have to manually install and enable the NuGet packages for the individual logging providers.</span></span>

### <a name="console-logging"></a><span data-ttu-id="024cf-163">Journalisation de console</span><span class="sxs-lookup"><span data-stu-id="024cf-163">Console logging</span></span>

<span data-ttu-id="024cf-164">Pour activer la journalisation de la Console, ajoutez le [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) package.</span><span class="sxs-lookup"><span data-stu-id="024cf-164">In order to enable Console logging, add the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) package.</span></span> <span data-ttu-id="024cf-165">Ensuite, utilisez le `AddConsole` méthode pour configurer le journal de console :</span><span class="sxs-lookup"><span data-stu-id="024cf-165">Then, use the `AddConsole` method to configure the console logger:</span></span>

[!code-csharp[Configuring console logging in .NET client](diagnostics/net-client-console-log.cs?highlight=6)]

### <a name="debug-output-window-logging"></a><span data-ttu-id="024cf-166">La journalisation de fenêtre de sortie du débogage</span><span class="sxs-lookup"><span data-stu-id="024cf-166">Debug output window logging</span></span>

<span data-ttu-id="024cf-167">Vous pouvez également configurer les journaux pour accéder à la **sortie** fenêtre dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="024cf-167">You can also configure logs to go to the **Output** window in Visual Studio.</span></span> <span data-ttu-id="024cf-168">Installer le [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) empaqueter et utiliser le `AddDebug` méthode :</span><span class="sxs-lookup"><span data-stu-id="024cf-168">Install the [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) package and use the `AddDebug` method:</span></span>

[!code-csharp[Configuring debug output window logging in .NET client](diagnostics/net-client-debug-log.cs?highlight=6)]

### <a name="other-logging-providers"></a><span data-ttu-id="024cf-169">Autres fournisseurs de journalisation</span><span class="sxs-lookup"><span data-stu-id="024cf-169">Other logging providers</span></span>

<span data-ttu-id="024cf-170">SignalR prend en charge d’autres fournisseurs de journalisation comme Serilog, Seq, NLog ou tout autre système de journalisation qui s’intègre à `Microsoft.Extensions.Logging`.</span><span class="sxs-lookup"><span data-stu-id="024cf-170">SignalR supports other logging providers such as Serilog, Seq, NLog, or any other logging system that integrates with `Microsoft.Extensions.Logging`.</span></span> <span data-ttu-id="024cf-171">Si votre système de journalisation fournit une `ILoggerProvider`, vous pouvez l’inscrire avec `AddProvider`:</span><span class="sxs-lookup"><span data-stu-id="024cf-171">If your logging system provides an `ILoggerProvider`, you can register it with `AddProvider`:</span></span>

[!code-csharp[Configuring a custom logging provider in .NET client](diagnostics/net-client-custom-log.cs?highlight=6)]

### <a name="control-verbosity"></a><span data-ttu-id="024cf-172">Contrôle le niveau de détail</span><span class="sxs-lookup"><span data-stu-id="024cf-172">Control verbosity</span></span>

<span data-ttu-id="024cf-173">Si vous vous connectez à partir d’autres emplacements dans votre application, la modification du niveau de la valeur par défaut pour `Debug` est peut-être trop détaillée.</span><span class="sxs-lookup"><span data-stu-id="024cf-173">If you are logging from other places in your app, changing the default level to `Debug` may be too verbose.</span></span> <span data-ttu-id="024cf-174">Vous pouvez utiliser un filtre pour configurer le niveau de journalisation pour les journaux de SignalR.</span><span class="sxs-lookup"><span data-stu-id="024cf-174">You can use a Filter to configure the logging level for SignalR logs.</span></span> <span data-ttu-id="024cf-175">Cela est possible dans le code, à peu près la même façon que sur le serveur :</span><span class="sxs-lookup"><span data-stu-id="024cf-175">This can be done in code, in much the same way as on the server:</span></span>

[!code-csharp[Controlling verbosity in .NET client](diagnostics/logging-config-client-code.cs?highlight=9-10)]

## <a name="network-traces"></a><span data-ttu-id="024cf-176">Suivis réseau</span><span class="sxs-lookup"><span data-stu-id="024cf-176">Network traces</span></span>

> [!WARNING]
> <span data-ttu-id="024cf-177">Une trace réseau contient le contenu complet de chaque message envoyé par votre application.</span><span class="sxs-lookup"><span data-stu-id="024cf-177">A network trace contains the full contents of every message sent by your app.</span></span> <span data-ttu-id="024cf-178">**Jamais** publier des traces réseau brutes à partir d’applications de production dans les forums publics comme GitHub.</span><span class="sxs-lookup"><span data-stu-id="024cf-178">**Never** post raw network traces from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="024cf-179">Si vous rencontrez un problème, une trace réseau peut parfois fournir beaucoup d’informations utiles.</span><span class="sxs-lookup"><span data-stu-id="024cf-179">If you encounter an issue, a network trace can sometimes provide a lot of helpful information.</span></span> <span data-ttu-id="024cf-180">Cela est particulièrement utile si vous voulez signaler un problème sur notre suivi des problèmes.</span><span class="sxs-lookup"><span data-stu-id="024cf-180">This is particularly useful if you're going to file an issue on our issue tracker.</span></span>

## <a name="collect-a-network-trace-with-fiddler-preferred-option"></a><span data-ttu-id="024cf-181">Recueillir une trace réseau avec Fiddler (option par défaut)</span><span class="sxs-lookup"><span data-stu-id="024cf-181">Collect a network trace with Fiddler (preferred option)</span></span>

<span data-ttu-id="024cf-182">Cette méthode fonctionne pour toutes les applications.</span><span class="sxs-lookup"><span data-stu-id="024cf-182">This method works for all apps.</span></span>

<span data-ttu-id="024cf-183">Fiddler est un outil très puissant pour la collecte des traces HTTP.</span><span class="sxs-lookup"><span data-stu-id="024cf-183">Fiddler is a very powerful tool for collecting HTTP traces.</span></span> <span data-ttu-id="024cf-184">Installez-le à partir de [telerik.com/fiddler](https://www.telerik.com/fiddler), lancez-le, puis exécutez votre application et reproduisez le problème.</span><span class="sxs-lookup"><span data-stu-id="024cf-184">Install it from [telerik.com/fiddler](https://www.telerik.com/fiddler), launch it, and then run your app and reproduce the issue.</span></span> <span data-ttu-id="024cf-185">Fiddler est disponible pour Windows, et il existe des versions bêta pour macOS et Linux.</span><span class="sxs-lookup"><span data-stu-id="024cf-185">Fiddler is available for Windows, and there are beta versions for macOS and Linux.</span></span>

<span data-ttu-id="024cf-186">Si vous vous connectez à l’aide de HTTPS, il existe quelques étapes supplémentaires pour vous assurer de que Fiddler peut déchiffrer le trafic HTTPS.</span><span class="sxs-lookup"><span data-stu-id="024cf-186">If you connect using HTTPS, there are some extra steps to ensure Fiddler can decrypt the HTTPS traffic.</span></span> <span data-ttu-id="024cf-187">Pour plus d’informations, consultez le [documentation Fiddler](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).</span><span class="sxs-lookup"><span data-stu-id="024cf-187">For more details, see the [Fiddler documentation](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).</span></span>

<span data-ttu-id="024cf-188">Une fois que vous avez collecté la trace, vous pouvez exporter la trace en choisissant **fichier** > **enregistrer** > **toutes les Sessions en cours...**  à partir de la barre de menus.</span><span class="sxs-lookup"><span data-stu-id="024cf-188">Once you've collected the trace, you can export the trace by choosing **File** > **Save** > **All Sessions...** from the menu bar.</span></span>

![Exportation de toutes les sessions à partir de Fiddler](diagnostics/fiddler-export.png)

## <a name="collect-a-network-trace-with-tcpdump-macos-and-linux-only"></a><span data-ttu-id="024cf-190">Recueillir une trace réseau avec tcpdump (Mac OS et Linux uniquement)</span><span class="sxs-lookup"><span data-stu-id="024cf-190">Collect a network trace with tcpdump (macOS and Linux only)</span></span>

<span data-ttu-id="024cf-191">Cette méthode fonctionne pour toutes les applications.</span><span class="sxs-lookup"><span data-stu-id="024cf-191">This method works for all apps.</span></span>

<span data-ttu-id="024cf-192">Vous pouvez collecter brutes suivis TCP à l’aide de tcpdump en exécutant la commande suivante à partir d’une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="024cf-192">You can collect raw TCP traces using tcpdump by running the following command from a command shell.</span></span> <span data-ttu-id="024cf-193">Vous devrez peut-être être `root` ou un préfixe de la commande avec `sudo` si vous obtenez une erreur d’autorisations :</span><span class="sxs-lookup"><span data-stu-id="024cf-193">You may need to be `root` or prefix the command with `sudo` if you get a permissions error:</span></span>

```console
tcpdump -i [interface] -w trace.pcap
```

<span data-ttu-id="024cf-194">Remplacez `[interface]` avec l’interface réseau que vous souhaitez capturer sur.</span><span class="sxs-lookup"><span data-stu-id="024cf-194">Replace `[interface]` with the network interface you wish to capture on.</span></span> <span data-ttu-id="024cf-195">En règle générale, il s’agit d’un élément comme `/dev/eth0` (pour votre interface Ethernet standard) ou `/dev/lo0` (pour le trafic de localhost).</span><span class="sxs-lookup"><span data-stu-id="024cf-195">Usually, this is something like `/dev/eth0` (for your standard Ethernet interface) or `/dev/lo0` (for localhost traffic).</span></span> <span data-ttu-id="024cf-196">Pour plus d’informations, consultez le `tcpdump` page man sur votre système hôte.</span><span class="sxs-lookup"><span data-stu-id="024cf-196">For more information, see the `tcpdump` man page on your host system.</span></span>

## <a name="collect-a-network-trace-in-the-browser"></a><span data-ttu-id="024cf-197">Collecter un suivi réseau dans le navigateur</span><span class="sxs-lookup"><span data-stu-id="024cf-197">Collect a network trace in the browser</span></span>

<span data-ttu-id="024cf-198">Cette méthode fonctionne uniquement pour les applications basées sur le navigateur.</span><span class="sxs-lookup"><span data-stu-id="024cf-198">This method only works for browser-based apps.</span></span>

<span data-ttu-id="024cf-199">La plupart des outils de développement de navigateur ont un onglet « Network » qui vous permet de capturer l’activité réseau entre le navigateur et le serveur.</span><span class="sxs-lookup"><span data-stu-id="024cf-199">Most browser Developer Tools have a "Network" tab that allows you to capture network activity between the browser and the server.</span></span> <span data-ttu-id="024cf-200">Toutefois, ces suivis n’incluent pas les messages WebSocket et les événements.</span><span class="sxs-lookup"><span data-stu-id="024cf-200">However, these traces don't include WebSocket and Server-Sent Event messages.</span></span> <span data-ttu-id="024cf-201">Si vous utilisez ces transports, à l’aide d’un outil comme Fiddler ou TcpDump (décrits ci-dessous) est une meilleure approche.</span><span class="sxs-lookup"><span data-stu-id="024cf-201">If you are using those transports, using a tool like Fiddler or TcpDump (described below) is a better approach.</span></span>

### <a name="microsoft-edge-and-internet-explorer"></a><span data-ttu-id="024cf-202">Microsoft Edge et Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="024cf-202">Microsoft Edge and Internet Explorer</span></span>

<span data-ttu-id="024cf-203">(Les instructions sont les mêmes pour Edge et Internet Explorer)</span><span class="sxs-lookup"><span data-stu-id="024cf-203">(The instructions are the same for both Edge and Internet Explorer)</span></span>

1. <span data-ttu-id="024cf-204">Appuyez sur F12 pour ouvrir les outils de développement</span><span class="sxs-lookup"><span data-stu-id="024cf-204">Press F12 to open the Dev Tools</span></span>
2. <span data-ttu-id="024cf-205">Cliquez sur l’onglet réseau</span><span class="sxs-lookup"><span data-stu-id="024cf-205">Click the Network Tab</span></span>
3. <span data-ttu-id="024cf-206">Actualisez la page (si nécessaire) et de reproduire le problème</span><span class="sxs-lookup"><span data-stu-id="024cf-206">Refresh the page (if needed) and reproduce the problem</span></span>
4. <span data-ttu-id="024cf-207">Cliquez sur l’icône Enregistrer dans la barre d’outils pour exporter la trace dans un fichier « HAR » :</span><span class="sxs-lookup"><span data-stu-id="024cf-207">Click the Save icon in the toolbar to export the trace as a "HAR" file:</span></span>

![L’enregistrement onglet réseau des outils icône sur le développement de Microsoft Edge](diagnostics/ie-edge-har-export.png)

### <a name="google-chrome"></a><span data-ttu-id="024cf-209">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="024cf-209">Google Chrome</span></span>

1. <span data-ttu-id="024cf-210">Appuyez sur F12 pour ouvrir les outils de développement</span><span class="sxs-lookup"><span data-stu-id="024cf-210">Press F12 to open the Dev Tools</span></span>
2. <span data-ttu-id="024cf-211">Cliquez sur l’onglet réseau</span><span class="sxs-lookup"><span data-stu-id="024cf-211">Click the Network Tab</span></span>
3. <span data-ttu-id="024cf-212">Actualisez la page (si nécessaire) et de reproduire le problème</span><span class="sxs-lookup"><span data-stu-id="024cf-212">Refresh the page (if needed) and reproduce the problem</span></span>
4. <span data-ttu-id="024cf-213">Avec le bouton droit sur n’importe où dans la liste de demandes et choisissez « Enregistrer en tant que HAR avec du contenu » :</span><span class="sxs-lookup"><span data-stu-id="024cf-213">Right click anywhere in the list of requests and choose "Save as HAR with content":</span></span>

![Option de « Enregistrer en tant que HAR avec du contenu » dans l’onglet réseau des outils de développement Google Chrome](diagnostics/chrome-har-export.png)

### <a name="mozilla-firefox"></a><span data-ttu-id="024cf-215">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="024cf-215">Mozilla Firefox</span></span>

1. <span data-ttu-id="024cf-216">Appuyez sur F12 pour ouvrir les outils de développement</span><span class="sxs-lookup"><span data-stu-id="024cf-216">Press F12 to open the Dev Tools</span></span>
2. <span data-ttu-id="024cf-217">Cliquez sur l’onglet réseau</span><span class="sxs-lookup"><span data-stu-id="024cf-217">Click the Network Tab</span></span>
3. <span data-ttu-id="024cf-218">Actualisez la page (si nécessaire) et de reproduire le problème</span><span class="sxs-lookup"><span data-stu-id="024cf-218">Refresh the page (if needed) and reproduce the problem</span></span>
4. <span data-ttu-id="024cf-219">Avec le bouton droit sur n’importe où dans la liste de demandes et choisissez « Enregistrer toutes en tant que HAR »</span><span class="sxs-lookup"><span data-stu-id="024cf-219">Right click anywhere in the list of requests and choose "Save All As HAR"</span></span>

![Option « Enregistrer tout en tant que HAR » dans l’onglet réseau des outils de développement Mozilla Firefox](diagnostics/firefox-har-export.png)

## <a name="attach-diagnostics-files-to-github-issues"></a><span data-ttu-id="024cf-221">Joindre des fichiers de diagnostic vers les problèmes GitHub</span><span class="sxs-lookup"><span data-stu-id="024cf-221">Attach diagnostics files to GitHub issues</span></span>

<span data-ttu-id="024cf-222">Vous pouvez joindre des fichiers de diagnostic vers les problèmes GitHub en les renommant afin qu’ils aient un `.txt` extension puis en faisant glisser et en les déposant sur le problème.</span><span class="sxs-lookup"><span data-stu-id="024cf-222">You can attach Diagnostics files to GitHub issues by renaming them so they have a `.txt` extension and then dragging and dropping them on to the issue.</span></span>

> [!NOTE]
> <span data-ttu-id="024cf-223">Veuillez ne collez le contenu des fichiers journaux ou des traces réseau problème GitHub.</span><span class="sxs-lookup"><span data-stu-id="024cf-223">Please don't paste the content of log files or network traces in GitHub issue.</span></span> <span data-ttu-id="024cf-224">Ces journaux et les traces peuvent être très volumineux et GitHub est généralement les tronquer.</span><span class="sxs-lookup"><span data-stu-id="024cf-224">These logs and traces can be quite large and GitHub will usually truncate them.</span></span>

![En faisant glisser des fichiers journaux à un problème GitHub](diagnostics/attaching-diagnostics-files.png)

## <a name="additional-resources"></a><span data-ttu-id="024cf-226">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="024cf-226">Additional resources</span></span>

* <xref:signalr/configuration>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
