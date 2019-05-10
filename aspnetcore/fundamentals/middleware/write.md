---
title: Écrire un intergiciel (middleware) ASP.NET Core personnalisé
author: rick-anderson
description: Découvrez comment écrire un intergiciel (middleware) ASP.NET Core personnalisé.
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2019
uid: fundamentals/middleware/write
ms.openlocfilehash: 2c5577394a10370d92c8a83f9d806b63f3245c8b
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64889164"
---
# <a name="write-custom-aspnet-core-middleware"></a>Écrire un intergiciel (middleware) ASP.NET Core personnalisé

Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Steve Smith](https://ardalis.com/)

Un middleware est un logiciel qui est assemblé dans un pipeline d’application pour gérer les requêtes et les réponses. Même si ASP.NET Core propose une large palette de composants d’intergiciel (middleware) intégrés, dans certains scénarios, vous voudrez certainement écrire un intergiciel personnalisé.

## <a name="middleware-class"></a>Classe d’intergiciel (middleware)

Les intergiciels sont généralement encapsulés dans une classe et exposés avec une méthode d’extension. Prenez en compte le middleware suivant, qui définit la culture de la requête actuelle à partir d’une chaîne de requête :

[!code-csharp[](index/snapshot/Culture/StartupCulture.cs?name=snippet1)]

L’exemple de code précédent est utilisé pour illustrer la création d’un composant de middleware. Pour plus d’informations sur la prise en charge intégrée de la localisation par ASP.NET Core, consultez <xref:fundamentals/localization>.

Vous pouvez tester l’intergiciel en transmettant la culture. Par exemple, `http://localhost:7997/?culture=no`.

Le code suivant déplace le délégué de l’intergiciel dans une classe :

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddleware.cs)]

::: moniker range="< aspnetcore-2.0"

Le nom de la méthode `Task` du middleware doit être `Invoke`. Dans ASP.NET Core 2.0 ou ultérieur, le nom peut être `Invoke` ou `InvokeAsync`.

::: moniker-end

## <a name="middleware-extension-method"></a>Méthode d’extension d’intergiciel

La méthode d’extension suivante expose le middleware par le biais de <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> :

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddlewareExtensions.cs)]

Le code suivant appelle l’intergiciel à partir de `Startup.Configure` :

[!code-csharp[](index/snapshot/Culture/Startup.cs?name=snippet1&highlight=5)]

L’intergiciel doit suivre le [principe de dépendances explicites](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) en exposant ses dépendances dans son constructeur. L’intergiciel est construit une fois par *durée de vie d’application*. Consultez la section [Dépendances par requête](#per-request-dependencies) si vous avez besoin de partager des services avec un middleware au sein d’une requête.

Les composants de middleware peuvent résoudre leurs dépendances à partir de l’[injection de dépendances](xref:fundamentals/dependency-injection) à l’aide des paramètres du constructeur. [UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) peut également accepter des paramètres supplémentaires directement.

## <a name="per-request-dependencies"></a>Dépendances par requête

Étant donné que le middleware est construit au démarrage de l’application, et non par requête, les services de durée de vie *délimités* utilisés par les constructeurs du middleware ne sont pas partagés avec d’autres types injectés par des dépendances lors de chaque requête. Si vous devez partager un service *délimité* entre votre intergiciel et d’autres types, ajoutez-le à la signature de la méthode `Invoke`. La méthode `Invoke` peut accepter des paramètres supplémentaires qui sont renseignés par injection de dépendances :

```csharp
public class CustomMiddleware
{
    private readonly RequestDelegate _next;

    public CustomMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    // IMyScopedService is injected into Invoke
    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:fundamentals/middleware/index>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
