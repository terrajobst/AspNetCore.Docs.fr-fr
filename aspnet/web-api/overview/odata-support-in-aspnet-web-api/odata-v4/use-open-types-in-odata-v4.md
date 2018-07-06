---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: Types ouverts dans OData v4 avec l’API Web ASP.NET | Microsoft Docs
author: microsoft
description: Dans OData v4, un type ouvert est un type de stuctured qui contient les propriétés dynamiques, en plus de toutes les propriétés qui sont déclarées dans la définition de type. Ouvrir...
ms.author: aspnetcontent
ms.date: 09/15/2014
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 560d47e0dc451847311eb9e2327190eed2209546
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832265"
---
<a name="open-types-in-odata-v4-with-aspnet-web-api"></a>Types ouverts dans OData v4 avec l’API Web ASP.NET
====================
par [Microsoft](https://github.com/microsoft)

> Dans OData v4, un *ouvrir type* est un type stuctured qui contient les propriétés dynamiques, en plus de toutes les propriétés qui sont déclarées dans la définition de type. Les types ouverts vous permettent d’ajouter de la flexibilité à vos modèles de données. Ce didacticiel montre comment utiliser les types ouverts dans ASP.NET Web API OData.
> 
> Ce didacticiel suppose que vous savez déjà comment créer un point de terminaison OData dans l’API Web ASP.NET. Dans le cas contraire, commencez par lire [créer un point de terminaison OData v4](create-an-odata-v4-endpoint.md) première.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
> 
> 
> - API Web OData 5.3
> - OData v4


Tout d’abord, OData définit la terminologie :

- Type d’entité : un type structuré avec une clé.
- Type complexe : un type structuré sans une clé.
- Type ouvert : un type avec des propriétés dynamiques. Types d’entités et les types complexes peuvent être ouverts.

La valeur d’une propriété dynamique peut être un type primitif, un type complexe ou un type d’énumération ; ou une collection d’une de ces types. Pour plus d’informations sur les types ouverts, consultez la [OData v4 spécification](http://www.odata.org/documentation/odata-version-4-0/).

## <a name="install-the-web-odata-libraries"></a>Installation des bibliothèques OData Web

Utilisez le Gestionnaire de Package NuGet pour installer les dernières bibliothèques d’API Web OData. À partir de la fenêtre de Console du Gestionnaire de Package :

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a>Définir les Types CLR

Commencez par définir les modèles EDM en tant que types CLR.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

Lorsque le modèle EDM (Entity Data Model) est créé,

- `Category` est un type d’énumération.
- `Address` est un type complexe. (Il n’a pas une clé, il n’est pas un type d’entité.)
- `Customer` est un type d’entité. (Elle a une clé).
- `Press` est un type complex ouvert.
- `Book` est un type d’entité ouverte.

Pour créer un type ouvert, le type CLR doit avoir une propriété de type `IDictionary<string, object>`, qui contient les propriétés dynamiques.

## <a name="build-the-edm-model"></a>Générer le modèle EDM

Si vous utilisez **ODataConventionModelBuilder** pour créer le modèle EDM, `Press` et `Book` sont automatiquement ajoutés en tant que types ouverts, en fonction de la présence d’un `IDictionary<string, object>` propriété.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

Vous pouvez également créer le modèle EDM explicitement, à l’aide de **ODataModelBuilder**.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a>Ajouter un contrôleur OData

Ensuite, ajoutez un contrôleur OData. Pour ce didacticiel, nous allons utiliser un contrôleur simplifié que prend en charge uniquement GET et POST demande et utilise une liste en mémoire pour stocker des entités.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

Notez que la première `Book` instance ne possède aucune propriété dynamique. La seconde `Book` instance a les propriétés dynamiques suivantes :

- « Publié » : type primitif
- « Auteurs » : Collection de types primitifs
- « OtherCategories » : Collection de types d’énumération.

En outre, le `Press` propriété cela `Book` instance a les propriétés dynamiques suivantes :

- « Blog » : type primitif
- « Address » : type complexe

## <a name="query-the-metadata"></a>Interroger les métadonnées

Pour obtenir le document de métadonnées OData, envoyez une demande GET à `~/$metadata`. Le corps de réponse doit ressembler à ceci :

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

À partir du document de métadonnées, vous pouvez voir que :

- Pour le `Book` et `Press` types, la valeur de la `OpenType` attribut a la valeur true. Le `Customer` et `Address` types n’ont pas cet attribut.
- Le `Book` type d’entité a trois propriétés déclarées : ISBN, titre, puis appuyez sur. Les métadonnées OData n’incluent pas le `Book.Properties` propriété à partir de la classe CLR.
- De même, le `Press` type complexe a uniquement deux propriétés déclarées : nom et une catégorie. Les métadonnées n’incluent pas le `Press.DynamicProperties` propriété à partir de la classe CLR.

## <a name="query-an-entity"></a>Interroger une entité

Pour obtenir le livre avec ISBN égal à « 978-0-7356-7942-9 », envoyer une demande GET à `~/Books('978-0-7356-7942-9')`. Le corps de réponse doit ressembler à ce qui suit. (Mis en retrait pour le rendre plus lisible).

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

Notez que les propriétés dynamiques sont incluses inline avec les propriétés déclarées.

## <a name="post-an-entity"></a>VALIDER une entité

Pour ajouter une entité de livre, envoyer une demande POST à `~/Books`. Le client peut définir des propriétés dynamiques dans la charge utile de demande.

Voici un exemple de demande. Notez les propriétés « Price » et « Publié ».

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

Si vous définissez un point d’arrêt dans la méthode de contrôleur, vous pouvez voir que les API Web ajouté ces propriétés pour le `Properties` dictionnaire.

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a>Ressources supplémentaires

[Exemple de Type OData Open](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
