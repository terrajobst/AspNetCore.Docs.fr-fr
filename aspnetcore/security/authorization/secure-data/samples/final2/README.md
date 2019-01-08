# <a name="how-to-buildrun-secure-user-data-sample"></a><span data-ttu-id="93021-101">L’échantillon de données utilisateur sécurisée d’exécution/de build</span><span class="sxs-lookup"><span data-stu-id="93021-101">How to build/run Secure user data sample</span></span>

* <span data-ttu-id="93021-102">Définir le mot de passe avec l’outil Secret Manager :</span><span class="sxs-lookup"><span data-stu-id="93021-102">Set password with the Secret Manager tool:</span></span>

  `dotnet user-secrets set SeedUserPW <pw>`

* <span data-ttu-id="93021-103">Mettre à jour la base de données :</span><span class="sxs-lookup"><span data-stu-id="93021-103">Update the database:</span></span>

    `dotnet ef database update`

* <span data-ttu-id="93021-104">Activer HTTPS dans le projet</span><span class="sxs-lookup"><span data-stu-id="93021-104">Enable HTTPS in the project</span></span>
