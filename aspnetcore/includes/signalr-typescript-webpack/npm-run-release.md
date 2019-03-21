---
ms.openlocfilehash: c82571d3cfa57ccd6e7c83f654f119bdd8991486
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210440"
---
```console
npm run release
```

Cette commande génère les ressources côté client à délivrer pendant l’exécution de l’application. Les ressources sont placées dans le dossier *wwwroot*.

Webpack a effectué les tâches suivantes :

* Purge du contenu du répertoire *wwwroot*.
* Conversion du code TypeScript en JavaScript&mdash;processus appelé *transpilation*.
* Troncation du code JavaScript généré pour réduire la taille de fichier&mdash;processus appelé *minimisation*.
* Copie des fichiers JavaScript, CSS et HTML traités à partir de *src* dans le répertoire *wwwroot*.
* Injection des éléments suivants dans le fichier *wwwroot/index.html* :
  * Une balise `<link>`, référençant le fichier *wwwroot/main.\<code de hachage\>.css*. Cette balise est placée immédiatement avant la balise `</head>` de fermeture.
  * Une balise `<script>`, référençant le fichier *wwwroot/main.\<code de hachage\>.css* minimisé. Cette balise est placée immédiatement avant la balise `</body>` de fermeture.
