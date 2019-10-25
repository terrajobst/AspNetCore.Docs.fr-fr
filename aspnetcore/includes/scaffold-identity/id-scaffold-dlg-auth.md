Exécutez l’échafaudage d’identité :

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet > **Ajouter** > **nouvel élément de génération de modèles**automatique.
* Dans le volet gauche de la boîte de dialogue **Ajouter une structure** , sélectionnez **identité** > **Ajouter**.
* Dans la boîte de dialogue **Ajouter une identité** , sélectionnez les options souhaitées.
  * Sélectionnez votre page de disposition existante, ou votre fichier de disposition sera remplacé par un balisage incorrect. Lorsqu’un fichier *\_Layout. cshtml* existant est sélectionné, il n’est **pas** remplacé.

 Par exemple : `~/Pages/Shared/_Layout.cshtml` pour Razor Pages `~/Views/Shared/_Layout.cshtml` pour les projets MVC
* Pour utiliser le contexte de données existant, sélectionnez au moins un fichier à substituer. Vous devez sélectionner au moins un fichier pour ajouter votre contexte de données.
  * Sélectionnez votre classe de contexte de données.
  * Sélectionnez **Ajouter** .
* Pour créer un nouveau contexte utilisateur et éventuellement créer une classe d’utilisateur personnalisée pour l’identité :
  * Sélectionnez le bouton **+** pour créer une **classe de contexte de données**.
  * Sélectionnez **Ajouter** .

Remarque : Si vous créez un nouveau contexte utilisateur, vous n’êtes pas obligé de sélectionner un fichier à remplacer.

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

Si vous n’avez pas déjà installé le générateur de modèles automatique ASP.NET Core, installez-le maintenant :

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Ajoutez une référence de package à [Microsoft. VisualStudio. Web. CodeGeneration. Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) au fichier projet (\*. csproj). Exécutez la commande suivante dans le répertoire du projet :

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Exécutez la commande suivante pour répertorier les options d’identification de l’échafaudage :

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

Dans le dossier du projet, exécutez l’échafaudage d’identité avec les options souhaitées. Par exemple, pour configurer l’identité avec l’interface utilisateur par défaut et le nombre minimal de fichiers, exécutez la commande suivante. Utilisez le nom complet correct pour votre contexte de base de connaissances :

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login"
```

PowerShell utilise un point-virgule comme séparateur de commande. Quand vous utilisez PowerShell, échapper les points-virgules dans la liste de fichiers ou placer la liste de fichiers entre guillemets doubles. Exemple :

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

Si vous exécutez l’échafaudage d’identité sans spécifier l’indicateur `--files` ou l’indicateur `--useDefaultUI`, toutes les pages d’interface utilisateur d’identité disponibles seront créées dans votre projet.

---
