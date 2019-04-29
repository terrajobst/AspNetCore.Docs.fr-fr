---
title: Authentification à deux facteurs avec SMS dans ASP.NET Core
author: rick-anderson
description: Découvrez comment configurer l’authentification à deux facteurs (2FA) avec une application ASP.NET Core.
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 09/22/2018
ms.custom: mvc, seodec18
uid: security/authentication/2fa
ms.openlocfilehash: 9398cd3bb81eab7b427b19bbd5497630f4dc0838
ms.sourcegitcommit: 8a84ce880b4c40d6694ba6423038f18fc2eb5746
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60165194"
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a>Authentification à deux facteurs avec SMS dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [les développeurs de la Suisse](https://github.com/Swiss-Devs)

>[!WARNING]
> Deux facteurs (2FA) authentificateur applications d’authentification, à l’aide d’une heure basée à usage unique mot de passe algorithme définie (TOTP), sont le secteur de l’approche 2fa recommandé. À 2 facteurs à l’aide de TOTP est préférée à SMS 2FA. Pour plus d’informations, consultez [génération activer un Code QR pour les applications de l’authentificateur TOTP dans ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) pour ASP.NET Core 2.0 et versions ultérieures.

Ce didacticiel montre comment configurer l’authentification à deux facteurs (2FA) à l’aide de SMS. Obtenir des instructions sont données pour [twilio](https://www.twilio.com/) et [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), mais vous pouvez utiliser n’importe quel autre fournisseur SMS. Nous vous recommandons de suivre [Confirmation de compte et de récupération de mot de passe](xref:security/authentication/accconfirm) avant de commencer ce didacticiel.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA). [Comment télécharger](xref:index#how-to-download-a-sample).

## <a name="create-a-new-aspnet-core-project"></a>Créer un projet ASP.NET Core

Créer une application web ASP.NET Core nommée `Web2FA` avec les comptes d’utilisateur individuels. Suivez les instructions de <xref:security/enforcing-ssl> pour configurer et d’exiger s-HTTP.

### <a name="create-an-sms-account"></a>Créer un compte SMS

Créer un compte SMS, par exemple, à partir de [twilio](https://www.twilio.com/) ou [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/). Enregistrer les informations d’identification d’authentification (pour twilio : accountSid et authToken, pour ASPSMS : UserKey et mot de passe).

#### <a name="figuring-out-sms-provider-credentials"></a>Déterminer les informations d’identification du fournisseur SMS

**Twilio :**

Sous l’onglet tableau de bord de votre compte Twilio, copiez la **SID de compte** et **du jeton d’authentification**.

**ASPSMS :**

À partir des paramètres de votre compte, accédez à **Userkey** et copiez-le avec votre **mot de passe**.

Nous stockons ultérieurement ces valeurs avec l’outil Gestionnaire de secret dans les clés `SMSAccountIdentification` et `SMSAccountPassword`.

#### <a name="specifying-senderid--originator"></a>Spécifiant l’ID d’expéditeur / donneur d’ordre

**Twilio :** À partir de l’onglet numéros, copiez votre Twilio **numéro de téléphone**.

**ASPSMS :** Dans le Menu des expéditeurs déverrouiller, déverrouiller un ou plusieurs expéditeurs, ou choisissez un expéditeur d’alphanumériques (non pris en charge par tous les réseaux).

Nous stockons ultérieurement cette valeur avec l’outil Gestionnaire de la clé secrète au sein de la clé `SMSAccountFrom`.

### <a name="provide-credentials-for-the-sms-service"></a>Fournir des informations d’identification pour le service SMS

Nous allons utiliser le [modèle Options](xref:fundamentals/configuration/options) accéder aux paramètres de compte et clé utilisateur.

* Créez une classe pour extraire la clé SMS sécurisée. Pour cet exemple, le `SMSoptions` classe est créée dans le *Services/SMSoptions.cs* fichier.

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

Définir le `SMSAccountIdentification`, `SMSAccountPassword` et `SMSAccountFrom` avec la [outil secret manager](xref:security/app-secrets). Exemple :

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```

* Ajoutez le package NuGet pour le fournisseur SMS. À partir de la Manager Console (package) exécuter :

**Twilio :**

`Install-Package Twilio`

**ASPSMS :**

`Install-Package ASPSMS`

* Ajoutez le code dans le *Services/MessageServices.cs* fichier pour activer les SMS. Utilisez la Twilio ou la section ASPSMS :

**Twilio :** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

**ASPSMS :** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a>Configurer le démarrage à utiliser `SMSoptions`

Ajouter `SMSoptions` au conteneur de service dans le `ConfigureServices` méthode dans le *Startup.cs*:

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a>Activer l’authentification à deux facteurs

Ouvrez le *Views/Manage/Index.cshtml* fichier vue Razor et supprimer le commentaire caractères (donc aucun balisage n’est commenté).

## <a name="log-in-with-two-factor-authentication"></a>Se connecter avec l’authentification à deux facteurs

* Exécutez l’application et inscrire un nouvel utilisateur

![Affichage de Registre application Web ouverte dans Microsoft Edge](2fa/_static/login2fa1.png)

* Appuyez sur votre nom d’utilisateur, ce qui active le `Index` méthode d’action de contrôleur de gestion. Appuyez sur le numéro de téléphone **ajouter** lien.

![Gérer la vue - appuyez sur le lien « Ajouter »](2fa/_static/login2fa2.png)

* Ajouter un numéro de téléphone qui recevra le code de vérification et appuyez sur **envoyer le code de vérification**.

![Ajouter une page de numéro de téléphone](2fa/_static/login2fa3.png)

* Vous obtiendrez un message texte avec le code de vérification. Entrez-le, puis appuyez sur **envoyer**

![Vérifiez la page de numéro de téléphone](2fa/_static/login2fa4.png)

Si vous n’obtenez pas un message texte, consultez la page du journal twilio.

* La vue de gérer affiche que votre numéro de téléphone a été ajouté avec succès.

![Gestion de vue - numéro de téléphone a été ajouté avec succès](2fa/_static/login2fa5.png)

* Appuyez sur **activer** pour activer l’authentification à deux facteurs.

![Gérer la vue - activer l’authentification à deux facteurs](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a>Authentification à deux facteurs test

* Fermez la session.

* Connectez-vous.

* Le compte d’utilisateur a activé l’authentification à deux facteurs, donc vous devez fournir le second facteur d’authentification. Dans ce didacticiel vous avez activé la vérification par téléphone. Les modèles intégrés vous permettent également à configurer la messagerie en tant que le second facteur. Vous pouvez configurer d’autres facteurs deuxième pour l’authentification comme les codes QR. Appuyez sur **soumettre**.

![Envoyer l’affichage du Code de vérification](2fa/_static/login2fa7.png)

* Entrez le code que vous obtenez dans le message SMS.

* En cliquant sur le **mémoriser ce navigateur** case à cocher vous exempter de devoir utiliser 2FA pour se connecter lors de l’utilisation de l’appareil et l’Explorateur. L’activation à 2 facteurs et en cliquant sur **mémoriser ce navigateur** vous fournira 2FA forte protection contre les utilisateurs malveillants qui tente d’accéder à votre compte, tant qu’ils n’ont pas accès à votre appareil. Vous pouvez effectuer cela sur n’importe quel appareil privé que vous utilisez régulièrement. En définissant **mémoriser ce navigateur**, vous obtenez la sécurité supplémentaire de 2FA à partir d’appareils que vous n’utilisez pas régulièrement, et la commodité sur ne pas avoir à passer par 2FA sur vos propres appareils.

![Vérifier l’affichage](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a>Verrouillage de compte pour la protection contre les attaques par force brute

Verrouillage de compte est recommandé avec 2FA. Une fois qu’un utilisateur se connecte via un compte local ou un compte de réseau social, chaque tentative ayant échoué à 2 facteurs est stocké. Si les tentatives d’accès ayant échoué maximale est atteinte, l’utilisateur est verrouillé (par défaut : 5 minute verrouillage après que 5 tentatives infructueuses accès). Une authentification réussie réinitialise le nombre de tentatives d’accès ayant échoué et réinitialise l’horloge. Le nombre maximal de tentatives d’accès infructueuses et l’heure de verrouillage peut être définie avec [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) et [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan). Les éléments suivants configurent le verrouillage de compte pendant 10 minutes après que 10 échecs de tentatives d’accès :

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

Vérifiez que [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) définit `lockoutOnFailure` à `true`:

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
