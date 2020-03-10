---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-vb
title: Bereitstellen Ihrer Website mithilfe von Visual Studio (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Visual Studio enthält Tools zum Bereitstellen einer Website. Weitere Informationen zu diesen Tools finden Sie in diesem Tutorial.
ms.author: riande
ms.date: 04/01/2009
ms.assetid: 977105f3-7987-4e50-8be7-afb53b4ca28a
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-vb
msc.type: authoredcontent
ms.openlocfilehash: 6c71e36a8a434947882cc767cd2f903ff6e8d422
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78474621"
---
# <a name="deploying-your-site-using-visual-studio-vb"></a>Bereitstellen einer Website mithilfe von Visual Studio (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_04_VB.zip) oder [PDF herunterladen](https://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial04_DeployingViaVS_vb.pdf)

> Visual Studio enthält Tools zum Bereitstellen einer Website. Weitere Informationen zu diesen Tools finden Sie in diesem Tutorial.

## <a name="introduction"></a>Einführung

Im vorherigen Tutorial wurde erläutert, wie eine einfache ASP.NET-Webanwendung für einen webhostinstanbieter bereitgestellt wird. Insbesondere wurde in diesem Tutorial gezeigt, wie ein FTP-Client wie FileZilla verwendet wird, um die erforderlichen Dateien aus der Entwicklungsumgebung in die Produktionsumgebung zu übertragen. Visual Studio bietet auch integrierte Tools, die die Bereitstellung für einen Webhost Anbieter vereinfachen. In diesem Tutorial werden zwei dieser Tools untersucht: das Tool zum Kopieren von Websites, mit dem Sie Dateien über FTP oder FrontPage-Servererweiterungen auf einen Remoteweb Server verschieben können; und das Veröffentlichungs Tool, das die gesamte Website an einen angegebenen Speicherort kopiert.

> [!NOTE]
> Weitere Tools für die Bereitstellung, die von Visual Studio angeboten werden, sind [Websetup Projekte](https://msdn.microsoft.com/library/wx3b589t.aspx) und Add-Ins für [Webbereitstellungs Projekte](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) . Websetup Projekte Verpacken die Inhalte und Konfigurationsinformationen einer Website in eine einzelne MSI-Datei. Diese Option ist besonders nützlich für Websites, die innerhalb eines Intranets bereitgestellt werden, oder für Unternehmen, die eine vorgefertigte Webanwendung verkaufen, die Kunden auf Ihren eigenen Webservern installieren. Das-Add-in für Webbereitstellungs Projekte ist ein Visual Studio-Add-in, das die Angabe von Konfigurations unterschieden zwischen Builds für Entwicklungsumgebungen und Produktionsumgebungen vereinfacht. Websetup Projekte werden in dieser tutorialreihe nicht erläutert. Webbereitstellungs Projekte sind im Tutorial [*allgemeine Konfigurations Unterschiede zwischen Entwicklungs-und Produktions*](common-configuration-differences-between-development-and-production-vb.md) Umgebungen zusammengefasst.

## <a name="deploying-your-site-using-the-copy-web-site-tool"></a>Bereitstellen Ihrer Website mit dem Tool "Website kopieren"

Das Tool zum Kopieren der Website von Visual Studio ähnelt der Funktionalität eines eigenständigen FTP-Clients. Kurz gesagt: das Tool zum Kopieren von Websites ermöglicht Ihnen das Herstellen einer Verbindung mit einer Remote Website über FTP oder FrontPage-Servererweiterungen. Ähnlich wie bei der Benutzeroberfläche von FileZilla besteht die Benutzeroberfläche zum Kopieren von Websites aus zwei Bereichen: im linken Bereich werden die lokalen Dateien aufgelistet, während der Rechte Bereich diese Dateien auf dem Zielserver auflistet.

> [!NOTE]
> Das Tool zum Kopieren der Website ist nur für Website Projekte verfügbar. Visual Studio bietet dieses Tool, wenn Sie mit einem Webanwendungs Projekt arbeiten.

Werfen wir einen Blick auf die Verwendung des Tools zum Kopieren der Website, um die Book Review-Anwendung in der Produktion zu veröffentlichen. Da das Tool zum Kopieren von Websites nur mit Projekten funktioniert, die das Website Projekt Modell verwenden, können wir nur die Verwendung dieses Tools mit dem bookreviewwaysp-Projekt untersuchen. Öffnen Sie das Projekt.

Starten Sie das Projekt "Website Tool kopieren", indem Sie in der Projektmappen-Explorer auf das Symbol "Website kopieren" klicken (dieses Symbol ist in Abbildung 1 dargestellt). Alternativ können Sie die Option Website kopieren im Menü Website auswählen. Bei beiden Ansätzen wird die in Abbildung 1 gezeigte Benutzeroberfläche zum Kopieren von Websites gestartet. nur der linke Bereich in Abbildung 1 wird aufgefüllt, weil noch keine Verbindung mit einem Remote Server hergestellt werden kann.

[![die Benutzeroberfläche des Copy-Website Tools in zwei Bereiche unterteilt ist.](deploying-your-site-using-visual-studio-vb/_static/image2.png)](deploying-your-site-using-visual-studio-vb/_static/image1.png)

**Abbildung 1**: die Benutzeroberfläche des Tools zum Kopieren der Website ist in zwei Bereiche unterteilt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](deploying-your-site-using-visual-studio-vb/_static/image3.png))

Zum Bereitstellen unserer Website müssen Sie zuerst eine Verbindung mit dem webhostinstanbieter herstellen. Klicken Sie oben auf der Benutzeroberfläche der Kopier Website auf die Schaltfläche verbinden. Dadurch wird das Dialogfeld Website öffnen angezeigt, das in Abbildung 2 angezeigt wird.

Sie können eine Verbindung mit der Zielwebsite herstellen, indem Sie eine der vier Optionen von Links auswählen:

- **Datei System** : Wählen Sie diese Option aus, um Ihren Standort in einem Ordner oder einer Netzwerkfreigabe bereitzustellen, auf den der Computer Zugriff hat
- **Lokale IIS** : Verwenden Sie diese Option, um den Standort auf dem IIS-Webserver bereitzustellen, der auf Ihrem Computer installiert ist.
- **FTP-Site** : Stellen Sie eine Verbindung mit einer Remote Website mithilfe von FTP her.
- **Remote Standort** : Stellen Sie eine Verbindung mit einer Remote Website mithilfe von FrontPage-Servererweiterungen her.

Die meisten Webhostinganbieter unterstützen FTP, aber weniger Unterstützung für FrontPage-Server Erweiterungen. Aus diesem Grund habe ich die Option FTP-Site ausgewählt und die Verbindungsinformationen eingegeben, wie in Abbildung 2 dargestellt.

[![geben Sie die Ziel Website an.](deploying-your-site-using-visual-studio-vb/_static/image5.png)](deploying-your-site-using-visual-studio-vb/_static/image4.png)

**Abbildung 2**: Angeben der Ziel Website ([Klicken Sie, um das Bild in voller Größe anzuzeigen](deploying-your-site-using-visual-studio-vb/_static/image6.png))

Nachdem Sie die Verbindung hergestellt haben, lädt das Tool zum Kopieren von Websites die Dateien an der Remote Website im rechten Bereich und gibt den Status der einzelnen Dateien an: neu, gelöscht, geändert oder unverändert. Sie können eine Datei vom lokalen Standort auf den Remote Standort kopieren oder umgekehrt.

Fügen Sie dem Projekt bookreviewws eine neue Seite hinzu, und stellen Sie es anschließend bereit, damit das Tool Website kopieren in Aktion angezeigt wird. Erstellen Sie eine neue ASP.NET-Seite in Visual Studio im Stammverzeichnis mit dem Namen `Privacy.aspx`. Legen Sie auf der Seite die Master Seite `Site.master`, und fügen Sie dieser Seite die Datenschutzrichtlinie Ihrer Website hinzu. Abbildung 3 zeigt Visual Studio, nachdem diese Seite erstellt wurde.

[![eine neue Seite mit dem Namen &lt;Code&gt;Privacy. aspx&lt;/Code&gt; dem Stamm Ordner der Website hinzufügen.](deploying-your-site-using-visual-studio-vb/_static/image8.png)](deploying-your-site-using-visual-studio-vb/_static/image7.png)

**Abbildung 3**: Hinzufügen einer neuen Seite mit dem Namen "`Privacy.aspx`" zum Stamm Ordner der Website ([Klicken Sie, um das Bild in voller Größe anzuzeigen](deploying-your-site-using-visual-studio-vb/_static/image9.png))

Kehren Sie als nächstes zur Website Benutzeroberfläche kopieren zurück. Wie in Abbildung 4 gezeigt, enthält der linke Bereich nun die neuen Dateien `Policy.aspx` und `Policy.aspx.vb`. Darüber hinaus sind diese Dateien mit einem Pfeilsymbol und dem Status neu gekennzeichnet, was darauf hinweist, dass Sie auf dem lokalen Standort, aber nicht auf der Remote Site vorhanden sind.

[![das Tool zum Kopieren der Website den neuen &lt;Code&gt;Privacy. aspx&lt;/Code&gt; Seite im linken Bereich enthält.](deploying-your-site-using-visual-studio-vb/_static/image11.png)](deploying-your-site-using-visual-studio-vb/_static/image10.png)

**Abbildung 4**: das Tool zum Kopieren der Website enthält die neue `Privacy.aspx` Seite im linken Bereich ([Klicken Sie, um das Bild in voller Größe anzuzeigen](deploying-your-site-using-visual-studio-vb/_static/image12.png))

Wählen Sie zum Bereitstellen der neuen Dateien diese aus, und klicken Sie auf das Pfeilsymbol, um Sie an den Remote Standort zu übertragen. Nachdem die Übertragung abgeschlossen ist, sind die `Policy.aspx`-und `Policy.aspx.vb` Dateien auf den lokalen und Remote Standorten mit dem Status unverändert vorhanden.

Das Tool zum Kopieren von Websites zeigt zusammen mit dem auflisten neuer Dateien alle Dateien an, die sich zwischen den lokalen und den Remote Standorten unterscheiden. Um dies in Aktion zu sehen, kehren Sie zur Seite `Privacy.aspx` zurück, und fügen Sie der Datenschutzrichtlinie einige weitere Wörter hinzu. Speichern Sie die Seite, und kehren Sie dann zum Tool zum Kopieren der Website zurück. Wie in Abbildung 5 dargestellt, hat die Seite `Privacy.aspx` im linken Bereich den Status geändert und zeigt an, dass Sie nicht mit dem Remote Standort synchronisiert ist.

[![das Tool zum Kopieren von Websites zeigt an, dass der &lt;Code&gt;Privacy. aspx&lt;/Code&gt; Seite geändert wurde.](deploying-your-site-using-visual-studio-vb/_static/image14.png)](deploying-your-site-using-visual-studio-vb/_static/image13.png)

**Abbildung 5**: das Tool "Website kopieren" zeigt an, dass die Seite "`Privacy.aspx`" geändert wurde ([Klicken Sie, um das Bild in voller Größe anzuzeigen](deploying-your-site-using-visual-studio-vb/_static/image15.png))

Das Tool Website kopieren gibt auch an, ob eine Datei seit dem letzten Kopiervorgang gelöscht wurde. Löschen Sie die `Privacy.aspx` aus dem lokalen Projekt, und aktualisieren Sie das Tool zum Kopieren von Websites. Die `Privacy.aspx`-und `Privacy.aspx.vb` Dateien bleiben im linken Bereich aufgelistet, haben aber den Status "gelöscht", der anzeigt, dass Sie seit dem letzten Kopiervorgang entfernt wurden.

## <a name="publishing-a-web-application"></a>Veröffentlichen einer Webanwendung

Eine weitere Möglichkeit zum Bereitstellen Ihrer Webanwendung in Visual Studio ist die Verwendung der Option veröffentlichen, auf die Sie über das Menü Build zugreifen können. Die Option veröffentlichen kompiliert die Anwendung explizit und kopiert dann alle erforderlichen Dateien bis zu der angegebenen Remote Site. Wie wir sehen werden, ist die Option "veröffentlichen" eine stumme als das Tool zum Kopieren von Websites. Während Sie mit dem Tool zum Kopieren von Websites die Dateien auf den lokalen und Remote Standorten untersuchen können und es Ihnen ermöglicht, einzelne Dateien nach Bedarf hoch-oder herunterzuladen, stellt die Option veröffentlichen die gesamte Webanwendung bereit.

Zusätzlich zum Kopieren aller benötigten Dateien auf die angegebene Remote Site kompiliert die Option "Publish" auch die Anwendung explizit. Da Webanwendungs Projekte explizit kompiliert werden müssen, sollte die Option veröffentlichen für Webanwendungs Projekte zur Verfügung stehen. Das ist vielleicht überraschend, wenn die Option veröffentlichen auch für Website Projekte verfügbar ist. Wie im Tutorial [*ermitteln, welche Dateien bereitgestellt werden müssen*](determining-what-files-need-to-be-deployed-vb.md) , können Website Projekte explizit durch einen Prozess kompiliert werden, der als *Vorkompilierung*bezeichnet wird. Dieses Tutorial konzentriert sich auf die Verwendung der Veröffentlichungs Option mit Webanwendungs Projekten. in einem zukünftigen Tutorial wird die Vorkompilierung untersucht. zu diesem Zeitpunkt wird die Verwendung der Veröffentlichungs Option mit Website Projekten erläutert.

> [!NOTE]
> Obwohl die Option veröffentlichen sowohl für Website Projekte als auch für Webanwendungs Projekte in Visual Studio verfügbar ist, bietet Visual Web Developer nur die Option veröffentlichen für Webanwendungs Projekte.

Sehen wir uns die Bereitstellung der Anwendung "Book Reviews" mithilfe der Option "veröffentlichen" an. Beginnen Sie, indem Sie bookreviewswap (das Webanwendungs Projekt) in Visual Studio öffnen. Wählen Sie im Menü "veröffentlichen" das Projekt Build bookreviewswap aus. Dadurch wird ein Dialogfeld geöffnet, in dem der Ziel Speicherort, unter anderem Konfigurationsoptionen, angezeigt wird (siehe Abbildung 6). Ähnlich wie beim Copy-Website Tool können Sie einen Speicherort eingeben, der auf einen lokalen Ordner, eine lokale Website auf IIS, eine Remote Website, die FrontPage-Servererweiterungen unterstützt, oder eine FTP-Server Adresse verweist. Sie können auswählen, ob Sie die Dateien auf dem Remoteweb Server durch die bereitgestellten Dateien ersetzen oder den gesamten Inhalt auf dem Remote Standort vor der Veröffentlichung löschen möchten. Sie können auch angeben, ob kopiert werden soll:

- Nur die Dateien im Projekt, die zum Ausführen der Anwendung erforderlich sind, sodass der nicht benötigte Quellcode und die projektbezogenen Dateien ausgelassen werden.
- Alle Projektdateien, einschließlich der Quell Code Dateien und Visual Studio-Projektdateien wie die Projektmappendatei.
- Alle Dateien im Quell Projektordner, von denen alle Dateien im Quell Projektordner kopiert werden, unabhängig davon, ob Sie im Projekt enthalten sind.

Es gibt auch eine Option zum Hochladen der Inhalte des Ordners "`App_Data`".

[![geben Sie die Ziel Website an.](deploying-your-site-using-visual-studio-vb/_static/image17.png)](deploying-your-site-using-visual-studio-vb/_static/image16.png)

**Abbildung 6**: Angeben der Ziel Website ([Klicken Sie, um das Bild in voller Größe anzuzeigen](deploying-your-site-using-visual-studio-vb/_static/image18.png))

Bei der Book Review-Anwendung enthält die Remote Site die Dateien, die beim Kopieren des bookreviewswap-Projekts über das Tool zum Kopieren von Websites bereitgestellt werden. Aus diesem Grund wird die Option veröffentlichen gestartet, indem alle vorhandenen Inhalte gelöscht werden. Kopieren Sie auch die erforderlichen Dateien, anstatt die Produktionsumgebung mit nicht benötigter Quell Code-und Projektdateien zu gruppieren. Nachdem Sie diese Optionen angegeben haben, klicken Sie auf die Schaltfläche veröffentlichen. In den nächsten Sekunden stellt Visual Studio die erforderlichen Dateien für die Zielwebsite bereit und zeigt den Fortschritt im Ausgabefenster an.

Abbildung 7 zeigt die Dateien auf der FTP-Site, nachdem der Veröffentlichungs Vorgang abgeschlossen wurde. Beachten Sie, dass nur die Markup Seiten und die erforderlichen Server-und Client seitigen Unterstützungs Dateien hochgeladen wurden.

[![nur die erforderlichen Dateien in der Produktionsumgebung veröffentlicht wurden.](deploying-your-site-using-visual-studio-vb/_static/image20.png)](deploying-your-site-using-visual-studio-vb/_static/image19.png)

**Abbildung 7**: nur die benötigten Dateien wurden in der Produktionsumgebung veröffentlicht ([Klicken Sie, um das Bild in voller Größe anzuzeigen](deploying-your-site-using-visual-studio-vb/_static/image21.png))

Die Option veröffentlichen ist ein weniger differenzierteres Tool als das Tool zum Kopieren von Websites. Während das Tool zum Kopieren von Websites es Ihnen ermöglicht, die Dateien auf den lokalen und Remote Standorten zu überprüfen und zu sehen, wie sich diese unterscheiden, bietet die Option veröffentlichen keine solche Schnittstelle. Darüber hinaus können Sie mit dem Tool zum Kopieren von Websites einmalige Änderungen vornehmen und einzelne Dateien hochladen oder löschen. Die Option "veröffentlichen" lässt eine solche fein abgestimmte Steuerung nicht zu; Stattdessen wird die *gesamte* Anwendung veröffentlicht. Dieses Verhalten hat vor-und Nachteile. Auf der Plus Seite wissen Sie, dass Sie bei Verwendung der Option "veröffentlichen" nicht vergessen, eine wichtige Datei hochzuladen. Beachten Sie jedoch, was geschieht, wenn Sie eine kleine Änderung an einer sehr großen Website vorgenommen haben: mit der Option veröffentlichen können Sie die Seite nicht aktualisieren, oder es wurden zwei Änderungen vorgenommen, die geändert wurden. stattdessen müssen Sie warten, bis die gesamte Website von Visual Studio bereitgestellt wird.

Es ist nicht ungewöhnlich, dass bestimmte Dateien vorhanden sind, deren Inhalt sich zwischen der Produktionsumgebung und der Entwicklungsumgebung unterscheidet. Ein wichtiges Beispiel ist die Konfigurationsdatei der Anwendung, `Web.config`. Da die Option veröffentlichen die Webanwendungs Dateien Blind kopiert, überschreibt Sie die benutzerdefinierten Konfigurationsdateien der Produktionsumgebung mit der Version in der Entwicklungsumgebung. Das nachfolgende Tutorial behandelt dieses Thema weiter und bietet Tipps zur Bereitstellung einer Webanwendung, wenn derartige Unterschiede bestehen.

## <a name="summary"></a>Zusammenfassung

Zum Bereitstellen einer Website müssen die erforderlichen Dateien aus der Entwicklungsumgebung in die Produktionsumgebung kopiert werden. Im vorherigen Tutorial wurde gezeigt, wie Dateien mithilfe eines FTP-Clients wie FileZilla übertragen werden. In diesem Tutorial wurden zwei Bereitstellungs Tools in Visual Studio untersucht: das Tool "Website kopieren" und die Option "veröffentlichen". Das Tool zum Kopieren von Websites ähnelt einem FTP-Client darin, dass es über eine zweistufige Oberfläche verfügt, die die Dateien auf dem lokalen Computer auflistet, und einen angegebenen Remote Computer, der das Hochladen oder Herunterladen von Dateien zwischen den beiden Computern vereinfacht. Die Option veröffentlichen ist ein stummeres Tool, das das Projekt explizit kompiliert und anschließend die gesamte Anwendung für das angegebene Ziel bereitstellt.

Fröhliche Programmierung!

### <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [Kopieren der Website mit dem Tool "Website kopieren"](https://msdn.microsoft.com/library/1cc82atw.aspx)
- Gewusst [wie: Bereitstellen einer Website mit dem Tool zum Kopieren von Websites](../../../videos/how-do-i/how-do-i-deploy-a-web-site-using-the-copy-web-site-tool.md) (Video)
- [Gewusst wie: Veröffentlichen von Webanwendungs Projekten](https://msdn.microsoft.com/library/aa983453.aspx)
- [Vorgehensweise: Veröffentlichen von Websites](https://msdn.microsoft.com/library/20yh9f1b.aspx)
- [Setup-und Bereitstellungs Projekte in Visual Studio](https://msdn.microsoft.com/library/wx3b589t.aspx)

> [!div class="step-by-step"]
> [Zurück](deploying-your-site-using-an-ftp-client-vb.md)
> [Weiter](common-configuration-differences-between-development-and-production-vb.md)
