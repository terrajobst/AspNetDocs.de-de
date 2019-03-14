---
ms.openlocfilehash: 71a40fcab8f1d1005cbe9327d01a42484765a421
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57059637"
---
# <a name="custom-model-binding-demo"></a><span data-ttu-id="67243-101">Demo zur benutzerdefinierten Modellbindung</span><span class="sxs-lookup"><span data-stu-id="67243-101">Custom Model Binding Demo</span></span>

<span data-ttu-id="67243-102">Testen Sie `ByteArrayModelBinder`, indem Sie die App ausführen und eine Base64-codierte Zeichenfolge an den `ImageController`-Endpunkt (`/api/image/`) übergeben.</span><span class="sxs-lookup"><span data-stu-id="67243-102">Test `ByteArrayModelBinder` by running the app and POSTing a base64-encoded string to the `ImageController` endpoint (`/api/image/`).</span></span> <span data-ttu-id="67243-103">Geben Sie die Datei- und Dateinameneigenschaften im Anforderungstext als Formulardaten an. Verwenden Sie dafür z.B. [Postman](https://www.getpostman.com/) oder ein ähnliches Tool.</span><span class="sxs-lookup"><span data-stu-id="67243-103">Specify the file and filename properties in the request body as form-data (using [Postman](https://www.getpostman.com/) or a similar tool).</span></span> <span data-ttu-id="67243-104">Sie können diese [Beispielzeichenfolge](Base64String.txt) verwenden.</span><span class="sxs-lookup"><span data-stu-id="67243-104">You can use [this sample string](Base64String.txt).</span></span> <span data-ttu-id="67243-105">Das Ergebnis wird dann mit dem angegebenen Dateinamen im Ordner *wwwroot/images/upload* gespeichert.</span><span class="sxs-lookup"><span data-stu-id="67243-105">The result is saved in the *wwwroot/images/upload* folder with the filename specified.</span></span>

<span data-ttu-id="67243-106">Probieren Sie folgende Endpunkte aus, um das Beispiel für die benutzerdefinierte Bindung zu testen:</span><span class="sxs-lookup"><span data-stu-id="67243-106">To test the custom binding example, try the following endpoints:</span></span>

* <span data-ttu-id="67243-107">/api/authors/1</span><span class="sxs-lookup"><span data-stu-id="67243-107">/api/authors/1</span></span>
* <span data-ttu-id="67243-108">/api/authors/2 (NICHT GEFUNDEN)</span><span class="sxs-lookup"><span data-stu-id="67243-108">/api/authors/2 (NOT FOUND)</span></span>
* <span data-ttu-id="67243-109">/api/boundauthors/1</span><span class="sxs-lookup"><span data-stu-id="67243-109">/api/boundauthors/1</span></span>
* <span data-ttu-id="67243-110">/api/boundauthors/2 (NICHT GEFUNDEN)</span><span class="sxs-lookup"><span data-stu-id="67243-110">/api/boundauthors/2 (NOT FOUND)</span></span>
* <span data-ttu-id="67243-111">/api/boundauthors/get/1</span><span class="sxs-lookup"><span data-stu-id="67243-111">/api/boundauthors/get/1</span></span>
* <span data-ttu-id="67243-112">/api/boundauthors/get/2 (KEIN INHALT): Diese Aktion überprüft nicht auf NULL-Werte und gibt *404 Not Found* (404 – Nicht gefunden) zurück.</span><span class="sxs-lookup"><span data-stu-id="67243-112">/api/boundauthors/get/2 (NO CONTENT) &ndash; This action doesn't check for null and returns a *404 Not Found*.</span></span>
