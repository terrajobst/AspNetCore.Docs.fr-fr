---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
title: Présentation des Pages Web ASP.NET - mise à jour de la base de données | Microsoft Docs
author: tfitzmac
description: Ce didacticiel vous montre comment mettre à jour (modifier) une base de données entrée lorsque vous utilisez les Pages Web ASP.NET (Razor). Il part du principe que vous avez terminé la série e...
ms.author: riande
ms.date: 01/02/2018
ms.assetid: ac86ec9c-6b69-485b-b9e0-8b9127b13e6b
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
msc.type: authoredcontent
ms.openlocfilehash: 9d0da2eed9964e56a01f3c811d6a14c9d1735e10
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835666"
---
<a name="introducing-aspnet-web-pages---updating-database-data"></a>Présentation des Pages Web ASP.NET - mise à jour de la base de données
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> Ce didacticiel vous montre comment mettre à jour (modifier) une base de données entrée lorsque vous utilisez les Pages Web ASP.NET (Razor). Il part du principe que vous avez terminé la série via [saisie de données par à l’aide de formulaires à l’aide des Pages Web ASP.NET](entering-data.md).
> 
> Ce que vous allez apprendre :
> 
> - Comment sélectionner un enregistrement individuel dans le `WebGrid` helper.
> - Guide pratique pour lire un enregistrement unique à partir d’une base de données.
> - Découvrez comment précharger un formulaire avec les valeurs de l’enregistrement de base de données.
> - Comment mettre à jour un enregistrement existant dans une base de données.
> - Guide pratique pour stocker les informations dans la page sans l’afficher.
> - Comment utiliser un champ masqué pour stocker des informations.
>   
> 
> Fonctionnalités technologies abordées :
> 
> - Le `WebGrid` helper.
> - Le code SQL `Update` commande.
> - Méthode `Database.Execute`
> - Champs masqués (`<input type="hidden">`).


## <a name="what-youll-build"></a>Ce que vous allez générer

Dans le didacticiel précédent, vous avez appris comment ajouter un enregistrement à une base de données. Ici, vous allez apprendre à afficher un enregistrement pour la modification. Dans le *films* page, vous allez mettre à jour le `WebGrid` helper afin qu’elle affiche un **modifier** lien en regard de chaque film :

![WebGrid affiche notamment un lien « Modifier » pour chaque film](updating-data/_static/image1.png)

Lorsque vous cliquez sur le **modifier** lien, il permet d’accéder à une autre page, où les informations de film sont déjà dans un formulaire :

![Modifier la page de film montrant le film à modifier](updating-data/_static/image2.png)

Vous pouvez modifier les valeurs. Lorsque vous envoyez les modifications, le code dans la page mises à jour de la base de données et vous ramène à la liste de films.

Cette partie du processus fonctionne presque exactement comme le *AddMovie.cshtml* page que vous avez créé dans le didacticiel précédent, par conséquent, une grande partie de ce didacticiel vous seront familier.

Il existe plusieurs façons vous pouvez implémenter un moyen de modifier un film individuel. L’approche illustrée a été choisie, car il est facile à implémenter et facile à comprendre.

## <a name="adding-an-edit-link-to-the-movie-listing"></a>Ajout d’un lien de modification à la liste de films

Pour commencer, vous allez mettre à jour le *films* page afin que chaque film répertoriant également contient un **modifier** lien.

Ouvrez le *Movies.cshtml* fichier.

Dans le corps de la page, modifiez le `WebGrid` balisage en ajoutant une colonne. Voici le balisage modifié :

[!code-html[Main](updating-data/samples/sample1.html?highlight=6)]

La nouvelle colonne est celle-ci :

[!code-html[Main](updating-data/samples/sample2.html)]

Le point de cette colonne est de montrer un lien (`<a>` élément) dont le texte indique « Modifier ». Qui nous intéresse à consiste à créer un lien qui ressemble à ceci lorsque la page s’exécute, avec la `id` valeur différente pour chaque film :

[!code-css[Main](updating-data/samples/sample3.css)]

Ce lien appellera une page nommée *EditMovie*, et il passe la chaîne de requête `?id=7` à cette page.

La syntaxe pour la nouvelle colonne peut sembler un peu complexe, mais qui est uniquement, car elle réunit plusieurs éléments. Chaque élément individuel est simple. Si vous vous concentrer sur uniquement le `<a>` élément, vous voyez ce balisage :

[!code-html[Main](updating-data/samples/sample4.html)]

Certaines notions concernant le fonctionnement de la grille : la grille affiche les lignes, une pour chaque enregistrement de la base de données, et il affiche les colonnes pour chaque champ dans l’enregistrement de base de données. Bien que chaque ligne de la grille est en cours de construction, le `item` objet contient l’enregistrement de base de données (élément) pour cette ligne. Ceci vous donne un moyen dans le code pour accéder à des données pour cette ligne. C’est ce que vous voyez ici : l’expression `item.ID` reçoit la valeur d’ID de l’élément actuel de la base de données. Vous pouvez obtenir les valeurs de base de données (titre, genre et année) de la même façon en utilisant `item.Title`, `item.Genre`, ou `item.Year`.

L’expression `"~/EditMovie?id=@item.ID` combine la partie codée en dur de l’URL cible (`~/EditMovie?id=`) avec l’ID dynamiquement dérivé. (Vous l’avez vu le `~` opérateur dans le didacticiel précédent ; il s’agit un opérateur d’ASP.NET qui représente la racine du site Web en cours.)

Le résultat est que cette partie du balisage dans la colonne génère simplement quelque chose comme le balisage suivant au moment de l’exécution :

[!code-xml[Main](updating-data/samples/sample5.xml)]

Naturellement, la valeur réelle de `id` sera différente pour chaque ligne.

## <a name="creating-a-custom-display-for-a-grid-column"></a>Création d’un affichage personnalisé pour une colonne de grille

Maintenant, retournons à la colonne de grille. Les trois colonnes vous aviez dans les grille affichée seules valeurs de données (titre, genre et année). Vous avez spécifié cet affichage en passant le nom de la colonne de base de données &mdash; , par exemple, `grid.Column("Title")`.

Cette nouvelle **modifier** colonne de lien est différent. Au lieu de spécifier un nom de colonne, vous transmettez un `format` paramètre. Ce paramètre vous permet de définir le balisage qui le `WebGrid` helper affichera avec le `item` valeur à afficher les données de colonne en caractères gras ou vert ou dans le format que vous souhaitez. Par exemple, si vous souhaitiez le titre à afficher en gras, vous pouvez créer une colonne comme dans cet exemple :

[!code-html[Main](updating-data/samples/sample6.html)]

(Les différents `@` caractères qui s’affichent dans le `format` propriété marquer la transition entre le balisage et une valeur de code.)

Une fois que vous connaissez le `format` propriété, il est plus facile à comprendre comment la nouvelle **modifier** colonne de lien est mis en place :

[!code-html[Main](updating-data/samples/sample7.html)]

La colonne se compose *uniquement* du balisage qui restitue le lien, ainsi que certaines informations (ID) qui est extrait à partir de l’enregistrement de base de données pour la ligne.

> [!TIP]
> 
> **Paramètres nommés et les paramètres positionnels d’une méthode**
> 
> Nombre de fois où lorsque vous avez appelé une méthode et des paramètres passés à ce dernier, vous avez répertoriées simplement les valeurs de paramètres séparés par des virgules. Voici quelques exemples :
> 
> `db.Execute(insertCommand, title, genre, year)`
> 
> `Validation.RequireField("title", "You must enter a title")`
> 
> Nous n’avez pas signaler le problème lorsque vous l’avez vu tout d’abord ce code, mais dans chaque cas, vous êtes passage de paramètres aux méthodes dans un ordre spécifique &mdash; à savoir, l’ordre dans lequel les paramètres sont définis dans cette méthode. Pour `db.Execute` et `Validation.RequireFields`, si une combinaison de l’ordre des valeurs que vous passez, vous obtiendriez un message d’erreur lorsque la page s’exécute, ou au moins des résultats inattendus. Clairement, vous devez connaître l’ordre pour passer les paramètres dans. (Dans WebMatrix, IntelliSense peut vous aider à apprendre figure le nom, le type et l’ordre des paramètres.)
> 
> Comme alternative à la transmission de valeurs dans l’ordre, vous pouvez utiliser *des paramètres nommés*. (Passage de paramètres dans l’ordre est appelé à l’aide de *paramètres positionnels*.) Pour les paramètres nommés, vous incluez explicitement le nom du paramètre lors du passage de sa valeur. Vous avez utilisé les paramètres nommés déjà un nombre de fois dans ces didacticiels. Exemple :
> 
> [!code-csharp[Main](updating-data/samples/sample8.cs)]
> 
> et
> 
> [!code-css[Main](updating-data/samples/sample9.css)]
> 
> Paramètres nommés sont pratiques pour deux situations, surtout quand une méthode accepte un nombre de paramètres. Un est lorsque vous souhaitez transmettre uniquement un ou deux paramètres, mais les valeurs que vous souhaitez transmettre ne sont pas entre les positions de premier dans la liste de paramètres. Une autre situation est lorsque vous souhaitez rendre votre code plus lisible en transmettant les paramètres dans l’ordre correspondant le mieux pour vous.
> 
> Bien sûr, pour utiliser des paramètres nommés, vous devez connaître les noms des paramètres. WebMatrix IntelliSense peut *afficher* vous les noms, mais il ne peut pas actuellement les remplir pour vous.


## <a name="creating-the-edit-page"></a>Création de la Page de modification

Vous pouvez désormais créer le *EditMovie* page. Lorsque les utilisateurs cliquent sur le **modifier** lien, ils vais me retrouver sur cette page.

Créez une page nommée *EditMovie.cshtml* et remplacer celui qui figure dans le fichier avec le balisage suivant :

[!code-cshtml[Main](updating-data/samples/sample10.cshtml)]

Ce balisage et le code est similaire à ce que vous avez dans le *AddMovie* page. Il existe une légère différence dans le texte du bouton Envoyer. Comme avec la *AddMovie* page, il existe un `Html.ValidationSummary` appel qui affiche les erreurs de validation, le cas échéant. Cette fois-ci, nous allons en omettant les appels à `Validation.Message`, dans la mesure où les erreurs sont affichées dans le résumé des validations. Comme indiqué dans le didacticiel précédent, vous pouvez utiliser le résumé des validations et les messages d’erreur individuels dans différentes combinaisons.

Notez à nouveau que le `method` attribut de la `<form>` élément est défini sur `post`. Comme avec la *AddMovie.cshtml* page, cette page apporte des modifications à la base de données. Par conséquent, ce formulaire doit effectuer une `POST` opération. (Pour plus d’informations sur la différence entre `GET` et `POST` opérations, consultez le [GET, POST et sécurité du verbe HTTP](form-basics.md#GET,_POST,_and_HTTP_Verb_Safety) encadré dans le didacticiel sur les formulaires HTML.)

Comme vous l’avez vu dans un tutoriel précédent, le `value` attributs des zones de texte sont définies avec le code Razor pour précharger les. Cette fois, cependant, vous utilisez les variables comme `title` et `genre` pour cette tâche au lieu de `Request.Form["title"]`:

`<input type="text" name="title" value="@title" />`

Comme avant, ce balisage sera précharger les valeurs de zone de texte avec les valeurs de film. Vous verrez dans un moment pourquoi il est utile d’utiliser des variables instant au lieu d’utiliser le `Request` objet.

Il existe également un `<input type="hidden">` élément sur cette page. Cet élément enregistre l’ID de film sans le rendre visible sur la page. L’ID est initialement passée à la page en utilisant une valeur de chaîne de requête (`?id=7` ou similaire dans l’URL). En plaçant la valeur d’ID dans un champ masqué, vous assurer qu’il est disponible lorsque le formulaire est envoyé, même si vous n’avez plus accès à l’URL d’origine que la page a été appelée avec.

Contrairement à la *AddMovie* page, le code pour le *EditMovie* page a deux fonctions distinctes. La première fonction est que lorsque la page s’affiche pour la première fois (et *uniquement* puis), le code obtient l’ID de film à partir de la chaîne de requête. Le code, puis utilise l’ID pour lire le film correspondant en dehors de la base de données et afficher (précharge) il dans les zones de texte.

La deuxième fonction est que lorsque l’utilisateur clique sur le **soumettre les modifications** bouton, le code doit lire les valeurs des zones de texte et de les valider. Le code a également mettre à jour l’élément de base de données avec les nouvelles valeurs. Cette technique est similaire à l’ajout d’un enregistrement, comme vous l’avez vu dans *AddMovie*.

## <a name="adding-code-to-read-a-single-movie"></a>Ajout de Code pour lire un film

Pour effectuer la première fonction, ajoutez ce code vers le haut de la page :

[!code-cshtml[Main](updating-data/samples/sample11.cshtml)]

La majeure partie de ce code est dans un bloc qui commence `if(!IsPost)`. Le `!` opérateur signifie « not », l’expression signifie *si cette demande n’est pas une soumission post*, qui est un moyen indirect de dire *si cette demande est la première fois que cette page a été exécutée*. Comme indiqué précédemment, ce code doit s’exécuter *uniquement* lors de la première exécution de la page. Si vous n’avez pas placer le code dans `if(!IsPost)`, elle est exécutée chaque fois que la page est appelée, si la première fois ou en réponse à un bouton, cliquez sur.

Notez que le code inclut un `else` bloquer cette fois-ci. Comme nous l’avons dit lorsque nous avons introduit `if` blocs, parfois vous souhaitez exécuter le code de remplacement si la condition que vous testez n’est pas vrai. Qui est le cas ici. Si la condition est transmise (autrement dit, si l’ID transmise à la page est OK), vous avez lu une ligne à partir de la base de données. Toutefois, si la condition ne remplit pas la `else` bloc s’exécute et le code définit un message d’erreur.

## <a name="validating-a-value-passed-to-the-page"></a>Validation d’une valeur passée à la Page

Le code utilise `Request.QueryString["id"]` pour obtenir l’ID qui est passé à la page. Le code permet de s’assurer qu’une valeur a été réellement passée pour le code. Si aucune valeur n’a été passée, le code définit une erreur de validation.

Ce code montre une manière différente pour valider les informations. Dans le didacticiel précédent, vous déjà travaillé avec le `Validation` helper. Vous avez inscrit des champs à valider, et ASP.NET fait la validation automatiquement et affiche les erreurs à l’aide de `Html.ValidationMessage` et `Html.ValidationSummary`. Dans ce cas, toutefois, vous n'êtes pas vraiment validation des entrées utilisateur. Au lieu de cela, vous êtes validation d’une valeur qui a été passée à la page depuis un autre emplacement. Le `Validation` helper ne fait pas pour vous.

Par conséquent, vous vérifiez la valeur vous-même, en le testant avec `if(!Request.QueryString["ID"].IsEmpty()`). S’il existe un problème, vous pouvez afficher l’erreur à l’aide de `Html.ValidationSummary`, comme vous l’avez fait avec le `Validation` helper. Pour ce faire, vous appelez `Validation.AddFormError` et passez-lui un message à afficher. `Validation.AddFormError` est une méthode intégrée qui vous permet de définir des messages personnalisés lier avec le système de validation que vous connaissez déjà. (Plus loin dans ce didacticiel nous allons parler comment rendre ce processus de validation un peu plus robuste.)

Après avoir vérifié qu’il existe un ID pour le film, le code lit la base de données, vous recherchez uniquement un élément de la base de données unique. (Vous avez probablement remarqué le modèle général pour les opérations de base de données : ouvrir la base de données, définissez une instruction SQL, et exécutez l’instruction.) Cette fois-ci, le code SQL `Select` instruction inclut `WHERE ID = @0`. Étant donné que l’ID est unique, un seul enregistrement peut être retourné.

La requête est exécutée à l’aide de `db.QuerySingle` (pas `db.Query`, comme vous avez utilisé pour obtenir la liste de films), et le code place le résultat dans le `row` variable. Le nom `row` est arbitraire ; vous pouvez nommer les variables comme vous le souhaitez. Les variables initialisées en haut sont ensuite remplis avec les détails de films afin que ces valeurs peuvent être affichées dans les zones de texte.

## <a name="testing-the-edit-page-so-far"></a>Test de la Page de modification (jusqu'à présent)

Si vous souhaitez tester votre page, exécutez le *films* page maintenant, cliquez sur un **modifier** lien en regard de n’importe quel film. Vous verrez la *EditMovie* page présentant les informations remplies pour le film que vous avez sélectionné :

![Modifier la page de film montrant le film à modifier](updating-data/_static/image3.png)

Notez que l’URL de la page inclut quelque chose comme `?id=10` (ou tout autre numéro). Jusqu'à présent, vous avez testé qui **modifier** des liens dans le *film* page travail, que votre page lit l’ID de la chaîne de requête, et que la base de données une requête pour obtenir un enregistrement vidéo unique fonctionne.

Vous pouvez modifier les informations sur les films, mais rien ne se produit lorsque vous cliquez sur **soumettre les modifications**.

## <a name="adding-code-to-update-the-movie-with-the-users-changes"></a>Ajout de Code pour mettre à jour le film avec les modifications apportées par l’utilisateur

Dans le *EditMovie.cshtml* de fichier, pour implémenter la deuxième fonction (enregistrer les modifications), ajoutez le code suivant à l’intérieur de l’accolade fermante de la `@` bloc. (Si vous n’êtes pas sûr de savoir exactement où placer le code, vous pouvez examiner le [l’intégralité du code pour la page Modifier le film](#Complete_Page_Listing_for_EditMovie) qui s’affiche à la fin de ce didacticiel.)

[!code-csharp[Main](updating-data/samples/sample12.cs)]

Là encore, ce balisage et le code est similaire au code dans *AddMovie*. Le code se trouve dans un `if(IsPost)` bloquer, étant donné que ce code s’exécute uniquement lorsque l’utilisateur clique sur le **soumettre les modifications** bouton &mdash; , autrement dit, si (et seulement si) le formulaire a été publié. Dans ce cas, vous n’utilisez pas un test comme `if(IsPost && Validation.IsValid())`, autrement dit, vous combinez pas les deux tests à l’aide d’and. Dans cette page, vous d’abord déterminer s’il existe une soumission de formulaire (`if(IsPost)`) et alors seulement inscrire les champs pour la validation. Puis vous pouvez tester les résultats de validation (`if(Validation.IsValid()`). Le flux est légèrement différent de celle dans le *AddMovie.cshtml* page, mais l’effet est le même.

Vous obtenez les valeurs des zones de texte à l’aide de `Request.Form["title"]` et un code similaire pour les autres `<input>` éléments. Notez que cette fois, le code obtient l’ID de film hors du champ masqué (`<input type="hidden">`). Lorsque la page a exécuté la première fois, le code obtenu l’ID de la chaîne de requête. Vous obtenez la valeur du champ masqué pour vous assurer que vous obtenez l’ID de la vidéo qui a été affichée à l’origine, au cas où la chaîne de requête a été modifiée depuis.

La différence très importante entre le *AddMovie* code et ce code est d’utiliser le code SQL dans ce code `Update` instruction au lieu du `Insert Into` instruction. L’exemple suivant montre la syntaxe de l’instruction SQL `Update` instruction :

`UPDATE table SET col1="value", col2="value", col3="value" ... WHERE ID = value`

Vous pouvez spécifier toutes les colonnes dans n’importe quel ordre, et vous n’avez plus à mettre à jour chaque colonne pendant une `Update` opération. (Vous ne peut pas mettre à jour l’ID lui-même, car cela permet d’économiser en vigueur l’enregistrement en tant qu’un nouvel enregistrement, et qui n’est pas autorisée pour un `Update` opération.)

> [!NOTE] 
> 
> **Important** le `Where` clause avec l’ID est très important, parce que c’est la façon dont la base de données sait quelle base de données enregistrement que vous souhaitez mettre à jour. Si vous vous étiez arrêté le `Where` clause, la base de données est mise à jour *chaque* enregistrements dans la base de données. Dans la plupart des cas, qui serait un incident.


Dans le code, les valeurs à mettre à jour sont passés à l’instruction SQL à l’aide des espaces réservés. Répéter ce que nous avons dit avant : pour des raisons de sécurité, *uniquement* utiliser des espaces réservés pour transmettre des valeurs à une instruction SQL.

Une fois que le code utilise `db.Execute` pour exécuter le `Update` instruction, il redirige vers la page de liste, où vous pouvez voir les modifications.

> [!TIP] 
> 
> **Instructions SQL différente, différentes méthodes**
> 
> Vous avez peut-être remarqué que vous utiliser des méthodes légèrement différentes pour exécuter des instructions SQL différentes. Pour exécuter un `Select` requête potentiellement renvoie plusieurs enregistrements, vous utilisez le `Query` (méthode). Pour exécuter un `Select` requête qui retournera un seul élément de base de données, vous utilisez le `QuerySingle` (méthode). Pour exécuter des commandes qui apporter des modifications, mais qui ne retournent pas les éléments de base de données, vous utilisez le `Execute` (méthode).
> 
> Vous devez disposer de différentes méthodes, car chacun d’eux retourne des résultats différents, comme vous l’avez vu déjà la différence entre `Query` et `QuerySingle`. (Le `Execute` méthode renvoie en fait une valeur également &mdash; à savoir, le nombre de lignes de base de données qui ont été affectés par la commande &mdash; , mais vous avez été en ignorant qui jusqu'à présent.)
> 
> Bien entendu, le `Query` méthode peut retourner qu’une seule ligne de base de données. Toutefois, ASP.NET traite toujours les résultats de la `Query` méthode en tant que collection. Même si la méthode retourne une seule ligne, vous devez extraire cette ligne unique de la collection. Par conséquent, dans les situations où vous *savoir* vous recevez qu’une seule ligne, c’est un peu plus pratique d’utiliser `QuerySingle`.
> 
> Il existe quelques autres méthodes qui effectuent des types spécifiques d’opérations de base de données. Vous trouverez une liste des méthodes de base de données dans le [référence rapide de l’API ASP.NET Web Pages](../../api-reference/asp-net-web-pages-api-reference.md#Data).


## <a name="making-validation-for-the-id-more-robust"></a>Effectue une Validation pour obtenir l’ID de plus robuste

La première fois que la page s’exécute, vous obtenez l’ID de film à partir de la chaîne de requête pour que vous pouvez accéder à obtenir ce film à partir de la base de données. Vous être assuré qu’il était réellement une valeur à rechercher, qui vous l’avez fait à l’aide de ce code :

[!code-csharp[Main](updating-data/samples/sample13.cs)]

Vous avez utilisé ce code pour vous assurer que si un utilisateur obtient à la *EditMovies* page sans un film en sélectionnant d’abord le *films* page, la page affiche un message d’erreur convivial. (Sinon, les utilisateurs verriez une erreur qui aurait probablement simplement les confondre.)

Toutefois, cette validation n’est pas très robuste. La page peut également être appelée avec ces erreurs :

- L’ID n’est pas un nombre. Par exemple, la page peut être appelée avec une URL telle que `http://localhost:nnnnn/EditMovie?id=abc`.
- L’ID est un nombre, mais elle fait référence à un film n’existe pas (par exemple, `http://localhost:nnnnn/EditMovie?id=100934`).

Si vous êtes impatient de voir les erreurs qui résultent de ces URL, exécutez le *films* page. Sélectionnez un film à modifier, puis modifiez l’URL de la *EditMovie* page à une URL qui contient une liste alphabétique, ID ou l’ID d’un film inexistantes.

Par conséquent, que devez-vous faire ? La première solution consiste à s’assurer que non seulement est un ID transmis à la page, mais que l’ID est un entier. Modifier le code pour le `!IsPost` test ressemble à cet exemple :

[!code-csharp[Main](updating-data/samples/sample14.cs)]

Vous avez ajouté une deuxième condition à la `IsEmpty` test, lié à `&&` (AND logique) :

[!code-csharp[Main](updating-data/samples/sample15.cs)]

Vous vous souvenez peut-être à partir de la [Introduction à la programmation de Pages Web ASP.NET](../introducing-razor-syntax-c.md) didacticiel méthodes telles que `AsBool` un `AsInt` convertir une chaîne de caractères en un autre type de données. Le `IsInt` (méthode) (et d’autres, telles que `IsBool` et `IsDateTime`) sont similaires. Toutefois, ils testent uniquement si vous *pouvez* convertir la chaîne, sans réellement effectuer la conversion. Je vais vous dites essentiellement *si la valeur de chaîne de requête peut être convertie en entier...* .

L’autre problème potentiel recherche un film n’existe pas. Le code pour obtenir un film ressemble à ce code :

[!code-csharp[Main](updating-data/samples/sample16.cs)]

Si vous passez un `movieId` valeur pour le `QuerySingle` méthode qui ne correspond pas à un film réel, rien n’est retourné et les instructions qui suivent (par exemple, `title=row.Title`) entraîne des erreurs.

Là encore il est facile à corriger. Si le `db.QuerySingle` méthode ne retourne aucun résultat, le `row` variable sera null. Par conséquent, vous pouvez vérifier si le `row` variable a la valeur null avant d’essayer d’obtenir des valeurs à partir de celui-ci. Le code suivant ajoute un `if` bloc autour des instructions permettant d’obtenir les valeurs de la `row` objet :

[!code-csharp[Main](updating-data/samples/sample17.cs)]

Avec ces deux tests de validation supplémentaires, la page devient plus à toute épreuve. Le code complet pour le `!IsPost` branche se présente désormais comme dans cet exemple :

[!code-csharp[Main](updating-data/samples/sample18.cs)]

Nous allons Notez encore une fois que cette tâche est un bon usage pour un `else` bloc. Si vous ne transmettez pas les tests, le `else` blocs définir des messages d’erreur.

## <a name="adding-a-link-to-return-to-the-movies-page"></a>Ajout d’un lien pour revenir à la Page de films

Un détail final et utile consiste à ajouter un lien vers le *films* page. Dans le flux ordinaire d’événements, les utilisateurs commenceront à la *films* page et cliquez sur un **modifier** lien. Ce qui amène la *EditMovie* page, où ils peuvent modifier la vidéo et cliquez sur le bouton. Une fois que le code traité la modification, il redirige vers le *films* page.

Cependant :

- L’utilisateur peut ne pas modifier quoi que ce soit.
- Éventuelles de l’utilisateur à cette page sans cliquer d’abord sur un **modifier** lien dans le *films* page.

Dans les deux cas, que vous souhaitez faciliter pour les faire revenir à la liste principale. Il est facile à corriger &mdash; ajoutez le balisage suivant juste après la fermeture `</form>` balise dans le balisage :

[!code-html[Main](updating-data/samples/sample19.html)]

Ce balisage utilise la même syntaxe pour une `<a>` élément que vous avez vu ailleurs. L’URL inclut `~` comme signifiant « racine du site Web ».

## <a name="testing-the-movie-update-process"></a>Tester le processus de mise à jour de film

Vous pouvez maintenant tester. Exécutez le *films* page, puis cliquez sur **modifier** en regard d’un film. Lorsque le *EditMovie* page s’affiche, apporter des modifications à la vidéo et cliquez sur **soumettre les modifications**. Lorsque la liste de films s’affiche, assurez-vous que vos modifications sont affichées.

Pour vous assurer que la validation fonctionne, cliquez sur **modifier** pour un autre film. Lorsque vous arrivez à la *EditMovie* page, désactivez le **Genre** champ (ou **année** champ, ou les deux) et essayez d’envoyer vos modifications. Vous verrez une erreur, comme vous pouvez l’imaginer :

![Modifier la page de film montrant les erreurs de validation](updating-data/_static/image4.png)

Cliquez sur le **revenir à la liste de films** lien pour abandonner vos modifications et revenir à la *films* page.

## <a name="coming-up-next"></a>Prochaine

Dans le didacticiel suivant, vous verrez comment supprimer un enregistrement de film.

## <a name="complete-listing-for-movie-page-updated-with-edit-links"></a>Intégralité de la Page de film (mis à jour avec les liens d’édition)

[!code-cshtml[Main](updating-data/samples/sample20.cshtml)]

<a id="Complete_Page_Listing_for_EditMovie"></a>
## <a name="complete-page-listing-for-edit-movie-page"></a>Terminer l’annonce de Page pour la Page de modification du film

[!code-cshtml[Main](updating-data/samples/sample21.cshtml)]

## <a name="additional-resources"></a>Ressources supplémentaires

- [Introduction à la programmation Web ASP.NET en utilisant la syntaxe Razor](../../getting-started/introducing-razor-syntax-c.md)
- [Instruction de mise à jour SQL](http://www.w3schools.com/sql/sql_update.asp) sur le site W3Schools

> [!div class="step-by-step"]
> [Précédent](entering-data.md)
> [Suivant](deleting-data.md)
