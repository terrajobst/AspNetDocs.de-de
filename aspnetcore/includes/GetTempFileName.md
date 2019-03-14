---
ms.openlocfilehash: 2cd201e16cd491d0f468e8ef141b522f1694a257
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57165869"
---
**Warnung**: Der folgende code verwendet `GetTempFileName`, welche löst eine `IOException` Wenn mehr als 65535 Dateien erstellt werden, ohne vorherige temporäre Dateien zu löschen. In einer echten Anwendung müssen temporäre Dateien gelöscht werden, oder Sie müssen für die Erstellung von Namen temporärer Dateien `GetTempPath` und `GetRandomFileName` verwenden. Die Obergrenze von 65535 Dateien gilt pro Server. Das heißt, sie kann schon durch eine andere App auf dem Server erreicht sein. 
