---
ms.openlocfilehash: d7ef9b11af8ee11e4ec84404f8cdeb0b89384a3c
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57130449"
---
## <a name="forward-request-information-with-a-proxy-or-load-balancer"></a><span data-ttu-id="dc196-101">Vorwärts Anfordern von Informationen mit einem Proxy oder load balancer</span><span class="sxs-lookup"><span data-stu-id="dc196-101">Forward request information with a proxy or load balancer</span></span>

<span data-ttu-id="dc196-102">Wenn die app hinter einem Proxyserver oder Lastenausgleich bereitgestellt wird, können einige der ursprünglichen Anforderungsinformationen zur app im Anforderungsheader weitergeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="dc196-102">If the app is deployed behind a proxy server or load balancer, some of the original request information might be forwarded to the app in request headers.</span></span> <span data-ttu-id="dc196-103">Diese Informationen umfassen in der Regel das sichere Anforderungsschema (`https`), Host und Client-IP-Adresse.</span><span class="sxs-lookup"><span data-stu-id="dc196-103">This information usually includes the secure request scheme (`https`), host, and client IP address.</span></span> <span data-ttu-id="dc196-104">Apps nicht automatisch diese Anforderungsheader, um zu ermitteln und verwenden Sie die Anforderungsinformationen der ursprünglichen gelesen.</span><span class="sxs-lookup"><span data-stu-id="dc196-104">Apps don't automatically read these request headers to discover and use the original request information.</span></span>

<span data-ttu-id="dc196-105">Das Schema wird in linkgenerierung verwendet, die Ablauf der Authentifizierung mit externen Anbietern betroffen sind.</span><span class="sxs-lookup"><span data-stu-id="dc196-105">The scheme is used in link generation that affects the authentication flow with external providers.</span></span> <span data-ttu-id="dc196-106">Das sichere Schema verlieren (`https`) führt in der app, die falsche unsicherer umleitungs-URLs generieren.</span><span class="sxs-lookup"><span data-stu-id="dc196-106">Losing the secure scheme (`https`) results in the app generating incorrect insecure redirect URLs.</span></span>

<span data-ttu-id="dc196-107">Verwenden Sie Forwardedheadersmiddleware, um Informationen zu den ursprünglichen an die app für die anforderungsverarbeitung verfügbar zu machen.</span><span class="sxs-lookup"><span data-stu-id="dc196-107">Use Forwarded Headers Middleware to make the original request information available to the app for request processing.</span></span>

<span data-ttu-id="dc196-108">Weitere Informationen finden Sie unter <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="dc196-108">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>
