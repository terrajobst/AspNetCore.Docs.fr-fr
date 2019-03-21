---
ms.openlocfilehash: 698a127120f7f52672ceb927ff24eb1b02f91521
ms.sourcegitcommit: 088e6744cd67a62f214f25146313a53949b17d35
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58320278"
---
<span data-ttu-id="46ddb-101">Le code de base de données d’identité généré nécessite [Migrations Entity Framework Core](/ef/core/managing-schemas/migrations/).</span><span class="sxs-lookup"><span data-stu-id="46ddb-101">The generated Identity database code requires [Entity Framework Core Migrations](/ef/core/managing-schemas/migrations/).</span></span> <span data-ttu-id="46ddb-102">Créer une migration et mettre à jour de la base de données.</span><span class="sxs-lookup"><span data-stu-id="46ddb-102">Create a migration and update the database.</span></span> <span data-ttu-id="46ddb-103">Par exemple, exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="46ddb-103">For example, run the following commands:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="46ddb-104">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="46ddb-104">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="46ddb-105">Dans Visual Studio **Console du Gestionnaire de Package**:</span><span class="sxs-lookup"><span data-stu-id="46ddb-105">In the Visual Studio **Package Manager Console**:</span></span>

```PMC
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="46ddb-106">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="46ddb-106">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

---

<span data-ttu-id="46ddb-107">Le paramètre de nom de « CreateIdentitySchema » pour le `Add-Migration` commande est arbitraire.</span><span class="sxs-lookup"><span data-stu-id="46ddb-107">The "CreateIdentitySchema" name parameter for the `Add-Migration` command is arbitrary.</span></span> <span data-ttu-id="46ddb-108">`"CreateIdentitySchema"` Décrit la migration.</span><span class="sxs-lookup"><span data-stu-id="46ddb-108">`"CreateIdentitySchema"` describes the migration.</span></span>
