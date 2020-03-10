---
uid: web-pages/overview/getting-started/aspnet-web-pages-razor-faq
title: FAQ zu ASP.net Web Pages (Razor) | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Artikel werden einige häufig gestellte Fragen zu ASP.net Web Pages (Razor) und webmatrix aufgeführt. Im Tutorial verwendete Software Versionen ASP.net Web Pages (R...
ms.author: riande
ms.date: 02/07/2014
ms.assetid: b137bd04-25e1-47cb-9d96-ef2e179ecf1f
msc.legacyurl: /web-pages/overview/getting-started/aspnet-web-pages-razor-faq
msc.type: authoredcontent
ms.openlocfilehash: 6fa1b668e915af856a693e79eafb7180ed504dc2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78520653"
---
# <a name="aspnet-web-pages-razor-faq"></a>ASP.NET-Webseiten (Razor) – FAQs

von [Tom fitzmacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > Webmatrix wird nicht mehr als integrierte Entwicklungsumgebung für ASP.net Web Pages empfohlen. Verwenden Sie [Visual Studio](xref:aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio) oder [Visual Studio Code](https://code.visualstudio.com/).
>
> In diesem Artikel werden einige häufig gestellte Fragen zu ASP.net Web Pages (Razor) und webmatrix aufgeführt.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
> 
> 
> - ASP.net Web Pages (Razor) 3
> - Visual Studio 2013
> - Webmatrix 3
>   
> 
> Dieses Tutorial funktioniert auch mit ASP.net Web Pages 2, webmatrix 2 und Visual Studio 2012.

- [Worin besteht der Unterschied zwischen ASP.net Web Pages, ASP.net Web Forms und ASP.NET MVC?](#Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC)
- [Benötige ich webmatrix, um mit Webseiten arbeiten zu können?](#Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages)
- [Kann ich ASP.net Web Forms-Steuerelemente auf einer Webseiten Seite verwenden?](#Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page)
- [Kann ich einen ASP.net Web Pages Standort ohne webmatrix bereitstellen?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix)
- [Muss ich das WebSecurity-Hilfsprogramm verwenden, um Anmeldungen zu unterstützen?](#Do_I_have_to_use_the_WebSecurity_helper_to_support_logins)
- [Unterstützt ASP.net Web Pages HTML5?](#Does_ASP.NET_Web_Pages_support_HTML5)
- [Kann ich JavaScript und jQuery mit Webseiten verwenden?](#Can_I_use_JavaScript_and_jQuery_with_Web_Pages)
- [Weitere Ressourcen](#AdditionalResources)

Fragen zu Fehlern und anderen Problemen finden Sie im Handbuch zur Problembehandlung für [ASP.net Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001).

<a id="Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC"></a>
## <a name="whats-the-difference-between-aspnet-web-pages-aspnet-web-forms-and-aspnet-mvc"></a>Worin besteht der Unterschied zwischen ASP.net Web Pages, ASP.net Web Forms und ASP.NET MVC?

Alle drei sind ASP.NET-Technologien zum Erstellen dynamischer Webanwendungen:

- ASP.net Web Pages konzentriert sich auf das Hinzufügen von dynamischem (Server seitigem) Code und Datenbankzugriff auf HTML-Seiten und bietet einfache und einfache Syntax.
- ASP.net Web Forms basiert auf einem Seiten Objektmodell und herkömmlichen Steuerelementen vom Typ "Fenster" (Schaltflächen, Listen usw.). Web Forms verwendet ein Ereignis basiertes Modell, das den Benutzern vertraut ist, die mit der Client basierten Entwicklung (Windows Forms) gearbeitet haben.
- ASP.NET MVC implementiert das Model-View-Controller-Muster für ASP.net. Der Schwerpunkt liegt auf der Trennung von Anliegen (Verarbeitung, Daten und UI-Schichten).

Alle drei Frameworks werden vollständig unterstützt und weiterhin vom ASP.net-Team entwickelt. Im Allgemeinen hängt die Wahl des zu verwendenden Frameworks von Ihrem Hintergrund und ihrer Erfahrung mit ASP.net ab.

ASP.net Web Pages insbesondere so konzipiert, dass Personen, die bereits über HTML verfügen, die Server Verarbeitung zu Ihren Seiten hinzufügen können. Es ist eine gute Wahl für Schüler/Studenten, Hobby Entwickler und allgemeine Personen, die mit der Programmierung noch nicht vertraut sind. Es ist auch eine gute Wahl für Entwickler, die mit Non-ASP.net-Webtechnologien arbeiten.

<a id="Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages"></a>
## <a name="do-i-need-webmatrix-in-order-to-work-with-web-pages"></a>Benötige ich webmatrix, um mit Webseiten arbeiten zu können?

Nein. Webmatrix wird nicht mehr als integrierte Entwicklungsumgebung für ASP.net Web Pages empfohlen. Verwenden Sie [Visual Studio](program-asp-net-web-pages-in-visual-studio.md) oder [Visual Studio Code](https://code.visualstudio.com/).

Wenn Sie weder Visual Studio noch Visual Studio Code verwenden möchten, können Sie die Komponenten Produkte einzeln mithilfe [Microsoft-Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx)installieren. Sie benötigen die folgenden Produkte:

- Microsoft .NET Framework 4.5
- ASP.NET MVC 5 (mit dem auch das ASP.net Web Pages Framework installiert wird)
- IIS Express (Webserver)
- Microsoft SQL Server Compact 4,0 (Datenbank)

Sie können einen Text-Editor verwenden, um die *cshtml* -Seiten (oder *vbhtml*-Seiten) zu bearbeiten.

Das Verwalten von SQL Server Compact Datenbanken (*sdf* -Dateien) ohne ein Tool ist etwas schwieriger. Visual Studio-Container Tools zum Verwalten von *. sdf* -Datenbanken. Sie können auch SQL-Befehle im Code ausführen, um viele SQL Server Verwaltungsaufgaben auszuführen.

Zum Testen von *. cshtml* -Seiten ohne Verwendung einer integrierten Entwicklungsumgebung (Integrated Development Environment, IDE) können Sie diese auf einem Live Server bereitstellen. (Weitere Informationen finden [Sie unter kann ich eine ASP.net Web Pages Site ohne Verwendung von webmatrix](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix)bereitstellen?)

### <a name="running-iis-express-without-using-an-ide"></a>Ausführen von IIS Express ohne Verwendung einer IDE

Wenn Sie IIS Express auf Ihrem Computer als Webserver installieren, können Sie diese verwenden, um die Seiten zu testen. Sie können IIS Express über die Befehlszeile ausführen und eine bestimmte Portnummer zuordnen. Sie geben diesen Port dann an, wenn Sie *cshtml* -Dateien in Ihrem Browser anfordern.

Öffnen Sie in Windows eine Eingabeaufforderung mit Administratorrechten, und wechseln Sie zu *c:\Programme\IIS Express.* (Bei 64-Bit-Systemen verwenden Sie den Ordner *c:\Programme (x86) \IIS Express.)* Geben Sie dann den folgenden Befehl ein, und verwenden Sie dabei den eigentlichen Pfad zu Ihrer Website:

`iisexpress.exe /port:35896 /path:C:\BasicWebSite`

Sie können eine beliebige Portnummer verwenden, die nicht bereits von einem anderen Prozess reserviert wurde. (Port Nummern oberhalb von 1024 sind in der Regel kostenlos.) Verwenden Sie für den `path` Wert den Pfad des Website Ordners, in dem sich die *cshtml* -Dateien befinden.

Nachdem Sie diesen Befehl ausgeführt haben, um IIS Express für Ihre Seiten einzurichten, können Sie einen Browser öffnen und zu einer *cshtml* -Datei navigieren. Verwenden Sie eine URL wie die folgende:

`http://localhost:35896/default.cshtml`

Um Hilfe zu IIS Express Befehlszeilenoptionen zu erhalten, geben Sie `iisexpress.exe /?` in der Befehlszeile ein.

<a id="Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page"></a>
## <a name="can-i-use-aspnet-web-forms-controls-on-a-web-pages-page"></a>Kann ich ASP.net Web Forms-Steuerelemente auf einer Webseiten Seite verwenden?

Nein. Web Forms Steuerelemente wie das [CheckBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.checkbox) -Steuerelement, die [Validierungs Steuerelemente](https://msdn.microsoft.com/library/bwd43d0x)und das [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview) -Steuerelement funktionieren nur in Web Forms Seiten (*aspx* -Dateien). Diese Steuerelemente erfordern das Web Forms Seite-Framework.

<a id="Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix"></a>
## <a name="can-i-deploy-an-aspnet-web-pages-site-without-using-webmatrix"></a>Kann ich einen ASP.net Web Pages Standort ohne webmatrix bereitstellen?

Ja. Sie können Website Dateien manuell auf einen Server kopieren (in der Regel mithilfe von FTP). Wenn Sie eine manuelle Kopie ausführen, müssen Sie auch die Dateien kopieren, die SQL Server Compact (die-Datenbank) unterstützen. Weitere Informationen finden Sie im Blogbeitrag Bereitstellen von [Web Pages-Anwendungen ohne ein Tool](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2317).

<a id="Do_I_have_to_use_the_WebSecurity_helper_to_support_logins"></a>
## <a name="do-i-have-to-use-the-websecurity-helper-to-support-logins"></a>Muss ich das WebSecurity-Hilfsprogramm verwenden, um Anmeldungen zu unterstützen?

Nein. Der `SimpleMembership`-Anbieter, der Teil von ASP.net Web Pages ist, ist eine Option. Die Sicherheitsanbieter, die Teil von ASP.net sind (die Sie möglicherweise für die Arbeit mit in Web Forms verwenden), sind ebenfalls verfügbar. Beispielsweise können Sie die Formular Authentifizierung in ASP.net Web Pages genauso wie in Web Forms verwenden. Ein Beispiel für die Verwendung der Formular Authentifizierung finden Sie im Microsoft-Support Artikel Gewusst [wie: Implementieren der Formular basierten Authentifizierung in Ihrer ASP.NET-Anwendung mithilfe C#von .net](https://support.microsoft.com/kb/301240). Ein einfaches Beispiel finden Sie unter [ASP.NET Version of Login &amp; Password](http://www.codeguru.com/csharp/.net/net_asp/scripting/article.php/c19295/ASPNET-version-of-Login--Password.htm).

Informationen zur Verwendung der Windows-Authentifizierung finden [Sie im Blogbeitrag using Windows Authentication in ASP.net Web Pages](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2298).

<a id="Does_ASP.NET_Web_Pages_support_HTML5"></a>
## <a name="does-aspnet-web-pages-support-html5"></a>Unterstützt ASP.net Web Pages HTML5?

Ja. Die Seiten, die Sie mit ASP.net Web Pages (*cshtml* -oder *vbhtml* -Seiten) erstellen, sind im wesentlichen HTML-Seiten, die auch Code enthalten, der auf dem Server ausgeführt wird, bevor die Seite gerendert wird. Solange der Browser des Benutzers HTML5 unterstützt, können Sie HTML5-Elemente auf einer *cshtml* -oder *vbhtml* -Seite verwenden.

<a id="Can_I_use_JavaScript_and_jQuery_with_Web_Pages"></a>
## <a name="can-i-use-javascript-and-jquery-with-web-pages"></a>Kann ich JavaScript und jQuery mit Webseiten verwenden?

Absolut. Die Seiten, die Sie mit ASP.net Web Pages (*cshtml* -oder *vbhtml* -Seiten) erstellen, sind nur HTML-Seiten mit Servercode. Daher können Sie alles, was Sie auf einer normalen HTML-Seite mithilfe von JavaScript oder jQuery ausführen können, auch auf einer *cshtml* -oder *vbhtml* -Seite tun.

Die Vorlage " **Starter Site** " in webmatrix enthält eine Reihe von jQuery-Bibliotheken. Wenn Sie mithilfe dieser Vorlage eine Website erstellen, enthält der Ordner *Scripts* eine jQuery-Kernbibliothek (*jQuery-1.6.2. js)* und Bibliotheken für die jQuery-Validierung (*jQuery. Validate. js*usw.).

Im folgenden finden Sie einige Blogbeiträge, die die Verwendung von jQuery mit ASP.net Web Pages veranschaulichen:

- [Hinzufügen von jQuery-Güte zu ASP.net Web Pages mithilfe von webmatrix](http://rachelappel.com/jquery/adding-jquery-goodness-to-asp-net-web-pages-using-webmatrix/) von Rachel Appel
- [5 min: webmatrix + jQuery UI + JSON + jQuery-Vorlagen](http://joeriks.com/2011/01/30/5-min-webmatrix-jquery-ui-json-jquery-templates/) von Jonas Eriksson
- [Webmatrix-und jQuery-Formulare](http://mikesdotnetting.com/Article/155/WebMatrix-And-jQuery-Forms) von Mike Brind

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Leitfaden zur Behandlung von Problemen mit ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001)

[Webmatrix und ASP.net Web Pages Forum](https://forums.asp.net/1224.aspx/1?WebMatrix) auf der ASP.NET-Website
