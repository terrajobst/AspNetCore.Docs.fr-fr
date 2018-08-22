---
title: Activer la génération de Code QR pour les applications de l’authentificateur TOTP dans ASP.NET Core
author: rick-anderson
description: Découvrez comment activer la génération de code QR pour les applications de l’authentificateur TOTP qui fonctionnent avec l’authentification à deux facteurs ASP.NET Core.
ms.author: riande
ms.date: 08/14/2018
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: 4535efdde7340436c6a508848bff86e103df570e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823646"
---
# <a name="enable-qr-code-generation-for-totp-authenticator-apps-in-aspnet-core"></a><span data-ttu-id="181b9-103">Activer la génération de Code QR pour les applications de l’authentificateur TOTP dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="181b9-103">Enable QR Code generation for TOTP authenticator apps in ASP.NET Core</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="181b9-104">Codes QR nécessite ASP.NET Core 2.0 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="181b9-104">QR Codes requires ASP.NET Core 2.0 or later.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="181b9-105">ASP.NET Core est livré avec la prise en charge pour les applications de l’authentificateur pour l’authentification individuelle.</span><span class="sxs-lookup"><span data-stu-id="181b9-105">ASP.NET Core ships with support for authenticator applications for individual authentication.</span></span> <span data-ttu-id="181b9-106">Deux facteurs (2FA) authentificateur applications d’authentification, à l’aide d’une heure basée à usage unique mot de passe algorithme définie (TOTP), sont le secteur de l’approche 2fa recommandé.</span><span class="sxs-lookup"><span data-stu-id="181b9-106">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="181b9-107">À 2 facteurs à l’aide de TOTP est préférée à SMS 2FA.</span><span class="sxs-lookup"><span data-stu-id="181b9-107">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="181b9-108">Une application d’authentification fournit un code de 6 à 8 chiffres que les utilisateurs doivent entrer après la confirmation de leur nom d’utilisateur et le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="181b9-108">An authenticator app provides a 6 to 8 digit code which users must enter after confirming their username and password.</span></span> <span data-ttu-id="181b9-109">En règle générale, une application d’authentification est installée sur un Smartphone.</span><span class="sxs-lookup"><span data-stu-id="181b9-109">Typically an authenticator app is installed on a smart phone.</span></span>

<span data-ttu-id="181b9-110">Les modèles d’application web ASP.NET Core prennent en charge des authentificateurs, mais ne fournissent pas la prise en charge pour la génération de QRCode.</span><span class="sxs-lookup"><span data-stu-id="181b9-110">The ASP.NET Core web app templates support authenticators, but don't provide support for QRCode generation.</span></span> <span data-ttu-id="181b9-111">Générateurs de QRCode facilitent l’installation de 2FA.</span><span class="sxs-lookup"><span data-stu-id="181b9-111">QRCode generators ease the setup of 2FA.</span></span> <span data-ttu-id="181b9-112">Ce document vous guidera ajout [Code QR](https://wikipedia.org/wiki/QR_code) génération à la page de configuration à 2 facteurs.</span><span class="sxs-lookup"><span data-stu-id="181b9-112">This document will guide you through adding [QR Code](https://wikipedia.org/wiki/QR_code) generation to the 2FA configuration page.</span></span>

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a><span data-ttu-id="181b9-113">Ajout des Codes QR à la page de configuration à 2 facteurs</span><span class="sxs-lookup"><span data-stu-id="181b9-113">Adding QR Codes to the 2FA configuration page</span></span>

<span data-ttu-id="181b9-114">Ces instructions utilisent *qrcode.js* à partir de la https://davidshimjs.github.io/qrcodejs/ dépôt.</span><span class="sxs-lookup"><span data-stu-id="181b9-114">These instructions use *qrcode.js* from the https://davidshimjs.github.io/qrcodejs/ repo.</span></span>

* <span data-ttu-id="181b9-115">Téléchargez le [qrcode.js javascript bibliothèque](https://davidshimjs.github.io/qrcodejs/) à la `wwwroot\lib` dossier dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="181b9-115">Download the [qrcode.js javascript library](https://davidshimjs.github.io/qrcodejs/) to the `wwwroot\lib` folder in your project.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="181b9-116">Suivez les instructions de [une structure identité](xref:security/authentication/scaffold-identity) pour générer */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="181b9-116">Follow the instructions in [Scaffold Identity](xref:security/authentication/scaffold-identity) to generate */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.</span></span>
* <span data-ttu-id="181b9-117">Dans */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*, localisez le `Scripts` section à la fin du fichier :</span><span class="sxs-lookup"><span data-stu-id="181b9-117">In */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*, locate the `Scripts` section at the end of the file:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <span data-ttu-id="181b9-118">Dans *Pages/Account/Manage/EnableAuthenticator.cshtml* (Pages Razor) ou *Views/Manage/EnableAuthenticator.cshtml* (MVC), recherchez le `Scripts` section à la fin du fichier :</span><span class="sxs-lookup"><span data-stu-id="181b9-118">In *Pages/Account/Manage/EnableAuthenticator.cshtml* (Razor Pages) or *Views/Manage/EnableAuthenticator.cshtml* (MVC), locate the `Scripts` section at the end of the file:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* <span data-ttu-id="181b9-119">Mise à jour le `Scripts` section pour ajouter une référence à la `qrcodejs` bibliothèque que vous avez ajouté et un appel pour générer le Code QR.</span><span class="sxs-lookup"><span data-stu-id="181b9-119">Update the `Scripts` section to add a reference to the `qrcodejs` library you added and a call to generate the QR Code.</span></span> <span data-ttu-id="181b9-120">Il doit se présenter comme suit :</span><span class="sxs-lookup"><span data-stu-id="181b9-120">It should look as follows:</span></span>

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

* <span data-ttu-id="181b9-121">Supprimer le paragraphe qui vous renvoie à ces instructions.</span><span class="sxs-lookup"><span data-stu-id="181b9-121">Delete the paragraph which links you to these instructions.</span></span>

<span data-ttu-id="181b9-122">Exécutez votre application et vous assurer que vous pouvez scanner le code QR et valider le code que destiné à prouver l’authentificateur.</span><span class="sxs-lookup"><span data-stu-id="181b9-122">Run your app and ensure that you can scan the QR code and validate the code the authenticator proves.</span></span>

## <a name="change-the-site-name-in-the-qr-code"></a><span data-ttu-id="181b9-123">Modifier le nom du site dans le Code QR</span><span class="sxs-lookup"><span data-stu-id="181b9-123">Change the site name in the QR Code</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="181b9-124">Le nom du site dans le Code QR est effectuée à partir du nom de projet que vous choisissez lors de la création initiale de votre projet.</span><span class="sxs-lookup"><span data-stu-id="181b9-124">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="181b9-125">Vous pouvez le modifier en recherchant le `GenerateQrCodeUri(string email, string unformattedKey)` méthode dans le */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="181b9-125">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="181b9-126">Le nom du site dans le Code QR est effectuée à partir du nom de projet que vous choisissez lors de la création initiale de votre projet.</span><span class="sxs-lookup"><span data-stu-id="181b9-126">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="181b9-127">Vous pouvez le modifier en recherchant le `GenerateQrCodeUri(string email, string unformattedKey)` méthode dans le *Pages/Account/Manage/EnableAuthenticator.cshtml.cs* fichier (Pages Razor) ou le *Controllers/ManageController.cs* les fichier (MVC).</span><span class="sxs-lookup"><span data-stu-id="181b9-127">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the *Pages/Account/Manage/EnableAuthenticator.cshtml.cs* (Razor Pages) file or the *Controllers/ManageController.cs* (MVC) file.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="181b9-128">Le code par défaut à partir du modèle se présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="181b9-128">The default code from the template looks as follows:</span></span>

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

<span data-ttu-id="181b9-129">Le deuxième paramètre dans l’appel à `string.Format` est le nom de votre site, effectuée à partir de votre nom de la solution.</span><span class="sxs-lookup"><span data-stu-id="181b9-129">The second parameter in the call to `string.Format` is your site name, taken from your solution name.</span></span> <span data-ttu-id="181b9-130">Il peut être modifié à n’importe quelle valeur, mais il doit toujours être codée URL.</span><span class="sxs-lookup"><span data-stu-id="181b9-130">It can be changed to any value, but it must always be URL encoded.</span></span>

## <a name="using-a-different-qr-code-library"></a><span data-ttu-id="181b9-131">À l’aide d’une autre bibliothèque de Code QR</span><span class="sxs-lookup"><span data-stu-id="181b9-131">Using a different QR Code library</span></span>

<span data-ttu-id="181b9-132">Vous pouvez remplacer la bibliothèque de Code QR avec votre bibliothèque par défaut.</span><span class="sxs-lookup"><span data-stu-id="181b9-132">You can replace the QR Code library with your preferred library.</span></span> <span data-ttu-id="181b9-133">Le code HTML contient un `qrCode` élément dans lequel vous pouvez placer un Code QR par le mécanisme qui fournit de votre bibliothèque.</span><span class="sxs-lookup"><span data-stu-id="181b9-133">The HTML contains a `qrCode` element into which you can place a QR Code by whatever mechanism your library provides.</span></span>

<span data-ttu-id="181b9-134">L’URL correctement formatée pour le Code QR est disponible dans le :</span><span class="sxs-lookup"><span data-stu-id="181b9-134">The correctly formatted URL for the QR Code is available in the:</span></span>

* <span data-ttu-id="181b9-135">`AuthenticatorUri` propriété du modèle.</span><span class="sxs-lookup"><span data-stu-id="181b9-135">`AuthenticatorUri` property of the model.</span></span>
* <span data-ttu-id="181b9-136">`data-url` propriété dans le `qrCodeData` élément.</span><span class="sxs-lookup"><span data-stu-id="181b9-136">`data-url` property in the `qrCodeData` element.</span></span>

## <a name="totp-client-and-server-time-skew"></a><span data-ttu-id="181b9-137">Décalage horaire TOTP client et serveur</span><span class="sxs-lookup"><span data-stu-id="181b9-137">TOTP client and server time skew</span></span>

<span data-ttu-id="181b9-138">L’authentification TOTP (à usage unique mot de passe temporaire) dépend du périphérique à la fois le serveur et un authentificateur ayant une heure précise.</span><span class="sxs-lookup"><span data-stu-id="181b9-138">TOTP (Time-based One-Time Password) authentication depends on both the server and authenticator device having an accurate time.</span></span> <span data-ttu-id="181b9-139">Jetons durent uniquement pendant 30 secondes.</span><span class="sxs-lookup"><span data-stu-id="181b9-139">Tokens only last for 30 seconds.</span></span> <span data-ttu-id="181b9-140">Si des connexions de 2FA TOTP échouent, vérifiez que l’heure du serveur est précise et préférence synchronisée à un service NTP précis.</span><span class="sxs-lookup"><span data-stu-id="181b9-140">If TOTP 2FA logins are failing, check that the server time is accurate, and preferably synchronized to an accurate NTP service.</span></span>

::: moniker-end
