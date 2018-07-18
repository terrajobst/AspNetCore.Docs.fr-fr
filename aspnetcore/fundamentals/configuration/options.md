---
title: Modèle d’options dans ASP.NET Core
author: guardrex
description: Découvrez comment utiliser le modèle d’options pour représenter des groupes de paramètres associés dans les applications ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/28/2017
uid: fundamentals/configuration/options
ms.openlocfilehash: c996ac6ab05b98bcca72d0993fe412f553b58106
ms.sourcegitcommit: 19cbda409bdbbe42553dc385ea72d2a8e246509c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38992962"
---
# <a name="options-pattern-in-aspnet-core"></a>Modèle d’options dans ASP.NET Core

Par [Luke Latham](https://github.com/guardrex)

Le modèle d’options utilise des classes pour représenter les groupes de paramètres associés. Quand les paramètres de configuration sont isolés par fonctionnalité dans des classes distinctes, l’application est conforme à deux principes d’ingénierie logicielle importants :

* [Principe de séparation des interfaces](http://deviq.com/interface-segregation-principle/) : les fonctionnalités (classes) qui dépendent de paramètres de configuration dépendent uniquement de ceux qu’elles utilisent.
* [Séparation des préoccupations](http://deviq.com/separation-of-concerns/) : les paramètres des différentes parties de l’application ne sont pas dépendants ou associés les uns aux autres.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample)) Cet article est plus facile à suivre avec l’exemple d’application.

## <a name="basic-options-configuration"></a>Configuration des options de base

La configuration des options de base est illustrée dans l’exemple &num;1 de [l’exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Une classe d’options doit être non abstraite avec un constructeur public sans paramètre. La classe suivante, `MyOptions`, a deux propriétés : `Option1` et `Option2`. Définir des valeurs par défaut est facultatif, mais le constructeur de classe dans l’exemple suivant définit la valeur par défaut `Option1`. `Option2` a une valeur par défaut définie par l’initialisation directe de la propriété (*Models/MyOptions.cs*) :

[!code-csharp[](options/sample/Models/MyOptions.cs?name=snippet1)]

La classe `MyOptions` est ajoutée au conteneur de service avec [Configure&lt;TOptions&gt;(IServiceCollection, IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) et liée à la configuration :

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example1)]

Le modèle de page suivant utilise une [injection de dépendances de constructeur](xref:fundamentals/dependency-injection#what-is-dependency-injection) avec [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) pour accéder aux paramètres (*Pages/Index.cshtml.cs*) :

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

Le fichier *appsettings.json* de l’exemple spécifie des valeurs pour `option1` et `option2` :

[!code-json[](options/sample/appsettings.json?highlight=2-3)]

Quand l’application est exécutée, la méthode `OnGet` du modèle de page retourne une chaîne indiquant les valeurs de la classe d’options :

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> Quand vous utilisez un [ConfigurationBuilder](/dotnet/api/system.configuration.configurationbuilder) personnalisé pour charger une configuration d’options à partir d’un fichier de paramètres, vérifiez que le chemin de base est correctement défini :
>
> ```csharp
> var configBuilder = new ConfigurationBuilder()
>    .SetBasePath(Directory.GetCurrentDirectory())
>    .AddJsonFile("appsettings.json", optional: true);
> var config = configBuilder.Build();
>
> services.Configure<MyOptions>(config);
> ```
>
> Vous n’avez pas besoin de définir explicitement le chemin de base quand vous chargez une configuration d’options à partir du fichier de paramètres via [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).

## <a name="configure-simple-options-with-a-delegate"></a>Configurer des options simples avec un délégué

La configuration d’options simples avec un délégué est illustrée dans l’exemple &num;2 de [l’exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Utilisez un délégué pour définir les valeurs des options. L’exemple d’application utilise la classe `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*) :

[!code-csharp[](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

Dans le code suivant, un second service `IConfigureOptions<TOptions>` est ajouté au conteneur de services. Il utilise un délégué pour configurer la liaison avec `MyOptionsWithDelegateConfig` :

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example2)]

*Index.cshtml.cs* :

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

Vous pouvez ajouter plusieurs fournisseurs de configuration. Des fournisseurs de configuration sont disponibles dans les packages NuGet. Ils sont appliqués dans l’ordre dans lequel ils sont inscrits.

Chaque appel à [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) ajoute un service `IConfigureOptions<TOptions>` au conteneur de services. Dans l’exemple précédent, les valeurs de `Option1` et `Option2` sont toutes deux spécifiées dans *appsettings.json*, mais les valeurs de `Option1` et `Option2` sont remplacées par le délégué configuré.

Quand plusieurs services de configuration sont activés, la dernière source de configuration spécifiée *gagne* et définit la valeur de configuration. Quand l’application est exécutée, la méthode `OnGet` du modèle de page retourne une chaîne indiquant les valeurs de la classe d’options :

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a>Configuration des sous-options

La configuration des sous-options est illustrée dans l’exemple &num;3 de [l’exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Les applications doivent créer des classes d’options qui appartiennent à des groupes de fonctionnalités spécifiques (classes) dans l’application. Les parties de l’application qui requièrent des valeurs de configuration ne doivent avoir accès qu’aux valeurs de configuration qu’elles utilisent.

Au moment de la liaison des options à la configuration, chaque propriété du type options est liée à une clé de configuration sous la forme `property[:sub-property:]`. Par exemple, la propriété `MyOptions.Option1` est liée à la clé `Option1`, qui est lue à partir de la propriété `option1` dans *appsettings.json*.

Dans le code suivant, un troisième service `IConfigureOptions<TOptions>` est ajouté au conteneur de services. Il lie `MySubOptions` à la section `subsection` du fichier *appsettings.json* :

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example3)]

La méthode d’extension `GetSection` requiert le package NuGet [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/). Si l’application utilise le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 ou version ultérieure), le package est automatiquement inclus.

Le fichier *appsettings.json* de l’exemple définit un membre `subsection` avec des clés pour `suboption1` et `suboption2` :

[!code-json[](options/sample/appsettings.json?highlight=4-7)]

La classe `MySubOptions` définit des propriétés, `SubOption1` et `SubOption2`, pour conserver les valeurs de sous-options (*Models/MySubOptions.cs*) :

[!code-csharp[](options/sample/Models/MySubOptions.cs?name=snippet1)]

La méthode `OnGet` du modèle de page retourne une chaîne avec les valeurs de sous-option (*Pages/Index.cshtml.cs*) :

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

Quand l’application est exécutée, la méthode `OnGet` retourne une chaîne indiquant les valeurs de la classe de sous-options :

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a>Options fournies par un modèle d’affichage ou avec une injection de vue directe

Les options fournies par un modèle d’affichage ou avec une injection de vue directe sont illustrées dans l’exemple &num;4 de [l’exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Vous pouvez fournir les options dans un modèle d’affichage ou en injectant `IOptions<TOptions>` directement dans une vue (*Pages/Index.cshtml.cs*) :

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

Pour l’injection directe, injectez `IOptions<MyOptions>` avec une directive `@inject` :

[!code-cshtml[](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

Quand l’application est exécutée, les valeurs d’options sont affichées dans la page rendue :

![Les valeurs d’options Option1: value1_from_json et Option2: -1 sont chargées à partir du modèle et par injection dans la vue.](options/_static/view.png)

::: moniker range=">= aspnetcore-1.1"

## <a name="reload-configuration-data-with-ioptionssnapshot"></a>Recharger les données de configuration avec IOptionsSnapshot

Le rechargement des données de configuration avec `IOptionsSnapshot` est illustré dans l’exemple &num;5 de [l’exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) prend en charge le rechargement des options avec une charge de traitement minimale.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

Les options sont calculées une fois par requête quand le système y accède et sont mises en cache pour toute la durée de vie de la requête.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

`IOptionsSnapshot` est un instantané d’[IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) et se met à jour automatiquement chaque fois que le moniteur déclenche des changements liés à la modification de la source de données.

::: moniker-end

::: moniker range=">= aspnetcore-1.1"

Dans l’exemple suivant, un `IOptionsSnapshot` est créé à la suite de la modification du fichier *appsettings.json* (*Pages/Index.cshtml.cs*). Plusieurs demandes adressées au serveur retournent des valeurs constantes fournies par le fichier *appsettings.json* tant que ce dernier n’est pas changé et que la configuration n’est pas rechargée.

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

L’image suivante montre les valeurs `option1` et `option2` initiales chargées à partir du fichier *appsettings.json* :

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

Changez les valeurs dans le fichier *appsettings.json* en `value1_from_json UPDATED` et `200`. Enregistrez le fichier *appsettings.json*. Actualisez le navigateur pour constater la mise à jour des valeurs des options :

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

## <a name="named-options-support-with-iconfigurenamedoptions"></a>Prise en charge des options nommées avec IConfigureNamedOptions

La prise en charge des options nommées avec [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) est illustrée dans l’exemple &num;6 de [l’exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

La prise en charge des *options nommées* permet à l’application de faire la distinction entre les configurations d’options nommées. Dans l’exemple d’application, les options nommées sont déclarées avec [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(IServiceCollection, String, Action&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure), qui appelle ensuite la méthode d’extension [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) :

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example6)]

L’exemple d’application accède aux options nommées avec [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*) :

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

Quand l’exemple d’application est exécuté, les options nommées sont retournées :

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

Les valeurs `named_options_1`, issues de la configuration, sont chargées à partir du fichier *appsettings.json*. Les valeurs `named_options_2` sont fournies par :

* Le délégué `named_options_2` dans `ConfigureServices` pour `Option1`.
* La valeur par défaut `Option2` fournie par la classe `MyOptions`.

Configurez toutes les instances d’options nommées avec la méthode [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall). Le code suivant configure `Option1` pour toutes les instances de configuration nommées ayant une valeur commune. Ajoutez le code suivant manuellement à la méthode `Configure` :

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

Exécuter l’exemple d’application après avoir ajouté le code produit le résultat suivant :

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> Toutes les options sont des instances nommées. Les instances `IConfigureOption` existantes sont traitées comme ciblant l’instance `Options.DefaultName`, qui est `string.Empty`. En outre, `IConfigureNamedOptions` implémente `IConfigureOptions`. L’implémentation par défaut de [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([source de référence](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs)) a une logique qui permet de les utiliser chacune de façon appropriée. L’option nommée `null` est utilisée pour cibler toutes les instances nommées au lieu d’une instance nommée spécifique ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) et [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) utilisent cette convention).

## <a name="ipostconfigureoptions"></a>IPostConfigureOptions

Définir la post-configuration avec [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1). La post-configuration s’exécute après la totalité de la configuration [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) :

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) permet de post-configurer les options nommées :

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

Utilisez [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) pour post-configurer toutes les instances de configuration nommées :

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

::: moniker-end

## <a name="options-factory-monitoring-and-cache"></a>Fabrique d’options, surveillance et cache

[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) est utilisé pour les notifications quand des instances `TOptions` changent. `IOptionsMonitor` prend en charge les options rechargeables, les notifications de modifications et `IPostConfigureOptions`.

::: moniker range=">= aspnetcore-2.0"

[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) est chargé de créer les instances d’options. Elle dispose d’une seule méthode ([Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create)). L’implémentation par défaut prend toutes les `IConfigureOptions` et `IPostConfigureOptions` inscrites et exécute toutes les configurations, puis les post-configurations. Elle fait la distinction entre `IConfigureNamedOptions` et `IConfigureOptions` et n’appelle que l’interface appropriée.

[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) est utilisé par `IOptionsMonitor` pour la mise en cache des instances `TOptions`. `IOptionsMonitorCache` invalide les instances des options dans le moniteur afin que la valeur soit recalculée ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)). Les valeurs peuvent aussi être introduites manuellement avec [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd). La méthode [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) est utilisée quand toutes les instances nommées doivent être recréées à la demande.

::: moniker-end

## <a name="accessing-options-during-startup"></a>Accès aux options au démarrage

`IOptions` peut être utilisé dans `Configure`, dans la mesure où les services sont générés avant que la méthode `Configure` ne s’exécute. Si un fournisseur de services est créé dans `ConfigureServices` pour accéder aux options, il ne contient aucune configuration d’options fournie après sa création. Ainsi, un état d’options incohérent peut exister en raison de l’ordre des inscriptions de service.

Les options étant généralement chargées à partir de la configuration, celle-ci peut être utilisée au démarrage à la fois dans `Configure` et `ConfigureServices`. Pour obtenir des exemples d’utilisation de la configuration au démarrage, consultez la rubrique [Démarrage de l’application](xref:fundamentals/startup).

## <a name="see-also"></a>Voir aussi

* [Configuration](xref:fundamentals/configuration/index)
