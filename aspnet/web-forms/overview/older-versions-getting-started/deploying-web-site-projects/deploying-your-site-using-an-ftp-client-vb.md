---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-vb
title: Bereitstellen Ihrer Website mithilfe eines FTP-Clients (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Die einfachste Möglichkeit zum Bereitstellen einer ASP.NET-Anwendung besteht darin, die erforderlichen Dateien manuell aus der Entwicklungsumgebung in die Produktionsumgebung zu kopieren. ...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: 09279194-bcf9-4b59-a09d-c68e5926a758
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-vb
msc.type: authoredcontent
ms.openlocfilehash: 7875304c672625d8c0eaaf0fea8ef509bb801a3a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78438873"
---
# <a name="deploying-your-site-using-an-ftp-client-vb"></a>Bereitstellen einer Website mithilfe eines FTP-Clients (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_03_VB.zip) oder [PDF herunterladen](https://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial03_DeployingViaFTP_vb.pdf)

> Die einfachste Möglichkeit zum Bereitstellen einer ASP.NET-Anwendung besteht darin, die erforderlichen Dateien manuell aus der Entwicklungsumgebung in die Produktionsumgebung zu kopieren. In diesem Tutorial wird gezeigt, wie Sie einen FTP-Client verwenden, um die Dateien von Ihrem Desktop zum webhostinstanbieter zu erhalten.

## <a name="introduction"></a>Einführung

Im vorherigen Tutorial wurde eine einfache Book Review ASP.NET-Webanwendung eingeführt, die aus einer Handvoll von ASP.NET Seiten besteht, einer Master Seite, einer benutzerdefinierten Basis `Page` Klasse, einer Reihe von Bildern und drei CSS-Stylesheets. Nun können wir diese Anwendung für einen Webhost Anbieter bereitstellen. zu diesem Zeitpunkt ist die Anwendung für alle Benutzer mit Internet Verbindung zugänglich.

Aus unseren Diskussionen im Tutorial [*ermitteln, welche Dateien bereitgestellt werden müssen*](determining-what-files-need-to-be-deployed-vb.md) , wissen wir, welche Dateien in den Webhost Anbieter kopiert werden müssen. (Denken Sie daran, welche Dateien kopiert werden, hängt davon ab, ob die Anwendung explizit oder automatisch kompiliert wird.) Aber wie werden die Dateien aus der Entwicklungsumgebung (unserem Desktop) bis zur Produktionsumgebung (der vom Webhost Anbieter verwaltete Webserver) in der Produktionsumgebung angezeigt? [ **F** Ile **T** ransfer **P** rotocol (FTP)](http://en.wikipedia.org/wiki/File_Transfer_Protocol) ist ein häufig verwendetes Protokoll zum Kopieren von Dateien von einem Computer auf einen anderen über ein Netzwerk. Eine weitere Option ist FrontPage-Servererweiterungen (FPSE). Dieses Tutorial konzentriert sich auf die Verwendung eigenständiger FTP-Client Software, um die erforderlichen Dateien aus der Entwicklungsumgebung in der Produktionsumgebung bereitzustellen.

> [!NOTE]
> Visual Studio enthält Tools zum Veröffentlichen von Websites über FTP. Diese Tools und die Tools, die FPSE verwenden, werden im nächsten Tutorial behandelt.

Zum Kopieren der Dateien mithilfe von FTP benötigen wir einen *FTP-Client* in der Entwicklungsumgebung. Bei einem FTP-Client handelt es sich um eine Anwendung, die zum Kopieren von Dateien von dem Computer, auf dem ein *FTP-Server*ausgeführt wird, entworfen wurde. (Wenn der Webhostanbieter Dateiübertragungen über FTP unterstützt, wird auf den Webservern ein FTP-Server ausgeführt.) Es stehen eine Reihe von FTP-Client Anwendungen zur Verfügung. Ihr Webbrowser kann sich sogar als FTP-Client verdoppeln. Mein bevorzugter FTP-Client, der für dieses Tutorial verwendet wird, ist [FileZilla](http://filezilla-project.org/), ein kostenloser Open-Source-FTP-Client, der für Windows, Linux und Macs verfügbar ist. Jeder FTP-Client funktioniert jedoch, sodass Sie den Client, mit dem Sie am ehesten vertraut sind, verwenden können.

Wenn Sie die folgenden Schritte ausführen, müssen Sie ein Konto mit einem Webhost Anbieter erstellen, bevor Sie dieses Tutorial oder nachfolgende Schritte ausführen können. Wie bereits im vorherigen Tutorial erwähnt, gibt es eine Reihe von Webhostinganbieter-Unternehmen, die ein breites Spektrum an Preisen, Features und Dienst Qualität aufweisen. Für diese tutorialreihe verwende ich " [Discount ASP.net](http://discountasp.net) " als webhostinstanbieter, aber Sie können auch jeden webhostinbieterbieter befolgen, sofern diese die ASP.NET-Version unterstützen, in der Ihre Website entwickelt wurde. (Diese Tutorials wurden mit ASP.NET 3,5 erstellt.) Da wir die Dateien in diesem Tutorial mithilfe von FTP in den Webhost Anbieter kopieren, ist es zwingend erforderlich, dass der Webhostanbieter FTP-Zugriff auf die Webserver unterstützt. Praktisch alle Webhostinganbieter bieten dieses Feature, aber Sie sollten vor der Registrierung eine doppelte Überprüfung ausführen.

## <a name="deploying-the-book-review-web-application-project"></a>Bereitstellen des Webanwendungs Projekts "Book Review"

Beachten Sie, dass es zwei Versionen der Book Review-Webanwendung gibt: eine mit dem Webanwendungs Projekt Modell (bookreviewswap) und die andere mit dem Website Projekt Modell (bookreviewswsp). Der Projekttyp wirkt sich darauf aus, ob die Site automatisch oder explizit kompiliert wird, und dass das Kompilierungs Modell festlegt, welche Dateien bereitgestellt werden müssen. Folglich wird die Bereitstellung von bookreviewswap-und bookreviewswsp-Projekten separat untersucht, beginnend mit bookreviewswap. Nehmen Sie sich einen Moment Zeit, um diese beiden ASP.NET-Anwendungen herunterzuladen, wenn Sie dies noch nicht getan haben.

Starten Sie das Projekt bookreviewswap, indem Sie zum Ordner "`BookReviewsWAP`" navigieren und auf die `BookReviewsWAP.sln`-Datei doppelklicken. Vor dem Bereitstellen des Projekts ist es wichtig, es zu erstellen, um sicherzustellen, dass alle Änderungen am Quellcode in der kompilierten Assembly enthalten sind. Um das Projekt zu erstellen, wechseln Sie zum Menü erstellen, und wählen Sie die Menüoption Build bookreviewswap aus. Dadurch wird der Quellcode im Projekt in eine einzelne Assembly, `BookReviewsWAP.dll`, kompiliert, die sich im Ordner `Bin` befindet.

Nun können Sie die erforderlichen Dateien bereitstellen. Starten Sie den FTP-Client, und stellen Sie eine Verbindung mit dem Webserver auf Ihrem Webhostanbieter her. (Wenn Sie sich bei einem Webhostingunternehmen registrieren, werden Sie per e-Mail darüber informiert, wie eine Verbindung mit dem FTP-Server hergestellt werden kann. Dies umfasst die Adresse des FTP-Servers sowie einen Benutzernamen und ein Kennwort.)

Kopieren Sie die folgenden Dateien von Ihrem Desktop in den Stamm Ordner der Website auf Ihrem webhostinstanbieter. Wenn Sie einen FTP-Zugriff auf den Webserver des Webhost Anbieters durchlaufen, sind Sie wahrscheinlich im Stammverzeichnis der Website. Einige Webhostinganbieter verfügen jedoch über einen Unterordner mit dem Namen `www` oder `wwwroot`, der als Stamm Ordner für Ihre Website Dateien fungiert. Wenn Sie zum Schluss die Dateien per Ping pingen, müssen Sie möglicherweise die entsprechende Ordnerstruktur in der Produktionsumgebung erstellen: den Ordner "`Bin`", den Ordner "`Fiction`", den Ordner "`Images`" usw.

- `~/Default.aspx`
- `~/About.aspx`
- `~/Site.master`
- `~/Web.config`
- `~/Web.sitemap`
- Der gesamte Inhalt des Ordners "`Styles`".
- Der gesamte Inhalt des `Images` Ordners (und dessen Unterordner `BookCovers`)
- `~/Fiction/Default.aspx`
- `~/Fiction/Blaze.aspx`
- `~/Tech/Default.aspx`
- `~/Tech/CYOW.aspx`
- `~/Tech/TYASP35.aspx`
- `~/Bin/BookReviewsWAP.dll`

Abbildung 1 zeigt FileZilla, nachdem die erforderlichen Dateien kopiert wurden. FileZilla zeigt die Dateien auf dem lokalen Computer auf der linken Seite und die Dateien auf dem Remote Computer auf der rechten Seite an. Wie in Abbildung 1 gezeigt, befinden sich die ASP.net-Quell Code Dateien, z. b. `About.aspx.vb`, auf dem lokalen Computer (der Entwicklungsumgebung), wurden jedoch nicht in den Webhost Anbieter (die Produktionsumgebung) kopiert, da Code Dateien bei Verwendung der expliziten Kompilierung nicht bereitgestellt werden müssen.

> [!NOTE]
> Das vorhanden sein der Quell Code Dateien auf dem Produktionsserver wird nicht beeinträchtigt, da Sie ignoriert werden. ASP.net verbietet standardmäßig HTTP-Anforderungen an Quell Code Dateien, sodass Benutzer Ihrer Website nicht darauf zugreifen können, selbst wenn die Quell Code Dateien auf dem Produktionsserver vorhanden sind. (Das heißt, wenn ein Benutzer versucht, `http://www.yoursite.com/Default.aspx.vb` zu besuchen, wird eine Fehlerseite angezeigt, die erklärt, dass diese Dateitypen `.vb` Dateien unzulässig sind.)

[![einen FTP-Client verwenden, um die erforderlichen Dateien vom Desktop auf den Webserver auf dem webhostinstanbieter zu kopieren.](deploying-your-site-using-an-ftp-client-vb/_static/image2.png)](deploying-your-site-using-an-ftp-client-vb/_static/image1.png)

**Abbildung 1**: Verwenden eines FTP-Clients zum Kopieren der erforderlichen Dateien vom Desktop auf den Webserver auf dem webhostinstanbieter ([Klicken Sie, um das Bild in voller Größe anzuzeigen](deploying-your-site-using-an-ftp-client-vb/_static/image3.png))

Nehmen Sie nach der Bereitstellung Ihres Standorts einen Moment Zeit, um den Standort zu testen. Wenn Sie einen Domänen Namen erworben und die DNS-Einstellungen ordnungsgemäß konfiguriert haben, können Sie Ihre Website aufrufen, indem Sie Ihren Domänen Namen eingeben. Alternativ sollte der Webhostanbieter Ihnen eine URL für Ihre Website bereitgestellt haben, die in etwa wie *Accountname*aussieht. *Webhostprovider*. com oder *Webhostprovider*. com/*Accountname*. Beispielsweise lautet die URL für mein Konto unter dem Rabatt ASP.net: `http://httpruntime.web703.discountasp.net`.

Abbildung 2 zeigt die Website zur Bereitstellung von Buchbesprechungen. Beachten Sie, dass ich Sie in Discount ASP anzeigen kann. Netzwerkserver, `http://httpruntime.web703.discountasp.net`. Zu diesem Zeitpunkt konnte jeder Benutzer, der eine Verbindung mit dem Internet hat, meine Website anzeigen. Wie zu erwarten ist, sieht die Website wie beim Testen in der Entwicklungsumgebung aus und verhält sich genauso.

> [!NOTE]
> Wenn beim Anzeigen der Anwendung ein Fehler auftritt, nehmen Sie sich einen Moment Zeit, um sicherzustellen, dass Sie den richtigen Satz von Dateien bereitgestellt haben. Überprüfen Sie als nächstes in der Fehlermeldung, ob Hinweise zu dem Problem angezeigt werden. Danach können Sie den Helpdesk Ihres Webhost-Unternehmens einschalten oder Ihre Frage im entsprechenden Forum in den [ASP.net-Foren](https://forums.asp.net/)veröffentlichen.

[![auf die Website für die Buch Überprüfung kann jeder Benutzer mit Internet Verbindung zugreifen.](deploying-your-site-using-an-ftp-client-vb/_static/image5.png)](deploying-your-site-using-an-ftp-client-vb/_static/image4.png)

**Abbildung 2**: auf die Website zur Buch Überprüfung kann nun jeder Benutzer mit einer Internet Verbindung zugreifen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](deploying-your-site-using-an-ftp-client-vb/_static/image6.png))

## <a name="deploying-the-book-review-web-site-project"></a>Bereitstellen des Book Review-Website Projekts

Beim Bereitstellen einer ASP.NET-Anwendung, die die automatische Kompilierung verwendet, wie z. b. das Website Projekt bookreviewtauscht, ist im Ordner `Bin` keine kompilierte Assembly vorhanden. Folglich müssen die Quell Code Dateien der Webanwendung in der Produktionsumgebung bereitgestellt werden. Wir durchlaufen diesen Prozess.

Wie beim Webanwendungs Projekt ist es ratsam, zuerst die Anwendung vor der Bereitstellung zu erstellen. Während der Erstellung eines Website Projekts keine Assembly erstellt, prüft es auf der Seite auf Kompilierzeitfehler. Sie sollten diese Fehler jetzt besser finden, anstatt einen Besucher Ihrer Website für Sie zu finden!

Nachdem Sie das Projekt erfolgreich erstellt haben, verwenden Sie den FTP-Client, um die folgenden Dateien in den Stamm Ordner der Website Ihres Webhost Anbieters zu kopieren. Möglicherweise müssen Sie die entsprechende Ordnerstruktur in der Produktionsumgebung erstellen.

> [!NOTE]
> Wenn Sie das Projekt bookreviewswap bereits bereitgestellt haben, aber trotzdem versuchen möchten, das Projekt bookreviewswsp bereitzustellen, löschen Sie zuerst alle Dateien auf dem Webserver, die beim Bereitstellen von bookreviewswap hochgeladen wurden, und stellen Sie dann die Dateien für bookreviewswsp bereit.

- `~/Default.aspx`
- `~/Default.aspx.vb`
- `~/About.aspx`
- `~/About.aspx.vb`
- `~/Site.master`
- `~/Site.master.vb`
- `~/Web.config`
- `~/Web.sitemap`
- Der gesamte Inhalt des Ordners "`Styles`".
- Der gesamte Inhalt des `Images` Ordners (und dessen Unterordner `BookCovers`)
- `~/App_Code/BasePage.vb`
- `~/Fiction/Default.aspx`
- `~/Fiction/Default.aspx.vb`
- `~/Fiction/Blaze.aspx`
- `~/Fiction/Blaze.aspx.vb`
- `~/Tech/Default.aspx`
- `~/Tech/Default.aspx.vb`
- `~/Tech/CYOW.aspx`
- `~/Tech/CYOW.aspx.vb`
- `~/Tech/TYASP35.aspx`
- `~/Tech/TYASP35.aspx.vb`

Abbildung 3 zeigt FileZilla nach dem Kopieren der erforderlichen Dateien. Wie Sie sehen können, sind die ASP.net-Quell Code Dateien, z. b. `About.aspx.vb`, auf dem lokalen Computer (der Entwicklungsumgebung) und dem Webhost Anbieter (der Produktionsumgebung) vorhanden, da Code Dateien bei Verwendung der automatischen Kompilierung bereitgestellt werden müssen.

[![einen FTP-Client verwenden, um die erforderlichen Dateien vom Desktop auf den Webserver auf dem webhostinstanbieter zu kopieren](deploying-your-site-using-an-ftp-client-vb/_static/image8.png)](deploying-your-site-using-an-ftp-client-vb/_static/image7.png)

**Abbildung 3**: Verwenden eines FTP-Clients zum Kopieren der erforderlichen Dateien vom Desktop auf den Webserver auf dem webhostinstanbieter ([Klicken Sie, um das Bild in voller Größe anzuzeigen](deploying-your-site-using-an-ftp-client-vb/_static/image9.png))

Der Benutzer hat keine Auswirkungen auf das Kompilierungs Modell der Anwendung. Die gleichen ASP.NET-Seiten sind verfügbar, und Sie sehen und Verhalten sich identisch, unabhängig davon, ob die Website mit dem Webanwendungs Projekt Modell oder dem Website Projekt Modell erstellt wurde.

## <a name="updating-a-web-application-on-production"></a>Aktualisieren einer Webanwendung in der Produktion

Webanwendungs Entwicklung und-Bereitstellung sind kein einmal Prozess. Beispielsweise erstellte ich beim Erstellen der Book Review-Website die verschiedenen Seiten und schrieb den begleitenden Code auf meinem PC (der Entwicklungsumgebung). Nachdem Sie einen bestimmten stabilen Zustand erreicht haben, habe ich meine Anwendung bereitgestellt, damit andere Benutzer die Website besuchen und meine Reviews lesen können. Aber die Bereitstellung markiert nicht das Ende meiner Entwicklung auf dieser Website. Ich kann weitere Buchbesprechungen hinzufügen oder neue Features implementieren, z. b. das zulassen, dass meine Besucher Bücher bewerten oder eigene Kommentare hinterlassen. Diese Verbesserungen werden in der Entwicklungsumgebung entwickelt und müssen, wenn Sie fertiggestellt werden, bereitgestellt werden. Die Entwicklung und Bereitstellung sind daher zyklisch. Sie entwickeln eine Anwendung und stellen Sie dann bereit. Während der Standort Live und in der Produktionsumgebung ausgeführt wird, werden neue Funktionen hinzugefügt, und Fehler werden im Laufe der Zeit behoben, was eine erneute Bereitstellung der Anwendung erfordert. Und so weiter.

Beim erneuten Bereitstellen einer Webanwendung müssen Sie möglicherweise nur neue und geänderte Dateien kopieren. Es ist nicht erforderlich, nicht geänderte Seiten oder Server-oder Client seitige Unterstützungs Dateien erneut bereitzustellen (obwohl dies in diesem Fall nicht beeinträchtigt wird).

> [!NOTE]
> Beachten Sie beim Verwenden der expliziten Kompilierung, dass Sie jedes Mal, wenn Sie dem Projekt eine neue ASP.NET-Seite hinzufügen oder Code bezogene Änderungen vornehmen, das Projekt neu erstellen müssen, um die Assembly im Ordner "`Bin`" zu aktualisieren. Folglich müssen Sie diese aktualisierte Assembly in die Produktion kopieren, wenn Sie eine Webanwendung in der Produktion aktualisieren (zusammen mit den anderen neuen und aktualisierten Inhalten).

Beachten Sie außerdem, dass alle Änderungen am `Web.config` oder an den Dateien im `Bin` Verzeichnis den Anwendungs Pool der Website anhalten und neu starten. Wenn der Sitzungs Status im `InProc` Modus (Standardeinstellung) gespeichert wird, verlieren die Besucher Ihrer Website den Sitzungs Status, wenn diese Schlüsseldateien geändert werden. Um dieses Problem zu vermeiden, sollten Sie eine Sitzung mit dem `StateServer`-oder `SQLServer`-Modus speichern. Weitere Informationen zu diesem Thema finden Sie unter [Sitzungs Zustands Modi](https://msdn.microsoft.com/library/ms178586.aspx).

Beachten Sie schließlich, dass die erneute Bereitstellung einer Anwendung von wenigen Sekunden bis zu mehreren Minuten dauern kann, abhängig von der Anzahl und Größe der Dateien, die in die Produktionsumgebung kopiert werden müssen. Während dieser Zeit können Benutzer, die Ihre Website besuchen, Fehler oder ungerade Verhalten erleben. Sie können Ihre gesamte Anwendung "Ausschalten", indem Sie eine Seite mit dem Namen "`App_Offline.htm`" zum Stammverzeichnis Ihrer Anwendung hinzufügen, in der Ihren Benutzern erläutert wird, dass die Website zur Wartung herunter ist (oder was auch immer) und in Kürze wieder verwendet wird. Wenn die `App_Offline.htm`-Datei vorhanden ist, werden alle eingehenden Anforderungen von der ASP.NET-Laufzeit auf diese Seite umgeleitet.

## <a name="summary"></a>Zusammenfassung

Das Bereitstellen einer Webanwendung umfasst das Kopieren der erforderlichen Dateien aus der Entwicklungsumgebung in die Produktionsumgebung. Die gängigste Methode, mit der Dateien über ein Netzwerk übertragen werden, ist die Dateiübertragungsprotokoll (FTP), und die meisten Webhostinganbieter unterstützen den FTP-Zugriff auf die Webserver. In diesem Tutorial wurde erläutert, wie ein FTP-Client verwendet wird, um die erforderlichen Dateien auf dem Webserver bereitzustellen. Nach der Bereitstellung kann die Website von jedem Benutzer mit Internet Verbindung besucht werden.

Fröhliche Programmierung!

### <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [App\_offline. htm und umgehen des Features "IE-benutzerfreundliche Fehler"](https://weblogs.asp.net/scottgu/App_5F00_Offline.htm-and-working-around-the-_2200_IE-Friendly-Errors_2200_-feature)
- [Sitzungs Zustands Modi](https://msdn.microsoft.com/library/ms178586.aspx)

> [!div class="step-by-step"]
> [Zurück](determining-what-files-need-to-be-deployed-vb.md)
> [Weiter](deploying-your-site-using-visual-studio-vb.md)
