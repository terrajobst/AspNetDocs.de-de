---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Veröffentlichen der app in Azure Azure App Service | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: a9a7b74a07c71b47253c0af304c7a26ffa4eaae5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504759"
---
# <a name="publish-the-app-to-azure-azure-app-service"></a>Veröffentlichen der app in Azure Azure App Service

von [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](https://github.com/MikeWasson/BookService)

Im letzten Schritt wird die Anwendung in Azure veröffentlicht. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie **veröffentlichen**aus.

![](part-10/_static/image1.png)

Durch Klicken auf **veröffentlichen** wird das Dialogfeld **Web veröffentlichen** aufgerufen. Wenn Sie beim ersten Erstellen des Projekts den **Host in der Cloud** aktiviert haben, sind die Verbindung und die Einstellungen bereits konfiguriert. Klicken Sie in diesem Fall einfach auf die Registerkarte **Einstellungen** , und aktivieren Sie &quot;Code First-Migrationen&quot;ausführen. (Wenn Sie den **Host in der Cloud** zu Beginn nicht überprüft haben, führen Sie die Schritte im [nächsten Abschnitt](#new-website)aus.)

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

Klicken Sie zum Bereitstellen der APP auf **veröffentlichen**. Sie können den Veröffentlichungs Fortschritt im Fenster **Webveröffentlichungs Aktivität** anzeigen. (Wählen Sie im Menü **Ansicht** die Option **Weitere Fenster**, und wählen Sie dann **Webveröffentlichungs Aktivität**aus.)

![](part-10/_static/image4.png)

Wenn Visual Studio die Bereitstellung der APP abgeschlossen hat, öffnet der Standardbrowser automatisch die URL der bereitgestellten Website, und die von Ihnen erstellte Anwendung wird nun in der Cloud ausgeführt. Die URL in der Adressleiste des Browsers zeigt, dass die Website aus dem Internet geladen wird.

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a>Bereitstellen auf einer neuen Website

Wenn Sie den **Host in der Cloud** beim ersten Erstellen des Projekts nicht überprüft haben, können Sie jetzt eine neue Web-App konfigurieren. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie **veröffentlichen**aus. Wählen Sie die Registerkarte **Profil** , und klicken Sie auf **Microsoft Azure Websites** Wenn Sie zurzeit nicht bei Azure angemeldet sind, werden Sie aufgefordert, sich anzumelden.

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

Klicken Sie im Dialogfeld **vorhandene Websites** auf **neu**.

![](part-10/_static/image9.png)

Geben Sie einen Website Namen ein. Wählen Sie Ihr Azure-Abonnement und die Region aus. Wählen Sie unter **Datenbankserver**die Option **neuen Server erstellen**aus, oder wählen Sie einen vorhandenen Server aus. Klicken Sie auf **Erstellen**.

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

Klicken Sie auf die Registerkarte **Einstellungen** , und aktivieren Sie &quot;Code First-Migrationen&quot;ausführen. Dann wird auf **Veröffentlichen** geklickt.

> [!div class="step-by-step"]
> [Previous](part-9.md)
