---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'Tutoriel : Améliorer la validation des données pour EF Database First avec une application ASP.NET MVC'
description: Ce didacticiel se concentre sur l’ajout d’annotations de données au modèle de données pour spécifier les exigences de validation et afficher la mise en forme.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 897cd7c6a40445e2a4abede50d81e101372d3233
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667620"
---
# <a name="tutorial-enhance-data-validation-for-ef-database-first-with-aspnet-mvc-app"></a>Tutoriel : Améliorer la validation des données pour EF Database First avec une application ASP.NET MVC

À l’aide de la structure ASP.NET MVC et Entity Framework, vous pouvez créer une application web qui fournit une interface à une base de données existante. Cette série de didacticiels vous montre comment générer du code qui permet aux utilisateurs d’afficher, modifier, créer et supprimer automatiquement les données qui résident dans une table de base de données. Le code généré correspond aux colonnes dans la table de base de données.

Ce didacticiel se concentre sur l’ajout d’annotations de données au modèle de données pour spécifier les exigences de validation et afficher la mise en forme. Il a été amélioré en se basant sur les commentaires des utilisateurs dans la section commentaires.

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Ajouter des annotations de données
> * Ajouter des classes de métadonnées

## <a name="prerequisites"></a>Prérequis

* [Personnaliser un affichage](customizing-a-view.md)

## <a name="add-data-annotations"></a>Ajouter des annotations de données

Comme vous l’avez vu dans une rubrique précédente, certaines règles de validation de données sont automatiquement appliquées à l’entrée utilisateur. Par exemple, vous pouvez fournir uniquement un nombre pour la propriété de classe. Pour spécifier plusieurs règles de validation de données, vous pouvez ajouter des annotations de données à votre classe de modèle. Ces annotations sont appliquées dans toute votre application web pour la propriété spécifiée. Vous pouvez également appliquer des attributs de mise en forme qui modifient la façon dont les propriétés sont affichées ; comme la modification de la valeur utilisée pour les étiquettes de texte.

Dans ce didacticiel, vous allez ajouter des annotations de données pour limiter la longueur des valeurs fournies pour les propriétés FirstName, LastName et MiddleName. Dans la base de données, ces valeurs sont limitées à 50 caractères ; Toutefois, dans votre application web cette limite de caractère n’est actuellement pas appliquée. Si un utilisateur fournit plus de 50 caractères pour l’une de ces valeurs, la page se bloque lorsque vous tentez d’enregistrer la valeur dans la base de données. Vous allez également restreindre le niveau pour les valeurs comprises entre 0 et 4.

Sélectionnez **modèles** > **ContosoModel.edmx** > **ContosoModel.tt** et ouvrez le *Student.cs* fichier. Ajoutez le code en surbrillance suivant à la classe.

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

Ouvrez *Enrollment.cs* et ajoutez le code en surbrillance suivant.

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

Générez la solution.

Cliquez sur **liste d’étudiants** et sélectionnez **modifier**. Si vous essayez d’entrer plus de 50 caractères, un message d’erreur s’affiche.

![afficher le message d’erreur](enhancing-data-validation/_static/image1.png)

Revenez à la page d’accueil. Cliquez sur **liste des inscriptions** et sélectionnez **modifier**. Essaie de fournir un niveau au-dessus de 4. Vous recevez cette erreur : *Le champ niveau doit être comprise entre 0 et 4.*

## <a name="add-metadata-classes"></a>Ajouter des classes de métadonnées

Ajout d’attributs de validation directement à la classe de modèle fonctionne lorsque vous ne prévoyez pas de la base de données pour modifier ; Toutefois, si vos modifications de base de données et vous devez régénérer la classe de modèle, vous allez perdre tous les attributs que vous aviez appliqué à la classe de modèle. Cette approche peut être très inefficace et sujette à la perte de règles de validation important.

Pour éviter ce problème, vous pouvez ajouter une classe de métadonnées qui contient les attributs. Lorsque vous associez la classe de modèle à la classe de métadonnées, ces attributs sont appliqués au modèle. Dans cette approche, la classe de modèle peut être régénérée sans perdre tous les attributs qui ont été appliqués à la classe de métadonnées.

Dans le **modèles** dossier, ajoutez une classe nommée *Metadata.cs*.

Remplacez le code dans *Metadata.cs* par le code suivant.

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

Ces classes de métadonnées contient tous les attributs de validation que vous aviez précédemment appliqués aux classes de modèle. Le **affichage** attribut est utilisé pour modifier la valeur utilisée pour les étiquettes de texte.

Maintenant, vous devez associer les classes de modèle avec les classes de métadonnées.

Dans le **modèles** dossier, ajoutez une classe nommée *PartialClasses.cs*.

Remplacez le contenu du fichier par le code suivant.

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

Notez que chaque classe est marquée comme un `partial` classe et chacune correspond au nom et l’espace de noms que la classe qui est générée automatiquement. En appliquant l’attribut de métadonnées à la classe partielle, vous assurer que les attributs de validation de données seront être appliqués à la classe générée automatiquement. Ces attributs ne seront pas perdues lorsque vous régénérez les classes de modèle, car l’attribut de métadonnées est appliqué dans des classes partielles qui ne sont pas régénérées.

Pour régénérer les classes générées automatiquement, ouvrez le *ContosoModel.edmx* fichier. Une fois encore, avec le bouton droit sur l’aire de conception et sélectionnez **modèle de mise à jour à partir de la base de données**. Même si vous n’avez pas modifié la base de données, ce processus sera régénérer les classes. Dans le **Actualiser** onglet, sélectionnez **Tables** et **Terminer**.

Enregistrer le *ContosoModel.edmx* fichier pour appliquer les modifications.

Ouvrez le *Student.cs* fichier ou le *Enrollment.cs* fichier et notez que les attributs de validation de données vous avez appliqué précédemment ne sont plus dans le fichier. Toutefois, exécutez l’application et notez que les règles de validation sont toujours appliqués lorsque vous entrez des données.

## <a name="conclusion"></a>Conclusion

Cette série fourni un exemple simple de la génération de code à partir d’une base de données existante qui permet aux utilisateurs de modifier, mettre à jour, créer et supprimer des données. Il utilisé ASP.NET MVC 5, Entity Framework et génération de modèles automatique ASP.NET pour créer le projet. 

Pour obtenir un exemple d’introduction du développement Code First, consultez [mise en route avec ASP.NET MVC 5](../introduction/getting-started.md). 

Pour un exemple plus avancé, consultez [création d’un modèle de données Entity Framework pour une application ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Notez que l’API DbContext que vous utilisez pour l’utilisation des données dans la première base de données est identique à l’API que vous utilisez pour travailler avec des données dans le Code First. Même si vous envisagez d’utiliser la première base de données, vous pouvez apprendre à gérer des scénarios plus complexes tels que la lecture et la mise à jour des données connexes, gestion des conflits d’accès concurrentiel, et ainsi de suite à partir d’un didacticiel de Code First. La seule différence est dans la base de données, classe de contexte, les classes d’entité sont créés.

## <a name="additional-resources"></a>Ressources supplémentaires

Pour obtenir une liste complète des annotations de validation de données que vous pouvez appliquer aux classes et propriétés, consultez [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Annotations de données ajoutées
> * Classes de métadonnées ajoutés

Pour savoir comment déployer une application web et la base de données SQL dans Azure App Service, consultez ce didacticiel :
> [!div class="nextstepaction"]
> [Déployer une application .NET sur Azure App Service](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase/)
