---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: Créer le Client JavaScript | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: acadd8100b2698b4b17a750a02f875d4fd51c414
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826512"
---
<a name="create-the-javascript-client"></a>Créer le Client JavaScript
====================
par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](https://github.com/MikeWasson/BookService)

Dans cette section, vous allez créer le client pour l’application, à l’aide de HTML, JavaScript et le [Knockout.js](http://knockoutjs.com/) bibliothèque. Nous créerons l’application cliente dans les étapes suivantes :

- Affiche une liste de livres.
- Affiche un détail de livre.
- Ajout d’un nouveau livre.

La bibliothèque Knockout utilise le modèle Model-View-ViewModel (MVVM) :

- Le **modèle** est la représentation sous forme de côté serveur des données dans le domaine d’entreprise (dans notre cas, les livres et les auteurs).
- Le **vue** est la couche de présentation (HTML).
- Le **modèle de vue** est un objet JavaScript qui contient les modèles. Le modèle de vue est une abstraction de code de l’interface utilisateur. Elle n’a aucune connaissance de la représentation HTML. Au lieu de cela, il représente les fonctionnalités abstraites de la vue, tel que &quot;une liste de livres&quot;.

La vue est lié aux données au modèle de vue. Mises à jour au modèle de vue sont automatiquement répercutées dans la vue. Le modèle de vue obtient également les événements à partir de la vue, tels que les clics de bouton.

![](part-6/_static/image1.png)

Cette approche rend plus facile à modifier la disposition et l’interface utilisateur de votre application, comme vous pouvez modifier les liaisons, sans réécrire le code. Par exemple, vous pouvez afficher une liste d’éléments comme une `<ul>`, puis le modifier ultérieurement à une table.

## <a name="add-the-knockout-library"></a>Ajoutez la bibliothèque Knockout

Dans Visual Studio, à partir de la **outils** menu, sélectionnez **Library Package Manager**. Puis sélectionnez **Console du Gestionnaire de Package**. Dans la fenêtre de Console du Gestionnaire de Package, entrez la commande suivante :

[!code-console[Main](part-6/samples/sample1.cmd)]

Cette commande ajoute les fichiers de Knockout dans le dossier Scripts.

## <a name="create-the-view-model"></a>Créer le modèle de vue

Ajoutez un fichier JavaScript nommé app.js dans le dossier Scripts. (Dans l’Explorateur de solutions, cliquez sur le dossier Scripts, sélectionnez **ajouter**, puis sélectionnez **fichier JavaScript**.) Collez le code suivant :

[!code-javascript[Main](part-6/samples/sample2.js)]

Dans Knockout, la `observable` classe permet la liaison de données. Lorsque le contenu d’un observable change, l’observable avertit tous les contrôles liés aux données, ils peuvent mettre à jour eux-mêmes. (Le `observableArray` classe est la version du tableau de *observable*.) Pour commencer, notre modèle de vue a deux observables :

- `books` contient la liste de livres.
- `error` contient un message d’erreur si un appel AJAX échoue.

Le `getAllBooks` méthode effectue un appel AJAX pour obtenir la liste de livres. Puis il transmet les résultats sur le `books` tableau.

Le `ko.applyBindings` méthode fait partie de la bibliothèque Knockout. Il prend le modèle de vue en tant que paramètre et définit la liaison de données.

## <a name="add-a-script-bundle"></a>Ajouter un groupe de scripts

Le regroupement est une fonctionnalité de ASP.NET 4.5 qui permet de facilement combiner ou regrouper plusieurs fichiers dans un seul fichier. Regroupement réduit le nombre de demandes au serveur, ce qui peut améliorer les temps de chargement de page.

Ouvrez le fichier App\_Start/BundleConfig.cs. Ajoutez le code suivant à la méthode RegisterBundles.

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> [Précédent](part-5.md)
> [Suivant](part-7.md)
