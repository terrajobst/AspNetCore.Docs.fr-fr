---
title: Fournisseur de configuration Azure Key Vault dans ASP.NET Core
author: guardrex
description: Découvrez comment utiliser le fournisseur de configuration Azure Key Vault pour configurer une application à l’aide de paires nom-valeur chargées lors de l’exécution.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/14/2019
uid: security/key-vault-configuration
ms.openlocfilehash: e0e55d40734e0cb6e3e1afe1c708ec47c6f43054
ms.sourcegitcommit: f91d322f790123d41ec3271fa084ae20ed9f89a6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/18/2019
ms.locfileid: "74155162"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a><span data-ttu-id="724e1-103">Fournisseur de configuration Azure Key Vault dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="724e1-103">Azure Key Vault Configuration Provider in ASP.NET Core</span></span>

<span data-ttu-id="724e1-104">Par [Luke Latham](https://github.com/guardrex) et [Andrew Stanton-infirmière](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="724e1-104">By [Luke Latham](https://github.com/guardrex) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="724e1-105">Ce document explique comment utiliser le fournisseur de configuration [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) pour charger des valeurs de configuration d’application à partir de Azure Key Vault secrets.</span><span class="sxs-lookup"><span data-stu-id="724e1-105">This document explains how to use the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Configuration Provider to load app configuration values from Azure Key Vault secrets.</span></span> <span data-ttu-id="724e1-106">Azure Key Vault est un service basé sur le Cloud qui permet de protéger les clés de chiffrement et les secrets utilisés par les applications et les services.</span><span class="sxs-lookup"><span data-stu-id="724e1-106">Azure Key Vault is a cloud-based service that assists in safeguarding cryptographic keys and secrets used by apps and services.</span></span> <span data-ttu-id="724e1-107">Les scénarios courants d’utilisation de Azure Key Vault avec les applications ASP.NET Core sont les suivants :</span><span class="sxs-lookup"><span data-stu-id="724e1-107">Common scenarios for using Azure Key Vault with ASP.NET Core apps include:</span></span>

* <span data-ttu-id="724e1-108">Contrôle de l’accès aux données de configuration sensibles.</span><span class="sxs-lookup"><span data-stu-id="724e1-108">Controlling access to sensitive configuration data.</span></span>
* <span data-ttu-id="724e1-109">Respect de la configuration requise pour les modules de sécurité matériels (HSM) certifiés FIPS 140-2 de niveau 2 lors du stockage des données de configuration.</span><span class="sxs-lookup"><span data-stu-id="724e1-109">Meeting the requirement for FIPS 140-2 Level 2 validated Hardware Security Modules (HSM's) when storing configuration data.</span></span>

<span data-ttu-id="724e1-110">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="724e1-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="packages"></a><span data-ttu-id="724e1-111">Packages</span><span class="sxs-lookup"><span data-stu-id="724e1-111">Packages</span></span>

<span data-ttu-id="724e1-112">Ajoutez une référence de package au package [Microsoft. extensions. Configuration. AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) .</span><span class="sxs-lookup"><span data-stu-id="724e1-112">Add a package reference to the [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) package.</span></span>

## <a name="sample-app"></a><span data-ttu-id="724e1-113">Exemple d’application</span><span class="sxs-lookup"><span data-stu-id="724e1-113">Sample app</span></span>

<span data-ttu-id="724e1-114">L’exemple d’application s’exécute dans l’un des deux modes déterminés par l’instruction `#define` en haut du fichier *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="724e1-114">The sample app runs in either of two modes determined by the `#define` statement at the top of the *Program.cs* file:</span></span>

* <span data-ttu-id="724e1-115">`Certificate` &ndash; montre l’utilisation d’un ID client Azure Key Vault et d’un certificat X. 509 pour accéder aux secrets stockés dans Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="724e1-115">`Certificate` &ndash; Demonstrates the use of an Azure Key Vault Client ID and X.509 certificate to access secrets stored in Azure Key Vault.</span></span> <span data-ttu-id="724e1-116">Cette version de l’exemple peut être exécutée à partir de n’importe quel emplacement, déployée sur Azure App Service ou n’importe quel hôte capable de servir une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="724e1-116">This version of the sample can be run from any location, deployed to Azure App Service or any host capable of serving an ASP.NET Core app.</span></span>
* <span data-ttu-id="724e1-117">`Managed` &ndash; montre comment utiliser [des identités gérées pour les ressources Azure](/azure/active-directory/managed-identities-azure-resources/overview) pour authentifier l’application afin de Azure Key Vault avec l’authentification Azure ad sans informations d’identification stockées dans le code ou la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="724e1-117">`Managed` &ndash; Demonstrates how to use [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) to authenticate the app to Azure Key Vault with Azure AD authentication without credentials stored in the app's code or configuration.</span></span> <span data-ttu-id="724e1-118">Lorsque vous utilisez des identités gérées pour l’authentification, un ID d’application et un mot de passe de Azure AD (clé secrète client) ne sont pas requis.</span><span class="sxs-lookup"><span data-stu-id="724e1-118">When using managed identities to authenticate, an Azure AD Application ID and Password (Client Secret) aren't required.</span></span> <span data-ttu-id="724e1-119">La version `Managed` de l’exemple doit être déployée sur Azure.</span><span class="sxs-lookup"><span data-stu-id="724e1-119">The `Managed` version of the sample must be deployed to Azure.</span></span> <span data-ttu-id="724e1-120">Suivez les instructions de la section [utiliser les identités gérées pour les ressources Azure](#use-managed-identities-for-azure-resources) .</span><span class="sxs-lookup"><span data-stu-id="724e1-120">Follow the guidance in the [Use the Managed identities for Azure resources](#use-managed-identities-for-azure-resources) section.</span></span>

<span data-ttu-id="724e1-121">Pour plus d’informations sur la configuration d’un exemple d’application à l’aide de directives de préprocesseur (`#define`), consultez <xref:index#preprocessor-directives-in-sample-code>.</span><span class="sxs-lookup"><span data-stu-id="724e1-121">For more information on how to configure a sample app using preprocessor directives (`#define`), see <xref:index#preprocessor-directives-in-sample-code>.</span></span>

## <a name="secret-storage-in-the-development-environment"></a><span data-ttu-id="724e1-122">Stockage secret dans l’environnement de développement</span><span class="sxs-lookup"><span data-stu-id="724e1-122">Secret storage in the Development environment</span></span>

<span data-ttu-id="724e1-123">Définissez les secrets localement à l’aide de l' [outil Gestionnaire de secret](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="724e1-123">Set secrets locally using the [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="724e1-124">Lorsque l’exemple d’application s’exécute sur l’ordinateur local dans l’environnement de développement, les secrets sont chargés à partir du magasin du gestionnaire de secret local.</span><span class="sxs-lookup"><span data-stu-id="724e1-124">When the sample app runs on the local machine in the Development environment, secrets are loaded from the local Secret Manager store.</span></span>

<span data-ttu-id="724e1-125">L’outil Gestionnaire de secret nécessite une propriété `<UserSecretsId>` dans le fichier projet de l’application.</span><span class="sxs-lookup"><span data-stu-id="724e1-125">The Secret Manager tool requires a `<UserSecretsId>` property in the app's project file.</span></span> <span data-ttu-id="724e1-126">Définissez la valeur de la propriété (`{GUID}`) sur n’importe quel GUID unique :</span><span class="sxs-lookup"><span data-stu-id="724e1-126">Set the property value (`{GUID}`) to any unique GUID:</span></span>

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

<span data-ttu-id="724e1-127">Les secrets sont créés en tant que paires nom-valeur.</span><span class="sxs-lookup"><span data-stu-id="724e1-127">Secrets are created as name-value pairs.</span></span> <span data-ttu-id="724e1-128">Les valeurs hiérarchiques (sections de configuration) utilisent un `:` (deux-points) comme séparateur dans ASP.NET Core noms de clé de [configuration](xref:fundamentals/configuration/index) .</span><span class="sxs-lookup"><span data-stu-id="724e1-128">Hierarchical values (configuration sections) use a `:` (colon) as a separator in [ASP.NET Core configuration](xref:fundamentals/configuration/index) key names.</span></span>

<span data-ttu-id="724e1-129">Le gestionnaire de secret est utilisé à partir d’une interface de commande ouverte à la [racine de contenu](xref:fundamentals/index#content-root)du projet, où `{SECRET NAME}` est le nom et `{SECRET VALUE}` est la valeur :</span><span class="sxs-lookup"><span data-stu-id="724e1-129">The Secret Manager is used from a command shell opened to the project's [content root](xref:fundamentals/index#content-root), where `{SECRET NAME}` is the name and `{SECRET VALUE}` is the value:</span></span>

```dotnetcli
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

<span data-ttu-id="724e1-130">Exécutez les commandes suivantes dans une interface de commande à partir de la [racine de contenu](xref:fundamentals/index#content-root) du projet pour définir les secrets de l’exemple d’application :</span><span class="sxs-lookup"><span data-stu-id="724e1-130">Execute the following commands in a command shell from the project's [content root](xref:fundamentals/index#content-root) to set the secrets for the sample app:</span></span>

```dotnetcli
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

<span data-ttu-id="724e1-131">Lorsque ces secrets sont stockés dans Azure Key Vault dans le [stockage secret dans l’environnement de production avec Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section, le suffixe `_dev` est remplacé par `_prod`.</span><span class="sxs-lookup"><span data-stu-id="724e1-131">When these secrets are stored in Azure Key Vault in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section, the `_dev` suffix is changed to `_prod`.</span></span> <span data-ttu-id="724e1-132">Le suffixe fournit un signal visuel dans la sortie de l’application qui indique la source des valeurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="724e1-132">The suffix provides a visual cue in the app's output indicating the source of the configuration values.</span></span>

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a><span data-ttu-id="724e1-133">Stockage secret dans l’environnement de production avec Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="724e1-133">Secret storage in the Production environment with Azure Key Vault</span></span>

<span data-ttu-id="724e1-134">Les instructions fournies par le Guide de [démarrage rapide : définir et récupérer un secret à partir de Azure Key Vault à l’aide de Azure CLI](/azure/key-vault/quick-create-cli) rubrique sont résumées ici pour créer un Azure Key Vault et stocker des secrets utilisés par l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="724e1-134">The instructions provided by the [Quickstart: Set and retrieve a secret from Azure Key Vault using Azure CLI](/azure/key-vault/quick-create-cli) topic are summarized here for creating an Azure Key Vault and storing secrets used by the sample app.</span></span> <span data-ttu-id="724e1-135">Pour plus d’informations, reportez-vous à la rubrique.</span><span class="sxs-lookup"><span data-stu-id="724e1-135">Refer to the topic for further details.</span></span>

1. <span data-ttu-id="724e1-136">Ouvrez Azure Cloud Shell à l’aide de l’une des méthodes suivantes dans la [portail Azure](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="724e1-136">Open Azure Cloud shell using any one of the following methods in the [Azure portal](https://portal.azure.com/):</span></span>

   * <span data-ttu-id="724e1-137">Sélectionnez **essayer** dans le coin supérieur droit d’un bloc de code.</span><span class="sxs-lookup"><span data-stu-id="724e1-137">Select **Try It** in the upper-right corner of a code block.</span></span> <span data-ttu-id="724e1-138">Utilisez la chaîne de recherche « Azure CLI » dans la zone de texte.</span><span class="sxs-lookup"><span data-stu-id="724e1-138">Use the search string "Azure CLI" in the text box.</span></span>
   * <span data-ttu-id="724e1-139">Ouvrez Cloud Shell dans votre navigateur à l’aide du bouton **Launch Cloud Shell** .</span><span class="sxs-lookup"><span data-stu-id="724e1-139">Open Cloud Shell in your browser with the **Launch Cloud Shell** button.</span></span>
   * <span data-ttu-id="724e1-140">Sélectionnez le bouton **Cloud Shell** dans le menu situé dans l’angle supérieur droit du portail Azure.</span><span class="sxs-lookup"><span data-stu-id="724e1-140">Select the **Cloud Shell** button on the menu in the upper-right corner of the Azure portal.</span></span>

   <span data-ttu-id="724e1-141">Pour plus d’informations, consultez [interface de ligne de commande Azure (CLI)](/cli/azure/) et [vue d’ensemble de Azure Cloud Shell](/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="724e1-141">For more information, see [Azure Command-Line Interface (CLI)](/cli/azure/) and [Overview of Azure Cloud Shell](/azure/cloud-shell/overview).</span></span>

1. <span data-ttu-id="724e1-142">Si vous n’êtes pas déjà authentifié, connectez-vous avec la commande `az login`.</span><span class="sxs-lookup"><span data-stu-id="724e1-142">If you aren't already authenticated, sign in with the `az login` command.</span></span>

1. <span data-ttu-id="724e1-143">Créez un groupe de ressources à l’aide de la commande suivante, où `{RESOURCE GROUP NAME}` est le nom du groupe de ressources pour le nouveau groupe de ressources et `{LOCATION}` la région Azure (Datacenter) :</span><span class="sxs-lookup"><span data-stu-id="724e1-143">Create a resource group with the following command, where `{RESOURCE GROUP NAME}` is the resource group name for the new resource group and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```azure-cli
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="724e1-144">Créez un coffre de clés dans le groupe de ressources à l’aide de la commande suivante, où `{KEY VAULT NAME}` est le nom du nouveau coffre de clés et `{LOCATION}` est la région Azure (Datacenter) :</span><span class="sxs-lookup"><span data-stu-id="724e1-144">Create a key vault in the resource group with the following command, where `{KEY VAULT NAME}` is the name for the new key vault and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```azure-cli
   az keyvault create --name "{KEY VAULT NAME}" --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="724e1-145">Créez des secrets dans le coffre de clés en tant que paires nom-valeur.</span><span class="sxs-lookup"><span data-stu-id="724e1-145">Create secrets in the key vault as name-value pairs.</span></span>

   <span data-ttu-id="724e1-146">Azure Key Vault noms de secrets sont limités aux caractères alphanumériques et aux tirets.</span><span class="sxs-lookup"><span data-stu-id="724e1-146">Azure Key Vault secret names are limited to alphanumeric characters and dashes.</span></span> <span data-ttu-id="724e1-147">Les valeurs hiérarchiques (sections de configuration) utilisent des `--` (deux tirets) comme séparateur.</span><span class="sxs-lookup"><span data-stu-id="724e1-147">Hierarchical values (configuration sections) use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="724e1-148">Les signes deux-points, qui sont normalement utilisés pour délimiter une section d’une sous-clé dans [ASP.net Core configuration](xref:fundamentals/configuration/index), ne sont pas autorisés dans les noms de secrets du coffre de clés.</span><span class="sxs-lookup"><span data-stu-id="724e1-148">Colons, which are normally used to delimit a section from a subkey in [ASP.NET Core configuration](xref:fundamentals/configuration/index), aren't allowed in key vault secret names.</span></span> <span data-ttu-id="724e1-149">Par conséquent, deux tirets sont utilisés et permutés pour un signe deux-points lorsque les secrets sont chargés dans la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="724e1-149">Therefore, two dashes are used and swapped for a colon when the secrets are loaded into the app's configuration.</span></span>

   <span data-ttu-id="724e1-150">Les secrets suivants sont destinés à être utilisés avec l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="724e1-150">The following secrets are for use with the sample app.</span></span> <span data-ttu-id="724e1-151">Les valeurs incluent un suffixe de `_prod` pour les distinguer des valeurs de suffixe `_dev` chargées dans l’environnement de développement des secrets d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="724e1-151">The values include a `_prod` suffix to distinguish them from the `_dev` suffix values loaded in the Development environment from User Secrets.</span></span> <span data-ttu-id="724e1-152">Remplacez `{KEY VAULT NAME}` par le nom du coffre de clés que vous avez créé à l’étape précédente :</span><span class="sxs-lookup"><span data-stu-id="724e1-152">Replace `{KEY VAULT NAME}` with the name of the key vault that you created in the prior step:</span></span>

   ```azure-cli
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-x509-certificate-for-non-azure-hosted-apps"></a><span data-ttu-id="724e1-153">Utiliser l’ID d’application et le certificat X. 509 pour les applications non hébergées sur Azure</span><span class="sxs-lookup"><span data-stu-id="724e1-153">Use Application ID and X.509 certificate for non-Azure-hosted apps</span></span>

<span data-ttu-id="724e1-154">Configurez Azure AD, Azure Key Vault et l’application pour qu’elle utilise un ID d’application Azure Active Directory et un certificat X. 509 pour s’authentifier auprès d’un coffre de clés **lorsque l’application est hébergée en dehors d’Azure**.</span><span class="sxs-lookup"><span data-stu-id="724e1-154">Configure Azure AD, Azure Key Vault, and the app to use an Azure Active Directory Application ID and X.509 certificate to authenticate to a key vault **when the app is hosted outside of Azure**.</span></span> <span data-ttu-id="724e1-155">Pour plus d’informations, consultez [à propos des clés, des secrets et des certificats](/azure/key-vault/about-keys-secrets-and-certificates).</span><span class="sxs-lookup"><span data-stu-id="724e1-155">For more information, see [About keys, secrets, and certificates](/azure/key-vault/about-keys-secrets-and-certificates).</span></span>

> [!NOTE]
> <span data-ttu-id="724e1-156">Bien que l’utilisation d’un ID d’application et d’un certificat X. 509 soit prise en charge pour les applications hébergées dans Azure, nous vous recommandons [d’utiliser des identités gérées pour les ressources Azure](#use-managed-identities-for-azure-resources) lors de l’hébergement d’une application dans Azure.</span><span class="sxs-lookup"><span data-stu-id="724e1-156">Although using an Application ID and X.509 certificate is supported for apps hosted in Azure, we recommend using [Managed identities for Azure resources](#use-managed-identities-for-azure-resources) when hosting an app in Azure.</span></span> <span data-ttu-id="724e1-157">Les identités gérées ne nécessitent pas le stockage d’un certificat dans l’application ou dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="724e1-157">Managed identities don't require storing a certificate in the app or in the development environment.</span></span>

<span data-ttu-id="724e1-158">L’exemple d’application utilise un ID d’application et un certificat X. 509 lorsque l’instruction `#define` en haut du fichier *Program.cs* est définie sur `Certificate`.</span><span class="sxs-lookup"><span data-stu-id="724e1-158">The sample app uses an Application ID and X.509 certificate when the `#define` statement at the top of the *Program.cs* file is set to `Certificate`.</span></span>

1. <span data-ttu-id="724e1-159">Créez un certificat d’archive PKCS # 12 ( *. pfx*).</span><span class="sxs-lookup"><span data-stu-id="724e1-159">Create a PKCS#12 archive (*.pfx*) certificate.</span></span> <span data-ttu-id="724e1-160">Les options de création de certificats incluent [Makecert sur Windows](/windows/desktop/seccrypto/makecert) et [OpenSSL](https://www.openssl.org/).</span><span class="sxs-lookup"><span data-stu-id="724e1-160">Options for creating certificates include [MakeCert on Windows](/windows/desktop/seccrypto/makecert) and [OpenSSL](https://www.openssl.org/).</span></span>
1. <span data-ttu-id="724e1-161">Installez le certificat dans le magasin de certificats personnel de l’utilisateur actuel.</span><span class="sxs-lookup"><span data-stu-id="724e1-161">Install the certificate into the current user's personal certificate store.</span></span> <span data-ttu-id="724e1-162">Le marquage de la clé comme étant exportable est facultatif.</span><span class="sxs-lookup"><span data-stu-id="724e1-162">Marking the key as exportable is optional.</span></span> <span data-ttu-id="724e1-163">Notez l’empreinte numérique du certificat, qui est utilisé plus loin dans ce processus.</span><span class="sxs-lookup"><span data-stu-id="724e1-163">Note the certificate's thumbprint, which is used later in this process.</span></span>
1. <span data-ttu-id="724e1-164">Exportez le certificat PKCS # 12 ( *. pfx*) sous la forme d’un certificat encodé der ( *. cer*).</span><span class="sxs-lookup"><span data-stu-id="724e1-164">Export the PKCS#12 archive (*.pfx*) certificate as a DER-encoded certificate (*.cer*).</span></span>
1. <span data-ttu-id="724e1-165">Inscrire l’application auprès de Azure AD (**inscriptions d’applications**).</span><span class="sxs-lookup"><span data-stu-id="724e1-165">Register the app with Azure AD (**App registrations**).</span></span>
1. <span data-ttu-id="724e1-166">Chargez le certificat codé DER ( *. cer*) dans Azure ad :</span><span class="sxs-lookup"><span data-stu-id="724e1-166">Upload the DER-encoded certificate (*.cer*) to Azure AD:</span></span>
   1. <span data-ttu-id="724e1-167">Sélectionnez l’application dans Azure AD.</span><span class="sxs-lookup"><span data-stu-id="724e1-167">Select the app in Azure AD.</span></span>
   1. <span data-ttu-id="724e1-168">Accédez à **certificats & secrets**.</span><span class="sxs-lookup"><span data-stu-id="724e1-168">Navigate to **Certificates & secrets**.</span></span>
   1. <span data-ttu-id="724e1-169">Sélectionnez **Télécharger le certificat** pour télécharger le certificat, qui contient la clé publique.</span><span class="sxs-lookup"><span data-stu-id="724e1-169">Select **Upload certificate** to upload the certificate, which contains the public key.</span></span> <span data-ttu-id="724e1-170">Un certificat. *CER*, *. pem*ou *. CRT* est acceptable.</span><span class="sxs-lookup"><span data-stu-id="724e1-170">A *.cer*, *.pem*, or *.crt* certificate is acceptable.</span></span>
1. <span data-ttu-id="724e1-171">Stockez le nom du coffre de clés, l’ID de l’application et l’empreinte numérique du certificat dans le fichier *appSettings. JSON* de l’application.</span><span class="sxs-lookup"><span data-stu-id="724e1-171">Store the key vault name, Application ID, and certificate thumbprint in the app's *appsettings.json* file.</span></span>
1. <span data-ttu-id="724e1-172">Accédez à **coffres de clés** dans le portail Azure.</span><span class="sxs-lookup"><span data-stu-id="724e1-172">Navigate to **Key vaults** in the Azure portal.</span></span>
1. <span data-ttu-id="724e1-173">Sélectionnez le coffre de clés que vous avez créé dans la section [stockage secret dans l’environnement de production avec Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) .</span><span class="sxs-lookup"><span data-stu-id="724e1-173">Select the key vault that you created in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section.</span></span>
1. <span data-ttu-id="724e1-174">Sélectionnez **stratégies d’accès**.</span><span class="sxs-lookup"><span data-stu-id="724e1-174">Select **Access policies**.</span></span>
1. <span data-ttu-id="724e1-175">Sélectionnez **Ajouter une stratégie d’accès**.</span><span class="sxs-lookup"><span data-stu-id="724e1-175">Select **Add Access Policy**.</span></span>
1. <span data-ttu-id="724e1-176">Ouvrez les **autorisations secret** et fournissez à l’application les autorisations **obtenir** et **liste** .</span><span class="sxs-lookup"><span data-stu-id="724e1-176">Open **Secret permissions** and provide the app with **Get** and **List** permissions.</span></span>
1. <span data-ttu-id="724e1-177">Sélectionnez **Sélectionner un principal** et sélectionnez l’application inscrite par son nom.</span><span class="sxs-lookup"><span data-stu-id="724e1-177">Select **Select principal** and select the registered app by name.</span></span> <span data-ttu-id="724e1-178">Sélectionnez le bouton **Sélectionner** .</span><span class="sxs-lookup"><span data-stu-id="724e1-178">Select the **Select** button.</span></span>
1. <span data-ttu-id="724e1-179">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="724e1-179">Select **OK**.</span></span>
1. <span data-ttu-id="724e1-180">Sélectionnez **Enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="724e1-180">Select **Save**.</span></span>
1. <span data-ttu-id="724e1-181">Déployez l’application.</span><span class="sxs-lookup"><span data-stu-id="724e1-181">Deploy the app.</span></span>

<span data-ttu-id="724e1-182">L’exemple d’application `Certificate` obtient ses valeurs de configuration à partir d' `IConfigurationRoot` portant le même nom que le nom de la clé secrète :</span><span class="sxs-lookup"><span data-stu-id="724e1-182">The `Certificate` sample app obtains its configuration values from `IConfigurationRoot` with the same name as the secret name:</span></span>

* <span data-ttu-id="724e1-183">Valeurs non hiérarchiques : la valeur de `SecretName` est obtenue avec `config["SecretName"]`.</span><span class="sxs-lookup"><span data-stu-id="724e1-183">Non-hierarchical values: The value for `SecretName` is obtained with `config["SecretName"]`.</span></span>
* <span data-ttu-id="724e1-184">Valeurs hiérarchiques (sections) : utilisez la notation `:` (deux-points) ou la méthode d’extension `GetSection`.</span><span class="sxs-lookup"><span data-stu-id="724e1-184">Hierarchical values (sections): Use `:` (colon) notation or the `GetSection` extension method.</span></span> <span data-ttu-id="724e1-185">Utilisez l’une des approches suivantes pour obtenir la valeur de configuration :</span><span class="sxs-lookup"><span data-stu-id="724e1-185">Use either of these approaches to obtain the configuration value:</span></span>
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

<span data-ttu-id="724e1-186">Le certificat X. 509 est géré par le système d’exploitation.</span><span class="sxs-lookup"><span data-stu-id="724e1-186">The X.509 certificate is managed by the OS.</span></span> <span data-ttu-id="724e1-187">L’application appelle <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> avec les valeurs fournies par le fichier *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="724e1-187">The app calls <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> with values supplied by the *appsettings.json* file:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](key-vault-configuration/samples/3.x/SampleApp/Program.cs?name=snippet1&highlight=20-23)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](key-vault-configuration/samples/2.x/SampleApp/Program.cs?name=snippet1&highlight=20-23)]

::: moniker-end

<span data-ttu-id="724e1-188">Exemples de valeurs :</span><span class="sxs-lookup"><span data-stu-id="724e1-188">Example values:</span></span>

* <span data-ttu-id="724e1-189">Nom du coffre de clés : `contosovault`</span><span class="sxs-lookup"><span data-stu-id="724e1-189">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="724e1-190">ID de l’application : `627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="724e1-190">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="724e1-191">Empreinte du certificat : `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span><span class="sxs-lookup"><span data-stu-id="724e1-191">Certificate thumbprint: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span></span>

<span data-ttu-id="724e1-192">*appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="724e1-192">*appsettings.json*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](key-vault-configuration/samples/3.x/SampleApp/appsettings.json?highlight=10-12)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](key-vault-configuration/samples/2.x/SampleApp/appsettings.json?highlight=10-12)]

::: moniker-end

<span data-ttu-id="724e1-193">Quand vous exécutez l’application, une page Web affiche les valeurs de secret chargées.</span><span class="sxs-lookup"><span data-stu-id="724e1-193">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="724e1-194">Dans l’environnement de développement, les valeurs secrètes sont chargées avec le suffixe `_dev`.</span><span class="sxs-lookup"><span data-stu-id="724e1-194">In the Development environment, secret values load with the `_dev` suffix.</span></span> <span data-ttu-id="724e1-195">Dans l’environnement de production, les valeurs sont chargées avec le suffixe `_prod`.</span><span class="sxs-lookup"><span data-stu-id="724e1-195">In the Production environment, the values load with the `_prod` suffix.</span></span>

## <a name="use-managed-identities-for-azure-resources"></a><span data-ttu-id="724e1-196">Utiliser des identités gérées pour les ressources Azure</span><span class="sxs-lookup"><span data-stu-id="724e1-196">Use Managed identities for Azure resources</span></span>

<span data-ttu-id="724e1-197">**Une application déployée sur Azure** peut tirer parti des [identités gérées pour les ressources Azure](/azure/active-directory/managed-identities-azure-resources/overview), ce qui permet à l’application de s’authentifier avec Azure Key Vault à l’aide de l’authentification Azure ad sans informations d’identification (ID d’application et mot de passe/clé secrète client) stockées dans l’application.</span><span class="sxs-lookup"><span data-stu-id="724e1-197">**An app deployed to Azure** can take advantage of [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview), which allows the app to authenticate with Azure Key Vault using Azure AD authentication without credentials (Application ID and Password/Client Secret) stored in the app.</span></span>

<span data-ttu-id="724e1-198">L’exemple d’application utilise des identités gérées pour les ressources Azure lorsque l’instruction `#define` en haut du fichier *Program.cs* est définie sur `Managed`.</span><span class="sxs-lookup"><span data-stu-id="724e1-198">The sample app uses Managed identities for Azure resources when the `#define` statement at the top of the *Program.cs* file is set to `Managed`.</span></span>

<span data-ttu-id="724e1-199">Entrez le nom du coffre dans le fichier *appSettings. JSON* de l’application.</span><span class="sxs-lookup"><span data-stu-id="724e1-199">Enter the vault name into the app's *appsettings.json* file.</span></span> <span data-ttu-id="724e1-200">L’exemple d’application n’a pas besoin d’un ID d’application et d’un mot de passe (clé secrète client) lorsqu’il est défini sur la version `Managed`, vous pouvez donc ignorer ces entrées de configuration.</span><span class="sxs-lookup"><span data-stu-id="724e1-200">The sample app doesn't require an Application ID and Password (Client Secret) when set to the `Managed` version, so you can ignore those configuration entries.</span></span> <span data-ttu-id="724e1-201">L’application est déployée sur Azure et Azure authentifie l’application pour accéder à Azure Key Vault uniquement à l’aide du nom de coffre stocké dans le fichier *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="724e1-201">The app is deployed to Azure, and Azure authenticates the app to access Azure Key Vault only using the vault name stored in the *appsettings.json* file.</span></span>

<span data-ttu-id="724e1-202">Déployez l’exemple d’application sur Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="724e1-202">Deploy the sample app to Azure App Service.</span></span>

<span data-ttu-id="724e1-203">Une application déployée sur Azure App Service est automatiquement inscrite auprès de Azure AD lors de la création du service.</span><span class="sxs-lookup"><span data-stu-id="724e1-203">An app deployed to Azure App Service is automatically registered with Azure AD when the service is created.</span></span> <span data-ttu-id="724e1-204">Obtenez l’ID d’objet à partir du déploiement pour l’utiliser dans la commande suivante.</span><span class="sxs-lookup"><span data-stu-id="724e1-204">Obtain the Object ID from the deployment for use in the following command.</span></span> <span data-ttu-id="724e1-205">L’ID d’objet est affiché dans le Portail Azure sur le panneau d' **identité** du App service.</span><span class="sxs-lookup"><span data-stu-id="724e1-205">The Object ID is shown in the Azure portal on the **Identity** panel of the App Service.</span></span>

<span data-ttu-id="724e1-206">À l’aide de Azure CLI et de l’ID d’objet de l’application, fournissez à l’application des autorisations `list` et `get` pour accéder au coffre de clés :</span><span class="sxs-lookup"><span data-stu-id="724e1-206">Using Azure CLI and the app's Object ID, provide the app with `list` and `get` permissions to access the key vault:</span></span>

```azure-cli
az keyvault set-policy --name '{KEY VAULT NAME}' --object-id {OBJECT ID} --secret-permissions get list
```

<span data-ttu-id="724e1-207">**Redémarrez l’application** à l’aide de Azure CLI, de PowerShell ou de l’portail Azure.</span><span class="sxs-lookup"><span data-stu-id="724e1-207">**Restart the app** using Azure CLI, PowerShell, or the Azure portal.</span></span>

<span data-ttu-id="724e1-208">L’exemple d’application :</span><span class="sxs-lookup"><span data-stu-id="724e1-208">The sample app:</span></span>

* <span data-ttu-id="724e1-209">Crée une instance de la classe `AzureServiceTokenProvider` sans chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="724e1-209">Creates an instance of the `AzureServiceTokenProvider` class without a connection string.</span></span> <span data-ttu-id="724e1-210">Lorsqu’une chaîne de connexion n’est pas fournie, le fournisseur tente d’obtenir un jeton d’accès à partir des identités gérées pour les ressources Azure.</span><span class="sxs-lookup"><span data-stu-id="724e1-210">When a connection string isn't provided, the provider attempts to obtain an access token from Managed identities for Azure resources.</span></span>
* <span data-ttu-id="724e1-211">Une nouvelle <xref:Microsoft.Azure.KeyVault.KeyVaultClient> est créée avec le rappel de jeton d’instance `AzureServiceTokenProvider`.</span><span class="sxs-lookup"><span data-stu-id="724e1-211">A new <xref:Microsoft.Azure.KeyVault.KeyVaultClient> is created with the `AzureServiceTokenProvider` instance token callback.</span></span>
* <span data-ttu-id="724e1-212">L’instance <xref:Microsoft.Azure.KeyVault.KeyVaultClient> est utilisée avec une implémentation par défaut de <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> qui charge toutes les valeurs secrètes et remplace les doubles tirets (`--`) par des signes deux-points (`:`) dans les noms de clés.</span><span class="sxs-lookup"><span data-stu-id="724e1-212">The <xref:Microsoft.Azure.KeyVault.KeyVaultClient> instance is used with a default implementation of <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> that loads all secret values and replaces double-dashes (`--`) with colons (`:`) in key names.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](key-vault-configuration/samples/3.x/SampleApp/Program.cs?name=snippet2&highlight=13-21)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](key-vault-configuration/samples/2.x/SampleApp/Program.cs?name=snippet2&highlight=13-21)]

::: moniker-end

<span data-ttu-id="724e1-213">Exemple de valeur de nom de coffre de clés : `contosovault`</span><span class="sxs-lookup"><span data-stu-id="724e1-213">Key vault name example value: `contosovault`</span></span>
    
<span data-ttu-id="724e1-214">*appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="724e1-214">*appsettings.json*:</span></span>

```json
{
  "KeyVaultName": "Key Vault Name"
}
```

<span data-ttu-id="724e1-215">Quand vous exécutez l’application, une page Web affiche les valeurs de secret chargées.</span><span class="sxs-lookup"><span data-stu-id="724e1-215">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="724e1-216">Dans l’environnement de développement, les valeurs secrètes ont le suffixe `_dev`, car elles sont fournies par les secrets de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="724e1-216">In the Development environment, secret values have the `_dev` suffix because they're provided by User Secrets.</span></span> <span data-ttu-id="724e1-217">Dans l’environnement de production, les valeurs sont chargées avec le suffixe `_prod`, car elles sont fournies par Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="724e1-217">In the Production environment, the values load with the `_prod` suffix because they're provided by Azure Key Vault.</span></span>

<span data-ttu-id="724e1-218">Si vous recevez une erreur de `Access denied`, vérifiez que l’application est inscrite auprès de Azure AD et que vous avez accès au coffre de clés.</span><span class="sxs-lookup"><span data-stu-id="724e1-218">If you receive an `Access denied` error, confirm that the app is registered with Azure AD and provided access to the key vault.</span></span> <span data-ttu-id="724e1-219">Confirmez que vous avez redémarré le service dans Azure.</span><span class="sxs-lookup"><span data-stu-id="724e1-219">Confirm that you've restarted the service in Azure.</span></span>

<span data-ttu-id="724e1-220">Pour plus d’informations sur l’utilisation du fournisseur avec une identité gérée et un pipeline Azure DevOps, consultez [créer une connexion de service Azure Resource Manager à une machine virtuelle avec une identité de service managée](/azure/devops/pipelines/library/connect-to-azure#create-an-azure-resource-manager-service-connection-to-a-vm-with-a-managed-service-identity).</span><span class="sxs-lookup"><span data-stu-id="724e1-220">For information on using the provider with a managed identity and an Azure DevOps pipeline, see [Create an Azure Resource Manager service connection to a VM with a managed service identity](/azure/devops/pipelines/library/connect-to-azure#create-an-azure-resource-manager-service-connection-to-a-vm-with-a-managed-service-identity).</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="configuration-options"></a><span data-ttu-id="724e1-221">Options de configuration</span><span class="sxs-lookup"><span data-stu-id="724e1-221">Configuration options</span></span>

<span data-ttu-id="724e1-222"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> pouvez accepter un <xref:Microsoft.Extensions.Configuration.AzureKeyVault.AzureKeyVaultConfigurationOptions>:</span><span class="sxs-lookup"><span data-stu-id="724e1-222"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> can accept an <xref:Microsoft.Extensions.Configuration.AzureKeyVault.AzureKeyVaultConfigurationOptions>:</span></span>

```csharp
config.AddAzureKeyVault(
    new AzureKeyVaultConfigurationOptions()
    {
        ...
    });
```

| <span data-ttu-id="724e1-223">Property</span><span class="sxs-lookup"><span data-stu-id="724e1-223">Property</span></span>         | <span data-ttu-id="724e1-224">Description</span><span class="sxs-lookup"><span data-stu-id="724e1-224">Description</span></span> |
| ---------------- | ----------- |
| `Client`         | <span data-ttu-id="724e1-225"><xref:Microsoft.Azure.KeyVault.KeyVaultClient> à utiliser pour récupérer des valeurs.</span><span class="sxs-lookup"><span data-stu-id="724e1-225"><xref:Microsoft.Azure.KeyVault.KeyVaultClient> to use for retrieving values.</span></span> |
| `Manager`        | <span data-ttu-id="724e1-226"><xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> instance utilisée pour contrôler le chargement du secret.</span><span class="sxs-lookup"><span data-stu-id="724e1-226"><xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> instance used to control secret loading.</span></span> |
| `ReloadInterval` | <span data-ttu-id="724e1-227">`Timespan` d’attente entre les tentatives d’interrogation du coffre de clés pour les modifications.</span><span class="sxs-lookup"><span data-stu-id="724e1-227">`Timespan` to wait between attempts at polling the key vault for changes.</span></span> <span data-ttu-id="724e1-228">La valeur par défaut est `null` (la configuration n’est pas rechargée).</span><span class="sxs-lookup"><span data-stu-id="724e1-228">The default value is `null` (configuration isn't reloaded).</span></span> |
| `Vault`          | <span data-ttu-id="724e1-229">URI du coffre de clés.</span><span class="sxs-lookup"><span data-stu-id="724e1-229">Key vault URI.</span></span> |

::: moniker-end

## <a name="use-a-key-name-prefix"></a><span data-ttu-id="724e1-230">Utiliser un préfixe de nom de clé</span><span class="sxs-lookup"><span data-stu-id="724e1-230">Use a key name prefix</span></span>

<span data-ttu-id="724e1-231"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> fournit une surcharge qui accepte une implémentation de <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>, ce qui vous permet de contrôler la façon dont les secrets de coffre de clés sont convertis en clés de configuration.</span><span class="sxs-lookup"><span data-stu-id="724e1-231"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> provides an overload that accepts an implementation of <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>, which allows you to control how key vault secrets are converted into configuration keys.</span></span> <span data-ttu-id="724e1-232">Par exemple, vous pouvez implémenter l’interface pour charger des valeurs de secret en fonction d’une valeur de préfixe que vous fournissez au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="724e1-232">For example, you can implement the interface to load secret values based on a prefix value you provide at app startup.</span></span> <span data-ttu-id="724e1-233">Cela vous permet, par exemple, de charger des secrets basés sur la version de l’application.</span><span class="sxs-lookup"><span data-stu-id="724e1-233">This allows you, for example, to load secrets based on the version of the app.</span></span>

> [!WARNING]
> <span data-ttu-id="724e1-234">N’utilisez pas de préfixes sur des secrets de coffre de clés pour placer des secrets pour plusieurs applications dans le même coffre de clés ou pour placer des secrets d’environnement (par exemple, le *développement* et les secrets de *production* ) dans le même coffre.</span><span class="sxs-lookup"><span data-stu-id="724e1-234">Don't use prefixes on key vault secrets to place secrets for multiple apps into the same key vault or to place environmental secrets (for example, *development* versus *production* secrets) into the same vault.</span></span> <span data-ttu-id="724e1-235">Nous recommandons que les applications et les environnements de développement/production utilisent des coffres de clés distincts pour isoler les environnements d’application pour le plus haut niveau de sécurité.</span><span class="sxs-lookup"><span data-stu-id="724e1-235">We recommend that different apps and development/production environments use separate key vaults to isolate app environments for the highest level of security.</span></span>

<span data-ttu-id="724e1-236">Dans l’exemple suivant, un secret est établi dans le coffre de clés (et l’utilisation de l’outil secret Manager pour l’environnement de développement) pour `5000-AppSecret` (les points ne sont pas autorisés dans les noms de secret de coffre de clés).</span><span class="sxs-lookup"><span data-stu-id="724e1-236">In the following example, a secret is established in the key vault (and using the Secret Manager tool for the Development environment) for `5000-AppSecret` (periods aren't allowed in key vault secret names).</span></span> <span data-ttu-id="724e1-237">Ce secret représente un secret d’application pour la version 5.0.0.0 de l’application.</span><span class="sxs-lookup"><span data-stu-id="724e1-237">This secret represents an app secret for version 5.0.0.0 of the app.</span></span> <span data-ttu-id="724e1-238">Pour une autre version de l’application, 5.1.0.0, un secret est ajouté au coffre de clés (et à l’aide de l’outil secret Manager) pour `5100-AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="724e1-238">For another version of the app, 5.1.0.0, a secret is added to the key vault (and using the Secret Manager tool) for `5100-AppSecret`.</span></span> <span data-ttu-id="724e1-239">Chaque version de l’application charge sa valeur secrète avec version dans sa configuration en tant que `AppSecret`, en éliminant la version au fur et à mesure qu’elle charge le secret.</span><span class="sxs-lookup"><span data-stu-id="724e1-239">Each app version loads its versioned secret value into its configuration as `AppSecret`, stripping off the version as it loads the secret.</span></span>

<span data-ttu-id="724e1-240"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> est appelé avec une <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>personnalisée :</span><span class="sxs-lookup"><span data-stu-id="724e1-240"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> is called with a custom <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>:</span></span>

[!code-csharp[](key-vault-configuration/samples_snapshot/Program.cs)]

<span data-ttu-id="724e1-241">L’implémentation <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> réagit aux préfixes de version des secrets pour charger le secret approprié dans la configuration :</span><span class="sxs-lookup"><span data-stu-id="724e1-241">The <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> implementation reacts to the version prefixes of secrets to load the proper secret into configuration:</span></span>

* <span data-ttu-id="724e1-242">`Load` charge un secret quand son nom commence par le préfixe.</span><span class="sxs-lookup"><span data-stu-id="724e1-242">`Load` loads a secret when its name starts with the prefix.</span></span> <span data-ttu-id="724e1-243">Les autres secrets ne sont pas chargés.</span><span class="sxs-lookup"><span data-stu-id="724e1-243">Other secrets aren't loaded.</span></span>
* <span data-ttu-id="724e1-244">`GetKey`:</span><span class="sxs-lookup"><span data-stu-id="724e1-244">`GetKey`:</span></span>
  * <span data-ttu-id="724e1-245">Supprime le préfixe du nom de la clé secrète.</span><span class="sxs-lookup"><span data-stu-id="724e1-245">Removes the prefix from the secret name.</span></span>
  * <span data-ttu-id="724e1-246">Remplace deux tirets dans n’importe quel nom par le `KeyDelimiter`, qui est le délimiteur utilisé dans la configuration (généralement un signe deux-points).</span><span class="sxs-lookup"><span data-stu-id="724e1-246">Replaces two dashes in any name with the `KeyDelimiter`, which is the delimiter used in configuration (usually a colon).</span></span> <span data-ttu-id="724e1-247">Azure Key Vault n’autorise pas les deux-points dans les noms de secrets.</span><span class="sxs-lookup"><span data-stu-id="724e1-247">Azure Key Vault doesn't allow a colon in secret names.</span></span>

[!code-csharp[](key-vault-configuration/samples_snapshot/Startup.cs)]

<span data-ttu-id="724e1-248">La méthode `Load` est appelée par un algorithme de fournisseur qui itère au sein des secrets du coffre pour trouver ceux qui ont le préfixe de version.</span><span class="sxs-lookup"><span data-stu-id="724e1-248">The `Load` method is called by a provider algorithm that iterates through the vault secrets to find the ones that have the version prefix.</span></span> <span data-ttu-id="724e1-249">Lorsqu’un préfixe de version est trouvé avec `Load`, l’algorithme utilise la méthode `GetKey` pour retourner le nom de configuration du nom de la clé secrète.</span><span class="sxs-lookup"><span data-stu-id="724e1-249">When a version prefix is found with `Load`, the algorithm uses the `GetKey` method to return the configuration name of the secret name.</span></span> <span data-ttu-id="724e1-250">Il supprime le préfixe de version du nom du secret et retourne le reste du nom de secret pour le chargement dans les paires nom-valeur de la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="724e1-250">It strips off the version prefix from the secret's name and returns the rest of the secret name for loading into the app's configuration name-value pairs.</span></span>

<span data-ttu-id="724e1-251">Lorsque cette approche est implémentée :</span><span class="sxs-lookup"><span data-stu-id="724e1-251">When this approach is implemented:</span></span>

1. <span data-ttu-id="724e1-252">Version de l’application spécifiée dans le fichier projet de l’application.</span><span class="sxs-lookup"><span data-stu-id="724e1-252">The app's version specified in the app's project file.</span></span> <span data-ttu-id="724e1-253">Dans l’exemple suivant, la version de l’application est définie sur `5.0.0.0`:</span><span class="sxs-lookup"><span data-stu-id="724e1-253">In the following example, the app's version is set to `5.0.0.0`:</span></span>

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. <span data-ttu-id="724e1-254">Confirmez qu’une propriété `<UserSecretsId>` est présente dans le fichier projet de l’application, où `{GUID}` est un GUID fourni par l’utilisateur :</span><span class="sxs-lookup"><span data-stu-id="724e1-254">Confirm that a `<UserSecretsId>` property is present in the app's project file, where `{GUID}` is a user-supplied GUID:</span></span>

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   <span data-ttu-id="724e1-255">Enregistrez les secrets suivants localement à l’aide de l' [outil secret Manager](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="724e1-255">Save the following secrets locally with the [Secret Manager tool](xref:security/app-secrets):</span></span>

   ```dotnetcli
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. <span data-ttu-id="724e1-256">Les secrets sont enregistrés dans Azure Key Vault à l’aide des commandes de Azure CLI suivantes :</span><span class="sxs-lookup"><span data-stu-id="724e1-256">Secrets are saved in Azure Key Vault using the following Azure CLI commands:</span></span>

   ```azure-cli
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. <span data-ttu-id="724e1-257">Lorsque l’application est exécutée, les secrets du coffre de clés sont chargés.</span><span class="sxs-lookup"><span data-stu-id="724e1-257">When the app is run, the key vault secrets are loaded.</span></span> <span data-ttu-id="724e1-258">Le secret de la chaîne pour `5000-AppSecret` est mis en correspondance avec la version de l’application spécifiée dans le fichier projet de l’application (`5.0.0.0`).</span><span class="sxs-lookup"><span data-stu-id="724e1-258">The string secret for `5000-AppSecret` is matched to the app's version specified in the app's project file (`5.0.0.0`).</span></span>

1. <span data-ttu-id="724e1-259">La version, `5000` (avec le tiret), est supprimée du nom de la clé.</span><span class="sxs-lookup"><span data-stu-id="724e1-259">The version, `5000` (with the dash), is stripped from the key name.</span></span> <span data-ttu-id="724e1-260">Tout au long de l’application, la lecture de la configuration avec la clé `AppSecret` charge la valeur secrète.</span><span class="sxs-lookup"><span data-stu-id="724e1-260">Throughout the app, reading configuration with the key `AppSecret` loads the secret value.</span></span>

1. <span data-ttu-id="724e1-261">Si la version de l’application est modifiée dans le fichier projet en `5.1.0.0` et que l’application est réexécutée, la valeur secrète retournée est `5.1.0.0_secret_value_dev` dans l’environnement de développement et `5.1.0.0_secret_value_prod` en production.</span><span class="sxs-lookup"><span data-stu-id="724e1-261">If the app's version is changed in the project file to `5.1.0.0` and the app is run again, the secret value returned is `5.1.0.0_secret_value_dev` in the Development environment and `5.1.0.0_secret_value_prod` in Production.</span></span>

> [!NOTE]
> <span data-ttu-id="724e1-262">Vous pouvez également fournir votre propre implémentation de <xref:Microsoft.Azure.KeyVault.KeyVaultClient> pour <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>.</span><span class="sxs-lookup"><span data-stu-id="724e1-262">You can also provide your own <xref:Microsoft.Azure.KeyVault.KeyVaultClient> implementation to <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>.</span></span> <span data-ttu-id="724e1-263">Un client personnalisé permet de partager une seule instance du client dans l’application.</span><span class="sxs-lookup"><span data-stu-id="724e1-263">A custom client permits sharing a single instance of the client across the app.</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="724e1-264">Lier un tableau à une classe</span><span class="sxs-lookup"><span data-stu-id="724e1-264">Bind an array to a class</span></span>

<span data-ttu-id="724e1-265">Le fournisseur est en mesure de lire les valeurs de configuration dans un tableau pour la liaison à un tableau POCO.</span><span class="sxs-lookup"><span data-stu-id="724e1-265">The provider is capable of reading configuration values into an array for binding to a POCO array.</span></span>

<span data-ttu-id="724e1-266">Lors de la lecture à partir d’une source de configuration qui autorise les clés à contenir des séparateurs de deux-points (`:`), un segment de clé numérique est utilisé pour distinguer les clés qui composent un tableau (`:0:`, `:1:`,...</span><span class="sxs-lookup"><span data-stu-id="724e1-266">When reading from a configuration source that allows keys to contain colon (`:`) separators, a numeric key segment is used to distinguish the keys that make up an array (`:0:`, `:1:`, …</span></span> <span data-ttu-id="724e1-267">`:{n}:`).</span><span class="sxs-lookup"><span data-stu-id="724e1-267">`:{n}:`).</span></span> <span data-ttu-id="724e1-268">Pour plus d’informations, consultez [Configuration : lier un tableau à une classe](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="724e1-268">For more information, see [Configuration: Bind an array to a class](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span></span>

<span data-ttu-id="724e1-269">Les clés de Azure Key Vault ne peuvent pas utiliser un signe deux-points comme séparateur.</span><span class="sxs-lookup"><span data-stu-id="724e1-269">Azure Key Vault keys can't use a colon as a separator.</span></span> <span data-ttu-id="724e1-270">L’approche décrite dans cette rubrique utilise des doubles tirets (`--`) comme séparateur pour les valeurs hiérarchiques (sections).</span><span class="sxs-lookup"><span data-stu-id="724e1-270">The approach described in this topic uses double dashes (`--`) as a separator for hierarchical values (sections).</span></span> <span data-ttu-id="724e1-271">Les clés de tableau sont stockées en Azure Key Vault avec des doubles tirets et des segments de clé numériques (`--0--`, `--1--`, &hellip; `--{n}--`).</span><span class="sxs-lookup"><span data-stu-id="724e1-271">Array keys are stored in Azure Key Vault with double dashes and numeric key segments (`--0--`, `--1--`, &hellip; `--{n}--`).</span></span>

<span data-ttu-id="724e1-272">Examinez la configuration de fournisseur de journalisation [Serilog](https://serilog.net/) suivante fournie par un fichier JSON.</span><span class="sxs-lookup"><span data-stu-id="724e1-272">Examine the following [Serilog](https://serilog.net/) logging provider configuration provided by a JSON file.</span></span> <span data-ttu-id="724e1-273">Deux littéraux d’objet sont définis dans le tableau `WriteTo` qui reflètent deux *récepteurs*Serilog, qui décrivent les destinations pour la sortie de journalisation :</span><span class="sxs-lookup"><span data-stu-id="724e1-273">There are two object literals defined in the `WriteTo` array that reflect two Serilog *sinks*, which describe destinations for logging output:</span></span>

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

<span data-ttu-id="724e1-274">La configuration indiquée dans le fichier JSON précédent est stockée dans Azure Key Vault à l’aide de la notation à deux tirets (`--`) et de segments numériques :</span><span class="sxs-lookup"><span data-stu-id="724e1-274">The configuration shown in the preceding JSON file is stored in Azure Key Vault using double dash (`--`) notation and numeric segments:</span></span>

| <span data-ttu-id="724e1-275">Touche</span><span class="sxs-lookup"><span data-stu-id="724e1-275">Key</span></span> | <span data-ttu-id="724e1-276">valeur</span><span class="sxs-lookup"><span data-stu-id="724e1-276">Value</span></span> |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a><span data-ttu-id="724e1-277">Recharger les secrets</span><span class="sxs-lookup"><span data-stu-id="724e1-277">Reload secrets</span></span>

<span data-ttu-id="724e1-278">Les secrets sont mis en cache jusqu’à ce que `IConfigurationRoot.Reload()` soit appelée.</span><span class="sxs-lookup"><span data-stu-id="724e1-278">Secrets are cached until `IConfigurationRoot.Reload()` is called.</span></span> <span data-ttu-id="724e1-279">Les secrets expirés, désactivés et mis à jour dans le coffre de clés ne sont pas respectés par l’application tant que `Reload` n’est pas exécutée.</span><span class="sxs-lookup"><span data-stu-id="724e1-279">Expired, disabled, and updated secrets in the key vault are not respected by the app until `Reload` is executed.</span></span>

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a><span data-ttu-id="724e1-280">Secrets désactivés et arrivés à expiration</span><span class="sxs-lookup"><span data-stu-id="724e1-280">Disabled and expired secrets</span></span>

<span data-ttu-id="724e1-281">Les secrets désactivés et arrivés à expiration lèvent une <xref:Microsoft.Azure.KeyVault.Models.KeyVaultErrorException>.</span><span class="sxs-lookup"><span data-stu-id="724e1-281">Disabled and expired secrets throw a <xref:Microsoft.Azure.KeyVault.Models.KeyVaultErrorException>.</span></span> <span data-ttu-id="724e1-282">Pour empêcher l’application de se déclencher, fournissez la configuration à l’aide d’un autre fournisseur de configuration ou mettez à jour le secret désactivé ou expiré.</span><span class="sxs-lookup"><span data-stu-id="724e1-282">To prevent the app from throwing, provide the configuration using a different configuration provider or update the disabled or expired secret.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="724e1-283">Résoudre les problèmes</span><span class="sxs-lookup"><span data-stu-id="724e1-283">Troubleshoot</span></span>

<span data-ttu-id="724e1-284">Lorsque l’application ne parvient pas à charger la configuration à l’aide du fournisseur, un message d’erreur est écrit dans l' [infrastructure de journalisation ASP.net Core](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="724e1-284">When the app fails to load configuration using the provider, an error message is written to the [ASP.NET Core Logging infrastructure](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="724e1-285">Les conditions suivantes empêchent le chargement de la configuration :</span><span class="sxs-lookup"><span data-stu-id="724e1-285">The following conditions will prevent configuration from loading:</span></span>

* <span data-ttu-id="724e1-286">L’application ou le certificat n’est pas configuré correctement dans Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="724e1-286">The app or certificate isn't configured correctly in Azure Active Directory.</span></span>
* <span data-ttu-id="724e1-287">Le coffre de clés n’existe pas dans Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="724e1-287">The key vault doesn't exist in Azure Key Vault.</span></span>
* <span data-ttu-id="724e1-288">L’application n’est pas autorisée à accéder au coffre de clés.</span><span class="sxs-lookup"><span data-stu-id="724e1-288">The app isn't authorized to access the key vault.</span></span>
* <span data-ttu-id="724e1-289">La stratégie d’accès n’inclut pas les autorisations `Get` et `List`.</span><span class="sxs-lookup"><span data-stu-id="724e1-289">The access policy doesn't include `Get` and `List` permissions.</span></span>
* <span data-ttu-id="724e1-290">Dans le coffre de clés, les données de configuration (paire nom-valeur) sont incorrectement nommées, manquantes, désactivées ou expirées.</span><span class="sxs-lookup"><span data-stu-id="724e1-290">In the key vault, the configuration data (name-value pair) is incorrectly named, missing, disabled, or expired.</span></span>
* <span data-ttu-id="724e1-291">L’application a un nom de coffre de clés (`KeyVaultName`) incorrect, un ID d’application Azure AD (`AzureADApplicationId`) ou une empreinte de certificat de Azure AD (`AzureADCertThumbprint`).</span><span class="sxs-lookup"><span data-stu-id="724e1-291">The app has the wrong key vault name (`KeyVaultName`), Azure AD Application Id (`AzureADApplicationId`), or Azure AD certificate thumbprint (`AzureADCertThumbprint`).</span></span>
* <span data-ttu-id="724e1-292">La clé de configuration (nom) est incorrecte dans l’application pour la valeur que vous essayez de charger.</span><span class="sxs-lookup"><span data-stu-id="724e1-292">The configuration key (name) is incorrect in the app for the value you're trying to load.</span></span>
* <span data-ttu-id="724e1-293">Lors de l’ajout de la stratégie d’accès pour l’application au coffre de clés, la stratégie a été créée, mais le bouton **Enregistrer** n’a pas été sélectionné dans l’interface utilisateur des **stratégies d’accès** .</span><span class="sxs-lookup"><span data-stu-id="724e1-293">When adding the access policy for the app to the key vault, the policy was created, but the **Save** button wasn't selected in the **Access policies** UI.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="724e1-294">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="724e1-294">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="724e1-295">Microsoft Azure : Key Vault</span><span class="sxs-lookup"><span data-stu-id="724e1-295">Microsoft Azure: Key Vault</span></span>](https://azure.microsoft.com/services/key-vault/)
* [<span data-ttu-id="724e1-296">Microsoft Azure : documentation Key Vault</span><span class="sxs-lookup"><span data-stu-id="724e1-296">Microsoft Azure: Key Vault Documentation</span></span>](/azure/key-vault/)
* [<span data-ttu-id="724e1-297">Génération et transfert de clés protégées par HSM pour Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="724e1-297">How to generate and transfer HSM-protected keys for Azure Key Vault</span></span>](/azure/key-vault/key-vault-hsm-protected-keys)
* [<span data-ttu-id="724e1-298">KeyVaultClient, classe</span><span class="sxs-lookup"><span data-stu-id="724e1-298">KeyVaultClient Class</span></span>](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* [<span data-ttu-id="724e1-299">Démarrage rapide : définir et récupérer un secret à partir de Azure Key Vault à l’aide d’une application Web .NET</span><span class="sxs-lookup"><span data-stu-id="724e1-299">Quickstart: Set and retrieve a secret from Azure Key Vault by using a .NET web app</span></span>](/azure/key-vault/quick-create-net)
* [<span data-ttu-id="724e1-300">Didacticiel : utilisation de Azure Key Vault avec une machine virtuelle Windows Azure dans .NET</span><span class="sxs-lookup"><span data-stu-id="724e1-300">Tutorial: How to use Azure Key Vault with Azure Windows Virtual Machine in .NET</span></span>](/azure/key-vault/tutorial-net-windows-virtual-machine)
