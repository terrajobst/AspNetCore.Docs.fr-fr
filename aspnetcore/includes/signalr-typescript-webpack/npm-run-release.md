```console
npm run release
```

Cette commande génère les ressources côté client à utiliser lors de l’exécution de l’application. Les ressources sont placées dans le dossier *wwwroot*.

Webpack a effectué les tâches suivantes :

* Purge du contenu du répertoire *wwwroot*.
* Conversion de la machine à écrire en JavaScript dans un processus appelé *transpilation*.
* A tronqué le code JavaScript généré pour réduire la taille du fichier dans un processus connu sous le nom de *minimisation*.
* Copie des fichiers JavaScript, CSS et HTML traités à partir de *src* dans le répertoire *wwwroot*.
* Injection des éléments suivants dans le fichier *wwwroot/index.html* :
  * Une balise `<link>`, référençant le fichier *wwwroot/main.\<code de hachage\>.css*. Cette balise est placée immédiatement avant la balise `</head>` de fermeture.
  * Une balise `<script>`, référençant le fichier *wwwroot/main.\<code de hachage\>.css* minimisé. Cette balise est placée immédiatement avant la balise `</body>` de fermeture.