---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
title: Présentation des fonctionnalités de débogage d’ASP.NET AJAX | Microsoft Docs
author: scottcate
description: La possibilité de déboguer du code est une compétence tous les développeurs doivent être placés dans leur arsenal quelle que soit la technologie qu’ils utilisent. Bien que de nombreux développeurs sont en cours...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/28/2008
ms.topic: article
ms.assetid: 7f9380c6-19f7-4c82-a019-916ec6dffc9c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
msc.type: authoredcontent
ms.openlocfilehash: 533eb8d2faf735915fa5cf5044db09d0ab636938
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390970"
---
<a name="understanding-aspnet-ajax-debugging-capabilities"></a>Présentation des fonctionnalités de débogage d’ASP.NET AJAX
====================
par [Scott Cate](https://github.com/scottcate)

[Télécharger PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial06_Debugging_MS_Ajax_Applications_cs.pdf)

> La possibilité de déboguer du code est une compétence tous les développeurs doivent être placés dans leur arsenal quelle que soit la technologie qu’ils utilisent. Bien que de nombreux développeurs sont habitués à l’aide de Visual Studio .NET ou Web Developer Express pour déboguer des applications ASP.NET qui utilisent du code VB.NET ou c#, certaines ne sont pas conscients qu’il est également très utile pour déboguer le code côté client, tels que JavaScript. Le même type de techniques utilisées pour déboguer des applications .NET peut également être appliqué aux applications activées par AJAX et plus spécifiquement les applications ASP.NET AJAX.


## <a name="debugging-aspnet-ajax-applications"></a>Débogage des Applications ASP.NET AJAX

Dan Wahlin

La possibilité de déboguer du code est une compétence tous les développeurs doivent être placés dans leur arsenal quelle que soit la technologie qu’ils utilisent. Il va sans dire que le comprendre les différentes options de débogage sont disponibles peut enregistrer une quantité énorme de temps sur un projet et peut-être même quelques maux de tête. Bien que de nombreux développeurs sont habitués à l’aide de Visual Studio .NET ou Web Developer Express pour déboguer des applications ASP.NET qui utilisent du code VB.NET ou c#, certaines ne sont pas conscients qu’il est également très utile pour déboguer le code côté client, tels que JavaScript. Le même type de techniques utilisées pour déboguer des applications .NET peut également être appliqué aux applications activées par AJAX et plus spécifiquement les applications ASP.NET AJAX.

Dans cet article, vous verrez comment Visual Studio 2008 et plusieurs autres outils permettent de déboguer des applications ASP.NET AJAX pour localiser rapidement les bogues et autres problèmes. Cette discussion comporte des informations sur l’activation Internet Explorer 6 ou version ultérieure pour le débogage, à l’aide de Visual Studio 2008 et l’Explorateur de scripts pour parcourir le code ainsi à l’aide d’autres outils gratuits tels que de l’Assistant de développement Web. Vous allez également apprendre à déboguer des applications ASP.NET AJAX à l’aide de Firefox qu'une extension nommée Firebug qui vous permet de vous parcourez le code JavaScript directement dans le navigateur sans d’autres outils. Enfin, vous allez découvrir aux classes dans la bibliothèque ASP.NET AJAX qui peuvent aider à différentes tâches de débogage telles que le suivi et les instructions d’assertion de code.

Avant d’essayer de déboguer des pages consultées dans Internet Explorer, il existe quelques opérations élémentaires que vous devez effectuer pour l’activer pour le débogage. Jetons un œil à certaines exigences de configuration de base qui doivent être effectuées pour commencer.

## <a name="configuring-internet-explorer-for-debugging"></a>Configuration d’Internet Explorer pour le débogage

La plupart des gens ne souhaitez pas afficher les problèmes de JavaScript a rencontré sur un site Web affiché avec Internet Explorer. En fait, l’utilisateur moyen ne même saura pas quoi faire si elles vu un message d’erreur. Par conséquent, les options de débogage sont désactivées par défaut dans le navigateur. Toutefois, il est très simple activer le débogage et le placer à utiliser lorsque vous développez de nouvelles applications AJAX.

Pour activer la fonctionnalité de débogage, accédez à outils/Options Internet dans le menu d’Internet Explorer et sélectionnez l’onglet Avancé. Dans la section navigation vous assurer que les éléments suivants sont désactivés :

- Désactiver le débogage des scripts (Internet Explorer)
- Désactiver le script de débogage (autre)

Bien que non obligatoire, si vous tentez de déboguer une application que vous souhaiterez probablement des erreurs JavaScript dans la page soit immédiatement visible et évident. Vous pouvez forcer toutes les erreurs à afficher avec une boîte de message en cochant la case « Afficher une notification de chaque erreur de script ». Bien que cela soit une option intéressante pour allumer pendant que vous développez une application, il peut rapidement devenir ennuyeux si vous êtes simplement parcourant les autres sites Web dans la mesure où vos chances de rencontrer des erreurs JavaScript sont assez bonnes.

Figure 1 montre quels Internet Explorer avancé de boîte de dialogue doit ressembler après que qu’il a été configuré correctement pour le débogage.


[![Configuration d’Internet Explorer pour le débogage.](understanding-asp-net-ajax-debugging-capabilities/_static/image2.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image1.png)

**Figure 1**: configuration d’Internet Explorer pour le débogage.  ([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-debugging-capabilities/_static/image3.png))


Une fois que le débogage a été activé, vous verrez un nouvel élément de menu s’affichent dans le menu affichage nommé débogueur de Script. Il propose deux options disponibles, notamment les ouvrir et de saut à la prochaine instruction. Lors de l’ouverture est activée vous devrez faire pour déboguer la page dans Visual Studio 2008 (Notez que Visual Web Developer Express peut également être utilisée pour le débogage). Si Visual Studio .NET est en cours d’exécution, vous pouvez choisir d’utiliser cette instance ou pour créer une nouvelle instance. Lors de l’arrêt à l’instruction suivante est sélectionnée, vous allez invité pour déboguer la page lors de l’exécution de code JavaScript. Si le code JavaScript s’exécute dans l’événement onLoad de la page, vous pouvez actualiser la page pour déclencher une session de débogage. Si le code JavaScript est exécuté après qu’un clic est effectué le débogueur exécute immédiatement après un clic sur le bouton.

> *> [!NOTE] Si vous exécutez sur Windows Vista avec accès contrôle utilisateur (UAC) est activée, et que Visual Studio 2008 configuré pour s’exécuter en tant qu’administrateur, Visual Studio échoue à attacher au processus lorsque vous êtes invité à joindre. Pour contourner ce problème, démarrez Visual Studio tout d’abord et utiliser cette instance pour déboguer.*


Bien que la section suivante va vous montrer comment déboguer une page ASP.NET AJAX directement à partir de Visual Studio 2008, à l’aide de l’option du débogueur de Script Internet Explorer est utile quand une page est déjà ouverte et vous souhaitez examiner plus en détail.

## <a name="debugging-with-visual-studio-2008"></a>Débogage avec Visual Studio 2008

Visual Studio 2008 fournit des fonctionnalités de débogage aux développeurs du monde entier s’appuient sur tous les jours à déboguer des applications .NET. Le débogueur intégré permet de parcourir le code, vue de données d’objet, suivi des variables spécifiques, surveiller la pile des appels et plus encore. En plus du débogage de code VB.NET ou c#, le débogueur s’avère également utile pour le débogage des applications ASP.NET AJAX et vous permet de parcourir le code JavaScript ligne par ligne. Les détails qui suivent le focus sur les techniques qui peuvent être utilisées pour déboguer les fichiers de script côté client, plutôt que de fournir un discours sur le processus global de débogage des applications à l’aide de Visual Studio 2008.

Le processus de débogage d’une page dans Visual Studio 2008 peut être démarré de plusieurs façons différentes. Tout d’abord, vous pouvez utiliser l’option de débogueur de Script Internet Explorer mentionnée dans la section précédente. Cela fonctionne bien quand une page est déjà chargée dans le navigateur et vous souhaitez commencer à le déboguer. Vous pouvez également, avec le bouton droit sur une page .aspx dans l’Explorateur de solutions et sélectionnez Définir comme Page de démarrage dans le menu. Si vous avez l’habitude de débogage des pages ASP.NET puis vous avez probablement fait auparavant. Une fois que vous appuyez sur F5, la page peut être déboguée. Toutefois, alors que vous pouvez généralement définir un point d’arrêt n’importe où vous souhaitez que dans le code VB.NET ou c#, qui n’est pas toujours le cas avec JavaScript comme vous verrez ensuite.

*Embedded par rapport à des Scripts externes*

Le débogueur de Visual Studio 2008 traite JavaScript incorporé dans une page différente de celle des fichiers JavaScript externes. Des fichiers de script externe, vous pouvez ouvrir le fichier et définir un point d’arrêt sur n’importe quelle ligne que vous choisissez. Points d’arrêt peuvent être définis en cliquant dans la zone de barre d’état grise à gauche de la fenêtre d’éditeur de code. Lorsque JavaScript est directement incorporé dans une page à l’aide de la `<script>` balise, en définissant un point d’arrêt en cliquant dans la zone grise de barre d’état n’est pas une option. Tente de définir un point d’arrêt sur une ligne de script incorporé entraîne un avertissement indiquant « Ce n’est pas un emplacement valide pour un point d’arrêt ».

Vous pouvez contourner ce problème en déplaçant le code dans un fichier .js externe et en référençant à l’aide de l’attribut src de le &lt;script&gt; balise :


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample1.html)]

Que se passe-t-il si vous déplacez le code dans un fichier externe n’est pas une option ou nécessite plus de travail qu’il ne vaut ? Tandis que vous ne pouvez pas définir un point d’arrêt à l’aide de l’éditeur, vous pouvez ajouter l’instruction du débogueur directement dans le code où vous souhaitez démarrer le débogage. Vous pouvez également utiliser la classe Sys.Debug disponible dans la bibliothèque ASP.NET AJAX pour forcer le démarrage du débogage. Vous en apprendrez davantage sur la classe Sys.Debug plus loin dans cet article.

Un exemple d’utilisation le `debugger` mot clé est indiqué dans le Listing 1. Cet exemple oblige le débogueur s’arrête droite avant un appel à une fonction de mise à jour est effectué.

**Listing 1. À l’aide du mot clé de débogueur pour forcer l’arrêt du débogueur Visual Studio .NET.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample2.js)]

Une fois que l’instruction du débogueur est atteint, vous serez invité à déboguer la page à l’aide de Visual Studio .NET et pouvez commencer à parcourir le code. Bien que fait ce que vous pouvez rencontrer un problème avec l’accès aux fichiers de script ASP.NET AJAX library utilisés dans la page, nous allons donc prendre un coup de œil à l’aide de Visual Studio. Explorateur de scripts de NET.

## <a name="using-visual-studio-net-windows-to-debug"></a>Pour déboguer à l’aide de Visual Studio .NET Windows

Une fois qu’une session de débogage est démarrée et commencer à parcourir le code à l’aide de la touche F11 par défaut, vous pouvez rencontrer l’erreur boîte de dialogue de voir la Figure 2, sauf si tous les fichiers de script utilisés dans la page sont ouverts et disponibles pour le débogage.


[![Boîte de dialogue erreur affiché lorsque aucun code source n’est disponible pour le débogage.](understanding-asp-net-ajax-debugging-capabilities/_static/image5.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image4.png)

**Figure 2**: boîte de dialogue erreur affiché lorsque aucun code source n’est disponible pour le débogage.  ([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-debugging-capabilities/_static/image6.png))


Cette boîte de dialogue est affichée, car Visual Studio .NET ne sait pas comment obtenir le code source de certains scripts référencés par la page. Bien que cela peut s’avérer très contrariant dans un premier temps, il existe un correctif simple. Une fois que vous avez démarré une session de débogage et atteindre un point d’arrêt, accédez à la fenêtre Explorateur de scripts Windows déboguer dans le menu de Visual Studio 2008 ou utilisez le raccourci clavier Ctrl + Alt + N.

> *> [!NOTE] Si vous ne voyez pas le menu Explorateur de scripts répertorié, accédez à outils* *personnaliser* *commandes du menu de Visual Studio .NET. Recherchez l’entrée de débogage dans la section de catégories et cliquez dessus pour afficher toutes les entrées de menu disponibles. Dans la liste de commandes, faites défiler jusqu'à l’Explorateur de scripts et puis la faire glisser sur le débogage* *menu Windows mentionné précédemment. Cela rend l’entrée de menu Explorateur de scripts disponibles chaque fois que vous exécutez Visual Studio .NET.*


L’Explorateur de scripts peut être utilisé pour afficher tous les scripts utilisés dans une page et de les ouvrir dans l’éditeur de code. Une fois que l’Explorateur de scripts est ouvert, double-cliquez sur la page .aspx en cours de débogage pour l’ouvrir dans la fenêtre d’éditeur de code. Effectuer la même action pour tous les autres scripts indiqués dans l’Explorateur de scripts. Une fois que tous les scripts sont ouverts dans la fenêtre de code que vous pouvez appuyez sur F11 (et utilisez les autres touches de raccourci de débogage) pour parcourir votre code. Figure 3 montre un exemple de l’Explorateur de scripts. Il répertorie le fichier actuel en cours de débogage (Demo.aspx), ainsi que deux scripts personnalisés et les deux scripts injectés de façon dynamique dans la page par ScriptManager ASP.NET AJAX.


[![L’Explorateur de scripts fournit un accès facile aux scripts utilisés dans une page.](understanding-asp-net-ajax-debugging-capabilities/_static/image8.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image7.png)

**Figure 3**. L’Explorateur de scripts fournit un accès facile aux scripts utilisés dans une page.  ([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-debugging-capabilities/_static/image9.png))


Plusieurs autres utilisateurs windows peuvent également être utilisés pour fournir des informations utiles à mesure que vous parcourez le code dans une page. Par exemple, vous pouvez utiliser la fenêtre variables locales pour afficher les valeurs des différentes variables utilisées dans la page, la fenêtre exécution pour évaluer des variables spécifiques ou des conditions et d’afficher la sortie. Vous pouvez également utiliser la fenêtre Sortie pour afficher les instructions de trace écrites à l’aide de la fonction Sys.Debug.trace (qui sera abordée plus loin dans cet article) ou Debug.writeln, fonction de Internet Explorer.

Mesure que vous parcourez le code à l’aide du débogueur vous pouvez placez la souris sur les variables dans le code pour afficher la valeur auxquelles ils sont affectés. Toutefois, le débogueur de script occasionnellement ne rien affiche comme vous placez la souris sur une variable JavaScript donnée. Pour afficher la valeur, mettez en surbrillance de l’instruction ou la variable que vous tentez d’afficher dans la fenêtre d’éditeur de code et puis déplacez la souris dessus. Bien que cette technique ne fonctionne pas dans tous les cas, nombre de fois où vous serez en mesure de voir la valeur sans avoir à rechercher dans une fenêtre de débogage différents tels que la fenêtre variables locales.

Vous pouvez consulter un didacticiel vidéo décrivant certaines des fonctionnalités abordées ici sur [ http://www.xmlforasp.net ](http://www.xmlforasp.net).

## <a name="debugging-with-web-development-helper"></a>Débogage avec l’Assistant de développement Web

Bien que Visual Studio 2008 et Visual Web Developer Express 2008 sont très efficaces, outils de débogage, il existe des options supplémentaires qui peuvent également être utilisées sont plus léger. Parmi les derniers outils pour être publié est l’application d’assistance du développement Web. Nikhil Kothari de Microsoft (soit les architectes d’ASP.NET AJAX clés chez Microsoft) a écrit cet excellent outil qui permet d’effectuer différentes tâches de débogage simple pour afficher les messages de demande et de réponse HTTP. Assistant de développement Web peut être téléchargée sur [ http://projects.nikhilk.net/Projects/WebDevHelper.aspx ](http://projects.nikhilk.net/Projects/WebDevHelper.aspx).

Assistant de développement Web peut être utilisé directement à l’intérieur d’Internet Explorer, ce qui le rend pratique à utiliser. Il est démarré en sélectionnant Outils Assistant de développement Web dans le menu d’Internet Explorer. L’outil s’ouvre dans la partie inférieure du navigateur qui est intéressant, car vous n’êtes pas obligé de laisser le navigateur pour effectuer plusieurs tâches telles que la journalisation de message de demande et de réponse HTTP. Figure 4 montre à quoi ressemble Assistant de développement Web en action.


[![Assistant de développement Web](understanding-asp-net-ajax-debugging-capabilities/_static/image11.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image10.png)

**Figure 4**: Assistant de développement Web ([cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-debugging-capabilities/_static/image12.png))


Assistant de développement Web n’est pas un outil que vous allez utiliser pour parcourir le code ligne par ligne comme avec Visual Studio 2008. Toutefois, il peut être utilisé pour afficher la sortie de trace, facilement évaluer les variables dans un script ou Explorer les données sont à l’intérieur d’un objet JSON. Il est également très utile pour la consultation des données qui sont passées vers et depuis une page ASP.NET AJAX et un serveur.

Une fois l’Assistant de développement Web est ouverte dans Internet Explorer, le débogage de script doit être activé en sélectionnant Script activer le débogage de Script dans le menu d’assistance de développement Web, comme indiqué plus haut dans la Figure 4. Cela permet à l’outil pour intercepter les erreurs qui se produisent lors de l’exécution d’une page. Il permet également un accès facile aux messages de trace sont affichés dans la page. Pour afficher les informations de trace ou exécuter des commandes de script pour tester différentes fonctions au sein d’une page, sélectionnez le Script afficher la Console Script dans le menu de l’Assistant de développement Web. Fournit l’accès à une fenêtre de commande et une simple fenêtre exécution.

*Affichage des Messages de Trace et les données d’objet JSON*

La fenêtre exécution peut être utilisée pour exécuter des commandes de script ou même charger ou enregistrer les scripts qui sont utilisés pour tester différentes fonctions dans une page. La fenêtre de commande affiche les messages de trace ou debug écrits à la page affichée. Listing 2 montre comment écrire un message de trace à l’aide de Debug.writeln, fonction de Internet Explorer.

**Listing 2. Écriture d’un message de trace côté client à l’aide de la classe Debug.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample3.js)]

Si la propriété LastName contient une valeur de Dupont, Assistant de développement Web affichera le message « nom de la personne : Doe » dans la fenêtre de commande de la console de script (en supposant que le débogage a été activé). Assistant de développement Web ajoute également un objet de niveau supérieur debugService dans des pages qui peuvent être utilisées pour écrire des informations de trace ou afficher le contenu d’objets JSON. Liste 3 montre un exemple d’utilisation de fonction de trace de la classe debugService.

**Liste 3. À l’aide de classe debugService de l’Assistant de développement Web pour écrire un message de trace.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample4.js)]

Une fonctionnalité intéressante de la classe debugService est qu’elle fonctionnera même si le débogage n’est pas activé dans Internet Explorer, ce qui facilite pour toujours accéder aux données de trace lors de l’Assistant de développement Web est en cours d’exécution. Quand l’outil n’est pas utilisé pour déboguer une page, les instructions de trace seront ignorées, car l’appel à window.debugService renvoie la valeur false.

La classe debugService permet également des données d’objet JSON à être affiché à l’aide de la fenêtre de l’inspecteur de l’assistance de développement Web. Liste 4 crée un objet JSON simple contenant les données person. Une fois que l’objet est créé, un appel est effectué à le debugService la classe inspecter (fonction) pour permettre à l’objet JSON à inspecter visuellement.

**Liste 4. À l’aide de la fonction debugService.inspect pour afficher les données d’objet JSON.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample5.js)]

Dans la page ou via la fenêtre exécution, l’appel de la fonction GetPerson() entraîne dans la fenêtre de boîte de dialogue Inspecteur de l’objet s’affiche comme indiqué dans la Figure 5. Propriétés au sein de l’objet peuvent être modifiées dynamiquement en mettant en surbrillance, puis en cliquant sur le lien de mise à jour et modifier la valeur indiquée dans la zone de texte de valeur. À l’aide de l’inspecteur de l’objet facilite son afficher les données d’objet JSON et faire des essais avec différentes valeurs s’applique aux propriétés.

*Erreurs de débogage*

En plus de permettre des données de trace et les objets JSON à afficher, d’aide au développement du Web peut également contribuer à déboguer les erreurs dans une page. Si une erreur s’est produite, vous devrez continuer à la ligne suivante du code ou de déboguer le script (voir Figure 6). La fenêtre de boîte de dialogue Erreur de Script affiche de pile des appels complète, ainsi que les numéros de ligne pour vous pouvez d’identifier facilement où sont des problèmes au sein d’un script.


[![À l’aide de la fenêtre d’inspecteur de l’objet pour afficher un objet JSON.](understanding-asp-net-ajax-debugging-capabilities/_static/image14.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image13.png)

**Figure 5**: à l’aide de la fenêtre d’inspecteur de l’objet pour afficher un objet JSON.  ([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-debugging-capabilities/_static/image15.png))


En sélectionnant l’option de débogage vous permet d’exécuter des instructions de script directement dans la fenêtre exécution de l’Assistant de développement Web pour afficher la valeur des variables, écrire des objets JSON, et plus encore. Si l’action qui a déclenché l’erreur est effectuée à nouveau et Visual Studio 2008 est disponible sur l’ordinateur, vous devrez démarrer une session de débogage afin que vous pouvez parcourir le code ligne par ligne, comme indiqué dans la section précédente.


[![Boîte de dialogue Erreur d’aide au développement Script de Web](understanding-asp-net-ajax-debugging-capabilities/_static/image17.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image16.png)

**Figure 6**: boîte de dialogue Erreur de Script de l’assistance de développement Web ([cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-debugging-capabilities/_static/image18.png))


*Inspection des Messages de demande et réponse*

Pendant le débogage des pages ASP.NET AJAX, il est souvent utile de consulter les messages de demande et de réponse envoyés entre une page et le serveur. Affichant le contenu dans les messages vous permet de voir si les données appropriées sont passées, ainsi que la taille des messages. Assistant de développement Web fournit une fonctionnalité de journal de message HTTP excellente qui facilite l’afficher les données sous forme de texte brut ou dans un format plus lisible.

Pour afficher les messages de demande et de réponse ASP.NET AJAX, l’enregistreur d’événements HTTP doit être activé en sélectionnant Activer HTTP journalisation HTTP à partir du menu de l’Assistant de développement Web. Une fois activé, tous les messages envoyés à partir de la page actuelle sont consultables dans la visionneuse du journal HTTP qui est accessible en sélectionnant HTTP afficher les journaux HTTP.

Bien que l’affichage du texte brut envoyé dans chaque message de demande/réponse est certainement utile (et une option dans l’Assistant de développement Web), il est souvent plus facile à afficher les données de message dans un format plus graphique. Une fois que la journalisation HTTP a été activée et que des messages ont été consignés, les données de message peuvent être affichées en double-cliquant sur le message dans la visionneuse du journal HTTP. Cela vous permet d’afficher tous les en-têtes associés à un message, ainsi que le message réel contenu. Figure 7 montre un exemple d’un message de demande et le message de réponse affiché dans la fenêtre de la visionneuse du journal HTTP.


[![À l’aide de la visionneuse du journal HTTP pour afficher les données de message de demande et de réponse.](understanding-asp-net-ajax-debugging-capabilities/_static/image20.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image19.png)

**Figure 7**: à l’aide de la visionneuse du journal HTTP pour afficher les données de message de demande et de réponse.  ([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-debugging-capabilities/_static/image21.png))


La visionneuse du journal HTTP automatiquement analyse des objets JSON et les affiche à l’aide d’une arborescence rend rapide et facile afficher les données de propriété de l’objet. Lorsqu’un UpdatePanel est utilisé dans une page ASP.NET AJAX, la visionneuse s’arrête chaque partie du message dans les parties individuelles comme indiqué dans la Figure 8. Il s’agit d’une fonctionnalité intéressante qui rend beaucoup plus facile de voir et comprendre ce qui est dans le message par rapport à l’affichage des données de message brut.


[![Un message de réponse UpdatePanel affiché à l’aide de la visionneuse du journal HTTP.](understanding-asp-net-ajax-debugging-capabilities/_static/image23.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image22.png)

**Figure 8**: message de réponse d’un UpdatePanel affichés à l’aide de la visionneuse du journal HTTP.  ([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-debugging-capabilities/_static/image24.png))


Il existe plusieurs autres outils qui peuvent être utilisées pour afficher les messages de demande et de réponse en plus de l’Assistant de développement Web. Une autre solution intéressante est Fiddler qui est disponible gratuitement à [ http://www.fiddlertool.com ](http://www.fiddlertool.com). Bien que Fiddler n’est pas abordé ici, il est également une bonne option lorsque vous devez soigneusement examiner les données et les en-têtes de message.

## <a name="debugging-with-firefox-and-firebug"></a>Débogage avec Firebug et Firefox

Si Internet Explorer est toujours le navigateur plus largement utilisé, autres navigateurs, tels que Firefox sont devenus très populaires et sont plus utilisés. Par conséquent, vous souhaiterez afficher et déboguer vos pages ASP.NET AJAX dans Firefox, ainsi que Internet Explorer pour vous assurer que vos applications fonctionnent correctement. Bien que Firefox ne peut pas lier directement dans Visual Studio 2008 pour le débogage, il a une extension appelée Firebug qui peut être utilisé pour déboguer des pages. Firebug est téléchargeable gratuitement en accédant à [ http://www.getfirebug.com ](http://www.getfirebug.com).

Firebug fournit un environnement de débogage complète qui peut être utilisé pour parcourir le code ligne par ligne, accéder à tous les scripts utilisés dans une page, afficher des structures de DOM, afficher les styles CSS et mêmes suivre les événements qui se produisent dans une page. Une fois installé, Firebug est accessible en sélectionnant Outils Firebug Open Firebug dans le menu de Firefox. Comme Assistant de développement Web, Firebug est utilisé directement dans le navigateur, bien qu’il peut également être utilisé comme une application autonome.

Une fois que Firebug est en cours d’exécution, les points d’arrêt peuvent être définies sur n’importe quelle ligne d’un fichier JavaScript si le script est incorporé dans une page ou non. Pour définir un point d’arrêt, d’abord charger la page appropriée que vous souhaitez déboguer dans Firefox. Une fois que la page est chargée, sélectionnez le script à déboguer à partir de la liste déroulante de Scripts de Firebug. Tous les scripts utilisés par la page seront affichera. Un point d’arrêt est défini en cliquant dans la zone de barre d’état grises de Firebug sur la ligne où le point d’arrêt doit se doit, comme vous le feriez dans Visual Studio 2008.

Une fois qu’un point d’arrêt a été défini dans le Firebug, vous pouvez effectuer l’action requise pour exécuter le script qui doit être déboguée comme en cliquant sur un bouton ou l’actualisation du navigateur pour déclencher l’événement onLoad. L’exécution s’arrête automatiquement sur la ligne contenant le point d’arrêt. Figure 9 illustre un exemple d’un point d’arrêt a été déclenché dans le Firebug.


[![Gestion des points d’arrêt dans le Firebug.](understanding-asp-net-ajax-debugging-capabilities/_static/image26.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image25.png)

**Figure 9**: gestion des points d’arrêt dans le Firebug.  ([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-debugging-capabilities/_static/image27.png))


Une fois qu’un point d’arrêt est atteint pas à pas détaillé, pas à pas principal ou sortir le code en utilisant les boutons fléchés. À mesure que vous parcourez le code, les variables de script sont affichées dans la partie droite du débogueur, ce qui vous permet de voir les valeurs et l’exploration des objets. Firebug inclut également une liste déroulante de pile des appels pour afficher les étapes de l’exécution du script qui a conduit à la ligne actuelle en cours de débogage.

Firebug inclut également une fenêtre de console qui peut être utilisée pour tester les instructions de script différents, évaluer les variables et afficher la sortie de trace. Il est accessible en cliquant sur l’onglet de la Console en haut de la fenêtre Firebug. La page en cours de débogage peut également être « analysée » pour afficher sa structure DOM et son contenu en cliquant sur l’onglet inspecter. En tant que vous pointez la souris sur les différents éléments DOM indiqué dans la fenêtre Inspecteur la partie appropriée de la page est mise en surbrillance, ce qui la rend facile de voir où l’élément est utilisé dans la page. Valeurs d’attribut associés à un élément donné peuvent être modifiés « live » pour faire des essais avec différentes largeurs, styles, etc. s’applique à un élément. Il s’agit d’une fonctionnalité très utile qui vous évite d’avoir à basculer constamment entre l’éditeur de code source et le navigateur Firefox pour afficher la simplicité modifications affectent une page.

Figure 10 illustre un exemple d’utilisation de l’inspecteur de DOM pour localiser une zone de texte nommée txtCountry dans la page. L’inspecteur Firebug peut également servir à afficher les styles CSS utilisés dans une page, ainsi que les événements qui se produisent telles que le suivi des mouvements de souris, les clics de bouton, ainsi que bien plus encore.


[![Utilisation de l’inspecteur de DOM de Firebug.](understanding-asp-net-ajax-debugging-capabilities/_static/image29.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image28.png)

**Figure 10**: inspecteur de DOM de Firebug à l’aide.  ([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-debugging-capabilities/_static/image30.png))


Firebug fournit un moyen léger pour déboguer rapidement une page directement dans Firefox, mais aussi un excellent outil pour l’inspection des différents éléments dans la page.

## <a name="debugging-support-in-aspnet-ajax"></a>Prise en charge du débogage dans ASP.NET AJAX

La bibliothèque ASP.NET AJAX inclut de nombreuses classes différentes qui peuvent être utilisées pour simplifier le processus d’ajout de fonctionnalités AJAX dans une page Web. Vous pouvez utiliser ces classes pour localiser des éléments dans une page et les manipuler, ajouter de nouveaux contrôles, appeler des Services Web et gérer des événements. La bibliothèque ASP.NET AJAX contient également des classes qui peuvent être utilisées pour améliorer le processus de débogage des pages. Dans cette section, vous allez découvrir la classe Sys.Debug et voir comment il peut être utilisé dans les applications.

*À l’aide de la classe Sys.Debug*

La classe de Sys.Debug (une classe JavaScript située dans l’espace de noms Sys) peut être utilisée pour effectuer plusieurs fonctions différentes, y compris l’écriture de la sortie de trace, l’exécution des assertions de code et forcer le code d’échec afin qu’il peut être débogué. Il est largement utilisé dans les fichiers de débogage de la bibliothèque ASP.NET AJAX (installés par défaut dans C:\Program Files\Microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0) pour effectuer des tests conditionnels) appelé assertions) qui vous assurer de paramètres sont correctement transmis aux fonctions, que les objets contiennent les données attendues et d’écrire des instructions de trace.

La classe Sys.Debug expose plusieurs fonctions qui peuvent être utilisées pour gérer le suivi, les assertions de code ou les échecs, comme indiqué dans le tableau 1.

**Tableau 1. Fonctions de classe Sys.Debug.**

| **Nom de la fonction** | **Description** |
| --- | --- |
| Assert (condition, message, displayCaller) | Déclare que le paramètre de condition est true. Si la condition testée a la valeur false, une boîte de message doit être utilisée pour afficher la valeur de paramètre de message. Si le paramètre displayCaller est true, la méthode affiche également des informations sur l’appelant. |
| clearTrace() | Efface la sortie des instructions à partir d’opérations de suivi. |
| Fail(message) | Le programme s’arrête l’exécution et entre dans le débogueur. Le paramètre de message peut être utilisé pour fournir une raison de l’échec. |
| trace(message) | Écrit le paramètre de message dans la sortie de trace. |
| traceDump (objet, nom) | Génère des données d’un objet dans un format lisible. Le paramètre de nom peut être utilisé pour fournir une étiquette pour le vidage de la trace. Les sous-objets au sein de l’objet en cours vidées seront écrite par défaut. |

Le suivi côté client peut être utilisé dans la même façon que la fonctionnalité de suivi disponible dans ASP.NET. Il permet différents messages destinés à être vus facilement sans interrompre le flux de l’application. Liste 5 illustre un exemple d’utilisation de la fonction Sys.Debug.trace pour écrire dans le journal des traces. Cette fonction prend simplement le message qui doit être écrite sous la forme d’un paramètre.

**Liste 5. À l’aide de la fonction Sys.Debug.trace.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample6.js)]

Si vous exécutez le code indiqué dans la liste 5, vous ne voyez aucune sortie de trace dans la page. La seule façon de voir consiste à utiliser une fenêtre de console disponible dans Visual Studio .NET, Assistant de développement Web ou Firebug. Si vous ne souhaitez pas voir la sortie de trace dans la page puis vous devrez ajouter une balise TextArea et attribuez-lui un id de TraceConsole comme indiqué ci-après :


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample7.html)]

Toutes les instructions dans la page Sys.Debug.trace écraseront les TraceConsole TextArea.

Dans les cas où vous souhaitez afficher les données contenues dans un objet JSON, vous pouvez utiliser la Sys.Debug fonction la classe traceDump. Cette fonction accepte deux paramètres, y compris l’objet qui doit être vidée à la console de trace et un nom qui peut être utilisé pour identifier l’objet dans la sortie de trace. Liste 6 montre un exemple d’utilisation de la fonction traceDump.

**Liste 6. À l’aide de la fonction Sys.Debug.traceDump.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample8.js)]

Figure 11 illustre la sortie de l’appel de la fonction Sys.Debug.traceDump. Notez qu’en plus de l’écriture de données de l’objet personne, elle écrit également out adresse données de l’objet-sub.

Outre le suivi, la classe Sys.Debug peut également être utilisée pour effectuer des assertions de code. Assertions sont utilisées pour tester que certaines conditions sont remplies pendant l’exécution d’une application. La version de débogage des scripts de bibliothèque ASP.NET AJAX contient plusieurs instructions pour tester diverses conditions d’assertion.

Liste 7 montre un exemple d’utilisation de la fonction Sys.Debug.assert pour tester une condition. Le code teste si l’objet d’adresse est null avant la mise à jour d’un objet Person ou non.


[![Sortie de la fonction Sys.Debug.traceDump.](understanding-asp-net-ajax-debugging-capabilities/_static/image32.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image31.png)

**Figure 11**: sortie de la fonction Sys.Debug.traceDump.  ([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-debugging-capabilities/_static/image33.png))


**Liste 7. À l’aide de la fonction debug.assert.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample9.js)]

Trois paramètres sont passés, y compris la condition à évaluer, le message à afficher si l’assertion retourne false et ou non plus d’informations sur l’appelant doivent être affichées. Dans les cas où une assertion échoue, le message s’affichera, ainsi que des informations sur l’appelant si le troisième paramètre est true. Figure 12 montre un exemple de la boîte de dialogue d’échec qui s’affiche si l’assertion illustrée dans la liste 7 échoue.

La fonction finale pour couvrir est Sys.Debug.fail. Lorsque vous souhaitez forcer le code à échouer sur une ligne particulière dans un script, vous pouvez ajouter un appel Sys.Debug.fail plutôt que l’instruction du débogueur généralement utilisées dans les applications JavaScript. La fonction Sys.Debug.fail accepte un paramètre de chaîne unique qui représente la raison de l’échec, comme indiqué ci-après :


[!code-css[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample10.css)]


[![Un message d’échec Sys.Debug.assert.](understanding-asp-net-ajax-debugging-capabilities/_static/image35.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image34.png)

**Figure 12**: Sys.Debug.assert d’un message d’échec.  ([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-debugging-capabilities/_static/image36.png))


Lorsqu’une instruction Sys.Debug.fail est rencontrée pendant l’exécution d’un script, la valeur du paramètre de message s’affichera dans la console d’une application de débogage telles que Visual Studio 2008 et vous êtes invité à déboguer l’application. Un cas où cela peut être très utile est lorsque vous ne pouvez pas définir un point d’arrêt avec Visual Studio 2008 sur un script inline mais que vous souhaitez que le code d’arrêt sur une ligne particulière pour que vous puissiez vérifier la valeur des variables.

*Présentation ScriptMode propriété du contrôle ScriptManager*

La bibliothèque ASP.NET AJAX inclut debug et release de versions de script qui sont installées dans C:\Program Files\Microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 par défaut. Les scripts de débogage sont bien formaté, facile à lire et avaient plusieurs appels Sys.Debug.assert façon dispersée à travers les tandis que les scripts de mise en production ont des espaces blancs sont supprimés et utilisent la classe Sys.Debug avec parcimonie afin de réduire leur taille globale.

Le contrôle ScriptManager ajouté aux pages ASP.NET AJAX lit l’attribut de débogage de l’élément de compilation dans le fichier web.config pour déterminer les versions des scripts de bibliothèque à charger. Toutefois, vous pouvez contrôler si debug ou release des scripts sont chargés (scripts de bibliothèque ou vos propres scripts personnalisés en modifiant la propriété ScriptMode). ScriptMode accepte une énumération ScriptMode dont les membres incluent automatiquement, Debug, Release et hériter.

ScriptMode par défaut est la valeur Auto, ce qui signifie que le ScriptManager vérifie l’attribut de débogage dans le fichier web.config. Lorsque le débogage a la valeur false ScriptManager chargera la version release de scripts de bibliothèque ASP.NET AJAX. Lorsque le débogage a la valeur true la version de débogage des scripts sera chargée. Modification de la propriété ScriptMode pour libérer ou déboguer force le ScriptManager pour charger les scripts appropriés, quel que soit la valeur que l’attribut de débogage a dans le fichier web.config. Liste 8 montre un exemple d’utilisation du contrôle ScriptManager pour charger des scripts de débogage à partir de la bibliothèque ASP.NET AJAX.

**Liste 8. Chargement des scripts de débogage à l’aide de ScriptManager**.


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample11.aspx)]

Vous pouvez également charger des versions différentes (debug ou release) de vos propres scripts personnalisés à l’aide de la propriété de ScriptManager Scripts, ainsi que le composant de ScriptReference comme indiqué dans la liste 9.

**Liste 9. Chargement des scripts personnalisés à l’aide de ScriptManager.**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample12.aspx)]

> [!NOTE]
> Si vous chargez des scripts personnalisés à l’aide du composant de ScriptReference, vous devez informer le ScriptManager lorsque le script a terminé le chargement en ajoutant le code suivant en bas du script :


[!code-csharp[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample13.cs)]

Le code indiqué dans la liste 9 indique le ScriptManager pour rechercher une version de débogage du script personne afin qu’il recherchera automatiquement Person.debug.js au lieu de Person.js. Si le fichier Person.debug.js ne trouve pas qu'une erreur sera déclenchée.

Dans le cas où vous souhaitez un débogage ou de version d’un script personnalisé à charger en fonction de la valeur de la propriété ScriptMode sur le contrôle ScriptManager, vous pouvez définir la ScriptReference propriété du contrôle ScriptMode hériter. Ainsi, la version correcte du script personnalisé à charger en fonction de la propriété de ScriptManager ScriptMode comme indiqué dans la liste des 10. Étant donné que la propriété ScriptMode du contrôle ScriptManager est définie sur Debug, le script Person.debug.js est chargé et utilisé dans la page.

**Liste 10. Hériter le ScriptMode le ScriptManager pour les scripts personnalisés.**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample14.aspx)]

À l’aide de la propriété ScriptMode correctement, vous pouvez plus facilement déboguer des applications et simplifier le processus global. Les scripts de version de la bibliothèque ASP.NET AJAX sont difficiles à parcourir et lire dans la mesure où la mise en forme de code a été supprimée alors que les scripts de débogage sont mis en forme spécifiquement pour le débogage.

## <a name="conclusion"></a>Conclusion

Technologie de Microsoft ASP.NET AJAX fournit une base solide pour créer des applications compatibles AJAX qui peuvent améliorer l’expérience globale de l’utilisateur final. Toutefois, comme avec toute technologie de programmation, bogues et autres problèmes d’application seront certainement surviennent. Connaître les différentes options de débogage disponibles gagnerez beaucoup de temps et le résultat dans un produit plus stable.

Dans cet article vous avez été présenté à plusieurs techniques pour le débogage des pages ASP.NET AJAX, y compris Internet Explorer avec Visual Studio 2008, Assistant de développement Web et Firebug. Ces outils peuvent simplifier le processus de débogage global dans la mesure où vous pouvez accéder aux données de variable, parcourir le code ligne par ligne et afficher les instructions de trace. Outre les outils de débogage différents abordés, vous avez également vu comment la classe de Sys.Debug de la bibliothèque ASP.NET AJAX peut être utilisée dans une application et comment la classe de ScriptManager peut être utilisée pour charger debug ou release des versions de scripts.

## <a name="bio"></a>Biographie

Dan Wahlin (Microsoft Most Valuable Professional pour ASP.NET et des Services Web XML) est développement instructor et architecture consultant .NET à la formation technique d’Interface ([www.interfacett.com)](http://www.interfacett.com). Dan fondé le code XML pour le site Web des développeurs ASP.NET ([www.XMLforASP.NET](http://www.XMLforASP.NET)), se trouve sur le Bureau de l’INETA et participe à des conférences plusieurs. Dan co-écrit Professional Windows DNA (Wrox), ASP.NET : conseils, didacticiels et Code (SAM), des Solutions ASP.NET 1.1 Insider, Professional ASP.NET 2.0 AJAX (Wrox), ASP.NET 2.0 MVP Hacks et XML créé pour les développeurs ASP.NET (SAM). Lorsqu’il n’écrit pas de code, des articles ou des livres, Dan bénéficie d’écriture et d’enregistrement de musique et de lecture de golf et basket-ball avec sa femme et les enfants.

Scott Cate travaille avec les technologies Web Microsoft depuis 1997 et est le président de myKB.com ([www.myKB.com](http://www.myKB.com)) où il est spécialisé dans l’écriture d’ASP.NET en fonction des applications axées sur les solutions logicielles de la Base de connaissances. Scott peut être contacté par courrier électronique en [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) ou son blog à l’adresse [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Précédent](understanding-asp-net-ajax-web-services.md)
