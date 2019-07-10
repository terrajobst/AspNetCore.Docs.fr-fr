---
title: Règlement de Protection de données générales (RGPD) prend en charge dans ASP.NET Core
author: rick-anderson
description: Découvrez comment accéder aux points d’extension RGPD dans une application web ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/11/2019
uid: security/gdpr
ms.openlocfilehash: 01d2f8943c0995c1400122b89c4ca7c459a85279
ms.sourcegitcommit: bee530454ae2b3c25dc7ffebf93536f479a14460
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67724565"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a><span data-ttu-id="72eec-103">Prise en charge de l’Union européenne général Protection des données règlement (RGPD) dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="72eec-103">EU General Data Protection Regulation (GDPR) support in ASP.NET Core</span></span>

<span data-ttu-id="72eec-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="72eec-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="72eec-105">ASP.NET Core fournit des API et des modèles pour répondre à certaines de la [règlement de Protection générale données (RGPD) d’Europe](https://www.eugdpr.org/) exigences :</span><span class="sxs-lookup"><span data-stu-id="72eec-105">ASP.NET Core provides APIs and templates to help meet some of the [EU General Data Protection Regulation (GDPR)](https://www.eugdpr.org/) requirements:</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="72eec-106">Les modèles de projet incluent des points d’extension et de stub balisage que vous pouvez remplacer votre stratégie d’utilisation de cookies et de confidentialité.</span><span class="sxs-lookup"><span data-stu-id="72eec-106">The project templates include extension points and stubbed markup that you can replace with your privacy and cookie use policy.</span></span>
* <span data-ttu-id="72eec-107">Le *Pages/Privacy.cshtml* page ou *Views/Home/Privacy.cshtml* vue fournit une page qui décrit en détail la politique de confidentialité de votre site.</span><span class="sxs-lookup"><span data-stu-id="72eec-107">The *Pages/Privacy.cshtml* page or *Views/Home/Privacy.cshtml* view provides a page to detail your site's privacy policy.</span></span>

<span data-ttu-id="72eec-108">Pour activer la fonctionnalité de consentement de cookie par défaut comme celui trouvé dans les modèles ASP.NET Core 2.2 dans une application de modèle générée d’ASP.NET Core 3.0 :</span><span class="sxs-lookup"><span data-stu-id="72eec-108">To enable the default cookie consent feature like that found in the ASP.NET Core 2.2 templates in an ASP.NET Core 3.0 template generated app:</span></span>

* <span data-ttu-id="72eec-109">Ajouter [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) trop `Startup.ConfigureServices` et [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) à `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="72eec-109">Add [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) too `Startup.ConfigureServices` and [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) to `Startup.Configure`:</span></span>

  [!code-csharp[Main](gdpr/sample/RP3.0/Startup.cs?name=snippet1&highlight=12-19,38)]

* <span data-ttu-id="72eec-110">Ajouter le partielle de consentement de cookie pour le *_Layout.cshtml* fichier :</span><span class="sxs-lookup"><span data-stu-id="72eec-110">Add the cookie consent partial to the *_Layout.cshtml* file:</span></span>

  [!code-cshtml[Main](gdpr/sample/RP3.0/Pages/Shared/_Layout.cshtml?name=snippet&highlight=4)]

* <span data-ttu-id="72eec-111">Ajouter le  *\_CookieConsentPartial.cshtml* fichier au projet :</span><span class="sxs-lookup"><span data-stu-id="72eec-111">Add the *\_CookieConsentPartial.cshtml* file to the project:</span></span>

  [!code-cshtml[Main](gdpr/sample/RP3.0/Pages/Shared/_CookieConsentPartial.cshtml)]

* <span data-ttu-id="72eec-112">Sélectionnez la version d’ASP.NET Core 2.2 de cet article pour en savoir plus sur la fonctionnalité de consentement du cookie.</span><span class="sxs-lookup"><span data-stu-id="72eec-112">Select the ASP.NET Core 2.2 version of this article to read about the cookie consent feature.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

* <span data-ttu-id="72eec-113">Les modèles de projet incluent des points d’extension et de stub balisage que vous pouvez remplacer votre stratégie d’utilisation de cookies et de confidentialité.</span><span class="sxs-lookup"><span data-stu-id="72eec-113">The project templates include extension points and stubbed markup that you can replace with your privacy and cookie use policy.</span></span>
* <span data-ttu-id="72eec-114">Une fonctionnalité de consentement de cookie vous permet demandera (et le suivi) consentement à partir de vos utilisateurs pour le stockage des informations personnelles.</span><span class="sxs-lookup"><span data-stu-id="72eec-114">A cookie consent feature allows you to ask for (and track) consent from your users for storing personal information.</span></span> <span data-ttu-id="72eec-115">Si un utilisateur n’a pas consenti à la collecte de données et de l’application a [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) défini sur `true`, les cookies non essentielles ne sont pas envoyés au navigateur.</span><span class="sxs-lookup"><span data-stu-id="72eec-115">If a user hasn't consented to data collection and the app has [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) set to `true`, non-essential cookies aren't sent to the browser.</span></span>
* <span data-ttu-id="72eec-116">Les cookies peuvent être marqués comme essentielles.</span><span class="sxs-lookup"><span data-stu-id="72eec-116">Cookies can be marked as essential.</span></span> <span data-ttu-id="72eec-117">Les cookies essentielles sont envoyés au navigateur même lorsque l’utilisateur n’a pas donné son consentement et le suivi est désactivé.</span><span class="sxs-lookup"><span data-stu-id="72eec-117">Essential cookies are sent to the browser even when the user hasn't consented and tracking is disabled.</span></span>
* <span data-ttu-id="72eec-118">[Les cookies TempData et Session](#tempdata) ne sont pas fonctionnelles lorsque le suivi est désactivé.</span><span class="sxs-lookup"><span data-stu-id="72eec-118">[TempData and Session cookies](#tempdata) aren't functional when tracking is disabled.</span></span>
* <span data-ttu-id="72eec-119">Le [gérer les identités](#pd) page fournit un lien pour télécharger et supprimer des données de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="72eec-119">The [Identity manage](#pd) page provides a link to download and delete user data.</span></span>

<span data-ttu-id="72eec-120">Le [exemple d’application](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) permet de vous tester la plupart des points d’extension RGPD et API ajoutées aux modèles ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="72eec-120">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) allows you test most of the GDPR extension points and APIs added to the ASP.NET Core 2.1 templates.</span></span> <span data-ttu-id="72eec-121">Consultez le [Lisez-moi](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) fichier d’instructions de test.</span><span class="sxs-lookup"><span data-stu-id="72eec-121">See the [ReadMe](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) file for testing instructions.</span></span>

<span data-ttu-id="72eec-122">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="72eec-122">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a><span data-ttu-id="72eec-123">ASP.NET Core RGPD prennent en charge dans le code généré par le modèle</span><span class="sxs-lookup"><span data-stu-id="72eec-123">ASP.NET Core GDPR support in template-generated code</span></span>

<span data-ttu-id="72eec-124">Les Pages Razor et MVC projets créés avec les modèles de projet incluent la prise en charge de RGPD suivante :</span><span class="sxs-lookup"><span data-stu-id="72eec-124">Razor Pages and MVC projects created with the project templates include the following GDPR support:</span></span>

* <span data-ttu-id="72eec-125">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) et [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) sont définies le `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="72eec-125">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) and [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) are set in the `Startup` class.</span></span>
* <span data-ttu-id="72eec-126">Le  *\_CookieConsentPartial.cshtml* [vue partielle](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="72eec-126">The *\_CookieConsentPartial.cshtml* [partial view](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span></span> <span data-ttu-id="72eec-127">Un **Accept** bouton est inclus dans ce fichier.</span><span class="sxs-lookup"><span data-stu-id="72eec-127">An **Accept** button is included in this file.</span></span> <span data-ttu-id="72eec-128">Lorsque l’utilisateur clique sur le **Accept** bouton, de donner son consentement pour stocker des cookies est fourni.</span><span class="sxs-lookup"><span data-stu-id="72eec-128">When the user clicks the **Accept** button, consent to store cookies is provided.</span></span>
* <span data-ttu-id="72eec-129">Le *Pages/Privacy.cshtml* page ou *Views/Home/Privacy.cshtml* vue fournit une page qui décrit en détail la politique de confidentialité de votre site.</span><span class="sxs-lookup"><span data-stu-id="72eec-129">The *Pages/Privacy.cshtml* page or *Views/Home/Privacy.cshtml* view provides a page to detail your site's privacy policy.</span></span> <span data-ttu-id="72eec-130">Le  *\_CookieConsentPartial.cshtml* fichier génère un lien vers la page de la confidentialité.</span><span class="sxs-lookup"><span data-stu-id="72eec-130">The *\_CookieConsentPartial.cshtml* file generates a link to the Privacy page.</span></span>
* <span data-ttu-id="72eec-131">Pour les applications créées avec des comptes d’utilisateur individuels, la page de gestion fournit des liens pour télécharger et supprimer des [données personnelles utilisateur](#pd).</span><span class="sxs-lookup"><span data-stu-id="72eec-131">For apps created with individual user accounts, the Manage page provides links to download and delete [personal user data](#pd).</span></span>

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a><span data-ttu-id="72eec-132">CookiePolicyOptions et UseCookiePolicy</span><span class="sxs-lookup"><span data-stu-id="72eec-132">CookiePolicyOptions and UseCookiePolicy</span></span>

<span data-ttu-id="72eec-133">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) sont initialisées dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="72eec-133">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) are initialized in `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

<span data-ttu-id="72eec-134">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) est appelée `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="72eec-134">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) is called in `Startup.Configure`:</span></span>

[!code-csharp[](gdpr/sample/Startup.cs?name=snippet1&highlight=51)]

### <a name="cookieconsentpartialcshtml-partial-view"></a><span data-ttu-id="72eec-135">\_Vue partielle CookieConsentPartial.cshtml</span><span class="sxs-lookup"><span data-stu-id="72eec-135">\_CookieConsentPartial.cshtml partial view</span></span>

<span data-ttu-id="72eec-136">Le  *\_CookieConsentPartial.cshtml* vue partielle :</span><span class="sxs-lookup"><span data-stu-id="72eec-136">The *\_CookieConsentPartial.cshtml* partial view:</span></span>

[!code-html[](gdpr/sample/RP2.2/Pages/Shared/_CookieConsentPartial.cshtml)]

<span data-ttu-id="72eec-137">Partielle :</span><span class="sxs-lookup"><span data-stu-id="72eec-137">This partial:</span></span>

* <span data-ttu-id="72eec-138">Obtient l’état de suivi pour l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="72eec-138">Obtains the state of tracking for the user.</span></span> <span data-ttu-id="72eec-139">Si l’application est configurée pour demander le consentement, l’utilisateur doit donner son consentement avant que les cookies peuvent être suivis.</span><span class="sxs-lookup"><span data-stu-id="72eec-139">If the app is configured to require consent, the user must consent before cookies can be tracked.</span></span> <span data-ttu-id="72eec-140">Si un consentement est requis, le panneau de consentement de cookie est résolu en haut de la barre de navigation créée par le  *\_Layout.cshtml* fichier.</span><span class="sxs-lookup"><span data-stu-id="72eec-140">If consent is required, the cookie consent panel is fixed at top of the navigation bar created by the *\_Layout.cshtml* file.</span></span>
* <span data-ttu-id="72eec-141">Fournit un élément HTML `<p>` élément permet de synthétiser votre confidentialité et le cookie utiliser la stratégie.</span><span class="sxs-lookup"><span data-stu-id="72eec-141">Provides an HTML `<p>` element to summarize your privacy and cookie use policy.</span></span>
* <span data-ttu-id="72eec-142">Fournit un lien vers la page de la confidentialité ou la vue dans laquelle vous pouvez décrit en détail la politique de confidentialité de votre site.</span><span class="sxs-lookup"><span data-stu-id="72eec-142">Provides a link to Privacy page or view where you can detail your site's privacy policy.</span></span>

## <a name="essential-cookies"></a><span data-ttu-id="72eec-143">Cookies essentielles</span><span class="sxs-lookup"><span data-stu-id="72eec-143">Essential cookies</span></span>

<span data-ttu-id="72eec-144">Si donner son consentement pour stocker des cookies n’a pas été fourni, uniquement les cookies marqués essentielles sont envoyés au navigateur.</span><span class="sxs-lookup"><span data-stu-id="72eec-144">If consent to store cookies hasn't been provided, only cookies marked essential are sent to the browser.</span></span> <span data-ttu-id="72eec-145">Le code suivant fait un cookie essentielles :</span><span class="sxs-lookup"><span data-stu-id="72eec-145">The following code makes a cookie essential:</span></span>

[!code-csharp[Main](gdpr/sample/RP2.2/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

### <a name="tempdata-provider-and-session-state-cookies-arent-essential"></a><span data-ttu-id="72eec-146">Cookies d’état de session et le fournisseur TempData ne sont pas essentiels</span><span class="sxs-lookup"><span data-stu-id="72eec-146">TempData provider and session state cookies aren't essential</span></span>

<span data-ttu-id="72eec-147">Le [fournisseur TempData](xref:fundamentals/app-state#tempdata) cookie n’est pas indispensable.</span><span class="sxs-lookup"><span data-stu-id="72eec-147">The [TempData provider](xref:fundamentals/app-state#tempdata) cookie isn't essential.</span></span> <span data-ttu-id="72eec-148">Si le suivi est désactivé, le fournisseur de TempData n’est pas fonctionnel.</span><span class="sxs-lookup"><span data-stu-id="72eec-148">If tracking is disabled, the TempData provider isn't functional.</span></span> <span data-ttu-id="72eec-149">Pour activer le fournisseur TempData lorsque le suivi est désactivé, marquer le cookie de TempData comme essentielle dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="72eec-149">To enable the TempData provider when tracking is disabled, mark the TempData cookie as essential in `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/RP2.2/Startup.cs?name=snippet1)]

<span data-ttu-id="72eec-150">[État de session](xref:fundamentals/app-state) les cookies ne sont pas indispensables.</span><span class="sxs-lookup"><span data-stu-id="72eec-150">[Session state](xref:fundamentals/app-state) cookies are not essential.</span></span> <span data-ttu-id="72eec-151">État de session n’est pas fonctionnel lorsque le suivi est désactivé.</span><span class="sxs-lookup"><span data-stu-id="72eec-151">Session state isn't functional when tracking is disabled.</span></span> <span data-ttu-id="72eec-152">Le code suivant fait les cookies de session essentielles :</span><span class="sxs-lookup"><span data-stu-id="72eec-152">The following code makes session cookies essential:</span></span>

[!code-csharp[](gdpr/sample/RP2.2/Startup.cs?name=snippet2)]

<a name="pd"></a>

## <a name="personal-data"></a><span data-ttu-id="72eec-153">Données personnelles</span><span class="sxs-lookup"><span data-stu-id="72eec-153">Personal data</span></span>

<span data-ttu-id="72eec-154">Les applications ASP.NET Core créées avec les comptes d’utilisateur individuels incluent du code pour télécharger et supprimer des données personnelles.</span><span class="sxs-lookup"><span data-stu-id="72eec-154">ASP.NET Core apps created with individual user accounts include code to download and delete personal data.</span></span>

<span data-ttu-id="72eec-155">Sélectionnez le nom d’utilisateur, puis **les données personnelles**:</span><span class="sxs-lookup"><span data-stu-id="72eec-155">Select the user name and then select **Personal data**:</span></span>

![Gérer les données personnelles](gdpr/_static/pd.png)

<span data-ttu-id="72eec-157">Remarques :</span><span class="sxs-lookup"><span data-stu-id="72eec-157">Notes:</span></span>

* <span data-ttu-id="72eec-158">Pour générer le `Account/Manage` de code, consultez [identité d’une structure](xref:security/authentication/scaffold-identity).</span><span class="sxs-lookup"><span data-stu-id="72eec-158">To generate the `Account/Manage` code, see [Scaffold Identity](xref:security/authentication/scaffold-identity).</span></span>
* <span data-ttu-id="72eec-159">Le **supprimer** et **télécharger** liens agissent uniquement sur les données d’identité par défaut.</span><span class="sxs-lookup"><span data-stu-id="72eec-159">The **Delete** and **Download** links only act on the default identity data.</span></span> <span data-ttu-id="72eec-160">Les applications qui créent des données utilisateur personnalisé doivent être étendues pour delete/télécharger les données utilisateur personnalisées.</span><span class="sxs-lookup"><span data-stu-id="72eec-160">Apps that create custom user data must be extended to delete/download the custom user data.</span></span> <span data-ttu-id="72eec-161">Pour plus d’informations, consultez [ajouter, télécharger et supprimer les données d’utilisateur personnalisée pour identité](xref:security/authentication/add-user-data).</span><span class="sxs-lookup"><span data-stu-id="72eec-161">For more information, see [Add, download, and delete custom user data to Identity](xref:security/authentication/add-user-data).</span></span>
* <span data-ttu-id="72eec-162">Enregistré des jetons pour l’utilisateur qui sont stockées dans la table de base de données d’identité `AspNetUserTokens` sont supprimés lorsque l’utilisateur est supprimé via le comportement de suppression en cascade en raison du [clé étrangère](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).</span><span class="sxs-lookup"><span data-stu-id="72eec-162">Saved tokens for the user that are stored in the Identity database table `AspNetUserTokens` are deleted when the user is deleted via the cascading delete behavior due to the [foreign key](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).</span></span>
* <span data-ttu-id="72eec-163">[L’authentification du fournisseur externe](xref:security/authentication/social/index), tels que Facebook et Google, n’est pas disponible avant l’acceptation de la stratégie de cookie.</span><span class="sxs-lookup"><span data-stu-id="72eec-163">[External provider authentication](xref:security/authentication/social/index), such as Facebook and Google, isn't available before the cookie policy is accepted.</span></span>

::: moniker-end

## <a name="encryption-at-rest"></a><span data-ttu-id="72eec-164">Chiffrement au repos</span><span class="sxs-lookup"><span data-stu-id="72eec-164">Encryption at rest</span></span>

<span data-ttu-id="72eec-165">Certaines bases de données et les mécanismes de stockage autorisent le chiffrement au repos.</span><span class="sxs-lookup"><span data-stu-id="72eec-165">Some databases and storage mechanisms allow for encryption at rest.</span></span> <span data-ttu-id="72eec-166">Chiffrement au repos :</span><span class="sxs-lookup"><span data-stu-id="72eec-166">Encryption at rest:</span></span>

* <span data-ttu-id="72eec-167">Chiffre automatiquement les données stockées.</span><span class="sxs-lookup"><span data-stu-id="72eec-167">Encrypts stored data automatically.</span></span>
* <span data-ttu-id="72eec-168">Chiffre sans configuration, de programmation ou d’autres tâches pour le logiciel qui accède aux données.</span><span class="sxs-lookup"><span data-stu-id="72eec-168">Encrypts without configuration, programming, or other work for the software that accesses the data.</span></span>
* <span data-ttu-id="72eec-169">Option est la plus simple et plus sûre.</span><span class="sxs-lookup"><span data-stu-id="72eec-169">Is the easiest and safest option.</span></span>
* <span data-ttu-id="72eec-170">Permet de gérer les clés et chiffrement de la base de données.</span><span class="sxs-lookup"><span data-stu-id="72eec-170">Allows the database to manage keys and encryption.</span></span>

<span data-ttu-id="72eec-171">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="72eec-171">For example:</span></span>

* <span data-ttu-id="72eec-172">Microsoft SQL et SQL Azure fournissent [Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).</span><span class="sxs-lookup"><span data-stu-id="72eec-172">Microsoft SQL and Azure SQL provide [Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).</span></span>
* [<span data-ttu-id="72eec-173">SQL Azure chiffre de la base de données par défaut</span><span class="sxs-lookup"><span data-stu-id="72eec-173">SQL Azure encrypts the database by default</span></span>](https://azure.microsoft.com/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* <span data-ttu-id="72eec-174">[Objets BLOB, de fichiers, de Table et d’un stockage file d’attente Azure sont chiffrées par défaut](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span><span class="sxs-lookup"><span data-stu-id="72eec-174">[Azure Blobs, Files, Table, and Queue Storage are encrypted by default](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span></span>

<span data-ttu-id="72eec-175">Pour les bases de données qui ne fournissent pas un chiffrement intégré au repos, vous pourrez peut-être utiliser le chiffrement de disque pour fournir le même niveau de protection.</span><span class="sxs-lookup"><span data-stu-id="72eec-175">For databases that don't provide built-in encryption at rest, you may be able to use disk encryption to provide the same protection.</span></span> <span data-ttu-id="72eec-176">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="72eec-176">For example:</span></span>

* [<span data-ttu-id="72eec-177">BitLocker pour Windows Server</span><span class="sxs-lookup"><span data-stu-id="72eec-177">BitLocker for Windows Server</span></span>](/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* <span data-ttu-id="72eec-178">Linux :</span><span class="sxs-lookup"><span data-stu-id="72eec-178">Linux:</span></span>
  * [<span data-ttu-id="72eec-179">eCryptfs</span><span class="sxs-lookup"><span data-stu-id="72eec-179">eCryptfs</span></span>](https://launchpad.net/ecryptfs)
  * <span data-ttu-id="72eec-180">[EncFS](https://github.com/vgough/encfs).</span><span class="sxs-lookup"><span data-stu-id="72eec-180">[EncFS](https://github.com/vgough/encfs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="72eec-181">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="72eec-181">Additional resources</span></span>

* [<span data-ttu-id="72eec-182">Microsoft.com/GDPR</span><span class="sxs-lookup"><span data-stu-id="72eec-182">Microsoft.com/GDPR</span></span>](https://www.microsoft.com/trustcenter/Privacy/GDPR)
