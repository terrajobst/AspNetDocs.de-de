---
uid: mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-cs
title: Verwenden von ASP.NET MVC mit verschiedenen Versionen von IISC#() | Microsoft-Dokumentation
author: microsoft
description: In diesem Tutorial erfahren Sie, wie Sie ASP.NET MVC und das URL-Routing mit unterschiedlichen Versionen von Internetinformationsdienste verwenden. Sie lernen verschiedene Strategien kennen...
ms.author: riande
ms.date: 08/19/2008
ms.assetid: b0cf4a34-2c1d-4717-bb54-ff029e722990
msc.legacyurl: /mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-cs
msc.type: authoredcontent
ms.openlocfilehash: 3b0a9509c0600f3598fd1218a7b383430548d4c0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78486525"
---
# <a name="using-aspnet-mvc-with-different-versions-of-iis-c"></a>Verwendung von ASP.NET MVC mit verschiedenen IIS-Versionen (C#)

von [Microsoft](https://github.com/microsoft)

> In diesem Tutorial erfahren Sie, wie Sie ASP.NET MVC und das URL-Routing mit unterschiedlichen Versionen von Internetinformationsdienste verwenden. Sie lernen verschiedene Strategien für die Verwendung von ASP.NET MVC mit IIS 7,0 (klassischer Modus), IIS 6,0 und früheren Versionen von IIS kennen.

Das ASP.NET-MVC-Framework ist von ASP.NET Routing abhängig, um Browser Anforderungen an Controller Aktionen weiterzuleiten. Um das ASP.NET-Routing zu nutzen, müssen Sie möglicherweise zusätzliche Konfigurationsschritte auf dem Webserver ausführen. Alles hängt von der Version von Internetinformationsdienste (IIS) und dem Anforderungs Verarbeitungsmodus für Ihre Anwendung ab.

Im folgenden finden Sie eine Zusammenfassung der verschiedenen Versionen von IIS:

- IIS 7,0 (integrierter Modus): für die Verwendung des ASP.NET-Routings ist keine spezielle Konfiguration erforderlich.
- IIS 7,0 (klassischer Modus): Sie müssen eine spezielle Konfiguration ausführen, um das ASP.NET-Routing zu verwenden.
- IIS 6,0 oder niedriger: Sie müssen für die Verwendung des ASP.NET-Routings eine spezielle Konfiguration durchführen.

Die neueste Version von IIS ist Version 7,5 (auf Win7). IIS 7 von IIS ist in Windows Server 2008 und Vista/SP1 und höher enthalten. Sie können IIS 7,0 auch unter allen Versionen des Vista-Betriebssystems installieren, ausgenommen Home Basic (siehe [https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx](https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx)).

IIS 7,0 unterstützt zwei Modi für die Verarbeitung von Anforderungen. Sie können den integrierten Modus oder den klassischen Modus verwenden. Sie müssen keine speziellen Konfigurationsschritte ausführen, wenn Sie IIS 7,0 im integrierten Modus verwenden. Sie müssen jedoch zusätzliche Konfigurationsschritte durchführen, wenn Sie IIS 7,0 im klassischen Modus verwenden.

Microsoft Windows Server 2003 enthält IIS 6,0. Bei Verwendung des Betriebssystems Windows Server 2003 kann IIS 6,0 nicht auf IIS 7,0 aktualisiert werden. Bei Verwendung von IIS 6,0 müssen Sie zusätzliche Konfigurationsschritte ausführen.

Microsoft Windows XP Professional umfasst IIS 5,1. Bei Verwendung von IIS 5,1 müssen Sie zusätzliche Konfigurationsschritte ausführen.

Schließlich enthalten Microsoft Windows 2000 und Microsoft Windows 2000 Professional IIS 5,0. Bei Verwendung von IIS 5,0 müssen Sie zusätzliche Konfigurationsschritte ausführen.

## <a name="integrated-versus-classic-mode"></a>Integrierter und klassischer Modus

IIS 7,0 kann Anforderungen mit zwei verschiedenen Anforderungs Verarbeitungsmodi verarbeiten: "integriert" und "klassisch". Der integrierte Modus bietet eine bessere Leistung und mehr Funktionen. Der klassische Modus ist aus Gründen der Abwärtskompatibilität mit früheren Versionen von IIS enthalten.

Der Anforderungs Verarbeitungsmodus wird vom Anwendungs Pool bestimmt. Sie können ermitteln, welcher Verarbeitungsmodus von einer bestimmten Webanwendung verwendet wird, indem Sie den Anwendungs Pool ermitteln, der der Anwendung zugeordnet ist. Folgen Sie diesen Schritten:

1. Starten des Internetinformationsdienste-Managers
2. Wählen Sie im Fenster Verbindungen eine Anwendung aus.
3. Klicken Sie im Aktions Fenster auf den Link " **Grundeinstellungen** ", um das Dialogfeld "Anwendung bearbeiten" zu öffnen (siehe Abbildung 1).
4. Notieren Sie sich den ausgewählten Anwendungs Pool.

IIS ist standardmäßig so konfiguriert, dass zwei Anwendungs Pools unterstützt werden: " **DefaultAppPool** " und " **Classic .NET AppPool**". Wenn DefaultAppPool ausgewählt ist, wird die Anwendung im integrierten Anforderungs Verarbeitungsmodus ausgeführt. Wenn klassisches .NET-AppPool ausgewählt ist, wird die Anwendung im klassischen Anforderungs Verarbeitungsmodus ausgeführt.

[![des Dialog Felds "Neues Projekt"](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image1.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image1.png)

**Abbildung 1**: Erkennen des Anforderungs Verarbeitungsmodus ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image2.png))

Beachten Sie, dass Sie den Anforderungs Verarbeitungsmodus im Dialogfeld Anwendung bearbeiten ändern können. Klicken Sie auf die Schaltfläche auswählen, und ändern Sie den der Anwendung zugeordneten Anwendungs Pool. Beachten Sie, dass es Kompatibilitätsprobleme gibt, wenn Sie eine ASP.NET-Anwendung vom klassischen in den integrierten Modus ändern. Weitere Informationen finden Sie in den folgenden Artikeln:

- Upgrade von ASP.NET 1,1 auf IIS 7,0 unter Windows Vista und Windows Server 2008-- [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008)
- ASP.NET-Integration mit IIS 7,0- [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)

Wenn eine ASP.NET-Anwendung den DefaultAppPool verwendet, müssen Sie keine weiteren Schritte ausführen, um das ASP.NET-Routing (und somit ASP.NET MVC) zu bearbeiten. Wenn jedoch die ASP.NET-Anwendung so konfiguriert ist, dass Sie den klassischen .NET-AppPool verwendet, müssen Sie weitere Aufgaben erledigen.

## <a name="using-aspnet-mvc-with-older-versions-of-iis"></a>Verwenden von ASP.NET MVC mit älteren Versionen von IIS

Wenn Sie ASP.NET MVC mit einer älteren Version von IIS als IIS 7,0 verwenden müssen, oder Sie IIS 7,0 im klassischen Modus verwenden müssen, haben Sie zwei Möglichkeiten. Zuerst können Sie die Routing Tabelle so ändern, dass Dateierweiterungen verwendet werden. Anstatt z. b. eine URL wie/Store/Details anzufordern, würden Sie eine URL wie/Store.aspx/Details. anfordern.

Die zweite Option besteht darin, eine "Platzhalter *Skript*Zuordnung" zu erstellen. Eine Platzhalter Skript Zuordnung ermöglicht es Ihnen, jede Anforderung dem ASP.NET-Framework zuzuordnen.

Wenn Sie keinen Zugriff auf Ihren Webserver haben (z. b. Wenn Ihre ASP.NET MVC-Anwendung von einem Internet Dienstanbieter gehostet wird), müssen Sie die erste Option verwenden. Wenn Sie die Darstellung Ihrer URLs nicht ändern möchten, und Sie Zugriff auf den Webserver haben, können Sie die zweite Option verwenden.

In den folgenden Abschnitten werden die einzelnen Optionen ausführlich erläutert.

## <a name="adding-extensions-to-the-route-table"></a>Hinzufügen von Erweiterungen zur Routing Tabelle

Die einfachste Möglichkeit, um mit älteren Versionen von IIS ASP.NET Routing zu erzielen, ist das Ändern der Routing Tabelle in der Datei "Global. asax". Die standardmäßige und unveränderte Global. asax-Datei in der Liste 1 konfiguriert eine Route mit dem Namen "Standardroute".

**Codebeispiel 1-Global. asax (nicht geändert)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample1.cs)]

Die in der Liste 1 konfigurierte Standardroute ermöglicht Ihnen das Weiterleiten von URLs, die wie folgt aussehen:

/Home/Index

/Product/Details/3

/Product

Ältere Versionen von IIS übergeben diese Anforderungen leider nicht an das ASP.NET-Framework. Diese Anforderungen werden daher nicht an einen Controller weitergeleitet. Wenn Sie z. b. eine Browser Anforderung für die URL/Home/Index, erhalten Sie die Fehlerseite in Abbildung 2.

[![des Dialog Felds "Neues Projekt"](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image2.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image3.png)

**Abbildung 2**: der Fehler "404 nicht gefunden" wird empfangen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image4.png))

Ältere Versionen von IIS ordnen nur bestimmte Anforderungen dem ASP.NET-Framework zu. Die Anforderung muss für eine URL mit der richtigen Dateierweiterung sein. Beispielsweise wird eine Anforderung für/SomePage.aspx dem ASP.NET-Framework zugeordnet. Eine Anforderung für/SomePage.htm jedoch nicht.

Damit das ASP.NET Routing funktioniert, müssen Sie die Standardroute so ändern, dass Sie eine Dateierweiterung enthält, die dem ASP.NET-Framework zugeordnet ist.

Dies erfolgt mithilfe eines Skripts mit dem Namen `registermvc.wsf`. Sie wurde in der ASP.NET MVC 1-Version in `C:\Program Files\Microsoft ASP.NET\ASP.NET MVC\Scripts`enthalten, aber ab ASP.net wurde dieses Skript in die ASP.net Futures verschoben, die unter [http://aspnet.codeplex.com/releases/view/39978](http://aspnet.codeplex.com/releases/view/39978)verfügbar sind.

Durch das Ausführen dieses Skripts wird eine neue MVC-Erweiterung mit IIS registriert. Nachdem Sie die MVC-Erweiterung registriert haben, können Sie die Routen in der Datei Global. asax ändern, sodass die Routen die MVC-Erweiterung verwenden.

Die geänderte Datei "Global. asax" in der Liste 2 funktioniert mit älteren Versionen von IIS.

**Codebeispiel 2: Global. asax (mit Erweiterungen geändert)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample2.cs)]

**Wichtig**: Denken Sie daran, die ASP.NET MVC-Anwendung nach dem Ändern der Datei "Global. asax" erneut zu erstellen.

Es gibt zwei wichtige Änderungen an der Datei Global. asax in der Liste 2. In "Global. asax" sind jetzt zwei Routen definiert. Das URL-Muster für die Standardroute, die erste Route, sieht nun wie folgt aus:

{Controller}. MVC/{Action}/{ID}

Durch das Hinzufügen der MVC-Erweiterung wird der Typ der Dateien geändert, die vom ASP.NET-Routing Modul abgefangen werden. Mit dieser Änderung leitet die ASP.NET MVC-Anwendung nun Anforderungen wie die folgenden weiter:

/Home.mvc/Index/

/Product.mvc/Details/3

/Product.mvc/

Die zweite Route, die Stamm Route, ist neu. Dieses URL-Muster für die Stamm Route ist eine leere Zeichenfolge. Diese Route ist erforderlich, um übereinstimmende Anforderungen für den Stamm der Anwendung zu erfüllen. Die Stamm Route entspricht beispielsweise einer Anforderung, die wie folgt aussieht:

[http://www.YourApplication.com/](http://www.YourApplication.com/)

Nachdem Sie diese Änderungen an der Routing Tabelle vorgenommen haben, müssen Sie sicherstellen, dass alle Verknüpfungen in der Anwendung mit diesen neuen URL-Mustern kompatibel sind. Stellen Sie also sicher, dass alle Ihre Verknüpfungen die Erweiterung. MVC enthalten. Wenn Sie die Link-Methode HTML. Action Link () verwenden, um Ihre Verknüpfungen zu generieren, sollten Sie keine Änderungen vornehmen.

Anstatt das Skript registermvc. WCF zu verwenden, können Sie IIS eine neue Erweiterung hinzufügen, die dem ASP.NET-Framework per Hand zugeordnet ist. Wenn Sie selbst eine neue Erweiterung hinzufügen, stellen Sie sicher, dass das Kontrollkästchen mit der Bezeichnung Vergewissern Sie sich **, dass die Datei vorhanden** ist

## <a name="hosted-server"></a>Gehosteter Server

Sie haben nicht immer Zugriff auf Ihren Webserver. Wenn Sie z. b. Ihre ASP.NET MVC-Anwendung mit einem Internet-Hosting-Anbieter gehostet haben, haben Sie nicht unbedingt Zugriff auf IIS.

In diesem Fall sollten Sie eine der vorhandenen Dateierweiterungen verwenden, die dem ASP.NET-Framework zugeordnet sind. Beispiele für Dateierweiterungen, die ASP.NET zugeordnet sind, sind die Erweiterungen ". aspx", ". axd" und ". ashx".

Beispielsweise verwendet die geänderte Datei "Global. asax" in "Listing 3" die Erweiterung ". aspx" anstelle der Erweiterung ". MVC".

**Codebeispiel 3: Global. asax (mit aspx-Erweiterungen geändert)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample3.cs)]

Die Datei "Global. asax" in Auflistung 3 ist genau mit der vorherigen Datei "Global. asax" identisch, mit der Ausnahme, dass Sie die Erweiterung ". aspx" anstelle der Erweiterung ". MVC" verwendet. Sie müssen kein Setup auf dem Remoteweb Server ausführen, um die Erweiterung ". aspx" zu verwenden.

## <a name="creating-a-wildcard-script-map"></a>Erstellen einer Platzhalter Skript Zuordnung

Wenn Sie die URLs für Ihre ASP.NET MVC-Anwendung nicht ändern möchten, und Sie Zugriff auf Ihren Webserver haben, haben Sie eine zusätzliche Option. Sie können eine Platzhalter Skript Zuordnung erstellen, die alle Anforderungen an den Webserver dem ASP.NET Framework zuordnet. Auf diese Weise können Sie die standardmäßige ASP.NET-MVC-Routing Tabelle mit IIS 7,0 (im klassischen Modus) oder IIS 6,0 verwenden.

Beachten Sie, dass diese Option dazu veranlasst, dass IIS jede Anforderung abfängt, die an den Webserver gerichtet ist. Dies umfasst Anforderungen für Images, klassische ASP-Seiten und HTML-Seiten. Daher hat das Aktivieren einer Platzhalter Skript Zuordnung zu ASP.net Auswirkungen auf die Leistung.

So aktivieren Sie eine Platzhalter Skript Zuordnung für IIS 7,0:

1. Wählen Sie Ihre Anwendung im Fenster Verbindungen aus.
2. Stellen Sie sicher, dass die Ansicht **Features** ausgewählt ist.
3. Doppelklicken Sie auf die Schaltfläche Handlerzuordnungen.
4. Klicken Sie auf den Link Platzhalter **Skript hinzufügen** (siehe Abbildung 3).
5. Geben Sie den Pfad für die ASPNET-\_Datei "ISAPI. dll" ein (Sie können diesen Pfad aus der PageHandlerFactory-Skript Zuordnung kopieren).
6. Geben Sie den Namen MVC ein.
7. Klicken Sie auf die Schaltfläche **OK**

[![des Dialog Felds "Neues Projekt"](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image3.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image5.png)

**Abbildung 3**: Erstellen einer Platzhalter Skript Zuordnung mit IIS 7,0 ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image6.png))

Führen Sie die folgenden Schritte aus, um eine Platzhalter Skript Zuordnung mit IIS 6,0 zu erstellen:

1. Mit der rechten Maustaste auf eine Website klicken und Eigenschaften auswählen
2. Registerkarte "Basis **Verzeichnis** " auswählen
3. Klicken auf die **Konfigurations** Schaltfläche
4. **Registerkarte** "Zuordnungen" auswählen
5. Klicken Sie auf die Schaltfläche **Einfügen** (siehe Abbildung 4).
6. Fügen Sie den Pfad für die ASPNET-\_"ISAPI. dll" in das Feld "ausführbare Datei" ein (Sie können diesen Pfad aus der Skript Zuordnung für ASPX-Dateien kopieren).
7. Deaktivieren Sie das Kontrollkästchen **überprüfen, ob die Datei vorhanden** ist.
8. Klicken Sie auf die Schaltfläche **OK**

[![des Dialog Felds "Neues Projekt"](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image4.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image7.png)

**Abbildung 4**: Erstellen einer Platzhalter Skript Zuordnung mit IIS 6,0 ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image8.png))

Nachdem Sie Platzhalter Skript Zuordnungen aktiviert haben, müssen Sie die Routing Tabelle in der Datei Global. asax so ändern, dass Sie eine Stamm Route enthält. Andernfalls erhalten Sie die Fehlerseite in Abbildung 5, wenn Sie eine Anforderung an die Stamm Seite der Anwendung senden. Sie können die geänderte Datei "Global. asax" in der Liste 4 verwenden.

[![des Dialog Felds "Neues Projekt"](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image5.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image9.png)

**Abbildung 5**: fehlender Fehler bei der Stamm Route ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image10.png))

**Codebeispiel 4: Global. asax (mit Stamm Route geändert)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample4.cs)]

Nachdem Sie eine Skript Zuordnung mit Platzhaltern für IIS 7,0 oder IIS 6,0 aktiviert haben, können Sie Anforderungen mit der Standardrouten Tabelle, die wie folgt aussehen, vornehmen:

/

/Home/Index

/Product/Details/3

/Product

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial wird erläutert, wie Sie ASP.NET MVC verwenden können, wenn Sie eine ältere Version von IIS (oder IIS 7,0 im klassischen Modus) verwenden. Wir haben zwei Methoden zur Verwendung von ASP.NET-Routing für ältere Versionen von IIS erläutert: Ändern der Standardrouten Tabelle oder Erstellen einer Skript Zuordnung mit Platzhaltern.

Die erste Option erfordert, dass Sie die in Ihrer ASP.NET MVC-Anwendung verwendeten URLs ändern. Ein sehr wichtiger Vorteil dieser ersten Option besteht darin, dass Sie keinen Zugriff auf einen Webserver benötigen, um die Routing Tabelle zu ändern. Dies bedeutet, dass Sie diese erste Option auch dann verwenden können, wenn Sie Ihre ASP.NET MVC-Anwendung mit einem Internet Hostingunternehmen gehostet haben.

Die zweite Option ist das Erstellen einer Skript Zuordnung mit Platzhaltern. Der Vorteil dieser zweiten Option besteht darin, dass Sie Ihre URLs nicht ändern müssen. Der Nachteil dieser zweiten Option besteht darin, dass Sie sich auf die Leistung Ihrer ASP.NET MVC-Anwendung auswirken kann.

> [!div class="step-by-step"]
> [Weiter](using-asp-net-mvc-with-different-versions-of-iis-vb.md)
