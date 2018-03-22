---
title: "À l’aide de Bower dans ASP.NET Core"
author: rick-anderson
description: "Gestion des packages Bower côté client."
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bower
ms.openlocfilehash: 67695843846cfaf1619db11a7bffcc65802e0f69
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/02/2018
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a>Gérer les dépendances côté client avec Bower dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel riz](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), et [Scott Addie](https://scottaddie.com) 

> [!IMPORTANT]
> Tandis que Bower est conservé, son chargés de maintenance est recommandé d’utiliser une autre solution. Fil avec Webpack est une solution courante pour laquelle [instructions de migration](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) sont disponibles.

[Bower](https://bower.io/) se définit lui-même comme "Un gestionnaire de paquets pour le web". Dans l’écosystème .NET, il remplit le vide laissé par l’impossibilité d'inclure des fichiers de contenu statique avec NuGet. Pour les projets ASP.NET Core, ces fichiers statiques sont inhérents aux bibliothèques côté client comme [jQuery](http://jquery.com/) et [Bootstrap](http://getbootstrap.com/). Pour les bibliothèques .NET, vous utilisez toujours le gestionnaire de package [NuGet](https://www.nuget.org/).

Les nouveaux projets créés avec les modèles de projet ASP.NET Core sont configurés avec la génération côté client. [jQuery](http://jquery.com/) et [Bootstrap](http://getbootstrap.com/) sont installés, et Bower est pris en charge.

Les paquets côté client sont répertoriés dans le fichier *bower.json*. Les modèles de projet ASP.NET Core configure *bower.json* avec jQuery, jQuery validation et d’amorçage.

Dans ce didacticiel, nous allons ajouter la prise en charge de [Font Awesome](http://fontawesome.io). Les paquets Bower peuvent être installés avec l'interface graphique **Gérer les paquets Bower** ou manuellement dans le fichier *bower.json*.

### <a name="installation-via-manage-bower-packages-ui"></a>Installation via gérer les Packages Bower l’interface utilisateur

* Créer une application ASP.NET Core Web avec le modèle **Application Web ASP.NET Core (.NET Core)**. Sélectionnez **Application Web** et **aucune authentification**.

* Cliquez sur le projet dans l’Explorateur de solutions et sélectionnez **Gérer les paquets Bower** (également dans le menu principal, **projet** > **Gérer les paquets Bower**).

* Dans la fenêtre **Bower : \<nom du projet\>**, cliquez sur l’onglet "Parcourir", puis filtrer la liste des paquets en entrant `font-awesome` dans la zone de recherche :

 ![gérer les packages bower](bower/_static/manage-bower-packages.png)

* Vérifiez que la case à cocher "enregistrer les modifications *bower.json*" est activée. Sélectionnez une version dans la liste déroulante et cliquez sur le bouton **installer**. Le fenêtre de **sortie** affiche les détails d’installation.

### <a name="manual-installation-in-bowerjson"></a>Installation manuelle de bower.json

Ouvrez le fichier *bower.json* et ajoutez "Font Awesome" aux dépendances. IntelliSense affiche les paquets disponibles. Lorsqu’un paquet est sélectionné, les versions disponibles sont affichées. Les images ci-dessous sont plus anciennes et ne correspond pas à ce que vous voyez.

![IntelliSense de l’Explorateur de package bower](bower/_static/add-package.png)

![version de bower IntelliSense](bower/_static/version-intelliSense.png)

Bower utilise le [contrôle de version sémantique](http://semver.org/) pour organiser les dépendances. Le contrôle de version sémantique, également appelé SemVer, identifie les paquets avec le modèle de numérotation \<majeure >.\< mineure >. \<correctif >. IntelliSense simplifie le contrôle de version sémantique en affichant uniquement quelques choix courants. Le premier élément dans la liste IntelliSense (4.6.3 dans l’exemple ci-dessus) est considéré comme la dernière version stable du package. Le symbole du signe insertion (^) correspond à la version majeure la plus récente et le tilde (~) correspond à la version mineure la plus récente.

Enregistrer le fichier *bower.json*. Visual Studio surveille les modifications du fichier *bower.json*. Lors de l’enregistrement, la commende *bower install* est exécutée. Consultez la fenêtre de sortie et la vue **Bower/npm** pour la commande exacte exécutée.

Ouvrez le fichier *.bowerrc* sous le fichier *bower.json*. La propriété `directory` est configurée sur *wwwroot/lib* qui indique l’emplacement où Bower installera les composants de paquet.

```json
{
 "directory": "wwwroot/lib"
}
```

Vous pouvez utiliser la zone de recherche dans l’Explorateur de solutions pour rechercher et afficher le paquet font-awesome.

Ouvrez le fichier *Views\Shared\_Layout.cshtml* et ajoutez le fichier CSS font-awesome à l’environnement [TagHelper](xref:mvc/views/tag-helpers/intro) pour `Development`. À partir de l’Explorateur de solutions, faites glisser et déposez *awesome.css de police* à l’intérieur de la balise `<environment names="Development">`.

[!code-html[](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]

Dans une application de production, vous ajouteriez *awesome.min.css de police* à l'environnement pour `Staging,Production`.

Remplacez le contenu du fichier Razor *Views\Home\About.cshtml* avec le balisage suivant :

[!code-html[](bower/sample/About.cshtml)]

Exécutez l’application et accédez à la vue About pour vérifier le fonctionnement du paquet font-awesome.

## <a name="exploring-the-client-side-build-process"></a>Explorer le processus de génération du côté client

La plupart des modèles de projet ASP.NET Core sont déjà configurés pour utiliser Bower. Cette procédure pas à pas commence par un projet ASP.NET Core vide et ajoute chaque élément manuellement pour vous donner une idée de l’utilisation de Bower dans un projet. Vous pouvez voir ainsi ce qui arrive à la structure du projet et l’exécution en sortie chaque fois que la configuration est modifiée.

Les étapes globales à utiliser pour processus de génération côté client avec Bower sont :

* Définir les paquets utilisés dans votre projet. <!-- once defined, you don't need to download them, VS does -->
* Référencer des paquets à partir de vos pages web.

### <a name="define-packages"></a>Définir des packages

Une fois la liste de paquets définie dans le fichier *bower.json*, Visual Studio téléchargent les paquets. L’exemple suivant utilise Bower pour charger jQuery et Bootstrap dans le dossier *wwwroot*.

* Créez une application ASP.NET Core Web avec le modèle **Application Web ASP.NET Core (.NET Core)**. Sélectionnez le modèle de projet **vide** et cliquez sur **OK**.

* Dans l’Explorateur de solutions, cliquez sur le projet > **ajouter un nouvel élément** et sélectionnez **fichier de Configuration Bower**. Remarque : Un fichier *.bowerrc* est également ajouté.

* Ouvrez le fichier *bower.json* et ajoutez jquery et bootstrap dans la section `dependencies`. Le fichier *bower.json* résultant ressemblera à l’exemple suivant. Les versions changent au fil du temps et peuvent ne pas correspondre à l’image ci-dessous.

[!code-json[](bower/sample/bower.json?highlight=5,6)]

* Enregistrer le fichier *bower.json*.

 Vérifiez que le projet inclut les répertoires *Bootstrap* et *jQuery* répertoires dans *wwwroot/lib*. Bower utilise le *.bowerrc* fichier pour installer les composants dans *wwwroot/lib*.

 Remarque : L’interface utilisateur "Gérer les paquets Bower" offre une alternative à la modification manuelle du fichier .

### <a name="enable-static-files"></a>Activer les fichiers statiques

* Ajouter le paquets NuGet `Microsoft.AspNetCore.StaticFiles` au projet.
* Activer le support des fichiers statiques grâce au [middleware de service des fichiers statiques](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions). Ajoutez un appel à [UseStaticFiles](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions) à la méthode `Configure` de la classe `Startup`.

[!code-csharp[](bower/sample/Startup.cs?highlight=9)]

### <a name="reference-packages"></a>Packages de référence

Dans cette section, vous allez créer une page HTML pour vérifier qu’il peut accéder aux packages déployés.

* Ajouter une nouvelle page HTML nommée *Index.html* dans le dossier *wwwroot*. Remarque : Vous devez ajouter le fichier HTML dans le dossier *wwwroot*. En effet par défaut, le contenu statique ne peut pas être traitée en dehors du dossier *wwwroot*. Consultez [utilisation des fichiers statiques](xref:fundamentals/static-files) pour plus d’informations.

 Remplacez le contenu du fichier *Index.html* par le contenu suivant :

[!code-html[](bower/sample/Index.html)]

* Exécutez l’application et accédez à `http://localhost:<port>/Index.html`. Vous pouvez également à partir du fichier *Index.html* ouvert, appuyez sur `Ctrl+Shift+W`. Vérifiez que le style "jumbotron" est appliqué, que le code jQuery répond quand le bouton est activé et que le bouton d’amorçage change d’état.

 ![style de JumboTron appliqué](bower/_static/jumbotron.png)
