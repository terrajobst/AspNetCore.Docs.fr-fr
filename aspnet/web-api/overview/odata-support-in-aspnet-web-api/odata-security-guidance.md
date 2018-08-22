---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: Conseils de sécurité pour ASP.NET Web API 2 OData | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 02/06/2013
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 2a5b776a81cb3e3cf809dd3c4229448988086a32
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823609"
---
<a name="security-guidance-for-aspnet-web-api-2-odata"></a>Conseils de sécurité pour ASP.NET Web API 2 OData
====================
par [Mike Wasson](https://github.com/MikeWasson)

Cette rubrique décrit certains des problèmes de sécurité dont vous devez tenir compte lorsque vous exposez via OData, un jeu de données.

## <a name="edm-security"></a>Sécurité du modèle EDM

La sémantique de requête est basée sur l’entity data model (EDM), pas les types de modèle sous-jacent. Vous pouvez exclure une propriété de l’EDM, et il ne sera pas visible à la requête. Par exemple, supposons que votre modèle inclut un type d’employé avec une propriété de salaire. Vous souhaiterez peut-être exclure cette propriété de l’EDM pour la masquer à partir de clients.

Il existe deux façons d’exclure une propriété à partir du modèle EDM. Vous pouvez définir le **[IgnoreDataMember]** attribut sur la propriété dans la classe de modèle :

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

Vous pouvez également supprimer par programmation la propriété à partir du modèle EDM :

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a>Sécurité de la requête

Un client malveillant ou naïve peut être en mesure de construire une requête qui prend beaucoup de temps à exécuter. Dans le pire des cas cela peut perturber l’accès à votre service.

Le **[Queryable]** attribut est un filtre d’action qui analyse, valide et applique la requête. Le filtre convertit les options de requête dans une expression LINQ. Lorsque le contrôleur OData retourne un **IQueryable** type, le **IQueryable** fournisseur LINQ convertit l’expression LINQ dans une requête. Par conséquent, les performances dépendent du fournisseur LINQ qui est utilisé, ainsi que sur les caractéristiques particulières de votre schéma de base de données ou le jeu de données.

Pour plus d’informations sur l’utilisation des options de requête OData dans l’API Web ASP.NET, consultez [prenant en charge des Options de requête OData](supporting-odata-query-options.md).

Si vous savez que tous les clients sont de confiance (par exemple, un environnement d’entreprise), ou si votre jeu de données est petite, les performances des requêtes peut-être pas un problème. Sinon, vous devez envisager les recommandations suivantes.

- Tester votre service avec différentes requêtes et la base de données de profil.
- Activer la pagination orientée serveur éviter le renvoi d’un jeu de données volumineux dans une seule requête. Pour plus d’informations, consultez [Server-Driven pagination](supporting-odata-query-options.md#server-paging). 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- Vous devez $filter et $orderby ? Certaines applications peuvent autoriser la pagination, à l’aide de $top et $skip de client, mais désactive les autres options de requête. 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- Pensez à restreindre $orderby aux propriétés dans un index cluster. Le tri des données volumineuses sans index cluster est lente. 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- Nombre maximal de nœuds : le **MaxNodeCount** propriété sur **[Queryable]** définit le nombre maximal de nœuds nombre autorisé dans l’arborescence de syntaxe $filter. La valeur par défaut est 100, mais peut vouloir définir une valeur inférieure, car un grand nombre de nœuds peut être lent à compiler. Cela est particulièrement vrai si vous utilisez LINQ to Objects (par exemple, les requêtes LINQ sur une collection en mémoire, sans l’utilisation d’un fournisseur LINQ intermédiaire). 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- Envisagez de désactiver les fonctions any() et all(), car ceux-ci peuvent être lentes. 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- Si les propriétés de chaîne contient des chaînes de grande taille #8212for exemple, une description de produit ou une entrée de blog & 8212consider # désactiver les fonctions de chaîne. 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- Envisagez d’interdiction de filtrage sur les propriétés de navigation. Filtrage sur les propriétés de navigation peut entraîner une jointure, qui peut être lente, en fonction de votre schéma de base de données. Le code suivant montre un validateur de requête qui empêche le filtrage sur les propriétés de navigation. Pour plus d’informations sur les validateurs de requête, consultez [Validation de requête](supporting-odata-query-options.md#query-validation). 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- Envisagez de limiter les requêtes $filter en écrivant un validateur personnalisé pour votre base de données. Par exemple, considérez ces deux requêtes : 

  - Tous les films avec les acteurs dont le nom commence par « A ».
  - Tous les films publiées en 1994.

    À moins que les films sont indexés par des acteurs, la première requête peut nécessiter le moteur de base de données à analyser l’intégralité de la liste de films. Tandis que la deuxième requête peut être acceptable, en supposant que films sont indexés par année de sortie.

    Le code suivant montre un validateur qui permet de filtrer sur les propriétés « ReleaseYear » et « Title », mais aucune autre propriété.

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- En règle générale, envisagez les fonctions de $filter que vous avez besoin. Si vos clients n’avez pas besoin de l’expressivité complète de $filter, vous pouvez limiter les fonctions admises.
