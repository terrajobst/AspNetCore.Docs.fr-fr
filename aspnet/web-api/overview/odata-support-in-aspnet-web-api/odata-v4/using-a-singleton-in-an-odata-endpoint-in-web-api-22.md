---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: Créer un Singleton dans OData v4 à l’aide de Web API 2.2 | Microsoft Docs
author: rick-anderson
description: Cette rubrique montre comment définir un singleton dans un point de terminaison OData dans Web API 2.2.
ms.author: aspnetcontent
ms.date: 06/27/2014
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: baccfed79949ae4efd45395bed3ad0058549cb4c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830514"
---
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a>Créer un Singleton dans OData v4 à l’aide de Web API 2.2
====================
par Zoe Luo

> En règle générale, une entité est uniquement accessible si elle a été encapsulée dans un jeu d’entités. Mais OData v4 fournit deux options supplémentaires, Singleton et relation contenant-contenu, les deux API Web 2.2 prend en charge.


Cet article explique comment définir un singleton dans un point de terminaison OData dans Web API 2.2. Pour plus d’informations sur quelles un singleton est et comment vous pouvez tirer parti de l’utiliser, consultez [à l’aide d’un singleton pour définir votre entité spéciale](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx). Pour créer un point de terminaison OData V4 dans l’API Web, consultez [créer une Using ASP.NET Web API 2.2 OData v4 point de terminaison](create-an-odata-v4-endpoint.md). 

Nous allons créer un singleton dans votre projet d’API Web à l’aide du modèle de données suivantes :

![Modèle de données](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

Un singleton nommé `Umbrella` seront définis en fonction de type `Company`et un jeu nommé d’entités `Employees` seront définis en fonction de type `Employee`.

La solution utilisée dans ce didacticiel peut être téléchargée à partir de [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).

## <a name="define-the-data-model"></a>Définir le modèle de données

1. Définir les types CLR.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. Générer le modèle EDM basé sur les types CLR.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    Ici, `builder.Singleton<Company>("Umbrella")` indique au Générateur de modèle pour créer un singleton nommé `Umbrella` dans le modèle EDM.

    Les métadonnées générées ressemblera à ce qui suit :

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    À partir des métadonnées, nous pouvons voir que la propriété de navigation `Company` dans le `Employees` jeu d’entités est lié au singleton `Umbrella`. La liaison est effectuée automatiquement par `ODataConventionModelBuilder`, dans la mesure où seuls `Umbrella` a le `Company` type. S’il existe toute ambiguïté dans le modèle, vous pouvez utiliser `HasSingletonBinding` lier explicitement une propriété de navigation à un singleton ; `HasSingletonBinding` a le même effet que l’utilisation de la `Singleton` attribut dans la définition de type CLR :

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a>Définir le contrôleur de singleton

Comme le contrôleur EntitySet, le contrôleur de singleton hérite `ODataController`, et le nom du contrôleur singleton doit être `[singletonName]Controller`.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

Afin de gérer différents types de demandes, les actions sont nécessaires pour être prédéfinies dans le contrôleur. **Routage par attributs** est activée par défaut dans l’API Web 2.2. Par exemple, pour définir une action pour gérer l’interrogation `Revenue` de `Company` à l’aide du routage par attributs, utilisez la commande suivante :

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

Si vous n’êtes pas disposé à définir des attributs pour chaque action, définir vos actions suivant [Conventions de routage OData](../odata-routing-conventions.md). Dans la mesure où une clé n’est pas requise pour l’interrogation d’un singleton, les actions définies dans le contrôleur de singleton sont légèrement différentes de ceux indiqués dans le contrôleur entityset.

Pour référence, les signatures de méthode pour chaque définition de l’action dans le contrôleur de singleton sont répertoriées ci-dessous.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

Cela est en fait, il que vous suffit de faire du côté service. Le [exemple de projet](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contient tout le code pour la solution et le client OData qui montre comment utiliser le singleton. Le client est créé en suivant les étapes décrites dans [créer une application cliente OData v4](create-an-odata-v4-client-app.md).

. 

*Merci à Leo Hu pour le contenu d’origine de cet article.*
