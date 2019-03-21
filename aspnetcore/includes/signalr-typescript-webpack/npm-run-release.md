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

<span data-ttu-id="f57bf-101">Cette commande génère les ressources côté client à délivrer pendant l’exécution de l’application.</span><span class="sxs-lookup"><span data-stu-id="f57bf-101">This command yields the client-side assets to be served when running the app.</span></span> <span data-ttu-id="f57bf-102">Les ressources sont placées dans le dossier *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="f57bf-102">The assets are placed in the *wwwroot* folder.</span></span>

<span data-ttu-id="f57bf-103">Webpack a effectué les tâches suivantes :</span><span class="sxs-lookup"><span data-stu-id="f57bf-103">Webpack completed the following tasks:</span></span>

* <span data-ttu-id="f57bf-104">Purge du contenu du répertoire *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="f57bf-104">Purged the contents of the *wwwroot* directory.</span></span>
* <span data-ttu-id="f57bf-105">Conversion du code TypeScript en JavaScript&mdash;processus appelé *transpilation*.</span><span class="sxs-lookup"><span data-stu-id="f57bf-105">Converted the TypeScript to JavaScript&mdash;a process known as *transpilation*.</span></span>
* <span data-ttu-id="f57bf-106">Troncation du code JavaScript généré pour réduire la taille de fichier&mdash;processus appelé *minimisation*.</span><span class="sxs-lookup"><span data-stu-id="f57bf-106">Mangled the generated JavaScript to reduce file size&mdash;a process known as *minification*.</span></span>
* <span data-ttu-id="f57bf-107">Copie des fichiers JavaScript, CSS et HTML traités à partir de *src* dans le répertoire *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="f57bf-107">Copied the processed JavaScript, CSS, and HTML files from *src* to the *wwwroot* directory.</span></span>
* <span data-ttu-id="f57bf-108">Injection des éléments suivants dans le fichier *wwwroot/index.html* :</span><span class="sxs-lookup"><span data-stu-id="f57bf-108">Injected the following elements into the *wwwroot/index.html* file:</span></span>
  * <span data-ttu-id="f57bf-109">Une balise `<link>`, référençant le fichier *wwwroot/main.\<code de hachage\>.css*.</span><span class="sxs-lookup"><span data-stu-id="f57bf-109">A `<link>` tag, referencing the *wwwroot/main.\<hash\>.css* file.</span></span> <span data-ttu-id="f57bf-110">Cette balise est placée immédiatement avant la balise `</head>` de fermeture.</span><span class="sxs-lookup"><span data-stu-id="f57bf-110">This tag is placed immediately before the closing `</head>` tag.</span></span>
  * <span data-ttu-id="f57bf-111">Une balise `<script>`, référençant le fichier *wwwroot/main.\<code de hachage\>.css* minimisé.</span><span class="sxs-lookup"><span data-stu-id="f57bf-111">A `<script>` tag, referencing the minified *wwwroot/main.\<hash\>.js* file.</span></span> <span data-ttu-id="f57bf-112">Cette balise est placée immédiatement avant la balise `</body>` de fermeture.</span><span class="sxs-lookup"><span data-stu-id="f57bf-112">This tag is placed immediately before the closing `</body>` tag.</span></span>
