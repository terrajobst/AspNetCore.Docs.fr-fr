---
title: Résoudre les problèmes des projets ASP.NET Core
author: Rick-Anderson
description: Comprenez et résolvez les avertissements et les erreurs avec les projets ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/26/2019
uid: test/troubleshoot
ms.openlocfilehash: c8b34f51fd329eb9a7c34f7be93bd7f2aa054283
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2019
ms.locfileid: "56899284"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="77e63-103">Résoudre les problèmes des projets ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="77e63-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="77e63-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="77e63-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="77e63-105">Les liens suivants fournissent des conseils de dépannage :</span><span class="sxs-lookup"><span data-stu-id="77e63-105">The following links provide troubleshooting guidance:</span></span>

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="77e63-106">Conférence NDC (Londres, 2018) : Diagnostic des problèmes dans les Applications ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="77e63-106">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="77e63-107">Blog de l’ASP.NET : Résolution des problèmes de performances d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="77e63-107">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="77e63-108">Avertissements du SDK .NET core</span><span class="sxs-lookup"><span data-stu-id="77e63-108">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="77e63-109">32 bits et 64 bits des versions du SDK .NET Core sont installées.</span><span class="sxs-lookup"><span data-stu-id="77e63-109">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="77e63-110">Dans le **nouveau projet** boîte de dialogue pour ASP.NET Core, vous pouvez voir l’avertissement suivant :</span><span class="sxs-lookup"><span data-stu-id="77e63-110">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="77e63-111">32 et 64 bits des versions du SDK .NET Core sont installées.</span><span class="sxs-lookup"><span data-stu-id="77e63-111">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="77e63-112">Seuls les modèles à partir de l’ou les versions 64 bits installée sur ' C:\\Program Files\\dotnet\\sdk\\» s’affiche.</span><span class="sxs-lookup"><span data-stu-id="77e63-112">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Une capture d’écran de la boîte de dialogue OneASP.NET affichant le message d’avertissement](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="77e63-114">Cet avertissement s’affiche lorsque les (x86) 32 bits et les versions 64 bits (x 64) de la [du SDK .NET Core](https://www.microsoft.com/net/download/all) sont installés.</span><span class="sxs-lookup"><span data-stu-id="77e63-114">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="77e63-115">Raisons courantes, les deux versions peuvent être installées sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="77e63-115">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="77e63-116">Vous initialement téléchargé le programme d’installation du SDK .NET Core à l’aide d’un ordinateur 32 bits, mais ensuite copié sur et installé sur un ordinateur 64 bits.</span><span class="sxs-lookup"><span data-stu-id="77e63-116">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="77e63-117">Le SDK .NET Core de 32 bits a été installé par une autre application.</span><span class="sxs-lookup"><span data-stu-id="77e63-117">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="77e63-118">Une version incorrecte a été téléchargée et installée.</span><span class="sxs-lookup"><span data-stu-id="77e63-118">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="77e63-119">Désinstaller le SDK 32 bits de .NET Core pour éviter cet avertissement.</span><span class="sxs-lookup"><span data-stu-id="77e63-119">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="77e63-120">Désinstaller à partir de **le panneau de configuration** > **programmes et fonctionnalités** > **désinstaller ou modifier un programme**.</span><span class="sxs-lookup"><span data-stu-id="77e63-120">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="77e63-121">Si vous comprenez pourquoi l’avertissement se produit et ses implications, vous pouvez ignorer l’avertissement.</span><span class="sxs-lookup"><span data-stu-id="77e63-121">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="77e63-122">Le SDK .NET Core est installé dans plusieurs emplacements</span><span class="sxs-lookup"><span data-stu-id="77e63-122">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="77e63-123">Dans le **nouveau projet** boîte de dialogue pour ASP.NET Core, vous pouvez voir l’avertissement suivant :</span><span class="sxs-lookup"><span data-stu-id="77e63-123">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="77e63-124">Le SDK .NET Core est installé dans plusieurs emplacements.</span><span class="sxs-lookup"><span data-stu-id="77e63-124">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="77e63-125">Seuls les modèles des SDK installés à ' C:\\Program Files\\dotnet\\sdk\\» s’affiche.</span><span class="sxs-lookup"><span data-stu-id="77e63-125">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Une capture d’écran de la boîte de dialogue OneASP.NET affichant le message d’avertissement](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="77e63-127">Ce message s’affiche lorsque vous avez au moins une installation du SDK .NET Core dans un répertoire en dehors de *C:\\Program Files\\dotnet\\sdk\\*.</span><span class="sxs-lookup"><span data-stu-id="77e63-127">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="77e63-128">Cela se produit généralement lorsque le SDK .NET Core a été déployé sur un ordinateur à l’aide de copier/coller au lieu du programme d’installation MSI.</span><span class="sxs-lookup"><span data-stu-id="77e63-128">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="77e63-129">Désinstaller le SDK 32 bits de .NET Core pour éviter cet avertissement.</span><span class="sxs-lookup"><span data-stu-id="77e63-129">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="77e63-130">Désinstaller à partir de **le panneau de configuration** > **programmes et fonctionnalités** > **désinstaller ou modifier un programme**.</span><span class="sxs-lookup"><span data-stu-id="77e63-130">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="77e63-131">Si vous comprenez pourquoi l’avertissement se produit et ses implications, vous pouvez ignorer l’avertissement.</span><span class="sxs-lookup"><span data-stu-id="77e63-131">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="77e63-132">Aucun SDK .NET Core ont été détectées.</span><span class="sxs-lookup"><span data-stu-id="77e63-132">No .NET Core SDKs were detected</span></span>

<span data-ttu-id="77e63-133">Dans le **nouveau projet** boîte de dialogue pour ASP.NET Core, vous pouvez voir l’avertissement suivant :</span><span class="sxs-lookup"><span data-stu-id="77e63-133">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="77e63-134">Aucun SDK .NET Core ont été détectées, assurez-vous qu’ils sont inclus dans la variable d’environnement « PATH ».</span><span class="sxs-lookup"><span data-stu-id="77e63-134">No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'.</span></span>

![Une capture d’écran de la boîte de dialogue OneASP.NET affichant le message d’avertissement](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="77e63-136">Cet avertissement apparaît lorsque la variable d’environnement `PATH` ne pointe pas vers n’importe quel SDK .NET Core sur l’ordinateur (par exemple, `C:\Program Files\dotnet\` et `C:\Program Files (x86)\dotnet\`).</span><span class="sxs-lookup"><span data-stu-id="77e63-136">This warning appears when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine (for example, `C:\Program Files\dotnet\` and `C:\Program Files (x86)\dotnet\`).</span></span> <span data-ttu-id="77e63-137">Pour résoudre ce problème :</span><span class="sxs-lookup"><span data-stu-id="77e63-137">To resolve this problem:</span></span>

* <span data-ttu-id="77e63-138">Installer ou vérifiez que le SDK .NET Core est installé.</span><span class="sxs-lookup"><span data-stu-id="77e63-138">Install or verify the .NET Core SDK is installed.</span></span> <span data-ttu-id="77e63-139">Obtenir le programme d’installation la plus récente à partir de [téléchargements .NET](https://dotnet.microsoft.com/download).</span><span class="sxs-lookup"><span data-stu-id="77e63-139">Obtain the latest installer from [.NET Downloads](https://dotnet.microsoft.com/download).</span></span> 
* <span data-ttu-id="77e63-140">Vérifiez que le `PATH` variable d’environnement pointe vers l’emplacement où est installé le Kit de développement.</span><span class="sxs-lookup"><span data-stu-id="77e63-140">Verify that the `PATH` environment variable points to the location where the SDK is installed.</span></span> <span data-ttu-id="77e63-141">Le programme d’installation définit normalement le `PATH`.</span><span class="sxs-lookup"><span data-stu-id="77e63-141">The installer normally sets the `PATH`.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="77e63-142">Obtenir des données à partir d’une application</span><span class="sxs-lookup"><span data-stu-id="77e63-142">Obtain data from an app</span></span>

<span data-ttu-id="77e63-143">Si une application est capable de répondre aux demandes, vous pouvez obtenir les données suivantes à partir de l’application à l’aide d’intergiciel (middleware) :</span><span class="sxs-lookup"><span data-stu-id="77e63-143">If an app is capable of responding to requests, you can obtain the following data from the app using middleware:</span></span>

* <span data-ttu-id="77e63-144">Demande &ndash; (méthode), schéma, hôte, pathbase, le chemin d’accès, chaîne de requête, en-têtes</span><span class="sxs-lookup"><span data-stu-id="77e63-144">Request &ndash; Method, scheme, host, pathbase, path, query string, headers</span></span>
* <span data-ttu-id="77e63-145">Connexion &ndash; adresse IP distante, port distant, adresse IP locale, port local, certificat client</span><span class="sxs-lookup"><span data-stu-id="77e63-145">Connection &ndash; Remote IP address, remote port, local IP address, local port, client certificate</span></span>
* <span data-ttu-id="77e63-146">Identité &ndash; nom, nom d’affichage</span><span class="sxs-lookup"><span data-stu-id="77e63-146">Identity &ndash; Name, display name</span></span>
* <span data-ttu-id="77e63-147">Paramètres de configuration</span><span class="sxs-lookup"><span data-stu-id="77e63-147">Configuration settings</span></span>
* <span data-ttu-id="77e63-148">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="77e63-148">Environment variables</span></span>

<span data-ttu-id="77e63-149">Placez le code suivant [intergiciel (middleware)](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) code au début de la `Startup.Configure` pipeline de traitement de demande de la méthode.</span><span class="sxs-lookup"><span data-stu-id="77e63-149">Place the following [middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) code at the beginning of the `Startup.Configure` method's request processing pipeline.</span></span> <span data-ttu-id="77e63-150">L’environnement est vérifié avant l’exécution de l’intergiciel (middleware) pour vous assurer que le code est exécuté uniquement dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="77e63-150">The environment is checked before the middleware is run to ensure that the code is only executed in the Development environment.</span></span>

<span data-ttu-id="77e63-151">Pour obtenir l’environnement, utilisez une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="77e63-151">To obtain the environment, use either of the following approaches:</span></span>

* <span data-ttu-id="77e63-152">Injecter le `IHostingEnvironment` dans le `Startup.Configure` (méthode) et la vérification de l’environnement avec la variable locale.</span><span class="sxs-lookup"><span data-stu-id="77e63-152">Inject the `IHostingEnvironment` into the `Startup.Configure` method and check the environment with the local variable.</span></span> <span data-ttu-id="77e63-153">L’exemple de code suivant illustre cette approche.</span><span class="sxs-lookup"><span data-stu-id="77e63-153">The following sample code demonstrates this approach.</span></span>

* <span data-ttu-id="77e63-154">Affecter l’environnement à une propriété dans la `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="77e63-154">Assign the environment to a property in the `Startup` class.</span></span> <span data-ttu-id="77e63-155">Vérifier l’environnement à l’aide de la propriété (par exemple, `if (Environment.IsDevelopment())`).</span><span class="sxs-lookup"><span data-stu-id="77e63-155">Check the environment using the property (for example, `if (Environment.IsDevelopment())`).</span></span>

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
