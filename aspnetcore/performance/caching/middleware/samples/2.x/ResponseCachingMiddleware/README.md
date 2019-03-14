---
ms.openlocfilehash: 93bda587eebc438e5da36b07cb7e4a37df8a91eb
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57062717"
---
# <a name="aspnet-core-response-caching-sample"></a>Beispiel: ASP.NET Core-Antwort zwischenspeichern

Dieses Beispiel veranschaulicht die Verwendung von ASP.NET Core [Antworten Zwischenspeichern Middleware](https://docs.microsoft.com/aspnet/core/performance/caching/middleware).

Die app reagiert, mit der Indexseite, einschließlich einer `Cache-Control` -Header auf das Verhalten beim Zwischenspeichern zu konfigurieren. Legt auch fest, die app die `Vary` Header zum Konfigurieren des Caches zum Verarbeiten der Antwort nur, wenn die `Accept-Encoding` Header nachfolgender Anforderungen entspricht, die aus der ursprünglichen Anforderung.

Beim Ausführen des Beispiels wird die Seite "Index" bereitgestellt, aus dem Cache gespeichert und bis zu 10 Sekunden lang zwischengespeichert.
