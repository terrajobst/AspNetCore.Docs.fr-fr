* Approuvez le certificat de développement HTTPS en exécutant la commande suivante :

    ```console
    dotnet dev-certs https --trust
    ```

* La commande précédente affiche la sortie suivante :

    ```console
    Trusting the HTTPS development certificate was requested. If the certificate 
    is not already trusted we will run the following command:
    'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain 
    <<certificate>>'
    This command might prompt you for your password to install the certificate on the 
    system keychain.
    The HTTPS developer certificate was generated successfully.
    ```

* Entrez le nom d’utilisateur administrateur et le mot de passe s’ils vous sont demandés.  Le certificat est maintenant installé et approuvé.

    Pour plus d’informations, consultez [Approuver le certificat de développement HTTPS ASP.NET Core](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).