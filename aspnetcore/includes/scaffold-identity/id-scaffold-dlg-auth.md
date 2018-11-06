Exécutez le Générateur de modèles automatique identité :

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* À partir de **l’Explorateur de solutions**, avec le bouton droit sur le projet > **ajouter** > **nouvel élément structuré**.
* Dans le volet gauche de la **ajouter une structure** boîte de dialogue, sélectionnez **identité** > **ajouter**.
* Dans le **identité ADD** boîte de dialogue, sélectionnez les options souhaitées.
  * Sélectionnez votre page de disposition existante, ou votre fichier de disposition est remplacée par balisage incorrect. Quand un existant  *\_Layout.cshtml* fichier est sélectionné, il est **pas** remplacé.

 Par exemple `~/Pages/Shared/_Layout.cshtml` pour les Pages Razor `~/Views/Shared/_Layout.cshtml` pour les projets MVC
* Pour utiliser votre contexte de données existant, sélectionnez au moins un fichier à remplacer. Vous devez sélectionner au moins un fichier pour ajouter votre contexte de données.
  * Sélectionnez votre classe de contexte de données.
  * Sélectionnez **ajouter**.
* Pour créer un nouveau contexte de l’utilisateur et éventuellement créer une classe d’utilisateur personnalisée pour l’identité :
  * Sélectionnez le **+** bouton pour créer un nouveau **classe de contexte de données**.
  * Sélectionnez **ajouter**.

Remarque : Si vous créez un nouveau contexte utilisateur, il est inutile de sélectionner un fichier à remplacer.

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

Dans le dossier du projet, exécutez le Générateur de modèles automatique identité avec les options souhaitées. Par exemple, pour configurer l’identité avec l’interface utilisateur par défaut et le nombre minimal de fichiers, exécutez la commande suivante. Utilisez le nom qualifié complet correct pour votre contexte de base de données :

```cli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```

PowerShell utilise le point-virgule comme séparateur de commande. Lorsque vous utilisez powershell, les points-virgules dans la liste des fichiers de séquence d’échappement ou placez la liste des fichiers dans des guillemets doubles. Exemple :

```cli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

Si vous exécutez le Générateur de modèles automatique identité sans spécifier le `--files` indicateur ou `--useDefaultUI` indicateur, toutes les pages de l’interface utilisateur de l’identité disponibles seront créées dans votre projet.

-------------
