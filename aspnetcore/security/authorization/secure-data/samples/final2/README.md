---
ms.openlocfilehash: e40d28e9a7ca12efe45988fabef23dece893d428
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57049107"
---
# <a name="how-to-buildrun-secure-user-data-sample"></a>Wie Sie Build/Datenbeispiel sichere Benutzer ausführen.

* Legen Sie das Kennwort, mit dem Geheimnis-Manager-Tool:

  `dotnet user-secrets set SeedUserPW <pw>`

* Die Datenbank zu aktualisieren:

    `dotnet ef database update`

* Aktivieren von HTTPS im Projekt
