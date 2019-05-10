Exécutez le Générateur de modèles automatique identité :

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* À partir de **l’Explorateur de solutions**, avec le bouton droit sur le projet > **ajouter** > **nouvel élément structuré**.
* Dans le volet gauche de la **ajouter une structure** boîte de dialogue, sélectionnez **identité** > **ajouter**.
* Dans le **identité ADD** boîte de dialogue, sélectionnez les options souhaitées.
  * Sélectionnez votre page de disposition existante, ou votre fichier de disposition est remplacée par balisage incorrect. Par exemple `~/Pages/Shared/_Layout.cshtml` pour les Pages Razor `~/Views/Shared/_Layout.cshtml` pour les projets MVC
  * Sélectionnez le **+** bouton pour créer un nouveau **classe de contexte de données**.
* Sélectionnez **ajouter**.

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

Si vous n’avez pas encore installé le Générateur de modèles automatique ASP.NET Core, vous pouvez l’installer maintenant :

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Ajouter une référence de package à [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) au projet (\*.csproj) fichier. Dans le répertoire du projet, exécutez la commande suivante :

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Exécutez la commande suivante pour répertorier les options de génération de modèles automatique d’identité :

```cli
dotnet aspnet-codegenerator identity -h
```

Dans le dossier du projet, exécutez le Générateur de modèles automatique identité avec les options souhaitées. Par exemple, pour configurer l’identité avec l’interface utilisateur par défaut et le nombre minimal de fichiers, exécutez la commande suivante :

```cli
dotnet aspnet-codegenerator identity --useDefaultUI
```

---
