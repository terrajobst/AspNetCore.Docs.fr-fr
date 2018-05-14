# <a name="key-vault-configuration-provider-sample-application-aspnet-core-2x"></a>Application d’exemple de fournisseur de Configuration de coffre de clés (ASP.NET Core 2.x)

Cet exemple illustre l’utilisation du fournisseur de Configuration Azure Key Vault pour ASP.NET Core 2.x. Pour l’exemple 1.x ASP.NET Core, consultez [application d’exemple de fournisseur de Configuration de coffre de clés (ASP.NET Core 1.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/1.x).

Pour plus d’informations sur le fonctionne de l’exemple, consultez la [fournisseur de configuration d’Azure Key Vault](xref:security/key-vault-configuration) rubrique.

## <a name="using-the-sample"></a>Utilisation de l'exemple
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
