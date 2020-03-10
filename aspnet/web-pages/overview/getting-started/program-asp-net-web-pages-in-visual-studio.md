---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: Programmieren von ASP.net Web Pages (Razor) mithilfe von Visual Studio | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Anhang wird erläutert, wie Sie Visual Studio 2010 oder Visual Web Developer 2010 Express verwenden können, um ASP.net Web Pages mit dem Razor-Syntax zu programmieren.
ms.author: riande
ms.date: 02/13/2014
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 1a76098779d05912bf7bdf2de5fdce024770752c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78514293"
---
# <a name="programming-aspnet-web-pages-razor-using-visual-studio"></a>Programmieren von ASP.net Web Pages (Razor) mithilfe von Visual Studio

von [Tom fitzmacken](https://github.com/tfitzmac)

> In diesem Artikel wird erläutert, wie Sie Visual Studio oder Visual Web Developer Express verwenden können, um ASP.net Web Pages Websites (Razor) zu programmieren.
>
> Sie lernen Folgendes
>
> - Was Sie benötigen, um mit ASP.net Web Pages in Ihrer Version von Visual Studio zu arbeiten.
> - Vorgehensweise beim Hinzufügen von Unterstützung für ASP.net Web Pages zu Visual Web Developer 2010 Express.
> - Verwenden von Funktionen in Visual Studio für die Arbeit mit ASP.net Razor Pages, einschließlich IntelliSense und dem Debugger.
>
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
>
>
> - ASP.net Web Pages (Razor) 3
> - Visual Studio 2013
> - Webmatrix 3
>
>
> Dieses Tutorial funktioniert auch mit ASP.net Web Pages 2, Visual Studio 2012, Visual Studio 2010 und webmatrix 2.

Sie können ASP.NET-Webseiten mit Razor-Syntax mithilfe von webmatrix oder vielen anderen Code-Editoren programmieren. Sie können auch Microsoft Visual Studio verwenden, bei dem es sich um eine integrierte Entwicklungsumgebung (Integrated Development Environment, IDE) mit vollem Funktionsumfang handelt, die eine Reihe leistungsfähiger Tools zum Erstellen vieler Anwendungs Typen (nicht nur Websites) bereitstellt. Um mit ASP.net Razor Pages Arbeiten zu können, können Sie [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)verwenden.

Zwei besonders nützliche Features, die Visual Studio für die Programmierung mit ASP.net Razor-Webseiten bietet:

- *IntelliSense*. Das IntelliSense-Feature, das in Visual Studio integriert ist, ist umfassender als IntelliSense in webmatrix.
- *Debugger*. Mit dem Debugger können Sie die Problembehandlung für den Code durchführen, indem Sie ein Programm während der Ausführung beenden, Variablen untersuchen und den Codezeilen Weise durchlaufen.

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a>Verwenden von Visual Studio mit verschiedenen Versionen von ASP.net Web Pages

Um ASP.net-Web-Apps in Visual Studio 2017 zu entwickeln, installieren Sie die Arbeitsauslastung **ASP.net und Webentwicklung** .

Visual Studio 2012 und Visual Studio 2013 enthalten Unterstützung für ASP.net Web Pages. (Die Pakete, die zur Unterstützung von ASP.net Web Pages erforderlich sind, werden bei der Installation von Visual Studio installiert.)

Visual Studio 2010 schließt standardmäßig keine Unterstützung für ASP.net Web Pages ein. Wenn Sie ASP.net Web Pages mit Visual Studio 2010 verwenden möchten, müssen Sie das ASP.NET-MVC-Paket installieren. Um ASP.net Web Pages 2 zu erhalten, installieren Sie ASP.NET MVC 4.

In der folgenden Tabelle wird die Unterstützung für ASP.net Web Pages in verschiedenen Versionen von Visual Studio zusammengefasst.

|  | Visual Studio 2010 | Visual Studio 2012 | Visual Studio 2013 |
| --- | --- | --- | --- |
| **ASP.net Web Pages 2** | Installieren von ASP.NET MVC 4 | Nahmen | Nahmen |
| **ASP.net Web Pages 3** |  | Aktualisieren auf ASP.net Web Pages 3 über nuget | Nahmen |

Informationen zum Arbeiten mit Visual Studio 2010 finden Sie [unter Installieren der Unterstützung für ASP.net Web Pages in Visual Studio 2010](#vs2010support).

## <a name="launching-visual-studio-from-webmatrix"></a>Starten von Visual Studio aus webmatrix

Wenn Sie ein Projekt in webmatrix gestartet haben und zu Visual Studio wechseln möchten, bietet webmatrix eine Schaltfläche, um das Projekt problemlos in Visual Studio zu öffnen. Sie müssen Visual Studio auf Ihrem Computer installiert haben, damit diese Schaltfläche aktiviert wird. Die folgende Abbildung zeigt die Schaltfläche in webmatrix.

![Starten von Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

Wenn Sie auf die Schaltfläche klicken, wird das Projekt in Visual Studio geöffnet. Sie können ohne Probleme zwischen webmatrix und Visual Studio hin-und herwechseln. Sie werden benachrichtigt, wenn sich Dateien in der anderen Umgebung geändert haben und erneut geladen werden müssen, um die neuesten Änderungen zu erhalten.

## <a name="creating-aspnet-razor-site-in-visual-studio"></a>Erstellen einer ASP.net Razor-Website in Visual Studio

So erstellen Sie eine ASP.net Razor-Website in Visual Studio:

1. Öffnen Sie Visual Studio.
2. Klicken Sie im Menü **Datei** auf **neue Website**.

    ![neue Website erstellen](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. Wählen Sie im Dialogfeld **neue Website** die zu verwendende Sprache (Visual C# oder Visual Basic) aus.
4. Wählen Sie die Vorlage **ASP.net Web Site (Razor)** aus.

    ![Razor-Site](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. Klicken Sie auf **OK**.

Ihr neues Projekt ist vorhanden und wird mit einigen Standard Webseiten aufgefüllt, um Ihnen den Einstieg zu erleichtern.

### <a name="using-intellisense"></a>Using IntelliSense

Nachdem Sie eine Website erstellt haben, können Sie sehen, wie IntelliSense in Visual Studio funktioniert.

1. Öffnen Sie auf der soeben erstellten Website die Seite *default. cshtml* .
2. Geben Sie nach den `<h3>`-Tags auf der Seite `@ServerInfo.` (einschließlich Punkt) ein. Beachten Sie, wie IntelliSense die verfügbaren Methoden für das `ServerInfo`-Hilfsprogramm in einer Dropdown Liste anzeigt.

    ![IntelliSense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. Wählen Sie die `GetHtml` Methode aus der Liste aus, und drücken Sie dann die EINGABETASTE. IntelliSense füllt automatisch die-Methode aus. (Wie bei jeder Methode in C#müssen Sie `()` Zeichen nach der-Methode hinzufügen.) Der vollständige Code für die `GetHtml`-Methode sieht wie im folgenden Beispiel aus:

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. Drücken Sie STRG + F5, um die Seite auszuführen. So sieht die Seite aus, wenn Sie in einem Browser angezeigt wird:

    ![Standardseite im Browser](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. Schließen Sie den Browser.

### <a name="using-the-debugger"></a>Verwenden des Debuggers

1. Fügen Sie am oberen Rand der Seite *default. cshtml* nach der Zeile, die mit `Page.Title`beginnt, die folgende Codezeile hinzu:

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. Klicken Sie im grauen Rand des Editors links neben dem Code auf neben dieser neuen Zeile, um einen *Breakpoint*hinzuzufügen. Ein Haltepunkt ist ein Marker, der den Debugger anweist, die Ausführung des Programms an diesem Punkt anzuhalten, damit Sie sehen können, was passiert.

    ![Haltepunkt festlegen](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. Entfernen Sie den aufzurufenden `ServerInfo.GetHtml`-Methode, und fügen Sie an seiner Stelle einen aufzurufenden `@myTime` Variablen hinzu. Dieser Aufruf zeigt den aktuellen Uhrzeitwert an, der von der neuen Codezeile zurückgegeben wird.
4. Drücken Sie F5, um die Seite im Debugger auszuführen. Die Seite wird an dem Haltepunkt angehalten, den Sie festlegen. In der folgenden Abbildung wird gezeigt, wie die Seite im Editor mit dem Haltepunkt (gelb) aussieht.

    ![Haltepunkt Debuggen](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. Klicken Sie in der Symbolleiste Debuggen auf die Schaltfläche Einzel **Schritt** (oder drücken Sie F11), um die nächste Codezeile auszuführen. Jedes Mal, wenn Sie auf diese Schaltfläche klicken, wird die Ausführung mit der nächsten Codezeile fortsetzen.

    ![Schaltfläche Einzelschritt](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. Überprüfen Sie den Wert der `myTime` Variablen, indem Sie den Mauszeiger darüber bewegen oder indem **Sie die Werte** in den Fenstern "lokal" und " **Telefon Stapel** " überprüfen. Visual Studio zeigt den Wert der Variablen an.

    ![Zeitwert anzeigen](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. Wenn Sie die Variable untersucht und den Code schrittweise durchlaufen haben, drücken Sie F5, um die Seite fortzusetzen, ohne in jeder Zeile anzuhalten. Nachdem Sie den gesamten Code schrittweise durchlaufen haben, zeigt der Browser die Seite an.

Weitere Informationen zum Debugger und zum Debuggen von Code in Visual Studio finden Sie unter Exemplarische Vorgehensweise [: Debuggen von Webseiten in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a>Verwenden von Razor in ASP.NET MVC-Projekten mit Visual Studio

Die Razor-Syntax wird auch intensiv in ASP.NET-MVC-Projekten verwendet. MVC ist eine leistungsstarke, auf Mustern basierende Methode zum Erstellen dynamischer Websites. Wenn die Verwaltung Ihrer ASP.net Web Pages Site schwierig wird, empfiehlt es sich, Sie in eine ASP.NET-MVC-Anwendung umzuwandeln. Ein Beispiel für das Erstellen einer MVC-Anwendung finden Sie unter [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a>Installieren der Unterstützung für ASP.net Web Pages in Visual Studio 2010

In diesem Abschnitt wird gezeigt, wie Visual Web Developer Express 2010 und die ASP.net Web Pages Tools für Visual Studio installiert werden.

1. Wenn Sie den Webplattform-Installer noch nicht installiert haben, laden Sie ihn unter der folgenden URL herunter:

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. Führen Sie den Webplattform-Installer aus.
3. Klicken Sie auf die Registerkarte **Produkte** .

    ![Registerkarte für webpi-Produkte](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. Suchen Sie nach **ASP.NET MVC 4** (für ASP.net Web Pages 2), und klicken Sie dann auf **Hinzufügen**. Diese Produkte umfassen Visual Studio-Tools zum Entwickeln von ASP.net Razor-Websites.

    ![Webpi-Installationsoptionen](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. Klicken Sie auf **Installieren** , um die Installation abzuschließen.
