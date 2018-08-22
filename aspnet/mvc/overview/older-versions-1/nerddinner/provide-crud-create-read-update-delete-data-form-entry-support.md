---
uid: mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
title: Fournir CRUD (créer, lire, mettre à jour, supprimer) les formulaires de données prise en charge de l’entrée | Microsoft Docs
author: microsoft
description: Étape 5 montre comment faire passer notre classe DinnersController davantage à prise en charge de l’activer pour modification, la création et la suppression dîners avec lui aussi.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: bbb976e5-6150-4283-a374-c22fbafe29f5
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
msc.type: authoredcontent
ms.openlocfilehash: 45d74249a34fc7e37e9776a398615d2f613a7582
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830543"
---
<a name="provide-crud-create-read-update-delete-data-form-entry-support"></a>Fournir CRUD (créer, lire, mettre à jour, supprimer) les formulaires de données prise en charge de l’entrée
====================
par [Microsoft](https://github.com/microsoft)

[Télécharger PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il s’agit d’étape 5 a gratuit [« « l’application NerdDinner](introducing-the-nerddinner-tutorial.md) qui présente en détail comment créer un petit mais terminé, l’application web à l’aide d’ASP.NET MVC 1.
> 
> Étape 5 montre comment faire passer notre classe DinnersController davantage à prise en charge de l’activer pour modification, la création et la suppression dîners avec lui aussi.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre le [mise en route avec MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [Store de musique MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) didacticiels.


## <a name="nerddinner-step-5-create-update-delete-form-scenarios"></a>NerdDinner étape 5 : Créer, mettre à jour, supprimer des scénarios de formulaire

Nous avons introduit des contrôleurs et des vues et vu comment les utiliser pour implémenter une expérience de liste/détails pour dîners sur site. Notre prochaine étape consistera à aller plus loin dans notre classe DinnersController et activer la prise en charge de la modification, la création et la suppression dîners avec lui aussi.

### <a name="urls-handled-by-dinnerscontroller"></a>URL gérées par DinnersController

Nous avons ajouté précédemment des méthodes d’action à DinnersController qui a implémenté la prise en charge des deux URL : */Dinners* et */Dinners/détails / [id]*.

| **URL** | **VERBE** | **Fonction** |
| --- | --- | --- |
| */Dinners/* | GET | Afficher une liste HTML de dîners à venir. |
| */Dinners/détails / [id]* | GET | Afficher des détails sur un dîner spécifique. |

Nous allons maintenant ajouter des méthodes d’action pour implémenter les trois URL supplémentaires : <em>/Dinners/modification / [id], / dîners/création,</em>et<em>/Dinners/Delete / [id]</em>. Ces URL permettra la prise en charge pour l’éditions dîners existants, création de nouveau dîners et la suppression des dîners.

Nous prendrons en charge les interactions de verbe HTTP GET et HTTP POST à la fois avec ces nouvelles URL. Les requêtes HTTP GET à ces URL affiche la vue HTML initiale des données (un formulaire rempli avec les données dîner dans le cas de « modifier », un formulaire vide dans le cas de « créer » et un écran de confirmation de suppression dans le cas de « delete »). Les requêtes HTTP POST à ces URL seront enregistrement ou de suppression/mise à jour les données dîner dans notre DinnerRepository (et à partir de là à la base de données).

| **URL** | **VERBE** | **Fonction** |
| --- | --- | --- |
| */Dinners/modification / [id]* | GET | Afficher un formulaire HTML modifiable rempli avec les données de dîner. |
| PUBLIER | Enregistrez les modifications de formulaire pour un dîner particulier à la base de données. |
| */Dinners/Create* | GET | Afficher un formulaire HTML vide qui permet aux utilisateurs de définir de nouvelles dîners. |
| PUBLIER | Créer un nouveau dîner et l’enregistrer dans la base de données. |
| */Dinners/Delete/[id]* | GET | Affichage supprimer l’écran de confirmation. |
| PUBLIER | Supprime le dîner spécifié à partir de la base de données. |

### <a name="edit-support"></a>Modifier la prise en charge

Nous allons commencer en implémentant le scénario « modifier ».

#### <a name="the-http-get-edit-action-method"></a>La méthode d’Action de modification HTTP-GET

Nous allons commencer par implémenter le comportement de « GET » HTTP de notre méthode d’action de modification. Cette méthode sera appelée lorsque le */Dinners/modification / [id]* URL est demandée. Notre implémentation doit ressembler :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample1.cs)]

Le code ci-dessus utilise le DinnerRepository pour récupérer un objet dîner. Il restitue ensuite un modèle de vue à l’aide de l’objet dîner. Étant donné que nous n’avons pas passé explicitement un nom de modèle pour le *View()* méthode d’assistance, il utilisera le chemin d’accès de la valeur par défaut basé sur une convention pour résoudre le modèle de vue : /Views/Dinners/Edit.aspx.

Nous allons maintenant créer ce modèle de vue. Pour cela, nous sera en effectuant un clic droit au sein de la méthode Edit, puis en sélectionnant la commande de menu contextuel « Ajouter une vue » :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image1.png)

Dans la boîte de dialogue « Ajouter une vue », nous allons indiquer que nous passent un objet dîner à notre modèle de vue en tant que son modèle et choisissez le modèle « Modifier » à une structure automatique :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image2.png)

Lorsque nous cliquons sur le bouton « Ajouter », Visual Studio ajoute un nouveau fichier de modèle de vue « Edit.aspx » pour nous dans le répertoire « \Views\Dinners ». Il ouvre également le nouveau modèle de vue « Edit.aspx » dans l’éditeur de code – remplie avec une « Modifier » une structure implémentation initiale, comme ci-dessous :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image3.png)

Nous allons apporter quelques modifications à la structure de « modifier » par défaut générée et mettre à jour le modèle de vue de modification pour que le contenu ci-dessous (ce qui supprime un quelques-unes des propriétés que nous ne souhaitons pas exposer) :

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample2.aspx)]

Lorsque nous exécutons l’application et demande le *« / dîners/modifier/1 »* URL nous verrons à la page suivante :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image4.png)

Le balisage HTML généré par notre vue se présente comme suit. Il est au format HTML standard – avec un &lt;formulaire&gt; élément qui effectue une requête HTTP POST vers le */Dinners/Edit/1* URL lors de la « enregistrer » &lt;type d’entrée = « envoyer » /&gt; c’est. Un HTML &lt;input type = « text » /&gt; élément a été de sortie pour chaque propriété modifiable :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image5.png)

#### <a name="htmlbeginform-and-htmltextbox-html-helper-methods"></a>Html.BeginForm() et méthodes de programme d’assistance Html Html.TextBox()

Notre modèle de vue « Edit.aspx » à l’aide de plusieurs méthodes de « Programme d’assistance Html » : Html.ValidationSummary(), Html.BeginForm(), Html.TextBox() et Html.ValidationMessage(). En plus de générer un balisage HTML pour nous, ces méthodes d’assistance fournissent la gestion des erreurs intégrée et validation prennent en charge.

##### <a name="htmlbeginform-helper-method"></a>Méthode d’assistance de Html.BeginForm()

La méthode d’assistance Html.BeginForm() est ce que le code HTML de sortie &lt;formulaire&gt; élément notre balisage. Dans notre modèle de vue Edit.aspx, vous remarquerez que nous appliquons une instruction « using » lors de l’utilisation de cette méthode c#. L’accolade ouvrante indique le début de la &lt;formulaire&gt; contenu et l’accolade fermante est ce qui indique la fin de la &lt;/forment&gt; élément :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample3.cs)]

Vous pouvez également, si vous trouvez l’instruction « using » approche non naturelles pour un scénario comme celui-ci, vous pouvez utiliser une combinaison de Html.BeginForm() et Html.EndForm() (qui fait la même chose) :

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample4.aspx)]

L’appel de Html.BeginForm() sans aucun paramètre entraîne à la sortie d’un élément de formulaire qui exécute une requête HTTP-POST à l’URL de la requête actuelle. Autrement dit pourquoi notre vue Edit génère un *&lt;action de formulaire = méthode « / dîners/modifier/1 » = « post »&gt;* élément. Nous pourrions avoir également passé des paramètres explicites à Html.BeginForm() si nous voulons publier vers une URL différente.

##### <a name="htmltextbox-helper-method"></a>Méthode d’assistance de Html.TextBox()

Notre vue Edit.aspx utilise la méthode d’assistance Html.TextBox() pour générer &lt;input type = « text » /&gt; éléments :

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample5.aspx)]

La méthode Html.TextBox() ci-dessus prend un seul paramètre : qui est utilisé pour spécifier les attributs id/nom de la &lt;d’entrée de type = « text » /&gt; élément à la sortie, ainsi que la propriété de modèle pour remplir la valeur de la zone de texte à partir de. Par exemple, l’objet dîner que nous avons transmis à la vue Edit avait une valeur de propriété « Title » de « Futures de .NET », et donc notre méthode Html.TextBox("Title") appeler sortie : *&lt;id d’entrée = « Title » name = « Title » type = « text » value = « Futures de .NET » /&gt;*.

Ou bien, nous pouvons utiliser le premier paramètre Html.TextBox() pour spécifier l’id/nom de l’élément, puis transmettre explicitement la valeur à utiliser en tant que deuxième paramètre :

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample6.aspx)]

Souvent, nous devrons effectuer la mise en forme personnalisée de la valeur de sortie. La méthode statique String.Format () intégrée à .NET est utile pour ces scénarios. Notre modèle de vue Edit.aspx utilise ceci pour mettre en forme la valeur EventDate (qui est de type date/heure) afin qu’elle n’affiche pas secondes de la durée :

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample7.aspx)]

Un troisième paramètre Html.TextBox() peut éventuellement être utilisé pour générer des attributs HTML supplémentaires. L’extrait de code ci-dessous montre comment restituer une taille supplémentaire = attribut « 30 » et une classe = attribut de « mycssclass » sur le &lt;d’entrée de type = « text » /&gt; élément. Notez comment nous avons échappement le nom de l’attribut de classe en utilisant un «@" character because "classe » est un mot clé réservé en c# :

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample8.aspx)]

#### <a name="implementing-the-http-post-edit-action-method"></a>Implémentation de la méthode d’Action de modification HTTP-POST

Nous disposons désormais la version de HTTP-GET de notre méthode d’action de modification implémentée. Quand un utilisateur demande la */Dinners/Edit/1* URL qu’ils reçoivent une page HTML comme suit :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image6.png)

En appuyant sur le bouton « Enregistrer » entraîne une publication de formulaire à la */Dinners/Edit/1* URL, et envoie le code HTML &lt;d’entrée&gt; valeurs à l’aide du verbe HTTP POST de formulaire. Nous allons maintenant implémenter le comportement de la requête HTTP POST de notre méthode d’action de modification : qui gère l’enregistrement le dîner.

Nous allons commencer en ajoutant une méthode d’action « Modifier » surchargée à notre DinnersController qui a un attribut « AcceptVerbs » sur ce qui indique qu’il gère des scénarios de requête HTTP POST :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample9.cs)]

Lorsque l’attribut [AcceptVerbs] est appliqué aux méthodes d’action surchargé, ASP.NET MVC gère automatiquement la distribution des demandes à la méthode d’action appropriée selon le verbe HTTP entrante. Des demandes HTTP POST à <em>/Dinners/modification / [id]</em> URL passera à la méthode Edit ci-dessus, lors de toutes les autres demandes de verbe HTTP à <em>/Dinners/modification / [id]</em>URL passera à la première méthode de modification, nous avons implémenté (qui pas avoir un attribut [AcceptVerbs]).

| **Rubrique de côté : Pourquoi la différence par le biais de verbes HTTP ?** |
| --- |
| Vous pouvez vous demander : pourquoi en faisons-nous son comportement via le verbe HTTP de plaisantes et différentes à l’aide d’une URL unique ? Pourquoi ne pas simplement avoir deux URL distinctes gère le chargement et l’enregistrement des modifications de la modification ? Par exemple : /Dinners/modification / [id] pour afficher le formulaire initial et /Dinners/Save / [id] pour gérer la publication de formulaire pour l’enregistrer ? L’inconvénient avec publication deux URL distinctes est que dans les cas où nous publier sur /Dinners/Save/2 et vous devez ensuite réafficher le formulaire HTML en raison d’une erreur d’entrée, l’utilisateur final sera finalement l’URL dîners/Save/2 dans la barre d’adresse de son navigateur (étant donné que c’était l’URL du formulaire publié sur). Si l’utilisateur final crée des signets pour cette page redisplayed à leur liste de Favoris de navigateur, ou copie-colle l’URL et l’envoie à un ami, ils finissent l’enregistrement d’une URL qui ne fonctionne pas dans le futur (étant donné que cette URL dépend des valeurs de publication). En exposant une URL unique (tels que : /Dinners/Edit/[id]) et ce qui différencie le traitement de celui-ci par le verbe HTTP, il est sécurisé pour les utilisateurs finaux à la page de modification de signet et/ou envoyer l’URL à d’autres personnes. |

#### <a name="retrieving-form-post-values"></a>Récupération des valeurs de publication de formulaire

Il existe une variété de manières que nous pouvons accéder validée au sein de notre méthode de « Modifier » HTTP POST, les paramètres de formulaire. Une approche simple consiste à simplement utiliser la propriété de la demande sur la classe de base du contrôleur pour accéder à la collection de formulaires et récupérer les valeurs publiées directement :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample10.cs)]

L’approche ci-dessus est un peu verbose, cependant, en particulier une fois que nous ajoutons la logique de gestion des erreurs.

Une meilleure approche pour ce scénario consiste à exploiter intégrés *UpdateModel()* méthode d’assistance sur la classe de base du contrôleur. Il prend en charge la mise à jour les propriétés d’un objet que nous transmettons à l’aide des paramètres entrants de l’écran. Il utilise la réflexion pour déterminer les noms de propriété sur l’objet et puis convertit automatiquement et leur affecte des valeurs en fonction des valeurs d’entrée soumis par le client.

Nous pourrions utiliser la méthode UpdateModel() pour simplifier notre HTTP-POST modifier l’Action à l’aide de ce code :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample11.cs)]

Nous pouvons à présent, consultez le */Dinners/Edit/1* URL, puis remplacez le titre de notre dîner :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image7.png)

Lorsque nous cliquons sur le bouton « Enregistrer », nous allons effectuer une publication de formulaire à notre action de modification, et les valeurs mises à jour sont conservées dans la base de données. Nous sera alors redirigés vers l’URL de détails pour le dîner (qui affiche les valeurs qui vient d’être enregistrées) :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image8.png)

#### <a name="handling-edit-errors"></a>Gestion des erreurs de modification

Notre actuel fonctionne de mise en œuvre de HTTP-POST correctement – sauf quand il existe des erreurs.

Quand un utilisateur commet une erreur édition d’un formulaire, nous avons besoin pour vous assurer que le formulaire est réaffiché avec un message d’erreur informatif qui les guide pour corriger ce problème. Cela inclut les cas où un utilisateur final publie une entrée incorrecte (par exemple : une chaîne de date incorrect), ainsi que les cas où le format d’entrée est valide mais il existe une violation de règle d’entreprise. Lorsque les erreurs se produisent que le formulaire doit conserver les données d’entrée de l’utilisateur a entré à l’origine afin qu’ils n’aient à recharger leurs modifications manuellement. Ce processus doit se répéter autant de fois que nécessaire jusqu'à ce que le formulaire se termine avec succès.

ASP.NET MVC inclut des fonctionnalités intégrées intéressantes qui facilitent la gestion des erreurs et actualiser de formulaire. Pour afficher ces fonctionnalités en action nous allons mettre à jour de notre méthode d’action de modification avec le code suivant :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample12.cs)]

Le code ci-dessus est similaire à notre implémentation précédente, à ceci près que nous sommes maintenant encapsulant un bloc de gestion des erreurs try/catch autour de notre travail. Si une exception se produit lors de l’appel UpdateModel(), soit lorsque nous essayer d’enregistrer le DinnerRepository (qui lève une exception si l’objet dîner que nous essayons de vous enregistrer n’est pas valide en raison d’une violation de règle au sein de notre modèle), notre bloc de gestion d’erreur catch sera exécuter. Dans celui-ci nous une boucle sur toutes les violations de règle qui existent dans l’objet Dinner et ajoutez-les à un objet de ModelState (que nous aborderons bientôt). Nous puis réaffichez la vue.

Pour voir cela utilisation exécutons à nouveau l’application, modifiez un dîner et modifiez-le pour donner un titre, une date d’événement de « BOGUS » et utiliser un numéro de téléphone du Royaume-Uni avec une valeur de pays USA. Lorsque vous appuyez sur le bouton « Enregistrer » notre méthode HTTP POST modifier ne sera pas en mesure d’enregistrer le dîner (car il existe des erreurs) et s’affichera de nouveau le formulaire :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image9.png)

Notre application a une expérience de l’erreur correcte. Les éléments de texte avec l’entrée non valide sont mises en surbrillance en rouge, et les messages d’erreur de validation sont affichés à l’utilisateur final à leur sujet. Le formulaire conserve également les données d’entrée, l’utilisateur a entré à l’origine – afin qu’ils n’êtes pas obligé d’acheter quoi que ce soit.

Comment procéder, vous pouvez vous demander, cela se produire ? Comment les zones de texte de titre, EventDate et ContactPhone fait eux-mêmes mettre en surbrillance en rouge et savoir pour générer les valeurs de l’utilisateur entré à l’origine ? Et comment des messages d’erreur s’affichent dans la liste en haut ? La bonne nouvelle est que cela n’a pas se produire par magie, au lieu de cela, il a été, car nous avons utilisé certaines des fonctionnalités intégrées ASP.NET MVC qui facilitent l’erreur et la validation des entrées des scénarios de gestion des.

#### <a name="understanding-modelstate-and-the-validation-html-helper-methods"></a>ModelState de présentation et les méthodes d’assistance HTML de Validation

Classes de contrôleur ont une collection de propriétés « ModelState » qui fournit un moyen d’indiquer que des erreurs existent avec un objet de modèle passé à une vue. Entrées d’erreur dans la collection ModelState identifient le nom de la propriété de modèle avec le problème (par exemple : « Title », « EventDate » ou « ContactPhone ») et autoriser un message d’erreur de conviviale être spécifié (par exemple : « Le titre est obligatoire »).

Le *UpdateModel()* méthode d’assistance remplit automatiquement la collection de ModelState quand il rencontre des erreurs lors de la tentative affecter des valeurs de formulaire à des propriétés sur l’objet de modèle. Par exemple, propriété EventDate de l’objet de notre dîner est de type DateTime. Lorsque la méthode UpdateModel() n’a pas pu lui affecter la valeur de chaîne « BOGUS » dans le scénario ci-dessus, la méthode UpdateModel() ajouté une entrée à la collection ModelState indiquant une erreur d’assignation s’est produite avec cette propriété.

Les développeurs peuvent également écrire du code pour ajouter explicitement les entrées d’erreur dans la collection ModelState que nous faisons ci-dessous au sein de notre « catch » erreur gestion bloc, remplissage de la collection ModelState avec entrées basées sur les Violations de règle active dans le Objet de dîner :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample13.cs)]

#### <a name="html-helper-integration-with-modelstate"></a>Intégration d’application d’assistance HTML à ModelState

Méthodes d’assistance HTML - comme Html.TextBox() - vérifient la collection de ModelState lors du rendu de sortie. Si une erreur pour l’élément existe, leur rendu la valeur entrée par l’utilisateur et une classe d’erreur CSS.

Par exemple, dans notre vue « Modifier », nous utilisons la méthode d’assistance Html.TextBox() pour restituer le EventDate de notre objet dîner :

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample14.aspx)]

Lorsque la vue est affichée dans le scénario d’erreur, la méthode Html.TextBox() vérifié la collection ModelState pour voir si des erreurs associées à la propriété « EventDate » de notre objet dîner. Lorsqu’il déterminé qu’une erreur s’est produite il rendu l’entrée d’utilisateur envoyé (« BOGUS ») comme valeur et ajouté une classe d’erreur css pour le &lt;type d’entrée = « textbox » /&gt; balisage qu’il a généré :

[!code-html[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample15.html)]

Vous pouvez personnaliser l’apparence de la classe d’erreur css à rechercher comme vous le souhaitez. La classe d’erreur par défaut CSS – « entrée-erreur de validation » – est définie dans le *\content\site.css* feuille de style et se présente comme ci-dessous :

[!code-css[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample16.css)]

Cette règle CSS est la cause de notre éléments d’entrée non valides être mis en surbrillance, comme ci-dessous :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image10.png)

##### <a name="htmlvalidationmessage-helper-method"></a>Méthode d’assistance de Html.ValidationMessage()

La méthode d’assistance Html.ValidationMessage() peut être utilisée pour générer le message d’erreur ModelState associé à une propriété de modèle particulier :

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample17.aspx)]

Génère le code ci-dessus :  *&lt;span classe = « erreur de validation de champ »&gt; la valeur « BOGUS » n’est pas valide&lt;/span&gt;*

La méthode d’assistance Html.ValidationMessage() prend également en charge un deuxième paramètre qui permet aux développeurs de substituer le message texte d’erreur qui s’affiche :

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample18.aspx)]

Génère le code ci-dessus :  <em>&lt;span classe = « erreur de validation de champ »&gt;\*&lt;/span&gt;</em>plutôt que le texte d’erreur par défaut lorsqu’une erreur est présente pour le Propriété de EventDate.

##### <a name="htmlvalidationsummary-helper-method"></a>Méthode d’assistance de Html.ValidationSummary()

La méthode d’assistance Html.ValidationSummary() peut être utilisée pour restituer un message d’erreur récapitulatif, accompagné d’un &lt;ul&gt;&lt;li /&gt;&lt;/ul&gt; liste des erreurs détaillées de tous les messages dans le Collection de ModelState :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image11.png)

La méthode d’assistance Html.ValidationSummary() prend un paramètre de chaîne facultatif – qui définit un message d’erreur récapitulatif à afficher au-dessus de la liste d’erreurs détaillées :

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample19.aspx)]

Vous pouvez éventuellement utiliser CSS pour remplacer l’aspect de la liste d’erreurs.

#### <a name="using-a-addruleviolations-helper-method"></a>À l’aide d’une méthode d’assistance AddRuleViolations

Notre implémentation initiale de HTTP-POST modifier utilisé une instruction foreach dans le bloc catch pour une boucle sur les Violations des règles de l’objet dîner et ajoutez-les à la collection de ModelState du contrôleur :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample20.cs)]

Nous pouvons rendre ce code un peu de nettoyage en ajoutant un « ControllerHelpers » classe au projet NerdDinner et implémenter une méthode d’extension « AddRuleViolations » qu’il contient qui ajoute une méthode d’assistance à la classe d’ASP.NET MVC ModelStateDictionary. Cette méthode d’extension peut encapsuler la logique nécessaire pour remplir le ModelStateDictionary avec une liste d’erreurs de RuleViolation :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample21.cs)]

Nous pouvons ensuite mettre à jour notre méthode d’action HTTP-POST modifier pour utiliser cette méthode d’extension pour remplir la collection ModelState avec notre dîner des Violations de règle.

#### <a name="complete-edit-action-method-implementations"></a>Effectuer des implémentations de méthode d’Action modifier

Le code ci-dessous implémente l’ensemble de la logique de contrôleur nécessaire pour notre scénario de modification :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample22.cs)]

Agréable dans notre implémentation de la modification est que notre modèle de vue ni de notre classe de contrôleur a tout savoir sur la validation spécifique ou appliquées par notre modèle dîner des règles d’entreprise. Nous peut ajouter des règles supplémentaires à notre modèle à l’avenir et que vous n’avez pas à apporter des modifications de code à notre contrôleur ou une vue dans l’ordre pour qu’ils puissent être pris en charge. Cela nous offre la possibilité d’évoluer facilement nos exigences des applications à l’avenir avec un minimum de modifications du code.

### <a name="create-support"></a>Créer la prise en charge

Nous avons terminé d’implémenter le comportement de « Modifier » de notre classe DinnersController. Nous allons à présent aborder d’implémenter la prise en charge de « Créer » dessus – ce qui permettra aux utilisateurs d’ajouter de nouveaux dîners.

#### <a name="the-http-get-create-action-method"></a>HTTP-GET créer la méthode d’Action

Nous allons commencer par implémenter le comportement de « GET » HTTP de notre créer la méthode d’action. Cette méthode est appelée lorsqu’un utilisateur visite le */dîners/créer* URL. Notre implémentation ressemble à :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample23.cs)]

Le code ci-dessus crée un nouvel objet dîner et affecte sa propriété EventDate soit une semaine à l’avenir. Il restitue ensuite une vue qui est basée sur le nouvel objet dîner. Étant donné que nous n’avons pas passé explicitement un nom pour le *View()* méthode d’assistance, il utilisera le chemin d’accès de la valeur par défaut basé sur une convention pour résoudre le modèle de vue : /Views/Dinners/Create.aspx.

Nous allons maintenant créer ce modèle de vue. Pour cela, nous pouvons en effectuant un clic droit au sein de la méthode d’action de créer, puis en sélectionnant la commande de menu contextuel « Ajouter une vue ». Dans la boîte de dialogue « Ajouter une vue », nous allons indiquer que nous transmettez un objet dîner pour le modèle de vue et choisir d’une structure automatiquement un modèle « Créer » :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image12.png)

Lorsque nous cliquons sur le bouton « Ajouter », Visual Studio enregistrer une nouvelle vue basée sur une structure « Create.aspx » dans le répertoire « \Views\Dinners » et ouvrez-le dans l’IDE :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image13.png)

Nous allons apporter quelques modifications au fichier une structure « créer » par défaut qui a été généré pour nous et modifiez-le à se présenter comme suit :

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample24.aspx)]

Et maintenant quand nous exécutons notre accès aux applications et le *« / dîners/créer »* URL dans le navigateur, il restitue l’interface utilisateur, comme ci-dessous à partir de notre implémentation d’action Create :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image14.png)

#### <a name="implementing-the-http-post-create-action-method"></a>Implémentation de la requête HTTP-POST créer de méthode d’Action

Nous avons la version de HTTP-GET de notre méthode d’action de création implémentée. Lorsqu’un utilisateur clique sur le bouton « Enregistrer » il effectue une publication de formulaire à la */dîners/créer* URL, et envoie le code HTML &lt;d’entrée&gt; valeurs à l’aide du verbe HTTP POST de formulaire.

Nous allons maintenant implémenter le comportement de la requête HTTP POST de notre créer la méthode d’action. Nous allons commencer en ajoutant une méthode d’action « Créer » surchargée à notre DinnersController qui a un attribut « AcceptVerbs » sur ce qui indique qu’il gère des scénarios de requête HTTP POST :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample25.cs)]

Il existe plusieurs façons, nous pouvons accéder à des paramètres de formulaire publiées au sein de notre méthode « Créer » de HTTP-POST est activé.

Une approche consiste à créer un nouvel objet dîner, puis utiliser le *UpdateModel()* méthode d’assistance (par exemple, nous avons fait avec l’action de modification) pour le remplir avec les valeurs de formulaire publiées. Nous pouvons ensuite ajouter à notre DinnerRepository conserver dans la base de données et rediriger l’utilisateur vers notre action de détails pour afficher le dîner nouvellement créé en utilisant le code ci-dessous :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample26.cs)]

Nous pouvons également utiliser une approche où nous avons notre méthode d’action Create() prennent un objet dîner comme un paramètre de méthode. ASP.NET MVC est ensuite automatiquement instancier un nouvel objet dîner pour nous, remplir ses propriétés en utilisant les entrées de formulaire et passez-le à notre méthode d’action :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample27.cs)]

Notre méthode d’action ci-dessus vérifie que l’objet dîner a été correctement rempli avec les valeurs de publication de formulaire en vérifiant la propriété ModelState.IsValid. Ceci renverra false s’il existe d’entrée des problèmes de conversion (par exemple : une chaîne de « BOGUS » pour la propriété EventDate), et si les éventuels problèmes de notre méthode d’action réaffiche le formulaire.

Si les valeurs d’entrée sont valides, puis la méthode d’action tente ajoutez et enregistrez le dîner de nouveau à la DinnerRepository. Il encapsule ce travail au sein d’un bloc try/catch et réaffiche le formulaire s’il existe des violations de règle d’entreprise (ce qui provoquent la méthode dinnerRepository.Save() lever une exception).

Pour voir ce comportement gestion des erreurs en action, nous pouvons demander le */dîners/créer* URL et remplissez les détails sur un nouveau dîner. Entrée incorrecte ou valeurs entraîne le formulaire de création être réaffichée avec les erreurs mis en surbrillance, comme ci-dessous :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image15.png)

Notez la façon dont notre formulaire de création honore les exacte même entreprise règles de validation et en tant que notre formulaire de modification. Il s’agit, car nos règles de validation et d’entreprise ont été définis dans le modèle et n’ont pas été incorporés dans l’interface utilisateur ou d’un contrôleur de l’application. Cela signifie que nous pouvons ultérieurement modifier/évoluer notre validation ou des règles d’entreprise dans un seul placent et les appliquer tout au long de notre application. Nous ne devrez pas modifier le code, grâce à notre modifier ou créer des méthodes d’action pour honorer automatiquement toutes les nouvelles règles ou modifications existants.

Lorsque nous corriger les valeurs d’entrée et cliquez sur le bouton « Enregistrer », notre ajout à la DinnerRepository réussira et un nouveau dîner sera ajouté à la base de données. Nous vous êtes ensuite redirigés vers le */Dinners/détails / [id]* URL – où nous s’affiche plus d’informations sur le dîner nouvellement créé :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image16.png)

### <a name="delete-support"></a>Supprimer la prise en charge

Nous allons maintenant ajouter la prise en charge de « Delete » à notre DinnersController.

#### <a name="the-http-get-delete-action-method"></a>La méthode d’Action Delete HTTP-GET

Nous allons commencer par implémenter le comportement HTTP GET de notre méthode d’action delete. Cette méthode est appelée lorsqu’un utilisateur visite le */Dinners/Delete / [id]* URL. Voici l’implémentation :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample28.cs)]

La méthode d’action tente de récupérer le dîner à supprimer. Si le dîner existe, il restitue une vue basée sur l’objet dîner. Si l’objet n’existe pas (ou a déjà été supprimé) il retourne une vue qui restitue le « NotFound » afficher modèle créé précédemment pour notre méthode d’action « Détails ».

Nous pouvons créer le modèle de vue « Delete » en effectuant un clic droit au sein de la méthode d’action de suppression, puis en sélectionnant la commande de menu contextuel « Ajouter une vue ». Dans la boîte de dialogue « Ajouter une vue », nous allons indiquer que nous transmettez un objet dîner à notre modèle de vue en tant que son modèle et choisir de créer un modèle vide :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image17.png)

Lorsque nous cliquons sur le bouton « Ajouter », Visual Studio ajoute un nouveau fichier de modèle de vue de « Delete.aspx » pour nous dans notre annuaire de « \Views\Dinners ». Nous allons ajouter du code HTML et le code pour le modèle pour implémenter un écran de confirmation de suppression comme ci-dessous :

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample29.aspx)]

Le code ci-dessus affiche le titre des dîner à supprimer et des sorties un &lt;formulaire&gt; élément qui effectue une publication à l’URL de /Dinners/Delete / [id] si l’utilisateur final clique sur le bouton « Supprimer » qu’il contient.

Lorsque nous exécutons notre accès aux applications et le *» / dîners/Delete / [id] »* URL pour un dîner valid l’objet restitue l’interface utilisateur comme ci-dessous :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image18.png)

| **Rubrique de côté : Pourquoi faisons-nous un billet ?** |
| --- |
| Vous vous demandez peut-être-pourquoi nous sommes-nous via l’effort de création d’un &lt;formulaire&gt; au sein de notre écran de confirmation de suppression ? Pourquoi ne pas utiliser simplement un lien hypertexte standard pour lier à une méthode d’action qui effectue l’opération de suppression ? La raison est que nous souhaitons veillez à protéger contre les robots et découvrir nos URL et à l’origine par inadvertance des données à supprimer quand ils suivent les liens de moteurs de recherche. HTTP-GET en URL sont considérés comme « sécurisés » pour qu’ils puissent / l’analyse de l’accès, et elles sont supposées pour suivre ne pas celles de HTTP-POST. Une bonne règle est de s’assurer que vous placez toujours le destructeur ou des opérations de modification de données derrière les requêtes HTTP POST. |

#### <a name="implementing-the-http-post-delete-action-method"></a>Implémentation de la méthode d’Action Delete HTTP-POST

Nous disposons désormais la version de HTTP-GET de notre méthode d’action Delete implémenté qui affiche un écran de confirmation de suppression. Lorsqu’un utilisateur final clique sur le bouton « Supprimer », elle effectue une publication de formulaire à la */Dinners/dîner / [id]* URL.

Nous allons maintenant implémenter le comportement de « POST » HTTP de la méthode d’action delete en utilisant le code ci-dessous :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample30.cs)]

La version de HTTP-POST de notre méthode d’action Delete tente de récupérer l’objet dîner à supprimer. S’il ne le trouve (car il a déjà été supprimé) il restitue notre modèle de « NotFound ». S’il trouve le dîner, il le supprime le DinnerRepository. Il restitue ensuite un modèle de « Supprimé ».

Pour implémenter le modèle « Supprimé » nous avec le bouton droit dans la méthode d’action et choisissez le menu contextuel « Ajouter une vue ». Nous nommez notre vue « Supprimé » et qu’il est un modèle vide (et pas prendre un objet de modèle fortement typé). Nous allons ensuite ajouter du contenu HTML à celui-ci :

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample31.aspx)]

Et maintenant quand nous exécutons notre accès aux applications et le *» / dîners/Delete / [id] »* URL pour un dîner valid objet il affichera notre dîner confirmation de la suppression de l’écran comme ci-dessous :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image19.png)

Lorsque nous cliquons sur le bouton « Supprimer ». celle-ci effectue une requête HTTP POST vers le */Dinners/Delete / [id]* URL, ce qui supprime le dîner à partir de notre base de données et afficher notre modèle de vue « Supprimé » :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image20.png)

### <a name="model-binding-security"></a>Sécurité de liaison de modèle

Nous avons vu deux façons différentes d’utiliser les fonctionnalités de liaison de modèle intégrées de ASP.NET MVC. Le tout d’abord à l’aide de la méthode UpdateModel() pour mettre à jour des propriétés sur un objet de modèle existant et la seconde à l’aide prise en charge de ASP.NET MVC pour passer des objets de modèle dans en tant que paramètres de méthode d’action. Ces deux techniques sont très puissants et extrêmement utiles.

Cette puissance offre également de responsabilité. Il est important de toujours être paranoïaque sur la sécurité lors de l’acceptation d’entrée d’utilisateur, et cela s’applique également lors de la liaison d’objets à l’entrée du formulaire. Vous devez être prudent de toujours encoder en HTML toutes les valeurs entrées par l’utilisateur pour éviter les attaques par injection de code HTML et JavaScript et soyez prudent d’attaques par injection SQL (Remarque : nous utilisons LINQ to SQL pour notre application, qui encode automatiquement les paramètres pour empêcher ces types d’attaques). Vous devez jamais compter sur la validation côté client autonome et toujours employer la validation côté serveur pour vous prémunir contre les pirates qui tentent d’envoyer des valeurs factices.

Un élément de sécurité supplémentaires pour vous assurer que vous pensez lors de l’utilisation d’ASP.NET MVC, les fonctionnalités de liaison est l’étendue des objets que vous liez. Plus précisément, vous souhaitez vous assurer que vous comprenez les implications de sécurité des propriétés que vous autorisez à être lié et assurez-vous que vous autorisez uniquement les propriétés qui doivent être mis à jour par un utilisateur final à mettre à jour.

Par défaut, la méthode UpdateModel() va tenter de mettre à jour toutes les propriétés sur l’objet de modèle qui correspondent aux valeurs de paramètre de formulaire entrant. De même, les objets passés comme paramètres de méthode d’action également par défaut peuvent avoir toutes leurs propriétés définis par le biais des paramètres de formulaire.

#### <a name="locking-down-binding-on-a-per-usage-basis"></a>Verrouillage de liaison sur une base par utilisation

Vous pouvez verrouiller la stratégie de liaison de manière à l’utilisation en fournissant un explicite « include liste » des propriétés pouvant être mis à jour. Cela est possible en passant un paramètre de tableau de chaîne supplémentaire à la méthode UpdateModel() comme ci-dessous :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample32.cs)]

Objets passés comme paramètres de méthode d’action également prendre en charge un attribut [Bind] qui permet une « include liste » d’autorisée des propriétés de la définir comme ci-dessous :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample33.cs)]

#### <a name="locking-down-binding-on-a-type-basis"></a>Verrouillage de liaison par type

Vous pouvez également verrouiller les règles de liaison sur une base par type. Cela vous permet de spécifier les règles de liaison qu’une seule fois et puis les appliquer dans tous les scénarios (y compris les scénarios de paramètre de méthode UpdateModel et action) sur tous les contrôleurs et méthodes d’action.

Vous pouvez personnaliser les règles de liaison par type en ajoutant un attribut [Bind] sur un type, ou en l’enregistrant dans le fichier Global.asax de l’application (utile pour les scénarios où vous ne possédez pas le type). Vous pouvez ensuite utiliser Include de l’attribut liaison et Exclude des propriétés pour contrôler quelles propriétés peuvent être liées pour l’interface ou une classe particulière.

Nous allons utiliser cette technique pour la classe dîner dans notre application NerdDinner et ajoutez un attribut [Bind] qui limite la liste de propriétés pouvant être liées à ce qui suit :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample34.cs)]

Notez que nous n’autorisons pas la collection des RSVP puissent être manipulés via une liaison, ni nous autorisons les DinnerID ou HostedBy des propriétés via la liaison. Pour des raisons de sécurité nous allons manipuler à la place uniquement ces propriétés particulières à l’aide de code explicite au sein de nos méthodes d’action.

### <a name="crud-wrap-up"></a>Conclusion CRUD

ASP.NET MVC inclut un nombre de fonctionnalités intégrées qui permettent à l’implémentation de scénarios de validation de formulaire. Nous avons utilisé une série de ces fonctionnalités pour prendre en charge de l’interface utilisateur CRUD par-dessus notre DinnerRepository.

Nous utilisons une approche axée sur le modèle pour implémenter notre application. Cela signifie que toutes nos validation et la règle d’entreprise logique est définie au sein de la couche de notre modèle – et pas au sein de nos contrôleurs ou des vues. Nos modèles de vue ni de notre classe de contrôleur connaître les règles d’entreprise spécifique appliquées par notre classe de modèle dîner.

Cela conserve notre architecture d’application propre et rendent plus faciles à tester. Nous pouvons ajouter des règles d’entreprise supplémentaires à notre couche de modèle à l’avenir et *n’a pas d’apporter des modifications de code* à notre contrôleur ou une vue pour qu’ils être pris en charge. Cela va nous fournir une grande agilité à évoluer et à modifier notre application à l’avenir.

Notre DinnersController maintenant permet dîner annonces/détails, ainsi que créer, modifier et supprimer la prise en charge. Vous trouverez le code complet pour la classe ci-dessous :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample35.cs)]

### <a name="next-step"></a>Étape suivante

Nous avons maintenant implémenter au sein de notre classe DinnersController prise en charge de base CRUD (Create, Read, Update et Delete).

Voyons maintenant comment nous pouvons utiliser des classes ViewData et ViewModel pour activer l’interface utilisateur encore plus riche sur nos formulaires.

> [!div class="step-by-step"]
> [Précédent](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
> [Suivant](use-viewdata-and-implement-viewmodel-classes.md)
