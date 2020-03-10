---
uid: web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
title: Verbesserungen in Visual Studio 2005 | Microsoft-Dokumentation
author: microsoft
description: Visual Studio 2005 bietet Webanwendungs Entwicklern eine lange Liste von Verbesserungen und Erweiterungen für Webprojekte.
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 72d90cd0-b3d9-454c-b2eb-ed0d9812f32c
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
msc.type: authoredcontent
ms.openlocfilehash: 64215d556ded0850537a13856fe69b094116ebca
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78464649"
---
# <a name="improvements-in-visual-studio-2005"></a>Verbesserungen in Visual Studio 2005

von [Microsoft](https://github.com/microsoft)

> Visual Studio 2005 bietet Webanwendungs Entwicklern eine lange Liste von Verbesserungen und Erweiterungen für Webprojekte.

Visual Studio 2005 bietet Webanwendungs Entwicklern eine lange Liste von Verbesserungen und Erweiterungen für Webprojekte. Die Leistungsfähigkeit von Visual Studio .NET 2002 und 2003 lag in der Art und Weise, wie Webprojekte behandelt wurden. Visual Studio 2005 fügt eine beträchtliche Anzahl neuer Features hinzu, um diese Beschwerden zu beheben. Weitere Informationen, die Visual Studio .NET 2003 bei der Kompilierung von Webanwendungen bevorzugen, finden Sie unter [Webanwendungs Projekte](https://go.microsoft.com/fwlink/?LinkId=57870).

In diesem Modul werden Verbesserungen bei der Erstellung, Verwaltung und Entwicklung von Webprojekten abgedeckt. In einem späteren Modul werden Verbesserungen bei der Entwicklung von Webprojekten und deren Bereitstellung abgedeckt.

## <a name="frontpage-server-extensions"></a>FrontPage-Servererweiterungen

Visual Studio .NET 2002 und 2003 erforderliche FrontPage-Servererweiterungen im Feld, um Webprojekte zu erstellen oder zu erstellen. Entwickler haben eine Auswahl zwischen zwei unterschiedlichen Zugriffs Modi (FrontPage-Servererweiterungen-oder Datei Zugriffsmodus), die zum Ausführen von Aufgaben FrontPage-Servererweiterungen verwendet werden, z. b. das Festlegen des Anwendungs Stamms in IIS usw.

Visual Studio 2005 entfernt die Abhängigkeit von FrontPage-Servererweiterungen für lokale Projekte. Visual Studio 2005 greift nun direkt auf die IIS-Metabase zu, anstatt die FrontPage-Servererweiterungen zu verwenden. Visual Studio 2005 bietet auch Unterstützung für FTP, wodurch Remote Projekt Zugriff ermöglicht wird, ohne dass FrontPage-Servererweiterungen erforderlich ist.

Für Entwickler, die FrontPage-Servererweiterungen in Ihren Projekten verwenden möchten, ist die Option weiterhin verfügbar. Basierend auf dem starken Feedback der ASP.NET Developer Community ist dies jedoch keine Voraussetzung.

> [!NOTE]
> FrontPage-Servererweiterungen sind nach wie vor für die Remote Projekt Erstellung, das Öffnen usw. erforderlich.

## <a name="aspnet-development-server"></a>ASP.NET Development Server

Visual Studio 2005 wird mit einem neuen Webserver namens ASP.NET Development Server ausgeliefert. (Dieser Webserver wurde früher als Cassini bezeichnet.)

Die ASP.NET Development Server bietet mehrere Vorteile.

- Es ist jetzt möglich, dass nicht-Administratoren auf einem Webserver entwickeln und Debuggen können.
- Der ASP.NET Development Server ordnet virtuelle Verzeichnisse dynamisch einem beliebigen Speicherort im Dateisystem zu, was flexible Projekt Speicherorte zulässt.
- Benutzer in Windows XP Professional, die bereits IIS verwenden, können jetzt neue Webanwendungen erstellen, die sich nicht auf die Datei-oder Ordnerstruktur Ihrer Standard Website in IIS auswirken.

Es ist keine besondere Konfiguration erforderlich, um die ASP.NET Development Server zu nutzen. Wenn ein Webprojekt, das im Dateisystem gehostet wird, deentschlbelt oder durchsucht wird, startet Visual Studio 2005 automatisch eine Instanz des ASP.NET Development Server auf einem zufälligen Port, um die Anforderung zu bedienen.

Weitere Informationen finden Sie im ASP.NET Development Server weiter unten in diesem Modul.

## <a name="improved-file-management"></a>Verbesserte Dateiverwaltung

In Visual Studio 2002 und 2003 werden in einer Projektdatei (VBPROJ für VB.net und. csproj für C#) Informationen zu allen Dateien in der Webanwendung gespeichert. Die Projektmappen-Explorer Anzeige basiert auf den Dateiinformationen in der Projektdatei. Aus diesem Grund zeigen die Projektmappen-Explorer häufig ungenaue Informationen in Fällen an, in denen externe Editoren verwendet wurden. Visual Studio 2002 und 2003 würden oft Dateiänderungen überschreiben oder die aktuellste Version der Dateien nicht anzeigen.

Visual Studio 2005 wird mit der Projektdatei entfernt. Stattdessen werden die Datei-und Ordner Informationen direkt vom Datenträger gelesen, was zu einer exakten Anzeige der Dateien in Ihrem Projekt führt. Da der Ordner Verweise in Visual Studio 2002 und 2003 keinen tatsächlichen Ordner in der Webanwendung darstellt, entfernt Visual Studio 2005 auch den Ordner References aus Projektmappen-Explorer. Um auf die Verweise für das Projekt in Visual Studio 2005 zuzugreifen, sollten Sie die Eigenschaften Seiten für das Projekt verwenden.

## <a name="creating-web-projects"></a>Erstellen von Webprojekten

Webentwickler verfügen über viele neue Optionen für die Projekt Erstellung in Visual Studio 2005. Websites können nun an beliebiger Stelle im Dateisystem erstellt werden, und Sie können dann mithilfe der neuen ASP.NET Development Server dedeentschlbelt oder durchsucht werden. Entwickler können auch mithilfe von FTP neue Websites erstellen.

Klicken Sie hier, um ein Video Exemplarische Vorgehensweise zum Erstellen von Webprojekten in Visual Studio 2005 anzuzeigen.

![](improvements-in-visual-studio-2005/_static/image1.png)

[Vollbildvideos öffnen](improvements-in-visual-studio-2005/_static/creating_projects1.wmv)

### <a name="file-system-projects"></a>Datei System Projekte

Wie Sie in der Video exemplarischen Vorgehensweise gesehen haben, können Sie auswählen, ob Sie Websites im Dateisystem entweder auf dem lokalen Computer oder an einem Remote Speicherort über eine Dateifreigabe erstellen möchten. Websites, die auf dem Dateisystem erstellt werden, werden mithilfe des ASP.NET Development Server durchsucht und deentschlbelt.

> [!NOTE]
> Der ASP.NET Development Server kann für Kunden Verwirrung verursachen. Wenn ein Webprojekt auf dem Dateisystem in der IISS-Verzeichnisstruktur (z. b. c:/Inetpub/Wwwroot) erstellt wird, wird die Website weiterhin über die ASP.NET Development Server durchsucht, wenn Sie in Visual Studio 2005 gestartet wird. Daher ist eine beliebige IIS-Konfiguration (d. h. Authentifizierungsmethoden) nicht anwendbar.

Das Standardweb Projekt entfernt auch einen großen Aufwand, indem nur eine default. aspx-Seite, Default.cs-Datei und ein App/_data-Ordner eingeschlossen werden. Die Web. config-Datei und die speziellen Ordner (d. h. app/_Code) werden hinzugefügt, sobald Sie benötigt werden. Das Webprojekt enthält nur die Dateien und Ordner, die Sie benötigen.

### <a name="http-projects"></a>HTTP-Projekte

Bei http-Projekten kann es sich entweder um Projekte handeln, die auf einer lokalen IIS-Website oder auf einer Remote Website erstellt werden. Der Standard Projekt Speicherort ist `http://localhost`. Wenn Sie auf die Schaltfläche "Durchsuchen" klicken, sind zwei http-Optionen verfügbar: lokale IIS und Remote Website. Der Hauptunterschied zwischen diesen beiden Optionen ist die Methode, mit der die Website Informationen im Dialogfeld Speicherort auswählen und in der Art und Weise, wie die Dateien auf den Webserver kopiert werden, angezeigt werden.

Mit der lokalen IIS-Option werden die Standortinformationen aus der Metabase auf dem lokalen Computer gelesen, und die Dateien werden mithilfe des Dateisystems kopiert. Die Option Remote Standort verwendet die FrontPage-Servererweiterungen, und die Standortinformationen und-Dateien werden mithilfe von http-und FrontPage-Servererweiterungen RPC-Aufrufen kopiert.

> [!NOTE]
> Die Dateien "vs # # #/_tmp. htm" und "Get/_aspx/_ver. aspx" werden nicht mehr verwendet, um Versionsinformationen zu bestimmen.

Die Standard-HTTP-Option ist Local IIS. Mit dieser Option wird die IIS-Metabase gelesen, um zu bestimmen, welche Standorte verfügbar sind und an welchem Speicherort Inhalt erstellt werden soll. Sie können einen anderen Ordner oder ein anderes virtuelles Verzeichnis auswählen, indem Sie ihn in der Strukturansicht auswählen. Sie können auch ein neues virtuelles Verzeichnis erstellen, Ordner als Anwendungen markieren und vorhandene virtuelle Verzeichnisse in diesem Dialogfeld löschen.

![Dialog Feld "Speicherort auswählen"](improvements-in-visual-studio-2005/_static/image1.gif)

**Abbildung 1**: das Dialog Feld "Speicherort auswählen"

Wenn Sie im Gegensatz zu früheren Versionen von Visual Studio das Kontrollkästchen **Secure Sockets Layer verwenden** aktivieren und das SSL-Zertifikat nicht mit der URL identisch ist, die Sie durchsuchen, wird Ihnen ein Dialogfeld mit einer Sicherheitswarnung angezeigt, in dem Sie gefragt werden, ob Sie den Vorgang fortsetzen möchten. Wenn das Zertifikat bei Verwendung von Visual Studio .NET 2003 nicht übereinstimmt, würde das Erstellen des Projekts fehlschlagen.

![Sicherheitswarnung bezüglich SSL-Zertifikat](improvements-in-visual-studio-2005/_static/image2.gif)

**Abbildung 2**: Sicherheitswarnung bezüglich SSL-Zertifikat

### <a name="note-on-host-headers"></a>Hinweis zu Host Headern

Wenn Sie eine Webanwendung auf einer Website erstellen, die an eine bestimmte IP-Adresse gebunden ist, müssen Sie sicherstellen, dass ein Host Header konfiguriert ist. Andernfalls wird die Website von Visual Studio unter `http://localhost`erstellt, aber die IP-Adresse wird nicht ordnungsgemäß aufgelöst, wenn die Website durchsucht oder innerhalb der IDE deentschlt wird.

Wenn Sie die Option Remote Standort auswählen, wird das Dialogfeld so geändert, dass Sie die Ziel-URL für die neue Website eingeben können. Diese URL muss sich auf einem Server befinden, auf dem die FrontPage-Servererweiterungen aktiviert ist. Wenn Sie mit dem FrontPage-Servererweiterungen auf dem lokalen Webserver arbeiten möchten, können Sie die Option Remote Site verwenden und eine lokale URL angeben.

![Erstellen einer Website auf einem Remote Server](improvements-in-visual-studio-2005/_static/image1.jpg)

**Abbildung 3**: Erstellen einer Website auf einem Remote Server

Wenn beim Erstellen einer Anwendung an einem Remote Standort über SSL ein SSL-Zertifikat nicht gefunden wird, unterscheidet sich das Bestätigungs Dialogfeld leicht von dem Dialogfeld, das bei Verwendung der lokalen IIS-Option angezeigt wird.

![Warnung zu Remote Standortsicherheit](improvements-in-visual-studio-2005/_static/image3.gif)

**Abbildung 4**: Warnung zur Remote Standortsicherheit

<a id="_Toc116100243"></a>

#### <a name="ftp"></a>FTP

In Visual Studio 2005 wird die Option zum Erstellen von Websites per FTP eingeführt. Wenn Sie diese Option verwenden, erstellt die IDE die Dateien lokal im temporären Ordner des Benutzers und verwendet dann FTP, um die Dateien an den FTP-Speicherort zu verschieben.

> [!NOTE]
> Der Speicherort des temporären Ordners ist c:/Documents und Settings/&lt;User&gt;/local ein Settings/Temp/vwdwebcache/&lt;Server&gt;/_&lt;Anwendungsname&gt;

Wenn Sie die FTP-Option verwenden, wird das Dialogfeld Speicherort auswählen angezeigt. Sie geben die erforderlichen FTP-Verbindungsinformationen in dieses Dialogfeld ein, wie unten gezeigt.

![Dialog Feld "Speicherort auswählen" für FTP](improvements-in-visual-studio-2005/_static/image2.jpg)

**Abbildung 5**: das Dialog Feld "Speicherort auswählen" für FTP

## <a name="lab-setup-ftp-site-and-create-a-project"></a>Lab: FTP-Site einrichten und ein Projekt erstellen

Mit den folgenden Schritten wird die FTP-Site so konfiguriert, dass ein Benutzer über einen Speicherort verfügt, den er nur über FTP hochladen kann.

### <a name="install-the-ftp-service"></a>Installieren des FTP-Dienstanbieter

1. Öffnen Sie die Option Software entfernen, wählen Sie Windows-Komponenten hinzufügen/entfernen
2. Wählen Sie Internetinformationsdienste (Anwendungs Server unter Windows 2003) aus, und klicken Sie auf **Details**.
3. Aktivieren Sie **Dateiübertragungsprotokoll (FTP)-Dienst** , und klicken Sie auf **OK**.
4. Klicken Sie zum Installieren des FTP-Dienes auf **weiter** .

### <a name="create-a-new-folder-for-content"></a>Neuen Ordner für Inhalt erstellen

1. Erstellen Sie in Windows-Explorer einen neuen Ordner mit dem Namen **User1** innerhalb von c:/Inetpub/Wwwroot.

#### <a name="configure-folders-and-permissions-on-folders"></a>Ordner und Berechtigungen für Ordner konfigurieren.

1. Öffnen Sie das Snap-in "Internetinformationsdienste" über "Verwaltung". Unter dem Knoten "Computername" befindet sich nun der Ordner "FTP-Sites".
2. Erweitern Sie **FTP-Sites**.
3. Klicken Sie mit der rechten Maustaste auf die **FTP-Standard Site**, wählen Sie **neu**und dann **virtuelles Verzeichnis**aus, und klicken Sie auf **weiter**
4. Geben Sie **User1** als Namen des virtuellen Verzeichnisses ein, und klicken Sie auf **weiter**.
5. Geben Sie **c:/Inetpub/Wwwroot/user1** als Pfad ein, und klicken Sie auf **weiter**.
6. Klicken Sie auf **weiter** und dann auf **Fertig** stellen, um den Assistenten abzuschließen.
7. Klicken Sie mit der rechten Maustaste auf das virtuelle Verzeichnis **User1** unter Default FTP Site, und wählen Sie **Eigenschaften**aus.
8. Aktivieren Sie das Kontrollkästchen **Schreiben** , und klicken Sie zum Schließen des Dialog Felds auf **OK** .
9. Klicken Sie mit der rechten Maustaste auf **default FTP Site** , und wählen Sie **Eigenschaften**
10. Deaktivieren Sie auf der Registerkarte **Sicherheits Konten** die Option **anonyme Verbindungen zulassen**.
11. Klicken Sie im Dialogfeld auf **Ja** , um zu Fragen, ob Sie fortfahren möchten.
12. Klicken Sie auf **OK** , um das Dialogfeld zu schließen.
13. Erweitern Sie die **Standard Website** unter dem Knoten **Websites** .
14. Klicken Sie mit der rechten Maustaste auf das Verzeichnis **User1** und wählen Sie **Eigenschaften**
15. Klicken Sie im Abschnitt **Anwendungseinstellungen** auf **Erstellen** , um den Ordner als Anwendung zu markieren.
16. Klicken Sie auf **OK** , um das Dialogfeld zu schließen.
17. Schließen Sie das Internetinformationsdienste-Snap-in.

### <a name="create-web-project"></a>Webprojekt erstellen

1. Öffnen Sie Visual Studio 2005.
2. Wählen Sie im Menü **Datei** die Option **neue Website**aus.
3. Wählen Sie in der Dropdown Liste **Speicherort** die Option **FTP**aus.
4. Klicken Sie auf **Durchsuchen**.
5. Geben Sie im Textfeld **Server** den **Namen localhost** ein.
6. Geben Sie **User1** in das Textfeld Verzeichnis ein.
7. Klicken Sie auf **Öffnen**. Der FTP-Speicherort wird im Dialogfeld "neue Website" eingegeben.
8. Klicken Sie auf **OK**.
9. Deaktivieren Sie im Dialogfeld FTP-Anmeldung die Option **Anonyme Anmeldung** , und geben Sie Ihre Anmelde Informationen ein, und klicken Sie auf **OK**.
10. Was ist die URL für das Projekt? (Die URL für das Projekt wird in Projektmappen-Explorer angezeigt.)
11. Wählen Sie im Menü **Erstellen** die Option **Website erstellen oder Projekt** Mappe **Erstellen**aus.
12. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf Default. aspx, und wählen Sie **in Browser anzeigen aus**.
13. Geben Sie im Dialogfeld Website-URL erforderlich `http://localhost/user1` für die URL ein, und klicken Sie auf **OK**.

> [!NOTE]
> Wenn Sie einen Fehler mit dem Hinweis erhalten, dass der Typ/_Default nicht geladen werden kann, stellen Sie sicher, dass Sie ASP.NET 2,0 auf Ihrer Website und nicht eine frühere Version ausführen. Dies können Sie über die Registerkarte ASP.net in Internetinformationsdienste tun.

## <a name="opening-web-projects"></a>Öffnen von Webprojekten

Das Öffnen von Webprojekten ähnelt dem Erstellen von Projekten. In den folgenden Abschnitten werden Bereiche erläutert, die bei der Arbeit innerhalb der IDE zu beobachten sind. Außerdem wird die Arbeit mit Webprojekten mithilfe von http und FTP behandelt.

Um ein Webprojekt zu öffnen, wählen Sie im Menü Datei die Option Website öffnen aus. Sie werden aufgefordert, das gleiche Dialogfeld auszuwählen, das Sie zuvor behandelt haben, und Sie verfügen über dieselben vier Optionen, die Ihnen zur Verfügung stehen: Datei System, lokale IIS, FTP und Remote Standort.

<a id="_Toc116100245"></a>

## <a name="file-system"></a>Dateisystem

Wie bereits zuvor in diesem Modul angegeben, verwendet Visual Studio keine Projektdatei mehr. Wenn Sie also eine Website aus dem Dateisystem öffnen möchten, haben Sie auch die Möglichkeit, einen beliebigen Ordner auszuwählen, auch wenn der Ordner, den Sie auswählen, nicht anfänglich als Webprojekt in Visual Studio erstellt wurde. Beispielsweise können Sie auswählen, dass der Ordner "eigene Dateien" als Website geöffnet wird. Visual Studio öffnet den Ordner und zeigt die Dateien wie unten dargestellt an.

![Meine Dokumente als Website geöffnet](improvements-in-visual-studio-2005/_static/image3.jpg)

**Abbildung 6**: *meine Dokumente* als Website geöffnet

Da Visual Studio bei Bedarf nur weitere Dateien und Ordner erstellt, werden keine weiteren Dateien oder Ordner zum geöffneten Speicherort hinzugefügt. Ein Nebeneffekt dieser Architektur besteht darin, dass Sie die Schachtelung von Websites im Dateisystem verhindern. Beachten Sie beispielsweise die folgende Verzeichnisstruktur.

Webprojekt bei "C:/mywebsite"

Ein weiteres Webprojekt bei "C:/mywebsite/netsted"

Wenn Sie die Website unter "c:/mywebsite" öffnen, wird der Ordner "der Ordner" als Unterordner dieser Anwendung angezeigt.

<a id="_Toc116100246"></a>

## <a name="http"></a>HTTP

Wenn Sie Websites über HTTP öffnen, werden die Einstellungen entweder aus der IIS-Metabasis (lokale IIS) oder mithilfe FrontPage-Servererweiterungen (Remote Site) gelesen. Wenn sich die Webanwendungen in einer Liste befinden, werden diese auch mit einem Symbol angezeigt, das Sie als Anwendung identifiziert. Wenn Sie mit der Arbeit mit Webanwendungen in FrontPage vertraut sind, ist das Verhalten in Visual Studio 2005 ähnlich.

Obwohl in Visual Studio ein Symbol für Anwendungen angezeigt wird, die unter der Anwendung geschachtelt sind, die derzeit in der IDE geöffnet ist, können Sie Sie nicht erweitern, um ihren Inhalt anzuzeigen. Sie können jedoch auf diese doppelklicken, um Sie zu öffnen. Wenn Sie dies tun, wird ein Dialogfeld angezeigt, in dem Sie aufgefordert werden, entweder die Webanwendung zu öffnen (und die aktuell geöffnete Projekt Mappe zu ersetzen) oder die Webanwendung der aktuellen Projekt Mappe hinzuzufügen.

![Durch Doppelklicken auf ein Symbol für eine Schaltfläche für eine Anwendung wird dieses Dialogfeld angezeigt.](improvements-in-visual-studio-2005/_static/image4.jpg)

**Abbildung 7**: durch Doppelklicken auf ein Symbol für eine Schaltfläche für eine Anwendung wird dieses Dialogfeld angezeigt.

<a id="_Toc116100247"></a>

## <a name="ftp-site"></a>FTP-Site

Wenn Sie eine Website über FTP öffnen, werden alle Dateien lokal in den temporären Ordner kopiert. Der vollständige Pfad für den lokalen Speicherort wird im Eigenschaften Bereich für das Projekt angezeigt und im folgenden Format erstellt.

C:/Dokumente und Einstellungen/&lt;Benutzer&gt;/local ein Settings/Temp/vwdwebcache/&lt;Server&gt;/_&lt;Anwendungsname&gt;

Bei Verwendung von FTP muss Visual Studio die Basis-URL für das Projekt angeben, damit Sie es wie unten dargestellt durchsuchen können. Wenn Sie keine Basis-URL angeben, werden Sie von Visual Studio gefragt, wenn Sie zum ersten Mal versuchen, eine Seite auf der Website zu durchsuchen.

![Angeben einer Basis-URL für FTP-Sites](improvements-in-visual-studio-2005/_static/image5.jpg)

**Abbildung 8**: Angeben einer Basis-URL für FTP-Sites

## <a name="improvements-in-compilation"></a>Verbesserungen beim Kompilieren

Das Arbeiten mit Webanwendungen in Visual Studio 2005 ist deutlich schneller als in früheren Versionen. Dies liegt in keinem kleinen Teil der Änderungen in der Kompilierungs Architektur.

In Visual Studio 2002 und 2003 wurden Webanwendungen in eine primäre Assembly kompiliert, die sich im Ordner ""/bin "" befindet. In Visual Studio 2005 wurde ein App/_Code-Ordner hinzugefügt. Klassen und anderer nicht-UI-Code werden dem App/_Code-Ordner hinzugefügt. Wenn Visual Studio das Projekt erstellt, werden alle Dateien im Ordner App/_Code in eine einzelne APP/_Code. dll-Datei kompiliert. Das Ergebnis dieser Änderung ist, dass nachfolgende Builds wesentlich schneller sind als in früheren Versionen.

> [!NOTE]
> Das MSBuild-Befehlszeilen Dienstprogramm kann auch zum Erstellen von ASP.NET-Webanwendungen verwendet werden. Dieses Tool wird in Modul 9 behandelt.

Eine weitere Kompilierungs Erweiterung ist die Option neue buildseite im Menü Build. Diese Funktion ermöglicht es einem Entwickler, nur die aktuelle Seite (zusammen mit, natürlich und Abhängigkeiten) neu zu erstellen, damit Änderungen schneller kompiliert werden können. Da C# keine Hintergrund Kompilierung für die Aktualisierung von IntelliSense usw. bietet, profitieren Sie von diesem Feature enorm, da damit IntelliSense schnell aktualisiert werden kann, indem einfach eine einzelne Seite neu erstellt wird.

Mit den Buildeigenschaften für ein Projekt können Sie den Typ des Builds konfigurieren, der vor der Ausführung der Startseite auftritt. Entwickler können auswählen, dass nur die aktuelle Seite erstellt wird, damit Visual Studio Anwendungen schneller nach Codeänderungen Debuggen kann.

![Start Aktion für die buildseite](improvements-in-visual-studio-2005/_static/image6.jpg)

**Abbildung 9**: Start Aktion der Build-Seite

Eine weitere hervor artige Erweiterung von Visual Studio und der ASP.NET-Architektur finden Sie im Bereich "Bearbeiten und Fortfahren". In Visual Studio 2005 können Entwickler mit dem Debuggen eines Projekts beginnen und Codeänderungen am Projekt vornehmen, ohne den Debugger zu trennen. Tatsächlich können Sie mit dem Debuggen eines Projekts beginnen, eine neue Klasse hinzufügen, dieser Klasse Code hinzufügen, der Seite Code hinzufügen, der eine neue Instanz dieser Klasse erstellt und eine Methode der Klasse ausführt, ohne den Debugger zu trennen. Das Ausführen des neuen Codes ist buchstäblich genauso einfach wie das Aktualisieren des Browsers!

Klicken Sie hier, um eine exemplarische Vorgehensweise für die Funktion "Bearbeiten und Fortfahren" in Visual Studio 2005 anzuzeigen.

![](improvements-in-visual-studio-2005/_static/image2.png)

[Vollbildvideos öffnen](improvements-in-visual-studio-2005/_static/editcontinue1.wmv)

Die robuste Funktion zum Bearbeiten und fortfahren in ASP.NET 2,0 und Visual Studio 2005 ist auf eine architektonische Änderung für ASP.NET-Anwendungen zurückzuführen. In ASP.NET 1. x wurden in Visual Studio 2002/2003 erstellte Anwendungen in eine primäre Assembly kompiliert, die im Ordner "/bin" gespeichert war. Alle Klassen, Seiten usw. für die Anwendung wurden in diese eine DLL kompiliert. Anschließend kompiliert ASP.net zur Laufzeit alle Steuerelemente, Markup und ASP.NET-Code in Seiten und kopiert diese DLLs in den temporären Ordner ASP.net.

In Visual Studio 2005 mit ASP.NET 2,0 wurden die beiden Kompilierungs Modelle oben (einer für Visual Studio und einer für ASP.net zur Laufzeit) zu einem gemeinsamen Kompilierungs Modell zusammengeführt. Dies bedeutet, dass alle Kompilierungs Probleme jetzt während der Entwicklungsphase statt zur Laufzeit abgefangen werden. Außerdem ermöglicht es Designer und IntelliSense-Unterstützung für Funktionen wie Benutzer Steuerelemente und Masterseiten.

Klicken Sie hier, um ein Video zur exemplarischen Vorgehensweise zur Designer Unterstützung für Benutzer Steuerelemente anzuzeigen.

![](improvements-in-visual-studio-2005/_static/image3.png)

[Vollbildvideos öffnen](improvements-in-visual-studio-2005/_static/usercontrols1.wmv)

> [!NOTE]
> Wenn ein Benutzer Steuerelement von einer Seite entfernt wird, verbleibt die @Register-Direktive im Markup und sollte manuell entfernt werden, um Parserfehler zu vermeiden, wenn das Benutzer Steuerelement von der Website gelöscht wird.

Eine weitere Verbesserung im Visual Studio-Kompilierungs Modell ist die Funktion Website veröffentlichen. Da das Veröffentlichungs Feature die Website vorkompiliert, können Entwickler die zusätzliche Leistung nutzen, wenn Sie keine Bedarfs gesteuerte Kompilierung durchführen müssen. Außerdem wird der gesamte Quellcode im App/_Code Ordner in einer DLL vorkompiliert, sodass kein Quellcode bereitgestellt werden muss.

![Dialog Feld "Website veröffentlichen"](improvements-in-visual-studio-2005/_static/image7.jpg)

**Abbildung 10**: Dialog Feld "Website veröffentlichen"

> [!NOTE]
> Das Hilfsprogramm ASPNET/_compile. exe kann auch zum Vorkompilieren einer ASP.NET-Webanwendung verwendet werden. Dieses Tool wird in Modul 9 behandelt.

Wenn Sie eine Website veröffentlichen, werden die vorkompilierten Dateien wie unten dargestellt im temporären Ordner ASP.net files gespeichert. Dateien mit der Erweiterung *. kompilierte* Dateien sind XML-Dateien, die Abhängigkeiten für bestimmte DLLs definieren. Alle WebForms-oder Benutzer Steuerelemente werden in zufällige DLLs kompiliert, die mit *App/_Web/_* beginnen.

Wenn Sie das Kontrollkästchen *diese vorkompilierte Site kann nicht aktualisiert werden* aktiviert lassen, wird das Markup in ihren WebForms-und Benutzer Steuerelementen nicht in eine DLL vorkompiliert, sodass Sie nach der Bereitstellung Änderungen vornehmen können. Wenn Sie das Markup lieber sperren möchten, damit Änderungen am bereitgestellten Inhalt nicht zulässig sind, deaktivieren Sie dieses Kontrollkästchen.

Mithilfe des Kontrollkästchens " *Fixed Naming" und "Single Page* Assemblys" können Sie die Batch Kompilierung deaktivieren, sodass jede Seite in eine Assembly mit festem Namen kompiliert wird. Wenn Sie dieses Kontrollkästchen deaktiviert lassen, können Sie die Batch Kompilierung nutzen.

Mit dem Kontrollkästchen *starke Benennung für vorkompilierte* Assemblys aktivieren können Sie die vorkompilierten Assemblys mit starkem Namen versehen.

> [!NOTE]
> In ASP.NET 1. x mussten Assemblys mit starkem Namen im globalen Assemblycache (GAC) installiert werden. In ASP.NET 2,0 müssen Assemblys mit starkem Namen nicht im GAC installiert werden.

![Vorkompilierte Dateien für ASP.NET-Anwendungen](improvements-in-visual-studio-2005/_static/image8.jpg)

**Abbildung 11**: vorkompilierte Dateien für ASP.NET-Anwendungen

> [!NOTE]
> In der obigen Anwendung war keine Web. config-Datei vorhanden. Wenn dies der Fall gewesen wäre, hätte Sie nach dem Vorgang "Website veröffentlichen" den Namen " *PrecompiledApp. config* " erhalten.

## <a name="improvements-in-deployment"></a>Verbesserte Bereitstellung

Wie bei Visual Studio 2002 und 2003 bietet Visual Studio 2005 eine Funktion zum Kopieren von Projekten. Die Funktion wurde jedoch in Visual Studio 2005 bereinigt und wird jetzt als "Website kopieren" bezeichnet.

Das Dialogfeld Website kopieren wird in einen linken und einen rechten Rahmen aufgeteilt. Der linke Frame wird als Quell Website und der Rechte Rahmen als Remote Website bezeichnet. Ein Problem, das einige Entwickler möglicherweise verwirrt, besteht darin, dass die im rechten Rahmen angezeigte Site nicht notwendigerweise eine Remote Site ist. Dabei kann es sich um eine Site auf dem lokalen Dateisystem oder auf der lokalen Instanz von IIS handeln. Außerdem ist die im linken Rahmen angezeigte Site nicht notwendigerweise die Quell Website, da das Dialogfeld Ihnen ermöglicht, von der Remote Website auf der Quell Website *zu* veröffentlichen.

Wenn Sie ein Projekt auf eine Remote Website kopieren, muss auf diesem Standort die FrontPage-Servererweiterungen installiert sein. Wenn dies nicht der Fall ist, müssen Sie über FTP eine Verbindung herstellen. Wenn Sie jedoch ein Projekt auf die lokale IIS-Instanz kopieren, sind FrontPage-Servererweiterungen nicht erforderlich.

> [!NOTE]
> Wenn Sie versuchen, auf der lokalen IIS-Instanz eine neue Website zu erstellen und die FrontPage 2002-Server Erweiterungen installiert sind, erhalten Sie eine Fehlermeldung mit dem Hinweis, dass das Erstellen von Websites auf einem SharePoint-Server nicht unterstützt wird. In diesem Fall haben Sie die Möglichkeit, die FrontPage 2000-Server Erweiterungen zu installieren oder die FrontPage-Servererweiterungen zu entfernen.

Klicken Sie hier, um ein Video mit der exemplarischen Vorgehensweise zum Kopieren einer Website zu lesen.

![](improvements-in-visual-studio-2005/_static/image4.png)

[Vollbildvideos öffnen](improvements-in-visual-studio-2005/_static/copysite1.wmv)

## <a name="improvements-in-debugging"></a>Verbesserungen beim Debugging

Es gibt vier wichtige Verbesserungen beim Debuggen in Visual Studio 2005.

- Das lokale Debuggen als nicht Administrator ist standardmäßig möglich.
- Das debug-Attribut für das Kompilierungs Element ist jetzt standardmäßig "false".
- Die Einrichtung und Konfiguration des Remote Debuggens ist einfacher als zuvor.
- Sie können jetzt eine Website Debuggen, die über einen FTP-Speicherort geöffnet wurde.

## <a name="debugging-as-a-non-administrator"></a>Debuggen als nicht Administrator

Das Hinzufügen der ASP.NET Development Server ermöglicht nicht Administratoren das einfache Debuggen von ASP.NET-Anwendungen. Wenn eine ASP.NET-Anwendung, die im lokalen Dateisystem ausgeführt wird, debuggt wird, startet Visual Studio die ASP.NET Development Server im Kontext des angemeldeten Benutzers. Dieser Benutzer kann die Anwendung dann ohne zusätzliche Konfiguration Debuggen.

## <a name="debug-is-false-by-default"></a>Debug ist standardmäßig "false"

In ASP.NET 1. x wurde das *Debug* -Attribut im *Kompilierungs* Element der Datei "Web. config" standardmäßig auf " *true* " festgelegt. Es wurde immer empfohlen, dass Entwickler dieses Attribut auf " *false* " setzen, bevor Sie eine Anwendung in der Produktionsumgebung bereitstellen, aber da die meisten Entwickler die Konsequenzen nicht vollständig verstehen, dass das debug-Attribut auf "true" festgelegt ist, haben Sie es einfach unverändert gelassen.

Das schwerwiegendsten Problem bei der Festlegung des Debug-Attributs auf "true" besteht darin, dass das ASP. Nets-Batch Kompilierungs Modell deaktiviert wird. Daher wird jede Seite in eine separate DLL kompiliert. Wenn eine Webanwendung aus Tausenden von Seiten besteht (nicht mit einer beliebigen Methode), bedeutet dies, dass mehrere tausend kleine DLLs von der Anwendung erstellt werden. Obwohl die Größe dieser DLLs gering ist, werden Sie nicht an eine bestimmte Position im Arbeitsspeicher geladen. Daher bewirken Sie eine Fragmentierung im Systemspeicher und können zu den Vorkommen von outo fmemoryexception beitragen.

In ASP.NET 2,0 ist das debug-Attribut standardmäßig auf false festgelegt. Wenn ein Entwickler eine ASP.NET-Anwendung in Visual Studio 2005 debuggt, werden Sie wie bereits gesehen aufgefordert, eine Web. config-Datei mit aktiviertem Debugging hinzuzufügen. Dies führt zu denselben Nachteilen, die in ASP.NET 1. x vorhanden waren, aber jetzt weist der Entwickler eindeutig darauf hin, dass das Attribut auf false zurückgesetzt werden sollte, bevor die Anwendung in die Produktionsumgebung verschoben wird.

## <a name="remote-debugging-setup-and-configuration"></a>Einrichten und Konfigurieren des Remote Debuggens

In Visual Studio 2002/2003 stützte das Remote Debuggen auf den Machine Debug-Manager (MDM. exe) und den Vs7jit. exe-Prozess. Aus diesem Grund war die Problembehandlung bei Problemen mit dem Remote Debuggen häufig ein schwarzes Kästchen für Kunden, und es war häufig nicht viel besser für PSS.

Visual Studio 2005 entfernt die Abhängigkeit von den Prozessen "MDM. exe" und "Vs7jit. exe". Stattdessen wird nun der Remotedebugmonitor-Dienst (msvsmon. exe) verwendet.

Das Remote Debuggen in Visual Studio 2005 ist ziemlich einfach. Vor dem Debuggen muss msvsmon. exe auf dem Remote Server ausgeführt werden. Sie können den Remotedebugmonitor von der Visual Studio-CD installieren oder einfach "msvsmon. exe" über eine Freigabe ausführen, ohne alles auf dem Webserver zu installieren.

Wenn Sie msvsmon. exe ausführen, wird es wahrscheinlich sein, dass die Ports für das Remote Debuggen blockiert werden. Glücklicherweise können Sie die Sperre der Ports direkt im Dialogfeld "Warnung" (wie unten gezeigt) blockieren.

![Benachrichtigung, dass die Windows-Firewall das Remote Debugging blockiert](improvements-in-visual-studio-2005/_static/image9.jpg)

**Abbildung 12**: Benachrichtigung, dass das Remote Debugging von der Windows-Firewall blockiert wird

Nachdem Sie die Blockierung der für das Debuggen erforderlichen Ports aufgehoben haben, wird die Remotedebugmonitor wie unten dargestellt angezeigt. Über diese Schnittstelle können Sie Verbindungen überwachen und die debuggingberechtigungen leicht ändern.

![Der Remotedebugmonitor](improvements-in-visual-studio-2005/_static/image10.jpg)

**Abbildung 13**: der Remotedebugmonitor

Es ist auch möglich, eine Webanwendung, die über FTP geöffnet wurde Die Schritte sind identisch mit denen, die zuvor abgedeckt wurden. Sie müssen jedoch eine Basis-URL angeben, um das FTP-Projekt zu durchsuchen, wie weiter oben in diesem Modul beschrieben.

## <a name="lab-2"></a>Lab 2

## <a name="remote-debugging-with-visual-studio-2005"></a>Remote Debuggen mit Visual Studio 2005

Diese Übungseinheit führt Sie durch das Remote Debuggen mit Visual Studio 2005.

Klicken Sie hier, um eine exemplarische Vorgehensweise für dieses Lab zu finden.

![](improvements-in-visual-studio-2005/_static/image5.png)

[Vollbildvideos öffnen](improvements-in-visual-studio-2005/_static/remdebug1.wmv)

Diese Übungseinheit erfordert, dass Sie über zwei Computer verfügen, von denen eine mit Visual Studio 2005 und die andere mit IIS 5 oder höher ausgeführt wird.

1. Öffnen Sie Visual Studio 2005, und erstellen Sie eine neue Website auf dem Remote Server.

> [!NOTE]
> Sie können die Website in einer IIS-Remote Instanz oder per FTP erstellen.

1. Suchen Sie auf dem Remoteweb Server mithilfe eines UNC-Pfads auf dem Entwicklungs Computer "msvsmon. exe", und führen Sie ihn aus.  
 Der Standard Speicherort für msvsmon. exe ist//Server/c $/Program Files/Microsoft Visual Studio 8/Common7/IDE/Remote Debugger/x86.
2. Wenn Sie aufgefordert werden, die Blockierung von Ports für das Remote Debuggen zu entsperren,
3. Öffnen Sie auf dem Entwicklungs Computer den Code Behind für Default. aspx, und legen Sie einen Breakpoint in der Page/_Load-Methode fest.
4. Starten Sie das Debuggen vom Entwicklungs Computer aus.

Sie sollten den Haltepunkt erwartungsgemäß erreichen.

## <a name="aspnet-development-server"></a>ASP.NET Development Server

Wie bereits erläutert, ist Visual Studio 2005 mit einem Webserver namens "ASP.NET Development Server" ausgeliefert. (Das ASP.NET Development Server wird manchmal auch als Cassini bezeichnet.) Dieser Webserver ist eine bequeme Möglichkeit, Webanwendungen zu durchsuchen und zu debuggen, die im Dateisystem ausgeführt werden.

Der ASP.NET Development Server ist ein eingeschränkter Webserver. Es sind keine Remote Verbindungen zulässig. es werden keine Anforderungen von anderen Benutzern als dem Benutzer zugelassen, der den Webserver gestartet hat. Außerdem bietet es keine Möglichkeit, ASP-Seiten zu bieten. Es werden nur ASP.NET-Ressourcen und HTML-Ressourcen (einschließlich Images, CSS-Dateien usw.) bereitgestellt.

Die ASP.NET Development Server kann über die Befehlszeile gestartet werden, indem Sie die Datei "WebDev. Webserver. exe" unter "c:/Windows/Microsoft. NET/Framework/v 2.0./ */* / */* /*" ausführen. Im folgenden Dialogfeld werden die verfügbaren Parameter angezeigt.

![](improvements-in-visual-studio-2005/_static/image11.jpg)

**Abbildung 14**

> [!NOTE]
> Die ASP.NET Development Server wird nicht unterstützt, wenn Sie explizit über die Befehlszeile gestartet wird.
