---
uid: web-api/overview/error-handling/exception-handling
title: Gestion des exceptions dans l’API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 03/12/2012
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: 2db00c1ab241e0dad051c2a3bcd3b0e4bbf4d5c4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807659"
---
<a name="exception-handling-in-aspnet-web-api"></a>Gestion des exceptions dans l’API Web ASP.NET
====================
par [Mike Wasson](https://github.com/MikeWasson)

Cet article décrit l’erreur et gestion des exceptions dans l’API Web ASP.NET.

- [HttpResponseException](#httpresponserexception)
- [Filtres d’exception](#exception_filters)
- [L’inscription des filtres d’Exception](#registering_exception_filters)
- [Erreur HTTP](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a>HttpResponseException

Que se passe-t-il si un contrôleur Web API lève une exception non interceptée ? Par défaut, la plupart des exceptions sont traduites en une réponse HTTP avec le code d’état 500, erreur interne du serveur.

Le **HttpResponseException** type est un cas spécial. Cette exception retourne n’importe quel code d’état HTTP que vous spécifiez dans le constructeur d’exception. Par exemple, la méthode suivante retourne 404, non trouvé, si le *id* paramètre n’est pas valide.

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

Pour mieux contrôler la réponse, vous pouvez également construire le message de réponse entière et l’inclure avec le **HttpResponseException :** 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a>Filtres d’exception

Vous pouvez personnaliser la façon dont les API Web gère les exceptions en écrivant un *filtre d’exception*. Un filtre d’exception est exécuté quand une méthode de contrôleur lève une exception non gérée qui est *pas* un **HttpResponseException** exception. Le **HttpResponseException** type est un cas spécial, car il est spécifiquement conçu pour renvoyer une réponse HTTP.

Filtres d’exception implémentent la **System.Web.Http.Filters.IExceptionFilter** interface. La méthode la plus simple pour écrire un filtre d’exception consiste à dériver à partir de la **System.Web.Http.Filters.ExceptionFilterAttribute** classe et substituer les **OnException** (méthode).

> [!NOTE]
> Filtres d’exception dans l’API Web ASP.NET sont semblables à celles dans ASP.NET MVC. Toutefois, ils sont déclarés dans un espace de noms distinct et une fonction séparément. En particulier, le **HandleErrorAttribute** classe utilisée dans MVC ne gère pas les exceptions levées par les contrôleurs d’API Web.


Voici un filtre qui convertit **NotImplementedException** exceptions dans l’état HTTP 501, non implémenté de code :

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

Le **réponse** propriété de la **HttpActionExecutedContext** objet contient le message de réponse HTTP qui sera envoyé au client.

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a>L’inscription des filtres d’Exception

Il existe plusieurs façons d’inscrire un filtre d’exception API Web :

- Par action
- Par contrôleur
- Dans le monde entier

Pour appliquer le filtre à une action spécifique, ajoutez le filtre en tant qu’attribut à l’action :

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

Pour appliquer le filtre à toutes les actions sur un contrôleur, ajoutez le filtre en tant qu’attribut à la classe de contrôleur :

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

Pour appliquer le filtre globalement à tous les contrôleurs d’API Web, ajoutez une instance du filtre à la **GlobalConfiguration.Configuration.Filters** collection. Filtres d’exception dans cette collection s’appliquent à une action de contrôleur d’API Web.

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

Si vous utilisez le modèle de projet « Application Web ASP.NET MVC 4 » pour créer votre projet, placez votre code de configuration d’API Web à l’intérieur de la `WebApiConfig` (classe), qui se trouve dans l’application\_dossier de démarrage :

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a>Erreur HTTP

Le **HttpError** objet fournit un moyen cohérent de renvoyer des informations d’erreur dans le corps de réponse. L’exemple suivant montre comment retourner le code d’état HTTP 404 (introuvable) avec un **HttpError** dans le corps de réponse.

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

**CreateErrorResponse** est une méthode d’extension définie dans le **System.Net.Http.HttpRequestMessageExtensions** classe. En interne, **CreateErrorResponse** crée un **HttpError** de l’instance, puis crée un **HttpResponseMessage** qui contient le **HttpError**.

Dans cet exemple, si la méthode réussite, elle retourne le produit dans la réponse HTTP. Mais si le produit demandé est introuvable, la réponse HTTP contient un **HttpError** dans le corps de la demande. La réponse peut se présenter comme suit :

[!code-console[Main](exception-handling/samples/sample9.cmd)]

Notez que le **HttpError** a été sérialisé au format JSON dans cet exemple. L’un des avantages de l’utilisation de **HttpError** est qu’il traverse le même [négociation de contenu](../formats-and-model-binding/content-negotiation.md) et processus de sérialisation comme tout autre modèle fortement typé.

### <a name="httperror-and-model-validation"></a>Erreur HTTP et la Validation du modèle

Pour la validation de modèle, vous pouvez passer à l’état du modèle **CreateErrorResponse**, pour inclure les erreurs de validation dans la réponse :

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

Cet exemple peut renvoyer la réponse suivante :

[!code-console[Main](exception-handling/samples/sample11.cmd)]

Pour plus d’informations sur la validation de modèle, consultez [Validation de modèle dans ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).

### <a name="using-httperror-with-httpresponseexception"></a>À l’aide d’erreur HTTP avec HttpResponseException

Les exemples précédents retournent un **HttpResponseMessage** message à partir de l’action du contrôleur, mais vous pouvez également utiliser **HttpResponseException** pour retourner un **HttpError**. Cela vous permet de renvoyer un modèle fortement typé dans le cas normal de réussite, tout en continuant à renvoyer **HttpError** si une erreur se produit :

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
