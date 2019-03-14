---
ms.openlocfilehash: 476580d4bc2435f73ef37c5344ab7fea4b67b9b8
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57032497"
---
Vertrauen Sie dem HTTPS-Entwicklungszertifikat, indem Sie den folgenden Befehl ausführen:

```console
dotnet dev-certs https --trust
```

Über den vorherigen Befehl wird der folgende Dialog angezeigt:

![Dialogfeld „Sicherheitswarnung“](~/getting-started/_static/cert.png)

Klicken Sie auf **Ja**, wenn Sie zustimmen möchten, dass das Entwicklungszertifikat vertrauenswürdig ist.

Weitere Informationen finden Sie unter [Trust the ASP.NET Core HTTPS development certificate (Festlegen des ASP.NET Core-HTTPS-Entwicklungszertifikats als vertrauenswürdig)](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).