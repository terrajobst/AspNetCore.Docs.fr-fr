* `-F|--no-formatting`

  <span data-ttu-id="8ca32-101">Indicateur dont la présence provoque la suppression de la mise en forme de la réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="8ca32-101">A flag whose presence suppresses HTTP response formatting.</span></span>

* `-h|--header`

  <span data-ttu-id="8ca32-102">Définit un en-tête de requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="8ca32-102">Sets an HTTP request header.</span></span> <span data-ttu-id="8ca32-103">Les deux formats de fichier suivants sont pris en charge :</span><span class="sxs-lookup"><span data-stu-id="8ca32-103">The following two value formats are supported:</span></span>

  * `{header}={value}`
  * `{header}:{value}`

* `--response`

  <span data-ttu-id="8ca32-104">Spécifie un fichier où la totalité de la réponse HTTP (notamment les en-têtes et le corps) doit être écrite.</span><span class="sxs-lookup"><span data-stu-id="8ca32-104">Specifies a file to which the entire HTTP response (including headers and body) should be written.</span></span> <span data-ttu-id="8ca32-105">Par exemple, `--response "C:\response.txt"`.</span><span class="sxs-lookup"><span data-stu-id="8ca32-105">For example, `--response "C:\response.txt"`.</span></span> <span data-ttu-id="8ca32-106">Le fichier est créé s’il n’existe pas.</span><span class="sxs-lookup"><span data-stu-id="8ca32-106">The file is created if it doesn't exist.</span></span>

* `--response:body`

  <span data-ttu-id="8ca32-107">Spécifie un fichier dans lequel le corps de la réponse HTTP doit être écrit.</span><span class="sxs-lookup"><span data-stu-id="8ca32-107">Specifies a file to which the HTTP response body should be written.</span></span> <span data-ttu-id="8ca32-108">Par exemple, `--response:body "C:\response.json"`.</span><span class="sxs-lookup"><span data-stu-id="8ca32-108">For example, `--response:body "C:\response.json"`.</span></span> <span data-ttu-id="8ca32-109">Le fichier est créé s’il n’existe pas.</span><span class="sxs-lookup"><span data-stu-id="8ca32-109">The file is created if it doesn't exist.</span></span>

* `--response:headers`

  <span data-ttu-id="8ca32-110">Spécifie un fichier dans lequel les en-têtes de la réponse HTTP doivent être écrits.</span><span class="sxs-lookup"><span data-stu-id="8ca32-110">Specifies a file to which the HTTP response headers should be written.</span></span> <span data-ttu-id="8ca32-111">Par exemple, `--response:headers "C:\response.txt"`.</span><span class="sxs-lookup"><span data-stu-id="8ca32-111">For example, `--response:headers "C:\response.txt"`.</span></span> <span data-ttu-id="8ca32-112">Le fichier est créé s’il n’existe pas.</span><span class="sxs-lookup"><span data-stu-id="8ca32-112">The file is created if it doesn't exist.</span></span>

* `-s|--streaming`

  <span data-ttu-id="8ca32-113">Indicateur dont la présence provoque l’activation du streaming de la réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="8ca32-113">A flag whose presence enables streaming of the HTTP response.</span></span>
