Exécutez les commandes CLI .NET Core suivantes :

```dotnetcli
dotnet tool install --global dotnet-ef
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

Les commandes précédentes ajoutent :

* L' [outil de génération de modèles automatique ASPNET-CodeGenerator](xref:fundamentals/tools/dotnet-aspnet-codegenerator).
* Les outils de Entity Framework Core pour le CLI .NET Core.
* Le fournisseur EF Core SQLite, qui installe le package EF Core comme une dépendance.
* Les packages nécessaires à la génération de modèles automatique : `Microsoft.VisualStudio.Web.CodeGeneration.Design` et `Microsoft.EntityFrameworkCore.SqlServer`.

Pour obtenir des conseils sur la configuration de plusieurs environnements qui permet à une application de configurer ses contextes de base de données par environnement, consultez <xref:fundamentals/environments#environment-based-startup-class-and-methods>.

[!INCLUDE[](~/includes/scaffoldTFM.md)]