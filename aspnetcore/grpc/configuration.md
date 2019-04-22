---
title: gRPC pour la configuration d’ASP.NET Core
author: jamesnk
description: Découvrez comment configurer gRPC pour les applications ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 04/09/2019
uid: grpc/configuration
ms.openlocfilehash: 66dfb9ec136616f10c1b7aaad766e18813b87de4
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59983131"
---
# <a name="grpc-for-aspnet-core-configuration"></a>gRPC pour la configuration d’ASP.NET Core

## <a name="configure-services-options"></a>Configurer les options de services

Le tableau suivant décrit les options de configuration des services de gRPC :

| Option | Valeur par défaut | Description |
| ------ | ------------- | ----------- |
| `SendMaxMessageSize` | `null` | La taille maximale en octets qui peuvent être envoyés à partir du serveur. Essayez d’envoyer un message qui dépasse les résultats de la taille maximale de message dans une exception. |
| `ReceiveMaxMessageSize` | 4 MB | La taille maximale en octets qui peuvent être reçus par le serveur. Si le serveur reçoit un message qui dépasse cette limite, elle lève une exception. Augmentation de cette valeur permet au serveur recevoir des messages plus volumineux, mais peut affecter négativement la consommation de mémoire. |
| `EnableDetailedErrors` | `false` | Si `true`et détaillée des messages d’exception sont retournées aux clients quand une exception est levée dans une méthode de service. La valeur par défaut est `false`. Définir cette valeur sur `true` peut entraîner une fuite des informations sensibles. |
| `CompressionProviders` | gzip | Une collection de fournisseurs de compression utilisé pour compresser et décompresser des messages. Fournisseurs de compression personnalisé peuvent être créés et ajoutés à la collection. La valeur par défaut configuré le fournisseur prend en charge **gzip** la compression. |
| `ResponseCompressionAlgorithm` | `null` | L’algorithme de compression utilisé pour compresser les messages envoyés à partir du serveur. L’algorithme doit correspondre à un fournisseur de la compression dans `CompressionProviders`. Pour l’algorthm compresser une réponse le client doit indiquer qu’il prend en charge l’algorithme en lui envoyant le **grpc-encodage** en-tête. |
| `ResponseCompressionLevel` | `null` | Le niveau de compression utilisé pour compresser les messages envoyés à partir du serveur. |

Options peuvent être configurées pour tous les services en fournissant un délégué d’options pour le `AddGrpc` appeler dans `Startup.ConfigureServices`.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.EnableDetailedErrors = true;
    });
}
```

Options pour un seul service remplacent les options globales fournies dans `AddGrpc` et peuvent être configurés à l’aide de `AddServiceOptions<TService>`:

```csharp
services.AddGrpc().AddServiceOptions<MyService>(options =>
{
    options.ReceiveMaxMessageSize = 10 * 1024 * 1024; // 10 megabytes
});
```

## <a name="configure-kestrel-options"></a>Configurer des options Kestrel

Serveur kestrel a des options de configuration qui affectent le comportement de gRPC pour ASP.NET.

### <a name="request-body-data-rate-limit"></a>Limite de débit de données de corps de la demande

Par défaut, le serveur Kestrel impose un [débit de données du corps de demande minimale](
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>). Pour le client de diffusion en continu et le mode duplex de la diffusion en continu des appels, ce taux ne peut pas être satisfait, et la connexion peut être expiré. La valeur minimale de la demande du corps de la limite de débit doit être désactivé lorsque le service gRPC inclut le client de diffusion en continu et duplex de diffusion en continu des appels :

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
         Host.CreateDefaultBuilder(args)
    .ConfigureWebHostDefaults(webBuilder =>
    {
        webBuilder.UseStartup<Startup>();
        webBuilder.ConfigureKestrel((context, options) =>
        {
            options.Limits.MinRequestBodyDataRate = null;
        });
    });
}
```

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
