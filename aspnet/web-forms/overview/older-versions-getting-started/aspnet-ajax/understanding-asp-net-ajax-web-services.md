---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
title: Présentation des Services Web ASP.NET AJAX | Microsoft Docs
author: scottcate
description: Services Web font partie intégrante du .NET framework qui fournissent une solution multiplateforme pour échanger des données entre les systèmes distribués. Bien que Web...
ms.author: aspnetcontent
ms.date: 03/28/2008
ms.assetid: 3332d6e7-e2e1-4144-b805-e71d51e7e415
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
msc.type: authoredcontent
ms.openlocfilehash: b98ef4c27ab7b4b729e9e5b68e7d2642a6418ab6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838599"
---
<a name="understanding-aspnet-ajax-web-services"></a>Présentation des Services Web ASP.NET AJAX
====================
par [Scott Cate](https://github.com/scottcate)

[Télécharger PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial05_Web_Services_with_MS_Ajax_cs.pdf)

> Services Web font partie intégrante du .NET framework qui fournissent une solution multiplateforme pour échanger des données entre les systèmes distribués. Bien que les Services Web sont normalement utilisés pour autoriser différents systèmes d’exploitation, les modèles d’objet et les langages de programmation pour envoyer et recevoir des données, elles peuvent également servir pour injecter des données dans une page ASP.NET AJAX ou envoyer des données à partir d’une page à un système back-end de manière dynamique. Tout cela est possible sans avoir recours pour les opérations de publication (postback).


## <a name="calling-web-services-with-aspnet-ajax"></a>Appel de Services Web avec ASP.NET AJAX

Dan Wahlin

Services Web font partie intégrante du .NET framework qui fournissent une solution multiplateforme pour échanger des données entre les systèmes distribués. Bien que les Services Web sont normalement utilisés pour autoriser différents systèmes d’exploitation, les modèles d’objet et les langages de programmation pour envoyer et recevoir des données, elles peuvent également servir pour injecter des données dans une page ASP.NET AJAX ou envoyer des données à partir d’une page à un système back-end de manière dynamique. Tout cela est possible sans avoir recours pour les opérations de publication (postback).

Bien que le contrôle UpdatePanel d’ASP.NET AJAX fournit un moyen simple d’AJAX activer n’importe quelle page ASP.NET, il peut arriver que vous deviez accéder dynamiquement des données sur le serveur sans utiliser un UpdatePanel. Dans cet article, vous verrez comment accomplir cette tâche en créant et en consommation de Services Web au sein des pages ASP.NET AJAX.

Cet article se concentrera sur les fonctionnalités disponibles dans les Extensions AJAX de ASP.NET core, ainsi que d’un contrôle de Service Web est activé dans le Kit de ressources ASP.NET AJAX appelé le AutoCompleteExtender. Les sujets abordés incluent la définition des Services Web compatibles AJAX, création de proxys de client et appeler les Services Web avec JavaScript. Vous verrez également comment les appels de Service Web peuvent être effectués directement à des méthodes de page ASP.NET.

## <a name="web-services-configuration"></a>Configuration des Services Web

Quand un nouveau projet de Site Web est créé avec Visual Studio 2008, le fichier web.config a un nombre de nouveaux ajouts qui peuvent être peu familières aux utilisateurs des versions antérieures de Visual Studio. Parmi ces modifications mapper le préfixe « asp » aux contrôles ASP.NET AJAX afin de pouvoir être utilisés dans les pages alors que d’autres définissent obligatoires HttpHandlers et HttpModules. Liste 1 montre les modifications apportées à la `<httpHandlers>` élément dans le fichier web.config qui affecte les appels de Service Web. La valeur par défaut que HttpHandler utilisée pour traiter les appels .asmx est supprimé et remplacé par une classe ScriptHandlerFactory située dans l’assembly System.Web.Extensions.dll. System.Web.Extensions.dll contient toutes les fonctionnalités de base utilisée par ASP.NET AJAX.

**Listing 1. Configuration du Gestionnaire de Service Web ASP.NET AJAX**

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample1.xml)]

Ce remplacement HttpHandler est effectué afin de permettre les appels JavaScript Objet Notation (JSON) à être effectué à partir de pages ASP.NET AJAX pour les Services Web .NET à l’aide d’un proxy de Service Web de JavaScript. ASP.NET AJAX envoie des messages JSON aux Services Web plutôt que les appels SOAP Simple Object Access Protocol () standard généralement associés aux Services Web. Cela entraîne plus petits messages de demande et réponse globales. Il permet également de traitement côté client plus efficace de données dans la mesure où la bibliothèque JavaScript AJAX ASP.NET est optimisée pour travailler avec des objets JSON. Listing 2 et des exemples de show Listing 3 Service Web messages de demande et réponse sérialisés au format JSON. Le message de demande indiqué dans le Listing 2 passe un paramètre de pays avec la valeur « Belgique » alors que le message de réponse dans le Listing 3 passe un tableau d’objets Customer et leurs propriétés associées.

**Listing 2. Message de demande de Service Web sérialisé vers JSON**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample2.json)]

> *> [!NOTE] le nom de l’opération est défini dans le cadre de l’URL pour le service web. en outre, les messages de demande ne sont pas toujours soumis par le biais de JSON. Services Web peuvent utiliser l’attribut ScriptMethod avec le paramètre UseHttpGet défini sur true, ce qui entraîne des paramètres à passer un les paramètres de chaîne de requête.*


**Liste 3. Message de réponse de Service Web sérialisé vers JSON**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample3.json)]

Dans la section suivante, vous allez apprendre à créer des Services Web capable de gérer les messages de demande JSON et répond avec les types simples et complexes.

## <a name="creating-ajax-enabled-web-services"></a>Création de Services Web compatibles AJAX

L’infrastructure ASP.NET AJAX fournit plusieurs façons d’appeler des Services Web. Vous pouvez utiliser le contrôle AutoCompleteExtender (disponible dans le Kit de ressources ASP.NET AJAX) ou JavaScript. Toutefois, avant d’appeler un service vous devez activer AJAX il afin qu’il peut être appelé par le code de script client.

Si vous débutez avec les Services Web ASP.NET, vous trouverez il simples à créer et AJAX-activer les services. Le .NET framework prend en charge la création de Services Web ASP.NET depuis sa publication initiale en 2002 et les Extensions ASP.NET AJAX fournissent des fonctionnalités AJAX supplémentaires qui s’appuie sur l’ensemble de fonctionnalités par défaut du .NET framework. Visual Studio .NET 2008 Bêta 2 a une prise en charge intégrée pour la création des fichiers de Service Web .asmx et déduit automatiquement le code associé à côté des classes à partir de la classe System.Web.Services.WebService. Lorsque vous ajoutez des méthodes dans la classe, vous devez appliquer l’attribut WebMethod dans l’ordre pour qu’ils puissent être appelées par les consommateurs de services Web.

Liste 4 montre un exemple d’application de l’attribut WebMethod à une méthode nommée GetCustomersByCountry().

**Liste 4. À l’aide de l’attribut WebMethod dans un Service Web**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample4.cs)]

La méthode GetCustomersByCountry() accepte un paramètre de pays et retourne un client tableau d’objets. La valeur de pays passée dans la méthode est transférée à une classe de couche métier qui à son tour appelle une classe de couche données pour récupérer les données à partir de la base de données, remplir les propriétés d’objet client avec des données et retourner le tableau.

## <a name="using-the-scriptservice-attribute"></a>À l’aide de l’attribut ScriptService

Lors de l’ajout de la méthode Web attribut permet à la méthode GetCustomersByCountry() doit être appelée par les clients qui envoient des messages SOAP standard pour le Service Web, il n’autorise pas les appels JSON à partir des applications ASP.NET AJAX. Pour permettre les appels JSON qu’il veut vous devez appliquer l’Extension d’ASP.NET AJAX `ScriptService` attribut à la classe de Service Web. Cela permet à un Service Web envoyer des messages de réponse mis en forme à l’aide de JSON et permet à un script côté client appeler un service en envoyant des messages JSON.

Liste 5 illustre un exemple d’application de l’attribut ScriptService à une classe de Service Web nommée CustomersService.

**Liste 5. À l’aide de l’attribut ScriptService à un Service Web compatible AJAX**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample5.cs)]

L’attribut ScriptService joue un marqueur qui indique qu’il peut être appelée à partir du code de script AJAX. Il en fait ne gère pas les tâches de sérialisation ou désérialisation JSON qui se produisent en arrière-plan. Le ScriptHandlerFactory (configuré dans le fichier web.config) et autres classes connexes effectuent le plus gros de traitement JSON.

## <a name="using-the-scriptmethod-attribute"></a>À l’aide de l’attribut ScriptMethod

L’attribut ScriptService est le seul attribut d’ASP.NET AJAX qui doit être définie dans un Service Web de .NET pour qu’il utilisable par les pages ASP.NET AJAX. Toutefois, un autre attribut nommé ScriptMethod peut également être appliqué directement à des méthodes Web dans un service. ScriptMethod définit trois propriétés, y compris `UseHttpGet`, `ResponseFormat` et `XmlSerializeString`. Modification des valeurs de ces propriétés peut être utile dans les cas où le type de demande acceptée par une méthode Web doit être modifié pour GET, lorsqu’une méthode Web doit renvoyer des données XML brutes sous la forme d’un `XmlDocument` ou `XmlElement` objet ou lorsque les données retournées à partir d’un  service doit toujours être sérialisé au format XML au lieu de JSON.

La propriété peut être utilisée lorsqu’une méthode Web doit accepter de UseHttpGet obtenir des demandes par opposition aux requêtes POST. Demandes sont envoyées à l’aide d’une URL avec des paramètres d’entrée de méthode Web convertie en fonction des paramètres. Le UseHttpGet propriété par défaut est false et doit uniquement être définie sur `true` lorsque les opérations sont connues pour être sécurisée et lorsque les données sensibles ne sont pas transmises à un Service Web. Liste 6 montre un exemple d’utilisation de l’attribut ScriptMethod avec la propriété UseHttpGet.

**Liste 6. À l’aide de l’attribut ScriptMethod avec la propriété UseHttpGet.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample6.cs)]

Les en-têtes envoyés lorsque la méthode Web HttpGetEcho indiqué dans la liste 6 est appelée sont montre suivant :

`GET /CustomerViewer/DemoService.asmx/HttpGetEcho?input=%22Input Value%22 HTTP/1.1`

En plus de permettre des méthodes Web à accepter les demandes HTTP GET, l’attribut ScriptMethod peut également servir lorsque des réponses XML doivent être retournés à partir d’un service plutôt que JSON. Par exemple, un Service Web peut récupérer un flux RSS à partir d’un site distant et le retourner comme un objet XmlDocument ou XmlElement. Traitement des données XML données peuvent ensuite se produire sur le client.

Liste 7 montre un exemple d’utilisation de la propriété ResponseFormat pour spécifier que les données XML doivent être retournées par une méthode Web.

**Liste 7. À l’aide de l’attribut ScriptMethod avec la propriété ResponseFormat.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample7.cs)]

La propriété ResponseFormat peut également être utilisée avec la propriété XmlSerializeString. La propriété XmlSerializeString a la valeur false, ce qui signifie que tous les retournent les types à l’exception des chaînes retournées par une méthode Web sont sérialisés au format XML par défaut lorsque le `ResponseFormat` propriété est définie sur `ResponseFormat.Xml`. Lorsque `XmlSerializeString` a la valeur `true`, tous les types retournés à partir d’une méthode Web sont sérialisées en XML, y compris les types de chaîne. Si la propriété ResponseFormat a la valeur `ResponseFormat.Json` la propriété XmlSerializeString est ignorée.

Liste 8 montre un exemple d’utilisation de la propriété XmlSerializeString pour forcer les chaînes doit être sérialisé au format XML.

**Liste 8. À l’aide de l’attribut ScriptMethod avec la propriété XmlSerializeString**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample8.cs)]

La valeur retournée par la méthode GetXmlString Web indiqué dans la liste 8 est indiquée ci-après :

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample9.cs)]

Bien que le format JSON par défaut réduit la taille globale des messages de demande et de réponse et est plus facilement consommé par les clients ASP.NET AJAX de manière multinavigateurs, les propriétés ResponseFormat et XmlSerializeString peuvent être utilisé lorsque client les applications telles que Internet Explorer 5 ou version ultérieure attendent des données XML doit être retourné à partir d’une méthode Web.

## <a name="working-with-complex-types"></a>Utilisation des Types complexes

Liste 5 a montré un exemple de retourner un type complexe nommé client à partir d’un Service Web. La classe Customer définit plusieurs types simples en interne en tant que propriétés telles que FirstName et LastName. Les types complexes utilisés en tant que paramètre d’entrée ou le type de retour sur une méthode Web compatibles AJAX sont automatiquement sérialisés en JSON avant d’être envoyés au côté client. Toutefois, les types complexes imbriqués (celles définies en interne dans un autre type) ne sont pas accessibles au client en tant qu’objets autonomes par défaut.

Dans les cas où un type complexe imbriqué utilisé par un Service Web doit également être utilisé dans une page de client, l’attribut d’ASP.NET AJAX GenerateScriptType peut être ajouté au Service Web. Par exemple, la classe CustomerDetails indiquée dans la liste 9 contient les propriétés d’adresse et le sexe qui ***représentent des types complexes imbriqués.***

**Liste 9. La classe CustomerDetails affichée ici contient deux types complexes imbriqués.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample10.cs)]

Les objets d’adresse et le sexe définis dans la classe CustomerDetails indiquée dans la liste 9 ne sont pas automatiquement rendus disponibles pour une utilisation du côté client à l’aide de JavaScript dans la mesure où ils sont les types imbriqués (adresse est une classe et sexe est une énumération). Dans les situations où un type imbriqué est utilisé au sein d’un Service Web doit être disponible sur la côté client, l’attribut GenerateScriptType mentionné plus haut peut être utilisé (voir Listing 10). Cet attribut peut être ajouté plusieurs fois dans les cas où les différents types complexes imbriqués sont retournées à partir d’un service. Il peut être appliqué directement à la classe de Service Web ou au-dessus de méthodes Web spécifiques.

**Liste 10. À l’aide de l’attribut GenerateScriptService pour définir les types imbriqués qui doivent être disponibles pour le client.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample11.cs)]

En appliquant la `GenerateScriptType` attribut pour les types de Service Web, l’adresse et le sexe automatiquement sera disponible pour une utilisation par le code d’ASP.NET AJAX JavaScript côté client. Liste 11 montre le code JavaScript qui est automatiquement généré et envoyé au client en ajoutant l’attribut GenerateScriptType sur un Service Web. Vous verrez comment utiliser des types complexes imbriqués plus loin dans l’article.

**Liste 11. Types complexes imbriqués mis à disposition une page ASP.NET AJAX.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample12.cs)]

Maintenant que vous avez vu comment créer des Services Web et les rendre accessibles aux pages ASP.NET AJAX, examinons comment créer et utiliser les proxys JavaScript afin que les données peuvent être récupérées ou envoyées aux Services Web.

## <a name="creating-javascript-proxies"></a>Création de proxys de JavaScript

Appel de Service Web standard (.NET ou une autre plateforme) généralement implique la création d’un objet proxy qui vous protège contre la complexité de l’envoi de messages de demande et de réponse SOAP. Lors des appels de Service Web de ASP.NET AJAX, les proxys JavaScript peuvent être créées et utilisées pour appeler facilement des services sans vous soucier de sérialiser et désérialiser les messages JSON. Les proxys JavaScript peuvent être générées automatiquement en utilisant le contrôle ASP.NET AJAX ScriptManager.

Création d’un proxy JavaScript pouvant appeler des Services Web s’effectue à l’aide de la propriété de ScriptManager Services. Cette propriété vous permet de définir un ou plusieurs services une page ASP.NET AJAX peut appeler de façon asynchrone pour envoyer ou recevoir des données sans nécessiter d’opérations de publication (postback). Vous définissez un service à l’aide d’ASP.NET AJAX `ServiceReference` contrôle et l’affectation de l’URL du Service Web pour le contrôle `Path` propriété. Liste 12 montre un exemple de référencement d’un service nommé CustomersService.asmx.

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample13.aspx)]

**Liste 12. Définition d’un Service Web utilisé dans une page ASP.NET AJAX.**

Ajout d’une référence à la CustomersService.asmx via le contrôle ScriptManager provoque un proxy JavaScript être générés dynamiquement et référencé par la page. Le proxy est incorporé à l’aide de la &lt;script&gt; balise et chargées dynamiquement en appelant le fichier CustomersService.asmx et en ajoutant/js à la fin de celle-ci. L’exemple suivant montre comment le proxy JavaScript est incorporé dans la page lorsque le débogage est désactivé dans le fichier web.config :

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample14.html)]

> *> [!NOTE] Si vous aimeriez voir le code proxy JavaScript qui est généré vous pouvez tapez l’URL de Service Web .NET souhaité dans la zone d’adresse d’Internet Explorer et ajouter/js à la fin de celle-ci.*


Si le débogage est activé dans le fichier web.config qu'une version debug du proxy JavaScript est incorporée dans la page, comme indiqué ci-après :

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample15.html)]

Le proxy JavaScript créé par ScriptManager peut également être incorporé directement dans la page plutôt que référencées à l’aide du &lt;script&gt; attribut src de la balise. Cela est possible en définissant la ServiceReference propriété du contrôle InlineScript sur true (la valeur par défaut est false). Cela peut être utile lorsqu’un proxy n’est pas partagé sur plusieurs pages, et lorsque vous souhaitez réduire le nombre d’appels réseau vers le serveur. Quand les InlineScript est définie sur true le script de proxy ne sont pas être mis en cache par le navigateur la valeur par défaut False est recommandée dans les cas où le proxy est utilisé par plusieurs pages dans une application ASP.NET AJAX. Voici un exemple d’utilisation de la propriété InlineScript suivant :

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample16.aspx)]

## <a name="using-javascript-proxies"></a>À l’aide de proxys JavaScript

Une fois qu’un Service Web est référencé par une page ASP.NET AJAX à l’aide du contrôle ScriptManager, un appel peut être effectué pour le Service Web et les données renvoyées peuvent être gérées à l’aide des fonctions de rappel. Un Service Web est appelé par faisant référence à son espace de noms (le cas échéant), nom de la classe et le nom de la méthode Web. Tous les paramètres transmis au Service Web peuvent être définies avec une fonction de rappel qui gère les données retournées.

Un exemple d’utilisation d’un proxy JavaScript pour appeler une méthode Web nommée GetCustomersByCountry() est indiqué dans la liste 13. La fonction GetCustomersByCountry() est appelée lorsqu’un utilisateur final clique sur un bouton sur la page.

**Liste 13. Appel d’un Service Web avec un proxy JavaScript.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample17.js)]

Cet appel fait référence à l’espace de noms InterfaceTraining, CustomersService classe et méthode de Web GetCustomersByCountry définis dans le service. Il transmet une valeur de pays obtenue à partir d’une zone de texte ainsi que d’une fonction de rappel nommée OnWSRequestComplete qui doit être appelé lors de l’appel de Service Web asynchrone retourne. OnWSRequestComplete gère le tableau d’objets de client renvoyé par le service et les convertit en une table qui s’affiche dans la page. La sortie générée à partir de l’appel est illustrée dans la Figure 1.


[![Liaison de données obtenues en effectuant un appel AJAX asynchrone à un Service Web.](understanding-asp-net-ajax-web-services/_static/image2.png)](understanding-asp-net-ajax-web-services/_static/image1.png)

**Figure 1**: liaison de données obtenue en effectuant un appel AJAX asynchrone à un Service Web.  ([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-web-services/_static/image3.png))


Les proxys JavaScript peuvent également effectuer des appels unidirectionnels aux Services Web dans les cas où une méthode Web doit être appelée, mais le proxy ne doit pas attendre une réponse. Vous souhaiterez par exemple, appeler un Service Web pour démarrer un processus tel qu’un flux de travail, mais pas attendre une valeur de retour à partir du service. Dans les cas où un appel à sens unique doit être apportées à un service, la fonction de rappel illustrée à la liste 13 peut simplement être omise. Dans la mesure où aucune fonction de rappel n’est défini par l’objet proxy n’attend pas le Service Web retourner des données.

## <a name="handling-errors"></a>Gestion des erreurs

Les rappels asynchrones aux Services Web peuvent rencontrer des différents types d’erreurs telles que le réseau en cours vers le bas, l’indisponibilité du Service Web ou une exception qui est retourné. Heureusement, les objets de proxy JavaScript générés par le ScriptManager autorisent plusieurs rappels doivent être définies pour gérer les erreurs et échecs en plus de rappel de réussite illustrée précédemment. Une fonction de rappel d’erreur peut être définie immédiatement après la fonction de rappel standard dans l’appel à la méthode Web comme indiqué dans la liste de 14.

**Liste 14. Définition d’une fonction de rappel d’erreur et affichage des erreurs.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample18.js)]

Les erreurs qui surviennent lorsque le Service Web est appelé déclenchera la fonction de rappel OnWSRequestFailed() pour être appelé qui accepte un objet représentant l’erreur en tant que paramètre. L’objet d’erreur expose plusieurs fonctions pour déterminer la cause de l’erreur également ou non l’appel a expiré. Liste 14 montre un exemple de l’utilisation des fonctions d’erreur différent et la Figure 2 montre un exemple de la sortie générée par les fonctions.


[![Sortie générée par l’appel de fonctions d’erreur ASP.NET AJAX.](understanding-asp-net-ajax-web-services/_static/image5.png)](understanding-asp-net-ajax-web-services/_static/image4.png)

**Figure 2**: sortie générée en appelant des fonctions d’erreur ASP.NET AJAX.  ([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-web-services/_static/image6.png))


## <a name="handling-xml-data-returned-from-a-web-service"></a>Gestion des données XML retournées à partir d’un Service Web

Vous avez vu précédemment comment une méthode Web peut retourner des données XML brutes à l’aide de l’attribut ScriptMethod, ainsi que sa propriété ResponseFormat. Lorsque ResponseFormat est définie sur ResponseFormat.Xml, les données retournées par le Service Web sont sérialisées XML plutôt que JSON. Cela peut être utile lorsque les données XML doivent être transmis directement au client pour traitement à l’aide de JavaScript ou XSLT. À l’heure actuelle, Internet Explorer 5 ou version ultérieure fournit le meilleur modèle objet côté client pour l’analyse et le filtrage des données XML en raison de sa prise en charge intégrée pour MSXML.

Extraction de données XML à partir d’un Service Web n’est pas différente de la récupération des autres types de données. Commencez par appeler le proxy JavaScript pour appeler la fonction appropriée et de définir une fonction de rappel. Une fois que l’appel retourne vous pouvez ensuite traiter les données dans la fonction de rappel.

Liste 15 montre un exemple d’appel d’une méthode Web nommée GetRssFeed() qui retourne un objet XmlElement. GetRssFeed() accepte un paramètre unique représentant l’URL pour le flux RSS pour récupérer.

**Liste 15. Utilisation des données XML retournées à partir d’un Service Web.**

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample19.html)]

Cet exemple transmet une URL vers un flux RSS et traite les données XML renvoyées dans la fonction OnWSRequestComplete(). OnWSRequestComplete() vérifie d’abord si le navigateur est Internet Explorer de savoir si l’analyseur MSXML est disponible. Dans le cas, une instruction XPath est utilisée pour localiser tous les &lt;élément&gt; balises dans le flux RSS. Chaque élément est ensuite parcouru et associé &lt;titre&gt; et &lt;lien&gt; balises sont situés et traitées pour afficher les données de chaque élément. Figure 3 montre un exemple de la sortie générée d’apporter un ASP.NET AJAX à appeler via un proxy JavaScript à la méthode Web GetRssFeed().

## <a name="handling-complex-types"></a>Gestion des Types complexes

Les types complexes accepté ou retournées par un Service Web sont automatiquement exposés via un proxy JavaScript. Toutefois, les types complexes imbriqués ne sont pas directement accessibles sur la côté client, sauf si l’attribut GenerateScriptType est appliquée au service, comme indiqué précédemment. Pourquoi voudriez-vous utiliser un type complexe imbriqué côté client ?

Pour répondre à cette question, supposons qu’une page ASP.NET AJAX affiche les données client et permet aux utilisateurs finaux mettre à jour l’adresse d’un client. Si le Service Web spécifie que le type d’adresse (un type complexe défini dans une classe CustomerDetails) peut être envoyé au client, puis le processus de mise à jour peuvent être divisé en séparer les fonctions afin de mieux réutiliser le code.


[![Sortie de création à partir de l’appel d’un Service Web qui retourne des données RSS.](understanding-asp-net-ajax-web-services/_static/image8.png)](understanding-asp-net-ajax-web-services/_static/image7.png)

**Figure 3**: création à partir de l’appel d’un Service Web qui retourne les données RSS de sortie.  ([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-web-services/_static/image9.png))


Liste 16 montre un exemple de code côté client qui appelle un objet d’adresse défini dans un espace de noms de modèle, il remplit avec les données mises à jour et affecte à la propriété d’adresse d’un objet CustomerDetails. L’objet CustomerDetails est ensuite passé au Service Web pour le traitement.

**Liste 16. À l’aide de types complexes imbriqués**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample20.js)]

## <a name="creating-and-using-page-methods"></a>Création et utilisation des méthodes de Page

Services Web fournissent un excellent moyen d’exposer des services réutilisables à une variété de clients, y compris les pages ASP.NET AJAX. Toutefois, il peut arriver dans lequel une page a besoin pour récupérer des données qui ne jamais être utilisées ou être partagées par d’autres pages. Dans ce cas, rendre un fichier .asmx pour permettre la page pour accéder aux données peut sembler excessifs, car le service est uniquement utilisé par une seule page.

ASP.NET AJAX fournit un autre mécanisme pour effectuer des appels de Service de type Web sans créer les fichiers .asmx autonome. Cela permet à l’aide d’une technique appelée « méthodes de page ». Méthodes de page sont des méthodes statiques (partagées dans VB.NET) intégrées directement dans un fichier de page ou code-beside qui ont l’attribut WebMethod est appliqué pour les. En appliquant l’attribut WebMethod, elles peuvent être appelées à l’aide d’un objet JavaScript spécial nommé PageMethods qui est créée dynamiquement lors de l’exécution. L’objet PageMethods agit comme un proxy qui vous protège le processus de sérialisation/désérialisation JSON. Notez que pour pouvoir utiliser l’objet PageMethods vous devez définir la propriété EnablePageMethods de ScriptManager sur true.

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample21.aspx)]

Liste 17 montre un exemple de définition des deux méthodes de page dans une classe code-beside ASP.NET. Ces méthodes récupèrent des données à partir d’une classe de couche métier située dans l’application\_dossier de Code du site Web.

**Liste 17. Définition des méthodes de page.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample22.cs)]

Lorsque ScriptManager détecte la présence des méthodes Web dans la page, il génère une référence dynamique à l’objet PageMethods mentionné précédemment. Appel d’une méthode Web s’effectue en faisant référence à la classe PageMethods suivie du nom de la méthode et toutes les données de paramètre requis qui doivent être passées. Liste 18 présente des exemples d’appeler les deux méthodes de page indiquées précédemment.

**Liste 18. Appel de méthodes de page avec l’objet PageMethods JavaScript.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample23.js)]

À l’aide de l’objet PageMethods est très similaire à l’aide d’un objet proxy JavaScript. Vous spécifiez d’abord toutes les données de paramètre qui doivent être passées à la méthode de page, puis définissent la fonction de rappel qui doit être appelée lorsque l’appel asynchrone est retournée. Un rappel d’échec peut également être spécifié (consultez répertoriant les 14 pour obtenir un exemple de gérer les défaillances).

## <a name="the-autocompleteextender-and-the-aspnet-ajax-toolkit"></a>Le AutoCompleteExtender et le Kit de ressources ASP.NET AJAX

Le Kit de ressources ASP.NET AJAX (disponible à partir de [ http://ajax.asp.net ](http://ajax.asp.net)) offre plusieurs contrôles qui peuvent être utilisées pour accéder aux Services Web. Plus précisément, le Kit de ressources contienne un contrôle utile nommé `AutoCompleteExtender` qui peut être utilisé pour appeler des Services Web et afficher les données dans les pages sans écrire de code JavaScript du tout.

Le contrôle de AutoCompleteExtender peut être utilisé pour étendre les fonctionnalités existantes d’une zone de texte et aident les utilisateurs à localiser facilement les données recherchées. Frappe dans une zone de texte le contrôle peut être utilisé pour interroger un Service Web et affiche les résultats sous la zone de texte dynamiquement. Figure 4 montre un exemple d’utilisation du contrôle AutoCompleteExtender pour afficher les ID de client pour une application de prise en charge. Comme l’utilisateur tape des caractères différents dans la zone de texte, différents éléments seront affichées ci-dessous en fonction de leur entrée. Les utilisateurs peuvent sélectionner ensuite l’id de client souhaité.

À l’aide de la AutoCompleteExtender au sein d’une page ASP.NET AJAX requiert que l’assembly AjaxControlToolkit.dll ajouté au dossier bin du site Web. Une fois que l’assembly du Kit de ressources a été ajoutée, vous souhaitez référencer dans le fichier web.config afin que les contrôles qu’il contient sont disponibles pour toutes les pages dans une application. Cela est possible en ajoutant la balise suivante au sein de web.config &lt;contrôles&gt; balise :

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample24.xml)]

Dans les cas où vous devez uniquement utiliser le contrôle dans une page spécifique, vous pouvez le référencer en ajoutant la directive de référence vers le haut d’une page, comme indiqué ci-après au lieu de la mise à jour de web.config :

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample25.aspx)]


[![Utilisation du contrôle AutoCompleteExtender.](understanding-asp-net-ajax-web-services/_static/image11.png)](understanding-asp-net-ajax-web-services/_static/image10.png)

**Figure 4**: utilisation du contrôle AutoCompleteExtender.  ([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-web-services/_static/image12.png))


Une fois que le site Web a été configuré pour utiliser le Kit de ressources ASP.NET AJAX, un contrôle AutoCompleteExtender peut être ajouté dans la page bien comme vous ajouteriez un contrôle de serveur ASP.NET normal. Liste 19 montre un exemple d’utilisation du contrôle pour appeler un Service Web.

**Liste 19. Utilisation du contrôle ASP.NET AJAX Toolkit AutoCompleteExtender.**

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample26.aspx)]

Le AutoCompleteExtender a plusieurs propriétés différentes, y compris les propriétés ID et runat standard trouvées sur les contrôles serveur. Outre ces éléments, il vous permet de définir le nombre de caractères un type d’utilisateur final avant que le Service Web est interrogée pour les données. La propriété MinimumPrefixLength illustrée à la liste de 19, le service à être appelée chaque fois qu’un caractère est tapé dans la zone de texte. Vous souhaiterez être prudent de définition de cette valeur dans la mesure où chaque fois que l’utilisateur tape un caractère, le Service Web sera appelé pour rechercher des valeurs qui correspondent aux caractères dans la zone de texte. Le Service Web à appeler, ainsi que la méthode Web cible est définie à l’aide des propriétés ServicePath et ServiceMethod respectivement. Enfin, la propriété TargetControlID identifie quelle zone de texte pour raccorder le contrôle AutoCompleteExtender.

Le Service Web appelé doit avoir l’attribut ScriptService appliquée comme indiqué précédemment, et la cible de méthode Web doit accepter deux paramètres nommés prefixText et nombre. Le paramètre prefixText représente les caractères tapés par l’utilisateur final et le paramètre count représente le nombre d’éléments à renvoyer (la valeur par défaut est 10). Liste des 20 montre un exemple de la méthode Web GetCustomerIDs appelé par le contrôle AutoCompleteExtender indiqué plus haut dans la liste de 19. La méthode Web appelle une méthode de couche métier qu’à son tour appelle une de la couche données méthode qui gère le filtrage des données et en retournant les résultats de correspondance. Le code pour la méthode de la couche données est illustré à la liste de 21.

**Liste de 20. Filtrage des données envoyées à partir du contrôle AutoCompleteExtender.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample27.cs)]

**Liste 21. Filtrage des résultats en fonction des entrées de l’utilisateur final.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample28.cs)]

## <a name="conclusion"></a>Conclusion

ASP.NET AJAX fournit une excellente prise en charge pour l’appel des Services Web sans écrire beaucoup de code JavaScript personnalisé pour gérer les messages de demande et de réponse. Dans cet article, vous avez vu comment les Services Web compatible AJAX .NET pour leur permettre de traiter des messages JSON et comment définir les proxys JavaScript en utilisant le contrôle ScriptManager. Vous avez également vu comment JavaScript proxys peuvent être utilisées pour appeler des Services Web, gérer des types simples et complexes et gérer les échecs. Enfin, vous avez vu comment les méthodes de page peuvent être utilisées pour simplifier le processus de création et d’effectuer des appels de Service Web et comment le contrôle AutoCompleteExtender peut permettre aux utilisateurs finaux frappe. Bien que le contrôle UpdatePanel disponible dans ASP.NET AJAX sera certainement le contrôle de choix pour nombreux programmeurs AJAX en raison de sa simplicité, en sachant comment appeler des Services Web par proxy JavaScript peut être utile dans de nombreuses applications.

## <a name="bio"></a>Biographie

Dan Wahlin (Microsoft Most Valuable Professional pour ASP.NET et des Services Web XML) est développement instructor et architecture consultant .NET à la formation technique d’Interface ([http://www.interfacett.com](http://www.interfacett.com)). Dan fondé le code XML pour le site Web des développeurs ASP.NET ([www.XMLforASP.NET](http://www.XMLforASP.NET)), se trouve sur le Bureau de l’INETA et participe à des conférences plusieurs. Dan co-écrit Professional Windows DNA (Wrox), ASP.NET : conseils, didacticiels et Code (SAM), des Solutions ASP.NET 1.1 Insider, Professional ASP.NET 2.0 AJAX (Wrox), ASP.NET 2.0 MVP Hacks et XML créé pour les développeurs ASP.NET (SAM). Lorsqu’il n’écrit pas de code, des articles ou des livres, Dan bénéficie d’écriture et d’enregistrement de musique et de lecture de golf et basket-ball avec sa femme et les enfants.

Scott Cate travaille avec les technologies Web Microsoft depuis 1997 et est le président de myKB.com ([www.myKB.com](http://www.myKB.com)) où il est spécialisé dans l’écriture d’ASP.NET en fonction des applications axées sur les solutions logicielles de la Base de connaissances. Scott peut être contacté par courrier électronique en [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) ou son blog à l’adresse [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Précédent](understanding-asp-net-ajax-localization.md)
> [Suivant](understanding-asp-net-ajax-debugging-capabilities.md)
