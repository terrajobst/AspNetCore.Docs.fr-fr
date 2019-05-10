---
title: Fournisseur de Configuration d’Azure Key Vault dans ASP.NET Core
author: guardrex
description: Découvrez comment utiliser le fournisseur de Configuration de coffre de clés Azure pour configurer une application à l’aide de paires nom-valeur chargées pendant l’exécution.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/25/2019
uid: security/key-vault-configuration
ms.openlocfilehash: 45eca05b5eb41815924ca48f60c3b00046c6bdaf
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64894986"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a><span data-ttu-id="2cd81-103">Fournisseur de Configuration d’Azure Key Vault dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2cd81-103">Azure Key Vault Configuration Provider in ASP.NET Core</span></span>

<span data-ttu-id="2cd81-104">Par [Luke Latham](https://github.com/guardrex) et [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="2cd81-104">By [Luke Latham](https://github.com/guardrex) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="2cd81-105">Ce document explique comment utiliser le [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) fournisseur de Configuration à charger des valeurs de configuration d’application de secrets Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="2cd81-105">This document explains how to use the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Configuration Provider to load app configuration values from Azure Key Vault secrets.</span></span> <span data-ttu-id="2cd81-106">Azure Key Vault est un service basé sur le cloud qui aide à protéger les clés de chiffrement et les secrets utilisés par les applications et services.</span><span class="sxs-lookup"><span data-stu-id="2cd81-106">Azure Key Vault is a cloud-based service that assists in safeguarding cryptographic keys and secrets used by apps and services.</span></span> <span data-ttu-id="2cd81-107">Scénarios courants d’utilisation d’Azure Key Vault avec les applications ASP.NET Core sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="2cd81-107">Common scenarios for using Azure Key Vault with ASP.NET Core apps include:</span></span>

* <span data-ttu-id="2cd81-108">Contrôler l’accès aux données de configuration sensibles.</span><span class="sxs-lookup"><span data-stu-id="2cd81-108">Controlling access to sensitive configuration data.</span></span>
* <span data-ttu-id="2cd81-109">Répondre aux exigences pour FIPS 140-2 de niveau 2 validé des Modules de sécurité matériel (HSM) lors du stockage des données de configuration.</span><span class="sxs-lookup"><span data-stu-id="2cd81-109">Meeting the requirement for FIPS 140-2 Level 2 validated Hardware Security Modules (HSM's) when storing configuration data.</span></span>

<span data-ttu-id="2cd81-110">Ce scénario est disponible pour les applications qui ciblent ASP.NET Core 2.1 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="2cd81-110">This scenario is available for apps that target ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="2cd81-111">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2cd81-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="packages"></a><span data-ttu-id="2cd81-112">Packages</span><span class="sxs-lookup"><span data-stu-id="2cd81-112">Packages</span></span>

<span data-ttu-id="2cd81-113">Pour utiliser le fournisseur de Configuration de coffre de clés Azure, ajoutez une référence de package pour le [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) package.</span><span class="sxs-lookup"><span data-stu-id="2cd81-113">To use the Azure Key Vault Configuration Provider, add a package reference to the [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) package.</span></span>

<span data-ttu-id="2cd81-114">À adopter le [gérés d’identités pour les ressources Azure](/azure/active-directory/managed-identities-azure-resources/overview) scénario, ajoutez une référence de package à la [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) package.</span><span class="sxs-lookup"><span data-stu-id="2cd81-114">To adopt the [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) scenario, add a package reference to the [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) package.</span></span>

> [!NOTE]
> <span data-ttu-id="2cd81-115">Au moment de l’écriture, la dernière version stable de `Microsoft.Azure.Services.AppAuthentication`, version `1.0.3`, prend en charge [attribué par le système géré identités](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka).</span><span class="sxs-lookup"><span data-stu-id="2cd81-115">At the time of writing, the latest stable release of `Microsoft.Azure.Services.AppAuthentication`, version `1.0.3`, provides support for [system-assigned managed identities](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka).</span></span> <span data-ttu-id="2cd81-116">Prise en charge de *affectée à l’utilisateur géré identités* est disponible dans le `1.2.0-preview2` package.</span><span class="sxs-lookup"><span data-stu-id="2cd81-116">Support for *user-assigned managed identities* is available in the `1.2.0-preview2` package.</span></span> <span data-ttu-id="2cd81-117">Cette rubrique illustre l’utilisation d’identités gérés par le système, et l’application exemple fourni utilise la version `1.0.3` de la `Microsoft.Azure.Services.AppAuthentication` package.</span><span class="sxs-lookup"><span data-stu-id="2cd81-117">This topic demonstrates the use of system-managed identities, and the provided sample app uses version `1.0.3` of the `Microsoft.Azure.Services.AppAuthentication` package.</span></span>

## <a name="sample-app"></a><span data-ttu-id="2cd81-118">Exemple d’application</span><span class="sxs-lookup"><span data-stu-id="2cd81-118">Sample app</span></span>

<span data-ttu-id="2cd81-119">L’exemple d’application s’exécute dans deux modes déterminé par le `#define` instruction en haut de la *Program.cs* fichier :</span><span class="sxs-lookup"><span data-stu-id="2cd81-119">The sample app runs in either of two modes determined by the `#define` statement at the top of the *Program.cs* file:</span></span>

* <span data-ttu-id="2cd81-120">`Certificate` &ndash; Illustre l’utilisation d’un certificat X.509 et les ID de Client Azure Key Vault à des secrets d’accès stockées dans Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="2cd81-120">`Certificate` &ndash; Demonstrates the use of an Azure Key Vault Client ID and X.509 certificate to access secrets stored in Azure Key Vault.</span></span> <span data-ttu-id="2cd81-121">Cette version de l’exemple peut être exécutée à partir de n’importe quel emplacement, déployé sur Azure App Service ou n’importe quel hôte peut être utilisée par une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2cd81-121">This version of the sample can be run from any location, deployed to Azure App Service or any host capable of serving an ASP.NET Core app.</span></span>
* <span data-ttu-id="2cd81-122">`Managed` &ndash; Montre comment utiliser [gérés d’identités pour les ressources Azure](/azure/active-directory/managed-identities-azure-resources/overview) pour authentifier l’application dans Azure Key Vault avec l’authentification Azure AD sans informations d’identification stockées dans le code ou la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="2cd81-122">`Managed` &ndash; Demonstrates how to use [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) to authenticate the app to Azure Key Vault with Azure AD authentication without credentials stored in the app's code or configuration.</span></span> <span data-ttu-id="2cd81-123">Lorsque vous utilisez des identités gérées pour s’authentifier, un ID d’Application Azure AD et un mot de passe (clé secrète Client) ne sont pas nécessaires.</span><span class="sxs-lookup"><span data-stu-id="2cd81-123">When using managed identities to authenticate, an Azure AD Application ID and Password (Client Secret) aren't required.</span></span> <span data-ttu-id="2cd81-124">Le `Managed` version de l’exemple doit être déployée sur Azure.</span><span class="sxs-lookup"><span data-stu-id="2cd81-124">The `Managed` version of the sample must be deployed to Azure.</span></span> <span data-ttu-id="2cd81-125">Suivez les instructions de la [utiliser les identités gérés pour les ressources Azure](#use-managed-identities-for-azure-resources) section.</span><span class="sxs-lookup"><span data-stu-id="2cd81-125">Follow the guidance in the [Use the Managed identities for Azure resources](#use-managed-identities-for-azure-resources) section.</span></span>

<span data-ttu-id="2cd81-126">Pour plus d’informations sur la façon de configurer un exemple d’application à l’aide de directives de préprocesseur (`#define`), consultez <xref:index#preprocessor-directives-in-sample-code>.</span><span class="sxs-lookup"><span data-stu-id="2cd81-126">For more information on how to configure a sample app using preprocessor directives (`#define`), see <xref:index#preprocessor-directives-in-sample-code>.</span></span>

## <a name="secret-storage-in-the-development-environment"></a><span data-ttu-id="2cd81-127">Stockage secret dans l’environnement de développement</span><span class="sxs-lookup"><span data-stu-id="2cd81-127">Secret storage in the Development environment</span></span>

<span data-ttu-id="2cd81-128">Définir des secrets localement à l’aide de la [outil Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="2cd81-128">Set secrets locally using the [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="2cd81-129">Lors de l’exemple d’application s’exécute sur l’ordinateur local dans l’environnement de développement, les secrets sont chargés à partir du magasin Secret Manager local.</span><span class="sxs-lookup"><span data-stu-id="2cd81-129">When the sample app runs on the local machine in the Development environment, secrets are loaded from the local Secret Manager store.</span></span>

<span data-ttu-id="2cd81-130">L’outil Secret Manager nécessite un `<UserSecretsId>` propriété dans le fichier de projet de l’application.</span><span class="sxs-lookup"><span data-stu-id="2cd81-130">The Secret Manager tool requires a `<UserSecretsId>` property in the app's project file.</span></span> <span data-ttu-id="2cd81-131">Définissez la valeur de propriété (`{GUID}`) à n’importe quel GUID unique :</span><span class="sxs-lookup"><span data-stu-id="2cd81-131">Set the property value (`{GUID}`) to any unique GUID:</span></span>

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

<span data-ttu-id="2cd81-132">Les secrets sont créés en tant que paires nom-valeur.</span><span class="sxs-lookup"><span data-stu-id="2cd81-132">Secrets are created as name-value pairs.</span></span> <span data-ttu-id="2cd81-133">Utilisent des valeurs hiérarchiques (sections de configuration) un `:` (deux-points) comme séparateur dans [configuration d’ASP.NET Core](xref:fundamentals/configuration/index) des noms de clé.</span><span class="sxs-lookup"><span data-stu-id="2cd81-133">Hierarchical values (configuration sections) use a `:` (colon) as a separator in [ASP.NET Core configuration](xref:fundamentals/configuration/index) key names.</span></span>

<span data-ttu-id="2cd81-134">Le Gestionnaire de Secret est utilisé à partir d’une invite de commandes ouverte à la racine du contenu du projet, où `{SECRET NAME}` est le nom et `{SECRET VALUE}` est la valeur :</span><span class="sxs-lookup"><span data-stu-id="2cd81-134">The Secret Manager is used from a command shell opened to the project's content root, where `{SECRET NAME}` is the name and `{SECRET VALUE}` is the value:</span></span>

```console
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

<span data-ttu-id="2cd81-135">Exécutez les commandes suivantes dans une invite de commandes à partir de la racine du contenu du projet pour définir les secrets de l’exemple d’application :</span><span class="sxs-lookup"><span data-stu-id="2cd81-135">Execute the following commands in a command shell from the project's content root to set the secrets for the sample app:</span></span>

```console
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

<span data-ttu-id="2cd81-136">Lorsque ces secrets sont stockés dans Azure Key Vault dans le [stockage Secret dans l’environnement de Production avec Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section, le `_dev` suffixe est remplacé par `_prod`.</span><span class="sxs-lookup"><span data-stu-id="2cd81-136">When these secrets are stored in Azure Key Vault in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section, the `_dev` suffix is changed to `_prod`.</span></span> <span data-ttu-id="2cd81-137">Le suffixe fournit un indice visuel dans la sortie de l’application indiquant la source des valeurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="2cd81-137">The suffix provides a visual cue in the app's output indicating the source of the configuration values.</span></span>

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a><span data-ttu-id="2cd81-138">Stockage secret dans l’environnement de Production avec Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="2cd81-138">Secret storage in the Production environment with Azure Key Vault</span></span>

<span data-ttu-id="2cd81-139">Les instructions fournies par le [Guide de démarrage rapide : Définir et récupérer un secret dans Azure Key Vault à l’aide d’Azure CLI](/azure/key-vault/quick-create-cli) rubrique sont résumées ici pour créer un Azure Key Vault et stocker des secrets utilisés par l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="2cd81-139">The instructions provided by the [Quickstart: Set and retrieve a secret from Azure Key Vault using Azure CLI](/azure/key-vault/quick-create-cli) topic are summarized here for creating an Azure Key Vault and storing secrets used by the sample app.</span></span> <span data-ttu-id="2cd81-140">Reportez-vous à la rubrique pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="2cd81-140">Refer to the topic for further details.</span></span>

1. <span data-ttu-id="2cd81-141">Ouvrir Azure Cloud shell à l’aide de l’une des méthodes suivantes dans le [Azure portal](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="2cd81-141">Open Azure Cloud shell using any one of the following methods in the [Azure portal](https://portal.azure.com/):</span></span>

   * <span data-ttu-id="2cd81-142">Sélectionnez **essayer** dans le coin supérieur droit d’un bloc de code.</span><span class="sxs-lookup"><span data-stu-id="2cd81-142">Select **Try It** in the upper-right corner of a code block.</span></span> <span data-ttu-id="2cd81-143">Utilisez la chaîne de recherche « Azure CLI » dans la zone de texte.</span><span class="sxs-lookup"><span data-stu-id="2cd81-143">Use the search string "Azure CLI" in the text box.</span></span>
   * <span data-ttu-id="2cd81-144">Ouvrez Cloud Shell dans votre navigateur avec la **lancer Cloud Shell** bouton.</span><span class="sxs-lookup"><span data-stu-id="2cd81-144">Open Cloud Shell in your browser with the **Launch Cloud Shell** button.</span></span>
   * <span data-ttu-id="2cd81-145">Sélectionnez le **Cloud Shell** bouton dans le menu dans le coin supérieur droit du portail Azure.</span><span class="sxs-lookup"><span data-stu-id="2cd81-145">Select the **Cloud Shell** button on the menu in the upper-right corner of the Azure portal.</span></span>

   <span data-ttu-id="2cd81-146">Pour plus d’informations, consultez [les Interface de ligne de commande (CLI) Azure](/cli/azure/) et [vue d’ensemble d’Azure Cloud Shell](/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="2cd81-146">For more information, see [Azure Command-Line Interface (CLI)](/cli/azure/) and [Overview of Azure Cloud Shell](/azure/cloud-shell/overview).</span></span>

1. <span data-ttu-id="2cd81-147">Si vous n’êtes pas déjà authentifié, connectez-vous avec le `az login` commande.</span><span class="sxs-lookup"><span data-stu-id="2cd81-147">If you aren't already authenticated, sign in with the `az login` command.</span></span>

1. <span data-ttu-id="2cd81-148">Créer un groupe de ressources avec la commande suivante, où `{RESOURCE GROUP NAME}` correspond au nom du groupe de ressources pour le nouveau groupe de ressources et `{LOCATION}` est la région Azure (centre de données) :</span><span class="sxs-lookup"><span data-stu-id="2cd81-148">Create a resource group with the following command, where `{RESOURCE GROUP NAME}` is the resource group name for the new resource group and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```console
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="2cd81-149">Créer un coffre de clés dans le groupe de ressources avec la commande suivante, où `{KEY VAULT NAME}` est le nom du nouveau coffre de clés et `{LOCATION}` est la région Azure (centre de données) :</span><span class="sxs-lookup"><span data-stu-id="2cd81-149">Create a key vault in the resource group with the following command, where `{KEY VAULT NAME}` is the name for the new key vault and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```console
   az keyvault create --name "{KEY VAULT NAME}" --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="2cd81-150">Créer des clés secrètes dans le coffre de clés en tant que paires nom-valeur.</span><span class="sxs-lookup"><span data-stu-id="2cd81-150">Create secrets in the key vault as name-value pairs.</span></span>

   <span data-ttu-id="2cd81-151">Les noms de secrets Azure Key Vault sont limités à des caractères alphanumériques et des tirets.</span><span class="sxs-lookup"><span data-stu-id="2cd81-151">Azure Key Vault secret names are limited to alphanumeric characters and dashes.</span></span> <span data-ttu-id="2cd81-152">Pour des valeurs hiérarchiques (sections de configuration), utilisez `--` (deux tirets) comme séparateur.</span><span class="sxs-lookup"><span data-stu-id="2cd81-152">Hierarchical values (configuration sections) use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="2cd81-153">Deux-points, qui sont normalement utilisés pour délimiter une section à partir d’une sous-clé dans [configuration d’ASP.NET Core](xref:fundamentals/configuration/index), ne sont pas autorisés dans les noms de clé secrète de coffre de clés.</span><span class="sxs-lookup"><span data-stu-id="2cd81-153">Colons, which are normally used to delimit a section from a subkey in [ASP.NET Core configuration](xref:fundamentals/configuration/index), aren't allowed in key vault secret names.</span></span> <span data-ttu-id="2cd81-154">Par conséquent, les deux tirets sont remplacés par deux points lorsque les clés secrètes sont chargées dans la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="2cd81-154">Therefore, two dashes are used and swapped for a colon when the secrets are loaded into the app's configuration.</span></span>

   <span data-ttu-id="2cd81-155">Les secrets suivantes sont pour une utilisation avec l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="2cd81-155">The following secrets are for use with the sample app.</span></span> <span data-ttu-id="2cd81-156">Les valeurs incluent un `_prod` suffixe pour les distinguer le `_dev` suffixe des valeurs chargées dans l’environnement de développement à partir de Secrets de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="2cd81-156">The values include a `_prod` suffix to distinguish them from the `_dev` suffix values loaded in the Development environment from User Secrets.</span></span> <span data-ttu-id="2cd81-157">Remplacez `{KEY VAULT NAME}` par le nom du coffre de clés que vous avez créé à l’étape précédente :</span><span class="sxs-lookup"><span data-stu-id="2cd81-157">Replace `{KEY VAULT NAME}` with the name of the key vault that you created in the prior step:</span></span>

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-client-secret-for-non-azure-hosted-apps"></a><span data-ttu-id="2cd81-158">Utiliser des ID d’Application et clé secrète Client pour les applications non-Azure-hébergé</span><span class="sxs-lookup"><span data-stu-id="2cd81-158">Use Application ID and Client Secret for non-Azure-hosted apps</span></span>

<span data-ttu-id="2cd81-159">Configurer Azure AD, Azure Key Vault et l’application à utiliser un ID d’Application Azure Active Directory et le X.509 de certificat pour s’authentifier auprès d’un coffre de clés **lorsque l’application est hébergée en dehors d’Azure**.</span><span class="sxs-lookup"><span data-stu-id="2cd81-159">Configure Azure AD, Azure Key Vault, and the app to use an Azure Active Directory Application ID and X.509 certificate to authenticate to a key vault **when the app is hosted outside of Azure**.</span></span> <span data-ttu-id="2cd81-160">Pour plus d’informations, consultez [sur les clés, secrets et certificats](/azure/key-vault/about-keys-secrets-and-certificates).</span><span class="sxs-lookup"><span data-stu-id="2cd81-160">For more information, see [About keys, secrets, and certificates](/azure/key-vault/about-keys-secrets-and-certificates).</span></span>

> [!NOTE]
> <span data-ttu-id="2cd81-161">À l’aide d’un certificat X.509 et les ID d’Application est pris en charge pour les applications hébergées dans Azure, nous vous recommandons d’utiliser [gérés d’identités pour les ressources Azure](#use-managed-identities-for-azure-resources) lors de l’hébergement d’une application dans Azure.</span><span class="sxs-lookup"><span data-stu-id="2cd81-161">Although using an Application ID and X.509 certificate is supported for apps hosted in Azure, we recommend using [Managed identities for Azure resources](#use-managed-identities-for-azure-resources) when hosting an app in Azure.</span></span> <span data-ttu-id="2cd81-162">Les identités ne nécessitent pas le stockage d’un certificat dans l’application ou dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="2cd81-162">Managed identities don't require storing a certificate in the app or in the development environment.</span></span>

<span data-ttu-id="2cd81-163">L’exemple d’application utilise un ID d’Application et de certificat X.509 quand le `#define` instruction en haut de la *Program.cs* fichier est défini sur `Certificate`.</span><span class="sxs-lookup"><span data-stu-id="2cd81-163">The sample app uses an Application ID and X.509 certificate when the `#define` statement at the top of the *Program.cs* file is set to `Certificate`.</span></span>

1. <span data-ttu-id="2cd81-164">Inscrire l’application avec Azure AD (**inscriptions**).</span><span class="sxs-lookup"><span data-stu-id="2cd81-164">Register the app with Azure AD (**App registrations**).</span></span>
1. <span data-ttu-id="2cd81-165">Télécharger la clé publique :</span><span class="sxs-lookup"><span data-stu-id="2cd81-165">Upload the public key:</span></span>
   1. <span data-ttu-id="2cd81-166">Sélectionnez l’application dans Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2cd81-166">Select the app in Azure AD.</span></span>
   1. <span data-ttu-id="2cd81-167">Accédez à **paramètres** > **clés**.</span><span class="sxs-lookup"><span data-stu-id="2cd81-167">Navigate to **Settings** > **Keys**.</span></span>
   1. <span data-ttu-id="2cd81-168">Sélectionnez **charger la clé publique** pour charger le certificat, qui contient la clé publique.</span><span class="sxs-lookup"><span data-stu-id="2cd81-168">Select **Upload Public Key** to upload the certificate, which contains the public key.</span></span> <span data-ttu-id="2cd81-169">En plus d’utiliser un *.cer*, *.pem*, ou *.crt* certificat, un *.pfx* certificat peut être chargé.</span><span class="sxs-lookup"><span data-stu-id="2cd81-169">In addition to using a *.cer*, *.pem*, or *.crt* certificate, a *.pfx* certificate can be uploaded.</span></span>
1. <span data-ttu-id="2cd81-170">Store le nom de coffre de clés et l’ID de l’Application dans l’application *appsettings.json* fichier.</span><span class="sxs-lookup"><span data-stu-id="2cd81-170">Store the key vault name and Application ID in the app's *appsettings.json* file.</span></span> <span data-ttu-id="2cd81-171">Placer le certificat à la racine de l’application ou dans le magasin de certificats de l’application&dagger;.</span><span class="sxs-lookup"><span data-stu-id="2cd81-171">Place the certificate at the root of the app or in the app's certificate store&dagger;.</span></span>
1. <span data-ttu-id="2cd81-172">Accédez à **coffres de clés** dans le portail Azure.</span><span class="sxs-lookup"><span data-stu-id="2cd81-172">Navigate to **Key vaults** in the Azure portal.</span></span>
1. <span data-ttu-id="2cd81-173">Sélectionnez le coffre de clés que vous avez créé dans le [stockage Secret dans l’environnement de Production avec Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section.</span><span class="sxs-lookup"><span data-stu-id="2cd81-173">Select the key vault that you created in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section.</span></span>
1. <span data-ttu-id="2cd81-174">Sélectionnez **stratégies d’accès**.</span><span class="sxs-lookup"><span data-stu-id="2cd81-174">Select **Access policies**.</span></span>
1. <span data-ttu-id="2cd81-175">Sélectionnez **Ajouter nouveau**.</span><span class="sxs-lookup"><span data-stu-id="2cd81-175">Select **Add new**.</span></span>
1. <span data-ttu-id="2cd81-176">Sélectionnez **sélectionner le principal** et sélectionnez l’application inscrite par nom.</span><span class="sxs-lookup"><span data-stu-id="2cd81-176">Select **Select principal** and select the registered app by name.</span></span> <span data-ttu-id="2cd81-177">Sélectionnez le **sélectionnez** bouton.</span><span class="sxs-lookup"><span data-stu-id="2cd81-177">Select the **Select** button.</span></span>
1. <span data-ttu-id="2cd81-178">Ouvrez **autorisations du Secret** et fournir l’application avec **obtenir** et **liste** autorisations.</span><span class="sxs-lookup"><span data-stu-id="2cd81-178">Open **Secret permissions** and provide the app with **Get** and **List** permissions.</span></span>
1. <span data-ttu-id="2cd81-179">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="2cd81-179">Select **OK**.</span></span>
1. <span data-ttu-id="2cd81-180">Sélectionnez **Enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="2cd81-180">Select **Save**.</span></span>
1. <span data-ttu-id="2cd81-181">Déployer l’application.</span><span class="sxs-lookup"><span data-stu-id="2cd81-181">Deploy the app.</span></span>

<span data-ttu-id="2cd81-182">&dagger;Dans l’exemple d’application, le certificat est directement consommé depuis le fichier de certificat physique à la racine de l’application en créant un nouveau `X509Certificate2` lors de l’appel `AddAzureKeyVault`.</span><span class="sxs-lookup"><span data-stu-id="2cd81-182">&dagger;In the sample app, the certificate is consumed directly from the physical certificate file in the root of the app by creating a new `X509Certificate2` when calling `AddAzureKeyVault`.</span></span> <span data-ttu-id="2cd81-183">Une autre approche consiste à autoriser le système d’exploitation gérer le certificat.</span><span class="sxs-lookup"><span data-stu-id="2cd81-183">An alternative approach is to allow the OS to manage the certificate.</span></span> <span data-ttu-id="2cd81-184">Pour plus d’informations, consultez le [autoriser le système d’exploitation gérer le certificat X.509](#allow-the-os-to-manage-the-x509-certificate) section.</span><span class="sxs-lookup"><span data-stu-id="2cd81-184">For more information, see the [Allow the OS to manage the X.509 certificate](#allow-the-os-to-manage-the-x509-certificate) section.</span></span>

<span data-ttu-id="2cd81-185">Le `Certificate` exemple d’application obtient ses valeurs de configuration à partir de `IConfigurationRoot` avec le même nom que le nom de secret :</span><span class="sxs-lookup"><span data-stu-id="2cd81-185">The `Certificate` sample app obtains its configuration values from `IConfigurationRoot` with the same name as the secret name:</span></span>

* <span data-ttu-id="2cd81-186">Valeurs non hiérarchiques : La valeur de `SecretName` est obtenu avec `config["SecretName"]`.</span><span class="sxs-lookup"><span data-stu-id="2cd81-186">Non-hierarchical values: The value for `SecretName` is obtained with `config["SecretName"]`.</span></span>
* <span data-ttu-id="2cd81-187">Valeurs hiérarchiques (sections) : Utilisez `:` la notation (deux-points) ou le `GetSection` méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="2cd81-187">Hierarchical values (sections): Use `:` (colon) notation or the `GetSection` extension method.</span></span> <span data-ttu-id="2cd81-188">Utilisez une des ces approches pour obtenir la valeur de configuration, :</span><span class="sxs-lookup"><span data-stu-id="2cd81-188">Use either of these approaches to obtain the configuration value:</span></span>
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

<span data-ttu-id="2cd81-189">Les appels de l’application `AddAzureKeyVault` avec les valeurs fournies par le *appsettings.json* fichier :</span><span class="sxs-lookup"><span data-stu-id="2cd81-189">The app calls `AddAzureKeyVault` with values supplied by the *appsettings.json* file:</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet1&highlight=12-15)]

<span data-ttu-id="2cd81-190">Exemples de valeurs :</span><span class="sxs-lookup"><span data-stu-id="2cd81-190">Example values:</span></span>

* <span data-ttu-id="2cd81-191">Nom du coffre de clés : `contosovault`</span><span class="sxs-lookup"><span data-stu-id="2cd81-191">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="2cd81-192">ID d’application : `627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="2cd81-192">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>

<span data-ttu-id="2cd81-193">*appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="2cd81-193">*appsettings.json*:</span></span>

[!code-json[](key-vault-configuration/sample/appsettings.json)]

<span data-ttu-id="2cd81-194">Lorsque vous exécutez l’application, une page Web affiche les valeurs de secret chargés.</span><span class="sxs-lookup"><span data-stu-id="2cd81-194">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="2cd81-195">Dans l’environnement de développement, chargement des valeurs secrètes avec le `_dev` suffixe.</span><span class="sxs-lookup"><span data-stu-id="2cd81-195">In the Development environment, secret values load with the `_dev` suffix.</span></span> <span data-ttu-id="2cd81-196">Dans l’environnement de Production, les valeurs de charge avec le `_prod` suffixe.</span><span class="sxs-lookup"><span data-stu-id="2cd81-196">In the Production environment, the values load with the `_prod` suffix.</span></span>

## <a name="use-managed-identities-for-azure-resources"></a><span data-ttu-id="2cd81-197">Utiliser des identités gérés pour les ressources Azure</span><span class="sxs-lookup"><span data-stu-id="2cd81-197">Use Managed identities for Azure resources</span></span>

<span data-ttu-id="2cd81-198">**Une application déployée sur Azure** peuvent tirer parti de [gérés d’identités pour les ressources Azure](/azure/active-directory/managed-identities-azure-resources/overview), ce qui permet à l’application auprès d’Azure Key Vault à l’aide de l’authentification Azure AD sans informations d’identification (ID d’Application et Clé secrète Password/Client) stockées dans l’application.</span><span class="sxs-lookup"><span data-stu-id="2cd81-198">**An app deployed to Azure** can take advantage of [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview), which allows the app to authenticate with Azure Key Vault using Azure AD authentication without credentials (Application ID and Password/Client Secret) stored in the app.</span></span>

<span data-ttu-id="2cd81-199">L’exemple d’application utilise des identités de géré pour les ressources Azure lorsque le `#define` instruction en haut de la *Program.cs* fichier est défini sur `Managed`.</span><span class="sxs-lookup"><span data-stu-id="2cd81-199">The sample app uses Managed identities for Azure resources when the `#define` statement at the top of the *Program.cs* file is set to `Managed`.</span></span>

<span data-ttu-id="2cd81-200">Entrez le nom du coffre dans l’application *appsettings.json* fichier.</span><span class="sxs-lookup"><span data-stu-id="2cd81-200">Enter the vault name into the app's *appsettings.json* file.</span></span> <span data-ttu-id="2cd81-201">L’exemple d’application ne nécessite pas un ID d’Application et le mot de passe (clé secrète Client) lorsque la valeur la `Managed` version, vous pouvez donc ignorer ces entrées de configuration.</span><span class="sxs-lookup"><span data-stu-id="2cd81-201">The sample app doesn't require an Application ID and Password (Client Secret) when set to the `Managed` version, so you can ignore those configuration entries.</span></span> <span data-ttu-id="2cd81-202">L’application est déployée sur Azure et Azure authentifie l’application pour accéder à Azure Key Vault uniquement en utilisant le nom de coffre stockée dans le *appsettings.json* fichier.</span><span class="sxs-lookup"><span data-stu-id="2cd81-202">The app is deployed to Azure, and Azure authenticates the app to access Azure Key Vault only using the vault name stored in the *appsettings.json* file.</span></span>

<span data-ttu-id="2cd81-203">Déployer l’exemple d’application dans Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="2cd81-203">Deploy the sample app to Azure App Service.</span></span>

<span data-ttu-id="2cd81-204">Une application déployée sur Azure App Service est automatiquement inscrite auprès d’Azure AD lorsque le service est créé.</span><span class="sxs-lookup"><span data-stu-id="2cd81-204">An app deployed to Azure App Service is automatically registered with Azure AD when the service is created.</span></span> <span data-ttu-id="2cd81-205">Obtenir l’ID d’objet du déploiement pour une utilisation dans la commande suivante.</span><span class="sxs-lookup"><span data-stu-id="2cd81-205">Obtain the Object ID from the deployment for use in the following command.</span></span> <span data-ttu-id="2cd81-206">L’ID d’objet est affiché dans le portail Azure sur le **identité** Panneau de configuration du Service d’application.</span><span class="sxs-lookup"><span data-stu-id="2cd81-206">The Object ID is shown in the Azure portal on the **Identity** panel of the App Service.</span></span>

<span data-ttu-id="2cd81-207">Fournir à l’interface CLI Azure et l’ID d’objet de l’application, l’application avec `list` et `get` autorisations d’accès au coffre de clés :</span><span class="sxs-lookup"><span data-stu-id="2cd81-207">Using Azure CLI and the app's Object ID, provide the app with `list` and `get` permissions to access the key vault:</span></span>

```console
az keyvault set-policy --name '{KEY VAULT NAME}' --object-id {OBJECT ID} --secret-permissions get list
```

<span data-ttu-id="2cd81-208">**Redémarrez l’application** à l’aide d’Azure CLI, PowerShell ou le portail Azure.</span><span class="sxs-lookup"><span data-stu-id="2cd81-208">**Restart the app** using Azure CLI, PowerShell, or the Azure portal.</span></span>

<span data-ttu-id="2cd81-209">L’exemple d’application :</span><span class="sxs-lookup"><span data-stu-id="2cd81-209">The sample app:</span></span>

* <span data-ttu-id="2cd81-210">Crée une instance de la `AzureServiceTokenProvider` classe sans une chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="2cd81-210">Creates an instance of the `AzureServiceTokenProvider` class without a connection string.</span></span> <span data-ttu-id="2cd81-211">Lorsqu’une chaîne de connexion n’est pas fournie, le fournisseur tente d’obtenir un jeton d’accès d’identités gérés pour les ressources Azure.</span><span class="sxs-lookup"><span data-stu-id="2cd81-211">When a connection string isn't provided, the provider attempts to obtain an access token from Managed identities for Azure resources.</span></span>
* <span data-ttu-id="2cd81-212">Un nouveau `KeyVaultClient` est créé avec le `AzureServiceTokenProvider` rappel jeton d’instance.</span><span class="sxs-lookup"><span data-stu-id="2cd81-212">A new `KeyVaultClient` is created with the `AzureServiceTokenProvider` instance token callback.</span></span>
* <span data-ttu-id="2cd81-213">Le `KeyVaultClient` instance est utilisée avec une implémentation par défaut de `IKeyVaultSecretManager` qui charge toutes les valeurs secrètes et remplace des tirets double (`--`) avec des deux-points (`:`) dans les noms de clé.</span><span class="sxs-lookup"><span data-stu-id="2cd81-213">The `KeyVaultClient` instance is used with a default implementation of `IKeyVaultSecretManager` that loads all secret values and replaces double-dashes (`--`) with colons (`:`) in key names.</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet2&highlight=13-21)]

<span data-ttu-id="2cd81-214">Lorsque vous exécutez l’application, une page Web affiche les valeurs de secret chargés.</span><span class="sxs-lookup"><span data-stu-id="2cd81-214">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="2cd81-215">Dans l’environnement de développement, les valeurs secrètes ont la `_dev` suffixe, car elles sont proposées par les Secrets de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="2cd81-215">In the Development environment, secret values have the `_dev` suffix because they're provided by User Secrets.</span></span> <span data-ttu-id="2cd81-216">Dans l’environnement de Production, les valeurs de charge avec le `_prod` suffixe, car elles sont proposées par Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="2cd81-216">In the Production environment, the values load with the `_prod` suffix because they're provided by Azure Key Vault.</span></span>

<span data-ttu-id="2cd81-217">Si vous recevez un `Access denied` erreur, vérifiez que l’application est inscrite auprès d’Azure AD et d’accéder au coffre de clés.</span><span class="sxs-lookup"><span data-stu-id="2cd81-217">If you receive an `Access denied` error, confirm that the app is registered with Azure AD and provided access to the key vault.</span></span> <span data-ttu-id="2cd81-218">Vérifiez que vous avez redémarré le service dans Azure.</span><span class="sxs-lookup"><span data-stu-id="2cd81-218">Confirm that you've restarted the service in Azure.</span></span>

## <a name="use-a-key-name-prefix"></a><span data-ttu-id="2cd81-219">Utiliser un préfixe de nom de la clé</span><span class="sxs-lookup"><span data-stu-id="2cd81-219">Use a key name prefix</span></span>

<span data-ttu-id="2cd81-220">`AddAzureKeyVault` Fournit une surcharge qui accepte une implémentation de `IKeyVaultSecretManager`, qui vous permet de contrôler comment les secrets de coffre sont converties en clés de configuration.</span><span class="sxs-lookup"><span data-stu-id="2cd81-220">`AddAzureKeyVault` provides an overload that accepts an implementation of `IKeyVaultSecretManager`, which allows you to control how key vault secrets are converted into configuration keys.</span></span> <span data-ttu-id="2cd81-221">Par exemple, vous pouvez implémenter l’interface pour charger les valeurs des secrets selon une valeur de préfixe que vous indiquez au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="2cd81-221">For example, you can implement the interface to load secret values based on a prefix value you provide at app startup.</span></span> <span data-ttu-id="2cd81-222">Cela vous permet, par exemple, de charger des secrets en fonction de la version de l’application.</span><span class="sxs-lookup"><span data-stu-id="2cd81-222">This allows you, for example, to load secrets based on the version of the app.</span></span>

> [!WARNING]
> <span data-ttu-id="2cd81-223">N’utilisez pas de préfixes sur les secrets de coffre de clés pour placer les secrets de plusieurs applications dans le même coffre de clés ou pour placer des secrets d'environnement (par exemple, des secrets de *développement* / de *production*) dans le même coffre.</span><span class="sxs-lookup"><span data-stu-id="2cd81-223">Don't use prefixes on key vault secrets to place secrets for multiple apps into the same key vault or to place environmental secrets (for example, *development* versus *production* secrets) into the same vault.</span></span> <span data-ttu-id="2cd81-224">Nous recommandons d'utiliser un coffre de clés distinct pour chaque application et chaque environnement de développement/production afin d’isoler les environnements d’application et ainsi de garantir le niveau de sécurité le plus élevé possible.</span><span class="sxs-lookup"><span data-stu-id="2cd81-224">We recommend that different apps and development/production environments use separate key vaults to isolate app environments for the highest level of security.</span></span>

<span data-ttu-id="2cd81-225">Dans l’exemple suivant, une clé secrète est établie dans la clé de coffre (et pour l’environnement de développement à l’aide de l’outil Secret Manager) pour `5000-AppSecret` (périodes ne sont pas autorisés dans les noms de clé secrète de coffre de clés).</span><span class="sxs-lookup"><span data-stu-id="2cd81-225">In the following example, a secret is established in the key vault (and using the Secret Manager tool for the Development environment) for `5000-AppSecret` (periods aren't allowed in key vault secret names).</span></span> <span data-ttu-id="2cd81-226">Ce secret représente une clé secrète d’application pour la version 5.0.0.0 de l’application.</span><span class="sxs-lookup"><span data-stu-id="2cd81-226">This secret represents an app secret for version 5.0.0.0 of the app.</span></span> <span data-ttu-id="2cd81-227">Pour une autre version de l’application, 5.1.0.0, un secret est ajouté à la clé de coffre (et à l’aide de l’outil Secret Manager) pour `5100-AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="2cd81-227">For another version of the app, 5.1.0.0, a secret is added to the key vault (and using the Secret Manager tool) for `5100-AppSecret`.</span></span> <span data-ttu-id="2cd81-228">Chaque version de l’application charge sa valeur secrète avec version dans sa configuration en tant que `AppSecret`, suppression dérivées de la version il charge le secret.</span><span class="sxs-lookup"><span data-stu-id="2cd81-228">Each app version loads its versioned secret value into its configuration as `AppSecret`, stripping off the version as it loads the secret.</span></span>

<span data-ttu-id="2cd81-229">`AddAzureKeyVault` est appelée avec un personnalisé `IKeyVaultSecretManager`:</span><span class="sxs-lookup"><span data-stu-id="2cd81-229">`AddAzureKeyVault` is called with a custom `IKeyVaultSecretManager`:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Program.cs?name=snippet1&highlight=22)]

<span data-ttu-id="2cd81-230">Les valeurs de nom de coffre de clés, ID d’Application et le mot de passe (clé secrète Client) sont fournies par le *appsettings.json* fichier :</span><span class="sxs-lookup"><span data-stu-id="2cd81-230">The values for key vault name, Application ID, and Password (Client Secret) are provided by the *appsettings.json* file:</span></span>

[!code-json[](key-vault-configuration/sample/appsettings.json)]

<span data-ttu-id="2cd81-231">Exemples de valeurs :</span><span class="sxs-lookup"><span data-stu-id="2cd81-231">Example values:</span></span>

* <span data-ttu-id="2cd81-232">Nom du coffre de clés : `contosovault`</span><span class="sxs-lookup"><span data-stu-id="2cd81-232">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="2cd81-233">ID d’application : `627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="2cd81-233">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="2cd81-234">Mot de passe : `g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=`</span><span class="sxs-lookup"><span data-stu-id="2cd81-234">Password: `g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=`</span></span>

<span data-ttu-id="2cd81-235">Le `IKeyVaultSecretManager` implémentation réagit aux préfixes de version de secrets pour charger la clé secrète appropriée dans configuration :</span><span class="sxs-lookup"><span data-stu-id="2cd81-235">The `IKeyVaultSecretManager` implementation reacts to the version prefixes of secrets to load the proper secret into configuration:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Startup.cs?name=snippet1)]

<span data-ttu-id="2cd81-236">La méthode `Load` est appelée par un algorithme de fourniture qui effectue une itération dans les secrets du coffre pour trouver ceux qui comportent le préfixe de la version.</span><span class="sxs-lookup"><span data-stu-id="2cd81-236">The `Load` method is called by a provider algorithm that iterates through the vault secrets to find the ones that have the version prefix.</span></span> <span data-ttu-id="2cd81-237">Quand un préfixe de version a été trouvé avec la méthode `Load`, l’algorithme utilise la méthode `GetKey` pour retourner le nom de configuration du nom du secret.</span><span class="sxs-lookup"><span data-stu-id="2cd81-237">When a version prefix is found with `Load`, the algorithm uses the `GetKey` method to return the configuration name of the secret name.</span></span> <span data-ttu-id="2cd81-238">Il supprime le préfixe de version du nom du secret et retourne le reste du nom du secret pour le charger dans les paires nom-valeur de configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="2cd81-238">It strips off the version prefix from the secret's name and returns the rest of the secret name for loading into the app's configuration name-value pairs.</span></span>

<span data-ttu-id="2cd81-239">Lorsque cette approche est mise en œuvre :</span><span class="sxs-lookup"><span data-stu-id="2cd81-239">When this approach is implemented:</span></span>

1. <span data-ttu-id="2cd81-240">Version de l’application spécifiée dans le fichier de projet de l’application.</span><span class="sxs-lookup"><span data-stu-id="2cd81-240">The app's version specified in the app's project file.</span></span> <span data-ttu-id="2cd81-241">Dans l’exemple suivant, la version est définie sur `5.0.0.0`:</span><span class="sxs-lookup"><span data-stu-id="2cd81-241">In the following example, the app's version is set to `5.0.0.0`:</span></span>

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. <span data-ttu-id="2cd81-242">Vérifiez qu’un `<UserSecretsId>` propriété n’est présente dans le fichier de projet application, où `{GUID}` est un GUID fourni par l’utilisateur :</span><span class="sxs-lookup"><span data-stu-id="2cd81-242">Confirm that a `<UserSecretsId>` property is present in the app's project file, where `{GUID}` is a user-supplied GUID:</span></span>

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   <span data-ttu-id="2cd81-243">Enregistrer les secrets suivants localement avec le [outil Secret Manager](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="2cd81-243">Save the following secrets locally with the [Secret Manager tool](xref:security/app-secrets):</span></span>

   ```console
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. <span data-ttu-id="2cd81-244">Secrets sont enregistrés dans Azure Key Vault en utilisant les commandes Azure CLI suivantes :</span><span class="sxs-lookup"><span data-stu-id="2cd81-244">Secrets are saved in Azure Key Vault using the following Azure CLI commands:</span></span>

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. <span data-ttu-id="2cd81-245">Quand l’application est exécutée, les secrets de coffre de clés sont chargés.</span><span class="sxs-lookup"><span data-stu-id="2cd81-245">When the app is run, the key vault secrets are loaded.</span></span> <span data-ttu-id="2cd81-246">Le secret de chaîne pour `5000-AppSecret` est mis en correspondance avec la version spécifiée dans le fichier de projet application (`5.0.0.0`).</span><span class="sxs-lookup"><span data-stu-id="2cd81-246">The string secret for `5000-AppSecret` is matched to the app's version specified in the app's project file (`5.0.0.0`).</span></span>

1. <span data-ttu-id="2cd81-247">La version, `5000` (avec le tiret), est supprimé du nom de clé.</span><span class="sxs-lookup"><span data-stu-id="2cd81-247">The version, `5000` (with the dash), is stripped from the key name.</span></span> <span data-ttu-id="2cd81-248">Tout au long de l’application, la lecture de la configuration avec la clé `AppSecret` se charge de la valeur du secret.</span><span class="sxs-lookup"><span data-stu-id="2cd81-248">Throughout the app, reading configuration with the key `AppSecret` loads the secret value.</span></span>

1. <span data-ttu-id="2cd81-249">Si la version de l’application est modifiée dans le fichier projet pour `5.1.0.0` et l’application est exécutée à nouveau, la valeur secrète retournée est `5.1.0.0_secret_value_dev` dans l’environnement de développement et `5.1.0.0_secret_value_prod` en Production.</span><span class="sxs-lookup"><span data-stu-id="2cd81-249">If the app's version is changed in the project file to `5.1.0.0` and the app is run again, the secret value returned is `5.1.0.0_secret_value_dev` in the Development environment and `5.1.0.0_secret_value_prod` in Production.</span></span>

> [!NOTE]
> <span data-ttu-id="2cd81-250">Vous pouvez également fournir votre propre implémentation `KeyVaultClient` à `AddAzureKeyVault`.</span><span class="sxs-lookup"><span data-stu-id="2cd81-250">You can also provide your own `KeyVaultClient` implementation to `AddAzureKeyVault`.</span></span> <span data-ttu-id="2cd81-251">Un client personnalisé permet le partage d’une instance unique du client sur l’application.</span><span class="sxs-lookup"><span data-stu-id="2cd81-251">A custom client permits sharing a single instance of the client across the app.</span></span>

## <a name="allow-the-os-to-manage-the-x509-certificate"></a><span data-ttu-id="2cd81-252">Autoriser le système d’exploitation gérer le certificat X.509</span><span class="sxs-lookup"><span data-stu-id="2cd81-252">Allow the OS to manage the X.509 certificate</span></span>

<span data-ttu-id="2cd81-253">Le certificat X.509 peut être géré par le système d’exploitation.</span><span class="sxs-lookup"><span data-stu-id="2cd81-253">The X.509 certificate can be managed by the OS.</span></span> <span data-ttu-id="2cd81-254">L’exemple suivant utilise le `AddAzureKeyVault` surcharge qui accepte un `X509Certificate2` à partir d’un empreinte numérique du certificat fourni par la configuration et de magasin de certificats utilisateur actuel de la machine :</span><span class="sxs-lookup"><span data-stu-id="2cd81-254">The following example uses the `AddAzureKeyVault` overload that accepts an `X509Certificate2` from the machine's current user certificate store and a certificate thumbprint supplied by configuration:</span></span>

```csharp
// using System.Linq;
// using System.Security.Cryptography.X509Certificates;
// using Microsoft.Extensions.Configuration;

WebHost.CreateDefaultBuilder(args)
    .ConfigureAppConfiguration((context, config) =>
    {
        if (context.HostingEnvironment.IsProduction())
        {
            var builtConfig = config.Build();

            using (var store = new X509Store(StoreName.My, 
                StoreLocation.CurrentUser))
            {
                store.Open(OpenFlags.ReadOnly);
                var certs = store.Certificates
                    .Find(X509FindType.FindByThumbprint, 
                        builtConfig["CertificateThumbprint"], false);

                config.AddAzureKeyVault(
                    builtConfig["KeyVaultName"], 
                    builtConfig["AzureADApplicationId"], 
                    certs.OfType<X509Certificate2>().Single());

                store.Close();
            }
        }
    })
    .UseStartup<Startup>();
```

<span data-ttu-id="2cd81-255">Pour plus d’informations, consultez [authentifier avec un certificat au lieu d’une clé secrète Client](/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret).</span><span class="sxs-lookup"><span data-stu-id="2cd81-255">For more information, see [Authenticate with a Certificate instead of a Client Secret](/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret).</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="2cd81-256">Lier un tableau à une classe</span><span class="sxs-lookup"><span data-stu-id="2cd81-256">Bind an array to a class</span></span>

<span data-ttu-id="2cd81-257">Le fournisseur est capable de lire les valeurs de configuration dans un tableau pour la liaison à un tableau POCO.</span><span class="sxs-lookup"><span data-stu-id="2cd81-257">The provider is capable of reading configuration values into an array for binding to a POCO array.</span></span>

<span data-ttu-id="2cd81-258">Lors de la lecture à partir d’une source de configuration qui permet des clés contenir le signe deux-points (`:`) séparateurs, un segment de clé numérique est utilisé pour distinguer les clés qui composent un tableau (`:0:`, `:1:`,...</span><span class="sxs-lookup"><span data-stu-id="2cd81-258">When reading from a configuration source that allows keys to contain colon (`:`) separators, a numeric key segment is used to distinguish the keys that make up an array (`:0:`, `:1:`, …</span></span> <span data-ttu-id="2cd81-259">`:{n}:`).</span><span class="sxs-lookup"><span data-stu-id="2cd81-259">`:{n}:`).</span></span> <span data-ttu-id="2cd81-260">Pour plus d’informations, consultez [Configuration : Lier un tableau à une classe](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="2cd81-260">For more information, see [Configuration: Bind an array to a class](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span></span>

<span data-ttu-id="2cd81-261">Clés d’Azure Key Vault ne peut pas utiliser un signe deux-points comme séparateur.</span><span class="sxs-lookup"><span data-stu-id="2cd81-261">Azure Key Vault keys can't use a colon as a separator.</span></span> <span data-ttu-id="2cd81-262">L’approche décrite dans cette rubrique utilise les doubles tirets (`--`) comme séparateur pour les valeurs hiérarchiques (sections).</span><span class="sxs-lookup"><span data-stu-id="2cd81-262">The approach described in this topic uses double dashes (`--`) as a separator for hierarchical values (sections).</span></span> <span data-ttu-id="2cd81-263">Clés de tableau sont stockées dans Azure Key Vault avec double des tirets et des segments de clé numériques (`--0--`, `--1--`, &hellip; `--{n}--`).</span><span class="sxs-lookup"><span data-stu-id="2cd81-263">Array keys are stored in Azure Key Vault with double dashes and numeric key segments (`--0--`, `--1--`, &hellip; `--{n}--`).</span></span>

<span data-ttu-id="2cd81-264">Examinez les éléments suivants [Serilog](https://serilog.net/) configuration du fournisseur fournie par un fichier JSON de journalisation.</span><span class="sxs-lookup"><span data-stu-id="2cd81-264">Examine the following [Serilog](https://serilog.net/) logging provider configuration provided by a JSON file.</span></span> <span data-ttu-id="2cd81-265">Il existe deux objets littéraux définis dans le `WriteTo` tableau qui reflètent les deux Serilog *récepteurs*, qui décrivent des destinations pour la sortie de journalisation :</span><span class="sxs-lookup"><span data-stu-id="2cd81-265">There are two object literals defined in the `WriteTo` array that reflect two Serilog *sinks*, which describe destinations for logging output:</span></span>

```json
"Serilog": {
  "WriteTo": [
    {
      "Name": "AzureTableStorage",
      "Args": {
        "storageTableName": "logs",
        "connectionString": "DefaultEnd...ountKey=Eby8...GMGw=="
      }
    },
    {
      "Name": "AzureDocumentDB",
      "Args": {
        "endpointUrl": "https://contoso.documents.azure.com:443",
        "authorizationKey": "Eby8...GMGw=="
      }
    }
  ]
}
```

<span data-ttu-id="2cd81-266">La configuration représentée dans le fichier JSON précédent est stockée dans Azure Key Vault à l’aide de double tiret (`--`) segments numériques et de notation :</span><span class="sxs-lookup"><span data-stu-id="2cd81-266">The configuration shown in the preceding JSON file is stored in Azure Key Vault using double dash (`--`) notation and numeric segments:</span></span>

| <span data-ttu-id="2cd81-267">Touche</span><span class="sxs-lookup"><span data-stu-id="2cd81-267">Key</span></span> | <span data-ttu-id="2cd81-268">Value</span><span class="sxs-lookup"><span data-stu-id="2cd81-268">Value</span></span> |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a><span data-ttu-id="2cd81-269">Recharger les secrets</span><span class="sxs-lookup"><span data-stu-id="2cd81-269">Reload secrets</span></span>

<span data-ttu-id="2cd81-270">Les secrets sont mis en cache jusqu'à ce que la méthode `IConfigurationRoot.Reload()` soit appelée.</span><span class="sxs-lookup"><span data-stu-id="2cd81-270">Secrets are cached until `IConfigurationRoot.Reload()` is called.</span></span> <span data-ttu-id="2cd81-271">Arrivé à expiration, désactivé, et les secrets mis à jour dans le coffre de clés ne sont pas respectées par l’application jusqu'à ce que `Reload` est exécutée.</span><span class="sxs-lookup"><span data-stu-id="2cd81-271">Expired, disabled, and updated secrets in the key vault are not respected by the app until `Reload` is executed.</span></span>

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a><span data-ttu-id="2cd81-272">Secrets désactivés et expirés</span><span class="sxs-lookup"><span data-stu-id="2cd81-272">Disabled and expired secrets</span></span>

<span data-ttu-id="2cd81-273">Les secrets désactivés et expirés lèvent une exception `KeyVaultClientException`.</span><span class="sxs-lookup"><span data-stu-id="2cd81-273">Disabled and expired secrets throw a `KeyVaultClientException`.</span></span> <span data-ttu-id="2cd81-274">Pour empêcher votre application de lever cette exception, remplacez votre application ou mettez à jour le secret désactivé/expiré.</span><span class="sxs-lookup"><span data-stu-id="2cd81-274">To prevent your app from throwing, replace your app or update the disabled/expired secret.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="2cd81-275">Résoudre les problèmes</span><span class="sxs-lookup"><span data-stu-id="2cd81-275">Troubleshoot</span></span>

<span data-ttu-id="2cd81-276">Lorsque l’application ne parvient pas à charger la configuration de l’utilisation du fournisseur, un message d’erreur est écrite à la [infrastructure de journalisation ASP.NET Core](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="2cd81-276">When the app fails to load configuration using the provider, an error message is written to the [ASP.NET Core Logging infrastructure](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="2cd81-277">Les conditions suivantes empêchent le chargement de la configuration :</span><span class="sxs-lookup"><span data-stu-id="2cd81-277">The following conditions will prevent configuration from loading:</span></span>

* <span data-ttu-id="2cd81-278">L’application n’est pas configurée correctement dans Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="2cd81-278">The app isn't configured correctly in Azure Active Directory.</span></span>
* <span data-ttu-id="2cd81-279">Le coffre de clés n’existe pas dans Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="2cd81-279">The key vault doesn't exist in Azure Key Vault.</span></span>
* <span data-ttu-id="2cd81-280">L’application n’est pas autorisée à accéder au coffre de clés.</span><span class="sxs-lookup"><span data-stu-id="2cd81-280">The app isn't authorized to access the key vault.</span></span>
* <span data-ttu-id="2cd81-281">La stratégie d’accès n’inclut pas les autorisations `Get` et `List`.</span><span class="sxs-lookup"><span data-stu-id="2cd81-281">The access policy doesn't include `Get` and `List` permissions.</span></span>
* <span data-ttu-id="2cd81-282">Dans le coffre de clés, les données de configuration (paire nom-valeur) sont manquantes, désactivées, expirées ou incorrectement nommées.</span><span class="sxs-lookup"><span data-stu-id="2cd81-282">In the key vault, the configuration data (name-value pair) is incorrectly named, missing, disabled, or expired.</span></span>
* <span data-ttu-id="2cd81-283">L’application a le nom de coffre de clés incorrect (`KeyVaultName`), Id d’Application Azure AD (`AzureADApplicationId`), ou mot de passe Azure AD (clé secrète Client) (`AzureADPassword`).</span><span class="sxs-lookup"><span data-stu-id="2cd81-283">The app has the wrong key vault name (`KeyVaultName`), Azure AD Application Id (`AzureADApplicationId`), or Azure AD Password (Client Secret) (`AzureADPassword`).</span></span>
* <span data-ttu-id="2cd81-284">Le mot de passe Azure AD (clé secrète Client) (`AzureADPassword`) a expiré.</span><span class="sxs-lookup"><span data-stu-id="2cd81-284">The Azure AD Password (Client Secret) (`AzureADPassword`) is expired.</span></span>
* <span data-ttu-id="2cd81-285">La clé de configuration (nom) est incorrecte dans l’application pour la valeur que vous tentez de charger.</span><span class="sxs-lookup"><span data-stu-id="2cd81-285">The configuration key (name) is incorrect in the app for the value you're trying to load.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2cd81-286">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="2cd81-286">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="2cd81-287">Microsoft Azure : Coffre de clés</span><span class="sxs-lookup"><span data-stu-id="2cd81-287">Microsoft Azure: Key Vault</span></span>](https://azure.microsoft.com/services/key-vault/)
* [<span data-ttu-id="2cd81-288">Microsoft Azure : Documentation Key Vault</span><span class="sxs-lookup"><span data-stu-id="2cd81-288">Microsoft Azure: Key Vault Documentation</span></span>](/azure/key-vault/)
* [<span data-ttu-id="2cd81-289">Pour générer et transférer protégée par HSM de clés pour Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="2cd81-289">How to generate and transfer HSM-protected keys for Azure Key Vault</span></span>](/azure/key-vault/key-vault-hsm-protected-keys)
* [<span data-ttu-id="2cd81-290">Classe de KeyVaultClient</span><span class="sxs-lookup"><span data-stu-id="2cd81-290">KeyVaultClient Class</span></span>](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* [<span data-ttu-id="2cd81-291">Démarrage rapide : Définir et récupérer un secret dans Azure Key Vault à l’aide d’une application web .NET</span><span class="sxs-lookup"><span data-stu-id="2cd81-291">Quickstart: Set and retrieve a secret from Azure Key Vault by using a .NET web app</span></span>](/azure/key-vault/quick-create-net)
* [<span data-ttu-id="2cd81-292">Tutoriel : Comment utiliser Azure Key Vault avec Azure Windows Virtual Machine dans .NET</span><span class="sxs-lookup"><span data-stu-id="2cd81-292">Tutorial: How to use Azure Key Vault with Azure Windows Virtual Machine in .NET</span></span>](/azure/key-vault/tutorial-net-windows-virtual-machine)
