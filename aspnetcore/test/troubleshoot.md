---
title: Résoudre les problèmes des projets ASP.NET Core
author: Rick-Anderson
description: Comprenez et résolvez les avertissements et les erreurs avec les projets ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2019
uid: test/troubleshoot
ms.openlocfilehash: 3d755b2f0c509d65dea86bbe719e42935d87d546
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64895326"
---
# <a name="troubleshoot-aspnet-core-projects"></a>Résoudre les problèmes des projets ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Les liens suivants fournissent des conseils de dépannage :

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [Conférence NDC (Londres, 2018) : Diagnostic des problèmes dans les Applications ASP.NET Core](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [Blog de l’ASP.NET : Résolution des problèmes de performances d’ASP.NET Core](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a>Avertissements du SDK .NET core

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>32 bits et 64 bits des versions du SDK .NET Core sont installées.

Dans le **nouveau projet** boîte de dialogue pour ASP.NET Core, vous pouvez voir l’avertissement suivant :

> 32 et 64 bits des versions du SDK .NET Core sont installées. Seuls les modèles à partir de l’ou les versions 64 bits installée sur ' C:\\Program Files\\dotnet\\sdk\\» s’affiche.

Cet avertissement s’affiche lorsque les (x86) 32 bits et les versions 64 bits (x 64) de la [du SDK .NET Core](https://www.microsoft.com/net/download/all) sont installés. Raisons courantes, les deux versions peuvent être installées sont les suivantes :

* Vous initialement téléchargé le programme d’installation du SDK .NET Core à l’aide d’un ordinateur 32 bits, mais ensuite copié sur et installé sur un ordinateur 64 bits.
* Le SDK .NET Core de 32 bits a été installé par une autre application.
* Une version incorrecte a été téléchargée et installée.

Désinstaller le SDK 32 bits de .NET Core pour éviter cet avertissement. Désinstaller à partir de **le panneau de configuration** > **programmes et fonctionnalités** > **désinstaller ou modifier un programme**. Si vous comprenez pourquoi l’avertissement se produit et ses implications, vous pouvez ignorer l’avertissement.

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>Le SDK .NET Core est installé dans plusieurs emplacements

Dans le **nouveau projet** boîte de dialogue pour ASP.NET Core, vous pouvez voir l’avertissement suivant :

> Le SDK .NET Core est installé dans plusieurs emplacements. Seuls les modèles des SDK installés à ' C:\\Program Files\\dotnet\\sdk\\» s’affiche.

Ce message s’affiche lorsque vous avez au moins une installation du SDK .NET Core dans un répertoire en dehors de *C:\\Program Files\\dotnet\\sdk\\*. Cela se produit généralement lorsque le SDK .NET Core a été déployé sur un ordinateur à l’aide de copier/coller au lieu du programme d’installation MSI.

Désinstallez tous les 32-bit SDK .NET Core et runtimes pour éviter cet avertissement. Désinstaller à partir de **le panneau de configuration** > **programmes et fonctionnalités** > **désinstaller ou modifier un programme**. Si vous comprenez pourquoi l’avertissement se produit et ses implications, vous pouvez ignorer l’avertissement.

### <a name="no-net-core-sdks-were-detected"></a>Aucun SDK .NET Core ont été détectées.

* Dans Visual Studio **nouveau projet** boîte de dialogue pour ASP.NET Core, vous pouvez voir l’avertissement suivant :

  > Aucun SDK .NET Core ont été détectées, assurez-vous qu’ils sont inclus dans la variable d’environnement `PATH`.

* Lors de l’exécution un `dotnet` commande, l’avertissement s’affiche en tant que :

  > Il n’était pas possible de trouver n’importe quel dotnet installé kits de développement logiciel.

Ces avertissements apparaissent lorsque la variable d’environnement `PATH` ne pointe pas vers n’importe quel SDK .NET Core sur l’ordinateur. Pour résoudre ce problème :

* Installez le SDK .NET Core. Obtenir le programme d’installation la plus récente à partir de [téléchargements .NET](https://dotnet.microsoft.com/download).
* Vérifiez que le `PATH` variable d’environnement pointe vers l’emplacement où est installé le Kit de développement logiciel (`C:\Program Files\dotnet\` pour 64-bit/x64 ou `C:\Program Files (x86)\dotnet\` pour 32 x bit/x86). Le programme d’installation du Kit de développement logiciel définit normalement le `PATH`. Installez toujours le même nombre de bits SDK et runtimes sur le même ordinateur.

### <a name="missing-sdk-after-installing-the-net-core-hosting-bundle"></a>Après avoir installé le Bundle d’hébergement .NET Core SDK est absent

L’installation de la [Bundle d’hébergement .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) modifie le `PATH` quand il installe le runtime .NET Core pour pointer vers la version 32 bits (x 86) de .NET Core (`C:\Program Files (x86)\dotnet\`). Cela peut entraîner manquant de kits de développement logiciel lorsque 32 bits (x 86) .NET Core `dotnet` commande est utilisée ([SDK .NET Core ont été détectées](#no-net-core-sdks-were-detected)). Pour résoudre ce problème, déplacez `C:\Program Files\dotnet\` à une position avant `C:\Program Files (x86)\dotnet\` sur le `PATH`.

## <a name="obtain-data-from-an-app"></a>Obtenir des données à partir d’une application

Si une application est capable de répondre aux demandes, vous pouvez obtenir les données suivantes à partir de l’application à l’aide d’intergiciel (middleware) :

* Demande &ndash; (méthode), schéma, hôte, pathbase, le chemin d’accès, chaîne de requête, en-têtes
* Connexion &ndash; adresse IP distante, port distant, adresse IP locale, port local, certificat client
* Identité &ndash; nom, nom d’affichage
* Paramètres de configuration
* Variables d’environnement

Placez le code suivant [intergiciel (middleware)](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) code au début de la `Startup.Configure` pipeline de traitement de demande de la méthode. L’environnement est vérifié avant l’exécution de l’intergiciel (middleware) pour vous assurer que le code est exécuté uniquement dans l’environnement de développement.

Pour obtenir l’environnement, utilisez une des approches suivantes :

* Injecter le `IHostingEnvironment` dans le `Startup.Configure` (méthode) et la vérification de l’environnement avec la variable locale. L’exemple de code suivant illustre cette approche.

* Affecter l’environnement à une propriété dans la `Startup` classe. Vérifier l’environnement à l’aide de la propriété (par exemple, `if (Environment.IsDevelopment())`).

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
