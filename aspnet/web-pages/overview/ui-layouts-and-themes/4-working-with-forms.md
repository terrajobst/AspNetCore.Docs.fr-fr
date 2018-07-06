---
uid: web-pages/overview/ui-layouts-and-themes/4-working-with-forms
title: Utilisation des formulaires HTML dans des Sites ASP.NET Web Pages (Razor) | Microsoft Docs
author: tfitzmac
description: Un formulaire est une section d’un document HTML dans laquelle vous placez des contrôles d’entrée de l’utilisateur, telles que les zones de texte, les cases à cocher, les cases d’option et les listes déroulantes. Vous utilisez des formulaires lorsqu...
ms.author: aspnetcontent
ms.date: 02/10/2014
ms.assetid: f3f4b8c8-e8f6-4474-ad94-69228a6c01ee
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/4-working-with-forms
msc.type: authoredcontent
ms.openlocfilehash: 4658e80d63c9c3665f197071f3f3d1420362e4a3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820283"
---
<a name="working-with-html-forms-in-aspnet-web-pages-razor-sites"></a>Utilisation des formulaires HTML dans les Sites ASP.NET Web Pages (Razor)
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article décrit comment traiter un formulaire HTML (avec des boutons et des zones de texte) lorsque vous travaillez dans un site Web ASP.NET Web Pages (Razor).
> 
> **Ce que vous allez apprendre :** 
> 
> - Comment créer un formulaire HTML.
> - Guide pratique pour lire l’entrée d’utilisateur à partir du formulaire.
> - Guide pratique pour valider l’entrée utilisateur.
> - Comment restaurer les valeurs de formulaire après l’envoi de la page.
> 
> Il s’agit des concepts présentés dans l’article de programmation ASP.NET :
> 
> - Objet `Request`.
> - Validation des entrées.
> - Codage HTML.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Ce didacticiel fonctionne également avec ASP.NET Web Pages 2.


## <a name="creating-a-simple-html-form"></a>Création d’un formulaire HTML Simple

1. Créer un nouveau site Web.
2. Dans le dossier racine, créez une page web nommée *Form.cshtml* et entrez le balisage suivant :

    [!code-html[Main](4-working-with-forms/samples/sample1.html)]
3. Lancer la page dans votre navigateur. (Dans WebMatrix, dans le **fichiers** espace de travail, cliquez sur le fichier, puis sélectionnez **lancer dans le navigateur**.) Un formulaire simple avec trois champs d’entrée et un **envoyer** bouton s’affiche.

    ![Capture d’écran d’un formulaire avec trois zones de texte.](4-working-with-forms/_static/image1.jpg)

    À ce stade, si vous cliquez sur le **envoyer** bouton, rien ne se produit. Pour que le formulaire soit utile, vous devez ajouter du code qui s’exécutera sur le serveur.

## <a name="reading-user-input-from-the-form"></a>Lecture de l’entrée utilisateur à partir du formulaire

Pour traiter le formulaire, vous ajoutez le code qui lit les valeurs de champ soumis et fait quelque chose avec eux. Cette procédure vous montre comment lire les champs et afficher l’entrée d’utilisateur sur la page. (Dans une application de production, vous le faites généralement plus intéressant avec les entrées utilisateur. Vous le ferez dans l’article sur l’utilisation des bases de données.)

1. En haut de la *Form.cshtml* de fichier, entrez le code suivant :

    [!code-cshtml[Main](4-working-with-forms/samples/sample2.cshtml)]

    Lorsque l’utilisateur demande tout d’abord la page, seul le formulaire vide s’affiche. L’utilisateur (qui sera vous) remplit le formulaire, puis sur **envoyer**. Cela envoie (publie) l’entrée utilisateur pour le serveur. Par défaut, la demande est transmise à la même page (à savoir, *Form.cshtml*).

    Lorsque vous envoyez la page de ce temps, les valeurs que vous avez entré sont affichent juste au-dessus de la forme :

    ![Capture d’écran montrant les valeurs que vous avez entré affichées sur la page.](4-working-with-forms/_static/image2.jpg)

    Examinez le code de la page. Vous utilisez d’abord la `IsPost` méthode pour déterminer si la page est en cours de publication &#8212; , autrement dit, si un utilisateur a cliqué sur le **envoyer** bouton. S’il s’agit d’un billet, `IsPost` retourne la valeur true. Il s’agit de la méthode standard dans les Pages Web ASP.NET pour déterminer si vous travaillez avec une demande initiale (une requête GET) ou une publication (postback) (une requête POST). (Pour plus d’informations sur GET et POST, consultez l’encadré « HTTP GET et POST et le IsPost Property » dans [Introduction à ASP.NET Web Pages programmation à l’aide de la syntaxe Razor](https://go.microsoft.com/fwlink/?LinkId=202890#SB_HttpGetPost).)

    Ensuite, vous obtenez les valeurs que l’utilisateur a renseigné à partir de la `Request.Form` objet et vous les placez dans des variables pour une utilisation ultérieure. Le `Request.Form` objet contient toutes les valeurs qui ont été envoyés avec la page, chacune étant identifiée par une clé. La clé est l’équivalent de la `name` attribut du champ de formulaire que vous souhaitez lire. Par exemple, pour lire le `companyname` champ (zone de texte), vous utilisez `Request.Form["companyname"]`.

    Les valeurs de formulaire sont stockées dans le `Request.Form` objet sous forme de chaînes. Par conséquent, lorsque vous devez travailler avec une valeur sous forme d’un nombre ou une date ou un autre type, vous devez convertir une chaîne à ce type. Dans l’exemple, le `AsInt` méthode de la `Request.Form` est utilisé pour convertir la valeur du champ employés (qui contient un nombre d’employés) en un entier.
2. Lancer la page dans votre navigateur, renseignez les champs de formulaire, puis cliquez sur **envoyer**. La page affiche les valeurs que vous avez entré.

> [!TIP] 
> 
> <a id="SB_HTMLEncoding"></a>
> ### <a name="html-encoding-for-appearance-and-security"></a>Encodage pour l’apparence et la sécurité HTML
> 
> HTML a des utilisations spéciales pour les caractères tels que `<`, `>`, et `&`. Si ces caractères spéciaux s’affiche, où elles ne sont pas attendues, ils peuvent endommager l’apparence et les fonctionnalités de votre page web. Par exemple, le navigateur interprète le `<` de caractères (sauf s’il est suivi par un espace) comme le début d’un élément HTML, comme `<b>` ou `<input ...>`. Si le navigateur ne reconnaît pas l’élément, il ignore simplement la chaîne qui commence par `<` jusqu'à atteindre quelque chose qui il reconnaît à nouveau. Évidemment, cela peut entraîner certaines bizarre rendu dans la page.
> 
> L’encodage HTML remplace les caractères réservés suivants avec un code de navigateurs interprètent en tant que le symbole correct. Par exemple, le `<` est remplacé par `&lt;` et `>` est remplacé par `&gt;`. Le navigateur restitue ces chaînes de remplacement en tant que les caractères que vous souhaitez voir.
> 
> Il est judicieux d’utiliser n’importe quel moment vous affichez les chaînes de codage HTML (entrée) que vous avez obtenu à partir d’un utilisateur. Si vous n’est pas le cas, un utilisateur peut essayer d’obtenir votre page web pour exécuter un script malveillant ou de faire autre chose qui peut compromettre la sécurité de votre site ou qui n’est pas seulement à vos attentes. (Cela est particulièrement important si d’entrée d’utilisateur, de les stocker un endroit et de les afficher ultérieurement &#8212; comme par exemple, un commentaire de blog, de révision de l’utilisateur ou quelque chose de similaire à celle.)
> 
> Pour aider à éviter ces problèmes, ASP.NET Web Pages automatiquement au format HTML n’importe quel texte de contenu que vous avez la sortie à partir de votre code. Par exemple, lorsque vous affichez le contenu d’une variable ou une expression à l’aide de code tel que `@MyVar`, ASP.NET Web Pages encode automatiquement la sortie.


## <a name="validating-user-input"></a>Validation des entrées utilisateur

Les utilisateurs font des erreurs. Demandez-lui de remplir un champ et ils oublient, ou vous lui demandez d’entrer le nombre d’employés et ils taper un nom à la place. Pour vous assurer qu’un formulaire a été renseigné correctement avant le traitement terminé, vous validez l’entrée utilisateur.

Cette procédure montre comment valider tous les champs de formulaire trois s’assurer que l’utilisateur n’a pas les laisser vide. Vous vérifiez également que la valeur du nombre employé est un nombre. S’il existe des erreurs, vous afficherez une erreur message qui indique à l’utilisateur quelles valeurs n’a pas être validé.

1. Dans le *Form.cshtml* de fichier, remplacez le premier bloc de code par le code suivant : 

    [!code-cshtml[Main](4-working-with-forms/samples/sample3.cshtml)]

    Pour valider l’entrée d’utilisateur, vous utilisez le `Validation` helper. Vous enregistrez des champs obligatoires en appelant `Validation.RequireField`. Inscrire d’autres types de validation en appelant `Validation.Add` et en spécifiant le champ à valider et le type de validation à effectuer.

    Lorsque la page s’exécute, ASP.NET effectue toutes les validations pour vous. Vous pouvez vérifier les résultats en appelant `Validation.IsValid`, qui retourne true si tout a réussi et false si n’importe quel champ Échec de la validation. En règle générale, vous appelez `Validation.IsValid` avant d’effectuer tout traitement de l’entrée utilisateur.
2. Mise à jour le `<body>` élément en ajoutant trois appels vers le `Html.ValidationMessage` méthode, comme suit :

    [!code-cshtml[Main](4-working-with-forms/samples/sample4.cshtml?highlight=8,13,18)]

    Pour afficher les messages d’erreur de validation, vous pouvez appeler Html.`ValidationMessage` et lui transmettre le nom du champ que vous souhaitez que le message pour.
3. Exécutez la page. Laissez les champs vides et cliquez sur **envoyer**. Vous voyez des messages d’erreur.

    ![Capture d’écran qui affiche les messages d’erreur affichées si l’entrée d’utilisateur n’est pas validé.](4-working-with-forms/_static/image3.jpg)
4. Ajouter une chaîne (par exemple, « ABC ») à la **Employee Count** champ et cliquez sur **envoyer** à nouveau. Cette fois que vous voyez une erreur qui indique que la chaîne n’est pas dans le format correct, à savoir, un entier.

    ![Capture d’écran qui affiche des messages d’erreur affichées si les utilisateurs entrent une chaîne pour le champ d’employés.](4-working-with-forms/_static/image4.jpg)

Les Pages Web ASP.NET fournit davantage d’options pour la validation des entrées d’utilisateur, y compris la possibilité d’effectuer automatiquement la validation à l’aide du script client, afin que les utilisateurs obtiennent des commentaires immédiats dans le navigateur. Consultez le [des ressources supplémentaires](#Additional_Resources) section ultérieurement pour plus d’informations.

## <a name="restoring-form-values-after-postbacks"></a>Restauration des valeurs de formulaire après les publications (postback)

Lorsque vous avez testé la page dans la section précédente, vous avez peut-être remarqué que si vous aviez une erreur de validation, tous les éléments que vous avez entré (pas seulement les données non valides) a disparu, et vous deviez entrer à nouveau les valeurs pour tous les champs. Cet exemple illustre un point important : lorsque vous soumettez une page, le traiter et ensuite restituer à nouveau la page, la page est recréée à partir de zéro. Comme vous l’avez vu, cela signifie que toutes les valeurs qui se trouvaient dans la page lorsqu’il a été envoyé sont perdues.

Vous pouvez résoudre ce problème facilement, toutefois. Vous avez accès aux valeurs qui ont été soumises (dans le `Request.Form` de l’objet, donc vous pouvez remplir ces valeurs dans les champs de formulaire lorsque la page est affichée.

1. Dans le *Form.cshtml* fichier, remplacez le `value` attributs de la `<input>` éléments à l’aide de la `value` attribut. : 

    [!code-cshtml[Main](4-working-with-forms/samples/sample5.cshtml?highlight=13,19,25)]

    Le `value` attribut de la `<input>` éléments a été définie pour lire dynamiquement la valeur du champ de la `Request.Form` objet. La première fois que la page est demandée, les valeurs dans le `Request.Form` objet sont tous vides. Cela convient, étant donné que de cette façon le formulaire est vide.
2. Lancer la page dans votre navigateur, renseignez les champs de formulaire ou laisser vide, puis cliquez sur **envoyer**. Une page qui affiche les valeurs soumises s’affiche.

    ![formulaires-5](4-working-with-forms/_static/image5.jpg)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

- [1 001 manières pour obtenir une entrée à partir des utilisateurs Web](https://msdn.microsoft.com/library/ms971057.aspx)
- [À l’aide de formulaires et de traitement des entrées utilisateur](https://msdn.microsoft.com/library/ms525182(VS.90).aspx)
- [Validation des entrées utilisateur dans les sites ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=253002)
- [À l’aide de la saisie semi-automatique dans les formulaires HTML](https://msdn.microsoft.com/library/ms533032(VS.85).aspx)
