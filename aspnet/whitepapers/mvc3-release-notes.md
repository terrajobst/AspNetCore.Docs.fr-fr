---
uid: whitepapers/mvc3-release-notes
title: ASP.NET MVC 3 | Microsoft Docs
author: rick-anderson
description: ''
ms.author: aspnetcontent
ms.date: 10/06/2010
ms.assetid: f44c166e-7e91-48a0-a6f8-d9285f3594e5
msc.legacyurl: /whitepapers/mvc3-release-notes
msc.type: content
ms.openlocfilehash: e6ad37d51e0475631314c1110f13fe8aef2f954c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831068"
---
<a name="aspnet-mvc-3"></a>ASP.NET MVC 3
====================
- [Vue d’ensemble](#overview)
- [Notes d’installation](#installation-notes)
- [Configuration logicielle requise](#software-requirements)
- [Documentation](#documentation)
- [Prise en charge](#support)
- [La mise à niveau d’un projet ASP.NET MVC 2 vers ASP.NET MVC 3 mis à jour](#upgrading)
- [ASP.NET MVC 3 Tools Update (12 avril 2011)](#tu-changes)

    - [Boîte de dialogue « Ajouter un contrôleur » peut générer automatiquement des modèles contrôleurs avec code d’accès aux données et de vues](#tu-AddControllerDialog)
    - [Améliorations apportées à la « ASP.NET MVC 3 nouveau projet « boîte de dialogue](#tu-ImprovementsNewDialogBox)
    - [Modèles de projet incluent à présent Modernizr 1.7](#tu-Modernizr)
    - [Modèles de projets incluent des versions mises à jour de jQuery, jQuery UI et jQuery Validation](#tu-UpdatedJQuery)
    - [Modèles de projet incluent à présent ADO.NET Entity Framework 4.1 comme package NuGet préinstallé](#tu-EF)
    - [Modèles de projet incluent des bibliothèques JavaScript comme packages NuGet préinstallés](#tu-JavaScriptLibsNuget)
    - [Problèmes connus](#tu-KI)
- [ASP.NET MVC 3 RTM (13 janvier 2011)](#MVC3RTM)

    - [Modification : Mise à jour la version de l’interface utilisateur jQuery à 1.8.7](#RTM-1)
    - [Modifications : Changé la valeur par défaut ModelMetadataProvider au DataAnnotationsModelMetadataProvider](#RTM-2)
    - [Problème résolu : Collez une partie d’une expression Razor contenant les résultats des espaces blancs qu’il contient est inversé](#RTM-3)
    - [Problème résolu : Renommer un fichier Razor qui est ouvert dans l’éditeur désactive la coloration syntaxique et IntelliSense](#RTM-4)
    - [Problèmes connus](#RTM-KI)
    - [Modifications avec rupture](#RTM-BC)
- [ASP.NET MVC 3 Release Candidate 2 (10 décembre 2010)](#_Toc2)

    - [Modèles de projet modifiés pour inclure jQuery 1.4.4, jQuery Validation 1.7 et jQuery UI 1.8.6y UI 1.8.6](#_Toc2_1)
    - [Classe de « AdditionalMetadataAttribute » ajouté](#_Toc2_2)
    - [Structure de la vue améliorée](#_Toc2_3)
    - [Méthode d’utilisation de Html.Raw ajoutée](#_Toc2_3)
    - [Propriétés de renommé « Controller.ViewModel » et « Vue » à « ViewBag »](#_Toc2_4)
    - [Classe renommé « ControllerSessionStateAttribute » à « SessionStateAttribute »](#_Toc2_5)
    - [Propriété RemoteAttribute renommé « Champs » « AdditionalFields »](#_Toc2_6)
    - [Renommé « SkipRequestValidationAttribute » à « AllowHtmlAttribute »](#_Toc2_7)
    - [Méthode modifiées « Html.ValidationMessage » pour afficher le premier Message d’erreur utiles](#_Toc2_8)
    - [Fixe @model déclaration de ne pas ajouter un espace blanc au Document](#_Toc2_9)
    - [Propriété ajoutée « FileExtensions » pour les moteurs d’affichage pour prendre en charge les noms de fichier spécifique de moteur](#_Toc2_10)
    - [Programme d’assistance fixe « LabelFor » pour émettre la valeur correcte pour l’attribut « For »](#_Toc2_11)
    - [Méthode RenderAction « fixe » pour accorder la priorité des valeurs explicites lors de la liaison de modèle](#_Toc2_12)
    - [Modifications avec rupture](#_Toc2_BC)
    - [Problèmes connus](#_Toc2_KI)
- [ASP.NET MVC 3 Release Candidate (9 novembre 2010)](#TOC_ASP_NET_3_RC)

    - [Nouvelles fonctionnalités dans ASP.NET MVC 3 RC](#_Toc276711785)
    - [Gestionnaire de Package NuGet](#_Toc276711786)
    - [Amélioration « Nouveau projet » boîte de dialogue](#_Toc276711787)
    - [Contrôleurs sessionless](#_Toc276711788)
    - [Nouveaux attributs de Validation](#_Toc276711789)
    - [Nouvelles surcharges pour « LabelFor » et les méthodes « LabelForModel »](#_Toc276711790)
    - [Action enfant la mise en cache de sortie](#_Toc276711791)
    - [Améliorations de boîte de dialogue « Add View »](#_Toc276711792)
    - [Validation de la demande granulaire](#_Toc276711793)
    - [Modifications avec rupture](#_Toc276711794)
    - [Problèmes connus](#_Toc276711795)
- [ASP. Notes de version bêta MVC 3 (6 octobre 2010)](#TOC_ASP_NET_3_Beta)

    - [Nouvelles fonctionnalités dans la version bêta d’ASP.NET MVC 3](#0.1__Toc274034215)
    - [Gestionnaire de Package NuPack](#0.1__Toc274034216)
    - [Boîte de dialogue Nouveau projet améliorée](#0.1__Toc274034217)
    - [Un moyen simple pour spécifier fortement typées modèles dans les vues Razor](#0.1__Toc274034218)
    - [Prise en charge de nouvelles méthodes d’assistance de Pages Web ASP.NET](#0.1__Toc274034219)
    - [Prise en charge l’Injection de dépendance supplémentaire](#0.1__Toc274034220)
    - [Nouvelle prise en charge d’Ajax de basée sur jQuery non obstructive](#0.1__Toc274034221)
    - [Nouvelle prise en charge pour la Validation jQuery non obstructive](#0.1__Toc274034222)
    - [Nouveaux indicateurs de l’Application pour la validation côté Client et le code JavaScript non Obstrusif](#0.1__Toc274034223)
    - [Nouvelle prise en charge pour le Code qui s’exécute avant l’exécution de vues](#0.1__Toc274034224)
    - [Nouvelle prise en charge pour la syntaxe Razor VBHTML](#0.1__Toc274034225)
    - [Contrôle plus granulaire sur ValidateInputAttribute](#0.1__Toc274034226)
    - [Programmes d’assistance convertir des traits de soulignement des traits d’union pour les noms d’attribut HTML spécifiés à l’aide d’objets anonymes](#0.1__Toc274034227)
    - [Correctifs de bogues](#0.1__Toc274034228)
    - [Modifications avec rupture](#0.1__Toc274034229)
    - [Problèmes connus](#0.1__Toc274034230)
- [Exclusion de responsabilité](#0.1__Toc274034231)

<a id="overview"></a>
## <a name="overview"></a>Vue d'ensemble

Ce document décrit la version de la version RTM d’ASP.NET MVC 3 pour Visual Studio 2010. ASP.NET MVC est une infrastructure pour le développement d’applications Web qui utilise le modèle Model-View-Controller (MVC). Le programme d’installation d’ASP.NET MVC 3 inclut les composants suivants :

- Composants d’exécution ASP.NET MVC 3
- Outils ASP.NET MVC 3 Visual Studio 2010
- Composants d’exécution ASP.NET Web Pages
- Outils de Visual Studio 2010 de Pages Web ASP.NET
- Gestionnaire de Package de Microsoft pour .NET (NuGet)
- Une mise à jour pour Visual Studio 2010 qui permet la prise en charge pour la syntaxe Razor. (Pour plus d’informations, consultez l’article de la base de connaissances 2483190.)

Vous trouverez l’ensemble des notes de publication pour chaque version préliminaire d’ASP.NET MVC 3 sur le site Web ASP.NET à l’adresse suivante :

https://www.asp.net/learn/whitepapers/mvc3-release-notes

<a id="installation-notes"></a>
## <a name="installation-notes"></a>Notes d’installation

Pour installer ASP.NET MVC 3 RTM à l’aide de Web Platform Installer (Web PI), visitez la page suivante :

[https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3](https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3)

Vous pouvez également télécharger le programme d’installation pour la version RTM d’ASP.NET MVC 3 pour Visual Studio 2010 à partir de la page suivante :

https://go.microsoft.com/fwlink/?LinkID=208140

ASP.NET MVC 3 peut être installé et peut s’exécuter côte à côte avec ASP.NET MVC 2.

<a id="software-requirements"></a>
## <a name="software-requirements"></a>Configuration logicielle

Les composants d’exécution ASP.NET MVC 3 requièrent le logiciel suivant :

- .NET framework version 4. 

    Outils ASP.NET MVC 3 Visual Studio 2010 requièrent le logiciel suivant :
- Visual Studio 2010 ou Visual Web Developer 2010 Express.

<a id="documentation"></a>
## <a name="documentation"></a>Documentation

Documentation pour ASP.NET MVC est disponible sur le site Web MSDN à l’adresse suivante :

[https://go.microsoft.com/fwlink/?LinkId=205717](https://go.microsoft.com/fwlink/?LinkId=205717)

Didacticiels et autres informations sur ASP.NET MVC sont disponibles sur la page MVC du site Web ASP.NET à l’adresse suivante :

[https://www.asp.net/mvc/](../mvc/index.md)

<a id="support"></a>
## <a name="support"></a>Assistance

Il s’agit d’une version entièrement prise en charge. Vous trouverez des informations sur l’obtention de support technique à la [site Web de Support Microsoft](https://support.microsoft.com/).

Également n’hésitez pas à poser des questions sur cette version sur le forum ASP.NET MVC, où les membres de la Communauté ASP.NET peuvent souvent fournir un support informel :

[https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)

<a id="upgrading"></a>
## <a name="upgrading-an-aspnet-mvc-2-project-to-aspnet-mvc-3-tools-update"></a>La mise à niveau d’un projet ASP.NET MVC 2 vers ASP.NET MVC 3 mis à jour

ASP.NET MVC 3 peut être installé côte à côte avec ASP.NET MVC 2 sur le même ordinateur, ce qui vous donne la souplesse dans le choix quand mettre à niveau une application ASP.NET MVC 2 vers ASP.NET MVC 3.

Pour mettre manuellement à une application ASP.NET MVC 2 existante vers la version 3, procédez comme suit :

1. Créer un nouveau projet ASP.NET MVC 3 vide sur votre ordinateur. Ce projet contiendra des fichiers qui sont requis pour la mise à niveau.
2. Copiez les fichiers suivants à partir du projet ASP.NET MVC 3 dans l’emplacement correspondant de votre projet ASP.NET MVC 2. Vous devez mettre à jour toutes les références à la bibliothèque jQuery pour prendre en compte le nouveau nom de fichier (jQuery-1.5.1.js) : 

    - /Views/Web.config
    - /packages.config
    - /scripts/\*.js
    - /Contenu/thèmes/\*.\*
3. Copie le *packages* dossier à la racine de la solution de projet ASP.NET MVC 3 vide dans la racine de votre solution, qui se trouve dans le répertoire où se trouve le fichier .sln de la solution.
4. Si votre projet ASP.NET MVC 2 contient des domaines, copiez le fichier /Views/Web.config le *vues* dossier de chaque zone.
5. Dans les deux fichiers Web.config dans le projet ASP.NET MVC 2, dans le monde entier rechercher et remplacer la version d’ASP.NET MVC. Recherchez la chaîne suivante : 

    [!code-console[Main](mvc3-release-notes/samples/sample1.cmd)]

    Remplacez-le par ce qui suit :

    [!code-console[Main](mvc3-release-notes/samples/sample2.cmd)]
6. Dans l’Explorateur de solutions, supprimez la référence à *System.Web.Mvc* (qui pointe vers la DLL de la version 2), puis ajoutez une référence à *System.Web.Mvc* (v3.0.0.0).
7. Ajoutez une référence à System.Web.WebPages.dll et à System.Web.Helpers.dll. Ces assemblys se trouvent dans les dossiers suivants : 

    - %ProgramFiles%\ Microsoft ASP.NET\ASP.NET MVC 3\Assemblies
    - %ProgramFiles%\ Microsoft ASP.NET\ASP.NET Web Pages\v1.0\Assemblies
8. Dans l’Explorateur de solutions, cliquez sur le nom de projet et sélectionnez Décharger le projet. Cliquez à nouveau sur le nom du projet, puis sélectionnez Modifier *nom_projet*.csproj.
9. Recherchez le *ProjectTypeGuids* élément et remplacez {F85E285D-A4E0-4152-9332-AB1D724D3325} par {E53F8FEA-EAE0-44A6-8774-FFD645390401}.
10. Enregistrer les modifications, cliquez sur le projet, puis sélectionnez recharger le projet.
11. Dans le fichier Web.config de racine de l’application, ajoutez les paramètres suivants pour le *assemblys* section. 

    [!code-xml[Main](mvc3-release-notes/samples/sample3.xml)]
12. Si le projet fait référence à toutes les bibliothèques tierces qui sont compilés à l’aide d’ASP.NET MVC 2, ajoutez ce qui suit en surbrillance *bindingRedirect* élément au fichier Web.config dans la racine de l’application sous le  *configuration* section : 

    [!code-xml[Main](mvc3-release-notes/samples/sample4.xml)]

<a id="tu-changes"></a>
## <a name="changes-in-aspnet-mvc-3-tools-update"></a>Nouveautés d’ASP.NET MVC 3 Tools Update

Cette section décrit les modifications apportées dans la version d’ASP.NET MVC 3 Tools Update depuis la version RTM d’ASP.NET MVC 3.

<a id="tu-AddControllerDialog"></a>
### <a name="add-controller-dialog-box-can-now-scaffold-controllers-with-views-and-data-access-code"></a>Boîte de dialogue « Ajouter un contrôleur » peut générer automatiquement des modèles contrôleurs avec code d’accès aux données et de vues

Génération de modèles automatique est un moyen de générer rapidement un contrôleur et les vues de votre application. Une fois que le code a été généré, vous pouvez le modifier pour répondre aux exigences de votre projet.

Pour lancer le *ajouter un contrôleur* avec le bouton droit de boîte de dialogue dans ASP.NET MVC 3, le *contrôleurs* dossier *l’Explorateur de solutions*, cliquez sur *ajouter*, puis cliquez sur *contrôleur*. La boîte de dialogue a été améliorée pour offrir des options de génération de modèles automatique supplémentaires.

![](mvc3-release-notes/_static/image1.png)

Il existe trois modèles de génération de modèles automatique par défaut.

#### <a name="empty-controller"></a>Contrôleur vide

Ce modèle génère un fichier de contrôleur vide. Ce modèle équivaut à ne pas cocher *ajouter des actions pour créer, modifier, détails, supprimez les scénarios* dans les versions précédentes d’ASP.NET MVC. Si vous choisissez cette option, aucune des options supplémentaires ne sont disponibles.

#### <a name="controller-with-empty-readwrite-actions"></a>Contrôleur avec actions en lecture/écriture vides

Ce modèle génère un fichier de contrôleur qui a toutes les méthodes d’action requise, mais aucun code de mise en œuvre dans les méthodes. Ce modèle est équivalent à la vérification *ajouter des actions pour créer, modifier, détails, supprimez les scénarios* dans les versions précédentes d’ASP.NET MVC. Si vous choisissez cette option, aucune des options supplémentaires ne sont disponibles.

#### <a name="controller-with-readwrite-actions-and-views-using-entity-framework"></a>Contrôleur avec actions en lecture/écriture et de vues, utilisant Entity Framework

Ce modèle vous permet de créer rapidement une interface utilisateur de saisie de données travail. Il génère un code qui gère un éventail de scénarios, tels que les éléments suivants et exigences courants :

- *Accès aux données*. Le code généré lit et écrit des entités dans une base de données. Il fonctionne avec l’approche Entity Framework Code First si vous choisissez une classe de contexte de données existante ou si vous laissez le modèle pour générer un nouveau *DbContext* classe. Elle fonctionne également avec l’approche Entity Framework Database First ou Model First si vous choisissez un existant *ObjectContext* classe.
- *Validation*. Le code généré utilise des fonctionnalités de métadonnées et de la liaison de modèle ASP.NET MVC afin que les envois de formulaire sont validés conformément aux règles déclarées sur votre classe de modèle. Cela inclut des règles de validation intégrées, telles que la *requis* et *StringLength* attributs et les règles de validation personnalisées.
- *Relations un-à-plusieurs*. Si vous définissez des relations de clé étrangère d’un-à-plusieurs entre vos classes de modèle, le code généré produira des listes déroulantes pour sélectionner des entités associées. Par exemple, vous pouvez définir les classes de modèle suivantes suit les conventions d’Entity Framework Code First : 

    [!code-csharp[Main](mvc3-release-notes/samples/sample5.cs)]

    Lorsque vous puis structurer un contrôleur pour le *produit* (classe), ses vues permettent aux utilisateurs de choisir un *catégorie* pour chaque objet *produit* instance.

    Ce modèle active des options supplémentaires dans le *ajouter un contrôleur* boîte de dialogue. Pour *classe de modèle*, vous pouvez choisir n’importe quelle classe de modèle dans votre solution, qui détermine le type de données que les utilisateurs pourront créer ou modifier :
- Si vous souhaitez utiliser Entity Framework Code First, vous pouvez choisir n’importe quelle classe de modèle.
- Si vous utilisez Entity Framework Database First ou Entity Framework Model First, veillez à choisir une classe d’entité définie dans votre modèle conceptuel.

Pour *classe de contexte de données*, vous pouvez effectuer les choix :

- Si vous souhaitez utiliser Code First et n’ont aucun contexte de données existant de classe, choisissez ** Nouveau contexte de données **. Une classe de contexte de données sera ensuite être générée pour vous.
- Si vous souhaitez utiliser Code First et disposent d’une classe de contexte de données existant, sélectionnez-le ici. Il sera mis à jour afin de conserver la classe de modèle que vous avez sélectionné.
- Si vous utilisez Database First ou Model First, choisissez votre classe de contexte d’objet.

Pour les vues, choisissez le moteur d’affichage que vous souhaitez utiliser, ou None si vous ne souhaitez pas générer automatiquement toutes les vues.

Vous pouvez sélectionner des options avancées spécifier d’autres options pour les vues générées. Par exemple, vous pouvez choisir la disposition ou la page maître à utiliser.

<a id="tu-ImprovementsNewDialogBox"></a>
### <a name="improvements-to-the-aspnet-mvc-3-new-project-dialog-box"></a>Améliorations apportées à la « ASP.NET MVC 3 nouveau projet « boîte de dialogue

La boîte de dialogue que vous permet de créer de nouveaux projets ASP.NET MVC 3 comprend plusieurs améliorations, comme indiqué ci-dessous.

![](mvc3-release-notes/_static/image2.png)

#### <a name="new-intranet-project-template"></a>Nouveau modèle « Projet Intranet »

La liste des modèles de projet inclut un nouveau modèle Application Intranet. Ce modèle contient des paramètres pour la création d’une application web à l’aide de l’authentification Windows au lieu de l’authentification par formulaire. Comme une application intranet requiert certains paramètres IIS qui ne peut pas être encapsulées dans un modèle de projet, le modèle inclut un fichier readme contenant des instructions pour savoir comment faire fonctionner dans IIS le modèle de projet. Documentation pour l’un nouveau modèle Application Intranet est disponible sur le site Web MSDN à l’adresse suivante :

[https://msdn.microsoft.com/library/gg703322(VS.98).aspx](https://msdn.microsoft.com/library/gg703322(VS.98).aspx)

#### <a name="project-templates-are-now-html5-enabled"></a>Modèles de projet sont maintenant HTML5 activé

La boîte de dialogue Nouveau projet contient maintenant une option pour ajouter des fonctionnalités HTML5 pour les modèles de projet. En sélectionnant l’option entraîne des vues générées qui contiennent la nouvelle HTML5 `<header>`, `<footer>`, et `<navigation>` éléments.

Notez que les versions antérieures des navigateurs ne gèrent pas les balises HTML5. Pour contourner cette limitation, les modèles de projet HTML5 incluent une référence à la bibliothèque Modernizr. (Voir la section suivante.)

<a id="tu-Modernizr"></a>
### <a name="project-templates-now-include-modernizr-17"></a>Modèles de projet incluent à présent Modernizr 1.7

Modernizr est une bibliothèque JavaScript qui permet la prise en charge CSS 3 et HTML5 dans les navigateurs qui ne prennent pas encore en charge ces fonctionnalités. Cette bibliothèque est incluse en tant que package NuGet préinstallé dans les modèles pour les projets ASP.NET MVC 3. Pour plus d’informations sur Modernizr, consultez [ http://www.modernizr.com/ ](http://www.modernizr.com/).

<a id="tu-UpdatedJQuery"></a>
### <a name="project-templates-include-updated-versions-of-jquery-jquery-ui-and-jquery-validation"></a>Modèles de projets incluent des versions mises à jour de jQuery, jQuery UI et jQuery Validation

Les modèles de projet incluent désormais les versions suivantes des scripts jQuery :

- jQuery 1.5.1
- jQuery Validation 1.8
- jQuery UI 1.8.11

Ces bibliothèques sont incluses comme packages NuGet préinstallés.

<a id="tu-EF"></a>
### <a name="project-templates-now-include-adonet-entity-framework-41-as-a-pre-installed-nuget-package"></a>Modèles de projet incluent à présent ADO.NET Entity Framework 4.1 comme package NuGet préinstallé

ADO.NET Entity Framework 4.1 inclut la fonctionnalité Code First. Code First est un nouveau modèle de développement pour ADO.NET Entity Framework qui fournit une alternative aux modèles Database First et Model First existants.

Code First consiste à définir votre modèle à l’aide des classes POCO (« plain old CLR objects ») écrites en Visual Basic ou c#. Ces classes peuvent ensuite être mappés à une base de données existante ou être utilisés pour générer un schéma de base de données. Une configuration supplémentaire peut être fournie à l’aide de *DataAnnotations* attributs ou à l’aide d’API fluent.

Documentation sur l’utilisation de Code Firstwith ASP.NET MVC est disponible sur le site Web ASP.NET aux URL suivantes :

[https://www.asp.net/mvc/tutorials/getting-started-with-mvc3-part1-cs](../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) [https://www.asp.net/entity-framework/tutorials/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)

<a id="tu-JavaScriptLibsNuget"></a>
### <a name="project-templates-include-javascript-libraries-as-pre-installed-nuget-packages"></a>Modèles de projet incluent des bibliothèques JavaScript comme packages NuGet préinstallés

Lorsque vous créez un nouveau projet ASP.NET MVC 3, le projet inclut les fichiers JavaScript mentionnés précédemment (par exemple, la bibliothèque Modernizr) en les installant à l’aide de NuGet au lieu d’ajouter directement les scripts dans le dossier Scripts dans le modèle de projet contenu. Cela vous permet d’utiliser NuGet pour mettre à jour les scripts vers la dernière version quand de nouvelles versions des scripts sont publiées.

Par exemple, étant donné la fréquence des nouvelles versions de jQuery, la version de jQuery incluse dans le modèle de projet à un moment donné sera obsolète. Toutefois, étant donné que jQuery est fourni comme un package NuGet installé, vous êtes averti dans la boîte de dialogue NuGet lorsque de nouvelles versions de jQuery sont disponibles.

Étant donné que jQuery inclut le numéro de version dans le nom de fichier, la mise à jour de jQuery vers la dernière version requiert également la mise à jour le `<script>` balise qui fait référence au fichier jQuery pour utiliser le nouveau nom de fichier. Autres bibliothèques de scripts fournies n’incluent pas le numéro de version dans le nom du script, ils peuvent être plus facilement mises à jour leurs versions les plus récentes.

<a id="tu-KI"></a>
## <a name="known-issues"></a>Problèmes connus

- Dans certains cas, l’installation peut échouer avec l’erreur message « Échec de l’Installation avec le code d’erreur (0 x 80070643) ». Pour plus d’informations sur la façon de contourner ce problème, consultez [article de la base de connaissances 2531566](https://support.microsoft.com/kb/2531566).
- La génération de modèles automatique pour l’ajout d’un contrôleur de ne pas générer automatiquement des modèles entités qui tirent parti de la prise en charge de l’héritage des entités dans Entity Framework. Par exemple, prenons une base de *personne* classe qui est héritée par un *étudiant* génération de modèles automatique de la classe la *étudiant* classe entraîne le code généré qui n’est pas compilé.
- Création d’un projet ASP.NET MVC 3 dans un dossier de solution entraîne une *NullReferenceException* erreur. La solution de contournement consiste à créer le projet ASP.NET MVC 3 dans la racine de la solution et puis le déplacer vers le dossier de solution.
- IntelliSense pour la syntaxe Razor ne fonctionne pas lorsque ReSharper est installé. Si vous avez ReSharper est installé et que vous souhaitez tirer parti de la prise en charge de Razor IntelliSense dans ASP.NET MVC 3, consultez l’entrée [Razor Intellisense and ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) sur le blog de hadi, qui décrit les manières de les utiliser aujourd'hui ensemble.
- Pendant l’installation, la boîte de dialogue d’acceptation CLUF affiche les termes du contrat de licence dans une fenêtre plus petite que prévue.
- Lors de la modification d’une vue Razor (.cshtml ou. *vbhtml* fichier), les vues. ASP.NET MVC 3 n’inclut pas les extraits de code pour les vues Razor... aspxselecting un extrait de code pour ASP.NET MVC affichera des extraits de code pour
- Si vous installez ASP.NET MVC 3 pour Visual Web Developer Express sur un ordinateur où Visual Studio n’est pas installé, installez Visual Studio ultérieurement, vous devez réinstaller ASP.NET MVC 3. Visual Studio et Visual Web Developer Express partagent des composants qui sont mis à niveau par le programme d’installation d’ASP.NET MVC 3. Le même problème s’applique si vous installez ASP.NET MVC 3 pour Visual Studio sur un ordinateur qui n’ont pas Visual Web Developer Express et installer par la suite Visual Web Developer Express.

<a id="MVC3RTM"></a>
## <a name="changes-in-aspnet-mvc-3-rtm"></a>Modifications dans la version RTM d’ASP.NET MVC 3

Cette section décrit les modifications et correctifs de bogues apportées dans la version RTM d’ASP.NET MVC 3 depuis la version RC2.

<a id="RTM-1"></a>
### <a name="change-updated-the-version-of-jquery-ui-to-187"></a>Modification : Mise à jour la version de l’interface utilisateur jQuery à 1.8.7

Les modèles de projet ASP.NET MVC pour Visual Studio ont été mis à jour pour inclure la dernière version de la bibliothèque jQuery UI. Les modèles incluent également l’ensemble minimal de fichiers de ressources requises par l’interface utilisateur jQuery, telles que le CSS associés et les fichiers image.

<a id="RTM-2"></a>
### <a name="change-changed-the-default-modelmetadataprovider-back-to-dataannotationsmodelmetadataprovider"></a>Modifications : Changé la valeur par défaut ModelMetadataProvider au DataAnnotationsModelMetadataProvider

La version RC2 d’ASP.NET MVC 3 a introduit un *CachedDataAnnotationsMetadataProvider* classe que la mise en cache sur existant fourni *DataAnnotationsModelMetadataProvider* classe en tant qu’un amélioration des performances. Toutefois, certains bogues ont été signalées avec cette implémentation, la modification a été rétablie et déplacée dans le projet MVC Futures, qui est disponible à l’adresse [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack).

<a id="RTM-3"></a>
### <a name="fixed-pasting-part-of-a-razor-expression-that-contains-whitespace-results-in-it-being-reversed"></a>Problème résolu : Collez une partie d’une expression Razor contenant les résultats des espaces blancs qu’il contient est inversé

Dans les versions préliminaires d’ASP.NET MVC 3, lorsque vous collez une partie d’une expression Razor qui contient un espace blanc dans un fichier Razor, l’expression obtenue est inversée. Par exemple, considérez le bloc de code Razor suivant :

[!code-cshtml[Main](mvc3-release-notes/samples/sample6.cshtml)]

Si vous sélectionnez la texte « premier param » dans la première méthode et collez en tant qu’argument dans la deuxième méthode, le résultat est comme suit :

[!code-cshtml[Main](mvc3-release-notes/samples/sample7.cshtml)]

Le comportement correct est que l’opération de collage doit avoir les conséquences suivantes :

[!code-cshtml[Main](mvc3-release-notes/samples/sample8.cshtml)]

Ce problème a été résolu dans la version RTM, afin que l’expression est correctement conservée pendant l’opération de collage.

<a id="RTM-4"></a>
### <a name="fixed-renaming-a-razor-file-that-is-opened-in-the-editor-disables-syntax-colorization-and-intellisense"></a>Problème résolu : Renommer un fichier Razor qui est ouvert dans l’éditeur désactive la coloration syntaxique et IntelliSense

Renommer un fichier Razor à l’aide de l’Explorateur de solutions tandis que le fichier est ouvert dans la fenêtre Éditeur entraîne la mise en surbrillance de syntaxe et IntelliSense pour cesser de fonctionner pour ce fichier. Ce problème a été résolu pour que la mise en surbrillance et IntelliSense sont conservés après un changement de nom.

<a id="RTM-KI"></a>
## <a name="known-issues"></a>Problèmes connus

- Si vous fermez Visual Studio 2010 SP1 bêta alors que la Console du Gestionnaire de Package NuGet est ouverte, Visual Studio se bloque et tente de redémarrer. Ce problème sera résolu dans la version RTM de Visual Studio 2010 SP1.
- Le programme d’installation d’ASP.NET MVC 3 est uniquement capable d’installer une version initiale du Gestionnaire de package NuGet. Une fois que vous avez installé la version initiale, NuGet peut être installé et mis à jour à l’aide du Gestionnaire d’extensions Visual Studio. Si vous avez déjà installé NuGet, accédez à la galerie d’extensions Visual Studio pour mettre à jour vers la dernière version de NuGet.
- Création d’un projet ASP.NET MVC 3 dans un dossier de solution entraîne une *NullReferenceException* erreur. La solution de contournement consiste à créer le projet ASP.NET MVC 3 dans la racine de la solution et puis le déplacer vers le dossier de solution.
- Le programme d’installation peut prendre beaucoup plus de temps que les versions antérieures d’ASP.NET MVC pour terminer. Il s’agit, car elle met à jour les composants de Visual Studio 2010.
- IntelliSense pour la syntaxe Razor ne fonctionne pas lorsque ReSharper est installé. Si vous avez ReSharper est installé et que vous souhaitez tirer parti de la prise en charge de Razor IntelliSense dans ASP.NET MVC 3, consultez l’entrée [Razor Intellisense and ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) sur le blog de hadi, qui décrit les manières de les utiliser aujourd'hui ensemble.
- CCSHTML et VBHTML vues créées avec la version bêta d’ASP.NET MVC 3 n’ont pas leur action de génération définie correctement, ce qui permet d’afficher ces types sont omis lorsque le projet est publié. La valeur de l’Action de génération pour ces fichiers doit être définie à « Contenu ». ASP.NET MVC 3 RTM résout ce problème pour les nouveaux fichiers, mais ne résout pas le paramètre pour les fichiers existants pour un projet créé avec les versions préliminaires.
- ![](mvc3-release-notes/_static/image3.png)
- Pendant l’installation, la boîte de dialogue d’acceptation CLUF affiche les termes du contrat de licence dans une fenêtre plus petite que prévue.
- Lorsque vous modifiez une vue Razor (.cshtml fichier), l’élément de menu Aller à contrôleur dans Visual Studio ne sera pas disponible, et il en existe aucun extrait de code.
- Si vous installez ASP.NET MVC 3 pour Visual Web Developer Express sur un ordinateur où Visual Studio n’est pas installé, installez Visual Studio ultérieurement, vous devez réinstaller ASP.NET MVC 3. Visual Studio et Visual Web Developer Express partagent des composants qui sont mis à niveau par le programme d’installation d’ASP.NET MVC 3. Le même problème s’applique si vous installez ASP.NET MVC 3 pour Visual Studio sur un ordinateur qui n’ont pas Visual Web Developer Express et installer par la suite Visual Web Developer Express.

<a id="RTM-BC"></a>
## <a name="breaking-changes"></a>Modifications avec rupture

- Dans les versions précédentes d’ASP.NET MVC, action, les filtres sont créer par demande, sauf dans certains cas. Ce comportement n’a jamais été un comportement de garantie, mais simplement un détail d’implémentation et le contrat pour les filtres était de prendre en compte sans état. Dans ASP.NET MVC 3, les filtres sont mis en cache en plus agressive. Par conséquent, les filtres d’action personnalisée qui stockent pas correctement état de l’instance peuvent être rompues.
- L’ordre d’exécution des filtres d’exception a été modifiée pour les filtres d’exception qui ont le même *ordre* valeur. Dans ASP.NET MVC 2 et versions antérieures, les filtres d’exception sur le contrôleur qui ont le même *ordre* valeur que ceux sur une méthode d’action sont exécutés avant les filtres d’exception sur la méthode d’action. Cela serait généralement le cas lorsque des filtres d’exception sont appliqués sans spécifier *ordre* valeur. Dans ASP.NET MVC 3, cette commande a été inversée afin que le Gestionnaire d’exceptions plus spécifique s’exécute en premier. Comme dans les versions antérieures, si le *ordre* propriété est explicitement spécifiée, les filtres sont exécutés dans l’ordre spécifié.
- Une nouvelle propriété nommée *FileExtensions* a été ajouté à la *VirtualPathProviderViewEngine* classe de base. Quand ASP.NET recherche une vue par chemin d’accès (pas par nom), uniquement les affichages avec une extension de fichier contenues dans la liste spécifiée par cette nouvelle propriété sont considérées comme. Il s’agit d’une modification avec rupture dans les applications où un fournisseur de générations personnalisé est inscrit afin d’activer une extension de fichier personnalisé pour les vues de formulaire Web et le fournisseur de référence ces vues à l’aide d’un chemin d’accès complet plutôt qu’un nom. La solution de contournement consiste à modifier la valeur de la *FileExtensions* propriété à inclure l’extension de fichier personnalisé.
- Les implémentations de fabrique de contrôleur personnalisé qui implémentent directement le *IControllerFactory* interface doit fournir une implémentation de la nouvelle *GetControllerSessionBehavior* méthode qui a été ajouté à l’interface dans cette version. En règle générale, il est recommandé de ne pas implémenter cette interface directement et à la place de dériver votre classe de *DefaultControllerFactory*.

<a id="_Toc2"></a>
## <a name="changes-in-aspnet-mvc-3-rc2"></a>Nouveautés d’ASP.NET MVC 3 RC2

Cette section décrit les modifications (nouvelles fonctionnalités et correctifs de bogues) dans la version d’ASP.NET MVC 3 RC2 depuis la version RC.

<a id="_Toc2_1"></a>
### <a name="project-templates-changed-to-include-jquery-144-jquery-validation-17-and-jquery-ui-186"></a>Modèles de projet modifiés pour inclure jQuery 1.4.4, jQuery Validation 1.7 et jQuery UI 1.8.6

Les modèles de projet pour ASP.NET MVC 3 incluent désormais les dernières versions de jQuery, jQuery Validation et jQuery UI. jQuery UI est une nouveauté pour les modèles de projet et fournit des widgets d’interface utilisateur utile. Pour plus d’informations sur l’interface utilisateur jQuery, visitez leur page d’accueil : [ http://jqueryui.com/ ](http://jqueryui.com/).

<a id="_Toc2_2"></a>
### <a name="added-additionalmetadataattribute-class"></a>Classe de « AdditionalMetadataAttribute » ajouté

Vous pouvez utiliser la *AdditionalMetadataAttribute* classe pour remplir la *ModelMetadata.AdditionalValues* dictionnaire pour une propriété de modèle.

Par exemple, qu'un modèle de vue a des propriétés qui doivent être affichées uniquement à un administrateur. Ce modèle peut être annoté avec le nouvel attribut à l’aide de AdminOnly en tant que la clé et la valeur true comme valeur, comme dans l’exemple suivant :

[!code-csharp[Main](mvc3-release-notes/samples/sample9.cs)]

Ces métadonnées sont accessible à n’importe quel modèle d’affichage ou de l’éditeur lors du rendu d’un modèle de vue du produit. Il vous incombe en tant que développeur d’applications pour interpréter les informations de métadonnées.

<a id="_Toc2_3"></a>
### <a name="improved-view-scaffolding"></a>Structure de la vue améliorée

Les modèles T4 utilisés pour les vues de la génération de modèles automatique maintenant génèrent des appels aux méthodes d’assistance de modèle tel que *EditorFor* au lieu de programmes d’assistance telles que *TextBoxFor*. Cette modification améliore la prise en charge des métadonnées sur le modèle sous la forme d’attributs d’annotation de données lorsque la boîte de dialogue Ajouter une vue génère une vue.

La génération de modèles Ajouter une vue inclut également une amélioration des détections et l’utilisation des informations de clé primaire sur le modèle, selon la convention. Par exemple, la boîte de dialogue Ajouter une vue utilise ces informations pour vous assurer que la valeur de clé primaire n’est pas structurée comme un champ de formulaire modifiable.

Par défaut modification et création de modèles inclure des références aux scripts jQuery requis pour la validation côté client.

<a id="_Toc2_4"></a>
### <a name="added-htmlraw-method"></a>Méthode d’utilisation de Html.Raw ajoutée

Par défaut, le code Razor moteur d’affichage au format HTML toutes les valeurs. Par exemple, l’extrait de code suivant encode le code HTML à l’intérieur de la variable de salutations afin qu’elle est affichée dans la page en tant que `<strong>Hello World!</strong>`.

[!code-cshtml[Main](mvc3-release-notes/samples/sample10.cshtml)]

La nouvelle *utilisation de Html.Raw* méthode fournit un moyen simple d’afficher le code HTML non codé lorsque le contenu est connu pour être sûr. L’exemple suivant affiche la même chaîne, mais la chaîne est restituée sous forme de balisage :

[!code-cshtml[Main](mvc3-release-notes/samples/sample11.cshtml)]

<a id="_Toc2_5"></a>
### <a name="renamed-controllerviewmodel-property-and-the-view-property-to-viewbag"></a>Propriétés de renommé « Controller.ViewModel » et « Vue » à « ViewBag »

Auparavant, le *ViewModel* propriété du *contrôleur* correspondaient au *vue* propriété d’une vue. Ces propriétés fournissent un moyen d’accéder aux valeurs de la *ViewDataDictionary* de l’objet à l’aide de la syntaxe de l’accesseur de propriété dynamique. Les deux propriétés ont été renommées pour correspondre à l’afin d’éviter toute confusion et plus cohérents.

<a id="_Toc2_6"></a>
### <a name="renamed-controllersessionstateattribute-class-to-sessionstateattribute"></a>Classe renommé « ControllerSessionStateAttribute » à « SessionStateAttribute »

Le *ControllerSessionStateAttribute* classe a été introduite dans la version RC d’ASP.NET MVC 3. La propriété a été renommée pour être plus concise.

<a id="_Toc2_7"></a>
### <a name="renamed-remoteattribute-fields-property-to-additionalfields"></a>Propriété RemoteAttribute renommé « Champs » « AdditionalFields »

Le *RemoteAttribute* la classe *champs* propriété a provoqué une certaine confusion parmi les utilisateurs. Modification du nom de cette propriété sur *AdditionalFields* clarifie son intention.

<a id="_Toc2_8"></a>
### <a name="renamed-skiprequestvalidationattribute-to-allowhtmlattribute"></a>Renommé « SkipRequestValidationAttribute » à « AllowHtmlAttribute »

Le *SkipRequestValidationAttribute* attribut a été renommé en *AllowHtmlAttribute* afin de mieux représenter son utilisation prévue.

<a id="_Toc2_9"></a>
### <a name="changed-htmlvalidationmessage-method-to-display-the-first-useful-error-message"></a>Méthode modifiées « Html.ValidationMessage » pour afficher le premier Message d’erreur utiles

Le *Html.ValidationMessage* méthode a été corrigée pour afficher le premier message d’erreur utiles au lieu d’afficher simplement la première erreur.

Lors de la liaison de modèle, le *ModelState* dictionnaire peut être rempli à partir de plusieurs sources avec des messages d’erreur sur la propriété, y compris à partir du modèle lui-même (s’il implémente *IValidatableObject* ), à partir des attributs de validation appliqués à la propriété et à partir d’exceptions levées alors que la propriété est accessible.

Lorsque le *Html.ValidationMessage* méthode affiche un message de validation, il ignore les entrées d’états de modèles qui incluent une exception, car ils sont généralement pas destinés à l’utilisateur final. Au lieu de cela, la méthode recherche le premier message de validation qui n’est pas associé à une exception et affiche ce message. Si aucun message n’est trouvée, la valeur par défaut est un message d’erreur générique qui est associé à la première exception.

<a id="_Toc2_10"></a>
### <a name="fixed-model-declaration-to-not-add-whitespace-to-the-document"></a>Fixe @model déclaration de ne pas ajouter un espace blanc au Document

Dans les versions antérieures, le <em>@model</em> déclaration en haut d’une vue ajouté une ligne vide à la sortie HTML rendue. Ce problème a été résolu afin que la déclaration n’introduit pas d’espace blanc.

<a id="_Toc2_11"></a>
### <a name="added-fileextensions-property-to-view-engines-to-support-engine-specific-file-names"></a>Propriété ajoutée « FileExtensions » pour les moteurs d’affichage pour prendre en charge les noms de fichier spécifique de moteur

Un moteur d’affichage peut retourner une vue à l’aide d’un chemin d’accès de mode explicite, comme dans l’exemple suivant :

[!code-csharp[Main](mvc3-release-notes/samples/sample12.cs)]

Le moteur d’affichage première tente toujours de restituer la vue. Par défaut, le moteur d’affichage Web Forms est le premier moteur d’affichage ; Étant donné que le moteur de Web Forms ne peut pas restituer une vue Razor, une erreur se produit. Moteurs d’affichage ont maintenant un *FileExtensions* propriété qui est utilisée pour spécifier les extensions de fichier prises en charge. Cette propriété est vérifiée lorsque ASP.NET détermine si un moteur d’affichage peut restituer un fichier. Il s’agit d’une modification avec rupture et plus de détails sont inclus dans le [modifications avec rupture](#_Toc2_BC) section de ce document.

<a id="_Toc2_12"></a>
### <a name="fixed-labelfor-helper-to-emit-the-correct-value-for-the-for-attribute"></a>Programme d’assistance fixe « LabelFor » pour émettre la valeur correcte pour l’attribut « For »

Correction d’un bogue où le *LabelFor* méthode rendu un *pour* attribut qui correspond à la *d’entrée* l’élément *nom* attribut au lieu de cela de son ID. Selon le W3C, le *pour* attribut doit correspondre à la *d’entrée* ID de. l’élément

<a id="_Toc2_13"></a>
### <a name="fixed-renderaction-method-to-give-explicit-values-precedence-during-model-binding"></a>Méthode RenderAction « fixe » pour accorder la priorité des valeurs explicites lors de la liaison de modèle

Dans les versions antérieures, les valeurs explicites ont été passés à la *RenderAction* (méthode) ont été ignorés en faveur de la valeur actuelle du formulaire lors de la liaison de modèle à l’intérieur d’une action enfant. Le correctif garantit que les valeurs explicites sont prioritaires pendant la liaison de modèle.

<a id="_Toc2_BC"></a>
## <a name="breaking-changes"></a>Modifications avec rupture

- Dans les versions précédentes d’ASP.NET MVC, les filtres d’action ont été créés par la demande, sauf dans certains cas. Ce comportement n’a jamais été un comportement de garantie, mais simplement un détail d’implémentation et le contrat pour les filtres était de prendre en compte sans état. Dans ASP.NET MVC 3, les filtres sont mis en cache en plus agressive. Par conséquent, les filtres d’action personnalisée qui stockent pas correctement état de l’instance peuvent être rompues.
- L’ordre d’exécution des filtres d’exception a été modifiée pour les filtres d’exception qui ont le même *ordre* valeur. Dans ASP.NET MVC 2 et versions antérieures, les filtres d’exception sur le contrôleur ayant le même *ordre* valeur que ceux sur une méthode d’action étaient exécutés avant les filtres d’exception sur la méthode d’action. Cela serait généralement le cas lorsque les filtres d’exception étaient appliqués sans spécifier *ordre* valeur. Dans ASP.NET MVC 3, cette commande a été inversée afin que le Gestionnaire d’exceptions plus spécifique s’exécute en premier. Comme dans les versions antérieures, si le *ordre* propriété est explicitement spécifiée, les filtres sont exécutés dans l’ordre spécifié.
- Une nouvelle propriété nommée *FileExtensions* a été ajouté à la *VirtualPathProviderViewEngine* classe de base. Quand ASP.NET recherche une vue par chemin d’accès (pas par nom), uniquement les affichages avec une extension de fichier contenues dans la liste spécifiée par cette nouvelle propriété sont considérées comme. Il s’agit d’une modification avec rupture dans les applications où un fournisseur de générations personnalisé est inscrit afin d’activer une extension de fichier personnalisé pour les vues de formulaire Web et le fournisseur de référence ces vues à l’aide d’un chemin d’accès complet plutôt qu’un nom. La solution de contournement consiste à modifier la valeur de la *FileExtensions* propriété à inclure l’extension de fichier personnalisé.
- Les implémentations de fabrique de contrôleur personnalisé qui implémentent directement le <em>IControllerFactory</em> interface doit fournir une implémentation de la nouvelle <em>GetControllerSessionBehavior</em>  <em>méthode qui a été ajoutée à l’interface dans cette version</em>. En règle générale, il est recommandé de ne pas implémenter cette interface directement et à la place de dériver votre classe de <em>DefaultControllerFactory</em>.

<a id="_Toc2_KI"></a>
## <a name="known-issues"></a>Problèmes connus

- Le programme d’installation d’ASP.NET MVC 3 est uniquement capable d’installer une version initiale du Gestionnaire de package NuGet. Une fois que vous avez installé la version initiale, NuGet peut être installé et mis à jour à l’aide du Gestionnaire d’extensions Visual Studio. Si vous avez déjà installé NuGet, accédez à la galerie d’extensions Visual Studio pour mettre à jour vers la dernière version de NuGet.
- Création d’un projet ASP.NET MVC 3 dans un dossier de solution entraîne une *NullReferenceException* erreur. La solution de contournement consiste à créer le projet ASP.NET MVC 3 dans la racine de la solution et puis le déplacer vers le dossier de solution.
- Le programme d’installation peut prendre beaucoup plus de temps que les versions antérieures d’ASP.NET MVC pour terminer. Il s’agit, car elle met à jour les composants de Visual Studio 2010.
- IntelliSense pour la syntaxe Razor ne fonctionne pas lorsque ReSharper est installé. Si vous avez ReSharper est installé et que vous souhaitez tirer parti de la prise en charge de Razor IntelliSense dans ASP.NET MVC 3 RC2, consultez l’entrée [Razor Intellisense and ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) sur le blog de hadi, qui décrit les manières de les utiliser aujourd'hui ensemble.
- CSHTML et VBHTML vues créées avec la version bêta d’ASP.NET MVC 3 n’ont pas leur action de génération définie correctement, ce qui permet d’afficher ces types sont omis lorsque le projet est publié. Le *Action de génération* valeur de ces fichiers doivent être définies au contenu ». ASP.NET MVC 3 RC2 résout ce problème pour les nouveaux fichiers, mais ne résout pas le paramètre pour les fichiers existants pour un projet créé avec la version bêta.![](mvc3-release-notes/_static/image4.png)
- Pendant l’installation, la boîte de dialogue d’acceptation CLUF affiche les termes du contrat de licence dans une fenêtre plus petite que prévue.
- Lorsque vous modifiez une vue Razor (.cshtml fichier), l’élément de menu Aller à contrôleur dans Visual Studio ne sera pas disponible, et il en existe aucun extrait de code.
- Si vous installez ASP.NET MVC 3 pour Visual Web Developer Express sur un ordinateur où Visual Studio n’est pas installé, installez Visual Studio ultérieurement, vous devez réinstaller ASP.NET MVC 3. Visual Studio et Visual Web Developer Express partagent des composants qui sont mis à niveau par le programme d’installation d’ASP.NET MVC 3. Le même problème s’applique si vous installez ASP.NET MVC 3 pour Visual Studio sur un ordinateur qui n’ont pas Visual Web Developer Express et installer par la suite Visual Web Developer Express.
- L’installation d’ASP.NET MVC 3 RC 2 ne pas mettre à jour NuGet si vous avez déjà installé. Pour mettre à niveau de NuGet, accédez à Visual Studio Extension manager et il doit s’afficher comme une mise à jour disponible. Vous pouvez mettre à niveau NuGet vers la dernière version à partir de là.

<a id="TOC_ASP_NET_3_RC"></a>
## <a name="aspnet-mvc-3-release-candidate"></a>ASP.NET MVC 3 Release Candidate

ASP.NET MVC version finale a été publiée le 9 novembre 2010.

<a id="_Toc276711785"></a>
## <a name="new-features-in-aspnet-mvc-3-rc"></a>Nouvelles fonctionnalités dans ASP.NET MVC 3 RC

Cette section décrit les fonctionnalités qui ont été introduites dans la version d’ASP.NET MVC 3 RC depuis la version bêta.

<a id="_Toc276711786"></a>
### <a name="nuget-package-manager"></a>Gestionnaire de package NuGet

ASP.NET MVC 3 inclut le Gestionnaire de Package NuGet (anciennement NuPack), qui est un outil de gestion de package intégré pour l’ajout des bibliothèques et des outils pour les projets Visual Studio. Cet outil automatise les étapes que les développeurs entreprendre dès aujourd'hui pour obtenir une bibliothèque dans leur arborescence source.

Vous pouvez utiliser NuGet comme un outil de ligne de commande, comme une fenêtre de console intégrée à l’intérieur de Visual Studio 2010, dans le menu contextuel de Visual Studio et comme un ensemble d’applets de commande PowerShell.

Pour plus d’informations sur NuGet, lisez le [Documentation de Nuget](https://docs.microsoft.com/nuget/).

<a id="_Toc276711787"></a>
### <a name="improved-new-project-dialog-box"></a>Amélioration « Nouveau projet » boîte de dialogue

Lorsque vous créez un nouveau projet, la boîte de dialogue Nouveau projet vous permet désormais de spécifier le moteur d’affichage, ainsi qu’un type de projet ASP.NET MVC.

![](mvc3-release-notes/_static/image5.png)

Prise en charge pour la modification de la liste de modèles et affichage de moteurs répertoriés dans la boîte de dialogue est incluse dans cette version.

Les modèles par défaut sont les suivantes :

Vide. Contient un ensemble minimal de fichiers pour un projet ASP.NET MVC, y compris la structure de répertoire par défaut pour les projets ASP.NET MVC, et un fichier Site.css qui contient la valeur par défaut que définit le style de ASP.NET MVC et un répertoire de Scripts qui contient les fichiers de JavaScript par défaut.

Application Internet. Contient les fonctionnalités d’exemple qui montre comment utiliser le fournisseur d’appartenances avec ASP.NET MVC.

La liste des modèles de projet qui s’affiche dans la boîte de dialogue est spécifiée dans le Registre Windows.

<a id="_Toc276711788"></a>
### <a name="sessionless-controllers"></a>Contrôleurs sessionless

La nouvelle *ControllerSessionStateAttribute* vous donne davantage de contrôle sur le comportement de l’état de session pour les contrôleurs en spécifiant un [System.Web.SessionState.SessionStateBehavior](https://msdn.microsoft.com/library/system.web.sessionstate.sessionstatebehavior.aspx) valeur d’énumération.

L’exemple suivant montre comment désactiver l’état de session pour toutes les demandes à un contrôleur.

[!code-csharp[Main](mvc3-release-notes/samples/sample13.cs)]

L’exemple suivant montre comment définir l’état de session en lecture seule pour toutes les demandes à un contrôleur.

[!code-csharp[Main](mvc3-release-notes/samples/sample14.cs)]

<a id="_Toc276711789"></a>
### <a name="new-validation-attributes"></a>Nouveaux attributs de Validation

#### <a name="compareattribute"></a>CompareAttribute

La nouvelle *CompareAttribute* attribut de validation vous permet de comparer les valeurs de deux propriétés différentes d’un modèle. Dans l’exemple suivant, le *ComparePassword* propriété doit correspondre à la *mot de passe* champ afin d’être valide.

[!code-csharp[Main](mvc3-release-notes/samples/sample15.cs)]

#### <a name="remoteattribute"></a>RemoteAttribute

La nouvelle *RemoteAttribute* attribut de validation tire parti de validateur à distance le jQuery Validation du plug-in, ce qui active la validation côté client appeler une méthode sur le serveur qui exécute la logique de validation réelle.

Dans l’exemple suivant, le *nom d’utilisateur* propriété a la *RemoteAttribute* appliqué. Lorsque vous modifiez cette propriété dans une vue d’édition, la validation côté client appellera une action nommée *UserNameAvailable* sur le *UsersController* classe afin de valider ce champ.

[!code-csharp[Main](mvc3-release-notes/samples/sample16.cs)]

L’exemple suivant montre le contrôleur correspondant.

[!code-csharp[Main](mvc3-release-notes/samples/sample17.cs)]

Par défaut, le nom de propriété, l’attribut est appliqué est envoyé à la méthode d’action comme un paramètre de chaîne de requête.

<a id="_Toc276711790"></a>
### <a name="new-overloads-for-labelfor-and-labelformodel-methods"></a>Nouvelles surcharges pour « LabelFor » et les méthodes « LabelForModel »

Nouvelles surcharges ont été ajoutées pour le *LabelFor* et *LabelForModel* les méthodes qui vous permettent de spécifient le texte d’étiquette. L’exemple suivant montre comment utiliser ces surcharges.

[!code-cshtml[Main](mvc3-release-notes/samples/sample18.cshtml)]

<a id="_Toc276711791"></a>
### <a name="child-action-output-caching"></a>Action enfant la mise en cache de sortie

Le *OutputCacheAttribute* prend en charge la mise en cache de sortie des actions enfants qui sont appelées à l’aide de la *Html.RenderAction* ou *Html.Action* méthodes d’assistance. L’exemple suivant présente une vue qui appelle une autre action.

[!code-cshtml[Main](mvc3-release-notes/samples/sample19.cshtml)]

Le *GetDate* action est annotée avec le *OutputCacheAttribute*:

[!code-csharp[Main](mvc3-release-notes/samples/sample20.cs)]

Lorsque ce code s’exécute, le résultat de l’appel à Html.Action("GetDate") est mis en cache pendant 100 secondes.

<a id="_Toc276711792"></a>
### <a name="add-view-dialog-box-improvements"></a>Améliorations de boîte de dialogue « Add View »

Lorsque vous ajoutez une vue fortement typée, la boîte de dialogue Ajouter une vue filtre maintenant les types non applicables plus que dans les versions précédentes, telles que de nombreux types de .NET Framework core. En outre, la liste est triée maintenant par le nom de classe et non par le nom de type qualifié complet, ce qui le rend plus facile à trouver des types. Par exemple, le nom de type est maintenant affiché dans l’exemple suivant :

ClassName (espace de noms)

Dans les versions antérieures, cela aurait affiché comme suit :

Namespace.ClassName

<a id="_Toc276711793"></a>
### <a name="granular-request-validation"></a>Validation de la demande granulaire

Le *exclure* propriété du *ValidateInputAttribute* n’existe plus. Pour que la validation de demande ignorée pour les propriétés spécifiques d’un modèle lors de la liaison de modèle, utilisez plutôt la nouvelle *SkipRequestValidationAttribute*.

Par exemple, qu'une méthode d’action permet de modifier un billet de blog :

[!code-csharp[Main](mvc3-release-notes/samples/sample21.cs)]

L’exemple suivant montre le modèle de vue pour un billet de blog.

[!code-csharp[Main](mvc3-release-notes/samples/sample22.cs)]

Lorsqu’un utilisateur envoie des balises pour la propriété de Description, la liaison de modèle échoue en raison de la validation de la demande. Pour désactiver la validation de la demande pendant la liaison de modèle pour la Description du billet de blog, appliquez le *SkipRequpestValidationAttribute* à la propriété, comme illustré dans cet exemple :.

[!code-csharp[Main](mvc3-release-notes/samples/sample23.cs)]

Sinon, pour désactiver la validation de la demande pour chaque propriété du modèle, appliquer *ValidateInputAttribute* avec la valeur *false* à la méthode d’action :

[!code-csharp[Main](mvc3-release-notes/samples/sample24.cs)]

<a id="_Toc276711794"></a>
## <a name="breaking-changes"></a>Modifications avec rupture

- L’ordre d’exécution des filtres d’exception a été modifiée pour les filtres d’exception qui ont le même *ordre* valeur. Dans ASP.NET MVC 2 et versions antérieures, les filtres d’exception sur le contrôleur ayant le même *ordre* comme ceux sur une méthode d’action étaient exécutés avant les filtres d’exception sur la méthode d’action. Cela serait généralement le cas lorsque les filtres d’exception étaient appliqués sans spécifier *ordre* valeur. Dans ASP.NET MVC 3, cette commande a été inversée afin que le Gestionnaire d’exceptions plus spécifique s’exécute en premier. Comme dans les versions antérieures, si le *ordre* propriété est explicitement spécifiée, les filtres sont exécutés dans l’ordre spécifié.
- Ajouter une nouvelle propriété nommée *FileExtensions* à la *VirtualPathProviderViewEngine* classe de base. Lorsque vous recherchez une vue par chemin d’accès (et non pas par nom), seuls les affichages avec une extension de fichier contenues dans la liste spécifiée par cette nouvelle propriété est considérée comme. Il s’agit d’une modification avec rupture pour ceux qui font référence à ces vues à l’aide d’un chemin d’accès complet plutôt qu’un nom et les inscrire un fournisseur de génération personnalisée pour activer une extension de fichier personnalisé pour les vues de formulaire web. La solution de contournement consiste à modifier la valeur de la *FileExtensions* propriété à inclure l’extension de fichier personnalisé.

<a id="_Toc276711795"></a>
## <a name="known-issues"></a>Problèmes connus

- Le programme d’installation peut prendre beaucoup plus de temps que les versions antérieures d’ASP.NET MVC, car elle met à jour les composants de Visual Studio 2010.
- La structure d’ajouter une vue lorsque vous sélectionnez astrongly typée afficher génère automatiquement les propriétés en écriture seule. Il doivent toujours être ignorées par génération de modèles automatique. La boîte de dialogue Ajouter une vue structure également des propriétés en lecture seule lors de la génération d’une vue « Modifier » ou « Créer ». Propriétés en lecture seule doivent uniquement être généré automatiquement pour les vues de liste et d’affichage.
- Le débogage ne fonctionne pas lorsque ASP.NET MVC 3 est installé en même temps que le Async CTP. ASP.NET MVC 3 ne peut pas être installé côte à côte avec le CTP asynchrone. Désinstallez le Async CTP pour réparer le débogage. Pour plus d’informations, consultez [ce billet de blog](http://drew-prog.blogspot.com/2010/11/how-to-uninstall-microsoft-aspnet-mvc-3.html) sur la désinstallation de tous les éléments d’ASP.NET MVC 3 RC.
- Razor Intellisense ne fonctionne pas lorsque Resharper est installé. Si vous avez ReSharper est installé et que vous souhaitez tirer parti de la prise en charge de Razor intellisense dans ASP.NET MVC 3 RC, veuillez lire [ce billet de blog](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) de JetBrains qui décrit les manières de les utiliser aujourd'hui ensemble.
- CSHTML et VBHTML vues créées avec la version bêta d’ASP.NET MVC 3 n’ont pas leur action de génération correctement qui omet les à partir de la publication. Le *Action de génération* pour ces fichiers doivent être définis sur « Contenu ». ASP.NET MVC 3 RC résout ce problème pour les nouveaux fichiers, mais ne résout pas le paramètre pour les fichiers existants pour un projet créé avec la version bêta.
- Le programme d’installation peut prendre beaucoup plus de temps que les versions antérieures d’ASP.NET MVC, car elle met à jour les composants de Visual Studio 2010.
- La structure d’ajouter une vue lorsqu’en sélectionnant une « modification » fortement typées génère automatiquement de vue Propriétés en lecture seule. De même, les propriétés en écriture seule sont généré automatiquement pour les vues de le « Affichage ».
- Pendant l’installation, la boîte de dialogue d’acceptation CLUF affiche les termes du contrat de licence dans une fenêtre plus petite que prévue.
- Installer le Visual Studio Async CTP provoque un conflit avec la version Razor qui est incluse dans le cadre de l’installation des outils ASP.NET MVC 3. Assurez-vous que vous n’essayez pas d’installer le Visual Studio Async CTP et la version Razor sur le même ordinateur.
- Lorsque vous modifiez une vue Razor (.cshtml fichier), l’élément de menu Aller à contrôleur dans Visual Studio ne sera pas disponible, et il en existe aucun extrait de code.

<a id="TOC_ASP_NET_3_Beta"></a>
## <a name="aspnet-mvc-3-beta"></a>Version bêta d’ASP.NET MVC 3

Version bêta d’ASP.NET MVC 3 a été publiée le 6 octobre 2010. Les notes suivantes sont spécifiques à la version bêta et sont soumis à toutes les mises à jour ou modifications figurant dans la section d’ASP.NET MVC 3 Release Candidate ci-dessus.

## <a id="0.1__Toc274034215"></a>  Nouveau bêta d’ASP.NET MVC 3 Featuresin

<a id="0.1__Default_validation_system"></a>Cette section décrit les fonctionnalités qui ont été introduites dans la version de la version bêta d’ASP.NET MVC 3.

### <a id="0.1__Toc274034216"></a>  Gestionnaire de Package NuGet

ASP.NET MVC 3 inclut NuGet Package Manager, qui est un outil de gestion de package intégré pour l’ajout de bibliothèques et outils pour les projets Visual Studio. La plupart du temps, il automatise les étapes que les développeurs entreprendre dès aujourd'hui pour obtenir une bibliothèque dans leur arborescence source.

Vous pouvez utiliser NuGet comme un outil de ligne de commande, comme une fenêtre de console intégrée à l’intérieur de Visual Studio 2010, dans le menu contextuel de Visual Studio et en tant qu’ensemble d’applets de commande PowerShell.

Pour plus d’informations sur NuGet, lisez le [Documentation de NuGet](https://docs.microsoft.com/nuget/).

### <a id="0.1__Toc274034217"></a>  Boîte de dialogue Nouveau projet améliorée

Lorsque vous créez un nouveau projet, la boîte de dialogue Nouveau projet vous permet désormais de spécifier le moteur d’affichage, ainsi qu’un type de projet ASP.NET MVC.

![](mvc3-release-notes/_static/image6.png)

Prise en charge de la modification de la liste de modèles et affichage de moteurs répertoriés dans la boîte de dialogue n’est pas inclus dans cette version.

Les modèles par défaut sont les suivantes :

Vide. Contient un ensemble minimal de fichiers pour un projet ASP.NET MVC, y compris la structure de répertoire par défaut pour les projets ASP.NET MVC, et un petit fichier Site.css qui contient la valeur par défaut que définit le style de ASP.NET MVC et un répertoire de Scripts qui contient les fichiers de JavaScript par défaut.

Application Internet. Contient les fonctionnalités d’exemple qui montre comment utiliser le fournisseur d’appartenances dans ASP.NET MVC.

### <a id="0.1__Toc274034218"></a>  Un moyen simple pour spécifier fortement typées modèles dans les vues Razor

La façon de spécifier le type de modèle pour les vues Razor fortement typées a été simplifiée à l’aide de la nouvelle @model directive pour les vues CSHTML et @ModelType directive pour les vues VBHTML. Dans les versions antérieures d’ASP.NET MVC, vous devez spécifier qu'un modèle fortement typé pour Razor vues de cette façon :

[!code-cshtml[Main](mvc3-release-notes/samples/sample25.cshtml)]

Dans cette version, vous pouvez utiliser la syntaxe suivante :

[!code-cshtml[Main](mvc3-release-notes/samples/sample26.cshtml)]

### <a id="0.1__Toc274034219"></a>  Prise en charge de nouvelles méthodes d’assistance de Pages Web ASP.NET

La nouvelle technologie de Pages Web ASP.NET inclut un ensemble de méthodes d’assistance qui sont utiles pour ajouter les fonctionnalités couramment utilisées pour les vues et contrôleurs. ASP.NET MVC 3 prend en charge à l’aide de ces méthodes d’assistance au sein des contrôleurs et des vues (le cas échéant). Ces méthodes sont contenues dans l’assembly System.Web.Helpers. Le tableau suivant répertorie quelques-unes des méthodes d’assistance ASP.NET Web Pages.

| **Application d’assistance** | **Description** |
| --- | --- |
| Graphique | Affiche un graphique dans une vue. Contient des méthodes telles que Chart.ToWebImage, Chart.Save et Chart.Write. |
| Crypto | Utilise le hachage d’algorithmes pour créer correctement salés et hachés les mots de passe. |
| WebGrid | Restitue une collection d’objets (en règle générale, les données à partir d’une base de données) sous la forme d’une grille. Prend en charge la pagination et le tri. |
| WebImage | Restitue une image. |
| Messagerie Web | Envoie un e-mail. |

Une rubrique de référence rapide qui répertorie les programmes d’assistance et la syntaxe de base est disponible en tant que partie de la documentation de la syntaxe ASP.NET Razor à l’adresse suivante :

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-api-reference](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)

### <a id="0.1__Toc274034220"></a>  Prise en charge l’Injection de dépendance supplémentaire

S’appuyant sur la version d’ASP.NET MVC 3 Preview 1, la version actuelle inclut la prise en charge pour les deux nouveaux services et les quatre services existants et la prise en charge améliorée pour la résolution des dépendances et Common Service Locator.

#### <a name="new-icontrolleractivator-interface-for-fine-grained-controller-instantiation"></a>Nouvelle Interface IControllerActivator lors de l’instanciation du contrôleur de granularité fine

La nouvelle interface IControllerActivator fournit un contrôle plus précis sur la façon dont les contrôleurs sont instanciés par le biais de l’injection de dépendances. L’exemple suivant montre l’interface :

[!code-csharp[Main](mvc3-release-notes/samples/sample27.cs)]

Par opposition au rôle de la fabrique de contrôleurs. Une fabrique de contrôleur est une implémentation de l’interface IControllerFactory, qui est chargé à la fois pour la localisation d’un type de contrôleur et de l’instanciation d’une instance de ce type de contrôleur.

Activateurs de contrôleurs sont uniquement responsables de l’instanciation d’une instance d’un type de contrôleur. Ils n’effectuent pas de la recherche du type de contrôleur. Après avoir localisé un type de contrôleur appropriée, les fabriques de contrôleurs doivent déléguer à une instance de IControllerActivator pour gérer l’instanciation réelle du contrôleur.

La classe de DefaultControllerFactory a un nouveau constructeur qui accepte une instance de IControllerFactory. Cela vous permet d’appliquer l’Injection de dépendances pour gérer cet aspect de création du contrôleur sans avoir à remplacer le comportement de recherche de type de contrôleur par défaut.

#### <a name="iservicelocator-interface-replaced-with-idependencyresolver"></a>Interface de IServiceLocator remplacée par IDependencyResolver

Selon les commentaires de la Communauté, la version de la version bêta d’ASP.NET MVC 3 a remplacé l’utilisation de l’interface IServiceLocator avec une interface de IDependencyResolver épurée spécifique aux besoins d’ASP.NET MVC. L’exemple suivant montre la nouvelle interface :

[!code-csharp[Main](mvc3-release-notes/samples/sample28.cs)]

Dans le cadre de cette modification, la classe ServiceLocator a été également remplacée par la classe DependencyResolver. L’inscription d’un résolveur de dépendance est similaire aux versions antérieures d’ASP.NET MVC :

[!code-csharp[Main](mvc3-release-notes/samples/sample29.cs)]

Les implémentations de cette interface doivent simplement déléguer pour le conteneur d’injection de dépendance sous-jacent à fournir le service inscrit pour le type demandé.

Lorsqu’il n’y a aucun service inscrit du type demandé, ASP.NET MVC attend des implémentations de cette interface pour retourner null à partir de la méthode GetService et retourner une collection vide à partir de GetServices.

La nouvelle classe DependencyResolver vous permet d’enregistrer des classes qui implémentent l’interface Common Service Locator (IServiceLocator) ou la nouvelle interface IDependencyResolver. Pour plus d’informations sur Common Service Locator, consultez [CommonServiceLocator sur GitHub](https://github.com/unitycontainer/commonservicelocator).

<a id="0.1__Breaking_Changes"></a>

#### <a name="new-iviewactivator-interface-for-fine-grained-view-page-instantiation"></a>Nouvelle Interface IViewActivator lors de l’instanciation de Page de vue précis

La nouvelle interface IViewPageActivator fournit un contrôle plus précis sur la façon dont les pages de vue sont instanciés par le biais de l’injection de dépendances. Cela s’applique aux instances de WebFormView et instances de RazorView. L’exemple suivant montre la nouvelle interface :

[!code-csharp[Main](mvc3-release-notes/samples/sample30.cs)]

Ces classes acceptent désormais un argument de constructeur IViewPageActivator, qui permet d’utiliser l’injection de dépendances pour contrôler la façon dont les types ViewPage, ViewUserControl et WebViewPage sont instanciés.

#### <a name="new-dependency-resolver-support-for-existing-services"></a>Dépendance de nouvelle prise en charge par programme de résolution pour les Services existants

La nouvelle version inclut la prise en charge de la résolution des dépendances pour les services suivants :

- Fournisseurs de validation de modèle. Les classes qui implémentent ModelValidatorProvider peuvent être inscrits dans le résolveur de dépendance et le système de les utiliser pour prendre en charge la validation côté client et côté serveur.
- Fournisseur de métadonnées de modèle. Une seule classe qui implémente ModelMetadataProvider peut être inscrits dans le résolveur de dépendance et le système utilisera pour fournir des métadonnées pour les systèmes de création de modèles et validation.
- Fournisseurs de valeurs. Les classes qui implémentent ValueProviderFactory peuvent être inscrits dans le résolveur de dépendance et le système de les utiliser pour créer des fournisseurs de valeurs qui sont consommés par le contrôleur et lors de la liaison de modèle.
- Classeurs de modèles. Les classes qui implémentent IModelBinderProvider peuvent être inscrits dans le résolveur de dépendance et le système de les utiliser pour créer des classeurs de modèles qui sont utilisés par le système de liaison de modèle.

### <a id="0.1__Toc274034221"></a>  Nouvelle prise en charge d’Ajax de basée sur jQuery non obstructive

ASP.NET MVC inclut les méthodes d’assistance Ajax tels que les éléments suivants :

- Ajax.ActionLink
- Ajax.RouteLink
- Ajax.BeginForm
- Ajax.BeginRouteForm

Ces méthodes utilisent JavaScript pour appeler une méthode d’action sur le serveur, plutôt que d’à l’aide d’une publication automatique complète. Cette fonctionnalité a été mis à jour pour tirer parti de jQuery de manière discrète. Au lieu d’intrusive générant des scripts de client inline, ces méthodes d’assistance séparent le comportement à partir du balisage en émettant des attributs HTML5 à l’aide de la *data-ajax* préfixe. Comportement est ensuite appliqué au balisage en référençant les fichiers JavaScript appropriés. Assurez-vous que les fichiers JavaScript suivants sont référencés :

- jQuery-1.4.1.js
- jquery.unobtrusive.ajax.js

Cette fonctionnalité est activée par défaut dans le fichier Web.config dans ASP.NET MVC 3 nouveaux modèles de projet, mais est désactivée par défaut pour les projets existants. Pour plus d’informations, consultez [ajouté des indicateurs de l’application pour la validation côté client et le code JavaScript non obstrusif](#0.1_AddedApplicationWideFlagsForClientValida) plus loin dans ce document.

### <a id="0.1__Toc274034222"></a>  Nouvelle prise en charge pour la Validation jQuery non obstructive

Par défaut, la version bêta d’ASP.NET MVC 3 utilise jQuery validation d’une manière discrète afin d’effectuer la validation côté client. Pour activer la validation client non obstructive, effectuer un appel comme suit dans une vue à partir de :

[!code-csharp[Main](mvc3-release-notes/samples/sample31.cs)]

Cela nécessite que ViewContext.UnobtrusiveJavaScriptEnabled propriété est définie sur true, ce que vous pouvez faire en effectuant l’appel suivant :

[!code-csharp[Main](mvc3-release-notes/samples/sample32.cs)]

Vérifiez également que les fichiers JavaScript suivants sont référencés.

- jQuery-1.4.1.js
- jquery.validate.js
- jquery.validate.unobtrusive.js

Cette fonctionnalité est activée sur par défaut dans le fichier Web.config dans ASP.NET MVC 3 nouveaux modèles de projet, mais est désactivée par défaut pour les projets existants. Pour plus d’informations, consultez [nouveaux indicateurs de l’application pour la validation côté client et le code JavaScript non obstrusif](#0.1_AddedApplicationWideFlagsForClientValida) plus loin dans ce document.

<a id="0.1__Toc274034223"></a>

### <a id="0.1_AddedApplicationWideFlagsForClientValida"></a>  Nouveaux indicateurs de l’Application pour la validation côté Client et le code JavaScript non Obstrusif

Vous pouvez activer ou désactiver la validation côté client et JavaScript non obstrusif globalement à l’aide des membres statiques de la classe HtmlHelper, comme dans l’exemple suivant :

[!code-csharp[Main](mvc3-release-notes/samples/sample33.cs)]

Les modèles de projet par défaut activent JavaScript non obstrusif par défaut. Vous pouvez également activer ou désactiver ces fonctionnalités dans le fichier Web.config racine de votre application en utilisant les paramètres suivants :

[!code-xml[Main](mvc3-release-notes/samples/sample34.xml)]

Étant donné que vous pouvez activer ces fonctionnalités par défaut, les nouvelles surcharges ont été apportées à la classe HtmlHelper qui vous permettre de remplacer les paramètres par défaut, comme indiqué dans les exemples suivants :

[!code-csharp[Main](mvc3-release-notes/samples/sample35.cs)]

Pour la compatibilité descendante, ces deux fonctionnalités sont désactivées par défaut.

### <a id="0.1__Toc274034224"></a>  Nouvelle prise en charge pour le Code qui s’exécute avant l’exécution de vues

Vous pouvez maintenant placer un fichier nommé \_viewstart.cshtml (ou \_viewstart.vbhtml) dans le répertoire Views et ajouter du code qui est partagé entre plusieurs vues dans ce répertoire et ses sous-répertoires. Par exemple, vous pouvez placer le code suivant dans le \_page viewstart.cshtml dans le dossier ~/Views :

[!code-cshtml[Main](mvc3-release-notes/samples/sample36.cshtml)]

Cela définit la page de disposition pour chaque vue dans le dossier Views et tous ses sous-dossiers de manière récursive. Lorsqu’une vue est rendu, le code dans le \_viewstart.cshtml fichier s’exécute avant que le code de la vue s’exécute. Le \_viewstart.cshtml code s’applique à chaque vue dans ce dossier.

Par défaut, le code dans le \_viewstart.cshtml fichier s’applique également aux vues dans n’importe quel sous-dossier. Toutefois, les sous-dossiers individuels peuvent avoir leur propre version de la \_viewstart.cshtml de fichiers ; que les cas, la version locale est prioritaire. Par exemple, pour exécuter le code commun à toutes les vues pour le HomeController, placez un \_fichier viewstart.cshtml dans le dossier ~/Views/Home.

### <a id="0.1__Toc274034225"></a>  Nouvelle prise en charge pour la syntaxe Razor VBHTML

L’aperçu de ASP.NET MVC précédente inclus la prise en charge pour les vues à l’aide de la syntaxe Razor basée sur c#. Ces vues utilisent l’extension de fichier .cshtml. Dans le cadre du travail en cours pour prendre en charge de Razor, la version bêta d’ASP.NET MVC 3 introduit la prise en charge de la syntaxe Razor dans Visual Basic, qui utilise l’extension de fichier .vbhtml.

Pour une introduction à l’utilisation de la syntaxe Visual Basic dans les pages VBHTML, consultez le didacticiel à l’adresse suivante :

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-visual-basic](../web-pages/overview/getting-started/introducing-razor-syntax-vb.md)

### <a id="0.1__Toc274034226"></a>  Contrôle plus granulaire sur ValidateInputAttribute

ASP.NET MVC a toujours inclus la classe ValidateInputAttribute, qui appelle l’infrastructure de validation de demande d’ASP.NET core pour vous assurer que la demande entrante ne contient pas d’entrées potentiellement malveillantes. Par défaut, la validation des entrées est activée. Il est possible de désactiver la validation de demande à l’aide de l’attribut ValidateInputAttribute, comme dans l’exemple suivant :

[!code-csharp[Main](mvc3-release-notes/samples/sample37.cs)]

Toutefois, de nombreuses applications web ont des champs de formulaire individuels qui doivent autoriser le HTML, tandis que les champs restants ne doivent pas. La classe ValidateInputAttribute permet désormais de spécifier une liste de champs qui ne doivent pas être inclus dans la validation de la demande.

Par exemple, si vous développez un moteur de blog, vous souhaiterez autoriser le balisage dans les champs du corps et résumé. Ces champs peuvent être représentées par deux éléments d’entrée, chacun avec un attribut de nom correspondant au nom de propriété (« Corps » et « Résumé »). Pour désactiver la demande de validation pour ces champs spécifier uniquement, les noms (séparées par des virgules) dans la propriété de l’exclusion de la classe ValidateInput, comme dans l’exemple suivant :

[!code-csharp[Main](mvc3-release-notes/samples/sample38.cs)]

### <a id="0.1__Toc274034227"></a>  Programmes d’assistance convertir des traits de soulignement des traits d’union pour les noms d’attribut HTML spécifiés à l’aide d’objets anonymes

Méthodes d’assistance vous permettent de spécifier des paires nom/valeur d’attribut à l’aide d’un objet anonyme, comme dans l’exemple suivant :

[!code-csharp[Main](mvc3-release-notes/samples/sample39.cs)]

Cette approche ne vous permettre d’utiliser des traits d’union dans le nom d’attribut, car un trait d’union ne peut pas être utilisé pour un nom de propriété dans ASP.NET. Toutefois, des traits d’union sont importantes pour les attributs HTML5 personnalisé ; par exemple, HTML5 utilise le préfixe « data- ».

En même temps, des traits de soulignement ne peut pas être utilisés pour les noms d’attribut au format HTML, mais sont valides dans les noms de propriété. Par conséquent, si vous spécifiez des attributs à l’aide d’un objet anonyme, et si les noms d’attribut incluent un trait de soulignement, des méthodes d’assistance convertira les traits de soulignement des traits d’union. Par exemple, la syntaxe d’assistance suivante utilise un trait de soulignement :

[!code-csharp[Main](mvc3-release-notes/samples/sample40.cs)]

L’exemple précédent restitue le balisage suivant lors de l’exécution de l’application d’assistance :

[!code-html[Main](mvc3-release-notes/samples/sample41.html)]

## <a id="0.1__Toc274034228"></a>  Correctifs de bogues

Le modèle d’objet par défaut pour les programmes d’assistance de modèle EditorFor et DisplayFor prend désormais en charge le classement spécifié dans la propriété DisplayAttribute.Order. (Dans les versions précédentes, le paramètre de commande ne était pas utilisé.)

Validation côté client maintenant prend en charge la validation des propriétés substituées qui ont des attributs de validation appliqués.

JsonValueProviderFactory est maintenant inscrit par défaut.

## <a id="0.1__Toc274034229"></a>  Modifications avec rupture

L’ordre d’exécution des filtres d’exception a changé pour les filtres d’exception qui ont la même valeur d’ordre. Dans ASP.NET MVC 2 et versions antérieures, filtres d’exception sur le contrôleur avec le même ordre que ceux sur une méthode d’action étaient exécutés avant les filtres d’exception sur la méthode d’action. Cela serait généralement le cas lorsque les filtres d’exception étaient appliqués sans une valeur d’ordre spécifiée. Dans ASP.NET MVC 3, cette commande a été inversée afin que le Gestionnaire d’exceptions plus spécifique s’exécute en premier. Comme dans les versions antérieures, si la propriété d’ordre est spécifiée explicitement, les filtres sont exécutés dans l’ordre spécifié.

## <a id="0.1__Toc274034230"></a>  Problèmes connus

Pendant l’installation, la boîte de dialogue d’acceptation CLUF affiche les termes du contrat de licence dans une fenêtre plus petite que prévue.

Les vues Razor n’ont pas de prise en charge IntelliSense, ni la coloration syntaxique. Il est prévu que la prise en charge pour la syntaxe Razor dans Visual Studio seront incluse dans le cadre d’une version ultérieure.

Lorsque vous modifiez une vue Razor (fichier CSHTML), le <a id="0.1__Toc224729061"> </a> <a id="0.1__Toc238051347"> </a> élément de menu Aller à contrôleur dans Visual Studio ne sera pas disponible, et il en existe aucun extrait de code.

Lorsque vous utilisez le @model afficher de la syntaxe permettant de spécifier un CSHTML fortement typée, spécifique à la langue des raccourcis pour les types ne sont pas reconnues. Par exemple, @model int ne fonctionnera pas, mais @model Int32 fonctionnera. La solution de contournement pour ce bogue consiste à utiliser le nom de type réel lorsque vous spécifiez le type de modèle.

Lorsque vous utilisez le @model syntaxe permettant de spécifier une vue CSHTML fortement typée (ou @ModelType pour spécifier une vue fortement typée de VBHTML), les types nullable et déclarations de tableau ne sont pas pris en charge. Par exemple, @model int ? n’est pas pris en charge. Au lieu de cela, utilisez `@model Nullable<Int32>`. La syntaxe @model string [] est également pas pris en charge ; utilisez plutôt `@model IList<string>`.

Lorsque vous mettez à niveau un projet ASP.NET MVC 2 vers ASP.NET MVC 3, veillez à ajoutez le code suivant à la section appSettings du fichier Web.config :

[!code-xml[Main](mvc3-release-notes/samples/sample42.xml)]

Il existe un problème connu qui provoque l’authentification par formulaire toujours rediriger les utilisateurs non authentifiés vers ~/Account/connexion, en ignorant le paramètre d’authentification de formulaires utilisé dans le fichier Web.config. La solution de contournement consiste à ajouter le paramètre d’application suivant.

[!code-xml[Main](mvc3-release-notes/samples/sample43.xml)]

## <a id="0.1__Toc274034231"></a>  Exclusion de responsabilité

© 2011 Microsoft Corporation. Tous droits réservés. Ce document est fourni « en tant que-est. » Informations et opinions exprimées dans ce document, y compris les URL et autres références à des sites Web Internet, peuvent changer sans préavis. Vous assumez tous les risques liés à leur utilisation.

Ce document ne vous donne aucun droit légal de propriété intellectuelle quant aux produits Microsoft. Vous pouvez copier et utiliser ce document à titre de référence interne.
