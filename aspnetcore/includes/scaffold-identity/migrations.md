Le code de base de données d’identité généré requiert des [migrations de Entity Framework Core](/ef/core/managing-schemas/migrations/). Créer une migration et mettre à jour de la base de données. Par exemple, exécutez les commandes suivantes :

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

Dans la console du **Gestionnaire de package**Visual Studio :

```powershell
Install-Package Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-cli"></a>[CLI .NET Core](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

---

Le paramètre de nom « CreateIdentitySchema » de la commande `Add-Migration` est arbitraire. `"CreateIdentitySchema"` décrit la migration.
