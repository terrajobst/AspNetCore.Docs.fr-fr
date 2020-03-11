<span data-ttu-id="2a7c8-101">Le code de base de données d’identité généré requiert des [migrations de Entity Framework Core](/ef/core/managing-schemas/migrations/).</span><span class="sxs-lookup"><span data-stu-id="2a7c8-101">The generated Identity database code requires [Entity Framework Core Migrations](/ef/core/managing-schemas/migrations/).</span></span> <span data-ttu-id="2a7c8-102">Créer une migration et mettre à jour de la base de données.</span><span class="sxs-lookup"><span data-stu-id="2a7c8-102">Create a migration and update the database.</span></span> <span data-ttu-id="2a7c8-103">Par exemple, exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="2a7c8-103">For example, run the following commands:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="2a7c8-104">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2a7c8-104">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="2a7c8-105">Dans la console du **Gestionnaire de package**Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="2a7c8-105">In the Visual Studio **Package Manager Console**:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-cli"></a>[<span data-ttu-id="2a7c8-106">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="2a7c8-106">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

---

<span data-ttu-id="2a7c8-107">Le paramètre de nom « CreateIdentitySchema » de la commande `Add-Migration` est arbitraire.</span><span class="sxs-lookup"><span data-stu-id="2a7c8-107">The "CreateIdentitySchema" name parameter for the `Add-Migration` command is arbitrary.</span></span> <span data-ttu-id="2a7c8-108">`"CreateIdentitySchema"` décrit la migration.</span><span class="sxs-lookup"><span data-stu-id="2a7c8-108">`"CreateIdentitySchema"` describes the migration.</span></span>
