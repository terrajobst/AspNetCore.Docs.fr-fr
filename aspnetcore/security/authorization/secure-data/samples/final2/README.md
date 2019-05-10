# <a name="how-to-buildrun-secure-user-data-sample"></a>L’échantillon de données utilisateur sécurisée d’exécution/de build

* Définir le mot de passe avec l’outil Secret Manager :

  `dotnet user-secrets set SeedUserPW <pw>`

* Mettre à jour la base de données :

  `dotnet ef database update`

* Activer HTTPS dans le projet
