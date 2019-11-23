# <a name="upload-files-sample-app"></a>Exemple d’application de téléchargement de fichiers

Cet exemple d’application montre les concepts décrits dans la rubrique [Télécharger des fichiers dans ASP.net Core](https://docs.microsoft.com/aspnet/core/mvc/models/file-uploads) .

## <a name="security-considerations"></a>Considérations relatives à la sécurité

Soyez prudent lorsque vous fournissez aux utilisateurs la possibilité de charger des fichiers sur un serveur. Les attaquants peuvent exécuter des attaques par [déni de service](/windows-hardware/drivers/ifs/denial-of-service) , tenter de charger des virus ou des programmes malveillants ou tenter de compromettre des réseaux et des serveurs d’une autre manière.

Les étapes de sécurité qui réduisent la probabilité d’une attaque réussie sont les suivantes :

* Chargez les fichiers dans une zone de chargement de fichier dédiée sur le système, de préférence sur un lecteur non-système. L’utilisation d’un emplacement dédié permet d’imposer des mesures de sécurité plus faciles sur les fichiers téléchargés. Désactivez les autorisations d’exécution sur l’emplacement de chargement du fichier.&dagger;
* Ne conservez jamais les fichiers téléchargés dans la même arborescence de répertoires que l’application.&dagger;
* Utilisez un nom de fichier sécurisé déterminé par l’application. N’utilisez pas un nom de fichier fourni par l’entrée d’utilisateur ou le nom de fichier non fiable du fichier téléchargé.&dagger;
* Autorise uniquement un ensemble spécifique d’extensions de fichier approuvé.&dagger;
* Vérifiez la signature de format de fichier pour empêcher un utilisateur de télécharger un fichier fictif (par exemple, en chargeant un fichier *. exe* avec une extension *. txt* ).&dagger;
* Vérifiez que les vérifications côté client sont également effectuées sur le serveur. Les contrôles côté client sont faciles à contourner.&dagger;
* Vérifiez la taille du téléchargement et empêchez les chargements supérieurs à ceux attendus.&dagger;
* Lorsque les fichiers ne doivent pas être remplacés par un fichier téléchargé portant le même nom, vérifiez le nom du fichier par rapport à la base de données ou au stockage physique avant de charger le fichier.
* **Exécutez un programme de détection de virus et de logiciels malveillants sur le contenu chargé avant que le fichier ne soit stocké.**

&dagger;l’exemple d’application illustre une approche qui répond aux critères.

> [!WARNING]
> Le chargement d’un code malveillant sur un système est généralement la première étape de l’exécution de code capable de :
>
> * Prendre complètement en charge un système.
> * Surchargez un système avec le résultat du blocage du système.
> * Compromettre les données utilisateur ou système.
> * Appliquez Graffiti à une interface utilisateur publique.
>
> Pour plus d’informations sur la réduction de la surface d’attaque quand vous acceptez des fichiers d’utilisateurs, consultez les ressources suivantes :
>
> * [Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload) (Chargement de fichiers illimité)
> * [Sécurité Azure : Vérifier que les contrôles appropriés sont en place quand vous acceptez des fichiers d’utilisateurs](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

Pour plus d’informations, consultez [charger des fichiers dans ASP.net Core](https://docs.microsoft.com/aspnet/core/mvc/models/file-uploads).

## <a name="how-to-use-the-sample"></a>Comment utiliser l’exemple

Dans le fichier *appSettings. JSON* :

1. Définissez le chemin d’accès pour les fichiers stockés (`StoredFilesPath`).

   * L’exemple d’application définit la valeur sur `c:\\files`, ce qui suppose qu’un dossier nommé *Files* existe à la racine du lecteur C : du système.
   * Le chemin doit exister. Créez un dossier de *fichiers* sur le lecteur C : du système ou définissez le chemin d’accès à un emplacement approprié.
   * Le processus de l’application nécessite des autorisations de lecture/écriture sur le chemin d’accès.
   * **PRÉCIEUSE!** Désactivez les autorisations d’exécution pour tous les utilisateurs dans le chemin.

1. Définissez la limite de taille de fichier (`FileSizeLimit`) en octets. La valeur par défaut de `2097152` (2 097 152 octets) de l’exemple d’application autorise les chargements de fichiers jusqu’à 2 Mo.
