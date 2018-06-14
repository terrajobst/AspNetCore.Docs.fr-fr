Exécutez le scaffolder d’identité :

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* À partir de **l’Explorateur de solutions**, avec le bouton droit sur le projet > **ajouter** > **un nouvel élément structuré**.
* Dans le volet gauche de la **ajouter une vue de structure** boîte de dialogue, sélectionnez **identité** > **ajouter**.
* Dans le **ajouter identité** boîte de dialogue, sélectionnez les options souhaitées.
  * Sélectionnez votre page de mise en page existante ou votre fichier de mise en page est écrasé par un balisage incorrect. Lorsqu’un fichier _Layout.cshtml existant est sélectionné, il est **pas** remplacé.

 Par exemple `~/Pages/Shared/_Layout.cshtml` des Pages Razor `~/Views/Shared/_Layout.cshtml` pour les projets MVC
* Pour utiliser votre contexte de données existant, sélectionnez au moins un fichier à remplacer. Vous devez sélectionner au moins un fichier pour ajouter votre contexte de données.
  * Sélectionnez votre classe de contexte de données.
  * Sélectionnez **ajouter**.
* Pour créer un contexte utilisateur et éventuellement créer une classe d’utilisateur personnalisées pour l’identité :
  * Sélectionnez le **+** bouton permettant de créer un nouveau **classe de contexte de données**.
  * Sélectionnez **ajouter**.

Remarque : Si vous créez un nouveau contexte de l’utilisateur, il est inutile de sélectionner un fichier à remplacer.

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

Si vous n’avez pas encore installé le scaffolder ASP.NET, vous pouvez l’installer maintenant :

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Ajouter une référence de package à [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) au projet (\*.csproj) fichier. Dans le répertoire du projet, exécutez la commande suivante :

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Exécutez la commande suivante pour répertorier les options de scaffolder d’identité :

```cli
dotnet aspnet-codegenerator identity -h
```

Dans le dossier du projet, exécutez le scaffolder d’identité avec les options souhaitées. Par exemple, pour configurer l’identité de l’interface utilisateur par défaut et le nombre minimal de fichiers, exécutez la commande suivante. Utilisez le nom qualifié complet correct pour votre contexte de base de données :

```cli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```

-------------
