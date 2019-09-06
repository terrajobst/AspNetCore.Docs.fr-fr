---
title: Configurer l’authentification par certificat dans ASP.NET Core
author: blowdart
description: Découvrez comment configurer l’authentification par certificat dans ASP.NET Core pour IIS et HTTP. sys.
monikerRange: '>= aspnetcore-3.0'
ms.author: bdorrans
ms.date: 08/19/2019
uid: security/authentication/certauth
ms.openlocfilehash: ce7bcdbfb8ce0f1febf34b49786e92c917be139c
ms.sourcegitcommit: 116bfaeab72122fa7d586cdb2e5b8f456a2dc92a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70384852"
---
# <a name="overview"></a>Présentation

`Microsoft.AspNetCore.Authentication.Certificate`contient une implémentation similaire à [l’authentification par certificat](https://tools.ietf.org/html/rfc5246#section-7.4.4) pour ASP.net core. L’authentification par certificat se produit au niveau du TLS, à long terme avant qu’il ne soit ASP.NET Core. Plus précisément, il s’agit d’un gestionnaire d’authentification qui valide le certificat et vous donne un événement dans lequel vous pouvez résoudre ce certificat en `ClaimsPrincipal`. 

[Configurez votre hôte](#configure-your-host-to-require-certificates) pour l’authentification par certificat, qu’il soit IIS, Kestrel, Azure Web Apps ou tout autre que vous utilisez.

## <a name="proxy-and-load-balancer-scenarios"></a>Scénarios de proxy et d’équilibrage de charge

L’authentification par certificat est un scénario avec état principalement utilisé dans lequel un proxy ou un équilibreur de charge ne gère pas le trafic entre les clients et les serveurs. Si un proxy ou un équilibreur de charge est utilisé, l’authentification par certificat fonctionne uniquement si le proxy ou l’équilibreur de charge :

* Gère l’authentification.
* Transmet les informations d’authentification de l’utilisateur à l’application (par exemple, dans un en-tête de demande), qui agit sur les informations d’authentification.

Une alternative à l’authentification par certificat dans les environnements où les proxies et les équilibreurs de charge sont utilisés est Active Directory Services fédérés (ADFS) avec OpenID Connect (OIDC).

## <a name="get-started"></a>Prise en main

Obtenez un certificat HTTPs, appliquez-le et [configurez votre hôte](#configure-your-host-to-require-certificates) pour exiger des certificats.

Dans votre application Web, ajoutez une référence au `Microsoft.AspNetCore.Authentication.Certificate` package. Ensuite, dans `Startup.Configure` la méthode, `app.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).UseCertificateAuthentication(...);` appelez avec vos options `OnCertificateValidated` , en fournissant un délégué à pour effectuer une validation supplémentaire sur le certificat client envoyé avec les demandes. Transformez ces informations `ClaimsPrincipal` en un et définissez- `context.Principal` les sur la propriété.

Si l’authentification échoue, ce gestionnaire renvoie `403 (Forbidden)` une réponse plutôt `401 (Unauthorized)`qu’un, comme vous pouvez l’imaginer. Le raisonnement est que l’authentification doit se produire pendant la connexion TLS initiale. Au moment où il atteint le gestionnaire, il est trop tard. Il n’existe aucun moyen de mettre à niveau la connexion d’une connexion anonyme à une avec un certificat.

Ajoutez `app.UseAuthentication();` également dans la `Startup.Configure` méthode. Sinon, HttpContext. User ne sera pas défini à `ClaimsPrincipal` créé à partir du certificat. Par exemple :

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

L’exemple précédent illustre la méthode par défaut pour ajouter l’authentification par certificat. Le gestionnaire construit un principal d’utilisateur à l’aide des propriétés de certificat courantes.

## <a name="configure-certificate-validation"></a>Configurer la validation des certificats

Le `CertificateAuthenticationOptions` gestionnaire a des validations intégrées qui sont les validations minimales que vous devez effectuer sur un certificat. Chacun de ces paramètres est activé par défaut.

### <a name="allowedcertificatetypes--chained-selfsigned-or-all-chained--selfsigned"></a>AllowedCertificateTypes = chained, SelfSigned ou All (chaîné | SelfSigned)

Cette vérification valide que seul le type de certificat approprié est autorisé.

### <a name="validatecertificateuse"></a>ValidateCertificateUse

Cette vérification permet de vérifier que le certificat présenté par le client a l’utilisation améliorée de la clé d’authentification du client, ou aucune EKU. Comme les spécifications indiquent, si aucune EKU n’est spécifiée, toutes les utilisations améliorées de la EKU sont considérées comme valides.

### <a name="validatevalidityperiod"></a>ValidateValidityPeriod

Ce contrôle vérifie que le certificat se trouve dans sa période de validité. À chaque demande, le gestionnaire s’assure qu’un certificat valide lorsqu’il a été présenté n’a pas expiré pendant sa session active.

### <a name="revocationflag"></a>RevocationFlag

Indicateur qui spécifie les certificats de la chaîne qui sont vérifiés pour la révocation.

Les vérifications de révocation sont effectuées uniquement lorsque le certificat est chaîné à un certificat racine.

### <a name="revocationmode"></a>RevocationMode

Indicateur qui spécifie comment les vérifications de révocation sont effectuées.

La spécification d’un contrôle en ligne peut entraîner un délai de longue durée pendant que l’autorité de certification est contactée.

Les vérifications de révocation sont effectuées uniquement lorsque le certificat est chaîné à un certificat racine.

### <a name="can-i-configure-my-app-to-require-a-certificate-only-on-certain-paths"></a>Puis-je configurer mon application pour exiger un certificat uniquement sur certains chemins d’accès ?

Cela n’est pas possible. Rappelez-vous que l’échange de certificats est effectué au début de la conversation HTTPs, car il est effectué par le serveur avant la réception de la première requête sur cette connexion, ce qui signifie qu’il n’est pas possible d’effectuer une étendue basée sur des champs de demande.

## <a name="handler-events"></a>Événements du gestionnaire

Le gestionnaire a deux événements :

* `OnAuthenticationFailed`&ndash; Appelé si une exception se produit pendant l’authentification et vous permet de réagir.
* `OnCertificateValidated`&ndash; Appelée une fois que le certificat a été validé, la validation a réussi et un principal par défaut a été créé. Cet événement vous permet d’effectuer votre propre validation et d’augmenter ou de remplacer le principal. Voici quelques exemples :
  * Déterminer si le certificat est connu de vos services.
  * Construction de votre propre principal. Prenons l’exemple suivant dans `Startup.ConfigureServices` :

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

Si vous trouvez que le certificat entrant ne répond pas à votre validation supplémentaire `context.Fail("failure reason")` , appelez en raison d’une erreur.

Pour les fonctionnalités réelles, vous souhaiterez probablement appeler un service enregistré dans une injection de dépendances qui se connecte à une base de données ou à un autre type de magasin d’utilisateurs. Accédez à votre service à l’aide du contexte passé dans votre délégué. Prenons l’exemple suivant dans `Startup.ConfigureServices` :

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

D’un plan conceptuel, la validation du certificat est un problème d’autorisation. L’ajout d’un contrôle, par exemple, à un émetteur ou une empreinte numérique dans une stratégie d’autorisation `OnCertificateValidated`, plutôt qu’à l’intérieur de, est parfaitement acceptable.

## <a name="configure-your-host-to-require-certificates"></a>Configuration de votre hôte pour exiger des certificats

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

Effectuez les étapes suivantes dans le gestionnaire des services Internet :

1. Sélectionnez votre site à partir de l’onglet **connexions** .
1. Double-cliquez sur l’option **Paramètres SSL** dans la fenêtre **affichage des fonctionnalités** .
1. Cochez la case **exiger SSL** , puis sélectionnez la case d’option **exiger** dans la section **certificats clients** .

![Paramètres de certificat client dans IIS](README-IISConfig.png)

### <a name="azure-and-custom-web-proxies"></a>Proxys Web Azure et personnalisés

Consultez la [documentation sur l’hôte et le déploiement](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) pour savoir comment configurer l’intergiciel (middleware) de transfert de certificats.
