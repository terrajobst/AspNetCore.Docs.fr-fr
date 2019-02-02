---
title: Attaques empêcher Cross-Site Request Forgery (XSRF/CSRF) dans ASP.NET Core
author: steve-smith
description: Découvrez comment éviter les attaques contre les applications web où un site Web malveillant peut influencer l’interaction entre un navigateur client et l’application.
ms.author: riande
ms.custom: mvc
ms.date: 10/11/2018
uid: security/anti-request-forgery
ms.openlocfilehash: 6e140717834b901e12ef7863fd07b983b0c55107
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667659"
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a>Attaques empêcher Cross-Site Request Forgery (XSRF/CSRF) dans ASP.NET Core

Par [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), et [Rick Anderson](https://twitter.com/RickAndMSFT)

Falsification de requête intersites (également appelé XSRF ou CSRF, prononcé *et navigation voir*) est une attaque contre les applications hébergées sur le web par lequel une application web malveillant peut influencer l’interaction entre un navigateur client et une application web qui fait confiance à qui Navigateur. Ces attaques sont possibles, car les navigateurs web envoient certains types de jetons d’authentification automatiquement avec chaque demande à un site Web. Cette forme de code malveillant est également appelé un *clic attaque* ou *par vol de session* , car l’attaque tire parti de l’utilisateur d’authentifié précédemment session.

Un exemple d’une attaque CSRF :

1. Un utilisateur se connecte à `www.good-banking-site.com` à l’aide de l’authentification par formulaire. Le serveur authentifie l’utilisateur et émet une réponse qui inclut un cookie d’authentification. Le site est vulnérable aux attaques, car il fait confiance à toute demande qu’il reçoit avec un cookie d’authentification valide.
1. L’utilisateur visite un site malveillant, `www.bad-crook-site.com`.

   Le site malveillant, `www.bad-crook-site.com`, contient un formulaire HTML similaire à ce qui suit :

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   Notez que le formulaire `action` publications sur le site vulnérable, et non vers le site malveillant. Il s’agit de la partie « cross-site » de falsification de requête Intersites.

1. L’utilisateur sélectionne le bouton Envoyer. Le navigateur effectue la demande et inclut automatiquement le cookie d’authentification pour le domaine demandé, `www.good-banking-site.com`.
1. La requête s’exécute sur le `www.good-banking-site.com` serveur avec un contexte de l’authentification de l’utilisateur et peut exécuter toute action pour laquelle un utilisateur authentifié est autorisé à effectuer.

Outre le scénario où l’utilisateur sélectionne le bouton Envoyer le formulaire, le site malveillant pourrait :

* Exécuter un script qui envoie automatiquement le formulaire.
* Envoyer l’envoi du formulaire sous la forme d’une requête AJAX.
* Masquer le formulaire à l’aide de CSS.

Ces scénarios alternatifs ne nécessitent aucune action ou une entrée de l’utilisateur autre qu’initialement visitant le site malveillant.

À l’aide de HTTPS n’empêche pas une attaque CSRF. Le site malveillant peut envoyer un `https://www.good-banking-site.com/` demander tout aussi facilement qu’il peut envoyer une requête non sécurisée.

Certaines attaques ciblent les points de terminaison qui répondent aux demandes GET, auquel cas une balise d’image peut être utilisée pour effectuer l’action. Ce type d’attaque est courant sur les sites de forum qui permettent des images mais bloquent JavaScript. Les applications qui modifient l’état sur les demandes GET, où les variables ou les ressources sont modifiés, sont vulnérables aux attaques malveillantes. **Les requêtes GET qui changent d’état sont non sécurisés. Une bonne pratique consiste à ne jamais changer l’état sur une requête GET.**

Les attaques CSRF sont possibles pour les applications web qui utilisent des cookies pour l’authentification, car :

* Navigateurs stockent des cookies émis par une application web.
* Les cookies stockés incluent les cookies de session pour les utilisateurs authentifiés.
* Les navigateurs envoient que tous les cookies associés à un domaine à l’application web chaque demande, quel que soit la façon dont la demande d’application a été générée dans le navigateur.

Toutefois, les attaques CSRF ne sont pas limitées à exploiter les cookies. Par exemple, l’authentification de base et Digest sont également vulnérables. Une fois un utilisateur se connecte avec l’authentification de base ou Digest, le navigateur envoie automatiquement les informations d’identification jusqu'à ce que la session&dagger; se termine.

&dagger;Dans ce contexte, *session* fait référence à la session côté client au cours de laquelle l’utilisateur est authentifié. Il est sans rapport avec les sessions côté serveur ou [Middleware de Session ASP.NET Core](xref:fundamentals/app-state).

Les utilisateurs peuvent se prémunir contre les vulnérabilités CSRF en prenant les précautions :

* Signature de l’applications web une fois que leur utilisation.
* Cookies du navigateur effacer périodiquement.

Toutefois, des vulnérabilités CSRF sont fondamentalement un problème avec l’application web, pas l’utilisateur final.

## <a name="authentication-fundamentals"></a>Principes de base de l’authentification

L’authentification basée sur le cookie est un formulaire populaires d’authentification. Systèmes d’authentification par jeton gagnent en popularité, en particulier pour les Applications à Page unique (SPA).

### <a name="cookie-based-authentication"></a>Authentification basée sur les cookies

Lorsqu’un utilisateur s’authentifie à l’aide de leur nom d’utilisateur et le mot de passe, ils sont émis un jeton contenant un ticket d’authentification qui peut être utilisé pour l’authentification et l’autorisation. Le jeton est stocké en tant que permet un cookie qui accompagne chaque demande du client. Génération et la validation de ce cookie sont effectuée par le Middleware Cookie d’authentification. Le [intergiciel (middleware)](xref:fundamentals/middleware/index) sérialise un principal d’utilisateur dans un cookie chiffré. Sur les demandes suivantes, le middleware valide le cookie recrée le principal et attribue au principal de la [utilisateur](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) propriété du [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).

### <a name="token-based-authentication"></a>Authentification par jeton

Lorsqu’un utilisateur est authentifié, ils sont émis un jeton (pas un jeton anti-contrefaçon). Le jeton contient les informations de l’utilisateur sous la forme de [revendications](/dotnet/framework/security/claims-based-identity-model) ou un jeton de référence qui pointe l’application à l’état utilisateur mis à jour dans l’application. Lorsqu’un utilisateur tente d’accéder à une ressource nécessitant une authentification, le jeton est envoyé à l’application avec un en-tête d’autorisation supplémentaires sous forme de jeton du porteur. Cela rend l’application sans état. Dans chaque demande ultérieure, le jeton est passé dans la demande pour la validation côté serveur. Ce jeton n’est pas *chiffrées*; il a *encodé*. Sur le serveur, le jeton est décodé pour accéder à ses informations. Pour envoyer le jeton sur les demandes suivantes, stocker le jeton dans le stockage local du navigateur. Ne soyez pas inquiet à une vulnérabilité CSRF si le jeton est stocké dans le stockage local du navigateur. CSRF est un problème lorsque le jeton est stocké dans un cookie.

### <a name="multiple-apps-hosted-at-one-domain"></a>Plusieurs applications hébergées sur un domaine

Environnements d’hébergement partagés sont vulnérables au piratage de session connexion CSRF et autres attaques.

Bien que `example1.contoso.net` et `example2.contoso.net` sont des hôtes différents, il existe une relation de confiance implicite entre les hôtes sous le `*.contoso.net` domaine. Cette relation de confiance implicite permet des hôtes potentiellement non fiables affecter des cookies entre eux (les stratégies de même origine qui régissent les demandes AJAX ne s’appliquent obligatoirement aux cookies HTTP).

Pour empêcher les attaques qui exploitent les cookies de confiance entre les applications hébergées sur le même domaine, ne pas partager des domaines. Lorsque chaque application est hébergée sur son propre domaine, il n’existe aucune relation d’approbation implicite de cookie à exploiter.

## <a name="aspnet-core-antiforgery-configuration"></a>Configuration anti-contrefaçon ASP.NET Core

> [!WARNING]
> ASP.NET Core implémente à l’aide d’anti-contrefaçon [Protection des données ASP.NET Core](xref:security/data-protection/introduction). La pile de protection des données doit être configurée pour fonctionner dans une batterie de serveurs. Consultez [configuration de la protection des données](xref:security/data-protection/configuration/overview) pour plus d’informations.

Dans ASP.NET Core 2.0 ou version ultérieure, le [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injecte des jetons anti-contrefaçon dans les éléments de formulaire HTML. Le balisage suivant dans un fichier Razor génère automatiquement des jetons anti-contrefaçon :

```cshtml
<form method="post">
    ...
</form>
```

Même manière, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) génère des jetons anti-contrefaçon par défaut si la méthode du formulaire n’est pas GET.

La génération automatique des jetons anti-contrefaçon pour les éléments de formulaire HTML se produit lorsque le `<form>` balise contient le `method="post"` attribut et une des opérations suivantes sont remplie :

  * L’attribut action est vide (`action=""`).
  * L’attribut d’action n’est pas fourni (`<form method="post">`).

Vous pouvez désactiver la génération automatique des jetons anti-contrefaçon pour les éléments de formulaire HTML :

* Désactiver explicitement des jetons anti-contrefaçon avec le `asp-antiforgery` attribut :

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* L’élément du formulaire est choisi par de Tag Helpers à l’aide du programme d’assistance de balise [! annulations symbole](xref:mvc/views/tag-helpers/intro#opt-out):

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
> [Pages Razor](xref:razor-pages/index) sont protégés automatiquement contre XSRF/CSRF. Pour plus d’informations, consultez [XSRF/CSRF et Pages Razor](xref:razor-pages/index#xsrf).

L’approche la plus courante pour se défendre contre les attaques CSRF consiste à utiliser le *modèle de jeton synchronisateur* (STP). STP est utilisé lorsque l’utilisateur demande une page avec les données de formulaire :

1. Le serveur envoie un jeton associé avec l’identité de l’utilisateur actuel au client.
1. Le client envoie le jeton au serveur pour la vérification.
1. Si le serveur reçoit un jeton qui ne correspond pas à identité de l’utilisateur authentifié, la demande est rejetée.

Le jeton est unique et imprévisible. Le jeton peut également être utilisé pour garantir un séquençage correct d’une série de demandes (par exemple, en garantissant la séquence de demande de : la page 1 &ndash; page 2 &ndash; page 3). Tous les formulaires dans les modèles ASP.NET Core MVC et les Pages Razor génèrent des jetons anti-contrefaçon. Les deux exemples de vue suivants génèrent des jetons anti-contrefaçon :

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

Ajouter explicitement un jeton anti-contrefaçon pour un `<form>` élément sans recourir à des Tag Helpers avec le programme d’assistance HTML [ @Html.AntiForgeryToken ](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

Dans chacun des cas précédents, ASP.NET Core ajoute un champ de formulaire masqué similaire à ce qui suit :

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

ASP.NET Core inclut trois [filtres](xref:mvc/controllers/filters) pour travailler avec des jetons anti-contrefaçon :

* [ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a>Options anti-contrefaçon

Personnaliser [options anti-contrefaçon](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) dans `Startup.ConfigureServices`:

::: moniker range=">= aspnetcore-2.0"

```csharp
services.AddAntiforgery(options => 
{
    // Set Cookie properties using CookieBuilder properties†.
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.SuppressXFrameOptionsHeader = false;
});
```

&dagger;Définir l’anti-contrefaçon `Cookie` propriétés à l’aide des propriétés de la [CookieBuilder](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder) classe.

| Option | Description |
| ------ | ----------- |
| [Cookie](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | Détermine les paramètres utilisés pour créer les cookies anti-contrefaçon. |
| [FormFieldName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | Le nom du champ de formulaire masqué utilisé par le système anti-contrefaçon pour restituer des jetons anti-contrefaçon dans les vues. |
| [HeaderName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | Le nom de l’en-tête utilisé par le système anti-contrefaçon. Si `null`, le système considère uniquement les données de formulaire. |
| [SuppressXFrameOptionsHeader](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | Spécifie s’il faut supprimer la génération de la `X-Frame-Options` en-tête. Par défaut, l’en-tête est généré avec la valeur « SAMEORIGIN ». La valeur par défaut est `false`. |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

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
| [Cookie](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | Détermine les paramètres utilisés pour créer les cookies anti-contrefaçon. |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | Le domaine du cookie. La valeur par défaut est `null`. Cette propriété est obsolète et sera supprimée dans une future version. L’alternative recommandée est Cookie.Domain. |
| [CookieName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | Le nom du cookie. Si ne pas définie, le système génère un nom unique commençant par le [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) ( ». AspNetCore.Antiforgery. »). Cette propriété est obsolète et sera supprimée dans une future version. L’alternative recommandée est Cookie.Name. |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | Le chemin d’accès défini sur le cookie. Cette propriété est obsolète et sera supprimée dans une future version. L’alternative recommandée est Cookie.Path. |
| [FormFieldName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | Le nom du champ de formulaire masqué utilisé par le système anti-contrefaçon pour restituer des jetons anti-contrefaçon dans les vues. |
| [HeaderName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | Le nom de l’en-tête utilisé par le système anti-contrefaçon. Si `null`, le système considère uniquement les données de formulaire. |
| [RequireSsl](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | Spécifie si HTTPS est requis par le système anti-contrefaçon. Si `true`, les requêtes non-HTTPS échouent. La valeur par défaut est `false`. Cette propriété est obsolète et sera supprimée dans une future version. L’alternative recommandée consiste à définir les Cookie.SecurePolicy. |
| [SuppressXFrameOptionsHeader](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | Spécifie s’il faut supprimer la génération de la `X-Frame-Options` en-tête. Par défaut, l’en-tête est généré avec la valeur « SAMEORIGIN ». La valeur par défaut est `false`. |

::: moniker-end

Pour plus d’informations, consultez [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).

## <a name="configure-antiforgery-features-with-iantiforgery"></a>Configurer les fonctionnalités anti-contrefaçon avec IAntiforgery

[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) fournit l’API pour configurer les fonctionnalités anti-contrefaçon. `IAntiforgery` peut être demandé dans le `Configure` méthode de la `Startup` classe. L’exemple suivant utilise l’intergiciel (middleware) à partir de la page d’accueil de l’application pour générer un jeton anti-contrefaçon et l’envoyer dans la réponse en tant que cookie (à l’aide de la convention de nommage Angular par défaut est décrite plus loin dans cette rubrique) :

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

### <a name="require-antiforgery-validation"></a>Exiger une validation anti-contrefaçon

[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) est un filtre d’action qui peut être appliqué à une action individuelle, un contrôleur, ou dans le monde entier. Les demandes effectuées à des actions qui ont ce filtre est appliqué sont bloquées, sauf si la demande inclut un jeton anti-contrefaçon valide.

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

Le `ValidateAntiForgeryToken` attribut requiert un jeton pour les demandes aux méthodes d’action il décore, y compris les requêtes HTTP GET. Si le `ValidateAntiForgeryToken` attribut est appliqué sur les contrôleurs de l’application, il peut être remplacé par le `IgnoreAntiforgeryToken` attribut.

> [!NOTE]
> ASP.NET Core ne prend en charge l’ajout automatique des jetons anti-contrefaçon aux demandes GET.

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a>Valider automatiquement les jetons anti-contrefaçon pour les méthodes HTTP non sécurisés

Les applications ASP.NET Core ne pas générer des jetons anti-contrefaçon pour les méthodes HTTP sécurisés (GET, HEAD, OPTIONS et TRACE). Au lieu d’appliquer largement le `ValidateAntiForgeryToken` attribut, puis en remplaçant avec `IgnoreAntiforgeryToken` attributs, la [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) attribut peut être utilisé. Cet attribut fonctionne de manière identique à la `ValidateAntiForgeryToken` d’attribut, à ceci près qu’il ne nécessite des jetons pour les demandes effectuées à l’aide de méthodes HTTP suivantes :

* GET
* HEAD
* OPTIONS
* TRACE

Nous recommandons l’utilisation de `AutoValidateAntiforgeryToken` largement pour les scénarios non-API. Cela garantit que les actions POST sont protégées par défaut. L’alternative consiste à ignorer les jetons anti-contrefaçon par défaut, sauf si `ValidateAntiForgeryToken` est appliqué aux méthodes d’action individuelles. Il n’est plus probable que dans ce scénario pour une méthode d’action POST rester pas protégé par erreur, en laissant l’application vulnérable aux attaques par falsification de requête Intersites. Toutes les publications doivent envoyer le jeton anti-contrefaçon.

API n’ont pas un mécanisme automatique pour l’envoi de la partie non cookie du jeton. L’implémentation dépend probablement de l’implémentation du code client. Certains exemples sont présentés ci-dessous :

Exemple de niveau classe :

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

### <a name="override-global-or-controller-antiforgery-attributes"></a>Remplacement global ou attributs anti-contrefaçon contrôleur

Le [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filtre est utilisé pour éliminer le besoin d’un jeton anti-contrefaçon pour une action donnée (ou un contrôleur). Lorsqu’il est appliqué, ce filtre remplace `ValidateAntiForgeryToken` et `AutoValidateAntiforgeryToken` filtres spécifiés à un niveau supérieur (globalement ou sur un contrôleur).

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

## <a name="refresh-tokens-after-authentication"></a>Une fois l’authentification des jetons d’actualisation

Les jetons doivent être actualisées une fois que l’utilisateur est authentifié en redirigeant l’utilisateur à une vue ou une page Pages Razor.

## <a name="javascript-ajax-and-spas"></a>JavaScript, AJAX et les applications SPA

Dans les applications HTML traditionnelles, les jetons anti-contrefaçon sont passés au serveur à l’aide des champs de formulaire masqué. Dans les applications modernes basées sur JavaScript et les applications SPA, nombre de demandes envoyées par programmation. Ces demandes AJAX peuvent utiliser d’autres techniques (telles que les en-têtes de requête ou des cookies) pour envoyer le jeton.

Si les cookies sont utilisés pour stocker les jetons d’authentification et pour authentifier les demandes d’API sur le serveur, CSRF est un problème potentiel. Si le stockage local est utilisé pour stocker le jeton, une vulnérabilité CSRF peut être atténuée, car les valeurs à partir du stockage local ne sont pas automatiquement envoyés au serveur avec chaque demande. Par conséquent, l’utilisation du stockage local pour stocker le jeton anti-contrefaçon sur le client et envoie le jeton, comme un en-tête de demande est une approche recommandée.

### <a name="javascript"></a>JavaScript

À l’aide de JavaScript avec des vues, le jeton peut être créé à l’aide d’un service à partir de la vue. Injecter le [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) service dans la vue et appelez [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

Cette approche évite de devoir traiter directement avec la définition des cookies à partir du serveur ou de les lire à partir du client.

L’exemple précédent utilise JavaScript pour lire la valeur de champ masqué pour l’en-tête AJAX POST.

JavaScript peut également jetons d’accès dans des cookies et utiliser le contenu du cookie pour créer un en-tête avec la valeur du jeton.

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

En supposant que le script demande à envoyer le jeton dans un en-tête appelé `X-CSRF-TOKEN`, configurer le service anti-contrefaçon pour rechercher le `X-CSRF-TOKEN` en-tête :

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

AngularJS utilise une convention à l’adresse CSRF. Si le serveur envoie un cookie portant le nom `XSRF-TOKEN`, le AngularJS `$http` service ajoute la valeur du cookie à un en-tête lorsqu’il envoie une demande au serveur. Ce processus est automatique. L’en-tête n’a pas besoin de définir explicitement dans le client. Le nom d’en-tête est `X-XSRF-TOKEN`. Le serveur doit détecter cet en-tête et valider son contenu.

Pour les API ASP.NET Core travailler avec cette convention dans le démarrage de votre application :

* Configurer votre application pour fournir un jeton dans un cookie appelé `XSRF-TOKEN`.
* Configurer le service anti-contrefaçon pour rechercher un en-tête nommé `X-XSRF-TOKEN`.

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

public void ConfigureServices(IServiceCollection services)
{
    // Angular's default header name for sending the XSRF token.
    services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
}
```

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="extend-antiforgery"></a>Étendre anti-contrefaçon

Le [IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) type permet aux développeurs d’étendre le comportement du système anti-CSRF par les données supplémentaires de l’aller-retour dans chaque jeton. Le [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) méthode est appelée chaque fois qu’un jeton de champ est généré, et la valeur de retour est incorporée dans le jeton généré. Un implémenteur peut retourner un horodatage, une valeur à usage unique ou toute autre valeur, puis appelez [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) pour valider ces données lorsque le jeton est validé. Nom d’utilisateur du client est déjà incorporée dans les jetons générés, il est donc inutile d’inclure ces informations. Si un jeton contient des données supplémentaires, mais aucune `IAntiForgeryAdditionalDataProvider` est configuré, les données supplémentaires n’est pas validées.

## <a name="additional-resources"></a>Ressources supplémentaires

* [CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) sur [ouvrir Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).
* <xref:host-and-deploy/web-farm>
