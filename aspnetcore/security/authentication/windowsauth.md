---
title: Configurer l’authentification Windows dans ASP.NET Core
author: scottaddie
description: Découvrez comment configurer l’authentification Windows dans ASP.NET Core pour IIS et HTTP.sys.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 07/01/2019
uid: security/authentication/windowsauth
ms.openlocfilehash: 30f1f554a29412ed6b84115d457d2da1aba91c17
ms.sourcegitcommit: eb3e51d58dd713eefc242148f45bd9486be3a78a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67500499"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="b9b5c-103">Configurer l’authentification Windows dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b9b5c-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="b9b5c-104">Par [Scott Addie](https://twitter.com/Scott_Addie) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b9b5c-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b9b5c-105">Authentification Windows (également appelé authentification Negotiate, Kerberos ou NTLM) peut être configurée pour les applications ASP.NET Core hébergées avec [IIS](xref:host-and-deploy/iis/index), [Kestrel](xref:fundamentals/servers/kestrel), ou [HTTP.sys](xref:fundamentals/servers/httpsys) .</span><span class="sxs-lookup"><span data-stu-id="b9b5c-105">Windows Authentication (also known as Negotiate, Kerberos, or NTLM authentication) can be configured for ASP.NET Core apps hosted with [IIS](xref:host-and-deploy/iis/index), [Kestrel](xref:fundamentals/servers/kestrel), or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="b9b5c-106">Authentification Windows (également appelé authentification Negotiate, Kerberos ou NTLM) peut être configurée pour les applications ASP.NET Core hébergées avec [IIS](xref:host-and-deploy/iis/index) ou [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="b9b5c-106">Windows Authentication (also known as Negotiate, Kerberos, or NTLM authentication) can be configured for ASP.NET Core apps hosted with [IIS](xref:host-and-deploy/iis/index) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

::: moniker-end

<span data-ttu-id="b9b5c-107">L’authentification Windows s’appuie sur le système d’exploitation pour authentifier les utilisateurs des applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-107">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="b9b5c-108">Vous pouvez utiliser l’authentification Windows lorsque votre serveur s’exécute sur un réseau d’entreprise à l’aide d’identités de domaine Active Directory ou les comptes Windows pour identifier les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-108">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or Windows accounts to identify users.</span></span> <span data-ttu-id="b9b5c-109">L’authentification Windows est mieux adaptée aux environnements intranet où les utilisateurs, les applications clientes et les serveurs web appartiennent au même domaine Windows.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-109">Windows Authentication is best suited to intranet environments where users, client apps, and web servers belong to the same Windows domain.</span></span>

> [!NOTE]
> <span data-ttu-id="b9b5c-110">L’authentification Windows n’est pas pris en charge avec HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-110">Windows Authentication isn't supported with HTTP/2.</span></span> <span data-ttu-id="b9b5c-111">Demandes d’authentification peuvent être envoyés sur les réponses HTTP/2, mais le client doit mettre à niveau vers HTTP/1.1 avant l’authentification.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-111">Authentication challenges can be sent on HTTP/2 responses, but the client must downgrade to HTTP/1.1 before authenticating.</span></span>

## <a name="iisiis-express"></a><span data-ttu-id="b9b5c-112">IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="b9b5c-112">IIS/IIS Express</span></span>

<span data-ttu-id="b9b5c-113">Ajoutez des services d’authentification en appelant <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> espace de noms) dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b9b5c-113">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> namespace) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

### <a name="launch-settings-debugger"></a><span data-ttu-id="b9b5c-114">Lancer des paramètres (débogueur)</span><span class="sxs-lookup"><span data-stu-id="b9b5c-114">Launch settings (debugger)</span></span>

<span data-ttu-id="b9b5c-115">Configuration des paramètres de lancement affecte uniquement le *Properties/launchSettings.json* de fichiers pour IIS Express et ne configure IIS pour l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-115">Configuration for launch settings only affects the *Properties/launchSettings.json* file for IIS Express and doesn't configure IIS for Windows Authentication.</span></span> <span data-ttu-id="b9b5c-116">Configuration du serveur est expliquée dans le [IIS](#iis) section.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-116">Server configuration is explained in the [IIS](#iis) section.</span></span>

<span data-ttu-id="b9b5c-117">Le **Web Application** modèle disponible via Visual Studio ou l’interface CLI .NET Core peut être configuré pour prendre en charge l’authentification Windows, ce qui met à jour le *Properties/launchSettings.json* fichier automatiquement.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-117">The **Web Application** template available via Visual Studio or the .NET Core CLI can be configured to support Windows Authentication, which updates the *Properties/launchSettings.json* file automatically.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b9b5c-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b9b5c-118">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b9b5c-119">**nouveau projet**</span><span class="sxs-lookup"><span data-stu-id="b9b5c-119">**New project**</span></span>

1. <span data-ttu-id="b9b5c-120">Créer un nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-120">Create a new project.</span></span>
1. <span data-ttu-id="b9b5c-121">Sélectionnez **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-121">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="b9b5c-122">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-122">Select **Next**.</span></span>
1. <span data-ttu-id="b9b5c-123">Fournissez un nom dans la **nom_projet** champ.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-123">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="b9b5c-124">Confirmer la **emplacement** entrée est correcte ou indiquez un emplacement pour le projet.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-124">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="b9b5c-125">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-125">Select **Create**.</span></span>
1. <span data-ttu-id="b9b5c-126">Sélectionnez **modification** sous **authentification**.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-126">Select **Change** under **Authentication**.</span></span>
1. <span data-ttu-id="b9b5c-127">Dans le **modifier l’authentification** fenêtre, sélectionnez **l’authentification Windows**.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-127">In the **Change Authentication** window, select **Windows Authentication**.</span></span> <span data-ttu-id="b9b5c-128">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-128">Select **OK**.</span></span>
1. <span data-ttu-id="b9b5c-129">Sélectionnez **Application web**.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-129">Select **Web Application**.</span></span>
1. <span data-ttu-id="b9b5c-130">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-130">Select **Create**.</span></span>

<span data-ttu-id="b9b5c-131">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-131">Run the app.</span></span> <span data-ttu-id="b9b5c-132">Le nom d’utilisateur s’affiche dans l’interface utilisateur de l’application rendue.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-132">The username appears in the rendered app's user interface.</span></span>

<span data-ttu-id="b9b5c-133">**Projet existant**</span><span class="sxs-lookup"><span data-stu-id="b9b5c-133">**Existing project**</span></span>

<span data-ttu-id="b9b5c-134">Les propriétés du projet activer l’authentification Windows et désactivez l’authentification anonyme :</span><span class="sxs-lookup"><span data-stu-id="b9b5c-134">The project's properties enable Windows Authentication and disable Anonymous Authentication:</span></span>

1. <span data-ttu-id="b9b5c-135">Cliquez avec le bouton droit sur le projet dans **l’Explorateur de solutions**, puis sélectionnez **Propriétés**.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-135">Right-click the project in **Solution Explorer** and select **Properties**.</span></span>
1. <span data-ttu-id="b9b5c-136">Sélectionnez l’onglet **Débogage**.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-136">Select the **Debug** tab.</span></span>
1. <span data-ttu-id="b9b5c-137">Désactivez la case à cocher **activer l’authentification anonyme**.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-137">Clear the check box for **Enable Anonymous Authentication**.</span></span>
1. <span data-ttu-id="b9b5c-138">Sélectionnez la case à cocher **activer l’authentification Windows**.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-138">Select the check box for **Enable Windows Authentication**.</span></span>
1. <span data-ttu-id="b9b5c-139">Enregistrez et fermez la page de propriétés.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-139">Save and close the property page.</span></span>

<span data-ttu-id="b9b5c-140">Vous pouvez également les propriétés peuvent être configurées dans le `iisSettings` nœud de la *launchSettings.json* fichier :</span><span class="sxs-lookup"><span data-stu-id="b9b5c-140">Alternatively, the properties can be configured in the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="b9b5c-141">Visual Studio Code/.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b9b5c-141">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

<span data-ttu-id="b9b5c-142">**nouveau projet**</span><span class="sxs-lookup"><span data-stu-id="b9b5c-142">**New project**</span></span>

<span data-ttu-id="b9b5c-143">Exécuter le [dotnet nouvelle](/dotnet/core/tools/dotnet-new) commande avec le `webapp` argument (application de Web ASP.NET Core) et `--auth Windows` commutateur :</span><span class="sxs-lookup"><span data-stu-id="b9b5c-143">Execute the [dotnet new](/dotnet/core/tools/dotnet-new) command with the `webapp` argument (ASP.NET Core Web App) and `--auth Windows` switch:</span></span>

```console
dotnet new webapp --auth Windows
```

<span data-ttu-id="b9b5c-144">**Projet existant**</span><span class="sxs-lookup"><span data-stu-id="b9b5c-144">**Existing project**</span></span>

<span data-ttu-id="b9b5c-145">Mise à jour le `iisSettings` nœud de la *launchSettings.json* fichier :</span><span class="sxs-lookup"><span data-stu-id="b9b5c-145">Update the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

---

<span data-ttu-id="b9b5c-146">Lorsque vous modifiez un projet existant, vérifiez que le fichier projet contient une référence de package pour le [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app) **ou** le [ Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) package NuGet.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-146">When modifying an existing project, confirm that the project file includes a package reference for the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) **or** the [Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet package.</span></span>

### <a name="iis"></a><span data-ttu-id="b9b5c-147">IIS</span><span class="sxs-lookup"><span data-stu-id="b9b5c-147">IIS</span></span>

<span data-ttu-id="b9b5c-148">IIS utilise le [Module ASP.NET Core](xref:host-and-deploy/aspnet-core-module) pour héberger des applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-148">IIS uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="b9b5c-149">L’authentification Windows est configurée pour IIS via le *web.config* fichier.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-149">Windows Authentication is configured for IIS via the *web.config* file.</span></span> <span data-ttu-id="b9b5c-150">Les sections suivantes montrent comment :</span><span class="sxs-lookup"><span data-stu-id="b9b5c-150">The following sections show how to:</span></span>

* <span data-ttu-id="b9b5c-151">Fournir une variable locale *web.config* fichier qui active l’authentification Windows sur le serveur quand l’application est déployée.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-151">Provide a local *web.config* file that activates Windows Authentication on the server when the app is deployed.</span></span>
* <span data-ttu-id="b9b5c-152">Utilisez le Gestionnaire des services Internet pour configurer le *web.config* fichier d’une application ASP.NET Core qui a déjà été déployée sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-152">Use the IIS Manager to configure the *web.config* file of an ASP.NET Core app that has already been deployed to the server.</span></span>

<span data-ttu-id="b9b5c-153">Si vous n’avez pas déjà fait, activez IIS pour héberger des applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-153">If you haven't already done so, enable IIS to host ASP.NET Core apps.</span></span> <span data-ttu-id="b9b5c-154">Pour plus d'informations, consultez <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-154">For more information, see <xref:host-and-deploy/iis/index>.</span></span>

<span data-ttu-id="b9b5c-155">Activer le Service de rôle IIS pour l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-155">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="b9b5c-156">Pour plus d’informations, consultez [activer l’authentification Windows dans les Services de rôle IIS (voir étape 2)](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="b9b5c-156">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="b9b5c-157">[Intergiciel d’intégration IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) est configuré pour authentifier les demandes automatiquement par défaut.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-157">[IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="b9b5c-158">Pour plus d’informations, consultez [héberger ASP.NET Core sur Windows avec IIS : Options d’IIS (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="b9b5c-158">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="b9b5c-159">Le Module ASP.NET Core est configuré pour transférer le jeton d’authentification de Windows à l’application par défaut.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-159">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="b9b5c-160">Pour plus d’informations, consultez [référence de configuration de Module ASP.NET Core : Attributs de l’élément aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="b9b5c-160">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

<span data-ttu-id="b9b5c-161">Utilisez **soit** des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="b9b5c-161">Use **either** of the following approaches:</span></span>

* <span data-ttu-id="b9b5c-162">**Avant la publication et le déploiement du projet,** ajoutez le code suivant *web.config* fichier à la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="b9b5c-162">**Before publishing and deploying the project,** add the following *web.config* file to the project root:</span></span>

  [!code-xml[](windowsauth/sample_snapshot/web_2.config)]

  <span data-ttu-id="b9b5c-163">Lorsque le projet est publié par le SDK .NET Core (sans le `<IsTransformWebConfigDisabled>` propriété définie sur `true` dans le fichier projet), publié *web.config* fichier inclut le `<location><system.webServer><security><authentication>` section.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-163">When the project is published by the .NET Core SDK (without the `<IsTransformWebConfigDisabled>` property set to `true` in the project file), the published *web.config* file includes the `<location><system.webServer><security><authentication>` section.</span></span> <span data-ttu-id="b9b5c-164">Pour plus d’informations sur la `<IsTransformWebConfigDisabled>` propriété, consultez <xref:host-and-deploy/iis/index#webconfig-file>.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-164">For more information on the `<IsTransformWebConfigDisabled>` property, see <xref:host-and-deploy/iis/index#webconfig-file>.</span></span>

* <span data-ttu-id="b9b5c-165">**Après la publication et le déploiement du projet,** effectuer la configuration côté serveur avec le Gestionnaire des services Internet :</span><span class="sxs-lookup"><span data-stu-id="b9b5c-165">**After publishing and deploying the project,** perform server-side configuration with the IIS Manager:</span></span>

  1. <span data-ttu-id="b9b5c-166">Dans le Gestionnaire des services Internet, sélectionnez le site IIS sous la **Sites** nœud de la **connexions** encadré.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-166">In IIS Manager, select the IIS site under the **Sites** node of the **Connections** sidebar.</span></span>
  1. <span data-ttu-id="b9b5c-167">Double-cliquez sur **authentification** dans le **IIS** zone.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-167">Double-click **Authentication** in the **IIS** area.</span></span>
  1. <span data-ttu-id="b9b5c-168">Sélectionnez **l’authentification anonyme**.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-168">Select **Anonymous Authentication**.</span></span> <span data-ttu-id="b9b5c-169">Sélectionnez **désactiver** dans le **Actions** encadré.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-169">Select **Disable** in the **Actions** sidebar.</span></span>
  1. <span data-ttu-id="b9b5c-170">Sélectionnez **l’authentification Windows**.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-170">Select **Windows Authentication**.</span></span> <span data-ttu-id="b9b5c-171">Sélectionnez **activer** dans le **Actions** encadré.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-171">Select **Enable** in the **Actions** sidebar.</span></span>

  <span data-ttu-id="b9b5c-172">Lorsque ces actions sont effectuées, le Gestionnaire des services Internet modifie l’application *web.config* fichier.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-172">When these actions are taken, IIS Manager modifies the app's *web.config* file.</span></span> <span data-ttu-id="b9b5c-173">Un `<system.webServer><security><authentication>` nœud est ajouté avec les paramètres mis à jour pour `anonymousAuthentication` et `windowsAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="b9b5c-173">A `<system.webServer><security><authentication>` node is added with updated settings for `anonymousAuthentication` and `windowsAuthentication`:</span></span>

  [!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

  <span data-ttu-id="b9b5c-174">Le `<system.webServer>` section ajoutée à la *web.config* fichier par le Gestionnaire des services Internet se trouve en dehors de l’application `<location>` section ajoutée par le SDK .NET Core lors de l’application est publiée.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-174">The `<system.webServer>` section added to the *web.config* file by IIS Manager is outside of the app's `<location>` section added by the .NET Core SDK when the app is published.</span></span> <span data-ttu-id="b9b5c-175">Étant donné que la section est ajoutée à l’extérieur de la `<location>` nœud, les paramètres sont hérités par les [sous-applications](xref:host-and-deploy/iis/index#sub-applications) à l’application actuelle.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-175">Because the section is added outside of the `<location>` node, the settings are inherited by any [sub-apps](xref:host-and-deploy/iis/index#sub-applications) to the current app.</span></span> <span data-ttu-id="b9b5c-176">Pour empêcher l’héritage, déplacer la `<security>` section à l’intérieur de la `<location><system.webServer>` section fourni le SDK .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-176">To prevent inheritance, move the added `<security>` section inside of the `<location><system.webServer>` section that the .NET Core SDK provided.</span></span>

  <span data-ttu-id="b9b5c-177">Lorsque le Gestionnaire des services Internet est utilisé pour ajouter la configuration d’IIS, il affecte uniquement l’application *web.config* fichier sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-177">When IIS Manager is used to add the IIS configuration, it only affects the app's *web.config* file on the server.</span></span> <span data-ttu-id="b9b5c-178">Un autre déploiement de l’application peut remplacer les paramètres sur le serveur si la copie du serveur de *web.config* est remplacé par le projet *web.config* fichier.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-178">A subsequent deployment of the app may overwrite the settings on the server if the server's copy of *web.config* is replaced by the project's *web.config* file.</span></span> <span data-ttu-id="b9b5c-179">Utilisez **soit** des approches suivantes pour gérer les paramètres :</span><span class="sxs-lookup"><span data-stu-id="b9b5c-179">Use **either** of the following approaches to manage the settings:</span></span>

  * <span data-ttu-id="b9b5c-180">Utilisez le Gestionnaire IIS pour réinitialiser les paramètres dans le *web.config* une fois que le fichier est remplacé sur le déploiement de fichiers.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-180">Use IIS Manager to reset the settings in the *web.config* file after the file is overwritten on deployment.</span></span>
  * <span data-ttu-id="b9b5c-181">Ajouter un *fichier web.config* à l’application localement avec les paramètres.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-181">Add a *web.config file* to the app locally with the settings.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="kestrel"></a><span data-ttu-id="b9b5c-182">Kestrel</span><span class="sxs-lookup"><span data-stu-id="b9b5c-182">Kestrel</span></span>

 <span data-ttu-id="b9b5c-183">Le [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) package NuGet peut être utilisé avec [Kestrel](xref:fundamentals/servers/kestrel) pour prendre en charge l’authentification Windows à l’aide de Negotiate, Kerberos et NTLM sur Windows, Linux et macOS.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-183">The [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) NuGet package can be used with [Kestrel](xref:fundamentals/servers/kestrel) to support Windows Authentication using Negotiate, Kerberos, and NTLM on Windows, Linux, and macOS.</span></span>

> [!WARNING]
> <span data-ttu-id="b9b5c-184">Informations d’identification peuvent être conservées dans les demandes sur une connexion.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-184">Credentials can be persisted across requests on a connection.</span></span> <span data-ttu-id="b9b5c-185">*Négocier l’authentification ne doit pas être utilisée avec les serveurs proxy, sauf si le serveur proxy gère une affinité de connexion de 1:1 (une connexion persistante) avec Kestrel.*</span><span class="sxs-lookup"><span data-stu-id="b9b5c-185">*Negotiate authentication must not be used with proxies unless the proxy maintains a 1:1 connection affinity (a persistent connection) with Kestrel.*</span></span>

> [!NOTE]
> <span data-ttu-id="b9b5c-186">Le gestionnaire à Negotiate détecte si le serveur sous-jacent prend en charge l’authentification Windows en mode natif et s’il est activé.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-186">The Negotiate handler detects if the underlying server supports Windows Authentication natively and if it's enabled.</span></span> <span data-ttu-id="b9b5c-187">Si le serveur prend en charge l’authentification Windows, mais il est désactivé, une erreur est générée pour vous demander d’activer l’implémentation du serveur.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-187">If the server supports Windows Authentication but it's disabled, an error is thrown asking you to enable the server implementation.</span></span> <span data-ttu-id="b9b5c-188">Lorsque l’authentification Windows est activée sur le serveur, le gestionnaire à Negotiate transfère en toute transparence à celui-ci.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-188">When Windows Authentication is enabled in the server, the Negotiate handler transparently forwards to it.</span></span>

 <span data-ttu-id="b9b5c-189">Ajoutez des services d’authentification en appelant <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (`Microsoft.AspNetCore.Authentication.Negotiate` espace de noms) et `AddNegotitate` (`Microsoft.AspNetCore.Authentication.Negotiate` espace de noms) dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b9b5c-189">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (`Microsoft.AspNetCore.Authentication.Negotiate` namespace) and `AddNegotitate` (`Microsoft.AspNetCore.Authentication.Negotiate` namespace) in `Startup.ConfigureServices`:</span></span>

 ```csharp
services.AddAuthentication(NegotiateDefaults.AuthenticationScheme)
    .AddNegotiate();
```

<span data-ttu-id="b9b5c-190">Ajoutez l’intergiciel d’authentification en appelant <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> dans `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="b9b5c-190">Add Authentication Middleware by calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> in `Startup.Configure`:</span></span>

 ```csharp
app.UseAuthentication();

app.UseMvc();
```

<span data-ttu-id="b9b5c-191">Pour plus d’informations sur l’intergiciel (middleware), consultez <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-191">For more information on middleware, see <xref:fundamentals/middleware/index>.</span></span>

<span data-ttu-id="b9b5c-192">Les demandes anonymes sont autorisés.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-192">Anonymous requests are allowed.</span></span> <span data-ttu-id="b9b5c-193">Utilisez [ASP.NET Core autorisation](xref:security/authorization/introduction) contester les demandes anonymes pour l’authentification.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-193">Use [ASP.NET Core Authorization](xref:security/authorization/introduction) to challenge anonymous requests for authentication.</span></span>

### <a name="windows-environment-configuration"></a><span data-ttu-id="b9b5c-194">Configuration de l’environnement Windows</span><span class="sxs-lookup"><span data-stu-id="b9b5c-194">Windows environment configuration</span></span>

<span data-ttu-id="b9b5c-195">Le [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) composant effectue l’authentification en Mode utilisateur.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-195">The [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) component performs User Mode authentication.</span></span> <span data-ttu-id="b9b5c-196">Noms de principal du service (SPN) doit être ajoutés au compte d’utilisateur exécutant le service, pas le compte d’ordinateur.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-196">Service Principal Names (SPNs) must be added to the user account running the service, not the machine account.</span></span> <span data-ttu-id="b9b5c-197">Exécutez `setspn -S HTTP/mysrevername.mydomain.com myuser` dans un interpréteur de commandes d’administration.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-197">Execute `setspn -S HTTP/mysrevername.mydomain.com myuser` in an administrative command shell.</span></span>

### <a name="linux-and-macos-environment-configuration"></a><span data-ttu-id="b9b5c-198">Configuration de l’environnement Linux et macOS</span><span class="sxs-lookup"><span data-stu-id="b9b5c-198">Linux and macOS environment configuration</span></span>

<span data-ttu-id="b9b5c-199">Instructions pour joindre un ordinateur Linux ou macOS à un domaine Windows sont disponibles dans le [connecter Azure Data Studio à votre serveur SQL à l’aide de l’authentification Windows - Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) article.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-199">Instructions for joining a Linux or macOS machine to a Windows domain are available in the [Connect Azure Data Studio to your SQL Server using Windows authentication - Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) article.</span></span> <span data-ttu-id="b9b5c-200">Les instructions de créer un compte d’ordinateur pour l’ordinateur Linux sur le domaine.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-200">The instructions create a machine account for the Linux machine on the domain.</span></span> <span data-ttu-id="b9b5c-201">Noms principaux de service doit être ajouté à ce compte d’ordinateur.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-201">SPNs must be added to that machine account.</span></span>

> [!NOTE]
> <span data-ttu-id="b9b5c-202">Lorsque vous suivez les instructions de la [connecter Azure Data Studio à votre serveur SQL à l’aide de l’authentification Windows - Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) article, remplacez `python-software-properties` avec `python3-software-properties` si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-202">When following the guidance in the [Connect Azure Data Studio to your SQL Server using Windows authentication - Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) article, replace `python-software-properties` with `python3-software-properties` if needed.</span></span>

<span data-ttu-id="b9b5c-203">Une fois l’ordinateur Linux ou macOS est joint au domaine, les étapes supplémentaires sont nécessaires pour fournir un [fichier keytab](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/) avec les noms SPN :</span><span class="sxs-lookup"><span data-stu-id="b9b5c-203">Once the Linux or macOS machine is joined to the domain, additional steps are required to provide a [keytab file](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/) with the SPNs:</span></span>

* <span data-ttu-id="b9b5c-204">Sur le contrôleur de domaine, ajoutez le nouveau service web SPN pour le compte d’ordinateur :</span><span class="sxs-lookup"><span data-stu-id="b9b5c-204">On the domain controller, add new web service SPNs to the machine account:</span></span>
  * `setspn -S HTTP/mywebservice.mydomain.com mymachine`
  * `setspn -S HTTP/mywebservice@MYDOMAIN.COM mymachine`
* <span data-ttu-id="b9b5c-205">Utilisez [ktpass](/windows-server/administration/windows-commands/ktpass) pour générer un fichier keytab :</span><span class="sxs-lookup"><span data-stu-id="b9b5c-205">Use [ktpass](/windows-server/administration/windows-commands/ktpass) to generate a keytab file:</span></span>
  * `ktpass -princ HTTP/mywebservice.mydomain.com@MYDOMAIN.COM -pass myKeyTabFilePassword -mapuser MYDOMAIN\mymachine$ -pType KRB5_NT_PRINCIPAL -out c:\temp\mymachine.HTTP.keytab -crypto AES256-SHA1`
  * <span data-ttu-id="b9b5c-206">Certains champs doivent être spécifiés en majuscules comme indiqué.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-206">Some fields must be specified in uppercase as indicated.</span></span>
* <span data-ttu-id="b9b5c-207">Copiez le fichier keytab sur l’ordinateur Linux ou macOS.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-207">Copy the keytab file to the Linux or macOS machine.</span></span>
* <span data-ttu-id="b9b5c-208">Sélectionnez le fichier keytab via une variable d’environnement : `export KRB5_KTNAME=/tmp/mymachine.HTTP.keytab`</span><span class="sxs-lookup"><span data-stu-id="b9b5c-208">Select the keytab file via an environment variable: `export KRB5_KTNAME=/tmp/mymachine.HTTP.keytab`</span></span>
* <span data-ttu-id="b9b5c-209">Appeler `klist` pour afficher les noms SPN actuellement disponibles pour utilisation.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-209">Invoke `klist` to show the SPNs currently available for use.</span></span>

> [!NOTE]
> <span data-ttu-id="b9b5c-210">Un fichier keytab contient des informations d’identification d’accès de domaine et doit être protégé en conséquence.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-210">A keytab file contains domain access credentials and must be protected accordingly.</span></span>

::: moniker-end

## <a name="httpsys"></a><span data-ttu-id="b9b5c-211">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="b9b5c-211">HTTP.sys</span></span>

<span data-ttu-id="b9b5c-212">[HTTP.sys](xref:fundamentals/servers/httpsys) prend en charge l’authentification de Windows en Mode noyau à l’aide de Negotiate, NTLM ou l’authentification de base.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-212">[HTTP.sys](xref:fundamentals/servers/httpsys) supports Kernel Mode Windows Authentication using Negotiate, NTLM, or Basic authentication.</span></span>

<span data-ttu-id="b9b5c-213">Ajoutez des services d’authentification en appelant <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> espace de noms) dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b9b5c-213">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> namespace) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

<span data-ttu-id="b9b5c-214">Configurer l’hôte web de l’application pour utiliser HTTP.sys avec l’authentification Windows (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="b9b5c-214">Configure the app's web host to use HTTP.sys with Windows Authentication (*Program.cs*).</span></span> <span data-ttu-id="b9b5c-215"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> est dans le <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> espace de noms.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-215"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> is in the <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> namespace.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_GenericHost.cs?highlight=13-19)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_WebHost.cs?highlight=9-15)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="b9b5c-216">HTTP.sys délègue l’authentification en mode noyau avec le protocole d’authentification Kerberos.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-216">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="b9b5c-217">L’authentification en mode utilisateur n’est pas prise en charge avec Kerberos et HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-217">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="b9b5c-218">Le compte d’ordinateur doit être utilisé pour déchiffrer le ticket/jeton Kerberos obtenu à partir d’Active Directory et transféré par le client au serveur afin d’authentifier l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-218">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="b9b5c-219">Inscrivez le nom de principal du service (SPN) pour l’hôte, et non l’utilisateur de l’application.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-219">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

> [!NOTE]
> <span data-ttu-id="b9b5c-220">HTTP.sys n’est pas pris en charge sur Nano Server version 1709 ou ultérieure.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-220">HTTP.sys isn't supported on Nano Server version 1709 or later.</span></span> <span data-ttu-id="b9b5c-221">Pour utiliser l’authentification Windows et HTTP.sys avec Nano Server, utilisez un [conteneur de Server Core (microsoft/windowsservercore)](https://hub.docker.com/r/microsoft/windowsservercore/).</span><span class="sxs-lookup"><span data-stu-id="b9b5c-221">To use Windows Authentication and HTTP.sys with Nano Server, use a [Server Core (microsoft/windowsservercore) container](https://hub.docker.com/r/microsoft/windowsservercore/).</span></span> <span data-ttu-id="b9b5c-222">Pour plus d’informations sur Server Core, consultez [quel est l’option d’installation Server Core de Windows Server ?](/windows-server/administration/server-core/what-is-server-core).</span><span class="sxs-lookup"><span data-stu-id="b9b5c-222">For more information on Server Core, see [What is the Server Core installation option in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span></span>

## <a name="authorize-users"></a><span data-ttu-id="b9b5c-223">Autoriser les utilisateurs</span><span class="sxs-lookup"><span data-stu-id="b9b5c-223">Authorize users</span></span>

<span data-ttu-id="b9b5c-224">L’état de configuration de l’accès anonyme détermine la façon dont le `[Authorize]` et `[AllowAnonymous]` attributs sont utilisés dans l’application.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-224">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="b9b5c-225">Les deux sections suivantes expliquent comment gérer les États de configuration non autorisés et autorisées de l’accès anonyme.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-225">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="b9b5c-226">Interdire l’accès anonyme</span><span class="sxs-lookup"><span data-stu-id="b9b5c-226">Disallow anonymous access</span></span>

<span data-ttu-id="b9b5c-227">Lorsque l’authentification Windows est activée et que l’accès anonyme est désactivé, le `[Authorize]` et `[AllowAnonymous]` attributs n’ont aucun effet.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-227">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="b9b5c-228">Si un site IIS est configuré pour interdire l’accès anonyme, la demande n’atteint jamais l’application.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-228">If an IIS site is configured to disallow anonymous access, the request never reaches the app.</span></span> <span data-ttu-id="b9b5c-229">Pour cette raison, le `[AllowAnonymous]` attribut n’est pas applicable.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-229">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="b9b5c-230">Autoriser l’accès anonyme</span><span class="sxs-lookup"><span data-stu-id="b9b5c-230">Allow anonymous access</span></span>

<span data-ttu-id="b9b5c-231">Lorsque l’authentification Windows et l’accès anonyme sont activées, utiliser le `[Authorize]` et `[AllowAnonymous]` attributs.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-231">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="b9b5c-232">Le `[Authorize]` attribut vous permet de sécuriser les points de terminaison de l’application qui requièrent une authentification.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-232">The `[Authorize]` attribute allows you to secure endpoints of the app which require authentication.</span></span> <span data-ttu-id="b9b5c-233">Le `[AllowAnonymous]` substitutions d’attribut le `[Authorize]` attribut dans les applications qui autorisent l’accès anonyme.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-233">The `[AllowAnonymous]` attribute overrides the `[Authorize]` attribute in apps that allow anonymous access.</span></span> <span data-ttu-id="b9b5c-234">Pour plus d’informations de l’utilisation d’attribut, consultez <xref:security/authorization/simple>.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-234">For attribute usage details, see <xref:security/authorization/simple>.</span></span>

> [!NOTE]
> <span data-ttu-id="b9b5c-235">Par défaut, les utilisateurs qui ne disposent pas d’autorisation pour accéder à une page sont présentées avec une réponse HTTP 403 vide.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-235">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="b9b5c-236">Le [StatusCodePages Middleware](xref:fundamentals/error-handling#usestatuscodepages) peut être configuré pour fournir aux utilisateurs une meilleure expérience « Accès refusé ».</span><span class="sxs-lookup"><span data-stu-id="b9b5c-236">The [StatusCodePages Middleware](xref:fundamentals/error-handling#usestatuscodepages) can be configured to provide users with a better "Access Denied" experience.</span></span>

## <a name="impersonation"></a><span data-ttu-id="b9b5c-237">emprunt d'identité</span><span class="sxs-lookup"><span data-stu-id="b9b5c-237">Impersonation</span></span>

<span data-ttu-id="b9b5c-238">ASP.NET Core n’implémente pas l’emprunt d’identité.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-238">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="b9b5c-239">Applications s’exécutent avec l’identité d’application pour toutes les demandes, à l’aide d’identité de pool ou les processus de l’application.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-239">Apps run with the app's identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="b9b5c-240">Si l’application doit effectuer une action de la part d’un utilisateur, utilisez [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) dans un [intergiciel (middleware) inline terminal](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-240">If the app should perform an action on behalf of a user, use [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) in a [terminal inline middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) in `Startup.Configure`.</span></span> <span data-ttu-id="b9b5c-241">Exécuter une action unique dans ce contexte, puis fermez le contexte.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-241">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

<span data-ttu-id="b9b5c-242">`RunImpersonated` ne prend pas en charge les opérations asynchrones et ne doit pas être utilisé pour les scénarios complexes.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-242">`RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="b9b5c-243">Par exemple, encapsulant les demandes entières ou des chaînes d’intergiciel (middleware) n’est pas pris en charge ni recommandé.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-243">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b9b5c-244">Bien que le [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) package permet à l’authentification sur Windows, Linux et macOS, l’emprunt d’identité est uniquement pris en charge sur Windows.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-244">While the [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) package enables authentication on Windows, Linux, and macOS, impersonation is only supported on Windows.</span></span>

::: moniker-end

## <a name="claims-transformations"></a><span data-ttu-id="b9b5c-245">Transformations de revendications</span><span class="sxs-lookup"><span data-stu-id="b9b5c-245">Claims transformations</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b9b5c-246">Lors de l’hébergement avec IIS, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> n’est pas appelée en interne pour initialiser un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-246">When hosting with IIS, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="b9b5c-247">Par conséquent, une implémentation de <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> utilisée pour transformer les revendications après chaque authentification n’est pas activée par défaut.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-247">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="b9b5c-248">Pour plus d’informations et un exemple de code qui active les transformations de revendications, consultez <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-248">For more information and a code example that activates claims transformations, see <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="b9b5c-249">Lorsque vous hébergez avec mode in-process IIS, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> n’est pas appelée en interne pour initialiser un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-249">When hosting with IIS in-process mode, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="b9b5c-250">Par conséquent, une implémentation de <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> utilisée pour transformer les revendications après chaque authentification n’est pas activée par défaut.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-250">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="b9b5c-251">Pour plus d’informations et un exemple de code qui active les transformations de revendications lors de l’hébergement intra-processus, consultez <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="b9b5c-251">For more information and a code example that activates claims transformations when hosting in-process, see <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="b9b5c-252">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="b9b5c-252">Additional resources</span></span>

* [<span data-ttu-id="b9b5c-253">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="b9b5c-253">dotnet publish</span></span>](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>
