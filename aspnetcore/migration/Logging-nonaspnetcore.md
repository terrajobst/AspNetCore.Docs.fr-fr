---
title: Migrer à partir de Microsoft.Extensions.Logging 2.1 vers 2.2 ou 3.0
author: pakrym
description: Découvrez comment migrer une application non - ASP.NET Core qui utilise Microsoft.Extensions.Logging 2.1 vers 2.2 ou 3.0.
ms.author: pakrym
ms.custom: mvc
ms.date: 01/04/2019
uid: migration/logging-nonaspnetcore
ms.openlocfilehash: 2519ddc02cee5978483bcaef4341a52aad3ba2a6
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892456"
---
# <a name="migrate-from-microsoftextensionslogging-21-to-22-or-30"></a>Migrer à partir de Microsoft.Extensions.Logging 2.1 vers 2.2 ou 3.0

Cet article décrit les étapes courantes pour la migration d’une application non - ASP.NET Core qui utilise `Microsoft.Extensions.Logging` 2.1 vers 2.2 ou 3.0.

## <a name="21-to-22"></a>2.1 à 2.2

Créer manuellement `ServiceCollection` et appelez `AddLogging`.

exemple 2.1 :

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

exemple 2.2 :

```csharp
var serviceCollection = new ServiceCollection();
serviceCollection.AddLogging(builder => builder.AddConsole());

using (var serviceProvider = serviceCollection.BuildServiceProvider())
using (var loggerFactory = serviceProvider.GetService<ILoggerFactory>())
{
    // use loggerFactory
}
```

## <a name="21-to-30"></a>2.1 à 3.0

Dans 3.0, utilisez `LoggingFactory.Create`.

exemple 2.1 :

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

exemple 3.0 :

```csharp
using (var loggerFactory = LoggerFactory.Create(builder => builder.AddConsole()))
{
    // use loggerFactory
}
```

## <a name="additional-resources"></a>Ressources supplémentaires

<xref:fundamentals/logging/index>