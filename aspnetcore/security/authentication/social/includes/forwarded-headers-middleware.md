---
ms.openlocfilehash: d7ef9b11af8ee11e4ec84404f8cdeb0b89384a3c
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57130449"
---
## <a name="forward-request-information-with-a-proxy-or-load-balancer"></a>Vorwärts Anfordern von Informationen mit einem Proxy oder load balancer

Wenn die app hinter einem Proxyserver oder Lastenausgleich bereitgestellt wird, können einige der ursprünglichen Anforderungsinformationen zur app im Anforderungsheader weitergeleitet werden. Diese Informationen umfassen in der Regel das sichere Anforderungsschema (`https`), Host und Client-IP-Adresse. Apps nicht automatisch diese Anforderungsheader, um zu ermitteln und verwenden Sie die Anforderungsinformationen der ursprünglichen gelesen.

Das Schema wird in linkgenerierung verwendet, die Ablauf der Authentifizierung mit externen Anbietern betroffen sind. Das sichere Schema verlieren (`https`) führt in der app, die falsche unsicherer umleitungs-URLs generieren.

Verwenden Sie Forwardedheadersmiddleware, um Informationen zu den ursprünglichen an die app für die anforderungsverarbeitung verfügbar zu machen.

Weitere Informationen finden Sie unter <xref:host-and-deploy/proxy-load-balancer>.
