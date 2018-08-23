---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
title: Présentation des Pages Web ASP.NET - saisie de données de base de données à l’aide de formulaires | Microsoft Docs
author: tfitzmac
description: Ce didacticiel vous montre comment créer un formulaire d’entrée, puis entrez les données que vous obtenez à partir du formulaire dans une table de base de données lorsque vous utilisez ASP.NET Web Pages (...)
ms.author: riande
ms.date: 05/28/2015
ms.assetid: d37c93fc-25fd-4e94-8671-0d437beef206
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
msc.type: authoredcontent
ms.openlocfilehash: 453ed888bc219175c5a3a25f857dfa79ab22b608
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838695"
---
<a name="introducing-aspnet-web-pages---entering-database-data-by-using-forms"></a>Présentation des Pages Web ASP.NET - saisie de données de base de données à l’aide de formulaires
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> Ce didacticiel vous montre comment créer un formulaire d’entrée, puis entrez les données que vous obtenez à partir du formulaire dans une table de base de données lorsque vous utilisez les Pages Web ASP.NET (Razor). Il part du principe que vous avez terminé la série via [principes de base de formulaires HTML dans ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251581).
> 
> Ce que vous allez apprendre :
> 
> - Plus d’informations sur le traitement des formulaires de saisie.
> - Comment ajouter les données (insert) dans une base de données.
> - Comment s’assurer que les utilisateurs ont entré une valeur requise dans un formulaire (comment valider l’entrée utilisateur).
> - Comment afficher les erreurs de validation.
> - Comment accéder à une autre page à partir de la page actuelle.
>   
> 
> Fonctionnalités technologies abordées :
> 
> - Méthode `Database.Execute`
> - Le code SQL `Insert Into` instruction
> - Le `Validation` helper.
> - Méthode `Response.Redirect`


## <a name="what-youll-build"></a>Ce que vous allez générer

Dans le didacticiel plus haut qui vous a montré comment créer une base de données, vous avez entré les données de la base de données en modifiant la base de données directement dans WebMatrix, travaillant dans le **base de données** espace de travail. Dans la plupart des applications, qui n’est pas un moyen pratique de placer des données dans la base de données, cependant. Par conséquent, dans ce didacticiel, vous allez créer une interface basée sur le web qui vous permet de vous ou un utilisateur d’entrer des données et l’enregistrer dans la base de données.

Vous allez créer une page où vous pouvez entrer de nouveaux films. La page contient un formulaire d’entrée qui a des champs (zones de texte) dans lequel vous pouvez entrer un titre du film, genre et année. La page doit ressembler à cette page :

![Ajouter des films de page dans le navigateur](entering-data/_static/image1.png)

Les zones de texte sera HTML `<input>` éléments qui cherchera, tels que ce balisage :

`<input type="text" name="genre" value="" />`

## <a name="creating-the-basic-entry-form"></a>Création du formulaire d’entrée de base

Créez une page nommée *AddMovie.cshtml*.

Remplacez ce qui est dans le fichier avec le balisage suivant. Remplacer tout ; vous allez ajouter un bloc de code en haut peu de temps.

[!code-cshtml[Main](entering-data/samples/sample1.cshtml)]

Cet exemple montre le HTML standard pour la création d’un formulaire. Il utilise `<input>` éléments pour les zones de texte et pour le bouton Envoyer. Les légendes pour les zones de texte sont créés à l’aide standard `<label>` éléments. Le `<fieldset>` et `<legend>` éléments placés une jolie boîte autour du formulaire.

Notez que dans cette page, le `<form>` élément utilise `post` comme valeur pour le `method` attribut. Dans le didacticiel précédent, vous avez créé un formulaire utilisé le `get` (méthode). Qui était correcte, car bien que le formulaire envoyé des valeurs pour le serveur, la demande ne pas apporter des modifications. Tous les c’était le cas a été récupérer des données de différentes façons. Toutefois, dans cette page vous *sera* apporter des modifications, vous allez ajouter de nouveaux enregistrements de base de données. Par conséquent, cette forme doit utiliser le `post` (méthode). (Pour plus d’informations sur la différence entre `GET` et `POST` opérations, consultez le[GET, POST et sécurité du verbe HTTP](https://go.microsoft.com/fwlink/?LinkId=251581#GET,_POST,_and_HTTP_Verb_Safety) encadré dans le didacticiel précédent.)

Notez que chaque zone de texte a un `name` élément (`title`, `genre`, `year`). Comme vous l’avez vu dans le didacticiel précédent, ces noms sont importantes, car vous devez disposer de ces noms afin d’obtenir l’entrée d’utilisateur ultérieurement. Vous pouvez utiliser des noms. Il est utile d’utiliser des noms explicites qui vous aident à mémoriser les données que vous travaillez avec.

Le `value` attribut de chaque `<input>` élément contient un peu de code Razor (par exemple, `Request.Form["title"]`). Vous avez appris une version de cette astuce dans le didacticiel précédent pour conserver la valeur entrée dans la zone de texte (le cas échéant) une fois que le formulaire a été envoyé.

## <a name="getting-the-form-values"></a>Obtenir les valeurs de formulaire

Ensuite, vous ajoutez le code qui traite le formulaire. Dans la structure, vous allez effectuer ce qui suit :

1. Vérifiez si la page est en cours de publication (a été envoyé). Vous souhaitez que votre code à exécuter uniquement quand les utilisateurs ont cliqué avec le bouton, pas lors de la première exécution de la page.
2. Obtenir les valeurs entrées par l’utilisateur dans les zones de texte. Dans ce cas, étant donné que le formulaire est à l’aide de la `POST` verbe, vous obtenez les valeurs de formulaire à partir de la `Request.Form` collection.
3. Insérez les valeurs en tant qu’un nouvel enregistrement dans le *films* table de base de données.

En haut du fichier, ajoutez le code suivant :

[!code-cshtml[Main](entering-data/samples/sample2.cshtml)]

Les premières lignes créent des variables (`title`, `genre`, et `year`) pour contenir les valeurs dans les zones de texte. La ligne `if(IsPost)` permet de s’assurer que les variables sont définies *uniquement* lorsque les utilisateurs cliquent sur le **film ajouter** bouton, autrement dit, lorsque le formulaire a été validé.

Comme vous l’avez vu dans un tutoriel précédent, vous obtenez la valeur d’une zone de texte à l’aide d’une expression telle que `Request.Form["name"]`, où *nom* est le nom de la `<input>` élément.

Les noms des variables (`title`, `genre`, et `year`) sont arbitraires. Comme les noms que vous attribuez à `<input>` éléments, vous pouvez les appeler comme vous le souhaitez. (Les noms des variables n’êtes pas obligé de correspondent aux attributs de nom `<input>` éléments sur le formulaire.) Mais comme avec la `<input>` éléments, il est judicieux d’utiliser des noms de variables qui reflètent les données qu’ils contiennent. Lorsque vous écrivez du code, des noms cohérents rendent plus facile à retenir les données avec lequel vous travaillez.

## <a name="adding-data-to-the-database"></a>Ajout de données à la base de données

Dans le code vous empêche vient d’être ajoutée, il vous suffit *à l’intérieur de* l’accolade fermante ( `}` ) de la `if` bloc (et pas seulement à l’intérieur du bloc de code), ajoutez le code suivant :

[!code-csharp[Main](entering-data/samples/sample3.cs)]

Cet exemple est similaire au code que vous avez utilisé dans le didacticiel précédent pour extraction et affichage des données. La ligne qui commence par `db =` ouvre la base de données, comme avant et la ligne suivante définit une instruction SQL, à nouveau en tant que vous avez vu avant. Toutefois, cette fois-ci il définit un SQL `Insert Into` instruction. L’exemple suivant montre la syntaxe générale de la `Insert Into` instruction :

`INSERT INTO table (column1, column2, column3, ...) VALUES (value1, value2, value3, ...)`

En d’autres termes, vous spécifiez la table pour insérer, puis répertoriez les colonnes à insérer, puis répertorier les valeurs à insérer. (Comme mentionné précédemment, SQL n’est pas sensible à la casse, mais certaines personnes capitaliser les mots clés pour le rendre plus facile à lire la commande.)

Les colonnes que vous insérez dans figurent déjà dans la commande — `(Title, Genre, Year)`. La partie intéressante est la façon dont vous obtenez les valeurs dans les zones de texte dans le `VALUES` dans le cadre de la commande. Au lieu des valeurs réelles, vous voyez `@0`, `@1`, et `@2`, qui sont bien entendu des espaces réservés. Lorsque vous exécutez la commande (sur le `db.Execute` ligne), vous passez les valeurs que vous avez obtenu les zones de texte.

**Important !** N’oubliez pas que la seule façon, vous devez jamais inclure des données saisies en ligne par un utilisateur dans une instruction SQL doit utiliser des espaces réservés, comme vous le voyez ici (`VALUES(@0, @1, @2)`). Si vous concaténez des entrées d’utilisateur dans une instruction SQL, vous ouvrez vous-même à une attaque par injection SQL, comme expliqué dans [principes fondamentaux de formulaire dans ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251581) (le didacticiel précédent).

Toujours dans le `if` bloquer, ajoutez la ligne suivante après la `db.Execute` ligne :

[!code-css[Main](entering-data/samples/sample4.css)]

Une fois que le nouveau film a été inséré dans la base de données, cette ligne vous (redirections) accède à la *films* page afin que vous puissiez voir le film que vous venez d’entrer. Le `~` opérateur signifie « racine du site Web ». (Le `~` opérateur fonctionne uniquement dans les pages ASP.NET, pas au format HTML en général.)

Le bloc de code complet se présente comme dans cet exemple :

[!code-cshtml[Main](entering-data/samples/sample5.cshtml)]

## <a name="testing-the-insert-command-so-far"></a>Test de la commande d’insertion (jusqu'à présent)

Vous n’avez pas encore terminé, mais il êtes maintenant temps de tester.

Dans l’arborescence de fichiers dans WebMatrix, cliquez sur le *AddMovie.cshtml* page, puis cliquez sur **lancer dans le navigateur**.

![Ajouter des films de page dans le navigateur](entering-data/_static/image2.png)

(Si vous retrouvez avec une autre page dans le navigateur, assurez-vous que l’URL est `http://localhost:nnnnn/AddMovie`), où *nnnnn* est le numéro de port que vous utilisez.)

Avez-vous reçu une page d’erreur ? Dans ce cas, lisez-le attentivement et assurez-vous que le code se présente exactement ce qui a été indiqué précédemment.

Entrez un film dans le formulaire &mdash; , par exemple, utilisez « Citoyen Kane », « Drama » et « 1941 ». (Ou dans toute autre.) Puis cliquez sur **film ajouter**.

Si tout se passe bien, vous êtes redirigé vers la *films* page. Assurez-vous que votre nouveau film est répertorié.

![Page de films montrant qui vient d’être ajouté de film](entering-data/_static/image3.png)

## <a name="validating-user-input"></a>Validation des entrées utilisateur

Revenez à la *AddMovie* page ou exécuter de nouveau. Entrez un autre film, mais cette fois, entrez uniquement le titre &mdash; par exemple, entrez « Domicile ' dans Rain ». Puis cliquez sur **film ajouter**.

Vous êtes redirigé vers la *films* page à nouveau. Vous trouverez ce nouveau film, mais il est incomplet.

![Films page affichant nouveau film auquel il manque certaines valeurs](entering-data/_static/image4.png)

Lorsque vous avez créé le *films* table, vous avez explicitement déclaré qu’aucun des champs peut être null. Ici, vous avez un formulaire de saisie de nouveaux films, et vous êtes laisser les champs vides. C’est une erreur.

Dans ce cas, la base de données n’a pas réellement augmenter (ou *lever*) une erreur. Vous n’avez pas fournir un genre ou une année, par conséquent, le code dans le *AddMovie* page traité ces valeurs en tant que ce que l'on appelle *des chaînes vides*. Lorsque le code SQL `Insert Into` commande s’est exécutée, les champs de genre et année n’a pas des données utiles, mais elles n’étaient pas null.

Évidemment, vous ne souhaitez pas permettre aux utilisateurs d’entrer des informations sur les films de moitié vide dans la base de données. La solution consiste à valider l’entrée utilisateur. Initialement, la validation permet simplement de garantir que l’utilisateur a entré une valeur pour tous les champs (autrement dit, qu’aucun d'entre eux contient une chaîne vide).

> [!TIP]
> 
> **Chaînes null et vides**
> 
> Dans la programmation, il existe une distinction entre les différentes notions sur « aucune valeur ». En règle générale, une valeur est *null* s’il n’a jamais été définie ou initialisé en aucune façon. En revanche, une variable qui attend des données caractères (chaînes) peut être définie sur une *une chaîne vide*. Dans ce cas, la valeur n’est pas null ; elle a juste été explicitement définie dans une chaîne de caractères dont la longueur est égale à zéro. Les deux instructions suivantes montrent la différence :
> 
> [!code-csharp[Main](entering-data/samples/sample6.cs)]
> 
> C’est un peu plus compliqué que celle, mais le point important est que `null` représente le tri d’un état indéterminé.
> 
> Maintenant, puis il est important de comprendre exactement quand une valeur est null, et quand il est simplement une chaîne vide. Dans le code pour le *AddMovie* page, vous obtenez les valeurs des zones de texte à l’aide de `Request.Form["title"]` et ainsi de suite. Lorsque la page de première exécution (avant de cliquer sur le bouton), la valeur de `Request.Form["title"]` est null. Mais lorsque vous envoyez le formulaire, `Request.Form["title"]` Obtient la valeur de la `title` zone de texte. Il n’est pas évident, mais une zone de texte vide n’est pas null ; elle doit simplement une chaîne vide qu’il contient. Par conséquent, lorsque le code s’exécute en réponse au bouton Cliquez sur, `Request.Form["title"]` comporte une chaîne vide.
> 
> Pourquoi cette distinction est importante ? Lorsque vous avez créé le *films* table, vous avez explicitement déclaré qu’aucun des champs peut être null. Mais ici, vous avez un formulaire de saisie de nouveaux films et vous êtes laisser les champs vides. Vous serez raisonnablement s’attendre à la base de données pour lequel vous souhaitez lorsque vous avez essayé d’enregistrer de nouveaux films que n’avait pas de valeurs pour le genre ou année. Mais c’est le point de &mdash; même si vous ne renseignez pas ces zones de texte, les valeurs ne sont pas null ; elles sont des chaînes vides. Par conséquent, vous êtes en mesure d’enregistrer de nouveaux films à la base de données avec ces colonnes vides &mdash; mais pas null ! des valeurs &mdash;. Par conséquent, vous devez vous assurer que les utilisateurs n’envoient pas une chaîne vide, ce que vous pouvez faire en validant l’entrée utilisateur.


### <a name="the-validation-helper"></a>L’application auxiliaire de Validation

Les Pages Web ASP.NET inclut un programme d’assistance &mdash; le `Validation` helper &mdash; que vous pouvez utiliser pour vous assurer que les utilisateurs entrent des données qui répond à vos besoins. Le `Validation` helper est un des programmes d’assistance qui est intégrée à ASP.NET Web Pages, donc vous n’êtes pas obligé d’installer un package à l’aide de NuGet, le mode d’installation de l’application d’assistance Gravatar dans un tutoriel précédent.

Pour valider l’entrée d’utilisateur, vous allez effectuer ce qui suit :

- Utilisez le code pour spécifier que vous souhaitez requièrent des valeurs dans les zones de texte sur la page.
- Placez un test dans le code afin que les informations de film sont ajoutées à la base de données uniquement si tous les éléments valide correctement.
- Ajoutez le code dans le balisage pour afficher les messages d’erreur.

Dans le bloc de code dans le *AddMovie* page, à droite vers le haut en haut avant les déclarations de variable, ajoutez le code suivant :

[!code-csharp[Main](entering-data/samples/sample7.cs)]

Vous appelez `Validation.RequireField` une fois pour chaque champ (`<input>` élément) dans lequel vous souhaitez exiger une entrée. Vous pouvez également ajouter un message d’erreur personnalisé pour chaque appel, comme indiqué ici. (Nous faisant varier les messages uniquement pour montrer que vous pouvez de stocker comme vous le souhaitez).

S’il existe un problème, vous souhaitez empêcher les nouvelles informations de film à partir de laquelle elle est insérée à la base de données. Dans le `if(IsPost)` bloquer, utilisez `&&` (AND logique) pour ajouter une autre condition qui teste `Validation.IsValid()`. Lorsque vous avez terminé, la totalité `if(IsPost)` bloc ressemble à ce code :

[!code-csharp[Main](entering-data/samples/sample8.cs)]

S’il existe une erreur de validation avec les champs que vous avez inscrite à l’aide de la `Validation` helper, le `Validation.IsValid` méthode retourne la valeur false. Et dans ce cas, le code dans ce bloc s’exécute, donc aucune entrée de film non valide ne sera insérée dans la base de données. Et bien sûr vous n’êtes pas redirigé vers la *films* page.

Le bloc de code complet, y compris le code de validation, ressemble à ceci :

[!code-cshtml[Main](entering-data/samples/sample9.cshtml?highlight=10)]

## <a name="displaying-validation-errors"></a>Affichage des erreurs de Validation

La dernière étape consiste à afficher les messages d’erreur. Vous pouvez afficher des messages individuels pour chaque erreur de validation, ou vous pouvez afficher un résumé, ou les deux. Pour ce didacticiel, vous ferez afin que vous puissiez voir son fonctionnement.

En regard de chaque `<input>` élément que vous êtes à la validation, appelez le `Html.ValidationMessage` (méthode) et lui transmettre le nom de la `<input>` élément que vous êtes à la validation. Vous placez le `Html.ValidationMessage` droite méthode où vous souhaitez le message d’erreur s’affiche. Lorsque la page s’exécute, le `Html.ValidationMessage` méthode restitue un `<span>` élément où l’erreur de validation passe. (Si aucune erreur n’est, le `<span>` élément est affiché, mais aucun texte n’est qu’il contient.)

Modifiez le balisage dans la page afin qu’il inclue un `Html.ValidationMessage` méthode pour chacun des trois `<input>` éléments sur la page, comme dans cet exemple :

[!code-cshtml[Main](entering-data/samples/sample10.cshtml?highlight=3,8,13)]

Pour observer le fonctionne de la synthèse, ajouter le balisage suivant et en code juste après le `<h1>Add a Movie</h1>` élément sur la page :

[!code-cshtml[Main](entering-data/samples/sample11.cshtml)]

Par défaut, le `Html.ValidationSummary` méthode affiche tous les messages de validation dans une liste (un `<ul>` élément qui se trouve dans un `<div>` élément). Comme avec la `Html.ValidationMessage` (méthode), le balisage pour le résumé des validations est alors toujours restitué ; s’il n’existe aucune erreur, aucun élément de liste n’est rendus.

Le résumé peut être une autre façon d’afficher les messages de validation au lieu d’à l’aide de la `Html.ValidationMessage` méthode pour afficher chaque erreur spécifiques aux champs. Ou vous pouvez utiliser un résumé et les détails. Ou vous pouvez utiliser la `Html.ValidationSummary` méthode pour afficher une erreur générique, puis utilisez individuels `Html.ValidationMessage` appels pour afficher les détails.

La page complète se présente désormais comme dans cet exemple :

[!code-cshtml[Main](entering-data/samples/sample12.cshtml)]

C'est tout ! Vous pouvez maintenant tester la page par l’ajout d’un film, mais en omettant une ou plusieurs des champs. Lorsque vous le faites, vous consultez l’erreur suivante à afficher :

![Ajouter une page de film montrant des messages d’erreur de validation](entering-data/_static/image5.png)

## <a name="styling-the-validation-error-messages"></a>Les Messages d’erreur de Validation de conception de styles

Vous pouvez voir que les messages d’erreur, mais ils ne se distinguent réellement très bien. Il existe un moyen simple d’appliquer un style cependant les messages d’erreur.

Pour définir les messages d’erreur individuels qui sont affichés par le style `Html.ValidationMessage`, créez une classe de style CSS nommée `field-validation-error`. Pour définir l’apparence pour le résumé des validations, créez une classe de style CSS nommée `validation-summary-errors`.

Pour voir comment cette technique fonctionne, ajoutez un `<style>` élément à l’intérieur du `<head>` section de la page. Définissez ensuite style `field-validation-error` et `validation-summary-errors` qui contiennent les règles suivantes :

[!code-cshtml[Main](entering-data/samples/sample13.cshtml?highlight=4-17)]

Normalement vous placeriez probablement les informations de style dans un distinct *.css* fichier, mais par souci de simplicité vous pouvez les placer dans la page pour l’instant. (Plus loin dans cet ensemble de didacticiels, vous allez déplacer les règles CSS pour distinct *.css* fichier.)

S’il existe une erreur de validation, le `Html.ValidationMessage` méthode restitue un `<span>` élément inclut `class="field-validation-error"`. En ajoutant une définition de style pour cette classe, vous pouvez configurer à quoi ressemble le message. S’il existe des erreurs, le `ValidationSummary` méthode restitue même dynamiquement l’attribut `class="validation-summary-errors"`.

Réexécutez la page et laissez délibérément deux champs. Les erreurs sont désormais plus visibles. (En fait, ils êtes allé trop loin, mais c’est uniquement pour montrer ce que vous pouvez faire.)

![Ajouter une page de film montrant les erreurs de validation qui ont été mise en forme](entering-data/_static/image6.png)

## <a name="adding-a-link-to-the-movies-page"></a>Ajout d’un lien vers la Page de films

La dernière étape consiste à rendre pratique pour accéder à la *AddMovie* page à partir de la liste de films d’origine.

Ouvrez le *films* page à nouveau. Après la fermeture `</div>` balise qui suit le `WebGrid` helper, ajoutez le balisage suivant :

[!code-cshtml[Main](entering-data/samples/sample14.cshtml)]

Comme vous l’avez vu précédemment, ASP.NET interprète le `~` opérateur en tant que la racine du site Web. Vous n’êtes pas obligé d’utiliser le `~` opérateur ; vous pouvez utiliser le balisage `<a href="./AddMovie">Add a movie</a>` ou une autre façon de définir le chemin d’accès qui comprend du code HTML. Mais le `~` opérateur constitue une bonne approche générale lorsque vous créez des liens pour les pages Razor, car il permet de définir le site plus souple : Si vous déplacez la page actuelle dans un sous-dossier, le lien sera toujours atteindre le *AddMovie* page. (N’oubliez pas que le `~` opérateur fonctionne uniquement dans *.cshtml* pages. ASP.NET comprend, mais il n’est pas HTML standard.)

Lorsque vous avez terminé, exécutez le *films* page. Il ressemblera à cette page :

![Page de films avec un lien vers la page « Ajouter Movies »](entering-data/_static/image7.png)

Cliquez sur le **ajouter un film** lien pour vous assurer qu’il accède à la *AddMovie* page.

## <a name="coming-up-next"></a>Prochaine

Dans le didacticiel suivant, vous allez apprendre à permettre aux utilisateurs de modifier les données qui se trouve déjà dans la base de données.

## <a name="complete-listing-for-addmovie-page"></a>Liste complète de la Page de AddMovie

[!code-cshtml[Main](entering-data/samples/sample15.cshtml)]

## <a name="additional-resources"></a>Ressources supplémentaires

- [Introduction à la programmation Web ASP.NET à l’aide de la syntaxe Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Instruction SQL INSERT INTO](http://www.w3schools.com/sql/sql_insert.asp) sur le site W3Schools
- [Validation des entrées d’utilisateur dans ASP.NET Web Pages des Sites](https://go.microsoft.com/fwlink/?LinkId=253002). Plus d’informations sur l’utilisation de la `Validation` helper.

> [!div class="step-by-step"]
> [Précédent](form-basics.md)
> [Suivant](updating-data.md)
