# <a name="how-to-buildrun-secure-user-data-sample"></a><span data-ttu-id="dc4f4-101">L’échantillon de données utilisateur sécurisée d’exécution/de build</span><span class="sxs-lookup"><span data-stu-id="dc4f4-101">How to build/run Secure user data sample</span></span>

* <span data-ttu-id="dc4f4-102">Définir le mot de passe avec l’outil Secret Manager :</span><span class="sxs-lookup"><span data-stu-id="dc4f4-102">Set password with the Secret Manager tool:</span></span>

  `dotnet user-secrets set SeedUserPW <pw>`

* <span data-ttu-id="dc4f4-103">Mettre à jour la base de données :</span><span class="sxs-lookup"><span data-stu-id="dc4f4-103">Update the database:</span></span>

    `dotnet ef database update`

* <span data-ttu-id="dc4f4-104">Activer le protocole SSL dans le projet</span><span class="sxs-lookup"><span data-stu-id="dc4f4-104">Enable SSL in the project</span></span>
