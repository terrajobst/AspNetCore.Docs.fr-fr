---
ms.openlocfilehash: 6246247788eefc8f094ec1a7f156cb36715edb53
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210104"
---
# <a name="how-to-buildrun-secure-user-data-sample"></a><span data-ttu-id="113fb-101">L’échantillon de données utilisateur sécurisée d’exécution/de build</span><span class="sxs-lookup"><span data-stu-id="113fb-101">How to build/run Secure user data sample</span></span>

* <span data-ttu-id="113fb-102">Définir le mot de passe avec l’outil Secret Manager :</span><span class="sxs-lookup"><span data-stu-id="113fb-102">Set password with the Secret Manager tool:</span></span>

  `dotnet user-secrets set SeedUserPW <pw>`

* <span data-ttu-id="113fb-103">Mettre à jour la base de données :</span><span class="sxs-lookup"><span data-stu-id="113fb-103">Update the database:</span></span>

  `dotnet ef database update`

* <span data-ttu-id="113fb-104">Activer HTTPS dans le projet</span><span class="sxs-lookup"><span data-stu-id="113fb-104">Enable HTTPS in the project</span></span>
