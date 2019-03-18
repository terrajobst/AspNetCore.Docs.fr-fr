---
ms.openlocfilehash: b1f7c306533e70f84e73a020c74756b91cae7ea7
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665749"
---
# <a name="error-handling-sample-application"></a>Exemple d'application de gestion des erreurs

Cet exemple d’application illustre les scénarios décrits dans la rubrique [Gérer les erreurs dans ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/error-handling).

## <a name="configure-a-custom-exception-handling-page"></a>Configurer une page personnalisée de gestion des exceptions

Lorsque l’application ne s’exécute pas dans l’environnement de développement, l’intergiciel (middleware) de gestion des exceptions :

* Intercepte les exceptions.
* Journalise les exceptions.
* Réexécute la requête dans un autre pipeline au chemin fourni.

## <a name="configure-custom-exception-handling-code"></a>Configurer du code personnalisé de gestion des exceptions

Pour éviter de distribuer les erreurs à un point de terminaison avec une page personnalisée de gestion des exceptions, il est possible de fournir une expression lambda à `UseExceptionHandler`. À l’aide d’une expression lambda avec `UseExceptionHandler` permet d’accéder à l’erreur avant de renvoyer la réponse.

L’exemple d’application illustre le code personnalisé de gestion des exceptions dans `Startup.Configure`. Suivez les instructions qui s’affichent en haut du fichier *Startup.cs* (`LambdaErrorHandler`). Après le lancement de l’application, déclenchez une exception avec le lien **Lever une exception** sur la page d’index.
