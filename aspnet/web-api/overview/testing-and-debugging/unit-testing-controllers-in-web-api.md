---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: Tests unitaires des contrôleurs dans ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Cette rubrique décrit certaines techniques spécifiques pour les tests unitaires des contrôleurs dans Web API 2. Avant de lire cette rubrique, il est conseillé de lire le didacticiel unité …
ms.author: riande
ms.date: 06/11/2014
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7b0d5266757219a05b25fc3d1d4cba8514a4dff7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826569"
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a>Tests unitaires des contrôleurs dans ASP.NET Web API 2
====================
par [Mike Wasson](https://github.com/MikeWasson)

> Cette rubrique décrit certaines techniques spécifiques pour les tests unitaires des contrôleurs dans Web API 2. Avant de lire cette rubrique, vous pouvez souhaiter lire le didacticiel [Unit Test API Web ASP.NET 2](unit-testing-with-aspnet-web-api.md), qui montre comment ajouter un projet de test unitaire à votre solution.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
> 
> - [Visual Studio 2017](https://www.visualstudio.com/vs/)
> - Web API 2
> - [Moq](https://github.com/Moq) 4.5.30

> [!NOTE]
> J’ai utilisé Moq, mais le même principe s’applique à n’importe quel framework de simulation. Moq 4.5.30 (et versions ultérieures) prend en charge de Visual Studio 2017, Roslyn et .NET 4.5 et versions ultérieures.

Il est courant dans les tests unitaires &quot;réorganiser act-assert&quot;:

- Organiser : Définir des conditions préalables pour le test s’exécute.
- ACT : Effectuer le test.
- Assert : Vérifiez que le test a réussi.

Dans l’étape d’organisation, vous souvent utiliser simulacre ou objets du stub. Qui réduit le nombre de dépendances, donc le test se concentre sur le test d’une chose.

Voici quelques points que vous devez le test unitaire dans vos contrôleurs d’API Web :

- L’action retourne le type de réponse approprié.
- Paramètres non valides renvoient la réponse d’erreur approprié.
- L’action appelle la méthode correcte sur la couche de service ou le référentiel.
- Si la réponse inclut un modèle de domaine, vérifiez le type de modèle.

Voici quelques-unes des choses générales pour tester, mais les spécificités dépendent de votre implémentation de contrôleur. En particulier, il fait une différence énorme si vos actions de contrôleur retournent **HttpResponseMessage** ou **IHttpActionResult**. Pour plus d’informations sur ces types de résultats, consultez [résultats d’Action dans Web Api 2](../getting-started-with-aspnet-web-api/action-results.md).

## <a name="testing-actions-that-return-httpresponsemessage"></a>Test des Actions qui retournent HttpResponseMessage

Voici un exemple d’un contrôleur dont retour actions **HttpResponseMessage**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

Notez que le contrôleur utilise l’injection de dépendances pour injecter un `IProductRepository`. Le contrôleur ainsi plus faciles à tester, étant donné que vous pouvez injecter un référentiel factice. Le test unitaire suivant vérifie que le `Get` méthode écrit un `Product` pour le corps de réponse. Supposons que `repository` est un simulacre `IProductRepository`.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

Il est important de définir **demande** et **Configuration** sur le contrôleur. Sinon, le test échoue avec une **ArgumentNullException** ou **InvalidOperationException**.

## <a name="testing-link-generation"></a>Génération de lien de test

Le `Post` les appels de méthode **UrlHelper.Link** pour créer des liens dans la réponse. Cela nécessite un peu plus de programme d’installation dans le test unitaire :

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

Le **UrlHelper** classe a besoin des données URL et l’itinéraire de requête, le test doit donc définir des valeurs pour ces. Une autre option consiste simulacre ou stub **UrlHelper**. Avec cette approche, vous remplacez la valeur par défaut [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) avec une version de simulacre ou stub qui retourne une valeur fixe.

Réécrivons le test à l’aide de la [Moq](https://github.com/Moq) framework. Installer le `Moq` package NuGet dans le projet de test.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

Dans cette version, vous n’avez pas besoin configurer les données d’itinéraire, étant donné que le simulacre **UrlHelper** retourne une chaîne constante.


## <a name="testing-actions-that-return-ihttpactionresult"></a>Test des Actions qui retournent IHttpActionResult

Dans Web API 2, une action de contrôleur peut retourner **IHttpActionResult**, ce qui est analogue à **ActionResult** dans ASP.NET MVC. Le **IHttpActionResult** interface définit un modèle de commande pour créer des réponses HTTP. Au lieu de créer directement la réponse, le contrôleur retourne un **IHttpActionResult**. Plus tard, le pipeline appelle le **IHttpActionResult** pour créer la réponse. Cette approche rend plus facile d’écrire des tests unitaires, car vous pouvez ignorer le programme d’installation est nécessaire pour de nombreuses **HttpResponseMessage**.

Voici un exemple de contrôleur dont retour actions **IHttpActionResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

Cet exemple montre quelques modèles courants à l’aide de **IHttpActionResult**. Nous allons voir comment des tests unitaires sur eux.

### <a name="action-returns-200-ok-with-a-response-body"></a>Action renvoie 200 (OK) avec un corps de réponse

Le `Get` les appels de méthode `Ok(product)` si le produit est trouvé. Dans le test unitaire, assurez-vous que le type de retour est **OkNegotiatedContentResult** et le produit retourné a l’ID de droite.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

Notez que le test unitaire ne s’exécute pas le résultat d’action. Vous pouvez supposer que le résultat d’action crée la réponse HTTP correctement. (C’est pourquoi l’infrastructure API Web a ses propres tests unitaires !)

### <a name="action-returns-404-not-found"></a>Action retourne le code 404 (introuvable)

Le `Get` les appels de méthode `NotFound()` si le produit est introuvable. Dans ce cas, le test unitaire vérifie simplement si le type de retour est **NotFoundResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a>Action renvoie 200 (OK) sans corps de réponse

Le `Delete` les appels de méthode `Ok()` pour renvoyer une réponse HTTP 200 vide. Comme dans l’exemple précédent, le test unitaire vérifie le type de retour, dans ce cas **OkResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a>Action renvoie 201 (créé) avec un en-tête d’emplacement

Le `Post` les appels de méthode `CreatedAtRoute` pour renvoyer une réponse HTTP 201 avec un URI dans l’en-tête Location. Dans le test unitaire, vérifiez que l’action définit les valeurs correctes de routage.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a>Action retourne un autre 2xx avec un corps de réponse

Le `Put` les appels de méthode `Content` pour renvoyer une réponse HTTP 202 (accepté) avec un corps de réponse. Ce cas est semblable au retour 200 (OK), mais le test unitaire doit également vérifier le code d’état.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a>Ressources supplémentaires

- [Simulation d’Entity Framework lors de l’API Web ASP.NET 2 de tests unitaires](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- [Écriture de tests pour un service API Web ASP.NET](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (billet de blog de Youssef Moussaoui).
- [API Web ASP.NET avec le débogueur d’itinéraires de débogage](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
