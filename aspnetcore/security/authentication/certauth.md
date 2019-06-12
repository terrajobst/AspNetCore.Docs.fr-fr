---
title: Configurer l’authentification par certificat dans ASP.NET Core
author: blowdart
description: Découvrez comment configurer l’authentification par certificat dans ASP.NET Core pour IIS et HTTP.sys.
monikerRange: '>= aspnetcore-3.0'
ms.author: barry.dorrans
ms.date: 06/11/2019
uid: security/authentication/certauth
ms.openlocfilehash: 37567128531187bfe7dd6c9f5aa990226e70f35f
ms.sourcegitcommit: 1bb3f3f1905b4e7d4ca1b314f2ce6ee5dd8be75f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/11/2019
ms.locfileid: "66837536"
---
# <a name="overview"></a>Vue d'ensemble

`Microsoft.AspNetCore.Authentication.Certificate` contient une implémentation semblable à [l’authentification par certificat](https://tools.ietf.org/html/rfc5246#section-7.4.4) pour ASP.NET Core. L’authentification par certificat se produit au niveau TLS, long avant qu’il jamais ASP.NET Core. Il s’agit plus précisément, un gestionnaire d’authentification qui valide le certificat et vous donne ensuite un événement où vous pouvez résoudre ce certificat pour un `ClaimsPrincipal`. 

[Configurer votre hôte](#configure-your-host-to-require-certificates) l’authentification par certificat, qu’il s’agisse IIS, Kestrel, Azure Web Apps, ou les autres éléments que vous utilisez.

## <a name="get-started"></a>Prise en main

Acquérir un certificat HTTPS, appliquez-le, et [configurer votre hôte](#configure-your-host-to-require-certificates) pour exiger les certificats.

Dans votre application web, ajoutez une référence à la `Microsoft.AspNetCore.Authentication.Certificate` package. Ensuite, dans le `Startup.Configure` méthode, appelez `app.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).UseCertificateAuthentication(...);` de vos options, fournissant un délégué pour `OnCertificateValidated` pour effectuer une validation supplémentaire sur le certificat client envoyé avec les demandes. Activer ces informations dans un `ClaimsPrincipal` et définissez-le sur le `context.Principal` propriété.

Si l’authentification échoue, ce gestionnaire renvoie un `403 (Forbidden)` réponse plutôt un `401 (Unauthorized)`, comme vous pouvez l’imaginer. Le raisonnement est que l’authentification doit se produire pendant la connexion TLS initiale. Au moment où qu'il atteint le gestionnaire, il est trop tard. Il n’existe aucun moyen pour mettre à niveau de la connexion à partir d’une connexion anonyme à une avec un certificat.

Ajoutez également `app.UseAuthentication();` dans le `Startup.Configure` (méthode). Sinon, le HttpContext.User ne sera pas définie `ClaimsPrincipal` créé à partir du certificat. Exemple :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(
        CertificateAuthenticationDefaults.AuthenticationScheme)
            .AddCertificate();
    // All the other service configuration.
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseAuthentication();

    // All the other app configuration.
}
```

L’exemple précédent montre la façon par défaut pour ajouter l’authentification par certificat. Le gestionnaire construit un principal d’utilisateur à l’aide des propriétés du certificat commun.

## <a name="configure-certificate-validation"></a>Configurer la validation du certificat

Le `CertificateAuthenticationOptions` gestionnaire a certaines validations intégrées qui sont les validations minimale, vous devez effectuer sur un certificat. Chacun de ces paramètres est activée par défaut.

### <a name="allowedcertificatetypes--chained-selfsigned-or-all-chained--selfsigned"></a>AllowedCertificateTypes = chaînées, SelfSigned, ou l’ensemble (chaînées | SelfSigned)

Cette vérification valide le fait que seul le type de certificat approprié est autorisé.

### <a name="validatecertificateuse"></a>ValidateCertificateUse

Cette vérification valide le fait que le certificat présenté par le client a l’authentification du Client étendus d’utilisation de la clé (EKU), ou aucun EKU du tout. Comme les spécifications par exemple, si aucun EKU n’est spécifié, toutes les utilisations sont considérés comme valides.

### <a name="validatevalidityperiod"></a>ValidateValidityPeriod

Cette vérification valide le fait que le certificat est dans sa période de validité. À chaque demande, le gestionnaire s’assure qu’un certificat qui a été valid lorsqu’il a été présenté n’a pas expiré pendant sa session en cours.

### <a name="revocationflag"></a>RevocationFlag

Un indicateur qui spécifie les certificats de la chaîne sont vérifiés pour révocation.

Vérifications de révocation sont exécutées uniquement lorsque le certificat est chaîné à un certificat racine.

### <a name="revocationmode"></a>RevocationMode

Un indicateur qui spécifie la façon dont sont effectuées les vérifications de révocation.

Spécification d’un contrôle en ligne peut entraîner un long délai pendant que l’autorité de certification est contactée.

Vérifications de révocation sont exécutées uniquement lorsque le certificat est chaîné à un certificat racine.

### <a name="can-i-configure-my-app-to-require-a-certificate-only-on-certain-paths"></a>Puis-je configurer mon application pour demander un certificat uniquement sur certains chemins d’accès ?

Cela n’est pas possible. N’oubliez pas de que l’échange de certificat s’effectue que le début de la conversation HTTPS, il est exécuté par le serveur avant la première demande est reçue sur cette connexion, il est donc pas possible de portée basé sur les champs de la demande.

## <a name="handler-events"></a>Gestionnaire d’événements

Le gestionnaire a deux événements :

* `OnAuthenticationFailed` &ndash; Appelé si une exception se produit lors de l’authentification et vous permet de réagir.
* `OnCertificateValidated` &ndash; Appelée une fois que le certificat a été validé, ont réussi la validation et un principal par défaut a été créé. Cet événement permet d’effectuer votre propre validation et d’augmenter ou de remplacer l’objet principal. Pour obtenir des exemples sont les suivantes :
  * Déterminer si le certificat est connu à vos services.
  * Construction de votre propre objet principal. Prenons l’exemple suivant dans `Startup.ConfigureServices`:

```csharp
services.AddAuthentication(
    CertificateAuthenticationDefaults.AuthenticationScheme)
    .AddCertificate(options =>
    {
        options.Events = new CertificateAuthenticationEvents
        {
            OnCertificateValidated = context =>
            {
                var claims = new[]
                {
                    new Claim(
                        ClaimTypes.NameIdentifier, 
                        context.ClientCertificate.Subject,
                        ClaimValueTypes.String, 
                        context.Options.ClaimsIssuer),
                    new Claim(ClaimTypes.Name,
                        context.ClientCertificate.Subject,
                        ClaimValueTypes.String, 
                        context.Options.ClaimsIssuer)
                };

                context.Principal = new ClaimsPrincipal(
                    new ClaimsIdentity(claims, context.Scheme.Name));
                context.Success();

                return Task.CompletedTask;
            }
        };
    });
```

Si vous trouvez le certificat entrant ne répond pas à votre validation supplémentaire, appelez `context.Fail("failure reason")` avec un motif de l’échec.

Fonctionnalités de réel, vous voudrez probablement appeler un service inscrit dans l’injection de dépendances qui se connecte à une base de données ou un autre type de magasin de l’utilisateur. Accéder à votre service en utilisant le contexte passé dans votre délégué. Prenons l’exemple suivant dans `Startup.ConfigureServices`:

```csharp
services.AddAuthentication(
    CertificateAuthenticationDefaults.AuthenticationScheme)
    .AddCertificate(options =>
    {
        options.Events = new CertificateAuthenticationEvents
        {
            OnCertificateValidated = context =>
            {
                var validationService =
                    context.HttpContext.RequestServices
                        .GetService<ICertificateValidationService>();
                
                if (validationService.ValidateCertificate(
                    context.ClientCertificate))
                {
                    var claims = new[]
                    {
                        new Claim(
                            ClaimTypes.NameIdentifier, 
                            context.ClientCertificate.Subject, 
                            ClaimValueTypes.String, 
                            context.Options.ClaimsIssuer),
                        new Claim(
                            ClaimTypes.Name, 
                            context.ClientCertificate.Subject, 
                            ClaimValueTypes.String, 
                            context.Options.ClaimsIssuer)
                    };

                    context.Principal = new ClaimsPrincipal(
                        new ClaimsIdentity(claims, context.Scheme.Name));
                    context.Success();
                }                     

                return Task.CompletedTask;
            }
        };
    });
```

Conceptuellement, la validation du certificat est un problème d’autorisation. Ajout d’une vérification, par exemple, un émetteur ou une empreinte numérique dans une stratégie d’autorisation, plutôt qu’à l’intérieur `OnCertificateValidated`, est parfaitement acceptable.

## <a name="configure-your-host-to-require-certificates"></a>Configurer votre hôte pour exiger des certificats

### <a name="kestrel"></a>Kestrel

Dans *Program.cs*, configurez Kestrel comme suit :

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel(options =>
        {
            options.ConfigureHttpsDefaults(opt => 
                opt.ClientCertificateMode = 
                    ClientCertificateMode.RequireCertificate);
        })
        .Build();
```

### <a name="iis"></a>IIS

Procédez comme suit dans le Gestionnaire des services Internet :

1. Sélectionnez votre site à partir de la **connexions** onglet.
1. Double-cliquez sur le **paramètres SSL** option dans le **affichage des fonctionnalités** fenêtre.
1. Vérifier le **exiger SSL** case à cocher, puis sélectionnez le **nécessitent** case d’option dans le **certificats clients** section.

![Paramètres du certificat client dans IIS](README-IISConfig.png)

### <a name="azure-and-custom-web-proxies"></a>Azure et les proxys web personnalisé

Consultez le [héberger et déployer la documentation](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) pour savoir comment configurer le transfert d’intergiciel (middleware) de certificat.
