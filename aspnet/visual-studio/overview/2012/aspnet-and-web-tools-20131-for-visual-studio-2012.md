---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: Anmerkungen zu dieser Version von ASP.net and Web Tools 2013,1 für Visual Studio 2012 | Microsoft-Dokumentation
author: microsoft
description: In diesem Dokument wird die Veröffentlichung von ASP.net and Web Tools 2013,1 für Visual Studio 2012 beschrieben.
ms.author: riande
ms.date: 11/13/2013
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 260af1018064d60d80cbb1002001f28c4daffffd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467103"
---
# <a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Anmerkungen zu dieser Version – ASP.NET and Web Tools 2013.1 für Visual Studio 2012

von [Microsoft](https://github.com/microsoft)

> In diesem Dokument wird die Veröffentlichung von ASP.net and Web Tools 2013,1 für Visual Studio 2012 beschrieben.

## <a name="contents"></a>Inhalt

- [Installationshinweise](#install)
- [Software Anforderungen](#requirements)
- Neue Features in ASP.net and Web Tools 2013,1 für Visual Studio 2012

    - [Bootstrap](#bootstrap)
    - [Vorlagen](#templates)

        - [ASP.NET MVC 5-Vorlage](#mvc5template)
        - [Vorlage für ASP.net-Web-API 2](#apitemplate)
        - [Element Vorlagen](#itemtemplate)
    - [Entity Framework 6](#ef6)
    - [ASP.net Gerüstbau](#scaffold)
    - [Razor-Editor](#razor)
    - [NuGet 2.7](#nuget)
- Bekannte Probleme und wichtige Änderungen

    - [ASP.net Gerüstbau](#issuescaffolding)

        - [MVC-und Web-API-Gerüstbau: Fehler HTTP 404, nicht gefunden](#404issue)
        - [Visual Studio Express 2012 für Web funktioniert nach dem Hinzufügen eines Gerüsts nicht mehr](#expressissue)
    - [ASP.net Razor 3](#issuerazor)

        - [Das Anzeigen der cshtml-Datei mit "Durchsuchen" oder "F5" verursacht einen Server Fehler](#browseissue)
        - [URL-Rewrite und Tilde (~)](#rewriteissue)
    - [Vorlagen](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a>Installationshinweise

[Installieren](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) von ASP.net and Web Tools 2013,1 für Visual Studio 2012.

<a id="requirements"></a>
## <a name="software-requirements"></a>Softwareanforderungen

Sie müssen entweder über Visual Studio 2012 oder Visual Studio Express 2012 für das Web verfügen.

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Neue Features in ASP.net and Web Tools 2013,1 für Visual Studio 2012

<a id="bootstrap"></a>
### <a name="bootstrap"></a>Bootstrap

Wenn Sie MVC 5-Controller und-Ansichten Gerüstbau, wird das Markup für die Ansichten [Bootstrap](http://getbootstrap.com/)verwendet.

<a id="templates"></a>
### <a name="templates"></a>Vorlagen

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a>ASP.NET MVC 5-Vorlage

Wir haben eine neue MVC 5-Vorlage hinzugefügt. Er verweist auf die neuesten MVC 5 nuget-Pakete, und Sie können das Gerüst zum Hinzufügen von Controllern und Ansichten verwenden.

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a>Vorlage für ASP.net-Web-API 2

Wir haben eine neue Web-API 2-Vorlage hinzugefügt. Er verweist auf die neuesten nuget-Pakete der Web-API 2, und Sie können das Gerüst zum Hinzufügen von Controllern und Ansichten verwenden.

<a id="itemtemplate"></a>
#### <a name="item-templates"></a>Element Vorlagen

Wir haben neue Element Vorlagen für MVC 5-Ansichten, Webseiten (Razor 3) und Web-API 2-Controller hinzugefügt. Beim Hinzufügen neuer Elemente werden die entsprechenden nuget-Pakete für das Projekt installiert.

<a id="ef6"></a>
### <a name="entity-framework-6"></a>Entity Framework 6

Wenn Sie einen MVC-oder Web-API-Controller mithilfe Entity Framework Gerüstbau, verwenden wir Framework 6. Weitere Informationen zu Entity Framework finden Sie im [Versionsverlauf Entity Framework](https://msdn.com/data/jj574253).

Sie können auch die Entity Framework 6-Tools für Visual Studio 2012 herunterladen und installieren. Weitere Informationen finden [Sie unter Get Entity Framework](https://msdn.com/data/ee712906#tooling).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>ASP.net Gerüstbau

ASP.net Gerüstbau ist ein Code Generierungs Framework für ASP.NET-Webanwendungen. Es vereinfacht das Hinzufügen von Code Bausteinen zu Ihrem Projekt, das mit einem Datenmodell interagiert.

In früheren Versionen von Visual Studio waren Gerüstbau auf ASP.NET MVC-Projekte beschränkt. Mit diesem Update können Sie jetzt Gerüstbau für alle ASP.net-Projekte verwenden, einschließlich Web Forms. Dieses Update unterstützt das Erstellen von Seiten für ein Web Forms Projekt nicht, aber Sie können weiterhin Gerüstbau mit Web Forms verwenden, indem Sie dem Projekt MVC-Abhängigkeiten hinzufügen. Die Unterstützung für das Erstellen von Seiten für Web Forms wird in einem späteren Update hinzugefügt.

Wenn Sie Gerüstbau verwenden, stellen wir sicher, dass alle erforderlichen Abhängigkeiten im Projekt installiert sind. Wenn Sie z. b. mit einem ASP.net-Web Forms Projekt beginnen und dann mithilfe von Gerüstbau einen Web-API-Controller hinzufügen, werden die erforderlichen nuget-Pakete und-Verweise automatisch dem Projekt hinzugefügt.

Fügen Sie ein **Neues Gerüstbau Element** hinzu, und wählen Sie **MVC 5-Abhängigkeiten** im Dialogfenster aus, um einem Web Forms Projekt MVC-Gerüstbau hinzuzufügen. Es gibt zwei Optionen für das Gerüstbau von MVC: Minimal und vollständig. Wenn Sie minimal auswählen, werden nur die nuget-Pakete und-Verweise für ASP.NET MVC dem Projekt hinzugefügt. Wenn Sie die Option vollständig auswählen, werden die minimalen Abhängigkeiten sowie die erforderlichen Inhalts Dateien für ein MVC-Projekt hinzugefügt.

Die Unterstützung für Gerüstbau-asynchrone Controller verwendet die neuen Async-Features aus Entity Framework 6.

Weitere Informationen und Tutorials finden Sie unter [Übersicht über ASP.net Gerüstbau](../2013/aspnet-scaffolding-overview.md). In diesen Tutorials wird Gerüstbau mit Visual Studio 2013 angezeigt, Sie sind jedoch auch auf ASP.net and Web Tools 2013,1 für Visual Studio 2012 anwendbar.

<a id="razor"></a>
### <a name="razor-editor"></a>Razor-Editor

Mit diesem Update unterstützt Visual Studio 2012 jetzt Razor 3-Tools/-Bearbeitung.

<a id="nuget"></a>
### <a name="nuget-27"></a>NuGet 2.7

Nuget 2,7 umfasst einen umfangreichen Satz neuer Features, die in den Anmerkungen zur [nuget-Version 2,7](http://docs.nuget.org/docs/release-notes/nuget-2.7)ausführlich beschrieben werden.

Mit dieser Version von nuget entfällt die Notwendigkeit, dass Benutzer explizit zulassen, dass Pakete fehlende Pakete wiederherstellen. Bei der Installation von nuget 2,7 Stimmen die Benutzer implizit zu, dass fehlende Pakete automatisch wieder hergestellt werden. Benutzer können die Paket Wiederherstellung über die nuget-Einstellungen in Visual Studio explizit ablehnen. Diese Änderung vereinfacht die Funktionsweise der Paket Wiederherstellung.

## <a name="known-issues-and-breaking-changes"></a>Bekannte Probleme und wichtige Änderungen

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a>ASP.net Gerüstbau

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC-und Web-API-Gerüstbau: Fehler HTTP 404, nicht gefunden

Wenn ein Fehler auftritt, wenn Sie einem Projekt ein Gerüst Element hinzufügen, ist es möglich, dass das Projekt in einem inkonsistenten Zustand verbleibt. Für einige der Änderungen, die an Gerüstbau vorgenommen werden, wird ein Rollback ausgeführt, aber für andere Änderungen, wie z. b Wenn für die Routing Konfigurationsänderungen ein Rollback ausgeführt wird, erhalten Benutzer einen HTTP 404-Fehler, wenn Sie zu Gerüst Elementen navigieren.

Um diesen Fehler für MVC zu beheben, fügen Sie ein neues Gerüst Element hinzu, und wählen Sie MVC 5-Abhängigkeiten (Minimal oder vollständig) aus. Durch diesen Vorgang werden alle erforderlichen Änderungen in Ihrem Projekt hinzugefügt.

So beheben Sie diesen Fehler für die Web-API:

1. Fügen Sie dem Projekt die folgende webapiconfig-Klasse hinzu.

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. Konfigurieren Sie webapiconfig. Register in der Anwendung\_Start-Methode in Global. asax wie folgt:

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a>Visual Studio Express 2012 für Web funktioniert nach dem Hinzufügen eines Gerüsts nicht mehr

Wenn Visual Studio Express 2012 für Web nicht mehr funktioniert, nachdem Sie ein Gerüst Element mit Entity Framework hinzugefügt haben (z. b. Web-API 2-Controller mit Aktionen, mithilfe Entity Framework), ist es möglich, dass Visual Studio Express das Native Image einer Assembly nicht laden konnte. abhängig von "System. Web. Extensions".

Um dieses Problem zu beheben, konfigurieren Sie Visual Studio Express, um mit dem MSIL-Image von System. Web. Extensions zu arbeiten:

1. Öffnen Sie die Eingabeaufforderung im Administrator Modus.
2. Wechseln Sie zu%ProgramFiles%\Microsoft Visual Studio 11.0 \ Common7\IDE oder% Program Files (x86)% \ Microsoft Visual Studio 11.0 \ Common7\IDE (für 64-Bit-Fenster).
3. Öffnen Sie vwdebug. exe. config in einem Text-Editor.
4. Fügen Sie die folgende Zeile unter dem &lt;Configuration&gt;/&lt;Runtime-&gt; Element hinzu:  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. Starten Sie Visual Studio Express 2012 für das Web neu.

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a>ASP.net Razor 3

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-with-browse-with-or-f5-causes-a-server-error"></a>Das Anzeigen der cshtml-Datei mit "Durchsuchen" oder "F5" verursacht einen Server Fehler

Wenn Sie ein MVC 5-Projekt in Visual Studio 2012 erstellen (oder in Visual Studio 2012 ein MVC 5-Projekt öffnen, das in Visual Studio 2013 erstellt wurde) und versuchen, eine cshtml-Datei mithilfe von "Durchsuchen" oder "F5" anzuzeigen, erhalten Sie einen Fehler, der besagt, dass der **Server Fehler in der Anwendung "/" angezeigt**wird. Der Server versucht, zu `http://localhost:XXXX/Views/../XXXX.cshtml` zu navigieren.

Um dieses Problem zu beheben, ändern Sie die Einstellung für die **Start Aktion** in Ihrem Projekt auf eine **bestimmte Seite**. Es ist nicht erforderlich, einen Wert für die Seite anzugeben.

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

Nachdem Sie diese Änderung vorgenommen haben, wechselt die Auswahl von F5 zum Stamm der Anwendung (`http://localhost:XXXX`). Dieses Verhalten ist nicht identisch mit dem Verhalten für MVC 5-Projekte in Visual Studio 2013, in dem die **Aktuelle Seiten** Einstellung die geöffnete Seite öffnet.

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a>URL-Rewrite und Tilde (~)

Nach dem Upgrade auf ASP.net Razor 3 oder ASP.NET MVC 5 funktioniert die Tilde (~)-Notation möglicherweise nicht mehr ordnungsgemäß, wenn Sie URL-Neuschreibungen verwenden. Die URL-Umschreibung wirkt sich auf die Tilde (~)-Notation in HTML-Elementen aus, z. b. &lt;A/&gt;, &lt;Script/&gt;, &lt;Link/&gt;. Folglich wird die Tilde nicht mehr dem Stammverzeichnis zugeordnet.

Wenn Sie z. b. Anforderungen für **ASP.NET/Content** zu **ASP.net**umschreiben, wird das href-Attribut in &lt;A href = "~/Content/"/&gt; in **/Content/Content/** anstatt **/** aufgelöst. Um diese Änderung zu unterdrücken, können Sie den **IIS-\_wasurlumgeschriebene** -Kontext auf jeder Webseite oder in **Application\_BeginRequest** in Global. asax auf false festlegen.

<a id="templateissue"></a>
### <a name="templates"></a>Vorlagen

Wenn Sie ASP.NET MVC-Projekte mit Visual Studio 2012 auf Windows 8.1 oder Windows Server 2012 R2 erstellen, zeigt Visual Studio eine Fehlermeldung an, die besagt, dass die Konfiguration von Web [URL] für ASP.NET 4,5 fehlgeschlagen ist.

![Konfigurationsfehler](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

Dieser Fehler wird angezeigt, da Visual Studio 2012 die Funktion ASP.NET 4,5 nicht aktiviert, wenn Sie auf diesen Versionen von Windows installiert ist. Um ASP.NET 4,5 zu aktivieren, führen Sie die unter Aktivieren [oder Deaktivieren von Windows-Features](https://windows.microsoft.com/windows-8/turn-windows-features-on-off)beschriebenen Schritte aus.

![Aktivieren oder Deaktivieren von Windows-Features](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

Wahlweise können Sie ASP.NET 4,5 auch über die Befehlszeile aktivieren.

1. Öffnen Sie die Eingabeaufforderung im Administrator Modus.
2. Führen Sie den folgenden Befehl aus, um ASP.NET 4,5 zu aktivieren.  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
