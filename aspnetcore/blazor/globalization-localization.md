---
title: ASP.NET Core Blazor la globalisation et la localisation
author: guardrex
description: Découvrez comment rendre des composants Razor accessibles aux utilisateurs dans plusieurs cultures et langages.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/globalization-localization
ms.openlocfilehash: aba62fa7b6285c8ba884652694f1ea3e3a66ed18
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77453267"
---
# <a name="aspnet-core-opno-locblazor-globalization-and-localization"></a>ASP.NET Core Blazor la globalisation et la localisation

Par [Luke Latham](https://github.com/guardrex) et [Daniel Roth](https://github.com/danroth27)

Les composants Razor peuvent être rendus accessibles aux utilisateurs dans plusieurs cultures et langages. Les scénarios de globalisation et de localisation .NET suivants sont disponibles :

* . Système de ressources du réseau
* Mise en forme des nombres et des dates spécifiques à la culture

Un ensemble limité de scénarios de localisation de ASP.NET Core est actuellement pris en charge :

* `IStringLocalizer<>` *est pris en charge* dans les applications Blazor.
* la localisation des annotations de données `IHtmlLocalizer<>`, `IViewLocalizer<>`et est ASP.NET Core les scénarios MVC et **non pris en charge** dans les applications Blazor.

Pour plus d’informations, consultez <xref:fundamentals/localization>.

## <a name="globalization"></a>Globalisation

les fonctionnalités de `@bind` de Blazoreffectuent des mises en forme et analysent les valeurs pour l’affichage en fonction de la culture actuelle de l’utilisateur.

La culture actuelle est accessible à partir de la propriété <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName>.

[CultureInfo. InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) est utilisé pour les types de champ suivants (`<input type="{TYPE}" />`) :

* `date`
* `number`

Les types de champ précédents :

* Sont affichés à l’aide des règles de mise en forme appropriées basées sur le navigateur.
* Ne peut pas contenir de texte de forme libre.
* Fournir des caractéristiques d’interaction de l’utilisateur en fonction de l’implémentation du navigateur.

Les types de champs suivants ont des exigences de mise en forme spécifiques et ne sont pas actuellement pris en charge par Blazor, car ils ne sont pas pris en charge par tous les principaux navigateurs :

* `datetime-local`
* `month`
* `week`

`@bind` prend en charge le paramètre `@bind:culture` pour fournir une <xref:System.Globalization.CultureInfo?displayProperty=fullName> pour l’analyse et la mise en forme d’une valeur. La spécification d’une culture n’est pas recommandée lors de l’utilisation des types de champ `date` et `number`. `date` et `number` disposent d’une prise en charge intégrée Blazor qui fournit la culture requise.

## <a name="localization"></a>Localisation

les applications Blazor Server sont localisées à l’aide de l' [intergiciel (middleware](xref:fundamentals/localization#localization-middleware)) de localisation. L’intergiciel sélectionne la culture appropriée pour les utilisateurs qui demandent des ressources à partir de l’application.

La culture peut être définie à l’aide de l’une des approches suivantes :

* [Cookies](#cookies)
* [Fournir l’interface utilisateur pour choisir la culture](#provide-ui-to-choose-the-culture)

Pour plus d’informations et d’exemples, consultez <xref:fundamentals/localization>.

### <a name="configure-the-linker-for-internationalization-opno-locblazor-webassembly"></a>Configurer l’éditeur de liens pour l’internationalisation (Blazor webassembly)

Par défaut, la configuration de l’éditeur de liens de Blazorpour les applications webassembly Blazor supprime les informations d’internationalisation, à l’exception des paramètres régionaux demandés explicitement. Pour plus d’informations et de conseils sur le contrôle du comportement de l’éditeur de liens, consultez <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.

### <a name="cookies"></a>Cookies

Un cookie de culture de localisation peut conserver la culture de l’utilisateur. Le cookie est créé par la méthode `OnGet` de la page hôte de l’application (*pages/Host. cshtml. cs*). L’intergiciel (middleware) de localisation lit le cookie sur les demandes suivantes pour définir la culture de l’utilisateur. 

L’utilisation d’un cookie garantit que la connexion WebSocket peut propager correctement la culture. Si les schémas de localisation sont basés sur le chemin d’URL ou la chaîne de requête, il est possible que le schéma ne soit pas en mesure de fonctionner avec les WebSockets, et donc de ne pas conserver la culture. Par conséquent, l’utilisation d’un cookie de culture de localisation est l’approche recommandée.

Toute technique peut être utilisée pour assigner une culture si la culture est rendue persistante dans un cookie de localisation. Si l’application a déjà un schéma de localisation établi pour les ASP.NET Core côté serveur, continuez à utiliser l’infrastructure de localisation existante de l’application et à définir le cookie de la culture de localisation dans le schéma de l’application.

L’exemple suivant montre comment définir la culture actuelle dans un cookie qui peut être lu par l’intergiciel (middleware) de localisation. Créez un fichier *pages/Host. cshtml. cs* avec le contenu suivant dans l’application Blazor Server :

```csharp
public class HostModel : PageModel
{
    public void OnGet()
    {
        HttpContext.Response.Cookies.Append(
            CookieRequestCultureProvider.DefaultCookieName,
            CookieRequestCultureProvider.MakeCookieValue(
                new RequestCulture(
                    CultureInfo.CurrentCulture,
                    CultureInfo.CurrentUICulture)));
    }
}
```

La localisation est gérée par l’application dans la séquence d’événements suivante :

1. Le navigateur envoie une requête HTTP initiale à l’application.
1. La culture est affectée par l’intergiciel (middleware) de localisation.
1. La méthode `OnGet` dans *_Host. cshtml. cs* rend persistante la culture dans un cookie dans le cadre de la réponse.
1. Le navigateur ouvre une connexion WebSocket pour créer une session Blazor Server interactive.
1. L’intergiciel de localisation lit le cookie et assigne la culture.
1. La session du serveur de Blazor commence par la culture correcte.

### <a name="provide-ui-to-choose-the-culture"></a>Fournir l’interface utilisateur pour choisir la culture

Pour fournir une interface utilisateur permettant à un utilisateur de sélectionner une culture, il est recommandé d’effectuer une *approche basée sur la redirection* . Le processus est similaire à ce qui se produit dans une application Web lorsqu’un utilisateur tente d’accéder à une ressource sécurisée&mdash;l’utilisateur est redirigé vers une page de connexion, puis redirigé vers la ressource d’origine. 

L’application conserve la culture sélectionnée de l’utilisateur via une redirection vers un contrôleur. Le contrôleur définit la culture sélectionnée de l’utilisateur dans un cookie et redirige l’utilisateur vers l’URI d’origine.

Établissez un point de terminaison HTTP sur le serveur pour définir la culture sélectionnée de l’utilisateur dans un cookie et effectuer la redirection vers l’URI d’origine :

```csharp
[Route("[controller]/[action]")]
public class CultureController : Controller
{
    public IActionResult SetCulture(string culture, string redirectUri)
    {
        if (culture != null)
        {
            HttpContext.Response.Cookies.Append(
                CookieRequestCultureProvider.DefaultCookieName,
                CookieRequestCultureProvider.MakeCookieValue(
                    new RequestCulture(culture)));
        }

        return LocalRedirect(redirectUri);
    }
}
```

> [!WARNING]
> Utilisez le résultat de l’action `LocalRedirect` pour empêcher les attaques de redirection ouvertes. Pour plus d’informations, consultez <xref:security/preventing-open-redirects>.

Le composant suivant montre un exemple d’exécution de la redirection initiale lorsque l’utilisateur sélectionne une culture :

```razor
@inject NavigationManager NavigationManager

<h3>Select your language</h3>

<select @onchange="OnSelected">
    <option>Select...</option>
    <option value="en-US">English</option>
    <option value="fr-FR">Français</option>
</select>

@code {
    private void OnSelected(ChangeEventArgs e)
    {
        var culture = (string)e.Value;
        var uri = new Uri(NavigationManager.Uri())
            .GetComponents(UriComponents.PathAndQuery, UriFormat.Unescaped);
        var query = $"?culture={Uri.EscapeDataString(culture)}&" +
            $"redirectUri={Uri.EscapeDataString(uri)}";

        NavigationManager.NavigateTo("/Culture/SetCulture" + query, forceLoad: true);
    }
}
```

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:fundamentals/localization>
