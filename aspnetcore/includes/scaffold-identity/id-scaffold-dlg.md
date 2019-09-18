Exécutez l’échafaudage d’identité:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* À partir de **l’Explorateur de solutions**, avec le bouton droit sur le projet > **ajouter** > **nouvel élément structuré**.
* Dans le volet gauche de la **ajouter une structure** boîte de dialogue, sélectionnez **identité** > **ajouter**.
* Dans la boîte de dialogue **Ajouter une identité** , sélectionnez les options souhaitées.
  * Sélectionnez votre page de disposition existante, ou votre fichier de disposition sera remplacé par un balisage incorrect. Par exemple `~/Pages/Shared/_Layout.cshtml` , pour `~/Views/Shared/_Layout.cshtml` les Razor pages pour les projets MVC
  * Sélectionnez le **+** bouton pour créer un nouveau **classe de contexte de données**.
* Sélectionnez **ajouter**.

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

Si vous n’avez pas encore installé le Générateur de modèles automatique ASP.NET Core, vous pouvez l’installer maintenant :

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Ajoutez une référence de package à [Microsoft. VisualStudio. Web. CodeGeneration. Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) dans le fichier\*projet (. csproj). Dans le répertoire du projet, exécutez la commande suivante :

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Exécutez la commande suivante pour répertorier les options de génération de modèles automatique d’identité :

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

Dans le dossier du projet, exécutez l’échafaudage d’identité avec les options souhaitées. Par exemple, pour configurer l’identité avec l’interface utilisateur par défaut et le nombre minimal de fichiers, exécutez la commande suivante :

```dotnetcli
dotnet aspnet-codegenerator identity --useDefaultUI
```

---
