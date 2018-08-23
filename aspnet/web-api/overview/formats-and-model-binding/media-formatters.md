---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: Formateurs de médias dans ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: 7b7ba2fb3f1bba0447e700c84a017266cba305e6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831404"
---
<a name="media-formatters-in-aspnet-web-api-2"></a>Formateurs de médias dans ASP.NET Web API 2
====================
par [Mike Wasson](https://github.com/MikeWasson)

Ce didacticiel montre comment prendre en charge des formats supplémentaires dans l’API Web ASP.NET.

## <a name="internet-media-types"></a>Types de média Internet

Un type de média, également appelé type MIME, identifie le format d’un élément de données. Dans HTTP, les types de médias décrivent le format du corps du message. Un type de média se compose de deux chaînes, un type et un sous-type. Exemple :

- texte/html
- image/png
- application/json

Lorsqu’un message HTTP contienne un corps d’entité, l’en-tête Content-Type spécifie le format du corps du message. Cela indique le destinataire d’analyser le contenu du corps du message.

Par exemple, si une réponse HTTP contient une image PNG, la réponse peut contenir les en-têtes suivants.

[!code-console[Main](media-formatters/samples/sample1.cmd)]

Lorsque le client envoie un message de demande, il peut inclure un en-tête Accept. L’en-tête Accept indique que le serveur quels supports type (s) le client veut à partir du serveur. Exemple :

[!code-console[Main](media-formatters/samples/sample2.cmd)]

Cet en-tête indique au serveur que le client veut HTML, XHTML ou XML.

Le type de support détermine la manière dont les API Web sérialise et désérialise le corps du message HTTP. API Web dispose d’une prise en charge intégrée des données XML, JSON, BSON et form-UrlEncode données, et peut prendre en charge les types de médias supplémentaires en écrivant un *formateur média*.

Pour créer un formateur de médias, dérivez à partir d’une de ces classes :

- [MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx). Cette classe utilise la lecture asynchrone et les méthodes d’écriture.
- [BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx). Cette classe est dérivée de **MediaTypeFormatter** , mais utilise des méthodes de lecture/écriture Synchronous.

Dérivant de **BufferedMediaTypeFormatter** est plus simple, car il n’existe aucun code asynchrone, mais cela signifie également que le thread appelant peut bloquer pendant les e/s.

## <a name="example-creating-a-csv-media-formatter"></a>Exemple : Création d’un formateur de médias CSV

L’exemple suivant montre un formateur de type de média qui peut sérialiser un objet de produit dans un format de valeurs séparées par des virgules (CSV). Cet exemple utilise le type de produit défini dans le didacticiel [d’une API Web qui prend en charge les opérations CRUD](../older-versions/creating-a-web-api-that-supports-crud-operations.md). Voici la définition de l’objet Product :

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

Pour implémenter un module de formatage de volume partagé de cluster, définissez une classe qui dérive de **BufferedMediaTypeFormater**:

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

Dans le constructeur, ajoutez les types de médias qui prend en charge par le formateur. Dans cet exemple, le formateur prend en charge un seul type de support, &quot;texte/csv&quot;:

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

Remplacer le **CanWriteType** méthode pour indiquer leurs types le formateur peut sérialiser :

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

Dans cet exemple, le module de formatage peut sérialiser unique `Product` objets ainsi que des collections de `Product` objets.

De même, remplacez le **CanReadType** méthode pour indiquer leurs types le formateur peut désérialiser. Dans cet exemple, le formateur ne prend pas en charge la désérialisation, par conséquent, la méthode retourne simplement **false**.

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

Enfin, vous pouvez substituer le **WriteToStream** (méthode). Cette méthode sérialise un type en l’écrivant dans un flux. Si votre formateur prend en charge la désérialisation, vous devez également substituer la **ReadFromStream** (méthode).

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a>Ajout d’un formateur de média pour le Pipeline de l’API Web

Pour ajouter un support de type formateur au pipeline API Web, utilisez le **formateurs** propriété sur le **HttpConfiguration** objet.

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a>Encodages de caractères

Si vous le souhaitez, un formateur de médias peut prendre en charge plusieurs codages de caractères, telles que UTF-8 ou ISO 8859-1.

Dans le constructeur, ajoutez un ou plusieurs [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) types à la **SupportedEncodings** collection. Placez le premier de codage par défaut.

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

Dans le **WriteToStream** et **ReadFromStream** appeler des méthodes, [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) pour sélectionner l’encodage de caractères par défaut. Cette méthode correspond aux en-têtes de demande par rapport à la liste des encodages pris en charge. Utilisez retourné **Encoding** lorsque vous lisez ou écrivez à partir du flux :

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
