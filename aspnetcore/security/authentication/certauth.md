---
title: Configurer l’authentification par certificat dans ASP.NET Core
author: blowdart
description: Découvrez comment configurer l’authentification par certificat dans ASP.NET Core pour IIS et HTTP. sys.
monikerRange: '>= aspnetcore-3.0'
ms.author: bdorrans
ms.date: 08/19/2019
uid: security/authentication/certauth
ms.openlocfilehash: bb375cf380175daf2399f3b56f543819ee5692b8
ms.sourcegitcommit: 07cd66e367d080acb201c7296809541599c947d1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71039243"
---
# <a name="configure-certificate-authentication-in-aspnet-core"></a><span data-ttu-id="88b97-103">Configurer l’authentification par certificat dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="88b97-103">Configure certificate authentication in ASP.NET Core</span></span>

<span data-ttu-id="88b97-104">`Microsoft.AspNetCore.Authentication.Certificate`contient une implémentation similaire à [l’authentification par certificat](https://tools.ietf.org/html/rfc5246#section-7.4.4) pour ASP.net core.</span><span class="sxs-lookup"><span data-stu-id="88b97-104">`Microsoft.AspNetCore.Authentication.Certificate` contains an implementation similar to [Certificate Authentication](https://tools.ietf.org/html/rfc5246#section-7.4.4) for ASP.NET Core.</span></span> <span data-ttu-id="88b97-105">L’authentification par certificat se produit au niveau du TLS, à long terme avant qu’il ne soit ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="88b97-105">Certificate authentication happens at the TLS level, long before it ever gets to ASP.NET Core.</span></span> <span data-ttu-id="88b97-106">Plus précisément, il s’agit d’un gestionnaire d’authentification qui valide le certificat et vous donne un événement dans lequel vous pouvez résoudre ce certificat en `ClaimsPrincipal`.</span><span class="sxs-lookup"><span data-stu-id="88b97-106">More accurately, this is an authentication handler that validates the certificate and then gives you an event where you can resolve that certificate to a `ClaimsPrincipal`.</span></span> 

<span data-ttu-id="88b97-107">[Configurez votre hôte](#configure-your-host-to-require-certificates) pour l’authentification par certificat, qu’il soit IIS, Kestrel, Azure Web Apps ou tout autre que vous utilisez.</span><span class="sxs-lookup"><span data-stu-id="88b97-107">[Configure your host](#configure-your-host-to-require-certificates) for certificate authentication, be it IIS, Kestrel, Azure Web Apps, or whatever else you're using.</span></span>

## <a name="proxy-and-load-balancer-scenarios"></a><span data-ttu-id="88b97-108">Scénarios de proxy et d’équilibrage de charge</span><span class="sxs-lookup"><span data-stu-id="88b97-108">Proxy and load balancer scenarios</span></span>

<span data-ttu-id="88b97-109">L’authentification par certificat est un scénario avec état principalement utilisé dans lequel un proxy ou un équilibreur de charge ne gère pas le trafic entre les clients et les serveurs.</span><span class="sxs-lookup"><span data-stu-id="88b97-109">Certificate authentication is a stateful scenario primarily used where a proxy or load balancer doesn't handle traffic between clients and servers.</span></span> <span data-ttu-id="88b97-110">Si un proxy ou un équilibreur de charge est utilisé, l’authentification par certificat fonctionne uniquement si le proxy ou l’équilibreur de charge :</span><span class="sxs-lookup"><span data-stu-id="88b97-110">If a proxy or load balancer is used, certificate authentication only works if the proxy or load balancer:</span></span>

* <span data-ttu-id="88b97-111">Gère l’authentification.</span><span class="sxs-lookup"><span data-stu-id="88b97-111">Handles the authentication.</span></span>
* <span data-ttu-id="88b97-112">Transmet les informations d’authentification de l’utilisateur à l’application (par exemple, dans un en-tête de demande), qui agit sur les informations d’authentification.</span><span class="sxs-lookup"><span data-stu-id="88b97-112">Passes the user authentication information to the app (for example, in a request header), which acts on the authentication information.</span></span>

<span data-ttu-id="88b97-113">Une alternative à l’authentification par certificat dans les environnements où les proxies et les équilibreurs de charge sont utilisés est Active Directory Services fédérés (ADFS) avec OpenID Connect (OIDC).</span><span class="sxs-lookup"><span data-stu-id="88b97-113">An alternative to certificate authentication in environments where proxies and load balancers are used is Active Directory Federated Services (ADFS) with OpenID Connect (OIDC).</span></span>

## <a name="get-started"></a><span data-ttu-id="88b97-114">Prise en main</span><span class="sxs-lookup"><span data-stu-id="88b97-114">Get started</span></span>

<span data-ttu-id="88b97-115">Obtenez un certificat HTTPs, appliquez-le et [configurez votre hôte](#configure-your-host-to-require-certificates) pour exiger des certificats.</span><span class="sxs-lookup"><span data-stu-id="88b97-115">Acquire an HTTPS certificate, apply it, and [configure your host](#configure-your-host-to-require-certificates) to require certificates.</span></span>

<span data-ttu-id="88b97-116">Dans votre application Web, ajoutez une référence au `Microsoft.AspNetCore.Authentication.Certificate` package.</span><span class="sxs-lookup"><span data-stu-id="88b97-116">In your web app, add a reference to the `Microsoft.AspNetCore.Authentication.Certificate` package.</span></span> <span data-ttu-id="88b97-117">Ensuite, dans `Startup.Configure` la méthode, `app.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).UseCertificateAuthentication(...);` appelez avec vos options `OnCertificateValidated` , en fournissant un délégué à pour effectuer une validation supplémentaire sur le certificat client envoyé avec les demandes.</span><span class="sxs-lookup"><span data-stu-id="88b97-117">Then in the `Startup.Configure` method, call `app.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).UseCertificateAuthentication(...);` with your options, providing a delegate for `OnCertificateValidated` to do any supplementary validation on the client certificate sent with requests.</span></span> <span data-ttu-id="88b97-118">Transformez ces informations `ClaimsPrincipal` en un et définissez- `context.Principal` les sur la propriété.</span><span class="sxs-lookup"><span data-stu-id="88b97-118">Turn that information into a `ClaimsPrincipal` and set it on the `context.Principal` property.</span></span>

<span data-ttu-id="88b97-119">Si l’authentification échoue, ce gestionnaire renvoie `403 (Forbidden)` une réponse plutôt `401 (Unauthorized)`qu’un, comme vous pouvez l’imaginer.</span><span class="sxs-lookup"><span data-stu-id="88b97-119">If authentication fails, this handler returns a `403 (Forbidden)` response rather a `401 (Unauthorized)`, as you might expect.</span></span> <span data-ttu-id="88b97-120">Le raisonnement est que l’authentification doit se produire pendant la connexion TLS initiale.</span><span class="sxs-lookup"><span data-stu-id="88b97-120">The reasoning is that the authentication should happen during the initial TLS connection.</span></span> <span data-ttu-id="88b97-121">Au moment où il atteint le gestionnaire, il est trop tard.</span><span class="sxs-lookup"><span data-stu-id="88b97-121">By the time it reaches the handler, it's too late.</span></span> <span data-ttu-id="88b97-122">Il n’existe aucun moyen de mettre à niveau la connexion d’une connexion anonyme à une avec un certificat.</span><span class="sxs-lookup"><span data-stu-id="88b97-122">There's no way to upgrade the connection from an anonymous connection to one with a certificate.</span></span>

<span data-ttu-id="88b97-123">Ajoutez `app.UseAuthentication();` également dans la `Startup.Configure` méthode.</span><span class="sxs-lookup"><span data-stu-id="88b97-123">Also add `app.UseAuthentication();` in the `Startup.Configure` method.</span></span> <span data-ttu-id="88b97-124">Sinon, HttpContext. User ne sera pas défini à `ClaimsPrincipal` créé à partir du certificat.</span><span class="sxs-lookup"><span data-stu-id="88b97-124">Otherwise, the HttpContext.User will not be set to `ClaimsPrincipal` created from the certificate.</span></span> <span data-ttu-id="88b97-125">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="88b97-125">For example:</span></span>

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

<span data-ttu-id="88b97-126">L’exemple précédent illustre la méthode par défaut pour ajouter l’authentification par certificat.</span><span class="sxs-lookup"><span data-stu-id="88b97-126">The preceding example demonstrates the default way to add certificate authentication.</span></span> <span data-ttu-id="88b97-127">Le gestionnaire construit un principal d’utilisateur à l’aide des propriétés de certificat courantes.</span><span class="sxs-lookup"><span data-stu-id="88b97-127">The handler constructs a user principal using the common certificate properties.</span></span>

## <a name="configure-certificate-validation"></a><span data-ttu-id="88b97-128">Configurer la validation des certificats</span><span class="sxs-lookup"><span data-stu-id="88b97-128">Configure certificate validation</span></span>

<span data-ttu-id="88b97-129">Le `CertificateAuthenticationOptions` gestionnaire a des validations intégrées qui sont les validations minimales que vous devez effectuer sur un certificat.</span><span class="sxs-lookup"><span data-stu-id="88b97-129">The `CertificateAuthenticationOptions` handler has some built-in validations that are the minimum validations you should perform on a certificate.</span></span> <span data-ttu-id="88b97-130">Chacun de ces paramètres est activé par défaut.</span><span class="sxs-lookup"><span data-stu-id="88b97-130">Each of these settings is enabled by default.</span></span>

### <a name="allowedcertificatetypes--chained-selfsigned-or-all-chained--selfsigned"></a><span data-ttu-id="88b97-131">AllowedCertificateTypes = chained, SelfSigned ou All (chaîné | SelfSigned)</span><span class="sxs-lookup"><span data-stu-id="88b97-131">AllowedCertificateTypes = Chained, SelfSigned, or All (Chained | SelfSigned)</span></span>

<span data-ttu-id="88b97-132">Cette vérification valide que seul le type de certificat approprié est autorisé.</span><span class="sxs-lookup"><span data-stu-id="88b97-132">This check validates that only the appropriate certificate type is allowed.</span></span>

### <a name="validatecertificateuse"></a><span data-ttu-id="88b97-133">ValidateCertificateUse</span><span class="sxs-lookup"><span data-stu-id="88b97-133">ValidateCertificateUse</span></span>

<span data-ttu-id="88b97-134">Cette vérification permet de vérifier que le certificat présenté par le client a l’utilisation améliorée de la clé d’authentification du client, ou aucune EKU.</span><span class="sxs-lookup"><span data-stu-id="88b97-134">This check validates that the certificate presented by the client has the Client Authentication extended key use (EKU), or no EKUs at all.</span></span> <span data-ttu-id="88b97-135">Comme les spécifications indiquent, si aucune EKU n’est spécifiée, toutes les utilisations améliorées de la EKU sont considérées comme valides.</span><span class="sxs-lookup"><span data-stu-id="88b97-135">As the specifications say, if no EKU is specified, then all EKUs are deemed valid.</span></span>

### <a name="validatevalidityperiod"></a><span data-ttu-id="88b97-136">ValidateValidityPeriod</span><span class="sxs-lookup"><span data-stu-id="88b97-136">ValidateValidityPeriod</span></span>

<span data-ttu-id="88b97-137">Ce contrôle vérifie que le certificat se trouve dans sa période de validité.</span><span class="sxs-lookup"><span data-stu-id="88b97-137">This check validates that the certificate is within its validity period.</span></span> <span data-ttu-id="88b97-138">À chaque demande, le gestionnaire s’assure qu’un certificat valide lorsqu’il a été présenté n’a pas expiré pendant sa session active.</span><span class="sxs-lookup"><span data-stu-id="88b97-138">On each request, the handler ensures that a certificate that was valid when it was presented hasn't expired during its current session.</span></span>

### <a name="revocationflag"></a><span data-ttu-id="88b97-139">RevocationFlag</span><span class="sxs-lookup"><span data-stu-id="88b97-139">RevocationFlag</span></span>

<span data-ttu-id="88b97-140">Indicateur qui spécifie les certificats de la chaîne qui sont vérifiés pour la révocation.</span><span class="sxs-lookup"><span data-stu-id="88b97-140">A flag that specifies which certificates in the chain are checked for revocation.</span></span>

<span data-ttu-id="88b97-141">Les vérifications de révocation sont effectuées uniquement lorsque le certificat est chaîné à un certificat racine.</span><span class="sxs-lookup"><span data-stu-id="88b97-141">Revocation checks are only performed when the certificate is chained to a root certificate.</span></span>

### <a name="revocationmode"></a><span data-ttu-id="88b97-142">RevocationMode</span><span class="sxs-lookup"><span data-stu-id="88b97-142">RevocationMode</span></span>

<span data-ttu-id="88b97-143">Indicateur qui spécifie comment les vérifications de révocation sont effectuées.</span><span class="sxs-lookup"><span data-stu-id="88b97-143">A flag that specifies how revocation checks are performed.</span></span>

<span data-ttu-id="88b97-144">La spécification d’un contrôle en ligne peut entraîner un délai de longue durée pendant que l’autorité de certification est contactée.</span><span class="sxs-lookup"><span data-stu-id="88b97-144">Specifying an online check can result in a long delay while the certificate authority is contacted.</span></span>

<span data-ttu-id="88b97-145">Les vérifications de révocation sont effectuées uniquement lorsque le certificat est chaîné à un certificat racine.</span><span class="sxs-lookup"><span data-stu-id="88b97-145">Revocation checks are only performed when the certificate is chained to a root certificate.</span></span>

### <a name="can-i-configure-my-app-to-require-a-certificate-only-on-certain-paths"></a><span data-ttu-id="88b97-146">Puis-je configurer mon application pour exiger un certificat uniquement sur certains chemins d’accès ?</span><span class="sxs-lookup"><span data-stu-id="88b97-146">Can I configure my app to require a certificate only on certain paths?</span></span>

<span data-ttu-id="88b97-147">Cela n’est pas possible.</span><span class="sxs-lookup"><span data-stu-id="88b97-147">This isn't possible.</span></span> <span data-ttu-id="88b97-148">Rappelez-vous que l’échange de certificats est effectué au début de la conversation HTTPs, car il est effectué par le serveur avant la réception de la première requête sur cette connexion, ce qui signifie qu’il n’est pas possible d’effectuer une étendue basée sur des champs de demande.</span><span class="sxs-lookup"><span data-stu-id="88b97-148">Remember the certificate exchange is done that the start of the HTTPS conversation, it's done by the server before the first request is received on that connection so it's not possible to scope based on any request fields.</span></span>

## <a name="handler-events"></a><span data-ttu-id="88b97-149">Événements du gestionnaire</span><span class="sxs-lookup"><span data-stu-id="88b97-149">Handler events</span></span>

<span data-ttu-id="88b97-150">Le gestionnaire a deux événements :</span><span class="sxs-lookup"><span data-stu-id="88b97-150">The handler has two events:</span></span>

* <span data-ttu-id="88b97-151">`OnAuthenticationFailed`&ndash; Appelé si une exception se produit pendant l’authentification et vous permet de réagir.</span><span class="sxs-lookup"><span data-stu-id="88b97-151">`OnAuthenticationFailed` &ndash; Called if an exception happens during authentication and allows you to react.</span></span>
* <span data-ttu-id="88b97-152">`OnCertificateValidated`&ndash; Appelée une fois que le certificat a été validé, la validation a réussi et un principal par défaut a été créé.</span><span class="sxs-lookup"><span data-stu-id="88b97-152">`OnCertificateValidated` &ndash; Called after the certificate has been validated, passed validation and a default principal has been created.</span></span> <span data-ttu-id="88b97-153">Cet événement vous permet d’effectuer votre propre validation et d’augmenter ou de remplacer le principal.</span><span class="sxs-lookup"><span data-stu-id="88b97-153">This event allows you to perform your own validation and augment or replace the principal.</span></span> <span data-ttu-id="88b97-154">Voici quelques exemples :</span><span class="sxs-lookup"><span data-stu-id="88b97-154">For examples include:</span></span>
  * <span data-ttu-id="88b97-155">Déterminer si le certificat est connu de vos services.</span><span class="sxs-lookup"><span data-stu-id="88b97-155">Determining if the certificate is known to your services.</span></span>
  * <span data-ttu-id="88b97-156">Construction de votre propre principal.</span><span class="sxs-lookup"><span data-stu-id="88b97-156">Constructing your own principal.</span></span> <span data-ttu-id="88b97-157">Prenons l’exemple suivant dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="88b97-157">Consider the following example in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="88b97-158">Si vous trouvez que le certificat entrant ne répond pas à votre validation supplémentaire `context.Fail("failure reason")` , appelez en raison d’une erreur.</span><span class="sxs-lookup"><span data-stu-id="88b97-158">If you find the inbound certificate doesn't meet your extra validation, call `context.Fail("failure reason")` with a failure reason.</span></span>

<span data-ttu-id="88b97-159">Pour les fonctionnalités réelles, vous souhaiterez probablement appeler un service enregistré dans une injection de dépendances qui se connecte à une base de données ou à un autre type de magasin d’utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="88b97-159">For real functionality, you'll probably want to call a service registered in dependency injection that connects to a database or other type of user store.</span></span> <span data-ttu-id="88b97-160">Accédez à votre service à l’aide du contexte passé dans votre délégué.</span><span class="sxs-lookup"><span data-stu-id="88b97-160">Access your service by using the context passed into your delegate.</span></span> <span data-ttu-id="88b97-161">Prenons l’exemple suivant dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="88b97-161">Consider the following example in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="88b97-162">D’un plan conceptuel, la validation du certificat est un problème d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="88b97-162">Conceptually, the validation of the certificate is an authorization concern.</span></span> <span data-ttu-id="88b97-163">L’ajout d’un contrôle, par exemple, à un émetteur ou une empreinte numérique dans une stratégie d’autorisation `OnCertificateValidated`, plutôt qu’à l’intérieur de, est parfaitement acceptable.</span><span class="sxs-lookup"><span data-stu-id="88b97-163">Adding a check on, for example, an issuer or thumbprint in an authorization policy, rather than inside `OnCertificateValidated`, is perfectly acceptable.</span></span>

## <a name="configure-your-host-to-require-certificates"></a><span data-ttu-id="88b97-164">Configuration de votre hôte pour exiger des certificats</span><span class="sxs-lookup"><span data-stu-id="88b97-164">Configure your host to require certificates</span></span>

### <a name="kestrel"></a><span data-ttu-id="88b97-165">Kestrel</span><span class="sxs-lookup"><span data-stu-id="88b97-165">Kestrel</span></span>

<span data-ttu-id="88b97-166">Dans *Program.cs*, configurez Kestrel comme suit :</span><span class="sxs-lookup"><span data-stu-id="88b97-166">In *Program.cs*, configure Kestrel as follows:</span></span>

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

### <a name="iis"></a><span data-ttu-id="88b97-167">IIS</span><span class="sxs-lookup"><span data-stu-id="88b97-167">IIS</span></span>

<span data-ttu-id="88b97-168">Effectuez les étapes suivantes dans le gestionnaire des services Internet :</span><span class="sxs-lookup"><span data-stu-id="88b97-168">Complete the following steps In IIS Manager:</span></span>

1. <span data-ttu-id="88b97-169">Sélectionnez votre site à partir de l’onglet **connexions** .</span><span class="sxs-lookup"><span data-stu-id="88b97-169">Select your site from the **Connections** tab.</span></span>
1. <span data-ttu-id="88b97-170">Double-cliquez sur l’option **Paramètres SSL** dans la fenêtre **affichage des fonctionnalités** .</span><span class="sxs-lookup"><span data-stu-id="88b97-170">Double-click the **SSL Settings** option in the **Features View** window.</span></span>
1. <span data-ttu-id="88b97-171">Cochez la case **exiger SSL** , puis sélectionnez la case d’option **exiger** dans la section **certificats clients** .</span><span class="sxs-lookup"><span data-stu-id="88b97-171">Check the **Require SSL** checkbox, and select the **Require** radio button in the **Client certificates** section.</span></span>

![Paramètres de certificat client dans IIS](README-IISConfig.png)

### <a name="azure-and-custom-web-proxies"></a><span data-ttu-id="88b97-173">Proxys Web Azure et personnalisés</span><span class="sxs-lookup"><span data-stu-id="88b97-173">Azure and custom web proxies</span></span>

<span data-ttu-id="88b97-174">Consultez la [documentation sur l’hôte et le déploiement](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) pour savoir comment configurer l’intergiciel (middleware) de transfert de certificats.</span><span class="sxs-lookup"><span data-stu-id="88b97-174">See the [host and deploy documentation](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) for how to configure the certificate forwarding middleware.</span></span>
