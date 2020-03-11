---
title: Empêcher les attaques par redirection ouverte dans ASP.NET Core
author: ardalis
description: Montre comment empêcher les attaques de redirection ouvertes contre une application ASP.NET Core
ms.author: riande
ms.date: 07/07/2017
uid: security/preventing-open-redirects
ms.openlocfilehash: 9d8cac8708fe9aeadba5af1287362a20df7f6bfe
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660523"
---
# <a name="prevent-open-redirect-attacks-in-aspnet-core"></a>Empêcher les attaques par redirection ouverte dans ASP.NET Core

Une application web qui effectue une redirection vers une URL qui est spécifiée via la demande tels que les données de chaîne de requête ou un formulaire peut potentiellement être falsifiée en redirigant les utilisateurs vers une URL externe malveillante. Cette manipulation est appelée une attaque de redirection ouverte.

Chaque fois que votre logique d’application redirige vers une URL spécifiée, vous devez vérifier que l’URL de redirection n’a pas été falsifiée. ASP.NET Core intègre des fonctionnalités pour aider à protéger les applications contre les attaques de redirection ouverte (également appelée open redirection).

## <a name="what-is-an-open-redirect-attack"></a>Qu’est une attaque de redirection ouverte ?

Les applications Web redirigent fréquemment les utilisateurs vers une page de connexion lorsqu’ils accèdent aux ressources qui requièrent une authentification. La redirection comprend généralement un paramètre `returnUrl` QueryString afin que l’utilisateur puisse être renvoyé à l’URL demandée à l’origine une fois qu’il a ouvert une session. Une fois que l’utilisateur s’authentifie, il est redirigé vers l’URL qui était initialement demandée.

Étant donné que l’URL de destination est spécifiée dans la chaîne de requête de la demande, un utilisateur malveillant peut falsifier la chaîne de requête. Une QueryString falsifiée peut permettre au site de rediriger l’utilisateur vers un site externe malveillant. Cette technique s’appelle une attaque de redirection (ou de redirection) ouverte.

### <a name="an-example-attack"></a>Un exemple d'attaque

Un utilisateur malveillant peut développer une attaque destinée à autoriser l’utilisateur malveillant à accéder aux informations d’identification d’un utilisateur ou à des informations sensibles. Pour lancer l’attaque, l’utilisateur malveillant persuade l’utilisateur de cliquer sur un lien vers la page de connexion de votre site avec une valeur QueryString de `returnUrl` ajoutée à l’URL. Par exemple, considérez une application sur `contoso.com` qui comprend une page de connexion sur `http://contoso.com/Account/LogOn?returnUrl=/Home/About`. L’attaque suit les étapes suivantes :

1. L’utilisateur clique sur un lien malveillant pour `http://contoso.com/Account/LogOn?returnUrl=http://contoso1.com/Account/LogOn` (la deuxième URL est « contoso**1**. com », et non pas « contoso.com »).
2. L’utilisateur se connecte avec succès.
3. L’utilisateur est redirigé (par le site) vers `http://contoso1.com/Account/LogOn` (un site malveillant qui ressemble exactement à un site réel).
4. L’utilisateur se reconnecte (ce qui donne des informations d’identification au site malveillant) et est redirigé vers le site réel.

L’utilisateur pense probablement que sa première tentative de connexion a échoué et que la deuxième tentative est réussie. L’utilisateur n’est probablement pas conscient que leurs informations d’identification sont compromises.

![Processus d’attaque Redirection ouverte](preventing-open-redirects/_static/open-redirection-attack-process.png)

En plus des pages de connexion, certains sites fournissent des points de terminaison ou des pages de redirection. Imaginez que votre application comporte une page avec une redirection ouverte, `/Home/Redirect`. Une personne malveillante pourrait créer, par exemple, un lien dans un message électronique qui mène à `[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login`. Un utilisateur voit que l’URL commence par le nom de votre site. Faisant confiance,il cliquera sur le lien. La redirection ouverte envoie ensuite l’utilisateur vers le site de hameçonnage, qui est identique au vôtre, et l’utilisateur probablement se connectera pensant que c'est votre site.

## <a name="protecting-against-open-redirect-attacks"></a>Protection contre les attaques de redirection ouverte

Lors du développement d’applications Web, traitez toutes les données fournies par l’utilisateur comme non fiables. Si votre application a une fonctionnalité qui redirige l’utilisateur en fonction du contenu de l’URL, assurez-vous que ces redirections sont effectuées uniquement localement dans votre application (ou sur une URL connue, et non dans une URL qui peut être fournie dans la chaîne de chaîne).

### <a name="localredirect"></a>LocalRedirect

Utilisez la méthode d’assistance `LocalRedirect` de la classe de `Controller` de base :

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

`LocalRedirect` lèvera une exception si une URL non locale est spécifiée. Dans le cas contraire, il se comporte comme la méthode `Redirect`.

### <a name="islocalurl"></a>IsLocalUrl

Utilisez la méthode [IsLocalUrl](/dotnet/api/Microsoft.AspNetCore.Mvc.IUrlHelper.islocalurl#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) pour tester des URL avant de rediriger :

L’exemple suivant montre comment vérifier si une URL est locale avant la redirection.

```csharp
private IActionResult RedirectToLocal(string returnUrl)
{
    if (Url.IsLocalUrl(returnUrl))
    {
        return Redirect(returnUrl);
    }
    else
    {
        return RedirectToAction(nameof(HomeController.Index), "Home");
    }
}
```

La méthode `IsLocalUrl` empêche les utilisateurs d’être redirigés par inadvertance vers un site malveillant. Vous pouvez consigner les détails de l’URL qui a été fourni lorsqu’une URL non local est fournie dans une situation dans laquelle vous vous attendiez une URL locale. La journalisation des redirection d'URL peuvent faciliter le diagnostic des attaques de redirection.
