# <a name="custom-model-binding-demo"></a>Démonstration de liaison de modèles personnalisée

Vous pouvez tester le `ByteArrayModelBinder` en exécutant l’application et en envoyant (à l’aide de POST) une chaîne codée en base64 au point de terminaison ImageController (/api/image/). Vous devez spécifier les propriétés de nom et de nom de fichier dans le corps de la requête en tant que form-data (à l’aide de Postman ou d’un outil similaire). Vous pouvez utiliser [cet exemple de chaîne](Base64String.txt). Le résultat est enregistré dans le dossier wwwroot/images/upload avec le nom de fichier que vous avez spécifié.

Pour tester l’exemple de liaison personnalisée, essayez les points de terminaison suivants : /api/authors/1 /api/authors/2 (NOT FOUND) /api/boundauthors/1 /api/boundauthors/2 (NOT FOUND) /api/boundauthors/get/1 /api/boundauthors/get/2 (NO CONTENT). Cette action ne vérifie pas la présence de la valeur null, et retourne Not Found.
