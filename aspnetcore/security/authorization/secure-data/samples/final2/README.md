---
ms.openlocfilehash: 6246247788eefc8f094ec1a7f156cb36715edb53
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210104"
---
# <a name="how-to-buildrun-secure-user-data-sample"></a>L’échantillon de données utilisateur sécurisée d’exécution/de build

* Définir le mot de passe avec l’outil Secret Manager :

  `dotnet user-secrets set SeedUserPW <pw>`

* Mettre à jour la base de données :

  `dotnet ef database update`

* Activer HTTPS dans le projet
