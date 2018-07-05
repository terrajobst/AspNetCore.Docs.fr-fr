---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
title: Bien démarrer avec Entity Framework 4.0 Database First et 4 d’ASP.NET Web Forms - partie 8 | Microsoft Docs
author: tdykstra
description: L’exemple d’application web Contoso University montre comment créer des applications Web Forms ASP.NET à l’aide d’Entity Framework. L’exemple d’application est en cours...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: aaadd9bb-5508-448c-ad57-5497dff90e13
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
msc.type: authoredcontent
ms.openlocfilehash: f5ad2c1caf6036a0d8ee2ebbd07de60009090f1b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374050"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-8"></a>Bien démarrer avec Entity Framework 4.0 Database First et 4 les Web Forms ASP.NET - partie 8
====================
par [Tom Dykstra](https://github.com/tdykstra)

> L’exemple d’application web Contoso University montre comment créer des applications Web Forms ASP.NET à l’aide de l’Entity Framework 4.0 et Visual Studio 2010. Pour plus d’informations sur la série de didacticiels, consultez [le premier didacticiel de la série](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-dynamic-data-functionality-to-format-and-validate-data"></a>À l’aide des fonctionnalités Dynamic Data pour mettre en forme et valider des données

Dans le didacticiel précédent, vous avez implémenté les procédures stockées. Ce didacticiel vous explique comment les fonctionnalités de données dynamiques peuvent offrir les avantages suivants :

- Les champs sont automatiquement mis en forme pour affichage en fonction de leur type de données.
- Les champs sont automatiquement validées en fonction de leur type de données.
- Vous pouvez ajouter des métadonnées au modèle de données pour personnaliser le comportement de mise en forme et validation. Lorsque vous faites pas, vous pouvez ajouter les règles de mise en forme et validation dans un seul endroit, et ils sont appliqués automatiquement partout vous accéder aux champs à l’aide des contrôles Dynamic Data.

Pour voir comment cela fonctionne, vous modifierez les contrôles que vous utilisez pour afficher et modifier les champs dans l’espace *Students.aspx* page et vous allez ajouter des métadonnées de mise en forme et de validation pour les champs nom et date de la `Student` type d’entité.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)

## <a name="using-dynamicfield-and-dynamiccontrol-controls"></a>À l’aide de DynamicField et contrôles de DynamicControl

Ouvrir le *Students.aspx* page puis, dans le `StudentsGridView` remplacement du contrôle le **nom** et **Date d’inscription** `TemplateField` éléments avec le balisage suivant :

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample1.aspx)]

Ce balisage utilise `DynamicControl` contrôle à la place de `TextBox` et `Label` champ de modèle de nom de contrôles dans l’étudiant, et il utilise un `DynamicField` contrôle pour la date d’inscription. Aucune chaîne de format n’est spécifiés.

Ajouter un `ValidationSummary` de contrôle après la `StudentsGridView` contrôle.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample2.aspx)]

Dans le `SearchGridView` contrôle Remplacez le balisage pour le **nom** et **Date d’inscription** colonnes comme vous l’avez fait dans le `StudentsGridView` contrôler, à ceci près omettre la `EditItemTemplate` élément. Le `Columns` élément de la `SearchGridView` contrôle contient maintenant le balisage suivant :

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample3.aspx)]

Ouvrez *Students.aspx.cs* et ajoutez le code suivant `using` instruction :

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample4.cs)]

Ajoutez un gestionnaire pour la page `Init` événement :

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample5.cs)]

Ce code spécifie que les données dynamiques fournirez mise en forme et validation dans ces contrôles liés aux données pour les champs de la `Student` entité. Si vous obtenez un message d’erreur comme dans l’exemple suivant lorsque vous exécutez la page, cela signifie généralement que vous avez oublié d’appeler le `EnableDynamicData` méthode dans `Page_Init`:

`Could not determine a MetaTable. A MetaTable could not be determined for the data source 'StudentsEntityDataSource' and one could not be inferred from the request URL.`

Exécutez la page.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)

Dans le **Date d’inscription** colonne, le temps s’affiche, ainsi que la date, car le type de propriété est `DateTime`. Vous corrigerez cela plus tard.

Pour l’instant, notez que Dynamic Data assure automatiquement la validation de la base de données. Par exemple, cliquez sur **modifier**, effacez le champ de date, cliquez sur **mise à jour**, et vous voyez que Dynamic Data rend cela un champ obligatoire, car la valeur n’est pas nullable dans le modèle de données. La page affiche un astérisque après le champ et un message d’erreur dans le `ValidationSummary` contrôle :

[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)

Vous pourriez omettre le `ValidationSummary` contrôler, étant donné que vous pouvez également maintenir le pointeur de la souris sur l’astérisque pour voir le message d’erreur :

[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)

Dynamic Data valide également que les données entrées dans le **Date d’inscription** champ est une date valide :

[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)

Comme vous pouvez le voir, il s’agit d’un message d’erreur générique. Dans la section suivante, vous verrez comment personnaliser des messages, ainsi que la validation et les règles de mise en forme.

## <a name="adding-metadata-to-the-data-model"></a>Ajout de métadonnées pour le modèle de données

En règle générale, vous souhaitez personnaliser les fonctionnalités fournies par Dynamic Data. Par exemple, vous pouvez modifier l’affichage des données et le contenu des messages d’erreur. Vous généralement également Personnalisez les règles de validation de données pour fournir plus de fonctionnalités que ce qui fournit des données dynamiques automatiquement en fonction des types de données. Pour ce faire, vous créez des classes partielles qui correspondent aux types d’entité.

Dans **l’Explorateur de solutions**, avec le bouton droit le **ContosoUniversity** projet, sélectionnez **ajouter une référence**et ajoutez une référence à `System.ComponentModel.DataAnnotations`.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)

Dans le *DAL* dossier, créez un fichier de classe, nommez-le *Student.cs*et remplacez le code du modèle qu’il contient par le code suivant.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample6.cs)]

Ce code crée une classe partielle pour le `Student` entité. Le `MetadataType` attribut appliqué à cette classe partielle identifie la classe que vous utilisez pour spécifier les métadonnées. La classe de métadonnées peut avoir n’importe quel nom, mais à l’aide du nom de l’entité plus « Métadonnées » est une pratique courante.

Les attributs appliqués aux propriétés dans la classe de métadonnées spécifient la mise en forme, les messages d’erreur, les règles et la validation. Les attributs illustrés ici aura les résultats suivants :

- `EnrollmentDate` affiche en tant que date (sans heure).
- Les deux champs de nom doivent être de 25 caractères ou inférieure en longueur et un message d’erreur personnalisé est fourni.
- Les deux champs de nom sont obligatoires, et un message d’erreur personnalisé est fourni.

Exécutez le *Students.aspx* page à nouveau, et vous voyez que les dates sont maintenant affichés sans les heures :

[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)

Modifier une ligne et essayer d’effacer les valeurs dans les champs de nom. Les astérisques qui indiquent des erreurs de champ apparaissent dès que vous laissez un champ, avant de cliquer sur **mise à jour**. Lorsque vous cliquez sur **mise à jour**, la page affiche le texte de message d’erreur que vous avez spécifié.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)

Essayez d’entrer des noms de plus de 25 caractères, cliquez sur **mise à jour**, et la page affiche le texte de message d’erreur que vous avez spécifié.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)

Maintenant que vous avez configuré ces règles de mise en forme et validation dans les métadonnées de modèle de données, les règles seront automatiquement appliquées sur chaque page qui affiche ou permet de modifier ces champs, tant que vous utilisez `DynamicControl` ou `DynamicField` contrôles. Cela réduit la quantité de code redondant, que vous devez écrire, ce qui rend la programmation et le test, et elle garantit que la mise en forme de données et de validation sont cohérentes dans une application.

## <a name="more-information"></a>Informations complémentaires

Ceci conclut cette série de didacticiels sur la mise en route avec Entity Framework. Pour plus de ressources pour vous aider à apprendre à utiliser Entity Framework, poursuivez [le premier didacticiel de la prochaine série de didacticiels Entity Framework](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) ou visitez les sites suivants :

- [Entity Framework Forum aux questions](http://www.ef-faq.org/introduction.html)
- [Blog de l’équipe Entity Framework](https://blogs.msdn.com/b/adonet/)
- [Entity Framework dans MSDN Library](https://msdn.microsoft.com/library/bb399572.aspx)
- [Entity Framework dans le centre de développement de données MSDN](https://msdn.microsoft.com/data/ef.aspx)
- [Vue d’ensemble du contrôle serveur Web EntityDataSource dans MSDN Library](https://msdn.microsoft.com/library/cc488502.aspx)
- [Contrôle EntityDataSource référence de l’API dans la bibliothèque MSDN](https://msdn.microsoft.com/library/system.web.ui.webcontrols.entitydatasource.aspx)
- [Entity Framework Forums sur MSDN](https://social.msdn.microsoft.com/forums/adodotnetentityframework/)
- [Blog de Julie](http://thedatafarm.com/blog/)

> [!div class="step-by-step"]
> [Précédent](the-entity-framework-and-aspnet-getting-started-part-7.md)
