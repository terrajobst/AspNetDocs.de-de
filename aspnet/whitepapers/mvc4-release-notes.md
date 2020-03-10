---
uid: whitepapers/mvc4-release-notes
title: ASP.NET MVC 4 | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Dokument wird die Veröffentlichung von ASP.NET MVC 4 beschrieben.
ms.author: riande
ms.date: 09/09/2011
ms.assetid: f014524f-25c0-4094-b8e1-886d99536f00
msc.legacyurl: /whitepapers/mvc4-release-notes
msc.type: content
ms.openlocfilehash: b57480bd0274fbb76c600dfb0dd09037bdcbf1e7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78454233"
---
# <a name="aspnet-mvc-4"></a>ASP.NET MVC 4

> In diesem Dokument wird die Veröffentlichung von ASP.NET MVC 4 beschrieben.

- [Installationshinweise](#_Toc303253802)
- [Dokumentation](#_Toc303253803)
- [Unterstützung](#_Toc303253804)
- [Software Anforderungen](#_Toc303253805)
- [Neue Features in ASP.NET MVC 4](#_Toc303253807)

    - [ASP.NET-Web-API](#_Toc317096197)
    - [Verbesserungen an Standard Projektvorlagen](#_Toc303253808)
    - [Mobile Projektvorlage](#_Toc303253809)
    - [Anzeigemodi](#_Toc303253810)
    - [jQuery Mobile, der Ansichts Switcher und Browser Überschreibung](#_Toc303253811)
    - [Aufgaben Unterstützung für asynchrone Controller](#_Toc303253813)
    - [Azure SDK](#_Toc303253814)
    - [Datenbankmigrationen](#_Toc303253818)
    - [Leere Projektvorlage](#_Toc303253819)
    - [Controller zu einem beliebigen Projektordner hinzufügen](#_Toc303253820)
    - [Bündelung und Minimierung](#_Toc303253821)
    - [Aktivieren von Anmeldungen von Facebook und anderen Websites mithilfe von OAuth und OpenID](#_Toc303253822)
- [Aktualisieren eines ASP.NET MVC 3-Projekts auf ASP.NET MVC 4](#_Toc303253806)
- [Änderungen von ASP.NET MVC 4 Release Candidate](#_Toc303253817)
- [Bekannte Probleme und wichtige Änderungen](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Installationshinweise

ASP.NET MVC 4 für Visual Studio 2010 kann mithilfe des Webplattform-Installers auf der [ASP.NET MVC 4-Startseite](../mvc/mvc4.md) installiert werden.

Es wird empfohlen, vor der Installation von ASP.NET MVC 4 eine zuvor installierte Vorschau von ASP.NET MVC 4 zu deinstallieren. Sie können ASP.NET MVC 4 Beta und Release Candidate auf ASP.NET MVC 4 aktualisieren, ohne zu deinstallieren.

Diese Version ist mit den Vorschau Versionen von .NET Framework 4,5 nicht kompatibel. Sie müssen alle installierten Vorschau Versionen von .NET Framework 4,5 separat auf die endgültige Version aktualisieren, bevor Sie ASP.NET MVC 4 installieren.

ASP.NET MVC 4 kann parallel mit ASP.NET MVC 3 installiert und ausgeführt werden.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Dokumentation

Die Dokumentation für ASP.NET MVC ist auf der MSDN-Website unter der folgenden URL verfügbar:

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Lernprogramme und weitere Informationen zu ASP.NET MVC sind auf der MVC 4-Seite der ASP.NET-Website ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)) verfügbar.

<a id="_Toc303253804"></a>
## <a name="support"></a>Support

ASP.NET MVC 4 wird vollständig unterstützt. Wenn Sie Fragen zum Arbeiten mit dieser Version haben, können Sie Sie auch im ASP.NET MVC-Forum ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)) veröffentlichen, in dem Mitglieder der ASP.net-Community häufig informelle Unterstützung bieten können.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Softwareanforderungen

Die ASP.NET MVC 4-Komponenten für Visual Studio erfordern PowerShell 2,0 und entweder Visual Studio 2010 mit Service Pack 1 oder Visual Web Developer Express 2010 mit Service Pack 1.

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4"></a>Neue Features in ASP.NET MVC 4

In diesem Abschnitt werden die Funktionen beschrieben, die in der ASP.NET MVC 4-Version eingeführt wurden.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>ASP.NET-Web-API

ASP.NET MVC 4 enthält ASP.net-Web-API, ein neues Framework zum Erstellen von http-Diensten, die eine breite Palette von Clients erreichen können, darunter auch Browser und mobile Geräte. ASP.net-Web-API ist auch eine ideale Plattform zum Aufbauen von Rest-Diensten.

ASP.net-Web-API bietet Unterstützung für die folgenden Features:

- **Modernes HTTP-Programmiermodell:** Direktes Zugreifen auf und Bearbeiten von HTTP-Anforderungen und-Antworten in Ihren Web-APIs mithilfe eines neuen, stark typisierten http-Objektmodells. Das gleiche Programmiermodell und die gleiche HTTP-Pipeline sind auf dem Client über den neuen *HttpClient* -Typ symmetrisch verfügbar.
- **Vollständige Unterstützung für Routen:** ASP.net-Web-API unterstützt den vollständigen Satz von Routen Funktionen des ASP.NET-Routings, einschließlich Routen Parameter und Einschränkungen. Verwenden Sie außerdem einfache Konventionen zum Zuordnen von Aktionen zu HTTP-Methoden.
- **Inhaltsaushandlung:** Der Client und der Server können zusammenarbeiten, um das richtige Format für Daten zu bestimmen, die von einer Web-API zurückgegeben werden. ASP.net-Web-API bietet standardmäßige Unterstützung für XML-, JSON-und Formular-URL-codierte Formate. Sie können diese Unterstützung erweitern, indem Sie eigene Formatierer hinzufügen oder sogar die standardmäßige inhaltsaushandlungs-Strategie ersetzen.
- **Modell Bindung und-Validierung:** Modell Binder stellen eine einfache Möglichkeit dar, Daten aus verschiedenen Teilen einer HTTP-Anforderung zu extrahieren und diese Nachrichten Teile in .NET-Objekte zu konvertieren, die von den Web-API-Aktionen verwendet werden können. Die Überprüfung wird auch für Aktionsparameter auf der Grundlage von Daten Anmerkungen ausgeführt.
- **Filter:** ASP.net-Web-API unterstützt Filter, einschließlich bekannter Filter, z. b. des *[Autorisierungs]* -Attributs. Sie können eigene Filter für Aktionen, Autorisierungen und Ausnahmebehandlung erstellen und einbinden.
- **Abfrage Komposition:** Verwenden Sie das *[quervable]* Filter-Attribut für eine Aktion, die *iquervable* zurückgibt, um die Unterstützung für das Abfragen Ihrer Web-API über die odata-Abfrage Konventionen zu aktivieren.
- **Verbesserte Testability:** Anstatt http-Details in statischen Kontext Objekten festzulegen, funktionieren Web-API-Aktionen mit Instanzen von *httprequestmessage* und *HttpResponseMessage*. Erstellen Sie ein Komponenten Testprojekt zusammen mit Ihrem Web-API-Projekt, um mit dem schnellen Schreiben von Komponententests für Ihre Web-API-Funktionalität zu beginnen.
- **Code basierte Konfiguration:** ASP.net-Web-API Konfiguration wird ausschließlich über Code durchgeführt, sodass Ihre Konfigurationsdateien bereinigt werden. Verwenden Sie das bereitgestellte dienstlocatormuster, um Erweiterbarkeits Punkte zu konfigurieren.
- **Verbesserte Unterstützung für IOC-Container (Inversion of Control):** ASP.net-Web-API bietet hervorragend Unterstützung für IOC-Container durch eine verbesserte Abhängigkeits Konflikt Löser-Abstraktion.
- **Self-Host:** Web-APIs können zusätzlich zu IIS in Ihrem eigenen Prozess gehostet werden, während gleichzeitig die volle Leistungsfähigkeit von Routen und anderen Features der Web-API genutzt wird.
- **Erstellen Sie benutzerdefinierte Hilfe-und Testseiten:** Sie können jetzt problemlos benutzerdefinierte Hilfe-und Testseiten für Ihre Web-APIs erstellen, indem Sie den neuen *iapiexplorer* -Dienst verwenden, um eine umfassende Lauf Zeit Beschreibung Ihrer Web-APIs zu erhalten.
- **Überwachung und Diagnose:** ASP.net-Web-API bietet nun eine einfache Ablauf Verfolgungs Infrastruktur, mit der die Integration in vorhandene Protokollierungs Lösungen wie System. Diagnostics, etw und Protokollierungs Frameworks von Drittanbietern vereinfacht wird. Sie können die Ablauf Verfolgung aktivieren, indem Sie eine *itracewriter* -Implementierung bereitstellen und Sie der Web-API-Konfiguration hinzufügen.
- **Link Generierung:** Verwenden Sie die ASP.net-Web-API *Urlhelper* , um Links zu verwandten Ressourcen in derselben Anwendung zu generieren.
- **Web-API-Projektvorlage:** Wählen Sie das neue Web-API-Projekt aus, ASP.net-Web-API und wählen Sie den Assistenten für neue MVC 4-Projekte aus.
- **Gerüst:** Verwenden Sie das Dialogfeld **Controller hinzufügen** , um schnell einen Web-API-Controller basierend auf einem Entity Framework basierten Modelltyp zu Gerüstbau.

Weitere Informationen zu ASP.net-Web-API finden Sie unter [https://www.asp.net/web-api](../web-api/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Verbesserungen an Standard Projektvorlagen

Die Vorlage, mit der neue ASP.NET MVC 4-Projekte erstellt werden, wurde aktualisiert, um eine modernere Website zu erstellen:

![](mvc4-release-notes/_static/image1.png)

Zusätzlich zu den kosmetischen Verbesserungen gibt es eine verbesserte Funktionalität in der neuen Vorlage. Die Vorlage verwendet ein Verfahren, das als adaptives Rendering bezeichnet wird, um in Desktop Browsern und mobilen Browsern ohne Anpassung gut zu suchen.

![](mvc4-release-notes/_static/image2.png)

Um das adaptive Rendering in Aktion zu sehen, können Sie einen mobilen Emulator verwenden oder einfach die Größe des Desktop Browserfensters ändern, sodass es kleiner ist. Wenn das Browserfenster klein genug ist, wird das Layout der Seite geändert.

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Mobile Projektvorlage

Wenn Sie ein neues Projekt starten und eine Website speziell für Mobile und Tablet-Browser erstellen möchten, können Sie die neue Projektvorlage für mobile Anwendungen verwenden. Dies basiert auf jQuery Mobile, einer Open-Source-Bibliothek zum Entwickeln einer Benutzeroberfläche mit Touchscreen:

![](mvc4-release-notes/_static/image3.png)

Diese Vorlage enthält dieselbe Anwendungs Struktur wie die Internet Anwendungs Vorlage (und der Controller Code ist praktisch identisch), aber Sie wird mit jQuery Mobile formatiert, um sich auf Touchscreen-basierten mobilen Geräten gut anzusehen und sich gut zu Verhalten. Weitere Informationen zum Strukturieren und Formatieren der mobilen Benutzeroberfläche finden Sie auf der [Website zum jQuery Mobile Project](http://jquerymobile.com/).

Wenn Sie bereits über eine Desktop orientierte Website verfügen, der Sie Mobile optimierte Ansichten hinzufügen möchten, oder wenn Sie einen einzelnen Standort erstellen möchten, der unterschiedliche Ansichten für Desktop-und Mobile Browser bereitstellt, können Sie die neue Funktion Anzeigemodi verwenden. (Siehe nächster Abschnitt.)

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Anzeigemodi

Mit der neuen Funktion "Anzeigemodi" kann eine Anwendung in Abhängigkeit vom Browser, von dem die Anforderung stammt, Ansichten auswählen. Wenn ein Desktop Browser z. b. die Startseite anfordert, verwendet die Anwendung möglicherweise die Vorlage "views\home\index.cshtml". Wenn ein mobiler Browser die Startseite anfordert, kann die Anwendung die Views\Home\Index.Mobile.cshtml-Vorlage zurückgeben.

Layouts und partiale können auch für bestimmte Browsertypen überschrieben werden. Beispiel:

- Wenn der Ordner "views\shared" sowohl die Vorlagen "\_Layout. cshtml" als auch "\_Layout. Mobile. cshtml" enthält, verwendet die Anwendung standardmäßig \_Layout. Mobile. cshtml bei Anforderungen von mobilen Browsern und \_"Layout. cshtml" bei anderen Anforderungen.
- Wenn ein Ordner sowohl \_mypartial. cshtml als auch \_mypartial. Mobile. cshtml enthält, wird die Anweisung @Html.Partial("\_mypartiell") bei Anforderungen von mobilen Browsern \_"myparti. Mobile. cshtml" und bei anderen Anforderungen \_"meinteil. cshtml".

Wenn Sie spezifischere Sichten, Layouts oder Teilansichten für andere Geräte erstellen möchten, können Sie eine neue *defaultdisplaymode* -Instanz registrieren, um anzugeben, nach welchem Namen gesucht werden soll, wenn eine Anforderung bestimmte Bedingungen erfüllt. Beispielsweise können Sie den folgenden Code der *Anwendung\_Start* -Methode in der Datei "Global. asax" hinzufügen, um die Zeichenfolge "iPhone" als Anzeigemodus zu registrieren, der angewendet wird, wenn der Apple iPhone-Browser eine Anforderung sendet:

[!code-csharp[Main](mvc4-release-notes/samples/sample1.cs)]

Wenn dieser Code ausgeführt wird, verwendet die Anwendung das Layout views\shared\\_Layout. iPhone. cshtml, wenn Sie eine Anforderung ausführt. Weitere Informationen zum Anzeigemodus finden Sie unter [ASP.NET MVC 4 Mobile Features](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md). Anwendungen, die displaymodeprovider verwenden, sollten das Fixed-nuget-Paket " [DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) " installieren. Das [ASP.net Fall 2012-Update](https://go.microsoft.com/fwlink/?LinkID=271322) umfasst das [Fixed-DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) -nuget-Paket in den neuen Projektvorlagen. Ausführliche Informationen zur Behebung finden Sie unter [ASP.NET MVC 4 Mobile Caching Bug fixedd](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) .

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-and-mobile-features"></a>Mobile und mobile Features von jQuery

Informationen zum Entwickeln mobiler Anwendungen mit ASP.NET MVC 4 mithilfe von jQuery Mobile finden Sie im Tutorial [ASP.NET Mobile Features von MVC 4](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md).

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Aufgaben Unterstützung für asynchrone Controller

Sie können jetzt asynchrone Aktionsmethoden als einzelne Methoden schreiben, die ein Objekt des Typs *Task* oder *Task&lt;Action result&gt;* zurückgeben.

 Weitere Informationen finden [Sie unter Verwenden von asynchronen Methoden in ASP.NET MVC 4](../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK

ASP.NET MVC 4 unterstützt die Versionen 1,6 und neueren Versionen des Windows Azure SDK.

<a id="_Toc303253818"></a>
### <a name="database-migrations"></a>Datenbankmigrationen

ASP.NET MVC 4-Projekte enthalten jetzt Entity Framework 5. Eine der großartigen Features in Entity Framework 5 ist die Unterstützung von Datenbankmigrationen. Mit dieser Funktion können Sie Ihr Datenbankschema auf einfache Weise mithilfe einer Code orientierten Migration weiterentwickeln, während die Daten in der Datenbank beibehalten werden. Weitere Informationen zu Datenbankmigrationen finden [Sie unter Hinzufügen eines neuen Felds zum Film Modell und zur Tabelle](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table.md) im [Tutorial Einführung in ASP.NET MVC 4](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

<a id="_Toc303253819"></a>
### <a name="empty-project-template"></a>Leere Projektvorlage

Die Vorlage für das leere MVC-Projekt ist nun wirklich leer, sodass Sie von einem vollständig sauberen Slate aus beginnen können. Die frühere Version der leeren Projektvorlage wurde in Basic umbenannt.

<a id="_Toc303253820"></a>
### <a name="add-controller-to-any-project-folder"></a>Controller zu einem beliebigen Projektordner hinzufügen

Sie können nun mit der rechten Maustaste klicken und Controller aus einem beliebigen Ordner in Ihrem MVC-Projekt **Hinzufügen** auswählen. Dadurch erhalten Sie mehr Flexibilität bei der Organisation ihrer Controller, auch wenn Sie Ihre MVC-und Web-API-Controller in separaten Ordnern aufbewahren möchten.

<a id="_Toc303253821"></a>
### <a name="bundling-and-minification"></a>Bündelung und Minimierung

Mit dem Framework zum bündeln und minimieren können Sie die Anzahl von HTTP-Anforderungen verringern, die eine Webseite vornehmen muss, indem Sie einzelne Dateien in einer einzelnen, gebündelten Datei für Skripts und CSS kombinieren. Die Gesamtgröße dieser Anforderungen kann dann verringert werden, indem der Inhalt des Pakets minimiert wird. Die minierung kann z. b. Aktivitäten wie das Entfernen von Leerzeichen zum Verkürzen von Variablennamen und das Reduzieren von CSS-Selektoren basierend auf ihrer Semantik einschließen. Bündel werden im Code deklariert und konfiguriert, und in Sichten kann auf einfache Weise über Hilfsmethoden verwiesen werden, die entweder einen einzelnen Link zum Bundle generieren oder beim Debuggen mehrere Links zum individuellen Inhalt des Pakets generieren können. Weitere Informationen finden Sie unter [Bündelung und Minimierung](../mvc/overview/performance/bundling-and-minification.md).

<a id="_Toc303253822"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Aktivieren von Anmeldungen von Facebook und anderen Websites mithilfe von OAuth und OpenID

Die Standardvorlagen in der ASP.NET MVC 4-Internet Projektvorlage unterstützen jetzt die OAuth-und OpenID-Anmeldung mithilfe der dotnetopenauth-Bibliothek. Weitere Informationen zum Konfigurieren eines OAuth-oder OpenID-Anbieters finden Sie [unter OAuth/OpenID-Unterstützung für WebForms, MVC und Webseiten](https://blogs.msdn.com/b/webdev/archive/2012/08/15/oauth-openid-support-for-webforms-mvc-and-webpages.aspx) und die [OAuth-und OpenID-Featuredokumentation in ASP.net Web Pages](../web-pages/overview/releases/top-features-in-web-pages-2.md#oauthsetup).

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Aktualisieren eines ASP.NET MVC 3-Projekts auf ASP.NET MVC 4

ASP.NET MVC 4 kann parallel mit ASP.NET MVC 3 auf demselben Computer installiert werden. auf diese Weise können Sie flexibel entscheiden, wann eine ASP.NET MVC 3-Anwendung auf ASP.NET MVC 4 aktualisiert werden soll.

Die einfachste Möglichkeit, ein Upgrade durchzuführen, besteht darin, ein neues ASP.NET MVC 4-Projekt zu erstellen und alle Sichten, Controller, Code und Inhalts Dateien aus dem vorhandenen MVC 3-Projekt in das neue Projekt zu kopieren und dann die Assemblyverweise im neuen Projekt so zu aktualisieren, dass Sie mit einer nicht-MVC-Vorlage in gruppierten Assemblys, die Sie verwenden. Wenn Sie Änderungen an der Datei "Web. config" im MVC 3-Projekt vorgenommen haben, müssen Sie diese Änderungen auch in der Datei "Web. config" im MVC 4-Projekt zusammenführen.

Gehen Sie folgendermaßen vor, um eine vorhandene ASP.NET MVC 3-Anwendung manuell auf Version 4 zu aktualisieren:

1. Ersetzen Sie in allen Web. config-Dateien im Projekt (es gibt eine im Stammverzeichnis des Projekts, eine im Ordner "Views" und eine im Ordner "Views" für jeden Bereich im Projekt), ersetzen Sie jede Instanz des folgenden Texts (Hinweis: System. Web. Webseiten, Version = 1.0.0.0 wurde nicht gefunden in Projekte, die mit Visual Studio 2012 erstellt wurden): 

    [!code-console[Main](mvc4-release-notes/samples/sample2.cmd)]

    mit dem folgenden entsprechenden Text:

    [!code-console[Main](mvc4-release-notes/samples/sample3.cmd)]
2. Aktualisieren Sie in der Web. config-Stammdatei das Element " *Webseiten: Version* " auf "2.0.0.0", und fügen Sie einen neuen *preserveloginurl* -Schlüssel mit dem Wert "true" hinzu: 

    [!code-xml[Main](mvc4-release-notes/samples/sample4.xml)]
3. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf die Verweise, und wählen Sie nuget-Pakete verwalten aus. Wählen Sie im linken Bereich **online\nuget Official Package Source**aus, und aktualisieren Sie dann Folgendes:

    - ASP.NET MVC 4
    - (Optional) jQuery, jquery-Validierung und jQuery-Benutzeroberfläche
    - Optionale Entity Framework
    - (Optonal) Modernizr
4. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen, und wählen Sie dann Projekt entladen aus. Klicken Sie dann mit der rechten Maustaste erneut auf den Namen, und wählen Sie *Projektname*. csproj bearbeiten aus.
5. Suchen Sie nach dem *projecttypguids* -Element, und ersetzen Sie {E53F8FEA-EAE0-44A6-8774-FFD645390401} durch {E3E379DF-F4C6-4180-9B81-6769533ABE47}.
6. Speichern Sie die Änderungen, schließen Sie die Projektdatei (. csproj), die Sie bearbeitet haben, klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie dann Projekt erneut laden.
7. Wenn das Projekt auf Bibliotheken von Drittanbietern verweist, die mit früheren Versionen von ASP.NET MVC kompiliert wurden, öffnen Sie die Web. config-Stammdatei, und fügen Sie die folgenden drei *bindingRedirect* -Elemente im *Konfigurations* Abschnitt hinzu: 

    [!code-xml[Main](mvc4-release-notes/samples/sample5.xml)]

<a id="_Toc303253817"></a>
## <a name="changes-from-aspnet-mvc-4-release-candidate"></a>Änderungen von ASP.NET MVC 4 Release Candidate

Die Anmerkungen zu dieser Version von ASP.NET MVC 4 Release Candidate finden Sie hier:

Die wichtigsten Änderungen von ASP.NET MVC 4 Release Candidate in dieser Version sind im folgenden zusammengefasst:

- **Pro Controller-Konfiguration:** ASP.net-Web-API Controller können mit einem benutzerdefinierten Attribut attributiert werden, das *icontrollerconfiguration* implementiert, um eigene Formatierer, Aktions Auswahl und Parameter Binder einzurichten. Das *httpcontrollerconfigurationattribute* wurde entfernt.
- **Nachrichten Handler pro Route:** Sie können jetzt den endgültigen Nachrichten Handler in der Anforderungs Kette für eine bestimmte Route angeben. Dadurch wird die Unterstützung für das Durchsuchen von Frameworks ermöglicht, um das Routing für die Verteilung an eigene (nicht-*ihttpcontroller*-) Endpunkte zu verwenden.
- Status **Benachrichtigungen:** Der *progressmessagehandler* generiert Status Benachrichtigungen für Anforderungs Entitäten, die hochgeladen werden, und Antwort Entitäten, die heruntergeladen werden. Mithilfe dieses Handlers können Sie nachverfolgen, wie weit Sie einen Anforderungs Text hochladen oder einen Antworttext herunterladen.
- **Push-Inhalte:** Die *pushstreamcontent* -Klasse ermöglicht Szenarien, in denen ein Daten Producer mithilfe eines Streams direkt in die Anforderung oder Antwort (synchron oder asynchron) schreiben möchte. Wenn *pushstreamcontent* bereit ist, Daten zu akzeptieren, ruft es an einen Aktions Delegaten mit dem Ausgabestream an. Der Entwickler kann dann so lange wie nötig in den Stream schreiben und den Stream schließen, wenn der Schreibvorgang abgeschlossen ist. *Pushstreamcontent* erkennt das Schließen des Streams und schließt die zugrunde liegende asynchrone *Aufgabe* zum Schreiben des Inhalts ab.
- **Fehler Antworten werden erstellt:** Verwenden Sie den *HttpError* -Typ zum konsistenten darstellen von Fehlerinformationen von, wie z. b. Validierungs Fehlern und Ausnahmen, während die *includeerrordetailpolicy*weiterhin berücksichtigt wird. Verwenden Sie die neuen Methoden für die Methode " *kreateerrorresponse* ", um problemlos Fehler Antworten mit *HttpError* als Inhalt zu erstellen. Der *HttpError* -Inhalt ist vollständig aushandelte Inhalte.
- **Mediarangemapping entfernt:** Medientyp Bereiche werden nun vom Standard inhaltsverhandler behandelt.
- **Standardparameter Bindung für einfache Typparameter ist jetzt [fromuri]:** In früheren Versionen von ASP.net-Web-API die Standardparameter Bindung für einfache Typparameter die Modell Bindung verwendet. Die Standardparameter Bindung für einfache Typparameter ist jetzt *[fromuri]* .
- Die **Aktions Auswahl berücksichtigt erforderliche Parameter:** Bei der Aktions Auswahl in ASP.net-Web-API wird jetzt nur eine Aktion ausgewählt, wenn alle erforderlichen Parameter, die aus dem URI stammen, bereitgestellt werden. Ein Parameter kann als optional angegeben werden, indem ein Standardwert für das Argument in der Aktionsmethoden Signatur angegeben wird.
- **Anpassen von http-Parameter Bindungen:** Verwenden Sie das *parameterbindingattribute* , um die Parameter Bindung für einen bestimmten Aktionsparameter anzupassen, oder verwenden Sie die *parameterbindingrules* in der *httpconfiguration* , um Parameter Bindungen breiter zu gestalten.
- **Verbesserungen von mediatypeer Formatter:** Formatierer haben jetzt Zugriff auf die vollständige *httpcontent* -Instanz.
- **Auswahl der Host Puffer Richtlinie:** Implementieren und konfigurieren Sie den *ihostbufferpolicyselector* -Dienst in ASP.net-Web-API, damit Hosts die Richtlinie ermitteln können, für die die Pufferung verwendet werden soll.
- **Client Zertifikate auf Host unabhängige Weise aufrufen:** Verwenden Sie die *getclientcertificate* -Erweiterungsmethode, um das angegebene Client Zertifikat aus der Anforderungs Nachricht zu erhalten.
- **Erweiterbarkeit von Inhaltsaushandlung:** Passen Sie die Inhaltsaushandlung an, indem Sie von *defaultcontentverhandler* ableiten und einen beliebigen Aspekt der Inhalts Aushandlung überschreiben.
- **Unterstützung für die Rückgabe von 406 nicht zulässigen Antworten:** Sie können jetzt 406 nicht akzeptable Antworten in ASP.net-Web-API zurückgeben, wenn kein geeigneter Formatierer gefunden wird, indem Sie einen *defaultcontentconverter* erstellen, bei dem der *excludematchontypeonly* -Parameter auf " *true*" festgelegt ist.
- **Formulardaten als NameValueCollection oder jtoken lesen:** Sie können Formulardaten in der URI-Abfrage Zeichenfolge oder im Anforderungs Text als *NameValueCollection* lesen, indem Sie die-Erweiterungs Methoden " *Parameterstring* " und "read *asformdataasync* " verwenden. Entsprechend können Sie Formulardaten in der URI-Abfrage Zeichenfolge oder im Anforderungs Text als *jtoken* lesen, indem Sie die Methoden *trylesequeryasjson* und *leseasasync*&lt;t&gt;.
- **Mehrteilige Verbesserungen:** Es ist jetzt möglich, einen *multipartstreamprovider* zu schreiben, der vollständig auf den Typ der mehrteiligen MIME-Daten zugeschnitten ist, den er lesen und das Ergebnis auf die optimale Weise für den Benutzer darstellen kann. Sie können auch einen nach Verarbeitungsschritt auf dem *multipartstreamprovider* hostet, der es der-Implementierung ermöglicht, jede beliebige nach Verarbeitung in den mehrteiligen MIME-Textteile durchzuführen. Die *multipartformdatastreamprovider* -Implementierung liest z. b. die HTML-Formulardaten Teile und fügt Sie einer *NameValueCollection* hinzu, damit Sie vom Aufrufer leicht durchlaufen werden können.
- **Verbesserungen der Link Generierung:** Der *Urlhelper* -Dienst ist nicht mehr von *httpcontrollercontext*abhängig. Sie können jetzt in jedem Kontext, in dem die *httprequestmessage* verfügbar ist, auf das *Urlhelper* zugreifen.
- **Änderung der Nachrichten Handler-Ausführungsreihenfolge:** Meldungs Handler werden nun in der Reihenfolge ausgeführt, in der Sie statt in umgekehrter Reihenfolge konfiguriert sind.
- Hilfsobjekt **zum Verknüpfen von Meldungs Handlern:** Die neue *httpclientfactory* , mit der *delegatinghandlers* verknüpft und ein *HttpClient* erstellt werden kann, wobei die gewünschte Pipeline bereit ist. Außerdem bietet Sie Funktionen für die Verknüpfung mit alternativen inneren Handlern (standardmäßig *httpclienthandler*) sowie die Verknüpfung, wenn Sie *httpmessageinvoker* oder einen anderen *delegatinghandler* anstelle von *HttpClient* als Top-Invoker verwenden.
- **Unterstützung für CDNs in ASP.net Web Optimization:** ASP.net Web Optimization bietet jetzt Unterstützung für alternative CDN-Pfade, die es Ihnen ermöglichen, für jedes Paket eine zusätzliche URL anzugeben, die auf die gleiche Ressource auf einem Content Delivery Network verweist. Durch die Unterstützung von CDNs können Sie Ihre Skripts und Stil Pakete geografisch näher an den Endbenutzern Ihrer Webanwendungen angleichen. In Produktions-apps sollte ein Fall Back implementiert werden, wenn das CDN nicht verfügbar ist. Testen Sie den Fallback.
- **ASP.net-Web-API Routen und die Konfiguration in die statische *webapiconfig. Register* -Methode verschoben, die im Testcode fortgesetzt werden kann.** ASP.net-Web-API Routen wurden zuvor in *routeconfig. RegisterRoutes* zusammen mit den MVC-Standardrouten hinzugefügt. Die Standard ASP.net-Web-API Routen und die Konfiguration werden nun in einer separaten *webapiconfig. Register* -Methode behandelt, um das Testen zu vereinfachen.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Bekannte Probleme und wichtige Änderungen

- **Die RC-und RTM-Version von ASP.NET MVC 4 hat fälschlicherweise zwischengespeicherte Desktop Sichten zurückgegeben, wenn mobile Ansichten zurückgegeben werden sollen.**

    - Ausführliche Informationen zur Behebung finden Sie unter [ASP.NET MVC 4 Mobile Caching Bug Fixed](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) . Die Korrektur kann über das [Feste DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) -nuget-Paket installiert werden.
- Wichtige **Änderungen in der Razor-Ansichts-Engine**. Die folgenden Typen wurden aus *System. Web. MVC. Razor*entfernt: 

    - *Modelspan*
    - *Mvcvbrazorcodegenerator*
    - *Mvccsharprazorcodegenerator*
    - *Mvcvbrazorcodeparser*

  Die folgenden Methoden wurden ebenfalls entfernt: 

    - *Mvccsharprazorcodeparser. parseinverersstatement (System. Web. Razor. Parser. Codeblock Info)*
    - *MvcWebPageRazorHost. decoratecodegenerator (System. Web. Razor. Generator. razorcodegenerator)*
    - *Mvcvbrazorcodeparser. parseinverersstatement (System. Web. Razor. Parser. Codeblock Info)*
- **Wenn webmatrix. WebData. dll im Verzeichnis "/bin" einer ASP.NET MVC 4-APP enthalten ist, übernimmt es die URL für die Formular Authentifizierung.** Wenn Sie der Anwendung die webmatrix. WebData. dll-Assembly hinzufügen (z. b. durch Auswählen von "ASP.net Web Pages mit Razor-Syntax" bei Verwendung des Dialog Felds "bereit Stellungen mit aktivierbaren Abhängigkeiten hinzufügen"), wird die Authentifizierung bei der Authentifizierungs Anmeldung an/Account/Logon anstatt an/Account/Login überschrieben. Um dieses Verhalten zu verhindern und die bereits im Abschnitt Authentifizierung von Web. config angegebene URL zu verwenden, können Sie eine appSetting namens preserveloginurl hinzufügen und diese auf "true" festlegen: 

    [!code-xml[Main](mvc4-release-notes/samples/sample6.xml)]
- **Der nuget-Paket-Manager kann nicht installiert werden, wenn Sie versuchen, ASP.NET MVC 4 für parallele Installationen von Visual Studio 2010 und Visual Web Developer 2010 zu installieren.** Wenn Sie Visual Studio 2010 und Visual Web Developer 2010 nebeneinander mit ASP.NET MVC 4 ausführen möchten, müssen Sie ASP.NET MVC 4 installieren, nachdem beide Versionen von Visual Studio bereits installiert wurden.
- **Das Deinstallieren von ASP.NET MVC 4 schlägt fehl, wenn die erforderlichen Komponenten bereits deinstalliert wurden.** Zum ordnungsgemäßen Deinstallieren von ASP.NET MVC 4müssen Sie ASP.NET MVC 4 deinstallieren, bevor Sie Visual Studio deinstallieren.
- **Installieren von ASP.NET MVC 4-Unterbrechungen ASP.NET MVC 3 RTM-Anwendungen.** ASP.NET MVC 3-Anwendungen, die mit der RTM-Version (nicht mit der [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/download/details.aspx?id=1491) -Version) erstellt wurden, erfordern die folgenden Änderungen, damit Sie parallel mit ASP.NET MVC 4 funktionieren. Das Erstellen des Projekts ohne diese Updates führt zu Kompilierungs Fehlern. 

    **Erforderliche Updates**

  1. Fügen Sie in der Web. config-Stammdatei einen neuen *&lt;appSettings-&gt;* Eintrag mit dem Schlüssel *Webseiten: Version* und dem Wert *1.0.0.0*hinzu. 

      [!code-xml[Main](mvc4-release-notes/samples/sample7.xml)]
  2. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen, und wählen Sie dann Projekt entladen aus. Klicken Sie dann mit der rechten Maustaste erneut auf den Namen, und wählen Sie *Projektname*. csproj bearbeiten aus.
  3. Suchen Sie die folgenden Assemblyverweise: 

      [!code-xml[Main](mvc4-release-notes/samples/sample8.xml)]

      Ersetzen Sie Sie durch Folgendes:

      [!code-xml[Main](mvc4-release-notes/samples/sample9.xml)]
  4. Speichern Sie die Änderungen, schließen Sie die Projektdatei (. csproj), die Sie bearbeitet haben, klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie erneut laden aus.

- Wenn **Sie ein ASP.NET MVC 4-Projekt in das Ziel 4,0 von 4,5 ändern, wird der EntityFramework-Assemblyverweis nicht aktualisiert:** Wenn Sie ein ASP.NET MVC 4-Projekt nach abzielt 4,5 in das Ziel 4,0 ändern, verweist der Verweis auf die EntityFramework-Assembly weiterhin auf die Version 4,5. Deinstallieren Sie das nuget-Paket "EntityFramework", und installieren Sie es neu.
- **403 unzulässig beim Ausführen einer ASP.NET MVC 4-Anwendung in Azure nach der Umstellung auf das Ziel 4,0 von 4,5:** Wenn Sie ein ASP.NET MVC 4-Projekt nach abzielt 4,5 in das Ziel 4,0 ändern und anschließend in Azure bereitstellen, wird zur Laufzeit möglicherweise der Fehler "403 verboten" angezeigt. Um dieses Problem zu umgehen, fügen Sie der Datei "Web. config" Folgendes hinzu: `<modules runAllManagedModulesForAllRequests="true" />`
- **Visual Studio 2012 stürzt ab, wenn Sie eine "\' in einem Zeichenfolgenliterals in einer Razor-Datei eingeben.** Um das Problem zu umgehen, geben Sie zuerst das schließende Anführungszeichen des Zeichenfolgenliterals ein
- <strong>Das Navigieren zu &quot;Account/Manage&quot; in der Internet Vorlage führt zu einem Laufzeitfehler für CHS-, TRK-und cht-Sprachen.</strong> Um das Problem zu beheben, ändern Sie die Seite, um die <em>@User.Identity.Name</em> zu trennen, indem Sie Sie als einzigen Inhalt innerhalb des <em>&lt;Strong&gt;</em> -Tags ablegen.
- **Google-und LinkedIn-Anbieter werden innerhalb von Azure-Websites nicht unterstützt.** Verwenden Sie alternative Authentifizierungs Anbieter bei der Bereitstellung auf Azure-Websites.
- **Wenn Sie uripathextensionmapping mit IIS 8 Express/IIS verwenden, erhalten Sie 404 nicht gefundene Fehler, wenn Sie versuchen, die Erweiterung zu verwenden.** Der statische Datei Handler beeinträchtigt Anforderungen an Web-APIs, die *uripathextensionmappings*verwenden. Legen Sie *runAllManagedModulesForAllRequests = true* in "Web. config" fest, um das Problem zu umgehen.
- **Die Controller. Execute-Methode wird nicht mehr aufgerufen.** Alle MVC-Controller werden nun immer asynchron ausgeführt.
