---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
title: Erste Schritte | Microsoft-Dokumentation
author: Rick-Anderson
description: Webmatrix wird nicht mehr als integrierte Entwicklungsumgebung für ASP.net Web Pages empfohlen. Verwenden Sie Visual Studio oder Visual Studio Code. Dieser Leitfaden...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: a36d3bdf-ef1b-47a4-b932-3a0cf4cad716
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
msc.type: authoredcontent
ms.openlocfilehash: bb863f8605e6f8faca3b285607b63a3e88e83012
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78440241"
---
# <a name="getting-started"></a>Erste Schritte

von [Tom fitzmacken](https://github.com/tfitzmac)

[!INCLUDE[](~/includes/rp.md)]

> > [!NOTE] 
> > 
> > Webmatrix wird nicht mehr als integrierte Entwicklungsumgebung für ASP.net Web Pages empfohlen. Verwenden Sie [Visual Studio](../program-asp-net-web-pages-in-visual-studio.md) oder [Visual Studio Code](https://code.visualstudio.com/).
> 
> 
> Diese Anleitung und Anwendung bietet Ihnen einen Überblick über ASP.net Web Pages (Version 2 oder höher) und Razor-Syntax ein schlankes Framework zum Erstellen dynamischer Websites. Außerdem wird webmatrix eingeführt, ein Tool zum Erstellen von Seiten und Websites.
> 
> **Ebene**: neu in ASP.net Web Pages.  
> **Kenntnisse**: HTML, grundlegende Cascading Stylesheets (CSS).
> 
> Informationen, die Sie im ersten Tutorial des Satzes erlernen:
> 
> - Was ASP.net Web Pages Technologie ist und wofür Sie sich eignet.
> - Was webmatrix ist.
> - So installieren Sie alles.
> - Erstellen einer Website mithilfe von webmatrix.
>   
> 
> Erörterte Features und Technologien:
> 
> - Microsoft-Webplattform-Installer.
> - WebMatrix.
> - *cshtml* -Seiten
>   
> 
> Mike Papst hat dieses Tutorial ursprünglich verfasst. Tom fitzmacken wurde für Microsoft webmatrix 3 aktualisiert.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
> 
> 
> - ASP.net Web Pages (Razor) 2
> - Webmatrix 3

## <a name="what-should-you-know"></a>Was sollten Sie wissen?

Wir gehen davon aus, dass Sie mit folgenden Aktionen vertraut sind:

- **HTML**. Es ist kein detailliertes Fachwissen erforderlich. Wir erklären HTML nicht, aber wir verwenden auch keine komplexen. Wir geben Links zu HTML-Tutorials an, in denen wir denken, dass Sie hilfreich sind.
- **Cascading Stylesheets (CSS)** . Identisch mit HTML.
- **Grundlegende Daten Bank Ideen**. Wenn Sie ein Arbeitsblatt für Daten verwendet haben und die Daten sortiert und gefiltert haben, ist dies das Fachwissen, das wir in der Regel für dieses Lernprogramm festlegen.

Wir gehen auch davon aus, dass Sie an der grundlegenden Programmierung interessiert sind. ASP.net Web Pages eine Programmiersprache mit dem C#Namen verwenden. Sie müssen keinen Hintergrund in der Programmierung haben, sondern nur ein Interesse an der Programmierung. Wenn Sie schon einmal JavaScript auf einer Webseite geschrieben haben, haben Sie viel Hintergrund.

Wenn Sie mit der Programmierung vertraut sind, werden Sie möglicherweise feststellen, dass diese tutorialreihe sich anfänglich langsam bewegt, während wir neue Programmierer schneller bringen. Wenn wir jedoch die ersten Lernprogramme überschreiten, gibt es weniger grundlegende Programmierungen, die Sie in einem schnelleren Clip Zusammenführen werden.

## <a name="what-do-you-need"></a>Was benötigen Sie?

Sie benötigen Folgendes:

- Ein Computer, auf dem Windows 8, Windows 7, Windows Server 2008 oder Windows Server 2012 ausgeführt wird.
- Eine Live-Internetverbindung.
- Administrator Rechte (erforderlich für den Installationsvorgang).

## <a name="what-is-aspnet-web-pages"></a>Was ist ASP.net Web Pages?

ASP.net Web Pages ist ein Framework, das Sie zum Erstellen dynamischer Webseiten verwenden können. Eine einfache HTML-Webseite ist statisch. der Inhalt wird durch das fixierte HTML-Markup auf der Seite bestimmt. Dynamische Seiten wie solche, die Sie mit ASP.net Web Pages erstellen, ermöglichen es Ihnen, den Seiten Inhalt dynamisch zu erstellen, indem Sie Code verwenden.

Mit dynamischen Seiten können Sie alle möglichen Aktionen ausführen. Sie können einen Benutzer über ein Formular zur Eingabe auffordern und dann ändern, was die Seite anzeigt oder wie Sie aussieht. Sie können Informationen von einem Benutzer übernehmen, in einer Datenbank speichern und später auflisten. Sie können eine e-Mail von Ihrer Site senden. Sie können mit anderen Diensten im Web (z. b. einem Mapping-Dienst) interagieren und Seiten entwickeln, die Informationen aus diesen Quellen integrieren.

## <a name="what-is-webmatrix"></a>Was ist webmatrix?

Webmatrix ist ein Tool, das einen Webseiten-Editor, ein Daten Bank Dienstprogramm, einen Webserver zum Testen von Seiten und Features zum Veröffentlichen Ihrer Website im Internet integriert. Webmatrix ist kostenlos, und es ist einfach zu installieren und zu verwenden. (Dies funktioniert auch für einfache HTML-Seiten sowie für andere Technologien wie PHP.)

Sie *müssen* webmatrix nicht verwenden, um mit ASP.net Web Pages zu arbeiten. Sie können Seiten mithilfe eines Text-Editors erstellen, z. b. und Testen von Seiten mithilfe eines Webservers, auf den Sie Zugriff haben. Webmatrix macht es jedoch ganz einfach, sodass in diesen Tutorials webmatrix verwendet wird.

## <a name="about-these-tutorials"></a>Informationen zu diesen Tutorials

Dieser Tutorial-Satz ist eine Einführung in die Verwendung von ASP.net Web Pages. In diesem Einführungsprogramm Satz sind insgesamt 9 Tutorials vorhanden. Es ist Teil einer Reihe von Lernprogramm Sätzen, die Sie von ASP.net Web Pages Einsteiger zum Erstellen realer, professionell aussehenden Websites gelangen.

In diesem ersten Lernprogramm Satz werden die Grundlagen der Arbeit mit ASP.net Web Pages erläutert. Wenn Sie fertig sind, können Sie mit zusätzlichen Lernprogramm Sätzen arbeiten, die an dieser Stelle teilnehmen und Webseiten ausführlicher untersuchen.

Wir gehen bewusst auf die ausführlichen Erläuterungen ein. Und wenn wir etwas anzeigen, wählen wir für dieses Tutorial immer die Methode aus, mit der wir die Ansicht am einfachsten verstehen. Spätere Lernprogramm Sätze gehen ausführlicher vor und zeigen effizientere oder flexiblere Ansätze (auch mehr Spaß). Für diese Tutorials müssen Sie sich jedoch zunächst mit den Grundlagen vertraut machen.

Der soeben gestartete Lernprogramm Satz behandelt diese Features von ASP.net Web Pages:

- Einführung und Installation alles. (Das ist das Tutorial, das Sie gerade lesen.)
- Grundlagen der ASP.net Web Pages Programmierung.
- Erstellen einer Datenbank.
- Erstellen und Verarbeiten eines Benutzereingabe Formulars.
- Hinzufügen, aktualisieren und Löschen von Daten in der Datenbank.

## <a name="what-will-you-create"></a>Was wird erstellt?

In diesem Lernprogramm Satz und den nachfolgenden Tutorials dreht sich um eine Website, auf der Sie Filme auflisten können. Sie können Filme eingeben, bearbeiten und auflisten. Im folgenden finden Sie eine Reihe von Seiten, die Sie in diesem Tutorial erstellen. Der erste zeigt die filmauflistungs Seite, die Sie erstellen:

![Finshed Movie-App, die eine Filmliste anzeigt](getting-started/_static/image1.png)

Und hier ist die Seite, auf der Sie Ihrer Website neue Filminformationen hinzufügen können:

![Beendete Movie-App mit der Seite "Film hinzufügen"](getting-started/_static/image2.png)

Nachfolgende Lernprogramm Sätze bauen auf diesem Satz auf und fügen weitere Funktionen hinzu, wie das Hochladen von Bildern, das Anmelden von Benutzern, das Senden von e-Mails und das integrieren in soziale Medien.

## <a name="see-this-app-running-on-azure"></a>Diese APP wird in Azure ausgeführt.

Möchten Sie sehen, dass die fertige Site als Live-Web-App ausgeführt wird? Sie können eine vollständige Version der APP für Ihr Azure-Konto bereitstellen, indem Sie einfach auf die folgende Schaltfläche klicken.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebPagesMovies)

Sie benötigen ein Azure-Konto, um diese Lösung in Azure bereitzustellen. Wenn Sie noch nicht über ein Konto verfügen, haben Sie die folgenden Optionen:

- [Öffnen Sie ein Azure-Konto kostenlos](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) . Sie erhalten Gutschriften, die Sie verwenden können, um kostenpflichtige Azure-Dienste zu testen. selbst nach ihrer Nutzung können Sie das Konto behalten und kostenlose Azure-Dienste nutzen.
- [Aktivieren von MSDN-Abonnenten Vorteilen](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) : Ihr MSDN-Abonnement bietet Ihnen jeden Monat ein Guthaben, das Sie für kostenpflichtige Azure-Dienste nutzen können.

## <a name="installing-everything"></a>Installieren von allem

Sie können alles mit dem Webplattform-Installer von Microsoft installieren. Tatsächlich installieren Sie das Installationsprogramm und verwenden es dann, um alles andere zu installieren.

Um Webseiten verwenden zu können, muss mindestens Windows XP mit SP3 oder Windows Server 2008 oder höher installiert sein.

Klicken Sie auf der [Seite Webseiten](../../../index.md) der ASP.NET-Website auf **Installieren**.

![ASP.NET-Website mit &quot;Schaltfläche "webmatrix&quot; installieren"](getting-started/_static/image3.png)

Sie werden aufgefordert, die Lizenzbedingungen und Datenschutzbestimmungen vor der Installation von webmatrix zu akzeptieren.

![Laufzeit akzeptieren, um die Installation zu starten](getting-started/_static/image4.png)

Klicken Sie auf **Ausführen** , um die Installation zu starten. (Wenn Sie das Installationsprogramm speichern möchten, klicken Sie auf **Speichern** , und führen Sie dann das Installationsprogramm in dem Ordner aus, in dem Sie ihn gespeichert haben.)

![](getting-started/_static/image5.png)

Der Webplattform-Installer wird angezeigt, und Sie können webmatrix installieren. Klicken Sie auf **Installieren**.

![](getting-started/_static/image6.png)

Im Rahmen des Installationsvorgangs wird ermittelt, was auf dem Computer installiert werden muss, und der Prozess wird gestartet. Je nachdem, was genau installiert werden muss, kann der Prozess von einigen Minuten bis zu mehreren Minuten dauern. Wählen Sie **Ich akzeptiere** aus, um die Lizenzbedingungen zu akzeptieren.

## <a name="hello-webmatrix"></a>Hello, webmatrix

Wenn dies der Fall ist, kann der Installationsvorgang webmatrix automatisch starten. Wenn dies nicht der Fall ist, starten Sie **Microsoft webmatrix**in Windows über das **Startmenü** .

Wenn Sie webmatrix zum ersten Mal starten, erhalten Sie die Möglichkeit, sich mit Ihrem Microsoft-Konto bei Microsoft Azure anzumelden. Wenn Sie sich anmelden, erhalten Sie 10 kostenlose Web-Apps über Azure. Diese kostenlosen Web-Apps stellen eine bequeme Möglichkeit zum Testen Ihrer Apps dar. Wenn Sie noch nicht über ein Azure-Konto verfügen, aber über ein MSDN-Abonnement verfügen, können Sie [Ihre MSDN-Abonnement Vorteile aktivieren](https://www.windowsazure.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604). Andernfalls können Sie in wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Einen Monat kostenlos testen](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

Sie müssen sich nicht sofort anmelden, um mit diesem Tutorial fortzufahren. Wenn Sie sich jetzt nicht anmelden, haben Sie noch die Möglichkeit, sich später anzumelden. Das letzte [Thema](publishing.md) in dieser tutorialreihe behandelt, wie Sie Ihre Website in Azure bereitstellen. Daher müssen Sie sich anmelden, um dieses Thema abzuschließen.

Melden Sie sich an diesem Punkt entweder mit Ihrem Microsoft-Konto an, oder wählen Sie **nicht** in der unteren rechten Ecke aus.

![Anmelden](getting-started/_static/image7.png)

Zunächst erstellen Sie eine leere Website und fügen eine Seite hinzu. In einem späteren Tutorial dieses Satzes können Sie mit einer der integrierten Website Vorlagen experimentieren.

Klicken Sie im Startfenster auf **neu**.

![Webmatrix-Startbildschirm](getting-started/_static/image8.png)

Vorlagen sind vorgefertigte Dateien und Seiten für verschiedene Arten von Websites. Wählen Sie die Option Vorlagen Katalog aus, um alle Vorlagen anzuzeigen, die standardmäßig verfügbar sind.

![Vorlagen Katalog auswählen](getting-started/_static/image9.png)

Wählen Sie im Fenster **Schnellstart** die Option **leere Website** aus der Gruppe **ASP.net** aus, und nennen Sie die neue Website "webpaar-Filme".

![Webmatrix Schnellstart Fenster mit leerer Website Vorlage ausgewählt](getting-started/_static/image10.png)

Klicken Sie auf **Weiter**.

Wenn Sie sich bei Ihrem Microsoft-Konto angemeldet haben, erhalten Sie die Möglichkeit, die Website in Azure zu erstellen. Basierend auf dem Namen Ihrer Site wird der Standardname **WebPagesMovies.azurewebsites.net** vorgeschlagen. Das Ausrufezeichen gibt jedoch an, dass dieser Name nicht in Windows Azure verfügbar ist. Wählen Sie aus Gründen der Einfachheit die Option über **springen** aus, um das Erstellen der Website in Azure momentan zu umgehen. Später in dieser Reihe veröffentlichen wir die Website in Azure.

![Azure-Website erstellen](getting-started/_static/image11.png)

Webmatrix erstellt und öffnet die Website:

![Neue webpgesmovies-Website in webmatrix geöffnet](getting-started/_static/image12.png)

Im oberen Bereich gibt es eine Symbolleiste für den schnell Zugriff und eine Multifunktionsleiste. Unten links sehen Sie die Arbeitsbereichs Auswahl, in der Sie zwischen Aufgaben wechseln (**Website**, **Dateien**, **Datenbanken**, **Berichte**). Auf der rechten Seite befindet sich der Inhalts Bereich für den Editor und für Berichte. Und im unteren Bereich sehen Sie gelegentlich eine Benachrichtigungsleiste für Nachrichten.

Weitere Informationen zu webmatrix und den dazugehörigen Features finden Sie in diesen Tutorials.

## <a name="creating-a-web-page"></a>Erstellen einer Webseite

Um sich mit webmatrix und ASP.net Web Pages vertraut zu machen, erstellen Sie eine einfache Seite.

Wählen Sie in der Arbeitsbereichs Auswahl den Arbeitsbereich **Dateien** aus. In diesem Arbeitsbereich können Sie mit Dateien und Ordnern arbeiten. Der linke Bereich zeigt die Dateistruktur Ihrer Site. Das Menüband wird geändert, um Datei bezogene Aufgaben anzuzeigen.

![Datei Arbeitsbereich in webmatrix](getting-started/_static/image13.png)

Klicken Sie im Menüband auf den Pfeil unter **neu** , und klicken Sie dann auf **neue Datei**.

![Verwenden des Befehls "&quot;New&quot;" im Menüband zum Erstellen einer neuen Datei](getting-started/_static/image14.png)

Webmatrix zeigt eine Liste von Dateitypen an. Wählen Sie **cshtml**aus, und geben Sie im Feld **Name den Namen** "HelloWorld" ein. Eine cshtml-Seite ist eine ASP.net Web Pages Seite.

![Erstellen einer neuen cshtml-Seite mit dem Namen "HelloWorld. cshtml"](getting-started/_static/image15.png)

Klicken Sie auf **OK**.

Webmatrix erstellt die Seite und öffnet Sie im Editor.

![Die neue Seite "HelloWorld" im webmatrix-Editor](getting-started/_static/image16.png)

Wie Sie sehen können, enthält die Seite größtenteils normales HTML-Markup, mit Ausnahme eines Blocks oben, der wie folgt aussieht:

[!code-cshtml[Main](getting-started/samples/sample1.cshtml)]

Das ist das Hinzufügen von Code, wie Sie in Kürze sehen werden.

Beachten Sie, dass die verschiedenen Teile der &mdash; Seite die Elementnamen, Attribute und Text sowie den Block oben – in unterschiedlichen Farben haben. Dies wird als *Syntax Hervorhebung*bezeichnet und erleichtert es, alles klar zu machen. Es ist eine der Features, die das Arbeiten mit Webseiten in webmatrix erleichtern.

Fügen Sie Inhalte für die `<head>` und `<body>` Elemente wie im folgenden Beispiel hinzu. (Wenn Sie möchten, können Sie einfach den folgenden-Block kopieren und die gesamte vorhandene Seite durch diesen Code ersetzen.)

[!code-cshtml[Main](getting-started/samples/sample2.cshtml)]

Klicken Sie in der Symbolleiste für den schnell Zugriff oder im Menü **Datei** auf **Speichern**.

![Schaltfläche "Speichern" in der webmatrix-Symbolleiste](getting-started/_static/image17.png)

## <a name="testing-the-page"></a>Testen der Seite

Klicken Sie im Arbeitsbereich " **Dateien** " mit der rechten Maustaste auf die Seite *HelloWorld. cshtml* , und klicken Sie dann auf **im Browser starten**.

![Ausführen einer Seite mit der Schaltfläche "ausführen" im webmatrix-Menüband](getting-started/_static/image18.png)

Webmatrix startet einen integrierten Webserver (IIS Express), den Sie zum Testen von Seiten auf dem Computer verwenden können. (Ohne IIS Express in webmatrix müssten Sie die Seite vor dem Testen auf einem Webserver veröffentlichen.) Die Seite wird in Ihrem Standardbrowser angezeigt.

![&quot;Hallo Welt&quot; Seite, die im Browser ausgeführt wird](getting-started/_static/image19.png)

Beachten Sie, dass beim Testen einer Seite in webmatrix die URL im Browser in etwa wie `http://localhost:33651/HelloWorld.cshtml.` der Name " *localhost* " auf einen lokalen Server verweist. Dies bedeutet, dass die Seite von einem Webserver bedient wird, der sich auf Ihrem Computer befindet. Wie bereits erwähnt, enthält webmatrix ein Webserver Programm mit dem Namen IIS Express, das ausgeführt wird, wenn Sie eine Seite starten.

Die Zahl nach *localhost* (z. b. *localhost: 33651*) bezieht sich auf eine *Portnummer* auf dem Computer. Dies ist die Nummer des "Kanals", die IIS Express für diese bestimmte Website verwendet. Die Portnummer wird nach dem Zufallsprinzip aus dem Bereich 1024 bis 65536 ausgewählt, wenn Sie den Standort erstellen, und Sie ist für jeden Standort, den Sie erstellen, unterschiedlich. (Wenn Sie Ihre eigene Website testen, ist die Portnummer mit Sicherheit eine andere Nummer als 33561.) Wenn Sie einen anderen Port für jede Website verwenden, können IIS Express den Stand ihrer Websites direkt halten.

Wenn Sie Ihre Website später auf einem öffentlichen Webserver veröffentlichen, wird " *localhost* " in der URL nicht mehr angezeigt. An diesem Punkt sehen Sie eine eher typische URL wie `http://myhostingsite/mywebsite/HelloWorld.cshtml` oder die Seite, auf der sich die Seite befindet. Weitere Informationen zum Veröffentlichen einer Website finden Sie später in dieser tutorialreihe.

## <a name="adding-some-server-side-code"></a>Hinzufügen von Server seitigem Code

Schließen Sie den Browser, und wechseln Sie zurück zu der Seite in webmatrix.

Fügen Sie dem Codeblock eine Zeile hinzu, damit Sie wie der folgende Code aussieht:

[!code-cshtml[Main](getting-started/samples/sample3.cshtml)]

Dies ist ein wenig Razor-Code. Es ist wahrscheinlich klar, dass das aktuelle Datum und die Uhrzeit abgerufen werden und der Wert in eine *Variable* namens "`currentDateTime`" eingefügt wird. Im nächsten Tutorial erfahren Sie mehr über Razor-Syntax.

Fügen Sie im Text der Seite hinter dem `<p>Hello World!</p>`-Element Folgendes hinzu:

[!code-html[Main](getting-started/samples/sample4.html)]

Mit diesem Code wird der Wert abgerufen, den Sie im oberen Bereich der `currentDateTime` Variablen eingefügt haben, und in das Markup der Seite eingefügt. Das `@` Zeichen markiert den ASP.net Web Pages Code auf der Seite.

Führen Sie die Seite erneut aus (webmatrix speichert die Änderungen für Sie, bevor die Seite ausgeführt wird). Dieses Mal sehen Sie das Datum und die Uhrzeit auf der Seite.

![&quot;Hallo Welt&quot; Seite, die im Browser mit einer dynamisch generierten Zeitanzeige ausgeführt wird](getting-started/_static/image20.png)

Warten Sie einen Moment, und aktualisieren Sie dann die Seite im Browser. Die Datums-und Uhrzeit Anzeige wird aktualisiert.

Sehen Sie sich im Browser die Seitenquelle an. Das folgende Markup sieht wie folgt aus:

[!code-html[Main](getting-started/samples/sample5.html)]

Beachten Sie, dass der `@{ }` Block oben nicht vorhanden ist. Beachten Sie auch, dass die Datums-und Uhrzeit Anzeige eine tatsächliche Zeichenfolge (`1/18/2012 2:49:50 PM` oder ähnliches) anzeigt, nicht `@currentDateTime` wie auf der Seite *. cshtml* . Der Grund dafür ist, dass ASP.NET den gesamten Code verarbeitet hat (in diesem Fall nur sehr wenig), der mit `@`gekennzeichnet wurde. Der Code erzeugt eine Ausgabe, und diese Ausgabe wurde in die Seite eingefügt.

## <a name="this-is-what-aspnet-web-pages-are-about"></a>Im folgenden werden ASP.net Web Pages

Wenn Sie lesen, dass ASP.net Web Pages dynamischen Webinhalt erzeugt, sehen Sie sich die Idee an. Die Seite, die Sie soeben erstellt haben, enthält das gleiche HTML-Markup, das Sie bereits gesehen haben. Sie kann auch Code enthalten, der alle Arten von Aufgaben ausführen kann. In diesem Beispiel wurde die triviale Aufgabe zum erhalten des aktuellen Datums und der aktuellen Uhrzeit. Wie Sie gesehen haben, können Sie Code mit HTML interagieren, um die Ausgabe auf der Seite zu liefern. Wenn ein Benutzer eine *. cshtml* -Seite im Browser anfordert, verarbeitet ASP.net die Seite, während Sie sich noch in der Hand des Webservers befindet. ASP.NET fügt die Ausgabe des Codes (sofern vorhanden) in die Seite als HTML ein. Wenn die Code Verarbeitung abgeschlossen ist, sendet ASP.net die resultierende Seite an den Browser. Der gesamte Browser wird als HTML angezeigt. Im folgenden finden Sie ein Diagramm:

![Konzeptioneller Fluss der dynamischen Generierung von HTML durch ASP.net](getting-started/_static/image21.png)

Die Idee ist einfach, aber es gibt viele interessante Aufgaben, die der Code ausführen kann, und es gibt viele interessante Möglichkeiten, wie Sie HTML-Inhalt dynamisch auf der Seite hinzufügen können. Und die ASP.net *. cshtml* -Seiten, wie jede beliebige HTML-Seite, können auch Code enthalten, der im Browser selbst ausgeführt wird (JavaScript-und jQuery-Code). Alle diese Aspekte werden in diesem Lernprogramm Satz und in nachfolgenden Tutorials erläutert.

## <a name="coming-up-next"></a>Nächste nächste

Im nächsten Tutorial dieser Reihe erfahren Sie, wie Sie ASP.net Web Pages programmieren können.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Erstellen Sie eine ASP.NET-Website von Grund auf neu](https://www.microsoft.com/web/post/create-an-aspnet-website-from-scratch). Dies ist ein Tutorial, in dem insbesondere webmatrix (nicht ASP.net Web Pages) verwendet wird. Es werden einige der zusätzlichen Features von webmatrix, die wir in diesem Lernprogramm Satz nicht abdecken, ausführlicher erläutert.

> [!div class="step-by-step"]
> [Weiter](intro-to-web-pages-programming.md)
