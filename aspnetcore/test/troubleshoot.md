---
title: Résoudre les problèmes liés aux projets ASP.NET Core
author: Rick-Anderson
description: Comprenez et résolvez les avertissements et les erreurs avec les projets ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/10/2019
uid: test/troubleshoot
ms.openlocfilehash: b434af2dd046045836d2f6f7f7b7b2d57699bedc
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308278"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="083f5-103">Résoudre les problèmes liés aux projets ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="083f5-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="083f5-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="083f5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="083f5-105">Les liens suivants fournissent des conseils de dépannage:</span><span class="sxs-lookup"><span data-stu-id="083f5-105">The following links provide troubleshooting guidance:</span></span>

* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="083f5-106">Conférence norvégiens (Londres, 2018): Diagnostic des problèmes dans les applications ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="083f5-106">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="083f5-107">Blog ASP.NET: Résolution des problèmes de performances de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="083f5-107">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="083f5-108">Avertissements de kit SDK .NET Core</span><span class="sxs-lookup"><span data-stu-id="083f5-108">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="083f5-109">Les versions 32 bits et 64 bits de la kit SDK .NET Core sont installées</span><span class="sxs-lookup"><span data-stu-id="083f5-109">Both the 32-bit and 64-bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="083f5-110">Dans la boîte de dialogue **nouveau projet** pour ASP.net Core, l’avertissement suivant peut s’afficher:</span><span class="sxs-lookup"><span data-stu-id="083f5-110">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="083f5-111">Les versions 32 bits et 64 bits de la kit SDK .NET Core sont installées.</span><span class="sxs-lookup"><span data-stu-id="083f5-111">Both 32-bit and 64-bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="083f5-112">Seuls les modèles des versions 64 bits installées à l’adresse «\\C:\\Program\\Files\\dotnet SDK» s’affichent.</span><span class="sxs-lookup"><span data-stu-id="083f5-112">Only templates from the 64-bit versions installed at 'C:\\Program Files\\dotnet\\sdk\\' are displayed.</span></span>

<span data-ttu-id="083f5-113">Cet avertissement s’affiche lorsque les versions 32 bits (x86) et 64 bits (x64) du [Kit SDK .net Core](https://www.microsoft.com/net/download/all) sont installées.</span><span class="sxs-lookup"><span data-stu-id="083f5-113">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="083f5-114">Les raisons courantes pour lesquelles les deux versions peuvent être installées sont les suivantes:</span><span class="sxs-lookup"><span data-stu-id="083f5-114">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="083f5-115">À l’origine, vous avez téléchargé le programme d’installation de kit SDK .NET Core à l’aide d’un ordinateur 32 bits, puis vous l’avez copié et installé sur un ordinateur 64 bits.</span><span class="sxs-lookup"><span data-stu-id="083f5-115">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="083f5-116">Le kit SDK .NET Core 32 bits a été installé par une autre application.</span><span class="sxs-lookup"><span data-stu-id="083f5-116">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="083f5-117">La version incorrecte a été téléchargée et installée.</span><span class="sxs-lookup"><span data-stu-id="083f5-117">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="083f5-118">Désinstallez le kit SDK .NET Core 32 bits pour éviter cet avertissement.</span><span class="sxs-lookup"><span data-stu-id="083f5-118">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="083f5-119">Désinstaller dans **le panneau de configuration** > **programmes et fonctionnalités** > **désinstaller ou modifier un programme**.</span><span class="sxs-lookup"><span data-stu-id="083f5-119">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="083f5-120">Si vous comprenez la raison pour laquelle l’avertissement se produit et ses implications, vous pouvez ignorer l’avertissement.</span><span class="sxs-lookup"><span data-stu-id="083f5-120">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="083f5-121">Le kit SDK .NET Core est installé à plusieurs emplacements</span><span class="sxs-lookup"><span data-stu-id="083f5-121">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="083f5-122">Dans la boîte de dialogue **nouveau projet** pour ASP.net Core, l’avertissement suivant peut s’afficher:</span><span class="sxs-lookup"><span data-stu-id="083f5-122">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="083f5-123">Le kit SDK .NET Core est installé à plusieurs emplacements.</span><span class="sxs-lookup"><span data-stu-id="083f5-123">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="083f5-124">Seuls les modèles des kits de développement logiciel (SDK\\) installés à\\l'\\adresse «C: Program Files\\dotnet SDK» s’affichent.</span><span class="sxs-lookup"><span data-stu-id="083f5-124">Only templates from the SDKs installed at 'C:\\Program Files\\dotnet\\sdk\\' are displayed.</span></span>

<span data-ttu-id="083f5-125">Ce message s’affiche lorsque vous avez au moins une installation du kit SDK .net core dans un répertoire en dehors de *C:\\Program Files\\dotnet\\SDK\\* .</span><span class="sxs-lookup"><span data-stu-id="083f5-125">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="083f5-126">Cela se produit généralement lorsque le kit SDK .NET Core a été déployé sur un ordinateur à l’aide de la fonction copier/coller au lieu du programme d’installation MSI.</span><span class="sxs-lookup"><span data-stu-id="083f5-126">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="083f5-127">Désinstallez tous les kits de développement logiciel (SDK) .NET Core 32 bits et les runtimes pour éviter cet avertissement.</span><span class="sxs-lookup"><span data-stu-id="083f5-127">Uninstall all 32-bit .NET Core SDKs and runtimes to prevent this warning.</span></span> <span data-ttu-id="083f5-128">Désinstaller dans **le panneau de configuration** > **programmes et fonctionnalités** > **désinstaller ou modifier un programme**.</span><span class="sxs-lookup"><span data-stu-id="083f5-128">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="083f5-129">Si vous comprenez la raison pour laquelle l’avertissement se produit et ses implications, vous pouvez ignorer l’avertissement.</span><span class="sxs-lookup"><span data-stu-id="083f5-129">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="083f5-130">Aucun SDK .NET Core n’a été détecté</span><span class="sxs-lookup"><span data-stu-id="083f5-130">No .NET Core SDKs were detected</span></span>

* <span data-ttu-id="083f5-131">Dans la boîte de dialogue **nouveau projet** de Visual Studio pour ASP.net Core, l’avertissement suivant peut s’afficher:</span><span class="sxs-lookup"><span data-stu-id="083f5-131">In the Visual Studio **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

  > <span data-ttu-id="083f5-132">Aucun kit de développement logiciel (SDK) .NET Core n’a été détecté, `PATH`Vérifiez qu’ils sont inclus dans la variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="083f5-132">No .NET Core SDKs were detected, ensure they are included in the environment variable `PATH`.</span></span>

* <span data-ttu-id="083f5-133">Lors de l’exécution `dotnet` d’une commande, l’avertissement s’affiche comme suit:</span><span class="sxs-lookup"><span data-stu-id="083f5-133">When executing a `dotnet` command, the warning appears as:</span></span>

  > <span data-ttu-id="083f5-134">Il n’était pas possible de trouver les kits de développement logiciel (SDK) dotnet installés.</span><span class="sxs-lookup"><span data-stu-id="083f5-134">It was not possible to find any installed dotnet SDKs.</span></span>

<span data-ttu-id="083f5-135">Ces avertissements s’affichent lorsque la `PATH` variable d’environnement ne pointe pas vers les kits de développement logiciel (SDK) .net Core sur l’ordinateur.</span><span class="sxs-lookup"><span data-stu-id="083f5-135">These warnings appear when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="083f5-136">Pour résoudre ce problème:</span><span class="sxs-lookup"><span data-stu-id="083f5-136">To resolve this problem:</span></span>

* <span data-ttu-id="083f5-137">Installez le SDK .NET Core.</span><span class="sxs-lookup"><span data-stu-id="083f5-137">Install the .NET Core SDK.</span></span> <span data-ttu-id="083f5-138">Obtenez le programme d’installation le plus récent à partir de [téléchargements .net](https://dotnet.microsoft.com/download).</span><span class="sxs-lookup"><span data-stu-id="083f5-138">Obtain the latest installer from [.NET Downloads](https://dotnet.microsoft.com/download).</span></span>
* <span data-ttu-id="083f5-139">Vérifiez que la `PATH` variable d’environnement pointe vers l’emplacement où le kit de développement`C:\Program Files\dotnet\` logiciel (SDK) est installé ( `C:\Program Files (x86)\dotnet\` pour 64 bits/x64 ou pour 32 bits/x86).</span><span class="sxs-lookup"><span data-stu-id="083f5-139">Verify that the `PATH` environment variable points to the location where the SDK is installed (`C:\Program Files\dotnet\` for 64-bit/x64 or `C:\Program Files (x86)\dotnet\` for 32-bit/x86).</span></span> <span data-ttu-id="083f5-140">Le programme d’installation du kit `PATH`de développement logiciel (SDK) définit normalement.</span><span class="sxs-lookup"><span data-stu-id="083f5-140">The SDK installer normally sets the `PATH`.</span></span> <span data-ttu-id="083f5-141">Installez toujours les mêmes kits de développement logiciel (SDK) et runtimes sur le même ordinateur.</span><span class="sxs-lookup"><span data-stu-id="083f5-141">Always install the same bitness SDKs and runtimes on the same machine.</span></span>

### <a name="missing-sdk-after-installing-the-net-core-hosting-bundle"></a><span data-ttu-id="083f5-142">SDK manquant après l’installation du bundle d’hébergement .NET Core</span><span class="sxs-lookup"><span data-stu-id="083f5-142">Missing SDK after installing the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="083f5-143">L’installation du [bundle d’hébergement .net Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) modifie `PATH` le quand il installe le Runtime .net Core pour pointer vers la version 32 bits (x86) de .net Core`C:\Program Files (x86)\dotnet\`().</span><span class="sxs-lookup"><span data-stu-id="083f5-143">Installing the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) modifies the `PATH` when it installs the .NET Core runtime to point to the 32-bit (x86) version of .NET Core (`C:\Program Files (x86)\dotnet\`).</span></span> <span data-ttu-id="083f5-144">Cela peut entraîner des kits de développement logiciel (SDK) manquants lors de l' `dotnet` utilisation de la commande .net Core 32 bits (x86) ([aucun SDK .net Core n’a été détecté](#no-net-core-sdks-were-detected)).</span><span class="sxs-lookup"><span data-stu-id="083f5-144">This can result in missing SDKs when the 32-bit (x86) .NET Core `dotnet` command is used ([No .NET Core SDKs were detected](#no-net-core-sdks-were-detected)).</span></span> <span data-ttu-id="083f5-145">Pour résoudre ce problème, passez `C:\Program Files\dotnet\` à une position antérieure `C:\Program Files (x86)\dotnet\` à sur `PATH`.</span><span class="sxs-lookup"><span data-stu-id="083f5-145">To resolve this problem, move `C:\Program Files\dotnet\` to a position before `C:\Program Files (x86)\dotnet\` on the `PATH`.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="083f5-146">Obtenir des données à partir d’une application</span><span class="sxs-lookup"><span data-stu-id="083f5-146">Obtain data from an app</span></span>

<span data-ttu-id="083f5-147">Si une application est capable de répondre aux demandes, vous pouvez obtenir les données suivantes à partir de l’application à l’aide de l’intergiciel (middleware):</span><span class="sxs-lookup"><span data-stu-id="083f5-147">If an app is capable of responding to requests, you can obtain the following data from the app using middleware:</span></span>

* <span data-ttu-id="083f5-148">Méthode &ndash; de demande, schéma, hôte, pathbase, chemin d’accès, chaîne de requête, en-têtes</span><span class="sxs-lookup"><span data-stu-id="083f5-148">Request &ndash; Method, scheme, host, pathbase, path, query string, headers</span></span>
* <span data-ttu-id="083f5-149">Adresse &ndash; IP distante de la connexion, port distant, adresse IP locale, port local, certificat client</span><span class="sxs-lookup"><span data-stu-id="083f5-149">Connection &ndash; Remote IP address, remote port, local IP address, local port, client certificate</span></span>
* <span data-ttu-id="083f5-150">Nom &ndash; d’identité, nom d’affichage</span><span class="sxs-lookup"><span data-stu-id="083f5-150">Identity &ndash; Name, display name</span></span>
* <span data-ttu-id="083f5-151">Paramètres de configuration</span><span class="sxs-lookup"><span data-stu-id="083f5-151">Configuration settings</span></span>
* <span data-ttu-id="083f5-152">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="083f5-152">Environment variables</span></span>

<span data-ttu-id="083f5-153">Placez le code [intergiciel](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) suivant au début du pipeline de `Startup.Configure` traitement des demandes de la méthode.</span><span class="sxs-lookup"><span data-stu-id="083f5-153">Place the following [middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) code at the beginning of the `Startup.Configure` method's request processing pipeline.</span></span> <span data-ttu-id="083f5-154">L’environnement est vérifié avant l’exécution de l’intergiciel pour s’assurer que le code est exécuté uniquement dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="083f5-154">The environment is checked before the middleware is run to ensure that the code is only executed in the Development environment.</span></span>

<span data-ttu-id="083f5-155">Pour obtenir l’environnement, utilisez l’une des approches suivantes:</span><span class="sxs-lookup"><span data-stu-id="083f5-155">To obtain the environment, use either of the following approaches:</span></span>

* <span data-ttu-id="083f5-156">Injectez `Startup.Configure` dans la méthode et vérifiez l’environnement avec la variable locale. `IHostingEnvironment`</span><span class="sxs-lookup"><span data-stu-id="083f5-156">Inject the `IHostingEnvironment` into the `Startup.Configure` method and check the environment with the local variable.</span></span> <span data-ttu-id="083f5-157">L’exemple de code suivant illustre cette approche.</span><span class="sxs-lookup"><span data-stu-id="083f5-157">The following sample code demonstrates this approach.</span></span>

* <span data-ttu-id="083f5-158">Assignez l’environnement à une propriété `Startup` dans la classe.</span><span class="sxs-lookup"><span data-stu-id="083f5-158">Assign the environment to a property in the `Startup` class.</span></span> <span data-ttu-id="083f5-159">Vérifiez l’environnement à l’aide de la propriété ( `if (Environment.IsDevelopment())`par exemple,).</span><span class="sxs-lookup"><span data-stu-id="083f5-159">Check the environment using the property (for example, `if (Environment.IsDevelopment())`).</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, 
    IConfiguration config)
{
    if (env.IsDevelopment())
    {
        app.Run(async (context) =>
        {
            var sb = new StringBuilder();
            var nl = System.Environment.NewLine;
            var rule = string.Concat(nl, new string('-', 40), nl);
            var authSchemeProvider = app.ApplicationServices
                .GetRequiredService<IAuthenticationSchemeProvider>();

            sb.Append($"Request{rule}");
            sb.Append($"{DateTimeOffset.Now}{nl}");
            sb.Append($"{context.Request.Method} {context.Request.Path}{nl}");
            sb.Append($"Scheme: {context.Request.Scheme}{nl}");
            sb.Append($"Host: {context.Request.Headers["Host"]}{nl}");
            sb.Append($"PathBase: {context.Request.PathBase.Value}{nl}");
            sb.Append($"Path: {context.Request.Path.Value}{nl}");
            sb.Append($"Query: {context.Request.QueryString.Value}{nl}{nl}");

            sb.Append($"Connection{rule}");
            sb.Append($"RemoteIp: {context.Connection.RemoteIpAddress}{nl}");
            sb.Append($"RemotePort: {context.Connection.RemotePort}{nl}");
            sb.Append($"LocalIp: {context.Connection.LocalIpAddress}{nl}");
            sb.Append($"LocalPort: {context.Connection.LocalPort}{nl}");
            sb.Append($"ClientCert: {context.Connection.ClientCertificate}{nl}{nl}");

            sb.Append($"Identity{rule}");
            sb.Append($"User: {context.User.Identity.Name}{nl}");
            var scheme = await authSchemeProvider
                .GetSchemeAsync(IISDefaults.AuthenticationScheme);
            sb.Append($"DisplayName: {scheme?.DisplayName}{nl}{nl}");

            sb.Append($"Headers{rule}");
            foreach (var header in context.Request.Headers)
            {
                sb.Append($"{header.Key}: {header.Value}{nl}");
            }
            sb.Append(nl);

            sb.Append($"Websockets{rule}");
            if (context.Features.Get<IHttpUpgradeFeature>() != null)
            {
                sb.Append($"Status: Enabled{nl}{nl}");
            }
            else
            {
                sb.Append($"Status: Disabled{nl}{nl}");
            }

            sb.Append($"Configuration{rule}");
            foreach (var pair in config.AsEnumerable())
            {
                sb.Append($"{pair.Path}: {pair.Value}{nl}");
            }
            sb.Append(nl);

            sb.Append($"Environment Variables{rule}");
            var vars = System.Environment.GetEnvironmentVariables();
            foreach (var key in vars.Keys.Cast<string>().OrderBy(key => key, 
                StringComparer.OrdinalIgnoreCase))
            {
                var value = vars[key];
                sb.Append($"{key}: {value}{nl}");
            }

            context.Response.ContentType = "text/plain";
            await context.Response.WriteAsync(sb.ToString());
        });
    }
}
```
