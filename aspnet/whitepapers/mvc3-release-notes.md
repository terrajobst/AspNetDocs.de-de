---
uid: whitepapers/mvc3-release-notes
title: ASP.NET MVC 3 | Microsoft-Dokumentation
author: rick-anderson
description: ''
ms.author: riande
ms.date: 10/06/2010
ms.assetid: f44c166e-7e91-48a0-a6f8-d9285f3594e5
msc.legacyurl: /whitepapers/mvc3-release-notes
msc.type: content
ms.openlocfilehash: 504202068f5db4f8614bba02e8066ffecfd15b48
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78501039"
---
# <a name="aspnet-mvc-3"></a>ASP.NET MVC 3

- [Übersicht](#overview)
- [Installationshinweise](#installation-notes)
- [Software Anforderungen](#software-requirements)
- [Dokumentation](#documentation)
- [Unterstützung](#support)
- [Aktualisieren eines ASP.NET MVC 2-Projekts auf ASP.NET MVC 3 Tools Update](#upgrading)
- [ASP.NET MVC 3 Tools Update (12. April 2011)](#tu-changes)

    - [Das Dialogfeld "Controller hinzufügen" kann jetzt Controller mit Ansichten und Datenzugriffs Code Gerüstbau](#tu-AddControllerDialog)
    - [Verbesserungen am Dialog Feld "ASP.NET MVC 3 New Project"](#tu-ImprovementsNewDialogBox)
    - [Projektvorlagen enthalten jetzt modernizr 1,7](#tu-Modernizr)
    - [Zu den Projektvorlagen zählen aktualisierte Versionen von jQuery, jQuery UI und jQuery-Validierung.](#tu-UpdatedJQuery)
    - [Projektvorlagen enthalten nun ADO.NET Entity Framework 4,1 als vorinstalliertes nuget-Paket.](#tu-EF)
    - [Projektvorlagen enthalten JavaScript-Bibliotheken als vorinstallierte nuget-Pakete.](#tu-JavaScriptLibsNuget)
    - [Bekannte Probleme](#tu-KI)
- [ASP.NET MVC 3 RTM (13. Januar 2011)](#MVC3RTM)

    - [Änderung: aktualisierte Version der jQuery-Benutzeroberfläche in 1.8.7](#RTM-1)
    - [Änderung: die Standard ModelMetadataProvider wurde wieder in DataAnnotationsModelMetadataProvider geändert.](#RTM-2)
    - [Korrigiert: beim Einfügen eines Teils eines Razor-Ausdrucks, der Leerzeichen enthält, wird er umgekehrt.](#RTM-3)
    - [Korrigiert: durch das Umbenennen einer Razor-Datei, die im Editor geöffnet ist, wird die farbliche Syntax Markierung und IntelliSense deaktiviert.](#RTM-4)
    - [Bekannte Probleme](#RTM-KI)
    - [Wichtige Änderungen](#RTM-BC)
- [ASP.NET MVC 3 Release Candidate 2 (10. Dezember 2010)](#_Toc2)

    - [Die Projektvorlagen wurden in jQuery 1.4.4, jQuery Validation 1,7 und jQuery UI 1.8.6 y UI 1.8.6 geändert.](#_Toc2_1)
    - ["AdditionalMetadataAttribute"-Klasse hinzugefügt](#_Toc2_2)
    - [Verbessertes Ansichts Gerüst](#_Toc2_3)
    - [Hinzugefügte HTML. RAW-Methode](#_Toc2_3)
    - [Umbenennen der Eigenschaft "Controller. ViewModel" und der Eigenschaft "View" in "viewbag"](#_Toc2_4)
    - [Umbenennen der Klasse "controllersessionstateattribute" in "sessionstateattribute"](#_Toc2_5)
    - [Umbenannte remoteattribute-Eigenschaft "Fields" in "additionalfields"](#_Toc2_6)
    - [Umbenennen von "skiprequestvalidationattribute" in "zugwhtmlattribute"](#_Toc2_7)
    - [Die "HTML. validationmessage"-Methode wurde geändert, um die erste nützliche Fehlermeldung anzuzeigen.](#_Toc2_8)
    - [Korrigiert @model Deklaration, um dem Dokument keine Leerzeichen hinzuzufügen.](#_Toc2_9)
    - ["File Extensions"-Eigenschaft wurde hinzugefügt, um Engines anzuzeigen, um Engine-spezifische Dateinamen zu unterstützen](#_Toc2_10)
    - [Das Hilfsprogramm "LabelFor" wurde korrigiert, um den korrekten Wert für das "for"-Attribut auszugeben.](#_Toc2_11)
    - [Die "renderaction"-Methode wurde korrigiert, um bei der Modell Bindung explizite Werte zu vergeben.](#_Toc2_12)
    - [Wichtige Änderungen](#_Toc2_BC)
    - [Bekannte Probleme](#_Toc2_KI)
- [ASP.NET MVC 3 Release Candidate (9. November 2010)](#TOC_ASP_NET_3_RC)

    - [Neue Features in ASP.NET MVC 3 RC](#_Toc276711785)
    - [NuGet-Paket-Manager](#_Toc276711786)
    - [Verbessertes Dialog Feld "Neues Projekt"](#_Toc276711787)
    - [Sessionless-Controller](#_Toc276711788)
    - [Neue Validierungs Attribute](#_Toc276711789)
    - [Neue über Ladungen für die Methoden "LabelFor" und "labelformodel"](#_Toc276711790)
    - [Ausgabe Zwischenspeichern von untergeordneten Aktionen](#_Toc276711791)
    - [Verbesserungen im Dialog Feld "Ansicht hinzufügen"](#_Toc276711792)
    - [Granulare Anforderungs Validierung](#_Toc276711793)
    - [Wichtige Änderungen](#_Toc276711794)
    - [Bekannte Probleme](#_Toc276711795)
- [ASP. MVC 3-Beta Notizen (6. Oktober 2010)](#TOC_ASP_NET_3_Beta)

    - [Neue Features in ASP.NET MVC 3 Beta](#0.1__Toc274034215)
    - [Nupack-Paket-Manager](#0.1__Toc274034216)
    - [Verbessertes Dialog Feld "Neues Projekt"](#0.1__Toc274034217)
    - [Vereinfachte Methode zum angeben stark typisierter Modelle in Razor-Ansichten](#0.1__Toc274034218)
    - [Unterstützung für neue ASP.net Web Pages-Hilfsmethoden](#0.1__Toc274034219)
    - [Zusätzliche Unterstützung der Abhängigkeitsinjektion](#0.1__Toc274034220)
    - [Neue Unterstützung für unaufdringliche jQuery-basiertes AJAX](#0.1__Toc274034221)
    - [Neue Unterstützung für unaufdringliche jquery-Validierung](#0.1__Toc274034222)
    - [Neue Anwendungs weite Flags für die Client Validierung und unaufdringliches JavaScript](#0.1__Toc274034223)
    - [Neue Unterstützung für Code, der vor der Ausführung von Sichten ausgeführt wird](#0.1__Toc274034224)
    - [Neue Unterstützung für die vbhtml-Razor-Syntax](#0.1__Toc274034225)
    - [Genauere Kontrolle über validateinputattribute](#0.1__Toc274034226)
    - [Hilfsprogramme konvertieren Unterstriche in Bindestriche für HTML-Attributnamen, die mit anonymen Objekten angegeben werden.](#0.1__Toc274034227)
    - [Fehlerbehebungen](#0.1__Toc274034228)
    - [Wichtige Änderungen](#0.1__Toc274034229)
    - [Bekannte Probleme](#0.1__Toc274034230)
- [ERS](#0.1__Toc274034231)

<a id="overview"></a>
## <a name="overview"></a>Übersicht

In diesem Dokument wird die Veröffentlichung von ASP.NET MVC 3 RTM für Visual Studio 2010 beschrieben. ASP.NET MVC ist ein Framework für die Entwicklung von Webanwendungen, die das Model-View-Controller (MVC)-Muster verwenden. Der ASP.NET MVC 3-Installer umfasst die folgenden Komponenten:

- ASP.NET MVC 3-Laufzeitkomponenten
- ASP.NET MVC 3 Visual Studio 2010-Tools
- Laufzeitkomponenten ASP.net Web Pages
- ASP.net Web Pages Visual Studio 2010-Tools
- Microsoft-Paket-Manager für .net (nuget)
- Ein Update für Visual Studio 2010, das die Unterstützung für Razor-Syntax ermöglicht. (Ausführliche Informationen finden Sie im Knowledge Base-Artikel 2483190.)

Den vollständigen Satz der Anmerkungen zu dieser Version für jede Vorabversion von ASP.NET MVC 3 finden Sie auf der ASP.NET-Website unter der folgenden URL:

https://www.asp.net/learn/whitepapers/mvc3-release-notes

<a id="installation-notes"></a>
## <a name="installation-notes"></a>Installationshinweise

Wenn Sie ASP.NET MVC 3 RTM mithilfe des Webplattform-Installers (Web PI) installieren möchten, besuchen Sie die folgende Seite:

[https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3](https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3)

Alternativ können Sie das Installationsprogramm für ASP.NET MVC 3 RTM für Visual Studio 2010 von der folgenden Seite herunterladen:

https://go.microsoft.com/fwlink/?LinkID=208140

ASP.NET MVC 3 kann installiert und parallel mit ASP.NET MVC 2 ausgeführt werden.

<a id="software-requirements"></a>
## <a name="software-requirements"></a>Softwareanforderungen

Die ASP.NET MVC 3-Laufzeitkomponenten erfordern folgende Software:

- .NET Framework Version 4. 

    ASP.NET MVC 3 Visual Studio 2010 Tools erfordern die folgende Software:
- Visual Studio 2010 oder Visual Web Developer 2010 Express.

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentation

Die Dokumentation für ASP.NET MVC ist auf der MSDN-Website unter der folgenden URL verfügbar:

[https://go.microsoft.com/fwlink/?LinkId=205717](https://go.microsoft.com/fwlink/?LinkId=205717)

Tutorials und weitere Informationen zu ASP.NET MVC sind auf der MVC-Seite der ASP.NET-Website unter der folgenden URL verfügbar:

[https://www.asp.net/mvc/](../mvc/index.md)

<a id="support"></a>
## <a name="support"></a>Support

Dies ist eine vollständig unterstützte Version. Informationen zum technischen Support finden Sie auf der Microsoft-Support- [Website](https://support.microsoft.com/).

Außerdem können Sie Fragen zu dieser Version im ASP.NET MVC-Forum stellen, in dem Mitglieder der ASP.net-Community häufig informelle Unterstützung bereitstellen können:

[https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)

<a id="upgrading"></a>
## <a name="upgrading-an-aspnet-mvc-2-project-to-aspnet-mvc-3-tools-update"></a>Aktualisieren eines ASP.NET MVC 2-Projekts auf ASP.NET MVC 3 Tools Update

ASP.NET MVC 3 kann parallel zu ASP.NET MVC 2 auf demselben Computer installiert werden. auf diese Weise können Sie flexibel entscheiden, wann eine ASP.NET MVC 2-Anwendung auf ASP.NET MVC 3 aktualisiert werden soll.

Gehen Sie folgendermaßen vor, um eine vorhandene ASP.NET MVC 2-Anwendung manuell auf Version 3 zu aktualisieren:

1. Erstellen Sie ein neues leeres ASP.NET MVC 3-Projekt auf Ihrem Computer. Dieses Projekt enthält einige Dateien, die für das Upgrade benötigt werden.
2. Kopieren Sie die folgenden Dateien aus dem ASP.NET MVC 3-Projekt in den entsprechenden Speicherort Ihres ASP.NET MVC 2-Projekts. Sie müssen alle Verweise auf die jQuery-Bibliothek aktualisieren, um den neuen Dateinamen (jQuery-1.5.1. js) zu berücksichtigen: 

    - /Views/Web.config
    - /packages.config
    - /Scripts/\*. js
    - /Content/Themes/-\*.\*
3. Kopieren Sie den Ordner " *Packages* " im Stammverzeichnis der leeren ASP.NET MVC 3-Projekt Mappe in das Stammverzeichnis der Projekt Mappe, das sich in dem Verzeichnis befindet, in dem sich die SLN-Datei der Projekt Mappe befindet.
4. Wenn Ihr ASP.NET MVC 2-Projektbereiche enthält, kopieren Sie die Datei/views/Web.config in den Ordner *views* jedes Bereichs.
5. In den Web. config-Dateien im ASP.NET MVC 2-Projekt müssen Sie die ASP.NET MVC-Version Global suchen und ersetzen. Suchen Sie Folgendes: 

    [!code-console[Main](mvc3-release-notes/samples/sample1.cmd)]

    Ersetzen Sie ihn durch Folgendes:

    [!code-console[Main](mvc3-release-notes/samples/sample2.cmd)]
6. Löschen Sie in Projektmappen-Explorer den Verweis auf *System. Web. MVC* (der auf die dll von Version 2 verweist), und fügen Sie dann einen Verweis auf *System. Web. MVC* (v 3.0.0.0) hinzu.
7. Fügen Sie einen Verweis auf "System. Web. Webseiten. dll" und "System. Web. Hilfsprogramme. dll" hinzu. Diese Assemblys befinden sich in den folgenden Ordnern: 

    - % Program Files% \ Microsoft ASP. net\asp.NET MVC 3 \ Assemblys
    - % Program Files% \ Microsoft ASP. net\asp.net Web-webbenutzer\v1.0\assemblys
8. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen, und wählen Sie Projekt entladen aus. Klicken Sie dann mit der rechten Maustaste erneut auf den Projektnamen, und wählen Sie *ProjectName*. csproj bearbeiten aus.
9. Suchen Sie nach dem *projecttypguids* -Element, und ersetzen Sie {F85E285D-A4E0-4152-9332-AB1D724D3325} durch {E53F8FEA-EAE0-44A6-8774-FFD645390401}.
10. Speichern Sie die Änderungen, klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie dann Projekt erneut laden.
11. Fügen Sie in der Web. config-Stammdatei der *Anwendung die folgenden Einstellungen zum Abschnitt* Assemblys hinzu. 

    [!code-xml[Main](mvc3-release-notes/samples/sample3.xml)]
12. Wenn das Projekt auf Bibliotheken von Drittanbietern verweist, die mit ASP.NET MVC 2 kompiliert werden, fügen Sie das folgende hervorgehobene *bindingRedirect* -Element in der Datei "Web. config" im Anwendungs Stamm im *Konfigurations* Abschnitt hinzu: 

    [!code-xml[Main](mvc3-release-notes/samples/sample4.xml)]

<a id="tu-changes"></a>
## <a name="changes-in-aspnet-mvc-3-tools-update"></a>Änderungen in ASP.NET MVC 3 Tools Update

In diesem Abschnitt werden die Änderungen beschrieben, die in der ASP.NET MVC 3 Tools Update-Version seit der Version ASP.NET MVC 3 RTM vorgenommen wurden.

<a id="tu-AddControllerDialog"></a>
### <a name="add-controller-dialog-box-can-now-scaffold-controllers-with-views-and-data-access-code"></a>Das Dialogfeld "Controller hinzufügen" kann jetzt Controller mit Ansichten und Datenzugriffs Code Gerüstbau

Gerüstbau ist eine Möglichkeit, schnell einen Controller und Ansichten für Ihre Anwendung zu erstellen. Nachdem der Code generiert wurde, können Sie ihn bearbeiten, um die Anforderungen des Projekts zu erfüllen.

Um das Dialog *Feld Controller hinzufügen* in ASP.NET MVC 3 aufzurufen, klicken Sie in *Projektmappen-Explorer*mit der rechten Maustaste auf den Ordner *Controller* , klicken Sie auf *Hinzufügen*und dann auf *Controller*. Das Dialogfeld wurde erweitert, um zusätzliche gerüstingoptionen zu bieten.

![](mvc3-release-notes/_static/image1.png)

Standardmäßig sind drei Gerüstbau Vorlagen verfügbar.

#### <a name="empty-controller"></a>Leerer Controller

Mit dieser Vorlage wird eine leere Controller Datei generiert. Diese Vorlage entspricht nicht der Überprüfung von *Add-Aktionen für CREATE-, Edit-, Details-und DELETE-Szenarien* in früheren Versionen von ASP.NET MVC. Wenn Sie diese Option auswählen, sind keine weiteren Optionen verfügbar.

#### <a name="controller-with-empty-readwrite-actions"></a>Controller mit leeren Lese-/Schreibaktionen

Diese Vorlage generiert eine Controller Datei, die über alle erforderlichen Aktionsmethoden, aber keinen Implementierungs Code in den-Methoden verfügt. Diese Vorlage entspricht der Überprüfung von *Aktionen zum Erstellen, bearbeiten, hinzufügen und Löschen von Aktionen* in früheren Versionen von ASP.NET MVC. Wenn Sie diese Option auswählen, sind keine weiteren Optionen verfügbar.

#### <a name="controller-with-readwrite-actions-and-views-using-entity-framework"></a>Controller mit Lese-/Schreibaktionen und Ansichten unter Verwendung Entity Framework

Mit dieser Vorlage können Sie schnell eine funktionierende Dateneingabe-Benutzeroberfläche erstellen. Es generiert Code, der eine Reihe allgemeiner Anforderungen und Szenarien behandelt, wie z. b. Folgendes:

- *Datenzugriff*. Der generierte Code liest und schreibt Entitäten in einer Datenbank. Dies funktioniert mit dem Entity Framework Code First Ansatz, wenn Sie eine vorhandene Datenkontext Klasse auswählen oder wenn Sie die Vorlage eine neue *dbcontext* -Klasse generieren lassen. Es funktioniert auch mit dem Entity Framework Database First oder Model First Ansatz, wenn Sie eine vorhandene *ObjectContext* -Klasse auswählen.
- *Validierung*. Der generierte Code verwendet ASP.NET MVC-Modell Bindung und Metadatenfeatures, damit Formular Übermittlungen gemäß den in der Modell Klasse deklarierten Regeln validiert werden. Dies schließt integrierte Validierungsregeln ein, z. b. die *erforderlichen* und *StringLength* -Attribute sowie benutzerdefinierte Validierungsregeln.
- *1:* n-Beziehungen. Wenn Sie 1: n-Fremdschlüssel Beziehungen zwischen den Modellklassen definieren, erzeugt der generierte Code Dropdown Listen für die Auswahl verwandter Entitäten. Beispielsweise können Sie die folgenden Modellklassen nach Entity Framework Code First Konventionen definieren: 

    [!code-csharp[Main](mvc3-release-notes/samples/sample5.cs)]

    Wenn Sie dann einen Controller für die *Product* -Klasse Gerüstbau, ermöglichen seine Ansichten Benutzern das Auswählen eines *kategorieobjekts* für jede *Produkt* Instanz.

    Diese Vorlage ermöglicht zusätzliche Optionen im Dialogfeld *Controller hinzufügen* . Für *Modellklassen*können Sie eine beliebige Modell Klasse in der Projekt Mappe auswählen, die den Typ der Daten bestimmt, die Benutzer erstellen oder bearbeiten können:
- Wenn Sie Entity Framework Code First verwenden möchten, können Sie eine beliebige Modell Klasse auswählen.
- Wenn Sie Entity Framework Database First oder Entity Framework Model First verwenden, stellen Sie sicher, dass Sie eine Entitäts Klasse auswählen, die im konzeptionellen Modell definiert ist.

Für die *Datenkontext Klasse*können Sie folgende Optionen auswählen:

- Wenn Sie Code First verwenden möchten und keine Datenkontext Klasse vorhanden ist, wählen Sie * * neuer Datenkontext * * aus. Eine Datenkontext Klasse wird dann für Sie generiert.
- Wenn Sie Code First verwenden und über eine vorhandene Datenkontext Klasse verfügen möchten, wählen Sie diese hier aus. Er wird aktualisiert, um die von Ihnen ausgewählte Modell Klasse beizubehalten.
- Wenn Sie Database First oder Model First verwenden, wählen Sie hier die Objekt Kontext Klasse aus.

Wählen Sie für Sichten das Ansichts Modul aus, das Sie verwenden möchten, oder wählen Sie keine aus, wenn Sie keine Ansichten für das Gerüst verwenden möchten.

Sie können erweiterte optionsto auswählen, um weitere Optionen für die generierten Sichten anzugeben. Beispielsweise können Sie das zu verwendende Layout oder die Master Seite auswählen.

<a id="tu-ImprovementsNewDialogBox"></a>
### <a name="improvements-to-the-aspnet-mvc-3-new-project-dialog-box"></a>Verbesserungen am Dialog Feld "ASP.NET MVC 3 New Project"

Das Dialogfeld, das Sie zum Erstellen neuer ASP.NET MVC 3-Projekte verwenden, umfasst mehrere Verbesserungen, wie unten aufgeführt.

![](mvc3-release-notes/_static/image2.png)

#### <a name="new-intranet-project-template"></a>Neue Vorlage "intranetprojekt"

Die Projektvorlagen Liste enthält eine neue Vorlage für Intranetanwendungen. Diese Vorlage enthält Einstellungen zum Erstellen einer Webanwendung mithilfe der Windows-Authentifizierung anstelle der Formular Authentifizierung. Da eine Intranetanwendung einige IIS-Einstellungen erfordert, die in einer Projektvorlage nicht gekapselt werden können, enthält die Vorlage eine Infodatei mit Anweisungen dazu, wie die Projektvorlage in IIS funktioniert. Die Dokumentation für die Vorlage neue Intranetanwendung ist auf der MSDN-Website unter der folgenden URL verfügbar:

[https://msdn.microsoft.com/library/gg703322(VS.98).aspx](https://msdn.microsoft.com/library/gg703322(VS.98).aspx)

#### <a name="project-templates-are-now-html5-enabled"></a>Die Projektvorlagen sind jetzt HTML5-fähig.

Das Dialogfeld "Neues Projekt" enthält jetzt eine Option zum Hinzufügen von HTML5-spezifischen Funktionen zu den Projektvorlagen. Das Auswählen der Option bewirkt, dass Sichten generiert werden, die die neuen HTML5-`<header>`, `<footer>`und `<navigation>` Elemente enthalten.

Beachten Sie, dass in früheren Versionen von Browsern keine HTML5-spezifischen Tags unterstützt werden. Die HTML5-Projektvorlagen enthalten einen Verweis auf die modernizr-Bibliothek, um diese Einschränkung zu beheben. (Siehe nächster Abschnitt.)

<a id="tu-Modernizr"></a>
### <a name="project-templates-now-include-modernizr-17"></a>Projektvorlagen enthalten jetzt modernizr 1,7

Modernizr ist eine JavaScript-Bibliothek, die die Unterstützung für CSS 3 und HTML5 in Browsern ermöglicht, die diese Features noch nicht unterstützen. Diese Bibliothek ist als vorinstalliertes nuget-Paket in Vorlagen für ASP.NET MVC 3-Projekte enthalten. Weitere Informationen zu modernizr finden Sie unter [http://www.modernizr.com/](http://www.modernizr.com/).

<a id="tu-UpdatedJQuery"></a>
### <a name="project-templates-include-updated-versions-of-jquery-jquery-ui-and-jquery-validation"></a>Zu den Projektvorlagen zählen aktualisierte Versionen von jQuery, jQuery UI und jQuery-Validierung.

Die Projektvorlagen enthalten nun die folgenden Versionen der jQuery-Skripts:

- jQuery 1.5.1
- jQuery-Validierung 1,8
- jQuery UI 1.8.11

Diese Bibliotheken sind als vorinstallierte nuget-Pakete enthalten.

<a id="tu-EF"></a>
### <a name="project-templates-now-include-adonet-entity-framework-41-as-a-pre-installed-nuget-package"></a>Projektvorlagen enthalten nun ADO.NET Entity Framework 4,1 als vorinstalliertes nuget-Paket.

Der ADO.NET Entity Framework 4,1 umfasst das Code First Feature. Code First ist ein neues Entwicklungsmuster für den ADO.NET-Entity Framework, das eine Alternative zu den vorhandenen Database First und Model First Mustern bereitstellt.

Code First ist darauf ausgerichtet, das Modell mit poco-Klassen ("Plain Old CLR Objects") zu definieren, C#die in Visual Basic oder geschrieben wurden. Diese Klassen können dann einer vorhandenen Datenbank zugeordnet oder zum Generieren eines Datenbankschemas verwendet werden. Zusätzliche Konfigurationen können mithilfe von *DataAnnotations* -Attributen oder mithilfe von fließenden APIs bereitgestellt werden.

Die Dokumentation zur Verwendung von Code firstwith ASP.NET MVC ist auf der ASP.NET-Website unter den folgenden URLs verfügbar:

[https://www.asp.net/mvc/tutorials/getting-started-with-mvc3-part1-cs](../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) [https://www.asp.net/entity-framework/tutorials/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)

<a id="tu-JavaScriptLibsNuget"></a>
### <a name="project-templates-include-javascript-libraries-as-pre-installed-nuget-packages"></a>Projektvorlagen enthalten JavaScript-Bibliotheken als vorinstallierte nuget-Pakete.

Wenn Sie ein neues ASP.NET MVC 3-Projekt erstellen, enthält das Projekt die zuvor erwähnten JavaScript-Dateien (z. b. die modernizr-Bibliothek), indem Sie diese mithilfe von nuget installieren, anstatt die Skripts direkt dem Ordner "Scripts" in der Projektvorlage hinzuzufügen. Haus. Dies ermöglicht es Ihnen, die Skripts mit nuget auf die neueste Version zu aktualisieren, wenn neue Versionen der Skripts veröffentlicht werden.

Wenn z. b. die Häufigkeit neuer jQuery-Releases auftritt, ist die Version von jQuery, die in der Projektvorlage enthalten ist, zu einem bestimmten Zeitpunkt veraltet. Da jQuery jedoch als installiertes nuget-Paket enthalten ist, werden Sie im nuget-Dialogfeld benachrichtigt, wenn neuere Versionen von jQuery verfügbar sind.

Da jQuery die Versionsnummer im Dateinamen enthält, erfordert die Aktualisierung von jQuery auf die aktuelle Version auch das Aktualisieren des `<script>`-Tags, das auf die jQuery-Datei verweist, damit der neue Dateiname verwendet wird. Andere enthaltene Skript Bibliotheken enthalten nicht die Versionsnummer im Skriptnamen, sodass Sie leichter auf die neuesten Versionen aktualisiert werden können.

<a id="tu-KI"></a>
## <a name="known-issues"></a>Bekannte Probleme

- In einigen Fällen schlägt die Installation möglicherweise mit der Fehlermeldung "Fehler bei der Installation mit Fehlercode (0x80070643)" fehl. Informationen zum umgehen dieses Problems finden Sie im [Knowledge Base-Artikel 2531566](https://support.microsoft.com/kb/2531566).
- Das Gerüst für das Hinzufügen eines Controllers umfasst keine Gerüstbau-Entitäten, die die Unterstützung von Entitäts Vererbung in Entity Framework nutzen Wenn z. b. eine Basisklasse der *Person* ist, die von einer *Student* -Klasse geerbt wird, führt das Gerüstbau der *Student* -Klasse zu generiertem Code, der nicht kompiliert wird.
- Das Erstellen eines neuen ASP.NET MVC 3-Projekts in einem Projektmappenordner verursacht einen *NullReferenceException* -Fehler. Die Problem Umgehung besteht darin, das ASP.NET MVC 3-Projekt im Stammverzeichnis der Projekt Mappe zu erstellen und dann in den Projektmappenordner zu verschieben.
- IntelliSense für Razor-Syntax funktioniert nicht, wenn reschärfere installiert ist. Wenn Sie installiert haben und die Razor IntelliSense-Unterstützung in ASP.NET MVC 3 nutzen möchten, lesen Sie den Eintrag [Razor IntelliSense und reschärfere](https://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) im Blog von Hadi Hari, in dem erläutert wird, wie Sie diese heute verwenden können.
- Während der Installation werden im Dialogfeld EULA-Annahme die Lizenzbedingungen in einem Fenster angezeigt, das kleiner als beabsichtigt ist.
- Beim Bearbeiten einer Razor-Ansicht (. cshtml oder). *vbhtml* -Datei), Ansichten. ASP.NET MVC 3 enthält keine Ausschnitte für Razor-Ansichten. aspxauswählt einen Code Ausschnitt für ASP.NET MVC zeigt Ausschnitte für
- Wenn Sie ASP.NET MVC 3 für Visual Web Developer Express auf einem Computer installieren, auf dem Visual Studio nicht installiert ist, und später Visual Studio installieren, müssen Sie ASP.NET MVC 3 neu installieren. Visual Studio und Visual Web Developer Express Teilen Komponenten auf, die mit dem ASP.NET MVC 3-Installer aktualisiert werden. Dasselbe Problem tritt auf, wenn Sie ASP.NET MVC 3 für Visual Studio auf einem Computer installieren, auf dem Visual Web Developer Express nicht installiert ist, und anschließend Visual Web Developer Express später installieren.

<a id="MVC3RTM"></a>
## <a name="changes-in-aspnet-mvc-3-rtm"></a>Änderungen in ASP.NET MVC 3 RTM

In diesem Abschnitt werden Änderungen und Fehlerbehebungen beschrieben, die in der ASP.NET MVC 3 RTM-Version seit der RC2-Version vorgenommen wurden.

<a id="RTM-1"></a>
### <a name="change-updated-the-version-of-jquery-ui-to-187"></a>Änderung: aktualisierte Version der jQuery-Benutzeroberfläche in 1.8.7

Die ASP.NET MVC-Projektvorlagen für Visual Studio wurden so aktualisiert, dass Sie die neueste Version der jQuery UI-Bibliothek enthalten. Die Vorlagen enthalten außerdem den minimalen Satz an Ressourcen Dateien, die für die jQuery-Benutzeroberfläche erforderlich sind, z.b. die zugeordneten CSS-und Bilddateien.

<a id="RTM-2"></a>
### <a name="change-changed-the-default-modelmetadataprovider-back-to-dataannotationsmodelmetadataprovider"></a>Änderung: die Standard ModelMetadataProvider wurde wieder in DataAnnotationsModelMetadataProvider geändert.

Mit der RC2-Version von ASP.NET MVC 3 wurde eine *CachedDataAnnotationsMetadataProvider* -Klasse eingeführt, die die Zwischenspeicherung auf der vorhandenen *DataAnnotationsModelMetadataProvider* -Klasse als Leistungsverbesserung bereitstellte. Bei dieser Implementierung wurden jedoch einige Fehler gemeldet, sodass die Änderung wieder hergestellt und in das MVC Futures-Projekt verschoben wurde, das unter [ASP.net webstack](https://github.com/aspnet/AspNetWebStack)verfügbar ist.

<a id="RTM-3"></a>
### <a name="fixed-pasting-part-of-a-razor-expression-that-contains-whitespace-results-in-it-being-reversed"></a>Korrigiert: beim Einfügen eines Teils eines Razor-Ausdrucks, der Leerzeichen enthält, wird er umgekehrt.

Wenn Sie in vorab Versionen von ASP.NET MVC 3 einen Teil eines Razor-Ausdrucks, der Leerzeichen enthält, in eine Razor-Datei einfügen, wird der resultierende Ausdruck umgekehrt. Sehen Sie sich beispielsweise den folgenden Razor-Codeblock an:

[!code-cshtml[Main](mvc3-release-notes/samples/sample6.cshtml)]

Wenn Sie in der ersten Methode den Text "First param" auswählen und ihn als Argument in die zweite Methode einfügen, sieht das Ergebnis wie folgt aus:

[!code-cshtml[Main](mvc3-release-notes/samples/sample7.cshtml)]

Das richtige Verhalten ist, dass der Einfügevorgang Folgendes ergeben sollte:

[!code-cshtml[Main](mvc3-release-notes/samples/sample8.cshtml)]

Dieses Problem wurde in der RTM-Version behoben, sodass der Ausdruck während des Einfügevorgangs ordnungsgemäß beibehalten wird.

<a id="RTM-4"></a>
### <a name="fixed-renaming-a-razor-file-that-is-opened-in-the-editor-disables-syntax-colorization-and-intellisense"></a>Korrigiert: durch das Umbenennen einer Razor-Datei, die im Editor geöffnet ist, wird die farbliche Syntax Markierung und IntelliSense deaktiviert.

Wenn Sie eine Razor-Datei mit Projektmappen-Explorer umbenennen, während die Datei im Editor Fenster geöffnet wird, bewirkt dies, dass die Syntax Hervorhebung und IntelliSense für diese Datei nicht mehr funktionieren. Dies wurde korrigiert, sodass Hervorhebung und IntelliSense nach einer Umbenennung beibehalten werden.

<a id="RTM-KI"></a>
## <a name="known-issues"></a>Bekannte Probleme

- Wenn Sie Visual Studio 2010 SP1 Beta schließen, während die nuget-Paket-Manager-Konsole geöffnet ist, stürzt Visual Studio ab und versucht, neu zu starten. Dies wird in der RTM-Version von Visual Studio 2010 SP1 korrigiert.
- Der ASP.NET MVC 3-Installer kann nur eine anfängliche Version des nuget-Paket-Managers installieren. Nachdem Sie die anfängliche Version installiert haben, kann nuget mit dem Erweiterungs-Manager von Visual Studio installiert und aktualisiert werden. Wenn Sie nuget bereits installiert haben, navigieren Sie zum Visual Studio-Erweiterungs Katalog, um ein Update auf die neueste Version von nuget durchzusetzen.
- Das Erstellen eines neuen ASP.NET MVC 3-Projekts in einem Projektmappenordner verursacht einen *NullReferenceException* -Fehler. Die Problem Umgehung besteht darin, das ASP.NET MVC 3-Projekt im Stammverzeichnis der Projekt Mappe zu erstellen und dann in den Projektmappenordner zu verschieben.
- Das Installationsprogramm kann deutlich länger dauern als vorherige Versionen von ASP.NET MVC. Dies liegt daran, dass die Komponenten von Visual Studio 2010 aktualisiert werden.
- IntelliSense für Razor-Syntax funktioniert nicht, wenn reschärfere installiert ist. Wenn Sie installiert haben und die Razor IntelliSense-Unterstützung in ASP.NET MVC 3 nutzen möchten, lesen Sie den Eintrag [Razor IntelliSense und reschärfere](https://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) im Blog von Hadi Hari, in dem erläutert wird, wie Sie diese heute verwenden können.
- Bei ccshtml-und vbhtml-Sichten, die mit der Beta Version von ASP.NET MVC 3 erstellt wurden, ist die Buildaktion nicht ordnungsgemäß festgelegt. das Ergebnis ist, dass diese Ansichts Typen bei der Veröffentlichung des Projekts ausgelassen werden. Der buildaktionswert für diese Dateien sollte auf "Content" festgelegt werden. ASP.NET MVC 3 RTM korrigiert dieses Problem für neue Dateien, korrigiert jedoch nicht die Einstellung für vorhandene Dateien für ein Projekt, das mit vorab Versionen erstellt wurde.
- ![](mvc3-release-notes/_static/image3.png)
- Während der Installation werden im Dialogfeld EULA-Annahme die Lizenzbedingungen in einem Fenster angezeigt, das kleiner als beabsichtigt ist.
- Wenn Sie eine Razor-Ansicht (cshtml-Datei) bearbeiten, ist das Menü Element gehe zu Controller in Visual Studio nicht verfügbar, und es sind keine Code Ausschnitte vorhanden.
- Wenn Sie ASP.NET MVC 3 für Visual Web Developer Express auf einem Computer installieren, auf dem Visual Studio nicht installiert ist, und später Visual Studio installieren, müssen Sie ASP.NET MVC 3 neu installieren. Visual Studio und Visual Web Developer Express Teilen Komponenten auf, die mit dem ASP.NET MVC 3-Installer aktualisiert werden. Dasselbe Problem tritt auf, wenn Sie ASP.NET MVC 3 für Visual Studio auf einem Computer installieren, auf dem Visual Web Developer Express nicht installiert ist, und anschließend Visual Web Developer Express später installieren.

<a id="RTM-BC"></a>
## <a name="breaking-changes"></a>Die Lauffähigkeit der Anwendung beeinträchtigende Änderungen

- In früheren Versionen von ASP.NET MVC werden Aktionsfilter pro Anforderung erstellt, außer in wenigen Fällen. Dieses Verhalten war nie ein garantiertes Verhalten, aber nur ein Implementierungsdetail, und der Vertrag für Filter war, Sie zustandslos zu überdenken. In ASP.NET MVC 3 werden Filter aggressiver zwischengespeichert. Daher können alle benutzerdefinierten Aktionsfilter, die den Instanzzustand nicht ordnungsgemäß speichern, beschädigt werden.
- Die Ausführungsreihenfolge für Ausnahme Filter hat sich bei Ausnahme Filtern mit dem gleichen *Reihen* folgen Wert geändert. In ASP.NET MVC 2 und früheren Versionen werden Ausnahme Filter auf dem Controller, die denselben *Reihen* folgen Wert wie die für eine Aktionsmethode aufweisen, vor der Ausnahme Filter für die Aktionsmethode ausgeführt. Dies wäre in der Regel der Fall, wenn Ausnahme Filter ohne einen angegebenen *Bestell* Wert angewendet werden. In ASP.NET MVC 3 wurde diese Reihenfolge umgekehrt, sodass der spezifischere Ausnahmehandler zuerst ausgeführt wird. Wie in früheren Versionen werden die Filter in der angegebenen Reihenfolge ausgeführt, wenn die *Order* -Eigenschaft explizit angegeben wird.
- Der Basisklasse " *virtualpathproviderviewengine* " wurde eine neue Eigenschaft mit dem Namen " *File Extensions* " hinzugefügt. Wenn ASP.net eine Ansicht nach Pfad (nicht nach Name) sucht, werden nur Sichten mit einer Dateierweiterung berücksichtigt, die in der von dieser neuen Eigenschaft angegebenen Liste enthalten sind. Dies ist ein Breaking Change in Anwendungen, in denen ein benutzerdefinierter Buildanbieter registriert ist, um eine benutzerdefinierte Dateierweiterung für Webformular Sichten zu aktivieren, und wo der Anbieter auf diese Sichten verweist, indem anstelle eines Namens ein vollständiger Pfad verwendet wird. Die Problem Umgehung besteht darin, den Wert der *FileExtensions* -Eigenschaft so zu ändern, dass Sie die benutzerdefinierte Dateierweiterung einschließt.
- Benutzerdefinierte Controller-Factory-Implementierungen, die die *IControllerFactory* -Schnittstelle direkt implementieren, müssen eine Implementierung der neuen *getcontrollersessionbehavior* -Methode bereitstellen, die der-Schnittstelle in dieser Version hinzugefügt wurde. Im Allgemeinen wird empfohlen, dass Sie diese Schnittstelle nicht direkt implementieren und stattdessen die Klasse von *defaultcontrollerfactory*ableiten.

<a id="_Toc2"></a>
## <a name="changes-in-aspnet-mvc-3-rc2"></a>Änderungen in ASP.NET MVC 3 rc2

In diesem Abschnitt werden die Änderungen (neue Features und Fehlerbehebungen) beschrieben, die in der ASP.NET MVC 3 RC2-Version seit der RC-Version vorgenommen wurden.

<a id="_Toc2_1"></a>
### <a name="project-templates-changed-to-include-jquery-144-jquery-validation-17-and-jquery-ui-186"></a>Projektvorlagen wurden geändert, sodass Sie jQuery 1.4.4, jQuery Validation 1,7 und jQuery UI 1.8.6 einschließen.

Die Projektvorlagen für ASP.NET MVC 3 enthalten nun die aktuellen Versionen von jQuery, jQuery Validation und jQuery UI. die jQuery-Benutzeroberfläche ist eine neue Ergänzung zu den Projektvorlagen und bietet nützliche Widgets für die Benutzeroberfläche. Weitere Informationen zur jQuery-Benutzeroberfläche finden Sie auf Ihrer Homepage: [http://jqueryui.com/](http://jqueryui.com/).

<a id="_Toc2_2"></a>
### <a name="added-additionalmetadataattribute-class"></a>"AdditionalMetadataAttribute"-Klasse hinzugefügt

Sie können die *AdditionalMetadataAttribute* -Klasse verwenden, um das *Model Metadata. additionalValues* -Wörterbuch für eine Modell Eigenschaft aufzufüllen.

Angenommen, ein Ansichts Modell verfügt über Eigenschaften, die nur einem Administrator angezeigt werden sollen. Dieses Modell kann mit dem neuen Attribut kommentiert werden, wobei adminonly als Schlüssel und true als Wert verwendet wird, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](mvc3-release-notes/samples/sample9.cs)]

Diese Metadaten werden für jede Anzeige-oder Editor Vorlage verfügbar gemacht, wenn ein Produkt Ansichts Modell gerendert wird. Sie sind als Anwendungsentwickler dabei, die Metadateninformationen zu interpretieren.

<a id="_Toc2_3"></a>
### <a name="improved-view-scaffolding"></a>Verbessertes Ansichts Gerüst

Die T4-Vorlagen, die für Gerüstbau Ansichten verwendet werden, generieren jetzt Aufrufe von Vorlagen Hilfsobjekten,wie z. b. *EditorFor* anstelle von Hilfsobjekten Diese Änderung verbessert die Unterstützung für Metadaten für das Modell in Form von Daten Anmerkung-Attributen, wenn im Dialogfeld Ansicht hinzufügen eine Ansicht generiert wird.

Das Gerüst zum Hinzufügen von Sichten umfasst auch eine verbesserte Erkennung und Verwendung von Primärschlüssel Informationen für das Modell, basierend auf der Konvention. Beispielsweise werden im Dialogfeld Ansicht hinzufügen diese Informationen verwendet, um sicherzustellen, dass der Primärschlüssel Wert nicht als bearbeitbares Formularfeld festgelegt wird.

Die Standardvorlagen für bearbeiten und erstellen enthalten Verweise auf die jQuery-Skripts, die für die Client Validierung benötigt werden.

<a id="_Toc2_4"></a>
### <a name="added-htmlraw-method"></a>Hinzugefügte HTML. RAW-Methode

Standardmäßig codiert die Razor-Ansichts-Engine alle Werte in HTML. Der folgende Code Ausschnitt codiert z. b. den HTML-Code in der Begrüßungs Variablen, sodass er auf der Seite als `<strong>Hello World!</strong>`angezeigt wird.

[!code-cshtml[Main](mvc3-release-notes/samples/sample10.cshtml)]

Die neue *HTML. RAW* -Methode bietet eine einfache Möglichkeit, nicht codiertes HTML anzuzeigen, wenn der Inhalt als sicher bekannt ist. Im folgenden Beispiel wird dieselbe Zeichenfolge angezeigt, aber die Zeichenfolge wird als Markup gerendert:

[!code-cshtml[Main](mvc3-release-notes/samples/sample11.cshtml)]

<a id="_Toc2_5"></a>
### <a name="renamed-controllerviewmodel-property-and-the-view-property-to-viewbag"></a>Umbenennen der Eigenschaft "Controller. ViewModel" und der Eigenschaft "View" in "viewbag"

Zuvor entsprach die *ViewModel* -Eigenschaft des *Controllers* der *View* -Eigenschaft einer Ansicht. Beide Eigenschaften bieten eine Möglichkeit, auf die Werte des *viewdatadictionary* -Objekts mithilfe der dynamischen Eigenschaft-Accessor-Syntax zuzugreifen. Beide Eigenschaften wurden so umbenannt, dass Sie identisch sind, um Verwechslungen zu vermeiden und konsistentere zu sein.

<a id="_Toc2_6"></a>
### <a name="renamed-controllersessionstateattribute-class-to-sessionstateattribute"></a>Umbenennen der Klasse "controllersessionstateattribute" in "sessionstateattribute"

Die *controllersessionstateattribute* -Klasse wurde in der RC-Version von ASP.NET MVC 3 eingeführt. Die Eigenschaft wurde in eine kompaktere Eigenschaft umbenannt.

<a id="_Toc2_7"></a>
### <a name="renamed-remoteattribute-fields-property-to-additionalfields"></a>Umbenannte remoteattribute-Eigenschaft "Fields" in "additionalfields"

Die *Fields* -Eigenschaft der *remoteattribute* -Klasse verursachte Verwirrung zwischen den Benutzern. Durch das Umbenennen dieser Eigenschaft in *additionalfields* wird ihre Absicht verdeutlicht.

<a id="_Toc2_8"></a>
### <a name="renamed-skiprequestvalidationattribute-to-allowhtmlattribute"></a>Umbenennen von "skiprequestvalidationattribute" in "zugwhtmlattribute"

Das *skiprequestvalidationattribute* -Attribut wurde in ' *zugswhtmlattribute* ' umbenannt, um die beabsichtigte Verwendung besser darzustellen.

<a id="_Toc2_9"></a>
### <a name="changed-htmlvalidationmessage-method-to-display-the-first-useful-error-message"></a>Die "HTML. validationmessage"-Methode wurde geändert, um die erste nützliche Fehlermeldung anzuzeigen.

Die *HTML. validationmessage* -Methode wurde korrigiert, um die erste nützliche Fehlermeldung anzuzeigen, anstatt einfach den ersten Fehler anzuzeigen.

Während der Modell Bindung kann das *Model State* -Wörterbuch aus mehreren Quellen mit Fehlermeldungen über die-Eigenschaft aufgefüllt werden, einschließlich des Modells selbst (wenn es *ivalidatableobject*implementiert), von Validierungs Attributen, die auf die-Eigenschaft angewendet werden, und von Ausnahmen, die ausgelöst werden, während auf die Eigenschaft zugegriffen wird.

Wenn die *HTML. validationmessage* -Methode eine Validierungs Meldung anzeigt, überspringt Sie Modell Zustands Einträge, die eine Ausnahme enthalten, da diese in der Regel nicht für den Endbenutzer vorgesehen sind. Stattdessen sucht die Methode nach der ersten Validierungs Nachricht, die keiner Ausnahme zugeordnet ist, und zeigt diese Meldung an. Wenn eine solche Nachricht nicht gefunden wird, wird standardmäßig eine generische Fehlermeldung angezeigt, die der ersten Ausnahme zugeordnet ist.

<a id="_Toc2_10"></a>
### <a name="fixed-model-declaration-to-not-add-whitespace-to-the-document"></a>Korrigiert @model Deklaration, um dem Dokument keine Leerzeichen hinzuzufügen.

In früheren Versionen wurde der gerenderten HTML-Ausgabe durch die `@model` Deklaration am oberen Rand einer Sicht eine leere Zeile hinzugefügt. Dies wurde korrigiert, sodass die Deklaration keine Leerzeichen einführt.

<a id="_Toc2_11"></a>
### <a name="added-fileextensions-property-to-view-engines-to-support-engine-specific-file-names"></a>"File Extensions"-Eigenschaft wurde hinzugefügt, um Engines anzuzeigen, um Engine-spezifische Dateinamen zu unterstützen

Eine Ansichts-Engine kann eine Ansicht mithilfe eines expliziten Ansichts Pfads zurückgeben, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](mvc3-release-notes/samples/sample12.cs)]

Die erste Ansichts-Engine versucht immer, die Ansicht zu Rendering. Standardmäßig ist die Web Forms Ansichts-Engine die erste Ansichts-Engine. Da die Web Forms-Engine keine Razor-Ansicht renderingansicht erzeugen kann, tritt ein Fehler auf Ansichts-Engines verfügen jetzt über eine *FileExtensions* -Eigenschaft, die verwendet wird, um anzugeben, welche Dateierweiterungen Sie unterstützen. Diese Eigenschaft wird überprüft, wenn ASP.NET bestimmt, ob eine Ansichts-Engine eine Datei renderingkann. Dies ist eine Breaking Change, und weitere Details finden Sie im Abschnitt "wichtige [Änderungen](#_Toc2_BC) " in diesem Dokument.

<a id="_Toc2_12"></a>
### <a name="fixed-labelfor-helper-to-emit-the-correct-value-for-the-for-attribute"></a>Das Hilfsprogramm "LabelFor" wurde korrigiert, um den korrekten Wert für das "for"-Attribut auszugeben.

Ein Fehler wurde behoben, bei dem die *LabelFor* -Methode ein *for* -Attribut gerendert hat, das dem *Name* -Attribut des *Input* -Elements anstelle der ID entspricht. Gemäß W3C muss das *for* -Attribut mit der ID des *Eingabe* Elements übereinstimmen.

<a id="_Toc2_13"></a>
### <a name="fixed-renderaction-method-to-give-explicit-values-precedence-during-model-binding"></a>Die "renderaction"-Methode wurde korrigiert, um bei der Modell Bindung explizite Werte zu vergeben.

In früheren Versionen wurden explizite Werte, die an die *renderaction* -Methode weitergegeben wurden, zugunsten der aktuellen Formular Werte während der Modell Bindung innerhalb einer untergeordneten Aktion ignoriert. Durch die Korrektur wird sichergestellt, dass explizite Werte während der Modell Bindung Vorrang haben.

<a id="_Toc2_BC"></a>
## <a name="breaking-changes"></a>Die Lauffähigkeit der Anwendung beeinträchtigende Änderungen

- In früheren Versionen von ASP.NET MVC wurden Aktionsfilter pro Anforderung außer in einigen Fällen erstellt. Dieses Verhalten war nie ein garantiertes Verhalten, aber nur ein Implementierungsdetail, und der Vertrag für Filter war, Sie zustandslos zu überdenken. In ASP.NET MVC 3 werden Filter aggressiver zwischengespeichert. Daher können alle benutzerdefinierten Aktionsfilter, die den Instanzzustand nicht ordnungsgemäß speichern, beschädigt werden.
- Die Ausführungsreihenfolge für Ausnahme Filter hat sich bei Ausnahme Filtern mit dem gleichen *Reihen* folgen Wert geändert. In ASP.NET MVC 2 und früheren Versionen wurden Ausnahme Filter auf dem Controller, die denselben *Bestell* Wert wie die in einer Aktionsmethode hatten, vor der Ausnahme Filter für die Aktionsmethode ausgeführt. Dies wäre in der Regel der Fall, wenn Ausnahme Filter ohne einen angegebenen *Bestell* Wert angewendet wurden. In ASP.NET MVC 3 wurde diese Reihenfolge umgekehrt, sodass der spezifischere Ausnahmehandler zuerst ausgeführt wird. Wie in früheren Versionen werden die Filter in der angegebenen Reihenfolge ausgeführt, wenn die *Order* -Eigenschaft explizit angegeben wird.
- Der Basisklasse " *virtualpathproviderviewengine* " wurde eine neue Eigenschaft mit dem Namen " *File Extensions* " hinzugefügt. Wenn ASP.net eine Ansicht nach Pfad (nicht nach Name) sucht, werden nur Sichten mit einer Dateierweiterung berücksichtigt, die in der von dieser neuen Eigenschaft angegebenen Liste enthalten sind. Dies ist ein Breaking Change in Anwendungen, in denen ein benutzerdefinierter Buildanbieter registriert ist, um eine benutzerdefinierte Dateierweiterung für Webformular Sichten zu aktivieren, und wo der Anbieter auf diese Sichten verweist, indem anstelle eines Namens ein vollständiger Pfad verwendet wird. Die Problem Umgehung besteht darin, den Wert der *FileExtensions* -Eigenschaft so zu ändern, dass Sie die benutzerdefinierte Dateierweiterung einschließt.
- Benutzerdefinierte Controller-Factory-Implementierungen, die die *IControllerFactory* -Schnittstelle direkt implementieren, müssen eine Implementierung der neuen *getcontrollersessionbehavior* -Methode bereitstellen, die der-Schnittstelle in dieser Version hinzugefügt wurde. Im Allgemeinen wird empfohlen, dass Sie diese Schnittstelle nicht direkt implementieren und stattdessen die Klasse von *defaultcontrollerfactory*ableiten.

<a id="_Toc2_KI"></a>
## <a name="known-issues"></a>Bekannte Probleme

- Der ASP.NET MVC 3-Installer kann nur eine anfängliche Version des nuget-Paket-Managers installieren. Nachdem Sie die anfängliche Version installiert haben, kann nuget mit dem Erweiterungs-Manager von Visual Studio installiert und aktualisiert werden. Wenn Sie nuget bereits installiert haben, navigieren Sie zum Visual Studio-Erweiterungs Katalog, um ein Update auf die neueste Version von nuget durchzusetzen.
- Das Erstellen eines neuen ASP.NET MVC 3-Projekts in einem Projektmappenordner verursacht einen *NullReferenceException* -Fehler. Die Problem Umgehung besteht darin, das ASP.NET MVC 3-Projekt im Stammverzeichnis der Projekt Mappe zu erstellen und dann in den Projektmappenordner zu verschieben.
- Das Installationsprogramm kann deutlich länger dauern als vorherige Versionen von ASP.NET MVC. Dies liegt daran, dass die Komponenten von Visual Studio 2010 aktualisiert werden.
- IntelliSense für Razor-Syntax funktioniert nicht, wenn reschärfere installiert ist. Wenn Sie installiert haben und die Razor IntelliSense-Unterstützung in ASP.NET MVC 3 rc2 nutzen möchten, lesen Sie den Eintrag [Razor IntelliSense und reschärfere](https://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) im Blog von Hadi Hari, in dem erläutert wird, wie Sie diese heute verwenden können.
- Für cshtml-und vbhtml-Sichten, die mit der Beta Version von ASP.NET MVC 3 erstellt wurden, ist die Buildaktion nicht ordnungsgemäß festgelegt, und das Ergebnis ist, dass diese Ansichts Typen bei der Veröffentlichung des Projekts ausgelassen werden. Der *buildaktionswert* für diese Dateien sollte auf "Content" festgelegt werden. ASP.NET MVC 3 rc2 korrigiert dieses Problem für neue Dateien, korrigiert jedoch nicht die Einstellung für vorhandene Dateien für ein Projekt, das mit der Beta Version erstellt wurde.![](mvc3-release-notes/_static/image4.png)
- Während der Installation werden im Dialogfeld EULA-Annahme die Lizenzbedingungen in einem Fenster angezeigt, das kleiner als beabsichtigt ist.
- Wenn Sie eine Razor-Ansicht (cshtml-Datei) bearbeiten, ist das Menü Element gehe zu Controller in Visual Studio nicht verfügbar, und es sind keine Code Ausschnitte vorhanden.
- Wenn Sie ASP.NET MVC 3 für Visual Web Developer Express auf einem Computer installieren, auf dem Visual Studio nicht installiert ist, und später Visual Studio installieren, müssen Sie ASP.NET MVC 3 neu installieren. Visual Studio und Visual Web Developer Express Teilen Komponenten auf, die mit dem ASP.NET MVC 3-Installer aktualisiert werden. Dasselbe Problem tritt auf, wenn Sie ASP.NET MVC 3 für Visual Studio auf einem Computer installieren, auf dem Visual Web Developer Express nicht installiert ist, und anschließend Visual Web Developer Express später installieren.
- Bei der Installation von ASP.NET MVC 3 RC 2 wird nuget nicht aktualisiert, wenn es bereits installiert ist. Wechseln Sie zum Aktualisieren von nuget zum Visual Studio-Erweiterungs-Manager und sollte als verfügbares Update angezeigt werden. Sie können das nuget-Upgrade auf das neueste Release von dort aus durchführen.

<a id="TOC_ASP_NET_3_RC"></a>
## <a name="aspnet-mvc-3-release-candidate"></a>ASP.NET MVC 3 Release Candidate

ASP.NET MVC Release Candidate wurde am 9. November 2010 veröffentlicht.

<a id="_Toc276711785"></a>
## <a name="new-features-in-aspnet-mvc-3-rc"></a>Neue Features in ASP.NET MVC 3 RC

In diesem Abschnitt werden Funktionen beschrieben, die seit der Beta Version in der ASP.NET MVC 3 RC-Version eingeführt wurden.

<a id="_Toc276711786"></a>
### <a name="nuget-package-manager"></a>NuGet-Paket-Manager

ASP.NET MVC 3 enthält den nuget-Paket-Manager (früher als nupack bezeichnet), ein integriertes Paket Verwaltungs Tool zum Hinzufügen von Bibliotheken und Tools zu Visual Studio-Projekten. Dieses Tool automatisiert die Schritte, die Entwickler heute durchführen, um eine Bibliothek in ihrer Quell Struktur zu erhalten.

Sie können mit nuget als Befehlszeilen Tool, als integriertes Konsolenfenster in Visual Studio 2010, über das Visual Studio-Kontextmenü und als eine Reihe von PowerShell-Cmdlets arbeiten.

Weitere Informationen zu nuget finden Sie in der [nuget-Dokumentation](https://docs.microsoft.com/nuget/).

<a id="_Toc276711787"></a>
### <a name="improved-new-project-dialog-box"></a>Verbessertes Dialog Feld "Neues Projekt"

Wenn Sie ein neues Projekt erstellen, können Sie im Dialogfeld "Neues Projekt" jetzt die Ansichts-Engine und einen ASP.NET MVC-Projekttyp angeben.

![](mvc3-release-notes/_static/image5.png)

Die Unterstützung für das Ändern der Liste der im Dialogfeld aufgeführten Vorlagen und Ansichts-Engines ist in dieser Version enthalten.

Die Standardvorlagen lauten wie folgt:

Leer. Enthält einen minimalen Satz von Dateien für ein ASP.NET-MVC-Projekt, einschließlich der Standardverzeichnis Struktur für ASP.NET MVC-Projekte, einer Site. CSS-Datei, die die standardmäßigen ASP.NET MVC-Stile enthält, und einem Scripts-Verzeichnis, das die standardmäßigen JavaScript-Dateien enthält.

Internet Anwendung. Enthält Beispiel Funktionen, die die Verwendung des Mitgliedschafts Anbieters mit ASP.NET MVC veranschaulichen.

Die Liste der Projektvorlagen, die im Dialogfeld angezeigt wird, wird in der Windows-Registrierung angegeben.

<a id="_Toc276711788"></a>
### <a name="sessionless-controllers"></a>Sessionless-Controller

Das neue *controllersessionstateattribute* bietet Ihnen mehr Kontrolle über das Sitzungs Zustands Verhalten für Controller, indem ein [System. Web. SessionState. SessionStateBehavior](https://msdn.microsoft.com/library/system.web.sessionstate.sessionstatebehavior.aspx) -Enumerationswert angegeben wird.

Im folgenden Beispiel wird gezeigt, wie der Sitzungszustand für alle Anforderungen an einen Controller deaktiviert wird.

[!code-csharp[Main](mvc3-release-notes/samples/sample13.cs)]

Im folgenden Beispiel wird gezeigt, wie der schreibgeschützte Sitzungszustand für alle Anforderungen an einen Controller festgelegt wird.

[!code-csharp[Main](mvc3-release-notes/samples/sample14.cs)]

<a id="_Toc276711789"></a>
### <a name="new-validation-attributes"></a>Neue Validierungs Attribute

#### <a name="compareattribute"></a>Compareattribute

Mit dem neuen *compareattribute* -Validierungs Attribut können Sie die Werte von zwei verschiedenen Eigenschaften eines Modells vergleichen. Im folgenden Beispiel muss die *ComparePassword* -Eigenschaft dem Kenn *Wort* Feld entsprechen, damit Sie gültig ist.

[!code-csharp[Main](mvc3-release-notes/samples/sample15.cs)]

#### <a name="remoteattribute"></a>Remoteattribute

Das neue *remoteattribute* -Validierungs Attribut nutzt das Remote Validierungs Steuerelement des jQuery-Validierungs-Plug-ins, das es der Client seitigen Validierung ermöglicht, eine Methode auf dem Server aufzurufen, der die tatsächliche Validierungs Logik ausführt.

Im folgenden Beispiel ist die Eigenschaft " *username* " auf " *remoteattribute* " angewendet. Wenn Sie diese Eigenschaft in einer Bearbeitungs Ansicht bearbeiten, ruft die Client Validierung eine Aktion mit dem Namen *usernameavailable* in der *userscontroller* -Klasse auf, um dieses Feld zu validieren.

[!code-csharp[Main](mvc3-release-notes/samples/sample16.cs)]

Das folgende Beispiel zeigt den entsprechenden Controller.

[!code-csharp[Main](mvc3-release-notes/samples/sample17.cs)]

Standardmäßig wird der Name der Eigenschaft, auf die das Attribut angewendet wird, als Abfrage Zeichen folgen Parameter an die Aktionsmethode gesendet.

<a id="_Toc276711790"></a>
### <a name="new-overloads-for-labelfor-and-labelformodel-methods"></a>Neue über Ladungen für die Methoden "LabelFor" und "labelformodel"

Es wurden neue über Ladungen für die Methoden " *LabelFor* " und " *labelformodel* " hinzugefügt, mit denen Sie den Bezeichnungs Text angeben können. Im folgenden Beispiel wird gezeigt, wie diese über Ladungen verwendet werden.

[!code-cshtml[Main](mvc3-release-notes/samples/sample18.cshtml)]

<a id="_Toc276711791"></a>
### <a name="child-action-output-caching"></a>Ausgabe Zwischenspeichern von untergeordneten Aktionen

*Outputcacheattribute* unterstützt die Ausgabe Zwischenspeicherung von untergeordneten Aktionen, die mithilfe der *HTML. renderaction* -oder *HTML. Action* -Hilfsmethoden aufgerufen werden. Im folgenden Beispiel wird eine Ansicht gezeigt, die eine andere Aktion aufruft.

[!code-cshtml[Main](mvc3-release-notes/samples/sample19.cshtml)]

Die *GETDATE* -Aktion wird mit *outputcacheattribute*kommentiert:

[!code-csharp[Main](mvc3-release-notes/samples/sample20.cs)]

Wenn dieser Code ausgeführt wird, wird das Ergebnis des Aufrufens von HTML. Action ("GETDATE") für 100 Sekunden zwischengespeichert.

<a id="_Toc276711792"></a>
### <a name="add-view-dialog-box-improvements"></a>Verbesserungen im Dialog Feld "Ansicht hinzufügen"

Wenn Sie eine stark typisierte Ansicht hinzufügen, werden im Dialogfeld Ansicht hinzufügen jetzt mehr nicht zutreffende Typen herausgefiltert als in früheren Versionen, wie z. b. viele Kerne .NET Framework Typen. Außerdem wird die Liste nun nach dem Klassennamen und nicht nach dem voll qualifizierten Typnamen sortiert, was das Auffinden von Typen erleichtert. Beispielsweise wird der Typname nun wie im folgenden Beispiel angezeigt:

ClassName (Namespace)

In früheren Versionen wurde dies wie folgt angezeigt:

Namespace.ClassName

<a id="_Toc276711793"></a>
### <a name="granular-request-validation"></a>Granulare Anforderungs Validierung

Die *Exclude* -Eigenschaft von *validateinputattribute* ist nicht mehr vorhanden. Verwenden Sie stattdessen das neue *skiprequestvalidationattribute*, damit die Anforderungs Validierung für bestimmte Eigenschaften eines Modells während der Modell Bindung übersprungen wird.

Nehmen wir beispielsweise an, dass eine Aktionsmethode zum Bearbeiten eines Blogbeitrags verwendet wird:

[!code-csharp[Main](mvc3-release-notes/samples/sample21.cs)]

Das folgende Beispiel zeigt das Ansichts Modell für einen Blogbeitrag.

[!code-csharp[Main](mvc3-release-notes/samples/sample22.cs)]

Wenn ein Benutzer ein Markup für die Description-Eigenschaft übermittelt, schlägt die Modell Bindung aufgrund der Anforderungs Validierung fehl. Um die Anforderungs Validierung während der Modell Bindung für die Beschreibung des Blogbeitrags zu deaktivieren, wenden Sie das *skiprequpeer-ValidationAttribute* auf die-Eigenschaft an, wie im folgenden Beispiel gezeigt:.

[!code-csharp[Main](mvc3-release-notes/samples/sample23.cs)]

Wenn Sie die Anforderungs Validierung für jede Eigenschaft des Modells deaktivieren möchten, wenden Sie das *validateinputattribute-Attribut* mit dem Wert *false* auf die Aktionsmethode an:

[!code-csharp[Main](mvc3-release-notes/samples/sample24.cs)]

<a id="_Toc276711794"></a>
## <a name="breaking-changes"></a>Die Lauffähigkeit der Anwendung beeinträchtigende Änderungen

- Die Ausführungsreihenfolge für Ausnahme Filter hat sich bei Ausnahme Filtern mit dem gleichen *Reihen* folgen Wert geändert. In ASP.NET MVC 2 und früheren Versionen wurden Ausnahme Filter auf dem Controller, die dieselbe *Reihenfolge* wie die in einer Aktionsmethode hatten, vor der Ausnahme Filter für die Aktionsmethode ausgeführt. Dies wäre in der Regel der Fall, wenn Ausnahme Filter ohne einen angegebenen *Bestell* Wert angewendet wurden. In ASP.NET MVC 3 wurde diese Reihenfolge umgekehrt, sodass der spezifischere Ausnahmehandler zuerst ausgeführt wird. Wie in früheren Versionen werden die Filter in der angegebenen Reihenfolge ausgeführt, wenn die *Order* -Eigenschaft explizit angegeben wird.
- Der Basisklasse " *virtualpathproviderviewengine* " wurde eine neue Eigenschaft mit dem Namen " *FileExtensions* " hinzugefügt. Beim Suchen nach einer Ansicht nach Pfad (und nicht nach Name) werden nur Sichten mit einer Dateierweiterung berücksichtigt, die in der von dieser neuen Eigenschaft angegebenen Liste enthalten sind. Dies ist ein Breaking Change für Benutzer, die einen benutzerdefinierten Buildanbieter registrieren, um eine benutzerdefinierte Dateierweiterung für Web Form-Sichten zu aktivieren, und auf diese Sichten mit einem vollständigen Pfad anstelle eines Namens verweisen. Die Problem Umgehung besteht darin, den Wert der *FileExtensions* -Eigenschaft so zu ändern, dass Sie die benutzerdefinierte Dateierweiterung einschließt.

<a id="_Toc276711795"></a>
## <a name="known-issues"></a>Bekannte Probleme

- Das Installationsprogramm kann deutlich länger dauern als frühere Versionen von ASP.NET MVC, da es Komponenten von Visual Studio 2010 aktualisiert.
- Das Hinzufügen von Ansichts Gerüstbau bei der Auswahl von schreibgeschützten Eigenschaften für das Ansichts Gerüst. Diese sollten immer durch Gerüstbau ignoriert werden. Im Dialogfeld Ansicht hinzufügen werden bei der Erstellung der Ansicht "Bearbeiten" oder "erstellen" auch schreibgeschützte Eigenschaften erstellt. Für schreibgeschützte Eigenschaften sollte nur ein Gerüst für die Anzeige-und Listenansichten angezeigt werden.
- Das Debuggen funktioniert nicht, wenn ASP.NET MVC 3 neben der Async-CTP-Version installiert wird. ASP.NET MVC 3 kann nicht parallel mit der CTP-Version Async installiert werden. Deinstallieren Sie die asynchrone CTP zum Reparieren des Debuggens Weitere Informationen finden Sie in [diesem Blogbeitrag](http://drew-prog.blogspot.com/2010/11/how-to-uninstall-microsoft-aspnet-mvc-3.html) zum Deinstallieren aller ASP.NET MVC 3 RC-Elemente.
- Razor IntelliSense funktioniert nicht, wenn reschärfere installiert ist. Wenn Sie installiert haben und die Unterstützung von Razor IntelliSense in ASP.NET MVC 3 RC nutzen möchten, lesen Sie [diesen Blogbeitrag](https://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) von JetBrains, in dem erläutert wird, wie Sie Sie noch heute verwenden können.
- Cshtml-und vbhtml-Ansichten, die mit der Beta Version von ASP.NET MVC 3 erstellt wurden, verfügen nicht über die ordnungsgemäße Buildaktion, die die Veröffentlichung auslässt. Die *Buildaktion* für diese Dateien sollte auf "Content" festgelegt werden. ASP.NET MVC 3 RC korrigiert dieses Problem für neue Dateien, korrigiert jedoch nicht die Einstellung für vorhandene Dateien für ein Projekt, das mit der Beta Version erstellt wurde.
- Das Installationsprogramm kann deutlich länger dauern als frühere Versionen von ASP.NET MVC, da es Komponenten von Visual Studio 2010 aktualisiert.
- Das Hinzufügen von Ansichts Gerüstbau bei der Auswahl eines "Bearbeiten"-Gerüstbau für stark typisierte Ansichten Ebenso werden schreibgeschützte Eigenschaften für "Anzeige"-Sichten als Gerüst angezeigt.
- Während der Installation werden im Dialogfeld EULA-Annahme die Lizenzbedingungen in einem Fenster angezeigt, das kleiner als beabsichtigt ist.
- Die Installation von Visual Studio Async CTP verursacht einen Konflikt mit der Razor-Version, die im Rahmen der Installation von ASP.NET MVC 3-Tools enthalten ist. Stellen Sie sicher, dass Sie nicht versuchen, die CTP-Version von Visual Studio Async und die Razor-Version auf demselben Computer zu installieren.
- Wenn Sie eine Razor-Ansicht (cshtml-Datei) bearbeiten, ist das Menü Element gehe zu Controller in Visual Studio nicht verfügbar, und es sind keine Code Ausschnitte vorhanden.

<a id="TOC_ASP_NET_3_Beta"></a>
## <a name="aspnet-mvc-3-beta"></a>ASP.NET MVC 3 Beta

ASP.NET MVC 3 Beta wurde am 6. Oktober 2010 veröffentlicht. Die folgenden Hinweise gelten speziell für die Beta Version und unterliegen allen Updates oder Änderungen, auf die im obigen Abschnitt ASP.NET MVC 3 Release Candidate verwiesen wird.

## <a id="0.1__Toc274034215"></a>Neue featuresin ASP.NET MVC 3 Beta

<a id="0.1__Default_validation_system"></a>In diesem Abschnitt werden Funktionen beschrieben, die in der ASP.NET MVC 3 Beta-Version eingeführt wurden.

### <a id="0.1__Toc274034216"></a>Nuget-Paket-Manager

ASP.NET MVC 3 umfasst den nuget-Paket-Manager, ein integriertes Paket Verwaltungs Tool zum Hinzufügen von Bibliotheken und Tools zu Visual Studio-Projekten. Größtenteils werden die Schritte, die Entwickler heute ausführen, automatisiert, um eine Bibliothek in Ihre Quell Struktur zu übernehmen.

Sie können mit nuget als Befehlszeilen Tool, als integriertes Konsolenfenster in Visual Studio 2010, über das Visual Studio-Kontextmenü und in Form von PowerShell-Cmdlets arbeiten.

Weitere Informationen zu nuget finden Sie in der [nuget-Dokumentation](https://docs.microsoft.com/nuget/).

### <a id="0.1__Toc274034217"></a>Verbessertes Dialog Feld "Neues Projekt"

Wenn Sie ein neues Projekt erstellen, können Sie im Dialogfeld "Neues Projekt" jetzt die Ansichts-Engine und einen ASP.NET MVC-Projekttyp angeben.

![](mvc3-release-notes/_static/image6.png)

Die Unterstützung für das Ändern der Liste der im Dialogfeld aufgeführten Vorlagen und Ansichts-Engines ist in dieser Version nicht enthalten.

Die Standardvorlagen lauten wie folgt:

Leer. Enthält einen minimalen Satz von Dateien für ein ASP.NET-MVC-Projekt, einschließlich der Standardverzeichnis Struktur für ASP.NET MVC-Projekte, einer kleinen Site. CSS-Datei, die die standardmäßigen ASP.NET MVC-Stile enthält, und einem Scripts-Verzeichnis, das die standardmäßigen JavaScript-Dateien enthält.

Internet Anwendung. Enthält Beispiel Funktionen, die die Verwendung des Mitgliedschafts Anbieters in ASP.NET MVC veranschaulichen.

### <a id="0.1__Toc274034218"></a>Vereinfachte Methode zum angeben stark typisierter Modelle in Razor-Ansichten

Die Möglichkeit, den Modelltyp für stark typisierte Razor-Sichten anzugeben, wurde durch die Verwendung der neuen @model-Direktive für cshtml-Sichten und @ModelType Direktive für vbhtml-Sichten vereinfacht. In früheren Versionen von ASP.NET MVC haben Sie ein stark typisiertes Modell für Razor-Ansichten auf diese Weise angegeben:

[!code-cshtml[Main](mvc3-release-notes/samples/sample25.cshtml)]

In dieser Version können Sie die folgende Syntax verwenden:

[!code-cshtml[Main](mvc3-release-notes/samples/sample26.cshtml)]

### <a id="0.1__Toc274034219"></a>Unterstützung für neue ASP.net Web Pages-Hilfsmethoden

Die neue ASP.net Web Pages-Technologie umfasst eine Reihe von Hilfsmethoden, die für das Hinzufügen häufig verwendeter Funktionen zu Sichten und Controllern nützlich sind. ASP.NET MVC 3 unterstützt die Verwendung dieser Hilfsmethoden in Controllern und Ansichten (falls zutreffend). Diese Methoden sind in der Assembly "System. Web. Hilfsprogramme" enthalten. In der folgenden Tabelle sind einige der ASP.net Web Pages Hilfsmethoden aufgeführt.

| **Helper** | **Beschreibung** |
| --- | --- |
| Diagramm | Rendert ein Diagramm in einer Ansicht. Enthält Methoden wie z. b. "Chart. dewebimage", "Chart. Save" und "Chart. Write". |
| Crypto | Verwendet Hash Algorithmen, um korrekt gesalzene und Hashwerte zu erstellen. |
| WebGrid | Rendert eine Auflistung von-Objekten (in der Regel Daten aus einer Datenbank) als Raster. Unterstützt das Paging und sortieren. |
| WebImage | Rendert ein Bild. |
| WebMail | Sendet eine E-Mail. |

Ein kurz Referenz Thema, das die Hilfsprogramme und grundlegende Syntax auflistet, ist als Teil der ASP.net-Razor-Syntax Dokumentation unter der folgenden URL verfügbar:

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-api-reference](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)

### <a id="0.1__Toc274034220"></a>Zusätzliche Unterstützung der Abhängigkeitsinjektion

Die aktuelle Version, die auf der ASP.NET MVC 3 Preview 1-Version aufbaut, bietet zusätzliche Unterstützung für zwei neue Dienste und vier vorhandene Dienste sowie verbesserte Unterstützung für die Abhängigkeitsauflösung und den allgemeinen Dienstlocator.

#### <a name="new-icontrolleractivator-interface-for-fine-grained-controller-instantiation"></a>Neue icontrolleractivator-Schnittstelle für differenzierte Controller Instanziierung

Die neue icontrolleractivator-Schnittstelle bietet eine präzisere Steuerung der Art und Weise, wie Controller mithilfe von Abhängigkeitsinjektion instanziiert werden. Das folgende Beispiel zeigt die-Schnittstelle:

[!code-csharp[Main](mvc3-release-notes/samples/sample27.cs)]

Vergleichen Sie dies mit der Rolle der controllerfactory. Eine controllerfactory ist eine Implementierung der IControllerFactory-Schnittstelle, die sowohl für die Suche nach einem Controllertyp als auch für das Instanziieren einer Instanz dieses Controller Typs verantwortlich ist.

Controller Aktivierer sind nur für das Instanziieren einer Instanz eines Controller Typs zuständig. Sie führen die Suche nach dem Controllertyp nicht durch. Nach dem Auffinden eines richtigen Controller Typs sollten Controller Factorys an eine Instanz von icontrolleractivator delegieren, um die tatsächliche Instanziierung des Controllers zu verarbeiten.

Die defaultcontrollerfactory-Klasse verfügt über einen neuen Konstruktor, der eine IControllerFactory-Instanz akzeptiert. Auf diese Weise können Sie eine Abhängigkeitsinjektion anwenden, um diesen Aspekt der Controller Erstellung zu verwalten, ohne dass das standardmäßige Suchverhalten des Controller Typs überschrieben werden muss.

#### <a name="iservicelocator-interface-replaced-with-idependencyresolver"></a>Iservicelocator-Schnittstelle ersetzt durch idepdencyresolver

Basierend auf communityfeedback hat die ASP.NET MVC 3 Beta-Version die Verwendung der iservicelocator-Schnittstelle durch eine abgespeckte idepdencyresolver-Schnittstelle ersetzt, die für die Anforderungen von ASP.NET MVC spezifisch ist. Das folgende Beispiel zeigt die neue-Schnittstelle:

[!code-csharp[Main](mvc3-release-notes/samples/sample28.cs)]

Im Rahmen dieser Änderung wurde die ServiceLocator-Klasse ebenfalls durch die DependencyResolver-Klasse ersetzt. Die Registrierung eines Abhängigkeits Konflikt Lösers ähnelt früheren Versionen von ASP.NET MVC:

[!code-csharp[Main](mvc3-release-notes/samples/sample29.cs)]

Implementierungen dieser Schnittstelle sollten einfach an den zugrunde liegenden Container für die Abhängigkeitsinjektion delegiert werden, um den registrierten Dienst für den angeforderten Typ bereitzustellen.

Wenn keine registrierten Dienste des angeforderten Typs vorhanden sind, erwartet ASP.NET MVC, dass Implementierungen dieser Schnittstelle NULL von GetService zurückgeben und eine leere Auflistung von GetServices zurückgeben.

Die neue DependencyResolver-Klasse ermöglicht Ihnen das Registrieren von Klassen, die entweder die neue idepdencyresolver-Schnittstelle oder die Common Service Locator-Schnittstelle (iservicelocator) implementieren. Weitere Informationen zum allgemeinen Dienstlocator finden Sie unter [commonservicelocator auf GitHub](https://github.com/unitycontainer/commonservicelocator).

<a id="0.1__Breaking_Changes"></a>

#### <a name="new-iviewactivator-interface-for-fine-grained-view-page-instantiation"></a>Neue iviewactivator-Schnittstelle für differenzierte Ansichts Seiten Instanziierung

Die neue iviewpageactivator-Schnittstelle bietet eine präzisere Steuerung der instanziierten Ansichts Seiten mithilfe der Abhängigkeitsinjektion. Dies gilt sowohl für webformview-Instanzen als auch für razorview-Instanzen. Das folgende Beispiel zeigt die neue-Schnittstelle:

[!code-csharp[Main](mvc3-release-notes/samples/sample30.cs)]

Diese Klassen akzeptieren jetzt ein iviewpageactivator-Konstruktorargument, mit dem Sie Abhängigkeitsinjektion verwenden können, um zu steuern, wie die Typen ViewPage, ViewUserControl und webviewpage instanziiert werden.

#### <a name="new-dependency-resolver-support-for-existing-services"></a>Neue Abhängigkeits Konflikt Löser-Unterstützung für vorhandene Dienste

Die neue Version umfasst die Unterstützung der Abhängigkeitsauflösung für die folgenden Dienste:

- Modell Validierungs Anbieter. Klassen, die modelvalidatorprovider implementieren, können im Abhängigkeits Konflikt Löser registriert werden, und das System verwendet diese, um die Client-und serverseitige Validierung zu unterstützen.
- Der modellmetadatenanbieter. Eine einzelne Klasse, die ModelMetadataProvider implementiert, kann im Abhängigkeits Konflikt Löser registriert werden, und das System verwendet diese, um Metadaten für die Vorlagen-und Validierungs Systeme bereitzustellen.
- Wert Anbieter. Klassen, die valueproviderfactory implementieren, können im Abhängigkeits Konflikt Löser registriert werden, und das System verwendet diese zum Erstellen von Wert Anbietern, die vom Controller und während der Modell Bindung genutzt werden.
- Modell Binder. Klassen, die imodelbinderprovider implementieren, können im Abhängigkeits Konflikt Löser registriert werden, und das System verwendet diese, um Modell Binder zu erstellen, die vom Modell Bindungssystem verwendet werden.

### <a id="0.1__Toc274034221"></a>Neue Unterstützung für unaufdringliche jQuery-basiertes AJAX

ASP.NET MVC enthält AJAX-Hilfsmethoden wie die folgenden:

- Ajax.ActionLink
- Ajax.RouteLink
- AJAX. BeginForm
- Ajax.BeginRouteForm

Diese Methoden verwenden JavaScript, um eine Aktionsmethode auf dem Server aufzurufen, anstatt ein vollständiges Postback zu verwenden. Diese Funktion wurde aktualisiert, um jQuery auf unaufdringliche Weise zu nutzen. Anstatt Inline Client Skripts zu übermitteln, trennen diese Hilfsmethoden das Verhalten vom Markup, indem Sie HTML5-Attribute mit dem *Data-AJAX-* Präfix ausgeben. Das Verhalten wird dann auf das Markup angewendet, indem auf die entsprechenden JavaScript-Dateien verwiesen wird. Stellen Sie sicher, dass auf die folgenden JavaScript-Dateien verwiesen wird:

- jquery-1.4.1.js
- jquery.unobtrusive.ajax.js

Diese Funktion ist standardmäßig in der Datei "Web. config" in den neuen Projektvorlagen ASP.NET MVC 3 aktiviert, aber standardmäßig für vorhandene Projekte deaktiviert. Weitere Informationen finden Sie weiter unten in diesem Dokument unter [hinzugefügte Anwendungs weite Flags für die Client Validierung und unaufdringliches JavaScript](#0.1_AddedApplicationWideFlagsForClientValida) .

### <a id="0.1__Toc274034222"></a>Neue Unterstützung für unaufdringliche jquery-Validierung

Standardmäßig verwendet ASP.NET MVC 3 Beta die jQuery-Validierung auf unaufdringliche Weise, um eine Client seitige Validierung auszuführen. Um die unaufdringliche Client Validierung zu aktivieren, führen Sie einen-Befehl wie den folgenden aus einer Sicht aus:

[!code-csharp[Main](mvc3-release-notes/samples/sample31.cs)]

Hierfür ist es erforderlich, dass die ViewContext. unauffällig sivejavascriptenabled-Eigenschaft auf true festgelegt ist, was Sie tun können, indem Sie den folgenden-Befehl ausführen:

[!code-csharp[Main](mvc3-release-notes/samples/sample32.cs)]

Stellen Sie außerdem sicher, dass auf die folgenden JavaScript-Dateien verwiesen wird.

- jquery-1.4.1.js
- jquery.validate.js
- jquery.validate.unobtrusive.js

Diese Funktion ist standardmäßig in der Datei "Web. config" in den neuen Projektvorlagen ASP.NET MVC 3 aktiviert, aber standardmäßig für vorhandene Projekte deaktiviert. Weitere Informationen finden Sie weiter unten in diesem Dokument unter [neue Anwendungs weite Flags für die Client Validierung und unaufdringliches JavaScript](#0.1_AddedApplicationWideFlagsForClientValida) .

<a id="0.1__Toc274034223"></a>

### <a id="0.1_AddedApplicationWideFlagsForClientValida"></a>Neue Anwendungs weite Flags für die Client Validierung und unaufdringliches JavaScript

Sie können die Client Validierung und unaufdringliches JavaScript Global mithilfe statischer Member der htmlhelper-Klasse aktivieren bzw. deaktivieren, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](mvc3-release-notes/samples/sample33.cs)]

Die Standard Projektvorlagen aktivieren unaufdringliches JavaScript standardmäßig. Sie können diese Funktionen auch in der Stammdatei Web. config Ihrer Anwendung aktivieren oder deaktivieren, indem Sie die folgenden Einstellungen verwenden:

[!code-xml[Main](mvc3-release-notes/samples/sample34.xml)]

Da Sie diese Features standardmäßig aktivieren können, wurden neue über Ladungen in die htmlhelper-Klasse eingeführt, mit der Sie die Standardeinstellungen überschreiben können, wie in den folgenden Beispielen gezeigt:

[!code-csharp[Main](mvc3-release-notes/samples/sample35.cs)]

Aus Gründen der Abwärtskompatibilität sind beide Funktionen standardmäßig deaktiviert.

### <a id="0.1__Toc274034224"></a>Neue Unterstützung für Code, der vor der Ausführung von Sichten ausgeführt wird

Sie können jetzt eine Datei mit dem Namen \_viewstart. cshtml (oder \_viewstart. vbhtml) im Verzeichnis "Views" ablegen und Code hinzufügen, der von mehreren Ansichten in diesem Verzeichnis und seinen Unterverzeichnissen gemeinsam genutzt wird. Beispielsweise können Sie den folgenden Code auf der Seite \_viewstart. cshtml im Ordner ~/views einfügen:

[!code-cshtml[Main](mvc3-release-notes/samples/sample36.cshtml)]

Dadurch wird die Layoutseite für jede Ansicht im Ordner "Views" und alle zugehörigen Unterordner rekursiv festgelegt. Wenn eine Sicht gerendert wird, wird der Code in der \_viewstart. cshtml-Datei vor dem Ausführen des Ansichts Codes ausgeführt. Der \_viewstart. cshtml-Code gilt für jede Ansicht in diesem Ordner.

Standardmäßig gilt der Code in der \_viewstart. cshtml-Datei auch für Sichten in einem beliebigen Unterordner. Einzelne Unterordner können jedoch eine eigene Version der \_viewstart. cshtml-Datei aufweisen; in diesem Fall hat die lokale Version Vorrang. Wenn Sie z. b. Code ausführen möchten, der allen Ansichten für den HomeController gemeinsam ist, fügen Sie eine \_viewstart. cshtml-Datei in den Ordner ~/views/Home ein.

### <a id="0.1__Toc274034225"></a>Neue Unterstützung für die vbhtml-Razor-Syntax

Die vorherige ASP.NET MVC-Vorschau enthielt Unterstützung für Sichten, die C#Razor-Syntax auf Grundlage von verwenden. Diese Sichten verwenden die Dateierweiterung. cshtml. Im Rahmen der kontinuierlichen Arbeit zur Unterstützung von Razor führt die ASP.NET MVC 3-Beta Version Unterstützung für die Razor-Syntax in Visual Basic ein, die die Dateierweiterung ". vbhtml" verwendet.

Eine Einführung in die Verwendung Visual Basic Syntax in vbhtml-Seiten finden Sie im Tutorial unter der folgenden URL:

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-visual-basic](../web-pages/overview/getting-started/introducing-razor-syntax-vb.md)

### <a id="0.1__Toc274034226"></a>Genauere Kontrolle über validateinputattribute

ASP.NET MVC hat immer die validateinputattribute-Klasse eingeschlossen, die die Kern ASP.net Anforderungs Validierungs Infrastruktur aufruft, um sicherzustellen, dass die eingehende Anforderung keine potenziell schädliche Eingabe enthält. Die Eingabevalidierung ist standardmäßig aktiviert. Es ist möglich, die Anforderungs Validierung mit dem validateinputattribute-Attribut zu deaktivieren, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](mvc3-release-notes/samples/sample37.cs)]

Viele Webanwendungen verfügen jedoch über einzelne Formularfelder, die HTML zulassen müssen, während die restlichen Felder dies nicht zulassen. Mit der validateinputattribute-Klasse können Sie nun eine Liste von Feldern angeben, die nicht in der Anforderungs Validierung enthalten sein sollen.

Wenn Sie z. b. eine Blog-Engine entwickeln, empfiehlt es sich, Markup in den Feldern Text und Zusammenfassung zuzulassen. Diese Felder können durch zwei Eingabeelemente dargestellt werden, von denen jedes über ein Name-Attribut verfügt, das dem Eigenschaftsnamen ("Body" und "Summary") entspricht. Um die Anforderungs Validierung nur für diese Felder zu deaktivieren, geben Sie die Namen (durch Trennzeichen getrennt) in der Exclude-Eigenschaft der ValidateInput-Klasse an, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](mvc3-release-notes/samples/sample38.cs)]

### <a id="0.1__Toc274034227"></a>Hilfsprogramme konvertieren Unterstriche in Bindestriche für HTML-Attributnamen, die mit anonymen Objekten angegeben werden.

Mithilfe von Hilfsmethoden können Sie Attribut Name-Wert-Paare mithilfe eines anonymen Objekts angeben, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](mvc3-release-notes/samples/sample39.cs)]

Bei diesem Ansatz können Sie keine Bindestriche im Attributnamen verwenden, da ein Bindestrich nicht für einen Eigenschaftsnamen in ASP.NET verwendet werden kann. Bindestriche sind jedoch für benutzerdefinierte HTML5-Attribute wichtig. beispielsweise verwendet HTML5 das "Data-"-Präfix.

Gleichzeitig können Unterstriche nicht für Attributnamen in HTML verwendet werden, sind jedoch innerhalb von Eigenschaftsnamen gültig. Wenn Sie daher Attribute mithilfe eines anonymen Objekts angeben und die Attributnamen einen Unterstrich enthalten, werden die Unterstriche von Hilfsmethoden in Bindestriche konvertiert. Beispielsweise wird in der folgenden hilfssyntax ein Unterstrich verwendet:

[!code-csharp[Main](mvc3-release-notes/samples/sample40.cs)]

Im vorherigen Beispiel wird das folgende Markup beim Ausführen des Hilfsprogramms gerendert:

[!code-html[Main](mvc3-release-notes/samples/sample41.html)]

## <a id="0.1__Toc274034228"></a>Fehlerbehebungen

Die Standard Objekt Vorlage für die Vorlagen Hilfen EditorFor und DisplayFor unterstützt jetzt die in der Eigenschaft DisplayAttribute. Order angegebene Reihenfolge. (In früheren Versionen wurde die Bestell Einstellung nicht verwendet.)

Die Client Validierung unterstützt jetzt die Validierung von überschriebenen Eigenschaften, auf die Validierungs Attribute angewendet werden.

Jsonvalueproviderfactory ist jetzt standardmäßig registriert.

## <a id="0.1__Toc274034229"></a>Wichtige Änderungen

Die Ausführungsreihenfolge für Ausnahme Filter hat sich bei Ausnahme Filtern mit dem gleichen Reihen folgen Wert geändert. In ASP.NET MVC 2 und früheren Versionen wurden Ausnahme Filter auf dem Controller mit derselben Reihenfolge wie die in einer Aktionsmethode ausgeführt, bevor die Ausnahme Filter für die Aktionsmethode ausgeführt wurde. Dies wäre in der Regel der Fall, wenn Ausnahme Filter ohne einen angegebenen Bestellwert angewendet wurden. In ASP.NET MVC 3 wurde diese Reihenfolge umgekehrt, sodass der spezifischere Ausnahmehandler zuerst ausgeführt wird. Wie in früheren Versionen werden die Filter in der angegebenen Reihenfolge ausgeführt, wenn die Order-Eigenschaft explizit angegeben wird.

## <a id="0.1__Toc274034230"></a> Bekannte Probleme

Während der Installation werden im Dialogfeld EULA-Annahme die Lizenzbedingungen in einem Fenster angezeigt, das kleiner als beabsichtigt ist.

Razor-Ansichten verfügen nicht über IntelliSense-Unterstützung und keine Syntax Hervorhebung. Es wird erwartet, dass die Unterstützung für Razor-Syntax in Visual Studio als Teil einer späteren Version enthalten ist.

Wenn Sie eine Razor-Ansicht (cshtml-Datei) bearbeiten, <a id="0.1__Toc224729061"></a> <a id="0.1__Toc238051347"></a> ist das Menü Element gehe zu Controller in Visual Studio nicht verfügbar, und es sind keine Code Ausschnitte vorhanden.

Wenn Sie die @model-Syntax verwenden, um eine stark typisierte cshtml-Ansicht anzugeben, werden keine sprachspezifischen Tastenkombinationen für Typen erkannt. Beispielsweise funktioniert @model int nicht, aber @model Int32 funktioniert. Die Problem Umgehung für diesen Fehler besteht darin, den tatsächlichen Typnamen zu verwenden, wenn Sie den Modelltyp angeben.

Wenn Sie die @model-Syntax verwenden, um eine stark typisierte cshtml-Ansicht anzugeben (oder @ModelType, um eine stark typisierte vbhtml-Ansicht anzugeben), werden Werte zulässt-Typen und Array Deklarationen nicht unterstützt. Beispielsweise @model int? wird nicht unterstützt. Verwenden Sie stattdessen `@model Nullable<Int32>`. Die Syntax @model Zeichenfolge [] wird ebenfalls nicht unterstützt. Verwenden Sie stattdessen `@model IList<string>`.

Wenn Sie ein ASP.NET MVC 2-Projekt auf ASP.NET MVC 3 aktualisieren, stellen Sie sicher, dass Sie dem Abschnitt appSettings der Datei Web. config Folgendes hinzufügen:

[!code-xml[Main](mvc3-release-notes/samples/sample42.xml)]

Es gibt ein bekanntes Problem, das bewirkt, dass die Formular Authentifizierung nicht authentifizierte Benutzer immer an ~/Account/Login umleitet, wobei die in "Web. config" verwendete Formular Authentifizierungs Einstellung ignoriert wird. Die Problem Umgehung besteht darin, die folgende APP-Einstellung hinzuzufügen.

[!code-xml[Main](mvc3-release-notes/samples/sample43.xml)]

## <a id="0.1__Toc274034231"></a>ERS

© 2011 Microsoft Corporation. Alle Rechte vorbehalten. Dieses Dokument wird so bereitgestellt, wie es ist. Informationen und Stellungnahmen in diesem Dokument einschließlich URLs und anderer Verweise auf Websites können ohne Ankündigung geändert werden. Sie tragen das mit der Nutzung verbundene Risiko.

Dieses Dokument stellt keinerlei Rechtsansprüche auf geistiges Eigentum in Microsoft-Produkten jeglicher Art bereit. Sie dürfen dieses Dokument zu internen Referenzzwecken kopieren und verwenden.
