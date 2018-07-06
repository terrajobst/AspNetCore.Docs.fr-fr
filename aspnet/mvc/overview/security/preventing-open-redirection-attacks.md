---
uid: mvc/overview/security/preventing-open-redirection-attacks
title: Prévention des attaques par Redirection ouverte (c#) | Microsoft Docs
author: jongalloway
description: Ce didacticiel explique comment vous pouvez empêcher les attaques par redirection ouverte dans vos applications ASP.NET MVC. Ce didacticiel décrit les modifications qui ont été apportées...
ms.author: aspnetcontent
ms.date: 02/27/2014
ms.assetid: 69fb02e0-f5b7-4c35-878c-fa87164fc785
msc.legacyurl: /mvc/overview/security/preventing-open-redirection-attacks
msc.type: authoredcontent
ms.openlocfilehash: 33d2d050805d9b65741c121cdb2b65a59e1ea392
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813198"
---
<a name="preventing-open-redirection-attacks-c"></a>Prévention des attaques par Redirection ouverte (c#)
====================
par [Jon Galloway](https://github.com/jongalloway)

> Ce didacticiel explique comment vous pouvez empêcher les attaques par redirection ouverte dans vos applications ASP.NET MVC. Ce didacticiel décrit les modifications qui ont été apportées dans le contrôle AccountController dans ASP.NET MVC 3 et montre comment vous pouvez appliquer ces modifications dans votre ASP.NET MVC 1.0 existante et 2 applications.


## <a name="what-is-an-open-redirection-attack"></a>Qu’est une attaque de Redirection ouverte ?

N’importe quelle application web qui redirige vers une URL qui est spécifiée par le biais de la demande tels que les données de chaîne de requête ou un formulaire peut potentiellement être falsifiée rediriger les utilisateurs vers une URL externe, malveillante. Cette manipulation est appelée une attaque de redirection ouverte.

Chaque fois que votre logique d’application redirige vers une URL spécifiée, vous devez vérifier que l’URL de redirection n’a pas été falsifiée. La connexion utilisée dans la valeur par défaut AccountController pour ASP.NET MVC 1.0 et ASP.NET MVC 2 est vulnérable aux attaques par redirection d’ouvrir. Heureusement, il est facile de mettre à jour vos applications existantes pour utiliser les corrections à partir de la version préliminaire de ASP.NET MVC 3.

Pour comprendre la vulnérabilité, examinons le fonctionnement de la redirection de connexion dans un projet d’Application Web de ASP.NET MVC 2 par défaut. Dans cette application, essayez de visiter une action de contrôleur qui possède l’attribut [Authorize] redirigera les utilisateurs non autorisés à la vue /Account/LogOn. Cette redirection vers /Account/LogOn inclut un paramètre de chaîne de requête returnUrl afin que l’utilisateur peut être retournée à l’URL demandée à l’origine une fois qu’ils ont connecté avec succès.

Dans la capture d’écran ci-dessous, nous constatons qu’une tentative d’accès de la vue /Account/ChangePassword lorsque vous n’êtes ne pas connecté entraîne une redirection vers /Account/LogOn ? ReturnUrl = % 2fAccount % 2fChangePassword % 2f.

[![](preventing-open-redirection-attacks/_static/image2.png)](preventing-open-redirection-attacks/_static/image1.png)

**Figure 01**: page de connexion avec une redirection ouverte

Dans la mesure où le paramètre de chaîne de requête ReturnUrl n’est pas validé, une personne malveillante peut le modifier pour injecter de n’importe quelle adresse URL dans le paramètre pour mener une attaque par redirection ouverte. Pour illustrer cela, nous pouvons modifier le paramètre ReturnUrl à [ http://bing.com ](http://bing.com), de sorte que l’URL de connexion qui en résulte sera/compte / d’ouverture de session ? ReturnUrl =<http://www.bing.com/>. Lors de la connexion avec succès vers le site, nous sommes redirigés vers [ http://bing.com ](http://bing.com). Dans la mesure où cette redirection n’est pas validée, il pourrait pointer à la place vers un site malveillant qui tente de tromper l’utilisateur.

### <a name="a-more-complex-open-redirection-attack"></a>Une attaque de Redirection ouverte plus complexes

Attaques par redirection ouverte sont particulièrement dangereuses, car un attaquant sait que nous essayons pour vous connecter à un site Web spécifique, ce qui nous rend vulnérable à un [attaque par phishing](https://www.microsoft.com/protect/fraud/phishing/symptoms.aspx). Par exemple, un attaquant pourrait envoyer des messages électroniques malveillants pour les utilisateurs du site Web dans une tentative pour capturer leurs mots de passe. Nous allons voir comment cela fonctionnerait sur le site de NerdDinner. (Notez que le site de NerdDinner actif a été mis à jour pour vous protéger contre les attaques par redirection ouverte).

Tout d’abord, une personne malveillante nous envoie un lien vers la page de connexion sur NerdDinner qui inclut une redirection vers leurs pages contrefaites :

[http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn](http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn)

Notez que l’URL de retour pointe vers nerddiner.com, ce qui n’a pas un « n » à partir du dîner de word. Dans cet exemple, il s’agit d’un domaine qui contrôle l’attaquant. Lorsque nous accédez le lien ci-dessus, nous allons dirigés vers la page de connexion NerdDinner.com légitime.

[![](preventing-open-redirection-attacks/_static/image4.png)](preventing-open-redirection-attacks/_static/image3.png)

**Figure 02**: page de connexion de NerdDinner avec une redirection ouverte

Lorsque nous connecter correctement, action d’ouverture de session de l’ASP.NET MVC du contrôle AccountController nous redirige vers l’URL spécifiée dans le paramètre de chaîne de requête returnUrl. Dans ce cas, il est l’URL qui est passée à l’attaquant, qui est [ http://nerddiner.com/Account/LogOn ](http://nerddiner.com/Account/LogOn). À moins que nous sommes extrêmement sont, qu'il est très probable que nous ne remarquerez pas cela, notamment parce que l’attaquant a veillé à vous assurer que leurs pages contrefaites est identique à la page de connexion légitime. Cette page de connexion inclut un message d’erreur demandant que nous connecter à nouveau. Maladroit nous, nous devons la saisie incorrecte notre mot de passe.

[![](preventing-open-redirection-attacks/_static/image6.png)](preventing-open-redirection-attacks/_static/image5.png)

**Figure 03**: écran de connexion de NerdDinner falsifiées

Lorsque nous retapez notre nom d’utilisateur et le mot de passe, la page de connexion falsifiées enregistre les informations et nous envoie vers le site de NerdDinner.com légitime. À ce stade, le site de NerdDinner.com a déjà authentifié us, afin de la page de connexion falsifiées permettre rediriger directement vers cette page. Le résultat final est que l’attaquant a notre nom d’utilisateur et le mot de passe, et nous ne savent pas que nous vous avons fourni il leur.

## <a name="looking-at-the-vulnerable-code-in-the-accountcontroller-logon-action"></a>Le code vulnérable à une Action d’ouverture de session AccountController

Le code pour l’action d’ouverture de session dans une application ASP.NET MVC 2 est indiqué ci-dessous. Notez que lors d’une connexion réussie, le contrôleur retourne une redirection vers l’élément returnUrl. Vous pouvez voir qu’aucune validation n’est effectuée sur le paramètre returnUrl.

**Liste 1 – action d’ouverture de session ASP.NET MVC 2 dans `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample1.cs)]

Maintenant nous allons examiner les modifications apportées à l’action d’ouverture de session ASP.NET MVC 3. Ce code a été modifié pour valider le paramètre returnUrl en appelant une nouvelle méthode dans la classe d’assistance System.Web.Mvc.Url nommée `IsLocalUrl()`.

**Listing 2 – action d’ouverture de session ASP.NET MVC 3 dans `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample2.cs)]

Cela a été modifié pour valider le paramètre URL de retour en appelant une nouvelle méthode dans la classe d’assistance System.Web.Mvc.Url, `IsLocalUrl()`.

## <a name="protecting-your-aspnet-mvc-10-and-mvc-2-applications"></a>Protection ASP.NET MVC 1.0 et MVC 2 vos Applications

Nous pouvons tirer parti des modifications d’ASP.NET MVC 3 dans notre ASP.NET MVC 1.0 existante et 2 applications en ajoutant la méthode d’assistance IsLocalUrl() et de la mise à jour de l’action d’ouverture de session pour valider le paramètre returnUrl.

La méthode UrlHelper IsLocalUrl() en fait appel à une méthode dans System.Web.WebPages, en tant que cette validation est également utilisée par les applications ASP.NET Web Pages.

**Liste 3 – la méthode IsLocalUrl() à partir de la UrlHelper de 3 ASP.NET MVC `class`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample3.cs)]

La méthode IsUrlLocalToHost contient la logique de validation réelle, comme indiqué sur la liste 4.

**Liste 4 – méthode IsUrlLocalToHost() à partir de la classe System.Web.WebPages RequestExtensions**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample4.cs)]

Dans notre ASP.NET MVC 1.0 ou une application 2, nous allons ajouter une méthode IsLocalUrl() à contrôle AccountController, mais vous êtes invités à ajouter à une classe d’assistance distincte si possible. Nous apporterons deux petites modifications à la version d’ASP.NET MVC 3 de IsLocalUrl() pour qu’il fonctionne à l’intérieur du contrôle AccountController. Tout d’abord, nous allons modifier cela à partir d’une méthode publique à une méthode privée, étant donné que les méthodes publiques dans les contrôleurs sont accessibles en tant qu’actions de contrôleur. Ensuite, nous allons modifier l’appel qui vérifie l’hôte de l’URL par rapport à l’hôte d’application. Qu’appel utilise un RequestContext local champ dans la classe UrlHelper. Au lieu d’utiliser cela. RequestContext.HttpContext.Request.Url.Host, nous utiliserons ce. Request.Url.Host. Le code suivant montre la méthode IsLocalUrl() modifiée pour une utilisation avec une classe de contrôleur dans ASP.NET MVC 1.0 et 2 applications.

**Liste 5 – IsLocalUrl() (méthode), qui est modifiée pour une utilisation avec une classe de contrôleur MVC**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample5.cs)]

Maintenant que la méthode IsLocalUrl() est en place, nous pouvons l’appeler à partir de notre action d’ouverture de session pour valider le paramètre returnUrl, comme indiqué dans le code suivant.

**Liste 6 – méthode d’ouverture de session de mise à jour qui valide le paramètre returnUrl**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample6.cs)]

Maintenant, nous pouvons tester une attaque par redirection ouverte à une tentative de connexion à l’aide d’une URL de retournée externe. Nous allons utiliser /Account/LogOn ? ReturnUrl =<http://www.bing.com/> à nouveau.

[![](preventing-open-redirection-attacks/_static/image8.png)](preventing-open-redirection-attacks/_static/image7.png)

**Figure 04**: test de l’Action d’ouverture de session de mise à jour

Après vous être connecté avec succès, nous sommes redirigés à l’action du contrôleur de Home/Index plutôt qu’à l’URL externe.

[![](preventing-open-redirection-attacks/_static/image10.png)](preventing-open-redirection-attacks/_static/image9.png)

**Figure 05**: attaque par Redirection ouverte vaincu

## <a name="summary"></a>Récapitulatif

Attaques par redirection ouverte peuvent se produire lors de la redirection URL lui sont passées en tant que paramètres dans l’URL pour une application. ASP.NET MVC 3, modèle inclut le code pour vous protéger contre les attaques par redirection d’ouvrir. Vous pouvez ajouter ce code avec quelques modifications à ASP.NET MVC 1.0 et 2 applications. Pour vous protéger contre les attaques par redirection ouverte lors de la journalisation dans ASP.NET 1.0 et 2 applications, ajoutez une méthode IsLocalUrl() et valider le paramètre returnUrl dans l’action d’ouverture de session.
