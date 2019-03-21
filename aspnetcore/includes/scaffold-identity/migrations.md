---
ms.openlocfilehash: 698a127120f7f52672ceb927ff24eb1b02f91521
ms.sourcegitcommit: 088e6744cd67a62f214f25146313a53949b17d35
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58320278"
---
Le code de base de données d’identité généré nécessite [Migrations Entity Framework Core](/ef/core/managing-schemas/migrations/). Créer une migration et mettre à jour de la base de données. Par exemple, exécutez les commandes suivantes :

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Dans Visual Studio **Console du Gestionnaire de Package**:

```PMC
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

```cli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

---

Le paramètre de nom de « CreateIdentitySchema » pour le `Add-Migration` commande est arbitraire. `"CreateIdentitySchema"` Décrit la migration.
