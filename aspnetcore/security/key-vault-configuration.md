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
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a>Fournisseur de Configuration d’Azure Key Vault dans ASP.NET Core

Par [Luke Latham](https://github.com/guardrex) et [Andrew Stanton-Nurse](https://github.com/anurse)

Ce document explique comment utiliser le [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) fournisseur de Configuration à charger des valeurs de configuration d’application de secrets Azure Key Vault. Azure Key Vault est un service basé sur le cloud qui aide à protéger les clés de chiffrement et les secrets utilisés par les applications et services. Scénarios courants d’utilisation d’Azure Key Vault avec les applications ASP.NET Core sont les suivantes :

* Contrôler l’accès aux données de configuration sensibles.
* Répondre aux exigences pour FIPS 140-2 de niveau 2 validé des Modules de sécurité matériel (HSM) lors du stockage des données de configuration.

Ce scénario est disponible pour les applications qui ciblent ASP.NET Core 2.1 ou version ultérieure.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="packages"></a>Packages

Pour utiliser le fournisseur de Configuration de coffre de clés Azure, ajoutez une référence de package pour le [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) package.

À adopter le [gérés d’identités pour les ressources Azure](/azure/active-directory/managed-identities-azure-resources/overview) scénario, ajoutez une référence de package à la [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) package.

> [!NOTE]
> Au moment de l’écriture, la dernière version stable de `Microsoft.Azure.Services.AppAuthentication`, version `1.0.3`, prend en charge [attribué par le système géré identités](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka). Prise en charge de *affectée à l’utilisateur géré identités* est disponible dans le `1.2.0-preview2` package. Cette rubrique illustre l’utilisation d’identités gérés par le système, et l’application exemple fourni utilise la version `1.0.3` de la `Microsoft.Azure.Services.AppAuthentication` package.

## <a name="sample-app"></a>Exemple d’application

L’exemple d’application s’exécute dans deux modes déterminé par le `#define` instruction en haut de la *Program.cs* fichier :

* `Certificate` &ndash; Illustre l’utilisation d’un certificat X.509 et les ID de Client Azure Key Vault à des secrets d’accès stockées dans Azure Key Vault. Cette version de l’exemple peut être exécutée à partir de n’importe quel emplacement, déployé sur Azure App Service ou n’importe quel hôte peut être utilisée par une application ASP.NET Core.
* `Managed` &ndash; Montre comment utiliser [gérés d’identités pour les ressources Azure](/azure/active-directory/managed-identities-azure-resources/overview) pour authentifier l’application dans Azure Key Vault avec l’authentification Azure AD sans informations d’identification stockées dans le code ou la configuration de l’application. Lorsque vous utilisez des identités gérées pour s’authentifier, un ID d’Application Azure AD et un mot de passe (clé secrète Client) ne sont pas nécessaires. Le `Managed` version de l’exemple doit être déployée sur Azure. Suivez les instructions de la [utiliser les identités gérés pour les ressources Azure](#use-managed-identities-for-azure-resources) section.

Pour plus d’informations sur la façon de configurer un exemple d’application à l’aide de directives de préprocesseur (`#define`), consultez <xref:index#preprocessor-directives-in-sample-code>.

## <a name="secret-storage-in-the-development-environment"></a>Stockage secret dans l’environnement de développement

Définir des secrets localement à l’aide de la [outil Secret Manager](xref:security/app-secrets). Lors de l’exemple d’application s’exécute sur l’ordinateur local dans l’environnement de développement, les secrets sont chargés à partir du magasin Secret Manager local.

L’outil Secret Manager nécessite un `<UserSecretsId>` propriété dans le fichier de projet de l’application. Définissez la valeur de propriété (`{GUID}`) à n’importe quel GUID unique :

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

Les secrets sont créés en tant que paires nom-valeur. Utilisent des valeurs hiérarchiques (sections de configuration) un `:` (deux-points) comme séparateur dans [configuration d’ASP.NET Core](xref:fundamentals/configuration/index) des noms de clé.

Le Gestionnaire de Secret est utilisé à partir d’une invite de commandes ouverte à la racine du contenu du projet, où `{SECRET NAME}` est le nom et `{SECRET VALUE}` est la valeur :

```console
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

Exécutez les commandes suivantes dans une invite de commandes à partir de la racine du contenu du projet pour définir les secrets de l’exemple d’application :

```console
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

Lorsque ces secrets sont stockés dans Azure Key Vault dans le [stockage Secret dans l’environnement de Production avec Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section, le `_dev` suffixe est remplacé par `_prod`. Le suffixe fournit un indice visuel dans la sortie de l’application indiquant la source des valeurs de configuration.

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a>Stockage secret dans l’environnement de Production avec Azure Key Vault

Les instructions fournies par le [Guide de démarrage rapide : Définir et récupérer un secret dans Azure Key Vault à l’aide d’Azure CLI](/azure/key-vault/quick-create-cli) rubrique sont résumées ici pour créer un Azure Key Vault et stocker des secrets utilisés par l’exemple d’application. Reportez-vous à la rubrique pour plus d’informations.

1. Ouvrir Azure Cloud shell à l’aide de l’une des méthodes suivantes dans le [Azure portal](https://portal.azure.com/):

   * Sélectionnez **essayer** dans le coin supérieur droit d’un bloc de code. Utilisez la chaîne de recherche « Azure CLI » dans la zone de texte.
   * Ouvrez Cloud Shell dans votre navigateur avec la **lancer Cloud Shell** bouton.
   * Sélectionnez le **Cloud Shell** bouton dans le menu dans le coin supérieur droit du portail Azure.

   Pour plus d’informations, consultez [les Interface de ligne de commande (CLI) Azure](/cli/azure/) et [vue d’ensemble d’Azure Cloud Shell](/azure/cloud-shell/overview).

1. Si vous n’êtes pas déjà authentifié, connectez-vous avec le `az login` commande.

1. Créer un groupe de ressources avec la commande suivante, où `{RESOURCE GROUP NAME}` correspond au nom du groupe de ressources pour le nouveau groupe de ressources et `{LOCATION}` est la région Azure (centre de données) :

   ```console
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. Créer un coffre de clés dans le groupe de ressources avec la commande suivante, où `{KEY VAULT NAME}` est le nom du nouveau coffre de clés et `{LOCATION}` est la région Azure (centre de données) :

   ```console
   az keyvault create --name "{KEY VAULT NAME}" --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. Créer des clés secrètes dans le coffre de clés en tant que paires nom-valeur.

   Les noms de secrets Azure Key Vault sont limités à des caractères alphanumériques et des tirets. Pour des valeurs hiérarchiques (sections de configuration), utilisez `--` (deux tirets) comme séparateur. Deux-points, qui sont normalement utilisés pour délimiter une section à partir d’une sous-clé dans [configuration d’ASP.NET Core](xref:fundamentals/configuration/index), ne sont pas autorisés dans les noms de clé secrète de coffre de clés. Par conséquent, les deux tirets sont remplacés par deux points lorsque les clés secrètes sont chargées dans la configuration de l’application.

   Les secrets suivantes sont pour une utilisation avec l’exemple d’application. Les valeurs incluent un `_prod` suffixe pour les distinguer le `_dev` suffixe des valeurs chargées dans l’environnement de développement à partir de Secrets de l’utilisateur. Remplacez `{KEY VAULT NAME}` par le nom du coffre de clés que vous avez créé à l’étape précédente :

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-client-secret-for-non-azure-hosted-apps"></a>Utiliser des ID d’Application et clé secrète Client pour les applications non-Azure-hébergé

Configurer Azure AD, Azure Key Vault et l’application à utiliser un ID d’Application Azure Active Directory et le X.509 de certificat pour s’authentifier auprès d’un coffre de clés **lorsque l’application est hébergée en dehors d’Azure**. Pour plus d’informations, consultez [sur les clés, secrets et certificats](/azure/key-vault/about-keys-secrets-and-certificates).

> [!NOTE]
> À l’aide d’un certificat X.509 et les ID d’Application est pris en charge pour les applications hébergées dans Azure, nous vous recommandons d’utiliser [gérés d’identités pour les ressources Azure](#use-managed-identities-for-azure-resources) lors de l’hébergement d’une application dans Azure. Les identités ne nécessitent pas le stockage d’un certificat dans l’application ou dans l’environnement de développement.

L’exemple d’application utilise un ID d’Application et de certificat X.509 quand le `#define` instruction en haut de la *Program.cs* fichier est défini sur `Certificate`.

1. Inscrire l’application avec Azure AD (**inscriptions**).
1. Télécharger la clé publique :
   1. Sélectionnez l’application dans Azure AD.
   1. Accédez à **paramètres** > **clés**.
   1. Sélectionnez **charger la clé publique** pour charger le certificat, qui contient la clé publique. En plus d’utiliser un *.cer*, *.pem*, ou *.crt* certificat, un *.pfx* certificat peut être chargé.
1. Store le nom de coffre de clés et l’ID de l’Application dans l’application *appsettings.json* fichier. Placer le certificat à la racine de l’application ou dans le magasin de certificats de l’application&dagger;.
1. Accédez à **coffres de clés** dans le portail Azure.
1. Sélectionnez le coffre de clés que vous avez créé dans le [stockage Secret dans l’environnement de Production avec Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section.
1. Sélectionnez **stratégies d’accès**.
1. Sélectionnez **Ajouter nouveau**.
1. Sélectionnez **sélectionner le principal** et sélectionnez l’application inscrite par nom. Sélectionnez le **sélectionnez** bouton.
1. Ouvrez **autorisations du Secret** et fournir l’application avec **obtenir** et **liste** autorisations.
1. Sélectionnez **OK**.
1. Sélectionnez **Enregistrer**.
1. Déployer l’application.

&dagger;Dans l’exemple d’application, le certificat est directement consommé depuis le fichier de certificat physique à la racine de l’application en créant un nouveau `X509Certificate2` lors de l’appel `AddAzureKeyVault`. Une autre approche consiste à autoriser le système d’exploitation gérer le certificat. Pour plus d’informations, consultez le [autoriser le système d’exploitation gérer le certificat X.509](#allow-the-os-to-manage-the-x509-certificate) section.

Le `Certificate` exemple d’application obtient ses valeurs de configuration à partir de `IConfigurationRoot` avec le même nom que le nom de secret :

* Valeurs non hiérarchiques : La valeur de `SecretName` est obtenu avec `config["SecretName"]`.
* Valeurs hiérarchiques (sections) : Utilisez `:` la notation (deux-points) ou le `GetSection` méthode d’extension. Utilisez une des ces approches pour obtenir la valeur de configuration, :
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

Les appels de l’application `AddAzureKeyVault` avec les valeurs fournies par le *appsettings.json* fichier :

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet1&highlight=12-15)]

Exemples de valeurs :

* Nom du coffre de clés : `contosovault`
* ID d’application : `627e911e-43cc-61d4-992e-12db9c81b413`

*appsettings.json* :

[!code-json[](key-vault-configuration/sample/appsettings.json)]

Lorsque vous exécutez l’application, une page Web affiche les valeurs de secret chargés. Dans l’environnement de développement, chargement des valeurs secrètes avec le `_dev` suffixe. Dans l’environnement de Production, les valeurs de charge avec le `_prod` suffixe.

## <a name="use-managed-identities-for-azure-resources"></a>Utiliser des identités gérés pour les ressources Azure

**Une application déployée sur Azure** peuvent tirer parti de [gérés d’identités pour les ressources Azure](/azure/active-directory/managed-identities-azure-resources/overview), ce qui permet à l’application auprès d’Azure Key Vault à l’aide de l’authentification Azure AD sans informations d’identification (ID d’Application et Clé secrète Password/Client) stockées dans l’application.

L’exemple d’application utilise des identités de géré pour les ressources Azure lorsque le `#define` instruction en haut de la *Program.cs* fichier est défini sur `Managed`.

Entrez le nom du coffre dans l’application *appsettings.json* fichier. L’exemple d’application ne nécessite pas un ID d’Application et le mot de passe (clé secrète Client) lorsque la valeur la `Managed` version, vous pouvez donc ignorer ces entrées de configuration. L’application est déployée sur Azure et Azure authentifie l’application pour accéder à Azure Key Vault uniquement en utilisant le nom de coffre stockée dans le *appsettings.json* fichier.

Déployer l’exemple d’application dans Azure App Service.

Une application déployée sur Azure App Service est automatiquement inscrite auprès d’Azure AD lorsque le service est créé. Obtenir l’ID d’objet du déploiement pour une utilisation dans la commande suivante. L’ID d’objet est affiché dans le portail Azure sur le **identité** Panneau de configuration du Service d’application.

Fournir à l’interface CLI Azure et l’ID d’objet de l’application, l’application avec `list` et `get` autorisations d’accès au coffre de clés :

```console
az keyvault set-policy --name '{KEY VAULT NAME}' --object-id {OBJECT ID} --secret-permissions get list
```

**Redémarrez l’application** à l’aide d’Azure CLI, PowerShell ou le portail Azure.

L’exemple d’application :

* Crée une instance de la `AzureServiceTokenProvider` classe sans une chaîne de connexion. Lorsqu’une chaîne de connexion n’est pas fournie, le fournisseur tente d’obtenir un jeton d’accès d’identités gérés pour les ressources Azure.
* Un nouveau `KeyVaultClient` est créé avec le `AzureServiceTokenProvider` rappel jeton d’instance.
* Le `KeyVaultClient` instance est utilisée avec une implémentation par défaut de `IKeyVaultSecretManager` qui charge toutes les valeurs secrètes et remplace des tirets double (`--`) avec des deux-points (`:`) dans les noms de clé.

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet2&highlight=13-21)]

Lorsque vous exécutez l’application, une page Web affiche les valeurs de secret chargés. Dans l’environnement de développement, les valeurs secrètes ont la `_dev` suffixe, car elles sont proposées par les Secrets de l’utilisateur. Dans l’environnement de Production, les valeurs de charge avec le `_prod` suffixe, car elles sont proposées par Azure Key Vault.

Si vous recevez un `Access denied` erreur, vérifiez que l’application est inscrite auprès d’Azure AD et d’accéder au coffre de clés. Vérifiez que vous avez redémarré le service dans Azure.

## <a name="use-a-key-name-prefix"></a>Utiliser un préfixe de nom de la clé

`AddAzureKeyVault` Fournit une surcharge qui accepte une implémentation de `IKeyVaultSecretManager`, qui vous permet de contrôler comment les secrets de coffre sont converties en clés de configuration. Par exemple, vous pouvez implémenter l’interface pour charger les valeurs des secrets selon une valeur de préfixe que vous indiquez au démarrage de l’application. Cela vous permet, par exemple, de charger des secrets en fonction de la version de l’application.

> [!WARNING]
> N’utilisez pas de préfixes sur les secrets de coffre de clés pour placer les secrets de plusieurs applications dans le même coffre de clés ou pour placer des secrets d'environnement (par exemple, des secrets de *développement* / de *production*) dans le même coffre. Nous recommandons d'utiliser un coffre de clés distinct pour chaque application et chaque environnement de développement/production afin d’isoler les environnements d’application et ainsi de garantir le niveau de sécurité le plus élevé possible.

Dans l’exemple suivant, une clé secrète est établie dans la clé de coffre (et pour l’environnement de développement à l’aide de l’outil Secret Manager) pour `5000-AppSecret` (périodes ne sont pas autorisés dans les noms de clé secrète de coffre de clés). Ce secret représente une clé secrète d’application pour la version 5.0.0.0 de l’application. Pour une autre version de l’application, 5.1.0.0, un secret est ajouté à la clé de coffre (et à l’aide de l’outil Secret Manager) pour `5100-AppSecret`. Chaque version de l’application charge sa valeur secrète avec version dans sa configuration en tant que `AppSecret`, suppression dérivées de la version il charge le secret.

`AddAzureKeyVault` est appelée avec un personnalisé `IKeyVaultSecretManager`:

[!code-csharp[](key-vault-configuration/sample_snapshot/Program.cs?name=snippet1&highlight=22)]

Les valeurs de nom de coffre de clés, ID d’Application et le mot de passe (clé secrète Client) sont fournies par le *appsettings.json* fichier :

[!code-json[](key-vault-configuration/sample/appsettings.json)]

Exemples de valeurs :

* Nom du coffre de clés : `contosovault`
* ID d’application : `627e911e-43cc-61d4-992e-12db9c81b413`
* Mot de passe : `g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=`

Le `IKeyVaultSecretManager` implémentation réagit aux préfixes de version de secrets pour charger la clé secrète appropriée dans configuration :

[!code-csharp[](key-vault-configuration/sample_snapshot/Startup.cs?name=snippet1)]

La méthode `Load` est appelée par un algorithme de fourniture qui effectue une itération dans les secrets du coffre pour trouver ceux qui comportent le préfixe de la version. Quand un préfixe de version a été trouvé avec la méthode `Load`, l’algorithme utilise la méthode `GetKey` pour retourner le nom de configuration du nom du secret. Il supprime le préfixe de version du nom du secret et retourne le reste du nom du secret pour le charger dans les paires nom-valeur de configuration de l’application.

Lorsque cette approche est mise en œuvre :

1. Version de l’application spécifiée dans le fichier de projet de l’application. Dans l’exemple suivant, la version est définie sur `5.0.0.0`:

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. Vérifiez qu’un `<UserSecretsId>` propriété n’est présente dans le fichier de projet application, où `{GUID}` est un GUID fourni par l’utilisateur :

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   Enregistrer les secrets suivants localement avec le [outil Secret Manager](xref:security/app-secrets):

   ```console
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. Secrets sont enregistrés dans Azure Key Vault en utilisant les commandes Azure CLI suivantes :

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. Quand l’application est exécutée, les secrets de coffre de clés sont chargés. Le secret de chaîne pour `5000-AppSecret` est mis en correspondance avec la version spécifiée dans le fichier de projet application (`5.0.0.0`).

1. La version, `5000` (avec le tiret), est supprimé du nom de clé. Tout au long de l’application, la lecture de la configuration avec la clé `AppSecret` se charge de la valeur du secret.

1. Si la version de l’application est modifiée dans le fichier projet pour `5.1.0.0` et l’application est exécutée à nouveau, la valeur secrète retournée est `5.1.0.0_secret_value_dev` dans l’environnement de développement et `5.1.0.0_secret_value_prod` en Production.

> [!NOTE]
> Vous pouvez également fournir votre propre implémentation `KeyVaultClient` à `AddAzureKeyVault`. Un client personnalisé permet le partage d’une instance unique du client sur l’application.

## <a name="allow-the-os-to-manage-the-x509-certificate"></a>Autoriser le système d’exploitation gérer le certificat X.509

Le certificat X.509 peut être géré par le système d’exploitation. L’exemple suivant utilise le `AddAzureKeyVault` surcharge qui accepte un `X509Certificate2` à partir d’un empreinte numérique du certificat fourni par la configuration et de magasin de certificats utilisateur actuel de la machine :

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

Pour plus d’informations, consultez [authentifier avec un certificat au lieu d’une clé secrète Client](/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret).

## <a name="bind-an-array-to-a-class"></a>Lier un tableau à une classe

Le fournisseur est capable de lire les valeurs de configuration dans un tableau pour la liaison à un tableau POCO.

Lors de la lecture à partir d’une source de configuration qui permet des clés contenir le signe deux-points (`:`) séparateurs, un segment de clé numérique est utilisé pour distinguer les clés qui composent un tableau (`:0:`, `:1:`,... `:{n}:`). Pour plus d’informations, consultez [Configuration : Lier un tableau à une classe](xref:fundamentals/configuration/index#bind-an-array-to-a-class).

Clés d’Azure Key Vault ne peut pas utiliser un signe deux-points comme séparateur. L’approche décrite dans cette rubrique utilise les doubles tirets (`--`) comme séparateur pour les valeurs hiérarchiques (sections). Clés de tableau sont stockées dans Azure Key Vault avec double des tirets et des segments de clé numériques (`--0--`, `--1--`, &hellip; `--{n}--`).

Examinez les éléments suivants [Serilog](https://serilog.net/) configuration du fournisseur fournie par un fichier JSON de journalisation. Il existe deux objets littéraux définis dans le `WriteTo` tableau qui reflètent les deux Serilog *récepteurs*, qui décrivent des destinations pour la sortie de journalisation :

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

La configuration représentée dans le fichier JSON précédent est stockée dans Azure Key Vault à l’aide de double tiret (`--`) segments numériques et de notation :

| Touche | Value |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a>Recharger les secrets

Les secrets sont mis en cache jusqu'à ce que la méthode `IConfigurationRoot.Reload()` soit appelée. Arrivé à expiration, désactivé, et les secrets mis à jour dans le coffre de clés ne sont pas respectées par l’application jusqu'à ce que `Reload` est exécutée.

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a>Secrets désactivés et expirés

Les secrets désactivés et expirés lèvent une exception `KeyVaultClientException`. Pour empêcher votre application de lever cette exception, remplacez votre application ou mettez à jour le secret désactivé/expiré.

## <a name="troubleshoot"></a>Résoudre les problèmes

Lorsque l’application ne parvient pas à charger la configuration de l’utilisation du fournisseur, un message d’erreur est écrite à la [infrastructure de journalisation ASP.NET Core](xref:fundamentals/logging/index). Les conditions suivantes empêchent le chargement de la configuration :

* L’application n’est pas configurée correctement dans Azure Active Directory.
* Le coffre de clés n’existe pas dans Azure Key Vault.
* L’application n’est pas autorisée à accéder au coffre de clés.
* La stratégie d’accès n’inclut pas les autorisations `Get` et `List`.
* Dans le coffre de clés, les données de configuration (paire nom-valeur) sont manquantes, désactivées, expirées ou incorrectement nommées.
* L’application a le nom de coffre de clés incorrect (`KeyVaultName`), Id d’Application Azure AD (`AzureADApplicationId`), ou mot de passe Azure AD (clé secrète Client) (`AzureADPassword`).
* Le mot de passe Azure AD (clé secrète Client) (`AzureADPassword`) a expiré.
* La clé de configuration (nom) est incorrecte dans l’application pour la valeur que vous tentez de charger.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:fundamentals/configuration/index>
* [Microsoft Azure : Coffre de clés](https://azure.microsoft.com/services/key-vault/)
* [Microsoft Azure : Documentation Key Vault](/azure/key-vault/)
* [Pour générer et transférer protégée par HSM de clés pour Azure Key Vault](/azure/key-vault/key-vault-hsm-protected-keys)
* [Classe de KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* [Démarrage rapide : Définir et récupérer un secret dans Azure Key Vault à l’aide d’une application web .NET](/azure/key-vault/quick-create-net)
* [Tutoriel : Comment utiliser Azure Key Vault avec Azure Windows Virtual Machine dans .NET](/azure/key-vault/tutorial-net-windows-virtual-machine)
