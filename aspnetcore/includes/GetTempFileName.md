---
ms.openlocfilehash: 2cd201e16cd491d0f468e8ef141b522f1694a257
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57165869"
---
<span data-ttu-id="2068b-101">**Warnung**: Der folgende code verwendet `GetTempFileName`, welche löst eine `IOException` Wenn mehr als 65535 Dateien erstellt werden, ohne vorherige temporäre Dateien zu löschen.</span><span class="sxs-lookup"><span data-stu-id="2068b-101">**Warning**: The following code uses `GetTempFileName`, which throws an `IOException` if more than 65535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="2068b-102">In einer echten Anwendung müssen temporäre Dateien gelöscht werden, oder Sie müssen für die Erstellung von Namen temporärer Dateien `GetTempPath` und `GetRandomFileName` verwenden.</span><span class="sxs-lookup"><span data-stu-id="2068b-102">A real app should either delete temporary files or use `GetTempPath` and `GetRandomFileName` to create temporary file names.</span></span> <span data-ttu-id="2068b-103">Die Obergrenze von 65535 Dateien gilt pro Server. Das heißt, sie kann schon durch eine andere App auf dem Server erreicht sein.</span><span class="sxs-lookup"><span data-stu-id="2068b-103">The 65535 files limit is per server, so another app on the server can use up all 65535 files.</span></span> 
