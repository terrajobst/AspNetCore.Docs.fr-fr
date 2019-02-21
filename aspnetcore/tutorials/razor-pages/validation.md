---
title: Ajouter une validation à une page Razor ASP.NET Core
author: rick-anderson
description: Découvrez comment ajouter la validation à une page Razor dans ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/5/2018
uid: tutorials/razor-pages/validation
ms.openlocfilehash: 93303b76561a8a800432ee707997f240f15e29c7
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410454"
---
# <a name="add-validation-to-an-aspnet-core-razor-page"></a>Ajouter une validation à une page Razor ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Dans cette section, une logique de validation est ajoutée au modèle `Movie`. Les règles de validation sont appliquées chaque fois qu’un utilisateur crée ou modifie un film.

## <a name="validation"></a>Validation

[DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself) (« **D**on't **R**epeat **Y**ourself », Ne vous répétez pas) constitue un principe clé du développement de logiciel. Les pages Razor favorisent le développement dans lequel une fonctionnalité est spécifiée une seule fois et sont répercutées dans toute l’application. DRY peut aider à :

* Réduire la quantité de code dans une application.
* Rendre le code moins sujet aux erreurs, et plus facile à tester et à maintenir.

La prise en charge de la validation fournie par les pages Razor et Entity Framework est un bon exemple du principe DRY. Des règles de validation sont spécifiées de façon déclarative à un seul emplacement (dans la classe de modèle), et sont appliquées partout dans l’application.

### <a name="adding-validation-rules-to-the-movie-model"></a>Ajout de règles de validation au modèle Movie

Ouvrez le fichier *Models/Movie.cs*. [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) fournit un ensemble intégré d’attributs de validation qui sont appliqués de manière déclarative à une classe ou une propriété. DataAnnotations contient également des attributs de mise en forme comme `DataType`,qui aident à effectuer la mise en forme et ne fournissent aucune validation.

Mettez à jour la classe `Movie` pour tirer parti des attributs de validation `Required`, `StringLength`, `RegularExpression` et `Range`.

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateRatingDA.cs?name=snippet1)]

Les attributs de validation spécifient un comportement qui est appliqué sur les propriétés du modèle :

* Les attributs `Required` et `MinimumLength` indiquent qu’une propriété doit avoir une valeur. Toutefois, rien n’empêche un utilisateur d’entrer un espace blanc pour satisfaire la contrainte de validation pour un type Nullable. Les [types valeur](/dotnet/csharp/language-reference/keywords/value-types) non Nullable (tels que `decimal`, `int`, `float` et `DateTime`) sont obligatoires par nature et n’ont pas besoin de l’attribut `Required`.
* L’attribut `RegularExpression` limite les caractères que l’utilisateur peut entrer. Dans le code précédent, `Genre` doit commencer par une ou plusieurs lettres majuscules, puis zéro ou plusieurs lettres, guillemets simples ou doubles, espaces ou tirets. `Rating` doit commencer par une ou plusieurs lettres majuscules, puis zéro ou plusieurs lettres, chiffres, guillemets simples ou doubles, espaces ou tirets.
* L’attribut `Range` contraint une valeur à une plage spécifiée.
* L’attribut `StringLength` définit la longueur maximale d’une chaîne, et éventuellement la longueur minimale. 

L’application automatique des règles de validation par ASP.NET Core permet d’accroître la fiabilité d’une application. La validation automatique sur des modèles permet de protéger l’application, car vous n’avez pas à penser à les appliquer lors de l’ajout de nouveau code.

### <a name="validation-error-ui-in-razor-pages"></a>Interface utilisateur d’erreur de validation dans les pages Razor

Exécutez l’application, puis accédez à Pages/Movies.

Sélectionnez le lien **Créer nouveau**. Complétez le formulaire avec des valeurs non valides. Quand la validation jQuery côté client détecte l’erreur, elle affiche un message d’erreur.

![Formulaire de vue Movie avec plusieurs erreurs de validation jQuery côté client](validation/_static/val.png)

[!INCLUDE[](~/includes/currency.md)]

Notez que le formulaire a affiché automatiquement un message d’erreur de validation dans chaque champ contenant une valeur non valide. Les erreurs sont appliquées à la fois côté client (à l’aide de JavaScript et de jQuery) et côté serveur (quand JavaScript est désactivé pour un utilisateur).

L’un des principaux avantages est qu’**aucun** changement de code n’a été nécessaire dans les pages Créer ou Modifier. Une fois les attributs DataAnnotations appliqués au modèle, l’interface utilisateur de validation a été activée. Les pages Razor créées dans ce didacticiel ont prélevé les règles de validation (à l’aide des attributs de validation définis sur les propriétés de la classe de modèle `Movie`). Testez la validation à l’aide de la page de modification. La même validation est appliquée.

Les données de formulaire ne sont pas publiées sur le serveur tant qu’il y a des erreurs de validation côté client. Vérifiez que les données du formulaire ne sont pas publiées à l’aide d’une ou de plusieurs des approches suivantes :

* Placez un point d’arrêt dans la méthode `OnPostAsync`. Envoyer le formulaire (en sélectionnant **Créer** ou **Enregistrer**). Le point d’arrêt n’est jamais atteint.
* Utilisez l’[outil Fiddler](http://www.telerik.com/fiddler).
* Utilisez les outils de développement du navigateur pour surveiller le trafic réseau.

### <a name="server-side-validation"></a>Validation côté serveur

Quand JavaScript est désactivé dans le navigateur, l’envoi du formulaire avec des erreurs poste les données sur le serveur.

Facultatif : Testez la validation côté serveur :

* Désactivez JavaScript dans le navigateur. Pour cela, utilisez les outils de développement de votre navigateur. Si vous ne pouvez pas désactiver JavaScript dans le navigateur, essayez un autre navigateur.
* Définissez un point d’arrêt dans la méthode `OnPostAsync` de la page Créer ou Modifier.
* Envoyez un formulaire avec des erreurs de validation.
* Vérifiez que l’état de modèle n’est pas valide :

  ```csharp
   if (!ModelState.IsValid)
   {
      return Page();
   }
  ```

Le code suivant affiche la partie de la page *Create.cshtml* pour laquelle vous avez généré automatiquement des modèles précédemment dans le didacticiel. Elle est utilisée par les pages Créer et Modifier pour afficher le formulaire initial et le réafficher en cas d’erreur.

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=14-20)]

Le [Tag Helper d’entrée](xref:mvc/views/working-with-forms) utilise les attributs [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) et produit les attributs HTML nécessaires à la validation jQuery côté client. Le [Tag Helper de validation](xref:mvc/views/working-with-forms#the-validation-tag-helpers) affiche les erreurs de validation. Pour plus d’informations, consultez [Validation](xref:mvc/models/validation).

Les pages Créer et Modifier ne contiennent pas de règles de validation. Les règles de validation et les chaînes d’erreur sont spécifiées uniquement dans la classe `Movie`. Ces règles de validation sont automatiquement appliquées aux pages Razor qui modifient le modèle `Movie`.

Quand la logique de validation doit être modifiée, cela s’effectue uniquement dans le modèle. La validation est appliquée de manière cohérente dans l’ensemble de l’application (la logique de validation est définie dans un emplacement unique). La validation dans un emplacement unique permet de maintenir votre code clair, et d’en faciliter la maintenance et la mise à jour.

## <a name="using-datatype-attributes"></a>Utilisation d’attributs DataType

Examiner la classe `Movie`. L’espace de noms `System.ComponentModel.DataAnnotations` fournit des attributs de mise en forme en plus de l’ensemble intégré d’attributs de validation. L'attribut `DataType` est appliqué aux propriétés `ReleaseDate` et `Price`.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

Les attributs `DataType` fournissent uniquement des conseils permettant au moteur de vue de mettre en forme les données (et fournissent des attributs tels que `<a>` pour les URL et `<a href="mailto:EmailAddress.com">` pour l’e-mail). Utilisez l’attribut `RegularExpression` pour valider le format des données. L’attribut `DataType` sert à spécifier un type de données qui est plus spécifique que le type intrinsèque de la base de données. Les attributs `DataType` ne sont pas des attributs de validation. Dans l’exemple d’application, seule la date est affichée, sans l’heure.

L’énumération `DataType` fournit de nombreux types de données, tels que Date, Time, PhoneNumber, Currency ou EmailAddress. L’attribut `DataType` peut également permettre à l’application de fournir automatiquement des fonctionnalités propres au type. Par exemple, il est possible de créer un lien `mailto:` pour `DataType.EmailAddress`. Un sélecteur de date peut être fourni pour `DataType.Date` dans les navigateurs qui prennent en charge HTML5. Les attributs `DataType` émettent des attributs HTML 5 `data-` utilisés par les navigateurs HTML 5. Les attributs `DataType` ne fournissent **aucune** validation.

`DataType.Date` ne spécifie pas le format de la date qui s’affiche. Par défaut, le champ de données est affiché conformément aux formats par défaut basés sur le `CultureInfo` du serveur.


L’annotation de données `[Column(TypeName = "decimal(18, 2)")]` est nécessaire pour qu’Entity Framework Core puisse correctement mapper `Price` en devise dans la base de données. Pour plus d’informations, consultez [Types de données](/ef/core/modeling/relational/data-types).

L’attribut `DisplayFormat` est utilisé pour spécifier explicitement le format de date :

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

Le paramètre `ApplyFormatInEditMode` indique que la mise en forme doit être appliquée quand la valeur est affichée à des fins de modification. Vous pouvez souhaiter ce comportement pour certains champs. Par exemple, dans les valeurs monétaires, vous ne voulez probablement pas le symbole monétaire dans l’interface utilisateur de modification.

L’attribut `DisplayFormat` peut être utilisé seul, mais il est généralement préférable d’utiliser l’attribut `DataType`. L’attribut `DataType` donne la sémantique des données, au lieu de d’expliquer comment les afficher sur un écran. Il présente, par ailleurs, les avantages suivants dont vous ne bénéficiez pas avec DisplayFormat :

* Le navigateur peut activer des fonctionnalités HTML5 (par exemple pour afficher un contrôle de calendrier, le symbole monétaire correspondant aux paramètres régionaux, des liens de messagerie, etc.).
* Par défaut, le navigateur affiche les données à l’aide du format correspondant à vos paramètres régionaux.
* L’attribut `DataType` peut permettre à l’infrastructure ASP.NET Core de choisir le modèle de champ approprié pour afficher les données. S’il est utilisé seul, `DisplayFormat` utilise le modèle de chaîne.

Remarque : La validation jQuery ne fonctionne pas avec l’attribut `Range` et `DateTime`. Par exemple, le code suivant affiche toujours une erreur de validation côté client, même quand la date se trouve dans la plage spécifiée :

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

Il n’est généralement pas recommandé de compiler des dates en dur dans vos modèles. Par conséquent, l’utilisation de l’attribut `Range` et de `DateTime` est déconseillée.

Le code suivant illustre la combinaison d’attributs sur une seule ligne :

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateRatingDAmult.cs?name=snippet1)]

La rubrique [Bien démarrer avec Razor Pages et Entity Framework Core](xref:data/ef-rp/intro) présente des opérations EF Core avancées avec Razor Pages.

### <a name="publish-to-azure"></a>Publier sur Azure

Pour plus d’informations sur le déploiement vers Azure, consultez [Didacticiel : Créer une application ASP.NET dans Azure avec SQL Database](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase). Ces instructions s’appliquent à une application ASP.NET, pas à une application ASP.NET Core, mais les étapes sont les mêmes.

Nous vous remercions d’avoir effectué cette introduction aux pages Razor. Pour compléter ce tutoriel, vous pouvez consulter [Bien démarrer avec Razor Pages et EF Core](xref:data/ef-rp/intro).

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:mvc/views/working-with-forms>
* <xref:fundamentals/localization>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/authoring>

> [!div class="step-by-step"]
> [Précédent : Ajout d’un nouveau champ](xref:tutorials/razor-pages/new-field)
