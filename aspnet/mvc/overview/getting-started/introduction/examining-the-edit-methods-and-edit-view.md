---
uid: mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
title: Examen des méthodes de modification et de la vue Edit | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 05/22/2015
ms.assetid: 52a4d5fe-aa31-4471-b3cb-a064f82cb791
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: a166f6c4450c72adc23f7d36113ceba7e04f1929
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820887"
---
<a name="examining-the-edit-methods-and-edit-view"></a>Examen des méthodes de modification et de la vue de modification
====================
par [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

Dans cette section, vous allez examiner généré `Edit` méthodes d’action et des vues pour le contrôleur de film. Mais tout d’abord prendra un détournement court s’améliorer l’aspect de la date de publication. Ouvrez le *Models\Movie.cs* fichier, puis ajoutez les lignes en surbrillance ci-dessous :

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cs?highlight=2,12-14)]

Vous pouvez également rendre la culture de date spécifique comme suit :

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs?highlight=3)]

Nous aborderons [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) dans le prochain didacticiel. L’attribut [Display](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayattribute.aspx) spécifie les éléments à afficher pour le nom d’un champ (dans le cas présent, « Release Date » au lieu de « ReleaseDate »). Le [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attribut spécifie le type des données, dans ce cas il étant une date, les informations d’heure stockées dans le champ ne sont pas affichées. Le [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attribut est nécessaire pour un bogue dans le navigateur Chrome qui restitue des formats de date incorrecte.

Exécutez l’application et accédez à la `Movies` contrôleur. Maintenez le pointeur de la souris sur un **modifier** lien pour afficher l’URL auxquelles il est lié.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

Le **modifier** lien a été généré par le `Html.ActionLink` méthode dans le *Views\Movies\Index.cshtml* vue :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

Le `Html` objet est une application d’assistance qui est exposée à l’aide d’une propriété sur le [System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx) classe de base. Le `ActionLink` méthode de l’application d’assistance facilite l’utilisation générer dynamiquement le code HTML des liens hypertexte aux méthodes d’action sur les contrôleurs. Le premier argument de la `ActionLink` méthode est le texte de lien à afficher (par exemple, `<a>Edit Me</a>`). Le deuxième argument est le nom de la méthode d’action à appeler (dans ce cas, le `Edit` action). L’argument final est un [objet anonyme](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) qui génère les données d’itinéraire (dans ce cas, l’ID de 4).

Le lien généré indiqué dans l’image précédente est `http://localhost:1234/Movies/Edit/4`. L’itinéraire par défaut (établie dans *application\_start\routeconfig*) prend le modèle d’URL `{controller}/{action}/{id}`. Par conséquent, ASP.NET se traduit par `http://localhost:1234/Movies/Edit/4` en une requête à la `Edit` méthode d’action de la `Movies` contrôleur avec le paramètre `ID` égal à 4. Examinez le code suivant à partir de la *application\_start\routeconfig* fichier. Le [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) méthode est utilisée pour acheminer les requêtes HTTP à la méthode de contrôleur et action correcte et fournir le paramètre ID facultatif. Le [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) méthode est également utilisée par le [HtmlHelpers](https://msdn.microsoft.com/library/system.web.mvc.htmlhelper(v=vs.108).aspx) comme `ActionLink` pour générer des URL étant données le contrôleur, méthode d’action et les données d’itinéraire.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cs?highlight=7)]

Vous pouvez également transmettre des paramètres de méthode d’action à l’aide d’une chaîne de requête. Par exemple, l’URL `http://localhost:1234/Movies/Edit?ID=3` passe également le paramètre `ID` de 3 à la `Edit` méthode d’action de la `Movies` contrôleur.

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

Ouvrez la `Movies` contrôleur. Les deux `Edit` méthodes d’action sont présentées ci-dessous.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs?highlight=19-21)]

Notez que la deuxième méthode d’action `Edit` est précédée de l’attribut `HttpPost`. Cet attribut spécifie que la surcharge de la `Edit` méthode peut être appelée uniquement pour les requêtes POST. Vous pouvez appliquer la `HttpGet` modifier la méthode de l’attribut à la première, mais qui n’est pas nécessaire car il s’agit de la valeur par défaut. (Nous allons nous référer aux méthodes d’action attribués de manière implicite le `HttpGet` sous la forme `HttpGet` méthodes.) Le [lier](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) attribut est un autre mécanisme de sécurité importante qui maintient les pirates à partir de données à votre modèle de survalidation. Vous devez inclure uniquement les propriétés dans l’attribut de liaison que vous souhaitez modifier. Vous pouvez consulter la survalidation et l’attribut de liaison dans mon [sur-validation note de sécurité](https://go.microsoft.com/fwlink/?LinkId=317598). Dans le modèle simple utilisé dans ce didacticiel, nous sera liaison toutes les données dans le modèle. Le [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) attribut est utilisé pour empêcher la falsification d’une demande et est associé à `@Html.AntiForgeryToken()` dans le fichier de la vue edit (*Views\Movies\Edit.cshtml*), une partie est affichée ci-dessous :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cshtml?highlight=9)]

`@Html.AntiForgeryToken()` génère un jeton anti-contrefaçon de formulaire masqué qui doit correspondre dans le `Edit` méthode de la `Movies` contrôleur. Vous trouverez plus d’informations sur Cross-site demande falsification (également appelé XSRF ou CSRF) dans mon didacticiel [prévention de XSRF/CSRF dans MVC](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md).

Le `HttpGet` `Edit` méthode prend le paramètre ID de film, recherche le film à l’aide d’Entity Framework `Find` (méthode) et retourne le film sélectionné à la vue Edit. Si un film est introuvable, [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx) est retourné. Quand le système de génération de modèles automatique a créé la vue Edit, il a examiné la classe `Movie` et a créé le code pour restituer les éléments `<label>` et `<input>` de chaque propriété de la classe. L’exemple suivant montre la vue Edit qui a été générée par le système de génération de modèles automatique de visual studio :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

Notez comment le modèle de vue a un `@model MvcMovie.Models.Movie` instruction en haut du fichier : Spécifie que la vue prévoit que le modèle pour le modèle de vue soit de type `Movie`.

Le code généré automatiquement utilise plusieurs *méthodes d’assistance* pour rationaliser le balisage HTML. Le [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) helper affiche le nom du champ (&quot;titre&quot;, &quot;ReleaseDate&quot;, &quot;Genre&quot;, ou &quot;prix &quot;). Le [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) helper restitue un élément HTML `<input>` élément. Le [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) helper affiche les messages de validation associés à cette propriété.

Exécutez l’application et accédez à la */Movies* URL. Cliquez sur un lien **Edit**. Dans le navigateur, affichez la source de la page. Vous trouverez ci-dessous le code HTML pour l’élément de formulaire.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cshtml?highlight=1-2)]

Le `<input>` sont des éléments dans le code HTML `<form>` élément dont `action` attribut a la valeur à valider dans le */Movies/Edit* URL. Les données du formulaire seront publiées sur le serveur lors de la **enregistrer** bouton. La deuxième ligne affiche le texte masqué [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) jeton généré par le `@Html.AntiForgeryToken()` appeler.

## <a name="processing-the-post-request"></a>Traitement de la requête POST

Le code suivant montre la version `HttpPost` de la méthode d’action `Edit`.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

Le [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) attribut valide le [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) jeton généré par le `@Html.AntiForgeryToken()` appeler dans la vue.

Le [binder de modèle ASP.NET MVC](https://msdn.microsoft.com/library/dd410405.aspx) prend les valeurs de formulaire publiées et crée un `Movie` objet qui est passé en tant que le `movie` paramètre. La méthode `ModelState.IsValid` vérifie que les données envoyées dans le formulaire peuvent être utilisées pour changer (modifier ou mettre à jour) un objet `Movie`. Si les données sont valides, les données de film sont enregistrées dans le `Movies` collection de la `db(MovieDBContext` instance). Les nouvelles données de film sont enregistrées dans la base de données en appelant le `SaveChanges` méthode de `MovieDBContext`. Après avoir enregistré les données, le code redirige l’utilisateur vers la méthode d’action `Index` de la classe `MoviesController`, qui affiche la collection de films, avec notamment les modifications qui viennent d’être apportées.

Dès que la validation côté client détermine que les valeurs d’un champ ne sont pas valides, un message d’erreur s’affiche. Si vous désactivez JavaScript, vous n’aurez pas la validation côté client, mais le serveur détecte les valeurs publiées ne sont pas valides, et les valeurs de formulaire sont réaffichées avec des messages d’erreur. Plus loin dans ce didacticiel, nous examinons plus en détail la validation.

Le `Html.ValidationMessageFor` helpers dans les *Edit.cshtml* vue modèle de prendre en charge d’affichage des messages d’erreur appropriés.

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

Tous les `HttpGet` méthodes suivent un modèle similaire. Ils obtiennent un objet de film (ou une liste d’objets, dans le cas de `Index`) et passer le modèle à la vue. Le `Create` méthode passe un objet de film vide à la vue de créer. Toutes les méthodes qui créent, modifient, suppriment ou changent d’une quelconque manière des données le font dans la surcharge `HttpPost` de la méthode. Modification des données dans une méthode HTTP GET est un risque de sécurité, comme décrit dans le billet de blog [ASP.NET MVC Conseil #46 : n’utilisez pas supprimer les liens, car ils créent des failles de sécurité](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Modification des données dans une méthode GET enfreint également les bonnes pratiques HTTP et l’architecture [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer) de modèle, qui spécifie que les requêtes GET ne doivent pas changer l’état de votre application. En d’autres termes, une opération GET doit être sûre, ne doit avoir aucun effet secondaire et ne doit pas modifier vos données persistantes.

Si vous utilisez un ordinateur de l’anglais des États-Unis, vous pouvez ignorer cette section et passer au didacticiel suivant. Vous pouvez télécharger la version Globalize de ce didacticiel [ici](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=16475). Pour obtenir un didacticiel de deux parties excellente sur l’internationalisation, consultez [ASP.NET MVC 5 internationalisation de Nadeem](http://afana.me/post/aspnet-mvc-internationalization.aspx).


> [!NOTE]
> Pour prendre en charge la validation jQuery pour les paramètres régionaux non anglais qui utilisent une virgule (&quot;,&quot;) pour une virgule décimale et les formats de date non anglais des États-Unis, vous devez inclure *globalize.js* et votre propre  *cultures/globalize.cultures.js* fichier (à partir de [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) et JavaScript pour utiliser `Globalize.parseFloat`. Vous pouvez obtenir la validation non anglaises de jQuery à partir de NuGet. (N’installez pas Globalize si vous utilisez des paramètres régionaux anglais.)


1. À partir de la **outils** menu, cliquez sur **Gestionnaire de Package NuGetLibrary**, puis cliquez sur **gérer les Packages NuGet pour la Solution**.  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image5.png)
2. Dans le volet gauche, sélectionnez <strong>Parcourir *.</strong>* (Voir l’image ci-dessous.)
3. Dans la zone d’entrée, tapez *Globalize* \*.  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image6.png) Choisissez `jQuery.Validation.Globalize`, choisissez `MvcMovie` et cliquez sur **installer**. Le *Scripts\jquery.globalize\globalize.js* fichier sera ajouté à votre projet. Le *Scripts\jquery.globalize\cultures\* dossier contient plusieurs fichiers JavaScript de culture. Il peut prendre cinq minutes pour installer ce package.

   Le code suivant montre les modifications au fichier Views\Movies\Edit.cshtml : 

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

Pour éviter de répéter ce code dans chaque vue de modification, vous pouvez le déplacer vers le fichier de disposition. Pour optimiser le téléchargement du script, consultez mon didacticiel [regroupement et minimisation](../../performance/bundling-and-minification.md).

Pour plus d’informations, consultez [ASP.NET MVC 3 internationalisation](http://afana.me/post/aspnet-mvc-internationalization.aspx) et [ASP.NET MVC 3 internationalisation - partie 2 (NerdDinner)](http://afana.me/post/aspnet-mvc-internationalization-part-2.aspx).

Comme solution temporaire, si vous ne recevez validation fonctionne dans vos paramètres régionaux, vous pouvez forcer votre ordinateur pour utiliser anglais (États-Unis) ou vous pouvez désactiver JavaScript dans votre navigateur. Pour forcer votre ordinateur pour utiliser anglais (États-Unis), vous pouvez ajouter l’élément de globalisation à la racine de projets *web.config* fichier. Le code suivant montre l’élément de globalisation avec la culture définie pour l’anglais des États-Unis.

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample11.xml)]

<a id="gettingstarted"></a><a id="jQueryAjaxJSON"></a> Dans le didacticiel suivant, nous allons implémenter la fonctionnalité de recherche.

> [!div class="step-by-step"]
> [Précédent](accessing-your-models-data-from-a-controller.md)
> [Suivant](adding-search.md)
