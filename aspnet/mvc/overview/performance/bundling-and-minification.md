---
uid: mvc/overview/performance/bundling-and-minification
title: Regroupement et minimisation | Microsoft Docs
author: Rick-Anderson
description: Regroupement et minimisation sont deux techniques vous pouvez utiliser dans ASP.NET 4.5 pour améliorer les temps de chargement de demande. Regroupement et minimisation améliore les temps de chargement en reducin...
ms.author: aspnetcontent
ms.date: 08/23/2012
ms.assetid: 5894dc13-5d45-4dad-8096-136499120f1d
msc.legacyurl: /mvc/overview/performance/bundling-and-minification
msc.type: authoredcontent
ms.openlocfilehash: 4e72804593c07318af8cc577f9d43ab96be4de05
ms.sourcegitcommit: cb0c27fa0184f954fce591d417e6ab2a51d8bb22
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39123786"
---
<a name="bundling-and-minification"></a>Regroupement et minimisation
====================
par [Rick Anderson](https://github.com/Rick-Anderson)

> Regroupement et minimisation sont deux techniques vous pouvez utiliser dans ASP.NET 4.5 pour améliorer les temps de chargement de demande. Regroupement et minimisation améliore les temps de chargement en réduisant le nombre de demandes au serveur et en réduisant la taille des ressources demandées (par exemple, CSS et JavaScript).


La plupart des principaux navigateurs actuels limite le nombre de [connexions simultanées](http://www.browserscope.org/?category=network) par chaque nom d’hôte à six. Cela signifie que pendant le traitement des demandes de six, des demandes supplémentaires de ressources sur un ordinateur hôte seront mises en attente par le navigateur. Dans l’image ci-dessous, les onglets de réseau d’outils de développeur Internet Explorer F12 présente la temporisation pour les ressources requises par la vue About d’un exemple d’application.

![B/M](bundling-and-minification/_static/image1.png)

Les barres grises affichent le temps que la demande est en file d’attente par le navigateur en attente sur la limite de six connexions. La barre jaune est la durée de la demande jusqu’au premier octet, autrement dit, le temps nécessaire pour envoyer la demande et de recevoir la première réponse du serveur. Les barres bleues indiquent le temps nécessaire pour recevoir les données de réponse du serveur. Vous pouvez double-cliquer sur un élément multimédia pour obtenir des informations de minutage détaillées. Par exemple, l’illustration suivante montre les détails de minuterie pour le chargement du */Scripts/MyScripts/JavaScript6.js* fichier.

![](bundling-and-minification/_static/image2.png)

L’illustration précédente montre le **Démarrer** événement, qui donne l’heure de la demande a été en file d’attente en raison du navigateur limite le nombre de connexions simultanées. Dans ce cas, la demande a été en file d’attente en millisecondes 46 attend une autre demande terminer.

## <a name="bundling"></a>Le regroupement

Le regroupement est une nouvelle fonctionnalité dans ASP.NET 4.5 qui permet de facilement combiner ou regrouper plusieurs fichiers dans un seul fichier. Vous pouvez créer des CSS, JavaScript et autres offres groupées. Moins de fichiers signifie moins de requêtes HTTP et qui peut améliorer les performances de chargement première page.

L’illustration suivante montre la même vue de minutage de la vue About indiquée précédemment, mais cette fois avec le regroupement et minimisation activé.

![](bundling-and-minification/_static/image3.png)

## <a name="minification"></a>Minimisation

Minimisation effectue diverses optimisations de code différents css, comme la suppression d’un espace blanc inutile et vos commentaires et raccourcir les noms de variables pour un caractère ou de scripts. Examinons la fonction JavaScript ci-dessous.

[!code-javascript[Main](bundling-and-minification/samples/sample1.js)]

Après la minimisation, la fonction est réduite à ce qui suit :

[!code-javascript[Main](bundling-and-minification/samples/sample2.js)]

Outre la suppression de commentaires et l’espace blanc inutile, les paramètres suivants et les noms de variables ont été renommés (raccourci) comme suit :

| **Langue source** | **Renommé** |
| --- | --- |
| imageTagAndImageID | n |
| imageContext | t |
| imageElement | g |

## <a name="impact-of-bundling-and-minification"></a>Impact de regroupement et minimisation

Le tableau suivant montre plusieurs différences importantes entre la liste de toutes les ressources individuellement et à l’aide de regroupement et minimisation (B/M) dans l’exemple de programme.

|  | **À l’aide de B/M** | **Sans B/M** | **Modification** |
| --- | --- | --- | --- |
| **Demandes de fichiers** | 9 | 34 | 256% |
| **Ko envoyé** | 3.26 | 11.92 | 266% |
| **Ko reçus** | 388.51 | 530 | 36% |
| **Temps de chargement** | 510 MS | 780 MS | 53% |

Les octets envoyés avaient une réduction significative avec regroupement comme navigateurs sont assez détaillés avec les en-têtes HTTP qu'elles s’appliquent sur les demandes. La réduction du nombre d’octets reçus n’est pas aussi volumineuse, car les fichiers les plus volumineux (*Scripts\\jquery-ui-1.8.11.min.js* et *Scripts\\jquery-1.7.1.min.js*) étant déjà minimisés . Remarque : Le minutage sur l’exemple de programme utilisé le [Fiddler](http://www.fiddler2.com/fiddler2/) outil pour simuler un réseau lent. (À partir de la Fiddler **règles** menu, sélectionnez **performances** puis **simuler les vitesses de Modem**.)

## <a name="debugging-bundled-and-minified-javascript"></a>Débogage regroupés et minimisés JavaScript

Il est facile de déboguer votre code JavaScript dans un environnement de développement (où le [compilation élément](https://msdn.microsoft.com/library/s10awwz0.aspx) dans le *Web.config* fichier est défini sur `debug="true"` ), car les fichiers JavaScript ne sont pas regroupées ou minimisés. Vous pouvez également déboguer une version Release où vos fichiers JavaScript sont regroupés et minimisés. Les outils de développement F12 d’Internet Explorer, vous de déboguer une fonction JavaScript incluse dans un regroupement minimisée à l’aide de l’approche suivante :

1. Sélectionnez le **Script** onglet, puis sélectionnez le **démarrer le débogage** bouton.
2. Sélectionnez le groupe qui contient la fonction JavaScript que vous souhaitez déboguer à l’aide du bouton de ressources.  
    ![](bundling-and-minification/_static/image4.png)
3. Mettre en forme le code JavaScript minimisée en sélectionnant le **bouton Configuration** ![](bundling-and-minification/_static/image5.png), puis en sélectionnant **Format JavaScript**.
4. Dans le **recherche Script** zone d’entrée, sélectionnez le nom de la fonction que vous souhaitez déboguer. Dans l’image suivante, **AddAltToImg** a été entré dans le **recherche Script** zone d’entrée.  
    ![](bundling-and-minification/_static/image6.png)

Pour plus d’informations sur le débogage avec les outils de développement F12, consultez l’article MSDN [à l’aide des outils de développement F12 pour déboguer les erreurs JavaScript](https://msdn.microsoft.com/library/ie/gg699336(v=vs.85).aspx).

## <a name="controlling-bundling-and-minification"></a>Contrôle de regroupement et minimisation

Regroupement et minimisation est activé ou désactivé en définissant la valeur de l’attribut de débogage dans le [compilation élément](https://msdn.microsoft.com/library/s10awwz0.aspx) dans le *Web.config* fichier. Dans le code XML suivant, `debug` est défini sur true, le regroupement et minimisation est désactivée.

[!code-xml[Main](bundling-and-minification/samples/sample3.xml?highlight=2)]

Pour activer le regroupement et minimisation, définissez la `debug` valeur « False ». Vous pouvez remplacer le *Web.config* avec la `EnableOptimizations` propriété sur le `BundleTable` classe. Le code suivant permet de regroupement et minimisation et remplace tous les paramètres dans le *Web.config* fichier.

[!code-csharp[Main](bundling-and-minification/samples/sample4.cs?highlight=7)]

> [!NOTE]
> Sauf si `EnableOptimizations` est `true` ou l’attribut de débogage dans le [compilation élément](https://msdn.microsoft.com/library/s10awwz0.aspx) dans le *Web.config* fichier est défini sur `false`, fichiers ne seront pas fournis ou minimisés. En outre, la version .min des fichiers ne sera pas utilisée, les versions de débogage complète seront sélectionnées. `EnableOptimizations` remplace l’attribut de débogage dans le [compilation élément](https://msdn.microsoft.com/library/s10awwz0.aspx) dans le *Web.config* fichier


## <a name="using-bundling-and-minification-with-aspnet-web-forms-and-web-pages"></a>À l’aide de regroupement et minimisation avec ASP.NET Web Forms et les Pages Web

- Pour les Pages Web, consultez le billet de blog [Ajout de l’optimisation du Web vers un Site Web Pages](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx).
- Pour les Web Forms, consultez le billet de blog [Ajout de regroupement et minimisation à Web Forms](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx).

## <a name="using-bundling-and-minification-with-aspnet-mvc"></a>À l’aide de regroupement et minimisation avec ASP.NET MVC

Dans cette section, nous allons créer un ASP.NET MVC projet pour examiner le regroupement et minimisation. Tout d’abord, créez un nouveau projet ASP.NET MVC internet nommé **MvcBM** sans modifier les valeurs par défaut.

Ouvrir le *application\\\_Démarrer\\BundleConfig.cs* de fichiers et d’examiner le `RegisterBundles` méthode qui est utilisée pour créer, enregistrer et configurer des offres groupées. Le code suivant montre une partie de la `RegisterBundles` (méthode).

[!code-csharp[Main](bundling-and-minification/samples/sample5.cs)]

Le code précédent crée un nouveau lot de JavaScript nommé *~/bundles/jquery* qui inclut toutes les approprié (qui est le débogage ou réduites, mais pas. *VSDoc*) des fichiers dans le *Scripts* dossier qui correspond à la chaîne de caractère générique « ~/Scripts/jquery-{version} .js ». Pour ASP.NET MVC 4, cela signifie qu’avec une configuration debug, le fichier *jquery-1.7.1.js* seront ajoutés au regroupement. Dans une configuration release, *jquery-1.7.1.min.js* sera ajouté. L’infrastructure de regroupement suit plusieurs conventions courantes telles que :

- En sélectionnant « .min » fichier pour la version quand *FileX.min.js* et *FileX.js* existe.
- Sélectionner la version de .min « non » pour le débogage.
- En ignorant »-vsdoc « fichiers (tels que *jquery-1.7.1-vsdoc.js*), qui sont uniquement utilisé par IntelliSense.

Le `{version}` correspondance générique indiqué ci-dessus est utilisé pour créer automatiquement un bundle jQuery avec la version appropriée de jQuery dans votre *Scripts* dossier. Dans cet exemple, à l’aide d’un caractère générique offre les avantages suivants :

- Vous permet d’utiliser NuGet pour mettre à jour vers une version plus récente de jQuery sans modifier le code de regroupement précédente ou les références de jQuery dans vos pages de vue.
- Sélectionne la version complète pour les configurations debug et la version « .min » pour la mise en production génère automatiquement.

## <a name="using-a-cdn"></a>À l’aide d’un CDN

 Le code suivant remplace le groupe de jQuery local avec une offre groupée de CDN jQuery.

[!code-csharp[Main](bundling-and-minification/samples/sample6.cs)]

Dans le code ci-dessus, jQuery sera demandé à partir du CDN, tandis que dans la version en mode et la version debug de jQuery seront extraites localement en mode débogage. Lorsque vous utilisez un CDN, vous devez avoir un mécanisme de secours en cas d’échec de la demande CDN. Le balisage suivant, extrait de la fin du script montre de fichier de disposition ajouté à la requête jQuery si le CDN en panne.

[!code-cshtml[Main](bundling-and-minification/samples/sample7.cshtml?highlight=5-13)]

## <a name="creating-a-bundle"></a>Création d’un regroupement

Le [Bundle](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) classe `Include` méthode prend un tableau de chaînes, où chaque chaîne est un chemin d’accès virtuel à la ressource. Le code suivant à partir de la `RegisterBundles` méthode dans le *application\\\_Démarrer\\BundleConfig.cs* fichier montre comment plusieurs fichiers sont ajoutés à un regroupement :

[!code-csharp[Main](bundling-and-minification/samples/sample8.cs)]

Le [Bundle](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) classe `IncludeDirectory` méthode est fournie pour ajouter tous les fichiers dans un répertoire (et éventuellement tous les sous-répertoires) qui correspondent à un modèle de recherche. Le [Bundle](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) classe `IncludeDirectory` API est indiqué ci-dessous :

[!code-csharp[Main](bundling-and-minification/samples/sample9.cs)]

Offres groupées sont référencées dans les vues à l’aide de la méthode Render, (`Styles.Render` pour CSS et `Scripts.Render` pour JavaScript). Le balisage suivant à partir de la *vues\\partagé\\\_Layout.cshtml* fichier montre comment les vues de projet par défaut ASP.NET internet référencent les offres groupées CSS et JavaScript.

[!code-cshtml[Main](bundling-and-minification/samples/sample10.cshtml?highlight=5-6,11)]

Notez que les méthodes de rendu prend un tableau de chaînes, vous pouvez donc ajouter plusieurs lots sur une seule ligne de code. En règle générale, vous devez utiliser les méthodes de rendu qui créent le code HTML nécessaire pour faire référence à la ressource. Vous pouvez utiliser la `Url` méthode permettant de générer l’URL à la ressource sans le balisage nécessaire pour référencer l’élément multimédia. Supposons que vous souhaitez utiliser le nouveau HTML5 [async](http://www.whatwg.org/specs/web-apps/current-work/#attr-script-async) attribut. Le code suivant montre comment référencer à l’aide de modernizr le `Url` (méthode).

[!code-cshtml[Main](bundling-and-minification/samples/sample11.cshtml?highlight=11)]

## <a name="using-the--wildcard-character-to-select-files"></a>À l’aide de la «\*« caractère générique pour sélectionner les fichiers

Le chemin d’accès virtuel spécifié dans le `Include` (méthode) et la recherche de modèle dans le `IncludeDirectory` méthode peut accepter un «\*« caractère générique en tant que préfixe ou suffixe à dans le dernier segment de chemin d’accès. La chaîne de recherche respecte la casse. Le `IncludeDirectory` méthode a la possibilité de rechercher dans les sous-répertoires.

Prenez un projet avec les fichiers JavaScript suivants :

- *Scripts\\commune\\AddAltToImg.js*
- *Scripts\\commune\\ToggleDiv.js*
- *Scripts\\commune\\ToggleImg.js*
- *Scripts\\commune\\Sub1\\ToggleLinks.js*

![dir imag](bundling-and-minification/_static/image7.png)

Le tableau suivant présente les fichiers ajoutés à un ensemble en utilisant le caractère générique, comme indiqué :

| **Call** | **Fichiers ajoutés ou Exception levée** |
| --- | --- |
| Inclure (« ~/Scripts/Common/\*.js ») | *AddAltToImg.js*, *ToggleDiv.js*, *ToggleImg.js* |
| Inclure (« ~/Scripts/Common/T\*.js ») | Exception de modèle non valide. Le caractère générique est uniquement autorisé sur le préfixe ou le suffixe. |
| Inclure (« ~/Scripts/Common/\*og.\*») | Exception de modèle non valide. Qu’un seul caractère générique est autorisé. |
| Inclure (« ~/Scripts/Common/T\*») | *ToggleDiv.js*, *ToggleImg.js* |
| Inclure (« ~/Scripts/Common/\*») | Exception de modèle non valide. Un segment de caractère générique pure n’est pas valide. |
| IncludeDirectory("~/Scripts/Common", "T\*") | *ToggleDiv.js*, *ToggleImg.js* |
| IncludeDirectory (« ~/Scripts/Common », « T\*", true) | *ToggleDiv.js*, *ToggleImg.js*, *ToggleLinks.js* |

Ajouter explicitement chaque fichier pour un regroupement est généralement la préférée sur le chargement de caractère générique de fichiers pour les raisons suivantes :

- Ajout de scripts par défaut de caractère générique pour les charger dans l’ordre alphabétique, qui est généralement pas ce que vous voulez. Fichiers CSS et JavaScript doivent souvent être ajoutés dans un ordre spécifique (non alphabétiques). Vous pouvez réduire ce risque en ajoutant un personnalisé [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) implémentation, mais en ajoutant explicitement chaque fichier est moins sujette aux erreurs. Par exemple, vous pouvez ajouter de nouveaux éléments multimédias dans un dossier à l’avenir, ce qui peuvent vous amener à modifier votre [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) implémentation.
- Ajouté à un répertoire à l’aide du caractère générique du chargement des fichiers spécifiques de vue peuvent être inclus dans toutes les vues faisant référence à cette offre groupée. Si le script spécifique d’affichage est ajouté à un regroupement, vous pouvez obtenir une erreur JavaScript sur les autres vues qui font référence à l’offre groupée.
- Les fichiers importés chargés à deux reprises entraînent des fichiers CSS qui importer d’autres fichiers. Par exemple, le code suivant crée un regroupement avec la plupart des fichiers CSS thème de l’interface utilisateur de jQuery chargés à deux reprises. 

    [!code-csharp[Main](bundling-and-minification/samples/sample12.cs)]

  Le sélecteur de caractère générique «\*.css » apporte dans chaque fichier CSS dans le dossier, y compris le *contenu\\thèmes\\base\\jquery.ui.all.css* fichier. Le *jquery.ui.all.css* fichier importe les autres fichiers CSS.

## <a name="bundle-caching"></a>Regrouper la mise en cache

Offres groupées définir l’en-tête d’expiration HTTP un an à partir de la création de l’offre groupée. Si vous accédez à une page précédemment affichée, montre Fiddler IE ne rend pas une demande conditionnelle pour le regroupement, autrement dit, il existe aucune demande HTTP GET à partir d’Internet Explorer pour les regroupements et aucune réponse HTTP 304 à partir du serveur. Vous pouvez forcer IE pour effectuer une demande conditionnelle pour chaque regroupement avec la touche F5 (entraînant une réponse HTTP 304 pour chaque regroupement). Vous pouvez forcer une actualisation complète à l’aide de ^ F5 (entraînant une réponse HTTP 200 pour chaque regroupement.)

L’illustration suivante montre le **mise en cache** onglet du volet de réponse Fiddler :

![image de mise en cache de Fiddler](bundling-and-minification/_static/image8.png)

La demande   
`http://localhost/MvcBM_time/bundles/AllMyScripts?v=r0sLDicvP58AIXN_mc3QdyVvVj5euZNzdsa2N1PKvb81`  
 est pour le regroupement **AllMyScripts** et contient une paire de chaîne de requête **v = r0sLDicvP58AIXN\\\_mc3QdyVvVj5euZNzdsa2N1PKvb81**. La chaîne de requête **v** a la valeur de jeton qui est un identificateur unique utilisé pour la mise en cache. Tant que l’application ne change pas, l’application ASP.NET demande le **AllMyScripts** regrouper à l’aide de ce jeton. Si n’importe quel fichier dans le regroupement change, l’infrastructure d’optimisation ASP.NET génère un nouveau jeton, ce qui garantit que les demandes du navigateur pour le regroupement obtiennent le dernier groupe.

Si vous exécutez les outils de développement F12 d’Internet Explorer 9 et que vous accédez à une page précédemment chargée, IE incorrectement montre les demandes GET conditionnelles apportées à chaque groupe et le serveur de renvoi HTTP 304. Vous pouvez lire Pourquoi Internet Explorer 9 a des problèmes de détermination si une demande conditionnelle a été effectuée dans le billet de blog [à l’aide de CDN et Expires pour améliorer les performances de Site Web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

## <a name="less-coffeescript-scss-sass-bundling"></a>MOINS, CoffeeScript, SCSS, Sass de regroupement.

L’infrastructure de regroupement et minimisation fournit un mécanisme permettant de traiter les langages intermédiaires telle que [SCSS](http://sass-lang.com/), [Sass](http://sass-lang.com/), [moins](http://www.dotlesscss.org/) ou [Coffeescript ](http://coffeescript.org/)et appliquer des transformations telles que la minimisation à l’offre groupée qui en résulte. Par exemple, pour ajouter [.less](http://www.dotlesscss.org/) fichiers à votre projet MVC 4 :

1. Créez un dossier pour moins de votre contenu. L’exemple suivant utilise le *contenu\\MyLess* dossier.
2. Ajouter le [.less](http://www.dotlesscss.org/) package NuGet **sans point** à votre projet.  
    ![Installation de NuGet sans point](bundling-and-minification/_static/image9.png)
3. Ajouter une classe qui implémente le [IBundleTransform](https://msdn.microsoft.com/library/system.web.optimization.ibundletransform(VS.110).aspx) interface. Pour la transformation .less, ajoutez le code suivant à votre projet.

    [!code-csharp[Main](bundling-and-minification/samples/sample13.cs)]
4. Créer un groupe de fichiers LESS avec le `LessTransform` et [CssMinify](https://msdn.microsoft.com/library/system.web.optimization.cssminify(VS.110).aspx) transformer. Ajoutez le code suivant à la `RegisterBundles` méthode dans le *application\\_démarrer\\BundleConfig.cs* fichier.

    [!code-csharp[Main](bundling-and-minification/samples/sample14.cs)]
5. Ajoutez le code suivant à toutes les vues qui fait référence à l’offre moins.

    [!code-cshtml[Main](bundling-and-minification/samples/sample15.cshtml)]

## <a name="bundle-considerations"></a>Considérations d’offre groupée

Une bonne convention à suivre lors de la création d’offres groupées consiste à inclure « regrouper » en tant que préfixe dans le nom du bundle. Cela empêche un éventuel [conflit de routage](https://forums.asp.net/post/5012037.aspx).

Une fois que vous mettez à jour un fichier dans un groupe, un nouveau jeton est généré pour le paramètre de chaîne de requête de regroupement et la prochaine fois qu’un client demande une page contenant l’ensemble doit être téléchargée à l’offre complète. Dans le balisage traditionnel où chaque élément multimédia est répertorié individuellement, uniquement le fichier modifié serait être téléchargé. Actifs qui changent fréquemment ne peuvent pas être de bons candidats pour le regroupement.

Regroupement et minimisation principalement améliorent la première fois de charge de demande de page. Une fois qu’une page Web a été demandée, le navigateur met en cache les ressources (JavaScript, CSS et images) afin de regroupement et minimisation ne fournissent toute amélioration des performances lors de la demande de la même page, ou les pages sur le même site demande les mêmes ressources. Si vous ne définissez pas l’expiration en-tête correctement sur vos ressources et vous n’utilisez pas le regroupement et minimisation, les heuristiques de fraîcheur navigateurs marquera les ressources obsolètes après quelques jours et le navigateur requiert une demande de validation pour chaque élément multimédia. Dans ce cas, regroupement et minimisation fournissent une augmentation des performances après la première demande de page. Pour plus d’informations, consultez le blog [à l’aide de CDN et Expires pour améliorer les performances de Site Web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

La limitation de navigateur de six connexions simultanées par chaque nom d’hôte peut être atténuée en utilisant un [CDN](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx). Étant donné que le CDN doit avoir un nom d’hôte différents à votre site d’hébergement, les demandes de ressource à partir du CDN ne seront pas décomptés de votre environnement d’hébergement de la limite de connexions simultanées six. Un CDN peut également fournir une mise en cache du package courants et avantages de la mise en cache.

Offres groupées doivent être partitionnées par les pages qui en ont besoin. Par exemple, le modèle ASP.NET MVC pour une application internet par défaut crée un groupement de Validation jQuery distinct de jQuery. Étant donné que les affichages par défaut créés ne possèdent aucune entrée et ne validez pas les valeurs, elles n’incluent pas le groupe de validation.

Le `System.Web.Optimization` espace de noms est implémenté dans *System.Web.Optimization.dll*. Il tire parti de la bibliothèque WebGrease (*WebGrease.dll*) pour les fonctionnalités de la minimisation, qui à son tour utilise *Antlr3.Runtime.dll*.

*Utiliser Twitter pour effectuer des publications rapides et de partager des liens. Mon pseudo Twitter est*: [@RickAndMSFT](http://twitter.com/RickAndMSFT)

## <a name="additional-resources"></a>Ressources supplémentaires

- Vidéo :[regroupement et l’optimisation](https://channel9.msdn.com/Events/aspConf/aspConf/Bundling-and-Optimizing) par [Howard Dierking](https://twitter.com/#!/howard_dierking)
- [Ajout de l’optimisation du Web à un Site Web Pages](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx).
- [Ajout de regroupement et minimisation à Web Forms](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx).
- [Impact sur les performances de regroupement et minimisation sur l’exploration de Web](https://blogs.msdn.com/b/henrikn/archive/2012/06/17/performance-implications-of-bundling-and-minification-on-http.aspx) par [Henrik F Nielsen](http://en.wikipedia.org/wiki/Henrik_Frystyk_Nielsen) [@frystyk](https://twitter.com/frystyk)
- [À l’aide de CDN et expire pour améliorer les performances de Site Web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx) par Rick Anderson [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)
- [Réduire la durée des boucles (durées de boucles)](https://developers.google.com/speed/docs/best-practices/rtt)

## <a name="contributors"></a>Contributors

- Arts martiaux hao
- [Howard Dierking](https://twitter.com/#!/howard_dierking)
- Diana LaRose
