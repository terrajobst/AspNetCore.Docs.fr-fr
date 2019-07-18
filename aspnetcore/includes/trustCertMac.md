* <span data-ttu-id="f6ea5-101">Approuvez le certificat de développement HTTPS en exécutant la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="f6ea5-101">Trust the HTTPS development certificate by running the following command:</span></span>

    ```console
    dotnet dev-certs https --trust
    ```

* <span data-ttu-id="f6ea5-102">La commande précédente affiche la sortie suivante :</span><span class="sxs-lookup"><span data-stu-id="f6ea5-102">The preceding command displays the following output:</span></span>

    ```console
    Trusting the HTTPS development certificate was requested. If the certificate 
    is not already trusted we will run the following command:
    'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain 
    <<certificate>>'
    This command might prompt you for your password to install the certificate on the 
    system keychain.
    The HTTPS developer certificate was generated successfully.
    ```

* <span data-ttu-id="f6ea5-103">Entrez le nom d’utilisateur administrateur et le mot de passe s’ils vous sont demandés.</span><span class="sxs-lookup"><span data-stu-id="f6ea5-103">Enter the admin username and password if prompted.</span></span>  <span data-ttu-id="f6ea5-104">Le certificat est maintenant installé et approuvé.</span><span class="sxs-lookup"><span data-stu-id="f6ea5-104">The certificate will now be installed and trusted.</span></span>

    <span data-ttu-id="f6ea5-105">Pour plus d’informations, consultez [Approuver le certificat de développement HTTPS ASP.NET Core](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).</span><span class="sxs-lookup"><span data-stu-id="f6ea5-105">See [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos) for more information.</span></span>