---
title: Activer la génération de Code QR pour les applications de l’authentificateur TOTP dans ASP.NET Core
author: rick-anderson
description: Découvrez comment activer la génération de code QR pour les applications de l’authentificateur TOTP qui fonctionnent avec l’authentification à deux facteurs ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/24/2017
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: b0d8f104119340b97bd65f1826bb921ca875acf8
ms.sourcegitcommit: 1faf2525902236428dae6a59e375519bafd5d6d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37089969"
---
# <a name="enable-qr-code-generation-for-totp-authenticator-apps-in-aspnet-core"></a><span data-ttu-id="e06a7-103">Activer la génération de Code QR pour les applications de l’authentificateur TOTP dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e06a7-103">Enable QR Code generation for TOTP authenticator apps in ASP.NET Core</span></span>

<span data-ttu-id="e06a7-104">ASP.NET Core est livré avec prise en charge pour les applications de l’authentificateur pour l’authentification individuelle.</span><span class="sxs-lookup"><span data-stu-id="e06a7-104">ASP.NET Core ships with support for authenticator applications for individual authentication.</span></span> <span data-ttu-id="e06a7-105">Deux applications-Factor authentication (2FA) l’authentificateur, à l’aide d’une durée à usage unique mot de passe algorithme (TOTP), sont l’approche pour 2FA recommandée.</span><span class="sxs-lookup"><span data-stu-id="e06a7-105">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="e06a7-106">2FA à l’aide de TOTP est préférée à 2FA SMS.</span><span class="sxs-lookup"><span data-stu-id="e06a7-106">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="e06a7-107">Une application d’authentification fournit un code de 6 à 8 chiffres que les utilisateurs doivent entrer après confirmation de leur nom d’utilisateur et un mot de passe.</span><span class="sxs-lookup"><span data-stu-id="e06a7-107">An authenticator app provides a 6 to 8 digit code which users must enter after confirming their username and password.</span></span> <span data-ttu-id="e06a7-108">En règle générale, une application d’authentification est installée sur un Smartphone.</span><span class="sxs-lookup"><span data-stu-id="e06a7-108">Typically an authenticator app is installed on a smart phone.</span></span>

<span data-ttu-id="e06a7-109">Les modèles d’application web ASP.NET Core prennent en charge des authentificateurs, mais ne fournissent pas la prise en charge pour la génération de QRCode.</span><span class="sxs-lookup"><span data-stu-id="e06a7-109">The ASP.NET Core web app templates support authenticators, but don't provide support for QRCode generation.</span></span> <span data-ttu-id="e06a7-110">Les générateurs de QRCode facilitent le programme d’installation de 2FA.</span><span class="sxs-lookup"><span data-stu-id="e06a7-110">QRCode generators ease the setup of 2FA.</span></span> <span data-ttu-id="e06a7-111">Ce document va vous guider ajout [Code QR](https://wikipedia.org/wiki/QR_code) génération à la page de configuration 2FA.</span><span class="sxs-lookup"><span data-stu-id="e06a7-111">This document will guide you through adding [QR Code](https://wikipedia.org/wiki/QR_code) generation to the 2FA configuration page.</span></span>

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a><span data-ttu-id="e06a7-112">Ajout des Codes QR à la page de configuration 2FA</span><span class="sxs-lookup"><span data-stu-id="e06a7-112">Adding QR Codes to the 2FA configuration page</span></span>

<span data-ttu-id="e06a7-113">Ces instructions utilisent *qrcode.js* à partir de la https://davidshimjs.github.io/qrcodejs/ référentiel.</span><span class="sxs-lookup"><span data-stu-id="e06a7-113">These instructions use *qrcode.js* from the https://davidshimjs.github.io/qrcodejs/ repo.</span></span>

* <span data-ttu-id="e06a7-114">Téléchargez le [bibliothèque javascript pour qrcode.js](https://davidshimjs.github.io/qrcodejs/) à la `wwwroot\lib` dossier dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="e06a7-114">Download the [qrcode.js javascript library](https://davidshimjs.github.io/qrcodejs/) to the `wwwroot\lib` folder in your project.</span></span>

* <span data-ttu-id="e06a7-115">Dans *Pages\Account\Manage\EnableAuthenticator.cshtml* (Pages Razor) ou *Views\Manage\EnableAuthenticator.cshtml* (MVC), recherchez le `Scripts` section à la fin du fichier :</span><span class="sxs-lookup"><span data-stu-id="e06a7-115">In *Pages\Account\Manage\EnableAuthenticator.cshtml* (Razor Pages) or *Views\Manage\EnableAuthenticator.cshtml* (MVC), locate the `Scripts` section at the end of the file:</span></span>

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* <span data-ttu-id="e06a7-116">Mise à jour la `Scripts` section pour ajouter une référence à la `qrcodejs` bibliothèque que vous avez ajouté et un appel pour générer le Code QR.</span><span class="sxs-lookup"><span data-stu-id="e06a7-116">Update the `Scripts` section to add a reference to the `qrcodejs` library you added and a call to generate the QR Code.</span></span> <span data-ttu-id="e06a7-117">Il doit se présenter comme suit :</span><span class="sxs-lookup"><span data-stu-id="e06a7-117">It should look as follows:</span></span>

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

* <span data-ttu-id="e06a7-118">Supprimer le paragraphe qui propose des liens vers ces instructions.</span><span class="sxs-lookup"><span data-stu-id="e06a7-118">Delete the paragraph which links you to these instructions.</span></span>

<span data-ttu-id="e06a7-119">Exécuter votre application et vous assurer que vous pouvez analyser le code QR et valider le code que destiné à prouver l’authentificateur.</span><span class="sxs-lookup"><span data-stu-id="e06a7-119">Run your app and ensure that you can scan the QR code and validate the code the authenticator proves.</span></span>

## <a name="change-the-site-name-in-the-qr-code"></a><span data-ttu-id="e06a7-120">Modifier le nom du site dans le Code QR</span><span class="sxs-lookup"><span data-stu-id="e06a7-120">Change the site name in the QR Code</span></span>

<span data-ttu-id="e06a7-121">Le nom du site dans le Code QR provient du nom du projet que vous sélectionnez lorsque vous créez votre projet.</span><span class="sxs-lookup"><span data-stu-id="e06a7-121">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="e06a7-122">Vous pouvez le modifier en recherchant le `GenerateQrCodeUri(string email, string unformattedKey)` méthode dans le *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* les fichiers (Pages Razor) ou le *Controllers\ManageController.cs* fichier (MVC).</span><span class="sxs-lookup"><span data-stu-id="e06a7-122">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* (Razor Pages) file or the *Controllers\ManageController.cs* (MVC) file.</span></span>

<span data-ttu-id="e06a7-123">Le code à partir du modèle par défaut se présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="e06a7-123">The default code from the template looks as follows:</span></span>

```c#
private string GenerateQrCodeUri(string email, string unformattedKey)
{
    return string.Format(
        AuthenicatorUriFormat,
        _urlEncoder.Encode("Razor Pages"),
        _urlEncoder.Encode(email),
        unformattedKey);
}
```

<span data-ttu-id="e06a7-124">Le deuxième paramètre dans l’appel à `string.Format` est le nom de votre site, obtenue à partir de votre nom de la solution.</span><span class="sxs-lookup"><span data-stu-id="e06a7-124">The second parameter in the call to `string.Format` is your site name, taken from your solution name.</span></span> <span data-ttu-id="e06a7-125">Il peut être modifié par n’importe quelle valeur, mais il doit toujours être codé URL.</span><span class="sxs-lookup"><span data-stu-id="e06a7-125">It can be changed to any value, but it must always be URL encoded.</span></span>

## <a name="using-a-different-qr-code-library"></a><span data-ttu-id="e06a7-126">À l’aide d’une autre bibliothèque de Code QR</span><span class="sxs-lookup"><span data-stu-id="e06a7-126">Using a different QR Code library</span></span>

<span data-ttu-id="e06a7-127">Vous pouvez remplacer la bibliothèque de Code QR avec la bibliothèque par défaut.</span><span class="sxs-lookup"><span data-stu-id="e06a7-127">You can replace the QR Code library with your preferred library.</span></span> <span data-ttu-id="e06a7-128">Le code HTML contient un `qrCode` élément dans lequel vous pouvez placer un Code QR par le mécanisme de votre bibliothèque fournit.</span><span class="sxs-lookup"><span data-stu-id="e06a7-128">The HTML contains a `qrCode` element into which you can place a QR Code by whatever mechanism your library provides.</span></span>

<span data-ttu-id="e06a7-129">L’URL correcte pour le Code QR est disponible dans le :</span><span class="sxs-lookup"><span data-stu-id="e06a7-129">The correctly formatted URL for the QR Code is available in the:</span></span>

* <span data-ttu-id="e06a7-130">`AuthenticatorUri` propriété du modèle.</span><span class="sxs-lookup"><span data-stu-id="e06a7-130">`AuthenticatorUri` property of the model.</span></span>
* <span data-ttu-id="e06a7-131">`data-url` propriété dans le `qrCodeData` élément.</span><span class="sxs-lookup"><span data-stu-id="e06a7-131">`data-url` property in the `qrCodeData` element.</span></span>

## <a name="totp-client-and-server-time-skew"></a><span data-ttu-id="e06a7-132">Décalage horaire TOTP client et serveur</span><span class="sxs-lookup"><span data-stu-id="e06a7-132">TOTP client and server time skew</span></span>

<span data-ttu-id="e06a7-133">Authentification de TOTP (à usage unique mot de passe temporaire) dépend du périphérique à la fois le serveur et un authentificateur ayant une heure précise.</span><span class="sxs-lookup"><span data-stu-id="e06a7-133">TOTP (Time-based One-Time Password) authentication depends on both the server and authenticator device having an accurate time.</span></span> <span data-ttu-id="e06a7-134">Les jetons ne durent que 30 secondes.</span><span class="sxs-lookup"><span data-stu-id="e06a7-134">Tokens only last for 30 seconds.</span></span> <span data-ttu-id="e06a7-135">Si des connexions de 2FA TOTP échouent, vérifiez que l’heure du serveur est précises et, de préférence, synchronisées avec un service NTP précis.</span><span class="sxs-lookup"><span data-stu-id="e06a7-135">If TOTP 2FA logins are failing, check that the server time is accurate, and preferably synchronized to an accurate NTP service.</span></span>
