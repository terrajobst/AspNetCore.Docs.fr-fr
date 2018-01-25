---
title: "Activer la génération de Code QR pour les applications d’authentification dans ASP.NET Core"
author: rick-anderson
description: "Activer la génération de Code QR pour les applications d’authentification dans ASP.NET Core"
ms.author: riande
manager: wpickett
ms.date: 09/24/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: d3710e72f3f4f2a5ecc4cfa53f721cca5239aa70
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
# <a name="enabling-qr-code-generation-for-authenticator-apps-in-aspnet-core"></a><span data-ttu-id="c9efd-103">Activer la génération de Code QR pour les applications d’authentification dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c9efd-103">Enabling QR Code generation for authenticator apps in ASP.NET Core</span></span>

<span data-ttu-id="c9efd-104">Remarque : Cette rubrique s’applique à ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c9efd-104">Note: This topic applies to ASP.NET Core 2.x</span></span>

<span data-ttu-id="c9efd-105">ASP.NET Core est livré avec prise en charge pour les applications de l’authentificateur pour l’authentification individuelle.</span><span class="sxs-lookup"><span data-stu-id="c9efd-105">ASP.NET Core ships with support for authenticator applications for individual authentication.</span></span> <span data-ttu-id="c9efd-106">Deux applications-Factor authentication (2FA) l’authentificateur, à l’aide d’une durée à usage unique mot de passe algorithme (TOTP), sont l’approche pour 2FA recommandée.</span><span class="sxs-lookup"><span data-stu-id="c9efd-106">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="c9efd-107">2FA à l’aide de TOTP est préférée à 2FA SMS.</span><span class="sxs-lookup"><span data-stu-id="c9efd-107">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="c9efd-108">Une application d’authentification fournit un code de 6 à 8 chiffres que les utilisateurs doivent entrer après confirmation de leur nom d’utilisateur et un mot de passe.</span><span class="sxs-lookup"><span data-stu-id="c9efd-108">An authenticator app provides a 6 to 8 digit code which users must enter after confirming their username and password.</span></span> <span data-ttu-id="c9efd-109">En règle générale, une application d’authentification est installée sur un Smartphone.</span><span class="sxs-lookup"><span data-stu-id="c9efd-109">Typically an authenticator app is installed on a smart phone.</span></span>

<span data-ttu-id="c9efd-110">Les modèles d’application web ASP.NET Core prennent en charge des authentificateurs, mais ne fournissent pas la prise en charge pour la génération de QRCode.</span><span class="sxs-lookup"><span data-stu-id="c9efd-110">The ASP.NET Core web app templates support authenticators, but don't provide support for QRCode generation.</span></span> <span data-ttu-id="c9efd-111">Les générateurs de QRCode facilitent le programme d’installation de 2FA.</span><span class="sxs-lookup"><span data-stu-id="c9efd-111">QRCode generators ease the setup of 2FA.</span></span> <span data-ttu-id="c9efd-112">Ce document va vous guider ajout [Code QR](https://wikipedia.org/wiki/QR_code) génération à la page de configuration 2FA.</span><span class="sxs-lookup"><span data-stu-id="c9efd-112">This document will guide you through adding [QR Code](https://wikipedia.org/wiki/QR_code) generation to the 2FA configuration page.</span></span>

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a><span data-ttu-id="c9efd-113">Ajout des Codes QR à la page de configuration 2FA</span><span class="sxs-lookup"><span data-stu-id="c9efd-113">Adding QR Codes to the 2FA configuration page</span></span>

<span data-ttu-id="c9efd-114">Ces instructions utilisent *qrcode.js* du référentiel https://davidshimjs.github.io/qrcodejs/.</span><span class="sxs-lookup"><span data-stu-id="c9efd-114">These instructions use *qrcode.js* from the https://davidshimjs.github.io/qrcodejs/ repo.</span></span>

* <span data-ttu-id="c9efd-115">Téléchargez le [bibliothèque javascript pour qrcode.js](https://davidshimjs.github.io/qrcodejs/) à la `wwwroot\lib` dossier dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="c9efd-115">Download the [qrcode.js javascript library](https://davidshimjs.github.io/qrcodejs/) to the `wwwroot\lib` folder in your project.</span></span>

* <span data-ttu-id="c9efd-116">Dans *Pages\Account\Manage\EnableAuthenticator.cshtml* (Pages Razor) ou *Views\Manage\EnableAuthenticator.cshtml* (MVC), recherchez le `Scripts` section à la fin du fichier :</span><span class="sxs-lookup"><span data-stu-id="c9efd-116">In *Pages\Account\Manage\EnableAuthenticator.cshtml* (Razor Pages) or *Views\Manage\EnableAuthenticator.cshtml* (MVC), locate the `Scripts` section at the end of the file:</span></span>

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* <span data-ttu-id="c9efd-117">Mise à jour la `Scripts` section pour ajouter une référence à la `qrcodejs` bibliothèque que vous avez ajouté et un appel pour générer le Code QR.</span><span class="sxs-lookup"><span data-stu-id="c9efd-117">Update the `Scripts` section to add a reference to the `qrcodejs` library you added and a call to generate the QR Code.</span></span> <span data-ttu-id="c9efd-118">Il doit se présenter comme suit :</span><span class="sxs-lookup"><span data-stu-id="c9efd-118">It should look as follows:</span></span>

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

* <span data-ttu-id="c9efd-119">Supprimer le paragraphe qui propose des liens vers ces instructions.</span><span class="sxs-lookup"><span data-stu-id="c9efd-119">Delete the paragraph which links you to these instructions.</span></span>

<span data-ttu-id="c9efd-120">Exécuter votre application et vous assurer que vous pouvez analyser le code QR et valider le code que destiné à prouver l’authentificateur.</span><span class="sxs-lookup"><span data-stu-id="c9efd-120">Run your app and ensure that you can scan the QR code and validate the code the authenticator proves.</span></span>

## <a name="change-the-site-name-in-the-qr-code"></a><span data-ttu-id="c9efd-121">Modifier le nom du site dans le Code QR</span><span class="sxs-lookup"><span data-stu-id="c9efd-121">Change the site name in the QR Code</span></span>

<span data-ttu-id="c9efd-122">Le nom du site dans le Code QR provient du nom du projet que vous sélectionnez lorsque vous créez votre projet.</span><span class="sxs-lookup"><span data-stu-id="c9efd-122">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="c9efd-123">Vous pouvez le modifier en recherchant le `GenerateQrCodeUri(string email, string unformattedKey)` méthode dans le *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* les fichiers (Pages Razor) ou le *Controllers\ManageController.cs* fichier (MVC).</span><span class="sxs-lookup"><span data-stu-id="c9efd-123">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* (Razor Pages) file or the *Controllers\ManageController.cs* (MVC) file.</span></span> 

<span data-ttu-id="c9efd-124">Le code à partir du modèle par défaut se présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="c9efd-124">The default code from the template looks as follows:</span></span>

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

<span data-ttu-id="c9efd-125">Le deuxième paramètre dans l’appel à `string.Format` est le nom de votre site, obtenue à partir de votre nom de la solution.</span><span class="sxs-lookup"><span data-stu-id="c9efd-125">The second parameter in the call to `string.Format` is your site name, taken from your solution name.</span></span> <span data-ttu-id="c9efd-126">Il peut être modifié par n’importe quelle valeur, mais il doit toujours être codé URL.</span><span class="sxs-lookup"><span data-stu-id="c9efd-126">It can be changed to any value, but it must always be URL encoded.</span></span>

## <a name="using-a-different-qr-code-library"></a><span data-ttu-id="c9efd-127">À l’aide d’une autre bibliothèque de Code QR</span><span class="sxs-lookup"><span data-stu-id="c9efd-127">Using a different QR Code library</span></span>

<span data-ttu-id="c9efd-128">Vous pouvez remplacer la bibliothèque de Code QR avec la bibliothèque par défaut.</span><span class="sxs-lookup"><span data-stu-id="c9efd-128">You can replace the QR Code library with your preferred library.</span></span> <span data-ttu-id="c9efd-129">Le code HTML contient un `qrCode` élément dans lequel vous pouvez placer un Code QR par le mécanisme de votre bibliothèque fournit.</span><span class="sxs-lookup"><span data-stu-id="c9efd-129">The HTML contains a `qrCode` element into which you can place a QR Code by whatever mechanism your library provides.</span></span>

<span data-ttu-id="c9efd-130">L’URL correcte pour le Code QR est disponible dans le :</span><span class="sxs-lookup"><span data-stu-id="c9efd-130">The correctly formatted URL for the QR Code is available in the:</span></span>

* <span data-ttu-id="c9efd-131">`AuthenticatorUri`propriété du modèle.</span><span class="sxs-lookup"><span data-stu-id="c9efd-131">`AuthenticatorUri` property of the model.</span></span>
* <span data-ttu-id="c9efd-132">`data-url`propriété dans le `qrCodeData` élément.</span><span class="sxs-lookup"><span data-stu-id="c9efd-132">`data-url` property in the `qrCodeData` element.</span></span> 

<span data-ttu-id="c9efd-133">Utilisez `@Html.Raw` pour accéder à la propriété de modèle dans une vue (sinon les signes & dans l’url doit être encodés en double et le paramètre d’étiquette du Code QR est ignoré).</span><span class="sxs-lookup"><span data-stu-id="c9efd-133">Use `@Html.Raw` to access the model property in a view (otherwise the ampersands in the url will be double encoded and the label parameter of the QR Code will be ignored).</span></span>

## <a name="totp-client-and-server-time-skew"></a><span data-ttu-id="c9efd-134">Décalage horaire TOTP client et serveur</span><span class="sxs-lookup"><span data-stu-id="c9efd-134">TOTP client and server time skew</span></span>

<span data-ttu-id="c9efd-135">Authentification de TOTP dépend du périphérique à la fois le serveur et un authentificateur ayant une heure précise.</span><span class="sxs-lookup"><span data-stu-id="c9efd-135">TOTP authentication depends on both the server and authenticator device having an accurate time.</span></span> <span data-ttu-id="c9efd-136">Les jetons ne durent que 30 secondes.</span><span class="sxs-lookup"><span data-stu-id="c9efd-136">Tokens only last for 30 seconds.</span></span> <span data-ttu-id="c9efd-137">Si des connexions de 2FA TOTP échouent, vérifiez que l’heure du serveur est précises et, de préférence, synchronisées avec un service NTP précis.</span><span class="sxs-lookup"><span data-stu-id="c9efd-137">If TOTP 2FA logins are failing, check that the server time is accurate, and preferably synchronized to an accurate NTP service.</span></span>
