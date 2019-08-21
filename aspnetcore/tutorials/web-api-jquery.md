---
title: 'Tutoriel : Appeler une API web avec jQuery à l’aide d’ASP.NET Core'
author: rick-anderson
description: Découvrez comment appeler une API web ASP.NET Core avec jQuery.
ms.author: riande
ms.custom: mvc
ms.date: 07/20/2019
uid: tutorials/web-api-jquery
ms.openlocfilehash: a319e4b4ce09e9b09afeaff065d5740276deb115
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022556"
---
# <a name="tutorial-call-an-aspnet-core-web-api-with-jquery"></a>Tutoriel : Appeler une API web ASP.NET Core avec jQuery

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Ce tutoriel montre comment appeler une API web ASP.NET Core avec jQuery

::: moniker range="< aspnetcore-3.0"

Pour ASP.NET Core 2.2, consultez la version 2.2 de la section [Appeler l’API web avec jQuery](xref:tutorials/first-web-api#call-the-api-with-jquery).

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a>Prérequis

* Suivre le [Tutoriel : Créer une API web](xref:tutorials/first-web-api)
* Connaissance de CSS, HTML, JavaScript et jQuery.

## <a name="call-the-api-with-jquery"></a>Appeler l’API avec jQuery

Dans cette section, une page HTML qui utilise jQuery pour appeler l’API web est ajoutée. jQuery lance la requête et met à jour la page avec les détails de la réponse de l’API.

Configurez l’application pour [traiter les fichiers statiques](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) et [activer le mappage de fichier par défaut](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) en mettant à jour *Startup.cs* avec le code en surbrillance suivant :

[!code-csharp[](first-web-api/samples/3.0/TodoApi/StartupJquery.cs?highlight=8-9&name=snippet_configure)]

Créez un dossier *wwwroot* dans le répertoire du projet.

Ajoutez un fichier HTML nommé *index.html* au répertoire *wwwroot*. Remplacez son contenu par le balisage suivant :

[!code-html[](first-web-api/samples/3.0/TodoApi/wwwroot/index.html)]

Ajoutez un fichier JavaScript nommé *site.js* au répertoire *wwwroot*. Remplacez son contenu par le code suivant :

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

Vous devrez peut-être changer les paramètres de lancement du projet ASP.NET Core pour tester la page HTML localement :

* Ouvrez *Properties\launchSettings.json*.
* Supprimez la propriété `launchUrl` pour forcer l’utilisation du fichier *index.html* (fichier par défaut du projet) à l’ouverture de l’application.

Il existe plusieurs façons d’obtenir jQuery. Dans l’extrait précédent, la bibliothèque est chargée à partir d’un CDN.

Cet exemple appelle toutes les méthodes CRUD de l’API. Les explications suivantes traitent des appels à l’API.

### <a name="get-a-list-of-to-do-items"></a>Obtenir une liste de tâches

La fonction JQuery [ajax](https://api.jquery.com/jquery.ajax/) envoie une requête `GET` à l’API, qui retourne du code JSON représentant un tableau de tâches. La fonction de rappel `success` est appelée si la requête réussit. Dans le rappel, le DOM est mis à jour avec les informations des tâches.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a>Ajouter une tâche

La fonction [ajax](https://api.jquery.com/jquery.ajax/) envoie une requête `POST` dont le corps indique la tâche. Les options `accepts` et `contentType` sont définies avec la valeur `application/json` pour spécifier le type de média qui est reçu et envoyé. La tâche est convertie au format JSON à l’aide de [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify). Quand l’API retourne un code d’état de réussite, la fonction `getData` est appelée pour mettre à jour la table HTML.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a>Mettre à jour une tâche

La mise à jour d’une tâche est similaire à l’ajout d’une tâche. L’identificateur unique de la tâche est ajouté à l’`url` et le `type` est `PUT`.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a>Supprimer une tâche

Pour supprimer une tâche, vous devez définir le `type` sur l’appel AJAX avec la valeur `DELETE` et spécifier l’identificateur unique de l’élément dans l’URL.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

Passez au tutoriel suivant pour apprendre à générer des pages d’aide d’API :

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>
::: moniker-end
