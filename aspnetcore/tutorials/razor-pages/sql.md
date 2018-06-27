---
title: Utilisation de SQL Server LocalDB et d’ASP.NET Core
author: rick-anderson
description: Explique l’utilisation de SQL Server LocalDB et d’ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/07/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 92a5965e7a535ca729c0bec13911b6bf051a7b19
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34582867"
---
# <a name="work-with-sql-server-localdb-and-aspnet-core"></a>Utilisation de SQL Server LocalDB et d’ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Joe Audette](https://twitter.com/joeaudette) 

L’objet `MovieContext` gère la tâche de connexion à la base de données et de mappage d’objets `Movie` à des enregistrements de la base de données. Le contexte de base de données est inscrit auprès du conteneur [Injection de dépendances](xref:fundamentals/dependency-injection) dans la méthode `ConfigureServices` du fichier *Startup.cs* :

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=7-8)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

Pour plus d’informations sur les méthodes utilisées dans `ConfigureServices`, consultez :

* [Prise en charge du règlement général sur la protection des données (RGPD) de l’Union Européenne dans ASP.NET Core](xref:security/gdpr) pour `CookiePolicyOptions`.
* [SetCompatibilityVersion](xref:fundamentals/startup#setcompatibilityversion-for-aspnet-core-mvc)

::: moniker-end

Le système de [configuration](xref:fundamentals/configuration/index) d’ASP.NET Core lit `ConnectionString`. Pour un développement local, il obtient la chaîne de connexion à partir du fichier *appsettings.json*. La valeur du nom de la base de données (`Database={Database name}`) est différent pour votre code généré. La valeur du nom est arbitraire.

[!code-json[](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]

Quand vous déployez l’application sur un serveur de test ou de production, vous pouvez utiliser une variable d’environnement ou une autre approche pour définir un serveur SQL Server réel comme chaîne de connexion. Pour plus d’informations, consultez [Configuration](xref:fundamentals/configuration/index).

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

LocalDB est une version allégée du moteur de base de données SQL Server Express qui est ciblée pour le développement de programmes. LocalDB démarre à la demande et s’exécute en mode utilisateur, ce qui n’implique aucune configuration complexe. Par défaut, la base de données LocalDB crée des fichiers « \*.mdf » dans le répertoire *C:/Users/\<utilisateur\>*.

<a name="ssox"></a>
* Dans le menu **Affichage**, ouvrez **l’Explorateur d’objets SQL Server** (SSOX).

  ![Menu View](sql/_static/ssox.png)

* Cliquez avec le bouton droit sur la table `Movie` et sélectionnez **Concepteur de vues** :

  ![Menu contextuel ouvert sur la table Movie](sql/_static/design.png)

  ![Table Movie ouverte dans le Concepteur](sql/_static/dv.png)

Notez l’icône de clé en regard de `ID`. Par défaut, EF crée une propriété nommée `ID` pour la clé primaire.

* Cliquez avec le bouton droit sur la table `Movie` et sélectionnez **Afficher les données** :

  ![Table Movie ouverte, affichant des données de table](sql/_static/vd22.png)

## <a name="seed-the-database"></a>Amorcer la base de données

Créez une classe nommée `SeedData` dans l’espace de noms *Modèles*. Remplacez le code généré par ce qui suit :

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/SeedData.cs?name=snippet_1)]

::: moniker-end

Si la base de données contient des films, l’initialiseur de valeur initiale retourne une valeur et aucun film n’est ajouté.

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a>Ajouter l’initialiseur de valeur initiale

Dans *Program.cs*, modifiez la méthode `Main` pour effectuer les opérations suivantes :

* Obtenir une instance de contexte de base de données à partir du conteneur d’injection de dépendances.
* Appeler la méthode seed et la passer au contexte.
* Supprimer le contexte une fois la méthode seed terminée.

Le code suivant montre le fichier *Program.cs* mis à jour.

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Program.cs)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Program.cs)]

::: moniker-end

Une application de production n’appelle pas `Database.Migrate`. Il est ajouté au code précédent afin d’éviter l’exception suivante quand `Update-Database` n’a pas été exécutée :

SqlException: impossible d’ouvrir la base de données 'RazorPagesMovieContext-21' demandée par la connexion. La connexion a échoué.
Échec de la connexion de l’utilisateur 'nom utilisateur'.

### <a name="test-the-app"></a>Tester l’application

* Supprimez tous les enregistrements de la base de données. Pour ce faire, utilisez les liens de suppression disponibles dans le navigateur ou à partir de [SSOX](xref:tutorials/razor-pages/new-field#ssox)
* Forcez l’application à s’initialiser (appelez les méthodes de la classe `Startup`) pour que la méthode seed s’exécute. Pour forcer l’initialisation, IIS Express doit être arrêté et redémarré. Pour cela, adoptez l’une des approches suivantes :

  * Cliquez avec le bouton droit sur l’icône de barre d’état système IIS Express dans la zone de notification, puis appuyez sur **Quitter** ou sur **Arrêter le site** :

    ![Icône de la barre d’état système IIS Express](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Menu contextuel](sql/_static/stopIIS.png)

    * Si vous exécutiez Visual Studio en mode de non-débogage, appuyez sur F5 pour l’exécuter en mode de débogage.
    * Si vous exécutiez Visual Studio en mode de débogage, arrêtez le débogueur et appuyez sur F5.
   
L’application affiche les données de départ :

![Application Movie ouverte dans Chrome, affichant les données relatives aux films](sql/_static/m55.png)

Le didacticiel suivant nettoie la présentation des données.

> [!div class="step-by-step"]
> [Précédent : Pages Razor obtenues par génération de modèles automatiques](xref:tutorials/razor-pages/page)
> [Suivant : Mises à jour des pages](xref:tutorials/razor-pages/da1)
