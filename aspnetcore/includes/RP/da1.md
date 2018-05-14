# <a name="update-the-generated-pages"></a>Mettre à jour les pages générées

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Nous avons une bonne ébauche de l’application de films, mais sa présentation n’est pas idéale. Nous ne voulons pas voir l’heure (« 12:00:00 AM » dans l’image ci-dessous) et **ReleaseDate** doit être **Release Date** (en deux mots).

![Application Movie ouverte dans Chrome, affichant les données relatives aux films](../../tutorials/razor-pages/sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Mettre à jour le code généré

Ouvrez le fichier *Models/Movie.cs*, puis ajoutez les lignes affichées en surbrillance dans le code suivant :

[!code-csharp[](code/Models/Movie.cs?highlight=2,11-12)]
