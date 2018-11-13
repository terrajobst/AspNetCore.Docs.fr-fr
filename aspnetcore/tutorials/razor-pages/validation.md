---
title: Ajouter une validation à une page Razor ASP.NET Core
author: rick-anderson
description: Découvrez comment ajouter la validation à une page Razor dans ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: tutorials/razor-pages/validation
ms.openlocfilehash: d4cc0ab9de314c0c5a1a9016efd1e566ff1c47d2
ms.sourcegitcommit: edb9d2d78c9a4d68b397e74ae2aff088b325a143
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/09/2018
ms.locfileid: "51505776"
---
# <a name="add-validation-to-an-aspnet-core-razor-page"></a><span data-ttu-id="0b1c2-103">Ajouter une validation à une page Razor ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0b1c2-103">Add validation to an ASP.NET Core Razor Page</span></span>

<span data-ttu-id="0b1c2-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0b1c2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0b1c2-105">Dans cette section, une logique de validation est ajoutée au modèle `Movie`.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-105">In this section, validation logic is added to the `Movie` model.</span></span> <span data-ttu-id="0b1c2-106">Les règles de validation sont appliquées chaque fois qu’un utilisateur crée ou modifie un film.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-106">The validation rules are enforced any time a user creates or edits a movie.</span></span>

## <a name="validation"></a><span data-ttu-id="0b1c2-107">Validation</span><span class="sxs-lookup"><span data-stu-id="0b1c2-107">Validation</span></span>

<span data-ttu-id="0b1c2-108">[DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself) (« **D**on't **R**epeat **Y**ourself », Ne vous répétez pas) constitue un principe clé du développement de logiciel.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-108">A key tenet of software development is called [DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D**on't **R**epeat **Y**ourself").</span></span> <span data-ttu-id="0b1c2-109">Les pages Razor favorisent le développement dans lequel une fonctionnalité est spécifiée une seule fois et sont répercutées dans toute l’application.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-109">Razor Pages encourages development where functionality is specified once, and it's reflected throughout the app.</span></span> <span data-ttu-id="0b1c2-110">Le principe DRY réduit la quantité de code dans une application.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-110">DRY can help reduce the amount of code in an app.</span></span> <span data-ttu-id="0b1c2-111">Il rend le code moins sujet aux erreurs, plus facile à tester et plus facile à gérer.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-111">DRY makes the code less error prone, and easier to test and maintain.</span></span>

<span data-ttu-id="0b1c2-112">La prise en charge de la validation fournie par les pages Razor et Entity Framework est un bon exemple du principe DRY.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-112">The validation support provided by Razor Pages and Entity Framework is a good example of the DRY principle.</span></span> <span data-ttu-id="0b1c2-113">Des règles de validation sont spécifiées de façon déclarative à un seul emplacement (dans la classe de modèle), et sont appliquées partout dans l’application.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-113">Validation rules are declaratively specified in one place (in the model class), and the rules are enforced everywhere in the app.</span></span>

### <a name="adding-validation-rules-to-the-movie-model"></a><span data-ttu-id="0b1c2-114">Ajout de règles de validation au modèle Movie</span><span class="sxs-lookup"><span data-stu-id="0b1c2-114">Adding validation rules to the movie model</span></span>

<span data-ttu-id="0b1c2-115">Ouvrez le fichier *Models/Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-115">Open the *Models/Movie.cs* file.</span></span> <span data-ttu-id="0b1c2-116">[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) fournit un ensemble intégré d’attributs de validation qui sont appliqués de manière déclarative à une classe ou une propriété.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-116">[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) provides a built-in set of validation attributes that are applied declaratively to a class or property.</span></span> <span data-ttu-id="0b1c2-117">DataAnnotations contient également des attributs de mise en forme comme `DataType` qui aident à effectuer la mise en forme et ne fournissent aucune validation.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-117">DataAnnotations also contains formatting attributes like `DataType` that help with formatting and don't provide validation.</span></span>

<span data-ttu-id="0b1c2-118">Mettez à jour la classe `Movie` pour tirer parti des attributs de validation `Required`, `StringLength`, `RegularExpression` et `Range`.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-118">Update the `Movie` class to take advantage of the `Required`, `StringLength`, `RegularExpression`, and `Range` validation attributes.</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRatingDA.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRatingDA.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="0b1c2-119">Les attributs de validation spécifient un comportement qui est appliqué sur les propriétés du modèle :</span><span class="sxs-lookup"><span data-stu-id="0b1c2-119">Validation attributes specify behavior that's enforced on model properties:</span></span>

* <span data-ttu-id="0b1c2-120">Les attributs `Required` et `MinimumLength` indiquent qu’une propriété doit avoir une valeur.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-120">The `Required` and `MinimumLength` attributes indicate that a property must have a value.</span></span> <span data-ttu-id="0b1c2-121">Toutefois, rien n’empêche un utilisateur d’entrer un espace blanc pour satisfaire la contrainte de validation pour un type Nullable.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-121">However, nothing prevents a user from entering whitespace to satisfy the validation constraint for a nullable type.</span></span> <span data-ttu-id="0b1c2-122">Les [types valeur](/dotnet/csharp/language-reference/keywords/value-types) non Nullable (tels que `decimal`, `int`, `float` et `DateTime`) sont obligatoires par nature et n’ont pas besoin de l’attribut `Required`.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-122">Non-nullable [value types](/dotnet/csharp/language-reference/keywords/value-types) (such as `decimal`, `int`, `float`, and `DateTime`) are inherently required and don't need the `Required` attribute.</span></span>
* <span data-ttu-id="0b1c2-123">L’attribut `RegularExpression` limite les caractères que l’utilisateur peut entrer.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-123">The `RegularExpression` attribute limits the characters that the user can enter.</span></span> <span data-ttu-id="0b1c2-124">Dans le code précédent, `Genre` doit commencer par une ou plusieurs lettres majuscules, puis zéro ou plusieurs lettres, guillemets simples ou doubles, espaces ou tirets.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-124">In the preceding code, `Genre` must start with one or more capital letters and follow with zero or more letters, single or double quotes, whitespace characters, or dashes.</span></span> <span data-ttu-id="0b1c2-125">`Rating` doit commencer par une ou plusieurs lettres majuscules, puis zéro ou plusieurs lettres, chiffres, guillemets simples ou doubles, espaces ou tirets.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-125">`Rating` must start with one or more capital letters and follow with zero or more letters, numbers, single or double quotes, whitespace characters, or dashes.</span></span>
* <span data-ttu-id="0b1c2-126">L’attribut `Range` contraint une valeur à une plage spécifiée.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-126">The `Range` attribute constrains a value to a specified range.</span></span>
* <span data-ttu-id="0b1c2-127">L’attribut `StringLength` définit la longueur maximale d’une chaîne, et éventuellement la longueur minimale.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-127">The `StringLength` attribute sets the maximum length of a string, and optionally the minimum length.</span></span> 

<span data-ttu-id="0b1c2-128">L’application automatique des règles de validation par ASP.NET Core permet d’accroître la fiabilité d’une application.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-128">Having validation rules automatically enforced by ASP.NET Core helps make an app more robust.</span></span> <span data-ttu-id="0b1c2-129">La validation automatique sur des modèles permet de protéger l’application, car vous n’avez pas à penser à les appliquer lors de l’ajout de nouveau code.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-129">Automatic validation on models helps protect the app because you don't have to remember to apply them when new code is added.</span></span>

### <a name="validation-error-ui-in-razor-pages"></a><span data-ttu-id="0b1c2-130">Interface utilisateur d’erreur de validation dans les pages Razor</span><span class="sxs-lookup"><span data-stu-id="0b1c2-130">Validation Error UI in Razor Pages</span></span>

<span data-ttu-id="0b1c2-131">Exécutez l’application, puis accédez à Pages/Movies.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-131">Run the app and navigate to Pages/Movies.</span></span>

<span data-ttu-id="0b1c2-132">Sélectionnez le lien **Créer nouveau**.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-132">Select the **Create New** link.</span></span> <span data-ttu-id="0b1c2-133">Complétez le formulaire avec des valeurs non valides.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-133">Complete the form with some invalid values.</span></span> <span data-ttu-id="0b1c2-134">Quand la validation jQuery côté client détecte l’erreur, elle affiche un message d’erreur.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-134">When jQuery client-side validation detects the error, it displays an error message.</span></span>

![Formulaire de vue Movie avec plusieurs erreurs de validation jQuery côté client](validation/_static/val.png)

[!INCLUDE[](~/includes/currency.md)]

<span data-ttu-id="0b1c2-136">Notez que le formulaire a affiché automatiquement un message d’erreur de validation dans chaque champ contenant une valeur non valide.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-136">Notice how the form has automatically rendered a validation error message in each field containing an invalid value.</span></span> <span data-ttu-id="0b1c2-137">Les erreurs sont appliquées à la fois côté client (à l’aide de JavaScript et de jQuery) et côté serveur (quand JavaScript est désactivé pour un utilisateur).</span><span class="sxs-lookup"><span data-stu-id="0b1c2-137">The errors are enforced both client-side (using JavaScript and jQuery) and server-side (when a user has JavaScript disabled).</span></span>

<span data-ttu-id="0b1c2-138">L’un des principaux avantages est qu’**aucun** changement de code n’a été nécessaire dans les pages Créer ou Modifier.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-138">A significant benefit is that **no** code changes were necessary in the Create  or Edit pages.</span></span> <span data-ttu-id="0b1c2-139">Une fois les attributs DataAnnotations appliqués au modèle, l’interface utilisateur de validation a été activée.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-139">Once DataAnnotations were applied to the model, the validation UI was enabled.</span></span> <span data-ttu-id="0b1c2-140">Les pages Razor créées dans ce didacticiel ont prélevé les règles de validation (à l’aide des attributs de validation définis sur les propriétés de la classe de modèle `Movie`).</span><span class="sxs-lookup"><span data-stu-id="0b1c2-140">The Razor Pages created in this tutorial automatically picked up the validation rules (using validation attributes on the properties of the `Movie` model class).</span></span> <span data-ttu-id="0b1c2-141">Testez la validation à l’aide de la page de modification. La même validation est appliquée.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-141">Test validation using the Edit page, the same validation is applied.</span></span>

<span data-ttu-id="0b1c2-142">Les données de formulaire ne sont pas publiées sur le serveur tant qu’il y a des erreurs de validation côté client.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-142">The form data isn't posted to the server until there are no client-side validation errors.</span></span> <span data-ttu-id="0b1c2-143">Vérifiez que les données du formulaire ne sont pas publiées à l’aide d’une ou de plusieurs des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="0b1c2-143">Verify form data isn't posted by one or more of the following approaches:</span></span>

* <span data-ttu-id="0b1c2-144">Placez un point d’arrêt dans la méthode `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-144">Put a break point in the `OnPostAsync` method.</span></span> <span data-ttu-id="0b1c2-145">Envoyer le formulaire (en sélectionnant **Créer** ou **Enregistrer**).</span><span class="sxs-lookup"><span data-stu-id="0b1c2-145">Submit the form (select **Create** or **Save**).</span></span> <span data-ttu-id="0b1c2-146">Le point d’arrêt n’est jamais atteint.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-146">The break point is never hit.</span></span>
* <span data-ttu-id="0b1c2-147">Utilisez l’[outil Fiddler](http://www.telerik.com/fiddler).</span><span class="sxs-lookup"><span data-stu-id="0b1c2-147">Use the [Fiddler tool](http://www.telerik.com/fiddler).</span></span>
* <span data-ttu-id="0b1c2-148">Utilisez les outils de développement du navigateur pour surveiller le trafic réseau.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-148">Use the browser developer tools to monitor network traffic.</span></span>

### <a name="server-side-validation"></a><span data-ttu-id="0b1c2-149">Validation côté serveur</span><span class="sxs-lookup"><span data-stu-id="0b1c2-149">Server-side validation</span></span>

<span data-ttu-id="0b1c2-150">Quand JavaScript est désactivé dans le navigateur, l’envoi du formulaire avec des erreurs poste les données sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-150">When JavaScript is disabled in the browser, submitting the form with errors will post to the server.</span></span>

<span data-ttu-id="0b1c2-151">Facultatif : Testez la validation côté serveur :</span><span class="sxs-lookup"><span data-stu-id="0b1c2-151">Optional, test server-side validation:</span></span>

* <span data-ttu-id="0b1c2-152">Désactivez JavaScript dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-152">Disable JavaScript in the browser.</span></span> <span data-ttu-id="0b1c2-153">Pour cela, utilisez les outils de développement de votre navigateur.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-153">You can do this using your browser's developer tools.</span></span> <span data-ttu-id="0b1c2-154">Si vous ne pouvez pas désactiver JavaScript dans le navigateur, essayez un autre navigateur.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-154">If you can't disable JavaScript in the browser, try another browser.</span></span>
* <span data-ttu-id="0b1c2-155">Définissez un point d’arrêt dans la méthode `OnPostAsync` de la page Créer ou Modifier.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-155">Set a break point in the `OnPostAsync` method of the Create or Edit page.</span></span>
* <span data-ttu-id="0b1c2-156">Envoyez un formulaire avec des erreurs de validation.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-156">Submit a form with validation errors.</span></span>
* <span data-ttu-id="0b1c2-157">Vérifiez que l’état de modèle n’est pas valide :</span><span class="sxs-lookup"><span data-stu-id="0b1c2-157">Verify the model state is invalid:</span></span>

  ```csharp
   if (!ModelState.IsValid)
   {
      return Page();
   }
  ```

<span data-ttu-id="0b1c2-158">Le code suivant affiche la partie de la page *Create.cshtml* pour laquelle vous avez généré automatiquement des modèles précédemment dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-158">The following code shows a portion of the *Create.cshtml* page that you scaffolded earlier in the tutorial.</span></span> <span data-ttu-id="0b1c2-159">Elle est utilisée par les pages Créer et Modifier pour afficher le formulaire initial et le réafficher en cas d’erreur.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-159">It's used by the Create and Edit pages to display the initial form and to redisplay the form in the event of an error.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=14-20)]

<span data-ttu-id="0b1c2-160">Le [Tag Helper d’entrée](xref:mvc/views/working-with-forms) utilise les attributs [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) et produit les attributs HTML nécessaires à la validation jQuery côté client.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-160">The [Input Tag Helper](xref:mvc/views/working-with-forms) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span> <span data-ttu-id="0b1c2-161">Le [Tag Helper de validation](xref:mvc/views/working-with-forms#the-validation-tag-helpers) affiche les erreurs de validation.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-161">The [Validation Tag Helper](xref:mvc/views/working-with-forms#the-validation-tag-helpers) displays validation errors.</span></span> <span data-ttu-id="0b1c2-162">Pour plus d’informations, consultez [Validation](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="0b1c2-162">See [Validation](xref:mvc/models/validation) for more information.</span></span>

<span data-ttu-id="0b1c2-163">Les pages Créer et Modifier ne contiennent pas de règles de validation.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-163">The Create and Edit pages have no validation rules in them.</span></span> <span data-ttu-id="0b1c2-164">Les règles de validation et les chaînes d’erreur sont spécifiées uniquement dans la classe `Movie`.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-164">The validation rules and the error strings are specified only in the `Movie` class.</span></span> <span data-ttu-id="0b1c2-165">Ces règles de validation sont automatiquement appliquées aux pages Razor qui modifient le modèle `Movie`.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-165">These validation rules are automatically applied to Razor Pages that edit the `Movie` model.</span></span>

<span data-ttu-id="0b1c2-166">Quand la logique de validation doit être modifiée, cela s’effectue uniquement dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-166">When validation logic needs to change, it's done only in the model.</span></span> <span data-ttu-id="0b1c2-167">La validation est appliquée de manière cohérente dans l’ensemble de l’application (la logique de validation est définie dans un emplacement unique).</span><span class="sxs-lookup"><span data-stu-id="0b1c2-167">Validation is applied consistently throughout the application (validation logic is defined in one place).</span></span> <span data-ttu-id="0b1c2-168">La validation dans un emplacement unique permet de maintenir votre code clair, et d’en faciliter la maintenance et la mise à jour.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-168">Validation in one place helps keep the code clean, and makes it easier to maintain and update.</span></span>

## <a name="using-datatype-attributes"></a><span data-ttu-id="0b1c2-169">Utilisation d’attributs DataType</span><span class="sxs-lookup"><span data-stu-id="0b1c2-169">Using DataType Attributes</span></span>

<span data-ttu-id="0b1c2-170">Examiner la classe `Movie`.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-170">Examine the `Movie` class.</span></span> <span data-ttu-id="0b1c2-171">L’espace de noms `System.ComponentModel.DataAnnotations` fournit des attributs de mise en forme en plus de l’ensemble intégré d’attributs de validation.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-171">The `System.ComponentModel.DataAnnotations` namespace provides formatting attributes in addition to the built-in set of validation attributes.</span></span> <span data-ttu-id="0b1c2-172">L'attribut `DataType` est appliqué aux propriétés `ReleaseDate` et `Price`.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-172">The `DataType` attribute is applied to the `ReleaseDate` and `Price` properties.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

<span data-ttu-id="0b1c2-173">Les attributs `DataType` fournissent uniquement des conseils permettant au moteur de vue de mettre en forme les données (et fournissent des attributs tels que `<a>` pour les URL et `<a href="mailto:EmailAddress.com">` pour l’e-mail).</span><span class="sxs-lookup"><span data-stu-id="0b1c2-173">The `DataType` attributes only provide hints for the view engine to format the data (and supplies attributes such as `<a>` for URL's and `<a href="mailto:EmailAddress.com">` for email).</span></span> <span data-ttu-id="0b1c2-174">Utilisez l’attribut `RegularExpression` pour valider le format des données.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-174">Use the `RegularExpression` attribute to validate the format of the data.</span></span> <span data-ttu-id="0b1c2-175">L’attribut `DataType` sert à spécifier un type de données qui est plus spécifique que le type intrinsèque de la base de données.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-175">The `DataType` attribute is used to specify a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="0b1c2-176">Les attributs `DataType` ne sont pas des attributs de validation.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-176">`DataType` attributes are not validation attributes.</span></span> <span data-ttu-id="0b1c2-177">Dans l’exemple d’application, seule la date est affichée, sans l’heure.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-177">In the sample application, only the date is displayed, without time.</span></span>

<span data-ttu-id="0b1c2-178">L’énumération `DataType` fournit de nombreux types de données, tels que Date, Time, PhoneNumber, Currency ou EmailAddress.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-178">The `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more.</span></span> <span data-ttu-id="0b1c2-179">L’attribut `DataType` peut également permettre à l’application de fournir automatiquement des fonctionnalités propres au type.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-179">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="0b1c2-180">Par exemple, il est possible de créer un lien `mailto:` pour `DataType.EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-180">For example, a `mailto:` link can be created for `DataType.EmailAddress`.</span></span> <span data-ttu-id="0b1c2-181">Un sélecteur de date peut être fourni pour `DataType.Date` dans les navigateurs qui prennent en charge HTML5.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-181">A date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="0b1c2-182">Les attributs `DataType` émettent des attributs HTML 5 `data-` utilisés par les navigateurs HTML 5.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-182">The `DataType` attributes emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="0b1c2-183">Les attributs `DataType` ne fournissent **aucune** validation.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-183">The `DataType` attributes do **not** provide any validation.</span></span>

<span data-ttu-id="0b1c2-184">`DataType.Date` ne spécifie pas le format de la date qui s’affiche.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-184">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="0b1c2-185">Par défaut, le champ de données est affiché conformément aux formats par défaut basés sur le `CultureInfo` du serveur.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-185">By default, the data field is displayed according to the default formats based on the server's `CultureInfo`.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="0b1c2-186">L’annotation de données `[Column(TypeName = "decimal(18, 2)")]` est nécessaire pour qu’Entity Framework Core puisse correctement mapper `Price` en devise dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-186">The `[Column(TypeName = "decimal(18, 2)")]` data annotation is required so Entity Framework Core can correctly map `Price` to currency in the database.</span></span> <span data-ttu-id="0b1c2-187">Pour plus d’informations, consultez [Types de données](/ef/core/modeling/relational/data-types).</span><span class="sxs-lookup"><span data-stu-id="0b1c2-187">For more information, see [Data Types](/ef/core/modeling/relational/data-types).</span></span>

::: moniker-end

<span data-ttu-id="0b1c2-188">L’attribut `DisplayFormat` est utilisé pour spécifier explicitement le format de date :</span><span class="sxs-lookup"><span data-stu-id="0b1c2-188">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

<span data-ttu-id="0b1c2-189">Le paramètre `ApplyFormatInEditMode` indique que la mise en forme doit être appliquée quand la valeur est affichée à des fins de modification.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-189">The `ApplyFormatInEditMode` setting specifies that the formatting should be applied when the value is displayed for editing.</span></span> <span data-ttu-id="0b1c2-190">Vous pouvez souhaiter ce comportement pour certains champs.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-190">You might not want that behavior for some fields.</span></span> <span data-ttu-id="0b1c2-191">Par exemple, dans les valeurs monétaires, vous ne voulez probablement pas le symbole monétaire dans l’interface utilisateur de modification.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-191">For example, in currency values, you probably don't want the currency symbol in the edit UI.</span></span>

<span data-ttu-id="0b1c2-192">L’attribut `DisplayFormat` peut être utilisé seul, mais il est généralement préférable d’utiliser l’attribut `DataType`.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-192">The `DisplayFormat` attribute can be used by itself, but it's generally a good idea to use the `DataType` attribute.</span></span> <span data-ttu-id="0b1c2-193">L’attribut `DataType` donne la sémantique des données, au lieu de d’expliquer comment les afficher sur un écran. Il présente, par ailleurs, les avantages suivants dont vous ne bénéficiez pas avec DisplayFormat :</span><span class="sxs-lookup"><span data-stu-id="0b1c2-193">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with DisplayFormat:</span></span>

* <span data-ttu-id="0b1c2-194">Le navigateur peut activer des fonctionnalités HTML5 (par exemple pour afficher un contrôle de calendrier, le symbole monétaire correspondant aux paramètres régionaux, des liens de messagerie, etc.).</span><span class="sxs-lookup"><span data-stu-id="0b1c2-194">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, etc.)</span></span>
* <span data-ttu-id="0b1c2-195">Par défaut, le navigateur affiche les données à l’aide du format correspondant à vos paramètres régionaux.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-195">By default, the browser will render data using the correct format based on your locale.</span></span>
* <span data-ttu-id="0b1c2-196">L’attribut `DataType` peut permettre à l’infrastructure ASP.NET Core de choisir le modèle de champ approprié pour afficher les données.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-196">The `DataType` attribute can enable the ASP.NET Core framework to choose the right field template to render the data.</span></span> <span data-ttu-id="0b1c2-197">S’il est utilisé seul, `DisplayFormat` utilise le modèle de chaîne.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-197">The `DisplayFormat` if used by itself uses the string template.</span></span>

<span data-ttu-id="0b1c2-198">Remarque : La validation jQuery ne fonctionne pas avec l’attribut `Range` et `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-198">Note: jQuery validation doesn't work with the `Range` attribute and `DateTime`.</span></span> <span data-ttu-id="0b1c2-199">Par exemple, le code suivant affiche toujours une erreur de validation côté client, même quand la date se trouve dans la plage spécifiée :</span><span class="sxs-lookup"><span data-stu-id="0b1c2-199">For example, the following code will always display a client-side validation error, even when the date is in the specified range:</span></span>

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

<span data-ttu-id="0b1c2-200">Il n’est généralement pas recommandé de compiler des dates en dur dans vos modèles. Par conséquent, l’utilisation de l’attribut `Range` et de `DateTime` est déconseillée.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-200">It's generally not a good practice to compile hard dates in your models, so using the `Range` attribute and `DateTime` is discouraged.</span></span>

<span data-ttu-id="0b1c2-201">Le code suivant illustre la combinaison d’attributs sur une seule ligne :</span><span class="sxs-lookup"><span data-stu-id="0b1c2-201">The following code shows combining attributes on one line:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDAmult.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRatingDAmult.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="0b1c2-202">La rubrique [Bien démarrer avec Razor Pages et Entity Framework Core](xref:data/ef-rp/intro) présente des opérations EF Core avancées avec Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-202">[Get started with Razor Pages and EF Core](xref:data/ef-rp/intro) shows advanced EF Core operations with Razor Pages.</span></span>

### <a name="publish-to-azure"></a><span data-ttu-id="0b1c2-203">Publier sur Azure</span><span class="sxs-lookup"><span data-stu-id="0b1c2-203">Publish to Azure</span></span>

<span data-ttu-id="0b1c2-204">Pour plus d’informations sur le déploiement sur Azure, consultez [Tutoriel : Créer une application ASP.NET dans Azure avec SQL Database](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase).</span><span class="sxs-lookup"><span data-stu-id="0b1c2-204">For information on deploying to Azure, See [Tutorial: Build an ASP.NET app in Azure with SQL Database](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase).</span></span> <span data-ttu-id="0b1c2-205">Ces instructions s’appliquent à une application ASP.NET, pas à une application ASP.NET Core, mais les étapes sont les mêmes.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-205">These instructions are for an ASP.NET app, not an ASP.NET Core app, but the steps are the same.</span></span>

<span data-ttu-id="0b1c2-206">Nous vous remercions d’avoir effectué cette introduction aux pages Razor.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-206">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="0b1c2-207">Votre avis nous intéresse.</span><span class="sxs-lookup"><span data-stu-id="0b1c2-207">We appreciate feedback.</span></span> <span data-ttu-id="0b1c2-208">Pour compléter ce tutoriel, vous pouvez consulter [Bien démarrer avec Razor Pages et EF Core](xref:data/ef-rp/intro).</span><span class="sxs-lookup"><span data-stu-id="0b1c2-208">[Get started with Razor Pages and EF Core](xref:data/ef-rp/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0b1c2-209">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="0b1c2-209">Additional resources</span></span>

* <xref:mvc/views/working-with-forms>
* <xref:fundamentals/localization>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/authoring>

> [!div class="step-by-step"]
> [<span data-ttu-id="0b1c2-210">Précédent : Ajout d’un nouveau champ</span><span class="sxs-lookup"><span data-stu-id="0b1c2-210">Previous: Adding a new field</span></span>](xref:tutorials/razor-pages/new-field)
