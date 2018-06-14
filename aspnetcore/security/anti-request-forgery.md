---
title: Attaques empêcher Cross-Site Request Forgery (XSRF/CSRF) dans ASP.NET Core
author: steve-smith
description: Découvrez comment éviter les attaques contre les applications web où un site Web malveillant peut influencer l’interaction entre un navigateur client et l’application.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/anti-request-forgery
ms.openlocfilehash: 3bca96f4a2e247eeeb93140df93221371d88d4d3
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/12/2018
ms.locfileid: "35341858"
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a>Attaques empêcher Cross-Site Request Forgery (XSRF/CSRF) dans ASP.NET Core

Par [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), et [Rick Anderson](https://twitter.com/RickAndMSFT)

Falsification de requête (également appelé XSRF ou CSRF, prononcé *voir-navigation*) est une attaque contre les applications hébergées par le web dans laquelle une application web malveillant peut influencer l’interaction entre un navigateur client et une application web qui approuve qui Navigateur. Ces attaques sont possible, car certains types de jetons d’authentification automatiquement avec chaque demande d’envoi de navigateurs web à un site Web. Cette forme d’attaque est également appelé un *en un clic attaque* ou *vol de session* puisque l’attaque tire parti de l’utilisateur d’authentifié précédemment de session.

Voici un exemple d’une attaque CSRF :

1. Un utilisateur se connecte à `www.good-banking-site.com` à l’aide de l’authentification par formulaire. Le serveur authentifie l’utilisateur et émet une réponse qui inclut un cookie d’authentification. Le site est vulnérable aux attaques, car il fait confiance à toute demande qu’il reçoit avec un cookie d’authentification valide.
1. L’utilisateur visite un site malveillant, `www.bad-crook-site.com`.

   Le site malveillant, `www.bad-crook-site.com`, contient un formulaire HTML semblable au suivant :

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   Notez que du formulaire `action` publications sur le site vulnérable, pas pour le site malveillant. Il s’agit de la partie « cross-site » de CSRF.

1. L’utilisateur sélectionne le bouton Envoyer. Le navigateur d'où émane la demande et inclut automatiquement le cookie d’authentification pour le domaine demandé, `www.good-banking-site.com`.
1. La requête s’exécute le `www.good-banking-site.com` serveur avec le contexte de l’utilisateur d’authentification et peut exécuter toute action qu’un utilisateur authentifié est autorisé à effectuer.

Outre le scénario où l’utilisateur sélectionne le bouton pour envoyer le formulaire, le site malveillant pourrait :

* Exécuter un script qui envoie automatiquement le formulaire.
* Envoyer l’envoi du formulaire sous la forme d’une requête AJAX.
* Masquer le formulaire à l’aide de CSS.

Ces scénarios ne nécessitent pas toute action ou une entrée d’utilisateur autre qu’initialement sur le site malveillant.

Une attaque CSRF n’empêche pas à l’aide de HTTPS. Le site malveillant peut envoyer un `https://www.good-banking-site.com/` demander tout aussi facilement qu’il peut envoyer une demande non sécurisée.

Certaines attaques de ciblent des points de terminaison qui répondent aux demandes GET, auquel cas une balise d’image peut être utilisée pour effectuer l’action. Cette forme d’attaque est courant sur les sites de forum qui autorisent les images mais bloquent JavaScript. Les applications qui modifient l’état sur les demandes GET, où les variables ou les ressources sont modifiés, sont vulnérables aux attaques malveillantes. **Les requêtes GET qui changent d’état sont non sécurisés. Une meilleure pratique consiste à ne jamais changer l’état sur une requête GET.**

Les attaques CSRF sont possibles par rapport aux applications web qui utilisent des cookies pour l’authentification, car :

* Navigateurs stockent des cookies émis par une application web.
* Les cookies stockés incluent les cookies de session pour les utilisateurs authentifiés.
* Les navigateurs envoient que tous les cookies associés à un domaine à l’application web chaque demande, quelle que soit la façon dont la demande d’application a été générée dans le navigateur.

Toutefois, les attaques CSRF ne sont pas limitées à exploiter les cookies. Par exemple, l’authentification de base et Digest sont également vulnérables. Une fois un utilisateur se connecte avec l’authentification de base ou Digest, le navigateur envoie automatiquement les informations d’identification jusqu'à ce que la session&dagger; se termine.

&dagger;Dans ce contexte, *session* fait référence à la session côté client au cours de laquelle l’utilisateur est authentifié. Il est sans rapport avec les sessions côté serveur ou [Middleware de Session ASP.NET Core](xref:fundamentals/app-state).

Les utilisateurs peuvent se protéger contre les vulnérabilités CSRF en prenant les précautions :

* Signature des applications web une fois leur utilisation.
* Cookies du navigateur effacer périodiquement.

Toutefois, les vulnérabilités CSRF sont fondamentalement un problème avec l’application web, pas l’utilisateur final.

## <a name="authentication-fundamentals"></a>Notions de base de l’authentification

L’authentification basée sur le cookie est une forme courante d’authentification. Systèmes d’authentification par jeton gagnent en popularité, en particulier pour les Applications à Page unique (ZPS).

### <a name="cookie-based-authentication"></a>Authentification par cookie

Lorsqu’un utilisateur s’authentifie à l’aide de leur nom d’utilisateur et un mot de passe, ils sont émis un jeton contenant un ticket d’authentification qui peut être utilisé pour l’authentification et d’autorisation. Le jeton est stocké en tant que permet d’un cookie qui accompagne chaque demande du client. Génération et la validation de ce cookie sont effectuée par l’intergiciel (middleware) d’authentification de Cookie. Le [intergiciel (middleware)](xref:fundamentals/middleware/index) sérialise un principal d’utilisateur dans un cookie chiffré. Pour les demandes suivantes, l’intergiciel (middleware) valide le cookie, recrée le principal et l’affecte au principal de la [utilisateur](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) propriété du [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).

### <a name="token-based-authentication"></a>Authentification basée sur un jeton

Lorsqu’un utilisateur est authentifié, ils sont émis un jeton (pas un jeton côté). Le jeton contient des informations de l’utilisateur sous la forme de [revendications](/dotnet/framework/security/claims-based-identity-model) ou un jeton de référence qui pointe l’application à l’état utilisateur mis à jour dans l’application. Lorsqu’un utilisateur tente d’accéder à une ressource nécessitant une authentification, le jeton est envoyé à l’application avec un en-tête d’autorisation supplémentaires sous forme de jeton de support. Cela rend l’application sans état. Dans chaque demande ultérieure, le jeton est passé dans la demande pour la validation côté serveur. Ce jeton n’est pas *chiffrées*; il a *codé*. Sur le serveur, le jeton est décodé pour accéder à ses informations. Pour envoyer le jeton pour les demandes suivantes, stockez le jeton dans le stockage local du navigateur. Ne soyez pas inquiet de la vulnérabilité CSRF si le jeton est stocké dans le stockage local du navigateur. CSRF est un problème lorsque le jeton est stocké dans un cookie.

### <a name="multiple-apps-hosted-at-one-domain"></a>Plusieurs applications hébergées sur un domaine

Environnements d’hébergement partagés sont vulnérables au piratage de session, la connexion CSRF et autres attaques.

Bien que `example1.contoso.net` et `example2.contoso.net` sont des hôtes différents, il existe une relation de confiance implicite entre les hôtes sous le `*.contoso.net` domaine. Cette relation de confiance implicite permet à des hôtes potentiellement non fiables affecter l’autre les cookies (les même origine les stratégies qui régissent les requêtes AJAX ne pas nécessairement appliquer les cookies HTTP).

Pour empêcher les attaques exploitant les cookies de confiance entre les applications hébergées sur le même domaine, ne partage ne pas de domaines. Lorsque chaque application est hébergée sur son propre domaine, il n’existe aucune relation d’approbation implicite de cookie à exploiter.

## <a name="aspnet-core-antiforgery-configuration"></a>Configuration de côté ASP.NET Core

> [!WARNING]
> ASP.NET Core implémente à l’aide de côté [Protection des données ASP.NET Core](xref:security/data-protection/introduction). La pile de protection des données doit être configurée pour fonctionner dans une batterie de serveurs. Consultez [configuration de la protection des données](xref:security/data-protection/configuration/overview) pour plus d’informations.

Dans ASP.NET Core 2.0 ou version ultérieure, le [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injecte des jetons côtés dans les éléments de formulaire HTML. Le balisage suivant dans un fichier Razor génère automatiquement des jetons côtés :

```cshtml
<form method="post">
    ...
</form>
```

De même, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) génère des jetons côtés par défaut si la méthode du formulaire n’est pas GET.

La génération automatique de jetons de côté pour les éléments de formulaire HTML se produit lorsque le `<form>` balise contient le `method="post"` attribut et une des opérations suivantes sont remplie :

  * L’attribut d’action est vide (`action=""`).
  * L’attribut d’action n’est pas fourni (`<form method="post">`).

Vous pouvez désactiver la génération automatique des jetons de côté pour les éléments de formulaire HTML :

* Désactiver explicitement les jetons côtés avec la `asp-antiforgery` attribut :

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* L’élément de formulaire est choisi par des applications d’assistance de balise à l’aide du programme d’assistance de balise [! annulations symbole](xref:mvc/views/tag-helpers/intro#opt-out):

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* Supprimer le `FormTagHelper` à partir de la vue. Le `FormTagHelper` peut être supprimé à partir d’une vue en ajoutant la directive suivante à la vue Razor :

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> [Pages Razor](xref:mvc/razor-pages/index) sont automatiquement protégés contre XSRF/CSRF. Pour plus d’informations, consultez [XSRF/CSRF et Pages Razor](xref:mvc/razor-pages/index#xsrf).

L’approche la plus courante de défense contre les attaques CSRF consiste à utiliser le *du modèle de jeton synchronisateur* (STP). STP est utilisé lorsque l’utilisateur demande une page de données de formulaire :

1. Le serveur envoie un jeton avec l’identité de l’utilisateur actuel au client.
1. Le client envoie le jeton sur le serveur pour la vérification.
1. Si le serveur reçoit un jeton qui ne correspond pas à identité de l’utilisateur authentifié, la demande est rejetée.

Le jeton est unique et imprévisibles. Le jeton peut également être utilisé pour garantir la mise en séquence correcte d’une série de demandes (par exemple, vous assurer de la séquence de demande : la page 1 &ndash; page 2 &ndash; page 3). Tous les formulaires dans les modèles ASP.NET MVC de base et les Pages Razor génèrent des jetons de côté. Les deux exemples de vue suivants génèrent des jetons côtés :

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

Ajouter explicitement un jeton à côté d’un `<form>` élément sans l’aide de programmes d’assistance de balise avec l’application d’assistance HTML [ @Html.AntiForgeryToken ](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

Dans chacun des cas précédents, ASP.NET Core ajoute un champ de formulaire masqué semblable au suivant :

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

ASP.NET Core inclut trois [filtres](xref:mvc/controllers/filters) pour l’utilisation des jetons de côté :

* [ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a>Options de côté

Personnaliser [options côtées](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) dans `Startup.ConfigureServices`:

```csharp
services.AddAntiforgery(options => 
{
    options.CookieDomain = "contoso.com";
    options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
    options.CookiePath = "Path";
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.RequireSsl = false;
    options.SuppressXFrameOptionsHeader = false;
});
```

| Option | Description |
| ------ | ----------- |
| [Cookie](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | Détermine les paramètres utilisés pour créer les cookies côtés. |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | Le domaine du cookie. La valeur par défaut est `null`. Cette propriété est obsolète et sera supprimée dans une future version. L’alternative recommandée est Cookie.Domain. |
| [CookieName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | Le nom du cookie. Si non définie, le système génère un nom unique commençant par le [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) ( ». AspNetCore.Antiforgery. »). Cette propriété est obsolète et sera supprimée dans une future version. L’alternative recommandée est Cookie.Name. |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | Le chemin d’accès défini sur le cookie. Cette propriété est obsolète et sera supprimée dans une future version. L’alternative recommandée est Cookie.Path. |
| [FormFieldName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | Le nom du champ de formulaire masqué utilisé par le système de côté pour effectuer le rendu côté des jetons dans les vues. |
| [HeaderName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | Le nom de l’en-tête utilisé par le système de côté. Si `null`, le système considère uniquement les données de formulaire. |
| [RequireSsl](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | Spécifie si SSL est requise par le système de côté. Si `true`, les demandes non-SSL échouent. La valeur par défaut est `false`. Cette propriété est obsolète et sera supprimée dans une future version. L’alternative recommandée consiste à définir des Cookie.SecurePolicy. |
| [SuppressXFrameOptionsHeader](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | Spécifie s’il faut supprimer la génération de la `X-Frame-Options` en-tête. Par défaut, l’en-tête est généré avec la valeur « SAMEORIGIN ». La valeur par défaut est `false`. |

Pour plus d’informations, consultez [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).

## <a name="configure-antiforgery-features-with-iantiforgery"></a>Configurer les fonctionnalités côtées avec IAntiforgery

[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) fournit l’API pour configurer les fonctionnalités côtées. `IAntiforgery` peut être demandé dans le `Configure` méthode de la `Startup` classe. L’exemple suivant utilise l’intergiciel (middleware) à partir de la page d’accueil de l’application pour générer un jeton de côté et l’envoyer dans la réponse sous forme de cookie (à l’aide de la convention de dénomination angulaire par défaut est décrite plus loin dans cette rubrique) :

```csharp
public void Configure(IApplicationBuilder app, IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;

        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // The request token can be sent as a JavaScript-readable cookie, 
            // and Angular uses it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
}
```

### <a name="require-antiforgery-validation"></a>Exiger la validation côtée

[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) est un filtre d’action qui peut être appliqué à une action individuelle, un contrôleur, ou globalement. Requêtes adressées à des actions qui ont ce filtre est appliqué sont bloquées, sauf si la demande inclut un jeton valide de côté.

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();

    if (user != null)
    {
        var result = 
            await _userManager.RemoveLoginAsync(
                user, account.LoginProvider, account.ProviderKey);

        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }

    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

Le `ValidateAntiForgeryToken` attribut requiert un jeton pour les demandes pour les méthodes d’action qu’il décore, y compris les demandes HTTP GET. Si le `ValidateAntiForgeryToken` attribut est appliqué sur les contrôleurs de l’application, il peut être remplacé par le `IgnoreAntiforgeryToken` attribut.

> [!NOTE]
> ASP.NET Core ne prend en charge l’ajout de jetons de côté pour les demandes GET automatiquement.

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a>Valider automatiquement les jetons de côté pour les méthodes HTTP non sécurisés

Applications ASP.NET Core ne pas générer de jetons de côté pour les méthodes HTTP sécurisés (GET, HEAD, OPTIONS et TRACE). Au lieu d’appliquer globalement la `ValidateAntiForgeryToken` attribut et en remplaçant puis avec `IgnoreAntiforgeryToken` attributs, la [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) attribut peut être utilisé. Cet attribut fonctionne de manière identique à la `ValidateAntiForgeryToken` d’attribut, sauf qu’elle ne nécessite pas les jetons pour les demandes effectuées à l’aide des méthodes HTTP suivantes :

* GET
* HEAD
* OPTIONS
* TRACE

Nous recommandons l’utilisation de `AutoValidateAntiforgeryToken` largement pour les scénarios non-API. Cela garantit que les actions de publication sont protégées par défaut. L’alternative consiste à ignorer les jetons côtés par défaut, sauf si `ValidateAntiForgeryToken` est appliqué aux méthodes d’action individuelles. Il n’est plus probable dans ce scénario pour une méthode d’action POST à laisser pas protégé par erreur, vous quittez l’application vulnérable aux attaques CSRF. Toutes les publications doivent envoyer le jeton côté.

API n’ont aucun mécanisme automatique pour l’envoi de la partie non-cookie du jeton. L’implémentation probablement dépend de l’implémentation du code client. Certains exemples sont illustrés ci-dessous :

Exemple de niveau de la classe :

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

Exemple global :

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

### <a name="override-global-or-controller-antiforgery-attributes"></a>Remplacement global ou attributs de côté contrôleur

Le [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filtre est utilisé pour éliminer le besoin d’un jeton de côté pour une action donnée (ou un contrôleur). Quand il est appliqué, ce filtre remplace `ValidateAntiForgeryToken` et `AutoValidateAntiforgeryToken` filtres spécifiés à un niveau supérieur (globalement ou sur un contrôleur).

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
    [HttpPost]
    [IgnoreAntiforgeryToken]
    public async Task<IActionResult> DoSomethingSafe(SomeViewModel model)
    {
        // no antiforgery token required
    }
}
```

## <a name="refresh-tokens-after-authentication"></a>Jetons d’actualisation après l’authentification

Les jetons doivent être actualisées une fois que l’utilisateur est authentifié en redirigeant l’utilisateur à une vue ou d’une page de Pages Razor.

## <a name="javascript-ajax-and-spas"></a>JavaScript, AJAX et SPAs

Dans les applications traditionnelles basées sur HTML, jetons côtés sont transmis au serveur à l’aide des champs de formulaire masqué. Dans les applications modernes basées JavaScript et SPAs, le nombre de requêtes est effectuée par programme. Ces demandes AJAX peuvent utiliser d’autres techniques (par exemple, les en-têtes de requête ou des cookies) pour envoyer le jeton.

Si les cookies sont utilisés pour stocker les jetons d’authentification et pour authentifier les demandes sur le serveur de l’API, CSRF est un problème potentiel. Si le stockage local est utilisé pour stocker le jeton, une vulnérabilité CSRF peut être atténuée, car les valeurs à partir du stockage local ne sont pas automatiquement envoyées au serveur avec chaque demande. Par conséquent, l’utilisation du stockage local pour stocker le jeton côté sur le client et envoie le jeton, comme un en-tête de demande est une approche recommandée.

### <a name="javascript"></a>JavaScript

À l’aide de JavaScript avec les vues, le jeton peut être créé à l’aide d’un service à partir de la vue. Injecter la [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) service dans la vue et appelez [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

Cette approche élimine le besoin de traiter directement avec la définition des cookies à partir du serveur ou de les lire à partir du client.

L’exemple précédent utilise JavaScript pour lire la valeur du champ masqué pour l’en-tête AJAX POST.

JavaScript peut également accéder à des jetons dans des cookies et utiliser les contenu du cookie pour créer un en-tête avec sa valeur.

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

En supposant que le script demande à envoyer le jeton dans un en-tête appelé `X-CSRF-TOKEN`, configurer le côté service pour rechercher le `X-CSRF-TOKEN` en-tête :

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

L’exemple suivant utilise JavaScript pour effectuer une requête AJAX avec l’en-tête approprié :

```javascript
function getCookie(cname) {
    var name = cname + "=";
    var decodedCookie = decodeURIComponent(document.cookie);
    var ca = decodedCookie.split(';');
    for(var i = 0; i <ca.length; i++) {
        var c = ca[i];
        while (c.charAt(0) == ' ') {
            c = c.substring(1);
        }
        if (c.indexOf(name) == 0) {
            return c.substring(name.length, c.length);
        }
    }
    return "";
}

var csrfToken = getCookie("CSRF-TOKEN");

var xhttp = new XMLHttpRequest();
xhttp.onreadystatechange = function() {
    if (xhttp.readyState == XMLHttpRequest.DONE) {
        if (xhttp.status == 200) {
            alert(xhttp.responseText);
        } else {
            alert('There was an error processing the AJAX request.');
        }
    }
};
xhttp.open('POST', '/api/password/changepassword', true);
xhttp.setRequestHeader("Content-type", "application/json");
xhttp.setRequestHeader("X-CSRF-TOKEN", csrfToken);
xhttp.send(JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }));
```

### <a name="angularjs"></a>AngularJS

AngularJS utilise une convention à l’adresse CSRF. Si le serveur envoie un cookie avec le nom `XSRF-TOKEN`, le AngularJS `$http` service ajoute la valeur du cookie à un en-tête lorsqu’il envoie une demande au serveur. Ce processus est automatique. L’en-tête ne doit être définie explicitement. Le nom d’en-tête est `X-XSRF-TOKEN`. Le serveur doit détecter cet en-tête et valider son contenu.

Pour le travail de l’API ASP.NET principale avec la convention :

* Configurer votre application pour fournir un jeton dans un cookie appelé `XSRF-TOKEN`.
* Configurer le côté service pour rechercher un en-tête nommé `X-XSRF-TOKEN`.

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="extend-antiforgery"></a>Étendre antiforgery

Le [IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) permet aux développeurs d’étendre le comportement du système anti-CSRF par des données supplémentaires aller-retour dans chaque jeton. Le [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) méthode est appelée chaque fois qu’un jeton de champ est généré, et la valeur de retour est incorporée dans le jeton généré. Un implémenteur peut retourner un horodatage, une valeur à usage unique ou toute autre valeur, puis appelez [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) pour valider ces données lorsque le jeton est validé. Nom d’utilisateur du client est déjà incorporée dans les jetons générés, il est donc inutile d’inclure ces informations. Si un jeton contient des données supplémentaires, mais non `IAntiForgeryAdditionalDataProvider` est configuré, les données supplémentaires n’est pas validées.

## <a name="additional-resources"></a>Ressources supplémentaires

* [CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) sur [ouvrir le projet de sécurité d’Application Web](https://www.owasp.org/index.php/Main_Page) (OWASP avoir).
