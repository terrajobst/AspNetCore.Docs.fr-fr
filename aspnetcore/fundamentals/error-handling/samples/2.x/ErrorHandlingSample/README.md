---
ms.openlocfilehash: b1f7c306533e70f84e73a020c74756b91cae7ea7
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665749"
---
# <a name="error-handling-sample-application"></a><span data-ttu-id="566c5-101">Exemple d'application de gestion des erreurs</span><span class="sxs-lookup"><span data-stu-id="566c5-101">Error Handling Sample Application</span></span>

<span data-ttu-id="566c5-102">Cet exemple d’application illustre les scénarios décrits dans la rubrique [Gérer les erreurs dans ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/error-handling).</span><span class="sxs-lookup"><span data-stu-id="566c5-102">This sample app demonstrates the scenarios described in the [Handle errors in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/error-handling) topic.</span></span>

## <a name="configure-a-custom-exception-handling-page"></a><span data-ttu-id="566c5-103">Configurer une page personnalisée de gestion des exceptions</span><span class="sxs-lookup"><span data-stu-id="566c5-103">Configure a custom exception handling page</span></span>

<span data-ttu-id="566c5-104">Lorsque l’application ne s’exécute pas dans l’environnement de développement, l’intergiciel (middleware) de gestion des exceptions :</span><span class="sxs-lookup"><span data-stu-id="566c5-104">When the app isn't running in the Development environment, Exception Handling Middleware:</span></span>

* <span data-ttu-id="566c5-105">Intercepte les exceptions.</span><span class="sxs-lookup"><span data-stu-id="566c5-105">Catches exceptions.</span></span>
* <span data-ttu-id="566c5-106">Journalise les exceptions.</span><span class="sxs-lookup"><span data-stu-id="566c5-106">Logs exceptions.</span></span>
* <span data-ttu-id="566c5-107">Réexécute la requête dans un autre pipeline au chemin fourni.</span><span class="sxs-lookup"><span data-stu-id="566c5-107">Re-executes the request in an alternate pipeline at the path provided.</span></span>

## <a name="configure-custom-exception-handling-code"></a><span data-ttu-id="566c5-108">Configurer du code personnalisé de gestion des exceptions</span><span class="sxs-lookup"><span data-stu-id="566c5-108">Configure custom exception handling code</span></span>

<span data-ttu-id="566c5-109">Pour éviter de distribuer les erreurs à un point de terminaison avec une page personnalisée de gestion des exceptions, il est possible de fournir une expression lambda à `UseExceptionHandler`.</span><span class="sxs-lookup"><span data-stu-id="566c5-109">An alternative to serving an endpoint for errors with a custom exception handling page is to provide a lambda to `UseExceptionHandler`.</span></span> <span data-ttu-id="566c5-110">À l’aide d’une expression lambda avec `UseExceptionHandler` permet d’accéder à l’erreur avant de renvoyer la réponse.</span><span class="sxs-lookup"><span data-stu-id="566c5-110">Using a lambda with `UseExceptionHandler` allows access to the error before returning the response.</span></span>

<span data-ttu-id="566c5-111">L’exemple d’application illustre le code personnalisé de gestion des exceptions dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="566c5-111">The sample app demonstrates custom exception handling code in `Startup.Configure`.</span></span> <span data-ttu-id="566c5-112">Suivez les instructions qui s’affichent en haut du fichier *Startup.cs* (`LambdaErrorHandler`).</span><span class="sxs-lookup"><span data-stu-id="566c5-112">Follow the instructions at the top of the *Startup.cs* file (`LambdaErrorHandler`).</span></span> <span data-ttu-id="566c5-113">Après le lancement de l’application, déclenchez une exception avec le lien **Lever une exception** sur la page d’index.</span><span class="sxs-lookup"><span data-stu-id="566c5-113">After the app starts, trigger an exception with the **Throw Exception** link on the Index page.</span></span>
