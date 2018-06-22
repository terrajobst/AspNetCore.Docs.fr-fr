---
title: Fournisseur de configuration Azure Key Vault dans ASP.NET Core
author: guardrex
description: Découvrez comment utiliser le fournisseur de Configuration du coffre de clé Azure pour configurer une application à l’aide de paires nom-valeur chargées pendant l’exécution.
ms.author: riande
ms.date: 08/09/2017
uid: security/key-vault-configuration
ms.openlocfilehash: 433a9cee917a36ff7aa950dbe2a631d127c74821
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273435"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a>Fournisseur de configuration Azure Key Vault dans ASP.NET Core

Par [Luke Latham](https://github.com/guardrex) et [Andrew Stanton-infirmière](https://github.com/anurse)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Afficher ou télécharger l’exemple de code pour 2.x :

* [Exemple de base](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/2.x) ([comment télécharger](xref:tutorials/index#how-to-download-a-sample)) - lire les valeurs secrets dans une application.
* [Exemple de préfixe de nom de la clé](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/2.x) ([comment télécharger](xref:tutorials/index#how-to-download-a-sample)) - lire les valeurs secrets à l’aide d’un préfixe de nom de la clé qui représente la version d’une application, ce qui vous permet de charger un ensemble différent de valeurs de clé secrètes pour chaque version de l’application. 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Afficher ou télécharger l’exemple de code pour 1.x :

* [Exemple de base](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/1.x) ([comment télécharger](xref:tutorials/index#how-to-download-a-sample)) - lire les valeurs secrets dans une application.
* [Exemple de préfixe de nom de la clé](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/1.x) ([comment télécharger](xref:tutorials/index#how-to-download-a-sample)) - lire les valeurs secrets à l’aide d’un préfixe de nom de la clé qui représente la version d’une application, ce qui vous permet de charger un ensemble différent de valeurs de clé secrètes pour chaque version de l’application.

---

Ce document explique comment utiliser le [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) fournisseur de configuration pour charger des valeurs de configuration d’application de secrets de coffre de clés Azure. Azure Key Vault est un service cloud qui vous aident à protéger les clés de chiffrement et les secrets utilisés par les applications et services. Les scénarios courants incluent le contrôle d’accès aux données de configuration sensibles et répondre aux exigences de la norme FIPS 140-2 niveau 2 validé par les Modules de sécurité matériel (HSM) lors du stockage des données de configuration. Cette fonctionnalité est disponible pour les applications qui ciblent ASP.NET Core 1.1 ou ultérieure.

## <a name="package"></a>Package

Pour utiliser le fournisseur, ajouter la référence au package Nuget [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/).

## <a name="application-configuration"></a>Configuration d’application

Vous pouvez explorer le fournisseur avec les [exemples d’applications](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples). Une fois que vous établissez un coffre de clés et créez des clés secrètes dans le coffre, les exemples d’applications chargent en toute sécurité les valeurs de secret dans leurs configurations et les affichent dans les pages Web.

Le fournisseur est ajouté à la `ConfigurationBuilder`avec l'extention `AddAzureKeyVault`. Dans les exemples d’applications, l’extension utilise trois valeurs de configuration chargées à partir du fichier *appsettings.json*.

| Paramètre d’application    | Description                    | Exemple                                      |
| -------------- | ------------------------------ | -------------------------------------------- |
| `Vault`        | Nom de coffre de clés Azure           | contosovault                                 |
| `ClientId`     | Id d’application Azure Active Directory  | 627e911e-43cc-61d4-992e-12db9c81b413         |
| `ClientSecret` | Clé de l’application Azure Active Directory | g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di= |

[!code-csharp[Program](key-vault-configuration/samples/basic-sample/2.x/Program.cs?name=snippet1)]

## <a name="creating-key-vault-secrets-and-loading-configuration-values-basic-sample"></a>Création des clés secrètes du coffre de clés Azure Key Vault et chargement des valeurs de configuration (exemple de base)

1. Créer un coffre de clés et configurer Azure Active Directory (Azure AD) pour l’application en suivant les instructions de [prise en main d’Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
   * Ajoutez des clés secrètes au coffre de clés à l’aide du [Module PowerShell Azure Resource Manager Key Vault](/powershell/module/azurerm.keyvault) disponible à partir de disponible dans [Gallery PowerShell](https://www.powershellgallery.com/packages/AzureRM.KeyVault), l'[API REST du coffre de clés Azure Key Vault](/rest/api/keyvault/), ou le [Portail azure](https://portal.azure.com/). Les clés secrètes créées sont de type *Manual* ou *Certificate*. Les clés secrètes *Certificate* sont des certificats pour les applications et les services, mais elles ne sont pas prises en charge par le fournisseur de configuration. Vous devez utiliser l'option *Manual* afin de créer des clés secrètes de paire nom-valeur pour le fournisseur de configuration.
     * Les secrets simples sont créées en tant que paires nom-valeur. Les noms de secret de coffre de clés Azure sont limités à des caractères alphanumériques et des tirets.
     * Les valeurs hiérarchiques (sections de configuration) utilisent `--` (deux tirets) comme séparateur dans l’exemple. Les deux points `:`, normalement utilisés pour délimiter une section de sous-clé dans [configuration d’ASP.NET Core](xref:fundamentals/configuration/index), ne sont pas autorisés dans les noms de clés secrètes. Par conséquent, les deux tirets sont remplacés par deux points lorsque les clés secrètes sont chargées dans la configuration de l’application.
     * Créez deux clés secrètes *Manual* avec les paires nom-valeur suivantes : la première clé secrète est une simple paire nom-valeur, et la seconde crée une valeur de clé secrète avec une section et une sous-clé dans le nom de la clé secrète :
       * `SecretName`: `secret_value_1`
       * `Section--SecretName`: `secret_value_2`
   * Inscrire l’exemple d’application dans Azure Active Directory.
   * Autoriser l’application à accéder au coffre de clés. Lorsque vous utilisez l’applet de commande PowerShell `Set-AzureRmKeyVaultAccessPolicy` pour autoriser l’application à accéder au coffre de clés, vous pouvez obtenir la liste des clés secrètes avec `List` et `Get` avec `-PermissionsToSecrets list,get`.

2. Mettre à jour le fichier *appsettings.json* de l’application avec les valeurs de `Vault`, `ClientId`, et `ClientSecret`.
3. Exécutez l’exemple d’application, qui obtient ses valeurs de configuration à partir de `IConfigurationRoot` avec le même nom que le nom secret.
   * Valeurs non hiérarchique : la valeur de `SecretName` est obtenu avec `config["SecretName"]`.
   * Valeurs hiérarchiques (sections) : utilisez la notation `:` (deux points) ou la méthode d’extension `GetSection`. Utilisez une des ces approches pour obtenir la valeur de configuration, :
     * `config["Section:SecretName"]`
     * `config.GetSection("Section")["SecretName"]`

Lorsque vous exécutez l’application, une page Web affiche les valeurs de secret principal chargés :

![Fenêtre de navigateur affichant les valeurs des secrets chargée via le fournisseur de Configuration Azure Key Vault](key-vault-configuration/_static/sample1.png)

## <a name="creating-prefixed-key-vault-secrets-and-loading-configuration-values-key-name-prefix-sample"></a>Créer des clés secrètes de coffre de clés avec préfixe et charger des valeurs de configuration (clé-nom-préfixe-exemple)

`AddAzureKeyVault` offre également une surcharge qui accepte une implémentation de `IKeyVaultSecretManager`, ce qui permet de choisir comment les secrets du coffre de clés sont convertis en clés de configuration. Par exemple, vous pouvez implémenter l’interface pour charger les valeurs des secrets selon une valeur de préfixe que vous indiquez au démarrage de l’application. Cela vous permet, par exemple, de charger des secrets en fonction de la version de l’application.

> [!WARNING]
> N’utilisez pas de préfixes sur les secrets de coffre de clés pour placer les secrets de plusieurs applications dans le même coffre de clés ou pour placer des secrets d'environnement (par exemple, des secrets de *développement* / de *production*) dans le même coffre. Nous recommandons d'utiliser un coffre de clés distinct pour chaque application et chaque environnement de développement/production afin d’isoler les environnements d’application et ainsi de garantir le niveau de sécurité le plus élevé possible.

À l’aide du deuxième exemple d’application, vous créez une clé secrète dans le coffre de clés pour `5000-AppSecret` (les points ne sont pas autorisés dans le nom des secrets du coffre de clés), représentant le secret de la version 5.0.0.0 de votre application. Dans le cas d’une autre version, 5.1.0.0, vous créez un secret pour `5100-AppSecret`. Chaque version de l’application charge sa propre valeur de secret dans sa configuration en tant que `AppSecret`, en extrayant la version lors du chargement du secret. L'implémentation de l’exemple est illustrée ci-dessous :

[!code-csharp[Configuration builder](key-vault-configuration/samples/key-name-prefix-sample/2.x/Program.cs?name=snippet1&highlight=20)]

[!code-csharp[PrefixKeyVaultSecretManager](key-vault-configuration/samples/key-name-prefix-sample/2.x/Startup.cs?name=snippet1)]

La méthode `Load` est appelée par un algorithme de fourniture qui effectue une itération dans les secrets du coffre pour trouver ceux qui comportent le préfixe de la version. Quand un préfixe de version a été trouvé avec la méthode `Load`, l’algorithme utilise la méthode `GetKey` pour retourner le nom de configuration du nom du secret. Il supprime le préfixe de version du nom du secret et retourne le reste du nom du secret pour le charger dans les paires nom-valeur de configuration de l’application.

Lorsque vous implémentez cette approche :

1. Les clés secrètes du coffre de clés sont chargées.
2. Le secret de chaîne pour `5000-AppSecret` est mis en correspondance.
3. La version, `5000` (avec le tiret), est supprimé sur le nom de clé en laissant `AppSecret` pour charger avec la valeur de secret principal dans la configuration de l’application.

> [!NOTE]
> Vous pouvez également fournir votre propre implémentation `KeyVaultClient` à `AddAzureKeyVault`. Fournir un client personnalisé permet de partager une seule instance du client entre le fournisseur de configuration et d’autres parties de l’application.

1. Créer un coffre de clés et configurer Azure Active Directory (Azure AD) pour l’application en suivant les instructions de [prise en main d’Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
   * Ajoutez des clés secrètes au coffre de clés à l’aide du [Module PowerShell Azure Resource Manager Key Vault](/powershell/module/azurerm.keyvault) disponible à partir de disponible dans [Gallery PowerShell](https://www.powershellgallery.com/packages/AzureRM.KeyVault), l'[API REST du coffre de clés Azure Key Vault](/rest/api/keyvault/), ou le [Portail azure](https://portal.azure.com/). Les clés secrètes créées sont de type *Manual* ou *Certificate*. Les clés secrètes *Certificate* sont des certificats pour les applications et les services, mais elles ne sont pas prises en charge par le fournisseur de configuration. Vous devez utiliser l'option *Manual* afin de créer des clés secrètes de paire nom-valeur pour le fournisseur de configuration.
     * Pour des valeurs hiérarchiques (sections de configuration), utilisez `--` (deux tirets) comme séparateur.
     * Créez deux secrets de type *manuel* avec les paires nom-valeur suivantes :
       * `5000-AppSecret`: `5.0.0.0_secret_value`
       * `5100-AppSecret`: `5.1.0.0_secret_value`
   * Inscrire l’exemple d’application dans Azure Active Directory.
   * Autoriser l’application à accéder au coffre de clés. Lorsque vous utilisez l’applet de commande PowerShell `Set-AzureRmKeyVaultAccessPolicy` pour autoriser l’application à accéder au coffre de clés, vous pouvez obtenir la liste des clés secrètes avec `List` et `Get` avec `-PermissionsToSecrets list,get`.

2. Mettre à jour le fichier *appsettings.json* de l’application avec les valeurs de `Vault`, `ClientId`, et `ClientSecret`.
3. Exécutez l’exemple d’application, qui obtient ses valeurs de configuration à partir de `IConfigurationRoot` avec le même nom que le nom secret. Dans cet exemple, le préfixe est la version de l’application que vous avez fournie pour `PrefixKeyVaultSecretManager` quand vous avez ajouté le fournisseur de configuration du coffre de clés Azure. La valeur de `AppSecret` est obtenu avec `config["AppSecret"]`. La page Web générée par l’application affiche la valeur chargée :

   ![Fenêtre de navigateur indiquant une valeur secrète chargée via le fournisseur de Configuration Azure Key Vault lors de la version de l’application est 5.0.0.0](key-vault-configuration/_static/sample2-1.png)

4. Modifier la version de l’assembly de l’application dans le fichier de projet à partir de `5.0.0.0` à `5.1.0.0` et réexécutez l’application. Cette fois, la valeur secrète retournée est `5.1.0.0_secret_value`. La page Web générée par l’application affiche la valeur chargée :

   ![Fenêtre de navigateur indiquant une valeur secrète chargée via le fournisseur de Configuration Azure Key Vault lors de la version de l’application est 5.1.0.0](key-vault-configuration/_static/sample2-2.png)

## <a name="controlling-access-to-the-clientsecret"></a>Contrôle l’accès à la ClientSecret

Utilisez le [Gestionnaire Secret](xref:security/app-secrets) pour maintenir le `ClientSecret` en dehors de l’arborescence du code source de votre projet. Avec le Gestionnaire de clé secrète, vous associez des secrets de l’application à un projet spécifique et les partagez entre plusieurs projets.

Lorsque vous développez une application avec le Framework .NET dans un environnement qui prend en charge les certificats, vous pouvez vous authentifier à Azure Key Vault avec un certificat X.509. La clé privée du certificat X.509 est gérée par le système d’exploitation. Pour plus d’informations, consultez [authentifier avec un certificat au lieu d’une clé secrète Client](https://docs.microsoft.com/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret). Utilisez la surcharge `AddAzureKeyVault` qui accepte un `X509Certificate2`.

```csharp
var store = new X509Store(StoreLocation.CurrentUser);
store.Open(OpenFlags.ReadOnly);
var cert = store.Certificates.Find(X509FindType.FindByThumbprint, config["CertificateThumbprint"], false);

builder.AddAzureKeyVault(
    config["Vault"],
    config["ClientId"],
    cert.OfType<X509Certificate2>().Single(),
    new EnvironmentSecretManager(env.ApplicationName));
store.Close();

Configuration = builder.Build();
```

## <a name="reloading-secrets"></a>Rechargement des clés secrètes

Les secrets sont mis en cache jusqu'à ce que la méthode `IConfigurationRoot.Reload()` soit appelée. Les secrets expirés, désactivés ou mis à jour dans le coffre de clés ne sont pas respectés par l’application tant que la méthode `Reload` n'est pas exécutée.

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a>Secrets désactivés et expirés

Les secrets désactivés et expirés lèvent une exception `KeyVaultClientException`. Pour empêcher votre application de lever cette exception, remplacez votre application ou mettez à jour le secret désactivé/expiré.

## <a name="troubleshooting"></a>Résolution des problèmes

Quand l’application ne parvient pas à charger la configuration à l'aide du fournisseur, un message d’erreur est écrit dans l'[infrastructure de journalisation ASP.NET](xref:fundamentals/logging/index). Les conditions suivantes empêchent le chargement de la configuration :

* L’application n’est pas configurée correctement dans Azure Active Directory.
* Le coffre de clés n’existe pas dans le coffre de clés Azure.
* L’application n’est pas autorisée à accéder au coffre de clés.
* La stratégie d’accès n’inclut pas les autorisations `Get` et `List`.
* Dans le coffre de clés, les données de configuration (paire nom-valeur) sont manquantes, désactivées, expirées ou incorrectement nommées.
* L’application a le nom de coffre de clés incorrect (`Vault`), Id d’application Azure AD (`ClientId`), ou la clé Azure AD (`ClientSecret`).
* La clé d’Azure AD (`ClientSecret`) a expiré.
* La clé de configuration (nom) est incorrecte dans l’application pour la valeur que vous essayez de charger.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Configuration](xref:fundamentals/configuration/index)
* [Microsoft Azure : Coffre de clés](https://azure.microsoft.com/services/key-vault/)
* [Microsoft Azure : Documentation de coffre de clés](https://docs.microsoft.com/azure/key-vault/)
* [Comment générer et transférer protégée par HSM clés pour le coffre de clés Azure](https://docs.microsoft.com/azure/key-vault/key-vault-hsm-protected-keys)
* [Classe de KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
