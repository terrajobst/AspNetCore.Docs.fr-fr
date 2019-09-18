<span data-ttu-id="68a3d-101">Le code de base de données d’identité généré requiert des [migrations de Entity Framework Core](/ef/core/managing-schemas/migrations/).</span><span class="sxs-lookup"><span data-stu-id="68a3d-101">The generated Identity database code requires [Entity Framework Core Migrations](/ef/core/managing-schemas/migrations/).</span></span> <span data-ttu-id="68a3d-102">Créer une migration et mettre à jour de la base de données.</span><span class="sxs-lookup"><span data-stu-id="68a3d-102">Create a migration and update the database.</span></span> <span data-ttu-id="68a3d-103">Par exemple, exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="68a3d-103">For example, run the following commands:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="68a3d-104">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="68a3d-104">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="68a3d-105">Dans Visual Studio **Console du Gestionnaire de Package**:</span><span class="sxs-lookup"><span data-stu-id="68a3d-105">In the Visual Studio **Package Manager Console**:</span></span>

```powershell
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="68a3d-106">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="68a3d-106">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

---

<span data-ttu-id="68a3d-107">Le paramètre de nom « CreateIdentitySchema » de `Add-Migration` la commande est arbitraire.</span><span class="sxs-lookup"><span data-stu-id="68a3d-107">The "CreateIdentitySchema" name parameter for the `Add-Migration` command is arbitrary.</span></span> <span data-ttu-id="68a3d-108">`"CreateIdentitySchema"`décrit la migration.</span><span class="sxs-lookup"><span data-stu-id="68a3d-108">`"CreateIdentitySchema"` describes the migration.</span></span>
