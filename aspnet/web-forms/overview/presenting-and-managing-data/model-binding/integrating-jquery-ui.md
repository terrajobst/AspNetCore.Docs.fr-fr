---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: L’intégration de sélecteur de dates JQuery UI avec liaison de modèle et web forms | Microsoft Docs
author: Rick-Anderson
description: Cette série de didacticiels montre les aspects de base de l’utilisation de la liaison de modèle avec un projet Web Forms ASP.NET. Liaison de modèle rend l’interaction des données plus simple-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: ff1b17295c58d40d55bdcd4346b83121b579bb4c
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020557"
---
<a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a>L’intégration de sélecteur de dates JQuery UI avec liaison de modèle et web forms
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> Cette série de didacticiels montre les aspects de base de l’utilisation de la liaison de modèle avec un projet Web Forms ASP.NET. Liaison de modèle rend interaction des données plus simple que vous traitez des données des objets de source (tels que ObjectDataSource ou SqlDataSource). Cette série commence par une partie introductive et progresse vers des concepts plus avancés dans les didacticiels suivants.
> 
> Ce didacticiel montre comment ajouter la JQuery UI [widget sélecteur](http://jqueryui.com/datepicker/) à un formulaire Web et utiliser le modèle de liaison pour mettre à jour de la base de données avec la valeur sélectionnée.
> 
> Ce didacticiel s’appuie sur le projet créé dans le [première](retrieving-data.md) et [deuxième](updating-deleting-and-creating-data.md) parties de la série.
> 
> Vous pouvez [télécharger](https://go.microsoft.com/fwlink/?LinkId=286116) le projet complet en c# ou VB. Le code téléchargeable fonctionne avec Visual Studio 2012 ou Visual Studio 2013. Elle utilise le modèle Visual Studio 2012, qui est légèrement différent de celle du modèle de Visual Studio 2013 présentée dans ce didacticiel.


## <a name="what-youll-build"></a>Vous allez générer

Dans ce didacticiel, vous allez :

1. Ajouter une propriété à votre modèle pour enregistrer la date d’inscription de l’étudiant
2. Activer l’utilisateur de sélectionner la date d’inscription à l’aide du widget sélecteur de dates JQuery UI
3. Appliquer les règles de validation pour la date d’inscription

Le widget sélecteur de dates JQuery UI permet aux utilisateurs de facilement sélectionner une date dans le calendrier qui s’affiche lorsque l’utilisateur interagit avec le champ. À l’aide de ce widget peut être plus pratique pour les utilisateurs que manuellement en tapant une date. Intégration du widget sélecteur de dates dans une page qui utilise la liaison de modèle pour les opérations de données requiert uniquement une petite quantité de travail supplémentaire.

## <a name="add-a-new-property-to-the-model"></a>Ajouter une nouvelle propriété au modèle

Tout d’abord, vous allez ajouter un **Datetime** propriété à vos étudiants modéliser et de migrer cette modification à la base de données. Ouvrez **UniversityModels.cs**et ajoutez le code en surbrillance vers le modèle Student.

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

Le **RangeAttribute** est inclus afin d’appliquer les règles de validation pour la propriété. Pour ce didacticiel, nous supposerons que Contoso University fondé le 1er janvier 2013 et par conséquent, les dates d’inscription antérieures ne sont pas valides.

Dans la fenêtre de gestion des packages, ajoutez une migration en exécutant la commande **migration ajouter AddEnrollmentDate**. Notez que le code de migration ajoute la nouvelle colonne de date/heure à la table Student. Pour faire correspondre la valeur que vous avez spécifié dans le RangeAttribute, ajoutez une valeur par défaut pour la nouvelle colonne, comme indiqué dans le code en surbrillance ci-dessous.

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

Enregistrez vos modifications dans le fichier de migration.

Vous n’avez pas besoin alimenter les données à nouveau. Par conséquent, ouvrez **Configuration.cs** dans le dossier Migrations et supprimez ou commentez le code dans le **Seed** (méthode). Enregistrez et fermez le fichier.

Maintenant, exécutez la commande **mise à jour la base de données**. Notez que la colonne existe maintenant dans la base de données et tous les enregistrements existants ont la valeur par défaut pour EnrollmentDate.

## <a name="add-dynamic-controls-for-enrollment-date"></a>Ajouter des contrôles dynamiques pour la date d’inscription

Vous allez maintenant ajouter des contrôles pour afficher et modifier la date d’inscription. À ce stade, la valeur est modifiée via une zone de texte. Plus loin dans ce didacticiel, vous allez modifier la zone de texte pour le widget de JQuery Datepicker.

Tout d’abord, il est important de noter que vous n’avez pas besoin d’apporter de modifications à la **AddStudent.aspx** fichier. Le contrôle DynamicEntity affiche automatiquement la nouvelle propriété.

Ouvrez **Students.aspx**et ajoutez le code en surbrillance suivant.

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

Exécutez l’application et notez que vous pouvez définir la valeur de la date d’inscription en tapant une date. Lorsque vous ajoutez un nouvel étudiant :

![définir la date](integrating-jquery-ui/_static/image1.png)

Ou bien, la modification d’une valeur existante :

![modifier la date](integrating-jquery-ui/_static/image2.png)

Taper la date, mais cela est peut-être pas l’expérience client que vous souhaitez fournir. Dans la section suivante, vous allez activer la sélection d’une date dans un calendrier.

## <a name="install-nuget-package-to-work-with-jquery-ui"></a>Installer le package NuGet pour travailler avec JQuery UI

Le **l’interface utilisateur de jus** package NuGet s’intègre facilement des widgets JQuery UI dans votre application web. Pour utiliser ce package, vous devez l’installer via NuGet.

![Ajouter une interface utilisateur de jus](integrating-jquery-ui/_static/image3.png)

La version de l’interface utilisateur de jus que vous installez peut entrer en conflit avec la version de JQuery dans votre application. Avant de poursuivre ce didacticiel, essayez d’exécuter votre application. Si vous rencontrez une erreur JavaScript, vous devez rapprocher de la version de JQuery. Vous pouvez ajouter la version attendue de JQuery dans votre dossier de Scripts (version 1.8.2 au moment de la rédaction de ce didacticiel), ou dans Site.master, spécifiez le chemin d’accès au fichier JQuery.

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a>Personnaliser le modèle de date/heure pour inclure le widget sélecteur

Vous allez ajouter le widget sélecteur de dates pour le modèle de données dynamiques pour la modification d’une valeur datetime. En ajoutant le widget au modèle, il est affiché automatiquement dans les deux le formulaire pour ajouter un nouvel étudiant et dans l’affichage de grille pour les étudiants de modifications. Ouvrez **DateTime\_Data Edit.ascx**et ajoutez le code en surbrillance suivant.

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

Dans le fichier code-behind, vous allez définir les dates minimales et maximales pour le sélecteur de dates. En définissant ces valeurs, vous empêchera les utilisateurs d’accéder à des dates non valides. Vous allez récupérer les valeurs minimales et maximales de la **RangeAttribute** sur la propriété de date/heure, le cas échéant. Ouvrez **DateTime\_Edit.ascx.cs**et ajoutez le code en surbrillance suivant à la Page\_méthode de charge.

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

Exécutez l’application web et accédez à la page AddStudent. Fournir des valeurs pour les champs et remarquez que lorsque vous cliquez sur la zone de texte pour la Date d’inscription, le calendrier s’affiche.

![Sélecteur de dates](integrating-jquery-ui/_static/image4.png)

Choisir une date, puis cliquez sur **insérer**. Le RangeAttribute effectue la validation sur le serveur. En définissant la propriété minDate Datepicker, vous appliquez également la validation sur le client. Le calendrier ne permet pas l’utilisateur de naviguer jusqu'à une date avant la valeur de minDate.

Lorsque vous modifiez un enregistrement dans l’affichage de grille, le calendrier s’affiche également.

![DatePicker dans GridView](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a>Conclusion

Dans ce didacticiel, vous avez appris à intégrer un widget de JQuery à un formulaire web qui utilise la liaison de modèle.

Dans la prochaine [didacticiel](using-query-string-values-to-retrieve-data.md), vous allez utiliser une valeur de chaîne de requête lors de la sélection de données.

> [!div class="step-by-step"]
> [Précédent](sorting-paging-and-filtering-data.md)
> [Suivant](using-query-string-values-to-retrieve-data.md)
