---
ms.openlocfilehash: a9bdff481b1a72a9ee19f4e51fada177530c0cbb
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/10/2019
ms.locfileid: "59472302"
---
*  Approuvez le certificat de développement HTTPS en exécutant la commande suivante :

    ```console
    dotnet dev-certs https --trust
    ```

    La commande précédente affiche la boîte de dialogue suivante :

    ![Boîte de dialogue Avertissement de sécurité](~/getting-started/_static/cert.png)

*    Sélectionnez **Oui** si vous acceptez d’approuver le certificat de développement.

     Pour plus d’informations, consultez [Approuver le certificat de développement HTTPS ASP.NET Core](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).