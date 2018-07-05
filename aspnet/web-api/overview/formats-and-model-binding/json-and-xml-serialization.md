---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: Sérialisation JSON et XML dans l’API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2012
ms.topic: article
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: e7fbcd41d64651255763c7629f0232788dcb3d30
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400919"
---
<a name="json-and-xml-serialization-in-aspnet-web-api"></a>Sérialisation JSON et XML dans l’API Web ASP.NET
====================
par [Mike Wasson](https://github.com/MikeWasson)

Cet article décrit les formateurs JSON et XML dans l’API Web ASP.NET.

Dans l’API Web ASP.NET, un *formateur de type de média* est un objet qui peut :

- Objets CLR de la lecture à partir de HTTP de corps du message
- Écrire des objets CLR dans un corps de message HTTP

API Web fournit les formateurs de type de média pour JSON et XML. Le framework insère ces formateurs dans le pipeline par défaut. Les clients peuvent demander au format JSON ou XML dans l’en-tête Accept de la requête HTTP.

## <a name="contents"></a>Sommaire

- [Formateur de Type de média JSON](#json_media_type_formatter)

    - [Propriétés en lecture seule](#json_readonly)
    - [Dates](#json_dates)
    - [Mise en retrait](#json_indenting)
    - [Casse mixte](#json_camelcasing)
    - [Objets anonymes et faiblement typé](#json_anon)
- [Formateur de Type de média XML](#xml_media_type_formatter)

    - [Propriétés en lecture seule](#xml_readonly)
    - [Dates](#xml_dates)
    - [Mise en retrait](#xml_indenting)
    - [Sérialiseurs XML par Type de paramètre](#xml_pertype)
- [Supprimer le JSON ou un formateur XML](#removing_the_json_or_xml_formatter)
- [Gestion des références d’objet circulaires](#handling_circular_object_references)
- [Sérialisation des objets de test](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a>Formateur de Type de média JSON

Mise en forme de JSON est fournie par le **JsonMediaTypeFormatter** classe. Par défaut, **JsonMediaTypeFormatter** utilise le [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) bibliothèque pour effectuer la sérialisation. Json.NET est un projet open source de tiers.

Si vous préférez, vous pouvez configurer le **JsonMediaTypeFormatter** classe à utiliser le **DataContractJsonSerializer** au lieu de Json.NET. Pour ce faire, définissez la **UseDataContractJsonSerializer** propriété **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a>Sérialisation JSON

Cette section décrit certains des comportements spécifiques du module de formatage JSON, à l’aide de la valeur par défaut [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) sérialiseur. Cela n’est pas censée être une documentation complète de la bibliothèque de Json.NET ; Pour plus d’informations, consultez le [Documentation Json.NET](http://james.newtonking.com/projects/json/help/).

#### <a name="what-gets-serialized"></a>Éléments sérialisés ?

Par défaut, toutes les propriétés et champs publics sont inclus dans le JSON sérialisé. Pour omettre une propriété ou un champ, la décorer avec le **JsonIgnore** attribut.

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

Si vous préférez un &quot;participer&quot; approche, décorer la classe avec le **DataContract** attribut. Si cet attribut est présent, les membres sont ignorés sauf s’ils ont le **DataMember**. Vous pouvez également utiliser **DataMember** pour sérialiser des membres privés.

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a>Propriétés en lecture seule

Propriétés en lecture seule sont sérialisées par défaut.

<a id="json_dates"></a>
### <a name="dates"></a>Dates

Par défaut, Json.NET écrit des dates [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format. Les dates au format UTC (Coordinated Universal Time) sont écrites avec un suffixe « Z ». Dates en heure locale incluent un décalage de fuseau horaire. Exemple :

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

Par défaut, Json.NET conserve le fuseau horaire. Vous pouvez le remplacer en définissant la propriété DateTimeZoneHandling :

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

Si vous préférez utiliser [format de date Microsoft JSON](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) au lieu de la norme ISO 8601, définir le **DateFormatHandling** propriété sur les paramètres du sérialiseur :

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a>Mise en retrait

Pour écrire le JSON mis en retrait, définissez le **mise en forme** à **Formatting.Indented**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a>Casse mixte

Pour écrire des noms de propriété JSON avec une casse mixte, sans modifier votre modèle de données, définissez la **CamelCasePropertyNamesContractResolver** sur le sérialiseur :

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a>Objets anonymes et faiblement typé

Une méthode d’action peut retourner un objet anonyme et sa sérialisation au format JSON. Exemple :

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

Le corps du message de réponse contient le code JSON suivant :

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

Si votre site web API reçoit faiblement structurées objets JSON à partir de clients, vous pouvez désérialiser le corps de la demande à un **Newtonsoft.Json.Linq.JObject** type.

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

Toutefois, il est généralement préférable d’utiliser des objets de données fortement typées. Puis vous n’avez pas besoin analyser les données vous-même, et vous offre les avantages de la validation du modèle.

Le sérialiseur XML ne prend pas en charge les types anonymes ou **JObject** instances. Si vous utilisez ces fonctionnalités pour vos données JSON, vous devez supprimer le formateur XML provenant du pipeline, comme décrit plus loin dans cet article.

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a>Formateur de Type de média XML

Mise en forme XML est fournie par le **XmlMediaTypeFormatter** classe. Par défaut, **XmlMediaTypeFormatter** utilise le **DataContractSerializer** classe pour effectuer la sérialisation.

Si vous préférez, vous pouvez configurer le **XmlMediaTypeFormatter** à utiliser le **XmlSerializer** au lieu du **DataContractSerializer**. Pour ce faire, définissez la **/usexmlserializer** propriété **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

Le **XmlSerializer** classe prend en charge un ensemble plus restreint de types que **DataContractSerializer**, mais offre davantage de contrôle sur le document XML obtenu. Envisagez d’utiliser **XmlSerializer** si vous avez besoin faire correspondre un schéma XML existant.

### <a name="xml-serialization"></a>Sérialisation XML

Cette section décrit certains des comportements spécifiques du formateur XML, à l’aide de la valeur par défaut **DataContractSerializer**.

Par défaut, le DataContractSerializer se comporte comme suit :

- Tous les champs et propriétés de lecture/écriture publique sont sérialisés. Pour omettre une propriété ou un champ, la décorer avec le **IgnoreDataMember** attribut.
- Membres privés et protégés ne sont pas sérialisées.
- Propriétés en lecture seule ne sont pas sérialisées. (Toutefois, le contenu d’une propriété de collection en lecture seule est sérialisé.)
- Noms de membre et de classe sont écrits dans le code XML exactement comme elles apparaissent dans la déclaration de classe.
- Un espace de noms XML par défaut est utilisé.

Si vous avez besoin de mieux contrôler la sérialisation, vous pouvez décorer la classe avec le **DataContract** attribut. Lorsque cet attribut est présent, la classe est sérialisée comme suit :

- &quot;Participer&quot; approche : propriétés et les champs ne sont pas sérialisés par défaut. Pour sérialiser une propriété ou un champ, la décorer avec le **DataMember** attribut.
- Pour sérialiser un membre privé ou protégé, la décorer avec le **DataMember** attribut.
- Propriétés en lecture seule ne sont pas sérialisées.
- Pour modifier la façon dont le nom de classe s’affiche dans le code XML, définissez le *nom* paramètre dans le **DataContract** attribut.
- Pour modifier la façon dont un nom de membre s’affiche dans le code XML, définissez le *nom* paramètre dans le **DataMember** attribut.
- Pour modifier l’espace de noms XML, définissez le *Namespace* paramètre dans le **DataContract** classe.

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a>Propriétés en lecture seule

Propriétés en lecture seule ne sont pas sérialisées. Si une propriété en lecture seule a un champ de stockage privé, vous pouvez marquer le champ privé avec les **DataMember** attribut. Cette approche nécessite la **DataContract** attribut sur la classe.

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a>Dates

Les dates sont écrites au format ISO 8601. Par exemple, &quot;2012-05-23T20:21:37.9116538Z&quot;.

<a id="xml_indenting"></a>
### <a name="indenting"></a>Mise en retrait

Pour écrire du code XML mis en retrait, définir le **mettre en retrait** propriété **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a>Sérialiseurs XML par Type de paramètre

Vous pouvez définir différents sérialiseurs XML pour les différents types CLR. Par exemple, vous pouvez avoir un objet de données particulière nécessite **XmlSerializer** pour la compatibilité descendante. Vous pouvez utiliser **XmlSerializer** pour cet objet et continuer à utiliser **DataContractSerializer** pour d’autres types.

Pour définir un sérialiseur XML pour un type particulier, appelez **SetSerializer**.

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

Vous pouvez spécifier un **XmlSerializer** ou n’importe quel objet qui dérive de **XmlObjectSerializer**.

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a>Supprimer le JSON ou un formateur XML

Vous pouvez supprimer le formateur JSON ou le formateur XML à partir de la liste des formateurs, si vous ne souhaitez pas les utiliser. Les principales raisons à cela sont :

- Pour restreindre les réponses de l’API web à un type de média spécifique. Par exemple, vous pouvez décider prendre en charge uniquement les réponses JSON et supprimer le formateur XML.
- Pour remplacer le formateur par défaut par un formateur personnalisé. Par exemple, vous pouvez remplacer le formateur JSON avec votre propre implémentation personnalisée d’un module de formatage JSON.

Le code suivant montre comment supprimer les formateurs par défaut. Appeler à partir votre **Application\_Démarrer** méthode, définie dans Global.asax.

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a>Gestion des références d’objet circulaires

Par défaut, les formateurs JSON et XML écrivent tous les objets en tant que valeurs. Si les deux propriétés font référence au même objet, ou si le même objet apparaît deux fois dans une collection, le formateur sérialise l’objet à deux reprises. Il s’agit une problématique en particulier si votre graphique d’objet contient des cycles, étant donné que le sérialiseur lève une exception lorsqu’il détecte une boucle dans le graphique.

Tenez compte des modèles d’objet suivants et contrôleur.

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

Appel de cette action provoque le formateur à levé une exception, ce qui se traduit par un état code 500 (erreur interne du serveur) réponse au client.

Pour conserver les références d’objet dans JSON, ajoutez le code suivant à **Application\_Démarrer** méthode dans le fichier Global.asax :

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

L’action du contrôleur retournent désormais JSON ressemble à ceci :

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

Notez que le sérialiseur ajoute un &quot;$id&quot; propriété pour les deux objets. En outre, il détecte que la propriété Employee.Department crée une boucle, elle remplace la valeur par une référence d’objet : {&quot;$ref&quot;:&quot;1&quot;}.

> [!NOTE]
> Références d’objet ne sont pas standard au format JSON. Avant d’utiliser cette fonctionnalité, réfléchissez aux si vos clients seront en mesure d’analyser les résultats. Il peut être préférable de ne supprimer cycles du graphique. Par exemple, le lien d’employé au département n’est pas vraiment nécessaire dans cet exemple.


Pour conserver les références d’objet dans XML, vous avez deux options. L’option la plus simple consiste à ajouter `[DataContract(IsReference=true)]` à votre classe de modèle. Le *IsReference* paramètre permet de références d’objet. N’oubliez pas que **DataContract** rend sérialisation participer, donc vous devrez également ajouter **DataMember** attributs aux propriétés :

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

Maintenant le formateur produira XML semblable au suivant :

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

Si vous souhaitez éviter les attributs sur votre classe de modèle, il existe une autre option : créer un nouveau spécifiques au type **DataContractSerializer** d’instance et définissez *preserveObjectReferences* à **true**  dans le constructeur. Puis définissez cette instance comme un sérialiseur par type sur le formateur de type de média XML. Le code suivant montre comment procéder :

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a>Sérialisation des objets de test

Lorsque vous concevez votre API web, il est utile tester comment vos objets de données seront sérialisés. Vous pouvez faire cela sans création d’un contrôleur ou d’appel d’une action de contrôleur.

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
