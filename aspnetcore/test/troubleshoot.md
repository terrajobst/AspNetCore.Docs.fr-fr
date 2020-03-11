---
title: Résoudre les problèmes et déboguer des projets ASP.NET Core
author: Rick-Anderson
description: Comprenez et résolvez les avertissements et les erreurs avec les projets ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/10/2019
uid: test/troubleshoot
ms.openlocfilehash: 042e1c84a7226cb4bf56bcc0ff4e576b67e68e1b
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78655301"
---
# <a name="troubleshoot-and-debug-aspnet-core-projects"></a>Résoudre les problèmes et déboguer des projets ASP.NET Core

De [Rick Anderson](https://twitter.com/RickAndMSFT)

Les liens suivants fournissent des conseils de dépannage :

* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [Conférence norvégiens (Londres, 2018) : diagnostic des problèmes dans les applications ASP.NET Core](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [Blog ASP.NET : résolution des problèmes de performances de ASP.NET Core](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a>Avertissements de kit SDK .NET Core

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>Les versions 32 bits et 64 bits de la kit SDK .NET Core sont installées

Dans la boîte de dialogue **nouveau projet** pour ASP.net Core, l’avertissement suivant peut s’afficher :

> Les versions 32 bits et 64 bits de la kit SDK .NET Core sont installées. Seuls les modèles des versions 64 bits installées à l’adresse « C :\\Program Files\\dotnet\\SDK\\» s’affichent.

Cet avertissement s’affiche lorsque les versions 32 bits (x86) et 64 bits (x64) du [Kit SDK .net Core](https://www.microsoft.com/net/download/all) sont installées. Les raisons courantes pour lesquelles les deux versions peuvent être installées sont les suivantes :

* À l’origine, vous avez téléchargé le programme d’installation de kit SDK .NET Core à l’aide d’un ordinateur 32 bits, puis vous l’avez copié et installé sur un ordinateur 64 bits.
* Le kit SDK .NET Core 32 bits a été installé par une autre application.
* La version incorrecte a été téléchargée et installée.

Désinstallez le kit SDK .NET Core 32 bits pour éviter cet avertissement. Désinstallez dans **le panneau de configuration** > **programmes et fonctionnalités** > **désinstaller ou modifier un programme**. Si vous comprenez la raison pour laquelle l’avertissement se produit et ses implications, vous pouvez ignorer l’avertissement.

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>Le kit SDK .NET Core est installé à plusieurs emplacements

Dans la boîte de dialogue **nouveau projet** pour ASP.net Core, l’avertissement suivant peut s’afficher :

> Le kit SDK .NET Core est installé à plusieurs emplacements. Seuls les modèles des kits de développement logiciel (SDK) installés à l’adresse « C :\\Program Files\\dotnet\\SDK\\» s’affichent.

Ce message s’affiche lorsque vous avez au moins une installation du kit SDK .NET Core dans un répertoire en dehors de *C :\\Program Files\\dotnet\\SDK\\* . Cela se produit généralement lorsque le kit SDK .NET Core a été déployé sur un ordinateur à l’aide de la fonction copier/coller au lieu du programme d’installation MSI.

Désinstallez tous les kits de développement logiciel (SDK) .NET Core 32 bits et les runtimes pour éviter cet avertissement. Désinstallez dans **le panneau de configuration** > **programmes et fonctionnalités** > **désinstaller ou modifier un programme**. Si vous comprenez la raison pour laquelle l’avertissement se produit et ses implications, vous pouvez ignorer l’avertissement.

### <a name="no-net-core-sdks-were-detected"></a>Aucun SDK .NET Core n’a été détecté

* Dans la boîte de dialogue **nouveau projet** de Visual Studio pour ASP.net Core, l’avertissement suivant peut s’afficher :

  > Aucun kit de développement logiciel (SDK) .NET Core n’a été détecté, vérifiez qu’ils sont inclus dans la variable d’environnement `PATH`.

* Lors de l’exécution d’une commande `dotnet`, l’avertissement s’affiche comme suit :

  > Il n’était pas possible de trouver les kits de développement logiciel (SDK) dotnet installés.

Ces avertissements s’affichent lorsque la variable d’environnement `PATH` ne pointe pas vers les kits de développement logiciel (SDK) .NET Core sur l’ordinateur. Pour résoudre ce problème :

* Installez le SDK .NET Core. Obtenez le programme d’installation le plus récent à partir de [téléchargements .net](https://dotnet.microsoft.com/download).
* Vérifiez que la variable d’environnement `PATH` pointe vers l’emplacement où le kit de développement logiciel (SDK) est installé (`C:\Program Files\dotnet\` pour 64 bits/x64 ou `C:\Program Files (x86)\dotnet\` pour 32 bits/x86). Le programme d’installation du kit de développement logiciel (SDK) définit normalement le `PATH`. Installez toujours les mêmes kits de développement logiciel (SDK) et runtimes sur le même ordinateur.

### <a name="missing-sdk-after-installing-the-net-core-hosting-bundle"></a>SDK manquant après l’installation du bundle d’hébergement .NET Core

L’installation du [bundle d’hébergement .net Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) modifie le `PATH` lorsqu’il installe le Runtime .net Core pour pointer vers la version 32 bits (x86) de .net core (`C:\Program Files (x86)\dotnet\`). Cela peut entraîner des kits de développement logiciel (SDK) manquants lors de l’utilisation 32 de la commande .NET Core `dotnet` (x86) .NET Core ([aucun SDK .net Core n’a été détecté](#no-net-core-sdks-were-detected)). Pour résoudre ce problème, déplacez `C:\Program Files\dotnet\` vers une position avant `C:\Program Files (x86)\dotnet\` sur le `PATH`.

## <a name="obtain-data-from-an-app"></a>Obtenir des données à partir d’une application

Si une application est capable de répondre aux demandes, vous pouvez obtenir les données suivantes à partir de l’application à l’aide de l’intergiciel (middleware) :

* Demande &ndash; méthode, schéma, hôte, pathbase, chemin d’accès, chaîne de requête, en-têtes
* Connexion &ndash; l’adresse IP distante, le port distant, l’adresse IP locale, le port local, le certificat client
* Nom d' &ndash; d’identité, nom d’affichage
* Paramètres de configuration
* Variables d'environnement

Placez le code [intergiciel](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) suivant au début du pipeline de traitement des demandes de la méthode `Startup.Configure`. L’environnement est vérifié avant l’exécution de l’intergiciel pour s’assurer que le code est exécuté uniquement dans l’environnement de développement.

Pour obtenir l’environnement, utilisez l’une des approches suivantes :

* Injectez le `IHostingEnvironment` dans la méthode `Startup.Configure` et vérifiez l’environnement avec la variable locale. L’exemple de code suivant illustre cette approche.

* Assignez l’environnement à une propriété dans la classe `Startup`. Vérifiez l’environnement à l’aide de la propriété (par exemple, `if (Environment.IsDevelopment())`).

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

## <a name="debug-aspnet-core-apps"></a>Déboguer les applications ASP.NET Core

Les liens suivants fournissent des informations sur le débogage des applications ASP.NET Core.

* [Débogage d’ASP Core sur Linux](https://devblogs.microsoft.com/premier-developer/debugging-asp-core-on-linux-with-visual-studio-2017/)
* [Débogage de .NET Core sur UNIX sur SSH](https://devblogs.microsoft.com/devops/debugging-net-core-on-unix-over-ssh/)
* [Démarrage rapide : déboguer ASP.NET avec le débogueur Visual Studio](/visualstudio/debugger/quickstart-debug-aspnet)
* Pour plus d’informations sur le débogage, consultez [ce problème GitHub](https://github.com/dotnet/AspNetCore.Docs/issues/2960) .
