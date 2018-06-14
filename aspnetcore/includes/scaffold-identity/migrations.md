<span data-ttu-id="d9701-101">Le code de base de données d’identité généré nécessite [Entity Framework Core Migrations](/ef/core/managing-schemas/migrations/).</span><span class="sxs-lookup"><span data-stu-id="d9701-101">The generated Identity database code requires [Entity Framework Core Migrations](/ef/core/managing-schemas/migrations/).</span></span> <span data-ttu-id="d9701-102">Créer une migration et mise à jour de la base de données.</span><span class="sxs-lookup"><span data-stu-id="d9701-102">Create a migration and update the database.</span></span> <span data-ttu-id="d9701-103">Par exemple, exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="d9701-103">For example, run the following commands:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d9701-104">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d9701-104">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d9701-105">Dans Visual Studio **Package Manager Console**:</span><span class="sxs-lookup"><span data-stu-id="d9701-105">In the Visual Studio **Package Manager Console**:</span></span>

```PMC
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d9701-106">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="d9701-106">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

------

<span data-ttu-id="d9701-107">Le paramètre de nom de « CreateIdentitySchema » pour le `Add-Migration` commande est arbitraire.</span><span class="sxs-lookup"><span data-stu-id="d9701-107">The "CreateIdentitySchema" name parameter for the `Add-Migration` command is arbitrary.</span></span> <span data-ttu-id="d9701-108">`"CreateIdentitySchema"` Décrit la migration.</span><span class="sxs-lookup"><span data-stu-id="d9701-108">`"CreateIdentitySchema"` describes the migration.</span></span>