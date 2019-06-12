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
# <a name="overview"></a><span data-ttu-id="c3023-103">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="c3023-103">Overview</span></span>

<span data-ttu-id="c3023-104">`Microsoft.AspNetCore.Authentication.Certificate` contient une implémentation semblable à [l’authentification par certificat](https://tools.ietf.org/html/rfc5246#section-7.4.4) pour ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c3023-104">`Microsoft.AspNetCore.Authentication.Certificate` contains an implementation similar to [Certificate Authentication](https://tools.ietf.org/html/rfc5246#section-7.4.4) for ASP.NET Core.</span></span> <span data-ttu-id="c3023-105">L’authentification par certificat se produit au niveau TLS, long avant qu’il jamais ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c3023-105">Certificate authentication happens at the TLS level, long before it ever gets to ASP.NET Core.</span></span> <span data-ttu-id="c3023-106">Il s’agit plus précisément, un gestionnaire d’authentification qui valide le certificat et vous donne ensuite un événement où vous pouvez résoudre ce certificat pour un `ClaimsPrincipal`.</span><span class="sxs-lookup"><span data-stu-id="c3023-106">More accurately, this is an authentication handler that validates the certificate and then gives you an event where you can resolve that certificate to a `ClaimsPrincipal`.</span></span> 

<span data-ttu-id="c3023-107">[Configurer votre hôte](#configure-your-host-to-require-certificates) l’authentification par certificat, qu’il s’agisse IIS, Kestrel, Azure Web Apps, ou les autres éléments que vous utilisez.</span><span class="sxs-lookup"><span data-stu-id="c3023-107">[Configure your host](#configure-your-host-to-require-certificates) for certificate authentication, be it IIS, Kestrel, Azure Web Apps, or whatever else you're using.</span></span>

## <a name="get-started"></a><span data-ttu-id="c3023-108">Prise en main</span><span class="sxs-lookup"><span data-stu-id="c3023-108">Get started</span></span>

<span data-ttu-id="c3023-109">Acquérir un certificat HTTPS, appliquez-le, et [configurer votre hôte](#configure-your-host-to-require-certificates) pour exiger les certificats.</span><span class="sxs-lookup"><span data-stu-id="c3023-109">Acquire an HTTPS certificate, apply it, and [configure your host](#configure-your-host-to-require-certificates) to require certificates.</span></span>

<span data-ttu-id="c3023-110">Dans votre application web, ajoutez une référence à la `Microsoft.AspNetCore.Authentication.Certificate` package.</span><span class="sxs-lookup"><span data-stu-id="c3023-110">In your web app, add a reference to the `Microsoft.AspNetCore.Authentication.Certificate` package.</span></span> <span data-ttu-id="c3023-111">Ensuite, dans le `Startup.Configure` méthode, appelez `app.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).UseCertificateAuthentication(...);` de vos options, fournissant un délégué pour `OnCertificateValidated` pour effectuer une validation supplémentaire sur le certificat client envoyé avec les demandes.</span><span class="sxs-lookup"><span data-stu-id="c3023-111">Then in the `Startup.Configure` method, call `app.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).UseCertificateAuthentication(...);` with your options, providing a delegate for `OnCertificateValidated` to do any supplementary validation on the client certificate sent with requests.</span></span> <span data-ttu-id="c3023-112">Activer ces informations dans un `ClaimsPrincipal` et définissez-le sur le `context.Principal` propriété.</span><span class="sxs-lookup"><span data-stu-id="c3023-112">Turn that information into a `ClaimsPrincipal` and set it on the `context.Principal` property.</span></span>

<span data-ttu-id="c3023-113">Si l’authentification échoue, ce gestionnaire renvoie un `403 (Forbidden)` réponse plutôt un `401 (Unauthorized)`, comme vous pouvez l’imaginer.</span><span class="sxs-lookup"><span data-stu-id="c3023-113">If authentication fails, this handler returns a `403 (Forbidden)` response rather a `401 (Unauthorized)`, as you might expect.</span></span> <span data-ttu-id="c3023-114">Le raisonnement est que l’authentification doit se produire pendant la connexion TLS initiale.</span><span class="sxs-lookup"><span data-stu-id="c3023-114">The reasoning is that the authentication should happen during the initial TLS connection.</span></span> <span data-ttu-id="c3023-115">Au moment où qu'il atteint le gestionnaire, il est trop tard.</span><span class="sxs-lookup"><span data-stu-id="c3023-115">By the time it reaches the handler, it's too late.</span></span> <span data-ttu-id="c3023-116">Il n’existe aucun moyen pour mettre à niveau de la connexion à partir d’une connexion anonyme à une avec un certificat.</span><span class="sxs-lookup"><span data-stu-id="c3023-116">There's no way to upgrade the connection from an anonymous connection to one with a certificate.</span></span>

<span data-ttu-id="c3023-117">Ajoutez également `app.UseAuthentication();` dans le `Startup.Configure` (méthode).</span><span class="sxs-lookup"><span data-stu-id="c3023-117">Also add `app.UseAuthentication();` in the `Startup.Configure` method.</span></span> <span data-ttu-id="c3023-118">Sinon, le HttpContext.User ne sera pas définie `ClaimsPrincipal` créé à partir du certificat.</span><span class="sxs-lookup"><span data-stu-id="c3023-118">Otherwise, the HttpContext.User will not be set to `ClaimsPrincipal` created from the certificate.</span></span> <span data-ttu-id="c3023-119">Exemple :</span><span class="sxs-lookup"><span data-stu-id="c3023-119">For example:</span></span>

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

<span data-ttu-id="c3023-120">L’exemple précédent montre la façon par défaut pour ajouter l’authentification par certificat.</span><span class="sxs-lookup"><span data-stu-id="c3023-120">The preceding example demonstrates the default way to add certificate authentication.</span></span> <span data-ttu-id="c3023-121">Le gestionnaire construit un principal d’utilisateur à l’aide des propriétés du certificat commun.</span><span class="sxs-lookup"><span data-stu-id="c3023-121">The handler constructs a user principal using the common certificate properties.</span></span>

## <a name="configure-certificate-validation"></a><span data-ttu-id="c3023-122">Configurer la validation du certificat</span><span class="sxs-lookup"><span data-stu-id="c3023-122">Configure certificate validation</span></span>

<span data-ttu-id="c3023-123">Le `CertificateAuthenticationOptions` gestionnaire a certaines validations intégrées qui sont les validations minimale, vous devez effectuer sur un certificat.</span><span class="sxs-lookup"><span data-stu-id="c3023-123">The `CertificateAuthenticationOptions` handler has some built-in validations that are the minimum validations you should perform on a certificate.</span></span> <span data-ttu-id="c3023-124">Chacun de ces paramètres est activée par défaut.</span><span class="sxs-lookup"><span data-stu-id="c3023-124">Each of these settings is enabled by default.</span></span>

### <a name="allowedcertificatetypes--chained-selfsigned-or-all-chained--selfsigned"></a><span data-ttu-id="c3023-125">AllowedCertificateTypes = chaînées, SelfSigned, ou l’ensemble (chaînées | SelfSigned)</span><span class="sxs-lookup"><span data-stu-id="c3023-125">AllowedCertificateTypes = Chained, SelfSigned, or All (Chained | SelfSigned)</span></span>

<span data-ttu-id="c3023-126">Cette vérification valide le fait que seul le type de certificat approprié est autorisé.</span><span class="sxs-lookup"><span data-stu-id="c3023-126">This check validates that only the appropriate certificate type is allowed.</span></span>

### <a name="validatecertificateuse"></a><span data-ttu-id="c3023-127">ValidateCertificateUse</span><span class="sxs-lookup"><span data-stu-id="c3023-127">ValidateCertificateUse</span></span>

<span data-ttu-id="c3023-128">Cette vérification valide le fait que le certificat présenté par le client a l’authentification du Client étendus d’utilisation de la clé (EKU), ou aucun EKU du tout.</span><span class="sxs-lookup"><span data-stu-id="c3023-128">This check validates that the certificate presented by the client has the Client Authentication extended key use (EKU), or no EKUs at all.</span></span> <span data-ttu-id="c3023-129">Comme les spécifications par exemple, si aucun EKU n’est spécifié, toutes les utilisations sont considérés comme valides.</span><span class="sxs-lookup"><span data-stu-id="c3023-129">As the specifications say, if no EKU is specified, then all EKUs are deemed valid.</span></span>

### <a name="validatevalidityperiod"></a><span data-ttu-id="c3023-130">ValidateValidityPeriod</span><span class="sxs-lookup"><span data-stu-id="c3023-130">ValidateValidityPeriod</span></span>

<span data-ttu-id="c3023-131">Cette vérification valide le fait que le certificat est dans sa période de validité.</span><span class="sxs-lookup"><span data-stu-id="c3023-131">This check validates that the certificate is within its validity period.</span></span> <span data-ttu-id="c3023-132">À chaque demande, le gestionnaire s’assure qu’un certificat qui a été valid lorsqu’il a été présenté n’a pas expiré pendant sa session en cours.</span><span class="sxs-lookup"><span data-stu-id="c3023-132">On each request, the handler ensures that a certificate that was valid when it was presented hasn't expired during its current session.</span></span>

### <a name="revocationflag"></a><span data-ttu-id="c3023-133">RevocationFlag</span><span class="sxs-lookup"><span data-stu-id="c3023-133">RevocationFlag</span></span>

<span data-ttu-id="c3023-134">Un indicateur qui spécifie les certificats de la chaîne sont vérifiés pour révocation.</span><span class="sxs-lookup"><span data-stu-id="c3023-134">A flag that specifies which certificates in the chain are checked for revocation.</span></span>

<span data-ttu-id="c3023-135">Vérifications de révocation sont exécutées uniquement lorsque le certificat est chaîné à un certificat racine.</span><span class="sxs-lookup"><span data-stu-id="c3023-135">Revocation checks are only performed when the certificate is chained to a root certificate.</span></span>

### <a name="revocationmode"></a><span data-ttu-id="c3023-136">RevocationMode</span><span class="sxs-lookup"><span data-stu-id="c3023-136">RevocationMode</span></span>

<span data-ttu-id="c3023-137">Un indicateur qui spécifie la façon dont sont effectuées les vérifications de révocation.</span><span class="sxs-lookup"><span data-stu-id="c3023-137">A flag that specifies how revocation checks are performed.</span></span>

<span data-ttu-id="c3023-138">Spécification d’un contrôle en ligne peut entraîner un long délai pendant que l’autorité de certification est contactée.</span><span class="sxs-lookup"><span data-stu-id="c3023-138">Specifying an online check can result in a long delay while the certificate authority is contacted.</span></span>

<span data-ttu-id="c3023-139">Vérifications de révocation sont exécutées uniquement lorsque le certificat est chaîné à un certificat racine.</span><span class="sxs-lookup"><span data-stu-id="c3023-139">Revocation checks are only performed when the certificate is chained to a root certificate.</span></span>

### <a name="can-i-configure-my-app-to-require-a-certificate-only-on-certain-paths"></a><span data-ttu-id="c3023-140">Puis-je configurer mon application pour demander un certificat uniquement sur certains chemins d’accès ?</span><span class="sxs-lookup"><span data-stu-id="c3023-140">Can I configure my app to require a certificate only on certain paths?</span></span>

<span data-ttu-id="c3023-141">Cela n’est pas possible.</span><span class="sxs-lookup"><span data-stu-id="c3023-141">This isn't possible.</span></span> <span data-ttu-id="c3023-142">N’oubliez pas de que l’échange de certificat s’effectue que le début de la conversation HTTPS, il est exécuté par le serveur avant la première demande est reçue sur cette connexion, il est donc pas possible de portée basé sur les champs de la demande.</span><span class="sxs-lookup"><span data-stu-id="c3023-142">Remember the certificate exchange is done that the start of the HTTPS conversation, it's done by the server before the first request is received on that connection so it's not possible to scope based on any request fields.</span></span>

## <a name="handler-events"></a><span data-ttu-id="c3023-143">Gestionnaire d’événements</span><span class="sxs-lookup"><span data-stu-id="c3023-143">Handler events</span></span>

<span data-ttu-id="c3023-144">Le gestionnaire a deux événements :</span><span class="sxs-lookup"><span data-stu-id="c3023-144">The handler has two events:</span></span>

* <span data-ttu-id="c3023-145">`OnAuthenticationFailed` &ndash; Appelé si une exception se produit lors de l’authentification et vous permet de réagir.</span><span class="sxs-lookup"><span data-stu-id="c3023-145">`OnAuthenticationFailed` &ndash; Called if an exception happens during authentication and allows you to react.</span></span>
* <span data-ttu-id="c3023-146">`OnCertificateValidated` &ndash; Appelée une fois que le certificat a été validé, ont réussi la validation et un principal par défaut a été créé.</span><span class="sxs-lookup"><span data-stu-id="c3023-146">`OnCertificateValidated` &ndash; Called after the certificate has been validated, passed validation and a default principal has been created.</span></span> <span data-ttu-id="c3023-147">Cet événement permet d’effectuer votre propre validation et d’augmenter ou de remplacer l’objet principal.</span><span class="sxs-lookup"><span data-stu-id="c3023-147">This event allows you to perform your own validation and augment or replace the principal.</span></span> <span data-ttu-id="c3023-148">Pour obtenir des exemples sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="c3023-148">For examples include:</span></span>
  * <span data-ttu-id="c3023-149">Déterminer si le certificat est connu à vos services.</span><span class="sxs-lookup"><span data-stu-id="c3023-149">Determining if the certificate is known to your services.</span></span>
  * <span data-ttu-id="c3023-150">Construction de votre propre objet principal.</span><span class="sxs-lookup"><span data-stu-id="c3023-150">Constructing your own principal.</span></span> <span data-ttu-id="c3023-151">Prenons l’exemple suivant dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c3023-151">Consider the following example in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="c3023-152">Si vous trouvez le certificat entrant ne répond pas à votre validation supplémentaire, appelez `context.Fail("failure reason")` avec un motif de l’échec.</span><span class="sxs-lookup"><span data-stu-id="c3023-152">If you find the inbound certificate doesn't meet your extra validation, call `context.Fail("failure reason")` with a failure reason.</span></span>

<span data-ttu-id="c3023-153">Fonctionnalités de réel, vous voudrez probablement appeler un service inscrit dans l’injection de dépendances qui se connecte à une base de données ou un autre type de magasin de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="c3023-153">For real functionality, you'll probably want to call a service registered in dependency injection that connects to a database or other type of user store.</span></span> <span data-ttu-id="c3023-154">Accéder à votre service en utilisant le contexte passé dans votre délégué.</span><span class="sxs-lookup"><span data-stu-id="c3023-154">Access your service by using the context passed into your delegate.</span></span> <span data-ttu-id="c3023-155">Prenons l’exemple suivant dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c3023-155">Consider the following example in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="c3023-156">Conceptuellement, la validation du certificat est un problème d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="c3023-156">Conceptually, the validation of the certificate is an authorization concern.</span></span> <span data-ttu-id="c3023-157">Ajout d’une vérification, par exemple, un émetteur ou une empreinte numérique dans une stratégie d’autorisation, plutôt qu’à l’intérieur `OnCertificateValidated`, est parfaitement acceptable.</span><span class="sxs-lookup"><span data-stu-id="c3023-157">Adding a check on, for example, an issuer or thumbprint in an authorization policy, rather than inside `OnCertificateValidated`, is perfectly acceptable.</span></span>

## <a name="configure-your-host-to-require-certificates"></a><span data-ttu-id="c3023-158">Configurer votre hôte pour exiger des certificats</span><span class="sxs-lookup"><span data-stu-id="c3023-158">Configure your host to require certificates</span></span>

### <a name="kestrel"></a><span data-ttu-id="c3023-159">Kestrel</span><span class="sxs-lookup"><span data-stu-id="c3023-159">Kestrel</span></span>

<span data-ttu-id="c3023-160">Dans *Program.cs*, configurez Kestrel comme suit :</span><span class="sxs-lookup"><span data-stu-id="c3023-160">In *Program.cs*, configure Kestrel as follows:</span></span>

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

### <a name="iis"></a><span data-ttu-id="c3023-161">IIS</span><span class="sxs-lookup"><span data-stu-id="c3023-161">IIS</span></span>

<span data-ttu-id="c3023-162">Procédez comme suit dans le Gestionnaire des services Internet :</span><span class="sxs-lookup"><span data-stu-id="c3023-162">Complete the following steps In IIS Manager:</span></span>

1. <span data-ttu-id="c3023-163">Sélectionnez votre site à partir de la **connexions** onglet.</span><span class="sxs-lookup"><span data-stu-id="c3023-163">Select your site from the **Connections** tab.</span></span>
1. <span data-ttu-id="c3023-164">Double-cliquez sur le **paramètres SSL** option dans le **affichage des fonctionnalités** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="c3023-164">Double-click the **SSL Settings** option in the **Features View** window.</span></span>
1. <span data-ttu-id="c3023-165">Vérifier le **exiger SSL** case à cocher, puis sélectionnez le **nécessitent** case d’option dans le **certificats clients** section.</span><span class="sxs-lookup"><span data-stu-id="c3023-165">Check the **Require SSL** checkbox, and select the **Require** radio button in the **Client certificates** section.</span></span>

![Paramètres du certificat client dans IIS](README-IISConfig.png)

### <a name="azure-and-custom-web-proxies"></a><span data-ttu-id="c3023-167">Azure et les proxys web personnalisé</span><span class="sxs-lookup"><span data-stu-id="c3023-167">Azure and custom web proxies</span></span>

<span data-ttu-id="c3023-168">Consultez le [héberger et déployer la documentation](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) pour savoir comment configurer le transfert d’intergiciel (middleware) de certificat.</span><span class="sxs-lookup"><span data-stu-id="c3023-168">See the [host and deploy documentation](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) for how to configure the certificate forwarding middleware.</span></span>
