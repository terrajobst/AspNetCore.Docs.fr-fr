Le code de base de données d’identité généré nécessite [Entity Framework Core Migrations](/ef/core/managing-schemas/migrations/). Créer une migration et mise à jour de la base de données. Par exemple, exécutez les commandes suivantes :

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Dans Visual Studio **Package Manager Console**:

```PMC
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

```cli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

------

Le paramètre de nom de « CreateIdentitySchema » pour le `Add-Migration` commande est arbitraire. `"CreateIdentitySchema"` Décrit la migration.