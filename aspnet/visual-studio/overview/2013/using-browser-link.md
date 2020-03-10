---
uid: visual-studio/overview/2013/using-browser-link
title: Verwenden von Browser Link in Visual Studio 2013 | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/04/2013
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: 723a38de4569b0bb58817c70aabb84fef8e19591
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78505203"
---
# <a name="using-browser-link-in-visual-studio-2013"></a>Verwenden von Browser Link in Visual Studio 2013

von [Mike Wasson](https://github.com/MikeWasson)

Browser Link ist ein neues Feature in Visual Studio 2013, das einen Kommunikationskanal zwischen der Entwicklungsumgebung und einem oder mehreren Webbrowsern erstellt. Sie können die Browser Verknüpfung verwenden, um Ihre Webanwendung in mehreren Browsern gleichzeitig zu aktualisieren, was für Browser übergreifende Tests nützlich ist.

- [Browser Aktualisierung](#browser-refresh)
- [Anzeigen des Browser Link-Dashboards](#dashboard)
- [Aktivieren von Browser Link für statische HTML-Dateien](#static-html)
- [Browser Verknüpfung wird deaktiviert](#disabling)
- [Wie funktioniert es?](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a>Browser Aktualisierung

Mit der Browser Aktualisierung können Sie mehrere Browser aktualisieren, die mit Visual Studio über den Browser Link verbunden sind.

Um die Browser Aktualisierung zu verwenden, erstellen Sie zuerst eine ASP.NET-Anwendung, indem Sie eine der Projektvorlagen verwenden. Debuggen Sie die Anwendung, indem Sie F5 drücken oder auf der Symbolleiste auf das Pfeilsymbol klicken:

![](using-browser-link/_static/image1.png)

Sie können auch die Dropdown Liste verwenden, um einen bestimmten Browser zum Debuggen auszuwählen.

![](using-browser-link/_static/image2.png)

Wählen Sie zum Debuggen mit mehreren Browsern **Durchsuchen mit**aus. Halten Sie im Dialogfeld " **Durchsuchen** " die STRG-Taste gedrückt, um mehr als einen Browser auszuwählen. Klicken Sie zum Debuggen mit den ausgewählten Browsern auf **Durchsuchen** . Der Browser Link funktioniert auch, wenn Sie einen Browser von außerhalb von Visual Studio starten und zur Anwendungs-URL navigieren.

![](using-browser-link/_static/image3.png)

Die Browser Link-Steuerelemente befinden sich in der Dropdown Liste mit dem kreisförmigen Pfeilsymbol. Das Pfeilsymbol ist die Schaltfläche " **Aktualisieren** ".

![](using-browser-link/_static/image4.png)

Um anzuzeigen, welche Browser verbunden sind, bewegen Sie den Mauszeiger über die Schaltfläche **Aktualisieren** während des Debuggens. Die verbundenen Browser werden in einem QuickInfo-Fenster angezeigt.

![](using-browser-link/_static/image5.png)

Um die verbundenen Browser zu aktualisieren, klicken Sie auf die Schaltfläche **Aktualisieren** , oder drücken Sie STRG + ALT + EINGABETASTE. Der folgende Screenshot zeigt z. b. ein ASP.net-Projekt, das ich mit der MVC 5-Projektvorlage erstellt habe. Sie können sehen, dass die Anwendung in zwei Browsern am oberen Rand ausgeführt wird. Im unteren Bereich ist das Projekt in Visual Studio geöffnet.

![](using-browser-link/_static/image6.png)

In Visual Studio habe ich die &lt;H1-&gt; der Überschrift für die Startseite geändert:

![](using-browser-link/_static/image7.png)

Beim Klicken auf die Schaltfläche " **Aktualisieren** " trat die Änderung in beiden Browserfenstern auf:

![](using-browser-link/_static/image8.png)

**Notizen**

- Um die Browser Verknüpfung zu aktivieren, legen Sie `debug=true` im [&lt;-Kompilierungs&gt;](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) -Element in der Datei "Web. config" des Projekts fest.
- Die Anwendung muss auf "localhost" ausgeführt werden.
- Die Anwendung muss auf .NET 4,0 oder höher ausgerichtet sein.

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a>Anzeigen des Browser Link-Dashboards

Das Browser Link-Dashboard zeigt Informationen zu den Browser Link Verbindungen an. Um das Dashboard anzuzeigen, klicken Sie auf das Dropdown Menü Browser Link (der kleine Pfeil neben der Schaltfläche **Aktualisieren** ). Klicken Sie dann auf **Browser Link Dashboard**.

![](using-browser-link/_static/image9.png)

Das Dashboard listet die verbundenen Browser und die URL auf, auf die die einzelnen Browser navigiert sind.

![](using-browser-link/_static/image10.png)

Im Abschnitt **Voraussetzungen** werden alle Schritte angezeigt, die zum Aktivieren des Browser Links für dieses Projekt erforderlich sind. Der folgende Screenshot zeigt z. b. ein Projekt, in dem "Debug" in der Datei "Web. config" auf "false" festgelegt ist.

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a>Aktivieren von Browser Link für statische HTML-Dateien

Fügen Sie der Datei "Web. config" Folgendes hinzu, um den Browser Link für statische HTML-Dateien zu aktivieren.

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

Entfernen Sie diese Einstellung aus Leistungsgründen, wenn Sie Ihr Projekt veröffentlichen.

<a id="disabling"></a>
## <a name="disabling-browser-link"></a>Browser Verknüpfung wird deaktiviert

Browser Link ist standardmäßig aktiviert. Es gibt mehrere Möglichkeiten, Sie zu deaktivieren:

- Deaktivieren Sie im Dropdown Menü Browser Link die Option **Browser Link aktivieren**. 

    ![](using-browser-link/_static/image12.png)
- Fügen Sie in der Datei "Web. config" im Abschnitt "appSettings" einen Schlüssel mit dem Namen "vs: enablebrowserlink" mit dem Wert "false" hinzu. 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- Legen Sie in der Datei Web. config Debuggen auf false fest. 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a>Wie funktioniert es?

Der Browser Link verwendet [signalr](../../../signalr/index.md) , um einen Kommunikationskanal zwischen Visual Studio und dem Browser zu erstellen. Wenn der Browser Link aktiviert ist, fungiert Visual Studio als signalr-Server, mit dem mehrere Clients (Browser) eine Verbindung herstellen können. Mit dem Browser Link wird auch ein HTTP-Modul bei ASP.net registriert. Dieses Modul fügt besondere &lt;Skripts&gt; Verweise auf jede Seiten Anforderung vom Server ein. Die Skript Verweise können angezeigt werden, indem Sie im Browser "Quelle anzeigen" auswählen.

![](using-browser-link/_static/image13.png)

Die Quelldateien werden nicht geändert. Das HTTP-Modul fügt die Skript Verweise dynamisch ein.

Da es sich bei dem Browser seitigen Code um JavaScript handelt, funktioniert er für alle Browser, die von [signalr unterstützt](../../../signalr/overview/getting-started/supported-platforms.md)werden, ohne dass ein Browser-Plug-in erforderlich ist.
