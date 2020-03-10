---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Verwenden der Web-API 2 mit Entity Framework 6 | Microsoft-Dokumentation
author: MikeWasson
description: Dieses Tutorial vermittelt Ihnen die Grundlagen der Erstellung einer Webanwendung mit einem ASP.net-Web-API-Back-End. Das Tutorial verwendet Entity Framework 6 für die Daten...
ms.author: riande
ms.date: 01/17/2019
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 0f5dc960f494af5bd4ce87863a510d1892319908
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504837"
---
# <a name="using-web-api-2-with-entity-framework-6"></a>Verwendung von Web-API 2 mit Entity Framework 6

[Herunterladen des abgeschlossenen Projekts](https://github.com/MikeWasson/BookService)

> Dieses Tutorial vermittelt Ihnen die Grundlagen der Erstellung einer Webanwendung mit einem ASP.net-Web-API-Back-End. Das Tutorial verwendet Entity Framework 6 für die Datenschicht und Knockout. js für die Client seitige JavaScript-Anwendung. Das Tutorial zeigt außerdem, wie Sie die APP für Azure App Service Web-Apps bereitstellen.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
>
> - Web-API 2,1
> - Visual Studio 2017 (Visual Studio 2017 [hier](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)herunterladen)
> - Entity Framework 6
> - .NET 4.7
> - [Knockout. js](http://knockoutjs.com/) 3,1

In diesem Tutorial wird ASP.net-Web-API 2 mit Entity Framework 6 verwendet, um eine Webanwendung zu erstellen, die eine Back-End-Datenbank bearbeitet. Im folgenden finden Sie einen Screenshot der Anwendung, die Sie erstellen.

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

Die APP verwendet einen Single-Page-Anwendungs Entwurf (Single-Page Application, Spa). "Single-Page Application" ist der allgemeine Begriff für eine Webanwendung, die eine einzelne HTML-Seite lädt und dann die Seite dynamisch aktualisiert, anstatt neue Seiten zu laden. Nach dem anfänglichen Laden der Seite kommuniziert die APP mit dem Server über AJAX-Anforderungen. Die AJAX-Anforderungen geben JSON-Daten zurück, die die APP verwendet, um die Benutzeroberfläche zu aktualisieren.

AJAX ist nicht neu, aber heute gibt es JavaScript-Frameworks, die das Erstellen und Verwalten einer großen anspruchsvollen Spa-Anwendung vereinfachen. In diesem Tutorial wird [Knockout. js](http://knockoutjs.com/)verwendet, Sie können jedoch ein beliebiges JavaScript-Client Framework verwenden.

Hier sind die wichtigsten Bausteine für diese APP:

- ASP.NET MVC erstellt die HTML-Seite.
- ASP.net-Web-API behandelt die AJAX-Anforderungen und gibt JSON-Daten zurück.
- Knockout. js-Daten: bindet die HTML-Elemente an die JSON-Daten.
- Entity Framework mit der-Datenbank kommuniziert.

## <a name="see-this-app-running-on-azure"></a>Diese APP wird in Azure ausgeführt.

Möchten Sie sehen, dass die fertige Site als Live-Web-App ausgeführt wird? Sie können eine vollständige Version der APP für Ihr Azure-Konto bereitstellen, indem Sie die folgende Schaltfläche auswählen.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

Sie benötigen ein Azure-Konto, um diese Lösung in Azure bereitzustellen. Wenn Sie noch nicht über ein Konto verfügen, haben Sie die folgenden Optionen:

- [Öffnen Sie ein Azure-Konto kostenlos](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) . Sie erhalten Gutschriften, die Sie verwenden können, um kostenpflichtige Azure-Dienste zu testen. selbst nach ihrer Nutzung können Sie das Konto behalten und kostenlose Azure-Dienste nutzen.
- [Aktivieren von MSDN-Abonnenten Vorteilen](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) : Ihr MSDN-Abonnement bietet Ihnen jeden Monat ein Guthaben, das Sie für kostenpflichtige Azure-Dienste nutzen können.

## <a name="create-the-project"></a>Erstellen eines Projekts

Öffnen Sie Visual Studio. Klicken Sie im Menü **Datei** auf **neu**, und wählen Sie dann **Projekt**aus. (Oder wählen Sie auf der Start Seite die Option **Neues Projekt** aus.)

Wählen Sie im Dialogfeld **Neues Projekt** im linken Bereich **Web** aus, und **ASP.NET-Webanwendung (.NET Framework)** im mittleren Bereich. Benennen Sie das Projekt **BookService** , und wählen Sie **OK**aus.

[![](part-1/_static/image11.png)](part-1/_static/image11.png)

Wählen Sie im Dialogfeld **Neues ASP.net-Projekt** die **Web-API** -Vorlage aus.

[![](part-1/_static/image12.png)](part-1/_static/image12.png)

Klicken Sie auf **OK**, um das Projekt zu erstellen.

## <a name="configure-azure-settings-optional"></a>Konfigurieren von Azure-Einstellungen (optional)

Nachdem Sie das Projekt erstellt haben, können Sie die Bereitstellung für die Azure App Service von Web-Apps jederzeit auswählen. 

1. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie **veröffentlichen**aus.

2. Klicken Sie im angezeigten Fenster auf **Start**. Das Dialogfeld **Veröffentlichungsziel auswählen** wird angezeigt.

   [![](part-1/_static/image14.png)](part-1/_static/image14.png)

3. Klicken Sie auf **Profil erstellen**. Das Dialogfeld **App Service erstellen** wird angezeigt.

   [![](part-1/_static/image15.png)](part-1/_static/image15.png)

   Übernehmen Sie die Standardwerte, oder geben Sie unterschiedliche Werte für Anwendungsname, Ressourcengruppe, Hostingplan, Azure-Abonnement und geografische Region ein. 

4. Wählen Sie **SQL-Datenbank erstellen**aus. Das Dialogfeld **SQL Server konfigurieren** wird angezeigt. 

   [![](part-1/_static/image16.png)](part-1/_static/image16.png)

   Akzeptieren Sie die Standardwerte, oder geben Sie andere Werte ein Geben Sie einen **Administrator Benutzernamen** und ein **Administrator Kennwort** für die neue Datenbank ein. Wählen Sie **OK** aus, wenn Sie fertig sind. Die Seite **App Service erstellen** wird erneut angezeigt.

5. Wählen Sie **Erstellen** aus, um Ihr Profil zu erstellen. In der unteren rechten Ecke wird eine Meldung angezeigt, die anzeigt, dass die Bereitstellung ausgeführt wird. Nach kurzer Zeit wird das Fenster " **veröffentlichen** " erneut angezeigt.

    [![](part-1/_static/image17.png)](part-1/_static/image17.png)
   
    Das Profil, das Sie zum Bereitstellen der App erstellt haben, ist jetzt verfügbar. 

> [!div class="step-by-step"]
> [Weiter](part-2.md)
