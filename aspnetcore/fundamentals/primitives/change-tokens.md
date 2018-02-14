---
title: "Détecter les modifications avec des jetons de modification dans ASP.NET Core"
author: guardrex
description: "Découvrez comment utiliser des jetons de modification pour effectuer le suivi des modifications."
manager: wpickett
ms.author: riande
ms.date: 11/10/2017
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/primitives/change-tokens
ms.openlocfilehash: 94bf356fcbfab3930804485c1b65e4a0f4c52b8e
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/31/2018
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a>Détecter les modifications avec des jetons de modification dans ASP.NET Core

Par [Luke Latham](https://github.com/guardrex)

Un *jeton de modification* est un module à usage général de bas niveau, utilisé pour effectuer le suivi des modifications.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="ichangetoken-interface"></a>Interface d’IChangeToken

[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) propage des notifications indiquant qu’une modification s’est produite. `IChangeToken` se trouve dans l’espace de noms [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives). Pour les applications qui n’utilisent pas le métapackage [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/), référencez le package NuGet [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) dans le fichier projet.

`IChangeToken` a deux propriétés :

* [ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) indique si le jeton déclenche des rappels de façon proactive. Si `ActiveChangedCallbacks` est défini sur `false`, un rappel n’est jamais appelé, et l’application doit interroger `HasChanged` pour connaître les modifications. Il est également possible qu’un jeton ne soit jamais annulé si aucune modification ne se produit, ou si l’écouteur de modifications sous-jacent est supprimé ou désactivé.
* [HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) obtient une valeur qui indique si une modification a eu lieu.

L’interface a une méthode, [RegisterChangeCallback(Action&lt;objet&gt;, Object)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback), qui inscrit un rappel qui est appelé quand le jeton a été modifié. `HasChanged` doit être défini avant que le rappel soit appelé.

## <a name="changetoken-class"></a>Classe ChangeToken

`ChangeToken` est une classe statique utilisée pour propager des notifications indiquant qu’une modification s’est produite. `ChangeToken` se trouve dans l’espace de noms [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives). Pour les applications qui n’utilisent pas le métapackage [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/), référencez le package NuGet [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) dans le fichier projet.

La méthode `ChangeToken` [OnChange(Func&lt;IChangeToken&gt;, Action)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) inscrit une `Action` à appeler chaque fois que le jeton change :
* `Func<IChangeToken>` produit le jeton.
* `Action` est appelée quand le jeton change.

`ChangeToken` a une surcharge [OnChange&lt;TState&gt;(Func&lt;IChangeToken&gt;, Action&lt;TState&gt;, TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) qui prend un paramètre `TState` supplémentaire passé dans l’élément `Action` du consommateur du jeton.

`OnChange` retourne un [IDisposable](/dotnet/api/system.idisposable). L’appel de [Dispose](/dotnet/api/system.idisposable.dispose) fait que le jeton cesse d’être à l’écoute des modifications suivantes et libère les ressources du jeton.

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a>Exemples d’utilisation de jetons de modification dans ASP.NET Core

Les jetons de modification sont utilisés dans des zones importantes d’ASP.NET Core pour la surveillance des modifications apportées aux objets :

* Pour surveiller les modifications apportées aux fichiers, la méthode [Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) de [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) crée un `IChangeToken` pour les fichiers ou le dossier à surveiller.
* Les jetons `IChangeToken` peuvent être ajoutés aux entrées du cache pour déclencher des suppressions dans le cache en cas de modification.
* Pour les modifications `TOptions`, l’implémentation par défaut de [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) de [IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) a une surcharge qui accepte une ou plusieurs instances de [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1). Chaque instance retourne un `IChangeToken` pour inscrire un rappel de notification de modification pour les modifications des options de suivi.

## <a name="monitoring-for-configuration-changes"></a>Surveillance des modifications de configuration

Par défaut, les modèles ASP.NET Core utilisent des [fichiers de configuration JSON](xref:fundamentals/configuration/index#json-configuration) (*appsettings.json*, *appsettings.Development.json* et *appsettings.Production.json*) pour charger les paramètres de configuration des applications.

Ces fichiers sont configurés avec la méthode d’extension [AddJsonFile(IConfigurationBuilder, chaîne, booléen, booléen)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) sur [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) qui accepte un paramètre `reloadOnChange` (ASP.NET Core 1.1 et ultérieur). `reloadOnChange` indique si la configuration doit être rechargée en cas de modification d’un fichier. Vous pouvez voir ce paramètre dans la méthode pratique [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) de [WebHost](/dotnet/api/microsoft.aspnetcore.webhost) ([source de référence](https://github.com/aspnet/MetaPackages/blob/rel/2.0.3/src/Microsoft.AspNetCore/WebHost.cs#L152-L193)) :

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, reloadOnChange: true);
```

La configuration basée sur les fichiers est représentée par [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource). `FileConfigurationSource` utilise [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) ([source de référence](https://github.com/aspnet/FileSystem/blob/patch/2.0.1/src/Microsoft.Extensions.FileProviders.Abstractions/IFileProvider.cs)) pour surveiller les fichiers.

Par défaut, le `IFileMonitor` est fourni par un [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) ([source de référence](https://github.com/aspnet/Configuration/blob/patch/2.0.1/src/Microsoft.Extensions.Configuration.FileExtensions/FileConfigurationSource.cs#L82)), qui utilise [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) pour surveiller les modifications des fichiers de configuration.

L’exemple d’application montre les deux implémentations de la surveillance des modifications de configuration. Si le fichier *appsettings.json* change ou que la version de l’environnement du fichier change, chaque implémentation exécute du code personnalisé. L’exemple d’application écrit un message sur la console.

Le `FileSystemWatcher` d’un fichier de configuration peut déclencher plusieurs rappels de jeton pour une même modification du fichier de configuration. L’implémentation de l’exemple protège de ce problème en vérifiant les hachages de fichier sur les fichiers de configuration. La vérification des hachages de fichier garantit qu’au moins un des fichiers de configuration a changé avant d’exécuter le code personnalisé. L’exemple utilise le hachage de fichier SHA1 (*Utilities/Utilities.cs*) :

   [!code-csharp[Main](change-tokens/sample/Utilities/Utilities.cs?name=snippet1)]

   Une nouvelle tentative est implémentée avec une interruption d’une durée exponentielle. Une nouvelle tentative est effectuée, car il peut se produire un verrouillage du fichier qui empêche temporairement le calcul d’un nouveau hachage sur un des fichiers.

### <a name="simple-startup-change-token"></a>Jeton de modification de démarrage simple

Inscrivez un rappel `Action` de consommateur de jeton pour les notifications de modification au jeton de rechargement de configuration (*Startup.cs*) :

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet2)]

`config.GetReloadToken()` fournit le jeton. Le rappel est la méthode `InvokeChanged` :

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet3)]

Le `state` du rappel est utilisé pour passer le `IHostingEnvironment`. C’est utile pour déterminer le fichier JSON de configuration *appsettings* correct à surveiller, *appsettings.&lt;Environnement&gt;.json*. Les hachages de fichier sont utilisés pour empêcher l’instruction `WriteConsole` d’être exécutée plusieurs fois en raison de plusieurs rappels de jeton alors que le fichier de configuration n’a été modifié qu’une seule fois.

Ce système s’exécute tant que l’application est en cours d’exécution et ne peut pas être désactivé par l’utilisateur.

### <a name="monitoring-configuration-changes-as-a-service"></a>Surveillance des modifications de configuration en tant que service

L’exemple implémente :

* Surveillance de jeton du démarrage de base.
* Surveillance en tant que service.
* Un mécanisme pour activer et désactiver la surveillance.

L’exemple établit une interface `IConfigurationMonitor` (*Extensions/ConfigurationMonitor.cs*) :

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet1)]

Le constructeur de la classe implémentée, `ConfigurationMonitor`, inscrit un rappel pour les notifications de modification :

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet2)]

`config.GetReloadToken()` fournit le jeton. `InvokeChanged` est la méthode de rappel. Le `state` dans cette instance est une chaîne qui décrit l’état de la surveillance. Deux propriétés sont utilisées :

* `MonitoringEnabled` indique si le rappel doit exécuter son code personnalisé.
* `CurrentState` décrit l’état actuel de la surveillance pour une utilisation dans l’interface utilisateur.

La méthode `InvokeChanged` est similaire à l’approche précédente, excepté que :

* Elle n’exécute pas son code, sauf si `MonitoringEnabled` est `true`.
* Elle définit la chaîne de la propriété `CurrentState` avec un message descriptif qui enregistre l’heure à laquelle le code a été exécuté.
* Elle indique le `state` actuel dans sa sortie `WriteConsole`.

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet3)]

Une instance de `ConfigurationMonitor` est inscrite en tant que service dans `ConfigureServices` de *Startup.cs* :

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet1)]

La page Index permet à l’utilisateur de contrôler la surveillance de la configuration. L’instance de `IConfigurationMonitor` est injectée dans le `IndexModel` :

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet1)]

Un bouton active et désactive la surveillance :

[!code-cshtml[Main](change-tokens/sample/Pages/Index.cshtml?range=35)]

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet2)]

Quand `OnPostStartMonitoring` est déclenché, la surveillance est activée et l’état actuel est effacé. Quand `OnPostStopMonitoring` est déclenché, la surveillance est désactivée et l’état est défini de façon à indiquer que la surveillance n’est pas effectuée.

## <a name="monitoring-cached-file-changes"></a>Surveillance des modifications de fichiers mis en cache

Le contenu des fichiers peut être mis en cache en mémoire avec [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache). La mise en cache en mémoire est décrite dans la rubrique [Mise en cache en mémoire](xref:performance/caching/memory). Sans actions supplémentaires, comme l’implémentation décrite ci-dessous, les données *périmées* (obsolètes) sont retournées depuis un cache si la source de données change.

Le fait de ne pas prendre en compte l’état d’un fichier source en cache lors du renouvellement d’une période [d’expiration décalée](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) rend obsolètes les données du cache. Chaque demande de données renouvelle la période d’expiration décalée, mais le fichier n’est jamais rechargé dans le cache. Toutes les fonctionnalités d’une application qui utilisent le contenu mis en cache d’un fichier sont exposées au risque de recevoir du contenu obsolète.

L’utilisation de jetons de modification dans un scénario de mise en cache de fichier empêche la présence de contenu obsolète dans le cache. L’exemple d’application montre une implémentation de l’approche.

L’exemple utilise `GetFileContent` pour :

* Retourner le contenu du fichier.
* Implémenter un algorithme de nouvelle tentative avec interruption exponentielle pour couvrir les cas où un verrou de fichier empêche temporairement la lecture d’un fichier.

*Utilities/Utilities.cs* :

[!code-csharp[Main](change-tokens/sample/Utilities/Utilities.cs?name=snippet2)]

Un `FileService` est créé pour gérer les recherches des fichiers mis en cache. L’appel de la méthode `GetFileContent` du service tente d’obtenir le contenu du fichier à partir du cache en mémoire et le retourne à l’appelant (*Services/FileService.cs*).

Si le contenu en cache n’est pas trouvé avec la clé du cache, les actions suivantes sont effectuées :

1. Le contenu du fichier est obtenu à l’aide de `GetFileContent`.
1. Un jeton de modification est obtenu auprès du fournisseur du fichier avec [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch). Le rappel du jeton est déclenché quand le fichier est modifié.
1. Le contenu du fichier est mis en cache avec une période [d’expiration décalée](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration). Le jeton de modification est attaché avec [MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken) pour supprimer l’entrée du cache si le fichier change alors qu’il est mis en cache.

[!code-csharp[Main](change-tokens/sample/Services/FileService.cs?name=snippet1)]

Le `FileService` est inscrit dans le conteneur de service, ainsi que le service de mise en cache en mémoire (*Startup.cs*) :

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet4)]

Le modèle de page charge le contenu du fichier en utilisant le service (*Pages/Index.cshtml.cs*) :

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a>Classe CompositeChangeToken

Pour représenter une ou plusieurs instances de `IChangeToken` dans un même objet, utilisez la classe [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken) ([source de référence](https://github.com/aspnet/Common/blob/patch/2.0.1/src/Microsoft.Extensions.Primitives/CompositeChangeToken.cs)).

```csharp
var firstCancellationTokenSource = new CancellationTokenSource();
var secondCancellationTokenSource = new CancellationTokenSource();

var firstCancellationToken = firstCancellationTokenSource.Token;
var secondCancellationToken = secondCancellationTokenSource.Token;

var firstCancellationChangeToken = new CancellationChangeToken(firstCancellationToken);
var secondCancellationChangeToken = new CancellationChangeToken(secondCancellationToken);

var compositeChangeToken = 
    new CompositeChangeToken(
        new List<IChangeToken> 
        { 
            firstCancellationChangeToken, 
            secondCancellationChangeToken
        });
```

`HasChanged` sur le jeton composite indique `true` si l’élément `HasChanged` d’un jeton représenté est `true`. `ActiveChangeCallbacks` sur le jeton composite indique `true` si l’élément `ActiveChangeCallbacks` d’un jeton représenté est `true`. Si plusieurs modifications simultanées se produisent, le rappel de modification composite n’est appelé qu’une seule fois.

## <a name="see-also"></a>Voir aussi

* [Mise en cache en mémoire](xref:performance/caching/memory)
* [Utilisation d’un cache distribué](xref:performance/caching/distributed)
* [Détecter les modifications à l’aide de jetons de modification](xref:fundamentals/primitives/change-tokens)
* [Mise en cache des réponses](xref:performance/caching/response)
* [Intergiciel de mise en cache des réponses](xref:performance/caching/middleware)
* [Tag Helper de cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Tag Helper de cache distribué](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
