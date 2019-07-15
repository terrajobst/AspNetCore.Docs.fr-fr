<span data-ttu-id="d05f0-101">Le code de base de données d’identité généré nécessite [Migrations Entity Framework Core](/ef/core/managing-schemas/migrations/).</span><span class="sxs-lookup"><span data-stu-id="d05f0-101">The generated Identity database code requires [Entity Framework Core Migrations](/ef/core/managing-schemas/migrations/).</span></span> <span data-ttu-id="d05f0-102">Créer une migration et mettre à jour de la base de données.</span><span class="sxs-lookup"><span data-stu-id="d05f0-102">Create a migration and update the database.</span></span> <span data-ttu-id="d05f0-103">Par exemple, exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="d05f0-103">For example, run the following commands:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d05f0-104">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d05f0-104">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d05f0-105">Dans Visual Studio **Console du Gestionnaire de Package**:</span><span class="sxs-lookup"><span data-stu-id="d05f0-105">In the Visual Studio **Package Manager Console**:</span></span>

```PMC
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d05f0-106">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="d05f0-106">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

---

<span data-ttu-id="d05f0-107">Le paramètre de nom de « CreateIdentitySchema » pour le `Add-Migration` commande est arbitraire.</span><span class="sxs-lookup"><span data-stu-id="d05f0-107">The "CreateIdentitySchema" name parameter for the `Add-Migration` command is arbitrary.</span></span> <span data-ttu-id="d05f0-108">`"CreateIdentitySchema"` Décrit la migration.</span><span class="sxs-lookup"><span data-stu-id="d05f0-108">`"CreateIdentitySchema"` describes the migration.</span></span>
