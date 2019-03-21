---
title: Opérations de demande et de réponse dans ASP.NET Core
author: jkotalik
description: Découvrez comment lire le corps de demande et écrire le corps de la réponse dans ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jkotalik
ms.custom: mvc
ms.date: 02/26/2019
uid: fundamentals/middleware/request-response
ms.openlocfilehash: b6e3cd4b79e0c062b271c65cd5ecbdb4ef80c3a1
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208055"
---
# <a name="request-and-response-operations-in-aspnet-core"></a>Opérations de demande et de réponse dans ASP.NET Core

Par [Justin Kotalik](https://github.com/jkotalik)

Cet article explique comment lire à partir du corps de la demande et écrire dans le corps de la réponse. Vous devrez peut-être écrire du code pour ces opérations lorsque vous écrivez le middleware. Sinon, il est en général inutile d’écrire ce code, car les opérations sont gérées par MVC et Razor Pages.

Dans ASP.NET Core 3.0, il existe deux abstractions pour les corps de la demande et de la réponse : <xref:System.IO.Stream> et <xref:System.IO.Pipelines.Pipe>. Pour la lecture de la demande, [HttpRequest.Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) est un <xref:System.IO.Stream>, et `HttpRequest.BodyPipe` est un <xref:System.IO.Pipelines.PipeReader>. Pour l’écriture de la réponse, [HttpResponse.Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body) est un `HttpResponse.BodyPipe` est un <xref:System.IO.Pipelines.PipeWriter>.

Nous recommandons d’utiliser des pipelines plutôt que des flux. Les flux peuvent être plus faciles à utiliser pour des opérations simples, mais les pipelines présentent un avantage de performances et sont plus faciles à utiliser dans la plupart des scénarios. Dans 3.0, ASP.NET Core commence à utiliser des pipelines au lieu de flux en interne. En voici quelques exemples :

- `FormReader`
- `TextReader`
- `TexWriter`
- `HttpResponse.WriteAsync`

Les flux ne sont pas amenée à disparaître. Ils continuent à être utilisés dans .NET et de nombreux types de flux n’ont d’équivalents en termes de canal, comme `FileStreams` et `ResponseCompression`.

## <a name="stream-examples"></a>Exemples de flux

Supposons que vous souhaitez créer un middleware qui lit le corps de la demande dans sa totalité comme une liste de chaînes, avec un fractionnement sur les nouvelles lignes. Une implémentation simple de flux peut se présenter comme dans l’exemple suivant :

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStream)]

Ce code fonctionne, mais il existe certains problèmes :

- Avant d’ajouter à `StringBuilder`, l’exemple crée une autre chaîne (`encodedString`) qui est immédiatement rejetée. Ce processus se produit pour tous les octets dans le flux, il en résulte une allocation de mémoire supplémentaire de la taille de la totalité du corps de la demande.
- L’exemple lit la chaîne entière avant de fractionner sur les nouvelles lignes. Il serait plus efficace de rechercher les nouvelles lignes dans le tableau d’octets.

Voici un exemple qui résout certains de ces problèmes :

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStreamMoreEfficient)]

Cet exemple :

- Ne met pas en mémoire tampon le corps entier de la demande dans un `StringBuilder` , sauf s’il n’y a pas de caractère de nouvelle ligne.
- N’appelle pas `Split` sur la chaîne.

Toutefois, il existe toujours quelques problèmes :

- Si les caractères de saut de ligne sont épars, une grande partie du corps de la demande est mis en mémoire tampon dans la chaîne.
- Cela crée toujours des chaînes (`remainingString`) et les ajoute à la mémoire tampon de chaîne, ce qui entraîne une allocation supplémentaire.

Ces problèmes sont réparables, mais le code est de plus en plus compliqué avec peu d’amélioration. Les pipelines permettent de résoudre ces problèmes avec un code peu compliqué.

## <a name="pipelines"></a>Pipelines

L’exemple suivant indique comment le même scénario peut être géré à l’aide d’un `PipeReader` :

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringFromPipe)]

Cet exemple résout de nombreux problèmes trouvés dans les implémentations de flux :

- Un mémoire tampon de chaîne est inutile, car le `PipeReader` gère des octets qui n’ont pas été utilisés.
- Les chaînes codées sont ajoutées directement à la liste des chaînes retournées.
- La création de chaînes est libre d’allocation en dehors de la mémoire utilisée par la chaîne (à l’exception de l’appel `ToArray()`).

## <a name="adapters"></a>Adaptateurs

Maintenant que les propriétés `Body` et `BodyPipe` sont disponibles pour `HttpRequest` et `HttpResponse`, que se passe-t-il lorsque vous définissez `Body` sur un autre flux ? Dans 3.0, un nouvel ensemble d’adaptateurs adaptent automatiquement chaque type à l’autre. Par exemple, si vous définissez `HttpRequest.Body` sur un nouveau flux, `HttpRequest.BodyPipe` est automatiquement défini sur un nouveau `PipeReader` qui encapsule `HttpRequest.Body`. Le même comportement s’applique au paramètre de la propriété `BodyPipe`. Si `HttpResponse.BodyPipe` est défini sur un nouveau `PipeWriter`, le `HttpResponse.Body` est automatiquement défini sur un flux qui encapsule `HttpResponse.BodyPipe`.

## <a name="startasync"></a>StartAsync

`HttpResponse.StartAsync` est nouveau dans 3.0. Il est utilisé pour indiquer que les en-têtes sont non modifiables et pour exécuter des rappels `OnStarting`. Dans 3.0-preview3, vous devez appeler `StartAsync` avant d’utiliser `HttpRequest.BodyPipe`, et dans les versions futures, ce sera une recommandation. Lors de l’utilisation de Kestrel en tant que serveur, l’appel de StartAsync avant d’utiliser le `PipeReader` garantit que la mémoire retournée par `GetMemory` appartiendra au <xref:System.IO.Pipelines.Pipe> interne de Kestrel au lieu d’une mémoire tampon externe.

## <a name="additional-resources"></a>Ressources supplémentaires

- [Présentation de System.IO.Pipelines](https://devblogs.microsoft.com/dotnet/system-io-pipelines-high-performance-io-in-net/)
- <xref:fundamentals/middleware/write>