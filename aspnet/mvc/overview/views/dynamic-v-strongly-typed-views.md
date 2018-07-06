---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Vues fortement typées. Fortement typées vues | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 01/27/2011
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 941cb24b81721eb75a8f7150ddb17acf71287da3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822180"
---
<a name="dynamic-v-strongly-typed-views"></a>Vues fortement typées. Vues fortement typées
====================
par [Rick Anderson](https://github.com/Rick-Anderson)

Il existe trois façons pour passer des informations à partir d’un contrôleur à une vue dans ASP.NET MVC 3 :

1. En tant qu’objet modèle fortement typé.
2. Comme un type dynamique (à l’aide de @model dynamique)
3. À l’aide de ViewBag

J’ai écrit une application MVC 3 haut Blog simple de comparer ou différencier des vues dynamiques et fortement typées. Le contrôleur commence par une liste de blogs simple :

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

Cliquez avec le bouton droit dans la méthode IndexNotStonglyTyped() et ajouter une vue Razor.

[![8475.NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)

Assurez-vous que le **créer une vue fortement typée** case n’est pas cochée. La vue résultante ne contient pas une grande partie :

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

Étant donné que nous utilisons une dynamique et non une vue fortement typée, intellisense ne nous aider à. Le code complet est indiqué ci-dessous :

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

[![6646.NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)

Nous allons maintenant ajouter une vue fortement typée. Ajoutez le code suivant pour le contrôleur :

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


Notez qu’il est exactement le View(topBlogs) retour même ; appeler en tant que la vue non fortement typée. Cliquez avec le bouton droit à l’intérieur de *StonglyTypedIndex()* et sélectionnez **ajouter une vue**. Cette fois-ci, sélectionnez le **Blog** classe de modèle et sélectionnez **liste** en tant que le modèle de structure.

[![5658.StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)

Dans le nouveau modèle de vue, nous obtenons prise en charge intellisense.

[![7002.intellesince [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)

Le projet c# peut être téléchargé [ici](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).
