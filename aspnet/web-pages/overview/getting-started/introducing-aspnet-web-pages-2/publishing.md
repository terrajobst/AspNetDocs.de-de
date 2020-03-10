---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
title: 'Einführung in ASP.NET Web Pages: Veröffentlichen einer Website mit WebMatrix | Microsoft-Dokumentation'
author: Rick-Anderson
description: Dieses Tutorial ist die letzte Folge der tutorialreihe, die ASP.NET Web Pages und Microsoft WebMatrix eingeführt werden. Es wird erläutert, wie zum Veröffentlichen Ihrer Website t...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 7e85c70e-1a88-4408-8b3d-29611c7713ed
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
msc.type: authoredcontent
ms.openlocfilehash: 49a841dbda183bf1d59153b83f694c9f517e0b94
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78514383"
---
# <a name="introducing-aspnet-web-pages---publishing-a-site-by-using-webmatrix"></a>Einführung in ASP.NET Web Pages - Veröffentlichen einer Website mit WebMatrix

von [Tom fitzmacken](https://github.com/tfitzmac)

> Dieses Tutorial ist die letzte Folge der tutorialreihe, die ASP.NET Web Pages und Microsoft WebMatrix eingeführt werden. Es wird erläutert, wie Ihre Website mit dem Internet zu veröffentlichen, damit andere Benutzer damit arbeiten können. Es wird vorausgesetzt, dass Sie die Reihe durchgeführt haben, indem Sie [eine konsistente Suche nach ASP.net Web Pages Websites erstellen](https://go.microsoft.com/fwlink/?LinkId=251585).
> 
> Sie erfahren, wie zum Veröffentlichen Ihrer Website verwenden können:
> 
> - Microsoft Azure
> - Web-Hosting-Unternehmen

## <a name="about-publishing-your-site"></a>Zum Veröffentlichen Ihrer Website

Bisher haben Sie Ihre Arbeit auf einem lokalen Computer, z. B. Ihre Seiten testen fertig. Um die<em>cshtml</em> -Seiten auszuführen, haben Sie den Webserver verwendet, der in webmatrix integriert ist, nämlich IIS Express. Aber natürlich niemand können finden Sie auf der Website, die Sie außer dass Sie erstellt haben. Damit um andere Benutzer die Arbeit mit Ihrer Website zu können, müssen Sie sie mit dem Internet zu veröffentlichen.

Wenn Sie nicht bereits über Zugriff auf einen öffentlichen Webserver verfügen, bedeutet das veröffentlichen, dass Sie über ein Konto mit einer *cloudplattform* oder einem *Hostinganbieter*verfügen müssen. Eine Cloud-Plattform, z. B. Microsoft Azure stellt bei Bedarf Infrastruktur bereit, für Ihre Anwendungen. Ein hosting-Anbieter ist ein Unternehmen, die öffentlich zugänglichen Webserver besitzt und wird, die zu mieten Sie, Speicherplatz für Ihre Website. Hosten von Plänen aus wenige Euro im Monat ausgeführt (oder sogar kostenlos) für kleine Standorte mit vielen Hunderten von Euro im Monat für umfangreiche kommerzielle Websites.

> [!NOTE]
> Möglicherweise haben Sie Zugriff auf einen öffentlichen Webserver über die Internetdienstanbieter (ISP), mit denen Sie zu Hause Internetdienst zu erhalten. Allerdings muss der Hostinganbieter ASP.NET Web Pages unterstützt. Viele ISPs nicht, aber es lohnt sich immer überprüfen.

In diesem Tutorial haben erhalten wir einen Überblick über die Informationen zum Veröffentlichen Sie. Es ist nicht praktikabel ist, geben Sie die genauen Details für alle Elemente, da ein wenig unterscheidet sich der Vorgang für alle Hostinganbieter. Aber Sie erhalten einen guten Überblick darüber, wie der Prozess ausgeführt.

Dieses Lernprogramm enthält vier Abschnitte:

1. [Einrichten der Standardseite](#defaultpage)
2. Veröffentlichen (Wählen Sie eine der folgenden)  
 a. [Veröffentlichen Ihrer Website auf Microsoft Azure](#azure)  
 b. [Veröffentlichen der Website in einem Webhostingunternehmen](#host)
3. [Die Live Site wird aktualisiert: Wiederveröffentlichung](#update)

<a id="defaultpage"></a>
## <a name="setting-up-the-default-page"></a>Die Standardseite einrichten

Wenn ein Benutzer auf die Basisadresse für Ihre Website navigiert, wird die Standardseite für die Website, die dem Benutzer angezeigt. Wenn beispielsweise *default. htm* als Standardseite für die Website auf `www.contoso.com`festgelegt ist, ist die Navigation zu `www.contoso.com` identisch mit der Navigation zu `www.contoso.com/Default.htm`.

Aktuell verwendet Ihre Website **default. cshtml** als Standardseite. Diese Seite ist für die voreingestellte Seite in Ordnung, aber in diesem Tutorial nicht hinzugefügt haben alle Inhalte der Seite, damit sie eine leere Seite angezeigt wird. Öffnen Sie Default.cshtml, und Ersetzen Sie den Inhalt durch den folgenden Code.

[!code-cshtml[Main](publishing/samples/sample1.cshtml)]

Nachdem Sie Ihre Website für die Veröffentlichung bereit ist. Zunächst sehen Sie wie die Website in Azure bereitstellen, und klicken Sie dann in einem Webhostinganbieter bereitgestellt. Beide funktionieren Option für Ihre Website, und Sie müssen nur eine der Optionen für die Bereitstellung ausführen.

<a id="azure"></a>
## <a name="publishing-your-site-to-microsoft-azure"></a>Veröffentlichen Sie Ihre Website in Microsoft Azure

In diesem Tutorial wird zuerst Ihre Website in Microsoft Azure bereitstellen, veranschaulichen. Anmeldung mit einem Microsoft-Konto, können Sie bis zu 10 kostenlose Websites in Azure erstellen. Diese kostenlosen Websites bieten eine praktische Möglichkeit zum Testen von Ihrer Websites. Sie können diese Beispielwebsite, später, um zu vermeiden, verwenden all Ihre kostenlosen Websites jederzeit löschen. Sie können ein kostenloses Testkonto in wenigen Minuten erstellen. Ausführliche Informationen finden Sie unter [Einen Monat kostenlos testen](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

Klicken Sie im Menüband webmatrix auf die Schaltfläche **veröffentlichen** .

!['Publish' Schaltfläche im Menüband von WebMatrix](publishing/_static/image1.png)

Das Dialogfeld **Website veröffentlichen** wird angezeigt. Wenn Sie sich nicht bei Ihrem Microsoft-Konto angemeldet haben, enthält das Dialogfeld den Link " **Get Started with Azure** ". Klicken Sie auf diesen Link.

![Ihre Website veröffentlichen](publishing/_static/image2.png)

Wenn Sie sich nicht um ein Microsoft-Konto angemeldet haben, erhalten Sie die Möglichkeit, melden Sie sich erneut. Sie müssen ein Microsoft-Konto melden Sie sich auf Ihre Website in Azure zu veröffentlichen.

![Anmelden](publishing/_static/image3.png)

Nach der Anmeldung bei Ihrem Azure-Konto, enthält das Dialogfeld Links zum Erstellen einer neuen Website in Azure oder eine Verbindung mit einer Ihrer vorhandenen Websites in Azure.

![Neue Website erstellen](publishing/_static/image4.png)

Wählen Sie **neue Website erstellen**aus.

Wenn Sie Ihr Projekt mit dem Namen " **webpgesmovies**" benannt haben, lautet der Standardname für Ihre Website " **webpagesmovies.azurewebsites.net**". Dieser Standardname ist wahrscheinlich nicht verfügbar ist, wie durch das rote Ausrufezeichen angezeigt.

![Name der Standard-Website](publishing/_static/image5.png)

Ändern Sie den Standortnamen in etwas, das verfügbar ist, und wählen Sie einen Speicherort, der Nähe Ihres eigenen Standorts ist.

![geänderte Standortname](publishing/_static/image6.png)

Klicken Sie auf **OK**.

WebMatrix Performss einen Test, um zu bestimmen, ob der Server mit Ihrer Website kompatibel ist.

![Testen der Kompatibilität](publishing/_static/image7.png)

Wählen Sie **Weiter**.

Die Ergebnisse von den Kompatibilitätstest werden angezeigt.

![Ergebnis der Kompatibilität](publishing/_static/image8.png)

Wählen Sie **Weiter**.

WebMatrix zeigt die Dateien und Datenbanken, die auf der Website veröffentlicht werden soll. Da dies beim ersten Sie die Website veröffentlichen ist, werden alle Dateien aufgeführt. Deaktivieren Sie eine Datei, die nicht veröffentlicht werden. In nachfolgenden Veröffentlichungen werden nur die geänderten Dateien angezeigt. Weitere Informationen finden Sie [unter Aktualisieren der Live Website: wieder veröffentlichen](#update).

![Vorschau veröffentlichen](publishing/_static/image9.png)

Wählen Sie **Weiter**.

Nachdem die Website in Azure bereitgestellt wurde, wird eine Meldung angezeigt, der angibt, dass die Bereitstellung abgeschlossen ist.

![Veröffentlichung abgeschlossen](publishing/_static/image10.png)

Eine Website und Datenbank in Azure veröffentlicht wurden, und sind jetzt frei verfügbar. Klicken Sie auf den Link in die Meldung, die Veröffentlichung abgeschlossen ist, und sehen Sie nun Ihrer bereitgestellten Website. Sie oder ein Benutzer mit Zugriff auf das Internet hinzufügen oder Ändern von Einträgen in der Datenbank.

![](publishing/_static/image11.png)

<a id="host"></a>
## <a name="publishing-your-site-to-a-web-hosting-company"></a>Veröffentlichen Sie Ihre Website in ein Webhostingunternehmen

Wenn Sie nicht in Azure veröffentlichen möchten, können Sie stattdessen Ihre Website auf einen Webhostinganbieter veröffentlichen.

Klicken Sie auf den Link **Webhosting suchen** .

![Schaltfläche "Webhosting suchen", "im Dialogfeld" Veröffentlichungseinstellungen "](publishing/_static/image12.png)

Sie gelangen zu einer Seite auf der Microsoft-Website, in der Hostinganbieter aufgeführt, die ASP.NET unterstützen.

![Auf Microsoft-Website, in der Hostinganbieter aufgeführt.](publishing/_static/image13.png)

Natürlich kann es schwierig sein, wissen nun genau welche hostingfeatures langfristig erforderlich sein können. Hier sind einige Dinge zu bedenken:

- Für die Zwecke des Standorts WebPagesMovies müssen Sie ein separates Add-on für SQL Server verfügen, die oft zusätzliche Kosten. Auf der Website verwenden SQL Server Compact Edition, Sie die ist in sich geschlossen. Allerdings benötigen Sie möglicherweise SQL Server-Zugriff für einige Aufgaben für die zukünftige-Website, die Sie ausführen. Wenn Sie, dass Sie möglicherweise glauben, stellen Sie sicher, dass Sie SQL Server-Funktion später hinzufügen können.
- Überprüfen Sie, ob der Hostinganbieter der Web Deploy-publishing-Protokoll unterstützt. Sie veröffentlichen, indem Sie mithilfe von FTP-Protokoll, aber es ist einfacher, Web Deploy verwendet werden.

Einige Websites bieten einen kostenlosen Testzeitraum. Eine kostenlose Testversion ist eine gute Möglichkeit zum Testen der Veröffentlichung, und hosten, während Sie weiterhin mit WebMatrix und ASP.NET Web Pages experimentieren können.

Wählen Sie eine, die Ihnen gefallen. Für dieses Tutorial ausgewählt wir DiscountASP.NET, weil während wir das Tutorial erstellt haben, dieses Unternehmen verwendet eine heraufstufung haben, mit die Benutzer eine Website, die einige Monate lang kostenlos hosten können.

> [!NOTE]
> Unsere Wahl eines Hostinganbieters für dieses Tutorial sollten nicht als Empfehlung der jeweiligen Unternehmen über andere interpretiert werden. Aber wir hatten, um eine Abbildung auszuwählen, und DiscountASP.NET ist eines der vielen Unternehmen, die ASP.NET Web Pages und Web Deploy-Protokoll für die Veröffentlichung unterstützt.

In der Regel nach dem Sie mit dem hosting-Anbieter registriert haben, sendet des Unternehmens Sie eine e-Mail, die einen Benutzernamen und Kennwort, die URL der Web-Server und So weiter enthält. Wenn der Hostinganbieter Web Deploy-Protokoll unterstützt, können sie senden Sie eine Datei mit veröffentlichungseinstellungen oder können Sie eine herunterladen. Eine Datei mit veröffentlichungseinstellungen vereinfacht den Prozess für Sie.

Wenn Sie sich angemeldet haben und bereit für die Veröffentlichung sind, klicken Sie im webmatrix-Menüband auf die Schaltfläche **veröffentlichen** . Das Dialogfeld **Veröffentlichungs Einstellungen** wird angezeigt.

Wenn Sie vom Hostinganbieter eine Datei mit Veröffentlichungs Einstellungen gesendet haben, klicken Sie auf den Link **Veröffentlichungs Einstellungen importieren** , und importieren Sie die Datei. Wenn Sie nicht über eine Datei mit veröffentlichungseinstellungen verfügen, geben Sie in den Feldern, unter Verwendung der Werte, die der Hostinganbieter Sie per e-Mail gesendet. Das Dialogfeld **Veröffentlichungs Einstellungen** könnte wie folgt aussehen:

![Veröffentlichungseinstellungen ausgefüllt, die im Dialogfeld "Veröffentlichungseinstellungen"](publishing/_static/image14.png)

Klicken Sie auf **Verbindung**überprüfen. Wenn alles in Ordnung ist, wird das Dialogfeld **erfolgreich verbunden**, was bedeutet, dass es mit dem Server des hostinganbieters kommunizieren kann.

![Nachricht erfolgreich, wenn veröffentlichen Einstellungen richtig sind.](publishing/_static/image15.png)

Wenn ein Problem vorliegt, führt WebMatrix empfiehlt es sich, Ihnen mitteilen, was das Problem ist:

![Fehlermeldung, wenn ein Problem mit den veröffentlichungseinstellungen](publishing/_static/image16.png)

Klicken Sie auf **Save**, um Ihre Einstellungen zu speichern. WebMatrix bietet zum Ausführen eines Tests aus, um sicherzustellen, dass er ordnungsgemäß mit der hosting-Site kommunizieren kann:

![Nachricht, die zum Ausführen eines Tests des Veröffentlichungsvorgangs Angebot](publishing/_static/image17.png)

Klicken Sie auf **Ja**. WebMatrix lädt einige Beispieldateien an den Hostinganbieter ein. Wenn das Testen der Kompatibilität von fertig ist, meldet WebMatrix die Ergebnisse an:

![Testergebnisse veröffentlichen](publishing/_static/image18.png)

Wenn Sie bereit sind, klicken Sie auf " **weiter** ", um den Veröffentlichungsprozess für "Real" zu starten. WebMatrix ermittelt, welche Dateien befinden sich in Ihre Website und befinden sich bereits auf dem Host (momentan keine) und bietet Ihnen eine Vorschau des Veröffentlichungsprozesses:

![Vorschau, welche Dateien, die der Veröffentlichungsprozess hochladen](publishing/_static/image19.png)

Die Liste der zu veröffentlichenden Dateien umfasst die Webseiten, die Sie erstellt haben, z. b. " *Movies. cshtml*". Die Liste enthält auch die Dateien für Hilfsmethoden, die Sie installiert haben, die Dateien in SQL Server Compact Edition für Ihre Datenbank ausführen und so weiter. Daher der ersten Veröffentlichungsprozess können beträchtlich sein.

Klicken Sie auf **Weiter**. WebMatrix kopiert die Dateien auf der hosting-Anbieter-Server. Wenn dies abgeschlossen ist, werden die Ergebnisse in der Statusleiste gemeldet:

![Meldung in der Statusleiste, wenn der Veröffentlichungsprozess erfolgreich abgeschlossen wurde](publishing/_static/image20.png)

Um Ihre live-Website anzuzeigen, klicken Sie auf den Link in der Statusleiste angezeigt. Fügen Sie der URL *Filme* hinzu, und Sie sehen die Datei *Movies. cshtml* , die Sie erstellt haben:

![Der live-Website mit der Seite "Movies"](publishing/_static/image21.png)

<a id="update"></a>
## <a name="updating-the-live-site-republishing"></a>Aktualisieren der Live-Websites: Erneutes Veröffentlichen

Nachdem Sie Ihre Website veröffentlicht haben (entweder in Azure oder in einem Webhostingunternehmen), gibt es zwei Kopien davon &mdash; die Version auf Ihrem Computer und die Version des Dienstanbieters. Sie sollten das Entwickeln des Standorts fortsetzen (Wenn nichts anderes, als Teil der nächsten Tutorial). Wenn Sie dies tun, müssen Sie Ihre Website erneut zu veröffentlichen, um Änderungen an den Dienstanbieter auf Ihrem Computer zu kopieren. Der Veröffentlichungsprozess in WebMatrix kann bestimmen, welche Dateien auf Ihrer Website geändert haben und nur die Dateien veröffentlichen.

Um zu sehen, wie die Wiederveröffentlichung funktioniert, öffnen Sie die Website *Movies. cshtml* , nehmen Sie einige kleine Änderungen vor, und speichern Sie die Datei. Ändern Sie beispielsweise den Titel in `Movies - Updated`.

Klicken Sie im Menüband auf die Schaltfläche **veröffentlichen** . WebMatrix wird bestimmt, was geändert wird, und zeigt eine Vorschau der Dateien, die sie veröffentlicht werden sollen.

![Das Dialogfeld "Veröffentlichen" mit der geänderten bereit Dateien, für die wiederveröffentlichung](publishing/_static/image22.png)

> [!IMPORTANT] 
> 
> Standardmäßig wird Ihre Datenbank (*sdf* -Datei) von webmatrix nur beim ersten Veröffentlichen der Website veröffentlicht. Sobald Ihre Website veröffentlicht wird, und Personen mit der Website interagieren, hat die Datenbank auf der live-Website in der Regel echte Daten von der Website. Sie müssen sehr vorsichtig sein, die Live Datenbank nicht mit der *. sdf* -Datei zu überschreiben, die sich auf Ihrem Computer befindet, die in der Regel nur Testdaten enthält. Aus diesem Grund werden Sie sehen, dass die **Veröffentlichungs Veröffentlichung alle Remote Datenbanken überschreibt**und warum das Kontrollkästchen für *Webweb. sdf* standardmäßig deaktiviert ist.

Klicken Sie auf **Weiter**. WebMatrix die geänderten Dateien veröffentlicht, und es wird gezeigt, eine Erfolgsmeldung, wie es beim ersten, die Sie veröffentlicht.

Wechseln Sie zu der live-Website (Sie können den Link in der Success-Nachricht klicken, wenn es immer noch angezeigt werden) und stellen Sie sicher, dass die Änderung veröffentlicht wurde.

> [!TIP] 
> 
> **Remote Bearbeitung von Dateien**
> 
> Als Alternative zum Ändern Ihrer Website und dann erneut veröffentlichen können Sie die remote-Dateien direkt in WebMatrix bearbeiten. Öffnen Sie eine Datei, die auf den Dienstanbieter ist, und WebMatrix lädt eine Kopie der Datei bearbeiten, in diesem Szenario. Jedes Mal, wenn Sie die Datei speichern, werden die Änderungen von WebMatrix an den Standort gesendet.
> 
> Remote bearbeiten, ist eine einfache Möglichkeit, um Ihre live-Website zu ändern. Allerdings werden nicht auf diese Weise vorgenommene Änderungen mit den Dateien an Ihrem lokalen Standort synchronisiert. Um die lokalen Dateien mit dem Remotestandort zu synchronisieren, können Sie die Remotedateien herunterzuladen. Dieser Prozess funktioniert ähnlich wie die Veröffentlichung, außer in umgekehrter Reihenfolge.
> 
> Es wird nicht mehr über die Remote-Bearbeitung und Remote-Download-Funktionen von WebMatrix hier beschrieben. Sie sind sehr nützlich, wenn mehrere Personen am selben Standort auf verschiedenen Computern zu arbeiten müssen. Weitere Informationen finden Sie unter [veröffentlichen und Bearbeiten einer Remote Website mit webmatrix 2 Beta](https://go.microsoft.com/fwlink/?LinkId=251591).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [ASP.net webmatrix ASP.net Web Pages Forum](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages), ein guter Ort, um Fragen zu stellen und Antworten zu erhalten.

> [!div class="step-by-step"]
> [Previous](layouts.md)
