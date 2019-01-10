---
title: Ajouter un contrôleur à une application ASP.NET Core MVC
author: rick-anderson
description: Découvrez comment ajouter un contrôleur à une application ASP.NET Core MVC simple.
ms.author: riande
ms.date: 02/28/2017
uid: tutorials/first-mvc-app/adding-controller
ms.openlocfilehash: bbb7b06e2c9c63f44cb7f7a8ee63bffa1e316b3e
ms.sourcegitcommit: 4e87712029de2aceb1cf2c52e9e3dda8195a5b8e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53381866"
---
# <a name="add-a-controller-to-an-aspnet-core-mvc-app"></a>Ajouter un contrôleur à une application ASP.NET Core MVC

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Le modèle d’architecture MVC (Model-View-Controller) sépare une application en trois composants principaux : **M**odèle, **V**ue et **C**ontrôleur. Le modèle MVC vous permet de créer des applications qui sont plus faciles à tester et à mettre à jour que les applications monolithiques traditionnelles. Les applications basées sur MVC contiennent :

* **M**odèles : des classes qui représentent les données de l’application. Les classes du modèle utilisent une logique de validation pour appliquer des règles d’entreprise à ces données. En règle générale, les objets du modèle récupèrent et stockent l’état du modèle dans une base de données. Dans ce didacticiel, un modèle `Movie` récupère les données des films dans une base de données, les fournit à la vue ou les met à jour. Les données mises à jour sont écrites dans une base de données.

* **V**ue : Les vues sont les composants qui affichent l’interface utilisateur de l’application. En règle générale, cette interface utilisateur affiche les données du modèle.

* **C**ontrôleurs : les classes qui gèrent les demandes du navigateur. Ils récupèrent les données du modèle et appellent les modèles de vue qui retournent une réponse. Dans une application MVC, la vue affiche seulement les informations ; le contrôleur gère et répond aux entrées et aux interactions de l’utilisateur. Par exemple, le contrôleur gère les valeurs des données de routage et des chaînes de requête, et passe ces valeurs au modèle. Le modèle peut utiliser ces valeurs pour interroger la base de données. Par exemple, `https://localhost:1234/Home/About` a comme données de routage `Home` (le contrôleur) et `About` (la méthode d’action à appeler sur le contrôleur Home). `https://localhost:1234/Movies/Edit/5` est une demande de modification du film avec l’ID 5 à l’aide du contrôleur movie. Les données d’itinéraire sont expliquées plus loin dans le didacticiel.

Le modèle MVC vous permet de créer des applications qui séparent les différents aspects de l’application (logique d’entrée, logique métier et logique de l’interface utilisateur), tout en assurant un couplage faible entre ces éléments. Le modèle spécifie l’emplacement de chaque type de logique dans l’application. La logique de l’interface utilisateur appartient à la vue. La logique d’entrée appartient au contrôleur. La logique métier appartient au modèle. Cette séparation vous aide à gérer la complexité quand vous créez une application, car elle vous permet de travailler sur un aspect de l’implémentation à la fois, sans impacter le code d’un autre aspect. Par exemple, vous pouvez travailler sur le code des vues de façon indépendante du code de la logique métier.

Nous présentons ces concepts dans cette série de didacticiels et nous vous montrons comment les utiliser pour créer une application de gestion de films. Le projet MVC contient des dossiers pour les *contrôleurs* et pour les *vues*.

## <a name="add-a-controller"></a>Ajouter un contrôleur

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur le ![menu contextuel](adding-controller/_static/add_controller.png) **Contrôleurs > Ajouter > Contrôleur**
  

* Dans la boîte de dialogue **Ajouter un modèle automatique**, sélectionnez **Contrôleur MVC - vide**

  ![Ajoutez un contrôleur MVC et nommez-le](adding-controller/_static/ac.png)

* Dans la **boîte de dialogue Ajouter un contrôleur MVC vide**, entrez **HelloWorldController** et sélectionnez **AJOUTER**.

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Sélectionnez l’icône **EXPLORATEUR** et cliquez en maintenant enfoncée la touche Ctrl (ou cliquez avec le bouton droit) sur **Contrôleurs > Nouveau fichier** et nommez le nouveau fichier *HelloWorldController.cs*.

  ![Menu contextuel](~/tutorials/first-mvc-app-xplat/adding-controller/_static/new_file.png)

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur **Contrôleurs > Ajouter > Nouveau fichier**.
![Menu contextuel](~/tutorials/first-mvc-app-mac/adding-controller/_static/add_controller.png)

Sélectionnez **ASP.NET Core** et **Classe de contrôleur MVC**.

Nommez le contrôleur **HelloWorldController**.

![Ajoutez un contrôleur MVC et nommez-le](~/tutorials/first-mvc-app-mac/adding-controller/_static/ac.png)

---
<!-- End of VS tabs -->

Remplacez le contenu de *Controllers/HelloWorldController.cs* par ceci :

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_1)]

Chaque méthode `public` d’un contrôleur peut être appelée en tant que point de terminaison HTTP. Dans l’exemple ci-dessus, les deux méthodes retournent une chaîne. Notez les commentaires qui précèdent chaque méthode.

Un point de terminaison HTTP est une URL qui peut être ciblée dans l’application web, comme `https://localhost:5001/HelloWorld`, et qui combine le protocole utilisé : `HTTPS`, l’emplacement réseau du serveur web (avec le port TCP) : `localhost:5001` et l’URI cible `HelloWorld`.

Le premier commentaire indique qu’il s’agit d’une méthode [HTTP GET](https://www.w3schools.com/tags/ref_httpmethods.asp) qui est appelée en ajoutant `/HelloWorld/` à l’URL de base. Le deuxième commentaire indique une méthode [HTTP GET](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) qui est appelée en ajoutant `/HelloWorld/Welcome/` à l’URL. Plus loin dans ce didacticiel, le moteur de génération de modèles automatique est utilisé pour générer des méthodes `HTTP POST` qui mettent à jour des données.

Exécutez l’application en mode de non-débogage et ajoutez « HelloWorld » au chemin dans la barre d’adresse. La méthode `Index` retourne une chaîne.

![Fenêtre de navigateur montrant une réponse de l’application à l’action This is my default](~/tutorials/first-mvc-app/adding-controller/_static/hell1.png)

MVC appelle les classes du contrôleur (et les méthodes d’action au sein de celles-ci) en fonction de l’URL entrante. La [logique de routage d’URL](xref:mvc/controllers/routing) par défaut utilisée par le modèle MVC utilise un format comme celui-ci pour déterminer le code à appeler :

`/[Controller]/[ActionName]/[Parameters]`

Le format de routage est défini dans la méthode `Configure` du fichier *Startup.cs*.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

<!-- 
Add link to explain lambda.
Remove link for simplified tutorial.
-->

Quand vous naviguez jusqu’à l’application et que vous ne fournissez aucun segment d’URL, sa valeur par défaut est le contrôleur « Home » et la méthode « Index » spécifiée dans la ligne du modèle mise en surbrillance ci-dessus.

Le premier segment d’URL détermine la classe du contrôleur à exécuter. Ainsi, `localhost:xxxx/HelloWorld` est mappé à la classe `HelloWorldController`. La seconde partie du segment d’URL détermine la méthode d’action sur la classe. Ainsi, `localhost:xxxx/HelloWorld/Index` provoque l’exécution de la méthode `Index` de la classe `HelloWorldController`. Notez que vous n’avez eu qu’à accéder à `localhost:xxxx/HelloWorld` pour que la méthode `Index` soit appelée par défaut. La raison en est que `Index` est la méthode par défaut qui est appelée sur un contrôleur si un nom de méthode n’est pas explicitement spécifié. La troisième partie du segment d’URL (`id`) concerne les données de routage. Les données d’itinéraire sont expliquées plus loin dans le didacticiel.

Accédez à `https://localhost:xxxx/HelloWorld/Welcome`. La méthode `Welcome` s’exécute et retourne la chaîne `This is the Welcome action method...`. Pour cette URL, le contrôleur est `HelloWorld`, et `Welcome` est la méthode d’action. Vous n’avez pas encore utilisé la partie `[Parameters]` de l’URL.

![Fenêtre de navigateur montrant une réponse de la méthode d’action « This is the Welcome action »](~/tutorials/first-mvc-app/adding-controller/_static/welcome.png)

Modifiez le code pour passer des informations sur les paramètres de l’URL au contrôleur. Par exemple, `/HelloWorld/Welcome?name=Rick&numtimes=4`. Modifiez la méthode `Welcome` en y incluant les deux paramètres, comme indiqué dans le code suivant.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_2)]

Le code précédent :

* Utilise la fonctionnalité de paramètre facultatif de C# pour indiquer que le paramètre `numTimes` a 1 comme valeur par défaut si aucune valeur n’est passée pour ce paramètre. <!-- remove for simplified -->
* Utilise `HtmlEncoder.Default.Encode` pour protéger l’application des entrées malveillantes (à savoir JavaScript).
* Utilise des [chaînes interpolées](/dotnet/articles/csharp/language-reference/keywords/interpolated-strings) dans `$"Hello {name}, NumTimes is: {numTimes}"`. <!-- remove for simplified -->

Exécutez l’application et accédez à :

   `https://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

(Remplacez xxxx par votre numéro de port.) Vous pouvez essayer différentes valeurs pour `name` et `numtimes` dans l’URL. Le système de [liaison de données](xref:mvc/models/model-binding) du modèle MVC mappe automatiquement les paramètres nommés provenant de la chaîne de requête dans la barre d’adresse aux paramètres de votre méthode. Pour plus d’informations, consultez [Liaison de données](xref:mvc/models/model-binding).

![Fenêtre de navigateur montrant une réponse de l’application « Hello Rick, NumTimes is: 4](~/tutorials/first-mvc-app/adding-controller/_static/rick4.png)

Dans l’image ci-dessus, le segment d’URL (`Parameters`) n’est pas utilisé, les paramètres `name` et `numTimes` sont passés en tant que [chaînes de requête](https://wikipedia.org/wiki/Query_string). Le `?` (point d’interrogation) dans l’URL ci-dessus est un séparateur, qui est suivi des chaînes de requête. Le caractère `&` sépare les chaînes de requête.

Remplacez la méthode `Welcome` par le code suivant :

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_3)]

Exécutez l’application et entrez l’URL suivante : `https://localhost:xxx/HelloWorld/Welcome/3?name=Rick`

Cette fois, le troisième segment de l’URL correspondait au paramètre de routage `id`. La méthode `Welcome` contient un paramètre `id` qui correspondait au modèle d’URL de la méthode `MapRoute`. Le `?` de fin (dans `id?`) indique que le paramètre `id` est facultatif.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

Dans ces exemples, le contrôleur a fait la partie « VC » du modèle MVC, autrement dit, la vue et le contrôleur fonctionnent. Le contrôleur retourne directement du HTML. En règle générale, vous ne souhaitez pas que les contrôleurs retournent directement du HTML, car le codage et la maintenance deviennent dans ce cas très laborieux. Au lieu de cela, vous utilisez généralement un fichier de modèle de vue Razor distinct pour faciliter la génération de la réponse HTML. Vous faites cela dans le didacticiel suivant.


> [!div class="step-by-step"]
> [Précédent](start-mvc.md)
> [Suivant](adding-view.md)
