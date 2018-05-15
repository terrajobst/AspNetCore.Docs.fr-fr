# <a name="how-to-buildrun-secure-user-data-sample"></a><span data-ttu-id="cd9f7-101">Comment faire pour générer/exécuter l’exemple de données utilisateur sécurisé</span><span class="sxs-lookup"><span data-stu-id="cd9f7-101">How to build/run Secure user data sample</span></span>

* <span data-ttu-id="cd9f7-102">Définir le mot de passe avec l’outil Gestionnaire de Secret :</span><span class="sxs-lookup"><span data-stu-id="cd9f7-102">Set password with the Secret Manager tool:</span></span>

  `dotnet user-secrets set SeedUserPW <pw>`

* <span data-ttu-id="cd9f7-103">Mettre à jour la base de données :</span><span class="sxs-lookup"><span data-stu-id="cd9f7-103">Update the database:</span></span>

    `dotnet ef database update`

* <span data-ttu-id="cd9f7-104">Activer le protocole SSL dans le projet</span><span class="sxs-lookup"><span data-stu-id="cd9f7-104">Enable SSL in the project</span></span>