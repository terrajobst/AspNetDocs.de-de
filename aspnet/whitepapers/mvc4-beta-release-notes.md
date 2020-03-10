---
uid: whitepapers/mvc4-beta-release-notes
title: ASP.NET MVC 4 | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Dokument wird die Veröffentlichung von ASP.NET MVC 4 Beta für Visual Studio 2010 beschrieben.
ms.author: riande
ms.date: 09/09/2011
ms.assetid: 666407bb-81de-4319-89ba-0302c382a208
msc.legacyurl: /whitepapers/mvc4-beta-release-notes
msc.type: content
ms.openlocfilehash: 17800dfe091bbb7afb25f7f41e3bd885b882edb0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419841"
---
# <a name="aspnet-mvc-4"></a>ASP.NET MVC 4

> In diesem Dokument wird die Veröffentlichung von ASP.NET MVC 4 Beta für Visual Studio 2010 beschrieben.
> 
> > [!NOTE]
> > Dies ist nicht die aktuellste Version. Die Anmerkungen zu dieser Version von ASP.NET MVC 4 RC sind [hier](mvc4-release-notes.md)verfügbar.

- [Installationshinweise](#_Toc303253802)
- [Dokumentation](#_Toc303253803)
- [Unterstützung](#_Toc303253804)
- [Software Anforderungen](#_Toc303253805)
- [Aktualisieren eines ASP.NET MVC 3-Projekts auf ASP.NET MVC 4](#_Toc303253806)
- [Neue Features in ASP.NET MVC 4 Beta](#_Toc303253807)

    - [ASP.NET-Web-API](#_Toc317096197)
    - [ASP.net Single-Page-Anwendung](#_Toc317096198)
    - [Verbesserungen an Standard Projektvorlagen](#_Toc303253808)
    - [Mobile Projektvorlage](#_Toc303253809)
    - [Anzeigemodi](#_Toc303253810)
    - [jQuery Mobile, der Ansichts Switcher und Browser Überschreibung](#_Toc303253811)
    - [Rezepte für die Code Generierung in Visual Studio](#_Toc303253812)
    - [Aufgaben Unterstützung für asynchrone Controller](#_Toc303253813)
    - [Azure SDK](#_Toc303253814)
    - [Bekannte Probleme und wichtige Änderungen](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Installationshinweise

ASP.NET MVC 4 Beta für Visual Studio 2010 kann mithilfe des Webplattform-Installers auf der [ASP.NET MVC 4-Startseite](../mvc/mvc4.md) installiert werden.

Vor der Installation von ASP.NET MVC 4 Beta müssen alle zuvor installierten Vorschau Versionen von ASP.NET MVC 4 deinstalliert werden.

Diese Version ist nicht kompatibel mit der .NET Framework 4,5 Developer Preview. Sie müssen .NET 4,5 Developer Preview deinstallieren, bevor Sie ASP.NET MVC 4 Beta installieren.

ASP.NET MVC 4 kann installiert und parallel mit ASP.NET MVC 3 ausgeführt werden.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Dokumentation

Die Dokumentation für ASP.NET MVC ist auf der MSDN-Website unter der folgenden URL verfügbar:

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Lernprogramme und weitere Informationen zu ASP.NET MVC sind auf der MVC 4-Seite der ASP.NET-Website ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)) verfügbar.

<a id="_Toc303253804"></a>
## <a name="support"></a>Support

Dies ist eine Vorschauversion und wird nicht offiziell unterstützt. Wenn Sie Fragen zum Arbeiten mit dieser Version haben, veröffentlichen Sie Sie im ASP.NET MVC-Forum ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), in dem Mitglieder der ASP.net-Community häufig informelle Unterstützung bieten können.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Softwareanforderungen

Die ASP.NET MVC 4-Komponenten für Visual Studio erfordern PowerShell 2,0 und entweder Visual Studio 2010 mit Service Pack 1 oder Visual Web Developer Express 2010 mit Service Pack 1.

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Aktualisieren eines ASP.NET MVC 3-Projekts auf ASP.NET MVC 4

ASP.NET MVC 4 kann parallel mit ASP.NET MVC 3 auf demselben Computer installiert werden. auf diese Weise können Sie flexibel entscheiden, wann eine ASP.NET MVC 3-Anwendung auf ASP.NET MVC 4 aktualisiert werden soll.

Die einfachste Möglichkeit, ein Upgrade durchzuführen, besteht darin, ein neues ASP.NET MVC 4-Projekt zu erstellen und alle Sichten, Controller, Code und Inhalts Dateien aus dem vorhandenen MVC 3-Projekt in das neue Projekt zu kopieren und dann die Assemblyverweise im neuen Projekt so zu aktualisieren, dass Sie dem alten Projekt entsprechen. Wenn Sie Änderungen an der Datei "Web. config" im MVC 3-Projekt vorgenommen haben, müssen Sie diese Änderungen auch in der Datei "Web. config" im MVC 4-Projekt zusammenführen.

Gehen Sie folgendermaßen vor, um eine vorhandene ASP.NET MVC 3-Anwendung manuell auf Version 4 zu aktualisieren:

1. Ersetzen Sie in allen Web. config-Dateien im Projekt (es gibt eine im Stammverzeichnis des Projekts, eine im Ordner "Views" und eine im Ordner "Views" für jeden Bereich im Projekt), und ersetzen Sie jede Instanz des folgenden Texts:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample1.cmd)]

    mit dem folgenden entsprechenden Text:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample2.cmd)]
2. Aktualisieren Sie in der Web. config-Stammdatei das Element " *Webseiten: Version* " auf "2.0.0.0", und fügen Sie einen neuen *preserveloginurl* -Schlüssel mit dem Wert "true" hinzu:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample3.xml)]
3. Löschen Sie in Projektmappen-Explorer den Verweis auf *System. Web. MVC* (der auf die dll der Version 3 verweist). Fügen Sie dann einen Verweis auf *System. Web. MVC* (v 4.0.0.0) hinzu. Nehmen Sie insbesondere die folgenden Änderungen vor, um die Assemblyverweise zu aktualisieren. Es folgen die Details:

    1. Löschen Sie in Projektmappen-Explorer die Verweise auf die folgenden Assemblys: 

        - *System. Web. MVC*(v 3.0.0.0)
        - *System. Web. Webseiten*(v 1.0.0.0)
        - *System. Web. Razor*(v 1.0.0.0)
        - *System. Web. Webseiten. Deployment*(v 1.0.0.0)
        - *System. Web. Webseiten. Razor*(v 1.0.0.0)
    2. Fügen Sie Verweise auf die folgenden Assemblys hinzu: 

        - *System. Web. MVC*(v 4.0.0.0)
        - *System. Web. Webseiten*(v 2.0.0.0)
        - *System. Web. Razor*(v 2.0.0.0)
        - *System. Web. Webseiten. Deployment*(v 2.0.0.0)
        - *System. Web. Webseiten. Razor*(v 2.0.0.0)
4. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen, und wählen Sie dann Projekt entladen aus. Klicken Sie dann mit der rechten Maustaste erneut auf den Namen, und wählen Sie *Projektname*. csproj bearbeiten aus.
5. Suchen Sie nach dem *projecttypguids* -Element, und ersetzen Sie {E53F8FEA-EAE0-44A6-8774-FFD645390401} durch {E3E379DF-F4C6-4180-9B81-6769533ABE47}.
6. Speichern Sie die Änderungen, schließen Sie die Projektdatei (. csproj), die Sie bearbeitet haben, klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie dann Projekt erneut laden.
7. Wenn das Projekt auf Bibliotheken von Drittanbietern verweist, die mit früheren Versionen von ASP.NET MVC kompiliert wurden, öffnen Sie die Web. config-Stammdatei, und fügen Sie die folgenden drei *bindingRedirect* -Elemente im *Konfigurations* Abschnitt hinzu: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample4.xml)]

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4-beta"></a>Neue Features in ASP.NET MVC 4 Beta

In diesem Abschnitt werden die Funktionen beschrieben, die in der ASP.NET MVC 4 Beta-Version eingeführt wurden.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>ASP.NET-Web-API

ASP.NET MVC 4 enthält jetzt ASP.net-Web-API, ein neues Framework zum Erstellen von http-Diensten, die eine breite Palette von Clients erreichen können, darunter auch Browser und mobile Geräte. ASP.net-Web-API ist auch eine ideale Plattform zum Aufbauen von Rest-Diensten.

ASP.net-Web-API bietet Unterstützung für die folgenden Features:

- **Modernes HTTP-Programmiermodell:** Direktes Zugreifen auf und Bearbeiten von HTTP-Anforderungen und-Antworten in Ihren Web-APIs mithilfe eines neuen, stark typisierten http-Objektmodells. Das gleiche Programmiermodell und die gleiche HTTP-Pipeline sind auf dem Client über den neuen httpclient-Typ symmetrisch verfügbar.
- **Vollständige Unterstützung für Routen**: Web-APIs unterstützen jetzt den vollständigen Satz von Routen Funktionen, die immer Bestandteil des webstapels waren, einschließlich der Routen Parameter und Einschränkungen. Außerdem verfügt die Zuordnung zu Aktionen über eine vollständige Unterstützung für Konventionen, sodass Sie keine Attribute mehr wie [HttpPost] auf Ihre Klassen und Methoden anwenden müssen.
- **Inhalts Aushandlung**: der Client und der Server können zusammenarbeiten, um das richtige Format für Daten zu bestimmen, die von einer API zurückgegeben werden. Wir stellen standardmäßige Unterstützung für XML-, JSON-und Formular-URL-codierte Formate bereit. Sie können diese Unterstützung erweitern, indem Sie eigene Formatierer hinzufügen oder sogar die standardmäßige inhaltsaushandlungs-Strategie ersetzen.
- **Modell Bindung und-Validierung:** Modell Binder stellen eine einfache Möglichkeit dar, Daten aus verschiedenen Teilen einer HTTP-Anforderung zu extrahieren und diese Nachrichten Teile in .NET-Objekte zu konvertieren, die von den Web-API-Aktionen verwendet werden können.
- **Filter:** Web-APIs unterstützen jetzt Filter, einschließlich bekannter Filter, wie z. b. des [autorisieren]-Attributs. Sie können eigene Filter für Aktionen, Autorisierungen und Ausnahmebehandlung erstellen und einbinden.
- **Abfrage Komposition:** Durch einfaches zurückgeben von iquervable&lt;t&gt;unterstützt Ihre Web-API Abfragen über die odata URL-Konventionen.
- **Verbesserte Testability von http-Details:** Anstatt http-Details in statischen Kontext Objekten festzulegen, können Web-API-Aktionen jetzt mit Instanzen von httprequestmessage und HttpResponseMessage funktionieren. Generische Versionen dieser Objekte sind auch vorhanden, damit Sie zusätzlich zu den HTTP-Typen mit den benutzerdefinierten Typen arbeiten können.
- **Verbessertes Inversion of Control (IOC) über DependencyResolver:** Die Web-API verwendet nun das vom Abhängigkeits Konflikt Löser von MVC implementierte Dienstlocator-Muster, um Instanzen für viele verschiedene Funktionen zu erhalten.
- **Code basierte Konfiguration:** Die Web-API-Konfiguration wird ausschließlich über Code ausgeführt, sodass Ihre Konfigurationsdateien bereinigt werden.
- **Self-Host:** Web-APIs können zusätzlich zu IIS in Ihrem eigenen Prozess gehostet werden, während gleichzeitig die volle Leistungsfähigkeit von Routen und anderen Features der Web-API genutzt wird.

Weitere Informationen zu ASP.net-Web-API finden Sie unter [https://www.asp.net/web-api](../web-api/index.md).

<a id="_Toc317096198"></a>
### <a name="aspnet-single-page-application"></a>ASP.net Single-Page-Anwendung

ASP.NET MVC 4 enthält jetzt eine frühe Vorschau der Funktionen zum Entwickeln von Anwendungen mit nur einer Seite mit erheblichen Client seitigen Interaktionen mithilfe von JavaScript und Web-APIs. Diese Unterstützung umfasst Folgendes:

- Eine Reihe von JavaScript-Bibliotheken für umfassendere lokale Interaktionen mit zwischengespeicherten Daten
- Zusätzliche Web-API-Komponenten für Arbeitseinheit und Dal-Unterstützung
- Eine MVC-Projektvorlage mit Gerüstbau für den schnellen Einstieg

Weitere Informationen zur Unterstützung einzelner Seiten Anwendungen in ASP.NET MVC 4 finden Sie unter [https://www.asp.net/single-page-application](../single-page-application/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Verbesserungen an Standard Projektvorlagen

Die Vorlage, mit der neue ASP.NET MVC 4-Projekte erstellt werden, wurde aktualisiert, um eine modernere Website zu erstellen:

![](mvc4-beta-release-notes/_static/image1.png)

Zusätzlich zu den kosmetischen Verbesserungen gibt es eine verbesserte Funktionalität in der neuen Vorlage. Die Vorlage verwendet ein Verfahren, das als adaptives Rendering bezeichnet wird, um in Desktop Browsern und mobilen Browsern ohne Anpassung gut zu suchen.

![](mvc4-beta-release-notes/_static/image2.png)

Um das adaptive Rendering in Aktion zu sehen, können Sie einen mobilen Emulator verwenden oder einfach die Größe des Desktop Browserfensters ändern, sodass es kleiner ist. Wenn das Browserfenster klein genug ist, wird das Layout der Seite geändert.

Eine weitere Erweiterung der Standard Projektvorlage ist die Verwendung von JavaScript, um eine umfassendere Benutzeroberfläche bereitzustellen. Die in der Vorlage verwendeten Anmelde-und Registrierungs Links sind Beispiele für die Verwendung des jQuery-Dialog Felds "UI", um einen umfangreichen Anmeldebildschirm anzuzeigen:

![](mvc4-beta-release-notes/_static/image3.png)

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Mobile Projektvorlage

Wenn Sie ein neues Projekt starten und eine Website speziell für Mobile und Tablet-Browser erstellen möchten, können Sie die neue Projektvorlage für mobile Anwendungen verwenden. Dies basiert auf jQuery Mobile, einer Open-Source-Bibliothek zum Entwickeln einer Benutzeroberfläche mit Touchscreen:

![](mvc4-beta-release-notes/_static/image4.png)

Diese Vorlage enthält dieselbe Anwendungs Struktur wie die Internet Anwendungs Vorlage (und der Controller Code ist praktisch identisch), aber Sie wird mit jQuery Mobile formatiert, um sich auf Touchscreen-basierten mobilen Geräten gut anzusehen und sich gut zu Verhalten. Weitere Informationen zum Strukturieren und Formatieren der mobilen Benutzeroberfläche finden Sie auf der [Website zum jQuery Mobile Project](http://jquerymobile.com/).

Wenn Sie bereits über eine Desktop orientierte Website verfügen, der Sie Mobile optimierte Ansichten hinzufügen möchten, oder wenn Sie einen einzelnen Standort erstellen möchten, der unterschiedliche Ansichten für Desktop-und Mobile Browser bereitstellt, können Sie die neue Funktion Anzeigemodi verwenden. (Siehe nächster Abschnitt.)

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Anzeigemodi

Mit der neuen Funktion "Anzeigemodi" kann eine Anwendung in Abhängigkeit vom Browser, von dem die Anforderung stammt, Ansichten auswählen. Wenn ein Desktop Browser z. b. die Startseite anfordert, verwendet die Anwendung möglicherweise die Vorlage "views\home\index.cshtml". Wenn ein mobiler Browser die Startseite anfordert, kann die Anwendung die Views\Home\Index.Mobile.cshtml-Vorlage zurückgeben.

Layouts und partiale können auch für bestimmte Browsertypen überschrieben werden. Beispiel:

- Wenn der Ordner "views\shared" sowohl die Vorlagen "\_Layout. cshtml" als auch "\_Layout. Mobile. cshtml" enthält, verwendet die Anwendung standardmäßig \_Layout. Mobile. cshtml bei Anforderungen von mobilen Browsern und \_"Layout. cshtml" bei anderen Anforderungen.
- Wenn ein Ordner sowohl \_mypartial. cshtml als auch \_mypartial. Mobile. cshtml enthält, wird die Anweisung @Html.Partial("\_mypartiell") bei Anforderungen von mobilen Browsern \_"myparti. Mobile. cshtml" und bei anderen Anforderungen \_"meinteil. cshtml".

Wenn Sie spezifischere Sichten, Layouts oder Teilansichten für andere Geräte erstellen möchten, können Sie eine neue *defaultdisplaymode* -Instanz registrieren, um anzugeben, nach welchem Namen gesucht werden soll, wenn eine Anforderung bestimmte Bedingungen erfüllt. Beispielsweise können Sie den folgenden Code der *Anwendung\_Start* -Methode in der Datei "Global. asax" hinzufügen, um die Zeichenfolge "iPhone" als Anzeigemodus zu registrieren, der angewendet wird, wenn der Apple iPhone-Browser eine Anforderung sendet:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample5.cs)]

Wenn dieser Code ausgeführt wird, verwendet die Anwendung das Layout views\shared\\_Layout. iPhone. cshtml, wenn Sie eine Anforderung ausführt.

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-the-view-switcher-and-browser-overriding"></a>jQuery Mobile, der Ansichts Switcher und Browser Überschreibung

jQuery Mobile ist eine Open-Source-Bibliothek zum Entwickeln einer Touchscreen-optimierten Webbenutzer Oberfläche. Wenn Sie jQuery Mobile mit einer ASP.NET MVC 4-Anwendung verwenden möchten, können Sie ein nuget-Paket herunterladen und installieren, das Ihnen den Einstieg erleichtert. Um es über die Visual Studio-Paket-Manager-Konsole zu installieren, geben Sie den folgenden Befehl ein:

[!code-powershell[Main](mvc4-beta-release-notes/samples/sample6.ps1)]

Hierdurch werden jQuery Mobile und einige Hilfsdateien installiert, einschließlich der folgenden:

- Views/Shared/\_Layout. Mobile. cshtml, bei dem es sich um ein auf jQuery mobiles basiertes Layout handelt.
- Eine Ansichts switcherkomponente, die aus der Teilansicht views/Shared/\_viewswitcher. cshtml und dem ViewSwitcherController.cs-Controller besteht.

Nachdem Sie das Paket installiert haben, führen Sie die Anwendung über einen mobilen Browser (oder eine entsprechende Anwendung wie das Add-on für den Firefox- [Benutzer-Agent-Switcher](http://chrispederick.com/work/user-agent-switcher/) ) aus. Sie werden feststellen, dass Ihre Seiten ganz anders aussehen, da jQuery Mobile Layout und Formatierung übernimmt. Um dies zu nutzen, können Sie Folgendes tun:

- Erstellen Sie Mobile-spezifische außer Kraft setzungen der Ansicht, wie unter [Anzeigemodi](#_Toc303253810) zuvor beschrieben (z. b. Create Views\Home\Index.Mobile.cshtml, um views\home\index.cshtml für Mobile Browser zu überschreiben).
- Weitere Informationen zum Hinzufügen von Berührungs optimierten UI-Elementen in mobilen Ansichten finden Sie in der [Dokumentation zu jQuery Mobile](http://jquerymobile.com/) .

Eine Konvention für Mobilgeräte optimierte Webseiten besteht darin, einen Link hinzuzufügen, dessen Text etwas wie die Desktop Ansicht oder der vollständige Website Modus ist, mit dem Benutzer zu einer Desktop Version der Seite wechseln können. Das Paket "jQuery. Mobile. MVC" enthält hierfür eine Beispiel-switcherkomponente. Sie wird in der Standardansicht views\shared\\_Layout. Mobile. cshtml verwendet und sieht wie folgt aus, wenn die Seite gerendert wird:

![](mvc4-beta-release-notes/_static/image5.png)

Wenn Besucher auf den Link klicken, werden Sie auf die Desktop Version der gleichen Seite umgestellt.

Da Ihr Desktop Layout standardmäßig keinen Ansichts Umschalter enthält, haben Besucher keine Möglichkeit, in den mobilen Modus zu gelangen. Um dies zu ermöglichen, fügen Sie *\_viewswitcher* den folgenden Verweis auf das Desktop Layout ein, direkt innerhalb des *&lt;Text&gt;* Elements:

[!code-cshtml[Main](mvc4-beta-release-notes/samples/sample7.cshtml)]

Der Ansichts Umschaltung verwendet ein neues Feature namens Browser Überschreibung. Diese Funktion ermöglicht Ihrer Anwendung, Anforderungen so zu behandeln, als würden Sie von einem anderen Browser (Benutzer-Agent) stammen als dem, von dem Sie tatsächlich stammen. In der folgenden Tabelle werden die Methoden aufgelistet, die vom Browser überschrieben werden.

| `HttpContext.SetOverriddenBrowser(userAgentString)` | Überschreibt den tatsächlichen Benutzer-Agent-Wert der Anforderung unter Verwendung des angegebenen Benutzer-Agents. |
| --- | --- |
| `HttpContext.GetOverriddenUserAgent()` | Gibt den Überschreibungs Wert des Benutzer-Agents der Anforderung oder die tatsächliche Benutzer-Agent-Zeichenfolge zurück, wenn keine außer Kraft Setzung angegeben wurde. |
| `HttpContext.GetOverriddenBrowser()` | Gibt eine *HttpBrowserCapabilitiesBase* -Instanz zurück, die dem Benutzer-Agent entspricht, der zurzeit für die Anforderung festgelegt ist (tatsächlich oder überschrieben). Sie können diesen Wert verwenden, um Eigenschaften wie *IsMobileDevice*zu erhalten. |
| `HttpContext.ClearOverriddenBrowser()` | Entfernt alle überschriebenen Benutzer-Agents für die aktuelle Anforderung. |

Die Überschreibung des Browsers ist ein zentrales Feature von ASP.NET MVC 4 und auch dann verfügbar, wenn Sie das Paket "jQuery. Mobile. MVC" nicht installieren. Dies wirkt sich jedoch nur auf Sicht, Layout und partielle Anzeige aus – Sie wirkt sich nicht auf andere ASP.NET-Funktionen aus, die vom *Request. Browser* -Objekt abhängen.

Standardmäßig wird die Benutzer-Agent-außer Kraft setzung mit einem Cookie gespeichert. Wenn Sie die außer Kraft Setzung an einem anderen Speicherort speichern möchten (z. b. in einer Datenbank), können Sie den Standardanbieter (*browseroverridebug. Current*) ersetzen. Die Dokumentation für diesen Anbieter steht für eine spätere Version von ASP.NET MVC zur Verfügung.

<a id="_Toc303253812"></a>
### <a name="recipes-for-code-generation-in-visual-studio"></a>Rezepte für die Code Generierung in Visual Studio

Mit der neuen Funktion "Rezepte" können Sie Visual Studio auf der Grundlage von Paketen, die Sie mithilfe von nuget installieren können, Lösungs spezifischen Code generieren. Das Framework "Rezepte" vereinfacht Entwicklern das Schreiben von Plug-Ins für die Codegenerierung, mit denen Sie auch die integrierten Code Generatoren für "Bereich hinzufügen", "Controller hinzufügen" und "Ansicht hinzufügen" ersetzen können. Da Rezepte als nuget-Pakete bereitgestellt werden, können Sie problemlos in die Quell Code Verwaltung eingecheckt und automatisch für alle Entwickler im Projekt freigegeben werden. Sie sind auch pro Lösung verfügbar.

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Aufgaben Unterstützung für asynchrone Controller

Sie können jetzt asynchrone Aktionsmethoden als einzelne Methoden schreiben, die ein Objekt des Typs *Task* oder *Task&lt;Action result&gt;* zurückgeben.

Wenn Sie z. b. Visual C# 5 verwenden (oder die CTP-Version [Async](https://msdn.microsoft.com/vstudio/async.aspx)verwenden), können Sie eine asynchrone Aktionsmethode erstellen, die wie folgt aussieht:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample8.cs)]

In der vorherigen Aktionsmethode werden die Aufrufe von *Newsservice. gezeige adlinesasync* und *sportsservice. getscoresasync* asynchron aufgerufen und blockieren keinen Thread aus dem Thread Pool.

Asynchrone Aktionsmethoden, die *Task* Instanzen zurückgeben, können auch Timeouts unterstützen. Um die Aktionsmethode abbrechbar zu machen, fügen Sie der Aktionsmethoden Signatur einen Parameter vom Typ *CancellationToken* hinzu. Das folgende Beispiel zeigt eine asynchrone Aktionsmethode mit einem Timeout von 2500 Millisekunden, die eine *TimedOut* -Ansicht für den Client anzeigt, wenn ein Timeout auftritt.

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample9.cs)]

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK

ASP.NET MVC 4 Beta unterstützt die Microsoft Azure SDK-Version vom September 2011 1,5.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Bekannte Probleme und wichtige Änderungen

- **Nach der Installation von ASP.NET MVC 4 Beta kann der CSHTML/VBHTML-Editor in Visual Studio 2010 Service Pack 1 CSHTML/VBHTML-Editor lange Zeit anhalten, nachdem Code Ausschnitt oder JavaScript in CSHTML-oder VBHTML-Dateien eingegeben wurde.** Dies geschieht nur in ASP.NET MVC 4-Anwendungen, die soeben erstellt wurden und noch nicht kompiliert wurden.

    Die Problem Umgehung besteht darin, das Projekt zu kompilieren, um die Assemblys im bin-Ordner zu erhalten. Beachten Sie Folgendes: Wenn Sie das Projekt bereinigen, das die Assemblys aus dem Ordner "bin" entfernt, wird das Editor Problem zurückgegeben.

    Dies wird in der nächsten Version korrigiert.
- **C#Projektvorlagen für Visual Studio 11 Beta enthalten in Global.asax.cs eine falsche Verbindungs Zeichenfolge.** Die in der Anwendung\_Start-Methode für Projekte, die in Visual Studio 11 Beta erstellt wurden, enthält eine localdb-Verbindungs Zeichenfolge, die einen umgekehrten Schrägstrich ohne Escapezeichen (\) Zeichen enthält. Dies führt zu einem Verbindungsfehler, wenn versucht wird, auf einen Entity Framework dbcontext zuzugreifen, der eine SqlException generiert.

    Um dieses Problem zu beheben, müssen Sie den umgekehrten Schrägstrich in der APP\_Start-Methode von Global.asax.cs mit Escapezeichen versehen, damit er wie folgt aussieht:

    [!code-csharp[Main](mvc4-beta-release-notes/samples/sample10.cs)]
- **ASP.NET MVC 4-Anwendungen, die auf .NET 4,5 abzielen, lösen eine FileLoadException aus, wenn versucht wird, auf die Assembly "System. net. http. dll" zuzugreifen, wenn Sie unter .NET 4,0 ausgeführt wird.** ASP.NET MVC 4-Anwendungen, die unter .NET 4,5 erstellt werden, enthalten eine Bindungs Umleitung, die zu einer FileLoadException führt, die besagt, dass die Datei oder Assembly "System .net. http" oder eine ihrer Abhängigkeiten nicht geladen werden konnte. " Wenn die Anwendung auf einem System ausgeführt wird, auf dem .NET 4,0 installiert ist. Entfernen Sie die folgende Bindungs Umleitung aus "Web. config", um dieses Problem zu beheben:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample11.xml)]

    Das assemblybindungselement in der geänderten Datei Web. config sollte folgendermaßen aussehen:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample12.xml)]
- <strong>Die Element Vorlage "Controller hinzufügen" in Visual Basic Projekten generiert einen falschen Namespace, wenn Sie</strong><strong>innerhalb eines Bereichs</strong> aufgerufen wird. Wenn Sie einem Bereich in einem ASP.NET-MVC-Projekt, das Visual Basic verwendet, einen Controller hinzufügen, fügt die Element Vorlage den falschen Namespace in den Controller ein. Das Ergebnis ist der Fehler "Datei nicht gefunden", wenn Sie zu einer Aktion im Controller navigieren.  
  
  Der generierte Namespace lässt alles nach dem Stamm Namespace aus. Der generierte Namespace ist z. b. *RootNamespace* , sollte aber *RootNamespace. Areas. AreaName. Controllers* lauten.
- **Wichtige Änderungen in der Razor-Ansichts-Engine.** Im Rahmen einer Neuschreibung des Razor-Parsers wurden die folgenden Typen aus *System. Web. MVC. Razor*entfernt: 

    - *Modelspan*
    - *Mvcvbrazorcodegenerator*
    - *Mvccsharprazorcodegenerator*
    - *Mvcvbrazorcodeparser*

  Die folgenden Methoden wurden ebenfalls entfernt: 

    - *Mvccsharprazorcodeparser. parseinverersstatement (System. Web. Razor. Parser. Codeblock Info)*
    - *MvcWebPageRazorHost. decoratecodegenerator (System. Web. Razor. Generator. razorcodegenerator)*
    - *Mvcvbrazorcodeparser. parseinverersstatement (System. Web. Razor. Parser. Codeblock Info)*
- **Wenn webmatrix. WebData. dll im Verzeichnis "/bin" einer ASP.NET MVC 4-APP enthalten ist, übernimmt es die URL für die Formular Authentifizierung.** Wenn Sie der Anwendung die webmatrix. WebData. dll-Assembly hinzufügen (z. b. durch Auswählen von "ASP.net Web Pages mit Razor-Syntax" bei Verwendung des Dialog Felds "bereit Stellungen mit aktivierbaren Abhängigkeiten hinzufügen"), wird die Authentifizierung bei der Authentifizierungs Anmeldung an/Account/Logon anstatt an/Account/Login überschrieben. Um dieses Verhalten zu verhindern und die bereits im Abschnitt Authentifizierung von Web. config angegebene URL zu verwenden, können Sie eine appSetting namens preserveloginurl hinzufügen und diese auf "true" festlegen: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample13.xml)]
- **Der nuget-Paket-Manager kann nicht installiert werden, wenn Sie versuchen, ASP.NET MVC 4 für parallele Installationen von Visual Studio 2010 und Visual Web Developer 2010 zu installieren.** Wenn Sie Visual Studio 2010 und Visual Web Developer 2010 nebeneinander mit ASP.NET MVC 4 ausführen möchten, müssen Sie ASP.NET MVC 4 installieren, nachdem beide Versionen von Visual Studio bereits installiert wurden.
- **Das Deinstallieren von ASP.NET MVC 4 schlägt fehl, wenn die erforderlichen Komponenten bereits deinstalliert wurden.** Zum ordnungsgemäßen Deinstallieren von ASP.NET MVC 4müssen Sie ASP.NET MVC 4 deinstallieren, bevor Sie Visual Studio deinstallieren.
- **Beim Ausführen eines standardmäßigen Web-API-Projekts werden Anweisungen angezeigt, die den Benutzer fälschlicherweise zum Hinzufügen von Routen mithilfe der registerapis-Methode, die nicht vorhanden ist,** Routen sollten in der RegisterRoutes-Methode mithilfe der Routing Tabelle ASP.net hinzugefügt werden.
- **Installieren von ASP.NET MVC 4 Beta-Unterbrechungen ASP.NET MVC 3 RTM-Anwendungen.** ASP.NET MVC 3-Anwendungen, die mit der RTM-Version (nicht mit der ASP.NET MVC 3 Tools Update-Version) erstellt wurden, erfordern die folgenden Änderungen, um parallel mit ASP.NET MVC 4 Beta arbeiten zu können. Das Erstellen des Projekts ohne diese Updates führt zu Kompilierungs Fehlern. 

    **Erforderliche Updates**

  1. Fügen Sie in der Web. config-Stammdatei einen neuen *&lt;appSettings-&gt;* Eintrag mit dem Schlüssel *Webseiten: Version* und dem Wert *1.0.0.0*hinzu.

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample14.xml)]
  2. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen, und wählen Sie dann Projekt entladen aus. Klicken Sie dann mit der rechten Maustaste erneut auf den Namen, und wählen Sie *Projektname*. csproj bearbeiten aus.
  3. Suchen Sie die folgenden Assemblyverweise: 

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample15.xml)]

      Ersetzen Sie Sie durch Folgendes:

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample16.xml)]
  4. Speichern Sie die Änderungen, schließen Sie die Projektdatei (. csproj), die Sie bearbeitet haben, klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie erneut laden aus.
