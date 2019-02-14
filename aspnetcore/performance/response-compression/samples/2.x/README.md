# <a name="response-compression-sample-application-aspnet-core-2x"></a>Réponse compression exemple d’application (ASP.NET Core 2.x)

Cet exemple illustre l’utilisation d’ASP.NET Core 2.x intergiciel de Compression des réponses pour compresser les réponses HTTP. L’exemple montre comment Gzip, Brotli et fournisseurs de compression personnalisé pour les réponses de type text et image et montre comment ajouter un type MIME pour la compression. Pour obtenir l’exemple ASP.NET Core 1.x, consultez [réponse compression exemple d’application (ASP.NET Core 1.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples/1.x).

## <a name="examples-in-this-sample"></a>Extraits de cet exemple

* `BrotliCompressionProvider`
  * `text/plain`
    * **/** -Lorem Ipsum fichier réponse à octets 2,044 qui compresse à ~ 979 octets.
    * **/testfile1kb.txt** -réponse de fichier texte à octets 1,033 qui compresse à environ 36 octets.
    * **/ segmentée** -réponse émis en tant que caractères uniques à intervalles de 1 seconde.
* `GzipCompressionProvider`
  * `text/plain`
    * **/** -Lorem Ipsum fichier réponse à octets 2,044 qui compresse à ~ 927 octets.
    * **/testfile1kb.txt** -réponse de fichier texte à octets 1,033 qui compresse à environ 47 octets.
    * **/ segmentée** -réponse émis en tant que caractères uniques à intervalles de 1 seconde.
  * `image/svg+xml`
    * **/Banner.SVG** -un graphique SVG (Scalable Vector) image en réponse à des octets 9,707 qui compresse à ~ 4,459 octets.
* `CustomCompressionProvider`<br>Montre comment implémenter un fournisseur de compression personnalisé pour une utilisation avec l’intergiciel (middleware).

Lorsque la demande inclut le `Accept-Encoding` la compression d’en-tête et de réponse est réussie, l’intergiciel (middleware) ajoute automatiquement un `Vary: Accept-Encoding` en-tête à la réponse. Le `Vary` caches à maintenir plusieurs copies de la réponse en fonction des valeurs alternatives de demande à l’en-tête `Accept-Encoding`, afin que les deux un compressé (Gzip ou Brotli) et de la version non compressée sont stockés dans les caches pour les systèmes qui peuvent accepter le compressé ou la réponse non compressée.

## <a name="use-the-sample"></a>Utilisez l’exemple

1. Effectuer une demande à l’aide [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), ou [Postman](https://www.getpostman.com/) à l’application sans un `Accept-Encoding` en-tête et notez la charge utile de réponse, la taille de la réponse, et en-têtes de réponse.
1. Ajouter un `Accept-Encoding: br` ou `Accept-Encoding: gzip` en-tête et notez la taille de la réponse compressée et les en-têtes de réponse. Supprime de la taille de la réponse et le `Content-Encoding` en-tête de réponse est inclus par l’intergiciel (middleware) qui indique que la compression avec soit Gzip ou Brotli s’est produite. Lorsque vous examinez le corps de réponse pour le Lorem Ipsum ou **testfile1kb.txt** réponse, vous voyez que le texte est compressé et illisible.
1. Ajouter un `Accept-Encoding: mycustomcompression` en-tête et notez les en-têtes de réponse. Le `CustomCompressionProvider` est une implémentation vide qui ne compresser réellement la réponse, mais vous pouvez créer un wrapper de flux de compression personnalisé pour le `CreateStream()` (méthode).
