---
title: Résoudre les problèmes des projets ASP.NET Core
author: Rick-Anderson
description: Comprenez et résolvez les avertissements et les erreurs avec les projets ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 06/19/2019
uid: test/troubleshoot
ms.openlocfilehash: bcec8a55a5111e1f3acf53ae2f57b45e6e609d25
ms.sourcegitcommit: 9f11685382eb1f4dd0fb694dea797adacedf9e20
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67313675"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="cc6f4-103">Résoudre les problèmes des projets ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cc6f4-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="cc6f4-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cc6f4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="cc6f4-105">Les liens suivants fournissent des conseils de dépannage :</span><span class="sxs-lookup"><span data-stu-id="cc6f4-105">The following links provide troubleshooting guidance:</span></span>

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="cc6f4-106">Conférence NDC (Londres, 2018) : Diagnostic des problèmes dans les Applications ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cc6f4-106">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="cc6f4-107">Blog de l’ASP.NET : Résolution des problèmes de performances d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cc6f4-107">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="cc6f4-108">Avertissements du SDK .NET core</span><span class="sxs-lookup"><span data-stu-id="cc6f4-108">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="cc6f4-109">Les versions 32 bits et 64 bits du SDK .NET Core sont installées</span><span class="sxs-lookup"><span data-stu-id="cc6f4-109">Both the 32-bit and 64-bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="cc6f4-110">Dans le **nouveau projet** boîte de dialogue pour ASP.NET Core, vous pouvez voir l’avertissement suivant :</span><span class="sxs-lookup"><span data-stu-id="cc6f4-110">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="cc6f4-111">Les versions 32 bits et 64 bits du SDK .NET Core sont installées.</span><span class="sxs-lookup"><span data-stu-id="cc6f4-111">Both 32-bit and 64-bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="cc6f4-112">Seuls les modèles à partir des versions 64 bits installées sur ' C:\\Program Files\\dotnet\\sdk\\» sont affichés.</span><span class="sxs-lookup"><span data-stu-id="cc6f4-112">Only templates from the 64-bit versions installed at 'C:\\Program Files\\dotnet\\sdk\\' are displayed.</span></span>

<span data-ttu-id="cc6f4-113">Cet avertissement s’affiche lorsque les (x86) 32 bits et les versions 64 bits (x 64) de la [du SDK .NET Core](https://www.microsoft.com/net/download/all) sont installés.</span><span class="sxs-lookup"><span data-stu-id="cc6f4-113">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="cc6f4-114">Raisons courantes, les deux versions peuvent être installées sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="cc6f4-114">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="cc6f4-115">Vous initialement téléchargé le programme d’installation du SDK .NET Core à l’aide d’un ordinateur 32 bits, mais ensuite copié sur et installé sur un ordinateur 64 bits.</span><span class="sxs-lookup"><span data-stu-id="cc6f4-115">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="cc6f4-116">Le SDK .NET Core de 32 bits a été installé par une autre application.</span><span class="sxs-lookup"><span data-stu-id="cc6f4-116">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="cc6f4-117">Une version incorrecte a été téléchargée et installée.</span><span class="sxs-lookup"><span data-stu-id="cc6f4-117">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="cc6f4-118">Désinstaller le SDK 32 bits de .NET Core pour éviter cet avertissement.</span><span class="sxs-lookup"><span data-stu-id="cc6f4-118">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="cc6f4-119">Désinstaller à partir de **le panneau de configuration** > **programmes et fonctionnalités** > **désinstaller ou modifier un programme**.</span><span class="sxs-lookup"><span data-stu-id="cc6f4-119">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="cc6f4-120">Si vous comprenez pourquoi l’avertissement se produit et ses implications, vous pouvez ignorer l’avertissement.</span><span class="sxs-lookup"><span data-stu-id="cc6f4-120">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="cc6f4-121">Le SDK .NET Core est installé dans plusieurs emplacements</span><span class="sxs-lookup"><span data-stu-id="cc6f4-121">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="cc6f4-122">Dans le **nouveau projet** boîte de dialogue pour ASP.NET Core, vous pouvez voir l’avertissement suivant :</span><span class="sxs-lookup"><span data-stu-id="cc6f4-122">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="cc6f4-123">Le SDK .NET Core est installé dans plusieurs emplacements.</span><span class="sxs-lookup"><span data-stu-id="cc6f4-123">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="cc6f4-124">Seuls les modèles à partir de kits de développement logiciel installés à ' C:\\Program Files\\dotnet\\sdk\\» sont affichés.</span><span class="sxs-lookup"><span data-stu-id="cc6f4-124">Only templates from the SDKs installed at 'C:\\Program Files\\dotnet\\sdk\\' are displayed.</span></span>

<span data-ttu-id="cc6f4-125">Ce message s’affiche lorsque vous avez au moins une installation du SDK .NET Core dans un répertoire en dehors de *C:\\Program Files\\dotnet\\sdk\\* .</span><span class="sxs-lookup"><span data-stu-id="cc6f4-125">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="cc6f4-126">Cela se produit généralement lorsque le SDK .NET Core a été déployé sur un ordinateur à l’aide de copier/coller au lieu du programme d’installation MSI.</span><span class="sxs-lookup"><span data-stu-id="cc6f4-126">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="cc6f4-127">Désinstallez tous les 32-bit SDK .NET Core et runtimes pour éviter cet avertissement.</span><span class="sxs-lookup"><span data-stu-id="cc6f4-127">Uninstall all 32-bit .NET Core SDKs and runtimes to prevent this warning.</span></span> <span data-ttu-id="cc6f4-128">Désinstaller à partir de **le panneau de configuration** > **programmes et fonctionnalités** > **désinstaller ou modifier un programme**.</span><span class="sxs-lookup"><span data-stu-id="cc6f4-128">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="cc6f4-129">Si vous comprenez pourquoi l’avertissement se produit et ses implications, vous pouvez ignorer l’avertissement.</span><span class="sxs-lookup"><span data-stu-id="cc6f4-129">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="cc6f4-130">Aucun SDK .NET Core ont été détectées.</span><span class="sxs-lookup"><span data-stu-id="cc6f4-130">No .NET Core SDKs were detected</span></span>

* <span data-ttu-id="cc6f4-131">Dans Visual Studio **nouveau projet** boîte de dialogue pour ASP.NET Core, vous pouvez voir l’avertissement suivant :</span><span class="sxs-lookup"><span data-stu-id="cc6f4-131">In the Visual Studio **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

  > <span data-ttu-id="cc6f4-132">Aucun SDK .NET Core ont été détectées, assurez-vous qu’ils sont inclus dans la variable d’environnement `PATH`.</span><span class="sxs-lookup"><span data-stu-id="cc6f4-132">No .NET Core SDKs were detected, ensure they are included in the environment variable `PATH`.</span></span>

* <span data-ttu-id="cc6f4-133">Lors de l’exécution un `dotnet` commande, l’avertissement s’affiche en tant que :</span><span class="sxs-lookup"><span data-stu-id="cc6f4-133">When executing a `dotnet` command, the warning appears as:</span></span>

  > <span data-ttu-id="cc6f4-134">Il n’était pas possible de trouver n’importe quel dotnet installé kits de développement logiciel.</span><span class="sxs-lookup"><span data-stu-id="cc6f4-134">It was not possible to find any installed dotnet SDKs.</span></span>

<span data-ttu-id="cc6f4-135">Ces avertissements apparaissent lorsque la variable d’environnement `PATH` ne pointe pas vers n’importe quel SDK .NET Core sur l’ordinateur.</span><span class="sxs-lookup"><span data-stu-id="cc6f4-135">These warnings appear when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="cc6f4-136">Pour résoudre ce problème :</span><span class="sxs-lookup"><span data-stu-id="cc6f4-136">To resolve this problem:</span></span>

* <span data-ttu-id="cc6f4-137">Installez le SDK .NET Core.</span><span class="sxs-lookup"><span data-stu-id="cc6f4-137">Install the .NET Core SDK.</span></span> <span data-ttu-id="cc6f4-138">Obtenir le programme d’installation la plus récente à partir de [téléchargements .NET](https://dotnet.microsoft.com/download).</span><span class="sxs-lookup"><span data-stu-id="cc6f4-138">Obtain the latest installer from [.NET Downloads](https://dotnet.microsoft.com/download).</span></span>
* <span data-ttu-id="cc6f4-139">Vérifiez que le `PATH` variable d’environnement pointe vers l’emplacement où est installé le Kit de développement logiciel (`C:\Program Files\dotnet\` pour 64-bit/x64 ou `C:\Program Files (x86)\dotnet\` pour 32 x bit/x86).</span><span class="sxs-lookup"><span data-stu-id="cc6f4-139">Verify that the `PATH` environment variable points to the location where the SDK is installed (`C:\Program Files\dotnet\` for 64-bit/x64 or `C:\Program Files (x86)\dotnet\` for 32-bit/x86).</span></span> <span data-ttu-id="cc6f4-140">Le programme d’installation du Kit de développement logiciel définit normalement le `PATH`.</span><span class="sxs-lookup"><span data-stu-id="cc6f4-140">The SDK installer normally sets the `PATH`.</span></span> <span data-ttu-id="cc6f4-141">Installez toujours le même nombre de bits SDK et runtimes sur le même ordinateur.</span><span class="sxs-lookup"><span data-stu-id="cc6f4-141">Always install the same bitness SDKs and runtimes on the same machine.</span></span>

### <a name="missing-sdk-after-installing-the-net-core-hosting-bundle"></a><span data-ttu-id="cc6f4-142">Après avoir installé le Bundle d’hébergement .NET Core SDK est absent</span><span class="sxs-lookup"><span data-stu-id="cc6f4-142">Missing SDK after installing the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="cc6f4-143">L’installation de la [Bundle d’hébergement .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) modifie le `PATH` quand il installe le runtime .NET Core pour pointer vers la version 32 bits (x 86) de .NET Core (`C:\Program Files (x86)\dotnet\`).</span><span class="sxs-lookup"><span data-stu-id="cc6f4-143">Installing the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) modifies the `PATH` when it installs the .NET Core runtime to point to the 32-bit (x86) version of .NET Core (`C:\Program Files (x86)\dotnet\`).</span></span> <span data-ttu-id="cc6f4-144">Cela peut entraîner manquant de kits de développement logiciel lorsque 32 bits (x 86) .NET Core `dotnet` commande est utilisée ([SDK .NET Core ont été détectées](#no-net-core-sdks-were-detected)).</span><span class="sxs-lookup"><span data-stu-id="cc6f4-144">This can result in missing SDKs when the 32-bit (x86) .NET Core `dotnet` command is used ([No .NET Core SDKs were detected](#no-net-core-sdks-were-detected)).</span></span> <span data-ttu-id="cc6f4-145">Pour résoudre ce problème, déplacez `C:\Program Files\dotnet\` à une position avant `C:\Program Files (x86)\dotnet\` sur le `PATH`.</span><span class="sxs-lookup"><span data-stu-id="cc6f4-145">To resolve this problem, move `C:\Program Files\dotnet\` to a position before `C:\Program Files (x86)\dotnet\` on the `PATH`.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="cc6f4-146">Obtenir des données à partir d’une application</span><span class="sxs-lookup"><span data-stu-id="cc6f4-146">Obtain data from an app</span></span>

<span data-ttu-id="cc6f4-147">Si une application est capable de répondre aux demandes, vous pouvez obtenir les données suivantes à partir de l’application à l’aide d’intergiciel (middleware) :</span><span class="sxs-lookup"><span data-stu-id="cc6f4-147">If an app is capable of responding to requests, you can obtain the following data from the app using middleware:</span></span>

* <span data-ttu-id="cc6f4-148">Demande &ndash; (méthode), schéma, hôte, pathbase, le chemin d’accès, chaîne de requête, en-têtes</span><span class="sxs-lookup"><span data-stu-id="cc6f4-148">Request &ndash; Method, scheme, host, pathbase, path, query string, headers</span></span>
* <span data-ttu-id="cc6f4-149">Connexion &ndash; adresse IP distante, port distant, adresse IP locale, port local, certificat client</span><span class="sxs-lookup"><span data-stu-id="cc6f4-149">Connection &ndash; Remote IP address, remote port, local IP address, local port, client certificate</span></span>
* <span data-ttu-id="cc6f4-150">Identité &ndash; nom, nom d’affichage</span><span class="sxs-lookup"><span data-stu-id="cc6f4-150">Identity &ndash; Name, display name</span></span>
* <span data-ttu-id="cc6f4-151">Paramètres de configuration</span><span class="sxs-lookup"><span data-stu-id="cc6f4-151">Configuration settings</span></span>
* <span data-ttu-id="cc6f4-152">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="cc6f4-152">Environment variables</span></span>

<span data-ttu-id="cc6f4-153">Placez le code suivant [intergiciel (middleware)](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) code au début de la `Startup.Configure` pipeline de traitement de demande de la méthode.</span><span class="sxs-lookup"><span data-stu-id="cc6f4-153">Place the following [middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) code at the beginning of the `Startup.Configure` method's request processing pipeline.</span></span> <span data-ttu-id="cc6f4-154">L’environnement est vérifié avant l’exécution de l’intergiciel (middleware) pour vous assurer que le code est exécuté uniquement dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="cc6f4-154">The environment is checked before the middleware is run to ensure that the code is only executed in the Development environment.</span></span>

<span data-ttu-id="cc6f4-155">Pour obtenir l’environnement, utilisez une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="cc6f4-155">To obtain the environment, use either of the following approaches:</span></span>

* <span data-ttu-id="cc6f4-156">Injecter le `IHostingEnvironment` dans le `Startup.Configure` (méthode) et la vérification de l’environnement avec la variable locale.</span><span class="sxs-lookup"><span data-stu-id="cc6f4-156">Inject the `IHostingEnvironment` into the `Startup.Configure` method and check the environment with the local variable.</span></span> <span data-ttu-id="cc6f4-157">L’exemple de code suivant illustre cette approche.</span><span class="sxs-lookup"><span data-stu-id="cc6f4-157">The following sample code demonstrates this approach.</span></span>

* <span data-ttu-id="cc6f4-158">Affecter l’environnement à une propriété dans la `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="cc6f4-158">Assign the environment to a property in the `Startup` class.</span></span> <span data-ttu-id="cc6f4-159">Vérifier l’environnement à l’aide de la propriété (par exemple, `if (Environment.IsDevelopment())`).</span><span class="sxs-lookup"><span data-stu-id="cc6f4-159">Check the environment using the property (for example, `if (Environment.IsDevelopment())`).</span></span>

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
