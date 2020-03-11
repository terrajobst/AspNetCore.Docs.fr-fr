# <a name="gdpr-sample"></a>Exemple RGPD

* Dans *appSettings. JSON*, définissez `CheckNotConsentNeeded` sur `false` pour demander le consentement ; Sinon, a la valeur true ou omettez. Testez l’application avec `CheckNotConsentNeeded` défini sur `false` et défini sur `true`.
* Créez des cookies essentiels et non essentiels avec chaque variation de `CheckConsentNeeded` et le consentement accordés.
* Inscrire un utilisateur.
* Supprimer les cookies.
