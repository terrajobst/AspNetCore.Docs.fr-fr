---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: Validation des entrées d’utilisateur dans ASP.NET Web Pages (Razor) Sites | Microsoft Docs
author: Rick-Anderson
description: Cet article explique comment valider des informations provenant d’utilisateurs &mdash; , autrement dit, s’assurer que les utilisateurs entrent valide les informations au format HTML forms dans un sous...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 8f049adce33e452896b5e2a444635ff30d18e480
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021519"
---
<a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a>Validation des entrées d’utilisateur dans les Sites ASP.NET Web Pages (Razor)
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article explique comment valider des informations provenant d’utilisateurs &mdash; , autrement dit, s’assurer que les utilisateurs entrent valide les informations au format HTML forms dans un site ASP.NET Web Pages (Razor).
> 
> Ce que vous allez apprendre :
> 
> - Comment vérifier qu’un entrée de l’utilisateur correspond aux critères de validation que vous définissez.
> - Comment déterminer si tous les tests de validation ont réussi.
> - Comment afficher les erreurs de validation (et comment les mettre en forme).
> - Comment valider les données qui ne proviennent directement les utilisateurs.
> 
> Il s’agit des concepts présentés dans l’article de programmation ASP.NET :
> 
> - Le `Validation` helper.
> - Le `Html.ValidationSummary` et `Html.ValidationMessage` méthodes.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Ce didacticiel fonctionne également avec ASP.NET Web Pages 2.


Cet article contient les sections suivantes :

- [Vue d’ensemble de la Validation des entrées utilisateur](#Overview_of_User_Input_Validation)
- [Validation des entrées utilisateur](#Validating_User_Input)
- [Ajout d’une Validation côté Client](#Adding_Client-Side_Validation)
- [Mise en forme des erreurs de Validation](#Formatting_Validation_Errors)
- [Validation des données qui ne proviennent directement d’utilisateurs](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a>Vue d’ensemble de la Validation des entrées utilisateur

Si vous demandez aux utilisateurs d’entrer des informations dans une page, par exemple, dans un formulaire, il est important de s’assurer que les valeurs qui entrent sont valides. Par exemple, vous ne souhaitez pas traiter un formulaire auquel il manque des informations critiques.

Quand les utilisateurs entrent des valeurs dans un formulaire HTML, ils entrent les valeurs sont des chaînes. Dans de nombreux cas, les valeurs que vous avez besoin sont certains autres types de données, tels que des entiers ou des dates. Par conséquent, vous devez également vous assurer que les valeurs que les utilisateurs entrent peuvent être convertis correctement aux types de données approprié.

Vous aurez peut-être également certaines restrictions sur les valeurs. Même si les utilisateurs entrent correctement un entier, par exemple, vous devrez peut-être vous assurer que la valeur est comprise dans une certaine plage.

![Erreurs de validation qui utilisent des classes de style CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> **Important** validation des entrées utilisateur est également important pour la sécurité. Lorsque vous limitez les valeurs que les utilisateurs peuvent entrer dans les formulaires, vous réduisez le risque que quelqu'un peut entrer une valeur qui peut compromettre la sécurité de votre site.


<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a>Validation des entrées utilisateur

Dans ASP.NET Web Pages 2, vous pouvez utiliser le `Validator` helper pour tester l’entrée d’utilisateur. L’approche de base consiste à effectuer les opérations suivantes :

1. Déterminer quels d’entrée des éléments (champs) que vous souhaitez valider.

    Vous validez généralement les valeurs dans `<input>` éléments dans un formulaire. Toutefois, il est conseillé de valider toutes les entrées, de même d’entrée qui provient d’un élément contraint comme un `<select>` liste. Cela permet de s’assurer que les utilisateurs ne pas ignorer les contrôles sur une page et envoyer un formulaire.
2. Dans le code de la page, ajouter des contrôles de validation individuels pour chaque élément d’entrée à l’aide des méthodes de la `Validation` helper.

    Pour vérifier les champs obligatoires, utilisez `Validation.RequireField(field, [error message])` (pour un champ individuel) ou `Validation.RequireFields(field1, field2, ...))` (pour obtenir la liste de champs). Pour les autres types de validation, utilisez `Validation.Add(field, ValidationType)`. Pour `ValidationType`, vous pouvez utiliser ces options :

    `Validator.DateTime ([error message])`  
   `Validator.Decimal([error message])`  
   `Validator.EqualsTo(otherField [, error message])`  
   `Validator.Float([error message])`  
   `Validator.Integer([error message])`  
   `Validator.Range(min, max [, error message])`  
   `Validator.RegEx(pattern [, error message])`  
   `Validator.Required([error message])`  
   `Validator.StringLength(length)`  
   `Validator.Url([error message])`
3. Lorsque la page est envoyée, vérifiez si la validation a réussi en vérifiant `Validation.IsValid`:

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    S’il existe des erreurs de validation, vous ignorez le traitement de la page normal. Par exemple, si l’objectif de la page est mise à jour une base de données, vous ne le faites que jusqu'à ce que toutes les erreurs de validation ont été résolus.
4. S’il existe des erreurs de validation, afficher des messages d’erreur dans le balisage de la page à l’aide de `Html.ValidationSummary` ou `Html.ValidationMessage`, ou les deux.

L’exemple suivant montre une page qui illustre ces étapes.

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

Pour observer le fonctionne de la validation, exécutez cette page et délibérément commettent des erreurs. Par exemple, voici comment la page se présente si vous oubliez d’entrer un nom de cours, si vous entrez un, et si vous entrez une date non valide :

![Erreurs de validation dans la page rendue](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a>Ajout d’une Validation côté Client

Par défaut, l’entrée d’utilisateur est validée, une fois que les utilisateurs envoient la page, autrement dit, la validation est effectuée dans le code serveur. L’inconvénient de cette approche est que les utilisateurs ne savent qu’ils avez apporté une erreur jusqu'à ce qu’après qu’ils envoient la page. Si un formulaire est longue ou complexe, peut être gênant à l’utilisateur de rapports d’erreurs uniquement une fois que la page est envoyée.

Vous pouvez ajouter la prise en charge pour effectuer une validation dans le script client. Dans ce cas, la validation est effectuée dans le navigateur. Par exemple, supposons que vous spécifiez qu’une valeur doit être un entier. Si un utilisateur entre une valeur non entière, l’erreur est signalée, dès que l’utilisateur quitte le champ d’entrée. Les utilisateurs obtiennent des commentaires immédiats, qui est très utile pour eux. Validation basée sur le client peut également réduire le nombre de fois où l’utilisateur doit envoyer le formulaire pour corriger plusieurs erreurs.

> [!NOTE]
> Même si vous utilisez la validation côté client, la validation est également toujours effectuée dans le code serveur. La validation dans le code serveur d’est une mesure de sécurité, au cas où les utilisateurs contournent la validation basée sur le client.


1. Inscrire les bibliothèques JavaScript suivants dans la page :  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   Deux des bibliothèques sont chargés à partir d’un réseau de distribution de contenu (CDN), afin que vous ne sont pas nécessairement à les entrer sur votre ordinateur ou serveur. Toutefois, vous devez disposer d’une copie locale de *jquery.validate.unobtrusive.js*. Si vous ne travaillez pas déjà avec un modèle de WebMatrix (comme **Starter Site** ) qui inclut la bibliothèque, créez un site Web Pages qui repose sur **Starter Site**. Copiez ensuite le *.js* fichier vers votre site actuel.
2. Dans le balisage, pour chaque élément que vous êtes validez, ajoutez un appel à `Validation.For(field)`. Cette méthode émet des attributs qui sont utilisés par la validation côté client. (Au lieu de l’émission de code JavaScript réel, la méthode émet des attributs tels que `data-val-...`. Ces attributs prennent en charge la validation client non obstructifs qui utilise jQuery pour effectuer le travail.)

La page suivante montre comment ajouter des fonctionnalités de validation client pour l’exemple présentée.

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

Pas toutes les vérifications de validation exécutées sur le client. En particulier, la validation de type de données (entier, date, etc.) ne s’exécutent sur le client. Les vérifications suivantes fonctionnent sur le client et le serveur :

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

Dans cet exemple, le test d’une date valide ne fonctionnera pas dans le code client. Toutefois, le test sera exécuté dans le code serveur.

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a>Mise en forme des erreurs de Validation

Vous pouvez contrôler l’affichent des erreurs de validation en définissant les classes CSS qui ont des noms réservés suivants :

- `field-validation-error`. Définit la sortie de la `Html.ValidationMessage` méthode lorsqu’il affiche une erreur.
- `field-validation-valid`. Définit la sortie de la `Html.ValidationMessage` méthode lorsqu’il n’existe aucune erreur.
- `input-validation-error`. Définit comment `<input>` éléments sont rendus lorsqu’il existe une erreur. (Par exemple, vous pouvez utiliser cette classe pour définir la couleur d’arrière-plan une &lt;d’entrée&gt; élément sur une couleur différente si sa valeur n’est pas valide.) Cette classe CSS est utilisée uniquement lors de la validation de client (dans ASP.NET Web Pages 2).
- `input-validation-valid`. Définit l’apparence des `<input>` éléments lorsqu’il n’existe aucune erreur.
- `validation-summary-errors`. Définit la sortie de la `Html.ValidationSummary` méthode il affiche une liste d’erreurs.
- `validation-summary-valid`. Définit la sortie de la `Html.ValidationSummary` méthode lorsqu’il n’existe aucune erreur.

Ce qui suit `<style>` bloc montre les règles de conditions d’erreur.

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

Si vous incluez ce bloc de style dans l’exemple des pages précédemment dans cet article, l’affichage de l’erreur ressemblera à l’illustration suivante :

![Erreurs de validation qui utilisent des classes de style CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> Si vous n’utilisez pas validation côté client dans ASP.NET Web Pages 2, les classes CSS pour le `<input>` éléments (`input-validation-error` et `input-validation-valid` n’a aucun effet.


### <a name="static-and-dynamic-error-display"></a>Affichage des erreurs statiques et dynamiques

Les règles CSS sont fournis par paires, tels que `validation-summary-errors` et `validation-summary-valid`. Ces paires vous permettent de définir des règles pour les deux conditions : une condition d’erreur et une condition (non-error) « normale ». Il est important de comprendre que le balisage pour l’affichage des erreurs est alors toujours restitué, même si aucune erreur. Par exemple, si une page a un `Html.ValidationSummary` méthode dans le balisage, la page source contient le balisage suivant même lorsque la page est demandée pour la première fois :

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

En d’autres termes, le `Html.ValidationSummary` méthode restitue toujours un `<div>` élément et une liste, même si la liste d’erreurs est vide. De même, le `Html.ValidationMessage` méthode restitue toujours un `<span>` élément comme espace réservé pour une erreur de champ individuel, même si aucune erreur.

Dans certaines situations, affichant un message d’erreur peuvent entraîner la page à la redistribution et peut entraîner des éléments dans la page à déplacer. Les règles CSS qui se terminent par `-valid` vous permettent de définir une disposition qui peut aider à éviter ce problème. Par exemple, vous pouvez définir `field-validation-error` et `field-validation-valid` pour les deux ont la même taille fixe. De cette façon, la zone d’affichage pour le champ est statique et ne change pas le flux de page si un message d’erreur s’affiche.

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a>Validation des données qui ne proviennent directement d’utilisateurs

Parfois, vous devez valider les informations qui ne proviennent directement d’un formulaire HTML. Un exemple classique est une page où une valeur est passée dans une chaîne de requête, comme dans l’exemple suivant :

`http://server/myapp/EditClassInformation?classid=1022`

Dans ce cas, vous souhaitez vous assurer que la valeur est passée à la page (ici, 1022 pour la valeur de `classid`) est valide. Vous ne pouvez pas utiliser directement le `Validation` helper pour effectuer cette validation. Toutefois, vous pouvez utiliser d’autres fonctionnalités du système de contrôle, comme la possibilité d’afficher des messages d’erreur de validation.

> [!NOTE] 
> 
> **Important** toujours valider les valeurs que vous obtenez à partir de *n’importe quel* source, y compris les valeurs de champ de formulaire, les valeurs de chaîne de requête et valeurs de cookie. Il est facile pour les personnes à modifier ces valeurs (par exemple à des fins malveillantes). Par conséquent, vous devez vérifier ces valeurs afin de protéger votre application.


L’exemple suivant montre comment vous pouvez valider une valeur qui est passée dans une chaîne de requête. Le code vérifie que la valeur n’est pas vide et qu’il s’agit d’un entier.

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

Notez que le test est effectué lorsque la demande n’est pas une soumission de formulaire (`if(!IsPost)`). Ce test peut passer la première fois que la page est demandée, mais pas lorsque la demande est une soumission de formulaire.

Pour afficher cette erreur, vous pouvez ajouter l’erreur à la liste des erreurs de validation en appelant `Validation.AddFormError("message")`. Si la page contient un appel à la `Html.ValidationSummary` (méthode), l’erreur s’affiche, comme une erreur de validation d’entrée d’utilisateur.

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

[Utilisation des formulaires HTML dans les Sites ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkID=202892)
