---
uid: web-pages/overview/releases/top-features-in-web-pages-2
title: La partie supérieure des fonctionnalités dans ASP.NET Web Pages 2 | Documents Microsoft
author: microsoft
description: Cette rubrique fournit une vue d’ensemble des nouvelles fonctionnalités dans ASP.NET Web Pages 2, une infrastructure de programmation web léger qui est incluse avec le WebMatr supérieure...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2012
ms.topic: article
ms.assetid: cc712e72-c3d0-4e43-bc2d-28cc09cd8f71
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/top-features-in-web-pages-2
msc.type: authoredcontent
ms.openlocfilehash: f0d32edd3ab54c55aa06c803cd91e01cbbb8f08a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30899380"
---
<a name="the-top-features-in-aspnet-web-pages-2"></a>Les fonctionnalités principales dans les Pages Web ASP.NET 2
====================
par [Microsoft](https://github.com/microsoft)

> Cet article fournit une vue d’ensemble des nouvelles fonctionnalités dans ASP.NET Web Pages 2 RC, une infrastructure de programmation web léger qui est incluse avec top [Microsoft WebMatrix 2 RC](https://www.microsoft.com/web/).
> 
> **Ce qui est inclus :** 
> 
> - [Installing WebMatrix](#install)
> - [Fonctionnalités nouvelles et améliorées](#New_and_Enhanced_Features)
> 
>     - [Modifications pour la version RC](#Changes_for_the_RC_Version)
>     - [Modifications apportées à la version bêta](#Changes_for_the_Beta_Version)
>     - [L’aide des modèles de Site nouvelles et mises à jour](#templates)
>     - [Validation des entrées utilisateur](#validation)
>     - [L’activation des connexions à partir de Facebook et d’autres sites à l’aide d’OAuth et OpenID](#oauthsetup)
>     - [Ajout de mappages à l’aide de l’application d’assistance de mappages](#maphelper)
>     - [Exécution des Pages Web Applications côte à côte](#sidebyside)
>     - [Rendu des Pages pour les appareils mobiles](#mobile)
> - [Ressources supplémentaires pour MSBuild](#resources)
> 
> > [!NOTE]
> > Cette rubrique suppose que vous utilisez WebMatrix pour travailler avec votre code ASP.NET Web Pages 2. Toutefois, comme avec 1 des Pages Web, vous pouvez également créer des sites Web de Web Pages 2 à l’aide de Visual Studio, qui vous permet d’amélioré les fonctionnalités IntelliSense et le débogage. Pour travailler avec des Pages Web dans Visual Studio, vous devez d’abord installer Visual Studio 2010 SP1, Visual Web Developer Express 2010 SP1 ou Visual Studio 11 Beta. Ensuite, installez la bêta ASP.NET MVC 4, qui inclut des modèles et des outils pour créer des applications ASP.NET MVC 4 et 2 de Pages Web dans Visual Studio.
> 
> 
> *Dernière mise à jour : 18 juin 2012*


<a id="install"></a>
## <a name="installing-webmatrix"></a>Installation de WebMatrix

Pour installer les Pages Web, vous pouvez utiliser Microsoft Web Platform Installer, ce qui est une application gratuite qui facilite la tâche installer et configurer les technologies web. Vous allez installer la version bêta 2 WebMatrix, qui inclut la version bêta 2 de Pages Web.

1. Accédez à la page d’installation de la dernière version de Web Platform Installer :

    [https://go.microsoft.com/fwlink/?LinkId=226883](https://go.microsoft.com/fwlink/?LinkId=226883)

    > [!NOTE]
    > Si vous avez déjà WebMatrix 1 est installé, cette installation met à jour à la bêta 2 de WebMatrix. Vous pouvez exécuter des sites Web qui ont été créés à l’aide de la version 1 ou 2 sur le même ordinateur. Pour plus d’informations, consultez la section sur [les Applications Web Pages en cours d’exécution côte à côte](#sidebyside).
2. Choisissez **installer maintenant**. 

    Si vous utilisez Internet Explorer, accédez à l’étape suivante. Si vous utilisez un autre navigateur comme Mozilla Firefox ou Google Chrome, vous êtes invité à enregistrer le *Webmatrix.exe* le fichier sur votre ordinateur. Enregistrez le fichier, puis cliquez dessus pour lancer le programme d’installation.
3. Exécutez le programme d’installation et choisissez le **installer** bouton. Cette commande installe WebMatrix et Web Pages.

## <a id="New_and_Enhanced_Features"></a>  Fonctionnalités nouvelles et améliorées

### <a id="Changes_for_the_RC_Version"></a>  Modifications de la Version RC (juin 2012)

La version RC en juin 2012 a peu de modifications à partir de la mise à jour version bêta qui a été publiée en mars 2012. Ces modifications sont :

- A `Validation.AddFormError` méthode a été ajoutée à la `Validation` helper. Cela est utile si vous effectuez une validation manuellement (par exemple, pour valider une valeur qui est passée dans la chaîne de requête) et que vous souhaitez ajouter un message d’erreur qui peut être affiché par le `Html.ValidationSummary` (méthode). Pour plus d’informations, consultez la section [validation des données que ne proviennent directement à partir d’utilisateurs](https://go.microsoft.com/fwlink/?LinkId=253002#Validating_Data_That_Doesnt_Come_Directly_from_Users) dans [validation des entrées d’utilisateur dans les Sites ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253002).
- La fonctionnalité de groupement et la minimisation a été supprimée à partir des assemblys principaux ASP.NET Web Pages 2. Par conséquent, le `Assets` répertoriées plus loin dans ce document n’est pas disponible. Au lieu de cela, vous devez installer le [optimisation ASP.NET](http://nuget.org/packages/Microsoft.Web.Optimization/0.1) package NuGet. Pour plus d’informations, consultez [regroupement et réduire les ressources dans un Site ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=255373).
- Les assemblys supplémentaires pour prendre en charge ASP.NET Web Pages 2 ont été ajoutés. L’effet uniquement visible de cette modification est que vous pouvez voir plusieurs assemblys dans d’un site *bin* dossier une fois que vous créez un site ou déployez un site vers un fournisseur d’hébergement.

<a id="Changes_for_the_Beta_Version"></a>
### <a name="changes-for-the-beta-version-february-2012"></a>Modifications pour la Version bêta (février 2012)

La version bêta publiée en février 2012 a peu de modifications de la version bêta qui a été publiée en décembre 2011. Ces modifications sont :

- Razor prend désormais en charge les attributs conditionnels. Dans le code HTML élément, si vous définissez un attribut à une valeur qui est résolu dans le code serveur pour `false` ou `null`, ASP.NET ne rend pas l’attribut du tout. Par exemple, imaginez que vous avez le balisage suivant pour une case à cocher :

    [!code-html[Main](top-features-in-web-pages-2/samples/sample1.html)]

    Si la valeur de `checked1` se résout en `false` ou `null`, le `checked` attribut n’est pas rendu. Il s’agit d’une modification avec rupture.
- Le `Validation.GetHtml` méthode a été renommée en `Validation.For`. Il s’agit d’une modification avec rupture ; `Validation.GetHtml` ne fonctionnent pas dans la version bêta.
- Vous pouvez maintenant inclure le `~` opérateur dans le balisage pour faire référence à la racine du site sans utiliser le `Href` (fonction). (Autrement dit, l’analyseur Razor peut maintenant rechercher et résoudre les `~` opérateur sans nécessiter un appel de méthode explicite à `Href`.) Le `Href` méthode fonctionne toujours, donc il ne s’agit pas d’une modification avec rupture.

    Par exemple, si vous aviez précédemment balisage comme suit :

    `<a href="@Href("~/Default.cshtml")">Home</a>`

    Vous pouvez maintenant utiliser le balisage comme suit :

    `<a href="~/Default.cshtml">Home</a>`
- Le `Scripts` auxiliaire pour la gestion des ressources (ressource) a été remplacée par la `Assets` assistance, ce qui a des méthodes légèrement différentes, telles que les éléments suivants :

  - Pour `Scripts.Add`, utiliser `Assets.AddScript`
  - Pour `Scripts.GetScriptTags`, utiliser `Assets.GetScripts`

    Il s’agit d’une modification avec rupture ; la `Scripts` classe n’est pas disponible dans la version bêta. Les exemples de code dans ce document qui utilisent la gestion de ressources ont été mis à jour avec cette modification.

<a id="templates"></a>
### <a name="using-the-new-and-updated-site-templates"></a>L’aide des modèles de Site nouvelles et mises à jour

Le **Starter Site** modèle a été mis à jour afin qu’elle s’exécute sur le Web Pages 2 par défaut. Il inclut également les nouvelles fonctionnalités suivantes :

- Rendu de page d’adaptés aux appareils mobiles. Grâce à l’utilisation des styles CSS et le `@media` sélecteur, le **Starter Site** fournit amélioré le rendu des pages sur les petits écrans, y compris les écrans des appareils mobiles.
- Options d’authentification et d’appartenance améliorées. Vous pouvez permettre aux utilisateurs de se votre site à l’aide de leurs comptes à partir d’autres sites de réseaux sociaux tels que Twitter, Facebook et Windows Live. Pour plus d’informations, consultez la [l’activation des connexions à partir de Facebook et d’autres Sites à l’aide d’OAuth et OpenID](#oauthsetup) section.
- Éléments HTML5.

La nouvelle **Site personnel** modèle vous permet de créer un site Web qui contient un blog personnel, une page de photos et une page Twitter. Vous pouvez personnaliser un site basé sur le **Site personnel** modèle en procédant comme suit :

- Modifier l’apparence du site en modifiant le fichier de disposition (*\_SiteLayout.cshtml*) et le fichier de styles (*Site.css*).
- Installer les packages NuGet qui ajoutent des fonctionnalités à votre site. Pour plus d’informations sur l’installation des packages, y compris la bibliothèque de programmes d’assistance de Web ASP.NET, consultez le didacticiel [l’installation de programmes d’assistance](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers).

Pour accéder à la **Site personnel** modèle, choisissez **modèles** sur le WebMatrix **Quick Start** écran.

[![topseven-personalsite-1](top-features-in-web-pages-2/_static/image2.png)](top-features-in-web-pages-2/_static/image1.png)

Dans le **modèles** boîte de dialogue, choisissez le **Site personnel** modèle.

[![topseven-personalsite-2](top-features-in-web-pages-2/_static/image4.png)](top-features-in-web-pages-2/_static/image3.png)

La page d’accueil de la **Site personnel** modèle vous permet de suivre les liens à configurer votre blog, Twitter et photos.

[![topseven-personalsite-3](top-features-in-web-pages-2/_static/image6.png)](top-features-in-web-pages-2/_static/image5.png)

<a id="validation"></a>
### <a name="validating-user-input"></a>Validation des entrées utilisateur

Dans les Pages Web 1, pour valider l’entrée d’utilisateur sur les formulaires envoyés, utilisez la `System.Web.WebPages.Html.ModelState` classe. (Ceci est illustré dans plusieurs exemples de code dans le didacticiel 1 des Pages Web intitulé [utilisation des données](../data/5-working-with-data.md).) Vous pouvez toujours utiliser cette approche dans les 2 Pages Web. Toutefois, Web Pages 2 offre également des outils optimisés pour la validation de l’entrée d’utilisateur :

- Nouvelles classes de validation, notamment `System.Web.WebPages.ValidationHelper` et `System.Web.WebPages.Validator`, qui vous permettent d’effectuer des tâches de validation puissante avec quelques lignes de code.
- Si vous le souhaitez, une validation côté client, qui fournit des commentaires immédiats à l’utilisateur au lieu de demander un aller-retour au serveur pour vérifier les erreurs de validation. (Pour des raisons de sécurité, la validation est effectuée sur le serveur même si les vérifications ont été effectuées dans le client au préalable.)

Pour utiliser les nouvelles fonctionnalités de validation, procédez comme suit :

Dans le code de la page, enregistrer un élément à valider à l’aide des méthodes de la `Validation` helper : `Validation.RequireField`, `Validation.RequireFields` (pour inscrire plusieurs éléments comme étant obligatoire), ou `Validation.Add`. Le `Add` méthode vous permet de spécifier d’autres types de contrôles de validation, comme le type de données la vérification, la comparaison des entrées dans des champs différents, des vérifications de la longueur de chaîne et les modèles (à l’aide d’expressions régulières). Voici quelques exemples :

[!code-html[Main](top-features-in-web-pages-2/samples/sample2.html)]

Pour afficher une erreur spécifique au champ, appelez `Html.ValidationMessage` dans le balisage de chaque élément en cours de validation :

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample3.cshtml)]

Pour afficher un résumé (`<ul>` liste) de toutes les erreurs dans la page, `Html.ValidationSummary` dans le balisage :

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample4.cshtml)]

Ces étapes suffisent pour implémenter la validation côté serveur. Si vous souhaitez ajouter la validation côté client, procédez comme suit en outre.

Ajoutez les références de fichier de script suivantes à l’intérieur de la `<head>` section d’une page web. Les deux premières références de script pointent vers les fichiers à distance sur un serveur de diffusion de contenu (CDN). La troisième référence pointe vers un fichier de script local. Les applications de production doivent implémenter une stratégie de secours lorsque le CDN n’est pas disponible. Tester le basculement.

[!code-html[Main](top-features-in-web-pages-2/samples/sample5.html)]

Le moyen le plus simple pour obtenir une copie locale de la *jquery.validate.unobtrusive.min.js* bibliothèque consiste à créer un nouveau site Web Pages basé sur l’un des modèles de site (par exemple, Site de démarrage). Le site créé par le modèle inclut *jquery.validate.unobtrusive.js* fichier dans son dossier de Scripts, à partir de laquelle vous pouvez le copier sur votre site.

Si votre site Web utilise un<em>\_SiteLayout</em> page pour contrôler la mise en page, vous pouvez inclure ces références de script dans cette page afin que la validation est disponible pour toutes les pages de contenu. Si vous souhaitez effectuer une validation uniquement sur certaines pages, vous pouvez utiliser le Gestionnaire de ressources pour enregistrer les scripts sur les pages. Pour ce faire, appelez `Assets.AddScript(path)` dans la page que vous souhaitez valider et de faire référence à chacun des fichiers de script. Puis ajoutez un appel à `Assets.GetScripts` dans les  <em>\_SiteLayout</em> page afin de restituer inscrit `<script>` balises. Pour plus d’informations, consultez la section [l’inscription des Scripts avec le Gestionnaire de ressources](#resmanagement).

Dans le balisage pour un élément individuel, appelez le `Validation.For` (méthode). Cette méthode émet des attributs que jQuery pouvez raccorder afin de fournir la validation côté client. Par exemple :

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample6.cshtml)]

L’exemple suivant montre une page qui valide l’entrée d’utilisateur sur un formulaire. Pour exécuter et tester ce code de validation, procédez comme suit :

1. Créer un nouveau site web à l’aide d’un des modèles de site WebMatrix 2 qui inclut un *Scripts* dossier, tel que le **Starter Site** modèle.
2. Dans le nouveau site, créez un *.cshtml* page et remplacez le contenu de la page par le code suivant.
3. Exécutez la page dans un navigateur. Entrez les valeurs valides et non valides pour voir les effets sur la validation. Par exemple, un champ obligatoire ne renseignez ou entrez une lettre dans le **crédits** champ.


[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample7.cshtml)]

Voici la page lorsqu’un utilisateur soumet une entrée valide :

[![topSeven-valid-1](top-features-in-web-pages-2/_static/image8.png)](top-features-in-web-pages-2/_static/image7.png)

Voici la page lorsqu’un utilisateur envoie les données avec un champ obligatoire est laissé vide :

[![topSeven-valid-2](top-features-in-web-pages-2/_static/image10.png)](top-features-in-web-pages-2/_static/image9.png)

Voici la page lorsqu’un utilisateur le soumet à autre chose qu’un entier dans la **crédits** champ :

[![topSeven-valid-3](top-features-in-web-pages-2/_static/image12.png)](top-features-in-web-pages-2/_static/image11.png)

Pour plus d’informations, consultez les billets de blog suivants :

- [Mise à jour de la validation dans les Pages Web v2](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2344) bases de l’ajout de validation à l’aide du `Validation` helper (côté serveur uniquement)
- [Mise à jour de la validation dans les Pages Web v2, partie 2](http://www.mikepope.com/blog/DisplayBlog.aspx?permalink=2347) Ajout d’une validation côté client.
- [Mise à jour de la validation dans les Pages Web v2, partie 3](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2351) mise en forme des erreurs de validation.

<a id="resmanagement"></a>
### <a name="registering-scripts-using-the-assets-manager"></a>L’inscription des Scripts à l’aide du Gestionnaire de ressources

Le Gestionnaire de ressources est une nouvelle fonctionnalité que vous pouvez utiliser dans le code serveur pour inscrire et de restituer les scripts clients. Cette fonctionnalité est utile lorsque vous travaillez avec le code à partir de plusieurs fichiers (par exemple, les pages de disposition, les pages de contenu, les programmes d’assistance, etc.) qui sont combinées en une seule page en cours d’exécution. Le Gestionnaire de ressources coordonne les fichiers sources pour vous assurer que les fichiers de script sont correctement référencés et efficacement sur la page rendue, quel que soit les fichiers de code, elles sont appelées à partir ou combien de fois qu’elles sont appelées. Le Gestionnaire de ressources rend également `<script>` balises au bon endroit pour que la page peut charger rapidement (sans téléchargement des scripts lors du rendu) et pour éviter les erreurs qui peuvent se produire si les scripts sont appelés avant le rendu est terminé.

Par exemple, supposons que vous créez un programme d’assistance personnalisé qui appelle un fichier JavaScript, et que vous appelez ce programme d’assistance à trois emplacements différents dans votre code de la page de contenu. Si vous n’utilisez pas le Gestionnaire de ressources pour enregistrer le script appelle dans l’application auxiliaire, trois différentes `<script>` balises tous les points dans le même fichier de script s’affiche dans votre page rendue. De plus, selon l’emplacement où le `<script>` balises sont insérées dans la page rendue, erreurs peuvent se produire si le script essaie d’accéder à certains éléments de page avant le charge complet de la page. Si vous utilisez le Gestionnaire de ressources pour enregistrer le script, vous évitez ces problèmes.

Vous pouvez enregistrer un script avec le Gestionnaire de ressources en procédant ainsi :

- Dans le code qui a besoin pour référencer le script, appelez le `Assets.AddScript` (méthode).
- Dans un  *\_SiteLayout* page, appelez le `Assets.GetScripts` méthode pour restituer la `<script>` balises. 

    > [!NOTE]
    > Mettre des appels à `Assets.GetScripts` en tant que le tout dernier élément dans le `<body>` élément de la  *\_SiteLayout* page. Cela permet à la page de chargement plus rapide et peut aider à éviter les erreurs de script.

L’exemple suivant montre le fonctionnement du Gestionnaire de ressources. Le code contient les éléments suivants :

- Un programme d’assistance personnalisé nommé `MakeNote`. Ce programme d’assistance restitue une chaîne à l’intérieur d’une zone en encapsulant un `div` élément qui a un style avec une bordure et en ajoutant &quot;Remarque :&quot; à celui-ci. L’application d’assistance appelle également un fichier JavaScript qui ajoute le comportement au moment de l’exécution à la note. Plutôt que référencer le script avec un `<script>` balise, l’Assistant enregistre le script en appelant `Assets.AddScript` .
- Un fichier JavaScript. Il s’agit du fichier qui est appelé par l’application d’assistance et il augmente temporairement la taille de police des éléments de note pendant un `mouseover` événement.
- Une page de contenu qui fait référence à la<em>\_SiteLayout</em> effectue le rendu du contenu dans le corps de la page et appelle ensuite la `MakeNote` helper.
- A  *\_SiteLayout* page. Cette page fournit un en-tête commun et une structure de mise en page. Il inclut également un appel à `Assets.GetScripts`, qui est la façon dont le Gestionnaire de ressources restitue le script appelle dans une page.

Pour exécuter l’exemple :

1. Créer un site Web de Web Pages 2 vide. Vous pouvez utiliser le WebMatrix **Site vide** modèle pour cela.
2. Créez un dossier nommé *Scripts* dans le site.
3. Dans le *Scripts* dossier, créez un fichier nommé *Test.js*, copie le *Test.js* contenu dans celui-ci à partir de l’exemple, enregistrez le fichier...
4. Créez un dossier nommé *application\_Code* dans le site.
5. Dans le *application\_Code* dossier, créez un fichier nommé *Helpers.cshtml*, copiez l’exemple de code et l’enregistrer dans un dossier nommé *application\_Code*dans le dossier racine.
6. Dans le dossier du site racine, créez un fichier nommé  *\_SiteLayout.cshtml,* copiez l’exemple, enregistrez le fichier.
7. Dans la racine du site, créez un fichier nommé *ContentPage.cshtml*, ajoutez l’exemple de code et l’enregistrer.
8. Exécutez *ContentPage* dans un navigateur. La chaîne que vous avez passé à la `MakeNote` helper est restitué sous la forme d’une note convertie (boxed).
9. Passez le pointeur de la souris sur la note. Le script augmente temporairement la taille de police de la note.
10. Afficher la source de la page rendue. En raison de l’emplacement de l’appel à `Assets.GetScripts`, le rendu `<script>` qui appelle *Test.js* est le tout dernier élément dans le corps de la page.

*Test.js*

[!code-javascript[Main](top-features-in-web-pages-2/samples/sample8.js)]

*Helpers.cshtml*

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample9.cshtml)]

*\_SiteLayout.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample10.html)]

*ContentPage.cshtml*

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample11.cshtml)]

La capture d’écran suivante affiche *ContentPage.cshtml* dans un navigateur lorsque vous placez le pointeur de la souris sur la note :

[![topSeven-resmgr-1](top-features-in-web-pages-2/_static/image14.png)](top-features-in-web-pages-2/_static/image13.png)

<a id="oauthsetup"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>L’activation des connexions à partir de Facebook et d’autres Sites à l’aide d’OAuth et OpenID

Web Pages 2 fournit les options améliorées pour l’appartenance et l’authentification. L’amélioration principale est qu’il n’y nouvelle [OAuth](http://oauth.net/) et [OpenID](http://openid.net/) fournisseurs. À l’aide de ces fournisseurs, vous pouvez permettre aux utilisateurs de se votre site à l’aide de leurs informations d’identification existantes à partir de Facebook, Twitter, Windows Live, Google et Yahoo. Par exemple, pour vous connecter à l’aide d’un compte Facebook, les utilisateurs peuvent simplement sélectionner une icône de Facebook, qui redirige vers la page de connexion Facebook où ils entrent leurs informations de l’utilisateur. Ils peuvent ensuite associer la connexion Facebook à leur compte sur votre site. Une amélioration associée aux fonctionnalités d’appartenance Web Pages est que les utilisateurs peuvent associer plusieurs connexions (y compris les connexions à partir des sites de réseau social) avec un seul compte sur votre site Web.

Cette illustration montre la page de connexion à partir de la **Starter Site** modèle, où un utilisateur peut choisir une icône Facebook, Twitter ou Windows Live pour activer la journalisation à l’aide d’un compte externe :

[![topSeven-oauth-1](top-features-in-web-pages-2/_static/image16.png)](top-features-in-web-pages-2/_static/image15.png)

Vous pouvez activer l’appartenance OAuth et OpenID à l’aide de quelques lignes de code. Les méthodes et propriétés que vous utilisez pour travailler avec OAuth et OpenID fournisseurs sont dans le `WebMatrix.Security.OAuthWebSecurity` classe.

Toutefois, au lieu d’activer les connexions à partir d’autres sites à l’aide de code, une méthode recommandée pour la prise en main les nouveaux fournisseurs consiste à utiliser la nouvelle **Starter Site** modèle qui est inclus avec la version bêta 2 de WebMatrix. Le **Starter Site** modèle inclut une infrastructure d’abonnement complet, avec une page de connexion, une base de données d’appartenance et tout le code que vous avez besoin permettre aux utilisateurs de se votre site à l’aide des informations d’identification locales ou celles d’un autre site .

#### <a name="how-to-enable-logins-using-the-oauth-and-openid-providers"></a>Comment activer les connexions à l’aide de la OAuth et OpenID fournisseurs

Cette section fournit un exemple montrant comment permettre aux utilisateurs de se connecter à partir des sites externes (Facebook, Twitter, Windows Live, Google ou Yahoo) à un site qui est basé sur le **Starter Site** modèle. Après avoir créé un site de démarrage, vous effectuez ce (suivre plus d’informations) :

- Pour les sites qui utilisent un fournisseur OAuth (Facebook, Twitter et Windows Live), créez une application sur le site externe. Cela vous donne les clés d’application dont vous avez besoin pour appeler la fonctionnalité de connexion pour ces sites. Pour les sites qui utilisent un fournisseur OpenID (Google, Yahoo), il est inutile de créer une application. Pour tous ces sites, vous devez disposer un compte pour se connecter et créer des applications de développeur. 

    > [!NOTE]
    > Applications Windows Live acceptent uniquement une URL dynamique pour un site Web de travail, vous ne pouvez pas utiliser une URL de site Web local pour le test des connexions.
- Modifier quelques fichiers dans votre site Web afin de spécifier le fournisseur d’authentification approprié et soumettre une connexion au site que vous souhaitez utiliser.

**Activer les connexions Google et Yahoo**:

1. Dans votre site Web, vous devez modifier le  *\_AppStart.cshtml* page et ajoutez les deux lignes suivantes du code dans le bloc de code Razor après l’appel à la `WebSecurity.InitializeDatabaseConnection` (méthode). Ce code permet à la fois le Google et Yahoo OpenID fournisseurs. 

    [!code-css[Main](top-features-in-web-pages-2/samples/sample12.css)]
2. Dans le *~/Account/Login.cshtml* page, supprimez les commentaires dans la liste suivante `<fieldset>` bloc de balisage à la fin de la page. Pour les commentaires du code, supprimez le `@*` caractères qui précèdent et suivent le `<fieldset>` bloc. Le bloc de code qui en résulte ressemble à ceci :

    [!code-html[Main](top-features-in-web-pages-2/samples/sample13.html)]
3. Ajouter un `<input>` élément pour le fournisseur de Google ou Yahoo à la `<fieldset>` groupe dans le *~/Account/Login.cshtml* page. La mise à jour `<fieldset>` groupe `<input>` éléments pour la recherche de Google et Yahoo tels que l’exemple suivant : 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample14.html)]
4. Dans le *~/Account/AssociateServiceAccount.cshtml* , ajoutez `<input>` éléments pour Google ou Yahoo, à le `<fieldset>` groupe vers la fin du fichier. Vous pouvez copier le même `<input>` les éléments que vous venez d’ajouter à la `<fieldset>` section dans le *~/Account/Login.cshtml* page. 

    Le *~/Account/AssociateServiceAccount.cshtml* page du modèle Starter Site peut être utilisée si vous souhaitez créer une page sur laquelle les utilisateurs peuvent associer plusieurs connexions à partir d’autres sites avec un seul compte sur votre site Web.

Vous pouvez maintenant tester les connexions Google et Yahoo.

1. Exécutez le *default.cshtml* page de votre site et choisissez la **connecter** bouton.
2. Sur le *connexion* page, dans le **utiliser un autre service pour vous connecter** section, choisissez la **Google** ou **Yahoo** bouton Envoyer. Cet exemple utilise la connexion de Google. 

    La page web redirige la demande vers la page de connexion Google.

    [![topSeven-oauth-6](top-features-in-web-pages-2/_static/image18.png)](top-features-in-web-pages-2/_static/image17.png)
3. Entrez les informations d’identification d’un compte Google existant.
4. Si Google vous demande si vous souhaitez autoriser Localhost utiliser les informations du compte, cliquez sur **autoriser**.

    Le code utilise le jeton de Google pour authentifier l’utilisateur et il retourne ensuite à cette page sur votre site Web. Cette page permet aux utilisateurs d’associer leur connexion Google avec un compte existant sur votre site Web, ou ils peuvent inscrire un nouveau compte sur votre site à associer la connexion externe.

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image20.png)](top-features-in-web-pages-2/_static/image19.png)
5. Choisissez le **associer** bouton. Le navigateur retourne à la page d’accueil de votre application.

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image22.png)](top-features-in-web-pages-2/_static/image21.png)

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image24.png)](top-features-in-web-pages-2/_static/image23.png)

**Activer les connexions de Facebook**:

1. Accédez à la [site des développeurs Facebook](https://developers.facebook.com/apps) (se connecter si vous n’êtes pas déjà connecté).
2. Choisissez le **créer une application** bouton, puis suivez les invites pour nommer et de créer l’application.
3. Dans la section **sélectionnez la façon dont votre application s’intègre avec Facebook**, choisissez le **site Web** section.
4. Renseignez le **URL du Site** champ avec l’URL de votre site (par exemple, [ `http://www.example.com` ](http://www.example.com)). Le **domaine** champ est facultatif ; vous pouvez l’utiliser pour fournir l’authentification pour un domaine entier (tel que *example.com*). 

    > [!NOTE]
    > Si vous exécutez un site sur votre ordinateur local avec une URL telle que `http://localhost:12345` (où le nombre est un numéro de port local), vous pouvez ajouter cette valeur à la **URL du Site** champ pour tester votre site. Toutefois, chaque fois que le numéro de port de vos modifications de site local, vous devez mettre à jour le **URL du Site** champ de votre application.
5. Choisissez le **enregistrer les modifications** bouton.
6. Choisissez le **applications** onglet à nouveau et afficher la page de démarrage pour votre application.
7. Copie le **ID d’application** et **Secret d’application** valeurs pour votre application et les coller dans un fichier texte temporaire. Vous allez passer ces valeurs pour le fournisseur de Facebook dans votre code de site Web.
8. Quitter le site de développement Facebook.

Maintenant vous apportez des modifications à deux pages de votre site Web afin que les utilisateurs seront en mesure de se connecter au site à l’aide de leur compte Facebook.

1. Dans votre site Web, vous devez modifier le  *\_AppStart.cshtml* page et les commentaires du code pour le fournisseur Facebook OAuth. Le bloc de code sans commentaire ressemble à ceci : 

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample15.xml)]
2. Copie le **ID d’application** valeur à partir de l’application Facebook en tant que la valeur de le `consumerKey` paramètre (à l’intérieur des guillemets simples).
3. Copie **Secret d’application** valeur à partir de l’application Facebook en tant que le `consumerSecret` la valeur du paramètre.
4. Enregistrez et fermez le fichier.
5. Modifier la *~/Account/Login.cshtml* page et supprimez les commentaires de la `<fieldset>` bloc près de la fin de la page. Pour les commentaires du code, supprimez le `@*` caractères qui précèdent et suivent le `<fieldset>` bloc. Le bloc de code avec des commentaires supprimés ressemble au suivant : 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample16.html)]
6. Enregistrez et fermez le fichier.

Vous pouvez maintenant tester la connexion Facebook.

1. Exécution du site *default.cshtml* page et choisissez la **connexion** bouton.
2. Sur le *connexion* page, dans le **utiliser un autre service pour vous connecter** , choisissez le **Facebook** icône. 

    La page web redirige la demande vers la page de connexion Facebook.

    [![topSeven-oauth-2](top-features-in-web-pages-2/_static/image26.png)](top-features-in-web-pages-2/_static/image25.png)
3. Connectez-vous à un compte Facebook. 

    Le code utilise le jeton de Facebook pour vous authentifier et renvoie à une page où vous pouvez associer votre compte de connexion Facebook avec un compte de connexion de votre site. Votre utilisateur nom ou adresse de messagerie est renseigné dans la **messagerie** champ sur le formulaire.

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image28.png)](top-features-in-web-pages-2/_static/image27.png)
4. Choisissez le **associer** bouton. 

    Le navigateur retourne à la page d’accueil et que vous êtes connecté.

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image30.png)](top-features-in-web-pages-2/_static/image29.png)

**Pour activer les connexions Twitter :** 

1. Accédez à la [site des développeurs Twitter](https://dev.twitter.com/).
2. Choisissez le **créer une application** lier et ouvrir une session sur le site.
3. Sur le **créer une Application** écran, renseignez le **nom** et **Description** champs.
4. Dans le **site Web** , entrez l’URL de votre site (par exemple, [ `http://www.example.com` ](http://www.example.com)). 

    > [!NOTE]
    > Si vous testez votre site localement (à l’aide d’une URL telle que `http://localhost:12345`), Twitter ne peut pas accepter l’URL. Toutefois, vous pourrez peut-être utiliser l’adresse IP de bouclage locale (par exemple `http://127.0.0.1:12345`). Cela simplifie le processus de test de votre application localement. Toutefois, chaque fois que le numéro de port de votre site local change, vous devez mettre à jour le **site Web** champ de votre application.
5. Dans le **URL de rappel** , saisissez une URL de la page dans votre site Web que vous souhaitez que les utilisateurs renvoyer à une fois la connexion Twitter. Par exemple, pour envoyer les utilisateurs vers la page d’accueil du Site de démarrage (qui reconnaît leur état connecté), entrez la même URL que vous avez entré dans le **site Web** champ.
6. Acceptez les termes du contrat et sélectionnez le **créer votre application Twitter** bouton.
7. Sur le **Mes Applications** accueil page, choisissez l’application que vous avez créé.
8. Sur le **détails** onglet, faites défiler vers le bas et sélectionnez le **créer mon jeton d’accès** bouton.
9. Sur le **détails** onglet, copiez la **clé de consommateur** et **Secret de consommateur** valeurs pour votre application et les coller dans un fichier texte temporaire. Vous allez passer ces valeurs au fournisseur Twitter dans votre code de site Web.
10. Quittez le site Twitter.

Maintenant vous apportez des modifications à deux pages de votre site Web afin que les utilisateurs pourront se connecter au site à l’aide de leur compte Twitter.

1. Dans votre site Web, vous devez modifier le  *\_AppStart.cshtml* page et les commentaires du code pour le fournisseur OAuth pour Twitter. Le bloc de code sans commentaire ressemble à ceci : 

    [!code-csharp[Main](top-features-in-web-pages-2/samples/sample17.cs)]
2. Copie le **clé de consommateur** valeur à partir de l’application Twitter comme valeur de le `consumerKey` paramètre (à l’intérieur des guillemets simples).
3. Copie le **Secret de consommateur** valeur à partir de l’application Twitter comme valeur de le `consumerSecret` paramètre.
4. Enregistrez et fermez le fichier.
5. Modifier la *~/Account/Login.cshtml* page et supprimez les commentaires de la `<fieldset>` bloc près de la fin de la page. Pour les commentaires du code, supprimez le `@*` caractères qui précèdent et suivent le `<fieldset>` bloc. Le bloc de code avec des commentaires supprimés ressemble au suivant : 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample18.html)]
6. Enregistrez et fermez le fichier.

Vous pouvez maintenant tester la connexion Twitter.

1. Exécutez le *default.cshtml* page de votre site et choisissez la **connexion** bouton.
2. Sur le *connexion* page, dans le **utiliser un autre service pour vous connecter** , choisissez le **Twitter** icône. 

    La page web redirige la demande vers une page de connexion Twitter pour l’application que vous avez créé.

    [![topSeven-oauth-4](top-features-in-web-pages-2/_static/image32.png)](top-features-in-web-pages-2/_static/image31.png)
3. Connectez-vous à un compte Twitter.
4. Le code utilise le jeton Twitter pour authentifier l’utilisateur et vous ramène à une page dans laquelle vous pouvez associer votre nom de connexion avec votre compte de site Web. Votre nom ou adresse de messagerie est renseigné dans la **messagerie** champ sur le formulaire.

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image34.png)](top-features-in-web-pages-2/_static/image33.png)
5. Choisissez le **associer** bouton. 

    Le navigateur retourne à la page d’accueil et que vous êtes connecté.

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image36.png)](top-features-in-web-pages-2/_static/image35.png)

<a id="maphelper"></a>
### <a name="adding-maps-using-the-maps-helper"></a>Ajout de mappages à l’aide de l’application d’assistance de mappages

Web Pages 2 inclut des ajouts à la bibliothèque de programmes d’assistance de Web ASP.NET, qui est un package de compléments pour un site Web Pages. Une d’elles est un composant de mappage fourni par le `Microsoft.Web.Helpers.Maps` classe. Vous pouvez utiliser la `Maps` classe génère des mappages en vous basés sur une adresse ou sur un jeu de coordonnées de latitude et de longitude. La `Maps` classe vous permet de vous appeler directement des moteurs de carte populaires, y compris de Bing, Google, MapQuest et Yahoo.

Pour utiliser le nouveau `Maps` classe dans votre site Web, vous devez d’abord installer la version 2 de la bibliothèque d’applications auxiliaires Web. Pour ce faire, accédez aux instructions pour l’installation de la version actuellement publiée de la [ASP.NET Web Helpers Library](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers) et installez la version 2.

Les étapes d’ajout du mappage à une page sont identiques quel des moteurs de carte que vous appelez. Vous suffit d’ajouter une référence de fichier JavaScript à votre page de mappage et puis ajoutez un appel qui restitue le `<script>` sur votre page. Appelez ensuite sur votre page de mappage, le moteur de carte que vous souhaitez utiliser.

L’exemple suivant montre comment créer une page qui restitue une table basée sur une adresse et une autre page qui restitue une table basée sur des coordonnées de longitude et de latitude. L’exemple de mappage d’adresse utilise Google Maps, et l’exemple de mappage de coordonnées utilise Bing Maps. Notez les éléments suivants dans le code :

- L’appel à `Assets.AddScript` en haut des deux pages de mappage. Cette méthode ajoute une référence à la *jquery-1.6.2.min.js* fichier qui est inclus dans le **Starter Site** modèle et qui est requis par le `Maps` classe.
- L’appel à la `Assets.GetScripts` méthode dans le fichier de disposition. Cette méthode restitue la `<script>` sur les deux pages de mappage de balise.
- L’appel à la `@Maps.GetGoogleHtml` et `@Maps.GetBingHtml` méthodes dans les pages de mappage. Pour mapper une adresse, vous devez passer une chaîne d’adresse. Pour mapper les coordonnées, vous devez passer la longitude et latitude coordonnées. Pour le moteur de Bing Maps, vous devez également transmettre une clé (que vous puissiez obtenir gratuitement en vous inscrivant à la [site Bing Maps développeurs](https://www.microsoft.com/maps/developers/web.aspx)). Les méthodes pour les autres moteurs de carte fonctionnent de manière similaire (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).

Pour créer des pages de mappage :

1. Créer un site Web basé sur le **Starter Site** modèle.
2. Créez un fichier nommé *MapAddress.cshtml* à la racine du site. Cette page génère une table basée sur une adresse que vous passez à celui-ci.
3. Copiez le code suivant dans le fichier, en remplaçant le contenu existant. 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample19.cshtml)]
4. Créez un fichier nommé  *\_MapLayout.cshtml* à la racine du site. Cette page sera la page de disposition pour les deux pages de mappage.
5. Copiez le code suivant dans le fichier, en remplaçant le contenu existant. 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample20.html)]
6. Créez un fichier nommé *MapCoordinates.cshtml* à la racine du site. Cette page génère une table basée sur un jeu de coordonnées que vous passez à celui-ci.
7. Copiez le code suivant dans le fichier, en remplaçant le contenu existant. 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample21.cshtml)]

Pour tester vos pages de mappage :

1. Exécutez la page *MapAddress.cshtml* fichier.
2. Entrez une chaîne de l’adresse complète, y compris une adresse postale, état ou province et le code postal, puis choisissez le **carte** bouton. La page affiche un mappage à partir de Google Maps : 

    [![topseven-maphelper-1](top-features-in-web-pages-2/_static/image38.png)](top-features-in-web-pages-2/_static/image37.png)
3. Rechercher un jeu de coordonnées de latitude et longitude pour un emplacement spécifique.
4. Exécutez la page *MapCoordinates.cshtml*. Entrez les coordonnées, puis choisissez le **carte** bouton. La page affiche un mappage à partir de Bing Maps : 

    [![topseven-maphelper-2](top-features-in-web-pages-2/_static/image40.png)](top-features-in-web-pages-2/_static/image39.png)

<a id="sidebyside"></a>
### <a name="running-web-pages-applications-side-by-side"></a>Exécution des Pages Web Applications côte à côte

Pages Web 2 ajoute la possibilité d’exécuter des applications côte à côte. Cela vous permet de continuer à exécuter vos applications Web Pages 1, créer de nouvelles applications Web Pages 2 et tous les exécuter sur le même ordinateur.

Voici quelques points à retenir lorsque vous installez la version bêta 2 de Pages Web avec WebMatrix :

- Par défaut, les applications Web Pages existantes exécutera en tant qu’applications de la version 2 sur votre ordinateur. (Les assemblys de version 2 sont installés dans le GAC et seront utilisés automatiquement.)
- Si vous souhaitez exécuter un site à l’aide de la version 1 des Pages Web (au lieu de la valeur par défaut, comme dans le point précédent), vous pouvez configurer le site pour ce faire. Si votre site n’a pas encore un *web.config* dans la racine du site, créez-en un et copiez-y le code XML suivant, remplacer le contenu existant. Si le site contient déjà un *web.config* , ajoutez une `<appSettings>` élément tel que le suivant à la `<configuration>` section.

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample22.xml)]
  '-Si vous ne spécifiez pas une version dans le *web.config* fichier, un site est déployé comme un site de version 2. (Les assemblys de version 2 sont copiés vers le *bin* dossier dans le site déployé.)
- Nouvelles applications que vous créez à l’aide des modèles de site dans la version Web Matrix bêta 2 inclut les assemblys de version 2 des Pages Web dans la table *bin* dossier.

En règle générale, vous pouvez contrôler toujours la version de Pages Web à utiliser avec votre site à l’aide de NuGet pour installer les assemblys appropriés dans le site *bin* dossier. Pour rechercher les packages, visitez [NuGet.org](http://NuGet.org).

<a id="mobile"></a>
### <a name="rendering-pages-for-mobile-devices"></a>Rendu des Pages pour les appareils mobiles

Pages Web 2 vous permet de créer des affichages personnalisés pour restituer le contenu sur mobile ou d’autres périphériques.

Le `System.Web.WebPages` espace de noms contient les classes suivantes qui vous permettent de travailler avec les modes d’affichage : `DefaultDisplayMode`, `DisplayInfo`, et `DisplayModes`. Vous pouvez utiliser ces classes directement et écrire du code qui restitue le résultat approprié pour des périphériques spécifiques.

Vous pouvez également créer des pages spécifiques à l’appareil à l’aide d’un modèle d’affectation de noms de fichier comme celui-ci : <em>nom de fichier.</em> <em>Mobile</em><em>.cshtml</em>. Par exemple, vous pouvez créer deux versions d’une page, un nommé <em>MyFile.cshtml</em> et l’autre nommé <em>MyFile.Mobile.cshtml</em>. À l’exécution, quand un appareil mobile demande <em>MyFile.cshtml</em>, Pages Web restitue le contenu à partir de <em>MyFile.Mobile.cshtml</em>. Dans le cas contraire, <em>MyFile.cshtml</em> est rendu.

L’exemple suivant montre comment activer le rendu mobile en ajoutant une page de contenu pour les appareils mobiles. *Page1.cshtml* contient le contenu ainsi qu’une barre latérale de navigation. *Page1.Mobile.cshtml* contient le même contenu, mais omet la barre latérale.

Pour générer et exécuter l’exemple de code :

1. Dans un site Web Pages, créez un fichier nommé *Page1.cshtml* et copiez le *Page1.cshtml* contenu dans celui-ci à partir de l’exemple.
2. Créez un fichier nommé *Page1.Mobile.cshtml* et copiez le *Page1.Mobile.cshtml* contenu dans celui-ci à partir de l’exemple. Notez que la version mobile de la page omet la section de navigation pour optimiser le rendu sur un écran de petite taille.
3. Exécuter un navigateur et accédez à *Page1.cshtml*.
4. Exécuter un navigateur mobile (ou un émulateur d’appareil mobile) et accédez à *Page1.cshtml*. Notez que cette fois, les Pages Web restitue la version mobile de la page. 

    > [!NOTE]
    > Pour tester les pages mobiles, vous pouvez utiliser un simulateur d’appareil mobile qui s’exécute sur un ordinateur de bureau. Cet outil vous permet de tester les pages web comme elles ressemblent sur les appareils mobiles (autrement dit, généralement avec un beaucoup plus petite afficher la zone). Par exemple, un simulateur est la [un module complémentaire du sélecteur de l’Agent utilisateur](http://addons.mozilla.org/en-us/firefox/addon/user-agent-switcher/) pour Mozilla Firefox, qui vous permet d’émuler différents navigateurs des appareils mobiles à partir d’une version de bureau de Firefox.

*Page1.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample23.html)]

*Page1.Mobile.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample24.html)]

*Page1.cshtml* rendus dans un navigateur de bureau :

[![topseven-displaymodes-1](top-features-in-web-pages-2/_static/image42.png)](top-features-in-web-pages-2/_static/image41.png)

*Page1.Mobile.cshtml* affiché dans une vue de simulateur iPhone Apple dans le navigateur Firefox. Même si la demande est de *Page1.cshtml*, les convertisseurs d’application *Page1.Mobile.cshtml*.

[![topseven-displaymodes-2](top-features-in-web-pages-2/_static/image44.png)](top-features-in-web-pages-2/_static/image43.png)

<a id="resources"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

### <a name="aspnet-web-pages-1-resources"></a>Ressources 1 des Pages Web ASP.NET

> [!NOTE]
> La plupart des Pages Web 1 programmation et ressources d’API s’appliquent toujours à 2 des Pages Web.

- [Introduction à la programmation des Pages Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202890)

### <a name="webmatrix-resources"></a>Ressources de WebMatrix

- [WebMatrix 2 Nouveautés](http://webmatrix.com/next)
- [Microsoft WebMatrix Site](https://go.microsoft.com/fwlink/?LinkID=195076)
- [Starting Web Development with Microsoft WebMatrix](https://msdn.microsoft.com/en-us/library/hh145669(v=VS.99).aspx)(inclut une application Web Pages standard)
