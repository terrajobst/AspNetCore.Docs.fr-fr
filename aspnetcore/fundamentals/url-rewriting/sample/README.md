# <a name="aspnet-core-url-rewriting-sample-aspnet-core-2x"></a>Exemple de réécriture d’URL ASP.NET Core (ASP.NET Core 2.x)

Cet exemple illustre l’utilisation de l’intergiciel (middleware) de réécriture d’URL ASP.NET Core 2.x. L’application montre les options de réécriture et de redirection d’URL. Pour obtenir l’exemple correspondant à ASP.NET Core 1.x, consultez [Exemple de réécriture d’URL ASP.NET Core (ASP.NET Core 1.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/1.x).

Quand vous exécutez l’exemple, une réponse est renvoyée, qui indique l’URL réécrite ou redirigée après l’application d’une des règles à une URL de requête.

## <a name="examples-in-this-sample"></a>Extraits de cet exemple

* `AddRedirect("redirect-rule/(.*)", "redirected/$1")`
  - Code d’état de réussite : 302 (Trouvé)
  - Exemple (redirection) : **/redirect-rule/{capture_group}** en **/redirected/{capture_group}**
* `AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", skipRemainingRules: true)`
  - Code d’état de réussite : 200 (OK)
  - Exemple (réécriture) : **/rewrite-rule/{capture_group_1}/{capture_group_2}** en **/rewritten?var1={capture_group_1}&var2={capture_group_2}**
* `AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")`
  - Code d’état de réussite : 302 (Trouvé)
  - Exemple (redirection) : **/apache-mod-rules-redirect/{capture_group}** en **/redirected?id={capture_group}**
* `AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")`
  - Code d’état de réussite : 200 (OK)
  - Exemple (réécriture) : **/iis-rules-rewrite/{capture_group}** en **/rewritten?id={capture_group}**
* `Add(RedirectXMLRequests)`
  - Code d’état de réussite : 301 (déplacé de façon permanente)
  - Exemple (redirection) : **/file.xml** en **/xmlfiles/file.xml**
* `Add(new RedirectPNGRequests(".png", "/png-images")))`<br>`Add(new RedirectPNGRequests(".jpg", "/jpg-images")))`
  - Code d’état de réussite : 301 (déplacé de façon permanente)
  - Exemple (redirection) : **/image.png** en **/png-images/image.png**
  - Exemple (redirection) : **/image.jpg** en **/jpg-images/image.jpg**

## <a name="using-a-physicalfileprovider"></a>Utilisation d’un `PhysicalFileProvider`
Vous pouvez également obtenir un `IFileProvider` en créant un `PhysicalFileProvider` qui est ensuite passé aux méthodes `AddApacheModRewrite()` et `AddIISUrlRewrite()` :
```csharp
using Microsoft.Extensions.FileProviders;
PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
```
## <a name="secure-redirection-extensions"></a>Extensions de redirection sécurisées
Pour faciliter votre exploration de ces méthodes de redirection, cet exemple inclut la configuration de `WebHostBuilder` pour que l’application utilise des URL (**https://localhost:5001**, **https://localhost**) et un certificat de test (**testCert.pfx**). Ajoutez les méthodes souhaitées au `RewriteOptions()` dans **Startup.cs** pour étudier leur comportement.


|              Méthode              | Code d’état |    Port    |
|----------------------------------|:-----------:|:----------:|
| `.AddRedirectToHttpsPermanent()` |     301     | Null (465) |
|     `.AddRedirectToHttps()`      |     302     | Null (465) |
|    `.AddRedirectToHttps(301)`    |     301     | Null (465) |
| `.AddRedirectToHttps(301, 5001)` |     301     |    5001    |

