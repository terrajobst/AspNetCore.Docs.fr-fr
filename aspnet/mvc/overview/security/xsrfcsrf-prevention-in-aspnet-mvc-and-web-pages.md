---
uid: mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
title: Prévention de XSRF/CSRF dans ASP.NET MVC et Web Pages | Microsoft Docs
author: Rick-Anderson
description: Falsification de requête intersites (également appelé XSRF ou CSRF) est une attaque contre les applications hébergées sur le web par laquelle un site web malveillant peut influencer l’interacti...
ms.author: riande
ms.date: 03/14/2013
ms.assetid: aadc5fa4-8215-4fc7-afd5-bcd2ef879728
msc.legacyurl: /mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
msc.type: authoredcontent
ms.openlocfilehash: cd1b8de51c180471ab273c4541959368ffbd48a3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833434"
---
<a name="xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages"></a>Prévention de XSRF/CSRF dans ASP.NET MVC et Web Pages
====================
par [Rick Anderson](https://github.com/Rick-Anderson)

> Requête intersites falsification (également appelé XSRF ou CSRF) est une attaque contre les applications hébergées sur le web par laquelle un site web malveillant peut influencer l’interaction entre un navigateur client et un site web approuvé par ce navigateur. Ces attaques sont rendues possibles, car les navigateurs web envoient automatiquement avec chaque demande de jetons d’authentification à un site web. L’exemple canonique est un cookie d’authentification, tels que ASP. Ticket d’authentification par formulaire du NET. Toutefois, les sites web qui utilisent n’importe quel mécanisme d’authentification persistant (par exemple, l’authentification Windows, Basic et ainsi de suite) peuvent être ciblés par ces attaques.
> 
> Une attaque XSRF est distincte à partir d’une attaque par phishing. Les attaques par hameçonnage requièrent une interaction avec la victime. Dans une attaque par phishing, un site web malveillant va imiter le site web cible et la victime est dupée pour fournir des informations sensibles à la personne malveillante. Dans une attaque XSRF, aucune interaction n’est souvent nécessaire de la victime. Au lieu de cela, l’attaquant se repose sur le navigateur d’envoyer automatiquement tous les cookies utiles pour le site de destination.
> 
> Pour plus d’informations, consultez le [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page)(OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)).


## <a name="anatomy-of-an-attack"></a>Anatomie d’une attaque

Pour parcourir une attaque XSRF, envisagez d’un utilisateur qui veut effectuer certaines transactions bancaires en ligne. Tout d’abord, cet utilisateur visite WoodgroveBank.com et journaux, à quel point l’en-tête de réponse contiendra le cookie d’authentification :

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample1.cmd)]

Étant donné que le cookie d’authentification est un cookie de session, il sera automatiquement effacé par le navigateur lorsque le processus de navigateur se ferme. Toutefois, en attendant, le navigateur inclut automatiquement le cookie avec chaque demande WoodgroveBank.com. L’utilisateur veut maintenant transférer 1 000 $ à un autre compte, afin qu’elle remplit un formulaire sur le site bancaire et le navigateur effectue cette demande au serveur :

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample2.cmd)]

Étant donné que cette opération a un effet secondaire (il lance une transaction monétaire), le site bancaire a choisi exiger une requête HTTP POST afin de lancer cette opération. Le serveur lit le jeton d’authentification à partir de la demande, recherche de numéro de compte de l’utilisateur actuel, vérifie que provision existe, puis lance la transaction dans le compte de destination.

Elle bancaires en ligne terminée, l’utilisateur navigue en dehors du site bancaire et visite d’autres emplacements sur le web. Un de ces sites – fabrikam.com – inclut le balisage suivant sur une page incorporée au sein d’un &lt;iframe&gt;:

[!code-html[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample3.html)]

Ce qui provoque le navigateur effectuer cette demande :

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample4.cmd)]

L’attaquant exploite le fait que l’utilisateur peut avoir toujours un jeton d’authentification valide pour le site web cible, et elle est à l’aide d’un petit extrait de code Javascript pour provoquer le navigateur à effectuer une requête HTTP POST vers le site cible automatiquement. Si le jeton d’authentification est toujours valide, le site bancaire lance un transfert de 250 $ dans le compte de son choix.

### <a name="ineffective-mitigations"></a>Atténuations inefficaces

Il est intéressant de noter que dans le scénario ci-dessus, le fait que WoodgroveBank.com a été accédée via SSL et avait un cookie d’authentification SSL uniquement a été insuffisant pour contrecarrer l’attaque. L’attaquant est en mesure de spécifier le [schéma d’URI](http://en.wikipedia.org/wiki/URI_scheme) (https) dans son &lt;formulaire&gt; élément et le navigateur continue à envoyer les cookies n’ont pas expirés sur le site cible tant que ces cookies sont cohérents avec l’URI schéma de la cible prévue.

Disent que l’utilisateur ne doit simplement pas consulter les sites non approuvés, en tant que consultant uniquement des sites de confiance est vous aide à rester en ligne sans échec. Il existe une part de vérité, mais malheureusement ce Conseil n’est pas toujours pratique. Peut-être l’utilisateur « approuve » le site d’actualités régionales ConsolidatedMessenger. ConsolidatedMessenger.com et est redirigée vers la visite de site au lieu de cela, mais ce site a une vulnérabilité XSS qui permet à un attaquant d’injecter l’extrait de code qui s’exécutait sur fabrikam.com.

Vous pouvez vous assurer que les demandes entrantes un [en-tête Referer](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) faisant référence à votre domaine. Cela empêchera les demandes envoyées involontairement à partir d’un domaine tiers. Toutefois, certaines personnes désactiver l’en-tête du référent de son navigateur pour des raisons de confidentialité et les attaquants peuvent parfois usurper l’identité de cet en-tête si la victime a certains logiciels non sécurisé installé. Vérification de la [en-tête Referer](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) n’est pas considéré comme une approche sécurisée si l'on souhaite empêcher les attaques XSRF.

## <a name="web-stack-runtime-xsrf-mitigations"></a>Atténuations de pile Runtime XSRF Web

Le Runtime de pile Web ASP.NET utilise une variante de la [modèle de jeton synchronisateur](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet#General_Recommendation:_Synchronizer_Token_Pattern) pour vous défendre contre les attaques XSRF. La forme générale du modèle de jeton synchronisateur est que les deux jetons anti-XSRF sont soumises au serveur avec chaque requête HTTP POST (en plus du jeton d’authentification) : un seul jeton comme un cookie et l’autre comme valeur de l’écran. Les valeurs de jeton générés par le runtime ASP.NET ne sont pas déterministes ou prévisible par une personne malveillante. Lorsque les jetons sont envoyés, le serveur autorisera la demande continuer uniquement si les deux jetons réussir une vérification de la comparaison.

La vérification de la demande XSRF *jeton de session* sont stockées sous la forme d’un cookie HTTP et actuellement contient les informations suivantes dans sa charge utile :

- Un jeton de sécurité, consistant en un identificateur aléatoire de 128 bits.   
 L’illustration suivante montre le jeton de session de demande XSRF vérification affiché avec les outils de développement F12 d’Internet Explorer : (Notez il s’agit de l’implémentation actuelle et est soumise, même probable, à modifier.)

![](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/_static/image1.png)

Le *token pole* est stockée comme un `<input type="hidden" />` et contient les informations suivantes dans sa charge utile :

- Nom d’utilisateur de l’utilisateur connecté (si authentifié).
- Les données supplémentaires fournies par un [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx).

Les charges utiles des jetons anti-XSRF sont chiffrés et signés, donc vous ne pouvez pas afficher le nom d’utilisateur lors de l’utilisation des outils pour examiner les jetons. Lors de l’application web cible ASP.NET 4.0, les services de chiffrement sont fournies par le [MachineKey.Encode](https://msdn.microsoft.com/library/system.web.security.machinekey.encode.aspx) routine. Lorsque l’application web cible ASP.NET 4.5 ou version ultérieure, le chiffrement des services sont fournis par le [MachineKey.Protect](https://msdn.microsoft.com/library/system.web.security.machinekey.protect(v=vs.110)) routine, qui offre de meilleures performances, extensibilité et la sécurité. Consultez que le blog suivant publie pour plus d’informations :

- [Améliorations de chiffrement dans ASP.NET 4.5, pt. 1](https://blogs.msdn.com/b/webdev/archive/2012/10/22/cryptographic-improvements-in-asp-net-4-5-pt-1.aspx)
- [Améliorations de chiffrement dans ASP.NET 4.5, pt. 2](https://blogs.msdn.com/b/webdev/archive/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2.aspx)
- [Améliorations de chiffrement dans ASP.NET 4.5, pt. 3](https://blogs.msdn.com/b/webdev/archive/2012/10/24/cryptographic-improvements-in-asp-net-4-5-pt-3.aspx)

## <a name="generating-the-tokens"></a>Générer les jetons

Pour générer les jetons anti-XSRF, appelez le [ @Html.AntiForgeryToken ](https://msdn.microsoft.com/library/dd470175.aspx) méthode à partir d’une vue MVC ou @AntiForgery.GetHtml() à partir d’une page Razor. Le runtime effectue ensuite les étapes suivantes :

1. Si la requête HTTP actuelle contient déjà un jeton de session anti-XSRF (le cookie anti-XSRF \_ \_RequestVerificationToken), le jeton de sécurité est extrait à partir de celui-ci. Si la requête HTTP ne contient-elle pas d’un jeton de session anti-XSRF ou si l’extraction du jeton de sécurité échoue, un nouveau jeton anti-XSRF aléatoire sera généré.
2. Un jeton de champ anti-XSRF est généré en utilisant le jeton de sécurité à l’étape (1) ci-dessus et l’identité de l’utilisateur connecté actuel. (Pour plus d’informations sur la détermination d’identité de l’utilisateur, consultez le **[scénarios avec prise en charge spéciale](#_Scenarios_with_special)** section ci-dessous.) En outre, si un [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/jj158328(v=vs.111).aspx) est configuré, le runtime appelle sa [GetAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.getadditionaldata(v=vs.111).aspx) (méthode) et inclure la chaîne retournée dans le jeton de champ. (Consultez le **[Configuration et l’extensibilité](#_Configuration_and_extensibility)** section pour plus d’informations.)
3. Si un nouveau jeton anti-XSRF a été généré à l’étape (1), un nouveau jeton de session est créé pour contenir et sera ajouté à la collection de cookies HTTP sortante. Le jeton de champ de l’étape (2) est encapsulé dans un `<input type="hidden" />` élément et ce balisage HTML sera la valeur de retour de `Html.AntiForgeryToken()` ou `AntiForgery.GetHtml()`.

## <a name="validating-the-tokens"></a>Valider les jetons

Pour valider les jetons anti-XSRF entrant, le développeur inclut un [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(VS.108).aspx) attribut sur son action MVC, contrôleur ou les appels she `@AntiForgery.Validate()` à partir de sa page Razor. Le runtime effectue les étapes suivantes :

1. Le jeton de session entrant et le jeton de champ sont lus et le jeton anti-XSRF extraites à partir de chacun. Les jetons anti-XSRF doivent être identiques par étape (2) dans la routine de génération.
2. Si l’utilisateur actuel est authentifié, son nom d’utilisateur est comparée avec le nom d’utilisateur stocké dans le jeton de champ. Les noms d’utilisateur doivent correspondre.
3. Si un [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) est configuré, le runtime appelle sa *ValidateAdditionalData* (méthode). La méthode doit retourner la valeur booléenne *true*.

Si la validation réussit, la demande est autorisée à continuer. Si la validation échoue, le framework lèvera une *HttpAntiForgeryException*.

## <a name="failure-conditions"></a>Conditions d’échec

En commençant par le Runtime de pile Web ASP.NET v2, toute *HttpAntiForgeryException* qui est levée au cours de validation contiendra des informations détaillées sur la cause du problème. Les conditions d’échec actuellement définies sont :

- Le jeton de session ou un jeton de formulaire n’est pas présent dans la demande.
- Le jeton de session ou un jeton de formulaire est illisible. La cause la plus probable est une batterie de serveurs exécutant des versions incompatibles de l’exécution de pile Web ASP.NET ou une batterie de serveurs où le &lt;machineKey&gt; élément dans le fichier Web.config est différent entre les machines. Vous pouvez utiliser un outil tel que Fiddler pour forcer cette exception à l’altération de des jetons anti-XSRF.
- Le jeton de session et le jeton de champ ont été permutées.
- Le jeton de session et le jeton de champ contiennent des jetons de sécurité qui ne correspondent pas.
- Le nom d’utilisateur incorporée dans le jeton de champ ne correspond pas de nom d’utilisateur connecté l’utilisateur actuel.
- Le *[IAntiForgeryAdditionalDataProvider.ValidateAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.validateadditionaldata(v=vs.111).aspx)* méthode retournée *false*.

Les fonctionnalités anti-XSRF peuvent également effectuer des vérifications supplémentaires lors de la génération de jetons ou de validation et les échecs au cours de ces vérifications peuvent entraîner des exceptions sont levées. Consultez le [WIF / ACS / basée sur les revendications authentification](#_WIF_ACS) et **[Configuration et l’extensibilité](#_Configuration_and_extensibility)** sections pour plus d’informations.

<a id="_Scenarios_with_special"></a>

## <a name="scenarios-with-special-support"></a>Scénarios avec prise en charge spéciale

### <a name="anonymous-authentication"></a>Authentification anonyme

Le système anti-XSRF contient la prise en charge spéciale pour les utilisateurs anonymes, où « anonyme » est définie en tant qu’utilisateur où le *sur IIdentity.IsAuthenticated* retourne de la propriété *false*. Les scénarios incluent une protection XSRF à la page de connexion (avant que l’utilisateur est authentifié) et les schémas d’authentification personnalisés où l’application utilise un mécanisme autre que *IIdentity* pour identifier les utilisateurs.

Pour prendre en charge ces scénarios, rappelez-vous que les jetons de session et de champ sont jointes à un jeton de sécurité, qui est un identificateur opaque de 128 bits générée de manière aléatoire. Ce jeton de sécurité est utilisé pour effectuer le suivi de session d’un utilisateur individuel comme elle accède le site, afin qu’il joue le rôle d’un identificateur anonyme pour l’efficacement. Une chaîne vide est utilisée à la place le nom d’utilisateur pour les routines de génération et de validation décrites ci-dessus.

<a id="_WIF_ACS"></a>

### <a name="wif--acs--claims-based-authentication"></a>WIF / ACS / basée sur les revendications authentification

Normalement, le *IIdentity* classes intégrées au .NET Framework ont la propriété qui *IIdentity.Name* est suffisant pour identifier de manière unique un utilisateur particulier dans une application donnée. Par exemple, *FormsIdentity.Name* retourne le nom d’utilisateur stocké dans la base de données d’appartenance (qui est unique pour toutes les applications en fonction de cette base de données), *WindowsIdentity.Name* retourne le identité de domaine qualifié de l’utilisateur et ainsi de suite. Ces systèmes fournissent non seulement l’authentification ; ils également *identifier* aux utilisateurs d’une application.

L’authentification basée sur les revendications, en revanche, ne requiert pas nécessairement identifiant un utilisateur particulier. Au lieu de cela, le *ClaimsPrincipal* et *ClaimsIdentity* types sont associés à un ensemble de *revendication* instances, où les revendications individuelles peuvent être « est de 18 ans d’âge » ou » est un administrateur à autre chose. » Étant donné que l’utilisateur n’a pas nécessairement été identifié, le runtime ne peut pas utiliser le *ClaimsIdentity.Name* propriété comme identificateur unique pour cet utilisateur particulier. L’équipe a constaté des exemples réels où *ClaimsIdentity.Name* retourne *null*, retourne un nom convivial (complet), ou sinon retourne une chaîne qui n’est pas appropriée pour une utilisation comme identificateur unique pour l’utilisateur.

La plupart des déploiements qui utilisent l’authentification basée sur les revendications sont à l’aide de [Azure Access Control Service](https://msdn.microsoft.com/library/windowsazure/gg429786.aspx) (ACS) en particulier. ACS permet au développeur de configurer individuels *fournisseurs d’identité* (tels qu’ADFS, le fournisseur de Microsoft Account fournisseurs OpenID comme Yahoo!, etc.), et les fournisseurs d’identité retournent *nommer les identificateurs*. Ces identificateurs de nom peuvent contenir des informations personnellement identifiables (PII) comme une adresse de messagerie, ou il pourraient être présentées de façon anonyme comme un identificateur PPID (Private Personal). Malgré tout, le tuple (fournisseur d’identité, identificateur de nom) suffisamment sert un jeton de suivi approprié pour un utilisateur particulier pendant qu’elle parcourt le site, afin que le Runtime de pile Web ASP.NET peut utiliser le tuple à la place le nom d’utilisateur lors de la génération et validation des jetons de champ d’anti-XSRF. Les URI particulier pour le fournisseur d’identité et l’identificateur de nom sont :

- `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`
- `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`

(consultez ce [page de documentation ACS](https://msdn.microsoft.com/library/windowsazure/gg185971.aspx) pour plus d’informations.)

Lorsque vous générez ou validation d’un jeton, le service d’exécution ASP.NET Web Stack essaient lors de l’exécution liaison aux types :

- `Microsoft.IdentityModel.Claims.IClaimsIdentity, Microsoft.IdentityModel, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35` (Pour le kit SDK WIF.)
- `System.Security.Claims.ClaimsIdentity` (Pour .NET 4.5).

Si ces types existent et si l’utilisateur actuel *IIIIdentity* implémente ou sous-classes un de ces types, la fonctionnalité anti-XSRF utilisera (fournisseur d’identité, identificateur de nom) tuple à la place le nom d’utilisateur lors de la génération et valider les jetons. Si aucun tuple de ce type n’est présent, la demande échoue avec une erreur décrivant au développeur comment configurer le système anti-XSRF pour comprendre le mécanisme d’authentification basée sur les revendications particulier en cours d’utilisation. Consultez le **[Configuration et l’extensibilité](#_Configuration_and_extensibility)** section pour plus d’informations.

### <a name="oauth--openid-authentication"></a>OAuth / OpenID authentification

Enfin, l’installation d’anti-XSRF dispose d’une prise en charge spéciale pour les applications qui utilisent l’authentification OAuth ou OpenID. Cette prise en charge est basé sur l’heuristique : si actuel *IIdentity.Name* commence par http:// ou https://, puis les comparaisons de nom d’utilisateur seront effectuées à l’aide d’un comparateur Ordinal plutôt que le comparateur d’OrdinalIgnoreCase par défaut.

<a id="_Configuration_and_extensibility"></a>

## <a name="configuration-and-extensibility"></a>Configuration et l’extensibilité

Parfois, les développeurs voudront un contrôle plus strict sur la génération d’anti-XSRF et les comportements de validation. Par exemple, peut-être par défaut helpers MVC et les Pages Web de l’ajout automatique des cookies HTTP pour la réponse n’est pas souhaitable, et le développeur pouvez souhaiter conserver les jetons ailleurs. Il existe deux API pour vous y aider :

`AntiForgery.GetTokens(string oldCookieToken, out string newCookieToken, out string formToken);`  
`AntiForgery.Validate(string cookieToken, string formToken);`

Le *GetTokens* méthode prend comme entrée un XSRF demande vérification session jeton existant (qui peut être null) et produit comme sortie un nouveau jeton de session de vérification XSRF demande et le jeton de champ. Les jetons sont des chaînes opaques simplement sans ornement ; le *formToken* valeur est par exemple pas encapsulée dans un &lt;d’entrée&gt; balise. Le *newCookieToken* valeur peut être null ; si cela se produit, puis le *oldCookieToken* valeur est toujours valide et aucun nouveau cookie de réponse ne doive être définie. L’appelant de *GetTokens* est responsable de la persistance des cookies de réponse nécessaire ou en générant tout balisage nécessaire ; le *GetTokens* méthode proprement dite ne modifiera en rien la réponse sous la forme d’un effet secondaire. Le *Validate* méthode accepte la session entrante et champ des jetons et exécute la logique de validation susmentionné au-dessus d’eux.

### <a name="antiforgeryconfig"></a>AntiForgeryConfig

Le développeur peut configurer le système anti-XSRF à partir de l’Application\_Démarrer. La configuration est par programmation. Les propriétés de la méthode statique *AntiForgeryConfig* type sont décrits ci-dessous. Vous devez définir la propriété UniqueClaimTypeIdentifier la plupart des utilisateurs à l’aide de revendications.

| **Property** | **Description** |
| --- | --- |
| **AdditionalDataProvider** | Un [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) qui fournit des données supplémentaires pendant la génération de jetons et consomme des données supplémentaires pendant la validation du jeton. La valeur par défaut est *null*. Pour plus d’informations, consultez le [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) section. |
| **CookieName** | Chaîne qui fournit le nom du cookie HTTP qui est utilisé pour stocker le jeton de session anti-XSRF. Si cette valeur n’est pas définie, un nom est automatiquement généré en fonction de chemin d’accès virtuel de l’application déployée. La valeur par défaut est *null*. |
| **RequireSsl** | Valeur booléenne qui détermine si les jetons anti-XSRF sont requis pour être soumis via un canal sécurisé SSL. Si cette valeur est *true*, tous les cookies généré automatiquement a l’indicateur « sécurisé » définie, et les API anti-XSRF lève si appelée à partir d’une demande qui n’est pas envoyée via SSL. La valeur par défaut est *false*. |
| **SuppressIdentityHeuristicChecks** | Valeur booléenne qui détermine si le système anti-XSRF doit se désactiver sa prise en charge pour les identités basées sur les revendications. Si cette valeur est *true*, le système suppose dès lors que *IIdentity.Name* est approprié pour une utilisation comme un identificateur d’utilisateur unique et n’essaie pas de cas spéciaux *IClaimsIdentity*ou *ClClaimsIdentity* comme décrit dans la [WIF / ACS / basée sur les revendications authentification](#_WIF_ACS) section. La valeur par défaut est `false`. |
| **UniqueClaimTypeIdentifier** | Une chaîne qui indique les revendications de type est appropriée pour une utilisation en tant qu’un identificateur d’utilisateur unique. Si cette valeur est définie et les cours *IIdentity* -basée sur les revendications, le système tente d’extraire une revendication du type spécifié par *UniqueClaimTypeIdentifier*, et la valeur correspondante sera utilisée. à la place de l’utilisateur lors de la génération du jeton de champ. Si le type de revendication n’est pas trouvé, le système fait échouer la requête. La valeur par défaut est *null*, ce qui indique que le système doit utiliser (fournisseur d’identité, identificateur de nom) tuple comme décrit précédemment à la place de l’utilisateur. |

<a id="_IAntiForgeryAdditionalDataProvider"></a>

### <a name="iantiforgeryadditionaldataprovider"></a>IAntiForgeryAdditionalDataProvider

Le *[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)* type permet aux développeurs d’étendre le comportement du système anti-XSRF par les données supplémentaires de l’aller-retour dans chaque jeton. Le *GetAdditionalData* méthode est appelée chaque fois qu’un jeton de champ est généré, et la valeur de retour est incorporée dans le jeton généré. Un implémenteur peut retourner un horodatage, une valeur à usage unique ou toute autre valeur si qu'elle souhaite à partir de cette méthode.

De même, le *ValidateAdditionalData* méthode est appelée chaque fois qu’un jeton de champ est validé, et la chaîne « données supplémentaires » qui a été incorporée dans le jeton est passée à la méthode. La routine de validation peut implémenter un délai d’expiration (en vérifiant l’heure actuelle par rapport à l’heure à laquelle a été stocké lorsque le jeton a été créé), vous le souhaitez une valeur à usage unique, la vérification de la routine ou toute autre logique.

## <a name="design-decisions-and-security-considerations"></a>Considérations de sécurité et les décisions de conception

Le jeton de sécurité qui lie les jetons de session et le champ est techniquement nécessaire uniquement lorsque vous tentez de protéger les utilisateurs anonymes / non authentifiés contre les attaques XSRF. Lorsque l’utilisateur est authentifié, le jeton d’authentification proprement dit (vraisemblablement soumis sous la forme d’un cookie) peut être utilisé comme une moitié d’un synchronisateur de paire de jetons. Toutefois, il existe des scénarios valides pour la protection des pages de connexion d’accès par les utilisateurs non authentifiés, et la logique d’anti-XSRF a été effectuée plus simple en générant et en validant le jeton de sécurité, même pour les utilisateurs authentifiés toujours. Il fournit une protection supplémentaire au cas où un jeton de champ est compromis par une personne malveillante, en tant que paramètre ou en devinant que le jeton de session serait également possible pour l’attaquant de surmonter.

Les développeurs doivent utiliser attention lorsque plusieurs applications sont hébergées dans un seul domaine. Par exemple, même si *example1.cloudapp.net* et *example2.cloudapp.net* sont des hôtes différents, il existe une relation de confiance implicite entre tous les hôtes sous le  *\*. cloudapp.net* domaine. Cette relation de confiance implicite [permet aux hôtes potentiellement non fiables affecter des cookies entre eux](http://stackoverflow.com/questions/9636857/how-can-asp-net-or-asp-net-mvc-be-protected-from-related-domain-cookie-attacks) (les stratégies de même origine qui régissent les demandes AJAX ne pas nécessairement s’appliquent pour les cookies HTTP). Le Runtime de pile Web ASP.NET fournit certaines atténuation dans la mesure où le nom d’utilisateur est incorporée dans le jeton de champ, même si un sous-domaine malveillant est en mesure de remplacer un jeton de session qu’il est donc impossible de générer un jeton de champ valide pour l’utilisateur. Toutefois, quand ils sont hébergés dans un tel environnement les routines intégrées anti-XSRF toujours ne peut pas se défendre contre au piratage de session ou connexion XSRF.

Les routines d’anti-XSRF n’actuellement pas défendre contre [le détournement de clics](https://www.owasp.org/index.php/Clickjacking). Applications qui souhaitent se défendre contre le détournement de clics peuvent faire facilement en envoyant un X-Frame-Options : en-tête SAMEORIGIN avec chaque réponse. Cet en-tête est pris en charge par tous les navigateurs récents. Pour plus d’informations, consultez le [blog IE](https://blogs.msdn.com/b/ieinternals/archive/2010/03/30/combating-clickjacking-with-x-frame-options.aspx), le [blog SDL](https://blogs.msdn.com/b/sdl/archive/2009/02/05/clickjacking-defense-in-ie8.aspx), et [OWASP](https://www.owasp.org/index.php/Clickjacking). Le Runtime de pile Web ASP.NET peut dans certains Vérifiez ultérieurement le MVC et programmes d’assistance de Pages Web anti-XSRF définissent automatiquement cet en-tête afin que les applications sont automatiquement protégées contre cette attaque.

Les développeurs Web doivent continuer à vous assurer que leur site n’est pas vulnérable aux attaques XSS. Les attaques XSS sont très puissantes et une exploitation réussie ne fonctionneraient pas également les défenses de Runtime de pile Web ASP.NET contre les attaques XSRF.

## <a name="acknowledgment"></a>Accusé de réception

[@LeviBroderick](https://twitter.com/LeviBroderick), qui a une grande partie du code de sécurité ASP.NET écrit la majeure partie de ces informations.
