---
ms.openlocfilehash: 209b5c41e17897693962954b1e795bdbb41f9384
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59012732"
---
# <a name="key-vault-configuration-provider-sample-app"></a>Exemple d’application de coffre de clés Configuration fournisseur

Cet exemple illustre l’utilisation du fournisseur de Configuration Azure Key Vault.

L’exemple s’exécute dans un des deux modes déterminés par le `#define` instruction en haut de la *Program.cs* fichier. Pour obtenir des instructions, consultez [directives de préprocesseur dans l’exemple de code](https://docs.microsoft.com/aspnet/core#preprocessor-directives-in-sample-code):

* `Certificate` &ndash; Illustre l’utilisation d’un certificat X.509 et les ID de Client Azure Key Vault à des secrets d’accès stockées dans Azure Key Vault. Cette version de l’exemple peut être exécutée à partir de n’importe quel emplacement, déployé sur Azure App Service ou n’importe quel hôte peut être utilisée par une application ASP.NET Core.
* `Managed` &ndash; Illustre l’utilisation d’Azure [Managed Service Identity](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) pour authentifier l’application dans Azure Key Vault avec l’authentification Azure AD sans informations d’identification dans le code ou la configuration de l’application. Un ID Client Azure AD et la clé secrète n’est requis pour l’application auprès d’Azure Key Vault. Cet exemple doit être déployé sur Azure App Service pour Explorer le scearnio msi.

Pour plus d’informations, consultez [fournisseur de Configuration d’Azure Key Vault](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration).
