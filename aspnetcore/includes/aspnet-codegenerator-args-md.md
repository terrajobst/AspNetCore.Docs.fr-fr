<!-- Options common to Razor Pages and Controller -->
| Option               | Description|
| ----------------- | ------------ |
| --model ou -m  | Classe de modèle à utiliser. |
| --dataContext ou -dc  | Classe `DbContext` à utiliser. |
| --bootstrapVersion ou -b  | Spécifie la version de démarrage. Les valeurs valides sont `3` ou `4`. La valeur par défaut est `4`. S’il est nécessaire mais absent, un répertoire *wwwroot* est créé, qui comprend les fichiers de démarrage de la version spécifiée. |
| --referenceScriptLibraries ou -scripts |  Référence les bibliothèques de scripts dans les vues générées. Ajoute `_ValidationScriptsPartial` aux pages Modifier et Créer. |
| --layout ou -l | Page de disposition personnalisée à utiliser. |
| --useDefaultLayout ou -udl | Utiliser la disposition par défaut pour les vues. |
| --force ou -f | Remplacer les fichiers existants. |
| --relativeFolderPath ou -outDir | Chemin relatif du dossier de sortie du projet où les fichiers sont générés. S’il n’est pas spécifié, les fichiers sont générés dans le dossier du projet. |