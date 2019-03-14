---
ms.openlocfilehash: 71a40fcab8f1d1005cbe9327d01a42484765a421
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57059637"
---
# <a name="custom-model-binding-demo"></a>Demo zur benutzerdefinierten Modellbindung

Testen Sie `ByteArrayModelBinder`, indem Sie die App ausführen und eine Base64-codierte Zeichenfolge an den `ImageController`-Endpunkt (`/api/image/`) übergeben. Geben Sie die Datei- und Dateinameneigenschaften im Anforderungstext als Formulardaten an. Verwenden Sie dafür z.B. [Postman](https://www.getpostman.com/) oder ein ähnliches Tool. Sie können diese [Beispielzeichenfolge](Base64String.txt) verwenden. Das Ergebnis wird dann mit dem angegebenen Dateinamen im Ordner *wwwroot/images/upload* gespeichert.

Probieren Sie folgende Endpunkte aus, um das Beispiel für die benutzerdefinierte Bindung zu testen:

* /api/authors/1
* /api/authors/2 (NICHT GEFUNDEN)
* /api/boundauthors/1
* /api/boundauthors/2 (NICHT GEFUNDEN)
* /api/boundauthors/get/1
* /api/boundauthors/get/2 (KEIN INHALT): Diese Aktion überprüft nicht auf NULL-Werte und gibt *404 Not Found* (404 – Nicht gefunden) zurück.
