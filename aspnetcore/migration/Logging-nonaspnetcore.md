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
# <a name="migrate-from-microsoftextensionslogging-21-to-22-or-30"></a><span data-ttu-id="895b0-103">Migrer à partir de Microsoft.Extensions.Logging 2.1 vers 2.2 ou 3.0</span><span class="sxs-lookup"><span data-stu-id="895b0-103">Migrate from Microsoft.Extensions.Logging 2.1 to 2.2 or 3.0</span></span>

<span data-ttu-id="895b0-104">Cet article décrit les étapes courantes pour la migration d’une application non - ASP.NET Core qui utilise `Microsoft.Extensions.Logging` 2.1 vers 2.2 ou 3.0.</span><span class="sxs-lookup"><span data-stu-id="895b0-104">This article outlines the common steps for migrating a non-ASP.NET Core application that uses `Microsoft.Extensions.Logging` from 2.1 to 2.2 or 3.0.</span></span>

## <a name="21-to-22"></a><span data-ttu-id="895b0-105">2.1 à 2.2</span><span class="sxs-lookup"><span data-stu-id="895b0-105">2.1 to 2.2</span></span>

<span data-ttu-id="895b0-106">Créer manuellement `ServiceCollection` et appelez `AddLogging`.</span><span class="sxs-lookup"><span data-stu-id="895b0-106">Manually create `ServiceCollection` and call `AddLogging`.</span></span>

<span data-ttu-id="895b0-107">exemple 2.1 :</span><span class="sxs-lookup"><span data-stu-id="895b0-107">2.1 example:</span></span>

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

<span data-ttu-id="895b0-108">exemple 2.2 :</span><span class="sxs-lookup"><span data-stu-id="895b0-108">2.2 example:</span></span>

```csharp
var serviceCollection = new ServiceCollection();
serviceCollection.AddLogging(builder => builder.AddConsole());

using (var serviceProvider = serviceCollection.BuildServiceProvider())
using (var loggerFactory = serviceProvider.GetService<ILoggerFactory>())
{
    // use loggerFactory
}
```

## <a name="21-to-30"></a><span data-ttu-id="895b0-109">2.1 à 3.0</span><span class="sxs-lookup"><span data-stu-id="895b0-109">2.1 to 3.0</span></span>

<span data-ttu-id="895b0-110">Dans 3.0, utilisez `LoggingFactory.Create`.</span><span class="sxs-lookup"><span data-stu-id="895b0-110">In 3.0, use `LoggingFactory.Create`.</span></span>

<span data-ttu-id="895b0-111">exemple 2.1 :</span><span class="sxs-lookup"><span data-stu-id="895b0-111">2.1 example:</span></span>

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

<span data-ttu-id="895b0-112">exemple 3.0 :</span><span class="sxs-lookup"><span data-stu-id="895b0-112">3.0 example:</span></span>

```csharp
using (var loggerFactory = LoggerFactory.Create(builder => builder.AddConsole()))
{
    // use loggerFactory
}
```

## <a name="additional-resources"></a><span data-ttu-id="895b0-113">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="895b0-113">Additional resources</span></span>

<xref:fundamentals/logging/index>