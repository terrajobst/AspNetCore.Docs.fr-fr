---
title: Activer la génération de Code QR pour les applications de l’authentificateur TOTP dans ASP.NET Core
author: rick-anderson
description: Découvrez comment activer la génération de code QR pour les applications de l’authentificateur TOTP qui fonctionnent avec l’authentification à deux facteurs ASP.NET Core.
ms.author: riande
ms.date: 08/14/2018
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: 5581f2001036746974a858d8a664db16df50edb2
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209224"
---
# <a name="enable-qr-code-generation-for-totp-authenticator-apps-in-aspnet-core"></a>Activer la génération de Code QR pour les applications de l’authentificateur TOTP dans ASP.NET Core

::: moniker range="<= aspnetcore-2.0"

Codes QR nécessite ASP.NET Core 2.0 ou version ultérieure.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

ASP.NET Core est livré avec la prise en charge pour les applications de l’authentificateur pour l’authentification individuelle. Deux facteurs (2FA) authentificateur applications d’authentification, à l’aide d’une heure basée à usage unique mot de passe algorithme définie (TOTP), sont le secteur de l’approche 2fa recommandé. À 2 facteurs à l’aide de TOTP est préférée à SMS 2FA. Une application d’authentification fournit un code de 6 à 8 chiffres que les utilisateurs doivent entrer après la confirmation de leur nom d’utilisateur et le mot de passe. En règle générale, une application d’authentification est installée sur un Smartphone.

Les modèles d’application web ASP.NET Core prennent en charge des authentificateurs, mais ne fournissent pas la prise en charge pour la génération de QRCode. Générateurs de QRCode facilitent l’installation de 2FA. Ce document vous guidera ajout [Code QR](https://wikipedia.org/wiki/QR_code) génération à la page de configuration à 2 facteurs.

Authentification à deux facteurs ne se produit pas à l’aide d’un fournisseur d’authentification externe, tel que [Google](xref:security/authentication/google-logins) ou [Facebook](xref:security/authentication/facebook-logins). Connexions externes sont protégées par le mécanisme qui fournit le fournisseur de connexion externe. Considérons, par exemple, le [Microsoft](xref:security/authentication/microsoft-logins) fournisseur d’authentification requiert une clé matérielle ou une autre approche à 2 facteurs. Si les modèles par défaut appliquées à 2 facteurs « local » puis les utilisateurs doivent satisfaire les deux approches à 2 facteurs, qui n’est pas un scénario couramment utilisé.

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a>Ajout des Codes QR à la page de configuration à 2 facteurs

Ces instructions utilisent *qrcode.js* à partir de la https://davidshimjs.github.io/qrcodejs/ dépôt.

* Téléchargez le [qrcode.js javascript bibliothèque](https://davidshimjs.github.io/qrcodejs/) à la `wwwroot\lib` dossier dans votre projet.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* Suivez les instructions de [une structure identité](xref:security/authentication/scaffold-identity) pour générer */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.
* Dans */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*, localisez le `Scripts` section à la fin du fichier :

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* Dans *Pages/Account/Manage/EnableAuthenticator.cshtml* (Pages Razor) ou *Views/Manage/EnableAuthenticator.cshtml* (MVC), recherchez le `Scripts` section à la fin du fichier :

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* Mise à jour le `Scripts` section pour ajouter une référence à la `qrcodejs` bibliothèque que vous avez ajouté et un appel pour générer le Code QR. Il doit se présenter comme suit :

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")

    <script type="text/javascript" src="~/lib/qrcode.js"></script>
    <script type="text/javascript">
        new QRCode(document.getElementById("qrCode"),
            {
                text: "@Html.Raw(Model.AuthenticatorUri)",
                width: 150,
                height: 150
            });
    </script>
}
```

* Supprimer le paragraphe qui vous renvoie à ces instructions.

Exécutez votre application et vous assurer que vous pouvez scanner le code QR et valider le code que destiné à prouver l’authentificateur.

## <a name="change-the-site-name-in-the-qr-code"></a>Modifier le nom du site dans le Code QR

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Le nom du site dans le Code QR est effectuée à partir du nom de projet que vous choisissez lors de la création initiale de votre projet. Vous pouvez le modifier en recherchant le `GenerateQrCodeUri(string email, string unformattedKey)` méthode dans le */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Le nom du site dans le Code QR est effectuée à partir du nom de projet que vous choisissez lors de la création initiale de votre projet. Vous pouvez le modifier en recherchant le `GenerateQrCodeUri(string email, string unformattedKey)` méthode dans le *Pages/Account/Manage/EnableAuthenticator.cshtml.cs* fichier (Pages Razor) ou le *Controllers/ManageController.cs* les fichier (MVC).

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

Le code par défaut à partir du modèle se présente comme suit :

```csharp
private string GenerateQrCodeUri(string email, string unformattedKey)
{
    return string.Format(
        AuthenticatorUriFormat,
        _urlEncoder.Encode("Razor Pages"),
        _urlEncoder.Encode(email),
        unformattedKey);
}
```

Le deuxième paramètre dans l’appel à `string.Format` est le nom de votre site, effectuée à partir de votre nom de la solution. Il peut être modifié à n’importe quelle valeur, mais il doit toujours être codée URL.

## <a name="using-a-different-qr-code-library"></a>À l’aide d’une autre bibliothèque de Code QR

Vous pouvez remplacer la bibliothèque de Code QR avec votre bibliothèque par défaut. Le code HTML contient un `qrCode` élément dans lequel vous pouvez placer un Code QR par le mécanisme qui fournit de votre bibliothèque.

L’URL correctement formatée pour le Code QR est disponible dans le :

* `AuthenticatorUri` propriété du modèle.
* `data-url` propriété dans le `qrCodeData` élément.

## <a name="totp-client-and-server-time-skew"></a>Décalage horaire TOTP client et serveur

L’authentification TOTP (à usage unique mot de passe temporaire) dépend du périphérique à la fois le serveur et un authentificateur ayant une heure précise. Jetons durent uniquement pendant 30 secondes. Si des connexions de 2FA TOTP échouent, vérifiez que l’heure du serveur est précise et préférence synchronisée à un service NTP précis.

::: moniker-end
