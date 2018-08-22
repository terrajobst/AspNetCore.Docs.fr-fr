---
title: Empêcher les attaques par redirection ouverte dans ASP.NET Core
author: ardalis
description: Montre comment empêcher les attaques par redirection ouverte par rapport à une application ASP.NET Core
ms.author: riande
ms.date: 07/07/2017
uid: security/preventing-open-redirects
ms.openlocfilehash: 0896189d2caaccb19647eb7c6d57f29dfc0290dd
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835372"
---
# <a name="prevent-open-redirect-attacks-in-aspnet-core"></a>Empêcher les attaques par redirection ouverte dans ASP.NET Core

Une application web qui effectue une redirection vers une URL qui est spécifiée via la demande tels que les données de chaîne de requête ou un formulaire peut potentiellement être falsifiée en redirigant les utilisateurs vers une URL externe malveillante. Cette manipulation est appelée une attaque de redirection ouverte.

Chaque fois que votre logique d’application redirige vers une URL spécifiée, vous devez vérifier que l’URL de redirection n’a pas été falsifiée. ASP.NET Core intègre des fonctionnalités pour aider à protéger les applications contre les attaques de redirection ouverte (également appelée open redirection).

## <a name="what-is-an-open-redirect-attack"></a>Qu’est une attaque de redirection ouverte ?

Les applications Web redirigent fréquemment les utilisateurs vers une page de connexion lorsqu’ils accèdent aux ressources qui requièrent une authentification. La redirection inclut généralement une `returnUrl` paramètre querystring afin que l’utilisateur peut être retournée à l’URL demandée à l’origine une fois qu’ils ont connecté avec succès. Une fois que l’utilisateur s’authentifie, il est redirigé vers l’URL qui était initialement demandée.

Étant donné que l’URL de destination est spécifié dans la chaîne de requête de la demande, un utilisateur malveillant peut falsifier la chaîne de requête. Une chaîne de requête falsifiée pourrait permettre le site rediriger l’utilisateur vers un site externe, malveillant. Cette technique est appelée une attaque de redirection (ou une redirection) ouverte.

### <a name="an-example-attack"></a>Un exemple d'attaque

Un utilisateur malveillant peut développer une attaque conçue pour permettre l’accès utilisateur malveillant pour les informations d’identification d’un utilisateur ou les informations sensibles. Pour commencer l’attaque, l’utilisateur malveillant malveillant convainc l’utilisateur clique sur un lien vers la page de connexion de votre site avec un `returnUrl` valeur de chaîne de requête ajoutée à l’URL. Par exemple, considérez une application à l’adresse `contoso.com` qui inclut une page de connexion à `http://contoso.com/Account/LogOn?returnUrl=/Home/About`. L’attaque procédez comme suit :

1. L’utilisateur clique sur un lien malveillant vers `http://contoso.com/Account/LogOn?returnUrl=http://contoso1.com/Account/LogOn` (l’URL du deuxième est « contoso**1**.com », pas « contoso.com »).
2. L’utilisateur parvient à se connecter.
3. L’utilisateur est redirigé (par le site) à `http://contoso1.com/Account/LogOn` (un site malveillant qui se présente exactement comme vrai site).
4. L’utilisateur se connecte à nouveau (donnant malveillant leurs informations d’identification de site) et est redirigé vers le site réel.

L’utilisateur probable est convaincu que leur première tentative de connexion a échoué et que leur seconde tentative a réussi. L’utilisateur reste très probablement pas au courant que leurs informations d’identification sont compromises.

![Processus d’attaque Redirection ouverte](preventing-open-redirects/_static/open-redirection-attack-process.png)

En plus des pages de connexion, certains sites fournissent des points de terminaison ou des pages de redirection. Imaginez que votre application comprend une page avec une redirection ouverte, `/Home/Redirect`. Un attaquant pourrait créer, par exemple, un lien dans un message électronique qui accède à `[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login`. Un utilisateur voit que l’URL commence par le nom de votre site. Faisant confiance,il cliquera sur le lien. La redirection ouverte envoie ensuite l’utilisateur vers le site de hameçonnage, qui est identique au vôtre, et l’utilisateur probablement se connectera pensant que c'est votre site.

## <a name="protecting-against-open-redirect-attacks"></a>Protection contre les attaques de redirection ouverte

Lors du développement d’applications web, traiter toutes les données fournies par l’utilisateur comme non fiables. Si votre application possède des fonctionnalités qui redirige l’utilisateur en fonction du contenu de l’URL, assurez-vous que ces redirections sont uniquement effectuées localement dans votre application (ou à une URL connue, pas n’importe quelle URL qui peut être fournie dans la chaîne de requête).

### <a name="localredirect"></a>LocalRedirect

Utilisez la méthode helper `LocalRedirect` à partir de la classe de base de `Controller`:

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

`LocalRedirect` lève une exception si une URL non local est spécifiée. Sinon, il se comporte comme le `Redirect` (méthode).

### <a name="islocalurl"></a>IsLocalUrl

Utilisez la méthode [IsLocalUrl](/dotnet/api/Microsoft.AspNetCore.Mvc.IUrlHelper?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) pour tester l’URL avant de rediriger les utlisateurs:

L’exemple suivant montre comment vérifier si une URL est locale avant de rediriger.

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

La méthde `IsLocalUrl` protège les utilisateurs d'être redirigés par inadvertance vers un site malveillant. Vous pouvez consigner les détails de l’URL qui a été fourni lorsqu’une URL non local est fournie dans une situation dans laquelle vous vous attendiez une URL locale. La journalisation des redirection d'URL peuvent faciliter le diagnostic des attaques de redirection.
