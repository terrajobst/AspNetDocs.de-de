---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: Komponententests von signalr-Anwendungen | Microsoft-Dokumentation
author: bradygaster
description: In diesem Artikel wird beschrieben, wie die Komponenten Testfunktionen von signalr 2,0 verwendet werden.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: 2cf2e88f141d89971439dc1fc4979849f8dded47
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467301"
---
# <a name="unit-testing-signalr-applications"></a>Komponententests für SignalR-Anwendungen

von [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In diesem Artikel wird die Verwendung der Komponenten Testfunktionen von signalr 2 beschrieben.
>
> ## <a name="software-versions-used-in-this-topic"></a>In diesem Thema verwendete Software Versionen
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - Signalr Version 2
>
>
>
> ## <a name="questions-and-comments"></a>Fragen und Kommentare
>
> Bitte informieren Sie sich darüber, wie Ihnen dieses Tutorial gefallen hat und was wir in den Kommentaren unten auf der Seite verbessern konnten. Wenn Sie Fragen haben, die nicht direkt mit dem Tutorial zusammenhängen, können Sie Sie im [ASP.net signalr-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder in [StackOverflow.com](http://stackoverflow.com/)veröffentlichen.

<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a>Komponententests von signalr-Anwendungen

Sie können die Komponenten Testfunktionen in signalr 2 verwenden, um Komponententests für Ihre signalr-Anwendung zu erstellen. Signalr 2 enthält die [ihubcallerconnectioncontext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) -Schnittstelle, die verwendet werden kann, um ein Mock-Objekt zum Simulieren der hubmethoden für Tests zu erstellen.

In diesem Abschnitt fügen Sie Komponententests für die Anwendung hinzu, die Sie im [Tutorial "Getting Started](../getting-started/tutorial-getting-started-with-signalr.md) " mithilfe von [XUnit.net](https://github.com/xunit/xunit) [und der](https://github.com/Moq/moq4)-Version erstellt haben.

XUnit.net wird verwendet, um den Test zu steuern. MOQ wird verwendet, um ein [Mock](http://en.wikipedia.org/wiki/Mock_object) -Objekt für Tests zu erstellen. Andere Frameworks können bei Bedarf verwendet werden. [Nersatz](http://nsubstitute.github.io/) ist auch eine gute Wahl. In diesem Tutorial wird veranschaulicht, wie Sie das Mock-Objekt auf zwei Arten einrichten: zuerst mithilfe eines `dynamic` Objekts (eingeführt in .NET Framework 4) und zweitens über eine Schnittstelle.

### <a name="contents"></a>Inhalt

Dieses Tutorial enthält die folgenden Abschnitte.

- [Unittests mit dynamischer](#dynamic)
- [Komponententests nach Typ](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a>Unittests mit dynamischer

In diesem Abschnitt fügen Sie einen Komponenten Test für die Anwendung hinzu, die im [Tutorial "Getting Started](../getting-started/tutorial-getting-started-with-signalr.md) " mithilfe eines dynamischen Objekts erstellt wurde.

1. Installieren Sie die [xUnit Runner-Erweiterung](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) für Visual Studio 2013.
2. Führen Sie entweder das [Tutorial zu](../getting-started/tutorial-getting-started-with-signalr.md)den ersten Schritten aus, oder laden Sie die fertige Anwendung aus der [MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)herunter.
3. Wenn Sie die Download Version der Anwendung für die ersten Schritte verwenden, öffnen Sie die **Paket-Manager-Konsole** , und klicken Sie auf **Wiederherstellen** , um dem Projekt das signalr-Paket hinzuzufügen.

    ![Pakete wiederherstellen](unit-testing-signalr-applications/_static/image1.png)
4. Fügen Sie der Projekt Mappe ein Projekt für den Komponenten Test hinzu. Klicken Sie in **Projektmappen-Explorer** mit der rechten Maustaste auf Ihre Projekt Mappe, und wählen Sie **Hinzufügen**, **Neues Projekt**aus. Wählen Sie **C#** unter dem Knoten den Knoten **Windows** aus. Wählen Sie **Klassenbibliothek**aus. Nennen Sie das neue Projekt **testlibrary** , und klicken Sie auf **OK**.

    ![Test Bibliothek erstellen](unit-testing-signalr-applications/_static/image2.png)
5. Fügen Sie dem Projekt signalrchat einen Verweis im Test Bibliotheksprojekt hinzu. Klicken Sie mit der rechten Maustaste auf das Projekt **testlibrary** , und wählen Sie **Hinzufügen**, **Verweis...** . Wählen Sie **unter dem Knoten** Projekt Mappe den Knoten **Projekte** aus, und überprüfen Sie **signalrchat**. Klicken Sie auf **OK**.

    ![Projekt Verweis hinzufügen](unit-testing-signalr-applications/_static/image3.png)
6. Fügen Sie die Module signalr, muq und xUnit dem Projekt **testlibrary** hinzu. Legen Sie in der **Paket-Manager-Konsole**die Dropdown Liste **Standard Projekt** auf **testlibrary**fest. Führen Sie im Konsolenfenster die folgenden Befehle aus:

   - `Install-Package Microsoft.AspNet.SignalR`
   - `Install-Package Moq`
   - `Install-Package XUnit`

     ![Installieren von Paketen](unit-testing-signalr-applications/_static/image4.png)
7. Erstellen Sie die Testdatei. Klicken Sie mit der rechten Maustaste auf das Projekt **testlibrary** **, und**klicken Sie auf **hinzufügen.** Nennen Sie die neue Klasse **Tests.cs**.
8. Ersetzen Sie den Inhalt von Tests.cs durch den folgenden Code.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    Im obigen Code wird ein-Test Client mit dem `Mock`-Objekt aus der- [Bibliothek der](https://github.com/Moq/moq4) Bibliothek vom Typ [ihubcallerconnectioncontext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in signalr 2,1 `dynamic` für den Typparameter zugewiesen) erstellt. Die `IHubCallerConnectionContext`-Schnittstelle ist das Proxy Objekt, mit dem Sie Methoden auf dem Client aufrufen. Die `broadcastMessage`-Funktion wird dann für den mockclient definiert, sodass Sie von der `ChatHub`-Klasse aufgerufen werden kann. Die Test-Engine ruft dann die `Send`-Methode der `ChatHub`-Klasse auf, die wiederum die simulierte `broadcastMessage`-Funktion aufruft.
9. Erstellen Sie die Projekt Mappe, indem Sie **F6**drücken.
10. Führen Sie den Komponententest aus. Wählen Sie in Visual Studio **Test**, **Windows**, **Test-Explorer**aus. Klicken Sie im Test-Explorer-Fenster mit der rechten Maustaste auf **hubsaremockableviadynamic** , und wählen Sie **Ausgewählte Tests ausführen**aus.

    ![Test-Explorer](unit-testing-signalr-applications/_static/image5.png)
11. Überprüfen Sie, ob der Test erfolgreich war, indem Sie den unteren Bereich im Fenster Test-Explorer aktivieren. Im Fenster wird angezeigt, dass der Test erfolgreich war.

    ![Bestandene Tests](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a>Komponententests nach Typ

In diesem Abschnitt fügen Sie einen Test für die im [Tutorial "Getting Started](../getting-started/tutorial-getting-started-with-signalr.md) " erstellte Anwendung mithilfe einer Schnittstelle hinzu, die die zu testende Methode enthält.

1. Führen Sie die Schritte 1-7 im obigen Tutorial für Komponenten [Tests mit dynamischem](#dynamic) Lernprogramm aus.
2. Ersetzen Sie den Inhalt von Tests.cs durch den folgenden Code.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    Im obigen Code wird eine Schnittstelle erstellt, die die Signatur der `broadcastMessage` Methode definiert, für die die Test-Engine einen Mock-Client erstellt. Anschließend wird ein Mock-Client mithilfe des `Mock` Objekts vom Typ " [ihubcallerconnectioncontext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) " (in signalr 2,1 `dynamic` für den Typparameter zugewiesen) erstellt. Die `IHubCallerConnectionContext`-Schnittstelle ist das Proxy Objekt, mit dem Sie Methoden auf dem Client aufrufen.

    Der Test erstellt dann eine Instanz von `ChatHub`und erstellt dann eine Pseudo Version der `broadcastMessage`-Methode, die wiederum durch Aufrufen der `Send`-Methode auf dem Hub aufgerufen wird.
3. Erstellen Sie die Projekt Mappe, indem Sie **F6**drücken.
4. Führen Sie den Komponententest aus. Wählen Sie in Visual Studio **Test**, **Windows**, **Test-Explorer**aus. Klicken Sie im Test-Explorer-Fenster mit der rechten Maustaste auf **hubsaremockableviadynamic** , und wählen Sie **Ausgewählte Tests ausführen**aus.

    ![Test-Explorer](unit-testing-signalr-applications/_static/image7.png)
5. Überprüfen Sie, ob der Test erfolgreich war, indem Sie den unteren Bereich im Fenster Test-Explorer aktivieren. Im Fenster wird angezeigt, dass der Test erfolgreich war.

    ![Bestandene Tests](unit-testing-signalr-applications/_static/image8.png)
