---
title: Zones dans ASP.NET Core
author: rick-anderson
description: Découvrez les zones, fonctionnalité d’ASP.NET MVC utilisée pour organiser des fonctionnalités connexes dans un groupe sous la forme d’un espace de noms distinct (pour le routage) et d’une structure de dossiers (pour les vues).
ms.author: riande
ms.date: 02/14/2019
uid: mvc/controllers/areas
ms.openlocfilehash: c21eed04ea68512515da262b6b6895dc1a821039
ms.sourcegitcommit: 2c7ffe349eabdccf2ed748dd303ffd0ba6e1cfe3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/26/2019
ms.locfileid: "56833525"
---
# <a name="areas-in-aspnet-core"></a>Zones dans ASP.NET Core

Par [Dhananjay Kumar](https://twitter.com/debug_mode) et [Rick Anderson](https://twitter.com/RickAndMSFT)

Les zones sont une fonctionnalité d’ASP.NET qui servent à organiser les fonctionnalités associées dans un groupe sous la forme d’un espace de noms (pour le routage) et d’une structure de dossiers (pour les vues) distincts. L’utilisation de zones a pour effet de créer une hiérarchie pour les besoins du routage en ajoutant un autre paramètre de route, `area`, à `controller` et `action` ou une Razor Page `page`.

Les zones offrent un moyen de partitionner une application web ASP.NET Core en groupes fonctionnels plus petits, chacun avec son propre ensemble de Razor Pages, de contrôleurs, de vues et de modèles. Une zone est en réalité une structure au sein d’une application. Dans un projet web ASP.NET Core, les composants logiques comme Pages, Controller et View sont conservés dans différents dossiers. Le runtime ASP.NET Core utilise des conventions de nommage pour créer la relation entre ces composants. Pour une application volumineuse, il peut être avantageux de partitionner l’application en différentes zones de fonctionnalités de premier niveau. Par exemple, pour une application d’e-commerce à plusieurs entités, il peut s’agir de la caisse, de la facturation et de la recherche. Chacune de ces unités a sa propre zone pour accueillir les vues, les contrôleurs, les Razor Pages et les modèles.

Envisagez d’utiliser des zones dans un projet quand :

* L’application est constituée de plusieurs composants fonctionnels globaux qui peuvent être logiquement séparés.
* Vous pouvez partitionner l’application de façon à pouvoir travailler indépendamment sur chaque zone fonctionnelle.

[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample)). L’exemple de code téléchargeable fournit une application de base pour tester les zones.

## <a name="areas-for-controllers-with-views"></a>Zones pour contrôleurs avec vues

Une application web ASP.NET Core type qui utilise des zones, des contrôleurs et des vues est constituée des éléments suivants :

* Une [structure de dossiers Zone](#area-folder-structure).
* Des contrôleurs décorés avec l’attribut [&lbrack;Area&rbrack;](#attribute) pour associer le contrôleur à la zone : [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]
* La [route de zone ajoutée au démarrage](#add-area-route) : [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]

## <a name="area-folder-structure"></a>Structure de dossiers Zone
Imaginez une application qui contient deux groupes logiques, *Produits* et *Services*. En utilisant des zones, la structure de dossiers se présenterait comme suit :

* Nom du projet
  * Zones (Areas)
    * Produits
      * Contrôleurs
        * HomeController.cs
        * ManageController.cs
      * Affichages
        * Accueil
          * Index.cshtml
        * Gérer
          * Index.cshtml
          * About.cshtml
    * Services
      * Contrôleurs
        * HomeController.cs
      * Affichages
        * Accueil
          * Index.cshtml

Si la disposition précédente est classique dans les cas où des zones sont utilisées, seuls les fichiers de vues sont nécessaires pour utiliser cette structure de dossiers. La détection de vues recherche un fichier de vue de zone correspondant dans l’ordre suivant :

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
/Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
/Views/Shared/<Action-Name>.cshtml
/Pages/Shared/<Action-Name>.cshtml
   ```

L’emplacement des dossiers autres que Vues comme *Contrôleurs* et *Modèles* **n’a pas** d’importance. Par exemple, les dossiers *Contrôleurs* et *Modèles* ne sont pas nécessaires. Le contenu de *Contrôleurs* et *Modèles* est constitué de code qui est compilé dans un fichier .dll. Le contenu de *Vues* n’est pas compilée tant qu’aucune demande n’a été émise pour la vue en question.

<!-- TODO review:
The content of the *Views* isn't compiled until a request to that view has been made.

What about precompiled views? 
 -->
<a name="attribute"></a>

### <a name="associate-the-controller-with-an-area"></a>Associer le contrôleur à une zone

Les contrôleurs de zone sont désignés à l’aide de l’attribut [&lbrack;Area&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) :

[!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?highlight=5&name=snippet)]

### <a name="add-area-route"></a>Ajouter une route de zone

Les routes de zones utilisent généralement un routage conventionnel plutôt qu’un routage par attributs. Le routage conventionnel est dépendant de l’ordre. En général, les routes avec des zones doivent être placées plus haut dans la table de routage, car elles sont plus spécifiques que les routes sans zone.

`{area:...}` peut être utilisé comme jeton dans les modèles de route si l’espace d’url est uniforme dans toutes les zones :

[!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet&highlight=18-21)]

Dans le code précédent, `exists` applique une contrainte qui impose à la route de correspondre à une zone. Pour ajouter un routage à des zones, le mécanisme le moins compliqué consiste à utiliser `{area:...}`.

Le code suivant utilise <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> pour créer deux routes de zone nommées :

[!code-csharp[](areas/samples/MVCareas/StartupMapAreaRoute.cs?name=snippet&highlight=18-27)]

Si vous utilisez `MapAreaRoute` avec ASP.NET Core 2.2, prenez connaissance de [ce problème sur GitHub](https://github.com/aspnet/AspNetCore/issues/7772).

Pour plus d’informations, consultez [Routage de zones](xref:mvc/controllers/routing#areas).

### <a name="link-generation-with-areas"></a>Génération de liens avec zones

Le code suivant tiré de l’[exemple de code téléchargeable](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) illustre une génération de liens avec la zone spécifiée :

[!code-cshtml[](areas/samples/MVCareas/Views/Shared/_testLinksPartial.cshtml?name=snippet)]

Les liens générés par le code précédent sont valides où que ce soit dans l’application.

L’exemple de code téléchargeable comprend une [vue partielle](xref:mvc/views/partial) qui contient les liens précédents et les mêmes liens sans spécification de la zone. La vue partielle étant référencée dans le [fichier de disposition](), chaque page de l’application affiche les liens générés. Les liens générés sans spécification de la zone sont valides uniquement quand ils sont référencés dans une page contenue dans la même zone et le même contrôleur.

Quand la zone ou le contrôleur n’est pas spécifié, le routage dépend des valeurs *ambiantes*. Les valeurs de route actuelles de la requête actuelle sont considérées comme des valeurs ambiantes pour la génération de liens. Dans de nombreux cas, pour l’exemple d’application, l’utilisation des valeurs ambiantes génère des liens incorrects.

Pour plus d’informations, consultez [Routage vers des actions de contrôleur](xref:mvc/controllers/routing).

### <a name="shared-layout-for-areas-using-the-viewstartcshtml-file"></a>Disposition partagée pour les zones en utilisant le fichier _ViewStart.cshtml

Pour partager une disposition commune pour l’ensemble de l’application, déplacez *_ViewStart.cshtml* dans le dossier racine de l’application.

<!-- This section will be completed after https://github.com/aspnet/Docs/pull/10978 is merged.
<a name="arp"></a>

## Areas for Razor Pages
-->
<a name="rename"></a>

### <a name="change-default-area-folder-where-views-are-stored"></a>Changer le dossier de zone par défaut où sont stockées les vues

Le code suivant remplace le dossier de zone par défaut `"Areas"` par `"MyAreas"` :

[!code-csharp[](areas/samples/MVCareas/Startup2.cs?name=snippet)]

<!-- TODO review - can we delete this. Areas doesn't change publishing - right? -->
### <a name="publishing-areas"></a>Zones de publication

Tous les fichiers `*.cshtml` et `wwwroot/**` sont publiés en sortie quand `<Project Sdk="Microsoft.NET.Sdk.Web">` est inclus dans le fichier *.csproj*.
